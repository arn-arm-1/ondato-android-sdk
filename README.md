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

\* Note: the SDK is only responsible for capturing and uploading photos/videos. You still need to access the [Ondato API](https://documentation.ondato.com/) to create and manage checks.

## Important note

We recommend you to lock your app to a portrait orientation.

## Getting started


The SDK supports API level 17 and above ([distribution stats](https://developer.android.com/about/dashboards/index.html)).

Our configuration is currently set to the following:

- `minSdkVersion = 17`
- `compileSdkVersion = 27`
- `targetSdkVersion = 27`
- `Android Support Library = 27.1.1`
- `Kotlin = 1.2+`

### 1. Adding the SDK dependency

Starting on version `4.2.0` you can integrate it:

Download OndatoSDK.aar and move to /libs folder of your project


```gradle
repositories {
   jcenter()
   google()
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    implementation 'com.android.support:appcompat-v7:27.1.1'
    implementation 'com.android.support:design:27.1.1'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    implementation 'com.android.support:support-v4:27.1.1'

    implementation 'com.squareup.retrofit2:retrofit:2.1.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.1.0'
    implementation  'com.squareup.retrofit2:converter-scalars:2.1.0'
    api 'com.squareup.okhttp3:okhttp:3.10.0'
    testImplementation 'junit:junit:4.12'

    implementation 'com.otaliastudios:cameraview:1.6.0'
}
```

### 2. Instantiating the client

To use the SDK, you need to obtain an instance of the client object:

Kotlin 

``` kotlin
Ondato ondato = Ondato.instance
```

Java

``` java
Ondato ondato = Ondato.Companion.getInstance();
```


### 3. Creating the SDK configuration

Create an `OndatoConfig` using your username, along with the password, choose mode of :"TEST" and "LIVE" environment, default mode is TEST.

``` java
final OndatoConfig config = new OndatoConfig.Builder()
            .loginWith("username","password")
            .withMode(OndatoConfig.Mode.TEST)
            .build();
```

### 4. Starting the flow

``` java
// start the flow.
ondato.init(config);
ondato.starIdentification(context, new Ondato.ResultListener(){

            @Override
            public void onSuccess() {

            }

            @Override
            public void onFailure() {

            }
        });
```

Congratulations! You have successfully started the flow. Carry on reading the next sections to learn how to:

- Customise the SDK
- Create checks

## Customising SDK

### 1. Capture type customisation

You can customise the capture type of the SDK via the `withCaptureType(Int)` method. You can do it in two different ways: Capturing user face by taking photo or a live video by using the front camera.

``` java

final Ondato config = OndatoConfig.builder()
    .withCaptureType(OndatoConfig.CAPTURE_TYPE_VIDEO) // OndatoConfig.CAPTURE_TYPE_PHOTO
    .loginWith("username","password")
    .build();

```

### Document Capture Step
In this step the user can pick which type of document to capture, and then use the phone camera to capture it.

You can also specify a particular document type that the user is allowed to upload by replacing this step with a `CaptureScreenStep` containing the desired type:

``` java

OndatoConfig.DOCUMENT_TYPE_ID_CARD
OndatoConfig.DOCUMENT_TYPE_PASSPORT

final Ondato config = OndatoConfig.builder()
    .withDocumentTypes(OndatoConfig.DOCUMENT_TYPE_PASSPORT, OndatoConfig.DOCUMENT_TYPE_ID_CARD)
    .loginWith("username","password")
    .build();

```

This way, the document type and country selection screens will not be visible prior to capturing the document.

#### Splash Screen Step
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

### 3. Localisation

Ondato Android SDK already comes with out-of-the-box translations for the following locales:
- English (en) :uk:
- Lithuanian (lt) :lt:
