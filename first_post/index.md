# 1.Socket通信


# 1.Socket通信

## Socket技术：
##### 服务端
- 创建两个文件描述符，一个用于创建监听套接字，一个用于服务端与客户端之间连接成功后传输数据的套接字；
- 创建服务端地址结构，指定服务端的IP协议簇、IP地址和端口号；
- 将监听套接字与服务端的地址信息进行绑定，调用函数为bind()；
- 将监听套接字设定为监听状态，开始监听，并且设定同时连接数上限，所有连接请求将放在一个连接请求队列里面，调用函数listen()；
- 在监听套接字处的连接请求队列中，取出第一个连接请求来建立连接，如果没有连接请求则会一直阻塞，调用函数accept()，如果连接成功则会返回这个新连接的套接字，这个表征新连接的套接字才表示一个成功的连接，后续的数据传输都是通过这个套接字进行的。通过accept()函数可获得成功接入的客户端的地址结构；
- 上述流程都执行成功后，说明服务端与客户端连接成功，就可以使用read()读取客户端发来的信息，write()向客户端进行信息写入了。（read和write都是使用新连接的套接字）
##### 服务端
- 创建一个文件描述符，用于创建套接字，客户端无需监听，这个套接字直接用于数据传输；
- 创建将要连接的服务端的地址结构，指定服务端的IP协议簇、IP地址和端口号；
- 通过套接字以及服务端的地址结构向服务端发起连接请求，调用函数connect()；
- 上述流程都执行成功后，说明服务端与客户端连接成功，connect时调用的套接字就保存着此次成功连接的信息，然后就可以使用这个套接字以及read()读取服务端发来的信息，write()向服务端进行信息写入了。

---
## 运行结果:  
![result](/result.jpg)
- 在一个终端中运行服务端并等待客户端连接
- 在另一个终端启动客户端并加上连接IP和端口
- 连接完成后可进行客户端与服务端的通信
- 客户端在发送后返回大写的输入文本

---

## Server:
![Server](/Server1.png)
![Server](/Server2.png)
```c++
#include <iostream>
#include <unistd.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <string.h>
#include <errno.h>
 
#define SERV_PORT 8888        //服务器端口
#define SERV_IP "127.1.1.1"   //服务器ip
 
using namespace std;
 
int main(int argc,char** argv)
{
	int servfd,clitfd;   //创建两个文件描述符，servfd为监听套接字，clitfd用于数据传输
	struct sockaddr_in serv_addr,clit_addr; //创建地址结构体，分别用来存放服务端和客户端的地址信息
 
	memset(&serv_addr,0,sizeof(serv_addr));  //初始化
	memset(&clit_addr,0,sizeof(clit_addr));  //初始化
 
	if((servfd = socket(AF_INET,SOCK_STREAM,0)) == -1)  //创建套接字
	{
		cout<<"creat socket failed : "<<strerror(errno)<<endl;//如果出错则打印错误
		return 0;
	}
        
        //给服务端的地址结构体赋值
	serv_addr.sin_family = AF_INET;
	serv_addr.sin_port = htons(SERV_PORT); //将主机上的小端字节序转换为网络传输的大端字节序（如果主机本身就是大端字节序就不用转换了）
	serv_addr.sin_addr.s_addr = inet_addr(SERV_IP); //将字符串形式的ip地址转换为点分十进制格式的ip地址
 
        //绑定地址信息到监听套接字上，第二个参数强转是因为形参类型为sockaddr ，而实参类型是sockaddr_in 型的
	if(bind(servfd,(sockaddr *)& serv_addr,sizeof(serv_addr)) == -1)
	{
		cout<<"bind failed : "<<strerror(errno)<<endl;
		return 0;
	}
        
        //将servfd套接字置为监听状态
	if(listen(servfd,1024) == -1)
	{
		cout<<"listen failed : "<<strerror(errno)<<endl;
		return 0;
	}
 
	cout<<"Init Success ! "<<endl;
	cout<<"ip : "<<inet_ntoa(serv_addr.sin_addr)<<"  port : "<<ntohs(serv_addr.sin_port)<<endl;
	cout<<"Waiting for connecting ... "<<endl;
 
	socklen_t clit_size = 0; //用于accept函数中保存客户端的地址结构体大小
 
        //accept成功后，clitfd则指向了这条服务端与客户端成功连接的”通路“
	if((clitfd = accept(servfd,(sockaddr *)& clit_addr,&clit_size)) == -1)
	{
		cout<<"accept failed : "<<strerror(errno)<<endl;
		return 0;
	}
 
	cout<<"Client access : "<<inet_ntoa(clit_addr.sin_addr)<<"  "<<ntohs(clit_addr.sin_port)<<endl;
 
 
	char buf[1024]; //用于读写数据
 
	while(1)
	{
		int rdstate;
		if((rdstate = read(clitfd,buf,sizeof(buf))) > 0 )//通过clitfd来读取数据，返回值为读取的长度
		{
			int i=0;
			cout<<"(Server)recv : ";
			for(i=0;i<rdstate;i++)
			{
				cout<<buf[i];
				buf[i] = toupper(buf[i]); //转换为大写
			}
			buf[i]='\0';
			cout<<endl;
			write(clitfd,buf,strlen(buf)); //发回客户端
		}
                else if(rdstate == 0)  //客户端退出
		{
			cout<<"client exit ! "<<endl;
			return 0;
		}
	}
 
	close(servfd);  //关闭套接字
	close(clitfd);  
	return 0;
}
```
---   

