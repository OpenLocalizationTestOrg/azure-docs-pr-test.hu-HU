---
title: "Alkalmazás frissítése témakörök aaaAdvanced |} Microsoft Docs"
description: "Ez a cikk a Service Fabric-alkalmazás tooupgrading vonatkozó speciális témaköröket ismerteti."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar;chackdan
ms.openlocfilehash: bdaf3db6209c574d39f57e0bf9951fad5ad1cbec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a><span data-ttu-id="8c7e3-103">A Service Fabric az alkalmazásfrissítés: speciális kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="8c7e3-103">Service Fabric application upgrade: advanced topics</span></span>
## <a name="adding-or-removing-services-during-an-application-upgrade"></a><span data-ttu-id="8c7e3-104">Hozzáadásával vagy eltávolításával a szolgáltatások egy alkalmazás frissítése során</span><span class="sxs-lookup"><span data-stu-id="8c7e3-104">Adding or removing services during an application upgrade</span></span>
<span data-ttu-id="8c7e3-105">Ha egy új szolgáltatás hozzáadása tooan alkalmazás már telepítve, és közzétett frissítéseként hello új szolgáltatás hozzáadott toohello telepített alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-105">If a new service is added tooan application that is already deployed, and published as an upgrade, hello new service is added toohello deployed application.</span></span>  <span data-ttu-id="8c7e3-106">Ilyen frissítés hello szolgáltatások hello alkalmazás már nincs hatással.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-106">Such an upgrade does not affect any of hello services that were already part of hello application.</span></span> <span data-ttu-id="8c7e3-107">Azonban felvett hello szolgáltatás egy példánya el kell indítani a hello új szolgáltatás toobe aktív (hello segítségével `New-ServiceFabricService` parancsmag).</span><span class="sxs-lookup"><span data-stu-id="8c7e3-107">However, an instance of hello service that was added must be started for hello new service toobe active (using hello `New-ServiceFabricService` cmdlet).</span></span>

<span data-ttu-id="8c7e3-108">Szolgáltatások frissítés részeként-alkalmazásból is eltávolítható lesz.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-108">Services can also be removed from an application as part of an upgrade.</span></span> <span data-ttu-id="8c7e3-109">Azonban hello-to-be törölt szolgáltatás minden aktuális példányának hello frissítés folytatása előtt le kell állítani (hello segítségével `Remove-ServiceFabricService` parancsmag).</span><span class="sxs-lookup"><span data-stu-id="8c7e3-109">However, all current instances of hello to-be-deleted service must be stopped before proceeding with hello upgrade (using hello `Remove-ServiceFabricService` cmdlet).</span></span>

## <a name="manual-upgrade-mode"></a><span data-ttu-id="8c7e3-110">Manuális frissítési módra</span><span class="sxs-lookup"><span data-stu-id="8c7e3-110">Manual upgrade mode</span></span>
> [!NOTE]
> <span data-ttu-id="8c7e3-111">hello nem figyelt manuális mód csak a sikertelen vagy felfüggesztett frissítés figyelembe kell venni.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-111">hello unmonitored manual mode should be considered only for a failed or suspended upgrade.</span></span> <span data-ttu-id="8c7e3-112">hello figyelt módja hello ajánlott frissítési mód a Service Fabric-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-112">hello monitored mode is hello recommended upgrade mode for Service Fabric applications.</span></span>
>
>

<span data-ttu-id="8c7e3-113">Az Azure Service Fabric biztosít több frissítési módok toosupport fejlesztési és éles fürt.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-113">Azure Service Fabric provides multiple upgrade modes toosupport development and production clusters.</span></span> <span data-ttu-id="8c7e3-114">A kiválasztott központi telepítési beállítások különböző környezetek eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-114">Deployment options chosen may be different for different environments.</span></span>

<span data-ttu-id="8c7e3-115">figyelt hello működés közbeni alkalmazásfrissítés hello legjellemzőbb frissítési toouse hello éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-115">hello monitored rolling application upgrade is hello most typical upgrade toouse in hello production environment.</span></span> <span data-ttu-id="8c7e3-116">Hello frissítésekor házirend van megadva, a Service Fabric biztosítja, hogy hello alkalmazás kifogástalan előtt hello frissítés előrehalad.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-116">When hello upgrade policy is specified, Service Fabric ensures that hello application is healthy before hello upgrade proceeds.</span></span>

 <span data-ttu-id="8c7e3-117">hello alkalmazás-rendszergazda használhat működés közbeni alkalmazás frissítési mód toohave teljes szabályozhatják hello frissítési folyamat állapota hello keresztül különböző frissítési tartományok hello manuális.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-117">hello application administrator can use hello manual rolling application upgrade mode toohave total control over hello upgrade progress through hello various upgrade domains.</span></span> <span data-ttu-id="8c7e3-118">Ebben a módban akkor hasznos, ha testreszabott vagy összetett értékelési állapotházirend szükség, vagy a nem hagyományos frissítés történik (például hello alkalmazás már szerepel az adatveszteség).</span><span class="sxs-lookup"><span data-stu-id="8c7e3-118">This mode is useful when a customized or complex health evaluation policy is required, or a non-conventional upgrade is happening (for example, hello application is already in data loss).</span></span>

