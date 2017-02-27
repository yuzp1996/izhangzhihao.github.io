##Django 曾经支持 Markdown，但在 1.6 版本中去掉了。虽然有第三方开发的如 Django-Markdown 等插件支持，但其实最简单的办法是自己直接做一个。同时也更便于测试、调试。

  目前 Python 的 Markdown 库有两个，Markdown 和 Markdown2。其中 Markdown2 宣称其性能要高于 Markdown。在网站应用中，性能是很重要的参数。

先下载安装markdown2 

1. 创建一个新的 Template Tag

在 myproject/myapp 目录下，创建子目录 templagetags，内部添加如下两个文件：__init__.py, djangomarkdown.py

编辑 djangomarkdown.py

## -*- coding: utf-8 -*-

import markdown2

from django import template
from django.template.defaultfilters import stringfilter
from django.utils.encoding import force_unicode
from django.utils.safestring import mark_safe

register = template.Library()

@register.filter(is_safe=True)
@stringfilter
def djangomarkdown(value):
    return mark_safe(markdown2.markdown(force_unicode(value),
                                        extras=["code-friendly"]
                                        )
                     )

如此，一个新的 Templage Tag "djangomarkdown" 已被创建。
2. 在 Template 中使用

很简单：

/{% load djangomarkdown /%}
/{{ value|djangomarkdown /}

3. 参考文档

http://www.dominicrodger.com/django-markdown.html
