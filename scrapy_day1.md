爬虫学习day1
1请求库requests
爬虫可以简单分为几步：抓取页面、分析页面和存储数据。
在抓取页面的过程中，我们需要模拟浏览器向服务器发出请求，所以需要用到一些Python库来实现HTTP请求操作。在本书中，我们用到的第三方库有Requests、Selenium和aiohttp等。
在本节中，我们介绍一下这些请求库的安装方法。
1. 相关链接
GitHub：https://github.com/requests/requests
PyPI：https://pypi.python.org/pypi/requests
官方文档：http://www.python-requests.org
中文文档：http://docs.python-requests.org/zh_CN/latest

2. pip安装
pip3 install requests

3. requests使用
get请求
import requestsr=requests.get('https://www.python.org')r.status_code>>200
post请求
payload = dict(key1='value1', key2='value2')r = requests.post('http://httpbin.org/post', data=payload)print(r.text)>>{  "args": {},   "data": "",   "files": {},   "form": {    "key1": "value1",     "key2": "value2"  },   "headers": {    "Accept": "*/*",     "Accept-Encoding": "gzip, deflate",     "Content-Length": "23",     "Content-Type": "application/x-www-form-urlencoded",     "Host": "httpbin.org",     "User-Agent": "python-requests/2.19.1"  },   "json": null,   "origin": "211.67.21.205, 211.67.21.205",   "url": "https://httpbin.org/post"}


4. 转态码含义
状态码常用来判断请求是否成功，而requests还提供了一个内置的状态码查询对象requests.codes，示例如下：
1234 import requests r = requests.get('http://www.jianshu.com')exit() if not r.status_code == requests.codes.ok else print('Request Successfully')
这里通过比较返回码和内置的成功的返回码，来保证请求得到了正常响应，输出成功请求的消息，否则程序终止，这里我们用requests.codes.ok得到的是成功的状态码200。
那么，肯定不能只有ok这个条件码。下面列出了返回码和相应的查询条件：
# 信息性状态码100: ('continue',),101: ('switching_protocols',),102: ('processing',),103: ('checkpoint',),122: ('uri_too_long', 'request_uri_too_long'), # 成功状态码200: ('ok', 'okay', 'all_ok', 'all_okay', 'all_good', '\\o/', '✓'),201: ('created',),202: ('accepted',),203: ('non_authoritative_info', 'non_authoritative_information'),204: ('no_content',),205: ('reset_content', 'reset'),206: ('partial_content', 'partial'),207: ('multi_status', 'multiple_status', 'multi_stati', 'multiple_stati'),208: ('already_reported',),226: ('im_used',), # 重定向状态码300: ('multiple_choices',),301: ('moved_permanently', 'moved', '\\o-'),302: ('found',),303: ('see_other', 'other'),304: ('not_modified',),305: ('use_proxy',),306: ('switch_proxy',),307: ('temporary_redirect', 'temporary_moved', 'temporary'),308: ('permanent_redirect',      'resume_incomplete', 'resume',), # These 2 to be removed in 3.0 # 客户端错误状态码400: ('bad_request', 'bad'),401: ('unauthorized',),402: ('payment_required', 'payment'),403: ('forbidden',),404: ('not_found', '-o-'),405: ('method_not_allowed', 'not_allowed'),406: ('not_acceptable',),407: ('proxy_authentication_required', 'proxy_auth', 'proxy_authentication'),408: ('request_timeout', 'timeout'),409: ('conflict',),410: ('gone',),411: ('length_required',),412: ('precondition_failed', 'precondition'),413: ('request_entity_too_large',),414: ('request_uri_too_large',),415: ('unsupported_media_type', 'unsupported_media', 'media_type'),416: ('requested_range_not_satisfiable', 'requested_range', 'range_not_satisfiable'),417: ('expectation_failed',),418: ('im_a_teapot', 'teapot', 'i_am_a_teapot'),421: ('misdirected_request',),422: ('unprocessable_entity', 'unprocessable'),423: ('locked',),424: ('failed_dependency', 'dependency'),425: ('unordered_collection', 'unordered'),426: ('upgrade_required', 'upgrade'),428: ('precondition_required', 'precondition'),429: ('too_many_requests', 'too_many'),431: ('header_fields_too_large', 'fields_too_large'),444: ('no_response', 'none'),449: ('retry_with', 'retry'),450: ('blocked_by_windows_parental_controls', 'parental_controls'),451: ('unavailable_for_legal_reasons', 'legal_reasons'),499: ('client_closed_request',), # 服务端错误状态码500: ('internal_server_error', 'server_error', '/o\\', '✗'),501: ('not_implemented',),502: ('bad_gateway',),503: ('service_unavailable', 'unavailable'),504: ('gateway_timeout',),505: ('http_version_not_supported', 'http_version'),506: ('variant_also_negotiates',),507: ('insufficient_storage',),509: ('bandwidth_limit_exceeded', 'bandwidth'),510: ('not_extended',),511: ('network_authentication_required', 'network_auth', 'network_authentication')


