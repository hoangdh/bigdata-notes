MongoDB là một nền tảng cơ sở dữ liệu tài liệu NoSQL có khả năng mở rộng và linh hoạt, được thiết kế để vượt qua phương pháp cơ sở dữ liệu quan hệ và các hạn chế của các giải pháp NoSQL khác. MongoDB nổi tiếng với khả năng mở rộng theo chiều ngang và cân bằng tải, mang lại cho các nhà phát triển ứng dụng một mức độ linh hoạt và khả năng mở rộng chưa từng có.

MongoDB Atlas là dịch vụ cơ sở dữ liệu đám mây toàn cầu hàng đầu cho các ứng dụng hiện đại. Sử dụng Atlas, nhà phát triển có thể triển khai cơ sở dữ liệu đám mây được quản lý đầy đủ trên AWS, Azure và Google Cloud. Tiêu chuẩn bảo mật và quyền riêng tư dữ liệu hàng đầu đồng nghĩa với việc nhà phát triển có thể yên tâm biết rằng họ có ngay lập tức quyền truy cập vào tính sẵn có, khả năng mở rộng và tuân thủ cần thiết cho phát triển ứng dụng cấp doanh nghiệp.

Nền tảng cơ sở dữ liệu MongoDB đã được tải xuống hơn 200 triệu lần với hơn 1,8 triệu đăng ký tại MongoDB University. Có các trình điều khiển cho hơn 10 ngôn ngữ, với hàng chục ngôn ngữ khác được thêm bởi cộng đồng. Quan trọng nhất, MongoDB hoàn toàn miễn phí để sử dụng.

MongoDB cung cấp cho nhà phát triển một số tính năng hữu ích ngay từ đầu, cho dù bạn cần chạy riêng tư trên site hoặc trong đám mây công cộng. Hãy cùng xem xét một số tính năng kỹ thuật quan trọng nhất của MongoDB.

### Mô hình tài liệu

MongoDB được thiết kế với sự tiện lợi và linh hoạt của nhà phát triển là ưu tiên hàng đầu. Đó là một cơ sở dữ liệu hướng tài liệu, có nghĩa là dữ liệu được lưu trữ dưới dạng tài liệu và các tài liệu này được nhóm lại trong các bộ sưu tập. Mô hình tài liệu tự nhiên hơn đối với nhà phát triển vì tài liệu là tự chứa và có thể được xử lý như các đối tượng. Điều này có nghĩa là nhà phát triển có thể tập trung vào dữ liệu mà họ cần lưu trữ và xử lý, thay vì lo lắng về cách phân chia dữ liệu qua các bảng cứng khác nhau.

Các tài liệu được nhóm lại trong các bộ sưu tập. Tuy nhiên, các tài liệu trong một bộ sưu tập không nhất thiết phải có chính xác cùng một tập hợp các trường. Điều này được gọi là "schema linh hoạt." Sự linh hoạt này cho phép nhà phát triển phát triển nhanh chóng và di chuyển dữ liệu giữa các schema khác nhau mà không làm gián đoạn dịch vụ. Tuy nhiên, nếu bạn muốn khóa chặt schema của mình tại một điểm cụ thể, bạn có thể làm điều này bằng cách áp dụng quy tắc xác nhận vào các bộ sưu tập của bạn.

Các tài liệu trong MongoDB được lưu trữ trong định dạng BSON, một định dạng JSON được mã hóa nhị phân. Điều này có nghĩa là dữ liệu được lưu trữ dưới dạng nhị phân, nhanh hơn nhiều so với JSON. Điều này cũng cho phép lưu trữ dữ liệu nhị phân, hữu ích cho việc lưu trữ hình ảnh, video và dữ liệu nhị phân khác. Mặc dù BSON là một định dạng được mã hóa nhị phân, nhưng việc làm việc với nó sử dụng trình điều khiển MongoDB cho ngôn ngữ lập trình của bạn vẫn rất dễ dàng.

Để tìm hiểu thêm về mô hình tài liệu, hãy kiểm tra bài viết về Cơ Sở Dữ Liệu Tài Liệu.

### Sharding

