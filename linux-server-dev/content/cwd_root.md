# 改变工作目录和根目录

使用getcwd获取进程当前工作目录，使用chdir改变进程工作目录
```
#include <unistd.h>
char* getcwd(char* buf, size_t size);
int chdir(const char* path);
```

buf指向的内存用于存储进程当前工作目录的绝对路径名，大小由size参数指定

    如果strlen(buf)+1大于size，则getcwd返回NULL，并设置errno
    
    如果buf为NULL并且size非0，则getcwd可能在内部使用malloc动态分配内存，如果是这种情况，必须手动释放buf指向的内存空间

getcwd成功返回指向目标存储区的指针，失败返回NULL并设置errno

chdir函数的path指定要切换的目标目录，成功返回0，失败返回-1并设置errno

使用chroot改变进程的根目录：
```
#include <unistd.h>
int chroot(const char* path);
```
path指定目标目录，成功返回0，失败返回-1并设置errno

chroot并不改变进程的当前工作目录，所以调用chroot之后，仍然要使用chdir("/")将工作目录切换至新的根目录

改变程序的根目录之后，程序可能无法访问类似/dev的文件或目录，因为这些文件或目录并非处于新的根目录之下，不过好在调用chroot之后，进程原先打开的文件描述符仍然有效，所以我们可以利用这些早先打开的文件描述符来访问调用chroot之后不能直接访问的文件，尤其是一些日志文件
此外，只有特权进程才能改变根目录