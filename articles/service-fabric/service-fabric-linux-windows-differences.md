---
title: "Különbségek az Azure Service Fabric Linux- és Windows-verziója között | Microsoft Docs"
description: "Az Azure Service Fabric előzetes verzió Linux- és Windows-verziója közötti különbségek."
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
ms.openlocfilehash: 7b80bb7d4a4e6a1b4cf47ce87200f47339785c53
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a><span data-ttu-id="50c77-103">A Service Fabric Linux (előzetes verziójú) és Windows (általánosan elérhető) rendszerhez készült verziója közötti különbségek</span><span class="sxs-lookup"><span data-stu-id="50c77-103">Differences between Service Fabric on Linux (preview) and Windows (generally available)</span></span>

<span data-ttu-id="50c77-104">Mivel a Service Fabric Linux rendszeren még előzetes verzióban van, ezért néhány szolgáltatás csak Windows rendszeren támogatott, Linuxon nem.</span><span class="sxs-lookup"><span data-stu-id="50c77-104">Since Service Fabric on Linux is a preview, there are some features that are supported on Windows, but not yet on Linux.</span></span> <span data-ttu-id="50c77-105">Idővel ugyanazok a szolgáltatások lesznek elérhetőek, amikor a Service Fabric általánosan elérhetővé válik Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="50c77-105">Eventually, the feature sets will be at parity when Service Fabric on Linux becomes generally available.</span></span> <span data-ttu-id="50c77-106">A jövőbeli kiadásokban a funkciók eltérései egyre csekélyebbé válnak.</span><span class="sxs-lookup"><span data-stu-id="50c77-106">With upcoming releases, this feature gap will shrink.</span></span> <span data-ttu-id="50c77-107">A legújabb elérhető kiadások (Windows rendszeren 5.6, Linuxon 5.5) között az alábbi eltérések állnak fenn:</span><span class="sxs-lookup"><span data-stu-id="50c77-107">The following differences exist between the latest available releases (that is, between version 5.6 on Windows and version 5.5 on Linux):</span></span> 

* <span data-ttu-id="50c77-108">Reliable Collections (és a Reliable Stateful Services)</span><span class="sxs-lookup"><span data-stu-id="50c77-108">Reliable Collections (and Reliable Stateful Services)</span></span> 
* <span data-ttu-id="50c77-109">ReverseProxy</span><span class="sxs-lookup"><span data-stu-id="50c77-109">ReverseProxy</span></span> 
* <span data-ttu-id="50c77-110">Önálló telepítő</span><span class="sxs-lookup"><span data-stu-id="50c77-110">Standalone installer</span></span> 
* <span data-ttu-id="50c77-111">Jegyzékfájlok XML-sémaérvényesítése</span><span class="sxs-lookup"><span data-stu-id="50c77-111">XML schema validation for manifest files</span></span> 
* <span data-ttu-id="50c77-112">Konzol-átirányítás</span><span class="sxs-lookup"><span data-stu-id="50c77-112">Console redirection</span></span> 
* <span data-ttu-id="50c77-113">Fault Analysis Service (FAS)</span><span class="sxs-lookup"><span data-stu-id="50c77-113">The Fault Analysis Service (FAS)</span></span>
* <span data-ttu-id="50c77-114">Docker compose, kötet- és naplózási illesztők tárolókhoz</span><span class="sxs-lookup"><span data-stu-id="50c77-114">Docker compose and volume and logging drivers for containers</span></span> 
* <span data-ttu-id="50c77-115">Tárolók és szolgáltatások erőforrás-szabályozása</span><span class="sxs-lookup"><span data-stu-id="50c77-115">Resource governance for containers and services</span></span> 
* <span data-ttu-id="50c77-116">DNS-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="50c77-116">DNS service</span></span>
* <span data-ttu-id="50c77-117">Azure Active Directory-támogatás</span><span class="sxs-lookup"><span data-stu-id="50c77-117">Azure Active Directory support</span></span>
* <span data-ttu-id="50c77-118">Egyes PowerShell-parancsok parancssori felületi megfelelője</span><span class="sxs-lookup"><span data-stu-id="50c77-118">CLI command equivalents of certain Powershell commands</span></span> 
* <span data-ttu-id="50c77-119">Csak a PowerShell-parancsok egy része futtatható Linux-fürtökön (a következő szakaszban leírtak szerint).</span><span class="sxs-lookup"><span data-stu-id="50c77-119">Only a subset of Powershell commands can be run against a Linux cluster (as expanded in the next section).</span></span>

