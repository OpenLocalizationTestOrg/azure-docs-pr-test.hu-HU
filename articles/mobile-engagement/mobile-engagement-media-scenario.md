---
title: "a Mobile Engagement megvalósítási aaaAzure Media alkalmazás"
description: "Media app forgatókönyv tooimplement Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 48201cc8-4e04-485c-a8dc-d6406d23f3ed
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 6a495eb790993a30d5c03802aa9e6404fea983d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="implement-mobile-engagement-with-media-app"></a>Mobile Engagement az Médiaalkalmazással megvalósítása
## <a name="overview"></a>Áttekintés
John egy mobil projektmenedzsert nagy adathordozó számára. Ezután egy új alkalmazást, amely rendelkezik egy nagyon magas letöltési száma nemrég indul el. Ezután elérte a letöltési a célokat, de továbbra is a térjen vissza a Investment(ROI) felhasználónként nem felel meg a követelményeknek. 

John már észlelt, ezért a ROI értéke túl alacsony. Felhasználók gyakran abbahagyja az alkalmazás csak 2 hét után, és legtöbbjük soha nem térjen vissza. Azt szeretné, hogy az alkalmazás tooincrease hello megőrzése.

Néhány kezdeti tesztelés után ő rendelkezik megtanulta, amikor ezután kapcsolatba lép a felhasználók a leküldéses értesítések, általában az alkalmazás használatával toocontinue. Inaktív volt felhasználók is gyakran ad vissza attól függően, hogy elküldi azokat értesítések toohello alkalmazás. John úgy dönt, a bevonási Program valamilyen tooinvest a alkalmazás, amely a leküldéses értesítések használ a speciális célcsoport-kezelési.

John nemrég rendelkezik olvasási hello [Azure Mobile Engagement – első lépések útmutató ajánlott eljárásoknak megfelelő beállításában](mobile-engagement-getting-started-best-practices.md) és határozott hello útmutató tooimplement hello javaslatait.

## <a name="objectives-and-kpis"></a>Célok és a KPI-k
John alkalmazás fő érdekelteknek felel meg. Üzleti ads jönnek létre, a felhasználók a saját media felhasználását. John felhasználónként fogyasztott tartalmak növelésével növeli a saját bevételek. Az összes megállapodjanak egyik fő cél a: 25 %-kal ads tooincrease értékesítések. Ezek toomeasure üzleti a fő teljesítménymutatók (KPI-k) és a meghajtó a célkitűzés létrehozása

* Felhasználónként kattintott reklámkattintások száma
* A cikk lapok meglátogatott (minden felhasználóhoz / munkamenetenként / hetente / havi...)
* Mik azok a kedvenc kategóriáinak

A fő érdekelteknek John értekezlet alapján definiált rendelkezik saját üzleti KPI-k. Ezután a következő hello 1. rész [Azure Mobile Engagement – első lépések útmutató ajánlott eljárásoknak megfelelő beállításában](mobile-engagement-getting-started-best-practices.md). 

A következő hoz hello bevonási KPI-k tooensure, amely célok elérésekor a következő:

* Megőrzési figyelése között a következő időközönként hello: napi, heti, Kétheti vagy havi.
* Aktív felhasználók száma
* hello alkalmazás minősítése hello alkalmazásban tárolja.

A következő műszaki KPI-k hello hello informatikai csapat javaslatai alapján, a következő kérdések tooanswer hello fel lettek hozzáadva:

* Mi az a felhasználó elérési út (mely megnyitják, hogy hány amikor a felhasználók egy azt)
* Összeomlások vagy munkamenetenként észlelt hibák száma?
* Milyen operációs rendszer verzióit futtatják a felhasználók?
* Mi az az hello átlagos mérete a képernyő a felhasználók számára?
* Milyen típusú hálózati kapcsolatok használata esetén rendelkeznek a felhasználók?

Az egyes KPI-KET ő osztályozza a hello adatot, amennyi szükséges, és hello megfelelő helyét a forgatókönyv a ő rögzíti.

