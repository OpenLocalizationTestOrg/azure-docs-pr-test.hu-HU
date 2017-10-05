---
title: "Azure DC/OS-fürtön a Vamp Kanári kiadás |} Microsoft Docs"
description: "Vamp segítségével Kanári kiadás szolgáltatások, és egy Azure tároló szolgáltatás DC/OS-fürtről végez a intelligens forgalom alkalmazása"
services: container-service
author: gggina
manager: rasquill
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/17/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: 4a20091b59f2643ea71cce99c159a5075706e35d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="69fdb-103">Egy Azure tároló szolgáltatás DC/OS-fürtön a Vamp Kanári kiadás mikroszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="69fdb-103">Canary release microservices with Vamp on an Azure Container Service DC/OS cluster</span></span>

<span data-ttu-id="69fdb-104">Ebben a bemutatóban beállítjuk Vamp Azure tárolószolgáltatás és a DC/OS-fürtről.</span><span class="sxs-lookup"><span data-stu-id="69fdb-104">In this walkthrough, we set up Vamp on Azure Container Service with a DC/OS cluster.</span></span> <span data-ttu-id="69fdb-105">Azt Kanári a Vamp bemutató szolgáltatás "Száva" kiadású, és oldja meg a Firefox szolgáltatást inkompatibilitás intelligens forgalomszűrést végez alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="69fdb-105">We canary release the Vamp demo service "sava", and then resolve an incompatibility of the service with Firefox by applying smart traffic filtering.</span></span> 

> [!TIP] 
> <span data-ttu-id="69fdb-106">A forgatókönyv Vamp a DC/OS-fürtön fut, de akkor is használható Vamp rendelkező Kubernetes az orchestrator.</span><span class="sxs-lookup"><span data-stu-id="69fdb-106">In this walkthrough Vamp runs on a DC/OS cluster, but you can also use Vamp with Kubernetes as the orchestrator.</span></span>
>

## <a name="about-canary-releases-and-vamp"></a><span data-ttu-id="69fdb-107">Feloldja a Kanári kapcsolatos és Vamp</span><span class="sxs-lookup"><span data-stu-id="69fdb-107">About canary releases and Vamp</span></span>


