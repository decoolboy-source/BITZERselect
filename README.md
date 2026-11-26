# BITZERselect — Chọn máy nén BITZER (PWA)

Ứng dụng web (PWA) chọn máy nén lạnh BITZER (HyperPAC) theo nhiệt độ ngưng tụ / bay hơi, tra theo model hoặc theo công suất lạnh. Chạy **offline** sau lần mở đầu tiên, có giỏ chọn
thiết bị và xuất phiếu yêu cầu báo giá (CSV / In-PDF / Word).

- Địa chỉ: https://decoolboy-source.github.io/BITZERselect/
- Hãng: **BITZER (HyperPAC)** — mỗi hãng là một app riêng (xem danh sách app trên
  https://decoolboy-source.github.io/cong-cu/).
- Tách ngày 2026-11-26 từ MCselect (app nhiều hãng, đã ngừng): giữ nguyên giao diện và
  cách tra, chỉ giữ dữ liệu của hãng này, trang chủ đi thẳng vào các dòng sản phẩm.

## Dữ liệu

`data.json`: 96 series, 2016 điểm vận hành; môi chất: R134A, R404A.
Tra đúng các mức nhiệt độ có trong dữ liệu nguồn (không nội suy).

Số liệu trích từ catalogue của hãng, chỉ để **tham khảo sơ bộ** — đối chiếu tài liệu gốc
hoặc nhà cung cấp trước khi chọn thiết bị cho dự án thực tế.

## Cấu trúc thư mục

```
index.html      Giao diện
app.js          Logic tra cứu, giỏ báo giá, xuất phiếu
data.json       Dữ liệu thiết bị của hãng — sửa/thêm model ở đây
data-io.js      Xuất/Nhập dữ liệu JSON (chuẩn chung các app)
nasa-ui.css     Giao diện CHUNG các app chọn thiết bị (giống hệt nhau giữa các app)
icons.js        Bộ icon CHUNG (trích từ Lucide, chạy offline)
style.css       Phần riêng của app
manifest.json   Khai báo PWA
sw.js           Service worker — cache offline
icon-*.png      Icon app
```

## Chạy thử trên máy

App là file tĩnh, không cần cài gói:

```
python -m http.server 8080
```

rồi mở http://localhost:8080/ (không mở bằng `file://` vì service worker không chạy).

## Triển khai

Repo GitHub `decoolboy-source/BITZERselect`, GitHub Pages từ nhánh `main`, thư mục gốc.
App chạy tại `/BITZERselect/` chung origin với site chính và các app khác, nên:

- Service worker đăng ký tương đối `./sw.js`, scope `./`.
- Cache tên `BITZERselect-v<số>`; khi kích hoạt chỉ xoá cache có tiền tố này.
- Khoá localStorage có tiền tố `BITZERselect:`.
- File xuất dữ liệu `BITZERselect_YYYYMMDD.json` (`app: "BITZERselect"`, `schemaVersion: 1`).

Cập nhật: sửa code/dữ liệu, tăng `CACHE_NAME` trong `sw.js` và `BUILD_VERSION` trong
`app.js`, push lên `main`. Dòng "build ..." cuối trang cho biết bản đang chạy.

## Lịch sử phiên bản

### 2026-11-26.1 — bản đầu `BITZERselect-v1`

Tách từ MCselect: chỉ dữ liệu BITZER (HyperPAC), ẩn bước chọn hãng, đánh số lại các bước 1–5; khoá lưu trữ, cache,
file xuất dữ liệu dùng tiền tố `BITZERselect`.
