# 🍇 Custom

### 注释

#### 单行注释

1. 在行的右侧添加注释会使代码和注释都难以阅读，尤其是在行很长的情况下。如果行相当短，评论可以简单地对齐（通常是一个制表位），像这样：

```cpp
std::cout << "Hello world!\n";                 // std::cout lives in the iostream library
std::cout << "It is very nice to meet you!\n"; // this is much easier to read
std::cout << "Yeah!\n";                        // don't you think so?
```

2. 但是，如果行很长，将注释放在右边会使行变得很长。在这种情况下，单行注释通常放在它正在注释的行之上：

```cilkcpp
// std::cout lives in the iostream library
std::cout << "Hello world!\n";

// this is much easier to read
std::cout << "It is very nice to meet you!\n";

// don't you think so?
std::cout << "Yeah!\n";
```

3. Commenting out code

```cpp
//    std::cout << 1;
```

#### 多行注释

1. Explaining code

```cpp
/* This is a multi-line comment.
 * the matching asterisks to the left
 * can make this easier to read
 */
```

2. Commenting out code

```cpp
/*
    std::cout << 1;
    std::cout << 2;
    std::cout << 3;
*/
```

### 代码的可读性

1. 您的行的长度不应超过 80 个字符

```cpp
int main()
{
    std::cout << "This is a really, really, really, really, really, really, really, "
        "really long line\n"; // one extra indentation for continuation line

    std::cout << "This is another really, really, really, really, really, really, really, "
                 "really long line\n"; // text aligned with the previous line for continuation line

    std::cout << "This one is short\n";
}
```

如果用操作符（例如 << 或 +）拆分长行，则应将操作符放在下一行的开头，而不是当前行的结尾

```cpp
std::cout << 3 + 4
    + 5 + 6
    * 7 * 8;
```

许多编辑器都有一个内置功能（或插件/扩展），可以在给定的列（例如 80 个字符）处显示一行（称为“列指南”），因此您可以轻松查看行何时变得太长

```
Clion column guide : 
File > Settings > Editor > Code Style > General > 在 "Visual guides" 矩形框中键入 80 并应用更改
```

2. 使用空格通过对齐值或注释或在代码块之间添加间距来使您的代码更易于阅读

```cpp
cost          = 57;
pricePerItem  = 24;
value         = 5;
numberOfItems = 17;

std::cout << "Hello world!\n";                  // cout lives in the iostream library
std::cout << "It is very nice to meet you!\n";  // these comments are easier to read
std::cout << "Yeah!\n";                         // especially when all lined up

// cout lives in the iostream library
std::cout << "Hello world!\n";

// these comments are easier to read
std::cout << "It is very nice to meet you!\n";

// when separated by whitespace
std::cout << "Yeah!\n";
```
