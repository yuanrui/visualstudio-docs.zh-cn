---
title: 修改在 DSL 中的标准菜单命令
ms.date: 11/04/2016
ms.topic: conceptual
helpviewer_keywords:
- .vsct files, adding commands to a domain-specific language
- Domain-Specific Language, adding custom commands
author: gewarren
ms.author: gewarren
manager: jillfra
ms.workload:
- multiple
ms.openlocfilehash: 120696fe335245775c6ea7188efc059ae9e71342
ms.sourcegitcommit: 117ece52507e86c957a5fd4f28d48a0057e1f581
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2019
ms.locfileid: "66263689"
---
# <a name="how-to-modify-a-standard-menu-command-in-a-domain-specific-language"></a>如何：使用域特定语言修改标准的菜单命令

可修改某些在 DSL 中自动定义的标准命令的行为。 例如，可以修改**剪切**以便排除敏感信息。 若要实现此目的，请重写命令集类中的方法。 这些类定义在 DslPackage 项目的 CommandSet.cs 文件中，并派生自 <xref:Microsoft.VisualStudio.Modeling.Shell.CommandSet>。

> [!NOTE]
> 如果你想要创建自己的菜单命令，请参阅[如何：将命令添加到快捷菜单](../modeling/how-to-add-a-command-to-the-shortcut-menu.md)。

## <a name="what-commands-can-you-modify"></a>可以修改哪些命令？

### <a name="to-discover-what-commands-you-can-modify"></a>发现可以修改的命令

1. 在中`DslPackage`项目中，打开`GeneratedCode\CommandSet.cs`。 此 C# 文件可在解决方案资源管理器作为的子公司`CommandSet.tt`。

2. 在此文件的名称结尾找到类"`CommandSet`"，例如`Language1CommandSet`和`Language1ClipboardCommandSet`。

3. 在每个命令集类中，键入“`override`”，后跟一个空格。 IntelliSense 将显示可重写方法的列表。 每个命令具有一对其名称以“`ProcessOnStatus`”和“`ProcessOnMenu`”开头的方法。

4. 记录哪些命令集类包含你想要修改的命令。

5. 关闭该文件，无需保存编辑。

    > [!NOTE]
    > 通常，不应编辑已生成的文件。 任何编辑都将在下次生成文件时丢失。

## <a name="extend-the-appropriate-command-set-class"></a>扩展相应的命令集类

创建包含命令集类的分部声明的新文件。

### <a name="to-extend-the-command-set-class"></a>扩展命令集类

1. 在“解决方案资源管理器”中，在 DslPackage 项目中打开 GeneratedCode 文件夹，然后在 CommandSet.tt 下进行查找并打开其生成的文件 CommandSet.cs。 注意在此处定义的第一个类的命名空间和名称。 例如，你可能看到：

     `namespace Company.Language1`

     `{ ...  internal partial class Language1CommandSet : ...`

2. 在中**DslPackage**，创建名为的文件夹**自定义代码**。 在此文件夹中创建名为的新类文件`CommandSet.cs`。

