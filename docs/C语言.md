



十六进制中，用 A 来表示 10，B 表示 11，C 表示 12，D 表示 13，E 表示 14，F 表示 15，因此有 0~F 共 16 个数字，基数为16。

逢十六进一，最大数为 F。

### 二进制整数转十六进制

将二进制整数 10110101011100 转换为十六进制，转换过程如下：

![13_二进制整数转十六进制.png](https://haicoder.net/uploads/pic/server/c/program-knowledge/13_%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%95%B4%E6%95%B0%E8%BD%AC%E5%8D%81%E5%85%AD%E8%BF%9B%E5%88%B6.png)

关键字

## 数据类型关键字

| 关键字     | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| *char*     | 声明字符型变量或函数。                                       |
| *double*   | 声明双精度变量或函数。                                       |
| *enum*     | 声明枚举类型。                                               |
| *float*    | 声明浮点型变量或函数。                                       |
| *int*      | 声明整型变量或函数。                                         |
| *long*     | 声明长整型变量或函数。                                       |
| *short*    | 声明短整型变量或函数。                                       |
| *signed*   | 声明有符号类型变量或函数。                                   |
| *struct*   | 声明结构体变量或函数。                                       |
| *union*    | 声明共用体（联合）数据类型。                                 |
| *unsigned* | 声明无符号类型变量或函数。                                   |
| *void*     | 声明函数无返回值或无参数，声明无类型指针（基本上就这三个作用）。 |

##### 控制语句关键字

##### 循环语句

| 关键字     | 描述                           |
| ---------- | ------------------------------ |
| *for*      | 一种循环语句。                 |
| *do*       | 循环语句的循环体。             |
| *while*    | 循环语句的循环条件。           |
| *break*    | 跳出当前循环。                 |
| *continue* | 结束当前循环，开始下一轮循环。 |

##### 条件语句

| 关键字 | 描述                             |
| ------ | -------------------------------- |
| *if*   | 条件语句。                       |
| *else* | 条件语句否定分支（与 if 连用）。 |
| *goto* | 无条件跳转语句。                 |

##### 开关语句

| 关键字    | 描述                       |
| --------- | -------------------------- |
| *switch*  | 用于开关语句。             |
| *case*    | 开关语句分支。             |
| *default* | 开关语句中的 “其他” 分支。 |

##### 返回语句

| 关键字   | 描述                                       |
| -------- | ------------------------------------------ |
| *return* | 子程序返回语句（可以带参数，也看不带参数） |

##### 存储类型关键字

| 关键字     | 描述                                                 |
| ---------- | ---------------------------------------------------- |
| *auto*     | 声明自动变量，一般不使用。                           |
| *extern*   | 声明变量是在其他文件正声明（也可以看做是引用变量）。 |
| *register* | 声明寄存器变量。                                     |
| *static*   | 声明静态变量。                                       |

##### 其他关键字

| 关键字     | 描述                                       |
| ---------- | ------------------------------------------ |
| *const*    | 声明只读变量。                             |
| *sizeof*   | 计算数据类型长度。                         |
| *typedef*  | 用以给数据类型取别名（当然还有其他作用）。 |
| *volatile* | 说明变量在程序执行中可被隐含地改变。       |

#### C语言关键字教程总结

C 语言的关键字共有 32 个，根据关键字的作用，可分其为数据类型关键字、控制语句关键字、存储类型关键字和其它关键字四类。

## C数据类型

![](C:\Users\dongqi.wei\Desktop\workspace\28_C语言数据类型.png)

### C语言printf的占位符

- **%d** 十进制有符号整数
- **%u** 十进制无符号整数
- **%f** 浮点数
- **%s** 字符串 string
- **%c** 单个字符 char
- **%p** 指针的值  void
- **%e** 指数形式的浮点数 float /double
- **%x, %X** 无符号以十六进制表示的整数   
- **%o** 无符号以八进制表示的整数
- **%g** 把输出的值按照 %e 或者 %f 类型中输出长度较小的方式输出
- **%p** 输出地址符
- **%lu** 32位无符号整数
- **%llu** 64位无符号整数

## 数据类型

- char ： 1个字节
- int ：4个字节
- float：4个字节
- double：8个字节

### system("pause")

> system("pause")意思就是让程序暂停一下，然后按任意键继续，初学的时候最多见于程序的末尾处，用于看运行结果，避免程序一闪而过。相同的我们还可以用getchar()

### short 短整型

```c
//范围-32768-32767
//2 byte
short (int可加可不加)  b = 100, c = 19, a = 1023;
printf("a=%hd,b=%hd=,c=%hd\n", a, b, c);
//%hd格式化输出
//a=1023,b=100=,c=19
```

> 查看字节

```c
printf("a-byte=%d",sizeof(a) );
// a-byte=2
```

### int 整型

short<int<long

```c
//格式化符	%d
//byte 4
int a=1;
printf("a = %d\n", a);

```

#include<limits.h>该库可以用INT_MIN和INT_MIN最大和最小值

```c
int intMin = INT_MIN;
	int intMax = INT_MAX;
	printf("%d\n", intMin);
	
	printf("%d\n", intMax);
//-2147483648
//2147483647
```

### long整型

```c
//byte	4
//格式化符	%ld
	long a = 100;
	long b = -12929303;
	printf("%ld\n", a);
	
	printf("%ld\n", b);
```

### long long 整型

```c
//btye 8
//格式化字符%lld
//取值-9223372036854775807ll - 1 ~ 9223372036854775807ll
```

简写

```c
long long a = 100;
printf("%lld\n", a);
//100
```

### float 浮点数

| 说明     | 描述         |
| -------- | ------------ |
| 精度     | 6 ~ 7 位小数 |
| byte     | 4            |
| 格式化符 | %f           |

### double  双精度

| 精度     | 15 ~ 16 位小数 |
| -------- | -------------- |
| byte     | 8              |
| 格式化符 | %lf            |

### char 字符串

| 取值范围 | -128 ~ 127 |
| -------- | ---------- |
| byte     | 1          |
| 格式化符 | %c         |

```c
char a = 'a';
printf("a = %c\n", a);
//a=a
```

char如果定义一个字符类型，那么该字符类型需要使用 `''`单引号

### 类型转化



```c
	short a = 100;
//转化int类型
	int b = (int)a;
	printf("a=%d\n",b);
//b=100
```

### const常量

> const +type+xx名=值

> type+const +xx名=值

```c
const int S = 1024;
	S = 1100;//不可修改
```

### #define -宏

简单来说就是替换简写

**#define <宏名/标识符一般大写> <字符串>**

```c
#define U 23		
		int h = U * U;
		printf("U=%d\n",h );
 //结果U=529

```

> 使用 define 定义的宏可修改

```c
#define S 1100
#define S 1400
```



### signed和unsigned

有符号（signed）也就是该整型可以表示正数也可以表示负数，而无符号（unsigned）则表示该整数只能表示正数，不能表示负数。

语法

> signed/unsigned +type+xx=val

### enum 枚举

第一个枚举成员的默认值为整型的 0，后续枚举成员的值在前一个成员上加 1

enum DAY（可有可无） {     MON=1, TUE, WED, THU, FRI, SAT, SUN };

```c
enum 
	{
		MON = 1, TUE, WED, THU, FRI, SAT, SUN
	};
	printf("MON = %d, TUE = %d, WED = %d, THU = %d\n", MON, TUE, WED, THU);
	system("pause");
	printf("FRI = %d, SAT = %d, SUN = %d\n", FRI, SAT, SUN);
//输出结果
//MON = 1, TUE = 2, WED = 3, THU = 4
//Press any key to continue . . .
//FRI = 5, SAT = 6, SUN = 7
```

### viod

void 用来表示无类型

表示 “没有任何值可以获得”。因此，不可以采用这个类型声明 **[变量]** 或 **[常量]**。C 语言中的 void 可以用于修饰 **[函数]或者 **[指针])**

