---
title: "aaaCreate webes API-k & REST API-k összekötők - Azure Logic Apps |} Microsoft Docs"
description: "Az API-k, szolgáltatások és rendszerek hozzon létre webes API-k & REST API-k toocall rendszer Integrációk az Azure Logic Apps-munkafolyamatok"
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
ms.openlocfilehash: 2792204d1d298a6f810e75211c1789ee204c2fdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-custom-apis-as-connectors-for-logic-apps"></a>Hozzon létre egyéni API-k összekötők logic Apps alkalmazások számára

Bár az Azure Logic Apps kínál [100 + beépített összekötők](../connectors/apis-list.md) , hogy a logic app munkafolyamatokban is használhat, érdemes toocall API-k, rendszerek, és szolgáltatások, amelyek nem érhetők el, összekötők. A saját egyéni API-t adja meg a logic apps műveletek és eseményindítók toouse hozhat létre. Az alábbiakban okokat, hogy miért érdemes toocreate saját API-k túl összekötők logic Apps alkalmazások használják:

* A jelenlegi rendszer-integrációval és adatok integrációs munkafolyamatok kiterjesztése.
* Segít a felhasználóknak a szolgáltatás toomanage professional vagy személyes feladat használja.
* Bontsa ki a hello reach felderíthetőség és a szolgáltatás használatát.

