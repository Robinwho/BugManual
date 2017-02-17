# BugBook
Record the major bugs encountered in the projects. So easy to find & summary in the future.
##BUG 1 [Tensorflow error: Docker on Mac]
* 1
Expected behavior

Follow the tips by the link:
https://github.com/Robinwho/tensorflow/blob/master/tensorflow/g3doc/get_started/os_setup.md#optional-setup-gpu-for-mac

After Docker is installed, launch a Docker container with the TensorFlow binary image as follows.

>$ docker run -it -p 8888:8888 gcr.io/tensorflow/tensorflow

* 2
Actual behavior

>$ docker run -it -p 8888:8888 gcr.io/tensorflow/tensorflow

* 3
Information

GET THE ERRORS:

Unable to find image 'gcr.io/tensorflow/tensorflow:latest' locally
docker: Error response from daemon: Get https://gcr.io/v1/_ping: dial < tcp 64.233.189.82:443: i/o timeout.
See 'docker run --help'. And when running:

>$ ping google.com

PING google.com (172.217.24.14): 56 data bytes

Request timeout for icmp_seq 0 ...3

Having tried many ways, but doesn't work. 

* 4
Finally with the help of follow link:
(http://blog.csdn.net/chenming_hnu/article/details/54600270), the problem is fixed!
The correct command:
>$ sudo docker run -it -p 8888:8888 tensorflow/tensorflow

###Detail
https://github.com/docker/for-mac/issues/1145

###TF官方文档中文版
http://wiki.jikexueyuan.com/project/tensorflow-zh/get_started/basic_usage.html

###Visualizing MNIST: An Exploration of Dimensionality Reduction
http://colah.github.io/posts/2014-10-Visualizing-MNIST/

##BUG2 [easy_install command not found]

>$ sudo easy_install pip
>Password:
>sudo: easy_install: command not found

Following this link(http://stackoverflow.com/questions/6012246/why-is-python-easy-install-not-working-on-my-mac), it can't work.

###Detail
>$ sudo find . -name "easy_install"
>./usr/local/Cellar/python/2.7.12_2/Frameworks/Python.framework/Versions/2.7/bin/easy_install

Then,
>$ sudo /usr/local/Cellar/python/2.7.12_2/Frameworks/Python.framework/Versions/2.7/bin/easy_install pip

Searching for pip
Best match: pip 6.0.8
Processing pip-6.0.8-py2.7.egg
pip 6.0.8 is already the active version in easy-install.pth
Installing pip script to /usr/local/Cellar/python/2.7.12_2/Frameworks/Python.framework/Versions/2.7/bin
Installing pip2.7 script to /usr/local/Cellar/python/2.7.12_2/Frameworks/Python.framework/Versions/2.7/bin
Installing pip2 script to /usr/local/Cellar/python/2.7.12_2/Frameworks/Python.framework/Versions/2.7/bin

Using /usr/local/lib/python2.7/site-packages/pip-6.0.8-py2.7.egg
Processing dependencies for pip
Finished processing dependencies for pip

###Exa: [python2.7 & 3.5 path configuration]
vim .bash_profile

<code>
  eval "$(pyenv init -)"
  export ECLIPSE_HOME=/Applications/eclipse
  # Setting PATH for Python 3.5
  # The original version is saved in .bash_profile.pysave
  #PATH="/Library/Frameworks/Python.framework/Versions/3.5/bin:${PATH}"  
  PATH="/usr/local/Cellar/python/2.7.12_2/Frameworks/Python.framework/Versions/2.7/bin/:${PATH}"  
  export PATH   
 # Setting PATH for Python 3.5 
 # The original version is saved in .bash_profile.pysave 
 #PATH="/Library/Frameworks/Python.framework/Versions/3.5/bin:${PATH}" 
 PATH="/usr/local/Cellar/python/2.7.12_2/Frameworks/Python.framework/Versions/2.7/bin/:${PATH}" 
 export PATH
</code>

-------------------------------------
PART
-------------------------------------

##BUG3 [Using Sqlite3]
e.g. On Mac:http://www.cnblogs.com/xingfuzzhd/archive/2013/11/04/3407166.html
###useful command:
>sqlite> select * from sqlite_master where type="table";


##BUG4 [Input URL: SyntaxError: invalid syntax]
Using 'http://xxxx.xxx.xxx' instead of http://xxxx.xxx.xxx. See the difference?

Or refer to this:http://stackoverflow.com/questions/2589309/command-line-input-causes-syntaxerror


##BUG5 [TypeError: 'encoding' is an invalid keyword argument for this function]
In Python 2, the open() function takes no encoding argument. The third argument is the buffering option instead.
You appear to be confused with the Python 3 version. If so use io.open() instead:
>import io
>with io.open("file1.txt", "a", encoding="utf-8-sig") as f:
In Python 3, the io.open() function replaced the version from Python 2.

##BUG6 [bs4.FeatureNotFound]
bs4.FeatureNotFound: Couldn't find a tree builder with the features you requested: lxml. Do you need to install a parser library?

###REASON:
You'll notice that in the BS4 documentation page above, they point out that by default BS4 will use the Python built-in HTML parser. Assuming you are in OSX, the Apple-bundled version of Python is 2.7.2 which is not lenient for character formatting.

###SOLUTION:
>sudo pip install lxml
If there is:
||In file included from src/lxml/lxml.etree.c:515:
    
    src/lxml/includes/etree_defs.h:14:10: fatal error: 'libxml/xmlversion.h' file not found
    #include "libxml/xmlversion.h"
             ^
    1 error generated.
To fix it(For Mac with XCODE):
>export >C_INCLUDE_PATH=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.10.sdk/usr/include/libxml2:$C>_INCLUDE_PATH


<pre>说明：MAC上的坑真不少。折腾半天，终于搞定。简要总结如下。
-首先，打开MAC的root账户：https://support.apple.com/en-in/HT204012
-其次，brew install libxml2安装libxml2
-接着，进入root用户操作（su）。
*（1）配置环境变量：如，export C_INCLUDE_PATH=/usr/local/Cellar/libxml2/2.9.4_2/include/libxml2:$C_INCLUDE_PATH。
其中版本号每个系统不一，需进入Cellar目录查看。
（2）使用 pip install lxml安装。成功完成。
</pre>


## BUG6 [>pip install scrapy & pip install --upgrade setuptools]
###Problem:
distutils.errors.DistutilsPlatformError: Microsoft Visual C++ 9.0 is require
d. Get it from http://aka.ms/vcpython27
    ----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in c:\users\hp430\ap
pdata\local\temp\pip-build-f17bsa\cffi

###SOLUTION1:
http://aka.ms/vcpython27
Then
>pip install scrapy
error: command 'C:\\Users\\HP430\\AppData\\Local\\Programs\\Common\\Microsoft\\Visual C++ for Python\\9.0\\VC\\Bin\\cl.exe' failed with exit status 2
    ----------------------------------------
<pre><code>
Command "c:\python27\python.exe -c "import setuptools, tokenize;__file__='c:\\users\\hp430\\appdata\\local\\temp\\pip-build-dugrv2\\cryptography\\setup.py';exec(compile(getattr(tokenize, 'open', open)(__file__).read().replace('\r\n', '\n'), __file__, 'exec'))" install --record c:\users\hp430\appdata\local\temp\pip-8ihq2x-record\install-record.txt --single-version-externally-managed --compile" failed with error code 1 in c:\users\hp430\appdata\local\temp\pip-build-dugrv2\cryptography
</code></pre>

###SOLUTION2:
https://cryptography.io/en/latest/installation/#on-windows

## BUG7 [libxml2]
    Building lxml version 3.7.2.
    Building without Cython.
    ERROR: 'xslt-config' 不是内部或外部命令，也不是可运行的程序
    或批处理文件。

    ** make sure the development packages of libxml2 and libxslt are installed *

Install "pywin32-220.win32-py2.7.exe" on Win7:
>>python version 2.7 required which was not found in the registry
###SOLUTION1:

>>python register.py

<pre><code>
#
# script to register Python 2.0 or later for use with win32all
# and other extensions that require Python registry settings
#
# written by Joakim Loew for Secret Labs AB / PythonWare
#
# source:
# http://www.pythonware.com/products/works/articles/regpy20.htm
#
# modified by Valentine Gogichashvili as described in http://www.mail-archive.com/distutils-sig@python.org/msg10512.html
 
import sys
 
from _winreg import *
 
# tweak as necessary
version = sys.version[:3]
installpath = sys.prefix
 
regpath = "SOFTWARE\\Python\\Pythoncore\\%s\\" % (version)
installkey = "InstallPath"
pythonkey = "PythonPath"
pythonpath = "%s;%s\\Lib\\;%s\\DLLs\\" % (
    installpath, installpath, installpath
)
 
def RegisterPy():
    try:
        reg = OpenKey(HKEY_CURRENT_USER, regpath)
    except EnvironmentError as e:
        try:
            reg = CreateKey(HKEY_CURRENT_USER, regpath)
            SetValue(reg, installkey, REG_SZ, installpath)
            SetValue(reg, pythonkey, REG_SZ, pythonpath)
            CloseKey(reg)
        except:
            print "*** Unable to register!"
            return
        print "--- Python", version, "is now registered!"
        return
    if (QueryValue(reg, installkey) == installpath and
        QueryValue(reg, pythonkey) == pythonpath):
        CloseKey(reg)
        print "=== Python", version, "is already registered!"
        return
    CloseKey(reg)
    print "*** Unable to register!"
    print "*** You probably have another Python installation!"
 
if __name__ == "__main__":
    RegisterPy()
</code></pre>

## BIG BUG [pip version need to be up-to-date]
You are using pip version 7.1.2, however version 9.0.1 is available.
You should consider upgrading via the 'python -m pip install --upgrade pip' comm
and.

##REF LINKS
* 1
[安装python爬虫scrapy踩过的那些坑和编程外的思考](http://www.cnblogs.com/rwxwsblog/p/4557123.html)
* 2
[安装Scrapy掉进的坑](http://www.tokyjiao.com/archives/214)

###TO DO LIST
####｛《｝
