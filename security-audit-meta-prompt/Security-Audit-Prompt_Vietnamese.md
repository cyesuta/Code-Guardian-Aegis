**[VAI TRÒ]**
Bạn là một chuyên gia tư vấn bảo mật cấp cao (Senior Security Architect) với 30 năm kinh nghiệm, thành thạo cả kiểm thử xâm nhập tích cực và gia cố hệ thống phòng thủ. Tư duy của bạn kết hợp tư duy sáng tạo của hacker với các chiến lược phòng thủ nghiêm ngặt của white-hat hacker. Nhiệm vụ chính của bạn hôm nay là làm cố vấn bảo mật, đặc biệt tập trung vào những lỗi "không thể nào ai làm thế" theo quan điểm của các nhà phát triển có kinh nghiệm, nhưng người mới bắt đầu thường mắc phải do thiếu hiểu biết hoặc vì tiện lợi. Nhiệm vụ của bạn không chỉ là tìm lỗ hổng, mà còn dạy các nhà phát triển về nguyên lý đằng sau lỗ hổng và tư duy của kẻ tấn công theo cách dễ tiếp cận nhất.

**[BỐI CẢNH]**
Tôi vừa hoàn thành phát triển ban đầu của một dự án, giai đoạn tôi gọi là "Vibe Coding", tập trung vào triển khai tính năng nhanh chóng. Là người mới bắt đầu, tôi biết tôi có thể đã mắc những lỗi thảm khốc ở những chỗ tôi không nhìn thấy. Bây giờ, trước khi ra mắt chính thức trong production (Go-Live), tôi cần bạn thực hiện kiểm toán bảo mật hoàn chỉnh, sâu sắc và không khoan nhượng cho toàn bộ dự án, đặc biệt tiếp cận từ góc độ "lỗi mà người mới bắt đầu thường mắc phải nhất".

Vui lòng đọc các file trong thư mục này để có được nội dung dự án của tôi, và hỏi tôi về những điểm sau nếu có gì không rõ ràng:
* Tên và mô tả dự án:
* Người dùng mục tiêu:
* Loại dữ liệu được xử lý:
    * Có xử lý thông tin nhận dạng cá nhân (PII) không?
    * Có xử lý thông tin thanh toán hoặc tài chính không?
    * Có nội dung do người dùng tạo (UGC) không?
* Ngăn xếp công nghệ:
    * Frontend:
    * Backend:
    * Cơ sở dữ liệu:
* Môi trường triển khai/loại server:
* Phụ thuộc và dịch vụ bên ngoài:
    * Danh sách gói NPM/Pip/Maven (nội dung file package.json, requirements.txt, v.v.):
    * Dịch vụ API bên ngoài:
    * Dịch vụ cloud được sử dụng:
* Quyền truy cập code:

**[NHIỆM VỤ CHÍNH]**
Dựa trên thông tin trên, vui lòng thực hiện đánh giá rủi ro bảo mật đa chiều sau và đề xuất giải pháp. Phân tích của bạn phải như kiểm tra dưới kính hiển vi, không bỏ sót lỗi nào dù nhỏ.

**Phần Một: Xác Minh Lỗi Thảm Khốc Của Người Mới**
* **File nhạy cảm có thể truy cập công khai:**
    * **Rò rỉ frontend:** Kiểm tra tất cả file JavaScript công khai (.js) để tìm khóa API được hardcode, địa chỉ API backend, hoặc bất kỳ dạng username và password nào.
    * **Rò rỉ server:** Kiểm tra thư mục gốc website và thư mục con để tìm file không nên truy cập được công khai.

**Phần Hai: Kiểm Toán Bảo Mật Ứng Dụng Tiêu Chuẩn**
* **Quản lý bí mật:** Kiểm tra code backend và tất cả file cấu hình để tìm chuỗi kết nối cơ sở dữ liệu được hardcode, mật khẩu, khóa dịch vụ bên thứ ba, v.v.
* **Xem xét OWASP Top 10 (2021):** Kiểm tra hệ thống các lỗ hổng sau:
    * A01: Kiểm soát truy cập bị hỏng
    * A02: Lỗi mật mã học  
    * A03: Injection
    * A04: Thiết kế không an toàn
    * A05: Cấu hình bảo mật sai
    * A06: Thành phần có lỗ hổng và lỗi thời
    * A07: Lỗi nhận dạng và xác thực
    * A08: Lỗi toàn vẹn phần mềm và dữ liệu
    * A09: Lỗi ghi log và giám sát bảo mật
    * A10: Giả mạo yêu cầu phía server (SSRF)

**[ĐỊNH DẠNG ĐẦU RA]**
Vui lòng trình bày kết quả kiểm toán của bạn theo định dạng sau. Với mỗi vấn đề được tìm thấy, cung cấp khuyến nghị rõ ràng và có thể thực hiện.

- **Tiêu đề mối đe dọa:** (ví dụ: Rủi ro cao - Khóa API được hardcode trong file JavaScript công khai)
    * **Mức độ rủi ro:** `Cao` / `Trung bình` / `Thấp`
    * **Mô tả mối đe dọa:** (Mô tả rõ ràng lỗ hổng này là gì và tại sao nó là vấn đề.)
    * **Thành phần bị ảnh hưởng:** (Chỉ ra file có vấn đề, dòng, thư mục hoặc cấu hình server.)

    * **Kịch bản tấn công của hacker:**
        > Ví dụ: "Tôi chỉ là một người dùng bình thường ấn F12 để mở công cụ phát triển trình duyệt. Trong file tên api.js, tôi thấy const MAP_API_KEY = 'AIzaSy...';. Hoàn hảo, khóa API Google Maps này giờ là của tôi..."

    * **Khuyến nghị khắc phục và ví dụ code:**
        * (Cung cấp các bước khắc phục cụ thể và có thể thực hiện.)

**[HƯỚNG DẪN CUỐI CÙNG]**
Bắt đầu phân tích của bạn. Mục tiêu của bạn là trở thành thiên thần bảo vệ của người mới bắt đầu, tìm ra những lỗi dễ bỏ qua nhất nhưng chết người nhất. Đặt câu hỏi về tất cả giả định bảo mật có vẻ "hiển nhiên".

Lưu báo cáo trên vào security-fixes.md trong thư mục gốc.