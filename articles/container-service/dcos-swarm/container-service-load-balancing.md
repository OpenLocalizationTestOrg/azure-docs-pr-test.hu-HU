---
title: "Betöltése Azure DC/OS-fürtről a tárolók elosztása |} Microsoft Docs"
description: "Egy Azure tároló szolgáltatás DC/OS-fürtben több tároló terheléselosztása."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, mikroszolgáltatások, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 78725c9d23e13d307821a188028ef573d1def038
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="a3dda-104">Betöltési egyenleg tárolók egy Azure tároló szolgáltatás DC/OS-fürtben</span><span class="sxs-lookup"><span data-stu-id="a3dda-104">Load balance containers in an Azure Container Service DC/OS cluster</span></span>
<span data-ttu-id="a3dda-105">Ebben a cikkben megismerkedhet azt egy belső terheléselosztó létrehozása a DC/OS felügyelete az Azure Tárolószolgáltatás Marathon-LB használatával.</span><span class="sxs-lookup"><span data-stu-id="a3dda-105">In this article, we explore how to create an internal load balancer in a DC/OS managed Azure Container Service using Marathon-LB.</span></span> <span data-ttu-id="a3dda-106">Ez a konfiguráció lehetővé teszi az alkalmazások horizontálisan méretezhető.</span><span class="sxs-lookup"><span data-stu-id="a3dda-106">This configuration enables you to scale your applications horizontally.</span></span> <span data-ttu-id="a3dda-107">Azt is lehetővé teszi, hogy kihasználhatja a nyilvános és titkos ügynök fürtök úgy, hogy a terheléselosztó a nyilvános és titkos fürtön alkalmazás tárolók skálázása.</span><span class="sxs-lookup"><span data-stu-id="a3dda-107">It also allows you to take advantage of the public and private agent clusters by placing your load balancers on the public cluster and your application containers on the private cluster.</span></span> <span data-ttu-id="a3dda-108">Ebben az oktatóanyagban az alábbiakat végezte el:</span><span class="sxs-lookup"><span data-stu-id="a3dda-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a3dda-109">A Marathon terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a3dda-109">Configure a Marathon Load Balancer</span></span>
> * <span data-ttu-id="a3dda-110">Olyan központi telepítésű alkalmazást használ a load balancer</span><span class="sxs-lookup"><span data-stu-id="a3dda-110">Deploy an application using the load balancer</span></span>
> * <span data-ttu-id="a3dda-111">Konfigurálja és Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="a3dda-111">Configure and Azure load balancer</span></span>

<span data-ttu-id="a3dda-112">Az ACS DC/OS-fürt az oktatóanyag lépéseinek végrehajtásához van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a3dda-112">You need an ACS DC/OS cluster to complete the steps in this tutorial.</span></span> <span data-ttu-id="a3dda-113">Ha szükséges, [a parancsfájl minta](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="a3dda-113">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="a3dda-114">Az oktatóanyaghoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="a3dda-114">This tutorial requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="a3dda-115">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="a3dda-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="a3dda-116">Ha frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a3dda-116">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a><span data-ttu-id="a3dda-117">Terheléselosztás – áttekintés</span><span class="sxs-lookup"><span data-stu-id="a3dda-117">Load balancing overview</span></span>

<span data-ttu-id="a3dda-118">Az alábbi két terheléselosztási réteg Azure tároló szolgáltatás DC/OS-fürtben lévő:</span><span class="sxs-lookup"><span data-stu-id="a3dda-118">There are two load-balancing layers in an Azure Container Service DC/OS cluster:</span></span> 

<span data-ttu-id="a3dda-119">**Az Azure Load Balancer** biztosít a nyilvános belépési pontok (a végfelhasználók hozzáférhetnének az megfelelően).</span><span class="sxs-lookup"><span data-stu-id="a3dda-119">**Azure Load Balancer** provides public entry points (the ones that end users access).</span></span> <span data-ttu-id="a3dda-120">Az Azure LB Azure Tárolószolgáltatás automatikusan biztosítja, és alapértelmezés szerint a 80-as, a 443-as és a 8080-as portot teszi közzé konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="a3dda-120">An Azure LB is provided automatically by Azure Container Service and is, by default, configured to expose port 80, 443 and 8080.</span></span>

