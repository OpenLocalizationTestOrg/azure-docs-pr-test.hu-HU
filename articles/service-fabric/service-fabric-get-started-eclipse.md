---
title: "Azure Service Fabric beépülő modul az Eclipse-hez | Microsoft Docs"
description: "Bevezetés az Eclipse Service Fabric beépülő moduljának használatába."
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
ms.openlocfilehash: 98c1b99972b9ad7a396d72b98e727286f6822e42
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a><span data-ttu-id="2f12e-103">Az Eclipse Service Fabric beépülő moduljának Java alkalmazásfejlesztése</span><span class="sxs-lookup"><span data-stu-id="2f12e-103">Service Fabric plug-in for Eclipse Java application development</span></span>
<span data-ttu-id="2f12e-104">Az Eclipse a Java-fejlesztők által leggyakrabban használt integrált fejlesztőkörnyezetek (IDE-k) közé tartozik.</span><span class="sxs-lookup"><span data-stu-id="2f12e-104">Eclipse is one of the most widely used integrated development environments (IDEs) for Java developers.</span></span> <span data-ttu-id="2f12e-105">Ebben a cikkben azt ismertetjük, hogyan állíthatja be az Eclipse fejlesztői környezetet az Azure Service Fabrickel való használathoz.</span><span class="sxs-lookup"><span data-stu-id="2f12e-105">In this article, we describe how to set up your Eclipse development environment to work with Azure Service Fabric.</span></span> <span data-ttu-id="2f12e-106">Megtudhatja, hogyan telepítheti a Service Fabric beépülő modult, hogyan hozhat létre Service Fabric-alkalmazást, és hogyan helyezhet üzembe Service Fabric-alkalmazásokat helyi vagy távoli Service Fabric-fürtön az Eclipse Neonon.</span><span class="sxs-lookup"><span data-stu-id="2f12e-106">Learn how to install the Service Fabric plug-in, create a Service Fabric application, and deploy your Service Fabric application to a local or remote Service Fabric cluster in Eclipse Neon.</span></span>

## <a name="install-or-update-the-service-fabric-plug-in-in-eclipse-neon"></a><span data-ttu-id="2f12e-107">A Service Fabric beépülő modul telepítése vagy frissítése az Eclipse Neonon</span><span class="sxs-lookup"><span data-stu-id="2f12e-107">Install or update the Service Fabric plug-in in Eclipse Neon</span></span>
<span data-ttu-id="2f12e-108">Telepíthet egy Service Fabric beépülő modult az Eclipse-en.</span><span class="sxs-lookup"><span data-stu-id="2f12e-108">You can install a Service Fabric plug-in in Eclipse.</span></span> <span data-ttu-id="2f12e-109">A beépülő modul segíthet leegyszerűsíteni a Java-szolgáltatások létrehozásának és üzembe helyezésének folyamatát.</span><span class="sxs-lookup"><span data-stu-id="2f12e-109">The plug-in can help simplify the process of building and deploying Java services.</span></span>

1.  <span data-ttu-id="2f12e-110">Gondoskodjon róla, hogy az Eclipse Neon és a Buildship legújabb verziója (1.0.17-es vagy újabb) legyen telepítve:</span><span class="sxs-lookup"><span data-stu-id="2f12e-110">Ensure that you have the latest version of Eclipse Neon and the latest version of Buildship (1.0.17 or a later version) installed:</span></span>
    -   <span data-ttu-id="2f12e-111">A telepített összetevők verziójának ellenőrzéséhez az Eclipse Neonban lépjen a **Help** > **Installation Details** (Súgó, Telepítés részletei) területre.</span><span class="sxs-lookup"><span data-stu-id="2f12e-111">To check the versions of installed components, in Eclipse Neon, go to **Help** > **Installation Details**.</span></span>
    -   <span data-ttu-id="2f12e-112">A Buildship frissítéséért lásd: [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update] (Eclipse Buildship: Eclipse beépülő modulok a Gradle-hez).</span><span class="sxs-lookup"><span data-stu-id="2f12e-112">To update Buildship, see [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update].</span></span>
    -   <span data-ttu-id="2f12e-113">Az Eclipse Neon frissítéseinek kereséséhez és telepítéséhez lépjen a **Help** > **Check for Updates** (Súgó, Frissítések keresése) területre.</span><span class="sxs-lookup"><span data-stu-id="2f12e-113">To check for and install updates for Eclipse Neon, go to **Help** > **Check for Updates**.</span></span>

