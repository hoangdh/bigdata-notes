## MongoDB: Cơ sở dữ liệu linh hoạt, có thể mở rộng

MongoDB là một nền tảng cơ sở dữ liệu NoSQL hướng tài liệu, có thể mở rộng, được thiết kế để vượt qua phương pháp của cơ sở dữ liệu quan hệ và những hạn chế của các giải pháp NoSQL khác. MongoDB nổi tiếng với khả năng mở rộng theo chiều ngang và cân bằng tải, mang đến cho các nhà phát triển ứng dụng mức độ linh hoạt và khả năng mở rộng chưa từng có.

MongoDB Atlas là dịch vụ cơ sở dữ liệu đám mây toàn cầu hàng đầu cho các ứng dụng hiện đại. Sử dụng Atlas, các nhà phát triển có thể triển khai các cơ sở dữ liệu đám mây được quản lý hoàn toàn trên AWS, Azure và Google Cloud. Các phương pháp thực hành tiêu chuẩn về bảo mật và quyền riêng tư dữ liệu tốt nhất có nghĩa là các nhà phát triển có thể yên tâm khi biết rằng họ có quyền truy cập tức thì vào tính khả dụng, khả năng mở rộng và tuân thủ mà họ yêu cầu để phát triển ứng dụng cấp doanh nghiệp.

Nền tảng cơ sở dữ liệu MongoDB đã được tải xuống hơn 200 triệu lần với hơn 1,8 triệu lượt đăng ký Đại học MongoDB. Có các trình điều khiển cho hơn 10 ngôn ngữ, với hàng chục ngôn ngữ khác được cộng đồng bổ sung. Điều tuyệt vời nhất là MongoDB hoàn toàn miễn phí sử dụng.

MongoDB cung cấp cho các nhà phát triển một số tính năng hữu ích ngay lập tức, cho dù bạn cần chạy riêng trên trang web hay trong đám mây công cộng.

Hãy cùng xem một số tính năng kỹ thuật thiết yếu nhất của MongoDB.

## Mô hình tài liệu

MongoDB được thiết kế hướng đến năng suất và sự linh hoạt của nhà phát triển. Đây là một cơ sở dữ liệu hướng tài liệu, có nghĩa là dữ liệu được lưu trữ dưới dạng tài liệu và tài liệu được nhóm trong các bộ sưu tập. Mô hình tài liệu tự nhiên hơn nhiều đối với nhà phát triển vì tài liệu là độc lập và có thể được coi như các đối tượng. Điều này có nghĩa là nhà phát triển có thể tập trung vào dữ liệu họ cần lưu trữ và xử lý, thay vì lo lắng về cách chia dữ liệu trên các bảng cứng nhắc khác nhau.

Các tài liệu được nhóm lại với nhau trong các bộ sưu tập. Tuy nhiên, các tài liệu trong một bộ sưu tập không nhất thiết phải có cùng một tập hợp các trường. Đây là cái mà chúng ta gọi là "schema linh hoạt". Tính linh hoạt này cho phép các nhà phát triển lặp lại nhanh hơn và di chuyển dữ liệu giữa các schema khác nhau mà không có thời gian ngừng hoạt động. Tuy nhiên, nếu bạn muốn khóa schema tại một điểm nhất định, bạn có thể thực hiện bằng cách áp dụng các quy tắc xác thực cho các bộ sưu tập của mình.

Tài liệu trong MongoDB được lưu trữ theo định dạng BSON, đây là định dạng JSON được mã hóa nhị phân. Điều này có nghĩa là dữ liệu được lưu trữ ở dạng nhị phân, nhanh hơn nhiều so với JSON. Điều này cũng cho phép lưu trữ dữ liệu nhị phân, hữu ích cho việc lưu trữ hình ảnh, video và các dữ liệu nhị phân khác. Mặc dù BSON là một định dạng được mã hóa nhị phân, nhưng rất dễ dàng để làm việc với nó bằng trình điều khiển MongoDB cho ngôn ngữ lập trình của bạn.

Để tìm hiểu thêm về mô hình tài liệu, hãy xem bài viết Cơ sở dữ liệu tài liệu.

## Phân mảnh

