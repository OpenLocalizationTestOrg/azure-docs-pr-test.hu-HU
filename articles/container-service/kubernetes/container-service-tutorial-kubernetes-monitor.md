---
title: "Azure Tárolószolgáltatás útmutató - figyelő Kubernetes |} Microsoft Docs"
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
ms.openlocfilehash: f4d09973ada8e3cd0ff2b00d20aca979e834cd7f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a><span data-ttu-id="fffe1-104">A figyelő az Operations Management Suite Kubernetes fürt</span><span class="sxs-lookup"><span data-stu-id="fffe1-104">Monitor a Kubernetes cluster with Operations Management Suite</span></span>

<span data-ttu-id="fffe1-105">A Kubernetes fürt és a tárolók figyelése fontos, különösen akkor, ha több alkalmazáshoz léptékű termelési fürt felügyeletére.</span><span class="sxs-lookup"><span data-stu-id="fffe1-105">Monitoring your Kubernetes cluster and containers is critical, especially when you manage a production cluster at scale with multiple apps.</span></span> 

<span data-ttu-id="fffe1-106">Kihasználhatja a több Kubernetes figyelési megoldás, a Microsoft vagy más szolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="fffe1-106">You can take advantage of several Kubernetes monitoring solutions, either from Microsoft or other providers.</span></span> <span data-ttu-id="fffe1-107">Ebben az oktatóanyagban a tárolók megoldás használatával figyelheti a Kubernetes fürt [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), a Microsoft felhőalapú informatikai felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="fffe1-107">In this tutorial, you monitor your Kubernetes cluster by using the Containers solution in [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), Microsoft's cloud-based IT management solution.</span></span> <span data-ttu-id="fffe1-108">(A képen van az OMS-tárolók megoldás.)</span><span class="sxs-lookup"><span data-stu-id="fffe1-108">(The OMS Containers solution is in preview.)</span></span>

<span data-ttu-id="fffe1-109">Ebben az oktatóanyagban hét részét hét, a következő feladatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="fffe1-109">This tutorial, part seven of seven, covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fffe1-110">OMS-munkaterület beállításainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="fffe1-110">Get OMS Workspace settings</span></span>
> * <span data-ttu-id="fffe1-111">Állítsa be a Kubernetes csomópontok OMS-ügynökök</span><span class="sxs-lookup"><span data-stu-id="fffe1-111">Set up OMS agents on the Kubernetes nodes</span></span>
> * <span data-ttu-id="fffe1-112">Elérni a figyelési adatait az OMS-portálon vagy az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="fffe1-112">Access monitoring information in the OMS portal or Azure portal</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="fffe1-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="fffe1-113">Before you begin</span></span>

