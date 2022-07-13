## Problem statement
While implementing `LazyColumn` with `TextField`. When `TextField` gains focus for text input from the user it hides the `LazyColumn` items with `Keyboard`. Following is example of what is happening:

![pic](https://i.ibb.co/Gt3XxYT/uploader-new.gif)

So `LazyColumn` doesn't scroll to very last element and its data gets hidden by the `Keyboard`. At least it should show the very last item that was visible on the screen before the keyboard was shown on to the screen

## Problem detection
This was a UI problem and hence it was detected while testing on the screen. 

## Problem solution
I have created an [issue](https://issuetracker.google.com/issues/237566530) for this bug, feel free to add (+1) on it. 

For now we will go with a workaround unless that issue is fixed. So the workaround is as following:
1. [Check for keyboard visibility/invisibility and listen to it](#step-1)
2. [On keyboards visibility, calculate keyboard height and scroll `LazyColumn` to that height](#step-2)
3. [Just before keyboards invisibility, get last visible elements index](#step-3)
4. [Store the length all the elements](#step-4)
5. [Scroll `LazyColumn` by summing up the elements size below the last visible element identified in **step 3**](#step-5)

###### Step 1
To check for keyboard visibility/invisibility we need to listen for keyboard state, for that we first need to know if keyboard is present on the screen or not.\
In order to check if keyboard is present on the screen or not we have the following code:
```
fun View.isKeyboardOpen(): Boolean {
    val rect = Rect()
    getWindowVisibleDisplayFrame(rect)
    val screenHeight = rootView.height
    val absoluteKeyboardHeight = if (rect.bottom > 0) screenHeight - rect.bottom else 0
    return absoluteKeyboardHeight > screenHeight * 0.15
}

```
This function `isKeyboardOpen()` will return `true` if keyboard is visible and `false` if keyboard is invisible.

Now that we know that the keyboards state is visible/invisible we will add a listener for these states. That can be done by the following code:
```
@Composable
fun rememberIsKeyboardOpen(): State<Boolean> {
    val view = LocalView.current
    return produceState(initialValue = view.isKeyboardOpen()) {
        val viewTreeObserver = view.viewTreeObserver
        val listener = ViewTreeObserver.OnGlobalLayoutListener { value = view.isKeyboardOpen()}
        viewTreeObserver.addOnGlobalLayoutListener(listener)
        awaitDispose { viewTreeObserver.removeOnGlobalLayoutListener(listener)  }
    }
}
```
We have created the above composable function `rememberIsKeyboardOpen()` for monitoring keyboard state. It will return true if keyboard is visible and false otherwise.


###### Step 2
So the code in the above step will give us the visibility of the keyboard. Now we need calculate the height of the keyboard and scroll up `LazyColumn` with the same amount. We can do that by modifiying the `isKeyboardOpen()` function as following:
```
fun View.isKeyboardOpen(
    scope: CoroutineScope,
    listScrollStateHolder: LazyListState
): Boolean {
    ...
    
    if (absoluteKeyboardHeight > screenHeight * 0.15){
        scope.launch { listScrollStateHolder.scrollBy(absoluteKeyboardHeight.toFloat()) }
    }
    
    return absoluteKeyboardHeight > screenHeight * 0.15
}

```
We also need to modify `rememberIsKeyboardOpen()` function to include the above modification. So the modified code look like this:
```
@Composable
fun rememberIsKeyboardOpen(
    scope: CoroutineScope,
    listScrollStateHolder: LazyListState
): State<Boolean> {
    ...
    return produceState(initialValue = view.isKeyboardOpen(scope, listScrollStateHolder)) {
        ...
        val listener = ViewTreeObserver.OnGlobalLayoutListener { value = view.isKeyboardOpen(scope, listScrollStateHolder)}
        ...
    }
}
```


###### Step 3 
Now that we are able to scroll up when the keyboard becomes visisble we must also be able to to scroll down if keyboard becomes invisible. We accomplish this behaviour with the following code:
```
fun View.isKeyboardOpen(
    ...
    lastVisibleItemIndex: MutableState<Int>
): Boolean {
    ...
    val visibleItemList = listScrollStateHolder.layoutInfo.visibleItemsInfo
    if (absoluteKeyboardHeight > screenHeight * 0.15){
        scope.launch { listScrollStateHolder.scrollBy(absoluteKeyboardHeight.toFloat()) }
        lastVisibleItemIndex.value = visibleItemList[visibleItemList.size-1].index
    }
    
    return absoluteKeyboardHeight > screenHeight * 0.15
}

```
We also need to modify `rememberIsKeyboardOpen()` function to include the above modification. So the modified code look like this:
```
@Composable
fun rememberIsKeyboardOpen(
    ...
    lastVisibleItemIndex: MutableState<Int>
): State<Boolean> {
    ...
    return produceState(initialValue = view.isKeyboardOpen(scope, listScrollStateHolder, lastVisibleItemIndex)) {
        ...
        val listener = ViewTreeObserver.OnGlobalLayoutListener { value = view.isKeyboardOpen(scope, listScrollStateHolder, lastVisibleItemIndex)}
        ...
    }
}
```


###### Step 4
Now that we know the last visible item index just before the keyboards invisibility. We know that after keyboards invisibility the last element that is visible on the screen should the one that we identified and stored in `lastVisibleItemIndex.value` variable. In short, we must scroll down until all the elements below `lastVisibleItemIndex.value` become invisible.

For that to happen, first we need to store the `size` of each element of `LazyColumn` in some `List` and reference it later. Following is the storage code:
```
fun View.isKeyboardOpen(
    ...
    listItemAbsoluteSizeList: MutableList<Int>
): Boolean {
    ...
    val visibleItemList = listScrollStateHolder.layoutInfo.visibleItemsInfo
    if (absoluteKeyboardHeight > screenHeight * 0.15){
        ...
        listItemAbsoluteSizeList[visibleItemList[visibleItemList.size-1].index] = visibleItemList[visibleItemList.size-1].size
    }
    
    return absoluteKeyboardHeight > screenHeight * 0.15
}

```
We also need to modify `rememberIsKeyboardOpen()` function to include the above modification. So the modified code look like this:
```
@Composable
fun rememberIsKeyboardOpen(
    ...
    listItemAbsoluteSizeList: MutableList<Int>
): State<Boolean> {
    ...
    return produceState(initialValue = view.isKeyboardOpen(scope, listScrollStateHolder, lastVisibleItemIndex, listItemAbsoluteSizeList)) {
        ...
        val listener = ViewTreeObserver.OnGlobalLayoutListener { value = view.isKeyboardOpen(scope, listScrollStateHolder, lastVisibleItemIndex, listItemAbsoluteSizeList)}
        ...
    }
}
```


###### Step 5
Now that we know the size of each element in the `LazyColumn`, we will add all element sizes below the last visible element and scroll `LazyColumn` down by the same amount. Following is the code for the desired functionality:
```
fun View.isKeyboardOpen(
    scope: CoroutineScope,
    listScrollStateHolder: LazyListState,
    isKeyboardVisibleBefore: MutableState<Boolean>,
    lastVisibleItemIndex: MutableState<Int>,
    listItemAbsoluteSizeList: MutableList<Int>
): Boolean {
    ...
    val visibleItemList = listScrollStateHolder.layoutInfo.visibleItemsInfo
    if(visibleItemList.isNotEmpty()){
        if(absoluteKeyboardHeight > screenHeight * 0.15){
            if (!isKeyboardVisibleBefore.value){
                ...
                isKeyboardVisibleBefore.value = true
            }
            lastVisibleItemIndex.value = visibleItemList[visibleItemList.size-1].index
            listItemAbsoluteSizeList[visibleItemList[visibleItemList.size-1].index] = visibleItemList[visibleItemList.size-1].size
        } else {
            if (isKeyboardVisibleBefore.value) {
                var scrollOffset = 0
                var lastNonAdditionedElementIndex = listScrollStateHolder.layoutInfo.totalItemsCount - 1
                while (lastNonAdditionedElementIndex > lastVisibleItemIndex.value){
                    scrollOffset += listItemAbsoluteSizeList[lastNonAdditionedElementIndex]
                    lastNonAdditionedElementIndex -= 1
                }
                scope.launch {listScrollStateHolder.scrollBy(-scrollOffset.toFloat())}
                isKeyboardVisibleBefore.value = false
            }
        }
    }

    return absoluteKeyboardHeight > screenHeight * 0.15
}
```
With the above code we can have a workaround for the problem state above. It might function as a workaround only.


Complete code is available [here](https://github.com/litoco/SmallProjects/blob/1c16e40beb81df7ffa95682d2b9cee794d521d99/SolutionTestingApp/app/src/main/java/com/example/solutiontestingapp/MainActivity.kt).

With the above code we achieve our desired output as shown below:

![Working proof gif](https://i.ibb.co/JFKn541/uploader.gif)
