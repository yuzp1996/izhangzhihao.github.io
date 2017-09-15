## Python数据结构总结

***
###set篇
python的set和其他语言类似, 是一个 **无序不重复元素集** , 基本功能包括关系测试和消除重复元素. 集合对象还支持union(联合), intersection(交), difference(差)和sysmmetric difference(对称差集)等数学运算.  
  
sets 支持 x in set, len(set),和 for x in set。作为一个无序的集合，sets不记录元素位置或者插入点。因此，sets不支持 indexing, slicing, 或其它类序列（sequence-like）的操作。  
  
集合用于包含一组无序的对象。要创建集合，可使用set()函数并像下面这样提供一系列的项：  
       
	s = set([3,5,9,10])      #创建一个数值集合  
  
	t = set("Hello")         #创建一个唯一字符的集合  
  
   
  
与列表和元组不同，集合是无序的，也无法通过数字进行索引。此外，集合中的元素不能重复。例如，如果检查前面代码中t集合的值，结果会是：  
  
   
  
	>>> t  
  
	set(['H', 'e', 'l', 'o'])  
  
   
  
注意只出现了一个'l'。	   

### 方法
	len(s)  
set 的长度  
  
	x in s  
测试 x 是否是 s 的成员  
  
	x not in s  
测试 x 是否不是 s 的成员  
  
	s.issubset(t)  
	s <= t  
测试是否 s 中的每一个元素都在 t 中  
  
	s.issuperset(t)  
	s >= t  
测试是否 t 中的每一个元素都在 s 中  
  
	s.union(t)  
	s | t  
返回一个新的 set 包含 s 和 t 中的每一个元素  
  
	s.intersection(t)  
	s & t  
返回一个新的 set 包含 s 和 t 中的公共元素  
  
	s.difference(t)  
	s - t  
返回一个新的 set 包含 s 中有但是 t 中没有的元素  
  
		s.symmetric_difference(t)  
	s ^ t  
返回一个新的 set 包含 s 和 t 中不重复的元素  
  
	s.copy()  
返回 set “s”的一个浅复制  





下面来点简单的小例子说明。  



	>>>x = set('spam')  
	>>>y = set(['h','a','m'])  
	>>>x, y  
	>>>(set(['a', 'p', 's', 'm']), set(['a', 'h', 'm']))  


  
再来些小应用。  

	>>> x & y # 交集  
	set(['a', 'm'])  
  
	>>> x | y # 并集  
	set(['a', 'p', 's', 'h', 'm'])  
  
	>>> x - y # 差集  
	set(['p', 's'])  

####去除海量列表里重复元素

    >>> a = [11,22,33,44,11,22]  
	>>> b = set(a)  
	>>> b  
	set([33, 11, 44, 22])  
	>>> c = [i for i in b]  # c=list(b)即可
	>>> c  
	[33, 11, 44, 22] 
