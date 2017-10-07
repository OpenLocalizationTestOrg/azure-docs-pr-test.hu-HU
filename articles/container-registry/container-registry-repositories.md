---
title: "aaaAzure tároló beállításjegyzék adattárak |} Microsoft Docs"
description: "Hogyan toouse Azure tároló beállításjegyzék tárolóhelyekkel Docker lemezképek"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: cristyg
ms.openlocfilehash: 108622c565e41777fbb1fc9da9a01168abc7a7fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-registry-repositories"></a>Azure-tárolót beállításjegyzék adattárak

Azure-tárolót beállításjegyzék lehetővé teszi a toostore tároló képek tárházak találhatók. Lemezképek tárolása tárházak találhatók, akkor elkülönített környezetben csoportok lemezképet (vagy képeket verzióját). A tárolóhelyekkel is megadhat, amikor képek tooyour beállításjegyzék.


## <a name="prerequisites"></a>Előfeltételek
* **Azure Container Registry** – Létrehozhat egy tároló-beállításjegyzéket Azure-előfizetésében. Például, használja a hello [Azure-portálon](container-registry-get-started-portal.md) vagy hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).
* **A docker parancssori felület** -tooset egy Docker gazdagép és a hozzáférés hello Docker parancssori felület parancsait, mint a helyi számítógép telepítéséhez [Docker-motorhoz](https://docs.docker.com/engine/installation/).
* **A képfájl lekéréses** - lemezkép lekéréses beállításjegyzékből hello nyilvános Docker Hub, címkével, és leküldeni tooyour beállításjegyzék. Útmutatás a leküldéses és lekéréses képek, lásd: [leküldéses Docker kép tooAzure titkos beállításjegyzék](container-registry-get-started-docker-cli.md).


## <a name="viewing-repositories-in-hello-portal"></a>Hello Portal adattárak megtekintése

Lemezképek tooyour tároló beállításjegyzék rendelkezik leküldött, miután hello Azure-portálon hello lemezképeit tároló hello adattárak listáját láthatja.

Ha követte hello hello lépéseit [leküldéses Docker kép tooAzure titkos beállításjegyzék](container-registry-get-started-docker-cli.md) cikk, most rendelkeznie kell egy Nginx-lemezképet a tároló beállításjegyzékben. Hello utasításokat részeként kell megadott hello kép egy névteret. Hello az alábbi példában lévő parancs hello hello NGinx toohello "minták" lemezképtárba leküldéses értesítések:

```
docker push myregistry.azurecr.io/samples/nginx
```
 Az Azure Container Registry támogatja a többszintű adattárnévtereket. Ez a funkció lehetővé teszi, hogy Ön toogroup gyűjtemények képek kapcsolódó tooa adott alkalmazás vagy alkalmazások toospecific fejlesztési vagy működési csapatok gyűjteménye. tooread tároló nyilvántartó, a tárolóhelyekkel kapcsolatos további információkért lásd: [saját Docker-tároló nyilvántartó az Azure-ban](container-registry-intro.md).

tooview hello tároló beállításjegyzék adattárak:

1. Jelentkezzen be toohello Azure-portálon
2. A hello **Azure tároló beállításjegyzék** panelen, jelölje be hello beállításjegyzék tooinspect kívánja
3. Hello beállításjegyzék paneljén kattintson **Tárházak** toosee összes hello tárházak találhatók, és a képek listája
4. (Választható) Egy adott kép toosee címkék kiválasztásához

![A tárolóhelyekkel hello portálon](./media/container-registry-repositories/container-registry-repositories.png)


## <a name="next-steps"></a>Következő lépések
Most, hogy tudja, hogy hello alapjai, készen áll a beállításjegyzékkel toostart áll! Például a tároló képek tooan telepítése start [Azure Tárolószolgáltatás](https://azure.microsoft.com/documentation/services/container-service/) fürt.