<span data-ttu-id="8c7e3-119">Végezetül hello alkalmazás automatikus működés közbeni frissítés esetén hasznos fejlesztési vagy tesztelési környezetben tooprovide gyors iterációs ciklus szolgáltatás fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-119">Finally, hello automated rolling application upgrade is useful for development or testing environments tooprovide a fast iteration cycle during service development.</span></span>

## <a name="change-toomanual-upgrade-mode"></a><span data-ttu-id="8c7e3-120">Toomanual frissítési módjának módosítása</span><span class="sxs-lookup"><span data-stu-id="8c7e3-120">Change toomanual upgrade mode</span></span>
<span data-ttu-id="8c7e3-121">**Manuális**--Stop hello alkalmazás frissítést, a hello aktuális UD és módosítási hello mód tooUnmonitored manuális frissítése.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-121">**Manual**--Stop hello application upgrade at hello current UD and change hello upgrade mode tooUnmonitored Manual.</span></span> <span data-ttu-id="8c7e3-122">hello rendszergazdának kell toomanually hívás **MoveNextApplicationUpgradeDomainAsync** tooproceed hello a frissítése vagy indul el, a visszaállítás elindítása az új frissítés által.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-122">hello administrator needs toomanually call **MoveNextApplicationUpgradeDomainAsync** tooproceed with hello upgrade or trigger a rollback by initiating a new upgrade.</span></span> <span data-ttu-id="8c7e3-123">Miután hello frissítés hello manuális módba lép, marad hello manuális üzemmódban addig, amíg az új frissítést lehet kezdeményezni.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-123">Once hello upgrade enters into hello Manual mode, it stays in hello Manual mode until a new upgrade is initiated.</span></span> <span data-ttu-id="8c7e3-124">Hello **GetApplicationUpgradeProgressAsync** parancs visszaadja a HÁLÓ\_alkalmazás\_frissítése\_állapot\_működés közbeni\_előre\_FÜGGŐBEN.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-124">hello **GetApplicationUpgradeProgressAsync** command returns FABRIC\_APPLICATION\_UPGRADE\_STATE\_ROLLING\_FORWARD\_PENDING.</span></span>

## <a name="upgrade-with-a-diff-package"></a><span data-ttu-id="8c7e3-125">A diff csomag frissítése</span><span class="sxs-lookup"><span data-stu-id="8c7e3-125">Upgrade with a diff package</span></span>
<span data-ttu-id="8c7e3-126">A Service Fabric-alkalmazás tudja frissíteni a teljes, önállóan alkalmazási csomaggal rendelkező kiépítés.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-126">A Service Fabric application can be upgraded by provisioning with a full, self-contained application package.</span></span> <span data-ttu-id="8c7e3-127">Egy alkalmazás, amely tartalmazza a frissített hello alkalmazásfájlok csak különbözeti csomag használatával is frissítése, hello alkalmazásjegyzék és hello service manifest-fájlok frissítése.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-127">An application can also be upgraded by using a diff package that contains only hello updated application files, hello updated application manifest, and hello service manifest files.</span></span>

<span data-ttu-id="8c7e3-128">A teljes alkalmazáscsomag szükséges toostart összes hello fájlokat tartalmazza, és futtassa a Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-128">A full application package contains all hello files necessary toostart and run a Service Fabric application.</span></span> <span data-ttu-id="8c7e3-129">Diff csomag csak a módosított hello utolsó kiépítés és hello aktuális frissítés között hello fájlokat tartalmazza, valamint hello teljes alkalmazás jegyzékfájlja és hello service manifest fájlt.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-129">A diff package contains only hello files that changed between hello last provision and hello current upgrade, plus hello full application manifest and hello service manifest files.</span></span> <span data-ttu-id="8c7e3-130">Összes hivatkozás hello alkalmazásjegyzék vagy szolgáltatás jegyzékfájl, amely nem található a hello build elrendezés hello lemezképtárolóhoz kell keresni.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-130">Any reference in hello application manifest or service manifest that can't be found in hello build layout is searched for in hello image store.</span></span>

<span data-ttu-id="8c7e3-131">Teljes alkalmazáscsomagok olyan alkalmazások toohello fürt hello első telepítéshez szükséges.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-131">Full application packages are required for hello first installation of an application toohello cluster.</span></span> <span data-ttu-id="8c7e3-132">Soron következő frissítések a teljes alkalmazáscsomagok vagy különbözeti csomag is lehet.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-132">Subsequent updates can be either a full application package or a diff package.</span></span>

<span data-ttu-id="8c7e3-133">Ha különbözeti csomagot lenne a legjobb megoldás alkalommal:</span><span class="sxs-lookup"><span data-stu-id="8c7e3-133">Occasions when using a diff package would be a good choice:</span></span>

