# 🍉 Config new project

* [https://www.learncpp.com/](https://www.learncpp.com/)
* [https://github.com/Xancoding/Learning-Cpp](https://github.com/Xancoding/Learning-Cpp)

1. 当您在 IDE 中创建新项目时，大多数 IDE 会为您设置两种不同的构建配置：发布配置和调试配置，开发程序时使用调试构建配置。当您准备好将可执行文件发布给其他人时，或者想要测试性能时，请使用发布构建配置
   1. Debug：**调试版本，** 包含调试信息，所以 **容量比Release大很多，** 并且不进行任何优化（优化会使调试复杂化，因为源代码和生成的指令间关系会更复杂），便于程序员调试
   2. Release：**发布版本，** 不对源代码进行调试，编译时对应用程序的速度进行优化，**使得程序在代码大小和运行速度上都是最优的**

```
对于 CLion 用户，您可以添加 Release：按 Alt+Ctrl+S 调出 Settings > Build，Execution，Deployment > CMake > 单击 Profiles 部分中的 + 按钮，将自动添加 Release 选项
```

2. **禁用编译器扩展**以确保您的程序（和编码实践）保持符合 C++ 标准并且可以在任何系统上运行

```
CLion 使用 CMake 构建项目。
将行 set(CMAKE_CXX_EXTENSIONS OFF) 添加到 CMakeList.txt 以禁用扩展
```

3. **将你的警告级别调到最大**，尤其是在你学习的时候。它将帮助您识别可能的问题

```
在 Clion（从 2021.3 版本开始）中，要将警告级别调到最高，您可以按 Alt+Ctrl+S 调出 Settings ，然后选择 Editor > Inspections > C/C++，然后手动选中所有框，或者在窗口的右侧，在矩形的 severity 框中选择错误，然后在另一个框中选择 In All Scopes
```

4. **选择语言标准**：在专业环境中，通常选择比最新标准低一个或两个版本的语言标准（例如，现在 C++20 已经出来了，这意味着 C++14 或 C++17）。这样做通常是为了确保编译器制造商有机会解决缺陷，以便更好地理解新功能的最佳实践。在相关的情况下，这也有助于确保更好的跨平台兼容性，因为某些平台上的编译器可能不会立即为更新的语言标准提供全面支持。启用 C++17 语言标准（或更高版本）后，您应该能够在没有任何警告或错误的情况下编译以下代码

```cpp
#include <array>
#include <iostream>
#include <string_view>
#include <tuple>
#include <type_traits>

namespace a::b::c
{
    inline constexpr std::string_view str{ "hello" };
}

template <class... T>
std::tuple<std::size_t, std::common_type_t<T...>> sum(T... args)
{
    return { sizeof...(T), (args + ...) };
}

int main()
{
    auto [iNumbers, iSum]{ sum(1, 2, 3) };
    std::cout << a::b::c::str << ' ' << iNumbers << ' ' << iSum << '\n';

    std::array arr{ 1, 2, 3 };

    std::cout << std::size(arr) << '\n';

    return 0;
}
```