>[!NOTE]
><span data-ttu-id="50c77-120">A konzolátirányítás nem támogatott éles fürtökben, még Windows rendszeren sem.</span><span class="sxs-lookup"><span data-stu-id="50c77-120">Console redirection is not supported in production clusters, even on Windows.</span></span>

<span data-ttu-id="50c77-121">A fejlesztői eszközök eltérnek Windows és Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="50c77-121">Development tooling is also different between Windows and Linux.</span></span> <span data-ttu-id="50c77-122">Windows rendszeren a VisualStudio, a PowerShell, a VSTS és az ETW, míg Linuxon a Yeoman, az Eclipse, a Jenkins és az LTTng érhető el.</span><span class="sxs-lookup"><span data-stu-id="50c77-122">VisualStudio, Powershell, VSTS, and ETW are used on Windows while Yeoman, Eclipse, Jenkins, and LTTng are used on Linux.</span></span>

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a><span data-ttu-id="50c77-123">PowerShell-parancsmagok, amelyek nem működnek Linux rendszerű Service Fabric-fürtökön</span><span class="sxs-lookup"><span data-stu-id="50c77-123">Powershell cmdlets that do not work against a Linux Service Fabric cluster</span></span>

* <span data-ttu-id="50c77-124">Invoke-ServiceFabricChaosTestScenario</span><span class="sxs-lookup"><span data-stu-id="50c77-124">Invoke-ServiceFabricChaosTestScenario</span></span>
* <span data-ttu-id="50c77-125">Invoke-ServiceFabricFailoverTestScenario</span><span class="sxs-lookup"><span data-stu-id="50c77-125">Invoke-ServiceFabricFailoverTestScenario</span></span>
* <span data-ttu-id="50c77-126">Invoke-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="50c77-126">Invoke-ServiceFabricPartitionDataLoss</span></span>
* <span data-ttu-id="50c77-127">Invoke-ServiceFabricPartitionQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="50c77-127">Invoke-ServiceFabricPartitionQuorumLoss</span></span>
* <span data-ttu-id="50c77-128">Restart-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="50c77-128">Restart-ServiceFabricPartition</span></span>
* <span data-ttu-id="50c77-129">Start-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="50c77-129">Start-ServiceFabricNode</span></span>
* <span data-ttu-id="50c77-130">Stop-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="50c77-130">Stop-ServiceFabricNode</span></span>
* <span data-ttu-id="50c77-131">Get-ServiceFabricImageStoreContent</span><span class="sxs-lookup"><span data-stu-id="50c77-131">Get-ServiceFabricImageStoreContent</span></span>
* <span data-ttu-id="50c77-132">Get-ServiceFabricChaosReport</span><span class="sxs-lookup"><span data-stu-id="50c77-132">Get-ServiceFabricChaosReport</span></span>
* <span data-ttu-id="50c77-133">Get-ServiceFabricNodeTransitionProgress</span><span class="sxs-lookup"><span data-stu-id="50c77-133">Get-ServiceFabricNodeTransitionProgress</span></span>
* <span data-ttu-id="50c77-134">Get-ServiceFabricPartitionDataLossProgress</span><span class="sxs-lookup"><span data-stu-id="50c77-134">Get-ServiceFabricPartitionDataLossProgress</span></span>
* <span data-ttu-id="50c77-135">Get-ServiceFabricPartitionQuorumLossProgress</span><span class="sxs-lookup"><span data-stu-id="50c77-135">Get-ServiceFabricPartitionQuorumLossProgress</span></span>
* <span data-ttu-id="50c77-136">Get-ServiceFabricPartitionRestartProgress</span><span class="sxs-lookup"><span data-stu-id="50c77-136">Get-ServiceFabricPartitionRestartProgress</span></span>
* <span data-ttu-id="50c77-137">Get-ServiceFabricTestCommandStatusList</span><span class="sxs-lookup"><span data-stu-id="50c77-137">Get-ServiceFabricTestCommandStatusList</span></span>
* <span data-ttu-id="50c77-138">Remove-ServiceFabricTestState</span><span class="sxs-lookup"><span data-stu-id="50c77-138">Remove-ServiceFabricTestState</span></span>
* <span data-ttu-id="50c77-139">Start-ServiceFabricChaos</span><span class="sxs-lookup"><span data-stu-id="50c77-139">Start-ServiceFabricChaos</span></span>
* <span data-ttu-id="50c77-140">Start-ServiceFabricNodeTransition</span><span class="sxs-lookup"><span data-stu-id="50c77-140">Start-ServiceFabricNodeTransition</span></span>
* <span data-ttu-id="50c77-141">Start-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="50c77-141">Start-ServiceFabricPartitionDataLoss</span></span>
* <span data-ttu-id="50c77-142">Start-ServiceFabricPartitionQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="50c77-142">Start-ServiceFabricPartitionQuorumLoss</span></span>
* <span data-ttu-id="50c77-143">Start-ServiceFabricPartitionRestart</span><span class="sxs-lookup"><span data-stu-id="50c77-143">Start-ServiceFabricPartitionRestart</span></span>
* <span data-ttu-id="50c77-144">Stop-ServiceFabricChaos</span><span class="sxs-lookup"><span data-stu-id="50c77-144">Stop-ServiceFabricChaos</span></span>
* <span data-ttu-id="50c77-145">Stop-ServiceFabricTestCommand</span><span class="sxs-lookup"><span data-stu-id="50c77-145">Stop-ServiceFabricTestCommand</span></span>
* <span data-ttu-id="50c77-146">Cmd</span><span class="sxs-lookup"><span data-stu-id="50c77-146">Cmd</span></span>
* <span data-ttu-id="50c77-147">Get-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="50c77-147">Get-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="50c77-148">Get-ServiceFabricClusterConfiguration</span><span class="sxs-lookup"><span data-stu-id="50c77-148">Get-ServiceFabricClusterConfiguration</span></span>
* <span data-ttu-id="50c77-149">Get-ServiceFabricClusterConfigurationUpgradeStatus</span><span class="sxs-lookup"><span data-stu-id="50c77-149">Get-ServiceFabricClusterConfigurationUpgradeStatus</span></span>
* <span data-ttu-id="50c77-150">Get-ServiceFabricPackageDebugParameters</span><span class="sxs-lookup"><span data-stu-id="50c77-150">Get-ServiceFabricPackageDebugParameters</span></span>
* <span data-ttu-id="50c77-151">New-ServiceFabricPackageDebugParameter</span><span class="sxs-lookup"><span data-stu-id="50c77-151">New-ServiceFabricPackageDebugParameter</span></span>
* <span data-ttu-id="50c77-152">New-ServiceFabricPackageSharingPolicy</span><span class="sxs-lookup"><span data-stu-id="50c77-152">New-ServiceFabricPackageSharingPolicy</span></span>
* <span data-ttu-id="50c77-153">Add-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="50c77-153">Add-ServiceFabricNode</span></span>
* <span data-ttu-id="50c77-154">Copy-ServiceFabricClusterPackage</span><span class="sxs-lookup"><span data-stu-id="50c77-154">Copy-ServiceFabricClusterPackage</span></span>
* <span data-ttu-id="50c77-155">Get-ServiceFabricRuntimeSupportedVersion</span><span class="sxs-lookup"><span data-stu-id="50c77-155">Get-ServiceFabricRuntimeSupportedVersion</span></span>
* <span data-ttu-id="50c77-156">Get-ServiceFabricRuntimeUpgradeVersion</span><span class="sxs-lookup"><span data-stu-id="50c77-156">Get-ServiceFabricRuntimeUpgradeVersion</span></span>
* <span data-ttu-id="50c77-157">New-ServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="50c77-157">New-ServiceFabricCluster</span></span>
* <span data-ttu-id="50c77-158">New-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="50c77-158">New-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="50c77-159">Remove-ServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="50c77-159">Remove-ServiceFabricCluster</span></span>
* <span data-ttu-id="50c77-160">Remove-ServiceFabricClusterPackage</span><span class="sxs-lookup"><span data-stu-id="50c77-160">Remove-ServiceFabricClusterPackage</span></span>
* <span data-ttu-id="50c77-161">Remove-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="50c77-161">Remove-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="50c77-162">Test-ServiceFabricClusterManifest</span><span class="sxs-lookup"><span data-stu-id="50c77-162">Test-ServiceFabricClusterManifest</span></span>
* <span data-ttu-id="50c77-163">Test-ServiceFabricConfiguration</span><span class="sxs-lookup"><span data-stu-id="50c77-163">Test-ServiceFabricConfiguration</span></span>
* <span data-ttu-id="50c77-164">Update-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="50c77-164">Update-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="50c77-165">Approve-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="50c77-165">Approve-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="50c77-166">Complete-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="50c77-166">Complete-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="50c77-167">Get-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="50c77-167">Get-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="50c77-168">Invoke-ServiceFabricDecryptText</span><span class="sxs-lookup"><span data-stu-id="50c77-168">Invoke-ServiceFabricDecryptText</span></span>
* <span data-ttu-id="50c77-169">Invoke-ServiceFabricEncryptSecret</span><span class="sxs-lookup"><span data-stu-id="50c77-169">Invoke-ServiceFabricEncryptSecret</span></span>
* <span data-ttu-id="50c77-170">Invoke-ServiceFabricEncryptText</span><span class="sxs-lookup"><span data-stu-id="50c77-170">Invoke-ServiceFabricEncryptText</span></span>
* <span data-ttu-id="50c77-171">Invoke-ServiceFabricInfrastructureCommand</span><span class="sxs-lookup"><span data-stu-id="50c77-171">Invoke-ServiceFabricInfrastructureCommand</span></span>
* <span data-ttu-id="50c77-172">Invoke-ServiceFabricInfrastructureQuery</span><span class="sxs-lookup"><span data-stu-id="50c77-172">Invoke-ServiceFabricInfrastructureQuery</span></span>
* <span data-ttu-id="50c77-173">Remove-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="50c77-173">Remove-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="50c77-174">Start-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="50c77-174">Start-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="50c77-175">Stop-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="50c77-175">Stop-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="50c77-176">Update-ServiceFabricRepairTaskHealthPolicy</span><span class="sxs-lookup"><span data-stu-id="50c77-176">Update-ServiceFabricRepairTaskHealthPolicy</span></span>



## <a name="next-steps"></a><span data-ttu-id="50c77-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="50c77-177">Next steps</span></span>
* [<span data-ttu-id="50c77-178">A fejlesztőkörnyezet előkészítése Linuxon</span><span class="sxs-lookup"><span data-stu-id="50c77-178">Prepare your development environment on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="50c77-179">A fejlesztőkörnyezet előkészítése OSX-en</span><span class="sxs-lookup"><span data-stu-id="50c77-179">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="50c77-180">Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren Yeoman használatával</span><span class="sxs-lookup"><span data-stu-id="50c77-180">Create and deploy your first Service Fabric Java application on Linux using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="50c77-181">Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren az Eclipse Service Fabric beépülő modul használatával</span><span class="sxs-lookup"><span data-stu-id="50c77-181">Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="50c77-182">Az első CSharp-alkalmazás létrehozása Linuxon</span><span class="sxs-lookup"><span data-stu-id="50c77-182">Create your first CSharp application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
* [<span data-ttu-id="50c77-183">A Service Fabric parancssori felület használata az alkalmazások kezeléséhez</span><span class="sxs-lookup"><span data-stu-id="50c77-183">Use the Service Fabric CLI to manage your applications</span></span>](service-fabric-application-lifecycle-sfctl.md)
