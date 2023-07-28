# 🍈 Debugging C++ Programs

## 调试过程

1. 找到问题的根本原因（通常是不起作用的代码行）
2. 确保您了解问题发生的原因
3. 确定您将如何解决问题
4. 修复问题
5. 重新测试以确保问题已解决并且没有出现新问题

## 调试策略

观察程序运行时的行为，并尝试从中诊断问题

1. 弄清楚如何**重现问题**
2. 运行程序并收集信息以**缩小问题所在的范围**
3. 重复前面的步骤，**直到找到问题**

### 调试策略 1：注释掉你的代码

如果您的程序表现出错误行为，减少必须搜索的代码量的一种方法是注释掉一些代码并查看问题是否仍然存在。如果问题仍然存在，则注释掉的代码不负责任

### 调试策略 2：验证代码流

另一个在更复杂的程序中常见的问题是程序调用一个函数的次数太多或太少（包括根本不调用）

在这种情况下，将语句放在函数的顶部以打印函数的名称会很有帮助。这样，当程序运行时，您可以看到调用了哪些函数

当**出于调试目的打印信息时**，使用 `std::cerr` 而不是 `std::cout`。这样做的一个原因是 std::cout 可能被缓冲，这意味着在您要求 std::cout 输出信息和它实际输出信息之间可能会有一个暂停。如果您使用 std::cout 进行输出，然后您的程序随后立即崩溃，则 std::cout 可能已经或可能还没有实际输出。这可能会误导您了解问题出在哪里。另一方面，std::cerr 是无缓冲的，这意味着您发送给它的任何内容都会立即输出。这有助于确保所有调试输出尽快出现（以一些性能为代价，我们在调试时通常不关心）

使用 std::cerr 还有助于明确输出的信息是针对错误情况而不是正常情况

添加临时调试语句时，不缩进它们会很有帮助。这使它们更容易在以后找到并移除

```cpp
#include <iostream>

int getValue()
{
std::cerr << "getValue() called\n";
	return 4;
}

int main()
{
std::cerr << "main() called\n";
    std::cout << getValue;

    return 0;
}
```

### 调试策略 3：打印值

对于某些类型的错误，程序可能会计算或传递错误的值。

我们还可以输出变量（包括参数）或表达式的值，以确保它们是正确的。

For example:

```cpp
#include <iostream>

int add(int x, int y)
{
std::cerr << "add() called (x=" << x <<", y=" << y << ")\n";
	return x + y;
}

void printResult(int z)
{
	std::cout << "The answer is: " << z << '\n';
}

int getUserInput()
{
	std::cout << "Enter a number: ";
	int x{};
	std::cin >> x;
	return x;
}

int main()
{
	int x{ getUserInput() };
std::cerr << "main::x = " << x << '\n';
	int y{ getUserInput() };
std::cerr << "main::y = " << y << '\n';

	std::cout << x << " + " << y << '\n';

	int z{ add(x, 5) };
std::cerr << "main::z = " << z << '\n';
	printResult(z);

	return 0;
}
```

现在我们将得到输出：

```
Enter a number: 4
main::x = 4
Enter a number: 3
main::y = 3
add() called (x=4, y=5)
main::z = 9
The answer is: 9
```

虽然为诊断目的向程序添加调试语句是一种常见的基本技术，也是一种功能性技术（尤其是当调试器由于某种原因不可用时），但由于多种原因它并不是很好：

1. 调试语句使您的代码混乱。
2. 调试语句使程序的输出混乱。
3. 调试语句必须在完成后删除，这使得它们不可重用。
4. 调试语句需要修改您的代码以添加和删除，这可能会引入新的错误。

### 条件化调试代码

一种更容易在整个程序中禁用和启用调试的方法是使用预处理器指令使调试语句有条件：

```cpp
#include <iostream>

#define ENABLE_DEBUG // comment out to disable debugging

int getUserInput()
{
#ifdef ENABLE_DEBUG
std::cerr << "getUserInput() called\n";
#endif
	std::cout << "Enter a number: ";
	int x{};
	std::cin >> x;
	return x;
}

int main()
{
#ifdef ENABLE_DEBUG
std::cerr << "main() called\n";
#endif
    int x{ getUserInput() };
    std::cout << "You entered: " << x;

    return 0;
}
```

现在我们可以通过注释/取消注释#define ENABLE\_DEBUG 来启用调试。这使我们能够重用以前添加的调试语句，然后在用完它们后将它们禁用，而不必从代码中实际删除它们

这解决了必须删除调试语句的问题以及这样做的风险，但代价是代码更加混乱。这种方法的另一个缺点是，如果您输入错误（例如拼错“DEBUG”）或忘记将标头包含到代码文件中，则可能无法启用该文件的部分或全部调试

### 使用日志

通过预处理器进行条件化调试的另一种方法是将调试信息发送到日志文件。日志文件是记录软件中发生的事件的文件（通常存储在磁盘上）。将信息写入日志文件的过程称为日志记录。大多数应用程序和操作系统都会写入可用于帮助诊断发生的问题的日志文件

日志文件有几个优点。因为写入日志文件的信息与程序的输出是分开的，所以可以避免将正常输出和调试输出混合在一起造成的混乱。日志文件也可以很容易地发送给其他人进行诊断——所以如果有人使用你的软件有问题，你可以让他们把日志文件发给你，这可能会帮助你找到问题所在的线索

虽然您可以编写自己的代码来创建日志文件并将输出发送给它们，但最好还是使用许多现有的第三方日志记录工具之一

