---
author: dotpaul
ms.author: paulming
ms.date: 04/05/2019
ms.topic: include
ms.openlocfilehash: 054198eff46c0983a5610b29dee5e29e5ac67a70
ms.sourcegitcommit: b468d71052a1b8a697f477ab23a3644de139f1e9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2019
ms.locfileid: "67254181"
---
反序列化不受信任的数据时，不安全反序列化程序是易受攻击。 攻击者可以修改以包括意外的类型具有恶意副作用注入对象的序列化的数据。 针对不安全的反序列化程序的攻击可能，例如，基础操作系统上执行命令、 通过网络进行通信或删除文件。