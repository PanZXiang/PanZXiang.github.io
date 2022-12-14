# 2.Git记录代码并上传仓库


# 1.本地安装并远程连接gitee仓库

- sudo apt-get install git
- git init  //选择文件夹初始化为本地仓库
- git config --global user.email "xx@xx.com"
- git config --global user.name "xxx" //配置用户邮箱和名字
- ssh-keygen -t rsa -C "xx@xx.com"  //获取ssh公钥
- it remote add origin "git仓库网址"  //
![ssh](/ssh.jpg)
- ssh -T git@gitee.com  //验证是否连接成功
![connect complete](/connect.jpg)
# 2.从本地仓库提交远程仓库
- git add xx //上传代码文件
- git commit -m '描述' //将上传代码文件打包并记录
- git push origin "仓库名"
![result](/gitResult.png)


