---
title: "Az Azure útmutató - alkalmazása Kubernetes"
description: "AKS oktatóanyag - alkalmazása"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6de5173aedc836f7a2d56370ea8e54ad6e77ab5e
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/18/2017
---
# <a name="update-an-application-in-azure-container-service-aks"></a>Az alkalmazás Azure tároló szolgáltatás (AKS) frissítése

Után az alkalmazás telepítve lett Kubernetes, akkor egy új tároló kép vagy lemezkép verziója lehet frissíteni. Annak során, a frissítés tartalma, hogy a központi telepítés egy részének egyidejűleg frissül. A két lépésben frissítés lehetővé teszi, hogy az alkalmazás tovább futnak a frissítés során. Azt is lehetővé teszi a visszaállítási központi telepítési hiba esetén. 

Ebben az oktatóanyagban nyolc, hat része a mintaalkalmazás Azure szavazattal frissül. Feladatokat, a következők:

> [!div class="checklist"]
> * Az előtér-alkalmazás kódja frissítése
> * Frissített tároló lemezkép létrehozása
> * A tároló kép küldését Azure tároló beállításjegyzék
> * A frissített tároló rendszerkép központi telepítése

A következő útmutatókból Operations Management Suite a Kubernetes fürt figyelésére van konfigurálva.

## <a name="before-you-begin"></a>Előkészületek

Az előző oktatóanyagok egy tároló lemezképet, az Azure-tároló beállításjegyzék feltöltött lemezkép és a létrehozott Kubernetes fürt alkalmazás lett csomagolva. Az alkalmazás ezután futtatták a Kubernetes fürtön. 

Egy alkalmazás tárház lett is klónozott ide tartozik az alkalmazás forráskódjához, és ebben az oktatóanyagban használt előre létrehozott Docker Compose fájl. Győződjön meg arról, hogy létrehozta a tárház másolat, és hogy módosult-könyvtárak a klónozott könyvtárba. Belső út egy könyvtár nevű `azure-vote` és nevű fájlt `docker-compose.yaml`.

Ha még nem fejeződött be az alábbi lépéseket, és követéséhez, térjen vissza [oktatóanyag 1 – létrehozás tároló képek][aks-tutorial-prepare-app]. 

## <a name="update-application"></a>Alkalmazás frissítése

Ebben az oktatóanyagban egy módosításakor az alkalmazást, és a Kubernetes fürt központi telepítése a frissített alkalmazás. 

Az alkalmazás forráskódjához található található a `azure-vote` könyvtár. Nyissa meg a `config_file.cfg` bármely kód vagy szöveges szerkesztővel fájlt. Ebben a példában `vi` szolgál.

```console
vi azure-vote/azure-vote/config_file.cfg
```

Megváltoztatni az `VOTE1VALUE` és `VOTE2VALUE`, majd mentse a fájlt.

```console
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Mentse és zárja be a fájlt.

## <a name="update-container-image"></a>Tároló lemezképek

Használjon [docker compose] [ docker-compose] hozza létre az előtér-kép és a frissített alkalmazás futtatásához. A `--build` argumentum használata az utasítási kiadása a Docker Compose újra létre kell hoznia az alkalmazás-lemezképet.

```console
docker-compose up --build -d
```

## <a name="test-application-locally"></a>Alkalmazás helyi teszteléséhez

Tallózással keresse meg az frissített alkalmazást 8080.

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Címke és leküldéses lemezképek

Címke a `azure-vote-front` a tároló beállításjegyzék loginServer lemezkép. 

A bejelentkezési kiszolgáló nevét, a [az acr lista](/cli/azure/acr#list) parancsot.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Használjon [docker címke] [ docker-tag] használatával címkézhesse a lemezképet. Cserélje le `<acrLoginServer>` az Azure-tároló beállításjegyzék bejelentkezési kiszolgáló neve vagy a nyilvános beállításjegyzék állomásnév. Is figyelje meg, hogy a kép verzióra frissül, és `redis-v2`.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

Használjon [docker leküldéses] [ docker-push] való feltölti a lemezképet a beállításjegyzékhez. Cserélje le `<acrLoginServer>` az Azure-tároló beállításjegyzék bejelentkezési kiszolgáló nevével.

```console
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a>Frissítés alkalmazás központi telepítése

Maximális hasznos üzemidő biztosítása érdekében az alkalmazás fogyasztanak több példánya fut. Ez a konfiguráció ellenőrzése a [kubectl beolvasása pod] [ kubectl-get] parancsot.

```
kubectl get pod
```

Kimenet:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

Ha nem rendelkezik több három munkaállomás-csoporttal fut az azure-szavazat-első kép, méretezni a `azure-vote-front` központi telepítés.


```azurecli
kubectl scale --replicas=3 deployment/azure-vote-front
```

Az alkalmazás frissítésére, használja a [kubectl set] [ kubectl-set] parancsot. Frissítés `<acrLoginServer>` a tároló beállításjegyzék bejelentkezési kiszolgáló vagy a gazdagép nevével.

```azurecli
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

Az üzemelő példány figyelése a [kubectl beolvasása pod] [ kubectl-get] parancsot. A frissített alkalmazás lett telepítve, mint a három munkaállomás-csoporttal lezárva, és újból létrehozza az új tároló lemezképpel.

```azurecli
kubectl get pod
```

Kimenet:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a>Frissített alkalmazás tesztelése

A külső IP-címet az beszerzése a `azure-vote-front` szolgáltatás.

```azurecli
kubectl get service azure-vote-front
```

Tallózással keresse meg az IP-címet a frissített alkalmazás megtekintéséhez.

![Egy Azure-beli Kubernetes-fürt képe](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>További lépések

Ebben az oktatóanyagban egy alkalmazás frissítése, és a frissítés Kubernetes fürtre megkezdődött. Befejeződtek a következő feladatokat:

> [!div class="checklist"]
> * Frissítve az előtér-alkalmazás kódja
> * Frissített tároló lemezkép létrehozása
> * A tároló kép leküldött Azure tároló beállításjegyzék
> * A frissített alkalmazás telepítve

Továbblépés figyelése az Operations Management Suite Kubernetes olvashat a következő oktatóanyag.

> [!div class="nextstepaction"]
> [A Naplóelemzési figyelő Kubernetes][aks-tutorial-monitor]

<!-- LINKS - external -->
[docker-compose]: https://docs.docker.com/compose/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-set]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#set

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-monitor]: ./tutorial-kubernetes-monitor.md