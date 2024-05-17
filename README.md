# Vmem-disk

虚拟磁盘 Linux 5.15 Ubuntu 测试

1. 测试环境
```sh
uname -a
Linux damone 5.15.0-100-generic #110-Ubuntu SMP Wed Feb 7 13:27:48 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```

2. make

```sh
 make
make -C /lib/modules/5.15.0-100-generic/build M=/home/damone/project/linux-drivers-study/code/io-labs/lab2 modules
make[1]: 进入目录“/usr/src/linux-headers-5.15.0-100-generic”
  CC [M]  /home/damone/project/linux-drivers-study/code/io-labs/lab2/vmem_disk.o
  MODPOST /home/damone/project/linux-drivers-study/code/io-labs/lab2/Module.symvers
  CC [M]  /home/damone/project/linux-drivers-study/code/io-labs/lab2/vmem_disk.mod.o
  LD [M]  /home/damone/project/linux-drivers-study/code/io-labs/lab2/vmem_disk.ko
  BTF [M] /home/damone/project/linux-drivers-study/code/io-labs/lab2/vmem_disk.ko
Skipping BTF generation for /home/damone/project/linux-drivers-study/code/io-labs/lab2/vmem_disk.ko due to unavailability of vmlinux
make[1]: 离开目录“/usr/src/linux-headers-5.15.0-100-generic”
```

3. 安装模块
```sh
sudo insmod ./vmem_disk.ko
sudo fdisk -l  

Disk /dev/vramdisk：50 MiB，52428800 字节，102400 个扇区
单元：扇区 / 1 * 512 = 512 字节
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
```

4.测试文件系统

sudo mkfs.ext2 /dev/vramdisk
```sh
mke2fs 1.46.5 (30-Dec-2021)
创建含有 12800 个块（每块 4k）和 12800 个 inode 的文件系统

正在分配组表： 完成                            
正在写入 inode表： 完成                            
写入超级块和文件系统账户统计信息： 已完成
```

5. 测试设置交换分区
``` sh
sudo mkswap /dev/vramdisk
mkswap: /dev/vramdisk：警告，将擦除旧的 ext2 签名。
正在设置交换空间版本 1，大小 = 50 MiB (52424704  个字节)
无标签， UUID=0da64c02-8205-4c78-ab29-5aa248f62ce0

sudo swapon /dev/vramdisk

swapon --show
NAME          TYPE       SIZE USED PRIO
/dev/sda6     partition 37.5G 7.9G   -2
/dev/vramdisk partition   50M   0B   -3
```

