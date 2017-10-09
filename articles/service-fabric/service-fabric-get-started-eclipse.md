---
title: "a Service Fabric Eclipse beépülő modul aaaAzure |} Microsoft Docs"
description: "Ismerkedés a Service Fabric beépülő modul hello az eclipse-ben."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/21/2016
ms.author: saysa
ms.openlocfilehash: 4ba5a28a6282387249a2bd4e62314e891ff04162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a><span data-ttu-id="96dca-103">Az Eclipse Service Fabric beépülő moduljának Java alkalmazásfejlesztése</span><span class="sxs-lookup"><span data-stu-id="96dca-103">Service Fabric plug-in for Eclipse Java application development</span></span>
<span data-ttu-id="96dca-104">Az eclipse-ben, hello legszélesebb körben használt egyik integrált fejlesztési környezetekben (IDEs) Java-fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="96dca-104">Eclipse is one of hello most widely used integrated development environments (IDEs) for Java developers.</span></span> <span data-ttu-id="96dca-105">Ez a cikk azt ismerteti hogyan tooset az Eclipse fejlesztői környezetet toowork Azure Service Fabric fel.</span><span class="sxs-lookup"><span data-stu-id="96dca-105">In this article, we describe how tooset up your Eclipse development environment toowork with Azure Service Fabric.</span></span> <span data-ttu-id="96dca-106">Ismerje meg, hogyan tooinstall hello Service Fabric beépülő modul, a Service Fabric-alkalmazás létrehozása, és a Service Fabric application tooa helyi vagy távoli Service Fabric-fürt üzembe az Eclipse Neonfény.</span><span class="sxs-lookup"><span data-stu-id="96dca-106">Learn how tooinstall hello Service Fabric plug-in, create a Service Fabric application, and deploy your Service Fabric application tooa local or remote Service Fabric cluster in Eclipse Neon.</span></span>

## <a name="install-or-update-hello-service-fabric-plug-in-in-eclipse-neon"></a><span data-ttu-id="96dca-107">Telepítse vagy frissítse a hello Service Fabric Eclipse Neonfény beépülő modulja</span><span class="sxs-lookup"><span data-stu-id="96dca-107">Install or update hello Service Fabric plug-in in Eclipse Neon</span></span>
<span data-ttu-id="96dca-108">Telepíthet egy Service Fabric beépülő modult az Eclipse-en.</span><span class="sxs-lookup"><span data-stu-id="96dca-108">You can install a Service Fabric plug-in in Eclipse.</span></span> <span data-ttu-id="96dca-109">beépülő modul hello megkönnyítheti hello folyamat létrehozása és a Java-szolgáltatások telepítése.</span><span class="sxs-lookup"><span data-stu-id="96dca-109">hello plug-in can help simplify hello process of building and deploying Java services.</span></span>

1.  <span data-ttu-id="96dca-110">Gondoskodjon arról, hogy Eclipse Neonfény hello legújabb verzióját és hello Buildship legújabb verziójának telepítése (1.0.17 vagy újabb verzió):</span><span class="sxs-lookup"><span data-stu-id="96dca-110">Ensure that you have hello latest version of Eclipse Neon and hello latest version of Buildship (1.0.17 or a later version) installed:</span></span>
    -   <span data-ttu-id="96dca-111">toocheck hello verziók telepített összetevőket, az Eclipse Neonfény, nyissa meg túl**súgó** > **telepítésének részletei**.</span><span class="sxs-lookup"><span data-stu-id="96dca-111">toocheck hello versions of installed components, in Eclipse Neon, go too**Help** > **Installation Details**.</span></span>
    -   <span data-ttu-id="96dca-112">tooupdate Buildship, lásd: [Eclipse Buildship: Eclipse beépülő modulokat Gradle][buildship-update].</span><span class="sxs-lookup"><span data-stu-id="96dca-112">tooupdate Buildship, see [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update].</span></span>
    -   <span data-ttu-id="96dca-113">a toocheck és telepítse a frissítéseket az Eclipse Neonfény, nyissa meg túl**súgó** > **frissítések keresése**.</span><span class="sxs-lookup"><span data-stu-id="96dca-113">toocheck for and install updates for Eclipse Neon, go too**Help** > **Check for Updates**.</span></span>

