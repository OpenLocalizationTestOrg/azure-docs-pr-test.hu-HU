---
title: "az Azure továbbítási csomópont API-k hello aaaOverview |} Microsoft Docs"
description: "Továbbítják a csomópont API – áttekintés"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b7d6e822-7c32-4cb5-a4b8-df7d009bdc85
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: sethm
ms.openlocfilehash: d231acc854be0eaa965dec0229cf63b08ff27067
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="relay-hybrid-connections-node-api-overview"></a><span data-ttu-id="9ca60-103">Továbbítják a hibrid kapcsolatok csomópont API – áttekintés</span><span class="sxs-lookup"><span data-stu-id="9ca60-103">Relay Hybrid Connections Node API overview</span></span>

## <a name="overview"></a><span data-ttu-id="9ca60-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="9ca60-104">Overview</span></span>

<span data-ttu-id="9ca60-105">Hello [ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) csomópont csomag Azure Relay a hibrid kapcsolatok épül, és kiterjeszti a hello ["ws"](https://www.npmjs.com/package/ws) NPM csomag.</span><span class="sxs-lookup"><span data-stu-id="9ca60-105">hello [`hyco-ws`](https://www.npmjs.com/package/hyco-ws) Node package for Azure Relay Hybrid Connections is built on and extends hello ['ws'](https://www.npmjs.com/package/ws) NPM package.</span></span> <span data-ttu-id="9ca60-106">Ez a csomag újra exportálja az alapszintű csomag összes exportálását, és hozzáadja az új sablonként exportálja, amelyek lehetővé teszik a hello Azure továbbítási szolgáltatás hibrid kapcsolatok szolgáltatásával való integráció.</span><span class="sxs-lookup"><span data-stu-id="9ca60-106">This package re-exports all exports of that base package and adds new exports that enable integration with hello Azure Relay service Hybrid Connections feature.</span></span> 

<span data-ttu-id="9ca60-107">A meglévő alkalmazásokat, amelyek `require('ws')` használhatja ezt a csomagot a `require('hyco-ws')` helyette, amely is lehetővé teszi hibrid környezetekben, ahol az alkalmazás figyelheti a helyileg a WebSocket-kapcsolatok "belső hello tűzfal" és a hibrid kapcsolatokon keresztül minden a hello azonos idő.</span><span class="sxs-lookup"><span data-stu-id="9ca60-107">Existing applications that `require('ws')` can use this package with `require('hyco-ws')` instead, which also enables hybrid scenarios in which an application can listen for WebSocket connections locally from "inside hello firewall" and via Hybrid Connections, all at hello same time.</span></span>
  
## <a name="documentation"></a><span data-ttu-id="9ca60-108">Dokumentáció</span><span class="sxs-lookup"><span data-stu-id="9ca60-108">Documentation</span></span>

<span data-ttu-id="9ca60-109">hello API-jainak [részletes ismertetését lásd: hello fő 'ws"csomag](https://github.com/websockets/ws/blob/master/doc/ws.md).</span><span class="sxs-lookup"><span data-stu-id="9ca60-109">hello APIs are [documented in hello main 'ws' package](https://github.com/websockets/ws/blob/master/doc/ws.md).</span></span> <span data-ttu-id="9ca60-110">Ez a cikk ismerteti, hogyan eltér az ezt a csomagot, hogy az alaptervhez.</span><span class="sxs-lookup"><span data-stu-id="9ca60-110">This article describes how this package differs from that baseline.</span></span> 

<span data-ttu-id="9ca60-111">hello hello ALAPCSOMAG és a "hyco ws" közötti fontosabb különbségeket az új kiszolgáló osztályt, keresztül exportált ad `require('hyco-ws').RelayedServer`, és néhány segédmódszereket.</span><span class="sxs-lookup"><span data-stu-id="9ca60-111">hello key differences between hello base package and this 'hyco-ws' is that it adds a new server class, exported via `require('hyco-ws').RelayedServer`, and a few helper methods.</span></span>

### <a name="package-helper-methods"></a><span data-ttu-id="9ca60-112">Csomag segédmódszereket</span><span class="sxs-lookup"><span data-stu-id="9ca60-112">Package helper methods</span></span>

<span data-ttu-id="9ca60-113">Többféleképpen segédprogram érhető el a hello csomag exportálása, melyeket referenciaként használhat az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9ca60-113">There are several utility methods available on hello package export that you can reference as follows:</span></span>

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

<span data-ttu-id="9ca60-114">hello segédmódszereket használható ezzel a csomaggal, de a webszolgáltatás vagy az eszköz ügyfelek toocreate figyelői vagy feladók engedélyező egy csomópont kiszolgáló is használható.</span><span class="sxs-lookup"><span data-stu-id="9ca60-114">hello helper methods are for use with this package, but can also be used by a Node server for enabling web or device clients toocreate listeners or senders.</span></span> <span data-ttu-id="9ca60-115">hello-kiszolgáló úgy, hogy azok rövid élettartamú jogkivonatok beágyazása URI-azonosítók. ezeket a módszereket használja.</span><span class="sxs-lookup"><span data-stu-id="9ca60-115">hello server uses these methods by passing them URIs that embed short-lived tokens.</span></span> <span data-ttu-id="9ca60-116">Az URI-k is, amelyek nem támogatják a beállítás HTTP-fejlécek hello WebSocket-kézfogással közös WebSocket-verem használható.</span><span class="sxs-lookup"><span data-stu-id="9ca60-116">These URIs can also be used with common WebSocket stacks that do not support setting HTTP headers for hello WebSocket handshake.</span></span> <span data-ttu-id="9ca60-117">Engedélyezési jogkivonatok beillesztése hello URI elsősorban e dokumentumtár-külső használati forgatókönyvek esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="9ca60-117">Embedding authorization tokens into hello URI is supported primarily for those library-external usage scenarios.</span></span> 

#### <a name="createrelaylistenuri"></a><span data-ttu-id="9ca60-118">createRelayListenUri</span><span class="sxs-lookup"><span data-stu-id="9ca60-118">createRelayListenUri</span></span>

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

<span data-ttu-id="9ca60-119">A névtér és az elérési utat adott hello egy érvényes Azure Relay a hibrid kapcsolat figyelő URI hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9ca60-119">Creates a valid Azure Relay Hybrid Connection listener URI for hello given namespace and path.</span></span> <span data-ttu-id="9ca60-120">Ezt az URI majd hello WebSocketServer osztály hello továbbítási verziójával használható.</span><span class="sxs-lookup"><span data-stu-id="9ca60-120">This URI can then be used with hello relay version of hello WebSocketServer class.</span></span>

- <span data-ttu-id="9ca60-121">`namespaceName`hello Azure továbbítási névtér toouse tartomány minősített nevét (szükséges) – hello.</span><span class="sxs-lookup"><span data-stu-id="9ca60-121">`namespaceName` (required) - hello domain-qualified name of hello Azure Relay namespace toouse.</span></span>
- <span data-ttu-id="9ca60-122">`path`(szükséges) – hello egy létező Azure Relay a hibrid kapcsolat, hogy a névtér nevét.</span><span class="sxs-lookup"><span data-stu-id="9ca60-122">`path` (required) - hello name of an existing Azure Relay Hybrid Connection in that namespace.</span></span>
- <span data-ttu-id="9ca60-123">`token`(nem kötelező) – egy korábban kiadott továbbítási access token, amely beágyazott hello figyelő URI (lásd a következő példa hello).</span><span class="sxs-lookup"><span data-stu-id="9ca60-123">`token` (optional) - a previously issued Relay access token that is embedded in hello listener URI (see hello following example).</span></span>
- <span data-ttu-id="9ca60-124">`id`(nem kötelező) – olyan nyomkövetési azonosító, amely lehetővé teszi, hogy a végpont diagnosztikai nyomkövetési kérelmek.</span><span class="sxs-lookup"><span data-stu-id="9ca60-124">`id` (optional) - a tracking identifier that enables end-to-end diagnostics tracking of requests.</span></span>

<span data-ttu-id="9ca60-125">Hello `token` értéket nem kötelező, és meg kell használni, ha már nem lehetséges toosend HTTP-fejlécek hello WebSocket kézfogás, valamint hello W3C WebSocket verem hello esetet.</span><span class="sxs-lookup"><span data-stu-id="9ca60-125">hello `token` value is optional and should only be used when it is not possible toosend HTTP headers along with hello WebSocket handshake, as is hello case with hello W3C WebSocket stack.</span></span>                  


#### <a name="createrelaysenduri"></a><span data-ttu-id="9ca60-126">createRelaySendUri</span><span class="sxs-lookup"><span data-stu-id="9ca60-126">createRelaySendUri</span></span>
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

<span data-ttu-id="9ca60-127">A névtér és az elérési utat adott hello egy érvényes Azure Relay a hibrid kapcsolat küldési URI hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9ca60-127">Creates a valid Azure Relay Hybrid Connection send URI for hello given namespace and path.</span></span> <span data-ttu-id="9ca60-128">A WebSocket ügyfél ezt az URI használható.</span><span class="sxs-lookup"><span data-stu-id="9ca60-128">This URI can be used with any WebSocket client.</span></span>

- <span data-ttu-id="9ca60-129">`namespaceName`hello Azure továbbítási névtér toouse tartomány minősített nevét (szükséges) – hello.</span><span class="sxs-lookup"><span data-stu-id="9ca60-129">`namespaceName` (required) - hello domain-qualified name of hello Azure Relay namespace toouse.</span></span>
- <span data-ttu-id="9ca60-130">`path`(szükséges) – hello egy létező Azure Relay a hibrid kapcsolat, hogy a névtér nevét.</span><span class="sxs-lookup"><span data-stu-id="9ca60-130">`path` (required) - hello name of an existing Azure Relay Hybrid Connection in that namespace.</span></span>
- <span data-ttu-id="9ca60-131">`token`(nem kötelező) – egy korábban kiadott továbbítási hozzáférési jogkivonat hello beágyazott küldése URI (lásd a következő példa hello).</span><span class="sxs-lookup"><span data-stu-id="9ca60-131">`token` (optional) - a previously issued Relay access token that is embedded in hello send URI (see hello following example).</span></span>
- <span data-ttu-id="9ca60-132">`id`(nem kötelező) – olyan nyomkövetési azonosító, amely lehetővé teszi, hogy a végpont diagnosztikai nyomkövetési kérelmek.</span><span class="sxs-lookup"><span data-stu-id="9ca60-132">`id` (optional) - a tracking identifier that enables end-to-end diagnostics tracking of requests.</span></span>

<span data-ttu-id="9ca60-133">Hello `token` értéket nem kötelező, és meg kell használni, ha már nem lehetséges toosend HTTP-fejlécek hello WebSocket kézfogás, valamint hello W3C WebSocket verem hello esetet.</span><span class="sxs-lookup"><span data-stu-id="9ca60-133">hello `token` value is optional and should only be used when it is not possible toosend HTTP headers along with hello WebSocket handshake, as is hello case with hello W3C WebSocket stack.</span></span>                   


#### <a name="createrelaytoken"></a><span data-ttu-id="9ca60-134">createRelayToken</span><span class="sxs-lookup"><span data-stu-id="9ca60-134">createRelayToken</span></span> 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

<span data-ttu-id="9ca60-135">Egy Azure továbbítási közös hozzáférésű Jogosultságkód (SAS) adatblokkot hello megadott cél URI Azonosítóját, SAS-szabály és hello adott másodpercek száma vagy egy óráig aktuális hello azonnali Ha hello lejárati argumentum nincs megadva érvényes SAS-szabály kulcsot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9ca60-135">Creates an Azure Relay Shared Access Signature (SAS) token for hello given target URI, SAS rule, and SAS rule key that is valid for hello given number of seconds or for an hour from hello current instant if hello expiry argument is omitted.</span></span>

- <span data-ttu-id="9ca60-136">`uri`mely hello a token toobe kibocsátott URI (szükséges) – hello.</span><span class="sxs-lookup"><span data-stu-id="9ca60-136">`uri` (required) - hello URI for which hello token is toobe issued.</span></span> <span data-ttu-id="9ca60-137">hello URI normalizált toouse hello HTTP-sémát, és a lekérdezési karakterlánc adatok tisztító van.</span><span class="sxs-lookup"><span data-stu-id="9ca60-137">hello URI is normalized toouse hello HTTP scheme, and query string information is stripped.</span></span>
- <span data-ttu-id="9ca60-138">`ruleName`SAS (kötelező) - szabály vagy megadott URI hello által képviselt hello entitás nevét, vagy a hello névtér által képviselt hello URI gazdagépre utaló.</span><span class="sxs-lookup"><span data-stu-id="9ca60-138">`ruleName` (required) - SAS rule name for either hello entity represented by hello given URI, or for hello namespace represented by hello URI host portion.</span></span>
- <span data-ttu-id="9ca60-139">`key`(szükséges) – hello SAS szabály érvényes kulcsát.</span><span class="sxs-lookup"><span data-stu-id="9ca60-139">`key` (required) - valid key for hello SAS rule.</span></span> 
- <span data-ttu-id="9ca60-140">`expirationSeconds`(nem kötelező) – hello hány másodperc múlva hello generált jogkivonat lejárati.</span><span class="sxs-lookup"><span data-stu-id="9ca60-140">`expirationSeconds` (optional) - hello number of seconds until hello generated token should expire.</span></span> <span data-ttu-id="9ca60-141">Ha nincs megadva, a hello alapértelmezett érték 1 óra (3600).</span><span class="sxs-lookup"><span data-stu-id="9ca60-141">If not specified, hello default is 1 hour (3600).</span></span>

<span data-ttu-id="9ca60-142">hello kiállított jogkivonat ruház megadott hello kapcsolódó hello jogosultságok SAS szabály a megadott időtartam hello.</span><span class="sxs-lookup"><span data-stu-id="9ca60-142">hello issued token confers hello rights associated with hello specified SAS rule for hello given duration.</span></span>

#### <a name="appendrelaytoken"></a><span data-ttu-id="9ca60-143">appendRelayToken</span><span class="sxs-lookup"><span data-stu-id="9ca60-143">appendRelayToken</span></span>

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

<span data-ttu-id="9ca60-144">Ez a módszer akkor funkcionális szempontból egyenértékű toohello `createRelayToken` metódus korábban dokumentált, de beolvasása hello token megfelelően hozzáfűzött toohello bemeneti URI-Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="9ca60-144">This method is functionally equivalent toohello `createRelayToken` method documented previously, but returns hello token correctly appended toohello input URI.</span></span>

### <a name="class-wsrelayedserver"></a><span data-ttu-id="9ca60-145">Osztály ws. RelayedServer</span><span class="sxs-lookup"><span data-stu-id="9ca60-145">Class ws.RelayedServer</span></span>

<span data-ttu-id="9ca60-146">Hello `hycows.RelayedServer` osztály egy alternatív toohello `ws.Server` osztály, amely nem figyel a hello helyi hálózaton, de figyelő toohello Azure továbbítási szolgáltatás gazdakiszolgálója.</span><span class="sxs-lookup"><span data-stu-id="9ca60-146">hello `hycows.RelayedServer` class is an alternative toohello `ws.Server` class that does not listen on hello local network, but delegates listening toohello Azure Relay service.</span></span>

<span data-ttu-id="9ca60-147">hello két osztályokat leginkább kompatibilis, ami azt jelenti, hogy egy meglévő alkalmazást hello szerződés `ws.Server` osztály könnyen módosított toouse továbbítón keresztüli hello verziója lehet.</span><span class="sxs-lookup"><span data-stu-id="9ca60-147">hello two classes are mostly contract compatible, meaning that an existing application using hello `ws.Server` class can easily be changed toouse hello relayed version.</span></span> <span data-ttu-id="9ca60-148">hello fő különbségek a következők: hello konstruktorban és hello elérhető lehetőségek a.</span><span class="sxs-lookup"><span data-stu-id="9ca60-148">hello main differences are in hello constructor and in hello available options.</span></span>

#### <a name="constructor"></a><span data-ttu-id="9ca60-149">Konstruktor</span><span class="sxs-lookup"><span data-stu-id="9ca60-149">Constructor</span></span>  

```JavaScript 
var ws = require('hyco-ws');
var server = ws.RelayedServer;

var wss = new server(
    {
        server: ws.createRelayListenUri(ns, path),
        token: function() { return ws.createRelayToken('http://' + ns, keyrule, key); }
    });
```

<span data-ttu-id="9ca60-150">Hello `RelayedServer` konstruktor támogatja a argumentumok hello mint egy másik készletét `Server`, mivel az nem egy önálló figyelő, vagy egy meglévő HTTP-figyelő keretrendszer beilleszthető képes toobe.</span><span class="sxs-lookup"><span data-stu-id="9ca60-150">hello `RelayedServer` constructor supports a different set of arguments than hello `Server`, because it is not a standalone listener, or able toobe embedded into an existing HTTP listener framework.</span></span> <span data-ttu-id="9ca60-151">Nincsenek is kevesebb lehetőséget mivel hello WebSocket felügyeleti nagymértékben delegált toohello továbbítási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9ca60-151">There are also fewer options available since hello WebSocket management is largely delegated toohello Relay service.</span></span>

<span data-ttu-id="9ca60-152">A konstruktor argumentumai:</span><span class="sxs-lookup"><span data-stu-id="9ca60-152">Constructor arguments:</span></span>

- <span data-ttu-id="9ca60-153">`server`(szükséges) – hello teljesen minősített URI mely toolisten hello WebSocket.createRelayListenUri() segédmetódus általában kialakítani a hibrid kapcsolat neve.</span><span class="sxs-lookup"><span data-stu-id="9ca60-153">`server` (required) - hello fully qualified URI for a Hybrid Connection name on which toolisten, usually constructed with hello WebSocket.createRelayListenUri() helper method.</span></span>
- <span data-ttu-id="9ca60-154">`token`(szükséges) – Ez az argumentum rendelkezik egy korábban kiállított jogkivonat karakterlánc vagy egy visszahívási függvényt, amely a token karakterlánc tooobtain hívható.</span><span class="sxs-lookup"><span data-stu-id="9ca60-154">`token` (required) - this argument holds either a previously issued token string or a callback function that can be called tooobtain such a token string.</span></span> <span data-ttu-id="9ca60-155">hello visszahívási módszert részesíti előnyben, mivel jogkivonat megújítási lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="9ca60-155">hello callback option is preferred, as it enables token renewal.</span></span>

#### <a name="events"></a><span data-ttu-id="9ca60-156">Események</span><span class="sxs-lookup"><span data-stu-id="9ca60-156">Events</span></span>

<span data-ttu-id="9ca60-157">`RelayedServer`példányok kibocsátás három eseményeket, amelyek akkor toohandle bejövő kérések engedélyezéséhez kapcsolatot és a hibát észleli.</span><span class="sxs-lookup"><span data-stu-id="9ca60-157">`RelayedServer` instances emit three events that enable you toohandle incoming requests, establish connections, and detect error conditions.</span></span> <span data-ttu-id="9ca60-158">Elő kell fizetnie toohello `connect` toohandle eseményüzenetek.</span><span class="sxs-lookup"><span data-stu-id="9ca60-158">You must subscribe toohello `connect` event toohandle messages.</span></span> 

##### <a name="headers"></a><span data-ttu-id="9ca60-159">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="9ca60-159">headers</span></span>

```JavaScript 
function(headers)
```

<span data-ttu-id="9ca60-160">Hello `headers` egy esemény jelenik meg a bejövő kapcsolatokat elfogadható, hello fejlécek toosend toohello ügyfél módosításának engedélyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="9ca60-160">hello `headers` event is raised just before an incoming connection is accepted, enabling modification of hello headers toosend toohello client.</span></span> 

##### <a name="connection"></a><span data-ttu-id="9ca60-161">kapcsolat</span><span class="sxs-lookup"><span data-stu-id="9ca60-161">connection</span></span>

```JavaScript
function(socket)
```

<span data-ttu-id="9ca60-162">Ha egy új WebSocket-kapcsolat elfogadható kibocsátott.</span><span class="sxs-lookup"><span data-stu-id="9ca60-162">Emitted when a new WebSocket connection is accepted.</span></span> <span data-ttu-id="9ca60-163">hello objektum típusa nem `ws.WebSocket`, ugyanaz, mint a hello alap csomaggal.</span><span class="sxs-lookup"><span data-stu-id="9ca60-163">hello object is of type `ws.WebSocket`, same as with hello base package.</span></span>


##### <a name="error"></a><span data-ttu-id="9ca60-164">error</span><span class="sxs-lookup"><span data-stu-id="9ca60-164">error</span></span>

```JavaScript
function(error)
```

<span data-ttu-id="9ca60-165">Hello alapjául szolgáló kiszolgáló bocsát ki a hibát, ha továbbítani itt.</span><span class="sxs-lookup"><span data-stu-id="9ca60-165">If hello underlying server emits an error, it is forwarded here.</span></span>  

#### <a name="helpers"></a><span data-ttu-id="9ca60-166">Segítő</span><span class="sxs-lookup"><span data-stu-id="9ca60-166">Helpers</span></span>

<span data-ttu-id="9ca60-167">toosimplify indítása a továbbítón keresztüli kiszolgáló, és azonnal előfizetés tooincoming kapcsolatok hello csomag közzététele egy egyszerű segítő függvénynek, amely is használva van hello példák, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9ca60-167">toosimplify starting a relayed server and immediately subscribing tooincoming connections, hello package exposes a simple helper function, which is also used in hello examples, as follows:</span></span>

##### <a name="createrelayedlistener"></a><span data-ttu-id="9ca60-168">createRelayedListener</span><span class="sxs-lookup"><span data-stu-id="9ca60-168">createRelayedListener</span></span>

```JavaScript
var WebSocket = require('hyco-ws');

var wss = WebSocket.createRelayedServer(
    {
        server: WebSocket.createRelayListenUri(ns, path),
        token: function() { return WebSocket.createRelayToken('http://' + ns, keyrule, key); }
    }, 
    function (ws) {
        console.log('connection accepted');
        ws.onmessage = function (event) {
            console.log(JSON.parse(event.data));
        };
        ws.on('close', function () {
            console.log('connection closed');
        });       
});
``` 

##### <a name="createrelayedserver"></a><span data-ttu-id="9ca60-169">createRelayedServer</span><span class="sxs-lookup"><span data-stu-id="9ca60-169">createRelayedServer</span></span>

```javascript
var server = createRelayedServer([options], [connectCallback] )
```

<span data-ttu-id="9ca60-170">Ez a metódus meghívja hello konstruktor toocreate hello RelayedServer egy új példányát, és majd előfizet megadott hello visszahívási toohello "kapcsolat" esemény.</span><span class="sxs-lookup"><span data-stu-id="9ca60-170">This method calls hello constructor toocreate a new instance of hello RelayedServer and then subscribes hello provided callback toohello 'connection' event.</span></span>
 
##### <a name="relayedconnect"></a><span data-ttu-id="9ca60-171">relayedConnect</span><span class="sxs-lookup"><span data-stu-id="9ca60-171">relayedConnect</span></span>

<span data-ttu-id="9ca60-172">Egyszerűen tükrözés hello `createRelayedServer` függvényben szereplő segítő `relayedConnect` kapcsolatot hoz létre ügyfél és a "Megnyitás" esemény toohello hello eredményül kapott szoftvercsatorna előfizet.</span><span class="sxs-lookup"><span data-stu-id="9ca60-172">Simply mirroring hello `createRelayedServer` helper in function, `relayedConnect` creates a client connection and subscribes toohello 'open' event on hello resulting socket.</span></span>

```JavaScript
var uri = WebSocket.createRelaySendUri(ns, path);
WebSocket.relayedConnect(
    uri,
    WebSocket.createRelayToken(uri, keyrule, key),
    function (socket) {
        ...
    }
);
```

## <a name="next-steps"></a><span data-ttu-id="9ca60-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ca60-173">Next steps</span></span>
<span data-ttu-id="9ca60-174">További információ az Azure továbbítási toolearn látogasson el ezeket a hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="9ca60-174">toolearn more about Azure Relay, visit these links:</span></span>
* [<span data-ttu-id="9ca60-175">Mi az az Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="9ca60-175">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="9ca60-176">Rendelkezésre álló továbbítási API-k</span><span class="sxs-lookup"><span data-stu-id="9ca60-176">Available Relay APIs</span></span>](relay-api-overview.md)
