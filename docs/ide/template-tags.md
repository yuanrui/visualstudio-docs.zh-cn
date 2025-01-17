---
title: 在项目模板上添加或编辑标签
ms.date: 04/30/2019
author: minsa110
ms.author: somin
manager: jillfra
ms.topic: reference
helpviewer_keywords:
- item templates, updating
- Visual Studio templates, updating
- project templates, updating
- updating templates [Visual Studio]
- template tagging, updating
- template tags, updating
ms.openlocfilehash: 4a5113fa7f420d58892e2737ec9196422486490e
ms.sourcegitcommit: cd21b38eefdea2cdefb53e68e7a30b868e78dd6b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66038623"
---
# <a name="add-tags-to-project-templates"></a>向项目模板添加标签

自 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 版本 16.1 预览版 2 起，可以向项目模板添加语言、平台和项目类型标签。 新建项目对话框中的两个位置使用了标签：

- 模板描述的下方将显示标签

   ![新建项目对话框中带标签的项目模板](media/npd-item-with-template-tags.png)

- 可以借助标签搜索和筛选模板

   ![在新建项目对话框中搜索和筛选](media/npd-search-and-filter.png)

可以通过使用 Visual Studio 内置模板标签更新 .vstemplate XML 文件，或通过创建自定义模板标签，来添加标签。 仅 Visual Studio 2019 新建项目对话框显示模板标签。 它们不会影响 Visual Studio 早期版本的模板呈现。

## <a name="add-or-edit-tags"></a>添加或编辑标签

在以下情况下，需要在项目模板的 .vstemplate XML 中添加或编辑标签：

* 使用“导出模板”向导[新建项目模板](/visualstudio/ide/how-to-create-project-templates)

* [更新现有项目模板](/visualstudio/ide/how-to-update-existing-templates)

* [新建 VSIX 项目模板](/visualstudio/extensibility/getting-started-with-the-vsix-project-template)

## <a name="syntax"></a>语法

```xml
<LanguageTag> Language Name </LanguageTag>
<PlatformTag> Platform Name </PlatformTag>
<ProjectTypeTag> Project Type </ProjectTypeTag>
```

## <a name="attributes"></a>特性

下面的属性为可选属性，它们适用于高级用户场景。

|特性|说明|
|---------------|-----------------|
|`Package`|指定 Visual Studio 包 ID 的 GUID。|
|`ID`|指定 Visual Studio 资源 ID。|

语法：

```xml
<LanguageTag Package="{PackageID}" ID="ResourceID" />
<PlatformTag Package="{PackageID}" ID="ResourceID" />
<ProjectTypeTag Package="{PackageID}" ID="ResourceID" />
```

## <a name="elements"></a>元素

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

|元素|说明|
|-------------|-----------------|
|[TemplateData](../extensibility/templatedata-element-visual-studio-templates.md)|（必需）将此模板分类并定义此模板在“新建项目”或“添加新项”对话框中的显示方式。|

## <a name="text-value"></a>文本值

若未使用 `Package` 和 `ID` 属性，则必须提供文本值。

该文本提供模板的名称。

## <a name="built-in-tags"></a>内置标签

Visual Studio 提供了内置标签列表，添加这些标签时它们将呈现已本地化的资源。 下面是内置标签列表及其对应的值（位于括号中）。

| 语言 | Platform | 项目类型 |
| -- | -- | -- |
| C++ (`cpp`) | Android (`android`) | 云 (`cloud`) |
| C# (`csharp`) | Azure (`azure`) | 控制台 (`console`) |
| F# (`fsharp`) | iOS (`ios`) | 桌面 (`desktop`) |
| Java (`java`) | Linux (`linux`) | 扩展 (`extension`) |
| JavaScript (`javascript`) | macOS (`macos`) | 游戏 (`games`) |
| Python (`python`) | tvOS (`tvos`) | IoT (`iot`) |
| 查询语言 (`querylanguage`) | Windows (`windows`) | 库 (`library`) |
| TypeScript (`typescript`) | Xbox (`xbox`) | 机器学习 (`machinelearning`) |
| Visual Basic (`visualbasic`) | | 移动 (`mobile`) |
| | | Office (`office`) |
| | | 其他 (`other`) |
| | | 服务 (`service`) |
| | | 测试 (`test`) |
| | | UWP (`uwp`) |
| | | Web (`web`) |

## <a name="example"></a>示例

下面的示例说明了 Visual C# 应用程序的项目模板的元数据。

```xml
<VSTemplate Type="Project" Version="3.0.0"
    xmlns="http://schemas.microsoft.com/developer/vstemplate/2005">
    <TemplateData>
        <Name>My template</Name>
        <Description>A basic template</Description>
        <Icon>TemplateIcon.ico</Icon>
        <ProjectType>CSharp</ProjectType>
        <LanguageTag>C#</LanguageTag>
        <PlatformTag>windows</PlatformTag>
        <PlatformTag>linux</PlatformTag>
        <PlatformTag>My Platform</PlatformTag>
        <ProjectTypeTag>console</ProjectTypeTag>
        <ProjectTypeTag>desktop</ProjectTypeTag>
    </TemplateData>
    <TemplateContent>
        <Project File="MyTemplate.csproj">
            <ProjectItem>Form1.cs<ProjectItem>
            <ProjectItem>Form1.Designer.cs</ProjectItem>
            <ProjectItem>Program.cs</ProjectItem>
            <ProjectItem>Properties\AssemblyInfo.cs</ProjectItem>
            <ProjectItem>Properties\Resources.resx</ProjectItem>
            <ProjectItem>Properties\Resources.Designer.cs</ProjectItem>
            <ProjectItem>Properties\Settings.settings</ProjectItem>
            <ProjectItem>Properties\Settings.Designer.cs</ProjectItem>
        </Project>
    </TemplateContent>
</VSTemplate>
```

## <a name="see-also"></a>请参阅

- [Visual Studio 模板架构参考](/visualstudio/extensibility/visual-studio-template-schema-reference)
- [创建项目和项模板](/visualstudio/ide/creating-project-and-item-templates)
- [自定义项目和项模板](/visualstudio/ide/customizing-project-and-item-templates)
- [VSIX 项目模板入门](/visualstudio/extensibility/getting-started-with-the-vsix-project-template)