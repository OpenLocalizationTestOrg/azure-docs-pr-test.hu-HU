---
title: "Munkafolyamat-műveleteket és eseményindítók - Azure Logic Apps |} Microsoft Docs"
description: 
services: logic-apps
author: MandiOhlinger
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/17/2016
ms.author: LADocs; mandia
ms.openlocfilehash: bd3f1d225b974ebde889738bb435825658d1e1e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a>Munkafolyamat-műveleteket, és az Azure Logic Apps eseményindítók

A Logic apps eseményindítók és műveletek alkotják. Nincsenek eseményindítók hat típusú. Minden különböző felület és a különböző viselkedés rendelkezik. Részleteinek megtekintésével is olvashat további részleteket a [Munkafolyamatdefiníciós nyelve](logic-apps-workflow-definition-language.md).  
  
Olvassa el a további információk eseményindítók és műveletek és hogyan használhatja azokat az üzleti folyamatokat és a munkafolyamatok javítása érdekében a logic Apps alkalmazásokat készíthet.  
  
### <a name="triggers"></a>Eseményindítók  

Egy eseményindító határozza meg a hívások is kezdeményezhető a logic app munkafolyamat futását. Az alábbiakban kezdeményezheti a munkafolyamatot futtató két különböző módon:  
  
-   A lekérdezési eseményindító  

