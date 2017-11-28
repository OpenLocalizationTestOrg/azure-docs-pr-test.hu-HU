---
title: "aaaMonitor Azure Kubernetes fürt - Sysdig |} Microsoft Docs"
description: "Figyelési Kubernetes Sysdig használata az Azure Tárolószolgáltatás-fürt"
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
ms.openlocfilehash: a27813d01ee4624b9e993f6185169ad73aeec3a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a><span data-ttu-id="ff953-103">Egy Azure-tároló szolgáltatás Kubernetes fürtjéhez Sysdig figyelése</span><span class="sxs-lookup"><span data-stu-id="ff953-103">Monitor an Azure Container Service Kubernetes cluster using Sysdig</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff953-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ff953-104">Prerequisites</span></span>
<span data-ttu-id="ff953-105">Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="ff953-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="ff953-106">Azt is feltételezi, hogy rendelkezik-e hello azure cli és kubectl eszközök vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="ff953-106">It also assumes that you have hello azure cli and kubectl tools installed.</span></span>

<span data-ttu-id="ff953-107">Ha hello tesztelheti `az` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="ff953-107">You can test if you have hello `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="ff953-108">Ha még nem rendelkezik hello `az` eszköz telepítve, az e-mail utasításokat is [Itt](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="ff953-108">If you don't have hello `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="ff953-109">Ha hello tesztelheti `kubectl` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="ff953-109">You can test if you have hello `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="ff953-110">Ha nem rendelkezik `kubectl` telepítve, futtatható:</span><span class="sxs-lookup"><span data-stu-id="ff953-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="sysdig"></a><span data-ttu-id="ff953-111">Sysdig</span><span class="sxs-lookup"><span data-stu-id="ff953-111">Sysdig</span></span>
<span data-ttu-id="ff953-112">Sysdig egy külső "figyelés" a szolgáltatás vállalat, amely képes figyelni az Azure-beli Kubernetes fürt tárolók.</span><span class="sxs-lookup"><span data-stu-id="ff953-112">Sysdig is an external monitoring as a service company which can monitor containers in your Kubernetes cluster running in Azure.</span></span> <span data-ttu-id="ff953-113">Aktív Sysdig fiók Sysdig a használatához.</span><span class="sxs-lookup"><span data-stu-id="ff953-113">Using Sysdig requires an active Sysdig account.</span></span>
<span data-ttu-id="ff953-114">Iratkozzon fel a fiókot saját [hely](https://app.sysdigcloud.com).</span><span class="sxs-lookup"><span data-stu-id="ff953-114">You can sign up for an account on their [site](https://app.sysdigcloud.com).</span></span>

<span data-ttu-id="ff953-115">Miután toohello Sysdig felhő webhelyen jelentkezett, kattintson a felhasználónevére, és hello oldalon kell megjelennie a "hozzáférési kulcsot."</span><span class="sxs-lookup"><span data-stu-id="ff953-115">Once you're logged in toohello Sysdig cloud website, click on your user name, and on hello page you should see your "Access Key."</span></span> 

![Sysdig API-kulcs](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-hello-sysdig-agents-tookubernetes"></a><span data-ttu-id="ff953-117">Hello Sysdig ügynökök tooKubernetes telepítése</span><span class="sxs-lookup"><span data-stu-id="ff953-117">Installing hello Sysdig agents tooKubernetes</span></span>
<span data-ttu-id="ff953-118">a tárolók Sysdig fut a folyamat minden egyes számítógép segítségével egy Kubernetes toomonitor `DaemonSet`.</span><span class="sxs-lookup"><span data-stu-id="ff953-118">toomonitor your containers, Sysdig runs a process on each machine using a Kubernetes `DaemonSet`.</span></span>
<span data-ttu-id="ff953-119">DaemonSets olyan gépenként tárolója egyetlen példányát futtató Kubernetes API-objektumok.</span><span class="sxs-lookup"><span data-stu-id="ff953-119">DaemonSets are Kubernetes API objects that run a single instance of a container per machine.</span></span>
<span data-ttu-id="ff953-120">Fontosságúak tökéletes megoldás az eszközök, például hello Sysdig tartozó figyelési ügynök telepítése.</span><span class="sxs-lookup"><span data-stu-id="ff953-120">They're perfect for installing tools like hello Sysdig's monitoring agent.</span></span>

<span data-ttu-id="ff953-121">először letöltse tooinstall hello Sysdig daemonset [hello sablon](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) a sysdig.</span><span class="sxs-lookup"><span data-stu-id="ff953-121">tooinstall hello Sysdig daemonset, you should first download [hello template](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) from sysdig.</span></span> <span data-ttu-id="ff953-122">Mentse a fájlt `sysdig-daemonset.yaml`.</span><span class="sxs-lookup"><span data-stu-id="ff953-122">Save that file as `sysdig-daemonset.yaml`.</span></span>

<span data-ttu-id="ff953-123">Linux-és OS X futtathatja:</span><span class="sxs-lookup"><span data-stu-id="ff953-123">On Linux and OS X you can run:</span></span>

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

<span data-ttu-id="ff953-124">A PowerShell:</span><span class="sxs-lookup"><span data-stu-id="ff953-124">In PowerShell:</span></span>

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

<span data-ttu-id="ff953-125">A hozzáférési kulcs, Sysdig fiókjából beszerzett mellett szerkesztése az adott fájl tooinsert.</span><span class="sxs-lookup"><span data-stu-id="ff953-125">Next edit that file tooinsert your Access Key, that you obtained from your Sysdig account.</span></span>

<span data-ttu-id="ff953-126">Végezetül hozza létre a hello DaemonSet:</span><span class="sxs-lookup"><span data-stu-id="ff953-126">Finally, create hello DaemonSet:</span></span>

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a><span data-ttu-id="ff953-127">A figyelést megtekintése</span><span class="sxs-lookup"><span data-stu-id="ff953-127">View your monitoring</span></span>
<span data-ttu-id="ff953-128">Miután telepített és futó, hello ügynökök adatokat hátsó tooSysdig kell szivattyú.</span><span class="sxs-lookup"><span data-stu-id="ff953-128">Once installed and running, hello agents should pump data back tooSysdig.</span></span>  <span data-ttu-id="ff953-129">Lépjen vissza toothe [sysdig irányítópult](https://app.sysdigcloud.com) és megjelenítheti a tárolók kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="ff953-129">Go back toothe [sysdig dashboard](https://app.sysdigcloud.com) and you should see information about your containers.</span></span>

<span data-ttu-id="ff953-130">Kubernetes-specifikus irányítópultok keresztül is telepíthet a [új irányítópult varázsló](https://app.sysdigcloud.com/#/dashboards/new).</span><span class="sxs-lookup"><span data-stu-id="ff953-130">You can also install Kubernetes-specific dashboards via the [new dashboard wizard](https://app.sysdigcloud.com/#/dashboards/new).</span></span>
