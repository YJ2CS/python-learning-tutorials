---
title: 10-0-python基础函数进阶
url: 10-0-python基础函数进阶
author: YJ2CS
avatar: '/custom/avatar.webp'
authorLink: YJ2CS.github.io
authorAbout: 愿青年摆脱了冷气，只是向前走！
authorDesc: 愿青年摆脱了冷气，只是向前走！
comments: true
categories:
  - 文章
tags:
  - 悦读
no-photos: 'https://random.52ecy.cn/randbg.php?size=1&rid-b808'
date: 2020-12-14T21:10:21.000Z
date updated: '2020-12-23T14:24:15+08:00'

---

# Python 函数进阶

## 一. 函数参数-动态参数

之前我们说过传参,如果我们在传参数的时候不很清楚有哪些的时候,或者说给一个函数传了很多参数,我们就要写很多,很麻烦怎么办呢,我们可以考虑使用动态参数

形参的第三种:动态参数

首先我们来回顾下位置参数

    def eat(a,b,c):

        print('我想吃%s%s%s'%(a,b,c))

    eat('大米饭','中米饭','小米饭')

现在有个问题,你们看我这体型也知道吃的不止这些,数量也没有写,这时我们就要用到动态参数　　

### 1.1 动态接收位置参数

在参数位置用*表示接受任意参数

    def eat(*args):

        print('我想吃',args)

    eat('大米饭','中米饭','小米饭')  # 收到的结果是一个tuple元祖

动态接收参数的时候要注意: 动态参数必须在位置参数后面

    def eat(*args,a,b):

        print('我想吃',args,a,b)

    eat('大米饭','中米饭','小米饭')

    结果:

    TypeError: eat() missing 2 required keyword-only arguments: 'a' and 'b'
    # eat函数在调用的时候发现缺少俩个位置参数没有进行传递

通过上述代码发现一个问题就是,我们明明给了多个参数,为什么还会提示参数未传递呢?

原因就是因为这个*在搞鬼  * _把所有的位置参数都给接受了,所有会报错.我们尝试着把a,b放在_的前面试试

    def eat(a,b,*args):

        print('我想吃',args,a,b)

    eat('大米饭','中米饭','小米饭')

    结果:

    我想吃 ('小米饭',) 大米饭 中米饭

动态接收参数的时候要注意:动态参数必须在位置参数后面

那默认值参数呢?

    def eat(a,b,c='白菜',*args):

        print('我想吃',a,b,c,args)

    eat('豆腐','粉条','猪肉','大葱')

    结果:

    我想吃 豆腐 粉条 猪肉 ('大葱',)  # 我们定义好的白菜没有生效,被猪肉给覆盖了

我们发现默认值参数写在动态参数前面,默认值的参数是不会生效的

    def eat(a,b,*args,c='白菜'):

        print('我想吃',a,b,args,c)

    eat('猪肉','粉条','豆腐','大葱')

    结果:

    我想吃 猪肉 粉条 ('豆腐', '大葱') 白菜  # 这样默认参数就生效了

这个时候如果你不给出关键字传参,那么你的默认值是永远都生效的　　

注意: 形参的顺序:  位置参数 , 动态参数 , 默认参数

### 1.2 动态接收关键字参数

在python中可以动态的位置参数,但是*这种情况只能接收位置参数无法接收关键字参数,在python中使用**来接收动态关键字参数

    def func(**kwargs):

        print(kwargs)     

    func(a=1, b=2, c=3)

    结果:

    {'a': 1, 'b': 2, 'c': 3}

动态关键字参数最后获取的是一个dict字典形式　　

顺序的问题, 在函数调用的时候, 如果先给出关键字参数, 则整个参数列表会报错.

    def func(a,b,c,d):

        print(a,b,c,d)

    func(1,2,c=3,4)

    结果:

      File "D:/python_object/path2/test.py", line 806

        func(1,2,c=3,4)              ^

    SyntaxError: positional argument follows keyword argument

关键参数必须要放在位置参数后边,由于实参是这个顺序,所以形参接收的时候也是这个顺序.也就是说位置参数必须在关键字参数前面.动态接收关键字参数也要在后面

最终顺序:

　　位置参数 > *args(动态位置参数)  > 默认值参数 > **kwargs(动态默认参数)

　　这四种参数可以任意的使用

如果想接收所有的参数:

    def func(*args,**kwargs):

        print(args,kwargs)

    func(1,23,5,a=1,b=6)

动态参数还可以这样传参:

    lst = [1,4,7]

    # 方法一

    def func(*args):

        print(args)

    func(lst[0],lst[1],lst[2])

    # 方法二

    def func(*args):

        print(args)

    func(*lst)  
    # 在实参的位置上用*将lst(可迭代对象)按照顺序打散

    # 在形参的位置上用*把收到的参数组合成一个元祖

字典也可以进行打散,不过需要**

    dic = {'a':1,'b':2}

    def func(**kwargs):

        print(kwargs)

    func(**dic)

## 二. 函数的注释

    def eat(food,drink):

        '''

        这里描述这个函数是做什么的.例如这函数eat就是吃

        :param food:  food这个参数是什么意思

        :param drink: drink这个参数是什么意思

        :return:  执行完这个函数想要返回给调用者什么东西

        '''

        print(food,drink)

    eat('麻辣烫','肯德基')