5. 添加请求头headers
比如，在上面“知乎”的例子中，如果不传递headers，就不能正常请求：
import requests headers = {    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36'}r = requests.get("https://www.zhihu.com/explore", headers=headers)print(r.text)
当然，我们可以在headers这个参数中任意添加其他的字段信息。
6. 响应
发送请求后，得到的自然就是响应。在上面的实例中，我们使用text和content获取了响应的内容。此外，还有很多属性和方法可以用来获取其他信息，比如状态码、响应头、Cookies等。示例如下：
12345678 import requests r = requests.get('http://www.jianshu.com')print(type(r.status_code), r.status_code)print(type(r.headers), r.headers)print(type(r.cookies), r.cookies)print(type(r.url), r.url)print(type(r.history), r.history)
这里分别打印输出status_code属性得到状态码，输出headers属性得到响应头，输出cookies属性得到Cookies，输出url属性得到URL，输出history属性得到请求历史。
运行结果如下：
<class 'int'> 200<class 'requests.structures.CaseInsensitiveDict'> {'X-Runtime': '0.006363', 'Connection': 'keep-alive', 'Content-Type': 'text/html; charset=utf-8', 'X-Content-Type-Options': 'nosniff', 'Date': 'Sat, 27 Aug 2016 17:18:51 GMT', 'Server': 'nginx', 'X-Frame-Options': 'DENY', 'Content-Encoding': 'gzip', 'Vary': 'Accept-Encoding', 'ETag': 'W/"3abda885e0e123bfde06d9b61e696159"', 'X-XSS-Protection': '1; mode=block', 'X-Request-Id': 'a8a3c4d5-f660-422f-8df9-49719dd9b5d4', 'Transfer-Encoding': 'chunked', 'Set-Cookie': 'read_mode=day; path=/, default_font=font2; path=/, _session_id=xxx; path=/; HttpOnly', 'Cache-Control': 'max-age=0, private, must-revalidate'}<class 'requests.cookies.RequestsCookieJar'> <RequestsCookieJar[<Cookie _session_id=xxx for www.jianshu.com/>, <Cookie default_font=font2 for www.jianshu.com/>, <Cookie read_mode=day for www.jianshu.com/>]><class 'str'> http://www.jianshu.com/<class 'list'> []
因为session_id过长，在此简写。可以看到，headers和cookies这两个属性得到的结果分别是CaseInsensitiveDict和RequestsCookieJar类型。
7. 思考
对库的学习首先可以通过help函数，需要加强对这样文档信息的获取能力。
import requests as reqhelp(req)help(req.get)help(req.post)import cv2help(cv2)
get和post的区别。post主要可以附带信息来向网址发送请求，主要应用在需要登录的网址上。get是直接获取网址信息（不需要登录）

2正则表达式
1.概念
正则表达式(Regular Expression)是一种文本模式，包括普通字符（例如，a到z之间的字母）和特殊字符（称为”元字符”）。正则表达式使用单个字符串来描述、匹配一系列匹配某个句法规则的字符串。
举例
 ^[0-9]+abc$，其含义是：
^为匹配输入字符串的开始位置
[0-9]+匹配多个数字，[0-9]匹配单个数字，+匹配一个或者多个
abc$匹配字符abc并以abc结尾，$为匹配输入字符串的结束位置。

我们在写用户注册表时，只允许用户名包含字符、数字、下划线和连接字符(-)，并设置用户名长度，可以用以下正则表达式设定： ^[a-z0-9_-]{3,15}$
为什么使用正则表达式？
通过使用正则表达式，可以：
测试字符串内的模式。例如，可以测试输入字符串，以查看字符串内是否出现电话号码模式或信用卡号码模式。这称为数据验证。
替换文本。可以使用正则表达式来识别文档中的特定文本，完全删除该文本或者用其他文本替换它。
基于模式匹配从字符串中提取子字符串。可以查找文档内或输入域内特定的文本。
2. python正则表达式
re模块使python语言拥有全部的正则表达式功能。
re.match函数
re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。函数语法:re.match(pattern, string, flags=0)。
pattern: 匹配的正则表达式
string: 要匹配的字符串
flags: 标志位，用于控制正则表达式的匹配方式
import reprint(re.match('www', 'www.runoob.com').span())  # 在起始位置匹配print(re.match('com', 'www.runoob.com'))         # 不在起始位置匹配以上实例运行输出结果为：(0, 3)None
其中span()函数span() 返回一个元组包含匹配 (开始,结束) 的位置。


我们可以使用group(num)或groups()匹配对象函数来获取匹配表达式。
group(num=0): 匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。
groups(): 返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。
line = "Cats are smarter than dogs"# .* 表示任意匹配除换行符（\n、\r）之外的任何单个或多个字符matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I) if matchObj:   print ("matchObj.group() : ", matchObj.group())   print ("matchObj.group(1) : ", matchObj.group(1))   print ("matchObj.group(2) : ", matchObj.group(2))else:   print ("No match!!")以上实例执行结果如下：matchObj.group() :  Cats are smarter than dogsmatchObj.group(1) :  CatsmatchObj.group(2) :  smarter
前面的一个r表示字符串为非转义的原始字符串，让编译器忽略反斜杠，也就是忽略转义字符。但是这个字符串里没有反斜杠，所以这个r可有可无。
 (.*) 第一个匹配分组，.*代表匹配除换行符之外的所有字符；(.*?)第二个匹配分组，.*?后面多个问号，代表非贪婪模式，也就是说只匹配符合条件的最少字符；后面的一个.* 没有括号包围，所以不是分组，匹配效果和第一个一样，但是不计入匹配结果中。

re.search函数
re.search 扫描整个字符串并返回第一个成功的匹配。函数语法：re.search(pattern, string, flags=0)，参数含义与match函数的相同。
例子：

re.match与re.search的区别
re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。
re.sub检索和替换
Python 的re模块提供了re.sub用于替换字符串中的匹配项。
#!/usr/bin/python3import re phone = "2004-959-559 # 这是一个电话号码" # 删除注释num = re.sub(r'#.*$', "", phone)print ("电话号码 : ", num) # 移除非数字的内容num = re.sub(r'\D', "", phone)print ("电话号码 : ", num)以上实例执行结果如下：电话号码 :  2004-959-559 电话号码 :  2004959559
re.sub 使用实例：改变日期的格式。如中国格式 2017-11-27 改为美国格式 11/27/2017:
>>> s = '2017-11-27'
>>> import re
>>> print(re.sub('(\d{4})-(\d{2})-(\d{2})',r'\2/\3/\1', s))
11/27/2017

用 () 来划定原字符串的组，{} 中表示数字的个数，r 即后面的字符串为原始字符串，防止计算机将 \ 理解为转义字符，2，3，1 为输入的字符串三段的序号。

re.sub 匹配标点符号、换行实例。
import re
s = "you're asking me out.that's so cute.what's your name again?"
print(re.sub(r"([.!?])", r"\1\n", s))
输出结果：
you're asking me out.
that's so cute.
what's your name again?

Python3 匹配 IP 地址实例
import re
ip ='192.168.1.1'
trueIp =re.search(r'(([01]{0,1}\d{0,1}\d|2[0-4]\d|25[0-5])\.){3}([01]{0,1}\d{0,1}\d|2[0-4]\d|25[0-5])',ip)
print(trueIp)
输出结果：
192.168.1.1


compile 函数
compile 函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。
语法格式为：
re.compile(pattern[, flags])
参数：
pattern : 一个字符串形式的正则表达式
flags 可选，表示匹配模式，比如忽略大小写，多行模式等，具体参数为：
re.I 忽略大小写
re.L 表示特殊字符集 \w, \W, \b, \B, \s, \S 依赖于当前环境
re.M 多行模式
re.S 即为' . '并且包括换行符在内的任意字符（' . '不包括换行符）
re.U 表示特殊字符集 \w, \W, \b, \B, \d, \D, \s, \S 依赖于 Unicode 字符属性数据库
re.X 为了增加可读性，忽略空格和' # '后面的注释
>>>import re>>> pattern = re.compile(r'([a-z]+) ([a-z]+)', re.I)   # re.I 表示忽略大小写>>> m = pattern.match('Hello World Wide Web')>>> print m                               # 匹配成功，返回一个 Match 对象<_sre.SRE_Match object at 0x10bea83e8>>>> m.group(0)                            # 返回匹配成功的整个子串'Hello World'>>> m.span(0)                             # 返回匹配成功的整个子串的索引(0, 11)>>> m.group(1)                            # 返回第一个分组匹配成功的子串'Hello'>>> m.span(1)                             # 返回第一个分组匹配成功的子串的索引(0, 5)>>> m.group(2)                            # 返回第二个分组匹配成功的子串'World'>>> m.span(2)                             # 返回第二个分组匹配成功的子串索引(6, 11)>>> m.groups()                            # 等价于 (m.group(1), m.group(2), ...)('Hello', 'World')>>> m.group(3)                            # 不存在第三个分组Traceback (most recent call last):  File "<stdin>", line 1, in <module>IndexError: no such group
findall
在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。
注意： match 和 search 是匹配一次 findall 匹配所有。
语法格式为：
findall(string[, pos[, endpos]])
参数：
string 待匹配的字符串。
pos 可选参数，指定字符串的起始位置，默认为 0。
endpos 可选参数，指定字符串的结束位置，默认为字符串的长度。

Python 3 教程
Python3 教程Python3 环境搭建Python3 基础语法Python3 基本数据类型Python3 解释器Python3 注释Python3 运算符Python3 数字(Number)Python3 字符串Python3 列表Python3 元组Python3 字典Python3 集合Python3 编程第一步Python3 条件控制Python3 循环语句Python3 迭代器与生成器Python3 函数Python3 数据结构Python3 模块Python3 输入和输出Python3 FilePython3 OSPython3 错误和异常Python3 面向对象Python3 标准库概览Python3 实例Python 测验
Python3 高级教程
Python3 正则表达式Python3 CGI编程Python3 MySQL(mysql-connector)Python3 MySQL(PyMySQL)Python3 网络编程Python3 SMTP发送邮件Python3 多线程Python3 XML 解析Python3 JSONPython3 日期和时间Python3 内置函数Python MongoDBPython uWSGI 安装配置
 Python 测验
Python3 CGI编程 
Python3 正则表达式
正则表达式是一个特殊的字符序列，它能帮助你方便的检查一个字符串是否与某种模式匹配。
Python 自1.5版本起增加了re 模块，它提供 Perl 风格的正则表达式模式。
re 模块使 Python 语言拥有全部的正则表达式功能。
compile 函数根据一个模式字符串和可选的标志参数生成一个正则表达式对象。该对象拥有一系列方法用于正则表达式匹配和替换。
re 模块也提供了与这些方法功能完全一致的函数，这些函数使用一个模式字符串做为它们的第一个参数。
本章节主要介绍 Python 中常用的正则表达式处理函数，如果你对正则表达式不了解，可以查看我们的 正则表达式 - 教程。
--------------------------------------------------------------------------------
re.match函数
re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。
函数语法：
re.match(pattern, string, flags=0)
函数参数说明：
参数 描述
pattern 匹配的正则表达式
string 要匹配的字符串。
flags 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。参见：正则表达式修饰符 - 可选标志
匹配成功re.match方法返回一个匹配的对象，否则返回None。
我们可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。
匹配对象方法 描述
group(num=0) 匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。
groups() 返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。
实例
#!/usr/bin/python
 
import re
print(re.match('www', 'www.runoob.com').span())  # 在起始位置匹配
print(re.match('com', 'www.runoob.com'))         # 不在起始位置匹配
以上实例运行输出结果为：
(0, 3)
None
实例
#!/usr/bin/python3
import re
 
line = "Cats are smarter than dogs"
# .* 表示任意匹配除换行符（\n、\r）之外的任何单个或多个字符
matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)
 
