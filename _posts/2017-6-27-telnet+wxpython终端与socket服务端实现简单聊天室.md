## 聊天室
这次打算把梦寐以求的聊天室做出来，以后的项目也可以用的到，现在的情况是需要图形界面化，所以需要安装wxpython，在ubuntu下的安装还算可以，借鉴了一个博文[菜鸟学Python-Ubuntu中安装wxpython](http://blog.csdn.net/iam333/article/details/8168955)

其中的内核我用的是cmd模块的，所以这次要换成图形界面的,我稍微看了wxpython的东西，用来做的话应该是没有问题的，

应该要分成两个部分，一个是服务器的部分，还有一个客户段的，主要是再客户端这里需要对界面进行界面化的包装，对于服务器是没有必要的，所以在这里省了事情

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
    
    
    
    
    