# Migrating to compose

## Files that we will interact with
1. Gradle file:  /Users/username/AndroidStudioProjects/appname/gradle/wrapper/gradle-wrapper.properties 
2. Project level build.gradle file: /Users/username/AndroidStudioProjects/appname/app/build.gradle 
3. App level build.gradle file: /Users/username/AndroidStudioProjects/appname/build.gradle 
4. MainActivity.kt : /Users/username/AndroidStudioProjects/appname/app/src/main/java/com/example/appname/MainActivity.kt

## Steps
1. Open the existing project in android studio
2. If the existing app is failing with android studio check for the gradle version that it is using. It it typically present in:\
/Users/username/AndroidStudioProjects/appname/gradle/wrapper/gradle-wrapper.properties \
The version for example can be something like [this](https://developer.android.com/jetpack/compose/interop/adding#setup-studio)
3. Make sure that we are using kotlin 1.7.20. We can check this in project level build.gradle file, which is typically present in:\
/Users/username/AndroidStudioProjects/appname/app/build.gradle \
For example at the time of writing this documentation it was `id 'org.jetbrains.kotlin.android' version '1.7.20' apply false`
4. Once the above steps are executed successfully in sequence, we can start adding build configuration to our project to make it compose compatible
      - First of all we need to add 
          ```
          android {
            …
            …   
            buildFeatures {
                compose = true
            }

            composeOptions {
                kotlinCompilerExtensionVersion = "1.3.2"
            }
          }
        ```
        To our app level build.gradle file.
        Setting **`compose = true`** enables compose functionality\
        and\
        `kotlinCompilerExtensionVersion = “1.3.2”`, is the compose compiler version that would be compatible with the project’s kotlin version. \
        To know the compatible version of compose, checkout [this](https://developer.android.com/jetpack/androidx/releases/compose-kotlin#pre-release_kotlin_compatibility)
        
      - Add the corresponding compose compiler version in the `dependency` section of app level build.gradle file. \
      For example like `implementation("androidx.activity:activity-compose:1.6.1”)`
      - (Optional) Add material design to the app by adding `implementation("androidx.compose.material3:material3:1.0.1")` in the `dependency` section of app level build.gradle
      - In addition to this add a compose BOM (Bill Of Materials). This BOM contains links to the stable versions of the different Compose libraries, such that they would work well together. The detail about BOM can be found [here](https://developer.android.com/jetpack/compose/setup#bom-version-mapping).\
      To add BOM, add the following code snippet to `dependencies` section of app level build.gradle file:
      ```
      def composeBom = platform("androidx.compose:compose-bom:2022.10.00")
      implementation composeBom
      androidTestImplementation composeBom
      ```
      
5. Now we need to modify the `MainActivity.kt` file because we will be using compose functions in this activity itself

    - Extend MainActivity with ComponentActivity instead of AppCompatActivity `class MainActivity : ComponentActivity()`
    - Replace `setContentView` function with `setContent` function\
    For Example:
    ```
    setContent {
      MaterialTheme {
        MainScreen()
      }
    }
    ```
    - Create a composable function that would translate into the apps screen\
    For Example:
    ```
    @Composable
    fun MainScreen() {
          Text(text = "Hello, there!!", modifier = Modifier.background(Color.Green))
    }
    ```
    
    For starter I created an empty activity project and then migrated it to compose. \
    [Link to the app with the above feature](https://github.com/litoco/SmallProjects/tree/12845dca680e8a78ad0ec2cec5ee15de918e0129).
