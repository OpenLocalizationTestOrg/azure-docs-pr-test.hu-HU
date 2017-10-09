---
title: "aaaNode.js – első lépések útmutató |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy egyszerű Node.js webalkalmazást, majd központilag telepítenie tooan Azure felhőszolgáltatást."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 50951a87-fed4-48e0-bcfa-453b9e50452e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 22945bfcc1b0e5da2a2d37dc5cc86be013cc0b5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-a-nodejs-application-tooan-azure-cloud-service"></a><span data-ttu-id="3bae4-103">Hozza létre, és a Node.js-alkalmazás tooan Azure Cloud Service telepítése</span><span class="sxs-lookup"><span data-stu-id="3bae4-103">Build and deploy a Node.js application tooan Azure Cloud Service</span></span>

<span data-ttu-id="3bae4-104">Ez az oktatóanyag bemutatja, hogyan toocreate egy egyszerű Node.js az Azure-Felhőszolgáltatásban futó alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3bae4-104">This tutorial shows how toocreate a simple Node.js application running in an Azure Cloud Service.</span></span> <span data-ttu-id="3bae4-105">Cloud Services csomag építőelemei hello méretezhető felhőalapú alkalmazások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="3bae4-105">Cloud Services are hello building blocks of scalable cloud applications in Azure.</span></span> <span data-ttu-id="3bae4-106">Hello elkülönítését és egymástól független kezelését és az alkalmazás előtér- és összetevők kibővített lehetővé teszik.</span><span class="sxs-lookup"><span data-stu-id="3bae4-106">They allow hello separation and independent management and scale-out of front-end and back-end components of your application.</span></span>  <span data-ttu-id="3bae4-107">A Cloud Services egy robusztus, dedikált virtuális gépet biztosít az egyes szerepkörök megbízható üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="3bae4-107">Cloud Services provide a robust dedicated virtual machine for hosting each role reliably.</span></span>

<span data-ttu-id="3bae4-108">A felhőalapú szolgáltatások, valamint annak összevetése tooAzure webhelyek és a virtuális gépek további információkért lásd: [Azure Websites, a Cloud Services és a virtuális gépek összevetése].</span><span class="sxs-lookup"><span data-stu-id="3bae4-108">For more information on Cloud Services, and how they compare tooAzure Websites and Virtual machines, see [Azure Websites, Cloud Services and Virtual Machines comparison].</span></span>

> [!TIP]
> <span data-ttu-id="3bae4-109">Egy egyszerű webhely toobuild szüksége?</span><span class="sxs-lookup"><span data-stu-id="3bae4-109">Looking toobuild a simple website?</span></span> <span data-ttu-id="3bae4-110">Ha csak egy egyszerű webhely előterét kívánja futtatni, fontolja meg egy [egyszerűsített webalkalmazás használatát].</span><span class="sxs-lookup"><span data-stu-id="3bae4-110">If your scenario involves just a simple website front-end, consider [using a lightweight web app].</span></span> <span data-ttu-id="3bae4-111">A felhőalapú szolgáltatás tooa könnyedén frissíthet, ha a webalkalmazás növekszik és a követelmények változnak.</span><span class="sxs-lookup"><span data-stu-id="3bae4-111">You can easily upgrade tooa Cloud Service as your web app grows and your requirements change.</span></span>

<span data-ttu-id="3bae4-112">Az oktatóanyag utasításait követve egy webes szerepkörben lévő egyszerű webalkalmazást fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3bae4-112">By following this tutorial, you will build a simple web application hosted inside a web role.</span></span> <span data-ttu-id="3bae4-113">Ön szolgáltatás hello compute emulator tootest az alkalmazást helyileg, majd PowerShell parancssori eszközök használatával telepítse.</span><span class="sxs-lookup"><span data-stu-id="3bae4-113">You will use hello compute emulator tootest your application locally, then deploy it using PowerShell command-line tools.</span></span>

<span data-ttu-id="3bae4-114">hello alkalmazás egy egyszerű "hello world" alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="3bae4-114">hello application is a simple "hello world" application:</span></span>

![A webböngészőben Hello World hello weblapot][A web browser displaying hello Hello World web page]

