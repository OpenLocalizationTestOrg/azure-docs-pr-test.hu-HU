---
title: "A figyelő Azure Kubernetes fürt - műveletek kezelése |} Microsoft Docs"
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
ms.openlocfilehash: bd5c81435c091d25bc14710589b7c043e9f56a25
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a><span data-ttu-id="9782a-103">A figyelő az Azure Tárolószolgáltatás-fürtöt a Microsoft Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="9782a-103">Monitor an Azure Container Service cluster with Microsoft Operations Management Suite (OMS)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9782a-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9782a-104">Prerequisites</span></span>
<span data-ttu-id="9782a-105">Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="9782a-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="9782a-106">Azt is feltételezi, hogy a `az` Azure cli és `kubectl` eszközök vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="9782a-106">It also assumes that you have the `az` Azure cli and `kubectl` tools installed.</span></span>

<span data-ttu-id="9782a-107">Ha tesztelheti a `az` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="9782a-107">You can test if you have the `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="9782a-108">Ha nem rendelkezik a `az` eszköz telepítve, az e-mail utasításokat is [Itt](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="9782a-108">If you don't have the `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>  
<span data-ttu-id="9782a-109">Használhatja azt is megteheti, [Azure Cloud rendszerhéj](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), amelynek a `az` Azure cli és `kubectl` már telepítette a eszközök.</span><span class="sxs-lookup"><span data-stu-id="9782a-109">Alternatively, you can use [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), which has the `az` Azure cli and `kubectl` tools already installed for you.</span></span>  

