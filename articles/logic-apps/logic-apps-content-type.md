---
title: "aaaHandle tartalomtípus - Azure Logic Apps |} Microsoft Docs"
description: "Hogy az Azure Logic Apps hogyan kezelje a tervezési és futásidejű tartalomtípusok"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: cd1f08fd-8cde-4afc-86ff-2e5738cc8288
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: a823249c5388b15ae0aae450b40499b420ea005e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="handle-content-types-in-logic-apps"></a>A logic apps leíró tartalomtípusok

Számos különböző típusú tartalmat is áramlása logikai alkalmazás, beleértve a JSON-NÁ, XML, egyszerű fájlok és a bináris adatok. Amíg hello Logic Apps motor támogatja az összes, néhány rendszer natív módon azt értelmezni tudja hello Logic Apps motor. Előfordulhat, hogy másokat, konvertálási vagy átalakítás esetén szükség szerint. Ez a cikk ismerteti, hogyan kezeli a hello motor a különböző típusú tartalmakat és hogyan toocorrectly kezelik ezeket a típusokat, amikor erre szükség van.

## <a name="content-type-header"></a>Content-Type fejléc

toostart alapvetően, nézzük hello két `Content-Types` , amelyek nem igényelnek konverzió vagy a logikai alkalmazás használható adattípusokról: `application/json` és `text/plain`.

## <a name="applicationjson"></a>Az Application/JSON

munkafolyamat-motor hello támaszkodik hello `Content-Type` HTTP fejléc meghívja toodetermine hello megfelelő kezelését. Hello tartalomtípussal kérésének `application/json` tárolja, és JSON-objektumként kezeli. Alapértelmezés szerint akkor értelmezhető JSON-tartalmak anélkül, hogy bármely adattípusokról. 

Például tudta elemezni a kérelmeket, amelyek hello content-type fejléc `application/json ` egy munkafolyamatban például a kifejezés használatával `@body('myAction')['foo'][0]` tooget hello érték `bar` ebben az esetben:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

Nincsenek további adattípusokról van szükség. Ha JSON, de nem volt a megadott fejléc adatokkal dolgozik, akkor is manuálisan típussá hello segítségével tooJSON `@json()` működnek, például: `@json(triggerBody())['foo']`.

### <a name="schema-and-schema-generator"></a>Séma és a séma-készítő

hello kérelem eseményindító lehetővé teszi egy JSON-séma tooenter hello tartalom tooreceive várt. A séma lehetővé teszi, hogy a rendszer jogkivonatokat hoz létre, így is hello tartalomfelhasználási hello kérelem hello Tervező. Ha készen áll a séma nem rendelkezik, válassza ki a **használata minta hasznos toogenerate séma**, így a JSON-séma generálása a minta hasznos adatok között.

![Séma](./media/logic-apps-http-endpoint/manualtrigger.png)

### <a name="parse-json-action"></a>A művelet "Elemzése JSON"

Hello `Parse JSON` művelet lehetővé teszi a JSON-tartalom elemzése a logic app felhasználásához rövid jogkivonatokba. Hasonló toohello kérelem eseményindító, ez a művelet lehetővé teszi adja meg vagy egy JSON-séma, a tartalom kívánt tooparse hello készítése. Ez az eszköz fogyasztó adatokat a Service Bus-, Azure Cosmos DB, és így tovább, sokkal egyszerűbbé teszi.

![JSON elemzése](./media/logic-apps-content-type/ParseJSON.png)

## <a name="textplain"></a>egyszerű szöveg

Hasonló túl`application/json`, HTTP-üzenetek hello érkezett `Content-Type` fejlécének `text/plain` nyers formátumban tárolódnak. Is, ha azokat az üzeneteket a későbbi műveletek nélkül adattípusokról szerepelnek, ezeket a kérelmeket, lépjen a `Content-Type`: `text/plain` fejléc. Például az egyszerű használatakor kaphat a HTTP tartalom másként `text/plain`:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