Sharding là quá trình chia các bộ dữ liệu lớn thành nhiều thể hiện phân tán, hay "shard." Khi áp dụng cho các bộ dữ liệu đặc biệt lớn, sharding giúp cơ sở dữ liệu phân phối và thực hiện các truy vấn mà nếu không có sharding, đó sẽ là những truy vấn khó khăn và cồng kềnh. Mà không có sharding, việc mở rộng một ứng dụng web đang phát triển với hàng triệu người dùng hàng ngày trở nên gần như không thể.

Trong MongoDB, sharding cho phép có khả năng mở rộng theo chiều ngang nhiều hơn. Mở rộng theo chiều ngang có nghĩa là mỗi shard trong mọi cụm chứa một phần của bộ dữ liệu cụ thể, hoạt động cơ bản như một cơ sở dữ liệu riêng biệt. Kết hợp dữ liệu từ các shard phân tán tạo thành một cơ sở dữ liệu toàn diện, phù hợp hơn để xử lý nhu cầu của một ứng dụng phổ biến và đang phát triển mà không gây ra thời gian chết.

Tất cả các hoạt động của khách hàng trong môi trường sharding được xử lý thông qua một quy trình nhẹ được gọi là mongos. Mongos có thể chỉ đạo các truy vấn đến shard chính xác dựa trên khóa shard. Việc sharding đúng cũng đóng góp đáng kể vào cân bằng tải tốt hơn.

Hãy kiểm tra bài viết riêng về Chia Dữ Liệu Cơ Sở Dữ Liệu để tìm hiểu thêm về các kiến trúc sharding khác nhau và các vấn đề mà chúng giải quyết.

### Đồng Bộ Hóa (Replication)

Khi dữ liệu của bạn chỉ tồn tại trên một máy chủ duy nhất, nó nằm trong tình trạng tiềm ẩn nhiều điểm tự nhiên thất bại, như sự cố máy chủ, gián đoạn dịch vụ, hoặc thậm chí là sự cố phần cứng. Bất kỳ sự kiện nào trong số này đều có thể làm cho việc truy cập dữ liệu của bạn gần như không thể.

Đồng bộ hóa cho phép bạn né tránh những yếu điểm này bằng cách triển khai nhiều máy chủ cho khôi phục sau thảm họa và sao lưu. Mở rộng theo chiều ngang qua nhiều máy chủ tăng đáng kể sẵn có dữ liệu, độ tin cậy và khả năng chống lỗi. Làm thế nào sẽ phân phối tải đọc đến các thành viên phụ của bộ nhân tạo với việc sử dụng ưu tiên đọc.

Trong MongoDB, bộ nhân tạo được sử dụng cho mục đích này. Một máy chủ chính hoặc nút chấp nhận tất cả các hoạt động ghi và áp dụng những hoạt động tương tự trên các máy chủ phụ, đồng bộ hóa dữ liệu. Nếu máy chủ chính bao giờ gặp phải một sự cố quan trọng, bất kỳ máy chủ phụ nào cũng có thể được bầu làm nút chính mới. Và nếu nút chính trước đây trở lại trực tuyến, nó sẽ làm như một máy chủ phụ cho nút chính mới.

MongoDB Atlas, nền tảng DBaaS (Cơ sở dữ liệu như Dịch vụ) của MongoDB, có ít nhất là bộ nhân tạo có ba thành viên. Chúng có thể lan rộng qua nhiều khu vực hoặc thậm chí là nhiều nhà cung cấp đám mây mà bạn chọn.

Hãy kiểm tra bài viết về Đồng Bộ Hóa để tìm hiểu thêm về cách đồng bộ hóa hoạt động trong MongoDB.

### Xác thực (Authentication)

Xác thực là một tính năng bảo mật quan trọng trong MongoDB. Xác thực đảm bảo rằng chỉ những người dùng được ủy quyền mới có thể truy cập cơ sở dữ liệu. Mà không có xác thực, bất kỳ ai cũng có thể truy cập dữ liệu của bạn.

MongoDB cung cấp một số cơ chế xác thực để người dùng có thể truy cập cơ sở dữ liệu. Phổ biến nhất là Cơ chế Xác thực Phản Hồi Thách thức với Muối (SCRAM), đây là cơ chế mặc định. Khi sử dụng SCRAM, người dùng cần cung cấp một cơ sở dữ liệu xác thực, tên người dùng và mật khẩu.

