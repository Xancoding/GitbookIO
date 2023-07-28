# 🍑 Operators

## `,` & `? :` 运算符

C++ 没有定义函数参数或运算符操作数的计算顺序

不要在给定语句中多次使用具有副作用的变量。如果这样做，结果可能是未定义的

逗号在所有运算符中的优先级最低，甚至低于赋值

请注意， `? :` 运算符的优先级非常低。如果除了将结果分配给变量之外做任何事情，整个 `? :` 运算符也需要用括号括起来

```cpp
std::cout << ((x > y) ? x : y) << '\n';
```

如果在上述情况下我们不将整个条件运算符括起来会发生什么。因为 << 运算符的优先级高于 ?: 运算符，所以语句：

```cpp
std::cout << (x > y) ? x : y << '\n';
```

将评估为：

```cpp
(std::cout << (x > y)) ? x : y << '\n';
```

## 比较浮点数大小

进行浮点相等的最常见方法涉及使用一个函数来查看两个数字是否几乎相同。如果它们“足够接近”，那么我们称它们相等。用于表示“足够接近”的值传统上称为 epsilon。 Epsilon 通常被定义为一个小的正数（例如 0.00000001，有时写作 1e-8）

```cpp
#include <cmath> // for std::abs()

// epsilon is an absolute value
bool approximatelyEqualAbs(double a, double b, double absEpsilon)
{
    // if the distance between a and b is less than absEpsilon, then a and b are "close enough"
    return std::abs(a - b) <= absEpsilon;
}
```

虽然这个功能可以工作，但不是很好。 0.00001 的 epsilon 适用于 1.0 左右的输入，对于 0.0000001 左右的输入太大，对于 10,000 这样的输入太小

著名计算机科学家唐纳德·高德纳 (Donald Knuth) 在他的著作“计算机编程的艺术，第二卷：半数值算法 (Addison-Wesley, 1969)”一书中提出了以下方法：

```cpp
#include <algorithm> // std::max
#include <cmath> // std::abs

// return true if the difference between a and b is within epsilon percent of the larger of a and b
bool approximatelyEqualRel(double a, double b, double relEpsilon)
{
    return (std::abs(a - b) <= (std::max(std::abs(a), std::abs(b)) * relEpsilon));
}
```

在这种情况下，epsilon 不是绝对数字，而是相对于 a 或 b 的大小。在 <= 运算符的左侧，std::abs(a - b) 告诉我们 a 和 b 之间的距离为正数。在 <= 运算符的右侧，我们需要计算我们愿意接受的“足够接近”的最大值。为此，该算法选择 a 和 b 中较大的一个（作为数字总体大小的粗略指标），然后将其乘以 relEpsilon。在此函数中，relEpsilon 表示百分比。例如，如果我们想说“足够接近”意味着 a 和 b 在 a 和 b 中较大者的 1% 以内，我们传入 0.01 (1% = 1/100 = 0.01) 的 relEpsilon。 relEpsilon 的值可以根据情况调整为最合适的值（例如，0.002 的 epsilon 表示在 0.2% 以内）

要执行不等式 (!=) 而不是相等，只需调用此函数并使用逻辑 NOT 运算符 (!) 翻转结果：

```cpp
if (!approximatelyEqualRel(a, b, 0.001))
    std::cout << a << " is not equal to " << b << '\n';
```

虽然 approximatelyEqualRel() 函数适用于大多数情况，但它并不完美，尤其是当数字接近零时：

```cpp
#include <algorithm>
#include <cmath>
#include <iostream>

// return true if the difference between a and b is within epsilon percent of the larger of a and b
bool approximatelyEqualRel(double a, double b, double relEpsilon)
{
	return (std::abs(a - b) <= (std::max(std::abs(a), std::abs(b)) * relEpsilon));
}

int main()
{
	// a is really close to 1.0, but has rounding errors, so it's slightly smaller than 1.0
	double a{ 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 };

	// First, let's compare a (almost 1.0) to 1.0.
	std::cout << approximatelyEqualRel(a, 1.0, 1e-8) << '\n';

	// Second, let's compare a-1.0 (almost 0.0) to 0.0
	std::cout << approximatelyEqualRel(a-1.0, 0.0, 1e-8) << '\n';
}
```

这会返回：

```
1
0
```

避免这种情况的一种方法是同时使用绝对 epsilon（如我们在第一种方法中所做的）和相对 epsilon（如我们在 Knuth 的方法中所做的）：

```cpp
// return true if the difference between a and b is less than absEpsilon, or within relEpsilon percent of the larger of a and b
bool approximatelyEqualAbsRel(double a, double b, double absEpsilon, double relEpsilon)
{
    // Check if the numbers are really close -- needed when comparing numbers near zero.
    double diff{ std::abs(a - b) };
    if (diff <= absEpsilon)
        return true;

    // Otherwise fall back to Knuth's algorithm
    return (diff <= (std::max(std::abs(a), std::abs(b)) * relEpsilon));
}
```

在这个算法中，我们首先检查 a 和 b 在绝对值上是否接近，这处理了 a 和 b 都接近于零的情况。 absEpsilon 参数应设置为非常小的值（例如 1e-12）。如果失败，则我们使用相对 epsilon 回退到 Knuth 的算法

浮点数的比较是一个困难的话题，并且没有适用于所有情况的“一刀切”算法。但是，absEpsilon 为 1e-12 和 relEpsilon 为 1e-8 的 approximatesEqualAbsRel() 应该足以处理您将遇到的大多数情况

## 逻辑 XOR 运算符

C++ 不提供逻辑 XOR 运算符。与逻辑或或逻辑与不同，逻辑异或不能进行短路评估。因此，从逻辑 OR 和逻辑 AND 运算符中创建逻辑 XOR 运算符具有挑战性。但是，您可以使用不等运算符 (!=) 轻松模拟逻辑 XOR：

```cpp
if (a != b) ... // a XOR b, assuming a and b are Booleans
```

这可以扩展到多个操作数，如下所示：

```cpp
if (a != b != c != d) ... // a XOR b XOR c XOR d, assuming a, b, c, and d are Booleans
```

请注意，上述 XOR 模式仅在操作数为布尔值（而非整数）时才有效。如果您需要一种适用于非布尔操作数的逻辑 XOR 形式，您可以将它们静态转换为布尔值：

```cpp
if (static_cast<bool>(a) != static_cast<bool>(b) != static_cast<bool>(c) != static_cast<bool>(d)) ... // a XOR b XOR c XOR d, for any type that can be converted to bool
```
