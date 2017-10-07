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
# <a name="handle-content-types-in-logic-apps"></a><span data-ttu-id="45f61-103">A logic apps leíró tartalomtípusok</span><span class="sxs-lookup"><span data-stu-id="45f61-103">Handle content types in logic apps</span></span>

<span data-ttu-id="45f61-104">Számos különböző típusú tartalmat is áramlása logikai alkalmazás, beleértve a JSON-NÁ, XML, egyszerű fájlok és a bináris adatok.</span><span class="sxs-lookup"><span data-stu-id="45f61-104">Many different types of content can flow through a logic app, including JSON, XML, flat files, and binary data.</span></span> <span data-ttu-id="45f61-105">Amíg hello Logic Apps motor támogatja az összes, néhány rendszer natív módon azt értelmezni tudja hello Logic Apps motor.</span><span class="sxs-lookup"><span data-stu-id="45f61-105">While hello Logic Apps Engine supports all content types, some are natively understood by hello Logic Apps Engine.</span></span> <span data-ttu-id="45f61-106">Előfordulhat, hogy másokat, konvertálási vagy átalakítás esetén szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="45f61-106">Others might require casting or conversions as necessary.</span></span> <span data-ttu-id="45f61-107">Ez a cikk ismerteti, hogyan kezeli a hello motor a különböző típusú tartalmakat és hogyan toocorrectly kezelik ezeket a típusokat, amikor erre szükség van.</span><span class="sxs-lookup"><span data-stu-id="45f61-107">This article describes how hello engine handles different content types and how toocorrectly handle these types when necessary.</span></span>

## <a name="content-type-header"></a><span data-ttu-id="45f61-108">Content-Type fejléc</span><span class="sxs-lookup"><span data-stu-id="45f61-108">Content-Type Header</span></span>

