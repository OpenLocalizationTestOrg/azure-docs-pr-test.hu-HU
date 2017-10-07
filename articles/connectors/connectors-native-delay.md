---
title: "a logic apps késés aaaAdd |} Microsoft Docs"
description: "Hello áttekintése késleltetés és a késleltetés-műveletek, amíg és hogyan toouse őket az az Azure Logic Apps alkalmazást."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 915f48bf-3bd8-4656-be73-91a941d0afcd
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: e5bc9d639adbddc01ee0f6a4c68716f586d4344a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-delay-and-delay-until-actions"></a>Ismerkedés a hello késleltetés és a késleltetés-amíg műveletek
Hello késleltetés használatával és a "késleltetés-amíg" műveletek, befejezheti a munkafolyamatokban.

Megteheti például a következőt:

* Várjon, amíg a hét napja toosend állapot frissítése keresztül e-mailt.
* Késleltetés hello munkafolyamat csak egy HTTP-hívás idő toofinish rendelkezik folytatása és hello eredmény beolvasása előtt.

tooget lépések hello késleltetés műveletével logikai alkalmazás, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-hello-delay-actions"></a>Hello késleltetés műveletek használata
Egy művelet során, amely logikai alkalmazás definiált hello munkafolyamat végzi. [További információ a műveletek](connectors-overview.md).

Íme egy parancssorozat-példa hogyan toouse késleltetést logikai alkalmazás lépést:

1. A felvett egy eseményindítót, kattintson a **új lépés** tooadd műveletet.
2. Keresse meg **késedelem** toobring hello késleltetés műveletek. Ebben a példában, azt fogja kiválasztani **késleltetés**.
   
    ![Késleltetés műveletek](./media/connectors-native-delay/using-action-1.png)
3. Hajtsa végre a hello művelet tulajdonságok tooconfigure hello késedelem.
   
    ![Késleltetési konfiguráció](./media/connectors-native-delay/using-action-2.png)
4. Kattintson a **mentése** toopublish és hello logikai alkalmazás aktiválása.

## <a name="action-details"></a>A művelet részletei
hello ismétlődési eseményindító következő konfigurálható tulajdonságai hello rendelkezik.

### <a name="delay-action"></a>Késleltetés művelet
Ez a művelet késleltetése hello futtassa egy meghatározott időtartam alatt.
A * azt jelenti, hogy mezőt kötelező kitölteni.

| Megjelenített név | Tulajdonság neve | Leírás |
| --- | --- | --- |
| Száma * |Száma |idő egységek toodelay hello száma |
| Egység * |egység |hello időegység: `Second`, `Minute`, `Hour`, vagy`Day` |

<br>

### <a name="delay-until-action"></a>Késleltetés-amíg művelet
Ez a művelet a megadott dátum és idő futását hello késlelteti.
A * azt jelenti, hogy mezőt kötelező kitölteni.

| Megjelenített név | Tulajdonság neve | Leírás |
| --- | --- | --- |
| Év * |időbélyeg |hello év toodelay amíg (GMT) |
| Hónap * |időbélyeg |hello hónap toodelay amíg (GMT) |
| Nap * |időbélyeg |hello nap toodelay amíg (GMT) |

<br>

## <a name="next-steps"></a>Következő lépések
Most, próbálja ki hello platform és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Ismerje meg a hello más rendelkezésre álló összekötők logic Apps alkalmazások felmérésével a [API-k lista](apis-list.md).

