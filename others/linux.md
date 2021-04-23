
# rsync
[15个Rsync命令实例](http://www.cnblogs.com/itech/archive/2011/06/04/2072505.html)

[Rsync’ing through an SSH tunnel](http://toddharris.net/blog/2005/10/23/rsyncing-through-an-ssh-tunnel/)

http://stackoverflow.com/questions/16654751/rsync-through-ssh-tunnel

http://blog.chinaunix.net/uid-29578485-id-5300984.html



## manul
https://download.samba.org/pub/rsync/rsync.html



## sample

rsync -z  --partial --progress --bwlimit 300k  root@115.236.59.88:/data1/vm-images/jira-wiki-qcow2.img ./



#open ssl
openssl  enc -e -aes256  -pass "pass:66xx88YY,,"    -in /cygdrive/c/Users/mfty1/Downloads/110.zip -out /cygdrive/c/Users/mfty1/Downloads/110-zsec
openssl  enc -d -aes256  -pass "pass:66xx88YY,,"    -in /cygdrive/c/Users/mfty1/Downloads/110-zsec -out /cygdrive/c/Users/mfty1/Downloads/110-x.zip


# vi
change format  

    set ff?    set ff=unix

show line number

    se nu

# rsync
[15个Rsync命令实例](http://www.cnblogs.com/itech/archive/2011/06/04/2072505.html)

[Rsync’ing through an SSH tunnel](http://toddharris.net/blog/2005/10/23/rsyncing-through-an-ssh-tunnel/)

http://stackoverflow.com/questions/16654751/rsync-through-ssh-tunnel

http://blog.chinaunix.net/uid-29578485-id-5300984.html



## manul
https://download.samba.org/pub/rsync/rsync.html



## sample

rsync -z  --partial --progress --bwlimit 300k  root@115.236.59.88:/data1/vm-images/jira-wiki-qcow2.img ./



# ssh

## 配置步骤

1. 生成公钥密钥对        ssh-keygen -b 4096 -f git_rsa
2. 拷贝公钥到目标服务器（制定目标的用户，源服务器）   ssh-copy-id -i git_rsa git@git.example.com
3. 配置本地 ssh 配置文件  

          ~/.ssh/config file:
          Host git git.example.com
            User git
            IdentityFile /home/thomas/.ssh/git_rsa


## 异常日志
sshd
/var/log/secure

## ssh-agent
An Illustrated Guide to SSH Agent Forwarding
http://www.unixwiz.net/techtips/ssh-agent-forwarding.html


## 反向代理
利用ssh反向代理以及autossh实现从外网连接内网服务器

https://www.cnblogs.com/kwongtai/p/6903420.html




# bash

##linux shell ${}简单用法（转自vbird)
 > http://unixboy.iteye.com/blog/499329

	为了完整起见，我这里再用一些例子加以说明 ${ } 的一些特异功能：
	假设我们定义了一个变量为：
	file=/dir1/dir2/dir3/my.file.txt
	我们可以用 ${ } 分别替换获得不同的值：
	${file#*/}：拿掉第一条 / 及其左边的字符串：dir1/dir2/dir3/my.file.txt
	${file##*/}：拿掉最后一条 / 及其左边的字符串：my.file.txt
	${file#*.}：拿掉第一个 . 及其左边的字符串：file.txt
	${file##*.}：拿掉最后一个 . 及其左边的字符串：txt
	${file%/*}：拿掉最后条 / 及其右边的字符串：/dir1/dir2/dir3
	${file%%/*}：拿掉第一条 / 及其右边的字符串：(空值)
	${file%.*}：拿掉最后一个 . 及其右边的字符串：/dir1/dir2/dir3/my.file
	${file%%.*}：拿掉第一个 . 及其右边的字符串：/dir1/dir2/dir3/my
	记忆的方法为：
	# 是去掉左边(在鉴盘上 # 在 $ 之左边)
	% 是去掉右边(在鉴盘上 % 在 $ 之右边)
	单一符号是最小匹配﹔两个符号是最大匹配。

	${file:0:5}：提取最左边的 5 个字节：/dir1
	${file:5:5}：提取第 5 个字节右边的连续 5 个字节：/dir2
	我们也可以对变量值里的字符串作替换：
	${file/dir/path}：将第一个 dir 提换为 path：/path1/dir2/dir3/my.file.txt
	${file//dir/path}：将全部 dir 提换为 path：/path1/path2/path3/my




	## history

	```
	出去行号
	cat  /cygdrive/c/Users/mengyuetao/Desktop/x.txt    |  sed 's/^[ ]*[0-9]\+[ ]*//'
	cat  /cygdrive/c/Users/mengyuetao/Desktop/x.txt    | sed 's/[ ][ ]*/ /g' | sort -u
	```



# lvm
##### 常用命令
    pvs
    pvdisplay
    vgs
    vgdisplay
    lvs
    lvdisplay

##### lv 扩容步骤

1. 卸载LV				
2. 扩展LV
3. 检查文件系统
4. 重建文件系统


VBoxManage list hdds
VBoxManage modifyhd "xxx.vdi" --resize 40960
fdisk -l
fdisk /dev/sda   ; n  w 添加一个新的主分区
pvcreate /dev/sda3

partprobe  重新加载分区表

vgdisplay
vgextend centos /dev/sda3

lvdisplay
lvextend /dev/centos/root /dev/sda3
xfs_growfs  /dev/centos/root
resize2fs /dev/centos/root



# 其他
#### 乱码处理

 ls -ldi *

 ls -ldi *

`find ./ -inum 1188822445`


#### Ip Route

ip route add default via 192.168.150.4 dev br0



#### split

split -b 66m /Users/mengyuetao/Documents/1/centos6.5-manage1.qcow2.tar.gz leo
rsync -zrvv --bwlimit=1024 /Volumes/Macintosh\ HD/Users/jutongbao/1   root@192.168.2.151:/data2
ls leo* | xargs cat >> ../centos6.5-manage1.qcow2.tar.gz


### find

https://linux.die.net/man/2/stat
https://linux.die.net/man/1/find

```
-type  -size  -prune

```
