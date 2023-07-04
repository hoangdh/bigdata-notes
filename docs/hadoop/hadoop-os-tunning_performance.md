## [Best Practices: Linux File Systems for HDFS](https://community.cloudera.com/t5/Community-Articles/Best-Practices-Linux-File-Systems-for-HDFS/ta-p/245284)

### ISSUE:

Choosing the appropriate Linux file system for HDFS deployment

### SOLUTION:

The Hadoop Distributed File System is platform independent and can function on top of any underlying file system and Operating System. Linux offers a variety of file system choices, each with caveats that have an impact on HDFS. As a general best practice, if you are mounting disks solely for Hadoop data, disable ‘noatime’. This speeds up reads for files. There are three Linux file system options that are popular to choose from:

- Ext3
- Ext4
- XFS

Yahoo uses the ext3 file system for its Hadoop deployments. ext3 is also the default filesystem choice for many popular Linux OS flavours. Since HDFS on ext3 has been publicly tested on Yahoo’s cluster it makes for a safe choice for the underlying file system. ext4 is the successor to ext3. ext4 has better performance with large files. ext4 also introduced delayed allocation of data, which adds a bit more risk with unplanned server outages while decreasing fragmentation and improving performance. XFS offers better disk space utilization than ext3 and has much quicker disk formatting times than ext3. This means that it is quicker to get started with a data node using XFS.

Most often performance of a Hadoop cluster will not be constrained by disk speed – I/O and RAM limitations will be more important. ext3 has been extensively tested with Hadoop and is currently the stable option to go with. ext4 and xfs can be considered as well and they give some performance benefits.

References:
- http://wiki.apache.org/hadoop/DiskSetup
- http://hadoop-common.472056.n3.nabble.com/Hadoop-performance-xfs-and-ext4-td742325.html
- http://www.quora.com/What-are-the-advantages-and-disadvantages-of-the-filesystems-ext2-ext3-ext4-ReiserFS-and-XFS

## [HDFS Settings for Better Hadoop Performance](https://community.cloudera.com/t5/Community-Articles/HDFS-Settings-for-Better-Hadoop-Performance/ta-p/245799)

### Use recommended mount options for all HDFS data disks

There are specific filesystem mount options that have proven to be more efficient for Hadoop clusters. Using these mount options will provide performance benefits. Since mount options are applied when mounting the filesystem, on system boot or remounting for example, changes to /etc/fstab alone are not enough for these settings to take effect. The recommended approach is to make the mount option changes and either manually remount the individual file systems or reboot the host for the settings to take effect.

Use the following mount options for the respective file systems:

```
ext4 —> "inode_readahead_blks=128","data=writeback","noatime","nodev"
xfs —> "noatime"
```

### Configure HDFS block size for optimal performance

Having optimal HDFS block size boosts NameNode performance as well as job execution performance.

Make sure that the blocksize ('dfs.blocksize'  in 'hdfs-site.xml') is within the recommended range of 134217728 to 1073741824 (exclusive).

### Enable HDFS short circuit reads

In HDFS, reads normally go through the DataNode. Thus, when the client asks the DataNode to read a file, the DataNode reads that file off of the disk and sends the data to the client over a TCP socket. So-called short-circuit reads bypass the DataNode, allowing the client to read the file directly. Obviously, this is only possible in cases where the client is co-located with the data. Short-circuit reads provide a substantial performance boost to many applications.

### Enable short circuit read for better performance. To configure short-circuit local reads, you will also need to enable libhadoop.so (dfs.domain.socket.path).

In hdfs-site.xml set the below:

```
dfs.client.read.shortcircuit=true
dfs.domain.socket.path=/var/lib/hadoop-hdfs/dn_socket
```

### Avoid file sizes that are smaller than a block size

Average block size should be greater than the recommended block size of 67108864 MB. An average size below the recommended size adds more burden to the NameNode, cause heap/GC issues in addition to cause storage and processing to be inefficient.

Set the block size greater than 67108864 MB

Also, use one or more of the following techniques to consolidate smaller files :
- run Hive/HBase compactions
- merge small files
- use HAR to compact small files

### Tune DataNode JVM for optimal performance

DataNode is sensitive to the JVM performance and behavior. Make sure that the DataNode JVM is configured for optimal performance

Sample JVM Configs:

```
-Djava.net.preferIPv4Stack=true, 
-XX:ParallelGCThreads=8, 
-XX:+UseConcMarkSweepGC, 
-Xloggc:*,
-verbose:gc, 
-XX:+PrintGCDetails, 
-XX:+PrintGCTimeStamps, 
-XX:+PrintGCDateStamps
```
Also:

