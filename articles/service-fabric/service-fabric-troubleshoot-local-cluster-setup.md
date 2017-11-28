---
title: "aaaTroubleshoot a helyi Service Fabric-fürt beállítása |} Microsoft Docs"
description: "Ez a cikk ismerteti a javaslatok az a helyi fejlesztési fürtök készlete"
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 97f4feaa-bba0-47af-8fdd-07f811fe2202
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: ce36f62a4bc69d2cd5b6c3df4abda6ca88fa84f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a><span data-ttu-id="f0eb8-103">A helyi fejlesztési fürtöt való beállításának hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="f0eb8-103">Troubleshoot your local development cluster setup</span></span>
<span data-ttu-id="f0eb8-104">Ha problémát tapasztal az Azure Service Fabric helyi fejlesztési fürtök való interakció során, tekintse át a következő javaslatok a lehetséges megoldásokról hello.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-104">If you run into an issue while interacting with your local Azure Service Fabric development cluster, review hello following suggestions for potential solutions.</span></span>

## <a name="cluster-setup-failures"></a><span data-ttu-id="f0eb8-105">Fürt telepítési hiba</span><span class="sxs-lookup"><span data-stu-id="f0eb8-105">Cluster setup failures</span></span>
### <a name="cannot-clean-up-service-fabric-logs"></a><span data-ttu-id="f0eb8-106">Nem lehet tisztítást végezni a Service Fabric-naplók</span><span class="sxs-lookup"><span data-stu-id="f0eb8-106">Cannot clean up Service Fabric logs</span></span>
#### <a name="problem"></a><span data-ttu-id="f0eb8-107">Probléma</span><span class="sxs-lookup"><span data-stu-id="f0eb8-107">Problem</span></span>
<span data-ttu-id="f0eb8-108">Hello DevClusterSetup parancsprogram futtatása során ehhez hasonló hibaüzenetet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="f0eb8-108">While running hello DevClusterSetup script, you see an error like this:</span></span>

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held tooitems in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a><span data-ttu-id="f0eb8-109">Megoldás</span><span class="sxs-lookup"><span data-stu-id="f0eb8-109">Solution</span></span>
<span data-ttu-id="f0eb8-110">Zárja be hello aktuális PowerShell-ablakot, és nyissa meg rendszergazdaként egy új PowerShell-ablakot.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-110">Close hello current PowerShell window and open a new PowerShell window as an administrator.</span></span> <span data-ttu-id="f0eb8-111">Hello parancsprogrammal képes toosuccessfully kell.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-111">You should now be able toosuccessfully run hello script.</span></span>

## <a name="cluster-connection-failures"></a><span data-ttu-id="f0eb8-112">Fürt kapcsolódási hibák</span><span class="sxs-lookup"><span data-stu-id="f0eb8-112">Cluster connection failures</span></span>
### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a><span data-ttu-id="f0eb8-113">Service Fabric PowerShell-parancsmagok a rendszer nem ismeri fel az Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f0eb8-113">Service Fabric PowerShell cmdlets are not recognized in Azure PowerShell</span></span>
#### <a name="problem"></a><span data-ttu-id="f0eb8-114">Probléma</span><span class="sxs-lookup"><span data-stu-id="f0eb8-114">Problem</span></span>
<span data-ttu-id="f0eb8-115">Ha megpróbál toorun bármelyik hello Service Fabric PowerShell-parancsmagok, például a `Connect-ServiceFabricCluster` egy Azure PowerShell-ablakban, ez nem sikerül, arról, hogy hello parancsmag nem ismerhető fel.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-115">If you try toorun any of hello Service Fabric PowerShell cmdlets, such as `Connect-ServiceFabricCluster` in an Azure PowerShell window, it fails, saying that hello cmdlet is not recognized.</span></span> <span data-ttu-id="f0eb8-116">hello ennek oka, hogy az Azure PowerShell hello 32 bites verzióját használja, a Windows PowerShell (még akkor is, 64 bites operációs rendszer verzió), mivel a Service Fabric parancsmagok hello csak 64 bites környezetben működik.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-116">hello reason for this is that Azure PowerShell uses hello 32-bit version of Windows PowerShell (even on 64-bit OS versions), whereas hello Service Fabric cmdlets only work in 64-bit environments.</span></span>

#### <a name="solution"></a><span data-ttu-id="f0eb8-117">Megoldás</span><span class="sxs-lookup"><span data-stu-id="f0eb8-117">Solution</span></span>
<span data-ttu-id="f0eb8-118">A Service Fabric parancsmagok futtatása mindig közvetlenül a Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-118">Always run Service Fabric cmdlets directly from Windows PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="f0eb8-119">Azure PowerShell legújabb verziójának hello nem hoz létre egy külön helyi, ez már nem jöjjön létre.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-119">hello latest version of Azure PowerShell does not create a special shortcut, so this should no longer occur.</span></span>
> 
> 

### <a name="type-initialization-exception"></a><span data-ttu-id="f0eb8-120">Írja be az inicializálási kivétel</span><span class="sxs-lookup"><span data-stu-id="f0eb8-120">Type Initialization exception</span></span>
#### <a name="problem"></a><span data-ttu-id="f0eb8-121">Probléma</span><span class="sxs-lookup"><span data-stu-id="f0eb8-121">Problem</span></span>
<span data-ttu-id="f0eb8-122">Amikor PowerShell toohello fürthöz csatlakozik, System.Fabric.Common.AppTrace lásd hello hiba TypeInitializationException.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-122">When you are connecting toohello cluster in PowerShell, you see hello error TypeInitializationException for System.Fabric.Common.AppTrace.</span></span>

