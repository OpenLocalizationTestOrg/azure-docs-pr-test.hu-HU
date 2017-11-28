---
title: "aaaManage az alkalmazások, a Visual Studio |} Microsoft Docs"
description: "Használja a Visual Studio toocreate, elkészítéséhez csomag, telepítése és hibakeresése a Service Fabric-alkalmazásokhoz és szolgáltatásokhoz."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: c317cb7e-7eae-466e-ba41-6aa2518be5cf
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: b2d5803d85e4f9645dcbece33a2208bc0955498d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-visual-studio-toosimplify-writing-and-managing-your-service-fabric-applications"></a><span data-ttu-id="824be-103">Visual Studio toosimplify írása, és a Service Fabric-alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="824be-103">Use Visual Studio toosimplify writing and managing your Service Fabric applications</span></span>
<span data-ttu-id="824be-104">Az Azure Service Fabric-alkalmazások és szolgáltatások Visual Studio alkalmazással kezelheti.</span><span class="sxs-lookup"><span data-stu-id="824be-104">You can manage your Azure Service Fabric applications and services through Visual Studio.</span></span> <span data-ttu-id="824be-105">Miután megismerte [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md), használja a Visual Studio toocreate Service Fabric-alkalmazások, adja hozzá a szolgáltatásokhoz, vagy a csomag, regisztrálása és alkalmazások telepítése a helyi fejlesztési fürtöt.</span><span class="sxs-lookup"><span data-stu-id="824be-105">Once you've [set up your development environment](service-fabric-get-started.md), you can use Visual Studio toocreate Service Fabric applications, add services, or package, register, and deploy applications in your local development cluster.</span></span>

## <a name="deploy-your-service-fabric-application"></a><span data-ttu-id="824be-106">A Service Fabric-alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="824be-106">Deploy your Service Fabric application</span></span>
<span data-ttu-id="824be-107">Alapértelmezés szerint az alkalmazás központi telepítése egyesíti a lépéseket követve egy egyszerű üzembe hello:</span><span class="sxs-lookup"><span data-stu-id="824be-107">By default, deploying an application combines hello following steps into one simple operation:</span></span>

1. <span data-ttu-id="824be-108">Hello alkalmazáscsomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="824be-108">Creating hello application package</span></span>
2. <span data-ttu-id="824be-109">Hello alkalmazás csomag toohello lemezképtárolóhoz feltöltése</span><span class="sxs-lookup"><span data-stu-id="824be-109">Uploading hello application package toohello image store</span></span>
3. <span data-ttu-id="824be-110">Hello alkalmazástípus regisztrálása</span><span class="sxs-lookup"><span data-stu-id="824be-110">Registering hello application type</span></span>
4. <span data-ttu-id="824be-111">Eltávolítani a alkalmazáspéldányok fut</span><span class="sxs-lookup"><span data-stu-id="824be-111">Removing any running application instances</span></span>
5. <span data-ttu-id="824be-112">Az alkalmazáspéldány létrehozása</span><span class="sxs-lookup"><span data-stu-id="824be-112">Creating an application instance</span></span>

