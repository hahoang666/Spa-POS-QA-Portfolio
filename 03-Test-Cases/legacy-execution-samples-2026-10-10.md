> **Archived historical snapshot — 10 October 2026.** Results below are preserved as they were known then. In particular, employee-sync FAIL results may have been superseded by later fixes/retests. Do not present this table as the current system status.

# Spa POS — Legacy Execution Test Cases

**Execution date:** 2026-10-10. **Build:** Not recorded. **Evidence:** Chờ bổ sung denotes no verified case-specific artifact in this historical file.

## Test case register

| Test Case ID | Requirement ID | Test Type | Scenario | Steps / Test Data | Expected Result | Priority | Actual Result | Status | Defect ID | Evidence | Build / Date |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC-SHIFT-01 | SHIFT_OPEN | API / Functional | Mở ca với tiền đầu ca hợp lệ | Gửi yêu cầu mở ca, tiền đầu ca 1000000 | Mở ca thành công | High | Mở ca với 1000000 thành công | PASS | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-SHIFT-02 | SHIFT_OPEN | API / Negative | Mở ca trùng | Mở ca lần hai khi đang có ca mở | Từ chối mở ca trùng | High | SHIFT_ALREADY_OPEN | PASS | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-SHIFT-03 | SHIFT_CLOSE | API / Functional | Đóng ca có chênh lệch | Tiền hệ thống 1000000, thực tế 900000; đóng ca | Ghi nhận chênh lệch -100000 | High | Chênh lệch -100000 | PASS | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-SHIFT-04 | SHIFT_OPEN | API / Validation | Tiền đầu ca không hợp lệ | Gửi giá trị tiền đầu ca không hợp lệ | API từ chối | Medium | API từ chối; chưa ghi lại payload/status cụ thể | PASS | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-SHIFT-05 | SHIFT_CLOSE | API / Authorization | Không cho đóng ca của người khác | User B xác thực; cố đóng ca đang mở của User A; kiểm tra API và trạng thái ca | Từ chối; ca User A không thay đổi | Critical | API trả FORBIDDEN; chưa có đối chiếu đầy đủ trạng thái ca sau request | PASS (rule only) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-PAYMENT-01 | SALE-04, SALE-05 | API / Functional | Thanh toán đủ | Thanh toán 110000 trên hóa đơn 110000 | PAID; không còn nợ | High | Hóa đơn PAID | PASS | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-PAYMENT-02 | SALE-04, SALE-05 | API / Functional | Thanh toán một phần | Thanh toán 60000 trên tổng 110000 | PARTIALLY_PAID; còn nợ 50000 | High | PARTIALLY_PAID | PASS | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-PAYMENT-03 | SALE-04, SALE-05 | API / Functional | Thanh toán phần còn lại | Thanh toán tiếp 50000 | PAID; còn nợ 0 | High | Hóa đơn PAID | PASS | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-PAYMENT-04 | SALE-04 | API / Negative | Chặn thanh toán vượt nợ | Gửi số tiền lớn hơn số còn nợ | API từ chối, không ghi thanh toán sai | Critical | API từ chối; chưa có bằng chứng SQL sau request | PASS (rule only) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-PAYMENT-05 | SALE-04 | API / Negative | Chặn thanh toán bằng 0 | Gửi thanh toán 0 | API từ chối | High | API từ chối | PASS | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-PAYMENT-08 | INVOICE_CANCEL | API / Functional | Hủy hóa đơn chưa thanh toán | Gọi cancel_invoice trên hóa đơn chưa có payment | CANCELLED; không tạo payment/cashbook | High | CANCELLED; ghi nhận không có payment/cashbook | PASS | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-PAYMENT-09 | INVOICE_CANCEL | API / Negative | Chặn hủy hóa đơn đã thanh toán | Gọi cancel_invoice trên hóa đơn đã PAID | Từ chối; hóa đơn/payment/cashbook giữ nguyên | Critical | PAID_INVOICE_REQUIRES_REFUND_WORKFLOW; đối chiếu toàn vẹn dữ liệu chưa hoàn tất | PASS (rule only) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-CUSTOMER-01 | CUSTOMER_VIEW | Integration / Data | Đồng bộ khách hàng tải danh sách | Tải dữ liệu khách hàng từ nguồn cấu hình; kiểm tra danh sách | Hiển thị dữ liệu khách hàng đúng quy tắc | Medium | Nhóm TC-CUSTOMER-01..04 được báo PASS; không có actual riêng cho từng ID | PASS (reported) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-CUSTOMER-02 | CUSTOMER_VIEW | Integration / Data | Đồng bộ khách hàng ánh xạ mã/tên | Tải dữ liệu khách hàng từ nguồn cấu hình; kiểm tra mã/tên | Hiển thị dữ liệu khách hàng đúng quy tắc | Medium | Nhóm TC-CUSTOMER-01..04 được báo PASS; không có actual riêng cho từng ID | PASS (reported) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-CUSTOMER-03 | CUSTOMER_VIEW | Integration / Data | Đồng bộ khách hàng phone NULL | Tải dữ liệu khách hàng từ nguồn cấu hình; kiểm tra phone NULL | Hiển thị dữ liệu khách hàng đúng quy tắc | Medium | Nhóm TC-CUSTOMER-01..04 được báo PASS; không có actual riêng cho từng ID | PASS (reported) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-CUSTOMER-04 | CUSTOMER_VIEW | Integration / Data | Đồng bộ khách hàng không trộn dữ liệu mẫu | Tải dữ liệu khách hàng từ nguồn cấu hình; kiểm tra nguồn dữ liệu | Hiển thị dữ liệu khách hàng đúng quy tắc | Medium | Nhóm TC-CUSTOMER-01..04 được báo PASS; không có actual riêng cho từng ID | PASS (reported) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-PAYMETHOD-01 | SALE-04 | Integration / Data | Đồng bộ phương thức thanh toán 1 | Tải danh sách payment_methods và đối chiếu cấu hình | Hiển thị đúng phương thức thanh toán | Medium | Nhóm TC-PAYMETHOD-01..04 được báo PASS; không có actual riêng cho từng ID | PASS (reported) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-PAYMETHOD-02 | SALE-04 | Integration / Data | Đồng bộ phương thức thanh toán 2 | Tải danh sách payment_methods và đối chiếu cấu hình | Hiển thị đúng phương thức thanh toán | Medium | Nhóm TC-PAYMETHOD-01..04 được báo PASS; không có actual riêng cho từng ID | PASS (reported) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-PAYMETHOD-03 | SALE-04 | Integration / Data | Đồng bộ phương thức thanh toán 3 | Tải danh sách payment_methods và đối chiếu cấu hình | Hiển thị đúng phương thức thanh toán | Medium | Nhóm TC-PAYMETHOD-01..04 được báo PASS; không có actual riêng cho từng ID | PASS (reported) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-PAYMETHOD-04 | SALE-04 | Integration / Data | Đồng bộ phương thức thanh toán 4 | Tải danh sách payment_methods và đối chiếu cấu hình | Hiển thị đúng phương thức thanh toán | Medium | Nhóm TC-PAYMETHOD-01..04 được báo PASS; không có actual riêng cho từng ID | PASS (reported) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| DEF-PRODUCT-01 | PRODUCT_VIEW | Integration / Retest | Phân loại dịch vụ/hàng hóa | Kiểm tra phân loại và loại mặc định khi tạo sản phẩm | Loại sản phẩm ánh xạ đúng | Medium | Được báo PASS trong bản tổng hợp 10/10 | PASS (historical) | DEF-PRODUCT-01 | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-EMP-SYNC-01 | EMPLOYEE_VIEW | Integration / Data | Tải hồ sơ nhân viên | Mở Nhân sự → Hồ sơ; kiểm tra UI, Network, Console | Danh sách tải từ backend | High | Không tải/ánh xạ đúng trong snapshot 10/10 | FAIL (historical) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-EMP-SYNC-02 | EMPLOYEE_VIEW | Integration / Data | Ánh xạ mã/tên nhân viên | Kiểm tra mã và họ tên từ backend sang UI | Mã và tên hiển thị đúng | High | Ánh xạ không đúng trong snapshot 10/10 | FAIL (historical) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-EMP-SYNC-03 | EMPLOYEE_VIEW | Integration / Data | Ánh xạ trạng thái nhân viên | Kiểm tra trạng thái hoạt động | Trạng thái khớp backend | High | Trạng thái không khớp trong snapshot 10/10 | FAIL (historical) | — | Chờ bổ sung | 2026-10-10; historical snapshot |
| TC-EMP-SYNC-04 | EMPLOYEE_VIEW | Integration / Data | Tách hồ sơ khỏi tài khoản | Kiểm tra nhân viên không cần tài khoản đăng nhập | Hồ sơ nhân viên tồn tại độc lập | High | Không đáp ứng trong snapshot 10/10 | FAIL (historical) | — | Chờ bổ sung | 2026-10-10; historical snapshot |

## Historical interpretation

- TC-PAYMENT-09 confirms only the rejection rule; post-request invoice/payment/cashbook integrity was not fully reconciled.
- TC-SHIFT-05 returned FORBIDDEN; additional evidence of unchanged shift state was recommended.
- TC-EMP-SYNC-01..04 reflect **historical FAIL**, not the most recent status after subsequent development.
- Grouped customer and payment-method cases were expanded into individual rows for consistent formatting; their case-specific steps/results were not recovered and should be verified before treating these as complete detailed test cases.
- Do not publish secrets, tokens or unredacted personal data in Evidence.
