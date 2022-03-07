

> Printf()、Sprintf()、Fprintf() 函数的区别用法是什么？

​	都是输出格式化字符串，只是输出到的目标不一样：

​	Printf() 是把格式化字符串输出到标准到标准输出（一般是屏幕，可以重定向）

​	Printf() 是和标准输出文件（stdout）关联的，Fprintf 则没有这个限制

​	Sprintf() 是把格式化字符串输出到指定的字符串中，可以用一个变量来接受，然后在打印

​	Fprintf() 是把格式字符串输出到指定的文件设备中，所以参数比Printf 多一个文件指针*File

## 	linux安装

- 下载到

  ```
  /usr/local
  ```

  位置并解压

  - ```linux
    - cd /usr/local
    - wget https://studygolang.com/dl/golang/go1.17.linux-amd64.tar.gz
    - tar -zxvf go1.17.linux-amd64.tar.gz -C /usr/local/
    - tar -zxvf goland-2021.2.1.tar.gz -C /usr/local/
    
    ```

    

- 在

  ```
  $HOME
  ```

  位置创建

  ```
  gopath
  ```

  工作目录

  - `cd`

  - `mkdir gopath`

  - `cd gopath` 

  - 创建三个bin/pkg/src

    > src 存放源代码（比如：.go .c .h .s等）
    > pkg 编译后生成的文件（比如：.a）
    > bin 编译后生成的可执行文件（为了方便，可以把此目录加入到 $PATH 变量中）

- 配置环境变量并设置代理

  - ```
    vim  /etc/profile
    export GOROOT=/usr/local/go
    export PATH=$PATH:$GOROOT/bin
    export GOPATH=$HOME/gopath
    export GO111MODULE=auto
    export GOPROXY=https://goproxy.cn,direct
    ```

  - `source  /etc/profile`

- 安装

  ```
  git
  ```

  - `yum update`
  - `yum install -y git`

```

"github.com/go-sql-driver/mysql"
```

> 关于goland看源码

driver结构

V：值类型  T: 类型  m：方法    F : 函数

### new和make

都在堆上分配内存，但是它们的行为不同，适用于不同的类型。

new(T) 为每个新的类型T分配一片内存，初始化为 0 并且返回类型为*T的内存地址：这种方法 **返回一个指向类型为 T，值为 0 的地址的指针**，它适用于值类型如数组和结构体；它相当于 &T{}。

make(T) **返回一个类型为 T 的初始值**，它只适用于3种内建的引用类型：slice、map 和 channel。

换言之，new 函数分配内存，make 函数初始化；

![img](https://upload-images.jianshu.io/upload_images/1863311-c7e88fd078c49716.png?imageMogr2/auto-orient/strip|imageView2/2)

```go
package main

import "fmt"

func main() {
    p := new([]int) //p == nil; with len and cap 0
    fmt.Println(p)

    v := make([]int, 10, 50) // v is initialed with len 10, cap 50
    fmt.Println(v)

    /*********Output****************
        &[]
        [0 0 0 0 0 0 0 0 0 0]
    *********************************/

    (*p)[0] = 18        // panic: runtime error: index out of range
                        // because p is a nil pointer, with len and cap 0
    v[1] = 18           // ok
    
}


```



## 常量位运算

```go
const (
   _=iota//定义为0
   n1 = 1<<(10*iota)//1左移10=1024相当于1000000000=2^10=1024
   
    n2 = 1<<(10*iota)//1左移20=1048576 
)
```

## 位运算符

```go
a:=1//001
b:=5//101
a&b=1//从右往左对其相加两位有0出0，全1 出1---001转化10进制为1
a|b=5//从右往左对其相加两位有1出1，全0 出0---101转化10进制为5
a^b=4//从右往左对其不一样有1出1，都一样出0---100转化10进制为4
1<<2//1左移2位原本为1变成100转化10进制为4
4>>2//4右移2位原本为100变成1转化10进制为1

```

## if

```go
if 表达式1 {
    分支1
} else if 表达式2 {
    分支2
} else{
    分支3
}
```

```go
相当于scoreifDeme的局部变量
func ifDemo1() {
	score := 65
	if score >= 90 {
		fmt.Println("A")
	} else if score > 75 {
		fmt.Println("B")
	} else {
		fmt.Println("C")
	}
}//输出c
```

```go
相当于score是ifDeme的局部变量里面if语句中的局部变量
func ifDemo2() {
	if score := 65; score >= 90 {
		fmt.Println("A")
	} else if score > 75 {
		fmt.Println("B")
	} else {
		fmt.Println("C")
	}
}//输出c
```

## for循环的基本格式如下：

```bash
for 初始语句;条件表达式;结束语句{
    循环体语句
}
```

条件表达式返回`true`时循环体不停地进行循环，直到条件表达式返回`false`时自动退出循环。

```go
func forDemo() {
	for i := 0; i < 10; i++ {
		fmt.Println(i)
	}
}//0123456789
```

for循环的初始语句可以被忽略，但是初始语句后的分号必须要写，例如：

```go
func forDemo2() {
	i := 0
	for ; i < 10; i++ {
		fmt.Println(i)
	}
}//0123456789
```

## break跳出循环

```go
for i:=0;i<10;i++{
fmt.Println(i)
if i==3{
break//当i=3跳出for循环
 }
}//0123
```

## break，continue，return

1、break：执行break操作，跳出所在的当前整个循环，到外层代码继续执行。

2、continue：执行continue操作，跳出本次循环，从下一个迭代继续运行循环，内层循环执行完毕，外层代码继续运行。

3、return：执行return操作，直接返回函数，所有该函数体内的代码（包括循环体）都不会再执行。

## return

### 语义

和函数中的renturn语义一样

### 示例

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	tick := time.Tick(time.Second)

	for {
		select {
		case t := <-tick:
			fmt.Println(t)
            return
		}

		fmt.Println("for")
	}

	fmt.Println("main exit")
}
```

输出:

```sh
2019-07-11 13:04:22.874804802 +0800 CST m=+1.000547150
# main函数返回，即整个程序停止
```

## break

### 语义

类似于switch中的break，break后的语句不会被执行到

### 示例

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	tick := time.Tick(time.Second)

	select {
	case t := <-tick:
		fmt.Println("t")
		break
		fmt.Println(t)
	}

	fmt.Println("main exit")
}
```