## <a name="prerequisites"></a><span data-ttu-id="3bae4-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3bae4-116">Prerequisites</span></span>
> [!NOTE]
> <span data-ttu-id="3bae4-117">A jelen oktatóanyagban szereplő Azure PowerShell használatához Windows rendszer szükséges.</span><span class="sxs-lookup"><span data-stu-id="3bae4-117">This tutorial uses Azure PowerShell, which requires Windows.</span></span>

* <span data-ttu-id="3bae4-118">Telepítse és konfigurálja az [Azure PowerShell] eszközt.</span><span class="sxs-lookup"><span data-stu-id="3bae4-118">Install and configure [Azure Powershell].</span></span>
* <span data-ttu-id="3bae4-119">Töltse le és telepítse a hello [Azure SDK for .NET 2.7].</span><span class="sxs-lookup"><span data-stu-id="3bae4-119">Download and install hello [Azure SDK for .NET 2.7].</span></span> <span data-ttu-id="3bae4-120">Hello telepítő telepíti, válassza ki:</span><span class="sxs-lookup"><span data-stu-id="3bae4-120">In hello install setup, select:</span></span>
  * <span data-ttu-id="3bae4-121">MicrosoftAzureAuthoringTools</span><span class="sxs-lookup"><span data-stu-id="3bae4-121">MicrosoftAzureAuthoringTools</span></span>
  * <span data-ttu-id="3bae4-122">MicrosoftAzureComputeEmulator</span><span class="sxs-lookup"><span data-stu-id="3bae4-122">MicrosoftAzureComputeEmulator</span></span>

## <a name="create-an-azure-cloud-service-project"></a><span data-ttu-id="3bae4-123">Azure Cloud Service-projektet létrehozása</span><span class="sxs-lookup"><span data-stu-id="3bae4-123">Create an Azure Cloud Service project</span></span>
<span data-ttu-id="3bae4-124">Hajtsa végre a következő feladatok toocreate egy új Azure Cloud Service-projekt alapszintű Node.js szerkezettel hello:</span><span class="sxs-lookup"><span data-stu-id="3bae4-124">Perform hello following tasks toocreate a new Azure Cloud Service project, along with basic Node.js scaffolding:</span></span>

1. <span data-ttu-id="3bae4-125">Futtatás **Windows PowerShell** eszközt rendszergazdaként: hello **Start menü** vagy **kezdőképernyőn**, keressen **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="3bae4-125">Run **Windows PowerShell** as Administrator; from hello **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span>
2. <span data-ttu-id="3bae4-126">[PowerShell összekapcsolása] tooyour előfizetés.</span><span class="sxs-lookup"><span data-stu-id="3bae4-126">[Connect PowerShell] tooyour subscription.</span></span>
3. <span data-ttu-id="3bae4-127">Adja meg a következő PowerShell parancsmagot toocreate toocreate hello projekt hello:</span><span class="sxs-lookup"><span data-stu-id="3bae4-127">Enter hello following PowerShell cmdlet toocreate toocreate hello project:</span></span>

        New-AzureServiceProject helloworld

    ![hello hello New-AzureService helloworld parancs eredménye][hello result of hello New-AzureService helloworld command]

    <span data-ttu-id="3bae4-129">Hello **New-AzureServiceProject** parancsmag létrehoz egy alapszintű struktúrát egy Node.js-alkalmazás tooa felhőalapú szolgáltatás-közzététel.</span><span class="sxs-lookup"><span data-stu-id="3bae4-129">hello **New-AzureServiceProject** cmdlet generates a basic structure for publishing a Node.js application tooa Cloud Service.</span></span> <span data-ttu-id="3bae4-130">Közzétételi tooAzure szükséges konfigurációs fájlokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3bae4-130">It contains configuration files necessary for publishing tooAzure.</span></span> <span data-ttu-id="3bae4-131">hello parancsmag módosítja a directory toohello munkakönyvtára hello szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="3bae4-131">hello cmdlet also changes your working directory toohello directory for hello service.</span></span>

    <span data-ttu-id="3bae4-132">hello parancsmag hozza létre a következő fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="3bae4-132">hello cmdlet creates hello following files:</span></span>

   * <span data-ttu-id="3bae4-133">**ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** és **ServiceDefinition.csdef**: az alkalmazás közzétételéhez szükséges Azure-specifikus fájlok.</span><span class="sxs-lookup"><span data-stu-id="3bae4-133">**ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** and **ServiceDefinition.csdef**: Azure-specific files necessary for publishing your application.</span></span> <span data-ttu-id="3bae4-134">További információkért lásd: [Üzemeltetett szolgáltatás létrehozása az Azure-ban – áttekintés].</span><span class="sxs-lookup"><span data-stu-id="3bae4-134">For more information, see [Overview of Creating a Hosted Service for Azure].</span></span>
   * <span data-ttu-id="3bae4-135">**deploymentSettings.json**: hello Azure PowerShell telepítési parancsmagok által használt helyi beállításokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="3bae4-135">**deploymentSettings.json**: Stores local settings that are used by hello Azure PowerShell deployment cmdlets.</span></span>
