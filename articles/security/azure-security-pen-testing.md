---
title: "aaaPen tesztelése |} Microsoft Docs"
description: "hello cikk hello behatolást vagy a biztonság (pentest) tesztelésének áttekintést nyújt, és hogyan hajtsa végre az Azure-infrastruktúra futó alkalmazások elleni pentest."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: yurid
ms.openlocfilehash: 202c239f46d8693ab7aa85e237235372e743e108
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pen-testing"></a>Toll tesztelése
Hello kiváló dolog Alkalmazástesztelési és a központi telepítés a Microsoft Azure használatával kapcsolatban, hogy nem kell tooput együtt egy a helyszíni infrastruktúra toodevelop, tesztelése és az alkalmazások központi telepítése. Minden hello infrastruktúra van hello Microsoft Azure platform szolgáltatásaiból által végrehajtott kezeli. Tooworry requisitioning, beszerzése, és "lefejtési és halmozott" nem rendelkezik a saját helyszíni hardverre.

Ez remek –, de továbbra is szükséges, hogy a normál biztonsági elvégezhető toomake megfelelő gondossággal. Egy hello kapcsolatos fontos toodo az behatolást vagy a biztonság teszt hello alkalmazások központi telepítése az Azure-ban.

Előfordulhat, hogy már tudja, hogy elvégzi a Microsoft [behatolást vagy a biztonság, az Azure környezetben végzett teszteléséhez](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Ez segít javítani a platformot, és végig is vezeti a műveletek biztonsági vezérlők javítása, új biztonsági ellenőrzéseire bemutatása és a biztonsági folyamatok javítása.

Jelenleg nem toll meg az alkalmazás teszteléséhez, de azt szeretné, hogy és tooperform toll tesztelése a saját alkalmazásai szükség lesz ismertetése. Amely azért hasznos, mert amikor Ön hello megfelelő biztonság érdekében az alkalmazások segítséget biztonságosabbá teszi az egész Azure ökoszisztéma hello.

Ha Ön toll tesztelje az alkalmazásokat, azt nézhet toous támadás. A Microsoft [folyamatosan figyelni](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) a támadási mintákat és fogja elindítani az incidensválasz-folyamata, ha igazolnia kell a. Azt nem oldották meg, és azt nem segítsen nekünk, ha azt indít az incidensekre adott reakciók esedékes tooyour saját gondossággal toll tesztelés miatt.

Milyen toodo?

Amikor készen áll a toopen tesztelje az Azure által kezelt alkalmazásokat, lehetősége van túl[ossza meg velünk](https://portal.msrc.microsoft.com/en-us/engage/pentest). Tudjuk, hogy meghatározott tesztek végrehajtása toobe fog, ha azt nem véletlenül (például blokkolja a tesztelést hello IP-cím), leállítása, mindaddig, amíg a tesztek felel meg az toohello Azure toll tesztelési feltételek és kikötések ismertetett[Microsoft felhő egyesített szabályok az Engagement tesztelés behatolás](https://technet.microsoft.com/en-us/mt784683).
Szabványos teszteket hajthat végre a következők:

* A végpontok toouncover hello vizsgálatainak [megnyitása webes alkalmazás biztonsági Project (OWASP) felső 10 biztonsági réseket](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Tesztelési fuzz](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) a végpontok
* [Port vizsgálatát](https://en.wikipedia.org/wiki/Port_scanner) a végpontok

Teszt nem végezhető el több típus ilyen típusú [szolgáltatásmegtagadásos (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) támadás. Ez magában foglalja a kezdeményezésének egy szolgáltatásmegtagadási támadás önmagában, vagy előfordulhat, hogy határozza meg, bemutatása vagy szolgáltatásmegtagadási támadás bármilyen típusú szimulálása kapcsolódó tesztek végrehajtása.

Biztosan meg készen áll a futó alkalmazások tesztelése a Microsoft Azure-ban toll használatába tooget? Ha igen, akkor a központi, a toohello keresztül [behatolást vagy a biztonság teszt áttekintése](https://technet.microsoft.com/library/mt784683.aspx) lapon (és hello létrehozása alján hello hello tesztelési kérelem gombra kattintson. További információ a feltételek és hogyan lehet jelentést a biztonsági hiányosságokat kapcsolódó tooAzure vagy bármely más Microsoft-szolgáltatást a hasznos hivatkozásokat tesztelés hello toll is találhat.
