# State Hoisting (Work In Progress)
To understand **state hoisting** lets first understand the two words that it comprises of i.e. **state** and **hoisting**.

**State**: As mentioned in the [offical docs](https://developer.android.com/jetpack/compose/state), in android, a state in an android app is any value that can change over time. And yes, it does sound similar to a variable definition in computer programming. And they are stored in variables (as I have seen till now). \
So why do they call it a **state** and not variable? Good question, I am also trying to understand that.

**Hoisting**: And hoisting generally means moving up.

So on combining the above two definitions **state hoisting** would mean moving up of any value that changes with time. In android term what we do in **state hoisting** is move the **states** from called `composable` function to the calling `composable` function.