输出:

```sh
t
main exit
```

在for-select中，break只会影响到select，不会影响到for

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	tick := time.Tick(time.Second)

	for {
		select {
		case t := <-tick:
			fmt.Println("t")
			break
			fmt.Println(t)
		}

		fmt.Println("for")
	}

	fmt.Println("main exit")
}
```

输出:

```sh
t
for
t
for
t
for
#...
```

## continue

### 语义

单独在select中是不能使用continue，会编译错误，只能用在for-select中。continue的语义就类似for中的语义，select后的代码不会被执行到。

### 示例1

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	tick := time.Tick(time.Second)

	for {
		select {
		case t := <-tick:
			fmt.Println("t")
			continue
			fmt.Println(t)
		}

		fmt.Println("for")
	}

	fmt.Println("main exit")
}
```

输出:

```sh
t
t
t
#..
```

## switch

使用`switch`语句可方便地对大量的值进行条件判断。

```go
func switchDemo1() {
	finger := 3
	switch finger {
	case 1:
		fmt.Println("大拇指")
	case 2:
		fmt.Println("食指")
	case 3:
		fmt.Println("中指")
	case 4:
		fmt.Println("无名指")
	case 5:
		fmt.Println("小拇指")
	default:
		fmt.Println("无效的输入！")
	}
}//中指
```

## 匿名函数

```go
一般---func 函数名(参数)(返回值){
    函数体
}
第一种变量赋值
func mian(){
    mm:=func(){
    fmt.Println("kk")
    }//将func赋值给mm
   mm
}
 //第二种
//定义且立即执行后面加（）
func mian(){
    func(){
    fmt.Println("kk")
    }()  
}
结果都为kk
```

## 闭包

```go
func a()func(){
    name:="xiaonan"
    return func(){
        fmt.Println("闭包就是返回一个函数+引用外部变量",name)
    }
}
func main(){
    r:=a()
    r()//引用a函数内部的匿名函数，而且也引用外部的变量的引用
}//输出为   闭包就是返回一个函数+外部变量 xiaonan
```

## 闭包进阶

```go

func calc(base int) func(int)int,func(int)int{
    add:=func(int)int{
        base +=i
        return base
    }
    sub:=func(int)int{
        base -=i
        return base
    }
    return add,sub
}//base 为返回匿名函数之外的外部局部变量
//如何调用
func main(){
   x,y:=calc(100) //100为base值，x为第一个匿名函数的返回值，y为第二个匿名函数的返回值的
    e1:=x(200)//func(int)in--base=100+200=300
    fmt.Println(e1)
    e2：=y(200)//func(int)in--base=300-200=100
    fmt.Println(e2)
}
```

## 内置函数介绍

| 内置函数       | 介绍                                                         |
| -------------- | ------------------------------------------------------------ |
| close          | 主要用来关闭channel                                          |
| len            | 用来求长度，比如string、array、slice、map、channel           |
| new            | 用来分配内存，主要用来分配值类型，比如int、struct。返回的是指针 |
| make           | 用来分配内存，主要用来分配引用类型，比如chan、map、slice     |
| append         | 用来追加元素到数组、slice中                                  |
| panic和recover | 用来做错误处理                                               |

## struct

`结构体`定义完之后，并不能直接使用，因此此时还没有分配内存，因此，我们必须在定义完结构体，并实例话之后才可以使用结构体。

声明结构体

```go
type 结构体名 struct {
    属性名   属性类型
    属性名   属性类型
    ...
}
```

比如我要定义一个可以存储个人资料名为 Profile 的结构体，可以这么写

```go
type Profile struct {
    name   string
    age    int
    gender string
    mother *Profile // 指针
    father *Profile // 指针
}
```

对其进行实例化

```go
p:=profile{
    name:"keke",
    age: 18,
    gender: "男",
}//如果不想用某个值实例化值为“nil”
```

> Golang 中结构体的实例化主要有三种方式，分别为：使用变量定义的方式、使用 new 分配内存和使用 & 符号定义结构体。使用变量定义的方式：

```
var varName StructName
```

```go
//使用变量定义的方式，实例化一个名为 Student 的结构体
	type Student struct {
		Name string
		Age int
		Score float64
	}
	var stu Student
	stu.Name = "kongzi"
	stu.Age = 1998
	stu.Score = 100.0
	fmt.Println("Student =", stu)
//Student = {kongzi 1998 100}

```

> 使用 new 分配内存的方式：

```
var varName = new(StructName)
```

```go
	//使用new分配内存的方式，实例化一个名为 Student 的结构体
	type Student struct {
		Name string
		Age int
		Score float64
	}
	var stu = new(Student)
	stu.Name = "kongzi"
	stu.Age = 1998
	stu.Score = 100.0
	fmt.Println("Student =", stu)

//输出Student = &{kongzi 1998 100}

```

> 使用取地址 & 符号定义结构体：

> 使用 & 定义一个名为 varName 的结构体，使用 & 符号定义时，结构体名后面必须要加 {}。

```
var varName = &StructName{}
```

```go
//第一张
type Student struct {
		Name string
		Age int
		Score float64
	}
	var stu = &Student{}
	stu.Name = "kongzi"
	stu.Age = 1998
	stu.Score = 100.0
	fmt.Println("Student =", stu)
//输出 Student = &{kongzi 1998 100}

//第二种简写(会自动匹配对应内容)

	stu:=&Student{
		"kongzi",
		1998,
		100.0,
	fmt.Println("Student =", stu)
//输出 Student = &{kongzi 1998 100}

```

结构体继承

```go
func main() {
   myCom := company{
      companyName: "Tencent",
      companyAddr: "深圳市南山区",
   }
   staffInfo := staff{
      name:     "小明",
      age:      28,
      gender:   "男",
      position: "云计算开发工程师",
      company:  myCom,
   }

   fmt.Printf("%s 在 %s 工作\n", staffInfo.name, staffInfo.companyName)
   fmt.Printf("%s 在 %s 工作\n", staffInfo.name, staffInfo.company.companyName)
}
//输出结果如下，可见staffInfo.companyName 和 staffInfo.company.companyName 的效果是一样的
```

