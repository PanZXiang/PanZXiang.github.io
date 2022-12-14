# 4.大作业完成步骤


# Git、Socket、Hugo学习及应用

本次大作业中首先在Ubuntu系统中完成Socket通信,在使用Hugo时遇到权限不足的bug后改为使用Kali系统完成,并且在两个系统中都配置了Git远程连接仓库

## 1.Git应用
### 运行`git init`初始化仓库并且会出现 .git文件
![](/git1.png)
### 编写好代码后使用`git add`与`git commit`命令提交文件
### 使用`git log`查看提交记录  
![](/git2.png)
### 使用`git push origin master`提交到远程仓库
### 学习网站
- https://gitee.com/help/categories/36
- https://blog.csdn.net/qq_43794633/article/details/120625061
---
## 2.Socket通信
### 最先使用python语言编写，由于Ubuntu中python版本过低，改为c++编写
### 通过学习了解到Socket客户端中主要使用函数bind()对IP与端口绑定并使用函数listen()开始监听请求，存在请求使用accpet()函数进行连接，成功则返回客户端的Socket，通过函数read()读取客户端发来的信息，函数write()向客户端输送信息。
### 学习网站
- https://www.bilibili.com/video/BV1a7411z75u?p=1&vd_source=f119c843c853cb3b12ba5f4844e23e89
- https://www.bilibili.com/video/BV1DV4y1j7SC/?spm_id_from=333.337.search-card.all.click&vd_source=f119c843c853cb3b12ba5f4844e23e89
---
## 3.Hugo个人博客搭建
### 使用`hugo new site 名字`搭建本地站点
![](/hugo1.png)
### 前往 https://themes.gohugo.io/选择主题
### 对站点内config文件配置对应主题和其他参数
![](/hugo2.png)
### 使用`hugo new posts/first_post.md`获取一个文章
### 使用`hugo server`启动本地网站
![](/hugo3.png)
### 使用markdown语法对文章进行编辑
![](/hugo4.png)
### 学习网站
- https://hugoloveit.com/zh-cn/theme-documentation-basics/
- https://www.bilibili.com/video/BV1JA411h7Gw/?spm_id_from=333.337.search-card.all.click&vd_source=f119c843c853cb3b12ba5f4844e23e89
## 4.将Hugo网页通过Apache部署到服务器
### 在站点中使用`hugo`在public生成配置文件
### 将public中的文件复制到/var/www/html中
### 使用`/etc/init.d/apache2 start`开启Apache服务
### 网页中输入IP地址即可访问
![](/resultHugo.png)
### 学习网站
- https://blog.csdn.net/Fighting_hawk/article/details/122640537

