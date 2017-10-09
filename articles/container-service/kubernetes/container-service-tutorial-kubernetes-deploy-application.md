---
title: "aaaAzure Tárolószolgáltatás oktatóanyag - alkalmazás központi telepítése |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató - alkalmazás központi telepítése"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7e2fa06d359caf83e684df3966624a6e9a8e7efa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-applications-in-kubernetes"></a>A Kubernetes alkalmazások futtatásához

Ebben az oktatóanyagban négy hét része, egy minta-alkalmazás központi telepítése egy Kubernetes fürtbe. Befejeződött a lépések az alábbiak:

> [!div class="checklist"]
> * Kubernetes fájlok letöltése
> * Futtatja az alkalmazást Kubernetes
> * Hello alkalmazás tesztelése

A következő útmutatókból az alkalmazás van méretezhető, frissítése és az Operations Management Suite konfigurált toomonitor hello Kubernetes fürt.

Ez az oktatóanyag azt feltételezi, hogy alapszinten megértse, Kubernetes fogalmakat, Kubernetes részletes információkat lásd: hello [Kubernetes dokumentáció](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Előkészületek

Az előző oktatóanyagokat egy alkalmazás egy tároló lemezképpel csomagolása, a lemezkép: feltöltött tooAzure tároló beállításjegyzék és Kubernetes fürt létrejött. Ha nem volna ezeket a lépéseket, és szeretné mentén toofollow, visszaadása túl[oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md). 

Legalább az oktatóanyag egy Kubernetes fürt szükséges.

## <a name="get-manifest-file"></a>A jegyzékfájlnak beolvasása

Ebben az oktatóanyagban [Kubernetes objektumok](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) Kubernetes jegyzékfájl vannak telepítve. A Kubernetes jegyzék Kubernetes objektum központi telepítési és konfigurálási utasításait tartalmazó YAM vagy JSON formátumú fájl.

hello Alkalmazásjegyzék-fájl az oktatóanyag előző oktatóprogram klónozása megtörtént hello Azure szavazattal alkalmazás tárházban érhető el. Ha még nem tette meg, klónozza a hello tárház a hello a következő parancsot: 

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

hello jegyzékfájl hello következő hello klónozott tárház könyvtárában található.

```bash
/azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

## <a name="update-manifest-file"></a>A jegyzékfájl frissítése

Ha Azure-tároló beállításjegyzék toostore hello tároló lemezképek használatával, hello jegyzék igények toobe frissített hello ACR loginServer neve.

Első hello ACR bejelentkezési kiszolgálónevet hello [az acr lista](/cli/azure/acr#list) parancsot.

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

hello minta jegyzékfájl már korábban létrehozott tárházba nevű *microsoft*. Nyissa meg szövegszerkesztőben hello fájlt, és cserélje le a hello *microsoft* hello bejelentkezési kiszolgálónevet a ACR példány érték.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

## <a name="deploy-application"></a>Alkalmazás központi telepítése

Használjon hello [kubectl létrehozása](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) toorun hello alkalmazás parancsot. Ez a parancs elemez hello manifest fájlt, és meghatározott hello Kubernetes objektumok létrehozása.

```azurecli-interactive
kubectl create -f ./azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

Kimenet:

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Alkalmazás tesztelése

A [Kubernetes szolgáltatás](https://kubernetes.io/docs/concepts/services-networking/service/) jön létre, amely közzétesz hello alkalmazás toohello internet. A folyamat eltarthat néhány percig. 

toomonitor folyamatban, használjon hello [kubectl beolvasása szolgáltatás](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) hello parancsot `--watch` argumentum.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Kezdetben hello **külső IP-** a hello *azure-szavazat-front* szolgáltatás jelenik meg *függőben lévő*. Miután hello külső IP-cím megváltozott *függőben lévő* tooan *IP-cím*, használjon `CTRL-C` toostop hello kubectl figyelési folyamat.

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

toosee hello alkalmazás, Tallózás toohello külső IP-címet.

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello Azure szavazattal alkalmazás telepített tooan Azure tároló szolgáltatás Kubernetes fürt volt. Befejeződött a következő feladatokból áll:  

> [!div class="checklist"]
> * Kubernetes fájlok letöltése
> * Kubernetes hello alkalmazást futtat
> * Tesztelt hello alkalmazás

Előzetes toohello oktatóanyag következő toolearn egy Kubernetes alkalmazás és az alapjául szolgáló Kubernetes infrastruktúra hello méretezésével kapcsolatos. 

> [!div class="nextstepaction"]
> [Skála Kubernetes alkalmazás- és infrastruktúra](./container-service-tutorial-kubernetes-scale.md)