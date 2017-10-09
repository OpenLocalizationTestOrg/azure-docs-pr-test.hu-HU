---
title: "kérelem és válasz műveletek aaaUse |} Microsoft Docs"
description: "Hello kérelem-válasz eseményindítója és tevékenysége fel az Azure logic App áttekintése"
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 566924a4-0988-4d86-9ecd-ad22507858c0
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 24c378cc12d5f3f65116d5e59278236186a99662
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-request-and-response-components"></a>Első lépések hello kérés- és összetevői
Hello kérelem-válasz összetevőket logikai alkalmazás reagálhat a valós idejű tooevents.

Megteheti például a következőt:

* Válaszol a HTTP-kérelem tooan adatokkal keresztül logikai alkalmazás a helyi adatbázisból.
* Indítás, a logikai alkalmazást egy külső webhook eseményből.
* Hívja meg a logikai alkalmazás belül egy másik logikai alkalmazás a kérelem-válasz művelettel.

tooget használatának hello kérelem-válasz műveletek a logikai alkalmazás, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-hello-http-request-trigger"></a>HTTP-kérelem eseményindító hello használata
Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény. [További tudnivalók az eseményindítók](connectors-overview.md).

Íme egy parancssorozat-példa hogyan tooset be HTTP hello Logic App Designer kérheti.

1. Adja hozzá a hello eseményindító **kérelem - amikor egy HTTP-kérelem érkezik** a Logic Apps alkalmazást a. Is megadhat egy JSON-séma (például egy olyan eszközzel [JSONSchema.net](http://jsonschema.net)) a hello kérés törzsében. Ez lehetővé teszi tulajdonságok hello Tervező toogenerate jogkivonatainak hello HTTP-kérelem.
2. Adja hozzá egy másik művelet, így hello logikai alkalmazás mentheti.
3. Hello logikai alkalmazást a mentés után a hello HTTP-kérelem URL-CÍMÉT az hello kérelem kártya kérheti le.
4. Egy HTTP POST (például egy eszközzel [Postman](https://www.getpostman.com/)) toohello URL-cím eseményindítók hello logikai alkalmazást.

> [!NOTE]
> Ha nem adja meg egy válasz művelet egy `202 ACCEPTED` azonnal érkezik válasz toohello hívó. Hello válasz művelet toocustomize választ is használhatja.
> 
> 

![Válasz eseményindító](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-hello-http-response-action"></a>HTTP-válasz művelet hello használata
hello HTTP-válasz művelet csak akkor érvényes, ha egy munkafolyamatban, amely egy HTTP-kérés használhatja. Ha nem adja meg egy válasz művelet egy `202 ACCEPTED` azonnal érkezik válasz toohello hívó.  A válasz művelet hello munkafolyamaton belül bármely lépése is hozzáadhat. hello logikai alkalmazás csak fenntartja hello bejövő kérelem választ egy percig.  Ha nem választ küldött hello munkafolyamat (és hello definíciója van a válasz művelet), egy perc után egy `504 GATEWAY TIMEOUT` toohello hívó adja vissza.

Itt hogyan tooadd egy HTTP-válasz műveletet:

1. Jelölje be hello **új lépés** gombra.
2. Válasszon **művelet hozzáadása**.
3. Hello művelet keresési mezőbe, írja be a **válasz** toolist hello válasz művelet.
   
    ![Válassza ki a hello válasz művelet](./media/connectors-native-reqres/using-action-1.png)
4. Adja hozzá a HTTP-válaszüzenetnek hello szükséges paramétereket.
   
    ![Teljes hello válasz művelet](./media/connectors-native-reqres/using-action-2.png)
5. Kattintson a bal felső sarkának hello hello eszköztár toosave, és a logic app fog egyaránt mentés, és tegye közzé (aktiválása).

## <a name="request-trigger"></a>Kérelem eseményindító
Az alábbiakban hello részletek hello eseményindító, amely támogatja ezt az összekötőt. Nincs egyetlen kérelem eseményindítót.

| Eseményindító | Leírás |
| --- | --- |
| Kérés |Akkor következik be, amikor egy HTTP-kérelem érkezik |

## <a name="response-action"></a>Válasz művelet
Az alábbiakban hello részleteit, amely támogatja ezt az összekötőt hello a művelethez. Nincs olyan egyetlen válasz művelet, amely csak akkor használható, amikor egy kérelem eseményindító kíséri.

| Műveletek | Leírás |
| --- | --- |
| Válasz |Visszaadja egy válasz toohello korrelált HTTP-kérelem |

### <a name="trigger-and-action-details"></a>Eseményindítója és tevékenysége részletei
hello táblázatokban leíró hello bemeneti mezőinek hello eseményindítója és tevékenysége fel, és megfelelő kimeneti részletek hello.

#### <a name="request-trigger"></a>Kérelem eseményindító
hello egy beviteli mezőt a bejövő HTTP-kérelem hello eseményindító látható.

| Megjelenített név | Tulajdonság neve | Leírás |
| --- | --- | --- |
| JSON-séma |Séma |hello JSON-séma hello HTTP-kérelem törzse |

<br>

**Kimeneti részletei**

Az alábbiakban hello kimeneti részletes hello kérelem.

| Tulajdonság neve | Adattípus | Leírás |
| --- | --- | --- |
| Fejlécek |Objektum |Kérelem fejlécei |
| Törzs |Objektum |Kérelem objektum |

#### <a name="response-action"></a>Válasz művelet
Az alábbiakban hello hello HTTP-válasz művelet a beviteli mezők. A * azt jelenti, hogy mezőt kötelező kitölteni.

| Megjelenített név | Tulajdonság neve | Leírás |
| --- | --- | --- |
| Állapot kód * |statusCode |hello HTTP-állapotkód: |
| Fejlécek |Fejlécek |Minden válasz fejlécek tooinclude, egy JSON-objektum |
| Törzs |Törzs |hello választörzs |

## <a name="next-steps"></a>Következő lépések
Most, próbálja ki hello platform és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Ismerje meg a hello más rendelkezésre álló összekötők logic Apps alkalmazások felmérésével a [API-k lista](apis-list.md).

