---
title: "az Azure Stream Analytics AAA mintavételi bemenetek |} Microsoft Docs"
description: "Meghatározhatja problémák elhárításakor Stream Analytics-feladatok."
keywords: "bemeneti, bemeneti mintavételi hibaelhárítása"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 9637a8664de099eebb8f5654036d2957f4c6b7ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-input-stream-sampling"></a>Az Azure Stream Analytics bemenet-adatfolyam mintavétel

Azure Stream Analytics segítségével bemeneti eseményeket, amelyek fájlból származnak és tesztelheti a lekérdezéseket hello portálon toostart anélkül vagy leállíthat feladatot is mintát.

## <a name="testing-your-query"></a>A lekérdezés tesztelése

Hello Stream Analytics feladat részletek ablaktábláján nyissa meg a hello **Lekérdezésszerkesztő** panel alatt hello lekérdezés nevére kattintva **lekérdezés**. (Nem a lekérdezés még jött létre, mert a példaforgatókönyvben, kattintson a hello **< >** helyőrző.)

![hello Stream Analytics query editor](media/stream-analytics-sample-data-input/stream-analytics-query-editor.png)

hello gazdag szerkesztő panelen a lekérdezés létrehozásához volt hello korábbi változatban jelenik meg. Most hello panel frissítve lett egy új, hogy látható hello be- és kimenetekkel, hello a lekérdezés és a feladat meghatározott bal oldali ablaktáblán.

![hello Stream Analytics query editor a bemeneti és kimeneti listák](media/stream-analytics-sample-data-input/stream-analytics-query-editor-highlight.png)

Egy további bemeneti és kimeneti, amelyek nem definiált is megjelennek a rendszer. Hello új lekérdezés sablont, amely a kiindulási pont származnak. Módosíthatja, vagy akár eltűnnek a teljes sorozatnak így hello lekérdezés szerkesztése. Nyugodtan figyelmen kívül hagyhatja őket most.

tootest minta bemeneti adatok, kattintson a jobb gombbal bármelyik bemenet, és válassza **tölthet fel fájlból adatot**.

![hello Stream Analytics lekérdezési szerkesztő feltöltés mintaadatok fájlból parancs](media/stream-analytics-sample-data-input/stream-analytics-query-editor-upload.png)

Hello feltöltés befejeződése után kattintson **teszt** tootest a hello lekérdezése az imént megadott adatokat.

![hello Stream Analytics query editor teszt gombra](media/stream-analytics-sample-data-input/stream-analytics-query-editor-test.png)

Ha toosave hello tesztkimenet későbbi használatra, a lekérdezés kimenetét hello egy hivatkozás toohello letöltés eredményeit hello böngésző jelenik meg. Mostantól egyszerűen és ismételt módosítsa a lekérdezést és tesztelik azt ismételten toosee hogyan hello kimeneti változik.

![Stream Analytics query editor minta kimenet](media/stream-analytics-sample-data-input/stream-analytics-query-editor-samples-output.png)

A kép megelőző hello, egy második kimenet hozzáadva, úgynevezett **HighAvgTempOutput**.

Ha több kimenet egy lekérdezésben, dokumentumkönyvtárából hello minden kimeneti külön-külön, és könnyen váltása.

Után elégedett hello eredményeket, mentheti a lekérdezést, indítsa el a feladat, elhelyezkedik vissza, és tekintse meg a Stream Analytics hello magic.

## <a name="get-help"></a>Segítségkérés

Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)
