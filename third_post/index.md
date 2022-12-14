# 3.Linux笔记


### pwd 显示当前工作目录绝对路径  
### cd  改变工作目录路径
- . 	代表当前所在目录
- ..	代表当前目录的上一层目录
- ~ 	代表家目录
- /	代表根目录
### mkdir 创建目录  
- -p 若路径中目录不存在则先创建目录
### ls 列出目录和文件信息
- -l 列出详细信息
- -a 显示所有文件（包括隐藏文件）
- -d 查看目录属性
- -h 以易于阅读格式输出文件大小，与-l一起
### rmdir 删除空目录
### touch 创建空文件、更改文件时间
### cp 复制文件或目录 
`cp old.txt ../new/`
### touch 创建文件
### rm 删除文件或文件夹
### find 查找文件
- -name 匹配文件名称
- -type 匹配文件类型(f普通文件，d目录)
- -size 匹配文件大小(+50k,+3G)  
`find /user/bin -name "host" -type f -size +1M`  
### cat * 显示文件内容、链接文件内容
### head、tail 显示文件头、尾数据
`tail/head -n 3 /etc/passwd`
### grep 全局查找以及打印
- -i 查找时忽略大小写
- -n 显示行号
- -r 递归搜索子目录
- -v 反向查找，查找与条件不符的行  
`grep user -irn /etc/`
### 重定向
- << 标准输入,代码为0
- > or >> 标准输出,代码为1,>>为追加
- 2> or 2>> 标准错误输出,代码为2,2>>为追加
### 修改权限
`chmod /config.toml 777`权限分别为用户、组、其他
### Vim编辑器
- i 插入
- Esc 退出插入
- :w/:q/:wq/q! 保存/退出/保存并退出/强制退出
- 数字0 跳转至行首
- $ 跳转到行尾
- gg 跳转到文件首行
- G 跳转到文件的末尾行
- #gg 跳转到文件第#行
- x 删除光标处字符
- dd 删除所在行
- #dd 删除从光标开始#行
- r 替换单个字符
- R 替换多个字符
- yy 复制光标所在行
- #yy 复制光标开始#行
- p 粘贴
- u 撤销操作
### 磁盘管理
`lsblk`查看当前所有磁盘
#### parted磁盘分区
- `sudo paered 磁盘路径`  进入parted进行分区 
- `mklabel gpt` 创建新的分区表   
- `mkpart part1 0% 10G`  创建从0到10G的分区
- `mkpart part2 10G 100%` 创建10G到全部空间的分区
- `rm` 删除分区
#### 创建并手动挂载文件系统
- `sudo mkfs -t ext4 磁盘路径`
- `sudo mount 磁盘路径 挂载文件夹路径`
#### LVM(逻辑卷管理)
##### 管理物理卷(PV,Physical Volumes)
- `pvcreate 磁盘路径`  创建物理卷
- `pvs`或`pvidsplay` 查看物理卷
- `pvremove 磁盘路径` 删除物理卷
#### 管理卷组(VG,Volume Group)
- `vgcreate 卷组名 磁盘路径` 创建卷组
- `vgextend 卷组名 磁盘路径` 扩展卷组
- `vgreduce 卷组名 磁盘路径` 收缩卷组
- `vgremove 卷组名 磁盘路径` 删除卷组
#### 管理逻辑卷(LV,Logical Volumns)
- `-L` 制定逻辑卷大小
- `-n` 指定逻辑卷名称
- `lvcreate -L 10G -n 逻辑卷名 卷组名` 创建逻辑卷
- `lvextend/lvreduce -L +/-10G 逻辑卷路径` 扩展/收缩逻辑卷
- `lvremove 逻辑卷路径` 删除逻辑卷


 

