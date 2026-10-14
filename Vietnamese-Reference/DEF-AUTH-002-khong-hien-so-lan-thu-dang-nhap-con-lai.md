# DEF-AUTH-002 — Không hiển thị số lần thử đăng nhập còn lại

| Trường | Nội dung |
|---|---|
| Dự án | Spa POS |
| Phân hệ | Đăng nhập |
| Yêu cầu | AUTH-01 |
| Test case | AUTH-VAL-04 |
| Loại lỗi | Phản hồi validation / Không đáp ứng yêu cầu |
| Severity | Minor (đề xuất) |
| Priority | Medium (đề xuất) |
| Trạng thái | OPEN — Chưa sửa |
| Ngày | 14/10/2026 |
| Môi trường | Spa POS bản web; chưa ghi trình duyệt và phiên bản |
| Bằng chứng | Ảnh image(10).png do người kiểm thử cung cấp trong hội thoại, chưa đưa lên GitHub |
| Retest | CHƯA THỰC HIỆN |

## Các bước tái hiện
1. Mở màn hình đăng nhập ở trạng thái chưa đăng nhập.
2. Nhập tên đăng nhập thử `abc123` và mật khẩu thử không hợp lệ.
3. Nhấn Đăng nhập một lần.
4. Quan sát thông báo lỗi và số lần thử còn lại.

## Kết quả mong đợi
Từ chối đăng nhập sai; hiển thị thông báo chung không tiết lộ trường sai và hiển thị số lần thử đăng nhập còn lại theo AUTH-01.

## Kết quả thực tế
Hệ thống từ chối đăng nhập, hiển thị “Tên đăng nhập hoặc mật khẩu không đúng.” và viền đỏ ô Mật khẩu. **Không có thông tin số lần thử còn lại.**

## Ảnh hưởng
Thiếu phản hồi được quy định trong yêu cầu. Chưa có bằng chứng cho thấy bộ đếm hoặc cơ chế khóa tài khoản bị lỗi; kiểm thử riêng tại AUTH-VAL-07.

## Đề xuất sửa
Bổ sung thông tin số lần thử còn lại dựa trên trạng thái khóa/bộ đếm phía máy chủ; không làm lộ tài khoản có tồn tại hay không. Đối chiếu quy tắc AUTH-02.

## Tiêu chí retest
Thông báo lỗi chung và số lần thử còn lại hiển thị chính xác sau đăng nhập sai. Kiểm tra các lần sai tiếp theo và khóa tài khoản bằng tài khoản thử độc lập. Lưu ảnh và phiên bản.
