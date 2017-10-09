---
title: "aaaPrivate Docker-tároló nyilvántartó az Azure-ban |} Microsoft Docs"
description: "Bevezetés toohello Azure tároló beállításjegyzék szolgáltatás felhőalapú, felügyelt, személyes Docker nyilvántartó biztosítása."
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: ee2b652b-fb7c-455b-8275-b8d4d08ffeb3
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f6edcf0bf947b7770ee0a4e4a5cfbf4ef8b7392a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooprivate-docker-container-registries"></a>Bevezetés tooprivate Docker-tároló nyilvántartó


Az Azure tároló beállításjegyzék egy felügyelt [Docker beállításjegyzék](https://docs.docker.com/registry/) szolgáltatáson hello nyílt forráskódú Docker beállításjegyzék 2.0. Hozzon létre és Azure-tárolót nyilvántartó toostore kezelheti és a saját [Docker-tároló](https://www.docker.com/what-docker) képek. A meglévő tárolóhoz fejlesztési és telepítési folyamatok tároló nyilvántartó az Azure-ban használjon, és megrajzolja a Docker közösségi szakértői hello törzsét.

A Dockerrel és a tárolókkal kapcsolatos háttérinformációk:

* [Docker felhasználói útmutató](https://docs.docker.com/engine/userguide/)




## <a name="use-cases"></a>Használati esetek
Lekéréses képek egy Azure-tárolót a beállításjegyzék toovarious telepítési célok:

* **Méretezhető előkészítési rendszerek**, amelyek tárolóalapú alkalmazásokat kezelnek gazdagépfürtökben (többek között [DC/OS](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/) és [Kubernetes](http://kubernetes.io/docs/)).
* **Azure-szolgáltatások**, amelyek támogatják az alkalmazások építését és nagy mennyiségű alkalmazás futtatását, beleértve a [Container Service](../container-service/index.yml), az [App Service](/app-service/index.md), a [Batch](../batch/index.md), a [Service Fabric](/azure/service-fabric/) és egyéb szolgáltatásokat.

A fejlesztők is tolható tooa tároló beállításjegyzék tároló fejlesztési munkafolyamat részeként. Például megcélozhat egy tároló-beállításjegyzéket egy olyan folyamatos integrációs és üzembe helyezési eszközből, mint a [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) vagy a [Jenkins](https://jenkins.io/).





## <a name="key-concepts"></a>Fő fogalmak
* **Beállításjegyzék** – Létrehozhat egy vagy több tároló-beállításjegyzéket Azure-előfizetésében. Egy szabványos Azure által támogatott minden egyes beállításjegyzék [tárfiók](../storage/common/storage-introduction.md) hello az ugyanazon a helyen. Ha létrehoz egy beállításjegyzék hello helyi, a hálózati zárja be a tároló képek tárolási előnyeit a központi telepítések azonos Azure-beli helyhez. Egy teljesen minősített beállításjegyzék neve van hello űrlap `myregistry.azurecr.io`.

  Ön [hozzáférés szabályozása](container-registry-authentication.md) tooa tároló beállításjegyzék használatával az Azure Active Directory-biztonsági [egyszerű](../active-directory/active-directory-application-objects.md) vagy egy megadott rendszergazdai fiókot. Standard futtatási hello `docker login` parancs tooauthenticate a beállításjegyzékbeli.

* **Felügyelt beállításjegyzék** – A beállításjegyzékekhez további képességeket nyújtó szint három termékváltozatban – Basic, Standard és Premium – érhető el. Ezek termékváltozatok hello képek hello Azure tároló nyilvántartó szolgáltatást, ami növeli a megbízhatóságot, és lehetővé teszi, hogy a új szolgáltatások által kezelt Storage-fiókok vannak tárolva. Az új képességek közé tartozik a webhook-integráció, az Azure Active Directoryval való adattár-hitelesítés és a törlési funkció támogatása. Felhasználóknál hello beállítás toochoose felügyelt nyilvántartó vagy toocreate között egy beállításjegyzék-kezelőkön létrehozásakor a saját Tárfiókok alapját.

* **Tár** – A beállításjegyzékek egy vagy több tárat tartalmaznak, amelyek tárolórendszerképek csoportjai. Az Azure Container Registry támogatja a többszintű adattárnévtereket. Ez a funkció lehetővé teszi, hogy Ön toogroup gyűjtemények képek kapcsolódó tooa adott alkalmazás vagy alkalmazások toospecific fejlesztési vagy működési csapatok gyűjteménye. Példa:

  * A(z) `myregistry.azurecr.io/aspnetcore:1.0.1` egy, a teljes vállalatban elérhető rendszerképet jelöl
  * `myregistry.azurecr.io/warrantydept/dotnet-build`jelölő kép használt közösen használt hello jótállás részleg toobuild .NET-alkalmazások
  * `myregistry.azrecr.io/warrantydept/customersubmissions/web`egy webes kép alkalmazásban hello ügyfél jelentések hello jótállás részleg által birtokolt csoportosítva jelenti.

* **Rendszerkép** – Az adattárakban tárolt egyes rendszerképek egy-egy Docker-tároló csak olvasható pillanatfelvételei. Az Azure tároló-beállításjegyzékek Windows- és Linux-rendszerképeket is tartalmazhatnak. A rendszerképek neveit Ön határozza meg mindegyik tárolókörnyezetben. Használjon szabványos [Docker parancsok](https://docs.docker.com/engine/reference/commandline/) toopush képek tárház vagy lekéréses a tárházban lévő lemezkép.

* **Tároló** – A tároló egy szoftveralkalmazást határoz meg annak függőségeivel együtt egy teljes fájlrendszerbe csomagolva, beleértve a kódot, a futtatókörnyezetet, a rendszereszközöket és a könyvtárakat. A Docker-tárolókat a tároló-beállításjegyzékekből előhívott Windows- vagy Linux-rendszerképek alapján futtathatja. Egyetlen számítógépen futó tárolók hello operációs rendszer kernel megosztani. Docker-tároló fő disztribúciókkal teljes mértékben hordozhatók tooall a Linux, Mac és a Windows rendszer.




## <a name="next-steps"></a>Következő lépések
* [Hozzon létre egy tároló beállításjegyzék hello Azure-portál használatával](container-registry-get-started-portal.md)
* [Hozzon létre egy tároló beállításjegyzék hello Azure parancssori felület használatával](container-registry-get-started-azure-cli.md)
* [Leküldéses az első kép hello Docker parancssori felület használatával](container-registry-get-started-docker-cli.md)
* toobuild folyamatos integrációt és telepítési munkafolyamat Visual Studio Team Services, Azure Tárolószolgáltatás és az Azure-tároló beállításjegyzék, lásd: [ebben az oktatóanyagban](../container-service/dcos-swarm/container-service-docker-swarm-setup-ci-cd.md).
* Ha azt szeretné, tooset saját Docker titkos beállításjegyzék az Azure-ban (nélkül egy nyilvános végpontot), lásd: [központi telepítése a saját személyes Docker beállításjegyzék Azure](../virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md).
