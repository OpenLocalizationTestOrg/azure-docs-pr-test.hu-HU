---
title: "Tartalomtípus - Azure Logic Apps alkalmazásokat kezeléséhez |} Microsoft Docs"
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
ms.openlocfilehash: ac67838344bbd10384299c086ff096fbe5dec6a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="handle-content-types-in-logic-apps"></a><span data-ttu-id="55d75-103">A logic apps leíró tartalomtípusok</span><span class="sxs-lookup"><span data-stu-id="55d75-103">Handle content types in logic apps</span></span>

<span data-ttu-id="55d75-104">Számos különböző típusú tartalmat is áramlása logikai alkalmazás, beleértve a JSON-NÁ, XML, egyszerű fájlok és a bináris adatok.</span><span class="sxs-lookup"><span data-stu-id="55d75-104">Many different types of content can flow through a logic app, including JSON, XML, flat files, and binary data.</span></span> <span data-ttu-id="55d75-105">Míg a Logic Apps-motor minden tartalomtípusok támogatja, néhány vannak natív módon képes volt értelmezni a Logic Apps motor.</span><span class="sxs-lookup"><span data-stu-id="55d75-105">While the Logic Apps Engine supports all content types, some are natively understood by the Logic Apps Engine.</span></span> <span data-ttu-id="55d75-106">Előfordulhat, hogy másokat, konvertálási vagy átalakítás esetén szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="55d75-106">Others might require casting or conversions as necessary.</span></span> <span data-ttu-id="55d75-107">Ez a cikk ismerteti, hogyan kezeli a motor a különböző típusú tartalmakat és annak megfelelően kezeli az ilyen jellegű, amikor erre szükség van.</span><span class="sxs-lookup"><span data-stu-id="55d75-107">This article describes how the engine handles different content types and how to correctly handle these types when necessary.</span></span>

## <a name="content-type-header"></a><span data-ttu-id="55d75-108">Content-Type fejléc</span><span class="sxs-lookup"><span data-stu-id="55d75-108">Content-Type Header</span></span>

