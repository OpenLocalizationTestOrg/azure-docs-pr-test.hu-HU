---
title: "aaaAzure Tárolószolgáltatás oktatóanyag - figyelő Kubernetes |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató - figyelő Kubernetes a Microsoft Operations Management Suite (OMS)"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, Micro-szolgáltatások, Kubernetes és az Azure-"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 54fa789453768529deaf25d7575e5b21d0e41882
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a><span data-ttu-id="d087b-104">A figyelő az Operations Management Suite Kubernetes fürt</span><span class="sxs-lookup"><span data-stu-id="d087b-104">Monitor a Kubernetes cluster with Operations Management Suite</span></span>

<span data-ttu-id="d087b-105">A Kubernetes fürt és a tárolók figyelése fontos, különösen akkor, ha több alkalmazáshoz léptékű termelési fürt felügyeletére.</span><span class="sxs-lookup"><span data-stu-id="d087b-105">Monitoring your Kubernetes cluster and containers is critical, especially when you manage a production cluster at scale with multiple apps.</span></span> 

<span data-ttu-id="d087b-106">Kihasználhatja a több Kubernetes figyelési megoldás, a Microsoft vagy más szolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="d087b-106">You can take advantage of several Kubernetes monitoring solutions, either from Microsoft or other providers.</span></span> <span data-ttu-id="d087b-107">Ebben az oktatóanyagban hello tárolók megoldás használatával figyelheti a Kubernetes fürt [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), a Microsoft felhőalapú informatikai felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="d087b-107">In this tutorial, you monitor your Kubernetes cluster by using hello Containers solution in [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), Microsoft's cloud-based IT management solution.</span></span> <span data-ttu-id="d087b-108">(OMS-tárolók megoldás hello előzetes verzió van.)</span><span class="sxs-lookup"><span data-stu-id="d087b-108">(hello OMS Containers solution is in preview.)</span></span>

<span data-ttu-id="d087b-109">Ebben az oktatóanyagban hét részét hét, a következő feladatok hello foglalkozik:</span><span class="sxs-lookup"><span data-stu-id="d087b-109">This tutorial, part seven of seven, covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d087b-110">OMS-munkaterület beállításainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="d087b-110">Get OMS Workspace settings</span></span>
> * <span data-ttu-id="d087b-111">Állítsa be az OMS-ügynököt a hello Kubernetes csomópontok</span><span class="sxs-lookup"><span data-stu-id="d087b-111">Set up OMS agents on hello Kubernetes nodes</span></span>
> * <span data-ttu-id="d087b-112">Elérni a figyelési adatait hello OMS-portálon vagy az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d087b-112">Access monitoring information in hello OMS portal or Azure portal</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d087b-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="d087b-113">Before you begin</span></span>

<span data-ttu-id="d087b-114">Előző oktatóanyagokat egy alkalmazás a tároló képek csomagolása, ezeket a lemezképeket tároló beállításjegyzék tooAzure, és létre Kubernetes fürt feltöltve.</span><span class="sxs-lookup"><span data-stu-id="d087b-114">In previous tutorials, an application was packaged into container images, these images uploaded tooAzure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="d087b-115">Ha nem volna ezeket a lépéseket, és szeretné mentén toofollow, visszaadása túl[oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="d087b-115">If you have not done these steps, and would like toofollow along, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="d087b-116">Legalább az oktatóanyag egy Kubernetes fürt Linux ügynök-csomópontok és az Operations Management Suite (OMS) fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="d087b-116">At minimum, this tutorial requires a Kubernetes cluster with Linux agent nodes, and an Operations Management Suite (OMS) account.</span></span> <span data-ttu-id="d087b-117">Ha szükséges, regisztráljon egy [OMS ingyenes](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).</span><span class="sxs-lookup"><span data-stu-id="d087b-117">If needed, sign up for a [free OMS trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).</span></span>

## <a name="get-workspace-settings"></a><span data-ttu-id="d087b-118">Munkaterület beállításainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="d087b-118">Get Workspace settings</span></span>

<span data-ttu-id="d087b-119">Hello elérésekor [OMS-portálon](https://mms.microsoft.com), nyissa meg túl**beállítások** > **csatlakoztatott források** > **Linux kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="d087b-119">When you can access hello [OMS portal](https://mms.microsoft.com), go too**Settings** > **Connected Sources** > **Linux Servers**.</span></span> <span data-ttu-id="d087b-120">Itt megtalálhatja hello *munkaterület azonosítója* és elsődleges vagy másodlagos *Munkaterületkulcsot*.</span><span class="sxs-lookup"><span data-stu-id="d087b-120">There, you can find hello *Workspace ID* and a primary or secondary *Workspace Key*.</span></span> <span data-ttu-id="d087b-121">Jegyezze fel ezeket az értékeket, amely szükséges az OMS-ügynököt a hello fürt mentése tooset.</span><span class="sxs-lookup"><span data-stu-id="d087b-121">Take note of these values, which you need tooset up OMS agents on hello cluster.</span></span>

## <a name="set-up-oms-agents"></a><span data-ttu-id="d087b-122">Állítsa be az OMS-ügynökök</span><span class="sxs-lookup"><span data-stu-id="d087b-122">Set up OMS agents</span></span>

<span data-ttu-id="d087b-123">Íme egy YAM fájl tooset mentése hello Linux fürtcsomópontokon OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="d087b-123">Here is a YAML file tooset up OMS agents on hello Linux cluster nodes.</span></span> <span data-ttu-id="d087b-124">Létrehoz egy Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), amely egy egyetlen azonos pod fut a fürt minden csomópontján.</span><span class="sxs-lookup"><span data-stu-id="d087b-124">It creates a Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), which runs a single identical pod on each cluster node.</span></span> <span data-ttu-id="d087b-125">hello DaemonSet erőforrás figyelési ügynök telepítéséhez ideális.</span><span class="sxs-lookup"><span data-stu-id="d087b-125">hello DaemonSet resource is ideal for deploying a monitoring agent.</span></span> 

