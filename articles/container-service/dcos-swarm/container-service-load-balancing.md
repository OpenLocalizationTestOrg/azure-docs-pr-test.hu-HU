---
title: "Azure DC/OS-fürtről a aaaLoad egyenleg tárolók |} Microsoft Docs"
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
ms.openlocfilehash: 2249cb06880cdb7e9a3aa94c0750c6a27316d349
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="72120-104">Betöltési egyenleg tárolók egy Azure tároló szolgáltatás DC/OS-fürtben</span><span class="sxs-lookup"><span data-stu-id="72120-104">Load balance containers in an Azure Container Service DC/OS cluster</span></span>
<span data-ttu-id="72120-105">Ez a cikk azt megismerkedhet toocreate belső terheléselosztót a DC/OS módjára vonatkozik. az Azure Tárolószolgáltatás Marathon-LB használatával.</span><span class="sxs-lookup"><span data-stu-id="72120-105">In this article, we explore how toocreate an internal load balancer in a DC/OS managed Azure Container Service using Marathon-LB.</span></span> <span data-ttu-id="72120-106">Ez a konfiguráció lehetővé teszi, hogy Ön tooscale az alkalmazások vízszintesen.</span><span class="sxs-lookup"><span data-stu-id="72120-106">This configuration enables you tooscale your applications horizontally.</span></span> <span data-ttu-id="72120-107">Azt is lehetővé teszi hello nyilvános és titkos ügynök fürtök tootake előnyeit úgy, hogy a terheléselosztó hello nyilvános és titkos fürtön hello alkalmazás tárolók skálázása.</span><span class="sxs-lookup"><span data-stu-id="72120-107">It also allows you tootake advantage of hello public and private agent clusters by placing your load balancers on hello public cluster and your application containers on hello private cluster.</span></span> <span data-ttu-id="72120-108">Ebben az oktatóanyagban az alábbiakat végezte el:</span><span class="sxs-lookup"><span data-stu-id="72120-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="72120-109">A Marathon terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="72120-109">Configure a Marathon Load Balancer</span></span>
> * <span data-ttu-id="72120-110">Hello terheléselosztót használó alkalmazások központi telepítése</span><span class="sxs-lookup"><span data-stu-id="72120-110">Deploy an application using hello load balancer</span></span>
> * <span data-ttu-id="72120-111">Konfigurálja és Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="72120-111">Configure and Azure load balancer</span></span>

