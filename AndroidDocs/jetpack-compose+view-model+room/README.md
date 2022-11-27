# Jetpack Compose + ViewModel + Room

In this documentation we are going to make an application that will diplay `userId` of the user and store it in the local storage using Room persistent library. This app is an extension of this [existing application](https://github.com/litoco/SmallProjects/tree/12845dca680e8a78ad0ec2cec5ee15de918e0129/MigrateItToCompose) on [migrating to compose](https://github.com/litoco/Docs/tree/main/AndroidDocs/jetpack-compose/migrating-to-compose).

# Steps
1. [Add necessary dependencies](https://github.com/litoco/Docs/edit/main/AndroidDocs/jetpack-compose+view-model+room/README.md#add-necessary-dependecies)
2. [Create Entities, DAO and RoomDatabase to CRUD local storage](https://github.com/litoco/Docs/edit/main/AndroidDocs/jetpack-compose+view-model+room/README.md#create-entities-dao-and-roomdatabase-to-createmodify-local-storage)
3. [Use LiveData to integrate Compose and ViewModel with Room](https://github.com/litoco/Docs/edit/main/AndroidDocs/jetpack-compose+view-model+room/README.md#use-livedata-to-integrate-compose-and-viewmodel-with-room)


## Add necessary dependecies
In app level `build.gradle` file add the following:
```
plugins {
    ...
    ...
    id 'kotlin-kapt'
}


dependencies {
    ...
    ...
    
    // Room database
    implementation("androidx.room:room-runtime:2.4.3")
    annotationProcessor("androidx.room:room-compiler:2.4.3")

    // To use Kotlin annotation processing tool (kapt)
    kapt("androidx.room:room-compiler:2.4.3")

    // optional - Kotlin Extensions and Coroutines support for Room
    implementation("androidx.room:room-ktx:2.4.3")
}
```


## Create Entities, DAO and RoomDatabase to CRUD local storage

- **`Entities`** in `Room Database` can be used to create table to store data. In our case since we will storing `userId` following will the class for it:
  ```
  @Entity(tableName = "UserId")
  class UserId (
      @PrimaryKey(autoGenerate = true) val id:Int,
      @ColumnInfo(name = "userId") val userId:String
  )
  ```
  **Important Note**: Over here we have created a table with name `UserId`. There is a required primary key `userId` column that will store our userId string in the table.


- Once we have created **`Entities`** successfully we would create its corresponding **`DAO (Data Access Object)`** to query this **`Entities`** table. Following is the code for that:
  ```
  @Dao
  interface RoomDAO {
      @Query("SELECT userId FROM Userid")
      fun getUserId(): LiveData<String>

      @Insert(onConflict = OnConflictStrategy.REPLACE)
      suspend fun insertUserId(userId: UserId)

  }
  ```

- After successful **`DAO`** creation we need **`Room Database`** to allow access of persistent local storage for the app. It allows you access **`DAO`** for performing CRUD operation on the local storage. 
Following is the code for that:
  ```
  @Database(entities = [UserId::class], version = 1)
  abstract class RoomLocalDatabase: RoomDatabase() {
      abstract fun getRoomDAO(): RoomDAO

      companion object {
          // Singleton prevents multiple instances of database opening at the
          // same time.
          @Volatile
          private var INSTANCE: RoomLocalDatabase? = null

          fun getDatabase(context: Context): RoomLocalDatabase {
              // if the INSTANCE is not null, then return it,
              // if it is, then create the database
              return INSTANCE ?: synchronized(this) {
                  val instance = Room.databaseBuilder(
                      context.applicationContext,
                      RoomLocalDatabase::class.java,
                      "word_database"
                  ).build()
                  INSTANCE = instance
                  // return instance
                  instance
              }
          }
      }
  }
  ```
  Here we have used `singleton pattern` that allows only one object creation for a class. It is necessary to prevent having multiple instances of the database opened at the same time which can bring inconsitency and hamper data integrity.

## Use LiveData to integrate Compose and ViewModel with Room

Now that we have our database configured and ready for use we can integrate it with `view model` and then with `compose` to show `UserId` in the UI.\
We need a `Repository` that would be act as a single source of truth for our data. It would handle the logic of creating `userId`, inserting and retrieving it from the local storage. Following is the code for that:
```
class Repository(private val roomDAO: RoomDAO) {
    fun getUserId(lifecycleOwner: LifecycleOwner, appViewModel: AppViewModel): LiveData<String> {
        val userIdLiveData = MutableLiveData("")
        roomDAO.getUserId().observe(lifecycleOwner){
            if (it.isNullOrEmpty()){
                appViewModel.messageData = "Generating userId please wait.."
                var string = ""
                for (i in (1..12)){
                    val isCapital = Random.nextBoolean()
                    string += if (isCapital){
                        ('A'..'Z').random()
                    } else {
                        ('a'..'z').random()
                    }
                }
                appViewModel.viewModelScope.launch {
                    insertToLocalStorage(string)
                }
                userIdLiveData.value = string
            } else {
                userIdLiveData.value = it
            }
        }
        return userIdLiveData
    }

    @WorkerThread
    private suspend fun insertToLocalStorage(string: String) {
        val userId = UserId(0, string)
        roomDAO.insertUserId(userId = userId)
    }
}
```

Here one important thing to note is that we are creating a `suspend` function called `insertToLocalStorage`. It is responsible for calling out the `insert` function that would insert the data into the local storage. 

Suspend functions are called from `coroutines` or another suspend function and they run on background thread, while the main thread handles the UI changes.


We now need to create instances of our **`repository`** and **`DAO`**. We will be doing that in our `ViewModel` like below:
```
class AppViewModel(application: Application) : AndroidViewModel(application) {
    ...
    private val database by lazy { RoomLocalDatabase.getDatabase(application) }
    private val repository by lazy { Repository(database.getRoomDAO()) }


    fun getUserId(lifecycleOwner: LifecycleOwner) {
        repository.getUserId(lifecycleOwner, this).observe(lifecycleOwner){
            ...
            ...
        }
    }
}
```

Then we will be able to access this `ViewModel` from our activity and call the `getUserId` function of the `ViewModel` on the function call to get our `userId`. Like this:
```
  @Composable
  fun MainScreen(
      viewModel: AppViewModel = viewModel()
  ) {
      val lifecycleOwner = LocalLifecycleOwner.current
      ShowUI(text = viewModel.messageData) { viewModel.getUserId( lifecycleOwner) }
  }
```

Then finally we can update our UI by modifying the state variables in the `ViewModel` like below:
```
class AppViewModel(application: Application) : AndroidViewModel(application) {
    var messageData by mutableStateOf("Hi, there!!!\nClick on the button below to get your userId")
    ...
    ...


    fun getUserId(lifecycleOwner: LifecycleOwner) {
        repository.getUserId(lifecycleOwner, this).observe(lifecycleOwner){
            messageData = if (!it.isNullOrEmpty()){
                "Your userId is: $it"
            } else{
                "Sorry!!\nCan't generate your userId at the moment\nPlease retry"
            }
        }
    }
}
```

Once the `messageData` state changes it triggers the recomposition of the compose functions. So we would see the UI with the updated message.

These changes are present in [this app](https://github.com/litoco/SmallProjects/tree/cf4a79d81a1033a29fa7d22433c108de6b1dea01/MigrateItToCompose).