2.  <span data-ttu-id="2f12e-114">A Service Fabric beépülő modul telepítéséhez az Eclipse Neonban lépjen a **Help** > **Install New Software** (Súgó, Új szoftver telepítése) területre.</span><span class="sxs-lookup"><span data-stu-id="2f12e-114">To install the Service Fabric plug-in, in Eclipse Neon, go to **Help** > **Install New Software**.</span></span>
  1.    <span data-ttu-id="2f12e-115">A **Work with** mezőbe írja be a **http://dl.microsoft.com/eclipse** címet.</span><span class="sxs-lookup"><span data-stu-id="2f12e-115">In the **Work with** box, enter **http://dl.microsoft.com/eclipse**.</span></span>
  2.    <span data-ttu-id="2f12e-116">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-116">Click **Add**.</span></span>

         ![Az Eclipse Neon Service Fabric beépülő modulja][sf-eclipse-plugin-install]
  3.    <span data-ttu-id="2f12e-118">Válassza ki a Service Fabric beépülő modult, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-118">Select the Service Fabric plug-in, and then click **Next**.</span></span>
  4.    <span data-ttu-id="2f12e-119">Végezze el a telepítés lépéseit, majd fogadja el a Microsoft szoftverlicenc-szerződését.</span><span class="sxs-lookup"><span data-stu-id="2f12e-119">Complete the installation steps, and then accept the Microsoft Software License Terms.</span></span>

<span data-ttu-id="2f12e-120">Ha a Service Fabric beépülő modul már telepítve van, győződjön meg arról, hogy a legújabb verzióval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2f12e-120">If you already have the Service Fabric plug-in installed, make sure that you have the latest version.</span></span> <span data-ttu-id="2f12e-121">Az elérhető frissítések kereséséhez lépjen a **Help** > **Installation Details** (Súgó, Telepítés részletei) területre.</span><span class="sxs-lookup"><span data-stu-id="2f12e-121">To check for available updates, go to **Help** > **Installation Details**.</span></span> <span data-ttu-id="2f12e-122">A telepített beépülő modulok listájában válassza ki a Service Fabric elemet, majd kattintson az **Update** (Frissítés) parancsra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-122">In the list of installed plug-ins, select Service Fabric, and then click **Update**.</span></span> <span data-ttu-id="2f12e-123">A rendszer telepíti az elérhető frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="2f12e-123">Available updates will be installed.</span></span>

