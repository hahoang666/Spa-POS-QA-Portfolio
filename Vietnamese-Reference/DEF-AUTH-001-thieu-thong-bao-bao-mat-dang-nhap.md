# DEF-AUTH-001 — Thiếu thông báo bảo mật trên màn hình đăng nhập

| Trường | Nội dung |
|---|---|
| Dự án | Spa POS |
| Phân hệ | Đăng nhập |
| Yêu cầu | AUTH-05 — Phiên đăng nhập |
| Ca kiểm thử | AUTH-UI-05 |
| Loại lỗi | Giao diện / Không đáp ứng yêu cầu |
| Mức độ nghiêm trọng | Nhẹ (đề xuất) |
| Độ ưu tiên | Trung bình (đề xuất) |
| Trạng thái | CHƯA SỬA |
| Ngày ghi nhận | 13/10/2026 |
| Môi trường | Spa POS bản web; chưa ghi trình duyệt và phiên bản |
| Bằng chứng | Kết quả quan sát do người kiểm thử báo cáo; chưa có ảnh chụp mới |
| Kiểm tra lại | CHƯA THỰC HIỆN |

## Các bước tái hiện
1. Mở Spa POS ở trạng thái chưa đăng nhập.
2. Quan sát toàn bộ màn hình đăng nhập.
3. Tìm thông báo bảo mật theo yêu cầu AUTH-05.

## Kết quả mong đợi
Hiển thị đúng thông báo: **“Không lưu thông tin đăng nhập trên thiết bị”**.

## Kết quả thực tế
Không thấy thông báo bảo mật trên màn hình đăng nhập. Theo người kiểm thử, các thành phần và chức năng đăng nhập vẫn hoạt động bình thường.

## Ảnh hưởng
Giao diện chưa đáp ứng một yêu cầu hiển thị đã được phê duyệt. **Chưa có bằng chứng** cho thấy mật khẩu hoặc phiên đăng nhập bị lưu không an toàn.

## Đề xuất xử lý
Bổ sung thông báo vào màn hình đăng nhập và xác minh khả năng đọc ở điện thoại, máy tính bảng, máy tính. Làm rõ ý nghĩa thông báo với chức năng Ghi nhớ tên đăng nhập và khôi phục phiên để tránh nội dung gây hiểu nhầm.

## Tiêu chí kiểm tra lại
Thông báo hiển thị và đọc được tại 375×667, 768×1024, 1366×768; đăng nhập và Ghi nhớ tên đăng nhập không bị ảnh hưởng. Bổ sung ảnh chụp và mã phiên bản.
