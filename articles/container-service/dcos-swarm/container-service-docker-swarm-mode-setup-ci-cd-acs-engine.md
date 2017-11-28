---
title: "az Azure-tároló szolgáltatás vezérlőprogramja és Swarm mód aaaCI/CD |} Microsoft Docs"
description: "Azure-tároló szolgáltatás vezérlőprogramja használata a Docker Swarm-módban, egy Azure-tároló beállításjegyzék és a Visual Studio Team Services toodeliver folyamatosan egy több tároló .NET Core-alkalmazás"
services: container-service
documentationcenter: " "
author: diegomrtnzg
manager: esterdnb
tags: acs, azure-container-service, acs-engine
keywords: "Swarm-docker, tárolók, Micro-szolgáltatások, Azure, a Visual Studio Team Services, a DevOps, ACS-motor"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2017
ms.author: diegomrtnzg
ms.custom: mvc
ms.openlocfilehash: 040522c452f7ea0ce3c92f2fe57b1c141b97e380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="full-cicd-pipeline-toodeploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-visual-studio-team-services"></a><span data-ttu-id="52144-104">Teljes CI/CD-feldolgozási folyamat toodeploy Azure tárolószolgáltatás egy több tároló alkalmazást az ACS-motor és a Docker Swarm mód Visual Studio Team Services használatával</span><span class="sxs-lookup"><span data-stu-id="52144-104">Full CI/CD pipeline toodeploy a multi-container application on Azure Container Service with ACS Engine and Docker Swarm Mode using Visual Studio Team Services</span></span>

<span data-ttu-id="52144-105">*Ez a cikk alapján [teljes CI/CD-feldolgozási folyamat toodeploy egy Azure tárolószolgáltatás a Docker Swarmmal Visual Studio Team Services használatával több tároló alkalmazás](container-service-docker-swarm-setup-ci-cd.md) dokumentáció*</span><span class="sxs-lookup"><span data-stu-id="52144-105">*This article is based on [Full CI/CD pipeline toodeploy a multi-container application on Azure Container Service with Docker Swarm using Visual Studio Team Services](container-service-docker-swarm-setup-ci-cd.md) documentation*</span></span>

<span data-ttu-id="52144-106">Napjainkban egyik hello legnagyobb kihívást képes toodeliver alatt hello felhő modern alkalmazások fejlesztése során ezeknek az alkalmazásoknak folyamatosan.</span><span class="sxs-lookup"><span data-stu-id="52144-106">Nowadays, One of hello biggest challenges when developing modern applications for hello cloud is being able toodeliver these applications continuously.</span></span> <span data-ttu-id="52144-107">Ebből a cikkből megismerheti, hogyan tooimplement egy teljes folyamatos integrációt és a központi telepítés (CI/CD) a következő feldolgozási sorban használatával:</span><span class="sxs-lookup"><span data-stu-id="52144-107">In this article, you learn how tooimplement a full continuous integration and deployment (CI/CD) pipeline using:</span></span> 
* <span data-ttu-id="52144-108">Azure-tárolót szolgáltatás motorja a Docker Swarm mód</span><span class="sxs-lookup"><span data-stu-id="52144-108">Azure Container Service Engine with Docker Swarm Mode</span></span>
* <span data-ttu-id="52144-109">Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="52144-109">Azure Container Registry</span></span>
* <span data-ttu-id="52144-110">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="52144-110">Visual Studio Team Services</span></span>

<span data-ttu-id="52144-111">Ez a cikk alapján egy egyszerű alkalmazást, a rendelkezésre álló [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), az ASP.NET Core fejlett.</span><span class="sxs-lookup"><span data-stu-id="52144-111">This article is based on a simple application, available on [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), developed with ASP.NET Core.</span></span> <span data-ttu-id="52144-112">négy különböző szolgáltatások hello alkalmazások állnak: három webes API-k és egy webes előtér:</span><span class="sxs-lookup"><span data-stu-id="52144-112">hello application is composed of four different services: three web APIs and one web front end:</span></span>

