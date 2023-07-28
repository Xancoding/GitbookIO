# 🥭 Bit Manipulation (optional chapter)

## 位标志 and 位操作 via std::bitset

要定义一组位标志，我们通常会使用适当大小的无符号整数（8 位、16 位、32 位等……取决于我们有多少标志），或 std::bitset

```cpp
#include <bitset> // for std::bitset

std::bitset<8> mybitset {}; // 8 bits in size means room for 8 flags
```

位操作是您应该明确使用无符号整数（或 std::bitset）的少数情况之一

std::bitset 提供了 4 个可用于位操作的关键函数：

* test() 允许我们查询某个位是 0 还是 1
* set() 允许我们打开一个位（如果位已经打开，这将不执行任何操作）
* reset() 允许我们关闭一个位（如果该位已经关闭，这将不执行任何操作）
* flip() 允许我们将位值从 0 翻转为 1，反之亦然

这些函数中的每一个都将我们要操作的位的位置作为它们唯一的参数

```cpp
#include <bitset>
#include <iostream>

int main()
{
    std::bitset<8> bits{ 0b0000'0101 }; // we need 8 bits, start with bit pattern 0000 0101
    bits.set(3); // set bit position 3 to 1 (now we have 0000 1101)
    bits.flip(4); // flip bit 4 (now we have 0001 1101)
    bits.reset(4); // set bit 4 back to 0 (now we have 0000 1101)

    std::cout << "All the bits: " << bits << '\n';
    std::cout << "Bit 3 has value: " << bits.test(3) << '\n';
    std::cout << "Bit 4 has value: " << bits.test(4) << '\n';

    return 0;
}
```

## 按位运算符