> 表示函数表示函数没有任何返回值。

```c
void func_name(paramlist（参数）)

{
  [return;](无)
}
```

```cc
void p_age(int age)
{
	printf("年纪：%d\n", age);
}

int main()
{
	p_age(18);

}
//年纪:18
```



>C语言void修饰函数参数

```c
void func_name(void) 
{ 

   [return;]

}

```

使用 void 修饰函数的参数，表示函数不接受任何参数。

```c
void print_info() {
	printf("无需参数做回自己\n");
}
int main() {
	print_info();
	printf("同步进行？");

}
```

> C语言void修饰指针

void 指针表示该指针可以接受保存任何类型的变量的地址。

```c
//void *ptr;

int age = 18;
	void* ptr = &age;
	printf("Age = %d, ptr = %p\n", age, ptr);
//  Age = 18, ptr = 000000DFFCF9FA44
```

### typedef -别名

关键字用来给一个数据类型起一个别名

```c
int main()
{	

	typedef int BOOL;
	#define true 1
	#define false 0
	BOOL isOk = true;
	BOOL isPass = false;
	printf("isOk = %d, isPass = %d\n", isOk, isPass);
}
```

 typedef 使用 int 类型定义了一个新的类型 BOOL，接着，我们使用了 define 定义了两个宏变量，即 true 和 false，并分别赋值为 1 和 0



