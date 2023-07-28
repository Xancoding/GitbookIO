# 🍒 Fundamental Data Types

## 数据类型大小

数据类型的大小取决于编译器和计算机体系结构！

C++ 仅保证每个基本数据类型都具有最小大小 ![1](https://bu.dusays.com/2022/11/26/6381d2545c3fa.png)

为了获得最大的兼容性，您不应假设变量大于指定的最小大小

```cpp
#include <iostream>

int main()
{
    std::cout << "bool:\t\t" << sizeof(bool) << " bytes\n";
    std::cout << "char:\t\t" << sizeof(char) << " bytes\n";
    std::cout << "wchar_t:\t" << sizeof(wchar_t) << " bytes\n";
    std::cout << "char16_t:\t" << sizeof(char16_t) << " bytes\n";
    std::cout << "char32_t:\t" << sizeof(char32_t) << " bytes\n";
    std::cout << "short:\t\t" << sizeof(short) << " bytes\n";
    std::cout << "int:\t\t" << sizeof(int) << " bytes\n";
    std::cout << "long:\t\t" << sizeof(long) << " bytes\n";
    std::cout << "long long:\t" << sizeof(long long) << " bytes\n";
    std::cout << "float:\t\t" << sizeof(float) << " bytes\n";
    std::cout << "double:\t\t" << sizeof(double) << " bytes\n";
    std::cout << "long double:\t" << sizeof(long double) << " bytes\n";

    return 0;
}
```

下面是我的 x64 机器的输出，使用 Clion：

```
bool:           1 bytes
char:           1 bytes
wchar_t:        2 bytes
char16_t:       2 bytes
char32_t:       4 bytes
short:          2 bytes
int:            4 bytes
long:           4 bytes
long long:      8 bytes
float:          4 bytes
double:         8 bytes
long double:    16 bytes
```

如果您使用不同类型的机器或不同的编译器，您的结果可能会有所不同。请注意，您不能对 void 类型使用 sizeof 运算符，因为它没有大小（这样做会导致编译错误）

## 无符号整数和有符号整数

有符号整数范围： ![1](https://bu.dusays.com/2022/11/26/6381d81f385fa.png)

无符号整数范围： ![1](https://bu.dusays.com/2022/11/26/6381d91f961eb.png)

如果无符号整数超出范围，则将其除以大于该类型的最大数，只保留余数

在 C++ 的数学运算中（例如算术或比较），如果使用一个有符号整数和一个无符号整数，则**有符号整数将转换为无符号整数**。并且无符号整数不能存储负数，这会导致数据丢失

在保存整数（甚至应该是非负的整数）和数学运算时，有符号数优于无符号数。避免混合有符号和无符号数字

在 C++ 中仍然有一些情况必须使用无符号数：

首先，在处理位操作时首选无符号数。当需要明确定义的环绕行为时，它们也很有用（在某些算法中很有用，例如加密和随机数生成）

其次，无符号数的使用在某些情况下仍然是不可避免的，主要是那些与数组索引有关的情况。。在这些情况下，无符号值可以转换为有符号值

## 固定宽度整数和 size\_t

### Fixed-width 整数

为什么整数变量的大小不固定？

这可以追溯到 C，当时计算机速度很慢，性能是最受关注的问题。 C 选择有意保留整数的大小，以便编译器实现者可以选择在目标计算机体系结构上表现最佳的 int 大小

C99 定义了一组固定宽度的整数（在 stdint.h 头文件中），保证在任何体系结构上都具有相同的大小

C++ 正式采用这些固定宽度整数作为 C++11 的一部分。可以通过包含 `<cstdint>` 头文件来访问它们，它们在 std 命名空间内定义

![1](https://bu.dusays.com/2022/11/26/6381dcb67d0e0.png)

### Fast and least 整数

The fast 类型（`std::int_fast#_t` 和 `std::uint_fast#_t`）提供最快的有符号/无符号整数类型，宽度至少为 # 位（其中 # = 8、16、32 或 64）。例如，`std::int_fast32_t` 将为您提供最快的至少 32 位的有符号整数类型

The least 类型（`std::int_least#_t` 和 `std::uint_least#_t`）提供宽度至少为 # 位（其中 # = 8、16、32 或 64）的最小有符号/无符号整数类型。例如，`std::uint_least32_t` 将为您提供至少 32 位的最小无符号整数类型

示例：

```cpp
#include <cstdint> // for fixed-width integers
#include <iostream>

int main()
{
	std::cout << "least 8:  " << sizeof(std::int_least8_t) * 8 << " bits\n";
	std::cout << "least 16: " << sizeof(std::int_least16_t) * 8 << " bits\n";
	std::cout << "least 32: " << sizeof(std::int_least32_t) * 8 << " bits\n";
	std::cout << '\n';
	std::cout << "fast 8:  " << sizeof(std::int_fast8_t) * 8 << " bits\n";
	std::cout << "fast 16: " << sizeof(std::int_fast16_t) * 8 << " bits\n";
	std::cout << "fast 32: " << sizeof(std::int_fast32_t) * 8 << " bits\n";

	return 0;
}
```

Result：

```
least 8:  8 bits
least 16: 16 bits
least 32: 32 bits

fast 8:  8 bits
fast 16: 16 bits
fast 32: 32 bits
```

然而，这些快速且最小的整数有其自身的缺点：首先，真正使用它们的程序员并不多，不熟悉会导致错误。其次，快速类型会导致与我们在 4 字节整数中看到的相同类型的内存浪费。最严重的是，由于快速/最小整数的大小可能会有所不同，因此您的程序可能会在解析为不同大小的架构上表现出不同的行为

### std::int8\_t 和 std::uint8\_t 可能表现得像字符而不是整数

由于 C++ 规范中的疏忽，大多数编译器分别将 std::int8\_t 和 std::uint8\_t（以及相应的快速和最小固定宽度类型）定义为 signed char 和 unsigned char 类型，并将其视为相同的类型。这意味着这些 8 位类型的行为可能（或可能不）与其他固定宽度类型不同，这可能会导致错误。此行为是系统相关的，因此在一种体系结构上正确运行的程序可能无法编译或在另一种体系结构上正确运行

为了保持一致性，最好完全避免使用 std::int8\_t 和 std::uint8\_t（以及相关的快速和最少类型）（改用 std::int16\_t 或 std::uint16\_t）

8 位固定宽度整数类型通常被视为字符而不是整数值（这可能因系统而异）。大多数情况下首选 16 位固定整数类型

### Best practice

我们的立场是正确比快速更好，在编译时失败比运行时更好——因此，我们建议避免使用快速/最少的类型，而使用固定宽度的类型。如果您后来发现需要支持无法编译固定宽度类型的平台，那么您可以在此时决定如何迁移您的程序（并彻底测试）

1. 当整数的大小无关紧要时，首选 int（例如，数字将始终适合 2 字节有符号整数的范围）。例如，如果您要求用户输入他们的年龄，或者从 1 数到 10，则 int 是 16 位还是 32 位都没有关系（数字将适合任何一种方式）。这将涵盖您可能遇到的绝大多数情况
2. 存储需要保证范围的数量时，首选 `std::int#_t`
3. 在进行位操作或需要明确定义的环绕行为时，首选 `std::uint#_t`
4. 尽可能避免以下情况：
   1. 存储数量的无符号类型
   2. 8 位固定宽度整数类型
   3. Fast and least 整数类型
   4. 任何特定于编译器的固定宽度整数——例如，Visual Studio 定义了 `__int8`、`__int16` ……

### size\_t

sizeof（以及许多返回大小或长度值的函数）返回一个 std::size\_t 类型的值。 std::size\_t 被定义为无符号整数类型，通常用于表示对象的大小或长度

有趣的是，我们可以使用 sizeof 运算符（返回 std::size\_t 类型的值）来询问 std::size\_t 本身的大小：

```cpp
#include <cstddef> // std::size_t
#include <iostream>

int main()
{
	std::cout << sizeof(std::size_t) << '\n';

	return 0;
}
```

就像整数的大小会因系统而异一样，std::size\_t 的大小也会有所不同。 std::size\_t 保证为无符号且至少为 16 位，但在大多数系统上将等同于应用程序的地址宽度。也就是说，对于 32 位应用程序，std::size\_t 通常是 32 位无符号整数，而对于 64 位应用程序，size\_t 通常是 64 位无符号整数。 size\_t 被定义为足够大以容纳系统上可创建的最大对象的大小（以字节为单位）。例如，如果 std::size\_t 为 4 字节宽，则系统上可创建的最大对象不能大于 4,294,967,295 字节，因为 4,294,967,295 是 4 字节无符号整数可以存储的最大数字。这只是对象大小的上限，实际大小限制可能会更低，具体取决于您使用的编译器

根据定义，任何大小（以字节为单位）大于 size\_t 可以容纳的最大整数值的对象都被视为格式错误（并将导致编译错误），因为 sizeof 运算符将无法在不环绕的情况下返回大小

## 浮点数（IEEE 754）

### 浮点范围和浮点精度

![1](https://bu.dusays.com/2022/11/26/63820f12cc479.png)

使用浮点文字时，始终至少包含一位小数（即使小数为 0）。这有助于编译器理解该数字是浮点数而不是整数

```
int x{5}; // 5 means integer
double y{5.0}; // 5.0 is a floating point literal (no suffix means double type by default)
float z{5.0f}; // 5.0 is a floating point literal, f suffix means float type
```

始终确保字面量的类型与分配给它们或用于初始化的变量的类型相匹配。否则会导致不必要的转换，可能会导致精度损失

确保在应该使用浮点文字的地方不使用整数文字。这包括初始化浮点对象或为浮点对象赋值、进行浮点运算以及调用需要浮点值的函数

![](https://bu.dusays.com/2022/11/26/6382110b7d067.png)

输出浮点数时，std::cout 的默认精度为 6——也就是说，它假定所有浮点变量仅对 6 位有效（浮点数的最小精度），因此它将截断之后的任何内容

浮点变量的精度位数取决于大小（浮点数的精度低于双精度数）和存储的特定值（某些值的精度高于其他值）。浮点值的精度在 6 到 9 位之间，大多数浮点值至少有 7 位有效数字。双精度值的精度在 15 到 18 位之间，大多数双精度值至少有 16 位有效数字。 Long double 的最小精度为 15、18 或 33 位有效数字，具体取决于它占用的字节数

### 舍入误差

我们可以使用名为 std::setprecision() 的输出操纵器函数覆盖 std::cout 显示的默认精度。输出操纵器改变数据的输出方式，并在 iomanip 标头中定义

```
#include <iostream>
#include <iomanip> // for output manipulator std::setprecision()

int main()
{
    std::cout << std::setprecision(16); // show 16 digits of precision
    std::cout << 3.33333333333333333333333333333333333333f <<'\n'; // f suffix means float
    std::cout << 3.33333333333333333333333333333333333333 << '\n'; // no suffix means double

    return 0;
}
```

在使用需要比变量所能容纳的精度更高的浮点数时，必须小心

**除非空间非常宝贵，否则最好使用 double over float，因为 float 缺乏精度通常会导致不准确**

值 123456789.0 具有 10 位有效数字，但浮点值通常具有 7 位精度（而 123456792 的结果仅精确到 7 位有效数字）。我们失去了一些精度！当由于无法精确存储数字而导致精度丢失时，这称为**舍入误差**

数学运算（例如加法和乘法）往往会使舍入误差增大。所以即使0.1在第17位有效位有舍入误差，但是当我们加上0.1十次时，舍入误差已经爬到第16位有效位了。继续操作会导致此错误变得越来越严重

### NaN 和 Inf

有两种特殊类别的浮点数。第一个是 `Inf`，代表无穷大。 Inf 可以是正数或负数。第二个是 `NaN`，代表“不是数字”。有几种不同类型的 NaN（我们不会在这里讨论）。 NaN 和 Inf 仅在编译器对浮点数使用特定格式 (IEEE 754) 时可用

### Conclusion

总而言之，关于浮点数你应该记住两件事：

浮点数对于存储非常大或非常小的数字很有用，包括带有小数部分的数字

浮点数通常有小的舍入误差，即使数字的有效数字少于精度也是如此。很多时候这些都没有引起注意，因为它们太小了，而且因为输出的数字被截断了。但是，浮点数的比较可能不会给出预期的结果。对这些值执行数学运算将导致舍入误差变大

## 布尔值

如果您希望 std::cout 打印“true”或“false”而不是 0 或 1，您可以使用 std::boolalpha。这是一个例子：

```cpp
#include <iostream>

int main()
{
    std::cout << true << '\n';
    std::cout << false << '\n';

    std::cout << std::boolalpha; // print bools as true or false

    std::cout << true << '\n';
    std::cout << false << '\n';
    return 0;
}
```

您可以使用 std::noboolalpha 将其关闭

您不能使用除 0 1 外的整数初始化布尔值：

```cpp
#include <iostream>

int main()
{
	bool b{ 4 }; // error: narrowing conversions disallowed
	std::cout << b;

	return 0;
}
```

但是，在任何可以将整数转换为布尔值的上下文中，整数 0 将转换为 false，而任何其他整数将转换为 true

事实证明，std::cin 只接受布尔变量的两个输入：0 和 1（不是 true 或 false）。任何其他输入都会导致 std::cin 无声地失败。在这种情况下，因为我们输入了 true，所以 std::cin 默默地失败了。失败的输入也会将变量清零，因此 b 也被赋值 false。因此，当 std::cout 打印 b 的值时，它打印 0

要允许 std::cin 接受“false”和“true”作为输入，必须启用 std::boolalpha 选项：

```cpp
#include <iostream>

int main()
{
	bool b{};
	std::cout << "Enter a boolean value: ";

	// Allow the user to enter 'true' or 'false' for boolean values
	// This is case-sensitive, so True or TRUE will not work
	std::cin >> std::boolalpha;
	std::cin >> b;

	std::cout << "You entered: " << b << '\n';

	return 0;
}
```

但是，当启用 std::boolalpha 时，“0”和“1”将不再被视为布尔值

## Chars

char 数据类型旨在保存单个字符。字符可以是单个字母、数字、符号或空格

char 数据类型是整数类型，这意味着基础值存储为整数。类似于布尔值 0 被解释为 false 而非零被解释为 true 的方式，char 变量存储的整数被解释为 ASCII 字符

Char 由 C++ 定义为大小始终为 1 个字节。默认情况下，char 可以是有符号的或无符号的（尽管它通常是有符号的）。如果您使用 chars 来保存 ASCII 字符，则不需要指定符号（因为有符号和无符号字符都可以保存 0 到 127 之间的值）

如果您使用 char 来保存小整数（除非您明确优化空间，否则您不应该这样做），您应该始终指定它是有符号的还是无符号的。 signed char 可以保存 -128 到 127 之间的数字。unsigned char 可以保存 0 到 255 之间的数字

**将单个字符放在单引号中（ e.g. `'t'` or `'\n'`, not `"t"` or `"\n"`）这有助于编译器更有效地进行优化**

出于向后兼容性的原因，许多 C++ 编译器支持多字符文字，即包含多个字符（例如“56”）的字符文字。如果支持，它们具有实现定义的值（意味着它因编译器而异）。因为它们不是 C++ 标准的一部分，而且它们的值也没有严格定义，所以应该避免使用多字符文字

ASCII 之外最著名的映射是 Unicode 标准，它将超过 144,000 个整数映射到许多不同语言的字符。由于 Unicode 包含如此多的代码点，因此单个 Unicode 代码点需要 32 位来表示一个字符（称为 UTF-32）。但是，Unicode 字符也可以使用多个 16 位或 8 位字符（分别称为 UTF-16 和 UTF-8）进行编码

char16\_t 和 char32\_t 添加到 C++11 以提供对 16 位和 32 位 Unicode 字符的明确支持。 C++20 中添加了 char8\_t

您不需要使用 char8\_t、char16\_t 或 char32\_t，除非您计划让您的程序与 Unicode 兼容

同时，在处理字符（和字符串）时，您应该只使用 ASCII 字符。使用来自其他字符集的字符可能会导致您的字符显示不正确

## 常量和符号常量

### const variables

Const 变量必须在定义它们时进行初始化，然后不能通过赋值更改该值

Const 变量可以从其他变量（包括非常量变量）初始化

命名时以 “k” 开头, 大小写混合,例如：

```
const int kDaysInAWeek = 7;
```

### 符号常量

符号常量指的是**被赋予常量值的名称**。`const variables` 是一种符号常量，因为变量有一个名称（它的标识符）和一个常量值

```cpp
#include <iostream>
#define MAX_STUDENTS_PER_CLASS 30

int main()
{
    std::cout << "The class has " << MAX_STUDENTS_PER_CLASS << " students.\n";

    return 0;
}
```

编译此程序时，预处理器会将 MAX\_STUDENTS\_PER\_CLASS 替换为字面值 30，然后编译器会将其编译为您的可执行文件

因为类对象宏有一个名字，并且替换文本是一个常量值，所以带有替换文本的类对象宏也是符号常量

### 对于符号常量，更喜欢常量变量而不是类对象宏

首先，因为宏是由预处理器解析的，所有出现的宏都在编译之前被定义的值替换。如果您正在调试代码，您将看不到实际值（例如 30）——您只会看到符号常量的名称（例如 MAX\_STUDENTS\_PER\_CLASS）。因为这些#defined 值不是变量，所以您无法在调试器中添加监视来查看它们的值。如果您想知道 MAX\_STUDENTS\_PER\_CLASS 解析为什么值，您必须找到 MAX\_STUDENTS\_PER\_CLASS 的定义（可能在不同的文件中）。这会使您的程序更难调试

其次，宏可能与普通代码有命名冲突

第三，宏不遵循正常的作用域规则，这意味着在极少数情况下，在程序的一部分中定义的宏可能会与在程序的另一部分中编写的代码发生冲突，而它不应该与之交互

## 编译时常量、常量表达式和 constexpr

### Constant expressions

常量表达式是可以在编译时由编译器求值的表达式。要成为常量表达式，表达式中的所有值必须在编译时已知（并且所有调用的运算符和函数必须支持编译时求值）

在编译时对常量表达式求值会使我们的编译时间变长（因为编译器必须做更多的工作），但这样的表达式只需要求值一次（而不是每次程序运行时）。生成的可执行文件速度更快，使用的内存更少

### Compile-time constants

编译时常量是其值在编译时已知的常量。文字（例如“1”、“2.3”和“Hello, world!”）是一种编译时常量

Const 变量可能是也可能不是编译时常量

### Compile-time const

如果 const 变量的初始值设定项是常量表达式，则它是编译时常量

```cpp
#include <iostream>

int main()
{
	const int x { 3 };  // x is a compile-time const
	const int y { 4 };  // y is a compile-time const

	const int z { x + y }; // x + y is a compile-time expression

	std::cout << z << '\n';

	return 0;
}
```

因为 x 和 y 的初始化值是常量表达式，所以 x 和 y 是编译时常量。这意味着 x + y 也是常量表达式。所以当编译器编译这个程序时，它可以计算 x + y 的值，并将常量表达式替换为结果文字 7

### Runtime const

任何使用非常量表达式初始化的 const 变量都是运行时常量。运行时常量是其初始化值直到运行时才知道的常量

```cpp
#include <iostream>

int getNumber()
{
    std::cout << "Enter a number: ";
    int y{};
    std::cin >> y;

    return y;
}

int main()
{
    const int x{ 3 };           // x is a compile time constant

    const int y{ getNumber() }; // y is a runtime constant

    const int z{ x + y };       // x + y is a runtime expression
    std::cout << z << '\n';     // this is also a runtime expression

    return 0;
}
```

即使 y 是常量，初始化值（getNumber() 的返回值）直到运行时才知道。因此，y 是运行时常量，而不是编译时常量。因此，表达式 x + y 是一个运行时表达式

### constexpr 关键字

当你声明一个 const 变量时，编译器会隐式地跟踪它是运行时常量还是编译时常量。在大多数情况下，除了优化目的之外，这无关紧要，但有一些奇怪的情况，C++ 需要编译时常量而不是运行时常量

因为编译时常量通常允许更好的优化（并且几乎没有缺点），所以我们通常希望尽可能使用编译时常量

我们可以寻求编译器的帮助，以确保我们得到一个我们期望的编译时常量。为此，我们在变量声明中使用 constexpr 关键字而不是 const。 constexpr（“常量表达式”的缩写）变量只能是编译时常量。如果 constexpr 变量的初始化值不是常量表达式，编译器会出错

```cpp
#include <iostream>

int five()
{
    return 5;
}

int main()
{
    constexpr double gravity { 9.8 }; // ok: 9.8 is a constant expression
    constexpr int sum { 4 + 5 };      // ok: 4 + 5 is a constant expression
    constexpr int something { sum };  // ok: sum is a constant expression

    std::cout << "Enter your age: ";
    int age{};
    std::cin >> age;

    constexpr int myAge { age };      // compile error: age is not a constant expression
    constexpr int f { five() };       // compile error: return value of five() is not a constant expression

    return 0;
}
```

任何在初始化后不应修改且其初始值设定项在编译时已知的变量都应声明为 `constexpr`

任何在初始化后不应修改且其初始值设定项在编译时未知的变量都应声明为 `const`

## Literals

文字是直接插入代码中的未命名值。例如：

```cpp
return 5;                   // 5 is an integer literal
bool myNameIsAlex { true }; // true is a boolean literal
std::cout << 3.4;           // 3.4 is a double literal
```

如对象有类型一样，所有文字都有类型。文字的类型是从文字的值推导出来的

## 十进制、二进制、十六进制和八进制

### 二进制文字和数字分隔符

在 C++14 之前，不支持二进制文字。然而，十六进制文字为我们提供了一个有用的解决方法（您可能仍会在现有代码库中看到）：

```cpp
#include <iostream>

int main()
{
    int bin{};    // assume 16-bit ints
    bin = 0x0001; // assign binary 0000 0000 0000 0001 to the variable
    bin = 0x0002; // assign binary 0000 0000 0000 0010 to the variable
    bin = 0x0004; // assign binary 0000 0000 0000 0100 to the variable
    bin = 0x0008; // assign binary 0000 0000 0000 1000 to the variable
    bin = 0x0010; // assign binary 0000 0000 0001 0000 to the variable
    bin = 0x0020; // assign binary 0000 0000 0010 0000 to the variable
    bin = 0x0040; // assign binary 0000 0000 0100 0000 to the variable
    bin = 0x0080; // assign binary 0000 0000 1000 0000 to the variable
    bin = 0x00FF; // assign binary 0000 0000 1111 1111 to the variable
    bin = 0x00B3; // assign binary 0000 0000 1011 0011 to the variable
    bin = 0xF770; // assign binary 1111 0111 0111 0000 to the variable

    return 0;
}
```

在 C++14 中，我们可以通过使用 0b 前缀来使用二进制文字：

```cpp
#include <iostream>

int main()
{
    int bin{};        // assume 16-bit ints
    bin = 0b1;        // assign binary 0000 0000 0000 0001 to the variable
    bin = 0b11;       // assign binary 0000 0000 0000 0011 to the variable
    bin = 0b1010;     // assign binary 0000 0000 0000 1010 to the variable
    bin = 0b11110000; // assign binary 0000 0000 1111 0000 to the variable

    return 0;
}
```

由于长文本可能难以阅读，C++14 还添加了使用引号 (‘) 作为数字分隔符的功能（分隔符不能出现在值的第一位数字之前）（数字分隔符纯粹是视觉上的，不会以任何方式影响字面值）

```cpp
#include <iostream>

int main()
{
    int bin { 0b1011'0010 };  // assign binary 1011 0010 to the variable
    long value { 2'132'673'462 }; // much easier to read than 2132673462

    return 0;
}
```

### 以十进制、八进制或十六进制输出值

默认情况下，C++ 以十进制形式输出值。但是，您可以通过使用 std::dec、std::oct 和 std::hex I/O 操纵器更改输出格式：

```cpp
#include <iostream>

int main()
{
    int x { 12 };
    std::cout << x << '\n'; // decimal (by default)
    std::cout << std::hex << x << '\n'; // hexadecimal
    std::cout << x << '\n'; // now hexadecimal
    std::cout << std::oct << x << '\n'; // octal
    std::cout << std::dec << x << '\n'; // return to decimal
    std::cout << x << '\n'; // decimal

    return 0;
}
```

### 以二进制输出值

以二进制形式输出值有点困难，因为 std::cout 没有内置此功能。幸运的是，C++ 标准库包含一个名为 std::bitset 的类型，它将为我们完成此操作（在 `<bitset>` 标头中）。要使用 std::bitset，我们可以定义一个 std::bitset 变量并告诉 std::bitset 我们要存储多少位。位数必须是编译时常量。 std::bitset 可以用无符号整数值（任何格式，包括十进制、八进制、十六进制或二进制）初始化

```cpp
#include <bitset> // for std::bitset
#include <iostream>

int main()
{
	// std::bitset<8> means we want to store 8 bits
	std::bitset<8> bin1{ 0b1100'0101 }; // binary literal for binary 1100 0101
	std::bitset<8> bin2{ 0xC5 }; // hexadecimal literal for binary 1100 0101

	std::cout << bin1 << '\n' << bin2 << '\n';
	std::cout << std::bitset<4>{ 0b1010 } << '\n'; // create a temporary std::bitset and print it

	return 0;
}
```

## std::string

### 使用 std::getline() 输入文本

事实证明，当使用 operator>> 从 std::cin 中提取字符串时，operator>> 只返回它遇到的第一个空格之前的字符。任何其他字符都留在 std::cin 中，等待下一次提取

要将整行输入读入字符串，最好改用 `std::getline()` 函数。 std::getline() 需要两个参数：第一个是 std::cin，第二个是您的字符串变量

```cpp
#include <string> // For std::string and std::getline
#include <iostream>

int main()
{
    std::cout << "Enter your full name: ";
    std::string name{};
    std::getline(std::cin >> std::ws, name); // read a full line of text into name

    std::cout << "Enter your age: ";
    std::string age{};
    std::getline(std::cin >> std::ws, age); // read a full line of text into age

    std::cout << "Your name is " << name << " and your age is " << age << '\n';

    return 0;
}
```

`std::ws` 输入操纵器告诉 std::cin 在提取之前**忽略任何前导空格**。前导空白是出现在字符串开头的任何空白字符（空格、制表符、换行符）

如果使用 `std::getline()` 读取字符串，请使用 `std::cin >> std::ws` 输入操纵器忽略前导空格

将提取运算符 (>>) 与 std::cin 一起使用会忽略前导空格

std::getline() 不会忽略前导空格，除非您使用输入操纵器 std::ws

### 字符串长度

如果我们想知道 std::string 中有多少个字符，我们可以向 std::string 对象询问它的长度。注意 std::string::length() 返回一个无符号整数值（很可能是 size\_t 类型）。如果你想将长度分配给一个 int 变量，你应该对其进行 static\_cast 以避免编译器关于有符号/无符号转换的警告：

```cpp
int length { static_cast<int>(name.length()) };
```

### std::string 的初始化和复制开销很大

每当初始化 std::string 时，都会生成用于初始化它的字符串的副本。每当 std::string 按值传递给 std::string 参数时，都会生成另一个副本。不要按值传递 std::string，因为生成 std::string 的副本开销很大。更喜欢 std::string\_view 参数

### Literals for `std::string` & `std::string_view`

双引号字符串文字（比如“Hello, world!”）默认是 C 风格的字符串

我们可以通过在双引号字符串文字后使用 s 后缀来创建类型为 std::string 的字符串文字

```cpp
#include <iostream>
#include <string>      // for std::string
#include <string_view> // for std::string_view

int main()
{
    using namespace std::literals; // easiest way to access the s and sv suffixes

    std::cout << "foo\n";   // no suffix is a C-style string literal
    std::cout << "goo\n"s;  // s suffix is a std::string literal
    std::cout << "moo\n"sv; // sv suffix is a std::string_view literal

    return 0;
}
```

“s”后缀位于命名空间 std::literals::string\_literals 中。“sv”后缀位于命名空间 std::literals::string\_view\_literals 中。访问文字后缀的最简单方法是通过使用指令使用命名空间 std::literals。这是可以使用整个命名空间的例外情况之一，因为其中定义的后缀不太可能与您的任何代码冲突

你可能不需要经常使用 std::string 文字（因为用 C 风格的字符串文字初始化 std::string 对象很好），但我们会在以后的课程中看到一些使用 std 的情况::string literals 而不是 C 风格的 string literals 使事情变得更容易

### Constexpr 字符串

如果您尝试定义一个 constexpr std::string，您的编译器可能会产生一个错误

```cpp
#include <iostream>
#include <string>

using namespace std::literals;

int main()
{
    constexpr std::string name{ "Alex"s }; // compile error

    std::cout << "My name is: " << name;

    return 0;
}
```

发生这种情况是因为 constexpr std::string 在 C++17 或更早版本中不受支持，并且在 C++20 中仅提供最低限度的支持。如果您需要 constexpr 字符串，请改用 std::string\_view

## std::string\_view

### std::string\_view C++17

为了解决 std::string 初始化（或复制）成本高昂的问题，C++17 引入了 std::string\_view（位于 \<string\_view> 标头中）。 std::string\_view 提供对现有字符串（C 风格字符串文字、std::string 或 char 数组）的只读访问，而无需制作副本

```cpp
#include <iostream>
#include <string_view>

void printSV(std::string_view str) // now a std::string_view
{
    std::cout << str << '\n';
}

int main()
{
    std::string_view s{ "Hello, world!" }; // now a std::string_view
    printSV(s);

    return 0;
}
```

当我们用 C 风格的字符串文字“Hello, world!”初始化 std::string\_view s 时，s 提供对“Hello, world!”的只读访问。无需复制字符串。当我们将 s 传递给 printSV() 时，参数 str 从 s 初始化。这使我们能够通过 str 访问“Hello, world!”，不用再次复制字符串

当您需要只读字符串时，尤其是对于函数参数，优先使用 std::string\_view 而不是 std::string

### constexpr std::string\_view

std::string\_view 完全支持 constexpr：

```cpp
#include <iostream>
#include <string_view>

int main()
{
    constexpr std::string_view s{ "Hello, world!" };
    std::cout << s << '\n'; // s will be replaced with "Hello, world!" at compile-time

    return 0;
}
```

### std::string & std::string\_view

可以使用 std::string 初始值设定项创建 std::string\_view，并且 std::string 将隐式转换为 std::string\_view：

```cpp
#include <iostream>
#include <string>
#include <string_view>

void printSV(std::string_view str)
{
    std::cout << str << '\n';
}

int main()
{
    std::string s{ "Hello, world" };
    std::string_view sv{ s }; // Initialize a std::string_view from a std::string
    std::cout << sv << '\n';

    printSV(s); // implicitly convert a std::string to std::string_view

    return 0;
}
```

因为 std::string 复制了它的初始化器（这开销很大），C++ 不允许将 std::string\_view 隐式转换为 std::string。但是，我们可以使用 std::string\_view 初始值设定项显式创建 std::string，或者我们可以使用 static\_cast 将现有的 std::string\_view 转换为 std::string

```cpp
#include <iostream>
#include <string>
#include <string_view>

void printString(std::string str)
{
    std::cout << str << '\n';
}

int main()
{
  std::string_view sv{ "balloon" };

  std::string str{ sv }; // okay, we can create std::string using std::string_view initializer

  // printString(sv);   // compile error: won't implicitly convert std::string_view to a std::string

  printString(static_cast<std::string>(sv)); // okay, we can explicitly cast a std::string_view to a std::string

  return 0;
}
```
