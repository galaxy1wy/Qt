# Qt Creator 编码规则

* [English Version](./README.md)

关于查看官网的Qt Creator 编码规则文档，请点击 https://doc.qt.io/qtcreator-extending/coding-style.html 。

遵循`代码构造`、`格式设计`、`模式和实践`

# 代码构造

遵循代码构造指南，使代码更快、更清晰。此外，这些准则还允许您利用 C++ 中的强类型检查。

- 尽可能首选 `++XXX` 而不是 `XXX++`。`++XXX` 可能比 `XXX++` 快。这条规则也适用于递减：
  ```
    ++T;
    --U;
    
    -NOT-
    
    T++;
    U--;
  ```
  
- 尽量减少重复执行相同代码；这尤其适用于循环：
  ```c++
    Container::iterator end = large.end();
    for (Container::iterator it = large.begin(); it != end; ++it) {
            ...;
    }
    
    -NOT-
    
    for (Container::iterator it = large.begin();it != large.end(); ++it) {
            ...;
    }
  ```

# 代码格式

标识符中使用驼峰式大小写。
  - 类名以大写字母开头
  - 函数名以小写字母开头
  - 变量名称以小写字母开头
  - 枚举名称和值以大写字母开头。无作用域枚举值包含枚举类型名称的某些部分。

空白
  - 使用四个空格进行缩进，不要使用制表符
  - 在合适的地方使用空行将语句分组在一起
  - 始终只使用一个空行

指针和引用
  - 对于指针或引用，请始终在星号 （*） 或与号 （&） 之前使用单个空格，但切勿在之后使用。尽可能避免 C 样式强制转换：
    ```c++
      char *blockOfMemory = (char *)malloc(data.size());
      char *blockOfMemory = reinterpret_cast<char *>(malloc(data.size()));
      
      -NOT-
      
      char* blockOfMemory = (char* ) malloc(data.size());
    ```

运算符名称和括号
  - 不要在运算符名称和括号之间使用空格。等式标记 (==) 是运算符名称的一部分，因此空格会使声明看起来像表达式：
    ```c++
      operator==(type)
      
      -NOT-
      
      operator == (type)
    ```

函数名称和括号
  - 不要在函数名称和括号之间使用空格：
    ```c++
      void mangle()
  
      -NOT-
      
      void mangle ()
    ```
  
关键字
  - 始终在关键字之后和大括号之前使用单个空格：
    ```c++
      if (foo) {
      }
      
      -NOT-
      
      if(foo){
      }
    ```

大括号{}
  - 作为基本规则，将左大括号放在与语句开头相同的行上
  - 例外：函数实现和类声明总是在行的开头有左大括号
    ```c++
      if (codec) {
      }

      static void foo(int g)
      {
          qDebug("foo: %i", g);
      }
      
      class Moo
      {
      };
    ```

  - 当条件语句的主体包含多行时，以及单行语句有点复杂时，请使用大括号。否则，请省略它们：
  - 例外1：如果 条件 语句包含多行或换行，则也使用大括号：
  - 例外2：在 if-code或 else-code 包含多行的 if-then-else 块中也使用大括号：
    ```c++
      if (address.isEmpty())
        return false;
    
      for (int i = 0; i < 10; ++i)
          qDebug("%i", i);

      if (address.isEmpty()
            || !isValid()
            || !codec) {
          return false;
      }

      if (address.isEmpty()) {
          --it;
      } else {
          qDebug("%s", qPrintable(address));
          ++it;
      }
    ```

  - 当条件语句的主体为空时使用大括号

声明
  - 按照以下顺序排列类的访问部分：public、protected、private。类的每个用户都对 public 部分感兴趣。只有类的实现者才对 private 部分感兴趣。
  - 避免在类的声明文件中声明全局对象。如果所有对象都使用相同的变量，请使用 static 成员。
  - 使用 class 而不是 struct。一些编译器将这种差异混淆到符号名称中，如果结构声明后跟类定义，则会发出警告。为了避免从一个到另一个的持续变化，我们以首选方式声明 class。
  - 避免使用类类型的全局变量以排除初始化顺序问题。如果无法避免，请考虑使用 Q_GLOBAL_STATIC。
  - 将全局字符串文本声明为 const 。
  - 尽可能避免使用短名称（例如，a、rbarr、nughdeget）。仅对计数器和临时变量使用单字符变量名，其中变量的用途很明显。
  - 在单独的行中声明每个变量。

