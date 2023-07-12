## Chuyển NameNode sang máy chủ khác (Không downtime)

## Đặt vấn đề

Giả sử; ta có 05 máy chủ chạy Hadoop HDFS. Ban đầu; ta cài đặt các role của Hadoop HDFS lộn xộn như sau:

| Node\Role | Namenode | Journalnode | Zookeeper | Datanode |
| -- | -- | -- | -- | -- |
| node1 | | x | x | x | x |
| node2 | | x | x | x | x |
| node3 | | x | x | x | x |
| node4 | x | | | | x |
| node5 | x | | | | x |

Một thời gian sau, ta xem xét lại hiệu năng, mức độ sử dụng của các máy chủ HDFS và thấy rằng cụm chạy 05 node còn dư thứa khá nhiều tài nguyên, tốc độ dữ liệu phát sinh cũng không lớn. Vì thế, ta lên kế hoạch để cắt giảm tài nguyên để tránh lãng phí.

Quy hoạch lại, ta sẽ cắt giảm 02 node `node4` và `node5` có bảng như sau:

| Node\Role | Namenode | Journalnode | Zookeeper | Datanode |
| -- | -- | -- | -- | -- |
| node1 | x | x | x | x | x |
| node2 | x | x | x | x | x |
| node3 | | x | x | x | x |
| node4 | | | | | |
| node5 | | | | | |

Trước đó, ta sẽ 'graceful' 02 datanode để đảm bảo dữ liệu được toàn vẹn theo [hướng dẫn](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDataNodeAdminGuide.html#Host-level_settings)

### Các bước tiến hành
- Chuẩn bị máy chủ mới `node1` (Cài đặt Hadoop; tạo và phân quyền cho user, tạo thư mục chứa dữ liệu Namenode,...)
- Tắt Namenode Standby và ZKFC (Ví dụ: `node5`)
- Thay đổi cấu hình trên toàn bộ các DataNode (`node1` -> `node3`)
- Bật Namenode mới `node1`
    - Bootstrap Namenode Standby: `hdfs namenode -bootstrapStandby`
    - Khởi động Namenode: `hdfs --daemon start namenode`
    - Khởi động ZKFC: `hdfs --daemon start zkfc`
- Khởi động lại lần lượt các Datanode (`node1` -> `node3`) để nhận Namenode Standby
  - Làm lần lượt từng máy; kiểm tra bằng câu lệnh `hdfs fsck /` và tra log trong suốt quá trình
- Kiểm tra log của Namenode Standby cho tới khi Namenode mới nhận hết thông tin báo cáo từ các Datanode và tự động thoát `Safemode`
- Failover sang Namenode mới `node1` và kiểm tra log
  - Ví dụ: `hdfs haadmin -failover nn1 nn2`

Thực hiện lại với `node2`

Sau khi hoàn thành, ta kiểm tra lại 2 `node4` và `node5` xem còn tiến trình nào đang chạy. Nếu không, ta có thể tắt máy và giải phóng tài nguyên.

*Hà Nội, ngày 12 tháng 07 năm 2023*
