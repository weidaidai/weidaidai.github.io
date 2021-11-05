#   javaScript

##    

1. ```bash
      JavaScript的用途
   1. **HTML** 定义了网页的内容
   
   2. **CSS** 描述了网页的布局
   
   3. **JavaScript** 控制了网页的行为
   
      JavaScript 显示数据
   
      JavaScript 可以通过不同的方式来输出数据：
   
      - 使用 **window.alert()** 弹出警告框。
      - 使用 **document.write()** 方法将内容写到HTML 文档中。
      - 使用 **innerHTML** 写入到 HTML 元素。
      - 使用 **console.log()** 写入到浏览器的控制台
   ```

## 开始使用

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
     <!--注释-->
    <!--代码位置-->
    <!--script成对出现-->
    <!--HTML 中的脚本必须位于 <script> 与 </script> 标签之间
     脚本可被放置在 HTML 页面的 <body> 和 <head> 部分中。-->

    <!--代码位置-->
    <script>
        //定义变量类型=变量量值,不可转变,严格区分大小写
        // alert() 方法用于显示带有一条指定消息和一个 OK 按钮的警告框。
        //console.log(score);可以在浏览器控制台查看变量，设置断点等等
        //sources看源码
       var score =70;
       if (score>60&&score<70){
           alert("60-70之间")
       }else if (score>70&&score<80){
           alert("70-80之间")
       }else{
           alert("other")
       }
    </script>
</head>
<body>
<!--代码位置-->
</body>
</html>
```

![](images/test.png)

## 字符串string

```javascript
<script>
    //java script 6版本以上可以用   'use strict'检查模式
    //
    'use strict';
    var msg = 'hello wrold';
    //转义字符串   \  和其他语言差不多
    console.log("\u4e2d")
    console.log("\a")
    //输出字符长度
    console.log(msg.length)
    //可以输出长字符串  使用反引号··
    var kk = `hhdajdja
   djadla
    ld;ald;a`
    //输出大小写
    var xx="xx"
    console.log(xx.toUpperCase())
    console.log(xx.toLowerCase())
    //获取下标位置
    console.log(msg.indexOf("e"))
</script>
```

![](D:\workspace\weidaidai.github.io\docs\images\test2.png)

## 数组array

```javascript
<script>
    //array数组 可以存储任何数据类型,可以根据下标赋值，或者length增加长度 ，如果长度赋值元素过小会丢失arr内部元素
    var arr =[1,2,3,"myworld"]
    console.log(arr)
    console.log(arr.length)
    console.log(arr.length=9)
    console.log(arr)
    console.log(arr.length=2)
</script>
```

![](images\test3.png)

以上用浏览器控制台方式查看元素内容

可以使用 document.getElementById(*id*) 方法。

请使用 "id" 属性来标识 HTML 元素，并 innerHTML 来获取或插入元素内容：

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<p id="demo"></p>
<p id="demo1"></p>
<script>
    var x = 7;
    var y = 8;
    var z = x * y;

    document.getElementById("demo").innerHTML = z;

</script>
<script>
    var msg ="hello";
    document.getElementById("demo1").innerHTML=msg;
</script>
<body>

</body>
</html>
```

网页输出内容为 56 hello

## 对象

类似go 中map， py中的Dictionary

```javascript
 <script>
        var person={
            name : "小明",
            age : 18,
            project : "数学"
        };
        document.getElementById("demo").innerHTML=
            person.age+"岁学"+person.project;
    </script>
    <p id="demo1"></p>
    <script>
        var person = {
            firstName : "John",
            lastName  : "Doe",
            age       : 50,
            eyeColor  : "blue"
        };
        document.getElementById("demo1").innerHTML =
            person.firstName + " 现在 " + person.age + " 岁。";
    </script>

```

输出为

18岁学数学

John现在50岁