---
title: "egy létező végrehajtható tooAzure Service Fabric aaaDeploy |} Microsoft Docs"
description: "A forgatókönyv a hogyan toopackage egy meglévő alkalmazást, és a Vendég végrehajtható fájlja, így azok telepítve tooa Service Fabric-fürt"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/02/2017
ms.author: mfussell;mikhegn
ms.openlocfilehash: 5599802bdb6bda2407a138d77e12148ccb64f437
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-guest-executable-tooservice-fabric"></a><span data-ttu-id="b1c2a-103">Központi telepítése egy Vendég végrehajtható tooService háló</span><span class="sxs-lookup"><span data-stu-id="b1c2a-103">Deploy a guest executable tooService Fabric</span></span>
<span data-ttu-id="b1c2a-104">Az Azure Service Fabric szolgáltatásként futtatható kódok, például a Node.js, Java vagy C++ bármilyen típusú.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-104">You can run any type of code, such as Node.js, Java, or C++ in Azure Service Fabric as a service.</span></span> <span data-ttu-id="b1c2a-105">A Service Fabric toothese típusú szolgáltatások Vendég végrehajtható fájlok néven hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-105">Service Fabric refers toothese types of services as guest executables.</span></span>

<span data-ttu-id="b1c2a-106">Vendég végrehajtható fájlok állapotmentes szolgáltatásokhoz hasonlóan a Service Fabric által kell kezelni.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-106">Guest executables are treated by Service Fabric like stateless services.</span></span> <span data-ttu-id="b1c2a-107">Emiatt a fürt rendelkezésre állási és más metrikák alapján csomópontján kerülnek.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-107">As a result, they are placed on nodes in a cluster, based on availability and other metrics.</span></span> <span data-ttu-id="b1c2a-108">Ez a cikk ismerteti, hogyan toopackage és központi telepítése egy Vendég végrehajtható tooa Service Fabric-fürt, a Visual Studio vagy a parancssori segédprogram segítségével.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-108">This article describes how toopackage and deploy a guest executable tooa Service Fabric cluster, by using Visual Studio or a command-line utility.</span></span>

<span data-ttu-id="b1c2a-109">Ebben a cikkben a Microsoft hello lépéseket toopackage egy végrehajtható Vendég fedik le és telepítse azt tooService háló.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-109">In this article, we cover hello steps toopackage a guest executable and deploy it tooService Fabric.</span></span>  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a><span data-ttu-id="b1c2a-110">A Vendég a Service Fabric végrehajtható előnyei</span><span class="sxs-lookup"><span data-stu-id="b1c2a-110">Benefits of running a guest executable in Service Fabric</span></span>
<span data-ttu-id="b1c2a-111">Van több előnyeit toorunning egy Vendég végrehajtható, a Service Fabric-fürt:</span><span class="sxs-lookup"><span data-stu-id="b1c2a-111">There are several advantages toorunning a guest executable in a Service Fabric cluster:</span></span>

* <span data-ttu-id="b1c2a-112">Magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-112">High availability.</span></span> <span data-ttu-id="b1c2a-113">A Service Fabric futó alkalmazások vannak magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-113">Applications that run in Service Fabric are made highly available.</span></span> <span data-ttu-id="b1c2a-114">A Service Fabric biztosítja, hogy futnak-e egy alkalmazás példányai.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-114">Service Fabric ensures that instances of an application are running.</span></span>
* <span data-ttu-id="b1c2a-115">Állapotfigyelés.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-115">Health monitoring.</span></span> <span data-ttu-id="b1c2a-116">Service Fabric állapotfigyelésének észleli, ha egy alkalmazás fut, és diagnosztikai információkat nyújt, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-116">Service Fabric health monitoring detects if an application is running, and provides diagnostic information if there is a failure.</span></span>   
* <span data-ttu-id="b1c2a-117">Alkalmazás-életciklus kezelésének.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-117">Application lifecycle management.</span></span> <span data-ttu-id="b1c2a-118">Mellett frissítéseket biztosító állásidő nélkül, a Service Fabric automatikus visszaállítási toohello korábbi verziója biztosít, ha egy frissítés során rossz állapotesemény.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-118">Besides providing upgrades with no downtime, Service Fabric provides automatic rollback toohello previous version if there is a bad health event reported during an upgrade.</span></span>    
* <span data-ttu-id="b1c2a-119">Sűrűség.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-119">Density.</span></span> <span data-ttu-id="b1c2a-120">A fürt, amely szükségtelenné teszi hello minden alkalmazás toorun saját hardveren több alkalmazást is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-120">You can run multiple applications in a cluster, which eliminates hello need for each application toorun on its own hardware.</span></span>
* <span data-ttu-id="b1c2a-121">Felderíthetőség: Használó többi hívása hello Service Fabric Naming service toofind más szolgáltatások hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-121">Discoverability: Using REST you can call hello Service Fabric Naming service toofind other services in hello cluster.</span></span> 

