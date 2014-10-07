# Centos 6下通过nicstat进行网络流量统计

## 1. 用途说明
nicstat is to network interfaces as “iostat” is to disks, or “prstat” is to processes.  
nicstat原本是Solaris平台下显示网卡流量的工具，Tim Cook将它移植到linux平台，官方网站见 [这里](https://blogs.oracle.com/timc/entry/nicstat_the_solaris_and_linux)。  
相比netstat, 他有以下关键特性：

- Reports bytes in & out as well as packets.
- Normalizes these values to per-second rates.
- Reports on all interfaces (while iterating)
- Reports Utilization (rough calculation as of now)
- Reports Saturation (also rough)
- Prefixes statistics with the current time


## 2. 安装
#### 2.1 获取源码包
`wget http://cznic.dl.sourceforge.net/project/nicstat/nicstat-1.95.tar.gz`

#### 2.2 修改Makefile
因为默认编译环境为32位，需要对Makefile进行修改以支持CentOS 6.x的64位系统。  
具体查看diff结果:
```
$ diff Makefile Makefile.Linux
23c23
< CMODEL =	-m64
---
> CMODEL =	-m32
33,34c33,34
< OSREL =		6
< CPUTYPE =	x86_64
---
> OSREL =		5
> CPUTYPE =	i386
```

#### 2.3 安装
```
tar -xzf nicstat-1.95.tar.gz
cd nicstat-1.95
cp Makefile.Linux Makefile # 此时按照上面的diff值修改Makefile文件
make
make install # 安装需要root权限
```
## 3. 用法
```
# 以KBytes大小查看网卡流量
$ nicstat 1
    Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
11:49:32       lo    1.02    1.02   17.62   17.62   59.10   59.10  0.00   0.00
11:49:32     eth0   12.32   441.1   67.51   347.6   186.8  1299.3  0.00   0.00
    Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
11:49:33       lo    0.00    0.00    0.00    0.00    0.00    0.00  0.00   0.00
11:49:33     eth0    2.58    2.85   36.95   35.96   71.62   81.31  0.00   0.00
    Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
11:49:34       lo    0.55    0.55   10.00   10.00   56.70   56.70  0.00   0.00
11:49:34     eth0    3.83    4.07   52.99   52.99   73.94   78.57  0.00   0.00

# 以Mbits大小查看网卡流量
$ nicstat -M 1
    Time      Int   rMbps   wMbps   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
11:53:17       lo    0.01    0.01   17.62   17.62   59.10   59.10  0.00   0.00
11:53:17     eth0    0.10    3.45   67.51   347.6   186.8  1299.3  0.00   0.00
    Time      Int   rMbps   wMbps   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
11:53:18       lo    0.01    0.01   19.97   19.97   58.15   58.15  0.00   0.00
11:53:18     eth0    0.04    0.04   70.91   69.91   71.69   76.54  0.00   0.00
    Time      Int   rMbps   wMbps   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
11:53:19       lo    0.01    0.01   20.00   20.00   58.25   58.25  0.00   0.00
11:53:19     eth0    0.03    0.03   56.99   56.99   71.81   77.18  0.00   0.00

# 统计tcp连接的情况
$ nicstat -t 1
11:55:19    InKB   OutKB   InSeg  OutSeg Reset  AttF %ReTX InConn OutCon Drops
TCP         0.00    0.00   77.95   77.44  0.04  0.04 0.000   1.76   13.3  0.00
11:55:20    InKB   OutKB   InSeg  OutSeg Reset  AttF %ReTX InConn OutCon Drops
TCP         0.00    0.00   84.87   84.87  0.00  0.00 0.000   2.00   14.0  0.00
11:55:21    InKB   OutKB   InSeg  OutSeg Reset  AttF %ReTX InConn OutCon Drops
TCP         0.00    0.00   84.96   86.96  0.00  0.00 0.000   3.00   14.0  0.00

# 统计udp连接的情况
$ nicstat -u 1
11:55:57                    InDG   OutDG     InErr  OutErr
UDP                         0.50   286.8      0.00    0.00
11:55:58                    InDG   OutDG     InErr  OutErr
UDP                         0.00    0.00      0.00    0.00
11:55:59                    InDG   OutDG     InErr  OutErr
UDP                         0.00    0.00      0.00    0.00
```
