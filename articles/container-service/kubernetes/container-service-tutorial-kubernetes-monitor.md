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
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a>A figyelő az Operations Management Suite Kubernetes fürt

A Kubernetes fürt és a tárolók figyelése fontos, különösen akkor, ha több alkalmazáshoz léptékű termelési fürt felügyeletére. 

Kihasználhatja a több Kubernetes figyelési megoldás, a Microsoft vagy más szolgáltatók. Ebben az oktatóanyagban hello tárolók megoldás használatával figyelheti a Kubernetes fürt [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), a Microsoft felhőalapú informatikai felügyeleti megoldás. (OMS-tárolók megoldás hello előzetes verzió van.)

Ebben az oktatóanyagban hét részét hét, a következő feladatok hello foglalkozik:

> [!div class="checklist"]
> * OMS-munkaterület beállításainak beolvasása
> * Állítsa be az OMS-ügynököt a hello Kubernetes csomópontok
> * Elérni a figyelési adatait hello OMS-portálon vagy az Azure-portálon

## <a name="before-you-begin"></a>Előkészületek

Előző oktatóanyagokat egy alkalmazás a tároló képek csomagolása, ezeket a lemezképeket tároló beállításjegyzék tooAzure, és létre Kubernetes fürt feltöltve. Ha nem volna ezeket a lépéseket, és szeretné mentén toofollow, visszaadása túl[oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md). 

Legalább az oktatóanyag egy Kubernetes fürt Linux ügynök-csomópontok és az Operations Management Suite (OMS) fiók szükséges. Ha szükséges, regisztráljon egy [OMS ingyenes](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).

## <a name="get-workspace-settings"></a>Munkaterület beállításainak beolvasása

Hello elérésekor [OMS-portálon](https://mms.microsoft.com), nyissa meg túl**beállítások** > **csatlakoztatott források** > **Linux kiszolgálók**. Itt megtalálhatja hello *munkaterület azonosítója* és elsődleges vagy másodlagos *Munkaterületkulcsot*. Jegyezze fel ezeket az értékeket, amely szükséges az OMS-ügynököt a hello fürt mentése tooset.

## <a name="set-up-oms-agents"></a>Állítsa be az OMS-ügynökök

Íme egy YAM fájl tooset mentése hello Linux fürtcsomópontokon OMS-ügynököt. Létrehoz egy Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), amely egy egyetlen azonos pod fut a fürt minden csomópontján. hello DaemonSet erőforrás figyelési ügynök telepítéséhez ideális. 

A következő nevű tooa szövegfájl hello mentése `oms-daemonset.yaml`, és cserélje le a helyőrző értékeket hello *myWorkspaceID* és *myWorkspaceKey* az OMS-munkaterület azonosítója és kulcsa. (A termelési, is kódol ezeket az értékeket, a titkos kulcsok.)

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

Hozzon létre hello DaemonSet hello a következő parancsot:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

toosee DaemonSet jön létre, hogy hello futtassa:

```azurecli-interactive
kubectl get daemonset
```

Hasonló toohello következő kimenete:

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

Miután hello ügynökök futnak, OMS tooingest és a folyamat hello adatok több percet vesz igénybe.

## <a name="access-monitoring-data"></a>Figyelési adatok hozzáférés

Megtekintése és elemzése hello OMS-tárolófigyelő hello adatok [tároló megoldás](../../log-analytics/log-analytics-containers.md) hello OMS-portálon vagy hello Azure-portálon. 

hello használó tooinstall hello tároló megoldás [OMS-portálon](https://mms.microsoft.com), nyissa meg túl**megoldások gyűjtemény**. Majd adja hozzá **tároló megoldás**. Másik lehetőségként adja hozzá hello tárolók megoldás hello [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).

Az OMS-portálon hello, keresse meg a **tárolók** hello OMS irányítópult összefoglalás csempére. Hello csempe, beleértve a részletekért kattintson: tároló események, a hibák, a állapota, a kép leltározás és a Processzor- és memóriahasználatról. Részletesebb információkért kattintson egy sorra a bármely csempére, vagy hajtsa végre a [naplófájl-keresési](../../log-analytics/log-analytics-log-searches.md).

![Az OMS-portálon tárolók irányítópult](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Hasonlóképpen, a hello Azure-portálon, váltson túl**Naplóelemzési** , és válassza ki a munkaterület nevét. toosee hello **tárolók** összefoglalás csempére, kattintson a **megoldások** > **tárolók**. toosee részleteit, kattintson a hello csempére.

Lásd: hello [Azure Log Analytics-dokumentáció](../../log-analytics/index.md) kérdez le, és a figyelési adatok elemzése részletes útmutatást.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban az OMS Kubernetes fürt figyeli. Feladatok kezelt tartalmazza:

> [!div class="checklist"]
> * OMS-munkaterület beállításainak beolvasása
> * Állítsa be az OMS-ügynököt a hello Kubernetes csomópontok
> * Elérni a figyelési adatait hello OMS-portálon vagy az Azure-portálon


Kövesse a hivatkozást toosee előzetesen elkészített parancsfájl-mintában a tároló szolgáltatás.

> [!div class="nextstepaction"]
> [Azure Tárolószolgáltatás-parancsfájl minták](cli-samples.md)
