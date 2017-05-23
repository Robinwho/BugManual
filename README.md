# BugBook
Record the major bugs encountered in the projects. So easy to find & summary in the future.
##BUG 1 [Tensorflow error: Docker on Mac]
* 1
Expected behavior

Follow the tips by the link:
[TF get_started](https://github.com/Robinwho/tensorflow/blob/master/tensorflow/g3doc/get_started/os_setup.md#optional-setup-gpu-for-mac)
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
[Fix the XXX Network Pro](http://blog.csdn.net/chenming_hnu/article/details/54600270), the problem is fixed!
The correct command:
>$ sudo docker run -it -p 8888:8888 tensorflow/tensorflow

####Detail
https://github.com/docker/for-mac/issues/1145

####TF官方文档中文版
[TF basic_usage](http://wiki.jikexueyuan.com/project/tensorflow-zh/get_started/basic_usage.html)

####[Visualizing MNIST: An Exploration of Dimensionality Reduction](http://colah.github.io/posts/2014-10-Visualizing-MNIST/)

##BUG2 [easy_install command not found]

>$ sudo easy_install pip
>Password:
>sudo: easy_install: command not found

Following this link[why-is-python-easy-install-not-working-on-my-mac](http://stackoverflow.com/questions/6012246/why-is-python-easy-install-not-working-on-my-mac), it can't work.

####Detail
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

####Exa: [python2.7 & 3.5 path configuration]
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

-------------------------------

##BUG3 [Using Sqlite3]
e.g. On Mac:http://www.cnblogs.com/xingfuzzhd/archive/2013/11/04/3407166.html
###useful command:
>sqlite> select * from sqlite_master where type="table";


##BUG4 [Input URL: SyntaxError: invalid syntax]
Using 'http://xxxx.xxx.xxx' instead of http://xxxx.xxx.xxx. See the difference?

Or refer to this:[command-line-input-causes-syntaxerror](http://stackoverflow.com/questions/2589309/command-line-input-causes-syntaxerror)

##BUG5 [TypeError: 'encoding' is an invalid keyword argument for this function]
In Python 2, the open() function takes no encoding argument. The third argument is the buffering option instead.
You appear to be confused with the Python 3 version. If so use io.open() instead:
>import io
>with io.open("file1.txt", "a", encoding="utf-8-sig") as f:
In Python 3, the io.open() function replaced the version from Python 2.

##BUG6 [bs4.FeatureNotFound]
bs4.FeatureNotFound: Couldn't find a tree builder with the features you requested: lxml. Do you need to install a parser library?

####REASON:
You'll notice that in the BS4 documentation page above, they point out that by default BS4 will use the Python built-in HTML parser. Assuming you are in OSX, the Apple-bundled version of Python is 2.7.2 which is not lenient for character formatting.

####SOLUTION:
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
-首先，打开MAC的root账户：
[启用root用户](https://support.apple.com/en-in/HT204012)

-其次，brew install libxml2安装libxml2
-接着，进入root用户操作（su）。
*（1）配置环境变量：如，export C_INCLUDE_PATH=/usr/local/Cellar/libxml2/2.9.4_2/include/libxml2:$C_INCLUDE_PATH。
其中版本号每个系统不一，需进入Cellar目录查看。
（2）使用 pip install lxml安装。成功完成。
</pre>


## BUG7 [pip install scrapy & pip install --upgrade setuptools]
###Problem:
distutils.errors.DistutilsPlatformError: Microsoft Visual C++ 9.0 is require
d. Get it from http://aka.ms/vcpython27
    ----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in c:\users\hp430\ap
pdata\local\temp\pip-build-f17bsa\cffi

####SOLUTION1:
http://aka.ms/vcpython27
Then
>pip install scrapy
error: command 'C:\\Users\\HP430\\AppData\\Local\\Programs\\Common\\Microsoft\\Visual C++ for Python\\9.0\\VC\\Bin\\cl.exe' failed with exit status 2
    ----------------------------------------
<pre><code>
Command "c:\python27\python.exe -c "import setuptools, tokenize;__file__='c:\\users\\hp430\\appdata\\local\\temp\\pip-build-dugrv2\\cryptography\\setup.py';exec(compile(getattr(tokenize, 'open', open)(__file__).read().replace('\r\n', '\n'), __file__, 'exec'))" install --record c:\users\hp430\appdata\local\temp\pip-8ihq2x-record\install-record.txt --single-version-externally-managed --compile" failed with error code 1 in c:\users\hp430\appdata\local\temp\pip-build-dugrv2\cryptography
</code></pre>

####SOLUTION2:
[How to install scrapy](https://cryptography.io/en/latest/installation/#on-windows)

>For Python3:
>pip3 install scrapy

## BUG8 [libxml2]
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
------------------------------------------------------
<pre><code>
//TO DO
</code></pre>

------------------------------------------------------

##111
## BIG BUG [pip version need to be up-to-date]

You are using pip version 7.1.2, however version 9.0.1 is available.
You should consider upgrading via the 'python -m pip install --upgrade pip' comm
and.

###REF LINKS
* 1
[安装python爬虫scrapy踩过的那些坑和编程外的思考](http://www.cnblogs.com/rwxwsblog/p/4557123.html)
* 2
[安装Scrapy掉进的坑](http://www.tokyjiao.com/archives/214)
<pre>
说明：在MACOS10.10+中，安装Scrapy已无问题。只需使用pip install scrapy命令即可自动安装所有必须文件。包括以下。
Successfully installed Automat-0.5.0 PyDispatcher-2.0.5 Twisted-17.1.0 attrs-16.3.0 cffi-1.9.1 constantly-15.1.0 cryptography-1.7.2 cssselect-1.0.1 enum34-1.1.6 idna-2.2 incremental-16.10.1 ipaddress-1.0.18 parsel-1.1.0 pyOpenSSL-16.2.0 pyasn1-0.2.2 pyasn1-modules-0.0.8 pycparser-2.17 queuelib-1.4.2 scrapy-1.3.2 service-identity-16.0.0 six-1.10.0 w3lib-1.17.0 zope.interface-4.3.3
</pre>
####TO DO LIST
<pre>
-bash: run.py: command not found
localhost:LianJia robin$ python run.py 
/usr/local/lib/python2.7/site-packages/scrapy/spiderloader.py:37: RuntimeWarning: 
Traceback (most recent call last):
  File "/usr/local/lib/python2.7/site-packages/scrapy/spiderloader.py", line 31, in _load_all_spiders
    for module in walk_modules(name):
  File "/usr/local/lib/python2.7/site-packages/scrapy/utils/misc.py", line 71, in walk_modules
    submod = import_module(fullpath)
  File "/usr/local/Cellar/python/2.7.12_2/Frameworks/Python.framework/Versions/2.7/lib/python2.7/importlib/__init__.py", line 37, in import_module
    __import__(name)
  File "/Users/robin/git/Crawler/LianJia/LianJia/spiders/lianjia.py", line 8, in <module>
    from scrapy_redis.spiders import RedisSpider
ImportError: No module named scrapy_redis.spiders
Could not load spiders from module 'LianJia.spiders'. Check SPIDER_MODULES setting
  warnings.warn(msg, RuntimeWarning)
2017-02-20 22:06:24 [scrapy.utils.log] INFO: Scrapy 1.3.2 started (bot: LianJia)
2017-02-20 22:06:24 [scrapy.utils.log] INFO: Overridden settings: {'NEWSPIDER_MODULE': 'LianJia.spiders', 'DUPEFILTER_CLASS': 'scrapy_redis.dupefilter.RFPDupeFilter', 'SPIDER_MODULES': ['LianJia.spiders'], 'BOT_NAME': 'LianJia', 'SCHEDULER': 'scrapy_redis.scheduler.Scheduler', 'DOWNLOAD_DELAY': 3}
Traceback (most recent call last):
  File "run.py", line 2, in
    cmdline.execute("scrapy crawl lianjiaspider".split())
  File "/usr/local/lib/python2.7/site-packages/scrapy/cmdline.py", line 142, in execute
    _run_print_help(parser, _run_command, cmd, args, opts)
  File "/usr/local/lib/python2.7/site-packages/scrapy/cmdline.py", line 88, in _run_print_help
    func(*a, **kw)
  File "/usr/local/lib/python2.7/site-packages/scrapy/cmdline.py", line 149, in _run_command
    cmd.run(args, opts)
  File "/usr/local/lib/python2.7/site-packages/scrapy/commands/crawl.py", line 57, in run
    self.crawler_process.crawl(spname, **opts.spargs)
  File "/usr/local/lib/python2.7/site-packages/scrapy/crawler.py", line 162, in crawl
    crawler = self.create_crawler(crawler_or_spidercls)
  File "/usr/local/lib/python2.7/site-packages/scrapy/crawler.py", line 190, in create_crawler
    return self._create_crawler(crawler_or_spidercls)
  File "/usr/local/lib/python2.7/site-packages/scrapy/crawler.py", line 194, in _create_crawler
    spidercls = self.spider_loader.load(spidercls)
  File "/usr/local/lib/python2.7/site-packages/scrapy/spiderloader.py", line 51, in load
    raise KeyError("Spider not found: {}".format(spider_name))
KeyError: 'Spider not found: lianjiaspider'
https://github.com/HunterChao/Crawler
</pre>

## BUG 9 [ImportError: No module named scrapy_redis.spiders]
[相关错误解决](http://blog.csdn.net/pipisorry/article/details/45190851)
scrapy ImportError: No module named settings
原因：爬虫根目录下不能有__init__文件，用startproject命令创建的目录是没有这个的，如果手动创建可能会创建相关文件
解决：将__init__文件移除就OK了

[ImportError: No module named spiders](https://github.com/scrapy/scrapy/issues/1859)
[ImportError: No module named 'spiders'](http://stackoverflow.com/questions/41028605/importerror-no-module-named-spiders)

对于Python3,如仍报错[ImportError: No module named 'scrapy_redis']，则执行pip3 install scrapy_redis解决。

## BUG 10 [redis.exceptions.ConnectionError: Error 61]
connecting to localhost:6379. Connection refused.
Although installing scrapy and redis by: pip install scrapy_redis, 'Error 61' does still happen..
The reason is that the redis server didn't work normally, refer to:[Check the redis server](http://stackoverflow.com/questions/4670049/rescue-connection-refused-unable-to-connect-to-redis-on-localhost6379)
So we should install redis by Homebrew[installing-redis-on-mac-os-x](http://jasdeep.ca/2012/05/installing-redis-on-mac-os-x/).
Then it works! Enjoy!

## BUG 11 [mongodb can't be connected]
[REF](http://blog.csdn.net/quuqu/article/details/52733139)

## BUG 12 [Gradle sync failed: Plugin with id 'com.android.application' not found.]
-SOLUTION:[Error:(1, 0) Plugin with id 'com.android.application' not found](http://stackoverflow.com/questions/24795079/error1-0-plugin-with-id-com-android-application-not-found)

## BUG 13 [Gradle sync failed: Minimum supported Gradle version is 3.3. Current version is 2.14.1]
 If using the gradle wrapper, try editing the distributionUrl in ...\gradle\wrapper\gradle-wrapper.properties to gradle-3.3-all.zip
-SOLUTION:[“Minimum supported Gradle version is 2.14.1. Current version is 2.10.” error](http://stackoverflow.com/questions/39164225/minimum-supported-gradle-version-is-2-14-1-current-version-is-2-10-error) 

## BUG 14 [Electron INSTALL & USE]
throw new Error('Electron failed to install correctly, please delete node_modules/electron and try installing again')
Try
npm install electron --verbose
It should output a progress bar for the download
----
Error: connect ETIMEDOUT 52.216.225.24:443
## BUG 15 [npm/node.js]
After git clone:
<pre>
/electron-quick-start (master)
$ du -m
1       ./.git/hooks
1       ./.git/info
1       ./.git/logs/refs/heads
1       ./.git/logs/refs/remotes/origin
1       ./.git/logs/refs/remotes
1       ./.git/logs/refs
1       ./.git/logs
0       ./.git/objects/info
1       ./.git/objects/pack
1       ./.git/objects
1       ./.git/refs/heads
1       ./.git/refs/remotes/origin
1       ./.git/refs/remotes
0       ./.git/refs/tags
1       ./.git/refs
1       ./.git
1       .
</pre>
After npm install:
<pre>
\electron-quick-start
`-- electron@1.6.8
  +-- electron-download@3.3.0
  | +-- debug@2.6.8
  | | `-- ms@2.0.0
  | +-- fs-extra@0.30.0
  | | +-- graceful-fs@4.1.11
  | | +-- jsonfile@2.4.0
  | | +-- klaw@1.3.1
  | | +-- path-is-absolute@1.0.1
  | | `-- rimraf@2.6.1
  | |   `-- glob@7.1.2
  | |     +-- fs.realpath@1.0.0
  | |     +-- inflight@1.0.6
  | |     | `-- wrappy@1.0.2
  | |     +-- minimatch@3.0.4
  | |     | `-- brace-expansion@1.1.7
  | |     |   +-- balanced-match@0.4.2
  | |     |   `-- concat-map@0.0.1
  | |     `-- once@1.4.0
  | +-- home-path@1.0.5
  | +-- minimist@1.2.0
  | +-- nugget@2.0.1
  | | +-- pretty-bytes@1.0.4
  | | | +-- get-stdin@4.0.1
  | | | `-- meow@3.7.0
  | | |   +-- camelcase-keys@2.1.0
  | | |   | `-- camelcase@2.1.1
  | | |   +-- decamelize@1.2.0
  | | |   +-- loud-rejection@1.6.0
  | | |   | +-- currently-unhandled@0.4.1
  | | |   | | `-- array-find-index@1.0.2
  | | |   | `-- signal-exit@3.0.2
  | | |   +-- map-obj@1.0.1
  | | |   +-- normalize-package-data@2.3.8
  | | |   | +-- hosted-git-info@2.4.2
  | | |   | +-- is-builtin-module@1.0.0
  | | |   | | `-- builtin-modules@1.1.1
  | | |   | `-- validate-npm-package-license@3.0.1
  | | |   |   +-- spdx-correct@1.0.2
  | | |   |   | `-- spdx-license-ids@1.2.2
  | | |   |   `-- spdx-expression-parse@1.0.4
  | | |   +-- object-assign@4.1.1
  | | |   +-- read-pkg-up@1.0.1
  | | |   | +-- find-up@1.1.2
  | | |   | `-- read-pkg@1.1.0
  | | |   |   +-- load-json-file@1.1.0
  | | |   |   | +-- parse-json@2.2.0
  | | |   |   | | `-- error-ex@1.3.1
  | | |   |   | |   `-- is-arrayish@0.2.1
  | | |   |   | +-- pify@2.3.0
  | | |   |   | `-- strip-bom@2.0.0
  | | |   |   |   `-- is-utf8@0.2.1
  | | |   |   `-- path-type@1.1.0
  | | |   +-- redent@1.0.0
  | | |   | +-- indent-string@2.1.0
  | | |   | | `-- repeating@2.0.1
  | | |   | |   `-- is-finite@1.0.2
  | | |   | `-- strip-indent@1.0.1
  | | |   `-- trim-newlines@1.0.0
  | | +-- progress-stream@1.2.0
  | | | +-- speedometer@0.1.4
  | | | `-- through2@0.2.3
  | | |   +-- readable-stream@1.1.14
  | | |   | +-- isarray@0.0.1
  | | |   | `-- string_decoder@0.10.31
  | | |   `-- xtend@2.1.2
  | | |     `-- object-keys@0.4.0
  | | +-- request@2.81.0
  | | | +-- aws-sign2@0.6.0
  | | | +-- aws4@1.6.0
  | | | +-- caseless@0.12.0
  | | | +-- combined-stream@1.0.5
  | | | | `-- delayed-stream@1.0.0
  | | | +-- extend@3.0.1
  | | | +-- forever-agent@0.6.1
  | | | +-- form-data@2.1.4
  | | | | `-- asynckit@0.4.0
  | | | +-- har-validator@4.2.1
  | | | | +-- ajv@4.11.8
  | | | | | +-- co@4.6.0
  | | | | | `-- json-stable-stringify@1.0.1
  | | | | |   `-- jsonify@0.0.0
  | | | | `-- har-schema@1.0.5
  | | | +-- hawk@3.1.3
  | | | | +-- boom@2.10.1
  | | | | +-- cryptiles@2.0.5
  | | | | +-- hoek@2.16.3
  | | | | `-- sntp@1.0.9
  | | | +-- http-signature@1.1.1
  | | | | +-- assert-plus@0.2.0
  | | | | +-- jsprim@1.4.0
  | | | | | +-- assert-plus@1.0.0
  | | | | | +-- extsprintf@1.0.2
  | | | | | +-- json-schema@0.2.3
  | | | | | `-- verror@1.3.6
  | | | | `-- sshpk@1.13.0
  | | | |   +-- asn1@0.2.3
  | | | |   +-- assert-plus@1.0.0
  | | | |   +-- bcrypt-pbkdf@1.0.1
  | | | |   +-- dashdash@1.14.1
  | | | |   | `-- assert-plus@1.0.0
  | | | |   +-- ecc-jsbn@0.1.1
  | | | |   +-- getpass@0.1.7
  | | | |   | `-- assert-plus@1.0.0
  | | | |   +-- jodid25519@1.0.2
  | | | |   +-- jsbn@0.1.1
  | | | |   `-- tweetnacl@0.14.5
  | | | +-- is-typedarray@1.0.0
  | | | +-- isstream@0.1.2
  | | | +-- json-stringify-safe@5.0.1
  | | | +-- mime-types@2.1.15
  | | | | `-- mime-db@1.27.0
  | | | +-- oauth-sign@0.8.2
  | | | +-- performance-now@0.2.0
  | | | +-- qs@6.4.0
  | | | +-- safe-buffer@5.0.1
  | | | +-- stringstream@0.0.5
  | | | +-- tough-cookie@2.3.2
  | | | | `-- punycode@1.4.1
  | | | +-- tunnel-agent@0.6.0
  | | | `-- uuid@3.0.1
  | | +-- single-line-log@1.1.2
  | | | `-- string-width@1.0.2
  | | |   +-- code-point-at@1.1.0
  | | |   +-- is-fullwidth-code-point@1.0.0
  | | |   | `-- number-is-nan@1.0.1
  | | |   `-- strip-ansi@3.0.1
  | | |     `-- ansi-regex@2.1.1
  | | `-- throttleit@0.0.2
  | +-- path-exists@2.1.0
  | | `-- pinkie-promise@2.0.1
  | |   `-- pinkie@2.0.4
  | +-- rc@1.2.1
  | | +-- deep-extend@0.4.2
  | | +-- ini@1.3.4
  | | `-- strip-json-comments@2.0.1
  | +-- semver@5.3.0
  | `-- sumchecker@1.3.1
  |   `-- es6-promise@4.1.0
  `-- extract-zip@1.6.5
    +-- concat-stream@1.6.0
    | +-- inherits@2.0.3
    | +-- readable-stream@2.2.9
    | | +-- buffer-shims@1.0.0
    | | +-- core-util-is@1.0.2
    | | +-- isarray@1.0.0
    | | +-- process-nextick-args@1.0.7
    | | +-- string_decoder@1.0.1
    | | `-- util-deprecate@1.0.2
    | `-- typedarray@0.0.6
    +-- debug@2.2.0
    | `-- ms@0.7.1
    +-- mkdirp@0.5.0
    | `-- minimist@0.0.8
    `-- yauzl@2.4.1
      `-- fd-slicer@1.0.1
        `-- pend@1.2.0
</pre>
<pre>
/electron-quick-start (master)
$ du -m
1       ./.git/hooks
1       ./.git/info
1       ./.git/logs/refs/heads
1       ./.git/logs/refs/remotes/origin
1       ./.git/logs/refs/remotes
1       ./.git/logs/refs
1       ./.git/logs
0       ./.git/objects/info
1       ./.git/objects/pack
1       ./.git/objects
1       ./.git/refs/heads
1       ./.git/refs/remotes/origin
1       ./.git/refs/remotes
0       ./.git/refs/tags
1       ./.git/refs
1       ./.git
1       ./node_modules/.bin
2       ./node_modules/ajv/dist
1       ./node_modules/ajv/lib/compile
1       ./node_modules/ajv/lib/dot/v5
1       ./node_modules/ajv/lib/dot
1       ./node_modules/ajv/lib/dotjs
1       ./node_modules/ajv/lib/refs
1       ./node_modules/ajv/lib
1       ./node_modules/ajv/scripts
3       ./node_modules/ajv
1       ./node_modules/ansi-regex
1       ./node_modules/array-find-index
1       ./node_modules/asn1/lib/ber
1       ./node_modules/asn1/lib
1       ./node_modules/asn1/tst/ber
1       ./node_modules/asn1/tst
1       ./node_modules/asn1
1       ./node_modules/assert-plus
1       ./node_modules/asynckit/lib
1       ./node_modules/asynckit
1       ./node_modules/aws-sign2
1       ./node_modules/aws4
1       ./node_modules/balanced-match
1       ./node_modules/bcrypt-pbkdf
1       ./node_modules/boom/images
1       ./node_modules/boom/lib
1       ./node_modules/boom/test
1       ./node_modules/boom
1       ./node_modules/brace-expansion
1       ./node_modules/buffer-shims
1       ./node_modules/builtin-modules
1       ./node_modules/camelcase
1       ./node_modules/camelcase-keys
1       ./node_modules/caseless
1       ./node_modules/co
1       ./node_modules/code-point-at
1       ./node_modules/combined-stream/lib
1       ./node_modules/combined-stream
1       ./node_modules/concat-map/example
1       ./node_modules/concat-map/test
1       ./node_modules/concat-map
1       ./node_modules/concat-stream/node_modules/isarray
1       ./node_modules/concat-stream/node_modules/readable-stream/doc/wg-meetings
1       ./node_modules/concat-stream/node_modules/readable-stream/doc
1       ./node_modules/concat-stream/node_modules/readable-stream/lib/internal/streams
1       ./node_modules/concat-stream/node_modules/readable-stream/lib/internal
1       ./node_modules/concat-stream/node_modules/readable-stream/lib
1       ./node_modules/concat-stream/node_modules/readable-stream
1       ./node_modules/concat-stream/node_modules/string_decoder/lib
1       ./node_modules/concat-stream/node_modules/string_decoder
1       ./node_modules/concat-stream/node_modules
1       ./node_modules/concat-stream
1       ./node_modules/core-util-is/lib
1       ./node_modules/core-util-is
1       ./node_modules/cryptiles/lib
1       ./node_modules/cryptiles/test
1       ./node_modules/cryptiles
1       ./node_modules/currently-unhandled
1       ./node_modules/dashdash/etc
1       ./node_modules/dashdash/lib
1       ./node_modules/dashdash/node_modules/assert-plus
1       ./node_modules/dashdash/node_modules
1       ./node_modules/dashdash
1       ./node_modules/debug/src
1       ./node_modules/debug
1       ./node_modules/decamelize
1       ./node_modules/deep-extend/lib
1       ./node_modules/deep-extend
1       ./node_modules/delayed-stream/lib
1       ./node_modules/delayed-stream
1       ./node_modules/ecc-jsbn/lib
1       ./node_modules/ecc-jsbn
1       ./node_modules/electron/dist/locales
1       ./node_modules/electron/dist/resources
129     ./node_modules/electron/dist
1       ./node_modules/electron/test
129     ./node_modules/electron
1       ./node_modules/electron-download/build
1       ./node_modules/electron-download
1       ./node_modules/error-ex
1       ./node_modules/es6-promise/dist
1       ./node_modules/es6-promise/lib/es6-promise/promise
1       ./node_modules/es6-promise/lib/es6-promise
1       ./node_modules/es6-promise/lib
1       ./node_modules/es6-promise
1       ./node_modules/extend
1       ./node_modules/extract-zip/node_modules/debug
1       ./node_modules/extract-zip/node_modules/ms
1       ./node_modules/extract-zip/node_modules
1       ./node_modules/extract-zip
1       ./node_modules/extsprintf/examples
1       ./node_modules/extsprintf/lib
1       ./node_modules/extsprintf
1       ./node_modules/fd-slicer/test
1       ./node_modules/fd-slicer
1       ./node_modules/find-up
1       ./node_modules/forever-agent
1       ./node_modules/form-data/lib
1       ./node_modules/form-data
1       ./node_modules/fs-extra/lib/copy
1       ./node_modules/fs-extra/lib/copy-sync
1       ./node_modules/fs-extra/lib/empty
1       ./node_modules/fs-extra/lib/ensure
1       ./node_modules/fs-extra/lib/json
1       ./node_modules/fs-extra/lib/mkdirs
1       ./node_modules/fs-extra/lib/move
1       ./node_modules/fs-extra/lib/output
1       ./node_modules/fs-extra/lib/remove
1       ./node_modules/fs-extra/lib/util
1       ./node_modules/fs-extra/lib/walk
1       ./node_modules/fs-extra/lib
1       ./node_modules/fs-extra
1       ./node_modules/fs.realpath
1       ./node_modules/get-stdin
1       ./node_modules/getpass/lib
1       ./node_modules/getpass/node_modules/assert-plus
1       ./node_modules/getpass/node_modules
1       ./node_modules/getpass
1       ./node_modules/glob
1       ./node_modules/graceful-fs
1       ./node_modules/har-schema/lib
1       ./node_modules/har-schema
1       ./node_modules/har-validator/lib/browser
1       ./node_modules/har-validator/lib/node4
1       ./node_modules/har-validator/lib/node6
1       ./node_modules/har-validator/lib/node7
1       ./node_modules/har-validator/lib
1       ./node_modules/har-validator/src
1       ./node_modules/har-validator
1       ./node_modules/hawk/dist
1       ./node_modules/hawk/example
1       ./node_modules/hawk/images
1       ./node_modules/hawk/lib
1       ./node_modules/hawk/test
1       ./node_modules/hawk
1       ./node_modules/hoek/images
1       ./node_modules/hoek/lib
1       ./node_modules/hoek/test/modules
1       ./node_modules/hoek/test
1       ./node_modules/hoek
1       ./node_modules/home-path/jsdoc2md
1       ./node_modules/home-path/lib
1       ./node_modules/home-path/test
1       ./node_modules/home-path
1       ./node_modules/hosted-git-info
1       ./node_modules/http-signature/lib
1       ./node_modules/http-signature
1       ./node_modules/indent-string
1       ./node_modules/inflight
1       ./node_modules/inherits
1       ./node_modules/ini
1       ./node_modules/is-arrayish
1       ./node_modules/is-builtin-module
1       ./node_modules/is-finite
1       ./node_modules/is-fullwidth-code-point
1       ./node_modules/is-typedarray
1       ./node_modules/is-utf8
1       ./node_modules/isarray/build
1       ./node_modules/isarray
1       ./node_modules/isstream
1       ./node_modules/jodid25519/lib
1       ./node_modules/jodid25519
1       ./node_modules/jsbn
1       ./node_modules/json-schema/draft-00
1       ./node_modules/json-schema/draft-01
1       ./node_modules/json-schema/draft-02
1       ./node_modules/json-schema/draft-03/examples
1       ./node_modules/json-schema/draft-03
1       ./node_modules/json-schema/draft-04
1       ./node_modules/json-schema/lib
1       ./node_modules/json-schema/test
1       ./node_modules/json-schema
1       ./node_modules/json-stable-stringify/example
1       ./node_modules/json-stable-stringify/test
1       ./node_modules/json-stable-stringify
1       ./node_modules/json-stringify-safe/test
1       ./node_modules/json-stringify-safe
1       ./node_modules/jsonfile
1       ./node_modules/jsonify/lib
1       ./node_modules/jsonify/test
1       ./node_modules/jsonify
1       ./node_modules/jsprim/lib
1       ./node_modules/jsprim/node_modules/assert-plus
1       ./node_modules/jsprim/node_modules
1       ./node_modules/jsprim
1       ./node_modules/klaw/src
1       ./node_modules/klaw
1       ./node_modules/load-json-file
1       ./node_modules/loud-rejection
1       ./node_modules/map-obj
1       ./node_modules/meow
1       ./node_modules/mime-db
1       ./node_modules/mime-types
1       ./node_modules/minimatch
1       ./node_modules/minimist/example
1       ./node_modules/minimist/test
1       ./node_modules/minimist
1       ./node_modules/mkdirp/bin
1       ./node_modules/mkdirp/examples
1       ./node_modules/mkdirp/node_modules/minimist/example
1       ./node_modules/mkdirp/node_modules/minimist/test
1       ./node_modules/mkdirp/node_modules/minimist
1       ./node_modules/mkdirp/node_modules
1       ./node_modules/mkdirp/test
1       ./node_modules/mkdirp
1       ./node_modules/ms
1       ./node_modules/normalize-package-data/lib
1       ./node_modules/normalize-package-data
1       ./node_modules/nugget/test
1       ./node_modules/nugget
1       ./node_modules/number-is-nan
1       ./node_modules/oauth-sign
1       ./node_modules/object-assign
1       ./node_modules/object-keys/test
1       ./node_modules/object-keys
1       ./node_modules/once
1       ./node_modules/parse-json/vendor
1       ./node_modules/parse-json
1       ./node_modules/path-exists
1       ./node_modules/path-is-absolute
1       ./node_modules/path-type
1       ./node_modules/pend
1       ./node_modules/performance-now/lib
1       ./node_modules/performance-now/src
1       ./node_modules/performance-now/test
1       ./node_modules/performance-now
1       ./node_modules/pify
1       ./node_modules/pinkie
1       ./node_modules/pinkie-promise
1       ./node_modules/pretty-bytes
1       ./node_modules/process-nextick-args
1       ./node_modules/progress-stream/test
1       ./node_modules/progress-stream
1       ./node_modules/punycode
1       ./node_modules/qs/dist
1       ./node_modules/qs/lib
1       ./node_modules/qs/test
1       ./node_modules/qs
1       ./node_modules/rc/lib
1       ./node_modules/rc/test
1       ./node_modules/rc
1       ./node_modules/read-pkg
1       ./node_modules/read-pkg-up
1       ./node_modules/readable-stream/lib
1       ./node_modules/readable-stream
1       ./node_modules/redent
1       ./node_modules/repeating
1       ./node_modules/request/lib
1       ./node_modules/request
1       ./node_modules/rimraf
1       ./node_modules/safe-buffer
1       ./node_modules/semver/bin
1       ./node_modules/semver
1       ./node_modules/signal-exit
1       ./node_modules/single-line-log
1       ./node_modules/sntp/examples
1       ./node_modules/sntp/lib
1       ./node_modules/sntp/test
1       ./node_modules/sntp
1       ./node_modules/spdx-correct
1       ./node_modules/spdx-expression-parse
1       ./node_modules/spdx-license-ids
1       ./node_modules/speedometer
1       ./node_modules/sshpk/bin
1       ./node_modules/sshpk/lib/formats
1       ./node_modules/sshpk/lib
1       ./node_modules/sshpk/man/man1
1       ./node_modules/sshpk/man
1       ./node_modules/sshpk/node_modules/assert-plus
1       ./node_modules/sshpk/node_modules
1       ./node_modules/sshpk
1       ./node_modules/string-width
1       ./node_modules/stringstream
1       ./node_modules/string_decoder
1       ./node_modules/strip-ansi
1       ./node_modules/strip-bom
1       ./node_modules/strip-indent
1       ./node_modules/strip-json-comments
1       ./node_modules/sumchecker
1       ./node_modules/throttleit
1       ./node_modules/through2
1       ./node_modules/tough-cookie/lib
1       ./node_modules/tough-cookie
1       ./node_modules/trim-newlines
1       ./node_modules/tunnel-agent
1       ./node_modules/tweetnacl
1       ./node_modules/typedarray/example
1       ./node_modules/typedarray/test/server
1       ./node_modules/typedarray/test
1       ./node_modules/typedarray
1       ./node_modules/util-deprecate
1       ./node_modules/uuid/bin
1       ./node_modules/uuid/lib
1       ./node_modules/uuid/test
1       ./node_modules/uuid
1       ./node_modules/validate-npm-package-license
1       ./node_modules/verror/examples
1       ./node_modules/verror/lib
1       ./node_modules/verror/tests
1       ./node_modules/verror
1       ./node_modules/wrappy
1       ./node_modules/xtend
1       ./node_modules/yauzl
138     ./node_modules
138     .
</pre>

##### TODO LIST
ELK/
<pre>
2017-03-13 21:48:59 [scrapy.core.engine] INFO: Spider opened
2017-03-13 21:49:00 [scrapy.extensions.logstats] INFO: Crawled 0 pages (at 0 pages/min), scraped 0 items (at 0 items/min)
2017-03-13 21:49:00 [scrapy.extensions.telnet] DEBUG: Telnet console listening on 127.0.0.1:6023
2017-03-13 21:49:00 [scrapy_redis.dupefilter] DEBUG: Filtered duplicate request < http://bj.lianjia.com/ershoufang/> - no more duplicates will be shown (see DUPEFILTER_DEBUG to show all duplicates)
2017-03-13 21:50:00 [scrapy.extensions.logstats] INFO: Crawled 0 pages (at 0 pages/min), scraped 0 items (at 0 items/min)
</pre>
