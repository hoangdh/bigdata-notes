## Cấu hình Rack trong Hadoop

- Script xác định rack `rack-topology.sh`

```
#!/bin/bash
# Adjust/Add the property "net.topology.script.file.name"
# to core-site.xml with the "absolute" path the this
# file. ENSURE the file is "executable".

# Supply appropriate rack prefix
RACK_PREFIX=default

# To test, supply a hostname as script input:
if [ $# -gt 0 ]; then

CTL_FILE=${CTL_FILE:-"rack_topology.data"}

HADOOP_CONF_DIR=${HADOOP_CONF_DIR:-"/etc/hadoop/conf"} 

if [ ! -f ${HADOOP_CONF_DIR}/${CTL_FILE} ]; then
 echo -n "/$RACK_PREFIX/rack "
 exit 0
fi

while [ $# -gt 0 ] ; do
 nodeArg=$1
 exec< ${HADOOP_CONF_DIR}/${CTL_FILE}
 result=""
 while read line ; do
 ar=( $line )
 if [ "${ar[0]}" = "$nodeArg" ] ; then
 result="${ar[1]}"
 fi
 done
 shift
 if [ -z "$result" ] ; then
 echo -n "/$RACK_PREFIX/rack "
 else
 echo -n "/$RACK_PREFIX/rack_$result "
 fi
done

else
 echo -n "/$RACK_PREFIX/rack "
fi
```

- File `rack_topology.data` chỉ định Rack cho các Datanode

Trong ví dụ, ta có 3 Rack `vn-han1`, `vn-han2` và `vn-han3` và các máy chủ tương ứng.

```
10.10.10.1 vn-han1
10.10.10.2 vn-han1
10.10.10.3 vn-han1
10.10.20.1 vn-han2
10.10.20.2 vn-han2
10.10.20.3 vn-han2
10.10.30.1 vn-han3
10.10.30.2 vn-han3
10.10.30.3 vn-han3
```

- Sau khi có 2 file trên, ta đưa chúng vào thư mục cấu hình của Hadoop - `$HADOOP_HOME/etc/hadoop`
- Thêm cấu hình sau vào `$HADOOP_HOME/etc/hadoop/core-site.xml`

```
...
<property>
  <name>net.topology.script.file.name</name> 
  <value>/etc/hadoop/conf/rack-topology.sh</value>
</property>
...
```

**Lưu ý**: Thay đổi đường dẫn file đúng với môi trường

- Khởi động lại NameNode
