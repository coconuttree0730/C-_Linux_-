# 基本概念
### 学习顺序：
        1.基本概念
        2.数据类型，运算符和表达式
        3.输入输出
        4.流程控制
        5.数组 *
        6.指针 *
        7.函数 *
        8.构造类型（自定义类型）
        9.动态内存管理 * （自定制内存空间大小）
        10.调试工具（gdb（调试错误）、make（文件管理））和调试技巧！！
        11.常用库函数的使用（避免重复造轮子）
### 学习（开发）环境（需要安装的程序）：
linux发行版、vim、gcc（make）
> linux开发环境与window开发环境，编译生成的  结果可能是会不一样的 



熟悉对man手册的使用：
例：了解malloc函数：
终端输入： man <查询函数名>
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35877764/1691129434245-ea398ab9-7971-4abf-9b4f-056256457c3d.png#averageHue=%232b2f3e&clientId=uc5c1a63d-6b6d-4&from=paste&height=16&id=u984264b2&originHeight=24&originWidth=663&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=6990&status=done&style=none&taskId=u8c880c07-8e4f-40b4-a6b4-0ca812966f0&title=&width=442)
查询结果：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35877764/1691129517478-24c1659a-735f-4654-9030-a629b4188361.png#averageHue=%23292c39&clientId=uc5c1a63d-6b6d-4&from=paste&height=320&id=u4f72d7d5&originHeight=480&originWidth=934&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=49391&status=done&style=none&taskId=ufe4d81ba-e6a7-4472-bba6-5f7beb6572e&title=&width=622.6666666666666)
-----查询结果，需要声明一个头文件：#include<stdlib.h>

### 程序编译过程（原理）：
       - c源文件 - 预处理 - 【编译】-汇编 - 【链接】- 可执行文件

        -预处理：
                gcc -E 源文件   ---（将带井号#的头文件预处理掉）
                可写成： gcc —E 源文件.c > 文件名.i
        -汇编文件：
                gcc -S 文件名.i
                -------生成文件名.s 的汇编文件
        -生成目标文件：
                gcc -C 文件名.s
                -------生成文件名.o 的目标文件
        -链接：
                gcc 文件名.o -o 起别名
                --------生成a.out(起别名) 可执行文件

gcc 用到的选项 （补充）：
> -  gcc 源文件.c -Wall   ---给出语法报错提示！
> - GDB调试工具并不是很常用（通过gcc  源文件.c -Wall 这个命令就可以列举很多的错误，然后结合 printf打印输出排查错误....）


### 基本函数结构
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35877764/1691123542366-ae6a1a8b-cf7b-4f34-a39f-19788097d2d2.png#averageHue=%23b7d0f6&clientId=uc5c1a63d-6b6d-4&from=paste&height=139&id=uf0554369&originHeight=209&originWidth=1112&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=207741&status=done&style=none&taskId=u2c364752-2138-4dc0-b4fd-84970e6ffef&title=&width=741.3333333333334)
c语言最开始的main就是int数据类型，而后引入void数据类型；void---函数没有返回值or函数未带参数：

- void main（viod）{ 语句；}

以下学习，都采用 ：
> int main（void）---无参数
> int main（数据类型 形参名，...） ---有参数

的形式....

```c
//头文件（带#号）的是编译最开始，是执行的第一步
#include<stdio.h>   //头文件包含 printf();
#include<stdlib.h>  //头文件包含 exit()函数
/*
*
*
*
*/
int main(void)  //linux环境下建议是在main（）方法里加入vode参数
{
    #if 0  //效果跟 “/*” 一样
    printf("hello world\n");  

	//如果主函数 没写上return 0；那么，返回值就是 上一行的printf（）的字符个数
    
    exit(0);  //退出的意思
    //return 0;   //-- 返回值给的是 运行它的环境（如linux环境下，这个main的返回值是给shell的）
    
    #endif  //效果跟 “*/” 一样
}


```


### 编程注意问题：
#### 一、正确包含对应函数的头文件的重要性
1.要善用  -Wall选项（提示程序语法错误）
> gcc 文件名  -Wall

例子：

