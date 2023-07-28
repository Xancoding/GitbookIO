# 🥥 Shallow copy and deep copy

编译器提供的默认复制构造函数和默认赋值运算符如下所示：

```cpp
#include <cassert>
#include <iostream>

class Fraction
{
private:
    int m_numerator { 0 };
    int m_denominator { 1 };

public:
    // Default constructor
    Fraction(int numerator = 0, int denominator = 1)
        : m_numerator{ numerator }
        , m_denominator{ denominator }
    {
        assert(denominator != 0);
    }

    // Possible implementation of implicit copy constructor
    Fraction(const Fraction& f)
        : m_numerator{ f.m_numerator }
        , m_denominator{ f.m_denominator }
    {
    }

    // Possible implementation of implicit assignment operator
    Fraction& operator= (const Fraction& fraction)
    {
        // self-assignment guard
        if (this == &fraction)
            return *this;

        // do the copy
        m_numerator = fraction.m_numerator;
        m_denominator = fraction.m_denominator;

        // return the existing object so we can chain this operator
        return *this;
    }

    friend std::ostream& operator<<(std::ostream& out, const Fraction& f1)
    {
	out << f1.m_numerator << '/' << f1.m_denominator;
	return out;
    }
};
```

请注意，因为这些默认版本可以很好地复制此类，所以在这种情况下真的没有理由编写我们自己的这些函数版本

然而，在设计处理动态分配内存的类时，成员（浅）复制会给我们带来很多麻烦！这是因为指针的浅拷贝只是复制指针的地址——它不分配任何内存或复制指向的内容！

深拷贝为副本分配内存，然后复制实际值，以便副本位于与源不同的内存中。这样，副本和来源是截然不同的，不会以任何方式相互影响。进行深度复制需要我们编写自己的复制构造函数和重载赋值运算符。

默认复制构造函数和默认赋值运算符执行浅拷贝，这适用于不包含动态分配变量的类。\
具有动态分配变量的类需要有一个复制构造函数和赋值运算符来执行深复制。\
喜欢使用标准库中的类而不是自己进行内存管理。

**深拷贝**为副本分配内存，然后复制实际值，以便副本位于与源不同的内存中。这样，副本和来源是截然不同的，不会以任何方式相互影响。进行深度复制需要我们编写自己的复制构造函数和重载赋值运算符。

```cpp
// assumes m_data is initialized
void MyString::deepCopy(const MyString& source)
{
    // first we need to deallocate any value that this string is holding!
    delete[] m_data;

    // because m_length is not a pointer, we can shallow copy it
    m_length = source.m_length;

    // m_data is a pointer, so we need to deep copy it if it is non-null
    if (source.m_data)
    {
        // allocate memory for our copy
        m_data = new char[m_length];

        // do the copy
        for (int i{ 0 }; i < m_length; ++i)
            m_data[i] = source.m_data[i];
    }
    else
        m_data = nullptr;
}

// Copy constructor
MyString::MyString(const MyString& source)
{
    deepCopy(source);
}
```

这比简单的浅拷贝要复杂得多！

现在让我们做重载的赋值运算符。重载的赋值运算符有点棘手：

```cpp
// Assignment operator
MyString& MyString::operator=(const MyString& source)
{
    // check for self-assignment
    if (this != &source)
    {
        // now do the deep copy
        deepCopy(source);
    }

    return *this;
}
```
