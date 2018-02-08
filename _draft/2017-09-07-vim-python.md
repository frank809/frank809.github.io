---
layout: post
title: Vim support python
---

>
    import brain
    brain.run()

在Ubuntu16.04下面编译VIM并支持python：
1. 需要python2.7-dev包，通过apt自动安装：apt install python2.7.dev
如果发现找不到包，apt search下，看看提供了python head 和lib的包是那个名字。然后安装。
2. 编译VIM的时候在src目录下打开python的支持：./configure --enable-pythoninterp=yes
3. make和make install，试权限情况决定要不要添加sudo:)
编译结束后看看python support了没:
j34liu@FrankTest:~$ vim --version | grep python
+comments          +libcall           +python            +vreplace
+conceal           +linebreak         -python3           +wildignore
Linking: gcc   -L/usr/local/lib -Wl,--as-needed -o vim        -lm -ltinfo -lnsl   -ldl    -L/usr/lib/python2.7/config-x86_64-linux-gnu -lpython2.7 -lpthread -ldl -lutil -lm -Xlinker -export-dynamic -Wl,-O1 -Wl,-Bsymbolic-functions

###### Test 测试下是在Ubuntu下面使用Pycharm提交github是不是正常的。
