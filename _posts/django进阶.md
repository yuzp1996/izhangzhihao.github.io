### 今天学了些django的东西，之前都没有接触过的

    class Creation2ProjectLabel(models.Model):
    Id = models.AutoField(primary_key=True)
    creation = models.ForeignKey(Creation,related_name='Creation2ProjectLabel_Creation_set', null=False)
    projectLabel = models.ForeignKey(ProjectLabel, related_name='Creation2ProjectLabel_ProjectLabel_set', null=False)

    def __unicode__(self):
        return unicode(self.projectLabel)

这个里面第一条注意的是

* null

设置为True时，django用Null来存储空值。日期型、时间型和数字型字段不接受空字符串。所
以设置IntegerField，DateTimeField型字段可以为空时，需要将blank，null均设为True。

* return unicode(self.projectLabel)

 返回值最好写成这个样子，要不会提示错误

* related_name这个字段

related_name是反调名称。


    class Category(models.Model): 
     name = models.CharField(max_length=16, unique=True) 
     sign_id = models.PositiveSmallIntegerField(unique=True) 


然后定义了: 

    class Node(models.Model): 
       category = models.ForeignKey(Category, related_name='nodes') 
       name = models.CharField(max_length=16, unique=True) 
       description = models.CharField(max_length=255) 
       members = models.ManyToManyField(Member, related_name='nodes', null=True) 
       topic_amount = models.IntegerField(default=0) 
       member_amount = models.IntegerField(default=0) 
打个比方，这段代码里面，category对象时，调.notes就能调到关联他的Node对象

在我们的例子中，就是相当于creation对象可以找出他在Creation2ProjectLabel里面有多少条记录，通过这个记录来找到他在这个里面对应着什么标签。


	class Creation2ProjectLabel(models.Model):
    Id = models.AutoField(primary_key=True)
    creation = models.ForeignKey(Creation,related_name='Creation2ProjectLabel_Creation_set', null=False)
    projectLabel = models.ForeignKey(ProjectLabel, related_name='Creation2ProjectLabel_ProjectLabel_set', null=False)

    def __unicode__(self):
        return unicode(self.projectLabel)


下面是对应调取代码
    
    creations = Creation2ProjectLabelObjs.creation.Creation2ProjectLabel_Creation_set.all()


这会调取出一种creation在创意标签表里的所有的标签，记住是creation这个对象来用，谁是外键谁就可以用。