## 回调函数

这里的排序函数就是回调函数。每取一次i和j对应的元素，就调用一次less函数。

```go
s1 := []int{112, 22, 52, 32, 12}
// 定义排序函数
less := func(i, j int) bool {
   // 降序排序
   return s1[i] > s1[j]
   // 升序排序：s1[i] < s1[j]
}
//
sort.SliceStable(s1, less)
fmt.Println(s1)

```

## 结构体构造函数

下方的代码就实现了一个`person`的构造函数。 因为`struct`是值类型，如果结构体比较复杂的话，值拷贝性能开销会比较大，所以该构造函数返回的是结构体指针类型。

**加&符号和new的是指针对象，没有的则是值对象，这点和php、java不一致，在传递对象的时候要根据实际情况来决定是要传递指针还是值。**

tips：当对象比较小的时候传递指针并不划算。

```go
type person struct {
	name1, city1 string
	age1         int8
}

func newPerson(name, city string, age int8) *person {
	return &person{
		name1: name,
		city1: city,
		age1:  age,
	}
}

```

调用构造函数

```go
p9 := newPerson("张三", "沙河", 90)
fmt.Printf("%#v\n", p9) //&main.person{name:"张三", city:"沙河", age:90}
```

## 方法和接收者

方法的定义格式如下：

```go
func (接收者变量名 接收者类型) 方法名(参数列表) (返回参数) {
    函数体
}
```

- 接收者变量：接收者中的参数变量名在命名时，官方建议使用接收者类型名称首字母的小写，而不是`self`、`this`之类的命名。例如，`Person`类型的接收者变量应该命名为 `p`，`Connector`类型的接收者变量应该命名为`c`等。

- 接收者类型：接收者类型和参数类似，可以是指针类型和非指针类型。

