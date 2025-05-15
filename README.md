# How to install the SDK

1. Add the `inseat.aar` to your project
    For multi-module projects, you can add the `inseat.aar` file as a dependency in your project, by following these steps:
    1. Create a new directory named `libs` in your project directory.
    2. Copy the `inseat.aar` file into the `libs` directory.
    3. Open your project's `settings.gradle.kts` file and add the following code:
    ```kotlin
   dependencyResolutionManagement {
     repositories {
          flatDir {
                dirs("libs")
          }
     }
   }
    ```
    4. Open your module's `build.gradle.kts` file and add the following code:
    ```kotlin
    dependencies {
        implementation("com.immfly.inseat.sdk:inseat@aar")
    }
    ```

2. This SDK requires the following dependencies, so you have to add them to your project:

module when you call initialize method:
```kotlin
dependencies {
    // Needed for all SDK methods
   implementation("androidx.core:core-ktx:1.10.1")
   implementation("androidx.core:core:1.9.0")
   implementation("androidx.appcompat:appcompat:1.6.1")
   implementation("androidx.annotation:annotation:1.8.0")
   implementation("androidx.lifecycle:lifecycle-process:2.5.1")
   implementation("androidx.lifecycle:lifecycle-runtime:2.5.1")
   
   implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.6.0")
   implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.6.4")
   implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4")
   implementation("org.jetbrains.kotlin:kotlin-stdlib:1.9.24")
   implementation("org.jetbrains.kotlin:kotlin-reflect:1.9.24")
   
   implementation("com.fasterxml.jackson.core:jackson-databind:2.13.4.2")
   implementation("com.fasterxml.jackson.module:jackson-module-kotlin:2.13.4")
   implementation("com.fasterxml.jackson.dataformat:jackson-dataformat-cbor:2.13.4")
   
   // Needed for method syncProductData() of this SDK
   implementation("com.squareup.okhttp3:okhttp:4.12.0")
   implementation("com.squareup.okhttp3:logging-interceptor:4.12.0")
}
```

# How to use the SDK
1. Initialize the SDK
   ```kotlin
   InseatSdk.initialize(
       initialize(
                applicationContext = this@MainApplication,
                apiKey = "Your_api_key"
            )
   )
   ```
   
2. Call the `InseatSdk.getInstance().syncProductData()` method to sync the product data.
3. Call the `InseatSdk.getInstance().start` method to start the SDK.
4. Call the `InseatSdk.getInstance().fetchMenus` method to get all menus.
5. Call the `InseatSdk.getInstance().setUserData` method to choose menu.
6. Use methods like `observeShop`, `observeProducts` observe necessary data.

# Support
If you have a feature request, or spotted a bug or a technical problem, please contact our support team.

# Preconditions
In order to access this repository you need a token to download the artifacts.
You will also need an API KEY to use our services.
In order to get them please contact our support team.
