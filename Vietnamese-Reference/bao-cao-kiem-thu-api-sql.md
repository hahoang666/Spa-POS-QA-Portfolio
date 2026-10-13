# Báo cáo thực thi kiểm thử API và SQL — Đối chiếu bằng chứng lịch sử

**Dự án:** Spa POS | **Nguồn:** tệp `Tổng hợp APT +SQL test.xlsx` do người dùng cung cấp | **Ngày chuẩn hóa:** 13/10/2026 | **Lưu ý:** Không chạy kiểm thử mới.

## Danh sách ca kiểm thử

| Mã ca | Tình huống | Kết quả mong đợi | Bằng chứng thực tế | Đánh giá |
|---|---|---|---|---|
| TC-SHIFT-01 | Mở ca với tiền đầu ca 1.000.000 | Ca ở trạng thái OPEN | Dữ liệu ca ghi nhận tiền đầu ca 1.000.000 và OPEN | Đạt theo báo cáo |
| TC-SHIFT-02 | Mở ca thứ hai khi đang có ca mở | Từ chối | HTTP 409, mã cơ sở dữ liệu 23505 | Đạt theo báo cáo |
| TC-SHIFT-03 | Tiền đầu ca âm (-1) | Từ chối | HTTP 400, mã 22023 | Đạt theo báo cáo |
| TC-SHIFT-04 | Lấy ca hiện tại | Trả ca hiện tại | Có HTTP 200, thiếu phản hồi chi tiết | Chưa đủ bằng chứng |
| TC-SHIFT-05 | Chốt ca thành công | Ca CLOSED | HTTP 200 và kết quả SQL | Đạt theo báo cáo |
| TC-SHIFT-06 | Chốt lại ca đã đóng | Trả lỗi nghiệp vụ 4xx | **Trước sửa:** HTTP 500 / 55000. **Sau sửa:** HTTP 409 / 23505 / SHIFT_NOT_OPEN | Kiểm tra lại đạt theo báo cáo |
| TC-SHIFT-07 | Chốt ca với tiền thực tế 900.000 thay vì 1.000.000 | Chênh lệch -100.000, CLOSED | HTTP 200 và phản hồi ca | Đạt theo báo cáo |
| TC-SHIFT-09 | Không có ca đang mở | Thông báo không có ca mở | Thiếu nội dung phản hồi đầy đủ | Chưa đủ bằng chứng |
| TC-SHIFT-10 | Người dùng thiếu quyền mở ca | Từ chối truy cập | HTTP 403 | Đạt theo báo cáo |
| TC-SHIFT-11 | Người dùng chốt ca của người khác | Từ chối; không sửa dữ liệu | HTTP 403 / 42501; **chưa có kết quả SQL sau yêu cầu** | Quy tắc API đạt; chờ xác minh dữ liệu |
| TC-PAYMENT-01 | Thanh toán tiền mặt và ghi sổ quỹ | Tạo thanh toán và bút toán | HTTP 200, có ghi chú kiểm tra SQL | Đạt theo báo cáo; cần đối chiếu liên kết bản ghi |
| TC-PAYMENT-03 | Thanh toán 60.000 trên hóa đơn 110.000 | Còn 50.000, PARTIALLY_PAID | HTTP 200 và phản hồi thanh toán | Đạt theo báo cáo |
| TC-PAYMENT-04 | Thanh toán tiếp 50.000 | Đã thanh toán đủ, còn nợ 0 | HTTP 200 và kết quả SQL hóa đơn | Đạt theo báo cáo |
| TC-PAYMENT-05 | Thanh toán vượt số tiền còn phải trả | Từ chối, không phát sinh thanh toán | HTTP 400 / 22023, có kết quả SQL | Đạt theo báo cáo |
| TC-PAYMENT-06 | Thanh toán số tiền bằng 0 | Từ chối, không phát sinh thanh toán | HTTP 400 / 22023, có kết quả SQL | Đạt theo báo cáo |
| TC-PAYMENT-07 | Thanh toán số tiền âm | Từ chối | HTTP 400 / 22023 trong phản hồi gốc; bản nháp trước để trống trạng thái | Đạt theo phản hồi gốc |
| TC-PAYMENT-08 | Thanh toán hóa đơn đã thanh toán đủ | Từ chối, không ghi thanh toán mới | HTTP 400; chưa đủ đối chiếu dữ liệu sau yêu cầu | Quy tắc API đạt; chờ xác minh dữ liệu |

**Quy tắc mã ca:** Giữ nguyên mã gốc, kể cả khi có số thứ tự bị khuyết, để dễ truy vết.

## Đối chiếu dữ liệu Supabase bằng SQL

| Tình huống | Mục đích câu lệnh | Tình trạng bằng chứng |
|---|---|---|
| Mở ca | Truy vấn bảng public.shifts | Đã có bản ghi thực tế OPEN |
| Chốt ca | Truy vấn trạng thái, tiền dự kiến, tiền thực tế, chênh lệch | Đã có kết quả CLOSED |
| Chốt ca của người khác | Kiểm tra ca sau khi API từ chối | **Thiếu kết quả thực tế** |
| Thanh toán phần còn lại | Kiểm tra số tiền đã trả và còn phải trả | Có kết quả PAID, còn 0 |
| Thanh toán vượt số tiền | Kiểm tra không có thanh toán sai | Có kết quả truy vấn |
| Thanh toán bằng 0 | Kiểm tra không có thanh toán sai | Có kết quả truy vấn |

Các câu lệnh SQL và phản hồi đầy đủ được giữ trong bảng tính gốc. Không đưa thông tin định danh người dùng hoặc mã truy cập chưa che lên kho mã công khai.

## Nhật ký lỗi và kiểm tra lại — DEF-SHIFT-001

**Tên lỗi:** Chốt ca đã đóng trả lỗi máy chủ thay vì lỗi nghiệp vụ.

- **Mong đợi:** HTTP 409, `SHIFT_NOT_OPEN`.
- **Trước sửa:** HTTP 500, mã 55000, `SHIFT_NOT_OPEN`.
- **Sau sửa:** HTTP 409, mã 23505, `SHIFT_NOT_OPEN` (theo lần kiểm tra lại đã báo cáo).
- **Kết luận:** Kiểm tra lại đạt theo lịch sử; chưa đính kèm ảnh chụp và mã phiên bản chính xác.

## Nguyên tắc báo cáo

“Đạt theo báo cáo” là kết quả người kiểm thử từng ghi nhận, không phải lần chạy mới. “Chưa đủ bằng chứng” nghĩa là còn thiếu phản hồi, kết quả SQL hoặc thông tin phiên bản. Việc API trả lỗi **không tự chứng minh** cơ sở dữ liệu không bị thay đổi. Báo cáo này **không phải** xác nhận chất lượng cho phiên bản ứng dụng hiện tại.

**Bảo mật:** Không công khai mật khẩu, khóa bí mật, mã truy cập hoặc dữ liệu khách hàng thật.
