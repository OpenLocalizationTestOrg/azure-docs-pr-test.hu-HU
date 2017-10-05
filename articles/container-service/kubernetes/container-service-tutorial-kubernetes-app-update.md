---
title: "Azure Tárolószolgáltatás útmutató - alkalmazása |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató - alkalmazása"
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
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: db580da3e2d70892bc37305394df5be609ebb8a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="update-an-application-in-kubernetes"></a>Kubernetes tartozó alkalmazás frissítése

Kubernetes az alkalmazás telepítése után azt egy új tároló kép vagy lemezkép verziója lehet frissíteni. Ha egy alkalmazás módosítását, a frissítés bevezetése elő van készítve, hogy a központi telepítés egy részének egyidejűleg frissül. A két lépésben frissítés lehetővé teszi, hogy az alkalmazás tovább futnak a frissítés során, és visszaállítási mechanizmust biztosít, ha a központi telepítési hiba lép fel. 

Ebben az oktatóanyagban hét, hat része a mintaalkalmazás Azure szavazattal frissül. Feladatokat, a következők:

> [!div class="checklist"]
> * Az előtér-alkalmazás kódja frissítése
> * Frissített tároló lemezkép létrehozása
> * A tároló kép küldését Azure tároló beállításjegyzék
> * A frissített tároló rendszerkép központi telepítése

A következő útmutatókból Operations Management Suite a Kubernetes fürt figyelésére van konfigurálva.

## <a name="before-you-begin"></a>Előkészületek

Az előző oktatóanyagok egy tároló lemezképet, az Azure-tároló beállításjegyzék feltöltött lemezkép és a létrehozott Kubernetes fürt alkalmazás lett csomagolva. Az alkalmazás ezután futtatták a Kubernetes fürtön. 

Ha még nem fejeződött be az alábbi lépéseket, és követéséhez, térjen vissza [oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="update-application"></a>Alkalmazás frissítése

Az oktatóanyag lépéseinek végrehajtásához, egy példányát az Azure szavazattal alkalmazás rendelkezik klónozása. Szükség esetén hozzon létre a klónozott másolása a következő parancsot:

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Nyissa meg a `config_file.cfg` bármely kód vagy szöveges szerkesztővel fájlt. Ez a fájl, a klónozott tárház a következő könyvtárban található.

```bash
 /azure-voting-app-redis/azure-vote/azure-vote/config_file.cfg
```

Megváltoztatni az `VOTE1VALUE` és `VOTE2VALUE`, majd mentse a fájlt.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Használjon [docker compose](https://docs.docker.com/compose/) hozza létre az előtér-kép és a frissített alkalmazás futtatásához.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up --build -d
```

## <a name="test-application-locally"></a>Alkalmazás helyi teszteléséhez

Keresse meg a `http://localhost:8080` az frissített alkalmazást.

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Címke és leküldéses lemezképek

Címke a *azure-szavazat-front* a tároló beállításjegyzék loginServer lemezkép.

Azure-tároló beállításjegyzék használata, a bejelentkezési kiszolgálónevet beolvasása a [az acr lista](/cli/azure/acr#list) parancsot.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Használjon [docker címke](https://docs.docker.com/engine/reference/commandline/tag/) használatával címkézhesse a lemezképet. Cserélje le `<acrLoginServer>` az Azure-tároló beállításjegyzék bejelentkezési kiszolgáló neve vagy a nyilvános beállításjegyzék állomásnév.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

Használjon [docker leküldéses](https://docs.docker.com/engine/reference/commandline/push/) való feltölti a lemezképet a beállításjegyzékhez. Cserélje le `<acrLoginServer>` az Azure-tároló beállításjegyzék bejelentkezési kiszolgáló neve vagy a nyilvános beállításjegyzék állomásnév.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a>Frissítés alkalmazás központi telepítése

Maximális hasznos üzemidő biztosítása érdekében az alkalmazás fogyasztanak több példánya fut. Ez a konfiguráció ellenőrzése a [kubectl beolvasása pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancsot.

```bash
kubectl get pod
```

Kimenet:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

Ha nem rendelkezik több három munkaállomás-csoporttal fut az azure-szavazat-első kép, méretezhető a *azure-szavazat-front* központi telepítés.


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

Az alkalmazás frissítésére, használja a [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) parancsot. Frissítés `<acrLoginServer>` a tároló beállításjegyzék bejelentkezési kiszolgáló vagy a gazdagép nevével.

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

Az üzemelő példány figyelése a [kubectl beolvasása pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancsot. A frissített alkalmazás lett telepítve, mint a három munkaállomás-csoporttal lezárva, és újból létrehozza az új tároló lemezképpel.

```azurecli-interactive
kubectl get pod
```

Kimenet:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a>Frissített alkalmazás tesztelése

A külső IP-címet az beszerzése a *azure-szavazat-front* szolgáltatás.

```azurecli-interactive
kubectl get service azure-vote-front
```

Tallózással keresse meg az IP-címet a frissített alkalmazás megtekintéséhez.

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban egy alkalmazás frissítése, és a frissítés Kubernetes fürtre megkezdődött. Befejeződtek a következő feladatokat:

> [!div class="checklist"]
> * Frissítve az előtér-alkalmazás kódja
> * Frissített tároló lemezkép létrehozása
> * A tároló kép leküldött Azure tároló beállításjegyzék
> * A frissített alkalmazás telepítve

Továbblépés figyelése az Operations Management Suite Kubernetes olvashat a következő oktatóanyag.

> [!div class="nextstepaction"]
> [A Kubernetes monitorozása az OMS használatával](./container-service-tutorial-kubernetes-monitor.md)