---
title: "az Azure Logic Apps aaaWebhook összekötő |} Microsoft Docs"
description: "Hogyan toouse webhookműveletek és eseményindítók tooperform műveletek, például szűrő tömb a logic Apps alkalmazásokból"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: b2dee12750f3f20f10e7b257da05a79f28f90f43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-webhook-connector"></a>Hello webhook összekötő az első lépései

Hello webhook művelet és eseményindító elindítása, szüneteltetése és folytatása adatfolyamok tooperform ezeket a feladatokat:

* Az indítás egy [Azure Event Hubs](https://github.com/logicappsio/EventHubAPI) amikor egy elem érkezik.
* Várjon, amíg a munkafolyamat folytatása előtt a jóváhagyás

További információ [hogyan toocreate egyéni API-k, amelyek támogatják a webhook](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-hello-webhook-trigger"></a>Hello webhook eseményindító használata

A [ *eseményindító* ](connectors-overview.md) olyan esemény, amely a logic app munkamenet indítása. A webhook eseményindító eseményalapú, és nem függ a lekérdezési új elemek. Például a hello [kérelem eseményindító](connectors-native-reqres.md), hello logikai alkalmazás hello azonnali történik, hogy az esemény következik be. hello webhook eseményindító regisztrál egy *visszahívási URL-cím* tooa szolgáltatás és -felhasználási adott URL-cím toofire hello logikai alkalmazás szerint szükséges.

Íme egy példa bemutatja, hogyan tooset HTTP be trigger hello Logic App Designer. hello lépések azt feltételezik, hogy már telepítették, vagy az alábbi hello API-k elérésére [webhook szolgáltatásra, és a logic apps mintát leiratkozhat](../logic-apps/logic-apps-create-api-app.md#webhook-triggers). hello előfizetés hívás történik, amikor a logikai alkalmazás egy új webhook mentése, vagy átváltott a letiltott tooenabled. hello leiratkozhat hívás legyen a logic app webhook eseményindító van eltávolítva és mentésekor, illetve engedélyezett toodisabled váltott.

**tooadd hello webhook eseményindító**

1. Adja hozzá a hello **HTTP Webhook** hello első lépéseként logikai alkalmazás eseményindító.
2. Töltse ki hello paraméterek, a hello webhook szolgáltatásra, és hívások leiratkozhat.

   Ez a lépés a következő ugyanaz, mint a hello mintát hello [HTTP-művelet](connectors-native-http.md) formátumban.

     ![HTTP-eseményindítóval](./media/connectors-native-webhook/using-trigger.png)

3. Legalább egy műveletet hozzá.
4. Kattintson a **mentése** toopublish hello logikai alkalmazást. E lépés hívások hello előfizetés hello visszahívási URL-cím szükséges tootrigger végpont a logikai alkalmazást.
5. Amikor a szolgáltatás elérhetővé válnak hello egy `HTTP POST` toohello visszahívási URL-címe, hello logic app következik be, és bármely hello kérelem átadott adatokat tartalmazza.

## <a name="use-hello-webhook-action"></a>Hello webhook művelettel

Egy [ *művelet* ](connectors-overview.md) van egy művelet által végzett meghatározott logikai alkalmazás hello munkafolyamat. A webhook művelet regisztrálja a *visszahívási URL-cím* egy szolgáltatás, és megvárja, amíg hello URL-cím neve folytatása előtt. Hello ["E-mail küldése a jóváhagyási"](connectors-create-api-office365-outlook.md) csatlakozó, amely ebben a mintában a következő példát. Ebben a mintában kiterjesztheti keresztül hello webhook művelet bármely szolgáltatásba. 

Íme egy példa bemutatja, hogyan tooset egy webhook műveletet a hello Logic App Tervező. Ezek a lépések feltételezik, hogy már telepítették, vagy az alábbi hello API-k elérésére [webhook szolgáltatásra, és a logic apps használt minta leiratkozhat](../logic-apps/logic-apps-create-api-app.md#webhook-actions). hello előfizetés hívás hello webhook művelet végrehajtásakor a logikai alkalmazás történik. hello leiratkozhat hívás futtató megszakadt a válaszra való várakozás során, vagy előtt hello logic app időtúllépés történik.

**a webhook művelet tooadd**

1. Válasszon **új lépés** > **művelet hozzáadása**.

2. Hello keresési mezőbe, írja be a "webhook" toofind hello **HTTP Webhook** művelet.

    ![Válassza ki a lekérdezési művelet](./media/connectors-native-webhook/using-action-1.png)

3. Hello paraméterek töltse ki a hello webhook szolgáltatásra, és leiratkozhat hívások

   Ez a lépés a következő ugyanaz, mint a hello mintát hello [HTTP-művelet](connectors-native-http.md) formátumban.

     ![Teljes lekérdezési művelet](./media/connectors-native-webhook/using-action-2.png)
   
   Alkalmazása futásidőben hello logic app hívások hello fizessen elő a végpont elérése a lépés után.

4. Kattintson a **mentése** toopublish hello logikai alkalmazást.

## <a name="technical-details"></a>Technikai részletek

Az alábbiakban további részleteket hello eseményindítók és műveletek adott webhook támogatja.

## <a name="webhook-triggers"></a>Webhook eseményindítók

| Műveletek | Leírás |
| --- | --- |
| HTTP Webhook |Az előfizetés visszahívási URL-cím tooa szolgáltatás meghívhatja hello URL-cím toofire logikai alkalmazás igény szerint. |

### <a name="trigger-details"></a>Eseményindító részletei

#### <a name="http-webhook"></a>HTTP Webhook

Az előfizetés visszahívási URL-cím tooa szolgáltatás meghívhatja hello URL-cím toofire logikai alkalmazás igény szerint.
Egy * azt jelenti, hogy a mezőt kötelező kitölteni.

| Megjelenített név | Tulajdonság neve | Leírás |
| --- | --- | --- |
| Előfizetés metódus * |Módszer |HTTP-metódus toouse előfizetés a kérelemhez |
| Előfizetés URI * |URI |Előfizetés a kérelemhez HTTP URI toouse |
| Leiratkozhat metódus * |Módszer |HTTP-metódus toouse lemondás kérelem |
| Leiratkozhat URI * |URI |A HTTP URI toouse lemondás kérelem |
| Fizessen elő a szervezet |Törzs |Az előfizetés HTTP-kérelem törzsében |
| Fejlécek előfizetés |Fejlécek |HTTP-kérelmek fejléceinek előfizetés |
| Hitelesítési előfizetés |Hitelesítés |HTTP-hitelesítés toouse az előfizetés. [Tekintse meg a HTTP-összekötő](connectors-native-http.md#authentication) részletek |
| Törzs leiratkozhat |Törzs |HTTP kérelem törzse lemondás |
| Fejlécek leiratkozhat |Fejlécek |HTTP-kérelmek fejléceinek lemondás |
| Hitelesítési leiratkozhat |Hitelesítés |HTTP-hitelesítés toouse a lemondás. [Tekintse meg a HTTP-összekötő](connectors-native-http.md#authentication) részletek |

**Kimeneti részletei**

Webhook kérelem

| Tulajdonság neve | Adattípus | Leírás |
| --- | --- | --- |
| Fejlécek |Objektum |Webhook kérelemfejléc |
| Törzs |Objektum |Webhook kérelem objektum |
| Állapotkód |int |Webhook kérésállapotkódot |

## <a name="webhook-actions"></a>Webhookműveletek

| Műveletek | Leírás |
| --- | --- |
| HTTP Webhook |Az előfizetés visszahívási URL-cím tooa szolgáltatás meghívhatja hello URL-cím tooresume egy munkafolyamat-lépés igény szerint. |

### <a name="action-details"></a>A művelet részletei

#### <a name="http-webhook"></a>HTTP Webhook

Az előfizetés visszahívási URL-cím tooa szolgáltatás meghívhatja hello URL-cím tooresume egy munkafolyamat-lépés igény szerint.
Egy * azt jelenti, hogy a mezőt kötelező kitölteni.

| Megjelenített név | Tulajdonság neve | Leírás |
| --- | --- | --- |
| Előfizetés metódus * |Módszer |HTTP-metódus toouse előfizetés a kérelemhez |
| Előfizetés URI * |URI |Előfizetés a kérelemhez HTTP URI toouse |
| Leiratkozhat metódus * |Módszer |HTTP-metódus toouse lemondás kérelem |
| Leiratkozhat URI * |URI |A HTTP URI toouse lemondás kérelem |
| Fizessen elő a szervezet |Törzs |Az előfizetés HTTP-kérelem törzsében |
| Fejlécek előfizetés |Fejlécek |HTTP-kérelmek fejléceinek előfizetés |
| Hitelesítési előfizetés |Hitelesítés |HTTP-hitelesítés toouse az előfizetés. [Tekintse meg a HTTP-összekötő](connectors-native-http.md#authentication) részletek |
| Törzs leiratkozhat |Törzs |HTTP kérelem törzse lemondás |
| Fejlécek leiratkozhat |Fejlécek |HTTP-kérelmek fejléceinek lemondás |
| Hitelesítési leiratkozhat |Hitelesítés |HTTP-hitelesítés toouse a lemondás. [Tekintse meg a HTTP-összekötő](connectors-native-http.md#authentication) részletek |

**Kimeneti részletei**

Webhook kérelem

| Tulajdonság neve | Adattípus | Leírás |
| --- | --- | --- |
| Fejlécek |Objektum |Webhook kérelemfejléc |
| Törzs |Objektum |Webhook kérelem objektum |
| Állapotkód |int |Webhook kérésállapotkódot |

## <a name="next-steps"></a>Következő lépések

* [Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md)
* [Más összekötők keresése](apis-list.md)