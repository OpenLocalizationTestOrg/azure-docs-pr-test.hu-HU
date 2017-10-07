---
title: "a Stream Analytics esemény feldolgozása aaaReal idő Eseményfeldolgozási |} Microsoft Docs"
description: "Ismerje meg, hogyan képes együttműködni az Azure szolgáltatások valós idejű esemény feldolgozása és az elemzések engedélyezéséhez."
keywords: "valós idejű feldolgozással, az események feldolgozásával, a referencia-architektúrában"
services: stream-analytics,event-hubs,storage,sql-database
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: 
ms.assetid: 11af48bc-313c-4527-8c80-91088dc9f3c6
ms.service: stream-analytics
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: jeffstok
ms.openlocfilehash: a43c503d709609ba61e9932822d30bc2208906ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Architektúra hivatkozni: a Microsoft Azure Stream Analytics valós idejű Eseményfeldolgozási
hello referencia-architektúrában az Azure Stream Analytics valós idejű Eseményfeldolgozási tervezett tooprovide valós idejű platform telepítése a Microsoft Azure-szolgáltatás (PaaS) adatfolyam-feldolgozási megoldásként egy általános szerkezeti terve.

## <a name="summary"></a>Összefoglalás
Hagyományosan elemzési megoldásokat alapulnak képességeit ETL (kinyerés, átalakítás, betöltés) és az adatraktározás terén, például a korábbi tooanalysis tárolt adatok esetén. Változó követelményeknek megfelelően további gyorsan érkező adatokat, beleértve a létező modell toohello korlát leküldendő. hello képességét tooanalyze adatok áthelyezése adatfolyamok előzetes toostorage belül egy megoldást, és bár ez nem egy új képesség, hello megközelítés nem széles körben elfogadott összes iparági referenciaegyenesen között. 

Microsoft Azure biztosít egy kiterjedt katalógust, amely támogathatja a másik megoldás forgatókönyvek és követelmények tömbje analytics technológiák. Ha a mely Azure-szolgáltatások toodeploy végpont megoldás ajánlatok hello mélysége adott feladat lehet. A dokumentum tervezett toodescribe hello képességeket, és együttműködését az hello különböző Azure-egy esemény-adatfolyam-megoldást támogató szolgáltatások. Ezen kívül néhány hello forgatókönyv, amelyben az ügyfelek is kihasználhatja az ilyen típusú megközelítés ismerteti.

## <a name="contents"></a>Tartalom
* Vezetői összefoglaló
* Bevezetés tooReal idejű elemzés
* A valós idejű adatok Azure-ban Értékajánlatához
* Valós idejű elemzések gyakori helyzetek
* Architektúrája és összetevői
  * Adatforrások
  * Adatintegráció réteg
  * Valós idejű elemzési réteg
  * Adatok tárolási rétegből
  * Bemutató / fogyasztás réteg
* Összegzés

**Szerző:** Insights adatközpont Charles Feddersen, megoldásokért felelős mérnök kiváló, Microsoft Corporation

**Közzétett:** 2015. januári

**Változat:** 1.0

**Letöltés:** [valós idejű, a Microsoft Azure Stream Analytics Eseményfeldolgozási](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