> [!NOTE]
> <span data-ttu-id="2f12e-124">Ha a Service Fabric beépülő modul telepítése vagy frissítése túl lassú, azt az Eclipse valamelyik beállítása okozhatja.</span><span class="sxs-lookup"><span data-stu-id="2f12e-124">If installing or updating the Service Fabric plug-in is slow, it might be due to an Eclipse setting.</span></span> <span data-ttu-id="2f12e-125">Az Eclipse metaadatokat gyűjt az Eclipse-példányhoz regisztrált frissítési helyek összes módosításáról.</span><span class="sxs-lookup"><span data-stu-id="2f12e-125">Eclipse collects metadata on all changes to update sites that are registered with your Eclipse instance.</span></span> <span data-ttu-id="2f12e-126">Ahhoz, hogy fel tudja gyorsítani a Service Fabric beépülő modul frissítéseinek keresési és telepítési folyamatát, lépjen az **Available Software Sites** (Elérhető szoftverhelyek) területre.</span><span class="sxs-lookup"><span data-stu-id="2f12e-126">To speed up the process of checking for and installing a Service Fabric plug-in update, go to **Available Software Sites**.</span></span> <span data-ttu-id="2f12e-127">Törölje az összes hely jelölőnégyzetét a Service Fabric beépülő modul helyére (http://dl.microsoft.com/eclipse/azure/servicefabric) mutató jelölőnégyzet kivételével.</span><span class="sxs-lookup"><span data-stu-id="2f12e-127">Clear the check boxes for all sites except for the one that points to the Service Fabric plug-in location (http://dl.microsoft.com/eclipse/azure/servicefabric).</span></span>

## <a name="create-a-service-fabric-application-in-eclipse"></a><span data-ttu-id="2f12e-128">Service Fabric-alkalmazás létrehozása az Eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="2f12e-128">Create a Service Fabric application in Eclipse</span></span>

1.  <span data-ttu-id="2f12e-129">Az Eclipse Neonban lépjen a **File** > **New** > **Other** (Fájl, Új, Egyéb) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="2f12e-129">In Eclipse Neon, go to **File** > **New** > **Other**.</span></span> <span data-ttu-id="2f12e-130">Válassza a **Service Fabric Project** (Service Fabric-projekt) lehetőséget, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-130">Select  **Service Fabric Project**, and then click **Next**.</span></span>

    ![Service Fabric – Új projekt, 1. oldal][create-application/p1]

2.  <span data-ttu-id="2f12e-132">Adja meg a projekt nevét, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-132">Enter a name for your project, and then click **Next**.</span></span>

    ![Service Fabric – Új projekt, 2. oldal][create-application/p2]

3.  <span data-ttu-id="2f12e-134">A sablonok listájában válassza a **Service Template** (Szolgáltatássablon) elemet.</span><span class="sxs-lookup"><span data-stu-id="2f12e-134">In the list of templates, select **Service Template**.</span></span> <span data-ttu-id="2f12e-135">Válassza ki a szolgáltatássablon típusát (Actor, Stateless, Container, Guest Binary – aktor, állapot nélküli, tároló, vendég bináris), majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-135">Select your service template type (Actor, Stateless, Container, or Guest Binary), and then click **Next**.</span></span>

    ![Service Fabric – Új projekt, 3. oldal][create-application/p3]

4.  <span data-ttu-id="2f12e-137">Adja meg a szolgáltatás nevét és a szolgáltatás részleteit, majd kattintson a **Finish** (Befejezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-137">Enter the service name and service details, and then click **Finish**.</span></span>

    ![Service Fabric – Új projekt, 4. oldal][create-application/p4]

5. <span data-ttu-id="2f12e-139">Az első Service Fabric-projekt létrehozásakor az **Open Associated Perspective** (Társított perspektíva megnyitása) párbeszédpanelen kattintson a **Yes** (Igen) gombra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-139">When you create your first Service Fabric project, in the **Open Associated Perspective** dialog box, click **Yes**.</span></span>

    ![Service Fabric – Új projekt, 5. oldal][create-application/p5]

6.  <span data-ttu-id="2f12e-141">Így néz ki az új projekt:</span><span class="sxs-lookup"><span data-stu-id="2f12e-141">Your new project looks like this:</span></span>

    ![Service Fabric – Új projekt, 6. oldal][create-application/p6]

## <a name="build-and-deploy-a-service-fabric-application-in-eclipse"></a><span data-ttu-id="2f12e-143">Service Fabric-alkalmazás felépítése és üzembe helyezése az Eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="2f12e-143">Build and deploy a Service Fabric application in Eclipse</span></span>

1.  <span data-ttu-id="2f12e-144">Kattintson a jobb gombbal az új Service Fabric-alkalmazásra, majd válassza a **Service Fabric** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2f12e-144">Right-click your new Service Fabric application, and then select **Service Fabric**.</span></span>

    ![Service Fabric – helyi menü][publish/RightClick]

2. <span data-ttu-id="2f12e-146">Az almenüben válassza ki a kívánt beállítást:</span><span class="sxs-lookup"><span data-stu-id="2f12e-146">In the submenu, select the option you want:</span></span>
    -   <span data-ttu-id="2f12e-147">Az alkalmazás tisztítás nélküli kiépítéséhez kattintson a **Build Application** (Alkalmazás buildelése) parancsra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-147">To build the application without cleaning, click **Build Application**.</span></span>
    -   <span data-ttu-id="2f12e-148">Az alkalmazás tiszta buildjének kiépítéséhez kattintson a **Rebuild Application** (Alkalmazás újrabuildelése) parancsra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-148">To do a clean build of the application, click **Rebuild Application**.</span></span>
    -   <span data-ttu-id="2f12e-149">A kiépített összetevők törléséhez az alkalmazásból kattintson a **Clean Application** (Alkalmazás tisztítása) parancsra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-149">To clean the application of built artifacts, click **Clean Application**.</span></span>

3.  <span data-ttu-id="2f12e-150">Ebből a menüből üzembe helyezheti vagy közzéteheti az alkalmazását, illetve visszavonhatja annak üzembe helyezését:</span><span class="sxs-lookup"><span data-stu-id="2f12e-150">From this menu, you also can deploy, undeploy, and publish your application:</span></span>
    -   <span data-ttu-id="2f12e-151">A helyi fürtre való üzembe helyezéshez kattintson a **Deploy Application** (Alkalmazás üzembe helyezése) parancsra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-151">To deploy to your local cluster, click **Deploy Application**.</span></span>
    -   <span data-ttu-id="2f12e-152">A **Publish Application** (Alkalmazás közzététele) párbeszédpanelen válasszon egy közzétételi profilt:</span><span class="sxs-lookup"><span data-stu-id="2f12e-152">In the **Publish Application** dialog box, select a publish profile:</span></span>
        -  <span data-ttu-id="2f12e-153">**Local.json**</span><span class="sxs-lookup"><span data-stu-id="2f12e-153">**Local.json**</span></span>
        -  <span data-ttu-id="2f12e-154">**Cloud.json**</span><span class="sxs-lookup"><span data-stu-id="2f12e-154">**Cloud.json**</span></span>

     <span data-ttu-id="2f12e-155">Ezek a JavaScript Object Notation (JSON-) fájlok a helyi vagy a felhőbeli (Azure-) fürthöz való csatlakozáshoz szükséges információk (például csatlakozási végpontok és biztonsági információk) tárolására szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="2f12e-155">These JavaScript Object Notation (JSON) files store information (such as connection endpoints and security information) that is required to connect to your local or cloud (Azure) cluster.</span></span>

  ![A Service Fabric közzétételi menüje][publish/Publish]

<span data-ttu-id="2f12e-157">A Service Fabric-alkalmazások üzembe helyezésének másik módszere, ha Eclipse futtatási konfigurációkat használ.</span><span class="sxs-lookup"><span data-stu-id="2f12e-157">An alternate way to deploy your Service Fabric application is by using Eclipse run configurations.</span></span>

  1.    <span data-ttu-id="2f12e-158">Lépjen a **Run** > **Run Configurations** (Futtatás, Konfigurációk futtatása) területre.</span><span class="sxs-lookup"><span data-stu-id="2f12e-158">Go to **Run** > **Run Configurations**.</span></span>
  2.    <span data-ttu-id="2f12e-159">A **Gradle Project** (Gradle-projekt) területen válassza a **ServiceFabricDeployer** futtatási konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="2f12e-159">Under **Gradle Project**, select the **ServiceFabricDeployer** run configuration.</span></span>
  3.    <span data-ttu-id="2f12e-160">A jobb oldali ablaktáblán, az **Arguments** (Argumentumok) lapon a **publishProfile** értékeként válassza a **local** (helyi) vagy a **cloud** (felhő) értéket.</span><span class="sxs-lookup"><span data-stu-id="2f12e-160">In the right pane, on the **Arguments** tab, for **publishProfile**, select **local** or **cloud**.</span></span>  <span data-ttu-id="2f12e-161">Az alapértelmezett érték a **local** (helyi).</span><span class="sxs-lookup"><span data-stu-id="2f12e-161">The default is **local**.</span></span> <span data-ttu-id="2f12e-162">Távoli vagy felhőalapú fürtbe végzett üzembe helyezéshez válassza a **cloud** (felhő) értéket.</span><span class="sxs-lookup"><span data-stu-id="2f12e-162">To deploy to a remote or cloud cluster, select **cloud**.</span></span>
  4.    <span data-ttu-id="2f12e-163">Ahhoz, hogy a megfelelő információk legyenek betöltve a közzétételi profilokba, szükség szerint szerkessze a **Local.json** vagy a **Cloud.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="2f12e-163">To ensure that the proper information is populated in the publish profiles, edit **Local.json** or **Cloud.json** as needed.</span></span> <span data-ttu-id="2f12e-164">Hozzáadhat vagy frissíthet végpontrészleteket és biztonsági hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="2f12e-164">You can add or update endpoint details and security credentials.</span></span>
  5.    <span data-ttu-id="2f12e-165">Győződjön meg arról, hogy a **Working Directory** (Munkakönyvtár) az üzembe helyezni kívánt alkalmazásra mutat.</span><span class="sxs-lookup"><span data-stu-id="2f12e-165">Ensure that **Working Directory** points to the application you want to deploy.</span></span> <span data-ttu-id="2f12e-166">Az alkalmazás módosításához kattintson a **Workspace** (Munkaterület) gombra, és válassza ki a kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2f12e-166">To change the application, click the **Workspace** button, and then select the application you want.</span></span>
  6.    <span data-ttu-id="2f12e-167">Kattintson az **Apply** (Alkalmaz), majd a **Run** (Futtatás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-167">Click **Apply**, and then click **Run**.</span></span>

<span data-ttu-id="2f12e-168">Néhány másodpercen belül megtörténik az alkalmazás felépítése és üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="2f12e-168">Your application builds and deploys within a few moments.</span></span> <span data-ttu-id="2f12e-169">Az üzembe helyezés állapotát a Service Fabric Explorerben követheti nyomon.</span><span class="sxs-lookup"><span data-stu-id="2f12e-169">You can monitor the deployment status in Service Fabric Explorer.</span></span>  

## <a name="add-a-service-fabric-service-to-your-service-fabric-application"></a><span data-ttu-id="2f12e-170">Service Fabric-szolgáltatás hozzáadása a Service Fabric-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="2f12e-170">Add a Service Fabric service to your Service Fabric application</span></span>

<span data-ttu-id="2f12e-171">Ha Service Fabric-szolgáltatást szeretne hozzáadni egy meglévő Service Fabric-alkalmazáshoz, végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2f12e-171">To add a Service Fabric service to an existing Service Fabric application, do the following steps:</span></span>

1.  <span data-ttu-id="2f12e-172">Kattintson a jobb gombbal a projektre, amelyhez hozzá szeretne adni egy szolgáltatást, majd kattintson a **Service Fabric** elemre.</span><span class="sxs-lookup"><span data-stu-id="2f12e-172">Right-click the project you want to add a service to, and then click **Service Fabric**.</span></span>

    ![Service Fabric – Szolgáltatás hozzáadása, 1. oldal][add-service/p1]

2.  <span data-ttu-id="2f12e-174">Kattintson az **Add Service Fabric Service** (Service Fabric-szolgáltatás hozzáadása) parancsra, és végezze el a szolgáltatás hozzáadásának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="2f12e-174">Click **Add Service Fabric Service**, and complete the set of steps to add a service to the project.</span></span>
3.  <span data-ttu-id="2f12e-175">Válassza ki a projekthez hozzáadni kívánt szolgáltatássablont, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-175">Select the service template you want to add to your project, and then click **Next**.</span></span>

    ![Service Fabric – Szolgáltatás hozzáadása, 2. oldal][add-service/p2]

4.  <span data-ttu-id="2f12e-177">Adja meg a szolgáltatás nevét (és a többi szükséges adatot), majd kattintson az **Add Service** (Szolgáltatás hozzáadása) gombra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-177">Enter the service name (and other details, as needed), and then click the **Add Service** button.</span></span>  

    ![Service Fabric – Szolgáltatás hozzáadása, 3. oldal][add-service/p3]

5.  <span data-ttu-id="2f12e-179">A szolgáltatás hozzáadása után a teljes projektstruktúra a következőhöz hasonlóan néz ki:</span><span class="sxs-lookup"><span data-stu-id="2f12e-179">After the service is added, your overall project structure looks similar to the following project:</span></span>

    ![Service Fabric – Szolgáltatás hozzáadása, 4. oldal][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a><span data-ttu-id="2f12e-181">A Service Fabric Java-alkalmazás jegyzékverzióinak szerkesztése</span><span class="sxs-lookup"><span data-stu-id="2f12e-181">Edit Manifest versions of your Service Fabric Java application</span></span>

<span data-ttu-id="2f12e-182">Jegyzékverziók szerkesztéséhez kattintson jobb gombbal a projektre, majd a legördülő menüből válassza a **Service Fabric** alatt az **Edit Manifest Versions...** (Jegyzékverziók szerkesztése) pontot.</span><span class="sxs-lookup"><span data-stu-id="2f12e-182">To edit manifest versions, right click on the project, go to **Service Fabric** and select **Edit Manifest Versions...** from the menu dropdown.</span></span> <span data-ttu-id="2f12e-183">A varázslóval frissítheti az alkalmazásjegyzék, szolgáltatásjegyzék, valamint a **Code** (Kód-), **Config** (Konfigurációs-) és **Data** (Adat-) csomagok verzióit.</span><span class="sxs-lookup"><span data-stu-id="2f12e-183">In the wizard, you can update the manifest versions for application manifest, service manifest and the versions for **Code**, **Config** and **Data** packages.</span></span>

<span data-ttu-id="2f12e-184">Ha bejelöli az **Automatically update application and service versions** (Alkalmazás- és szolgáltatás-verziók automatikus frissítése) lehetőséget, akkor a verziófrissítéskor a jegyzékverziók is automatikusan frissülnek.</span><span class="sxs-lookup"><span data-stu-id="2f12e-184">If you check the option **Automatically update application and service versions** and then update a version, then the manifest versions will be automatically updated.</span></span> <span data-ttu-id="2f12e-185">Ha például előre bejelöli a jelölőnégyzetet, majd 0.0.0-ról 0.0.1-re frissíti a **Code** verzióját és a **Finish** gombra kattint, akkor a szolgáltatásjegyzék és alkalmazásjegyzék verziója is automatikusan 0.0.1-re frissül.</span><span class="sxs-lookup"><span data-stu-id="2f12e-185">To give an example, you first select the check-box, then update the version of **Code** version from 0.0.0 to 0.0.1 and click on **Finish**, then service manifest version and application manifest version will be automatically updated to 0.0.1.</span></span>

## <a name="upgrade-your-service-fabric-java-application"></a><span data-ttu-id="2f12e-186">A Service Fabric Java-alkalmazásának frissítése</span><span class="sxs-lookup"><span data-stu-id="2f12e-186">Upgrade your Service Fabric Java application</span></span>

<span data-ttu-id="2f12e-187">Frissítési forgatókönyv esetén tegyük fel, hogy az **App1** projektet hozta létre a Service Fabric beépülő modullal az Eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="2f12e-187">For an upgrade scenario, say you created the **App1** project by using the Service Fabric plug-in in Eclipse.</span></span> <span data-ttu-id="2f12e-188">A projektet a beépülő modullal helyezte üzembe a **fabric:/App1Application** nevű alkalmazás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="2f12e-188">You deployed it by using the plug-in to create an application named **fabric:/App1Application**.</span></span> <span data-ttu-id="2f12e-189">Az alkalmazás típusa **App1AppicationType**, a verziója pedig 1.0.</span><span class="sxs-lookup"><span data-stu-id="2f12e-189">The application type is **App1AppicationType**, and the application version is 1.0.</span></span> <span data-ttu-id="2f12e-190">A rendelkezésre állás megszakítása nélkül szeretné frissíteni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2f12e-190">Now, you want to upgrade your application without interrupting availability.</span></span>

<span data-ttu-id="2f12e-191">Először végezzen módosítást az alkalmazáson, majd építse újra a módosított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2f12e-191">First, make any changes to your application, and then rebuild the modified service.</span></span> <span data-ttu-id="2f12e-192">Frissítse a módosított szolgáltatás jegyzékfájlját (ServiceManifest.xml) a szolgáltatás frissített verzióival (és a megfelelő Code, Config vagy Data értékkel).</span><span class="sxs-lookup"><span data-stu-id="2f12e-192">Update the modified service’s manifest file (ServiceManifest.xml) with the updated versions for the service (and Code, Config, or Data, as relevant).</span></span> <span data-ttu-id="2f12e-193">Módosítsa az alkalmazás jegyzékfájlját is (ApplicationManifest.xml) az alkalmazás frissített verziószámával és a módosított szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="2f12e-193">Also, modify the application’s manifest (ApplicationManifest.xml) with the updated version number for the application and the modified service.</span></span>  

<span data-ttu-id="2f12e-194">Ha az Eclipse Neonnal szeretné frissíteni az alkalmazást, létrehozhat egy duplikált futtatási konfigurációs profilt,</span><span class="sxs-lookup"><span data-stu-id="2f12e-194">To upgrade your application by using Eclipse Neon, you can create a duplicate run configuration profile.</span></span> <span data-ttu-id="2f12e-195">amelyet aztán szükség szerint az alkalmazás frissítésére használhat.</span><span class="sxs-lookup"><span data-stu-id="2f12e-195">Then, use it to upgrade your application as needed.</span></span>

1.  <span data-ttu-id="2f12e-196">Lépjen a **Run** > **Run Configurations** (Futtatás, Konfigurációk futtatása) területre.</span><span class="sxs-lookup"><span data-stu-id="2f12e-196">Go to **Run** > **Run Configurations**.</span></span> <span data-ttu-id="2f12e-197">A bal oldali ablaktáblában kattintson a **Gradle Project** (Gradle-projekt) bal oldalán található kis nyílra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-197">In the left pane, click the small arrow to the left of **Gradle Project**.</span></span>
2.  <span data-ttu-id="2f12e-198">Kattintson a jobb gombbal a **ServiceFabricDeployer** elemre, majd válassza a **Duplicate** (Megkettőzés) parancsot.</span><span class="sxs-lookup"><span data-stu-id="2f12e-198">Right-click **ServiceFabricDeployer**, and then select **Duplicate**.</span></span> <span data-ttu-id="2f12e-199">Adjon egy új nevet a konfigurációnak, például **ServiceFabricUpgrader**.</span><span class="sxs-lookup"><span data-stu-id="2f12e-199">Enter a new name for this configuration, for example, **ServiceFabricUpgrader**.</span></span>
3.  <span data-ttu-id="2f12e-200">A jobb oldali ablaktáblán, az **Arguments** (Argumentumok) lapon módosítsa a **-Pconfig='deploy'** értéket **-Pconfig='upgrade'** értékre, majd kattintson az **Apply** (Alkalmaz) gombra.</span><span class="sxs-lookup"><span data-stu-id="2f12e-200">In the right panel, on the **Arguments** tab, change **-Pconfig='deploy'** to **-Pconfig='upgrade'**, and then click **Apply**.</span></span>

<span data-ttu-id="2f12e-201">Ez a folyamat olyan futtatási konfigurációs profilt hoz létre és ment el, amellyel bármikor frissítheti az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2f12e-201">This process creates and saves a run configuration profile you can use at any time to upgrade your application.</span></span> <span data-ttu-id="2f12e-202">Az alkalmazás jegyzékfájljából a legújabb frissített alkalmazástípus-verziót is lekéri.</span><span class="sxs-lookup"><span data-stu-id="2f12e-202">It also gets the latest updated application type version from the application manifest file.</span></span>

<span data-ttu-id="2f12e-203">Az alkalmazás frissítése eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="2f12e-203">The application upgrade takes a few minutes.</span></span> <span data-ttu-id="2f12e-204">Az alkalmazás frissítésének folyamatát a Service Fabric Explorerben követheti nyomon.</span><span class="sxs-lookup"><span data-stu-id="2f12e-204">You can monitor the application upgrade in Service Fabric Explorer.</span></span>

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a><span data-ttu-id="2f12e-205">A Mavennel használni kívánt régi Service Fabric Java-alkalmazások migrálása</span><span class="sxs-lookup"><span data-stu-id="2f12e-205">Migrating old Service Fabric Java applications to be used with Maven</span></span>
<span data-ttu-id="2f12e-206">Nemrégiben áthelyeztük a Service Fabric Java-kódtárakat a Service Fabric Java SDK-ból a Mavenen futó adattárba.</span><span class="sxs-lookup"><span data-stu-id="2f12e-206">We have recently moved Service Fabric Java libraries from Service Fabric Java SDK to Maven repository.</span></span> <span data-ttu-id="2f12e-207">Az Eclipse-szel létrehozott új alkalmazások a legfrissebb projekteket hozzák létre (amelyek képesek együttműködni a Mavennel), a meglévő állapotmentes vagy aktor Service Fabric Java-alkalmazások pedig, amelyek korábban a Service Fabric Java SDK-t használták, frissíthetők a Mavenben található Service Fabric Java-függőségek használatára.</span><span class="sxs-lookup"><span data-stu-id="2f12e-207">While the new applications you generate using Eclipse, will generate latest updated projects (which will be able to work with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using the Service Fabric Java SDK earlier, to use the Service Fabric Java dependencies from Maven.</span></span> <span data-ttu-id="2f12e-208">Kövesse az [itt](service-fabric-migrate-old-javaapp-to-use-maven.md) felsorolt lépéseket, ha biztosítani kívánja, hogy a régebbi alkalmazásaik együttműködjenek a Mavennel.</span><span class="sxs-lookup"><span data-stu-id="2f12e-208">Please follow the steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) to ensure your older application works with Maven.</span></span>

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