<span data-ttu-id="824be-113">A Visual Studio lenyomásával **F5** telepíti az alkalmazást, és csatlakoztassa a hello hibakereső tooall alkalmazáspéldányok.</span><span class="sxs-lookup"><span data-stu-id="824be-113">In Visual Studio, pressing **F5** deploys your application and attach hello debugger tooall application instances.</span></span> <span data-ttu-id="824be-114">Használhat **Ctrl + F5** toodeploy anélkül, hogy hibakeresés, vagy akkor tehet közzé tooa helyi vagy távoli fürtre hello közzétételi profil.</span><span class="sxs-lookup"><span data-stu-id="824be-114">You can use **Ctrl+F5** toodeploy an application without debugging, or you can publish tooa local or remote cluster by using hello publish profile.</span></span> <span data-ttu-id="824be-115">További információkért lásd: [az alkalmazás tooa távoli fürt közzététele a Visual Studio használatával](service-fabric-publish-app-remote-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="824be-115">For more information, see [Publish an application tooa remote cluster by using Visual Studio](service-fabric-publish-app-remote-cluster.md).</span></span>

### <a name="application-debug-mode"></a><span data-ttu-id="824be-116">Alkalmazás hibakeresési módban</span><span class="sxs-lookup"><span data-stu-id="824be-116">Application Debug Mode</span></span>
<span data-ttu-id="824be-117">A Visual Studio adja meg a tulajdonságot, **alkalmazás hibakeresési módban**, amely meghatározza, hogy a hibakeresés részeként Visual Studios toohandle alkalmazás központi telepítésének módját.</span><span class="sxs-lookup"><span data-stu-id="824be-117">Visual Studio provide a property called **Application Debug Mode**, which controls how you want Visual Studios toohandle Application deployment as part of debugging.</span></span>

#### <a name="tooset-hello-application-debug-mode-property"></a><span data-ttu-id="824be-118">tooset hello alkalmazás hibakeresési módban tulajdonság</span><span class="sxs-lookup"><span data-stu-id="824be-118">tooset hello Application Debug Mode property</span></span>
1. <span data-ttu-id="824be-119">Hello Service Fabric application projekt (*.sfproj) a helyi menüben válassza a **tulajdonságok** (vagy nyomja le az hello **F4** kulcs).</span><span class="sxs-lookup"><span data-stu-id="824be-119">On hello Service Fabric application project's (*.sfproj) shortcut menu, choose **Properties** (or press hello **F4** key).</span></span>
2. <span data-ttu-id="824be-120">A hello **tulajdonságok** ablakban, a set hello **alkalmazás hibakeresési módban** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="824be-120">In hello **Properties** window, set hello **Application Debug Mode** property.</span></span>

![Állítsa be az alkalmazás hibakeresési módban tulajdonság][debugmodeproperty]

#### <a name="application-debug-modes"></a><span data-ttu-id="824be-122">Alkalmazás hibakeresési üzemmód</span><span class="sxs-lookup"><span data-stu-id="824be-122">Application Debug Modes</span></span>

1. <span data-ttu-id="824be-123">**Frissítse az alkalmazás** ebben a módban tooquickly változás lehetővé teszi, és hibakeresése a kód és a statikus webes fájlok szerkesztésének hibakeresés során támogatja.</span><span class="sxs-lookup"><span data-stu-id="824be-123">**Refresh Application** This mode enables you tooquickly change and debug your code and supports editing static web files while debugging.</span></span> <span data-ttu-id="824be-124">Ebben az üzemmódban csak akkor működik, ha a helyi fejlesztési fürtök [1-csomópont mód](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).</span><span class="sxs-lookup"><span data-stu-id="824be-124">This mode only works if your local development cluster is in [1-Node mode](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).</span></span>
2. <span data-ttu-id="824be-125">**Távolítsa el az alkalmazás** okok hello alkalmazás toobe távolítja el, amikor hello hibakeresési munkamenet azért ér véget.</span><span class="sxs-lookup"><span data-stu-id="824be-125">**Remove Application** causes hello application toobe removed when hello debug session ends.</span></span>
3. <span data-ttu-id="824be-126">**Automatikus frissítés** hello alkalmazás toorun továbbra is, amikor hello hibakeresési munkamenet befejeződik.</span><span class="sxs-lookup"><span data-stu-id="824be-126">**Auto Upgrade** hello application continues toorun when hello debug session ends.</span></span> <span data-ttu-id="824be-127">hello következő hibakeresési munkamenet kezeli hello telepítési frissítéseként.</span><span class="sxs-lookup"><span data-stu-id="824be-127">hello next debug session will treat hello deployment as an upgrade.</span></span> <span data-ttu-id="824be-128">hello frissítési folyamat megőrzi a korábbi hibakeresési munkamenetben megadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="824be-128">hello upgrade process preserves any data that you entered in a previous debug session.</span></span>
4. <span data-ttu-id="824be-129">**Alkalmazás megtartása** hello alkalmazás tartja hello fürt hello futó debug munkamenete véget nem ér.</span><span class="sxs-lookup"><span data-stu-id="824be-129">**Keep Application** hello application keeps running in hello cluster when hello debug session ends.</span></span> <span data-ttu-id="824be-130">Hello elején hello következő hibakeresési munkamenetben, hello alkalmazás el lesz távolítva.</span><span class="sxs-lookup"><span data-stu-id="824be-130">At hello beginning of hello next debug session, hello application will be removed.</span></span>

<span data-ttu-id="824be-131">A **automatikus frissítése** adatok megmaradjanak hello alkalmazás frissítési lehetőségeit a Service Fabric alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="824be-131">For **Auto Upgrade** data is preserved by applying hello application upgrade capabilities of Service Fabric.</span></span> <span data-ttu-id="824be-132">Alkalmazások, és hogyan lehet, hogy frissíti az valós környezetben történő frissítéssel kapcsolatos további információkért lásd: [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="824be-132">For more information about upgrading applications and how you might perform an upgrade in a real environment, see [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

## <a name="add-a-service-tooyour-service-fabric-application"></a><span data-ttu-id="824be-133">A szolgáltatás tooyour Service Fabric-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="824be-133">Add a service tooyour Service Fabric application</span></span>
<span data-ttu-id="824be-134">Új szolgáltatások tooyour alkalmazás tooextend funkciókat adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="824be-134">You can add new services tooyour application tooextend its functionality.</span></span>  <span data-ttu-id="824be-135">hogy hello szolgáltatás szerepel-e az alkalmazáscsomag tooensure hozzáadása hello szolgáltatást keresztül hello **új Fabric-szolgáltatás...**  menüpont.</span><span class="sxs-lookup"><span data-stu-id="824be-135">tooensure that hello service is included in your application package, add hello service through hello **New Fabric Service...** menu item.</span></span>

![Egy új Service Fabric-szolgáltatás hozzáadása][newservice]

<span data-ttu-id="824be-137">Jelöljön ki egy Service Fabric projekt típus tooadd tooyour alkalmazást, és adjon meg egy nevet hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="824be-137">Select a Service Fabric project type tooadd tooyour application, and specify a name for hello service.</span></span>  <span data-ttu-id="824be-138">Lásd: [keretrendszere, amely a szolgáltatás kiválasztása](service-fabric-choose-framework.md) úgy dönt, hogy melyik szolgáltatás toohelp toouse írja be.</span><span class="sxs-lookup"><span data-stu-id="824be-138">See [Choosing a framework for your service](service-fabric-choose-framework.md) toohelp you decide which service type toouse.</span></span>

![Jelöljön ki egy Service Fabric szolgáltatás projekt típus tooadd tooyour alkalmazást][addserviceproject]

<span data-ttu-id="824be-140">hello új szolgáltatás tooyour megoldás és a meglévő alkalmazáscsomag hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="824be-140">hello new service is added tooyour solution and existing application package.</span></span> <span data-ttu-id="824be-141">hello szolgáltatási hivatkozást lekérni, és egy alapértelmezett szolgáltatáspéldány hozzáadott toohello alkalmazásjegyzék, ezért hello szolgáltatás toobe létrehozott, és elindította hello hello alkalmazás következő indításakor.</span><span class="sxs-lookup"><span data-stu-id="824be-141">hello service references and a default service instance will be added toohello application manifest, causing hello service toobe created and started hello next time you deploy hello application.</span></span>

![hello új szolgáltatás tooyour alkalmazásjegyzék hozzáadása][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a><span data-ttu-id="824be-143">A Service Fabric-alkalmazás becsomagolása</span><span class="sxs-lookup"><span data-stu-id="824be-143">Package your Service Fabric application</span></span>
<span data-ttu-id="824be-144">toodeploy hello alkalmazás és a szolgáltatások tooa fürtje kell toocreate egy alkalmazáscsomagot.</span><span class="sxs-lookup"><span data-stu-id="824be-144">toodeploy hello application and its services tooa cluster, you need toocreate an application package.</span></span>  <span data-ttu-id="824be-145">hello csomag rendszerezi hello alkalmazásjegyzék szolgáltatás jegyzékfájlban és egyéb szükséges fájlok egy adott elrendezésben.</span><span class="sxs-lookup"><span data-stu-id="824be-145">hello package organizes hello application manifest, service manifests, and other necessary files in a specific layout.</span></span>  <span data-ttu-id="824be-146">A Visual Studio állít be, és hello alkalmazás projekt mappájában hello "pkg" directory hello csomag kezeli.</span><span class="sxs-lookup"><span data-stu-id="824be-146">Visual Studio sets up and manages hello package in hello application project's folder, in hello 'pkg' directory.</span></span>  <span data-ttu-id="824be-147">Kattintson a **csomag** a hello **alkalmazás** helyi menü hoz létre vagy frissítések hello alkalmazáscsomagot.</span><span class="sxs-lookup"><span data-stu-id="824be-147">Clicking **Package** from hello **Application** context menu creates or updates hello application package.</span></span>

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a><span data-ttu-id="824be-148">Távolítsa el az alkalmazások és a Cloud Explorer használatával alkalmazástípusok</span><span class="sxs-lookup"><span data-stu-id="824be-148">Remove applications and application types using Cloud Explorer</span></span>
<span data-ttu-id="824be-149">Műveleteket hajthat végre alapszintű fürt felügyeleti belül a Visual Studio használatával indítja el a hello Cloud Explorerben **nézet** menü.</span><span class="sxs-lookup"><span data-stu-id="824be-149">You can perform basic cluster management operations from within Visual Studio using Cloud Explorer, which you can launch from hello **View** menu.</span></span> <span data-ttu-id="824be-150">Például alkalmazások törlése és leépíteni a következőt: alkalmazástípusok helyi vagy távoli fürtökön.</span><span class="sxs-lookup"><span data-stu-id="824be-150">For instance, you can delete applications and unprovision application types on local or remote clusters.</span></span>

![Alkalmazások eltávolítása][removeapplication]

> [!TIP]
> <span data-ttu-id="824be-152">Több fürt kezelési funkcióját, lásd: [a fürt megjelenítése a Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="824be-152">For richer cluster management functionality, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
>
>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="824be-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="824be-153">Next steps</span></span>
* [<span data-ttu-id="824be-154">A Service Fabric alkalmazásmodellt.</span><span class="sxs-lookup"><span data-stu-id="824be-154">Service Fabric application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="824be-155">A Service Fabric-alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="824be-155">Service Fabric application deployment</span></span>](service-fabric-deploy-remove-applications.md)
* [<span data-ttu-id="824be-156">Alkalmazás paramétereinek több környezet kezelése</span><span class="sxs-lookup"><span data-stu-id="824be-156">Managing application parameters for multiple environments</span></span>](service-fabric-manage-multiple-environment-app-configuration.md)
* [<span data-ttu-id="824be-157">A Service Fabric-alkalmazás hibakeresés</span><span class="sxs-lookup"><span data-stu-id="824be-157">Debugging your Service Fabric application</span></span>](service-fabric-debugging-your-application.md)
* [<span data-ttu-id="824be-158">A fürt megjelenítése a Service Fabric Explorer használatával</span><span class="sxs-lookup"><span data-stu-id="824be-158">Visualizing your cluster by using Service Fabric Explorer</span></span>](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png