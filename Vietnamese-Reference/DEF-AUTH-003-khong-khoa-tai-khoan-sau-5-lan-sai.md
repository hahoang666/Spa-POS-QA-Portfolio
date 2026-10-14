# DEF-AUTH-003 — Không khóa tài khoản sau 5 lần đăng nhập sai

| Trường | Nội dung |
|---|---|
| Dự án | Spa POS |
| Phân hệ | Xác thực đăng nhập |
| Yêu cầu | AUTH-02 |
| Test case | AUTH-VAL-07 |
| Loại lỗi | Bảo mật / Kiểm soát xác thực |
| Severity | Critical (đề xuất) |
| Priority | High (đề xuất) |
| Trạng thái | OPEN — Chưa sửa |
| Ngày phát hiện | 14/10/2026 |
| Môi trường | Spa POS web; chưa ghi trình duyệt và build |
| Bằng chứng | Kết quả kiểm thử thủ công do người kiểm thử báo cáo; chưa có log backend/video |
| Retest | CHƯA THỰC HIỆN |

## Điều kiện tiên quyết
Tài khoản thử đang hoạt động, có mật khẩu đúng, chưa bị khóa, bộ đếm sai đã được đặt về 0. **Chưa có xác minh độc lập trạng thái bộ đếm ban đầu**.

## Các bước tái hiện
1. Mở màn hình đăng nhập ở trạng thái chưa đăng nhập.
2. Với cùng tài khoản thử, nhập mật khẩu sai liên tiếp 7 lần.
3. Quan sát thông báo khóa và thời gian khóa sau lần thứ 5.
4. Ở lần thứ 8, nhập mật khẩu đúng.

## Kết quả mong đợi
Sau 5 lần đăng nhập sai liên tiếp, tài khoản bị khóa tạm 15 phút. Trong thời gian khóa, mật khẩu đúng cũng không thể đăng nhập; giao diện thông báo trạng thái khóa và thời gian còn lại theo AUTH-02.

## Kết quả thực tế
Lần 1–7 chỉ hiển thị “Tên đăng nhập hoặc mật khẩu không đúng.”, không thông báo khóa và không hiển thị thời gian khóa. Lần 8 nhập mật khẩu đúng thì đăng nhập thành công. Luồng quan sát không thực thi khóa tài khoản theo yêu cầu.

## Ảnh hưởng
Cơ chế khóa tài khoản theo tài liệu không có hiệu lực trong kịch bản này, tăng nguy cơ thử đoán mật khẩu nhiều lần. Chưa đánh giá các biện pháp giới hạn tần suất khác.

## Đề xuất điều tra / sửa
Kiểm tra luồng xác thực và cơ chế đếm lần sai, thời điểm hết hạn khóa ở backend. Phải chặn cả mật khẩu đúng trong thời gian khóa; trả trạng thái khóa an toàn cho UI. Không chỉ khóa ở frontend.

## Tiêu chí retest
Với tài khoản thử riêng và bộ đếm được xác nhận sạch: lần 1–4 bị từ chối; lần 5 kích hoạt khóa 15 phút; mật khẩu đúng/sai đều bị chặn trong thời gian khóa; hiển thị thời gian còn lại; hết hạn mới đăng nhập thành công. Lưu thời gian, phản hồi API và mã build, không lưu mật khẩu.
