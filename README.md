## 自动化巡检系统说明书

#### 巡检项设计

| System    | Usages    | Services  | Networks | Securitys |
| ----------|:---------:| :---------:| :-------:| :--------:|
| 系统重启   | 磁盘       |  端口      | TCP状态  |  系统用户  |
| Dmesg     | 内存       |  服务进程  |  流量    |  Login   | 
|           | CPU       |  服务日志   |
|           | 进程       |


#### 巡检命令

> 系统重启时间

    who -b |awk '{print $(NF-1),$NF}'
    who -b |awk '{print $(NF-1)}'

> 系统硬件异常信息

     dmesg |grep -Pi 'error|Fail'

> 磁盘空间 >70％ 

     /bin/df -Phm | /bin/grep -vP 'Filesystem|none|udev|tmpfs|ossfs|mnt' | awk '{print $(NF-1)}' | cut -d'%' -f1 | awk '{if($0 > 70) print "1"}' | head -1

> 磁盘空间inode >70％

     /bin/df -i |egrep -v 'Filesystem|none|udev|tmpfs|ossfs' | awk '{print $(NF-1)}' | cut -d'%' -f1 | awk '{if($0 > 70) print "1"}' | head -1

>  CPU (idle / iowait )

     /usr/bin/sar -u 1 2 | grep "Average:" | awk '{if ($NF < 40 || $(NF-3) > 30) print "1"}'

> 进程数

      ps -ef | egrep -v "^root" | wc -l

#### 结果输出标准
------
* 巡检时间
  `2017-6-12 15:39`

* 自动化巡检结果汇总


|  Systems:  |    正常(`100`%) |     异常 `0` | 
| ----  | ---- | ---- |
|  Usages:    |  正常(`99`%)   |   异常 `1` | 
| Services:  |  正常(`100`%)  |  异常 `0` |
 | Networks: |   正常(`70`%)  |   异常 `3` |
|  Securitys: |  正常(`100`%)  |  异常 `0` |

*  异常代码
     
        `XXX-XXX-01000`
        `XXX-XXX-00050`

* 巡检参考说明：
   github site:

------
