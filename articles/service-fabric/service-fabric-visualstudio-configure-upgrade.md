---
title: "a Service Fabric-alkalmazás aaaConfigure hello frissítése |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure hello Microsoft Visual Studio használatával a Service Fabric-alkalmazás frissítésére vonatkozó beállításokat."
services: service-fabric
documentationcenter: na
author: mikkelhegn
manager: mfussell
editor: tglee
ms.assetid: 1757ba85-0b7b-4f16-8a23-2ddaa61c86c6
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/29/2017
ms.author: mikkelhegn
ms.openlocfilehash: 8ca50aa9d911f3c98f017490c8fe29011e8d80cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-upgrade-of-a-service-fabric-application-in-visual-studio"></a><span data-ttu-id="c5d62-103">A Service Fabric-alkalmazás hello frissítését a Visual Studio konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c5d62-103">Configure hello upgrade of a Service Fabric application in Visual Studio</span></span>
<span data-ttu-id="c5d62-104">A Visual Studio eszközök Azure Service Fabric frissítési támogatást nyújt a toolocal vagy távoli fürtök.</span><span class="sxs-lookup"><span data-stu-id="c5d62-104">Visual Studio tools for Azure Service Fabric provide upgrade support for publishing toolocal or remote clusters.</span></span> <span data-ttu-id="c5d62-105">Nincsenek használni kívánt tooupgrade az alkalmazás tooa újabb verziójával hello alkalmazás tesztelés és hibakeresés során felülírása helyett három forgatókönyv:</span><span class="sxs-lookup"><span data-stu-id="c5d62-105">There are three scenarios in which you want tooupgrade your application tooa newer version instead of replacing hello application during testing and debugging:</span></span>

* <span data-ttu-id="c5d62-106">Alkalmazásadatok nem fognak veszni hello frissítés során.</span><span class="sxs-lookup"><span data-stu-id="c5d62-106">Application data won't be lost during hello upgrade.</span></span>
* <span data-ttu-id="c5d62-107">Rendelkezésre állási marad magas bármely szolgáltatáskiesést hello a frissítés során nem lesz, ha nincsenek a frissítési tartományok elosztva elég szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="c5d62-107">Availability remains high so there won't be any service interruption during hello upgrade, if there are enough service instances spread across upgrade domains.</span></span>
* <span data-ttu-id="c5d62-108">Tesztek futtatható az alkalmazáshoz, amíg a frissítés alatt áll.</span><span class="sxs-lookup"><span data-stu-id="c5d62-108">Tests can be run against an application while it's being upgraded.</span></span>