<span data-ttu-id="45f61-109">toostart alapvetően, nézzük hello két `Content-Types` , amelyek nem igényelnek konverzió vagy a logikai alkalmazás használható adattípusokról: `application/json` és `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="45f61-109">toostart basically, let's look at hello two `Content-Types` that don't require conversion or casting that you can use in a logic app: `application/json` and `text/plain`.</span></span>

## <a name="applicationjson"></a><span data-ttu-id="45f61-110">Az Application/JSON</span><span class="sxs-lookup"><span data-stu-id="45f61-110">Application/JSON</span></span>

<span data-ttu-id="45f61-111">munkafolyamat-motor hello támaszkodik hello `Content-Type` HTTP fejléc meghívja toodetermine hello megfelelő kezelését.</span><span class="sxs-lookup"><span data-stu-id="45f61-111">hello workflow engine relies on hello `Content-Type` header from HTTP calls toodetermine hello appropriate handling.</span></span> <span data-ttu-id="45f61-112">Hello tartalomtípussal kérésének `application/json` tárolja, és JSON-objektumként kezeli.</span><span class="sxs-lookup"><span data-stu-id="45f61-112">Any request with hello content type `application/json` is stored and handled as a JSON Object.</span></span> <span data-ttu-id="45f61-113">Alapértelmezés szerint akkor értelmezhető JSON-tartalmak anélkül, hogy bármely adattípusokról.</span><span class="sxs-lookup"><span data-stu-id="45f61-113">Also, JSON content can be parsed by default without needing any casting.</span></span> 

<span data-ttu-id="45f61-114">Például tudta elemezni a kérelmeket, amelyek hello content-type fejléc `application/json ` egy munkafolyamatban például a kifejezés használatával `@body('myAction')['foo'][0]` tooget hello érték `bar` ebben az esetben:</span><span class="sxs-lookup"><span data-stu-id="45f61-114">For example, you could parse a request that has hello content type header `application/json ` in a workflow by using an expression like `@body('myAction')['foo'][0]` tooget hello value `bar` in this case:</span></span>

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

<span data-ttu-id="45f61-115">Nincsenek további adattípusokról van szükség.</span><span class="sxs-lookup"><span data-stu-id="45f61-115">No additional casting is needed.</span></span> <span data-ttu-id="45f61-116">Ha JSON, de nem volt a megadott fejléc adatokkal dolgozik, akkor is manuálisan típussá hello segítségével tooJSON `@json()` működnek, például: `@json(triggerBody())['foo']`.</span><span class="sxs-lookup"><span data-stu-id="45f61-116">If you are working with data that is JSON but didn't have a header specified, you can manually cast it tooJSON using hello `@json()` function, for example: `@json(triggerBody())['foo']`.</span></span>

### <a name="schema-and-schema-generator"></a><span data-ttu-id="45f61-117">Séma és a séma-készítő</span><span class="sxs-lookup"><span data-stu-id="45f61-117">Schema and schema generator</span></span>

<span data-ttu-id="45f61-118">hello kérelem eseményindító lehetővé teszi egy JSON-séma tooenter hello tartalom tooreceive várt.</span><span class="sxs-lookup"><span data-stu-id="45f61-118">hello Request trigger lets you tooenter a JSON schema for hello payload you expect tooreceive.</span></span> <span data-ttu-id="45f61-119">A séma lehetővé teszi, hogy a rendszer jogkivonatokat hoz létre, így is hello tartalomfelhasználási hello kérelem hello Tervező.</span><span class="sxs-lookup"><span data-stu-id="45f61-119">This schema lets hello designer generate tokens so you can consume hello content of hello request.</span></span> <span data-ttu-id="45f61-120">Ha készen áll a séma nem rendelkezik, válassza ki a **használata minta hasznos toogenerate séma**, így a JSON-séma generálása a minta hasznos adatok között.</span><span class="sxs-lookup"><span data-stu-id="45f61-120">If you don't have a schema ready, select **Use sample payload toogenerate schema**, so you can generate a JSON schema from a sample payload.</span></span>

![Séma](./media/logic-apps-http-endpoint/manualtrigger.png)

### <a name="parse-json-action"></a><span data-ttu-id="45f61-122">A művelet "Elemzése JSON"</span><span class="sxs-lookup"><span data-stu-id="45f61-122">'Parse JSON' action</span></span>

<span data-ttu-id="45f61-123">Hello `Parse JSON` művelet lehetővé teszi a JSON-tartalom elemzése a logic app felhasználásához rövid jogkivonatokba.</span><span class="sxs-lookup"><span data-stu-id="45f61-123">hello `Parse JSON` action lets you parse JSON content into friendly tokens for logic app consumption.</span></span> <span data-ttu-id="45f61-124">Hasonló toohello kérelem eseményindító, ez a művelet lehetővé teszi adja meg vagy egy JSON-séma, a tartalom kívánt tooparse hello készítése.</span><span class="sxs-lookup"><span data-stu-id="45f61-124">Similar toohello Request trigger, this action lets you enter or generate a JSON schema for hello content you want tooparse.</span></span> <span data-ttu-id="45f61-125">Ez az eszköz fogyasztó adatokat a Service Bus-, Azure Cosmos DB, és így tovább, sokkal egyszerűbbé teszi.</span><span class="sxs-lookup"><span data-stu-id="45f61-125">This tool makes consuming data from Service Bus, Azure Cosmos DB, and so on, much easier.</span></span>

![JSON elemzése](./media/logic-apps-content-type/ParseJSON.png)

## <a name="textplain"></a><span data-ttu-id="45f61-127">egyszerű szöveg</span><span class="sxs-lookup"><span data-stu-id="45f61-127">Text/plain</span></span>

<span data-ttu-id="45f61-128">Hasonló túl`application/json`, HTTP-üzenetek hello érkezett `Content-Type` fejlécének `text/plain` nyers formátumban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="45f61-128">Similar too`application/json`, HTTP messages received with hello `Content-Type` header of `text/plain` are stored in raw form.</span></span> <span data-ttu-id="45f61-129">Is, ha azokat az üzeneteket a későbbi műveletek nélkül adattípusokról szerepelnek, ezeket a kérelmeket, lépjen a `Content-Type`: `text/plain` fejléc.</span><span class="sxs-lookup"><span data-stu-id="45f61-129">Also, if those messages are included in subsequent actions without casting, those requests go out with  `Content-Type`: `text/plain` header.</span></span> <span data-ttu-id="45f61-130">Például az egyszerű használatakor kaphat a HTTP tartalom másként `text/plain`:</span><span class="sxs-lookup"><span data-stu-id="45f61-130">For example, when working with a flat file, you might get this HTTP content as `text/plain`:</span></span>

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

<span data-ttu-id="45f61-131">Ha a következő művelet hello meg hello kérelem küldése egy másik kérelem törzse hello (`@body('flatfile')`), hello kérelmet egy `text/plain` Content-Type fejléc.</span><span class="sxs-lookup"><span data-stu-id="45f61-131">If in hello next action, you send hello request as hello body of another request (`@body('flatfile')`), hello request would have a `text/plain` Content-Type header.</span></span> <span data-ttu-id="45f61-132">Egyszerű szöveges, de nem volt a megadott fejléc adatokkal dolgozik, ha manuálisan is leadott hello adatok tootext hello használata `@string()` működnek, például: `@string(triggerBody())`.</span><span class="sxs-lookup"><span data-stu-id="45f61-132">If you are working with data that is plain text but didn't have a header specified, you can manually cast hello data tootext using hello `@string()` function, for example: `@string(triggerBody())`.</span></span>

## <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a><span data-ttu-id="45f61-133">Application/xml és Application/octet-stream és konverter funkciók</span><span class="sxs-lookup"><span data-stu-id="45f61-133">Application/xml and Application/octet-stream and converter functions</span></span>

<span data-ttu-id="45f61-134">Logic Apps motor hello mindig megőrzi hello `Content-Type` , amely hello HTTP-kérés vagy válasz érkezett.</span><span class="sxs-lookup"><span data-stu-id="45f61-134">hello Logic Apps Engine always preserves hello `Content-Type` that was received on hello HTTP request or response.</span></span> <span data-ttu-id="45f61-135">Tehát ha hello motor hello a tartalmat megkap `Content-Type` a `application/octet-stream`, és megadja, hogy tartalom nélkül adattípusokról későbbi művelettel, hello kimenő kérelem rendelkezik `Content-Type`: `application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="45f61-135">So if hello engine receives content with hello `Content-Type` of `application/octet-stream`, and you include that content in a subsequent action without casting, hello outgoing request has `Content-Type`: `application/octet-stream`.</span></span> <span data-ttu-id="45f61-136">Ezzel a módszerrel hello motor is garantálható, hogy nem vesznek el adatok hello munkafolyamaton keresztül áthelyezés közben.</span><span class="sxs-lookup"><span data-stu-id="45f61-136">This way, hello engine can guarantee data isn't lost while moving through hello workflow.</span></span> <span data-ttu-id="45f61-137">Azonban hello műveletállapot (bemenetekhez és kimenetekhez) van tárolva a JSON-objektum hello hello munkafolyamat állapota mozog.</span><span class="sxs-lookup"><span data-stu-id="45f61-137">However, hello action state (inputs and outputs) is stored in a JSON object as hello state moves through hello workflow.</span></span> <span data-ttu-id="45f61-138">Ezért egyes adattípusok, hello motor alakít toopreserve hello tartalom tooa bináris base64 kódolású karakterlánc, amely megőrzi az mindkét megfelelő metaadatok `$content` és `$content-type`, amelyen a program automatikusan alakítható.</span><span class="sxs-lookup"><span data-stu-id="45f61-138">So toopreserve some data types, hello engine converts hello content tooa binary base64 encoded string with appropriate metadata that preserves both `$content` and `$content-type`, which are automatically be converted.</span></span> 