<span data-ttu-id="55d75-109">Alapvetően elindításához vizsgáljuk meg a két `Content-Types` , amelyek nem igényelnek konverzió vagy a logikai alkalmazás használható adattípusokról: `application/json` és `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="55d75-109">To start basically, let's look at the two `Content-Types` that don't require conversion or casting that you can use in a logic app: `application/json` and `text/plain`.</span></span>

## <a name="applicationjson"></a><span data-ttu-id="55d75-110">Az Application/JSON</span><span class="sxs-lookup"><span data-stu-id="55d75-110">Application/JSON</span></span>

<span data-ttu-id="55d75-111">A munkafolyamat-motor támaszkodik a `Content-Type` HTTP fejléc meghívja a megfelelő kezelési meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="55d75-111">The workflow engine relies on the `Content-Type` header from HTTP calls to determine the appropriate handling.</span></span> <span data-ttu-id="55d75-112">A kérelem tartalomtípus `application/json` tárolja, és JSON-objektumként kezeli.</span><span class="sxs-lookup"><span data-stu-id="55d75-112">Any request with the content type `application/json` is stored and handled as a JSON Object.</span></span> <span data-ttu-id="55d75-113">Alapértelmezés szerint akkor értelmezhető JSON-tartalmak anélkül, hogy bármely adattípusokról.</span><span class="sxs-lookup"><span data-stu-id="55d75-113">Also, JSON content can be parsed by default without needing any casting.</span></span> 

<span data-ttu-id="55d75-114">Például tudta elemezni a kérelmeket, amelyek a content-type fejléc `application/json ` egy munkafolyamatban például a kifejezés használatával `@body('myAction')['foo'][0]` a érték `bar` ebben az esetben:</span><span class="sxs-lookup"><span data-stu-id="55d75-114">For example, you could parse a request that has the content type header `application/json ` in a workflow by using an expression like `@body('myAction')['foo'][0]` to get the value `bar` in this case:</span></span>

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

<span data-ttu-id="55d75-115">Nincsenek további adattípusokról van szükség.</span><span class="sxs-lookup"><span data-stu-id="55d75-115">No additional casting is needed.</span></span> <span data-ttu-id="55d75-116">Ha JSON, de nem volt a megadott fejléc adatokkal dolgozik, akkor is manuális típussá JSON használata a `@json()` működnek, például: `@json(triggerBody())['foo']`.</span><span class="sxs-lookup"><span data-stu-id="55d75-116">If you are working with data that is JSON but didn't have a header specified, you can manually cast it to JSON using the `@json()` function, for example: `@json(triggerBody())['foo']`.</span></span>

### <a name="schema-and-schema-generator"></a><span data-ttu-id="55d75-117">Séma és a séma-készítő</span><span class="sxs-lookup"><span data-stu-id="55d75-117">Schema and schema generator</span></span>

<span data-ttu-id="55d75-118">A kérelem eseményindító lehetővé teszi, hogy meg kell adnia egy JSON-séma az fogadni várt tartalom.</span><span class="sxs-lookup"><span data-stu-id="55d75-118">The Request trigger lets you to enter a JSON schema for the payload you expect to receive.</span></span> <span data-ttu-id="55d75-119">A séma lehetővé teszi, hogy a Tervező generate jogkivonatokat, a kérelem tartalmának felhasználhat.</span><span class="sxs-lookup"><span data-stu-id="55d75-119">This schema lets the designer generate tokens so you can consume the content of the request.</span></span> <span data-ttu-id="55d75-120">Ha készen áll a séma nem rendelkezik, válassza ki a **séma létrehozásához használja a minta hasznos**, így a JSON-séma generálása a minta hasznos adatok között.</span><span class="sxs-lookup"><span data-stu-id="55d75-120">If you don't have a schema ready, select **Use sample payload to generate schema**, so you can generate a JSON schema from a sample payload.</span></span>

![Séma](./media/logic-apps-http-endpoint/manualtrigger.png)

### <a name="parse-json-action"></a><span data-ttu-id="55d75-122">A művelet "Elemzése JSON"</span><span class="sxs-lookup"><span data-stu-id="55d75-122">'Parse JSON' action</span></span>

<span data-ttu-id="55d75-123">A `Parse JSON` művelet lehetővé teszi a JSON-tartalom elemzése a logic app felhasználásához rövid jogkivonatokba.</span><span class="sxs-lookup"><span data-stu-id="55d75-123">The `Parse JSON` action lets you parse JSON content into friendly tokens for logic app consumption.</span></span> <span data-ttu-id="55d75-124">A kérelem eseményindító hasonló, ez a művelet lehetővé teszi adja meg vagy hozható létre a JSON-séma az elemezni kívánt tartalom.</span><span class="sxs-lookup"><span data-stu-id="55d75-124">Similar to the Request trigger, this action lets you enter or generate a JSON schema for the content you want to parse.</span></span> <span data-ttu-id="55d75-125">Ez az eszköz fogyasztó adatokat a Service Bus-, Azure Cosmos DB, és így tovább, sokkal egyszerűbbé teszi.</span><span class="sxs-lookup"><span data-stu-id="55d75-125">This tool makes consuming data from Service Bus, Azure Cosmos DB, and so on, much easier.</span></span>

![JSON elemzése](./media/logic-apps-content-type/ParseJSON.png)

## <a name="textplain"></a><span data-ttu-id="55d75-127">egyszerű szöveg</span><span class="sxs-lookup"><span data-stu-id="55d75-127">Text/plain</span></span>

<span data-ttu-id="55d75-128">Hasonló `application/json`, érkezett a HTTP-üzenetek a `Content-Type` fejlécének `text/plain` nyers formátumban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="55d75-128">Similar to `application/json`, HTTP messages received with the `Content-Type` header of `text/plain` are stored in raw form.</span></span> <span data-ttu-id="55d75-129">Is, ha azokat az üzeneteket a későbbi műveletek nélkül adattípusokról szerepelnek, ezeket a kérelmeket, lépjen a `Content-Type`: `text/plain` fejléc.</span><span class="sxs-lookup"><span data-stu-id="55d75-129">Also, if those messages are included in subsequent actions without casting, those requests go out with  `Content-Type`: `text/plain` header.</span></span> <span data-ttu-id="55d75-130">Például az egyszerű használatakor kaphat a HTTP tartalom másként `text/plain`:</span><span class="sxs-lookup"><span data-stu-id="55d75-130">For example, when working with a flat file, you might get this HTTP content as `text/plain`:</span></span>

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

<span data-ttu-id="55d75-131">Ha a következő művelet küldi el a kérelmet egy másik kérelem törzsében (`@body('flatfile')`), a kérelmet egy `text/plain` Content-Type fejléc.</span><span class="sxs-lookup"><span data-stu-id="55d75-131">If in the next action, you send the request as the body of another request (`@body('flatfile')`), the request would have a `text/plain` Content-Type header.</span></span> <span data-ttu-id="55d75-132">Egyszerű szöveges, de nem volt a megadott fejléc adatokkal dolgozik, ha manuálisan is konvertálni szöveg használatával az adatok a `@string()` működnek, például: `@string(triggerBody())`.</span><span class="sxs-lookup"><span data-stu-id="55d75-132">If you are working with data that is plain text but didn't have a header specified, you can manually cast the data to text using the `@string()` function, for example: `@string(triggerBody())`.</span></span>

## <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a><span data-ttu-id="55d75-133">Application/xml és Application/octet-stream és konverter funkciók</span><span class="sxs-lookup"><span data-stu-id="55d75-133">Application/xml and Application/octet-stream and converter functions</span></span>

<span data-ttu-id="55d75-134">A Logic Apps motor mindig megőrzi a `Content-Type` , amelyek a HTTP-kérés vagy válasz érkezett.</span><span class="sxs-lookup"><span data-stu-id="55d75-134">The Logic Apps Engine always preserves the `Content-Type` that was received on the HTTP request or response.</span></span> <span data-ttu-id="55d75-135">Igen, ha a motor megkapja a tartalmat a `Content-Type` a `application/octet-stream`, és megadja, hogy tartalom nélkül adattípusokról későbbi művelettel, a kimenő kérelem rendelkezik `Content-Type`: `application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="55d75-135">So if the engine receives content with the `Content-Type` of `application/octet-stream`, and you include that content in a subsequent action without casting, the outgoing request has `Content-Type`: `application/octet-stream`.</span></span> <span data-ttu-id="55d75-136">Ezzel a módszerrel a motor is garantálja az adatok nem vesznek el a munkafolyamaton keresztül áthelyezés közben.</span><span class="sxs-lookup"><span data-stu-id="55d75-136">This way, the engine can guarantee data isn't lost while moving through the workflow.</span></span> <span data-ttu-id="55d75-137">Azonban a műveletállapot (bemenetekhez és kimenetekhez) van tárolva egy JSON-objektum állapotát a munkafolyamaton keresztül helyezi át.</span><span class="sxs-lookup"><span data-stu-id="55d75-137">However, the action state (inputs and outputs) is stored in a JSON object as the state moves through the workflow.</span></span> <span data-ttu-id="55d75-138">Így egyes adattípusok megőrzéséhez a motor konvertálja a tartalmat egy bináris a base64 kódolású karakterlánc, amely megőrzi az mindkét megfelelő metaadatok `$content` és `$content-type`, amelyeket automatikusan alakítható.</span><span class="sxs-lookup"><span data-stu-id="55d75-138">So to preserve some data types, the engine converts the content to a binary base64 encoded string with appropriate metadata that preserves both `$content` and `$content-type`, which are automatically be converted.</span></span> 

