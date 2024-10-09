# Qt Creator 编码规则

* [English Version](./README.md)

关于查看官网的Qt Creator 编码规则文档，请点击 https://doc.qt.io/qtcreator-extending/coding-style.html 。

遵循代码构造、格式设计、模式和实践

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

