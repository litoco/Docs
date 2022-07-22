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
2. [On keyboards visibility, calculate keyboard height and scroll LazyColumn to the keyboard height](#step-2-step-3-and-step-4)
3. [Calcuate last visible items distance from keyboards top border](#step-2-step-3-and-step-4)
4. [After keyboard close, scroll `LazyColumn` such that the last visible item and its distance as calculated in **step 3** are same now as well](##step-2-step-3-and-step-4)

###### Step 1
Following is the code for checking keyboard visibility:
```
fun View.getKeyboardHeight(): Boolean {
    val rect = Rect()
    getWindowVisibleDisplayFrame(rect)
    val screenHeight = rootView.height
    val absoluteKeyboardHeight = screenHeight - rect.bottom
    return if (rect.bottom > 0) absoluteKeyboardHeight > screenHeight * .15 else false
}
```

and it is getting observed for any modifications by the following code:
```
@Composable
fun rememberIsKeyboardOpen(): State<Boolean> {
    val view = LocalView.current

    return produceState(initialValue = view.getKeyboardHeight()) {
        val viewTreeObserver = view.viewTreeObserver
        val listener = ViewTreeObserver.OnGlobalLayoutListener { value = view.getKeyboardHeight()}
        viewTreeObserver.addOnGlobalLayoutListener(listener)

        awaitDispose { viewTreeObserver.removeOnGlobalLayoutListener(listener)  }
    }
}
```


###### Step 2, Step 3 and Step 4
Following is the function for all these steps, it has steps 2,3 and 4 marked in it:
```
@Composable
fun SyncLazyColumnScroll(
    scope: CoroutineScope,
    listScrollStateHolder: LazyListState
) {
    val isKeyboardOpen by rememberIsKeyboardOpen()
    var prevScreenSize by remember { mutableStateOf(0)}
    var lastVisibleItemIndex by remember { mutableStateOf(0)}
    var lastVisibleItemOffset by remember { mutableStateOf(0)}
    var hasScrolledOnce by remember { mutableStateOf(false)}

    LaunchedEffect(key1 = isKeyboardOpen, key2 = listScrollStateHolder.isScrollInProgress){
        if (listScrollStateHolder.layoutInfo.viewportEndOffset > prevScreenSize)
            prevScreenSize = listScrollStateHolder.layoutInfo.viewportEndOffset
        if (listScrollStateHolder.layoutInfo.visibleItemsInfo.isNotEmpty()) {
            if (isKeyboardOpen) {   //**step 2** 
                if (!listScrollStateHolder.isScrollInProgress && !hasScrolledOnce) {
                    val size = listScrollStateHolder.layoutInfo.viewportEndOffset
                    scope.launch { listScrollStateHolder.animateScrollBy((prevScreenSize - size).toFloat()) }
                    hasScrolledOnce = true
                }
                // **step 3**
                lastVisibleItemIndex = listScrollStateHolder.layoutInfo.visibleItemsInfo[listScrollStateHolder.layoutInfo.visibleItemsInfo.size - 1].index
                lastVisibleItemOffset = listScrollStateHolder.layoutInfo.viewportEndOffset - listScrollStateHolder.layoutInfo.visibleItemsInfo[listScrollStateHolder.layoutInfo.visibleItemsInfo.size - 1].offset
            } else {    // **step 4**
                if (!listScrollStateHolder.isScrollInProgress) {
                    var itemDetails: LazyListItemInfo? = null
                    for (item in listScrollStateHolder.layoutInfo.visibleItemsInfo){
                        if (item.index == lastVisibleItemIndex){
                            itemDetails = item
                            break
                        }
                    }
                    if (itemDetails != null){
                        val lastItemCurrentOffset = (listScrollStateHolder.layoutInfo.viewportEndOffset - itemDetails.offset) - lastVisibleItemOffset
                        if (hasScrolledOnce){
                            scope.launch { listScrollStateHolder.animateScrollBy(-lastItemCurrentOffset.toFloat()) }
                        }
                    }
                    hasScrolledOnce = false
                }
            }
        }
    }

}
```
and we can call this function like [it is called here](https://github.com/litoco/SmallProjects/blob/9c58dbc45b03d640921bc9f8f594358e8c327bd7/SolutionTestingApp/app/src/main/java/com/example/solutiontestingapp/MainActivity.kt#L73).

</br>
</br>

Complete code is available [here](https://github.com/litoco/SmallProjects/blob/9c58dbc45b03d640921bc9f8f594358e8c327bd7/SolutionTestingApp/app/src/main/java/com/example/solutiontestingapp/MainActivity.kt).

With the above code we achieve our desired output as shown below:

![Working proof gif](https://i.ibb.co/JFKn541/uploader.gif)