## <a name="parameters-needed-tooupgrade"></a><span data-ttu-id="c5d62-109">Paraméterek szükséges tooupgrade</span><span class="sxs-lookup"><span data-stu-id="c5d62-109">Parameters needed tooupgrade</span></span>
<span data-ttu-id="c5d62-110">Kétféle típusú központi telepítés közül választhat: rendszeres vagy frissítés.</span><span class="sxs-lookup"><span data-stu-id="c5d62-110">You can choose from two types of deployment: regular or upgrade.</span></span> <span data-ttu-id="c5d62-111">A szokásos telepítési töröl bármely korábbi központi telepítési információk és adatok hello fürtön során egy frissítés telepítése megőrzi azt.</span><span class="sxs-lookup"><span data-stu-id="c5d62-111">A regular deployment erases any previous deployment information and data on hello cluster, while an upgrade deployment preserves it.</span></span> <span data-ttu-id="c5d62-112">A Service Fabric-alkalmazás, a Visual Studio frissít, meg kell tooprovide alkalmazás frissítési paraméterek és az állapotházirendeket ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="c5d62-112">When you upgrade a Service Fabric application in Visual Studio, you need tooprovide application upgrade parameters and health check policies.</span></span> <span data-ttu-id="c5d62-113">Az alkalmazásfrissítés paraméterek segítségével hello frissítését, szabályozhatja, amíg állapotházirendeket jelölőnégyzet határozza meg, hogy hello frissítése sikeres volt-e.</span><span class="sxs-lookup"><span data-stu-id="c5d62-113">Application upgrade parameters help control hello upgrade, while health check policies determine whether hello upgrade was successful.</span></span> <span data-ttu-id="c5d62-114">Lásd: [Service Fabric az alkalmazásfrissítés: frissítési paraméterek](service-fabric-application-upgrade-parameters.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="c5d62-114">See [Service Fabric application upgrade: upgrade parameters](service-fabric-application-upgrade-parameters.md) for more details.</span></span>

<span data-ttu-id="c5d62-115">Három frissítési módot: *figyelt*, *UnmonitoredAuto*, és *UnmonitoredManual*.</span><span class="sxs-lookup"><span data-stu-id="c5d62-115">There are three upgrade modes: *Monitored*, *UnmonitoredAuto*, and *UnmonitoredManual*.</span></span>

* <span data-ttu-id="c5d62-116">A figyelt frissítés hello frissítés és alkalmazáshoz automatizálja az állapot-ellenőrzéssel.</span><span class="sxs-lookup"><span data-stu-id="c5d62-116">A Monitored upgrade automates hello upgrade and application health check.</span></span>
* <span data-ttu-id="c5d62-117">UnmonitoredAuto frissítés automatizálja hello frissítését, de átugrása hello alkalmazás állapot-ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="c5d62-117">An UnmonitoredAuto upgrade automates hello upgrade, but skips hello application health check.</span></span>
* <span data-ttu-id="c5d62-118">Ekkor-UnmonitoredManual frissítés toomanually kell frissíteni mindegyik frissítési tartományon.</span><span class="sxs-lookup"><span data-stu-id="c5d62-118">When you do an UnmonitoredManual upgrade, you need toomanually upgrade each upgrade domain.</span></span>

<span data-ttu-id="c5d62-119">Minden egyes frissítési módhoz paraméterek más-más részhalmazához.</span><span class="sxs-lookup"><span data-stu-id="c5d62-119">Each upgrade mode requires different sets of parameters.</span></span> <span data-ttu-id="c5d62-120">Lásd: [alkalmazás frissítési paraméterei](service-fabric-application-upgrade-parameters.md) hello érhető el frissítési beállításokkal kapcsolatos további toolearn.</span><span class="sxs-lookup"><span data-stu-id="c5d62-120">See [Application upgrade parameters](service-fabric-application-upgrade-parameters.md) toolearn more about hello available upgrade options.</span></span>

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a><span data-ttu-id="c5d62-121">A Service Fabric-alkalmazás, a Visual Studio frissítése</span><span class="sxs-lookup"><span data-stu-id="c5d62-121">Upgrade a Service Fabric application in Visual Studio</span></span>
<span data-ttu-id="c5d62-122">Hello Visual Studio Service Fabric eszközök tooupgrade a Service Fabric-alkalmazás használata, megadhatja a közzétételi folyamat toobe frissítés helyett a szokásos telepítési hello ellenőrzésével **hello alkalmazás frissítése** ellenőrzése mezőbe.</span><span class="sxs-lookup"><span data-stu-id="c5d62-122">If you’re using hello Visual Studio Service Fabric tools tooupgrade a Service Fabric application, you can specify a publish process toobe an upgrade rather than a regular deployment by checking hello **Upgrade hello application** check box.</span></span>

### <a name="tooconfigure-hello-upgrade-parameters"></a><span data-ttu-id="c5d62-123">tooconfigure hello frissítési paraméterek</span><span class="sxs-lookup"><span data-stu-id="c5d62-123">tooconfigure hello upgrade parameters</span></span>
1. <span data-ttu-id="c5d62-124">Kattintson a hello **beállítások** gomb következő toohello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="c5d62-124">Click hello **Settings** button next toohello check box.</span></span> <span data-ttu-id="c5d62-125">Hello **frissítése paraméterek szerkesztése** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c5d62-125">hello **Edit Upgrade Parameters** dialog box appears.</span></span> <span data-ttu-id="c5d62-126">Hello **frissítése paraméterek szerkesztése** párbeszédpanel hello figyelt UnmonitoredAuto és UnmonitoredManual frissítési módot támogat.</span><span class="sxs-lookup"><span data-stu-id="c5d62-126">hello **Edit Upgrade Parameters** dialog box supports hello Monitored, UnmonitoredAuto, and UnmonitoredManual upgrade modes.</span></span>
2. <span data-ttu-id="c5d62-127">Válassza ki a frissítési mód hello toouse szeretne, és ezután adja meg a hello paraméter rács.</span><span class="sxs-lookup"><span data-stu-id="c5d62-127">Select hello upgrade mode that you want toouse and then fill out hello parameter grid.</span></span>

    <span data-ttu-id="c5d62-128">Minden paraméter alapértelmezett értéke van.</span><span class="sxs-lookup"><span data-stu-id="c5d62-128">Each parameter has default values.</span></span> <span data-ttu-id="c5d62-129">nem kötelező paraméter hello *DefaultServiceTypeHealthPolicy* egy kivonatoló tábla bemenetből fogad adatokat.</span><span class="sxs-lookup"><span data-stu-id="c5d62-129">hello optional parameter *DefaultServiceTypeHealthPolicy* takes a hash table input.</span></span> <span data-ttu-id="c5d62-130">Példa hello kivonatoló tábla bemeneti formátumának *DefaultServiceTypeHealthPolicy*:</span><span class="sxs-lookup"><span data-stu-id="c5d62-130">Here’s an example of hello hash table input format for *DefaultServiceTypeHealthPolicy*:</span></span>

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    <span data-ttu-id="c5d62-131">*Servicetypehealthpolicymap paraméterek hiányzó értékei* van egy másik nem kötelező paraméter, amely egy kivonatoló tábla bemenetből fogad adatokat hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="c5d62-131">*ServiceTypeHealthPolicyMap* is another optional parameter that takes a hash table input in hello following format:</span></span>

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    <span data-ttu-id="c5d62-132">Íme egy valós példa:</span><span class="sxs-lookup"><span data-stu-id="c5d62-132">Here's a real-life example:</span></span>

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. <span data-ttu-id="c5d62-133">Ha UnmonitoredManual frissítési módot választja, akkor manuálisan indítsa el a PowerShell-konzol toocontinue és hello frissítési folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="c5d62-133">If you select UnmonitoredManual upgrade mode, you must manually start a PowerShell console toocontinue and finish hello upgrade process.</span></span> <span data-ttu-id="c5d62-134">Tekintse meg a túl[Service Fabric az alkalmazásfrissítés: témakörök speciális](service-fabric-application-upgrade-advanced.md) toolearn hogyan kézi frissítés működik.</span><span class="sxs-lookup"><span data-stu-id="c5d62-134">Refer too[Service Fabric application upgrade: advanced topics](service-fabric-application-upgrade-advanced.md) toolearn how manual upgrade works.</span></span>

## <a name="upgrade-an-application-by-using-powershell"></a><span data-ttu-id="c5d62-135">Alkalmazások frissítése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="c5d62-135">Upgrade an application by using PowerShell</span></span>
<span data-ttu-id="c5d62-136">Használhatja a PowerShell-parancsmagok tooupgrade a Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c5d62-136">You can use PowerShell cmdlets tooupgrade a Service Fabric application.</span></span> <span data-ttu-id="c5d62-137">Lásd: [Service Fabric-alkalmazás frissítési oktatóanyag](service-fabric-application-upgrade-tutorial.md) és [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) részletes információkat.</span><span class="sxs-lookup"><span data-stu-id="c5d62-137">See [Service Fabric application upgrade tutorial](service-fabric-application-upgrade-tutorial.md) and [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) for detailed information.</span></span>

## <a name="specify-a-health-check-policy-in-hello-application-manifest-file"></a><span data-ttu-id="c5d62-138">Adja meg a jelölőnégyzet állapotházirend hello Alkalmazásjegyzék-fájl</span><span class="sxs-lookup"><span data-stu-id="c5d62-138">Specify a health check policy in hello application manifest file</span></span>
<span data-ttu-id="c5d62-139">Minden szolgáltatás a Service Fabric-alkalmazás rendelkezhet saját hello az alapértelmezett érték felülírására állapotfigyelő házirend paramétereket.</span><span class="sxs-lookup"><span data-stu-id="c5d62-139">Every service in a Service Fabric application can have its own health policy parameters that override hello default values.</span></span> <span data-ttu-id="c5d62-140">A paraméterértékek hello alkalmazás-jegyzékfájl biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="c5d62-140">You can provide these parameter values in hello application manifest file.</span></span>

<span data-ttu-id="c5d62-141">hello következő példa bemutatja, hogyan tooapply egyedi állapotát ellenőrzi az egyes szolgáltatásokhoz hello alkalmazásjegyzékben házirend.</span><span class="sxs-lookup"><span data-stu-id="c5d62-141">hello following example shows how tooapply a unique health check policy for each service in hello application manifest.</span></span>

```xml
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a><span data-ttu-id="c5d62-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5d62-142">Next steps</span></span>
<span data-ttu-id="c5d62-143">Egy alkalmazás központi telepítésével kapcsolatos további információkért lásd: [telepítsen egy meglévő alkalmazást az Azure Service Fabric](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="c5d62-143">For more information about deploying an application, see [Deploy an existing application in Azure Service Fabric](service-fabric-deploy-existing-app.md).</span></span>