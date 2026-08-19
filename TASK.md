# HTML5 Scanner & Kiosk System for Power Apps - Project Summary & Status

**Repository:** https://github.com/CloudHoang/HTML5-Scanner  
**Live URL:** https://cloudhoang.github.io/HTML5-Scanner/  
**Phiên bản phát triển:** `v2.1 - Smart Print Timeout & Error Handling`  

---

## 📌 Tổng Quan Hệ Thống & Tệp Tin

1. **[`index.html`](file:///e:/Claude/HTML5-Scanner/index.html) & [`kiosk-v2.html`](file:///e:/Claude/HTML5-Scanner/kiosk-v2.html):**
   - Tích hợp quy trình: **Đợi Iframe Power Apps load xong 100% ➔ Đếm ngược 10 giây chờ lệnh in**:
     - *Thành công:* Nhận lệnh in trong 10s ➔ Chuyển sang **Trang Cảm ơn (5s)** ➔ Tự động quay về Trang 1.
     - *Lỗi Timeout:* Hết 10s không có lệnh in ➔ Chuyển sang **Trang Báo Lỗi (10s)** ➔ Tự động quay về Trang 1.
   - Tích hợp **Bộ chọn chế độ nạp mã (Input Mode Selector)**: `📷 Camera` / `🔫 Máy bắn QR` / `⚡ Cả hai`.
   - Tích hợp điều hướng **3 Tab tương tác trực tiếp** trên thanh công cụ (`1. Hướng dẫn` ➔ `2. Đang in (10s)` ➔ `3. Cảm ơn`).
   - Hỗ trợ toàn diện **Máy bắn mã vạch / QR Code cầm tay (USB & Bluetooth)** với cơ chế bắt phím toàn cục và bộ đệm chờ 1 giây (1000ms) inactive.
   - Hỗ trợ **Xác thực / Đăng nhập Microsoft Power Apps lần đầu** (qua tương tác trực tiếp Tab 2 hoặc mở Tab mới vượt chặn MFA/CORS).
   - Bộ tổng hợp âm thanh Web Audio API (BEEP công nghiệp + Chime chúc mừng + Âm cảnh báo lỗi), Lịch sử quét xuất CSV, quản lý đa Camera & Đèn Flash.

---

## 🎯 Tiến Độ & Đánh Giá Tính Năng (Checklist)

### ✅ 1. Tính năng ĐÃ HOÀN THÀNH (Completed)
- [x] **Kiểm soát Lệnh In & Xử Lý Lỗi Thông Minh:**
  - Chờ Iframe Power Apps load xong hoàn toàn (`iframe.onload`).
  - Đếm ngược 10 giây chờ lệnh in (`window.afterprint` hoặc `postMessage`).
  - Chuyển sang Trang Cảm ơn khi nhận lệnh in thành công.
  - Chuyển sang Màn hình Báo Lỗi (`PRINT_TIMEOUT_NOT_DISPATCHED (10s)`) khi hết 10 giây không có lệnh in, đếm 10 giây và tự động reset về Trang đầu.
- [x] **Tùy chọn Chế độ nạp mã (Input Mode Selector):**
  - Chuyển đổi 1-chạm: `📷 Camera` / `🔫 Máy bắn` / `⚡ Cả hai`.
  - Khi chọn `🔫 Chỉ Máy bắn`: Tự động tắt luồng camera (tiết kiệm CPU/pin) và hiển thị đồ họa vector chuyên dụng.
- [x] **Điều hướng 3 Tab trực tiếp:**
  - `Tab 1: 📖 1. Hướng dẫn`: Màn hình hướng dẫn quét, tự động bật camera và bắt focus.
  - `Tab 2: ⚡ 2. Đang in (10s)`: Hiển thị Iframe Power Apps và đếm ngược chờ lệnh in.
  - `Tab 3: 🎉 3. Cảm ơn`: Trạng thái `DISPATCHED_TO_SPOOLER` sau khi in thành công.
- [x] **Hỗ trợ Máy bắn QR Code / Barcode Gun:**
  - Bắt tín hiệu phím HID toàn cục (`window.addEventListener('keydown', ..., true)`).
  - Bộ đệm chờ 1 giây Inactive (1000ms debounce) để gom trọn vẹn 100% dữ liệu mã QR dài trước khi nạp vào Power Apps.
- [x] **Xác thực Power Apps lần đầu (Authentication Setup):**
  - Tương tác trực tiếp Iframe ở Tab 2.
  - Nút `🌐 Mở Tab mới (Đăng nhập MFA)` hỗ trợ đăng nhập tài khoản Microsoft 365 khi Iframe bị chặn `X-Frame-Options`.
- [x] **Âm thanh & Phản hồi (Audio & Haptics):**
  - Web Audio API synthesizer: Tiếng BEEP quét mã, Chime chúc mừng và Âm cảnh báo lỗi.
- [x] **Quản lý Lịch sử & Dữ liệu:**
  - Lưu trữ `localStorage` lên đến 500 mã, tìm kiếm, copy và xuất file **CSV**.
- [x] **Cài đặt & Tùy biến (Settings Modal):**
  - Cấu hình Thời gian chờ lệnh in (10s), Thời gian hiển thị trang lỗi (10s), Chế độ nạp mã mặc định, URL Power Apps.

---

## 🔧 Technical Stack

| Hạng mục | Công nghệ / Thư viện | Trạng thái |
|---|---|---|
| Core Engine | HTML5 + Vanilla JS + CSS3 | ✅ Hoàn thành |
| Barcode Engine | `html5-qrcode` v2.3.8 | ✅ Hoạt động ổn định |
| Print Timeout Flow | `iframe.onload` ➔ 10s Wait Timer ➔ Success / Error Screen (10s) | ✅ Hoạt động chính xác |
| Input Modes | Camera Only / Gun Only / Dual Mode | ✅ Hoạt động linh hoạt |
| HID Scanner | Global Keystroke Interceptor + 1s Inactive Debounce | ✅ Hoạt động ổn định |
| Audio Engine | Web Audio API Oscillator/Gain Synthesizer (Beep/Chime/Error) | ✅ Hoạt động tốt |
| Data Export | Client-side CSV Exporter | ✅ Hoạt động tốt |

---

**Cập nhật lần cuối:** 18/08/2026  
**Chủ sở hữu:** CloudHoang
