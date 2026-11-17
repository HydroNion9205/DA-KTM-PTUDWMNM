# 🏨 Hotel Booking – Quản lý đặt phòng khách sạn trực tuyến

Đồ án môn **Phát triển ứng dụng web mã nguồn mở**. Website cho phép khách xem loại phòng, tra cứu phòng trống và đặt phòng online; quản trị viên quản lý phòng, đơn đặt phòng và thanh toán.

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

## Chạy dự án
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
