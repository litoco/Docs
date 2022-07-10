## Problem statement
While implementing the [recomposition](https://github.com/litoco/AndroidDocs/blob/main/jetpack-compose/recomposition/README.md) with a list, I encountered this problem of `composable` function getting called multiple times. This lead to the problem in which the size of the list was increased infinitely on adding just an element on a button click.
Following is the code that had the problem:\
***File1.kt***
```
@Composable
fun ShowList(previousElements:List<String>){
  ...
  ...
  val list = remember{mutableStateListOf<String>()}
  list.addAll(previousElements)
  ...
  ...
  RespondToButtonClick(list)
}
```
***File2.kt***
```
@Composable
fun RespondToButtonClick(list:List<String>){
  ...
  ...
  IconButton (onClick = {
  list.add("One more element added")
  })
  ...
  ...
}
```


## Problem detection
It was visible from the UI that the size of the list which was initially fixed, kept on increasing after I tried adding an element on a button click.
It was also visible that the same set of elements that were added before, were getting added again.\
So my very first response was to look into the initialisation part and try to find the problem there. 

I added the logger just below the line where I was initialising the list with a set of items. 
```
@Composable
fun ShowList(previousElements:List<String>){
  ...
  ...
  val list = remember{mutableStateListOf<String>()}
  list.addAll(previousElements)
  Log.d("Debugging", "The size of the list is ${list.size}"
  ...
  ...
}
```
After relaunching the application with the logger, I tried to confirm my assumption of assumption by looking into the logcat for debug message. After the button click there was an infinite log of debug log messages in the logcat. The size of the list was also increasing infinitely.

This confirmed that the there was some problem with the in the initialisation of the list.


## Problem Solution
After identifying this problem, I got to understand that the `composable` function `ShowList` was getting called multiple times because the data inside this was changing (i.e. the line `list.addAll(previousElements)` was getting called after each recomposition and this was resposible for calling `ShowList` function again because the list data changed). Hence we were stuck in a cycle.

It became almost certain that we needed to modify the list initialisation line `list.addAll(previousElements)`. I tried to find how we can do that modify this initialisation and came up with the with following code:
```
@Composable
fun ShowList(previousElements:List<String>){
  ...
  ...
  val list = remember{previousElements.toMutableStateList()}
  Log.d("Debugging", "The size of the list is ${list.size}"
  ...
  ...
}
```

This modification `val list = remember{previousElements.toMutableStateList()}` helped me to do initialise the variable to `mutableStateList` correctly. It solved the problem and I learned a new thing today.

Hope this will help someone.\
Thanks.
