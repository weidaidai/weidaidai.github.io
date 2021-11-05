#   javaScript的学习

##    JavaScript的用途

1. **HTML** 定义了网页的内容
2. **CSS** 描述了网页的布局
3. **JavaScript** 控制了网页的行为



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

![uTools_1636092562507](D:\workspace\weidaidai.github.io\docs\images\uTools_1636092562507.png)

