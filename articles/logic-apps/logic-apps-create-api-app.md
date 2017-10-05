---
title: "Összekötők - Azure Logic Apps webes API-k & REST API-k létrehozása |} Microsoft Docs"
description: "Webes API-k & REST API-k hívására az API-k, a szolgáltatások vagy a rendszer-munkafolyamatok rendszer Integrációk az Azure Logic Apps hozható létre"
keywords: "webes API-k, a REST API-k, a összekötőket, a munkafolyamatok, a rendszer integrációja"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: bd229179-7199-4aab-bae0-1baf072c7659
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/26/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 4ae98804aced23c0261c1d58721cb18d8152c6f1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-custom-apis-as-connectors-for-logic-apps"></a>Hozzon létre egyéni API-k összekötők logic Apps alkalmazások számára

Bár az Azure Logic Apps kínál [100 + beépített összekötők](../connectors/apis-list.md) , hogy a logic app munkafolyamatokban is használhat, érdemes hívja az API-k, rendszerek és szolgáltatásokról, amelyek nem érhetők el összekötők. A saját egyéni API-műveletek és eseményindítók használata a logic Apps alkalmazásokat hozhat létre. Az alábbiakban okokat, hogy miért érdemes a saját API-k használata a logic apps összekötők létrehozásához:

* A jelenlegi rendszer-integrációval és adatok integrációs munkafolyamatok kiterjesztése.
* Segít a felhasználóknak a szolgáltatás segítségével professional vagy személyes feladatok kezelése.
* Bontsa ki a reach felderíthetőség és a szolgáltatás használatát.

