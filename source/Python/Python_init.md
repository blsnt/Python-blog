## 第十章 文件处理

### 基本流程

 应用程序运行过程中产生的数据最先都是存放于内存中的，若想永久保存下来，必须要保存于硬盘中。应用程序若想操作硬件必须通过操作系统，而文件就是操作系统提供给应用程序来操作硬盘的虚拟概念，用户或应用程序对文件的操作，就是向操作系统发起调用，然后由操作系统完成对硬盘的具体操作。

有了文件的概念，我们无需再去考虑操作硬盘的细节，只需要关注操作文件的流程：

```python
# 1. 打开文件，由应用程序向操作系统发起系统调用open(...)，操作系统打开该文件，对应一块硬盘空间，并返回一个文件对象赋值给一个变量f
f=open('a.txt','r',encoding='utf-8') #默认打开模式就为r

# 2. 调用文件对象下的读/写方法，会被操作系统转换为读/写硬盘的操作
data=f.read()

# 3. 向操作系统发起关闭文件的请求，回收系统资源
f.close()
```

### 资源回收与with上下文管理

打开一个文件包含两部分资源：应用程序的变量f和操作系统打开的文件。在操作完毕一个文件时，必须把与该文件的这两部分资源全部回收，回收方法为：

```python
1、f.close() #回收操作系统打开的文件资源
2、del f #回收应用程序级的变量
```

其中del f一定要发生在f.close()之后，否则就会导致操作系统打开的文件无法关闭，白白占用资源， 而python自动的垃圾回收机制决定了我们无需考虑del f，这就要求我们，在操作完毕文件后，一定要记住f.close()，虽然我们如此强调，但是大多数读者还是会不由自主地忘记f.close()，考虑到这一点，python提供了with关键字来帮我们管理上下文

```python
# 1、在执行完子代码块后，with 会自动执行f.close()
with open('a.txt','w') as f:
    pass 

# 2、可用用with同时打开多个文件，用逗号分隔开即可
with open('a.txt','r') as read_f,open('b.txt','w') as write_f:  
    data = read_f.read()
    write_f.write(data)
```

### 指定操作文本文件的字符编码

```python
f = open(...)是由操作系统打开文件，如果打开的是文本文件，会涉及到字符编码问题，如果没有为open指定编码，那么打开文本文件的默认编码很明显是操作系统说了算了，操作系统会用自己的默认编码去打开文件，在windows下是gbk，在linux下是utf-8。
这就用到了上节课讲的字符编码的知识：若要保证不乱码，文件以什么方式存的，就要以什么方式打开。

f = open('a.txt','r',encoding='utf-8')
```

### 文件的操作模式

控制文件读写操作的模式

```python
r(默认的)：只读
w：只写
a：只追加写
```

#### r 模式的使用

```python
# r只读模式: 在文件不存在时则报错,文件存在文件内指针直接跳到文件开头
 with open('a.txt',mode='r',encoding='utf-8') as f:
     res=f.read() # 会将文件的内容由硬盘全部读入内存，赋值给res

# 小练习：实现用户认证功能
 inp_name=input('请输入你的名字: ').strip()
 inp_pwd=input('请输入你的密码: ').strip()
 with open(r'db.txt',mode='r',encoding='utf-8') as f:
     for line in f:
         # 把用户输入的名字与密码与读出内容做比对
         u,p=line.strip('\n').split(':')
         if inp_name == u and inp_pwd == p:
             print('登录成功')
             break
     else:
         print('账号名或者密码错误')
```

#### w模式的使用

```python
# w只写模式: 在文件不存在时会创建空文档,文件存在会清空文件,文件指针跑到文件开头
with open('b.txt',mode='w',encoding='utf-8') as f:
    f.write('你好\n')
    f.write('我好\n') 
    f.write('大家好\n')
    f.write('111\n222\n333\n')
#强调：
# 1 在文件不关闭的情况下,连续的写入，后写的内容一定跟在前写内容的后面
# 2 如果重新以w模式打开文件，则会清空文件内容
```

#### a 模式的使用

```python
# a只追加写模式: 在文件不存在时会创建空文档,文件存在会将文件指针直接移动到文件末尾
 with open('c.txt',mode='a',encoding='utf-8') as f:
     f.write('44444\n')
     f.write('55555\n')
#强调 w 模式与 a 模式的异同：
# 1 相同点：在打开的文件不关闭的情况下，连续的写入，新写的内容总会跟在前写的内容之后
# 2 不同点：以 a 模式重新打开文件，不会清空原文件内容，会将文件指针直接移动到文件末尾，新写的内容永远写在最后

# 小练习：实现注册功能:
 name=input('username>>>: ').strip()
 pwd=input('password>>>: ').strip()
 with open('db1.txt',mode='a',encoding='utf-8') as f:
     info='%s:%s\n' %(name,pwd)
     f.write(info)
```



```python
控制文件读写操作的模式
```

