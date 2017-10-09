---
title: "aaaManage Azure DC/OS fürtben a Marathon REST API-hoz |} Microsoft Docs"
description: "Tárolók tooan Azure tároló szolgáltatás DC/OS-fürt üzembe hello Marathon REST API használatával."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Mesos, Azure"
ms.assetid: c7175446-4507-4a33-a7a2-63583e5996e3
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: d926b9b90f5d4eda85a015d9ea0d96fea2c4b566
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dcos-container-management-through-hello-marathon-rest-api"></a><span data-ttu-id="5d49d-104">A DC/OS hello Marathon REST API-tárolók kezelése</span><span class="sxs-lookup"><span data-stu-id="5d49d-104">DC/OS container management through hello Marathon REST API</span></span>
<span data-ttu-id="5d49d-105">A DC/OS telepítését és skálázását lehetővé fürtözött munkaterhelések, absztrakt módon megjelenítve a mögöttes hardver hello közben környezetet biztosít.</span><span class="sxs-lookup"><span data-stu-id="5d49d-105">DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting hello underlying hardware.</span></span> <span data-ttu-id="5d49d-106">A DC/OS fölötti keretrendszer gondoskodik a számítási feladatok ütemezéséről és végrehajtásáról.</span><span class="sxs-lookup"><span data-stu-id="5d49d-106">On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.</span></span> <span data-ttu-id="5d49d-107">Bár számos népszerű számítási elérhetők keretrendszerek, ez a dokumentum beolvasása létrehozásán és skálázásán üzemelő tárolópéldányokat a Marathon REST API hello lépésekhez.</span><span class="sxs-lookup"><span data-stu-id="5d49d-107">Although frameworks are available for many popular workloads, this document gets you started creating and scaling container deployments by using hello Marathon REST API.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="5d49d-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5d49d-108">Prerequisites</span></span>

<span data-ttu-id="5d49d-109">A példákban szereplő feladatok elvégzéséhez szüksége lesz egy az Azure tárolószolgáltatásban konfigurált DC/OS-fürtre,</span><span class="sxs-lookup"><span data-stu-id="5d49d-109">Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service.</span></span> <span data-ttu-id="5d49d-110">Meg kell toohave távoli kapcsolatot toothis fürtöt is.</span><span class="sxs-lookup"><span data-stu-id="5d49d-110">You also need toohave remote connectivity toothis cluster.</span></span> <span data-ttu-id="5d49d-111">További információ ezekről az elemekről tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="5d49d-111">For more information on these items, see hello following articles:</span></span>

* [<span data-ttu-id="5d49d-112">Azure Container Service-fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="5d49d-112">Deploying an Azure Container Service cluster</span></span>](container-service-deployment.md)
* [<span data-ttu-id="5d49d-113">Csatlakozás Azure Tárolószolgáltatás-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="5d49d-113">Connecting tooan Azure Container Service cluster</span></span>](../container-service-connect.md)

## <a name="access-hello-dcos-apis"></a><span data-ttu-id="5d49d-114">Hozzáférés hello DC/OS API-k</span><span class="sxs-lookup"><span data-stu-id="5d49d-114">Access hello DC/OS APIs</span></span>
<span data-ttu-id="5d49d-115">Után csatlakoztatott toohello Azure Tárolószolgáltatási fürthöz, a DC/OS hello és a kapcsolódó REST API-k elérheti http://localhost-porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="5d49d-115">After you are connected toohello Azure Container Service cluster, you can access hello DC/OS and related REST APIs through http://localhost:local-port.</span></span> <span data-ttu-id="5d49d-116">Ebben a dokumentumban a hello példák feltételezik, hogy, hogy az alagutat a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="5d49d-116">hello examples in this document assume that you are tunneling on port 80.</span></span> <span data-ttu-id="5d49d-117">Például hello Marathon végpontok címen érhető el URI-azonosítók kezdve `http://localhost/marathon/v2/`.</span><span class="sxs-lookup"><span data-stu-id="5d49d-117">For example, hello Marathon endpoints can be reached at URIs beginning with `http://localhost/marathon/v2/`.</span></span> 

