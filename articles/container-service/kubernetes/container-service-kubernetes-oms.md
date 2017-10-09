---
title: "Azure Kubernetes aaaMonitor - fürt Operations Management |} Microsoft Docs"
description: "Használatával a Microsoft Operations Management Suite Azure Tárolószolgáltatás-fürt Kubernetes figyelése"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 7474ee1571134ffe43ff8e4041cf5a64f5635bb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a><span data-ttu-id="2f97c-103">A figyelő az Azure Tárolószolgáltatás-fürtöt a Microsoft Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="2f97c-103">Monitor an Azure Container Service cluster with Microsoft Operations Management Suite (OMS)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f97c-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2f97c-104">Prerequisites</span></span>
<span data-ttu-id="2f97c-105">Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="2f97c-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="2f97c-106">Azt is feltételezi, hogy rendelkezik-e hello `az` Azure cli és `kubectl` eszközök vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="2f97c-106">It also assumes that you have hello `az` Azure cli and `kubectl` tools installed.</span></span>

<span data-ttu-id="2f97c-107">Ha hello tesztelheti `az` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="2f97c-107">You can test if you have hello `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="2f97c-108">Ha még nem rendelkezik hello `az` eszköz telepítve, az e-mail utasításokat is [Itt](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="2f97c-108">If you don't have hello `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>  
<span data-ttu-id="2f97c-109">Másik megoldásként használhatja [Azure Cloud rendszerhéj](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), hello rendelkező `az` Azure cli és `kubectl` már telepítette a eszközök.</span><span class="sxs-lookup"><span data-stu-id="2f97c-109">Alternatively, you can use [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), which has hello `az` Azure cli and `kubectl` tools already installed for you.</span></span>  

