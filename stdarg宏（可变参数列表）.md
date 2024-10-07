# <stdarg.h>

## ...

表示此处可能传递数量和类型未定的参数

在其出现之前一定要已经出现参数

## 声明

### 类型

#### va_list

## 宏

#### va_start

```c
va_start(参数1,参数2)
```

参数1为va_list类型变量名

参数2为省略号前最后一个有名字的参数

#### va_arg

```
va_arg(参数1,参数2)
```

参数1为访问的va_list类型变量名

参数2为返回的数据类型

返回参数的值，并且va_arg指向下一个可变参数

#### va_end

释放

## 使用方法

1.声明va_list类型变量

2.va_start初始化变量

3.va_arg访问变量

4.释放变量

Ep：

```c
float averge(int v,...)
{
  va_list s;/*声明va_list变量s*/
  va_start(s,v);/*初始化s*/
  int x = var_arg(s,int);/*调用参数*/
  va_end(s);

}
```

## 限制

必须按顺序

传入的均本为缺省数据类型

无法判断实际传入的参数数量（可能会使va_arg访问到不应访问的区域）