---
title: "aaaQuickly meglévő app tooan Azure Service Fabric-fürt üzembe helyezése"
description: "Az Azure Service Fabric-fürt toohost egy meglévő Node.js alkalmazást használja a Visual Studio."
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
ms.openlocfilehash: 20a3eb4a9206ba465acf96d0976ba241b07158bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="host-a-nodejs-application-on-azure-service-fabric"></a><span data-ttu-id="1ef10-103">Node.js-alkalmazás üzemeltetése az Azure Service Fabricban</span><span class="sxs-lookup"><span data-stu-id="1ef10-103">Host a Node.js application on Azure Service Fabric</span></span>

<span data-ttu-id="1ef10-104">A gyors üzembe helyezés a meglévő alkalmazás (Node.js ebben a példában) tooa Service Fabric-fürt üzembe helyezése az Azure-on futó nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="1ef10-104">This quickstart helps you deploy an existing application (Node.js in this example) tooa Service Fabric cluster running on Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1ef10-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1ef10-105">Prerequisites</span></span>

<span data-ttu-id="1ef10-106">Mielőtt elkezdené, győződjön meg arról, hogy [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1ef10-106">Before you get started, make sure that you have [set up your development environment](service-fabric-get-started.md).</span></span> <span data-ttu-id="1ef10-107">Amely magában foglalja a Service Fabric SDK hello és a Visual Studio 2017 vagy 2015 telepítése.</span><span class="sxs-lookup"><span data-stu-id="1ef10-107">Which includes installing hello Service Fabric SDK and Visual Studio 2017 or 2015.</span></span>

<span data-ttu-id="1ef10-108">Szükség toohave egy meglévő Node.js-alkalmazás központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="1ef10-108">You also need toohave an existing Node.js application for deployment.</span></span> <span data-ttu-id="1ef10-109">Ez a rövid útmutató egy egyszerű Node.js-webhelyet használ, amely [innen][download-sample] tölthető le.</span><span class="sxs-lookup"><span data-stu-id="1ef10-109">This quickstart uses a simple Node.js website that can be downloaded [here][download-sample].</span></span> <span data-ttu-id="1ef10-110">Bontsa ki a fájl tooyour `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\` mappa a következő lépésben hello hello projekt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="1ef10-110">Extract this file tooyour `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\` folder after you create hello project in hello next step.</span></span>

<span data-ttu-id="1ef10-111">Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot][create-account].</span><span class="sxs-lookup"><span data-stu-id="1ef10-111">If you don't have an Azure subscription, create a [free account][create-account].</span></span>

## <a name="create-hello-service"></a><span data-ttu-id="1ef10-112">Hello szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ef10-112">Create hello service</span></span>

<span data-ttu-id="1ef10-113">Indítsa el a Visual Studiót **rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="1ef10-113">Launch Visual Studio as an **administrator**.</span></span>

<span data-ttu-id="1ef10-114">Projekt létrehozása `CTRL`+`SHIFT`+`N` használatával</span><span class="sxs-lookup"><span data-stu-id="1ef10-114">Create a project with `CTRL`+`SHIFT`+`N`</span></span>

<span data-ttu-id="1ef10-115">A hello **új projekt** párbeszédpanelen válasszon **felhő > Service Fabric-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1ef10-115">In hello **New Project** dialog, choose **Cloud > Service Fabric Application**.</span></span>

<span data-ttu-id="1ef10-116">Hello alkalmazás neve **MyGuestApp** nyomja le az ENTER **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ef10-116">Name hello application **MyGuestApp** and press **OK**.</span></span>

>[!IMPORTANT]
><span data-ttu-id="1ef10-117">NODE.js könnyen érvénytelenné elérési utak, amelyen a windows hello 260 karakteres korlátot.</span><span class="sxs-lookup"><span data-stu-id="1ef10-117">Node.js can easily break hello 260 character limit for paths that windows has.</span></span> <span data-ttu-id="1ef10-118">Használjon például rövid elérési utat maga hello projekt **c:\code\svc1**.</span><span class="sxs-lookup"><span data-stu-id="1ef10-118">Use a short path for hello project itself such as **c:\code\svc1**.</span></span>
   
![A Visual Studio Új projekt párbeszédpanelje][new-project]

<span data-ttu-id="1ef10-120">Service Fabric-szolgáltatás bármilyen típusú hello tovább párbeszédpanelről hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="1ef10-120">You can create any type of Service Fabric service from hello next dialog.</span></span> <span data-ttu-id="1ef10-121">Ebben a rövid útmutatóban válassza az **Vendég végrehajtható** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1ef10-121">For this quickstart, choose **Guest Executable**.</span></span>

<span data-ttu-id="1ef10-122">Hello szolgáltatás **MyGuestService** és a jobb oldali toohello hello hello-beállítások megadása a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="1ef10-122">Name hello service **MyGuestService** and set hello options on hello right toohello following values:</span></span>

| <span data-ttu-id="1ef10-123">Beállítás</span><span class="sxs-lookup"><span data-stu-id="1ef10-123">Setting</span></span>                   | <span data-ttu-id="1ef10-124">Érték</span><span class="sxs-lookup"><span data-stu-id="1ef10-124">Value</span></span> |
| ------------------------- | ------ |
| <span data-ttu-id="1ef10-125">Kódcsomag mappája</span><span class="sxs-lookup"><span data-stu-id="1ef10-125">Code Package Folder</span></span>       | <span data-ttu-id="1ef10-126">_&lt;hello mappában, amelynek a Node.js-alkalmazás&gt;_</span><span class="sxs-lookup"><span data-stu-id="1ef10-126">_&lt;hello folder with your Node.js app&gt;_</span></span> |
| <span data-ttu-id="1ef10-127">Kódcsomag viselkedése</span><span class="sxs-lookup"><span data-stu-id="1ef10-127">Code Package Behavior</span></span>     | <span data-ttu-id="1ef10-128">Mappa tartalmának tooproject másolása</span><span class="sxs-lookup"><span data-stu-id="1ef10-128">Copy folder contents tooproject</span></span> |
| <span data-ttu-id="1ef10-129">Program</span><span class="sxs-lookup"><span data-stu-id="1ef10-129">Program</span></span>                   | <span data-ttu-id="1ef10-130">node.exe</span><span class="sxs-lookup"><span data-stu-id="1ef10-130">node.exe</span></span> |
| <span data-ttu-id="1ef10-131">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="1ef10-131">Arguments</span></span>                 | <span data-ttu-id="1ef10-132">server.js</span><span class="sxs-lookup"><span data-stu-id="1ef10-132">server.js</span></span> |
| <span data-ttu-id="1ef10-133">Munkamappa</span><span class="sxs-lookup"><span data-stu-id="1ef10-133">Working Folder</span></span>            | <span data-ttu-id="1ef10-134">CodePackage</span><span class="sxs-lookup"><span data-stu-id="1ef10-134">CodePackage</span></span> |

<span data-ttu-id="1ef10-135">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1ef10-135">Press **OK**.</span></span>

![A Visual Studio Új szolgáltatás párbeszédpanelje][new-service]

<span data-ttu-id="1ef10-137">A Visual Studio hello alkalmazási projektet és hello szereplő projektet hoz létre, és megjeleníti őket a Megoldáskezelőben.</span><span class="sxs-lookup"><span data-stu-id="1ef10-137">Visual Studio creates hello application project and hello actor service project and displays them in Solution Explorer.</span></span>

<span data-ttu-id="1ef10-138">hello projektet (**MyGuestApp**) nem tartalmaz közvetlenül semmilyen kódot.</span><span class="sxs-lookup"><span data-stu-id="1ef10-138">hello application project (**MyGuestApp**) does not contain any code directly.</span></span> <span data-ttu-id="1ef10-139">Helyette számos szolgáltatási projektre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="1ef10-139">Instead, it references a set of service projects.</span></span> <span data-ttu-id="1ef10-140">Ezenfelül három egyéb típusú tartalmat is tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="1ef10-140">In addition, it contains three other types of content:</span></span>

* <span data-ttu-id="1ef10-141">**Profilok közzététele**</span><span class="sxs-lookup"><span data-stu-id="1ef10-141">**Publish profiles**</span></span>  
<span data-ttu-id="1ef10-142">Különböző környezetek eszközbeállításai.</span><span class="sxs-lookup"><span data-stu-id="1ef10-142">Tooling preferences for different environments.</span></span>

* <span data-ttu-id="1ef10-143">**Szkriptek**</span><span class="sxs-lookup"><span data-stu-id="1ef10-143">**Scripts**</span></span>  
<span data-ttu-id="1ef10-144">Az alkalmazás üzembe helyezéséhez/frissítéséhez szükséges PowerShell-szkript.</span><span class="sxs-lookup"><span data-stu-id="1ef10-144">PowerShell script for deploying/upgrading your application.</span></span>

* <span data-ttu-id="1ef10-145">**Alkalmazásdefiníció**</span><span class="sxs-lookup"><span data-stu-id="1ef10-145">**Application definition**</span></span>  
<span data-ttu-id="1ef10-146">Magában foglalja az alkalmazásjegyzék hello alatt *ApplicationPackageRoot*.</span><span class="sxs-lookup"><span data-stu-id="1ef10-146">Includes hello application manifest under *ApplicationPackageRoot*.</span></span> <span data-ttu-id="1ef10-147">Kapcsolódó alkalmazásparaméter-fájlokat a rendszer *ApplicationParameters*, amely hello alkalmazás meghatározása, és lehetővé teszik tooconfigure kifejezetten az adott környezetben.</span><span class="sxs-lookup"><span data-stu-id="1ef10-147">Associated application parameter files are under *ApplicationParameters*, which define hello application and allow you tooconfigure it specifically for a given environment.</span></span>
    
<span data-ttu-id="1ef10-148">Hello hello szolgáltatási projekt tartalmának áttekintéséhez lásd: [Bevezetés a Reliable Services használatába](service-fabric-reliable-services-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="1ef10-148">For an overview of hello contents of hello service project, see [Getting started with Reliable Services](service-fabric-reliable-services-quick-start.md).</span></span>

## <a name="set-up-networking"></a><span data-ttu-id="1ef10-149">A hálózat beállítása</span><span class="sxs-lookup"><span data-stu-id="1ef10-149">Set up networking</span></span>

<span data-ttu-id="1ef10-150">Node.js-alkalmazás telepít azt a portot használja hello példa **80** ezért ellenőriznünk kell, hogy elérhetővé tett port szükséges Service Fabric tootell.</span><span class="sxs-lookup"><span data-stu-id="1ef10-150">hello example Node.js app we're deploying uses port **80** and we need tootell Service Fabric that we need that port exposed.</span></span>

<span data-ttu-id="1ef10-151">Nyissa meg hello **ServiceManifest.xml** fájl hello projektben.</span><span class="sxs-lookup"><span data-stu-id="1ef10-151">Open hello **ServiceManifest.xml** file in hello project.</span></span> <span data-ttu-id="1ef10-152">Hello jegyzékfájl hello alján van egy `<Resources> \ <Endpoints>` olyan bejegyzés már meg van adva.</span><span class="sxs-lookup"><span data-stu-id="1ef10-152">At hello bottom of hello manifest, there is a `<Resources> \ <Endpoints>` with an entry already defined.</span></span> <span data-ttu-id="1ef10-153">Módosítsa az adott bejegyzés tooadd `Port`, `Protocol`, és `Type`.</span><span class="sxs-lookup"><span data-stu-id="1ef10-153">Modify that entry tooadd `Port`, `Protocol`, and `Type`.</span></span> 

```xml
  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which too
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyGuestAppServiceTypeEndpoint" Port="80" Protocol="http" Type="Input" />
    </Endpoints>
  </Resources>
```

## <a name="deploy-tooazure"></a><span data-ttu-id="1ef10-154">TooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="1ef10-154">Deploy tooAzure</span></span>

<span data-ttu-id="1ef10-155">Ha lenyomja az **F5** , és futtassa a hello projektet, telepített toohello helyi fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1ef10-155">If you press **F5** and run hello project, it is deployed toohello local cluster.</span></span> <span data-ttu-id="1ef10-156">Azonban helyezzünk üzembe tooAzure helyette.</span><span class="sxs-lookup"><span data-stu-id="1ef10-156">However, let's deploy tooAzure instead.</span></span>

<span data-ttu-id="1ef10-157">Kattintson a jobb gombbal a hello projektet, és válassza a **közzététel...**  amely megnyílik egy párbeszédpanel toopublish tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1ef10-157">Right-click on hello project and choose **Publish...** which opens a dialog toopublish tooAzure.</span></span>

![A service fabric-szolgáltatás tooazure párbeszédpanel közzététele][publish]

<span data-ttu-id="1ef10-159">Jelölje be hello **PublishProfiles\Cloud.xml** céloz profil.</span><span class="sxs-lookup"><span data-stu-id="1ef10-159">Select hello **PublishProfiles\Cloud.xml** target profile.</span></span>

<span data-ttu-id="1ef10-160">Ha korábban még nem válasszon egy Azure-fiók toodeploy számára.</span><span class="sxs-lookup"><span data-stu-id="1ef10-160">If you haven't previously, choose an Azure account toodeploy to.</span></span> <span data-ttu-id="1ef10-161">Ha még nem rendelkezik ilyen fiókkal, [regisztráljon egyet][create-account].</span><span class="sxs-lookup"><span data-stu-id="1ef10-161">If you don't have one yet, [sign-up for one][create-account].</span></span>

<span data-ttu-id="1ef10-162">A **csatlakozási végpont**, válassza ki a Service Fabric-fürt toodeploy hello.</span><span class="sxs-lookup"><span data-stu-id="1ef10-162">Under **Connection Endpoint**, select hello Service Fabric cluster toodeploy to.</span></span> <span data-ttu-id="1ef10-163">Ha még nem rendelkezik ilyennel, válassza ki a  **&lt;új fürt létrehozása... &gt;**  amely megnyílik webes böngésző ablak toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1ef10-163">If you do not have one, select **&lt;Create New Cluster...&gt;** which opens up web browser window toohello Azure portal.</span></span> <span data-ttu-id="1ef10-164">További információkért lásd: [hello portálon hozzon létre egy fürtöt](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="1ef10-164">For more information, see [create a cluster in hello portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal).</span></span> 

<span data-ttu-id="1ef10-165">Amikor hello Service Fabric-fürtöt hoz létre, győződjön meg arról, hogy tooset hello **egyéni végpontok** beállítás túl**80**.</span><span class="sxs-lookup"><span data-stu-id="1ef10-165">When you create hello Service Fabric cluster, make sure tooset hello **Custom endpoints** setting too**80**.</span></span>

![Service Fabric-csomóponttípus konfigurálása egyéni végponttal][custom-endpoint]

<span data-ttu-id="1ef10-167">Bizonyos idő toocomplete egy új Service Fabric-fürt létrehozása vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="1ef10-167">Creating a new Service Fabric cluster takes some time toocomplete.</span></span> <span data-ttu-id="1ef10-168">Azt követően létrehozott, lépjen vissza toohello közzétételére párbeszédpanelt, és válassza ki  **&lt;frissítése&gt;**.</span><span class="sxs-lookup"><span data-stu-id="1ef10-168">Once it has been created, go back toohello publish dialog and select **&lt;Refresh&gt;**.</span></span> <span data-ttu-id="1ef10-169">hello új fürt szerepel az hello legördülő; Válassza ki azt.</span><span class="sxs-lookup"><span data-stu-id="1ef10-169">hello new cluster is listed in hello drop-down box; select it.</span></span>

<span data-ttu-id="1ef10-170">Nyomja le az **közzététel** és hello telepítési toofinish várja.</span><span class="sxs-lookup"><span data-stu-id="1ef10-170">Press **Publish** and wait for hello deployment toofinish.</span></span>

<span data-ttu-id="1ef10-171">Ez eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="1ef10-171">This may take a few minutes.</span></span> <span data-ttu-id="1ef10-172">Miután befejeződött, hello alkalmazás toobe teljes rendelkezésre álló néhány percet is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="1ef10-172">After it completes, it may take a few more minutes for hello application toobe fully available.</span></span>

## <a name="test-hello-website"></a><span data-ttu-id="1ef10-173">Teszt hello webhely</span><span class="sxs-lookup"><span data-stu-id="1ef10-173">Test hello website</span></span>

<span data-ttu-id="1ef10-174">A szolgáltatás közzététel után tesztelje a szolgáltatást egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="1ef10-174">After your service has been published, test it in a web browser.</span></span> 

<span data-ttu-id="1ef10-175">Először nyissa meg a hello Azure-portálon, és a Service Fabric-szolgáltatás található.</span><span class="sxs-lookup"><span data-stu-id="1ef10-175">First, open hello Azure portal and find your Service Fabric service.</span></span>

<span data-ttu-id="1ef10-176">Hello áttekintése panel hello szolgáltatás címének ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="1ef10-176">Check hello overview blade of hello service address.</span></span> <span data-ttu-id="1ef10-177">Hello használata hello tartománynévnek _ügyfél-csatlakozási végpont_ tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="1ef10-177">Use hello domain name from hello _Client connection endpoint_ property.</span></span> <span data-ttu-id="1ef10-178">Például: `http://mysvcfab1.westus2.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="1ef10-178">For example, `http://mysvcfab1.westus2.cloudapp.azure.com`.</span></span>

![Service fabric áttekintése paneljét hello Azure-portálon][overview]

<span data-ttu-id="1ef10-180">Keresse meg a toothis cím megjelenik hello `HELLO WORLD` választ.</span><span class="sxs-lookup"><span data-stu-id="1ef10-180">Navigate toothis address where you will see hello `HELLO WORLD` response.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="1ef10-181">Hello fürt törlése</span><span class="sxs-lookup"><span data-stu-id="1ef10-181">Delete hello cluster</span></span>

<span data-ttu-id="1ef10-182">Ne feledje toodelete minden Ön által létrehozott a gyors üzembe helyezés, ha Ön hello erőforrás van szó, az adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="1ef10-182">Do not forget toodelete all of hello resources you've created for this quickstart, as you are charged for those resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ef10-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1ef10-183">Next steps</span></span>
<span data-ttu-id="1ef10-184">További információk a [futtatható vendégalkalmazásokról](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="1ef10-184">Read more about [guest executables](service-fabric-deploy-existing-app.md).</span></span>

<!-- Image References -->

[new-project]: ./media/quickstart-guest-app/new-project.png
[new-service]: ./media/quickstart-guest-app/template.png
[solution-exp]: ./media/quickstart-guest-app/solution-explorer.png
[publish]: ./media/quickstart-guest-app/publish.png
[overview]: ./media/quickstart-guest-app/overview.png
[custom-endpoint]: ./media/quickstart-guest-app/custom-endpoint.png

[download-sample]: https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/service-fabric-node-website.zip
[create-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F