2.  <span data-ttu-id="96dca-114">túl nyissa meg a Service Fabric beépülő modulja, Eclipse Neonfény tooinstall hello**súgó** > **új szoftverek telepítése**.</span><span class="sxs-lookup"><span data-stu-id="96dca-114">tooinstall hello Service Fabric plug-in, in Eclipse Neon, go too**Help** > **Install New Software**.</span></span>
  1.    <span data-ttu-id="96dca-115">A hello **együttműködve** adja meg a **http://dl.microsoft.com/eclipse**.</span><span class="sxs-lookup"><span data-stu-id="96dca-115">In hello **Work with** box, enter **http://dl.microsoft.com/eclipse**.</span></span>
  2.    <span data-ttu-id="96dca-116">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="96dca-116">Click **Add**.</span></span>

         ![Az Eclipse Neon Service Fabric beépülő modulja][sf-eclipse-plugin-install]
  3.    <span data-ttu-id="96dca-118">Válassza ki a hello Service Fabric beépülő modult, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="96dca-118">Select hello Service Fabric plug-in, and then click **Next**.</span></span>
  4.    <span data-ttu-id="96dca-119">Hello telepítési lépéseket, és fogadja el a hello Microsoft szoftverlicenc-szerződést.</span><span class="sxs-lookup"><span data-stu-id="96dca-119">Complete hello installation steps, and then accept hello Microsoft Software License Terms.</span></span>

<span data-ttu-id="96dca-120">Ha már hello Service Fabric beépülő modul telepítve van, győződjön meg arról, hogy rendelkezik-e hello legújabb verziójára.</span><span class="sxs-lookup"><span data-stu-id="96dca-120">If you already have hello Service Fabric plug-in installed, make sure that you have hello latest version.</span></span> <span data-ttu-id="96dca-121">túl nyissa meg az elérhető frissítések toocheck**súgó** > **telepítésének részletei**.</span><span class="sxs-lookup"><span data-stu-id="96dca-121">toocheck for available updates, go too**Help** > **Installation Details**.</span></span> <span data-ttu-id="96dca-122">A telepített beépülő modulok hello listájában válassza ki a Service Fabric, és kattintson **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="96dca-122">In hello list of installed plug-ins, select Service Fabric, and then click **Update**.</span></span> <span data-ttu-id="96dca-123">A rendszer telepíti az elérhető frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="96dca-123">Available updates will be installed.</span></span>

