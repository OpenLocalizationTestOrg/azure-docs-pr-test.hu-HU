---
title: "a Service Fabric aaaAzure különbségek attól függnek, Linux és a Windows között |} Microsoft Docs"
description: "Hello Azure Service Fabric előnézete a Linux és a Windows Azure Service Fabric közötti különbségeket."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 7a16a440dfc8d9006e274f46951be1562e6f10d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a><span data-ttu-id="f70f7-103">A Service Fabric Linux (előzetes verziójú) és Windows (általánosan elérhető) rendszerhez készült verziója közötti különbségek</span><span class="sxs-lookup"><span data-stu-id="f70f7-103">Differences between Service Fabric on Linux (preview) and Windows (generally available)</span></span>

<span data-ttu-id="f70f7-104">Mivel a Service Fabric Linux rendszeren még előzetes verzióban van, ezért néhány szolgáltatás csak Windows rendszeren támogatott, Linuxon nem.</span><span class="sxs-lookup"><span data-stu-id="f70f7-104">Since Service Fabric on Linux is a preview, there are some features that are supported on Windows, but not yet on Linux.</span></span> <span data-ttu-id="f70f7-105">Végül hello szolgáltatáskészletek lesz paritásos amikor Linux Service Fabric általánosan elérhetővé válik.</span><span class="sxs-lookup"><span data-stu-id="f70f7-105">Eventually, hello feature sets will be at parity when Service Fabric on Linux becomes generally available.</span></span> <span data-ttu-id="f70f7-106">A jövőbeli kiadásokban a funkciók eltérései egyre csekélyebbé válnak.</span><span class="sxs-lookup"><span data-stu-id="f70f7-106">With upcoming releases, this feature gap will shrink.</span></span> <span data-ttu-id="f70f7-107">hello következő különbségek vannak hello legújabb elérhető kiadásai között (Ez azt jelenti, hogy verziója 5.6 Windows és Linux 5.5-ös verzió):</span><span class="sxs-lookup"><span data-stu-id="f70f7-107">hello following differences exist between hello latest available releases (that is, between version 5.6 on Windows and version 5.5 on Linux):</span></span> 

* <span data-ttu-id="f70f7-108">Reliable Collections (és a Reliable Stateful Services)</span><span class="sxs-lookup"><span data-stu-id="f70f7-108">Reliable Collections (and Reliable Stateful Services)</span></span> 
* <span data-ttu-id="f70f7-109">ReverseProxy</span><span class="sxs-lookup"><span data-stu-id="f70f7-109">ReverseProxy</span></span> 
* <span data-ttu-id="f70f7-110">Önálló telepítő</span><span class="sxs-lookup"><span data-stu-id="f70f7-110">Standalone installer</span></span> 
* <span data-ttu-id="f70f7-111">Jegyzékfájlok XML-sémaérvényesítése</span><span class="sxs-lookup"><span data-stu-id="f70f7-111">XML schema validation for manifest files</span></span> 
* <span data-ttu-id="f70f7-112">Konzol-átirányítás</span><span class="sxs-lookup"><span data-stu-id="f70f7-112">Console redirection</span></span> 
* <span data-ttu-id="f70f7-113">hello tartalék Analysis Service (eszközök)</span><span class="sxs-lookup"><span data-stu-id="f70f7-113">hello Fault Analysis Service (FAS)</span></span>
* <span data-ttu-id="f70f7-114">Docker compose, kötet- és naplózási illesztők tárolókhoz</span><span class="sxs-lookup"><span data-stu-id="f70f7-114">Docker compose and volume and logging drivers for containers</span></span> 
* <span data-ttu-id="f70f7-115">Tárolók és szolgáltatások erőforrás-szabályozása</span><span class="sxs-lookup"><span data-stu-id="f70f7-115">Resource governance for containers and services</span></span> 
* <span data-ttu-id="f70f7-116">DNS-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f70f7-116">DNS service</span></span>
* <span data-ttu-id="f70f7-117">Azure Active Directory-támogatás</span><span class="sxs-lookup"><span data-stu-id="f70f7-117">Azure Active Directory support</span></span>
* <span data-ttu-id="f70f7-118">Egyes PowerShell-parancsok parancssori felületi megfelelője</span><span class="sxs-lookup"><span data-stu-id="f70f7-118">CLI command equivalents of certain Powershell commands</span></span> 
* <span data-ttu-id="f70f7-119">Powershell-parancsokat csak egy részét is futtathatók a Linux-fürt (a kibontott hello a következő szakaszban).</span><span class="sxs-lookup"><span data-stu-id="f70f7-119">Only a subset of Powershell commands can be run against a Linux cluster (as expanded in hello next section).</span></span>