<span data-ttu-id="72120-112">Az ACS a DC/OS fürtben toocomplete hello lépések ebben az oktatóanyagban van szüksége.</span><span class="sxs-lookup"><span data-stu-id="72120-112">You need an ACS DC/OS cluster toocomplete hello steps in this tutorial.</span></span> <span data-ttu-id="72120-113">Ha szükséges, [a parancsfájl minta](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="72120-113">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="72120-114">Ez az oktatóanyag szükséges hello Azure CLI 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="72120-114">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="72120-115">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="72120-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="72120-116">Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="72120-116">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a><span data-ttu-id="72120-117">Terheléselosztás – áttekintés</span><span class="sxs-lookup"><span data-stu-id="72120-117">Load balancing overview</span></span>

<span data-ttu-id="72120-118">Az alábbi két terheléselosztási réteg Azure tároló szolgáltatás DC/OS-fürtben lévő:</span><span class="sxs-lookup"><span data-stu-id="72120-118">There are two load-balancing layers in an Azure Container Service DC/OS cluster:</span></span> 

<span data-ttu-id="72120-119">**Az Azure Load Balancer** biztosít a nyilvános belépési pontok (hello azokat, a végfelhasználók számára a hozzáférést).</span><span class="sxs-lookup"><span data-stu-id="72120-119">**Azure Load Balancer** provides public entry points (hello ones that end users access).</span></span> <span data-ttu-id="72120-120">Az Azure LB automatikusan Azure tárolószolgáltatáson, és, alapértelmezés szerint beállított tooexpose port a 80-as, a 443-as és a 8080-as.</span><span class="sxs-lookup"><span data-stu-id="72120-120">An Azure LB is provided automatically by Azure Container Service and is, by default, configured tooexpose port 80, 443 and 8080.</span></span>

<span data-ttu-id="72120-121">**hello Marathon terheléselosztó (marathon-lb)** útvonalak bejövő kérelmek toocontainer példányai, amelyeket a szolgáltatás ilyen kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="72120-121">**hello Marathon Load Balancer (marathon-lb)** routes inbound requests toocontainer instances that service these requests.</span></span> <span data-ttu-id="72120-122">A webszolgáltatás nyújtó hello tárolók skálázásához, hello marathon-lb dinamikusan alkalmazkodik.</span><span class="sxs-lookup"><span data-stu-id="72120-122">As we scale hello containers providing our web service, hello marathon-lb dynamically adapts.</span></span> <span data-ttu-id="72120-123">Ez a terheléselosztó nem szerepel a Tárolószolgáltatás alapértelmezés szerint, de egyszerű tooinstall.</span><span class="sxs-lookup"><span data-stu-id="72120-123">This load balancer is not provided by default in your Container Service, but it is easy tooinstall.</span></span>

## <a name="configure-marathon-load-balancer"></a><span data-ttu-id="72120-124">A Marathon terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="72120-124">Configure Marathon Load Balancer</span></span>

<span data-ttu-id="72120-125">A Marathon terheléselosztó dinamikusan újrakonfigurálása telepítése után hello tárolókat alapján.</span><span class="sxs-lookup"><span data-stu-id="72120-125">Marathon Load Balancer dynamically reconfigures itself based on hello containers that you've deployed.</span></span> <span data-ttu-id="72120-126">Is egy tároló vagy ügynök - rugalmas toohello elvesztését, ha ez történik, Apache Mesos újraindul hello tárolót máshol, és a marathon-lb alkalmazkodik.</span><span class="sxs-lookup"><span data-stu-id="72120-126">It's also resilient toohello loss of a container or an agent - if this occurs, Apache Mesos restarts hello container elsewhere and marathon-lb adapts.</span></span>

<span data-ttu-id="72120-127">Futtassa a parancsot tooinstall hello marathon terheléselosztó hello nyilvános ügynök fürtön a következő hello.</span><span class="sxs-lookup"><span data-stu-id="72120-127">Run hello following command tooinstall hello marathon load balancer on hello public agent's cluster.</span></span>

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a><span data-ttu-id="72120-128">Elosztott terhelésű alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="72120-128">Deploy load balanced application</span></span>

<span data-ttu-id="72120-129">Most, hogy hello marathon-lb csomag, telepíthetünk egy alkalmazás-tárolót, hogy jelenítsük tooload egyensúlyára.</span><span class="sxs-lookup"><span data-stu-id="72120-129">Now that we have hello marathon-lb package, we can deploy an application container that we wish tooload balance.</span></span> 

<span data-ttu-id="72120-130">Szereznie hello nyilvánosan elérhetővé tett hello ügynökök teljes Tartományneve.</span><span class="sxs-lookup"><span data-stu-id="72120-130">First, get hello FQDN of hello publicly exposed agents.</span></span>

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

<span data-ttu-id="72120-131">Következő lépésként hozzon létre egy fájlt *hello-web.json* és hello példányának a következő tartalmát.</span><span class="sxs-lookup"><span data-stu-id="72120-131">Next, create a file named *hello-web.json* and copy in hello following contents.</span></span> <span data-ttu-id="72120-132">Hello `HAPROXY_0_VHOST` címke kell toobe hello hello DC/OS-ügynökök teljes Tartományneve frissül.</span><span class="sxs-lookup"><span data-stu-id="72120-132">hello `HAPROXY_0_VHOST` label needs toobe updated with hello FQDN of hello DC/OS agents.</span></span> 

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

<span data-ttu-id="72120-133">Hello DC/OS parancssori felület toorun hello alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="72120-133">Use hello DC/OS CLI toorun hello application.</span></span> <span data-ttu-id="72120-134">Alapértelmezés szerint a Marathon hello hello alkalmazástelepítések toohello titkos fürt központilag telepíti.</span><span class="sxs-lookup"><span data-stu-id="72120-134">By default Marathon deploys hello hello applicaton toohello private cluster.</span></span> <span data-ttu-id="72120-135">Ez azt jelenti, hogy a fenti telepítési hello csak keresztül érhető a terheléselosztó, amely általában a szükséges hello működés.</span><span class="sxs-lookup"><span data-stu-id="72120-135">This means that hello above deployment is only accessible via your load balancer, which is usually hello desired behavior.</span></span>

```azurecli-interactive
dcos marathon app add hello-web.json
```

<span data-ttu-id="72120-136">Miután hello alkalmazás telepítve van, keresse meg a toohello hello ügynök fürt tooview elosztott terhelésű alkalmazások teljes Tartományneve.</span><span class="sxs-lookup"><span data-stu-id="72120-136">Once hello application has been deployed, browse toohello FQDN of hello agent cluster tooview load balanced application.</span></span>

![Elosztott terhelésű alkalmazások képe](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a><span data-ttu-id="72120-138">Az Azure terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="72120-138">Configure Azure Load Balancer</span></span>

<span data-ttu-id="72120-139">Alapértelmezés szerint az Azure Load Balancer a 80-as, 8080-as és 443-as portokat teszi elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="72120-139">By default, Azure Load Balancer exposes ports 80, 8080, and 443.</span></span> <span data-ttu-id="72120-140">Használata esetén egy három portok (mint mi a fenti példa hello), majd semmilyen toodo van szüksége.</span><span class="sxs-lookup"><span data-stu-id="72120-140">If you're using one of these three ports (as we do in hello above example), then there is nothing you need toodo.</span></span> <span data-ttu-id="72120-141">Meg kell tudni toohit az ügynök betölti a terheléselosztó FQDN, és minden alkalommal, amikor frissíti, lesz találati egyet a három webkiszolgálók ciklikus multiplexelés.</span><span class="sxs-lookup"><span data-stu-id="72120-141">You should be able toohit your agent load balancer's FQDN, and each time you refresh, you'll hit one of your three web servers in a round-robin fashion.</span></span> 

<span data-ttu-id="72120-142">Ha egy másik portot használ, szükség van tooadd egy ciklikus multiplexelési szabályt és egy mintavételt hello terheléselosztón hello használt portot.</span><span class="sxs-lookup"><span data-stu-id="72120-142">If you use a different port, you need tooadd a round-robin rule and a probe on hello load balancer for hello port that you used.</span></span> <span data-ttu-id="72120-143">Ehhez a hello [Azure CLI](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), hello parancsokkal `azure network lb rule create` és `azure network lb probe create`.</span><span class="sxs-lookup"><span data-stu-id="72120-143">You can do this from hello [Azure CLI](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), with hello commands `azure network lb rule create` and `azure network lb probe create`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72120-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="72120-144">Next steps</span></span>

<span data-ttu-id="72120-145">Ebben az oktatóprogramban megismerte megtudni a terheléselosztásról az ACS hello Marathon és az Azure load beleértve terheléselosztók hello a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="72120-145">In this tutorial, you learned about load balancing in ACS with both hello Marathon and Azure load balancers including hello following actions:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="72120-146">A Marathon terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="72120-146">Configure a Marathon Load Balancer</span></span>
> * <span data-ttu-id="72120-147">Hello terheléselosztót használó alkalmazások központi telepítése</span><span class="sxs-lookup"><span data-stu-id="72120-147">Deploy an application using hello load balancer</span></span>
> * <span data-ttu-id="72120-148">Konfigurálja és Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="72120-148">Configure and Azure load balancer</span></span>

<span data-ttu-id="72120-149">Előzetes toohello oktatóanyag következő toolearn az Azure storage együttes használata a DC/OS az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="72120-149">Advance toohello next tutorial toolearn about integrating Azure storage with DC/OS in Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="72120-150">A DC/OS-fürt csatlakoztatási Azure fájlmegosztás</span><span class="sxs-lookup"><span data-stu-id="72120-150">Mount Azure File Share in DC/OS cluster</span></span>](container-service-dcos-fileshare.md)