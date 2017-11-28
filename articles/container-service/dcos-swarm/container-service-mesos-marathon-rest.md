---
title: "A Marathon REST API-t Azure DC/OS-fürt kezeléséhez |} Microsoft Docs"
description: "Tárolók telepítése egy Azure tároló szolgáltatás DC/OS fürtben a Marathon REST API használatával."
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
ms.openlocfilehash: 65f8e0170fa7b89162e811a1d5dd58775fd20d7b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="dcos-container-management-through-the-marathon-rest-api"></a><span data-ttu-id="bda86-104">A Marathon REST API-t a DC/OS-tárolók kezelése</span><span class="sxs-lookup"><span data-stu-id="bda86-104">DC/OS container management through the Marathon REST API</span></span>
<span data-ttu-id="bda86-105">A DC/OS biztosítja a fürtözött feladatok telepítését és skálázását lehetővé tevő környezetet, ugyanakkor absztrakciós rétegként működik a hardver fölött.</span><span class="sxs-lookup"><span data-stu-id="bda86-105">DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting the underlying hardware.</span></span> <span data-ttu-id="bda86-106">A DC/OS fölötti keretrendszer gondoskodik a számítási feladatok ütemezéséről és végrehajtásáról.</span><span class="sxs-lookup"><span data-stu-id="bda86-106">On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.</span></span> <span data-ttu-id="bda86-107">Bár számos népszerű számítási elérhetők keretrendszerek, ez a dokumentum beolvasása elkezdésének létrehozásán és skálázásán üzemelő tárolópéldányokat a Marathon REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="bda86-107">Although frameworks are available for many popular workloads, this document gets you started creating and scaling container deployments by using the Marathon REST API.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="bda86-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bda86-108">Prerequisites</span></span>

<span data-ttu-id="bda86-109">A példákban szereplő feladatok elvégzéséhez szüksége lesz egy az Azure tárolószolgáltatásban konfigurált DC/OS-fürtre,</span><span class="sxs-lookup"><span data-stu-id="bda86-109">Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service.</span></span> <span data-ttu-id="bda86-110">valamint távoli kapcsolatot kell tudnia létesíteni a fürttel.</span><span class="sxs-lookup"><span data-stu-id="bda86-110">You also need to have remote connectivity to this cluster.</span></span> <span data-ttu-id="bda86-111">Ezekkel az elemekkel kapcsolatban a következő cikkekben talál további tájékoztatást:</span><span class="sxs-lookup"><span data-stu-id="bda86-111">For more information on these items, see the following articles:</span></span>

* [<span data-ttu-id="bda86-112">Azure Container Service-fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="bda86-112">Deploying an Azure Container Service cluster</span></span>](container-service-deployment.md)
* [<span data-ttu-id="bda86-113">Csatlakozás Azure Container Service-fürthöz</span><span class="sxs-lookup"><span data-stu-id="bda86-113">Connecting to an Azure Container Service cluster</span></span>](../container-service-connect.md)

## <a name="access-the-dcos-apis"></a><span data-ttu-id="bda86-114">Hozzáférés a DC/OS API-k</span><span class="sxs-lookup"><span data-stu-id="bda86-114">Access the DC/OS APIs</span></span>
<span data-ttu-id="bda86-115">Miután csatlakozott az Azure tárolószolgáltatás-fürthöz, a DC/OS-t és a megfelelő REST API-kat a http://localhost:local-port címen érheti el.</span><span class="sxs-lookup"><span data-stu-id="bda86-115">After you are connected to the Azure Container Service cluster, you can access the DC/OS and related REST APIs through http://localhost:local-port.</span></span> <span data-ttu-id="bda86-116">Az ebben a dokumentumban szereplő példák azt feltételezik, hogy az alagutat a 80-as porton keresztül hozta létre.</span><span class="sxs-lookup"><span data-stu-id="bda86-116">The examples in this document assume that you are tunneling on port 80.</span></span> <span data-ttu-id="bda86-117">Például a Marathon végpontok címen érhető el URI-azonosítók kezdve `http://localhost/marathon/v2/`.</span><span class="sxs-lookup"><span data-stu-id="bda86-117">For example, the Marathon endpoints can be reached at URIs beginning with `http://localhost/marathon/v2/`.</span></span> 

