# HTML5 Scanner & Kiosk System for Power Apps - Project Summary & Status

**Repository:** https://github.com/CloudHoang/HTML5-Scanner  
**Live URL:** https://cloudhoang.github.io/HTML5-Scanner/  
**Phiên bản phát triển:** `v2.0 - Kiosk Pro`  

---

## 📌 Tổng Quan Hệ Thống & Tệp Tin

1. **[`kiosk-v2.html`](file:///e:/Claude/HTML5-Scanner/kiosk-v2.html) (Flagship Kiosk Application):**
   - Tích hợp **Bộ chọn chế độ nạp mã (Input Mode Selector)**: `📷 Camera` / `🔫 Máy bắn QR` / `⚡ Cả hai`.
   - Tích hợp điều hướng **3 Tab tương tác trực tiếp** trên thanh công cụ (`1. Hướng dẫn` ➔ `2. Đang in (5s)` ➔ `3. Cảm ơn (5s)`).
   - Hỗ trợ toàn diện **Máy bắn mã vạch / QR Code cầm tay (USB & Bluetooth)** với cơ chế bắt phím toàn cục và bộ đệm chờ 1 giây (1000ms) inactive.
   - Hỗ trợ **Xác thực / Đăng nhập Microsoft Power Apps lần đầu** (qua tương tác trực tiếp Tab 2 hoặc mở Tab mới vượt chặn MFA/CORS).
   - Tích hợp cơ chế in Kiosk tự động `triggerKioskPrint()` lắng nghe sự kiện `window.addEventListener('afterprint')`.
   - Bộ tổng hợp âm thanh Web Audio API (BEEP công nghiệp + Chime chúc mừng), Lịch sử quét xuất CSV, quản lý đa Camera & Đèn Flash.

2. **[`index.html`](file:///e:/Claude/HTML5-Scanner/index.html) (Bản gốc Split-screen):**
   - Phiên bản chia màn hình 70% (Power Apps) / 30% (Camera scanner).

---

## 🎯 Tiến Độ & Đánh Giá Tính Năng (Checklist)

### ✅ 1. Tính năng ĐÃ HOÀN THÀNH (Completed)
- [x] **Tùy chọn Chế độ nạp mã (Input Mode Selector):**
  - Chuyển đổi 1-chạm: `📷 Camera` / `🔫 Máy bắn` / `⚡ Cả hai`.
  - Khi chọn `🔫 Chỉ Máy bắn`: Tự động tắt luồng camera (tiết kiệm CPU/pin) và hiển thị đồ họa chuyên dụng.
  - Lưu cấu hình mặc định trong `⚙️ Cài đặt`.
- [x] **Điều hướng 3 Tab trực tiếp:**
  - `Tab 1: 📖 1. Hướng dẫn`: Màn hình hướng dẫn quét, tự động bật camera và bắt focus.
  - `Tab 2: ⚡ 2. Đang in (5s)`: Hiển thị Iframe Power Apps. Bấm thủ công để đăng nhập tài khoản / kiểm tra app; tự động kích hoạt khi quét mã (đếm ngược 5s + gọi in).
  - `Tab 3: 🎉 3. Cảm ơn (5s)`: Trạng thái `DISPATCHED_TO_SPOOLER`, đếm ngược 5s và tự động quay lại Tab 1.
- [x] **Hỗ trợ Máy bắn QR Code / Barcode Gun:**
  - Bắt tín hiệu phím HID toàn cục (`window.addEventListener('keydown', ..., true)`), hoạt động ngay cả khi user click mất focus.
  - Bộ đệm chờ 1 giây Inactive (1000ms debounce) để gom trọn vẹn 100% dữ liệu mã QR dài trước khi chuyển tab.
  - Ô nhập liệu chuyên dụng có Auto-Refocus khi nhấp chuột ở Tab 1.
- [x] **Xác thực Power Apps lần đầu (Authentication Setup):**
  - Tương tác trực tiếp Iframe ở Tab 2.
  - Nút `🌐 Mở Tab mới (Đăng nhập MFA)` hỗ trợ đăng nhập tài khoản Microsoft 365 khi Iframe bị chặn `X-Frame-Options`.
- [x] **Quy trình In Kiosk (Spooler Printing):**
  - Hàm `triggerKioskPrint()` kích hoạt `window.print()` và bắt sự kiện `window.afterprint`.
  - Nhận diện `postMessage` xác nhận in từ Power Apps.
- [x] **Âm thanh & Phản hồi (Audio & Haptics):**
  - Web Audio API synthesizer: Tiếng BEEP quét mã và Chime chúc mừng (không cần file mp3 ngoài).
  - Rung phản hồi (Haptic Vibration).
- [x] **Quản lý Lịch sử & Dữ liệu:**
  - Lưu trữ `localStorage` lên đến 500 mã.
  - Tìm kiếm, Copy, Gửi lại mã vào Power Apps, và Xuất file **CSV**.
- [x] **Cài đặt & Tùy biến (Settings Modal):**
  - Cấu hình Chế độ nạp mã mặc định, URL Power Apps, thời gian chờ máy bắn mã (ms), thời gian đếm ngược đang in / cảm ơn / tạm dừng camera.

---

## 🔧 Technical Stack

| Hạng mục | Công nghệ / Thư viện | Trạng thái |
|---|---|---|
| Core Engine | HTML5 + Vanilla JS + CSS3 | ✅ Hoàn thành |
| Barcode Engine | `html5-qrcode` v2.3.8 | ✅ Hoạt động ổn định |
| Input Modes | Camera Only / Gun Only / Dual Mode | ✅ Hoạt động linh hoạt |
| HID Scanner | Global Keystroke Interceptor + 1s Inactive Debounce | ✅ Hoạt động ổn định |
| Audio Engine | Web Audio API Oscillator/Gain Synthesizer | ✅ Hoạt động tốt |
| Power Apps Iframe | Deep-link Parameter (`?barcode=...&timestamp=...`) + postMessage | ✅ Hoạt động tốt |
| Printing | `window.print()` + `afterprint` Spooler Listener | ✅ Hoạt động tốt |
| Data Export | Client-side CSV Exporter | ✅ Hoạt động tốt |

---

**Cập nhật lần cuối:** 18/08/2026  
**Chủ sở hữu:** CloudHoang
