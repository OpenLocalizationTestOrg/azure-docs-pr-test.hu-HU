---
title: "a logic apps aaaAdd hello lekérdezési művelet |} Microsoft Docs"
description: "A műveleteket, például a szűrő tömb hello lekérdezés műveletei áttekintése."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 34e702c7-f9e5-4885-9266-fc7404adecfe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2016
ms.author: jehollan
ms.openlocfilehash: 3d4be901e7e6bf1b644057648930667ab34f2124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-query-action"></a>Ismerkedés a hello lekérdezési művelet
Hello lekérdezési művelet használatával kötegek és tömbök tooaccomplish munkafolyamatokat dolgozhat:

* Hozzon létre egy feladatot az összes magasabb prioritású rekord adatbázisból.
* Az e-mailekhez, az Azure-blobot összes PDF melléklet mentése.

tooget használatának hello lekérdezési művelet egy logic App, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-hello-query-action"></a>Hello lekérdezés művelettel
Egy művelet során, amely logikai alkalmazás definiált hello munkafolyamat végzi. [További információ a műveletek](connectors-overview.md).  

hello lekérdezés jelenleg műveletnek egy művelet hívása hello szűrő tömb, hello Designer elérhetővé. Ez lehetővé teszi egy tömb tooquery, és vissza szűrt eredmények.

Itt látható, hogyan adhat hozzá azt a logikai alkalmazást:

1. Jelölje be hello **új lépés** gombra.
2. Válasszon **művelet hozzáadása**.
3. Hello művelet keresési mezőbe, írja be a **szűrő** toolist hello **szűrő tömb** művelet.
   
    ![Válassza ki a hello lekérdezési művelet](./media/connectors-native-query/using-action-1.png)
4. Válasszon egy tömb toofilter. (hello következő képernyőfelvétel egy Twitter-keresés eredményei hello tömbje.)
5. Hozzon létre egy feltétel tooevaluate minden elemhez. (a következő képernyőkép hello szűrők a felhasználók, akik több mint 100 followers Twitter-üzeneteket.)
   
    ![Teljes hello lekérdezési művelet](./media/connectors-native-query/using-action-2.png)
   
    hello művelet kimeneti fog egy új, csak a hello szűrő megfelel eredményeket tartalmazó tömb.
6. Kattintson a bal felső sarkának hello hello eszköztár toosave, és a logic app fog egyaránt mentés, és tegye közzé (aktiválása).

## <a name="query-action"></a>Lekérdezési művelet
Az alábbiakban hello részleteit, amely támogatja ezt az összekötőt hello a művelethez. hello összekötő lehetséges egy-egy művelettel rendelkezik.

| Műveletek | Leírás |
| --- | --- |
| Szűrő tömb |Minden elem tömbben feltételt ad meg, és hello eredményt ad vissza. |

## <a name="action-details"></a>A művelet részletei
hello lekérdezési művelet egy lehetséges műveletet tartalmaz. hello alábbi táblázatok bemutatják az hello szükséges és választható beviteli mezők hello művelet és hello megfelelő kimeneti részletekért társított hello műveletével.

### <a name="filter-array"></a>Szűrő tömb
Az alábbiakban hello hello művelet, amely a kimenő HTTP-kérelem a beviteli mezők.
A * azt jelenti, hogy mezőt kötelező kitölteni.

| Megjelenített név | Tulajdonság neve | Leírás |
| --- | --- | --- |
| A * |A |hello tömb toofilter |
| Az állapot * |Ha |hello feltétel tooevaluate minden elemhez |

<br>

### <a name="output-details"></a>Kimeneti részletei
Az alábbiakban hello kimeneti részletes hello HTTP-válasz.

| Tulajdonság neve | Adattípus | Leírás |
| --- | --- | --- |
| Szűrt tömb |A tömb |Olyan tömb, amely minden szűrt eredmény objektumot tartalmaz |

## <a name="next-steps"></a>Következő lépések
Most, próbálja ki hello platform és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Ismerje meg a hello más rendelkezésre álló összekötők logic Apps alkalmazások felmérésével a [API-k lista](apis-list.md).

