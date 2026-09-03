# 📖 HƯỚNG DẪN KẾT NỐI HỆ THỐNG & DỰ ÁN ĐĂNG BÀI FACEBOOK

Tài liệu này lưu trữ toàn bộ thông số kỹ thuật, cấu hình mạng và hướng dẫn vận hành để phục vụ việc kết nối, bảo trì và phát triển dự án ở các phiên làm việc tiếp theo.

---

## 🖥️ 1. Thông Tin Kết Nối Máy Bàn (Windows PC)

* **Thiết bị:** `thuypc` (Hệ điều hành: Windows 10/11)
* **Mạng nội bộ Tailscale IP:** `100.97.77.69` (hoặc domain MagicDNS: `thuypc.tail59384d.ts.net`)
* **Cổng SSH:** `22` (OpenSSH Server đã bật)
* **Tài khoản Windows (User):** `ICTSAIGON`
* **Mật khẩu Windows (Password):** `ta123`

### 🔹 Cách kết nối SSH từ máy Mac / Linux:
```bash
ssh ICTSAIGON@100.97.77.69
# Nhập mật khẩu: ta123
```

---

## 🐳 2. Dịch Vụ Docker & n8n trên Máy Bàn

* **Container Docker:** `n8n` (Image: `n8nio/n8n`)
* **Volume lưu trữ dữ liệu workflow:** `n8n_data`
* **Cổng nội bộ:** `5678`
* **Đường dẫn truy cập trực tiếp qua Tailscale:** `http://100.97.77.69:5678`
* **Tình trạng tự khởi động:** Container được thiết lập `--restart unless-stopped`. Khi máy bàn bật Docker Desktop, n8n sẽ tự động chạy.

---

## 🌐 3. Cấu Hình Ngrok Tĩnh 24/7 (Windows Service)

Ngrok đã được cài đặt thành một **Windows Service ngầm vĩnh viễn**, tự động chạy cùng Windows mỗi khi mở máy bàn:

* **Tên miền Ngrok cố định:** `https://supplier-gloss-neatly.ngrok-free.dev`
* **Ngrok Authtoken:** `3IZGoEP6Qf3J5rS3fQTELPXDIZm_3Vy4J6T22Fo5Fom97LDYS`
* **Thư mục cài đặt trên PC:** `C:\ngrok\ngrok.exe`
* **File cấu hình:** `C:\ngrok\ngrok.yml`
* **Tên Service trong Windows:** `ngrok`

### 🔹 Nội dung file cấu hình `C:\ngrok\ngrok.yml`:
```yaml
version: "3"
agent:
  authtoken: 3IZGoEP6Qf3J5rS3fQTELPXDIZm_3Vy4J6T22Fo5Fom97LDYS
tunnels:
  n8n:
    proto: http
    addr: http://100.97.77.69:5678
    url: https://supplier-gloss-neatly.ngrok-free.dev
```

### 🔹 Lệnh kiểm tra / quản lý Service qua PowerShell máy bàn:
```powershell
# Kiểm tra trạng thái
Get-Service ngrok

# Khởi động lại service nếu cần
Restart-Service ngrok
```

---

## 🔗 4. Danh Sách Webhook n8n Nhận Dữ Liệu

1. **Fanpage Thạch Cao Tâm An:**
   * `https://supplier-gloss-neatly.ngrok-free.dev/webhook/dang-bai-fb`
2. **Fanpage Dunvex Software:**
   * `https://supplier-gloss-neatly.ngrok-free.dev/webhook/dang-bai-dunvex`

---

## 📱 5. Web App Giao Diện Đăng Bài

* **Trang web chính thức:** [https://dang-bai-fb-app.vercel.app](https://dang-bai-fb-app.vercel.app)
* **Repository GitHub:** [https://github.com/nguyenthong123/dang-bai-fb-app](https://github.com/nguyenthong123/dang-bai-fb-app)
* **Tính năng:**
  * Hỗ trợ tải lên **cùng lúc nhiều hình ảnh** (JPG, PNG, WEBP) với khung lưới xem trước (Thumbnail gallery grid).
  * Hỗ trợ tự động ghép banner logo thương hiệu và nén video chuẩn HD.
  * Phân quyền tài khoản (Super Admin & thành viên được duyệt).
  * Tự động gửi dữ liệu và hình ảnh đến máy chủ n8n trên máy bàn.

---

## 🤖 6. Hướng Dẫn Dành Cho AI / Kỹ Thuật Viên Ở Các Phiên Sau

Khi mở phiên làm việc mới, để kiểm tra nhanh toàn bộ hệ thống:

1. **Kiểm tra kết nối Tailscale tới máy bàn:**
   ```bash
   curl -I -m 5 http://100.97.77.69:5678/healthz
   ```
2. **Kiểm tra đường truyền Ngrok ra Internet:**
   ```bash
   curl -I -m 5 https://supplier-gloss-neatly.ngrok-free.dev/healthz
   ```
3. **Nếu cần SSH vào máy bàn chạy lệnh:**
   * Dùng lệnh `ssh ICTSAIGON@100.97.77.69` (Password: `ta123`).
   * Sử dụng PowerShell hoặc CMD để thao tác với Docker và Ngrok service.
