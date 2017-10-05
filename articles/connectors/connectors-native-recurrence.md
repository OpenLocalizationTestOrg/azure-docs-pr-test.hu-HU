---
title: "Adja hozzá az ismétlődési eseményindító a logic apps |} Microsoft Docs"
description: "Az ismétlődési eseményindító, és azt egy Azure logikai alkalmazás használata áttekintése."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 51dd4f22-7dc5-41af-a0a9-e7148378cd50
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: fe558958c316c8dba42163e277ae01451f712e5a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-recurrence-trigger"></a>Ismerkedjen meg az ismétlődési eseményindító
Az ismétlődési eseményindító segítségével hatékony munkafolyamatokat hozhat létre a felhőben.

Megteheti például a következőt:

* Egy SQL tárolt eljárás futtatására minden nap munkafolyamat ütemezéséhez.
* A múlt héten kapcsolatos egy bizonyos hashtaggel történő e-mail-összes Twitter-üzeneteket összegzését.

Ismerkedés az ismétlődési eseményindító logikai alkalmazás használatával, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Ismétlődés eseményindítók
Egy eseményindító egy eseményt, amely segítségével indítsa el a munkafolyamatot, amely a logikai alkalmazás van definiálva. [További tudnivalók az eseményindítók](connectors-overview.md).

Íme egy parancssorozat-példa bemutatja, hogyan állíthatja be a logikai alkalmazás ismétlődési eseményindító:

1. Adja hozzá a **ismétlődési** logikai alkalmazás első lépéseként eseményindító.
2. Töltse ki a paraméterek az ismétlődési.

A logikai alkalmazást most futtató minden időköz után elindul.

![HTTP eseményindító](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Eseményindító részletei
Az ismétlődési eseményindító tulajdonságai a következők konfigurálható.

A logikai alkalmazást a megadott időtartam után következik be.
A * azt jelenti, hogy mezőt kötelező kitölteni.

| Megjelenített név | Tulajdonság neve | Leírás |
| --- | --- | --- |
| Gyakoriság * |gyakoriság |Időegység: `Second`, `Minute`, `Hour`, `Day`, vagy `Year`. |
| Időköz * |időköz |Az adott gyakorisága az ismétlődés időköze. |
| Időzóna |Időzóna |Ha a kezdési idő áll rendelkezésre a UTC eltolás nélkül, ezt az időzónát használható. |
| Kezdési idő |startTime |A kezdési idő [ISO 8601 formátum](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). |

<br>

## <a name="next-steps"></a>Következő lépések
Most, próbálja ki a platformot és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Az egyéb rendelkezésre álló összekötők logic Apps alkalmazások felmérésével felfedezheti a [API-k lista](apis-list.md).

