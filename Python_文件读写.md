# 文件读写

## re 模块

```python
# 最基本用法
虽然在 Python 中使用正则表达式有几个步骤，但每一步都相当简单。
1．用 import re 导入正则表达式模块。
2．用 re.compile()函数创建一个 Regex 对象（记得使用原始字符串）。
3．向 Regex 对象的 search()方法传入想查找的字符串。它返回一个 Match 对象。
4．调用 Match 对象的 group()方法，返回实际匹配文本的字符串。

import re

phoneNumRegex = re.compile(r'\d\d\d-\d\d\d-\d\d\d\d')
mo = phoneNumRegex.search('My number is 415-555-4242.')
print('Phone number found: ' + mo.group())
-->> Phone number found: 415-555-4242
# re.compile() 返回 Regex 对象
# search() 返回 Match 对象
# group() 返回匹配结果
```

```python
>>> phoneNumRegex = re.compile(r'(\d\d\d)-(\d\d\d-\d\d\d\d)')
>>> mo = phoneNumRegex.search('My number is 415-555-4242.')
>>> mo.group(1)
'415'
>>> mo.group(2)
'555-4242'
>>> mo.group(0)
'415-555-4242'
>>> mo.group()
'415-555-4242'

>>> mo.groups()   # groups()
('415', '555-4242')
>>> areaCode, mainNumber = mo.groups()
>>> print(areaCode)
415
>>> print(mainNumber)
555-4242
```

```python
>>> greedyHaRegex = re.compile(r'(Ha){3,5}')
>>> mo1 = greedyHaRegex.search('HaHaHaHaHa')
>>> mo1.group()
'HaHaHaHaHa'
>>> nongreedyHaRegex = re.compile(r'(Ha){3,5}?')
>>> mo2 = nongreedyHaRegex.search('HaHaHaHaHa')
>>> mo2.group()
'HaHaHa'
# 贪心与非贪心
```

```python
>>> phoneNumRegex = re.compile(r'\d\d\d-\d\d\d-\d\d\d\d')
>>> mo = phoneNumRegex.search('Cell: 415-555-9999 Work: 212-555-0000')
>>> mo.group()
'415-555-9999'

>>> phoneNumRegex = re.compile(r'\d\d\d-\d\d\d-\d\d\d\d') # has no groups
>>> phoneNumRegex.findall('Cell: 415-555-9999 Work: 212-555-0000')
['415-555-9999', '212-555-0000']
```

```python
# 替换字符串
namesRegex = re.compile(r'Agent \w+')
>>> namesRegex.sub('CENSORED', 'Agent Alice gave the secret documents to Agent Bob.')
'CENSORED gave the secret documents to CENSORED.'

>>> agentNamesRegex = re.compile(r'Agent (\w)\w*')
>>> agentNamesRegex.sub(r'\1****', 'Agent Alice told Agent Carol that Agent
Eve knew Agent Bob was a double agent.')
A**** told C**** that E**** knew B**** was a double agent.'
```

```python
re.VERBOSE 来编写注释，还希望使用
re.IGNORECASE 来忽略大小写
so = re.compile('foo', re.IGNORECASE | re.DOTALL | re.VERBOSE)

```

### 表达式全集

