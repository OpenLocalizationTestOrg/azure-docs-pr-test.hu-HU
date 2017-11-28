---
title: "CI/CD-ről az Azure Tárolószolgáltatás és a Swarm |} Microsoft Docs"
description: "Azure Tárolószolgáltatás használja a Docker Swarm, egy Azure-tároló beállításkulcs, és a Visual Studio Team Services folyamatosan egy több tároló .NET Core alkalmazás képes biztosítani"
services: container-service
documentationcenter: " "
author: jcorioland
manager: pierlag
tags: acs, azure-container-service
keywords: "Docker, tárolók, Micro-szolgáltatások, a Swarm, a Azure, a Visual Studio Team Services, a DevOps"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: 99c27c37218a35d2a3416d6edd5e0a871cd5c011
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a><span data-ttu-id="9bc92-104">Teljes CI/CD adatcsatorna egy Azure tárolószolgáltatás a Docker Swarmmal Visual Studio Team Services segítségével többszörös tároló alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="9bc92-104">Full CI/CD pipeline to deploy a multi-container application on Azure Container Service with Docker Swarm using Visual Studio Team Services</span></span>

<span data-ttu-id="9bc92-105">Egyik legnagyobb kihívás a felhő modern alkalmazások fejlesztése során ezeknek az alkalmazásoknak folyamatosan küldött alatt.</span><span class="sxs-lookup"><span data-stu-id="9bc92-105">One of the biggest challenges when developing modern applications for the cloud is being able to deliver these applications continuously.</span></span> <span data-ttu-id="9bc92-106">Ebből a cikkből megismerheti, hogyan egy teljes folyamatos integrációt és az Azure Tárolószolgáltatás használata a Docker Swarm Azure tároló beállításjegyzék és a Visual Studio Team Services build (CI/CD) telepítési folyamat bevezetése, valamint a kiadáskezelés.</span><span class="sxs-lookup"><span data-stu-id="9bc92-106">In this article, you learn how to implement a full continuous integration and deployment (CI/CD) pipeline using Azure Container Service with Docker Swarm, Azure Container Registry, and Visual Studio Team Services build and release management.</span></span>

<span data-ttu-id="9bc92-107">Ez a cikk alapján egy egyszerű alkalmazást, a rendelkezésre álló [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), az ASP.NET Core fejlett.</span><span class="sxs-lookup"><span data-stu-id="9bc92-107">This article is based on a simple application, available on [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), developed with ASP.NET Core.</span></span> <span data-ttu-id="9bc92-108">Az alkalmazások négy különböző szolgáltatások állnak: három webes API-k és egy webes előtér:</span><span class="sxs-lookup"><span data-stu-id="9bc92-108">The application is composed of four different services: three web APIs and one web front end:</span></span>

