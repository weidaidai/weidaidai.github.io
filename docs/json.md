## json

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