|     字符     |                             描述                             |
| :----------: | :----------------------------------------------------------: |
|      \       | 将下一个字符标记为一个特殊字符、或一个原义字符、或一个向后引用、或一个八进制转义符。 |
|      ^       |                  匹配输入字符串的开始位置。                  |
|      $       |                  匹配输入字符串的结束位置。                  |
|      *       |                匹配前面的子表达式零次或多次。                |
|      +       |                匹配前面的子表达式一次或多次。                |
|      ?       |                匹配前面的子表达式零次或一次。                |
|    {*n*}     |             *n*是一个非负整数。匹配确定的*n*次。             |
|    {*n*,}    |              *n*是一个非负整数。至少匹配*n*次。              |
|  {*n*,*m*}   | *m*和*n*均为非负整数，其中*n*<=*m*。最少匹配*n*次且最多匹配*m*次。 |
|      ?       | 当该字符紧跟在任何一个其他限制符（*,+,?，{*n*}，{*n*,}，{*n*,*m*}）后面时，匹配模式是非贪婪的。 |
|      .       |             匹配除“`\`*`n`*”之外的任何单个字符。             |
|  (pattern)   | 匹配pattern并获取这一匹配。所获取的匹配可以从产生的Matches集合得到，在VBScript中使用SubMatches集合，在JScript中则使用$0…$9属性。要匹配圆括号字符，请使用“`\(`”或“`\)`”。 |
| (?:pattern)  | 匹配pattern但不获取匹配结果，也就是说这是一个非获取匹配，不进行存储供以后使用。这在使用或字符“`(|)`”来组合一个模式的各个部分是很有用。例如“`industr(?:y|ies)`”就是一个比“`industry|industries`”更简略的表达式。 |
| (?=pattern)  | 正向肯定预查，在任何匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹配不需要获取供以后使用。例如，“`Windows(?=95|98|NT|2000)`”能匹配“`Windows2000`”中的“`Windows`”，但不能匹配“`Windows3.1`”中的“`Windows`”。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始。 |
| (?!pattern)  | 正向否定预查，在任何不匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹配不需要获取供以后使用。例如“`Windows(?!95|98|NT|2000)`”能匹配“`Windows3.1`”中的“`Windows`”，但不能匹配“`Windows2000`”中的“`Windows`”。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始 |
| (?<=pattern) | 反向肯定预查，与正向肯定预查类拟，只是方向相反。例如，“`(?<=95|98|NT|2000)Windows`”能匹配“`2000Windows`”中的“`Windows`”，但不能匹配“`3.1Windows`”中的“`Windows`”。 |
| (?<!pattern) | 反向否定预查，与正向否定预查类拟，只是方向相反。例如“`(?<!95|98|NT|2000)Windows`”能匹配“`3.1Windows`”中的“`Windows`”，但不能匹配“`2000Windows`”中的“`Windows`”。 |
|     x\|y     |                          匹配x或y。                          |
|    [xyz]     |             字符集合。匹配所包含的任意一个字符。             |
|    [^xyz]    |             负值字符集合。匹配未包含的任意字符。             |
|    [a-z]     |             字符范围。匹配指定范围内的任意字符。             |
|    [^a-z]    |       负值字符范围。匹配任何不在指定范围内的任意字符。       |
|      \b      | 匹配一个单词边界，也就是指单词和空格间的位置。例如，“`er\b`”可以匹配“`never`”中的“`er`”，但不能匹配“`verb`”中的“`er`”。 |
|      \B      | 匹配非单词边界。“`er\B`”能匹配“`verb`”中的“`er`”，但不能匹配“`never`”中的“`er`”。 |
|     \cx      | 匹配由x指明的控制字符。例如，\cM匹配一个Control-M或回车符。x的值必须为A-Z或a-z之一。否则，将c视为一个原义的“`c`”字符。 |
|      \d      |               匹配一个数字字符。等价于[0-9]。                |
|      \D      |              匹配一个非数字字符。等价于[^0-9]。              |
|      \f      |              匹配一个换页符。等价于\x0c和\cL。               |
|      \n      |              匹配一个换行符。等价于\x0a和\cJ。               |
|      \r      |              匹配一个回车符。等价于\x0d和\cM。               |
|      \s      | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于[ \f\n\r\t\v]。 |
|      \S      |          匹配任何非空白字符。等价于[^ \f\n\r\t\v]。          |
|      \t      |              匹配一个制表符。等价于\x09和\cI。               |
|      \v      |            匹配一个垂直制表符。等价于\x0b和\cK。             |
|      \w      |    匹配包括下划线的任何单词字符。等价于“`[A-Za-z0-9_]`”。    |
|      \W      |        匹配任何非单词字符。等价于“`[^A-Za-z0-9_]`”。         |
|    \x*n*     | 匹配*n*，其中*n*为十六进制转义值。十六进制转义值必须为确定的两个数字长。例如，“`\x41`”匹配“`A`”。“`\x041`”则等价于“`\x04&1`”。正则表达式中可以使用ASCII编码。. |
|    \*num*    | 匹配*num*，其中*num*是一个正整数。对所获取的匹配的引用。例如，“`(.)\1`”匹配两个连续的相同字符。 |
|     \*n*     | 标识一个八进制转义值或一个向后引用。如果\*n*之前至少*n*个获取的子表达式，则*n*为向后引用。否则，如果*n*为八进制数字（0-7），则*n*为一个八进制转义值。 |
|    \*nm*     | 标识一个八进制转义值或一个向后引用。如果\*nm*之前至少有*nm*个获得子表达式，则*nm*为向后引用。如果\*nm*之前至少有*n*个获取，则*n*为一个后跟文字*m*的向后引用。如果前面的条件都不满足，若*n*和*m*均为八进制数字（0-7），则\*nm*将匹配八进制转义值*nm*。 |
|    \*nml*    | 如果*n*为八进制数字（0-3），且*m和l*均为八进制数字（0-7），则匹配八进制转义值*nm*l。 |
|    \u*n*     | 匹配*n*，其中*n*是一个用四个十六进制数字表示的Unicode字符。例如，\u00A9匹配版权符号（©）。 |



### 常用正则表达式

|         用户名          | /^[a-z0-9_-]{3,16}$/                                         |
| :---------------------: | ------------------------------------------------------------ |
|          密码           | /^[a-z0-9_-]{6,18}$/                                         |
|       十六进制值        | /^#?([a-f0-9]{6}\|[a-f0-9]{3})$/                             |
|        电子邮箱         | /^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/ /^[a-z\d]+(\.[a-z\d]+)*@([\da-z](-[\da-z])?)+(\.{1,2}[a-z]+)+$/ |
|           URL           | /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/ |
|         IP 地址         | /((2[0-4]\d\|25[0-5]\|[01]?\d\d?)\.){3}(2[0-4]\d\|25[0-5]\|[01]?\d\d?)/ /^(?:(?:25[0-5]\|2[0-4][0-9]\|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]\|2[0-4][0-9]\|[01]?[0-9][0-9]?)$/ |
|        HTML 标签        | /^<([a-z]+)([^<]+)*(?:>(.*)<\/\1>\|\s+\/>)$/                 |
|     删除代码\\注释      | (?<!http:\|\S)//.*$                                          |
| Unicode编码中的汉字范围 | /^[\u2E80-\u9FFF]+$/                                         |





## os 模块

```python
import re 
os.chdir('tennis') 		# 更换目录
os.getcwd()				# 获取当前目录
os.listdir()			# 列出当前或指定目录下的	文件和文件夹
os.makedirs('newFoild') # 新建文件夹
```

```python
>>> os.chdir('tennis')		# 更改所在目录，‘相对路径’
>>> os.getcwd()				# 获取当前目录
'E:\\Desktop\\tennis'

