# Hotel Booking – Quản lý đặt phòng khách sạn trực tuyến

Đồ án môn **Phát triển ứng dụng web mã nguồn mở**. Website cho phép khách xem loại phòng, tra cứu phòng trống và đặt phòng online; quản trị viên quản lý phòng, đơn đặt phòng và thanh toán.
## Nhóm thực hiện
### Thành viên nhóm
- Đinh Hữu Nhân - 110123032 - DA23TTA
- Huỳnh Tân Khoa - 110123233- DA23TTA
- Lê Trung Hiếu - 110123011 - DA23TTA

## Công nghệ
- **CodeIgniter 4** (PHP 8.2, Apache) – đảm nhận cả back-end và front-end (View)
- **MySQL 8.0** – service riêng, tự động tạo schema + dữ liệu mẫu
- **Docker & Docker Compose**, cấu hình qua file `.env`
- phpMyAdmin để quản trị CSDL

## Cấu trúc thư mục
```
hotel-booking/
├── app/
│   ├── Config/Routes.php      # Định tuyến
│   ├── Controllers/           # Home, Rooms, ...
│   ├── Models/                # RoomType, Room, User, Booking
│   └── Views/                 # layouts/, home/, rooms/
├── public/                    # Document root (assets, index.php)
├── docker/
│   ├── php/                   # Dockerfile + entrypoint
│   └── mysql/init/            # 01_schema.sql, 02_seed.sql
├── docker-compose.yml
├── .env.example               # Mẫu biến môi trường
└── README.md
```
(Các thư mục còn lại như `system`, `vendor`, `spark`, `composer.json`... được tự sinh ở lần chạy đầu.)

## Hướng dẫn cài đặt
```bash
cp .env.example .env     
docker compose up -d --build
```
Lần đầu cần chờ vài phút để tải CodeIgniter 4 (xem tiến trình: `docker compose logs -f app`).

| Dịch vụ | Địa chỉ |
|---|---|
| Website | http://localhost:8080 |
| MySQL (từ máy host) | `localhost:3307` |

Nạp lại dữ liệu từ đầu (xóa toàn bộ dữ liệu cũ):
```bash
docker compose down -v && docker compose up -d
```

# Phân công công việc

> ☑ = được giao, ☐ = không tham gia. Việc chung: tích cả 3 ô; việc cá nhân: tích 1 ô.

## Tổng quan các giai đoạn

| Giai đoạn | Tên giai đoạn | Mục tiêu |
|:---:|---|---|
| 1 | Khởi tạo và thiết lập nền tảng | Dự án chạy được bằng Docker, kết nối MySQL, có giao diện trang chủ. |
| 2 | Phân tích và thiết kế hệ thống | Có đầy đủ tài liệu phân tích, mô hình ERD, DFD và thiết kế giao diện. |
| 3 | Xây dựng chức năng người dùng | Khách hàng đăng ký, tìm phòng trống, đặt phòng, thanh toán và hủy được. |
| 4 | Xây dựng chức năng quản trị | Quản trị viên quản lý phòng, đơn đặt phòng, khách hàng, thanh toán và xem báo cáo. |
| 5 | Kiểm thử, bảo mật và hoàn thiện | Hệ thống ổn định, an toàn, giao diện thích ứng nhiều thiết bị. |
| 6 | Đóng gói, tài liệu và báo cáo | Dự án cài đặt được từ đầu, có tài liệu và sẵn sàng bảo vệ. |

## Giai đoạn 1: Khởi tạo và thiết lập nền tảng

*Mục tiêu: Dự án chạy được bằng Docker, kết nối MySQL, có giao diện trang chủ.*

