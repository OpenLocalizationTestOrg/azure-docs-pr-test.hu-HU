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
# <a name="azure-container-registry-repositories"></a><span data-ttu-id="f7258-103">Azure-tárolót beállításjegyzék adattárak</span><span class="sxs-lookup"><span data-stu-id="f7258-103">Azure container registry repositories</span></span>

<span data-ttu-id="f7258-104">Az Azure tároló nyilvántartó szolgáltatások és orchestrators számos kompatibilisek.</span><span class="sxs-lookup"><span data-stu-id="f7258-104">Azure Container Registries are compatible with a multitude of services and orchestrators.</span></span> <span data-ttu-id="f7258-105">toomake azt könnyebb tootrack hello forrás szolgáltatások és az ügynökök, amelyből ACR használatos, azt elindította hello Docker fejlécmező hello Docker.config fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="f7258-105">toomake it easier tootrack hello source services and agents from which ACR is used, we have started using hello Docker header field in hello Docker.config file.</span></span>



## <a name="viewing-repositories-in-hello-portal"></a><span data-ttu-id="f7258-106">Hello Portal adattárak megtekintése</span><span class="sxs-lookup"><span data-stu-id="f7258-106">Viewing repositories in hello Portal</span></span>

<span data-ttu-id="f7258-107">hello ACR fejlécek hello formátumot követi:</span><span class="sxs-lookup"><span data-stu-id="f7258-107">hello ACR headers follow hello format:</span></span>
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* <span data-ttu-id="f7258-108">Felhő: Azure, Azure verem, vagy más kormányzati vagy ország-specifikus Azure felhők.</span><span class="sxs-lookup"><span data-stu-id="f7258-108">Cloud: Azure, Azure Stack, or other government or country-specific Azure clouds.</span></span> <span data-ttu-id="f7258-109">Azure verem és a kormányzati jelenleg nem támogatott, bár ez a paraméter lehetővé teszi a jövőbeli támogatási.</span><span class="sxs-lookup"><span data-stu-id="f7258-109">Although Azure Stack and government clouds are not currently supported, this parameter enables future support.</span></span>
* <span data-ttu-id="f7258-110">Szolgáltatás: hello szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="f7258-110">Service: name of hello service.</span></span>
* <span data-ttu-id="f7258-111">Optionalservicename: szolgáltatások subservices, vagy egy SKU toospecify nem kötelező paraméter (pl.: webalkalmazások megfeleljen az Azure-beli/app-szolgáltatás vagy-webalkalmazások).</span><span class="sxs-lookup"><span data-stu-id="f7258-111">Optionalservicename: optional parameter for services with subservices, or toospecify a SKU (ex: web apps correspond with Azure/app-service/web-apps).</span></span>

<span data-ttu-id="f7258-112">Partner szolgáltatásai és orchestrators a javasolt toouse specifikus fejléccel értékek toohelp rendelkező a telemetriai adatok.</span><span class="sxs-lookup"><span data-stu-id="f7258-112">Partner services and orchestrators are encouraged toouse specific header values toohelp with our telemetry.</span></span> <span data-ttu-id="f7258-113">Felhasználók módosíthatja átadott toohello fejléc, ha azok kívánják hello érték is.</span><span class="sxs-lookup"><span data-stu-id="f7258-113">Users can also modify hello value passed toohello header if they so desire.</span></span>

<span data-ttu-id="f7258-114">hello szeretnénk ACR partnerek toouse toopopulate hello "X-Meta-forrás-ügyfél" mező értékei alatt:</span><span class="sxs-lookup"><span data-stu-id="f7258-114">hello values we want ACR partners toouse toopopulate hello "X-Meta-Source-Client" field are below:</span></span>

