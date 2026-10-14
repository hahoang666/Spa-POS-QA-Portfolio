# DEF-AUTH-004 — Mất kết nối nhưng thông báo sai tài khoản hoặc mật khẩu

- **Yêu cầu:** AUTH-01
- **Test case:** AUTH-VAL-15
- **Phân hệ:** Đăng nhập / Xác thực
- **Severity:** Minor (đề xuất)
- **Priority:** Medium (đề xuất)
- **Trạng thái:** OPEN
- **Ngày kiểm thử:** 2026-10-14
- **Môi trường:** Spa POS web, Chrome DevTools Network Offline; chưa ghi nhận build

## Điều kiện tiên quyết
- Đang ở màn hình đăng nhập Spa POS.
- Có tài khoản thử nghiệm hợp lệ.
- DevTools đang mở, Network đặt thành **Offline**.

## Các bước tái hiện
1. Mở trang đăng nhập và đăng xuất nếu cần.
2. Mở DevTools → Network → chọn **Offline**, giữ DevTools mở.
3. Nhập tài khoản và mật khẩu hợp lệ.
4. Nhấn **Đăng nhập**, quan sát thông báo.
5. Nhấn lại **Đăng nhập**, kiểm tra trạng thái nút.

## Kết quả mong đợi
Hiển thị thông báo phù hợp khi không kết nối được máy chủ, không cho truy cập trái phép và nút Đăng nhập trở lại bình thường (giữ nguyên Expected Result gốc AUTH-VAL-15).

## Kết quả thực tế
Ứng dụng không cho truy cập Dashboard nhưng hiển thị **“Tên tài khoản hoặc mật khẩu không đúng”** dù thông tin hợp lệ và DevTools đang mô phỏng mất mạng. Nút Đăng nhập vẫn phản hồi khi nhấn lại.

## Ảnh hưởng
Người dùng có thể hiểu nhầm rằng thông tin đăng nhập sai, trong khi nguyên nhân là không thể kết nối máy chủ.

## Quan sát bổ sung
Ở lần thử khác, sau khi chọn Offline rồi đóng DevTools, người kiểm thử đăng nhập thành công. **Chưa xác minh chế độ Offline còn hiệu lực** khi đóng DevTools, nên không xem đây là bằng chứng vượt qua xác thực.

## Bằng chứng / Phạm vi
Kết quả kiểm thử thủ công do tester báo cáo; không thu thập mật khẩu hay token; không gây mất kết nối máy chủ thực tế.

## Đề xuất sửa lỗi
Phân biệt lỗi mạng/kết nối với lỗi sai thông tin đăng nhập; hiển thị thông báo phù hợp và giữ khả năng thử lại.

## Retest
Chạy lại AUTH-VAL-15 với DevTools mở và Network Offline; xác minh thông báo lỗi kết nối, không vào Dashboard và nút thử lại hoạt động; sau đó chọn No throttling.
