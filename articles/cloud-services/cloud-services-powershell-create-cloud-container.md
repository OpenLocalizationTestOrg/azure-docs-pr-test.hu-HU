---
title: "Egy felhőalapú szolgáltatás tárolójának létrehozása a PowerShell használatával |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan egy felhőalapú szolgáltatás tárolójának létrehozása a PowerShell használatával. A tároló webes és feldolgozói szerepköröket futtat."
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
ms.openlocfilehash: 2023fa7b318f9f76ce1e1ea0a46110297be9a001
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a><span data-ttu-id="17536-104">Az Azure PowerShell-parancs segítségével hozzon létre egy üres felhőalapú szolgáltatás tárolójának</span><span class="sxs-lookup"><span data-stu-id="17536-104">Use an Azure PowerShell command to create an empty cloud service container</span></span>
<span data-ttu-id="17536-105">Ez a cikk azt ismerteti, hogyan hozhat létre gyorsan egy Azure PowerShell-parancsmagok használatával Felhőszolgáltatások tároló.</span><span class="sxs-lookup"><span data-stu-id="17536-105">This article explains how to quickly create a Cloud Services container using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="17536-106">Kérjük, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="17536-106">Please follow the steps below:</span></span>

1. <span data-ttu-id="17536-107">Telepítse a Microsoft Azure PowerShell parancsmag a [Azure PowerShell letölti](http://aka.ms/webpi-azps) lap.</span><span class="sxs-lookup"><span data-stu-id="17536-107">Install the Microsoft Azure PowerShell cmdlet from the [Azure PowerShell downloads](http://aka.ms/webpi-azps) page.</span></span>
2. <span data-ttu-id="17536-108">Nyissa meg a PowerShell-parancssort.</span><span class="sxs-lookup"><span data-stu-id="17536-108">Open the PowerShell command prompt.</span></span>
3. <span data-ttu-id="17536-109">Használja a [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) való bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="17536-109">Use the [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) to sign in.</span></span>

   > [!NOTE]
   > <span data-ttu-id="17536-110">További információk az telepítése az Azure PowerShell-parancsmag és az Azure-előfizetéssel való kapcsolódás, lásd [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="17536-110">For further instruction on installing the Azure PowerShell cmdlet and connecting to your Azure subscription, refer to [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
   >
   >
4. <span data-ttu-id="17536-111">Használja a **New-AzureService** parancsmaggal hozzon létre egy üres Azure felhőalapú szolgáltatás tárolójának.</span><span class="sxs-lookup"><span data-stu-id="17536-111">Use the **New-AzureService** cmdlet to create an empty Azure cloud service container.</span></span>

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. <span data-ttu-id="17536-112">Kövesse az ebben a példában a parancsmag meghívásához:</span><span class="sxs-lookup"><span data-stu-id="17536-112">Follow this example to invoke the cmdlet:</span></span>

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

<span data-ttu-id="17536-113">Az Azure felhőszolgáltatást létrehozásával kapcsolatos további információkért futtassa:</span><span class="sxs-lookup"><span data-stu-id="17536-113">For more information about creating the Azure cloud service, run:</span></span>

```
Get-help New-AzureService
```

### <a name="next-steps"></a><span data-ttu-id="17536-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="17536-114">Next steps</span></span>
* <span data-ttu-id="17536-115">A felhőalapú szolgáltatás telepítés kezeléséhez, tekintse meg a [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), és [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) parancsok.</span><span class="sxs-lookup"><span data-stu-id="17536-115">To manage the cloud service deployment, refer to the [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), and [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) commands.</span></span> <span data-ttu-id="17536-116">Akkor is hivatkozhat [konfigurálása felhőszolgáltatások](cloud-services-how-to-configure.md) további tájékoztatást talál.</span><span class="sxs-lookup"><span data-stu-id="17536-116">You may also refer to [How to configure cloud services](cloud-services-how-to-configure.md) for further information.</span></span>
* <span data-ttu-id="17536-117">A felhőszolgáltatás-projekt közzététele az Azure-ba, tekintse meg a **PublishCloudService.ps1** a kódminta [folyamatos kézbesítését az Azure-ban a felhőszolgáltatás](cloud-services-dotnet-continuous-delivery.md).</span><span class="sxs-lookup"><span data-stu-id="17536-117">To publish your cloud service project to Azure, refer to the  **PublishCloudService.ps1** code sample from [Continuous delivery for cloud service in Azure](cloud-services-dotnet-continuous-delivery.md).</span></span>
