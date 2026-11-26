<img src="app/src/main/res/drawable/ic_prettiembee_logo.png" alt="PrettieMBee" width="72" />

# PrettieMBee

[![Build APK](https://github.com/premierere/PrettieMBee/actions/workflows/build.yml/badge.svg)](https://github.com/premierere/PrettieMBee/actions/workflows/build.yml)
[![License: GPL-3.0](https://img.shields.io/badge/License-GPL--3.0-blue.svg)](LICENSE)

Ứng dụng Android yêu cầu quyền root để nạp theme tuỳ chỉnh vào ứng dụng MB Bank (`com.mbmobile`).

## Tính năng

- **Ánh xạ theme đích linh hoạt:** nạp theme vào bất kỳ theme MB Bank nào đã tải sẵn trên máy hoặc chọn từ danh mục MB Store, không bị cố định vào một UUID duy nhất.
- **Kho theme và nhập ZIP thủ công:** tải theme cộng đồng từ GitHub hoặc nhập trực tiếp file `.zip` từ bộ nhớ máy (hỗ trợ cấu trúc `images/*.png` + `theme/token.json`, thư mục lồng nhau và gói `.bin` từ module Magisk).
- **Xử lý quyền và SELinux tự động:** tự lấy `UID:GID` của gói MB Bank, sao chép tài nguyên (có cơ chế fallback sang `tar` stream khi shell `cp` lỗi), chạy `chown -R`, `chmod` và `restorecon -R` trên thư mục `app_flutter`.
- **Biến thể Priority:** tự động dùng `theme/token_priority.json` nếu theme hỗ trợ và người dùng bật tuỳ chọn Priority.
- **Mở nhanh bằng deeplink:** sau khi nạp xong có thể tự mở MB Bank vào thẳng trang chi tiết theme đích qua `mbbank://installingnew` để bấm áp dụng.

## Yêu cầu

- Android 8.0 (API 26) trở lên
- Thiết bị đã root bằng Magisk, KernelSU hoặc APatch
- Đã cài đặt và mở MB Bank (`com.mbmobile`) ít nhất một lần

## Cách sử dụng

1. Cài đặt file APK từ mục [GitHub Actions](https://github.com/premierere/PrettieMBee/actions/workflows/build.yml) hoặc [Releases](https://github.com/premierere/PrettieMBee/releases).
2. Mở PrettieMBee và cấp quyền Superuser khi hệ thống hỏi.
3. Kiểm tra trạng thái root và MB Bank ở tab **Trang chủ** (nếu dùng Dual App hoặc bản clone, đổi tên gói mục tiêu trong tab **Cài đặt**).
4. Chuyển sang tab **Kho theme**, chọn một theme từ danh sách hoặc bấm **Nạp ZIP** để nhập gói từ máy.
5. Trong bảng chi tiết theme:
   - Giữ theme đích mặc định hoặc bấm **Đổi** để chọn vị trí theme MB Bank khác.
   - Bật **Chế độ Priority** nếu tài khoản là MB Priority và theme có hỗ trợ.
   - Bấm **Áp dụng theme** và chờ quá trình nạp hoàn tất.
6. Khi MB Bank mở trang chi tiết theme, bấm **Áp dụng** trong MB Bank để kích hoạt.

Để gỡ toàn bộ theme đã nạp và đưa MB Bank về giao diện gốc, vào **Cài đặt > Xoá toàn bộ theme MB Bank**.

## Cấu trúc gói ZIP tuỳ chỉnh

File `.zip` nhập thủ công cần chứa tối thiểu:

```text
theme.zip
├── images/
│   └── *.png
└── theme/
    ├── token.json
    └── token_priority.json   # tuỳ chọn
```

Ứng dụng cũng tự giải nén khi các thư mục trên nằm bên trong một thư mục con hoặc một tệp `.bin` nén kiểu module Magisk cũ.

## Biên dịch từ mã nguồn

Yêu cầu JDK 17 và Android SDK (compileSdk 34):

```bash
git clone https://github.com/premierere/PrettieMBee.git
cd PrettieMBee
chmod +x gradlew
./gradlew assembleDebug
```

File APK được tạo tại `app/build/outputs/apk/debug/app-debug.apk`. Ngoài ra, workflow [`.github/workflows/build.yml`](.github/workflows/build.yml) tự động build Debug APK trên mỗi lần push và đính kèm vào GitHub Release khi tạo tag `v*`.

## Lưu ý

PrettieMBee chỉ thay đổi ảnh và tệp cấu hình màu trong thư mục dữ liệu giao diện `app_flutter/app_theme` của MB Bank. Ứng dụng không chỉnh sửa mã nguồn, tệp `.dex` hay cơ chế xác thực của ứng dụng ngân hàng. Dự án mang tính cá nhân hoá giao diện phi thương mại; người dùng tự chịu trách nhiệm khi sử dụng trên thiết bị của mình.

## Giấy phép

Phát hành theo giấy phép [GPL-3.0](LICENSE).