Phân mảnh là quá trình chia nhỏ các tập dữ liệu lớn thành nhiều phiên bản phân tán, hoặc "shard". Khi áp dụng cho các tập dữ liệu đặc biệt lớn, phân mảnh giúp cơ sở dữ liệu phân phối và thực thi tốt hơn những truy vấn có thể gây ra vấn đề và cồng kềnh. Nếu không có phân mảnh, việc mở rộng ứng dụng web đang phát triển với hàng triệu người dùng hàng ngày gần như là bất khả thi.

Phân mảnh trong MongoDB cho phép khả năng mở rộng ngang lớn hơn nhiều. Mở rộng ngang có nghĩa là mỗi shard trong mọi cụm chứa một phần của tập dữ liệu đang được đề cập, về cơ bản hoạt động như một cơ sở dữ liệu riêng biệt. Việc kết hợp dữ liệu của các shard phân tán tạo thành một cơ sở dữ liệu duy nhất, toàn diện, phù hợp hơn nhiều để xử lý các nhu cầu của một ứng dụng phổ biến, đang phát triển mà không có thời gian ngừng hoạt động.

Tất cả các hoạt động của khách hàng trong môi trường phân mảnh đều được xử lý thông qua một quy trình nhẹ được gọi là mongos. Mongos có thể hướng các truy vấn đến shard chính xác dựa trên khóa shard. Phân mảnh hợp lý cũng góp phần đáng kể vào việc cân bằng tải tốt hơn.

Hãy xem bài viết Phân mảnh cơ sở dữ liệu chuyên dụng để tìm hiểu thêm về các kiến trúc phân mảnh khác nhau và những vấn đề mà chúng giải quyết.

## Sao chép

Khi dữ liệu của bạn chỉ nằm trên một máy chủ duy nhất, nó sẽ gặp phải nhiều điểm lỗi tiềm ẩn, chẳng hạn như sự cố máy chủ, gián đoạn dịch vụ hoặc thậm chí lỗi phần cứng. Bất kỳ sự kiện nào trong số này đều có thể khiến việc truy cập dữ liệu của bạn gần như không thể.

Sao chép cho phép bạn tránh những lỗ hổng này bằng cách triển khai nhiều máy chủ để phục hồi sau thảm họa và sao lưu. Mở rộng theo chiều ngang trên nhiều máy chủ làm tăng đáng kể tính sẵn sàng của dữ liệu, độ tin cậy và khả năng chịu lỗi. Về mặt tiềm năng, sao chép có thể giúp phân tán tải đọc sang các thành viên thứ cấp của tập bản sao bằng cách sử dụng tùy chọn đọc.

Trong MongoDB, các tập bản sao được sử dụng cho mục đích này. Một máy chủ hoặc nút chính chấp nhận tất cả các hoạt động ghi và áp dụng các hoạt động tương tự trên các máy chủ thứ cấp, sao chép dữ liệu. Nếu máy chủ chính gặp phải sự cố nghiêm trọng, bất kỳ máy chủ thứ cấp nào cũng có thể được chọn làm nút chính mới. Và nếu nút chính trước đó trở lại trực tuyến, nó sẽ hoạt động như một máy chủ thứ cấp cho nút chính mới.

MongoDB Atlas, nền tảng DBaaS (Database-as-a-Service) của MongoDB, có tối thiểu ba tập bản sao thành viên. Chúng có thể trải dài trên nhiều vùng hoặc thậm chí nhiều nhà cung cấp đám mây theo lựa chọn của bạn.

Kiểm tra bài viết Sao chép để tìm hiểu thêm về cách sao chép hoạt động trong MongoDB.

## Xác thực

Xác thực là một tính năng bảo mật quan trọng trong MongoDB. Xác thực đảm bảo rằng chỉ những người dùng được ủy quyền mới có thể truy cập cơ sở dữ liệu. Nếu không có xác thực, bất kỳ ai cũng có thể truy cập dữ liệu của bạn.

MongoDB cung cấp một số cơ chế xác thực cho phép người dùng truy cập cơ sở dữ liệu. Phổ biến nhất là Cơ chế xác thực phản hồi thử thách được thêm muối (SCRAM), đây là mặc định. Khi được sử dụng, SCRAM yêu cầu người dùng cung cấp cơ sở dữ liệu xác thực, tên người dùng và mật khẩu.

