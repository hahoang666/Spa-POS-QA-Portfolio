# Bộ ca kiểm thử giao diện và kiểm tra dữ liệu — Đăng nhập Spa POS

**Ứng dụng:** [Spa POS](https://hahoang666.github.io/Spa-pos-pp/)  
**Yêu cầu nguồn:** [Requirement Baseline v1.0](https://github.com/hahoang666/Spa-pos-pp/blob/main/docs/REQUIREMENT_BASELINE_V1.0.md)  
**Mã giao diện đã đọc:** [index.html](https://github.com/hahoang666/Spa-pos-pp/blob/main/index.html)  
**Ngày soạn:** 13/10/2026  
**Trạng thái:** AUTH-UI-01 ĐẠT (ảnh chụp đã đối chiếu, Console theo người kiểm thử); các ca còn lại CHƯA CHẠY.

## Điều kiện thực hiện
Dùng tài khoản thử được cấp phép; không gửi mật khẩu hoặc mã truy cập. Với ca khóa tài khoản, chuẩn bị tài khoản riêng và phương án mở khóa trước khi thử. Ghi rõ trình duyệt, phiên bản, ngày và ảnh chụp.

## Danh sách ca kiểm thử

Mã ca kiểm thử | Mã yêu cầu | Loại kiểm thử | Tình huống | Bước thực hiện / dữ liệu thử | Kết quả mong đợi | Mức ưu tiên | Kết quả thực tế | Trạng thái | Mã lỗi | Bằng chứng | Phiên bản / Ngày
--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ---
AUTH-UI-01 | AUTH-01 | Giao diện | Thành phần màn hình đăng nhập | Mở trang đăng nhập | Hiển thị nhãn và ô tên đăng nhập/mật khẩu, mật khẩu được che, ô ghi nhớ và nút Đăng nhập | Thấp | Màn hình hiển thị đủ tên đăng nhập, mật khẩu, nút hiện/ẩn, ô ghi nhớ và nút Đăng nhập; mật khẩu được che mặc định, không chồng lấn. Người kiểm thử báo cáo Console không lỗi. | ĐẠT |  | Ảnh chụp người dùng cung cấp: image(9).png (đính kèm hội thoại, chưa lưu vào GitHub). Console theo báo cáo người kiểm thử. | 2026-10-13; build not recorded
AUTH-UI-02 | AUTH-01 | Giao diện | Hiện/ẩn mật khẩu | Nhập chuỗi giả; nhấn biểu tượng hiện/ẩn hai lần | Mật khẩu chuyển ẩn → hiện → ẩn mà không thay đổi giá trị | Trung bình | Nội dung nhập ban đầu được che; nhấn biểu tượng lần đầu hiển thị nội dung, nhấn lần hai che lại. Không ghi nhận thay đổi nội dung. | ĐẠT |  | Theo kết quả thao tác người kiểm thử cung cấp; chưa có ảnh chụp. | 13/10/2026; chưa ghi nhận phiên bản
AUTH-UI-03 | AUTH-01 | Giao diện | Thao tác bằng bàn phím | Dùng Tab, Shift+Tab và Enter | Thứ tự focus hợp lý, thấy focus và Enter gửi biểu mẫu | Trung bình | Tab và Shift+Tab điều hướng được biểu mẫu; nút hiện/ẩn mật khẩu và ô ghi nhớ tên đăng nhập đều nhận focus bằng bàn phím, có viền nhận biết. Enter gửi biểu mẫu với thông tin sai và hiện thông báo lỗi đúng. | ĐẠT |  | Theo kết quả kiểm thử thủ công người dùng báo cáo; chưa có video thao tác. | 13/10/2026; chưa ghi phiên bản
AUTH-UI-04 | AUTH-01 | Giao diện | Giao diện theo kích thước màn hình | Kiểm tra màn hình máy tính và điện thoại | Không tràn ngang, che khuất hoặc chồng lấn thành phần | Trung bình | Ở kích thước 375x667, 768x1024 và 1366x768, biểu mẫu đăng nhập không chồng lấn và vẫn thao tác được. Trên điện thoại, phần giới thiệu bên trái bị cắt; chưa xác nhận đây là chủ đích thiết kế. | ĐẠT (CÓ GHI CHÚ) |  | Theo kết quả kiểm thử người dùng cung cấp; chưa có ảnh chụp; cần xác minh yêu cầu thiết kế phần giới thiệu trên điện thoại. | 13/10/2026; chưa ghi phiên bản
AUTH-UI-05 | AUTH-05 | Giao diện | Thông báo bảo mật | Mở màn hình đăng nhập | Hiển thị: Không lưu thông tin đăng nhập trên thiết bị | Trung bình | Màn hình đăng nhập không hiển thị thông báo 'Không lưu thông tin đăng nhập trên thiết bị'; các chức năng đăng nhập khác vẫn hoạt động theo báo cáo người kiểm thử. | KHÔNG ĐẠT | DEF-AUTH-001 | Người kiểm thử quan sát và báo cáo; chưa có ảnh chụp mới. Yêu cầu AUTH-05 trong baseline. | 13/10/2026; chưa ghi phiên bản
AUTH-UI-06 | AUTH-04 | Giao diện | Giao diện sau đăng xuất | Đăng nhập bằng tài khoản thử rồi đăng xuất | Hiện màn hình đăng nhập; xóa nội dung và menu hệ thống | Cao | Sau khi chọn Đăng xuất, ứng dụng chuyển về màn hình đăng nhập. Menu và nội dung của phiên đăng nhập trước không còn hiển thị. Người kiểm thử không ghi nhận bất thường. | ĐẠT |  | Kết quả thực thi thủ công do người kiểm thử cung cấp; chưa đính kèm ảnh. Chưa xác minh phiên bằng nút Quay lại/tải lại (AUTH-VAL-10). | 13/10/2026; chưa ghi phiên bản
AUTH-VAL-01 | AUTH-01 | Kiểm tra dữ liệu | Bỏ trống cả hai trường | Không nhập gì và nhấn Đăng nhập | Báo thiếu tên đăng nhập tại đúng trường; không đăng nhập | Cao | Để trống cả Tên đăng nhập và Mật khẩu rồi nhấn Đăng nhập: xuất hiện 'Vui lòng nhập tên đăng nhập.', ô Tên đăng nhập có viền đỏ, hệ thống không cho đăng nhập. | ĐẠT |  | Người kiểm thử báo cáo thực thi thủ công; chưa đính kèm ảnh chụp. | 13/10/2026; chưa ghi phiên bản
AUTH-VAL-02 | AUTH-01 | Kiểm tra dữ liệu | Bỏ trống tên đăng nhập | Nhập mật khẩu thử; bỏ trống tên đăng nhập | Báo thiếu tên đăng nhập; không đăng nhập | Cao | Để trống Tên đăng nhập, nhập mật khẩu thử rồi nhấn Đăng nhập: hiển thị 'Vui lòng nhập tên đăng nhập.', ô Tên đăng nhập có viền đỏ và hệ thống ngăn đăng nhập. | ĐẠT |  | Theo kết quả kiểm thử thủ công người dùng báo cáo; chưa đính kèm ảnh. | 13/10/2026; chưa ghi phiên bản
AUTH-VAL-03 | AUTH-01 | Kiểm tra dữ liệu | Bỏ trống mật khẩu | Nhập tên đăng nhập thử; bỏ trống mật khẩu | Báo thiếu mật khẩu; không đăng nhập | Cao |  | CHƯA CHẠY |  |  | 
AUTH-VAL-04 | AUTH-01 | Kiểm tra dữ liệu | Sai thông tin đăng nhập | Nhập tên đăng nhập thử không tồn tại và mật khẩu sai | Báo sai thông tin kèm số lần còn lại theo baseline; không đăng nhập | Cao |  | CHƯA CHẠY |  |  | 
AUTH-VAL-05 | AUTH-01 | Kiểm tra dữ liệu | Tên đăng nhập không phân biệt chữ hoa/thường | Đăng nhập với tên hợp lệ nhưng thay đổi hoa/thường | Đăng nhập được cùng một tài khoản | Trung bình |  | CHƯA CHẠY |  |  | 
AUTH-VAL-06 | AUTH-01 | Kiểm tra dữ liệu | Khoảng trắng đầu/cuối tên đăng nhập | Thêm khoảng trắng quanh tên hợp lệ; nhập mật khẩu đúng | Loại bỏ khoảng trắng theo mã hiện tại và đăng nhập thành công | Trung bình |  | CHƯA CHẠY |  |  | 
AUTH-VAL-07 | AUTH-02 | Kiểm tra dữ liệu | Khóa tạm sau năm lần sai | Dùng tài khoản thử riêng; nhập sai mật khẩu năm lần | Khóa 15 phút, hiển thị thời gian còn lại; không cho đăng nhập trong thời gian khóa | Cao |  | CHƯA CHẠY |  |  | 
AUTH-VAL-08 | AUTH-02 | Kiểm tra dữ liệu | Đặt lại bộ đếm sai | Nhập sai dưới năm lần, đăng nhập đúng, rồi thử sai lần nữa | Bộ đếm được đặt lại sau đăng nhập đúng | Cao |  | CHƯA CHẠY |  |  | 
AUTH-VAL-09 | AUTH-03 | Kiểm tra dữ liệu | Tài khoản bị khóa thủ công | Admin khóa tài khoản thử rồi thử đăng nhập | Từ chối; báo Tài khoản đã bị khóa | Cao |  | CHƯA CHẠY |  |  | 
AUTH-VAL-10 | AUTH-04 | Kiểm tra dữ liệu | Quay lại và tải lại sau đăng xuất | Đăng nhập, đăng xuất, dùng Back và Reload | Không truy cập hệ thống khi chưa xác thực | Cao |  | CHƯA CHẠY |  |  | 
AUTH-VAL-11 | AUTH-05 | Kiểm tra dữ liệu | Chỉ ghi nhớ tên đăng nhập | Chọn ghi nhớ, đăng nhập, đăng xuất/tải lại | Giữ tên đăng nhập; không giữ mật khẩu | Trung bình |  | CHƯA CHẠY |  |  | 
AUTH-VAL-12 | AUTH-05 | Kiểm tra dữ liệu | Không ghi nhớ tên đăng nhập | Bỏ chọn ghi nhớ, đăng nhập, đăng xuất/tải lại | Không giữ tên đăng nhập hoặc mật khẩu | Trung bình |  | CHƯA CHẠY |  |  | 
AUTH-VAL-13 | AUTH-05 | Kiểm tra dữ liệu | Bảo mật dữ liệu lưu trên trình duyệt | Mở DevTools → Application → Storage sau đăng nhập/đăng xuất | Không lưu mật khẩu dạng rõ; phiên đăng nhập tuân theo baseline đã thống nhất | Cao |  | CHƯA CHẠY |  |  | 
AUTH-VAL-14 | AUTH-01 | Kiểm tra dữ liệu | Đăng nhập đúng thông tin | Đăng nhập bằng tài khoản thử được phép | Xác thực thành công và hiển thị Tổng quan | Cao |  | CHƯA CHẠY |  |  | 
AUTH-VAL-15 | AUTH-01 | Kiểm tra dữ liệu | Không kết nối được máy chủ | Giả lập mất kết nối có kiểm soát bằng DevTools | Thông báo phù hợp, không truy cập trái phép, nút Đăng nhập trở lại bình thường | Trung bình |  | CHƯA CHẠY |  |  | 

## Nhận xét từ việc đọc mã (chưa kết luận lỗi)
- Biểu mẫu có tên đăng nhập, mật khẩu, nút hiện/ẩn, ô ghi nhớ tên và nút Đăng nhập.
- Hàm `login()` loại khoảng trắng quanh tên đăng nhập, kiểm tra trường trống và dùng Supabase Auth.
- Thông báo sai thông tin hiện tại chưa thể hiện số lần còn lại trong phần xử lý giao diện đã đọc; baseline có yêu cầu này.
- Chưa thấy bộ đếm khóa sau năm lần sai ngay trong hàm `login()`; cần kiểm tra thêm backend và chạy thực tế.
- Hàm `restoreSupabaseSession()` thử khôi phục phiên; cần đối chiếu với yêu cầu AUTH-05 và cơ chế ghi nhớ tên.
- Cần xác nhận thông báo bảo mật theo baseline có hiển thị thực tế không.
- Kiểm thử bàn phím và giao diện điện thoại cần thực hiện trên trình duyệt.

## Cách ghi kết quả
Điền **Kết quả thực tế** đúng những gì quan sát, **Trạng thái** là ĐẠT/KHÔNG ĐẠT/BỊ CHẶN/CHƯA CHẠY và đính kèm bằng chứng. Nếu không đạt, ghi nhật ký lỗi với các bước tái hiện. Không xem nhận xét đọc mã là lỗi đã xác nhận.

## Đợt thực thi đầu tiên
Ưu tiên AUTH-UI-01, AUTH-UI-02, AUTH-UI-03, AUTH-VAL-01, AUTH-VAL-02, AUTH-VAL-03, AUTH-VAL-04. Chỉ kiểm thử khóa tạm trên tài khoản thử riêng.