4. <span data-ttu-id="3bae4-136">Adja meg a következő parancs tooadd új webes szerepkör hello:</span><span class="sxs-lookup"><span data-stu-id="3bae4-136">Enter hello following command tooadd a new web role:</span></span>

       Add-AzureNodeWebRole

   ![hello hello Add-AzureNodeWebRole parancs kimenete][hello output of hello Add-AzureNodeWebRole command]

   <span data-ttu-id="3bae4-138">Hello **Add-AzureNodeWebRole** parancsmag létrehoz egy alapszintű Node.js-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3bae4-138">hello **Add-AzureNodeWebRole** cmdlet creates a basic Node.js application.</span></span> <span data-ttu-id="3bae4-139">Módosítja hello **.csfg** és **.csdef** tooadd konfigurációs bejegyzéseket hello új szerepkör-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="3bae4-139">It also modifies hello **.csfg** and **.csdef** files tooadd configuration entries for hello new role.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3bae4-140">Ha nem ad meg egy nevet a szerepkörhöz, alapértelmezett név lesz használva.</span><span class="sxs-lookup"><span data-stu-id="3bae4-140">If you do not specify a role name, a default name is used.</span></span> <span data-ttu-id="3bae4-141">Hello első parancsmag paraméterként megadhat egy nevet:`Add-AzureNodeWebRole MyRole`</span><span class="sxs-lookup"><span data-stu-id="3bae4-141">You can provide a name as hello first cmdlet parameter: `Add-AzureNodeWebRole MyRole`</span></span>

