---
title: "aaaWorkflow műveletek és eseményindítók - Azure Logic Apps |} Microsoft Docs"
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
ms.openlocfilehash: 857927b7d7df3fc9cdc4931ffdb613efde0db9f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a>Munkafolyamat-műveleteket, és az Azure Logic Apps eseményindítók

A Logic apps eseményindítók és műveletek alkotják. Nincsenek eseményindítók hat típusú. Minden különböző felület és a különböző viselkedés rendelkezik. Hello hello részleteinek megtekintésével is olvashat egyéb részletek [Munkafolyamatdefiníciós nyelve](logic-apps-workflow-definition-language.md).  
  
További információ az eseményindítók és műveletek és hogyan használhatja őket toobuild logic apps tooimprove toolearn olvasni az üzleti folyamatokat és munkafolyamatok.  
  
### <a name="triggers"></a>Eseményindítók  

Egy eseményindító is kezdeményezhető a logic app munkafolyamatot futtató hello hívások határozza meg. Az alábbiakban hello két különböző módon tooinitiate a munkafolyamatot futtató:  
  
-   A lekérdezési eseményindító  

-   A leküldéses eseményindító - hívó hello által [munkafolyamat szolgáltatás REST API](https://docs.microsoft.com/rest/api/logic/workflows)  
  
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
    "splitOn" : "<property toocreate runs for>",
    "operationOptions": "<operation options on hello trigger>"
}
```

### <a name="trigger-types-and-their-inputs"></a>Az eseményindítók típusai és azok  

Az ilyen típusú eseményindítók használhatja:
  
-   **Kérelem** \- teszi hello logic app, toocall végpont  
  
-   **Ismétlődés** \- következik be a meghatározott ütemezés szerint  
  
-   **HTTP** \- HTTP webes végpont kérdezi le. hello HTTP-végpont meg kell felelnie a tooa meghatározott eseményindító szerződés \- használva egy 202\-aszinkron mintát, vagy egy tömb vissza  
  
-   **ApiConnection** \- indul el, például hello HTTP szavazások, azonban kihasználja hello [Microsoft által felügyelt API-k](https://docs.microsoft.com/azure/connectors/apis-list)  
  
-   **HTTPWebhook** \- megnyit egy végpontot, hasonló toohello kézi indítási, azonban ez szükségessé teszi ki tooa megadott URL-cím tooregister és regisztrációjának megszüntetése  
  
-   **ApiConnectionWebhook** \- működik, mint ha kihasználja a Microsoft által felügyelt API-k hello HTTPWebhook eseményindító hello       
    Minden eseményindító más rendelkezik **bemenetek** , amely meghatározza, hogy a viselkedését.  
  
## <a name="request-trigger"></a>Kérelem eseményindító  

Ehhez az eseményindítóhoz szolgál egy végpontot, amely meghívja a egy HTTP-kérelem tooinvoke keresztül a Logic Apps alkalmazást. A kérelem eseményindító ebben a példában néz ki:  
  
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
|Séma|Nem|A JSON-séma, amely megvizsgálja a bejövő kérelem hello. Útmutatás nyújtása a soron következő munkafolyamat lépéseket tudja, milyen tulajdonságok tooreference hasznos.|

tooinvoke ezt a végpontot kell toocall hello *listCallbackUrl* API. Lásd: [munkafolyamat szolgáltatás REST API](https://docs.microsoft.com/rest/api/logic/workflows).  
  
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

Ahogy látja, akkor egy egyszerű módon toorun munkafolyamat.  
  
|Elem neve|Szükséges|Leírás|  
|----------------|------------|---------------|  
|frequency|Igen|Milyen gyakran hello eseményindító végrehajtása. Használja a lehetséges értékek egyikét: másodperc, perc, óra, nap, hét, hónap vagy év|  
|interval|Igen|Hello tartozó hello Ismétlődési gyakoriság megadott időköz|  
|startTime|Nem|A startTime nélkül a UTC eltolás valósul meg, ha a timeZone szolgál.|  
|Időzóna|nem|A startTime nélkül a UTC eltolás valósul meg, ha a timeZone szolgál.|  
  
Egy eseményindító toostart végrehajtása a jövőbeli hello valamikor is ütemezhető. Például ha azt szeretné, hogy a heti jelentés minden hétfőn toostart ütemezhet hello logic app toostart minden hétfőn eseményindítót követő hello létrehozásával:  

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

HTTP-eseményindítók kérdezze le a megadott végpont és hello válasz toodetermine ellenőrizze, hogy hello munkafolyamat végre kell hajtani. hello bemenetek objektum paraméterek szükséges tooconstruct egy HTTP-hívás hello készlete hajtja végre:  
  
|Elem neve|Szükséges|Leírás|Típus|  
|----------------|------------|---------------|--------|  
|Módszer|igen|A következő HTTP-metódus hello egyike lehet: GET, POST, PUT, DELETE, a javítás vagy a HEAD|Karakterlánc|  
|URI|igen|hello http vagy https-végpont is nevezett. Legfeljebb 2 KB.|Karakterlánc|  
|Lekérdezések|Nem|Hello lekérdezési paraméterek tooadd toohello URL-CÍMRE képviselő objektum. Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` toohello URL-CÍMÉT.|Objektum|  
|Fejlécek|Nem|Egyes toohello kérelmet küldött hello fejlécek képviselő objektum. Például a tooset hello nyelvét és típusát kérelem lehetőséget:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|Objektum|  
|Törzs|Nem|Hello hasznos toohello végpont küldött képviselő objektum.|Objektum|  
|retryPolicy|Nem|Egy objektum, amely lehetővé teszi testre szabhatja a hello újrapróbálási viselkedése 4xx vagy 5xx hiba.|Objektum|  
|Hitelesítés|Nem|Jelöli hello módszer, amelynek a hello kérelem hitelesítése történik. Ezen az objektumon, lásd: [Feladatütemező kimenő hitelesítési](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Ütemező túl van egy több támogatott tulajdonságot: `authority` alapértelmezés szerint ez az érték van `https://login.windows.net` Ha nincs megadva, de használhat például egy másik célközönség`https://login.windows\-ppe.net`|Objektum|  
  
hello HTTP-eseményindítóval hello HTTP API tooconform az egy adott minta toowork jól igazodik a Logic Apps alkalmazást igényli. A következő mezők hello szükségesek hozzá:  
  
|Válasz|Leírás|  
|------------|---------------|  
|Állapotkód|Állapotkód 200 \(OK\) toocause futását. Bármely más állapotkód futtató nem okoz.|  
|Próbálja meg újra\-fejléc után|Hány másodperc múlva hello logikai alkalmazás kérdezze le az hello végpont újra.|  
|Hely fejléc|hello URL-cím toocall a hello következő lekérdezési időközt. Ha nincs megadva, hello eredeti URL-címet használja.|  
  
Íme néhány példa a kérelmek különböző típusú különböző beállításokat:  
  
|Válaszkód|Próbálja meg újra\-után|Viselkedés|  
|-----------------|----------------|------------|  
|200|\(egyik sem\)|Nem érvényes eseményindító, újrapróbálkozási\-után van, kötelező, vagy más hello motor soha nem hello következő kérés lekérdezések.|  
|202|60|Nem indítható el, hello munkafolyamat. hello következő kísérlet történik, egy perc alatt.|  
|200|10|Hello munkafolyamat futtatásához, és újra keressen további tartalmat 10 másodperc múlva.|  
|400|\(egyik sem\)|Hibás kérés, hello munkafolyamat nem futnak. Ha nincs **ismételje meg a házirend** definiált, majd hello alapértelmezett házirend. Az újrapróbálkozások számát hello elérése után hello eseményindító már nem érvényes.|  
|500|\(egyik sem\)|Kiszolgálóhiba, akkor futtassa a hello munkafolyamat.  Ha nincs **ismételje meg a házirend** definiált, majd hello alapértelmezett házirend. Az újrapróbálkozások számát hello elérése után hello eseményindító már nem érvényes.|  
  
egy HTTP-eseményindítóval hello kimenetének ebben a példában látható:  
  
|Elem neve|Leírás|Típus|  
|----------------|---------------|--------|  
|Fejlécek|hello fejlécek hello http-válaszok.|Objektum|  
|Törzs|hello hello HTTP-válasz törzsében.|Objektum|  
  
## <a name="api-connection-trigger"></a>API-kapcsolat eseményindító  

hello API-kapcsolat eseményindító hasonló toohello HTTP-eseményindítóval alapvető funkciókat, hogy a rendszer. Hello paraméterek hello művelet azonosítására szolgáló azonban eltérőek. Például:  
  
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
|állomás|Igen||hello ApiApp átjáró és az azonosítóját tárolja.|  
|Módszer|Igen|Karakterlánc|A következő HTTP-metódus hello egyike lehet: **beolvasása**, **POST**, **PUT**, **törlése**, **javítás**, vagy  **HEAD**|  
|Lekérdezések|Nem|Objektum|Jelöli hello lekérdezési paraméterek toobe hozzáadott toohello URL-CÍMÉT. Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` toohello URL-CÍMÉT.|  
|Fejlécek|Nem|Objektum|Egyes toohello kérelmet küldött hello fejlécek jelöli. Például a tooset hello nyelvét és típusát kérelem lehetőséget:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Törzs|Nem|Objektum|Hello hasznos toohello végpont küldött jelöli.|  
|retryPolicy|Nem|Objektum|Lehetővé teszi a toocustomize hello újrapróbálási viselkedése 4xx vagy 5xx hibákat.|  
|Hitelesítés|Nem|Objektum|Jelöli hello módszer, amelynek a hello kérelem hitelesítése történik. Ezen az objektumon, lásd: [Feladatütemező kimenő hitelesítésre](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)|  
  
a gazdagép hello tulajdonságok a következők:  
  
|Elem neve|Szükséges|Leírás|  
|----------------|------------|---------------|  
|API runtimeUrl|Igen|hello hello végpontja felügyelt API-t.|  
|Kapcsolat neve||Meg kell a hivatkozás tooa paraméter hívni `$connection` és hello felügyelt API-kapcsolat, amely a munkafolyamat által használt hello hello neve.|
  
az API-kapcsolat eseményindító hello kimenetének a következők:
  
|Elem neve|Típus|Leírás|  
|----------------|--------|---------------|  
|Fejlécek|Objektum|hello fejlécek hello http-válaszok.|  
|Törzs|Objektum|hello hello HTTP-válasz törzsében.|  
  
## <a name="httpwebhook-trigger"></a>HTTPWebhook eseményindító  

hello HTTPWebhook eseményindító megnyit egy végpontot, a hasonló toohello kézi indítási, de a hello HTTPWebhook eseményindító is meghívja a kimenő tooa megadott URL-cím tooregister és regisztrációjának megszüntetése. Íme egy példa hogyan nézhet ki egy HTTPWebhook eseményindító:  

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

Ezek a szakaszok számos nem kötelező, és hello Webhook hello viselkedését függ, mely szakaszok megadva, vagy nincs megadva.  
a Webhook tulajdonságainak hello a következők:  
  
|Elem neve|Szükséges|Leírás|  
|----------------|------------|---------------|  
|előfizetés|Nem|a kimenő kérelem is nevezett hello eseményindító jön létre, és elvégzi a kezdeti regisztrációs hello hello.|  
|előfizetés lemondása|Nem|hello kimenő kérelem hello eseményindító törlésekor.|  
  
-   **Előfizetés** hello kimenő hívás toostart figyelő tooevents jogosultságokat. Ez a hívás ugyanazokat a paramétereket, normál HTTP-műveletek hello tegye hello kezdődik. A kimenő kezdeményezték bármely idő hello munkafolyamat-módosítások bármely olyan módon, például amikor hello hitelesítő adatok már megkezdődött, vagy hello eseményindító bemeneti paraméterek módosítása.
  
    toosupport ez hívja, van egy új funkció: `@listCallbackUrl()`. Ez a függvény egy egyedi URL-címet az adott eseményindító a munkafolyamat adja vissza. Azt jelenti, hogy hello szolgáltatás REST használó végpontokon hello hello egyedi azonosítója.  
  
-   **Leiratkozhat** van meghívva, amikor egy művelet Renderelés ehhez az eseményindítóhoz érvénytelen, beleértve:  
  
    -   Törlése vagy hello eseményindító letiltása  
  
    -   Törlése vagy hello munkafolyamat letiltása  
  
    -   Törlése vagy hello előfizetés letiltása  
  
    hello logikai alkalmazás automatikusan meghívja a hello leiratkozhat művelet. hello paraméterek toothis függvény van mint a HTTP-eseményindítóval hello hello azonos.  
  
    hello hello HTTPWebhook eseményindító kimenetének hello bejövő kérelem hello tartalma:  
  
|Elem neve|Típus|Leírás|  
|-----------------|--------|---------------|  
|Fejlécek|Objektum|hello fejlécek hello HTTP-kérelem.|  
|Törzs|Objektum|hello hello http-kérelem törzse.|  

A webhook művelet vonatkozó korlátozások is megadhatók hello megegyező módon [HTTP aszinkron korlátok](#asynchronous-limits).
  

## <a name="conditions"></a>Feltételek  

Bármely eseményindító használhatja egy vagy több feltételek toodetermine e hello munkafolyamat futtasson-e vagy sem. Példa:  

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

Ebben az esetben a jelentés csak eseményindítók hello munkafolyamat közben hello `sendReports` paraméter értéke tootrue. Végezetül feltételek hivatkozhatnak hello állapotkód: hello eseményindító. Például sikerült indítsa el a munkafolyamat csak akkor, ha a webhely adja vissza egy állapotkód: 500, az alábbiak szerint:
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> Ha bármely kifejezés hivatkozik hello állapotkód: hello eseményindító \(valamilyen módon\), alapértelmezett viselkedést hello \(eseményindító csak a 200-as \(OK\) \) váltja fel. Például, ha azt szeretné, hogy a állapotkód 200-as és a 201-es állapotkód tootrigger, hogy tooinclude: `@or(equals(triggers().code, 200),equals(triggers().code,201))` a feltételként.  
  
## <a name="start-multiple-runs-for-a-request"></a>Indítsa el a kérelmek több futtatása

egyetlen kérelem több futtatások ki tookick `splitOn` akkor hasznos, például, ha azt szeretné, hogy egy végpontot, amely több új elemek közötti lekérdezési időközök toopoll.
  
A `splitOn`, belül, amely tartalmazza az elemet, amelyek mindegyike hello tömbje hello válasz hasznos hello tulajdonság megadása toouse toostart hello eseményindító futását. Tegyük fel, hogy rendelkezik egy, a válasz a következő hello visszaadó API:  
  
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
  
A Logic Apps alkalmazást csak a kell hello sorok tartalom, hogyan hozhat létre például ebben a példában az eseményindító:  
  
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
  
Ezt követően a hello munkafolyamat-definíciót, `@triggerBody().name` adja vissza `mycoolrow` a hello először futtatja, és `another row` a hello második futtató. hello eseményindító kimenetek kinézetét ebben a példában:  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

Igen, ha `SplitOn`, nem olvasható be, amelyek túlmutatnak hello tömb, ebben az esetben hello hello tulajdonságok `Status` mező.  
  
> [!NOTE]  
> A jelen példában használjuk hello `?` operátor toobe képes tooavoid egy hiba, ha hello `Rows` tulajdonság nincs jelen. 
  
## <a name="single-run-instance"></a>Egyszeri futtatási példánya

Konfigurálhatja az eseményindítók rendelkező tooonly tűz ismétlődési tulajdonság, ha az összes aktív futtatása befejeződött. Ütemezett ismétlődése következik be, amíg a folyamatban lévő futtassa, ha a hello eseményindító kihagyja, és megvárja a hello következő ütemezett ismétlődési időköz toocheck újra.

A beállítás hello művelet lehetőségeit:

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
  
-   **ApiConnection** \- , de ez a művelet úgy viselkedik, mint a HTTP-művelet hello, használja a Microsoft által felügyelt API-k hello.  
  
-   **ApiConnectionWebhook** \- például HTTPWebhook, de a Microsoft által felügyelt API-k által használt hello.  
  
-   **Válasz** \- Ez a művelet bejövő válasz meghatározása.  
  
-   **Várjon, amíg** \- egyszerű művelet várakozik a rögzített méretű idő vagy egy adott időpontig.  
  
-   **Munkafolyamat** \- Ez a művelet egy beágyazott munkafolyamat jelöli.  

-   **Függvény** \- Ez a művelet egy Azure-függvény jelöli.

### <a name="collection-actions"></a>Gyűjtemény műveletek

-   **Hatókör** \- Ez a művelet akkor más műveletek logikai csoportosítása.

-   **Az állapot** \- Ez a művelet egy kifejezés kiértékelése és hello megfelelő eredmény fiókirodai végrehajtja.

-   **ForEach** \- ismétlési művelet tömb telepítéseket, és minden elemhez belső műveleteket hajtja végre.

-   **Amíg** \- ismétlési művelet belső műveleteket hajt végre, amíg egy feltétel eredménye tootrue.
  
Írja be a van más **bemenetek** , amely egy művelet viselkedését határozza meg.  
  
## <a name="http-action"></a>HTTP-művelet  

HTTP-műveletek a megadott végpont hívja, és hello válasz toodetermine ellenőrizze, hogy hello munkafolyamat futtatásának. Hello **bemenetek** objektum veszi hello set paraméterek szükséges tooconstruct hello HTTP-hívás:  
  
|Elem neve|Szükséges|Típus|Leírás|  
|----------------|------------|--------|---------------|  
|Módszer|Igen|Karakterlánc|A következő HTTP-metódus hello egyike lehet: **beolvasása**, **POST**, **PUT**, **törlése**, **javítás**, vagy  **HEAD**|  
|URI|Igen|Karakterlánc|hello http vagy https-végpont is nevezett. Hossza legfeljebb 2 KB.|  
|Lekérdezések|Nem|Objektum|Hello lekérdezési paraméterek tooadd toohello URL-címet jelöli. Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` toohello URL-CÍMÉT.|  
|Fejlécek|Nem|Objektum|Egyes toohello kérelmet küldött hello fejlécek jelöli. Például a tooset hello nyelvét és típusát kérelem lehetőséget:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Törzs|Nem|Objektum|Hello hasznos toohello végpont küldött jelöli.|  
|retryPolicy|Nem|Objektum|Lehetővé teszi testre szabhatja a hello újrapróbálási viselkedése 4xx vagy 5xx hiba.|  
|operationsOptions|Nem|Karakterlánc|Különleges viselkedések toooverride hello csoportját határozza meg.|  
|Hitelesítés|Nem|Objektum|Jelöli hello módszer, amelynek a hello kérelem hitelesítése történik. Ezen az objektumon, lásd: [Feladatütemező kimenő hitelesítési](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Ütemező túl van egy több támogatott tulajdonságot: `authority`. Alapértelmezés szerint ez a `https://login.windows.net` Ha nincs megadva, de használhat például egy másik célközönség`https://login.windows\-ppe.net`|  
  
HTTP-műveletek \(és API-kapcsolat\) műveletek támogatási házirendek próbálkozzon újra. Újrapróbálkozási házirendje érvényes toointermittent hibák, mint a HTTP-állapot jellemzőek 408 429 és 5xx, továbbá tooany csatlakozási kivétel a kódok. Ez a házirend leírását hello segítségével *retryPolicy* objektum megadva, az itt látható:
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
hello újrapróbálkozási időköz hello ISO 8601 formátumban van megadva. Ez az időtartam alatt alapértelmezett és 20 másodperc, minimális érték rendelkezik hello maximális értéke pedig egy óra. hello alapértelmezett és a maximális újrapróbálkozások száma négy óra. Ha nincs megadva hello újrapróbálkozási házirend-definíció, egy `fixed` stratégia használatos az alapértelmezett értékekkel újrapróbálkozási számát és az időközt. toodisable hello újrapróbálkozási házirendje, állítsa be a típusa túl`None`.  
  
Például hello következő művelet újrapróbálja lekérdezésekor hello legfrissebb hírek kétszer, ha nincsenek időszakos hibák, három végrehajtások, 30 másodperces késleltetéssel minden kísérlet között összesen:  
  
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

Alapértelmezés szerint az összes HTTP-alapú műveletek támogatja hello szabványos aszinkron művelet szabályt. Így ha hello távoli kiszolgáló azt jelzi, hogy hello kérelem elfogadható egy 202 feldolgozásra \(elfogadott\) választ, a hello Logic Apps motor tartja a lekérdezés egy terminált eléréséig hello válasz hely fejlécben megadott hello URL-cím állapot \(nem\-202 válasz\).  
  
toodisable hello aszinkron viselkedés korábban leírt, állítsa be a `DisableAsyncPattern` hello művelet bemeneti lehetősége. Ebben az esetben hello kimeneti hello művelet hello kezdeti 202 hello kiszolgáló válasza alapján történik.  
  
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

Egy aszinkron mintát lehet korlátozni a duration tooa adott időintervallumban.  Ha hello alatt az időtartam alatt nem érte el a Terminálszolgáltatások állapot eltelte után hello művelet hello állapota lesz megjelölve `Cancelled` egy kóddal `ActionTimedOut`.  ISO 8601 formátumban megadott hello korlát időkorlát.  Korlátozások a szintaxis a következő hello adható meg:

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
Ez a művelet hivatkozás tooa érvényes kapcsolat, és a hello API és a szükséges paraméterek szükséges.

|Elem neve|Szükséges|Típus|Leírás|  
|----------------|------------|--------|---------------|  
|állomás|Igen|Objektum|Hello összekötővel kapcsolatos adatokat, például a hello runtimeUrl és a hivatkozás toohello kapcsolatobjektumot jelenti.|
|Módszer|Igen|Karakterlánc|A következő HTTP-metódus hello egyike lehet: **beolvasása**, **POST**, **PUT**, **törlése**, **javítás**, vagy  **HEAD**|  
|Elérési út|Igen|Karakterlánc|hello API művelet hello elérési útját.|  
|Lekérdezések|Nem|Objektum|Hello lekérdezési paraméterek tooadd toohello URL-címet jelöli. Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` toohello URL-CÍMÉT.|  
|Fejlécek|Nem|Objektum|Egyes toohello kérelmet küldött hello fejlécek jelöli. Például a tooset hello nyelvét és típusát kérelem lehetőséget:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Törzs|Nem|Objektum|Hello hasznos toohello végpont küldött jelöli.|  
|retryPolicy|Nem|Objektum|Lehetővé teszi testre szabhatja a hello újrapróbálási viselkedése 4xx vagy 5xx hiba.|  
|operationsOptions|Nem|Karakterlánc|Különleges viselkedések toooverride hello csoportját határozza meg.|  

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

A webhook művelet vonatkozó korlátozások is megadhatók hello megegyező módon [HTTP aszinkron korlátok](#asynchronous-limits).
  
## <a name="response-action"></a>Válasz művelet  

A művelet típusa hello teljes válasz hasznos a HTTP-kérelem tartalmazza, és egy statusCode, a törzs és a fejlécek tartalmazza:  
  
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
  
hello válasz műveletnek speciális tooother műveletek nem vonatkozó korlátozások. Konkrétan:  
  
-   Sérülésével kapcsolatos válaszműveletek nem lehet a definícióban párhuzamos, mert a mérvadó válasz toohello bejövő kérelmet.  
  
-   Ha egy válasz művelet elérése után hello bejövő kérelmet kapott választ hello művelet nem sikerült tekinthető \(ütközés\), és emiatt futtatása hello `Failed`.  
  
-   A válaszműveleteket egy munkafolyamatban nem lehet `splitOn` a eseményindítójára, mert egy hívás hatására hány futtatása. Ennek köszönhetően ez érvényesítendő hello folyamata PUT és ok egy hibás kérés esetén.  
  
## <a name="wait-action"></a>Várjon, amíg a művelet  

Hello `wait` művelet felfüggeszti a munkafolyamat-végrehajtási hello megadott időtartam alatt. Például a toowait 15 perc, a részlet is használhatja:  
  
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
  
Másik lehetőségként toowait időben adott néhány percet, amíg ez a példa használhatja:  
  
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
> hello várakozási időtartama vagy szövegtípusához hello **időköz** objektum vagy hello **amíg** objektum, de soha sem egyszerre mindkettőre.  
  
|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|interval|Nem|Objektum|hello várakozás időtartama idő alapján.|  
|időköze|Igen|Karakterlánc|Egy intervallumok: másodperc, perc, óra, nap, hét, hónap, év.|  
|időköz száma|Igen|Karakterlánc|A megadott belső egységgel hello alapján időtartama.|  
|amíg|Nem|Objektum|hello várakozás időtartama időben a pontok alapján.|  
|amíg időbélyeg|Igen|Karakterlánc|Karakterlánc &#124; időpontja (UTC) hello várakozási lejártakor hello pontot.|  

## <a name="query-action"></a>Lekérdezési művelet

Hello `query` művelet lehetővé teszi, hogy egy tömb feltétel alapján szűrni. 2-nél nagyobb tooselect számokat, például a következő szempontokat is használhatja:

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

hello kimenete hello `query` művelete olyan tömb, amely rendelkezik, amelyek megfelelnek a hello feltétel hello bemeneti tömb elemei.

> [!NOTE]
> Ha nincs érték kielégítéséhez hello `where` feltétel, hello eredménye üres tömb.

|Név|Szükséges|Típus|Leírás|
|--------|------------|--------|---------------|
|A|Igen|Tömb|hello forrástömb.|
|Ha|Igen|Karakterlánc|hello feltétel tooapply tooeach eleme hello forrástömb.|

## <a name="select-action"></a>Kijelölési művelet

Hello `select` művelet lehetővé teszi, hogy a tömb egyes elemei projekt be új értéket.
Például egy tömb a azokat egy tömb számú objektum, tooconvert használhatja:

```json
"SelectNumbers" : {
    "type": "select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

hello kimenete hello `select` művelete olyan tömb, amely rendelkezik hello azonos számossága, a bemeneti tömb egyes elemei át legyenek-e a hello definiált hello `select` tulajdonság. Hello bemenet üres tömb, hello kimeneti akkor is üres tömb.

|Név|Szükséges|Típus|Leírás|
|--------|------------|--------|---------------|
|A|Igen|Tömb|hello forrástömb.|
|Válassza ki|Igen|Bármelyik|hello leképezése tooapply tooeach eleme hello forrástömb.|

## <a name="terminate-action"></a>Leállítási művelet

hello megszakítási művelet leáll hello a munkafolyamat végrehajtásának futtatja, az üzenetsoroktól műveletek megszakad, és semmilyen további műveletet kihagyja. Például tooterminate állapotú futtató **sikertelen**, használhatja a következő kódrészletet hello:

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
> Már elvégzett műveletek nem érinti a hello leállítási művelet.

|Név|Szükséges|Típus|Leírás|
|--------|------------|--------|---------------|
|runStatus|Igen|Karakterlánc|hello cél futtatási állapot. Vagy **sikertelen** vagy **megszakítva**.|
|runError|Nem|Objektum|hello hiba részletes adatait. Csak támogatott, ha **runStatus** értéke túl**sikertelen**.|
|runError kód|Nem|Karakterlánc|Hiba a kódra hello.|
|runError üzenet|Nem|Karakterlánc|Futtassa a hibaüzenet a következő hello.|

## <a name="compose-action"></a>A művelet összeállítása

hello Compose művelet lehetővé teszi, hogy egy tetszőleges objektum hozhatja létre. hello hello kimenete írása műveleti bemenet kiértékelése hello eredménye. Például használhatja hello művelet toomerge kimenetének több művelet állítható össze:

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
> Hello **Compose** művelet lehet használt tooconstruct kimenetet, többek között az objektumok, tömbök és bármely más típusból XML és bináris hasonlóan a logic apps natívan támogatja.

## <a name="table-action"></a>Tábla művelet

Hello `table` lehetővé teszi egy elemeket tömbje tooconvert egy **CSV** vagy **HTML** tábla.

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

Hello művelet kell definiálni, így

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

a fenti hello eredményezne.

<table><thead><tr><th>id</th><th>név</th></tr></thead><tbody><tr><td>0</td><td>almák</td></tr><tr><td>1</td><td>narancs</td></tr></tbody></table>"

Rendelés toocustomize hello tábla explicit módon megadhat hello oszlopok. Példa:

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

a fenti hello eredményezne.

<table><thead><tr><th>Készítsen azonosítója</th><th>leírás</th></tr></thead><tbody><tr><td>0</td><td>friss almák</td></tr><tr><td>1</td><td>friss narancs</td></tr></tbody></table>"

Ha hello `from` tulajdonság értéke üres tömb, hello eredménye a program üres táblát.

|Név|Szükséges|Típus|Leírás|
|--------|------------|--------|---------------|
|A|Igen|Tömb|hello forrástömb.|
|Formátumban|Igen|Karakterlánc|formátumú, vagy hello **CSV** vagy **HTML**.|
|oszlopok|Nem|Tömb|hello oszlopok. Lehetővé teszi, hogy toooverride hello alapértelmezett hello tábla alakját.|
|Oszlopfejléc|Nem|Karakterlánc|hello hello oszlop fejlécében.|
|oszlop értéke|Igen|Karakterlánc|hello hello oszlop értéke.|

## <a name="workflow-action"></a>Munkafolyamat-művelet   

|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|állomás azonosítója|Igen|Karakterlánc|hello erőforrás-azonosító, amelyet az toocall hello munkafolyamat.|  
|állomás Eseményindító_neve|Igen|Karakterlánc|hello neve, amelyet az tooinvoke hello eseményindító.|  
|Lekérdezések|Nem|Objektum|Hello lekérdezési paraméterek tooadd toohello URL-címet jelöli. Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` toohello URL-CÍMÉT.|  
|Fejlécek|Nem|Objektum|Egyes toohello kérelmet küldött hello fejlécek jelöli. Például a tooset hello nyelvét és típusát kérelem lehetőséget:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Törzs|Nem|Objektum|Hello hasznos toohello végpont küldött jelöli.|  
  
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
  
Hozzáférés-ellenőrzést készült hello munkafolyamat \(pontosabban hello eseményindító\), ami azt jelenti, hogy toohello munkafolyamat kell hozzáférni.  
  
hello kiírja a hello `workflow` művelet alapján hello definiált `response` hello alárendelt munkafolyamat műveletét. Ha nincs megadva, a `response` műveletet, majd hello kimenetek üresek.  

## <a name="function-action"></a>Függvény művelet   

|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|függvény azonosítója|Igen|Karakterlánc|hello erőforrás-azonosító, amelyet az tooinvoke hello függvény.|  
|Módszer|Nem|Karakterlánc|HTTP-metódus hello tooinvoke hello funkció használata. Alapértelmezés szerint van `POST` Ha nincs megadva.|  
|Lekérdezések|Nem|Objektum|Hello lekérdezési paraméterek tooadd toohello URL-címet jelöli. Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` toohello URL-CÍMÉT.|  
|Fejlécek|Nem|Objektum|Egyes toohello kérelmet küldött hello fejlécek jelöli. Például a tooset hello nyelvet és a kérelem típusa: `"headers" : { "Accept-Language": "en-us" }`.|  
|Törzs|Nem|Objektum|Hello hasznos toohello végpont küldött jelöli.|  

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

Hello logikai alkalmazás mentésekor a hivatkozott hello függvény néhány ellenőrzést végezzük:
-   Toohave hozzáférés toohello függvény van szüksége.
-   Szabványos HTTP-eseményindítóval vagy általános JSON webhook eseményindító csak engedélyezve van.
-   A megadott útvonal nem tartalmazhat.
-   Csak "függvény" és "névtelen" jogosultsági szint engedélyezett.

hello indítási URL-cím lekérése, gyorsítótárazva, és futásidőben használja. Így az összes műveletet érvényteleníti gyorsítótárazott hello URL-címe, ha futásidőben hello művelet sikertelen lesz. toowork kerülheti, mentés hello logikai alkalmazás, amely miatt a logic app tooretrieve és hello indítási URL-cím gyorsítótárazása újra.

## <a name="collection-actions-scopes-and-loops"></a>Gyűjtemény műveletek (hatókörök és hurkok)

Néhány művelettípusok műveletek belül maguk is tartalmazhat. A gyűjteményen belül hivatkozás műveletek közvetlenül kívül hello gyűjtemény lehet hivatkozni. Ha az Ön által definiált `http` hatókörben, `@body('http')` a munkafolyamaton belül bárhol továbbra is érvényes. A gyűjteményen belül műveletek is `runAfter` belül csak más műveletek hello ugyanahhoz a gyűjteményhez.

## <a name="scope-action"></a>Hatókör művelet

Hello `scope` művelet lehetővé teszi, hogy logikailag a munkafolyamat műveleteit.

|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|műveletek|Igen|Objektum|Belső műveletek tooexecute hello hatókörén belül|

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

Ez a ismétlési művelet tömb telepítéseket, és minden elemhez belső műveleteket hajtja végre. Alapértelmezés szerint a hello foreach hurok párhuzamos (20 végrehajtások párhuzamosan egyszerre) hajtja végre. Végrehajtási szabályainak használatával hello beállítása `operationOptions` paraméter.

|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|műveletek|Igen|Objektum|Belső műveletek tooexecute hello hurkon belül|
|foreach|Igen|Karakterlánc|hello tömb tooiterate keresztül|
|operationOptions|nem|Karakterlánc|Viselkedés művelet lehetőségeket. Jelenleg csak támogatja `sequential` tooexecute ismétlési egymás után (az alapértelmezés lesz párhuzamos)|

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

Ismétlési művelet belső műveleteket hajt végre, amíg feltétel eredménye tootrue.

|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|műveletek|Igen|Objektum|Belső műveletek tooexecute hello hurkon belül|
|kifejezés|Igen|Karakterlánc|Mindegyik iteráció után hello kifejezés tooevaluate|
|Korlát|igen|Objektum|hello korlátok hello hurok - legalább egy korlátot kell megadni.|
|Száma|nem|int|hello korlát toohello végrehajtható ismétlések száma|
|timeout|nem|Karakterlánc|mennyi ideig kell a hurok hello időkorlátját.  ISO 8601 formátumban|


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

Hello `If` művelet lehetővé teszi, hogy olyan feltétel értékelése, és hogy hello kifejezés túl értéke alapján ág végrehajtható`true`.

|Név|Szükséges|Típus|Leírás|  
|--------|------------|--------|---------------|  
|műveletek|Igen|Objektum|Ha a kifejezés eredménye túl belső műveletek tooexecute`true`|
|kifejezés|Igen|Karakterlánc|hello kifejezés tooevaluate|
|más|nem|Objektum|Ha a kifejezés eredménye túl belső műveletek tooexecute`false`|
  
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
  
hello következő táblázat példákat mutat a feltételek használatát kifejezések egy műveletben:  
  
|JSON-érték|eredménye|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|Bármely érték, amely tootrue értékelné ki, ez az állapot toopass okoz. Csak a logikai kifejezések használhatók. más tooconvert típusokat tooBoolean, függvények használata `empty`, `equals`.|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|Összehasonlító függvény támogatott. Például hello itt hello művelet csak akkor megy végbe hello küszöbértéknél nagyobb act1 hello kimenete esetén.|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|Támogatott toocreate beágyazott logikai kifejezésen logika funkciók is. Ebben az esetben hello művelet hajtja végre, amikor hello act1 eredménye hello küszöbérték fölött vagy alatt 100.|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|Tömb funkciók toocheck is használja, ha egy tömb van elemeket. Ebben az esetben hello műveletet hajt végre, ha hello hibák tömb üres.| 
|`"expression": "parameters('hasSpecialAction')"`|Hiba – nem egy érvényes, mert @ van szükség a feltétel feltételeket.|  
  
Ha a feltétel sikeresen, hello feltétel jelölése `Succeeded`. Műveletek belül vagy hello `actions` vagy `else` objektumok kiértékelése túl`Succeeded` hajtotta végre, és sikeresen befejeződött, `Failed` hajtotta végre, és nem sikerült, vagy `Skipped` mikor nem hajtotta végre a fiókirodában.

## <a name="next-steps"></a>Következő lépések

[A munkafolyamat szolgáltatás REST API](https://docs.microsoft.com/rest/api/logic/workflows)