#### <a name="solution"></a><span data-ttu-id="f0eb8-123">Megoldás</span><span class="sxs-lookup"><span data-stu-id="f0eb8-123">Solution</span></span>
<span data-ttu-id="f0eb8-124">Az elérésiút-változó beállítása helytelen telepítés során.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-124">Your path variable was not correctly set during installation.</span></span> <span data-ttu-id="f0eb8-125">Jelentkeznie a Windowsból, és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-125">Sign out of Windows and sign back in.</span></span> <span data-ttu-id="f0eb8-126">Ez frissíti a elérési utat.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-126">This refreshes your path.</span></span>

### <a name="cluster-connection-fails-with-object-is-closed"></a><span data-ttu-id="f0eb8-127">Fürt létesített kapcsolat megszakad, a "A objektum be van zárva"</span><span class="sxs-lookup"><span data-stu-id="f0eb8-127">Cluster connection fails with "Object is closed"</span></span>
#### <a name="problem"></a><span data-ttu-id="f0eb8-128">Probléma</span><span class="sxs-lookup"><span data-stu-id="f0eb8-128">Problem</span></span>
<span data-ttu-id="f0eb8-129">A hívás tooConnect-ServiceFabricCluster meghiúsul, és ehhez hasonló hibát:</span><span class="sxs-lookup"><span data-stu-id="f0eb8-129">A call tooConnect-ServiceFabricCluster fails with an error like this:</span></span>

    Connect-ServiceFabricCluster : hello object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a><span data-ttu-id="f0eb8-130">Megoldás</span><span class="sxs-lookup"><span data-stu-id="f0eb8-130">Solution</span></span>
<span data-ttu-id="f0eb8-131">Zárja be hello aktuális PowerShell-ablakot, és nyissa meg rendszergazdaként egy új PowerShell-ablakot.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-131">Close hello current PowerShell window and open a new PowerShell window as an administrator.</span></span> <span data-ttu-id="f0eb8-132">Most tudnia kell csatlakozni toosuccessfully.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-132">You should now be able toosuccessfully connect.</span></span>

### <a name="fabric-connection-denied-exception"></a><span data-ttu-id="f0eb8-133">Háló kapcsolat megtagadva kivétel</span><span class="sxs-lookup"><span data-stu-id="f0eb8-133">Fabric Connection Denied exception</span></span>
#### <a name="problem"></a><span data-ttu-id="f0eb8-134">Probléma</span><span class="sxs-lookup"><span data-stu-id="f0eb8-134">Problem</span></span>
<span data-ttu-id="f0eb8-135">Hibakereséshez a következőből: Visual Studio, a FabricConnectionDeniedException hiba nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-135">When debugging from Visual Studio, you get a FabricConnectionDeniedException error.</span></span>

#### <a name="solution"></a><span data-ttu-id="f0eb8-136">Megoldás</span><span class="sxs-lookup"><span data-stu-id="f0eb8-136">Solution</span></span>
<span data-ttu-id="f0eb8-137">Ez a hiba általában akkor történik meg a szolgáltatás gazdafolyamatokon toostart manuálisan, ahelyett, hogy így hello Service Fabric futásidejű toostart azt meg.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-137">This error usually occurs when you try toostart a service host process manually, rather than allowing hello Service Fabric runtime toostart it for you.</span></span>

<span data-ttu-id="f0eb8-138">Győződjön meg arról, hogy nem kell minden service projektek állítja be kiindulási projektet a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-138">Ensure that you do not have any service projects set as startup projects in your solution.</span></span> <span data-ttu-id="f0eb8-139">Csak a Service Fabric application projektek indítási projektként kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-139">Only Service Fabric application projects should be set as startup projects.</span></span>

> [!TIP]
> <span data-ttu-id="f0eb8-140">Ha a telepítés után a helyi fürt toobehave rendellenesen kezdődik, visszaállíthatja rajta a hello helyi fürt manager rendszer tálca alkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-140">If, following setup, your local cluster begins toobehave abnormally, you can reset it using hello local cluster manager system tray application.</span></span> <span data-ttu-id="f0eb8-141">Ez eltávolítja a meglévő fürt hello, és állítson be egy új.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-141">This removes hello existing cluster and set up a new one.</span></span> <span data-ttu-id="f0eb8-142">Vegye figyelembe, hogy a telepített alkalmazások és a kapcsolódó adatokat távolítja el.</span><span class="sxs-lookup"><span data-stu-id="f0eb8-142">Note that all deployed applications and associated data is removed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="f0eb8-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0eb8-143">Next steps</span></span>
* [<span data-ttu-id="f0eb8-144">Ismertetése és hibaelhárítása a fürt rendszerállapot-jelentések</span><span class="sxs-lookup"><span data-stu-id="f0eb8-144">Understand and troubleshoot your cluster with system health reports</span></span>](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [<span data-ttu-id="f0eb8-145">A fürt megjelenítése a Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="f0eb8-145">Visualize your cluster with Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)

