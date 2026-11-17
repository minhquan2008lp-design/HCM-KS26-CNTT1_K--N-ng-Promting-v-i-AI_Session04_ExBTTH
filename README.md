1. Thiết kế giải pháp
Tên giải pháp: Tự động hóa quy trình xử lý đơn hàng tại cửa hàng.

Mục tiêu: Sử dụng AI để kiểm tra thông tin, tính tiền, xác nhận thanh toán và xuất kết quả đơn hàng.

Input (Đầu vào):
- Mã đơn hàng.
- Tên khách hàng.
- Tên sản phẩm.
- Số lượng.
- Đơn giá.
- Trạng thái thanh toán.
Output (Đầu ra):
- Thông tin đơn hàng.
- Tổng tiền.
- Trạng thái đơn hàng.
- Danh sách lỗi.
- Hướng xử lý tiếp theo.
2. Thiết kế chuỗi Prompt Chaining
Quy trình gồm 3 bước liên kết:
Prompt 1 – Kiểm tra dữ liệu

Kiểm tra dữ liệu trống, sai định dạng và không hợp lệ.


Prompt 2 – Xử lý logic

Tính tổng tiền, kiểm tra thanh toán và xác định trạng thái.


Prompt 3 – Xuất kết quả

Tổng hợp dữ liệu thành JSON sạch và lưu trạng thái xử lý.


3. Prompt thực thi từng bước
Prompt 1: Kiểm tra dữ liệu
Bạn là AI kiểm tra đơn hàng.

Hãy kiểm tra mã đơn, khách hàng, sản phẩm, số lượng và đơn giá.
- Thiếu thông tin: NEEDS_INFO.
- Số lượng hoặc đơn giá <= 0: INVALID_DATA.
- Dữ liệu sai định dạng: FORMAT_ERROR.
- Không tự ý đoán hoặc sửa dữ liệu.
Xuất danh sách dữ liệu hợp lệ và lỗi dưới dạng JSON.




Prompt 2: Xử lý logic
Dựa trên kết quả Prompt 1:
- Chỉ xử lý khi dữ liệu hợp lệ.
- Tính tổng tiền = Số lượng × Đơn giá.
- Nếu chưa thanh toán, ghi Chờ thanh toán.
- Nếu dữ liệu còn lỗi, không xác nhận đơn hàng.
- Đưa ra kết quả và trạng thái xử lý.
Xuất JSON.




Prompt 3: Tổng hợp kết quả
Dựa trên kết quả Prompt 1 và Prompt 2:
- Tổng hợp thông tin đơn hàng.
- Kiểm tra tính nhất quán.
- Ghi rõ lỗi nếu có.
- Lưu trạng thái để tiếp tục khi bị gián đoạn.
- Không tự tạo dữ liệu còn thiếu.
- Chỉ xuất JSON hợp lệ, không thêm giải thích bên ngoài.




4. Cơ chế xử lý lỗi và đứt gãy
Tình huống	Cách xử lý
Thiếu dữ liệu	Yêu cầu bổ sung
Số lượng bằng 0	Báo lỗi
Đơn giá âm	Báo lỗi
Sai định dạng	Đánh dấu lỗi
Chưa thanh toán	Chờ thanh toán
Mất ngữ cảnh	Khôi phục trạng thái gần nhất

Ứng dụng kỹ thuật:
- Chain-of-Thought: Kiểm tra logic theo từng bước, chỉ xuất kết luận cần thiết.
- Prompt Chaining: Liên kết 3 prompt tuần tự.
- Context Management: Lưu dữ liệu và trạng thái để tiếp tục xử lý.
5. Kết quả chạy thử
Dữ liệu đầu vào:
- Mã đơn: DH001
- Khách hàng: Nguyễn Văn A
- Sản phẩm: Chuột máy tính
- Số lượng: 2
- Đơn giá: 150.000đ
- Thanh toán: Đã thanh toán.
Kết quả đầu ra:
Xử lý thành công
{
  "order_id": "DH001",
  "customer": "Nguyễn Văn A",
  "product": "Chuột máy tính",
  "quantity": 2,
  "unit_price": 150000,
  "total": 300000,
  "payment_status": "Đã thanh toán",
  "order_status": "Hợp lệ",
  "errors": [],
  "next_action": "Xác nhận đơn hàng"
}