-Xms should be same as -Xmx
New generation size should be ⅛ of the total JVM size.

### Avoid reads or write from stale DataNodes

DataNodes that have not sent a heartbeat to NameNode for a defined interval, may be under load or may have died. Avoid sending any read/write requests to such 'stale' DataNodes.

In hdfs-site.xml set the below:

```
dfs.namenode.avoid.read.stale.datanode=true
dfs.namenode.avoid.write.stale.datanode=true
```

### Use JNI-based group lookup over other implementations

Hadoop uses a pluggable interface with multiple possible implementations for looking up the group memberships of a user. The JNI-based implementation has better performance characteristics than other implementations.

In core-site.xml set:

```
hadoop.security.group.mapping=org.apache.hadoop.security.JniBasedUnixGroupsMappingWithFallback
```

*** This article focuses on settings that would improve HDFS performance. However, may impact other areas such as stability and uptime. Please understand the settings before applying them ***

*** You might also be interested in the following articles: ***

## [OS Configurations for Better Hadoop Performance](https://community.cloudera.com/t5/Community-Articles/OS-Configurations-for-Better-Hadoop-Performance/ta-p/247300)

### Disable Transparent Huge Pages (THP)

Transparent Huge Pages (THP) is a Linux memory management system that reduces the overhead of Translation Lookaside Buffer (TLB) lookups on machines with large amounts of memory by using larger memory pages.

However THP feature is known to perform poorly in Hadoop cluster and results in excessively high CPU utilization.

```
Disable THP to reduce the amount of system CPU utilization on your worker nodes.  This can be done by ensuring that both proc entries are set to [never] instead of [always].
```

### Use Recommended File System Types

Some file systems offer better performance and stability than others. As such, the HDFS dfs.datanode.data.dir and YARN yarn.nodemanager.local-dirs should be configured to use mount points that are not formatted with the most optimal file systems.

Take a look at this article on file system choices: https://community.hortonworks.com/articles/14508/best-practices-linux-file-systems-for-hdfs.html

### Disable Host Swappiness

The Linux kernel provides a tweakable setting that controls how often the swap file is used, called swappiness.

A swappiness setting of zero means that the disk will be avoided unless absolutely necessary (when host runs out of memory), while a swappiness setting of 100 means that programs will be swapped to disk almost instantly.

Reducing the value for swappiness reduces the likelihood that the Linux kernel will push application memory from memory into swap space. Swap space is much slower than memory as it is backed by disk instead of RAM. Processes that are swapped to disk are likely to experience pauses, which may cause issues and missed SLAs.

```
Add `vm.swappiness=0` to /etc/sysctl.conf and reboot for the change to take effect. 

Or you can also change the value while your system is still running `sysctl -w vm.swappiness=0`. 

Also clear your swap by running `swapoff -a` and then `swapon -a` as root instead of rebooting to achieve the same effect.
```

### Improve Virtual Memory Usage

The vm.dirty_background_ratio and vm.dirty_ratio parameters control the percentage of system memory that can be filled with memory pages that still need to be written to disk. Ratios too small force frequent IO operations, and too large leave too much data stored in volatile memory, so optimizing this ration is a careful balance between optimizing IO operations and reducing risk of data loss.

```
Update vm.dirty_background_ratio=20 and vm.dirty_ratio=50 in /etc/sysctl.conf and reboot for the change to take effect, or change the values while your system is still running using `sysctl -p`.
```

### Configure CPUs for Performance Scaling

CPU Scaling is configurable and defaults commonly to favor power saving over performance. For Hadoop clusters, it is important that we configure then for better performance over other options.

```
Please set scaling governors to performance, which means running the CPU at maximum frequency.  To do so run `cpufreq-set -r -g performance` OR edit /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor and set the content to 'performance'
```

### Tune SSD Configurations

SSDs provide great performance boost. If configured optimally for Hadoop workloads, they can provide even better results. Scheduler, read buffers, number of requests etc are the parameters to consider for tuning.

Refer following link for further details: https://wiki.archlinux.org/index.php/Solid_State_Drives#I.2FO_Scheduler

```
For all of SSD devices, set following things 

echo 'deadline' > {{device}}/queue/scheduler ; 

echo '256' > {{device}}/queue/read_ahead_kb ; 

echo '256' > {{device}}/queue/nr_requests ;
```
