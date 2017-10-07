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
# <a name="use-an-azure-powershell-command-toocreate-an-empty-cloud-service-container"></a><span data-ttu-id="8e490-104">Az Azure PowerShell-parancs toocreate egy üres felhőalapú szolgáltatás tárolójának használatára</span><span class="sxs-lookup"><span data-stu-id="8e490-104">Use an Azure PowerShell command toocreate an empty cloud service container</span></span>
<span data-ttu-id="8e490-105">Ez a cikk azt ismerteti, hogyan tooquickly Azure PowerShell-parancsmagok használatával Felhőszolgáltatások tárolókat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="8e490-105">This article explains how tooquickly create a Cloud Services container using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="8e490-106">Kérjük, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8e490-106">Please follow hello steps below:</span></span>

1. <span data-ttu-id="8e490-107">Telepítse a Microsoft Azure PowerShell-parancsmag hello a hello [Azure PowerShell letölti](http://aka.ms/webpi-azps) lap.</span><span class="sxs-lookup"><span data-stu-id="8e490-107">Install hello Microsoft Azure PowerShell cmdlet from hello [Azure PowerShell downloads](http://aka.ms/webpi-azps) page.</span></span>
2. <span data-ttu-id="8e490-108">Nyissa meg a hello PowerShell-parancssort.</span><span class="sxs-lookup"><span data-stu-id="8e490-108">Open hello PowerShell command prompt.</span></span>
3. <span data-ttu-id="8e490-109">Használjon hello [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) a toosign.</span><span class="sxs-lookup"><span data-stu-id="8e490-109">Use hello [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) toosign in.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8e490-110">További információk az hello Azure PowerShell-parancsmag telepítése és csatlakozás az Azure-előfizetés tooyour, tekintse meg túl[hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8e490-110">For further instruction on installing hello Azure PowerShell cmdlet and connecting tooyour Azure subscription, refer too[How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
   >
   >
4. <span data-ttu-id="8e490-111">Használjon hello **New-AzureService** parancsmag toocreate egy üres Azure felhőalapú szolgáltatás tárolójának.</span><span class="sxs-lookup"><span data-stu-id="8e490-111">Use hello **New-AzureService** cmdlet toocreate an empty Azure cloud service container.</span></span>

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. <span data-ttu-id="8e490-112">Hajtsa végre a tooinvoke hello példaparancsmagot:</span><span class="sxs-lookup"><span data-stu-id="8e490-112">Follow this example tooinvoke hello cmdlet:</span></span>

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

<span data-ttu-id="8e490-113">Hello Azure-felhőszolgáltatásban létrehozásával kapcsolatos további információkért futtassa:</span><span class="sxs-lookup"><span data-stu-id="8e490-113">For more information about creating hello Azure cloud service, run:</span></span>

```
Get-help New-AzureService
```

### <a name="next-steps"></a><span data-ttu-id="8e490-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8e490-114">Next steps</span></span>
* <span data-ttu-id="8e490-115">toomanage hello felhőalapú szolgáltatás telepítését, tekintse meg a toohello [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), és [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) parancsok.</span><span class="sxs-lookup"><span data-stu-id="8e490-115">toomanage hello cloud service deployment, refer toohello [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), and [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) commands.</span></span> <span data-ttu-id="8e490-116">Túl is mutathat[hogyan tooconfigure felhőalapú szolgáltatások](cloud-services-how-to-configure.md) további tájékoztatást talál.</span><span class="sxs-lookup"><span data-stu-id="8e490-116">You may also refer too[How tooconfigure cloud services](cloud-services-how-to-configure.md) for further information.</span></span>
* <span data-ttu-id="8e490-117">toopublish a felhőalapú szolgáltatás tooAzure projektre, tekintse meg a toohello **PublishCloudService.ps1** a kódminta [folyamatos kézbesítését az Azure-ban a felhőszolgáltatás](cloud-services-dotnet-continuous-delivery.md).</span><span class="sxs-lookup"><span data-stu-id="8e490-117">toopublish your cloud service project tooAzure, refer toohello  **PublishCloudService.ps1** code sample from [Continuous delivery for cloud service in Azure](cloud-services-dotnet-continuous-delivery.md).</span></span>
