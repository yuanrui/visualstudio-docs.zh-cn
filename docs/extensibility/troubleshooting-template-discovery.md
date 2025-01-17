---
title: 在 Visual Studio 中的模板发现疑难解答 |Microsoft Docs
ms.date: 01/02/2018
ms.topic: conceptual
author: madskristensen
ms.author: madsk
manager: jillfra
ms.workload:
- vssdk
ms.openlocfilehash: b0b6af3e24d4e563ebbbcf0a233d85a1cd038cb3
ms.sourcegitcommit: 40d612240dc5bea418cd27fdacdf85ea177e2df3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66316368"
---
# <a name="troubleshooting-template-installation"></a>模板安装故障排除

如果遇到问题，部署项目或项模板，则可以启用诊断日志记录。

::: moniker range="vs-2017"

1. 创建 pkgdef 文件中的*Common7\IDE\CommonExtensions*为您的安装文件夹。 例如， *C:\Program Files (x86) \Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\EnablePkgDefLogging.pkgdef*。

::: moniker-end

::: moniker range=">=vs-2019"

1. 创建 pkgdef 文件中的*Common7\IDE\CommonExtensions*为您的安装文件夹。 例如， *C:\Program Files (x86) \Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\EnablePkgDefLogging.pkgdef*。

::: moniker-end

2. 以下代码添加到 pkgdef 文件：

    ```
    [$RootKey$\VsTemplate]
    "EnableTemplateDiscoveryLog"=dword:00000001
    ```

3. 打开[开发人员命令提示](/dotnet/framework/tools/developer-command-prompt-for-vs)进行安装和运行`devenv /updateConfiguration`。

::: moniker range="vs-2017"

4. 打开 Visual Studio，并启动的新项目和新项对话框来初始化两个模板树。

   模板日志现在显示在 **%LOCALAPPDATA%\Microsoft\VisualStudio\15.0_[instanceid]\VsTemplateDiagnosticsList.csv** （对应于您的 Visual Studio 实例的安装 ID 的实例 id）。 每个模板树初始化附加到此日志条目。

::: moniker-end

::: moniker range=">=vs-2019"

4. 打开 Visual Studio 和启动**创建一个新项目**并**新项**对话框初始化这两个模板树。

   模板日志现在显示在 **%LOCALAPPDATA%\Microsoft\VisualStudio\16.0_[instanceid]\VsTemplateDiagnosticsList.csv** （对应于您的 Visual Studio 实例的安装 ID 的实例 id）。 每个模板树初始化附加到此日志条目。

::: moniker-end

日志文件包含以下列：

- **FullPathToTemplate**，其中包含以下值：

    - 1 表示基于清单的部署

    - 0 为基于磁盘的部署的

- **TemplateFileName**

- 其他模板属性

> [!NOTE]
> 若要禁用日志记录，请删除该 pkgdef 文件，或更改的值`EnableTemplateDiscoveryLog`到`dword:00000000`，然后运行`devenv /updateConfiguration`试。

## <a name="see-also"></a>请参阅

- [创建自定义项目和项模板](creating-custom-project-and-item-templates.md)