![MyShop mintaalkalmazás](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

<span data-ttu-id="52144-114">hello cél toodeliver van az alkalmazás folyamatosan fürtben Docker Swarm-módban, a Visual Studio Team Services használatával.</span><span class="sxs-lookup"><span data-stu-id="52144-114">hello objective is toodeliver this application continuously in a Docker Swarm Mode cluster, using Visual Studio Team Services.</span></span> <span data-ttu-id="52144-115">hello következő részletek az folyamatos kézbesítési adatcsatornát. ábra:</span><span class="sxs-lookup"><span data-stu-id="52144-115">hello following figure details this continuous delivery pipeline:</span></span>

![MyShop mintaalkalmazás](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

<span data-ttu-id="52144-117">Hello lépéseket rövid leírása itt található:</span><span class="sxs-lookup"><span data-stu-id="52144-117">Here is a brief explanation of hello steps:</span></span>

1. <span data-ttu-id="52144-118">Kód változtatások véglegesített toohello forráskódraktárban (Itt GitHub)</span><span class="sxs-lookup"><span data-stu-id="52144-118">Code changes are committed toohello source code repository (here, GitHub)</span></span> 
2. <span data-ttu-id="52144-119">GitHub elindítja a Visual Studio Team Services build</span><span class="sxs-lookup"><span data-stu-id="52144-119">GitHub triggers a build in Visual Studio Team Services</span></span> 
3. <span data-ttu-id="52144-120">A Visual Studio Team Services lekérdezi hello hello források legújabb verzióját, és hello alkalmazást alkotó összes hello-lemezképet állít össze</span><span class="sxs-lookup"><span data-stu-id="52144-120">Visual Studio Team Services gets hello latest version of hello sources and builds all hello images that compose hello application</span></span> 
4. <span data-ttu-id="52144-121">A Visual Studio Team Services minden hello Azure tároló beállításjegyzék szolgáltatás használatával létrehozott lemezképet tooa Docker beállításjegyzék leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="52144-121">Visual Studio Team Services pushes each image tooa Docker registry created using hello Azure Container Registry service</span></span> 
5. <span data-ttu-id="52144-122">A Visual Studio Team Services új verziót váltja ki.</span><span class="sxs-lookup"><span data-stu-id="52144-122">Visual Studio Team Services triggers a new release</span></span> 
6. <span data-ttu-id="52144-123">hello kiadás néhány SSH használatával fürtcsomóponton hello Azure tároló szolgáltatás fő parancsokat futtat</span><span class="sxs-lookup"><span data-stu-id="52144-123">hello release runs some commands using SSH on hello Azure container service cluster master node</span></span> 
7. <span data-ttu-id="52144-124">Docker Swarm mód hello fürtön hello hello képek legújabb verzióját kéri le.</span><span class="sxs-lookup"><span data-stu-id="52144-124">Docker Swarm Mode on hello cluster pulls hello latest version of hello images</span></span> 
8. <span data-ttu-id="52144-125">hello alkalmazás új verziójának hello Docker verem használatával van telepítve</span><span class="sxs-lookup"><span data-stu-id="52144-125">hello new version of hello application is deployed using Docker Stack</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="52144-126">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="52144-126">Prerequisites</span></span>

<span data-ttu-id="52144-127">Az oktatóanyag elindítása előtt kell toocomplete hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="52144-127">Before starting this tutorial, you need toocomplete hello following tasks:</span></span>

- [<span data-ttu-id="52144-128">Az Azure Tárolószolgáltatásban ACS motorral mód Swarm-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="52144-128">Create a Swarm Mode cluster in Azure Container Service with ACS Engine</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [<span data-ttu-id="52144-129">Csatlakozás az Azure Tárolószolgáltatásban hello Swarm-fürthöz</span><span class="sxs-lookup"><span data-stu-id="52144-129">Connect with hello Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)
- [<span data-ttu-id="52144-130">Hozzon létre egy Azure-tárolót beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="52144-130">Create an Azure container registry</span></span>](../../container-registry/container-registry-get-started-portal.md)
- [<span data-ttu-id="52144-131">Egy Visual Studio Team Services fiók és a team projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="52144-131">Have a Visual Studio Team Services account and team project created</span></span>](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [<span data-ttu-id="52144-132">Elágazás hello GitHub tárház tooyour GitHub-fiók</span><span class="sxs-lookup"><span data-stu-id="52144-132">Fork hello GitHub repository tooyour GitHub account</span></span>](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> <span data-ttu-id="52144-133">az Azure Tárolószolgáltatásban hello Docker Swarm orchestrator örökölt önálló Swarm használja.</span><span class="sxs-lookup"><span data-stu-id="52144-133">hello Docker Swarm orchestrator in Azure Container Service uses legacy standalone Swarm.</span></span> <span data-ttu-id="52144-134">Jelenleg az integrált hello [Swarm mód](https://docs.docker.com/engine/swarm/) (Docker 1.12 és újabb) értéke nem egy támogatott orchestrator, az Azure Tárolószolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="52144-134">Currently, hello integrated [Swarm mode](https://docs.docker.com/engine/swarm/) (in Docker 1.12 and higher) is not a supported orchestrator in Azure Container Service.</span></span> <span data-ttu-id="52144-135">Emiatt használjuk [ACS motor](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), közösségi hozzájárult [gyorsindítási sablonon](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), vagy egy Docker-megoldás a hello [Azure piactér](https://azuremarketplace.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="52144-135">For this reason, we are using [ACS Engine](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), a community-contributed [quickstart template](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), or a Docker solution in hello [Azure Marketplace](https://azuremarketplace.microsoft.com).</span></span>
>

## <a name="step-1-configure-your-visual-studio-team-services-account"></a><span data-ttu-id="52144-136">1. lépés: A Visual Studio Team Services-fiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="52144-136">Step 1: Configure your Visual Studio Team Services account</span></span> 

<span data-ttu-id="52144-137">Ebben a szakaszban konfigurálhatja a Visual Studio Team Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="52144-137">In this section, you configure your Visual Studio Team Services account.</span></span> <span data-ttu-id="52144-138">tooconfigure VSTS Services végpontjainak, a Visual Studio Team Services projektben kattintson hello **beállítások** ikonra hello eszköztár, és válassza ki a **szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="52144-138">tooconfigure VSTS Services Endpoints, in your Visual Studio Team Services project, click hello **Settings** icon in hello toolbar, and select **Services**.</span></span>

![Nyissa meg a szolgáltatások végpont](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-visual-studio-team-services-and-azure-account"></a><span data-ttu-id="52144-140">Csatlakozás a Visual Studio Team Services és az Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="52144-140">Connect Visual Studio Team Services and Azure account</span></span>

<span data-ttu-id="52144-141">Állítsa be a kapcsolatot a VSTS-projektet és az Azure-fiókjával között.</span><span class="sxs-lookup"><span data-stu-id="52144-141">Set up a connection between your VSTS project and your Azure account.</span></span>

1. <span data-ttu-id="52144-142">Hello bal oldalon kattintson **új szolgáltatási végpont** > **Azure Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="52144-142">On hello left, click **New Service Endpoint** > **Azure Resource Manager**.</span></span>
2. <span data-ttu-id="52144-143">tooauthorize VSTS toowork az Azure-fiókjával, és válassza ki a **előfizetés** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="52144-143">tooauthorize VSTS toowork with your Azure account, select your **Subscription** and click **OK**.</span></span>

    ![A Visual Studio Team Services - Azure engedélyezése](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-visual-studio-team-services-and-github"></a><span data-ttu-id="52144-145">Csatlakoztassa a Visual Studio Team Services, GitHub</span><span class="sxs-lookup"><span data-stu-id="52144-145">Connect Visual Studio Team Services and GitHub</span></span>

<span data-ttu-id="52144-146">Állítsa be a kapcsolatot a VSTS-projektet és a GitHub-fiók között.</span><span class="sxs-lookup"><span data-stu-id="52144-146">Set up a connection between your VSTS project and your GitHub account.</span></span>

1. <span data-ttu-id="52144-147">Hello bal oldalon kattintson **új szolgáltatási végpont** > **GitHub**.</span><span class="sxs-lookup"><span data-stu-id="52144-147">On hello left, click **New Service Endpoint** > **GitHub**.</span></span>
2. <span data-ttu-id="52144-148">a GitHub-fiókjában, tooauthorize VSTS toowork kattintson **engedélyezés** és hello eljárás a hello ablakban.</span><span class="sxs-lookup"><span data-stu-id="52144-148">tooauthorize VSTS toowork with your GitHub account, click **Authorize** and follow hello procedure in hello window that opens.</span></span>

    ![A Visual Studio Team Services - engedélyezik a Githubon](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-vsts-tooyour-azure-container-service-cluster"></a><span data-ttu-id="52144-150">Csatlakozás VSTS tooyour Azure Tárolószolgáltatás-fürt</span><span class="sxs-lookup"><span data-stu-id="52144-150">Connect VSTS tooyour Azure Container Service cluster</span></span>

<span data-ttu-id="52144-151">hello utolsó be hello CI/CD adatcsatorna elérése előtt lépésekre tooconfigure külső kapcsolatok tooyour Docker Swarm-fürt az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="52144-151">hello last steps before getting into hello CI/CD pipeline are tooconfigure external connections tooyour Docker Swarm cluster in Azure.</span></span> 

1. <span data-ttu-id="52144-152">Hello Docker Swarm-fürt, a típusú végpont hozzáadása **SSH**.</span><span class="sxs-lookup"><span data-stu-id="52144-152">For hello Docker Swarm cluster, add an endpoint of type **SSH**.</span></span> <span data-ttu-id="52144-153">Majd adja meg a hello SSH csatlakozási adatait a Swarm-fürt (főcsomópont).</span><span class="sxs-lookup"><span data-stu-id="52144-153">Then enter hello SSH connection information of your Swarm cluster (master node).</span></span>

    ![A Visual Studio Team Services - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

<span data-ttu-id="52144-155">Minden hello konfigurálást most.</span><span class="sxs-lookup"><span data-stu-id="52144-155">All hello configuration is done now.</span></span> <span data-ttu-id="52144-156">A következő lépések hello hello CI/CD-feldolgozási folyamat létrehozza és telepíti hello alkalmazás toohello Docker Swarm-fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="52144-156">In hello next steps, you create hello CI/CD pipeline that builds and deploys hello application toohello Docker Swarm cluster.</span></span> 

## <a name="step-2-create-hello-build-definition"></a><span data-ttu-id="52144-157">2. lépés: Hello build definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="52144-157">Step 2: Create hello build definition</span></span>

<span data-ttu-id="52144-158">Ebben a lépésben a VSTS project build definícióját beállítása és hello létrehozási munkafolyamat megadása a tároló lemezképek</span><span class="sxs-lookup"><span data-stu-id="52144-158">In this step, you set up a build definition for your VSTS project and define hello build workflow for your container images</span></span>

### <a name="initial-definition-setup"></a><span data-ttu-id="52144-159">Kezdeti beállításának</span><span class="sxs-lookup"><span data-stu-id="52144-159">Initial definition setup</span></span>

1. <span data-ttu-id="52144-160">toocreate build definícióját, tooyour Visual Studio Team Services projektre, és kattintson a csatlakozás **Build & kiadási**.</span><span class="sxs-lookup"><span data-stu-id="52144-160">toocreate a build definition, connect tooyour Visual Studio Team Services project and click **Build & Release**.</span></span> <span data-ttu-id="52144-161">A hello **definíciók Build** kattintson **+ új**.</span><span class="sxs-lookup"><span data-stu-id="52144-161">In hello **Build definitions** section, click **+ New**.</span></span> 

    ![A Visual Studio Team Services - új definíció létrehozása](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. <span data-ttu-id="52144-163">Jelölje be hello **üres folyamat**.</span><span class="sxs-lookup"><span data-stu-id="52144-163">Select hello **Empty process**.</span></span>

    ![A Visual Studio Team Services - új üres Build definíció](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. <span data-ttu-id="52144-165">Kattintson a hello **változók** lapra, és hozzon létre két új változót: **RegistryURL** és **AgentURL**.</span><span class="sxs-lookup"><span data-stu-id="52144-165">Then, click hello **Variables** tab and create two new variables: **RegistryURL** and **AgentURL**.</span></span> <span data-ttu-id="52144-166">Illessze be a beállításjegyzék és a fürt ügynökök DNS hello értékének.</span><span class="sxs-lookup"><span data-stu-id="52144-166">Paste hello values of your Registry and Cluster Agents DNS.</span></span>

    ![A Visual Studio Team Services - Build változók konfigurációját](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. <span data-ttu-id="52144-168">A hello **Build definíciók** lap, nyissa meg hello **eseményindítók** lapra, és hello build toouse folyamatos integrációt hello elágazás hello MyShop projekt a hello Előfeltételek konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="52144-168">On hello **Build Definitions** page, open hello **Triggers** tab and configure hello build toouse continuous integration with hello fork of hello MyShop project that you created in hello prerequisites.</span></span> <span data-ttu-id="52144-169">Ezt követően válassza **módosítások kötegelt**.</span><span class="sxs-lookup"><span data-stu-id="52144-169">Then, select **Batch changes**.</span></span> <span data-ttu-id="52144-170">Győződjön meg arról, hogy kiválassza *docker-linux* , hello **specification fiókirodai**.</span><span class="sxs-lookup"><span data-stu-id="52144-170">Make sure that you select *docker-linux* as hello **Branch specification**.</span></span>

    ![Visual Studio Team Services - Build tárház konfigurációja](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. <span data-ttu-id="52144-172">Végül kattintson a hello **beállítások** lapra, és konfigurálja a hello alapértelmezett ügynök várólista túl**üzemeltetett Linux előzetes**.</span><span class="sxs-lookup"><span data-stu-id="52144-172">Finally, click hello **Options** tab and configure hello Default agent queue too**Hosted Linux Preview**.</span></span>

    ![A Visual Studio Team Services - Gazdagépügynök-konfigurálási](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-hello-build-workflow"></a><span data-ttu-id="52144-174">Adja meg a hello létrehozási munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="52144-174">Define hello build workflow</span></span>
<span data-ttu-id="52144-175">hello lépések hello létrehozási munkafolyamat határozza meg.</span><span class="sxs-lookup"><span data-stu-id="52144-175">hello next steps define hello build workflow.</span></span> <span data-ttu-id="52144-176">Először hello kód tooconfigure hello forrását.</span><span class="sxs-lookup"><span data-stu-id="52144-176">First, you need tooconfigure hello source of hello code.</span></span> <span data-ttu-id="52144-177">toodo, jelölje be **GitHub** és a **tárház** és **fiókirodai** (docker-linux).</span><span class="sxs-lookup"><span data-stu-id="52144-177">toodo it, select **GitHub** and your **repository** and **branch** (docker-linux).</span></span>

![Konfigurálja a Visual Studio Team Services - kód forrása](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

<span data-ttu-id="52144-179">Nincsenek hello öt tároló képek toobuild *MyShop* alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="52144-179">There are five container images toobuild for hello *MyShop* application.</span></span> <span data-ttu-id="52144-180">Minden egyes lemezképének összeállítása a hello projekt mappákban lévő Dockerfile hello használata:</span><span class="sxs-lookup"><span data-stu-id="52144-180">Each image is built using hello Dockerfile located in hello project folders:</span></span>

* <span data-ttu-id="52144-181">ProductsApi</span><span class="sxs-lookup"><span data-stu-id="52144-181">ProductsApi</span></span>
* <span data-ttu-id="52144-182">Proxy</span><span class="sxs-lookup"><span data-stu-id="52144-182">Proxy</span></span>
* <span data-ttu-id="52144-183">RatingsApi</span><span class="sxs-lookup"><span data-stu-id="52144-183">RatingsApi</span></span>
* <span data-ttu-id="52144-184">RecommandationsApi</span><span class="sxs-lookup"><span data-stu-id="52144-184">RecommandationsApi</span></span>
* <span data-ttu-id="52144-185">ShopFront</span><span class="sxs-lookup"><span data-stu-id="52144-185">ShopFront</span></span>

<span data-ttu-id="52144-186">Az egyes lemezképek két Docker lépéseket, egy toobuild hello lemezképét és egy toopush hello kép hello Azure tároló beállításjegyzék van szüksége.</span><span class="sxs-lookup"><span data-stu-id="52144-186">You need two Docker steps for each image, one toobuild hello image, and one toopush hello image in hello Azure container registry.</span></span> 

1. <span data-ttu-id="52144-187">tooadd hello létrehozási munkafolyamat, egy lépésben kattintson **+ Hozzáadás összeállítása lépés** válassza **Docker**.</span><span class="sxs-lookup"><span data-stu-id="52144-187">tooadd a step in hello build workflow, click **+ Add build step** and select **Docker**.</span></span>

    ![A Visual Studio Team Services - Build lépéseket adhat hozzá.](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. <span data-ttu-id="52144-189">Az egyes lemezképek konfigurálása egy lépésben hello használó `docker build` parancsot.</span><span class="sxs-lookup"><span data-stu-id="52144-189">For each image, configure one step that uses hello `docker build` command.</span></span>

    ![A Visual Studio Team Services - Docker-Build](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    <span data-ttu-id="52144-191">Hello művelet létrehozni, jelölje be az Azure-tároló beállításjegyzék hello **lemezképet készít** művelet, és egyes lemezképek definiáló Dockerfile hello.</span><span class="sxs-lookup"><span data-stu-id="52144-191">For hello build operation, select your Azure Container Registry, hello **Build an image** action, and hello Dockerfile that defines each image.</span></span> <span data-ttu-id="52144-192">Set hello **munkakönyvtár** hello Dockerfile gyökérkönyvtár, adja meg a hello **kép neve**, és válassza ki **legújabb címkét tartalmaz**.</span><span class="sxs-lookup"><span data-stu-id="52144-192">Set hello **Working Directory** as hello Dockerfile root directory, define hello **Image Name**, and select **Include Latest Tag**.</span></span>
    
    <span data-ttu-id="52144-193">hello Lemezképnév toobe rendelkezik a következő formátumban: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```.</span><span class="sxs-lookup"><span data-stu-id="52144-193">hello Image Name has toobe in this format: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```.</span></span> <span data-ttu-id="52144-194">Cserélje le **[NAME]** hello kép nevű:</span><span class="sxs-lookup"><span data-stu-id="52144-194">Replace **[NAME]** with hello image name:</span></span>
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. <span data-ttu-id="52144-195">Az egyes lemezképek beállítása egy második lépésben hello használó `docker push` parancsot.</span><span class="sxs-lookup"><span data-stu-id="52144-195">For each image, configure a second step that uses hello `docker push` command.</span></span>

    ![A Visual Studio Team Services - Docker leküldéses](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    <span data-ttu-id="52144-197">Hello leküldéses művelethez, válassza ki az Azure-tárolót beállításjegyzék hello **lemezkép leküldéses** művelet, adja meg a hello **kép neve** , amely hello előző lépést, és válassza ki a beépített **legújabb címke közé tartozik** .</span><span class="sxs-lookup"><span data-stu-id="52144-197">For hello push operation, select your Azure container registry, hello **Push an image** action, enter hello **Image Name** that is built in hello previous step and select **Include Latest Tag**.</span></span>

4. <span data-ttu-id="52144-198">Hello build konfigurálását követően és leküldéses lépéseket az egyes hello öt képek, adja hozzá a hello három további lépéseket munkafolyamat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="52144-198">After you configure hello build and push steps for each of hello five images, add three more steps in hello build workflow.</span></span>

   ![A Visual Studio Team Services - parancssori feladat hozzáadása](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

      1. <span data-ttu-id="52144-200">A parancssori feladatot, amely használja a bash parancsfájlok tooreplace hello *RegistryURL* hello docker-compose.yml fájlt hello RegistryURL változóval előfordulása.</span><span class="sxs-lookup"><span data-stu-id="52144-200">A command-line task that uses a bash script tooreplace hello *RegistryURL* occurrence in hello docker-compose.yml file with hello RegistryURL variable.</span></span> 
    
          ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

          ![Visual Studio Team Services - frissítés Compose fájl beállításjegyzék URL-címe](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

      2. <span data-ttu-id="52144-202">A parancssori feladatot, amely használja a bash parancsfájlok tooreplace hello *AgentURL* hello docker-compose.yml fájlt hello AgentURL változóval előfordulása.</span><span class="sxs-lookup"><span data-stu-id="52144-202">A command-line task that uses a bash script tooreplace hello *AgentURL* occurrence in hello docker-compose.yml file with hello AgentURL variable.</span></span>
  
          ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

     3. <span data-ttu-id="52144-203">Egy feladatot, amely hello esik Compose fájl frissíteni, mivel a egy összeállítási összetevő így hello verzióban is használható.</span><span class="sxs-lookup"><span data-stu-id="52144-203">A task that drops hello updated Compose file as a build artifact so it can be used in hello release.</span></span> <span data-ttu-id="52144-204">Tekintse meg a következő képernyő Részletek hello.</span><span class="sxs-lookup"><span data-stu-id="52144-204">See hello following screen for details.</span></span>

         ![A Visual Studio Team Services - összetevő közzététele](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

         ![A Visual Studio Team Services - Compose közzététele fájl](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. <span data-ttu-id="52144-207">Kattintson a **mentés és a feldolgozási sor** tootest a build definícióját.</span><span class="sxs-lookup"><span data-stu-id="52144-207">Click **Save & queue** tootest your build definition.</span></span>

   ![Visual Studio Team Services - mentés- és várólista](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![A Visual Studio Team Services - új sor](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. <span data-ttu-id="52144-210">Ha hello **Build** helyes-e, rendelkezik toosee ezen a képernyőn látható:</span><span class="sxs-lookup"><span data-stu-id="52144-210">If hello **Build** is correct, you have toosee this screen:</span></span>

  ![A Visual Studio Team Services - Build sikeres volt.](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-hello-release-definition"></a><span data-ttu-id="52144-212">3. lépés: Hello kiadás definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="52144-212">Step 3: Create hello release definition</span></span>

<span data-ttu-id="52144-213">A Visual Studio Team Services lehetővé teszi túl[kiadásokban kezelése környezetek között](https://www.visualstudio.com/team-services/release-management/).</span><span class="sxs-lookup"><span data-stu-id="52144-213">Visual Studio Team Services allows you too[manage releases across environments](https://www.visualstudio.com/team-services/release-management/).</span></span> <span data-ttu-id="52144-214">Folyamatos üzembe helyezés toomake meg arról, hogy az alkalmazás telepítve van a különböző környezetekben (például a fejlesztői, tesztelési, éles üzem előtti és éles) zökkenőmentes úgy is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="52144-214">You can enable continuous deployment toomake sure that your application is deployed on your different environments (such as dev, test, pre-production, and production) in a smooth way.</span></span> <span data-ttu-id="52144-215">Létrehozhat egy olyan környezetben, az Azure tároló szolgáltatás Docker Swarm mód fürt jelöli.</span><span class="sxs-lookup"><span data-stu-id="52144-215">You can create an environment that represents your Azure Container Service Docker Swarm Mode cluster.</span></span>

![A Visual Studio Team Services - kiadás tooACS](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a><span data-ttu-id="52144-217">Eredeti kiadásának beállítása</span><span class="sxs-lookup"><span data-stu-id="52144-217">Initial release setup</span></span>

1. <span data-ttu-id="52144-218">toocreate kiadás definícióját, kattintson a **kiadásokban** > **+ kiadás**</span><span class="sxs-lookup"><span data-stu-id="52144-218">toocreate a release definition, click **Releases** > **+ Release**</span></span>

2. <span data-ttu-id="52144-219">tooconfigure hello összetevő forrás, kattintson **összetevők** > **hivatkozás egy összetevő forrás**.</span><span class="sxs-lookup"><span data-stu-id="52144-219">tooconfigure hello artifact source, Click **Artifacts** > **Link an artifact source**.</span></span> <span data-ttu-id="52144-220">Itt csatolja az új kiadási definition toohello build hello előző lépésben meghatározott.</span><span class="sxs-lookup"><span data-stu-id="52144-220">Here, link this new release definition toohello build that you defined in hello previous step.</span></span> <span data-ttu-id="52144-221">Ezt követően hello docker-compose.yml fájlt érhető hello kiadás során.</span><span class="sxs-lookup"><span data-stu-id="52144-221">After that, hello docker-compose.yml file is available in hello release process.</span></span>

    ![A Visual Studio Team Services - kiadás összetevők](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. <span data-ttu-id="52144-223">tooconfigure hello kiadás eseményindító, kattintson a **eseményindítók** válassza **folyamatos üzembe helyezés**.</span><span class="sxs-lookup"><span data-stu-id="52144-223">tooconfigure hello release trigger, click **Triggers** and select **Continuous Deployment**.</span></span> <span data-ttu-id="52144-224">Hello beállítása hello eseményindítón ugyanazon összetevő-forrás.</span><span class="sxs-lookup"><span data-stu-id="52144-224">Set hello trigger on hello same artifact source.</span></span> <span data-ttu-id="52144-225">Ez a beállítás biztosítja, hogy egy új kiadási kezdődik, amikor hello build sikeresen befejeződik.</span><span class="sxs-lookup"><span data-stu-id="52144-225">This setting ensures that a new release starts when hello build completes successfully.</span></span>

    ![A Visual Studio Team Services - kiadás eseményindítók](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. <span data-ttu-id="52144-227">tooconfigure hello kiadás változók, kattintson a **változók** válassza **+ változó** toocreate három új változók hello beállításjegyzék hello információval: **docker.username**, **docker.password**, és **docker.registry**.</span><span class="sxs-lookup"><span data-stu-id="52144-227">tooconfigure hello release variables, click **Variables** and select **+Variable** toocreate three new variables with hello info of hello registry: **docker.username**, **docker.password**, and **docker.registry**.</span></span> <span data-ttu-id="52144-228">Illessze be a beállításjegyzék és a fürt ügynökök DNS hello értékének.</span><span class="sxs-lookup"><span data-stu-id="52144-228">Paste hello values of your Registry and Cluster Agents DNS.</span></span>

    ![Visual Studio Team Services - Build tárház konfigurációja](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > <span data-ttu-id="52144-230">Amint az előző üdvözlő képernyőt, kattintson a hello **zárolási** docker.password jelölőnégyzet.</span><span class="sxs-lookup"><span data-stu-id="52144-230">As shown on hello previous screen, click hello **Lock** checkbox in docker.password.</span></span> <span data-ttu-id="52144-231">Ez a beállítás az fontos toorestrict hello jelszó.</span><span class="sxs-lookup"><span data-stu-id="52144-231">This setting is important toorestrict hello password.</span></span>
    >

### <a name="define-hello-release-workflow"></a><span data-ttu-id="52144-232">Adja meg a hello kiadás munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="52144-232">Define hello release workflow</span></span>

<span data-ttu-id="52144-233">hello kiadás munkafolyamat két feladatot hozzáadott tevődik össze.</span><span class="sxs-lookup"><span data-stu-id="52144-233">hello release workflow is composed of two tasks that you add.</span></span>

1. <span data-ttu-id="52144-234">Egy feladat toosecurely másolási hello konfigurálása állítható össze a fájl tooa *telepítése* mappa a hello Docker Swarm fő csomóponton korábban konfigurált hello SSH-kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="52144-234">Configure a task toosecurely copy hello compose file tooa *deploy* folder on hello Docker Swarm master node, using hello SSH connection you configured previously.</span></span> <span data-ttu-id="52144-235">Tekintse meg a következő képernyő Részletek hello.</span><span class="sxs-lookup"><span data-stu-id="52144-235">See hello following screen for details.</span></span>
    
    <span data-ttu-id="52144-236">Forrásmappa:```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```</span><span class="sxs-lookup"><span data-stu-id="52144-236">Source folder: ```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```</span></span>

    ![Visual Studio Team Services - kiadás SCP-je](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. <span data-ttu-id="52144-238">Egy második feladat tooexecute a bash parancs toorun konfigurálása `docker` és `docker stack deploy` parancsok hello fő csomóponton.</span><span class="sxs-lookup"><span data-stu-id="52144-238">Configure a second task tooexecute a bash command toorun `docker` and `docker stack deploy` commands on hello master node.</span></span> <span data-ttu-id="52144-239">Tekintse meg a következő képernyő Részletek hello.</span><span class="sxs-lookup"><span data-stu-id="52144-239">See hello following screen for details.</span></span>

    ```docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth```

    ![A Visual Studio Team Services - kiadás Bash](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    <span data-ttu-id="52144-241">hello fő végrehajtott hello parancs hello Docker parancssori felületén és hello Docker Compose CLI toodo hello feladatok a következő használja:</span><span class="sxs-lookup"><span data-stu-id="52144-241">hello command executed on hello master uses hello Docker CLI and hello Docker-Compose CLI toodo hello following tasks:</span></span>

    - <span data-ttu-id="52144-242">Jelentkezzen be Azure-tárolót beállításjegyzék toohello (hello definiált három build-változókat használ **változók** lapon)</span><span class="sxs-lookup"><span data-stu-id="52144-242">Log in toohello Azure container registry (it uses three build variables that are defined in hello **Variables** tab)</span></span>
    - <span data-ttu-id="52144-243">Adja meg a hello **DOCKER_HOST** hello Swarm végponthoz változó toowork (: 2375)</span><span class="sxs-lookup"><span data-stu-id="52144-243">Define hello **DOCKER_HOST** variable toowork with hello Swarm endpoint (:2375)</span></span>
    - <span data-ttu-id="52144-244">Keresse meg a toohello *telepítése* , amelyek biztonságos másolási feladat megelőző hello hozta létre, és hello docker-compose.yml fájlt tartalmazó mappa</span><span class="sxs-lookup"><span data-stu-id="52144-244">Navigate toohello *deploy* folder that was created by hello preceding secure copy task and that contains hello docker-compose.yml file</span></span> 
    - <span data-ttu-id="52144-245">Végrehajtás `docker stack deploy` parancsokat, amelyek hello új képek lekéréses és hello tárolók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="52144-245">Execute `docker stack deploy` commands that pull hello new images and create hello containers.</span></span>

    >[!IMPORTANT]
    > <span data-ttu-id="52144-246">Amint az előző üdvözlő képernyőt, hagyja hello **stderr-en sikertelen** jelölőnégyzet nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="52144-246">As shown on hello previous screen, leave hello **Fail on STDERR** checkbox unchecked.</span></span> <span data-ttu-id="52144-247">Ez a beállítás lehetővé teszi toocomplete hello kiadás folyamat miatt túl`docker-compose` több diagnosztikai üzeneteket, jelenít meg, például a tárolók leállítása vagy törlése, a standard hiba kimeneti hello.</span><span class="sxs-lookup"><span data-stu-id="52144-247">This setting allows us toocomplete hello release process due too`docker-compose` prints several diagnostic messages, such as containers are stopping or being deleted, on hello standard error output.</span></span> <span data-ttu-id="52144-248">Ha hello jelölőnégyzetet a Visual Studio Team Services jelenti, hogy hello kiadás során hibák jelentkeztek akkor is, ha minden megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="52144-248">If you check hello checkbox, Visual Studio Team Services reports that errors occurred during hello release, even if all goes well.</span></span>
    >
3. <span data-ttu-id="52144-249">Mentse az új kiadási definíciója.</span><span class="sxs-lookup"><span data-stu-id="52144-249">Save this new release definition.</span></span>

## <a name="step-4-test-hello-cicd-pipeline"></a><span data-ttu-id="52144-250">4. lépés: Hello CI/CD folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="52144-250">Step 4: Test hello CI/CD pipeline</span></span>

<span data-ttu-id="52144-251">Most, hogy hello konfiguráció befejezése után a rendszer idő tootest Ez új CI/CD folyamat.</span><span class="sxs-lookup"><span data-stu-id="52144-251">Now that you are done with hello configuration, it's time tootest this new CI/CD pipeline.</span></span> <span data-ttu-id="52144-252">a GitHub-tárház hello tooupdate hello forrás kódot, és aztán hello legegyszerűbb módja tootest változik.</span><span class="sxs-lookup"><span data-stu-id="52144-252">hello easiest way tootest it is tooupdate hello source code and commit hello changes into your GitHub repository.</span></span> <span data-ttu-id="52144-253">Néhány másodperccel azután leküldéses hello kód jelenik meg a Visual Studio Team Services rendszert futtató új buildverziót.</span><span class="sxs-lookup"><span data-stu-id="52144-253">A few seconds after you push hello code, you will see a new build running in Visual Studio Team Services.</span></span> <span data-ttu-id="52144-254">Sikeres befejezést követően egy új kiadási elindul, és hello hello Azure Tárolószolgáltatás-fürt hello alkalmazás új verziójának telepítése.</span><span class="sxs-lookup"><span data-stu-id="52144-254">Once completed successfully, a new release is triggered and deployed hello new version of hello application on hello Azure Container Service cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="52144-255">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="52144-255">Next steps</span></span>

* <span data-ttu-id="52144-256">Visual Studio Team Services CI/CD kapcsolatos további információkért lásd: hello [VSTS összeállítása – áttekintés](https://www.visualstudio.com/docs/build/overview).</span><span class="sxs-lookup"><span data-stu-id="52144-256">For more information about CI/CD with Visual Studio Team Services, see hello [VSTS Build overview](https://www.visualstudio.com/docs/build/overview).</span></span>
* <span data-ttu-id="52144-257">Az ACS-motor kapcsolatos további információkért lásd: hello [ACS motor GitHub-tárház](https://github.com/Azure/acs-engine).</span><span class="sxs-lookup"><span data-stu-id="52144-257">For more information about ACS Engine, see hello [ACS Engine GitHub repo](https://github.com/Azure/acs-engine).</span></span>
* <span data-ttu-id="52144-258">Docker Swarm móddal kapcsolatos további információkért lásd: hello [Docker Swarm – áttekintés](https://docs.docker.com/engine/swarm/).</span><span class="sxs-lookup"><span data-stu-id="52144-258">For more information about Docker Swarm mode, see hello [Docker Swarm mode overview](https://docs.docker.com/engine/swarm/).</span></span>
