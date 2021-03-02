# Mono repo with multiple clients CI test

## Setup for both apps

- Follow [instructions](https://flutter.dev/docs/deployment/android) for building android app for release

- Make the following changes to `android/app/build.gradle` to use the CI environment variables

```gradle
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
} else {
    keystoreProperties.setProperty('storePassword', System.getenv('KEY_STORE_PASSWORD'));
    keystoreProperties.setProperty('keyPassword', System.getenv('KEY_PASSWORD'));
    keystoreProperties.setProperty('keyAlias', System.getenv('KEY_ALIAS'));
    keystoreProperties.setProperty('storeFile', System.getenv('KEY_PATH'));
}
```

## Save your secrets to Github

- Export your key.jks signing key to base64, then save the output with the name `ANDROID_SIGNING_KEY`

```bash
openssl base64 -A -in <location of the key store file, such as /Users/<user name>/key.jks>
```

- From the key.properties file save the storePassword, keyPassword and keyAlias as Github secrets. This example uses `ANDROID_KEY_STORE_PASSWORD`, `ANDROID_KEY_PASSWORD` and `ANDROID_KEY_ALIAS` as the Github secret names

```properties
storePassword=<password from previous step>
keyPassword=<password from previous step>
keyAlias=key
storeFile=<location of the key store file, such as /Users/<user name>/key.jks>
```