<span data-ttu-id="2f97c-110">Ha hello tesztelheti `kubectl` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="2f97c-110">You can test if you have hello `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="2f97c-111">Ha nem rendelkezik `kubectl` telepítve, futtatható:</span><span class="sxs-lookup"><span data-stu-id="2f97c-111">If you don't have `kubectl` installed, you can run:</span></span>
```console
$ az acs kubernetes install-cli
```

<span data-ttu-id="2f97c-112">Ha kubernetes kulcsok a kubectl eszköz telepítve rendelkezik tootest futtathatja:</span><span class="sxs-lookup"><span data-stu-id="2f97c-112">tootest if you have kubernetes keys installed in your kubectl tool you can run:</span></span>
```console
$ kubectl get nodes
```

<span data-ttu-id="2f97c-113">Ha hello fent parancs hibák ki, a kubectl eszközt szüksége tooinstall kubernetes fürt kulcsok.</span><span class="sxs-lookup"><span data-stu-id="2f97c-113">If hello above command errors out, you need tooinstall kubernetes cluster keys into your kubectl tool.</span></span> <span data-ttu-id="2f97c-114">Azt a parancs a következő hello teheti meg:</span><span class="sxs-lookup"><span data-stu-id="2f97c-114">You can do that with hello following command:</span></span>
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-operations-management-suite-oms"></a><span data-ttu-id="2f97c-115">Az Operations Management Suite (OMS) tárolók figyelése</span><span class="sxs-lookup"><span data-stu-id="2f97c-115">Monitoring Containers with Operations Management Suite (OMS)</span></span>

<span data-ttu-id="2f97c-116">A Microsoft Operations Management (OMS) a Microsoft felhőalapú informatikai felügyeleti megoldás, amely segít a kezelése és védelme a helyszíni és felhőalapú infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="2f97c-116">Microsoft Operations Management (OMS) is Microsoft's cloud-based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="2f97c-117">Tároló egy olyan megoldás, az OMS szolgáltatáshoz, így a nézet hello tároló szoftverleltár, a teljesítmény és a naplók egyetlen helyen.</span><span class="sxs-lookup"><span data-stu-id="2f97c-117">Container Solution is a solution in OMS Log Analytics, which helps you view hello container inventory, performance, and logs in a single location.</span></span> <span data-ttu-id="2f97c-118">Naplózási, tárolók hibaelhárításáról hello naplók megtekintése központi helyen, és zajos fel felesleges tároló egy gazdagépen található.</span><span class="sxs-lookup"><span data-stu-id="2f97c-118">You can audit, troubleshoot containers by viewing hello logs in centralized location, and find noisy consuming excess container on a host.</span></span>

![](media/container-service-monitoring-oms/image1.png)

<span data-ttu-id="2f97c-119">A tároló megoldásról további információkért tekintse meg az toothe [tároló megoldás Naplóelemzési](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="2f97c-119">For more information about Container Solution, please refer toothe [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

## <a name="installing-oms-on-kubernetes"></a><span data-ttu-id="2f97c-120">Kubernetes OMS telepítése</span><span class="sxs-lookup"><span data-stu-id="2f97c-120">Installing OMS on Kubernetes</span></span>

### <a name="obtain-your-workspace-id-and-key"></a><span data-ttu-id="2f97c-121">Munkaterületének Azonosítóját és kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="2f97c-121">Obtain your workspace ID and key</span></span>
<span data-ttu-id="2f97c-122">Az OMS ügynökszolgáltatás tootalk toohello hello kell konfigurálni a munkaterület azonosítóját és kulcsát egy toobe.</span><span class="sxs-lookup"><span data-stu-id="2f97c-122">For hello OMS agent tootalk toohello service it needs toobe configured with a workspace id and a workspace key.</span></span> <span data-ttu-id="2f97c-123">tooget hello munkaterület azonosítója és kulcsa az OMS toocreate van szüksége a fiókot a <https://mms.microsoft.com>. Kövesse az hello lépéseket toocreate fiókkal.</span><span class="sxs-lookup"><span data-stu-id="2f97c-123">tooget hello workspace id and key you need toocreate an OMS account at <https://mms.microsoft.com>. Please follow hello steps toocreate an account.</span></span> <span data-ttu-id="2f97c-124">Miután a létrehozásakor hello fiók, kell tooobtain a azonosítója és kulcsa kattintva **beállítások**, majd **csatlakoztatott források**, majd **Linux kiszolgálók**lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="2f97c-124">Once you are done creating hello account, you need tooobtain your id and key by clicking **Settings**, then **Connected Sources**, and then **Linux Servers**, as shown below.</span></span>

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-hello-oms-agent-using-a-daemonset"></a><span data-ttu-id="2f97c-125">Egy DaemonSet használatával hello OMS-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="2f97c-125">Install hello OMS agent using a DaemonSet</span></span>
<span data-ttu-id="2f97c-126">DaemonSets Kubernetes toorun által használt egy tároló minden gazdagépen hello fürt egyetlen példányát.</span><span class="sxs-lookup"><span data-stu-id="2f97c-126">DaemonSets are used by Kubernetes toorun a single instance of a container on each host in hello cluster.</span></span>
<span data-ttu-id="2f97c-127">Fontosságúak tökéletes figyelőügynökök futtatásához.</span><span class="sxs-lookup"><span data-stu-id="2f97c-127">They're perfect for running monitoring agents.</span></span>

<span data-ttu-id="2f97c-128">Íme hello [DaemonSet YAM fájl](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes).</span><span class="sxs-lookup"><span data-stu-id="2f97c-128">Here is hello [DaemonSet YAML file](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes).</span></span> <span data-ttu-id="2f97c-129">Menteni tooa nevű `oms-daemonset.yaml` , és cserélje le a helyőrző értékek hello `WSID` és `KEY` a munkaterület azonosítója és a kulcs hello fájlban.</span><span class="sxs-lookup"><span data-stu-id="2f97c-129">Save it tooa file named `oms-daemonset.yaml` and replace hello place-holder values for `WSID` and `KEY` with your workspace id and key in hello file.</span></span>

<span data-ttu-id="2f97c-130">Miután hozzáadta a munkaterület azonosítója és a kulcs toohello DaemonSet konfigurációs, telepítheti hello OMS-ügynököt a fürt hello `kubectl` parancssori eszköz:</span><span class="sxs-lookup"><span data-stu-id="2f97c-130">Once you have added your workspace ID and key toohello DaemonSet configuration, you can install hello OMS agent on your cluster with hello `kubectl` command line tool:</span></span>

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-hello-oms-agent-using-a-kubernetes-secret"></a><span data-ttu-id="2f97c-131">Kubernetes titkos kulcs használatával hello OMS-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="2f97c-131">Installing hello OMS agent using a Kubernetes Secret</span></span>
<span data-ttu-id="2f97c-132">tooprotect az OMS-munkaterület azonosítója és a DaemonSet YAM-fájl részeként is használhatja a Kubernetes titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="2f97c-132">tooprotect your OMS workspace ID and key you can use Kubernetes Secret as a part of DaemonSet YAML file.</span></span>

 - <span data-ttu-id="2f97c-133">Hello parancsfájl, titkos sablonfájl és hello DaemonSet YAM fájl másolása (a [tárház](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)), és győződjön meg arról, hogy a hello ugyanabban a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="2f97c-133">Copy hello script, secret template file and hello DaemonSet YAML file (from [repository](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) and make sure they are on hello same directory.</span></span> 
      - <span data-ttu-id="2f97c-134">Titkos kulcs parancsprogram - secret-gen.sh létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f97c-134">secret generating script - secret-gen.sh</span></span>
      - <span data-ttu-id="2f97c-135">titkos sablon - secret-template.yaml</span><span class="sxs-lookup"><span data-stu-id="2f97c-135">secret template - secret-template.yaml</span></span>
   - <span data-ttu-id="2f97c-136">DaemonSet YAM fájl - omsagent – ds-secrets.yaml</span><span class="sxs-lookup"><span data-stu-id="2f97c-136">DaemonSet YAML file - omsagent-ds-secrets.yaml</span></span>
 - <span data-ttu-id="2f97c-137">Hello parancsprogrammal.</span><span class="sxs-lookup"><span data-stu-id="2f97c-137">Run hello script.</span></span> <span data-ttu-id="2f97c-138">hello parancsfájl kéri hello OMS-munkaterület azonosítója és az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="2f97c-138">hello script will ask for hello OMS Workspace ID and Primary Key.</span></span> <span data-ttu-id="2f97c-139">Helyezze be, és hello parancsfájl létrehoz egy titkos yam fájlt, ezért is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="2f97c-139">Please insert that and hello script will create a secret yaml file so you can run it.</span></span>   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - <span data-ttu-id="2f97c-140">Hozzon létre hello titkok pod hello következő futtatásával:``` kubectl create -f omsagentsecret.yaml ```</span><span class="sxs-lookup"><span data-stu-id="2f97c-140">Create hello secrets pod by running hello following: ``` kubectl create -f omsagentsecret.yaml ```</span></span>
 
   - <span data-ttu-id="2f97c-141">Futtassa a következő hello toocheck:</span><span class="sxs-lookup"><span data-stu-id="2f97c-141">toocheck, run hello following:</span></span> 

   ``` 
   root@ubuntu16-13db:~# kubectl get secrets
   NAME                  TYPE                                  DATA      AGE
   default-token-gvl91   kubernetes.io/service-account-token   3         50d
   omsagent-secret       Opaque                                2         1d
   root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
   Name:           omsagent-secret
   Namespace:      default
   Labels:         <none>
   Annotations:    <none>

   Type:   Opaque

   Data
   ====
   WSID:   36 bytes
   KEY:    88 bytes 
   ```
 
  - <span data-ttu-id="2f97c-142">A omsagent futó démon-készlet létrehozása``` kubectl create -f omsagent-ds-secrets.yaml ```</span><span class="sxs-lookup"><span data-stu-id="2f97c-142">Create your omsagent daemon-set by running ``` kubectl create -f omsagent-ds-secrets.yaml ```</span></span>

### <a name="conclusion"></a><span data-ttu-id="2f97c-143">Összegzés</span><span class="sxs-lookup"><span data-stu-id="2f97c-143">Conclusion</span></span>
<span data-ttu-id="2f97c-144">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="2f97c-144">That's it!</span></span> <span data-ttu-id="2f97c-145">Néhány perc elteltével tud toosee adatok továbbítására tooyour OMS irányítópult kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2f97c-145">After a few minutes, you should be able toosee data flowing tooyour OMS dashboard.</span></span>
