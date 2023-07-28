# 🫐 Functions and Files

## 函数返回值

C++标准只定义了3种状态码的含义：0、EXIT\_SUCCESS、EXIT\_FAILURE。 0 和 EXIT\_SUCCESS 都表示程序执行成功。 EXIT\_FAILURE 表示程序没有成功执行

EXIT\_SUCCESS 和 EXIT\_FAILURE 是 `<cstdlib>` 标头中定义的预处理器宏：

```c++
#include <cstdlib> // for EXIT_SUCCESS and EXIT_FAILURE

int main()
{
    return EXIT_SUCCESS;
}
```

如果你想最大限度地提高可移植性，你应该只使用 0 或 EXIT\_SUCCESS 来指示成功终止，或者使用 EXIT\_FAILURE 来指示不成功终止

## include 头文件顺序

在 C++ 中，头文件的顺序通常应该遵循以下顺序：

1. C 标准库头文件（如 `<stdio.h>`）
2. C++ 标准库头文件（如 `<iostream>`）
3. 第三方库头文件（如 `Boost`）
4. 项目特定的头文件（如自定义的头文件）

这样的顺序可以避免头文件之间的依赖关系问题，同时也可以更快地查找问题。

The headers for each grouping should be sorted alphabetically（按字母顺序排序）.

这个顺序并不是强制的，主要取决于项目的需要和编程风格。

## Header file best practices

1. 始终 include header guards
2. 不要在头文件中定义变量和函数（全局常量是一个例外）
3. 为头文件指定与其关联的源文件相同的名称（例如，grades.h 与 grades.cpp 配对）
4. 每个头文件应该有一个特定的工作，并且尽可能独立。例如，您可以将与功能 A 相关的所有声明放在 A.h 中，将与功能 B 相关的所有声明放在 B.h 中。这样如果你以后只关心 A，你可以只 include A.h 而不会得到任何与 B 相关的东西
5. 请注意您需要为代码文件中使用的功能显式包含哪些 header
6. 您编写的每个 header 都应该自行编译（它应该`#include` 它需要的每个依赖项）
7. 仅 `#include` 您需要的内容（不要仅仅因为可以就包含所有内容）
8. 不要`#include` .cpp 文件

## 预处理器

在编译之前，代码文件会经历一个称为翻译的阶段。翻译阶段会发生很多事情，让您的代码准备好进行编译（如果您好奇，可以在此处找到翻译阶段列表）。应用了翻译的代码文件称为翻译单元

最值得注意的翻译阶段涉及预处理器。预处理器最好被认为是一个单独的程序，它可以处理每个代码文件中的文本

当预处理器运行时，它会扫描代码文件（从上到下），寻找预处理器指令。预处理程序指令（通常简称为 **指令（Directives）**）是以# 符号开头并以换行符（不是分号）结尾的指令。这些指令告诉预处理器执行某些文本操作任务。请注意，预处理器不理解 C++ 语法——相反，指令有自己的语法（在某些情况下类似于 C++ 语法，而在其他情况下，则不太相似）

请注意，预处理器不会以任何方式修改原始代码文件——相反，预处理器所做的所有文本更改都会在每次编译代码文件时临时发生在内存中或使用临时文件

### Includes

当您 `#include` 一个文件时，预处理器会用 include 文件的内容替换 `#include` 指令。然后对 include 的内容进行预处理（连同文件的其余部分），然后进行编译

### Macro defines

`#define` 指令可用于创建 **宏**

有两种基本类型的宏：**类对象宏**和**类函数宏**

**类函数宏**的行为类似于函数，并且具有相似的目的。它们的使用通常被认为是危险的，它们几乎可以做的任何事情都可以通过一个正常的函数来完成

**类对象宏**可以通过以下两种方式之一定义：

```c++
#define identifier
#define identifier substitution_text
```

**替换文本**的类对象宏被用作（在 C 中）将名称分配给文字的一种方式。这不再是必需的，因为 C++ 中提供了更好的方法（Const 变量和符号常量）。带有替换文本的类对象宏现在通常只能在遗留代码中看到

**没有替换文本**的类对象宏：标识符的任何进一步出现都将被删除并被替换为任何东西！

