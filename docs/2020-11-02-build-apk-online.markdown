**Build Android APK online with buddy.works**


The requirements are:

 - [ ] A Github Account
 - [ ] keytool 
     + `keytool` is used to create keystore to sign APK. install openjdk8 on your system to have `keytool`
     + `keytool` is also available on iSH app for iPadOS after we successfully installed `openjdk8`
 - [ ] A buddy.works account


1. Register a buddy.works account, I register it with my second Github account.

    + Since you have to allow buddy.works to access both private and public repositories in order to clone them to buddy to build, if you have many private repositories that are private to your company or your organization, it is advisable not to use such critical important account with third party services.

2. Create new project in Buddy.works and clone your Android app project from Git repository.
3. Add Piplines and **Choose Build Apk**

In this step, remember to choose a suitable SDK and Build Tool version. To know which value is suitable, you can check these variables in **your git Android project/app/build.grade**

    minSdkVersion
    targetSdkVersion
    compileSdkVersion
    buildToolsVersion

## Sign your APK

> Note:

> If you don't sign your APK app, sometimes the built APK will **not** be installable.

To sign an APK, you need to have a keystore. We can use `keytool` to generate a **keystore**

See: http://docs.phonegap.com/phonegap-build/signing/android/

    keytool -genkey -v -keystore [keystore_name].keystore -alias [alias_name] -keyalg RSA -keysize 2048 -validity 10000



Upload you `keystore` to **File System** on Buddy.works and provide your keystore *password*.


After uploaded your `keystore`, now back to **Actions** and add **Sign Apk action**.

Fill in what it asks.

## Run pipeline again

Now you can proceed to **Run pipline** and ignore all the other things.

The output will be located in **File System** 

```
app/build/outputs/apk/release/
```



