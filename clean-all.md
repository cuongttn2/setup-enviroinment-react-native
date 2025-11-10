
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

echo "🧹 Bắt đầu dọn sạch cache/build cho Android và iOS..."

# Đóng Metro/packager nếu đang chạy
echo "📦 Đóng Metro packager..."
killall node || true

# Xóa node_modules, cache Yarn, lockfile npm (nếu có)
echo "🗑️  Xóa node_modules và cache Yarn..."
rm -rf node_modules
yarn cache clean
rm -f package-lock.json

# Xóa cache Metro/Watchman (nếu dùng)
echo "🔄 Xóa cache Metro và Watchman..."
watchman watch-del-all || true
rm -rf $TMPDIR/metro-* $TMPDIR/haste-map-*

# Xóa build Android & cache Gradle
echo "🤖 Dọn sạch build Android và cache Gradle..."
cd android
# Không chạy gradlew clean vì node_modules chưa có, chỉ xóa thư mục build
rm -rf .gradle app/build build
cd ..
rm -rf ~/.gradle/caches

# Xóa Pods, lockfile, DerivedData iOS
echo "🍎 Dọn sạch Pods, Podfile.lock và DerivedData iOS..."
cd ios
rm -rf Pods Podfile.lock
pod deintegrate || true
pod cache clean --all || true
cd ..
rm -rf ~/Library/Developer/Xcode/DerivedData

# Cài lại dependencies & patch lại node_modules
echo "📥 Cài lại dependencies và áp dụng patches..."
yarn install

# Cài lại pods iOS
echo "🔧 Cài lại CocoaPods cho iOS..."
cd ios && pod install --repo-update && cd ..

echo ""
echo "✅ Hoàn tất! Đã dọn sạch cache/build cho cả Android và iOS."
echo ""
echo "📱 Bây giờ bạn có thể build lại:"
echo "   - Android: yarn android"
echo "   - iOS: yarn ios"
echo ""
```

**B2**: Cấp quyền thực thi:

```
chmod +x clean-all.sh
```

**B3**: Chạy script:

```
./clean-all.sh
```