3. 在该新文件中，编写具有与生成的分部类相同的命名空间和名称的分部声明。 例如：

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel.Design;
    namespace Company.Language1 /* Make sure this is correct */
    { internal partial class Language1CommandSet { ...
    ```

     **请注意**如果类文件模板用于创建新文件，则必须更正的命名空间和类名。

## <a name="override-the-command-methods"></a>重写命令方法

大多数命令都具有两个关联的方法：具有名称的方法让`ProcessOnStatus`...确定命令是否应为可见和启用状态。 它将在每当用户右键单击关系图时调用，并应快速执行且不做任何更改。 `ProcessOnMenu`...当用户单击该命令，并应执行该命令的函数时调用。 你可能想要重写其中一个方法，或两者都进行重写。

### <a name="to-change-when-the-command-appears-on-a-menu"></a>更改命令何时显示在菜单上

重写 ProcessOnStatus...方法。 此方法应设置其参数 MenuCommand 的“可见”和“已启用”属性。 通常，命令查看 this.CurrentSelection 来确定命令是否应用到选定的元素，还可能查看其属性来确定命令是否可以应用到其当前状态中。

一般原则是，“可见”属性应由选定了哪些什么元素来确定。 “已启用”属性（确定命令在菜单上是显示为黑色还是灰色）应取决于选择的当前状态。

以下示例将在用户已选择多个形状时禁用“删除”菜单项。

> [!NOTE]
> 此方法不影响命令通过击键是否可用。 例如，禁用“删除”菜单项不会阻止通过 Delete 键来调用命令。

```csharp
/// <summary>
/// Called when user right-clicks on the diagram or clicks the Edit menu.
/// </summary>
/// <param name="command">Set Visible and Enabled properties.</param>
protected override void ProcessOnStatusDeleteCommand (MenuCommand command)
{
  // Default settings from the base method.
  base.ProcessOnStatusDeleteCommand(command);
  if (this.CurrentSelection.Count > 1)
  {
    // If user has selected more than one item, Delete is greyed out.
    command.Enabled = false;
  }
}
```

最佳做法是先调用基方法，以处理所有你无需考虑的用例和设置。

ProcessOnStatus 方法不应在“存储”中创建、删除或更新元素。

### <a name="to-change-the-behavior-of-the-command"></a>更改命令的行为

重写 ProcessOnMenu...方法。 以下示例将阻止用户一次删除多个元素，即使使用 Delete 键也是如此。

```csharp
/// <summary>
/// Called when user presses Delete key
/// or clicks the Delete command on a menu.
/// </summary>
protected override void ProcessOnMenuDeleteCommand()
{
  // Allow users to delete only one thing at a time.
  if (this.CurrentSelection.Count <= 1)
  {
    base.ProcessOnMenuDeleteCommand();
  }
}
```

如果你的代码将对“存储”进行更改（例如创建、删除或更新元素或链接），则必须在事务内进行这些更改。 有关详细信息，请参阅[如何创建和更新模型元素](../modeling/how-to-modify-a-standard-menu-command-in-a-domain-specific-language.md)。

### <a name="write-the-code-of-the-methods"></a>编写代码的方法

以下片段通常在这些方法内十分有用：

- `this.CurrentSelection`。 用户右键单击的形状始终包含在此形状和连接符列表中。 如果用户单击关系图的空白部分，则“关系图”是该列表中的唯一成员。

- `this.IsDiagramSelected()` - `true` 如果用户单击关系图的空白部分。

- `this.IsCurrentDiagramEmpty()`

- `this.IsSingleSelection()` - 用户未选择多个形状

- `this.SingleSelection` - 用户右键单击的形状或关系图

- `shape.ModelElement as MyLanguageElement` - 由形状表示的模型元素。

有关如何在元素之间导航以及如何创建对象和链接的详细信息，请参阅[导航和更新程序代码中的模型](../modeling/navigating-and-updating-a-model-in-program-code.md)。

## <a name="see-also"></a>请参阅

- <xref:System.ComponentModel.Design.MenuCommand>
- [编写代码以自定义域特定语言](../modeling/writing-code-to-customise-a-domain-specific-language.md)
- [如何：向快捷菜单添加命令](../modeling/how-to-add-a-command-to-the-shortcut-menu.md)
- [VSPackage 如何添加用户界面元素](../extensibility/internals/how-vspackages-add-user-interface-elements.md)
- [Visual Studio 命令表格 (.Vsct) 文件](../extensibility/internals/visual-studio-command-table-dot-vsct-files.md)
- [VSCT XML 架构参考](../extensibility/vsct-xml-schema-reference.md)
- [VMSDK-电路图示例。广泛的 DSL 自定义](https://code.msdn.microsoft.com/Visualization-Modeling-SDK-763778e8)
