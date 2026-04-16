# Hướng Dẫn Deploy Jewelry Store Management System lên Render

## Yêu Cầu
- GitHub repository (public hoặc private)
- Tài khoản Render (https://render.com)
- SQL Server database (có thể sử dụng Azure SQL, AWS RDS, hoặc database online khác)

## Các Bước Deploy

### 1. Chuẩn Bị Repository
```bash
# Đảm bảo các file sau đã được thêm vào repository:
# - Dockerfile.txt (hoặc rename thành Dockerfile)
# - .dockerignore
# - render.yaml
# - src/main/resources/application-prod.properties
# - pom.xml

git add .
git commit -m "Configure for Render deployment"
git push origin main
```

### 2. Rename Dockerfile (Tùy Chọn)
Nếu Render không nhận Dockerfile.txt, rename thành `Dockerfile`:
```bash
mv Dockerfile.txt Dockerfile
```

### 3. Đăng Nhập Render & Tạo Web Service
1. Truy cập https://render.com
2. Login với GitHub
3. Click "New +" > "Web Service"
4. Chọn repository của bạn
5. Điền thông tin:
   - **Name**: jewelry-store-management
   - **Environment**: Docker
   - **Plan**: Free (hoặc Pro nếu cần)
   - **Region**: Singapore (hoặc gần bạn nhất)
   - **Branch**: main
   - **Dockerfile Path**: ./Dockerfile hoặc ./Dockerfile.txt

### 4. Cấu Hình Environment Variables

Trong Render, thêm các environment variables sau:

```
SPRING_PROFILES_ACTIVE=prod
DB_URL=jdbc:sqlserver://<your-sql-server>:1433;databaseName=<db-name>
DB_USERNAME=<username>
DB_PASSWORD=<password>
MAIL_HOST=smtp.gmail.com (hoặc email provider khác)
MAIL_PORT=587
MAIL_USERNAME=<your-email>
MAIL_PASSWORD=<app-password>
SERVER_PORT=8080
```

### 5. Deploy
1. Click "Create Web Service"
2. Render sẽ tự động build và deploy
3. Xem logs trong tab "Logs" để kiểm tra tiến độ

## Troubleshooting

### Lỗi Build
- Kiểm tra file `pom.xml` có đúng không
- Đảm bảo `pom.xml` có `<packaging>jar</packaging>` (mặc định cho Spring Boot)

### Lỗi Runtime
- Kiểm tra `application-prod.properties` có tất cả biến môi trường cần thiết
- Đảm bảo database URL, username, password là chính xác
- Kiểm tra logs trong Render dashboard

### Port Issues
- Render sẽ tự động set `PORT` environment variable
- Application đang sử dụng `server.port=8080` sẽ hoạt động tốt

## Tối Ưu Thêm

### Sử dụng Hệ Thống Cơ Sở Dữ Liệu Miễn Phí
- **Azure SQL**: https://azure.microsoft.com/free/
- **AWS RDS Free Tier**: https://aws.amazon.com/free/
- **Clever Cloud**: https://www.clever-cloud.com/ (hỗ trợ SQL Server)

### Tối Ưu Build Time
- Cập nhật `pom.xml` để loại bỏ dependencies không cần thiết
- Sử dụng Maven cache để tăng tốc độ build

### Giám Sát Ứng Dụng
- Sử dụng Render's built-in monitoring
- Cấu hình logging level cho phù hợp
- Kiểm tra logs thường xuyên

## Links Hữu Ích
- Render Documentation: https://render.com/docs
- Spring Boot on Render: https://render.com/docs/deploy-spring-boot
- Docker Best Practices: https://docs.docker.com/develop/dev-best-practices/
