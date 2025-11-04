# IOS .app

```
xcodebuild \
  -workspace MyApp.xcworkspace \
  -scheme MyApp \
  -configuration Release \
  -sdk iphonesimulator \
  -derivedDataPath build \
  CODE_SIGN_IDENTITY="" CODE_SIGNING_REQUIRED=NO CODE_SIGNING_ALLOWED=NO
```

# Android Apk

```
 ./gradlew assembleDebug
 ./gradlew assembleRelease
```
