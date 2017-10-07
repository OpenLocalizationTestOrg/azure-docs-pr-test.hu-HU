---
title: "aaaCI/CD-KET a virtuális gépek Team Services Jenkins tooAzure |} Microsoft Docs"
description: "Folyamatos integrációt (CI) és a folyamatos üzembe helyezés (CD) Jenkins tooAzure virtuális gépek a Kiadáskezelés a Visual Studio Team Services (VSTS) vagy a Microsoft Team Foundation Server (TFS) használata Node.js-alkalmazás beállítása"
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/15/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: 400ae34cbdf45da65351811c0ff6ff5d61ef862c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-toolinux-vms-using-jenkins-and-team-services"></a><span data-ttu-id="65c0e-103">Az alkalmazás tooLinux virtuális gépek telepítése Jenkins és Team Services használatával</span><span class="sxs-lookup"><span data-stu-id="65c0e-103">Deploy your app tooLinux VMs using Jenkins and Team Services</span></span>

<span data-ttu-id="65c0e-104">Folyamatos integrációt (CI) és a folyamatos üzembe helyezés (CD) nem egy folyamatot, amely létre, a kiadási és a kód telepítésére.</span><span class="sxs-lookup"><span data-stu-id="65c0e-104">Continuous integration (CI) and continuous deployment (CD) is a pipeline by which you can build, release, and deploy your code.</span></span> <span data-ttu-id="65c0e-105">Team Services biztosít egy teljes, teljes funkcionalitású CI/CD automatizálási eszközeivel telepítési tooAzure.</span><span class="sxs-lookup"><span data-stu-id="65c0e-105">Team Services provides a complete, fully featured set of CI/CD automation tools for deployment tooAzure.</span></span> <span data-ttu-id="65c0e-106">Jenkins egy olyan népszerű 3. fél CI/CD server-alapú eszköz, amely CI/CD automation is biztosít.</span><span class="sxs-lookup"><span data-stu-id="65c0e-106">Jenkins is a popular 3rd-party CI/CD server-based tool that also provides CI/CD automation.</span></span> <span data-ttu-id="65c0e-107">Mindkét együtt toocustomize is használhatja a felhőalapú alkalmazás vagy szolgáltatás biztosításához hogyan.</span><span class="sxs-lookup"><span data-stu-id="65c0e-107">You can use both together toocustomize how you deliver your cloud app or service.</span></span>

<span data-ttu-id="65c0e-108">Ebben az oktatóanyagban Jenkins toobuild használhat egy **Node.js webalkalmazás**, és a Visual Studio Team Services toodeploy azt tooa [üzembe helyezési csoport](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) Linux virtuális gépeket tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="65c0e-108">In this tutorial, you use Jenkins toobuild a **Node.js web app**, and Visual Studio Team Services toodeploy it tooa [deployment group](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) containing Linux virtual machines.</span></span>

<span data-ttu-id="65c0e-109">Az alábbiakat fogja elvégezni:</span><span class="sxs-lookup"><span data-stu-id="65c0e-109">You will:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="65c0e-110">Az alkalmazás a Jenkins elkészítésére</span><span class="sxs-lookup"><span data-stu-id="65c0e-110">Build your app in Jenkins</span></span>
> * <span data-ttu-id="65c0e-111">Jenkins Team Services-integráció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65c0e-111">Configure Jenkins for Team Services integration</span></span>
> * <span data-ttu-id="65c0e-112">Egy központi telepítési csoport hello Azure virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="65c0e-112">Create a deployment group for hello Azure virtual machines</span></span>
> * <span data-ttu-id="65c0e-113">Konfigurálja a hello virtuális gépek és hello alkalmazást telepíti a kiadási-definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="65c0e-113">Create a release definition that configures hello VMs and deploys hello app</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="65c0e-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="65c0e-114">Before you begin</span></span>

