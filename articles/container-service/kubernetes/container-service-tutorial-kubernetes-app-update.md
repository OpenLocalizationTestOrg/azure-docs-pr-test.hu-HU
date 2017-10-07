---
title: "aaaAzure Tárolószolgáltatás oktatóanyag – alkalmazása |} Microsoft Docs"
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
ms.openlocfilehash: c467498bab7952926a18e45ffbb21051a98739d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-an-application-in-kubernetes"></a>Kubernetes tartozó alkalmazás frissítése

Kubernetes az alkalmazás telepítése után azt egy új tároló kép vagy lemezkép verziója lehet frissíteni. Ha egy alkalmazás módosítását, hello frissítés bevezetése elő van készítve, hogy csak egy részét hello telepítési egyidejűleg frissül. A előkészített frissítés lehetővé teszi, hogy a hello alkalmazás tookeep hello frissítés alatt fut, és visszaállítási mechanizmust biztosít, ha a központi telepítési hiba lép fel. 

Ebben az oktatóanyagban hét, hello Azure szavazattal mintaalkalmazás hat részét frissül. Feladatokat, a következők:

> [!div class="checklist"]
> * Hello előtér-alkalmazás kódja frissítése
> * Frissített tároló lemezkép létrehozása
> * Hello tároló kép tooAzure tároló beállításjegyzék küldését
> * Hello frissített tároló rendszerkép központi telepítése

A következő útmutatókból Operations Management Suite konfigurált toomonitor hello Kubernetes fürtön.

## <a name="before-you-begin"></a>Előkészületek

Az előző oktatóanyagokat egy alkalmazás egy tároló lemezképpel csomagolása, hello lemezkép feltöltése tooAzure tároló beállításjegyzék és a létrehozott Kubernetes fürt. hello alkalmazás majd hello Kubernetes fürt volt futtatva. 

Ha még nem fejeződött be az alábbi lépéseket, és szeretné, hogy mentén toofollow, visszaadása túl[oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="update-application"></a>Alkalmazás frissítése

Ebben az oktatóanyagban toocomplete hello lépéseiért kell rendelkeznie klónozott hello Azure szavazattal alkalmazás másolatát. Szükség esetén hozzon létre a klónozott példány hello a következő parancsot:

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Nyissa meg hello `config_file.cfg` bármely kód vagy szöveges szerkesztővel fájlt. Ez a fájl a következő hello klónozott tárház könyvtárában hello található.

```bash
 /azure-voting-app-redis/azure-vote/azure-vote/config_file.cfg
```

Hello értékeinek módosítása `VOTE1VALUE` és `VOTE2VALUE`, majd mentse hello fájlt.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Használjon [docker compose](https://docs.docker.com/compose/) toore-hello előtér-kép és a frissített futtatási hello alkalmazás létrehozása.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up --build -d
```

## <a name="test-application-locally"></a>Alkalmazás helyi teszteléséhez

Keresse meg a túl`http://localhost:8080` toosee hello frissíteni az alkalmazást.

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Címke és leküldéses lemezképek

Címke hello *azure-szavazat-front* hello loginServer hello tároló beállításjegyzék lemezkép.

Azure-tároló beállításjegyzék használata, get hello bejelentkezési kiszolgálónevet hello [az acr lista](/cli/azure/acr#list) parancsot.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Használjon [docker címke](https://docs.docker.com/engine/reference/commandline/tag/) tootag hello kép. Cserélje le `<acrLoginServer>` az Azure-tároló beállításjegyzék bejelentkezési kiszolgáló neve vagy a nyilvános beállításjegyzék állomásnév.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

Használjon [docker leküldéses](https://docs.docker.com/engine/reference/commandline/push/) tooupload hello kép tooyour beállításjegyzék. Cserélje le `<acrLoginServer>` az Azure-tároló beállításjegyzék bejelentkezési kiszolgáló neve vagy a nyilvános beállításjegyzék állomásnév.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a>Frissítés alkalmazás központi telepítése

tooensure maximális hasznos üzemidő hello alkalmazás pod több példánya fut. Ellenőrizze a konfiguráció hello [kubectl beolvasása pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancs.

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

Ha nem rendelkezik több három munkaállomás-csoporttal hello azure-szavazat-első kép fut, méretezhető hello *azure-szavazat-front* központi telepítés.


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

tooupdate hello alkalmazás használatát hello [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) parancsot. Frissítés `<acrLoginServer>` hello bejelentkezési névvel kiszolgáló vagy a gazdagép a tároló beállításjegyzékről.

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

toomonitor hello központi telepítését, használatát hello [kubectl beolvasása pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancsot. Frissített hello alkalmazás lett telepítve, mint a három munkaállomás-csoporttal lezárva, és újból létrehozza hello új tároló lemezképpel.

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

Hello külső IP-cím hello az beszerzése *azure-szavazat-front* szolgáltatás.

```azurecli-interactive
kubectl get service azure-vote-front
```

Tallózás toohello IP cím toosee hello alkalmazás frissítése.

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban egy alkalmazás frissítése, és ez tooa Kubernetes fürt frissítése megkezdődött. befejeződtek a hello a következő feladatokat:

> [!div class="checklist"]
> * Frissített hello előtér-alkalmazás kódja
> * Frissített tároló lemezkép létrehozása
> * Hello tároló kép tooAzure tároló beállításjegyzék leküldött
> * Frissített telepített hello alkalmazás

Előzetes toohello oktatóanyag következő toolearn kapcsolatos toomonitor az Operations Management Suite Kubernetes.

> [!div class="nextstepaction"]
> [A Kubernetes monitorozása az OMS használatával](./container-service-tutorial-kubernetes-monitor.md)