* <span data-ttu-id="45f61-139">`@json()`-adatok túl kerül`application/json`</span><span class="sxs-lookup"><span data-stu-id="45f61-139">`@json()` - casts data too`application/json`</span></span>
* <span data-ttu-id="45f61-140">`@xml()`-adatok túl kerül`application/xml`</span><span class="sxs-lookup"><span data-stu-id="45f61-140">`@xml()` - casts data too`application/xml`</span></span>
* <span data-ttu-id="45f61-141">`@binary()`-adatok túl kerül`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="45f61-141">`@binary()` - casts data too`application/octet-stream`</span></span>
* <span data-ttu-id="45f61-142">`@string()`-adatok túl kerül`text/plain`</span><span class="sxs-lookup"><span data-stu-id="45f61-142">`@string()` - casts data too`text/plain`</span></span>
* <span data-ttu-id="45f61-143">`@base64()`-tartalom tooa Base64 kódolású karakterlánc konvertálása</span><span class="sxs-lookup"><span data-stu-id="45f61-143">`@base64()` - converts content tooa base64 string</span></span>
* <span data-ttu-id="45f61-144">`@base64toString()`-alakítja át a base64 kódolású karakterlánc túl`text/plain`</span><span class="sxs-lookup"><span data-stu-id="45f61-144">`@base64toString()` - converts a base64 encoded string too`text/plain`</span></span>
* <span data-ttu-id="45f61-145">`@base64toBinary()`-alakítja át a base64 kódolású karakterlánc túl`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="45f61-145">`@base64toBinary()` - converts a base64 encoded string too`application/octet-stream`</span></span>
* <span data-ttu-id="45f61-146">`@encodeDataUri()`-dataUri bájt tömbként karakterlánc kódolja</span><span class="sxs-lookup"><span data-stu-id="45f61-146">`@encodeDataUri()` - encodes a string as a dataUri byte array</span></span>
* <span data-ttu-id="45f61-147">`@decodeDataUri()`-a dataUri visszafejti azokat egy bájttömbben.</span><span class="sxs-lookup"><span data-stu-id="45f61-147">`@decodeDataUri()` - decodes a dataUri into a byte array</span></span>

