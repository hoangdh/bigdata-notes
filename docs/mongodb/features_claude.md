 Đây là bản dịch bài viết sang tiếng Việt ở định dạng Markdown:

# Các tính năng của MongoDB

**MongoDB** là một nền tảng cơ sở dữ liệu NoSQL đáp ứng tốt việc mở rộng, linh hoạt, được thiết kế để vượt qua giới hạn của cách tiếp cận cơ sở dữ liệu quan hệ và các giải pháp NoSQL khác. MongoDB nổi tiếng về khả năng mở rộng theo chiều ngang và cân bằng tải, mang đến cho các nhà phát triển ứng dụng mức độ linh hoạt và khả năng mở rộng chưa từng có.

**MongoDB Atlas** là dịch vụ cơ sở dữ liệu đám mây hàng đầu thế giới cho các ứng dụng hiện đại. Với Atlas, các nhà phát triển có thể triển khai các cơ sở dữ liệu đám mây được quản lý hoàn toàn trên AWS, Azure và Google Cloud. Các tiêu chuẩn hàng đầu về bảo mật và quyền riêng tư dữ liệu có nghĩa là các nhà phát triển có thể yên tâm biết rằng họ có quyền truy cập ngay lập tức vào khả năng sẵn sàng, khả năng mở rộng và tuân thủ mà họ cần cho việc phát triển ứng dụng doanh nghiệp.

Nền tảng cơ sở dữ liệu MongoDB đã được tải xuống hơn 200 triệu lần với hơn 1,8 triệu đăng ký của trường Đại học MongoDB. Có các driver cho 10+ ngôn ngữ, với hàng chục được thêm vào bởi cộng đồng. Tốt nhất là MongoDB hoàn toàn miễn phí để sử dụng.

MongoDB cung cấp cho các nhà phát triển một số khả năng hữu ích ngay lập tức, cho dù bạn cần chạy riêng trên trang web hay trên đám mây công cộng.

Hãy cùng xem một số tính năng kỹ thuật quan trọng nhất của MongoDB.

## Mô hình tài liệu
MongoDB được thiết kế với mục tiêu năng suất và tính linh hoạt của nhà phát triển. Đây là cơ sở dữ liệu hướng tài liệu, có nghĩa là dữ liệu được lưu trữ dưới dạng các tài liệu, và các tài liệu được nhóm lại thành các bộ sưu tập. Mô hình tài liệu rất tự nhiên để các nhà phát triển làm việc vì các tài liệu là độc lập và có thể được xử lý như các đối tượng. Điều này có nghĩa là các nhà phát triển có thể tập trung vào dữ liệu họ cần lưu trữ và xử lý, thay vì lo lắng về cách phân chia dữ liệu trên các bảng cứng nhắc khác nhau.  

Các tài liệu được nhóm lại với nhau trong các bộ sưu tập. Tuy nhiên, các tài liệu trong cùng một bộ sưu tập không nhất thiết phải có chính xác cùng một tập hợp các trường. Đây là những gì chúng tôi gọi là "schema linh hoạt". Sự linh hoạt này cho phép các nhà phát triển lặp lại nhanh hơn và di chuyển dữ liệu giữa các lược đồ khác nhau mà không có thời gian chết. 

Các tài liệu trong MongoDB được lưu trữ ở định dạng BSON, đó là một định dạng JSON được mã hóa nhị phân. Điều này có nghĩa là dữ liệu được lưu trữ ở định dạng nhị phân, nhanh hơn nhiều so với JSON. Để tìm hiểu nhiều hơn về mô hình tài liệu, hãy xem bài viết về cơ sở dữ liệu tài liệu.

## Phân mảnh (Sharding)
Phân mảnh là quá trình chia nhỏ và phân phối dữ liệu lên nhiều máy chủ. Khi áp dụng cho các dữ liệu lớn, phân mảnh sẽ giúp cơ sở dữ liệu phân phối và xử lý các truy vấn tốt hơn. 

Phân mảnh trong MongoDB cho phép mở rộng theo chiều ngang lớn. Mỗi mảnh đóng vai trò là cơ sở dữ liệu riêng biệt. Kết hợp dữ liệu giữa các mảnh tạo thành một cơ sở dữ liệu toàn diện, xử lý tốt hơn nhu cầu của ứng dụng đang phát triển mà không gây gián đoạn.  

Hãy xem bài viết về Phân mảnh Cơ sở dữ liệu để biết thêm chi tiết.

