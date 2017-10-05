---
title: "CI/CD-ről az Azure-tárolót szolgáltatás vezérlőprogramja és Swarm mód |} Microsoft Docs"
description: "Azure-tároló szolgáltatás vezérlőprogramja használja Docker Swarm-módban, egy Azure-tároló beállításkulcs, és a Visual Studio Team Services egy több tároló .NET Core alkalmazás folyamatosan továbbítására"
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
ms.openlocfilehash: 2c0e5fe4f60738fcc1aa67a78674e6f3c62e5628
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-visual-studio-team-services"></a><span data-ttu-id="b6c96-104">Azure tárolószolgáltatás egy több tároló alkalmazás telepítését az ACS-motor és a Docker Swarm mód Visual Studio Team Services használatával teljes CI/CD folyamat</span><span class="sxs-lookup"><span data-stu-id="b6c96-104">Full CI/CD pipeline to deploy a multi-container application on Azure Container Service with ACS Engine and Docker Swarm Mode using Visual Studio Team Services</span></span>

<span data-ttu-id="b6c96-105">*Ez a cikk alapján [teljes CI/CD adatcsatorna egy Azure tárolószolgáltatás a Docker Swarmmal Visual Studio Team Services segítségével többszörös tároló alkalmazás üzembe helyezése](container-service-docker-swarm-setup-ci-cd.md) dokumentáció*</span><span class="sxs-lookup"><span data-stu-id="b6c96-105">*This article is based on [Full CI/CD pipeline to deploy a multi-container application on Azure Container Service with Docker Swarm using Visual Studio Team Services](container-service-docker-swarm-setup-ci-cd.md) documentation*</span></span>

<span data-ttu-id="b6c96-106">Napjainkban egyik legnagyobb kihívás a felhő modern alkalmazások fejlesztése során az igényt ezeknek az alkalmazásoknak folyamatosan továbbítására.</span><span class="sxs-lookup"><span data-stu-id="b6c96-106">Nowadays, One of the biggest challenges when developing modern applications for the cloud is being able to deliver these applications continuously.</span></span> <span data-ttu-id="b6c96-107">Ebből a cikkből megismerheti, hogyan egy teljes folyamatos integrációt és a központi telepítés (CI/CD) folyamat használatával megvalósításához:</span><span class="sxs-lookup"><span data-stu-id="b6c96-107">In this article, you learn how to implement a full continuous integration and deployment (CI/CD) pipeline using:</span></span> 
* <span data-ttu-id="b6c96-108">Azure-tárolót szolgáltatás motorja a Docker Swarm mód</span><span class="sxs-lookup"><span data-stu-id="b6c96-108">Azure Container Service Engine with Docker Swarm Mode</span></span>
* <span data-ttu-id="b6c96-109">Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="b6c96-109">Azure Container Registry</span></span>
* <span data-ttu-id="b6c96-110">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="b6c96-110">Visual Studio Team Services</span></span>

<span data-ttu-id="b6c96-111">Ez a cikk alapján egy egyszerű alkalmazást, a rendelkezésre álló [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), az ASP.NET Core fejlett.</span><span class="sxs-lookup"><span data-stu-id="b6c96-111">This article is based on a simple application, available on [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), developed with ASP.NET Core.</span></span> <span data-ttu-id="b6c96-112">Az alkalmazások négy különböző szolgáltatások állnak: három webes API-k és egy webes előtér:</span><span class="sxs-lookup"><span data-stu-id="b6c96-112">The application is composed of four different services: three web APIs and one web front end:</span></span>