Để tìm hiểu thêm về SCRAM và các cơ chế xác thực khác có sẵn, hãy xem bài viết Xác thực MongoDB.

## Triggers cơ sở dữ liệu

Triggers cơ sở dữ liệu trong MongoDB Atlas là một tính năng mạnh mẽ cho phép bạn thực thi code khi một số sự kiện nhất định xảy ra trong cơ sở dữ liệu của bạn. Ví dụ, bạn có thể sử dụng triggers để thực thi một script khi một tài liệu được chèn, cập nhật hoặc xóa. Triggers cũng có thể được đặt lịch thực thi vào các thời điểm cụ thể.

MongoDB Atlas cho phép bạn tạo và quản lý triggers theo cách đơn giản và trực quan. Bạn có thể kiểm soát triggers của mình thông qua giao diện người dùng (UI) của Atlas.

Triggers cơ sở dữ liệu là một cách tuyệt vời để thực hiện kiểm toán, đảm bảo tính nhất quán và toàn vẹn của dữ liệu, cũng như để thực hiện xử lý sự kiện phức tạp. Kiểm tra bài viết Triggers cơ sở dữ liệu chuyên dụng để tìm hiểu thêm về các loại triggers khác nhau và cách sử dụng chúng.

## Dữ liệu chuỗi thời gian

Dữ liệu chuỗi thời gian thường được tạo bởi một thiết bị, chẳng hạn như cảm biến, ghi lại dữ liệu theo thời gian. Dữ liệu được lưu trữ trong một tập hợp các tài liệu, mỗi tài liệu chứa một dấu thời gian và một giá trị. MongoDB cung cấp một số tính năng giúp bạn quản lý dữ liệu chuỗi thời gian.

Các bộ sưu tập chuỗi thời gian gốc trong MongoDB được thiết kế để tiết kiệm dung lượng lưu trữ và hoạt động tốt với các chuỗi phép đo. Bạn có một số tham số để kiểm soát việc lưu trữ dữ liệu chuỗi thời gian, bao gồm độ chi tiết (khoảng thời gian giữa các phép đo) và ngưỡng hết hạn của dữ liệu cũ.

Để tìm hiểu thêm về các bộ sưu tập chuỗi thời gian gốc và các tính năng khác của MongoDB giúp việc làm việc với dữ liệu chuỗi thời gian dễ dàng hơn, hãy xem bài viết Dữ liệu chuỗi thời gian.

## Truy vấn Ad-Hoc

Khi thiết kế schema của một cơ sở dữ liệu, không thể biết trước tất cả các truy vấn sẽ được thực hiện bởi người dùng cuối. Một truy vấn ad-hoc là một lệnh ngắn hạn có giá trị phụ thuộc vào một biến. Mỗi lần một truy vấn ad-hoc được thực thi, kết quả có thể khác nhau, tùy thuộc vào các biến đang được hỏi.

Việc tối ưu hóa cách xử lý các truy vấn ad-hoc có thể tạo ra sự khác biệt đáng kể về quy mô, khi hàng nghìn đến hàng triệu biến cần được xem xét. Đây là lý do tại sao MongoDB, một cơ sở dữ liệu có schema linh hoạt theo hướng tài liệu, nổi bật như nền tảng cơ sở dữ liệu đám mây được lựa chọn cho các ứng dụng doanh nghiệp yêu cầu phân tích thời gian thực. Với sự hỗ trợ cho truy vấn ad-hoc cho phép các nhà phát triển cập nhật truy vấn ad-hoc trong thời gian thực, việc cải thiện hiệu suất có thể thay đổi cuộc chơi.

MongoDB hỗ trợ truy vấn trường, truy vấn địa lý và tìm kiếm biểu thức chính quy. Các truy vấn có thể trả về các trường cụ thể và cũng tính đến các hàm do người dùng định nghĩa. Điều này có thể thực hiện được nhờ vào các index của MongoDB, tài liệu BSON và Ngôn ngữ truy vấn MongoDB (MQL). MongoDB cũng hỗ trợ tổng hợp thông qua Khung tổng hợp.