<span data-ttu-id="d087b-126">A következő nevű tooa szövegfájl hello mentése `oms-daemonset.yaml`, és cserélje le a helyőrző értékeket hello *myWorkspaceID* és *myWorkspaceKey* az OMS-munkaterület azonosítója és kulcsa.</span><span class="sxs-lookup"><span data-stu-id="d087b-126">Save hello following text tooa file named `oms-daemonset.yaml`, and replace hello placeholder values for *myWorkspaceID* and *myWorkspaceKey* with your OMS Workspace ID and Key.</span></span> <span data-ttu-id="d087b-127">(A termelési, is kódol ezeket az értékeket, a titkos kulcsok.)</span><span class="sxs-lookup"><span data-stu-id="d087b-127">(In production, you can encode these values as secrets.)</span></span>

```YAML
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
 name: omsagent
spec:
 template:
  metadata:
   labels:
    app: omsagent
    agentVersion: v1.3.4-127
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: myWorkspaceID
       - name: KEY 
         value: myWorkspaceKey
       - name: DOMAIN
         value: opinsights.azure.com
       securityContext:
         privileged: true
       ports:
       - containerPort: 25225
         protocol: TCP 
       - containerPort: 25224
         protocol: UDP
       volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /var/log 
          name: host-log
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   volumes:
    - name: docker-sock 
      hostPath:
       path: /var/run/docker.sock
    - name: host-log
      hostPath:
       path: /var/log
```

<span data-ttu-id="d087b-128">Hozzon létre hello DaemonSet hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d087b-128">Create hello DaemonSet with hello following command:</span></span>

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

<span data-ttu-id="d087b-129">toosee DaemonSet jön létre, hogy hello futtassa:</span><span class="sxs-lookup"><span data-stu-id="d087b-129">toosee that hello DaemonSet is created, run:</span></span>

```azurecli-interactive
kubectl get daemonset
```

<span data-ttu-id="d087b-130">Hasonló toohello következő kimenete:</span><span class="sxs-lookup"><span data-stu-id="d087b-130">Output is similar toohello following:</span></span>

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

<span data-ttu-id="d087b-131">Miután hello ügynökök futnak, OMS tooingest és a folyamat hello adatok több percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="d087b-131">After hello agents are running, it takes several minutes for OMS tooingest and process hello data.</span></span>

## <a name="access-monitoring-data"></a><span data-ttu-id="d087b-132">Figyelési adatok hozzáférés</span><span class="sxs-lookup"><span data-stu-id="d087b-132">Access monitoring data</span></span>

<span data-ttu-id="d087b-133">Megtekintése és elemzése hello OMS-tárolófigyelő hello adatok [tároló megoldás](../../log-analytics/log-analytics-containers.md) hello OMS-portálon vagy hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="d087b-133">View and analyze hello OMS container monitoring data with hello [Container solution](../../log-analytics/log-analytics-containers.md) in either hello OMS portal or hello Azure portal.</span></span> 