## <a name="engagement-program-and-integration"></a>Bevonási program és az integráció
Most, hogy John befejezte a KPI-k meghatározása, a bevonási stratégia fázis először 4 bevonási programok és a célok meghatározása:![][1]

John megnyitja mélyebb és részletesen leírja a leküldéses értesítések minden program által. Leküldéses értesítési öt elem határozzák meg:

1. Cél: Mi célja hello hello értesítési
2. Hogyan hello cél érhető el
3. Cél: ki fogja megkapni hello értesítést?
4. Tartalom: Mi az az hello megfogalmazást és hello formátum hello értesítés (a App/Out a következő alkalmazás)
5. Amikor: Mi van hello legjobb jelenleg toosend a leküldéses értesítés
   
    ![][2]

További információt talál a toohello [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Toohello részét 2 János cél szakasz toodefine rendelkezik adatokkal hello tanulmány szerint, toocollect, mind az írás a címkézési tervben a közösen az informatikai csapat tooimplement hello megoldással. A megvalósítás és a felhasználói tesztelés 1 hét után John végül indíthatja el a programok.

## <a name="program-results"></a>Program eredmények
4 hónappal később János áttekinti a programok teljesítményét. Üdvözli a Program üdvözlő és heti Program hello teljesítésének a célokat. csak egy munkamenet-felhasználó hello száma csökken, hello alkalmazás több szolgáltatást használják, és hetente kapcsolatok hello száma rendelkezik kettőzni.

Hello **inaktív Program** elősegíti az John felhasználói tendenciák feltárásához ismertetése. Úgy tűnik, hogy 15 %-a hello inaktív felhasználók visszatérhet toohello alkalmazást. Azonban ezek többsége nem maradnak aktívak, több mint 1 hónap. John úgy rendelkezik, a potenciális optimalizálása további értesítések és a tartalom lehetőségek bővítése esik.

Hello **felderítése Program** nem működik jól. Növeli a több értékesítő, de nincs elég tooreach a céljait. John azonosítja, hogy nem rendelkezik elég adat toomake megfelelő célcsoport-kezelési, és javaslatot tesz a megfelelő tartalom. Ezután a program leáll, és "Szerkesztői leküldéses értesítések küldése" az Azure Mobile Engagement-ra összpontosít. A újságírók már rendelkezik egy CMS megoldás toosend leküldéses értesítéseket, és nem szeretné, hogy toochange.

John úgy dönt, hogy toouse hello Reach API, amely egy HTTP REST API-t, amely lehetővé teszi a Reach-kampányokat kezelése toouse AZME webes felület nélkül. Ezt a módszert John ezután van szüksége, és annak írók hello adatokat gyűjthet tookeep hello CMS megoldással.

az tooensure, amely a szolgáltatás megfelelően működik, John informatikai csapat toobe vigilant megkérdezi, a következő pontok hello:

1. **A művelet rendszerek** : minden rendelkeznek a saját szabályok tooadministrate leküldéses értesítések, ezért John úgy dönt, toolist minden esetben és ellenőrzések Ha hello API-k kezelnie.
   Például: Android leküldéses rendszer lehetővé teszi, hogy nagy vonalakban tekinti, ami nem hello elő az iOS.
2. **Időkereten belül**: John szeretne egy API-t, amely hello időkereten belül és egy záró toocampaigns. Azt szeretné, hogy bármely zavaró értesítési bombing toopreserve felhasználóit.
3. **Kategóriák**: Marketing csapat sablon előkészíti az egyes riasztások. John informatikai csapat tooset kategóriák belül hello API kéri.

Néhány teszt után John meggyőződött. Köszönjük, hogy toothis API-t a újságírók továbbra is küldhet a leküldéses értesítések az azok CMS és Azure Mobile Engagement összes viselkedési adatokat gyűjt a számukra

Ezek 4 után először hónap, eredmények tükröznie egy jó általános teljesítmény- és ad abban, hogy John és annak üzenőfalon, ROI felhasználói növekszik a tartomány 15 %-a / és mobil értékesítési képviselő 17.5 % összes értékesítés 7.5 %-os növekedést csak négy hónapon belül.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
