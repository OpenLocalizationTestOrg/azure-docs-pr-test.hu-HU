---
title: "aaaCall, indítsa el, vagy a HTTP-végpontok – Azure Logic Apps munkafolyamatok beágyazásához |} Microsoft Docs"
description: "Az Azure Logic Apps HTTP végpontok toocall, eseményindító vagy nest munkafolyamatok beállítása"
services: logic-apps
keywords: "munkafolyamatok, HTTP-végpontok"
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 73ba2a70-03e9-4982-bfc8-ebfaad798bc2
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 03/31/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 072a314c3bff75ab7696f86bb063bb7c03c4ae89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-logic-apps"></a>Hívja, eseményindító, vagy a logic Apps alkalmazások a HTTP-végpontokról munkafolyamatok egymásba

Is natív módon teszi ki szinkron HTTP-végpontokról a logic apps eseményindítóként használható, hogy indul el, vagy hívja a logic apps segítségével egy URL-címet. A logic Apps alkalmazásait is ágyazhatók be munkafolyamatok hívható végpontok egy mintát.

HTTP-végpontokról toocreate, adhat hozzá ezek az eseményindítók, hogy a logic apps bejövő kérelmek fogadására:

* [Kérés](../connectors/connectors-native-reqres.md)

* [API-kapcsolat Webhook](logic-apps-workflow-actions-triggers.md#api-connection-trigger)

* [HTTP Webhook](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > Bár a jelen példában használják a hello **kérelem** eseményindító, akkor használhatja a hello felsorolt HTTP-eseményindítók, és minden alapelvek azonos toohello más alkalmazhatunk eseményindító.

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a>A Logic Apps alkalmazást HTTP végpont beállítása

toocreate HTTP-végponttal, adjon hozzá egy eseményindító, amely képes fogadni a beérkező kéréseket.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com "Azure-portálon"). Nyissa meg tooyour Logic Apps alkalmazást, és nyissa meg a Logic App Tervező.

2. Adjon hozzá egy eseményindító, amely lehetővé teszi, hogy a bejövő kérések fogadásához Logic Apps alkalmazást. Adja hozzá például hello **kérelem** eseményindító tooyour logikai alkalmazást.

3.  A **kérelem törzse JSON-séma**, opcionálisan adhatja meg a JSON-séma hello tartalom (adatok) hello eseményindító tooreceive várható.

    hello designer a sémát, hogy a logikai alkalmazás használhatja tooconsume, elemzési és pass adatait a munkafolyamaton keresztül hello eseményindító jogkivonatokat előállító használ. 
    További részletek [JSON-sémák alapján generált jogkivonatok](#generated-tokens).

    Ehhez a példához írja be a hello séma hello designer látható:

    ```json
    {
      "type": "object",
      "properties": {
        "address": {
          "type": "string"
        }
      },
      "required": [
        "address"
      ]
    }
    ```

    ![Hello kérelem művelet hozzáadása][1]

    > [!TIP]
    > 
    > A sémát a JSON hasznos generálása egy eszköz, például [jsonschema.net](http://jsonschema.net/), vagy a hello **kérelem** kiválasztásával eseményindító **használata minta hasznos toogenerate séma**. 
    > Adjon meg a minta-adattartalmat, és válassza a **végzett**.

    Ha például a minta hasznos:

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    hoz létre ebben a sémában:

    ```json
    }
       "type": "object",
       "properties": {
          "address": {
             "type": "string" 
          }
       }
    }
    ```

4.  Mentse a Logic Apps alkalmazást. A **HTTP POST toothis URL-cím**, most látnia kell egy generált visszahívási URL-t, például ebben a példában:

    ![Generált visszahívási végpont URL-címe](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    Az URL-cím a hitelesítéshez használt hello lekérdezési paraméterek közös hozzáférésű Jogosultságkód (SAS) kulcsot tartalmaz. 
    Hello HTTP végponti URL-cím is lekérheti a logic app – áttekintés hello Azure-portálon. A **eseményindító előzmények**, válassza ki az eseményindító:

    ![HTTP-végpont URL-cím beszerzése az Azure portálról][2]

    Vagy adott hívás végrehajtása kaphat hello URL-címe:

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-hello-http-method-for-your-trigger"></a>Az eseményindító hello HTTP-metódus módosítása

Alapértelmezés szerint hello **kérelem** eseményindító vár HTTP POST-kérelmet, de különböző HTTP-metódus használatával kérheti le. 

> [!NOTE]
> Megadhatja, hogy csak egy metódus típusa.

1. Az a **kérelem** indítható el, válassza a **speciális beállítások megjelenítése**.

2. Nyissa meg hello **metódus** listája. Ehhez a példához válassza ki a **beolvasása** , hogy később tesztelheti a HTTP-végpont URL-CÍMÉT.

    > [!NOTE]
    > Jelölje be más HTTP-metódus, vagy adjon meg egyéni módszer a saját logikai alkalmazást.

    ![HTTP-metódus módosítása](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a>Fogadja el a paraméterek használatával a HTTP-végpont URL-címe

Ha azt szeretné, hogy a HTTP végpont URL-cím tooaccept paramétereket, testre szabhatja a trigger elem relatív elérési útja.

1. Az a **kérelem** indítható el, válassza a **speciális beállítások megjelenítése**. 

2. A **metódus**, adja meg, amelyet a kérés toouse hello HTTP-metódus. Ehhez a példához válassza ki a hello **beolvasása** metódust, ha még nem tette meg, így ellenőrizheti, hogy a HTTP-végpont URL-címet.

      > [!NOTE]
      > Relatív elérési útvonal az eseményindító megadása esetén az eseményindító közvetlenül is adjon meg egy HTTP-metódus.

3. A **relatív elérési út**, adja meg a hello relatív elérési út hello paraméter, amely az URL-CÍMÉRE kell fogadnia, például a `customers/{customerID}`.

    ![Adja meg a hello HTTP-metódus és a relatív elérési út paraméter](./media/logic-apps-http-endpoint/relativeurl.png)

4. toouse hello paraméter, adjon hozzá egy **válasz** művelet tooyour logikai alkalmazást. (Válassza ki az eseményindító **új lépés** > **művelet hozzáadása** > **válasz**) 

5. A válaszban szereplő **törzs**, tartalmaz hello tokent hello paraméter, a trigger elem relatív elérési utat adott meg.

    Például tooreturn `Hello {customerID}`, frissítse a válasz **törzs** rendelkező `Hello {customerID token}`. 
    dinamikus tartalom listába hello kell jelenik meg, és hello megjelenítése `customerID` meg tooselect jogkivonatból.

    ![Adja hozzá a paraméter tooresponse törzse](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    A **törzs** ebben a példában hasonlóan kell kinéznie:

    ![Választörzs paraméterrel](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. Mentse a Logic Apps alkalmazást. 

    A HTTP-végpont URL-CÍMÉT most már tartalmaz hello relatív elérési út, például: 

    HTTPS & # 58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...

7. a HTTP-végpont, másolás és beillesztés hello frissített URL-cím egy másik böngészőben ablakba, de cserélje le tootest `{customerID}` a `123456`, nyomja le az ENTER billentyűt.

    A böngésző ezt a szöveget kell megjelenítése: 

    `Hello 123456`

<a name="generated-tokens"></a>
### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a>A logikai alkalmazásnak JSON-sémák alapján generált jogkivonatok

Ha megadta a JSON-séma a a **kérelem** indítás, hello Logic App Designer jogkivonatokat hoz létre a tulajdonságokat, hogy a séma. A jogkivonatok az adatokat a logic app munkafolyamaton keresztül használhatja.

Ehhez a példához hello hozzáadásakor `title` és `name` tulajdonságok tooyour JSON-séma, a jogkivonatok most elérhető toouse munkafolyamat a későbbi lépésekben. 

Hello teljes JSON-séma a következő:

```json
{
   "type": "object",
   "properties": {
      "address": {
         "type": "string"
      },
      "title": {
         "type": "string"
      },
      "name": {
         "type": "string"
      }
   },
   "required": [
      "address",
      "title",
      "name"
   ]
}
```

## <a name="create-nested-workflows-for-logic-apps"></a>Beágyazott munkafolyamatok a logic Apps alkalmazások létrehozása

A Logic Apps alkalmazást a munkafolyamatok ágyazhatja más logikai alkalmazások fogadhassanak kérelmek hozzáadásával. tooinclude a logic apps hozzáadása hello **Azure Logic Apps - válassza ki a Logic Apps munkafolyamat** művelet tooyour eseményindító. Ezután kiválaszthatja a megfelelő logic Apps alkalmazásokból.

![Egy másik logikai alkalmazás hozzáadása](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a>Hívás vagy eseményindítót, a logic apps HTTP-végpontokról keresztül

A HTTP-végpont létrehozása után a logikai alkalmazás keresztül is elindíthatja egy `POST` metódus toohello teljes URL-címet. A Logic apps van a közvetlen hozzáférés végpontok beépített támogatása.

## <a name="reference-content-from-an-incoming-request"></a>Olyan bejövő kérelemre a referencia-tartalom

Ha a tartalom hello főelem típusából `application/json`, tulajdonságok hello bejövő kérelem hivatkozhat. Ellenkező esetben tartalmat a rendszer egyetlen bináris egységbe, hogy átadhatók tooother API-k. Ez a tartalom belül hello munkafolyamat tooreference kell konvertálnia ezt a tartalmat. Ha át például `application/xml` tartalom, használhat `@xpath()` egy XPath kiolvasásához vagy `@json()` XML tooJSON alakításának. További tudnivalók [tartalomtípusok használata](../logic-apps/logic-apps-content-type.md).

tooget hello kimenetét olyan bejövő kérelemre, használhatja a hello `@triggerOutputs()` függvény. hello kimeneti ebben a példában látható:

```json
{
    "headers": {
        "content-type" : "application/json"
    },
    "body": {
        "myProperty" : "property value"
    }
}
```

tooaccess hello `body` tulajdonság pontosabban használható hello `@triggerBody()` helyi. 

## <a name="respond-toorequests"></a>Válaszoljon toorequests

Érdemes lehet toorespond toocertain kérelmek indítsa el a logikai alkalmazás tartalom toohello hívó vissza. tooconstruct hello állapotkód, fejléc és a válasz törzsében, használhatja a hello **válasz** művelet. Ez a művelet bárhol megjelenhet a Logic Apps alkalmazást, nem csak hello végén a munkafolyamatba.

> [!NOTE] 
> Ha nem adja meg a Logic Apps alkalmazást egy **válasz**, hello HTTP-végpont válaszol *azonnal* rendelkező egy **202 elfogadott** állapotát. Emellett hello eredeti kérelem tooget hello válasz, hello válasz szükséges lépéseket kell belül lefutnak hello [kérelem időkorlátja](./logic-apps-limits-and-config.md) kivéve, ha meghívja a hello munkafolyamat beágyazott logikai alkalmazás számára. Nincs válasz a határidőn belül történik, ha a bejövő kérelem hello érvénytelenné válik, és hello HTTP-válasz fogadása **408 az ügyfél időtúllépése**. Beágyazott logic apps hello szülő logikai alkalmazás továbbra is toowait befejezéséig, függetlenül attól, hogy mennyi idő szükséges választ.

### <a name="construct-hello-response"></a>Szerkezet hello válasz

Megadhat több fejléc és a tartalom bármilyen típusú hello adott válasz törzse. A példa egy válasz, a hello fejléc határozza meg, hogy rendelkezik-e hello válasz tartalomtípusa `application/json`. hello törzsében és `title` és `name`frissítve korábban hello hello JSON-séma alapján **kérelem** eseményindító.

![HTTP-válasz művelet][3]

Válaszok ezekkel a tulajdonságokkal rendelkeznek:

| Tulajdonság | Leírás |
| --- | --- |
| statusCode |Megadja a hello HTTP-állapotkód válaszol toohello bejövő kérelemhez. Ez a kód bármilyen érvényes állapotkód 2xx, 4xx vagy 5xx kezdetű lehet. 3xx állapotkódok azonban nem engedélyezett. |
| Fejlécek |Definiálja a fejlécek tooinclude tetszőleges számú hello válasz. |
| Törzs |Adja meg a szervezet objektum, amely egy karakterlánc lehet, egy JSON-objektumból, vagy az előző lépésben hivatkozott még bináris tartalom. |

Íme, milyen láthatóhoz hasonló most a hello hello JSON-séma **válasz** művelet:

``` json
"Response": {
   "inputs": {
      "body": {
         "title": "@{triggerBody()?['title']}",
         "name": "@{triggerBody()?['name']}"
      },
      "headers": {
           "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {},
   "type": "Response"
}
```

> [!TIP]
> Válassza a tooview hello teljes JSON-definíció a Logic Apps alkalmazást, a Logic App Designer hello **nézet Code**.

## <a name="q--a"></a>Kérdések és válaszok

#### <a name="q-what-about-url-security"></a>K: Mi a helyzet URL-cím biztonsági?

V: azure biztonságosan logika visszahívási URL-címek egy közös hozzáférésű Jogosultságkód (SAS) használatával hoz létre. Az aláírás haladnak keresztül lekérdezési paramétert, és ellenőrizni kell, mielőtt a Logic Apps alkalmazást is érvényesítést. Az Azure létrehoz egy titkos kulcsot a Logic Apps alkalmazást, a hello eseményindító nevét és a hello műveletet végzett egyedi kombinációját használva hello aláírás. Kivéve, ha valaki hozzáférést toohello titkos logic app kulccsal rendelkezik, azok nem hoznak létre érvényes aláírással.

   > [!IMPORTANT]
   > A gyártás és biztonságos erősen ajánlott elleni hívja közvetlenül hello böngészőből a Logic Apps alkalmazást, mert:
   > 
   > * hello megosztott elérési kulcsával hello URL-cím jelenik meg.
   > * Biztonságos tartalom házirendek tooshared tartományok miatt nem tudja kezelni a logikai alkalmazást az ügyfelek között.

#### <a name="q-can-i-configure-http-endpoints-further"></a>K: beállítani a HTTP-végpontokról további?

V: Igen, támogatja a HTTP-végpontokról a keresztül speciális konfigurációs [ **API Management**](../api-management/api-management-key-concepts.md). Ezt a szolgáltatást is kínál hello funkció, akkor tooconsistently kezelhetők minden az API-kat, beleértve a logic apps, állítson be egyéni tartománynevek, használja a további hitelesítési módszerek és egyéb, például:

* [Hello kérelem módszer váltása](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [Hello URL-szegmensek hello kérelem módosítása](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* Állítsa be az API Management tartományok hello [Azure-portálon](https://portal.azure.com/ "Azure-portálon")
* Az egyszerű hitelesítés házirend toocheck beállítása

#### <a name="q-what-changed-when-hello-schema-migrated-from-hello-december-1-2014-preview"></a>K: milyen megváltozik, miközben hello séma átemelt hello 2014. decemberi 1 preview?

V: Itt található egy Összegzés, ezekről a változásokról:

| 2014. decemberi 1 preview | 2016. június 1. |
| --- | --- |
| Kattintson a **HTTP-figyelő** API-alkalmazás |Kattintson a **kézi indítási** (API-alkalmazás nem kötelező) |
| HTTP-figyelő beállítás "*automatikusan választ küld*" |Tartalmaznak egy **válasz** művelet, vagy nem az hello munkafolyamat-definíció |
| Basic vagy az OAuth-hitelesítés konfigurálása |API Management szolgáltatáson keresztül |
| HTTP-módszer konfigurálása |A **speciális beállítások megjelenítése**, válassza a HTTP-metódus |
| Adja meg relatív elérési útját |A **speciális beállítások megjelenítése**, relatív elérési út |
| Hivatkozás hello bejövő törzs keresztül`@triggerOutputs().body.Content` |A hivatkozás`@triggerOutputs().body` |
| **HTTP-válasz küldése** műveletet a hello HTTP-figyelő |Kattintson a **válaszoljon tooHTTP kérelem** (API-alkalmazás nem kötelező) |

## <a name="get-help"></a>Segítségkérés

tooask kérdések kérdést, és ismerje meg, milyen egyéb Azure Logic Apps felhasználók végzi, látogasson el hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp Azure Logic Apps alkalmazások és összekötők javítása, szavazhatnak, vagy küldje el a következő hello ötleteket [Azure Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Következő lépések

* [Logikaialkalmazás-definíciók készítése](./logic-apps-author-definitions.md)
* [Hibák és kivételek kezelése](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png
