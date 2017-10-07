---
title: "aaaWhat toomy felhőszolgáltatás-projekt történt? | Microsoft Docs"
description: "Ismerteti, mi történik a felhőalapú szolgáltatások projektben szolgáltatások csatlakozás tooan Visual Studio használatával Azure storage-fiók összekötése után"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 65662dde45dd75bca1b57022283f76305f95e94a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-happened-toomy-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Milyen történt toomy felhőszolgáltatások projektre (csatlakozik a Visual Studio Azure Storage szolgáltatás)?
## <a name="references-added"></a>Hozzáadott
hello Azure Storage NuGet-csomagot a Visual Studio-projekt tooyour lett hozzáadva.  
Ez a csomag .NET hivatkozik a következő hello bővült:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Hozzáadott Azure Storage kapcsolati karakterlánca
Elemek hello kiválasztott tárfiók kapcsolati karakterláncot és a kulcs-ekkel hozta létre. Változtatás történt volna a következő fájlok toohello:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

