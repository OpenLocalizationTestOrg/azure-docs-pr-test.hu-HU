---
title: "aaaSecure a az eszközök internetes hálózatát (IoT) az Azure-ban |} Microsoft Docs"
description: " Azure internetes hálózata (IoT) szolgáltatások széles skálájával képességeket kínálnak. Ez a cikk segítségével megismerheti, hogyan toosecure az IoT-megoldások Azure-ban. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 1473c8dd-8669-48fb-86db-b3c50e2eaf59
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: b6cb2ea1c1facada854fb52c55066f34a8289e47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-overview"></a>Az eszközök internetes hálózatát biztonsági áttekintése
Azure internetes hálózata (IoT) szolgáltatások széles skálájával képességeket kínálnak. Ezekkel a vállalati szintű szolgáltatásokkal a következőket teheti:

* Adatok gyűjtése eszközökről
* Streamek elemzése mozgás közben
* Nagy adatkészletek tárolása és lekérdezése
* Valós idejű és előzményadatok megjelenítése
* Integrálás irodai háttérrendszerekkel

Ezek a képességek, Azure IoT Suite csomagok együtt toodeliver több Azure szolgáltatások egyéni kiterjesztéseket tartalmazó előkonfigurált megoldásokat. Ezek előkonfigurált megoldásokat olyan alap megvalósítások annak a közös IoT megoldást, amelyek segítenek tooreduce hello időt, toodeliver az IoT-megoldásokhoz. Hello IoT szoftverfejlesztői készletek használ, testre szabhatja és a megoldások toomeet kiterjesztése a saját követelményeinek megfelelően. Ezeket a megoldásokat példaként vagy sablonként is használhatja az új IoT-megoldások fejlesztésekor.

hello Azure IoT suite egy hatékony megoldás, az IoT-igényeinek. Célszerű upmost fontos, hogy az IoT-megoldások a biztonságot szem előtt tartva hello indítás készültek. Az IoT-eszközök számát puszta hello, mert minden biztonsági esemény vehet a széles körű esemény jelentős következményekkel.

tisztában toohelp hogyan toosecure hello a következő információkat kell az IoT-megoldásokhoz.

## <a name="security-architecture"></a>Biztonsági architektúra
A rendszer tervezésekor fontos toounderstand hello potenciális fenyegetések toothat rendszer, és adjon hozzá megfelelő védelmekkel ennek megfelelően módon hello rendszer terveztek és tervezett. Fontos toodesign hello hello start terméket a biztonságot szem előtt tartva, mert megértése, hogyan lehet egy támadó, képes toocompromise a rendszer segít, gondoskodjon arról, hogy megfelelő megoldást hello elejétől helyen.

Megismerheti az IoT-biztonsági architektúrája olvasásával [Internet a dolgok biztonsági architektúrája](../iot-suite/iot-security-architecture.md).

Ez a cikk ismerteti, amelyek a következő témakörök hello:

* [A fenyegetések modellezése biztonsági kezdődik](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
* [Az IoT biztonsági](../iot-suite/iot-security-architecture.md#security-in-iot)
* [A fenyegetés modellezési hello Azure IoT-Referenciaarchitektúra](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-hello-ground-up"></a>Biztonsági szabad hello a
hello IoT kockázatot egyedi biztonsági, adatvédelmi és megfelelőségi kihívást toobusinesses világszerte. Hagyományos számítógépes technológia, ahol a problémák szoftver, és hogyan megvalósított köré szerveződik, eltérően IoT vonatkozik, mi történik, ha hello számítógépes és hello fizikai világot átszervezését. Az IoT-megoldások védelmének biztosítása az eszközök, az adott eszközöket és a hello felhő és a biztonságos adatvédelem hello felhőben feldolgozás és a tárolás során közötti biztonságos kapcsolat biztonságos kiépítése igényel. Eszközök fejlesztésének ilyen funkció, azonban a következők: erőforrás-korlátozott eszközök, központi telepítések földrajzi eloszlása és megoldást belül számos eszközön.

Áttekintheti, hogyan toohandle biztonsági olvasásával évi [az eszközök internetes hálózatát biztonsági szabad hello a](../iot-suite/securing-iot-ground-up.md).

hello cikkből hello a következő témaköröket:

* [Biztonságos infrastruktúra hello szabad](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
* [A Microsoft Azure – a vállalati biztonságos IoT-infrastruktúra](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Ajánlott eljárások
Az IoT-infrastruktúra védelmének biztosítása megköveteli a szigorú jellegű biztonsági stratégia. Biztonságossá tétele a felhasználók az adatok integritását, miközben átvitel közben keresztüli hello nyilvános internet, toosecurely létesítési eszközök, az egyes rétegekhez nagyobb biztonság alkot hello felhőben adatok hello átfogó infrastruktúra.

Megismerheti az eszközök internetes hálózatát biztonsági gyakorlati tanácsok olvasásával [az eszközök internetes hálózatát ajánlott biztonsági eljárások](../iot-suite/iot-security-best-practices.md).

hello cikkből hello a következő témaköröket:

* [Az IoT hardver gyártója/integráló](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
* [Az IoT-megoldás fejlesztői](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
* [Az IoT-megoldás deployer](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
* [Az IoT-megoldás operátor](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
