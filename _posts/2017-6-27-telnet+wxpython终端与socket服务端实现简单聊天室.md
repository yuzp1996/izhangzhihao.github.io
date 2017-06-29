## 聊天室
这次打算把梦寐以求的聊天室做出来，以后的项目也可以用的到，现在的情况是需要图形界面化，所以需要安装wxpython，在ubuntu下的安装还算可以，借鉴了一个博文[菜鸟学Python-Ubuntu中安装wxpython](http://blog.csdn.net/iam333/article/details/8168955)

其中的内核我用的是cmd模块的，所以这次要换成图形界面的,我稍微看了wxpython的东西，用来做的话应该是没有问题的，

应该要分成两个部分，一个是服务器的部分，还有一个客户段的，主要是再客户端这里需要对界面进行界面化的包装，对于服务器是没有必要的，所以在这里省了事情

登录时使用 
>telnet 123.207.153.155 5005

今天试了试python的telnet的库感觉用起来还是很不错的叫做telnetlib

借鉴的博客[像风一样的自由 ](http://blog.csdn.net/five3/article/details/8099997)
加深了对telnet的理解[Telnet是什么？](http://www.jianshu.com/p/4bf3c19aace0)
当然对于wxpython也是要需要了解的[wxPython基本框架与运行原理--App与Frame](http://www.knowsky.com/885046.html)

困扰了老子好多天的事情终于找到解决方法了，原来是自己的输入方式有些问题,因为使用的是python的telnetlib模块做的客户端，所以想输入相应命令的时候，但是在write后总是得不到正确的返回结果，好像这个命令没有传输过去，

    tn.read_until('Welcome to TestChat')
    tn.write("login admin")
     
    
    
   后来怎么也找不到问题所在，后来谷歌了下在[这里](https://stackoverflow.com/questions/37704228/telnetlib-write-doesnt-replicate-telnet-interact)
   
  应该改成
      
    tn.read_until('Welcome to TestChat')
    tn.write("login admin\r\n")
    tn.write("admin\r\n")
    tn.read_until('Welcome to here "admin"')
    tn.write('here')
  
   找得到了答案,果然stackoverflow是业界良心，而且懂英语真的很重要。
   
   原来GUI版本还是有些门道的，看了看官网上的实现，原来没有那么简单呢，要好好学习了，需要先建立一个类才行.
   
   自己还是有些激进，应该的做法是先自己作出涞一个相应的简单的GUI的程序，再和telnet进行接洽，找了一些资料，差查了查xwpython的官网，后来发现还是有些好东西的，下面市相应的代码。
   
	  import wx

	class Client(wx.App):
	    def _init_(self):
		super(Client,self).__init__()
	    def OnInit(self):
		win = wx.Frame(None,title="Let's chat bar",size=(400,400))
		bkg = wx.Panel(win)
		loadButton = wx.Button(bkg, label='send')
		loadButton.Bind(wx.EVT_BUTTON, self.OnSend)
		saveButton = wx.Button(bkg, label='cancle')
		self.filename = filename = wx.TextCtrl(bkg)
		contents = wx.TextCtrl(bkg,style=wx.TE_MULTILINE | wx.HSCROLL)
		hbox = wx.BoxSizer()
		hbox.Add(filename, proportion=1, flag=wx.EXPAND)
		hbox.Add(loadButton, proportion=0, flag=wx.LEFT, border=5)
		hbox.Add(saveButton, proportion=0, flag=wx.LEFT, border=5)

		vbox = wx.BoxSizer(wx.VERTICAL)
		vbox.Add(hbox, proportion=0, flag=wx.EXPAND | wx.ALL,border=5 )
		vbox.Add(contents, proportion=1,flag=wx.EXPAND|wx.LEFT|wx.BOTTOM|wx.RIGHT,border=5)
		bkg.SetSizer(vbox)


		win.Show()
		return True
	    def OnSend(self, event):
		print self.filename.GetValue()



	def main():

	    client = Client()
	    client.MainLoop()

	if __name__ == '__main__':

	    main()

   
   
这样可以实现在filename输入框中输入相应的字符后，点击send按钮可以在终端打印相应的字符。


最终终于是做出来了，还是参考了很多的帮助，客户端的问题还是比较多的，因为这里涉及了wxpython,telnetlib和线程的一些问题，最后借鉴了小象的帮助，[点击这里进入](http://www.10tiao.com/html/520/201607/2650719503/1.html),这是后来我发现的一个博客，没想到真的有和我一样想法的人，觉的很开心，用xwpython来做客户端的界面，但是我们的服务器端的内核都是一样的，可能都是因为借鉴的Python基础教程这本书把，哈哈。


下面是第一版

###客户端
	     

    #encoding=utf-8

	import wx
	import telnetlib
	from time import sleep
	import thread
	class Client(wx.App):
	    def _init_(self):
		super(Client,self).__init__()
	    def OnInit(self):
		self.win = win = wx.Frame(None,title="Connect",size=(400,120))
		bkg = wx.Panel(win)
		loadButton = wx.Button(bkg, label='Connect',size=(100,30))
		loadButton.Bind(wx.EVT_BUTTON, self.OnSend)
		str1 = wx.StaticText(bkg, label="IP地址")
		self.filename = filename = wx.TextCtrl(bkg)
		str2 = wx.StaticText(bkg, label="用户名")
		self.contents = contents = wx.TextCtrl(bkg)


        hbox = wx.BoxSizer()
        hbox.Add(str1,proportion=0,flag=wx.LEFT, border=5)
        hbox.Add(filename, proportion=1, flag=wx.EXPAND)
        # hbox.Add(loadButton, proportion=1, flag=wx.LEFT, border=5)

        hbox1 = wx.BoxSizer()
        hbox1.Add(str2,proportion=0,flag=wx.LEFT,border=5)
        hbox1.Add(contents, proportion=1,flag=wx.EXPAND|wx.LEFT|wx.BOTTOM|wx.RIGHT,border=5)


        vbox = wx.BoxSizer(wx.VERTICAL)
        vbox.Add(hbox, proportion=1, flag=wx.EXPAND | wx.ALL,border=5 )
        vbox.Add(hbox1,proportion=1, flag=wx.EXPAND | wx.ALL,border=5)
        vbox.Add(loadButton, proportion=0, flag=wx.LEFT, border=270)
        bkg.SetSizer(vbox)


        win.Show()
        return True
  
    def OnSend(self, event):

        import telnetlib
        '''''Telnet远程登录：Windows客户端连接Linux服务器'''

        # 连接Telnet服务器
        username = self.contents.GetValue().encode('ascii')   # 登录用户名
        Host = self.filename.GetValue().encode('ascii')
        tn.open(Host, port=5005, timeout = 10)
        tn.set_debuglevel(2)
        tn.read_until('Welcome to TestChat')
        tn.write("login "+username+"\r\n")
        self.win.Close()
        ChatFrame(None, -1, title = 'Yuzhipeng Chat Room', size = (500, 350))




	class ChatFrame(wx.Frame):
	"""
	聊天窗口
	"""

    def __init__(self, parent, id, title, size):
        '初始化，添加控件并绑定事件'
        wx.Frame.__init__(self, parent, id, title)
        self.SetSize(size)
        self.Center()
        self.chatFrame = wx.TextCtrl(self, pos = (5, 5), size = (490, 310), style = wx.TE_MULTILINE | wx.TE_READONLY)
        self.message = wx.TextCtrl(self, pos = (5, 320), size = (300, 25))
        self.sendButton = wx.Button(self, label = "Send", pos = (310, 320), size = (58, 25))
        self.usersButton = wx.Button(self, label = "Users", pos = (373, 320), size = (58, 25))
        self.closeButton = wx.Button(self, label = "Close", pos = (436, 320), size = (58, 25))
        self.sendButton.Bind(wx.EVT_BUTTON, self.send)
        self.usersButton.Bind(wx.EVT_BUTTON, self.lookUsers)
        self.closeButton.Bind(wx.EVT_BUTTON, self.close)
        self.chatFrame.SetValue("Welcome to Yuzhipeng's ChatRoom! \r\n")
        thread.start_new_thread(self.receive, ())
        self.Show()

    def send(self, event):
        '发送消息'
        message = str(self.message.GetLineText(0)).strip()
        if message != '':
            tn.write(message + '\r\n')
            self.message.Clear()
            # self.chatFrame.AppendText('me: '+message+'\n')

    def lookUsers(self, event):
        '查看当前在线用户'
        tn.write('look\n')

    def close(self, event):
        '关闭窗口'
    	tn.write('logout\n')
    	tn.close()
    	self.Close()

    def receive(self):
        '接受服务器的消息'
        while True:
        	sleep(0.1)
        	result = tn.read_very_eager()
        	if result != '':
        		self.chatFrame.AppendText(result)


	def main():

	    client = Client()
	    client.MainLoop()

	if __name__ == '__main__':
	    finish = ':~$ '      # 命令提示符
	    #123.207.153.155
	    tn = telnetlib.Telnet()
	    main()


#### 服务器端

	from asyncore import dispatcher
	from asynchat import async_chat
	import socket, asyncore
	import wx

	PORT = 5005
	NAME = 'TestChat'
	class EndSession(Exception):pass

	#this is command
	class CommandHandler:

	    def unknown(self, session, cmd):
		session.push ('Unknown command : %s\r\n' % cmd)

	    def handle(self, session, line):
		if not line.strip(): return
		parts = line.split(' ', 1)
		cmd = parts[0]

		if cmd not in [ 'login', 'look', 'logout','who','getout']:
		    meth = getattr(self, 'do_say', None)
		    meth(session, line)
		else:
		    try: line = parts[1].strip()
		    except IndexError: line = ''
		    meth = getattr(self, 'do_'+cmd, None)
		    try:
		        meth(session, line)
		    except TypeError:
		        self.unknown(session, cmd)

	# above these are kinds of rooms
	class Room(CommandHandler):

	    def __init__(self, server):
		self.server = server
		self.sessions = []

	    def add(self, session):
		self.sessions.append(session)

	    def remove(self, session):
		self.sessions.remove(session)

	    def broadcast(self, line):
		for session in self.sessions:
		     session.push(line)

	    def do_logout(self, session, line):
		raise EndSession

	class LoginRoom(Room):

	    def add(self, session):
		Room.add(self, session)
		self.broadcast("Welcome to %s\r\n"% self.server.name)

	    def unknown(self, session, cmd):
		session.push('Please log in \n Use"login <nick>"\r\n')

	    def do_login(self, session, line):
		name = line.strip()
		if not name:
		    session.push("Please Enter a Name\r\n")
		elif name in self.server.users:
		    session.push('The name "%s" is taken.\r\n'% name)
		    session.push('Please try again\t\n')
		else:
		    session.name = name
		    session.push('Welcome to here "%s".\r\n'% name)
		    session.enter(self.server.main_room)

	class ChatRoom(Room):

	    def add(self, session):
		self.broadcast(session.name + ' has enter room \r\n')
		self.server.users[session.name] = session
		Room.add(self, session)
	    def remove(self, session):
		Room. remove(self, session)
		self.broadcast(session.name + ' has leave the room \r\n')

	    def do_say(self, session, line):
		self.broadcast(session.name+': '+line+'\r\n')

	    def do_look(self, session, line):
		session.push('The foolowing are in this room :\r\n')
		for other in self.sessions:
		    session.push(other.name + '\r\n')

	    def do_who(self, session, line):
		session.push('The following are logged in :\r\n')
		for name in self.server.users:
		    session.push(name+'\r\n')

	    def do_getout(self, session, line):

		if session.name == 'admin':
		    self.broadcast(line + ' has been cleaned \r\n')
		    self.server.users[line].handle_close()
		else:
		    self.broadcast(session.name+' try to kill other people \r\n')
		    session.push('please check your account,you have no right to do it \r\n')


	class LogoutRoom(Room):
	    def add(self, session):
		try: del self.server.users[session.name]
		except KeyError:
		    pass

	#above is
	class ChatSession(async_chat):
	    def __init__(self, server, sock):
		async_chat.__init__(self, sock)
		self.server = server
		self.set_terminator("\r\n")
		self.data = []
		self.name = None
		self.enter(LoginRoom(server))

	    def enter(self, room):
		try: cur = self.room
		except AttributeError: pass
		else: cur.remove(self)
		self.room = room
		room.add(self)

	    def collect_incoming_data(self, data):
		self.data.append(data)

	    def found_terminator(self):
		line = ''.join(self.data)

		self.data = []

		try: self.room.handle(self, line)
		except EndSession:
		    self.handle_close()

	    def handle_close(self):
		async_chat.handle_close(self)
		self.enter(LogoutRoom(self.server))


	# this is the main server
	class ChatServer(dispatcher):
	    def __init__(self, port, name):
		dispatcher.__init__(self)
		self.create_socket(socket.AF_INET, socket.SOCK_STREAM)
		self.set_reuse_addr()
		self.bind(('',port))
		self.listen(5)
		self.name =name
		self.users={}
		self.main_room = ChatRoom(self)
	    def handle_accept(self):
		conn, addr=self.accept()
		print conn
		print addr
		ChatSession(self, conn)




	# you can start here
	if __name__ == '__main__':

	# init the wx box
	    # wxPythonBox();
	    s = ChatServer(PORT, NAME)
	    try: asyncore.loop()
	    except KeyboardInterrupt: print

	    


因为我把服务搭在了服务器上，所以怎么使用我还是直接上图了

![登录框](https://raw.githubusercontent.com/yuzp1996/mymaterial/master/1%E8%BF%9E%E6%8E%A5%E6%9C%8D%E5%8A%A1%E5%99%A8.png)


![对话框](https://raw.githubusercontent.com/yuzp1996/mymaterial/master/1%E5%AF%B9%E8%AF%9D%E6%A1%86.png)
    
    
    
   之后会有相应的升级更新，敬请期待
    
    
    
    