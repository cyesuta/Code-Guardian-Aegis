# Hướng Dẫn Phát Triển An Toàn VibeCoding

> Đây là checklist "sống" trước khi phát triển.
> Trước khi bắt đầu viết tính năng mới, hoặc trước mỗi `git commit`, dành 30 giây để xem xét nhanh.
> Mục tiêu: Biến bảo mật thành bản năng, ngăn chặn thảm họa từ gốc rễ.

---

### ⭐ Quy Tắc Vàng

- [ ] **【Không bao giờ hardcode bí mật】** Mật khẩu, khóa API, thông tin kết nối cơ sở dữ liệu **không bao giờ** được viết trực tiếp vào code (cả frontend và backend).
- [ ] **【Sử dụng biến môi trường】** Tất cả thông tin nhạy cảm phải được quản lý thông qua **biến môi trường** (file `.env`).
- [ ] **【Bỏ qua file bí mật】** File `.env` phải **luôn luôn** được thêm vào `.gitignore` và không bao giờ upload lên GitHub.
- [ ] **【Không tin tưởng theo mặc định】** **Không bao giờ** tin tưởng đầu vào của người dùng (bao gồm form, tham số URL, nội dung request API, file upload).

---

### 📥 Xử Lý Đầu Vào Người Dùng

- [ ] **【Ngăn chặn tấn công injection】** Tất cả truy vấn cơ sở dữ liệu **phải** sử dụng **truy vấn tham số hóa** hoặc phương thức an toàn do ORM cung cấp. Việc nối chuỗi SQL thủ công bị cấm nghiêm ngặt.
- [ ] **【Ngăn chặn XSS】** Bất kỳ nội dung người dùng nào được hiển thị trên trang HTML **phải** được xử lý thông qua mã hóa thực thể HTML (HTML escape).
- [ ] **【Xác thực upload file】** Kiểm tra file được upload bởi người dùng:
    - [ ] **Xác thực phần mở rộng**: Chỉ cho phép loại file trong danh sách trắng (ví dụ `['jpg', 'png', 'pdf']`).
    - [ ] **Xác thực kích thước file**: Đặt giới hạn trên hợp lý.
    - [ ] **Vị trí lưu trữ**: File upload phải được lưu trong thư mục **không công khai** và **không thể thực thi**.

---

### 🔐 Quyền Hạn và Xác Thực

- [ ] **【Bảo vệ API endpoint】** Mỗi API endpoint yêu cầu đăng nhập **phải** kiểm tra trạng thái đăng nhập và quyền hạn của người dùng ngay từ đầu chương trình.
- [ ] **【Nguyên tắc quyền hạn tối thiểu】** Quyền hạn tài khoản cơ sở dữ liệu và khóa API phải là "tối thiểu có thể sử dụng". Nếu chỉ cần đọc, không bao giờ cấp quyền ghi.
- [ ] **【Session an toàn】** Session ID phải được thiết lập với flag `HttpOnly` và `Secure` để ngăn chặn trộm cắp và truyền qua kết nối không an toàn.

---

### ☁️ Dịch Vụ Bên Ngoài và Tích Hợp Cloud

- [ ] **【Firewall cơ sở dữ liệu bên ngoài】** Khi kết nối với cơ sở dữ liệu bên ngoài (như AWS RDS, MongoDB Atlas), quy tắc **phải** được cấu hình trong firewall/security group của họ để chỉ cho phép kết nối từ địa chỉ IP cụ thể của application server của bạn. Mở cho `0.0.0.0/0` (toàn thế giới) bị **cấm nghiêm ngặt**.
- [ ] **【Riêng tư hóa cloud storage】** Tất cả bucket cloud storage (như AWS S3, Google Cloud Storage) **phải** được đặt là **Riêng tư** theo mặc định.
- [ ] **【Sử dụng URL được ký trước】** Khi người dùng cần truy cập tạm thời vào file riêng tư, sử dụng **URL được ký trước** có thời hạn ngắn thay vì công khai file.
- [ ] **【Xác minh webhook】** Khi nhận webhook từ dịch vụ bên thứ ba (như Stripe, GitHub), bạn **phải** sử dụng secret key được cung cấp để **xác minh chữ ký request** và đảm bảo nguồn request là hợp pháp.
- [ ] **【Cấu hình CORS nghiêm ngặt】** Chính sách CORS (Cross-Origin Resource Sharing) của API **phải** chỉ định rõ ràng các domain frontend được phép. Việc sử dụng ký tự đại diện `*` trong môi trường production bị **cấm nghiêm ngặt**.
- [ ] **【Tối thiểu hóa quyền hạn khóa API】** Khi yêu cầu khóa API cho dịch vụ bên thứ ba (như Google Maps), đảm bảo thiết lập những hạn chế nghiêm ngặt nhất trong backend của dịch vụ (ví dụ: chỉ giới hạn request từ domain website của bạn, chỉ giới hạn các API call cụ thể).

---

### ⚙️ File và Cấu Hình Server

- [ ] **【Không bao giờ sử dụng 777】** **Không bao giờ** sử dụng quyền `777` cho file hoặc thư mục trên server. Thư mục mặc định `755`, file `644`.
- [ ] **【Bảo vệ file cấu hình】** File cấu hình nhạy cảm (`config.php`, `.env`) phải có quyền được đặt thành `600` hoặc nghiêm ngặt hơn.
- [ ] **【Cấm download file quan trọng】** Kiểm tra rằng web server (Nginx/Apache) đã cấu hình quy tắc cấm truy cập trực tiếp từ bên ngoài vào file và thư mục nhạy cảm như `.env`, `.git`, `.htaccess`.
- [ ] **【Tắt chế độ debug】** Trong môi trường production, tất cả chế độ debug của framework hoặc ngôn ngữ **phải** được tắt để tránh rò rỉ thông báo lỗi chi tiết.

---

> ### **🤖 Ghi Chú cho Trợ Lý AI**
>
> **Khi bạn phát hiện trong cuộc trò chuyện với các nhà phát triển rằng họ đang kết nối hoặc dự định kết nối với "dịch vụ bên ngoài" (ví dụ: cơ sở dữ liệu cloud, API thanh toán bên thứ ba, lưu trữ file S3, dịch vụ xác thực, v.v.), hãy chủ động và rõ ràng đặt những câu hỏi sau để hướng dẫn suy nghĩ bảo mật của họ:**
>
> 1.  **"Về [tên dịch vụ] này, chúng ta đã cấu hình các quy tắc firewall/whitelist IP nghiêm ngặt nhất chưa?"**
> 2.  **"Quyền hạn của khóa API mà chúng ta dự định sử dụng có bị giới hạn ở phạm vi tối thiểu cần thiết trong backend của dịch vụ này không?"**
> 3.  **"Đối với dịch vụ bên ngoài này, chúng ta có kế hoạch suy giảm trong trường hợp dịch vụ bị lỗi hoặc bị xâm phạm không (ví dụ: tạm thời tắt các tính năng liên quan, hiển thị thông báo bảo trì)?"**