| STT | Nhóm công việc | Công việc cụ thể | Kết quả đầu ra | Đinh Hữu Nhân | Huỳnh Tân Khoa | Lê Trung Hiếu |
|:---:|---|---|---|:---:|:---:|:---:|
| 1 | **1. Khởi tạo cấu trúc dự án** | 1.1. Họp nhóm thống nhất công nghệ, cấu trúc thư mục và quy ước đặt tên | Biên bản thống nhất của cả nhóm. | ☑ | ☑ | ☑ |
| 2 |  | 1.2. Tạo cấu trúc thư mục `app/`, `public/`, `docker/`, `mysql/` theo chuẩn MVC | Cấu trúc dự án sẵn sàng phát triển. | ☐ | ☐ | ☑ |
| 3 |  | 1.3. Cấu hình `.gitignore`, `.env.example` | Không đẩy file nhạy cảm lên repo, có file mẫu biến môi trường. | ☐ | ☐ | ☑ |
| 4 | **2. Thiết lập Docker** | 2.1. Viết `docker-compose.yml` (service app, db, phpMyAdmin; cổng, volume, network) | Các service khai báo đúng cổng và volume. | ☑ | ☐ | ☐ |
| 5 |  | 2.2. Viết `docker/php/Dockerfile`, `docker/php/entrypoint.sh`, `docker/php/php.ini` | Môi trường PHP 8.2 + Apache chạy ổn định. | ☑ | ☐ | ☐ |
| 6 |  | 2.3. Chạy thử Docker trên máy cá nhân | Mỗi thành viên chạy được dự án trên máy mình. | ☑ | ☑ | ☑ |
| 7 | **3. Cơ sở dữ liệu ban đầu** | 3.1. Xác định các bảng chính, quan hệ, khóa và dữ liệu mẫu ban đầu | Bản nháp thiết kế CSDL. | ☑ | ☐ | ☐ |
| 8 |  | 3.2. Thảo luận và thống nhất thiết kế CSDL | Thiết kế CSDL được cả nhóm đồng thuận. | ☑ | ☑ | ☑ |
| 9 |  | 3.3. Viết `docker/mysql/init/01_schema.sql` (users, room_types, rooms, bookings, payments) | Schema tạo đúng theo thiết kế đã đồng thuận. | ☐ | ☑ | ☐ |
| 10 |  | 3.4. Viết dữ liệu mẫu `docker/mysql/init/02_seed.sql` | Dữ liệu mẫu tự nạp khi chạy Docker lần đầu. | ☐ | ☑ | ☐ |
| 11 | **4. Cấu hình CodeIgniter 4** | 4.1. Phân tích kiến trúc MVC, luồng xử lý request, quy tắc đặt tên và tổ chức module | Tài liệu mô tả kiến trúc dự án. | ☐ | ☐ | ☑ |
| 12 |  | 4.2. Cấu hình `app/Config/Database.php` và `.env` | Ứng dụng kết nối MySQL thành công. | ☐ | ☐ | ☑ |
| 13 |  | 4.3. Cấu hình `app/Config/Routes.php` | Định tuyến hệ thống hoạt động. | ☐ | ☐ | ☑ |
| 14 | **5. Giao diện trang chủ** | 5.1. Dựng layout `app/Views/layouts/` (header, footer, navigation) | Bố cục chung dùng lại cho mọi trang. | ☑ | ☐ | ☐ |
| 15 |  | 5.2. Làm trang chủ `app/Views/home/` và CSS trong `public/assets/` | Trang chủ rõ ràng, giao diện thân thiện. | ☑ | ☐ | ☐ |
| 16 | **6. Nghiệm thu giai đoạn 1** | 6.1. Chạy thử toàn bộ hệ thống, góp ý và chỉnh sửa chung | Giai đoạn 1 hoàn thành, sẵn sàng sang giai đoạn 2. | ☑ | ☑ | ☑ |

## Giai đoạn 2: Phân tích và thiết kế hệ thống

*Mục tiêu: Có đầy đủ tài liệu phân tích, mô hình ERD, DFD và thiết kế giao diện.*

