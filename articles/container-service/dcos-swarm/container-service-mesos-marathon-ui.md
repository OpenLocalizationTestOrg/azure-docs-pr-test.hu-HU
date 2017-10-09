---
title: "Azure DC/OS-fürtöt aaaManage Marathon felhasználói felület |} Microsoft Docs"
description: "Tárolók tooan Azure tárolószolgáltatásba hello Marathon webes felhasználói felület segítségével telepítheti."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: a90180e1b4763e6d2ddfa699ed4b7269f209f728
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-container-service-dcos-cluster-through-hello-marathon-web-ui"></a><span data-ttu-id="e9da8-104">Egy Azure tároló szolgáltatás DC/OS-fürt hello Marathon webes felhasználói felületen keresztül kezeléséhez</span><span class="sxs-lookup"><span data-stu-id="e9da8-104">Manage an Azure Container Service DC/OS cluster through hello Marathon web UI</span></span>
<span data-ttu-id="e9da8-105">A DC/OS telepítését és skálázását lehetővé fürtözött munkaterhelések, absztrakt módon megjelenítve a mögöttes hardver hello közben környezetet biztosít.</span><span class="sxs-lookup"><span data-stu-id="e9da8-105">DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting hello underlying hardware.</span></span> <span data-ttu-id="e9da8-106">A DC/OS fölötti keretrendszer gondoskodik a számítási feladatok ütemezéséről és végrehajtásáról.</span><span class="sxs-lookup"><span data-stu-id="e9da8-106">On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.</span></span>

<span data-ttu-id="e9da8-107">Számos népszerű számítási elérhetők keretrendszerek, ez a dokumentum ismerteti, hogyan tooget el őket a marathon segítségével tárolók telepítése.</span><span class="sxs-lookup"><span data-stu-id="e9da8-107">While frameworks are available for many popular workloads, this document describes how tooget started deploying containers with Marathon.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="e9da8-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e9da8-108">Prerequisites</span></span>
<span data-ttu-id="e9da8-109">A példákban szereplő feladatok elvégzéséhez szüksége lesz egy az Azure tárolószolgáltatásban konfigurált DC/OS-fürtre,</span><span class="sxs-lookup"><span data-stu-id="e9da8-109">Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service.</span></span> <span data-ttu-id="e9da8-110">Meg kell toohave távoli kapcsolatot toothis fürtöt is.</span><span class="sxs-lookup"><span data-stu-id="e9da8-110">You also need toohave remote connectivity toothis cluster.</span></span> <span data-ttu-id="e9da8-111">További információ ezekről az elemekről tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="e9da8-111">For more information on these items, see hello following articles:</span></span>

* [<span data-ttu-id="e9da8-112">Az Azure Container Service-fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="e9da8-112">Deploy an Azure Container Service cluster</span></span>](container-service-deployment.md)
* [<span data-ttu-id="e9da8-113">Csatlakozás Azure Tárolószolgáltatás-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="e9da8-113">Connect tooan Azure Container Service cluster</span></span>](../container-service-connect.md)

> [!NOTE]
> <span data-ttu-id="e9da8-114">Ez a cikk feltételezi, hogy az alagutat toohello DC/OS-fürtről a helyi 80-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="e9da8-114">This article assumes you are tunneling toohello DC/OS cluster through your local port 80.</span></span>
>

## <a name="explore-hello-dcos-ui"></a><span data-ttu-id="e9da8-115">DC/OS felhasználói felületének hello felfedezés</span><span class="sxs-lookup"><span data-stu-id="e9da8-115">Explore hello DC/OS UI</span></span>
<span data-ttu-id="e9da8-116">Az egy Secure Shell (SSH) alagút [létrehozott](../container-service-connect.md), keresse meg a toohttp://localhost/.</span><span class="sxs-lookup"><span data-stu-id="e9da8-116">With a Secure Shell (SSH) tunnel [established](../container-service-connect.md), browse toohttp://localhost/.</span></span> <span data-ttu-id="e9da8-117">Ez betölti a hello DC/OS webes felhasználói felület, és hello fürt, például a felhasznált erőforrásokat, aktív ügynököket és futó szolgáltatások információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e9da8-117">This loads hello DC/OS web UI and shows information about hello cluster, such as used resources, active agents, and running services.</span></span>

