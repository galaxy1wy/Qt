# Qt创建插件

  [官网文档地址](https://doc.qt.io/qtcreator-extending/creating-plugins.html)

  Qt Creator 的核心是插件加载器，它可以加载和运行一组插件，然后这些插件实际上会提供从 Qt Creator IDE 中了解的功能。因此，即使是主应用程序窗口和菜单也都是由插件提供的。插件可以使用不同的方式为其他插件提供对其功能的访问权限，并允许它们扩展应用程序的某些方面。例如：
  - Core 插件是 Qt Creator 运行所必需的非常基本的插件，它提供主窗口本身，以及用于添加菜单项、模式、编辑器类型、导航面板和许多其他内容的 API。
  - TextEditor 插件为不同的文本编辑器提供了一个框架和基本实现，具有高亮、完成和折叠功能，然后其他插件可以使用该插件向 Qt Creator 添加更专业的文本编辑器类型，例如编辑 C/C++ 或 .pro 文件。

  了解基本插件由什么组成，如何编写插件规范文件，插件的生命周期是什么，扩展现有插件功能和为其他插件提供接口的一般原则是什么，能够编写第一个插件。<br>

 # 创建插件项目操作步骤

   **注意：** 此操作基于Qt Creator IDE。

  1.选择 File > New Project > Library > Qt Creator Plugin >Choose  
  ![选择](https://doc.qt.io/qtcreator-extending/images/firstplugin-wizard.png)  

  此时将打开 Introduction and Project Location 对话框  
  ![打开对话框](https://doc.qt.io/qtcreator-extending/images/firstplugin-nameandpath.png)  

  2.为项目命名并指定将在哪个路径中创建此项目。实际插件的名称可能与项目名称不同  

  3.继续转到 Plugin Information 对话框  
  ![转到 Plugin Information 对话框](https://doc.qt.io/qtcreator-extending/images/firstplugin-pluginsetup.png)  

  4.在 Plugin name 字段中，键入 Example。插件的名称用作其标识符，也是代码中文件名和类的基础  

  5.以下字段的值主要是信息性的，显示在 Qt Creator 插件概述（帮助>关于插件，或 Qt Creator >关于 macOS 上的插件）的详细视图中
    
    - Vendor name 供应商名称 是创建插件的公司或组织的一个简短的单字名称。这也用于插件将部署到的路径名。
    - Copyright 版权 是一个单行的简短版权字符串
    - License 是许可证文本
    - Description 是插件功能的简短描述。
    - URL 是一个网站，用户可以在其中找到有关插件和/或提供插件的组织的更多信息。

  6.将 Qt Creator build 字段设置为要用于测试插件的 Qt Creator 实例的 build 目录。如果你没有正确地做到这一点，你的插件就会出现编译错误，你的插件可能根本不会出现在 Qt Creator 中  

  7.继续 Translation File 对话框  
  ![继续 Translation File 对话框](https://doc.qt.io/qtcreator-extending/images/firstplugin-translation-file.png)  

  8.选择要将插件本地化为的语言。这将为所选语言设置翻译支持  

  9.继续 Kit Selection 对话框  
  ![继续 Kit Selection 对话框](https://doc.qt.io/qtcreator-extending/images/firstplugin-kitselection.png)  

  10.选择要用于构建和运行项目的工具包。对于 Qt Creator 插件，这需要是桌面设备类型的工具包，以及与构建 Qt Creator 时使用的 Qt 版本兼容的 Qt 版本（在最好的情况下是完全相同的版本）。如果您使用不兼容的 Qt 版本来构建插件，则在 Qt Creator 尝试加载插件时会出现错误。  

  11.继续 Project Management 对话框  
  ![继续 Project Management 对话框](https://doc.qt.io/qtcreator-extending/images/firstplugin-summary.png)  

  12.查看将要创建的文件，选择 Qt Creator 应该用于的项目的版本控制系统，然后完成  

# 构建和运行插件

  如果在项目向导中传递了正确的 Qt Creator 构建路径，则您的插件在按下构建按钮时应该可以正常构建。在运行项目之前，选择Build & Run > Run以指定运行设置：  
  ![](https://doc.qt.io/qtcreator-extending/images/firstplugin-runsettings.png)  
  在项目向导的 Qt Creator 构建字段中指定的构建中选择 Qt Creator 可执行文件的路径，并将 Command line arguments 字段的值设置为 -pluginpath %{buildDir}  
  单击“确定”后，Qt Creator 将启动，可以通过查找菜单项 Tools > Example 并在 Help > About Plugins 对话框中查找插件来验证插件是否已成功加载
  
  
  

   
