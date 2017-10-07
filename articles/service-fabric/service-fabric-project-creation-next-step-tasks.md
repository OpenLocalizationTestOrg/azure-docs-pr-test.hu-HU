---
title: "aaaService háló projekt létrehozásának további lépések |} Microsoft Docs"
description: "Ez a cikk tartalmaz hivatkozásokat tooa core fejlesztési feladatokhoz a Service Fabric"
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: rwike77
ms.openlocfilehash: 45598bfabedf280fba8af449ef920f40b409a609
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a><span data-ttu-id="59427-103">A Service Fabric-alkalmazás és a következő lépések</span><span class="sxs-lookup"><span data-stu-id="59427-103">Your Service Fabric application and next steps</span></span>
<span data-ttu-id="59427-104">Az Azure Service Fabric-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="59427-104">Your Azure Service Fabric application has been created.</span></span> <span data-ttu-id="59427-105">Ez a cikk ismerteti a projekt és a potenciális lépéseket hello makeup.</span><span class="sxs-lookup"><span data-stu-id="59427-105">This article describes hello makeup of your project and some potential next steps.</span></span>

## <a name="your-application"></a><span data-ttu-id="59427-106">Az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="59427-106">Your application</span></span>
<span data-ttu-id="59427-107">Minden egyes új alkalmazás egy alkalmazás projektet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="59427-107">Every new application includes an application project.</span></span> <span data-ttu-id="59427-108">Előfordulhat, hogy egy vagy két további projektek, attól függően, hogy a kiválasztott szolgáltatás hello típusát.</span><span class="sxs-lookup"><span data-stu-id="59427-108">There may be one or two additional projects, depending on hello type of service chosen.</span></span>

### <a name="hello-application-project"></a><span data-ttu-id="59427-109">hello projekt</span><span class="sxs-lookup"><span data-stu-id="59427-109">hello application project</span></span>
<span data-ttu-id="59427-110">hello projektet tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="59427-110">hello application project consists of:</span></span>

* <span data-ttu-id="59427-111">Az alkalmazás alkotó hivatkozások toohello szolgáltatások készlete.</span><span class="sxs-lookup"><span data-stu-id="59427-111">A set of references toohello services that make up your application.</span></span>
* <span data-ttu-id="59427-112">Profilok (1-csomópont helyi 5-csomópont helyi és felhőalapú) használható toomaintain beállításainak különböző környezetekhez – például a beállítások kapcsolódó tooa a fürt végpontja, és hogy tooperform végezze el központi telepítések alapértelmezés szerint három közzététele.</span><span class="sxs-lookup"><span data-stu-id="59427-112">Three publish profiles (1-Node Local, 5-Node Local, and Cloud) that you can use toomaintain preferences for working with different environments--such as preferences related tooa cluster endpoint and whether tooperform upgrade deployments by default.</span></span>
* <span data-ttu-id="59427-113">Három alkalmazásparaméter-fájlokat (megegyezik a fenti) használható toomaintain környezetfüggő alkalmazás beállításai, például egy szolgáltatás partíciók toocreate hello száma.</span><span class="sxs-lookup"><span data-stu-id="59427-113">Three application parameter files (same as above) that you can use toomaintain environment-specific application configurations, such as hello number of partitions toocreate for a service.</span></span>
* <span data-ttu-id="59427-114">A telepítési parancsfájl használható toodeploy az alkalmazás hello parancssorban vagy egy automatizált folyamatos integrációt és telepítést folyamat részeként.</span><span class="sxs-lookup"><span data-stu-id="59427-114">A deployment script that you can use toodeploy your application from hello command line or as part of an automated continuous integration and deployment pipeline.</span></span>
* <span data-ttu-id="59427-115">hello alkalmazásjegyzék, amely hello alkalmazás ismerteti.</span><span class="sxs-lookup"><span data-stu-id="59427-115">hello application manifest, which describes hello application.</span></span> <span data-ttu-id="59427-116">Hello jegyzékfájl hello ApplicationPackageRoot mappában található.</span><span class="sxs-lookup"><span data-stu-id="59427-116">You can find hello manifest under hello ApplicationPackageRoot folder.</span></span>

### <a name="stateless-service"></a><span data-ttu-id="59427-117">Állapotmentes szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="59427-117">Stateless service</span></span>
<span data-ttu-id="59427-118">Ha hozzáad egy új állapotmentes szolgáltatások, a Visual Studio hozzáadja-e a szolgáltatás projekt tooyour megoldás, amely tartalmazza a felvett típus `StatelessService`.</span><span class="sxs-lookup"><span data-stu-id="59427-118">When you add a new stateless service, Visual Studio adds a service project tooyour solution that includes a type descended from `StatelessService`.</span></span> <span data-ttu-id="59427-119">hello szolgáltatást egy helyi változót a számlálót növeli.</span><span class="sxs-lookup"><span data-stu-id="59427-119">hello service increments a local variable in a counter.</span></span>