## <a name="samples"></a><span data-ttu-id="b1c2a-122">Példák</span><span class="sxs-lookup"><span data-stu-id="b1c2a-122">Samples</span></span>
* [<span data-ttu-id="b1c2a-123">Minta csomagolás és központi telepítése egy Vendég végrehajtható fájl</span><span class="sxs-lookup"><span data-stu-id="b1c2a-123">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="b1c2a-124">A két Vendég végrehajtható fájlok (C# és nodejs)-en keresztül hello elnevezési szolgáltatás REST minta</span><span class="sxs-lookup"><span data-stu-id="b1c2a-124">Sample of two guest executables (C# and nodejs) communicating via hello Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a><span data-ttu-id="b1c2a-125">Alkalmazás és szolgáltatás jegyzékfájlt áttekintése</span><span class="sxs-lookup"><span data-stu-id="b1c2a-125">Overview of application and service manifest files</span></span>
<span data-ttu-id="b1c2a-126">Központi telepítése egy Vendég végrehajtható részeként hasznos toounderstand hello Service Fabric csomagolás és üzembe helyezési modellben a [alkalmazásmodell](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="b1c2a-126">As part of deploying a guest executable, it is useful toounderstand hello Service Fabric packaging and deployment model as described in [application model](service-fabric-application-model.md).</span></span> <span data-ttu-id="b1c2a-127">Service Fabric-csomagban modell hello két XML-fájlok támaszkodik: hello alkalmazás és szolgáltatás jegyzékfájljai.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-127">hello Service Fabric packaging model relies on two XML files: hello application and service manifests.</span></span> <span data-ttu-id="b1c2a-128">hello sémadefiníciót hello ApplicationManifest.xml és ServiceManifest.xml fájlok van telepítve a hello a Service Fabric SDK *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-128">hello schema definition for hello ApplicationManifest.xml and ServiceManifest.xml files is installed with hello Service Fabric SDK into *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

* <span data-ttu-id="b1c2a-129">**Az alkalmazásjegyzék** hello alkalmazásjegyzék használt toodescribe hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-129">**Application manifest** hello application manifest is used toodescribe hello application.</span></span> <span data-ttu-id="b1c2a-130">Az azt alkotó hello szolgáltatások, és hogyan egy vagy több szolgáltatás kell telepíteni, például a példányok száma hello használt toodefine más paramétereket sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-130">It lists hello services that compose it, and other parameters that are used toodefine how one or more services should be deployed, such as hello number of instances.</span></span>

  <span data-ttu-id="b1c2a-131">A Service Fabric egy alkalmazás központi telepítése és frissítése munkaegység.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-131">In Service Fabric, an application is a unit of deployment and upgrade.</span></span> <span data-ttu-id="b1c2a-132">Ahol lehetséges hibák és a potenciális visszagörgetése felügyelete egyetlen egységként alkalmazás frissítése.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-132">An application can be upgraded as a single unit where potential failures and potential rollbacks are managed.</span></span> <span data-ttu-id="b1c2a-133">A Service Fabric biztosítja, hogy a frissítési folyamat hello vagy sikeres, vagy hello adatokat sikertelen frissítés esetén, ha nem hagynak hello kérelem ismeretlen vagy instabil állapotban.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-133">Service Fabric guarantees that hello upgrade process is either successful, or, if hello upgrade fails, does not leave hello application in an unknown or unstable state.</span></span>
* <span data-ttu-id="b1c2a-134">**Szolgáltatás jegyzékfájl** hello szolgáltatás jegyzékfájl szolgáltatás hello összetevőit mutatja be.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-134">**Service manifest** hello service manifest describes hello components of a service.</span></span> <span data-ttu-id="b1c2a-135">Ez magában foglalja az adatok, például hello nevét és típusát. a szolgáltatás, és a kódjával és a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-135">It includes data, such as hello name and type of service, and its code and configuration.</span></span> <span data-ttu-id="b1c2a-136">hello szolgáltatás jegyzékfájl is néhány további paramétereket tartalmaz, amelyek lehetnek tooconfigure hello szolgáltatást használja, ha telepítve van.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-136">hello service manifest also includes some additional parameters that can be used tooconfigure hello service once it is deployed.</span></span>

## <a name="application-package-file-structure"></a><span data-ttu-id="b1c2a-137">Alkalmazás csomag fájlstruktúra</span><span class="sxs-lookup"><span data-stu-id="b1c2a-137">Application package file structure</span></span>
<span data-ttu-id="b1c2a-138">egy alkalmazás tooService háló toodeploy, hello alkalmazás egy előre meghatározott könyvtárstruktúrát kell követnie.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-138">toodeploy an application tooService Fabric, hello application should follow a predefined directory structure.</span></span> <span data-ttu-id="b1c2a-139">hello struktúra egy példa látható.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-139">hello following is an example of that structure.</span></span>

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

<span data-ttu-id="b1c2a-140">hello ApplicationPackageRoot hello alkalmazás meghatározó hello ApplicationManifest.xml fájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-140">hello ApplicationPackageRoot contains hello ApplicationManifest.xml file that defines hello application.</span></span> <span data-ttu-id="b1c2a-141">Egy alkönyvtárban hello alkalmazáshoz tartozó minden egyes szolgáltatás összes hello hello szolgáltatást igénylő összetevők használt toocontain.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-141">A subdirectory for each service included in hello application is used toocontain all hello artifacts that hello service requires.</span></span> <span data-ttu-id="b1c2a-142">Ezek alkönyvtárai hello ServiceManifest.xml, és általában a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b1c2a-142">These subdirectories are hello ServiceManifest.xml and, typically, hello following:</span></span>

* <span data-ttu-id="b1c2a-143">*Kód*.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-143">*Code*.</span></span> <span data-ttu-id="b1c2a-144">Ez a könyvtár hello szolgáltatás kódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-144">This directory contains hello service code.</span></span>
* <span data-ttu-id="b1c2a-145">*Config*. Ez a könyvtár tartalmaz egy Settings.xml fájlban (és egyéb fájlokat, ha szükséges), hogy hello szolgáltatás hozzá tud-e férni futásidejű tooretrieve megadott konfigurációs beállítások.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-145">*Config*. This directory contains a Settings.xml file (and other files if necessary) that hello service can access at runtime tooretrieve specific configuration settings.</span></span>
* <span data-ttu-id="b1c2a-146">*Adatok*.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-146">*Data*.</span></span> <span data-ttu-id="b1c2a-147">Ez az egy további könyvtár toostore további helyi hello szolgáltatást is szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-147">This is an additional directory toostore additional local data that hello service may need.</span></span> <span data-ttu-id="b1c2a-148">Az adatok használt toostore csak rövid élettartamú adatok kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-148">Data should be used toostore only ephemeral data.</span></span> <span data-ttu-id="b1c2a-149">A Service Fabric másolása vagy replicate módosítások toohello adatkönyvtára végzi, ezért ha hello szolgáltatás toobe (például a feladatátvétel során) áthelyezését.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-149">Service Fabric does not copy or replicate changes toohello data directory if hello service needs toobe relocated (for example, during failover).</span></span>

> [!NOTE]
> <span data-ttu-id="b1c2a-150">Nincs toocreate hello `config` és `data` könyvtárak, ha már nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-150">You don't have toocreate hello `config` and `data` directories if you don't need them.</span></span>
>
>

## <a name="package-an-existing-executable"></a><span data-ttu-id="b1c2a-151">Egy létező végrehajtható fájl csomag</span><span class="sxs-lookup"><span data-stu-id="b1c2a-151">Package an existing executable</span></span>
<span data-ttu-id="b1c2a-152">Amikor egy Vendég végrehajtható csomagolás, dönthet úgy, vagy toouse a Visual Studio-projektsablont vagy túl[hozzon létre manuálisan hello alkalmazáscsomag](#manually).</span><span class="sxs-lookup"><span data-stu-id="b1c2a-152">When packaging a guest executable, you can choose either toouse a Visual Studio project template or too[create hello application package manually](#manually).</span></span> <span data-ttu-id="b1c2a-153">Visual Studio használatával hello alkalmazás csomag struktúra, és a fájlok a rendszer létrehozza a hello új projektsablon által.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-153">Using Visual Studio, hello application package structure and manifest files are created by hello new project template for you.</span></span>

> [!TIP]
> <span data-ttu-id="b1c2a-154">hello legegyszerűbb módja toopackage végrehajtható a szolgáltatásba egy meglévő Windows rendszer toouse Visual Studio és a Linux toouse Yeoman</span><span class="sxs-lookup"><span data-stu-id="b1c2a-154">hello easiest way toopackage an existing Windows executable into a service is toouse Visual Studio and on Linux toouse Yeoman</span></span>
>

## <a name="use-visual-studio-toopackage-and-deploy-an-existing-executable"></a><span data-ttu-id="b1c2a-155">Visual Studio toopackage használja, és egy létező végrehajtható fájl központi telepítése</span><span class="sxs-lookup"><span data-stu-id="b1c2a-155">Use Visual Studio toopackage and deploy an existing executable</span></span>
<span data-ttu-id="b1c2a-156">A Visual Studio biztosít a Service Fabric szolgáltatás sablon toohelp Vendég végrehajtható tooa Service Fabric-fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-156">Visual Studio provides a Service Fabric service template toohelp you deploy a guest executable tooa Service Fabric cluster.</span></span>

1. <span data-ttu-id="b1c2a-157">Válasszon **fájl** > **új projekt**, és a Service Fabric-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-157">Choose **File** > **New Project**, and create a Service Fabric application.</span></span>
2. <span data-ttu-id="b1c2a-158">Válasszon **Vendég végrehajtható** hello szolgáltatássablonként.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-158">Choose **Guest Executable** as hello service template.</span></span>
3. <span data-ttu-id="b1c2a-159">Kattintson a **Tallózás** tooselect hello mappában, amelynek a végrehajtható fájl, és adja meg a többi hello hello paraméterek toocreate hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-159">Click **Browse** tooselect hello folder with your executable and fill in hello rest of hello parameters toocreate hello service.</span></span>
   * <span data-ttu-id="b1c2a-160">*Csomag viselkedés Code*.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-160">*Code Package Behavior*.</span></span> <span data-ttu-id="b1c2a-161">Lehet set toocopy a mappa toohello Visual Studio-projekt tartalmának összes hello Ez akkor hasznos, ha végrehajtható hello nem változik.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-161">Can be set toocopy all hello content of your folder toohello Visual Studio Project, which is useful if hello executable does not change.</span></span> <span data-ttu-id="b1c2a-162">Ha várhatóan hello végrehajtható toochange és hello képességét toopick mentése új buildek dinamikusan, ehelyett kiválaszthatja toolink toohello mappa.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-162">If you expect hello executable toochange and want hello ability toopick up new builds dynamically, you can choose toolink toohello folder instead.</span></span> <span data-ttu-id="b1c2a-163">Csatolt mappák hello projektet a Visual Studio létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-163">You can use linked folders when creating hello application project in Visual Studio.</span></span> <span data-ttu-id="b1c2a-164">Ez a művelet összeköti toohello forráshely a hello projektben, lehetővé téve, tooupdate hello Vendég végrehajtható a forrás cél.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-164">This links toohello source location from within hello project, making it possible for you tooupdate hello guest executable in its source destination.</span></span> <span data-ttu-id="b1c2a-165">Ezek a frissítések a build hello alkalmazáscsomag részét képezik.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-165">Those updates become part of hello application package on build.</span></span>
   * <span data-ttu-id="b1c2a-166">*Program* toostart hello szolgáltatás fusson hello végrehajtható határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-166">*Program* specifies hello executable that should be run toostart hello service.</span></span>
   * <span data-ttu-id="b1c2a-167">*Argumentumok* határozza meg a legyen átadva toohello végrehajtható hello argumentumokat.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-167">*Arguments* specifies hello arguments that should be passed toohello executable.</span></span> <span data-ttu-id="b1c2a-168">Azok a argumentumokkal paraméterek listáját.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-168">It can be a list of parameters with arguments.</span></span>
   * <span data-ttu-id="b1c2a-169">*WorkingFolder* határozza meg, amelyet lépések toobe hello folyamat hello munkakönyvtára.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-169">*WorkingFolder* specifies hello working directory for hello process that is going toobe started.</span></span> <span data-ttu-id="b1c2a-170">Három értékeket adhat meg:</span><span class="sxs-lookup"><span data-stu-id="b1c2a-170">You can specify three values:</span></span>
     * <span data-ttu-id="b1c2a-171">`CodeBase`Meghatározza, hogy hello munkakönyvtár érintetlen toobe toohello kódjának könyvtárában beállítása hello alkalmazáscsomagban (`Code` fájlstruktúra megelőző hello könyvtárral).</span><span class="sxs-lookup"><span data-stu-id="b1c2a-171">`CodeBase` specifies that hello working directory is going toobe set toohello code directory in hello application package (`Code` directory shown in hello preceding file structure).</span></span>
     * <span data-ttu-id="b1c2a-172">`CodePackage`Megadja, hogy hello munkakönyvtár érintetlen toobe beállítása toohello legfelső szintű hello alkalmazáscsomag (`GuestService1Pkg` fájlstruktúra megelőző hello látható).</span><span class="sxs-lookup"><span data-stu-id="b1c2a-172">`CodePackage` specifies that hello working directory is going toobe set toohello root of hello application package    (`GuestService1Pkg` shown in hello preceding file structure).</span></span>
     * <span data-ttu-id="b1c2a-173">`Work`Megadja, hogy a hello fájlok vannak egy munkahelyi nevű alkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-173">`Work` specifies that hello files are placed in a subdirectory called work.</span></span>
4. <span data-ttu-id="b1c2a-174">Nevezze el a szolgáltatást, és kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-174">Give your service a name, and click **OK**.</span></span>
5. <span data-ttu-id="b1c2a-175">Ha a szolgáltatás egy olyan végpont-kommunikációra van szüksége, hozzáadhat hello protokoll, port és típus toohello ServiceManifest.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-175">If your service needs an endpoint for communication, you can now add hello protocol, port, and type toohello ServiceManifest.xml file.</span></span> <span data-ttu-id="b1c2a-176">Például: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-176">For example: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.</span></span>
6. <span data-ttu-id="b1c2a-177">Mostantól hello csomag és a helyi fürt által hello megoldást a Visual Studio hibakeresési művelet közzétételt.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-177">You can now use hello package and publish action against your local cluster by debugging hello solution in Visual Studio.</span></span> <span data-ttu-id="b1c2a-178">Ha készen áll, hello alkalmazás tooa távoli fürt közzététele, vagy ellenőrizze hello megoldás toosource vezérlőben.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-178">When ready, you can publish hello application tooa remote cluster or check in hello solution toosource control.</span></span>
7. <span data-ttu-id="b1c2a-179">Nyissa meg a cikk toosee toohello végét hogyan tooview a Vendég végrehajtható szolgáltatás fut a Service Fabric Explorerben.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-179">Go toohello end of this article toosee how tooview your guest executable service running in Service Fabric Explorer.</span></span>

## <a name="use-yoeman-toopackage-and-deploy-an-existing-executable-on-linux"></a><span data-ttu-id="b1c2a-180">Yoeman toopackage használja, és egy létező végrehajtható fájl Linux telepítése</span><span class="sxs-lookup"><span data-stu-id="b1c2a-180">Use Yoeman toopackage and deploy an existing executable on Linux</span></span>

<span data-ttu-id="b1c2a-181">hello eljárás létrehozása és telepítése egy Vendég végrehajtható Linux rendszer hello ugyanaz, mint a csharp vagy java-alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-181">hello procedure for creating and deploying a guest executable on Linux is hello same as deploying a csharp or java application.</span></span>

1. <span data-ttu-id="b1c2a-182">Írja be a terminálba a következőt: `yo azuresfguest`.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-182">In a terminal, type `yo azuresfguest`.</span></span>
2. <span data-ttu-id="b1c2a-183">Adjon nevet az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-183">Name your application.</span></span>
3. <span data-ttu-id="b1c2a-184">A szolgáltatás, és adja meg hello adatait, beleértve a hello végrehajtható fájl elérési útját és hello paramétereket együtt kell meghívni.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-184">Name your service, and provide hello details including path of hello executable and hello parameters it must be invoked with.</span></span>

<span data-ttu-id="b1c2a-185">Yeoman hello megfelelő alkalmazást hoz létre egy alkalmazáscsomagot, és együtt fájlok telepítése, és távolítsa el a parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-185">Yeoman creates an application package with hello appropriate application and manifest files along with install and uninstall scripts.</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a><span data-ttu-id="b1c2a-186">Manuálisan csomag és egy létező végrehajtható fájl központi telepítése</span><span class="sxs-lookup"><span data-stu-id="b1c2a-186">Manually package and deploy an existing executable</span></span>
<span data-ttu-id="b1c2a-187">a Vendég végrehajtható manuálisan csomagolási hello folyamat alábbi általános lépésekkel hello alapul:</span><span class="sxs-lookup"><span data-stu-id="b1c2a-187">hello process of manually packaging a guest executable is based on hello following general steps:</span></span>

1. <span data-ttu-id="b1c2a-188">Hello csomag könyvtárstruktúrát létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-188">Create hello package directory structure.</span></span>
2. <span data-ttu-id="b1c2a-189">Adja hozzá a hello alkalmazás kódja és konfigurációs fájljait.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-189">Add hello application's code and configuration files.</span></span>
3. <span data-ttu-id="b1c2a-190">Hello service manifest-fájl szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-190">Edit hello service manifest file.</span></span>
4. <span data-ttu-id="b1c2a-191">Hello Alkalmazásjegyzék-fájl szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-191">Edit hello application manifest file.</span></span>

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you toocreate hello ApplicationPackage automatically. hello tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-hello-package-directory-structure"></a><span data-ttu-id="b1c2a-192">Hello csomag könyvtárstruktúrát létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1c2a-192">Create hello package directory structure</span></span>
<span data-ttu-id="b1c2a-193">Hello előző szakaszban leírtak szerint indítsa el hello könyvtárstruktúrát, létrehozásával "Alkalmazás csomag fájlstruktúra."</span><span class="sxs-lookup"><span data-stu-id="b1c2a-193">You can start by creating hello directory structure, as described in hello preceding section, "Application package file structure."</span></span>

### <a name="add-hello-applications-code-and-configuration-files"></a><span data-ttu-id="b1c2a-194">Hello alkalmazás kódja és konfigurációs fájlok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b1c2a-194">Add hello application's code and configuration files</span></span>
<span data-ttu-id="b1c2a-195">A létrehozást követően hello könyvtárstruktúrát, hello alkalmazás kódja és konfigurációs fájljait a hello kódot és a konfigurációverziókat címtárak is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-195">After you have created hello directory structure, you can add hello application's code and configuration files under hello code and config directories.</span></span> <span data-ttu-id="b1c2a-196">További címtárak vagy hello vagy konfigurációverziót könyvtárak alkönyvtárába is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-196">You can also create additional directories or subdirectories under hello code or config directories.</span></span>

<span data-ttu-id="b1c2a-197">A Service Fabric does egy `xcopy` hello tartalmának hello alkalmazás gyökérkönyvtárában, így nincs előre definiált struktúra toouse eltérő két legfontosabb könyvtárak, a kód és a beállítások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-197">Service Fabric does an `xcopy` of hello content of hello application root directory, so there is no predefined structure toouse other than creating two top directories, code and settings.</span></span> <span data-ttu-id="b1c2a-198">(Tárolhat különböző neveket, ha azt szeretné.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-198">(You can pick different names if you want.</span></span> <span data-ttu-id="b1c2a-199">További részletek szerepelnek hello következő szakaszt.)</span><span class="sxs-lookup"><span data-stu-id="b1c2a-199">More details are in hello next section.)</span></span>

> [!NOTE]
> <span data-ttu-id="b1c2a-200">Győződjön meg arról, hogy minden hello fájlok és alkalmazások igényeihez hello függőségeit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-200">Make sure that you include all hello files and dependencies that hello application needs.</span></span> <span data-ttu-id="b1c2a-201">A Service Fabric átmásolja a terjesztendő hello fürt, ahol hello alkalmazási szolgáltatások folyamatos toobe telepített hello tartalom hello alkalmazáscsomag összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-201">Service Fabric copies hello content of hello application package on all nodes in hello cluster where hello application's services are going toobe deployed.</span></span> <span data-ttu-id="b1c2a-202">hello csomagnak tartalmaznia kell, hogy hello alkalmazás toorun kell-e az összes hello kód.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-202">hello package should contain all hello code that hello application needs toorun.</span></span> <span data-ttu-id="b1c2a-203">Feltételezi azt, hogy hello függősége már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-203">Do not assume that hello dependencies are already installed.</span></span>
>
>

### <a name="edit-hello-service-manifest-file"></a><span data-ttu-id="b1c2a-204">Hello service manifest-fájl szerkesztése</span><span class="sxs-lookup"><span data-stu-id="b1c2a-204">Edit hello service manifest file</span></span>
<span data-ttu-id="b1c2a-205">hello következő lépésre tooedit hello szolgáltatás jegyzékfájl tooinclude hello a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="b1c2a-205">hello next step is tooedit hello service manifest file tooinclude hello following information:</span></span>

* <span data-ttu-id="b1c2a-206">hello hello szolgáltatás típusának neve.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-206">hello name of hello service type.</span></span> <span data-ttu-id="b1c2a-207">Ez az, hogy a Service Fabric tooidentify szolgáltatás által használt azonosító.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-207">This is an ID that Service Fabric uses tooidentify a service.</span></span>
* <span data-ttu-id="b1c2a-208">hello parancs toouse toolaunch hello alkalmazás (ExeHost).</span><span class="sxs-lookup"><span data-stu-id="b1c2a-208">hello command toouse toolaunch hello application (ExeHost).</span></span>
* <span data-ttu-id="b1c2a-209">Bármely, amelyet a toobe parancsfájlt tooset mentése hello alkalmazás (SetupEntrypoint).</span><span class="sxs-lookup"><span data-stu-id="b1c2a-209">Any script that needs toobe run tooset up hello application (SetupEntrypoint).</span></span>

<span data-ttu-id="b1c2a-210">hello az alábbiakban látható egy példa egy `ServiceManifest.xml` fájlt:</span><span class="sxs-lookup"><span data-stu-id="b1c2a-210">hello following is an example of a `ServiceManifest.xml` file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

<span data-ttu-id="b1c2a-211">a következő szakaszok hello hello különböző részeit, hogy kell-e tooupdate hello fájl ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-211">hello following sections go over hello different parts of hello file that you need tooupdate.</span></span>

#### <a name="update-servicetypes"></a><span data-ttu-id="b1c2a-212">Frissítés ServiceTypes</span><span class="sxs-lookup"><span data-stu-id="b1c2a-212">Update ServiceTypes</span></span>
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* <span data-ttu-id="b1c2a-213">Kiválaszthatja a nevét, aki a `ServiceTypeName`.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-213">You can pick any name that you want for `ServiceTypeName`.</span></span> <span data-ttu-id="b1c2a-214">hello érték szerepel hello `ApplicationManifest.xml` tooidentify hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-214">hello value is used in hello `ApplicationManifest.xml` file tooidentify hello service.</span></span>
* <span data-ttu-id="b1c2a-215">Adja meg `UseImplicitHost="true"`.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-215">Specify `UseImplicitHost="true"`.</span></span> <span data-ttu-id="b1c2a-216">Ez az attribútum a Service Fabric közli az, hogy hello szolgáltatást egy önálló alkalmazás alapul, így minden Service Fabric kell toodo toolaunch azt folyamatként és állapotának figyelése.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-216">This attribute tells Service Fabric that hello service is based on a self-contained app, so all Service Fabric needs toodo is toolaunch it as a process and monitor its health.</span></span>

#### <a name="update-codepackage"></a><span data-ttu-id="b1c2a-217">Frissítés CodePackage</span><span class="sxs-lookup"><span data-stu-id="b1c2a-217">Update CodePackage</span></span>
<span data-ttu-id="b1c2a-218">hello CodePackage elem hello helye (és verzió) hello szolgáltatást kód adja meg.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-218">hello CodePackage element specifies hello location (and version) of hello service's code.</span></span>

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

<span data-ttu-id="b1c2a-219">Hello `Name` elem hello könyvtár hello szolgáltatás kódot tartalmazó hello alkalmazáscsomagban foglalt toospecify hello nevét.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-219">hello `Name` element is used toospecify hello name of hello directory in hello application package that contains hello service's code.</span></span> <span data-ttu-id="b1c2a-220">`CodePackage`szintén tartalmazza a hello `version` attribútum.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-220">`CodePackage` also has hello `version` attribute.</span></span> <span data-ttu-id="b1c2a-221">Ez lehet hello kód használt toospecify hello verzióját, és potenciálisan is lehet tooupgrade hello szolgáltatást használja, a Service Fabric hello alkalmazás életciklusa felügyeleti infrastruktúra használatával.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-221">This can be used toospecify hello version of hello code, and can also potentially be used tooupgrade hello service's code by using hello application lifecycle management infrastructure in Service Fabric.</span></span>

#### <a name="optional-update-setupentrypoint"></a><span data-ttu-id="b1c2a-222">Választható lehetőség: Frissítés SetupEntrypoint</span><span class="sxs-lookup"><span data-stu-id="b1c2a-222">Optional: Update SetupEntrypoint</span></span>
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
<span data-ttu-id="b1c2a-223">hello SetupEntryPoint elem van használt toospecify végrehajtható vagy kötegelt fájlok hello szolgáltatást kód elindítása előtt kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-223">hello SetupEntryPoint element is used toospecify any executable or batch file that should be executed before hello service's code is launched.</span></span> <span data-ttu-id="b1c2a-224">Egy opcionális lépés, így nem kell a része, ha nincs inicializálás szükséges toobe.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-224">It is an optional step, so it does not need toobe included if there is no initialization required.</span></span> <span data-ttu-id="b1c2a-225">hello SetupEntryPoint végrehajtása minden egyes hello szolgáltatás újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-225">hello SetupEntryPoint is executed every time hello service is restarted.</span></span>

<span data-ttu-id="b1c2a-226">Csak egy SetupEntryPoint van így telepítési parancsfájlokat kell toobe csoportosítva egyetlen parancsfájlban, ha hello alkalmazás telepítéséhez több parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-226">There is only one SetupEntryPoint, so setup scripts need toobe grouped in a single batch file if hello application's setup requires multiple scripts.</span></span> <span data-ttu-id="b1c2a-227">hello SetupEntryPoint végrehajtható fájl bármilyen: végrehajtható fájlok, a parancsfájlokat és a PowerShell-parancsmagokkal.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-227">hello SetupEntryPoint can execute any type of file: executable files, batch files, and PowerShell cmdlets.</span></span> <span data-ttu-id="b1c2a-228">További részletekért lásd: [konfigurálása SetupEntryPoint](service-fabric-application-runas-security.md).</span><span class="sxs-lookup"><span data-stu-id="b1c2a-228">For more details, see [Configure SetupEntryPoint](service-fabric-application-runas-security.md).</span></span>

<span data-ttu-id="b1c2a-229">A fenti példa hello, hello SetupEntryPoint fut egy kötegfájlt nevű `LaunchConfig.cmd` , amely a található hello `scripts` alkönyvtárába hello kódot (feltéve, hogy hello WorkingFolder elem értéke tooCodeBase).</span><span class="sxs-lookup"><span data-stu-id="b1c2a-229">In hello preceding example, hello SetupEntryPoint runs a batch file called `LaunchConfig.cmd` that is located in hello `scripts` subdirectory of hello code directory (assuming hello WorkingFolder element is set tooCodeBase).</span></span>

#### <a name="update-entrypoint"></a><span data-ttu-id="b1c2a-230">Frissítés belépési pont</span><span class="sxs-lookup"><span data-stu-id="b1c2a-230">Update EntryPoint</span></span>
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

<span data-ttu-id="b1c2a-231">Hello `EntryPoint` hello szolgáltatás jegyzékfájl eleme használt toospecify hogyan toolaunch hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-231">hello `EntryPoint` element in hello service manifest file is used toospecify how toolaunch hello service.</span></span> <span data-ttu-id="b1c2a-232">Hello `ExeHost` elem végrehajtható hello (és az argumentumok) meghatározza, hogy toolaunch hello szolgáltatást használja.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-232">hello `ExeHost` element specifies hello executable (and arguments) that should be used toolaunch hello service.</span></span>

* <span data-ttu-id="b1c2a-233">`Program`Megadja a hello hello végrehajtható fájl nevét, kezdő hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-233">`Program` specifies hello name of hello executable that should start hello service.</span></span>
* <span data-ttu-id="b1c2a-234">`Arguments`Adja meg a legyen átadva toohello végrehajtható hello argumentumokat.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-234">`Arguments` specifies hello arguments that should be passed toohello executable.</span></span> <span data-ttu-id="b1c2a-235">Azok a argumentumokkal paraméterek listáját.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-235">It can be a list of parameters with arguments.</span></span>
* <span data-ttu-id="b1c2a-236">`WorkingFolder`Adja meg, amelyet lépések toobe hello folyamat hello munkakönyvtára.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-236">`WorkingFolder` specifies hello working directory for hello process that is going toobe started.</span></span> <span data-ttu-id="b1c2a-237">Három értékeket adhat meg:</span><span class="sxs-lookup"><span data-stu-id="b1c2a-237">You can specify three values:</span></span>
  * <span data-ttu-id="b1c2a-238">`CodeBase`Meghatározza, hogy hello munkakönyvtár érintetlen toobe toohello kódjának könyvtárában beállítása hello alkalmazáscsomagban (`Code` könyvtárban lévő fájlstruktúra megelőző hello).</span><span class="sxs-lookup"><span data-stu-id="b1c2a-238">`CodeBase` specifies that hello working directory is going toobe set toohello code directory in hello application package (`Code` directory in hello preceding file structure).</span></span>
  * <span data-ttu-id="b1c2a-239">`CodePackage`Megadja, hogy hello munkakönyvtár érintetlen toobe beállítása toohello legfelső szintű hello alkalmazáscsomag (`GuestService1Pkg` a fájlstruktúra megelőző hello).</span><span class="sxs-lookup"><span data-stu-id="b1c2a-239">`CodePackage` specifies that hello working directory is going toobe set toohello root of hello application package    (`GuestService1Pkg` in hello preceding file structure).</span></span>
    * <span data-ttu-id="b1c2a-240">`Work`Megadja, hogy a hello fájlok vannak egy munkahelyi nevű alkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-240">`Work` specifies that hello files are placed in a subdirectory called work.</span></span>

<span data-ttu-id="b1c2a-241">hello WorkingFolder lesz hasznos tooset hello megfelelő munkakönyvtár, így a relatív elérési utak vagy hello inicializálása vagy alkalmazás parancsfájlok által használható.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-241">hello WorkingFolder is useful tooset hello correct working directory so that relative paths can be used by either hello application or initialization scripts.</span></span>

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a><span data-ttu-id="b1c2a-242">Végpontok frissítése és a kommunikációs szolgáltatás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="b1c2a-242">Update Endpoints and register with Naming Service for communication</span></span>
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
<span data-ttu-id="b1c2a-243">A fenti példa hello, hello `Endpoint` elem hello alkalmazás figyelheti a hello végpontokhoz határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-243">In hello preceding example, hello `Endpoint` element specifies hello endpoints that hello application can listen on.</span></span> <span data-ttu-id="b1c2a-244">Ebben a példában a Node.js-alkalmazás hello port 3000 HTTP-figyeli.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-244">In this example, hello Node.js application listens on http on port 3000.</span></span>

<span data-ttu-id="b1c2a-245">Továbbá megkérheti a Service Fabric toopublish a végpont toohello Naming Service, más szolgáltatásokkal hello végpont címe toothis szolgáltatás képes felderíteni.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-245">Furthermore you can ask Service Fabric toopublish this endpoint toohello Naming Service so other services can discover hello endpoint address toothis service.</span></span> <span data-ttu-id="b1c2a-246">Ez lehetővé teszi toobe képes toocommunicate Vendég futtatható szolgáltatások között.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-246">This enables you toobe able toocommunicate between services that are guest executables.</span></span>
<span data-ttu-id="b1c2a-247">hello közzétett végpont címe hello űrlap `UriScheme://IPAddressOrFQDN:Port/PathSuffix`.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-247">hello published endpoint address is of hello form `UriScheme://IPAddressOrFQDN:Port/PathSuffix`.</span></span> <span data-ttu-id="b1c2a-248">`UriScheme`és `PathSuffix` nem kötelező attribútum.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-248">`UriScheme` and `PathSuffix` are optional attributes.</span></span> <span data-ttu-id="b1c2a-249">`IPAddressOrFQDN`hello IP-cím vagy a végrehajtható fájl helyezve lekérdezi hello csomópont teljesen minősített tartománynevét, de az Ön számítja ki.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-249">`IPAddressOrFQDN` is hello IP address or fully qualified domain name of hello node this executable gets placed on, and it is calculated for you.</span></span>

<span data-ttu-id="b1c2a-250">A hello következő példának, egyszer hello szolgáltatás telepítve van a Service Fabric Explorerben úgy, hogy hasonló végpont túl`http://10.1.4.92:3000/myapp/` hello szolgáltatáspéldány közzé.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-250">In hello following example, once hello service is deployed, in Service Fabric Explorer you see an endpoint similar too`http://10.1.4.92:3000/myapp/` published for hello service instance.</span></span> <span data-ttu-id="b1c2a-251">Vagy ha a helyi számítógépen, látható `http://localhost:3000/myapp/`.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-251">Or if this is a local machine, you see `http://localhost:3000/myapp/`.</span></span>

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
<span data-ttu-id="b1c2a-252">Ezekre a címekre használható [fordított proxy](service-fabric-reverseproxy.md) toocommunicate szolgáltatások között.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-252">You can use these addresses with [reverse proxy](service-fabric-reverseproxy.md) toocommunicate between services.</span></span>

### <a name="edit-hello-application-manifest-file"></a><span data-ttu-id="b1c2a-253">Hello Alkalmazásjegyzék-fájl szerkesztése</span><span class="sxs-lookup"><span data-stu-id="b1c2a-253">Edit hello application manifest file</span></span>
<span data-ttu-id="b1c2a-254">Miután konfigurálta a hello `Servicemanifest.xml` fájl, toomake egyes módosítások toohello kell `ApplicationManifest.xml` fájlt, amely hello tooensure helyes-e használni a szolgáltatás típusa és neve.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-254">Once you have configured hello `Servicemanifest.xml` file, you need toomake some changes toohello `ApplicationManifest.xml` file tooensure that hello correct service type and name are used.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a><span data-ttu-id="b1c2a-255">ServiceManifestImport</span><span class="sxs-lookup"><span data-stu-id="b1c2a-255">ServiceManifestImport</span></span>
<span data-ttu-id="b1c2a-256">A hello `ServiceManifestImport` elem, megadhatja, hogy szeretné-e tooinclude hello alkalmazásban egy vagy több szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-256">In hello `ServiceManifestImport` element, you can specify one or more services that you want tooinclude in hello app.</span></span> <span data-ttu-id="b1c2a-257">A hivatkozott szolgáltatások `ServiceManifestName`, amely adja meg hello hello könyvtár nevét, ahol hello `ServiceManifest.xml` -fájl.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-257">Services are referenced with `ServiceManifestName`, which specifies hello name of hello directory where hello `ServiceManifest.xml` file is located.</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a><span data-ttu-id="b1c2a-258">A naplózás beállítása</span><span class="sxs-lookup"><span data-stu-id="b1c2a-258">Set up logging</span></span>
<span data-ttu-id="b1c2a-259">Az Vendég végrehajtható hasznos toobe képes toosee konzol naplók toofind ki, ha alkalmazás- és konfigurációs parancsfájlokat hello hibák megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-259">For guest executables, it is useful toobe able toosee console logs toofind out if hello application and configuration scripts show any errors.</span></span>
<span data-ttu-id="b1c2a-260">Hello segítségével konfigurálható `ServiceManifest.xml` hello fájl `ConsoleRedirection` elemet.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-260">Console redirection can be configured in hello `ServiceManifest.xml` file using hello `ConsoleRedirection` element.</span></span>

> [!WARNING]
> <span data-ttu-id="b1c2a-261">Az alkalmazásban, mert ez hatással lehet a hello alkalmazás feladatátvételi éles környezetben telepített soha ne használja a hello konzol átirányítási házirend.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-261">Never use hello console redirection policy in an application that is deployed in production because this can affect hello application failover.</span></span> <span data-ttu-id="b1c2a-262">*Csak* használja ezt a helyi fejlesztési és hibakeresési célra.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-262">*Only* use this for local development and debugging purposes.</span></span>  
>
>

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

<span data-ttu-id="b1c2a-263">`ConsoleRedirection`használt tooredirect konzol (az stdout és az stderr) kimeneti tooa munkakönyvtár lehet.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-263">`ConsoleRedirection` can be used tooredirect console output (both stdout and stderr) tooa working directory.</span></span> <span data-ttu-id="b1c2a-264">Ez itt hello képességét tooverify, hogy nincsenek-e hibák hello telepítése vagy a Service Fabric-fürt hello hello alkalmazás végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-264">This provides hello ability tooverify that there are no errors during hello setup or execution of hello application in hello Service Fabric cluster.</span></span>

<span data-ttu-id="b1c2a-265">`FileRetentionCount`azt határozza meg, hány fájl hello munkakönyvtár lesznek mentve.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-265">`FileRetentionCount` determines how many files are saved in hello working directory.</span></span> <span data-ttu-id="b1c2a-266">5-öt, például azt jelenti, hogy a hello előző öt végrehajtások hello naplófájlokban hello munkakönyvtár vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-266">A value of 5, for example, means that hello log files for hello previous five executions are stored in hello working directory.</span></span>

<span data-ttu-id="b1c2a-267">`FileMaxSizeInKb`hello hello naplófájlok maximális méretét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-267">`FileMaxSizeInKb` specifies hello maximum size of hello log files.</span></span>

<span data-ttu-id="b1c2a-268">Naplófájlok mentése hello szolgáltatás használatának könyvtárak egyikében.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-268">Log files are saved in one of hello service's working directories.</span></span> <span data-ttu-id="b1c2a-269">toodetermine, ahol hello fájlok találhatók, használja a Service Fabric Explorer toodetermine melyik csomópont hello szolgáltatás fut, és melyik munkakönyvtár használatban van.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-269">toodetermine where hello files are located, use Service Fabric Explorer toodetermine which node hello service is running on, and which working directory is being used.</span></span> <span data-ttu-id="b1c2a-270">Ez a folyamat a cikk vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-270">This process is covered later in this article.</span></span>

## <a name="deployment"></a><span data-ttu-id="b1c2a-271">Környezet</span><span class="sxs-lookup"><span data-stu-id="b1c2a-271">Deployment</span></span>
<span data-ttu-id="b1c2a-272">hello utolsó lépése túl van[az alkalmazás központi telepítése](service-fabric-deploy-remove-applications.md).</span><span class="sxs-lookup"><span data-stu-id="b1c2a-272">hello last step is too[deploy your application](service-fabric-deploy-remove-applications.md).</span></span> <span data-ttu-id="b1c2a-273">a következő PowerShell-parancsfájl bemutatja hogyan hello toodeploy a alkalmazás toohello helyi fejlesztési fürtöt, és egy új Service Fabric-szolgáltatás elindításához.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-273">hello following PowerShell script shows how toodeploy your application toohello local development cluster, and start a new Service Fabric service.</span></span>

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```

>[!TIP]
> <span data-ttu-id="b1c2a-274">[Hello csomag tömörítése](service-fabric-package-apps.md#compress-a-package) toohello lemezképtárolóhoz másolás, ha hello csomag nagy, vagy sok fájl van előtt.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-274">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store if hello package is large or has many files.</span></span> <span data-ttu-id="b1c2a-275">További [Itt](service-fabric-deploy-remove-applications.md#upload-the-application-package).</span><span class="sxs-lookup"><span data-stu-id="b1c2a-275">Read more [here](service-fabric-deploy-remove-applications.md#upload-the-application-package).</span></span>
>

<span data-ttu-id="b1c2a-276">A Service Fabric-szolgáltatás telepíthető különböző "konfigurációkban."</span><span class="sxs-lookup"><span data-stu-id="b1c2a-276">A Service Fabric service can be deployed in various "configurations."</span></span> <span data-ttu-id="b1c2a-277">Például, egy vagy több példányát is telepíthető, vagy úgy, hogy nincs-e hello szolgáltatás hello Service Fabric-fürt mindegyik csomópontján több példánya is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-277">For example, it can be deployed as single or multiple instances, or it can be deployed in such a way that there is one instance of hello service on each node of hello Service Fabric cluster.</span></span>

<span data-ttu-id="b1c2a-278">Hello `InstanceCount` hello paramétere `New-ServiceFabricService` parancsmag használt toospecify kell indítani a Service Fabric-fürt hello hello szolgáltatás hány példánya.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-278">hello `InstanceCount` parameter of hello `New-ServiceFabricService` cmdlet is used toospecify how many instances of hello service should be launched in hello Service Fabric cluster.</span></span> <span data-ttu-id="b1c2a-279">Beállíthatja a hello `InstanceCount` érték, amely elérhetővé tett alkalmazás hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-279">You can set hello `InstanceCount` value, depending on hello type of application that you are deploying.</span></span> <span data-ttu-id="b1c2a-280">a két hello leggyakoribb forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="b1c2a-280">hello two most common scenarios are:</span></span>

* <span data-ttu-id="b1c2a-281">`InstanceCount = "1"`.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-281">`InstanceCount = "1"`.</span></span> <span data-ttu-id="b1c2a-282">Ebben az esetben hello fürt hello szolgáltatás csak egy példánya van telepítve.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-282">In this case, only one instance of hello service is deployed in hello cluster.</span></span> <span data-ttu-id="b1c2a-283">A Service Fabric-ütemező meghatározza, hogy melyik csomópont hello szolgáltatás toobe telepítve lesz.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-283">Service Fabric's scheduler determines which node hello service is going toobe deployed on.</span></span>
* <span data-ttu-id="b1c2a-284">`InstanceCount ="-1"`.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-284">`InstanceCount ="-1"`.</span></span> <span data-ttu-id="b1c2a-285">Ebben az esetben hello Service Fabric-fürt minden csomópontja hello szolgáltatás egy példánya van telepítve.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-285">In this case, one instance of hello service is deployed on every node in hello Service Fabric cluster.</span></span> <span data-ttu-id="b1c2a-286">hello eredmény az egyes csomópontok hello szolgáltatás egy (és csak egy) példánya nehézségekkel hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-286">hello result is having one (and only one) instance of hello service for each node in hello cluster.</span></span>

<span data-ttu-id="b1c2a-287">Ennek oka az előtér-alkalmazás (például egy REST-végpont), egy hasznos konfigurációjának ügyfélalkalmazások kell túl "Csatlakozás" tooany hello fürt toouse hello végpont hello csomópontot.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-287">This is a useful configuration for front-end applications (for example, a REST endpoint), because client applications need too"connect" tooany of hello nodes in hello cluster toouse hello endpoint.</span></span> <span data-ttu-id="b1c2a-288">Ez a konfiguráció is használható, ha, például hello Service Fabric-fürt összes csomópontja csatlakoztatott tooa terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-288">This configuration can also be used when, for example, all nodes of hello Service Fabric cluster are connected tooa load balancer.</span></span> <span data-ttu-id="b1c2a-289">Ügyfél forgalmát a Hozzáadás után terjeszthetők hello szolgáltatásban futó hello fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-289">Client traffic can then be distributed across hello service that is running on all nodes in hello cluster.</span></span>

## <a name="check-your-running-application"></a><span data-ttu-id="b1c2a-290">Ellenőrizze, fut-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b1c2a-290">Check your running application</span></span>
<span data-ttu-id="b1c2a-291">A Service Fabric Explorerben hello szolgáltatást futtató hello csomópont azonosításához.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-291">In Service Fabric Explorer, identify hello node where hello service is running.</span></span> <span data-ttu-id="b1c2a-292">Ebben a példában az fut az 1. csomópont:</span><span class="sxs-lookup"><span data-stu-id="b1c2a-292">In this example, it runs on Node1:</span></span>

![Csomópont, ahol a szolgáltatás fut.](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

<span data-ttu-id="b1c2a-294">Ha toohello csomópont keresse meg, és keresse meg az alkalmazás toohello, lásd: hello alapvető csomópont információt, beleértve a helye a lemezen.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-294">If you navigate toohello node and browse toohello application, you see hello essential node information, including its location on disk.</span></span>

![Hely a lemezen](./media/service-fabric-deploy-existing-app/locationondisk2.png)

<span data-ttu-id="b1c2a-296">Toohello directory Server Explorer használatával keresse meg, ha található hello munkakönyvtár és hello szolgáltatás naplómappában, ahogy az alábbi képernyőfelvétel a hello:</span><span class="sxs-lookup"><span data-stu-id="b1c2a-296">If you browse toohello directory by using Server Explorer, you can find hello working directory and hello service's log folder, as shown in hello following screenshot:</span></span> 

![Napló helye](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a><span data-ttu-id="b1c2a-298">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b1c2a-298">Next steps</span></span>
<span data-ttu-id="b1c2a-299">Ebben a cikkben megtanulta, hogyan toopackage egy Vendég végrehajtható fájlt, és telepítse azt tooService háló.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-299">In this article, you have learned how toopackage a guest executable and deploy it tooService Fabric.</span></span> <span data-ttu-id="b1c2a-300">Tekintse meg a következő cikkeket összegyűjtő kapcsolódó információkat és feladatokat hello.</span><span class="sxs-lookup"><span data-stu-id="b1c2a-300">See hello following articles for related information and tasks.</span></span>

* <span data-ttu-id="b1c2a-301">[Minta csomagolás és központi telepítése egy Vendég végrehajtható](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), beleértve a hivatkozás toohello előzetes verzióját hello csomagolás eszköz</span><span class="sxs-lookup"><span data-stu-id="b1c2a-301">[Sample for packaging and deploying a guest executable](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), including a link toohello prerelease of hello packaging tool</span></span>
* [<span data-ttu-id="b1c2a-302">A két Vendég végrehajtható fájlok (C# és nodejs)-en keresztül hello elnevezési szolgáltatás REST minta</span><span class="sxs-lookup"><span data-stu-id="b1c2a-302">Sample of two guest executables (C# and nodejs) communicating via hello Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
* [<span data-ttu-id="b1c2a-303">Több futtatható vendégalkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="b1c2a-303">Deploy multiple guest executables</span></span>](service-fabric-deploy-multiple-apps.md)
* [<span data-ttu-id="b1c2a-304">A Visual Studio használatával első Service Fabric-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1c2a-304">Create your first Service Fabric application using Visual Studio</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