-   A leküldéses eseményindító - meghívásával a [munkafolyamat szolgáltatás REST API](https://docs.microsoft.com/rest/api/logic/workflows)  
  
Indítók a legfelső szintű elemet tartalmaznak:  
  
```json
"<name-of-the-trigger>" : {
    "type": "<type-of-trigger>",
    "inputs": { <settings-for-the-call> },
    "recurrence": {  
        "frequency": "Second|Minute|Hour|Week|Month|Year",
        "interval": "<recurrence interval in units of frequency>"
    },
    "conditions": [ <array-of-required-conditions > ],
    "splitOn" : "<property to create runs for>",
    "operationOptions": "<operation options on the trigger>"
}
```

### <a name="trigger-types-and-their-inputs"></a>Az eseményindítók típusai és azok  

Az ilyen típusú eseményindítók használhatja:
  
-   **Kérelem** \- lehetővé teszi a logikai alkalmazást hívja meg a végpont  
  
-   **Ismétlődés** \- következik be a meghatározott ütemezés szerint  
  
-   **HTTP** \- HTTP webes végpont kérdezi le. A HTTP-végpont meg kell felelnie egy meghatározott eseményindító szerződés \- használva egy 202\-aszinkron mintát, vagy egy tömb vissza  
  
-   **ApiConnection** \- indul el, mint a HTTP szavazások, azonban kihasználja a [Microsoft által felügyelt API-k](https://docs.microsoft.com/azure/connectors/apis-list)  
  
-   **HTTPWebhook** \- megnyit egy végpontot, hasonló manuális eseményindító, azonban ez szükségessé teszi a megadott URL-címhez regisztrálásához és  
  
-   **ApiConnectionWebhook** \- Operates hasonlóan a HTTPWebhook eseményindító, ha kihasználja a Microsoft által felügyelt API-k       
    Minden eseményindító más rendelkezik **bemenetek** , amely meghatározza, hogy a viselkedését.  
  
## <a name="request-trigger"></a>Kérelem eseményindító  

Ehhez az eseményindítóhoz szolgál egy végpontot, amely meghívja a keresztül egy HTTP-kérelem lehet meghívni a Logic Apps alkalmazást. A kérelem eseményindító ebben a példában néz ki:  
  
```json
"<name-of-the-trigger>" : {
    "type" : "request",
    "kind": "http",
    "inputs" : {
        "schema" : {
            "properties" : {
                "myInputProperty1" : { "type" : "string" },
                "myInputProperty2" : { "type" : "number" }
            },
        "required" : [ "myInputProperty1" ],
        "type" : "object"
        }
    }
} 
```

Szerepel továbbá egy nem kötelező tulajdonság neve **séma**:  
  
|Elem neve|Szükséges|Leírás|  
|----------------|------------|---------------|  
|Séma|Nem|A JSON-séma, amely megvizsgálja a bejövő kérelem. Hasznos segítséget nyújt a soron következő munkafolyamat lépéseket tudja, mely tulajdonságokat kell hivatkoznia.|

Ehhez a végponthoz meghívni, meg kell hívnia a *listCallbackUrl* API. Lásd: [munkafolyamat szolgáltatás REST API](https://docs.microsoft.com/rest/api/logic/workflows).  
  
## <a name="recurrence-trigger"></a>Ismétlődés eseményindító  

Ismétlődés eseményindító egyike, amelyek egy meghatározott ütemezés alapján fut. Ez a példa ilyen eseményindító nézhet ki:  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

Ahogy látja, akkor egyszerűen egy munkafolyamat futtatásához.  
  
|Elem neve|Szükséges|Leírás|  
|----------------|------------|---------------|  
|gyakoriság|Igen|Milyen gyakran az eseményindító végrehajtása. Használja a lehetséges értékek egyikét: másodperc, perc, óra, nap, hét, hónap vagy év|  
|időköz|Igen|Adott gyakoriságának az ismétlődés időköze|  
|startTime|Nem|A startTime nélkül a UTC eltolás valósul meg, ha a timeZone szolgál.|  
|Időzóna|nem|A startTime nélkül a UTC eltolás valósul meg, ha a timeZone szolgál.|  
  
Egy eseményindító végrehajtása egy bizonyos ponton a jövőben el is ütemezhető. Például ha el szeretné indítani a heti jelentés minden hétfőn ütemezheti a logikai alkalmazást a következő eseményindító létrehozása minden hétfőn el:  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": "1",
        "startTime" : "2015-06-22T00:00:00Z"
    }
}
```

## <a name="http-trigger"></a>HTTP eseményindító  

HTTP-eseményindítók kérdezze le a megadott végpont, és ellenőrizze a válasz határozza meg, hogy a munkafolyamat végre kell hajtani. A bemeneti objektum hajtja végre egy HTTP-hívás létrehozásához szükséges paraméterek készletét:  
  
|Elem neve|Szükséges|Leírás|Típus|  
|----------------|------------|---------------|--------|  
|Módszer|igen|A következő HTTP-metódus egyike lehet: GET, POST, PUT, DELETE, a javítás vagy a HEAD|Karakterlánc|  
|URI|igen|A http vagy https-végpont is nevezett. Legfeljebb 2 KB.|Karakterlánc|  
|Lekérdezések|Nem|Az URL-cím hozzáadása a lekérdezési paraméterek képviselő objektum. Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` URL-címét.|Objektum|  
|Fejlécek|Nem|A fejlécek, a kérelemben küldött mindegyikének képviselő objektum. Ha például a nyelv, és írja be a kérelem:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|Objektum|  
|Törzs|Nem|A tartalom a végpontnak küldött képviselő objektum.|Objektum|  
|retryPolicy|Nem|Egy objektum, amely lehetővé teszi testre szabhatja az újrapróbálási viselkedése 4xx vagy 5xx hibákat.|Objektum|  
|Hitelesítés|Nem|A metódust, hogy van-e a kérelem hitelesítése jelöli. Ezen az objektumon, lásd: [Feladatütemező kimenő hitelesítési](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Ütemező túl van egy több támogatott tulajdonságot: `authority` alapértelmezés szerint ez az érték van `https://login.windows.net` Ha nincs megadva, de használhat például egy másik célközönség`https://login.windows\-ppe.net`|Objektum|  
  
A HTTP-eseményindítóval egy adott minta a Logic Apps alkalmazást való ahhoz, hogy a HTTP API szükséges. A következő mezők szükségesek hozzá:  
  
|Válasz|Leírás|  
|------------|---------------|  
|Állapotkód|Állapotkód 200 \(OK\) futtató okozza. Bármely más állapotkód futtató nem okoz.|  
|Próbálja meg újra\-fejléc után|Amíg a logic app kérdezze le a végpont újra másodpercek számát.|  
|Hely fejléc|Hívja meg a következő lekérdezési időköz az URL-címe. Ha nincs megadva, az eredeti URL-címet használja.|  
  
Íme néhány példa a kérelmek különböző típusú különböző beállításokat:  
  
|Válaszkód|Próbálja meg újra\-után|Viselkedés|  
|-----------------|----------------|------------|  
|200|\(egyik sem\)|Nem érvényes eseményindító, újrapróbálkozási\-után kell megadni, vagy ellenkező esetben a motor soha nem kérdezi le a következő kérelem.|  
|202|60|Nem indítható el, a munkafolyamat. A következő kísérlet történik, egy perc alatt.|  
|200|10|A munkafolyamat futtatása, és újra keressen további tartalmat 10 másodperc múlva.|  
|400|\(egyik sem\)|Hibás kérés, amelyeken fut a munkafolyamat. Ha nincs **ismételje meg a házirend** definiálva, akkor az alapértelmezett házirend. Miután a rendszer elérte az újbóli próbálkozások számát, az eseményindító már nem érvényes.|  
|500|\(egyik sem\)|Kiszolgálóhiba, a munkafolyamat nem futnak.  Ha nincs **ismételje meg a házirend** definiálva, akkor az alapértelmezett házirend. Miután a rendszer elérte az újbóli próbálkozások számát, az eseményindító már nem érvényes.|  
  
A HTTP-eseményindítóval kimenetének ebben a példában látható:  
  
|Elem neve|Leírás|Típus|  
|----------------|---------------|--------|  
|Fejlécek|A fejléceket a http-válaszok.|Objektum|  
|Törzs|A HTTP-válasz törzsében.|Objektum|  
  
## <a name="api-connection-trigger"></a>API-kapcsolat eseményindító  

Az API-kapcsolat eseményindító hasonlít az alapszintű funkciókat, hogy a HTTP-eseményindítóval. A művelet azonosítására szolgáló paraméterek azonban eltérőek. Például:  
  
```json
"dailyReport" : {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://myarticles.example.com/"
            },
        }
        "connection": {
            "name": "@parameters('$connections')['myconnection'].name"
        }
    },  
    "method": "POST",
    "body": {
        "category": "awesomest"
    }
}
```

|Elem neve|Szükséges|Típus|Leírás|  
|----------------|------------|--------|---------------|  
|állomás|Igen||A ApiApp átjáró és az azonosítóját tárolja.|  
|Módszer|Igen|Karakterlánc|A következő HTTP-metódus egyike lehet: **beolvasása**, **POST**, **PUT**, **törlése**, **javítás**, vagy **HEAD**|  
|Lekérdezések|Nem|Objektum|A lekérdezési paraméterek fel kell venni az URL-címet jelöli. Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` URL-címét.|  
|Fejlécek|Nem|Objektum|A fejlécek, a kérelemben küldött mindegyikének jelöli. Ha például a nyelv, és írja be a kérelem:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Törzs|Nem|Objektum|A tartalom a végpontnak küldött jelöli.|  
|retryPolicy|Nem|Objektum|Lehetővé teszi testre szabhatja az újrapróbálási viselkedése 4xx vagy 5xx hibákat.|  
|Hitelesítés|Nem|Objektum|A metódust, hogy van-e a kérelem hitelesítése jelöli. Ezen az objektumon, lásd: [Feladatütemező kimenő hitelesítésre](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)|  
  
A gazdagép tulajdonságai a következők:  
  
|Elem neve|Szükséges|Leírás|  
|----------------|------------|---------------|  
|API runtimeUrl|Igen|A felügyelt API végpontja.|  
|Kapcsolat neve||Egy paraméter, hivatkozást kell `$connection` és a munkafolyamat által használt felügyelt API-kapcsolat neve.|
  
A kimenetek egy API-kapcsolat eseményindító a következők:
  
|Elem neve|Típus|Leírás|  
|----------------|--------|---------------|  
|Fejlécek|Objektum|A fejléceket a http-válaszok.|  
|Törzs|Objektum|A HTTP-válasz törzsében.|  
  
## <a name="httpwebhook-trigger"></a>HTTPWebhook eseményindító  

HTTPWebhook eseményindító megnyit egy végpontot, a kézi indítási hasonló, de a HTTPWebhook eseményindító is meghívja a regisztrálásához és megadott URL-címhez. Íme egy példa hogyan nézhet ki egy HTTPWebhook eseményindító:  

```json
"myappspottrigger": {
    "type": "httpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "https://pubsubhubbub.appspot.com/subscribe",
            "headers": { },
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": { },
            "retryPolicy": { }
        },
        "unsubscribe": {
            "url": "https://pubsubhubbub.appspot.com/subscribe",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "method": "POST",
            "authentication": { }
        }
    },
    "conditions": [ ]
    }