### <a name="stateful-service"></a><span data-ttu-id="59427-120">Az állapotalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="59427-120">Stateful service</span></span>
<span data-ttu-id="59427-121">Ha hozzáad egy új állapotalapú szolgáltatásból, Visual Studio hozzáadja-e a szolgáltatási projekt tooyour megoldás, amely tartalmazza a felvett típus `StatefulService`.</span><span class="sxs-lookup"><span data-stu-id="59427-121">When you add a new stateful service, Visual Studio adds a service project tooyour solution that includes a type descended from `StatefulService`.</span></span> <span data-ttu-id="59427-122">hello szolgáltatás növeli a számlálót a `RunAsync` metódus és a tárolók hello eredményez a `ReliableDictionary`.</span><span class="sxs-lookup"><span data-stu-id="59427-122">hello service increments a counter in its `RunAsync` method and stores hello result in a `ReliableDictionary`.</span></span>

### <a name="actor-service"></a><span data-ttu-id="59427-123">Aktor szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="59427-123">Actor service</span></span>
<span data-ttu-id="59427-124">Ha hozzáad egy új megbízható szereplő, a Visual Studio hozzáadja két projektek tooyour megoldás: az aktor projektek és egy illesztőfelület-projekthez.</span><span class="sxs-lookup"><span data-stu-id="59427-124">When you add a new reliable actor, Visual Studio adds two projects tooyour solution: an actor project and an interface project.</span></span>

<span data-ttu-id="59427-125">hello szereplő projekt beállítás metódusokat biztosít, és a számláló értéke hello beolvasása, amely megbízhatóan megőrzött hello szereplő állapot belül.</span><span class="sxs-lookup"><span data-stu-id="59427-125">hello actor project provides methods for setting and getting hello value of a counter that is reliably persisted within hello actor's state.</span></span> <span data-ttu-id="59427-126">hello felület projekt olyan felületet biztosít a többi szolgáltatások tooinvoke hello szereplő használhatja.</span><span class="sxs-lookup"><span data-stu-id="59427-126">hello interface project provides an interface that other services can use tooinvoke hello actor.</span></span>

### <a name="stateless-web-api"></a><span data-ttu-id="59427-127">Állapot nélküli webes API-k</span><span class="sxs-lookup"><span data-stu-id="59427-127">Stateless Web API</span></span>
<span data-ttu-id="59427-128">hello állapotmentes Web API-projektet tartalmaz egy alapszintű webszolgáltatás használható tooopen az alkalmazás tooexternal ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="59427-128">hello stateless Web API project provides a basic web service that you can use tooopen your application tooexternal clients.</span></span> <span data-ttu-id="59427-129">Hogyan hello projekt strukturált kapcsolatos további információkért lásd: [Service Fabric Web API szolgáltatások OWIN önálló üzemeltető](service-fabric-reliable-services-communication-webapi.md).</span><span class="sxs-lookup"><span data-stu-id="59427-129">For more information about how hello project structured, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md).</span></span>


### <a name="aspnet-core"></a><span data-ttu-id="59427-130">Az ASP.NET core</span><span class="sxs-lookup"><span data-stu-id="59427-130">ASP.NET core</span></span>
<span data-ttu-id="59427-131">Service Fabric SDK hello biztosít azonos beállítása az ASP.NET Core sablonokat, amelyeket az ASP.NET Core projektek önálló hello: üres, [Web API][aspnet-webapi], és [webalkalmazás][aspnet-webapp].</span><span class="sxs-lookup"><span data-stu-id="59427-131">hello Service Fabric SDK provides hello same set of ASP.NET Core templates that are available for standalone ASP.NET Core projects: empty, [Web API][aspnet-webapi], and [Web Application][aspnet-webapp].</span></span>

### <a name="guest-executables-and-guest-containers"></a><span data-ttu-id="59427-132">Vendég végrehajtható fájlok és a Vendég-tárolókon</span><span class="sxs-lookup"><span data-stu-id="59427-132">Guest executables and guest containers</span></span>