![MyShop mintaalkalmazás](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

<span data-ttu-id="9bc92-110">A célja, hogy az alkalmazás folyamatosan biztosítanak a Docker Swarm fürtön Visual Studio Team Services használatával.</span><span class="sxs-lookup"><span data-stu-id="9bc92-110">The objective is to deliver this application continuously in a Docker Swarm cluster, using Visual Studio Team Services.</span></span> <span data-ttu-id="9bc92-111">Az alábbi ábra az folyamatos kézbesítési adatcsatorna részletesen:</span><span class="sxs-lookup"><span data-stu-id="9bc92-111">The following figure details this continuous delivery pipeline:</span></span>

![MyShop mintaalkalmazás](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

<span data-ttu-id="9bc92-113">A lépések rövid leírása itt található:</span><span class="sxs-lookup"><span data-stu-id="9bc92-113">Here is a brief explanation of the steps:</span></span>

1. <span data-ttu-id="9bc92-114">A forráskódraktárban kódmódosításokat elkötelezettek (Itt GitHub)</span><span class="sxs-lookup"><span data-stu-id="9bc92-114">Code changes are committed to the source code repository (here, GitHub)</span></span> 
2. <span data-ttu-id="9bc92-115">GitHub elindítja a Visual Studio Team Services build</span><span class="sxs-lookup"><span data-stu-id="9bc92-115">GitHub triggers a build in Visual Studio Team Services</span></span> 
3. <span data-ttu-id="9bc92-116">Visual Studio Team Services lekérdezi a források legújabb verzióját, és létrehozza a lemezképeket, amelyek az alkalmazás összeállítása</span><span class="sxs-lookup"><span data-stu-id="9bc92-116">Visual Studio Team Services gets the latest version of the sources and builds all the images that compose the application</span></span> 
4. <span data-ttu-id="9bc92-117">A Visual Studio Team Services az egyes lemezképek egy Docker beállításjegyzék, az Azure-tároló beállításjegyzék szolgáltatással létrehozott leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="9bc92-117">Visual Studio Team Services pushes each image to a Docker registry created using the Azure Container Registry service</span></span> 
5. <span data-ttu-id="9bc92-118">A Visual Studio Team Services új verziót váltja ki.</span><span class="sxs-lookup"><span data-stu-id="9bc92-118">Visual Studio Team Services triggers a new release</span></span> 
6. <span data-ttu-id="9bc92-119">A kiadás néhány SSH használatával az Azure-tárolót szolgáltatás fő fürtcsomóponton parancsokat futtat</span><span class="sxs-lookup"><span data-stu-id="9bc92-119">The release runs some commands using SSH on the Azure container service cluster master node</span></span> 
7. <span data-ttu-id="9bc92-120">A docker Swarm a fürtön a képek legújabb verzióját kéri le.</span><span class="sxs-lookup"><span data-stu-id="9bc92-120">Docker Swarm on the cluster pulls the latest version of the images</span></span> 
8. <span data-ttu-id="9bc92-121">Az alkalmazás új verziójának használatával a Docker Compose van telepítve</span><span class="sxs-lookup"><span data-stu-id="9bc92-121">The new version of the application is deployed using Docker Compose</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="9bc92-122">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9bc92-122">Prerequisites</span></span>

<span data-ttu-id="9bc92-123">Az oktatóanyag elindítása előtt végre kell hajtania a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="9bc92-123">Before starting this tutorial, you need to complete the following tasks:</span></span>

- [<span data-ttu-id="9bc92-124">Swarm-fürt létrehozása az Azure Container Service-ben</span><span class="sxs-lookup"><span data-stu-id="9bc92-124">Create a Swarm cluster in Azure Container Service</span></span>](container-service-deployment.md)
- [<span data-ttu-id="9bc92-125">Csatlakozás a Swarm-fürthöz az Azure Container Service-ben</span><span class="sxs-lookup"><span data-stu-id="9bc92-125">Connect with the Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)
- [<span data-ttu-id="9bc92-126">Hozzon létre egy Azure-tárolót beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="9bc92-126">Create an Azure container registry</span></span>](../../container-registry/container-registry-get-started-portal.md)
- [<span data-ttu-id="9bc92-127">Egy Visual Studio Team Services fiók és a team projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bc92-127">Have a Visual Studio Team Services account and team project created</span></span>](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [<span data-ttu-id="9bc92-128">Elágazás a GitHub-tárházban, a GitHub-fiókba</span><span class="sxs-lookup"><span data-stu-id="9bc92-128">Fork the GitHub repository to your GitHub account</span></span>](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

<span data-ttu-id="9bc92-129">Egy Ubuntu (14.04 vagy 16.04) gépet is telepítve Docker kell.</span><span class="sxs-lookup"><span data-stu-id="9bc92-129">You also need an Ubuntu (14.04 or 16.04) machine with Docker installed.</span></span> <span data-ttu-id="9bc92-130">Ezen a számítógépen a build és kiadás folyamatok során használja a Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="9bc92-130">This machine is used by Visual Studio Team Services during the build and release processes.</span></span> <span data-ttu-id="9bc92-131">Ez gépen létrehozásának egyik módja, hogy a lemezképpel találhatók a [Azure piactér](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/).</span><span class="sxs-lookup"><span data-stu-id="9bc92-131">One way to create this machine is to use the image available in the [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/).</span></span> 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a><span data-ttu-id="9bc92-132">1. lépés: A Visual Studio Team Services-fiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9bc92-132">Step 1: Configure your Visual Studio Team Services account</span></span> 

<span data-ttu-id="9bc92-133">Ebben a szakaszban konfigurálhatja a Visual Studio Team Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="9bc92-133">In this section, you configure your Visual Studio Team Services account.</span></span>

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a><span data-ttu-id="9bc92-134">A Visual Studio Team Services Linux build ügynök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9bc92-134">Configure a Visual Studio Team Services Linux build agent</span></span>

<span data-ttu-id="9bc92-135">Docker-képek létrehozására és az Azure-tárolót beállításjegyzékbe, a Visual Studio Team Services build leküldéses ezeket a lemezképeket, akkor regisztrálnia kell egy Linux-ügynök.</span><span class="sxs-lookup"><span data-stu-id="9bc92-135">To create Docker images and push these images into an Azure container registry from a Visual Studio Team Services build, you need to register a Linux agent.</span></span> <span data-ttu-id="9bc92-136">Ezen telepítési lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="9bc92-136">You have these installation options:</span></span>

* [<span data-ttu-id="9bc92-137">Egy Linux-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="9bc92-137">Deploy an agent on Linux</span></span>](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [<span data-ttu-id="9bc92-138">A VSTS-ügynök futtatásához használt Docker</span><span class="sxs-lookup"><span data-stu-id="9bc92-138">Use Docker to run the VSTS agent</span></span>](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-the-docker-integration-vsts-extension"></a><span data-ttu-id="9bc92-139">A Docker-integráció VSTS-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="9bc92-139">Install the Docker Integration VSTS extension</span></span>

<span data-ttu-id="9bc92-140">A Microsoft biztosít egy VSTS bővítménye build a Docker használni, és felszabadíthatja a folyamatok.</span><span class="sxs-lookup"><span data-stu-id="9bc92-140">Microsoft provides a VSTS extension to work with Docker in build and release processes.</span></span> <span data-ttu-id="9bc92-141">A bővítmény érhető el a [VSTS piactér](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker).</span><span class="sxs-lookup"><span data-stu-id="9bc92-141">This extension is available in the [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker).</span></span> <span data-ttu-id="9bc92-142">Kattintson a **telepítése** VSTS-fiókját a bővítmény hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="9bc92-142">Click **Install** to add this extension to your VSTS account:</span></span>

![A Docker-integráció telepítése](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

<span data-ttu-id="9bc92-144">A rendszer felkéri a hitelesítő adatok használatával VSTS-fiókjához.</span><span class="sxs-lookup"><span data-stu-id="9bc92-144">You are asked to connect to your VSTS account using your credentials.</span></span> 

### <a name="connect-visual-studio-team-services-and-github"></a><span data-ttu-id="9bc92-145">Csatlakoztassa a Visual Studio Team Services, GitHub</span><span class="sxs-lookup"><span data-stu-id="9bc92-145">Connect Visual Studio Team Services and GitHub</span></span>

<span data-ttu-id="9bc92-146">Állítsa be a kapcsolatot a VSTS-projektet és a GitHub-fiók között.</span><span class="sxs-lookup"><span data-stu-id="9bc92-146">Set up a connection between your VSTS project and your GitHub account.</span></span>

1. <span data-ttu-id="9bc92-147">A Visual Studio Team Services projektben kattintson a **beállítások** ikonra az eszköztár, és válassza ki a **szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="9bc92-147">In your Visual Studio Team Services project, click the **Settings** icon in the toolbar, and select **Services**.</span></span>

    ![A Visual Studio Team Services - külső kapcsolatot](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

2. <span data-ttu-id="9bc92-149">Kattintson a bal oldali **új szolgáltatási végpont** > **GitHub**.</span><span class="sxs-lookup"><span data-stu-id="9bc92-149">On the left, click **New Service Endpoint** > **GitHub**.</span></span>

    ![A Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

3. <span data-ttu-id="9bc92-151">A GitHub-fiók használható VSTS engedélyezésére, kattintson a **engedélyezés** és kövesse a megjelenő ablakban.</span><span class="sxs-lookup"><span data-stu-id="9bc92-151">To authorize VSTS to work with your GitHub account, click **Authorize** and follow the procedure in the window that opens.</span></span>

    ![A Visual Studio Team Services - engedélyezik a Githubon](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-to-your-azure-container-registry-and-azure-container-service-cluster"></a><span data-ttu-id="9bc92-153">VSTS csatlakozni az Azure-tárolót beállításjegyzék és Azure Tárolószolgáltatás-fürt</span><span class="sxs-lookup"><span data-stu-id="9bc92-153">Connect VSTS to your Azure container registry and Azure Container Service cluster</span></span>

<span data-ttu-id="9bc92-154">Az utolsó lépéseket azokat a CI/CD adatcsatorna elérése előtt kell a tároló beállításjegyzék és a Docker Swarm-fürt külső kapcsolatok konfigurálása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="9bc92-154">The last steps before getting into the CI/CD pipeline are to configure external connections to your container registry and your Docker Swarm cluster in Azure.</span></span> 

1. <span data-ttu-id="9bc92-155">Az a **szolgáltatások** a Visual Studio Team Services projekt beállítások hozzáadása típusú szolgáltatásvégpont **Docker beállításjegyzék**.</span><span class="sxs-lookup"><span data-stu-id="9bc92-155">In the **Services** settings of your Visual Studio Team Services project, add a service endpoint of type **Docker Registry**.</span></span> 

2. <span data-ttu-id="9bc92-156">A felugró ablakban nyílik meg adja meg az URL-címet, és az Azure-tárolót beállításjegyzék hitelesítő adatai.</span><span class="sxs-lookup"><span data-stu-id="9bc92-156">In the popup that opens, enter the URL and the credentials of your Azure container registry.</span></span>

    ![A Visual Studio Team Services - Docker beállításjegyzék](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

3. <span data-ttu-id="9bc92-158">A Docker Swarm-fürt típusú végpont hozzáadása **SSH**.</span><span class="sxs-lookup"><span data-stu-id="9bc92-158">For the Docker Swarm cluster, add an endpoint of type **SSH**.</span></span> <span data-ttu-id="9bc92-159">Majd adja meg az SSH csatlakozási adatait a Swarm-fürt.</span><span class="sxs-lookup"><span data-stu-id="9bc92-159">Then enter the SSH connection information of your Swarm cluster.</span></span>

    ![A Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

<span data-ttu-id="9bc92-161">A konfiguráció most történik.</span><span class="sxs-lookup"><span data-stu-id="9bc92-161">All the configuration is done now.</span></span> <span data-ttu-id="9bc92-162">A következő lépésekben a CI/CD-feldolgozási folyamat alapszik, és központilag telepíti az alkalmazást a Docker Swarm-fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9bc92-162">In the next steps, you create the CI/CD pipeline that builds and deploys the application to the Docker Swarm cluster.</span></span> 

## <a name="step-2-create-the-build-definition"></a><span data-ttu-id="9bc92-163">2. lépés: A build definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bc92-163">Step 2: Create the build definition</span></span>

<span data-ttu-id="9bc92-164">Ebben a lépésben állítson be egy build definitionfor a VSTS-projektet, és a létrehozási munkafolyamat megadása a tároló lemezképek</span><span class="sxs-lookup"><span data-stu-id="9bc92-164">In this step, you set up a build definitionfor your VSTS project and define the build workflow for your container images</span></span>

### <a name="initial-definition-setup"></a><span data-ttu-id="9bc92-165">Kezdeti beállításának</span><span class="sxs-lookup"><span data-stu-id="9bc92-165">Initial definition setup</span></span>

1. <span data-ttu-id="9bc92-166">Build definíció létrehozása, csatlakozzon a Visual Studio Team Services-projektet, és kattintson **Build & kiadási**.</span><span class="sxs-lookup"><span data-stu-id="9bc92-166">To create a build definition, connect to your Visual Studio Team Services project and click **Build & Release**.</span></span> 

2. <span data-ttu-id="9bc92-167">Az a **definíciók Build** kattintson **+ új**.</span><span class="sxs-lookup"><span data-stu-id="9bc92-167">In the **Build definitions** section, click **+ New**.</span></span> <span data-ttu-id="9bc92-168">Válassza ki a **üres** sablont.</span><span class="sxs-lookup"><span data-stu-id="9bc92-168">Select the **Empty** template.</span></span>

    ![A Visual Studio Team Services - új definíció létrehozása](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

3. <span data-ttu-id="9bc92-170">A GitHub tárház forrását, ellenőrizze az új buildverziót konfigurálása **folyamatos integrációt**, és válassza ki az ügynök várólista, ahol regisztrálták-e a Linux-ügynök.</span><span class="sxs-lookup"><span data-stu-id="9bc92-170">Configure the new build with a GitHub repository source, check **Continuous integration**, and select the agent queue where you registered your Linux agent.</span></span> <span data-ttu-id="9bc92-171">Kattintson a **létrehozása** hozza létre a build definícióit.</span><span class="sxs-lookup"><span data-stu-id="9bc92-171">Click **Create** to create the build definition.</span></span>

    ![A Visual Studio Team Services - Build-definíció létrehozása](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

4. <span data-ttu-id="9bc92-173">Az a **Build definíciók** lapon, először megnyitja a **tárház** lapot és a build az Előfeltételek című szakaszban létrehozott MyShop projekt elágazás használatára konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="9bc92-173">On the **Build Definitions** page, first open the **Repository** tab and configure the build to use the fork of the MyShop project that you created in the prerequisites.</span></span> <span data-ttu-id="9bc92-174">Győződjön meg arról, hogy kiválassza *acs-dokumentumok* , a **alapértelmezett ágat**.</span><span class="sxs-lookup"><span data-stu-id="9bc92-174">Make sure that you select *acs-docs* as the **Default branch**.</span></span>

    ![Visual Studio Team Services - Build tárház konfigurációja](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

5. <span data-ttu-id="9bc92-176">Az a **eseményindítók** lapra, konfigurálja a build minden véglegesítés után aktiválására.</span><span class="sxs-lookup"><span data-stu-id="9bc92-176">On the **Triggers** tab, configure the build to be triggered after each commit.</span></span> <span data-ttu-id="9bc92-177">Válassza ki **folyamatos integrációt** és **módosítások kötegelt**.</span><span class="sxs-lookup"><span data-stu-id="9bc92-177">Select **Continuous integration** and **Batch changes**.</span></span>

    ![Visual Studio Team Services - Build eseményindító konfigurációja](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-the-build-workflow"></a><span data-ttu-id="9bc92-179">Adja meg a létrehozási munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="9bc92-179">Define the build workflow</span></span>
<span data-ttu-id="9bc92-180">A következő lépéseket a build munkafolyamat határozza meg.</span><span class="sxs-lookup"><span data-stu-id="9bc92-180">The next steps define the build workflow.</span></span> <span data-ttu-id="9bc92-181">Öt tároló lemezképeit számára részére a *MyShop* alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9bc92-181">There are five container images to build for the *MyShop* application.</span></span> <span data-ttu-id="9bc92-182">Minden egyes lemezképének összeállítása a Dockerfile a projektben található használatával:</span><span class="sxs-lookup"><span data-stu-id="9bc92-182">Each image is built using the Dockerfile located in the project folders:</span></span>

* <span data-ttu-id="9bc92-183">ProductsApi</span><span class="sxs-lookup"><span data-stu-id="9bc92-183">ProductsApi</span></span>
* <span data-ttu-id="9bc92-184">Proxy</span><span class="sxs-lookup"><span data-stu-id="9bc92-184">Proxy</span></span>
* <span data-ttu-id="9bc92-185">RatingsApi</span><span class="sxs-lookup"><span data-stu-id="9bc92-185">RatingsApi</span></span>
* <span data-ttu-id="9bc92-186">RecommandationsApi</span><span class="sxs-lookup"><span data-stu-id="9bc92-186">RecommandationsApi</span></span>
* <span data-ttu-id="9bc92-187">ShopFront</span><span class="sxs-lookup"><span data-stu-id="9bc92-187">ShopFront</span></span>

<span data-ttu-id="9bc92-188">Adjon hozzá két Docker lépéseket az egyes lemezképek, egy a lemezkép létrehozásához, és egy leküldéses a lemezkép az Azure-tárolót beállításjegyzékben kell.</span><span class="sxs-lookup"><span data-stu-id="9bc92-188">You need to add two Docker steps for each image, one to build the image, and one to push the image in the Azure container registry.</span></span> 

1. <span data-ttu-id="9bc92-189">A build munkafolyamat egy lépés felvételéhez kattintson **+ Hozzáadás összeállítása lépés** válassza **Docker**.</span><span class="sxs-lookup"><span data-stu-id="9bc92-189">To add a step in the build workflow, click **+ Add build step** and select **Docker**.</span></span>

    ![A Visual Studio Team Services - Build lépéseket adhat hozzá.](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

2. <span data-ttu-id="9bc92-191">Az egyes lemezképek konfigurálása egy lépést, amely használja a `docker build` parancsot.</span><span class="sxs-lookup"><span data-stu-id="9bc92-191">For each image, configure one step that uses the `docker build` command.</span></span>

    ![A Visual Studio Team Services - Docker-Build](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    <span data-ttu-id="9bc92-193">A létrehozási műveletet, válassza ki az Azure-tárolót beállításjegyzék, a **lemezképet készít** művelet, és az egyes lemezképek definiáló Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="9bc92-193">For the build operation, select your Azure container registry, the **Build an image** action, and the Dockerfile that defines each image.</span></span> <span data-ttu-id="9bc92-194">Állítsa be a **összeállítási környezet** , a Dockerfile gyökérkönyvtár, és adja meg a **Lemezképnév**.</span><span class="sxs-lookup"><span data-stu-id="9bc92-194">Set the **Build context** as the Dockerfile root directory, and define the **Image Name**.</span></span> 
    
    <span data-ttu-id="9bc92-195">Amint az előző képernyőn, a lemezkép neve kezdődhet URI-azonosítója az Azure-tárolót beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="9bc92-195">As shown on the preceding screen, start the image name with the URI of your Azure container registry.</span></span> <span data-ttu-id="9bc92-196">(Is segítségével build változó parametrizálja a címke a kép, például az ebben a példában a build azonosítója.)</span><span class="sxs-lookup"><span data-stu-id="9bc92-196">(You can also use a build variable to parameterize the tag of the image, such as the build identifier in this example.)</span></span>

3. <span data-ttu-id="9bc92-197">Az egyes lemezképek, a második lépésben használó konfigurálása a `docker push` parancsot.</span><span class="sxs-lookup"><span data-stu-id="9bc92-197">For each image, configure a second step that uses the `docker push` command.</span></span>

    ![A Visual Studio Team Services - Docker leküldéses](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    <span data-ttu-id="9bc92-199">Leküldéses művelethez válassza ki az Azure-tárolót beállításjegyzék a **lemezkép leküldéses** művelet, és írja be a **kép neve** , amely az előző lépésben épül.</span><span class="sxs-lookup"><span data-stu-id="9bc92-199">For the push operation, select your Azure container registry, the **Push an image** action, and enter the **Image Name** that is built in the previous step.</span></span>

4. <span data-ttu-id="9bc92-200">Miután konfigurálta a build és leküldéses lépéseket az egyes öt lemezképet, adja hozzá a két további lépés a build munkafolyamatban.</span><span class="sxs-lookup"><span data-stu-id="9bc92-200">After you configure the build and push steps for each of the five images, add two more steps in the build workflow.</span></span>

    <span data-ttu-id="9bc92-201">a.</span><span class="sxs-lookup"><span data-stu-id="9bc92-201">a.</span></span> <span data-ttu-id="9bc92-202">A parancssori feladatot, amely egy bash parancsfájlt használ a *BuildNumber* a docker-compose.yml fájlt, és a jelenlegi számainak létrehozása azonosítóját. További részletek a következő képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9bc92-202">A command-line task that uses a bash script to replace the *BuildNumber* occurrence in the docker-compose.yml file with the current build Id. See the following screen for details.</span></span>

    ![A Visual Studio Team Services - frissítés Compose fájl](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    <span data-ttu-id="9bc92-204">b.</span><span class="sxs-lookup"><span data-stu-id="9bc92-204">b.</span></span> <span data-ttu-id="9bc92-205">Ez a feladat elutasítja azokat a frissített Compose fájlt egy összeállítási összetevő, így használható a kiadásban.</span><span class="sxs-lookup"><span data-stu-id="9bc92-205">A task that drops the updated Compose file as a build artifact so it can be used in the release.</span></span> <span data-ttu-id="9bc92-206">További részletek a következő képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9bc92-206">See the following screen for details.</span></span>

    ![A Visual Studio Team Services - Compose közzététele fájl](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

5. <span data-ttu-id="9bc92-208">Kattintson a **mentése** és a build definition neve.</span><span class="sxs-lookup"><span data-stu-id="9bc92-208">Click **Save** and name your build definition.</span></span>

## <a name="step-3-create-the-release-definition"></a><span data-ttu-id="9bc92-209">3. lépés: A kiadási definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="9bc92-209">Step 3: Create the release definition</span></span>

<span data-ttu-id="9bc92-210">A Visual Studio Team Services lehetővé teszi [kiadásokban kezelése környezetek között](https://www.visualstudio.com/team-services/release-management/).</span><span class="sxs-lookup"><span data-stu-id="9bc92-210">Visual Studio Team Services allows you to [manage releases across environments](https://www.visualstudio.com/team-services/release-management/).</span></span> <span data-ttu-id="9bc92-211">Folyamatos üzembe helyezés, győződjön meg arról, hogy az alkalmazás telepítve van a különböző környezetekben (például a fejlesztői, tesztelési, éles üzem előtti és éles) zökkenőmentes úgy is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="9bc92-211">You can enable continuous deployment to make sure that your application is deployed on your different environments (such as dev, test, pre-production, and production) in a smooth way.</span></span> <span data-ttu-id="9bc92-212">Létrehozhat egy új környezetben, amely az Azure tároló szolgáltatás Docker Swarm-fürt jelöli.</span><span class="sxs-lookup"><span data-stu-id="9bc92-212">You can create a new environment that represents your Azure Container Service Docker Swarm cluster.</span></span>

![A Visual Studio Team Services - ACS verzióját](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a><span data-ttu-id="9bc92-214">Eredeti kiadásának beállítása</span><span class="sxs-lookup"><span data-stu-id="9bc92-214">Initial release setup</span></span>

1. <span data-ttu-id="9bc92-215">Egy kiadási definition létrehozásához kattintson a **kiadásokban** > **+ kiadás**</span><span class="sxs-lookup"><span data-stu-id="9bc92-215">To create a release definition, click **Releases** > **+ Release**</span></span>

2. <span data-ttu-id="9bc92-216">Az összetevő-forrás konfigurálásához kattintson **összetevők** > **hivatkozás egy összetevő forrás**.</span><span class="sxs-lookup"><span data-stu-id="9bc92-216">To configure the artifact source, Click **Artifacts** > **Link an artifact source**.</span></span> <span data-ttu-id="9bc92-217">A build, amelyet az előző lépésben megadott itt, csatolja az új kiadási definíciója.</span><span class="sxs-lookup"><span data-stu-id="9bc92-217">Here, link this new release definition to the build that you defined in the previous step.</span></span> <span data-ttu-id="9bc92-218">Ezzel a docker-compose.yml fájlt érhető el a kibocsátási folyamatban.</span><span class="sxs-lookup"><span data-stu-id="9bc92-218">By doing this, the docker-compose.yml file is available in the release process.</span></span>

    ![A Visual Studio Team Services - kiadás összetevők](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

3. <span data-ttu-id="9bc92-220">A kiadási eseményindító konfigurálásához kattintson **eseményindítók** válassza **folyamatos üzembe helyezés**.</span><span class="sxs-lookup"><span data-stu-id="9bc92-220">To configure the release trigger, click **Triggers** and select **Continuous Deployment**.</span></span> <span data-ttu-id="9bc92-221">Az eseményindító be ugyanazon összetevő forrás.</span><span class="sxs-lookup"><span data-stu-id="9bc92-221">Set the trigger on the same artifact source.</span></span> <span data-ttu-id="9bc92-222">Ez a beállítás biztosítja, hogy az új kiadási elindul, amint a létrehozás sikeresen befejeződik.</span><span class="sxs-lookup"><span data-stu-id="9bc92-222">This setting ensures that a new release starts as soon as the build completes successfully.</span></span>

    ![A Visual Studio Team Services - kiadás eseményindítók](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-the-release-workflow"></a><span data-ttu-id="9bc92-224">Adja meg a kiadási munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="9bc92-224">Define the release workflow</span></span>

<span data-ttu-id="9bc92-225">A kiadási munkafolyamat két feladatot hozzáadott tevődik össze.</span><span class="sxs-lookup"><span data-stu-id="9bc92-225">The release workflow is composed of two tasks that you add.</span></span>

1. <span data-ttu-id="9bc92-226">Egy feladat biztonságosan másolja a compose-fájlt egy *telepítése* mappa a Docker Swarm fő csomóponton korábban konfigurált SSH-kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="9bc92-226">Configure a task to securely copy the compose file to a *deploy* folder on the Docker Swarm master node, using the SSH connection you configured previously.</span></span> <span data-ttu-id="9bc92-227">További részletek a következő képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9bc92-227">See the following screen for details.</span></span>

    ![Visual Studio Team Services - kiadás SCP-je](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

2. <span data-ttu-id="9bc92-229">Egy második feladat futtatásához bash parancs `docker` és `docker-compose` parancsok a fő csomóponton.</span><span class="sxs-lookup"><span data-stu-id="9bc92-229">Configure a second task to execute a bash command to run `docker` and `docker-compose` commands on the master node.</span></span> <span data-ttu-id="9bc92-230">További részletek a következő képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9bc92-230">See the following screen for details.</span></span>

    ![A Visual Studio Team Services - kiadás Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    <span data-ttu-id="9bc92-232">A parancs végrehajtása meg a fő használja, a Docker parancssori felületén és a Docker Compose CLI-t a következő feladatok elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="9bc92-232">The command executed on the master use the Docker CLI and the Docker-Compose CLI to do the following tasks:</span></span>

    - <span data-ttu-id="9bc92-233">Bejelentkezés az Azure-tárolót beállításjegyzék (három build variab'les definiált használ a **változók** lapon)</span><span class="sxs-lookup"><span data-stu-id="9bc92-233">Login to the Azure container registry (it uses three build variab\`les that are defined in the **Variables** tab)</span></span>
    - <span data-ttu-id="9bc92-234">Adja meg a **DOCKER_HOST** működéséhez a Swarm végponthoz változó (: 2375)</span><span class="sxs-lookup"><span data-stu-id="9bc92-234">Define the **DOCKER_HOST** variable to work with the Swarm endpoint (:2375)</span></span>
    - <span data-ttu-id="9bc92-235">Keresse meg a *telepítése* , amely az előző biztonságos másolási feladat hozta létre, és a docker-compose.yml fájlt tartalmazó mappa</span><span class="sxs-lookup"><span data-stu-id="9bc92-235">Navigate to the *deploy* folder that was created by the preceding secure copy task and that contains the docker-compose.yml file</span></span> 
    - <span data-ttu-id="9bc92-236">Végrehajtás `docker-compose` parancsokat, amelyek az új lemezképet lekéréses leállítania a szolgáltatásokat, távolítsa el a szolgáltatásokat, és a tárolók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9bc92-236">Execute `docker-compose` commands that pull the new images, stop the services, remove the services, and create the containers.</span></span>

    >[!IMPORTANT]
    > <span data-ttu-id="9bc92-237">Amint az előző képernyőn, hagyja a **stderr-en sikertelen** jelölőnégyzet nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="9bc92-237">As shown on the preceding screen, leave the **Fail on STDERR** checkbox unchecked.</span></span> <span data-ttu-id="9bc92-238">Ez az egy fontos beállítása, mert `docker-compose` több diagnosztikai üzeneteket, jelenít meg, például a tárolók leállítása vagy törlése, a standard hibák kimenetét.</span><span class="sxs-lookup"><span data-stu-id="9bc92-238">This is an important setting, because `docker-compose` prints several diagnostic messages, such as containers are stopping or being deleted, on the standard error output.</span></span> <span data-ttu-id="9bc92-239">Jelölje be a jelölőnégyzetet, ha a Visual Studio Team Services jelenti, hogy a kiadás során hibák jelentkeztek akkor is, ha minden megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="9bc92-239">If you check the checkbox, Visual Studio Team Services reports that errors occurred during the release, even if all goes well.</span></span>
    >
3. <span data-ttu-id="9bc92-240">Mentse az új kiadási definíciója.</span><span class="sxs-lookup"><span data-stu-id="9bc92-240">Save this new release definition.</span></span>


>[!NOTE]
><span data-ttu-id="9bc92-241">A központi telepítés tartalmazza a némi állásidővel, mivel azt a régi szolgáltatások leállítása és futtatja az újjal.</span><span class="sxs-lookup"><span data-stu-id="9bc92-241">This deployment includes some downtime because we are stopping the old services and running the new one.</span></span> <span data-ttu-id="9bc92-242">Ennek elkerüléséhez a kék-zöld központi telepítés végrehajtásával lehetőség.</span><span class="sxs-lookup"><span data-stu-id="9bc92-242">It is possible to avoid this by doing a blue-green deployment.</span></span>
>

## <a name="step-4-test-the-cicd-pipeline"></a><span data-ttu-id="9bc92-243">4. lépés</span><span class="sxs-lookup"><span data-stu-id="9bc92-243">Step 4.</span></span> <span data-ttu-id="9bc92-244">A CI/CD-feldolgozási folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="9bc92-244">Test the CI/CD pipeline</span></span>

<span data-ttu-id="9bc92-245">Most, hogy a konfiguráció befejezése után is tesztelheti az új CI/CD adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="9bc92-245">Now that you are done with the configuration, it's time to test this new CI/CD pipeline.</span></span> <span data-ttu-id="9bc92-246">A tesztek legkönnyebben frissítése a forráskódot, és véglegesítse a módosításokat a GitHub-tárházban történő.</span><span class="sxs-lookup"><span data-stu-id="9bc92-246">The easiest way to test it is to update the source code and commit the changes into your GitHub repository.</span></span> <span data-ttu-id="9bc92-247">Néhány másodperccel azután leküldéses a kód jelenik meg a Visual Studio Team Services rendszert futtató új buildverziót.</span><span class="sxs-lookup"><span data-stu-id="9bc92-247">A few seconds after you push the code, you will see a new build running in Visual Studio Team Services.</span></span> <span data-ttu-id="9bc92-248">Sikeres befejezést követően az új kiadási indul, és az Azure Tárolószolgáltatás-fürthöz az alkalmazás új verzióját telepíti.</span><span class="sxs-lookup"><span data-stu-id="9bc92-248">Once completed successfully, a new release will be triggered and will deploy the new version of the application on the Azure Container Service cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9bc92-249">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9bc92-249">Next Steps</span></span>

* <span data-ttu-id="9bc92-250">Visual Studio Team Services CI/CD kapcsolatos további információkért tekintse meg a [VSTS összeállítása – áttekintés](https://www.visualstudio.com/docs/build/overview).</span><span class="sxs-lookup"><span data-stu-id="9bc92-250">For more information about CI/CD with Visual Studio Team Services, see the [VSTS Build overview](https://www.visualstudio.com/docs/build/overview).</span></span>
