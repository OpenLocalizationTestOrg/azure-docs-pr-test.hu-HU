---
title: "Mi történt az ASP.NET 5 project (Visual Studio kapcsolódó szolgáltatások) |} Microsoft Docs"
description: "Ismerteti, mi történik az services csatlakozik a Visual Studio ASP.NET 5 projektben Visual Studio használatával Azure storage-fiók összekötése után?"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 4390993772eaf35516e48ad7adcdcec5f1df8d71
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Mi történt az ASP.NET 5 project (a Visual Studio Azure Storage szolgáltatások csatlakozik)?
## <a name="references-added"></a>Hozzáadott
Az Azure Storage NuGet-csomagot a Visual Studio-projekt lett adva.  
Ez a csomag a következő .NET hivatkozásokat ad:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

Emellett a NuGet-csomag **Microsoft.Framework.Configuration.Json** hozzá lett adva.

## <a name="connection-string-for-azure-storage-added"></a>Hozzáadott Azure Storage kapcsolati karakterlánca
A projekt config.json fájlban a kiválasztott tárfiók kapcsolati karakterláncot és a kulcs elem hozták létre.

További információkért lásd: [ASP.NET 5](http://www.asp.net/vnext).

