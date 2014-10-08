## 用途和主要的分析项目
oprofile 是一个性能监控工具, 支持linux内核2.6以上的系统. 可以在多种系统架构上使用.
oprofile 提供如下特性:

- 分析工具
- 为分析profile数据提供后期处理工具
- 事件计数

oprofile 可以监控到运行系统中的所有原生硬件事件.包括:内核(组件和处理中断), 共享类, 二进制文件.
oprofile 可以以非常小的开销在后台收集整个系统的所有事件信息. 这些特性使它非常适合监控整个系统, 以分析出实际系统运行中的瓶颈.
很多 CPU 自身提供性能计数器, 他们的硬件寄存器可以对事件计数. 例如: 缓存失效数, CPU周期等. 
oprofile 以基于代码的方式收集正在发生的事件数, 性能计数器的值会记录下每个周期内发生的事件数. 将这些信息放入到一个profile的二进制文件.
另外, oprofile的事件计数工具可以收集简单的原始事件统计.

- 支持动态编译代码(如:JIT)
- 不支持虚拟机(但支持kvm的虚拟机)

## 安装方法
#### 编译安装
###### 新增用户
```
sudo groupadd oprofile -g 700
sudo useradd -g oprofile oprofile -u 700 -s /bin/fasle

# 需要系统内有oprofile的用户和组, 否则将有如下警告
Warning: The user account 'oprofile:oprofile' does not exist on the system.
         To profile JITed code, this special user account must exist.
         Please ask your system administrator to add the following user and group:
             user name : 'oprofile'
             group name: 'oprofile'
         The 'oprofile' group must be the default group for the 'oprofile' user.
```

###### 安装依赖
sudo yum install binutils-devel popt-devel  popt

###### 编译安装
```
wget http://prdownloads.sourceforge.net/oprofile/oprofile-1.0.0.tar.gz
tar -xzf oprofile-1.0.0.tar.gz
cd oprofile-1.0.0
./configure
make
sudo make install
```
#### yum安装
` yum install oprofile `

## 使用方法
参考1:http://oprofile.sourceforge.net/examples/  
参考2:http://oprofile.sourceforge.net/docs/  