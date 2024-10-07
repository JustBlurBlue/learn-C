## strlen

长度 

返回unsigned int

## strcat

返回第一个参数的副本

## strncat

```c
char *strcat(char *dst,char const *src,size_t len);
```

会加NUL

但是不管先前字符串的空间够不够

直接加NUL

返回第一个参数的副本

## strcpy



返回第一个参数的副本

## strcmp

匹配字符串

直到发现不匹配的

认定小的是ascii表中小的

NULL字符ascii是0

```c
int strcmp(char const *s1,char const *s2)
```

如果s1小于s2，返回小于零

s1大于s2，返回大于零

字符串相等  返回零

## strncmp

```c
int strncmp(char const *s1,char const *s2,size_t len)
```

有限len长度

## strncpy

```c
char *strcpy(char *dst,char const *src,size_t len);
```

一样复制

但是只复制len个字符

如果src小于len，dst数组就用额外的NIUL字节填充到len长度

如果src大于len，那么就只有len被复制*注意：此时不包含NUL*



## strchr

```c
char *strchr(char const *str,int ch)
```

str中找ch

返回指向找的字符的指针（最左边的）

如果不存在返回NULL指针

## strrchr

```c
char *strrchr(char const *str,int ch)
```

同strchr

但是返回最右边的

## strpbrk

```c
char *strpbrk(char const *str,char const *group)
```

查一组字符（这组字符中的任意字符    只要出现一次   就会被查找）

返回位置  指针

如果未找到  返回NULL指针

## strstr

查找子串

```c
char *strstr(char const *s1,char const*s2)
```

查s2

没出现：返回NULL指针

s2为空字符串  返回s1



# 查找前缀

## strspn

查找字符串开头起属于字符组的字符数，一旦右不匹配的则结束

```c
size_t strspn(char const *str,char const *group)
```

group是一个字符集

str开头起直至第一个不匹配     有几个字符属于group  就返回unsigned int值

## strcspn

```c
size_t strcspn(char const *str,char const *group)
```

与strspn相反

str开头起直至第一个匹配     有几个字符不属于group  就返回unsigned int值

# 查找标记

## strtok

分解字符串str

sep为用作分隔符的字符   是一个字符组（只要出现都可以分隔）

```c
char *strtok(char *str,char *sep)
```

返回被分解字符的第一个子字符串

若无   返回NULL指针

 而在后续循环赋值过程中，strtok函数的第⼀个参数为 NULL 

函数将在同⼀个字符串中被保存的位置开始，查找下⼀个标记（如果字符串中不存在更多的标记，则返回 NULL 指针。） 

原理是  把  所有指定分隔符替换为'\0'   并返回指向先前这个字符串的指针

Ep：

```c
// 定义一个字符串数组，并初始化
    char arr[] = "To  The -Ones I Used To   ， Love";
    // 定义分隔符字符串
    char *sep = " ，";
    // 定义一个指向字符串的指针，用于存储每次分割得到的子字符串
    char *str = NULL;

    // 使用 strtok 函数进行字符串分割，每次调用都会返回被分割出的部分
    for (str = strtok(arr, sep); str != NULL; str = strtok(NULL, sep))
    {
        // 输出每次分割得到的子字符串
        printf("%s\n", str);
    }

/*程序输出：
To
The
-Ones
I
Used
To
Love
    
```

*警告：*
*由于strtok函数保存了它所处理的函数的局部状态信息，因此它不能同时解析两个字符串*

*如果在上面程序  for  循环体内再调用新的  strtok  函数  那么会出错*

# 错误信息

## strerror

```c
char *strerror(int error_number)
```

当调用某些请求操作系统的函数（如打开文件）时，如果出现错误

操作系统通过设置一个外部整型变量errno进行错误代码报告

*\#include <errno.h>以获取系统传递的错误代码*

strerror把错误代码作为参数

返回一个用于指向用于描述错误的字符串的指针

*返回值应被声明为const，别修改系统参数啊*

Ep:

```c
#include <stdio.h>
#include <string.h>
#include <errno.h>

int main ()
{
   FILE *fp;

   fp = fopen("file.txt","r");
   if( fp == NULL ) 
   {
      printf("Error: %s\n", strerror(errno));
   }
   
  return(0);
}
```

# 字符操作（ctype.h）

## 字符分类

以下函数只接受一个包含字符型的整型参数

返回int（真或假）

| 函数     | 如果它的参数符合下列要求就返回真                             |
| -------- | ------------------------------------------------------------ |
| iscntrl  | 任何控制字符                                                 |
| isspace  | 空  ' '  ，换页' \f ' ，换行  '\n' ，回车'\r'  ，制表符 '\t'，垂直制表符 '\v' |
| isdigit  | 十进制0-9                                                    |
| isxdigit | 十六进制                                                     |
| islower  | 小写字母                                                     |
| isupper  | 大写字母                                                     |
|          |                                                              |
|          |                                                              |
|          |                                                              |

懒得记了，这个不如敲代码自己写

唯一有用的地方是

```c
if(isupper(ch))
```

无论机器使用什么字符集，它都能顺利运行（提升移植性）

## 字符转换

把大写字母转换成小写字母

或相反

### int tolower(int ch)

返回对应小写

### int toupper(int ch)

返回对应大写

# 内存操作（string.h）

此组函数能够处理任意字节序列

不受NUL限制

每个都包含一个显式的擦书来说明需要处理的字节数

逻辑与字符串类似

但是能够处理任意字节



## 1.memcpy

```c
void *memcpy(void *dst,void const*src,size_t lenth)
```

从src起始位置复制lenth个字节到dst位置

## 2.memmove

```c
void *memmove(void *dst,void const*src,size_t lenth)
```

源操作数和目标操作数可以重叠

它把源操作数复制到临时位置再复制到目标位置

可以在同一个数组内操作

返回

## 3.memcmp

```c
void *memcmp(void const *a,void const*b,size_t lenth)
```

对比两段内存

负：a<b

正：b<a

这些值是根据一个字节比较的，不可用于int，float等等

或是有正负的



## 4.memchr

```c
void *memchr(void const*a,int ch,size_t lenth)
```

查找ch第一次出现的位置

返回指向该位置的指针

共查找lenth字节

若未找到 返回NULL

## 5.memset

```c
void *memset(void *a,int ch,size_t lenth)
```

把从a开始的lenth个字节都设置为字符值ch

Ep：

```c
memset(buffer,0,SIZE)
```

将buffer的前SIZE个字节都初始化为零

