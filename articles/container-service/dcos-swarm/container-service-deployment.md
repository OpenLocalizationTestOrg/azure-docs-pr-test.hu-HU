---
title: "egy Docker-tároló aaaDeploy fürtön, az Azure-ban |} Microsoft Docs"
description: "Az Azure Tárolószolgáltatásban Kubernetes, DC/OS- vagy Docker Swarm megoldás telepítése hello Azure-portálon vagy a Resource Manager-sablon használatával."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Mesos, Azure, dcos, swarm, kubernetes, azure container service, acs"
ms.service: container-service
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: rogardle
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 26e3a7d0af9d71acd8b5c85fd667fcf7d84cef66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-docker-container-hosting-solution-using-hello-azure-portal"></a><span data-ttu-id="c19ff-104">Egy Docker-tároló üzemeltetési megoldás használatával hello Azure-portál telepítése</span><span class="sxs-lookup"><span data-stu-id="c19ff-104">Deploy a Docker container hosting solution using hello Azure portal</span></span>



<span data-ttu-id="c19ff-105">Az Azure tárolószolgáltatással gyorsan üzembe helyezhet népszerű nyílt forráskódú tárolófürtözési és vezénylési megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="c19ff-105">Azure Container Service provides rapid deployment of popular open-source container clustering and orchestration solutions.</span></span> <span data-ttu-id="c19ff-106">Ez a dokumentum végigvezeti az Azure Tárolószolgáltatás-fürt telepítése hello Azure-portálon vagy az Azure Resource Manager gyorsindítási sablonon használatával.</span><span class="sxs-lookup"><span data-stu-id="c19ff-106">This document walks you through deploying an Azure Container Service cluster by using hello Azure portal or an Azure Resource Manager quickstart template.</span></span> 

<span data-ttu-id="c19ff-107">Az Azure Tárolószolgáltatás-fürt hello segítségével is telepíthet [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) vagy hello Azure tároló szolgáltatás API-k.</span><span class="sxs-lookup"><span data-stu-id="c19ff-107">You can also deploy an Azure Container Service cluster by using hello [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) or hello Azure Container Service APIs.</span></span>