```

Ezek a szakaszok számos nem kötelező, és a Webhook viselkedését függ, mely szakaszok megadva, vagy nincs megadva.  
A Webhook tulajdonságainak a következők:  
  
|Elem neve|Szükséges|Leírás|  
|----------------|------------|---------------|  
|előfizetés|Nem|A kimenő kérelem is nevezett, amikor az eseményindító jön létre, és elvégzi a kezdeti regisztráció.|  
|előfizetés lemondása|Nem|A kimenő kérelem, az eseményindító törlésekor.|  
  
-   **Előfizetés** a kimenő hívás megkezdeni a figyelést események jogosultságokat. Ez a hívás kezdődik-e ugyanazokat a paramétereket, amelyek a normál HTTP-műveleteket. A kimenő kezdeményezték bármely bármely olyan módon, például a munkafolyamat-módosítások idő, amikor a hitelesítő adatok már megkezdődött, vagy az eseményindító bemeneti paraméterek módosítása.
  
    Ez a hívás támogatásához van egy új funkció: `@listCallbackUrl()`. Ez a függvény egy egyedi URL-címet az adott eseményindító a munkafolyamat adja vissza. Azt jelenti, hogy a szolgáltatás további használó végpontokon egyedi azonosítója.  
  
-   **Leiratkozhat** van meghívva, amikor egy művelet Renderelés ehhez az eseményindítóhoz érvénytelen, beleértve:  
  
    -   Törlése vagy az eseményindító letiltása  
  
    -   Törlése vagy a munkafolyamat letiltása  
  
    -   Törlése vagy az előfizetés letiltása  
  
    A logikai alkalmazás automatikusan meghívja a lemondás műveletet. A paraméterek, a függvény nem ugyanaz, mint a HTTP-eseményindítóval.  
  
    A kimenetek az HTTPWebhook eseményindító a bejövő kérelem:  
  
|Elem neve|Típus|Leírás|  
|-----------------|--------|---------------|  
|Fejlécek|Objektum|A HTTP-kérés fejlécébe.|  
|Törzs|Objektum|A http-kérelem törzsét.|  

A webhook művelet vonatkozó korlátozások is megadhatók a azonos módon [HTTP aszinkron korlátok](#asynchronous-limits).
  

## <a name="conditions"></a>Feltételek  

Bármely eseményindító egy vagy több feltételt segítségével határozza meg, hogy a munkafolyamat futtasson-e vagy sem. Példa:  

```json
"dailyReport" : {
    "type": "recurrence",
    "conditions": [ {
        "expression": "@parameters('sendReports')"
    } ],
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

Ebben az esetben a jelentés csak eseményindítók során a munkafolyamat `sendReports` paraméter értéke igaz. Végezetül feltételek hivatkozhatnak az állapotkód: az eseményindító. Például sikerült indítsa el a munkafolyamat csak akkor, ha a webhely adja vissza egy állapotkód: 500, az alábbiak szerint:
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> Ha bármely kifejezés hivatkozik az állapotkód: az eseményindító \(valamilyen módon\), az alapértelmezett viselkedés \(eseményindító csak a 200-as \(OK\) \) váltja fel. Például, ha azt szeretné, állapotkód: 200-as és a 201-es állapotkód is elindítható, fel kell vennie: `@or(equals(triggers().code, 200),equals(triggers().code,201))` a feltételként.  
  
## <a name="start-multiple-runs-for-a-request"></a>Indítsa el a kérelmek több futtatása

Az egy kérelemhez több futtatások indítsa `splitOn` akkor hasznos, például, ha azt szeretné, és egy végpontot, amely több új elemek közötti lekérdezési időközök kérdezze le.
  
A `splitOn`, a válasz forgalma, amely tartalmazza a tömb elemek, amelyek az eseményindító futtató indításához használni kívánt belül tulajdonság megadása. Tegyük fel, hogy az API-k, a következő választ ad vissza, amely rendelkezik:  
  
```json
{
    "Status" : "success",
    "Rows" : [
        {  
            "id" : 938109380,
            "name" : "mycoolrow"
        },
        {
            "id" : 938109381,
            "name" : "another row"
        }
    ]
}
```
  
A Logic Apps alkalmazást csak a sorok tartalom van szüksége, hogyan hozhat létre például ebben a példában az eseményindító:  
  
```json
"mysplitter" : {
    "type" : "http",
    "recurrence": {
        "frequency": "Minute",
        "interval": "1"
    },
    "intputs" : {
        "uri" : "https://mydomain.com/myAPI",
        "method" : "GET"
    },
    "splitOn" : "@triggerBody()?.Rows"
}
```
  
Ezt követően a munkafolyamat-definíciót, `@triggerBody().name` adja vissza `mycoolrow` az első alkalommal történő futtatásakor, és `another row` második futtatáshoz. Az eseményindító kimenetek kinézetét ebben a példában:  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

Igen, ha `SplitOn`, nem olvasható be a tulajdonságokat a tömb kívül ebben az esetben a `Status` mező.  
  
> [!NOTE]  
> A jelen példában használjuk a `?` operátor elkerülhető a hibát, ha a `Rows` tulajdonság nincs jelen. 
  
## <a name="single-run-instance"></a>Egyszeri futtatási példánya

Beállíthatja, hogy eseményindítókat, amelyek rendelkeznek egy ismétlődési tulajdonság csak akkor működik, amikor az összes aktív futtatása befejeződött. Ütemezett ismétlődése következik be, amíg a folyamatban lévő futtassa, ha az eseményindító kihagyja, és megvárja a következő ütemezett ismétlési időköz újra.

A beállítás a művelet lehetőségeit:

```json
"triggers": {
    "mytrigger": {
        "type": "http",
        "inputs": { ... },
        "recurrence": { ... },
        "operationOptions": "singleInstance"
    }
}
```

## <a name="types-and-inputs"></a>Típusok és a bemeneti adatok  

Nincsenek számos különböző típusú műveletek, az egyedi viselkedését. Gyűjtemény műveletek önmagába sok más műveleteket is tartalmazhat.

### <a name="standard-actions"></a>Standard műveletek  

-   **HTTP** Ez a művelet HTTP webes végpont hívja.  
  
-   **ApiConnection** \- Ez a művelet a HTTP-művelet viselkedik, de a Microsoft által felügyelt API-k.  
  
-   **ApiConnectionWebhook** \- például HTTPWebhook, de a Microsoft által felügyelt API-kat használ.  
  
-   **Válasz** \- Ez a művelet bejövő válasz meghatározása.  
  
-   **Várjon, amíg** \- egyszerű művelet várakozik a rögzített méretű idő vagy egy adott időpontig.  
  
-   **Munkafolyamat** \- Ez a művelet egy beágyazott munkafolyamat jelöli.  

-   **Függvény** \- Ez a művelet egy Azure-függvény jelöli.

### <a name="collection-actions"></a>Gyűjtemény műveletek

-   **Hatókör** \- Ez a művelet akkor más műveletek logikai csoportosítása.

-   **Az állapot** \- Ez a művelet egy kifejezés kiértékelése, és végrehajtja a megfelelő eredmény ágat.

-   **ForEach** \- ismétlési művelet tömb telepítéseket, és minden elemhez belső műveleteket hajtja végre.

-   **Amíg** \- ismétlési művelet belső műveleteket hajt végre, amíg egy feltétel eredménye igaz.
  
Írja be a van más **bemenetek** , amely egy művelet viselkedését határozza meg.  
  
## <a name="http-action"></a>HTTP-művelet  

HTTP-műveletek hívható meg a megadott végpont, és ellenőrizze a válasz határozza meg, hogy a munkafolyamat fusson-e. A **bemenetek** objektum veszi a HTTP-hívás létrehozásához szükséges paraméterek készletét:  
  
|Elem neve|Szükséges|Típus|Leírás|  
|----------------|------------|--------|---------------|  
|Módszer|Igen|Karakterlánc|A következő HTTP-metódus egyike lehet: **beolvasása**, **POST**, **PUT**, **törlése**, **javítás**, vagy **HEAD**|  
|URI|Igen|Karakterlánc|A http vagy https-végpont is nevezett. Hossza legfeljebb 2 KB.|  
|Lekérdezések|Nem|Objektum|A lekérdezési paraméterek hozzáadása az URL-címet jelöli. Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` URL-címét.|  
|Fejlécek|Nem|Objektum|A fejlécek, a kérelemben küldött mindegyikének jelöli. Ha például a nyelv, és írja be a kérelem:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Törzs|Nem|Objektum|A tartalom a végpontnak küldött jelöli.|  
|retryPolicy|Nem|Objektum|Lehetővé teszi testre szabhatja az újrapróbálási viselkedése 4xx vagy 5xx hibákat.|  
|operationsOptions|Nem|Karakterlánc|A speciális viselkedés felülbírálásához csoportját határozza meg.|  
|Hitelesítés|Nem|Objektum|A metódust, hogy van-e a kérelem hitelesítése jelöli. Ezen az objektumon, lásd: [Feladatütemező kimenő hitelesítési](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Ütemező túl van egy több támogatott tulajdonságot: `authority`. Alapértelmezés szerint ez a `https://login.windows.net` Ha nincs megadva, de használhat például egy másik célközönség`https://login.windows\-ppe.net`|  
  
HTTP-műveletek \(és API-kapcsolat\) műveletek támogatási házirendek próbálkozzon újra. Újrapróbálkozási házirendje érvényes időszakos hibák, mint a HTTP-állapotkódok jellemzőek 408 429 és 5xx minden csatlakozási kivétel mellett. Ez a házirend leírását használja a *retryPolicy* objektum megadva, az itt látható:
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
Az újrapróbálkozási időköz az ISO 8601 formátumban van megadva. Ezt az időközt értéke a minimális és az alapértelmezett 20 másodperc, a maximális érték pedig egy óra. Az alapértelmezett és a maximális újrapróbálkozási számláló négy óra. Ha nincs megadva az újrapróbálkozási házirend-definíció, egy `fixed` stratégia használatos az alapértelmezett értékekkel újrapróbálkozási számát és az időközt. Tiltsa le az újrapróbálkozási házirendet, adja meg a típus `None`.  
  
Például a következő tevékenységek ismétlése beolvasásához használt a legújabb híreket kétszer, ha nincsenek időszakos hibák, három végrehajtások, 30 másodperces késleltetéssel minden kísérlet között összesen:  
  
```json
"latestNews" : {
    "type": "http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT30S",
            "count": 2
        }
    }
}
```
### <a name="asynchronous-patterns"></a>Aszinkron minták

Alapértelmezés szerint az összes HTTP-alapú műveletek támogatja a szabványos aszinkron művelet szabályt. Igen, ha a távoli kiszolgáló, az azt jelzi, hogy a kérelem elfogadása egy 202 feldolgozásra \(elfogadott\) választ, a Logic Apps motor tartja a lekérdezés egy terminál állapot eléréséig hely válaszfejléc megadott URL- \(nem\-202 válasz\).  
  
A korábban leírt aszinkron viselkedés letiltásához állítsa be a `DisableAsyncPattern` lehetősége a művelet bemeneti adatok. A művelet kimenetének ebben az esetben a kiszolgáló válaszát kezdeti 202 alapul.  
  
```json
"invokeLongRunningOperation" : {
    "type": "http",
    "inputs": {
        "method": "POST",
        "uri": "https://host.example.com/resources"
    },
    "operationOptions": "DisableAsyncPattern"
}
```

#### <a name="asynchronous-limits"></a>Aszinkron korlátok

Egy aszinkron mintát is korlátozni az egy adott időintervallumban időtartamát.  Ha az időtartam nem érte el a Terminálszolgáltatások állapot eltelte után a művelet állapota lesz megjelölve `Cancelled` egy kóddal `ActionTimedOut`.  ISO 8601 formátumban megadott korlát időkorlát.  Korlátozások a következő szintaxissal adható meg:

``` json
"<action-name>": {
    "type": "workflow|webhook|http|apiconnectionwebhook|apiconnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="api-connection"></a>API-kapcsolat  

API-kapcsolat a Microsoft által felügyelt összekötők hivatkozó művelet.
Ez a művelet érvényes kapcsolat, és az API és a szükséges paraméterek hivatkozás szükséges.

|Elem neve|Szükséges|Típus|Leírás|  
|----------------|------------|--------|---------------|  
|állomás|Igen|Objektum|Az összekötővel kapcsolatos adatokat, például a runtimeUrl és a kapcsolat objektum hivatkozását jelenti.|
|Módszer|Igen|Karakterlánc|A következő HTTP-metódus egyike lehet: **beolvasása**, **POST**, **PUT**, **törlése**, **javítás**, vagy **HEAD**|  
|Elérési út|Igen|Karakterlánc|Az API-művelet elérési útját.|  
|Lekérdezések|Nem|Objektum|A lekérdezési paraméterek hozzáadása az URL-címet jelöli. Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` URL-címét.|  
|Fejlécek|Nem|Objektum|A fejlécek, a kérelemben küldött mindegyikének jelöli. Ha például a nyelv, és írja be a kérelem:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Törzs|Nem|Objektum|A tartalom a végpontnak küldött jelöli.|  
|retryPolicy|Nem|Objektum|Lehetővé teszi testre szabhatja az újrapróbálási viselkedése 4xx vagy 5xx hibákat.|  
|operationsOptions|Nem|Karakterlánc|A speciális viselkedés felülbírálásához csoportját határozza meg.|  

```json
"Send_Email": {
    "type": "apiconnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "method": "post",
        "body": {
            "Subject": "New Tweet from @{triggerBody()['TweetedBy']}",
            "Body": "@{triggerBody()['TweetText']}",
            "To": "me@example.com"
        },
        "path": "/Mail"
    },
    "runAfter": {}
    }
```

## <a name="api-connection-webhook-action"></a>API-kapcsolat webhook művelet

```json
"Send_approval_email": {
    "type": "apiconnectionwebhook",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "body": {
            "Message": {
                "Subject": "Approval Request",
                "Options": "Approve, Reject",
                "Importance": "Normal",
                "To": "me@email.com"
            }
        },
        "path": "/approvalmail",
        "authentication": "@parameters('$authentication')"
    },
    "runAfter": {}
}
```

A webhook művelet vonatkozó korlátozások is megadhatók a azonos módon [HTTP aszinkron korlátok](#asynchronous-limits).
  
## <a name="response-action"></a>Válasz művelet  

A művelet típusa a teljes válasz forgalma a HTTP-kérelem tartalmazza, és egy statusCode, a törzs és a fejlécek tartalmazza:  
  
```json
"myresponse" : {
    "type" : "response",
    "inputs" : {
        "statusCode" : 200,
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        }
    },
    "runAfter": {}
}
```
  
A válasz műveletnek speciális egyéb műveletek nem vonatkozó korlátozások. Konkrétan:  
  
-   Sérülésével kapcsolatos válaszműveletek nem lehet a definícióban párhuzamos, mert a bejövő kérelem determinisztikus választ szükség.  
  
-   Ha egy válasz művelet elérése után a bejövő kérelem kapott választ, a művelet nem sikerült tekinthető \(ütközés\), és ennek eredményeképpen a Futtatás `Failed`.  
  
-   A válaszműveleteket egy munkafolyamatban nem lehet `splitOn` a eseményindítójára, mert egy hívás hatására hány futtatása. Ennek köszönhetően ez kell ellenőriznie a folyamat PUT és ok egy hibás kérés esetén.  
  
## <a name="wait-action"></a>Várjon, amíg a művelet  

A `wait` művelet felfüggeszti a munkafolyamat végrehajtása a megadott intervallumban. Például várjon 15 percig, használhatja a kódrészletet:  
  
```json
"waitForFifteenMinutes" : {
    "type": "wait",
    "inputs": {
        "interval": {
            "unit" : "minute",
            "count" : 15
        }
    }
}
```  
  
Azt is megteheti várakozási idő az adott néhány percet, használhatja a példa:  
  
```json
"waitUntilOctober" : {
    "type": "wait",
    "inputs": {
        "until": {
            "timestamp" : "2016-10-01T00:00:00Z"
        }
    }
}
```
  
> [!NOTE]  
> A várakozási időtartama vagy adható a **időköz** objektum vagy a **amíg** objektum, de soha sem egyszerre mindkettőre.  
  
|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|időköz|Nem|Objektum|A várakozási időtartama idő alapján.|  
|időköze|Igen|Karakterlánc|Egy intervallumok: másodperc, perc, óra, nap, hét, hónap, év.|  
|időköz száma|Igen|Karakterlánc|A megadott belső egység alapján időtartama.|  
|amíg|Nem|Objektum|A várakozási időtartama időben a pontok alapján.|  
|amíg időbélyeg|Igen|Karakterlánc|Karakterlánc &#124; A pont UTC formátumban, ha a lejár a várakozási idő.|  

## <a name="query-action"></a>Lekérdezési művelet

A `query` művelet lehetővé teszi, hogy egy tömb feltétel alapján szűrni. Például válassza ki a 2-nél nagyobb számot, használhatja:

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

A kimenet a `query` művelete olyan tömb, amely rendelkezik, amelyek megfelelnek a következő feltételt: a bemeneti tömb elemei.

> [!NOTE]
> Ha nincs érték felel meg a `where` , az eredmény feltétele egy üres tömb.

|Név|Szükséges|Típus|Leírás|
|--------|------------|--------|---------------|
|A|Igen|A tömb|A forrástömb.|
|Ha|Igen|Karakterlánc|A forrás tömb egyes elemei a vonatkozó feltételt.|

## <a name="select-action"></a>Kijelölési művelet

A `select` művelet lehetővé teszi, hogy a tömb egyes elemei projekt be új értéket.
Például egy tömb számok alakítani egy objektumokból álló tömb, használhatja:

```json
"SelectNumbers" : {
    "type": "select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

A kimenetét a `select` művelete olyan tömb, amely a bemeneti tömb azonos számossága tartozik minden elem által meghatározott átalakítva a `select` tulajdonság. Ha a bemeneti érték egy üres tömb, a kimeneti is üres tömb.

|Név|Szükséges|Típus|Leírás|
|--------|------------|--------|---------------|
|A|Igen|A tömb|A forrástömb.|
|Válassza ki|Igen|Bármelyik|A forrás tömb egyes elemei alkalmazandó leképezési.|

## <a name="terminate-action"></a>Leállítási művelet

A megszakítási művelet munkafolyamat futtatás, bármilyen az üzenetsoroktól műveletek megszakad, és kihagyja a fennmaradó műveletek végrehajtása leáll. Ahhoz például, hogy leállítja állapottal futtató **sikertelen**, használhatja a következő kódrészletet:

```json
"HandleUnexpectedResponse" : {
    "type": "terminate",
    "inputs": {
        "runStatus" : "failed",
        "runError": {
            "code": "UnexpectedResponse",
            "message": "Received an unexpected response.",
        }
    }
}
```

> [!NOTE]
> A megszakítási művelet nem érinti a művelet már befejeződött.

|Név|Szükséges|Típus|Leírás|
|--------|------------|--------|---------------|
|runStatus|Igen|Karakterlánc|A futtatási állapot cél. Vagy **sikertelen** vagy **megszakítva**.|
|runError|Nem|Objektum|A hiba részletes adatait. Csak támogatott, ha **runStatus** értéke **sikertelen**.|
|runError kód|Nem|Karakterlánc|A futtatási hibakód.|
|runError üzenet|Nem|Karakterlánc|A futtatási hibaüzenet.|

## <a name="compose-action"></a>A művelet összeállítása

Az új művelet lehetővé teszi, hogy egy tetszőleges objektum hozhatja létre. Az új művelet eredménye a bemenet kiértékelése eredményét. Például a compose művelet segítségével több művelet kimenetének egyesítése:

```json
"composeUserRecord" : {
    "type": "compose",
    "inputs": {
        "firstName": "@actions('getUser').firstName",
        "alias": "@actions('getUser').alias",
        "thumbnailLink": "@actions('lookupThumbnail').url"
        }
    }
}
```

> [!NOTE]
> A **Compose** művelet is segítségével hozza létre a kimenetet, többek között az objektumok, tömbök és bármely más típusból XML és bináris hasonlóan a logic apps natívan támogatja.

## <a name="table-action"></a>Tábla művelet

A `table` lehetővé teszi, hogy elemeket tömbje alakíthatja át egy **CSV** vagy **HTML** tábla.

Tegyük fel, hogy @triggerBodyvan)

```json
[{
  "id": 0,
  "name": "apples"
},{
  "id": 1, 
  "name": "oranges"
}]
```

A művelet kell definiálni, így

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

A fenti akkor az eredmény

<table><thead><tr><th>id</th><th>név</th></tr></thead><tbody><tr><td>0</td><td>almák</td></tr><tr><td>1</td><td>narancs</td></tr></tbody></table>"

Ahhoz, hogy a tábla testreszabása, explicit módon megadhatja az oszlopok. Példa:

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html",
        "columns": [{
          "header": "produce id",
          "value": "@item().id"
        },{
          "header": "description",
          "value": "@concat('fresh ', item().name)"
        }]
    }
}
```

A fenti akkor az eredmény

<table><thead><tr><th>Készítsen azonosítója</th><th>Leírás</th></tr></thead><tbody><tr><td>0</td><td>friss almák</td></tr><tr><td>1</td><td>friss narancs</td></tr></tbody></table>"

Ha a `from` tulajdonság értéke üres tömb, a program üres táblát kimenete.

|Név|Szükséges|Típus|Leírás|
|--------|------------|--------|---------------|
|A|Igen|A tömb|A forrástömb.|
|Formátumban|Igen|Karakterlánc|A formátum vagy **CSV** vagy **HTML**.|
|oszlopok|Nem|A tömb|Az oszlopok. Lehetővé teszi, hogy a tábla alapértelmezett alakját felülbírálására.|
|Oszlopfejléc|Nem|Karakterlánc|Az oszlop fejlécében.|
|oszlop értéke|Igen|Karakterlánc|Az oszlop értéke.|

## <a name="workflow-action"></a>Munkafolyamat-művelet   

|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|állomás azonosítója|Igen|Karakterlánc|A munkafolyamatot, amely a hívni kívánt erőforrás-azonosító.|  
|állomás Eseményindító_neve|Igen|Karakterlánc|A meghívni kívánt eseményindító nevét.|  
|Lekérdezések|Nem|Objektum|A lekérdezési paraméterek hozzáadása az URL-címet jelöli. Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` URL-címét.|  
|Fejlécek|Nem|Objektum|A fejlécek, a kérelemben küldött mindegyikének jelöli. Ha például a nyelv, és írja be a kérelem:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Törzs|Nem|Objektum|A végpontnak küldött hasznos jelöli.|  
  
```json
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName " : "mytrigger001"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },  
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "runAfter": {}
    }