>[!NOTE]
><span data-ttu-id="f70f7-120">A konzolátirányítás nem támogatott éles fürtökben, még Windows rendszeren sem.</span><span class="sxs-lookup"><span data-stu-id="f70f7-120">Console redirection is not supported in production clusters, even on Windows.</span></span>

<span data-ttu-id="f70f7-121">A fejlesztői eszközök eltérnek Windows és Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="f70f7-121">Development tooling is also different between Windows and Linux.</span></span> <span data-ttu-id="f70f7-122">Windows rendszeren a VisualStudio, a PowerShell, a VSTS és az ETW, míg Linuxon a Yeoman, az Eclipse, a Jenkins és az LTTng érhető el.</span><span class="sxs-lookup"><span data-stu-id="f70f7-122">VisualStudio, Powershell, VSTS, and ETW are used on Windows while Yeoman, Eclipse, Jenkins, and LTTng are used on Linux.</span></span>

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a><span data-ttu-id="f70f7-123">PowerShell-parancsmagok, amelyek nem működnek Linux rendszerű Service Fabric-fürtökön</span><span class="sxs-lookup"><span data-stu-id="f70f7-123">Powershell cmdlets that do not work against a Linux Service Fabric cluster</span></span>

* <span data-ttu-id="f70f7-124">Invoke-ServiceFabricChaosTestScenario</span><span class="sxs-lookup"><span data-stu-id="f70f7-124">Invoke-ServiceFabricChaosTestScenario</span></span>
* <span data-ttu-id="f70f7-125">Invoke-ServiceFabricFailoverTestScenario</span><span class="sxs-lookup"><span data-stu-id="f70f7-125">Invoke-ServiceFabricFailoverTestScenario</span></span>
* <span data-ttu-id="f70f7-126">Invoke-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="f70f7-126">Invoke-ServiceFabricPartitionDataLoss</span></span>
* <span data-ttu-id="f70f7-127">Invoke-ServiceFabricPartitionQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="f70f7-127">Invoke-ServiceFabricPartitionQuorumLoss</span></span>
* <span data-ttu-id="f70f7-128">Restart-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="f70f7-128">Restart-ServiceFabricPartition</span></span>
* <span data-ttu-id="f70f7-129">Start-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="f70f7-129">Start-ServiceFabricNode</span></span>
* <span data-ttu-id="f70f7-130">Stop-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="f70f7-130">Stop-ServiceFabricNode</span></span>
* <span data-ttu-id="f70f7-131">Get-ServiceFabricImageStoreContent</span><span class="sxs-lookup"><span data-stu-id="f70f7-131">Get-ServiceFabricImageStoreContent</span></span>
* <span data-ttu-id="f70f7-132">Get-ServiceFabricChaosReport</span><span class="sxs-lookup"><span data-stu-id="f70f7-132">Get-ServiceFabricChaosReport</span></span>
* <span data-ttu-id="f70f7-133">Get-ServiceFabricNodeTransitionProgress</span><span class="sxs-lookup"><span data-stu-id="f70f7-133">Get-ServiceFabricNodeTransitionProgress</span></span>
* <span data-ttu-id="f70f7-134">Get-ServiceFabricPartitionDataLossProgress</span><span class="sxs-lookup"><span data-stu-id="f70f7-134">Get-ServiceFabricPartitionDataLossProgress</span></span>
* <span data-ttu-id="f70f7-135">Get-ServiceFabricPartitionQuorumLossProgress</span><span class="sxs-lookup"><span data-stu-id="f70f7-135">Get-ServiceFabricPartitionQuorumLossProgress</span></span>
* <span data-ttu-id="f70f7-136">Get-ServiceFabricPartitionRestartProgress</span><span class="sxs-lookup"><span data-stu-id="f70f7-136">Get-ServiceFabricPartitionRestartProgress</span></span>
* <span data-ttu-id="f70f7-137">Get-ServiceFabricTestCommandStatusList</span><span class="sxs-lookup"><span data-stu-id="f70f7-137">Get-ServiceFabricTestCommandStatusList</span></span>
* <span data-ttu-id="f70f7-138">Remove-ServiceFabricTestState</span><span class="sxs-lookup"><span data-stu-id="f70f7-138">Remove-ServiceFabricTestState</span></span>
* <span data-ttu-id="f70f7-139">Start-ServiceFabricChaos</span><span class="sxs-lookup"><span data-stu-id="f70f7-139">Start-ServiceFabricChaos</span></span>
* <span data-ttu-id="f70f7-140">Start-ServiceFabricNodeTransition</span><span class="sxs-lookup"><span data-stu-id="f70f7-140">Start-ServiceFabricNodeTransition</span></span>
* <span data-ttu-id="f70f7-141">Start-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="f70f7-141">Start-ServiceFabricPartitionDataLoss</span></span>
* <span data-ttu-id="f70f7-142">Start-ServiceFabricPartitionQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="f70f7-142">Start-ServiceFabricPartitionQuorumLoss</span></span>
* <span data-ttu-id="f70f7-143">Start-ServiceFabricPartitionRestart</span><span class="sxs-lookup"><span data-stu-id="f70f7-143">Start-ServiceFabricPartitionRestart</span></span>
* <span data-ttu-id="f70f7-144">Stop-ServiceFabricChaos</span><span class="sxs-lookup"><span data-stu-id="f70f7-144">Stop-ServiceFabricChaos</span></span>
* <span data-ttu-id="f70f7-145">Stop-ServiceFabricTestCommand</span><span class="sxs-lookup"><span data-stu-id="f70f7-145">Stop-ServiceFabricTestCommand</span></span>
* <span data-ttu-id="f70f7-146">Cmd</span><span class="sxs-lookup"><span data-stu-id="f70f7-146">Cmd</span></span>
* <span data-ttu-id="f70f7-147">Get-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="f70f7-147">Get-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="f70f7-148">Get-ServiceFabricClusterConfiguration</span><span class="sxs-lookup"><span data-stu-id="f70f7-148">Get-ServiceFabricClusterConfiguration</span></span>
* <span data-ttu-id="f70f7-149">Get-ServiceFabricClusterConfigurationUpgradeStatus</span><span class="sxs-lookup"><span data-stu-id="f70f7-149">Get-ServiceFabricClusterConfigurationUpgradeStatus</span></span>
* <span data-ttu-id="f70f7-150">Get-ServiceFabricPackageDebugParameters</span><span class="sxs-lookup"><span data-stu-id="f70f7-150">Get-ServiceFabricPackageDebugParameters</span></span>
* <span data-ttu-id="f70f7-151">New-ServiceFabricPackageDebugParameter</span><span class="sxs-lookup"><span data-stu-id="f70f7-151">New-ServiceFabricPackageDebugParameter</span></span>
* <span data-ttu-id="f70f7-152">New-ServiceFabricPackageSharingPolicy</span><span class="sxs-lookup"><span data-stu-id="f70f7-152">New-ServiceFabricPackageSharingPolicy</span></span>
* <span data-ttu-id="f70f7-153">Add-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="f70f7-153">Add-ServiceFabricNode</span></span>
* <span data-ttu-id="f70f7-154">Copy-ServiceFabricClusterPackage</span><span class="sxs-lookup"><span data-stu-id="f70f7-154">Copy-ServiceFabricClusterPackage</span></span>
* <span data-ttu-id="f70f7-155">Get-ServiceFabricRuntimeSupportedVersion</span><span class="sxs-lookup"><span data-stu-id="f70f7-155">Get-ServiceFabricRuntimeSupportedVersion</span></span>
* <span data-ttu-id="f70f7-156">Get-ServiceFabricRuntimeUpgradeVersion</span><span class="sxs-lookup"><span data-stu-id="f70f7-156">Get-ServiceFabricRuntimeUpgradeVersion</span></span>
* <span data-ttu-id="f70f7-157">New-ServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="f70f7-157">New-ServiceFabricCluster</span></span>
* <span data-ttu-id="f70f7-158">New-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="f70f7-158">New-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="f70f7-159">Remove-ServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="f70f7-159">Remove-ServiceFabricCluster</span></span>
* <span data-ttu-id="f70f7-160">Remove-ServiceFabricClusterPackage</span><span class="sxs-lookup"><span data-stu-id="f70f7-160">Remove-ServiceFabricClusterPackage</span></span>
* <span data-ttu-id="f70f7-161">Remove-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="f70f7-161">Remove-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="f70f7-162">Test-ServiceFabricClusterManifest</span><span class="sxs-lookup"><span data-stu-id="f70f7-162">Test-ServiceFabricClusterManifest</span></span>
* <span data-ttu-id="f70f7-163">Test-ServiceFabricConfiguration</span><span class="sxs-lookup"><span data-stu-id="f70f7-163">Test-ServiceFabricConfiguration</span></span>
* <span data-ttu-id="f70f7-164">Update-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="f70f7-164">Update-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="f70f7-165">Approve-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="f70f7-165">Approve-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="f70f7-166">Complete-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="f70f7-166">Complete-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="f70f7-167">Get-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="f70f7-167">Get-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="f70f7-168">Invoke-ServiceFabricDecryptText</span><span class="sxs-lookup"><span data-stu-id="f70f7-168">Invoke-ServiceFabricDecryptText</span></span>
* <span data-ttu-id="f70f7-169">Invoke-ServiceFabricEncryptSecret</span><span class="sxs-lookup"><span data-stu-id="f70f7-169">Invoke-ServiceFabricEncryptSecret</span></span>
* <span data-ttu-id="f70f7-170">Invoke-ServiceFabricEncryptText</span><span class="sxs-lookup"><span data-stu-id="f70f7-170">Invoke-ServiceFabricEncryptText</span></span>
* <span data-ttu-id="f70f7-171">Invoke-ServiceFabricInfrastructureCommand</span><span class="sxs-lookup"><span data-stu-id="f70f7-171">Invoke-ServiceFabricInfrastructureCommand</span></span>
* <span data-ttu-id="f70f7-172">Invoke-ServiceFabricInfrastructureQuery</span><span class="sxs-lookup"><span data-stu-id="f70f7-172">Invoke-ServiceFabricInfrastructureQuery</span></span>
* <span data-ttu-id="f70f7-173">Remove-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="f70f7-173">Remove-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="f70f7-174">Start-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="f70f7-174">Start-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="f70f7-175">Stop-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="f70f7-175">Stop-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="f70f7-176">Update-ServiceFabricRepairTaskHealthPolicy</span><span class="sxs-lookup"><span data-stu-id="f70f7-176">Update-ServiceFabricRepairTaskHealthPolicy</span></span>



