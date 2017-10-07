---
title: "aaaAzure Tárolószolgáltatás oktatóanyag – Scale Application |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató – Scale Application"
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
ms.openlocfilehash: 29571eef0fd91bd6b40f00d8c018539f320179bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-kubernetes-pods-and-kubernetes-infrastructure"></a>Skála Kubernetes három munkaállomás-csoporttal és Kubernetes infrastruktúra

Ha korábban már következő hello oktatóprogramok, működő Kubernetes fürt rendelkezik az Azure Tárolószolgáltatásban, és hello Azure szavazás alkalmazást telepítette. 

Ebben az oktatóanyagban öt részét hét, horizontális felskálázás hello három munkaállomás-csoporttal hello alkalmazásban, majd próbálja pod automatikus skálázást. Azt is megtudhatja, hogyan tooscale hello számú Azure virtuális gép ügynök csomópontok toochange hello fürt kapacitása munkaterhelések. Befejeződött a következő feladatokból áll:

> [!div class="checklist"]
> * Manuális skálázás Kubernetes három munkaállomás-csoporttal
> * Automatikus skálázás három munkaállomás-csoporttal hello alkalmazás előtér futtató konfigurálása
> * Hello Kubernetes Azure ügynök csomópontok méretezése

A következő útmutatókból hello Azure szavazattal alkalmazás frissül, és az Operations Management Suite konfigurált toomonitor hello Kubernetes fürt.

## <a name="before-you-begin"></a>Előkészületek

Az előző oktatóanyagokat egy alkalmazás egy tároló lemezképpel csomagolása, ez a Rendszerkép feltöltése tooAzure tároló beállításjegyzék és a létrehozott Kubernetes fürt. hello alkalmazás majd hello Kubernetes fürt volt futtatva. Ha nem volna ezeket a lépéseket, és szeretné mentén toofollow, térjen vissza a toohello [oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md). 

Legalább az oktatóanyag a Kubernetes fürtben futó alkalmazás van szükség.

## <a name="manually-scale-pods"></a>Manuálisan méretezhető, három munkaállomás-csoporttal

Így sokkal hello Azure szavazattal előtér- és Redis példánya van telepítve, az egy replikához. tooverify, futtassa a hello [kubectl beolvasása](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancsot.

```azurecli-interactive
kubectl get pods
```

Kimenet:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Manuálisan módosítja a hello három munkaállomás-csoporttal hello száma `azure-vote-front` állításra hello [kubectl méretezési](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) parancsot. Ez a példa hello számú too5 növeli.

```azurecli-interactive
kubectl scale --replicas=5 deployment/azure-vote-front
```

Futtatás [kubectl első három munkaállomás-csoporttal](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) tooverify, hogy Kubernetes hoz létre a hello három munkaállomás-csoporttal. Egy perc vagy tette, után hello további három munkaállomás-csoporttal használja:

```azurecli-interactive
kubectl get pods
```

Kimenet:

```bash
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Automatikus skálázás három munkaállomás-csoporttal

Támogatja a Kubernetes [vízszintes pod automatikus skálázás](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) tooadjust hello száma attól függően, hogy a CPU-felhasználás, vagy más központi telepítés három munkaállomás-csoporttal kiválaszthatja a metrikákat. 

toouse hello autoscaler, a három munkaállomás-csoporttal kell rendelkeznie, CPU-kérelmek és meghatározott korlátozásokat. A hello `azure-vote-front` központi telepítését, előtér-tároló kérelmek 0,25 Processzor 0,5 legfeljebb hello Processzor. hello beállításokat a következőhöz hasonló:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

hello alábbi példában hello [kubectl automatikus skálázás](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) tooautoscale hello száma a hello három munkaállomás-csoporttal parancs `azure-vote-front` központi telepítés. Itt Ha a CPU-felhasználás meghaladja az 50 %-os, hello autoscaler hello három munkaállomás-csoporttal tooa legfeljebb 10 növeli.


```azurecli-interactive
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

hello autoscaler, futtassa a következő parancs hello toosee hello állapota:

```azurecli-interactive
kubectl get hpa
```

Kimenet:

```bash
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Néhány perc elteltével hello Azure szavazattal alkalmazást, és minimális terhelése hello pod replikák száma csökken automatikusan too3.

## <a name="scale-hello-agents"></a>Skála hello ügynökök

Ha az alapértelmezett parancsokkal hello előző oktatóprogram Kubernetes fürt hozott létre, három ügynök csomópontot tartalmaz. Ha a fürt több vagy kevesebb tároló munkaterhelések, manuálisan módosíthatja hello ügynökök száma. Használjon hello [az acs méretezése](/cli/azure/acs#scale) parancsot, és adja meg a hello ügynökök száma az hello `--new-agent-count` paraméter.

hello alábbi példa növeli az ügynök csomópontok too4 hello Kubernetes fürt nevű hello száma *myK8sCluster*. hello parancs néhány perc toocomplete vesz igénybe.

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

hello parancs kimenetét mutatja hello számát ügynök csomópontok hello értékének `agentPoolProfiles:count`:

```azurecli
{
  "agentPoolProfiles": [
    {
      "count": 4,
      "dnsPrefix": "myK8SCluster-myK8SCluster-e44f25-k8s-agents",
      "fqdn": "",
      "name": "agentpools",
      "vmSize": "Standard_D2_v2"
    }
  ],
...

```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban használt különböző méretezési szolgáltatásait a Kubernetes fürt. Feladatok kezelt tartalmazza:

> [!div class="checklist"]
> * Manuális skálázás Kubernetes három munkaállomás-csoporttal
> * Automatikus skálázás három munkaállomás-csoporttal hello alkalmazás előtér futtató konfigurálása
> * Hello Kubernetes Azure ügynök csomópontok méretezése

Előzetes toohello oktatóanyag következő toolearn Kubernetes alkalmazás frissítésével kapcsolatban.

> [!div class="nextstepaction"]
> [Kubernetes tartozó alkalmazás frissítése](./container-service-tutorial-kubernetes-app-update.md)