```
  
Hozzáférés-ellenőrzést a munkafolyamat készült \(pontosabban az eseményindító\), ami azt jelenti, hozzá kell férnie a munkafolyamat.  
  
A kimeneteinek a `workflow` művelet alapján a meghatározott a `response` alárendelt munkafolyamat műveletét. Ha nincs megadva, a `response` műveletet, majd a kimenetek üresek.  

## <a name="function-action"></a>Függvény művelet   

|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|függvény azonosítója|Igen|Karakterlánc|A függvény meghívása kívánt erőforrás-azonosító.|  
|Módszer|Nem|Karakterlánc|A függvény hívásához használt HTTP-metódus. Alapértelmezés szerint van `POST` Ha nincs megadva.|  
|Lekérdezések|Nem|Objektum|A lekérdezési paraméterek hozzáadása az URL-címet jelöli. Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` URL-címét.|  
|Fejlécek|Nem|Objektum|A fejlécek, a kérelemben küldött mindegyikének jelöli. Ahhoz például, hogy állítsa be a nyelvét és típusát kérelem: `"headers" : { "Accept-Language": "en-us" }`.|  
|Törzs|Nem|Objektum|A végpontnak küldött hasznos jelöli.|  

```json
"myfunc" : {
    "type" : "Function",
    "inputs" : {
        "function" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Web/sites/myfuncapp/functions/myfunc"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },  
        "headers" : {
            "x-ms-date" : "@utcnow()"
        },
        "method" : "POST",
    "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "runAfter": {}
}
```