在外部查看函数的注释 函数名.__doc__

    print(eat.__doc__)  #函数名.__doc__

    结果:
        这里描述这个函数是做什么的.例如这函数eat就是吃

        :param food:  food这个参数是什么意思

        :param drink: drink这个参数是什么意思

        :return:  执行完这个函数想要返回给调用者什么东西

## 三 .名称空间

在python解释器开始执行之后, 就会在内存中开辟一个空间, 每当遇到一个变量的时候, 就把变量名和值之间的关系记录下来, 但是当遇到函数定义的时候, 解释器只是把函数名读入内存, 表示这个函数存在了,  至于函数内部的变量和逻辑, 解释器是不关心的. 也就是说一开始的时候函数只是加载进来, 仅此而已, 只有当函数被调用和访问的时候, 解释器才会根据函数内部声明的变量来进行开辟变量的内部空间. 随着函数执行完毕, 这些函数内部变量占用的空间也会随着函数执行完毕而被清空.

    def fun():   
        a = 10   
        print(a)
    fun()
    print(a)    # a不存在了已经..
    我们给存放名字和值的关系的空间起一个名字叫: 命名空间. 我们的变量在存储的时候就 是存储在这片空间中的.   

    命名空间分类:         

            1. 全局命名空间--> 我们直接在py文件中, 函数外声明的变量都属于全局命名空间       

            2. 局部命名空间--> 在函数中声明的变量会放在局部命名空间       

            3. 内置命名空间--> 存放python解释器为我们提供的名字, list, tuple, str, int这些都是内置命名空间　　

**加载顺序:**

　　1. 内置命名空间

　　2. 全局命名空间

       3. 局部命名空间(函数被执行的时候)

**取值顺序:**

       1. 局部命名空间

       2. 全局命名空间

       3. 内置命名空间
    a = 10
    def func():  
        a = 20   
        print(a)

    func()  # 20

作用域:  作用域就是作用范围, 按照生效范围来看分为  全局作用域  和   局部作用域

　　 全局作用域: 包含内置命名空间和全局命名空间. 在整个文件的任何位置都可以使用(遵循 从上到下逐⾏执行).

　　 局部作用域: 在函数内部可以使用.

作⽤域命名空间:

　　1. 全局作⽤用域:    全局命名空间 + 内置命名空间\
　　2.\
　　3. 局部作⽤用域:    局部命名空间

我们可以通过globals()函数来查看全局作⽤用域中的内容,也可以通过locals()来查看局部作 ⽤用域中的变量量和函数信息

    a = 10
    def func():   
        a = 40   
        b = 20   
        print("哈哈")   
        print(a, b)        
        print(globals())    # 打印全局作用域中的内容   
        print(locals())     # 打印局部作用域中的内容
    func()    　　

![](1-1548388202066.gif)

## 四. 函数的嵌套

1.  只要遇见了()就是函数的调用. 如果没有()就不是函数的调用

2.  函数的执行顺序
    def fun1():   
        print(111)  
    def fun2():   
        print(222)   
        fun1()   
    fun2()
    print(111)

![1548388589142](1548388589142.png)

    def fun2():   
        print(222)   
        def fun3():       
            print(666)   
        print(444)   
        fun3()   
        print(888)
    print(33)
    fun2()
    print(555)

![1548388743478](1548388743478.png)

## 五 .gloabal、nonlocal

首先我们写这样一个代码, 首先在全局声明一个变量, 然后再局部调用这个变量, 并改变这 个变量的值

    a = 100
    def func():   
        global a    # 加了个global表示不再局部创建这个变量了. 而是直接使用全局的a   
        a = 28   
    print(a)
    func()
    print(a)

global表示. 不再使用局部作用域中的内容了. 而改用全局作用域中的变量

### 5.1global 宗旨

在函数内部修改全局的变量,如果全局中不存在就创建一个变量

    lst = ["麻花藤", "刘嘉玲", "詹姆斯"]
    def func():   
        lst.append("⻢云")   
        # 对于可变数据类型可以直接进⾏访问
    　　 print(lst)
    func()
    print(lst)

### 5.2 nonlocal宗旨

nonlocal 只修改上一层变量,如果上一层中没有变量就往上找一层,只会找到函数的最外层,不会找到全局进行修改

    a = 10
    def func1():   
        a = 20   
        def func2():
            nonlocal a       
            a = 30       
            print(a)  
        func2()   
        print(a)
    func1()

    结果:
    加了nonlocal
    30
    30

    不加nonlocal
    30
    20   

再看, 如果嵌套了很多层, 会是一种什么效果:

    a = 1
    def fun_1():   
        a = 2   
        def fun_2():       
            nonlocal a       
            a = 3       
            def fun_3():           
                a = 4           
                print(a)       
            print(a)       
            fun_3()       
            print(a)   
        print(a)   
        fun_2()   
        print(a)
    print(a)
    fun_1()
    print(a)

这样的程序如果能分析明白. 那么作用域, global, nonlocal就没问题了
