---
title: "Azure Kubernetes-fürtöt aaaMonitor Datadog |} Microsoft Docs"
description: "Figyelési Kubernetes Datadog használata az Azure Tárolószolgáltatás-fürt"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: bccd8b59a048e0f791172fcfc4eeba6370dafcc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a><span data-ttu-id="93cb3-103">A figyelő egy DataDog az Azure Tárolószolgáltatás-fürt</span><span class="sxs-lookup"><span data-stu-id="93cb3-103">Monitor an Azure Container Service cluster with DataDog</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93cb3-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="93cb3-104">Prerequisites</span></span>
<span data-ttu-id="93cb3-105">Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="93cb3-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="93cb3-106">Azt is feltételezi, hogy rendelkezik-e hello `az` Azure cli és `kubectl` eszközök vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="93cb3-106">It also assumes that you have hello `az` Azure cli and `kubectl` tools installed.</span></span>

<span data-ttu-id="93cb3-107">Ha hello tesztelheti `az` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="93cb3-107">You can test if you have hello `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="93cb3-108">Ha még nem rendelkezik hello `az` eszköz telepítve, az e-mail utasításokat is [Itt](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="93cb3-108">If you don't have hello `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="93cb3-109">Ha hello tesztelheti `kubectl` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="93cb3-109">You can test if you have hello `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="93cb3-110">Ha nem rendelkezik `kubectl` telepítve, futtatható:</span><span class="sxs-lookup"><span data-stu-id="93cb3-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a><span data-ttu-id="93cb3-111">DataDog</span><span class="sxs-lookup"><span data-stu-id="93cb3-111">DataDog</span></span>
<span data-ttu-id="93cb3-112">Datadog egy figyelési szolgáltatás, amely a figyelési adatokat gyűjt a tárolók skálázása az Azure Tárolószolgáltatás-fürt belül.</span><span class="sxs-lookup"><span data-stu-id="93cb3-112">Datadog is a monitoring service that gathers monitoring data from your containers within your Azure Container Service cluster.</span></span> <span data-ttu-id="93cb3-113">Datadog rendelkezik egy Docker integrációs irányítópultot, ahol láthatja adott mérőszámok belül a tárolók.</span><span class="sxs-lookup"><span data-stu-id="93cb3-113">Datadog has a Docker Integration Dashboard where you can see specific metrics within your containers.</span></span> <span data-ttu-id="93cb3-114">A tárolók gyűjtött metrikák Processzor, memória, hálózati és i/o szerint vannak rendszerezve.</span><span class="sxs-lookup"><span data-stu-id="93cb3-114">Metrics gathered from your containers are organized by CPU, Memory, Network and I/O.</span></span> <span data-ttu-id="93cb3-115">Datadog metrikák felosztja a tárolók és a képeket.</span><span class="sxs-lookup"><span data-stu-id="93cb3-115">Datadog splits metrics into containers and images.</span></span>

<span data-ttu-id="93cb3-116">Először túl[-fiók létrehozása](https://www.datadoghq.com/lpg/)</span><span class="sxs-lookup"><span data-stu-id="93cb3-116">You first need too[create an account](https://www.datadoghq.com/lpg/)</span></span>

## <a name="installing-hello-datadog-agent-with-a-daemonset"></a><span data-ttu-id="93cb3-117">Egy DaemonSet hello Datadog ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="93cb3-117">Installing hello Datadog Agent with a DaemonSet</span></span>
<span data-ttu-id="93cb3-118">DaemonSets Kubernetes toorun által használt egy tároló minden gazdagépen hello fürt egyetlen példányát.</span><span class="sxs-lookup"><span data-stu-id="93cb3-118">DaemonSets are used by Kubernetes toorun a single instance of a container on each host in hello cluster.</span></span>
<span data-ttu-id="93cb3-119">Fontosságúak tökéletes figyelőügynökök futtatásához.</span><span class="sxs-lookup"><span data-stu-id="93cb3-119">They're perfect for running monitoring agents.</span></span>

<span data-ttu-id="93cb3-120">Miután jelentkezett be Datadog, követésével hello [Datadog utasításokat](https://app.datadoghq.com/account/settings#agent/kubernetes) tooinstall Datadog ügynököt a fürt egy DaemonSet használatával.</span><span class="sxs-lookup"><span data-stu-id="93cb3-120">Once you have logged into Datadog, you can follow hello [Datadog instructions](https://app.datadoghq.com/account/settings#agent/kubernetes) tooinstall Datadog agents on your cluster using a DaemonSet.</span></span>

## <a name="conclusion"></a><span data-ttu-id="93cb3-121">Összegzés</span><span class="sxs-lookup"><span data-stu-id="93cb3-121">Conclusion</span></span>
<span data-ttu-id="93cb3-122">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="93cb3-122">That's it!</span></span> <span data-ttu-id="93cb3-123">Miután hello ügynökök működik, és fut, akkor kell megjelennie hello adatai néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="93cb3-123">Once hello agents are up and running you should see data in hello console in a few minutes.</span></span> <span data-ttu-id="93cb3-124">Integrált hello ellátogathat [kubernetes irányítópult](https://app.datadoghq.com/screen/integration/kubernetes) toosee a fürt összegzését.</span><span class="sxs-lookup"><span data-stu-id="93cb3-124">You can visit hello integrated [kubernetes dashboard](https://app.datadoghq.com/screen/integration/kubernetes) toosee a summary of your cluster.</span></span>
