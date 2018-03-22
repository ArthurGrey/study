# 算法和流程控制
代码整体结构是执行速度的决定因素之一。代码量少不一定运行速度快，代码量多也不一定运行速度慢。性能损失与代码组织方式和具体问题解决办法直接相关。

## 循环
循环的类型：
- `for` 大概是最常用的循环结构，由四个部分组成：初始化体、前测条件、后执行体、循环体
```
for(初始化体;前测条件;后执行体){
  循环体
}
```
- `while` 是一个简单的预测试循环，由一个预测试条件和一个循环体构成
```
while(预测试条件){
  循环体
}
```
- `do while` 循环中，循环体至少运行一次，后测试条件决定循环体是否应再次执行
```
do{
  循环体
}while(后测试条件)
```
- `for-in` 用来枚举任何对象的命名属性，每次循环执行，属性变量被填充以对象属性的名字（一个字符串），直到所有的对象属性遍历完成才返回。返回的属性包括对象的实例属性和它从原型链继承而来的属性。
```
for(var a in onject){
  循环体
}
```

### 循环性能

在javascript的四种循环类型中，只有`for-in`循环明显要慢。

由于每次迭代操作要搜索实例或原形的属性，for-in 循环每次迭代都要付出更多开销，所以比其他类型循环慢一些。在同样的循环迭代操作中，for-in 循环比其他类型的循环慢7 倍之多。因此推荐的做法如下：除非你需要对数目不详的对象属性进行操作，否则避免使用for-in 循环。

除for-in 循环外，其他循环类型性能相当，难以确定哪种循环更快。选择循环类型应基于需求而不是性能。
如果循环类型与性能无关，那么如何选择？其实只有两个因素：
- 每次迭代做什么
- 迭代的次数

通过减少两者中的一个或者全部，都可以积极的影响循环的整体性能。

#### 减少迭代的工作量

不言而喻，如果一次循环迭代需要较长时间来执行，那么多次循环将需要更长时间。限制在循环体内进行耗时操作的数量是一个加快循环的好方法。

优化循环工作量的第一步是减少对象成员和数组项查找的次数。在大多数浏览器上，这些操作比访问局部变量或直接量需要更长时间。前面的例子中每次循环都查找items.length。这是一种浪费，因为该值在循环体执行过程中不会改变，因此产生了不必要的性能损失。你可以简单地将此值存入一个局部变量中，在控制条件中使用这个局部变量，从而提高了循环性能。

<font color='#dd0000'>注</font>

你还可以通过改变他们的顺序提高循环性能。通常，数组元素的处理顺序与任务无关，你可以从最后一个开始，直到处理完第一个元素。倒序循环是编程语言中常用的性能优化方法，但一般来说不太容易理解。在JavaScript 中，倒序循环可以略微提高循环性能，只要你消除因此而产生的额外操作。
