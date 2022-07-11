## Problem statement
While implementing `LazyColumn` with `TextField`. When `TextField` gains focus for text input from the user it hides the `LazyColumn` items with `Keyboard`. Following is example of what is happening:
![pic](https://i.stack.imgur.com/vcAbP.gif)

So `LazyColumn` doesn't scroll to very last element and its data gets hidden by the `Keyboard`. At least it should show the very last item that was visible on the screen before the keyboard was shown on to the screen

## Problem detection
This was a UI problem and hence it was detected while testing on the screen. 

## Problem solution
I have created an [issue](https://issuetracker.google.com/issues/237566530) for this bug, feel free to add (+1) on it. 

For now we will go with a workaround unless that issue is fixed. So the workaround is as following:
1. Write logic to check if keyboard is visible or not
2. Listen to keyboard visibility change
3. On keyboard visibililty scroll the lazy column
4. On keyboard invisiblility scroll back lazy column to its previous state

Step 1:\
To check for keyboard availability, following function was used
```
fun View.isKeyboardOpen(): Boolean {
    val rect = Rect()
    getWindowVisibleDisplayFrame(rect)
    val screenHeight = rootView.height
    val keypadHeight = screenHeight - rect.bottom
    return keypadHeight > screenHeight * 0.1
}
```

Step 2:\
Next we have to listen for keyboard visibility changes, we can do that by the following composable function:
```
@Composable
fun rememberIsKeyboardOpen(): State<Boolean> {
    val view = LocalView.current
    return produceState(initialValue = view.isKeyboardOpen()) {
        val viewTreeObserver = view.viewTreeObserver
        val listener = ViewTreeObserver.OnGlobalLayoutListener { value = view.isKeyboardOpen() }
        viewTreeObserver.addOnGlobalLayoutListener(listener)
        awaitDispose { viewTreeObserver.removeOnGlobalLayoutListener(listener)  }
    }
}
```

Step 3 & Step 4:\
We accomplish behaviour with respect to keyboard visibility and invisibility, via the following code block:
```
@Composable
fun AppUI(){
    ...
    ...
    val listScrollStateHolder = rememberLazyListState()
    val scope = rememberCoroutineScope()
    val keyboardVisible by rememberIsKeyboardOpen()
    var firstVisibleItemIndexedValue by remember { mutableStateOf(0)}
    var screenSize by remember { mutableStateOf(0)}
    var scrollOffset by remember{ mutableStateOf(0)}
    ...
    ...
    ...
    LaunchedEffect(key1 = keyboardVisible){
          if (listScrollStateHolder.layoutInfo.visibleItemsInfo.size - screenSize > 10){
              screenSize = listScrollStateHolder.layoutInfo.visibleItemsInfo.size
          }
          if(keyboardVisible){
              firstVisibleItemIndexedValue = listScrollStateHolder.firstVisibleItemIndex
              scrollOffset = if(screenSize - listScrollStateHolder.layoutInfo.visibleItemsInfo.size > 0) screenSize + 1 - listScrollStateHolder.layoutInfo.visibleItemsInfo.size else 0
              scope.launch {
                  listScrollStateHolder.animateScrollToItem(firstVisibleItemIndexedValue + scrollOffset)
              }
          } else{
              if (listScrollStateHolder.firstVisibleItemIndex + screenSize + 2 < list.size) {
                  firstVisibleItemIndexedValue = if (listScrollStateHolder.firstVisibleItemIndex - scrollOffset > 0)  listScrollStateHolder.firstVisibleItemIndex - scrollOffset else 0
                  scope.launch {
                      listScrollStateHolder.animateScrollToItem(firstVisibleItemIndexedValue)
                  }
              }
          }
      }
}
```

Complete code is available [here](https://github.com/litoco/SmallProjects/blob/main/SolutionTestingApp/app/src/main/java/com/example/solutiontestingapp/MainActivity.kt).

With the above code we achieve our desired output as shown below:
![Working proof gif](https://i.ibb.co/FxsmMKj/uploader.gif)