## 语句

#### for

```c
for (express1; express2; express3)
{
	// statements
}
```

eg.

```c

```





#### while

当 cond 条件为真时，一直执行 `{}` 里面的代码块，直到 cond 条件为假，循环结束。

```c
while(condition)
{
    （when false pop loop）
}
```

```c
	while (num < 3)
	{
		printf("Num = %d\n", num);
		num = num + 1;
	}
	    /*
	    Num=0
        Num=0
        Num=0
        */
```

#### do while

当 cond 条件为真时，一直执行 `{}` 里面的代码块，直到 cond 条件为假，循环结束。即使 cond 条件为假，那么 do while 循环也会执行一次

```c
do{
    //do something 
}
while (cond);
```

#### if-esle

```c
if (i > j)
  return i;
else
  return j;
等于=
 (i > j) ? i : j;
```

#### switch

> 语法

```c
switch (expression) {
    case val1:
        //do something
    	break;
    case val2:
         //do something
    	break;
    default:
         //do something
    	break;
}
```

eg.

```c

int main()
{
	int socore = 9;
		switch (socore)
		{
		case 5:
			printf("c");
		case 6:
			printf("b");
		case 0:
			printf("a");
		default:
			break;
		}

		return 0;
}
```



## 数组

> 语法

type +paramlist_name[count]={val1,val2,val3};

```c

int main() {

	int g[5] = { 1,3,4,5 };
	int i = 0;
	for ( i = 0; i < 5; i++)
	{
		printf("i=%d\n",g[i]);
	}
	//printf("\n");

}
/*i=1
i=3
i=4
i=5
i=0*/
```

数组未初始化和未赋值情况

```c
// 不初始化数组
	int arr[5];
	int i = 0;
	for(i = 0; i < 5; i++)
	{
		printf("i = %d\n", arr[i]);
	}
//answer是不确定 
/*
i = -858993460
i = -858993460
i = -858993460
i = -858993460
i = -858993460
*/
```

### 数组赋值



```c
int main() {

	char arr_string[9];
	arr_string[0] = 'w';
	arr_string[1] = 'e';
	arr_string[2] = 'i';
	arr_string[3] = 'a';
	arr_string[4] = 'i';
	arr_string[5] = 'h';
	arr_string[6] = 'u';
	arr_string[7] = 'a';
	arr_string[8] = 'n';
	int i = 0;
	for ( i = 0; i <9; i++)
	{
		printf("%c", arr_string[i]);
	}
	
	return 0;
}

//weiaihuan
```

### 数组length

```c
int arr3[5];
	int count1 = sizeof(arr3) / sizeof(arr3[0]);
	char arr2[13];
	int count2 = sizeof(arr2) / sizeof(arr2[0]);
	printf("Count1 = %d, Count2 = %d\n", count1, count2);
//Count1 = 5, Count2 = 13
```

> 我们使用sizeof获取数组的内存大小除以数组的单个元素占用的内存大小

### 访问数组元素

```c
    int arr5[5] = { 1,6,0 };

	printf("arr5[0]=%d",arr5[0]);
	printf("arr5[4]=%d", arr5[4]);
//arr5[0]=1 
//arr5[4]=0 (超过数组元素赋予随机值)
```

