# HTML5 Scanner for Power Apps - Project Summary & Status

**Project Date:** July 22, 2026  
**Repository:** https://github.com/CloudHoang/HTML5-Scanner  
**Live URL:** https://cloudhoang.github.io/HTML5-Scanner/  
**Current Stable Git Commit:** `6654866` *(Use `git reset --hard 6654866` to restore)*

---

## 📌 Current Stable Configuration (`6654866`)

- **Bố cục giao diện Kiosk:** Power Apps nằm bên TRÁI (70% màn hình), Máy quét Camera nằm bên PHẢI (30% màn hình).
- **Quy trình truyền dữ liệu (Solution 2):** Cập nhật URL Iframe kèm tham số `?barcode=[VALUE]&timestamp=[TIME]` để ứng dụng Power Apps đọc qua hàm `Param("barcode")`. Tự động copy vào Clipboard và gửi postMessage làm phương án dự phòng.
- **Quy trình tạm dừng quét:** Tạm dừng máy quét 30 giây sau mỗi lần quét thành công, hiển thị đồng hồ đếm ngược trực quan trên thanh status (`⏸️ PAUSED (29s remaining...)`) và hỗ trợ click vào status để tiếp tục quét ngay lập tức.
- **Công cụ Debug:** Tích hợp Debug Console với nút bật/tắt (🐛).

---

## 🎯 Đánh giá theo Kế hoạch (Plan vs Achievements)

### ✅ 1. Những gì ĐÃ ĐẠT ĐƯỢC (Completed & Working Features)
1. **Bộ giải mã QR/Mã vạch HTML5 chuẩn xác:**
   - Sử dụng thư viện `html5-qrcode` (v2.3.8) với khung vuông tỷ lệ 1:1, tự động nhận diện camera sau (`facingMode: "environment"`).
   - Nhận diện đa dạng định dạng: QR Code, Code 128, Code 39, EAN-13, PDF-417, Data Matrix...
2. **Mô hình Reverse Embedding cho Kiosk:**
   - Nhúng ứng dụng Power Apps trực tiếp qua Iframe song song với máy quét camera.
3. **Tích hợp truyền dữ liệu vào Power Apps (Solution 2):**
   - Đẩy mã QR quét được vào Power Apps qua URL parameter `?barcode=...` thành công.
4. **Cơ chế Tạm dừng (Pause) & Đếm ngược 30 giây:**
   - Tránh việc quét trùng lặp mã QR, loại bỏ hộp thoại `alert()` gây hoãn tiến trình và có đồng hồ đếm ngược trực quan.
5. **Bộ công cụ kiểm thử & Debug Console:**
   - Log toàn bộ lịch sử khởi tạo camera, mã quét và lỗi thời gian thực.
6. **Triển khai tự động (CI/CD):**
   - Tự động build và deploy công khai thành công trên GitHub Pages.

---

### ⚠️ 2. Những gì CÒN THIẾU / HẠN CHẾ (Remaining & Known Limitations)

1. **Truyền giá trị không Reload (Typing Simulation):**
   - **Thực trạng:** Chưa thể giả lập phím gõ trực tiếp vào ô input của Power Apps mà không làm nạp lại trang Iframe.
   - **Nguyên nhân:** Do chính sách bảo mật trình duyệt (Same-Origin Policy) cấm Javascript bên ngoài bắn sự kiện phím vào Iframe tên miền `apps.powerapps.com`, đồng thời Power Apps không có bộ lắng nghe `postMessage` mặc định.
   - **Hướng xử lý tương lai:** Người lập trình Power Apps cần nhúng một Custom PCF Component bên trong Power Apps để đăng ký bộ lắng nghe `window.addEventListener('message', ...)`.

2. **Trải nghiệm Reload Iframe (Độ trễ 2-5s):**
   - **Thực trạng:** Khi truyền mã QR qua URL `Param("barcode")`, Power Apps phải nạp lại trang, xuất hiện vòng xoay Loading và reset lại trạng thái màn hình hiện tại.

3. **Chế độ Ẩn Camera/Log hoàn toàn:**
   - **Thực trạng:** `html5-qrcode` bắt buộc khung xem trước phải hiển thị cố định trong DOM để duy trì luồng giải mã 15-20fps; việc ẩn bằng `display: none` làm sập kích thước canvas nhận diện.

4. **Các tính năng nâng cao chưa phát triển (Phase tiếp theo):**
   - 🔮 **Chế độ Offline:** Lưu trữ mã QR cục bộ bằng Service Worker.
   - 🔮 **Lịch sử quét (Scan History):** Lưu và xuất danh sách các mã đã quét ra CSV/Excel.
   - 🔮 **Quản trị Kiosk (Admin Dashboard):** Cấu hình từ xa các thông số quét và URL Power Apps.

---

## 🔧 Technical Stack & Dependencies

| Hạng mục | Công nghệ / Thư viện | Trạng thái |
|---|---|---|
| Core Engine | HTML5 + Vanilla JS + CSS3 | ✅ Hoạt động tốt |
| Barcode Library | `html5-qrcode` v2.3.8 | ✅ Hoạt động tốt |
| Embedded App | Microsoft Power Apps (Iframe) | ✅ Đã nhúng |
| Transmission | Deep-link Parameter (`Param("barcode")`) | ✅ Đã tích hợp |
| Deployment | GitHub Pages (HTTPS) | ✅ Live |

---

**Last Updated:** July 22, 2026  
**Version Commit:** `6654866`  
**Owner:** CloudHoang