Összekötők alapvetően webes API-k, moduláris felületek, a többi használó [Swagger-metaadatok formátumú](http://swagger.io/specification/) dokumentációját, és JSON, az adatcsere-formátumot. Mivel összekötők REST API-k, amelyek a HTTP-végpontokról keresztül kommunikál egymással, a ismeretcikkekkel, például a .NET, Java vagy Node.js, összekötők készítéséhez is használhatja. Az API-kat is tartozhat a [Azure App Service](../app-service/app-service-value-prop-what-is.md), a platform,--szolgáltatás (PaaS) ajánlat, amely nyújt hello legjobb legegyszerűbb és legjobban méretezhető módszerek valamelyikével API üzemeltetéséhez. 

A egyéni API-k toowork a logic apps, az API-biztosíthat [ *műveletek* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) , amelyek folyamatban speciális feladatokat a logic app munkafolyamatokban. Az API-t is működhet, és egy [ *eseményindító* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) , elindul a logic app munkafolyamat, amikor új adatokat vagy egy esemény megfelel-e a megadott feltétel. Ez a témakör ismerteti, amelyeket követve műveletek és az API-eseményindítók készítéséhez közös mintázatok, amelyet a API tooprovide hello viselkedés alapján.

> [!TIP] 
> Bár az API-k telepítése [webalkalmazások](../app-service-web/app-service-web-overview.md), fontolja meg az API-k telepítésével [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md), amely megkönnyítheti a projekt felépítéséhez, üzemeltetni, és felhasználhatják az API-k hello felhőben és a helyszíni. Nincs toochange összes kódot az API-k előnyeit – csupán telepítse a kód tooan API-alkalmazásba. Ismerje meg, hogyan túl [létrehozni ASP.NET API-alkalmazások készítéséhez](../app-service-api/app-service-api-dotnet-get-started.md), [Java](../app-service-api/app-service-api-java-api-app.md), vagy [Node.js](../app-service-api/app-service-api-nodejs-api-app.md). 
>
> A logic apps épített API-alkalmazás minták, a Microsoft hello [Azure Logic Apps GitHub-tárházban](http://github.com/logicappsio) vagy [blog](http://aka.ms/logicappsblog).

## <a name="helpful-tools"></a>Hasznos eszközök

Egy egyéni API optimális működéséhez a logic apps hello API is rendelkezik egy [Swagger-dokumentum](http://swagger.io/specification/) hello API műveletek és a paraméterek leírását.
Sok szalagtárak, például [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle), automatikusan elő tud állítani hello Swagger-fájl meg. tooannotate hello Swagger-fájl megjelenített nevek, tulajdonságtípus és így tovább, használhatja [TRex](https://github.com/nihaue/TRex) , hogy a logic apps jól működik a Swagger-fájl.

<a name="actions"></a>

## <a name="action-patterns"></a>A művelet minták

A logic apps tooperform feladatokhoz, biztosítania kell az egyéni API [ *műveletek*](./logic-apps-what-are-logic-apps.md#logic-app-concepts). Egyes műveletek az API-t a maps tooan művelet. Egy alapszintű művelet eredménye egy tartományvezérlőre, amely elfogadja a HTTP-kérelmek és HTTP-válaszokat ad vissza. Így például a logikai alkalmazás küld egy HTTP-kérelem tooyour webalkalmazás vagy API-alkalmazás. Az alkalmazást, majd visszatér egy HTTP-válasz, logika hello tartalom együtt app tud feldolgozni.

Szabványos tevékenység írja be az API-t egy HTTP-kérési metódust, és ez a módszer a Swagger-fájl leírása. Majd hívása közvetlenül az API-t egy [HTTP-művelet](../connectors/connectors-native-http.md) vagy egy [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) művelet. Alapértelmezés szerint válaszok hello belül vissza kell [kérelem időkorlátja](./logic-apps-limits-and-config.md). 

![Standard műveleti minta](./media/logic-apps-create-api-app/standard-action.png)

<a name="pattern-overview"></a>toomake logikai alkalmazás türelmet, amíg az API-t hosszabb futó feladat befejeződik, az API-követve hello [lekérdezési aszinkron mintát](#async-pattern) vagy hello [webhook aszinkron mintát](#webhook-actions) a jelen témakörben ismertetett. Egy analógia, amelynek segítségével jelenítheti meg az ezen mintákat műveletet képzelhető el, egy magát az egyéni torta rendezéshez hello folyamat. hello lekérdezési mintát tükrözi hello viselkedést, ha meghívja a hello magát minden 20 perc toocheck hello torta készen áll-e. hello webhook mintát hello viselkedését, ahol hello magát kéri a telefonszámát, azok hívhatják meg, amikor készen áll a hello torta tükrözi.

Minták, a Microsoft hello [Logic Apps GitHub-tárházban](https://github.com/logicappsio). Emellett további információ [használatmérés műveletekhez](logic-apps-pricing.md).

<a name="async-pattern"></a>

### <a name="perform-long-running-tasks-with-hello-polling-action-pattern"></a>A lekérdezési művelet mintát hello hosszan futó feladatok végrehajtása

az API-feladatokat futtathat hello már toohave [kérelem időkorlátja](./logic-apps-limits-and-config.md), használhatja a hello lekérdezési aszinkron mintát. Ebben a mintában az API tegye egy külön szálban, de tartsa egy aktív kapcsolat toohello Logic Apps alrendszeren működni rendelkezik. Ily módon a hello logikai alkalmazás nem nem túllépi az időkorlátot, és folytassa a következő lépése hello hello munkafolyamat az API-működő befejeződése előtt.

Hello általános mintát itt található:

1. Győződjön meg arról, hogy hello motor tudja, hogy az API-hello kérés elfogadva, és megkezdte működését.
2. Amikor hello motor későbbi kérelmek feladat állapotát, az API-hello feladat befejezése után tudja hello motor segítségével.
3. Térjen vissza a vonatkozó adatokat toohello motor, így hello logic app munkafolyamat folytatása.

<a name="bakery-polling-action"></a>Most hello előző magát analógia toohello lekérdezési minta alkalmazásához, és képzelhető el, hogy hívjon egy magát és egy rendezési egyéni torta kézbesítésre. Adja meg a megfelelő hello torta hello folyamat időt vesz igénybe, és nem kívánt toowait hello telefonon, amíg hello magát hello torta működéséről. hello magát megerősíti, hogy a rendelés és 20 percenként hello torta állapot hívása során. Után 20 perc átadni, meghívja a hello magát, de azok jelzi, hogy a torta nem történik, és, hogy meg kell hívni a másik 20 perc. Ez vissza oda-folyamat folytatódik, amíg nem hívja, és hello magát tájékoztat, hogy a rendelés készen áll-e, és továbbítja a torta. 

Ezért képezze le a lekérdezési mintát vissza. hello magát az egyéni API jelöli meg, amíg, hello torta-ügyfelekre kijelenti hello Logic Apps motor. Hello programmag meghívja az API-t egy kérelmet, ha az API-hello kérelem megerősíti, és válaszol hello alatt az időtartam alatt, ha hello motor ellenőrizheti a feladat állapotát. hello motor továbbra is fennáll, az API-válaszol, hogy hello történik, amíg a feladat állapotát és értéket ad vissza adatokat tooyour logikai alkalmazást, majd folytatja a munkafolyamat ellenőrzése. 

![Lekérdezési művelet minta](./media/logic-apps-create-api-app/custom-api-async-action-pattern.png)

Az alábbiakban a API toofollow hello API szempontjából leírt hello lépései:

1. Ha az API lekérdezi egy HTTP kérelem toostart munka, azonnal visszatérési HTTP `202 ACCEPTED` response hello `location` ebben a lépésben ismertetett fejléc. Ez a válasz lehetővé teszi, hogy a hello motor tudja, hogy az API-hoz készült Logic Apps hello kérés elfogadva hello-kérések forgalma (bemeneti adatokat), és feldolgozása már folyamatban van. 
   
   Hello `202 ACCEPTED` válasz tartalmaznia kell ezek a fejlécek:
   
   * *Szükséges*: A `location` fejlécet, amely meghatározza a hello abszolút elérési út tooa URL ahol hello Logic Apps motor ellenőrizheti az API-feladat állapota

   * *Nem kötelező*: A `retry-after` hello motor hello másodpercek számát megadó fejléc várakozási hello ellenőrzése `location` feladatállapot URL-címe. 

     Alapértelmezés szerint hello motor minden 20 másodperc ellenőrzi. egy másik időszakban toospecify tartalmaznak hello `retry-after` hello a következő lekérdezési másodperc fejléc és hello száma.

2. Hello megadott idő eltelte után a hello Logic Apps szavazások hello motor `location` URL-cím toocheck feladat állapotát. Az API-t kell ezen ellenőrzések elvégzéséhez és a ezeket a válaszokat vissza:
   
   * Hello történik, ha vissza HTTP `200 OK` válasz hello válasz forgalma (a következő lépés hello megadott) együtt.

   * Ha hello feladat még dolgozik, adja vissza egy másik HTTP `202 ACCEPTED` választ, de az hello azonos fejlécek hello eredeti válaszként.

Ha az API-t ezt a mintát követi, toodo semmi sincs a hello logic app munkafolyamat definícióját toocontinue állapotának ellenőrzését. Ha hello motor lekérdezi HTTP `202 ACCEPTED` válasz és egy érvényes `location` fejléc, hello motor tekintetben hello aszinkron mintát, és ellenőrzi, hello `location` fejléc, amíg az API-t egy nem 202 választ ad vissza.

> [!TIP]
> Egy példa aszinkron mintát, tekintse meg a [aszinkron vezérlő válasz mintát a Githubon](https://github.com/logicappsio/LogicAppsAsyncResponseSample).

<a name="webhook-actions"></a>

### <a name="perform-long-running-tasks-with-hello-webhook-action-pattern"></a>Hosszan futó feladatokat hello webhook művelet mintával

Alternatív megoldásként használható hello webhook mintát hosszan futó feladatokat és az aszinkron feldolgozás. Ebben a mintában hello logikai alkalmazás "callback" a munkafolyamat folytatásához feldolgozása API toofinish a várjon rendelkezik. A visszahívási egy HTTP POST által küldött üzenet tooa URL-címet, ha olyan esemény történik. 

<a name="bakery-webhook-action"></a>Most hello előző magát analógia toohello webhook minta alkalmazásához, és képzelhető el, hogy hívjon egy magát és egy rendezési egyéni torta kézbesítésre. Adja meg a megfelelő hello torta hello folyamat időt vesz igénybe, és nem kívánt toowait hello telefonon, amíg hello magát hello torta működéséről. hello magát megerősíti, hogy a rendelés, de ezúttal meg számukra a telefonszámát, azok hívhatják meg, amikor hello torta hajtja végre. Megadott idő hello magát megtudhatja, ha a sorrend készen áll, és továbbítja a torta.

Azt leképezni a webhook mintát vissza, ha a hello magát az egyéni API, akkor hello torta vevő, jelentik hello Logic Apps motor közben jelöli. hello motor hívja az API-t a kérelmet, és a "callback" URL-címet.
Hello történik, amikor az API-hello URL-cím toonotify hello motort használja, és térjen vissza az adatok tooyour logikai alkalmazást, majd folytatja a munkafolyamat. 

Ebben a mintában két végpontot a tartományvezérlőn beállított: `subscribe` és`unsubscribe`

*  `subscribe`végpont: hello munkafolyamatban az API-művelet végrehajtása elérésekor hello Logic Apps motor hívások hello `subscribe` végpont. Ez a lépés azt eredményezi, hello logic app toocreate visszahívási URL-címet a API tárolja, és majd várja meg, amíg a hello visszahíváshoz az API-t a munkahelyi. Az API-t, majd visszahívja egy HTTP POST toohello URL-CÍMÉT, és továbbadja minden visszaadott tartalom és a fejlécek bemeneti toohello logika alkalmazásként.

* `unsubscribe`végpont: hello logikai alkalmazás futtatása megszűnik, ha a hello Logic Apps hívások hello motor `unsubscribe` végpont. Az API-regisztrációjának törlése hello visszahívási URL-címet, majd állítsa le a folyamatokat, szükség szerint.

![Webhook műveleti minta](./media/logic-apps-create-api-app/custom-api-webhook-action-pattern.png)

> [!NOTE]
> Jelenleg hello Logic App tervező nem támogatja a felderítő webhook végpontok Swagger keresztül. Így az ebben a mintában tooadd kell egy [ **Webhook** művelet](../connectors/connectors-native-webhook.md) , és adja meg a hello URL-címe, fejlécek és a kérés törzsében. Lásd még: [munkafolyamat-műveleteket és eseményindítók](logic-apps-workflow-actions-triggers.md#api-connection-webhook-action). toopass hello visszahívási URL-címben, a hello használhatja `@listCallbackUrl()` munkafolyamat-funkció sem hello előző mező szükséges.

> [!TIP]
> Tekintse át egy példa a webhook mintában ez [webhook eseményindító mintát a Githubon](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

<a name="triggers"></a>

## <a name="trigger-patterns"></a>Eseményindító minták

Az egyéni API működhet, és egy [ *eseményindító* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) , amely a logikai alkalmazás elindul, és amikor új adatok vagy egy esemény megfelel-e a megadott feltétel. Ehhez az eseményindítóhoz is vagy rendszeresen figyelni, vagy várjon, amíg és figyelni, az új adatokat és eseményeket a szolgáltatási végpont. Ha új adatokat vagy egy esemény megfelel-e hello megadott feltétel, hello eseményindító következik be, és elindítja a hello logikai alkalmazás, amely figyel toothat eseményindító. toostart a logic apps ily módon, az API-követve hello [ *lekérdezési eseményindító* ](#polling-triggers) vagy hello [ *webhook eseményindító* ](#webhook-triggers) mintát. Ezek a minták megfelelői hasonló tootheir a [lekérdezési műveletek](#async-pattern) és [webhookműveletek](#webhook-actions). Emellett további információ [az eseményindítók a használatmérés](logic-apps-pricing.md).

<a name="polling-triggers"></a>

### <a name="check-for-new-data-or-events-regularly-with-hello-polling-trigger-pattern"></a>Ellenőrizze a az új adatokat és eseményeket rendszeresen hello lekérdezési eseményindító minta

A *lekérdezési eseményindító* hello hasonlóan működik [lekérdezési művelet](#async-pattern) ebben a témakörben korábban leírt. hello Logic Apps motor rendszeresen hívja, és ellenőrzi az új adatokat és eseményeket hello eseményindító végpont. Ha hello motor új adatokat vagy egy eseményt, amely megfelel a megadott feltétel, hello eseményindító következik be. Ezt követően hello motor hoz létre egy logic app-példány bemeneti hello adatokat feldolgozó. 

![Lekérdezési eseményindító minta](./media/logic-apps-create-api-app/custom-api-polling-trigger-pattern.png)

> [!NOTE]
> Egyes lekérdezési kérelmek egy művelet végrehajtási számít, akkor is, ha nincs logic app-példány jön létre. tooprevent feldolgozási hello ugyanazokat az adatokat többször, az eseményindító, amely már elolvasta és toohello logikai alkalmazás átadott adatok kell törlése.

Az alábbiakban leírt hello API szempontjából lekérdezési eseményindító lépéseit:

| Új adatok vagy esemény található?  | API-válaszból | 
| ------------------------- | ------------ |
| Található | Térjen vissza a HTTP `200 OK` a hello válasz tartalom (a következő lépésben megadott) állapotát. <br/>Ez a válasz hoz létre egy logic app-példány, és elindítja a hello munkafolyamat. |
| Nem található | Térjen vissza a HTTP `202 ACCEPTED` állapot egy `location` fejléc és a `retry-after` fejléc. <br/>Eseményindítók, hello `location` fejléc is tartalmaznia kell egy `triggerState` lekérdezési paraméter, amely általában a "időbélyege." Az API-t használhatja az azonosító tootrack hello logika hello legutóbbi alkalmazás lett elindítva. |

Például tooperiodically ellenőrizze a szolgáltatást az új fájlok, előfordulhat, hogy készít, amelyen ezek közül a viselkedésmódok lekérdezési eseményindító:

| Kérelemben `triggerState`? | API-válaszból |
| -------------------------------- | -------------|
| Nem | Térjen vissza a HTTP `202 ACCEPTED` állapot plusz a `location` fejléc a következő `triggerState` toohello aktuális idő beállítása és hello `retry-after` időköz too15 (másodperc). |
| Igen | Ellenőrizze a szolgáltatást a fájlok hello után `DateTime` a `triggerState`. |

| Található fájlok száma | API-válaszból |
| --------------------- | -------------|
| Egyetlen fájl | Térjen vissza a HTTP `200 OK` állapotát és hello tartalom hasznos, a frissítés `triggerState` toohello `DateTime` hello által visszaadott fájl, és állítsa be `retry-after` időköz too15 (másodperc). |
| Több fájl | Egy fájl vissza egy időben és HTTP `200 OK` állapot, a frissítés `triggerState`, és a set hello `retry-after` időköz (másodperc) too0. </br>A lépések segítségével hello motor arról, hogy több adat, és hello motor azonnal igényeljen hello adatok hello hello URL-címet a `location` fejléc. |
| Nincsenek fájlok | Térjen vissza a HTTP `202 ACCEPTED` állapota, ne módosítsa `triggerState`, és a set hello `retry-after` időköz too15 (másodperc). |

> [!TIP]
> Például egy lekérdezési eseményindító mintát, tekintse át a [lekérdezési eseményindító vezérlő mintát a Githubon](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/PollTriggerController.cs).

<a name="webhook-triggers"></a>

### <a name="wait-and-listen-for-new-data-or-events-with-hello-webhook-trigger-pattern"></a>Várja meg, és az új adatokat és eseményeket hello webhook eseményindító mintával figyelésére

A webhook trigger egy *leküldéses eseményindító* , amely megvárja-e, és az új adatokat és eseményeket a szolgáltatási végpont figyel. Ha új adatokat vagy egy esemény megfelel-e a hello megadott feltétel, hello eseményindító következik be, és létrehoz egy alkalmazást majd feldolgozza a hello adatokat bemenetként logika példányt.
Webhook eseményindítók működésre hello hasonlóan [webhookműveletek](#webhook-actions) korábban a jelen témakörben ismertetett, és be vannak állítva a `subscribe` és `unsubscribe` végpontok. 

* `subscribe`végpont: hozzáadása, és mentse egy webhook eseményindító a Logic Apps alkalmazást, a hello Logic Apps hívások hello motor `subscribe` végpont. Ez a lépés azt eredményezi, hello logic app toocreate egy visszahívási URL-címet, amely az API-t tárolja. Ha az új adatok, vagy egy eseményt, amely megfelel a megadott feltétel hello, az API-visszahívja egy HTTP POST toohello URL-címmel. hello tartalom forgalma és a fejlécek megfelel bemeneti toohello logika alkalmazásként.

* `unsubscribe`végpont: hello webhook eseményindító vagy teljes logikai alkalmazás törlésekor hello Logic Apps motor hívások hello `unsubscribe` végpont. Az API-regisztrációjának törlése hello visszahívási URL-címet, majd állítsa le a folyamatokat, szükség szerint.

![Webhook eseményindító minta](./media/logic-apps-create-api-app/custom-api-webhook-trigger-pattern.png)

> [!NOTE]
> Jelenleg hello Logic App tervező nem támogatja a felderítő webhook végpontok Swagger keresztül. Így az ebben a mintában tooadd kell egy [ **Webhook** eseményindító](../connectors/connectors-native-webhook.md) , és adja meg a hello URL-címe, fejlécek és a kérés törzsében. Lásd még: [HTTPWebhook eseményindító](logic-apps-workflow-actions-triggers.md#httpwebhook-trigger). toopass hello visszahívási URL-címben, a hello használhatja `@listCallbackUrl()` munkafolyamat-funkció sem hello előző mező szükséges.
>
> tooprevent feldolgozási hello ugyanazokat az adatokat többször, az eseményindító, amely már elolvasta és toohello logikai alkalmazás átadott adatok kell törlése.

> [!TIP]
> Tekintse át egy példa a webhook mintában ez [webhook eseményindító vezérlő mintát a Githubon](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

## <a name="deploy-call-and-secure-custom-apis"></a>Központi telepítése, hívja, és biztonságos egyéni API-k

Miután létrehozta az egyéni API-kat, beállítása az API-kat, központi telepítés biztonságosan keresheti őket. Ismerje meg, hogyan túl[központi telepítése, hívja, és egyéni API-k a logic Apps alkalmazások biztonságos](./logic-apps-custom-hosted-api.md).

## <a name="publish-custom-apis-tooazure"></a>Egyéni API-k tooAzure közzététele

toomake az egyéni API-kat az Azure nyilvános használható küldje el a jelölések toohello [hivatalos Microsoft Azure program](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/).

## <a name="get-help"></a>Segítségkérés

Egyéni API-k meghatározott kapcsolatban forduljon [ customapishelp@microsoft.com ](mailto:customapishelp@microsoft.com).

tooask kérdéseket, válasz kérdések és milyen egyéb Azure Logic Apps felhasználók végzi, tekintse meg a Microsoft hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Logic Apps alkalmazások és összekötők javítása, szavazhatnak vagy küldje el a következő hello ötleteket toohelp [Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Következő lépések

* [A használatmérés műveletek és eseményindítók](logic-apps-pricing.md)
* [Tartalomtípusok kezelése](./logic-apps-content-type.md)
* [Hibák és kivételek kezelése](./logic-apps-exception-handling.md)
* [Biztonságos hozzáférés tooyour logic Apps alkalmazások](./logic-apps-securing-a-logic-app.md)
* [Hívja, eseményindító, vagy a logic apps a HTTP-végpontokról egymásba](./logic-apps-http-endpoint.md)