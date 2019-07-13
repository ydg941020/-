# -perror函数：
C 库函数 void perror(const char *str) 把一个描述性错误消息输出到标准错误 stderr。首先输出字符串 str，后跟一个冒号，然后是一个空格

一、文件的分类：
1、普通文件 （regular file）  
2、目录文件  （directory file ） 
 drwxrwxr-x  2 nantian nantian 78 6月  19 10:46 C
3、字符特殊文件 (character special file)  

4、块特殊文件(block special file)
brw-rw----+ 1 root cdrom    11,   0 6月  19 09:36 sr0
5、FIFO
6、套接字
7、符号连接（symbolic link）:指向另一个文件，是另一个文件的引用

ls -l  /bin/znew
-rwxr-xr-x.   1 root root       5343 5月  11 2015 znew   
"-" 标识bin目录下的普通文件
===============================================
二、文件的访问权限及控制

=============================================
三、文件的输入输出
1、文件的创建、打开、关闭
=============================================
open函数：

pathname：打开或创建的含路径的文件名
flags:表示打开文件的方式
O_RDONLY：以只读方式打开文件
O_WRONLY：以只写方式打开文件
O_RDWR：以可读可写的方式打开文件
这三种方式是互斥的，不能同时使用，但是可以分别与一下标志进行或运算
O_CREAT：文件不存在时，自动创建，需要用到mode，说明文件权限
O_EXCL;  检查文件是否存在，不存在则创建，存在则报错
O_TRUNC：文件存在且以读的形式打开，会将源文件中的数据清0，文件属性保持不变
O_APPEND： 所写的文件以追加的方式加在文件的末尾

O_SYNC：以同步的方式打开文件，任何对文件的修改都会阻塞，直到物理磁盘上的数据同步		以后才会返回

O_NOFOLLOW：打开的文件只能为符号连接
O_DIRECTORY： 打开的文件只能为目录文件
O_NONBLOCK或O_NDELAY：以非阻塞的方式打开文件，对于open及随后对文件的操作，都会立即返回

creat函数

参数与open函数的参数用法一样。crteate函数相当于open函数的以下用法：
open(const char * pathname,(O_CREATE|O_WRONLY|O_TRUNC))

close函数  
用来关闭一个已打开的文件

2、文件的读写

read函数  
从打开的文件中读取数据

从fd指向的文件中读取count个字节的数据到buf所指先的缓存中。若count为0，则read不会读取数据，只返回0。返回值表示实际读取到的字节数，如果返回0，表示已达到文件尾或是无可读取的数据。文件读取，指针会随读取的字节移动。如果read()顺利返回实际读到的字节数，最好能将返回值与参数count做比较。如果返回的数据比要求读取的数据少，则有可能读到了文件末尾或者read被信号中断了读取过程，或者其他原因。当有错误发生是返回-1，错误代码存入errno中。

write函数

将buf缓存区中的count个字节数据写入到fd指示的文件中，文件读写指针也会随之移动。

文件读写指针的移动
lseek 系统调用用来移动文件读写指针的位置

fd：表示文件描述
offset：根据参数whence来移动读写位置的位移数
whence有三种取值：
SEEK_SET：从文件开始出计算偏移量，文件指针到文件开始处的距离为offset
SEEK_CUR：从文件指针的当前位置开始计算偏移量，文件指针值等于当前指针值加上offset的值，offset的值允许取负数。
SEEK_END：从文件结尾处开始计算偏移量，文件指针值等于当前指针值加上offset的值，offset值允许取负数。



lseek的常用方法：
将文件的读写指针移到开头     lseek(int fildes,0,SEEK_SET)
将文件读写指针移到文件结尾    lseek（int fildes,0,SEEK_END）
获取文件读写指针当前的位置（相对于文件开头的偏离）  lseek（int fildes,0,SEEK_CUR）

注：有些设备（或者设备文件）不能使用lseek。Linux系统不允许lseek()对tty设备进行操作，此项操作会使得lseek()返回错误代码。
---------------------------------------------------------------------------------------------------
预编译器的内置宏：(两个下划线)
__TIME__: 表示时间
__LINE__:表示行数
__FUNCTION__:表示函数名
__FILE__:表示文件名
-----------------------------------------------------------------------------------------------------
dup和dup2函数：用来复制文件描述符（复制fd）

fcntl函数（通过对打开的fd进行操作来改变文件的属性）

cmd的值：F_DUPFD、  F_GETFD、F_SETFD、F_GETFL、F_SETFL

Linux的文件记录锁：fcntl函数的3种功能（涉及进程和信号）
 	fcntl函数用来管理文件记录锁的操作时，第三个参数指向了一个struct flock *lock的结构。
struct flock{
short_1_type;                 /*锁的类型*/
short_1_whence;          //偏移量的其实位置
off_t_1_start;
off_t_1_len;
pid_t_1_pid;                   //锁的属主进程
} 
lock->1_type
======================================================
ioctl函数：用来控制特殊文件的属性（可用来获取网络设备的信息）



四、获取文件的属性

参数struct stat *buf是一个保存文件状态信息的结构体



struct stat结构体的成员比较多，但有的很少使用。常用的：st_mode、st_uid、st_gid、st_size、st_atime、st_mtime。

五、文件的移动和删除

六、目录操作
1、目录的创建和删除

七、实现自己的ls命令
