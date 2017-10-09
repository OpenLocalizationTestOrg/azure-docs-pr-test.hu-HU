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
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>A figyelő egy DataDog az Azure Tárolószolgáltatás-fürt

## <a name="prerequisites"></a>Előfeltételek
Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).

Azt is feltételezi, hogy rendelkezik-e hello `az` Azure cli és `kubectl` eszközök vannak telepítve.

Ha hello tesztelheti `az` eszköz futtatásával telepítve:

```console
$ az --version
```

Ha még nem rendelkezik hello `az` eszköz telepítve, az e-mail utasításokat is [Itt](https://github.com/azure/azure-cli#installation).

Ha hello tesztelheti `kubectl` eszköz futtatásával telepítve:

```console
$ kubectl version
```

Ha nem rendelkezik `kubectl` telepítve, futtatható:

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a>DataDog
Datadog egy figyelési szolgáltatás, amely a figyelési adatokat gyűjt a tárolók skálázása az Azure Tárolószolgáltatás-fürt belül. Datadog rendelkezik egy Docker integrációs irányítópultot, ahol láthatja adott mérőszámok belül a tárolók. A tárolók gyűjtött metrikák Processzor, memória, hálózati és i/o szerint vannak rendszerezve. Datadog metrikák felosztja a tárolók és a képeket.

Először túl[-fiók létrehozása](https://www.datadoghq.com/lpg/)

## <a name="installing-hello-datadog-agent-with-a-daemonset"></a>Egy DaemonSet hello Datadog ügynök telepítése
DaemonSets Kubernetes toorun által használt egy tároló minden gazdagépen hello fürt egyetlen példányát.
Fontosságúak tökéletes figyelőügynökök futtatásához.

Miután jelentkezett be Datadog, követésével hello [Datadog utasításokat](https://app.datadoghq.com/account/settings#agent/kubernetes) tooinstall Datadog ügynököt a fürt egy DaemonSet használatával.

## <a name="conclusion"></a>Összegzés
Ennyi az egész! Miután hello ügynökök működik, és fut, akkor kell megjelennie hello adatai néhány perc múlva. Integrált hello ellátogathat [kubernetes irányítópult](https://app.datadoghq.com/screen/integration/kubernetes) toosee a fürt összegzését.
