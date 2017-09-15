### Python链表

链表的生成

```

# -*- coding:utf8 -*-
#/usr/bin/env python

class  Node(object):
    def __init__(self, data, pnext = None):
        self.data = data
        self._next = pnext

    def __repr__(self):
        return str(self.data)

class  Chinatable(object):
    def __init__(self):
        self.head = None
        self.length = 0

    def  isEmpty(self):
        return (self.length == 0)

    def append(self, NodeOrData):

        item = None  # 总是忘记
        if isinstance(NodeOrData, Node): #判断是不是节点
            item = NodeOrData
        else:
            item = Node(NodeOrData)

        if not self.head:    #这才算是插入
            self.head = item  
            self.length += 1
        else:
            node = self.head
            while node._next:
                node = node._next
            node._next = item
            self.length += 1  #长度要一直进行更新
    
    def delete(self, index):
        if self.isEmpty():    #空
            print "empty linktable"
            return
        if index < 0 or index >= self.length: #超出
            print "wrong index"
        if index == 0:    #首部
            node = self.head._next
            self.head = node
            length -= 1
            return

        j = 0 #因为没有前即对象，只有后记对象，所以要进行前即的保留
        node = self.head
        pernode = self.head
        while j<index:
            node = node._next
            pernode = node
            j += 1
        if j == index:
            pernode._next = node._next
            self.length += 1


    def insert(self, index, dataOrNode):
        if self.isEmpty(): #是否为空
            print "It is a empty linklist"
            return
        
        if index < 0 or index >= self.length: #检查条件
            print "Error: out of index"
            return
        
        item = None #造点
        if isinstance(dataOrNode, Node):
            item = dataOrNode
        else:
            item = Node(dataOrNode)

        if index == 0: #插点
            item._next = self.head
            self.head = item
            self.length += 1
            return

        j = 0 #找点
        node = self.head
        prenode = self.head
        while node._next and j < index:
            prenode = node
            node = node._next
            j += 1

        if j == index: #插点
            prenode._next = item
            item._next = node
            self.length += 1

    def update(self, index, data):
        if index<0 or index>self.length or self.isEmpty():
             print "out of index or this is a empty linktable"
             return
        j = 0
        node = self.head
        while node._next and j < index :
            node = node._next
            j += 1
        if j == index:
            node.data = data

    def clear(self):
        self.head = None
        self.length = 0

    def __len__(self):
        return self.length

    def __getitem__(self, ind):
        if self.isEmpty() or ind>self.length or ind<0:
            print "Out of index"
            return
        return self.getItem(ind)
    
    def  __setitem__(self,ind, data):
        if self.isEmpty() or ind < 0 or ind >= self.length:
            print "error: out of index"
            return
        return self.update(ind, data)


    def __repr__(self):
        if self.isEmpty():
            return "emputy linktable"
        node = self.head
        nlist = ''
        while node:
            nlist += str(node.data)+" ->"
            node = node._next
        return nlist

    def getItem(self, index):
        if self.isEmpty() or index < 0 or index >= self.length:
            print "error: out of index"
            return
        j = 0
        node = self.head
        while node._next and j < index:
            node = node._next
            j += 1

        return node.data
```

反转链表

```
# 非递归
class Solution:
    # 返回ListNode
    def ReverseList(self, pHead):
        # write code here
        if not pHead or not pHead.next:
            return pHead
        pre = None
        while pHead:
            next = pHead.next
            pHead.next = pre
            pre = pHead
            pHead = next
            
        return pre

# 递归

    def reverse(head):
        if not head or not head.next:
            return head

        newhead = reverse(head.next)  #不停的往里面传下一个节点  直到最后一个为空，然后返回倒数第二个，然后反转

        head.next.next = head  #然后开始反转 反转，为了加上反转条件 要设置为空
        head.next = None

        return newhead

```