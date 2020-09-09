# Linux













- [CnBlogs](http://www.cnblogs.com/)

  

- [Home](http://www.cnblogs.com/xiaofengkang/)

  

- [New Post](http://i.cnblogs.com/EditPosts.aspx?opt=1)

  

- [Contact](http://msg.cnblogs.com/send/itbird)

  

- [Admin](http://i.cnblogs.com/)

  

- [Rss](http://www.cnblogs.com/xiaofengkang/rss)

- sudo aptitude clean --purge-unused

- sudo apt-get install gnome-control-center

- sudo apt-get install unity-control-center

Posts - 131 Articles - 0 Comments - 34 

# [ubuntu自动挂载硬盘方法](http://www.cnblogs.com/xiaofengkang/archive/2011/07/02/2096550.html)

首先建立挂载目录

例如：

sudo mkdir /movie #根目录下建立movie文件夹

sudo mkdir /work  #根目录下建立work文件夹

然后查看硬盘信息

sudo fdisk -l

Disk /dev/sda: 500.1 GB, 500107862016 bytes

255 heads, 63 sectors/track, 60801 cylinders

Units = cylinders of 16065 * 512 = 8225280 bytes

Disk identifier: 0x6fd074d4

  Device Boot   Start     End   Blocks  Id System

/dev/sda1  *      1    3251  26113626  7 HPFS/NTFS

/dev/sda2      3252    48177  360868095  f W95 Ext'd (LBA)

/dev/sda3      48178    60801  101402280  83 Linux

/dev/sda5      3252    8414  41471766  7 HPFS/NTFS

/dev/sda6      8415    21800  107523013+  7 HPFS/NTFS

/dev/sda7      21801    47934  209921323+  7 HPFS/NTFS

/dev/sda8      47935    48177   1951866  82 Linux swap / Solaris

Disk /dev/sdb: 320.0 GB, 320072933376 bytes

255 heads, 63 sectors/track, 38913 cylinders

Units = cylinders of 16065 * 512 = 8225280 bytes

Disk identifier: 0x4fd353d4

  Device Boot   Start     End   Blocks  Id System

/dev/sdb1        1    12876  103425446  7 HPFS/NTFS

/dev/sdb2      12876    38914  209151681  7 HPFS/NTFS

这里要把sda7 sdb2自动挂载在work和movie下

修改fstab文件

sudo vim /etc/fstab

sw       0    0

/dev/scd0    /media/cdrom0  udf,iso9660 user,noauto,exec,utf8 0    0

/dev/fd0    /media/floppy0 auto  rw,user,noauto,exec,utf8 0    0

**/dev/sda6    /doc      auto               0    0**

**/dev/sdb1    /xp       auto               0    0**

**/dev/sda7    /work      auto               0    0**

**/dev/sdb2    /movie     auto               0    0**

加黑是手动加入的

保存退出重新启动就ok了









$tar -zxvf ntfs-3g-1.2918.tgz

\#cd ntfs-3g-1.2918

\#./configure

\#make install

这个 ntfs-3g-1.2918.tgz文件是源码压缩包，和＊.tar.gz是一样的文件，所有用tar -zxvf ntfs-3g-1.2918.tgz.

注：有些tgz文件和tar.gz文件不一样，比如：slackware的tgz包，里边是已经编译好的二进制文件（当然也有些关于此软件的文本文件），里边可能还有安装用的脚本文件，还有包在安装时显示的说明文件。只有源码压缩包才一样。

之后在linux下建立个挂载点

\#mkdir /mnt/windows

\#cd /mnt/windows

\#mkdir c d e f g

分别建立windows的盘符，然后挂载

\#mount -t ntfs-3g /dev/sda1 /mnt/windows/c

\#cd /mnt/windows/c

\#ls

就能看到windows的C盘文件了，如果有这个盘符的话

卸载命令：

\#umount -t ntfs-3g /mnt/windows/c

呵呵，很简单的命令吧

顺便说一下linux系统下看到的盘符

\#fdisk -l

vim /etc/profile



export JAVA_HOME=/usr/software/jdk1.8.0_65



export JRE_HOME=/usr/software/jdk1.8.0_65/jre



export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib



export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin

source /etc/profile



sudo dpkg -l|grep ^rc|awk '{print $9}'|xargs apt-get --purge remove -y



sudo mount -t ntfs-3g /dev/sda2 /usr/c -ro force

grub> rootnoverify (hd0,0)

grub> chainloader +1

grub> boot

sudo gedit /boot/grub/grub.cfg

\### BEGIN /etc/grub.d/30_os_prober ### 

menuentry 'Windows 7 Ultimate' { 

   insmod part_msdos 

  insmod ntfs 

  set root='(hd0,msdos2)' 

  chainloader +1 

} 

\### END /etc/grub.d/30_os_prober ### 





- menuentry "Windows XP" { 

- insmod chain 

- insmod ntfs 

- search --fs-uuid --set 10ac99d8ac99b8a4 

- chainloader +1 

- } 

  

GRUB的重装[方法](http://www.jb51.net/)有很多，这种[方法](http://www.jb51.net/)不行，换一种试下:

1.用[安装](http://www.jb51.net/)光盘启动,选升级[安装](http://www.jb51.net/),再只选[安装](http://www.jb51.net/)GRUB行了。

2.用[安装](http://www.jb51.net/)光盘启动,到BOOT那里输入linux rescue也就是进入救援模式，到出现#命令提示符时，输入chroot /mnt/sysimage,然后再输入grub-install /dev/hda,搞定...

3.没有软驱如何修复grub/lilo引导菜单？

a.把第一张linux安装盘里的dosutils目录复制到windows盘中。如果是iso可以用winrar3提取。

b.进入纯dos，进入dosutils目录，执行loadlin autoboot/vmlinuz root=/dev/hdxx()hdxx是你的linux根分区。这样就能进入linux。

c.执行grub-install /dev/hdx(x=a,b,c,d) 或lilo即可以重写引导。



idea.lanyus.com



今天打算删掉已经不好使的vmware，于是上网找到了段手动卸载的博文，并成功完成卸载。

下面写一下过程：

**1.先查看安装的虚拟机**

**vmware-installer -l**

然后会显示版本和产品名称

Product Name      Product Version   

====================== ====================

vmware-workstation   7.0.1.227600 

**2.卸载虚拟机**

**sudo vmware-installer --uninstall-product vmware-workstation**

 

然后根据提示点两下就ok了。

**PS：收藏一下，说不定下次还会用得到…… - -** 

http://repo.springsource.org/libs-release-local/org/springframework/spring/4.2.3.RELEASE/