<span data-ttu-id="45f61-148">Például, ha HTTP kérést fogadott `Content-Type`: `application/xml`:</span><span class="sxs-lookup"><span data-stu-id="45f61-148">For example, if you received an HTTP request with `Content-Type`: `application/xml`:</span></span>

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

<span data-ttu-id="45f61-149">Nem sikerült konvertálni és későbbi használatra a következőhöz hasonlóan `@xml(triggerBody())`, vagy hasonló függvények `@xpath(xml(triggerBody()), '/CustomerName')`.</span><span class="sxs-lookup"><span data-stu-id="45f61-149">You could cast and use later with something like `@xml(triggerBody())`, or in a function like `@xpath(xml(triggerBody()), '/CustomerName')`.</span></span>

## <a name="other-content-types"></a><span data-ttu-id="45f61-150">Egyéb tartalomtípusok</span><span class="sxs-lookup"><span data-stu-id="45f61-150">Other content types</span></span>

<span data-ttu-id="45f61-151">Egyéb tartalomtípusok támogatott és a logic apps működni, de előfordulhat, hogy manuálisan lekérése során hello üzenettörzs által hello dekódolás `$content`.</span><span class="sxs-lookup"><span data-stu-id="45f61-151">Other content types are supported and work with logic apps, but might require manually retrieving hello message body by decoding hello `$content`.</span></span> <span data-ttu-id="45f61-152">Tegyük fel például, hogy indít el egy `application/x-www-url-formencoded` where kérelem `$content` minden adatot a Base64 kódolású karakterlánc toopreserve kódolva hello payload:</span><span class="sxs-lookup"><span data-stu-id="45f61-152">For example, suppose you trigger an `application/x-www-url-formencoded` request where `$content` is hello payload encoded as a base64 string toopreserve all data:</span></span>

```
CustomerName=Frank&Address=123+Avenue
```

<span data-ttu-id="45f61-153">Hello kérelem nem egyszerű szöveges vagy JSON-NÁ, mert hello kérelem tárolódik hello műveletet az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="45f61-153">Because hello request isn't plain text or JSON, hello request is stored in hello action as follows:</span></span>

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Jelenleg nem áll rendelkezésre egy natív űrlapadat függvényt, így továbbra is használhatja ezeket az adatokat a munkafolyamat manuális elérésével funkcióval rendelkező hello adatok, például `@string(body('formdataAction'))`. Ha tooalso rendelkezik hello kimenő kérelem hello `application/x-www-url-formencoded` content-type fejléc, hozzáadhatja hello toohello művelet kérelemtörzset nélkül bármely adattípusokról például `@body('formdataAction')`. Azonban ez a módszer csak működik, ha hello törzse egyetlen paraméterének hello hello `body` bemeneti. <span data-ttu-id="45f61-157">Ha megpróbál toouse `@body('formdataAction')` a egy `application/json` kérte, futásidejű hiba beolvasni, mert hello kódolt body zajlik.</span><span class="sxs-lookup"><span data-stu-id="45f61-157">If you try toouse `@body('formdataAction')` in an `application/json` request, you get a runtime error because hello encoded body is sent.</span></span>

