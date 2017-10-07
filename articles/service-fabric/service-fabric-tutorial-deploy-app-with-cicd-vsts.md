---
title: "az Azure Service Fabric-alkalmazás az folyamatos integráció (Team Services) aaaDeploy |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset folyamatos integrációt és a Visual Studio Team Services használatával a Service Fabric-alkalmazás központi telepítését.  Alkalmazás tooa Service Fabric-fürt üzembe helyezése az Azure-ban."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi
ms.openlocfilehash: ba9a632b247b0f467e7b66fbe77b4ad54fb3d9ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-with-cicd-tooa-service-fabric-cluster"></a><span data-ttu-id="bf9bf-104">Telepítsen egy alkalmazást CI/CD tooa Service Fabric-fürt</span><span class="sxs-lookup"><span data-stu-id="bf9bf-104">Deploy an application with CI/CD tooa Service Fabric cluster</span></span>
<span data-ttu-id="bf9bf-105">Ez az oktatóanyag három azokat, és ismerteti, hogyan tooset folyamatos integrációt és a Visual Studio Team Services használata az Azure Service Fabric-alkalmazások központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-105">This tutorial is part three of a series and describes how tooset up continuous integration and deployment for an Azure Service Fabric application using Visual Studio Team Services.</span></span>  <span data-ttu-id="bf9bf-106">Egy meglévő Service Fabric-alkalmazás van szükség, a létrehozott hello alkalmazás [létre olyan .NET alkalmazás](service-fabric-tutorial-create-dotnet-app.md) példaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-106">An existing Service Fabric application is needed, hello application created in [Build a .NET application](service-fabric-tutorial-create-dotnet-app.md) is used as an example.</span></span>

<span data-ttu-id="bf9bf-107">A hello sorozat három része megismerheti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="bf9bf-107">In part three of hello series, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bf9bf-108">A verziókövetési tooyour hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bf9bf-108">Add source control tooyour project</span></span>
> * <span data-ttu-id="bf9bf-109">Hozzon létre egy build definition Team Services</span><span class="sxs-lookup"><span data-stu-id="bf9bf-109">Create a build definition in Team Services</span></span>
> * <span data-ttu-id="bf9bf-110">Hozzon létre egy kiadási definition Team Services</span><span class="sxs-lookup"><span data-stu-id="bf9bf-110">Create a release definition in Team Services</span></span>
> * <span data-ttu-id="bf9bf-111">Automatikus központi telepítése és alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="bf9bf-111">Automatically deploy and upgrade an application</span></span>