A logikai alkalmazás mentésekor végezzük a hivatkozott függvény néhány ellenőrzése:
-   A funkció eléréséhez szüksége.
-   Szabványos HTTP-eseményindítóval vagy általános JSON webhook eseményindító csak engedélyezve van.
-   A megadott útvonal nem tartalmazhat.
-   Csak "függvény" és "névtelen" jogosultsági szint engedélyezett.

Az eseményindító URL-cím lekérése, gyorsítótárazva, és futásidőben használja. Így bármilyen műveletet érvényteleníti a gyorsítótárazott URL-címe, ha a művelet futásidőben nem sikerül. Ez, mentse a logikai alkalmazást újra, és emiatt logikai alkalmazás kérhető le, és az indítási URL-cím gyorsítótárazása újra.

## <a name="collection-actions-scopes-and-loops"></a>Gyűjtemény műveletek (hatókörök és hurkok)

Néhány művelettípusok műveletek belül maguk is tartalmazhat. A gyűjteményen belül hivatkozás műveletek közvetlenül a gyűjtemény kívül lehet hivatkozni. Ha az Ön által definiált `http` hatókörben, `@body('http')` a munkafolyamaton belül bárhol továbbra is érvényes. A gyűjteményen belül műveletek is `runAfter` belül ugyanaz a gyűjtemény csak más műveleteket.