## Client:
![Client](/Client1.png)
![Client](/Client2.png)
```c++
#include <iostream>
#include <unistd.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <string.h>
#include <errno.h>
#include <cstdio>
 
using namespace std;
 
int main(int argc,char** argv)
{
	int clitfd;  //文件描述符
	struct sockaddr_in serv_addr;  //目的服务端地址结构体
 
	memset(&serv_addr,0,sizeof(serv_addr));
 
	if(argc!=3)
	{
		cout<<"Input error! Usage should be : "<<argv[0]<<"  xxx.xxx.xxx.xxx(ip)  1234(port)"<<endl;
		return 0;
	}
 
	if((clitfd = socket(AF_INET,SOCK_STREAM,0)) == -1)  //创建套接字
	{
		cout<<"creat socket failed : "<<strerror(errno)<<endl;
		return 0;
	}
        //将目的服务端的地址信息赋值给地址结构体
	serv_addr.sin_family = AF_INET;
	serv_addr.sin_port = htons(atoi(argv[2]));
	serv_addr.sin_addr.s_addr = inet_addr(argv[1]);
 
	cout<<"try to connect ... "<<endl;
 
        //通过套接字发起连接请求，成功后clitfd套接字则表示此次成功的连接
	if( connect(clitfd,(struct sockaddr*)& serv_addr,sizeof(serv_addr)) == -1)
	{
		cout<<"connet failed : "<<strerror(errno)<<endl;
		return 0;
	}
 
	cout<<"connect success !"<<endl;
 
	while(1)
	{
		char sdbuf[1024];
		char rvbuf[1024];
		int rdlen,sdlen,i=0;
		cout<<"(Client)send : ";
 
		while((sdbuf[i] = getchar()) != '\n')i++;
                if(i==0)continue; //防止客户端只发一个换行符，此时i=0，write不发送数据，服务端就不回回发数据，然后客户端就一直阻塞在read处。因此如果i=0，则直接重新输入
 
		sdlen = write(clitfd,sdbuf,i);  //向套接字中写入数据发送
        //可能会出现发送端把长度为sdlen的字符串分为多次发送，调用一次read就很有可能不能读取完全，就有以下两种方式进行读取：
        /*1. write了多少字节就读取多少字节长的字符串*/
		rdlen=0;
		while(rdlen<sdlen)//防止发送端将数据分开发送
		{	
			int rdcnt = read(clitfd,&rvbuf[rdlen],sizeof(rvbuf));
			if(rdcnt == -1)
			{
				perror(NULL);
				continue;
			}
			rdlen+=rdcnt;
		}
                if(rdlen)
                {
		    rvbuf[rdlen]='\0';
		    cout<<"(Client)recv : "<<rvbuf<<endl;
                }   
                else 
		{
	            cout<<"Server has closed ! "<<endl;
		    cout<<"Client will close..."<<endl;
		break;
		} 
 
 
        /*2. 用recv函数中的MSG_WAITALL参数，读到指定长度的数据才返回*/
        /*if(rdlen = recv(clitfd,&rvbuf[rdlen],sdlen,MSG_WAITALL))
		{	
			rvbuf[rdlen]='\0';
	
			cout<<"(Client)recv : "<<rvbuf<<endl;
		}
		else 
		{
			cout<<"Server has closed ! "<<endl;
			cout<<"Client will close..."<<endl;
			break;
		}*/
 
	}
 
	close(clitfd);
}
```
---  

