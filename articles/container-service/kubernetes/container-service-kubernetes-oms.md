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
# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a>A figyelő az Azure Tárolószolgáltatás-fürtöt a Microsoft Operations Management Suite (OMS)

## <a name="prerequisites"></a>Előfeltételek
Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).

Azt is feltételezi, hogy rendelkezik-e hello `az` Azure cli és `kubectl` eszközök vannak telepítve.

Ha hello tesztelheti `az` eszköz futtatásával telepítve:

```console
$ az --version
```

Ha még nem rendelkezik hello `az` eszköz telepítve, az e-mail utasításokat is [Itt](https://github.com/azure/azure-cli#installation).  
Másik megoldásként használhatja [Azure Cloud rendszerhéj](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), hello rendelkező `az` Azure cli és `kubectl` már telepítette a eszközök.  

Ha hello tesztelheti `kubectl` eszköz futtatásával telepítve:

```console
$ kubectl version
```

Ha nem rendelkezik `kubectl` telepítve, futtatható:
```console
$ az acs kubernetes install-cli
```

Ha kubernetes kulcsok a kubectl eszköz telepítve rendelkezik tootest futtathatja:
```console
$ kubectl get nodes
```

Ha hello fent parancs hibák ki, a kubectl eszközt szüksége tooinstall kubernetes fürt kulcsok. Azt a parancs a következő hello teheti meg:
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-operations-management-suite-oms"></a>Az Operations Management Suite (OMS) tárolók figyelése

A Microsoft Operations Management (OMS) a Microsoft felhőalapú informatikai felügyeleti megoldás, amely segít a kezelése és védelme a helyszíni és felhőalapú infrastruktúra. Tároló egy olyan megoldás, az OMS szolgáltatáshoz, így a nézet hello tároló szoftverleltár, a teljesítmény és a naplók egyetlen helyen. Naplózási, tárolók hibaelhárításáról hello naplók megtekintése központi helyen, és zajos fel felesleges tároló egy gazdagépen található.

![](media/container-service-monitoring-oms/image1.png)

A tároló megoldásról további információkért tekintse meg az toothe [tároló megoldás Naplóelemzési](../../log-analytics/log-analytics-containers.md).

## <a name="installing-oms-on-kubernetes"></a>Kubernetes OMS telepítése

### <a name="obtain-your-workspace-id-and-key"></a>Munkaterületének Azonosítóját és kulcs beszerzése
Az OMS ügynökszolgáltatás tootalk toohello hello kell konfigurálni a munkaterület azonosítóját és kulcsát egy toobe. tooget hello munkaterület azonosítója és kulcsa az OMS toocreate van szüksége a fiókot a <https://mms.microsoft.com>. Kövesse az hello lépéseket toocreate fiókkal. Miután a létrehozásakor hello fiók, kell tooobtain a azonosítója és kulcsa kattintva **beállítások**, majd **csatlakoztatott források**, majd **Linux kiszolgálók**lent látható módon.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-hello-oms-agent-using-a-daemonset"></a>Egy DaemonSet használatával hello OMS-ügynök telepítése
DaemonSets Kubernetes toorun által használt egy tároló minden gazdagépen hello fürt egyetlen példányát.
Fontosságúak tökéletes figyelőügynökök futtatásához.

Íme hello [DaemonSet YAM fájl](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Menteni tooa nevű `oms-daemonset.yaml` , és cserélje le a helyőrző értékek hello `WSID` és `KEY` a munkaterület azonosítója és a kulcs hello fájlban.

Miután hozzáadta a munkaterület azonosítója és a kulcs toohello DaemonSet konfigurációs, telepítheti hello OMS-ügynököt a fürt hello `kubectl` parancssori eszköz:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-hello-oms-agent-using-a-kubernetes-secret"></a>Kubernetes titkos kulcs használatával hello OMS-ügynök telepítése
tooprotect az OMS-munkaterület azonosítója és a DaemonSet YAM-fájl részeként is használhatja a Kubernetes titkos kulcs.

 - Hello parancsfájl, titkos sablonfájl és hello DaemonSet YAM fájl másolása (a [tárház](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)), és győződjön meg arról, hogy a hello ugyanabban a könyvtárban. 
      - Titkos kulcs parancsprogram - secret-gen.sh létrehozása
      - titkos sablon - secret-template.yaml
   - DaemonSet YAM fájl - omsagent – ds-secrets.yaml
 - Hello parancsprogrammal. hello parancsfájl kéri hello OMS-munkaterület azonosítója és az elsődleges kulcs. Helyezze be, és hello parancsfájl létrehoz egy titkos yam fájlt, ezért is futtathatja.   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - Hozzon létre hello titkok pod hello következő futtatásával:``` kubectl create -f omsagentsecret.yaml ```
 
   - Futtassa a következő hello toocheck: 

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
 
  - A omsagent futó démon-készlet létrehozása``` kubectl create -f omsagent-ds-secrets.yaml ```

### <a name="conclusion"></a>Összegzés
Ennyi az egész! Néhány perc elteltével tud toosee adatok továbbítására tooyour OMS irányítópult kell lennie.