>>> os.chdir(r'C:\Users\rov')	# 更改所在目录，‘绝对路径’
>>> os.getcwd()
'C:\\Users\\rov'

>>> os.getcwd()
'E:\\Desktop\\tennis'
>>> os.listdir()
['Dress', 'handbook', 'notes']
>>> os.makedirs('newFoild') # 新建文件夹
>>> os.listdir()			# Linux --> ls/win --> dir
['Dress', 'handbook', 'newFoild', 'notes']
```

## os.path模块

```python
import os
os.path.join()						
os.path.abspath()			# 返回参数的绝对路径的字符串	
os.path.isabs()				# 判断是否是绝对路径
os.path.relpath(path, start)# 将返回从 start 路径到 path 的相对路径
os.path.basename(path)		# 返回文件名【基本名称】
os.path.dirname(path)		# 返回文件坐在的目录【目录名称】
os.path.split(path)			# 分割目录名称与基本名称
calcFilePath.split(os.path.sep)# 分割文件路径上的所有目录名称
os.path.getsize(path)		# 获取文件的大小
os.path.exists(path) 		# 判断文件或文件夹是否存在
os.path.isdir(path) 		# 判断文件夹是否存在
os.path.isfile(path)		# 判断文件是否存在
```

```python
os.path.join()函数来做这件事很简单。
如果将单个文件和路径上的文件夹名称的字符串传递给它，
os.path.join()就会返回一个文件路径的字符串，
包含正确的路径分隔符
>>> import os
>>> os.path.join('usr', 'bin', 'spam')
'usr\\bin\\spam'

>>> os.path.abspath('.')	#将返回参数的绝对路径的字符串		
'C:\\Python34'
>>> os.path.abspath('.\\Scripts')
'C:\\Python34\\Scripts'
>>> os.path.isabs('.')		# 判断是否是绝对路径
False
>>> os.path.isabs(os.path.abspath('.'))
True

