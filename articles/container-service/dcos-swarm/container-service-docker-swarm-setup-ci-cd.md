---
title: "az Azure Tárolószolgáltatás és Swarm aaaCI/CD |} Microsoft Docs"
description: "Azure Tárolószolgáltatás használata a Docker Swarm, egy Azure-tároló beállításjegyzék és a Visual Studio Team Services toodeliver folyamatosan egy több tároló .NET Core-alkalmazás"
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
ms.openlocfilehash: 35348800aa620469fb62ab3e5a02b33834359446
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="full-cicd-pipeline-toodeploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a><span data-ttu-id="e59f2-104">Teljes CI/CD-feldolgozási folyamat toodeploy egy több tároló alkalmazás Azure tárolószolgáltatás a Docker Swarmmal Visual Studio Team Services használatával</span><span class="sxs-lookup"><span data-stu-id="e59f2-104">Full CI/CD pipeline toodeploy a multi-container application on Azure Container Service with Docker Swarm using Visual Studio Team Services</span></span>

<span data-ttu-id="e59f2-105">Egyik hello legnagyobb kihívást képes toodeliver alatt hello felhő modern alkalmazások fejlesztése során ezeknek az alkalmazásoknak folyamatosan.</span><span class="sxs-lookup"><span data-stu-id="e59f2-105">One of hello biggest challenges when developing modern applications for hello cloud is being able toodeliver these applications continuously.</span></span> <span data-ttu-id="e59f2-106">Ebből a cikkből megtudhatja, hogyan tooimplement egy teljes folyamatos integrációt és a Docker Swarm Azure tároló beállításjegyzék és a Visual Studio Team Services használata az Azure Tárolószolgáltatás (CI/CD) telepítési folyamat készítsen, és kiadáskezelés.</span><span class="sxs-lookup"><span data-stu-id="e59f2-106">In this article, you learn how tooimplement a full continuous integration and deployment (CI/CD) pipeline using Azure Container Service with Docker Swarm, Azure Container Registry, and Visual Studio Team Services build and release management.</span></span>

<span data-ttu-id="e59f2-107">Ez a cikk alapján egy egyszerű alkalmazást, a rendelkezésre álló [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), az ASP.NET Core fejlett.</span><span class="sxs-lookup"><span data-stu-id="e59f2-107">This article is based on a simple application, available on [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), developed with ASP.NET Core.</span></span> <span data-ttu-id="e59f2-108">négy különböző szolgáltatások hello alkalmazások állnak: három webes API-k és egy webes előtér:</span><span class="sxs-lookup"><span data-stu-id="e59f2-108">hello application is composed of four different services: three web APIs and one web front end:</span></span>