<span data-ttu-id="bf9bf-112">Az oktatóanyag adatsorozat elsajátíthatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="bf9bf-112">In this tutorial series you learn how to:</span></span>
> [!div class="checklist"]
> * [<span data-ttu-id="bf9bf-113">A .NET Service Fabric-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf9bf-113">Build a .NET Service Fabric application</span></span>](service-fabric-tutorial-create-dotnet-app.md)
> * [<span data-ttu-id="bf9bf-114">Hello alkalmazás tooa távoli fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="bf9bf-114">Deploy hello application tooa remote cluster</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * <span data-ttu-id="bf9bf-115">Konfigurálja a CI/CD Visual Studio Team Services használatával</span><span class="sxs-lookup"><span data-stu-id="bf9bf-115">Configure CI/CD using Visual Studio Team Services</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf9bf-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bf9bf-116">Prerequisites</span></span>
<span data-ttu-id="bf9bf-117">Ez az oktatóanyag elkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="bf9bf-117">Before you begin this tutorial:</span></span>
- <span data-ttu-id="bf9bf-118">Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="bf9bf-118">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="bf9bf-119">[Telepítse a Visual Studio 2017](https://www.visualstudio.com/) és hello telepítése **Azure fejlesztési** és **ASP.NET és a webes fejlesztési** munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-119">[Install Visual Studio 2017](https://www.visualstudio.com/) and install hello **Azure development** and **ASP.NET and web development** workloads.</span></span>
- [<span data-ttu-id="bf9bf-120">Hello Service Fabric SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="bf9bf-120">Install hello Service Fabric SDK</span></span>](service-fabric-get-started.md)
- <span data-ttu-id="bf9bf-121">Létrehozhat például egy Service Fabric-alkalmazás által [oktatóanyag](service-fabric-tutorial-create-dotnet-app.md).</span><span class="sxs-lookup"><span data-stu-id="bf9bf-121">Create a Service Fabric application, for example by [following this tutorial](service-fabric-tutorial-create-dotnet-app.md).</span></span> 
- <span data-ttu-id="bf9bf-122">Létrehozhat például egy Azure, a Windows Service Fabric-fürt által [oktatóanyag](service-fabric-tutorial-create-cluster-azure-ps.md)</span><span class="sxs-lookup"><span data-stu-id="bf9bf-122">Create a Windows Service Fabric cluster on Azure, for example by [following this tutorial](service-fabric-tutorial-create-cluster-azure-ps.md)</span></span>
- <span data-ttu-id="bf9bf-123">Hozzon létre egy [Team Services-fiók](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).</span><span class="sxs-lookup"><span data-stu-id="bf9bf-123">Create a [Team Services account](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).</span></span>

## <a name="download-hello-voting-sample-application"></a><span data-ttu-id="bf9bf-124">Hello Voting mintaalkalmazás letöltése</span><span class="sxs-lookup"><span data-stu-id="bf9bf-124">Download hello Voting sample application</span></span>
<span data-ttu-id="bf9bf-125">Ha Ön nem build hello Voting mintaalkalmazást [rész az oktatóanyag adatsorozat](service-fabric-tutorial-create-dotnet-app.md), tölthető le.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-125">If you did not build hello Voting sample application in [part one of this tutorial series](service-fabric-tutorial-create-dotnet-app.md), you can download it.</span></span> <span data-ttu-id="bf9bf-126">A parancs-ablakban futtassa a következő parancs tooclone hello sample app tárház tooyour helyi számítógép hello.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-126">In a command window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a><span data-ttu-id="bf9bf-127">Készítse elő a közzétételi profilt</span><span class="sxs-lookup"><span data-stu-id="bf9bf-127">Prepare a publish profile</span></span>
<span data-ttu-id="bf9bf-128">Most, hogy megismerte [az alkalmazások](service-fabric-tutorial-create-dotnet-app.md) és [hello alkalmazás tooAzure telepített](service-fabric-tutorial-deploy-app-to-party-cluster.md), készen áll a tooset folyamatos integrációt be most.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-128">Now that you've [created an application](service-fabric-tutorial-create-dotnet-app.md) and have [deployed hello application tooAzure](service-fabric-tutorial-deploy-app-to-party-cluster.md), you're ready tooset up continuous integration.</span></span>  <span data-ttu-id="bf9bf-129">Először készítsen számára az alkalmazáson belül közzétételi profilt, amely végrehajtja a Team Services belül hello központi telepítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-129">First, prepare a publish profile within your application for use by hello deployment process that executes within Team Services.</span></span>  <span data-ttu-id="bf9bf-130">a közzétételi profil hello konfigurált tootarget hello fürt, amely a korábban létrehozott kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-130">hello publish profile should be configured tootarget hello cluster that you've previously created.</span></span>  <span data-ttu-id="bf9bf-131">Indítsa el a Visual Studiót, és nyissa meg a meglévő Service Fabric-alkalmazás projekt.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-131">Start Visual Studio and open an existing Service Fabric application project.</span></span>  <span data-ttu-id="bf9bf-132">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello alkalmazást, és válassza ki **közzététel...** .</span><span class="sxs-lookup"><span data-stu-id="bf9bf-132">In **Solution Explorer**, right-click hello application and select **Publish...**.</span></span>

<span data-ttu-id="bf9bf-133">Válasszon egy cél-profilt, az alkalmazás projekt toouse a folyamatos integrációt munkafolyamat belül, például a felhő.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-133">Choose a target profile within your application project toouse for your continuous integration workflow, for example Cloud.</span></span>  <span data-ttu-id="bf9bf-134">Adja meg a hello fürt kapcsolat végpontja.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-134">Specify hello cluster connection endpoint.</span></span>  <span data-ttu-id="bf9bf-135">Ellenőrizze a hello **frissítési hello alkalmazás** jelölőnégyzetet, hogy az alkalmazás minden központi telepítést, Team Services frissíti.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-135">Check hello **Upgrade hello Application** checkbox so that your application upgrades for each deployment in Team Services.</span></span>  <span data-ttu-id="bf9bf-136">Hello kattintson **mentése** hivatkozás toosave hello beállítások toohello közzétételi profillal, és kattintson a **Mégse** tooclose hello párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-136">Click hello **Save** hyperlink toosave hello settings toohello publish profile and then click **Cancel** tooclose hello dialog box.</span></span>  

![Leküldéses profil][publish-app-profile]

## <a name="share-your-visual-studio-solution-tooa-new-team-services-git-repo"></a><span data-ttu-id="bf9bf-138">A Visual Studio megoldás tooa új Team Services Git-tárház megosztása</span><span class="sxs-lookup"><span data-stu-id="bf9bf-138">Share your Visual Studio solution tooa new Team Services Git repo</span></span>
<span data-ttu-id="bf9bf-139">Ossza meg az alkalmazás forrásfájljait tooa csapatprojekt Team Services, létrehozhat alkot.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-139">Share your application source files tooa team project in Team Services so you can generate builds.</span></span>  

<span data-ttu-id="bf9bf-140">Hozzon létre egy új helyi Git-tárház a projekthez kiválasztásával **tooSource vezérlő hozzáadása** -> **Git** hello állapotsoron a hello jobb alsó sarkában Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-140">Create a new local Git repo for your project by selecting **Add tooSource Control** -> **Git** on hello status bar in hello lower right-hand corner of Visual Studio.</span></span> 

<span data-ttu-id="bf9bf-141">A hello **leküldéses** megtekintése **Team Explorer**, jelölje be hello **Git-tárház közzététele** gombra kattint, a **tooVisual Studio Team Services leküldéses**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-141">In hello **Push** view in **Team Explorer**, select hello **Publish Git Repo** button under **Push tooVisual Studio Team Services**.</span></span>

![Leküldéses Git-tárház][push-git-repo]

<span data-ttu-id="bf9bf-143">Ellenőrizze az e-maileket, és válassza ki a fiókot hello **Team Services tartomány** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-143">Verify your email and select your account in hello **Team Services Domain** drop-down.</span></span> <span data-ttu-id="bf9bf-144">Adja meg a tárház nevét, és válassza ki **közzététel tárház**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-144">Enter your repository name and select **Publish repository**.</span></span>

![Leküldéses Git-tárház][publish-code]

<span data-ttu-id="bf9bf-146">Hello tárház közzétételi hoz létre egy új csapatprojektbe azonos nevet, amint hello helyi tárház hello fiókját.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-146">Publishing hello repo creates a new team project in your account with hello same name as hello local repo.</span></span> <span data-ttu-id="bf9bf-147">Kattintson egy meglévő csapatprojektben toocreate hello tárház **speciális** következő túl**tárház** nevet, és válassza ki a csapatprojekt.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-147">toocreate hello repo in an existing team project, click **Advanced** next too**Repository** name and select a team project.</span></span> <span data-ttu-id="bf9bf-148">Megtekintheti a kód hello Web kiválasztásával **megtekintéséhez hello webes**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-148">You can view your code on hello web by selecting **See it on hello web**.</span></span>

## <a name="configure-continuous-delivery-with-vsts"></a><span data-ttu-id="bf9bf-149">Folyamatos kézbesítési VSTS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bf9bf-149">Configure Continuous Delivery with VSTS</span></span>
<span data-ttu-id="bf9bf-150">Team Services build definícióját egy munkafolyamatot, amelyik build ismertetett lépések egymás után végrehajtott áll ismerteti.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-150">A Team Services build definition describes a workflow that is composed of a set of build steps that are executed sequentially.</span></span> <span data-ttu-id="bf9bf-151">A build definíció létrehozása, hogy a Service Fabric alkalmazáscsomag és más összetevők, toodeploy tooa Service Fabric-fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-151">Create a build definition that that produces a Service Fabric application package, and other artifacts, toodeploy tooa Service Fabric cluster.</span></span> <span data-ttu-id="bf9bf-152">További információ [Team Services build definíciók](https://www.visualstudio.com/docs/build/define/create).</span><span class="sxs-lookup"><span data-stu-id="bf9bf-152">Learn more about [Team Services build definitions](https://www.visualstudio.com/docs/build/define/create).</span></span> 

<span data-ttu-id="bf9bf-153">Team Services kiadás definícióját egy munkafolyamatot, amely az alkalmazás csomag tooa fürtöt telepít ismerteti.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-153">A Team Services release definition describes a workflow that deploys an application package tooa cluster.</span></span> <span data-ttu-id="bf9bf-154">Együttes használatuk esetén a hello definition készítse el és kiadás definition hello teljes munkafolyamat forrás fájlok tooending a fürtön futó alkalmazással kezdve végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-154">When used together, hello build definition and release definition execute hello entire workflow starting with source files tooending with a running application in your cluster.</span></span> <span data-ttu-id="bf9bf-155">További tudnivalók Team Services [definíciók kiadási](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).</span><span class="sxs-lookup"><span data-stu-id="bf9bf-155">Learn more about Team Services [release definitions](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).</span></span>

### <a name="create-a-build-definition"></a><span data-ttu-id="bf9bf-156">A build definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf9bf-156">Create a build definition</span></span>
<span data-ttu-id="bf9bf-157">Nyisson meg egy webböngészőt, és keresse meg az új csapatprojektbe tooyour:: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-157">Open a web browser and navigate tooyour new team project at: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting .</span></span> 

<span data-ttu-id="bf9bf-158">SELECT hello **Build & kiadási** lapra, majd **buildek**, majd **+ új definition**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-158">Select hello **Build & Release** tab, then **Builds**, then **+ New definition**.</span></span>  <span data-ttu-id="bf9bf-159">A **válasszon olyan sablont,**, jelölje be hello **Azure Service Fabric-alkalmazás** sablont, és kattintson **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-159">In **Select a template**, select hello **Azure Service Fabric Application** template and click **Apply**.</span></span> 

![Build sablon kiválasztása][select-build-template] 

<span data-ttu-id="bf9bf-161">szavazás alkalmazás hello .NET Core projektet tartalmaz, ezért adja hozzá egy feladatot, amely visszaállítja az hello függőségek.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-161">hello voting application contains a .NET Core project, so add a task that restores hello dependencies.</span></span> <span data-ttu-id="bf9bf-162">A hello **feladatok** nézetben jelölje ki **+ Hozzáadás feladat** a hello bal alsó.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-162">In hello **Tasks** view, select **+ Add Task** in hello bottom left.</span></span> <span data-ttu-id="bf9bf-163">Keresse a "Parancssori" toofind hello parancssori feladatot, majd kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-163">Search on "Command Line" toofind hello command-line task, then click **Add**.</span></span> 

![Feladat hozzáadása][add-task] 

<span data-ttu-id="bf9bf-165">Hello új feladat esetén adja meg "Dotnet.exe Futtatás" **megjelenített név**, "dotnet.exe" **eszköz**, és a "visszaállítás" a **argumentumok**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-165">In hello new task, enter "Run dotnet.exe" in **Display name**, "dotnet.exe" in **Tool**, and "restore" in **Arguments**.</span></span> 

![Új feladat][new-task] 

<span data-ttu-id="bf9bf-167">A hello **eseményindítók** megtekintéséhez kattintson a hello **engedélyezése ehhez az eseményindítóhoz** alatt kapcsoló **folyamatos integrációt**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-167">In hello **Triggers** view, click hello **Enable this trigger** switch under **Continuous Integration**.</span></span> 

<span data-ttu-id="bf9bf-168">Válassza ki **mentés és a feldolgozási sor** , és írja be a "Kihelyezett VS2017" hello, **ügynök várólista**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-168">Select **Save & queue** and enter "Hosted VS2017" as hello **Agent queue**.</span></span> <span data-ttu-id="bf9bf-169">Válassza ki **várólista** toomanually build elindításához.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-169">Select **Queue** toomanually start a build.</span></span>  <span data-ttu-id="bf9bf-170">Épít is eseményindítók leküldéses vagy be.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-170">Builds also triggers upon push or check-in.</span></span>

<span data-ttu-id="bf9bf-171">toocheck a build folyamatban, a kapcsoló toohello **buildek** fülre.  Miután meggyőződött hello build sikeresen végrehajtja, adja meg egy kiadási-definíciót, amely telepíti az alkalmazást tooa fürt.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-171">toocheck your build progress, switch toohello **Builds** tab.  Once you verify that hello build executes successfully, define a release definition that deploys your application tooa cluster.</span></span> 

### <a name="create-a-release-definition"></a><span data-ttu-id="bf9bf-172">Egy kiadási definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf9bf-172">Create a release definition</span></span>  

<span data-ttu-id="bf9bf-173">Jelölje be hello **Build & kiadási** lapra, majd **kiadásokban**, majd **+ új definition**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-173">Select hello **Build & Release** tab, then **Releases**, then **+ New definition**.</span></span>  <span data-ttu-id="bf9bf-174">A **létrehozás kiadás definition**, jelölje be hello **Azure Service Fabric telepítési** hello listán, és kattintson a sablon **következő**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-174">In **Create release definition**, select hello **Azure Service Fabric Deployment** template from hello list and click **Next**.</span></span>  <span data-ttu-id="bf9bf-175">Jelölje be hello **Build** forrás-, ellenőrizze a hello **folyamatos üzembe helyezés** mezőbe, majd kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-175">Select hello **Build** source, check hello **Continuous deployment** box, and click **Create**.</span></span> 

<span data-ttu-id="bf9bf-176">A hello **környezetek** megtekintéséhez kattintson az **Hozzáadás** jobbra toohello **Fürtkapcsolat**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-176">In hello **Environments** view, click **Add** toohello right of **Cluster Connection**.</span></span>  <span data-ttu-id="bf9bf-177">Adja meg a kapcsolat neve a "mysftestcluster", "tcp://mysftestcluster.westus.cloudapp.azure.com:19000", és a hello Azure Active Directory vagy a tanúsítvány hitelesítő adatok a fürt végpontja hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-177">Specify a connection name of "mysftestcluster", a cluster endpoint of "tcp://mysftestcluster.westus.cloudapp.azure.com:19000", and hello Azure Active Directory or certificate credentials for hello cluster.</span></span> <span data-ttu-id="bf9bf-178">Az Azure Active Directory hitelesítő adatok, adja meg a hello hitelesítő adatok toouse tooconnect toohello fürt a hello **felhasználónév** és **jelszó** mezőket.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-178">For Azure Active Directory credentials, define hello credentials you want toouse tooconnect toohello cluster in hello **Username** and **Password** fields.</span></span> <span data-ttu-id="bf9bf-179">A tanúsítványalapú hitelesítéshez, adja meg a hello Base64 kódolás hello ügyfél tanúsítványfájl hello **ügyféltanúsítvány** mező.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-179">For certificate-based authentication, define hello Base64 encoding of hello client certificate file in hello **Client Certificate** field.</span></span>  <span data-ttu-id="bf9bf-180">Hello súgó előugró ablak megjelenik, hogy a mező kapcsolatos információk, amelyek érték tooget.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-180">See hello help pop-up on that field for info on how tooget that value.</span></span>  <span data-ttu-id="bf9bf-181">Ha a tanúsítvány jelszóval védett, meghatározása hello jelszó hello **jelszó** mező.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-181">If your certificate is password-protected, define hello password in hello **Password** field.</span></span>  <span data-ttu-id="bf9bf-182">Kattintson a **mentése** toosave hello kiadás definíciója.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-182">Click **Save** toosave hello release definition.</span></span>

![Fürt-kapcsolat hozzáadása][add-cluster-connection] 

<span data-ttu-id="bf9bf-184">Kattintson a **ügynökön**, majd jelölje be **üzemeltetett VS2017** a **telepítési várólista**.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-184">Click **Run on agent**, then select **Hosted VS2017** for **Deployment queue**.</span></span> <span data-ttu-id="bf9bf-185">Kattintson a **mentése** toosave hello kiadás definíciója.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-185">Click **Save** toosave hello release definition.</span></span>

![Ügynökön][run-on-agent]

<span data-ttu-id="bf9bf-187">Válassza ki **+ kiadási** -> **létrehozása kiadási** -> **létrehozása** toomanually verziót létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-187">Select **+Release** -> **Create Release** -> **Create** toomanually create a release.</span></span>  <span data-ttu-id="bf9bf-188">Ellenőrizze, hogy hello központi telepítés sikeres volt, és hello alkalmazás hello fürtben fut.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-188">Verify that hello deployment succeeded and hello application is running in hello cluster.</span></span>  <span data-ttu-id="bf9bf-189">Nyisson meg egy webböngészőt, és keresse meg a túl[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span><span class="sxs-lookup"><span data-stu-id="bf9bf-189">Open a web browser and navigate too[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span></span>  <span data-ttu-id="bf9bf-190">Vegye figyelembe a hello Alkalmazásverzió, ebben a példában "1.0.0.20170616.3".</span><span class="sxs-lookup"><span data-stu-id="bf9bf-190">Note hello application version, in this example it is "1.0.0.20170616.3".</span></span> 

## <a name="commit-and-push-changes-trigger-a-release"></a><span data-ttu-id="bf9bf-191">Véglegesítse és módosításokat, kiadási indítás</span><span class="sxs-lookup"><span data-stu-id="bf9bf-191">Commit and push changes, trigger a release</span></span>
<span data-ttu-id="bf9bf-192">tooverify, amelyek folyamatos integrációt csővezeték hello tooTeam szolgáltatások kód változást ellenőrzésével működik.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-192">tooverify that hello continuous integration pipeline is functioning by checking in some code changes tooTeam Services.</span></span>    

<span data-ttu-id="bf9bf-193">Amikor tartalmat ír a kódot, a automatikusan kötetblokkok változásait a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-193">As you write your code, your changes are automatically tracked by Visual Studio.</span></span> <span data-ttu-id="bf9bf-194">Véglegesítse a módosításokat tooyour helyi Git-tárház függőben lévő módosítások ikonra (hello kiválasztásával</span><span class="sxs-lookup"><span data-stu-id="bf9bf-194">Commit changes tooyour local Git repository by selecting hello pending changes icon (</span></span>![Függőben][pending]<span data-ttu-id="bf9bf-196">) a hello állapotsor a hello jobb alsó sarok.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-196">) from hello status bar in hello bottom right.</span></span>

<span data-ttu-id="bf9bf-197">A hello **módosítások** Team Intézőben, vegye fel a frissítést leíró üzenet és a változtatások véglegesítése a határidő.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-197">On hello **Changes** view in Team Explorer, add a message describing your update and commit your changes.</span></span>

![Az összes véglegesítése][changes]

<span data-ttu-id="bf9bf-199">Jelölje be hello közzé nem tett változás állapotikon sávjának (![közzé nem tett módosítások][unpublished-changes]) vagy a szinkronizálási nézet Team Explorer hello.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-199">Select hello unpublished changes status bar icon (![Unpublished changes][unpublished-changes]) or hello Sync view in Team Explorer.</span></span> <span data-ttu-id="bf9bf-200">Válassza ki **leküldéses** tooupdate a kódot a Team Services/TFS.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-200">Select **Push** tooupdate your code in Team Services/TFS.</span></span>

![Küldje el a módosításokat][push]

<span data-ttu-id="bf9bf-202">Hello módosítások tooTeam küldését szolgáltatások automatikusan eseményindítók build.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-202">Pushing hello changes tooTeam Services automatically triggers a build.</span></span>  <span data-ttu-id="bf9bf-203">Ha hello build definition sikeresen befejeződött, a kiadási automatikusan létrejön, és elindítja a hello alkalmazás hello fürt frissítése.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-203">When hello build definition successfully completes, a release is automatically created and starts upgrading hello application on hello cluster.</span></span>

<span data-ttu-id="bf9bf-204">toocheck a build folyamatban, a kapcsoló toohello **buildek** lapján **Team Explorer** a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-204">toocheck your build progress, switch toohello **Builds** tab in **Team Explorer** in Visual Studio.</span></span>  <span data-ttu-id="bf9bf-205">Miután meggyőződött hello build sikeresen végrehajtja, adja meg egy kiadási-definíciót, amely telepíti az alkalmazást tooa fürt.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-205">Once you verify that hello build executes successfully, define a release definition that deploys your application tooa cluster.</span></span>

<span data-ttu-id="bf9bf-206">Ellenőrizze, hogy hello központi telepítés sikeres volt, és hello alkalmazás hello fürtben fut.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-206">Verify that hello deployment succeeded and hello application is running in hello cluster.</span></span>  <span data-ttu-id="bf9bf-207">Nyisson meg egy webböngészőt, és keresse meg a túl[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span><span class="sxs-lookup"><span data-stu-id="bf9bf-207">Open a web browser and navigate too[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span></span>  <span data-ttu-id="bf9bf-208">Vegye figyelembe a hello Alkalmazásverzió, ebben a példában "1.0.0.20170815.3".</span><span class="sxs-lookup"><span data-stu-id="bf9bf-208">Note hello application version, in this example it is "1.0.0.20170815.3".</span></span>

![Service Fabric Explorer][sfx1]

## <a name="update-hello-application"></a><span data-ttu-id="bf9bf-210">Hello alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="bf9bf-210">Update hello application</span></span>
<span data-ttu-id="bf9bf-211">Kód módosításokat hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-211">Make code changes in hello application.</span></span>  <span data-ttu-id="bf9bf-212">Mentse és hello módosítások véglegesítéséhez, hello előző lépések.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-212">Save and commit hello changes, following hello previous steps.</span></span>

<span data-ttu-id="bf9bf-213">Miután hello frissítése hello alkalmazás kezd, hello a frissítési folyamat állapotát, a Service Fabric Explorer figyelemmel követheti:</span><span class="sxs-lookup"><span data-stu-id="bf9bf-213">Once hello upgrade of hello application begins, you can watch hello upgrade progress in Service Fabric Explorer:</span></span>

![Service Fabric Explorer][sfx2]

<span data-ttu-id="bf9bf-215">Az alkalmazásfrissítés hello több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-215">hello application upgrade may take several minutes.</span></span> <span data-ttu-id="bf9bf-216">Hello frissítés befejeződése után hello alkalmazást futtatni fogja hello következő verziójára.</span><span class="sxs-lookup"><span data-stu-id="bf9bf-216">When hello upgrade is complete, hello application will be running hello next version.</span></span>  <span data-ttu-id="bf9bf-217">Ebben a példában "1.0.0.20170815.4".</span><span class="sxs-lookup"><span data-stu-id="bf9bf-217">In this example "1.0.0.20170815.4".</span></span>

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a><span data-ttu-id="bf9bf-219">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf9bf-219">Next steps</span></span>
<span data-ttu-id="bf9bf-220">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="bf9bf-220">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bf9bf-221">A verziókövetési tooyour hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bf9bf-221">Add source control tooyour project</span></span>
> * <span data-ttu-id="bf9bf-222">A build definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf9bf-222">Create a build definition</span></span>
> * <span data-ttu-id="bf9bf-223">Egy kiadási definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf9bf-223">Create a release definition</span></span>
> * <span data-ttu-id="bf9bf-224">Automatikus központi telepítése és alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="bf9bf-224">Automatically deploy and upgrade an application</span></span>

<span data-ttu-id="bf9bf-225">Most, hogy egy alkalmazást telepített és konfigurált folyamatos integrációt, próbálkozzon a hello következő:</span><span class="sxs-lookup"><span data-stu-id="bf9bf-225">Now that you have deployed an application and configured continuous integration, try hello following:</span></span>
- [<span data-ttu-id="bf9bf-226">Alkalmazások frissítése</span><span class="sxs-lookup"><span data-stu-id="bf9bf-226">Upgrade an app</span></span>](service-fabric-application-upgrade.md)
- [<span data-ttu-id="bf9bf-227">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="bf9bf-227">Test an app</span></span>](service-fabric-testability-overview.md) 
- [<span data-ttu-id="bf9bf-228">Figyelése és diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="bf9bf-228">Monitor and diagnose</span></span>](service-fabric-diagnostics-overview.md)


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[add-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddTask.png
[new-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewTask.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
[run-on-agent]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/RunOnAgent.png