if matchObj:
   print ("matchObj.group() : ", matchObj.group())
   print ("matchObj.group(1) : ", matchObj.group(1))
   print ("matchObj.group(2) : ", matchObj.group(2))
else:
   print ("No match!!")
以上实例执行结果如下：
matchObj.group() :  Cats are smarter than dogs
matchObj.group(1) :  Cats
matchObj.group(2) :  smarter
--------------------------------------------------------------------------------
re.search方法
re.search 扫描整个字符串并返回第一个成功的匹配。
函数语法：
re.search(pattern, string, flags=0)
函数参数说明：
参数 描述
pattern 匹配的正则表达式
string 要匹配的字符串。
flags 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。参见：正则表达式修饰符 - 可选标志
匹配成功re.search方法返回一个匹配的对象，否则返回None。
我们可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。
匹配对象方法 描述
group(num=0) 匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。
groups() 返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。
实例
#!/usr/bin/python3
 
import re
 
print(re.search('www', 'www.runoob.com').span())  # 在起始位置匹配
print(re.search('com', 'www.runoob.com').span())         # 不在起始位置匹配
以上实例运行输出结果为：
(0, 3)
(11, 14)
实例
#!/usr/bin/python3
 
import re
 
line = "Cats are smarter than dogs";
 
searchObj = re.search( r'(.*) are (.*?) .*', line, re.M|re.I)
 