## <a name="next-steps"></a><span data-ttu-id="f70f7-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f70f7-177">Next steps</span></span>
* [<span data-ttu-id="f70f7-178">A fejlesztőkörnyezet előkészítése Linuxon</span><span class="sxs-lookup"><span data-stu-id="f70f7-178">Prepare your development environment on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="f70f7-179">A fejlesztőkörnyezet előkészítése OSX-en</span><span class="sxs-lookup"><span data-stu-id="f70f7-179">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="f70f7-180">Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren Yeoman használatával</span><span class="sxs-lookup"><span data-stu-id="f70f7-180">Create and deploy your first Service Fabric Java application on Linux using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="f70f7-181">Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren az Eclipse Service Fabric beépülő modul használatával</span><span class="sxs-lookup"><span data-stu-id="f70f7-181">Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="f70f7-182">Az első CSharp-alkalmazás létrehozása Linuxon</span><span class="sxs-lookup"><span data-stu-id="f70f7-182">Create your first CSharp application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
* [<span data-ttu-id="f70f7-183">Hello Service Fabric CLI toomanage az alkalmazások használata</span><span class="sxs-lookup"><span data-stu-id="f70f7-183">Use hello Service Fabric CLI toomanage your applications</span></span>](service-fabric-application-lifecycle-sfctl.md)
