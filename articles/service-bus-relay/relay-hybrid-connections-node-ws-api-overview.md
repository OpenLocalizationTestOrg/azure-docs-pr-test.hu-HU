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
# <a name="relay-hybrid-connections-node-api-overview"></a>Továbbítják a hibrid kapcsolatok csomópont API – áttekintés

## <a name="overview"></a>Áttekintés

Hello [ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) csomópont csomag Azure Relay a hibrid kapcsolatok épül, és kiterjeszti a hello ["ws"](https://www.npmjs.com/package/ws) NPM csomag. Ez a csomag újra exportálja az alapszintű csomag összes exportálását, és hozzáadja az új sablonként exportálja, amelyek lehetővé teszik a hello Azure továbbítási szolgáltatás hibrid kapcsolatok szolgáltatásával való integráció. 

A meglévő alkalmazásokat, amelyek `require('ws')` használhatja ezt a csomagot a `require('hyco-ws')` helyette, amely is lehetővé teszi hibrid környezetekben, ahol az alkalmazás figyelheti a helyileg a WebSocket-kapcsolatok "belső hello tűzfal" és a hibrid kapcsolatokon keresztül minden a hello azonos idő.
  
## <a name="documentation"></a>Dokumentáció

hello API-jainak [részletes ismertetését lásd: hello fő 'ws"csomag](https://github.com/websockets/ws/blob/master/doc/ws.md). Ez a cikk ismerteti, hogyan eltér az ezt a csomagot, hogy az alaptervhez. 

hello hello ALAPCSOMAG és a "hyco ws" közötti fontosabb különbségeket az új kiszolgáló osztályt, keresztül exportált ad `require('hyco-ws').RelayedServer`, és néhány segédmódszereket.

### <a name="package-helper-methods"></a>Csomag segédmódszereket

Többféleképpen segédprogram érhető el a hello csomag exportálása, melyeket referenciaként használhat az alábbiak szerint:

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

hello segédmódszereket használható ezzel a csomaggal, de a webszolgáltatás vagy az eszköz ügyfelek toocreate figyelői vagy feladók engedélyező egy csomópont kiszolgáló is használható. hello-kiszolgáló úgy, hogy azok rövid élettartamú jogkivonatok beágyazása URI-azonosítók. ezeket a módszereket használja. Az URI-k is, amelyek nem támogatják a beállítás HTTP-fejlécek hello WebSocket-kézfogással közös WebSocket-verem használható. Engedélyezési jogkivonatok beillesztése hello URI elsősorban e dokumentumtár-külső használati forgatókönyvek esetén támogatott. 

#### <a name="createrelaylistenuri"></a>createRelayListenUri

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

A névtér és az elérési utat adott hello egy érvényes Azure Relay a hibrid kapcsolat figyelő URI hoz létre. Ezt az URI majd hello WebSocketServer osztály hello továbbítási verziójával használható.

- `namespaceName`hello Azure továbbítási névtér toouse tartomány minősített nevét (szükséges) – hello.
- `path`(szükséges) – hello egy létező Azure Relay a hibrid kapcsolat, hogy a névtér nevét.
- `token`(nem kötelező) – egy korábban kiadott továbbítási access token, amely beágyazott hello figyelő URI (lásd a következő példa hello).
- `id`(nem kötelező) – olyan nyomkövetési azonosító, amely lehetővé teszi, hogy a végpont diagnosztikai nyomkövetési kérelmek.

Hello `token` értéket nem kötelező, és meg kell használni, ha már nem lehetséges toosend HTTP-fejlécek hello WebSocket kézfogás, valamint hello W3C WebSocket verem hello esetet.                  


#### <a name="createrelaysenduri"></a>createRelaySendUri
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

A névtér és az elérési utat adott hello egy érvényes Azure Relay a hibrid kapcsolat küldési URI hoz létre. A WebSocket ügyfél ezt az URI használható.

- `namespaceName`hello Azure továbbítási névtér toouse tartomány minősített nevét (szükséges) – hello.
- `path`(szükséges) – hello egy létező Azure Relay a hibrid kapcsolat, hogy a névtér nevét.
- `token`(nem kötelező) – egy korábban kiadott továbbítási hozzáférési jogkivonat hello beágyazott küldése URI (lásd a következő példa hello).
- `id`(nem kötelező) – olyan nyomkövetési azonosító, amely lehetővé teszi, hogy a végpont diagnosztikai nyomkövetési kérelmek.

Hello `token` értéket nem kötelező, és meg kell használni, ha már nem lehetséges toosend HTTP-fejlécek hello WebSocket kézfogás, valamint hello W3C WebSocket verem hello esetet.                   


#### <a name="createrelaytoken"></a>createRelayToken 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Egy Azure továbbítási közös hozzáférésű Jogosultságkód (SAS) adatblokkot hello megadott cél URI Azonosítóját, SAS-szabály és hello adott másodpercek száma vagy egy óráig aktuális hello azonnali Ha hello lejárati argumentum nincs megadva érvényes SAS-szabály kulcsot hoz létre.

- `uri`mely hello a token toobe kibocsátott URI (szükséges) – hello. hello URI normalizált toouse hello HTTP-sémát, és a lekérdezési karakterlánc adatok tisztító van.
- `ruleName`SAS (kötelező) - szabály vagy megadott URI hello által képviselt hello entitás nevét, vagy a hello névtér által képviselt hello URI gazdagépre utaló.
- `key`(szükséges) – hello SAS szabály érvényes kulcsát. 
- `expirationSeconds`(nem kötelező) – hello hány másodperc múlva hello generált jogkivonat lejárati. Ha nincs megadva, a hello alapértelmezett érték 1 óra (3600).

hello kiállított jogkivonat ruház megadott hello kapcsolódó hello jogosultságok SAS szabály a megadott időtartam hello.

#### <a name="appendrelaytoken"></a>appendRelayToken

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Ez a módszer akkor funkcionális szempontból egyenértékű toohello `createRelayToken` metódus korábban dokumentált, de beolvasása hello token megfelelően hozzáfűzött toohello bemeneti URI-Azonosítót.

### <a name="class-wsrelayedserver"></a>Osztály ws. RelayedServer

Hello `hycows.RelayedServer` osztály egy alternatív toohello `ws.Server` osztály, amely nem figyel a hello helyi hálózaton, de figyelő toohello Azure továbbítási szolgáltatás gazdakiszolgálója.

hello két osztályokat leginkább kompatibilis, ami azt jelenti, hogy egy meglévő alkalmazást hello szerződés `ws.Server` osztály könnyen módosított toouse továbbítón keresztüli hello verziója lehet. hello fő különbségek a következők: hello konstruktorban és hello elérhető lehetőségek a.

#### <a name="constructor"></a>Konstruktor  

```JavaScript 
var ws = require('hyco-ws');
var server = ws.RelayedServer;

var wss = new server(
    {
        server: ws.createRelayListenUri(ns, path),
        token: function() { return ws.createRelayToken('http://' + ns, keyrule, key); }
    });
```

Hello `RelayedServer` konstruktor támogatja a argumentumok hello mint egy másik készletét `Server`, mivel az nem egy önálló figyelő, vagy egy meglévő HTTP-figyelő keretrendszer beilleszthető képes toobe. Nincsenek is kevesebb lehetőséget mivel hello WebSocket felügyeleti nagymértékben delegált toohello továbbítási szolgáltatás.

A konstruktor argumentumai:

- `server`(szükséges) – hello teljesen minősített URI mely toolisten hello WebSocket.createRelayListenUri() segédmetódus általában kialakítani a hibrid kapcsolat neve.
- `token`(szükséges) – Ez az argumentum rendelkezik egy korábban kiállított jogkivonat karakterlánc vagy egy visszahívási függvényt, amely a token karakterlánc tooobtain hívható. hello visszahívási módszert részesíti előnyben, mivel jogkivonat megújítási lehetővé teszi.

#### <a name="events"></a>Események

`RelayedServer`példányok kibocsátás három eseményeket, amelyek akkor toohandle bejövő kérések engedélyezéséhez kapcsolatot és a hibát észleli. Elő kell fizetnie toohello `connect` toohandle eseményüzenetek. 

##### <a name="headers"></a>Fejlécek

```JavaScript 
function(headers)
```

Hello `headers` egy esemény jelenik meg a bejövő kapcsolatokat elfogadható, hello fejlécek toosend toohello ügyfél módosításának engedélyezése előtt. 

##### <a name="connection"></a>kapcsolat

```JavaScript
function(socket)
```

Ha egy új WebSocket-kapcsolat elfogadható kibocsátott. hello objektum típusa nem `ws.WebSocket`, ugyanaz, mint a hello alap csomaggal.


##### <a name="error"></a>error

```JavaScript
function(error)
```

Hello alapjául szolgáló kiszolgáló bocsát ki a hibát, ha továbbítani itt.  

#### <a name="helpers"></a>Segítő

toosimplify indítása a továbbítón keresztüli kiszolgáló, és azonnal előfizetés tooincoming kapcsolatok hello csomag közzététele egy egyszerű segítő függvénynek, amely is használva van hello példák, az alábbiak szerint:

##### <a name="createrelayedlistener"></a>createRelayedListener

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

##### <a name="createrelayedserver"></a>createRelayedServer

```javascript
var server = createRelayedServer([options], [connectCallback] )
```

Ez a metódus meghívja hello konstruktor toocreate hello RelayedServer egy új példányát, és majd előfizet megadott hello visszahívási toohello "kapcsolat" esemény.
 
##### <a name="relayedconnect"></a>relayedConnect

Egyszerűen tükrözés hello `createRelayedServer` függvényben szereplő segítő `relayedConnect` kapcsolatot hoz létre ügyfél és a "Megnyitás" esemény toohello hello eredményül kapott szoftvercsatorna előfizet.

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

## <a name="next-steps"></a>Következő lépések
További információ az Azure továbbítási toolearn látogasson el ezeket a hivatkozásokat:
* [Mi az az Azure Relay?](relay-what-is-it.md)
* [Rendelkezésre álló továbbítási API-k](relay-api-overview.md)