<span data-ttu-id="5d49d-118">További információ a hello különböző API-k: hello Mesosphere dokumentációjában hello [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) és a [Chronos API](https://mesos.github.io/chronos/docs/api.html), és az Apache dokumentációjában hello [Mesos Scheduler API-RÓL ](http://mesos.apache.org/documentation/latest/scheduler-http-api/).</span><span class="sxs-lookup"><span data-stu-id="5d49d-118">For more information on hello various APIs, see hello Mesosphere documentation for hello [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) and the [Chronos API](https://mesos.github.io/chronos/docs/api.html), and the Apache documentation for hello [Mesos Scheduler API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).</span></span>

## <a name="gather-information-from-dcos-and-marathon"></a><span data-ttu-id="5d49d-119">Információgyűjtés a DC/OS-ről és a Marathonról</span><span class="sxs-lookup"><span data-stu-id="5d49d-119">Gather information from DC/OS and Marathon</span></span>
<span data-ttu-id="5d49d-120">Tárolók toohello DC/OS-fürt központi telepítése érdekében hello DC/OS fürtben, például hello neveket és hello DC/OS-ügynökök állapotának néhány információt gyűjteni.</span><span class="sxs-lookup"><span data-stu-id="5d49d-120">Before you deploy containers toohello DC/OS cluster, gather some information about hello DC/OS cluster, such as hello names and status of hello DC/OS agents.</span></span> <span data-ttu-id="5d49d-121">Igen, a lekérdezési hello toodo `master/slaves` hello DC/OS REST API végpontja.</span><span class="sxs-lookup"><span data-stu-id="5d49d-121">toodo so, query hello `master/slaves` endpoint of hello DC/OS REST API.</span></span> <span data-ttu-id="5d49d-122">Ha minden megfelelően működik, az hello lekérdezés az egyes DC/OS-ügynökök és több tulajdonságok listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="5d49d-122">If everything goes well, hello query returns a list of DC/OS agents and several properties for each.</span></span>

```bash
curl http://localhost/mesos/master/slaves
```

<span data-ttu-id="5d49d-123">Most, használja a hello Marathon `/apps` végpont toocheck az aktuális alkalmazás központi telepítések toohello DC/OS-fürtről.</span><span class="sxs-lookup"><span data-stu-id="5d49d-123">Now, use hello Marathon `/apps` endpoint toocheck for current application deployments toohello DC/OS cluster.</span></span> <span data-ttu-id="5d49d-124">Ha ez egy új fürt, akkor az alkalmazásoknál egy üres tömb jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5d49d-124">If this is a new cluster, you see an empty array for apps.</span></span>

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a><span data-ttu-id="5d49d-125">Docker-formázású tároló üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="5d49d-125">Deploy a Docker-formatted container</span></span>
<span data-ttu-id="5d49d-126">Docker-formátumú tárolók Marathon REST API hello telepítése hello szánt központi telepítését ismertető JSON-fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="5d49d-126">You deploy Docker-formatted containers through hello Marathon REST API by using a JSON file that describes hello intended deployment.</span></span> <span data-ttu-id="5d49d-127">hello következő minta Nginx tároló tooa titkos ügynököt telepít hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="5d49d-127">hello following sample deploys an Nginx container tooa private agent in hello cluster.</span></span> 

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

<span data-ttu-id="5d49d-128">egy Docker-formázású tároló toodeploy hello JSON fájlt tárolja elérhető helyen.</span><span class="sxs-lookup"><span data-stu-id="5d49d-128">toodeploy a Docker-formatted container, store hello JSON file in an accessible location.</span></span> <span data-ttu-id="5d49d-129">Ezt követően toodeploy hello tároló, futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="5d49d-129">Next, toodeploy hello container, run hello following command.</span></span> <span data-ttu-id="5d49d-130">Adja meg hello hello JSON-fájl nevét (`marathon.json` ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="5d49d-130">Specify hello name of hello JSON file (`marathon.json` in this example).</span></span>

```bash
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

<span data-ttu-id="5d49d-131">hello hasonló toohello következő kimenete:</span><span class="sxs-lookup"><span data-stu-id="5d49d-131">hello output is similar toohello following:</span></span>

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

<span data-ttu-id="5d49d-132">Ha az alkalmazásokat a marathonban, az új alkalmazás megjelenik hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="5d49d-132">Now, if you query Marathon for applications, this new application appears in hello output.</span></span>

```bash
curl localhost/marathon/v2/apps
```

## <a name="reach-hello-container"></a><span data-ttu-id="5d49d-133">Hello tároló elérése</span><span class="sxs-lookup"><span data-stu-id="5d49d-133">Reach hello container</span></span>

<span data-ttu-id="5d49d-134">Ellenőrizheti, hogy Nginx fut a tárolóban lévő titkos ügynökök hello hello fürt egyik hello.</span><span class="sxs-lookup"><span data-stu-id="5d49d-134">You can verify that hello Nginx is running in a container on one of hello private agents in hello cluster.</span></span> <span data-ttu-id="5d49d-135">toofind hello állomás és port hello tárolót futtató marathonban az éppen futó feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="5d49d-135">toofind hello host and port where hello container is running, query Marathon for hello running tasks:</span></span> 

```bash
curl localhost/marathon/v2/tasks
```

<span data-ttu-id="5d49d-136">Keresse meg a hello értéket `host` hello kimenet (egy IP-hasonló túl`10.32.0.x`), és hello értékének `ports`.</span><span class="sxs-lookup"><span data-stu-id="5d49d-136">Find hello value of `host` in hello output (an IP address similar too`10.32.0.x`), and hello value of `ports`.</span></span>


<span data-ttu-id="5d49d-137">Egy SSH terminál kapcsolat (ez nem egy bújtatott kapcsolat) toohello felügyeleti FQDN hello fürt ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="5d49d-137">Now make an SSH terminal connection (not a tunneled connection) toohello management FQDN of hello cluster.</span></span> <span data-ttu-id="5d49d-138">A csatlakozás után ellenőrizze a következő kérelmet, és a megfelelő értékeket hello hello `host` és `ports`:</span><span class="sxs-lookup"><span data-stu-id="5d49d-138">Once connected, make hello following request, substituting hello correct values of `host` and `ports`:</span></span>

```bash
curl http://host:ports
```

<span data-ttu-id="5d49d-139">hello Nginx server kimenet hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="5d49d-139">hello Nginx server output is similar toohello following:</span></span>

![Nginx-tárolójából.](./media/container-service-mesos-marathon-rest/nginx.png)




## <a name="scale-your-containers"></a><span data-ttu-id="5d49d-141">A tárolók skálázása</span><span class="sxs-lookup"><span data-stu-id="5d49d-141">Scale your containers</span></span>
<span data-ttu-id="5d49d-142">Alkalmazások központi telepítésének hello Marathon API tooscale kimenő vagy skálája használhatja.</span><span class="sxs-lookup"><span data-stu-id="5d49d-142">You can use hello Marathon API tooscale out or scale in application deployments.</span></span> <span data-ttu-id="5d49d-143">Hello előző példában üzembe helyezett egy alkalmazáspéldányt az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="5d49d-143">In hello previous example, you deployed one instance of an application.</span></span> <span data-ttu-id="5d49d-144">Skálázzunk ez toothree alkalmazáspéldányra ki.</span><span class="sxs-lookup"><span data-stu-id="5d49d-144">Let's scale this out toothree instances of an application.</span></span> <span data-ttu-id="5d49d-145">toodo Igen, egy JSON-fájl létrehozása a következő JSON-szöveg hello használatával, és tárolja elérhető helyen.</span><span class="sxs-lookup"><span data-stu-id="5d49d-145">toodo so, create a JSON file by using hello following JSON text, and store it in an accessible location.</span></span>

```json
{ "instances": 3 }
```

<span data-ttu-id="5d49d-146">A bújtatott kapcsolat létrehozásakor futtassa a következő parancs tooscale hello alkalmazás kimenő hello.</span><span class="sxs-lookup"><span data-stu-id="5d49d-146">From your tunneled connection, run hello following command tooscale out hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="5d49d-147">hello URI a http://localhost/marathon/v2/apps/ hello alkalmazás tooscale hello azonosítója követ.</span><span class="sxs-lookup"><span data-stu-id="5d49d-147">hello URI is http://localhost/marathon/v2/apps/ followed by hello ID of hello application tooscale.</span></span> <span data-ttu-id="5d49d-148">Ha itt használ hello Nginx mintát, hello URI http://localhost/marathon/v2/apps/nginx lesz.</span><span class="sxs-lookup"><span data-stu-id="5d49d-148">If you are using hello Nginx sample that is provided here, hello URI would be http://localhost/marathon/v2/apps/nginx.</span></span>
> 
> 

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

<span data-ttu-id="5d49d-149">Végezetül lekérdezési hello Marathon-végpont alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="5d49d-149">Finally, query hello Marathon endpoint for applications.</span></span> <span data-ttu-id="5d49d-150">Láthatja majd, hogy most már három Nginx-tároló létezik.</span><span class="sxs-lookup"><span data-stu-id="5d49d-150">You see that there are now three Nginx containers.</span></span>

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a><span data-ttu-id="5d49d-151">Egyenértékű PowerShell-parancsok</span><span class="sxs-lookup"><span data-stu-id="5d49d-151">Equivalent PowerShell commands</span></span>
<span data-ttu-id="5d49d-152">Ugyanezeket a műveleteket elvégezheti Windows rendszerben is a PowerShell-parancsok használatával.</span><span class="sxs-lookup"><span data-stu-id="5d49d-152">You can perform these same actions by using PowerShell commands on a Windows system.</span></span>

<span data-ttu-id="5d49d-153">toogather információ hello DC/OS fürtben, mint az ügynökök nevét és állapotát, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="5d49d-153">toogather information about hello DC/OS cluster, such as agent names and agent status, run hello following command:</span></span>

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

<span data-ttu-id="5d49d-154">Docker-formátumú tárolók Marathon telepítése hello szánt központi telepítését ismertető JSON-fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="5d49d-154">You deploy Docker-formatted containers through Marathon by using a JSON file that describes hello intended deployment.</span></span> <span data-ttu-id="5d49d-155">hello következő minta telepíti a kötelező hello DC/OS ügynök tooport 80 hello tároló 80-as portja hello Nginx-tároló.</span><span class="sxs-lookup"><span data-stu-id="5d49d-155">hello following sample deploys hello Nginx container, binding port 80 of hello DC/OS agent tooport 80 of hello container.</span></span>

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

<span data-ttu-id="5d49d-156">egy Docker-formázású tároló toodeploy hello JSON fájlt tárolja elérhető helyen.</span><span class="sxs-lookup"><span data-stu-id="5d49d-156">toodeploy a Docker-formatted container, store hello JSON file in an accessible location.</span></span> <span data-ttu-id="5d49d-157">Ezt követően toodeploy hello tároló, futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="5d49d-157">Next, toodeploy hello container, run hello following command.</span></span> <span data-ttu-id="5d49d-158">Adja meg a hello elérési toohello JSON-fájl (`marathon.json` ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="5d49d-158">Specify hello path toohello JSON file (`marathon.json` in this example).</span></span>

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

<span data-ttu-id="5d49d-159">Az alkalmazások központi telepítéseit is hello Marathon API tooscale kimenő vagy skálája használható.</span><span class="sxs-lookup"><span data-stu-id="5d49d-159">You can also use hello Marathon API tooscale out or scale in application deployments.</span></span> <span data-ttu-id="5d49d-160">Hello előző példában üzembe helyezett egy alkalmazáspéldányt az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="5d49d-160">In hello previous example, you deployed one instance of an application.</span></span> <span data-ttu-id="5d49d-161">Skálázzunk ez toothree alkalmazáspéldányra ki.</span><span class="sxs-lookup"><span data-stu-id="5d49d-161">Let's scale this out toothree instances of an application.</span></span> <span data-ttu-id="5d49d-162">toodo Igen, egy JSON-fájl létrehozása a következő JSON-szöveg hello használatával, és tárolja elérhető helyen.</span><span class="sxs-lookup"><span data-stu-id="5d49d-162">toodo so, create a JSON file by using hello following JSON text, and store it in an accessible location.</span></span>

```json
{ "instances": 3 }
```

<span data-ttu-id="5d49d-163">Futtassa a következő parancs tooscale hello alkalmazás kimenő hello:</span><span class="sxs-lookup"><span data-stu-id="5d49d-163">Run hello following command tooscale out hello application:</span></span>

> [!NOTE]
> <span data-ttu-id="5d49d-164">hello URI a http://localhost/marathon/v2/apps/ hello alkalmazás tooscale hello azonosítója követ.</span><span class="sxs-lookup"><span data-stu-id="5d49d-164">hello URI is http://localhost/marathon/v2/apps/ followed by hello ID of hello application tooscale.</span></span> <span data-ttu-id="5d49d-165">Ha itt használ hello Nginx-mintát, hello URI http://localhost/marathon/v2/apps/nginx lesz.</span><span class="sxs-lookup"><span data-stu-id="5d49d-165">If you are using hello Nginx sample provided here, hello URI would be http://localhost/marathon/v2/apps/nginx.</span></span>
> 
> 

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a><span data-ttu-id="5d49d-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5d49d-166">Next steps</span></span>
* [<span data-ttu-id="5d49d-167">Tudjon meg többet a Mesos HTTP-végpontokról hello</span><span class="sxs-lookup"><span data-stu-id="5d49d-167">Read more about hello Mesos HTTP endpoints</span></span>](http://mesos.apache.org/documentation/latest/endpoints/)
* [<span data-ttu-id="5d49d-168">Tudjon meg többet a Marathon REST API hello</span><span class="sxs-lookup"><span data-stu-id="5d49d-168">Read more about hello Marathon REST API</span></span>](https://mesosphere.github.io/marathon/docs/rest-api.html)