if searchObj:
   print ("searchObj.group() : ", searchObj.group())
   print ("searchObj.group(1) : ", searchObj.group(1))
   print ("searchObj.group(2) : ", searchObj.group(2))
else:
   print ("Nothing found!!")
以上实例执行结果如下：
searchObj.group() :  Cats are smarter than dogs
searchObj.group(1) :  Cats
searchObj.group(2) :  smarter
--------------------------------------------------------------------------------
re.match与re.search的区别
re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。
实例
#!/usr/bin/python3
 
import re
 
line = "Cats are smarter than dogs";
 
matchObj = re.match( r'dogs', line, re.M|re.I)
if matchObj:
   print ("match --> matchObj.group() : ", matchObj.group())
else:
   print ("No match!!")
 
matchObj = re.search( r'dogs', line, re.M|re.I)
if matchObj:
   print ("search --> matchObj.group() : ", matchObj.group())
else:
   print ("No match!!")
以上实例运行结果如下：
No match!!
search --> matchObj.group() :  dogs
--------------------------------------------------------------------------------
检索和替换
Python 的re模块提供了re.sub用于替换字符串中的匹配项。
语法：
re.sub(pattern, repl, string, count=0)
参数：
pattern : 正则中的模式字符串。
repl : 替换的字符串，也可为一个函数。
string : 要被查找替换的原始字符串。
count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。
实例
#!/usr/bin/python3
import re
 