Để tìm hiểu thêm về SCRAM và các cơ chế xác thực khác có sẵn, hãy kiểm tra bài viết về Xác Thực MongoDB.

### Kích hoạt Cơ sở Dữ liệu (Database Triggers)

Các kích hoạt cơ sở dữ liệu trong MongoDB Atlas là một tính năng mạnh mẽ cho phép bạn thực thi mã khi các sự kiện cụ thể xảy ra trong cơ sở dữ liệu của bạn. Ví dụ, bạn có thể sử dụng kích hoạt để thực thi một đoạn mã khi một tài liệu được chèn, cập nhật hoặc xóa. Kích hoạt cũng có thể được lên lịch để thực thi vào những thời điểm cụ thể.

MongoDB Atlas cho phép bạn tạo và quản lý các kích hoạt một cách đơn giản, trực quan. Bạn có thể kiểm soát các kích hoạt của mình thông qua giao diện người dùng Atlas.

Kích hoạt cơ sở dữ liệu là một cách tuyệt vời để thực hiện kiểm toán, đảm bảo tính nhất quán và tính toàn vẹn dữ liệu, cũng như để thực hiện xử lý sự kiện phức tạp. Hãy kiểm tra bài viết chuyên sâu về Kích Hoạt Cơ Sở Dữ Liệu để tìm hiểu thêm về các loại kích hoạt khác nhau và cách sử dụng chúng.

### Dữ liệu chuỗi thời gian

Dữ liệu chuỗi thời gian thường được tạo ra bởi một thiết bị, chẳng hạn như một cảm biến, ghi lại dữ liệu theo thời gian. Dữ liệu được lưu trữ trong một bộ sưu tập các tài liệu, mỗi tài liệu chứa một dấu thời gian và một giá trị. MongoDB cung cấp nhiều tính năng để giúp bạn quản lý dữ liệu chuỗi thời gian.

Các bộ sưu tập chuỗi thời gian nội địa trong MongoDB được thiết kế để tiết kiệm lưu trữ và hoạt động hiệu quả với các chuỗi đo lường. Bạn có nhiều tham số để kiểm soát việc lưu trữ dữ liệu chuỗi thời gian, bao gồm độ tỉ mỉ (khoảng thời gian giữa các đo lường) và ngưỡng hết hạn của dữ liệu cũ.

Để tìm hiểu thêm về các bộ sưu tập chuỗi thời gian nội địa và các tính năng MongoDB khác giúp làm việc với dữ liệu chuỗi thời gian dễ dàng hơn, hãy kiểm tra bài viết về Dữ liệu Chuỗi Thời Gian.

### Truy vấn Ngẫu Nhiên (Ad-Hoc Queries)

Khi thiết kế schema của một cơ sở dữ liệu, không thể biết trước tất cả các truy vấn mà người dùng cuối sẽ thực hiện. Một truy vấn ngẫu nhiên là một lệnh có hiệu lực ngắn hạn mà giá trị của nó phụ thuộc vào một biến. Mỗi khi một truy vấn ngẫu nhiên được thực hiện, kết quả có thể khác nhau, tùy thuộc vào các biến cụ thể.

Tối ưu hóa cách xử lý các truy vấn ngẫu nhiên có thể tạo ra sự khác biệt đáng kể khi quy mô lớn, khi cần xem xét hàng nghìn đến hàng triệu biến. Điều này làm cho MongoDB, một cơ sở dữ liệu hướng văn bản với schema linh hoạt, trở nên xuất sắc như một nền tảng cơ sở dữ liệu đám mây lựa chọn cho các ứng dụng doanh nghiệp yêu cầu phân tích thời gian thực. Với hỗ trợ truy vấn ngẫu nhiên cho phép nhà phát triển cập nhật các truy vấn ngẫu nhiên trong thời gian thực, cải thiện hiệu suất có thể là quyết định thay đổi trò chơi.

MongoDB hỗ trợ truy vấn trường, truy vấn địa lý và tìm kiếm biểu thức chính quy. Các truy vấn có thể trả về các trường cụ thể và cũng có thể tính đến các hàm do người dùng định nghĩa. Điều này được thực hiện với các chỉ mục MongoDB, tài liệu BSON và Ngôn ngữ Truy vấn MongoDB (MQL). MongoDB cũng hỗ trợ tổng hợp thông qua Framework Tổng hợp.

