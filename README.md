<img src="app/src/main/res/drawable/ic_prettiembee_logo.png" alt="PrettieMBee" width="72" />

# PrettieMBee

[![Build APK](https://github.com/PrettieMBee/PrettieMBee/actions/workflows/build.yml/badge.svg)](https://github.com/PrettieMBee/PrettieMBee/actions/workflows/build.yml)

Công cụ Android yêu cầu quyền root để nạp theme tuỳ chỉnh cho MB Bank (`com.mbmobile`).

## Tải về

Tải bản build mới nhất tại mục [Releases (`canary`)](https://github.com/PrettieMBee/PrettieMBee/releases/tag/canary) hoặc lấy artifact từ [GitHub Actions](https://github.com/PrettieMBee/PrettieMBee/actions/workflows/build.yml).

## Tính năng

- Nạp theme vào bất kỳ slot theme nào đã tải sẵn trên máy hoặc chọn từ danh mục MB Store, không bị cố định vào một UUID.
- Tải theme cộng đồng từ GitHub (tự động fallback sang release `v1.0.0-themes` nếu không tải được từ nhánh `theme`) hoặc nhập trực tiếp file `.zip` từ bộ nhớ máy.
- Tự xác định `UID:GID` của MB Bank và đặt lại quyền truy cập (`chown`, `chmod`, `restorecon`) trên thư mục `app_flutter`.
- Hỗ trợ biến thể Priority (`theme/token_priority.json`) khi theme có sẵn và người dùng bật tuỳ chọn.
- Mở nhanh trang chi tiết theme trong MB Bank qua deeplink `mbbank://installingnew` sau khi nạp xong.

## Yêu cầu

- Android 8.0 (API 26) trở lên
- Thiết bị đã root bằng Magisk, KernelSU hoặc APatch
- Đã cài đặt và mở MB Bank (`com.mbmobile`) ít nhất một lần

## Tài liệu

- [Hướng dẫn tự tạo theme tuỳ chỉnh](docs/custom-theme.md)

## Biên dịch từ mã nguồn

Yêu cầu JDK 17 và Android SDK (compileSdk 34):

```bash
git clone https://github.com/PrettieMBee/PrettieMBee.git
cd PrettieMBee
chmod +x gradlew
./gradlew assembleDebug
```

File APK được tạo tại `app/build/outputs/apk/debug/app-debug.apk`. Workflow [`.github/workflows/build.yml`](.github/workflows/build.yml) tự động build APK khi push, cập nhật bản mới nhất từ `main` lên release `canary` và tạo release riêng khi đẩy tag `v*`.

## Tuyên bố miễn trừ trách nhiệm

PrettieMBee chỉ thay đổi ảnh và tệp cấu hình màu trong thư mục `app_flutter/app_theme` của MB Bank. Ứng dụng không chỉnh sửa mã nguồn, tệp `.dex` hay cơ chế xác thực của ngân hàng. Dự án phục vụ cá nhân hoá giao diện phi thương mại; người dùng tự chịu trách nhiệm khi sử dụng trên thiết bị của mình.

## Credits

- **Chủ repo & phát triển ứng dụng:** anhiutangerine ([premierere](https://github.com/premierere))
- **Kho theme gốc & cộng đồng:** [cuynu](https://cuynutt.ddns.net/mbcp/tweaked-mbflutter-themes) cùng cộng đồng trong và ngoài MBCP
- **Đóng góp theme:** cuynu, TsumugiShiina0, nguyencaoantuong, khoictm, Fukiame, minhwuangbeo, Meowice

## Giấy phép

Phát hành theo giấy phép [GPL-3.0](LICENSE).