Để tìm hiểu thêm về các tính năng phân tích của MongoDB, hãy xem bài viết chuyên dụng Phân tích thời gian thực.

## Lập chỉ mục

Theo kinh nghiệm của chúng tôi, vấn đề số một mà nhiều nhóm hỗ trợ kỹ thuật không giải quyết được với người dùng của họ là lập chỉ mục. Khi thực hiện đúng, các chỉ mục nhằm mục đích cải thiện tốc độ và hiệu suất tìm kiếm. Việc không xác định đúng các chỉ mục thích hợp có thể và thường sẽ dẫn đến vô số vấn đề về khả năng truy cập, chẳng hạn như sự cố với việc thực thi truy vấn và cân bằng tải.

Nếu không có các chỉ mục thích hợp, cơ sở dữ liệu sẽ buộc phải quét các tài liệu từng cái một để xác định tài liệu nào khớp với câu lệnh truy vấn. Nhưng nếu tồn tại một chỉ mục thích hợp cho mỗi truy vấn, các yêu cầu của người dùng có thể được máy chủ thực thi một cách tối ưu. MongoDB cung cấp một loạt các chỉ mục và tính năng với thứ tự sắp xếp theo ngôn ngữ cụ thể hỗ trợ các mô hình truy cập phức tạp cho các tập dữ liệu.

Đáng chú ý, các chỉ mục MongoDB có thể được tạo theo yêu cầu để đáp ứng các mẫu truy vấn và yêu cầu ứng dụng thay đổi liên tục theo thời gian thực. Chúng cũng có thể được khai báo trên bất kỳ trường nào trong bất kỳ tài liệu nào của bạn, bao gồm cả những tài liệu được lồng trong các mảng.

Kiểm tra bài viết Các thực tiễn hiệu suất tốt nhất: Lập chỉ mục để tìm hiểu thêm về các loại chỉ mục khác nhau và cách sử dụng chúng.

## Cân bằng tải

Cuối cùng, việc cân bằng tải tối ưu vẫn là một trong những điều quan trọng nhất của việc quản lý cơ sở dữ liệu quy mô lớn cho các ứng dụng doanh nghiệp đang phát triển. Việc phân phối hợp lý hàng triệu yêu cầu của khách hàng đến hàng trăm hoặc hàng nghìn máy chủ có thể dẫn đến sự khác biệt đáng chú ý (và được đánh giá cao) về hiệu suất.

May mắn thay, thông qua các tính năng mở rộng ngang như sao chép và phân mảnh, MongoDB hỗ trợ cân bằng tải quy mô lớn. Nền tảng này có thể xử lý nhiều yêu cầu đọc và ghi đồng thời cho cùng dữ liệu với khả năng kiểm soát đồng thời tốt nhất và giao thức khóa đảm bảo tính nhất quán của dữ liệu. Không cần phải thêm một bộ cân bằng tải bên ngoài — MongoDB đảm bảo rằng mọi người dùng đều có chế độ xem và trải nghiệm chất lượng nhất quán với dữ liệu họ cần truy cập.

Nếu bạn tò mò về cách cân bằng tải hoạt động trong một cụm sharding, hãy xem trang Cân bằng cụm Sharded trong Tài liệu MongoDB.

## Tổng kết

MongoDB là một nền tảng cơ sở dữ liệu hướng tài liệu, linh hoạt được thiết kế để trở thành cơ sở dữ liệu đám mây được lựa chọn cho các ứng dụng doanh nghiệp. MongoDB cung cấp một số tính năng khiến nó trở thành lựa chọn tuyệt vời cho nhiều ứng dụng khác nhau.

MongoDB Atlas là nền tảng DBaaS (Database-as-a-Service) của MongoDB cung cấp cụm MongoDB được quản lý hoàn toàn với phiên bản MongoDB chuyên dụng cho mỗi người dùng. Atlas có tất cả các tính năng của một phiên bản MongoDB Enterprise, cộng với khả năng mở rộng theo chiều ngang và chiều dọc chỉ bằng một cú nhấp chuột. Bạn có thể bắt đầu sử dụng MongoDB Atlas ngay hôm nay bằng cách tạo một cụm miễn phí.
