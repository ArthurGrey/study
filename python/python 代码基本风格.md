# python 代码的基本风格
## 关键点

> 注释

对于自己和以后要看代码的人都是非常重要的。既不能没有注释，也不能过度注释，注释应该简洁明了，并且放在最合适的地方，还有要确保注释的准确性。

> 文档

 python 提供了一个机制，可以通过 `__doc__` 来动态获取文档字符串。在模块、类声明，或者函数声明中第一个没有复制的字符串可以用 `obj.__doc__` 来访问，其中 `obj` 是一个模块、类或函数的名字。

> 缩进

四个空格很好， `tab` 可能对不同的平台不兼容。

> 选择标识符名称

选择短而意义丰富的标识符。

## 模块结构和布局
应该按照下面的顺序布局：

1. 起始行（Unix）：通常只有在类Unix环境下使用起始行，有起始行就能够通过输入脚本名称来执行脚本，无需直接调用解释器。
2. 模块文档：简要介绍模块的功能和重要全局变量的含义，模块外可以通过 `module.__doc__` 访问这些内容。
3. 模块导入：导入当前模块得到代码需要的所有模块，每个模块只要导入一次，函数内部的模块导入代码不会执行，除非该函数正在执行。
4. 变量定义：这里定义的变量为全局变量，本模块中的所有函数都可以直接使用。从好的编码风格来说，除非必须，否则应该尽量使用局部变量来代替全局变量，这样代码更容易维护，还可以提高性能并节省内存。
5. 类定义：所有的类都在这里定义，当模块被导入时 `class` 语句会被执行，类就会被定义，类的文档变量是 class.__doc__。
6. 函数定义：此处定义的函数可以通过 `module.function()` 在外部被访问到，当模块导入时， def 语句就会被执行，函数也就会被定义好，函数的文档变量是 `function.__doc__` 。
7. 主程序：无论这个模块是被别的模块导入函数作为脚本直接执行，都会执行这部分代码，通常这里不会有太多功能性代码，而是根据执行的模式调用不同的函数。

## 推荐代码风格：主程序调用 main() 函数
主程序中的代码通常包括变量赋值、类定义和函数定义，随后检查 `__name__` 来决定是否调用另一个函数来完成该模块的功能。

很多项目都是一个主程序，由它导入所有需要的模块。绝大多数模块创建的目的是为了被别人调用而不是作为独立执行的脚本。通常只有主程序模块中有大量的顶级可执行代码，所有其他被导入的模块应该只有很少的顶级可执行代码，所有的功能代码都应该封装在函数或类中。

## __name__ 指示模块应该如何被加载
由于主程序代码无论模块是被导入函数被直接执行都会运行，我们必须知道模块是如何决定运行方向。一个应用程序可能需要导入另一个应用程序的一个模块，以便重用一些有用的代码。这种情况下，只想访问位于其他应用程序中的代码而不运行那个应用程序。这时候就需要使用 `__name__` ，如果模块是被导入， `__name__` 值为模块名；如果模块是被直接执行， `__name__`值为 `__main__`。

## 在主程序中写测试代码
测试代码仅当该文件被直接执行时运行。将测试代码放在一个叫 main() 或者 test() (能够表明是测试代码的名称) 的函数中，如果该模块被当成脚本运行，就调用这个函数。

测试代码应该随着测试条件和结果及时修改。每次代码更新都应该运行测试代码，来确保没有引发新的问题。

主程序中放置测试代码只是测试模块的简单快捷的手段，python 标准库中还提供了 `unittest` 模块，当需要对一个大规模系统的组件进行正规系统的测试时，它会很有用。