Összekötők alapvetően webes API-k, moduláris felületek, a többi használó [Swagger-metaadatok formátumú](http://swagger.io/specification/) dokumentációját, és JSON, az adatcsere-formátumot. Mivel összekötők REST API-k, amelyek a HTTP-végpontokról keresztül kommunikál egymással, a ismeretcikkekkel, például a .NET, Java vagy Node.js, összekötők készítéséhez is használhatja. Az API-kat is tartozhat a [Azure App Service](../app-service/app-service-value-prop-what-is.md), a platform-,-a-szolgáltatásajánlat (PaaS), amely biztosítja a legjobb, a legegyszerűbb és a leginkább méretezhető módszerek valamelyikével API üzemeltetéséhez. 

A egyéni API-k a logic apps működjön, az API-biztosíthat [ *műveletek* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) , amelyek folyamatban speciális feladatokat a logic app munkafolyamatokban. Az API-t is működhet, és egy [ *eseményindító* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) , elindul a logic app munkafolyamat, amikor új adatokat vagy egy esemény megfelel-e a megadott feltétel. Ez a témakör ismerteti a gyakori mintázatok, amelyeket követve műveletek és az API-ban, az API-t adja meg a kívánt viselkedése alapján eseményindítók készítéséhez.

> [!TIP] 
> Bár az API-k telepítése [webalkalmazások](../app-service-web/app-service-web-overview.md), fontolja meg az API-k telepítésével [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md), amely megkönnyítheti a projekt felépítéséhez, tárolására és felhasználása a felhőben és helyszíni API-k. Nincs módosíthatja bármely kódot az API-k – csupán telepítse kódját egy API-alkalmazásba. Megtudhatja, hogyan [létrehozni ASP.NET API-alkalmazások készítéséhez](../app-service-api/app-service-api-dotnet-get-started.md), [Java](../app-service-api/app-service-api-java-api-app.md), vagy [Node.js](../app-service-api/app-service-api-nodejs-api-app.md). 
>
> API-alkalmazás minták a logic apps készült, látogasson el a [Azure Logic Apps GitHub-tárházban](http://github.com/logicappsio) vagy [blog](http://aka.ms/logicappsblog).

## <a name="helpful-tools"></a>Hasznos eszközök

Egy egyéni API optimális működéséhez a logic apps az API-t is rendelkezik egy [Swagger-dokumentum](http://swagger.io/specification/) , amely az API-t műveleteket és paramétereket ismerteti.
Sok szalagtárak, például [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle), automatikusan elő tud állítani a Swagger-fájl meg. A megjelenített nevek, tulajdonságtípus és így tovább a Swagger-fájl a jegyzet használhatja [TRex](https://github.com/nihaue/TRex) , hogy a logic apps jól működik a Swagger-fájl.

<a name="actions"></a>

## <a name="action-patterns"></a>A művelet minták

A logic apps feladatok elvégzéséhez, biztosítania kell az egyéni API [ *műveletek*](./logic-apps-what-are-logic-apps.md#logic-app-concepts). Az API-t az egyes műveletek művelet van leképezve. Egy alapszintű művelet eredménye egy tartományvezérlőre, amely elfogadja a HTTP-kérelmek és HTTP-válaszokat ad vissza. Így például a logikai alkalmazás HTTP kérelmet küld a webes alkalmazás vagy API-alkalmazás. Az alkalmazás ezután egy HTTP-válasz együtt, amely a logikai alkalmazás képes tartalmat adja vissza.

Szabványos tevékenység írja be az API-t egy HTTP-kérési metódust, és ez a módszer a Swagger-fájl leírása. Majd hívása közvetlenül az API-t egy [HTTP-művelet](../connectors/connectors-native-http.md) vagy egy [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) művelet. Alapértelmezés szerint válaszok belül vissza kell a [kérelem időkorlátja](./logic-apps-limits-and-config.md). 

![Standard műveleti minta](./media/logic-apps-create-api-app/standard-action.png)

<a name="pattern-overview"></a>Várjon, amíg az API-befejezése hosszabb ideig futó feladatok logikai alkalmazás, az API-t is hajtsa végre a [lekérdezési aszinkron mintát](#async-pattern) vagy a [webhook aszinkron mintát](#webhook-actions) a jelen témakörben ismertetett. Egy analógia, amelynek segítségével jelenítheti meg az ezen mintákat műveletet képzelhető el, a folyamat egy magát az egyéni torta rendezéshez. A lekérdezési mintát tükrözi a viselkedést, ha meghívja a magát 20 percenként ellenőrizze, hogy a torta készen áll-e. A webhook mintát a viselkedést, ahol a magát kéri a telefonszámát, azok hívhatják meg, amikor készen áll a torta tükrözi.

Mintát, keresse fel a [Logic Apps GitHub-tárházban](https://github.com/logicappsio). Emellett további információ [használatmérés műveletekhez](logic-apps-pricing.md).

<a name="async-pattern"></a>

### <a name="perform-long-running-tasks-with-the-polling-action-pattern"></a>A lekérdezési művelet mintával hosszú ideig futó feladatok végrehajtása

Az API-t, feladatokat futtathat hosszabb, mint hogy a [kérelem időkorlátja](./logic-apps-limits-and-config.md), használhatja a lekérdezési aszinkron mintát. Ebben a mintában az API tegye működni egy külön szálban, de tartsa a Logic Apps motor aktív kapcsolat van. Ily módon a a logikai alkalmazást nem nem túllépi az időkorlátot, és folytassa a következő lépéssel a munkafolyamat, az API-működő befejezése előtt.

Az általános mintát itt található:

1. Győződjön meg arról, hogy a vezérlő tudja, hogy az API-elfogadta a kérést, és megkezdte működését.
2. Amikor a motor későbbi kérelmek feladat állapotát, lehetővé teszik az API-t a feladat befejezése után tudja a motor.
3. Térjen vissza a vonatkozó adatokat a motor, hogy a logic app munkafolyamat továbbra is.

<a name="bakery-polling-action"></a>Most a korábbi magát hasonlóan az lekérdezési szabályt alkalmazza, és képzelhető el, hogy hívjon egy magát és egy rendezési egyéni torta kézbesítésre. A folyamat, hogy a torta időt vesz igénybe, és nem kívánja a telefonján türelmet, amíg a magát a torta működéséről. A magát megerősíti, hogy a rendelés, és a torta állapot 20 percenként hívása során. Után 20 perc átadni, meghívja a magát, de azok jelzi, hogy a torta nem történik, és, hogy meg kell hívni a másik 20 perc. Ez vissza oda-folyamat folytatódik, amíg nem hívja, és a magát tájékoztat, hogy a rendelés készen áll-e, és továbbítja a torta. 

Ezért képezze le a lekérdezési mintát vissza. A magát az egyéni API jelöli, amíg Ön, a torta ügyfél határoz meg a Logic Apps motor. A programmag meghívja az API-t egy kérelmet, ha az API-megerősíti, hogy a kérelmet, és válaszol az időtartam alatt, ha a motor ellenőrizheti a feladat állapotát. A motor továbbra is fennáll, addig, amíg az API-válaszol, hogy történik-e a feladat állapotának ellenőrzését, és visszaadja az adatokat a Logic Apps alkalmazást, majd folytatja a munkafolyamat. 

![Lekérdezési művelet minta](./media/logic-apps-create-api-app/custom-api-async-action-pattern.png)

Az API-követi, hogy adott lépései a következők az API-t szempontjából ismerteti:

1. Ha az API-t kap egy HTTP-kérelem indítsa el a munkahelyi, azonnal vissza HTTP `202 ACCEPTED` response a `location` ebben a lépésben ismertetett fejléc. Ez a válasz lehetővé teszi, hogy a Logic Apps motor tudja, hogy az API-t a kérés érkezett, elfogadta a kérést forgalma (bemeneti adatokat), és feldolgozása már folyamatban van. 
   
   A `202 ACCEPTED` válasz tartalmaznia kell ezek a fejlécek:
   
   * *Szükséges*: A `location` fejlécet tartalmazta, amely egy URL-címet, ahol a Logic Apps motor ellenőrizheti az API-feladat állapotának abszolút elérési útja

   * *Nem kötelező*: A `retry-after` fejlécet tartalmazta, amely megvárja-e a motor bejelölése előtt másodpercek számát adja meg a `location` feladatállapot URL-címe. 

     Alapértelmezés szerint a motor minden 20 másodperc ellenőrzi. Adjon meg egy másik időközt, közé tartozik a `retry-after` fejléc, és csak a következő lekérdezési másodpercben.

2. A megadott idő letelte után a Logic Apps motor szavazások a `location` URL-feladat állapotának ellenőrzése. Az API-t kell ezen ellenőrzések elvégzéséhez és a ezeket a válaszokat vissza:
   
   * Ha a feladat kész, térjen vissza a HTTP `200 OK` választ, a válasz forgalma (a következő lépés a megadott) együtt.

   * Ha a feladat még dolgozik, adja vissza egy másik HTTP `202 ACCEPTED` válasz, azonban csak az azonos fejlécek az eredeti válaszként.

Az API-t ezt a mintát követi, amikor nincs tennivalója a logic app munkafolyamat-definíciót is feladat állapotát ellenőrzi. Ha a motor lekérdezi HTTP `202 ACCEPTED` válasz és egy érvényes `location` fejléc, a motor tiszteletben tartja az aszinkron mintát, és ellenőrzi a `location` fejléc, amíg az API nem 202 választ ad.

> [!TIP]
> Egy példa aszinkron mintát, tekintse meg a [aszinkron vezérlő válasz mintát a Githubon](https://github.com/logicappsio/LogicAppsAsyncResponseSample).

<a name="webhook-actions"></a>

### <a name="perform-long-running-tasks-with-the-webhook-action-pattern"></a>A webhook művelet mintával hosszú ideig futó feladatok végrehajtása

Alternatív megoldásként használhatja a webhook mintát hosszan futó feladatokat és az aszinkron feldolgozás. Ebben a mintában az API-n befejezze a feldolgozást munkafolyamat folytatása előtt a logikai alkalmazás "callback" Várjon rendelkezik. A visszahívási egy HTTP POST, amelyek üzenetet küld egy URL-címet, ha olyan esemény történik. 

<a name="bakery-webhook-action"></a>Most a korábbi magát hasonlóan a webhook mintát vonatkoznak, és képzelhető el, hogy hívjon egy magát és egy rendezési egyéni torta kézbesítésre. A folyamat, hogy a torta időt vesz igénybe, és nem kívánja a telefonján türelmet, amíg a magát a torta működéséről. A magát megerősíti, hogy a rendelés, de ezúttal meg számukra a telefonszámát, azok hívhatják meg, amikor a torta hajtja végre. Ezúttal a magát megtudhatja, ha a sorrend készen áll, és továbbítja a torta.

Azt leképezni a webhook mintát vissza, ha a magát az egyéni API jelöli, amíg Ön, a torta ügyfél határoz meg a Logic Apps-motor. A motor az API-t a kérelmet, és a "callback" URL-címet.
A feladat kész, amikor az API használatával értesíti a motor és a Logic Apps alkalmazást, majd folytatja a munkafolyamat adatokat az URL-címet. 

Ebben a mintában két végpontot a tartományvezérlőn beállított: `subscribe` és`unsubscribe`

*  `subscribe`végpont: a munkafolyamatban az API-művelet végrehajtása elérésekor a Logic Apps motor hívások a `subscribe` végpont. Ez a lépés azt eredményezi, a logikai alkalmazás létrehozása egy visszahívási URL-címet, amely az API-t tárolja, és majd várja meg az API-t a visszahívás során végzett. Az API-t, majd visszahívja az URL-címre egy HTTP POST, és minden visszaadott tartalom és a fejlécek továbbítja a logikai alkalmazás bemenetként.

* `unsubscribe`végpont: a logikai alkalmazást futtatni megszűnik, ha a Logic Apps motor hívások a `unsubscribe` végpont. Az API-regisztrációjának törlése a visszahívási URL-címet, majd állítsa le a folyamatokat, szükség szerint.

![Webhook műveleti minta](./media/logic-apps-create-api-app/custom-api-webhook-action-pattern.png)

> [!NOTE]
> Jelenleg a Logic App tervező nem támogatja a felderítő webhook végpontok Swagger keresztül. Így az ebben a mintában fel kell vennie egy [ **Webhook** művelet](../connectors/connectors-native-webhook.md) , és adja meg az URL-címe, fejlécek és a kérés törzsében. Lásd még: [munkafolyamat-műveleteket és eseményindítók](logic-apps-workflow-actions-triggers.md#api-connection-webhook-action). A visszahívási URL-ben továbbítani, használhatja a `@listCallbackUrl()` szükség szerint az előző mezőinek munkafolyamat-funkció.

> [!TIP]
> Tekintse át egy példa a webhook mintában ez [webhook eseményindító mintát a Githubon](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

<a name="triggers"></a>

## <a name="trigger-patterns"></a>Eseményindító minták

Az egyéni API működhet, és egy [ *eseményindító* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) , amely a logikai alkalmazás elindul, és amikor új adatok vagy egy esemény megfelel-e a megadott feltétel. Ehhez az eseményindítóhoz is vagy rendszeresen figyelni, vagy várjon, amíg és figyelni, az új adatokat és eseményeket a szolgáltatási végpont. Ha új adatokat vagy egy esemény megfelel a megadott feltételnek, az eseményindító következik be, és elindítja a logikai alkalmazást, amely adott eseményindító figyel. A logic apps ily módon indul el, az API-kövesse a [ *lekérdezési eseményindító* ](#polling-triggers) vagy a [ *webhook eseményindító* ](#webhook-triggers) mintát. Ezek a minták hasonlóak a mint a [lekérdezési műveletek](#async-pattern) és [webhookműveletek](#webhook-actions). Emellett további információ [az eseményindítók a használatmérés](logic-apps-pricing.md).

<a name="polling-triggers"></a>

### <a name="check-for-new-data-or-events-regularly-with-the-polling-trigger-pattern"></a>Ellenőrizze az új adatokat és eseményeket rendszeresen a lekérdezési eseményindító mintával

A *lekérdezési eseményindító* hasonlóan működik a [lekérdezési művelet](#async-pattern) ebben a témakörben korábban leírt. A Logic Apps motor rendszeresen hívja, és ellenőrzi az eseményindító végpont az új adatokat és eseményeket. Ha a motor új adatokat vagy egy eseményt, amely megfelel a megadott feltétel, az eseményindító következik be. Ezt követően a motor hoz létre egy logic app-példány bemeneti adatokat feldolgozó. 

![Lekérdezési eseményindító minta](./media/logic-apps-create-api-app/custom-api-polling-trigger-pattern.png)

> [!NOTE]
> Egyes lekérdezési kérelmek egy művelet végrehajtási számít, akkor is, ha nincs logic app-példány jön létre. Ugyanazokat az adatokat többször feldolgozása érdekében az eseményindító már elolvasta és a logikai alkalmazás átadott adatok kell tisztítása.

Az alábbiakban lépéseit a API szempontjából leírt lekérdezési eseményindító:

| Új adatok vagy esemény található?  | API-válaszból | 
| ------------------------- | ------------ |
| Található | Térjen vissza a HTTP `200 OK` , ahol a válasz forgalma (a következő lépésben megadott) állapot. <br/>Ez a válasz hoz létre egy logic app-példány, és elindítja a munkafolyamatot. |
| Nem található | Térjen vissza a HTTP `202 ACCEPTED` állapot egy `location` fejléc és a `retry-after` fejléc. <br/>Az eseményindítók a `location` fejléc is tartalmaznia kell egy `triggerState` lekérdezési paraméter, amely általában a "időbélyege." Az API-Ez az azonosító segítségével nyomon követheti a legutóbbi biztonsági lett elindítva. a logikai alkalmazást. |

A szolgáltatás az új fájlok rendszeres időnként ellenőrzik, például egy lekérdezési eseményindítót, amelyen ezek közül a viselkedésmódok összeállítása előfordulhat, hogy:

| Kérelemben `triggerState`? | API-válaszból |
| -------------------------------- | -------------|
| Nem | Térjen vissza a HTTP `202 ACCEPTED` állapot plusz a `location` fejléc a következő `triggerState` állítsa be az aktuális idő és a `retry-after` időköz 15 másodpercre. |
| Igen | Ellenőrizze a szolgáltatást a fájlok után a `DateTime` a `triggerState`. |

| Található fájlok száma | API-válaszból |
| --------------------- | -------------|
| Egyetlen fájl | Térjen vissza a HTTP `200 OK` állapotát és a tartalom tartalom frissítése `triggerState` való a `DateTime` a visszaadott fájlt, és a beállított `retry-after` időköz 15 másodpercre. |
| Több fájl | Válaszként egy fájlt egy időben és HTTP `200 OK` állapot, a frissítés `triggerState`, és állítsa be a `retry-after` 0 másodperc időközt. </br>Ezeket a lépéseket lehetővé teszik a motor tudja, hogy több adatot, és, hogy a motor kell azonnal kérhet az adatok az URL-címet a `location` fejléc. |
| Nincsenek fájlok | Térjen vissza a HTTP `202 ACCEPTED` állapota, ne módosítsa `triggerState`, és állítsa be a `retry-after` időköz 15 másodpercre. |

> [!TIP]
> Például egy lekérdezési eseményindító mintát, tekintse át a [lekérdezési eseményindító vezérlő mintát a Githubon](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/PollTriggerController.cs).

<a name="webhook-triggers"></a>

### <a name="wait-and-listen-for-new-data-or-events-with-the-webhook-trigger-pattern"></a>Várja meg, és az új adatokat és eseményeket a webhook eseményindító mintával figyelésére

A webhook trigger egy *leküldéses eseményindító* , amely megvárja-e, és az új adatokat és eseményeket a szolgáltatási végpont figyel. Ha új adatokat vagy egy esemény megfelel a megadott feltételnek, az eseményindító következik be, és létrehoz egy logic app példány, majd dolgozza fel a bemeneti adatok.
Webhook eseményindítók működésre hasonlóan a [webhookműveletek](#webhook-actions) korábban a jelen témakörben ismertetett, és be vannak állítva a `subscribe` és `unsubscribe` végpontok. 

* `subscribe`végpont: hozzáadása, és mentse egy webhook eseményindító a Logic Apps alkalmazást, a Logic Apps motor hívások a `subscribe` végpont. Ez a lépés azt eredményezi, a logikai alkalmazás létrehozása egy visszahívási URL-címet, amely az API-t tárolja. Amikor új adatok vagy egy eseményt, amely megfelel a megadott feltétel, az API-visszahívja az URL-címre egy HTTP POST. A tartalom forgalma és a fejlécek adja át a logikai alkalmazás bemenetként.

* `unsubscribe`végpont: a webhook eseményindító vagy a teljes logikai alkalmazás törlése, ha a Logic Apps motor hívások a `unsubscribe` végpont. Az API-regisztrációjának törlése a visszahívási URL-címet, majd állítsa le a folyamatokat, szükség szerint.

![Webhook eseményindító minta](./media/logic-apps-create-api-app/custom-api-webhook-trigger-pattern.png)

> [!NOTE]
> Jelenleg a Logic App tervező nem támogatja a felderítő webhook végpontok Swagger keresztül. Így az ebben a mintában fel kell vennie egy [ **Webhook** eseményindító](../connectors/connectors-native-webhook.md) , és adja meg az URL-címe, fejlécek és a kérés törzsében. Lásd még: [HTTPWebhook eseményindító](logic-apps-workflow-actions-triggers.md#httpwebhook-trigger). A visszahívási URL-ben továbbítani, használhatja a `@listCallbackUrl()` szükség szerint az előző mezőinek munkafolyamat-funkció.
>
> Ugyanazokat az adatokat többször feldolgozása érdekében az eseményindító már elolvasta és a logikai alkalmazás átadott adatok kell tisztítása.

> [!TIP]
> Tekintse át egy példa a webhook mintában ez [webhook eseményindító vezérlő mintát a Githubon](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

## <a name="deploy-call-and-secure-custom-apis"></a>Központi telepítése, hívja, és biztonságos egyéni API-k

Miután létrehozta az egyéni API-kat, beállítása az API-kat, központi telepítés biztonságosan keresheti őket. Megtudhatja, hogyan [központi telepítése, hívja, és egyéni API-k a logic Apps alkalmazások biztonságos](./logic-apps-custom-hosted-api.md).

## <a name="publish-custom-apis-to-azure"></a>Egyéni API-k közzététele az Azure-bA

Az egyéni API-kat nyilvános elérhetővé teszi az Azure-ban, hogy küldje el a kijelölését, hogy a [hivatalos Microsoft Azure program](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/).

## <a name="get-help"></a>Segítségkérés

Egyéni API-k meghatározott kapcsolatban forduljon [ customapishelp@microsoft.com ](mailto:customapishelp@microsoft.com).

Látogasson el az [Azure Logic Apps fórumára](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps), ahol kérdéseket tehet fel és válaszolhat meg, valamint megtudhatja, mivel foglalkoznak az Azure Logic Apps más felhasználói.

Ha szeretne segíteni a Logic Apps és összekötők fejlesztésében, szavazzon vagy küldje el javaslatait a [Logic Apps felhasználói visszajelzések oldalon](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Következő lépések

* [A használatmérés műveletek és eseményindítók](logic-apps-pricing.md)
* [Tartalomtípusok kezelése](./logic-apps-content-type.md)
* [Hibák és kivételek kezelése](./logic-apps-exception-handling.md)
* [Biztonságos hozzáférés a logic Apps alkalmazásait](./logic-apps-securing-a-logic-app.md)
* [Hívja, eseményindító, vagy a logic apps a HTTP-végpontokról egymásba](./logic-apps-http-endpoint.md)