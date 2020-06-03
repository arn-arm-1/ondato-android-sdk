# Ondato Android SDK

## Table of contents

* [Overview](#overview)
* [Getting started](#getting-started)
* [Customising SDK](#customising-sdk)

## Overview

This SDK provides a drop-in set of screens and tools for Android applications to allow capturing of identity documents and face photos/live videos for the purpose of identity verification. The SDK offers a number of benefits to help you create the best onboarding/identity verification experience for your customers:

- Carefully designed UI to guide your customers through the entire photo/video-capturing process
- Modular design to help you seamlessly integrate the photo/video-capturing process into your application flow
- Advanced image quality detection technology to ensure the quality of the captured images meets the requirement of the Ondato identity verification process, guaranteeing the best success rate
- Direct image upload to the Ondato service, to simplify integration\*

\* Note: the SDK is only responsible for capturing and uploading photos/videos. You still need to access the [Ondato API](https://documenter.getpostman.com/view/6997242/S1TZwaZe?version=latest) to create and manage checks.

## Important note

We recommend you to lock your app to a portrait orientation.

## Getting started


The SDK supports API level 18 and above ([distribution stats](https://developer.android.com/about/dashboards/index.html)).

Our configuration is currently set to the following:

- `minSdkVersion = 18`
- `compileSdkVersion = 29`
- `targetSdkVersion = 29`
- `Kotlin = 1.2+`

### 1. Adding the SDK dependency

Download Ondato.zip, extract and move files to /libs folder of your project

```gradle
repositories {
   jcenter()
   google()
}
```

```app-gradle
repositories {
   jcenter()
   google()
}

repositories {
    maven {
        url 'http://maven.facetec.com'
    }
}

android {
      compileOptions {
            sourceCompatibility 1.8
            targetCompatibility 1.8
      }
}
            
dependencies {
   implementation fileTree(dir: 'libs', include: ['*.aar'])

   implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
   implementation 'androidx.appcompat:appcompat:1.1.0'
   implementation 'com.google.android.material:material:1.0.0'
   implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
   
   implementation 'com.squareup.retrofit2:retrofit:2.7.0'
   implementation 'com.squareup.retrofit2:converter-gson:2.7.0'
   implementation 'com.squareup.retrofit2:converter-scalars:2.1.0'
   api 'com.squareup.okhttp3:okhttp:4.2.2'
   
   testImplementation 'junit:junit:4.12'
   
   implementation 'com.otaliastudios:cameraview:1.6.0'
   implementation "com.github.bumptech.glide:glide:4.10.0"
   kapt "com.github.bumptech.glide:compiler:4.10.0"
}
```

### 2. Instantiating the client

To use the SDK, you need to obtain an instance of the client object:

Kotlin 

``` kotlin
Ondato ondato = Ondato.instance// or OndatoFactory().getInstance()
```

Java

``` java
Ondato ondato = Ondato.Companion.getInstance();// or new OndatoFactory().getInstance();
```


### 3. Creating the SDK configuration

Create an `OndatoConfig` using your username, along with the password, choose mode of :"TEST" and "LIVE" environment, default mode is TEST. Also you can initialize SDK with token, token parameter is availble only for LIVE mode.

``` java
final OndatoConfig config = new OndatoConfig.Builder()
            .loginWith(ACCESS_TOKEN)//Token must be with type (Bearer $bearerToken)
            .loginWith("username","password") //just in Kotlin, not able to use in JAVA. 
            .loginWith("username","password","token") // Must pass token parameter in JAVA, token can be null.
            .withMode(OndatoConfig.Mode.TEST)
            .withCustomData(customData: Any?) // optionally pass an object as a customData parameter
            .withFrontendValidation(false) // optionally disable in app validations
            .withIdentificationId("identificationId") // optionally provide identificationId value
            .build();
```

### 4. Starting the flow

``` java
// start the flow.
ondato.init(config);
ondato.starIdentification(context, new Ondato.ResultListener(){

            @Override
            public void onSuccess(identificationId: String?) {

            }

            @Override
            public void onFailure(identificationId: String?, error: OndatoError) {

            }
        });
```

Congratulations! You have successfully started the flow. Carry on reading the next sections to learn how to:

- Customise the SDK
- Create checks

## Customising SDK

### 1. Splash Screen Step
This screen is used to load data from server. Logo can be changed with the following parameter:`withSplashScreenLogo(R.drawable.res : Int)`


### 2. Theme customisation

In order to enhance the user experience on the transition between your application and the SDK, you can provide some customisation by defining certain colors inside your own `colors.xml` file:
    
`ondatoColorProgressBarAccent`: Defines the background color of the `ProgressBarView` which guides the user through the flow

`ondatoColorButtonText`: Defines the background color of the primary action buttons text

`ondatoColorButtonBackgroundUnfocused`: Defines the background color of the primary action buttons

`ondatoColorButtonBackgroundFocusedStart`: Defines the background color of the primary action buttons gradient start when pressed

`ondatoColorButtonBackgroundFocusedCenter`: Defines the background color of the primary action buttons gradient center when pressed

`ondatoColorButtonBackgroundFocusedEnd`: Defines the background color of the primary action buttons gradient end when pressed

`ondatoColorErrorBg`: Defines the background color of the error message background

`ondatoColorErrorText`: Defines the background color of the error message text color

`ondatoColorPrimaryDark`: Defines the taskbar color

`ondatoColorBackground`: Defines the background of the Main activity

### 3. Flow
`OndatoViewCreator` provide some optional methods to replace Ondato windows with custom ones: 
``` java
.withViewCreator(object: OndatoViewCreator {
    override fun createInitialFragment(): Fragment? {
        return InitialFragment() //it can return null, if null is returned, start layout is not shown
    }
    override fun createLoadingFragment(): Fragment {
        return LoadingFragment()
    }
    override fun createTypeSelectionFragment(): Fragment {
        return TypeSelectionFragment()
    }
})
.withShowSuccess(true) // you can show or hide success layout
.withLanguage(Languages.Lithuanian) // set app language, need to pass enum
.withShowDefaultSplash(showSplash: Boolean) // boolean value set splash screen visibility
```

InitialFragment should contain 
`(activity as? OndatoFlowController).start() // start the flow`

TypeSelectionFragment should contain
`(activity as? OndatoFlowController).onDocumentTypeSelected(DocumentType.PASSPORT) // pass document type enum`

OndatoFlowController can change language with 
`(activity as OndatoFlowController).changeLanguage(Languages.Lithuanian) // pass language enum`

After language change identification process restart.

Every fragment must have

`
android:background="@color/your_color" 
android:clickable="true" 
android:focusable="true"
`

### 4. Localisation

Ondato Android SDK already comes with out-of-the-box translations for the following locales:
- English (en) :uk:
- Lithuanian (lt) :lt:
