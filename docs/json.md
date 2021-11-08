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

  转化json格式 -JSON.stringify()

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

使用 XMLHttpRequest 从服务器获取数据：

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

在java script 中可以用json来修改

访问使用括号表示法访问 

```javascript
var peolpe,x
    peolpe={xiao:"小米",h:"华为"};
    //x=peolpe.h
    //华为
    x=peolpe["h"];
    //华为
```

