---
title: "CI/CD Jenkins az Azure virtuális gépek Team Services |} Microsoft Docs"
description: "Folyamatos integrációt (CI) és a folyamatos üzembe helyezés (CD) Jenkins segítségével Azure virtuális gépeken futó szolgáltatásban a Visual Studio Team Services (VSTS) vagy a Microsoft Team Foundation Server (TFS) a Node.js-alkalmazás beállítása"
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
ms.openlocfilehash: a40e26a8681df31fad664e4d1df4c1513311900d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-your-app-to-linux-vms-using-jenkins-and-team-services"></a><span data-ttu-id="9783a-103">Az alkalmazás telepítése Linux virtuális gépek Jenkins és Team Services használatával</span><span class="sxs-lookup"><span data-stu-id="9783a-103">Deploy your app to Linux VMs using Jenkins and Team Services</span></span>

<span data-ttu-id="9783a-104">Folyamatos integrációt (CI) és a folyamatos üzembe helyezés (CD) nem egy folyamatot, amely létre, a kiadási és a kód telepítésére.</span><span class="sxs-lookup"><span data-stu-id="9783a-104">Continuous integration (CI) and continuous deployment (CD) is a pipeline by which you can build, release, and deploy your code.</span></span> <span data-ttu-id="9783a-105">Team Services számos teljes, teljes funkcionalitású CI/CD automatizálási eszközeivel központi telepítés az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="9783a-105">Team Services provides a complete, fully featured set of CI/CD automation tools for deployment to Azure.</span></span> <span data-ttu-id="9783a-106">Jenkins egy olyan népszerű 3. fél CI/CD server-alapú eszköz, amely CI/CD automation is biztosít.</span><span class="sxs-lookup"><span data-stu-id="9783a-106">Jenkins is a popular 3rd-party CI/CD server-based tool that also provides CI/CD automation.</span></span> <span data-ttu-id="9783a-107">Segítségével mindkét együtt a felhőalapú alkalmazás vagy szolgáltatás biztosításához hogyan testreszabása.</span><span class="sxs-lookup"><span data-stu-id="9783a-107">You can use both together to customize how you deliver your cloud app or service.</span></span>

<span data-ttu-id="9783a-108">Az oktatóanyag segítségével Jenkins összeállítása a **Node.js webalkalmazás**, és a Visual Studio Team Services telepítéséhez, úgy, hogy egy [üzembe helyezési csoport](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) Linux virtuális gépeket tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="9783a-108">In this tutorial, you use Jenkins to build a **Node.js web app**, and Visual Studio Team Services to deploy it to a [deployment group](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) containing Linux virtual machines.</span></span>

<span data-ttu-id="9783a-109">Az alábbiakat fogja elvégezni:</span><span class="sxs-lookup"><span data-stu-id="9783a-109">You will:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9783a-110">Az alkalmazás a Jenkins elkészítésére</span><span class="sxs-lookup"><span data-stu-id="9783a-110">Build your app in Jenkins</span></span>
> * <span data-ttu-id="9783a-111">Jenkins Team Services-integráció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9783a-111">Configure Jenkins for Team Services integration</span></span>
> * <span data-ttu-id="9783a-112">Az Azure virtuális gépek telepítési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="9783a-112">Create a deployment group for the Azure virtual machines</span></span>
> * <span data-ttu-id="9783a-113">Hozzon létre olyan kiadási-definícióval, konfigurálja a virtuális gépeket, és telepíti az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="9783a-113">Create a release definition that configures the VMs and deploys the app</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="9783a-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="9783a-114">Before you begin</span></span>