## <a name="scope-action"></a>Hatókör művelet

A `scope` művelet lehetővé teszi, hogy logikailag a munkafolyamat műveleteit.

|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|műveletek|Igen|Objektum|Belső műveletek végrehajtásához a hatókörén belül|

```json
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```

## <a name="foreach-action"></a>ForEach-művelet

Ez a ismétlési művelet tömb telepítéseket, és minden elemhez belső műveleteket hajtja végre. Alapértelmezés szerint a foreach hurok végrehajtja a párhuzamos (20 végrehajtások párhuzamosan egyszerre). Végrehajtási szabályok használatával beállíthatja a `operationOptions` paraméter.

|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|műveletek|Igen|Objektum|Belső műveletek végrehajtásához a hurkon belül|
|foreach|Igen|Karakterlánc|Az ismétlés a tömb|
|operationOptions|nem|Karakterlánc|Viselkedés művelet lehetőségeket. Jelenleg csak `sequential` ismétlési végrehajtani egymás után (az alapértelmezés lesz párhuzamos)|

```json
"forEach_email": {
    "type": "foreach",
    "foreach": "@body('email_filter')",
    "actions": {
        "send_email": {
            "type": "ApiConnection",
            "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
            }
        }
    },
    "runAfter":{
        "email_filter": [ "Succeeded" ]
    }
}
```

