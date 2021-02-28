## Giới thiệu về hbtop - Công cụ giám sát HBase tương đương top trong Unix

- `hbtop` là một công cụ giám sát HBase theo thời gian thực giống như câu lệnh `top` trong Unix. Nó có thể hiển thị các thông tin tóm lược như số Region/Namespace/Table/RegionServer. Với công cụ này, chúng ta có thể phân loại, lọc các số liệu mà ta mong muốn hiển thị. Ngoài ra, với tính năng Drill-down, ta có thể xem được Region nào đang 'nổi bật'.

- `htop` xuất hiện trên hbase 2.1.7, 2.2.2 và các bản trên 2.3.0. (Đã thực hiện trên Bản 2.2.6)

Để sử dụng `hbtop`, ta chạy lệnh sau:

```
hbase hbtop
```

Với trường hợp trên, câu lệnh sẽ sử dụng 2 giá trị `base.client.zookeeper.quorum` và `zookeeper.znode.parent` trong ` hbase-site.xml` để kết nối tới cụm HBase. Ngoài ra, ta có thể truyền các thông số đó vào thông qua câu lệnh:

```
hbase hbtop -Dhbase.client.zookeeper.quorum=<zookeeper quorum> -Dzookeeper.znode.parent=<znode parent>
```

Khi chạy câu lệnh, ta sẽ thấy mọi thư như hình:

![top_screen](https://hbase.apache.org/hbtop-images/top_screen.gif)

Trên đầu là một vài thông tin tóm tắt về các thông số như Phiên bản HBasee, Cluster ID, Số lượng RegionServer, số lượng Region, Tải trung bình của cụm và Tổng số Request/s. Bảng phía dưới thể hiện các thông tin về Region/Namespace/Table/Regionserver dựa trên một mode đã được chọn sẵn (Mặc định là Region). 3 giây là khoảng thời gian `hbtop` tự động làm mới các thông số mà nó thu thập.

Ta có thể cuộn lên/xuống/trái/phải để xem chi tiết các thông số.

Một vài đối số như sau:

- `-d`, `--delay` <arg>: Thời gian làm mới các thông số. Mặc định: 3 giây.
- `-h`, `--help`: Xem hướng dẫn sử dụng. Hoặc khi ở trong Tool; ta có thể bấm phím `h`.
- `-m`, `--mode`: Chọn chế độ xem theo Namespace (n), Table (t), Region (r), RegionServer (s). Mặc định: Region (r).

Chúng ta có thể thay đổi Mode khi đang xem bằng cách bấm `m`.

![changing_mode](https://hbase.apache.org/hbtop-images/changing_mode.gif)

Bấm `d` để thay đổi thời gian làm mới

![changing_refresh_delay](https://hbase.apache.org/hbtop-images/changing_refresh_delay.gif)

Nếu muốn thay đổi hiển thị các trường, ta bấm `f`. Thay đổi chúng bằng cách bấm `<Space>` hoặc phím `d`.

![changing_displayed_fields](https://hbase.apache.org/hbtop-images/changing_displayed_fields.gif)

Ta có thể thay đối sắp xếp bẳng cách chọn tới trường đó và bấm phím `s`.  Có thể thay đổi sự phân loại (tăng dần, giảm dần) bằng cách bấm phím `R`.

![changing_sort_field](https://hbase.apache.org/hbtop-images/changing_sort_field.gif)

Di chuyển các trường bằng cách:

![changing_order_of_fields](https://hbase.apache.org/hbtop-images/changing_order_of_fields.gif)

### Bộ lọc (Filters)

Chúng ta có thể lọc các thông số bằng cách bấm phím `o` hoặc `O` cho các trường hợp phân biệt chữ in.

![adding_filters](https://hbase.apache.org/hbtop-images/adding_filters.gif)

Cú pháp như sau:

```
<Field><Operator><Value>
```

Một vài ví dụ:

- `NAMESPACE==default`: Chỉ lấy thông tin của namespace `default`
- `REQ/S>1000`: Chỉ lấy thông tin khi số request/s lớn hơn 1000.

Các phép so sánh như sau:

- `=`: So sánh tương đương, không cần chính xác
- `==`: Chính xác với giá trị được so sánh
- `>`: Lớn hơn giá trị so sánh
- `>=`: Lớn hơn hoặc bằng
- `<`: Nhỏ hơn
- `<=`: Nhỏ hơn hoặc bằng

Chúng ta có thể xem chi tết các hướng dẫn khi bấm phím `h`.

![help_screen](https://hbase.apache.org/hbtop-images/help_screen.gif)

### Cách mà HBTop lấy được các thông số.

`hbtop` lấy các thông số từ CluserMetrics, kết quả trả về được gọi tới hàm `Admin#getClusterMetrics()`  trên` HMaster`. Nếu muốn lấy thêm các thông số khác, chúng sẽ cần được hiển thị thông qua `ClusterMetrics`.

### Tham khảo:
- https://blogs.apache.org/hbase/entry/introduction-hbtop-a-real-time
- https://hbase.apache.org/book.html#hbtop