> [!NOTE]
> <span data-ttu-id="96dca-124">Ha telepítésekor vagy frissítésekor a Service Fabric beépülő modul hello lassú, tooan Eclipse beállítás miatt lehet.</span><span class="sxs-lookup"><span data-stu-id="96dca-124">If installing or updating hello Service Fabric plug-in is slow, it might be due tooan Eclipse setting.</span></span> <span data-ttu-id="96dca-125">Eclipse metaadatok gyűjti az Eclipse-példány regisztrált összes módosítások tooupdate helyeken.</span><span class="sxs-lookup"><span data-stu-id="96dca-125">Eclipse collects metadata on all changes tooupdate sites that are registered with your Eclipse instance.</span></span> <span data-ttu-id="96dca-126">hello folyamat keresése és telepítése a Service Fabric beépülő modul frissítés toospeed lépjen túl**elérhető szoftverek helyek**.</span><span class="sxs-lookup"><span data-stu-id="96dca-126">toospeed up hello process of checking for and installing a Service Fabric plug-in update, go too**Available Software Sites**.</span></span> <span data-ttu-id="96dca-127">Törölje az hello jelölőnégyzetet egy mutat, toohello Service Fabric beépülő modul helye (http://dl.microsoft.com/eclipse/azure/servicefabric) hello kivételével minden webhelyére vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="96dca-127">Clear hello check boxes for all sites except for hello one that points toohello Service Fabric plug-in location (http://dl.microsoft.com/eclipse/azure/servicefabric).</span></span>

## <a name="create-a-service-fabric-application-in-eclipse"></a><span data-ttu-id="96dca-128">Service Fabric-alkalmazás létrehozása az Eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="96dca-128">Create a Service Fabric application in Eclipse</span></span>

1.  <span data-ttu-id="96dca-129">Az Eclipse Neonfény, nyissa meg túl**fájl** > **új** > **más**.</span><span class="sxs-lookup"><span data-stu-id="96dca-129">In Eclipse Neon, go too**File** > **New** > **Other**.</span></span> <span data-ttu-id="96dca-130">Válassza a **Service Fabric Project** (Service Fabric-projekt) lehetőséget, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="96dca-130">Select  **Service Fabric Project**, and then click **Next**.</span></span>

    ![Service Fabric – Új projekt, 1. oldal][create-application/p1]

2.  <span data-ttu-id="96dca-132">Adja meg a projekt nevét, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="96dca-132">Enter a name for your project, and then click **Next**.</span></span>

    ![Service Fabric – Új projekt, 2. oldal][create-application/p2]

3.  <span data-ttu-id="96dca-134">Válassza a sablonok hello lista **Szolgáltatássablon**.</span><span class="sxs-lookup"><span data-stu-id="96dca-134">In hello list of templates, select **Service Template**.</span></span> <span data-ttu-id="96dca-135">Válassza ki a szolgáltatássablon típusát (Actor, Stateless, Container, Guest Binary – aktor, állapot nélküli, tároló, vendég bináris), majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="96dca-135">Select your service template type (Actor, Stateless, Container, or Guest Binary), and then click **Next**.</span></span>

    ![Service Fabric – Új projekt, 3. oldal][create-application/p3]

4.  <span data-ttu-id="96dca-137">Adja meg a hello szolgáltatás nevét és részletes ismertetése, és kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="96dca-137">Enter hello service name and service details, and then click **Finish**.</span></span>

    ![Service Fabric – Új projekt, 4. oldal][create-application/p4]

5. <span data-ttu-id="96dca-139">Amikor az első Service Fabric-projektet hoz létre a hello **nyissa meg a társított perspektíva** párbeszédpanel, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="96dca-139">When you create your first Service Fabric project, in hello **Open Associated Perspective** dialog box, click **Yes**.</span></span>

    ![Service Fabric – Új projekt, 5. oldal][create-application/p5]

6.  <span data-ttu-id="96dca-141">Így néz ki az új projekt:</span><span class="sxs-lookup"><span data-stu-id="96dca-141">Your new project looks like this:</span></span>

    ![Service Fabric – Új projekt, 6. oldal][create-application/p6]

## <a name="build-and-deploy-a-service-fabric-application-in-eclipse"></a><span data-ttu-id="96dca-143">Service Fabric-alkalmazás felépítése és üzembe helyezése az Eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="96dca-143">Build and deploy a Service Fabric application in Eclipse</span></span>

1.  <span data-ttu-id="96dca-144">Kattintson a jobb gombbal az új Service Fabric-alkalmazásra, majd válassza a **Service Fabric** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="96dca-144">Right-click your new Service Fabric application, and then select **Service Fabric**.</span></span>

    ![Service Fabric – helyi menü][publish/RightClick]

2. <span data-ttu-id="96dca-146">Hello almenü válassza ki a kívánt hello beállítást:</span><span class="sxs-lookup"><span data-stu-id="96dca-146">In hello submenu, select hello option you want:</span></span>
    -   <span data-ttu-id="96dca-147">toobuild hello alkalmazás nélkül a tisztítás, kattintson a **Build alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="96dca-147">toobuild hello application without cleaning, click **Build Application**.</span></span>
    -   <span data-ttu-id="96dca-148">hello alkalmazás össze toodo kattintson **újraépítése alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="96dca-148">toodo a clean build of hello application, click **Rebuild Application**.</span></span>
    -   <span data-ttu-id="96dca-149">a beépített összetevőihez tooclean hello alkalmazást kattintson **tiszta alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="96dca-149">tooclean hello application of built artifacts, click **Clean Application**.</span></span>

3.  <span data-ttu-id="96dca-150">Ebből a menüből üzembe helyezheti vagy közzéteheti az alkalmazását, illetve visszavonhatja annak üzembe helyezését:</span><span class="sxs-lookup"><span data-stu-id="96dca-150">From this menu, you also can deploy, undeploy, and publish your application:</span></span>
    -   <span data-ttu-id="96dca-151">toodeploy tooyour helyi fürthöz, és kattintson a **alkalmazás központi telepítése**.</span><span class="sxs-lookup"><span data-stu-id="96dca-151">toodeploy tooyour local cluster, click **Deploy Application**.</span></span>
    -   <span data-ttu-id="96dca-152">A hello **alkalmazás közzététele** párbeszédpanelen jelölje ki a közzétételi profilt:</span><span class="sxs-lookup"><span data-stu-id="96dca-152">In hello **Publish Application** dialog box, select a publish profile:</span></span>
        -  <span data-ttu-id="96dca-153">**Local.json**</span><span class="sxs-lookup"><span data-stu-id="96dca-153">**Local.json**</span></span>
        -  <span data-ttu-id="96dca-154">**Cloud.json**</span><span class="sxs-lookup"><span data-stu-id="96dca-154">**Cloud.json**</span></span>

     <span data-ttu-id="96dca-155">A JavaScript Object Notation (JSON) fájlok (például kapcsolati végpontok és biztonsági információk) helyi szükséges tooconnect tooyour vagy Azure-felhőbe információk tárolására.</span><span class="sxs-lookup"><span data-stu-id="96dca-155">These JavaScript Object Notation (JSON) files store information (such as connection endpoints and security information) that is required tooconnect tooyour local or cloud (Azure) cluster.</span></span>

  ![A Service Fabric közzétételi menüje][publish/Publish]

<span data-ttu-id="96dca-157">Egy másik módja toodeploy a Service Fabric-alkalmazás Eclipse használatával fut konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="96dca-157">An alternate way toodeploy your Service Fabric application is by using Eclipse run configurations.</span></span>

  1.    <span data-ttu-id="96dca-158">Nyissa meg túl**futtatása** > **konfigurációk futtatása**.</span><span class="sxs-lookup"><span data-stu-id="96dca-158">Go too**Run** > **Run Configurations**.</span></span>
  2.    <span data-ttu-id="96dca-159">A **Gradle projekt**, jelölje be hello **ServiceFabricDeployer** futtassa konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="96dca-159">Under **Gradle Project**, select hello **ServiceFabricDeployer** run configuration.</span></span>
  3.    <span data-ttu-id="96dca-160">Hello jobb oldali ablaktáblában lévő hello **argumentumok** lapon, a **publishProfile**, jelölje be **helyi** vagy **felhő**.</span><span class="sxs-lookup"><span data-stu-id="96dca-160">In hello right pane, on hello **Arguments** tab, for **publishProfile**, select **local** or **cloud**.</span></span>  <span data-ttu-id="96dca-161">hello alapértelmezett érték a **helyi**.</span><span class="sxs-lookup"><span data-stu-id="96dca-161">hello default is **local**.</span></span> <span data-ttu-id="96dca-162">távoli toodeploy tooa vagy felhő fürthöz, jelölje be **felhő**.</span><span class="sxs-lookup"><span data-stu-id="96dca-162">toodeploy tooa remote or cloud cluster, select **cloud**.</span></span>
  4.    <span data-ttu-id="96dca-163">profilok közzététele, a Szerkesztés, hogy a nem üres-e a megfelelő információkat hello hello tooensure **Local.json** vagy **Cloud.json** igény szerint.</span><span class="sxs-lookup"><span data-stu-id="96dca-163">tooensure that hello proper information is populated in hello publish profiles, edit **Local.json** or **Cloud.json** as needed.</span></span> <span data-ttu-id="96dca-164">Hozzáadhat vagy frissíthet végpontrészleteket és biztonsági hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="96dca-164">You can add or update endpoint details and security credentials.</span></span>
  5.    <span data-ttu-id="96dca-165">Győződjön meg arról, hogy **munkakönyvtár** toodeploy kívánt toohello alkalmazás mutat.</span><span class="sxs-lookup"><span data-stu-id="96dca-165">Ensure that **Working Directory** points toohello application you want toodeploy.</span></span> <span data-ttu-id="96dca-166">toochange hello alkalmazás, kattintson a hello **munkaterület** gombra, és válassza ki a kívánt hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="96dca-166">toochange hello application, click hello **Workspace** button, and then select hello application you want.</span></span>
  6.    <span data-ttu-id="96dca-167">Kattintson az **Apply** (Alkalmaz), majd a **Run** (Futtatás) gombra.</span><span class="sxs-lookup"><span data-stu-id="96dca-167">Click **Apply**, and then click **Run**.</span></span>

<span data-ttu-id="96dca-168">Néhány másodpercen belül megtörténik az alkalmazás felépítése és üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="96dca-168">Your application builds and deploys within a few moments.</span></span> <span data-ttu-id="96dca-169">A Service Fabric Explorerben hello központi telepítési állapot figyelése</span><span class="sxs-lookup"><span data-stu-id="96dca-169">You can monitor hello deployment status in Service Fabric Explorer.</span></span>  

## <a name="add-a-service-fabric-service-tooyour-service-fabric-application"></a><span data-ttu-id="96dca-170">A Service Fabric-szolgáltatás tooyour Service Fabric-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="96dca-170">Add a Service Fabric service tooyour Service Fabric application</span></span>

<span data-ttu-id="96dca-171">a Service Fabric szolgáltatás tooan meglévő Service Fabric-alkalmazás, tooadd hello lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="96dca-171">tooadd a Service Fabric service tooan existing Service Fabric application, do hello following steps:</span></span>

1.  <span data-ttu-id="96dca-172">Kattintson a jobb gombbal hello projekt tooadd szeretné, hogy a szolgáltatás számára, és kattintson a **Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="96dca-172">Right-click hello project you want tooadd a service to, and then click **Service Fabric**.</span></span>

    ![Service Fabric – Szolgáltatás hozzáadása, 1. oldal][add-service/p1]

2.  <span data-ttu-id="96dca-174">Kattintson a **Fabric-szolgáltatás hozzáadása**, és a lépéseket tooadd készletét teljes hello toohello szolgáltatásprojektet.</span><span class="sxs-lookup"><span data-stu-id="96dca-174">Click **Add Service Fabric Service**, and complete hello set of steps tooadd a service toohello project.</span></span>
3.  <span data-ttu-id="96dca-175">Jelölje be hello Szolgáltatássablon tooadd tooyour projektet, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="96dca-175">Select hello service template you want tooadd tooyour project, and then click **Next**.</span></span>

    ![Service Fabric – Szolgáltatás hozzáadása, 2. oldal][add-service/p2]

4.  <span data-ttu-id="96dca-177">Írjon be hello szolgáltatás neve (és más részleteket, igény szerint), és kattintson a hello **hozzáadása szolgáltatás** gombra.</span><span class="sxs-lookup"><span data-stu-id="96dca-177">Enter hello service name (and other details, as needed), and then click hello **Add Service** button.</span></span>  

    ![Service Fabric – Szolgáltatás hozzáadása, 3. oldal][add-service/p3]

5.  <span data-ttu-id="96dca-179">Hello szolgáltatás hozzáadása után a projekt általános struktúra a következő projekt hasonló toohello néz ki:</span><span class="sxs-lookup"><span data-stu-id="96dca-179">After hello service is added, your overall project structure looks similar toohello following project:</span></span>

    ![Service Fabric – Szolgáltatás hozzáadása, 4. oldal][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a><span data-ttu-id="96dca-181">A Service Fabric Java-alkalmazás jegyzékverzióinak szerkesztése</span><span class="sxs-lookup"><span data-stu-id="96dca-181">Edit Manifest versions of your Service Fabric Java application</span></span>

<span data-ttu-id="96dca-182">tooedit jegyzék verziók, kattintson jobb gombbal a hello projektet, nyissa meg túl**Service Fabric** válassza **Manifest verziók szerkesztése...**  hello menü legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="96dca-182">tooedit manifest versions, right click on hello project, go too**Service Fabric** and select **Edit Manifest Versions...** from hello menu dropdown.</span></span> <span data-ttu-id="96dca-183">Hello varázslóban, frissítheti az hello jegyzék a jegyzék, szolgáltatás alkalmazásjegyzék és verziók hello a **kód**, **Config** és **adatok** csomagok.</span><span class="sxs-lookup"><span data-stu-id="96dca-183">In hello wizard, you can update hello manifest versions for application manifest, service manifest and hello versions for **Code**, **Config** and **Data** packages.</span></span>

<span data-ttu-id="96dca-184">Ha hello beállítást **automatikus frissítése az alkalmazás és szolgáltatás verziók** és majd frissítés, majd hello jegyzék verziók automatikusan frissül.</span><span class="sxs-lookup"><span data-stu-id="96dca-184">If you check hello option **Automatically update application and service versions** and then update a version, then hello manifest versions will be automatically updated.</span></span> <span data-ttu-id="96dca-185">például toogive, kiválaszthatja hello jelölőnégyzetet, majd a frissítés hello verziójának **kód** 0.0.0 verziójában too0.0.1, majd kattintson a **Befejezés**, majd szolgáltatás a fürtjegyzék verziója és az alkalmazás jegyzékében verzió too0.0.1 automatikusan frissítve lesz.</span><span class="sxs-lookup"><span data-stu-id="96dca-185">toogive an example, you first select hello check-box, then update hello version of **Code** version from 0.0.0 too0.0.1 and click on **Finish**, then service manifest version and application manifest version will be automatically updated too0.0.1.</span></span>

## <a name="upgrade-your-service-fabric-java-application"></a><span data-ttu-id="96dca-186">A Service Fabric Java-alkalmazásának frissítése</span><span class="sxs-lookup"><span data-stu-id="96dca-186">Upgrade your Service Fabric Java application</span></span>

<span data-ttu-id="96dca-187">A frissítési esetben mondja ki a létrehozott hello **App1** projektben hello Service Fabric az Eclipse beépülő modul segítségével.</span><span class="sxs-lookup"><span data-stu-id="96dca-187">For an upgrade scenario, say you created hello **App1** project by using hello Service Fabric plug-in in Eclipse.</span></span> <span data-ttu-id="96dca-188">Telepítette azt beépülő modul toocreate hello segítségével nevű alkalmazást **fabric: / App1Application**.</span><span class="sxs-lookup"><span data-stu-id="96dca-188">You deployed it by using hello plug-in toocreate an application named **fabric:/App1Application**.</span></span> <span data-ttu-id="96dca-189">hello alkalmazástípus **App1AppicationType**, és a hello alkalmazás verziója 1.0.</span><span class="sxs-lookup"><span data-stu-id="96dca-189">hello application type is **App1AppicationType**, and hello application version is 1.0.</span></span> <span data-ttu-id="96dca-190">Most szeretné tooupgrade az alkalmazás rendelkezésre állásának megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="96dca-190">Now, you want tooupgrade your application without interrupting availability.</span></span>

<span data-ttu-id="96dca-191">Először a beállítások módosításait tooyour alkalmazás, és majd újraépítése a módosított hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="96dca-191">First, make any changes tooyour application, and then rebuild hello modified service.</span></span> <span data-ttu-id="96dca-192">Frissítés hello szolgáltatás jegyzékfájl (ServiceManifest.xml) módosított frissítése hello verzióival hello szolgáltatás (és kód, konfigurációs vagy vonatkozó adatokat).</span><span class="sxs-lookup"><span data-stu-id="96dca-192">Update hello modified service’s manifest file (ServiceManifest.xml) with hello updated versions for hello service (and Code, Config, or Data, as relevant).</span></span> <span data-ttu-id="96dca-193">Is módosítsa a hello alkalmazás jegyzékfájlja (ApplicationManifest.xml) frissítése hello verziószámú hello alkalmazáshoz, és hello módosított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="96dca-193">Also, modify hello application’s manifest (ApplicationManifest.xml) with hello updated version number for hello application and hello modified service.</span></span>  

<span data-ttu-id="96dca-194">tooupgrade az alkalmazás Eclipse Neonfény használatával, hozhat létre egy duplikált futtatási konfigurációs profilt.</span><span class="sxs-lookup"><span data-stu-id="96dca-194">tooupgrade your application by using Eclipse Neon, you can create a duplicate run configuration profile.</span></span> <span data-ttu-id="96dca-195">Ezt követően tooupgrade használni az alkalmazást igény szerint.</span><span class="sxs-lookup"><span data-stu-id="96dca-195">Then, use it tooupgrade your application as needed.</span></span>

1.  <span data-ttu-id="96dca-196">Nyissa meg túl**futtatása** > **konfigurációk futtatása**.</span><span class="sxs-lookup"><span data-stu-id="96dca-196">Go too**Run** > **Run Configurations**.</span></span> <span data-ttu-id="96dca-197">Hello bal oldali ablaktáblában kattintson a hello kis nyílra toohello bal oldalán **Gradle projekt**.</span><span class="sxs-lookup"><span data-stu-id="96dca-197">In hello left pane, click hello small arrow toohello left of **Gradle Project**.</span></span>
2.  <span data-ttu-id="96dca-198">Kattintson a jobb gombbal a **ServiceFabricDeployer** elemre, majd válassza a **Duplicate** (Megkettőzés) parancsot.</span><span class="sxs-lookup"><span data-stu-id="96dca-198">Right-click **ServiceFabricDeployer**, and then select **Duplicate**.</span></span> <span data-ttu-id="96dca-199">Adjon egy új nevet a konfigurációnak, például **ServiceFabricUpgrader**.</span><span class="sxs-lookup"><span data-stu-id="96dca-199">Enter a new name for this configuration, for example, **ServiceFabricUpgrader**.</span></span>
3.  <span data-ttu-id="96dca-200">Hello jobb oldali panelen, a hello **argumentumok** lapján módosítsa **- Pconfig = "telepítése"** túl**- Pconfig = a "frissítés"**, és kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="96dca-200">In hello right panel, on hello **Arguments** tab, change **-Pconfig='deploy'** too**-Pconfig='upgrade'**, and then click **Apply**.</span></span>

<span data-ttu-id="96dca-201">Ez a folyamat hoz létre, és futtassa a konfigurációs profil mentése használhat bármilyen idő tooupgrade az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="96dca-201">This process creates and saves a run configuration profile you can use at any time tooupgrade your application.</span></span> <span data-ttu-id="96dca-202">Azt is lekérése hello legújabb frissített alkalmazástípus verziója hello Alkalmazásjegyzék-fájl.</span><span class="sxs-lookup"><span data-stu-id="96dca-202">It also gets hello latest updated application type version from hello application manifest file.</span></span>

<span data-ttu-id="96dca-203">hello alkalmazás frissítése néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="96dca-203">hello application upgrade takes a few minutes.</span></span> <span data-ttu-id="96dca-204">A Service Fabric Explorerben hello az alkalmazásfrissítés figyelheti.</span><span class="sxs-lookup"><span data-stu-id="96dca-204">You can monitor hello application upgrade in Service Fabric Explorer.</span></span>

## <a name="migrating-old-service-fabric-java-applications-toobe-used-with-maven"></a><span data-ttu-id="96dca-205">Service Fabric Java-alkalmazások régi toobe Maven használt áttelepítése</span><span class="sxs-lookup"><span data-stu-id="96dca-205">Migrating old Service Fabric Java applications toobe used with Maven</span></span>
<span data-ttu-id="96dca-206">A Service Fabric Java SDK tooMaven tárházat nemrég került át Service Fabric Java szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="96dca-206">We have recently moved Service Fabric Java libraries from Service Fabric Java SDK tooMaven repository.</span></span> <span data-ttu-id="96dca-207">Amíg hello használata az eclipse-ben létrehozhat új alkalmazások legújabb frissített projektek (Ez az a Maven képes toowork) hoz létre, frissítheti a meglévő Service Fabric állapot nélküli vagy szereplő Java-alkalmazások, amelyek hello Service Fabric Java SDK használata korábbi, toouse hello Service Fabric Java függőségek a Mavenben.</span><span class="sxs-lookup"><span data-stu-id="96dca-207">While hello new applications you generate using Eclipse, will generate latest updated projects (which will be able toowork with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using hello Service Fabric Java SDK earlier, toouse hello Service Fabric Java dependencies from Maven.</span></span> <span data-ttu-id="96dca-208">Kövesse az említett hello lépéseket [Itt](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure Maven együttműködve biztosítja a régebbi alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="96dca-208">Please follow hello steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure your older application works with Maven.</span></span>

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-eclipse/service-fabric-eclipse-plugin.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
