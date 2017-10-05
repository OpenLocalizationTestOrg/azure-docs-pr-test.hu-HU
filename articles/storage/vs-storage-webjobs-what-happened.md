---
title: "Mi történt a webjobs-feladat project (a Visual Studio Azure Storage szolgáltatás csatlakozik)? | Microsoft Docs"
description: "Ismerteti, mi történt Azure webjobs-feladat projektben services csatlakozik a Visual Studio használatával storage-fiók összekötése után"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 3b28ddeadc87937941d60b16fae817e59a220b22
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a><span data-ttu-id="2dbdb-104">Mi történt a webjobs-feladat project (a Visual Studio Azure Storage szolgáltatás csatlakozik)?</span><span class="sxs-lookup"><span data-stu-id="2dbdb-104">What happened to my WebJob project (Visual Studio Azure Storage connected service)?</span></span>
## <a name="references-added"></a><span data-ttu-id="2dbdb-105">Hozzáadott</span><span class="sxs-lookup"><span data-stu-id="2dbdb-105">References Added</span></span>
<span data-ttu-id="2dbdb-106">Az Azure Storage NuGet-csomag hozzáadni vagy frissíteni a Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="2dbdb-106">The Azure Storage NuGet package was added to or updated in your Visual Studio project.</span></span>  
<span data-ttu-id="2dbdb-107">Ez a csomag a következő .NET hivatkozásokat ad:</span><span class="sxs-lookup"><span data-stu-id="2dbdb-107">This package adds the following .NET references:</span></span>

* <span data-ttu-id="2dbdb-108">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="2dbdb-108">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="2dbdb-109">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="2dbdb-109">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="2dbdb-110">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="2dbdb-110">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="2dbdb-111">**Microsoft.WindowsAzure.ConfigurationManager**</span><span class="sxs-lookup"><span data-stu-id="2dbdb-111">**Microsoft.WindowsAzure.ConfigurationManager**</span></span>
* <span data-ttu-id="2dbdb-112">**Microsoft.WindowsAzure.Storage**</span><span class="sxs-lookup"><span data-stu-id="2dbdb-112">**Microsoft.WindowsAzure.Storage**</span></span>
* <span data-ttu-id="2dbdb-113">**Newtonsoft.Json**</span><span class="sxs-lookup"><span data-stu-id="2dbdb-113">**Newtonsoft.Json**</span></span>
* <span data-ttu-id="2dbdb-114">**System.Data**</span><span class="sxs-lookup"><span data-stu-id="2dbdb-114">**System.Data**</span></span>
* <span data-ttu-id="2dbdb-115">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="2dbdb-115">**System.Spatial**</span></span>

## <a name="connection-string-for-azure-storage-added"></a><span data-ttu-id="2dbdb-116">Hozzáadott Azure Storage kapcsolati karakterlánca</span><span class="sxs-lookup"><span data-stu-id="2dbdb-116">Connection string for Azure Storage added</span></span>
<span data-ttu-id="2dbdb-117">A projekt App.config fájlban a **AzureWebJobsStorage** és **AzureWebJobsDashboard** bejegyzések frissültek a kiválasztott tárfiók kapcsolati karakterlánc és kulccsal.</span><span class="sxs-lookup"><span data-stu-id="2dbdb-117">In the App.config file of your project, the **AzureWebJobsStorage** and **AzureWebJobsDashboard** entries were updated with the selected storage account's connection string and key.</span></span>

<span data-ttu-id="2dbdb-118">További információkért lásd: [Azure WebJobs-dokumentáció erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="2dbdb-118">For more information, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

