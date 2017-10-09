---
title: "-Microsoft fenyegetések modellezése eszköz - aaaThreats Azure |} Microsoft Docs"
description: "Hello Microsoft fenyegetések modellezése eszköz fenyegetés kategória lapján kategóriák tartalmazó összes elérhetővé tett generált fenyegetéseket."
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
ms.openlocfilehash: 0bd51f81370b6385ff1ac9769e34fc089e1dfc9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-threat-modeling-tool-threats"></a>Microsoft fenyegetések modellezése eszköz fenyegetések

hello fenyegetések modellezése eszköz a Microsoft biztonsági fejlesztési életciklus (SDL) hello alapeleme. Ez lehetővé teszi, hogy a szoftver tooidentify építészek részére, és amelyek viszonylag egyszerű és költséghatékony tooresolve korai, csökkenthető a potenciális biztonsági problémákat. Ennek eredményeképpen jelentősen csökkenti fejlesztési hello összköltsége. Emellett azt tervezett hello eszköz szakértőivel nem biztonsági szem előtt, könnyítve modellezni az összes által létrehozása, és fenyegetést modellek elemzése a világos útmutatást biztosít.

> A Microsoft hello  **[fenyegetések modellezése eszköz](./azure-security-threat-modeling-tool.md)**  tooget neki még ma!

hello fenyegetések modellezése eszköz, például az alábbi ők hello bizonyos kérdések megválaszolása nyújt segítséget:

* Hogyan lehet módosítani egy támadó hello hitelesítési adatok?
* Mi az hello hatása, ha egy támadó tudja olvasni a hello felhasználói profil adatainak?
* Mi történik, ha a hozzáférés megtagadva toohello felhasználói profil adatbázis?

## <a name="stride-model"></a>Modell STRIDE

hegyes kérdéseket, a Microsoft által használt hello STRIDE modell, amely különböző típusú fenyegetések kategorizálja és egyszerűbbé teszi az ilyen típusú fogalmaz meg toobetter súgó hello átfogó biztonsági beszélgetéseket.

| Kategória | Leírás |
| -------- | ----------- |
| **-Címek hamisítását** | Magában foglalja a érvénytelenül elérése és egy másik felhasználói hitelesítési adatok, például felhasználónév és jelszó használata |
| **Illetéktelen módosítása** | Magában foglalja az adatok hello rosszindulatú módosítását. Ilyen például az illetéktelen módosítások toopersistent adatok, például a azt tranzakciós két számítógép között egy nyitott hálózatokon, például hello interneten keresztül egy adatbázis, és az adatok hello átalakítására használatban |
| **Megtagadás** | Felhasználók, akik megtagadja egy művelet végrehajtása nélkül más felek számára bármilyen módon tooprove egyébként társított – például a felhasználó szabálytalan műveletet hajt egy rendszer, amely nem rendelkezik hello képességét tootrace hello tiltott operatív. Nem megtagadás toohello azon képessége, a rendszer toocounter repudiation fenyegetések hivatkozik. A felhasználó, aki egy elemet a későbbi szoftvervásárlások Előfordulhat például, toosign hello elem fogadásakor. hello szállító csak akkor használja hello hello felhasználó kapta hello csomag bizonyító adatok fogadását aláírása |
| **Információk felfedése** | Magában foglalja a hello elérhetővé tegyék információk tooindividuals, akik toohave hozzáférés tooit feltételezhetően nem – például hello képességét felhasználók tooread egy fájlt, amely nem voltak meg az engedélyt, vagy arra hello azon képessége, egy behatoló tooread adatokat átvitel közben két számítógép között |
| **Szolgáltatásmegtagadás** | Szolgáltatásmegtagadási (DoS-) támadások toovalid felhasználói megtagadása – például azáltal, hogy a webkiszolgáló, átmenetileg nem érhető el vagy nem használható. Meg kell védeni ellen DoS bizonyos típusú tooimprove rendszer rendelkezésre állásának és megbízhatóságának egyszerűen fenyegetések |
| **Jogok kiterjesztése** | Egy nem rendszerjogosultságú felhasználói privilegizált hozzáférést kap, és ezáltal rendelkezik megfelelő hozzáférési toocompromise vagy megsemmisítik hello teljes rendszer. Jogosultságszint-emelés jogosultság fenyegetések közé tartoznak az ilyen helyzetek, amelyben a támadó rendelkezik hatékony követhessék minden rendszer védelmekkel és megbízható hello rendszert illeti, valóban veszélyes helyzet részét képezik |

## <a name="next-steps"></a>Következő lépések

A folytatáshoz túl**[fenyegetések modellezése eszköz azok mérséklési](./azure-security-threat-modeling-tool-mitigations.md)**  toolearn hello különböző módokon az Azure-ral ezek a fenyegetések mérséklésére vonatkozó.