![](https://bu.dusays.com/2022/11/27/638344de90243.png) 为避免意外，请使用无符号操作数或 std::bitset 的按位运算符

在计算按位 XOR 时，如果一列中有奇数个 1 位，则该列的结果为 1

## 位掩码

位掩码是一组预定义的位，用于选择哪些特定位将被后续操作修改。位掩码阻止按位运算符接触我们不想修改的位，并允许访问我们确实想要修改的位

最简单的一组位掩码是为每个位位置定义一个位掩码。我们用 0 来屏蔽我们不关心的位，用 1 来表示我们想要修改的位

尽管位掩码可以是文字，但它们通常被定义为符号常量，因此可以为它们指定一个有意义的名称并易于重用

### 在 C++14 中定义位掩码

因为 C++14 支持二进制文字，所以定义这些位掩码很容易：

```cpp
#include <cstdint>

constexpr std::uint8_t mask0{ 0b0000'0001 }; // represents bit 0
constexpr std::uint8_t mask1{ 0b0000'0010 }; // represents bit 1
constexpr std::uint8_t mask2{ 0b0000'0100 }; // represents bit 2
constexpr std::uint8_t mask3{ 0b0000'1000 }; // represents bit 3
constexpr std::uint8_t mask4{ 0b0001'0000 }; // represents bit 4
constexpr std::uint8_t mask5{ 0b0010'0000 }; // represents bit 5
constexpr std::uint8_t mask6{ 0b0100'0000 }; // represents bit 6
constexpr std::uint8_t mask7{ 0b1000'0000 }; // represents bit 7
```

### 在 C++11 或更早版本中定义位掩码

由于 C++11 不支持二进制文字，我们必须使用其他方法来设置符号常量

第一种方法是使用十六进制文字：

```cpp
constexpr std::uint8_t mask0{ 0x01 }; // hex for 0000 0001
constexpr std::uint8_t mask1{ 0x02 }; // hex for 0000 0010
constexpr std::uint8_t mask2{ 0x04 }; // hex for 0000 0100
constexpr std::uint8_t mask3{ 0x08 }; // hex for 0000 1000
constexpr std::uint8_t mask4{ 0x10 }; // hex for 0001 0000
constexpr std::uint8_t mask5{ 0x20 }; // hex for 0010 0000
constexpr std::uint8_t mask6{ 0x40 }; // hex for 0100 0000
constexpr std::uint8_t mask7{ 0x80 }; // hex for 1000 0000
```

另一种更简单的方法是使用左移运算符将一位移动到正确的位置：

```cpp
constexpr std::uint8_t mask0{ 1 << 0 }; // 0000 0001
constexpr std::uint8_t mask1{ 1 << 1 }; // 0000 0010
constexpr std::uint8_t mask2{ 1 << 2 }; // 0000 0100
constexpr std::uint8_t mask3{ 1 << 3 }; // 0000 1000
constexpr std::uint8_t mask4{ 1 << 4 }; // 0001 0000
constexpr std::uint8_t mask5{ 1 << 5 }; // 0010 0000
constexpr std::uint8_t mask6{ 1 << 6 }; // 0100 0000
constexpr std::uint8_t mask7{ 1 << 7 }; // 1000 0000
```

### Testing a bit

要确定某个位是开还是关，我们使用 `&` 结合相应位的位掩码：

```cpp
#include <cstdint>
#include <iostream>

int main()
{
	constexpr std::uint8_t mask0{ 0b0000'0001 }; // represents bit 0
	constexpr std::uint8_t mask1{ 0b0000'0010 }; // represents bit 1
	constexpr std::uint8_t mask2{ 0b0000'0100 }; // represents bit 2
	constexpr std::uint8_t mask3{ 0b0000'1000 }; // represents bit 3
	constexpr std::uint8_t mask4{ 0b0001'0000 }; // represents bit 4
	constexpr std::uint8_t mask5{ 0b0010'0000 }; // represents bit 5
	constexpr std::uint8_t mask6{ 0b0100'0000 }; // represents bit 6
	constexpr std::uint8_t mask7{ 0b1000'0000 }; // represents bit 7

	std::uint8_t flags{ 0b0000'0101 }; // 8 bits in size means room for 8 flags

	std::cout << "bit 0 is " << ((flags & mask0) ? "on\n" : "off\n");
	std::cout << "bit 1 is " << ((flags & mask1) ? "on\n" : "off\n");

	return 0;
}
```

### Setting a bit

要设置（打开）位，我们将按位或等于（运算符 `|=`）与相应位的位掩码结合使用：

```cpp
#include <cstdint>
#include <iostream>

int main()
{
    constexpr std::uint8_t mask0{ 0b0000'0001 }; // represents bit 0
    constexpr std::uint8_t mask1{ 0b0000'0010 }; // represents bit 1
    constexpr std::uint8_t mask2{ 0b0000'0100 }; // represents bit 2
    constexpr std::uint8_t mask3{ 0b0000'1000 }; // represents bit 3
    constexpr std::uint8_t mask4{ 0b0001'0000 }; // represents bit 4
    constexpr std::uint8_t mask5{ 0b0010'0000 }; // represents bit 5
    constexpr std::uint8_t mask6{ 0b0100'0000 }; // represents bit 6
    constexpr std::uint8_t mask7{ 0b1000'0000 }; // represents bit 7

    std::uint8_t flags{ 0b0000'0101 }; // 8 bits in size means room for 8 flags

    std::cout << "bit 1 is " << ((flags & mask1) ? "on\n" : "off\n");

    flags |= mask1; // turn on bit 1

    std::cout << "bit 1 is " << ((flags & mask1) ? "on\n" : "off\n");

    return 0;
}
```

我们还可以使用按位或同时打开多个位：

```cpp
flags |= (mask4 | mask5); // turn bits 4 and 5 on at the same time
```

### Resetting a bit

要清除位（关闭），我们同时使用 `&=` 和 `~` ：

```cpp
#include <cstdint>
#include <iostream>

int main()
{
    constexpr std::uint8_t mask0{ 0b0000'0001 }; // represents bit 0
    constexpr std::uint8_t mask1{ 0b0000'0010 }; // represents bit 1
    constexpr std::uint8_t mask2{ 0b0000'0100 }; // represents bit 2
    constexpr std::uint8_t mask3{ 0b0000'1000 }; // represents bit 3
    constexpr std::uint8_t mask4{ 0b0001'0000 }; // represents bit 4
    constexpr std::uint8_t mask5{ 0b0010'0000 }; // represents bit 5
    constexpr std::uint8_t mask6{ 0b0100'0000 }; // represents bit 6
    constexpr std::uint8_t mask7{ 0b1000'0000 }; // represents bit 7

    std::uint8_t flags{ 0b0000'0101 }; // 8 bits in size means room for 8 flags

    std::cout << "bit 2 is " << ((flags & mask2) ? "on\n" : "off\n");

    flags &= ~mask2; // turn off bit 2

    std::cout << "bit 2 is " << ((flags & mask2) ? "on\n" : "off\n");

    return 0;
}
```

我们可以同时关闭多个位：

```cpp
flags &= ~(mask4 | mask5); // turn bits 4 and 5 off at the same time
```

### Flipping a bit

要切换位状态，我们使用 `^=`：

```cpp
#include <cstdint>
#include <iostream>

int main()
{
    constexpr std::uint8_t mask0{ 0b0000'0001 }; // represents bit 0
    constexpr std::uint8_t mask1{ 0b0000'0010 }; // represents bit 1
    constexpr std::uint8_t mask2{ 0b0000'0100 }; // represents bit 2
    constexpr std::uint8_t mask3{ 0b0000'1000 }; // represents bit 3
    constexpr std::uint8_t mask4{ 0b0001'0000 }; // represents bit 4
    constexpr std::uint8_t mask5{ 0b0010'0000 }; // represents bit 5
    constexpr std::uint8_t mask6{ 0b0100'0000 }; // represents bit 6
    constexpr std::uint8_t mask7{ 0b1000'0000 }; // represents bit 7

    std::uint8_t flags{ 0b0000'0101 }; // 8 bits in size means room for 8 flags

    std::cout << "bit 2 is " << ((flags & mask2) ? "on\n" : "off\n");
    flags ^= mask2; // flip bit 2
    std::cout << "bit 2 is " << ((flags & mask2) ? "on\n" : "off\n");
    flags ^= mask2; // flip bit 2
    std::cout << "bit 2 is " << ((flags & mask2) ? "on\n" : "off\n");

    return 0;
}
```

我们可以同时翻转多个位：

```cpp
flags ^= (mask4 | mask5); // flip bits 4 and 5 at the same time
```

## 位掩码和 std::bitset

std::bitset 支持全套位运算符。因此，尽管使用函数（测试、设置、重置和翻转）修改单个位更容易，但如果需要，您可以使用按位运算符和位掩码

函数只允许您一次修改单个位。按位运算符允许您一次修改多个位

```cpp
#include <cstdint>
#include <iostream>
#include <bitset>

int main()
{
	constexpr std::bitset<8> mask0{ 0b0000'0001 }; // represents bit 0
	constexpr std::bitset<8> mask1{ 0b0000'0010 }; // represents bit 1
	constexpr std::bitset<8> mask2{ 0b0000'0100 }; // represents bit 2
	constexpr std::bitset<8> mask3{ 0b0000'1000 }; // represents bit 3
	constexpr std::bitset<8> mask4{ 0b0001'0000 }; // represents bit 4
	constexpr std::bitset<8> mask5{ 0b0010'0000 }; // represents bit 5
	constexpr std::bitset<8> mask6{ 0b0100'0000 }; // represents bit 6
	constexpr std::bitset<8> mask7{ 0b1000'0000 }; // represents bit 7

	std::bitset<8> flags{ 0b0000'0101 }; // 8 bits in size means room for 8 flags
	std::cout << "bit 1 is " << (flags.test(1) ? "on\n" : "off\n");
	std::cout << "bit 2 is " << (flags.test(2) ? "on\n" : "off\n");

	flags ^= (mask1 | mask2); // flip bits 1 and 2
	std::cout << "bit 1 is " << (flags.test(1) ? "on\n" : "off\n");
	std::cout << "bit 2 is " << (flags.test(2) ? "on\n" : "off\n");

	flags |= (mask1 | mask2); // turn bits 1 and 2 on
	std::cout << "bit 1 is " << (flags.test(1) ? "on\n" : "off\n");
	std::cout << "bit 2 is " << (flags.test(2) ? "on\n" : "off\n");

	flags &= ~(mask1 | mask2); // turn bits 1 and 2 off
	std::cout << "bit 1 is " << (flags.test(1) ? "on\n" : "off\n");
	std::cout << "bit 2 is " << (flags.test(2) ? "on\n" : "off\n");

	return 0;
}
```

## Summary

1. query bit states

```cpp
if (flags & option4) ... // if option4 is set, do something
```

2. set bits (turn on)

```cpp
flags |= option4; // turn option 4 on.
flags |= (option4 | option5); // turn options 4 and 5 on.
```

3. clear bits (turn off)

```cpp
flags &= ~option4; // turn option 4 off
flags &= ~(option4 | option5); // turn options 4 and 5 off
```

4. flip bit states

```cpp
flags ^= option4; // flip option4 from on to off, or vice versa
flags ^= (option4 | option5); // flip options 4 and 5
```
