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
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a><span data-ttu-id="64c2d-103">Mi történt az ASP.NET 5 project (a Visual Studio Azure Storage szolgáltatások csatlakozik)?</span><span class="sxs-lookup"><span data-stu-id="64c2d-103">What happened to my ASP.NET 5 project (Visual Studio Azure Storage connected services)?</span></span>
## <a name="references-added"></a><span data-ttu-id="64c2d-104">Hozzáadott</span><span class="sxs-lookup"><span data-stu-id="64c2d-104">References added</span></span>
<span data-ttu-id="64c2d-105">Az Azure Storage NuGet-csomagot a Visual Studio-projekt lett adva.</span><span class="sxs-lookup"><span data-stu-id="64c2d-105">The Azure Storage NuGet package was added to your Visual Studio project.</span></span>  
<span data-ttu-id="64c2d-106">Ez a csomag a következő .NET hivatkozásokat ad:</span><span class="sxs-lookup"><span data-stu-id="64c2d-106">This package adds the following .NET references:</span></span>

* <span data-ttu-id="64c2d-107">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="64c2d-107">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="64c2d-108">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="64c2d-108">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="64c2d-109">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="64c2d-109">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="64c2d-110">**Microsoft.WindowsAzure.Configuration**</span><span class="sxs-lookup"><span data-stu-id="64c2d-110">**Microsoft.WindowsAzure.Configuration**</span></span>
* <span data-ttu-id="64c2d-111">**Microsoft.WindowsAzure.Storage**</span><span class="sxs-lookup"><span data-stu-id="64c2d-111">**Microsoft.WindowsAzure.Storage**</span></span>
* <span data-ttu-id="64c2d-112">**Newtonsoft.Json**</span><span class="sxs-lookup"><span data-stu-id="64c2d-112">**Newtonsoft.Json**</span></span>
* <span data-ttu-id="64c2d-113">**System.Data**</span><span class="sxs-lookup"><span data-stu-id="64c2d-113">**System.Data**</span></span>
* <span data-ttu-id="64c2d-114">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="64c2d-114">**System.Spatial**</span></span>

<span data-ttu-id="64c2d-115">Emellett a NuGet-csomag **Microsoft.Framework.Configuration.Json** hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="64c2d-115">Also, the NuGet package **Microsoft.Framework.Configuration.Json** was added.</span></span>

## <a name="connection-string-for-azure-storage-added"></a><span data-ttu-id="64c2d-116">Hozzáadott Azure Storage kapcsolati karakterlánca</span><span class="sxs-lookup"><span data-stu-id="64c2d-116">Connection string for Azure Storage added</span></span>
<span data-ttu-id="64c2d-117">A projekt config.json fájlban a kiválasztott tárfiók kapcsolati karakterláncot és a kulcs elem hozták létre.</span><span class="sxs-lookup"><span data-stu-id="64c2d-117">In the config.json file of your project, an element was created with the selected storage account's connection string and key.</span></span>

<span data-ttu-id="64c2d-118">További információkért lásd: [ASP.NET 5](http://www.asp.net/vnext).</span><span class="sxs-lookup"><span data-stu-id="64c2d-118">For more information, see [ASP.NET 5](http://www.asp.net/vnext).</span></span>