与带有替换文本的类对象宏不同，这种形式的宏通常被认为可以使用。

### Conditional compilation

**条件编译预处理器指令**允许您指定在什么条件下编译或不编译。有很多不同的条件编译指令，但我们在这里只介绍目前使用最多的三个：`#ifdef`、`#ifndef` 和 `#endif`

`#ifdef` 预处理器指令允许预处理器检查标识符是否先前已被`#defined`。如果是，编译 `#ifdef` 和匹配的 `#endif` 之间的代码。如果不是，代码将被忽略

考虑以下程序：

```cpp
#include <iostream>

#define PRINT_JOE

int main()
{
#ifdef PRINT_JOE
    std::cout << "Joe\n"; // will be compiled since PRINT_JOE is defined
#endif

#ifdef PRINT_BOB
    std::cout << "Bob\n"; // will be ignored since PRINT_BOB is not defined
#endif

    return 0;
}
```

因为 `PRINT_JOE` 已被 `#defined`，`std::cout << "Joe\n"` 行将被编译。因为 `PRINT_BOB` 尚未被 `#defined`，`std::cout << "Bob\n"` 行将被忽略

`#ifndef` 与 `#ifdef` 相反，因为它允许您检查标识符是否尚未 `#defined`

```cpp
#include <iostream>

int main()
{
#ifndef PRINT_BOB
    std::cout << "Bob\n";
#endif

    return 0;
}
```

该程序打印 `“Bob”`，因为 `PRINT_BOB` 从未被 `#defined`

条件编译的一个更常见的用途是使用 `#if 0` 将代码块排除在编译之外（就像它在注释块中一样）：

```cpp
#include <iostream>

int main()
{
    std::cout << "Joe\n";

#if 0 // Don't compile anything starting here
    std::cout << "Bob\n";
    std::cout << "Steve\n";
#endif // until this point

    return 0;
}
```

这也提供了一种方便的方法来\*\*“注释掉”包含多行注释的代码\*\*（由于多行注释是不可嵌套的，因此不能使用另一个多行注释来注释掉）：

```cpp
#include <iostream>

int main()
{
    std::cout << "Joe\n";

#if 0 // Don't compile anything starting here
    std::cout << "Bob\n";
    /* Some
     * multi-line
     * comment here
     */
    std::cout << "Steve\n";
#endif // until this point

    return 0;
}
```

**类对象宏不影响其他预处理器指令，宏只会导致普通代码的文本替换。其他预处理器命令将被忽略**

```cpp
#define FOO 9 // Here's a macro substitution

#ifdef FOO // This FOO does not get replaced because it’s part of another preprocessor directive
    std::cout << FOO; // This FOO gets replaced with 9 because it's part of the normal code
#endif
```

指令在编译前解析，逐个文件地从上到下解析

```cpp
#include <iostream>

void foo()
{
#define MY_NAME "Alex"
}

int main()
{
	std::cout << "My name is: " << MY_NAME;

	return 0;
}
```

即使看起来 `#define MY_NAME “Alex”` 是在函数 `foo` 中定义的，预处理器也不会注意到，因为它不理解像函数这样的 C++ 概念。因此，该程序的行为与 `#define MY_NAME “Alex”` 在函数 `foo` 之前或之后立即定义的程序相同。为了一般的可读性，您通常希望在函数之外 `#define` 标识符

预处理器完成后，该文件中所有定义的标识符都将被丢弃。这意味着指令**仅从定义点到定义它们的文件末尾有效**。**一个代码文件中定义的指令不会影响同一项目中的其他代码文件**

考虑以下示例：

`function.cpp`:

```cpp
#include <iostream>

void doSomething()
{
#ifdef PRINT
    std::cout << "Printing!";
#endif
#ifndef PRINT
    std::cout << "Not printing!";
#endif
}
```

`main.cpp`:

```cpp
void doSomething(); // forward declaration for function doSomething()

#define PRINT

int main()
{
    doSomething();

    return 0;
}
```

上面的程序会打印：

```cpp
Not printing!
```

尽管 `PRINT` 是在 `main.cpp` 中定义的，但这对 `function.cpp` 中的任何代码都没有任何影响（`PRINT` 只是从定义点到 `main.cpp` 末尾的 `#defined`）
