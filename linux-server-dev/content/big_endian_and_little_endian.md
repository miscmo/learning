# 主机字节序和网络字节序

主机字节序 <==> 小端字节序

网络字节序 <==> 大端字节序

以十六进制数0x1234为例：

小端字节序：00000001 00000010 00000011 00000100（高地址存高字节）

大端字节序：00000100 00000011 00000010 00000001（高地址存低字节）

主机字节序和网络字节序转换API：

```
#include <netinet/in.h>

unsigned long htonl( unsigned long hostlong );
unsigned short htons( unsigned short hostshort );
unsigned long ntohl( unsigned long netlong );
unsigned short ntohs( unsigned short netshor
```