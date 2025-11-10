
# 1. Đóng Metro/packager nếu đang chạy
killall node

# 2. Xóa node_modules, cache Yarn, lockfile npm (nếu có)
rm -rf node_modules
yarn cache clean
rm -f package-lock.json

# 3. Xóa cache Metro/Watchman (nếu dùng)
watchman watch-del-all || true
rm -rf $TMPDIR/metro-* $TMPDIR/haste-map-*

# 4. Xóa build Android & cache Gradle
cd android
./gradlew clean
rm -rf .gradle app/build build
cd ..
rm -rf ~/.gradle

# 5. Xóa Pods, lockfile, DerivedData iOS
cd ios
rm -rf Pods Podfile.lock
pod deintegrate
pod cache clean --all
cd ..
rm -rf ~/Library/Developer/Xcode/DerivedData

# 6. Cài lại dependencies & patch lại node_modules
yarn install

# 7. Cài lại pods iOS
cd ios && pod install --repo-update && cd ..

# 8. Khởi động lại Metro với cache sạch (nếu cần)
yarn start --reset-cache

# 9. Tạo file clean-all.sh

**B1**: Tạo file clean-all.sh với nội dung sau để clean all.:

```
#!/bin/bash

# Đóng Metro/packager nếu đang chạy
killall node || true

# Xóa node_modules, cache Yarn, lockfile npm (nếu có)
rm -rf node_modules
yarn cache clean
rm -f package-lock.json

# Xóa cache Metro/Watchman (nếu dùng)
watchman watch-del-all || true
rm -rf $TMPDIR/metro-* $TMPDIR/haste-map-*

# Xóa build Android & cache Gradle
cd android
./gradlew clean
rm -rf .gradle app/build build
cd ..
rm -rf ~/.gradle

# Xóa Pods, lockfile, DerivedData iOS
cd ios
rm -rf Pods Podfile.lock
pod deintegrate
pod cache clean --all
cd ..
rm -rf ~/Library/Developer/Xcode/DerivedData

# Cài lại dependencies & patch lại node_modules
yarn install

# Cài lại pods iOS
cd ios && pod install --repo-update && cd ..

echo "✅ Đã dọn sạch cache/build cho cả Android và iOS!"
```

**B2**: Cấp quyền thực thi:

```
chmod +x clean-all.sh
```

**B3**: Chạy script:

```
./clean-all.sh
```