* <span data-ttu-id="8c7e3-134">Különbözeti csomag használata ajánlott, ha a nagy alkalmazáscsomagok, amelyek több service manifest fájl és/vagy több kód csomagok, konfigurációs csomagokat vagy adatok csomagok hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-134">A diff package is preferred when you have a large application package that references several service manifest files and/or several code packages, config packages, or data packages.</span></span>
* <span data-ttu-id="8c7e3-135">Diff csomag részesíti előnyben, amikor a központi telepítési rendszer közvetlenül a az alkalmazás létrehozási folyamata hello build elrendezés generáló.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-135">A diff package is preferred when you have a deployment system that generates hello build layout directly from your application build process.</span></span> <span data-ttu-id="8c7e3-136">Ebben az esetben annak ellenére, hogy hello kódja nem változott, újonnan beépített szerelvények egy másik ellenőrzőösszeg beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-136">In this case, even though hello code hasn't changed, newly built assemblies get a different checksum.</span></span> <span data-ttu-id="8c7e3-137">A teljes alkalmazás csomag használata, tooupdate hello verzió összes kódot csomag kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-137">Using a full application package would require you tooupdate hello version on all code packages.</span></span> <span data-ttu-id="8c7e3-138">Diff csomagot használ, csak megadja, hogy módosultak a hello fájlok és hello jegyzékfájlt Ha hello verziója megváltozott.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-138">Using a diff package, you only provide hello files that changed and hello manifest files where hello version has changed.</span></span>

<span data-ttu-id="8c7e3-139">Ha egy alkalmazás frissítve van, a Visual Studio használatával, hello különbözeti csomag automatikusan van közzétéve.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-139">When an application is upgraded using Visual Studio, hello diff package is published automatically.</span></span> <span data-ttu-id="8c7e3-140">diff csomag toocreate manuálisan, hello alkalmazásjegyzék, és hello szolgáltatás jegyzéknek meg kell adni, de csak a módosított hello csomagok végső alkalmazáscsomag hello fel kell venni.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-140">toocreate a diff package manually, hello application manifest, and hello service manifests must be updated, but only hello changed packages should be included in hello final application package.</span></span>

<span data-ttu-id="8c7e3-141">Például kezdjük hello (verziószámok ismertetése könnyű előírt) alkalmazás a következő:</span><span class="sxs-lookup"><span data-stu-id="8c7e3-141">For example, let's start with hello following application (version numbers provided for ease of understanding):</span></span>

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

<span data-ttu-id="8c7e3-142">Most tegyük fel a keresett tooupdate csak hello kódcsomag service1 a PowerShell használatával különbözeti csomagot.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-142">Now, let's assume you wanted tooupdate only hello code package of service1 using a diff package using PowerShell.</span></span> <span data-ttu-id="8c7e3-143">A frissített alkalmazás már hello mappastruktúra a következő:</span><span class="sxs-lookup"><span data-stu-id="8c7e3-143">Now, your updated application has hello following folder structure:</span></span>

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

<span data-ttu-id="8c7e3-144">Ebben az esetben frissítenie hello application manifest too2.0.0 és hello szolgáltatás jegyzékfájljának service1 tooreflect hello csomag frissítése.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-144">In this case, you update hello application manifest too2.0.0, and hello service manifest for service1 tooreflect hello code package update.</span></span> <span data-ttu-id="8c7e3-145">az alkalmazáscsomag hello mappa hello struktúra a következő lenne:</span><span class="sxs-lookup"><span data-stu-id="8c7e3-145">hello folder for your application package would have hello following structure:</span></span>

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a><span data-ttu-id="8c7e3-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8c7e3-146">Next steps</span></span>
<span data-ttu-id="8c7e3-147">[Az alkalmazás használata a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti Önt az alkalmazásfrissítés Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-147">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>

<span data-ttu-id="8c7e3-148">[Az alkalmazás használatával Powershell frissítése](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti Önt az alkalmazásfrissítés PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="8c7e3-148">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>

<span data-ttu-id="8c7e3-149">Szabályozhatja, hogy az alkalmazás használatával frissíti [frissítése paraméterek](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="8c7e3-149">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>

<span data-ttu-id="8c7e3-150">Az alkalmazásfrissítéseket Learning kompatibilissé hogyan toouse [Adatszerializálás](service-fabric-application-upgrade-data-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="8c7e3-150">Make your application upgrades compatible by learning how toouse [Data Serialization](service-fabric-application-upgrade-data-serialization.md).</span></span>

<span data-ttu-id="8c7e3-151">Javítsa ki a gyakori problémákat alkalmazásfrissítések toohello lépéseit hivatkozással [Alkalmazásfrissítések hibaelhárítási](service-fabric-application-upgrade-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="8c7e3-151">Fix common problems in application upgrades by referring toohello steps in [Troubleshooting Application Upgrades](service-fabric-application-upgrade-troubleshooting.md).</span></span>
