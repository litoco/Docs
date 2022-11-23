# Using viewmodels with Jetpack Compose

Before we start I assume that you have completed the [basic setup](https://github.com/litoco/Docs/tree/main/AndroidDocs/jetpack-compose/migrating-to-compose#readme) of migrating your app to compose. And you know how [compose functions work within the app](https://developer.android.com/jetpack/compose/state)

In this section, we will try to create an app that updates a text on button click, using jetpack compose and view model

# Steps:

1. [Add necessary dependency](https://github.com/litoco/Docs/edit/main/AndroidDocs/jetpack-compose/migrating-to-compose/compose_and_viewmodels/README.md#add-necessary-dependency)
2. [Add compose UI](https://github.com/litoco/Docs/edit/main/AndroidDocs/jetpack-compose/migrating-to-compose/compose_and_viewmodels/README.md#add-compose-ui)
3. [Create a ViewModel class with necessary details](https://github.com/litoco/Docs/edit/main/AndroidDocs/jetpack-compose/migrating-to-compose/compose_and_viewmodels/README.md#add-viewmodel-class-with-logic)
4. [Integrate ViewModel with compose](https://github.com/litoco/Docs/edit/main/AndroidDocs/jetpack-compose/migrating-to-compose/compose_and_viewmodels/README.md#integrate-viewmodel-with-jetpack-compose)


## Add necessary dependency

In your app level `buid.gradle` file add the following dependency
```
dependencies {
  ...
  ...
  
  implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.5.1")
}
```

## Add compose UI

Add your compose function with the necessary details to display the UI\
For example I had the following composable function
```
@Composable
fun ShowUI(text:String, updateText: ()-> Unit) {
    Column(
        modifier = Modifier.fillMaxSize(1f),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text(text = text, modifier = Modifier.background(color = Color.Cyan))
        Button(onClick = updateText ) {
            Text(text = "Click me")
        }

    }
}
```

## Add ViewModel class with logic

Create your `ViewModel` class and put the necessary logic in it.\
For example in this case we had to update the text when the user clicks a button, so following is the implementation for that

```
class AppViewModel: ViewModel() {
    var messageData by mutableStateOf(MainScreenDataModel().message)
        private set

    var counter = 1

    fun updateMessageText(){
        messageData = "Button clicked $counter time(s)"
        counter ++
    }
}
```

**NOTE:**

<span style="color:blue">Notice that here we have used `mutableStateOf` function which is part of the compose recomposition API that tell compose to update its view whenever the data within these API changes</span>


## Integrate ViewModel with Jetpack Compose

Now that we have our stateless compose function (compose function with state hoisted up) with [state hoisting](https://developer.android.com/jetpack/compose/architecture#udf) and viewmodel with the necessary logic. 

We just need to integrate both of them. For that 
  - Create another composable function that would be parent to `ShowUI` function that we had created above
  - get ViewModel's instance, and handle the button click `event` by calling the `updateMessageText` function that we had written in the `AppViewModel` class
  - Call `ShowUI` from this composable function with appropriate parameters

Following is the code for the above steps:
```
@Composable
fun MainScreen(
    viewModel: AppViewModel = viewModel()
) {
    ShowUI(text = viewModel.messageData) { viewModel.updateMessageText() }
}
```

The working app is present [here](https://github.com/litoco/SmallProjects/tree/12845dca680e8a78ad0ec2cec5ee15de918e0129)

Thats all. \
Build and run the application, to see it in action!!
