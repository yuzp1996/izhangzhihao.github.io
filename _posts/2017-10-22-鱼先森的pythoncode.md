#### 鱼先森的pythoncode

```
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


#转着圈圈的 打印出这个 矩阵

# -*- coding:utf-8 -*-
class Solution:
    # matrix类型为二维列表，需要返回列表
    def printMatrix(self, matrix):
        # write code here
        res = []
        while matrix:
            res.extend(matrix.pop(0))    #第一行拿出来
            if not matrix or not matrix[0]:
                break
            matrix = self.turn(matrix)    #旋转
        return res
    
    def turn(self,List):    #重新布置这个矩阵
        newList = []
        lenofy = len(List)
        lenofx = len(List[0])
        for i in range(lenofx):
            tmplist=[]    #每一行
            for j in range(lenofy):
                tmplist.append(List[j][i])
            newList.append(tmplist)
        newList.reverse()
        return newList

print 1, #保持不换行










输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序
。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4，5,3,2,1
是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意
：这两个序列的长度是相等的）


相当于按照出栈顺序将入栈的数字弹出来，如果能弹空则说明这个入栈序列是按照所给的弹出的序列
class Solution:
    def IsPopOrder(self, pushV, popV):
        # write code here
        stack = []
        for i in pushV:
            stack.append(i)
            while len(stack) and stack[-1] == popV[0]:  #总要有个终止条件，也就是
长度为0的时候
                stack.pop(-1)
                popV.pop(0)
        if len(stack) == 0:
            return True
        return False















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



sorted(list(set(map(''.join, itertools.permutations(ss)))))

map组成全排列的字符，set去重，list和sorted合作组成排序




字典判断值或键的存在

d={'site':'http://www.jb51.net','name':'jb51','is_good':'yes'}

d.has_key('site')

'site' in d.keys()



#反转数字
class Solution(object):
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        r = -cmp(0, x)
        p = int(`r*x`[::-1])
        return p*r*(p<2**32) 
```