---
title: "a logic apps aaaAdd hello ismétlődési eseményindítót |} Microsoft Docs"
description: "Áttekintés hello ismétlődési eseményindító, és hogyan toouse azt egy Azure logikai alkalmazást."
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
ms.openlocfilehash: e7c625c382a88a1e7cdfff4ddc0caf55727232bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-recurrence-trigger"></a>Ismerkedés a hello ismétlődési eseményindító
Hello ismétlődési eseményindító segítségével hatékony munkafolyamatokat hozhat létre hello felhőben.

Megteheti például a következőt:

* Ütemezhet egy munkafolyamat toorun SQL tárolt eljárás minden nap.
* Az e-mail hello kapcsolatos egy bizonyos hashtaggel történő múlt héten belül minden Twitter-üzeneteket összegzését.

tooget használatának hello ismétlődési eseményindító a logikai alkalmazás, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Ismétlődés eseményindítók
Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény. [További tudnivalók az eseményindítók](connectors-overview.md).

Íme egy parancssorozat-példa hogyan tooset ismétlődése be trigger logikai alkalmazás:

1. Adja hozzá a hello **ismétlődési** hello első lépéseként logikai alkalmazás eseményindító.
2. Töltse ki hello paraméterek hello ismétlési időköz.

hello logikai alkalmazás most futtató minden időköz után elindul.

![HTTP eseményindító](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Eseményindító részletei
hello ismétlődési eseményindító következő konfigurálható tulajdonságai hello rendelkezik.

A logikai alkalmazást a megadott időtartam után következik be.
A * azt jelenti, hogy mezőt kötelező kitölteni.

| Megjelenített név | Tulajdonság neve | Leírás |
| --- | --- | --- |
| Gyakoriság * |frequency |hello időegység: `Second`, `Minute`, `Hour`, `Day`, vagy `Year`. |
| Időköz * |interval |hello hello tartozó hello Ismétlődési gyakoriság megadott időintervallum. |
| Időzóna |Időzóna |Ha a kezdési idő áll rendelkezésre a UTC eltolás nélkül, ezt az időzónát használható. |
| Kezdési idő |startTime |hello kezdési időpontja [ISO 8601 formátum](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). |

<br>

## <a name="next-steps"></a>Következő lépések
Most, próbálja ki hello platform és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Ismerje meg a hello más rendelkezésre álló összekötők logic Apps alkalmazások felmérésével a [API-k lista](apis-list.md).

