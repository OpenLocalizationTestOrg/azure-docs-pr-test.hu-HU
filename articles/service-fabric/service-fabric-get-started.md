---
title: "egy Azure mikroszolgáltatások a fejlesztési környezet létrehozása aaaSet |} Microsoft Docs"
description: "Hello futtatókörnyezet, az SDK és az eszközök telepítése, és hozzon létre egy helyi fejlesztési fürtöt. A telepítés befejezése után készen áll a toobuild alkalmazások fogja."
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
ms.openlocfilehash: 9b0442778999d4c3d2b99adb98f6596dcbdc36d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-development-environment"></a><span data-ttu-id="3a2f8-104">A fejlesztőkörnyezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="3a2f8-104">Prepare your development environment</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3a2f8-105">Windows</span><span class="sxs-lookup"><span data-stu-id="3a2f8-105">Windows</span></span>](service-fabric-get-started.md) 
> * [<span data-ttu-id="3a2f8-106">Linux</span><span class="sxs-lookup"><span data-stu-id="3a2f8-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="3a2f8-107">OSX</span><span class="sxs-lookup"><span data-stu-id="3a2f8-107">OSX</span></span>](service-fabric-get-started-mac.md)
> 
> 

 <span data-ttu-id="3a2f8-108">toobuild, és futtassa [Azure Service Fabric-alkalmazások] [ 1] a fejlesztői számítógépén, telepítse a hello futtatókörnyezet, az SDK és az eszközök.</span><span class="sxs-lookup"><span data-stu-id="3a2f8-108">toobuild and run [Azure Service Fabric applications][1] on your development machine, install hello runtime, SDK, and tools.</span></span> <span data-ttu-id="3a2f8-109">Meg kell tooenable hello hello SDK szereplő Windows PowerShell-parancsfájlok végrehajtását is.</span><span class="sxs-lookup"><span data-stu-id="3a2f8-109">You also need tooenable execution of hello Windows PowerShell scripts included in hello SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3a2f8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3a2f8-110">Prerequisites</span></span>
### <a name="supported-operating-system-versions"></a><span data-ttu-id="3a2f8-111">Támogatott operációsrendszer-verziók</span><span class="sxs-lookup"><span data-stu-id="3a2f8-111">Supported operating system versions</span></span>
<span data-ttu-id="3a2f8-112">a következő operációsrendszer-verziók hello fejlesztési támogatottak:</span><span class="sxs-lookup"><span data-stu-id="3a2f8-112">hello following operating system versions are supported for development:</span></span>

* <span data-ttu-id="3a2f8-113">Windows 7</span><span class="sxs-lookup"><span data-stu-id="3a2f8-113">Windows 7</span></span>
* <span data-ttu-id="3a2f8-114">Windows 8/Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="3a2f8-114">Windows 8/Windows 8.1</span></span>
* <span data-ttu-id="3a2f8-115">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="3a2f8-115">Windows Server 2012 R2</span></span>
* <span data-ttu-id="3a2f8-116">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="3a2f8-116">Windows Server 2016</span></span>
* <span data-ttu-id="3a2f8-117">Windows 10</span><span class="sxs-lookup"><span data-stu-id="3a2f8-117">Windows 10</span></span>

> [!NOTE]
> <span data-ttu-id="3a2f8-118">Alapértelmezés szerint a Windows 7 csak a Windows PowerShell 2.0-t tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3a2f8-118">Windows 7 only includes Windows PowerShell 2.0 by default.</span></span> <span data-ttu-id="3a2f8-119">A Service Fabric PowerShell-parancsmagokhoz a PowerShell 3.0 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="3a2f8-119">Service Fabric PowerShell cmdlets requires PowerShell 3.0 or higher.</span></span> <span data-ttu-id="3a2f8-120">Is [töltse le a Windows PowerShell 5.0] [ powershell5-download] a hello Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="3a2f8-120">You can [download Windows PowerShell 5.0][powershell5-download] from hello Microsoft Download Center.</span></span>
> 
> 

## <a name="install-hello-sdk-and-tools"></a><span data-ttu-id="3a2f8-121">Hello SDK és az eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="3a2f8-121">Install hello SDK and tools</span></span>
### <a name="toouse-visual-studio-2017"></a><span data-ttu-id="3a2f8-122">a Visual Studio 2017 toouse</span><span class="sxs-lookup"><span data-stu-id="3a2f8-122">toouse Visual Studio 2017</span></span>
<span data-ttu-id="3a2f8-123">Service Fabric-eszközök Visual Studio 2017 hello Azure fejlesztői és felügyeleti munkaterhelés részét képezik.</span><span class="sxs-lookup"><span data-stu-id="3a2f8-123">Service Fabric Tools are part of hello Azure Development and Management workload in Visual Studio 2017.</span></span> <span data-ttu-id="3a2f8-124">A Visual Studio telepítésének részeként engedélyezze ezt a munkafolyamatot.</span><span class="sxs-lookup"><span data-stu-id="3a2f8-124">Enable this workload as part of your Visual Studio installation.</span></span>
<span data-ttu-id="3a2f8-125">Ezenkívül a Microsoft Azure Service Fabric SDK, Webplatform-telepítővel történő tooinstall hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3a2f8-125">In addition, you need tooinstall hello Microsoft Azure Service Fabric SDK, using Web Platform Installer.</span></span>

* <span data-ttu-id="3a2f8-126">[Telepítse a Microsoft Azure Service Fabric SDK hello][core-sdk]</span><span class="sxs-lookup"><span data-stu-id="3a2f8-126">[Install hello Microsoft Azure Service Fabric SDK][core-sdk]</span></span>

### <a name="toouse-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a><span data-ttu-id="3a2f8-127">Visual Studio 2015-öt toouse (a Visual Studio 2015 Update 2 vagy újabb szükséges)</span><span class="sxs-lookup"><span data-stu-id="3a2f8-127">toouse Visual Studio 2015 (requires Visual Studio 2015 Update 2 or later)</span></span>
<span data-ttu-id="3a2f8-128">A Visual Studio 2015 a Service Fabric eszközök hello SDK használatával hello Webplatform-telepítővel együtt vannak telepítve:</span><span class="sxs-lookup"><span data-stu-id="3a2f8-128">For Visual Studio 2015, Service Fabric tools are installed together with hello SDK, using hello Web Platform Installer:</span></span>

* <span data-ttu-id="3a2f8-129">[A Microsoft Azure Service Fabric SDK hello és eszközök telepítése][full-bundle-vs2015]</span><span class="sxs-lookup"><span data-stu-id="3a2f8-129">[Install hello Microsoft Azure Service Fabric SDK and Tools][full-bundle-vs2015]</span></span>

### <a name="sdk-installation-only"></a><span data-ttu-id="3a2f8-130">Csak az SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="3a2f8-130">SDK installation only</span></span>
<span data-ttu-id="3a2f8-131">Ha csak hello SDK, telepítheti ezt a csomagot:</span><span class="sxs-lookup"><span data-stu-id="3a2f8-131">If you only need hello SDK, you can install this package:</span></span>
* <span data-ttu-id="3a2f8-132">[Telepítse a Microsoft Azure Service Fabric SDK hello][core-sdk]</span><span class="sxs-lookup"><span data-stu-id="3a2f8-132">[Install hello Microsoft Azure Service Fabric SDK][core-sdk]</span></span>

<span data-ttu-id="3a2f8-133">hello aktuális verziók az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="3a2f8-133">hello current versions are:</span></span>
* <span data-ttu-id="3a2f8-134">Service Fabric SDK 2.7.198</span><span class="sxs-lookup"><span data-stu-id="3a2f8-134">Service Fabric SDK 2.7.198</span></span>
* <span data-ttu-id="3a2f8-135">Service Fabric-futtatókörnyezet 5.7.198</span><span class="sxs-lookup"><span data-stu-id="3a2f8-135">Service Fabric runtime 5.7.198</span></span>
* <span data-ttu-id="3a2f8-136">Service Fabric Tools for Visual Studio 2015 1.7.50721</span><span class="sxs-lookup"><span data-stu-id="3a2f8-136">Service Fabric Tools for Visual Studio 2015 1.7.50721</span></span>
* <span data-ttu-id="3a2f8-137">A Visual Studio 2017 Update 2 tartalmazza a Service Fabric Tools for Visual Studio 1.6.20170504-es verzióját</span><span class="sxs-lookup"><span data-stu-id="3a2f8-137">Visual Studio 2017 Update 2 includes Service Fabric Tools for Visual Studio 1.6.20170504</span></span>
* <span data-ttu-id="3a2f8-138">A Visual Studio 2017 Update 3 Preview 7 (15.3.0 Preview 7.0) tartalmazza a Service Fabric Tools for Visual Studio 1.7.20170721-es verzióját</span><span class="sxs-lookup"><span data-stu-id="3a2f8-138">Visual Studio 2017 Update 3 Preview 7 (15.3.0 Preview 7.0) includes Service Fabric Tools for Visual Studio 1.7.20170721</span></span>

<span data-ttu-id="3a2f8-139">A támogatott verziók listáját lásd: [Service Fabric-támogatás](service-fabric-support.md).</span><span class="sxs-lookup"><span data-stu-id="3a2f8-139">For a list of supported versions, see [Service Fabric support](service-fabric-support.md)</span></span>

## <a name="enable-powershell-script-execution"></a><span data-ttu-id="3a2f8-140">A PowerShell-parancsfájl végrehajtásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3a2f8-140">Enable PowerShell script execution</span></span>
<span data-ttu-id="3a2f8-141">A Service Fabric Windows PowerShell-parancsfájlokat használ a helyi fejlesztési fürtök létrehozásához és az alkalmazások Visual Studióból történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="3a2f8-141">Service Fabric uses Windows PowerShell scripts for creating a local development cluster and for deploying applications from Visual Studio.</span></span> <span data-ttu-id="3a2f8-142">Alapértelmezés szerint a Windows blokkolja ezen szkriptek futását.</span><span class="sxs-lookup"><span data-stu-id="3a2f8-142">By default, Windows blocks these scripts from running.</span></span> <span data-ttu-id="3a2f8-143">tooenable őket, módosítania kell a PowerShell végrehajtási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="3a2f8-143">tooenable them, you must modify your PowerShell execution policy.</span></span> <span data-ttu-id="3a2f8-144">Nyissa meg a Powershellt rendszergazdaként, és írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="3a2f8-144">Open PowerShell as an administrator and enter hello following command:</span></span>

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a><span data-ttu-id="3a2f8-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3a2f8-145">Next steps</span></span>
<span data-ttu-id="3a2f8-146">Most, hogy végzett a fejlesztőkörnyezet beállításával, belefoghat az alkalmazások létrehozásába és futtatásába.</span><span class="sxs-lookup"><span data-stu-id="3a2f8-146">Now that you've finished setting up your development environment, start building and running apps.</span></span>

* [<span data-ttu-id="3a2f8-147">Az első Service Fabric-alkalmazás létrehozása a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="3a2f8-147">Create your first Service Fabric application in Visual Studio</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
* [<span data-ttu-id="3a2f8-148">Megtudhatja, hogyan toodeploy és a helyi fürtön lévő alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="3a2f8-148">Learn how toodeploy and manage applications on your local cluster</span></span>](service-fabric-get-started-with-a-local-cluster.md)
* [<span data-ttu-id="3a2f8-149">További tudnivalók a programozási modellek hello: Reliable Services és Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="3a2f8-149">Learn about hello programming models: Reliable Services and Reliable Actors</span></span>](service-fabric-choose-framework.md)
* [<span data-ttu-id="3a2f8-150">Tekintse meg a hello Service Fabric mintakódjainak megtekintése a Githubon</span><span class="sxs-lookup"><span data-stu-id="3a2f8-150">Check out hello Service Fabric code samples on GitHub</span></span>](https://aka.ms/servicefabricsamples)
* [<span data-ttu-id="3a2f8-151">A fürt megjelenítése a Service Fabric Explorer segítségével</span><span class="sxs-lookup"><span data-stu-id="3a2f8-151">Visualize your cluster by using Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)
* [<span data-ttu-id="3a2f8-152">Hajtsa végre a Service Fabric képzési elérési tooget a széles körű bevezetés toohello platform hello</span><span class="sxs-lookup"><span data-stu-id="3a2f8-152">Follow hello Service Fabric learning path tooget a broad introduction toohello platform</span></span>](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* <span data-ttu-id="3a2f8-153">A [Service Fabric támogatási lehetőségeinek](service-fabric-support.md) ismertetése</span><span class="sxs-lookup"><span data-stu-id="3a2f8-153">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "A Service Fabric kampányoldala"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI-hivatkozás"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI-hivatkozás"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI-hivatkozás"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
