---
title: "Az Azure továbbítási csomópont API-k áttekintése |} Microsoft Docs"
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
ms.openlocfilehash: 28526c05c7f364f0fcaaa362fc97857f850040ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="relay-hybrid-connections-node-api-overview"></a><span data-ttu-id="06ff0-103">Továbbítják a hibrid kapcsolatok csomópont API – áttekintés</span><span class="sxs-lookup"><span data-stu-id="06ff0-103">Relay Hybrid Connections Node API overview</span></span>

## <a name="overview"></a><span data-ttu-id="06ff0-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="06ff0-104">Overview</span></span>

<span data-ttu-id="06ff0-105">A [ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) csomópont csomag Azure Relay a hibrid kapcsolatok épül, és kiterjeszti a ["ws"](https://www.npmjs.com/package/ws) NPM csomag.</span><span class="sxs-lookup"><span data-stu-id="06ff0-105">The [`hyco-ws`](https://www.npmjs.com/package/hyco-ws) Node package for Azure Relay Hybrid Connections is built on and extends the ['ws'](https://www.npmjs.com/package/ws) NPM package.</span></span> <span data-ttu-id="06ff0-106">Ez a csomag újra exportálja az alapszintű csomag összes exportálását, és hozzáadja az új sablonként exportálja, amelyek lehetővé teszik az Azure-továbbító szolgáltatás hibrid kapcsolatok szolgáltatásával való integráció.</span><span class="sxs-lookup"><span data-stu-id="06ff0-106">This package re-exports all exports of that base package and adds new exports that enable integration with the Azure Relay service Hybrid Connections feature.</span></span> 

<span data-ttu-id="06ff0-107">A meglévő alkalmazásokat, amelyek `require('ws')` használhatja ezt a csomagot a `require('hyco-ws')` helyette, amely is lehetővé teszi hibrid környezetekben, ahol az alkalmazás figyelheti a WebSocket-kapcsolatokat "belül a tűzfal" a helyi és hibrid kapcsolatokon keresztül minden a egy időben.</span><span class="sxs-lookup"><span data-stu-id="06ff0-107">Existing applications that `require('ws')` can use this package with `require('hyco-ws')` instead, which also enables hybrid scenarios in which an application can listen for WebSocket connections locally from "inside the firewall" and via Hybrid Connections, all at the same time.</span></span>
  
## <a name="documentation"></a><span data-ttu-id="06ff0-108">Dokumentáció</span><span class="sxs-lookup"><span data-stu-id="06ff0-108">Documentation</span></span>

<span data-ttu-id="06ff0-109">Az API-jainak [részletes ismertetését lásd: a fő 'ws"csomag](https://github.com/websockets/ws/blob/master/doc/ws.md).</span><span class="sxs-lookup"><span data-stu-id="06ff0-109">The APIs are [documented in the main 'ws' package](https://github.com/websockets/ws/blob/master/doc/ws.md).</span></span> <span data-ttu-id="06ff0-110">Ez a cikk ismerteti, hogyan eltér az ezt a csomagot, hogy az alaptervhez.</span><span class="sxs-lookup"><span data-stu-id="06ff0-110">This article describes how this package differs from that baseline.</span></span> 

<span data-ttu-id="06ff0-111">Az alapszintű csomag és a "hyco ws" közötti legfőbb különbségek az új kiszolgáló osztályt, keresztül exportált ad `require('hyco-ws').RelayedServer`, és néhány segédmódszereket.</span><span class="sxs-lookup"><span data-stu-id="06ff0-111">The key differences between the base package and this 'hyco-ws' is that it adds a new server class, exported via `require('hyco-ws').RelayedServer`, and a few helper methods.</span></span>

### <a name="package-helper-methods"></a><span data-ttu-id="06ff0-112">Csomag segédmódszereket</span><span class="sxs-lookup"><span data-stu-id="06ff0-112">Package helper methods</span></span>

<span data-ttu-id="06ff0-113">Többféleképpen segédprogram érhető el a csomag export, melyeket referenciaként használhat az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="06ff0-113">There are several utility methods available on the package export that you can reference as follows:</span></span>

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

<span data-ttu-id="06ff0-114">A segédmódszereket használható ezzel a csomaggal, de a webszolgáltatás vagy az eszköz ügyfelek figyelői vagy feladók hozzon létre egy csomópont kiszolgáló is használható.</span><span class="sxs-lookup"><span data-stu-id="06ff0-114">The helper methods are for use with this package, but can also be used by a Node server for enabling web or device clients to create listeners or senders.</span></span> <span data-ttu-id="06ff0-115">A kiszolgáló úgy, hogy azok rövid élettartamú jogkivonatok beágyazása URI-azonosítók. ezeket a módszereket használja.</span><span class="sxs-lookup"><span data-stu-id="06ff0-115">The server uses these methods by passing them URIs that embed short-lived tokens.</span></span> <span data-ttu-id="06ff0-116">Az URI-k is, hogy a beállítás HTTP-fejlécek nem támogatják a WebSocket-kézfogással közös WebSocket-verem használható.</span><span class="sxs-lookup"><span data-stu-id="06ff0-116">These URIs can also be used with common WebSocket stacks that do not support setting HTTP headers for the WebSocket handshake.</span></span> <span data-ttu-id="06ff0-117">Engedélyezési jogkivonatok beillesztése az URI elsősorban e dokumentumtár-külső használati forgatókönyvek támogatott.</span><span class="sxs-lookup"><span data-stu-id="06ff0-117">Embedding authorization tokens into the URI is supported primarily for those library-external usage scenarios.</span></span> 

#### <a name="createrelaylistenuri"></a><span data-ttu-id="06ff0-118">createRelayListenUri</span><span class="sxs-lookup"><span data-stu-id="06ff0-118">createRelayListenUri</span></span>

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

<span data-ttu-id="06ff0-119">A megadott névtér és elérési útja egy érvényes Azure Relay a hibrid kapcsolat figyelő URI hoz létre.</span><span class="sxs-lookup"><span data-stu-id="06ff0-119">Creates a valid Azure Relay Hybrid Connection listener URI for the given namespace and path.</span></span> <span data-ttu-id="06ff0-120">Ezt az URI használható a WebSocketServer osztály a továbbító verziójával.</span><span class="sxs-lookup"><span data-stu-id="06ff0-120">This URI can then be used with the relay version of the WebSocketServer class.</span></span>

- <span data-ttu-id="06ff0-121">`namespaceName`(szükséges) – a tartomány minősített név a Azure továbbítási névtér használatára.</span><span class="sxs-lookup"><span data-stu-id="06ff0-121">`namespaceName` (required) - the domain-qualified name of the Azure Relay namespace to use.</span></span>
- <span data-ttu-id="06ff0-122">`path`(kötelező) – a neve egy létező Azure Relay a hibrid kapcsolat az adott névtérben.</span><span class="sxs-lookup"><span data-stu-id="06ff0-122">`path` (required) - the name of an existing Azure Relay Hybrid Connection in that namespace.</span></span>
- <span data-ttu-id="06ff0-123">`token`(nem kötelező) – egy korábban kiadott továbbítási access token, amely a figyelő URI beágyazott (lásd a következő példát).</span><span class="sxs-lookup"><span data-stu-id="06ff0-123">`token` (optional) - a previously issued Relay access token that is embedded in the listener URI (see the following example).</span></span>
- <span data-ttu-id="06ff0-124">`id`(nem kötelező) – olyan nyomkövetési azonosító, amely lehetővé teszi, hogy a végpont diagnosztikai nyomkövetési kérelmek.</span><span class="sxs-lookup"><span data-stu-id="06ff0-124">`id` (optional) - a tracking identifier that enables end-to-end diagnostics tracking of requests.</span></span>

<span data-ttu-id="06ff0-125">A `token` értéket nem kötelező, és csak amikor nincs lehetőség küldeni HTTP-fejlécekkel együtt a WebSocket-kézfogás, mint a W3C WebSocket veremmel rendelkező esetében használható.</span><span class="sxs-lookup"><span data-stu-id="06ff0-125">The `token` value is optional and should only be used when it is not possible to send HTTP headers along with the WebSocket handshake, as is the case with the W3C WebSocket stack.</span></span>                  


#### <a name="createrelaysenduri"></a><span data-ttu-id="06ff0-126">createRelaySendUri</span><span class="sxs-lookup"><span data-stu-id="06ff0-126">createRelaySendUri</span></span>
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

<span data-ttu-id="06ff0-127">A megadott névtér és elérési útja egy érvényes Azure Relay a hibrid kapcsolat küldési URI hoz létre.</span><span class="sxs-lookup"><span data-stu-id="06ff0-127">Creates a valid Azure Relay Hybrid Connection send URI for the given namespace and path.</span></span> <span data-ttu-id="06ff0-128">A WebSocket ügyfél ezt az URI használható.</span><span class="sxs-lookup"><span data-stu-id="06ff0-128">This URI can be used with any WebSocket client.</span></span>

- <span data-ttu-id="06ff0-129">`namespaceName`(szükséges) – a tartomány minősített név a Azure továbbítási névtér használatára.</span><span class="sxs-lookup"><span data-stu-id="06ff0-129">`namespaceName` (required) - the domain-qualified name of the Azure Relay namespace to use.</span></span>
- <span data-ttu-id="06ff0-130">`path`(kötelező) – a neve egy létező Azure Relay a hibrid kapcsolat az adott névtérben.</span><span class="sxs-lookup"><span data-stu-id="06ff0-130">`path` (required) - the name of an existing Azure Relay Hybrid Connection in that namespace.</span></span>
- <span data-ttu-id="06ff0-131">`token`(nem kötelező) – egy korábban kiadott továbbítási access token, amely a küldési URI beágyazott (lásd a következő példát).</span><span class="sxs-lookup"><span data-stu-id="06ff0-131">`token` (optional) - a previously issued Relay access token that is embedded in the send URI (see the following example).</span></span>
- <span data-ttu-id="06ff0-132">`id`(nem kötelező) – olyan nyomkövetési azonosító, amely lehetővé teszi, hogy a végpont diagnosztikai nyomkövetési kérelmek.</span><span class="sxs-lookup"><span data-stu-id="06ff0-132">`id` (optional) - a tracking identifier that enables end-to-end diagnostics tracking of requests.</span></span>

<span data-ttu-id="06ff0-133">A `token` értéket nem kötelező, és csak amikor nincs lehetőség küldeni HTTP-fejlécekkel együtt a WebSocket-kézfogás, mint a W3C WebSocket veremmel rendelkező esetében használható.</span><span class="sxs-lookup"><span data-stu-id="06ff0-133">The `token` value is optional and should only be used when it is not possible to send HTTP headers along with the WebSocket handshake, as is the case with the W3C WebSocket stack.</span></span>                   


#### <a name="createrelaytoken"></a><span data-ttu-id="06ff0-134">createRelayToken</span><span class="sxs-lookup"><span data-stu-id="06ff0-134">createRelayToken</span></span> 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

<span data-ttu-id="06ff0-135">Létrehoz egy Azure továbbítási közös hozzáférésű Jogosultságkód (SAS) jogkivonatot adott cél URI Azonosítóját, a SAS-szabály és a SAS-szabály kulcs, amely érvényes a megadott számú másodpercig vagy az aktuális azonnali órán keresztül, ha a lejárati argumentum.</span><span class="sxs-lookup"><span data-stu-id="06ff0-135">Creates an Azure Relay Shared Access Signature (SAS) token for the given target URI, SAS rule, and SAS rule key that is valid for the given number of seconds or for an hour from the current instant if the expiry argument is omitted.</span></span>

- <span data-ttu-id="06ff0-136">`uri`(szükséges) – az URI Azonosítót, amelynek a token kell végezni.</span><span class="sxs-lookup"><span data-stu-id="06ff0-136">`uri` (required) - the URI for which the token is to be issued.</span></span> <span data-ttu-id="06ff0-137">Az URI normalizálva van a HTTP protokollt használja, és a lekérdezési karakterlánc adatok van csíkozott.</span><span class="sxs-lookup"><span data-stu-id="06ff0-137">The URI is normalized to use the HTTP scheme, and query string information is stripped.</span></span>
- <span data-ttu-id="06ff0-138">`ruleName`(szükséges) – SAS szabály neve vagy URI-Azonosítóhoz által képviselt entitás vagy a névtér URI gazdagépre utaló képviseli.</span><span class="sxs-lookup"><span data-stu-id="06ff0-138">`ruleName` (required) - SAS rule name for either the entity represented by the given URI, or for the namespace represented by the URI host portion.</span></span>
- <span data-ttu-id="06ff0-139">`key`(szükséges) – a biztonsági Társítások szabály érvényes kulcs.</span><span class="sxs-lookup"><span data-stu-id="06ff0-139">`key` (required) - valid key for the SAS rule.</span></span> 
- <span data-ttu-id="06ff0-140">`expirationSeconds`(választható) - másodpercben, amíg a generált jogkivonat lejárati.</span><span class="sxs-lookup"><span data-stu-id="06ff0-140">`expirationSeconds` (optional) - the number of seconds until the generated token should expire.</span></span> <span data-ttu-id="06ff0-141">Ha nincs megadva, az alapértelmezett érték 1 óra (3600).</span><span class="sxs-lookup"><span data-stu-id="06ff0-141">If not specified, the default is 1 hour (3600).</span></span>

<span data-ttu-id="06ff0-142">A kibocsátott jogkivonat ruház megadott időtartama alatt a megadott SAS-szabály kapcsolódó jogosultságok.</span><span class="sxs-lookup"><span data-stu-id="06ff0-142">The issued token confers the rights associated with the specified SAS rule for the given duration.</span></span>

#### <a name="appendrelaytoken"></a><span data-ttu-id="06ff0-143">appendRelayToken</span><span class="sxs-lookup"><span data-stu-id="06ff0-143">appendRelayToken</span></span>

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

<span data-ttu-id="06ff0-144">Ez a módszer akkor funkcionális szempontból egyenértékű az `createRelayToken` metódus dokumentált korábban, de a megfelelően hozzáfűzi a bemeneti URI jogkivonatot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="06ff0-144">This method is functionally equivalent to the `createRelayToken` method documented previously, but returns the token correctly appended to the input URI.</span></span>

### <a name="class-wsrelayedserver"></a><span data-ttu-id="06ff0-145">Osztály ws. RelayedServer</span><span class="sxs-lookup"><span data-stu-id="06ff0-145">Class ws.RelayedServer</span></span>

<span data-ttu-id="06ff0-146">A `hycows.RelayedServer` osztály helyett a `ws.Server` osztály, amely a helyi hálózaton, de az Azure-továbbítási szolgáltatás figyel delegáltak nem figyel.</span><span class="sxs-lookup"><span data-stu-id="06ff0-146">The `hycows.RelayedServer` class is an alternative to the `ws.Server` class that does not listen on the local network, but delegates listening to the Azure Relay service.</span></span>

<span data-ttu-id="06ff0-147">A két kompatibilis, ami azt jelenti, hogy egy meglévő alkalmazást, amely főleg szerződés a `ws.Server` osztály könnyen módosíthatja a továbbítón keresztüli verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="06ff0-147">The two classes are mostly contract compatible, meaning that an existing application using the `ws.Server` class can easily be changed to use the relayed version.</span></span> <span data-ttu-id="06ff0-148">A fő különbség a konstruktorban, és a rendelkezésre álló lehetőségeket vannak.</span><span class="sxs-lookup"><span data-stu-id="06ff0-148">The main differences are in the constructor and in the available options.</span></span>

#### <a name="constructor"></a><span data-ttu-id="06ff0-149">Konstruktor</span><span class="sxs-lookup"><span data-stu-id="06ff0-149">Constructor</span></span>  

```JavaScript 
var ws = require('hyco-ws');
var server = ws.RelayedServer;

var wss = new server(
    {
        server: ws.createRelayListenUri(ns, path),
        token: function() { return ws.createRelayToken('http://' + ns, keyrule, key); }
    });
```

<span data-ttu-id="06ff0-150">A `RelayedServer` konstruktor támogatja a argumentumok, mint egy másik készletét a `Server`, mivel az nem egy önálló figyelő vagy egy meglévő HTTP-figyelő keretrendszer beilleszthető tudni.</span><span class="sxs-lookup"><span data-stu-id="06ff0-150">The `RelayedServer` constructor supports a different set of arguments than the `Server`, because it is not a standalone listener, or able to be embedded into an existing HTTP listener framework.</span></span> <span data-ttu-id="06ff0-151">Nincsenek is kevesebb lehetőséget a WebSocket felügyeleti nagymértékben delegált a továbbítási szolgáltatás óta.</span><span class="sxs-lookup"><span data-stu-id="06ff0-151">There are also fewer options available since the WebSocket management is largely delegated to the Relay service.</span></span>

<span data-ttu-id="06ff0-152">A konstruktor argumentumai:</span><span class="sxs-lookup"><span data-stu-id="06ff0-152">Constructor arguments:</span></span>

- <span data-ttu-id="06ff0-153">`server`(szükséges) – a teljes URI a hibrid kapcsolat neve a figyelésre, általában kialakítani WebSocket.createRelayListenUri() segédmetódus.</span><span class="sxs-lookup"><span data-stu-id="06ff0-153">`server` (required) - the fully qualified URI for a Hybrid Connection name on which to listen, usually constructed with the WebSocket.createRelayListenUri() helper method.</span></span>
- <span data-ttu-id="06ff0-154">`token`(szükséges) – Ez az argumentum rendelkezik egy korábban kiállított jogkivonat karakterlánc vagy egy visszahívási függvényt, amely a token karakterlánc beszerzése hívható.</span><span class="sxs-lookup"><span data-stu-id="06ff0-154">`token` (required) - this argument holds either a previously issued token string or a callback function that can be called to obtain such a token string.</span></span> <span data-ttu-id="06ff0-155">A visszahívási beállítás részesíti előnyben, mivel lehetővé teszi a jogkivonat megújítási.</span><span class="sxs-lookup"><span data-stu-id="06ff0-155">The callback option is preferred, as it enables token renewal.</span></span>

#### <a name="events"></a><span data-ttu-id="06ff0-156">Események</span><span class="sxs-lookup"><span data-stu-id="06ff0-156">Events</span></span>

<span data-ttu-id="06ff0-157">`RelayedServer`példányok kibocsátás három események, amelyek lehetővé teszik, hogy a bejövő kérelmeket kezelnek, kapcsolatot és a hibát észleli.</span><span class="sxs-lookup"><span data-stu-id="06ff0-157">`RelayedServer` instances emit three events that enable you to handle incoming requests, establish connections, and detect error conditions.</span></span> <span data-ttu-id="06ff0-158">Elő kell fizetnie a `connect` esemény üzenetek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="06ff0-158">You must subscribe to the `connect` event to handle messages.</span></span> 

##### <a name="headers"></a><span data-ttu-id="06ff0-159">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="06ff0-159">headers</span></span>

```JavaScript 
function(headers)
```

<span data-ttu-id="06ff0-160">A `headers` egy esemény jelenik meg a bejövő kapcsolatokat elfogadható, elküld az ügyfélnek a fejlécek módosítása engedélyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="06ff0-160">The `headers` event is raised just before an incoming connection is accepted, enabling modification of the headers to send to the client.</span></span> 

##### <a name="connection"></a><span data-ttu-id="06ff0-161">kapcsolat</span><span class="sxs-lookup"><span data-stu-id="06ff0-161">connection</span></span>

```JavaScript
function(socket)
```

<span data-ttu-id="06ff0-162">Ha egy új WebSocket-kapcsolat elfogadható kibocsátott.</span><span class="sxs-lookup"><span data-stu-id="06ff0-162">Emitted when a new WebSocket connection is accepted.</span></span> <span data-ttu-id="06ff0-163">Az objektum típusa nem `ws.WebSocket`, ugyanaz, mint az alapszintű csomag.</span><span class="sxs-lookup"><span data-stu-id="06ff0-163">The object is of type `ws.WebSocket`, same as with the base package.</span></span>


##### <a name="error"></a><span data-ttu-id="06ff0-164">error</span><span class="sxs-lookup"><span data-stu-id="06ff0-164">error</span></span>

```JavaScript
function(error)
```

<span data-ttu-id="06ff0-165">Ha az alapul szolgáló server bocsát ki a hibát, továbbítja azt itt.</span><span class="sxs-lookup"><span data-stu-id="06ff0-165">If the underlying server emits an error, it is forwarded here.</span></span>  

#### <a name="helpers"></a><span data-ttu-id="06ff0-166">Segítő</span><span class="sxs-lookup"><span data-stu-id="06ff0-166">Helpers</span></span>

<span data-ttu-id="06ff0-167">Egyszerűbbé teheti a továbbítón keresztüli kiszolgáló indításához, és azonnal fizessen elő a bejövő kapcsolatok, a csomag közzététele egy egyszerű segítő függvénynek, amely is szolgál a példákban az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="06ff0-167">To simplify starting a relayed server and immediately subscribing to incoming connections, the package exposes a simple helper function, which is also used in the examples, as follows:</span></span>

##### <a name="createrelayedlistener"></a><span data-ttu-id="06ff0-168">createRelayedListener</span><span class="sxs-lookup"><span data-stu-id="06ff0-168">createRelayedListener</span></span>

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

##### <a name="createrelayedserver"></a><span data-ttu-id="06ff0-169">createRelayedServer</span><span class="sxs-lookup"><span data-stu-id="06ff0-169">createRelayedServer</span></span>

```javascript
var server = createRelayedServer([options], [connectCallback] )
```

<span data-ttu-id="06ff0-170">Ez a metódus meghívja a konstruktor a RelayedServer új példányának létrehozása, és majd előfizet a "kapcsolat" eseményhez megadott visszahívás.</span><span class="sxs-lookup"><span data-stu-id="06ff0-170">This method calls the constructor to create a new instance of the RelayedServer and then subscribes the provided callback to the 'connection' event.</span></span>
 
##### <a name="relayedconnect"></a><span data-ttu-id="06ff0-171">relayedConnect</span><span class="sxs-lookup"><span data-stu-id="06ff0-171">relayedConnect</span></span>

<span data-ttu-id="06ff0-172">Egyszerűen tükrözést a `createRelayedServer` függvényben szereplő segítő `relayedConnect` kapcsolatot hoz létre ügyfél és a "Megnyitás" esemény az eredményül kapott szoftvercsatorna számítógépcsoportra fizetett elő.</span><span class="sxs-lookup"><span data-stu-id="06ff0-172">Simply mirroring the `createRelayedServer` helper in function, `relayedConnect` creates a client connection and subscribes to the 'open' event on the resulting socket.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="06ff0-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06ff0-173">Next steps</span></span>
<span data-ttu-id="06ff0-174">Azure-továbbítási kapcsolatos további információkért látogasson el ezeket a hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="06ff0-174">To learn more about Azure Relay, visit these links:</span></span>
* [<span data-ttu-id="06ff0-175">Mi az az Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="06ff0-175">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="06ff0-176">Rendelkezésre álló továbbítási API-k</span><span class="sxs-lookup"><span data-stu-id="06ff0-176">Available Relay APIs</span></span>](relay-api-overview.md)
