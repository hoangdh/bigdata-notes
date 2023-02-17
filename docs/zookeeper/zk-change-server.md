## Các bước thay thế server ZK

### Ngữ cảnh:

Do chuyển đổi hạ tầng, 01 máy chủ ZK bị thu hồi hoặc máy chủ bị 'khai tử' bất thình lình. Ta được cấp một server thay thế nó. Dưới đây là các bước thực hiện.

### Các bước thực hiện

- Cài đặt môi trường trên Server mới (Java, Zookeeper)
- Cấu hình trên server mới (Địa chỉ và ID của server mới)
- Chỉnh cấu hình trên các server được giữ lại
- Dừng server cũ
- Bật Server mới
- Khởi động lại từng server cũ

**Chú ý**: Trong lúc thực hiện, xem log của ZK để năm được vấn đề nếu có phát sinh.
