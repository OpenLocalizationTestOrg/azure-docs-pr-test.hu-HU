---
title: Service Fabric Docker Compose Preview aaaAzure
description: "Az Azure Service Fabric elfogadja a Docker Compose formátum toomake azt könnyebb tooorchestrate meglévő tárolók Service Fabric használatával. Ez a támogatás jelenleg előzetes verzió."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: a60d1321fd6ef07b241a98c5ab2b8dfe5d441b53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="docker-compose-application-support-in-azure-service-fabric-preview"></a><span data-ttu-id="6d21c-104">Docker Compose alkalmazások támogatása az Azure Service Fabric (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="6d21c-104">Docker Compose application support in Azure Service Fabric (Preview)</span></span>

<span data-ttu-id="6d21c-105">Docker használ hello [docker-compose.yml](https://docs.docker.com/compose) fájl több tároló alkalmazások meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="6d21c-105">Docker uses hello [docker-compose.yml](https://docs.docker.com/compose) file for defining multi-container applications.</span></span> <span data-ttu-id="6d21c-106">toomake az ügyfelek könnyen térhetnek ismeri a Docker tooorchestrate meglévő tároló alkalmazások az Azure Service Fabric, vannak megadva Docker Compose preview támogatása natív módon hello platform.</span><span class="sxs-lookup"><span data-stu-id="6d21c-106">toomake it easy for customers familiar with Docker tooorchestrate existing container applications on Azure Service Fabric, we have included preview support for Docker Compose natively in hello platform.</span></span> <span data-ttu-id="6d21c-107">A Service Fabric fogadhat 3 és az újabb `docker-compose.yml` fájlokat.</span><span class="sxs-lookup"><span data-stu-id="6d21c-107">Service Fabric can accept version 3 and later of `docker-compose.yml` files.</span></span> 

<span data-ttu-id="6d21c-108">Ez a támogatás jelenleg előzetes verzióban érhető, mert a Compose irányelvek csak egy részhalmazát esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="6d21c-108">Because this support is in preview, only a subset of Compose directives is supported.</span></span> <span data-ttu-id="6d21c-109">Például alkalmazás frissítések nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="6d21c-109">For example, application upgrades are not supported.</span></span> <span data-ttu-id="6d21c-110">Azonban mindig távolítsa el és telepíthet központilag alkalmazásokat helyett őket.</span><span class="sxs-lookup"><span data-stu-id="6d21c-110">However, you can always remove and deploy applications instead of upgrading them.</span></span>

<span data-ttu-id="6d21c-111">toouse ez megtekintéséhez a fürt létrehozása verziójával 5.7-es vagy nagyobb a Service Fabric-futtatókörnyezet hello hello hello megfelelő SDK és Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="6d21c-111">toouse this preview, create your cluster with version 5.7 or greater of hello Service Fabric runtime through hello Azure portal along with hello corresponding SDK.</span></span> 

> [!NOTE]
> <span data-ttu-id="6d21c-112">Ez a funkció jelenleg előzetes verzióban érhető és éles környezetben nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6d21c-112">This feature is in preview and is not supported in production.</span></span>

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a><span data-ttu-id="6d21c-113">A Service Fabric a Docker Compose fájl központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6d21c-113">Deploy a Docker Compose file on Service Fabric</span></span>

<span data-ttu-id="6d21c-114">hello következő parancsokat a Service Fabric-alkalmazás létrehozása (nevű `fabric:/TestContainerApp` a fenti példa hello), amelyet figyelése és kezelése, mint bármely más Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6d21c-114">hello following commands create a Service Fabric application (named `fabric:/TestContainerApp` in hello preceding example), which you can monitor and manage like any other Service Fabric application.</span></span> <span data-ttu-id="6d21c-115">Használhatja a megadott alkalmazásnév hello állapotlekérdezések száma.</span><span class="sxs-lookup"><span data-stu-id="6d21c-115">You can use hello specified application name for health queries.</span></span>

### <a name="use-powershell"></a><span data-ttu-id="6d21c-116">A PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="6d21c-116">Use PowerShell</span></span>

<span data-ttu-id="6d21c-117">Egy docker-compose.yml fájlt a Service Fabric Compose-alkalmazás létrehozása a következő parancsot a PowerShell hello futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6d21c-117">Create a Service Fabric Compose application from a docker-compose.yml file by running hello following command in PowerShell:</span></span>

```powershell
New-ServiceFabricComposeApplication -ApplicationName fabric:/TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

<span data-ttu-id="6d21c-118">`RegistryUserName`és `RegistryPassword` tekintse meg a toohello tároló beállításjegyzék felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="6d21c-118">`RegistryUserName` and `RegistryPassword` refer toohello container registry username and password.</span></span> <span data-ttu-id="6d21c-119">Hello alkalmazás befejezése után ellenőrizheti annak állapotát hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="6d21c-119">After you've completed hello application, you can check its status by using hello following command:</span></span>

```powershell
Get-ServiceFabricComposeApplicationStatus -ApplicationName fabric:/TestContainerApp -GetAllPages
```

<span data-ttu-id="6d21c-120">toodelete hello Compose alkalmazást PowerShell, a következő parancs használata hello keresztül:</span><span class="sxs-lookup"><span data-stu-id="6d21c-120">toodelete hello Compose application through PowerShell, use hello following command:</span></span>

```powershell
Remove-ServiceFabricComposeApplication  -ApplicationName fabric:/TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a><span data-ttu-id="6d21c-121">Azure Service Fabric CLI (sfctl) használata</span><span class="sxs-lookup"><span data-stu-id="6d21c-121">Use Azure Service Fabric CLI (sfctl)</span></span>

<span data-ttu-id="6d21c-122">Service Fabric CLI parancs a következő hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="6d21c-122">Alternatively, you can use hello following Service Fabric CLI command:</span></span>

```azurecli
sfctl compose create --application-id fabric:/TestContainerApp --compose-file docker-compose.yml [ [ --repo-user --repo-pass --encrypted ] | [ --repo-user ] ] [ --timeout ]
```

<span data-ttu-id="6d21c-123">Hello alkalmazás létrehozása után hello a következő parancs használatával ellenőrizheti az állapotát:</span><span class="sxs-lookup"><span data-stu-id="6d21c-123">After you've created hello application, you can check its status by using hello following command:</span></span>

```azurecli
sfctl compose status --application-id TestContainerApp [ --timeout ]
```

<span data-ttu-id="6d21c-124">toodelete hello új alkalmazást, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="6d21c-124">toodelete hello Compose application, use hello following command:</span></span>

```azurecli
sfctl compose remove  --application-id TestContainerApp [ --timeout ]
```

## <a name="supported-compose-directives"></a><span data-ttu-id="6d21c-125">Támogatott Compose irányelvek</span><span class="sxs-lookup"><span data-stu-id="6d21c-125">Supported Compose directives</span></span>

<span data-ttu-id="6d21c-126">Ez az előzetes kiadás egy részhalmazát hello konfigurációs beállítások formátumból hello Compose 3-as verzió, beleértve a következő primitívek hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="6d21c-126">This preview supports a subset of hello configuration options from hello Compose version 3 format, including hello following primitives:</span></span>

* <span data-ttu-id="6d21c-127">Szolgáltatások > telepítése > replikák</span><span class="sxs-lookup"><span data-stu-id="6d21c-127">Services > Deploy > Replicas</span></span>
* <span data-ttu-id="6d21c-128">Szolgáltatások > telepítése > elhelyezési > megkötések</span><span class="sxs-lookup"><span data-stu-id="6d21c-128">Services > Deploy > Placement > Constraints</span></span>
* <span data-ttu-id="6d21c-129">Szolgáltatások > telepítése > erőforrások > korlátok</span><span class="sxs-lookup"><span data-stu-id="6d21c-129">Services > Deploy > Resources > Limits</span></span>
    * <span data-ttu-id="6d21c-130">-cpu-megosztások</span><span class="sxs-lookup"><span data-stu-id="6d21c-130">-cpu-shares</span></span>
    * <span data-ttu-id="6d21c-131">-memória</span><span class="sxs-lookup"><span data-stu-id="6d21c-131">-memory</span></span>
    * <span data-ttu-id="6d21c-132">-memória-csere</span><span class="sxs-lookup"><span data-stu-id="6d21c-132">-memory-swap</span></span>
* <span data-ttu-id="6d21c-133">Szolgáltatások > parancsok</span><span class="sxs-lookup"><span data-stu-id="6d21c-133">Services > Commands</span></span>
* <span data-ttu-id="6d21c-134">Szolgáltatások > környezet</span><span class="sxs-lookup"><span data-stu-id="6d21c-134">Services > Environment</span></span>
* <span data-ttu-id="6d21c-135">Szolgáltatások > portok</span><span class="sxs-lookup"><span data-stu-id="6d21c-135">Services > Ports</span></span>
* <span data-ttu-id="6d21c-136">Szolgáltatások > kép</span><span class="sxs-lookup"><span data-stu-id="6d21c-136">Services > Image</span></span>
* <span data-ttu-id="6d21c-137">Szolgáltatások > elszigetelése (csak Windows)</span><span class="sxs-lookup"><span data-stu-id="6d21c-137">Services > Isolation (only for Windows)</span></span>
* <span data-ttu-id="6d21c-138">Szolgáltatások > Naplózás > illesztőprogram</span><span class="sxs-lookup"><span data-stu-id="6d21c-138">Services > Logging > Driver</span></span>
* <span data-ttu-id="6d21c-139">Szolgáltatások > Naplózás > illesztőprogram > Beállítások</span><span class="sxs-lookup"><span data-stu-id="6d21c-139">Services > Logging > Driver > Options</span></span>
* <span data-ttu-id="6d21c-140">Kötet & telepítése > kötet</span><span class="sxs-lookup"><span data-stu-id="6d21c-140">Volume & Deploy > Volume</span></span>

<span data-ttu-id="6d21c-141">Hello fürt erőforrás-korlátok, felelősek beállítása, a [Service Fabric erőforrás irányítás](service-fabric-resource-governance.md).</span><span class="sxs-lookup"><span data-stu-id="6d21c-141">Set up hello cluster for enforcing resource limits, as described in [Service Fabric resource governance](service-fabric-resource-governance.md).</span></span> <span data-ttu-id="6d21c-142">Minden más Docker Compose irányelvek nem támogatottak az előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="6d21c-142">All other Docker Compose directives are unsupported for this preview.</span></span>

## <a name="servicednsname-computation"></a><span data-ttu-id="6d21c-143">ServiceDnsName számítási</span><span class="sxs-lookup"><span data-stu-id="6d21c-143">ServiceDnsName computation</span></span>

<span data-ttu-id="6d21c-144">Ha hello szolgáltatás neve meg egy új fájlt a teljes tartománynév (Ez azt jelenti, hogy pontot tartalmaz [.]), a Service Fabric által regisztrált hello DNS-név `<ServiceName>` (beleértve a hello pont).</span><span class="sxs-lookup"><span data-stu-id="6d21c-144">If hello service name that you specify in a Compose file is a fully qualified domain name (that is, it contains a dot [.]), hello DNS name registered by Service Fabric is `<ServiceName>` (including hello dot).</span></span> <span data-ttu-id="6d21c-145">Ha nem, akkor minden elérésiút-szegmens az hello alkalmazásnév hello szolgáltatás DNS-név, hello első elérésiút-szegmens hello felső szintű tartomány címke váljon a tartomány címke válik.</span><span class="sxs-lookup"><span data-stu-id="6d21c-145">If not, each path segment in hello application name becomes a domain label in hello service DNS name, with hello first path segment becoming hello top-level domain label.</span></span>

<span data-ttu-id="6d21c-146">Például ha hello megadott alkalmazásnév nem `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` hello regisztrált DNS-neve lesz.</span><span class="sxs-lookup"><span data-stu-id="6d21c-146">For example, if hello specified application name is `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` would be hello registered DNS name.</span></span>

## <a name="differences-between-compose-instance-definition-and-service-fabric-application-model-type-definition"></a><span data-ttu-id="6d21c-147">Új (példány-definíció) és a Service Fabric-alkalmazás modell (típusdefinícióban) közötti különbségek</span><span class="sxs-lookup"><span data-stu-id="6d21c-147">Differences between Compose (instance definition) and Service Fabric application model (type definition)</span></span>

<span data-ttu-id="6d21c-148">Egy docker-compose.yml fájlt a tárolók, beleértve azok tulajdonságait és konfigurációk telepíthető együttesét írja le.</span><span class="sxs-lookup"><span data-stu-id="6d21c-148">A docker-compose.yml file describes a deployable set of containers, including their properties and configurations.</span></span>
<span data-ttu-id="6d21c-149">Például hello fájl tartalmazhat környezeti változókat és a portok.</span><span class="sxs-lookup"><span data-stu-id="6d21c-149">For example, hello file can contain environment variables and ports.</span></span> <span data-ttu-id="6d21c-150">Telepítési paraméterek, például egy elhelyezési korlátozás, erőforrás-korlátozások és DNS-nevek, a hello docker-compose.yml fájlt is megadható.</span><span class="sxs-lookup"><span data-stu-id="6d21c-150">You can also specify deployment parameters, such as placement constraints, resource limits, and DNS names, in hello docker-compose.yml file.</span></span>

<span data-ttu-id="6d21c-151">Hello [Service Fabric-alkalmazás modell](service-fabric-application-model.md) használ szolgáltatás és alkalmazás típusainak, sok alkalmazás példány esetében hello ugyanarra a típusra.</span><span class="sxs-lookup"><span data-stu-id="6d21c-151">hello [Service Fabric application model](service-fabric-application-model.md) uses service types and application types, where you can have many application instances of hello same type.</span></span> <span data-ttu-id="6d21c-152">Lehet például egy ügyfél egy alkalmazáspéldányt.</span><span class="sxs-lookup"><span data-stu-id="6d21c-152">For example, you can have one application instance per customer.</span></span> <span data-ttu-id="6d21c-153">Ez a típus-alapú modell hello több verzióit támogatja ugyanazt az alkalmazástípus hello futásidejű regisztrált.</span><span class="sxs-lookup"><span data-stu-id="6d21c-153">This type-based model supports multiple versions of hello same application type that's registered with hello runtime.</span></span>

<span data-ttu-id="6d21c-154">Például az ügyfél A rendelkezhet példányként létrehozott AppTypeA 1.0 típusú kérelmet, és B ügyfél rendelkezhet a hello példányként létrehozott egy másik alkalmazás azonos típusú és verziót.</span><span class="sxs-lookup"><span data-stu-id="6d21c-154">For example, customer A can have an application instantiated with type 1.0 of AppTypeA, and customer B can have another application instantiated with hello same type and version.</span></span> <span data-ttu-id="6d21c-155">Hello alkalmazástípusok hello alkalmazásjegyzékeknek határozhat meg, és hello alkalmazás nevét és a telepítési paraméterek megadva hello alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="6d21c-155">You define hello application types in hello application manifests, and you specify hello application name and deployment parameters when you create hello application.</span></span>

<span data-ttu-id="6d21c-156">Bár ez a modell rugalmasságot nyújt, azt is tervezi, ahol típusok tartoznak, amely implicit hello jegyzékfájl egyszerűbb, a példány-alapú telepítési modell toosupport.</span><span class="sxs-lookup"><span data-stu-id="6d21c-156">Although this model offers flexibility, we are also planning toosupport a simpler, instance-based deployment model where types are implicit from hello manifest file.</span></span> <span data-ttu-id="6d21c-157">Ebben a modellben minden alkalmazás saját független jegyzékfájl lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="6d21c-157">In this model, each application gets its own independent manifest.</span></span> <span data-ttu-id="6d21c-158">Ebből a törekvésből támogatásának hozzáadásával a docker-compose.yml, ez az egy példány-alapú üzembe helyezési formátum jelenleg előzetes.</span><span class="sxs-lookup"><span data-stu-id="6d21c-158">We are previewing this effort by adding support for docker-compose.yml, which is an instance-based deployment format.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d21c-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d21c-159">Next steps</span></span>

* <span data-ttu-id="6d21c-160">Olvassa a hello [Service Fabric alkalmazásmodellt.](service-fabric-application-model.md)</span><span class="sxs-lookup"><span data-stu-id="6d21c-160">Read up on hello [Service Fabric application model](service-fabric-application-model.md)</span></span>
* [<span data-ttu-id="6d21c-161">A Service Fabric parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="6d21c-161">Get started with Service Fabric CLI</span></span>](service-fabric-cli.md)
