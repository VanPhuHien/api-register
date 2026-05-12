# Backend Registration Module - MinLish Project

## Thông tin sinh viên
- **Họ và tên:** Văn Phú Hiền
- **MSSV:** 23110213

## Tổng quan
Module này chịu trách nhiệm xử lý luồng đăng ký người dùng mới cho ứng dụng MinLish, bao gồm xác thực dữ liệu đầu vào, bảo mật thông tin và xác thực danh tính qua email (OTP).

## Công nghệ sử dụng
- **Runtime:** Node.js
- **Framework:** Express.js
- **Database:** MySQL (Sequelize ORM)
- **Security:** bcrypt (hashing), express-rate-limit (chống brute-force)
- **Communication:** Nodemailer (gửi mã OTP qua email)

## Quy trình Đăng ký (Workflow)
1. **Khởi tạo Đăng ký:**
   - Người dùng gửi thông tin: `username`, `email`, `password`.
   - Hệ thống kiểm tra trùng lặp email/username trong database.
   - Mật khẩu được mã hóa (hash) bằng `bcrypt` trước khi lưu trữ.
   - Tài khoản được khởi tạo với trạng thái `INACTIVE` (chưa kích hoạt).
   
2. **Xác thực OTP:**
   - Hệ thống tạo mã OTP ngẫu nhiên gồm 6 chữ số, có thời hạn 10 phút.
   - Mã OTP được gửi đến email người dùng thông qua dịch vụ Mail.
   
3. **Kích hoạt Tài khoản:**
   - Người dùng nhập mã OTP nhận được từ email.
   - Hệ thống kiểm tra tính hợp lệ và thời hạn của mã.
   - Nếu chính xác, trạng thái tài khoản chuyển sang `ACTIVE`, cho phép người dùng đăng nhập.

## API Endpoints
| Phương thức | Endpoint | Mô tả | Bảo mật |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/v1/auth/register` | Đăng ký tài khoản mới | Rate limit (3 req/giờ) |
| `POST` | `/api/v1/auth/verify-otp` | Xác thực OTP & kích hoạt | Validation Joi/Zod |

## Các tính năng bảo mật tích hợp
- **Rate Limiting:** Giới hạn số lần đăng ký để tránh spam tài khoản.
- **Data Validation:** Đảm bảo dữ liệu đầu vào đúng định dạng (email hợp lệ, mật khẩu đủ mạnh).
- **Password Hashing:** Không lưu mật khẩu dạng văn bản thuần túy.
- **OTP Expiration:** Mã xác thực tự động hết hiệu lực sau một khoảng thời gian ngắn để đảm bảo an toàn.