<span data-ttu-id="d087b-134">hello használó tooinstall hello tároló megoldás [OMS-portálon](https://mms.microsoft.com), nyissa meg túl**megoldások gyűjtemény**.</span><span class="sxs-lookup"><span data-stu-id="d087b-134">tooinstall hello Container solution using hello [OMS portal](https://mms.microsoft.com), go too**Solutions Gallery**.</span></span> <span data-ttu-id="d087b-135">Majd adja hozzá **tároló megoldás**.</span><span class="sxs-lookup"><span data-stu-id="d087b-135">Then add **Container Solution**.</span></span> <span data-ttu-id="d087b-136">Másik lehetőségként adja hozzá hello tárolók megoldás hello [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).</span><span class="sxs-lookup"><span data-stu-id="d087b-136">Alternatively, add hello Containers solution from hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).</span></span>

<span data-ttu-id="d087b-137">Az OMS-portálon hello, keresse meg a **tárolók** hello OMS irányítópult összefoglalás csempére.</span><span class="sxs-lookup"><span data-stu-id="d087b-137">In hello OMS portal, look for a **Containers** summary tile on hello OMS dashboard.</span></span> <span data-ttu-id="d087b-138">Hello csempe, beleértve a részletekért kattintson: tároló események, a hibák, a állapota, a kép leltározás és a Processzor- és memóriahasználatról.</span><span class="sxs-lookup"><span data-stu-id="d087b-138">Click hello tile for details including: container events, errors, status, image inventory, and CPU and memory usage.</span></span> <span data-ttu-id="d087b-139">Részletesebb információkért kattintson egy sorra a bármely csempére, vagy hajtsa végre a [naplófájl-keresési](../../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="d087b-139">For more granular information, click a row on any tile, or perform a [log search](../../log-analytics/log-analytics-log-searches.md).</span></span>

![Az OMS-portálon tárolók irányítópult](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

<span data-ttu-id="d087b-141">Hasonlóképpen, a hello Azure-portálon, váltson túl**Naplóelemzési** , és válassza ki a munkaterület nevét.</span><span class="sxs-lookup"><span data-stu-id="d087b-141">Similarly, in hello Azure portal, go too**Log Analytics** and select your workspace name.</span></span> <span data-ttu-id="d087b-142">toosee hello **tárolók** összefoglalás csempére, kattintson a **megoldások** > **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="d087b-142">toosee hello **Containers** summary tile, click **Solutions** > **Containers**.</span></span> <span data-ttu-id="d087b-143">toosee részleteit, kattintson a hello csempére.</span><span class="sxs-lookup"><span data-stu-id="d087b-143">toosee details, click hello tile.</span></span>

<span data-ttu-id="d087b-144">Lásd: hello [Azure Log Analytics-dokumentáció](../../log-analytics/index.md) kérdez le, és a figyelési adatok elemzése részletes útmutatást.</span><span class="sxs-lookup"><span data-stu-id="d087b-144">See hello [Azure Log Analytics documentation](../../log-analytics/index.md) for detailed guidance on querying and analyzing monitoring data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d087b-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d087b-145">Next steps</span></span>

<span data-ttu-id="d087b-146">Ebben az oktatóanyagban az OMS Kubernetes fürt figyeli.</span><span class="sxs-lookup"><span data-stu-id="d087b-146">In this tutorial, you monitored your Kubernetes cluster with OMS.</span></span> <span data-ttu-id="d087b-147">Feladatok kezelt tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="d087b-147">Tasks covered included:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d087b-148">OMS-munkaterület beállításainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="d087b-148">Get OMS Workspace settings</span></span>
> * <span data-ttu-id="d087b-149">Állítsa be az OMS-ügynököt a hello Kubernetes csomópontok</span><span class="sxs-lookup"><span data-stu-id="d087b-149">Set up OMS agents on hello Kubernetes nodes</span></span>
> * <span data-ttu-id="d087b-150">Elérni a figyelési adatait hello OMS-portálon vagy az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d087b-150">Access monitoring information in hello OMS portal or Azure portal</span></span>


<span data-ttu-id="d087b-151">Kövesse a hivatkozást toosee előzetesen elkészített parancsfájl-mintában a tároló szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d087b-151">Follow this link toosee pre-built script samples for Container Service.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d087b-152">Azure Tárolószolgáltatás-parancsfájl minták</span><span class="sxs-lookup"><span data-stu-id="d087b-152">Azure Container Service script samples</span></span>](cli-samples.md)
