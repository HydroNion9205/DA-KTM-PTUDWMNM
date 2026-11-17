# 🏨 Hotel Booking – Quản lý đặt phòng khách sạn trực tuyến

Đồ án môn **Phát triển ứng dụng web mã nguồn mở**. Website cho phép khách xem loại phòng, tra cứu phòng trống và đặt phòng online; quản trị viên quản lý phòng, đơn đặt phòng và thanh toán.
## Nhóm thực hiện
### Thành viên nhóm
- Đinh Hữu Nhân - 110123032 - DA23TTA
- Huỳnh Tân Khoa - DA23TTA
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

# Phân công

## Giai đoạn 1: Khởi tạo và thiết lập nền tảng

| STT | Công việc | Người phụ trách | File / thư mục thực hiện | Yêu cầu đầu ra |
|---:|---|---|---|---|
| 1 | Khởi tạo cấu trúc dự án | Lê Trung Hiếu | `app/`, `public/`, `docker/`, `mysql/` | Tạo cấu trúc dự án theo chuẩn MVC, sẵn sàng phát triển. |
| 2 | Thiết lập Docker | Đinh Hữu Nhân | `docker-compose.yml`, `docker/php/Dockerfile`, `docker/php/php.ini` | Môi trường PHP + MySQL chạy ổn định, có cổng và volume đúng cấu hình. |
| 3 | Khởi tạo cơ sở dữ liệu | Huỳnh Tân Khoa | `docker/mysql/01_schema.sql`, `database/schema.sql`, `database/seeders/` | Xác định các bảng chính, quan hệ, khóa và dữ liệu mẫu ban đầu. |
| 4 | Phân tích cấu trúc dự án | Lê Trung Hiếu | `docs/project-structure.md`, `docs/architecture.md` | Mô tả rõ kiến trúc MVC, luồng xử lý, quy tắc đặt tên và tổ chức module. |
| 5 | Thiết lập cấu hình CodeIgniter | Lê Trung Hiếu | `app/Config/Database.php`, `app/Config/Routes.php`, `.env` | Kết nối ứng dụng với MySQL thành công và định tuyến hệ thống hoạt động. |
| 6 | Thiết kế giao diện trang chủ | Đinh Hữu Nhân | `app/Views/layouts/`, `app/Views/home/`, `public/assets/` | Trang chủ có bố cục rõ ràng, header, footer, navigation và giao diện thân thiện. |

## Ghi chú:
- Mọi thành viên có thể liên lạc nhau để họp và bàn luận trước khi thực hiện công việc