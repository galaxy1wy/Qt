# Qt Creator 编码规则

* [English Version](./README.md)

关于查看官网的Qt Creator 编码规则文档，请点击 https://doc.qt.io/qtcreator-extending/coding-style.html 。

遵循代码构造、格式设计、模式和实践

# 代码构造

遵循代码构造指南，使代码更快、更清晰。此外，这些准则还允许您利用 C++ 中的强类型检查。

- 尽可能首选 `++XXX` 而不是 `XXX++`。`++XXX` 可能比 `XXX++` 快。这条规则也适用于递减。
  ```
    ++T;
    --U;
    
    -NOT-
    
    T++;
    U--;
  ```
  
- 尽量减少重复执行相同代码；这尤其适用于循环。
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

空白。
  - 使用四个空格进行缩进，不要使用制表符
  - 在合适的地方使用空行将语句分组在一起
  - 始终只使用一个空行

指针和引用。
  - 对于指针或引用，请始终在星号 （*） 或与号 （&） 之前使用单个空格，但切勿在之后使用。尽可能避免 C 样式强制转换。
    ```c++
      char *blockOfMemory = (char *)malloc(data.size());
      char *blockOfMemory = reinterpret_cast<char *>(malloc(data.size()));
      
      -NOT-
      
      char* blockOfMemory = (char* ) malloc(data.size());
    ```
  
  