## Nhân bản (Replication)
Nhân bản cho phép triển khai nhiều máy chủ để sao lưu và khôi phục khi xảy ra sự cố. Nhân rộng trên nhiều máy chủ sẽ tăng khả năng sẵn sàng, độ tin cậy và khả năng chịu lỗi của dữ liệu.  

Trong MongoDB, các bộ nhân bản được sử dụng cho mục đích này. Một máy chủ chính chấp nhận các hoạt động ghi và áp dụng trên các máy chủ phụ, sao chép dữ liệu. Nếu máy chủ chính gặp sự cố, các máy chủ phụ có thể được bầu làm máy chủ chính mới.  

Hãy xem bài viết về Nhân bản để biết thêm chi tiết về cách nhân bản hoạt động trong MongoDB.

## Xác thực (Authentication)  
Xác thực là tính năng bảo mật quan trọng trong MongoDB. Xác thực đảm bảo chỉ người dùng được ủy quyền mới có thể truy cập cơ sở dữ liệu.  

MongoDB hỗ trợ một số cơ chế xác thực cho người dùng truy cập cơ sở dữ liệu. Phổ biến nhất là SCRAM - yêu cầu người dùng cung cấp database xác thực, tên người dùng và mật khẩu.

## Trigger dữ liệu
Trigger cho phép thực thi mã khi có sự kiện xảy ra trên cơ sở dữ liệu như chèn, cập nhật, xóa tài liệu.  

MongoDB Atlas cho phép tạo và quản lý trigger một cách đơn giản, trực quan. Trigger rất hữu ích để kiểm tra tính nhất quán dữ liệu và xử lý sự kiện phức tạp.

## Dữ liệu chuỗi thời gian (Time Series)
Dữ liệu chuỗi thời gian thường được tạo ra bởi các cảm biến, lưu trữ dưới dạng tài liệu với timestamp và giá trị.  

Các collection dữ liệu chuỗi thời gian trong MongoDB được tối ưu hóa để lưu trữ hiệu quả và thực thi tốt trên các chuỗi giá trị đo đạc. Bạn có thể kiểm soát các thông số như khoảng thời gian giữa các giá trị, ngưỡng hết hạn của dữ liệu cũ.

## Truy vấn tình huống (Ad-hoc Queries) 
Các truy vấn tình huống ngắn hạn có kết quả phụ thuộc vào biến. Tối ưu hóa cách xử lý truy vấn tình huống có thể làm thay đổi đáng kể hiệu năng khi có hàng triệu biến cần xét đến.  

MongoDB hỗ trợ linh hoạt các truy vấn phức tạp với hiệu năng cao. MongoDB mang lại sự lựa chọn tuyệt vời cho các ứng dụng doanh nghiệp đòi hỏi phân tích thời gian thực.

## Chỉ mục (Indexing)
Chỉ mục giúp tăng tốc độ tìm kiếm và hiệu năng của cơ sở dữ liệu. Việc không xác định chỉ mục phù hợp có thể dẫn đến nhiều vấn đề về hiệu năng và khả năng truy cập.  

MongoDB cho phép tạo chỉ mục trên bất kỳ trường nào trong tài liệu để tối ưu hóa hiệu năng. Chỉ mục có thể được tạo động để đáp ứng theo thời gian thực.

## Cân bằng tải (Load Balancing)  
Cân bằng tải là thánh chỉ của việc quản lý cơ sở dữ liệu ở quy mô lớn. MongoDB hỗ trợ cân bằng tải ở quy mô lớn thông qua các tính năng scale theo chiều ngang như nhân bản và sharding.  

Nền tảng có thể xử lý nhiều yêu cầu đọc/ghi đồng thời cho cùng dữ liệu với giao thức kiểm soát đồng thời và khóa đảm bảo tính nhất quán dữ liệu. Mỗi người dùng đều có trải nghiệm nhất quán với dữ liệu.

# Kết luận

MongoDB là nền tảng cơ sở dữ liệu hướng tài liệu linh hoạt, được thiết kế để trở thành lựa chọn cơ sở dữ liệu đám mây cho các ứng dụng doanh nghiệp. MongoDB cung cấp nhiều tính năng giúp nó trở thành sự lựa chọn tuyệt vời cho đa dạng các ứng dụng. 

MongoDB Atlas là nền tảng DBaaS của MongoDB, cung cấp cụm MongoDB được quản lý hoàn toàn với tài nguyên riêng biệt cho mỗi người dùng. Atlas có tất cả các tính năng của MongoDB Enterprise, cộng thêm khả năng mở rộng theo chiều ngang & dọc chỉ với một cú nhấp chuột. 

Bạn có thể bắt đầu với MongoDB Atlas ngay hôm nay bằng cách tạo một cụm miễn phí.
