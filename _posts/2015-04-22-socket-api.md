---
layout: post
title: "record about Network socket API"
description: socket, network
category: "Network"
tags: [github]
---
{% include JB/setup %}

####1.字节序  
网络协议使用‘大端字节序’（起始地址存的高序字节）来传送这些多字节整数。
{% highlight c++ %}
#include <netinet/in.h>
uint16_t htons(uint16_t host16bitvalue);
uint32_t htonl(uint32_t host32bitvalue);
uint16_t ntohs(uint16_t net16bitvalue);
uint32_t ntohl(uint32_t net32bitvalue);  
{% endhighlight %}

####2.点分十进制的ASCII码字符串和32位的网络字节序二进制值间转换IPv4地址
//下面两个函数淘汰，写出来帮忙过渡记忆
{% highlight c %}
#include <arpa/inet.h>
//字符串符串有效则返回1，否则0
// 不足：出错时addrptr返回 INADDR_NONE(32位1),又有手册称返回-1
int inet_aton(const char *strptr, struct in_addr *addrptr);		
char* inet_ntoa(struct in_addr inaddr);	// 不足：不可重入


#include <arpa/inet.h>
// 优点：ipv4和ipv6都适合 
// p:presentation n:numeric
// 注意：成功返回1，输入格式无效返回0，出错返回-1；EAFNOSUPPORT
int inet_pton(int family, const char *strptr, void *addrptr);
// 成功返回指向结果的指针，出错返回NULL; ENOSPC
const char* inet_ntop(int family, const void *addrptr, char *strptr, size_t len);
{% endhighlight %}

####3. #include <sys/socket.h>
{% highlight c %}
int socket(int family, int type, int protocol);
int connect(int sockfd, const struct sockaddr* servaddr, socklen_t addrlen);

//bind可以指定IP地址或端口，或两者都指定，或两者都不指定。
//bind系统保留端口，需要root权限
int bind(int sockfd, const struct sockaddr* myaddr, socklen_t addrlen);

//返回套接字关联的本地协议地址
int getsockname(int sockfd, struct sockaddr* localaddr, socklen_t *addrlen);
//返回套接字关联的远程协议地址 如accept返回的客户sockfd的对应地址
int getpeername(itn sockfd, struct sockaddr* peeraddr, socklen_t addrlen);
int listen(int sockfd, int backlog);//还是不明白backlog指定什么值比较好?

//监听套接字维护两个队列：未完成连接队列(SYN_RCVD)和已完成连接队列(ESTABLISHED)
int accept(int listenfd, struct sockaddr* cliaddr, socklen_t *addrlen);
{% endhighlight %}

####4. #include <unistd.h>
{% highlight c %}
int close(int sockfd);
/*
fork调用一次，返回两次：调用进程返回子进程ID号，子进程返回0
fork的两个用法：一是进程创建自身的一个副本；二是一个进程想要执行另一个程序，副本调用 exec把自身替换成新的程序。
*/
pid_t fork(void);
pid_t getppid(void);//获得父进程ID
{% endhighlight %}