## <a name="until-action"></a>Amíg a művelet

Ismétlési művelet belső műveleteket hajtja végre, amíg feltétel eredménye igaz.

|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|műveletek|Igen|Objektum|Belső műveletek végrehajtásához a hurkon belül|
|kifejezés|Igen|Karakterlánc|A kifejezés kiértékelése mindegyik iteráció után|
|Korlát|igen|Objektum|A hurok - legalább egy korlát korlátok kell megadni.|
|Száma|nem|int|A korlát végrehajtható ismétlések száma|
|Időtúllépés|nem|Karakterlánc|Mennyi ideig kell a hurok időtúllépés.  ISO 8601 formátumban|


```json
 "Until_succeeded": {
    "actions": {
        "Http": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "expression": "@equals(outputs('Http')['statusCode', 200)",
    "limit": {
        "count": 1000,
        "timeout": "PT1H"
    },
    "runAfter": {},
    "type": "Until"
}
```

## <a name="conditions---if-action"></a>Feltételek – Ha a művelet

A `If` művelet lehetővé teszi, hogy olyan feltétel értékelése, majd hajtsa végre egy ágat, hogy a kifejezés eredménye alapján `true`.

|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|műveletek|Igen|Objektum|Belső műveletek hajtható végre, ha a kifejezés eredménye`true`|
|kifejezés|Igen|Karakterlánc|A kiértékelendő kifejezés|
|más|nem|Objektum|Belső műveletek hajtható végre, ha a kifejezés eredménye`false`|
  
