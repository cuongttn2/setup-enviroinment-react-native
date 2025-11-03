# setup-enviroinment-react-native
setup for new react-native project

# React Native Setup Note (macOS)

## 1. Biến môi trường Android SDK và Java

Thêm vào file `~/.zshrc`:

```bash
# Android SDK
export ANDROID_SDK_ROOT=~/Library/Android/sdk
export PATH=$ANDROID_SDK_ROOT/platform-tools:$PATH
export PATH=$ANDROID_SDK_ROOT/emulator:$PATH
export PATH=$ANDROID_SDK_ROOT/tools:$PATH

# Java 17
export JAVA_HOME=$(/usr/libexec/java_home -v 17)
export PATH=$JAVA_HOME/bin:$PATH
```

Áp dụng ngay:

```bash
source ~/.zshrc
```

Kiểm tra:

```bash
echo $ANDROID_SDK_ROOT
java -version
```

## 2. Cài NDK

* React Native cần NDK version **27.1.12297006**.
* Cài qua command line SDK manager:

```bash
sdkmanager "ndk;27.1.12297006"
```

* Kiểm tra thư mục NDK:

```bash
ls $ANDROID_SDK_ROOT/ndk/27.1.12297006
```

## 3. Java

* React Native + Gradle 9.0.0 yêu cầu **Java 17**.
* Tránh dùng Java 24 (mặc định macOS mới có thể tự chọn).

## 4. React Native Build Commands

* Khởi động dev server:

```bash
npx react-native start
```

* Chạy trên Android:

```bash
npx react-native run-android
```

* Xoá cache metro nếu gặp lỗi module:

```bash
rm -rf /tmp/metro-*
```

* Xoá Gradle cache khi gặp lỗi:

```bash
cd android
./gradlew clean
```

## 5. Package dependencies

Trong `package.json`, cần các phiên bản khớp:

```json
"dependencies": {
  "react": "19.1.1",
  "react-native": "0.82.1",
  "react-native-safe-area-context": "^5.5.2"
},
"devDependencies": {
  "@babel/runtime": "^7.25.0",
  "@babel/core": "^7.25.2",
  "@react-native-community/cli": "20.0.0",
  "@react-native/babel-preset": "0.82.1",
  ...
}
```

> Lưu ý: Nếu gặp lỗi `@babel/runtime/helpers/... could not be found`, chạy:

```bash
npm install
rm -rf /tmp/metro-*
npx react-native start --reset-cache
```

## 6. Lưu ý khác

* Tránh để Android Studio tự động chọn NDK khác với project yêu cầu.
* Luôn chắc chắn Java 17 được dùng khi chạy `./gradlew`.
* Kiểm tra biến môi trường bằng:

```bash
echo $JAVA_HOME
echo $ANDROID_SDK_ROOT
```

