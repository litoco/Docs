# State Hoisting (Work In Progress)
To understand **state hoisting** lets first understand the two words that it comprises of i.e. **state** and **hoisting**.

**State**: As mentioned in the [offical docs](https://developer.android.com/jetpack/compose/state), in android, a state in an android app is any value that can change over time. And yes, it does sound similar to a variable definition in computer programming.\
So why not call a state, a variable? Because states are Jetpack specific whereas variables are programming language specific.

**Hoisting**: And hoisting generally means moving up.

So on combining the above two definitions **state hoisting** would mean moving up of any value that changes with time. In android term what we do in state hoisting is move the **states** from called composable function to the composable function that is calling it.\
This makes the composables easy to reuse and decoupled (because any modification in caller function will not require modification in called function). Since this makes composables not strore state we calling it a `stateless` composable.

So state hoisting help us to make the function **testable** and **reusable**


We can implement state hoisting by:
1. replacing the state as functions argument, and
2. replacing events as function argument
3. Help implementing [recommended architecture](https://developer.android.com/topic/architecture#recommended-app-arch) with jetpack compose.

For example: In [here](https://github.com/litoco/SmallProjects/blob/f1261663f27c65682a7217f553d63446f7e3d9ab/SolutionTestingApp/app/src/main/java/com/example/solutiontestingapp/MainActivity.kt#L42) we have `text` as state and `onTextChange` as event handler function.