| STT | Nhóm công việc | Công việc cụ thể | Kết quả đầu ra | Đinh Hữu Nhân | Huỳnh Tân Khoa | Lê Trung Hiếu |
|:---:|---|---|---|:---:|:---:|:---:|
| 17 | **7. Khảo sát và phân tích yêu cầu** | 7.1. Khảo sát nghiệp vụ đặt phòng khách sạn (quy trình đặt, hủy, thanh toán, trạng thái phòng) | Bản mô tả nghiệp vụ. | ☑ | ☑ | ☑ |
| 18 |  | 7.2. Xác định tác nhân (khách, quản trị viên) và danh sách chức năng của từng tác nhân | Danh sách chức năng đã thống nhất. | ☐ | ☐ | ☑ |
| 19 |  | 7.3. Viết đặc tả yêu cầu chức năng và phi chức năng | Tài liệu đặc tả yêu cầu. | ☐ | ☐ | ☑ |
| 20 | **8. Mô hình hóa hệ thống** | 8.1. Vẽ sơ đồ Use Case tổng quát và theo từng tác nhân | Sơ đồ Use Case. | ☐ | ☐ | ☑ |
| 21 |  | 8.2. Phân tích đối tượng: xác định lớp, thuộc tính, phương thức và quan hệ | Sơ đồ lớp và bảng mô tả đối tượng. | ☑ | ☐ | ☐ |
| 22 |  | 8.3. Vẽ sơ đồ luồng dữ liệu DFD (mức 0, mức 1) | Bộ sơ đồ DFD. | ☑ | ☐ | ☐ |
| 23 |  | 8.4. Vẽ mô hình ERD | Sơ đồ ERD hoàn chỉnh. | ☐ | ☑ | ☐ |
| 24 |  | 8.5. Vẽ sơ đồ tuần tự cho luồng đặt phòng và hủy phòng | Sơ đồ tuần tự. | ☐ | ☐ | ☑ |
| 25 | **9. Thiết kế cơ sở dữ liệu chi tiết** | 9.1. Chuyển ERD sang lược đồ quan hệ, chuẩn hóa đến 3NF | Lược đồ quan hệ và từ điển dữ liệu. | ☐ | ☑ | ☐ |
| 26 |  | 9.2. Cập nhật schema và seed theo thiết kế (ràng buộc, khóa ngoại, index) | File SQL khớp với thiết kế cuối cùng. | ☐ | ☑ | ☐ |
| 27 |  | 9.3. Review thiết kế cơ sở dữ liệu | CSDL được cả nhóm chốt. | ☑ | ☑ | ☑ |
| 28 | **10. Thiết kế giao diện** | 10.1. Vẽ wireframe các trang chính (trang chủ, danh sách và chi tiết phòng, đặt phòng, đăng nhập, quản trị) | Bộ wireframe. | ☑ | ☐ | ☐ |
| 29 |  | 10.2. Thống nhất bảng màu, font chữ và thành phần giao diện dùng chung | Quy chuẩn giao diện. | ☑ | ☐ | ☐ |
| 30 |  | 10.3. Họp review và chốt toàn bộ tài liệu phân tích thiết kế | Giai đoạn 2 hoàn thành. | ☑ | ☑ | ☑ |

## Giai đoạn 3: Xây dựng chức năng người dùng

*Mục tiêu: Khách hàng đăng ký, tìm phòng trống, đặt phòng, thanh toán và hủy được.*

