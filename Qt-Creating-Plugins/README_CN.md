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
  单击“确定”后，Qt Creator 将启动，可以通过查找菜单项 Tools > Example 并在 Help > About Plugins 对话框中查找插件来验证插件是否已成功加载。

# 文件结构

  插件向导会创建一组插件需要或应该具有的基本文件。我们将在以下部分中详细介绍其中的一些，这是一个简短的概述：  

  |          File文件          |          Role角色          |
  |:---------------------------|:---------------------------|
  |README.md                    |描述如何构建和运行插件      |
  |Example.json.in              |插件元数据模板。CMake 从此文件创建一个 Example.json，该文件作为元数据编译到插件中。Qt Creator 读取元数据以了解有关插件的信息|
  |CMakeLists.txt                |Project 文件，CMake 使用它来生成构建文件并构建插件|
  |example_global.h              |包含宏定义，当此插件应将元件导出到其他插件时，这些定义非常有用|
  |exampleconstants.h            |定义插件代码使用的常量的 Headers|
  |example.h, example.cpp        |C++ 头文件和源文件，用于定义插件类，该插件类将由 Qt Creator 的插件管理器实例化和运行|
  |build_cmake.yml|添加 GitHub 操作和工作流程，在Windows、Linux和macOS上将提交推送到 GitHub 时，该工作流程都会构建插件。更多信息，请参阅.github\workflow\README.md|


# CMake 项目

  CMake 项目文件CMakeLists.txt定义插件的编译方式。Qt Creator 插件除了告诉 CMake 哪些文件需要编译（或由 moc 或 uic 处理）之外，还需要在那里进行特定的设置。详细看看项目向导生成了什么  
  ```cmake
    # Remove when sharing with others.
    list(APPEND CMAKE_PREFIX_PATH "/Users/example/qt-creator/build")
  ```
  list（APPEND ...） 调用告诉 CMake 在其依赖项的搜索路径中包含您在向导中指定的 Qt Creator 构建路径。由于它包含本地计算机上的绝对路径，因此在与他人共享项目时应删除此行。  
  如果没有这一行，在使用 CMake 配置插件时，您需要显式添加 Qt Creator 构建的路径以CMAKE_PREFIX_PATH  

  ```cmake
    project(Example)

    set(CMAKE_AUTOMOC ON)
    set(CMAKE_AUTORCC ON)
    set(CMAKE_AUTOUIC ON)
    set(CMAKE_CXX_STANDARD 17)
  ```
  本节对 Qt/CMake 项目进行一些标准设置。除了设置项目名称和要使用的 C++ 标准外，它还会自动检测需要通过 moc、rcc 或 uic 运行的文件  

  ```cmake
    find_package(QtCreator COMPONENTS Core REQUIRED)
    find_package(QT NAMES Qt6 Qt5 COMPONENTS Widgets REQUIRED)
    set(QtX Qt${QT_VERSION_MAJOR})
  ```
  本节告诉 CMake 找到 Qt Creator 和 Qt。如果你的插件需要额外的 Qt 模块，你需要将它们添加到本节中对应的 find_package 调用中  
  要查找 Qt Creator 和 Qt，当您使用 CMake 配置插件时，CMAKE_PREFIX_PATH中必须存在 Qt Creator 和 Qt 安装的路径
  
  ```cmake
    add_qtc_plugin(Example
      PLUGIN_DEPENDS
        QtCreator::Core
      DEPENDS
        ${QtX}::Widgets
        QtCreator::ExtensionSystem
        QtCreator::Utils
      SOURCES
        .github/workflows/build_cmake.yml
        .github/workflows/README.md
        README.md
        example.cpp
        example.h
        example_global.h
        exampleconstants.h
        examplefunctions.h
    )
  ```
  add_qtc_plugin 调用会为您的插件创建一个具有指定名称的目标  
  在 PLUGIN_DEPENDS 子部分中，您需要指定您的插件所依赖的 Qt Creator 插件。有效值是前缀为 QtCreator::的插件名称  
  在 DEPENDS 子部分中，您需要指定插件所依赖的库。使用前缀为 $\{QtX\}:: 的 Qt 模块名称链接到其他 Qt 模块。要链接到其他 Qt Creator 库，请为其名称添加 QtCreator::前缀。在此小节中，您还可以指定插件所依赖的其他库  
  在 SOURCES 子部分中，指定属于您的插件项目的所有文件。CMake 会自动将这些分类为源文件和头文件。本节中的其他文件被 CMake 忽略，但会出现在 Qt Creator 等 IDE 中显示的项目树中，以便于访问  