<span data-ttu-id="9782a-110">Ha tesztelheti a `kubectl` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="9782a-110">You can test if you have the `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="9782a-111">Ha nem rendelkezik `kubectl` telepítve, futtatható:</span><span class="sxs-lookup"><span data-stu-id="9782a-111">If you don't have `kubectl` installed, you can run:</span></span>
```console
$ az acs kubernetes install-cli
```

<span data-ttu-id="9782a-112">Ha telepítette a kubectl eszköz futtatása kubernetes kulcsok tesztelése:</span><span class="sxs-lookup"><span data-stu-id="9782a-112">To test if you have kubernetes keys installed in your kubectl tool you can run:</span></span>
```console
$ kubectl get nodes
```

<span data-ttu-id="9782a-113">Ha a fenti parancs hibák kimenő, telepítendő kubernetes fürt kulcsok a kubectl eszközt.</span><span class="sxs-lookup"><span data-stu-id="9782a-113">If the above command errors out, you need to install kubernetes cluster keys into your kubectl tool.</span></span> <span data-ttu-id="9782a-114">Azt a következő paranccsal teheti meg:</span><span class="sxs-lookup"><span data-stu-id="9782a-114">You can do that with the following command:</span></span>
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-operations-management-suite-oms"></a><span data-ttu-id="9782a-115">Az Operations Management Suite (OMS) tárolók figyelése</span><span class="sxs-lookup"><span data-stu-id="9782a-115">Monitoring Containers with Operations Management Suite (OMS)</span></span>

<span data-ttu-id="9782a-116">A Microsoft Operations Management (OMS) a Microsoft felhőalapú informatikai felügyeleti megoldás, amely segít a kezelése és védelme a helyszíni és felhőalapú infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="9782a-116">Microsoft Operations Management (OMS) is Microsoft's cloud-based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="9782a-117">Tároló egy olyan megoldás az OMS Naplóelemzési, amely segít a tároló szoftverleltár, a teljesítmény és a naplók megtekintéséhez egyetlen helyen.</span><span class="sxs-lookup"><span data-stu-id="9782a-117">Container Solution is a solution in OMS Log Analytics, which helps you view the container inventory, performance, and logs in a single location.</span></span> <span data-ttu-id="9782a-118">Naplózási, tárolók hibaelhárítás a naplók megtekintése központi helyen, és zajos fel felesleges tároló egy gazdagépen található.</span><span class="sxs-lookup"><span data-stu-id="9782a-118">You can audit, troubleshoot containers by viewing the logs in centralized location, and find noisy consuming excess container on a host.</span></span>

![](media/container-service-monitoring-oms/image1.png)

<span data-ttu-id="9782a-119">A tároló megoldásról további információkért tekintse meg a [tároló megoldás Naplóelemzési](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="9782a-119">For more information about Container Solution, please refer to the [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

## <a name="installing-oms-on-kubernetes"></a><span data-ttu-id="9782a-120">Kubernetes OMS telepítése</span><span class="sxs-lookup"><span data-stu-id="9782a-120">Installing OMS on Kubernetes</span></span>

### <a name="obtain-your-workspace-id-and-key"></a><span data-ttu-id="9782a-121">Munkaterületének Azonosítóját és kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="9782a-121">Obtain your workspace ID and key</span></span>
<span data-ttu-id="9782a-122">Az OMS felvegye a szolgáltatás az ügynök kell konfigurálni a munkaterület azonosítóját és kulcsát egy.</span><span class="sxs-lookup"><span data-stu-id="9782a-122">For the OMS agent to talk to the service it needs to be configured with a workspace id and a workspace key.</span></span> <span data-ttu-id="9782a-123">A munkaterület azonosítója és kulcsa OMS fiók létrehozásához szükséges <https://mms.microsoft.com>. Kérjük, kövesse a lépéseket egy fiók létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9782a-123">To get the workspace id and key you need to create an OMS account at <https://mms.microsoft.com>. Please follow the steps to create an account.</span></span> <span data-ttu-id="9782a-124">Ha elkészült a fiók létrehozását, be kell szereznie a azonosítója és kulcsa kattintva **beállítások**, majd **csatlakoztatott források**, majd **Linux kiszolgálók**lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="9782a-124">Once you are done creating the account, you need to obtain your id and key by clicking **Settings**, then **Connected Sources**, and then **Linux Servers**, as shown below.</span></span>

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-oms-agent-using-a-daemonset"></a><span data-ttu-id="9782a-125">Egy DaemonSet használatával OMS-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="9782a-125">Install the OMS agent using a DaemonSet</span></span>
<span data-ttu-id="9782a-126">A tároló egyetlen példány futhat az a fürt minden egyes állomás DaemonSets Kubernetes használják.</span><span class="sxs-lookup"><span data-stu-id="9782a-126">DaemonSets are used by Kubernetes to run a single instance of a container on each host in the cluster.</span></span>
<span data-ttu-id="9782a-127">Fontosságúak tökéletes figyelőügynökök futtatásához.</span><span class="sxs-lookup"><span data-stu-id="9782a-127">They're perfect for running monitoring agents.</span></span>

<span data-ttu-id="9782a-128">Itt a [DaemonSet YAM fájl](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes).</span><span class="sxs-lookup"><span data-stu-id="9782a-128">Here is the [DaemonSet YAML file](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes).</span></span> <span data-ttu-id="9782a-129">Mentse a fájlt `oms-daemonset.yaml` , és cserélje le a helyőrző értékeinek `WSID` és `KEY` a munkaterület azonosítója és a fájlban kulccsal.</span><span class="sxs-lookup"><span data-stu-id="9782a-129">Save it to a file named `oms-daemonset.yaml` and replace the place-holder values for `WSID` and `KEY` with your workspace id and key in the file.</span></span>

<span data-ttu-id="9782a-130">Miután hozzáadta a munkaterület azonosítója és kulcsa az DaemonSet konfigurációhoz, telepítheti az OMS-ügynököt a fürt a `kubectl` parancssori eszköz:</span><span class="sxs-lookup"><span data-stu-id="9782a-130">Once you have added your workspace ID and key to the DaemonSet configuration, you can install the OMS agent on your cluster with the `kubectl` command line tool:</span></span>

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-oms-agent-using-a-kubernetes-secret"></a><span data-ttu-id="9782a-131">Kubernetes titkos kulcs használata az OMS-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="9782a-131">Installing the OMS agent using a Kubernetes Secret</span></span>
<span data-ttu-id="9782a-132">Az OMS-munkaterület azonosítója és kulcsára használhatja Kubernetes titkos DaemonSet YAM-fájl részeként.</span><span class="sxs-lookup"><span data-stu-id="9782a-132">To protect your OMS workspace ID and key you can use Kubernetes Secret as a part of DaemonSet YAML file.</span></span>

 - <span data-ttu-id="9782a-133">Másolja a parancsfájlt, titkos sablon és a DaemonSet YAM fájlt (a [tárház](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)), és győződjön meg arról, hogy az ugyanabban a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="9782a-133">Copy the script, secret template file and the DaemonSet YAML file (from [repository](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) and make sure they are on the same directory.</span></span> 
      - <span data-ttu-id="9782a-134">Titkos kulcs parancsprogram - secret-gen.sh létrehozása</span><span class="sxs-lookup"><span data-stu-id="9782a-134">secret generating script - secret-gen.sh</span></span>
      - <span data-ttu-id="9782a-135">titkos sablon - secret-template.yaml</span><span class="sxs-lookup"><span data-stu-id="9782a-135">secret template - secret-template.yaml</span></span>
   - <span data-ttu-id="9782a-136">DaemonSet YAM fájl - omsagent – ds-secrets.yaml</span><span class="sxs-lookup"><span data-stu-id="9782a-136">DaemonSet YAML file - omsagent-ds-secrets.yaml</span></span>
 - <span data-ttu-id="9782a-137">Futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="9782a-137">Run the script.</span></span> <span data-ttu-id="9782a-138">A parancsprogram kéri az OMS-munkaterület azonosítója és az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="9782a-138">The script will ask for the OMS Workspace ID and Primary Key.</span></span> <span data-ttu-id="9782a-139">Helyezze be, és a parancsfájl létrehoz egy titkos yam fájlt, ezért is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="9782a-139">Please insert that and the script will create a secret yaml file so you can run it.</span></span>   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - <span data-ttu-id="9782a-140">Hozza létre a titkos kulcsok fogyasztanak a következő futtatásával:``` kubectl create -f omsagentsecret.yaml ```</span><span class="sxs-lookup"><span data-stu-id="9782a-140">Create the secrets pod by running the following: ``` kubectl create -f omsagentsecret.yaml ```</span></span>
 
   - <span data-ttu-id="9782a-141">Ellenőrzéséhez futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="9782a-141">To check, run the following:</span></span> 

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
 
  - <span data-ttu-id="9782a-142">A omsagent futó démon-készlet létrehozása``` kubectl create -f omsagent-ds-secrets.yaml ```</span><span class="sxs-lookup"><span data-stu-id="9782a-142">Create your omsagent daemon-set by running ``` kubectl create -f omsagent-ds-secrets.yaml ```</span></span>

### <a name="conclusion"></a><span data-ttu-id="9782a-143">Összegzés</span><span class="sxs-lookup"><span data-stu-id="9782a-143">Conclusion</span></span>
<span data-ttu-id="9782a-144">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="9782a-144">That's it!</span></span> <span data-ttu-id="9782a-145">Néhány perc múlva megtekintheti az OMS-irányítópultra áramló adatokat kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9782a-145">After a few minutes, you should be able to see data flowing to your OMS dashboard.</span></span>