* <span data-ttu-id="55d75-139">`@json()`-adatok kerül`application/json`</span><span class="sxs-lookup"><span data-stu-id="55d75-139">`@json()` - casts data to `application/json`</span></span>
* <span data-ttu-id="55d75-140">`@xml()`-adatok kerül`application/xml`</span><span class="sxs-lookup"><span data-stu-id="55d75-140">`@xml()` - casts data to `application/xml`</span></span>
* <span data-ttu-id="55d75-141">`@binary()`-adatok kerül`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="55d75-141">`@binary()` - casts data to `application/octet-stream`</span></span>
* <span data-ttu-id="55d75-142">`@string()`-adatok kerül`text/plain`</span><span class="sxs-lookup"><span data-stu-id="55d75-142">`@string()` - casts data to `text/plain`</span></span>
* <span data-ttu-id="55d75-143">`@base64()`-tartalom konvertálja a Base64 kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="55d75-143">`@base64()` - converts content to a base64 string</span></span>
* <span data-ttu-id="55d75-144">`@base64toString()`-a base64 kódolású karakterlánc konvertálása`text/plain`</span><span class="sxs-lookup"><span data-stu-id="55d75-144">`@base64toString()` - converts a base64 encoded string to `text/plain`</span></span>
* <span data-ttu-id="55d75-145">`@base64toBinary()`-a base64 kódolású karakterlánc konvertálása`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="55d75-145">`@base64toBinary()` - converts a base64 encoded string to `application/octet-stream`</span></span>
* <span data-ttu-id="55d75-146">`@encodeDataUri()`-dataUri bájt tömbként karakterlánc kódolja</span><span class="sxs-lookup"><span data-stu-id="55d75-146">`@encodeDataUri()` - encodes a string as a dataUri byte array</span></span>
* <span data-ttu-id="55d75-147">`@decodeDataUri()`-a dataUri visszafejti azokat egy bájttömbben.</span><span class="sxs-lookup"><span data-stu-id="55d75-147">`@decodeDataUri()` - decodes a dataUri into a byte array</span></span>

