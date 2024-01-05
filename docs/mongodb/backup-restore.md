# Backup & Restore

\
## Sao lưu dữ liệu

Sử dụng `mongodump` kèm tùy chọn `--arrchive` để gom các file backup vào thành 1 file; sau đó dùng `pigz` để nén dữ liệu nhằm mục đích tăng tốc độ nén

```javascript
mongodump --host 10.10.10.10 --port 27017 \
-d adtech_db \
-j 10 \
--archive=/data/mongodb/backup/adtech_db.new.archive

pigz -v -9 /data/mongodb/backup/adtech_db.archive
```

## Khôi phục dữ liệu

Ta sử dụng `mongorestore` để khôi phục dữ liệu:

### Khôi phục toàn bộ database

Ở ví dụ này; ta sẽ khôi phục lại dữ liệu trên một máy chủ/cụm MongoDB mới:

```javascript
mongorestore --host 10.10.20.20 --port 27017 \
--nsFrom 'adtech_db.*' \
--nsTo 'adtech_db_new.*' \ 
--archive=/data/mongodb/backup/adtech_db.archive.gz \
--gzip
```

**Chú ý**:

* `--nsFrom`: Tên DB cũ kèm theo subfix \* - tất cả các collection
* `--nsTo` :  Tên DB mới kèm theo subfix \* - tất cả các collection
* `--archive`: Chỉ ra file archive đã backup trước đó
* `--gzip`: Sử dụng gzip để giải nén 


### Khôi phục 01 colection của database

```javascript
mongorestore --host 10.10.20.20 --port 27017 \
--nsFrom 'adtech_db.*' \
--nsTo 'adtech_db_new.*' \ 
--nsInclude="adtech_db.collection1" \
--archive=/data/mongodb/backup/adtech_db.archive.gz \
--gzip
```

**Chú ý**:

* `--nsInclude`: Chỉ ra collection sẽ restore


## Tham khảo thêm:

* <https://www.mongodb.com/docs/database-tools/mongodump/>
* <https://www.mongodb.com/docs/database-tools/mongorestore/>