Ha a következő művelet hello meg hello kérelem küldése egy másik kérelem törzse hello (`@body('flatfile')`), hello kérelmet egy `text/plain` Content-Type fejléc. Egyszerű szöveges, de nem volt a megadott fejléc adatokkal dolgozik, ha manuálisan is leadott hello adatok tootext hello használata `@string()` működnek, például: `@string(triggerBody())`.

## <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Application/xml és Application/octet-stream és konverter funkciók

Logic Apps motor hello mindig megőrzi hello `Content-Type` , amely hello HTTP-kérés vagy válasz érkezett. Tehát ha hello motor hello a tartalmat megkap `Content-Type` a `application/octet-stream`, és megadja, hogy tartalom nélkül adattípusokról későbbi művelettel, hello kimenő kérelem rendelkezik `Content-Type`: `application/octet-stream`. Ezzel a módszerrel hello motor is garantálható, hogy nem vesznek el adatok hello munkafolyamaton keresztül áthelyezés közben. Azonban hello műveletállapot (bemenetekhez és kimenetekhez) van tárolva a JSON-objektum hello hello munkafolyamat állapota mozog. Ezért egyes adattípusok, hello motor alakít toopreserve hello tartalom tooa bináris base64 kódolású karakterlánc, amely megőrzi az mindkét megfelelő metaadatok `$content` és `$content-type`, amelyen a program automatikusan alakítható. 

* `@json()`-adatok túl kerül`application/json`
* `@xml()`-adatok túl kerül`application/xml`
* `@binary()`-adatok túl kerül`application/octet-stream`
* `@string()`-adatok túl kerül`text/plain`
* `@base64()`-tartalom tooa Base64 kódolású karakterlánc konvertálása
* `@base64toString()`-alakítja át a base64 kódolású karakterlánc túl`text/plain`
* `@base64toBinary()`-alakítja át a base64 kódolású karakterlánc túl`application/octet-stream`
* `@encodeDataUri()`-dataUri bájt tömbként karakterlánc kódolja
* `@decodeDataUri()`-a dataUri visszafejti azokat egy bájttömbben.

Például, ha HTTP kérést fogadott `Content-Type`: `application/xml`:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Nem sikerült konvertálni és későbbi használatra a következőhöz hasonlóan `@xml(triggerBody())`, vagy hasonló függvények `@xpath(xml(triggerBody()), '/CustomerName')`.

## <a name="other-content-types"></a>Egyéb tartalomtípusok

Egyéb tartalomtípusok támogatott és a logic apps működni, de előfordulhat, hogy manuálisan lekérése során hello üzenettörzs által hello dekódolás `$content`. Tegyük fel például, hogy indít el egy `application/x-www-url-formencoded` where kérelem `$content` minden adatot a Base64 kódolású karakterlánc toopreserve kódolva hello payload:

```
CustomerName=Frank&Address=123+Avenue
```

Hello kérelem nem egyszerű szöveges vagy JSON-NÁ, mert hello kérelem tárolódik hello műveletet az alábbiak szerint:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Jelenleg nem áll rendelkezésre egy natív űrlapadat függvényt, így továbbra is használhatja ezeket az adatokat a munkafolyamat manuális elérésével funkcióval rendelkező hello adatok, például `@string(body('formdataAction'))`. Ha tooalso rendelkezik hello kimenő kérelem hello `application/x-www-url-formencoded` content-type fejléc, hozzáadhatja hello toohello művelet kérelemtörzset nélkül bármely adattípusokról például `@body('formdataAction')`. Azonban ez a módszer csak működik, ha hello törzse egyetlen paraméterének hello hello `body` bemeneti. Ha megpróbál toouse `@body('formdataAction')` a egy `application/json` kérte, futásidejű hiba beolvasni, mert hello kódolt body zajlik.

