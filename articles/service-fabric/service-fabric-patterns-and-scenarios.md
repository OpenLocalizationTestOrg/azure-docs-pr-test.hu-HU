---
title: "a Service Fabric aaaAzure mintákat és forgatókönyvek |} Microsoft Docs"
description: "Ismerje meg az ajánlott eljárások és bizonyítása, újrahasznosítható minták toodesign fejleszthet, és a Service Fabric a mikroszolgáltatások működnek."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: d5aa75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2017
ms.author: ryanwi
ms.openlocfilehash: 3811420eb53d9a49e490dd2e2e5319d8dea5629c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-patterns-and-scenarios"></a>A Service Fabric mintákat és forgatókönyvek
Tartózkodik felépítése az Azure Service Fabric használatával a nagyméretű mikroszolgáltatások létrehozására, ha további hello szakértők számára készült és ezen a platformon beépített szolgáltatásként (PaaS). Ismerkedés a megfelelő architektúra, és ezután megtudhatja, hogyan toooptimize erőforrások az alkalmazáshoz. Hello [Service Fabric minták és gyakorlatok](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) működés során leggyakrabban ismételt valós ügyfelek Service Fabric forgatókönyvek és alkalmazási területek hello kérdésekre ad választ.
 
Ismerje meg toodesign, hogyan fejleszthet, és működik a mikroszolgáltatások a Service Fabric használatával ajánlott eljárásairól és mintáiról bevált, újrafelhasználható. A Service Fabric áttekintést kaphat, és ezután alaposabban tanulmányozhatja a mély témakörök, amelyek közé tartozik a fürt optimalizálása és a biztonsági, örökölt alkalmazások áttelepítése léptékű játék motorok és egyéb üzemeltetési IoT. Nézze meg a különböző munkaterhelések folyamatos kézbesítési és még a Linux-támogatás és tárolók hello részletek elérése. 

## <a name="introduction"></a>Bevezetés
Gyakorlati tanácsok vizsgálatát, és Sajátítsa el kiválasztására vonatkozó platform (PaaS) szolgáltatás keresztül infrastruktúra (IaaS) szolgáltatás. Ezzel a hello adatokat a következő bevált alkalmazás tervezési alapelvek.

<table><tr><th>Videó</th><th>PowerPoint fedélzet</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=N2KwbbSGD_6405167344">
<img src="./media/service-fabric-patterns-and-scenarios/intro.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Bevezetés tooService háló</a></td></tr>
</table>

## <a name="cluster-planning-and-management"></a>Tervezési és a felügyeleti fürtön
További tudnivalók a kapacitástervezés, a fürt optimalizálása és a fürt biztonsági, az Azure Service Fabric nézze meg a.

<table><tr><th>Videó</th><th>PowerPoint fedélzet</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=cyDYZcSGD_2805167344">
<img src="./media/service-fabric-patterns-and-scenarios/cluster.png" WIDTH="360" HEIGHT="244">
</a></td><td> <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Fürt tervezési és kezelése</a></td></tr>
</table>

## <a name="hyper-scale-web"></a>Hyper-skálázási webes
Körül kapacitású webalkalmazás rendelkezésre állásának és megbízhatóságának, kapacitású és állapotkezelés fogalmak áttekintése.

<table><tr><th>Videó</th><th>PowerPoint fedélzet</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=NgldAdSGD_405167344">
<img src="./media/service-fabric-patterns-and-scenarios/hyperscaleweb.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Hyper-skálázási webes</a></td></tr>
</table>

## <a name="iot"></a>IoT
Megismerkedhet az eszközök internetes hálózatát (IoT) hello Azure Service Fabric, beleértve a hello Azure IoT csővezeték, több-bérlős és IoT léptékű hello környezetében.

<table><tr><th>Videó</th><th>PowerPoint fedélzet</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=naFUVeSGD_1505167344">
<img src="./media/service-fabric-patterns-and-scenarios/iot.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></td></tr>
</table>

## <a name="gaming"></a>Játékok
Nézze meg a kapcsolja játékokat, interaktív játékok és meglévő játék motorok üzemeltetéséhez.

<table><tr><th>Videó</th><th>PowerPoint fedélzet</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=6wECzeSGD_3805167344">
<img src="./media/service-fabric-patterns-and-scenarios/gaming.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Játék</a></td></tr>
</table>

## <a name="continuous-delivery"></a>Folyamatos készregyártás
Megismerkedhet a fogalmakat, beleértve a Visual Studio Team Services, a munkafolyamat build/csomag/közzététele, a több környezet beállítása és a szolgáltatás csomagmegosztás folyamatos integráció/folyamatos kézbesítési.

<table><tr><th>Videó</th><th>PowerPoint fedélzet</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=78h5ofSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/cd.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Folyamatos kézbesítés</a></td></tr>
</table>

## <a name="migration"></a>Migrálás
További információk a áttelepítése egy felhőalapú szolgáltatás, továbbá az örökölt alkalmazások toomigration.

<table><tr><th>Videó</th><th>PowerPoint fedélzet</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=hd1cMgSGD_5605167344">
<img src="./media/service-fabric-patterns-and-scenarios/migration.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Áttelepítés</a></td></tr>
</table>

## <a name="containers-and-linux-support"></a>Tárolók és a Linux-támogatás
Hello válasz toohello kérdés beolvasása "Miért tárolók?" További információk a Windows tárolók, támogatja a Linux és a Linux tárolók vezénylési hello előzetes verzióját. Továbbá, megtudhatja, hogyan toomigrate .NET Core alkalmazások tooLinux.

<table><tr><th>Videó</th><th>PowerPoint fedélzet</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=V1ERJhSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/containers.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Tárolók és a Linux-támogatás</a></td></tr>
</table>

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Service Fabric mintákat és forgatókönyvek, tudjon meg többet az hogyan túl[létrehozása és kezelése a fürtök](service-fabric-deploy-anywhere.md), [Felhőszolgáltatások alkalmazások tooService háló áttelepítése](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [beállítása folyamatos kézbesítési](service-fabric-set-up-continuous-integration.md), és [tároló üzembe helyezése](service-fabric-containers-overview.md).
