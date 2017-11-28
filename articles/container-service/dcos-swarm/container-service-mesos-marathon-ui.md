---
title: "A Marathon felhasználói felület Azure DC/OS-fürt kezeléséhez |} Microsoft Docs"
description: "A cikk azt ismerteti, hogyan telepíthetők tárolók egy Azure tárolószolgáltatásba a Marathon webes felhasználói felület segítségével."
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
ms.openlocfilehash: b00088bb005519dc5d533433308c0e3e33c7f433
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a><span data-ttu-id="2cb78-104">Azure Container Service DC/OS-fürt kezelése a Marathon webes felhasználói felületén</span><span class="sxs-lookup"><span data-stu-id="2cb78-104">Manage an Azure Container Service DC/OS cluster through the Marathon web UI</span></span>
<span data-ttu-id="2cb78-105">A DC/OS biztosítja a fürtözött feladatok telepítését és skálázását lehetővé tevő környezetet, ugyanakkor absztrakciós rétegként működik a hardver fölött.</span><span class="sxs-lookup"><span data-stu-id="2cb78-105">DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting the underlying hardware.</span></span> <span data-ttu-id="2cb78-106">A DC/OS fölötti keretrendszer gondoskodik a számítási feladatok ütemezéséről és végrehajtásáról.</span><span class="sxs-lookup"><span data-stu-id="2cb78-106">On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.</span></span>

<span data-ttu-id="2cb78-107">Számos népszerű számítási elérhetők keretrendszerek, ez a dokumentum ismerteti a marathon segítségével tárolók üzembe helyezésének első lépései.</span><span class="sxs-lookup"><span data-stu-id="2cb78-107">While frameworks are available for many popular workloads, this document describes how to get started deploying containers with Marathon.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="2cb78-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2cb78-108">Prerequisites</span></span>
<span data-ttu-id="2cb78-109">A példákban szereplő feladatok elvégzéséhez szüksége lesz egy az Azure tárolószolgáltatásban konfigurált DC/OS-fürtre,</span><span class="sxs-lookup"><span data-stu-id="2cb78-109">Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service.</span></span> <span data-ttu-id="2cb78-110">valamint távoli kapcsolatot kell tudnia létesíteni a fürttel.</span><span class="sxs-lookup"><span data-stu-id="2cb78-110">You also need to have remote connectivity to this cluster.</span></span> <span data-ttu-id="2cb78-111">Ezekkel az elemekkel kapcsolatban a következő cikkekben talál további tájékoztatást:</span><span class="sxs-lookup"><span data-stu-id="2cb78-111">For more information on these items, see the following articles:</span></span>

* [<span data-ttu-id="2cb78-112">Az Azure Container Service-fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="2cb78-112">Deploy an Azure Container Service cluster</span></span>](container-service-deployment.md)
* [<span data-ttu-id="2cb78-113">Csatlakozás Azure Container Service-fürthöz</span><span class="sxs-lookup"><span data-stu-id="2cb78-113">Connect to an Azure Container Service cluster</span></span>](../container-service-connect.md)

> [!NOTE]
> <span data-ttu-id="2cb78-114">Ez a cikk feltételezi, hogy az alagutat a DC/OS-fürtre a helyi 80-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="2cb78-114">This article assumes you are tunneling to the DC/OS cluster through your local port 80.</span></span>
>

## <a name="explore-the-dcos-ui"></a><span data-ttu-id="2cb78-115">A DC/OS felhasználói felületének megnyitása</span><span class="sxs-lookup"><span data-stu-id="2cb78-115">Explore the DC/OS UI</span></span>
<span data-ttu-id="2cb78-116">[Alakítson ki](../container-service-connect.md) egy Secure Shell- (SSH-) alagutat, és nyissa meg a böngészőben a http://localhost/ címet.</span><span class="sxs-lookup"><span data-stu-id="2cb78-116">With a Secure Shell (SSH) tunnel [established](../container-service-connect.md), browse to http://localhost/.</span></span> <span data-ttu-id="2cb78-117">Ez betölti a DC/OS webes felhasználói felületét, és a fürtre vonatkozó információkat jelenít meg, például a felhasznált erőforrásokat, az aktív ügynököket és a futó szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="2cb78-117">This loads the DC/OS web UI and shows information about the cluster, such as used resources, active agents, and running services.</span></span>

