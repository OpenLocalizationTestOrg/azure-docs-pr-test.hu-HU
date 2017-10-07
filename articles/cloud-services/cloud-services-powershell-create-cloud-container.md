---
title: "a PowerShell használatával a felhőalapú szolgáltatás tárolójának aaaCreate |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan toocreate egy felhőalapú szolgáltatás PowerShell-tárolóban. hello tároló webes és feldolgozói szerepköröket futtat."
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: 
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: 4c47b10b5ba1f9cc39594a45cd869bf04fcaadf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-an-azure-powershell-command-toocreate-an-empty-cloud-service-container"></a>Az Azure PowerShell-parancs toocreate egy üres felhőalapú szolgáltatás tárolójának használatára
Ez a cikk azt ismerteti, hogyan tooquickly Azure PowerShell-parancsmagok használatával Felhőszolgáltatások tárolókat hozhat létre. Kérjük, kövesse az alábbi hello lépéseket:

1. Telepítse a Microsoft Azure PowerShell-parancsmag hello a hello [Azure PowerShell letölti](http://aka.ms/webpi-azps) lap.
2. Nyissa meg a hello PowerShell-parancssort.
3. Használjon hello [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) a toosign.

   > [!NOTE]
   > További információk az hello Azure PowerShell-parancsmag telepítése és csatlakozás az Azure-előfizetés tooyour, tekintse meg túl[hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
   >
   >
4. Használjon hello **New-AzureService** parancsmag toocreate egy üres Azure felhőalapú szolgáltatás tárolójának.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Hajtsa végre a tooinvoke hello példaparancsmagot:

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Hello Azure-felhőszolgáltatásban létrehozásával kapcsolatos további információkért futtassa:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Következő lépések
* toomanage hello felhőalapú szolgáltatás telepítését, tekintse meg a toohello [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), és [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) parancsok. Túl is mutathat[hogyan tooconfigure felhőalapú szolgáltatások](cloud-services-how-to-configure.md) további tájékoztatást talál.
* toopublish a felhőalapú szolgáltatás tooAzure projektre, tekintse meg a toohello **PublishCloudService.ps1** a kódminta [folyamatos kézbesítését az Azure-ban a felhőszolgáltatás](cloud-services-dotnet-continuous-delivery.md).