### 遍历数组

```c
int i = 0;
while(i < count)
{
	// arr[i]
	i++;	
 }
```

eg.

```c
int arr5[5] = { 1,6,0 };

while (wi < 4) 
	{
		printf("while arr=%d\n", arr5[wi]);
		wi++;
	}

/*
while arr=1
while arr=6
while arr=0
while arr=0
*/
```

### char 数组

```c
char arrchar[8] = { 'h','e', 'l','e','o'};
		int i = 0;
		for ( i = 0; i < 8; i++)
		{
			printf("%c", arrchar[i]);
		}
//hello
```



### 数组比较compare



```c
  //长度一致，数组元素不一致
        int arr1[4] = { 1,2,3,4, };
        int arr2[4] = { 1,3,4,8,};
//用sizeof获取arr字节大小/arr字节单元素，获取数组len
		int countarr1 = sizeof(arr1) / sizeof(arr1[0]);
		int countarr2 = sizeof(arr2) / sizeof(arr2[0]);
		if (countarr1 != countarr2)
		{
			printf("arr1!=arr2\n");
			return;
		}

		int i2 = 0;
//遍历数组元素进行比较
		for ( i2 = 0; i2 < countarr1; i2++)
		{
			if (arr1[i2] != arr2[i2])
			{
				printf("arr1!=arr2\n");
				return;
			}
			i2++;
		}
		printf("arr1==arr2\n");
```

### 多维数组

> 如果是二维数组，那么数组的每一个元素都是一个一维数组，如果数组是三维数组，那么每一个元素都是一个二维数组

```c
//二维数组

	int arr1[2][3] = 
	{
		{1,1,1},
		{2,2,2}
	};



	int i = 0;
	for ( i = 0; i < 2; i++)
	{
		int j = 0;

		for (j = 0; j < 3; j++)
		{
			printf("%d", arr1[i][j]);
		}
		printf("\n");
	}

	}
```

## 字符串

> 语法

```c
char strName[count] = {'item1', 'item2', .... '\0'};
//字符串的结尾一定是 ‘\0’ 字符。
```

eg.

```c
char arr[9] = {'w','e','i','w','e','i','\0'};
	printf("str=%s\n", arr);
//str=weiwei
```



## 函数

> 语法说明

**[C 语言]** 中 **[函数]** 可以**不返回任何值**，也可以**返回一个值**，但 C 语言的函数**不支持返回多个值**。同时，

C 语言函数的返回值需要**显式的声明其类型**。

```c
type (类型)  +func_name(paramlist)
{
  return val
}
```

eg.

```c
int sum(int a,int b)
{
    return a+b;
}
int main()
{
    int c=sum(1,3);
    printf("%d",c);
}
//4
```

### 函数参数

如果函数的声明中没有写任何参数，那么可以传递任意参数

```c
#include <stdio.h> 
int printint() {
	printf("hello coder\n\n");
}

int main() {

	printint();
	printint(1, 23444);
}
//hello coder

//hello coder

//函数虽然没有报错，但是修改的参数并没有起作用，所以function的参数在传递时候只是参数的传递，
```

## 字符串函数

### gets()

```c
	char str[20];
	printf("输入字符：");
	gets(str);
	printf("输入的字符串：%s\n", str);
```



##  static 关键字

​    常见用法有三种：

- 用于局部变量的修饰符；
- 用于全局变量的修饰符；
- 用于函数的修饰符。

### 用于局部变量

**局部静态变量**，它的值不会因为函数调用的结束而被清除，当函数再次被调用时，它的值是上一次调用结束后的值

```c
void my_fuction() {

	int x = 0;
	static int y = 0;
	printf("x:%d,y:%d\n", x, y);
	x = x + 5;
	y = y + 5;

}

int main(){
    
    my_fuction();
    my_fuction();
    my_fuction();
    
    
}

/*
x:0,y:0
x:0,y:5
x:0,y:10
/*
```

### 用于全局变量修饰

关键字 static 还可用于修饰全局变量，该变量在某一个文件中变量，但不属于任何一个函数内，这样的变量通常称为**静态全局变量**。

