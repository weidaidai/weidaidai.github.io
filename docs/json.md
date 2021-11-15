## JSON

## 用途

**JSON 是存储和传输数据的格式。**

**JSON 经常在数据从服务器发送到网页时使用。**

前后端分离的时候交互数据用的

动态网页需要获取数据库时，服务器从数据库查找数据，然后将数据转换为json格式

## JSON 语法规则

- 数据是名称/值对

- 数据由逗号分隔

- 花括号保存对象

- 方括号保存数组

  ## json使用
  
  ```json
  "key":"value",
  //对象
  "key1":{"晓彤"：你xx"，"冲哈哈"},
  //数组：
  "key2":[123,"json"],
  //数组有对象
  "key3":[{"刘浩然":"李光明",
            "key":"value",
          },
          {"对象1":"对象2"},
          {"1":2}],
  //查询
   key3[0].刘浩然
  //空格或者换行用转义字符/n或者/r
  ```
  
  访问json对象
  
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <h2>访问json对象值</h2>
  <body>
  <div>
    姓名:<span id="x1"></span><br>
    地点:<span id="x2"></span><br>
  </div>
  
  <script>
   var test={
     "姓名":"小黄",
     "姓名2":"小红",
     "地点":"桂林"
   };
   document.getElementById("x1").innerHTML=test.姓名2;
   document.getElementById("x2").innerHTML=test.地点;
  </script>
  </body>
  ```
  
  ![](\images\test6.png)
  
  ## JSON.stringify()
  
  转化json格式 

```javascript
<script type="text/javascript">
    // type="text/javascript"指定类型不指定默认js格式
//编写对象
var user ={
    name:"小明",
    age:3,
    sex:"men"
};
console.log(user);
//转化json格式
var str=JSON.stringify(user);
console.log(str)
//{"name":"小明","age":3,"sex":"menbir"}
</script>
```

## XMLHttpRequest() 从服务器获取数据

```javascript

var xmlhttp = new XMLHttpRequest();
xmlhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
        var myObj = JSON.parse(this.responseText);
        document.getElementById("demo").innerHTML = myObj.name;
    }
};
xmlhttp.open("GET", "/src/js/json_demo.txt", true);
xmlhttp.send();
```

## JSON.parse() 解析数据

## java script 中可以用json格式来修改查询

访问使用括号表示法访问 

```javascript
var peolpe,x
    peolpe={xiao:"小米",h:"华为"};
    javascript方式    
    x=peolpe.h
    //华为
    json方式
    x=peolpe["h"];
    //华为
```

## YAML

### 基本语法

- 大小写敏感
- 使用缩进表示层级关系
- 缩进不允许使用tab，只允许空格
- 缩进的空格数不重要，只要相同层级的元素左对齐即可
- '#'表示注释

YAML 支持以下几种数据类型：

- 对象：键值对的集合，又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）
- 数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）
- 纯量（scalars）：单个的、不可再分的值

### 基本使用

```yaml
#对象使用
key:
  child-key: value
  child-key2: value2

  #复杂的对象格式

  ?
    -complexkey1
    -complexkey2
  :
    -complexvalue1
    -complexvalue2

#yaml数组结构-开头
-
 -a
 -b
 -c
#复杂数组
complexarr:
  -
    id: 1
    name: "小明"
    sex: '男'
  -
    id: 2
    name: "小花"
    sex: "女"
    
#流体方式表示
complexarr: [{ id: 1,name: "小明",sex: '男'},{id: 2,name: "小花", sex: "女"}]
```

```yaml
#复杂类型
languages:
  - Ruby
  - Perl
  - Python
websites:
  YAML: yaml.org
  Ruby: ruby-lang.org
  Python: python.org
  Perl: use.perl.org

#转json格式
  {
    "languages": [
        "Ruby",
        "Perl",
        "Python"
    ],
    "websites": {
      "YAML": "yaml.org",
      "Ruby": "ruby-lang.org",
      "Python": "python.org",
      "Perl": "use.perl.org"
    }
  }
  
```

### 引用

**&** 锚点和 ***** 别名，可以用来引用:

```yaml
defaults: &defaults
  adapter:  postgres
  host:     localhost
test:
  database: myapp_test
  <<: *defaults
#相当于
test:
  database: myapp_test
  adapter:  postgres
  host:     localhost
```

### 纯量

纯量是最基本的，不可再分的值，包括：

- 字符串
- 布尔值
- 整数
- 浮点数
- Null
- 时间
- 日期

使用一个例子来快速了解纯量的基本使用：

```yaml
boolean: 
    - TRUE  #true,True都可以
    - FALSE  #false，False都可以
float:
    - 3.14
    - 6.8523015e+5  #可以使用科学计数法
int:
    - 123
    - 0b1010_0111_0100_1010_1110    #二进制表示
null:
    nodeName: 'node'
    parent: ~  #使用~表示null
string:
    - 哈哈
    - 'Hello world'  #可以使用双引号或者单引号包裹特殊字符
    - newline
      newline2    #字符串可以拆成多行，每一行会被转化成一个空格
date:
    - 2018-02-17    #日期必须使用ISO 8601格式，即yyyy-MM-dd
datetime: 
    -  2018-02-17T15:02:31+08:00    #时间使用ISO 8601格式，时间和日期之间使用T连接，最后使用+代表时区
```