### "动态"css
Less编译器，是用Javascrip编写的

### Lessc and the command-line

```
npm install less -g(安装)
lessc style.css > style.css (编译)
```



 ### @ symbol. 变量(less的变量其实是常量)

 ### mixin

 ```
 .Round{
    -webkit-border-radius:9999px;
       -moz-border-radius:9999px;
            border-radius:9999px;   
}
 
#shape1{
    .Round;
}
```

### 颜色函数

```
darken() and lighten(), which add some black or white,
saturate() and desaturate(), to make a color more “colorful” or more “grayscale”,
fadein() and fadeout(), to increase or reduce transparency,
and spin(), which modifies the hue of the color.
```


### 群组选择器简洁写法

```
.shape{
    &:hover{ background:@lightRed   }
}
```

```
/**********************
       CONSTANTS
***********************/
 
@lightRed:   #fdd;
@lightGreen: #dfd;
@lightBlue:  #ddf;
@defaultShapesWidth:200px;
@defaultRadius:30px;
 
/*********************************
   OPERATIONS & COLOR FUNCTIONS
**********************************/
 
@darkBlue: @lightBlue - #555;
 
@defaultThemeColor:@lightBlue;
//@defaultThemeColor:spin(@lightBlue, 100);
 
@borderSize:@defaultShapesWidth * 0.1;
@borderColor:@defaultThemeColor - #222; 
//@borderColor:darken(desaturate(@defaultThemeColor, 100%), 20%); 
 
 
/**********************
        MIXINS
***********************/
 
.RoundedShape(@radius:@defaultRadius){
    -webkit-border-radius:@radius;
       -moz-border-radius:@radius;
            border-radius:@radius;  
}
 
.Round{
    .RoundedShape(9999px);
}
 
.RoundedSquare(@radius:@defaultRadius){
    .RoundedShape(@radius);
}
 
/**********************
         STYLES
***********************/
 
.shape{
    display:inline-block;
    width:@defaultShapesWidth;
    height:200px;
    background:@defaultThemeColor;
    margin:20px;
    border:@borderSize solid @borderColor;
}
 
.shape{
    &:hover{ background:@lightRed   }
}
 
#shape1{ .Round }
#shape2{ .RoundedSquare }
#shape3{ .RoundedSquare(60px) }
```

注意几点:
+ 注意，mixin和嵌套的区别在于嵌套必须要带大括号的。
+ （fuck:defaultFuck）后面的冒号之后的是默认值
