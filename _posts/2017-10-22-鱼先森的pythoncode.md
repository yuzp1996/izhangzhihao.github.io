
### 鱼先森的pythoncode


```
reverse()对列表进行反转，不能将反转结果赋值，因为这个是一个操作，只是将原来的列表进行反转


num=map(str,numbers)  #转换成字符


if not numbers:  #为空条件判断




num.sort(lambda x,y:cmp(x+y,y+x))  #函数排序

>>> P.sort(cmp = lambda b,a:a-b)
>>> P
[64, 5, 4, 3, 3, 2, 2, 1]






import time    #计算程序运行时间
start = time.clock()
elapsed = (time.clock() - start)


for (k,v) in  dict.items(): #遍历字典中的key与value

for index，text in enumerate(list): #一般情况下对一个列表或数组既要遍历索引又要遍历元素时
   print index ,text

for i in s:  #找出出现一次的字符及其位置
    if s.count(i)==1:
        return s.index(i)


[res.append(matrix[o][i]) for i in xrange(o,m-o)]  #直接在列表里循环

List.reverse() #不能赋值，只是对本身函数的反转

List+List #增长列表，+ 只能增加列表，不能增加元素



print 1, #保持不换行



chr()、unichr()和ord()

chr()函数用一个范围在range（256）内的（就是0～255）整数作参数，返回一个对应的字符。unichr()跟它一样，只不过返回的是Unicode字符

>>> chr(3)
'\x03'
>>> unichr(1)
u'\x01'

>>> ord("a")
97
>>> ord("b")
98


测试代码时间

import timeit

def test1():
    l = []
    for i in range(1000):
        l = l + [i]


t1 = timeit.Timer("test1()", "from __main__ import test1")
print(t1.timeit(number=10000),"seconds")


x = list(range(2000000))
popend = timeit.Timer("x.pop()","from __main__ import x")
popend.timeit(number=1000)









计数模块

>>> import collections
>>> T = collections.Counter("qqwwssaaqqaawwss")
>>> for i,v in T.items():
...     print i,v
...
q 4
a 4
s 4
w 4







全排列
>>> import itertools
>>> P = itertools.permutations("abc")
>>> P
<itertools.permutations object at 0x02D27390>
>>> for i in P:
...     print i
...
('a', 'b', 'c')
('a', 'c', 'b')
('b', 'a', 'c')
('b', 'c', 'a')
('c', 'a', 'b')
('c', 'b', 'a')
>>>


这个用列表表达式做的，感觉还可以
>>> sorted([i+j+k  for i in 'abc'  for j in 'abc'  for k in 'abc' if  i!=j and j!=k and i!=k])





sorted(list(set(map(''.join, itertools.permutations(ss)))))

map组成全排列的字符，set去重，list和sorted合作组成排序




字典判断值或键的存在

d={'site':'http://www.jb51.net','name':'jb51','is_good':'yes'}

d.has_key('site')

'site' in d.keys()



想要某个段的数字
>>> range(2,23)
[2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22]






>>> P = [2,3,17,5,4,3]
>>> P.sort()
>>> P
[2, 3, 3, 4, 5, 17]
>>> P.sort(reverse = True)
>>> P
[17, 5, 4, 3, 3, 2]



按照空格分隔并旋转，将列表转成字符串
L = s.split()
L.reverse()
return ' '.join(L)




>>> sum([1,2,3,4])
10






Python 字典(Dictionary) items() 函数以列表返回可遍历的(键, 值) 元组数组。

>>> import collections
>>> C = collections.Counter([22,11,22,33,3,4,5,44,44,3,3,2,1])
>>> print C
Counter({3: 3, 44: 2, 22: 2, 33: 1, 2: 1, 4: 1, 5: 1, 1: 1, 11: 1})
>>> for i,v in C.items():
...     print i,v
...
33 1
2 1
3 3
4 1
5 1
1 1
11 1
44 2
22 2






总有时候会很愚蠢的用错某些符号

List[n:m]
是冒号 不是逗号


range(n,m)
这个是逗号 不是冒号


list[::-1] 反转列表
```