2. malloc函数的解释：
:::info
malloc函数是包含在 stdlib.h 头文件内的，故需要声明 #include<stdlib.h>
:::
```c
#include<stdio.h>
#include<stdlib.h>
int main(void)
{
    int *p =malloc(sizeof(int )); //开辟一个内存空间，这个开了一个int大小的空间，然后将开辟的内存地址给p这个指针变量（malloc函数默认的就是void数据类型）
    if(p==NULL){exit(1);
    //return -1;
    }


    return 0;//exit(0);
}

```

- malloc（）函数默认函数返回值是 void数据类型
> void类型的数据：可以给任何数据类型 and  任何数据类型都可以给void类型；
> 故：**没必要**加上强制类型转换：int *p =（int *）malloc(sizeof(int ));


例子2：
```c
#include<stdio.h>
#include<stdlib.h>

#include<string.h>  // strerror(errno)
#include<errno.h>    //errno
                 
int main(void)
{

    FILE * fp;  //定义一个fp文件类型的指针变量，用于指向文件地址

    fp= fopen("111.c","r"); //找到则fp不等于NULL，没找到文件，则返回空值NULL
    if (fp==NULL)
    {
        fprintf(stderr,"fopen():%s\n",strerror(errno));
        exit(1);
    }
    puts("ok");


    return 0;
    
}
```

---

#### 二、以函数为单位进行函数的编写（模块化）
> 养成习惯，以函数为单位进行编程，对问题分解成一个个小问题，由小问题组合解决大问题，函数就是小问题的解


#### 三、声明

- 函数声明、全局变量、结构体、联合体、枚举..统一格式发在这里：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/35877764/1691131407678-edbe8c1d-96f1-4511-b22a-7ee94b0d75a5.png#averageHue=%238eb4e7&clientId=uc5c1a63d-6b6d-4&from=paste&height=146&id=ufb2241be&originHeight=219&originWidth=450&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=116309&status=done&style=none&taskId=ua91f5d0b-95d5-4123-a796-11e04fd65af&title=&width=300)

- 普通变量（局部变量）建议也是先定义再使用；（不推荐即时定义即时使用）

#### return 0；
> return 0；就是正常返回结束函数，出栈返回至它的父程序
> exit（0）；就是结束进程的意思
> ------单线程的程序中，这两者没有区别

相关面试题：
```c
#include<stdio.h>
int main(){
printf("hello linux-c\n");
//exit(0);
//return 0;
}
```
问：上述程序没有写return 0；则返回给父进程的是什么值？
答：c99之后会自动补齐return 0;    //还有一种解释是返回上一个printf的格式化参数的字符个数！！！；例上述代码hello linux-c   就是返回值：13


#### 四、注释的使用：

1. // 注释

2. /*  注释  */

3. /*  说明1  ----这种主要就是介绍函数怎么用....给出使用说明

        *  说明2
        *  说明3 ...
        */

  **4. 适合大段代码注释掉：**
:::info
    #if 0
...语句1；
...语句2；
...
    #endif
:::

##### 五、内存泄漏问题的考虑
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35877764/1691133302202-9168f3ad-dbb7-4626-b1af-2e04df6d0e1b.png#averageHue=%23dccfe4&clientId=uc5c1a63d-6b6d-4&from=paste&height=30&id=u5c230fdf&originHeight=45&originWidth=1090&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=78728&status=done&style=none&taskId=u882bece6-cb37-4a30-8b4e-e3c6d483377&title=&width=726.6666666666666)

---


---

# 数据类型

- 标准C的 char没有符号位这个说法：或者说char有无符号意义不大；
- 旧时期，考虑硬件问题，所以会采用char（占用空间小，故使用unsigend char 取代int）
- bool（布尔）类型：真and假：
- ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35877764/1691135192565-45b9cc95-4ff9-4c42-ad7f-03c5b1dff6c3.png#averageHue=%23292b37&clientId=uc5c1a63d-6b6d-4&from=paste&height=426&id=ua507a8a6&originHeight=639&originWidth=678&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=79184&status=done&style=none&taskId=u24b12477-32c0-4e52-bfc4-317671eea12&title=&width=452)
