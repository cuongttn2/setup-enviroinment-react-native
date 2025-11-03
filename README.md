# setup-enviroinment-react-native
setup for new react-native project

# React Native Android Environment Setup

## 1. Java (JVM)

React Native Android yêu cầu **Java 17**.

**Thiết lập mặc định cho terminal:**

```bash
# ~/.zshrc
export JAVA_HOME=$(/usr/libexec/java_home -v 17)
export PATH=$JAVA_HOME/bin:$PATH
```

Kiểm tra:

```bash
java -version
# Phải hiển thị Java 17.x.x
```

---

## 2. Android NDK

React Native 0.82.x yêu cầu **NDK 27.1.12297006**.

* Android Studio: `Preferences → System Settings → Android SDK → SDK Tools`
* Nếu không có, tải từ [NDK Older Releases](https://developer.android.com/ndk/downloads/older_releases)
* Cố định NDK trong `local.properties`:

```properties
ndk.dir=/Users/cuong/Library/Android/sdk/ndk/27.1.12297006
```

---

## 3. Gradle

* React Native 0.82.x tương thích **Gradle 9.x**
* Cố định Gradle Wrapper trong project:

```bash
cd android
./gradlew wrapper --gradle-version 9.0.0
```

---

## 4. Node.js

* Sử dụng Node >= 20
* Quản lý nhiều version Node bằng `nvm`:

```bash
nvm use 20
```

* Khi thay đổi Node hoặc cài lại deps:

```bash
rm -rf node_modules
rm package-lock.json
npm install
```

---

## 5. Xoá cache cũ

Để tránh lỗi Metro / Gradle:

```bash
rm -rf ~/.gradle/caches/
rm -rf /tmp/metro-*
```

---

## 6. Chạy React Native dev server sạch

Nếu gặp lỗi Metro cache:

```bash
npx react-native start --reset-cache
```

---

## 7. Build & Run Android

```bash
cd android
./gradlew clean
cd ..
npx react-native run-android
```

---

> ✅ Lưu ý: Với setup này, môi trường Android cho React Native sẽ ổn định, không còn lỗi JVM version, Gradle hay NDK.
