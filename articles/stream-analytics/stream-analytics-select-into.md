---
title: "Azure Stream Analytics aaaDebug lekérdezi a SELECT INTO használatával |} Microsoft Docs"
description: "Adatok közepes mintalekérdezés Stream Analytics a SELECT INTO utasítások használatával"
keywords: 
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9952e2cf-b335-4a5c-8f45-8d3e1eda2e20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 27e41af1a6ea06b4509d07a3a67087490d0ec1fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-queries-by-using-select-into-statements"></a>A SELECT INTO utasítás használatával lekérdezések hibakeresése

A valós idejű adatfeldolgozással ismerik, mely hello adatok tűnik hello hello közepén lekérdezés hasznos lehet. Bemeneti adatok vagy a lépéseket az Azure Stream Analytics-feladat több alkalommal olvashatók, mert külön SELECT INTO utasítások is írhat. Ezzel köztes adatok kiírja a tárolóba, és lehetővé teszi, hogy vizsgálja meg a hello helyességét hello adatok, ahogy *változók bemutató* tegye amikor hibakeresése a program.

## <a name="use-select-into-toocheck-hello-data-stream"></a>A SELECT INTO toocheck hello az adatfolyam használata

hello egy Azure Stream Analytics-feladat a következő példalekérdezés van egy adatfolyam-bemenet, két hivatkozás adatok bemeneti és egy kimeneti tooAzure Table Storage. hello lekérdezés adatokat hello eseményközpont és két blobok tooget hello nevét és kategóriáját referenciaadatai csatlakozik:

![Példa a SELECT INTO lekérdezést](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

Vegye figyelembe, hogy hello feladat fut, de nincsenek események előállítása hello kimenet alatt. A hello **figyelés** csempe látható itt, láthatja, hogy hello bemeneti van olyan adatokat, de nem tudja, melyik hello lépését **csatlakozás** összes hello eldobott események toobe okozta.

![hello figyelés csempe](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
Ebben az esetben is hozzáadhat, néhány további SELECT INTO utasítások túl "naplófájl" hello köztes ILLESZTÉS eredményei és hello hello bemeneti olvasható adatok.

Ebben a példában fel lett véve a két új "ideiglenes kimenetek." A fogadó tetszés el. Azure Storage itt példaként használjuk:

![Extra SELECT INTO utasítások hozzáadása](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

Ehhez hasonló hello lekérdezés majd módosíthatja:

![Egy átírt SELECT INTO lekérdezést](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

Most indítsa újra a hello feladat, és hagyja, hogy néhány percig futtassa. Ezután lekérdezés temp1, és a Visual Studio Cloud Explorer tooproduce hello a következő táblák temp2:

**temp1 tábla**
![SELECT INTO temp1 tábla](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)

**temp2 tábla**
![SELECT INTO temp2 tábla](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)

Ahogy látja, temp1 és temp2 mindkét és adatokat, és hello névoszlopa megfelelően van-e feltöltve temp2. Azonban mivel a még nincsenek adatok kimenet, valami probléma:

![A SELECT INTO output1 tábla adatot nem tartalmazó](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

Hello az adatok mintavétele biztos lehet szinte bizonyos jelenti, hogy hello probléma hello ILLESZTÉSI második. Hello blob hello referenciaadatok letöltését, és tekintse meg:

![A SELECT INTO ref tábla](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

Ahogy látja, a referenciaadatok a GUID hello hello formátuma eltér [a] oszlopában temp2 hello hello formátuma. Ezért hello adatok nem érkeznek output1 a várt módon.

Javítsa ki a hello adatformátum, tooreference blob feltöltése, és próbálkozzon újra:

![A SELECT INTO ideiglenes tábla](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

Megadott idő hello adatok hello kimenet formázásakor és feltölti a várt módon.

![A SELECT INTO végső tábla](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a>Segítségkérés

Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések

* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

