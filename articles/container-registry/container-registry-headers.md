---
title: "Azure-tárolót beállításjegyzék adattárak |} Microsoft Docs"
description: "Azure-tároló beállításjegyzék adattárak Docker lemezképek használata"
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
ms.openlocfilehash: dd4feff057269ed7106990bb63eed7fcffa2dbec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-container-registry-repositories"></a><span data-ttu-id="2859a-103">Azure-tárolót beállításjegyzék adattárak</span><span class="sxs-lookup"><span data-stu-id="2859a-103">Azure container registry repositories</span></span>

<span data-ttu-id="2859a-104">Az Azure tároló nyilvántartó szolgáltatások és orchestrators számos kompatibilisek.</span><span class="sxs-lookup"><span data-stu-id="2859a-104">Azure Container Registries are compatible with a multitude of services and orchestrators.</span></span> <span data-ttu-id="2859a-105">Könnyebb nyomon követheti a forrás-szolgáltatások és az ügynökök, amelyből ACR használatos, azt elindította a Docker fejlécmező az Docker.config fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="2859a-105">To make it easier to track the source services and agents from which ACR is used, we have started using the Docker header field in the Docker.config file.</span></span>



## <a name="viewing-repositories-in-the-portal"></a><span data-ttu-id="2859a-106">A portálon megtekintik adattárak</span><span class="sxs-lookup"><span data-stu-id="2859a-106">Viewing repositories in the Portal</span></span>

<span data-ttu-id="2859a-107">A ACR fejlécek a formátumot követi:</span><span class="sxs-lookup"><span data-stu-id="2859a-107">The ACR headers follow the format:</span></span>
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* <span data-ttu-id="2859a-108">Felhő: Azure, Azure verem, vagy más kormányzati vagy ország-specifikus Azure felhők.</span><span class="sxs-lookup"><span data-stu-id="2859a-108">Cloud: Azure, Azure Stack, or other government or country-specific Azure clouds.</span></span> <span data-ttu-id="2859a-109">Azure verem és a kormányzati jelenleg nem támogatott, bár ez a paraméter lehetővé teszi a jövőbeli támogatási.</span><span class="sxs-lookup"><span data-stu-id="2859a-109">Although Azure Stack and government clouds are not currently supported, this parameter enables future support.</span></span>
* <span data-ttu-id="2859a-110">Szolgáltatás: a szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="2859a-110">Service: name of the service.</span></span>
* <span data-ttu-id="2859a-111">Optionalservicename: szolgáltatások subservices, vagy adjon meg egy SKU nem kötelező paraméter (pl.: webalkalmazások megfeleljen az Azure-beli/app-szolgáltatás vagy-webalkalmazások).</span><span class="sxs-lookup"><span data-stu-id="2859a-111">Optionalservicename: optional parameter for services with subservices, or to specify a SKU (ex: web apps correspond with Azure/app-service/web-apps).</span></span>

<span data-ttu-id="2859a-112">Partneri szolgáltatások és orchestrators javasolt, hogy adott térközkaraktert használja a telemetriai adatok számára.</span><span class="sxs-lookup"><span data-stu-id="2859a-112">Partner services and orchestrators are encouraged to use specific header values to help with our telemetry.</span></span> <span data-ttu-id="2859a-113">Felhasználók is módosíthatja a értéket kapott a fejlécre, ha ezt kívánják.</span><span class="sxs-lookup"><span data-stu-id="2859a-113">Users can also modify the value passed to the header if they so desire.</span></span>

<span data-ttu-id="2859a-114">Az értékek szeretnénk ACR partnerek számára, hogy az "X-Meta-forrás-ügyfél" mező tölti ki az alábbi:</span><span class="sxs-lookup"><span data-stu-id="2859a-114">The values we want ACR partners to use to populate the "X-Meta-Source-Client" field are below:</span></span>