phone = "2004-959-559 # 这是一个电话号码"
 
# 删除注释
num = re.sub(r'#.*$', "", phone)
print ("电话号码 : ", num)
 
# 移除非数字的内容
num = re.sub(r'\D', "", phone)
print ("电话号码 : ", num)
以上实例执行结果如下：
电话号码 :  2004-959-559 
电话号码 :  2004959559
repl 参数是一个函数
以下实例中将字符串中的匹配的数字乘于 2：
实例
#!/usr/bin/python
 
import re
 
# 将匹配的数字乘于 2
def double(matched):
    value = int(matched.group('value'))
    return str(value * 2)
 
s = 'A23G4HFD567'
print(re.sub('(?P<value>\d+)', double, s))
执行输出结果为：
A46G8HFD1134
compile 函数
compile 函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。
语法格式为：
re.compile(pattern[, flags])
参数：
pattern : 一个字符串形式的正则表达式
flags 可选，表示匹配模式，比如忽略大小写，多行模式等，具体参数为：
re.I 忽略大小写
re.L 表示特殊字符集 \w, \W, \b, \B, \s, \S 依赖于当前环境
re.M 多行模式
re.S 即为' . '并且包括换行符在内的任意字符（' . '不包括换行符）
re.U 表示特殊字符集 \w, \W, \b, \B, \d, \D, \s, \S 依赖于 Unicode 字符属性数据库
re.X 为了增加可读性，忽略空格和' # '后面的注释
实例
实例
>>>import re
>>> pattern = re.compile(r'\d+')                    # 用于匹配至少一个数字
>>> m = pattern.match('one12twothree34four')        # 查找头部，没有匹配
>>> print m
None
>>> m = pattern.match('one12twothree34four', 2, 10) # 从'e'的位置开始匹配，没有匹配
>>> print m
None
>>> m = pattern.match('one12twothree34four', 3, 10) # 从'1'的位置开始匹配，正好匹配
>>> print m                                         # 返回一个 Match 对象
<_sre.SRE_Match object at 0x10a42aac0>
>>> m.group(0)   # 可省略 0
'12'
>>> m.start(0)   # 可省略 0
3
>>> m.end(0)     # 可省略 0
5
>>> m.span(0)    # 可省略 0
(3, 5)
在上面，当匹配成功时返回一个 Match 对象，其中：
group([group1, …]) 方法用于获得一个或多个分组匹配的字符串，当要获得整个匹配的子串时，可直接使用 group() 或 group(0)；
start([group]) 方法用于获取分组匹配的子串在整个字符串中的起始位置（子串第一个字符的索引），参数默认值为 0；
end([group]) 方法用于获取分组匹配的子串在整个字符串中的结束位置（子串最后一个字符的索引+1），参数默认值为 0；
span([group]) 方法返回 (start(group), end(group))。
再看看一个例子：
实例
>>>import re
>>> pattern = re.compile(r'([a-z]+) ([a-z]+)', re.I)   # re.I 表示忽略大小写
>>> m = pattern.match('Hello World Wide Web')
>>> print m                               # 匹配成功，返回一个 Match 对象
<_sre.SRE_Match object at 0x10bea83e8>
>>> m.group(0)                            # 返回匹配成功的整个子串
'Hello World'
>>> m.span(0)                             # 返回匹配成功的整个子串的索引
(0, 11)
>>> m.group(1)                            # 返回第一个分组匹配成功的子串
'Hello'
>>> m.span(1)                             # 返回第一个分组匹配成功的子串的索引
(0, 5)
>>> m.group(2)                            # 返回第二个分组匹配成功的子串
'World'
>>> m.span(2)                             # 返回第二个分组匹配成功的子串索引
(6, 11)
>>> m.groups()                            # 等价于 (m.group(1), m.group(2), ...)
('Hello', 'World')
>>> m.group(3)                            # 不存在第三个分组
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: no such group
findall
在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。
注意： match 和 search 是匹配一次 findall 匹配所有。
语法格式为：
findall(string[, pos[, endpos]])
参数：
string 待匹配的字符串。
pos 可选参数，指定字符串的起始位置，默认为 0。
endpos 可选参数，指定字符串的结束位置，默认为字符串的长度。
查找字符串中的所有数字：
实例
import re
 
