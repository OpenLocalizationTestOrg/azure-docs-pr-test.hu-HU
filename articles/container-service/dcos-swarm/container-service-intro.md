---
title: "az Azure felhőalapú üzemeltetési tároló aaaDocker |} Microsoft Docs"
description: "Az Azure tárolószolgáltatással egy módon toosimplify hello létrehozását, konfigurációs és felügyeleti fürt virtuális gépek, amelyek indexelése előre konfigurált toorun alkalmazások."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: rogardle
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 46a0071a7497a3ff44d75413b49f1d06f844c446
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toodocker-container-hosting-solutions-with-azure-container-service"></a>Azure Tárolószolgáltatás-megoldások üzemeltető bemutatása tooDocker tároló 
Az Azure Tárolószolgáltatás egyszerűbbé teszi az Ön toocreate, konfigurálásához és egy fürt virtuális gépek, amelyek indexelése előre konfigurált toorun alkalmazásokat kezeléséhez. Ezt nyílt forráskódú ütemezési és vezénylési eszközök optimalizált konfigurációját igénybe véve teszi lehetővé. Ez lehetővé teszi, hogy a hogy toouse a meglévő ismeretei, vagy egy nagy- és az egyre növekvő törzsének közösségi szakértelmét, toodeploy épülnek, és tároló-alapú alkalmazások a Microsoft Azure kezelése.

![Az Azure Tárolószolgáltatás lehetővé teszi indexelése toomanage alkalmazások az Azure-on a több gazdagépen.](./media/acs-intro/acs-cluster-new.png)

Az Azure Tárolószolgáltatás hello Docker tároló formátum tooensure, hogy teljes mértékben hordozhatók-e az alkalmazás tárolók kihasználja. A választott Marathon és a DC/OS, a Docker Swarm vagy Kubernetes is, hogy ezek alkalmazások toothousands tároló, vagy még akkor is, tízezreit méretezheti támogatja.

Azure Tárolószolgáltatás használatával kihasználhatja Azure, a vállalati szintű szolgáltatásait alkalmazás hordozhatóság--hordozhatóság hello vezénylési rétegek, beleértve továbbra is megőrzésével.

## <a name="using-azure-container-service"></a>Az Azure Container Service használata
Az Azure Tárolószolgáltatás célunk tooprovide tároló üzemeltetési környezet nyílt forrású eszközök és technológiák manapság népszerű, az ügyfelek között. toothis célból elérhetővé kell tenni hello szabványos API-végpontok a kiválasztott orchestrator (a DC/OS, a Docker Swarm, vagy a Kubernetes). Ezeket a végpontokat használatával teheti az olyan szoftvert, amely képes a toothose végpontok van szó. Például hello Docker Swarm végponthoz hello esetben választása toouse hello Docker parancssori felület (CLI). DC/os hello Vezénylőtípusú CLI akkor célszerű használni. a Kubernetes esetében pedig a `kubectl` használata mellett.

## <a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Egy Docker-fürt létrehozása az Azure Container Service használatával
Azure Tárolószolgáltatás használatával toobegin meg az Azure Tárolószolgáltatás-fürt üzembe helyezése hello portálon (keresési hello piactér a **Azure Tárolószolgáltatás**), Azure Resource Manager-sablon használatával ([Docker Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm), [DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos), vagy [Kubernetes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)), vagy a hello [Azure CLI 2.0](container-service-create-acs-cluster-cli.md). a megadott hello gyorsindítási sablonok lehet módosított tooinclude további vagy speciális Azure konfigurálása. Több információ: [Azure tárolószolgáltatás-fürt üzembe helyezése](container-service-deployment.md).

## <a name="deploying-an-application"></a>Alkalmazás üzembe helyezése
Az Azure Container Service lehetővé teszi annak eldöntését, hogy Docker Swarmot, DC/OS-t vagy Kubernetest szeretne-e használni a vezényléshez. A kiválasztott vezénylőtől függ, hogyan helyezi üzembe az alkalmazást.

