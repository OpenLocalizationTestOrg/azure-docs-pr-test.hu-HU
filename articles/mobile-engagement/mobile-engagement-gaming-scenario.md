---
title: "a Mobile Engagement megvalósítási aaaAzure játék alkalmazás"
description: "Alkalmazás forgatókönyv tooimplement Azure Mobile Engagement játékok"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2cafc044-4902-4058-8037-49399bf6bf7f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b82b4a868a33f42e5b759e43e66103556c097f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="implement-mobile-engagement-with-gaming-app"></a>Mobile Engagement az Játékalkalmazásról megvalósítása
## <a name="overview"></a>Áttekintés
Egy játék kezdeti elindította az új alapú halászati role-play/stratégia játék alkalmazás. hello játék már működik, és 6 hónapig. A game hatalmas sikeres, és több millió letöltések rendelkezik pedig hello megőrzési nagyon magas összehasonlított tooother kezdeti játék alkalmazásokat. Hello negyedéves felülvizsgálati értekezleten az érdekelt felek elfogadja tooincrease átlagos bevétel felhasználónként (ARPU) van szükségük. Prémium szintű a játékbeli csomagok elérhetők, különleges ajánlatokat. A game csomagok lehetővé teszik a felhasználók tooupgrade hello Megjelenés és a teljesítmény halászati sorok és lures vagy tackles hello játékban. Azonban a csomag értékesítési nem nagyon alacsony. Úgy döntenek, első tooanalyze hello felhasználói élmény analytics eszközzel, és ezután egy bevonási program tooincrease értékesítési használatával toodevelop speciális Szegmentálás.

Hello alapján [Azure Mobile Engagement – első lépések útmutató ajánlott eljárásoknak megfelelő beállításában](mobile-engagement-getting-started-best-practices.md) engagement-stratégia kialakítása.

## <a name="objectives-and-kpis"></a>Célok és a KPI-k
Hello játék kulcsfontosságú érintettekkel felel meg. Az összes fogadja el az egyik fő cél - tooincrease prémium csomag értékesítési 15 %-kal. Ezek toomeasure üzleti a fő teljesítménymutatók (KPI-k) és a meghajtó a célkitűzés létrehozása

* Mely szintjén hello játék ezeket a csomagokat vásárolt?
* Mi az a hello bevétel felhasználónként, munkamenetenként, heti és havi?
* Mik azok a hello kedvenc beszerzési típusok?

Hello 1. rész [Getting Started Guide](mobile-engagement-getting-started-best-practices.md) azt ismerteti, hogyan toodefine hello célkitűzések és a KPI-k. 

Hello üzleti KPI-k már meg van adva hello Mobile termék Manager bevonási KPI-k toodetermine új felhasználói trendek és a megőrzési hoz létre.

* Megőrzési figyelése és a következő időközönként hello használja: napi, 2 naponta, hetente, havonta, és minden harmadik hónapban
* Aktív felhasználók száma
* tárolja a hello hello alkalmazás minősítése

A következő műszaki KPI-k hello hello informatikai csapat javaslatai alapján, a következő kérdések tooanswer hello fel lettek hozzáadva:

* Mi az a felhasználó elérési út (mely megnyitják, mennyi időt igénybe rajta)
* Összeomlások vagy munkamenetenként észlelt hibák száma
* Milyen operációs rendszer verzióit futtatják a felhasználók?
* Mi az az hello átlagos mérete a képernyő a felhasználók számára?
* Milyen típusú internetkapcsolattal rendelkeznek a felhasználók?

Minden KPI-hez tartozó hello Mobile termék Manager hello adatok elemzőnek szüksége van, és hol helyezkedik el saját forgatókönyv határozza meg.

## <a name="engagement-program-and-integration"></a>Bevonási program és az integráció
Egy speciális bevonási program létrehozása, előtt hello Mobile projekt igazgató feladata hello projekt rendelkeznie kell egy részletes ismertetése, hogyan és mikor termékek hello felhasználók vannak-e használni.

Hello Mobile projekt igazgató 3 hónap után összegyűjtötte elég adat tooenhance alkalmazásbeli leküldéses értesítési eladásait. Ezután Tanulja meg, amelyek:

* hello első beszerzési általában 14 hello szintjén történik. Ezekben az esetekben 90 %-át hello beszerzési új legendary fegyverek $3.
* Ezekben az esetekben 80 %-át, a felhasználók, akik a beszerzési hello termékkel, és további végzett vásárol.
* Felhasználók, akik hello szint 20, indítsa el a toospend több mint 10 $vagy hét.
* A felhasználók általában toobuy prémium csomagok 16, 24 és 32 szinten.

Köszönjük toothis elemzés, Mobile projekt igazgató hello úgy dönt, toocreate meghatározott leküldéses értesítési sorozatok tooincrease app Sales. Három leküldéses folyamatok, amelyek ezután meghívja hoz: üdvözli a program, értékesítési Program és inaktív Program. További információt talál a toohello [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