命名空间
  - 将左大括号放在与 namespace 关键字相同的行上。
  - 不要在里面缩进声明或定义。
  - 如果命名空间跨越多行，则建议使用：在右大括号后添加注释以重复命名空间（可选）。
  - 如果命名空间内只有一个类声明，则所有类声明都可以在一行上。
    ```c++
      namespace MyPlugin {

      void someFunction() { ... }
      
          }  // namespace MyPlugin


      namespace MyPlugin { class MyClass; }
    ```

  - 不要在头文件中使用 using 指令。
  - 在定义类和函数时，不要依赖 using 指令，而是在正确命名的声明性区域中定义它。
  - 访问全局函数时不要依赖 using 指令。
  - 在其他情况下，尽可能使用 using 指令，因为它们可以帮助您避免代码混乱。最好将所有 using 指令放在文件顶部附近。
    ```c++
      [in foo.cpp]
      ...
      #include "foos.h"
      ...
      #include <utils/filename.h>
      ...
      using namespace Utils;
      
      namespace Foo::Internal {
      
      void SomeThing::bar()
      {
          FilePath f;              // or Utils::FilePath f
          ...
      }
      ...
      } // Foo::Internal           // or // namespace Foo::Internal
    ```

# 模式和实践

命名空间：<br>
  >Qt Creator 中的命名空间策略如下:
    - 导出以供其他库或插件使用的库或插件的类/元件位于特定于该库/插件的命名空间中，例如 MyPlugin
    - 未导出的库或插件的类/元件位于其他 Internal 命名空间中，例如 MyPlugin::Internal

传递文件名：<br>
  >Qt Creator API 要求文件名采用可移植格式，即即使在 Windows 上也使用斜杠 (/) 而不是反斜杠 (\)。要将文件名从用户传递给 API，请先使用 QDir::fromNativeSeparators 进行转换。要向用户显示文件名，请使用 QDir::toNativeSeparators 将其转换回本机格式。考虑使用 Utils::FilePath::fromUserInput(QString) 和 Utils::FilePath::toUserOutput() 执行这些任务。
  >在比较文件名时使用 Utils::FilePath，因为这会考虑区分大小写。还要确保比较干净的路径（QDir::cleanPath()）

要使用的类和不使用的类：<br>
  >Qt Creator 代码的很大一部分处理与开发计算机不同的设备上的数据。这些可能在路径分隔符、行结束符、流程启动详细信息等方面有所不同；但是，一些基本的 Qt 类假定 Qt 应用程序只与类似于开发计算机的机器有关。因此，这些类不适合在 Qt Creator 中与非本地代码相关的部分使用。相反，Qt Creator 的 Utils 库提供了替代品，从而得出以下规则：<br>
    - 将 Utils::FilePath 用于语义上是文件或目录的任何 QString <br>
    - 首选使用 Utils::FilePath，而不是使用 QDir 和 QFileInfo <br>
    - 首选使用 Utils::P rocess 而不是 QProcess  <br>
    - 如果 Utils::FilePath 或 Utils::Process 功能不足以达到您的目的，则最好增强它们，而不是回退到 QString 或 QProcess <br>
    - 避免使用平台 #ifdefs，除非本地执行的代码绝对需要它们，即使这样，也更喜欢 Utils::HostInfo 而不是 #ifdefs <br>

插件依赖项：<br>
  >为了保持 Qt Creator 的可扩展性，我们的目标是尽可能少地保持插件之间和外部库的硬运行时依赖关系

通过回调扩展基本插件功能：<br>
  >此模式允许叶插件通过注入回调来向中央插件提供额外的功能，它用于在某些设置中有用的功能，但在其他设置中不适用或被视为侵入性（例如，由于大小或外部依赖关系），因此用户可能希望在不影响其余设置的情况下启用或禁用它。例如，叶子插件可以依赖于一个大的（或其他不方便的）外部依赖项，而无需将此依赖项强加给中央插件。

插件扩展点：<br>
  >插件扩展点是由一个插件提供的接口，由其他插件实现。然后，该插件会检索接口的所有实现并使用它们。也就是说，它们扩展了插件的功能。通常，接口的实现在插件初始化期间被放入全局对象池中，插件在插件初始化结束时从对象池中检索它们。例如，Find 插件提供了 FindFilter 接口供其他插件实现。使用 FindFilter 界面，可以添加其他搜索范围，这些范围显示在 Advanced Search 对话框中。Find 插件从全局对象池中检索所有 FindFilter 实现，并在对话框中显示它们。该插件将实际的搜索请求转发到正确的 FindFilter 实现，然后执行搜索。

使用全局对象池：<br>
  >可以通过 ExtensionSystem::PluginManager::addObject() 将对象添加到全局对象池中，并通过 ExtensionSystem::PluginManager::getObject() 再次检索特定类型的对象。这应该主要用于 Plugin Extension Points 的实现。

Lambda 表达式：<br>
  >当 lambda 既不接受参数也不指定返回类型时，删除圆括号<br>
  >定义 lambda 时，将方括号与圆括号粘合在一起<br>
  >将捕获列表、参数列表、返回类型和左括号放在第一行，将函数主体缩进以下几行，将右括号放在新行<br>
  >将封闭函数调用的右括号和分号放在与 lambda 的右大括号相同的行上<br>
  >如果您在 'if' 语句中使用 lambda，请在新行开始 lambda，以避免 lambda 的左大括号和 'if' 语句的左大括号之间产生混淆<br>
  >如果合适，将 lambda 完全放在一行上 <br>
  


