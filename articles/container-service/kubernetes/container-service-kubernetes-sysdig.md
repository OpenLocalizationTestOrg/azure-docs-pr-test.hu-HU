---
title: "A figyelő Azure Kubernetes fürt - Sysdig |} Microsoft Docs"
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
ms.openlocfilehash: afe22b84015526f901111238e36baaa94694ccbf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a><span data-ttu-id="83c3e-103">Egy Azure-tároló szolgáltatás Kubernetes fürtjéhez Sysdig figyelése</span><span class="sxs-lookup"><span data-stu-id="83c3e-103">Monitor an Azure Container Service Kubernetes cluster using Sysdig</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83c3e-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="83c3e-104">Prerequisites</span></span>
<span data-ttu-id="83c3e-105">Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="83c3e-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="83c3e-106">Azt is feltételezi, hogy rendelkezik-e az azure cli és kubectl eszközök vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="83c3e-106">It also assumes that you have the azure cli and kubectl tools installed.</span></span>

<span data-ttu-id="83c3e-107">Ha tesztelheti a `az` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="83c3e-107">You can test if you have the `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="83c3e-108">Ha nem rendelkezik a `az` eszköz telepítve, az e-mail utasításokat is [Itt](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="83c3e-108">If you don't have the `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="83c3e-109">Ha tesztelheti a `kubectl` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="83c3e-109">You can test if you have the `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="83c3e-110">Ha nem rendelkezik `kubectl` telepítve, futtatható:</span><span class="sxs-lookup"><span data-stu-id="83c3e-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="sysdig"></a><span data-ttu-id="83c3e-111">Sysdig</span><span class="sxs-lookup"><span data-stu-id="83c3e-111">Sysdig</span></span>
<span data-ttu-id="83c3e-112">Sysdig egy külső "figyelés" a szolgáltatás vállalat, amely képes figyelni az Azure-beli Kubernetes fürt tárolók.</span><span class="sxs-lookup"><span data-stu-id="83c3e-112">Sysdig is an external monitoring as a service company which can monitor containers in your Kubernetes cluster running in Azure.</span></span> <span data-ttu-id="83c3e-113">Aktív Sysdig fiók Sysdig a használatához.</span><span class="sxs-lookup"><span data-stu-id="83c3e-113">Using Sysdig requires an active Sysdig account.</span></span>
<span data-ttu-id="83c3e-114">Iratkozzon fel a fiókot saját [hely](https://app.sysdigcloud.com).</span><span class="sxs-lookup"><span data-stu-id="83c3e-114">You can sign up for an account on their [site](https://app.sysdigcloud.com).</span></span>

<span data-ttu-id="83c3e-115">Miután bejelentkezett a Sysdig felhő webhelyére, kattintson a felhasználónevére, és az oldalon meg kell jelennie a hívóbetűjének („Access Key”).</span><span class="sxs-lookup"><span data-stu-id="83c3e-115">Once you're logged in to the Sysdig cloud website, click on your user name, and on the page you should see your "Access Key."</span></span> 

![Sysdig API-kulcs](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-the-sysdig-agents-to-kubernetes"></a><span data-ttu-id="83c3e-117">A Kubernetes a Sysdig ügynökök telepítése</span><span class="sxs-lookup"><span data-stu-id="83c3e-117">Installing the Sysdig agents to Kubernetes</span></span>
<span data-ttu-id="83c3e-118">A tárolók figyeléséhez Sysdig minden gép, egy Kubernetes használatával futtat egy folyamatot `DaemonSet`.</span><span class="sxs-lookup"><span data-stu-id="83c3e-118">To monitor your containers, Sysdig runs a process on each machine using a Kubernetes `DaemonSet`.</span></span>
<span data-ttu-id="83c3e-119">DaemonSets olyan gépenként tárolója egyetlen példányát futtató Kubernetes API-objektumok.</span><span class="sxs-lookup"><span data-stu-id="83c3e-119">DaemonSets are Kubernetes API objects that run a single instance of a container per machine.</span></span>
<span data-ttu-id="83c3e-120">Fontosságúak tökéletes megoldás az eszközök, például a Sysdig figyelési ügynök telepítése.</span><span class="sxs-lookup"><span data-stu-id="83c3e-120">They're perfect for installing tools like the Sysdig's monitoring agent.</span></span>

<span data-ttu-id="83c3e-121">A Sysdig daemonset telepítéséhez kell előbb az [a sablon](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) a sysdig.</span><span class="sxs-lookup"><span data-stu-id="83c3e-121">To install the Sysdig daemonset, you should first download [the template](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) from sysdig.</span></span> <span data-ttu-id="83c3e-122">Mentse a fájlt `sysdig-daemonset.yaml`.</span><span class="sxs-lookup"><span data-stu-id="83c3e-122">Save that file as `sysdig-daemonset.yaml`.</span></span>

<span data-ttu-id="83c3e-123">Linux-és OS X futtathatja:</span><span class="sxs-lookup"><span data-stu-id="83c3e-123">On Linux and OS X you can run:</span></span>

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

<span data-ttu-id="83c3e-124">A PowerShell:</span><span class="sxs-lookup"><span data-stu-id="83c3e-124">In PowerShell:</span></span>

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

<span data-ttu-id="83c3e-125">A hozzáférési kulcsot, Sysdig fiókjából beszerzett fájl mellett szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="83c3e-125">Next edit that file to insert your Access Key, that you obtained from your Sysdig account.</span></span>

<span data-ttu-id="83c3e-126">Végezetül hozza létre a DaemonSet:</span><span class="sxs-lookup"><span data-stu-id="83c3e-126">Finally, create the DaemonSet:</span></span>

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a><span data-ttu-id="83c3e-127">A figyelést megtekintése</span><span class="sxs-lookup"><span data-stu-id="83c3e-127">View your monitoring</span></span>
<span data-ttu-id="83c3e-128">Miután telepített és futó, az ügynökök adatokat vissza Sysdig kell szivattyú.</span><span class="sxs-lookup"><span data-stu-id="83c3e-128">Once installed and running, the agents should pump data back to Sysdig.</span></span>  <span data-ttu-id="83c3e-129">Lépjen vissza a [sysdig irányítópult](https://app.sysdigcloud.com) és megjelenítheti a tárolók kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="83c3e-129">Go back to the [sysdig dashboard](https://app.sysdigcloud.com) and you should see information about your containers.</span></span>

<span data-ttu-id="83c3e-130">Kubernetes-specifikus irányítópultok keresztül is telepíthet a [új irányítópult varázsló](https://app.sysdigcloud.com/#/dashboards/new).</span><span class="sxs-lookup"><span data-stu-id="83c3e-130">You can also install Kubernetes-specific dashboards via the [new dashboard wizard](https://app.sysdigcloud.com/#/dashboards/new).</span></span>
