---
title: "aaaAzure tároló beállításjegyzék adattárak |} Microsoft Docs"
description: "Hogyan toouse Azure tároló beállításjegyzék tárolóhelyekkel Docker lemezképek"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: cristyg
ms.openlocfilehash: 06172a63465838a78a607f268da116d8158789ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-registry-repositories"></a>Azure-tárolót beállításjegyzék adattárak

Az Azure tároló nyilvántartó szolgáltatások és orchestrators számos kompatibilisek. toomake azt könnyebb tootrack hello forrás szolgáltatások és az ügynökök, amelyből ACR használatos, azt elindította hello Docker fejlécmező hello Docker.config fájl használatával.



## <a name="viewing-repositories-in-hello-portal"></a>Hello Portal adattárak megtekintése

hello ACR fejlécek hello formátumot követi:
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* Felhő: Azure, Azure verem, vagy más kormányzati vagy ország-specifikus Azure felhők. Azure verem és a kormányzati jelenleg nem támogatott, bár ez a paraméter lehetővé teszi a jövőbeli támogatási.
* Szolgáltatás: hello szolgáltatás neve.
* Optionalservicename: szolgáltatások subservices, vagy egy SKU toospecify nem kötelező paraméter (pl.: webalkalmazások megfeleljen az Azure-beli/app-szolgáltatás vagy-webalkalmazások).

Partner szolgáltatásai és orchestrators a javasolt toouse specifikus fejléccel értékek toohelp rendelkező a telemetriai adatok. Felhasználók módosíthatja átadott toohello fejléc, ha azok kívánják hello érték is.

hello szeretnénk ACR partnerek toouse toopopulate hello "X-Meta-forrás-ügyfél" mező értékei alatt:

| Szolgáltatás neve              | Fejléc                                |
| ------------------------- | ------------------------------------- |
| Azure Container Service   | Azure/compute/azure-tároló-szolgáltatás |
| Az App Service - webalkalmazásokban    | Azure-beli/app-szolgáltatás vagy-webalkalmazások            |
| Az App Service - Logic Apps alkalmazások  | Azure/app-service/logikai-alkalmazások          |
| Batch                     | Azure/compute/kötegelt                   |
| Felhő konzol             | Azure/felhő-konzol                   |
| Functions                 | Azure/compute/funkciók               |
| Az eszközök internetes hálózata - központ  | Azure-iot-központ                         |
| HDInsight                 | Azure/data/hdinsight                  |
| Jenkins                   | Azure/jenkins                         |
| Machine Learning          | Azure/data/machile-tanulás           |
| Service Fabric            | Azure/compute/service-háló          |
| VSTS                      | Azure/vsts                            |


## <a name="next-steps"></a>Következő lépések
[További információ a nyilvántartó és hello támogatott szolgáltatások orchestrators](container-registry-intro.md)