```json
"My_condition": {
    "actions": {
        "If_true": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "else": {
        "actions": {
            "if_false": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://myurl"
                },
                "runAfter": {},
                "type": "Http"
            }
        }
    },
    "expression": "@equals(triggerBody(), json(true))",
    "runAfter": {},
    "type": "If"
}
```  
  
Az alábbi táblázat példákat feltételek használatát kifejezések egy műveletben:  
  
|JSON-érték|eredménye|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|Bármely érték, amely true értékre állítására értékelné ki ezt az állapotot adja át. Csak a logikai kifejezések használhatók. Más típusúra átalakítása logikai érték, használja a függvényt `empty`, `equals`.|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|Összehasonlító függvény támogatott. Itt például a művelet csak során végrehajt act1 eredménye meghaladja a küszöbértéket.|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|Logika funkciók is támogatottak a beágyazott logikai kifejezésen létrehozásához. Ebben az esetben a művelet hajtja végre, amikor act1 eredménye 100 alatt vagy a küszöbérték felett.|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|Tömb funkciók segítségével ellenőrizze, hogy egy tömb rendelkezik-e az elemeket. Ebben az esetben a művelet végrehajtása, ha a hibák tömb üres.| 
|`"expression": "parameters('hasSpecialAction')"`|Hiba – nem egy érvényes, mert @ van szükség a feltétel feltételeket.|  
  
Ha a feltétel sikeresen, a feltétel jelölése `Succeeded`. Műveletek belül vagy a `actions` vagy `else` objektumok értékelhetők `Succeeded` hajtotta végre, és sikeresen befejeződött, `Failed` hajtotta végre, és nem sikerült, vagy `Skipped` mikor nem hajtotta végre a fiókirodában.

## <a name="next-steps"></a>Következő lépések

[A munkafolyamat szolgáltatás REST API](https://docs.microsoft.com/rest/api/logic/workflows)