# os.path.relpath(path, start)
# 将返回从 start 路径到 path 的相对路径的字符串。
>>> os.path.relpath('C:\\Windows', 'C:\\')
'Windows'
>>> os.path.relpath('C:\\Windows', 'C:\\spam\\eggs')
'..\\..\\Windows'
>>> os.getcwd()
'C:\\Python34'

>>> path = 'C:\\Windows\\System32\\calc.exe'
>>> os.path.basename(path)	# 基本名称
'calc.exe'
>>> os.path.dirname(path)	# 目录名称
'C:\\Windows\\System32'

>>> calcFilePath = 'C:\\Windows\\System32\\calc.exe'
>>> os.path.split(calcFilePath)	# 分割目录名称与基本名称
('C:\\Windows\\System32', 'calc.exe')
>>> calcFilePath.split(os.path.sep) # 分割路径上的每个文件夹
['C:', 'Windows', 'System32', 'calc.exe']

>>> os.path.getsize('C:\\Windows\\System32\\calc.exe')
776192
```

```python
# 检查路径的有效性【是否存在】
>>> os.path.exists('C:\\Windows')   # 文件或文件夹
True
>>> os.path.exists('C:\\some_made_up_folder')
False
>>> os.path.isdir('C:\\Windows\\System32') # 文件
True
>>> os.path.isfile('C:\\Windows\\System32')
False
>>> os.path.isdir('C:\\Windows\\System32\\calc.exe') # 文件夹
False
>>> os.path.isfile('C:\\Windows\\System32\\calc.exe')
True
```

## 文件读写过程



```python
# 1.打开文件
hello = open('C:\\Users\\your_home_folder\\hello.txt')
# 2.读取文件
helloContent = hello.read()
helloContent = hello.readlines()
# 3.写入文件
>>> baconFile = open('bacon.txt', 'w')# 覆盖写
>>> baconFile.write('Hello world!\n')
13
>>> baconFile.close()
>>> baconFile = open('bacon.txt', 'a')# 追加写
>>> baconFile.write('Bacon is not a vegetable.')
25
>>> baconFile.close()
>>> baconFile = open('bacon.txt')
>>> content = baconFile.read()

# 4.关闭文件
>>> baconFile.close()
>>> print(content)
Hello world!
Bacon is not a vegetable.

```

## shelve模块

利用 `shelve` 模块，你可以将 `Python` 程序中的变量保存到二进制的 `shelf` 文件中。就像字典一样，shelf 值有 keys()和 values()方法。

```python
open()
read()/write()
close()
```

```python
>>> import shelve
>>> shelfFile = shelve.open('mydata')
>>> cats = ['Zophie', 'Pooka', 'Simon']
>>> shelfFile['cats'] = cats
>>> shelfFile.close()

>>> shelfFile = shelve.open('mydata')
>>> type(shelfFile)
<class 'shelve.DbfilenameShelf'>
>>> shelfFile['cats']
['Zophie', 'Pooka', 'Simon']
>>> shelfFile.close()