<span data-ttu-id="c19ff-108">Háttér-információk: [Az Azure Container Service bemutatása](../container-service-intro.md).</span><span class="sxs-lookup"><span data-stu-id="c19ff-108">For background, see [Azure Container Service introduction](../container-service-intro.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="c19ff-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c19ff-109">Prerequisites</span></span>

* <span data-ttu-id="c19ff-110">**Azure-előfizetés**: Ha nem rendelkezik előfizetéssel, regisztrálhat az [ingyenes próbaverzióra](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span><span class="sxs-lookup"><span data-stu-id="c19ff-110">**Azure subscription**: If you don't have one, sign up for a [free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span></span> <span data-ttu-id="c19ff-111">Nagyobb fürt esetében fontolja meg a használatalapú előfizetést vagy az egyéb fizetési lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="c19ff-111">For a larger cluster, consider a pay-as-you go subscription or other purchase options.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c19ff-112">Az Azure-előfizetés használatának és [erőforráskvótákkal](../../azure-subscription-service-limits.md), például magok kvótákat, az korlátozhatja hello fürt központi telepítése hello méretét.</span><span class="sxs-lookup"><span data-stu-id="c19ff-112">Your Azure subscription usage and [resource quotas](../../azure-subscription-service-limits.md), such as cores quotas, can limit hello size of hello cluster you deploy.</span></span> <span data-ttu-id="c19ff-113">a kvóta növelését, nyissa meg toorequest egy [online felhasználói támogatási kérelem](../../azure-supportability/how-to-create-azure-support-request.md) díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="c19ff-113">toorequest a quota increase, open an [online customer support request](../../azure-supportability/how-to-create-azure-support-request.md) at no charge.</span></span>
    >

* <span data-ttu-id="c19ff-114">**SSH-RSA nyilvános kulcs**: hello portal vagy hello Azure gyors üzembe helyezés sablonok egyikét telepítésekor tooprovide hello nyilvános kulcsot a hitelesítéshez az Azure Tárolószolgáltatás virtuális gépek elleni szükség van.</span><span class="sxs-lookup"><span data-stu-id="c19ff-114">**SSH RSA public key**: When deploying through hello portal or one of hello Azure quickstart templates, you need tooprovide hello public key for authentication against Azure Container Service virtual machines.</span></span> <span data-ttu-id="c19ff-115">Secure Shell (SSH) RSA-kulcsok toocreate, lásd: hello [OS X- és Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) vagy [Windows](../../virtual-machines/linux/ssh-from-windows.md) útmutatást.</span><span class="sxs-lookup"><span data-stu-id="c19ff-115">toocreate Secure Shell (SSH) RSA keys, see hello [OS X and Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) or [Windows](../../virtual-machines/linux/ssh-from-windows.md) guidance.</span></span> 

* <span data-ttu-id="c19ff-116">**Szolgáltatás egyszerű ügyfél-azonosító és a titkos kulcs** (csak Kubernetes): további információt és útmutatást toocreate egy Azure Active Directory szolgáltatás egyszerű, lásd: [kapcsolatos hello szolgáltatás egyszerű Kubernetes fürt](../kubernetes/container-service-kubernetes-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="c19ff-116">**Service principal client ID and secret** (Kubernetes only): For more information and guidance toocreate an Azure Active Directory service principal, see [About hello service principal for a Kubernetes cluster](../kubernetes/container-service-kubernetes-service-principal.md).</span></span>



## <a name="create-a-cluster-by-using-hello-azure-portal"></a><span data-ttu-id="c19ff-117">Hozzon létre egy fürtöt hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="c19ff-117">Create a cluster by using hello Azure portal</span></span>
1. <span data-ttu-id="c19ff-118">Jelentkezzen be Azure-portálon toohello válasszon **új**, és az Azure piactér hello keresési **Azure Tárolószolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="c19ff-118">Sign in toohello Azure portal, select **New**, and search hello Azure Marketplace for **Azure Container Service**.</span></span>

    ![Az Azure Container Service a Marketplace-en](./media/container-service-deployment/acs-portal1.png)  <br />

2. <span data-ttu-id="c19ff-120">Kattintson az **Azure Container Service** elemre, majd kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c19ff-120">Click **Azure Container Service**, and click **Create**.</span></span>

3. <span data-ttu-id="c19ff-121">A hello **alapjai** panelen adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="c19ff-121">On hello **Basics** blade, enter hello following information:</span></span>

    * <span data-ttu-id="c19ff-122">**Az orchestrator**: Válasszon ki egy hello tároló orchestrators toodeploy hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="c19ff-122">**Orchestrator**: Select one of hello container orchestrators toodeploy on hello cluster.</span></span>
        * <span data-ttu-id="c19ff-123">**DC/OS**: DC/OS fürt üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="c19ff-123">**DC/OS**: Deploys a DC/OS cluster.</span></span>
        * <span data-ttu-id="c19ff-124">**Swarm**: Docker Swarm-fürt üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="c19ff-124">**Swarm**: Deploys a Docker Swarm cluster.</span></span>
        * <span data-ttu-id="c19ff-125">**Kubernetes**: Kubernetes-fürt üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="c19ff-125">**Kubernetes**: Deploys a Kubernetes cluster.</span></span>
    * <span data-ttu-id="c19ff-126">**Előfizetés**: válasszon ki egy Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="c19ff-126">**Subscription**: Select an Azure subscription.</span></span>
    * <span data-ttu-id="c19ff-127">**Erőforráscsoport**: hello központi telepítés hello egy új erőforráscsoport nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="c19ff-127">**Resource group**: Enter hello name of a new resource group for hello deployment.</span></span>
    * <span data-ttu-id="c19ff-128">**Hely**: válassza ki a kívánt Azure-régiót hello Azure Tárolószolgáltatás-telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="c19ff-128">**Location**: Select an Azure region for hello Azure Container Service deployment.</span></span> <span data-ttu-id="c19ff-129">Az elérhetőségért tekintse meg a [Régiónként elérhető termékek](https://azure.microsoft.com/regions/services/) listáját.</span><span class="sxs-lookup"><span data-stu-id="c19ff-129">For availability, check [Products available by region](https://azure.microsoft.com/regions/services/).</span></span>
    
    ![Alapbeállítások](./media/container-service-deployment/acs-portal3.png)  <br />
    
    <span data-ttu-id="c19ff-131">Kattintson a **OK** készen tooproceed közben.</span><span class="sxs-lookup"><span data-stu-id="c19ff-131">Click **OK** when you're ready tooproceed.</span></span>

4. <span data-ttu-id="c19ff-132">A hello **konfigurációs fő** panelen adja meg a következő beállítások hello Linux főcsomópont vagy (egyes beállítások, amelyek adott tooeach orchestrator) hello fürt csomópontjai hello:</span><span class="sxs-lookup"><span data-stu-id="c19ff-132">On hello **Master configuration** blade, enter hello following settings for hello Linux master node or nodes in hello cluster (some settings are specific tooeach orchestrator):</span></span>

    * <span data-ttu-id="c19ff-133">**Fő DNS-név**: hello használt előtag toocreate egy egyedi teljesen minősített tartományneve (FQDN) hello főkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="c19ff-133">**Master DNS name**: hello prefix used toocreate a unique fully qualified domain name (FQDN) for hello master.</span></span> <span data-ttu-id="c19ff-134">fő FQDN-je hello űrlap hello *előtag*felügyeleti*hely*. cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="c19ff-134">hello master FQDN is of hello form *prefix*mgmt.*location*.cloudapp.azure.com.</span></span>
    * <span data-ttu-id="c19ff-135">**Felhasználónév**: egy fiók egyes hello Linux virtuális gépek hello fürt hello felhasználónevét.</span><span class="sxs-lookup"><span data-stu-id="c19ff-135">**User name**: hello user name for an account on each of hello Linux virtual machines in hello cluster.</span></span>
    * <span data-ttu-id="c19ff-136">**SSH-RSA nyilvános kulcs**: hello nyilvános kulcs toobe hello Linux virtuális gépek elleni hitelesítéshez használt hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="c19ff-136">**SSH RSA public key**: Add hello public key toobe used for authentication against hello Linux virtual machines.</span></span> <span data-ttu-id="c19ff-137">Fontos, hogy ezt a kulcsot ne tartalmazzon sortörést, és ez magában foglalja a hello `ssh-rsa` előtag.</span><span class="sxs-lookup"><span data-stu-id="c19ff-137">It is important that this key contains no line breaks, and it includes hello `ssh-rsa` prefix.</span></span> <span data-ttu-id="c19ff-138">Hello `username@domain` utótag megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="c19ff-138">hello `username@domain` postfix is optional.</span></span> <span data-ttu-id="c19ff-139">hello kulcs hasonlóan kell kinéznie hello alábbi: **ssh-rsa AAAAB3Nz... <>...... UcyupgH azureuser@linuxvm** .</span><span class="sxs-lookup"><span data-stu-id="c19ff-139">hello key should look something like hello following: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**.</span></span> 
    * <span data-ttu-id="c19ff-140">**Szolgáltatás egyszerű**: Ha hello Kubernetes orchestrator választotta, adjon meg egy Azure Active Directory **szolgáltatás fő ügyfél-azonosító** (más néven hello appId) és **szolgáltatás egyszerű ügyfélkulcs** (jelszó).</span><span class="sxs-lookup"><span data-stu-id="c19ff-140">**Service principal**: If you selected hello Kubernetes orchestrator, enter an Azure Active Directory **Service principal client ID** (also called hello appId) and **Service principal client secret** (password).</span></span> <span data-ttu-id="c19ff-141">További információkért lásd: [kapcsolatos hello szolgáltatás egyszerű Kubernetes fürt](../kubernetes/container-service-kubernetes-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="c19ff-141">For more information, see [About hello service principal for a Kubernetes cluster](../kubernetes/container-service-kubernetes-service-principal.md).</span></span>
    * <span data-ttu-id="c19ff-142">**Fő száma**: hello hello fürt főkiszolgálók száma.</span><span class="sxs-lookup"><span data-stu-id="c19ff-142">**Master count**: hello number of masters in hello cluster.</span></span>
    * <span data-ttu-id="c19ff-143">**Virtuálisgép-diagnosztika**: néhány orchestrators hello főkiszolgálók a Virtuálisgép-diagnosztika engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="c19ff-143">**VM diagnostics**: For some orchestrators, you can enable VM diagnostics on hello masters.</span></span>

    ![Fő konfiguráció](./media/container-service-deployment/acs-portal4.png)  <br />

    <span data-ttu-id="c19ff-145">Kattintson a **OK** készen tooproceed közben.</span><span class="sxs-lookup"><span data-stu-id="c19ff-145">Click **OK** when you're ready tooproceed.</span></span>

5. <span data-ttu-id="c19ff-146">A hello **Gazdagépügynök-konfigurálási** panelen adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="c19ff-146">On hello **Agent configuration** blade, enter hello following information:</span></span>

    * <span data-ttu-id="c19ff-147">**Ügynökök száma**: A Docker Swarm és Kubernetes, az értéke hello hello ügynökskálázási készletben lévő ügynökök kezdeti száma.</span><span class="sxs-lookup"><span data-stu-id="c19ff-147">**Agent count**: For Docker Swarm and Kubernetes, this value is hello initial number of agents in hello agent scale set.</span></span> <span data-ttu-id="c19ff-148">A DC/OS esetén hello a privát skálázási készletekben ügynökök kezdeti száma.</span><span class="sxs-lookup"><span data-stu-id="c19ff-148">For DC/OS, it is hello initial number of agents in a private scale set.</span></span> <span data-ttu-id="c19ff-149">Ezenkívül létrejön egy nyilvános méretezési csoport a DC/OS számára, amely az ügynökök előre meghatározott számát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c19ff-149">Additionally, a public scale set is created for DC/OS, which contains a predetermined number of agents.</span></span> <span data-ttu-id="c19ff-150">hello a nyilvános skálázási készlet ügynökök száma határozza meg hello fürt főkiszolgálók száma hello: egy fő egy nyilvános ügynököt, és három vagy öt főkiszolgálók két nyilvános ügynököt.</span><span class="sxs-lookup"><span data-stu-id="c19ff-150">hello number of agents in this public scale set is determined by hello number of masters in hello cluster: one public agent for one master, and two public agents for three or five masters.</span></span>
    * <span data-ttu-id="c19ff-151">**Ügynök virtuálisgép-méret**: hello hello ügynök-virtuálisgépek mérete.</span><span class="sxs-lookup"><span data-stu-id="c19ff-151">**Agent virtual machine size**: hello size of hello agent virtual machines.</span></span>
    * <span data-ttu-id="c19ff-152">**Operációs rendszer**: Ez a beállítás érhető el jelenleg csak akkor, ha a kiválasztott hello Kubernetes orchestrator.</span><span class="sxs-lookup"><span data-stu-id="c19ff-152">**Operating system**: This setting is currently available only if you selected hello Kubernetes orchestrator.</span></span> <span data-ttu-id="c19ff-153">Válassza a Linux-disztribúció vagy egy Windows Server operációs rendszer toorun hello ügynökökre.</span><span class="sxs-lookup"><span data-stu-id="c19ff-153">Choose either a Linux distribution or a Windows Server operating system toorun on hello agents.</span></span> <span data-ttu-id="c19ff-154">A beállítás meghatározza, hogy a fürt Linux vagy Windows tárolóalkalmazásokat futtathat.</span><span class="sxs-lookup"><span data-stu-id="c19ff-154">This setting determines whether your cluster can run Linux or Windows container apps.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="c19ff-155">A Windows tároló támogatása előzetes verziójú kiadásban érhető el Kubernetes fürtökön.</span><span class="sxs-lookup"><span data-stu-id="c19ff-155">Windows container support is in preview for Kubernetes clusters.</span></span> <span data-ttu-id="c19ff-156">A DC/OS- és Swarm-fürtökön jelenleg csak a Linux-ügynökök támogatottak az Azure Container Service-ben.</span><span class="sxs-lookup"><span data-stu-id="c19ff-156">On DC/OS and Swarm clusters, only Linux agents are currently supported in Azure Container Service.</span></span>

    * <span data-ttu-id="c19ff-157">**Ügynök hitelesítő adatait**: Ha hello Windows operációs rendszert választotta, adjon meg egy rendszergazdai **felhasználónév** és **jelszó** hello ügynök virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="c19ff-157">**Agent credentials**: If you selected hello Windows operating system, enter an administrator **User name** and **Password** for hello agent VMs.</span></span> 

    ![Ügynökkonfiguráció](./media/container-service-deployment/acs-portal5.png)  <br />

    <span data-ttu-id="c19ff-159">Kattintson a **OK** készen tooproceed közben.</span><span class="sxs-lookup"><span data-stu-id="c19ff-159">Click **OK** when you're ready tooproceed.</span></span>

6. <span data-ttu-id="c19ff-160">A szolgáltatás érvényesítésének befejeződése után kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c19ff-160">After service validation finishes, click **OK**.</span></span>

    ![Ellenőrzés](./media/container-service-deployment/acs-portal6.png)  <br />

7. <span data-ttu-id="c19ff-162">Tekintse át a hello feltételeket.</span><span class="sxs-lookup"><span data-stu-id="c19ff-162">Review hello terms.</span></span> <span data-ttu-id="c19ff-163">toostart hello a központi telepítési folyamat kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c19ff-163">toostart hello deployment process, click **Create**.</span></span>

    <span data-ttu-id="c19ff-164">Ha úgy döntött, hogy toopin hello telepítési toohello Azure-portálon, hello telepítés állapota látható.</span><span class="sxs-lookup"><span data-stu-id="c19ff-164">If you've elected toopin hello deployment toohello Azure portal, you can see hello deployment status.</span></span>

    ![Üzembe helyezés állapota](./media/container-service-deployment/acs-portal8.png)  <br />

<span data-ttu-id="c19ff-166">hello telepítés több percet toocomplete vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="c19ff-166">hello deployment takes several minutes toocomplete.</span></span> <span data-ttu-id="c19ff-167">Ezt követően hello Azure Tárolószolgáltatás-fürt használatra készen áll.</span><span class="sxs-lookup"><span data-stu-id="c19ff-167">Then, hello Azure Container Service cluster is ready for use.</span></span>


## <a name="create-a-cluster-by-using-a-quickstart-template"></a><span data-ttu-id="c19ff-168">Fürt létrehozása gyorsindítási sablon használatával</span><span class="sxs-lookup"><span data-stu-id="c19ff-168">Create a cluster by using a quickstart template</span></span>
<span data-ttu-id="c19ff-169">Azure gyors üzembe helyezési sablonokat elérhető toodeploy az Azure Tárolószolgáltatásban fürt.</span><span class="sxs-lookup"><span data-stu-id="c19ff-169">Azure quickstart templates are available toodeploy a cluster in Azure Container Service.</span></span> <span data-ttu-id="c19ff-170">a megadott hello gyorsindítási sablonok lehet módosított tooinclude további vagy speciális Azure konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c19ff-170">hello provided quickstart templates can be modified tooinclude additional or advanced Azure configuration.</span></span> <span data-ttu-id="c19ff-171">toocreate egy Azure Tárolószolgáltatás-fürt egy Azure gyors üzembe helyezés sablon használatával, akkor Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="c19ff-171">toocreate an Azure Container Service cluster by using an Azure quickstart template, you need an Azure subscription.</span></span> <span data-ttu-id="c19ff-172">Ha nem rendelkezik előfizetéssel, regisztrálhat az [ingyenes próbaverzióra](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span><span class="sxs-lookup"><span data-stu-id="c19ff-172">If you don't have one, then sign up for a [free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span></span> 

<span data-ttu-id="c19ff-173">Kövesse ezeket a lépéseket toodeploy sablon használatával fürt és hello Azure CLI 2.0 (lásd: [telepítési és beállítási utasításokat](/cli/azure/install-az-cli2)).</span><span class="sxs-lookup"><span data-stu-id="c19ff-173">Follow these steps toodeploy a cluster using a template and hello Azure CLI 2.0 (see [installation and setup instructions](/cli/azure/install-az-cli2)).</span></span>

> [!NOTE] 
> <span data-ttu-id="c19ff-174">Ha a számítógép Windows rendszert, használhatja a sablon Azure PowerShell használatával hasonló lépéseket toodeploy.</span><span class="sxs-lookup"><span data-stu-id="c19ff-174">If you're on a Windows system, you can use similar steps toodeploy a template using Azure PowerShell.</span></span> <span data-ttu-id="c19ff-175">A lépéseket lásd a szakasz későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="c19ff-175">See steps later in this section.</span></span> <span data-ttu-id="c19ff-176">A sablonok hello keresztül is telepíthet [portal](../../azure-resource-manager/resource-group-template-deploy-portal.md) vagy más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="c19ff-176">You can also deploy a template through hello [portal](../../azure-resource-manager/resource-group-template-deploy-portal.md) or other methods.</span></span>

1. <span data-ttu-id="c19ff-177">a DC/OS, Docker Swarm vagy Kubernetes fürt toodeploy válasszon ki egy hello elérhető gyorsindítási sablonok a Githubról.</span><span class="sxs-lookup"><span data-stu-id="c19ff-177">toodeploy a DC/OS, Docker Swarm, or Kubernetes cluster, select one of hello available quickstart templates from GitHub.</span></span> <span data-ttu-id="c19ff-178">Az alábbiakban egy részleges lista látható.</span><span class="sxs-lookup"><span data-stu-id="c19ff-178">A partial list follows.</span></span> <span data-ttu-id="c19ff-179">hello DC/OS és Swarm sablonok hello hello alapértelmezett vezénylési kivételével azonos történik.</span><span class="sxs-lookup"><span data-stu-id="c19ff-179">hello DC/OS and Swarm templates are hello same, except for hello default orchestrator selection.</span></span>

    * [<span data-ttu-id="c19ff-180">DC/OS-sablon</span><span class="sxs-lookup"><span data-stu-id="c19ff-180">DC/OS template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [<span data-ttu-id="c19ff-181">Swarm-sablon</span><span class="sxs-lookup"><span data-stu-id="c19ff-181">Swarm template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [<span data-ttu-id="c19ff-182">Kubernetes-sablon</span><span class="sxs-lookup"><span data-stu-id="c19ff-182">Kubernetes template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. <span data-ttu-id="c19ff-183">Jelentkezzen be Azure-fiók tooyour (`az login`), és győződjön meg arról, hogy az Azure parancssori felület hello csatlakoztatott tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="c19ff-183">Log in tooyour Azure account (`az login`), and make sure that hello Azure CLI is connected tooyour Azure subscription.</span></span> <span data-ttu-id="c19ff-184">Megtekintheti a hello alapértelmezett előfizetés hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="c19ff-184">You can see hello default subscription by using hello following command:</span></span>

    ```azurecli
    az account show
    ```
    
    <span data-ttu-id="c19ff-185">Ha egynél több előfizetés és a szükséges tooset más alapértelmezett előfizetéssel rendelkezik, futtassa `az account set --subscription` , és adja meg a hello előfizetés-Azonosítóval vagy névvel.</span><span class="sxs-lookup"><span data-stu-id="c19ff-185">If you have more than one subscription and need tooset a different default subscription, run `az account set --subscription` and specify hello subscription ID or name.</span></span>

3. <span data-ttu-id="c19ff-186">Ajánlott eljárásként hello telepítéshez egy új erőforráscsoportot használni.</span><span class="sxs-lookup"><span data-stu-id="c19ff-186">As a best practice, use a new resource group for hello deployment.</span></span> <span data-ttu-id="c19ff-187">egy erőforráscsoport, toocreate hello használata `az group create` parancs egy erőforráscsoport-név és hely megadása:</span><span class="sxs-lookup"><span data-stu-id="c19ff-187">toocreate a resource group, use hello `az group create` command specify a resource group name and location:</span></span> 

    ```azurecli
    az group create --name "RESOURCE_GROUP" --location "LOCATION"
    ```

4. <span data-ttu-id="c19ff-188">Hozzon létre egy JSON fájlt tartalmazó hello szükséges sablont paraméterek.</span><span class="sxs-lookup"><span data-stu-id="c19ff-188">Create a JSON file containing hello required template parameters.</span></span> <span data-ttu-id="c19ff-189">Nevű letöltési hello paraméterfájl `azuredeploy.parameters.json` , amely kísérik hello Azure Tárolószolgáltatás sablon `azuredeploy.json` a Githubon.</span><span class="sxs-lookup"><span data-stu-id="c19ff-189">Download hello parameters file named `azuredeploy.parameters.json` that accompanies hello Azure Container Service template `azuredeploy.json` in GitHub.</span></span> <span data-ttu-id="c19ff-190">Adja meg a fürt szükséges paraméterértékeit.</span><span class="sxs-lookup"><span data-stu-id="c19ff-190">Enter required parameter values for your cluster.</span></span> 

    <span data-ttu-id="c19ff-191">Például toouse hello [DC/OS-sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos), adja meg a paraméterértékek `dnsNamePrefix` és `sshRSAPublicKey`.</span><span class="sxs-lookup"><span data-stu-id="c19ff-191">For example, toouse hello [DC/OS template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos), supply parameter values for `dnsNamePrefix` and `sshRSAPublicKey`.</span></span> <span data-ttu-id="c19ff-192">Lásd: hello leírásokat `azuredeploy.json` és más paramétereket beállításokat.</span><span class="sxs-lookup"><span data-stu-id="c19ff-192">See hello descriptions in `azuredeploy.json` and options for other parameters.</span></span>  
 

5. <span data-ttu-id="c19ff-193">Hozzon létre egy Tárolószolgáltatás-fürt úgy, hogy hello telepítési paraméterek fájlt a hello a következő parancsot, amelyben:</span><span class="sxs-lookup"><span data-stu-id="c19ff-193">Create a Container Service cluster by passing hello deployment parameters file with hello following command, where:</span></span>

    * <span data-ttu-id="c19ff-194">**Erőforráscsoport** hello előző lépésben létrehozott erőforráscsoport hello hello neve.</span><span class="sxs-lookup"><span data-stu-id="c19ff-194">**RESOURCE_GROUP** is hello name of hello resource group that you created in hello previous step.</span></span>
    * <span data-ttu-id="c19ff-195">**DEPLOYMENT_NAME** van egy toohello telepítési neve (nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="c19ff-195">**DEPLOYMENT_NAME** (optional) is a name you give toohello deployment.</span></span>
    * <span data-ttu-id="c19ff-196">**TEMPLATE_URI** hello hely hello telepítési fájl `azuredeploy.json`.</span><span class="sxs-lookup"><span data-stu-id="c19ff-196">**TEMPLATE_URI** is hello location of hello deployment file `azuredeploy.json`.</span></span> <span data-ttu-id="c19ff-197">Ezt az URI hello Raw fájlt, nem egy mutató toohello GitHub felhasználói felületén kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c19ff-197">This URI must be hello Raw file, not a pointer toohello GitHub UI.</span></span> <span data-ttu-id="c19ff-198">toofind ezt az URI, jelölje be hello `azuredeploy.json` fájlt a Githubon, majd kattintson a hello **Raw** gombra.</span><span class="sxs-lookup"><span data-stu-id="c19ff-198">toofind this URI, select hello `azuredeploy.json` file in GitHub, and click hello **Raw** button.</span></span>  

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters @azuredeploy.parameters.json
    ```

    <span data-ttu-id="c19ff-199">Paraméterek egy JSON-formátumú karakterlánc hello parancssorban is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="c19ff-199">You can also provide parameters as a JSON-formatted string on hello command line.</span></span> <span data-ttu-id="c19ff-200">Egy hasonló toohello utáni parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="c19ff-200">Use a command similar toohello following:</span></span>

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters "{ \"param1\": {\"value1\"} … }"
    ```

    > [!NOTE]
    > <span data-ttu-id="c19ff-201">hello telepítés több percet toocomplete vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="c19ff-201">hello deployment takes several minutes toocomplete.</span></span>
    > 

### <a name="equivalent-powershell-commands"></a><span data-ttu-id="c19ff-202">Egyenértékű PowerShell-parancsok</span><span class="sxs-lookup"><span data-stu-id="c19ff-202">Equivalent PowerShell commands</span></span>
<span data-ttu-id="c19ff-203">A PowerShell használatával is üzembe helyezhet Azure Container Service-fürtöket.</span><span class="sxs-lookup"><span data-stu-id="c19ff-203">You can also deploy an Azure Container Service cluster template with PowerShell.</span></span> <span data-ttu-id="c19ff-204">Ez a dokumentum hello 1.0-s verziója alapján [Azure PowerShell modul](https://azure.microsoft.com/blog/azps-1-0/).</span><span class="sxs-lookup"><span data-stu-id="c19ff-204">This document is based on hello version 1.0 [Azure PowerShell module](https://azure.microsoft.com/blog/azps-1-0/).</span></span>

1. <span data-ttu-id="c19ff-205">a DC/OS, Docker Swarm vagy Kubernetes fürt toodeploy válasszon ki egy hello elérhető gyorsindítási sablonok a Githubról.</span><span class="sxs-lookup"><span data-stu-id="c19ff-205">toodeploy a DC/OS, Docker Swarm, or Kubernetes cluster, select one of hello available quickstart templates from GitHub.</span></span> <span data-ttu-id="c19ff-206">Az alábbiakban egy részleges lista látható.</span><span class="sxs-lookup"><span data-stu-id="c19ff-206">A partial list follows.</span></span> <span data-ttu-id="c19ff-207">Vegye figyelembe, hogy hello DC/OS és Swarm sablonok vannak hello azonos, hello alapértelmezett vezénylési hello kivételével.</span><span class="sxs-lookup"><span data-stu-id="c19ff-207">Note that hello DC/OS and Swarm templates are hello same, with hello exception of hello default orchestrator selection.</span></span>

    * [<span data-ttu-id="c19ff-208">DC/OS-sablon</span><span class="sxs-lookup"><span data-stu-id="c19ff-208">DC/OS template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [<span data-ttu-id="c19ff-209">Swarm-sablon</span><span class="sxs-lookup"><span data-stu-id="c19ff-209">Swarm template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [<span data-ttu-id="c19ff-210">Kubernetes-sablon</span><span class="sxs-lookup"><span data-stu-id="c19ff-210">Kubernetes template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. <span data-ttu-id="c19ff-211">Mielőtt az Azure-előfizetésében létrehozna egy fürtöt, győződjön meg arról, hogy a PowerShell-munkamenet bejelentkezett az tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c19ff-211">Before creating a cluster in your Azure subscription, verify that your PowerShell session has been signed in tooAzure.</span></span> <span data-ttu-id="c19ff-212">Ehhez a hello `Get-AzureRMSubscription` parancs:</span><span class="sxs-lookup"><span data-stu-id="c19ff-212">You can do this with hello `Get-AzureRMSubscription` command:</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

3. <span data-ttu-id="c19ff-213">Ha a tooAzure toosign van szüksége, használja a hello `Login-AzureRMAccount` parancs:</span><span class="sxs-lookup"><span data-stu-id="c19ff-213">If you need toosign in tooAzure, use hello `Login-AzureRMAccount` command:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

4. <span data-ttu-id="c19ff-214">Ajánlott eljárásként hello telepítéshez egy új erőforráscsoportot használni.</span><span class="sxs-lookup"><span data-stu-id="c19ff-214">As a best practice, use a new resource group for hello deployment.</span></span> <span data-ttu-id="c19ff-215">egy erőforráscsoport, toocreate hello használata `New-AzureRmResourceGroup` parancsot, és adjon meg egy erőforrás-csoport nevét és helyét régióban:</span><span class="sxs-lookup"><span data-stu-id="c19ff-215">toocreate a resource group, use hello `New-AzureRmResourceGroup` command, and specify a resource group name and destination region:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
    ```

5. <span data-ttu-id="c19ff-216">Miután létrehozott egy erőforráscsoport, a parancs a következő hello hozhat létre a fürt.</span><span class="sxs-lookup"><span data-stu-id="c19ff-216">After you create a resource group, you can create your cluster with hello following command.</span></span> <span data-ttu-id="c19ff-217">hello URI-jának hello kívánt sablon meg van adva hello `-TemplateUri` paraméter.</span><span class="sxs-lookup"><span data-stu-id="c19ff-217">hello URI of hello desired template is specified with hello `-TemplateUri` parameter.</span></span> <span data-ttu-id="c19ff-218">A parancs futtatásakor a rendszerhéj kéri az üzembehelyezési paramétereket.</span><span class="sxs-lookup"><span data-stu-id="c19ff-218">When you run this command, PowerShell prompts you for deployment parameter values.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
    ```

#### <a name="provide-template-parameters"></a><span data-ttu-id="c19ff-219">A sablon paramétereinek megadása</span><span class="sxs-lookup"><span data-stu-id="c19ff-219">Provide template parameters</span></span>
<span data-ttu-id="c19ff-220">Ha ismeri a PowerShell-lel, biztos, hogy válthat hello parancsmag elérhető paraméterei keresztül a mínuszjel (-) beírásával, és nyomja le az hello TAB billentyűt.</span><span class="sxs-lookup"><span data-stu-id="c19ff-220">If you're familiar with PowerShell, you know that you can cycle through hello available parameters for a cmdlet by typing a minus sign (-) and then pressing hello TAB key.</span></span> <span data-ttu-id="c19ff-221">Ez a funkció a sablonban megadott saját paraméterekkel is működik.</span><span class="sxs-lookup"><span data-stu-id="c19ff-221">This same functionality also works with parameters that you define in your template.</span></span> <span data-ttu-id="c19ff-222">Amint beírja a sablonnevet hello, hello parancsmag hello sablon beolvassa hello paraméterek elemzi és hello sablon paraméterek toohello parancs dinamikusan hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="c19ff-222">As soon as you type hello template name, hello cmdlet fetches hello template, parses hello parameters, and adds hello template parameters toohello command dynamically.</span></span> <span data-ttu-id="c19ff-223">Így könnyen toospecify hello sablon-paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="c19ff-223">This makes it easy toospecify hello template parameter values.</span></span> <span data-ttu-id="c19ff-224">És, ha elfelejti a kötelező paraméter értékét, PowerShell hello értékhez.</span><span class="sxs-lookup"><span data-stu-id="c19ff-224">And, if you forget a required parameter value, PowerShell prompts you for hello value.</span></span>

<span data-ttu-id="c19ff-225">Hello teljes parancs paraméterekkel együtt ez.</span><span class="sxs-lookup"><span data-stu-id="c19ff-225">Here is hello full command, with parameters included.</span></span> <span data-ttu-id="c19ff-226">Adja meg a saját hello erőforrások hello nevét.</span><span class="sxs-lookup"><span data-stu-id="c19ff-226">Provide your own values for hello names of hello resources.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a><span data-ttu-id="c19ff-227">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c19ff-227">Next steps</span></span>
<span data-ttu-id="c19ff-228">Most, hogy működő fürtje van, tekintse meg ezeket a dokumentumokat a kapcsolatra és a felügyeletre vonatkozó részletekért:</span><span class="sxs-lookup"><span data-stu-id="c19ff-228">Now that you have a functioning cluster, see these documents for connection and management details:</span></span>

* [<span data-ttu-id="c19ff-229">Csatlakozás Azure Tárolószolgáltatás-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="c19ff-229">Connect tooan Azure Container Service cluster</span></span>](../container-service-connect.md)
* [<span data-ttu-id="c19ff-230">Az Azure Container Service és a DC/OS használata</span><span class="sxs-lookup"><span data-stu-id="c19ff-230">Work with Azure Container Service and DC/OS</span></span>](container-service-mesos-marathon-rest.md)
* [<span data-ttu-id="c19ff-231">Az Azure Container Service és a Docker Swarm használata</span><span class="sxs-lookup"><span data-stu-id="c19ff-231">Work with Azure Container Service and Docker Swarm</span></span>](container-service-docker-swarm.md)
* [<span data-ttu-id="c19ff-232">Az Azure Container Service és a Kubernetes használata</span><span class="sxs-lookup"><span data-stu-id="c19ff-232">Work with Azure Container Service and Kubernetes</span></span>](../kubernetes/container-service-kubernetes-walkthrough.md)