* <span data-ttu-id="65c0e-115">Hozzáférés tooa Jenkins fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="65c0e-115">You need access tooa Jenkins account.</span></span> <span data-ttu-id="65c0e-116">Ha még nem hozott létre egy Jenkins server, lásd: [Jenkins dokumentáció](https://jenkins.io/doc/).</span><span class="sxs-lookup"><span data-stu-id="65c0e-116">If you have not yet created a Jenkins server, see [Jenkins Documentation](https://jenkins.io/doc/).</span></span> 

* <span data-ttu-id="65c0e-117">Jelentkezzen be tooyour Team Services-fiók (`https://{youraccount}.visualstudio.com`).</span><span class="sxs-lookup"><span data-stu-id="65c0e-117">Sign in tooyour Team Services account (`https://{youraccount}.visualstudio.com`).</span></span> 
  <span data-ttu-id="65c0e-118">Beszerezheti a [ingyenes Team Services-fiókot](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).</span><span class="sxs-lookup"><span data-stu-id="65c0e-118">You can get a [free Team Services account](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).</span></span>

  > [!NOTE]
  > <span data-ttu-id="65c0e-119">További információkért lásd: [Connect tooTeam szolgáltatások](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).</span><span class="sxs-lookup"><span data-stu-id="65c0e-119">For more information, see [Connect tooTeam Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).</span></span>

* <span data-ttu-id="65c0e-120">Ha még nem rendelkezik egy, hozzon létre egy személyes hozzáférési jogkivonat (PAT) Team Services-fiókját.</span><span class="sxs-lookup"><span data-stu-id="65c0e-120">Create a personal access token (PAT) in your Team Services account if you don't already have one.</span></span> <span data-ttu-id="65c0e-121">Jenkins igényel az információk tooaccess Team Services-fiókját.</span><span class="sxs-lookup"><span data-stu-id="65c0e-121">Jenkins requires this information tooaccess your Team Services account.</span></span>
  <span data-ttu-id="65c0e-122">Olvasási [hogyan hozható létre személyes hozzáférési jogkivonat Team Services és a TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) toolearn hogyan toogenerate egyet.</span><span class="sxs-lookup"><span data-stu-id="65c0e-122">Read [How do I create a personal access token for Team Services and TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) toolearn how toogenerate one.</span></span>

## <a name="get-hello-sample-app"></a><span data-ttu-id="65c0e-123">Hello minta alkalmazás letöltése</span><span class="sxs-lookup"><span data-stu-id="65c0e-123">Get hello sample app</span></span>

<span data-ttu-id="65c0e-124">Egy alkalmazás toodeploy egy Git-tárház tárolva van szüksége.</span><span class="sxs-lookup"><span data-stu-id="65c0e-124">You need an app toodeploy stored in a Git repository.</span></span>
<span data-ttu-id="65c0e-125">A jelen oktatóanyag esetében javasoljuk, használjon [a mintaalkalmazás elérhető a Githubon](https://github.com/azooinmyluggage/fabrikam-node).</span><span class="sxs-lookup"><span data-stu-id="65c0e-125">For this tutorial, we recommend you use [this sample app available from GitHub](https://github.com/azooinmyluggage/fabrikam-node).</span></span>

1. <span data-ttu-id="65c0e-126">Hozzon létre egy elágazás az alkalmazás, és jegyezze fel a hello helye (URL) számára ez az oktatóanyag későbbi lépéseiben.</span><span class="sxs-lookup"><span data-stu-id="65c0e-126">Create a fork of this app and take note of hello location (URL) for use in later steps of this tutorial.</span></span>

1. <span data-ttu-id="65c0e-127">Ellenőrizze hello elágazás **nyilvános** toosimplify tooGitHub később csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="65c0e-127">Make hello fork **public** toosimplify connecting tooGitHub later.</span></span>

> [!NOTE]
> <span data-ttu-id="65c0e-128">További információkért lásd: [oszthatja ketté a tárházban](https://help.github.com/articles/fork-a-repo/) és [nyilvános és titkos tárház](https://help.github.com/articles/making-a-private-repository-public/).</span><span class="sxs-lookup"><span data-stu-id="65c0e-128">For more information, see [Fork a repo](https://help.github.com/articles/fork-a-repo/) and [Making a private repository public](https://help.github.com/articles/making-a-private-repository-public/).</span></span>

> [!NOTE]
> <span data-ttu-id="65c0e-129">hello alkalmazás használatával lett létrehozva [Yeoman](http://yeoman.io/learning/index.html); használ **Express**, **bower**, és **grunt**; és van-e néhány **npm**szerint függőségeinek csomagok.</span><span class="sxs-lookup"><span data-stu-id="65c0e-129">hello app was built using [Yeoman](http://yeoman.io/learning/index.html); it uses **Express**, **bower**, and **grunt**; and it has some **npm** packages as dependencies.</span></span>
> <span data-ttu-id="65c0e-130">hello mintaalkalmazás tartalmaz [Azure Resource Manager-sablonok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) , vannak használt toodynamically hozzon létre hello virtuális gépek telepítése az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="65c0e-130">hello sample app contains a set of [Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) that are used toodynamically create hello virtual machines for deployment on Azure.</span></span> <span data-ttu-id="65c0e-131">Ezek a sablonok hello feladatai által használt [Team Services kiadási definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).</span><span class="sxs-lookup"><span data-stu-id="65c0e-131">These templates are used by tasks in hello [Team Services release definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).</span></span>
> <span data-ttu-id="65c0e-132">hello fő sablont hoz létre a hálózati biztonsági csoport, a virtuális gépek és virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="65c0e-132">hello main template creates a network security group, a virtual machine, and a virtual network.</span></span> <span data-ttu-id="65c0e-133">A nyilvános IP-címet rendel, és megnyílik a 80-as portot a bejövő.</span><span class="sxs-lookup"><span data-stu-id="65c0e-133">It assigns a public IP address and opens inbound port 80.</span></span> <span data-ttu-id="65c0e-134">Hozzáadja a hello telepítési csoport túl válassza hello gépek tooreceive hello központi telepítése által használt címke is.</span><span class="sxs-lookup"><span data-stu-id="65c0e-134">It also adds a tag that is used by hello deployment group too select hello machines tooreceive hello deployment.</span></span>
>
> <span data-ttu-id="65c0e-135">hello minta is tartalmaz egy parancsfájlt, amely beállítja a Nginx hello alkalmazást telepíti.</span><span class="sxs-lookup"><span data-stu-id="65c0e-135">hello sample also contains a script that sets up Nginx and deploys hello app.</span></span> <span data-ttu-id="65c0e-136">Az egyes virtuális gépek hello végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="65c0e-136">It is executed on each of hello virtual machines.</span></span> <span data-ttu-id="65c0e-137">Pontosabban telepíti az hello parancsfájl a csomópont, Nginx és PM2; Konfigurálja az Nginx és PM2; Ezután elindítja a hello csomópont alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="65c0e-137">Specifically, hello script installs Node, Nginx, and PM2; configures Nginx and PM2; then starts hello Node app.</span></span>

## <a name="configure-jenkins-plugins"></a><span data-ttu-id="65c0e-138">Jenkins beépülő modulok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65c0e-138">Configure Jenkins plugins</span></span>

<span data-ttu-id="65c0e-139">Először konfigurálnia kell a két Jenkins beépülők **NodeJS** és **integráció a Team Services**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-139">First, you must configure two Jenkins plugins for **NodeJS** and **Integration with Team Services**.</span></span>

1. <span data-ttu-id="65c0e-140">Nyissa meg a Jenkins fiókját, és válassza a **kezelése Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-140">Open your Jenkins account and choose **Manage Jenkins**.</span></span>

1. <span data-ttu-id="65c0e-141">A hello **kezelése Jenkins** lapon, válassza ki **kezelése beépülő modulok**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-141">In hello **Manage Jenkins** page, choose **Manage Plugins**.</span></span>

1. <span data-ttu-id="65c0e-142">Szűrő hello lista toolocate hello **NodeJS** beépülő modul és a restart záradék nélkül telepítheti.</span><span class="sxs-lookup"><span data-stu-id="65c0e-142">Filter hello list toolocate hello **NodeJS** plugin and install it without restart.</span></span>

   ![Hello NodeJS beépülő modul tooJenkins hozzáadása](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. <span data-ttu-id="65c0e-144">Szűrő hello lista toofind hello **a Team Foundation Server** beépülő modul és telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="65c0e-144">Filter hello list toofind hello **Team Foundation Server** plugin and install it.</span></span> <span data-ttu-id="65c0e-145">(Ez a Team Services és a Team Foundation Server a beépülő modul működik.) Jenkins újraindítása nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="65c0e-145">(This plug-in works for both Team Services and Team Foundation Server.) Restarting Jenkins is not necessary.</span></span>

## <a name="configure-jenkins-build-for-nodejs"></a><span data-ttu-id="65c0e-146">A Node.js Jenkins build konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65c0e-146">Configure Jenkins build for Node.js</span></span>

<span data-ttu-id="65c0e-147">A Jenkins hozzon létre egy új build projektet, és konfigurálja az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="65c0e-147">In Jenkins, create a new build project and configure it as follows:</span></span>

1. <span data-ttu-id="65c0e-148">A hello **általános** lapra, adja meg a build projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="65c0e-148">In hello **General** tab, enter a name for your build project.</span></span>

1. <span data-ttu-id="65c0e-149">A hello **forrás kód felügyeleti** lapon jelölje be **Git** , és írja be a hello tárház és az alkalmazás kódját tartalmazó hello fiókirodai hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="65c0e-149">In hello **Source Code Management** tab, select **Git** and enter hello details of hello repository and hello branch containing your app code.</span></span>

   ![A tárház tooyour build hozzáadása](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > <span data-ttu-id="65c0e-151">Ha a tárház nem nyilvános, válasszon **Hozzáadás** , és adja meg a hitelesítő adatok tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="65c0e-151">If your repository is not public, choose **Add** and provide credentials tooconnect tooit.</span></span>

1. <span data-ttu-id="65c0e-152">A hello **Build eseményindítók** lapon jelölje be **lekérdezési SCM** , és írja be a hello ütemezés `H/03 * * * *` toopoll hello Git-tárház változásainak három percenként.</span><span class="sxs-lookup"><span data-stu-id="65c0e-152">In hello **Build Triggers** tab, select **Poll SCM** and enter hello schedule `H/03 * * * *` toopoll hello Git repository for changes every three minutes.</span></span> 

1. <span data-ttu-id="65c0e-153">A hello **környezet létrehozása** lapon jelölje be **meg csomópont &amp; npm bin / mappa elérési ÚTJA** , és írja be `NodeJS` hello csomópont JS telepítés érték a.</span><span class="sxs-lookup"><span data-stu-id="65c0e-153">In hello **Build Environment** tab, select **Provide Node &amp; npm bin/ folder PATH** and enter `NodeJS` for hello Node JS Installation value.</span></span> <span data-ttu-id="65c0e-154">Hagyja **npmrc fájl** "használja a rendszer alapértelmezett."</span><span class="sxs-lookup"><span data-stu-id="65c0e-154">Leave **npmrc file** set to "use system default."</span></span>

1. <span data-ttu-id="65c0e-155">A hello **Build** lapra, adja meg a hello parancs `npm install` tooensure összes függőségét frissülnek.</span><span class="sxs-lookup"><span data-stu-id="65c0e-155">In hello **Build** tab, enter hello command `npm install` tooensure all dependencies are updated.</span></span>

## <a name="configure-jenkins-for-team-services-integration"></a><span data-ttu-id="65c0e-156">Jenkins Team Services-integráció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65c0e-156">Configure Jenkins for Team Services integration</span></span>

1. <span data-ttu-id="65c0e-157">A hello **utáni műveletek** lapon, a **fájlok tooarchive**, adja meg `**/*` tooinclude összes fájlt.</span><span class="sxs-lookup"><span data-stu-id="65c0e-157">In hello **Post-build Actions** tab, for **Files tooarchive**, enter `**/*` tooinclude all files.</span></span>

1. <span data-ttu-id="65c0e-158">A **indul el, a TFS/Team Services kiadás**, hello teljes fiókja URL-CÍMÉT adja meg (például `https://your-account-name.visualstudio.com`), hello projekt nevét, egy nevet a hello kiadás definition (később létre), és a hitelesítő adatok tooconnect tooyour fiók hello.</span><span class="sxs-lookup"><span data-stu-id="65c0e-158">For **Trigger release in TFS/Team Services**, enter hello full URL of your account (such as `https://your-account-name.visualstudio.com`), hello project name, a name for hello release definition (created later), and hello credentials tooconnect tooyour account.</span></span>
   <span data-ttu-id="65c0e-159">A felhasználónév és a korábban létrehozott PAT hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="65c0e-159">You need your user name and hello PAT you created earlier.</span></span> 

   ![Jenkins utáni műveletek konfigurálása](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. <span data-ttu-id="65c0e-161">Hello build projekt mentése.</span><span class="sxs-lookup"><span data-stu-id="65c0e-161">Save hello build project.</span></span>

## <a name="create-a-jenkins-service-endpoint"></a><span data-ttu-id="65c0e-162">Hozzon létre egy Jenkins szolgáltatási végpont</span><span class="sxs-lookup"><span data-stu-id="65c0e-162">Create a Jenkins service endpoint</span></span>

<span data-ttu-id="65c0e-163">A szolgáltatási végpont lehetővé teszi, hogy a Team Services tooconnect tooJenkins.</span><span class="sxs-lookup"><span data-stu-id="65c0e-163">A service endpoint allows Team Services tooconnect tooJenkins.</span></span>

1. <span data-ttu-id="65c0e-164">Nyissa meg hello **szolgáltatások** Team Services, nyissa meg hello lap **új szolgáltatási végpont** listában, és válassza a **Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-164">Open hello **Services** page in Team Services, open hello **New Service Endpoint** list, and choose **Jenkins**.</span></span>

   ![Vegyen fel egy Jenkins végpontot](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. <span data-ttu-id="65c0e-166">Adjon meg egy nevet, toorefer toothis kapcsolat használni fog.</span><span class="sxs-lookup"><span data-stu-id="65c0e-166">Enter a name you will use toorefer toothis connection.</span></span>

1. <span data-ttu-id="65c0e-167">Adja meg a Jenkins kiszolgáló hello URL-CÍMÉT, és hello osztásjelek **nem megbízható az SSL-tanúsítványokat elfogadó** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="65c0e-167">Enter hello URL of your Jenkins server, and tick hello **Accept untrusted SSL certificates** option.</span></span>

1. <span data-ttu-id="65c0e-168">Adja meg a Jenkins fiók hello felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="65c0e-168">Enter hello user name and password for your Jenkins account.</span></span>

1. <span data-ttu-id="65c0e-169">Válasszon **kapcsolat ellenőrzéséhez** , amelyek információt hello toocheck helyességéről.</span><span class="sxs-lookup"><span data-stu-id="65c0e-169">Choose **Verify connection** toocheck that hello information is correct.</span></span>

1. <span data-ttu-id="65c0e-170">Válasszon **OK** toocreate hello szolgáltatásvégpontot.</span><span class="sxs-lookup"><span data-stu-id="65c0e-170">Choose **OK** toocreate hello service endpoint.</span></span>

## <a name="create-a-deployment-group"></a><span data-ttu-id="65c0e-171">Központi telepítési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="65c0e-171">Create a deployment group</span></span>

<span data-ttu-id="65c0e-172">Kell egy [üzembe helyezési csoport](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) túl a hello virtuális gépeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="65c0e-172">You need a [deployment group](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) too contain hello virtual machines.</span></span>

1. <span data-ttu-id="65c0e-173">Megnyitás hello **kiadásokban** hello lapján **Build &amp; kiadás** hub, majd nyissa meg hello **telepítési csoportban** lapot, és válassza a **+ új**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-173">Open hello **Releases** tab of hello **Build &amp; Release** hub, then open hello **Deployment groups** tab, and choose **+ New**.</span></span>

1. <span data-ttu-id="65c0e-174">Adjon meg egy nevet hello üzembe helyezési csoport, és opcionális leírását.</span><span class="sxs-lookup"><span data-stu-id="65c0e-174">Enter a name for hello deployment group, and an optional description.</span></span>
   <span data-ttu-id="65c0e-175">Válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-175">Then choose **Create**.</span></span>

<span data-ttu-id="65c0e-176">hello Azure erőforrás-csoport központi telepítésének feladatot hoz létre, és regisztrálja a hello virtuális gépek, hello Azure Resource Manager sablonnal futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="65c0e-176">hello Azure Resource Group Deployment task creates and registers hello VMs when it runs using hello Azure Resource Manager template.</span></span>
<span data-ttu-id="65c0e-177">Nem kell toocreate, és saját kezűleg hello a virtuális gépek regisztrálását.</span><span class="sxs-lookup"><span data-stu-id="65c0e-177">You don't need toocreate and register hello virtual machines yourself.</span></span>

## <a name="create-a-release-definition"></a><span data-ttu-id="65c0e-178">Egy kiadási definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="65c0e-178">Create a release definition</span></span>

<span data-ttu-id="65c0e-179">Egy kiadási definition határozza meg, hello folyamat Team Services toodeploy hello alkalmazás fogja végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="65c0e-179">A release definition specifies hello process Team Services will execute toodeploy hello app.</span></span>
<span data-ttu-id="65c0e-180">toocreate hello kiadás definíciójában Team Services:</span><span class="sxs-lookup"><span data-stu-id="65c0e-180">toocreate hello release definition in Team Services:</span></span>

1. <span data-ttu-id="65c0e-181">Megnyitás hello **kiadásokban** hello lapján **Build &amp; kiadási** hub, nyissa meg hello  **+**  legördülő kiadás definíciók hello listájában, és válassza a Hello **létrehozás kiadás definition**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-181">Open hello **Releases** tab of hello **Build &amp; Release** hub, open hello **+** drop-down in hello list of release definitions, and choose hello **Create release definition**.</span></span> 

1. <span data-ttu-id="65c0e-182">Jelölje be hello **üres** sablon válassza **következő**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-182">Select hello **Empty** template and choose **Next**.</span></span>

1. <span data-ttu-id="65c0e-183">A hello **összetevők** területen kattintson a **összetevő hivatkozás** válassza **Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-183">In hello **Artifacts** section, click on **Link an Artifact** and choose **Jenkins**.</span></span> <span data-ttu-id="65c0e-184">Válassza ki a Jenkins szolgáltatási végpont kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="65c0e-184">Select your Jenkins service endpoint connection.</span></span> <span data-ttu-id="65c0e-185">Ezután jelölje ki az hello Jenkins forrás feladatot, és válassza **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-185">Then select hello Jenkins source job and choose **Create**.</span></span> 

1. <span data-ttu-id="65c0e-186">Az új hello definition kiadási, válassza a **+ Hozzáadás feladatok** , és adja hozzá egy **Azure erőforrás-csoport központi telepítésének** toohello alapértelmezett feladatsor.</span><span class="sxs-lookup"><span data-stu-id="65c0e-186">In hello new release definition, choose **+ Add tasks** and add an **Azure Resource Group Deployment** task toohello default environment.</span></span>

1. <span data-ttu-id="65c0e-187">Hello legördülő nyílra következő toohello **+ Hozzáadás feladatok** hivatkozásra, és adja hozzá a telepítési fázis toohello meghatározása.</span><span class="sxs-lookup"><span data-stu-id="65c0e-187">Choose hello drop down arrow next toohello **+ Add tasks** link and add a deployment group phase toohello definition.</span></span>

   ![A központi telepítési csoport szakaszok hozzáadása](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. <span data-ttu-id="65c0e-189">Hello feladat katalógusban, nyissa meg a hello **segédprogram** szakaszt, és adjon hozzá egy hello **Héjparancsfájlt** feladat.</span><span class="sxs-lookup"><span data-stu-id="65c0e-189">In hello Task catalog, open hello **Utility** section and add an instance of hello **Shell Script** task.</span></span>

1. <span data-ttu-id="65c0e-190">hello Azure erőforrás-csoport központi telepítése a feladatban használt hello paraméterek sablon hello admin használt jelszó tooconnect toohello virtuális gépek állítja be.</span><span class="sxs-lookup"><span data-stu-id="65c0e-190">hello parameters template used in hello Azure Resource Group Deployment task sets hello admin password used tooconnect toohello VMs.</span></span>
   <span data-ttu-id="65c0e-191">Adja meg ezt a jelszót a hello változóval **$(adminpassword)**:</span><span class="sxs-lookup"><span data-stu-id="65c0e-191">You provide this password with hello variable **$(adminpassword)**:</span></span>
   
   - <span data-ttu-id="65c0e-192">Nyissa meg hello **változók** lap és hello **változók** területen adja meg a hello neve `adminpassword`.</span><span class="sxs-lookup"><span data-stu-id="65c0e-192">Open hello **Variables** tab and, in hello **Variables** section, enter hello name `adminpassword`.</span></span>

   - <span data-ttu-id="65c0e-193">Adja meg a hello rendszergazdai jelszót.</span><span class="sxs-lookup"><span data-stu-id="65c0e-193">Enter hello administrator password.</span></span>

   - <span data-ttu-id="65c0e-194">Hello "lakat" ikonra következő toohello érték szövegmező tooprotect hello jelszó választására.</span><span class="sxs-lookup"><span data-stu-id="65c0e-194">Choose hello "padlock" icon next toohello value textbox tooprotect hello password.</span></span> 

## <a name="configure-hello-azure-resource-group-deployment-task"></a><span data-ttu-id="65c0e-195">Hello Azure erőforrás-csoport központi telepítésének tevékenység konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65c0e-195">Configure hello Azure Resource Group Deployment task</span></span>

<span data-ttu-id="65c0e-196">Hello **Azure erőforrás-csoport központi telepítésének** feladata használt toocreate hello üzembe helyezési csoport.</span><span class="sxs-lookup"><span data-stu-id="65c0e-196">hello **Azure Resource Group Deployment** task is used toocreate hello deployment group.</span></span> <span data-ttu-id="65c0e-197">Konfigurálja úgy az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="65c0e-197">Configure it as follows:</span></span>

* <span data-ttu-id="65c0e-198">**Azure-előfizetés:** alatti hello listából válasszon ki egy kapcsolatot **Azure-szolgáltatás rendelkezésre álló kapcsolatok**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-198">**Azure Subscription:** Select a connection from hello list under **Available Azure Service Connections**.</span></span> 
  <span data-ttu-id="65c0e-199">Ha a kapcsolat nem jelenik meg, válassza a **kezelése**, jelölje be **új szolgáltatási végpont** majd **Azure Resource Manager**, és hello utasításokat követve.</span><span class="sxs-lookup"><span data-stu-id="65c0e-199">If no connections appear, choose **Manage**, select **New Service Endpoint** then **Azure Resource Manager**, and follow hello prompts.</span></span>
  <span data-ttu-id="65c0e-200">Térjen vissza a tooyour kiadási definícióját, frissítési hello **AzureRM előfizetés** listából, és a létrehozott válassza hello kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="65c0e-200">Return tooyour release definition, refresh hello **AzureRM Subscription** list, and select hello connection you created.</span></span>

* <span data-ttu-id="65c0e-201">**Erőforráscsoport**: Adjon meg egy korábban létrehozott erőforráscsoportot hello.</span><span class="sxs-lookup"><span data-stu-id="65c0e-201">**Resource group**: Enter a name of hello resource group you created earlier.</span></span>

* <span data-ttu-id="65c0e-202">**Hely**: Válasszon ki egy régiót hello központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="65c0e-202">**Location**: Select a region for hello deployment.</span></span>

  ![Új erőforráscsoport létrehozása](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* <span data-ttu-id="65c0e-204">**A sablon helye**:`URL of hello file`</span><span class="sxs-lookup"><span data-stu-id="65c0e-204">**Template location**: `URL of hello file`</span></span>

* <span data-ttu-id="65c0e-205">**Sablon hivatkozás**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.json`</span><span class="sxs-lookup"><span data-stu-id="65c0e-205">**Template link**: `{your-git-repo}/ARM-Templates/UbuntuWeb1.json`</span></span>

* <span data-ttu-id="65c0e-206">**Sablon paraméterei link**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`</span><span class="sxs-lookup"><span data-stu-id="65c0e-206">**Template parameters link**: `{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`</span></span>

* <span data-ttu-id="65c0e-207">**Sablon paraméterének**: hello listája felülírják értékeket, például: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.</span><span class="sxs-lookup"><span data-stu-id="65c0e-207">**Override template parameters**: A list of hello override values, for example: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.</span></span><br /><span data-ttu-id="65c0e-208">Helyezze be a saját hello {helyőrzők} adott értékeit.</span><span class="sxs-lookup"><span data-stu-id="65c0e-208">Insert your own specific values for hello {placeholders}.</span></span> 

* <span data-ttu-id="65c0e-209">**Előfeltételek engedélyezése**:`Configure with Deployment Group agent`</span><span class="sxs-lookup"><span data-stu-id="65c0e-209">**Enable prerequisites**: `Configure with Deployment Group agent`</span></span>

* <span data-ttu-id="65c0e-210">**TFS/VSTS-végpont**: válasszon **Hozzáadás** , és válassza az "Új Team Foundation Server/Team Services-kapcsolat hozzáadása" hello párbeszédpanelen **jogkivonat-alapú hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-210">**TFS/VSTS endpoint**: Choose **Add** and, in hello "Add new Team Foundation Server/Team Services Connection" dialog, select **Token Based Authentication**.</span></span> <span data-ttu-id="65c0e-211">Adja meg a hello kapcsolat neve és a csapatprojekt hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="65c0e-211">Enter a name of hello connection and hello URL of your team project.</span></span> <span data-ttu-id="65c0e-212">Ezután létre, és adja meg a [személyes hozzáférési jogkivonat (PAT)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) tooauthenticate hello kapcsolat tooyour csapatprojektben.</span><span class="sxs-lookup"><span data-stu-id="65c0e-212">Then generate and enter a [Personal Access Token (PAT)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) tooauthenticate hello connection tooyour team project.</span></span>

  ![Hozzon létre egy személyes hozzáférési tokent](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* <span data-ttu-id="65c0e-214">**Csapatprojekt**: válassza ki az aktuális projektben.</span><span class="sxs-lookup"><span data-stu-id="65c0e-214">**Team project**: Select your current project.</span></span>

* <span data-ttu-id="65c0e-215">**Üzembe helyezési csoport**: Adjon meg hello azonos telepítési csoport neve, mint hello használt **erőforráscsoport** paraméter.</span><span class="sxs-lookup"><span data-stu-id="65c0e-215">**Deployment Group**: Enter hello same deployment group name as you used for hello **Resource group** parameter.</span></span>

<span data-ttu-id="65c0e-216">hello Azure erőforrás-csoport központi telepítésének feladat hello alapértelmezett beállításai toocreate vagy olyan erőforráshoz és toodo így Növekményesen frissíteni.</span><span class="sxs-lookup"><span data-stu-id="65c0e-216">hello default settings for hello Azure Resource Group Deployment task are toocreate or update a resource, and toodo so incrementally.</span></span> <span data-ttu-id="65c0e-217">hello feladat hoz létre a virtuális gépek hello hello első alkalommal fut, és csak ezt követően frissítse azokat.</span><span class="sxs-lookup"><span data-stu-id="65c0e-217">hello task creates hello VMs hello first time it runs, and subsequently just update them.</span></span>

## <a name="configure-hello-shell-script-task"></a><span data-ttu-id="65c0e-218">Hello Héjparancsfájlt tevékenység konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65c0e-218">Configure hello Shell Script task</span></span>

<span data-ttu-id="65c0e-219">Hello **Héjparancsfájlt** feladata egy parancsfájl toorun minden kiszolgáló tooinstall Node.js és kezdő hello alkalmazásában használt tooprovide hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="65c0e-219">hello **Shell Script** task is used tooprovide hello configuration for a script toorun on each server tooinstall Node.js and start hello app.</span></span> <span data-ttu-id="65c0e-220">Konfigurálja úgy az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="65c0e-220">Configure it as follows:</span></span>

* <span data-ttu-id="65c0e-221">**Parancsfájl elérési útja**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`</span><span class="sxs-lookup"><span data-stu-id="65c0e-221">**Script Path**: `$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`</span></span>

* <span data-ttu-id="65c0e-222">**Adjon meg működő Directory**:`Checked`</span><span class="sxs-lookup"><span data-stu-id="65c0e-222">**Specify Working Directory**: `Checked`</span></span>

* <span data-ttu-id="65c0e-223">**Munkakönyvtár**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`</span><span class="sxs-lookup"><span data-stu-id="65c0e-223">**Working Directory**: `$(System.DefaultWorkingDirectory)/Fabrikam-Node`</span></span>
   
## <a name="rename-and-save-hello-release-definition"></a><span data-ttu-id="65c0e-224">Nevezze át és hello kiadás definíció mentése</span><span class="sxs-lookup"><span data-stu-id="65c0e-224">Rename and save hello release definition</span></span>

1. <span data-ttu-id="65c0e-225">Hello neve hello kiadás definition toohello megadott szerkesztése a **utáni műveletek** Jenkins hello buildverziót lapján.</span><span class="sxs-lookup"><span data-stu-id="65c0e-225">Edit hello name of hello release definition toohello name you specified in the **Post-build Actions** tab of hello build in Jenkins.</span></span> <span data-ttu-id="65c0e-226">Jenkins igényel a név toobe képes tootrigger új kiadását, hello forrás összetevők frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="65c0e-226">Jenkins requires this name toobe able tootrigger a new release when hello source artifacts are updated.</span></span>

1. <span data-ttu-id="65c0e-227">Igény szerint megváltoztathatja hello környezet hello nevét a hello nevére kattintva.</span><span class="sxs-lookup"><span data-stu-id="65c0e-227">Optionally, change hello name of hello environment by clicking on hello name.</span></span> 

1. <span data-ttu-id="65c0e-228">Válasszon **mentése**, és válassza a **OK**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-228">Choose **Save**, and choose **OK**.</span></span>

## <a name="start-a-manual-deployment"></a><span data-ttu-id="65c0e-229">Indítsa el a manuális központi telepítése</span><span class="sxs-lookup"><span data-stu-id="65c0e-229">Start a manual deployment</span></span>

1. <span data-ttu-id="65c0e-230">Válassza ki **+ kiadási** válassza **létrehozása kiadási**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-230">Choose **+ Release** and select **Create Release**.</span></span>

1. <span data-ttu-id="65c0e-231">Hello build elvégezte hello kiemelt legördülő listában jelölje ki, és válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="65c0e-231">Select hello build you completed in hello highlighted drop-down list and choose **Create**.</span></span>

1. <span data-ttu-id="65c0e-232">Hello felbukkanó üzenet válassza hello kiadás hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="65c0e-232">Choose hello release link in hello popup message.</span></span> <span data-ttu-id="65c0e-233">Például: "kiadás **Release-1** létrejött."</span><span class="sxs-lookup"><span data-stu-id="65c0e-233">For example: "Release **Release-1** has been created."</span></span>

1. <span data-ttu-id="65c0e-234">Nyissa meg hello **naplók** toowatch hello kiadás konzol kimeneti fülre.</span><span class="sxs-lookup"><span data-stu-id="65c0e-234">Open hello **Logs** tab toowatch hello release console output.</span></span>

1. <span data-ttu-id="65c0e-235">A böngészőben nyissa meg a hello URL-cím hello kiszolgálók közül az egyik tooyour üzembe helyezési csoport hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="65c0e-235">In your browser, open hello URL of one of hello servers you added tooyour deployment group.</span></span> <span data-ttu-id="65c0e-236">Adja meg például`http://{your-server-ip-address}`</span><span class="sxs-lookup"><span data-stu-id="65c0e-236">For example, enter `http://{your-server-ip-address}`</span></span>

## <a name="start-a-cicd-deployment"></a><span data-ttu-id="65c0e-237">Indítsa el a CI/CD központi telepítés</span><span class="sxs-lookup"><span data-stu-id="65c0e-237">Start a CI/CD deployment</span></span>

1. <span data-ttu-id="65c0e-238">Hello kiadásában definícióját, törölje a jelet hello **engedélyezve** hello jelölőnégyzet **beállítások** hello Azure erőforrás-csoport központi telepítésének feladat hello beállításai szakasza.</span><span class="sxs-lookup"><span data-stu-id="65c0e-238">In hello release definition, uncheck hello **Enabled** checkbox in hello **Control Options** section of hello settings for hello Azure Resource Group Deployment task.</span></span>
   <span data-ttu-id="65c0e-239">A jövőben a központi telepítésekre toohello meglévő üzembe helyezési csoport, nem kell toore – a feladat végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="65c0e-239">For future deployments toohello existing deployment group, you do not need toore-execute this task.</span></span>

1. <span data-ttu-id="65c0e-240">Nyissa meg toohello forrás Git-tárház és hello hello tartalmának módosítása **h1** hello fájlban fejléc [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).</span><span class="sxs-lookup"><span data-stu-id="65c0e-240">Go toohello source Git repository and modify hello contents of hello **h1** heading in hello file [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).</span></span>

1. <span data-ttu-id="65c0e-241">A módosítás véglegesítése.</span><span class="sxs-lookup"><span data-stu-id="65c0e-241">Commit your change.</span></span>

1. <span data-ttu-id="65c0e-242">Néhány perc elteltével látni fogja a hello létrehozott új kiadási **kiadásokban** Team Services vagy a TFS oldalán.</span><span class="sxs-lookup"><span data-stu-id="65c0e-242">After a few minutes, you will see a new release created in hello **Releases** page of Team Services or TFS.</span></span> <span data-ttu-id="65c0e-243">Nyissa meg a hello kiadás toosee hello telepítése folyamatban.</span><span class="sxs-lookup"><span data-stu-id="65c0e-243">Open hello release toosee hello deployment taking place.</span></span> <span data-ttu-id="65c0e-244">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="65c0e-244">Congratulations!</span></span>

## <a name="next-steps"></a><span data-ttu-id="65c0e-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65c0e-245">Next Steps</span></span>

<span data-ttu-id="65c0e-246">Ebben az oktatóanyagban automatikus hello központi telepítése egy app tooAzure Jenkins build és a kiadáshoz Team Services használatával.</span><span class="sxs-lookup"><span data-stu-id="65c0e-246">In this tutorial, you automated hello deployment of an app tooAzure using Jenkins build and Team Services for release.</span></span> <span data-ttu-id="65c0e-247">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="65c0e-247">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="65c0e-248">Az alkalmazás a Jenkins elkészítésére</span><span class="sxs-lookup"><span data-stu-id="65c0e-248">Build your app in Jenkins</span></span>
> * <span data-ttu-id="65c0e-249">Jenkins Team Services-integráció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65c0e-249">Configure Jenkins for Team Services integration</span></span>
> * <span data-ttu-id="65c0e-250">Egy központi telepítési csoport hello Azure virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="65c0e-250">Create a deployment group for hello Azure virtual machines</span></span>
> * <span data-ttu-id="65c0e-251">Konfigurálja a hello virtuális gépek és hello alkalmazást telepíti a kiadási-definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="65c0e-251">Create a release definition that configures hello VMs and deploys hello app</span></span>

<span data-ttu-id="65c0e-252">Előzetes toohello következő útmutató toolearn kapcsolatos hogyan toodeploy egy LÁMPA (Linux, Apache, MySQL és a PHP) a verem.</span><span class="sxs-lookup"><span data-stu-id="65c0e-252">Advance toohello next tutorial toolearn more about how toodeploy a LAMP (Linux, Apache, MySQL, and PHP) stack.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="65c0e-253">LAMP stack telepítése</span><span class="sxs-lookup"><span data-stu-id="65c0e-253">Deploy LAMP stack</span></span>](tutorial-lamp-stack.md)