* <span data-ttu-id="9783a-115">Hozzáférés Jenkins fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="9783a-115">You need access to a Jenkins account.</span></span> <span data-ttu-id="9783a-116">Ha még nem hozott létre egy Jenkins server, lásd: [Jenkins dokumentáció](https://jenkins.io/doc/).</span><span class="sxs-lookup"><span data-stu-id="9783a-116">If you have not yet created a Jenkins server, see [Jenkins Documentation](https://jenkins.io/doc/).</span></span> 

* <span data-ttu-id="9783a-117">Jelentkezzen be a Team Services-fiókhoz (`https://{youraccount}.visualstudio.com`).</span><span class="sxs-lookup"><span data-stu-id="9783a-117">Sign in to your Team Services account (`https://{youraccount}.visualstudio.com`).</span></span> 
  <span data-ttu-id="9783a-118">Beszerezheti a [ingyenes Team Services-fiókot](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).</span><span class="sxs-lookup"><span data-stu-id="9783a-118">You can get a [free Team Services account](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).</span></span>

  > [!NOTE]
  > <span data-ttu-id="9783a-119">További információkért lásd: [Team Services kapcsolódás](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).</span><span class="sxs-lookup"><span data-stu-id="9783a-119">For more information, see [Connect to Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).</span></span>

* <span data-ttu-id="9783a-120">Ha még nem rendelkezik egy, hozzon létre egy személyes hozzáférési jogkivonat (PAT) Team Services-fiókját.</span><span class="sxs-lookup"><span data-stu-id="9783a-120">Create a personal access token (PAT) in your Team Services account if you don't already have one.</span></span> <span data-ttu-id="9783a-121">Jenkins a Team Services-fiók eléréséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="9783a-121">Jenkins requires this information to access your Team Services account.</span></span>
  <span data-ttu-id="9783a-122">Olvasási [hogyan hozható létre személyes hozzáférési jogkivonat Team Services és a TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) megtudhatja, hogyan hozhat létre egy.</span><span class="sxs-lookup"><span data-stu-id="9783a-122">Read [How do I create a personal access token for Team Services and TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) to learn how to generate one.</span></span>

## <a name="get-the-sample-app"></a><span data-ttu-id="9783a-123">A minta-alkalmazás letöltése</span><span class="sxs-lookup"><span data-stu-id="9783a-123">Get the sample app</span></span>

<span data-ttu-id="9783a-124">Kell telepíteni kívánt alkalmazást, a Git-tárház tárolja.</span><span class="sxs-lookup"><span data-stu-id="9783a-124">You need an app to deploy stored in a Git repository.</span></span>
<span data-ttu-id="9783a-125">A jelen oktatóanyag esetében javasoljuk, használjon [a mintaalkalmazás elérhető a Githubon](https://github.com/azooinmyluggage/fabrikam-node).</span><span class="sxs-lookup"><span data-stu-id="9783a-125">For this tutorial, we recommend you use [this sample app available from GitHub](https://github.com/azooinmyluggage/fabrikam-node).</span></span>

1. <span data-ttu-id="9783a-126">Hozzon létre egy elágazás az alkalmazáshoz, és tekintse meg a helye (URL) számára ez az oktatóanyag későbbi lépéseiben.</span><span class="sxs-lookup"><span data-stu-id="9783a-126">Create a fork of this app and take note of the location (URL) for use in later steps of this tutorial.</span></span>

1. <span data-ttu-id="9783a-127">Ellenőrizze a elágazás **nyilvános** való egyszerűbben csatlakozhatnak a Githubon később.</span><span class="sxs-lookup"><span data-stu-id="9783a-127">Make the fork **public** to simplify connecting to GitHub later.</span></span>

> [!NOTE]
> <span data-ttu-id="9783a-128">További információkért lásd: [oszthatja ketté a tárházban](https://help.github.com/articles/fork-a-repo/) és [nyilvános és titkos tárház](https://help.github.com/articles/making-a-private-repository-public/).</span><span class="sxs-lookup"><span data-stu-id="9783a-128">For more information, see [Fork a repo](https://help.github.com/articles/fork-a-repo/) and [Making a private repository public](https://help.github.com/articles/making-a-private-repository-public/).</span></span>

> [!NOTE]
> <span data-ttu-id="9783a-129">Az alkalmazás használatával lett létrehozva [Yeoman](http://yeoman.io/learning/index.html); használ **Express**, **bower**, és **grunt**; és van-e néhány **npm** csomagok szerint függőségeinek.</span><span class="sxs-lookup"><span data-stu-id="9783a-129">The app was built using [Yeoman](http://yeoman.io/learning/index.html); it uses **Express**, **bower**, and **grunt**; and it has some **npm** packages as dependencies.</span></span>
> <span data-ttu-id="9783a-130">A mintaalkalmazás tartalmaz [Azure Resource Manager-sablonok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) dinamikusan létrehozni a virtuális gépek telepítési Azure használt.</span><span class="sxs-lookup"><span data-stu-id="9783a-130">The sample app contains a set of [Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) that are used to dynamically create the virtual machines for deployment on Azure.</span></span> <span data-ttu-id="9783a-131">A feladatok által használt ezeket a sablonokat a [Team Services kiadási definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).</span><span class="sxs-lookup"><span data-stu-id="9783a-131">These templates are used by tasks in the [Team Services release definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).</span></span>
> <span data-ttu-id="9783a-132">A fő sablont hoz létre a hálózati biztonsági csoport, a virtuális gépek és virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="9783a-132">The main template creates a network security group, a virtual machine, and a virtual network.</span></span> <span data-ttu-id="9783a-133">A nyilvános IP-címet rendel, és megnyílik a 80-as portot a bejövő.</span><span class="sxs-lookup"><span data-stu-id="9783a-133">It assigns a public IP address and opens inbound port 80.</span></span> <span data-ttu-id="9783a-134">Hozzáadja egy címkét, amellyel a központi telepítési csoport válassza ki a gépeket megkapná a központi telepítést is.</span><span class="sxs-lookup"><span data-stu-id="9783a-134">It also adds a tag that is used by the deployment group to select the machines to receive the deployment.</span></span>
>
> <span data-ttu-id="9783a-135">A minta is tartalmaz egy parancsfájlt, amely beállítja a Nginx telepíti az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9783a-135">The sample also contains a script that sets up Nginx and deploys the app.</span></span> <span data-ttu-id="9783a-136">Az egyes virtuális gépek végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="9783a-136">It is executed on each of the virtual machines.</span></span> <span data-ttu-id="9783a-137">Pontosabban a parancsfájl telepíti-e a csomópont, Nginx és PM2; Konfigurálja az Nginx és PM2; Ezután elindítja a csomópont-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9783a-137">Specifically, the script installs Node, Nginx, and PM2; configures Nginx and PM2; then starts the Node app.</span></span>

## <a name="configure-jenkins-plugins"></a><span data-ttu-id="9783a-138">Jenkins beépülő modulok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9783a-138">Configure Jenkins plugins</span></span>

<span data-ttu-id="9783a-139">Először konfigurálnia kell a két Jenkins beépülők **NodeJS** és **integráció a Team Services**.</span><span class="sxs-lookup"><span data-stu-id="9783a-139">First, you must configure two Jenkins plugins for **NodeJS** and **Integration with Team Services**.</span></span>

1. <span data-ttu-id="9783a-140">Nyissa meg a Jenkins fiókját, és válassza a **kezelése Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="9783a-140">Open your Jenkins account and choose **Manage Jenkins**.</span></span>

1. <span data-ttu-id="9783a-141">Az a **kezelése Jenkins** lapon, válassza ki **kezelése beépülő modulok**.</span><span class="sxs-lookup"><span data-stu-id="9783a-141">In the **Manage Jenkins** page, choose **Manage Plugins**.</span></span>

1. <span data-ttu-id="9783a-142">Keresse meg a listát szűrheti a **NodeJS** beépülő modul és telepítse a restart záradék nélkül.</span><span class="sxs-lookup"><span data-stu-id="9783a-142">Filter the list to locate the **NodeJS** plugin and install it without restart.</span></span>

   ![A NodeJS beépülő modul hozzáadása Jenkins](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. <span data-ttu-id="9783a-144">Szűrheti a listát a **a Team Foundation Server** beépülő modul és telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9783a-144">Filter the list to find the **Team Foundation Server** plugin and install it.</span></span> <span data-ttu-id="9783a-145">(Ez a Team Services és a Team Foundation Server a beépülő modul működik.) Jenkins újraindítása nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="9783a-145">(This plug-in works for both Team Services and Team Foundation Server.) Restarting Jenkins is not necessary.</span></span>

## <a name="configure-jenkins-build-for-nodejs"></a><span data-ttu-id="9783a-146">A Node.js Jenkins build konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9783a-146">Configure Jenkins build for Node.js</span></span>

<span data-ttu-id="9783a-147">A Jenkins hozzon létre egy új build projektet, és konfigurálja az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9783a-147">In Jenkins, create a new build project and configure it as follows:</span></span>

1. <span data-ttu-id="9783a-148">Az a **általános** lapra, adja meg a build projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="9783a-148">In the **General** tab, enter a name for your build project.</span></span>

1. <span data-ttu-id="9783a-149">Az a **forrás kód felügyeleti** lapon jelölje be **Git** , és adja meg a tárház és az alkalmazás kódját tartalmazó fiókirodai részleteit.</span><span class="sxs-lookup"><span data-stu-id="9783a-149">In the **Source Code Management** tab, select **Git** and enter the details of the repository and the branch containing your app code.</span></span>

   ![A tárház hozzáadása a build](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > <span data-ttu-id="9783a-151">Ha a tárház nem nyilvános, válasszon **Hozzáadás** adja meg hitelesítő adatokat a csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="9783a-151">If your repository is not public, choose **Add** and provide credentials to connect to it.</span></span>

1. <span data-ttu-id="9783a-152">Az a **Build eseményindítók** lapon jelölje be **lekérdezési SCM** , és írja be az ütemezés `H/03 * * * *` , és kérdezze le a Git-tárház változásainak három percenként.</span><span class="sxs-lookup"><span data-stu-id="9783a-152">In the **Build Triggers** tab, select **Poll SCM** and enter the schedule `H/03 * * * *` to poll the Git repository for changes every three minutes.</span></span> 

1. <span data-ttu-id="9783a-153">A a **telepítési környezet** lapon jelölje be **meg csomópont &amp; npm bin / mappa elérési ÚTJA** , és írja be `NodeJS` csomópont JS telepítés érték.</span><span class="sxs-lookup"><span data-stu-id="9783a-153">In the **Build Environment** tab, select **Provide Node &amp; npm bin/ folder PATH** and enter `NodeJS` for the Node JS Installation value.</span></span> <span data-ttu-id="9783a-154">Hagyja **npmrc fájl** "használja a rendszer alapértelmezett."</span><span class="sxs-lookup"><span data-stu-id="9783a-154">Leave **npmrc file** set to "use system default."</span></span>

1. <span data-ttu-id="9783a-155">Az a **Build** lapra, adja meg a parancsot `npm install` annak biztosítása érdekében minden függőséget frissülnek.</span><span class="sxs-lookup"><span data-stu-id="9783a-155">In the **Build** tab, enter the command `npm install` to ensure all dependencies are updated.</span></span>

## <a name="configure-jenkins-for-team-services-integration"></a><span data-ttu-id="9783a-156">Jenkins Team Services-integráció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9783a-156">Configure Jenkins for Team Services integration</span></span>

1. <span data-ttu-id="9783a-157">Az a **utáni műveletek** lapon, a **archiválására fájlok**, adja meg `**/*` összes fájlokról.</span><span class="sxs-lookup"><span data-stu-id="9783a-157">In the **Post-build Actions** tab, for **Files to archive**, enter `**/*` to include all files.</span></span>

1. <span data-ttu-id="9783a-158">A **indul el, a TFS/Team Services kiadás**, adja meg a fiók teljes URL-CÍMÉT (például `https://your-account-name.visualstudio.com`), a projekt nevét, egy nevet a kiadási definition (később létre), és a fiókjához szükséges hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="9783a-158">For **Trigger release in TFS/Team Services**, enter the full URL of your account (such as `https://your-account-name.visualstudio.com`), the project name, a name for the release definition (created later), and the credentials to connect to your account.</span></span>
   <span data-ttu-id="9783a-159">A felhasználónév és a korábban létrehozott PAT van szüksége.</span><span class="sxs-lookup"><span data-stu-id="9783a-159">You need your user name and the PAT you created earlier.</span></span> 

   ![Jenkins utáni műveletek konfigurálása](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. <span data-ttu-id="9783a-161">A build projekt mentése.</span><span class="sxs-lookup"><span data-stu-id="9783a-161">Save the build project.</span></span>

## <a name="create-a-jenkins-service-endpoint"></a><span data-ttu-id="9783a-162">Hozzon létre egy Jenkins szolgáltatási végpont</span><span class="sxs-lookup"><span data-stu-id="9783a-162">Create a Jenkins service endpoint</span></span>

<span data-ttu-id="9783a-163">A szolgáltatási végpont lehetővé teszi, hogy a Team Services Jenkins való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="9783a-163">A service endpoint allows Team Services to connect to Jenkins.</span></span>

1. <span data-ttu-id="9783a-164">Nyissa meg a **szolgáltatások** Team Services, nyissa meg a lap a **új szolgáltatási végpont** listában, és válassza a **Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="9783a-164">Open the **Services** page in Team Services, open the **New Service Endpoint** list, and choose **Jenkins**.</span></span>

   ![Vegyen fel egy Jenkins végpontot](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. <span data-ttu-id="9783a-166">Adjon meg egy nevet, tekintse meg a kapcsolat használatával fogja.</span><span class="sxs-lookup"><span data-stu-id="9783a-166">Enter a name you will use to refer to this connection.</span></span>

1. <span data-ttu-id="9783a-167">Adja meg az URL-CÍMÉT a Jenkins kiszolgáló, és az osztásjelek a **nem megbízható az SSL-tanúsítványokat elfogadó** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9783a-167">Enter the URL of your Jenkins server, and tick the **Accept untrusted SSL certificates** option.</span></span>

1. <span data-ttu-id="9783a-168">Adja meg a felhasználónevet és jelszót a Jenkins fiók.</span><span class="sxs-lookup"><span data-stu-id="9783a-168">Enter the user name and password for your Jenkins account.</span></span>

1. <span data-ttu-id="9783a-169">Válasszon **kapcsolat ellenőrzéséhez** futtatásával ellenőrizze, hogy az információ helyes.</span><span class="sxs-lookup"><span data-stu-id="9783a-169">Choose **Verify connection** to check that the information is correct.</span></span>

1. <span data-ttu-id="9783a-170">Válasszon **OK** a végpont létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9783a-170">Choose **OK** to create the service endpoint.</span></span>

## <a name="create-a-deployment-group"></a><span data-ttu-id="9783a-171">Központi telepítési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="9783a-171">Create a deployment group</span></span>

<span data-ttu-id="9783a-172">Kell egy [üzembe helyezési csoport](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) magában foglalja a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="9783a-172">You need a [deployment group](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) to  contain the virtual machines.</span></span>

1. <span data-ttu-id="9783a-173">Nyissa meg a **kiadásokban** lapján a **Build &amp; kiadás** hub, nyissa meg a **telepítési csoportban** lapot, és válassza a **+ új**.</span><span class="sxs-lookup"><span data-stu-id="9783a-173">Open the **Releases** tab of the **Build &amp; Release** hub, then open the **Deployment groups** tab, and choose **+ New**.</span></span>

1. <span data-ttu-id="9783a-174">Adjon meg egy nevet a központi telepítési csoportot, és opcionális leírását.</span><span class="sxs-lookup"><span data-stu-id="9783a-174">Enter a name for the deployment group, and an optional description.</span></span>
   <span data-ttu-id="9783a-175">Válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9783a-175">Then choose **Create**.</span></span>

<span data-ttu-id="9783a-176">Az Azure erőforrás-csoport központi telepítésének feladat hoz létre, és regisztrálja a virtuális gépek, amikor fut az Azure Resource Manager sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="9783a-176">The Azure Resource Group Deployment task creates and registers the VMs when it runs using the Azure Resource Manager template.</span></span>
<span data-ttu-id="9783a-177">Nem kell létrehozni és regisztrálni a virtuális gépek magát.</span><span class="sxs-lookup"><span data-stu-id="9783a-177">You don't need to create and register the virtual machines yourself.</span></span>

## <a name="create-a-release-definition"></a><span data-ttu-id="9783a-178">Egy kiadási definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="9783a-178">Create a release definition</span></span>

<span data-ttu-id="9783a-179">Egy kiadási definition határozza meg, a folyamat Team Services fogja végrehajtani az alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="9783a-179">A release definition specifies the process Team Services will execute to deploy the app.</span></span>
<span data-ttu-id="9783a-180">A kiadási definíció létrehozása Team Services:</span><span class="sxs-lookup"><span data-stu-id="9783a-180">To create the release definition in Team Services:</span></span>

1. <span data-ttu-id="9783a-181">Nyissa meg a **kiadásokban** lapján a **Build &amp; kiadási** központi, nyissa meg a  **+**  legördülő kiadás definíciók listáján, majd válassza ki a a  **Kiadás definíció létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9783a-181">Open the **Releases** tab of the **Build &amp; Release** hub, open the **+** drop-down in the list of release definitions, and choose the **Create release definition**.</span></span> 

1. <span data-ttu-id="9783a-182">Válassza ki a **üres** sablon kiválasztása és **következő**.</span><span class="sxs-lookup"><span data-stu-id="9783a-182">Select the **Empty** template and choose **Next**.</span></span>

1. <span data-ttu-id="9783a-183">Az a **összetevők** területen kattintson a **összetevő hivatkozás** válassza **Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="9783a-183">In the **Artifacts** section, click on **Link an Artifact** and choose **Jenkins**.</span></span> <span data-ttu-id="9783a-184">Válassza ki a Jenkins szolgáltatási végpont kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="9783a-184">Select your Jenkins service endpoint connection.</span></span> <span data-ttu-id="9783a-185">Ezután jelölje ki a Jenkins forrás feladatot, és válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9783a-185">Then select the Jenkins source job and choose **Create**.</span></span> 

1. <span data-ttu-id="9783a-186">Az új kiadási definícióját, válassza a **+ Hozzáadás feladatok** , és adja hozzá egy **Azure erőforrás-csoport központi telepítésének** feladat az alapértelmezett környezetre.</span><span class="sxs-lookup"><span data-stu-id="9783a-186">In the new release definition, choose **+ Add tasks** and add an **Azure Resource Group Deployment** task to the default environment.</span></span>

1. <span data-ttu-id="9783a-187">Válassza ki a legördülő nyílra mellett a **+ Hozzáadás feladatok** hivatkozásra, és adja hozzá a központi telepítési csoport szakaszok a definíció.</span><span class="sxs-lookup"><span data-stu-id="9783a-187">Choose the drop down arrow next to the **+ Add tasks** link and add a deployment group phase to the definition.</span></span>

   ![A központi telepítési csoport szakaszok hozzáadása](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. <span data-ttu-id="9783a-189">A feladat-katalógusban, nyissa meg a **segédprogram** szakaszt, és adjon hozzá egy a **Héjparancsfájlt** feladat.</span><span class="sxs-lookup"><span data-stu-id="9783a-189">In the Task catalog, open the **Utility** section and add an instance of the **Shell Script** task.</span></span>

1. <span data-ttu-id="9783a-190">Az Azure erőforrás-csoport központi telepítése a feladatban használt paraméterek sablon állítja be a virtuális gépek való csatlakozáshoz használt rendszergazdai jelszavát.</span><span class="sxs-lookup"><span data-stu-id="9783a-190">The parameters template used in the Azure Resource Group Deployment task sets the admin password used to connect to the VMs.</span></span>
   <span data-ttu-id="9783a-191">Ezt a jelszót adja meg a változó a **$(adminpassword)**:</span><span class="sxs-lookup"><span data-stu-id="9783a-191">You provide this password with the variable **$(adminpassword)**:</span></span>
   
   - <span data-ttu-id="9783a-192">Nyissa meg a **változók** lapon és a a **változók** területen adja meg a név `adminpassword`.</span><span class="sxs-lookup"><span data-stu-id="9783a-192">Open the **Variables** tab and, in the **Variables** section, enter the name `adminpassword`.</span></span>

   - <span data-ttu-id="9783a-193">Adja meg a rendszergazdai jelszót.</span><span class="sxs-lookup"><span data-stu-id="9783a-193">Enter the administrator password.</span></span>

   - <span data-ttu-id="9783a-194">Válassza a "lakat" ikont a érték szövegbeviteli mező a jelszó védelme mellett.</span><span class="sxs-lookup"><span data-stu-id="9783a-194">Choose the "padlock" icon next to the value textbox to protect the password.</span></span> 

## <a name="configure-the-azure-resource-group-deployment-task"></a><span data-ttu-id="9783a-195">Az Azure erőforrás-csoport központi telepítésének tevékenység konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9783a-195">Configure the Azure Resource Group Deployment task</span></span>

<span data-ttu-id="9783a-196">A **Azure erőforrás-csoport központi telepítésének** feladattal az üzembe helyezési csoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9783a-196">The **Azure Resource Group Deployment** task is used to create the deployment group.</span></span> <span data-ttu-id="9783a-197">Konfigurálja úgy az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9783a-197">Configure it as follows:</span></span>

* <span data-ttu-id="9783a-198">**Azure-előfizetés:** alatti listából válasszon ki egy kapcsolatot **Azure-szolgáltatás rendelkezésre álló kapcsolatok**.</span><span class="sxs-lookup"><span data-stu-id="9783a-198">**Azure Subscription:** Select a connection from the list under **Available Azure Service Connections**.</span></span> 
  <span data-ttu-id="9783a-199">Ha a kapcsolat nem jelenik meg, válassza a **kezelése**, jelölje be **új szolgáltatási végpont** majd **Azure Resource Manager**, és kövesse a megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="9783a-199">If no connections appear, choose **Manage**, select **New Service Endpoint** then **Azure Resource Manager**, and follow the prompts.</span></span>
  <span data-ttu-id="9783a-200">Térjen vissza a kiadási definícióját, frissítse a **AzureRM előfizetés** listában, és válassza ki a létrehozott kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="9783a-200">Return to your release definition, refresh the **AzureRM Subscription** list, and select the connection you created.</span></span>

* <span data-ttu-id="9783a-201">**Erőforráscsoport**: Adjon meg egy korábban létrehozott erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9783a-201">**Resource group**: Enter a name of the resource group you created earlier.</span></span>

* <span data-ttu-id="9783a-202">**Hely**: Válasszon ki egy régiót a központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="9783a-202">**Location**: Select a region for the deployment.</span></span>

  ![Új erőforráscsoport létrehozása](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* <span data-ttu-id="9783a-204">**A sablon helye**:`URL of the file`</span><span class="sxs-lookup"><span data-stu-id="9783a-204">**Template location**: `URL of the file`</span></span>

* <span data-ttu-id="9783a-205">**Sablon hivatkozás**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.json`</span><span class="sxs-lookup"><span data-stu-id="9783a-205">**Template link**: `{your-git-repo}/ARM-Templates/UbuntuWeb1.json`</span></span>

* <span data-ttu-id="9783a-206">**Sablon paraméterei link**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`</span><span class="sxs-lookup"><span data-stu-id="9783a-206">**Template parameters link**: `{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`</span></span>

* <span data-ttu-id="9783a-207">**Sablon paraméterének**: listáját a felülbírálási értékeket, például: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.</span><span class="sxs-lookup"><span data-stu-id="9783a-207">**Override template parameters**: A list of the override values, for example: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.</span></span><br /><span data-ttu-id="9783a-208">Helyezze be a {helyőrzőket} saját egyedi értékeket.</span><span class="sxs-lookup"><span data-stu-id="9783a-208">Insert your own specific values for the {placeholders}.</span></span> 

* <span data-ttu-id="9783a-209">**Előfeltételek engedélyezése**:`Configure with Deployment Group agent`</span><span class="sxs-lookup"><span data-stu-id="9783a-209">**Enable prerequisites**: `Configure with Deployment Group agent`</span></span>

* <span data-ttu-id="9783a-210">**TFS/VSTS-végpont**: válasszon **Hozzáadás** , és válassza az "Új Team Foundation Server/Team Services-kapcsolat hozzáadása" párbeszédpanelen **jogkivonat-alapú hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="9783a-210">**TFS/VSTS endpoint**: Choose **Add** and, in the "Add new Team Foundation Server/Team Services Connection" dialog, select **Token Based Authentication**.</span></span> <span data-ttu-id="9783a-211">Adja meg a kapcsolat nevét és a csapatprojekt URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="9783a-211">Enter a name of the connection and the URL of your team project.</span></span> <span data-ttu-id="9783a-212">Ezután létre, és adja meg egy [személyes hozzáférési jogkivonat (PAT)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) a csapat projekthez a kapcsolat hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9783a-212">Then generate and enter a [Personal Access Token (PAT)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) to authenticate the connection to your team project.</span></span>

  ![Hozzon létre egy személyes hozzáférési tokent](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* <span data-ttu-id="9783a-214">**Csapatprojekt**: válassza ki az aktuális projektben.</span><span class="sxs-lookup"><span data-stu-id="9783a-214">**Team project**: Select your current project.</span></span>

* <span data-ttu-id="9783a-215">**Üzembe helyezési csoport**: Adja meg a központi telepítési csoport néven közben használatos a **erőforráscsoport** paraméter.</span><span class="sxs-lookup"><span data-stu-id="9783a-215">**Deployment Group**: Enter the same deployment group name as you used for the **Resource group** parameter.</span></span>

<span data-ttu-id="9783a-216">Az alapértelmezett beállításokat az Azure erőforrás-csoport központi telepítésének feladathoz a következők: az erőforrás frissítése és fokozatosan ehhez.</span><span class="sxs-lookup"><span data-stu-id="9783a-216">The default settings for the Azure Resource Group Deployment task are to create or update a resource, and to do so incrementally.</span></span> <span data-ttu-id="9783a-217">A feladat létrehozza a virtuális gépeket először akkor fut, és csak ezt követően frissítse azokat.</span><span class="sxs-lookup"><span data-stu-id="9783a-217">The task creates the VMs the first time it runs, and subsequently just update them.</span></span>

## <a name="configure-the-shell-script-task"></a><span data-ttu-id="9783a-218">A PowerShell parancsfájlhoz tevékenység konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9783a-218">Configure the Shell Script task</span></span>

<span data-ttu-id="9783a-219">A **Héjparancsfájlt** feladattal minden egyes kiszolgálón telepítse a Node.js, és indítsa el az alkalmazást futtatni kívánt parancsfájl konfigurációját adja meg.</span><span class="sxs-lookup"><span data-stu-id="9783a-219">The **Shell Script** task is used to provide the configuration for a script to run on each server to install Node.js and start the app.</span></span> <span data-ttu-id="9783a-220">Konfigurálja úgy az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9783a-220">Configure it as follows:</span></span>

* <span data-ttu-id="9783a-221">**Parancsfájl elérési útja**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`</span><span class="sxs-lookup"><span data-stu-id="9783a-221">**Script Path**: `$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`</span></span>

* <span data-ttu-id="9783a-222">**Adjon meg működő Directory**:`Checked`</span><span class="sxs-lookup"><span data-stu-id="9783a-222">**Specify Working Directory**: `Checked`</span></span>

* <span data-ttu-id="9783a-223">**Munkakönyvtár**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`</span><span class="sxs-lookup"><span data-stu-id="9783a-223">**Working Directory**: `$(System.DefaultWorkingDirectory)/Fabrikam-Node`</span></span>
   
## <a name="rename-and-save-the-release-definition"></a><span data-ttu-id="9783a-224">Nevezze át, és a kiadási definíció mentése</span><span class="sxs-lookup"><span data-stu-id="9783a-224">Rename and save the release definition</span></span>

1. <span data-ttu-id="9783a-225">A kiadási-definíciót a megadott néven nevének módosítása a **utáni műveletek** Jenkins buildverziót lapján.</span><span class="sxs-lookup"><span data-stu-id="9783a-225">Edit the name of the release definition to the name you specified in the **Post-build Actions** tab of the build in Jenkins.</span></span> <span data-ttu-id="9783a-226">Jenkins ezt a nevet fogja tudni indul el egy új kiadásban, ha a forrás-összetevők frissítése szükséges.</span><span class="sxs-lookup"><span data-stu-id="9783a-226">Jenkins requires this name to be able to trigger a new release when the source artifacts are updated.</span></span>

1. <span data-ttu-id="9783a-227">Igény szerint megváltoztathatja a környezet neve a nevére kattintva.</span><span class="sxs-lookup"><span data-stu-id="9783a-227">Optionally, change the name of the environment by clicking on the name.</span></span> 

1. <span data-ttu-id="9783a-228">Válasszon **mentése**, és válassza a **OK**.</span><span class="sxs-lookup"><span data-stu-id="9783a-228">Choose **Save**, and choose **OK**.</span></span>

## <a name="start-a-manual-deployment"></a><span data-ttu-id="9783a-229">Indítsa el a manuális központi telepítése</span><span class="sxs-lookup"><span data-stu-id="9783a-229">Start a manual deployment</span></span>

1. <span data-ttu-id="9783a-230">Válassza ki **+ kiadási** válassza **létrehozása kiadási**.</span><span class="sxs-lookup"><span data-stu-id="9783a-230">Choose **+ Release** and select **Create Release**.</span></span>

1. <span data-ttu-id="9783a-231">A build befejezte a kijelölt legördülő listában jelölje ki, és válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9783a-231">Select the build you completed in the highlighted drop-down list and choose **Create**.</span></span>

1. <span data-ttu-id="9783a-232">Adja meg a kiadási hivatkozás a felbukkanó üzenet.</span><span class="sxs-lookup"><span data-stu-id="9783a-232">Choose the release link in the popup message.</span></span> <span data-ttu-id="9783a-233">Például: "kiadás **Release-1** létrejött."</span><span class="sxs-lookup"><span data-stu-id="9783a-233">For example: "Release **Release-1** has been created."</span></span>

1. <span data-ttu-id="9783a-234">Nyissa meg a **naplók** a kiadás a konzol kimeneti bemutató fülre.</span><span class="sxs-lookup"><span data-stu-id="9783a-234">Open the **Logs** tab to watch the release console output.</span></span>

1. <span data-ttu-id="9783a-235">A böngészőben nyissa meg az URL-CÍMÉT a központi telepítés csoporthoz hozzáadott kiszolgálók egyikét.</span><span class="sxs-lookup"><span data-stu-id="9783a-235">In your browser, open the URL of one of the servers you added to your deployment group.</span></span> <span data-ttu-id="9783a-236">Adja meg például`http://{your-server-ip-address}`</span><span class="sxs-lookup"><span data-stu-id="9783a-236">For example, enter `http://{your-server-ip-address}`</span></span>

## <a name="start-a-cicd-deployment"></a><span data-ttu-id="9783a-237">Indítsa el a CI/CD központi telepítés</span><span class="sxs-lookup"><span data-stu-id="9783a-237">Start a CI/CD deployment</span></span>

1. <span data-ttu-id="9783a-238">A kiadási-definícióban, törölje a jelet a **engedélyezve** jelölőnégyzet a **beállítások** az Azure erőforrás-csoport központi telepítésének feladat beállításai.</span><span class="sxs-lookup"><span data-stu-id="9783a-238">In the release definition, uncheck the **Enabled** checkbox in the **Control Options** section of the settings for the Azure Resource Group Deployment task.</span></span>
   <span data-ttu-id="9783a-239">Jövőbeli központi telepítésére a meglévő üzembe helyezési csoport nem kell újra végrehajtani a feladatot.</span><span class="sxs-lookup"><span data-stu-id="9783a-239">For future deployments to the existing deployment group, you do not need to re-execute this task.</span></span>

1. <span data-ttu-id="9783a-240">Nyissa meg a forrás Git-tárházban, és a tartalmának módosítása a **h1** fejléc a fájlban [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).</span><span class="sxs-lookup"><span data-stu-id="9783a-240">Go to the source Git repository and modify the contents of the **h1** heading in the file [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).</span></span>

1. <span data-ttu-id="9783a-241">A módosítás véglegesítése.</span><span class="sxs-lookup"><span data-stu-id="9783a-241">Commit your change.</span></span>

1. <span data-ttu-id="9783a-242">Néhány perc elteltével látni fogja a létrehozott új verziót a **kiadásokban** Team Services vagy a TFS oldalán.</span><span class="sxs-lookup"><span data-stu-id="9783a-242">After a few minutes, you will see a new release created in the **Releases** page of Team Services or TFS.</span></span> <span data-ttu-id="9783a-243">Nyissa meg a verzió: a központi telepítése folyamatban.</span><span class="sxs-lookup"><span data-stu-id="9783a-243">Open the release to see the deployment taking place.</span></span> <span data-ttu-id="9783a-244">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="9783a-244">Congratulations!</span></span>

## <a name="next-steps"></a><span data-ttu-id="9783a-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9783a-245">Next Steps</span></span>

<span data-ttu-id="9783a-246">Ebben az oktatóanyagban az Azure-ban Jenkins build és a Team Services, a kiadáshoz alkalmazások központi telepítése automatikus.</span><span class="sxs-lookup"><span data-stu-id="9783a-246">In this tutorial, you automated the deployment of an app to Azure using Jenkins build and Team Services for release.</span></span> <span data-ttu-id="9783a-247">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="9783a-247">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9783a-248">Az alkalmazás a Jenkins elkészítésére</span><span class="sxs-lookup"><span data-stu-id="9783a-248">Build your app in Jenkins</span></span>
> * <span data-ttu-id="9783a-249">Jenkins Team Services-integráció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9783a-249">Configure Jenkins for Team Services integration</span></span>
> * <span data-ttu-id="9783a-250">Az Azure virtuális gépek telepítési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="9783a-250">Create a deployment group for the Azure virtual machines</span></span>
> * <span data-ttu-id="9783a-251">Hozzon létre olyan kiadási-definícióval, konfigurálja a virtuális gépeket, és telepíti az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="9783a-251">Create a release definition that configures the VMs and deploys the app</span></span>

<span data-ttu-id="9783a-252">A következő oktatóanyaghoz egy LÁMPA telepítésével kapcsolatos további előzetes (Linux, Apache, MySQL és a PHP) verem.</span><span class="sxs-lookup"><span data-stu-id="9783a-252">Advance to the next tutorial to learn more about how to deploy a LAMP (Linux, Apache, MySQL, and PHP) stack.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9783a-253">LAMP stack telepítése</span><span class="sxs-lookup"><span data-stu-id="9783a-253">Deploy LAMP stack</span></span>](tutorial-lamp-stack.md)