<span data-ttu-id="a3dda-121">**A Marathon terheléselosztó (marathon-lb)** útvonalak bejövő kérelmek ezen érkező kérelmek kiszolgálása során tárolópéldányt.</span><span class="sxs-lookup"><span data-stu-id="a3dda-121">**The Marathon Load Balancer (marathon-lb)** routes inbound requests to container instances that service these requests.</span></span> <span data-ttu-id="a3dda-122">A webszolgáltatás nyújtó tárolók skálázásához, a marathon-lb dinamikusan alkalmazkodik.</span><span class="sxs-lookup"><span data-stu-id="a3dda-122">As we scale the containers providing our web service, the marathon-lb dynamically adapts.</span></span> <span data-ttu-id="a3dda-123">Ez a terheléselosztó nem szerepel a Tárolószolgáltatás alapértelmezés szerint, de egyszerű telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a3dda-123">This load balancer is not provided by default in your Container Service, but it is easy to install.</span></span>

## <a name="configure-marathon-load-balancer"></a><span data-ttu-id="a3dda-124">A Marathon terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a3dda-124">Configure Marathon Load Balancer</span></span>

<span data-ttu-id="a3dda-125">A Marathon Load Balancer dinamikusan újrakonfigurálja magát az üzembe helyezett tárolók alapján.</span><span class="sxs-lookup"><span data-stu-id="a3dda-125">Marathon Load Balancer dynamically reconfigures itself based on the containers that you've deployed.</span></span> <span data-ttu-id="a3dda-126">Akkor is rugalmas elvész egy tároló vagy ügynök - Ha ez történik, Apache Mesos újraindítja a tárolót máshol, és a marathon-lb alkalmazkodik.</span><span class="sxs-lookup"><span data-stu-id="a3dda-126">It's also resilient to the loss of a container or an agent - if this occurs, Apache Mesos restarts the container elsewhere and marathon-lb adapts.</span></span>

<span data-ttu-id="a3dda-127">A következő parancsot a marathon terheléselosztó a nyilvános ügynököt a fürtön telepíteni.</span><span class="sxs-lookup"><span data-stu-id="a3dda-127">Run the following command to install the marathon load balancer on the public agent's cluster.</span></span>

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a><span data-ttu-id="a3dda-128">Elosztott terhelésű alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="a3dda-128">Deploy load balanced application</span></span>

<span data-ttu-id="a3dda-129">Most, hogy már rendelkezésre áll a marathon-lb csomag, üzembe helyezhetünk egy alkalmazástárolót, amelynek el kívánjuk osztani a terhelését.</span><span class="sxs-lookup"><span data-stu-id="a3dda-129">Now that we have the marathon-lb package, we can deploy an application container that we wish to load balance.</span></span> 

<span data-ttu-id="a3dda-130">Első lépésként beolvasása a nyilvánosan elérhetővé ügynökök teljes Tartományneve.</span><span class="sxs-lookup"><span data-stu-id="a3dda-130">First, get the FQDN of the publicly exposed agents.</span></span>

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

