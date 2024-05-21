### Ghi chép linh tinh

#### Xóa CF của bảng

> alter 'sample_table','delete'=>'cf'

Ref: https://stackoverflow.com/q/36132336

#### Pre-split và ngăn không cho auto-split

```
create 'test', 'cf', {SPLITS => ['1', '2', '3', '4', '5', '6', '7', '8', '9']}
alter 'test',  {NAME => 'cf', COMPRESSION => 'GZ', VERSIONS => 1 }
alter 'test', {METADATA => {'SPLIT_POLICY' => 'org.apache.hadoop.hbase.regionserver.DisabledRegionSplitPolicy'}}
```

#### Kích hoạt lại tính năng auto-spilt

```
alter 'test', METHOD => 'table_att_unset', NAME => 'SPLIT_POLICY'
```