![MyShop mintaalkalmazás](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

<span data-ttu-id="b6c96-114">A célja, hogy az alkalmazás folyamatosan biztosítanak a Docker Swarm mód fürtben, a Visual Studio Team Services használatával.</span><span class="sxs-lookup"><span data-stu-id="b6c96-114">The objective is to deliver this application continuously in a Docker Swarm Mode cluster, using Visual Studio Team Services.</span></span> <span data-ttu-id="b6c96-115">Az alábbi ábra az folyamatos kézbesítési adatcsatorna részletesen:</span><span class="sxs-lookup"><span data-stu-id="b6c96-115">The following figure details this continuous delivery pipeline:</span></span>

![MyShop mintaalkalmazás](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

<span data-ttu-id="b6c96-117">A lépések rövid leírása itt található:</span><span class="sxs-lookup"><span data-stu-id="b6c96-117">Here is a brief explanation of the steps:</span></span>

1. <span data-ttu-id="b6c96-118">A forráskódraktárban kódmódosításokat elkötelezettek (Itt GitHub)</span><span class="sxs-lookup"><span data-stu-id="b6c96-118">Code changes are committed to the source code repository (here, GitHub)</span></span> 
2. <span data-ttu-id="b6c96-119">GitHub elindítja a Visual Studio Team Services build</span><span class="sxs-lookup"><span data-stu-id="b6c96-119">GitHub triggers a build in Visual Studio Team Services</span></span> 
3. <span data-ttu-id="b6c96-120">Visual Studio Team Services lekérdezi a források legújabb verzióját, és létrehozza a lemezképeket, amelyek az alkalmazás összeállítása</span><span class="sxs-lookup"><span data-stu-id="b6c96-120">Visual Studio Team Services gets the latest version of the sources and builds all the images that compose the application</span></span> 
4. <span data-ttu-id="b6c96-121">A Visual Studio Team Services az egyes lemezképek egy Docker beállításjegyzék, az Azure-tároló beállításjegyzék szolgáltatással létrehozott leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="b6c96-121">Visual Studio Team Services pushes each image to a Docker registry created using the Azure Container Registry service</span></span> 
5. <span data-ttu-id="b6c96-122">A Visual Studio Team Services új verziót váltja ki.</span><span class="sxs-lookup"><span data-stu-id="b6c96-122">Visual Studio Team Services triggers a new release</span></span> 
6. <span data-ttu-id="b6c96-123">A kiadás néhány SSH használatával az Azure-tárolót szolgáltatás fő fürtcsomóponton parancsokat futtat</span><span class="sxs-lookup"><span data-stu-id="b6c96-123">The release runs some commands using SSH on the Azure container service cluster master node</span></span> 
7. <span data-ttu-id="b6c96-124">Docker Swarm üzemmódot a fürtön a képek legújabb verzióját kéri le.</span><span class="sxs-lookup"><span data-stu-id="b6c96-124">Docker Swarm Mode on the cluster pulls the latest version of the images</span></span> 
8. <span data-ttu-id="b6c96-125">Az alkalmazás új verziójának Docker verem használatával van telepítve</span><span class="sxs-lookup"><span data-stu-id="b6c96-125">The new version of the application is deployed using Docker Stack</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b6c96-126">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b6c96-126">Prerequisites</span></span>

<span data-ttu-id="b6c96-127">Az oktatóanyag elindítása előtt végre kell hajtania a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="b6c96-127">Before starting this tutorial, you need to complete the following tasks:</span></span>

- [<span data-ttu-id="b6c96-128">Az Azure Tárolószolgáltatásban ACS motorral mód Swarm-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6c96-128">Create a Swarm Mode cluster in Azure Container Service with ACS Engine</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [<span data-ttu-id="b6c96-129">Csatlakozás a Swarm-fürthöz az Azure Container Service-ben</span><span class="sxs-lookup"><span data-stu-id="b6c96-129">Connect with the Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)
- [<span data-ttu-id="b6c96-130">Hozzon létre egy Azure-tárolót beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="b6c96-130">Create an Azure container registry</span></span>](../../container-registry/container-registry-get-started-portal.md)
- [<span data-ttu-id="b6c96-131">Egy Visual Studio Team Services fiók és a team projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6c96-131">Have a Visual Studio Team Services account and team project created</span></span>](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [<span data-ttu-id="b6c96-132">Elágazás a GitHub-tárházban, a GitHub-fiókba</span><span class="sxs-lookup"><span data-stu-id="b6c96-132">Fork the GitHub repository to your GitHub account</span></span>](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> <span data-ttu-id="b6c96-133">Az Azure Container Service Docker Swarm-vezénylője örökölt önálló Swarmot használ.</span><span class="sxs-lookup"><span data-stu-id="b6c96-133">The Docker Swarm orchestrator in Azure Container Service uses legacy standalone Swarm.</span></span> <span data-ttu-id="b6c96-134">Az integrált [Swarm mód](https://docs.docker.com/engine/swarm/) (a Docker 1.12-es és újabb verzióiban) jelenleg nem támogatott vezénylő az Azure Container Service-ben.</span><span class="sxs-lookup"><span data-stu-id="b6c96-134">Currently, the integrated [Swarm mode](https://docs.docker.com/engine/swarm/) (in Docker 1.12 and higher) is not a supported orchestrator in Azure Container Service.</span></span> <span data-ttu-id="b6c96-135">Emiatt használjuk [ACS motor](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), közösségi hozzájárult [gyorsindítási sablonon](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), vagy egy Docker-megoldás a a [Azure piactér](https://azuremarketplace.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b6c96-135">For this reason, we are using [ACS Engine](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), a community-contributed [quickstart template](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), or a Docker solution in the [Azure Marketplace](https://azuremarketplace.microsoft.com).</span></span>
>

## <a name="step-1-configure-your-visual-studio-team-services-account"></a><span data-ttu-id="b6c96-136">1. lépés: A Visual Studio Team Services-fiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b6c96-136">Step 1: Configure your Visual Studio Team Services account</span></span> 

<span data-ttu-id="b6c96-137">Ebben a szakaszban konfigurálhatja a Visual Studio Team Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="b6c96-137">In this section, you configure your Visual Studio Team Services account.</span></span> <span data-ttu-id="b6c96-138">A VSTS Services végpontjainak, a Visual Studio Team Services projekt konfigurálásához kattintson a **beállítások** ikonra az eszköztár, és válassza ki a **szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-138">To configure VSTS Services Endpoints, in your Visual Studio Team Services project, click the **Settings** icon in the toolbar, and select **Services**.</span></span>

![Nyissa meg a szolgáltatások végpont](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-visual-studio-team-services-and-azure-account"></a><span data-ttu-id="b6c96-140">Csatlakozás a Visual Studio Team Services és az Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="b6c96-140">Connect Visual Studio Team Services and Azure account</span></span>

<span data-ttu-id="b6c96-141">Állítsa be a kapcsolatot a VSTS-projektet és az Azure-fiókjával között.</span><span class="sxs-lookup"><span data-stu-id="b6c96-141">Set up a connection between your VSTS project and your Azure account.</span></span>

1. <span data-ttu-id="b6c96-142">Kattintson a bal oldali **új szolgáltatási végpont** > **Azure Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-142">On the left, click **New Service Endpoint** > **Azure Resource Manager**.</span></span>
2. <span data-ttu-id="b6c96-143">Használható az Azure-fiókjával VSTS engedélyezésére, válassza ki a **előfizetés** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-143">To authorize VSTS to work with your Azure account, select your **Subscription** and click **OK**.</span></span>

    ![A Visual Studio Team Services - Azure engedélyezése](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-visual-studio-team-services-and-github"></a><span data-ttu-id="b6c96-145">Csatlakoztassa a Visual Studio Team Services, GitHub</span><span class="sxs-lookup"><span data-stu-id="b6c96-145">Connect Visual Studio Team Services and GitHub</span></span>

<span data-ttu-id="b6c96-146">Állítsa be a kapcsolatot a VSTS-projektet és a GitHub-fiók között.</span><span class="sxs-lookup"><span data-stu-id="b6c96-146">Set up a connection between your VSTS project and your GitHub account.</span></span>

1. <span data-ttu-id="b6c96-147">Kattintson a bal oldali **új szolgáltatási végpont** > **GitHub**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-147">On the left, click **New Service Endpoint** > **GitHub**.</span></span>
2. <span data-ttu-id="b6c96-148">A GitHub-fiók használható VSTS engedélyezésére, kattintson a **engedélyezés** és kövesse a megjelenő ablakban.</span><span class="sxs-lookup"><span data-stu-id="b6c96-148">To authorize VSTS to work with your GitHub account, click **Authorize** and follow the procedure in the window that opens.</span></span>

    ![A Visual Studio Team Services - engedélyezik a Githubon](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-vsts-to-your-azure-container-service-cluster"></a><span data-ttu-id="b6c96-150">VSTS csatlakozni az Azure Tárolószolgáltatás-fürt</span><span class="sxs-lookup"><span data-stu-id="b6c96-150">Connect VSTS to your Azure Container Service cluster</span></span>

<span data-ttu-id="b6c96-151">Az utolsó előtt a CI/CD-feldolgozási folyamat az első lépésekre külső kapcsolatot a Docker Swarm-fürt konfigurálása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b6c96-151">The last steps before getting into the CI/CD pipeline are to configure external connections to your Docker Swarm cluster in Azure.</span></span> 

1. <span data-ttu-id="b6c96-152">A Docker Swarm-fürt típusú végpont hozzáadása **SSH**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-152">For the Docker Swarm cluster, add an endpoint of type **SSH**.</span></span> <span data-ttu-id="b6c96-153">Majd adja meg az SSH csatlakozási adatait a Swarm-fürt (főcsomópont).</span><span class="sxs-lookup"><span data-stu-id="b6c96-153">Then enter the SSH connection information of your Swarm cluster (master node).</span></span>

    ![A Visual Studio Team Services - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

<span data-ttu-id="b6c96-155">A konfiguráció most történik.</span><span class="sxs-lookup"><span data-stu-id="b6c96-155">All the configuration is done now.</span></span> <span data-ttu-id="b6c96-156">A következő lépésekben a CI/CD-feldolgozási folyamat alapszik, és központilag telepíti az alkalmazást a Docker Swarm-fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b6c96-156">In the next steps, you create the CI/CD pipeline that builds and deploys the application to the Docker Swarm cluster.</span></span> 

## <a name="step-2-create-the-build-definition"></a><span data-ttu-id="b6c96-157">2. lépés: A build definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6c96-157">Step 2: Create the build definition</span></span>

<span data-ttu-id="b6c96-158">Ebben a lépésben egy build definition beállítása a VSTS-projekthez, és a létrehozási munkafolyamat megadása a tároló lemezképek</span><span class="sxs-lookup"><span data-stu-id="b6c96-158">In this step, you set up a build definition for your VSTS project and define the build workflow for your container images</span></span>

### <a name="initial-definition-setup"></a><span data-ttu-id="b6c96-159">Kezdeti beállításának</span><span class="sxs-lookup"><span data-stu-id="b6c96-159">Initial definition setup</span></span>

1. <span data-ttu-id="b6c96-160">Build definíció létrehozása, csatlakozzon a Visual Studio Team Services-projektet, és kattintson **Build & kiadási**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-160">To create a build definition, connect to your Visual Studio Team Services project and click **Build & Release**.</span></span> <span data-ttu-id="b6c96-161">Az a **definíciók Build** kattintson **+ új**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-161">In the **Build definitions** section, click **+ New**.</span></span> 

    ![A Visual Studio Team Services - új definíció létrehozása](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. <span data-ttu-id="b6c96-163">Válassza ki a **üres folyamat**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-163">Select the **Empty process**.</span></span>

    ![A Visual Studio Team Services - új üres Build definíció](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. <span data-ttu-id="b6c96-165">Kattintson a **változók** lapra, és hozzon létre két új változót: **RegistryURL** és **AgentURL**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-165">Then, click the **Variables** tab and create two new variables: **RegistryURL** and **AgentURL**.</span></span> <span data-ttu-id="b6c96-166">Illessze be a beállításjegyzék és a fürt ügynökök DNS értékeit.</span><span class="sxs-lookup"><span data-stu-id="b6c96-166">Paste the values of your Registry and Cluster Agents DNS.</span></span>

    ![A Visual Studio Team Services - Build változók konfigurációját](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. <span data-ttu-id="b6c96-168">A a **Build definíciók** lapon nyissa meg a **eseményindítók** lapot és a build folyamatos integrációt használata az Előfeltételek című szakaszban létrehozott MyShop projekt elágazás konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="b6c96-168">On the **Build Definitions** page, open the **Triggers** tab and configure the build to use continuous integration with the fork of the MyShop project that you created in the prerequisites.</span></span> <span data-ttu-id="b6c96-169">Ezt követően válassza **módosítások kötegelt**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-169">Then, select **Batch changes**.</span></span> <span data-ttu-id="b6c96-170">Győződjön meg arról, hogy kiválassza *docker-linux* , a **specification fiókirodai**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-170">Make sure that you select *docker-linux* as the **Branch specification**.</span></span>

    ![Visual Studio Team Services - Build tárház konfigurációja](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. <span data-ttu-id="b6c96-172">Végül kattintson a **beállítások** lapra, és konfigurálja az alapértelmezett ügynök várólistához **üzemeltetett Linux előzetes**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-172">Finally, click the **Options** tab and configure the Default agent queue to **Hosted Linux Preview**.</span></span>

    ![A Visual Studio Team Services - Gazdagépügynök-konfigurálási](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-the-build-workflow"></a><span data-ttu-id="b6c96-174">Adja meg a létrehozási munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="b6c96-174">Define the build workflow</span></span>
<span data-ttu-id="b6c96-175">A következő lépéseket a build munkafolyamat határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b6c96-175">The next steps define the build workflow.</span></span> <span data-ttu-id="b6c96-176">Először a kód forrását konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="b6c96-176">First, you need to configure the source of the code.</span></span> <span data-ttu-id="b6c96-177">Ehhez jelölje ki a **GitHub** és a **tárház** és **fiókirodai** (docker-linux).</span><span class="sxs-lookup"><span data-stu-id="b6c96-177">To do it, select **GitHub** and your **repository** and **branch** (docker-linux).</span></span>

![Konfigurálja a Visual Studio Team Services - kód forrása](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

<span data-ttu-id="b6c96-179">Öt tároló lemezképeit számára részére a *MyShop* alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b6c96-179">There are five container images to build for the *MyShop* application.</span></span> <span data-ttu-id="b6c96-180">Minden egyes lemezképének összeállítása a Dockerfile a projektben található használatával:</span><span class="sxs-lookup"><span data-stu-id="b6c96-180">Each image is built using the Dockerfile located in the project folders:</span></span>

* <span data-ttu-id="b6c96-181">ProductsApi</span><span class="sxs-lookup"><span data-stu-id="b6c96-181">ProductsApi</span></span>
* <span data-ttu-id="b6c96-182">Proxy</span><span class="sxs-lookup"><span data-stu-id="b6c96-182">Proxy</span></span>
* <span data-ttu-id="b6c96-183">RatingsApi</span><span class="sxs-lookup"><span data-stu-id="b6c96-183">RatingsApi</span></span>
* <span data-ttu-id="b6c96-184">RecommandationsApi</span><span class="sxs-lookup"><span data-stu-id="b6c96-184">RecommandationsApi</span></span>
* <span data-ttu-id="b6c96-185">ShopFront</span><span class="sxs-lookup"><span data-stu-id="b6c96-185">ShopFront</span></span>

<span data-ttu-id="b6c96-186">Két Docker lépéseket az egyes lemezképek, egy a lemezkép létrehozásához, és egy leküldéses a lemezkép az Azure-tárolót beállításjegyzék van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b6c96-186">You need two Docker steps for each image, one to build the image, and one to push the image in the Azure container registry.</span></span> 

1. <span data-ttu-id="b6c96-187">A build munkafolyamat egy lépés felvételéhez kattintson **+ Hozzáadás összeállítása lépés** válassza **Docker**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-187">To add a step in the build workflow, click **+ Add build step** and select **Docker**.</span></span>

    ![A Visual Studio Team Services - Build lépéseket adhat hozzá.](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. <span data-ttu-id="b6c96-189">Az egyes lemezképek konfigurálása egy lépést, amely használja a `docker build` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b6c96-189">For each image, configure one step that uses the `docker build` command.</span></span>

    ![A Visual Studio Team Services - Docker-Build](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    <span data-ttu-id="b6c96-191">A létrehozási műveletet, válassza ki az Azure-tároló beállításjegyzék, a **lemezképet készít** művelet, és az egyes lemezképek definiáló Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="b6c96-191">For the build operation, select your Azure Container Registry, the **Build an image** action, and the Dockerfile that defines each image.</span></span> <span data-ttu-id="b6c96-192">Állítsa be a **munkakönyvtár** , Dockerfile gyökérkönyvtáraként határozza meg a **kép neve**, és válassza ki **legújabb címke közé tartoznak**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-192">Set the **Working Directory** as the Dockerfile root directory, define the **Image Name**, and select **Include Latest Tag**.</span></span>
    
    <span data-ttu-id="b6c96-193">A lemezkép neve-nak kell lennie a következő formátumban: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```.</span><span class="sxs-lookup"><span data-stu-id="b6c96-193">The Image Name has to be in this format: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```.</span></span> <span data-ttu-id="b6c96-194">Cserélje le **[NAME]** kép nevű:</span><span class="sxs-lookup"><span data-stu-id="b6c96-194">Replace **[NAME]** with the image name:</span></span>
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. <span data-ttu-id="b6c96-195">Az egyes lemezképek, a második lépésben használó konfigurálása a `docker push` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b6c96-195">For each image, configure a second step that uses the `docker push` command.</span></span>

    ![A Visual Studio Team Services - Docker leküldéses](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    <span data-ttu-id="b6c96-197">A leküldéses művelethez válassza ki az Azure-tárolót beállításjegyzék a **lemezkép leküldéses** művelet, adja meg a **kép neve** , amely az előző lépést, és válassza ki a beépített **legújabb címkét tartalmaz**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-197">For the push operation, select your Azure container registry, the **Push an image** action, enter the **Image Name** that is built in the previous step and select **Include Latest Tag**.</span></span>

4. <span data-ttu-id="b6c96-198">Miután konfigurálta a build és leküldéses lépéseket az egyes öt lemezképet, a build munkafolyamat a három további lépéseket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="b6c96-198">After you configure the build and push steps for each of the five images, add three more steps in the build workflow.</span></span>

   ![A Visual Studio Team Services - parancssori feladat hozzáadása](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

      1. <span data-ttu-id="b6c96-200">A parancssori feladatot, amely egy bash parancsfájlt használ a *RegistryURL* a docker-compose.yml fájlt a RegistryURL változóval előfordulása.</span><span class="sxs-lookup"><span data-stu-id="b6c96-200">A command-line task that uses a bash script to replace the *RegistryURL* occurrence in the docker-compose.yml file with the RegistryURL variable.</span></span> 
    
          ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

          ![Visual Studio Team Services - frissítés Compose fájl beállításjegyzék URL-címe](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

      2. <span data-ttu-id="b6c96-202">A parancssori feladatot, amely egy bash parancsfájlt használ a *AgentURL* a docker-compose.yml fájlt a AgentURL változóval előfordulása.</span><span class="sxs-lookup"><span data-stu-id="b6c96-202">A command-line task that uses a bash script to replace the *AgentURL* occurrence in the docker-compose.yml file with the AgentURL variable.</span></span>
  
          ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

     3. <span data-ttu-id="b6c96-203">Ez a feladat elutasítja azokat a frissített Compose fájlt egy összeállítási összetevő, így használható a kiadásban.</span><span class="sxs-lookup"><span data-stu-id="b6c96-203">A task that drops the updated Compose file as a build artifact so it can be used in the release.</span></span> <span data-ttu-id="b6c96-204">További részletek a következő képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b6c96-204">See the following screen for details.</span></span>

         ![A Visual Studio Team Services - összetevő közzététele](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

         ![A Visual Studio Team Services - Compose közzététele fájl](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. <span data-ttu-id="b6c96-207">Kattintson a **mentés és a feldolgozási sor** a build definition teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b6c96-207">Click **Save & queue** to test your build definition.</span></span>

   ![Visual Studio Team Services - mentés- és várólista](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![A Visual Studio Team Services - új sor](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. <span data-ttu-id="b6c96-210">Ha a **Build** helyes, a képernyő jelenik meg, hogy:</span><span class="sxs-lookup"><span data-stu-id="b6c96-210">If the **Build** is correct, you have to see this screen:</span></span>

  ![A Visual Studio Team Services - Build sikeres volt.](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-the-release-definition"></a><span data-ttu-id="b6c96-212">3. lépés: A kiadási definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6c96-212">Step 3: Create the release definition</span></span>

<span data-ttu-id="b6c96-213">A Visual Studio Team Services lehetővé teszi [kiadásokban kezelése környezetek között](https://www.visualstudio.com/team-services/release-management/).</span><span class="sxs-lookup"><span data-stu-id="b6c96-213">Visual Studio Team Services allows you to [manage releases across environments](https://www.visualstudio.com/team-services/release-management/).</span></span> <span data-ttu-id="b6c96-214">Folyamatos üzembe helyezés, győződjön meg arról, hogy az alkalmazás telepítve van a különböző környezetekben (például a fejlesztői, tesztelési, éles üzem előtti és éles) zökkenőmentes úgy is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="b6c96-214">You can enable continuous deployment to make sure that your application is deployed on your different environments (such as dev, test, pre-production, and production) in a smooth way.</span></span> <span data-ttu-id="b6c96-215">Létrehozhat egy olyan környezetben, az Azure tároló szolgáltatás Docker Swarm mód fürt jelöli.</span><span class="sxs-lookup"><span data-stu-id="b6c96-215">You can create an environment that represents your Azure Container Service Docker Swarm Mode cluster.</span></span>

![A Visual Studio Team Services - ACS verzióját](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a><span data-ttu-id="b6c96-217">Eredeti kiadásának beállítása</span><span class="sxs-lookup"><span data-stu-id="b6c96-217">Initial release setup</span></span>

1. <span data-ttu-id="b6c96-218">Egy kiadási definition létrehozásához kattintson a **kiadásokban** > **+ kiadás**</span><span class="sxs-lookup"><span data-stu-id="b6c96-218">To create a release definition, click **Releases** > **+ Release**</span></span>

2. <span data-ttu-id="b6c96-219">Az összetevő-forrás konfigurálásához kattintson **összetevők** > **hivatkozás egy összetevő forrás**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-219">To configure the artifact source, Click **Artifacts** > **Link an artifact source**.</span></span> <span data-ttu-id="b6c96-220">A build, amelyet az előző lépésben megadott itt, csatolja az új kiadási definíciója.</span><span class="sxs-lookup"><span data-stu-id="b6c96-220">Here, link this new release definition to the build that you defined in the previous step.</span></span> <span data-ttu-id="b6c96-221">Ezt követően a docker-compose.yml fájlt érhető el a kibocsátási folyamatban.</span><span class="sxs-lookup"><span data-stu-id="b6c96-221">After that, the docker-compose.yml file is available in the release process.</span></span>

    ![A Visual Studio Team Services - kiadás összetevők](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. <span data-ttu-id="b6c96-223">A kiadási eseményindító konfigurálásához kattintson **eseményindítók** válassza **folyamatos üzembe helyezés**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-223">To configure the release trigger, click **Triggers** and select **Continuous Deployment**.</span></span> <span data-ttu-id="b6c96-224">Az eseményindító be ugyanazon összetevő forrás.</span><span class="sxs-lookup"><span data-stu-id="b6c96-224">Set the trigger on the same artifact source.</span></span> <span data-ttu-id="b6c96-225">Ez a beállítás biztosítja, hogy egy új kiadási kezdődik, amikor a létrehozás sikeresen befejeződik.</span><span class="sxs-lookup"><span data-stu-id="b6c96-225">This setting ensures that a new release starts when the build completes successfully.</span></span>

    ![A Visual Studio Team Services - kiadás eseményindítók](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. <span data-ttu-id="b6c96-227">A kiadási változók konfigurálásához kattintson **változók** válassza **+ változó** három új változó létrehozása a beállításjegyzék információval: **docker.username**, **docker.password**, és **docker.registry**.</span><span class="sxs-lookup"><span data-stu-id="b6c96-227">To configure the release variables, click **Variables** and select **+Variable** to create three new variables with the info of the registry: **docker.username**, **docker.password**, and **docker.registry**.</span></span> <span data-ttu-id="b6c96-228">Illessze be a beállításjegyzék és a fürt ügynökök DNS értékeit.</span><span class="sxs-lookup"><span data-stu-id="b6c96-228">Paste the values of your Registry and Cluster Agents DNS.</span></span>

    ![Visual Studio Team Services - Build tárház konfigurációja](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > <span data-ttu-id="b6c96-230">Amint az előző képernyőn, kattintson a **zárolási** docker.password jelölőnégyzet.</span><span class="sxs-lookup"><span data-stu-id="b6c96-230">As shown on the previous screen, click the **Lock** checkbox in docker.password.</span></span> <span data-ttu-id="b6c96-231">A beállítás akkor fontos, hogy a jelszó korlátozása.</span><span class="sxs-lookup"><span data-stu-id="b6c96-231">This setting is important to restrict the password.</span></span>
    >

### <a name="define-the-release-workflow"></a><span data-ttu-id="b6c96-232">Adja meg a kiadási munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="b6c96-232">Define the release workflow</span></span>

<span data-ttu-id="b6c96-233">A kiadási munkafolyamat két feladatot hozzáadott tevődik össze.</span><span class="sxs-lookup"><span data-stu-id="b6c96-233">The release workflow is composed of two tasks that you add.</span></span>

1. <span data-ttu-id="b6c96-234">Egy feladat biztonságosan másolja a compose-fájlt egy *telepítése* mappa a Docker Swarm fő csomóponton korábban konfigurált SSH-kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="b6c96-234">Configure a task to securely copy the compose file to a *deploy* folder on the Docker Swarm master node, using the SSH connection you configured previously.</span></span> <span data-ttu-id="b6c96-235">További részletek a következő képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b6c96-235">See the following screen for details.</span></span>
    
    <span data-ttu-id="b6c96-236">Forrásmappa:```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```</span><span class="sxs-lookup"><span data-stu-id="b6c96-236">Source folder: ```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```</span></span>

    ![Visual Studio Team Services - kiadás SCP-je](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. <span data-ttu-id="b6c96-238">Egy második feladat futtatásához bash parancs `docker` és `docker stack deploy` parancsok a fő csomóponton.</span><span class="sxs-lookup"><span data-stu-id="b6c96-238">Configure a second task to execute a bash command to run `docker` and `docker stack deploy` commands on the master node.</span></span> <span data-ttu-id="b6c96-239">További részletek a következő képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b6c96-239">See the following screen for details.</span></span>

    ```docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth```

    ![A Visual Studio Team Services - kiadás Bash](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    <span data-ttu-id="b6c96-241">A fő végrehajtott parancs a Docker parancssori felület és a Docker Compose CLI használja a következő feladatok elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="b6c96-241">The command executed on the master uses the Docker CLI and the Docker-Compose CLI to do the following tasks:</span></span>

    - <span data-ttu-id="b6c96-242">Jelentkezzen be az Azure-tárolót beállításjegyzék (definiált három build-változókat használ a **változók** lapon)</span><span class="sxs-lookup"><span data-stu-id="b6c96-242">Log in to the Azure container registry (it uses three build variables that are defined in the **Variables** tab)</span></span>
    - <span data-ttu-id="b6c96-243">Adja meg a **DOCKER_HOST** működéséhez a Swarm végponthoz változó (: 2375)</span><span class="sxs-lookup"><span data-stu-id="b6c96-243">Define the **DOCKER_HOST** variable to work with the Swarm endpoint (:2375)</span></span>
    - <span data-ttu-id="b6c96-244">Keresse meg a *telepítése* , amely az előző biztonságos másolási feladat hozta létre, és a docker-compose.yml fájlt tartalmazó mappa</span><span class="sxs-lookup"><span data-stu-id="b6c96-244">Navigate to the *deploy* folder that was created by the preceding secure copy task and that contains the docker-compose.yml file</span></span> 
    - <span data-ttu-id="b6c96-245">Végrehajtás `docker stack deploy` parancsokat, amelyek az új lemezképet lekéréses és a tárolók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b6c96-245">Execute `docker stack deploy` commands that pull the new images and create the containers.</span></span>

    >[!IMPORTANT]
    > <span data-ttu-id="b6c96-246">Ahogy az az előző képernyőre, hagyja a **stderr-en sikertelen** jelölőnégyzet nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="b6c96-246">As shown on the previous screen, leave the **Fail on STDERR** checkbox unchecked.</span></span> <span data-ttu-id="b6c96-247">Ez a beállítás lehetővé teszi a kiadási befejezéséhez miatt `docker-compose` több diagnosztikai üzeneteket, jelenít meg, például a tárolók leállítása vagy törlése, a standard hibák kimenetét.</span><span class="sxs-lookup"><span data-stu-id="b6c96-247">This setting allows us to complete the release process due to `docker-compose` prints several diagnostic messages, such as containers are stopping or being deleted, on the standard error output.</span></span> <span data-ttu-id="b6c96-248">Jelölje be a jelölőnégyzetet, ha a Visual Studio Team Services jelenti, hogy a kiadás során hibák jelentkeztek akkor is, ha minden megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="b6c96-248">If you check the checkbox, Visual Studio Team Services reports that errors occurred during the release, even if all goes well.</span></span>
    >
3. <span data-ttu-id="b6c96-249">Mentse az új kiadási definíciója.</span><span class="sxs-lookup"><span data-stu-id="b6c96-249">Save this new release definition.</span></span>

## <a name="step-4-test-the-cicd-pipeline"></a><span data-ttu-id="b6c96-250">4. lépés: A CI/CD-feldolgozási folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="b6c96-250">Step 4: Test the CI/CD pipeline</span></span>

<span data-ttu-id="b6c96-251">Most, hogy a konfiguráció befejezése után is tesztelheti az új CI/CD adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="b6c96-251">Now that you are done with the configuration, it's time to test this new CI/CD pipeline.</span></span> <span data-ttu-id="b6c96-252">A tesztek legkönnyebben frissítése a forráskódot, és véglegesítse a módosításokat a GitHub-tárházban történő.</span><span class="sxs-lookup"><span data-stu-id="b6c96-252">The easiest way to test it is to update the source code and commit the changes into your GitHub repository.</span></span> <span data-ttu-id="b6c96-253">Néhány másodperccel azután leküldéses a kód jelenik meg a Visual Studio Team Services rendszert futtató új buildverziót.</span><span class="sxs-lookup"><span data-stu-id="b6c96-253">A few seconds after you push the code, you will see a new build running in Visual Studio Team Services.</span></span> <span data-ttu-id="b6c96-254">Sikeres befejezést követően egy új kiadási elindul, és az Azure Tárolószolgáltatás-fürthöz az alkalmazás új verziójának telepítése.</span><span class="sxs-lookup"><span data-stu-id="b6c96-254">Once completed successfully, a new release is triggered and deployed the new version of the application on the Azure Container Service cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6c96-255">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6c96-255">Next steps</span></span>

* <span data-ttu-id="b6c96-256">Visual Studio Team Services CI/CD kapcsolatos további információkért tekintse meg a [VSTS összeállítása – áttekintés](https://www.visualstudio.com/docs/build/overview).</span><span class="sxs-lookup"><span data-stu-id="b6c96-256">For more information about CI/CD with Visual Studio Team Services, see the [VSTS Build overview](https://www.visualstudio.com/docs/build/overview).</span></span>
* <span data-ttu-id="b6c96-257">Az ACS-motor kapcsolatos további információkért tekintse meg a [ACS motor GitHub-tárház](https://github.com/Azure/acs-engine).</span><span class="sxs-lookup"><span data-stu-id="b6c96-257">For more information about ACS Engine, see the [ACS Engine GitHub repo](https://github.com/Azure/acs-engine).</span></span>
* <span data-ttu-id="b6c96-258">Docker Swarm móddal kapcsolatos további információkért lásd: a [Docker Swarm – áttekintés](https://docs.docker.com/engine/swarm/).</span><span class="sxs-lookup"><span data-stu-id="b6c96-258">For more information about Docker Swarm mode, see the [Docker Swarm mode overview](https://docs.docker.com/engine/swarm/).</span></span>
