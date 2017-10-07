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
# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a>Egy Azure-tároló szolgáltatás Kubernetes fürtjéhez Sysdig figyelése

## <a name="prerequisites"></a>Előfeltételek
Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).

Azt is feltételezi, hogy rendelkezik-e hello azure cli és kubectl eszközök vannak telepítve.

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

## <a name="sysdig"></a>Sysdig
Sysdig egy külső "figyelés" a szolgáltatás vállalat, amely képes figyelni az Azure-beli Kubernetes fürt tárolók. Aktív Sysdig fiók Sysdig a használatához.
Iratkozzon fel a fiókot saját [hely](https://app.sysdigcloud.com).

Miután toohello Sysdig felhő webhelyen jelentkezett, kattintson a felhasználónevére, és hello oldalon kell megjelennie a "hozzáférési kulcsot." 

![Sysdig API-kulcs](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-hello-sysdig-agents-tookubernetes"></a>Hello Sysdig ügynökök tooKubernetes telepítése
a tárolók Sysdig fut a folyamat minden egyes számítógép segítségével egy Kubernetes toomonitor `DaemonSet`.
DaemonSets olyan gépenként tárolója egyetlen példányát futtató Kubernetes API-objektumok.
Fontosságúak tökéletes megoldás az eszközök, például hello Sysdig tartozó figyelési ügynök telepítése.

először letöltse tooinstall hello Sysdig daemonset [hello sablon](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) a sysdig. Mentse a fájlt `sysdig-daemonset.yaml`.

Linux-és OS X futtathatja:

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

A PowerShell:

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

A hozzáférési kulcs, Sysdig fiókjából beszerzett mellett szerkesztése az adott fájl tooinsert.

Végezetül hozza létre a hello DaemonSet:

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a>A figyelést megtekintése
Miután telepített és futó, hello ügynökök adatokat hátsó tooSysdig kell szivattyú.  Lépjen vissza toothe [sysdig irányítópult](https://app.sysdigcloud.com) és megjelenítheti a tárolók kapcsolatos információkat.

Kubernetes-specifikus irányítópultok keresztül is telepíthet a [új irányítópult varázsló](https://app.sysdigcloud.com/#/dashboards/new).
