title: python的命名空间
author: hero576
tags:
  - python
categories:
  - python
date: 2017-08-16 19:51:00
---
## 命名空间的概念

C++会用到`std::cout`这样的用法，定义cout的命名空间，使用`using namespace std;`可以简化std::cout的写法，命名空间的使用可以避免不同的作用域之间函数重名的冲突问题。

## python的命名空间

python中也有命名空间，是用来记录变量轨迹的。命名空间是一个 字典，它的键就是变量名，它的值就是那些变量的值。
在一个 Python 程序中的任何一个地方，都存在几个可用的命名空间。这四类命名空间可以简记为 LEGB:  
1. **局部命名空间（local）：**指的是一个函数或者一个类所定义的名称空间；包括函数的参数、局部变量、类的属性等。
2. **闭包命名空间（enclosing function）：**闭包函数 的名称空间（Python 3 引入）。
3. **全局命名空间（global）：**读入一个模块（也即一个.py文档）后产生的名称空间。
4. **内建命名空间（builtin）：**Python 解释器启动时自动载入__built__模块后所形成的名称空间；诸如 str/list/dict...等内置对象的名称就处于这里。

## 命名空间查找顺序

同样的标识符在各层命名空间中可以被重复使用而不会发生冲突，但 Python 寻找一个标识符的过程总是从当前层开始逐层往上找，直到首次找到这个标识符为止：
1. 局部命名空间：特指当前函数或类的方法。如果函数定义了一个局部变量 x，或一个参数 x，Python 将使用它，然后停止搜索。
2. 全局命名空间：特指当前的模块。如果模块定义了一个名为 x 的变量，函数或类，Python 将使用它然后停止搜索。
3. 内置命名空间：对每个模块都是全局的。作为最后的尝试，Python 将假设 x 是内置函数或变量。
4. 如果 Python 在这些名字空间找不到 x，它将放弃查找并引发一个 NameError 异常，如，NameError: name 'aa' is not defined。

## 命名空间的生命周期

不同的命名空间在不同的时刻创建，有不同的生存期。
1. 内置命名空间在 Python 解释器启动时创建，会一直保留，不被删除。
2. 模块的全局命名空间在模块定义被读入时创建，通常模块命名空间也会一直保存到解释器退出。
3. 当函数被调用时创建一个局部命名空间，当函数返回结果 或 抛出异常时，被删除。每一个递归调用的函数都拥有自己的命名空间。

## global 和 nonlocal 语句

```python
#e.py
gv = ['a', 'global', 'var']

def func(v):
    gv = ['gv'] + gv #UnboundLocalError:local variable 'gv' referenced before assignment
    lv = []
    def inn_func():
        lv = lv + [v]  #UnboundLocalError:local variable 'lv' referenced before assignment
        gv.insert(1, lv[0])
        return gv
    return inn_func
```
**为什么使用global**  

	Python 在执行函数前，会首先生成各层命名空间和作用域，因此 Python 在执行赋值前会将func 内的 'gv' 'lv' 写入局部命名空间和闭包命名空间，当 Python 执行赋值时会在局部作用域、闭包作用域内发现局部命名空间和闭包命名空间内已经具有'gv' 和 'lv' 标识符，但这两个非全局标识符在该赋值语句执行之前并没有被赋值，也即没有对象与标识符关联，因此无法参与四则运算，从而引发错误；

局部空间无法获得，Python 便引入了 global、nonlocal 语句就来说明所修饰的 gv、lv 分别来自全局命名空间和局部命名空间，声明之后，就可以在 func 和 inn_func 内直接改写上层命名空间内 gv 和 lv 的值：
```python
#f.py
gv = ['a', 'global', 'var']
def func(v):
    global gv
    gv = ['gv'] + gv
    lv = []
    print(id(lv))
    def inn_func():
        nonlocal lv
        lv = lv + [v]
        print(id(lv))
        gv.insert(1, lv[0])
        return gv
    return inn_func
 ```

## 借壳
那么是不是不使用 global 和 nonlocal 就不能达到上面的目的呢？来看看这段程序：
```python
#g.py
gv = ['a', 'global', 'var']

def func(v):
    gv.insert(0, 'gv')
    lv = []
    print(id(lv))
    def inn_func():
        lv.append(v)
        print(id(lv))
        gv.insert(1, lv[0])
        return gv
    return inn_func
```
我们改写的不是 gv 和 lv ,而是 gv 和 lv 的元素 gv[0:0] 和 lv[:] 。因此，不需要 global 和 nonlocal 修饰就可以直接改写，这就是“借壳”。修改 gv 和 lv实际上是在父函数作用域内，子函数调用只能读取而不能直接改写 gv 和 lv。
在 nonlocal 尚未引入 Python 中，比如 Python 2.x 若要在子函数中改写父函数变量的值就得通过这种方法。

## 命名空间的访问
- 局部命名空间可以 locals()  BIF来访问。

```python
str='1233'
def func1(i, str ):
    x = 12345
    print(locals())
func1(1 , "first")

>>>{'str': 'first', 'x': 12345, 'i': 1}
```

- 全局 (模块级别)命名空间可以通过 globals() BIF来访问。

```python
gstr = "global string"
 
def func1(i, info):
    x = 12345
    print(locals())
func1(1,'first')
print(globals())

>>>{'x': 12345, 'info': 'first', 'i': 1}
{'__name__': '__main__', ....'func1': <function func1 at 0x000002A7BF382D90>}
```
- locals 与 globals 之间的一个重要的区别
locals 是只读的，globals 可以改写

```python
def func1(i, info):
    x = 12345
    print(locals())
    locals()["x"]= 6789
    print("x=",x)
 
y=54321
func1(1 , "first")
globals()["y"]= 9876
print( "y=",y)
>>>{'i': 1, 'x': 12345, 'info': 'first'}
x= 12345
y= 9876
```  

locals 实际上没有返回局部名字空间，它返回的是一个拷贝。所以对它进行改变对局部名字空间中的变量值并无影响。  
globals 返回实际的全局名字空间，而不是一个拷贝。所以对 globals 所返回的 dictionary 的任何的改动都会直接影响到全局变量。（[原文解释连接](http://www.cnblogs.com/windlaughing/archive/2013/05/26/3100362.html)）