静态全局变量的存储位置、初始化操作同静态局部变量的特性，但其作用域有所不同：静态全局变量可以被该文件内的所有函数访问，但不能被其它文件内的函数访问。

```c
static int c=19;
```



### 用于函数修饰

关键字 static 还可以用于修饰一个函数，这样的函数称之为静态函数。

定义一个静态函数就是在函数的返回类型前加上 static 关键字。

静态函数的作用域仅限于本文件，不能被其它文件调用。



## viod 关键字

void 用来表示无类型

表示 “没有任何值可以获得”。因此，不可以采用这个类型声明 **[变量]** 或 **[常量]**。C 语言中的 void 可以用于修饰 **[函数]或者 **[指针])**

### void修饰函数

**没有任何返回值

```c
void func_name(paramlist（参数）)

{
  [return;](无)
}
```

```cc
void p_age(int age)
{
	printf("年纪：%d\n", age);
}

int main()
{
	p_age(18);

}
//年纪:18
```



### void修饰函数参数

```c
void func_name(void) 
{ 
  
}

```

**使用 void 修饰函数的参数，表示函数不接受任何参数**

```c
void print_info(void) {
	printf("无需参数做回自己\n");
}
int main() {
	print_info();
	printf("同步进行？");

}
```

### void修饰指针

**void 指针表示该指针可以接受保存任何类型的变量的地址**。

```c
//void *ptr;

int age = 18;
	void* ptr = &age;
	printf("Age = %d, ptr = %p\n", age, ptr);
//  Age = 18, ptr = 000000DFFCF9FA44
```

## 指针

### 获取变量地址

语法

```
&varName
```

参数

| 参数      | 描述                 |
| --------- | -------------------- |
| *varName* | 要获取地址的变量名。 |

说明

我们使用 `&` 符号获取一个变量的地址。

### 指针变量指向的值

语法

```
*varName
```

参数

| 参数      | 描述                   |
| --------- | ---------------------- |
| *varName* | 要获取值的指针变量名。 |

说明

我们使用 `*` 符号获取一个指针变量所指向的地址的值。

```c
	
	int a = 2022;
	int* pa = &a;
	printf("a=%d,&a=%p\n", a, &a);
	printf("*pa=%d,pa=%p\n", *pa, pa);

/*
a=2022,&a=000000F4FDAFFCF4
*pa=2022,pa=000000F4FDAFFCF4
*/
```

### 数组指针

**数组名=指针**

**并且数组元素在内存里是连续的**

```c
int arr[10];
```

> 数组名来遍历数组元素

```c
int arr[3] = { 1,2,3 };
	int i = 0;
	for (i = 0; i < 3; i++)
	{
		printf("arr[%d]=%d\n", i, *(arr + i));//获取数组名

	}


/*
arr[0]=1
arr[1]=2
arr[2]=3
*/
```

> 使用指针指向数组名,第一种

```c
 int arr[3] = {1, 2, 3};
    int *pArr = arr;
	int i = 0;
    for(i = 0; i < 3; i++)
    {
    	printf("pArr = %d\n", *(pArr+i));
    }

//定义了一个指针，其指向了数组名，接着，我们就直接可以通过指针的移动来访问数组中的元素。
```

> 使用指针指向数组名,第二种

```c
   int arr[3] = {1, 2, 3};
    int *pArr = arr;
	int i = 0;
    for(i = 0; i < 3; i++)
    {
    	printf("pArr = %d\n", *pArr++);
    }

/*
pArr = 1
pArr = 2
pArr = 3
*/
```

## struct-结构体

struct 是由一系列相同或者不同数据的集合

语法：

```c
struct structName{
    变量类型 变量名;
    变量类型 变量名;
    fieldType3 filed3;
};
```



```c
#incule <sto>
struct struct1 {
	int age;
	char* name;
	int class;
};

void main() {
	struct struct1 struct2;//实例化结构体才可以使用结构体的数据
	struct2.age = 19;
	struct2.name = "小王";
	struct2.class = 1;
	printf("age=%d,name=%s,class=%d", struct2.age, struct2.name, struct2.class);
	
}
//age=19,name=小王,class=1
```