- 方法名、参数列表、返回参数：具体格式与函数定义相同。

  > 建议全部使用指针接收者
  
  ![img](https://img.jbzj.com/file_images/article/202012/20201215142103.jpg)
  
  1、& 是取地址符号 , 即取得某个变量的地址 , 如 ; &a
  
  ![img](https://img.jbzj.com/file_images/article/202012/20201215142112.jpg)
  
  2、*是指针运算符 , 可以表示一个变量是指针类型 , 也可以表示一个指针变量所指向的存储单元 , 也就是这个地址所存储的值 .
  
  ![img](https://img.jbzj.com/file_images/article/202012/20201215142120.jpg)

```go
//Person 结构体
type Person struct {
	name string
	age  int8
}

//NewPerson 构造函数
func NewPerson(name string, age int8) *Person {
	return &Person{
		name: name,
		age:  age,
	}
}

//Dream Person做梦的方法
func (p Person) Dream() {
	fmt.Printf("%s的梦想是学好Go语言！\n", p.name)
}

func main() {
	p1 := NewPerson("小王子", 25)
	p1.Dream()
}
```

```go
1.定义方法
type Profile struct {
	name   string
	age    int
	gender string
	mother *Profile // 指针
	father *Profile // 指针
}
// 定义一个与 Profile 的绑定的方法
//其中FmtProfile 是方法名，而(person Profile) ：表示将 FmtProfile 方法与 Profile 的实例绑定。
//我们把 Profile 称为方法的接收者，而 person 表示实例本身，
//它相当于 Python 中的 self，在方法内可以使用 person.属性名 的方法来访问实例属性。
func (person Profile) FmtProfile() {
	fmt.Printf("名字：%s\n", person.name)
	fmt.Printf("年龄：%d\n", person.age)
	fmt.Printf("性别：%s\n", person.gender)
}
func main() {
	// 实例化
	myself := Profile{name: "小明", age: 24, gender: "male"}
	// 调用函数
	myself.FmtProfile()
}
//输出：
//名字：小明
//年龄：24
//性别：male
```

现在这里有一个表示公司（company）的结构体，还有一个表示公司职员（staff）的结构体。

```go
type company struct {
    companyName string
    companyAddr string
}

type staff struct {
    name string
    age int
    gender string
    position string
}
```

若要将公司信息与公司职员关联起来

```go
type staff struct {
    name string
    age int
    gender string
    position string
    company   // 匿名字段
}
```

```go
func main()  {
    myCom := company{
        companyName: "Tencent",
        companyAddr: "深圳市南山区",
    }
    staffInfo := staff{
        name:     "小明",
        age:      28,
        gender:   "男",
        position: "云计算开发工程师",
        company: myCom,
    }

    fmt.Printf("%s 在 %s 工作\n", staffInfo.name, staffInfo.companyName)
    fmt.Printf("%s 在 %s 工作\n", staffInfo.name, staffInfo.company.companyName)
}
```

输出结果如下，可见`staffInfo.companyName` 和 `staffInfo.company.companyName` 的效果是一样的。

```bash
小明 在 Tencent 工作
小明 在 Tencent 工作
```

```go
//结构体的可见性无非大写公开，小写私有
type student struct {
   name string
   id   int
}
//构造函数
func newStudent(id int,name string) student {
   return student{
      id:   id,
      name: name,

   }
}

type class struct {
   title   string
   Student []student
}


//实例化
func main() {

   c1 := class{
      title:   "终极一班",
      Student: make([]student, 0, 20),//切片使用时要对其进行初始化表示type，size，容量
   }
   //往班级添加学生
   for i := 0; i < 10; i++ {
      //弄十个学生
      tmpstu := newStudent(i, fmt.Sprintf("stu%02d", i))//stu%02d表示补位后两位
      c1.Student = append(c1.Student, tmpstu)
   }
   fmt.Printf("%#v\n", c1)
}//输出如下
//main.class{title:"终极一班", Student:[]main.student{main.student{name:"stu00", id:0}, main.student{name:"stu01", id:1}, main.student{name:"stu02", id:2}, main.student{name:"stu03", id:3}, main.student{name:"stu04", id:4}, main.student{name:"stu05", id:5}, main.student{name:"stu06", id:6}, main.student{name:"stu07", id:7}, main.student{name:"stu08", id:8}, main.student{name:"stu09", id:9}}}
 
```

## json

反引号

```go
//json序列号go语言的数据，
json.Marshal(数据)---data,err:=json.Marshal(数据)
if err！=nil{
fmt.Println("json.Marshal failed",err)
return
}
	fmt.Printf("json:%s\n", data)
//JSON反序列化：JSON格式的字符串-->结构体
	str := `{"Title":"101","Students":[{"ID":0,"Gender":"男","Name":"stu00"},{"ID":1,"Gender":"男","Name":"stu01"},{"ID":2,"Gender":"男","Name":"stu02"},{"ID":3,"Gender":"男","Name":"stu03"},{"ID":4,"Gender":"男","Name":"stu04"},{"ID":5,"Gender":"男","Name":"stu05"},{"ID":6,"Gender":"男","Name":"stu06"},{"ID":7,"Gender":"男","Name":"stu07"},{"ID":8,"Gender":"男","Name":"stu08"},{"ID":9,"Gender":"男","Name":"stu09"}]}`
	c1 := &Class{}
	err = json.Unmarshal([]byte(str), c1)
	if err != nil {
		fmt.Println("json unmarshal failed!")
		return
	}
	fmt.Printf("%#v\n", c1)
```

```go
//Student 学生
type Student struct {
	ID     int    `json:"id"` //比如database数据id是小写的为了可以识别，通过指定tag实现json序列化该字段时的key。
	Gender string //json序列化是默认使用字段名作为key
	name   string //私有不能被json包访问
}
```

## 接口是一种类型

返回值按照开发者需求定义，可以不写

```go
type 接口类型名 interface{
    方法名1( 参数列表1 ) 返回值列表1
    方法名2( 参数列表2 ) 返回值列表2
    …
}
```

一个对象只要全部实现了接口中的方法，那么就实现了这个接口。换句话说，接口就是一个**需要实现的方法列表**。

我们来定义一个`Sayer`接口：

```go
// Sayer 接口
type Sayer interface {
	say()
}
```

定义`dog`和`cat`两个结构体：

```go
type dog struct {}

type cat struct {}
```

因为`Sayer`接口里只有一个`say`方法，所以我们只需要给`dog`和`cat `分别实现`say`方法就可以实现`Sayer`接口了。

```go
// dog实现了Sayer接口
func (d dog) say() {
	fmt.Println("汪汪汪")
}

// cat实现了Sayer接口
func (c cat) say() {
	fmt.Println("喵喵喵")
}
```

打的函数

```go
//无论传的是什么值都会都会执行da的say方法
func da(arg sayer){
arg.say()
}//打了他都会叫
func main() {
	var x Sayer // 声明一个Sayer类型的变量x
	a := cat{}  // 实例化一个cat
	b := dog{}  // 实例化一个dog
	x = a       // 可以把cat实例直接赋值给x
	x.say()     // 喵喵喵
	x = b       // 可以把dog实例直接赋值给x
	x.say()     // 汪汪汪
}

```



## goroutine

并发：同一时间段内执行多个任务

### 使用goroutine

Go语言中使用`goroutine`非常简单，只需要在调用函数的时候在前面加上`go`关键字，就可以为一个函数创建一个`goroutine`。

一个`goroutine`必定对应一个函数，可以创建多个`goroutine`去执行相同的函数。

### 启动单个goroutine

启动goroutine的方式非常简单，只需要在调用的函数（普通函数和匿名函数）前面加上一个`go`关键字。

举个例子如下：

```go
func hello() {
	fmt.Println("Hello Goroutine!")
}
func main() {
	hello()
	fmt.Println("main goroutine done!")
}
```

这个示例中hello函数和下面的语句是串行的，执行的结果是打印完`Hello Goroutine!`后打印`main goroutine done!`。

接下来我们在调用hello函数前面加上关键字`go`，也就是启动一个goroutine去执行hello这个函数。

```go
func main() {
	go hello() // 启动另外一个goroutine去执行hello函数
	fmt.Println("main goroutine done!")
}
```

这一次的执行结果只打印了`main goroutine done!`，并没有打印`Hello Goroutine!`。为什么呢？

在程序启动时，Go程序就会为`main()`函数创建一个默认的`goroutine`。

当main()函数返回的时候该`goroutine`就结束了，所有在`main()`函数中启动的`goroutine`会一同结束，`main`函数所在的`goroutine`就像是权利的游戏中的夜王，其他的`goroutine`都是异鬼，夜王一死它转化的那些异鬼也就全部GG了。

所以我们要想办法让main函数等一等hello函数，最简单粗暴的方式就是`time.Sleep`了。

```go
func main() {
	go hello() // 启动另外一个goroutine去执行hello函数
	fmt.Println("main goroutine done!")
	time.Sleep(time.Second)
}
```

执行上面的代码你会发现，这一次先打印`main goroutine done!`，然后紧接着打印`Hello Goroutine!`。

首先为什么会先打印`main goroutine done!`是因为我们在创建新的goroutine的时候需要花费一些时间，而此时main函数所在的`goroutine`是继续执行的。

## channel

单纯地将函数并发执行是没有意义的。函数与函数间需要交换数据才能体现并发执行函数的意义。

### channel类型

`channel`是一种类型，一种引用类型。声明通道类型的格式如下：

```go
var 变量 chan 元素类型
```

举几个例子：

```go
var ch1 chan int   // 声明一个传递整型的通道
var ch2 chan bool  // 声明一个传递布尔型的通道
var ch3 chan []int // 声明一个传递int切片的通道
```

### 创建channel

通道是引用类型，通道类型的空值是`nil`。

```go
var ch chan int
fmt.Println(ch) // <nil>
```

建有缓冲channel的格式如下：

```go
make(chan 元素类型, [缓冲大小])
```

channel的缓冲大小是可选的。

```go
func main() {
	ch := make(chan int, 1) // 创建一个容量为1的有缓冲区通道
	ch <- 10
	fmt.Println("发送成功")
}
```

## channel example1

建立打印先后数据

```go
package main
 
import (
    "fmt"
    "time"
)
 
var s = make(chan int)
 
func printer(stc string) {
    for _, v := range stc {
        fmt.Printf("%c", v) //打印字符
        time.Sleep(time.Millisecond * 300)
    }
 
}
func person1() {
     
    printer("hello")
    s <- 00
}
func person2() {
    <-s
 
    printer("world")
 
}
func main() {
    go person1()
    go person2()
    for {
 ;
    }
}
//hello
//world
```

## channel example2通知

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	stop := make(chan bool)
	go func() {
		for {
			select {
			case <-stop://接受到true信号打印
				fmt.Println("停止chan")
				return//返回main
			default://没有true信号打印5秒
				fmt.Println("继续chan")
				time.Sleep(1 * time.Second)
			}
		}
	}()
	time.Sleep(5 * time.Second)
	fmt.Println("停止gorutine")
	stop <- true//5秒后有true信号
	time.Sleep(5 * time.Second)
}
//继续chan
//继续chan
//继续chan
//继续chan
//继续chan
//停止gorutine
//停止chan
//
//进程 已完成，退出代码为 0

```



## 无缓冲的通道

无缓冲的通道又称为阻塞的通道

有发送必须有接受，不然就可能造成堵塞

我们来看一下下面的代码：

```go
func main() {
	ch := make(chan int)
	ch <- 10
	fmt.Println("发送成功")
}
```

上面这段代码能够通过编译，但是执行的时候会出现以下错误：

```bash
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan send]:
main.main()
        .../src/github.com/Q1mi/studygo/day06/channel02/main.go:8 +0x54
```

为什么会出现`deadlock`错误呢？

因为我们使用`ch := make(chan int)`创建的是无缓冲的通道，无缓冲的通道只有在有人接收值的时候才能发送值。就像你住的小区没有快递柜和代收点，快递员给你打电话必须要把这个物品送到你的手中，简单来说就是无缓冲的通道必须有接收才能发送。

上面的代码会阻塞在`ch <- 10`这一行代码形成死锁，那如何解决这个问题呢？

一种方法是启用一个`goroutine`去接收值，例如：

```go
func recv(c chan int) {
	ret := <-c
	fmt.Println("接收成功", ret)
}
func main() {
	ch := make(chan int)
	go recv(ch) // 启用goroutine从通道接收值
	ch <- 10
	fmt.Println("发送成功")
}
```

无缓冲通道上的发送操作会阻塞，直到另一个`goroutine`在该通道上执行接收操作，这时值才能发送成功，两个`goroutine`将继续执行。相反，如果接收操作先执行，接收方的goroutine将阻塞，直到另一个`goroutine`在该通道上发送一个值。

使用无缓冲通道进行通信将导致发送和接收的`goroutine`同步化。因此，无缓冲通道也被称为`同步通道`。

### 有缓冲的通道

解决上面问题的方法还有一种就是使用有缓冲区的通道。我们可以在使用make函数初始化通道的时候为其指定通道的容量，例如：

```go
func main() {
	ch := make(chan int, 1) // 创建一个容量为1的有缓冲区通道
	ch <- 10
	fmt.Println("发送成功")
}
```

只要通道的容量大于零，那么该通道就是有缓冲的通道，通道的容量表示通道中能存放元素的数量。就像你小区的快递柜只有那么个多格子，格子满了就装不下了，就阻塞了，等到别人取走一个快递员就能往里面放一个。

我们可以使用内置的`len`函数获取通道内元素的数量，使用`cap`函数获取通道的容量，虽然我们很少会这么做。

## goroutine和channel协同工作例子

```go
package main

import "fmt"
//两个goroutine,两个channel
//生成1-100数字发送到ch1.从ch1取出数据计算平方把结果发送到ch2
//生成1-100数字发送到ch1
func f1(ch1 chan int){
   for i:=0;i<100;i++ {
      ch1<-i
   }
   close(ch1)
}
//取出数字,计算
func f2(ch1 chan int,ch2 chan int)  {
   //从通道中取值的方式1，当！ok 表示fales值 以已经取完了，使用break跳出循环，ch2对取出的值进行平方计算0^2-1^2....到100^2
   //最后开始关闭ch2
   for  {
      tmp,ok:=<-ch1
      if !ok {
         break
      }
      ch2<-tmp*tmp
   }
   close(ch2)
}
func main() {
   ch1:=make(chan int,100 )//初始化通道,缓冲100
   
   ch2:=make(chan int ,100)//初始化通道,缓冲100
   go f1(ch1)//开启ch1
   go f2(ch1,ch2)//
   //第二种取值for range会在内部自己判断当取值返回fales会自动退出
   for jieguo:= range ch2{
      fmt.Println(jieguo)
   }
}
```

## 单向通道

`chan<- int`是一个只写单向通道（只能对其写入int类型值），可以对其执行发送操作但是不能执行接收操作；

`<-chan int`是一个只读单向通道（只能从其读取int类型值），可以对其执行接收操作但是不能执行发送操作

## select 和chan配套使用

```go
import (
   "fmt"
   "time"
)

func main() {

   flag := make(chan bool)
   message := make(chan int)
   go son(flag, message)//将flag, message传到son
   for i := 0; i < 10; i++ {
      message <- i
   }
   flag <- true
   time.Sleep(time.Second)

}
func son(flg chan bool, msg chan int) {
   t := time.Tick(time.Second)//1秒钟
   for _ = range t {
      select {
      case m := <-msg:
         fmt.Printf("接受到值%d\n", m)
      case <-flg:
         fmt.Println("结束")
         break
      }

   }

}
//接受到值0
//接受到值1
//接受到值2
//接受到值3
//接受到值4
//接受到值5
//接受到值6
//接受到值7
//接受到值8
//接受到值9
//结束

```

常用控制并发的两种方式sync.WaitGroup和context

## Mutex和RWMuteux 

使用互斥锁（Mutex，全称 mutual exclusion）是为了来保护一个资源不会因为并发操作而引起冲突导致数据不准确。

` Mutex 锁`的两种定义方法

```
// 第一种
var lock *sync.Mutex
lock = new(sync.Mutex)

// 第二种
lock := &sync.Mutex{}
```

```go
package main

import (
    "fmt"
    "sync"
)

func add(count *int, wg *sync.WaitGroup) {
    for i := 0; i < 1000; i++ {
        *count = *count + 1
    }
    wg.Done()
}

func main() {
    var wg sync.WaitGroup
    count := 0
    wg.Add(3)
    go add(&count, &wg)
    go add(&count, &wg)
    go add(&count, &wg)

    wg.Wait()
    fmt.Println("count 的值为：", count)
}
```

```go
// 第一次
count 的值为： 2854

// 第二次
count 的值为： 2673

// 第三次
count 的值为： 2840
```



```go
//开启了三个协程，每个协程分别往 count 这个变量加1000次 1，理论上看，最终的 count 值应试为 3000

func add(count *int, wg *sync.WaitGroup,lock *sync.Mutex) {
   for i := 0; i < 1000; i++ {
      //加锁
      lock.Lock()
      *count = *count + 1
      //解锁
      lock.Unlock()
   }
   wg.Done()
}
func main() {
   var wg sync.WaitGroup
   lock := &sync.Mutex{}
   count := 0
   wg.Add(3)
   go add(&count, &wg,lock)
   go add(&count, &wg,lock)
   go add(&count, &wg,lock)
   wg.Wait()
   fmt.Println(count)
}
```

RWMutex，也是如此，它将程序对资源的访问分为读操作和写操作

- 为了保证数据的安全，它规定了当有人还在读取数据（即读锁占用）时，不允计有人更新这个数据（即写锁会阻塞）
- 为了保证程序的效率，多个人（线程）读取数据（拥有读锁）时，互不影响不会造成阻塞，它不会像 Mutex 那样只允许有一个人（线程）读取同一个数据。

定义一个 `RWMuteux 锁`，有两种方法

```
// 第一种
var lock *sync.RWMutex
lock = new(sync.RWMutex)

// 第二种
lock := &sync.RWMutex{}
```

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func main() {
    lock := &sync.RWMutex{}
    lock.Lock()

    for i := 0; i < 4; i++ {
        go func(i int) {
            fmt.Printf("第 %d 个协程准备开始... \n", i)
            lock.RLock()
            fmt.Printf("第 %d 个协程获得读锁, sleep 1s 后，释放锁\n", i)
            time.Sleep(time.Second)
            lock.RUnlock()
        }(i)
    }

    time.Sleep(time.Second * 2)

    fmt.Println("准备释放写锁，读锁不再阻塞")
    // 写锁一释放，读锁就自由了
    lock.Unlock()

    // 由于会等到读锁全部释放，才能获得写锁
    // 因为这里一定会在上面 4 个协程全部完成才能往下走
    lock.Lock()
    fmt.Println("程序退出...")
    lock.Unlock()
}
第 1 个协程准备开始...
第 0 个协程准备开始...
第 3 个协程准备开始...
第 2 个协程准备开始...
准备释放写锁，读锁不再阻塞
第 2 个协程获得读锁, sleep 1s 后，释放锁
第 3 个协程获得读锁, sleep 1s 后，释放锁
第 1 个协程获得读锁, sleep 1s 后，释放锁
第 0 个协程获得读锁, sleep 1s 后，释放锁
程序退出...
```



## sync.WaitGroup 类型（多个goroutine执行同一件事情）



`sync.WaitGroup` 类型是开箱即用的，也是并发安全的。该类型提供了以下三个方法：

一般定义一个全局的 var wg sync.waitgroup

- `Add`：`WaitGroup` 类型有一个计数器，默认值是0，我们可以通过 `Add` 方法来增加这个计数器的值，通常我们可以通过个方法来标记需要等待的子协程数量；wg.add(要执行的任务）

- `Done`：当某个子协程执行完毕后，可以通过 `Done` 方法标记已完成，该方法会将所属 `WaitGroup` 类型实例计数器值减一，通常可以通过 `defer` 语句来调用它；wg.done()标记已完成

- `Wait`：`Wait` 方法的作用是阻塞当前协程，直到对应 `WaitGroup` 类型实例的计数器值归零，如果在该方法被调用的时候，对应计数器的值已经是 0，那么它将不会做任何事情。wg.wait()

- 因为main函数自带gorutine，如果没有标记会直接忽视

  ```go
  package main
  
  import (
  	"fmt"
  	"sync"
  	"time"
  )
  
  var wg sync.WaitGroup
  
  func say(id string) {
  	time.Sleep(time.Second)//启动一个两秒的任务
  	fmt.Println("你好", id)
  	wg.Done()//表示任务已完成
  }
  func main() {
  	wg.Add(2)//有两个任务
  	go say("hello")
  	go say("wrold")
  	wg.Wait()//等待所有任务停止
  	fmt.Println("eixt")
  }
  //你好 hello
  //你好 wrold
  //eixt
  ```

  也可以用匿名函数

  ```go
  go func(id string) {//这样就会让它先执行，等待两秒后执行	go say("wrold")
      fmt.Println(id)
  	wg.Done()}("hello")
  	go say("wrold")
  ```

  ## context包（多个goroutine内又有goroutine）

  先举个例子：为什么需要context

   Context 4个方法      

  WithCancel创建一个带有clear（）关闭的ctx上下文

  WithDeadline 创建一个带有超时间点关闭的ctx上下文（时间段12：10）

   WithTimeout 创建一个有超时间的ctx（时间秒10秒后关闭）

  WithValue 创建一个携带了参数的ctx

  ​      Context 6个基础接口

  ​      Deadline（）

  ​      Done（）

  ​      Err（）

  ​      Value（）

  ​      TODO（）不确定CTX使用

  ​      Background（）确定CTX使用

  ```go
  //之前的channel例子用context改写意思也一样
  package main
  
  import (
  	"context"
  	"fmt"
  	"time"
  )
  
  func main() {
  	ctx, cancel := context.WithCancel(context.Background())//cancel取消的意思
  	go func() {
  		for {
  			select {
  			case <-ctx.Done()://收到停止的信息
  				fmt.Println("停止chan")
  				return //返回main
  			default: //没有true信号打印5秒
  				fmt.Println("继续chan")
  				time.Sleep(1 * time.Second)
  			}
  		}
  	}()
  	time.Sleep(5 * time.Second)
  	fmt.Println("停止gorutine")
  	cancel()//停止
  	time.Sleep(5 * time.Second)
  }
  ```

  

## 多个goroutine内部有goroutine

`Context`通常被译作`上下文`，它是一个比较抽象的概念。在公司技术讨论时也经常会提到`上下文`。一般理解为程序单元的一个运行状态、现场、快照，而翻译中`上下`又很好地诠释了其本质，上下上下则是存在上下层的传递，`上`会把内容传递给`下`。在Go语言中，程序单元也就指的是Goroutine。

每个Goroutine在执行之前，都要先知道程序当前的执行状态，通常将这些执行状态封装在一个`Context`变量中，传递给要执行的Goroutine中。上下文则几乎已经成为传递与请求同生存周期变量的标准方法。在网络编程下，当接收到一个网络请求Request，处理Request时，我们可能需要开启不同的Goroutine来获取数据与逻辑处理，即一个请求Request，会在多个Goroutine中处理。而这些Goroutine可能需要共享Request的一些信息；同时当Request被取消或者超时的时候，所有从这个Request创建的所有Goroutine**也应该被结束**

proto文件生成的代码，接口函数第一个参数统一是`ctx context.Context`接口s

```go

func worker(ctx context.Context, name string) {
	go func() {
		for {
			select {
			case <-ctx.Done():
				fmt.Println(name, "工作停止")
				return
			default:
				fmt.Println(name, "工作中")
				time.Sleep(1 * time.Second)//三个一秒
			}
		}
	}()
}
func main() {
	ctx, cancel := context.WithCancel(context.Background())
	go worker(ctx, "工作者一号")
	go worker(ctx, "工作者二号")
	go worker(ctx, "工作者三号")
	time.Sleep(5 * time.Second)
	fmt.Println("停止gorutine")
	cancel()
	time.Sleep(5 * time.Second)
}
//工作者一号 工作中
//工作者二号 工作中
//工作者三号 工作中
//工作者三号 工作中
//工作者一号 工作中
//工作者二号 工作中
//工作者二号 工作中
//工作者一号 工作中
//工作者三号 工作中
//工作者一号 工作中
//工作者二号 工作中
//工作者三号 工作中
//工作者三号 工作中
//工作者二号 工作中
//工作者一号 工作中
//停止gorutine
//工作者一号 工作停止
//工作者二号 工作停止
//工作者三号 工作停止

```

## 文件io操作

type reader interface{ read(p []byte)(n int,err error) }将len（p）个字节读取到p中

type wirter interface{write(p []byte)(n int, err error) }wirte方法将p数据写入对象数据中

type seeker interface{seek(offset int64,whence int)(ret int64,err error)}控制光标位置

type closer interface{close() error}关闭文件，连接，数据库

```go
func file() {
	f, err := os.OpenFile("./test1.txt", os.O_CREATE|os.O_RDWR, 0777)
	/*
		文件名，创建，读写，最高命令
	*/
	if err != nil {
		fmt.Println(err)
		return

	}
	}
```

```go
package main

import (
"bufio"
"fmt"
"io"
"os"
)

func main() {
// 1.读取一个文件的内容
file, err := os.Open("./abc.txt")
if err != nil {
fmt.Println("open file err:", err.Error())
return
}

// 2.处理结束后关闭文件
defer file.Close()

// 3.使用bufio读取
r := bufio.NewReader(file)

for {
// 4.分行读取文件  ReadLine返回单个行，不包括行尾字节(\n  或 \r\n)
//data, _, err := r.ReadLine()

//5. 以分隔符形式读取,比如此处设置的分割符是\n,则遇到\n就返回,且包括\n本身 直接返回字符串
//str, err := r.ReadString('\n')

// 6.以分隔符形式读取,比如此处设置的分割符是\n,则遇到\n就返回,且包括\n本身 直接返回字节数数组
data, err := r.ReadBytes('\n')

// 7.读取到末尾退出
if err == io.EOF {
break
}

if err != nil {
fmt.Println("read err", err.Error())
break
}

//8. 打印出内容
fmt.Printf("%v", string(data))
}
}
```

## 规范注释和单元测试

[Golang 注释规范 - ZhiChao& - 博客园 (cnblogs.com)](https://www.cnblogs.com/zhichaoma/p/12509999.html)

[go-test 单元测试](https://studygolang.gitbook.io/learn-go-with-tests/)

## go启动http服务的两种方式

> server

```go
//ResponseWriter用来“给客户端回发数据”。它是一个interface：
type ResponseWriter interface {    
    Header() Header     
    Write([]byte) (int, error)    
    WriteHeader(statusCode int) }
//Request 用来“接收客户端发送的数据”。浏览器发送给服务器的http请求包的内容可以借助r来查看。它对应一个结构体：
type Request struct {
    Method string		// 浏览器请求方法 GET、POST…
    URL *url.URL		// 浏览器请求的访问路径
	……
    Header Header		// 请求头部
    Body io.ReadCloser	// 请求包体
    RemoteAddr string	// 浏览器地址
    ……
    ctx context.Context
}

```



> http.Handle 和 http.HandleFunc 

```go
//http.handle

func Handle(pattern string, handler Handler) { DefaultServeMux.Handle(pattern, handler) }


//第二个参数是Handler这个接口, 这个接口有一个ServeHTTP()的方法
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}
```

第一个参数是pattern string 文件的路径

第二个参数是Handler这个接口, 这个接口有一个ServeHTTP()的方法

```javascript
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}
```

所以这个方法使用的时候需要自己去定义struct实现这个**Handler**接口。

```go
package main

import (
	"net/http"
	"log"
)

type httpServer struct {
}

func (dms *httpServer) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte(r.URL.Path))
}

func main() {
    //实例化
    server:= httpServer{}
	http.Handle("/", &server) //uri
	log.Fatal(http.ListenAndServe("localhost:9000", nil))
// 当参数2为nil时，服务端调用http.DefaultServeMux进行处理
```

第二种方法

这个第二个参数是一个方法，参数是ResponseWriter, 和 *Request 所以使用的时候需要传方法。

```go
func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
	DefaultServeMux.HandleFunc(pattern, handler)
}
```

```go
package main

import (
	"net/http"
	"log"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte(r.URL.Path))
	})
	log.Fatal(http.ListenAndServe("localhost:9000", nil))
// 当参数2为nil时，服务端调用http.DefaultServeMux进行处理
```

一般使用HandleFunc

> ServeMux(服务复用器)

上面的代码中可以看到不论是使用`http.HandleFunc`还是`http.Handle`注册路由的处理函数时最后都会用到`ServerMux`结构的`Handle`方法去注册路由处理函数。

我们先来看一下`ServeMux`的定义：

```text
type ServeMux struct {
    mu    sync.RWMutex
    m     map[string]muxEntry
    es    []muxEntry // slice of entries sorted from longest to shortest.
    hosts bool       // whether any patterns contain hostnames
}

type muxEntry struct {
    h       Handler
    pattern string
}
```

`ServeMux`中的字段`m`，是一个`map`，`key`是[路由表达式](https://www.zhihu.com/search?q=路由表达式&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A104182198})，`value`是一个`muxEntry`结构，`muxEntry`结构体存储了路由表达式和对应的`handler`。字段`m`对应的 `map`用于路由的精确匹配而`es`字段的`slice`会用于路由的部分匹配



> client 



Response 表示来自 HTTP 请求的响应。一旦收到响应头，客户端和传输从服务器返回响应。响应正文在读取 Body 字段时按需流式传输。

客户端访问web服务器数据，主要使用函数

```go
func Get(url string) (resp *Response, err error) {
   return DefaultClient.Get(url)
}
```

来完成。读到的响应报文数据被保存在 Response 结构体中。

```go
type Response struct {
   Status     string // e.g. "200 OK"
   StatusCode int    // e.g.  200
   Proto      string // e.g. "HTTP/1.0"
   ……
   Header Header
   Body io.ReadCloser
   ……
}
```

Get 向指定的 URL 发出 GET。如果响应是以下重定向代码之一，则 Get 在调用 Client 的 CheckRedirect 函数后跟随重定向：

```go
//    301 (Moved Permanently)
//    302 (Found)
//    303 (See Other)
//    307 (Temporary Redirect)
//    308 (Permanent Redirect)
```

```go
func (r *Request) ParseForm() error {
	var err error
	if r.PostForm == nil {
		if r.Method == "POST" || r.Method == "PUT" || r.Method == "PATCH" {
			r.PostForm, err = parsePostForm(r
。。。。。。

//ParseForm 填充 r.Form 和 r.PostForm。对于所有请求，ParseForm 解析来自 URL 的原始查询并更新 r.Form。对于 POST、PUT 和 PATCH 请求，它还读取请求正文，将其解析为表单并将结果放入 r.PostForm 和 r.Form。请求正文参数优先于 r.Form 中的 URL 查询字符串值。如果请求正文的大小尚未受 MaxBytesReader 限制，则大小上限为 10MB。对于其他 HTTP 方法，或者当 Content-Type 不是 applicationx-www-form-urlencoded 时，不读取请求正文，并且将 r.PostForm 初始化为非 nil 的空值。 ParseMultipartForm 自动调用 ParseForm。 ParseForm 是幂等的。 
```

> server 的完整代码

```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func HelloHandler(w http.ResponseWriter,r *http.Request) {
	r.ParseForm()
	fmt.Println(r.Form)
	//遍历打印解析结果
	for key, value := range r.Form {
		fmt.Println(key, value)

	}
	w.Write([]byte("hello,world"))
}
func main(){

	http.HandleFunc("/",HelloHandler)

   err:=http.ListenAndServe("127.0.0.1:8080",nil)
// 当handler为nil时，服务端调用http.DefaultServeMux进行处理
	if err != nil {
		log.Fatal("ListenAndServe",err)
	}
}

```



```bash
D:\workspace\program\http\server>.\server.exe
map[age:[24] class:[1411] name:[lisa]]
age [24]
class [1411]
name [lisa]

```

> client 完整代码

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
)

func main() {
	//使用Get方法获取服务器响应包数据
	resp, err := http.Get("http://localhost:8080/hello?name=weidaidai&age=24&complany=huaguoshan")

	if err != nil {
		fmt.Println(err)
	}
	defer resp.Body.Close()
	// 获取服务器端读到的数据
	fmt.Println("Status = ", resp.Status)         // 状态
	fmt.Println("StatusCode = ", resp.StatusCode) // 状态码
	fmt.Println("Header = ", resp.Header)         // 响应头部
	fmt.Println("Body = ", resp.Body)             // 响应包体
	//读取body内的内容
	content, err := ioutil.ReadAll(resp.Body)

	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(string(content))

}

```

```go

packeage:  ioutil. ReadAll

// ReadAll从r读取，直到出现错误或EOF，并返回它读取的数据。
//成功调用返回err == nil，而不是err == EOF。因为ReadAll
//定义为从src读取EOF，它不处理从read读取的EOF
//作为一个错误报告。
//
//从Go 1.16开始，这个函数简单地调用io.ReadAll。
func ReadAll(r io.Reader) ([]byte, error) {
	return io.ReadAll(r)
}

```



```bash
#编译后执行
D:\workspace\program\http\client>.\client.exe

Status =  200 OK
StatusCode =  200
Header =  map[Content-Length:[11] Content-Type:[text/plain; charset=utf-8] Date:[Thu, 09 Dec 2021 06:12:10 GMT]]
Body =  &{0xc0000583c0 {0 0} false <nil> 0xfa2f40 0xfa2ec0}
hello,world
```

defer resp.body.close()

程序在使用完response后必须关闭回复的主体

post 

```go
func Post(url, contentType string, body io.Reader) (resp *Response, err error) {
   return DefaultClient.Post(url, contentType, body)
}
```



>自定义Server

要管理服务端的行为，可以创建一个自定义的Server：

```go
s := &http.Server{
	Addr:           ":8080",
	Handler:        myHandler,
	ReadTimeout:    10 * time.Second,
	WriteTimeout:   10 * time.Second,
	MaxHeaderBytes: 1 << 20,
}
log.Fatal(s.ListenAndServe())
```

r.URL.Rawquery(？后的参数)

r.URL.Host（client地址）

r.URL.Path（/ 路径)

```go
回调函数
```



















