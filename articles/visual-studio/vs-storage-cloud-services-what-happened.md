---
title: "aaaWhat toomy felhőszolgáltatás-projekt történt? | Microsoft Docs"
description: "Ismerteti, mi történik a felhőalapú szolgáltatások projektben szolgáltatások csatlakozás tooan Visual Studio használatával Azure storage-fiók összekötése után"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 52e292558fb66b56e9d2e8e06ff489ae7a4bc845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-happened-toomy-cloud-services-project-visual-studio-azure-storage-connected-service"></a><span data-ttu-id="3e116-104">Milyen történt toomy felhőszolgáltatások projektre (csatlakozik a Visual Studio Azure Storage szolgáltatás)?</span><span class="sxs-lookup"><span data-stu-id="3e116-104">What happened toomy cloud services project (Visual Studio Azure Storage connected service)?</span></span>
## <a name="references-added"></a><span data-ttu-id="3e116-105">Hozzáadott</span><span class="sxs-lookup"><span data-stu-id="3e116-105">References added</span></span>
<span data-ttu-id="3e116-106">hello Azure Storage NuGet-csomagot a Visual Studio-projekt tooyour lett hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="3e116-106">hello Azure Storage NuGet package was added tooyour Visual Studio project.</span></span>  
<span data-ttu-id="3e116-107">Ez a csomag .NET hivatkozik a következő hello bővült:</span><span class="sxs-lookup"><span data-stu-id="3e116-107">This package adds hello following .NET references:</span></span>

* <span data-ttu-id="3e116-108">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="3e116-108">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="3e116-109">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="3e116-109">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="3e116-110">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="3e116-110">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="3e116-111">**Microsoft.WindowsAzure.Configuration**</span><span class="sxs-lookup"><span data-stu-id="3e116-111">**Microsoft.WindowsAzure.Configuration**</span></span>
* <span data-ttu-id="3e116-112">**Microsoft.WindowsAzure.Storage**</span><span class="sxs-lookup"><span data-stu-id="3e116-112">**Microsoft.WindowsAzure.Storage**</span></span>
* <span data-ttu-id="3e116-113">**Newtonsoft.Json**</span><span class="sxs-lookup"><span data-stu-id="3e116-113">**Newtonsoft.Json**</span></span>
* <span data-ttu-id="3e116-114">**System.Data**</span><span class="sxs-lookup"><span data-stu-id="3e116-114">**System.Data**</span></span>
* <span data-ttu-id="3e116-115">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="3e116-115">**System.Spatial**</span></span>

## <a name="connection-string-for-azure-storage-added"></a><span data-ttu-id="3e116-116">Hozzáadott Azure Storage kapcsolati karakterlánca</span><span class="sxs-lookup"><span data-stu-id="3e116-116">Connection string for Azure Storage added</span></span>
<span data-ttu-id="3e116-117">Elemek hello kiválasztott tárfiók kapcsolati karakterláncot és a kulcs-ekkel hozta létre.</span><span class="sxs-lookup"><span data-stu-id="3e116-117">Elements were created with hello selected storage account's connection string and key.</span></span> <span data-ttu-id="3e116-118">Változtatás történt volna a következő fájlok toohello:</span><span class="sxs-lookup"><span data-stu-id="3e116-118">Modifications were made toohello following files:</span></span>

* <span data-ttu-id="3e116-119">**ServiceDefinition.csdef**</span><span class="sxs-lookup"><span data-stu-id="3e116-119">**ServiceDefinition.csdef**</span></span>
* <span data-ttu-id="3e116-120">**ServiceConfiguration.Cloud.cscfg**</span><span class="sxs-lookup"><span data-stu-id="3e116-120">**ServiceConfiguration.Cloud.cscfg**</span></span>
* <span data-ttu-id="3e116-121">**ServiceConfiguration.Local.cscfg**</span><span class="sxs-lookup"><span data-stu-id="3e116-121">**ServiceConfiguration.Local.cscfg**</span></span>

