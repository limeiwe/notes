为了安全起见，最好还是给打开的文件对象指定一个名字，这样在完成操作之后可以迅速关闭文件，防止一些无用的文件对象占用内存。举个例子，对文本文件读取：
file_object = open('thefile.txt') 
try: 
all_the_text = file_object.read( ) 
finally: 

file_object.close( )



[Python](http://lib.csdn.net/base/python)读写文件实际操作的五大步骤



**一、打开文件**

[Python](https://so.csdn.net/so/search?from=pc_blog_highlight&q=Python)读写文件在计算机语言中被广泛的应用，如果你想了解其应用的程序，以下的文章会给你详细的介绍相关内容，会你在以后的学习的过程中有所帮助，下面我们就详细介绍其应用程序。
代码如下：

```
 f = open("d:\test.txt", "w")  
```

说明：

第一个参数是文件名称，包括路径；第二个参数是打开的模式mode
'r'：只读（缺省。如果文件不存在，则抛出错误）
'w'：只写（如果文件不存在，则自动创建文件）
'a'：附加到文件末尾
'r+'：读写
如果需要以二进制方式打开文件，需要在mode后面加上字符"b"，比如"rb""wb"等
**二、读取内容**

```
 f.read(size)  
```

参数size表示读取的数量，可以省略。如果省略size参数，则表示读取文件所有内容。

```
 f.readline()  
```

读取文件一行的内容

```
 f.readlines()  
```

读取所有的行到数组里面[line1,line2,...lineN]。在避免将所有文件内容加载到内存中，这种方法常常使用，便于提高效率。

**三、写入文件
**

```
 f.write(string)  
```

将一个字符串写入文件，如果写入结束，必须在字符串后面加上"\n"，然后f.close()关闭文件
**四、文件中的内容定位**

```
 f.read() 
```

读取之后，文件指针到达文件的末尾，如果再来一次f.read()将会发现读取的是空内容，如果想再次读取全部内容，必须将定位指针移动到文件开始：

```
 f.seek(0)  
```

这个函数的格式如下（单位是bytes）：

```
 f.seek(offset, from_what)  
```

from_what表示开始读取的位置，offset表示从from_what再移动一定量的距离，比如f.seek(10, 3)表示定位到第三个字符并再后移10个字符。from_what值为0时表示文件的开始，它也可以省略，缺省是0即文件开头。下面给出一

```
 f = open('/tmp/workfile', 'r+')  f.write('0123456789abcdef')  f.seek(5) # Go to the 6th byte in the file  f.read(1)   '5'  f.seek (-3, 2) # Go to the 3rd byte before the end  f.read(1)  'd'   
```

**五、关闭文件释放资源**

文件操作完毕，一定要记得关闭文件f.close()，可以释放资源供其他程序使用

Python读写文件在计算机语言中被广泛的应用，如果你想了解其应用的程序，以下的文章会给你详细的介绍相关内容，会你在以后的学习的过程中有所帮助，下面我们就详细介绍其应用程序。











**一、python中对文件、文件夹操作时经常用到的os模块和shutil模块常用方法。**
1.得到当前工作目录，即当前Python脚本工作的目录路径: os.getcwd()
2.返回指定目录下的所有文件和目录名:os.listdir()
3.函数用来删除一个文件:os.remove()
4.删除多个目录：os.removedirs(r"c：\python")
5.检验给出的路径是否是一个文件：os.path.isfile()
6.检验给出的路径是否是一个目录：os.path.isdir()
7.判断是否是绝对路径：os.path.isabs()
8.检验给出的路径是否真地存:os.path.exists()
9.返回一个路径的目录名和文件名:os.path.split()   
例子：

代码如下:

os.path.split('/home/swaroop/byte/code/poem.txt') 结果：('/home/swaroop/byte/code', 'poem.txt')


10.分离扩展名：os.path.splitext()
11.获取路径名：os.path.dirname()
12.获取文件名：os.path.basename()
13.运行shell命令: os.system()
14.读取和设置环境变量:os.getenv() 与os.putenv()
15.给出当前平台使用的行终止符:os.linesep  Windows使用'\r\n'，Linux使用'\n'而Mac使用'\r'
16.指示你正在使用的平台：os.name    对于Windows，它是'nt'，而对于Linux/Unix用户，它是'posix'
17.重命名：os.rename(old， new)
18.创建多级目录：os.makedirs(r"c：\python\test")
19.创建单个目录：os.mkdir("test")
20.获取文件属性：os.stat(file)
21.修改文件权限与时间戳：os.chmod(file)
22.终止当前进程：os.exit()
23.获取文件大小：os.path.getsize(filename)
二、文件操作方法大全
1.os.mknod("test.txt")    创建空文件
2.fp = open("test.txt",w)   直接打开一个文件，如果文件不存在则创建文件
3.关于open 模式：

复制代码 代码如下:

r:以读方式打开文件，可读取文件信息。
w:以写方式打开文件，可向文件写入信息。如文件存在，则清空该文件，再写入新内容
a:以追加模式打开文件（即一打开文件，文件指针自动移到文件末尾），如果文件不存在则创建
b:以二进制模式打开文件，而不是以文本模式。该模式只对Windows或Dos有效，类Unix的文件是用二进制模式进行操作的。
r+：以读写模式打开
w+：以读写模式打开 (参见 w )
a+：以读写模式打开 (参见 a )
rb：以二进制读模式打开
wb：以二进制写模式打开 (参见 w )
ab：以二进制追加模式打开 (参见 a )
rb+：以二进制读写模式打开 (参见 r+ )
wb+：以二进制读写模式打开 (参见 w+ )
ab+：以二进制读写模式打开 (参见 a+ )



文件对象方法
f.close():关闭文件，记住用open()打开文件后一定要记得关闭它，否则会占用系统的可打开文件句柄数。
f.fileno():获得文件描述符，是一个数字
f.flush():刷新输出缓存
f.isatty():如果文件是一个交互终端，则返回True，否则返回False。
f.read([count]):读出文件，如果有count，则读出count个字节。
f.readline():读出一行信息。
f.readlines():
读出所有行，也就是读出整个文件的信息。
f.seek(offset[,where]):把文件指针移动到相对于where的offset位置。where为0表示文件开始处，这是默认值 ；1表示当前位置；2表示文件结尾。
f.tell():获得文件指针位置。
f.truncate([size]):截取文件，使文件的大小为size。
f.write(string):把string字符串写入文件。
f.writelines(list):把list中的字符串一行一行地写入文件，是连续写入文件，没有换行。


fp.read([size])           #size为读取的长度，以byte为单位
fp.readline([size])         #读一行，如果定义了size，有可能返回的只是一行的一部分
fp.readlines([size])        #把文件每一行作为一个list的一个成员，并返回这个list。其实它的内部是通过循环调用readline()来实现的。如果提供size参数，size是表示读取内容的总长，也就是说可能只读到文件的一部分。
fp.write(str)            #把str写到文件中，write()并不会在str后加上一个换行符
fp.writelines(seq)         #把seq的内容全部写到文件中(多行一次性写入)。这个函数也只是忠实地写入，不会在每行后面加上任何东西。
fp.close()             #关闭文件。python会在一个文件不用后自动关闭文件，不过这一功能没有保证，最好还是养成自己关闭的习惯。 如果一个文件在关闭后还对其进行操作会产生ValueError
fp.flush()             #把缓冲区的内容写入硬盘
fp.fileno()             #返回一个长整型的"文件标签"
fp.isatty()             #文件是否是一个终端设备文件（unix系统中的）
fp.tell()              #返回文件操作标记的当前位置，以文件的开头为原点
fp.next()              #返回下一行，并将文件操作标记位移到下一行。把一个file用于for … in file这样的语句时，就是调用next()函数来实现遍历的。
fp.seek(offset[,whence])      #将文件打操作标记移到offset的位置。这个offset一般是相对于文件的开头来计算的，一般为正数。但如果提供了whence参数就不一定了，whence可以为0表示从头开始计算，1表示以当前位置为原点计算。2表示以文件末尾为原点进行计算。需要注意，如果文件以a或a+的模式打开，每次进行写操作时，文件操作标记会自动返回到文件末尾。
fp.truncate([size])         #把文件裁成规定的大小，默认的是裁到当前文件操作标记的位置。如果size比文件的大小还要大，依据系统的不同可能是不改变文件，也可能是用0把文件补到相应的大小，也可能是以一些随机的内容加上去。
**三、目录操作方法大全**
1.创建目录
os.mkdir("file")          
2.复制文件：
shutil.copyfile("oldfile","newfile")    #oldfile和newfile都只能是文件
shutil.copy("oldfile","newfile")      #oldfile只能是文件夹，newfile可以是文件，也可以是目标目录
3.复制文件夹：
4.shutil.copytree("olddir","newdir")    #olddir和newdir都只能是目录，且newdir必须不存在
5.重命名文件（目录）
os.rename("oldname","newname")       #文件或目录都是使用这条命令
6.移动文件（目录）
shutil.move("oldpos","newpos")  
7.删除文件
os.remove("file")
8.删除目录
os.rmdir("dir")               #只能删除空目录
shutil.rmtree("dir")            #空目录、有内容的目录都可以删
9.转换目录
os.chdir("path")              #换路径







目录操作：
os.mkdir("file")          创建目录
复制文件：
shutil.copyfile("oldfile","newfile")    oldfile和newfile都只能是文件
shutil.copy("oldfile","newfile")      oldfile只能是文件夹，newfile可以是文件，也可以是目标目录
复制文件夹：
shutil.copytree("olddir","newdir")    olddir和newdir都只能是目录，且newdir必须不存在
重命名文件（目录）
os.rename("oldname","newname")    文件或目录都是使用这条命令
移动文件（目录）
shutil.move("oldpos","newpos")  
删除文件
os.remove("file")
删除目录
os.rmdir("dir")只能删除空目录
shutil.rmtree("dir")   空目录、有内容的目录都可以删
转换目录
os.chdir("path")  换路径