| <span data-ttu-id="2859a-115">Szolgáltatás neve</span><span class="sxs-lookup"><span data-stu-id="2859a-115">Service Name</span></span>              | <span data-ttu-id="2859a-116">Fejléc</span><span class="sxs-lookup"><span data-stu-id="2859a-116">Header</span></span>                                |
| ------------------------- | ------------------------------------- |
| <span data-ttu-id="2859a-117">Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="2859a-117">Azure Container Service</span></span>   | <span data-ttu-id="2859a-118">Azure/compute/azure-tároló-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="2859a-118">azure/compute/azure-container-service</span></span> |
| <span data-ttu-id="2859a-119">Az App Service - webalkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="2859a-119">App Service - Web Apps</span></span>    | <span data-ttu-id="2859a-120">Azure-beli/app-szolgáltatás vagy-webalkalmazások</span><span class="sxs-lookup"><span data-stu-id="2859a-120">azure/app-service/web-apps</span></span>            |
| <span data-ttu-id="2859a-121">Az App Service - Logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="2859a-121">App Service - Logic Apps</span></span>  | <span data-ttu-id="2859a-122">Azure/app-service/logikai-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="2859a-122">azure/app-service/logic-apps</span></span>          |
| <span data-ttu-id="2859a-123">Batch</span><span class="sxs-lookup"><span data-stu-id="2859a-123">Batch</span></span>                     | <span data-ttu-id="2859a-124">Azure/compute/kötegelt</span><span class="sxs-lookup"><span data-stu-id="2859a-124">azure/compute/batch</span></span>                   |
| <span data-ttu-id="2859a-125">Felhő konzol</span><span class="sxs-lookup"><span data-stu-id="2859a-125">Cloud Console</span></span>             | <span data-ttu-id="2859a-126">Azure/felhő-konzol</span><span class="sxs-lookup"><span data-stu-id="2859a-126">azure/cloud-console</span></span>                   |
| <span data-ttu-id="2859a-127">Functions</span><span class="sxs-lookup"><span data-stu-id="2859a-127">Functions</span></span>                 | <span data-ttu-id="2859a-128">Azure/compute/funkciók</span><span class="sxs-lookup"><span data-stu-id="2859a-128">azure/compute/functions</span></span>               |
| <span data-ttu-id="2859a-129">Az eszközök internetes hálózata - központ</span><span class="sxs-lookup"><span data-stu-id="2859a-129">Internet of Things - Hub</span></span>  | <span data-ttu-id="2859a-130">Azure-iot-központ</span><span class="sxs-lookup"><span data-stu-id="2859a-130">azure/iot/hub</span></span>                         |
| <span data-ttu-id="2859a-131">HDInsight</span><span class="sxs-lookup"><span data-stu-id="2859a-131">HDInsight</span></span>                 | <span data-ttu-id="2859a-132">Azure/data/hdinsight</span><span class="sxs-lookup"><span data-stu-id="2859a-132">azure/data/hdinsight</span></span>                  |
| <span data-ttu-id="2859a-133">Jenkins</span><span class="sxs-lookup"><span data-stu-id="2859a-133">Jenkins</span></span>                   | <span data-ttu-id="2859a-134">Azure/jenkins</span><span class="sxs-lookup"><span data-stu-id="2859a-134">azure/jenkins</span></span>                         |
| <span data-ttu-id="2859a-135">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="2859a-135">Machine Learning</span></span>          | <span data-ttu-id="2859a-136">Azure/data/machile-tanulás</span><span class="sxs-lookup"><span data-stu-id="2859a-136">azure/data/machile-learning</span></span>           |
| <span data-ttu-id="2859a-137">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="2859a-137">Service Fabric</span></span>            | <span data-ttu-id="2859a-138">Azure/compute/service-háló</span><span class="sxs-lookup"><span data-stu-id="2859a-138">azure/compute/service-fabric</span></span>          |
| <span data-ttu-id="2859a-139">VSTS</span><span class="sxs-lookup"><span data-stu-id="2859a-139">VSTS</span></span>                      | <span data-ttu-id="2859a-140">Azure/vsts</span><span class="sxs-lookup"><span data-stu-id="2859a-140">azure/vsts</span></span>                            |


## <a name="next-steps"></a><span data-ttu-id="2859a-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2859a-141">Next steps</span></span>
[<span data-ttu-id="2859a-142">További tudnivalók nyilvántartó és a támogatott szolgáltatások és orchestrators</span><span class="sxs-lookup"><span data-stu-id="2859a-142">Learn more about registries and the supported services and orchestrators</span></span>](container-registry-intro.md)
