---
title: "Az Azure mikroszolgáltatások fejlesztési környezetének kialakítása | Microsoft Docs"
description: "Telepítse a futtatókörnyezetet, az SDK-t és az eszközöket, majd hozzon létre egy helyi fejlesztési fürtöt. A beállítás befejezése után készen áll az alkalmazások létrehozására."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/10/2017
ms.author: ryanwi, mikhegn
ms.openlocfilehash: f0c6957217c21bdfd76498944e248fc808f2d271
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="prepare-your-development-environment"></a><span data-ttu-id="5ba91-104">A fejlesztőkörnyezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="5ba91-104">Prepare your development environment</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5ba91-105">Windows</span><span class="sxs-lookup"><span data-stu-id="5ba91-105">Windows</span></span>](service-fabric-get-started.md) 
> * [<span data-ttu-id="5ba91-106">Linux</span><span class="sxs-lookup"><span data-stu-id="5ba91-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="5ba91-107">OSX</span><span class="sxs-lookup"><span data-stu-id="5ba91-107">OSX</span></span>](service-fabric-get-started-mac.md)
> 
> 

 <span data-ttu-id="5ba91-108">Az [Azure Service Fabric-alkalmazásoknak][1] a fejlesztői gépen való létrehozásához és futtatásához telepítse a futtatókörnyezetet, az SDK-t és az eszközöket.</span><span class="sxs-lookup"><span data-stu-id="5ba91-108">To build and run [Azure Service Fabric applications][1] on your development machine, install the runtime, SDK, and tools.</span></span> <span data-ttu-id="5ba91-109">Továbbá engedélyeznie kell az SDK-ban található Windows PowerShell-parancsfájlok végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="5ba91-109">You also need to enable execution of the Windows PowerShell scripts included in the SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ba91-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5ba91-110">Prerequisites</span></span>
### <a name="supported-operating-system-versions"></a><span data-ttu-id="5ba91-111">Támogatott operációsrendszer-verziók</span><span class="sxs-lookup"><span data-stu-id="5ba91-111">Supported operating system versions</span></span>
<span data-ttu-id="5ba91-112">A fejlesztéshez a következő operációsrendszer-verziók támogatottak:</span><span class="sxs-lookup"><span data-stu-id="5ba91-112">The following operating system versions are supported for development:</span></span>

* <span data-ttu-id="5ba91-113">Windows 7</span><span class="sxs-lookup"><span data-stu-id="5ba91-113">Windows 7</span></span>
* <span data-ttu-id="5ba91-114">Windows 8/Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="5ba91-114">Windows 8/Windows 8.1</span></span>
* <span data-ttu-id="5ba91-115">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="5ba91-115">Windows Server 2012 R2</span></span>
* <span data-ttu-id="5ba91-116">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="5ba91-116">Windows Server 2016</span></span>
* <span data-ttu-id="5ba91-117">Windows 10</span><span class="sxs-lookup"><span data-stu-id="5ba91-117">Windows 10</span></span>

> [!NOTE]
> <span data-ttu-id="5ba91-118">Alapértelmezés szerint a Windows 7 csak a Windows PowerShell 2.0-t tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5ba91-118">Windows 7 only includes Windows PowerShell 2.0 by default.</span></span> <span data-ttu-id="5ba91-119">A Service Fabric PowerShell-parancsmagokhoz a PowerShell 3.0 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="5ba91-119">Service Fabric PowerShell cmdlets requires PowerShell 3.0 or higher.</span></span> <span data-ttu-id="5ba91-120">A [Windows PowerShell 5.0-t letöltheti][powershell5-download] a Microsoft letöltőközpontból.</span><span class="sxs-lookup"><span data-stu-id="5ba91-120">You can [download Windows PowerShell 5.0][powershell5-download] from the Microsoft Download Center.</span></span>
> 
> 

## <a name="install-the-sdk-and-tools"></a><span data-ttu-id="5ba91-121">Az SDK és az eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="5ba91-121">Install the SDK and tools</span></span>
### <a name="to-use-visual-studio-2017"></a><span data-ttu-id="5ba91-122">A Visual Studio 2017 használata</span><span class="sxs-lookup"><span data-stu-id="5ba91-122">To use Visual Studio 2017</span></span>
<span data-ttu-id="5ba91-123">A Service Fabric-eszközök a Visual Studio 2017 Azure Development and Management Workload munkafolyamatának részét képezik.</span><span class="sxs-lookup"><span data-stu-id="5ba91-123">Service Fabric Tools are part of the Azure Development and Management workload in Visual Studio 2017.</span></span> <span data-ttu-id="5ba91-124">A Visual Studio telepítésének részeként engedélyezze ezt a munkafolyamatot.</span><span class="sxs-lookup"><span data-stu-id="5ba91-124">Enable this workload as part of your Visual Studio installation.</span></span>
<span data-ttu-id="5ba91-125">Emellett telepítenie kell a Microsoft Azure Service Fabric SDK-t is a webplatform-telepítővel.</span><span class="sxs-lookup"><span data-stu-id="5ba91-125">In addition, you need to install the Microsoft Azure Service Fabric SDK, using Web Platform Installer.</span></span>

* <span data-ttu-id="5ba91-126">[A Microsoft Azure Service Fabric SDK telepítése][core-sdk]</span><span class="sxs-lookup"><span data-stu-id="5ba91-126">[Install the Microsoft Azure Service Fabric SDK][core-sdk]</span></span>

### <a name="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a><span data-ttu-id="5ba91-127">A Visual Studio 2015 használata (Visual Studio 2015 2. frissítés vagy újabb szükséges)</span><span class="sxs-lookup"><span data-stu-id="5ba91-127">To use Visual Studio 2015 (requires Visual Studio 2015 Update 2 or later)</span></span>
<span data-ttu-id="5ba91-128">A Visual Studio 2015 esetében a Service Fabric-eszközök az SDK-val együtt települnek a webplatform-telepítő használatával:</span><span class="sxs-lookup"><span data-stu-id="5ba91-128">For Visual Studio 2015, Service Fabric tools are installed together with the SDK, using the Web Platform Installer:</span></span>

* <span data-ttu-id="5ba91-129">[A Microsoft Azure Service Fabric SDK és eszközök telepítése][full-bundle-vs2015]</span><span class="sxs-lookup"><span data-stu-id="5ba91-129">[Install the Microsoft Azure Service Fabric SDK and Tools][full-bundle-vs2015]</span></span>

### <a name="sdk-installation-only"></a><span data-ttu-id="5ba91-130">Csak az SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="5ba91-130">SDK installation only</span></span>
<span data-ttu-id="5ba91-131">Ha csak az SDK-ra van szükség, telepítse a következő csomagot:</span><span class="sxs-lookup"><span data-stu-id="5ba91-131">If you only need the SDK, you can install this package:</span></span>
* <span data-ttu-id="5ba91-132">[A Microsoft Azure Service Fabric SDK telepítése][core-sdk]</span><span class="sxs-lookup"><span data-stu-id="5ba91-132">[Install the Microsoft Azure Service Fabric SDK][core-sdk]</span></span>

<span data-ttu-id="5ba91-133">Az aktuális verziók a következők:</span><span class="sxs-lookup"><span data-stu-id="5ba91-133">The current versions are:</span></span>
* <span data-ttu-id="5ba91-134">Service Fabric SDK 2.7.198</span><span class="sxs-lookup"><span data-stu-id="5ba91-134">Service Fabric SDK 2.7.198</span></span>
* <span data-ttu-id="5ba91-135">Service Fabric-futtatókörnyezet 5.7.198</span><span class="sxs-lookup"><span data-stu-id="5ba91-135">Service Fabric runtime 5.7.198</span></span>
* <span data-ttu-id="5ba91-136">Service Fabric Tools for Visual Studio 2015 1.7.50721</span><span class="sxs-lookup"><span data-stu-id="5ba91-136">Service Fabric Tools for Visual Studio 2015 1.7.50721</span></span>
* <span data-ttu-id="5ba91-137">A Visual Studio 2017 Update 2 tartalmazza a Service Fabric Tools for Visual Studio 1.6.20170504-es verzióját</span><span class="sxs-lookup"><span data-stu-id="5ba91-137">Visual Studio 2017 Update 2 includes Service Fabric Tools for Visual Studio 1.6.20170504</span></span>
* <span data-ttu-id="5ba91-138">A Visual Studio 2017 Update 3 Preview 7 (15.3.0 Preview 7.0) tartalmazza a Service Fabric Tools for Visual Studio 1.7.20170721-es verzióját</span><span class="sxs-lookup"><span data-stu-id="5ba91-138">Visual Studio 2017 Update 3 Preview 7 (15.3.0 Preview 7.0) includes Service Fabric Tools for Visual Studio 1.7.20170721</span></span>

<span data-ttu-id="5ba91-139">A támogatott verziók listáját lásd: [Service Fabric-támogatás](service-fabric-support.md).</span><span class="sxs-lookup"><span data-stu-id="5ba91-139">For a list of supported versions, see [Service Fabric support](service-fabric-support.md)</span></span>

## <a name="enable-powershell-script-execution"></a><span data-ttu-id="5ba91-140">A PowerShell-parancsfájl végrehajtásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5ba91-140">Enable PowerShell script execution</span></span>
<span data-ttu-id="5ba91-141">A Service Fabric Windows PowerShell-parancsfájlokat használ a helyi fejlesztési fürtök létrehozásához és az alkalmazások Visual Studióból történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="5ba91-141">Service Fabric uses Windows PowerShell scripts for creating a local development cluster and for deploying applications from Visual Studio.</span></span> <span data-ttu-id="5ba91-142">Alapértelmezés szerint a Windows blokkolja ezen szkriptek futását.</span><span class="sxs-lookup"><span data-stu-id="5ba91-142">By default, Windows blocks these scripts from running.</span></span> <span data-ttu-id="5ba91-143">Az engedélyezésükhöz módosítania kell a PowerShell végrehajtási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="5ba91-143">To enable them, you must modify your PowerShell execution policy.</span></span> <span data-ttu-id="5ba91-144">Nyissa meg a PowerShellt rendszergazdaként, és írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="5ba91-144">Open PowerShell as an administrator and enter the following command:</span></span>

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a><span data-ttu-id="5ba91-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ba91-145">Next steps</span></span>
<span data-ttu-id="5ba91-146">Most, hogy végzett a fejlesztőkörnyezet beállításával, belefoghat az alkalmazások létrehozásába és futtatásába.</span><span class="sxs-lookup"><span data-stu-id="5ba91-146">Now that you've finished setting up your development environment, start building and running apps.</span></span>

* [<span data-ttu-id="5ba91-147">Az első Service Fabric-alkalmazás létrehozása a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="5ba91-147">Create your first Service Fabric application in Visual Studio</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
* [<span data-ttu-id="5ba91-148">Ismerje meg az alkalmazások helyi fürtön történő üzembe helyezését és kezelését</span><span class="sxs-lookup"><span data-stu-id="5ba91-148">Learn how to deploy and manage applications on your local cluster</span></span>](service-fabric-get-started-with-a-local-cluster.md)
* [<span data-ttu-id="5ba91-149">További tudnivalók a programozási modellekről: Reliable Services és Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="5ba91-149">Learn about the programming models: Reliable Services and Reliable Actors</span></span>](service-fabric-choose-framework.md)
* [<span data-ttu-id="5ba91-150">A Service Fabric mintakódjainak megtekintése a GitHubon</span><span class="sxs-lookup"><span data-stu-id="5ba91-150">Check out the Service Fabric code samples on GitHub</span></span>](https://aka.ms/servicefabricsamples)
* [<span data-ttu-id="5ba91-151">A fürt megjelenítése a Service Fabric Explorer segítségével</span><span class="sxs-lookup"><span data-stu-id="5ba91-151">Visualize your cluster by using Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)
* [<span data-ttu-id="5ba91-152">A platform átfogó bemutatásának megtekintéséhez kövesse a Service Fabric képzési tervét</span><span class="sxs-lookup"><span data-stu-id="5ba91-152">Follow the Service Fabric learning path to get a broad introduction to the platform</span></span>](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* <span data-ttu-id="5ba91-153">A [Service Fabric támogatási lehetőségeinek](service-fabric-support.md) ismertetése</span><span class="sxs-lookup"><span data-stu-id="5ba91-153">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>

<span data-ttu-id="5ba91-154">[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "A Service Fabric kampányoldala"</span><span class="sxs-lookup"><span data-stu-id="5ba91-154">[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric campaign page"</span></span>
<span data-ttu-id="5ba91-155">[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"</span><span class="sxs-lookup"><span data-stu-id="5ba91-155">[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"</span></span>
<span data-ttu-id="5ba91-156">[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI-hivatkozás"</span><span class="sxs-lookup"><span data-stu-id="5ba91-156">[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI link"</span></span>
<span data-ttu-id="5ba91-157">[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI-hivatkozás"</span><span class="sxs-lookup"><span data-stu-id="5ba91-157">[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI link"</span></span>
<span data-ttu-id="5ba91-158">[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI-hivatkozás"</span><span class="sxs-lookup"><span data-stu-id="5ba91-158">[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI link"</span></span>
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
