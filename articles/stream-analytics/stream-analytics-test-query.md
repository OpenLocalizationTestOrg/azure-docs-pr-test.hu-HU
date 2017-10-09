---
title: "aaaAzure Stream Analytics lekérdezési tesztelése |} Microsoft Docs"
description: "Hogyan tootest a lekérdezéseket a Stream Analytics-feladatok."
keywords: "lekérdezés tesztelése, a lekérdezés hibaelhárítása"
documentation center: 
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
ms.openlocfilehash: 3b141d98332fdc170e696e181c8446796a86f78e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="test-azure-stream-analytics-queries-in-hello-azure-portal"></a>Azure Stream Analytics lekérdezések tesztelése hello Azure-portálon

Az Azure Stream Analytics lekérdezések tesztelhet hello Azure-portálon a toostart anélkül, illetve leállíthat feladatot.

## <a name="test-hello-input"></a>Hello bemeneti tesztelése

1. tootest minta bemeneti adatok, kattintson a jobb gombbal bármelyik bemenet, és válassza **tölthet fel fájlból adatot**.

    ![Stream analytics lekérdezési szerkesztő lekérdezés tesztelése](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. Hello feltöltés befejeződése után kattintson **teszt** tootest a hello lekérdezése az megadott adatokat.

    ![Stream analytics lekérdezési szerkesztő teszt mintaadatokat](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

a lekérdezés kimenetét hello megjelenő hello böngészőben a letöltés eredményeit hivatkozással azt szeretné, hogy toosave hello tesztkimenet későbbi használatra. Mostantól egyszerűen és ismételt módosítsa a lekérdezést és tesztelik azt ismételten toosee hogyan hello kimeneti változik.

![Stream Analytics query editor minta kimenet](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

Több kimenettel, a lekérdezésben használt lásd: külön-külön is kimenetek hello eredményeit, és könnyen váltása.

Miután elégedett a lekérdezés mentéséhez, indítsa el a feladatot, és lehetővé teszik hello böngészőben megjelenített hello eredményekkel feldolgozni az eseményeket hiba nélkül.

## <a name="get-help"></a>Segítségkérés

Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések

* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)
