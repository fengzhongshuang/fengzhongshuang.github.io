---
title: "JQuery——each函数"
categories: ["JQuery"]
tags: ["JQuery"]
---

## JQuery——each函数
---
本文用于记录JQuery的each函数的定义与用法，使用each可以对筛选出的结果进行遍历，对每个对象执行指定的函数，处理各自的逻辑。

#### 定义
each() 方法对筛选出来的每个结果指定特定的执行函数。
```javascript
$(selector).each(function(index,element));
```
`function(index,element)`中的`index`参数指元素的索引，`element`参数指元素对象，若未传递，则默认为this。

#### 实例
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>JQury each</title>
  </head>
  <body>
    <div>1</div>
    <div>2</div>
    <div>3</div>
    <div>4</div>
    <div>5</div>

    <script type="text/javascript" src="http://apps.bdimg.com/libs/jquery/1.9.0/jquery.min.js"></script>
    <script type="text/javascript">
    $(function(){
      $('div').each(function(i, item){
        if( i%2 == 0 ){
          $(item).css('background', '#ff0000');
        }else{
          $(item).css('background', '#00ff00');
        }
      });
    });
    </script>
  </body>
</html>
```
![盒子模型](/images/posts/jquery-each-1.png)
