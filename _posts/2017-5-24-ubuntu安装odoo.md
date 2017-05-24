ubuntu安装
参考网站http://blog.itpub.net/9697/viewspace-2127129/
安装node.js的教程网站https://www.howtoing.com/install-latest-nodejs-npm-on-ubuntu/

安装psycopg2提示错误然后
sudo apt-get install libpq-dev python-dev

因为odoo是Python2的但是pip装的可能是python3的，所以会报错的，pip安装的时候应该在pip的后面加上一个2，pip2 install -r requments.txt之类的

安装lxml出错因为
sudo apt-get install libxml2-dev libxslt1-dev
sudo pip install lxml

安装PIL时用一下语句
sudo apt-get install python-imaging

pip install python-pychart 