pattern = re.compile(r'\d+')   # 查找数字
result1 = pattern.findall('runoob 123 google 456')
result2 = pattern.findall('run88oob123google456', 0, 10)
 
print(result1)
print(result2)
输出结果：
['123', '456']
['88', '12']

re.finditer
和 findall 类似，在字符串中找到正则表达式所匹配的所有子串，并把它们作为一个迭代器返回。
实例
import re
 
it = re.finditer(r"\d+","12a32bc43jf3") 
for match in it: 
    print (match.group() )
输出结果：
12 
32 
43 
3

re.split
split 方法按照能够匹配的子串将字符串分割后返回列表，它的使用形式如下：
re.split(pattern, string[, maxsplit=0, flags=0])
实例
>>>import re
>>> re.split('\W+', 'runoob, runoob, runoob.')
['runoob', 'runoob', 'runoob', '']
>>> re.split('(\W+)', ' runoob, runoob, runoob.') 
['', ' ', 'runoob', ', ', 'runoob', ', ', 'runoob', '.', '']
>>> re.split('\W+', ' runoob, runoob, runoob.', 1) 
['', 'runoob, runoob, runoob.']
 
>>> re.split('a*', 'hello world')   # 对于一个找不到匹配的字符串而言，split 不会对其作出分割
['hello world']

