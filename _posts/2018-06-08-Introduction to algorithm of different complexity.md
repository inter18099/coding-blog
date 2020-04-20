---
title: 不同复杂度的算法简介
category: Python
---

### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">算法复杂度的表示：大O</div>

算法复杂度不能用运行秒数来衡量，因为这是计算机速度、编译器速度相关的。应该以算法运行步数为基准。而如果算法步数是一个多项式，那么只取多项式里增长幅度最大的一项，然后这项的系数要省略，因为对于极大的输入来说，这是最关键的决定速度的因素。

### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">常数复杂度的算法</div>

常数复杂度就是指该算法的运行时间跟输入规模无关。不管输入大还是小，运行时间都是一定的。这种算法很少见，举例来说有 len(list)，2 个浮点数相乘等。

### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">对数复杂度的算法</div>

该算法的运行时间是呈对数级增加的，并且运行时间与至少其中一个输入有关。二分查找的运行时间就是对数级的。还有，对数的底不重要，因为根据公式，对数的底实质上就是一个常数系数。

举个例子：
```
def intToStr(i):
    digit = '0123456789'
    if i == 0:
        return '0'
    result = ''
    while i > 0:
        result = digit[i%10] + result
        i = i // 10
        return result
```
我们可以看到，循环决定了这个算法的时间复杂度。我们可以算这个循环运行了多少次，这个循环运行次数等于 i 的长度，因为当 i 增长时，长度增长大概是 log(i) 的，所以 intTostr 的复杂度为 O(log(i))。

### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">线性复杂度的算法</div>

处理list的时间常常是线性的，因为我们经常需要接触 list 的每个元素。
```
def addDigits(s):
    val = 0
    for c in s:
        val += int(c)
        return val
```
还有并不是处理list时也可能有线性复杂度：
```
def factorial(x):
    if x == 1:
        return 1
    else:
        return x*factorial(x - 1)
```
该算法的空间复杂度也是 O(x)。因为递归每进行一次都会分配一个新的栈给它，而这个栈会一直被占用直到调用返回。

### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">对数线性复杂度的算法</div>

这个复杂度是 2 个单位的相乘，每个都跟输入的规模有关。最常用的对数线性算法是归并排序，是 O(nlog(n)) 的复杂度，其中n为排序 list 的长度。

### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">多项式复杂度的算法</div>

最常见的多项式复杂度算法是 2 次方复杂度算法，它的复杂度是输入 n 的平方。
```
def isSubset(L1, L2):
    for e1 in L1:
    matched = False
    for e2 in L2:
        if e1 == e2:
            matched = True
            break
    if not matched:
        return false
    return True
```
内循环运行了 O(len(L2)) 次，外循环运行了 O(len(L1)) 次。所以总体时间复杂度为 O(len(L1)*len(L2))。

### <div style="color:#FFFFFF;background-color:#0000FF;border-bottom:2px solid #0000FF;padding:4px;width:100%">指数复杂度的算法</div>

这个超纲了，就不介绍了。:smile::smile: