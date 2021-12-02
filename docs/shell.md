shell



bash脚本

echo 命令可打印输出，和搜索查询

## 变量

声明变量

```bash

var1=xiaoming
echo $var1
#xiaoming

echo ?.text #查找text类型的文件

echo ${a..c}
#a b c

#循环
$  for i in {1..4}; do echo $i; done
1
2
3
4
```

> 删除变量

unset + varname

也可以 varname=   #不设置值就行

> export

夫shell和子shell之间互通

```bash
# 输出变量 $foo
$ export foo=bar

# 新建子 Shell
$ bash

# 读取 $foo
$ echo $foo
bar

# 修改继承的变量
$ foo=baz

# 退出子 Shell
$ exit

# 读取 $foo
$ echo $foo
bar
#父类不会受到子类影响
```

> declare

也是声明的一种 

语法结构

> declare OPTION VARIABLE=value

`declare`命令的主要参数（OPTION）如下。

- `-a`：声明数组变量。
- `-f`：输出所有函数定义。
- `-F`：输出所有函数名。
- `-i`：声明整数变量。
- `-l`：声明变量为小写字母。
- `-p`：查看变量信息。
- `-r`：声明只读变量。
- `-u`：声明变量为大写字母。
- `-x`：该变量输出为环境变量。

```bash
$ declare -i val=2 val=3 #重复声明相当于重新赋值
$ declare -i result
$ result=val*val
$ echo $result
9
```

> readonly

只读变量类似 declare -r

> let 

可直接运算

 ```bash
$ let k=4+5
$ echo $k
9
$ let k = 4 + 5
-bash: let: =: syntax error: operand expected (error token is "=") #tab有影响要加双引号
$ let "k = 4 + 5"
$ echo $k
 ```

## 字符串

语法为

${varname}

```bash
#获取长度
${#varname}


$ varname=只有坏掉的东西会停下来
#截取字符串从0开始
$ echo ${varname:5:5}
东西会停下
```

## 算术表达式 

`((...))`语法可以进行整数的算术运算。

```bash
$ ((j=5-4))
weidongqi@DESKTOP-192H8KJ:/$ echo $j
1
```

`((...))`语法支持的算术运算符如下。

- `+`：加法
- `-`：减法
- `*`：乘法
- `/`：除法（整除）
- `%`：余数
- `**`：指数
- `++`：自增运算（前缀或后缀）
- `--`：自减运算（前缀或后缀）

```bash
#返回第一个,有先后运算
$ echo $((foo = 1 + 2, 3 * 4))
12
$ echo $foo
3
```