| <span data-ttu-id="f7258-115">Szolgáltatás neve</span><span class="sxs-lookup"><span data-stu-id="f7258-115">Service Name</span></span>              | <span data-ttu-id="f7258-116">Fejléc</span><span class="sxs-lookup"><span data-stu-id="f7258-116">Header</span></span>                                |
| ------------------------- | ------------------------------------- |
| <span data-ttu-id="f7258-117">Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="f7258-117">Azure Container Service</span></span>   | <span data-ttu-id="f7258-118">Azure/compute/azure-tároló-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f7258-118">azure/compute/azure-container-service</span></span> |
| <span data-ttu-id="f7258-119">Az App Service - webalkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="f7258-119">App Service - Web Apps</span></span>    | <span data-ttu-id="f7258-120">Azure-beli/app-szolgáltatás vagy-webalkalmazások</span><span class="sxs-lookup"><span data-stu-id="f7258-120">azure/app-service/web-apps</span></span>            |
| <span data-ttu-id="f7258-121">Az App Service - Logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="f7258-121">App Service - Logic Apps</span></span>  | <span data-ttu-id="f7258-122">Azure/app-service/logikai-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="f7258-122">azure/app-service/logic-apps</span></span>          |
| <span data-ttu-id="f7258-123">Batch</span><span class="sxs-lookup"><span data-stu-id="f7258-123">Batch</span></span>                     | <span data-ttu-id="f7258-124">Azure/compute/kötegelt</span><span class="sxs-lookup"><span data-stu-id="f7258-124">azure/compute/batch</span></span>                   |
| <span data-ttu-id="f7258-125">Felhő konzol</span><span class="sxs-lookup"><span data-stu-id="f7258-125">Cloud Console</span></span>             | <span data-ttu-id="f7258-126">Azure/felhő-konzol</span><span class="sxs-lookup"><span data-stu-id="f7258-126">azure/cloud-console</span></span>                   |
| <span data-ttu-id="f7258-127">Functions</span><span class="sxs-lookup"><span data-stu-id="f7258-127">Functions</span></span>                 | <span data-ttu-id="f7258-128">Azure/compute/funkciók</span><span class="sxs-lookup"><span data-stu-id="f7258-128">azure/compute/functions</span></span>               |
| <span data-ttu-id="f7258-129">Az eszközök internetes hálózata - központ</span><span class="sxs-lookup"><span data-stu-id="f7258-129">Internet of Things - Hub</span></span>  | <span data-ttu-id="f7258-130">Azure-iot-központ</span><span class="sxs-lookup"><span data-stu-id="f7258-130">azure/iot/hub</span></span>                         |
| <span data-ttu-id="f7258-131">HDInsight</span><span class="sxs-lookup"><span data-stu-id="f7258-131">HDInsight</span></span>                 | <span data-ttu-id="f7258-132">Azure/data/hdinsight</span><span class="sxs-lookup"><span data-stu-id="f7258-132">azure/data/hdinsight</span></span>                  |
| <span data-ttu-id="f7258-133">Jenkins</span><span class="sxs-lookup"><span data-stu-id="f7258-133">Jenkins</span></span>                   | <span data-ttu-id="f7258-134">Azure/jenkins</span><span class="sxs-lookup"><span data-stu-id="f7258-134">azure/jenkins</span></span>                         |
| <span data-ttu-id="f7258-135">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="f7258-135">Machine Learning</span></span>          | <span data-ttu-id="f7258-136">Azure/data/machile-tanulás</span><span class="sxs-lookup"><span data-stu-id="f7258-136">azure/data/machile-learning</span></span>           |
| <span data-ttu-id="f7258-137">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f7258-137">Service Fabric</span></span>            | <span data-ttu-id="f7258-138">Azure/compute/service-háló</span><span class="sxs-lookup"><span data-stu-id="f7258-138">azure/compute/service-fabric</span></span>          |
| <span data-ttu-id="f7258-139">VSTS</span><span class="sxs-lookup"><span data-stu-id="f7258-139">VSTS</span></span>                      | <span data-ttu-id="f7258-140">Azure/vsts</span><span class="sxs-lookup"><span data-stu-id="f7258-140">azure/vsts</span></span>                            |


## <a name="next-steps"></a><span data-ttu-id="f7258-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f7258-141">Next steps</span></span>
[<span data-ttu-id="f7258-142">További információ a nyilvántartó és hello támogatott szolgáltatások orchestrators</span><span class="sxs-lookup"><span data-stu-id="f7258-142">Learn more about registries and hello supported services and orchestrators</span></span>](container-registry-intro.md)