Using the [plog](https://github.com/SergiusTheBest/plog) logger：

```cpp
#include <iostream>
#include <plog/Log.h> // Step 1: include the logger headers
#include <plog/Initializers/RollingFileInitializer.h>

int getUserInput()
{
	PLOGD << "getUserInput() called"; // PLOGD is defined by the plog library

	std::cout << "Enter a number: ";
	int x{};
	std::cin >> x;
	return x;
}

int main()
{
	plog::init(plog::debug, "Logfile.txt"); // Step 2: initialize the logger

	PLOGD << "main() called"; // Step 3: Output to the log as if you were writing to the console

	int x{ getUserInput() };
	std::cout << "You entered: " << x;

	return 0;
}
```

这是上述记录器的输出（在 `Logfile.txt` 文件中）：

```
2018-12-26 20:03:33.295 DEBUG [4752] [main@14] main() called
2018-12-26 20:03:33.296 DEBUG [4752] [getUserInput@4] getUserInput() called
```

您如何包含、初始化和使用记录器将根据您选择的特定记录器而有所不同

请注意，使用此方法也不需要条件编译指令，因为大多数记录器都有减少/消除将输出写入日志的方法。这使得代码更容易阅读，因为条件编译行增加了很多混乱。使用 plog，可以通过将 init 语句更改为以下内容来暂时禁用日志记录：

```cpp
plog::init(plog::none , "Logfile.txt"); // plog::none eliminates writing of most messages, essentially turning logging off
```

## 使用集成调试器：Stepping（步进）

当您运行您的程序时，执行从 main 函数的顶部开始，然后逐个语句按顺序执行，直到程序结束。在你的程序运行的任何时间点，程序都在跟踪很多事情：你正在使用的变量的值，调用了哪些函数（这样当这些函数返回时，程序就会知道在哪里返回），以及程序中的当前执行点（因此它知道接下来要执行哪个语句）。所有这些跟踪信息都称为您的程序状态（或简称为状态）

**Stepping** 是一组相关调试器功能的名称，这些功能让我们逐条语句地执行（单步执行）我们的代码

箭头标记表示接下来将执行所指向的行

### Step into

**步入** 命令执行程序正常执行路径中的下一条语句，然后暂停程序的执行，以便我们可以使用调试器检查程序的状态。如果正在执行的语句包含一个函数调用，step into 会使程序跳转到被调用函数的顶部，并在那里暂停

### Step over

同**步入**命令一样，**步过**命令执行程序正常执行路径中的下一条语句。然而，step into 将输入函数调用并逐行执行它们，step over 将不间断地执行整个函数，并在函数执行后将控制权返回给您

step over 命令提供了一种方便的方法来跳过函数，当您确定它们已经工作或现在对调试它们不感兴趣时

### Step out

与其他两个步进命令不同，**步出** 命令不只是执行下一行代码。相反，它**执行当前正在执行的函数中的所有剩余代码**，然后在函数返回时将控制权返回给您

当你不小心进入了一个你不想调试的函数时，这个命令最有用

## 使用集成调试器：Running and breakpoints（运行和断点）

### Run to cursor

第一个有用的命令通常称为**运行到光标**。此运行到光标命令执行程序，直到执行到达光标选择的语句。然后它将控制权返回给您，以便您可以从那时开始进行调试

### Breakpoints

**断点** 是一个特殊的标记，它告诉调试器在调试模式下运行时在断点处停止执行程序

## 使用集成调试器：调用堆栈

调用堆栈是为到达当前执行点而调用的所有活动函数的列表。调用堆栈包括每个调用的函数的条目，以及函数返回时将返回到哪一行代码。每当调用一个新函数时，该函数就会被添加到调用堆栈的顶部。当前函数返回给调用者时，它会从调用栈的顶部移除，控制权会返回到它下面的函数

调用堆栈窗口是一个显示当前调用堆栈的调试器窗口

函数名称后的行号显示了每个函数中要执行的下一行

由于调用堆栈的顶部条目代表当前正在执行的函数，因此此处的行号显示了执行恢复时将执行的下一行。调用堆栈中的其余条目表示将在某个时间点返回的函数，因此这些条目的行号表示函数返回后将执行的下一条语句

## 在问题成为问题之前发现问题

### 重构你的代码

当您向程序添加新功能（“行为更改”）时，您会发现某些功能的长度会增加。随着函数越来越长，它们变得越来越复杂，也越来越难以理解

解决此问题的一种方法是将单个长函数分解为多个较短的函数。这种在不改变代码行为的情况下对代码进行结构更改的过程（通常是为了使您的程序更有组织性、模块化或性能）称为**重构**

一个函数占据一个垂直屏幕的代码通常被认为太长了——如果你必须滚动才能阅读整个函数，函数的可理解性会显着下降。但越短越好 -- 少于十行的功能是好的。少于五行的函数就更好了

更改代码时，**进行行为更改或结构更改**，然后重新测试正确性。同时进行行为和结构更改往往会导致更多错误以及更难发现的错误

### 约束

基于约束的技术涉及添加一些额外的代码（如果需要，可以在非调试构建中编译出来）以检查是否违反了某些假设或期望集

例如，如果我们正在编写一个函数来计算一个数字的阶乘，它需要一个非负参数，该函数可以检查以确保调用者在继续之前传递了一个非负数。如果调用者传入一个负数，那么该函数可能会立即出错，而不是产生一些不确定的结果，从而有助于确保立即发现问题

一种常见的方法是通过 assert 和 static\_assert