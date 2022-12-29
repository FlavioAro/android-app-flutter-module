# Integrating a Flutter module into a native Kotlin Android app

### Start Project
to start the project access the kotlin_app project in Android Studio and do a 'sync'
```sh 
This command is moved to File > Sync Project with Gradle Files.
```

#### Documentation
Integrate a Flutter module into your Android project:
https://docs.flutter.dev/development/add-to-app/android/project-setup#manual-integration

Adding a Flutter screen to an Android app:
https://docs.flutter.dev/development/add-to-app/android/add-flutter-screen?tab=default-activity-launch-kotlin-tab

In the kotlin_app project, below are the steps to implement the module

## Create a Flutter module
```sh 
flutter create -t module --org com.example flutter_module
```

## Add the Flutter module as a dependency
```sh 
flutter build aar
```

## Add the `profile` build type to `app/build.gradle`
```sh
    android {
      buildTypes {
        profile {
          initWith debug
        }
      }
    }
```

## App depend on the Flutter module to `app/build.gradle`
```sh
    dependencies {
      debugImplementation 'com.example.flutter_module:flutter_debug:1.0'
      profileImplementation 'com.example.flutter_module:flutter_profile:1.0'
      releaseImplementation 'com.example.flutter_module:flutter_release:1.0'
    }
```

## App url in build to `settings.gradle`
```sh
      repositories {
        maven {
            url '../flutter_module/build/host/outputs/repo'
        }
        maven {
            url "https://storage.googleapis.com/download.flutter.io"
        }
      }
```

## Add FlutterActivity to `AndroidManifest.xml`
```sh
 <activity
   android:name="io.flutter.embedding.android.FlutterActivity"
   android:theme="@style/LaunchTheme"
   android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|layoutDirection|fontScale|screenLayout|density|uiMode"
   android:hardwareAccelerated="true"
   android:windowSoftInputMode="adjustResize"
   />
```

## Add import Launch FlutterActivity to `MainActivity.kt`
```sh
   import io.flutter.embedding.android.FlutterActivity
```

## `Kotlin` code example:
Start the flutter module
```sh
 myButton.setOnClickListener {
   startActivity(
     FlutterActivity.createDefaultIntent(this)
   )
 }
```

Access a specific screen, through a route
```sh
 myButton.setOnClickListener {
   startActivity(
     FlutterActivity
       .withNewEngine()
       .initialRoute("/my_route")
       .build(this)
   )
 }
```