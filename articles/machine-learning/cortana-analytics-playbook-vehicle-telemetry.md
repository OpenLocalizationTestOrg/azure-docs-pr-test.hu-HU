---
title: "aaaPredict vehicle állapotát és ki irányítja az Azure - a szokásokat |} Microsoft Docs"
description: "Használja a Cortana Intelligence toogain valós idejű és prediktív elemzések hello lehetőségeit a vehicle állapotát, és ki irányítja az szokásokat."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 54cc890ff39493bc040bb809721388349665720f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>A járműtelemetria-elemzési megoldás forgatókönyve
Ez **menü** Ez a forgatókönyv toohello fejezetek hivatkozásokat tartalmaz. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Áttekintés
Super számítógépek ki hello laborból került át, és most leparkolni vannak a garázs! Ezek a legmodernebb autók érzékelők hello képességét tootrack jogosultságot ad a számtalan tartalmaz, és több millió esemény másodpercenként figyelése. Elvárjuk, hogy által 2020, ezek autók része lesz történt csatlakoztatott toohello Internet. Képzelje el azokat a számos olyan adatok tooprovide nagyobb biztonsági, megbízhatósági és jobban vezetői élmény koppintva! A Microsoft tett ezt a helyzetet a Cortana Intelligence megváltozz.

A Microsoft a Cortana Intelligence egy teljes körűen felügyelt big Data típusú adatok, és az adatok, amely lehetővé teszi tootransform analytics suite speciális intelligens működésbe. Azt szeretnénk, ha toointroduce Cortana Intelligence Vehicle Telemetriai Analytics Megoldássablonban toohello. Ez a megoldás bemutatja, hogyan car kereskedők autó gyártók és biztosítási vállalatok szolgáltatásai segítségével végre hello Cortana Intelligence toogain valós idejű és prediktív elemzések a vehicle állapotát, és ki irányítja a szokásokat. 

hello megoldás valósul meg a [lambda architektúra mintát](https://en.wikipedia.org/wiki/Lambda_architecture) hello hello Cortana Intelligence Platform for felhasználása megjelenítő valós idejű és kötegelt feldolgozást. hello megoldást: 

* a Vehicle telematika szimulátor biztosít
* kihasználja a választásával dolgozhat fel szimulált vehicle telemetriai események több millió az Azure Event Hubs 
* használja a Stream Analytics toogain valós idejű elemzése a vehicle állapotát
* a hosszú távú tároló gazdagabb kötegelt elemzésekben az tartja fenn a hello adatokat. 
* kihasználja a Machine Learning közüli a valós idejű és feldolgozási toogain prediktív elemzések kötegelt.
* kihasználja a HDInsight tootransform adatokat a méretezési és a Data Factory toohandle vezénylési, ütemezés, erőforrás-kezelés és hello kötegelt feldolgozási folyamatának figyelése 
* Ez a megoldás részletes irányítópult ad valós idejű adatok és a prediktív elemzés képi megjelenítések Power BI használatával

## <a name="architecture"></a>Architektúra
![Megoldás architektúrája](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*1. ábra – Vehicle Telemetriai Analytics megoldás architektúrája*

Ez a megoldás tartalmaz hello következő **Cortana Intelligence összetevők** és azok befejezési tooend integrációs bővíthető:

* **Az Event Hubs** választásával dolgozhat fel a vehicle telemetriai események több millió az Azure.
* **Stream Analytics** való vehicle egészségügyi valós idejű elemzése és továbbra is fennáll, hogy az adatok a hosszú távú tároló gazdagabb kötegelt elemzéséhez.
* **Gépi tanulás** közüli valós idejű és kötegfeldolgozási toogain prediktív elemzések.
* **HDInsight** léptékű kihasználhatók tootransform adat
* **Adat-előállító** kezeli az orchestration, erőforrás-kezelés ütemezése és hello kötegelt folyamat figyelését.
* **A Power BI** Ez a megoldás részletes irányítópult ad valós idejű adatok és a prediktív elemzés képi megjelenítések.

Ez a megoldás két különböző hozzáfér **adatforrások**: 

* **Szimulált vehicle jelek és diagnosztikai**: vehicle telematika szimulátor bocsát ki a diagnosztikai adatokat, és azt jelzi, hogy hello vehicle és befolyásoló tényezők minta egy időben hello toohello állapotának felel meg. 
* **Vehicle katalógus**: a VIN toomodel tartalmazó referencia-adatkészletnek.