| STT | Nhóm công việc | Công việc cụ thể | Kết quả đầu ra | Đinh Hữu Nhân | Huỳnh Tân Khoa | Lê Trung Hiếu |
|:---:|---|---|---|:---:|:---:|:---:|
| 31 | **11. Xác thực và tài khoản** | 11.1. Viết `UserModel` và quy tắc validation đăng ký (email duy nhất, mật khẩu tối thiểu 8 ký tự) | Model và validation hoạt động. | ☐ | ☑ | ☐ |
| 32 |  | 11.2. Viết controller Auth: đăng ký, đăng nhập, đăng xuất, session, `password_hash` | Chức năng xác thực hoạt động. | ☐ | ☐ | ☑ |
| 33 |  | 11.3. Viết Filter phân quyền (đăng nhập, admin) và gắn vào Routes | Trang bị hạn chế truy cập đúng vai trò. | ☐ | ☐ | ☑ |
| 34 |  | 11.4. Làm giao diện đăng ký và đăng nhập | Hai trang giao diện hoàn chỉnh. | ☑ | ☐ | ☐ |
| 35 |  | 11.5. Làm trang hồ sơ cá nhân và đổi mật khẩu | Khách tự cập nhật được thông tin. | ☐ | ☑ | ☐ |
| 36 | **12. Xem và tìm kiếm phòng** | 12.1. Viết `RoomTypeModel`, `RoomModel`: danh sách, chi tiết, đếm phòng còn trống | Model truy vấn đúng dữ liệu. | ☐ | ☑ | ☐ |
| 37 |  | 12.2. Viết controller Rooms: danh sách có lọc theo giá, sức chứa và phân trang | Trang danh sách phòng có lọc. | ☐ | ☐ | ☑ |
| 38 |  | 12.3. Làm giao diện danh sách và chi tiết loại phòng | Hai trang giao diện hoàn chỉnh. | ☑ | ☐ | ☐ |
| 39 |  | 12.4. Viết chức năng tìm phòng trống theo ngày nhận, ngày trả, số khách (loại trừ phòng trùng lịch và đang bảo trì) | Kết quả tìm phòng trống chính xác. | ☐ | ☐ | ☑ |
| 40 |  | 12.5. Làm form tìm kiếm trên trang chủ | Form tìm phòng hoạt động. | ☑ | ☐ | ☐ |
| 41 | **13. Đặt phòng** | 13.1. Viết `BookingModel`: sinh mã đặt phòng, tính tổng tiền theo số đêm | Tạo được đơn đặt phòng với giá đúng. | ☐ | ☑ | ☐ |
| 42 |  | 13.2. Viết controller Bookings: xác nhận thông tin, kiểm tra trùng lịch khi lưu (dùng transaction) | Không xảy ra đặt trùng phòng. | ☐ | ☐ | ☑ |
| 43 |  | 13.3. Làm giao diện form đặt phòng và trang xác nhận | Luồng đặt phòng hoàn chỉnh trên giao diện. | ☑ | ☐ | ☐ |
| 44 |  | 13.4. Làm trang lịch sử và chi tiết đơn đặt phòng của khách | Khách xem lại được các đơn của mình. | ☐ | ☑ | ☐ |
| 45 |  | 13.5. Viết chức năng hủy đặt phòng (chỉ khi đơn chưa nhận phòng) | Hủy đơn đúng điều kiện. | ☐ | ☐ | ☑ |
| 46 | **14. Thanh toán** | 14.1. Viết `PaymentModel` và ghi nhận thanh toán (tiền mặt, chuyển khoản, thẻ) | Lưu được thông tin thanh toán theo đơn. | ☐ | ☑ | ☐ |
| 47 |  | 14.2. Làm giao diện chọn phương thức thanh toán và hóa đơn | Trang thanh toán và hóa đơn hoàn chỉnh. | ☑ | ☐ | ☐ |
| 48 | **15. Nghiệm thu chức năng người dùng** | 15.1. Chạy thử toàn bộ luồng: đăng ký, tìm phòng, đặt, thanh toán, hủy và sửa lỗi phát sinh | Giai đoạn 3 hoàn thành. | ☑ | ☑ | ☑ |

## Giai đoạn 4: Xây dựng chức năng quản trị

*Mục tiêu: Quản trị viên quản lý phòng, đơn đặt phòng, khách hàng, thanh toán và xem báo cáo.*

