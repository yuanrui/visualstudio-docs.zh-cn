---
title: TemplateGroupID 元素 （Visual Studio 模板） |Microsoft Docs
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.technology: vs-ide-general
ms.topic: reference
f1_keywords:
- http://schemas.microsoft.com/developer/vstemplate/2005#TemplateGroupID
helpviewer_keywords:
- TemplateGroupID element [Visual Studio Templates]
- <TemplateGroupID> element [Visual Studio Templates]
ms.assetid: bce7b49a-90bc-4691-aff3-a87e209f6d83
caps.latest.revision: 19
ms.author: gregvanl
manager: jillfra
ms.openlocfilehash: 53d1f6628ff9df48879a34417b7d89223d848dd8
ms.sourcegitcommit: 8b538eea125241e9d6d8b7297b72a66faa9a4a47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "58934202"
---
# <a name="templategroupid-element-visual-studio-templates"></a>TemplateGroupID 元素（Visual Studio 模板）
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

指定项模板将显示在哪种项目类型中。 当此元素很重要[ShowByDefault （Visual Studio 模板）](../extensibility/showbydefault-visual-studio-templates.md)设置为`false`。 当[ShowByDefault （Visual Studio 模板）](../extensibility/showbydefault-visual-studio-templates.md)设置为`true`，则项模板是在所有项目类型中可用。  
  
 \<VSTemplate>  
 \<TemplateData>  
 \<TemplateGroupID>  
  
## <a name="syntax"></a>语法  
  
```  
<TemplateGroupID> ... </TemplateGroupID>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  
 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
 无。  
  
### <a name="child-elements"></a>子元素  
 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[TemplateData](../extensibility/templatedata-element-visual-studio-templates.md)|将此模板分类并定义此模板在 **“新建项目”** 或 **“添加新项”** 对话框中的显示方式。|  
  
## <a name="text-value"></a>文本值  
 需要一个文本值。  
  
 此文本指定项模板类别的标识符。  
  
## <a name="remarks"></a>备注  
 `TemplateGroupID` 是一个元素。  
  
 值`TemplateGroupID`元素使用与项目系统注册 (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\VisualStudio\\ *\<版本号 >* \Projects\\)对显示在筛选器模板**添加新项**对话框。  
  
|Visual C++ 值|含义|  
|------------------------|-------------|  
|VC-Native|用于本机项目。 如果无法确定项目类型，也可以用于默认项目。|  
|VC-Managed|用于托管 (/clr) 项目|  
|VC-Windows|用于面向 Windows 平台（本机/托管/应用商店）的所有项目|  
|WinRT-Native-UAP|用于 Windows 10 应用商店项目|  
|CodeSharing-Native|用于共享项项目|  
|WinRT-Native-6.3|用于 Windows 8.1 应用商店项目|  
|WinRT-Native-Phone-6.3|用于 Windows Phone 8.1 项目|  
|WinRT-Native|用于 Windows 8.0 应用商店项目|  
|VC-Android|用于 Android 项目|  
  
## <a name="see-also"></a>请参阅  
 [Visual Studio 模板架构参考](../extensibility/visual-studio-template-schema-reference.md)   
 [创建项目和项模板](../ide/creating-project-and-item-templates.md)
