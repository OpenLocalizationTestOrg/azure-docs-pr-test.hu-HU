---
title: "Az Azure Service Fabric Docker Compose előzetes verzió"
description: "Az Azure Service Fabric a Docker Compose formátum használatával könnyebben lehet levezényelni a Service Fabric használatával meglévő tárolók fogad el. Ez a támogatás jelenleg előzetes verzió."
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
ms.openlocfilehash: e05d1a3d6111e3bbc34008226bcd1fdf35935450
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="docker-compose-application-support-in-azure-service-fabric-preview"></a><span data-ttu-id="43cde-104">Docker Compose alkalmazások támogatása az Azure Service Fabric (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="43cde-104">Docker Compose application support in Azure Service Fabric (Preview)</span></span>

<span data-ttu-id="43cde-105">Docker használja a [docker-compose.yml](https://docs.docker.com/compose) fájl több tároló alkalmazások meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="43cde-105">Docker uses the [docker-compose.yml](https://docs.docker.com/compose) file for defining multi-container applications.</span></span> <span data-ttu-id="43cde-106">Megkönnyítheti a felhasználók ismernie kell levezényelni a meglévő tárolóhoz alkalmazások az Azure Service Fabric Docker, vannak megadva Docker Compose preview támogatása natív módon a platform.</span><span class="sxs-lookup"><span data-stu-id="43cde-106">To make it easy for customers familiar with Docker to orchestrate existing container applications on Azure Service Fabric, we have included preview support for Docker Compose natively in the platform.</span></span> <span data-ttu-id="43cde-107">A Service Fabric fogadhat 3 és az újabb `docker-compose.yml` fájlokat.</span><span class="sxs-lookup"><span data-stu-id="43cde-107">Service Fabric can accept version 3 and later of `docker-compose.yml` files.</span></span> 

<span data-ttu-id="43cde-108">Ez a támogatás jelenleg előzetes verzióban érhető, mert a Compose irányelvek csak egy részhalmazát esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="43cde-108">Because this support is in preview, only a subset of Compose directives is supported.</span></span> <span data-ttu-id="43cde-109">Például alkalmazás frissítések nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="43cde-109">For example, application upgrades are not supported.</span></span> <span data-ttu-id="43cde-110">Azonban mindig távolítsa el és telepíthet központilag alkalmazásokat helyett őket.</span><span class="sxs-lookup"><span data-stu-id="43cde-110">However, you can always remove and deploy applications instead of upgrading them.</span></span>

<span data-ttu-id="43cde-111">Ez az előzetes kiadás használatához hozzon létre a fürt verziójával 5.7-es vagy nagyobb a Service Fabric-futtatókörnyezet, valamint a megfelelő SDK az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="43cde-111">To use this preview, create your cluster with version 5.7 or greater of the Service Fabric runtime through the Azure portal along with the corresponding SDK.</span></span> 

> [!NOTE]
> <span data-ttu-id="43cde-112">Ez a funkció jelenleg előzetes verzióban érhető és éles környezetben nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="43cde-112">This feature is in preview and is not supported in production.</span></span>

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a><span data-ttu-id="43cde-113">A Service Fabric a Docker Compose fájl központi telepítése</span><span class="sxs-lookup"><span data-stu-id="43cde-113">Deploy a Docker Compose file on Service Fabric</span></span>

<span data-ttu-id="43cde-114">Az alábbi parancsokat a Service Fabric-alkalmazás létrehozása (nevű `fabric:/TestContainerApp` az előző példában), amelyet figyelése és kezelése, mint bármely más Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="43cde-114">The following commands create a Service Fabric application (named `fabric:/TestContainerApp` in the preceding example), which you can monitor and manage like any other Service Fabric application.</span></span> <span data-ttu-id="43cde-115">A megadott alkalmazásnév állapotlekérdezések is használhatja.</span><span class="sxs-lookup"><span data-stu-id="43cde-115">You can use the specified application name for health queries.</span></span>

### <a name="use-powershell"></a><span data-ttu-id="43cde-116">A PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="43cde-116">Use PowerShell</span></span>

<span data-ttu-id="43cde-117">Egy docker-compose.yml fájlt a Service Fabric Compose-alkalmazás létrehozása a PowerShell a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="43cde-117">Create a Service Fabric Compose application from a docker-compose.yml file by running the following command in PowerShell:</span></span>

```powershell
New-ServiceFabricComposeApplication -ApplicationName fabric:/TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

<span data-ttu-id="43cde-118">`RegistryUserName`és `RegistryPassword` tekintse meg a tároló beállításjegyzék felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="43cde-118">`RegistryUserName` and `RegistryPassword` refer to the container registry username and password.</span></span> <span data-ttu-id="43cde-119">Miután megadta az alkalmazást, annak állapotát a következő paranccsal ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="43cde-119">After you've completed the application, you can check its status by using the following command:</span></span>

```powershell
Get-ServiceFabricComposeApplicationStatus -ApplicationName fabric:/TestContainerApp -GetAllPages
```

<span data-ttu-id="43cde-120">A PowerShell segítségével az új alkalmazás törléséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="43cde-120">To delete the Compose application through PowerShell, use the following command:</span></span>

```powershell
Remove-ServiceFabricComposeApplication  -ApplicationName fabric:/TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a><span data-ttu-id="43cde-121">Azure Service Fabric CLI (sfctl) használata</span><span class="sxs-lookup"><span data-stu-id="43cde-121">Use Azure Service Fabric CLI (sfctl)</span></span>

<span data-ttu-id="43cde-122">Másik lehetőségként használhatja a következő Service Fabric CLI parancsot:</span><span class="sxs-lookup"><span data-stu-id="43cde-122">Alternatively, you can use the following Service Fabric CLI command:</span></span>

```azurecli
sfctl compose create --application-id fabric:/TestContainerApp --compose-file docker-compose.yml [ [ --repo-user --repo-pass --encrypted ] | [ --repo-user ] ] [ --timeout ]
```

<span data-ttu-id="43cde-123">Miután létrehozta az alkalmazást, annak állapotát a következő paranccsal ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="43cde-123">After you've created the application, you can check its status by using the following command:</span></span>

```azurecli
sfctl compose status --application-id TestContainerApp [ --timeout ]
```

<span data-ttu-id="43cde-124">Az új alkalmazás törléséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="43cde-124">To delete the Compose application, use the following command:</span></span>

```azurecli
sfctl compose remove  --application-id TestContainerApp [ --timeout ]
```

## <a name="supported-compose-directives"></a><span data-ttu-id="43cde-125">Támogatott Compose irányelvek</span><span class="sxs-lookup"><span data-stu-id="43cde-125">Supported Compose directives</span></span>

<span data-ttu-id="43cde-126">Ez az előzetes kiadás egy részét a konfigurációs beállítások formátumból a Compose 3-as verzió, beleértve a következő primitívek támogatja:</span><span class="sxs-lookup"><span data-stu-id="43cde-126">This preview supports a subset of the configuration options from the Compose version 3 format, including the following primitives:</span></span>

* <span data-ttu-id="43cde-127">Szolgáltatások > telepítése > replikák</span><span class="sxs-lookup"><span data-stu-id="43cde-127">Services > Deploy > Replicas</span></span>
* <span data-ttu-id="43cde-128">Szolgáltatások > telepítése > elhelyezési > megkötések</span><span class="sxs-lookup"><span data-stu-id="43cde-128">Services > Deploy > Placement > Constraints</span></span>
* <span data-ttu-id="43cde-129">Szolgáltatások > telepítése > erőforrások > korlátok</span><span class="sxs-lookup"><span data-stu-id="43cde-129">Services > Deploy > Resources > Limits</span></span>
    * <span data-ttu-id="43cde-130">-cpu-megosztások</span><span class="sxs-lookup"><span data-stu-id="43cde-130">-cpu-shares</span></span>
    * <span data-ttu-id="43cde-131">-memória</span><span class="sxs-lookup"><span data-stu-id="43cde-131">-memory</span></span>
    * <span data-ttu-id="43cde-132">-memória-csere</span><span class="sxs-lookup"><span data-stu-id="43cde-132">-memory-swap</span></span>
* <span data-ttu-id="43cde-133">Szolgáltatások > parancsok</span><span class="sxs-lookup"><span data-stu-id="43cde-133">Services > Commands</span></span>
* <span data-ttu-id="43cde-134">Szolgáltatások > környezet</span><span class="sxs-lookup"><span data-stu-id="43cde-134">Services > Environment</span></span>
* <span data-ttu-id="43cde-135">Szolgáltatások > portok</span><span class="sxs-lookup"><span data-stu-id="43cde-135">Services > Ports</span></span>
* <span data-ttu-id="43cde-136">Szolgáltatások > kép</span><span class="sxs-lookup"><span data-stu-id="43cde-136">Services > Image</span></span>
* <span data-ttu-id="43cde-137">Szolgáltatások > elszigetelése (csak Windows)</span><span class="sxs-lookup"><span data-stu-id="43cde-137">Services > Isolation (only for Windows)</span></span>
* <span data-ttu-id="43cde-138">Szolgáltatások > Naplózás > illesztőprogram</span><span class="sxs-lookup"><span data-stu-id="43cde-138">Services > Logging > Driver</span></span>
* <span data-ttu-id="43cde-139">Szolgáltatások > Naplózás > illesztőprogram > Beállítások</span><span class="sxs-lookup"><span data-stu-id="43cde-139">Services > Logging > Driver > Options</span></span>
* <span data-ttu-id="43cde-140">Kötet & telepítése > kötet</span><span class="sxs-lookup"><span data-stu-id="43cde-140">Volume & Deploy > Volume</span></span>

<span data-ttu-id="43cde-141">A felelősek az erőforrás-korlátok, a fürt beállítása [Service Fabric erőforrás irányítás](service-fabric-resource-governance.md).</span><span class="sxs-lookup"><span data-stu-id="43cde-141">Set up the cluster for enforcing resource limits, as described in [Service Fabric resource governance](service-fabric-resource-governance.md).</span></span> <span data-ttu-id="43cde-142">Minden más Docker Compose irányelvek nem támogatottak az előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="43cde-142">All other Docker Compose directives are unsupported for this preview.</span></span>

## <a name="servicednsname-computation"></a><span data-ttu-id="43cde-143">ServiceDnsName számítási</span><span class="sxs-lookup"><span data-stu-id="43cde-143">ServiceDnsName computation</span></span>

<span data-ttu-id="43cde-144">Ha a szolgáltatás neve meg egy új fájlt a teljes tartománynév (Ez azt jelenti, hogy pontot tartalmaz [.]), a DNS-nevet a Service Fabric által regisztrált `<ServiceName>` (beleértve a pont).</span><span class="sxs-lookup"><span data-stu-id="43cde-144">If the service name that you specify in a Compose file is a fully qualified domain name (that is, it contains a dot [.]), the DNS name registered by Service Fabric is `<ServiceName>` (including the dot).</span></span> <span data-ttu-id="43cde-145">Ha nem, akkor minden elérésiút-szegmens az alkalmazás nevét a tartomány címkét a szolgáltatás DNS-név, az első elérésiút-szegmens, a legfelső szintű tartomány címke váljon a válik.</span><span class="sxs-lookup"><span data-stu-id="43cde-145">If not, each path segment in the application name becomes a domain label in the service DNS name, with the first path segment becoming the top-level domain label.</span></span>

<span data-ttu-id="43cde-146">Ha a megadott alkalmazás neve például `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` lenne regisztrált DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="43cde-146">For example, if the specified application name is `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` would be the registered DNS name.</span></span>

## <a name="differences-between-compose-instance-definition-and-service-fabric-application-model-type-definition"></a><span data-ttu-id="43cde-147">Új (példány-definíció) és a Service Fabric-alkalmazás modell (típusdefinícióban) közötti különbségek</span><span class="sxs-lookup"><span data-stu-id="43cde-147">Differences between Compose (instance definition) and Service Fabric application model (type definition)</span></span>

<span data-ttu-id="43cde-148">Egy docker-compose.yml fájlt a tárolók, beleértve azok tulajdonságait és konfigurációk telepíthető együttesét írja le.</span><span class="sxs-lookup"><span data-stu-id="43cde-148">A docker-compose.yml file describes a deployable set of containers, including their properties and configurations.</span></span>
<span data-ttu-id="43cde-149">Például a fájl tartalmazhat környezeti változókat és a portok.</span><span class="sxs-lookup"><span data-stu-id="43cde-149">For example, the file can contain environment variables and ports.</span></span> <span data-ttu-id="43cde-150">Is üzembe helyezéshez megadott paraméterek, például egy elhelyezési korlátozás, erőforrás-korlátozások és DNS-nevek, megadhatja a docker-compose.yml fájlt.</span><span class="sxs-lookup"><span data-stu-id="43cde-150">You can also specify deployment parameters, such as placement constraints, resource limits, and DNS names, in the docker-compose.yml file.</span></span>

<span data-ttu-id="43cde-151">A [Service Fabric-alkalmazás modell](service-fabric-application-model.md) használ szolgáltatás és alkalmazás típusainak, ahol nincs sok alkalmazáspéldányok ugyanabba a típusba tartozik.</span><span class="sxs-lookup"><span data-stu-id="43cde-151">The [Service Fabric application model](service-fabric-application-model.md) uses service types and application types, where you can have many application instances of the same type.</span></span> <span data-ttu-id="43cde-152">Lehet például egy ügyfél egy alkalmazáspéldányt.</span><span class="sxs-lookup"><span data-stu-id="43cde-152">For example, you can have one application instance per customer.</span></span> <span data-ttu-id="43cde-153">Ez a típus-alapú modell regisztrálva van a futtatókörnyezet az alkalmazás ugyanolyan több verzióit támogatja.</span><span class="sxs-lookup"><span data-stu-id="43cde-153">This type-based model supports multiple versions of the same application type that's registered with the runtime.</span></span>

<span data-ttu-id="43cde-154">Például A felhasználói lehet egy példányként létrehozott típussal 1.0 AppTypeA rendelkező alkalmazást, és B ügyfél rendelkezhet azonos típusú és verzió példányként létrehozott egy másik alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="43cde-154">For example, customer A can have an application instantiated with type 1.0 of AppTypeA, and customer B can have another application instantiated with the same type and version.</span></span> <span data-ttu-id="43cde-155">Alkalmazástípusok az alkalmazásjegyzékeknek határozhat meg, és az alkalmazás létrehozásakor adja meg az alkalmazás nevét és a központi telepítési paramétereit.</span><span class="sxs-lookup"><span data-stu-id="43cde-155">You define the application types in the application manifests, and you specify the application name and deployment parameters when you create the application.</span></span>

<span data-ttu-id="43cde-156">Bár ez a modell rugalmasságot nyújt, azt is tervezi, hogy hol típusok tartoznak, amely implicit a jegyzékfájl egyszerűbb, a példány-alapú telepítési modell támogatja.</span><span class="sxs-lookup"><span data-stu-id="43cde-156">Although this model offers flexibility, we are also planning to support a simpler, instance-based deployment model where types are implicit from the manifest file.</span></span> <span data-ttu-id="43cde-157">Ebben a modellben minden alkalmazás saját független jegyzékfájl lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="43cde-157">In this model, each application gets its own independent manifest.</span></span> <span data-ttu-id="43cde-158">Ebből a törekvésből támogatásának hozzáadásával a docker-compose.yml, ez az egy példány-alapú üzembe helyezési formátum jelenleg előzetes.</span><span class="sxs-lookup"><span data-stu-id="43cde-158">We are previewing this effort by adding support for docker-compose.yml, which is an instance-based deployment format.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43cde-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43cde-159">Next steps</span></span>

* <span data-ttu-id="43cde-160">Olvassa a a [Service Fabric alkalmazásmodellt.](service-fabric-application-model.md)</span><span class="sxs-lookup"><span data-stu-id="43cde-160">Read up on the [Service Fabric application model](service-fabric-application-model.md)</span></span>
* [<span data-ttu-id="43cde-161">A Service Fabric parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="43cde-161">Get started with Service Fabric CLI</span></span>](service-fabric-cli.md)
