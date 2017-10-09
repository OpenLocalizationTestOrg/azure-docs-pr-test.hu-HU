---
title: "az Azure idő adatsorozat Insights aaaOverview |} Microsoft Docs"
description: "Bevezetés tooAzure idő adatsorozat Insight, egy új szolgáltatás idő adatsorozat adatelemzés és az IoT-megoldások"
keywords: 
services: tsi
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: 
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: omravi
ms.openlocfilehash: 8c022bf1fae88eddab3dbebc7bb36cc15a785172
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-time-series-insights"></a>Mi az Azure Time Series Insights?

Az Azure idő adatsorozat Insights egy tárolási, elemzés és a képi megjelenítés összetevők, melyek könnyen tooingest felügyelt felhőszolgáltatás, tárolásához, vizsgálatát, és események milliárd egyidejűleg elemzését. A Time Series Insights globális áttekintést nyújt az adatokról, így gyorsan ellenőrizheti IoT-megoldásait, és elkerülheti az eszközök költséges leállását, mivel a rendszer segít a rejtett trendek és rendellenességek felderítésében, valamint a problémák kiváltó okainak közel valós idejű elemzésében. Idő adatsorozat Insights ingests esemény-brókerek (pl. az IoT-központok vagy az Event Hubs) idősorozat adatokat, hello adatok indexeket és kivonja konfigurálható adatmegőrzési szabály alapján. Felhasználók adatfelhasználó hello egy egyszerűen elsajátítható UX vagy REST lekérdezés API-kon keresztül.

![A Time Series Insights áttekintése](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Elsődleges forgatókönyvek

* IoT-megoldások monitorozása és ellenőrzése néhány perc alatt.
* Nagy mennyiségű IoT-adat vizualizációja és elemzése.
* Gyorsabb kiváltó okokat kereső elemzések és rendellenességészlelés
* Több eszközt, üzemet és adatot tartalmazó globális nézet készítése.

## <a name="capabilities-and-benefits"></a>Képességek és előnyök

* **Egyszerű tooget lépések**: Azure idő adatsorozat elemzések nem jár előzetes adatok előkészítése igényel, és nagyon gyorsan elvégezhető. Csatlakozás az Azure IoT-központ vagy az Eseményközpont kiválasztásával események toobillions perc múlva. A csatlakozás után megjelenítheti és tooquickly érvényesíteni az IoT-megoldások másodpercben érzékelő adatok kezeléséhez. Idő adatsorozat Insights egy egyszerű toouse; Egysoros kód írása nélkül is kommunikálnak az adatokkal.  Nincs semmilyen új nyelvi toolearn; Idő adatsorozat Insights egy részletes, szabad szöveges lekérdezési felületet biztosít a tapasztalt felhasználók és pontot, majd kattintson az összes feltárása.

* **Majdnem valós idejű elemzése**: idő adatsorozat Insights fogadására képes több millió érzékelő esemény naponta több száz, egy perc késést, így gyorsan reagálhasson toochanges. A Time Series Insights segítségével részletesebb betekintést nyerhet az érzékelőadatokba, mivel lehetővé teszi a trendek és rendellenességek gyors észlelését, az összetett, kiváltó okokat kereső elemzések elvégzését és a költséges leállási idő elkerülését. Azáltal, hogy a valós idejű és előzményes adatok közötti összefüggés, idő adatsorozat Insights segítségével rejtett trendeket hello adatok zárolásának feloldásához.

* **Egyéni megoldások készítése**: Az Azure Time Series Insights-adatokat beágyazhatja meglévő alkalmazásaiba, vagy új egyéni megoldásokat hozhat létre a Time Series Insights REST API-jaival. Az egyéni megoszthatja mások tooexplore a felderítések nézetek létrehozása és megosztása.

* **Méretezhetőség**: idő adatsorozat Insights egy tervezett toosupport IoT méretekben. A képen a 31 napos span napi egy alapértelmezett megőrzést millió esemény 1 millió too100 érkező is. Közel valós időben jelenítheti meg és elemezheti az élő adatstreameket és nagy mennyiségű előzményadatot. Soron, bemenő és a megőrzési díjszabás megnöveli a tooaccommodate egy legalább egyszer fenyegető vállalati méretű.

## <a name="time-series-insights-glossary"></a>Time Series Insights – Szószedet

* **Környezet**: A környezet egy Azure-erőforrás, amely bejövő adatforgalmi és tárolási kapacitással rendelkezik.  Az ügyfelek kiépítése keresztül hello Azure-portálon a szükséges kapacitással rendelkező környezetekben.
* **Eseményforrás**: Az eseményforrás egy eseményközvetítőből, például az Azure Event Hubsból származik.  Idő adatsorozat Insights keresztül közvetlenül tooEvent források választásával dolgozhat fel hello adatfolyam programozás nélkül. A Time Series Insights jelenleg az Azure Event Hubs és Azure IoT Hubs forrásokat támogatja.
* **Referenciaadatok**: idő adatsorozat Insights biztosít a felhasználók számára hello képességét toojoin idő adatsor referenciaadatokkal.  A referenciaadatok lehetnek az eszközökkel kapcsolatos metaadatok vagy egyéb statikus adatok, amelyek viszonylag ritkán változnak. Idő adatsorozat Insights illesztése hello referenciaadatok az adatfolyamokat, amely lehetővé teszi a felhasználók toovisualize és elemezheti az adatokat közel valós időben.