<span data-ttu-id="3bae4-142">Node.js-alkalmazás hello hello fájlban definiált **server.js**, hello hello webes szerepkör könyvtárában található (**WebRole1** alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="3bae4-142">hello Node.js app is defined in hello file **server.js**, located in hello directory for hello web role (**WebRole1** by default).</span></span> <span data-ttu-id="3bae4-143">Íme hello kódot:</span><span class="sxs-lookup"><span data-stu-id="3bae4-143">Here is hello code:</span></span>

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

<span data-ttu-id="3bae4-144">Ez a kód alapvetően megegyeznek a "Hello World" hello mintát a hello hello [nodejs.org] webhelyen, azzal a különbséggel hello hello felhőkörnyezet által hozzárendelt portszámot használja.</span><span class="sxs-lookup"><span data-stu-id="3bae4-144">This code is essentially hello same as hello "Hello World" sample on hello [nodejs.org] website, except it uses hello port number assigned by hello cloud environment.</span></span>

## <a name="deploy-hello-application-tooazure"></a><span data-ttu-id="3bae4-145">Hello alkalmazás tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="3bae4-145">Deploy hello application tooAzure</span></span>

> [!NOTE]
> <span data-ttu-id="3bae4-146">toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3bae4-146">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="3bae4-147">[Aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF), vagy [regisztrálhat egy ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).</span><span class="sxs-lookup"><span data-stu-id="3bae4-147">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) or [sign up for a free account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).</span></span>

### <a name="download-hello-azure-publishing-settings"></a><span data-ttu-id="3bae4-148">Töltse le a hello Azure közzétételi beállítások</span><span class="sxs-lookup"><span data-stu-id="3bae4-148">Download hello Azure publishing settings</span></span>
<span data-ttu-id="3bae4-149">toodeploy az alkalmazás tooAzure először le kell töltenie hello közzétételi beállításait az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="3bae4-149">toodeploy your application tooAzure, you must first download hello publishing settings for your Azure subscription.</span></span>

1. <span data-ttu-id="3bae4-150">Futtassa a következő Azure PowerShell-parancsmag hello:</span><span class="sxs-lookup"><span data-stu-id="3bae4-150">Run hello following Azure PowerShell cmdlet:</span></span>

       Get-AzurePublishSettingsFile

   <span data-ttu-id="3bae4-151">A böngésző használata toonavigate toohello közzététele beállítások letöltése oldalt.</span><span class="sxs-lookup"><span data-stu-id="3bae4-151">This will use your browser toonavigate toohello publish settings download page.</span></span> <span data-ttu-id="3bae4-152">Előfordulhat, hogy a kért toolog be egy Microsoft Account.</span><span class="sxs-lookup"><span data-stu-id="3bae4-152">You may be prompted toolog in with a Microsoft Account.</span></span> <span data-ttu-id="3bae4-153">Ha igen, használja az Azure-előfizetéshez társított hello fiók.</span><span class="sxs-lookup"><span data-stu-id="3bae4-153">If so, use hello account associated with your Azure subscription.</span></span>

   <span data-ttu-id="3bae4-154">Mentse a letöltött hello profil tooa fájl helye könnyen elérhetők.</span><span class="sxs-lookup"><span data-stu-id="3bae4-154">Save hello downloaded profile tooa file location you can easily access.</span></span>
2. <span data-ttu-id="3bae4-155">Futtassa a következő parancsmag tooimport hello közzétételi profil letöltött:</span><span class="sxs-lookup"><span data-stu-id="3bae4-155">Run following cmdlet tooimport hello publishing profile you downloaded:</span></span>

       Import-AzurePublishSettingsFile [path toofile]

    > [!NOTE]
    > <span data-ttu-id="3bae4-156">Hello importálása után közzétételi beállítások, javasoljuk, hogy hello törlése letöltött .publishSettings-fájlt, mert nem tartalmaz információt, amelyekkel mások tooaccess fiókját.</span><span class="sxs-lookup"><span data-stu-id="3bae4-156">After importing hello publish settings, consider deleting hello downloaded .publishSettings file, because it contains information that could allow someone tooaccess your account.</span></span>

### <a name="publish-hello-application"></a><span data-ttu-id="3bae4-157">Hello alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="3bae4-157">Publish hello application</span></span>
<span data-ttu-id="3bae4-158">Futtassa a következő parancsok hello toopublish:</span><span class="sxs-lookup"><span data-stu-id="3bae4-158">toopublish, run hello following commands:</span></span>

      $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

* <span data-ttu-id="3bae4-159">**Szolgáltatásnév -** hello telepítési hello nevét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="3bae4-159">**-ServiceName** specifies hello name for hello deployment.</span></span> <span data-ttu-id="3bae4-160">Ennek egyedi névnek kell lennie, ellenkező esetben hello közzétételi folyamat meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="3bae4-160">This must be a unique name, otherwise hello publish process will fail.</span></span> <span data-ttu-id="3bae4-161">Hello **Get-Date** parancs hozzátold egy dátum/idő karakterláncot, hogy hello neve egyedi.</span><span class="sxs-lookup"><span data-stu-id="3bae4-161">hello **Get-Date** command tacks on a date/time string that should make hello name unique.</span></span>
* <span data-ttu-id="3bae4-162">**-Hely** határozza meg az üzemeltetett hello alkalmazás hello datacenter.</span><span class="sxs-lookup"><span data-stu-id="3bae4-162">**-Location** specifies hello datacenter that hello application will be hosted in.</span></span> <span data-ttu-id="3bae4-163">az elérhető adatközpontok, hello használja listáját toosee **Get-AzureLocation** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="3bae4-163">toosee a list of available datacenters, use hello **Get-AzureLocation** cmdlet.</span></span>
* <span data-ttu-id="3bae4-164">**-Launch** egy böngészőablakban megnyitja és toohello üzemeltetett szolgáltatás lép a telepítés befejezése után.</span><span class="sxs-lookup"><span data-stu-id="3bae4-164">**-Launch** opens a browser window and navigates toohello hosted service after deployment has completed.</span></span>

<span data-ttu-id="3bae4-165">Miután a közzététel sikeresen megtörtént, a válasz hasonló toohello következő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="3bae4-165">After publishing succeeds, you will see a response similar toohello following:</span></span>

![hello hello Publish-AzureService parancs kimenete][hello output of hello Publish-AzureService command]

> [!NOTE]
> <span data-ttu-id="3bae4-167">Ez a hello alkalmazás toodeploy néhány percet igénybe vehet, és válnak elérhetővé, az első közzététel alkalmával.</span><span class="sxs-lookup"><span data-stu-id="3bae4-167">It can take several minutes for hello application toodeploy and become available when first published.</span></span>

<span data-ttu-id="3bae4-168">Hello központi telepítés befejezése után egy böngészőablakban nyissa meg, és keresse meg a toohello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3bae4-168">Once hello deployment has completed, a browser window will open and navigate toohello cloud service.</span></span>

![Hello hello world oldalt megjelenítő böngészőablak hello URL-cím jelzi hello lap az Azure-on.][A browser window displaying hello hello world page; hello URL indicates hello page is hosted on Azure.]

<span data-ttu-id="3bae4-170">Az alkalmazás most már az Azure-ban fut.</span><span class="sxs-lookup"><span data-stu-id="3bae4-170">Your application is now running on Azure.</span></span>

<span data-ttu-id="3bae4-171">Hello **Publish-AzureServiceProject** parancsmaggal hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3bae4-171">hello **Publish-AzureServiceProject** cmdlet performs hello following steps:</span></span>

1. <span data-ttu-id="3bae4-172">Létrehoz egy csomag toodeploy.</span><span class="sxs-lookup"><span data-stu-id="3bae4-172">Creates a package toodeploy.</span></span> <span data-ttu-id="3bae4-173">hello csomag tartalmazza az összes hello fájlt az alkalmazás mappájában.</span><span class="sxs-lookup"><span data-stu-id="3bae4-173">hello package contains all hello files in your application folder.</span></span>
2. <span data-ttu-id="3bae4-174">Létrehoz egy új **tárfiókot**, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="3bae4-174">Creates a new **storage account** if one does not exist.</span></span> <span data-ttu-id="3bae4-175">hello Azure storage-fiók üzembe helyezése során használt toostore hello alkalmazáscsomag.</span><span class="sxs-lookup"><span data-stu-id="3bae4-175">hello Azure storage account is used toostore hello application package during deployment.</span></span> <span data-ttu-id="3bae4-176">Hello storage-fiók a telepítés befejezése után nyugodtan törölheti.</span><span class="sxs-lookup"><span data-stu-id="3bae4-176">You can safely delete hello storage account after deployment is done.</span></span>
3. <span data-ttu-id="3bae4-177">Létrehoz egy új **felhőszolgáltatást**, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="3bae4-177">Creates a new **cloud service** if one does not already exist.</span></span> <span data-ttu-id="3bae4-178">A **felhőalapú szolgáltatás** hello tároló, amelyben az alkalmazás üzemel, ha a telepített tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3bae4-178">A **cloud service** is hello container in which your application is hosted when it is deployed tooAzure.</span></span> <span data-ttu-id="3bae4-179">További információkért lásd: [Üzemeltetett szolgáltatás létrehozása az Azure-ban – áttekintés].</span><span class="sxs-lookup"><span data-stu-id="3bae4-179">For more information, see [Overview of Creating a Hosted Service for Azure].</span></span>
4. <span data-ttu-id="3bae4-180">Hello központi telepítési csomag tooAzure közzéteszi.</span><span class="sxs-lookup"><span data-stu-id="3bae4-180">Publishes hello deployment package tooAzure.</span></span>

## <a name="stopping-and-deleting-your-application"></a><span data-ttu-id="3bae4-181">Az alkalmazás leállítása és törlése</span><span class="sxs-lookup"><span data-stu-id="3bae4-181">Stopping and deleting your application</span></span>
<span data-ttu-id="3bae4-182">Miután az alkalmazás telepítéséhez, érdemes lehet a toodisable azt további költségek elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="3bae4-182">After deploying your application, you may want toodisable it so you can avoid extra costs.</span></span> <span data-ttu-id="3bae4-183">Az Azure a webesszerepkör-példányok esetében óránként számol fel díjat a felhasznált kiszolgálóidő után.</span><span class="sxs-lookup"><span data-stu-id="3bae4-183">Azure bills web role instances per hour of server time consumed.</span></span> <span data-ttu-id="3bae4-184">Kiszolgálói felhasznált után az alkalmazás van telepítve, akkor is, ha hello példányok nem futnak, és hello leállt állapotban van.</span><span class="sxs-lookup"><span data-stu-id="3bae4-184">Server time is consumed once your application is deployed, even if hello instances are not running and are in hello stopped state.</span></span>

1. <span data-ttu-id="3bae4-185">Hello Windows PowerShell-ablakban állítsa le a hello szolgáltatás központi telepítése a következő parancsmag hello hello előző szakaszban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="3bae4-185">In hello Windows PowerShell window, stop hello service deployment created in hello previous section with hello following cmdlet:</span></span>

       Stop-AzureService

   <span data-ttu-id="3bae4-186">Hello szolgáltatás leállítása eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="3bae4-186">Stopping hello service may take several minutes.</span></span> <span data-ttu-id="3bae4-187">Hello szolgáltatás leáll, amikor megjelenik egy üzenet, amely azt jelzi, hogy leállt.</span><span class="sxs-lookup"><span data-stu-id="3bae4-187">When hello service is stopped, you receive a message indicating that it has stopped.</span></span>

   ![hello hello Stop-AzureService parancs állapota][hello status of hello Stop-AzureService command]
2. <span data-ttu-id="3bae4-189">toodelete hello szolgáltatást, a következő parancsmag hívás hello:</span><span class="sxs-lookup"><span data-stu-id="3bae4-189">toodelete hello service, call hello following cmdlet:</span></span>

       Remove-AzureService

   <span data-ttu-id="3bae4-190">Amikor a rendszer kéri, adja meg a **Y** toodelete hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="3bae4-190">When prompted, enter **Y** toodelete hello service.</span></span>

   <span data-ttu-id="3bae4-191">Hello szolgáltatás törlése eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="3bae4-191">Deleting hello service may take several minutes.</span></span> <span data-ttu-id="3bae4-192">Hello szolgáltatás törlése után megjelenik egy üzenet, amely azt jelzi, hogy törölve lett-e a hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="3bae4-192">After hello service has been deleted you receive a message indicating that hello service was deleted.</span></span>

   ![hello hello Remove-AzureService parancs állapota][hello status of hello Remove-AzureService command]

   > [!NOTE]
   > <span data-ttu-id="3bae4-194">Hello szolgáltatás törlése nem érinti hello szolgáltatás első közzétételekor létrehozott tárfiók hello és, továbbra is fizetnie kell a tárhelyet toobe.</span><span class="sxs-lookup"><span data-stu-id="3bae4-194">Deleting hello service does not delete hello storage account that was created when hello service was initially published, and you will continue toobe billed for storage used.</span></span> <span data-ttu-id="3bae4-195">Ha nincs más hello tárolót használ, érdemes lehet a toodelete azt.</span><span class="sxs-lookup"><span data-stu-id="3bae4-195">If nothing else is using hello storage, you may want toodelete it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3bae4-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3bae4-196">Next steps</span></span>
<span data-ttu-id="3bae4-197">További információkért lásd: hello [Node.js fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="3bae4-197">For more information, see hello [Node.js Developer Center].</span></span>

<!-- URL List -->

[Azure Websites, a Cloud Services és a virtuális gépek összevetése]: ../app-service-web/choose-web-site-cloud-service-vm.md
[egyszerűsített webalkalmazás használatát]: ../app-service-web/app-service-web-get-started-nodejs.md
[Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Azure SDK for .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[PowerShell összekapcsolása]: /powershell/azureps-cmdlets-docs#step-3-connect
[nodejs.org]: http://nodejs.org/
[Üzemeltetett szolgáltatás létrehozása az Azure-ban – áttekintés]: https://azure.microsoft.com/documentation/services/cloud-services/
[Node.js fejlesztői központ]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[hello result of hello New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[hello output of hello Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying hello Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[hello output of hello Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying hello hello world page; hello URL indicates hello page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[hello status of hello Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[hello status of hello Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
