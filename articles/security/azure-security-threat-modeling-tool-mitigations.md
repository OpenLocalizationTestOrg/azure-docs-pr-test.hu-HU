---
title: "-Microsoft fenyegetések modellezése eszköz - aaaMitigations Azure |} Microsoft Docs"
description: "Hello Microsoft fenyegetések modellezése eszköz kijelölő lehetséges megoldásokat toohello leginkább kitett azok mérséklési lapján fenyegetések jönnek létre."
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 000c0980d976b09fc9287e582e3776efaf7e390c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-threat-modeling-tool-mitigations"></a>Microsoft fenyegetések modellezése eszköz megoldást

hello fenyegetések modellezése eszköz a Microsoft biztonsági fejlesztési életciklus (SDL) hello alapeleme. Ez lehetővé teszi, hogy a szoftver tooidentify építészek részére, és amelyek viszonylag egyszerű és költséghatékony tooresolve korai, csökkenthető a potenciális biztonsági problémákat. Ennek eredményeképpen jelentősen csökkenti fejlesztési hello összköltsége. Emellett azt tervezett hello eszköz szakértőivel nem biztonsági szem előtt, könnyítve modellezni az összes által létrehozása, és fenyegetést modellek elemzése a világos útmutatást biztosít.

A Microsoft hello  **[fenyegetések modellezése eszköz](./azure-security-threat-modeling-tool.md)**  tooget neki még ma!

## <a name="mitigation-categories"></a>Megoldás kategóriák

hello fenyegetések modellezése eszköz azok mérséklési szerint vannak kategóriába toohello hello következő tevődik össze webes alkalmazás biztonsági keret szerint:

| Kategória | Leírás |
| -------- | ----------- |
| **[A vizsgálati és naplózási](./azure-security-threat-modeling-tool-auditing-and-logging.md)** | Ki nem adott meg, mit és mikor? Vizsgálati és naplózási tekintse meg a toohow az alkalmazás rekordok biztonsági események |
| **[Hitelesítés](./azure-security-threat-modeling-tool-authentication.md)** | hogy vagy? A hitelesítés az hello folyamat, ha egy entitás igazolja hello identitását egy másik entitás, általában hitelesítő adatokat, például egy felhasználónév és jelszó használatával |
| **[Engedélyezési](./azure-security-threat-modeling-tool-authorization.md)** | Mire szolgál? Engedélyezési, hogy az alkalmazás biztosítja az erőforrások és a műveletek hozzáférés-vezérlést |
| **[A kommunikáció biztonsága](./azure-security-threat-modeling-tool-communication-security.md)** | Aki akkor beszélünk? Kommunikációs biztonsági biztosítja végzett összes kommunikáció megfelelő biztonsága |
| **[Konfigurációkezelés](./azure-security-threat-modeling-tool-configuration-management.md)** | Aki futtató az alkalmazást? Mely adatbázisokat nem azt csatlakozni? Hogyan történik az az alkalmazás? Hogyan biztonságosak ezeket a beállításokat? Konfigurációkezelés toohow hivatkozik az alkalmazás kezeli a működési problémák |
| **[Titkosítás](./azure-security-threat-modeling-tool-cryptography.md)** | Hogyan meg megakadályozzák titkos kulcsokat (bizalmas)? Hogyan esetleg illetéktelen ellenőrző az adatok és a szalagtárak (integritása)? Hogyan akkor ad magok meg lehet erős titkosítással véletlenszerű értéket? Titkosítás toohow hivatkozik az alkalmazás érvénybe lépteti a titkosítás és integritás |
| **[Kivételek kezelése](./azure-security-threat-modeling-tool-exception-management.md)** | Ha az alkalmazás metódus hívása nem sikerült, mire az alkalmazást? Milyen mértékű tegye meg felfedése? Hajtsa végre ismét rövid hiba információk tooend felhasználók? Kivétel értékes információk hátsó toohello hívó át? Szabályosan elakad az alkalmazást? |
| **[Bemenet-ellenőrzést](./azure-security-threat-modeling-tool-input-validation.md)** | Honnan tudhatja, hogy az alkalmazás fogad hello bemeneti-e érvényes és biztonságos? Bemenet-ellenőrzést az alkalmazás-szűrők, scrubs vagy elutasítja a további feldolgozás előtt bemeneti toohow hivatkozik. Vegye figyelembe a korlátozási belépési ponton keresztül bemeneti és kimeneti kilépési ponton keresztül kódolás. Megbízik, például az adatbázisok és a fájlmegosztások forrásból származó adatokat? |
| **[Bizalmas adatok](./azure-security-threat-modeling-tool-sensitive-data.md)** | Hogyan az alkalmazás bizalmas adatokat kezelni? Bizalmas adatok az alkalmazás kezeli az adatokat a memóriában, hello hálózaton, vagy az állandó tároló védeni kell toohow hivatkozik |
| **[Munkamenet-kezelés](./azure-security-threat-modeling-tool-session-management.md)** | Hogyan nem az alkalmazás kezelése és védelme a felhasználói munkamenetek? A munkamenet a felhasználó és a webes alkalmazás közötti kapcsolódó interakciókat tooa sorozatát hivatkozik |

Ez segít azonosítani:

* Hol vannak hello végzett leggyakoribb hibák
* Hol vannak hello legtöbb végrehajthatóként fejlesztései

Ennek eredményeképpen ezek kategóriák toofocus használ, és munkánk biztonsági, így ha tudja, hello legjellemzőbb biztonsági problémákat hello bemeneti érvényesítési, hitelesítési és engedélyezési kategóriákban, megkezdheti van. További információt  **[a szabadalmi hivatkozás](https://www.google.com/patents/US7818788)**

## <a name="next-steps"></a>Következő lépések

Látogasson el  **[fenyegetések modellezése eszköz fenyegetések](./azure-security-threat-modeling-tool-threats.md)**  toolearn arról hello fenyegetés kategóriák hello eszköz toogenerate lehetséges tervezési fenyegetések használja.
