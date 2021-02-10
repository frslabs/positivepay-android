# POSITIVE-PAY ANDROID SDK
![version](https://img.shields.io/badge/version-v3.0.0-blue)

The Positive pay Android SDK is a real-time MICR code capture for bank cheques in Android.

Features available are
- MICR Capture
- Manual Crop
- Online and Offline MICR capture support

**Find the changelog and release history at [Here](CHANGELOG.md)**

# Table Of Content

- [Prerequisite](#prerequisite)
- [Android SDK Requirements](#android-sdk-requirements)
- [Download](#download)
  - [Using maven repository](#using-maven-repository)
- [Setup](#setup)
  - [Permissions](#permissions)
- [Quick Start](#quick-start)
  - [Invoking the Cropus SDK](#invoking-the-positivePay-sdk)
- [PositivePay SDK Result](#positivePay-sdk-result)
- [PositivePay SDK Error Codes](#positivePay-sdk-error-codes)
- [PositivePay SDK Parameters](#positivePay-sdk-parameters)
- [Help](#help)

## Prerequisite

You will need a valid license to use the Positive-pay Android SDK, which can be obtained by contacting `support@frslabs.com` . 

Once you have the license , follow the below instructions for a successful integration of Cropus Android SDK onto your Android Application.

## Android SDK Requirements

**Minimum SDK Version** -  **19**

**Compile SDK Version** - **29+**

## Download

#### Using maven repository

Add the following code to your `project` level `build.gradle` file

```groovy
allprojects { 
    repositories { 
       maven { 
            // Maven Url and Credentials for Cropus SDK. 
            url "https://positivepay-android.repo.frslabs.space/"                  
            credentials { 
                   username 'repo-username' 
                   password 'repo-password' 
            }
       }
        
    }
}
```

After that, add the following code to your `app` level `build.gradle` file

```groovy
// ...

  compileOptions {
      
       sourceCompatibility = 1.8
       targetCompatibility = 1.8
       
       //Add below line only if minSdkVersion is < 24 
       coreLibraryDesugaringEnabled true
       
  }

// ...
```

And then, add the dependencies
```groovy

// ...

dependencies {
    /* Standard AndroidX Library Dependencies */ 
    implementation 'com.google.android.material:material:1.3.0'
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.0.4'
   
    //REQUIRED - Cropus SDK Dependency
    implementation 'com.frslabs.android.sdk:positivepay:3.0.0'
    
    //OPTIONAL - only for offline version
    implementation 'com.google.android.gms:play-services-mlkit-text-recognition:16.1.1'
    implementation 'com.rmtheis:tess-two:9.1.0'
    
}
```

## Setup

#### Permissions

PositivePay SDK uses the required permissions and features
```xml

    <!--Permissions-->
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.VIBRATE" />
    
    <!--Features-->
    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />

```

## Quick Start

#### Invoking the PositivePay SDK

Initialize the `PositivePay` instance with the appropriate configurations. 
Call `start` on the instance to invoke the SDK.
Handle the result by extending the `PositivePayResultCallback`

```java
public class MainActivity extends AppCompatActivity implements PositivePayResultCallback {

    // ...

    /* Enter the PositivePay SDK license key here */
    private String PositivePay_LICENSE_KEY = "ENTER_YOUR_LICENSE_KEY_HERE";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        Button callSdk = findViewById(R.id.call_sdk);
        callSdk.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                /* Invoke the Cropus SDK */
                invokePosPaySDK();
            }
        });
    }

    private void invokePosPaySDK() {
    
         PositivePayConfig posPayConfig = new PositivePayConfig.Builder()
                .setLicenseKey("LICENCE_KEY_POSPAY_SDK")
                .setDocumentSide(Utility.Side.TWO)
                .setOcrFlag("API")
                .setApiCredentials(new PositivePayApiCredentials(POSPAY_API_BASE_URL
                        , POSPAY_API_REFERENCE_ID
                        , POSPAY_API_CRED1
                        , POSPAY_API_CRED2))
                .build();

        PositivePay posPay = new PositivePay(posPayConfig);
        posPay.start(this, this);
    }
    
    @Override
    public void onPosPaySuccess(final PositivePayResult positivePayResult) {
        //Handle the Cropus SDK Result Here
        Log.v("onCaptusSuccess...", "onScanSuccess: " + Arrays.toString(positivePayResult.getImageReferenceIds()));
        Log.v("onCaptusSuccess...", "onScanSuccess: " + Arrays.toString(positivePayResult.getImagePaths()));
    }

    @Override
    public void onPosPayFailure(int errorCode) {
        /* Handle the Cropus SDK error here */
        Toast.makeText(this, "Error: "+ errorCode, Toast.LENGTH_SHORT).show();
    }
    
    // ...

}
```
## PositivePay SDK Result

The result is obtained through the `PositivePayResult` object

Given below are the public methods in brief.
<div>
<table style="width:100%">
 <tr>
 <th bgcolor="#F1F1F1" colspan="3">Public Methods</th>
 </tr>
 <tr>
 <td>Uri</td>
 <td>getImagePath()</td>
 <td>Returns the Signature Image Path as Uri</td>
 </tr>
</table>
</div>

## PositivePay SDK Error Codes

Error codes and their meaning are tabulated below

| Label          | Code |Message                 |
| -------------- | ----- |---------------------- |
|ERROR_CODE_CAM_PREMISSION | 803 | Required permissions for PositivePay SDK were not granted |
|ERROR_CODE_INTERRUPTED | 804 | PositivePay SDK Interrupted |
|ERROR_CODE_EXPIRED_LICENSE | 805 | PositivePay SDK License has expired |
|ERROR_CODE_INVALID_LICENSE | 806 | Invalid PositivePay SDK License |
|ERROR_CODE_INVALID_CONFIG | 807 | Invalid PositivePay SDK Config |
|ERROR_CODE_FILE_IO | 809 | Error Saving File to Disk |

## Cropus SDK Parameters

- `setLicenseKey(String positivePayLicenseKey)`   ***(Required)***
  
  Accepts the Cropus SDK licence key as a `String`

## Help
For any queries/feedback , contact us at `support@frslabs.com` 
