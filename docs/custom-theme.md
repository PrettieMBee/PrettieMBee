# Tạo theme tuỳ chỉnh cho PrettieMBee

Hướng dẫn này mô tả cách lấy tài nguyên gốc của MB Bank, chỉnh sửa ảnh và bảng màu, rồi đóng gói thành file `.zip` để nạp trực tiếp trong PrettieMBee.

## 1. Cấu trúc gói ZIP

Một gói theme hợp lệ cần có thư mục `images/` (chứa ít nhất một tệp `.png`) và tệp `theme/token.json`:

```text
my_theme.zip
├── images/
│   ├── login_bg_main.png
│   ├── homeLanding_bg_main.png
│   └── ...
└── theme/
    ├── token.json
    └── token_priority.json   # tuỳ chọn, dùng khi bật chế độ Priority
```

Quy tắc khi đóng gói:

- Nên đặt `images/` và `theme/` trực tiếp ở thư mục gốc của file `.zip`.
- PrettieMBee cũng tự xử lý nếu hai thư mục này nằm trong một thư mục con cấp 1 (ví dụ `my_theme/images/` và `my_theme/theme/`) hoặc bên trong tệp `.bin` nén kiểu module Magisk cũ.
- Không dùng đường dẫn chứa `..` hoặc bắt đầu bằng `/` trong entry của file ZIP.
- Tên file `.zip` sẽ được dùng làm tên hiển thị khi nhập thủ công (bỏ đuôi `.zip`, đổi dấu `_` thành khoảng trắng).

## 2. Lấy bộ tài nguyên mẫu

Cách nhanh nhất để có đúng danh sách tệp ảnh và cấu trúc `token.json` mà MB Bank đang sử dụng:

### Cách A: Lấy trực tiếp từ MB Bank trên máy đã root

1. Mở MB Bank và tải một theme bất kỳ từ MB Store.
2. Sao chép thư mục theme vừa tải ra bộ nhớ ngoài để chỉnh sửa trên máy tính:

```bash
adb shell "su -c 'ls -1 /data/data/com.mbmobile/app_flutter/app_theme/unzip'"
adb shell "su -c 'cp -r /data/data/com.mbmobile/app_flutter/app_theme/unzip/<UUID> /sdcard/mb_base_theme'"
adb pull /sdcard/mb_base_theme ./my-theme
```

Nếu dùng Dual App hoặc bản clone, thay `com.mbmobile` bằng tên gói tương ứng.

### Cách B: Dùng một gói mẫu có sẵn

Tải một file `.zip` bất kỳ trong nhánh [`theme`](https://github.com/PrettieMBee/PrettieMBee/tree/theme/themes) hoặc từ release [`v1.0.0-themes`](https://github.com/PrettieMBee/PrettieMBee/releases/tag/v1.0.0-themes), giải nén rồi thay thế ảnh và màu theo ý muốn.

## 3. Chỉnh sửa hình ảnh (`images/`)

- Giữ nguyên tên tệp gốc và định dạng `.png`.
- Giữ kích thước hoặc tỷ lệ khung hình gần với ảnh gốc để tránh bị cắt lệch trên các độ phân giải màn hình khác nhau.
- Nên tối ưu dung lượng PNG (bằng `pngquant`, `oxipng` hoặc công cụ tương đương) để gói ZIP nhẹ và thời gian nạp nhanh hơn.

Một số ảnh thường thay đầu tiên:

| Tệp ảnh | Vị trí hiển thị |
|---|---|
| `login_bg_main.png` | Nền màn hình đăng nhập MB Bank; PrettieMBee cũng dùng ảnh này làm ảnh xem trước trên Status Card |
| `homeLanding_bg_main.png` | Nền trang chủ sau khi đăng nhập |
| `loading_bg_main.png` | Nền màn hình chờ |
| `general_bg_success.png` | Nền màn hình thông báo thành công |
| `coreBanking_img_successBackground.png` | Nền màn hình giao dịch thành công |

Khi nạp, PrettieMBee sẽ xoá và thay toàn bộ thư mục `images/` của slot theme đích bằng thư mục `images/` trong gói ZIP. Vì vậy, bạn nên giữ đủ bộ ảnh gốc và chỉ ghi đè những ảnh muốn tuỳ biến.

## 4. Chỉnh sửa bảng màu (`theme/token.json`)

Tệp `theme/token.json` chứa các nhóm design token cho giao diện Flutter của MB Bank, bao gồm các nhóm chính:

- `global`: bảng màu nền tảng (neutral, gray, brand...) được các token khác tham chiếu.
- `surface_neutral`, `surface_interation`, `surface_semantic`, `surface_highlight`, `background`: màu nền thẻ, nút và màn hình.
- `text_neutral`, `text_on`, `text_inverse`, `text_dynamic`, `text_semantic`: màu chữ.
- `icon_neutral`, `icon_on`, `icon_dynamic`, `icon_semantic`: màu biểu tượng.
- `border_neutral`, `border_interation`, `border_dynamic`: màu viền.
- `navigation_top_background`, `login`, `home`: màu riêng cho từng khu vực màn hình.

Giá trị màu trong `token.json` thường có ba dạng:

```json
"main": {
  "$type": "color",
  "$value": "{global.neutral.black}"
},
"secondary": {
  "$type": "color",
  "$value": "#00000000"
},
"primary": {
  "value": "linear-gradient(90deg, #252791 0%, #252791 100%)",
  "type": "color"
}
```

Lưu ý khi sửa `token.json`:

- Kiểm tra JSON hợp lệ sau khi chỉnh sửa (không để dấu phẩy thừa ở phần tử cuối, không sai ngoặc):

```bash
python3 -m json.tool my-theme/theme/token.json > /dev/null
```

- Nếu muốn hỗ trợ riêng giao diện tài khoản MB Priority, tạo thêm tệp `theme/token_priority.json` với cùng cấu trúc. PrettieMBee sẽ tự nhận diện tệp này và bật tuỳ chọn Priority khi áp dụng theme.

## 5. Đóng gói và kiểm thử

Nén từ bên trong thư mục theme để `images/` và `theme/` nằm ngay ở gốc file ZIP:

```bash
cd my-theme
zip -r ../My_Custom_Theme.zip images theme
```

Kiểm tra nhanh cấu trúc bên trong file ZIP:

```bash
unzip -l ../My_Custom_Theme.zip | head -n 20
```

Sau đó chép file `.zip` vào điện thoại, mở PrettieMBee, vào tab **Kho theme**, bấm **Nạp ZIP** để nhập và áp dụng thử vào một slot theme đích của MB Bank.

Nếu quá trình nạp báo lỗi cấu trúc ZIP, kiểm tra lại hai điều kiện:

1. `theme/token.json` phải tồn tại và là tệp hợp lệ (không rỗng, đúng tên viết thường).
2. `images/` phải tồn tại và chứa ít nhất một tệp `.png`.