<span data-ttu-id="fffe1-114">Az előző oktatóanyagok tároló lemezképek, a feltöltött Azure tároló beállításjegyzék ezeket a lemezképeket és a létrehozott Kubernetes fürt alkalmazás lett csomagolva.</span><span class="sxs-lookup"><span data-stu-id="fffe1-114">In previous tutorials, an application was packaged into container images, these images uploaded to Azure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="fffe1-115">Ha nem volna ezeket a lépéseket, és szeretné követéséhez, vissza [oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="fffe1-115">If you have not done these steps, and would like to follow along, return to [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="fffe1-116">Legalább az oktatóanyag egy Kubernetes fürt Linux ügynök-csomópontok és az Operations Management Suite (OMS) fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="fffe1-116">At minimum, this tutorial requires a Kubernetes cluster with Linux agent nodes, and an Operations Management Suite (OMS) account.</span></span> <span data-ttu-id="fffe1-117">Ha szükséges, regisztráljon egy [OMS ingyenes](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).</span><span class="sxs-lookup"><span data-stu-id="fffe1-117">If needed, sign up for a [free OMS trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).</span></span>

## <a name="get-workspace-settings"></a><span data-ttu-id="fffe1-118">Munkaterület beállításainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="fffe1-118">Get Workspace settings</span></span>

<span data-ttu-id="fffe1-119">Amikor megnyitja a [OMS-portálon](https://mms.microsoft.com), keresse fel **beállítások** > **csatlakoztatott források** > **Linux kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="fffe1-119">When you can access the [OMS portal](https://mms.microsoft.com), go to **Settings** > **Connected Sources** > **Linux Servers**.</span></span> <span data-ttu-id="fffe1-120">Itt megtalálhatja a *munkaterület azonosítója* és elsődleges vagy másodlagos *Munkaterületkulcsot*.</span><span class="sxs-lookup"><span data-stu-id="fffe1-120">There, you can find the *Workspace ID* and a primary or secondary *Workspace Key*.</span></span> <span data-ttu-id="fffe1-121">Jegyezze fel ezeket az értékeket, amely OMS-ügynököt a fürt beállításához szükséges.</span><span class="sxs-lookup"><span data-stu-id="fffe1-121">Take note of these values, which you need to set up OMS agents on the cluster.</span></span>

## <a name="set-up-oms-agents"></a><span data-ttu-id="fffe1-122">Állítsa be az OMS-ügynökök</span><span class="sxs-lookup"><span data-stu-id="fffe1-122">Set up OMS agents</span></span>

<span data-ttu-id="fffe1-123">Itt egy olyan YAM fájl OMS-ügynököt a Linux fürtcsomópontokon beállítása.</span><span class="sxs-lookup"><span data-stu-id="fffe1-123">Here is a YAML file to set up OMS agents on the Linux cluster nodes.</span></span> <span data-ttu-id="fffe1-124">Létrehoz egy Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), amely egy egyetlen azonos pod fut a fürt minden csomópontján.</span><span class="sxs-lookup"><span data-stu-id="fffe1-124">It creates a Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), which runs a single identical pod on each cluster node.</span></span> <span data-ttu-id="fffe1-125">A DaemonSet erőforrás figyelési ügynök telepítéséhez ideális.</span><span class="sxs-lookup"><span data-stu-id="fffe1-125">The DaemonSet resource is ideal for deploying a monitoring agent.</span></span> 

<span data-ttu-id="fffe1-126">A következő szöveg nevű fájlba mentése `oms-daemonset.yaml`, és cserélje le a helyőrző értékeket az *myWorkspaceID* és *myWorkspaceKey* az OMS-munkaterület azonosítója és kulcsa.</span><span class="sxs-lookup"><span data-stu-id="fffe1-126">Save the following text to a file named `oms-daemonset.yaml`, and replace the placeholder values for *myWorkspaceID* and *myWorkspaceKey* with your OMS Workspace ID and Key.</span></span> <span data-ttu-id="fffe1-127">(A termelési, is kódol ezeket az értékeket, a titkos kulcsok.)</span><span class="sxs-lookup"><span data-stu-id="fffe1-127">(In production, you can encode these values as secrets.)</span></span>

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

<span data-ttu-id="fffe1-128">A DaemonSet létrehozása a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fffe1-128">Create the DaemonSet with the following command:</span></span>

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

<span data-ttu-id="fffe1-129">A DaemonSet létrehozott megtekintéséhez futtassa:</span><span class="sxs-lookup"><span data-stu-id="fffe1-129">To see that the DaemonSet is created, run:</span></span>

```azurecli-interactive
kubectl get daemonset
```

<span data-ttu-id="fffe1-130">A kimenet a következőkhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="fffe1-130">Output is similar to the following:</span></span>

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

<span data-ttu-id="fffe1-131">Miután az ügynökök futnak, OMS betöltési és feldolgozni az adatokat több percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="fffe1-131">After the agents are running, it takes several minutes for OMS to ingest and process the data.</span></span>

## <a name="access-monitoring-data"></a><span data-ttu-id="fffe1-132">Figyelési adatok hozzáférés</span><span class="sxs-lookup"><span data-stu-id="fffe1-132">Access monitoring data</span></span>

<span data-ttu-id="fffe1-133">Megtekintése és elemzése a figyelési adatok az OMS-tároló a [tároló megoldás](../../log-analytics/log-analytics-containers.md) az OMS-portálon vagy az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fffe1-133">View and analyze the OMS container monitoring data with the [Container solution](../../log-analytics/log-analytics-containers.md) in either the OMS portal or the Azure portal.</span></span> 

<span data-ttu-id="fffe1-134">A tároló megoldás használatával telepítéséhez a [OMS-portálon](https://mms.microsoft.com), keresse fel **megoldások gyűjteménye**.</span><span class="sxs-lookup"><span data-stu-id="fffe1-134">To install the Container solution using the [OMS portal](https://mms.microsoft.com), go to **Solutions Gallery**.</span></span> <span data-ttu-id="fffe1-135">Majd adja hozzá **tároló megoldás**.</span><span class="sxs-lookup"><span data-stu-id="fffe1-135">Then add **Container Solution**.</span></span> <span data-ttu-id="fffe1-136">Másik lehetőségként adja hozzá a tárolók megoldást a [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).</span><span class="sxs-lookup"><span data-stu-id="fffe1-136">Alternatively, add the Containers solution from the [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).</span></span>

<span data-ttu-id="fffe1-137">Az OMS-portálon keresse meg a **tárolók** összefoglalás csempéje az OMS-irányítópultot.</span><span class="sxs-lookup"><span data-stu-id="fffe1-137">In the OMS portal, look for a **Containers** summary tile on the OMS dashboard.</span></span> <span data-ttu-id="fffe1-138">Kattintson a csempére vonatkozó információt talál többek között: tároló események, a hibák, a állapota, a kép leltározás és a Processzor- és memóriahasználatról.</span><span class="sxs-lookup"><span data-stu-id="fffe1-138">Click the tile for details including: container events, errors, status, image inventory, and CPU and memory usage.</span></span> <span data-ttu-id="fffe1-139">Részletesebb információkért kattintson egy sorra a bármely csempére, vagy hajtsa végre a [naplófájl-keresési](../../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="fffe1-139">For more granular information, click a row on any tile, or perform a [log search](../../log-analytics/log-analytics-log-searches.md).</span></span>

![Az OMS-portálon tárolók irányítópult](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

<span data-ttu-id="fffe1-141">Ehhez hasonlóan az Azure-portálon lépjen **Naplóelemzési** , és válassza ki a munkaterület nevét.</span><span class="sxs-lookup"><span data-stu-id="fffe1-141">Similarly, in the Azure portal, go to **Log Analytics** and select your workspace name.</span></span> <span data-ttu-id="fffe1-142">Hogy a **tárolók** összefoglalás csempére, kattintson a **megoldások** > **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="fffe1-142">To see the **Containers** summary tile, click **Solutions** > **Containers**.</span></span> <span data-ttu-id="fffe1-143">Részletek megtekintéséhez kattintson a csempére.</span><span class="sxs-lookup"><span data-stu-id="fffe1-143">To see details, click the tile.</span></span>

<span data-ttu-id="fffe1-144">Tekintse meg a [Azure Log Analytics-dokumentáció](../../log-analytics/index.md) kérdez le, és a figyelési adatok elemzése részletes útmutatást.</span><span class="sxs-lookup"><span data-stu-id="fffe1-144">See the [Azure Log Analytics documentation](../../log-analytics/index.md) for detailed guidance on querying and analyzing monitoring data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fffe1-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fffe1-145">Next steps</span></span>

<span data-ttu-id="fffe1-146">Ebben az oktatóanyagban az OMS Kubernetes fürt figyeli.</span><span class="sxs-lookup"><span data-stu-id="fffe1-146">In this tutorial, you monitored your Kubernetes cluster with OMS.</span></span> <span data-ttu-id="fffe1-147">Feladatok kezelt tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="fffe1-147">Tasks covered included:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fffe1-148">OMS-munkaterület beállításainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="fffe1-148">Get OMS Workspace settings</span></span>
> * <span data-ttu-id="fffe1-149">Állítsa be a Kubernetes csomópontok OMS-ügynökök</span><span class="sxs-lookup"><span data-stu-id="fffe1-149">Set up OMS agents on the Kubernetes nodes</span></span>
> * <span data-ttu-id="fffe1-150">Elérni a figyelési adatait az OMS-portálon vagy az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="fffe1-150">Access monitoring information in the OMS portal or Azure portal</span></span>


<span data-ttu-id="fffe1-151">Kövesse a hivatkozásra kattintva megtekintheti az előre elkészített parancsfájl minták számára.</span><span class="sxs-lookup"><span data-stu-id="fffe1-151">Follow this link to see pre-built script samples for Container Service.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fffe1-152">Azure Tárolószolgáltatás-parancsfájl minták</span><span class="sxs-lookup"><span data-stu-id="fffe1-152">Azure Container Service script samples</span></span>](cli-samples.md)