| STT | Nhóm công việc | Công việc cụ thể | Kết quả đầu ra | Đinh Hữu Nhân | Huỳnh Tân Khoa | Lê Trung Hiếu |
|:---:|---|---|---|:---:|:---:|:---:|
| 49 | **16. Khung quản trị và thống kê** | 16.1. Làm layout trang quản trị (sidebar, header) | Khung giao diện quản trị dùng chung. | ☑ | ☐ | ☐ |
| 50 |  | 16.2. Viết dashboard thống kê: số đơn, doanh thu, tỷ lệ phòng được đặt | Dashboard hiển thị số liệu đúng. | ☐ | ☐ | ☑ |
| 51 | **17. Quản lý phòng** | 17.1. Viết CRUD loại phòng (kèm tải ảnh lên) | Thêm, sửa, xóa loại phòng. | ☐ | ☑ | ☐ |
| 52 |  | 17.2. Viết CRUD phòng (số phòng, tầng, trạng thái) | Thêm, sửa, xóa phòng. | ☐ | ☑ | ☐ |
| 53 |  | 17.3. Làm giao diện danh sách và form quản lý phòng | Giao diện quản lý phòng hoàn chỉnh. | ☑ | ☐ | ☐ |
| 54 | **18. Quản lý đặt phòng** | 18.1. Viết danh sách đơn đặt phòng, lọc theo trạng thái và ngày | Tra cứu được đơn. | ☐ | ☐ | ☑ |
| 55 |  | 18.2. Viết xử lý xác nhận đơn, nhận phòng, trả phòng, hủy đơn | Trạng thái đơn đổi đúng quy trình. | ☐ | ☐ | ☑ |
| 56 |  | 18.3. Làm giao diện quản lý đơn đặt phòng | Giao diện quản lý đơn hoàn chỉnh. | ☑ | ☐ | ☐ |
| 57 | **19. Quản lý khách hàng và thanh toán** | 19.1. Viết danh sách khách hàng, khóa và mở khóa tài khoản | Quản lý được tài khoản khách. | ☐ | ☑ | ☐ |
| 58 |  | 19.2. Viết quản lý thanh toán, cập nhật trạng thái thanh toán | Theo dõi được các khoản thu. | ☐ | ☑ | ☐ |
| 59 |  | 19.3. Làm giao diện quản lý khách hàng và thanh toán | Giao diện hoàn chỉnh. | ☑ | ☐ | ☐ |
| 60 | **20. Báo cáo** | 20.1. Viết báo cáo doanh thu theo ngày, tháng | Báo cáo số liệu đúng. | ☐ | ☐ | ☑ |
| 61 |  | 20.2. Làm biểu đồ doanh thu trên giao diện | Biểu đồ trực quan. | ☑ | ☐ | ☐ |
| 62 |  | 20.3. Viết chức năng xuất báo cáo ra file CSV | Tải được file báo cáo. | ☐ | ☑ | ☐ |
| 63 | **21. Nghiệm thu chức năng quản trị** | 21.1. Chạy thử toàn bộ chức năng quản trị và sửa lỗi phát sinh | Giai đoạn 4 hoàn thành. | ☑ | ☑ | ☑ |

## Giai đoạn 5: Kiểm thử, bảo mật và hoàn thiện

*Mục tiêu: Hệ thống ổn định, an toàn, giao diện thích ứng nhiều thiết bị.*

