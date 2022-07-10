# Summary of contents
In this document we will learn the following:
1. [What is recomposition](#recomposition)
2. [Why do we need recomposition](#understanding-the-need-to-recompose)
3. [What are the steps to achieve recomposition](#steps-for-recomposition)

## Recomposition
**Recomposition** is the process of calling composable functions to update the UI based on dynamically changing data.

## Understanding the need to recompose
In jetpack compose the very first UI that shows up on the screen is preserved. So, if we have a dynamically changing data, it won't update the UI automatically based on the data. We need mention this explicitly to android that it should update the UI whenever the data changes.\
Lets see the following behaviour to understand the need of recomposition:
|**Before Recomposition**|**After Recomposition**|
|------------------------|-----------------------|
|![before recomposition](https://i.stack.imgur.com/xYXW6.gif)|![after recomposition](https://i.stack.imgur.com/p7Jx3.gif)|

As we can see in the first case, that although we expect the `text field` to update with the text that we type, it doesn't do so because we haven't told it to update the UI when the text change. This is exaclty what we do in **recomposition**.

In **recomposition** we first tell which data to observe to. Whenver this observable data gets changed it re-calls the `composable` function to re-create the UI with updated data. In this way the UI is updated with the dynamic content.

For more details please go through the official android documentation on [thinking in compose](https://developer.android.com/jetpack/compose/mental-model#recomposition).

## Steps for recomposition
So, in order to implement recomposition, we follow the following steps:
#### Create a composable variable 
```
@Composable
fun ShowText(){
  ...
  ...
  val text = remember { mutableStateOf("") } //this creates an empty observable string, any change to this variable will trigger recomposition
  ...
  ...
}
```
NOTE: we surround the initialisation of composable function with `remember` block because we want the string initialisation to happen only. After that it just adds the new value to the previously existing one. \
If we don't use `remember` it will re-initialise `text` variable on every recomposition (i.e. on every `ShowText()` function call). 

#### Update this variable with latest value
```
@Composable
fun ShowText(){
  ...
  ...
  val text = remember { mutableStateOf("") } //this creates an empty observable string, any change to this variable will trigger recomposition
  OutlinedTextField (
    value = text
    onValueChange = {text = it} // updates the observable variable to contain the old value + the latest value
  )
  ...
  ...
}
```