>>> shelfFile = shelve.open('mydata')
>>> list(shelfFile.keys())
['cats']
>>> list(shelfFile.values())
[['Zophie', 'Pooka', 'Simon']]
>>> shelfFile.close()
```

在 `Windows` 上运行前面的代码，你会看到在当前工作目录下有 3 个新文件： `mydata.bak`、`mydata.dat` 和 `mydata.dir`。

## pprint模块

`pprint.pprint()`函数将列表或字典中的内容“漂 亮打印”到屏幕，而 `pprint.pformat()`函数将返回同样的文本字符串，但不是打印它。

```python
>>> import pprint
>>> cats = [{'name': 'Zophie', 'desc': 'chubby'}, {'name': 'Pooka', 'desc': 'fluffy'}]
>>> pprint.pformat(cats)
"[{'desc': 'chubby', 'name': 'Zophie'}, {'desc': 'fluffy', 'name': 'Pooka'}]"
>>> fileObj = open('myCats.py', 'w')
>>> fileObj.write('cats = ' + pprint.pformat(cats) + '\n')
83
>>> fileObj.close()
```

```python
>>> import myCats 不是（标准和第三方）库，是导入刚刚创建的文件myCats.py
>>> myCats.cats
[{'name': 'Zophie', 'desc': 'chubby'}, {'name': 'Pooka', 'desc': 'fluffy'}]
>>> myCats.cats[0]
{'name': 'Zophie', 'desc': 'chubby'}
>>> myCats.cats[0]['name']
'Zophie'
```

## shutil 模块

`shutil`（或称为 `shell` 工具）模块中包含一些函数，让你在 `Python` 程序中复制、 移动、改名和删除文件。要使用 `shutil` 的函数，首先需要 `import shutil`。

```python
import shutil
shutil.copy() 							# 复制一个文件
shutil.copytree(source, destination) 	# 复制一个文件夹
shutil.move(source, destination) 		# 移动文件夹/文件[可改名]
# 将删除 path 处的文件夹。
os.rmdir(path)
# 将删除 path 处的文件夹，它包含的所有文件和文件夹都会被删除。
shutil.rmtree(path)
```

```python
>>> import shutil, os
>>> os.chdir('C:\\')
# 将文件 C:\spam.txt 复制到文件夹 C:\delicious
>>> shutil.copy('C:\\spam.txt', 'C:\\delicious')
'C:\\delicious\\spam.txt'
# 也将文件 C:\eggs.txt 复制到文件夹 C:\delicious,相当于改名
>>> shutil.copy('eggs.txt', 'C:\\delicious\\eggs2.txt')
'C:\\delicious\\eggs2.txt'

>>> import shutil, os
>>> os.chdir('C:\\')
# 将路径 source 处的文件夹，包括它的所有文件和子文件夹，
# 复制到路径 destination 处的文件夹。不存在则创建一个新的文件夹
>>> shutil.copytree('C:\\bacon', 'C:\\bacon_backup')
'C:\\bacon_backup'

# 构成目的地的文件夹必须已经存在，否则 Python 会抛出异常
>>> shutil.move('C:\\bacon.txt', 'C:\\eggs\\new_bacon.txt')
'C:\\eggs\\new_bacon.txt'
```



## send2trash 模块

```python
• 用 os.unlink(path)将删除 path 处的文件。
• 调用 os.rmdir(path)将删除 path 处的文件夹。该文件夹必须为空，其中没有任何文件和文件夹。
• 调用 shutil.rmtree(path)将删除 path 处的文件夹，它包含的所有文件和文件夹都会被删除。
在程序中使用这些函数时要小心！可以第一次运行程序时，注释掉这些调用，并且加上 print()调用，显示会被删除的文件。


因为 Python 内建的 shutil.rmtree()函数不可恢复地删除文件和文件夹，所以 用起来可能有危险。删除文件和文件夹的更好方法，是使用第三方的 send2trash 模块。因为它会将文件夹和文件发送到计算机的垃圾箱或回收站，而不是永久删除它们。
```

```python
>>> import send2trash
>>> baconFile = open('bacon.txt', 'a') # creates the file
>>> baconFile.write('Bacon is not a vegetable.')
25
>>> baconFile.close()
>>> send2trash.send2trash('bacon.txt')
```

## 遍历目录树

`os.walk()`函数被传入一个字符串值，即一个文件夹的路径。你可以在一个 `for` 循环语句中使用 `os.walk()`函数，遍历目录树，就像使用 `range()`函数遍历一个范围的 数字一样。不像 `range()`，`os.walk()`在循环的每次迭代中，返回 3 个值： 

1．当前文件夹名称的字符串。 

2．当前文件夹中子文件夹的字符串的列表。 

3．当前文件夹中文件的字符串的列表。

```python
import os
for folderName,subfolders,filenames in os.walk(r'E:\Desktop\入侵检测'):
    #   os.walk()返回字符串的列表，
    #   保存在 subfolder 和 filename 变量中，
    #	所以你可以在它们自己的 for 循环中使用这些列表
    #   folderName          文件夹
    #   subfolders          子文件夹
    #   filenames           文件名
    print('The current folder is ' + folderName)
    for subfolder in subfolders:
        print('SUBFOLDER OF ' + folderName + ':'+subfolder)
    for filename in filenames:
        print('FILE INSIDE ' + folderName + ':' + filename)
    print('')