<span data-ttu-id="bda86-118">A [Marathon API-ról](https://mesosphere.github.io/marathon/docs/rest-api.html) és a [Chronos API-ról](https://mesos.github.io/chronos/docs/api.html) a Mesosphere dokumentációjában, a [Mesos Scheduler API-ról](http://mesos.apache.org/documentation/latest/scheduler-http-api/) pedig az Apache dokumentációjában talál további információt.</span><span class="sxs-lookup"><span data-stu-id="bda86-118">For more information on the various APIs, see the Mesosphere documentation for the [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) and the [Chronos API](https://mesos.github.io/chronos/docs/api.html), and the Apache documentation for the [Mesos Scheduler API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).</span></span>

## <a name="gather-information-from-dcos-and-marathon"></a><span data-ttu-id="bda86-119">Információgyűjtés a DC/OS-ről és a Marathonról</span><span class="sxs-lookup"><span data-stu-id="bda86-119">Gather information from DC/OS and Marathon</span></span>
<span data-ttu-id="bda86-120">Mielőtt tárolókat a DC/OS-fürtről telepít, a DC/OS-fürtről, például nevét és a DC/OS-ügynökök állapotának néhány információt gyűjteni.</span><span class="sxs-lookup"><span data-stu-id="bda86-120">Before you deploy containers to the DC/OS cluster, gather some information about the DC/OS cluster, such as the names and status of the DC/OS agents.</span></span> <span data-ttu-id="bda86-121">Ehhez kérdezze le a DC/OS REST API fő- és alárendelt kiszolgálóinak (`master/slaves`) végpontját.</span><span class="sxs-lookup"><span data-stu-id="bda86-121">To do so, query the `master/slaves` endpoint of the DC/OS REST API.</span></span> <span data-ttu-id="bda86-122">Ha minden megfelelően működik, a lekérdezés a DC/OS-ügynökök listáját és az ügynökök különböző tulajdonságait adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bda86-122">If everything goes well, the query returns a list of DC/OS agents and several properties for each.</span></span>

```bash
curl http://localhost/mesos/master/slaves
```

<span data-ttu-id="bda86-123">A DC/OS-fürtben üzembe helyezett alkalmazások lekérdezéséhez használja a Marathon `/apps` végpontot.</span><span class="sxs-lookup"><span data-stu-id="bda86-123">Now, use the Marathon `/apps` endpoint to check for current application deployments to the DC/OS cluster.</span></span> <span data-ttu-id="bda86-124">Ha ez egy új fürt, akkor az alkalmazásoknál egy üres tömb jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bda86-124">If this is a new cluster, you see an empty array for apps.</span></span>

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a><span data-ttu-id="bda86-125">Docker-formázású tároló üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="bda86-125">Deploy a Docker-formatted container</span></span>
<span data-ttu-id="bda86-126">A JSON-fájl, amely leírja a kívánt üzembe helyezéssel segítségével telepítheti Docker-formátumú tárolók Marathon REST API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="bda86-126">You deploy Docker-formatted containers through the Marathon REST API by using a JSON file that describes the intended deployment.</span></span> <span data-ttu-id="bda86-127">Az alábbi minta egy titkos ügynököt a fürt egy Nginx tároló telepíti.</span><span class="sxs-lookup"><span data-stu-id="bda86-127">The following sample deploys an Nginx container to a private agent in the cluster.</span></span> 

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

<span data-ttu-id="bda86-128">Egy Docker-formátumú tároló üzembe helyezéséhez tárolja elérhető helyen a JSON-fájlt.</span><span class="sxs-lookup"><span data-stu-id="bda86-128">To deploy a Docker-formatted container, store the JSON file in an accessible location.</span></span> <span data-ttu-id="bda86-129">Ezt követően a tároló üzembe helyezéséhez futtassa az alábbi parancsot.</span><span class="sxs-lookup"><span data-stu-id="bda86-129">Next, to deploy the container, run the following command.</span></span> <span data-ttu-id="bda86-130">Adja meg a JSON-fájl nevét (`marathon.json` ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="bda86-130">Specify the name of the JSON file (`marathon.json` in this example).</span></span>

```bash
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

<span data-ttu-id="bda86-131">A kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="bda86-131">The output is similar to the following:</span></span>

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

<span data-ttu-id="bda86-132">Ha ezt követően lekérdezi az alkalmazásokat a Marathonban, az eredmények között megjelenik az új alkalmazás is.</span><span class="sxs-lookup"><span data-stu-id="bda86-132">Now, if you query Marathon for applications, this new application appears in the output.</span></span>

```bash
curl localhost/marathon/v2/apps
```

## <a name="reach-the-container"></a><span data-ttu-id="bda86-133">A tároló elérése</span><span class="sxs-lookup"><span data-stu-id="bda86-133">Reach the container</span></span>

<span data-ttu-id="bda86-134">Ellenőrizheti, hogy a Nginx tároló fut. a titkos ügynököket a fürt egyik.</span><span class="sxs-lookup"><span data-stu-id="bda86-134">You can verify that the Nginx is running in a container on one of the private agents in the cluster.</span></span> <span data-ttu-id="bda86-135">A gazdagép és a port, amelyen fut a tárolóban található, marathonban a futó feladatok:</span><span class="sxs-lookup"><span data-stu-id="bda86-135">To find the host and port where the container is running, query Marathon for the running tasks:</span></span> 

```bash
curl localhost/marathon/v2/tasks
```

<span data-ttu-id="bda86-136">Keresse meg az értéket a `host` kimenet (hasonló IP-cím `10.32.0.x`), és az értéke `ports`.</span><span class="sxs-lookup"><span data-stu-id="bda86-136">Find the value of `host` in the output (an IP address similar to `10.32.0.x`), and the value of `ports`.</span></span>


<span data-ttu-id="bda86-137">Egy terminál SSH-kapcsolat (nem bújtatott kapcsolat) Ellenőrizze a fürt FQDN-felügyelet.</span><span class="sxs-lookup"><span data-stu-id="bda86-137">Now make an SSH terminal connection (not a tunneled connection) to the management FQDN of the cluster.</span></span> <span data-ttu-id="bda86-138">A csatlakozás után ellenőrizze a következő kérelmet, és a megfelelő értékeket `host` és `ports`:</span><span class="sxs-lookup"><span data-stu-id="bda86-138">Once connected, make the following request, substituting the correct values of `host` and `ports`:</span></span>

```bash
curl http://host:ports
```

<span data-ttu-id="bda86-139">A Nginx server kimenete az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="bda86-139">The Nginx server output is similar to the following:</span></span>

![Nginx-tárolójából.](./media/container-service-mesos-marathon-rest/nginx.png)




## <a name="scale-your-containers"></a><span data-ttu-id="bda86-141">A tárolók skálázása</span><span class="sxs-lookup"><span data-stu-id="bda86-141">Scale your containers</span></span>
<span data-ttu-id="bda86-142">A Marathon API segítségével horizontális felskálázás vagy méretezni az alkalmazások központi telepítéseit.</span><span class="sxs-lookup"><span data-stu-id="bda86-142">You can use the Marathon API to scale out or scale in application deployments.</span></span> <span data-ttu-id="bda86-143">Az előző példában üzembe helyezett egy alkalmazáspéldányt.</span><span class="sxs-lookup"><span data-stu-id="bda86-143">In the previous example, you deployed one instance of an application.</span></span> <span data-ttu-id="bda86-144">Ezt most skálázhatja három alkalmazáspéldányra.</span><span class="sxs-lookup"><span data-stu-id="bda86-144">Let's scale this out to three instances of an application.</span></span> <span data-ttu-id="bda86-145">Ehhez hozzon létre egy JSON-fájlt az alábbi JSON-szöveg használatával, és tárolja elérhető helyen.</span><span class="sxs-lookup"><span data-stu-id="bda86-145">To do so, create a JSON file by using the following JSON text, and store it in an accessible location.</span></span>

```json
{ "instances": 3 }
```

<span data-ttu-id="bda86-146">A bújtatott kapcsolat létrehozásakor futtassa a következő parancsot az alkalmazás horizontális.</span><span class="sxs-lookup"><span data-stu-id="bda86-146">From your tunneled connection, run the following command to scale out the application.</span></span>

> [!NOTE]
> <span data-ttu-id="bda86-147">Az URI a http://localhost/marathon/v2/apps/ cím, amelyet a skálázandó alkalmazás azonosítója követ.</span><span class="sxs-lookup"><span data-stu-id="bda86-147">The URI is http://localhost/marathon/v2/apps/ followed by the ID of the application to scale.</span></span> <span data-ttu-id="bda86-148">Ha az itt szerepelő Nginx mintát használja, akkor az URI a http://localhost/marathon/v2/apps/nginx cím lesz.</span><span class="sxs-lookup"><span data-stu-id="bda86-148">If you are using the Nginx sample that is provided here, the URI would be http://localhost/marathon/v2/apps/nginx.</span></span>
> 
> 

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

<span data-ttu-id="bda86-149">Végül kérdezze le az alkalmazásokat a Marathon végponton.</span><span class="sxs-lookup"><span data-stu-id="bda86-149">Finally, query the Marathon endpoint for applications.</span></span> <span data-ttu-id="bda86-150">Láthatja majd, hogy most már három Nginx-tároló létezik.</span><span class="sxs-lookup"><span data-stu-id="bda86-150">You see that there are now three Nginx containers.</span></span>

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a><span data-ttu-id="bda86-151">Egyenértékű PowerShell-parancsok</span><span class="sxs-lookup"><span data-stu-id="bda86-151">Equivalent PowerShell commands</span></span>
<span data-ttu-id="bda86-152">Ugyanezeket a műveleteket elvégezheti Windows rendszerben is a PowerShell-parancsok használatával.</span><span class="sxs-lookup"><span data-stu-id="bda86-152">You can perform these same actions by using PowerShell commands on a Windows system.</span></span>

<span data-ttu-id="bda86-153">Gyűjtsön információt a DC/OS fürtben, mint az ügynökök nevét és állapotát, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="bda86-153">To gather information about the DC/OS cluster, such as agent names and agent status, run the following command:</span></span>

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

<span data-ttu-id="bda86-154">A Docker-formátumú tárolók Marathon segítségével való üzembe helyezéséhez egy olyan JSON-fájlt kell használnia, amelyben megadhatja a kívánt üzembe helyezéssel kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="bda86-154">You deploy Docker-formatted containers through Marathon by using a JSON file that describes the intended deployment.</span></span> <span data-ttu-id="bda86-155">Az alábbi példában, amely egy Nginx-tároló üzembe helyezését szemlélteti, a DC/OS-ügynök 80-as portja a tároló 80-as portjával van összekötve.</span><span class="sxs-lookup"><span data-stu-id="bda86-155">The following sample deploys the Nginx container, binding port 80 of the DC/OS agent to port 80 of the container.</span></span>

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

<span data-ttu-id="bda86-156">Egy Docker-formátumú tároló üzembe helyezéséhez tárolja elérhető helyen a JSON-fájlt.</span><span class="sxs-lookup"><span data-stu-id="bda86-156">To deploy a Docker-formatted container, store the JSON file in an accessible location.</span></span> <span data-ttu-id="bda86-157">Ezt követően a tároló üzembe helyezéséhez futtassa az alábbi parancsot.</span><span class="sxs-lookup"><span data-stu-id="bda86-157">Next, to deploy the container, run the following command.</span></span> <span data-ttu-id="bda86-158">Adja meg a JSON-fájl elérési útját (`marathon.json` ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="bda86-158">Specify the path to the JSON file (`marathon.json` in this example).</span></span>

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

<span data-ttu-id="bda86-159">A Marathon API-t az üzemelő alkalmazáspéldányok horizontális skálázására is használhatja.</span><span class="sxs-lookup"><span data-stu-id="bda86-159">You can also use the Marathon API to scale out or scale in application deployments.</span></span> <span data-ttu-id="bda86-160">Az előző példában üzembe helyezett egy alkalmazáspéldányt.</span><span class="sxs-lookup"><span data-stu-id="bda86-160">In the previous example, you deployed one instance of an application.</span></span> <span data-ttu-id="bda86-161">Ezt most skálázhatja három alkalmazáspéldányra.</span><span class="sxs-lookup"><span data-stu-id="bda86-161">Let's scale this out to three instances of an application.</span></span> <span data-ttu-id="bda86-162">Ehhez hozzon létre egy JSON-fájlt az alábbi JSON-szöveg használatával, és tárolja elérhető helyen.</span><span class="sxs-lookup"><span data-stu-id="bda86-162">To do so, create a JSON file by using the following JSON text, and store it in an accessible location.</span></span>

```json
{ "instances": 3 }
```

<span data-ttu-id="bda86-163">A következő parancsot az alkalmazás horizontális:</span><span class="sxs-lookup"><span data-stu-id="bda86-163">Run the following command to scale out the application:</span></span>

> [!NOTE]
> <span data-ttu-id="bda86-164">Az URI a http://localhost/marathon/v2/apps/ cím, amelyet a skálázandó alkalmazás azonosítója követ.</span><span class="sxs-lookup"><span data-stu-id="bda86-164">The URI is http://localhost/marathon/v2/apps/ followed by the ID of the application to scale.</span></span> <span data-ttu-id="bda86-165">Ha az Nginx-mintát használja, akkor az URI a http://localhost/marathon/v2/apps/nginx cím lesz.</span><span class="sxs-lookup"><span data-stu-id="bda86-165">If you are using the Nginx sample provided here, the URI would be http://localhost/marathon/v2/apps/nginx.</span></span>
> 
> 

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a><span data-ttu-id="bda86-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bda86-166">Next steps</span></span>
* [<span data-ttu-id="bda86-167">További tudnivalók a Mesos HTTP-végpontokról</span><span class="sxs-lookup"><span data-stu-id="bda86-167">Read more about the Mesos HTTP endpoints</span></span>](http://mesos.apache.org/documentation/latest/endpoints/)
* [<span data-ttu-id="bda86-168">További tudnivalók a Marathon REST API</span><span class="sxs-lookup"><span data-stu-id="bda86-168">Read more about the Marathon REST API</span></span>](https://mesosphere.github.io/marathon/docs/rest-api.html)

