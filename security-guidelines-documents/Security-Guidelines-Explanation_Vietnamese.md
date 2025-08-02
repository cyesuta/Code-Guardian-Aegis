# Giải Thích Hướng Dẫn Bảo Mật VibeCoding: Tại Sao Chúng Tôi Nhấn Mạnh Nhiều Đến Vậy?

Tài liệu này giải thích tầm quan trọng của từng quy tắc trong "Hướng Dẫn Bảo Mật". Hiểu được điều này sẽ giúp bạn nhìn code của mình từ "góc nhìn của hacker" và ngăn chặn vấn đề trước khi chúng xảy ra.

---

## ⭐ Quy Tắc Vàng

### 【Không bao giờ hardcode bí mật】 & 【Sử dụng biến môi trường】 & 【Bỏ qua file bí mật】

**Tại sao điều này quan trọng?**

Ba quy tắc này là luật sinh tồn tối cao của thế giới số. Code của bạn (đặc biệt khi sử dụng Git) sẽ được sao chép, chia sẻ và lưu trữ ở nhiều nơi. Viết mật khẩu, khóa API và các "bí mật" khác trực tiếp vào code giống như xăm mật khẩu két sắt lên trán - bất kỳ ai nhìn thấy bạn (nhìn thấy code) đều có thể dễ dàng mở két sắt của bạn.

**Kịch Bản Hacker 😈**
> Tôi thích tìm kiếm `password`, `api_key` hoặc `db_connect` trên GitHub. Tôi tìm thấy dự án công khai của bạn và trong file tên `config.js` tôi thấy code này: `const db_password = 'Password123!';`. Hoàn hảo! Tôi thậm chí không cần tấn công website của bạn - giờ tôi có thể thử kết nối trực tiếp với cơ sở dữ liệu của bạn bằng mật khẩu này.

**Hậu Quả Thảm Khốc 💥**

**Kiểm soát hoàn toàn dịch vụ.** Hacker có thể đánh cắp, thay đổi, xóa tất cả dữ liệu người dùng của bạn, hoặc sử dụng dịch vụ API trả phí của bạn cho hoạt động bất hợp pháp, với tất cả hóa đơn ghi tên bạn.

---

## 📥 Xử Lý Đầu Vào Người Dùng

### 【Ngăn Chặn Tấn Công SQL Injection】

**Tại sao điều này quan trọng?**

Hãy tưởng tượng cơ sở dữ liệu như một robot chỉ hiểu ngôn ngữ SQL. Nếu bạn nối trực tiếp đầu vào của người dùng với lệnh của mình, người dùng có cơ hội nói "lệnh" của riêng họ. Truy vấn tham số hóa nói với robot: "Nghe này, phần tiếp theo chỉ là **dữ liệu** - dù nội dung là gì, đừng thực thi nó như lệnh."

**Kịch Bản Hacker 😈**
> Trong trường đăng nhập của website bạn, tôi nhập `' OR '1'='1' --` làm tên người dùng. Tôi giả định rằng truy vấn SQL của bạn được viết như thế này: `"SELECT * FROM users WHERE username = '" + userInput + "';`. Đầu vào của tôi đã biến nó thành `SELECT * FROM users WHERE username = '' OR '1'='1' --';`. `'1'='1'` luôn đúng, vậy nên tôi đã bỏ qua xác minh mật khẩu và đăng nhập thành công vào tài khoản người dùng đầu tiên (thường là admin).

**Hậu Quả Thảm Khốc 💥**

Kẻ tấn công có thể bỏ qua đăng nhập, đánh cắp tất cả dữ liệu cơ sở dữ liệu (danh sách người dùng, hash mật khẩu), hoặc thậm chí xóa cơ sở dữ liệu.

---

## 🔐 Quyền Hạn và Xác Thực

### 【Bảo Vệ API Endpoint】

**Tại sao điều này quan trọng?**

Giao diện frontend (UI) có thể ẩn nút, nhưng hacker không bao giờ tin tưởng giao diện. Họ gọi API backend của bạn trực tiếp bằng công cụ (như Postman, curl). Bạn phải giả định rằng mỗi API endpoint sẽ bị tấn công trực tiếp, vì vậy mỗi endpoint phải là một bảo vệ độc lập, tự kiểm tra danh tính và quyền hạn của khách thăm.

**Kịch Bản Hacker 😈**
> Tôi phát hiện rằng để sửa đổi thông tin cá nhân, frontend gửi request đến `/api/user/update`. Mặc dù tôi không thể thấy nút chỉnh sửa của người khác, tôi giả định rằng API này phân biệt người dùng thông qua `userId`. Tôi thử gửi request đến `/api/user/update?userId=1` (ID admin) với dữ liệu tôi muốn sửa đổi. Trời ơi, server không kiểm tra xem request có thực sự đến từ tôi cá nhân và đã thành công thay đổi email của admin!

**Hậu Quả Thảm Khốc 💥**

Người dùng bình thường có thể thay đổi dữ liệu của người dùng khác hoặc thậm chí admin, hoặc thực thi quyền hạn mà họ không nên có, gây ra hỗn loạn hệ thống.