![A DC/OS UI felhasználói felülete](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-the-marathon-ui"></a><span data-ttu-id="2cb78-119">A Marathon felhasználói felület megnyitása</span><span class="sxs-lookup"><span data-stu-id="2cb78-119">Explore the Marathon UI</span></span>
<span data-ttu-id="2cb78-120">A Marathon felhasználói felület megtekintéséhez keresse meg azt a http://localhost/marathon.</span><span class="sxs-lookup"><span data-stu-id="2cb78-120">To see the Marathon UI, browse to http://localhost/marathon.</span></span> <span data-ttu-id="2cb78-121">Ezen a képernyőn elindíthat egy új tárolót vagy más szolgáltatást az Azure tárolószolgáltatási DC/OS-fürtön.</span><span class="sxs-lookup"><span data-stu-id="2cb78-121">From this screen, you can start a new container or another application on the Azure Container Service DC/OS cluster.</span></span> <span data-ttu-id="2cb78-122">A futó tárolókkal és alkalmazásokkal kapcsolatos információkat is láthat.</span><span class="sxs-lookup"><span data-stu-id="2cb78-122">You can also see information about running containers and applications.</span></span>  

![Marathon felhasználói felület](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a><span data-ttu-id="2cb78-124">Docker-formázású tároló üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="2cb78-124">Deploy a Docker-formatted container</span></span>
<span data-ttu-id="2cb78-125">Ha új tárolót szeretne üzembe helyezni a Marathon segítségével, kattintson a **Create Application** (Alkalmazás létrehozása) gombra, és adja meg a következő információkat az űrlapon:</span><span class="sxs-lookup"><span data-stu-id="2cb78-125">To deploy a new container by using Marathon, click **Create Application**, and enter the following information into the form tabs:</span></span>

| <span data-ttu-id="2cb78-126">Mező</span><span class="sxs-lookup"><span data-stu-id="2cb78-126">Field</span></span> | <span data-ttu-id="2cb78-127">Érték</span><span class="sxs-lookup"><span data-stu-id="2cb78-127">Value</span></span> |
| --- | --- |
| <span data-ttu-id="2cb78-128">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="2cb78-128">ID</span></span> |<span data-ttu-id="2cb78-129">nginx</span><span class="sxs-lookup"><span data-stu-id="2cb78-129">nginx</span></span> |
| <span data-ttu-id="2cb78-130">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="2cb78-130">Memory</span></span> | <span data-ttu-id="2cb78-131">32</span><span class="sxs-lookup"><span data-stu-id="2cb78-131">32</span></span> |
| <span data-ttu-id="2cb78-132">Image (Kép)</span><span class="sxs-lookup"><span data-stu-id="2cb78-132">Image</span></span> |<span data-ttu-id="2cb78-133">nginx</span><span class="sxs-lookup"><span data-stu-id="2cb78-133">nginx</span></span> |
| <span data-ttu-id="2cb78-134">Network (Hálózat)</span><span class="sxs-lookup"><span data-stu-id="2cb78-134">Network</span></span> |<span data-ttu-id="2cb78-135">Bridged</span><span class="sxs-lookup"><span data-stu-id="2cb78-135">Bridged</span></span> |
| <span data-ttu-id="2cb78-136">Host Port (Gazdaport)</span><span class="sxs-lookup"><span data-stu-id="2cb78-136">Host Port</span></span> |<span data-ttu-id="2cb78-137">80</span><span class="sxs-lookup"><span data-stu-id="2cb78-137">80</span></span> |
| <span data-ttu-id="2cb78-138">Protocol (Protokoll)</span><span class="sxs-lookup"><span data-stu-id="2cb78-138">Protocol</span></span> |<span data-ttu-id="2cb78-139">TCP</span><span class="sxs-lookup"><span data-stu-id="2cb78-139">TCP</span></span> |

![New Application (Új alkalmazás) felhasználói felület – General (Általános)](./media/container-service-mesos-marathon-ui/dcos4.png)

![New Application (Új alkalmazás) felhasználói felület – Docker Container (Docker-tároló)](./media/container-service-mesos-marathon-ui/dcos5.png)

![New Application (Új alkalmazás) felhasználói felület – Ports and Service Discovery (Portok és szolgáltatások felderítése)](./media/container-service-mesos-marathon-ui/dcos6.png)

<span data-ttu-id="2cb78-143">Ha statikusan le kívánja képezni a tárolóportot egy az ügynökön lévő portra, akkor a JSON üzemmódot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="2cb78-143">If you want to statically map the container port to a port on the agent, you need to use JSON Mode.</span></span> <span data-ttu-id="2cb78-144">Ehhez váltsa át a New Application (Új alkalmazás) varázslót erre az üzemmódra a **JSON Mode** (JSON üzemmód) váltógombbal.</span><span class="sxs-lookup"><span data-stu-id="2cb78-144">To do so, switch the New Application wizard to **JSON Mode** by using the toggle.</span></span> <span data-ttu-id="2cb78-145">Ezután adja meg a következő beállítást az alkalmazás definíciójának `portMappings` szakaszában.</span><span class="sxs-lookup"><span data-stu-id="2cb78-145">Then enter the following setting under the `portMappings` section of the application definition.</span></span> <span data-ttu-id="2cb78-146">A példa a tároló 80-as portját a DC/OS-ügynök 80-as portjához köti.</span><span class="sxs-lookup"><span data-stu-id="2cb78-146">This example binds port 80 of the container to port 80 of the DC/OS agent.</span></span> <span data-ttu-id="2cb78-147">Ennek a módosításnak az elvégzése után visszaválthat a varázsló JSON üzemmódjából.</span><span class="sxs-lookup"><span data-stu-id="2cb78-147">You can switch this wizard out of JSON Mode after you make this change.</span></span>

```none
"hostPort": 80,
```

![New Application (Új alkalmazás) felhasználói felület – példa a 80-as portra](./media/container-service-mesos-marathon-ui/dcos13.png)

<span data-ttu-id="2cb78-149">Ha engedélyezni szeretné az állapot-ellenőrzéseket, adjon meg egy útvonalat a **Health Checks** (Állapot-ellenőrzések) lapon.</span><span class="sxs-lookup"><span data-stu-id="2cb78-149">If you want to enable health checks, set a path on the **Health Checks** tab.</span></span>

![New Application (Új alkalmazás) felhasználói felület – állapot-ellenőrzések](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

<span data-ttu-id="2cb78-151">A DC/OS-fürt üzembe helyezésekor különféle privát és nyilvános ügynökök is települnek.</span><span class="sxs-lookup"><span data-stu-id="2cb78-151">The DC/OS cluster is deployed with set of private and public agents.</span></span> <span data-ttu-id="2cb78-152">Ahhoz, hogy a fürt elérhessen alkalmazásokat az internetről, nyilvános ügynökre kell telepíteni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2cb78-152">For the cluster to be able to access applications from the Internet, you need to deploy the applications to a public agent.</span></span> <span data-ttu-id="2cb78-153">Ehhez válassza a New Application (Új alkalmazás) varázsló **Optional** (Választható) lapfülét, és írja be a **slave_public** értéket az **Accepted Resource Roles** (Elfogadott erőforrás-szerepkörök) mezőbe.</span><span class="sxs-lookup"><span data-stu-id="2cb78-153">To do so, select the **Optional** tab of the New Application wizard and enter **slave_public** for the **Accepted Resource Roles**.</span></span>

<span data-ttu-id="2cb78-154">Ezután kattintson a **Create Application** (Alkalmazás létrehozása) elemre.</span><span class="sxs-lookup"><span data-stu-id="2cb78-154">Then click **Create Application**.</span></span>

![New Application (Új alkalmazás) felhasználói felület – nyilvános ügynök beállítása](./media/container-service-mesos-marathon-ui/dcos14.png)

<span data-ttu-id="2cb78-156">A Marathon főoldalára visszatérve láthatja a tároló üzembe helyezési állapotát.</span><span class="sxs-lookup"><span data-stu-id="2cb78-156">Back on the Marathon main page, you can see the deployment status for the container.</span></span> <span data-ttu-id="2cb78-157">Kezdetben a **Deploying** (Üzembe helyezés) állapotot fogja látni.</span><span class="sxs-lookup"><span data-stu-id="2cb78-157">Initially you see a status of **Deploying**.</span></span> <span data-ttu-id="2cb78-158">A sikeres üzembe helyezést követően az állapot **Running** (Fut) állapotra vált.</span><span class="sxs-lookup"><span data-stu-id="2cb78-158">After a successful deployment, the status changes to **Running**.</span></span>

![A Marathon főoldala – a tároló üzembe helyezési állapota](./media/container-service-mesos-marathon-ui/dcos7.png)

<span data-ttu-id="2cb78-160">Amikor visszavált a DC/OS webes felhasználói felületére (http://localhost/), láthatja, hogy a DC/OS-fürtön fut egy feladat (ez esetben egy Docker-formázású tároló).</span><span class="sxs-lookup"><span data-stu-id="2cb78-160">When you switch back to the DC/OS web UI (http://localhost/), you see that a task (in this case, a Docker-formatted container) is running on the DC/OS cluster.</span></span>

![DC/OS webes felhasználói felülete – a fürtön futó feladat](./media/container-service-mesos-marathon-ui/dcos8.png)

<span data-ttu-id="2cb78-162">Ha szeretné megtekinteni azt a fürtcsomópontot, amelyen a feladat fut, kattintson a **Nodes** (Csomópontok) fülre.</span><span class="sxs-lookup"><span data-stu-id="2cb78-162">To see the cluster node that the task is running on, click the **Nodes** tab.</span></span>

![A DC/OS webes felhasználói felülete – a feladat fürtcsomópontja](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-the-container"></a><span data-ttu-id="2cb78-164">A tároló elérése</span><span class="sxs-lookup"><span data-stu-id="2cb78-164">Reach the container</span></span>

<span data-ttu-id="2cb78-165">Ebben a példában az alkalmazás fut egy nyilvános ügynök csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2cb78-165">In this example, the application is running on a public agent node.</span></span> <span data-ttu-id="2cb78-166">Keresse meg azt az ügynöknek a fürt eléri az alkalmazást az internetről: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, ahol:</span><span class="sxs-lookup"><span data-stu-id="2cb78-166">You reach the application from the internet by browsing to the agent FQDN of the cluster: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, where:</span></span>

* <span data-ttu-id="2cb78-167">a **DNSPREFIX** a fürt telepítésekor megadott DNS-előtag.</span><span class="sxs-lookup"><span data-stu-id="2cb78-167">**DNSPREFIX** is the DNS prefix that you provided when you deployed the cluster.</span></span>
* <span data-ttu-id="2cb78-168">a **REGION** az a régió, ahol az erőforráscsoport megtalálható.</span><span class="sxs-lookup"><span data-stu-id="2cb78-168">**REGION** is the region in which your resource group is located.</span></span>

    ![Internetről érkező Nginx](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a><span data-ttu-id="2cb78-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2cb78-170">Next steps</span></span>
* [<span data-ttu-id="2cb78-171">A DC/OS és a Marathon API használata</span><span class="sxs-lookup"><span data-stu-id="2cb78-171">Work with DC/OS and the Marathon API</span></span>](container-service-mesos-marathon-rest.md)

* <span data-ttu-id="2cb78-172">A Mesost használó Azure Container Service részletes bemutatása</span><span class="sxs-lookup"><span data-stu-id="2cb78-172">Deep dive on the Azure Container Service with Mesos</span></span>

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