<span data-ttu-id="59427-133">A Service Fabric "Vendég" egy olyan szolgáltatás, nem épített hello platform programozási modellek.</span><span class="sxs-lookup"><span data-stu-id="59427-133">A Service Fabric 'guest' is a service that is not built with hello platform's programming models.</span></span> <span data-ttu-id="59427-134">Csomagot hello bináris fájlok esetében a Vendég vagy [közvetlenül a hello alkalmazáscsomag](service-fabric-deploy-existing-app.md) vagy [keresztül a tároló lemezkép](service-fabric-deploy-container.md).</span><span class="sxs-lookup"><span data-stu-id="59427-134">You can package hello binaries for a guest either [directly in hello application package](service-fabric-deploy-existing-app.md) or [through a container image](service-fabric-deploy-container.md).</span></span> <span data-ttu-id="59427-135">Mindkét esetben a Visual Studio létrehozza hello hello a szükséges összetevők **ApplicationPackageRoot** hello projekt mappájából.</span><span class="sxs-lookup"><span data-stu-id="59427-135">In both cases, Visual Studio creates hello necessary artifacts in hello **ApplicationPackageRoot** folder of hello application project.</span></span> <span data-ttu-id="59427-136">A Visual Studio nem hoz létre egy új service-projekt, mert hello kódot már létezik a rendszerben található.</span><span class="sxs-lookup"><span data-stu-id="59427-136">Visual Studio will not create a new service project because hello code already exists elsewhere.</span></span> <span data-ttu-id="59427-137">Ha szeretné, hogy a Vendég projektek mellett hello Service Fabric-alkalmazás projekt toomanage, később is hozzáadhatja toohello azonos Visual Studio-megoldáshoz.</span><span class="sxs-lookup"><span data-stu-id="59427-137">If you would like toomanage your guest projects alongside hello Service Fabric application project, you can add them toohello same Visual Studio solution.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59427-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59427-138">Next steps</span></span>
### <a name="create-an-azure-cluster"></a><span data-ttu-id="59427-139">Egy Azure-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="59427-139">Create an Azure cluster</span></span>
<span data-ttu-id="59427-140">Service Fabric SDK hello biztosít a helyi fürt a fejlesztéshez és teszteléshez.</span><span class="sxs-lookup"><span data-stu-id="59427-140">hello Service Fabric SDK provides a local cluster for development and testing.</span></span> <span data-ttu-id="59427-141">az Azure, a fürt toocreate lásd: [a Service Fabric-fürt beállítása az Azure-portálon hello][create-cluster-in-portal].</span><span class="sxs-lookup"><span data-stu-id="59427-141">toocreate a cluster in Azure, see [Setting up a Service Fabric cluster from hello Azure portal][create-cluster-in-portal].</span></span>

### <a name="publish-your-application-tooazure"></a><span data-ttu-id="59427-142">Az alkalmazás tooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="59427-142">Publish your application tooAzure</span></span>
<span data-ttu-id="59427-143">A közvetlenül a Visual Studio tooan Azure-fürttel alkalmazást is közzétehet.</span><span class="sxs-lookup"><span data-stu-id="59427-143">You can publish your application directly from Visual Studio tooan Azure cluster.</span></span> <span data-ttu-id="59427-144">toolearn című témakörben talál [közzététele az alkalmazás tooAzure][publish-app-to-azure].</span><span class="sxs-lookup"><span data-stu-id="59427-144">toolearn how, see [Publishing your application tooAzure][publish-app-to-azure].</span></span>

### <a name="use-service-fabric-explorer-toovisualize-your-cluster"></a><span data-ttu-id="59427-145">Service Fabric Explorer toovisualize a fürt használatára</span><span class="sxs-lookup"><span data-stu-id="59427-145">Use Service Fabric Explorer toovisualize your cluster</span></span>
<span data-ttu-id="59427-146">Service Fabric Explorer nyújt egy egyszerű módot toovisualize a fürtöt, beleértve a központilag telepített alkalmazások és a fizikai elrendezését.</span><span class="sxs-lookup"><span data-stu-id="59427-146">Service Fabric Explorer offers an easy way toovisualize your cluster, including deployed applications and physical layout.</span></span> <span data-ttu-id="59427-147">több, lásd: toolearn [a fürt megjelenítése a Service Fabric Explorer használatával][visualize-with-sfx].</span><span class="sxs-lookup"><span data-stu-id="59427-147">toolearn more, see [Visualizing your cluster by using Service Fabric Explorer][visualize-with-sfx].</span></span>

### <a name="version-and-upgrade-your-services"></a><span data-ttu-id="59427-148">Verziója és a szolgáltatások frissítésére</span><span class="sxs-lookup"><span data-stu-id="59427-148">Version and upgrade your services</span></span>
<span data-ttu-id="59427-149">A Service Fabric lehetővé teszi, hogy függetlenül verziószámozásának és független az alkalmazás szolgáltatások frissítésével.</span><span class="sxs-lookup"><span data-stu-id="59427-149">Service Fabric enables independent versioning and upgrading of independent services in an application.</span></span> <span data-ttu-id="59427-150">több, lásd: toolearn [Versioning és a szolgáltatások frissítése][app-upgrade-tutorial].</span><span class="sxs-lookup"><span data-stu-id="59427-150">toolearn more, see [Versioning and upgrading your services][app-upgrade-tutorial].</span></span>

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a><span data-ttu-id="59427-151">A Visual Studio Team Services folyamatos integráció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="59427-151">Configure continuous integration with Visual Studio Team Services</span></span>
<span data-ttu-id="59427-152">toolearn hogyan állíthatja be a folyamatos integrációs folyamatban a Service Fabric-alkalmazás, lásd: [folyamatos integráció konfigurálása a Visual Studio Team Services][ci-with-vso].</span><span class="sxs-lookup"><span data-stu-id="59427-152">toolearn how you can set up a continuous integration process for your Service Fabric application, see [Configure continuous integration with Visual Studio Team Services][ci-with-vso].</span></span>

<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
