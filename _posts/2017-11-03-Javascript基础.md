#### JavaScript基础

> //这是一条注释

简单数据有undefined，null，boolean，number和string这五种。复杂数据只有一种，即对象（object）   

##### 常用算数运算符
```
var x, y=5; //初始化两个变量x和y，给y赋初始值为5。{x:undefined, y:5}。

x=y+5;     //即x=5+5，结果{x:10, y:5}。

x=y-5;     //即x=5-5，结果{x:0, y:5}。

x=y/2;     //即x=5/2，结果{x:2.5, y:5}。

x=y*2;     //即x=5*2，结果{x:10, y:5}。

x=y%2;     //即x=5%2，结果{x:1, y:5}。结果是x=5/2整数求商时的余数1。

x=--y;     //--y等价于y=y-1，此后y=4，然后x=y，即x=4结果{x:4, y:4}。

x=++y;     //++y等价于y=y+1，此后y=5，然后x=y，即x=5结果{x:5, y:5}。

var x = 5;
x+=2;     //7  等价于x=x+2，即x=5+2。
x-=2;     //5  等价于x=x-2，即x=7-2。
x*=2;     //10 等价于x=x*2，即x=5*2。
x/=2;     //5  等价于x=x/2，即x=10/2。
x%=2;    //1  等价于x=x%2，即x=5%2。
```

##### 比较运算符
```
x == y  //等于 (判断x，y的值是否相等)
x === y //等于 (判断x，y的值和类型是否都相同)
x != y  //不等于 (判断x，y的值是否不相等)
x > y   //大于 (判断x是否大于y)
x < y   //小于 (判断x是否小于y)
x >= y  //大于等于 (判断x是否大于或者等于y)
x <= y  //小于等于 (判断x是否小于或者等于y)
```

##### 逻辑运算符
```
&&  ||  !
"与"，"或"，"非"
```

#####条件运算符
```
JavaScript中条件判断语句的基本语法:

condition ? expr1 : expr2;

condition实际值为true时，执行expr1。condition实际值为false时，执行expr2。
```

#####if语句
当指定条件为 true 时，该语句才会执行其内部的代码。

语法:
```
if (condition){
  //code that runs if the condition is true
}
if (answer == 27) {
  console.log('恭喜你答对了。');
}

if (condition) {
    statement1;    //当 condition 的值为 true 时，statement1 被执行。
} else {
    statement2;    //当 condition 的值为 false 时，statement2 被执行。
}

if (condition1) {
  statement1;
} else if (condition2) {
  statement2;
} else {
  statement3;
}
```

####for循环
```
for (var i=0; i<10; i++){
    console.log( i )
}

//  使用break跳出循环
for(var i = 0;i<10;i++){
    if(i==3){
        break;
    }
    console.log(i);
}
console.log("循环结束");

例如下面是一个仅当循环变量为偶数时才进行处理的for循环语句。

for( var i = 0; i < 10; i++){
    if(i % 2 != 0){
        continue;
    }
    console.log( i );
}
console.log("循环结束")
```

#####   函数
```
function 函数名(参数, 参数, ...){
    代码块
}

function sum(a, b){
    return a+b ;
}
```