### <a name="using-dcos"></a>A DC/OS használata
A DC/OS egy olyan elosztott operációs rendszer, hello Apache Mesos elosztott rendszerek kernel alapján. Apache Mesos van: hello Apache szoftver Foundation fájlban található, és néhány olyan hello [legnagyobb nevek informatikai](http://mesos.apache.org/documentation/latest/powered-by-mesos/) felhasználók és közreműködő szerepkörrel rendelkező személyek.

![DC/OS használatához konfigurált Azure Container Service, ügynökökkel és főkiszolgálókkal.](media/acs-intro/dcos.png)

A DC/OS és az Apache Mesos lenyűgöző szolgáltatáskészletet tesz elérhetővé:

* Bizonyított méretezhetőség
* Apache Zookeepert használó, nagy hibatűrésű replikált főkiszolgáló és alkiszolgálók
* Docker formátumú tárolók támogatása
* Feladatok közötti natív elkülönítés Linux-tárolókkal
* Többforrású ütemezés (memória, processzor, lemez és portok)
* Java, Python és C++ API-k új párhuzamos alkalmazások fejlesztéséhez
* Webes felhasználói felület a fürtállapot áttekintésére

Alapértelmezés szerint futó Azure Tárolószolgáltatási DC/OS hello Marathon vezénylési platform munkaterhelések ütemezéshez foglalja magában. Hello az ACS a DC/OS központi telepítés részét képező azonban hello Mesosphere Universe szolgáltatások tooyour szolgáltatás adható hozzá. A hello Universe szolgáltatások közé tartoznak, Spark, Hadoop, Cassandra és még sok más.

![A DC/OS Universe az Azure Container Service-ben](media/dcos/universe.png)

#### <a name="using-marathon"></a>A Marathon használata
A Marathon, a fürtre vonatkozó init és a szolgáltatások cgroups – vagy – hello kis-és az Azure Tárolószolgáltatás, Docker-formátumú tárolók ellenőrzési rendszer. A Marathon egy olyan webes felhasználói felületet biztosít, ahonnan telepítheti az alkalmazásokat. Ezt a felületet egy, a következőhöz hasonló URL-címmel érheti el: `http://DNS_PREFIX.REGION.cloudapp.azure.com`, ahol mind a DNS\_PREFIX, mind a REGION az üzembe helyezéskor van meghatározva. Természetesen megadhatja a saját DNS-nevét is. További információ a tároló hello Marathon webes felhasználói felület használatával, lásd: [hello Marathon webes felhasználói felület DC/OS-tárolók kezelése](container-service-mesos-marathon-ui.md).

![A Marathon alkalmazáslistája](media/dcos/marathon-applications-list.png)

Hello REST API-k használata a marathon segítségével való kommunikációhoz. Számos ügyfélkódtár létezik, amelyek elérhetők minden egyes eszköz számára. A különböző nyelveken--terjed ki, és természetesen hello HTTP protokoll használható bármilyen nyelven. Továbbá sok népszerű DevOps-eszköz is támogatja a Marathont. Ez maximális rugalmasságot biztosít a műveleti csapatnak, amikor egy Azure Container Service-fürttel dolgozik. További információ a tároló hello Marathon REST API használatával, lásd: [hello Marathon REST API-t a DC/OS-tárolók kezelése](container-service-mesos-marathon-rest.md).

### <a name="using-docker-swarm"></a>A Docker Swarm használata
A Docker Swarm natív fürtszolgáltatást biztosít a Docker számára. Mivel Docker Swarm funkcionál hello standard Docker API, bármely eszköz, amely már kommunikál a Docker démon Swarm tootransparently méretezési toomultiple állomások használhatja az Azure Tárolószolgáltatás.

![Az Azure Tárolószolgáltatás toouse Swarm konfigurálva.](media/acs-intro/acs-swarm2.png)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Támogatott eszközök a tárolók a Swarm-fürt kezeléséhez tartalmazza, azonban nem csak hello következő:

* Dokku
* Docker parancssori felület (CLI) és Docker Compose
* Krane
* Jenkins

### <a name="using-kubernetes"></a>A Kubernetes használata
A Kubernetes egy népszerű, nyílt forráskódú, termelési szintű tárolóvezénylő eszköz. A Kubernetes automatizálja a tárolóalapú alkalmazások üzembe helyezését, méretezését és felügyeletét. Megoldás, nyílt forráskódú, és hello nyílt forráskódú Közösség célja, mert az Azure Tárolószolgáltatás zökkenőmentesen fut, és használt toodeploy tárolók Azure tárolószolgáltatás léptékű lehet.

![Az Azure Tárolószolgáltatás toouse Kubernetes konfigurálva.](media/acs-intro/kubernetes.png)

Többek között a következő funkciókat tartalmazza:
* Vízszintes méretezés
* Szolgáltatásészlelés és terheléselosztás
* Titkos kódok és konfigurációk kezelése
* API-alapú automatizált kibocsátások és visszaállítások
* Önjavítás

## <a name="videos"></a>Videók
Bevezetés az Azure Container Service használatába (101):  

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Container-Service-101/player]
>
>

Épület alkalmazások használatával hello Azure Tárolószolgáltatás (Build 2016)

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/B822/player]
>
>

## <a name="next-steps"></a>Következő lépések

A tárolószolgáltatási fürt hello segítségével telepítheti [portal](container-service-deployment.md) vagy [Azure CLI 2.0](container-service-create-acs-cluster-cli.md).
