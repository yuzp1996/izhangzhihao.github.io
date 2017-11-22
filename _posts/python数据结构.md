####Python队列

队列：
队列，又称为伫列（queue），是先进先出（FIFO, First-In-First-Out）的线性表。在具体应用中通常用链表或者数组来实现。队列只允许在后端（称为rear）进行插入操作，在前端（称为front）进行删除操作。

队列的操作方式和堆栈类似，唯一的区别在于队列只允许新数据在后端进行添加。
 

```
import Queue

q = Queue.Queue()

for i in range(5):
    q.put(i)

while not q.empty():
    print q.get()
输出：

0
1
2
3
4
```
举例：

```
>>> q= Queue.Queue()
>>> q.put(1)
>>> q.put(2)
>>> q.put(3)
>>> q.put(4)
>>> q.get()
1
>>> q.get()
2
>>> q.get()
3
>>> q.get()
4
>>> q.get()
```

python栈的话，用列表就可以了，很好用

```
>>> S
[1]
>>> S.pop()
1
>>> S
[]
>>> S.append(1)
>>> S.append(2)
>>> S.append(3)
>>> S.pop()
3
>>> S.pop()
2
>>> S.pop()
1
```

***
一些常见攻击

* xss(跨站脚本攻击)  在文本域中填进脚本 进行攻击 
获取jsession 冒充用户去登录

* crsf (跨站请求伪造) 把一个网页或者按钮伪装成一个网页 链接 让被攻击者点击  因为被攻击者的浏览器上有对某网站的信任（比如记录帐号密码七天）这时候点击后 用户就执行了不想去做的  被 攻击者伪装的比如转账操作等等

* sql注入攻击

brup soup