![MyShop mintaalkalmazás](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

<span data-ttu-id="e59f2-110">hello cél van toodeliver ezen alkalmazás folyamatosan a Docker Swarm-fürt, a Visual Studio Team Services használatával.</span><span class="sxs-lookup"><span data-stu-id="e59f2-110">hello objective is toodeliver this application continuously in a Docker Swarm cluster, using Visual Studio Team Services.</span></span> <span data-ttu-id="e59f2-111">hello következő részletek az folyamatos kézbesítési adatcsatornát. ábra:</span><span class="sxs-lookup"><span data-stu-id="e59f2-111">hello following figure details this continuous delivery pipeline:</span></span>

![MyShop mintaalkalmazás](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

<span data-ttu-id="e59f2-113">Hello lépéseket rövid leírása itt található:</span><span class="sxs-lookup"><span data-stu-id="e59f2-113">Here is a brief explanation of hello steps:</span></span>

1. <span data-ttu-id="e59f2-114">Kód változtatások véglegesített toohello forráskódraktárban (Itt GitHub)</span><span class="sxs-lookup"><span data-stu-id="e59f2-114">Code changes are committed toohello source code repository (here, GitHub)</span></span> 
2. <span data-ttu-id="e59f2-115">GitHub elindítja a Visual Studio Team Services build</span><span class="sxs-lookup"><span data-stu-id="e59f2-115">GitHub triggers a build in Visual Studio Team Services</span></span> 
3. <span data-ttu-id="e59f2-116">A Visual Studio Team Services lekérdezi hello hello források legújabb verzióját, és hello alkalmazást alkotó összes hello-lemezképet állít össze</span><span class="sxs-lookup"><span data-stu-id="e59f2-116">Visual Studio Team Services gets hello latest version of hello sources and builds all hello images that compose hello application</span></span> 
4. <span data-ttu-id="e59f2-117">A Visual Studio Team Services minden hello Azure tároló beállításjegyzék szolgáltatás használatával létrehozott lemezképet tooa Docker beállításjegyzék leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="e59f2-117">Visual Studio Team Services pushes each image tooa Docker registry created using hello Azure Container Registry service</span></span> 
5. <span data-ttu-id="e59f2-118">A Visual Studio Team Services új verziót váltja ki.</span><span class="sxs-lookup"><span data-stu-id="e59f2-118">Visual Studio Team Services triggers a new release</span></span> 
6. <span data-ttu-id="e59f2-119">hello kiadás néhány SSH használatával fürtcsomóponton hello Azure tároló szolgáltatás fő parancsokat futtat</span><span class="sxs-lookup"><span data-stu-id="e59f2-119">hello release runs some commands using SSH on hello Azure container service cluster master node</span></span> 
7. <span data-ttu-id="e59f2-120">A docker Swarm hello fürt ponttá hello legújabb verziójában hello lemezképek</span><span class="sxs-lookup"><span data-stu-id="e59f2-120">Docker Swarm on hello cluster pulls hello latest version of hello images</span></span> 
8. <span data-ttu-id="e59f2-121">használatával a Docker Compose hello hello alkalmazás új verziója van telepítve</span><span class="sxs-lookup"><span data-stu-id="e59f2-121">hello new version of hello application is deployed using Docker Compose</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e59f2-122">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e59f2-122">Prerequisites</span></span>

<span data-ttu-id="e59f2-123">Az oktatóanyag elindítása előtt kell toocomplete hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="e59f2-123">Before starting this tutorial, you need toocomplete hello following tasks:</span></span>

- [<span data-ttu-id="e59f2-124">Swarm-fürt létrehozása az Azure Container Service-ben</span><span class="sxs-lookup"><span data-stu-id="e59f2-124">Create a Swarm cluster in Azure Container Service</span></span>](container-service-deployment.md)
- [<span data-ttu-id="e59f2-125">Csatlakozás az Azure Tárolószolgáltatásban hello Swarm-fürthöz</span><span class="sxs-lookup"><span data-stu-id="e59f2-125">Connect with hello Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)
- [<span data-ttu-id="e59f2-126">Hozzon létre egy Azure-tárolót beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="e59f2-126">Create an Azure container registry</span></span>](../../container-registry/container-registry-get-started-portal.md)
- [<span data-ttu-id="e59f2-127">Egy Visual Studio Team Services fiók és a team projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="e59f2-127">Have a Visual Studio Team Services account and team project created</span></span>](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [<span data-ttu-id="e59f2-128">Elágazás hello GitHub tárház tooyour GitHub-fiók</span><span class="sxs-lookup"><span data-stu-id="e59f2-128">Fork hello GitHub repository tooyour GitHub account</span></span>](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

<span data-ttu-id="e59f2-129">Egy Ubuntu (14.04 vagy 16.04) gépet is telepítve Docker kell.</span><span class="sxs-lookup"><span data-stu-id="e59f2-129">You also need an Ubuntu (14.04 or 16.04) machine with Docker installed.</span></span> <span data-ttu-id="e59f2-130">Ezen a számítógépen használt Visual Studio Team Services során hello létrehozni, és felszabadíthatja a folyamatokat.</span><span class="sxs-lookup"><span data-stu-id="e59f2-130">This machine is used by Visual Studio Team Services during hello build and release processes.</span></span> <span data-ttu-id="e59f2-131">Egyirányú toocreate ezen a számítógépen toouse hello kép hello elérhető [Azure piactér](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/).</span><span class="sxs-lookup"><span data-stu-id="e59f2-131">One way toocreate this machine is toouse hello image available in hello [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/).</span></span> 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a><span data-ttu-id="e59f2-132">1. lépés: A Visual Studio Team Services-fiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e59f2-132">Step 1: Configure your Visual Studio Team Services account</span></span> 

<span data-ttu-id="e59f2-133">Ebben a szakaszban konfigurálhatja a Visual Studio Team Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="e59f2-133">In this section, you configure your Visual Studio Team Services account.</span></span>

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a><span data-ttu-id="e59f2-134">A Visual Studio Team Services Linux build ügynök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e59f2-134">Configure a Visual Studio Team Services Linux build agent</span></span>

<span data-ttu-id="e59f2-135">toocreate Docker képek és ezeket a lemezképeket egy Azure-tárolót beállításjegyzékbe, a Visual Studio Team Services a build leküldéses, kell tooregister egy Linux-ügynök.</span><span class="sxs-lookup"><span data-stu-id="e59f2-135">toocreate Docker images and push these images into an Azure container registry from a Visual Studio Team Services build, you need tooregister a Linux agent.</span></span> <span data-ttu-id="e59f2-136">Ezen telepítési lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="e59f2-136">You have these installation options:</span></span>

* [<span data-ttu-id="e59f2-137">Egy Linux-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="e59f2-137">Deploy an agent on Linux</span></span>](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [<span data-ttu-id="e59f2-138">Használja a Docker toorun hello VSTS-ügynökök</span><span class="sxs-lookup"><span data-stu-id="e59f2-138">Use Docker toorun hello VSTS agent</span></span>](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-hello-docker-integration-vsts-extension"></a><span data-ttu-id="e59f2-139">Hello Docker integrációs VSTS-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="e59f2-139">Install hello Docker Integration VSTS extension</span></span>

<span data-ttu-id="e59f2-140">A Microsoft a VSTS bővítmény toowork, a Docker építés biztosít, és felszabadíthatja a folyamatokat.</span><span class="sxs-lookup"><span data-stu-id="e59f2-140">Microsoft provides a VSTS extension toowork with Docker in build and release processes.</span></span> <span data-ttu-id="e59f2-141">A bővítmény érhető el hello [VSTS piactér](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker).</span><span class="sxs-lookup"><span data-stu-id="e59f2-141">This extension is available in hello [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker).</span></span> <span data-ttu-id="e59f2-142">Kattintson a **telepítése** tooadd a bővítmény tooyour VSTS-fiókhoz:</span><span class="sxs-lookup"><span data-stu-id="e59f2-142">Click **Install** tooadd this extension tooyour VSTS account:</span></span>

![Hello Docker-integráció telepítése](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

<span data-ttu-id="e59f2-144">Meg kell adnia tooconnect tooyour VSTS fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="e59f2-144">You are asked tooconnect tooyour VSTS account using your credentials.</span></span> 

### <a name="connect-visual-studio-team-services-and-github"></a><span data-ttu-id="e59f2-145">Csatlakoztassa a Visual Studio Team Services, GitHub</span><span class="sxs-lookup"><span data-stu-id="e59f2-145">Connect Visual Studio Team Services and GitHub</span></span>

<span data-ttu-id="e59f2-146">Állítsa be a kapcsolatot a VSTS-projektet és a GitHub-fiók között.</span><span class="sxs-lookup"><span data-stu-id="e59f2-146">Set up a connection between your VSTS project and your GitHub account.</span></span>

1. <span data-ttu-id="e59f2-147">A Visual Studio Team Services projektben kattintson hello **beállítások** ikonra hello eszköztár, és válassza ki a **szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="e59f2-147">In your Visual Studio Team Services project, click hello **Settings** icon in hello toolbar, and select **Services**.</span></span>

    ![A Visual Studio Team Services - külső kapcsolatot](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

2. <span data-ttu-id="e59f2-149">Hello bal oldalon kattintson **új szolgáltatási végpont** > **GitHub**.</span><span class="sxs-lookup"><span data-stu-id="e59f2-149">On hello left, click **New Service Endpoint** > **GitHub**.</span></span>

    ![A Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

3. <span data-ttu-id="e59f2-151">a GitHub-fiókjában, tooauthorize VSTS toowork kattintson **engedélyezés** és hello eljárás a hello ablakban.</span><span class="sxs-lookup"><span data-stu-id="e59f2-151">tooauthorize VSTS toowork with your GitHub account, click **Authorize** and follow hello procedure in hello window that opens.</span></span>

    ![A Visual Studio Team Services - engedélyezik a Githubon](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-tooyour-azure-container-registry-and-azure-container-service-cluster"></a><span data-ttu-id="e59f2-153">Csatlakozás a VSTS tooyour Azure tároló beállításjegyzék és az Azure Tárolószolgáltatás-fürt</span><span class="sxs-lookup"><span data-stu-id="e59f2-153">Connect VSTS tooyour Azure container registry and Azure Container Service cluster</span></span>

<span data-ttu-id="e59f2-154">hello utolsó be hello CI/CD adatcsatorna elérése előtt lépésekre tooconfigure külső kapcsolatok tooyour tároló beállításjegyzék és a Docker Swarm-fürt az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="e59f2-154">hello last steps before getting into hello CI/CD pipeline are tooconfigure external connections tooyour container registry and your Docker Swarm cluster in Azure.</span></span> 

1. <span data-ttu-id="e59f2-155">A hello **szolgáltatások** a Visual Studio Team Services projekt beállítások hozzáadása típusú szolgáltatásvégpont **Docker beállításjegyzék**.</span><span class="sxs-lookup"><span data-stu-id="e59f2-155">In hello **Services** settings of your Visual Studio Team Services project, add a service endpoint of type **Docker Registry**.</span></span> 

2. <span data-ttu-id="e59f2-156">A hello felugró ablakban nyílik meg írja be a hello URL-cím és az Azure-tárolót beállításjegyzék hello hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="e59f2-156">In hello popup that opens, enter hello URL and hello credentials of your Azure container registry.</span></span>

    ![A Visual Studio Team Services - Docker beállításjegyzék](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

3. <span data-ttu-id="e59f2-158">Hello Docker Swarm-fürt, a típusú végpont hozzáadása **SSH**.</span><span class="sxs-lookup"><span data-stu-id="e59f2-158">For hello Docker Swarm cluster, add an endpoint of type **SSH**.</span></span> <span data-ttu-id="e59f2-159">Majd adja meg a hello SSH csatlakozási adatait a Swarm-fürt.</span><span class="sxs-lookup"><span data-stu-id="e59f2-159">Then enter hello SSH connection information of your Swarm cluster.</span></span>

    ![A Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

<span data-ttu-id="e59f2-161">Minden hello konfigurálást most.</span><span class="sxs-lookup"><span data-stu-id="e59f2-161">All hello configuration is done now.</span></span> <span data-ttu-id="e59f2-162">A következő lépések hello hello CI/CD-feldolgozási folyamat létrehozza és telepíti hello alkalmazás toohello Docker Swarm-fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e59f2-162">In hello next steps, you create hello CI/CD pipeline that builds and deploys hello application toohello Docker Swarm cluster.</span></span> 

## <a name="step-2-create-hello-build-definition"></a><span data-ttu-id="e59f2-163">2. lépés: Hello build definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="e59f2-163">Step 2: Create hello build definition</span></span>

<span data-ttu-id="e59f2-164">Ebben a lépésben a VSTS-projekt egy build definitionfor beállításában és hello létrehozási munkafolyamat megadása a tároló lemezképek</span><span class="sxs-lookup"><span data-stu-id="e59f2-164">In this step, you set up a build definitionfor your VSTS project and define hello build workflow for your container images</span></span>

### <a name="initial-definition-setup"></a><span data-ttu-id="e59f2-165">Kezdeti beállításának</span><span class="sxs-lookup"><span data-stu-id="e59f2-165">Initial definition setup</span></span>

1. <span data-ttu-id="e59f2-166">toocreate build definícióját, tooyour Visual Studio Team Services projektre, és kattintson a csatlakozás **Build & kiadási**.</span><span class="sxs-lookup"><span data-stu-id="e59f2-166">toocreate a build definition, connect tooyour Visual Studio Team Services project and click **Build & Release**.</span></span> 

2. <span data-ttu-id="e59f2-167">A hello **definíciók Build** kattintson **+ új**.</span><span class="sxs-lookup"><span data-stu-id="e59f2-167">In hello **Build definitions** section, click **+ New**.</span></span> <span data-ttu-id="e59f2-168">Jelölje be hello **üres** sablont.</span><span class="sxs-lookup"><span data-stu-id="e59f2-168">Select hello **Empty** template.</span></span>

    ![A Visual Studio Team Services - új definíció létrehozása](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

3. <span data-ttu-id="e59f2-170">Új build hello konfigurálása forrással GitHub tárház, ellenőrizze **folyamatos integrációt**, és jelölje be hello ügynök várólista ahol regisztrálták-e a Linux-ügynök.</span><span class="sxs-lookup"><span data-stu-id="e59f2-170">Configure hello new build with a GitHub repository source, check **Continuous integration**, and select hello agent queue where you registered your Linux agent.</span></span> <span data-ttu-id="e59f2-171">Kattintson a **létrehozása** toocreate hello build definíciója.</span><span class="sxs-lookup"><span data-stu-id="e59f2-171">Click **Create** toocreate hello build definition.</span></span>

    ![A Visual Studio Team Services - Build-definíció létrehozása](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

4. <span data-ttu-id="e59f2-173">A hello **Build definíciók** lapon, először megnyitja az hello **tárház** lapra, és hello build toouse hello elágazás hello MyShop projekt a hello Előfeltételek konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e59f2-173">On hello **Build Definitions** page, first open hello **Repository** tab and configure hello build toouse hello fork of hello MyShop project that you created in hello prerequisites.</span></span> <span data-ttu-id="e59f2-174">Győződjön meg arról, hogy kiválassza *acs-dokumentumok* , hello **alapértelmezett ágat**.</span><span class="sxs-lookup"><span data-stu-id="e59f2-174">Make sure that you select *acs-docs* as hello **Default branch**.</span></span>

    ![Visual Studio Team Services - Build tárház konfigurációja](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

5. <span data-ttu-id="e59f2-176">A hello **eseményindítók** lapján minden véglegesítés után elindított hello build toobe konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e59f2-176">On hello **Triggers** tab, configure hello build toobe triggered after each commit.</span></span> <span data-ttu-id="e59f2-177">Válassza ki **folyamatos integrációt** és **módosítások kötegelt**.</span><span class="sxs-lookup"><span data-stu-id="e59f2-177">Select **Continuous integration** and **Batch changes**.</span></span>

    ![Visual Studio Team Services - Build eseményindító konfigurációja](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-hello-build-workflow"></a><span data-ttu-id="e59f2-179">Adja meg a hello létrehozási munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="e59f2-179">Define hello build workflow</span></span>
<span data-ttu-id="e59f2-180">hello lépések hello létrehozási munkafolyamat határozza meg.</span><span class="sxs-lookup"><span data-stu-id="e59f2-180">hello next steps define hello build workflow.</span></span> <span data-ttu-id="e59f2-181">Nincsenek hello öt tároló képek toobuild *MyShop* alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e59f2-181">There are five container images toobuild for hello *MyShop* application.</span></span> <span data-ttu-id="e59f2-182">Minden egyes lemezképének összeállítása a hello projekt mappákban lévő Dockerfile hello használata:</span><span class="sxs-lookup"><span data-stu-id="e59f2-182">Each image is built using hello Dockerfile located in hello project folders:</span></span>

* <span data-ttu-id="e59f2-183">ProductsApi</span><span class="sxs-lookup"><span data-stu-id="e59f2-183">ProductsApi</span></span>
* <span data-ttu-id="e59f2-184">Proxy</span><span class="sxs-lookup"><span data-stu-id="e59f2-184">Proxy</span></span>
* <span data-ttu-id="e59f2-185">RatingsApi</span><span class="sxs-lookup"><span data-stu-id="e59f2-185">RatingsApi</span></span>
* <span data-ttu-id="e59f2-186">RecommandationsApi</span><span class="sxs-lookup"><span data-stu-id="e59f2-186">RecommandationsApi</span></span>
* <span data-ttu-id="e59f2-187">ShopFront</span><span class="sxs-lookup"><span data-stu-id="e59f2-187">ShopFront</span></span>

<span data-ttu-id="e59f2-188">Tooadd két Docker lépéseket az egyes lemezképek, egy toobuild hello lemezképét és egy toopush hello kép hello Azure tároló beállításjegyzék van szüksége.</span><span class="sxs-lookup"><span data-stu-id="e59f2-188">You need tooadd two Docker steps for each image, one toobuild hello image, and one toopush hello image in hello Azure container registry.</span></span> 

1. <span data-ttu-id="e59f2-189">tooadd hello létrehozási munkafolyamat, egy lépésben kattintson **+ Hozzáadás összeállítása lépés** válassza **Docker**.</span><span class="sxs-lookup"><span data-stu-id="e59f2-189">tooadd a step in hello build workflow, click **+ Add build step** and select **Docker**.</span></span>

    ![A Visual Studio Team Services - Build lépéseket adhat hozzá.](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

2. <span data-ttu-id="e59f2-191">Az egyes lemezképek konfigurálása egy lépésben hello használó `docker build` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e59f2-191">For each image, configure one step that uses hello `docker build` command.</span></span>

    ![A Visual Studio Team Services - Docker-Build](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    <span data-ttu-id="e59f2-193">Hello művelet létrehozni, jelölje be az Azure-tárolót beállításjegyzék hello **lemezképet készít** művelet, és egyes lemezképek definiáló Dockerfile hello.</span><span class="sxs-lookup"><span data-stu-id="e59f2-193">For hello build operation, select your Azure container registry, hello **Build an image** action, and hello Dockerfile that defines each image.</span></span> <span data-ttu-id="e59f2-194">Set hello **összeállítási környezet** hello Dockerfile, gyökérkönyvtár, és adja meg a hello **Lemezképnév**.</span><span class="sxs-lookup"><span data-stu-id="e59f2-194">Set hello **Build context** as hello Dockerfile root directory, and define hello **Image Name**.</span></span> 
    
    <span data-ttu-id="e59f2-195">Amint a képernyő megelőző hello, hello kép neve kezdődhet hello URI-azonosítója az Azure-tárolót beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="e59f2-195">As shown on hello preceding screen, start hello image name with hello URI of your Azure container registry.</span></span> <span data-ttu-id="e59f2-196">(A build változó tooparameterize hello címke hello kép, például a hello build azonosítója ebben a példában is használhatja.)</span><span class="sxs-lookup"><span data-stu-id="e59f2-196">(You can also use a build variable tooparameterize hello tag of hello image, such as hello build identifier in this example.)</span></span>

3. <span data-ttu-id="e59f2-197">Az egyes lemezképek beállítása egy második lépésben hello használó `docker push` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e59f2-197">For each image, configure a second step that uses hello `docker push` command.</span></span>

    ![A Visual Studio Team Services - Docker leküldéses](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    <span data-ttu-id="e59f2-199">Hello leküldéses művelethez, válassza ki az Azure-tárolót beállításjegyzék hello **lemezkép leküldéses** művelet, és írja be a hello **Lemezképnév** hello előző lépésben épül, amely.</span><span class="sxs-lookup"><span data-stu-id="e59f2-199">For hello push operation, select your Azure container registry, hello **Push an image** action, and enter hello **Image Name** that is built in hello previous step.</span></span>

4. <span data-ttu-id="e59f2-200">Hello build konfigurálását követően, és leküldéses lépéseket az egyes hello öt képek, két további lépéseket a hello build munkafolyamat hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="e59f2-200">After you configure hello build and push steps for each of hello five images, add two more steps in hello build workflow.</span></span>

    <span data-ttu-id="e59f2-201">a.</span><span class="sxs-lookup"><span data-stu-id="e59f2-201">a.</span></span> <span data-ttu-id="e59f2-202">A parancssori feladatot, amely használja a bash parancsfájlok tooreplace hello *BuildNumber* hello docker-compose.yml fájlt az aktuális hello számainak létrehozása azonosítóját. Tekintse meg a következő képernyő Részletek hello.</span><span class="sxs-lookup"><span data-stu-id="e59f2-202">A command-line task that uses a bash script tooreplace hello *BuildNumber* occurrence in hello docker-compose.yml file with hello current build Id. See hello following screen for details.</span></span>

    ![A Visual Studio Team Services - frissítés Compose fájl](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    <span data-ttu-id="e59f2-204">b.</span><span class="sxs-lookup"><span data-stu-id="e59f2-204">b.</span></span> <span data-ttu-id="e59f2-205">Egy feladatot, amely hello esik Compose fájl frissíteni, mivel a egy összeállítási összetevő így hello verzióban is használható.</span><span class="sxs-lookup"><span data-stu-id="e59f2-205">A task that drops hello updated Compose file as a build artifact so it can be used in hello release.</span></span> <span data-ttu-id="e59f2-206">Tekintse meg a következő képernyő Részletek hello.</span><span class="sxs-lookup"><span data-stu-id="e59f2-206">See hello following screen for details.</span></span>

    ![A Visual Studio Team Services - Compose közzététele fájl](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

5. <span data-ttu-id="e59f2-208">Kattintson a **mentése** és a build definition neve.</span><span class="sxs-lookup"><span data-stu-id="e59f2-208">Click **Save** and name your build definition.</span></span>

## <a name="step-3-create-hello-release-definition"></a><span data-ttu-id="e59f2-209">3. lépés: Hello kiadás definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="e59f2-209">Step 3: Create hello release definition</span></span>

<span data-ttu-id="e59f2-210">A Visual Studio Team Services lehetővé teszi túl[kiadásokban kezelése környezetek között](https://www.visualstudio.com/team-services/release-management/).</span><span class="sxs-lookup"><span data-stu-id="e59f2-210">Visual Studio Team Services allows you too[manage releases across environments](https://www.visualstudio.com/team-services/release-management/).</span></span> <span data-ttu-id="e59f2-211">Folyamatos üzembe helyezés toomake meg arról, hogy az alkalmazás telepítve van a különböző környezetekben (például a fejlesztői, tesztelési, éles üzem előtti és éles) zökkenőmentes úgy is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="e59f2-211">You can enable continuous deployment toomake sure that your application is deployed on your different environments (such as dev, test, pre-production, and production) in a smooth way.</span></span> <span data-ttu-id="e59f2-212">Létrehozhat egy új környezetben, amely az Azure tároló szolgáltatás Docker Swarm-fürt jelöli.</span><span class="sxs-lookup"><span data-stu-id="e59f2-212">You can create a new environment that represents your Azure Container Service Docker Swarm cluster.</span></span>

![A Visual Studio Team Services - kiadás tooACS](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a><span data-ttu-id="e59f2-214">Eredeti kiadásának beállítása</span><span class="sxs-lookup"><span data-stu-id="e59f2-214">Initial release setup</span></span>

1. <span data-ttu-id="e59f2-215">toocreate kiadás definícióját, kattintson a **kiadásokban** > **+ kiadás**</span><span class="sxs-lookup"><span data-stu-id="e59f2-215">toocreate a release definition, click **Releases** > **+ Release**</span></span>

2. <span data-ttu-id="e59f2-216">tooconfigure hello összetevő forrás, kattintson **összetevők** > **hivatkozás egy összetevő forrás**.</span><span class="sxs-lookup"><span data-stu-id="e59f2-216">tooconfigure hello artifact source, Click **Artifacts** > **Link an artifact source**.</span></span> <span data-ttu-id="e59f2-217">Itt csatolja az új kiadási definition toohello build hello előző lépésben meghatározott.</span><span class="sxs-lookup"><span data-stu-id="e59f2-217">Here, link this new release definition toohello build that you defined in hello previous step.</span></span> <span data-ttu-id="e59f2-218">Ezzel az eljárással hello docker-compose.yml fájlt érhető hello kiadás során.</span><span class="sxs-lookup"><span data-stu-id="e59f2-218">By doing this, hello docker-compose.yml file is available in hello release process.</span></span>

    ![A Visual Studio Team Services - kiadás összetevők](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

3. <span data-ttu-id="e59f2-220">tooconfigure hello kiadás eseményindító, kattintson a **eseményindítók** válassza **folyamatos üzembe helyezés**.</span><span class="sxs-lookup"><span data-stu-id="e59f2-220">tooconfigure hello release trigger, click **Triggers** and select **Continuous Deployment**.</span></span> <span data-ttu-id="e59f2-221">Hello beállítása hello eseményindítón ugyanazon összetevő-forrás.</span><span class="sxs-lookup"><span data-stu-id="e59f2-221">Set hello trigger on hello same artifact source.</span></span> <span data-ttu-id="e59f2-222">Ez a beállítás biztosítja, hogy az új kiadási elindul, amint hello build sikeresen befejeződik.</span><span class="sxs-lookup"><span data-stu-id="e59f2-222">This setting ensures that a new release starts as soon as hello build completes successfully.</span></span>

    ![A Visual Studio Team Services - kiadás eseményindítók](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-hello-release-workflow"></a><span data-ttu-id="e59f2-224">Adja meg a hello kiadás munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="e59f2-224">Define hello release workflow</span></span>

<span data-ttu-id="e59f2-225">hello kiadás munkafolyamat két feladatot hozzáadott tevődik össze.</span><span class="sxs-lookup"><span data-stu-id="e59f2-225">hello release workflow is composed of two tasks that you add.</span></span>

1. <span data-ttu-id="e59f2-226">Egy feladat toosecurely másolási hello konfigurálása állítható össze a fájl tooa *telepítése* mappa a hello Docker Swarm fő csomóponton korábban konfigurált hello SSH-kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="e59f2-226">Configure a task toosecurely copy hello compose file tooa *deploy* folder on hello Docker Swarm master node, using hello SSH connection you configured previously.</span></span> <span data-ttu-id="e59f2-227">Tekintse meg a következő képernyő Részletek hello.</span><span class="sxs-lookup"><span data-stu-id="e59f2-227">See hello following screen for details.</span></span>

    ![Visual Studio Team Services - kiadás SCP-je](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

2. <span data-ttu-id="e59f2-229">Egy második feladat tooexecute a bash parancs toorun konfigurálása `docker` és `docker-compose` parancsok hello fő csomóponton.</span><span class="sxs-lookup"><span data-stu-id="e59f2-229">Configure a second task tooexecute a bash command toorun `docker` and `docker-compose` commands on hello master node.</span></span> <span data-ttu-id="e59f2-230">Tekintse meg a következő képernyő Részletek hello.</span><span class="sxs-lookup"><span data-stu-id="e59f2-230">See hello following screen for details.</span></span>

    ![A Visual Studio Team Services - kiadás Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    <span data-ttu-id="e59f2-232">hello parancs hello fő használata hello Docker parancssori felületén és hello Docker Compose CLI toodo hello követő feladatok végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="e59f2-232">hello command executed on hello master use hello Docker CLI and hello Docker-Compose CLI toodo hello following tasks:</span></span>

    - <span data-ttu-id="e59f2-233">Bejelentkezési toohello Azure tároló beállításjegyzék (három build variab'les hello definiált használ **változók** lapon)</span><span class="sxs-lookup"><span data-stu-id="e59f2-233">Login toohello Azure container registry (it uses three build variab\`les that are defined in hello **Variables** tab)</span></span>
    - <span data-ttu-id="e59f2-234">Adja meg a hello **DOCKER_HOST** hello Swarm végponthoz változó toowork (: 2375)</span><span class="sxs-lookup"><span data-stu-id="e59f2-234">Define hello **DOCKER_HOST** variable toowork with hello Swarm endpoint (:2375)</span></span>
    - <span data-ttu-id="e59f2-235">Keresse meg a toohello *telepítése* , amelyek biztonságos másolási feladat megelőző hello hozta létre, és hello docker-compose.yml fájlt tartalmazó mappa</span><span class="sxs-lookup"><span data-stu-id="e59f2-235">Navigate toohello *deploy* folder that was created by hello preceding secure copy task and that contains hello docker-compose.yml file</span></span> 
    - <span data-ttu-id="e59f2-236">Végrehajtás `docker-compose` parancsokat, amelyek hello új képek, lekéréses hello szolgáltatásainak leállítása, hello szolgáltatások eltávolítása és hello tárolók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e59f2-236">Execute `docker-compose` commands that pull hello new images, stop hello services, remove hello services, and create hello containers.</span></span>

    >[!IMPORTANT]
    > <span data-ttu-id="e59f2-237">Amint a képernyő megelőző hello, hagyja hello **stderr-en sikertelen** jelölőnégyzet nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="e59f2-237">As shown on hello preceding screen, leave hello **Fail on STDERR** checkbox unchecked.</span></span> <span data-ttu-id="e59f2-238">Ez az egy fontos beállítása, mert `docker-compose` több diagnosztikai üzeneteket, jelenít meg, például a tárolók leállítása vagy törlése, a standard hiba kimeneti hello.</span><span class="sxs-lookup"><span data-stu-id="e59f2-238">This is an important setting, because `docker-compose` prints several diagnostic messages, such as containers are stopping or being deleted, on hello standard error output.</span></span> <span data-ttu-id="e59f2-239">Ha hello jelölőnégyzetet a Visual Studio Team Services jelenti, hogy hello kiadás során hibák jelentkeztek akkor is, ha minden megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="e59f2-239">If you check hello checkbox, Visual Studio Team Services reports that errors occurred during hello release, even if all goes well.</span></span>
    >
3. <span data-ttu-id="e59f2-240">Mentse az új kiadási definíciója.</span><span class="sxs-lookup"><span data-stu-id="e59f2-240">Save this new release definition.</span></span>


>[!NOTE]
><span data-ttu-id="e59f2-241">A központi telepítés tartalmazza a némi állásidővel, mivel azt hello régi szolgáltatások leállítása és hello újat futtatja.</span><span class="sxs-lookup"><span data-stu-id="e59f2-241">This deployment includes some downtime because we are stopping hello old services and running hello new one.</span></span> <span data-ttu-id="e59f2-242">Azt az lehetséges tooavoid Ez egy kék-zöld telepítési végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="e59f2-242">It is possible tooavoid this by doing a blue-green deployment.</span></span>
>

## <a name="step-4-test-hello-cicd-pipeline"></a><span data-ttu-id="e59f2-243">4. lépés</span><span class="sxs-lookup"><span data-stu-id="e59f2-243">Step 4.</span></span> <span data-ttu-id="e59f2-244">Hello CI/CD folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="e59f2-244">Test hello CI/CD pipeline</span></span>

<span data-ttu-id="e59f2-245">Most, hogy hello konfiguráció befejezése után a rendszer idő tootest Ez új CI/CD folyamat.</span><span class="sxs-lookup"><span data-stu-id="e59f2-245">Now that you are done with hello configuration, it's time tootest this new CI/CD pipeline.</span></span> <span data-ttu-id="e59f2-246">a GitHub-tárház hello tooupdate hello forrás kódot, és aztán hello legegyszerűbb módja tootest változik.</span><span class="sxs-lookup"><span data-stu-id="e59f2-246">hello easiest way tootest it is tooupdate hello source code and commit hello changes into your GitHub repository.</span></span> <span data-ttu-id="e59f2-247">Néhány másodperccel azután leküldéses hello kód jelenik meg a Visual Studio Team Services rendszert futtató új buildverziót.</span><span class="sxs-lookup"><span data-stu-id="e59f2-247">A few seconds after you push hello code, you will see a new build running in Visual Studio Team Services.</span></span> <span data-ttu-id="e59f2-248">Sikeres befejezést követően az új kiadási indul, és hello hello Azure Tárolószolgáltatás-fürt hello alkalmazás új verzióját telepíti.</span><span class="sxs-lookup"><span data-stu-id="e59f2-248">Once completed successfully, a new release will be triggered and will deploy hello new version of hello application on hello Azure Container Service cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e59f2-249">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e59f2-249">Next Steps</span></span>

* <span data-ttu-id="e59f2-250">Visual Studio Team Services CI/CD kapcsolatos további információkért lásd: hello [VSTS összeállítása – áttekintés](https://www.visualstudio.com/docs/build/overview).</span><span class="sxs-lookup"><span data-stu-id="e59f2-250">For more information about CI/CD with Visual Studio Team Services, see hello [VSTS Build overview](https://www.visualstudio.com/docs/build/overview).</span></span>
