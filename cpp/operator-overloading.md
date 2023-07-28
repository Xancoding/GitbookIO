# 🍍 Operator overloading

## 何时使用普通函数、友元函数或成员函数重载运算符?

1. 在处理不修改左操作数的二元运算符（例如 operator+）时，通常首选普通或友元函数版本，因为它适用于所有参数类型（即使左操作数不是类对象，或者是一个不可修改的类）。普通或友元函数版本具有“对称”的额外好处，因为所有操作数都成为显式参数（而不是左操作数成为 `*this` 而右操作数成为显式参数）
2. 在处理确实修改左操作数的二元运算符时（例如 operator+=），通常首选成员函数版本。在这些情况下，最左边的操作数将始终是类类型，并且让被修改的对象成为 `*this` 指向的对象是很自然的。因为最右边的操作数成为一个显式参数，所以不会混淆谁正在修改和谁正在评估
3. 一元运算符通常也作为成员函数重载，因为成员函数版本没有参数
4. 以下经验法则可以帮助您确定哪种形式最适合给定情况：
   1. 如果要重载赋值 (=)、下标 (\[])、函数调用 (()) 或成员选择 (->)，请将其作为成员函数进行重载
   2. 如果要重载一元运算符，请将其作为成员函数
   3. 如果要重载不修改其左操作数的二元运算符（例如 operator+），请将其作为普通函数（首选）或友元函数
   4. 如果您正在重载修改其左操作数的二元运算符，但您不能将成员添加到左操作数的类定义中（例如，operator<<，它有一个 ostream 类型的左操作数），请像往常一样这样做函数（首选）或友元函数
   5. 如果您正在重载修改其左操作数的二元运算符（例如 operator+=），并且您可以修改左操作数的定义，请将其作为成员函数进行

## 最小化比较冗余

也就是说我们只需要实现`operator==`和`operator<`的逻辑，其他四个比较运算符就可以根据这两个来定义了！这是一个更新的 `Cents` 示例，说明了这一点：

```cpp
#include <iostream>

class Cents
{
private:
    int m_cents;

public:
    Cents(int cents)
        : m_cents{ cents }
    {}

    friend bool operator== (const Cents& c1, const Cents& c2) { return c1.m_cents == c2.m_cents; };
    friend bool operator!= (const Cents& c1, const Cents& c2) { return !(operator==(c1, c2)); };

    friend bool operator< (const Cents& c1, const Cents& c2) { return c1.m_cents < c2.m_cents; };
    friend bool operator> (const Cents& c1, const Cents& c2) { return operator<(c2, c1); };

    friend bool operator<= (const Cents& c1, const Cents& c2) { return !(operator>(c1, c2)); };
    friend bool operator>= (const Cents& c1, const Cents& c2) { return !(operator<(c1, c2)); };

};

int main()
{
    Cents dime{ 10 };
    Cents nickel{ 5 };

    if (nickel > dime)
        std::cout << "a nickel is greater than a dime.\n";
    if (nickel >= dime)
        std::cout << "a nickel is greater than or equal to a dime.\n";
    if (nickel < dime)
        std::cout << "a dime is greater than a nickel.\n";
    if (nickel <= dime)
        std::cout << "a dime is greater than or equal to a nickel.\n";
    if (nickel == dime)
        std::cout << "a dime is equal to a nickel.\n";
    if (nickel != dime)
        std::cout << "a dime is not equal to a nickel.\n";

    return 0;
}
```

这样，如果我们需要更改某些内容，我们只需要更新 operator== 和 operator< 而不是所有六个比较运算符！
