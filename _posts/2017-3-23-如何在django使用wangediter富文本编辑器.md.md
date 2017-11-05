---
layout: post
title: wangEditer in django
description: how to use wangEditer in django
keywords: wangEditer
---

###### 今天试试来做富文本编辑器，用的是一个比较接地气的wangEditer编辑器，因为之前项目中使用的是百度的Uediter,比较大，也比较臃肿，当数据量大的时候就会出现相应文本无法加载的问题，所以要换一个轻便点的富文本编辑器，之前没有接触过，现在试试看

### 以下是我的 *解题思路*

只有在配置了server端之后才会出现上传图片的logo，要明白的是这个编辑器应该是 *我的理解* 就是把文本与格式变成了html格式的文本，然后存储起来，显示的时候也是按照html来显示，图片的显示也是在 < img >中的src来实现调用，这个是需要明白的，然后就是把图片上传到对应的位置
***
要把编辑器里的值放入隐藏的input中,这样的话就可以使用form提交，数据提交还是要自己来做的，
现在基本的文字提交还是可可以实现的的，但是加上图片的话还是会有一些问题，设置的话其实就是按照文档来就可以了

***
文档给了一段后台代码，但是具体是怎么从文档流中取出图片还是不太清楚

开始以为需要后段地址，以为需要的是文件夹的位置，后来发现富文本编辑器图片的上传与其他的上传是分开的，并不是并列一起上传的

****
他把图片传上去 然后再返回一个图片的地址，编辑器里还要放图片的地址，应该是这么一回事，完全正确，我的想法完全正确，

***

***
##### 以上是我的解决问题的思路，现在思路清晰了，把自己的思路跟大家分享一下

首先先明确一下，如果想做富文本编辑器的话， **那就要先明白其中的原理**，要不就是食屎了也觉不出味道臭来，其实原理是很简单的，就是我们用富文本编辑器把图片上传到服务器上的某个文件夹中，然后服务器端返回这个图片的地址到富文本编辑器中，最后富文本编辑其中保存的是当前文本的html文本，比如
![look](https://raw.githubusercontent.com/yuzp1996/yuzp1996.github.io/master/websites/look.png)

其实在富文本编辑器中是这种效果

![real](https://raw.githubusercontent.com/yuzp1996/yuzp1996.github.io/master/websites/real.png)



我们在使用这个编辑器之前是需要自己能实现基本的后端图片保存技术，官网上给出的python的代码是基于flask框架的，基于django框架的我在网上没有找到，现在把sever端的代码粘出来

    def imgupload(request):

    try:
        ret=str(uuid.uuid1())
        rollPictureName="webStatic/upload/"+ret+".jpg"
        rollPicturePath=os.path.join(basePath,rollPictureName)
        file = request.FILES["myFileName"]
        if file == None:
            result = r"error|未成功获取文件，上传失败"
            return HttpResponse(result)
        else:
            img = Image.open(file)
            img.thumbnail((500,500),Image.ANTIALIAS)#对图片进行等比缩放
              img.save(rollPicturePath,"png")#保存图片
             return HttpResponse("/webStatic/upload/" +ret+".jpg")
    except Exception as err:
        print err



另外 这是前端的代码

    <script type="text/javascript">

        var editor = new wangEditor('div1');
        // 上传图片（举例）
        editor.config.uploadImgUrl = '/Seal/imgupload';

        editor.config.uploadImgFileName = 'myFileName'


        // 隐藏掉插入网络图片功能。该配置，只有在你正确配置了图片上传功能之后才可用。
        editor.config.hideLinkImg = true;
        editor.create();
    </script>


关于参数其中有好多东西在官网上都有具体的解释，我在这里就不解释了，我说一个自己一直没有搞清楚的问题，那就是


>         // 上传图片（举例）  
        editor.config.uploadImgUrl = '/Seal/imgupload';



我开始一直没有弄明白这里 *editor.config.uploadImgUrl* 这个地址是什么，一直以为是放置图片的文件夹而已，后来通过排查终于弄明白了， **这里需要的是一个上传图片的后台代码的URL地址**，也就是我们看到的server端的URL地址,在编辑器中上传图片时会把相应的图片传到imgupload函数中，然后它负责把图片放到文件夹中，再返回一个这个图片的URL地址给编辑器，编辑器再根据这地址在编辑器框中显现出来， *还好我之前负责图片上传工作*，这个基本上就是富文本编辑器上传图片的原理，并不是我之前想的把所有的文本一起提交，然后从这个文本中提取出图片文件，再进行存储。

这里是html的代码：


   		<div>
                    <b style="float:left;">商品介绍:</b>
                        <div id="div1" style="width:170%;float:left;margin-left:5px;">
                        </div>
                        <input type="hidden" name="materialinfo" id="materialinfo">

                </div>



这里有个hidden的input标签，这里需要说明的是，其实富文本编辑器就是一个把我们的文本输入转换成浏览器可以认出的html文本而已，最终我们是要把富文本编辑器的内容放到后台数据库中存储起来

>  a=$("input[name='materialinfo']").val(editor.$txt.html());

这里是在Chrome上的log输出，成功的话就会是这个样子，从这个log输出就可以看出wangEditer的图片上传的工作模式

![real](https://raw.githubusercontent.com/yuzp1996/yuzp1996.github.io/master/websites/pro.png)




这里把富文本编辑器中的内容放到那个隐藏域中，最终可以传到数据库中，需要的时候提取出来放到指定的位置即可



欢迎大家参考，有问题请联系我 yuzp1996@qq.com
