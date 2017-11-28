---
title: "Meglévő alkalmazás gyors üzembe helyezése egy Azure Service Fabric-fürtön"
description: "Meglévő Node.js-alkalmazás üzemeltetése egy Azure Service Fabric-fürtön a Visual Studio használatával."
services: service-fabric
documentationcenter: nodejs
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: adegeo
ms.openlocfilehash: 3601b73872bbea4b4e5324382eb97b7384ca6e13
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="host-a-nodejs-application-on-azure-service-fabric"></a><span data-ttu-id="fa95f-103">Node.js-alkalmazás üzemeltetése az Azure Service Fabricban</span><span class="sxs-lookup"><span data-stu-id="fa95f-103">Host a Node.js application on Azure Service Fabric</span></span>

<span data-ttu-id="fa95f-104">Ez a rövid útmutató segítséget nyújt egy meglévő alkalmazás (ebben a példában a Node.js) Azure-on futó Service Fabric-fürtön történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="fa95f-104">This quickstart helps you deploy an existing application (Node.js in this example) to a Service Fabric cluster running on Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa95f-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fa95f-105">Prerequisites</span></span>

<span data-ttu-id="fa95f-106">Mielőtt elkezdené, győződjön meg arról, hogy [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fa95f-106">Before you get started, make sure that you have [set up your development environment](service-fabric-get-started.md).</span></span> <span data-ttu-id="fa95f-107">Ebbe beletartozik a Service Fabric SDK és a Visual Studio 2017 vagy 2015 telepítése is.</span><span class="sxs-lookup"><span data-stu-id="fa95f-107">Which includes installing the Service Fabric SDK and Visual Studio 2017 or 2015.</span></span>

<span data-ttu-id="fa95f-108">Egy meglévő Node.js-alkalmazással is rendelkeznie kell, amelyet üzembe helyezhet.</span><span class="sxs-lookup"><span data-stu-id="fa95f-108">You also need to have an existing Node.js application for deployment.</span></span> <span data-ttu-id="fa95f-109">Ez a rövid útmutató egy egyszerű Node.js-webhelyet használ, amely [innen][download-sample] tölthető le.</span><span class="sxs-lookup"><span data-stu-id="fa95f-109">This quickstart uses a simple Node.js website that can be downloaded [here][download-sample].</span></span> <span data-ttu-id="fa95f-110">Miután a következő lépésben létrehozza a projektet, csomagolja ki ezt a fájl a következő mappába: `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\`.</span><span class="sxs-lookup"><span data-stu-id="fa95f-110">Extract this file to your `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\` folder after you create the project in the next step.</span></span>

<span data-ttu-id="fa95f-111">Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot][create-account].</span><span class="sxs-lookup"><span data-stu-id="fa95f-111">If you don't have an Azure subscription, create a [free account][create-account].</span></span>

## <a name="create-the-service"></a><span data-ttu-id="fa95f-112">A szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa95f-112">Create the service</span></span>

<span data-ttu-id="fa95f-113">Indítsa el a Visual Studiót **rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="fa95f-113">Launch Visual Studio as an **administrator**.</span></span>

<span data-ttu-id="fa95f-114">Projekt létrehozása `CTRL`+`SHIFT`+`N` használatával</span><span class="sxs-lookup"><span data-stu-id="fa95f-114">Create a project with `CTRL`+`SHIFT`+`N`</span></span>

<span data-ttu-id="fa95f-115">Az **Új projekt** párbeszédpanelen válassza a **Felhő > Service Fabric-alkalmazás** elemet.</span><span class="sxs-lookup"><span data-stu-id="fa95f-115">In the **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

<span data-ttu-id="fa95f-116">Adja a **MyGuestApp** nevet az alkalmazásnak, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="fa95f-116">Name the application **MyGuestApp** and press **OK**.</span></span>

>[!IMPORTANT]
><span data-ttu-id="fa95f-117">A Node.js gyakran túllépi a Windows elérési utakra vonatkozó 260 karakteres korlátját.</span><span class="sxs-lookup"><span data-stu-id="fa95f-117">Node.js can easily break the 260 character limit for paths that windows has.</span></span> <span data-ttu-id="fa95f-118">Használjon rövid elérési utat a projekthez, például: **c:\code\svc1**.</span><span class="sxs-lookup"><span data-stu-id="fa95f-118">Use a short path for the project itself such as **c:\code\svc1**.</span></span>
   
![A Visual Studio Új projekt párbeszédpanelje][new-project]

<span data-ttu-id="fa95f-120">A következő párbeszédpanelen bármilyen típusú Service Fabric-szolgáltatást létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="fa95f-120">You can create any type of Service Fabric service from the next dialog.</span></span> <span data-ttu-id="fa95f-121">Ebben a rövid útmutatóban válassza az **Vendég végrehajtható** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="fa95f-121">For this quickstart, choose **Guest Executable**.</span></span>

<span data-ttu-id="fa95f-122">Adja a **MyGuestService** nevet a szolgáltatásnak, adja meg a következő értékeket a jobb oldali beállításokhoz:</span><span class="sxs-lookup"><span data-stu-id="fa95f-122">Name the service **MyGuestService** and set the options on the right to the following values:</span></span>

| <span data-ttu-id="fa95f-123">Beállítás</span><span class="sxs-lookup"><span data-stu-id="fa95f-123">Setting</span></span>                   | <span data-ttu-id="fa95f-124">Érték</span><span class="sxs-lookup"><span data-stu-id="fa95f-124">Value</span></span> |
| ------------------------- | ------ |
| <span data-ttu-id="fa95f-125">Kódcsomag mappája</span><span class="sxs-lookup"><span data-stu-id="fa95f-125">Code Package Folder</span></span>       | <span data-ttu-id="fa95f-126">_&lt;a mappa, ahol a Node.js-alkalmazás található&gt;_</span><span class="sxs-lookup"><span data-stu-id="fa95f-126">_&lt;the folder with your Node.js app&gt;_</span></span> |
| <span data-ttu-id="fa95f-127">Kódcsomag viselkedése</span><span class="sxs-lookup"><span data-stu-id="fa95f-127">Code Package Behavior</span></span>     | <span data-ttu-id="fa95f-128">A mappa tartalmának másolása a projektbe</span><span class="sxs-lookup"><span data-stu-id="fa95f-128">Copy folder contents to project</span></span> |
| <span data-ttu-id="fa95f-129">Program</span><span class="sxs-lookup"><span data-stu-id="fa95f-129">Program</span></span>                   | <span data-ttu-id="fa95f-130">node.exe</span><span class="sxs-lookup"><span data-stu-id="fa95f-130">node.exe</span></span> |
| <span data-ttu-id="fa95f-131">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="fa95f-131">Arguments</span></span>                 | <span data-ttu-id="fa95f-132">server.js</span><span class="sxs-lookup"><span data-stu-id="fa95f-132">server.js</span></span> |
| <span data-ttu-id="fa95f-133">Munkamappa</span><span class="sxs-lookup"><span data-stu-id="fa95f-133">Working Folder</span></span>            | <span data-ttu-id="fa95f-134">CodePackage</span><span class="sxs-lookup"><span data-stu-id="fa95f-134">CodePackage</span></span> |

<span data-ttu-id="fa95f-135">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="fa95f-135">Press **OK**.</span></span>

![A Visual Studio Új szolgáltatás párbeszédpanelje][new-service]

<span data-ttu-id="fa95f-137">A Visual Studio létrehozza az alkalmazásprojektet és az aktorszolgáltatás-projektet, és megjeleníti őket a Megoldáskezelőben.</span><span class="sxs-lookup"><span data-stu-id="fa95f-137">Visual Studio creates the application project and the actor service project and displays them in Solution Explorer.</span></span>

<span data-ttu-id="fa95f-138">Az alkalmazásprojekt (**MyGuestApp**) nem tartalmaz közvetlenül semmilyen kódot.</span><span class="sxs-lookup"><span data-stu-id="fa95f-138">The application project (**MyGuestApp**) does not contain any code directly.</span></span> <span data-ttu-id="fa95f-139">Helyette számos szolgáltatási projektre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="fa95f-139">Instead, it references a set of service projects.</span></span> <span data-ttu-id="fa95f-140">Ezenfelül három egyéb típusú tartalmat is tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="fa95f-140">In addition, it contains three other types of content:</span></span>

* <span data-ttu-id="fa95f-141">**Profilok közzététele**</span><span class="sxs-lookup"><span data-stu-id="fa95f-141">**Publish profiles**</span></span>  
<span data-ttu-id="fa95f-142">Különböző környezetek eszközbeállításai.</span><span class="sxs-lookup"><span data-stu-id="fa95f-142">Tooling preferences for different environments.</span></span>

* <span data-ttu-id="fa95f-143">**Szkriptek**</span><span class="sxs-lookup"><span data-stu-id="fa95f-143">**Scripts**</span></span>  
<span data-ttu-id="fa95f-144">Az alkalmazás üzembe helyezéséhez/frissítéséhez szükséges PowerShell-szkript.</span><span class="sxs-lookup"><span data-stu-id="fa95f-144">PowerShell script for deploying/upgrading your application.</span></span>

* <span data-ttu-id="fa95f-145">**Alkalmazásdefiníció**</span><span class="sxs-lookup"><span data-stu-id="fa95f-145">**Application definition**</span></span>  
<span data-ttu-id="fa95f-146">Tartalmazza az alkalmazásjegyzéket az *ApplicationPackageRoot* területen.</span><span class="sxs-lookup"><span data-stu-id="fa95f-146">Includes the application manifest under *ApplicationPackageRoot*.</span></span> <span data-ttu-id="fa95f-147">A társított alkalmazások *ApplicationParameters* területen található paraméterfájljai határozzák meg az alkalmazást, és teszik lehetővé az adott környezetnek megfelelően történő konfigurálást.</span><span class="sxs-lookup"><span data-stu-id="fa95f-147">Associated application parameter files are under *ApplicationParameters*, which define the application and allow you to configure it specifically for a given environment.</span></span>
    
<span data-ttu-id="fa95f-148">A szolgáltatási projekt tartalmának áttekintéséhez lásd: [Bevezetés a Reliable Services használatába](service-fabric-reliable-services-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="fa95f-148">For an overview of the contents of the service project, see [Getting started with Reliable Services](service-fabric-reliable-services-quick-start.md).</span></span>

## <a name="set-up-networking"></a><span data-ttu-id="fa95f-149">A hálózat beállítása</span><span class="sxs-lookup"><span data-stu-id="fa95f-149">Set up networking</span></span>

<span data-ttu-id="fa95f-150">A példában üzembe helyezett Node.js-alkalmazás a **80**-as portot használja, és a Service Fabricnak tudnia kell, hogy ennek a portnak elérhetőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fa95f-150">The example Node.js app we're deploying uses port **80** and we need to tell Service Fabric that we need that port exposed.</span></span>

<span data-ttu-id="fa95f-151">Nyissa meg a **ServiceManifest.xml** fájlt a projektben.</span><span class="sxs-lookup"><span data-stu-id="fa95f-151">Open the **ServiceManifest.xml** file in the project.</span></span> <span data-ttu-id="fa95f-152">A jegyzék végén található egy `<Resources> \ <Endpoints>` elem, amelyhez már meg van határozva egy bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="fa95f-152">At the bottom of the manifest, there is a `<Resources> \ <Endpoints>` with an entry already defined.</span></span> <span data-ttu-id="fa95f-153">Módosítsa ezt a bejegyzést a `Port`, a `Protocol` és a `Type` hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="fa95f-153">Modify that entry to add `Port`, `Protocol`, and `Type`.</span></span> 

```xml
  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyGuestAppServiceTypeEndpoint" Port="80" Protocol="http" Type="Input" />
    </Endpoints>
  </Resources>
```

## <a name="deploy-to-azure"></a><span data-ttu-id="fa95f-154">Üzembe helyezés az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="fa95f-154">Deploy to Azure</span></span>

<span data-ttu-id="fa95f-155">Ha az **F5** billentyű lenyomásával futtathatja a projektet, az üzembe lesz helyezve a helyi fürtön.</span><span class="sxs-lookup"><span data-stu-id="fa95f-155">If you press **F5** and run the project, it is deployed to the local cluster.</span></span> <span data-ttu-id="fa95f-156">Azonban helyezzük üzembe inkább az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="fa95f-156">However, let's deploy to Azure instead.</span></span>

<span data-ttu-id="fa95f-157">Kattintson a jobb gombbal a projektre, és válassza a **Közzététel** lehetőséget, amely egy párbeszédpanelt nyit meg az Azure-on történő közzétételhez.</span><span class="sxs-lookup"><span data-stu-id="fa95f-157">Right-click on the project and choose **Publish...** which opens a dialog to publish to Azure.</span></span>

![Service Fabric-szolgáltatáshoz tartozó Közzététel az Azure-ba párbeszédpanel][publish]

<span data-ttu-id="fa95f-159">Válassza ki a **PublishProfiles\Cloud.xml** célprofilt.</span><span class="sxs-lookup"><span data-stu-id="fa95f-159">Select the **PublishProfiles\Cloud.xml** target profile.</span></span>

<span data-ttu-id="fa95f-160">Ha korábban még nem tette meg, válassza ki az üzembe helyezés céljaként szolgáló Azure-fiókot.</span><span class="sxs-lookup"><span data-stu-id="fa95f-160">If you haven't previously, choose an Azure account to deploy to.</span></span> <span data-ttu-id="fa95f-161">Ha még nem rendelkezik ilyen fiókkal, [regisztráljon egyet][create-account].</span><span class="sxs-lookup"><span data-stu-id="fa95f-161">If you don't have one yet, [sign-up for one][create-account].</span></span>

<span data-ttu-id="fa95f-162">A **Kapcsolati végpont** területen válassza ki az üzembe helyezés céljaként szolgáló Service Fabric-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="fa95f-162">Under **Connection Endpoint**, select the Service Fabric cluster to deploy to.</span></span> <span data-ttu-id="fa95f-163">Ha még nem rendelkezik ilyen fürttel, válassza az **&lt;Új fürt létrehozása&gt;** lehetőséget, amely új webböngésző-ablakot nyit meg az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="fa95f-163">If you do not have one, select **&lt;Create New Cluster...&gt;** which opens up web browser window to the Azure portal.</span></span> <span data-ttu-id="fa95f-164">További információért lásd: [Fürt létrehozása a portálon](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="fa95f-164">For more information, see [create a cluster in the portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal).</span></span> 

<span data-ttu-id="fa95f-165">A Service Fabric-fürt létrehozásakor ügyeljen rá, hogy az **Egyéni végpontok** beállítás értéke **80** legyen.</span><span class="sxs-lookup"><span data-stu-id="fa95f-165">When you create the Service Fabric cluster, make sure to set the **Custom endpoints** setting to **80**.</span></span>

![Service Fabric-csomóponttípus konfigurálása egyéni végponttal][custom-endpoint]

<span data-ttu-id="fa95f-167">Az új Service Fabric-fürt létrehozása eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="fa95f-167">Creating a new Service Fabric cluster takes some time to complete.</span></span> <span data-ttu-id="fa95f-168">A létrehozás után térjen vissza a közzétételi párbeszédpanelre, és válassza a **&lt;Frissítés&gt;** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="fa95f-168">Once it has been created, go back to the publish dialog and select **&lt;Refresh&gt;**.</span></span> <span data-ttu-id="fa95f-169">Válassza ki a legördülő listában megjelenő új fürtöt.</span><span class="sxs-lookup"><span data-stu-id="fa95f-169">The new cluster is listed in the drop-down box; select it.</span></span>

<span data-ttu-id="fa95f-170">Kattintson a **Közzététel** gombra, és várjon, amíg az üzembe helyezés befejeződik.</span><span class="sxs-lookup"><span data-stu-id="fa95f-170">Press **Publish** and wait for the deployment to finish.</span></span>

<span data-ttu-id="fa95f-171">Ez eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="fa95f-171">This may take a few minutes.</span></span> <span data-ttu-id="fa95f-172">Az üzembe helyezés után még néhány percet várnia kell, amíg az alkalmazás teljes mértékben elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="fa95f-172">After it completes, it may take a few more minutes for the application to be fully available.</span></span>

## <a name="test-the-website"></a><span data-ttu-id="fa95f-173">A webhely tesztelése</span><span class="sxs-lookup"><span data-stu-id="fa95f-173">Test the website</span></span>

<span data-ttu-id="fa95f-174">A szolgáltatás közzététel után tesztelje a szolgáltatást egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="fa95f-174">After your service has been published, test it in a web browser.</span></span> 

<span data-ttu-id="fa95f-175">Először nyissa meg az Azure Portalt, majd keresse meg a Service Fabric-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fa95f-175">First, open the Azure portal and find your Service Fabric service.</span></span>

<span data-ttu-id="fa95f-176">Ellenőrizze a szolgáltatás címének Áttekintés paneljét.</span><span class="sxs-lookup"><span data-stu-id="fa95f-176">Check the overview blade of the service address.</span></span> <span data-ttu-id="fa95f-177">Használja az _Ügyfélkapcsolati végpont_ tulajdonságban szereplő tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="fa95f-177">Use the domain name from the _Client connection endpoint_ property.</span></span> <span data-ttu-id="fa95f-178">Például: `http://mysvcfab1.westus2.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="fa95f-178">For example, `http://mysvcfab1.westus2.cloudapp.azure.com`.</span></span>

![A Service Fabric áttekintési panelje az Azure Portalon][overview]

<span data-ttu-id="fa95f-180">Keresse meg ezt a címet, ahol láthatja a `HELLO WORLD` választ.</span><span class="sxs-lookup"><span data-stu-id="fa95f-180">Navigate to this address where you will see the `HELLO WORLD` response.</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="fa95f-181">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="fa95f-181">Delete the cluster</span></span>

<span data-ttu-id="fa95f-182">Ne felejtse el törölni az összes erőforrást, amelyet létrehozott ebben az útmutatóban, mert ezek fizetősek.</span><span class="sxs-lookup"><span data-stu-id="fa95f-182">Do not forget to delete all of the resources you've created for this quickstart, as you are charged for those resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa95f-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fa95f-183">Next steps</span></span>
<span data-ttu-id="fa95f-184">További információk a [futtatható vendégalkalmazásokról](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="fa95f-184">Read more about [guest executables](service-fabric-deploy-existing-app.md).</span></span>

<!-- Image References -->

[new-project]: ./media/quickstart-guest-app/new-project.png
[new-service]: ./media/quickstart-guest-app/template.png
[solution-exp]: ./media/quickstart-guest-app/solution-explorer.png
[publish]: ./media/quickstart-guest-app/publish.png
[overview]: ./media/quickstart-guest-app/overview.png
[custom-endpoint]: ./media/quickstart-guest-app/custom-endpoint.png

[download-sample]: https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/service-fabric-node-website.zip
[create-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F