```

```python
# 输出
The current folder is E:\Desktop\入侵检测
SUBFOLDER OF E:\Desktop\入侵检测:漏洞扫描（实验一）
FILE INSIDE E:\Desktop\入侵检测:10.84.85.103.ses
FILE INSIDE E:\Desktop\入侵检测:入侵检测模板实验模板.docx
FILE INSIDE E:\Desktop\入侵检测:分公司个人.ses
FILE INSIDE E:\Desktop\入侵检测:来了.ses
FILE INSIDE E:\Desktop\入侵检测:漏洞扫描（实验一）.rar

The current folder is E:\Desktop\入侵检测\漏洞扫描（实验一）
FILE INSIDE E:\Desktop\入侵检测\漏洞扫描（实验一）:SoftonicDownloader_for_shadow-security-scanner.exe
FILE INSIDE E:\Desktop\入侵检测\漏洞扫描（实验一）:SSS.exe
FILE INSIDE E:\Desktop\入侵检测\漏洞扫描（实验一）:使用_SSS_进行漏洞扫描.pdf
FILE INSIDE E:\Desktop\入侵检测\漏洞扫描（实验一）:入侵检测实验报告.docx
FILE INSIDE E:\Desktop\入侵检测\漏洞扫描（实验一）:试验说明.doc
```

## zipfile 模块

**读取ZIP 文件**

```python
>>> import zipfile, os
>>> os.chdir('C:\\') # move to the folder with example.zip
# 要读取 ZIP 文件的内容，首先必须创建一个 ZipFile 对象
# 调用 zipfile.ZipFile()函数，向它传入一个字符串，
# 表示.zip 文件的文件名。
>>> exampleZip = zipfile.ZipFile('example.zip')

# namelist()方法，
# 返回 ZIP 文件中包含的所有文件和文件夹的字符串的列表。
>>> exampleZip.namelist()
['spam.txt', 'cats/', 'cats/catnames.txt', 'cats/zophie.jpg']

# getinfo()方法，返回一个关于特定文件的 ZipInfo 对象。
# ZipInfo 对象则保存该归档文件中每个文件的有用信息。
>>> spamInfo = exampleZip.getinfo('spam.txt')

# file_size和 compress_size，
# 它们分别表示原来文件大小和压缩后文件大小
>>> spamInfo.file_size
13908
>>> spamInfo.compress_size
3828

# ZipInfo 对象则保存该归档文件中每个文件的有用信息。
>>> 'Compressed file is %sx smaller!' % (round(spamInfo.file_size / spamInfo
.compress_size, 2))
'Compressed file is 3.63x smaller!'
>>> exampleZip.close()
```

**解压缩**

```python
>>> import zipfile, os
>>> os.chdir('C:\\') # move to the folder with example.zip
>>> exampleZip = zipfile.ZipFile('example.zip')

# extractall()方法从 ZIP 文件中解压缩所有文件和文件夹，
# 放到当前工作目录中。
# 如果传递给 extractall()方法的文件夹不存在，它会被创建
>>> exampleZip.extractall()
>>> exampleZip.close()
```

```python
# extract()方法从 ZIP 文件中解压缩单个文件
>>> exampleZip.extract('spam.txt')
'C:\\spam.txt'

# 可以向 extract()传递第二个参数，将文件解压缩到指定的文件夹，
# 而不是当前工作目录,如果第二个参数指定的文件夹不存在，Python 就会创建它。
>>> exampleZip.extract('spam.txt', 'C:\\some\\new\\folders')
'C:\\some\\new\\folders\\spam.txt'
>>> exampleZip.close()
```

**创建和添加ZIP文件**

```python
>>> import zipfile

# 要创建你自己的压缩 ZIP 文件，必须以“写模式”打开 ZipFile 对象，即传入'w'
>>> newZip = zipfile.ZipFile('new.zip', 'w')

# 第二个参数是“压缩类型”参数，它告诉计算机使用怎样的算法来压缩文件。
# 可以总是将这个值设置为 zipfile.ZIP_DEFLATED
#（这指定了 deflate 压缩算法，它对各种类型的数据都很有效）
>>> newZip.write('spam.txt', compress_type=zipfile.ZIP_DEFLATED)
>>> newZip.close()
```



























