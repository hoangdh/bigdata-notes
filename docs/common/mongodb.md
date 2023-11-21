# MongoDB

## Giới thiệu MongoDB

MongoDB là cơ sở dữ liệu document, được sử dụng để xây dựng các ứng dụng internet có khả năng mở rộng và tính sẵn sàng cao. Với cách tiếp cận schema linh hoạt, MongoDB rất phổ biến với các nhóm phát triển sử dụng các phương pháp linh hoạt. 

MongoDB là cơ sở dữ liệu document mã nguồn mở, được xây dựng trên kiến trúc mở rộng theo chiều ngang, sử dụng schema linh hoạt để lưu trữ dữ liệu. Được thành lập năm 2007, MongoDB có cộng đồng người dùng phát triển trên toàn thế giới.

Trong MongoDB, các bản ghi được lưu trữ dưới dạng tài liệu trong các tệp BSON được nén. Các tài liệu có thể được truy xuất trực tiếp ở định dạng JSON, có nhiều lợi ích:

- Đây là một dạng tự nhiên để lưu trữ dữ liệu.  
- Dễ đọc đối với con người.
- Thông tin có cấu trúc và phi cấu trúc có thể được lưu trữ trong cùng một tài liệu.
- Bạn có thể lồng JSON để lưu trữ các đối tượng dữ liệu phức tạp.
- JSON có schema linh hoạt và động, vì vậy việc thêm trường hoặc bỏ qua một trường không phải là vấn đề.
- Tài liệu ánh xạ đến các đối tượng trong hầu hết các ngôn ngữ lập trình phổ biến.

## Tại sao nên sử dụng MongoDB?

MongoDB được xây dựng dựa trên kiến trúc mở rộng theo chiều ngang đã trở nên phổ biến với các nhà phát triển để phát triển các ứng dụng có khả năng mở rộng với các schema dữ liệu đang phát triển.

Là cơ sở dữ liệu dạng tài liệu, MongoDB giúp các nhà phát triển dễ dàng lưu trữ dữ liệu có cấu trúc hoặc phi cấu trúc. Nó sử dụng định dạng JSON để lưu trữ các tài liệu. Định dạng này ánh xạ trực tiếp đến các đối tượng native trong hầu hết các ngôn ngữ lập trình hiện đại, khiến nó trở thành một lựa chọn tự nhiên cho các nhà phát triển, vì họ không cần phải suy nghĩ về chuẩn hóa dữ liệu. 

MongoDB cũng có thể xử lý khối lượng lớn và có thể mở rộng theo chiều dọc hoặc ngang để đáp ứng các tải dữ liệu lớn.

MongoDB được xây dựng dành cho những người đang xây dựng các ứng dụng internet và doanh nghiệp cần phát triển nhanh chóng và mở rộng thanh lịch. 

## Khi nào nên sử dụng MongoDB?

MongoDB là cơ sở dữ liệu đa mục đích được sử dụng theo nhiều cách khác nhau để hỗ trợ các ứng dụng trong nhiều ngành công nghiệp khác nhau. MongoDB đã tìm thấy sự ứng dụng trong nhiều doanh nghiệp và chức năng khác nhau bởi vì nó giải quyết các vấn đề lâu dài trong quản lý dữ liệu và phát triển phần mềm. 

Các trường hợp sử dụng điển hình cho MongoDB bao gồm:

- Tích hợp lượng lớn dữ liệu đa dạng: Mô hình document linh hoạt giúp tích hợp dữ liệu từ nhiều nguồn.

- Mô tả các cấu trúc dữ liệu phức tạp có khả năng phát triển: Cho phép nhúng tài liệu và dễ dàng thay đổi schema.

- Cung cấp dữ liệu trong các ứng dụng hiệu suất cao: Khả năng mở rộng tốt để xử lý khối lượng lớn.

- Hỗ trợ các ứng dụng lai và đa đám mây: Có thể triển khai On-premise hoặc trên đám mây. 

- Hỗ trợ phát triển linh hoạt và hợp tác: Cho phép các nhóm làm việc độc lập với dữ liệu.

## Các tính năng chính của MongoDB

### Mô hình tài liệu

- Dữ liệu được lưu dưới dạng các tài liệu JSON, cho phép schema linh hoạt và phát triển nhanh chóng.
- Tài liệu được nhóm thành các collections. Mỗi collection có thể có các trường khác nhau giữa các document.
- Dữ liệu được lưu ở định dạng BSON cho hiệu suất cao hơn so với JSON.

### Phân mảnh (Sharding)

- Phân mảnh cho phép phân chia dữ liệu ra nhiều node, tăng khả năng mở rộng theo chiều ngang.
- Các truy vấn được định tuyến tới đúng shard dựa trên shard key.
- Phân mảnh cải thiện khả năng cân bằng tải.

### Sao chép (Replication)

- Sao chép dữ liệu giữa các node để dự phòng và tăng tính sẵn sàng.
- Có thể cân bằng tải đọc giữa các node phụ trong replica set.

### Xác thực (Authentication)

- Xác thực người dùng quan trọng để đảm bảo an toàn.
- MongoDB hỗ trợ nhiều cơ chế xác thực, phổ biến nhất là SCRAM.

### Kích hoạt cơ sở dữ liệu (Database Triggers)

- Cho phép thực thi mã khi có sự kiện xảy ra trong cơ sở dữ liệu.
- Có thể dùng để kiểm tra tính toàn vẹn dữ liệu, xử lý sự kiện phức tạp.

### Dữ liệu chuỗi thời gian (Time Series Data)

- MongoDB hỗ trợ tốt việc lưu trữ và xử lý dữ liệu chuỗi thời gian như dữ liệu cảm biến.
- Có các tùy chọn để kiểm soát hiệu quả lưu trữ dữ liệu chuỗi thời gian.

### Truy vấn tình huống (Ad-hoc Queries)

- MongoDB hỗ trợ tốt các truy vấn tình huống, truy vấn động.
- Sử dụng các chỉ mục, tài liệu BSON và ngôn ngữ truy vấn MQL.
- Hỗ trợ tổng hợp dữ liệu với Aggregation Framework.

### Chỉ mục (Indexing)

- Chỉ mục cải thiện đáng kể hiệu suất truy vấn. 
- MongoDB hỗ trợ nhiều loại chỉ mục và có thể tự động tạo chỉ mục dựa trên mẫu truy vấn.

### Cân bằng tải (Load Balancing)

- MongoDB tự động cân bằng tải giữa các node thông qua các tính năng phân mảnh và nhân bản.
- Đảm bảo truy cập nhất quán đến dữ liệu ở quy mô lớn.

## Kết luận

- MongoDB là nền tảng cơ sở dữ liệu mã nguồn mở linh hoạt, phù hợp cho nhiều ứng dụng doanh nghiệp.
- Cung cấp nhiều tính năng hữu ích cho việc phát triển và vận hành ứng dụng.  
- MongoDB Atlas cung cấp dịch vụ MongoDB được quản lý đầy đủ trên đám mây.