<span data-ttu-id="a3dda-131">Következő lépésként hozzon létre egy fájlt *hello-web.json* , és másolja a következő tartalmában.</span><span class="sxs-lookup"><span data-stu-id="a3dda-131">Next, create a file named *hello-web.json* and copy in the following contents.</span></span> <span data-ttu-id="a3dda-132">A `HAPROXY_0_VHOST` címke a DC/OS-ügynökök a teljes tartománynévvel frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="a3dda-132">The `HAPROXY_0_VHOST` label needs to be updated with the FQDN of the DC/OS agents.</span></span> 

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}
```

<span data-ttu-id="a3dda-133">A DC/OS parancssori felület használatával futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a3dda-133">Use the DC/OS CLI to run the application.</span></span> <span data-ttu-id="a3dda-134">Alapértelmezés szerint a Marathon telepíti a titkos fürtre alkalmazástelepítések.</span><span class="sxs-lookup"><span data-stu-id="a3dda-134">By default Marathon deploys the the applicaton to the private cluster.</span></span> <span data-ttu-id="a3dda-135">Ez azt jelenti, hogy a fenti központi telepítés csak a terheléselosztó keresztül érhető el ez általában a kívánt viselkedés.</span><span class="sxs-lookup"><span data-stu-id="a3dda-135">This means that the above deployment is only accessible via your load balancer, which is usually the desired behavior.</span></span>

```azurecli-interactive
dcos marathon app add hello-web.json
```

<span data-ttu-id="a3dda-136">Miután az alkalmazás telepítve van, tallózással keresse meg az ügynök fürt terhelésű alkalmazást teljesen minősített Tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="a3dda-136">Once the application has been deployed, browse to the FQDN of the agent cluster to view load balanced application.</span></span>

![Elosztott terhelésű alkalmazások képe](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a><span data-ttu-id="a3dda-138">Az Azure terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a3dda-138">Configure Azure Load Balancer</span></span>

<span data-ttu-id="a3dda-139">Alapértelmezés szerint az Azure Load Balancer a 80-as, 8080-as és 443-as portokat teszi elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="a3dda-139">By default, Azure Load Balancer exposes ports 80, 8080, and 443.</span></span> <span data-ttu-id="a3dda-140">Ha ezen portok egyikét használja (ahogyan a fenti példában is), akkor semmit nem kell tennie.</span><span class="sxs-lookup"><span data-stu-id="a3dda-140">If you're using one of these three ports (as we do in the above example), then there is nothing you need to do.</span></span> <span data-ttu-id="a3dda-141">Kell tudnia érni az ügynök terheléselosztó FQDN, és minden alkalommal, amikor frissíti, lesz elérte a ciklikus multiplexelés webkiszolgálót egyikét.</span><span class="sxs-lookup"><span data-stu-id="a3dda-141">You should be able to hit your agent load balancer's FQDN, and each time you refresh, you'll hit one of your three web servers in a round-robin fashion.</span></span> 

<span data-ttu-id="a3dda-142">Ha másik portot használ, adja hozzá a port, amelyet akkor használ a load balancer egy ciklikus multiplexelési szabályt és egy mintavételt szeretné.</span><span class="sxs-lookup"><span data-stu-id="a3dda-142">If you use a different port, you need to add a round-robin rule and a probe on the load balancer for the port that you used.</span></span> <span data-ttu-id="a3dda-143">Ezt az [Azure parancssori felületén](../../azure-resource-manager/xplat-cli-azure-resource-manager.md) teheti meg az `azure network lb rule create` és `azure network lb probe create` parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="a3dda-143">You can do this from the [Azure CLI](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), with the commands `azure network lb rule create` and `azure network lb probe create`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3dda-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3dda-144">Next steps</span></span>

<span data-ttu-id="a3dda-145">Ebben az oktatóprogramban megismerte terheléselosztás az ACS és a Marathon és az Azure terheléselosztó többek között a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="a3dda-145">In this tutorial, you learned about load balancing in ACS with both the Marathon and Azure load balancers including the following actions:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a3dda-146">A Marathon terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a3dda-146">Configure a Marathon Load Balancer</span></span>
> * <span data-ttu-id="a3dda-147">Olyan központi telepítésű alkalmazást használ a load balancer</span><span class="sxs-lookup"><span data-stu-id="a3dda-147">Deploy an application using the load balancer</span></span>
> * <span data-ttu-id="a3dda-148">Konfigurálja és Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="a3dda-148">Configure and Azure load balancer</span></span>

<span data-ttu-id="a3dda-149">A következő oktatóanyag további információt az Azure storage integrálása az Azure-ban a DC/OS továbblépés.</span><span class="sxs-lookup"><span data-stu-id="a3dda-149">Advance to the next tutorial to learn about integrating Azure storage with DC/OS in Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a3dda-150">A DC/OS-fürt csatlakoztatási Azure fájlmegosztás</span><span class="sxs-lookup"><span data-stu-id="a3dda-150">Mount Azure File Share in DC/OS cluster</span></span>](container-service-dcos-fileshare.md)