---
title: "aaaPush Docker kép tooprivate Azure beállításjegyzék |} Microsoft Docs"
description: "Leküldéses és lekéréses Docker képek tooa személyes tárolót beállításjegyzék az Azure-ban hello Docker parancssori felületén"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 64fbe43f-fdde-4c17-a39a-d04f2d6d90a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a81a6f4bfcb23642a89ac7631348d40e2f4911a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="push-your-first-image-tooa-private-docker-container-registry-using-hello-docker-cli"></a>Leküldéses az első kép tooa titkos Docker tároló beállításjegyzék hello Docker parancssori felület használatával
Egy Azure-tárolót beállításjegyzék tárolja és kezeli a saját [Docker](http://hub.docker.com) tároló képek, hasonló toohello módon [Docker Hub](https://hub.docker.com/) nyilvános Docker-lemezképet tárolja. Hello használata [Docker parancssori felületet](https://docs.docker.com/engine/reference/commandline/cli/) (a Docker parancssori felületén) a [bejelentkezési](https://docs.docker.com/engine/reference/commandline/login/), [leküldéses](https://docs.docker.com/engine/reference/commandline/push/), [lekéréses](https://docs.docker.com/engine/reference/commandline/pull/), és más műveleteket végez a tároló beállításjegyzék.

További háttér és fogalmak [hello áttekintése](container-registry-intro.md)



## <a name="prerequisites"></a>Előfeltételek
* **Azure Container Registry** – Létrehozhat egy tároló-beállításjegyzéket Azure-előfizetésében. Például, használja a hello [Azure-portálon](container-registry-get-started-portal.md) vagy hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).
* **A docker parancssori felület** -tooset egy Docker gazdagép és a hozzáférés hello Docker parancssori felület parancsait, mint a helyi számítógép telepítéséhez [Docker-motorhoz](https://docs.docker.com/engine/installation/).

## <a name="log-in-tooa-registry"></a>Jelentkezzen be tooa beállításjegyzék
Futtatás `docker login` tooyour tároló beállításjegyzék rendelkező toolog a [beállításjegyzék hitelesítő adatok](container-registry-authentication.md).

hello alábbi példa továbbítja hello Azonosítót és jelszót egy Azure Active Directory [egyszerű](../active-directory/active-directory-application-objects.md). Például előfordulhat, hogy rendelt hozzá egy szolgáltatás egyszerű tooyour beállításjegyzék az automation-forgatókönyv.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

> [!TIP]
> Győződjön meg arról, hogy toospecify hello beállításjegyzék teljesen minősített neve (összes, kisbetű). Ebben a példában ez `myregistry.azurecr.io`.

## <a name="steps-toopull-and-push-an-image"></a>Lépéseket toopull és leküldéses kép
hello letöltések hello Nginx kép beállításjegyzékből hello nyilvános Docker Hub, azt a saját Azure-tárolót beállításjegyzék leküldéses értesítések tooyour beállításjegyzék, majd kéri le újra címkék példa kövesse.

**1. Az Nginx hello Docker hivatalos kép lekéréses**

Első lekéréses hello nyilvános Nginx kép tooyour helyi számítógépen.

```
docker pull nginx
```
**2. Indítsa el a hello Nginx tároló**

hello következő indítja el a hello helyi Nginx tároló interaktív porton 8080-as, így Nginx toosee kimenetét. Eltávolítja a hello fut a tárolót egyszer leállt.

```
docker run -it --rm -p 8080:80 nginx
```

Keresse meg a túl[8080](http://localhost:8080) tooview hello futtató tároló. Megjelenik egy egyet a következő képernyő hasonló toohello.

![Nginx egy helyi számítógépen](./media/container-registry-get-started-docker-cli/nginx.png)

toostop futó tároló hello megadásához nyomja meg a [CTRL] + [C].

**3. A beállításjegyzék-alias hello lemezkép létrehozása**

hello parancsban hoz létre hello lemezkép alias teljes elérési útja tooyour beállításjegyzékbeli. Ez a példa meghatározza, hogy hello `samples` névtér tooavoid zsúfoltságát hello beállításjegyzék hello gyökérmappájában.

```
docker tag nginx myregistry.azurecr.io/samples/nginx
```  

**4. Leküldéses hello kép tooyour beállításjegyzék**

```
docker push myregistry.azurecr.io/samples/nginx
```

**5. A beállításjegyzék lekéréses hello lemezkép**

```
docker pull myregistry.azurecr.io/samples/nginx
```

**6. A beállításjegyzék-tól kezdődnek hello Nginx tároló**

```
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Keresse meg a túl[8080](http://localhost:8080) tooview hello futtató tároló.

toostop futó tároló hello megadásához nyomja meg a [CTRL] + [C].

**7. (Választható) Hello kép eltávolítása**

```
docker rmi myregistry.azurecr.io/samples/nginx
```

##<a name="concurrent-limits"></a>Egyidejű korlátok
Bizonyos forgatókönyvekben a párhuzamos hívások végrehajtása hibát eredményezhet. a következő táblázat hello hello korlátok számos egyidejű hívás az Azure-tárolót beállításjegyzékbeli "Leküldéses" és "Pull" műveleteket tartalmazza:

| Művelet  | Korlát                                  |
| ---------- | -------------------------------------- |
| LEKÉRÉS       | Egyidejű too10 mentése kéri le. egy beállításjegyzék |
| LEKÜLDÉSES ÉRTESÍTÉSEK       | Egyidejű too5 be leküldéses értesítések beállításjegyzék száma |

## <a name="next-steps"></a>Következő lépések
Most, hogy tudja, hogy hello alapjai, készen áll a beállításjegyzékkel toostart áll! Például a tároló képek tooan telepítése start [Azure Tárolószolgáltatás](https://azure.microsoft.com/documentation/services/container-service/) fürt.
