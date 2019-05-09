zhezhongfangfa neiyou yunxing chenggong
这种方法没有运行成果，应该用CPIO


 cd menu
  811  gcc -o init test.c menu.c linktable.c -m32 -static
  812  gcc -o init test.c menu.c linktable.c -m32 -static -lpthread
  813  cp init ../rootfs/
  814  find . | cpio -o -Hnewc |gzip -9 > ../rootfs.img
  815  cd ../rootfs
  816  find . | cpio -o -Hnewc |gzip -9 > ../rootfs.img
  817  cd ..
  818  ls
  819  qemu -kernel linux-3.18.6/arch/x86/boot/bzImage -initrd rootfs.img




find .|cpio -o -H newc|gzip>~/myinitramfs.gz
1. find . 查找显示当前目录下的所有文件、文件夹
2. | 管道，将 | 左边命令的结果（也就是find . 得到的所有文件、文件夹路径名称）传给右边(也就cpio命令)
3. cpio -o -H newc cpio是将文件系统打包或解包， -o 是打包 ，-H 指定格式 为newc
newc The new (SVR4) portable format, which supports file systems having more than 65536 i-nodes. (4294967295 bytes)
4 | 将cpio打好的包传给gzip压缩
5 gzip 压缩命令的一种，gnuzip。类似zip，rar。
6 >~/myinitramfs.gz 将压缩后的数据 存为 文件myinitramfs.gz
