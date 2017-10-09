---
title: "Azure DC/OS-fürtön a Vamp aaaCanary kiadás |} Microsoft Docs"
description: "Hogyan toouse Vamp toocanary kiadás szolgáltatások, és egy Azure tároló szolgáltatás DC/OS-fürtről végez a intelligens forgalom alkalmazása"
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
ms.openlocfilehash: e7b8658a161a7cddcf718e3e1c12a889a330d3d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="262d2-103">Egy Azure tároló szolgáltatás DC/OS-fürtön a Vamp Kanári kiadás mikroszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="262d2-103">Canary release microservices with Vamp on an Azure Container Service DC/OS cluster</span></span>

<span data-ttu-id="262d2-104">Ebben a bemutatóban beállítjuk Vamp Azure tárolószolgáltatás és a DC/OS-fürtről.</span><span class="sxs-lookup"><span data-stu-id="262d2-104">In this walkthrough, we set up Vamp on Azure Container Service with a DC/OS cluster.</span></span> <span data-ttu-id="262d2-105">Azt Kanári szabadítsa fel a hello Vamp bemutató szolgáltatás "Száva", és oldja meg a Firefox hello szolgáltatást inkompatibilitás intelligens forgalomszűrést végez alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="262d2-105">We canary release hello Vamp demo service "sava", and then resolve an incompatibility of hello service with Firefox by applying smart traffic filtering.</span></span> 

> [!TIP] 
> <span data-ttu-id="262d2-106">A forgatókönyv Vamp a DC/OS-fürtön fut, de akkor is használható Vamp Kubernetes a hello orchestrator.</span><span class="sxs-lookup"><span data-stu-id="262d2-106">In this walkthrough Vamp runs on a DC/OS cluster, but you can also use Vamp with Kubernetes as hello orchestrator.</span></span>
>

## <a name="about-canary-releases-and-vamp"></a><span data-ttu-id="262d2-107">Feloldja a Kanári kapcsolatos és Vamp</span><span class="sxs-lookup"><span data-stu-id="262d2-107">About canary releases and Vamp</span></span>