Để tìm hiểu thêm về các tính năng phân tích của MongoDB, hãy kiểm tra bài viết riêng về Phân Tích Thời Gian Thực.

### Chỉ Mục (Indexing)

Theo kinh nghiệm của chúng tôi, vấn đề quan trọng nhất mà nhiều đội hỗ trợ kỹ thuật thường bỏ qua khi làm việc với người dùng của họ là chỉ mục. Khi thực hiện đúng, chỉ mục được thiết kế để cải thiện tốc độ và hiệu suất tìm kiếm. Việc không định nghĩa đúng các chỉ mục thích hợp có thể và thường dẫn đến nhiều vấn đề về khả năng truy cập, chẳng hạn như vấn đề với việc thực thi truy vấn và cân bằng tải.

Mà không có các chỉ mục phù hợp, một cơ sở dữ liệu buộc phải quét tài liệu một cách tuần tự để xác định những tài liệu phù hợp với câu truy vấn. Nhưng nếu mỗi câu truy vấn đều có một chỉ mục phù hợp, các yêu cầu của người dùng có thể được thực hiện một cách tối ưu bởi máy chủ. MongoDB cung cấp một loạt rộng các chỉ mục và tính năng với các thứ tự sắp xếp theo ngôn ngữ hỗ trợ các mô hình truy cập phức tạp đến các bộ dữ liệu.

Đáng chú ý, chỉ mục MongoDB có thể được tạo ra theo yêu cầu để đáp ứng các mô hình truy vấn và yêu cầu ứng dụng thay đổi thời gian thực. Chúng cũng có thể được khai báo trên bất kỳ trường nào trong bất kỳ tài liệu nào, bao gồm cả những trường nằm trong các mảng lồng.

Hãy kiểm tra bài viết Performance Best Practices: Indexing để tìm hiểu thêm về các loại chỉ mục khác nhau và cách sử dụng chúng.

### Cân Bằng Tải (Load Balancing)

Cuối cùng, cân bằng tải tối ưu vẫn là một trong những mục tiêu quan trọng của quản lý cơ sở dữ liệu quy mô lớn cho các ứng dụng doanh nghiệp đang phát triển. Việc phân phối đều hàng triệu yêu cầu từ người dùng đến hàng trăm hoặc nghìn máy chủ có thể dẫn đến sự khác biệt đáng kể (và rất được đánh giá cao) về hiệu suất.

May mắn thay, thông qua các tính năng mở rộng theo chiều ngang như sao chép và sharding, MongoDB hỗ trợ cân bằng tải quy mô lớn. Nền tảng có thể xử lý đồng thời nhiều yêu cầu đọc và ghi cho cùng một dữ liệu với kiểm soát đồng thời và giao thức khóa hàng đầu để đảm bảo tính nhất quán của dữ liệu. Không cần thêm một trình cân bằng tải ngoại vi - MongoDB đảm bảo mỗi người dùng có một cái nhìn nhất quán và trải nghiệm chất lượng với dữ liệu mà họ cần truy cập.

Nếu bạn muốn tìm hiểu cách cân bằng tải hoạt động trong một cụm sharded, hãy kiểm tra Trang Cân Bằng Tải Cụm Sharded trong Tài Liệu MongoDB.

### Tổng Kết

MongoDB là một nền tảng cơ sở dữ liệu linh hoạt, hướng tài liệu được thiết kế để trở thành cơ sở dữ liệu đám mây lựa chọn cho các ứng dụng doanh nghiệp. MongoDB cung cấp nhiều tính năng làm cho nó trở thành lựa chọn tuyệt vời cho nhiều loại ứng dụng.

MongoDB Atlas là nền tảng DBaaS (Cơ sở dữ liệu như Dịch vụ) của MongoDB, cung cấp một cụm MongoDB được quản lý hoàn toàn với một MongoDB instance riêng cho mỗi người dùng. Atlas có tất cả các tính năng của một instance MongoDB Enterprise, cộng thêm khả năng mở rộng theo chiều ngang và chiều dọc chỉ bằng một cú nhấp chuột. Bạn có thể bắt đầu với MongoDB Atlas ngay hôm nay bằng cách tạo một cụm miễn phí.