# 插件元数据模板

  .json 文件是一个 JSON 文件，其中包含插件管理器在实际加载插件的库文件之前查找插件并解决其依赖项所需的信息。在这里只简单介绍一下。有关更多信息，请参阅 [插件元数据](https://doc.qt.io/qtcreator-extending/plugin-meta-data.html)。  
  该向导实际上并不直接创建 .json 文件，而是创建一个 .json.in 文件。qmake 使用它来生成实际的插件.json元数据文件，将 QTCREATOR_VERSION 等变量替换为它们的实际值。因此，需要转义 .json.in 文件中的所有反斜杠和引号（即，需要编写 \ 来获得反斜杠，编写 \“ 来获取生成的插件 JSON 元数据中的引号）  
  
  ```json
    "Name" : "Example",
    "Version" : "0.0.1",
    "CompatVersion" : "0.0.1",
  ```
  向导创建的元数据中的第一项定义插件的名称、版本以及当前版本与此插件的哪个版本二进制兼容
  
  ```json
    "Vendor" : "MyCompany",
    "Copyright" : "(C) MyCompany",
    "License" : "Put short license information here",
    "Description" : "Put a short description of your plugin here.",
    "Url" : "https://www.mycompany.com",
  ```
  之后，您将找到有关您在项目向导中提供的插件的信息

  ```json
    ${IDE_PLUGIN_DEPENDENCIES}
  ```
  IDE_PLUGIN_DEPENDENCIES 变量会自动替换为插件的 .pro 文件中QTC_PLUGIN_RECOMMENDS QTC_PLUGIN_DEPENDS 中的依赖项信息

# 插件类

  文件 example.h 和 example.cpp 定义了的小插件的插件实现。将在此处重点介绍一些亮点，并提供指向各个部分的更多详细信息的指针  

## Header File 头文件
  头文件 example.h 定义插件类的接口。
  
  ```c
    namespace Example {
    namespace Internal {
  ```
  该插件在 Example：：Internal 命名空间中定义，这符合 Qt Creator 源代码中[命名空间](https://doc.qt.io/qtcreator-extending/coding-style.html#coding-rules-namespacing)的编码规则。

  ```cpp
    class ExamplePlugin : public ExtensionSystem::IPlugin
    {
        Q_OBJECT
        Q_PLUGIN_METADATA(IID "org.qt-project.Qt.QtCreatorPlugin" FILE "Example.json")
  ```
  所有Qt Creator插件都必须从[ExtensionSystem::IPlugin](https://doc.qt.io/qtcreator-extending/extensionsystem-iplugin.html)派生，并且是QObjects。Q_PLUGIN_METADATA 宏 是创建有效 Qt 插件所必需的。宏中给出的 IID 必须是 org.qt-project.Qt.QtCreatorPlugin ，才能将其标识为 Qt Creator 插件，并且 FILE 必须指向插件的元数据文件，如[插件元数据](https://doc.qt.io/qtcreator-extending/plugin-meta-data.html)中所述。

  ```cpp
    bool initialize(const QStringList &arguments, QString *errorString);
    void extensionsInitialized();
    ShutdownFlag aboutToShutdown();
  ```
  基类定义了在[插件的生命周期](https://doc.qt.io/qtcreator-extending/plugin-lifecycle.html)中调用的基本函数，这些函数在这里为您的新插件实现。这些功能及其作用在 插件生命周期 中有详细说明。

  ```cpp
    private:
        void triggerAction();
  ```
  该插件有一个额外的自定义插槽，用于在用户选择此插件添加的菜单项时弹出一个对话框。

## Source File 源文件
  源文件包含插件的实际实现，该插件注册新的菜单和菜单项，并在触发该项时打开一个消息框。

  来自插件代码本身、Core 插件和 Qt 的所有必要的头文件都包含在文件的开头。菜单和菜单项的设置是在插件的 initialize 函数中完成的，该函数是插件构造函数之后调用的第一件事。在该函数中，插件可以确保它所依赖的插件的基本设置已经完成，例如，Core 插件的 ActionManager 实例已经创建。

  有关实现插件接口的更多信息，请参阅 ExtensionSystem::IPlugin API 文档和插件生命周期。
  ```cpp
    auto action = new QAction(tr("Example Action"), this);
    Core::Command *cmd = Core::ActionManager::registerAction(action, Constants::ACTION_ID,
                                                             Core::Context(Core::Constants::C_GLOBAL));
    cmd->setDefaultKeySequence(QKeySequence(tr("Ctrl+Alt+Meta+A")));
    connect(action, &QAction::triggered, this, &ExamplePlugin::triggerAction);
  ```
  这部分代码创建一个新的 QAction，在操作管理器中将其注册为新 Command，并将其连接到插件的插槽。action manager 提供了一个中心位置，用户可以在其中分配和更改键盘快捷键，并管理例如在不同情况下应将菜单项定向到不同插件的情况，以及其他一些事情。

  ```cpp
    Core::ActionContainer *menu = Core::ActionManager::createMenu(Constants::MENU_ID);
    menu->menu()->setTitle(tr("Example"));
    menu->addAction(cmd);
    Core::ActionManager::actionContainer(Core::Constants::M_TOOLS)->addMenu(menu);
  ```
  在这里，将创建一个新的菜单项，将创建的命令添加到其中，并将菜单添加到菜单栏的 Tools 菜单中。

  ```cpp
    void ExamplePlugin::triggerAction()
    {
        QMessageBox::information(Core::ICore::mainWindow(),
                                 tr("Action Triggered"),
                                 tr("This is an action from Example."));
    }
  ```
  此部分定义触发菜单项时调用的代码。它使用 Qt API 打开一个消息框，该消息框显示信息性文本和 OK 按钮
   