正则表达式修饰符 - 可选标志
正则表达式可以包含一些可选标志修饰符来控制匹配的模式。修饰符被指定为一个可选的标志。多个标志可以通过按位 OR(|) 它们来指定。如 re.I | re.M 被设置成 I 和 M 标志：
修饰符 描述
re.I 使匹配对大小写不敏感
re.L 做本地化识别（locale-aware）匹配
re.M 多行匹配，影响 ^ 和 $
re.S 使 . 匹配包括换行在内的所有字符
re.U 根据Unicode字符集解析字符。这个标志影响 \w, \W, \b, \B.
re.X 该标志通过给予你更灵活的格式以便你将正则表达式写得更易于理解。
--------------------------------------------------------------------------------
正则表达式模式
模式字符串使用特殊的语法来表示一个正则表达式：
字母和数字表示他们自身。一个正则表达式模式中的字母和数字匹配同样的字符串。
多数字母和数字前加一个反斜杠时会拥有不同的含义。
标点符号只有被转义时才匹配自身，否则它们表示特殊的含义。
反斜杠本身需要使用反斜杠转义。
由于正则表达式通常都包含反斜杠，所以你最好使用原始字符串来表示它们。模式元素(如 r'\t'，等价于 \\t )匹配相应的特殊字符。
下表列出了正则表达式模式语法中的特殊元素。如果你使用模式的同时提供了可选的标志参数，某些模式元素的含义会改变。
模式 描述
^ 匹配字符串的开头
$ 匹配字符串的末尾。
. 匹配任意字符，除了换行符，当re.DOTALL标记被指定时，则可以匹配包括换行符的任意字符。
[...] 用来表示一组字符,单独列出：[amk] 匹配 'a'，'m'或'k'
[^...] 不在[]中的字符：[^abc] 匹配除了a,b,c之外的字符。
re* 匹配0个或多个的表达式。
re+ 匹配1个或多个的表达式。
re? 匹配0个或1个由前面的正则表达式定义的片段，非贪婪方式
re{ n} 匹配n个前面表达式。例如，"o{2}"不能匹配"Bob"中的"o"，但是能匹配"food"中的两个o。
re{ n,} 精确匹配n个前面表达式。例如，"o{2,}"不能匹配"Bob"中的"o"，但能匹配"foooood"中的所有o。"o{1,}"等价于"o+"。"o{0,}"则等价于"o*"。
re{ n, m} 匹配 n 到 m 次由前面的正则表达式定义的片段，贪婪方式
a| b 匹配a或b
(re) 匹配括号内的表达式，也表示一个组
(?imx) 正则表达式包含三种可选标志：i, m, 或 x 。只影响括号中的区域。
(?-imx) 正则表达式关闭 i, m, 或 x 可选标志。只影响括号中的区域。
(?: re) 类似 (...), 但是不表示一个组
(?imx: re) 在括号中使用i, m, 或 x 可选标志
(?-imx: re) 在括号中不使用i, m, 或 x 可选标志
(?#...) 注释.
(?= re) 前向肯定界定符。如果所含正则表达式，以 ... 表示，在当前位置成功匹配时成功，否则失败。但一旦所含表达式已经尝试，匹配引擎根本没有提高；模式的剩余部分还要尝试界定符的右边。
(?! re) 前向否定界定符。与肯定界定符相反；当所含表达式不能在字符串当前位置匹配时成功。
(?> re) 匹配的独立模式，省去回溯。
\w 匹配数字字母下划线
\W 匹配非数字字母下划线
\s 匹配任意空白字符，等价于 [\t\n\r\f]。
\S 匹配任意非空字符
\d 匹配任意数字，等价于 [0-9]。
\D 匹配任意非数字
\A 匹配字符串开始
\Z 匹配字符串结束，如果是存在换行，只匹配到换行前的结束字符串。
\z 匹配字符串结束
\G 匹配最后匹配完成的位置。
\b 匹配一个单词边界，也就是指单词和空格间的位置。例如， 'er\b' 可以匹配"never" 中的 'er'，但不能匹配 "verb" 中的 'er'。
\B 匹配非单词边界。'er\B' 能匹配 "verb" 中的 'er'，但不能匹配 "never" 中的 'er'。
\n, \t, 等。 匹配一个换行符。匹配一个制表符, 等
\1...\9 匹配第n个分组的内容。
\10 匹配第n个分组的内容，如果它经匹配。否则指的是八进制字符码的表达式。
--------------------------------------------------------------------------------
正则表达式实例
字符匹配
实例 描述
python 匹配 "python".
字符类
实例 描述
[Pp]ython 匹配 "Python" 或 "python"
rub[ye] 匹配 "ruby" 或 "rube"
[aeiou] 匹配中括号内的任意一个字母
[0-9] 匹配任何数字。类似于 [0123456789]
[a-z] 匹配任何小写字母
[A-Z] 匹配任何大写字母
[a-zA-Z0-9] 匹配任何字母及数字
[^aeiou] 除了aeiou字母以外的所有字符
[^0-9] 匹配除了数字外的字符
特殊字符类
实例 描述
. 匹配除 "\n" 之外的任何单个字符。要匹配包括 '\n' 在内的任何字符，请使用象 '[.\n]' 的模式。
\d 匹配一个数字字符。等价于 [0-9]。
\D 匹配一个非数字字符。等价于 [^0-9]。
\s 匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。
\S 匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。
\w 匹配包括下划线的任何单词字符。等价于'[A-Za-z0-9_]'。
\W 匹配任何非单词字符。等价于 '[^A-Za-z0-9_]'。

正则表达式符号使用小总结：
1、[ ]：方括号。匹配需要的字符集合，如[1-3]或[123]都是匹配1、2或者3。
2、^：脱字符号。方括号中加入脱字符号，就是匹配未列出的所有其他字符，如[^a]匹配除a以外的所有其他字符。
3、\：反斜杠。和python字符串使用规则一样，可以匹配特殊字符本身，如\d表示匹配0到9的任意一个数字字符，而\\d则表示匹配\d本身。
4、*：星号。匹配前一个字符0到n次，如pytho*n可以匹配pythn、pytoon、pythooooon等。还有其它匹配重复字符的如？、+或{m,n}，其中{n,m}可以灵活使用，它表示匹配n次到m次。

任务
结合requests、re两者的内容爬取 https://movie.douban.com/top250 中的内容，要求抓取名次、影片名称、国家、导演等字段。
import requestsimport reimport csv# https://blog.csdn.net/bmjhappy/article/details/80512917 中文字符串匹配def movie_info(url):    headers = {     'User-Agent':"Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_8; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50"    }    res = requests.get(url, headers=headers)    ranks = re.findall(' <em class="">(.*?)</em>',res.text, re.S)    names = re.findall('<span class="title">([\u4e00-\u9fa5]+)</span>',res.text, re.S)    countries = re.findall('&nbsp;/&nbsp;([\u4e00-\u9fa5]+)&nbsp;/&nbsp;', res.text, re.S)    text = re.sub('导演: ',"",res.text)  # ：中文标点符号    directors = re.findall('<p class="">(.*?)&nbsp;&nbsp;', text, re.S)    scores = re.findall('<span class="rating_num" property="v:average">(.*?)</span>',res.text,re.S)    for rank,name,country,director,score in zip(ranks,names,countries,directors,scores):        writer.writerow([rank,name,country,director,score])if __name__ == '__main__':    file = open('E:/NLP/movie.csv','w+',encoding='utf-8',newline='')    writer = csv.writer(file)    writer.writerow(['rank','name','country','director','score'])    for i in range(0,250,25):        url = 'https://movie.douban.com/top250?start={}&filter='.format(i)        movie_info(url)
过程中遇到的问题：
本来是不打算加请求头，但是在查找资料时发现不加请求头可能会被阻止(其实不加好像也可以爬取)
爬取的数据我使用csv存储
过程中需要用到中文匹配的知识，一开始没什么头绪，后来查找博客发现使用[\u4e00-\u9fa5]代表中文字符
一开始只能抓到前20多部电影，后来经群里的同学提示，可以通过for循环来跨页爬取。
内容显示如下：