<span data-ttu-id="69fdb-108">[Kanári felszabadítása](https://martinfowler.com/bliki/CanaryRelease.html) például Netflix, a Facebook-on és a Spotify új szervezetek által elfogadott intelligens központi telepítési stratégiát is.</span><span class="sxs-lookup"><span data-stu-id="69fdb-108">[Canary releasing](https://martinfowler.com/bliki/CanaryRelease.html) is a smart deployment strategy adopted by innovative organizations like Netflix, Facebook, and Spotify.</span></span> <span data-ttu-id="69fdb-109">Értelme, mivel csökkenti a problémákat, biztonsági-háló vezet be, és növeli az innováció egy megközelítést.</span><span class="sxs-lookup"><span data-stu-id="69fdb-109">It’s an approach that makes sense, because it reduces issues, introduces safety-nets, and increases innovation.</span></span> <span data-ttu-id="69fdb-110">Ezért miért nem minden vállalat használja azt?</span><span class="sxs-lookup"><span data-stu-id="69fdb-110">So why aren’t all companies using it?</span></span> <span data-ttu-id="69fdb-111">CI/CD csővezeték Kanári stratégiákat kiterjesztése hozzáadása összetettségét, valamint kiterjedt devops tudásuk és tapasztalataik igényel.</span><span class="sxs-lookup"><span data-stu-id="69fdb-111">Extending a CI/CD pipeline to include canary strategies adds complexity, and requires extensive devops knowledge and experience.</span></span> <span data-ttu-id="69fdb-112">Ez elég blokkolására kisebb cégek és vállalatok hasonló ahhoz, azok még első lépései.</span><span class="sxs-lookup"><span data-stu-id="69fdb-112">That’s enough to block smaller companies and enterprises alike before they even get started.</span></span> 

<span data-ttu-id="69fdb-113">[Vamp](http://vamp.io/) ad egy nyílt forráskódú rendszer célja, hogy ez a változás megkönnyítik és Kanári kapcsolja ki az előnyben részesített tároló Feladatütemező szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="69fdb-113">[Vamp](http://vamp.io/) is an open-source system designed to ease this transition and bring canary releasing features to your preferred container scheduler.</span></span> <span data-ttu-id="69fdb-114">Vamp tartozó Kanári funkció túllép százalék alapú exportálása.</span><span class="sxs-lookup"><span data-stu-id="69fdb-114">Vamp’s canary functionality goes beyond percentage-based rollouts.</span></span> <span data-ttu-id="69fdb-115">Forgalom szűrve, és azokat a feltételeket, például adott felhasználók megcélzása, IP-címtartományok vagy eszközök felosztással állít elő.</span><span class="sxs-lookup"><span data-stu-id="69fdb-115">Traffic can be filtered and split on a wide range of conditions, for example to target specific users, IP-ranges, or devices.</span></span> <span data-ttu-id="69fdb-116">Vamp nyomon követi, és elemzi a teljesítménymutatók, lehetővé teszi a automation valós adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="69fdb-116">Vamp tracks and analyzes performance metrics, allowing for automation based on real-world data.</span></span> <span data-ttu-id="69fdb-117">A hibák automatikus visszaállítási beállítása, vagy méretezni terhelés alapján vagy késés szolgáltatás Variant adattípusban.</span><span class="sxs-lookup"><span data-stu-id="69fdb-117">You can set up automatic rollback on errors, or scale individual service variants based on load or latency.</span></span>

## <a name="set-up-azure-container-service-with-dcos"></a><span data-ttu-id="69fdb-118">Azure Tárolószolgáltatási DC/OS rendelkező beállítása</span><span class="sxs-lookup"><span data-stu-id="69fdb-118">Set up Azure Container Service with DC/OS</span></span>



1. <span data-ttu-id="69fdb-119">[A DC/OS-fürt üzembe](container-service-deployment.md) egy fő és a két ügynök alapértelmezett mérete.</span><span class="sxs-lookup"><span data-stu-id="69fdb-119">[Deploy a DC/OS cluster](container-service-deployment.md) with one master and two agents of default size.</span></span> 

2. <span data-ttu-id="69fdb-120">[SSH-alagút létrehozása](../container-service-connect.md) a DC/OS-fürtön való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="69fdb-120">[Create an SSH tunnel](../container-service-connect.md) to connect to the DC/OS cluster.</span></span> <span data-ttu-id="69fdb-121">Ez a cikk azt feltételezi, hogy a 80-as portjához helyi fürthöz alagutat.</span><span class="sxs-lookup"><span data-stu-id="69fdb-121">This article assumes that you tunnel to the cluster on local port 80.</span></span>


## <a name="set-up-vamp"></a><span data-ttu-id="69fdb-122">Vamp beállítása</span><span class="sxs-lookup"><span data-stu-id="69fdb-122">Set up Vamp</span></span>

<span data-ttu-id="69fdb-123">Most, hogy a futó DC/OS-fürtről, a DC/OS felhasználói felületének (http://localhost:80) az telepíthető Vamp.</span><span class="sxs-lookup"><span data-stu-id="69fdb-123">Now that you have a running DC/OS cluster, you can install Vamp from the DC/OS UI (http://localhost:80).</span></span> 

![A DC/OS UI felhasználói felülete](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

<span data-ttu-id="69fdb-125">Telepítés két lépésben történik:</span><span class="sxs-lookup"><span data-stu-id="69fdb-125">Installation is done in two stages:</span></span>

1. <span data-ttu-id="69fdb-126">**Elasticsearch telepítése**.</span><span class="sxs-lookup"><span data-stu-id="69fdb-126">**Deploy Elasticsearch**.</span></span>

2. <span data-ttu-id="69fdb-127">Majd **Vamp telepítése** a Vamp DC/OS universe csomag telepítésével.</span><span class="sxs-lookup"><span data-stu-id="69fdb-127">Then **deploy Vamp** by installing the Vamp DC/OS universe package.</span></span>

### <a name="deploy-elasticsearch"></a><span data-ttu-id="69fdb-128">Elasticsearch telepítése</span><span class="sxs-lookup"><span data-stu-id="69fdb-128">Deploy Elasticsearch</span></span>

<span data-ttu-id="69fdb-129">Vamp Elasticsearch metrikák adatgyűjtési és -összesítési igényel.</span><span class="sxs-lookup"><span data-stu-id="69fdb-129">Vamp requires Elasticsearch for metrics collection and aggregation.</span></span> <span data-ttu-id="69fdb-130">Használhatja a [magneticio Docker képek](https://hub.docker.com/r/magneticio/elastic/) egy kompatibilis Vamp Elasticsearch verem telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="69fdb-130">You can use the [magneticio Docker images](https://hub.docker.com/r/magneticio/elastic/) to deploy a compatible Vamp Elasticsearch stack.</span></span>

1. <span data-ttu-id="69fdb-131">A DC/OS felhasználói felületének Ugrás **szolgáltatások** kattintson **szolgáltatás telepítése**.</span><span class="sxs-lookup"><span data-stu-id="69fdb-131">In the DC/OS UI, go to **Services** and click **Deploy Service**.</span></span>

2. <span data-ttu-id="69fdb-132">Válassza ki **JSON üzemmódot** a a **új szolgáltatás telepítése** előugró.</span><span class="sxs-lookup"><span data-stu-id="69fdb-132">Select **JSON mode** from the **Deploy New Service** pop-up.</span></span>

  ![Válassza ki a JSON üzemmód](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. <span data-ttu-id="69fdb-134">Illessze be a következő JSON-ban.</span><span class="sxs-lookup"><span data-stu-id="69fdb-134">Paste in the following JSON.</span></span> <span data-ttu-id="69fdb-135">Ez a konfiguráció a tárolóban fut, 1 GB RAM és a Elasticsearch port alapszintű állapotának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="69fdb-135">This configuration runs the container with 1 GB of RAM and a basic health check on the Elasticsearch port.</span></span>
  
  ```JSON
  {
    "id": "elasticsearch",
    "instances": 1,
    "cpus": 0.2,
    "mem": 1024.0,
    "container": {
      "docker": {
        "image": "magneticio/elastic:2.2",
        "network": "HOST",
        "forcePullImage": true
      }
    },
    "healthChecks": [
      {
        "protocol": "TCP",
        "gracePeriodSeconds": 30,
        "intervalSeconds": 10,
        "timeoutSeconds": 5,
        "port": 9200,
        "maxConsecutiveFailures": 0
      }
    ]
  }
  ```
  

3. <span data-ttu-id="69fdb-136">Kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="69fdb-136">Click **Deploy**.</span></span>

  <span data-ttu-id="69fdb-137">A DC/OS telepíti a Elasticsearch tároló.</span><span class="sxs-lookup"><span data-stu-id="69fdb-137">DC/OS deploys the Elasticsearch container.</span></span> <span data-ttu-id="69fdb-138">A végrehajtás nyomon a **szolgáltatások** lap.</span><span class="sxs-lookup"><span data-stu-id="69fdb-138">You can track progress on the **Services** page.</span></span>  

  ![e telepítéséhez? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a><span data-ttu-id="69fdb-140">Vamp telepítése</span><span class="sxs-lookup"><span data-stu-id="69fdb-140">Deploy Vamp</span></span>

<span data-ttu-id="69fdb-141">Miután Elasticsearch jelzi **futtató**, a DC/OS Universe Vamp csomag is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="69fdb-141">Once Elasticsearch reports as **Running**, you can add the Vamp DC/OS Universe package.</span></span> 

1. <span data-ttu-id="69fdb-142">Ugrás a **Universe** keresse meg a **vamp**.</span><span class="sxs-lookup"><span data-stu-id="69fdb-142">Go to **Universe** and search for **vamp**.</span></span> 
  <span data-ttu-id="69fdb-143">![A DC/OS universe vamp](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)</span><span class="sxs-lookup"><span data-stu-id="69fdb-143">![Vamp on DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)</span></span>

2. <span data-ttu-id="69fdb-144">Kattintson a **telepítése** mellett a vamp csomagot, majd válassza a **speciális telepítési**.</span><span class="sxs-lookup"><span data-stu-id="69fdb-144">Click **install** next to the vamp package, and choose **Advanced Installation**.</span></span>

3. <span data-ttu-id="69fdb-145">Görgessen lefelé, és írja be a következő elasticsearch-URL-cím: `http://elasticsearch.marathon.mesos:9200`.</span><span class="sxs-lookup"><span data-stu-id="69fdb-145">Scroll down and enter the following elasticsearch-url: `http://elasticsearch.marathon.mesos:9200`.</span></span> 

  ![Adja meg a Elasticsearch URL-címe](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. <span data-ttu-id="69fdb-147">Kattintson a **áttekintése és telepítése**, majd kattintson a **telepítése** a telepítés elindításához.</span><span class="sxs-lookup"><span data-stu-id="69fdb-147">Click **Review and Install**, then click **Install** to start the deployment.</span></span>  

  <span data-ttu-id="69fdb-148">A DC/OS Vamp minden szükséges összetevőket telepíti.</span><span class="sxs-lookup"><span data-stu-id="69fdb-148">DC/OS deploys all required Vamp components.</span></span> <span data-ttu-id="69fdb-149">A végrehajtás nyomon a **szolgáltatások** lap.</span><span class="sxs-lookup"><span data-stu-id="69fdb-149">You can track progress on the **Services** page.</span></span>
  
  ![Vamp universe csomag telepítése](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. <span data-ttu-id="69fdb-151">Központi telepítés befejezése után a Vamp felhasználói felület érhető el:</span><span class="sxs-lookup"><span data-stu-id="69fdb-151">Once deployment has completed, you can access the Vamp UI:</span></span>

  ![A DC/OS vamp szolgáltatás](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
  ![Felhasználói felület vamp](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a><span data-ttu-id="69fdb-154">Az első szolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="69fdb-154">Deploy your first service</span></span>

<span data-ttu-id="69fdb-155">Most, hogy Vamp működik-e és fut, a tervezetének a szolgáltatás telepítése.</span><span class="sxs-lookup"><span data-stu-id="69fdb-155">Now that Vamp is up and running, deploy a service from a blueprint.</span></span> 

<span data-ttu-id="69fdb-156">Legegyszerűbb formájukban a [Vamp tervezetének](http://vamp.io/documentation/using-vamp/blueprints/) a végpontok (átjárók), a fürtök és a telepítendő szolgáltatások ismerteti.</span><span class="sxs-lookup"><span data-stu-id="69fdb-156">In its simplest form, a [Vamp blueprint](http://vamp.io/documentation/using-vamp/blueprints/) describes the endpoints (gateways), clusters, and services to deploy.</span></span> <span data-ttu-id="69fdb-157">Vamp fürtök használja ugyanazt a szolgáltatást különböző változatai csoportot logikai csoportokba Kanári feloldása vagy A / B tesztelés.</span><span class="sxs-lookup"><span data-stu-id="69fdb-157">Vamp uses clusters to group different variants of the same service into logical groups for canary releasing or A/B testing.</span></span>  

<span data-ttu-id="69fdb-158">Ez a forgatókönyv használ nevű egységes mintaalkalmazás [ **Száva**](https://github.com/magneticio/sava), amely jelenleg 1.0-s verziója.</span><span class="sxs-lookup"><span data-stu-id="69fdb-158">This scenario uses a sample monolithic application called [**sava**](https://github.com/magneticio/sava), which is at version 1.0.</span></span> <span data-ttu-id="69fdb-159">A monolit egy Docker-tároló, amely Docker központban magneticio/sava:1.0.0 alatt van csomagolva.</span><span class="sxs-lookup"><span data-stu-id="69fdb-159">The monolith is packaged in a Docker container, which is in Docker Hub under magneticio/sava:1.0.0.</span></span> <span data-ttu-id="69fdb-160">Az alkalmazás megfelelően fut a 8080-as porton, de azt szeretné, ebben az esetben tenni a port 9050.</span><span class="sxs-lookup"><span data-stu-id="69fdb-160">The app normally runs on port 8080, but you want to expose it under port 9050 in this case.</span></span> <span data-ttu-id="69fdb-161">Telepítse az alkalmazást egy egyszerű tervezetének használatával Vamp keresztül.</span><span class="sxs-lookup"><span data-stu-id="69fdb-161">Deploy the app through Vamp using a simple blueprint.</span></span>

1. <span data-ttu-id="69fdb-162">Ugrás a **központi telepítések**.</span><span class="sxs-lookup"><span data-stu-id="69fdb-162">Go to **Deployments**.</span></span>

2. <span data-ttu-id="69fdb-163">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="69fdb-163">Click **Add**.</span></span>

3. <span data-ttu-id="69fdb-164">Illessze be a következő szerkezeti terve YAM.</span><span class="sxs-lookup"><span data-stu-id="69fdb-164">Paste in the following blueprint YAML.</span></span> <span data-ttu-id="69fdb-165">Ez tervezetének egy fürt csak egy szolgáltatás variant, amely egy későbbi lépésben módosítjuk tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="69fdb-165">This blueprint contains one cluster with only one service variant, which we change in a later step:</span></span>

  ```YAML
  name: sava                        # deployment name
  gateways:
    9050: sava_cluster/webport      # stable endpoint
  clusters:
    sava_cluster:               # cluster to create
     services:
        -
          breed:
            name: sava:1.0.0        # service variant name
            deployable: magneticio/sava:1.0.0
            ports:
              webport: 8080/http # cluster endpoint, used for canary releasing
  ```

4. <span data-ttu-id="69fdb-166">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="69fdb-166">Click **Save**.</span></span> <span data-ttu-id="69fdb-167">Vamp kezdeményezi a központi telepítést.</span><span class="sxs-lookup"><span data-stu-id="69fdb-167">Vamp initiates the deployment.</span></span>

<span data-ttu-id="69fdb-168">A központi telepítés szerepel a **központi telepítések** lap.</span><span class="sxs-lookup"><span data-stu-id="69fdb-168">The deployment is listed on the **Deployments** page.</span></span> <span data-ttu-id="69fdb-169">Kattintson a telepítés állapotának figyelésére.</span><span class="sxs-lookup"><span data-stu-id="69fdb-169">Click the deployment to monitor its status.</span></span>

![Vamp UI - Száva telepítése](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![a felhasználói felületen Vamp Száva szolgáltatás](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

<span data-ttu-id="69fdb-172">Két átjáró jönnek létre, amely szerepel a **átjárók** lap:</span><span class="sxs-lookup"><span data-stu-id="69fdb-172">Two gateways are created, which are listed on the **Gateways** page:</span></span>

* <span data-ttu-id="69fdb-173">egy stabil végponton keresztül biztosít hozzáférést a futó szolgáltatás (port 9050)</span><span class="sxs-lookup"><span data-stu-id="69fdb-173">a stable endpoint to access the running service (port 9050)</span></span> 
* <span data-ttu-id="69fdb-174">Vamp által kezelt belső átjáró (erről az átjáróról később további).</span><span class="sxs-lookup"><span data-stu-id="69fdb-174">a Vamp-managed internal gateway (more on this gateway later).</span></span> 

![Felhasználói felület – Száva átjárók vamp](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

<span data-ttu-id="69fdb-176">A Száva szolgáltatás már telepítve van, de nem férhet hozzá az külsőleg, mert az Azure Load Balancer hozzá forgalom továbbítására még nem ismert.</span><span class="sxs-lookup"><span data-stu-id="69fdb-176">The sava service has now deployed, but you can’t access it externally because the Azure Load Balancer doesn’t know to forward traffic to it yet.</span></span> <span data-ttu-id="69fdb-177">A szolgáltatás eléréséhez Azure-alapú hálózati beállításainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="69fdb-177">To access the service, update the Azure networking configuration.</span></span>


## <a name="update-the-azure-network-configuration"></a><span data-ttu-id="69fdb-178">Az Azure-hálózat konfiguráció frissítése</span><span class="sxs-lookup"><span data-stu-id="69fdb-178">Update the Azure network configuration</span></span>

<span data-ttu-id="69fdb-179">Vamp üzembe a DC/OS ügynök csomópontok, egy stabil végpont-es porton 9050 kitettségének Száva szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="69fdb-179">Vamp deployed the sava service on the DC/OS agent nodes, exposing a stable endpoint at port 9050.</span></span> <span data-ttu-id="69fdb-180">A szolgáltatás a DC/OS-fürtön kívüli eléréséhez a következő módosításokat az Azure hálózati konfigurációhoz a fürtöt tartalmazó környezetben:</span><span class="sxs-lookup"><span data-stu-id="69fdb-180">To access the service from outside the DC/OS cluster, make the following changes to the Azure network configuration in your cluster deployment:</span></span> 

1. <span data-ttu-id="69fdb-181">**Konfigurálja az Azure Load Balancer** az ügynökök (nevű erőforrás **vezénylőtípusú-ügynök-lb-xxxx**) egy állapotmintáihoz és egy szabályt, amely továbbítsa a forgalmat a Száva példányokhoz 9050 porton.</span><span class="sxs-lookup"><span data-stu-id="69fdb-181">**Configure the Azure Load Balancer** for the agents (the resource named **dcos-agent-lb-xxxx**) with a health probe and a rule to forward traffic on port 9050 to the sava instances.</span></span> 

2. <span data-ttu-id="69fdb-182">**A hálózati biztonsági csoport** a nyilvános ügynökök (nevű erőforrás **XXXX-ügynök – nyilvános-nsg-XXXX**) forgalmat engedélyezi a port 9050.</span><span class="sxs-lookup"><span data-stu-id="69fdb-182">**Update the network security group** for the public agents (the resource named **XXXX-agent-public-nsg-XXXX**) to allow traffic on port 9050.</span></span>

<span data-ttu-id="69fdb-183">Az Azure portál használatával a feladatok elvégzéséhez részletes lépéseiért lásd: [Azure Tárolószolgáltatás alkalmazáshoz való nyilvános hozzáférés engedélyezésére](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="69fdb-183">For detailed steps to complete these tasks using the Azure portal, see [Enable public access to an Azure Container Service application](container-service-enable-public-access.md).</span></span> <span data-ttu-id="69fdb-184">Adja meg az összes portbeállítások 9050 portot.</span><span class="sxs-lookup"><span data-stu-id="69fdb-184">Specify port 9050 for all port settings.</span></span>


<span data-ttu-id="69fdb-185">Minden létrehozása, navigáljon a **áttekintése** panel terheléselosztó DC/OS-ügynök (nevű erőforrás **vezénylőtípusú-ügynök-lb-xxxx**).</span><span class="sxs-lookup"><span data-stu-id="69fdb-185">Once everything has been created, go to the **Overview** blade of the DC/OS agent load balancer (the resource named **dcos-agent-lb-xxxx**).</span></span> <span data-ttu-id="69fdb-186">Keresés a **nyilvános IP-cím**, és a cím: port 9050 Száva eléréséhez használja.</span><span class="sxs-lookup"><span data-stu-id="69fdb-186">Find the **Public IP address**, and use the address to access sava at port 9050.</span></span>

![Azure portál – get nyilvános IP-cím](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![Száva](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a><span data-ttu-id="69fdb-189">Kanári kiadási futtatása</span><span class="sxs-lookup"><span data-stu-id="69fdb-189">Run a canary release</span></span>

<span data-ttu-id="69fdb-190">Tegyük fel egy új verziója, amelyet az alkalmazás Kanári kiadásához éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="69fdb-190">Suppose you have a new version of this application that you want to canary release into production.</span></span> <span data-ttu-id="69fdb-191">Annak indexelése magneticio/sava:1.1.0, és készen állnak.</span><span class="sxs-lookup"><span data-stu-id="69fdb-191">You have it containerized as magneticio/sava:1.1.0 and are ready to go.</span></span> <span data-ttu-id="69fdb-192">Vamp lehetővé teszi, hogy könnyen vehet fel új szolgáltatások a futó központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="69fdb-192">Vamp lets you easily add new services to the running deployment.</span></span> <span data-ttu-id="69fdb-193">Ezek a szolgáltatások "egyesített" mellett a meglévő szolgáltatások a fürtben telepített, és egy 0 %-os értéket kapó.</span><span class="sxs-lookup"><span data-stu-id="69fdb-193">These "merged" services are deployed alongside the existing services in the cluster, and assigned a weight of 0%.</span></span> <span data-ttu-id="69fdb-194">Nem továbbítódik újonnan egyesített szolgáltatás mindaddig, amíg a forgalom eloszlása módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="69fdb-194">No traffic is routed to a newly merged service until you adjust the traffic distribution.</span></span> <span data-ttu-id="69fdb-195">A Vamp felhasználói felületén súly csúszkája lehetővé teszi a terjesztési, lehetővé teszi a növekményes módosításának (Kanári kiadás) vagy egy azonnali visszavonás teljes ellenőrzését.</span><span class="sxs-lookup"><span data-stu-id="69fdb-195">The weight slider in the Vamp UI gives you complete control over the distribution, allowing for incremental adjustments (canary release) or an instant rollback.</span></span>

### <a name="merge-a-new-service-variant"></a><span data-ttu-id="69fdb-196">Egy új szolgáltatás variant egyesítése</span><span class="sxs-lookup"><span data-stu-id="69fdb-196">Merge a new service variant</span></span>

<span data-ttu-id="69fdb-197">Az új Száva 1.1-bA a futó telepítési egyesíteni:</span><span class="sxs-lookup"><span data-stu-id="69fdb-197">To merge the new sava 1.1 service with the running deployment:</span></span>

1. <span data-ttu-id="69fdb-198">A Vamp felhasználói felületén kattintson **tervrajzokat**.</span><span class="sxs-lookup"><span data-stu-id="69fdb-198">In the Vamp UI, click **Blueprints**.</span></span>

2. <span data-ttu-id="69fdb-199">Kattintson a **Hozzáadás** és illessze be a következő szerkezeti terve YAM: A tervezetének ismerteti a meglévő fürt (sava_cluster) telepítése egy új szolgáltatás variant (Száva: 1.1.0-ás).</span><span class="sxs-lookup"><span data-stu-id="69fdb-199">Click **Add** and paste in the following blueprint YAML: This blueprint describes a new service variant (sava:1.1.0) to deploy within the existing cluster (sava_cluster).</span></span>

  ```YAML
  name: sava:1.1.0      # blueprint name
  clusters:
    sava_cluster:       # cluster to update
      services:
        -
          breed:
            name: sava:1.1.0    # service variant name
            deployable: magneticio/sava:1.1.0    
            ports:
              webport: 8080/http # cluster endpoint to update
  ```
  
3. <span data-ttu-id="69fdb-200">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="69fdb-200">Click **Save**.</span></span> <span data-ttu-id="69fdb-201">Szerkezeti terve tárolja, és szerepel a **tervrajzokat** lap.</span><span class="sxs-lookup"><span data-stu-id="69fdb-201">The blueprint is stored and listed on the **Blueprints** page.</span></span>

4. <span data-ttu-id="69fdb-202">Nyissa meg a művelet menü Száva: 1.1 szerkezeti terve a, és kattintson a **egyesítése**.</span><span class="sxs-lookup"><span data-stu-id="69fdb-202">Open the action menu on the sava:1.1 blueprint and click **Merge to**.</span></span>

  ![Felhasználói felület – tervrajzokat vamp](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. <span data-ttu-id="69fdb-204">Válassza ki a **Száva** üzembe helyezési és kattintson **egyesítése**.</span><span class="sxs-lookup"><span data-stu-id="69fdb-204">Select the **sava** deployment and click **Merge**.</span></span>

  ![Felhasználói felület - telepítéshez egyesítési tervezetének vamp](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

<span data-ttu-id="69fdb-206">Vamp telepíti az új Száva: 1.1.0-ás szolgáltatás variant mellett Száva: 1.0.0 a szerkezeti terve ismertetett a **sava_cluster** a futó központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="69fdb-206">Vamp deploys the new sava:1.1.0 service variant described in the blueprint alongside sava:1.0.0 in the **sava_cluster** of the running deployment.</span></span> 

![Felhasználói felület – frissített Száva telepítési vamp](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

<span data-ttu-id="69fdb-208">A **Száva/sava_cluster/webport** átjáró (a fürt végpontja) is frissül, egy útvonal hozzáadása az újonnan telepített Száva: 1.1.0-ás.</span><span class="sxs-lookup"><span data-stu-id="69fdb-208">The **sava/sava_cluster/webport** gateway (the cluster endpoint) is also updated, adding a route to the newly deployed sava:1.1.0.</span></span> <span data-ttu-id="69fdb-209">Ezen a ponton nem továbbítódik itt (a **súly** 0 % értékre van állítva).</span><span class="sxs-lookup"><span data-stu-id="69fdb-209">At this point, no traffic is routed here (the **WEIGHT** is set to 0%).</span></span>

![Felhasználói felület - fürt átjáró vamp](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a><span data-ttu-id="69fdb-211">Kanári kiadás</span><span class="sxs-lookup"><span data-stu-id="69fdb-211">Canary release</span></span>

<span data-ttu-id="69fdb-212">Mindkét Száva ugyanabban a fürtben telepített verziója, a közöttük át forgalmat beállítása a **súly** csúszkát.</span><span class="sxs-lookup"><span data-stu-id="69fdb-212">With both versions of sava deployed in the same cluster, adjust the distribution of traffic between them by moving the **WEIGHT** slider.</span></span>

1. <span data-ttu-id="69fdb-213">Kattintson a ![Vamp UI - szerkesztése](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) melletti **súly**.</span><span class="sxs-lookup"><span data-stu-id="69fdb-213">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) next to **WEIGHT**.</span></span>

2. <span data-ttu-id="69fdb-214">50 % vagy 50 %, majd kattintson a súlyozások elosztása beállítása **mentése**.</span><span class="sxs-lookup"><span data-stu-id="69fdb-214">Set the weight distribution to 50%/50% and click **Save**.</span></span>

  ![Felhasználói felület – átjáró súly csúszkát vamp](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. <span data-ttu-id="69fdb-216">Lépjen vissza a böngészőt, és frissítse a Száva oldalt néhány alkalommal.</span><span class="sxs-lookup"><span data-stu-id="69fdb-216">Go back to your browser and refresh the sava page a few more times.</span></span> <span data-ttu-id="69fdb-217">A Száva alkalmazást vált egy Száva: 1.0 lap és Száva: 1.1 oldal között.</span><span class="sxs-lookup"><span data-stu-id="69fdb-217">The sava application now switches between a sava:1.0 page and a sava:1.1 page.</span></span>

  ![váltakozó sava1.0 és sava1.1 szolgáltatások](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > <span data-ttu-id="69fdb-219">Ez a lap váltási működik a legjobban a "Incognito" vagy "Névtelen" módban, a böngésző statikus rendezett gyorsítótárazása miatt.</span><span class="sxs-lookup"><span data-stu-id="69fdb-219">This alternation of the page works best with the "Incognito" or “Anonymous” mode of your browser because of the caching of static assets.</span></span>
  >

### <a name="filter-traffic"></a><span data-ttu-id="69fdb-220">Szűrő forgalom</span><span class="sxs-lookup"><span data-stu-id="69fdb-220">Filter traffic</span></span>

<span data-ttu-id="69fdb-221">Tegyük fel, hogy a központi telepítést követően, hogy a inkompatibilitás Száva: 1.1.0-ás okok Firefox böngészőket problémákat jeleníti meg a felderíteni.</span><span class="sxs-lookup"><span data-stu-id="69fdb-221">Suppose after deployment that you discovered an incompatibility in sava:1.1.0 that causes display problems in Firefox browsers.</span></span> <span data-ttu-id="69fdb-222">Bejövő forgalom szűrésére, és az ismert stabil Száva: 1.0.0 biztonsági senki Firefox közvetlen Vamp állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="69fdb-222">You can set Vamp to filter incoming traffic and direct all Firefox users back to the known stable sava:1.0.0.</span></span> <span data-ttu-id="69fdb-223">Ez a szűrő azonnal miközben mindenki más tovább teszik a továbbfejlesztett Száva: 1.1.0-ás az oldja fel a megszakítás a Firefox használó felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="69fdb-223">This filter instantly resolves the disruption for Firefox users, while everybody else continues to enjoy the benefits of the improved sava:1.1.0.</span></span>

<span data-ttu-id="69fdb-224">Felhasználási vamp **feltételek** útvonal az átjáró közötti forgalom szűrésére.</span><span class="sxs-lookup"><span data-stu-id="69fdb-224">Vamp uses **conditions** to filter traffic between routes in a gateway.</span></span> <span data-ttu-id="69fdb-225">Adatforgalom először szűrt és irányítja a rendszer minden útvonal használt feltételek szerint.</span><span class="sxs-lookup"><span data-stu-id="69fdb-225">Traffic is first filtered and directed according to the conditions applied to each route.</span></span> <span data-ttu-id="69fdb-226">Az összes többi forgalom az átjáró súly beállításnak megfelelően történik.</span><span class="sxs-lookup"><span data-stu-id="69fdb-226">All remaining traffic is distributed according to the gateway weight setting.</span></span>

<span data-ttu-id="69fdb-227">Létrehozhat olyan állapotban, hogy senki Firefox szűrése forgalmazójával, és a régi Száva: 1.0.0:</span><span class="sxs-lookup"><span data-stu-id="69fdb-227">You can create a condition to filter all Firefox users and direct them to the old sava:1.0.0:</span></span>

1. <span data-ttu-id="69fdb-228">A Száva/sava_cluster/webport **átjárók** lapján kattintson ![Vamp UI - szerkesztése](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) hozzáadása egy **feltétel** az útvonal sava/sava_cluster/sava:1.0.0/webport számára.</span><span class="sxs-lookup"><span data-stu-id="69fdb-228">On the sava/sava_cluster/webport **Gateways** page, click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) to add a **CONDITION** to the route sava/sava_cluster/sava:1.0.0/webport.</span></span> 

2. <span data-ttu-id="69fdb-229">Adja meg a feltétel **felhasználói ügynök == Firefox** kattintson ![Vamp UI - mentése](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).</span><span class="sxs-lookup"><span data-stu-id="69fdb-229">Enter the condition **user-agent == Firefox** and click ![Vamp UI - save](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).</span></span>

  <span data-ttu-id="69fdb-230">Vamp felveszi a feltételt, amelynek alapértelmezett 0 %.</span><span class="sxs-lookup"><span data-stu-id="69fdb-230">Vamp adds the condition with a default strength of 0%.</span></span> <span data-ttu-id="69fdb-231">Forgalom megkezdéséhez szüksége úgy, hogy a feltétel erőssége.</span><span class="sxs-lookup"><span data-stu-id="69fdb-231">To start filtering traffic, you need to adjust the condition strength.</span></span>

3. <span data-ttu-id="69fdb-232">Kattintson a ![Vamp UI - szerkesztése](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) módosítása a **erőssége** alkalmazza a feltételnek.</span><span class="sxs-lookup"><span data-stu-id="69fdb-232">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) to change the **STRENGTH** applied to the condition.</span></span>
 
4. <span data-ttu-id="69fdb-233">Állítsa be a **erőssége** 100 %-os, majd kattintson ![Vamp UI - mentése](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="69fdb-233">Set the **STRENGTH** to 100% and click ![Vamp UI - save](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) to save.</span></span>

  <span data-ttu-id="69fdb-234">Vamp most küld az összes forgalom megfelelő Száva: 1.0.0 megtagadásra (minden felhasználó Firefox).</span><span class="sxs-lookup"><span data-stu-id="69fdb-234">Vamp now sends all traffic matching the condition (all Firefox users) to sava:1.0.0.</span></span>

  ![Felhasználói felület vamp - feltétel érvényes átjáró](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. <span data-ttu-id="69fdb-236">Végül módosítsa az átjáró súlyozást az összes többi forgalom (minden felhasználó nem Firefox) küldhet az új Száva: 1.1.0-ás.</span><span class="sxs-lookup"><span data-stu-id="69fdb-236">Finally, adjust the gateway weight to send all remaining traffic (all non-Firefox users) to the new sava:1.1.0.</span></span> <span data-ttu-id="69fdb-237">Kattintson a ![Vamp UI - szerkesztése](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) melletti **súly** és állítsa be a súlyozások elosztása, így a 100 %-át van irányítva az útvonal sava/sava_cluster/sava:1.1.0/webport.</span><span class="sxs-lookup"><span data-stu-id="69fdb-237">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) next to **WEIGHT** and set the weight distribution so 100% is directed to the route sava/sava_cluster/sava:1.1.0/webport.</span></span>

  <span data-ttu-id="69fdb-238">Az összes forgalom a feltétel nem szűrt most már az új Száva: 1.1.0-ás van átirányítva.</span><span class="sxs-lookup"><span data-stu-id="69fdb-238">All traffic not filtered by the condition is now directed to the new sava:1.1.0.</span></span>

6. <span data-ttu-id="69fdb-239">A művelet a szűrő megtekintéséhez nyissa meg két különböző böngészők (egy Firefox és egy másik böngészőben), és éri el a Száva szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="69fdb-239">To see the filter in action, open two different browsers (one Firefox and one other browser) and access the sava service from both.</span></span> <span data-ttu-id="69fdb-240">Összes Firefox kérelem küldése a Száva: 1.0.0, míg minden más böngészők Száva: 1.1.0-ás van irányítva.</span><span class="sxs-lookup"><span data-stu-id="69fdb-240">All Firefox requests are sent to sava:1.0.0, while all other browsers are directed to sava:1.1.0.</span></span>

  ![Felhasználói felület - forgalom szűrésére vamp](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a><span data-ttu-id="69fdb-242">Összegzése</span><span class="sxs-lookup"><span data-stu-id="69fdb-242">Summing up</span></span>

<span data-ttu-id="69fdb-243">Ez a cikk lett egy gyors bevezetés Vamp a DC/OS-fürtön.</span><span class="sxs-lookup"><span data-stu-id="69fdb-243">This article was a quick introduction to Vamp on a DC/OS cluster.</span></span> <span data-ttu-id="69fdb-244">Első Vamp portáltól és fut a az Azure tároló szolgáltatás DC/OS fürt, telepített egy szolgáltatást a egy Vamp tervezetének, címen érhető el: a kitett végpont (átjáró).</span><span class="sxs-lookup"><span data-stu-id="69fdb-244">For starters, you got Vamp up and running on your Azure Container Service DC/OS cluster, deployed a service with a Vamp blueprint, and accessed it at the exposed endpoint (gateway).</span></span>

<span data-ttu-id="69fdb-245">Azt is az egyes hatékony szolgáltatásainak Vamp érint: egyesítése egy új szolgáltatás variant futó központi és bevezeti a Növekményesen, majd egy ismert kompatibilitási megoldásához forgalom.</span><span class="sxs-lookup"><span data-stu-id="69fdb-245">We also touched on some powerful features of Vamp:  merging a new service variant to the running deployment and introducing it incrementally, then filtering traffic to resolve a known incompatibility.</span></span>


## <a name="next-steps"></a><span data-ttu-id="69fdb-246">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="69fdb-246">Next steps</span></span>

* <span data-ttu-id="69fdb-247">Vamp műveletek keresztül kezeléséről további információt a [Vamp a REST API-t](http://vamp.io/documentation/api/api-reference/).</span><span class="sxs-lookup"><span data-stu-id="69fdb-247">Learn about managing Vamp actions through the [Vamp REST API](http://vamp.io/documentation/api/api-reference/).</span></span>

* <span data-ttu-id="69fdb-248">A node.js Vamp automatizálási parancsfájlokat létrehozása és futtatása azokat [munkafolyamatok Vamp](http://vamp.io/documentation/tutorials/create-a-workflow/).</span><span class="sxs-lookup"><span data-stu-id="69fdb-248">Build Vamp automation scripts in Node.js and run them as [Vamp workflows](http://vamp.io/documentation/tutorials/create-a-workflow/).</span></span>

* <span data-ttu-id="69fdb-249">További részletek [VAMP oktatóanyagok](http://vamp.io/documentation/tutorials/overview/).</span><span class="sxs-lookup"><span data-stu-id="69fdb-249">See additional [VAMP tutorials](http://vamp.io/documentation/tutorials/overview/).</span></span>