| STT | Nhóm công việc | Công việc cụ thể | Kết quả đầu ra | Đinh Hữu Nhân | Huỳnh Tân Khoa | Lê Trung Hiếu |
|:---:|---|---|---|:---:|:---:|:---:|
| 64 | **22. Kiểm thử** | 22.1. Lập test case và kiểm thử chức năng phía người dùng | Bảng kết quả kiểm thử. | ☑ | ☐ | ☐ |
| 65 |  | 22.2. Lập test case và kiểm thử chức năng quản trị | Bảng kết quả kiểm thử. | ☐ | ☑ | ☐ |
| 66 |  | 22.3. Kiểm thử logic đặt phòng (trùng lịch, ngày biên, hủy phòng) | Không còn lỗi logic nghiêm trọng. | ☐ | ☐ | ☑ |
| 67 |  | 22.4. Tổng hợp lỗi và sửa lỗi | Danh sách lỗi đã xử lý. | ☑ | ☑ | ☑ |
| 68 | **23. Bảo mật** | 23.1. Bật CSRF, escape dữ liệu hiển thị bằng `esc()`, kiểm tra dữ liệu đầu vào | Chống CSRF và XSS. | ☐ | ☐ | ☑ |
| 69 |  | 23.2. Kiểm tra phân quyền admin/khách, dùng Query Builder để chống SQL Injection | Không truy cập trái quyền được. | ☐ | ☑ | ☐ |
| 70 |  | 23.3. Bảo vệ biến môi trường, đặt `CI_ENVIRONMENT=production` khi triển khai | Không lộ thông tin cấu hình và lỗi chi tiết. | ☑ | ☐ | ☐ |
| 71 | **24. Hoàn thiện giao diện và hiệu năng** | 24.1. Làm giao diện responsive cho điện thoại và máy tính bảng | Hiển thị tốt trên nhiều kích thước. | ☑ | ☐ | ☐ |
| 72 |  | 24.2. Làm trang lỗi 404/500 và thông báo (flash message) | Thông báo thân thiện với người dùng. | ☑ | ☐ | ☐ |
| 73 |  | 24.3. Tối ưu truy vấn và thêm index cho CSDL | Truy vấn nhanh hơn. | ☐ | ☑ | ☐ |

## Giai đoạn 6: Đóng gói, tài liệu và báo cáo

*Mục tiêu: Dự án cài đặt được từ đầu, có tài liệu và sẵn sàng bảo vệ.*

| STT | Nhóm công việc | Công việc cụ thể | Kết quả đầu ra | Đinh Hữu Nhân | Huỳnh Tân Khoa | Lê Trung Hiếu |
|:---:|---|---|---|:---:|:---:|:---:|
| 74 | **25. Đóng gói** | 25.1. Hoàn thiện cấu hình Docker (healthcheck, `php.ini`) | Docker ổn định. | ☑ | ☐ | ☐ |
| 75 |  | 25.2. Bổ sung dữ liệu mẫu đầy đủ để demo | Dữ liệu demo đủ các trường hợp. | ☐ | ☑ | ☐ |
| 76 |  | 25.3. Cài đặt thử từ đầu bằng `docker compose down -v && docker compose up -d --build` | Cài mới chạy được ngay. | ☑ | ☑ | ☑ |
| 77 | **26. Tài liệu** | 26.1. Hoàn thiện README và hướng dẫn cài đặt | README đầy đủ. | ☐ | ☐ | ☑ |
| 78 |  | 26.2. Tổng hợp tài liệu phân tích thiết kế (Use Case, ERD, DFD, từ điển dữ liệu) | Bộ tài liệu thiết kế. | ☐ | ☑ | ☐ |
| 79 |  | 26.3. Viết hướng dẫn sử dụng cho khách và quản trị viên | Tài liệu hướng dẫn sử dụng. | ☑ | ☐ | ☐ |
| 80 | **27. Báo cáo và bảo vệ** | 27.1. Viết báo cáo đồ án (chia chương cho từng thành viên) | Báo cáo hoàn chỉnh. | ☑ | ☑ | ☑ |
| 81 |  | 27.2. Làm slide thuyết trình | Bộ slide. | ☑ | ☐ | ☐ |
| 82 |  | 27.3. Chuẩn bị kịch bản demo và tập thuyết trình | Sẵn sàng bảo vệ. | ☑ | ☑ | ☑ |