<span data-ttu-id="262d2-108">[Kanári felszabadítása](https://martinfowler.com/bliki/CanaryRelease.html) például Netflix, a Facebook-on és a Spotify új szervezetek által elfogadott intelligens központi telepítési stratégiát is.</span><span class="sxs-lookup"><span data-stu-id="262d2-108">[Canary releasing](https://martinfowler.com/bliki/CanaryRelease.html) is a smart deployment strategy adopted by innovative organizations like Netflix, Facebook, and Spotify.</span></span> <span data-ttu-id="262d2-109">Értelme, mivel csökkenti a problémákat, biztonsági-háló vezet be, és növeli az innováció egy megközelítést.</span><span class="sxs-lookup"><span data-stu-id="262d2-109">It’s an approach that makes sense, because it reduces issues, introduces safety-nets, and increases innovation.</span></span> <span data-ttu-id="262d2-110">Ezért miért nem minden vállalat használja azt?</span><span class="sxs-lookup"><span data-stu-id="262d2-110">So why aren’t all companies using it?</span></span> <span data-ttu-id="262d2-111">A CI/CD adatcsatorna tooinclude Kanári stratégiák kiterjesztése hozzáadja összetettségét, majd kiterjedt devops tudásuk és tapasztalataik igényel.</span><span class="sxs-lookup"><span data-stu-id="262d2-111">Extending a CI/CD pipeline tooinclude canary strategies adds complexity, and requires extensive devops knowledge and experience.</span></span> <span data-ttu-id="262d2-112">Ez elég tooblock kisebb vállalatok és a vállalatok egyaránt ahhoz, azok még a kezdéshez.</span><span class="sxs-lookup"><span data-stu-id="262d2-112">That’s enough tooblock smaller companies and enterprises alike before they even get started.</span></span> 

<span data-ttu-id="262d2-113">[Vamp](http://vamp.io/) egy nyílt forráskódú rendszert tooease Ez a változás, és kapcsolja a szolgáltatások előnyben részesített tooyour Kanári lazító tároló Feladatütemező.</span><span class="sxs-lookup"><span data-stu-id="262d2-113">[Vamp](http://vamp.io/) is an open-source system designed tooease this transition and bring canary releasing features tooyour preferred container scheduler.</span></span> <span data-ttu-id="262d2-114">Vamp tartozó Kanári funkció túllép százalék alapú exportálása.</span><span class="sxs-lookup"><span data-stu-id="262d2-114">Vamp’s canary functionality goes beyond percentage-based rollouts.</span></span> <span data-ttu-id="262d2-115">Forgalom szűrve, és számos különböző feltételek, például adott felhasználók tootarget, IP-címtartományok vagy eszközök felosztással állít elő.</span><span class="sxs-lookup"><span data-stu-id="262d2-115">Traffic can be filtered and split on a wide range of conditions, for example tootarget specific users, IP-ranges, or devices.</span></span> <span data-ttu-id="262d2-116">Vamp nyomon követi, és elemzi a teljesítménymutatók, lehetővé teszi a automation valós adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="262d2-116">Vamp tracks and analyzes performance metrics, allowing for automation based on real-world data.</span></span> <span data-ttu-id="262d2-117">A hibák automatikus visszaállítási beállítása, vagy méretezni terhelés alapján vagy késés szolgáltatás Variant adattípusban.</span><span class="sxs-lookup"><span data-stu-id="262d2-117">You can set up automatic rollback on errors, or scale individual service variants based on load or latency.</span></span>

## <a name="set-up-azure-container-service-with-dcos"></a><span data-ttu-id="262d2-118">Azure Tárolószolgáltatási DC/OS rendelkező beállítása</span><span class="sxs-lookup"><span data-stu-id="262d2-118">Set up Azure Container Service with DC/OS</span></span>



1. <span data-ttu-id="262d2-119">[A DC/OS-fürt üzembe](container-service-deployment.md) egy fő és a két ügynök alapértelmezett mérete.</span><span class="sxs-lookup"><span data-stu-id="262d2-119">[Deploy a DC/OS cluster](container-service-deployment.md) with one master and two agents of default size.</span></span> 

2. <span data-ttu-id="262d2-120">[SSH-alagút létrehozása](../container-service-connect.md) tooconnect toohello DC/OS-fürtről.</span><span class="sxs-lookup"><span data-stu-id="262d2-120">[Create an SSH tunnel](../container-service-connect.md) tooconnect toohello DC/OS cluster.</span></span> <span data-ttu-id="262d2-121">Ez a cikk feltételezi, hogy a 80-as portjához helyi toohello fürt alagútkezelésre.</span><span class="sxs-lookup"><span data-stu-id="262d2-121">This article assumes that you tunnel toohello cluster on local port 80.</span></span>


## <a name="set-up-vamp"></a><span data-ttu-id="262d2-122">Vamp beállítása</span><span class="sxs-lookup"><span data-stu-id="262d2-122">Set up Vamp</span></span>

<span data-ttu-id="262d2-123">Most, hogy a futó DC/OS-fürtről, a DC/OS felhasználói felületének (http://localhost:80) hello Vamp is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="262d2-123">Now that you have a running DC/OS cluster, you can install Vamp from hello DC/OS UI (http://localhost:80).</span></span> 

![A DC/OS UI felhasználói felülete](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

<span data-ttu-id="262d2-125">Telepítés két lépésben történik:</span><span class="sxs-lookup"><span data-stu-id="262d2-125">Installation is done in two stages:</span></span>

1. <span data-ttu-id="262d2-126">**Elasticsearch telepítése**.</span><span class="sxs-lookup"><span data-stu-id="262d2-126">**Deploy Elasticsearch**.</span></span>

2. <span data-ttu-id="262d2-127">Majd **Vamp telepítése** hello Vamp DC/OS universe csomag telepítésével.</span><span class="sxs-lookup"><span data-stu-id="262d2-127">Then **deploy Vamp** by installing hello Vamp DC/OS universe package.</span></span>

### <a name="deploy-elasticsearch"></a><span data-ttu-id="262d2-128">Elasticsearch telepítése</span><span class="sxs-lookup"><span data-stu-id="262d2-128">Deploy Elasticsearch</span></span>

<span data-ttu-id="262d2-129">Vamp Elasticsearch metrikák adatgyűjtési és -összesítési igényel.</span><span class="sxs-lookup"><span data-stu-id="262d2-129">Vamp requires Elasticsearch for metrics collection and aggregation.</span></span> <span data-ttu-id="262d2-130">Használhatja a hello [magneticio Docker képek](https://hub.docker.com/r/magneticio/elastic/) toodeploy egy kompatibilis Vamp Elasticsearch verem.</span><span class="sxs-lookup"><span data-stu-id="262d2-130">You can use hello [magneticio Docker images](https://hub.docker.com/r/magneticio/elastic/) toodeploy a compatible Vamp Elasticsearch stack.</span></span>

1. <span data-ttu-id="262d2-131">A DC/OS felhasználói felületének hello, váltson túl**szolgáltatások** kattintson **szolgáltatás telepítése**.</span><span class="sxs-lookup"><span data-stu-id="262d2-131">In hello DC/OS UI, go too**Services** and click **Deploy Service**.</span></span>

2. <span data-ttu-id="262d2-132">Válassza ki **JSON üzemmódot** a hello **új szolgáltatás telepítése** előugró.</span><span class="sxs-lookup"><span data-stu-id="262d2-132">Select **JSON mode** from hello **Deploy New Service** pop-up.</span></span>

  ![Válassza ki a JSON üzemmód](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. <span data-ttu-id="262d2-134">Illessze be a következő JSON hello.</span><span class="sxs-lookup"><span data-stu-id="262d2-134">Paste in hello following JSON.</span></span> <span data-ttu-id="262d2-135">Ez a konfiguráció hello tárolóban fut, 1 GB RAM és hello Elasticsearch port alapszintű állapotának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="262d2-135">This configuration runs hello container with 1 GB of RAM and a basic health check on hello Elasticsearch port.</span></span>
  
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
  

3. <span data-ttu-id="262d2-136">Kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="262d2-136">Click **Deploy**.</span></span>

  <span data-ttu-id="262d2-137">A DC/OS hello Elasticsearch tároló telepíti.</span><span class="sxs-lookup"><span data-stu-id="262d2-137">DC/OS deploys hello Elasticsearch container.</span></span> <span data-ttu-id="262d2-138">Akkor tudja nyomon követni a hello **szolgáltatások** lap.</span><span class="sxs-lookup"><span data-stu-id="262d2-138">You can track progress on hello **Services** page.</span></span>  

  ![e telepítéséhez? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a><span data-ttu-id="262d2-140">Vamp telepítése</span><span class="sxs-lookup"><span data-stu-id="262d2-140">Deploy Vamp</span></span>

<span data-ttu-id="262d2-141">Miután Elasticsearch jelzi **futtató**, hello Vamp DC/OS Universe csomag is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="262d2-141">Once Elasticsearch reports as **Running**, you can add hello Vamp DC/OS Universe package.</span></span> 

1. <span data-ttu-id="262d2-142">Nyissa meg túl**Universe** keresse meg a **vamp**.</span><span class="sxs-lookup"><span data-stu-id="262d2-142">Go too**Universe** and search for **vamp**.</span></span> 
  <span data-ttu-id="262d2-143">![A DC/OS universe vamp](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)</span><span class="sxs-lookup"><span data-stu-id="262d2-143">![Vamp on DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)</span></span>

2. <span data-ttu-id="262d2-144">Kattintson a **telepítése** következő toohello csomag vamp, és válassza a **speciális telepítési**.</span><span class="sxs-lookup"><span data-stu-id="262d2-144">Click **install** next toohello vamp package, and choose **Advanced Installation**.</span></span>

3. <span data-ttu-id="262d2-145">Görgessen lefelé, és írja be a következő elasticsearch-url hello: `http://elasticsearch.marathon.mesos:9200`.</span><span class="sxs-lookup"><span data-stu-id="262d2-145">Scroll down and enter hello following elasticsearch-url: `http://elasticsearch.marathon.mesos:9200`.</span></span> 

  ![Adja meg a Elasticsearch URL-címe](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. <span data-ttu-id="262d2-147">Kattintson a **áttekintése és telepítése**, majd kattintson a **telepítése** toostart hello központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="262d2-147">Click **Review and Install**, then click **Install** toostart hello deployment.</span></span>  

  <span data-ttu-id="262d2-148">A DC/OS Vamp minden szükséges összetevőket telepíti.</span><span class="sxs-lookup"><span data-stu-id="262d2-148">DC/OS deploys all required Vamp components.</span></span> <span data-ttu-id="262d2-149">Akkor tudja nyomon követni a hello **szolgáltatások** lap.</span><span class="sxs-lookup"><span data-stu-id="262d2-149">You can track progress on hello **Services** page.</span></span>
  
  ![Vamp universe csomag telepítése](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. <span data-ttu-id="262d2-151">Központi telepítés befejezése után hello Vamp felhasználói felület érhető el:</span><span class="sxs-lookup"><span data-stu-id="262d2-151">Once deployment has completed, you can access hello Vamp UI:</span></span>

  ![A DC/OS vamp szolgáltatás](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
  ![Felhasználói felület vamp](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a><span data-ttu-id="262d2-154">Az első szolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="262d2-154">Deploy your first service</span></span>

<span data-ttu-id="262d2-155">Most, hogy Vamp működik-e és fut, a tervezetének a szolgáltatás telepítése.</span><span class="sxs-lookup"><span data-stu-id="262d2-155">Now that Vamp is up and running, deploy a service from a blueprint.</span></span> 

<span data-ttu-id="262d2-156">Legegyszerűbb formájukban a [Vamp tervezetének](http://vamp.io/documentation/using-vamp/blueprints/) hello végpontok (átjárók), a fürtök és a szolgáltatások toodeploy ismerteti.</span><span class="sxs-lookup"><span data-stu-id="262d2-156">In its simplest form, a [Vamp blueprint](http://vamp.io/documentation/using-vamp/blueprints/) describes hello endpoints (gateways), clusters, and services toodeploy.</span></span> <span data-ttu-id="262d2-157">Vamp használ fürtök toogroup különböző változatai hello azonos logikai csoportokba szolgáltatás Kanári feloldása vagy A / B tesztelés.</span><span class="sxs-lookup"><span data-stu-id="262d2-157">Vamp uses clusters toogroup different variants of hello same service into logical groups for canary releasing or A/B testing.</span></span>  

<span data-ttu-id="262d2-158">Ez a forgatókönyv használ nevű egységes mintaalkalmazás [ **Száva**](https://github.com/magneticio/sava), amely jelenleg 1.0-s verziója.</span><span class="sxs-lookup"><span data-stu-id="262d2-158">This scenario uses a sample monolithic application called [**sava**](https://github.com/magneticio/sava), which is at version 1.0.</span></span> <span data-ttu-id="262d2-159">hello monolit egy Docker-tároló, amely Docker központban magneticio/sava:1.0.0 alatt van csomagolva.</span><span class="sxs-lookup"><span data-stu-id="262d2-159">hello monolith is packaged in a Docker container, which is in Docker Hub under magneticio/sava:1.0.0.</span></span> <span data-ttu-id="262d2-160">hello alkalmazás megfelelően fut a 8080-as porton, de azt szeretné, hogy ebben az esetben port e 9050 tooexpose.</span><span class="sxs-lookup"><span data-stu-id="262d2-160">hello app normally runs on port 8080, but you want tooexpose it under port 9050 in this case.</span></span> <span data-ttu-id="262d2-161">Egy egyszerű tervezetének használatával Vamp keresztül hello alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="262d2-161">Deploy hello app through Vamp using a simple blueprint.</span></span>

1. <span data-ttu-id="262d2-162">Nyissa meg túl**központi telepítések**.</span><span class="sxs-lookup"><span data-stu-id="262d2-162">Go too**Deployments**.</span></span>

2. <span data-ttu-id="262d2-163">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="262d2-163">Click **Add**.</span></span>

3. <span data-ttu-id="262d2-164">Illessze be a következő hello tervezetének YAM.</span><span class="sxs-lookup"><span data-stu-id="262d2-164">Paste in hello following blueprint YAML.</span></span> <span data-ttu-id="262d2-165">Ez tervezetének egy fürt csak egy szolgáltatás variant, amely egy későbbi lépésben módosítjuk tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="262d2-165">This blueprint contains one cluster with only one service variant, which we change in a later step:</span></span>

  ```YAML
  name: sava                        # deployment name
  gateways:
    9050: sava_cluster/webport      # stable endpoint
  clusters:
    sava_cluster:               # cluster toocreate
     services:
        -
          breed:
            name: sava:1.0.0        # service variant name
            deployable: magneticio/sava:1.0.0
            ports:
              webport: 8080/http # cluster endpoint, used for canary releasing
  ```

4. <span data-ttu-id="262d2-166">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="262d2-166">Click **Save**.</span></span> <span data-ttu-id="262d2-167">Vamp hello telepítési kezdeményezi.</span><span class="sxs-lookup"><span data-stu-id="262d2-167">Vamp initiates hello deployment.</span></span>

<span data-ttu-id="262d2-168">hello telepítési szerepel hello **központi telepítések** lap.</span><span class="sxs-lookup"><span data-stu-id="262d2-168">hello deployment is listed on hello **Deployments** page.</span></span> <span data-ttu-id="262d2-169">Kattintson a hello telepítési toomonitor annak állapotát.</span><span class="sxs-lookup"><span data-stu-id="262d2-169">Click hello deployment toomonitor its status.</span></span>

![Vamp UI - Száva telepítése](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![a felhasználói felületen Vamp Száva szolgáltatás](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

<span data-ttu-id="262d2-172">Két átjáró jönnek létre, amely szerepel hello **átjárók** lap:</span><span class="sxs-lookup"><span data-stu-id="262d2-172">Two gateways are created, which are listed on hello **Gateways** page:</span></span>

* <span data-ttu-id="262d2-173">egy stabil végpont tooaccess hello szolgáltatást (port 9050) futtató</span><span class="sxs-lookup"><span data-stu-id="262d2-173">a stable endpoint tooaccess hello running service (port 9050)</span></span> 
* <span data-ttu-id="262d2-174">Vamp által kezelt belső átjáró (erről az átjáróról később további).</span><span class="sxs-lookup"><span data-stu-id="262d2-174">a Vamp-managed internal gateway (more on this gateway later).</span></span> 

![Felhasználói felület – Száva átjárók vamp](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

<span data-ttu-id="262d2-176">hello Száva szolgáltatás már telepítve van, de nem férhet hozzá az külsőleg mert hello Azure Load Balancer még nem ismert tooforward forgalom tooit.</span><span class="sxs-lookup"><span data-stu-id="262d2-176">hello sava service has now deployed, but you can’t access it externally because hello Azure Load Balancer doesn’t know tooforward traffic tooit yet.</span></span> <span data-ttu-id="262d2-177">tooaccess hello szolgáltatást, a frissítés hello Azure hálózati konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="262d2-177">tooaccess hello service, update hello Azure networking configuration.</span></span>


## <a name="update-hello-azure-network-configuration"></a><span data-ttu-id="262d2-178">Hello Azure hálózati konfiguráció frissítése</span><span class="sxs-lookup"><span data-stu-id="262d2-178">Update hello Azure network configuration</span></span>

<span data-ttu-id="262d2-179">Telepített vamp hello Száva szolgáltatás csomópontján hello DC/OS ügynök, egy stabil végpont-es porton 9050 teszi ki.</span><span class="sxs-lookup"><span data-stu-id="262d2-179">Vamp deployed hello sava service on hello DC/OS agent nodes, exposing a stable endpoint at port 9050.</span></span> <span data-ttu-id="262d2-180">tooaccess hello szolgáltatást külső hello DC/OS-fürtről, ellenőrizze a következő módosításokat toohello Azure hálózati konfigurációt a fürtöt tartalmazó környezetben hello:</span><span class="sxs-lookup"><span data-stu-id="262d2-180">tooaccess hello service from outside hello DC/OS cluster, make hello following changes toohello Azure network configuration in your cluster deployment:</span></span> 

1. <span data-ttu-id="262d2-181">**Azure Load Balancer hello konfigurálása** hello ügynökök (nevű erőforrás hello **vezénylőtípusú-ügynök-lb-xxxx**) egy állapotmintáihoz és a szabály tooforward forgalom port 9050 toohello Száva példányokon.</span><span class="sxs-lookup"><span data-stu-id="262d2-181">**Configure hello Azure Load Balancer** for hello agents (hello resource named **dcos-agent-lb-xxxx**) with a health probe and a rule tooforward traffic on port 9050 toohello sava instances.</span></span> 

2. <span data-ttu-id="262d2-182">**Hello hálózati biztonsági csoport** hello nyilvános ügynökök (nevű erőforrás hello **XXXX-ügynök – nyilvános-nsg-XXXX**) port 9050 tooallow forgalmát.</span><span class="sxs-lookup"><span data-stu-id="262d2-182">**Update hello network security group** for hello public agents (hello resource named **XXXX-agent-public-nsg-XXXX**) tooallow traffic on port 9050.</span></span>

<span data-ttu-id="262d2-183">A részletes lépéseket toocomplete ezek a feladatok az Azure portál, lásd: hello [nyilvános hozzáférés tooan Azure Tárolószolgáltatás-alkalmazás engedélyezése](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="262d2-183">For detailed steps toocomplete these tasks using hello Azure portal, see [Enable public access tooan Azure Container Service application](container-service-enable-public-access.md).</span></span> <span data-ttu-id="262d2-184">Adja meg az összes portbeállítások 9050 portot.</span><span class="sxs-lookup"><span data-stu-id="262d2-184">Specify port 9050 for all port settings.</span></span>


<span data-ttu-id="262d2-185">Ha minden létrejött, nyissa meg toohello **áttekintése** hello DC/OS-ügynök terheléselosztó panel (hello nevű erőforrás **vezénylőtípusú-ügynök-lb-xxxx**).</span><span class="sxs-lookup"><span data-stu-id="262d2-185">Once everything has been created, go toohello **Overview** blade of hello DC/OS agent load balancer (hello resource named **dcos-agent-lb-xxxx**).</span></span> <span data-ttu-id="262d2-186">Hello található **nyilvános IP-cím**, és hello cím tooaccess Száva port 9050 használatát.</span><span class="sxs-lookup"><span data-stu-id="262d2-186">Find hello **Public IP address**, and use hello address tooaccess sava at port 9050.</span></span>

![Azure portál – get nyilvános IP-cím](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![Száva](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a><span data-ttu-id="262d2-189">Kanári kiadási futtatása</span><span class="sxs-lookup"><span data-stu-id="262d2-189">Run a canary release</span></span>

<span data-ttu-id="262d2-190">Tegyük fel, az alkalmazás új verzióját, amelyet az éles környezetben toocanary kiadás.</span><span class="sxs-lookup"><span data-stu-id="262d2-190">Suppose you have a new version of this application that you want toocanary release into production.</span></span> <span data-ttu-id="262d2-191">Annak indexelése magneticio/sava:1.1.0, és készen áll a toogo.</span><span class="sxs-lookup"><span data-stu-id="262d2-191">You have it containerized as magneticio/sava:1.1.0 and are ready toogo.</span></span> <span data-ttu-id="262d2-192">Vamp lehetővé teszi a könnyen a telepítést futtató új szolgáltatások toohello hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="262d2-192">Vamp lets you easily add new services toohello running deployment.</span></span> <span data-ttu-id="262d2-193">A "egyesített" szolgáltatások hello meglévő szolgáltatások hello fürt együtt telepített, és egy 0 %-os értéket kapó.</span><span class="sxs-lookup"><span data-stu-id="262d2-193">These "merged" services are deployed alongside hello existing services in hello cluster, and assigned a weight of 0%.</span></span> <span data-ttu-id="262d2-194">Sincs forgalom újonnan egyesített irányított tooa szolgáltatás, amíg a hello forgalomeloszlás módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="262d2-194">No traffic is routed tooa newly merged service until you adjust hello traffic distribution.</span></span> <span data-ttu-id="262d2-195">hello Vamp felhasználói felületén súly csúszkája hello lehetővé teszi az hello terjesztési, lehetővé teszi a növekményes módosításának (Kanári kiadás) vagy egy azonnali visszavonás teljes ellenőrzését.</span><span class="sxs-lookup"><span data-stu-id="262d2-195">hello weight slider in hello Vamp UI gives you complete control over hello distribution, allowing for incremental adjustments (canary release) or an instant rollback.</span></span>

### <a name="merge-a-new-service-variant"></a><span data-ttu-id="262d2-196">Egy új szolgáltatás variant egyesítése</span><span class="sxs-lookup"><span data-stu-id="262d2-196">Merge a new service variant</span></span>

<span data-ttu-id="262d2-197">toomerge hello új Száva 1.1 szolgáltatás a telepítést futtató hello:</span><span class="sxs-lookup"><span data-stu-id="262d2-197">toomerge hello new sava 1.1 service with hello running deployment:</span></span>

1. <span data-ttu-id="262d2-198">Hello Vamp felhasználói felület, kattintson **tervrajzokat**.</span><span class="sxs-lookup"><span data-stu-id="262d2-198">In hello Vamp UI, click **Blueprints**.</span></span>

2. <span data-ttu-id="262d2-199">Kattintson a **Hozzáadás** és illessze be a következő hello tervezetének YAM: A tervezetének ismerteti egy új szolgáltatás variant (Száva: 1.1.0-ás) toodeploy hello meglévő fürt (sava_cluster) belül.</span><span class="sxs-lookup"><span data-stu-id="262d2-199">Click **Add** and paste in hello following blueprint YAML: This blueprint describes a new service variant (sava:1.1.0) toodeploy within hello existing cluster (sava_cluster).</span></span>

  ```YAML
  name: sava:1.1.0      # blueprint name
  clusters:
    sava_cluster:       # cluster tooupdate
      services:
        -
          breed:
            name: sava:1.1.0    # service variant name
            deployable: magneticio/sava:1.1.0    
            ports:
              webport: 8080/http # cluster endpoint tooupdate
  ```
  
3. <span data-ttu-id="262d2-200">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="262d2-200">Click **Save**.</span></span> <span data-ttu-id="262d2-201">hello tervezetének tárolja, és meg hello **tervrajzokat** lap.</span><span class="sxs-lookup"><span data-stu-id="262d2-201">hello blueprint is stored and listed on hello **Blueprints** page.</span></span>

4. <span data-ttu-id="262d2-202">Megnyitás hello művelet menü elemre, majd hello Száva: 1.1 tervezetének **egyesítése**.</span><span class="sxs-lookup"><span data-stu-id="262d2-202">Open hello action menu on hello sava:1.1 blueprint and click **Merge to**.</span></span>

  ![Felhasználói felület – tervrajzokat vamp](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. <span data-ttu-id="262d2-204">Jelölje be hello **Száva** üzembe helyezési és kattintson **egyesítése**.</span><span class="sxs-lookup"><span data-stu-id="262d2-204">Select hello **sava** deployment and click **Merge**.</span></span>

  ![Felhasználói felület vamp - tervezetének toodeployment egyesítése](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

<span data-ttu-id="262d2-206">Vamp telepíti hello új Száva: 1.1.0-ás szolgáltatás variant hello tervezetének mellett Száva: 1.0.0 hello a leírt **sava_cluster** a telepítést futtató hello.</span><span class="sxs-lookup"><span data-stu-id="262d2-206">Vamp deploys hello new sava:1.1.0 service variant described in hello blueprint alongside sava:1.0.0 in hello **sava_cluster** of hello running deployment.</span></span> 

![Felhasználói felület – frissített Száva telepítési vamp](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

<span data-ttu-id="262d2-208">Hello **Száva/sava_cluster/webport** átjáró (hello a fürt végpontja) is frissül, egy útvonal toohello hozzáadása újonnan telepített Száva: 1.1.0-ás.</span><span class="sxs-lookup"><span data-stu-id="262d2-208">hello **sava/sava_cluster/webport** gateway (hello cluster endpoint) is also updated, adding a route toohello newly deployed sava:1.1.0.</span></span> <span data-ttu-id="262d2-209">Ezen a ponton nem továbbítódik itt (hello **súly** too0 % van beállítva).</span><span class="sxs-lookup"><span data-stu-id="262d2-209">At this point, no traffic is routed here (hello **WEIGHT** is set too0%).</span></span>

![Felhasználói felület - fürt átjáró vamp](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a><span data-ttu-id="262d2-211">Kanári kiadás</span><span class="sxs-lookup"><span data-stu-id="262d2-211">Canary release</span></span>

<span data-ttu-id="262d2-212">A Száva verzióját is üzembe helyezett hello azonos fürt esetén állítsa be őket hello mozgatásával forgalom eloszlása hello **súly** csúszkát.</span><span class="sxs-lookup"><span data-stu-id="262d2-212">With both versions of sava deployed in hello same cluster, adjust hello distribution of traffic between them by moving hello **WEIGHT** slider.</span></span>

1. <span data-ttu-id="262d2-213">Kattintson a ![Vamp UI - szerkesztése](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) következő túl**súly**.</span><span class="sxs-lookup"><span data-stu-id="262d2-213">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) next too**WEIGHT**.</span></span>

2. <span data-ttu-id="262d2-214">Hello súly terjesztési too50%/50% beállítva, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="262d2-214">Set hello weight distribution too50%/50% and click **Save**.</span></span>

  ![Felhasználói felület – átjáró súly csúszkát vamp](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. <span data-ttu-id="262d2-216">Lépjen vissza tooyour böngészőt, és néhány alkalommal hello Száva lap frissítése.</span><span class="sxs-lookup"><span data-stu-id="262d2-216">Go back tooyour browser and refresh hello sava page a few more times.</span></span> <span data-ttu-id="262d2-217">Száva alkalmazás hello vált egy Száva: 1.0 lap és Száva: 1.1 oldal között.</span><span class="sxs-lookup"><span data-stu-id="262d2-217">hello sava application now switches between a sava:1.0 page and a sava:1.1 page.</span></span>

  ![váltakozó sava1.0 és sava1.1 szolgáltatások](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > <span data-ttu-id="262d2-219">A váltási hello lap működik a legjobban hello "Incognito" vagy "Névtelen" módban, a böngésző hello statikus rendezett gyorsítótárazása miatt.</span><span class="sxs-lookup"><span data-stu-id="262d2-219">This alternation of hello page works best with hello "Incognito" or “Anonymous” mode of your browser because of hello caching of static assets.</span></span>
  >

### <a name="filter-traffic"></a><span data-ttu-id="262d2-220">Szűrő forgalom</span><span class="sxs-lookup"><span data-stu-id="262d2-220">Filter traffic</span></span>

<span data-ttu-id="262d2-221">Tegyük fel, hogy a központi telepítést követően, hogy a inkompatibilitás Száva: 1.1.0-ás okok Firefox böngészőket problémákat jeleníti meg a felderíteni.</span><span class="sxs-lookup"><span data-stu-id="262d2-221">Suppose after deployment that you discovered an incompatibility in sava:1.1.0 that causes display problems in Firefox browsers.</span></span> <span data-ttu-id="262d2-222">Állítsa be a Vamp toofilter bejövő forgalmat, és közvetlen senki Firefox toohello stabil Száva: 1.0.0 ismert biztonsági.</span><span class="sxs-lookup"><span data-stu-id="262d2-222">You can set Vamp toofilter incoming traffic and direct all Firefox users back toohello known stable sava:1.0.0.</span></span> <span data-ttu-id="262d2-223">Ez a szűrő instantly oldja fel a rendszer hello megszűnésének a Firefox felhasználók számára, miközben mindenki más tovább tooenjoy hello előnyei hello továbbfejlesztett Száva: 1.1.0-ás.</span><span class="sxs-lookup"><span data-stu-id="262d2-223">This filter instantly resolves hello disruption for Firefox users, while everybody else continues tooenjoy hello benefits of hello improved sava:1.1.0.</span></span>

<span data-ttu-id="262d2-224">Felhasználási vamp **feltételek** toofilter forgalom útvonal az átjáró között.</span><span class="sxs-lookup"><span data-stu-id="262d2-224">Vamp uses **conditions** toofilter traffic between routes in a gateway.</span></span> <span data-ttu-id="262d2-225">Forgalom függően toohello alkalmazott feltételek tooeach útvonal vezérelt, és először vannak leszűrve.</span><span class="sxs-lookup"><span data-stu-id="262d2-225">Traffic is first filtered and directed according toohello conditions applied tooeach route.</span></span> <span data-ttu-id="262d2-226">Az összes többi forgalom toohello átjáró súlyozási beállítás szerint történik.</span><span class="sxs-lookup"><span data-stu-id="262d2-226">All remaining traffic is distributed according toohello gateway weight setting.</span></span>

<span data-ttu-id="262d2-227">Létrehozhat egy feltétel toofilter senki Firefox és toohello régi Száva: 1.0.0 közvetlen őket:</span><span class="sxs-lookup"><span data-stu-id="262d2-227">You can create a condition toofilter all Firefox users and direct them toohello old sava:1.0.0:</span></span>

1. <span data-ttu-id="262d2-228">A Száva/sava_cluster/webport hello **átjárók** kattintson ![Vamp UI - szerkesztése](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) tooadd egy **feltétel** toohello útvonal sava/sava_cluster/sava:1.0.0/webport.</span><span class="sxs-lookup"><span data-stu-id="262d2-228">On hello sava/sava_cluster/webport **Gateways** page, click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) tooadd a **CONDITION** toohello route sava/sava_cluster/sava:1.0.0/webport.</span></span> 

2. <span data-ttu-id="262d2-229">Adja meg a hello feltétel **felhasználói ügynök == Firefox** kattintson ![Vamp UI - mentése](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).</span><span class="sxs-lookup"><span data-stu-id="262d2-229">Enter hello condition **user-agent == Firefox** and click ![Vamp UI - save](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).</span></span>

  <span data-ttu-id="262d2-230">Vamp hello feltételt a következő egy alapértelmezett erőssége 0 % hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="262d2-230">Vamp adds hello condition with a default strength of 0%.</span></span> <span data-ttu-id="262d2-231">toostart forgalmának szűrésével, tooadjust hello feltétel erőssége kell.</span><span class="sxs-lookup"><span data-stu-id="262d2-231">toostart filtering traffic, you need tooadjust hello condition strength.</span></span>

3. <span data-ttu-id="262d2-232">Kattintson a ![Vamp UI - szerkesztése](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) toochange hello **erőssége** toohello feltétel alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="262d2-232">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) toochange hello **STRENGTH** applied toohello condition.</span></span>
 
4. <span data-ttu-id="262d2-233">Set hello **erőssége** too100 % kattintson ![Vamp UI - mentése](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) toosave.</span><span class="sxs-lookup"><span data-stu-id="262d2-233">Set hello **STRENGTH** too100% and click ![Vamp UI - save](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) toosave.</span></span>

  <span data-ttu-id="262d2-234">Vamp most az összes forgalom megfelelő hello feltétel (minden felhasználó Firefox) toosava:1.0.0 küldi el.</span><span class="sxs-lookup"><span data-stu-id="262d2-234">Vamp now sends all traffic matching hello condition (all Firefox users) toosava:1.0.0.</span></span>

  ![Felhasználói felület vamp - feltétel toogateway alkalmazása](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. <span data-ttu-id="262d2-236">Végül módosítsa hello átjáró súly toosend összes többi forgalom (minden felhasználó nem Firefox) toohello új Száva: 1.1.0-ás.</span><span class="sxs-lookup"><span data-stu-id="262d2-236">Finally, adjust hello gateway weight toosend all remaining traffic (all non-Firefox users) toohello new sava:1.1.0.</span></span> <span data-ttu-id="262d2-237">Kattintson a ![Vamp UI - szerkesztése](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) következő túl**súly** és állítsa be a hello súlyozások elosztása, így a 100 %-os irányított toohello útvonal sava/sava_cluster/sava:1.1.0/webport.</span><span class="sxs-lookup"><span data-stu-id="262d2-237">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) next too**WEIGHT** and set hello weight distribution so 100% is directed toohello route sava/sava_cluster/sava:1.1.0/webport.</span></span>

  <span data-ttu-id="262d2-238">Az összes forgalom nem szűrt hello feltétel már irányított toohello új Száva: 1.1.0-ás.</span><span class="sxs-lookup"><span data-stu-id="262d2-238">All traffic not filtered by hello condition is now directed toohello new sava:1.1.0.</span></span>

6. <span data-ttu-id="262d2-239">toosee hello szűrő műveletben, nyissa meg két különböző böngészők (egy Firefox és egy másik böngészőben), és elérni a hello Száva szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="262d2-239">toosee hello filter in action, open two different browsers (one Firefox and one other browser) and access hello sava service from both.</span></span> <span data-ttu-id="262d2-240">Összes Firefox kérelem küldése toosava:1.0.0, míg minden más böngészők irányított toosava:1.1.0.</span><span class="sxs-lookup"><span data-stu-id="262d2-240">All Firefox requests are sent toosava:1.0.0, while all other browsers are directed toosava:1.1.0.</span></span>

  ![Felhasználói felület - forgalom szűrésére vamp](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a><span data-ttu-id="262d2-242">Összegzése</span><span class="sxs-lookup"><span data-stu-id="262d2-242">Summing up</span></span>

<span data-ttu-id="262d2-243">Ez a cikk a DC/OS-fürt gyors bevezetés tooVamp volt.</span><span class="sxs-lookup"><span data-stu-id="262d2-243">This article was a quick introduction tooVamp on a DC/OS cluster.</span></span> <span data-ttu-id="262d2-244">Első Vamp portáltól és fut a az Azure tároló szolgáltatás DC/OS fürt, telepített egy szolgáltatást a egy Vamp tervezetének, címen érhető el: hello kitett végpont (átjáró).</span><span class="sxs-lookup"><span data-stu-id="262d2-244">For starters, you got Vamp up and running on your Azure Container Service DC/OS cluster, deployed a service with a Vamp blueprint, and accessed it at hello exposed endpoint (gateway).</span></span>

<span data-ttu-id="262d2-245">Azt is az egyes hatékony szolgáltatásainak Vamp érint: egyesítése egy új szolgáltatás variant toohello-telepítést futtató és a bevezeti a Növekményesen, majd a forgalom tooresolve egy ismert kompatibilitási szűrés.</span><span class="sxs-lookup"><span data-stu-id="262d2-245">We also touched on some powerful features of Vamp:  merging a new service variant toohello running deployment and introducing it incrementally, then filtering traffic tooresolve a known incompatibility.</span></span>


## <a name="next-steps"></a><span data-ttu-id="262d2-246">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="262d2-246">Next steps</span></span>

* <span data-ttu-id="262d2-247">Kezelésével kapcsolatos Vamp műveletek keresztül hello [Vamp a REST API-t](http://vamp.io/documentation/api/api-reference/).</span><span class="sxs-lookup"><span data-stu-id="262d2-247">Learn about managing Vamp actions through hello [Vamp REST API](http://vamp.io/documentation/api/api-reference/).</span></span>

* <span data-ttu-id="262d2-248">A node.js Vamp automatizálási parancsfájlokat létrehozása és futtatása azokat [munkafolyamatok Vamp](http://vamp.io/documentation/tutorials/create-a-workflow/).</span><span class="sxs-lookup"><span data-stu-id="262d2-248">Build Vamp automation scripts in Node.js and run them as [Vamp workflows](http://vamp.io/documentation/tutorials/create-a-workflow/).</span></span>

* <span data-ttu-id="262d2-249">További részletek [VAMP oktatóanyagok](http://vamp.io/documentation/tutorials/overview/).</span><span class="sxs-lookup"><span data-stu-id="262d2-249">See additional [VAMP tutorials](http://vamp.io/documentation/tutorials/overview/).</span></span>