<span data-ttu-id="55d75-148">Például, ha HTTP kérést fogadott `Content-Type`: `application/xml`:</span><span class="sxs-lookup"><span data-stu-id="55d75-148">For example, if you received an HTTP request with `Content-Type`: `application/xml`:</span></span>

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

<span data-ttu-id="55d75-149">Nem sikerült konvertálni és későbbi használatra a következőhöz hasonlóan `@xml(triggerBody())`, vagy hasonló függvények `@xpath(xml(triggerBody()), '/CustomerName')`.</span><span class="sxs-lookup"><span data-stu-id="55d75-149">You could cast and use later with something like `@xml(triggerBody())`, or in a function like `@xpath(xml(triggerBody()), '/CustomerName')`.</span></span>

## <a name="other-content-types"></a><span data-ttu-id="55d75-150">Egyéb tartalomtípusok</span><span class="sxs-lookup"><span data-stu-id="55d75-150">Other content types</span></span>

<span data-ttu-id="55d75-151">Egyéb tartalomtípusok támogatott és a logic apps működni, de előfordulhat, hogy manuálisan beolvasása az üzenettörzs dekódolás által a `$content`.</span><span class="sxs-lookup"><span data-stu-id="55d75-151">Other content types are supported and work with logic apps, but might require manually retrieving the message body by decoding the `$content`.</span></span> <span data-ttu-id="55d75-152">Tegyük fel például, hogy indít el egy `application/x-www-url-formencoded` where kérelem `$content` kódolt Base64 kódolású karakterlánc összes adatok megőrzése érdekében a tartalom:</span><span class="sxs-lookup"><span data-stu-id="55d75-152">For example, suppose you trigger an `application/x-www-url-formencoded` request where `$content` is the payload encoded as a base64 string to preserve all data:</span></span>

```
CustomerName=Frank&Address=123+Avenue
```

<span data-ttu-id="55d75-153">A kérelem nem egyszerű szöveges vagy JSON-NÁ, mert a kérelem tárolódik a műveletet az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="55d75-153">Because the request isn't plain text or JSON, the request is stored in the action as follows:</span></span>

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Jelenleg nem áll rendelkezésre az űrlap adatait egy natív függvény, így továbbra is használhatja ezeket az adatokat a munkafolyamat manuális elérésével, például az adatokat egy olyan függvényt a `@string(body('formdataAction'))`. Ha a kimenő kérelem rendelkeznie kell a `application/x-www-url-formencoded` content-type fejléc, hozzáadhatja a kérelem a műveleti szövegtörzs nélkül bármely adattípusokról például `@body('formdataAction')`. Azonban ezt a módszert csak akkor működik, ha a szervezet az az egyetlen paraméter a `body` bemeneti. <span data-ttu-id="55d75-157">Ha próbálja használni a `@body('formdataAction')` a egy `application/json` kérte, futásidejű hiba beolvasni, mert a kódolt body zajlik.</span><span class="sxs-lookup"><span data-stu-id="55d75-157">If you try to use `@body('formdataAction')` in an `application/json` request, you get a runtime error because the encoded body is sent.</span></span>