![A DC/OS UI felhasználói felülete](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-hello-marathon-ui"></a><span data-ttu-id="e9da8-119">Marathon felhasználói felület hello felfedezés</span><span class="sxs-lookup"><span data-stu-id="e9da8-119">Explore hello Marathon UI</span></span>
<span data-ttu-id="e9da8-120">Marathon felhasználói felület toosee hello toohttp://localhost/marathon Tallózás.</span><span class="sxs-lookup"><span data-stu-id="e9da8-120">toosee hello Marathon UI, browse toohttp://localhost/marathon.</span></span> <span data-ttu-id="e9da8-121">Ezen a képernyőn elindíthatja egy új tároló vagy egy másik alkalmazás hello Azure tároló szolgáltatás DC/OS-fürtön.</span><span class="sxs-lookup"><span data-stu-id="e9da8-121">From this screen, you can start a new container or another application on hello Azure Container Service DC/OS cluster.</span></span> <span data-ttu-id="e9da8-122">A futó tárolókkal és alkalmazásokkal kapcsolatos információkat is láthat.</span><span class="sxs-lookup"><span data-stu-id="e9da8-122">You can also see information about running containers and applications.</span></span>  

![Marathon felhasználói felület](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a><span data-ttu-id="e9da8-124">Docker-formázású tároló üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="e9da8-124">Deploy a Docker-formatted container</span></span>
<span data-ttu-id="e9da8-125">a Marathon, egy új tároló toodeploy kattintson **alkalmazás létrehozása**, és írja be a következő információ be hello űrlap lapok hello:</span><span class="sxs-lookup"><span data-stu-id="e9da8-125">toodeploy a new container by using Marathon, click **Create Application**, and enter hello following information into hello form tabs:</span></span>

| <span data-ttu-id="e9da8-126">Mező</span><span class="sxs-lookup"><span data-stu-id="e9da8-126">Field</span></span> | <span data-ttu-id="e9da8-127">Érték</span><span class="sxs-lookup"><span data-stu-id="e9da8-127">Value</span></span> |
| --- | --- |
| <span data-ttu-id="e9da8-128">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="e9da8-128">ID</span></span> |<span data-ttu-id="e9da8-129">nginx</span><span class="sxs-lookup"><span data-stu-id="e9da8-129">nginx</span></span> |
| <span data-ttu-id="e9da8-130">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="e9da8-130">Memory</span></span> | <span data-ttu-id="e9da8-131">32</span><span class="sxs-lookup"><span data-stu-id="e9da8-131">32</span></span> |
| <span data-ttu-id="e9da8-132">Image (Kép)</span><span class="sxs-lookup"><span data-stu-id="e9da8-132">Image</span></span> |<span data-ttu-id="e9da8-133">nginx</span><span class="sxs-lookup"><span data-stu-id="e9da8-133">nginx</span></span> |
| <span data-ttu-id="e9da8-134">Network (Hálózat)</span><span class="sxs-lookup"><span data-stu-id="e9da8-134">Network</span></span> |<span data-ttu-id="e9da8-135">Bridged</span><span class="sxs-lookup"><span data-stu-id="e9da8-135">Bridged</span></span> |
| <span data-ttu-id="e9da8-136">Host Port (Gazdaport)</span><span class="sxs-lookup"><span data-stu-id="e9da8-136">Host Port</span></span> |<span data-ttu-id="e9da8-137">80</span><span class="sxs-lookup"><span data-stu-id="e9da8-137">80</span></span> |
| <span data-ttu-id="e9da8-138">Protocol (Protokoll)</span><span class="sxs-lookup"><span data-stu-id="e9da8-138">Protocol</span></span> |<span data-ttu-id="e9da8-139">TCP</span><span class="sxs-lookup"><span data-stu-id="e9da8-139">TCP</span></span> |

![New Application (Új alkalmazás) felhasználói felület – General (Általános)](./media/container-service-mesos-marathon-ui/dcos4.png)

![New Application (Új alkalmazás) felhasználói felület – Docker Container (Docker-tároló)](./media/container-service-mesos-marathon-ui/dcos5.png)

![New Application (Új alkalmazás) felhasználói felület – Ports and Service Discovery (Portok és szolgáltatások felderítése)](./media/container-service-mesos-marathon-ui/dcos6.png)

<span data-ttu-id="e9da8-143">Ha azt szeretné, hogy toostatically térkép hello tárolóportot tooa port hello ügynökön, toouse JSON üzemmódot kell.</span><span class="sxs-lookup"><span data-stu-id="e9da8-143">If you want toostatically map hello container port tooa port on hello agent, you need toouse JSON Mode.</span></span> <span data-ttu-id="e9da8-144">toodo tehát az túl hello új alkalmazás varázsló kapcsoló**JSON üzemmódot** hello váltása használatával.</span><span class="sxs-lookup"><span data-stu-id="e9da8-144">toodo so, switch hello New Application wizard too**JSON Mode** by using hello toggle.</span></span> <span data-ttu-id="e9da8-145">Írja be a következő hello beállítás hello `portMappings` hello definíciót szakasza.</span><span class="sxs-lookup"><span data-stu-id="e9da8-145">Then enter hello following setting under hello `portMappings` section of hello application definition.</span></span> <span data-ttu-id="e9da8-146">Ebben a példában a hello tároló tooport 80 hello DC/OS-ügynök 80-as porton van kötve.</span><span class="sxs-lookup"><span data-stu-id="e9da8-146">This example binds port 80 of hello container tooport 80 of hello DC/OS agent.</span></span> <span data-ttu-id="e9da8-147">Ennek a módosításnak az elvégzése után visszaválthat a varázsló JSON üzemmódjából.</span><span class="sxs-lookup"><span data-stu-id="e9da8-147">You can switch this wizard out of JSON Mode after you make this change.</span></span>

```none
"hostPort": 80,
```

![New Application (Új alkalmazás) felhasználói felület – példa a 80-as portra](./media/container-service-mesos-marathon-ui/dcos13.png)

<span data-ttu-id="e9da8-149">Ha azt szeretné, hogy tooenable állapotellenőrzést, egy elérési utat meg hello **ellenőrzést** lapon.</span><span class="sxs-lookup"><span data-stu-id="e9da8-149">If you want tooenable health checks, set a path on hello **Health Checks** tab.</span></span>

![New Application (Új alkalmazás) felhasználói felület – állapot-ellenőrzések](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

<span data-ttu-id="e9da8-151">hello DC/OS-fürtről van telepítve, privát és nyilvános ügynökök vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="e9da8-151">hello DC/OS cluster is deployed with set of private and public agents.</span></span> <span data-ttu-id="e9da8-152">A hello fürt toobe képes tooaccess alkalmazások hello Internet toodeploy hello alkalmazások tooa nyilvános ügynökre kell.</span><span class="sxs-lookup"><span data-stu-id="e9da8-152">For hello cluster toobe able tooaccess applications from hello Internet, you need toodeploy hello applications tooa public agent.</span></span> <span data-ttu-id="e9da8-153">toodo Igen, válassza ki a hello **nem kötelező** hello új alkalmazás varázsló lapja, és írja be **slave_public** a hello **elfogadott erőforrás szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="e9da8-153">toodo so, select hello **Optional** tab of hello New Application wizard and enter **slave_public** for hello **Accepted Resource Roles**.</span></span>

<span data-ttu-id="e9da8-154">Ezután kattintson a **Create Application** (Alkalmazás létrehozása) elemre.</span><span class="sxs-lookup"><span data-stu-id="e9da8-154">Then click **Create Application**.</span></span>

![New Application (Új alkalmazás) felhasználói felület – nyilvános ügynök beállítása](./media/container-service-mesos-marathon-ui/dcos14.png)

<span data-ttu-id="e9da8-156">Vissza a hello Marathon főoldalára visszatérve hello tároló hello telepítési állapota látható.</span><span class="sxs-lookup"><span data-stu-id="e9da8-156">Back on hello Marathon main page, you can see hello deployment status for hello container.</span></span> <span data-ttu-id="e9da8-157">Kezdetben a **Deploying** (Üzembe helyezés) állapotot fogja látni.</span><span class="sxs-lookup"><span data-stu-id="e9da8-157">Initially you see a status of **Deploying**.</span></span> <span data-ttu-id="e9da8-158">A sikeres telepítést követően hello állapotmódosítások túl**futtató**.</span><span class="sxs-lookup"><span data-stu-id="e9da8-158">After a successful deployment, hello status changes too**Running**.</span></span>

![A Marathon főoldala – a tároló üzembe helyezési állapota](./media/container-service-mesos-marathon-ui/dcos7.png)

<span data-ttu-id="e9da8-160">Amikor hátsó toohello DC/OS webes felhasználói Felületére (http://localhost/), láthatja, hogy hello DC/OS fürtben fut egy feladat (ebben az esetben egy Docker-formázású tároló).</span><span class="sxs-lookup"><span data-stu-id="e9da8-160">When you switch back toohello DC/OS web UI (http://localhost/), you see that a task (in this case, a Docker-formatted container) is running on hello DC/OS cluster.</span></span>

![A DC/OS webes felhasználói felülete – hello fürtben futó feladat](./media/container-service-mesos-marathon-ui/dcos8.png)

<span data-ttu-id="e9da8-162">toosee hello fürtcsomóponton, amely hello feladat fut, kattintson a hello **csomópontok** fülre.</span><span class="sxs-lookup"><span data-stu-id="e9da8-162">toosee hello cluster node that hello task is running on, click hello **Nodes** tab.</span></span>

![A DC/OS webes felhasználói felülete – a feladat fürtcsomópontja](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-hello-container"></a><span data-ttu-id="e9da8-164">Hello tároló elérése</span><span class="sxs-lookup"><span data-stu-id="e9da8-164">Reach hello container</span></span>

<span data-ttu-id="e9da8-165">Ebben a példában hello alkalmazás fut egy nyilvános ügynök csomóponton.</span><span class="sxs-lookup"><span data-stu-id="e9da8-165">In this example, hello application is running on a public agent node.</span></span> <span data-ttu-id="e9da8-166">Hello hello alkalmazás eléri toohello ügynök hello fürt tallózással internet: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, ahol:</span><span class="sxs-lookup"><span data-stu-id="e9da8-166">You reach hello application from hello internet by browsing toohello agent FQDN of hello cluster: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, where:</span></span>

* <span data-ttu-id="e9da8-167">**DNSPREFIX** hello hello fürt telepítésekor megadott DNS-előtag van.</span><span class="sxs-lookup"><span data-stu-id="e9da8-167">**DNSPREFIX** is hello DNS prefix that you provided when you deployed hello cluster.</span></span>
* <span data-ttu-id="e9da8-168">**A régióban** hello régió, ahol az erőforráscsoport megtalálható.</span><span class="sxs-lookup"><span data-stu-id="e9da8-168">**REGION** is hello region in which your resource group is located.</span></span>

    ![Internetről érkező Nginx](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a><span data-ttu-id="e9da8-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e9da8-170">Next steps</span></span>
* [<span data-ttu-id="e9da8-171">A DC/OS használata és hello Marathon API</span><span class="sxs-lookup"><span data-stu-id="e9da8-171">Work with DC/OS and hello Marathon API</span></span>](container-service-mesos-marathon-rest.md)

* <span data-ttu-id="e9da8-172">Részletes bemutatója a hello Azure Tárolószolgáltatás a Mesos</span><span class="sxs-lookup"><span data-stu-id="e9da8-172">Deep dive on hello Azure Container Service with Mesos</span></span>

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
