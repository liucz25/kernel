# kernel

这种方法不行

linux rootfs.img的制作
2016年12月01日 16:20:08 随风奔跑的小蜗牛 阅读数：3217
linux rootfs.img的制作
cramfs是只读压缩的文件系统，文件系统类型可以是ext2，ext3，什么的， 
cramfs和romfs只是一个文件系统类型，ramdisk相当于一块硬盘空间，可以理解为在内存中虚拟出一块硬盘来，所以它上面就可以有你linux支持的各种文件系统什么的。所以你问的，它和romfs和cramfs确实不是一个层次的概念。 ^-^恭喜你，你答对了的根文件系统的目录是 rootfs (你将来要用到的所有的文件就在这里） 
like this ： mkcramfs rootfs rootfs.cramfs 就搞定了。如名字所言，它是只读压缩，所以比较省空间，如果你的flash比较小，就用这个吧！ 系统启动后，kernel把他load到内存中，解压 ，所以比较占内存。看你的需要了。 

而ramdisk呢？ 这个用的比较多，ramdisk相当于一块硬盘空间，可以理解为在内存中虚拟出一块硬盘来，所以它上面就可以有你linux支持的各种文件系统什么的。所以你问的，它和romfs和cramfs确实不是一个层次的概念。 关键是以后，在ramdisk里面可以写，这是一个和cramfs重要的区别了。 
具体制作方法： 
dd if=/dev/zero of=rootfs.img bs=1M count=100 一个整数(看你的实际的需要的空间了，一般也就10M) 
把它格式化为你需要的文件系统，比如 ext2 ,ext3 ,reiserfs 什么的， 
比如ext3 : mkfs.ext3 -m 0 -O none -F root.img 
然后把它mount到某个目录，比如tmp 吧： 
mount -o loop root.img /tmp/ 

然后，你的文件系统所在的目录的所有文件copy到tmp目录下： 比如你的文件系统目录在/root/rootfs-test : 
cp -av /root/rootfs-test/* /tmp/ （这里注意一个细节：copy的时候，用参数a表示copy全部，v表示只copy链接本身，不copy它指向的内容，这点很关键哦!) ,另外，有的人常用：cp -pdR 这个你也可以试试，意思就是原来什么样，copy过去就什么样。 

然后卸载/tmp/ 目录就好了。 
umount /tmp 

一般的情况下，ramdisk是要压缩的，对于上面的生成好的img， rootfs.img ,你可以这样压缩： 
gzip -v9 rootfs.img 会自动生成rootfs.img.gz ，一般压缩率，30％吧！