[Python集合（set）类型的操作](http://blog.csdn.net/business122/article/details/7541486)
***

### list篇


序列是Python中最基本的数据结构。序列中的每个元素都分配一个数字 - 它的位置，或索引，第一个索引是0，第二个索引是1，依此类推。

Python有6个序列的内置类型，但最常见的是列表和元组。

序列都可以进行的操作包括索引，切片，加，乘，检查成员。

此外，Python已经内置确定序列的长度以及确定最大和最小的元素的方法。

列表是最常用的Python数据类型，它可以作为一个方括号内的逗号分隔值出现。

列表的数据项不需要具有相同的类型

创建一个列表，只要把逗号分隔的不同的数据项使用方括号括起来即可。如下所示：

	list1 = ['physics', 'chemistry', 1997, 2000];
	list2 = [1, 2, 3, 4, 5 ];
	list3 = ["a", "b", "c", "d"];
与字符串的索引一样，列表索引从0开始。列表可以进行截取、组合等。

更新列表
你可以对列表的数据项进行修改或更新，你也可以使用append()方法来添加列表项，如下所示：

	!/usr/bin/python

	list = ['physics', 'chemistry', 1997, 2000];

	print "Value available at index 2 : "
	print list[2];
	list[2] = 2001;
	print "New value available at index 2 : "
	print list[2];

可以使用 del 语句来删除列表的的元素

	list1 = ['physics', 'chemistry', 1997, 2000];

	print list1;
	del list1[2];

###方法
*	cmp(list1, list2)
比较两个列表的元素

*	len(list)
列表元素个数

*	max(list)
返回列表元素最大值

*	min(list)
返回列表元素最小值

*	list(seq)
将元组转换为列表

*	list.append(obj)
在列表末尾添加新的对象

*	list.count(obj)
统计某个元素在列表中出现的次数

*	list.extend(seq)
在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表）

*	list.index(obj)
从列表中找出某个值第一个匹配项的索引位置

*	list.insert(index, obj)
将对象插入列表

*	list.pop(obj=list[-1])
移除列表中的一个元素（默认最后一个元素），并且返回该元素的值

*	list.remove(obj)
移除列表中某个值的第一个匹配项

*	list.reverse()
反向列表中元素

*	list.sort([func])
对原列表进行排序


***
dictionary篇

####Python 字典(Dictionary)

字典是另一种可变容器模型，且可存储任意类型对象。

字典的每个键值(key=>value)对用冒号(:)分割，每个对之间用逗号(,)分割，整个字典包括在花括号({})中 ,格式如下所示：

	d = {key1 : value1, key2 : value2 }

键必须是唯一的，但值则不必。

值可以取任何数据类型，但键必须是不可变的，如字符串，数字或元组。

一个简单的字典实例：

	ict = {'Alice': '2341', 'Beth': '9102', 'Cecil': '3258'}

也可如此创建字典：

	dict1 = { 'abc': 456 };

	dict2 = { 'abc': 123, 98.6: 37 };

####实例

	dict = {'Name': 'Zara', 'Age': 7, 'Class': 'First'};
 
	print "dict['Name']: ", dict['Name'];
	print "dict['Age']: ", dict['Age'];


	del dict['Name']; # 删除键是'Name'的条目
	dict.clear();     # 清空词典所有条目
	el dict ;        # 删除词典

####注意

两个重要的点需要记住：

1）不允许同一个键出现两次。创建时如果同一个键被赋值两次，后一个值会被记住，

2）键必须不可变，所以可以用数字，字符串或元组充当，所以用列表就不行

#### 字典内置函数&方法

*	cmp(dict1, dict2)
比较两个字典元素。

*	len(dict)
计算字典元素个数，即键的总数。

*	str(dict)
输出字典可打印的字符串表示。

*	type(variable)
返回输入的变量类型，如果变量是字典就返回字典类型。


*	dict.clear()
删除字典内所有元素

*	dict.copy()
返回一个字典的浅复制

*	dict.fromkeys(seq[, val]))
创建一个新字典，以序列 seq 中元素做字典的键，val 为字典所有键对应的初始值

*	dict.get(key, default=None)
返回指定键的值，如果值不在字典中返回default值

*	dict.has_key(key)
如果键在字典dict里返回true，否则返回false

*	dict.items()
以列表返回可遍历的(键, 值) 元组数组

*	dict.keys()
以列表返回一个字典所有的键

*	dict.setdefault(key, default=None)
和get()类似, 但如果键不存在于字典中，将会添加键并将值设为default

*	dict.update(dict2)
把字典dict2的键/值对更新到dict里

*	dict.values()
以列表返回字典中的所有值

*	pop(key[,default])
删除字典给定键 key 所对应的值，返回值为被删除的值。key值必须给出。 否则，返回default值。

*	popitem()
随机返回并删除字典中的一对键和值。






     -*- coding: utf-8 -*-
   	__author__ = 'wwh'

     一个可迭代的对象，获取其中指定的元素

	#求平均值函数
	def avg(mid):
    sum = 0
    for i in range(len(mid)):
        print(i,mid[i])
        sum += mid[i]
    return sum/(i+1)

#去除首尾元素求平均值
def drop_first_last(grades):
    # *号是可变参数,表示一个元组
    # **号也是可变参数,表示键值对
    del grades[len(grades)-1]
    del grades[0]
    return avg(grades)

if __name__ == '__main__':
    #任意可迭代的元素都可以分解为单独的变量
    t = (1,2,3)
    x,y,z = t
    print(x,y,z)
    s = 'hello'
    a,b,c,d,e = s
    print(a,b,c,d,e)
    #给出一组成绩，求其平均值
    student_grade = [88,29,100,65,78,66]
    print(drop_first_last(student_grade))