---
title: "a node.js várólisták aaaHow toouse Service Bus |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Service Bus várólisták az Azure-ban a Node.js-alkalmazásokat."
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: c55354b2061c41aba1093cc3f12ce2a1bc37a3cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-nodejs"></a>Hogyan toouse Service Bus várólisták a Node.js

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ez a cikk ismerteti, hogyan toouse Service Bus várólisták a Node.js. hello minták JavaScript nyelven íródtak, és Node.js Azure modul hello használata. hello tárgyalt forgatókönyvekben szerepel a **üzenetsorok létrehozása**, **üzenetek küldése és fogadása**, és **várólisták törlése**. A várólisták további információkért lásd: hello [további lépések](#next-steps) szakasz.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-nodejs-application"></a>Node.js alkalmazás létrehozása
Üres Node.js-alkalmazás létrehozása. Útmutatást toocreate egy Node.js-alkalmazást, lásd: [létrehozása és központi telepítése egy Node.js-alkalmazás tooan Azure-webhely][Create and deploy a Node.js application tooan Azure Website], vagy [Node.js Felhőszolgáltatás] [ Node.js Cloud Service] Windows PowerShell használatával.

## <a name="configure-your-application-toouse-service-bus"></a>Az alkalmazás toouse Service Bus konfigurálása
Azure Service Bus toouse töltse le, és hello Node.js Azure csomagot használja. Ez a csomag tartalmaz egy szalagtár szerepel, amely hello Service Bus REST szolgáltatásokkal kommunikálni.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Csomópont Package Manager (NPM) tooobtain hello csomag használata
1. Használjon hello **Windows PowerShell, a Node.js** parancs ablak toonavigate toohello **c:\\csomópont\\sbqueues\\WebRole1** mappa, amelyben létrehozta a mintaalkalmazást.
2. Típus **npm telepítése azure** hello parancssori ablakban, amely kell eredményeznie kimeneti hasonló toohello következő:

    ```
    azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```
3. Manuális futtatásával hello **ls** parancs tooverify, amely egy **node_modules** mappa hozták létre. A mappában található hello belül **azure** csomagot, amely tartalmazza a Service Bus-üzenetsorok tooaccess kell hello szalagtárak.

### <a name="import-hello-module"></a>Importálás hello modul
A Jegyzettömbben vagy más szövegszerkesztőben, adja hozzá a következő toohello felső részén hello hello **server.js** hello alkalmazás fájlt:

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Az Azure Service Bus-kapcsolat beállítása
hello Azure a modul olvassa be hello környezeti változó `AZURE_SERVICEBUS_CONNECTION_STRING` szükséges tooconnect tooService Bus tooobtain információt. Ha e környezeti változó nincs beállítva, meg kell adnia hello fiók meghívásakor `createServiceBusService`.

A konfigurációs fájlban hello környezeti változók beállítása az Azure Cloud Service példát lásd: [Node.js Felhőszolgáltatás tárolási][Node.js Cloud Service with Storage].

Példa a hello hello környezeti változók beállítása [Azure-portálon] [ Azure portal] egy Azure-webhelyre, lásd: [Node.js-webalkalmazás tárolóval] [ Node.js Web Application with Storage].

## <a name="create-a-queue"></a>Üzenetsor létrehozása
Hello **ServiceBusService** objektum lehetővé teszi a Service Bus-üzenetsorok toowork. hello alábbi kód létrehoz egy **ServiceBusService** objektum. Hello hello tetején adja hozzá **server.js** fájl hello utasítás tooimport hello Azure modul után:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Meghívásával `createQueueIfNotExists` a hello **ServiceBusService** objektum, hello megadott várólista ad vissza (ha van ilyen), vagy létrehoz egy új várólistát hello megadott névvel. hello következő kódban `createQueueIfNotExists` toocreate vagy kapcsolódási nevű toohello várólista `myqueue`:

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

Hello `createServiceBusService` metódus is támogatja a további beállításokat, amelyek lehetővé teszik a toooverride alapértelmezett várólista-beállításokat például üzenet ideje toolive és maximális várólista-méret. hello alábbi mintakód hello várólista maximális mérete too5 GB, és egy időt toolive (TTL) értéke 1 perc:

```javascript
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Szűrők
Választható szűrési műveletek lehet segítségével alkalmazott toooperations **ServiceBusService**. Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. Szűrők hello aláírással metódus megvalósító objektumok a következők:

```javascript
function handle (requestOptions, next)
```

Ezután az előzetes feldolgozás hello kérelem lehetőségekről, hello metódust kell hívni `next`, egy visszahívási átadja a aláírását hello:

```javascript
function (returnObject, finalCallback, next)
```

A visszahívási, és feldolgozás hello követően `returnObject` (hello hello kérelem toohello kiszolgálótól kapott válasz), hello visszahívási vagy kell meghívni `next` Ha azt egyéb szűrők feldolgozása toocontinue létezik, vagy egyszerűen meghívása `finalCallback`, mely vége hello szolgáltatás hívása.

Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el a hello Azure SDK for Node.js, `ExponentialRetryPolicyFilter` és `LinearRetryPolicyFilter`. hello alábbi kód létrehoz egy `ServiceBusService` hello használó objektum `ExponentialRetryPolicyFilter`:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-tooa-queue"></a>Üzenetek tooa várólista küldése
toosend üzenet tooa Service Bus-üzenetsorba, az alkalmazás meghívja hello `sendQueueMessage` hello metódusa **ServiceBusService** objektum. Üzenetek túl elküldött (és a fogadott) Service Bus-üzenetsor **BrokeredMessage** objektumokat, és rendelkezik egy szabványos tulajdonságkészlettel (például **címke** és **TimeToLive**), egy amely használt toohold egyéni alkalmazásspecifikus tulajdonságokat, valamint egy tetszőleges alkalmazásadatokból álló törzzsel szótárban. Az alkalmazás beállíthatja hello üzenet törzsét hello úgy, hogy egy karakterláncot hello üzenetben. Egyéb szabványos tulajdonságokat a rendszer feltölti az alapértelmezett értékekkel.

hello következő példa bemutatja, hogyan toosend egy teszt üzenetsor toohello nevű `myqueue` használatával `sendQueueMessage`:

```javascript
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Service Bus-üzenetsorok támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md). hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet. Az üzenetsorban tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello üzenetsor által tárolt hello üzenetek teljes mérete. Az üzenetsor ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB. Kvóták kapcsolatos további információkért lásd: [Service Bus kvóták][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Üzenetek fogadása egy üzenetsorból
Üzenetek fogadása egy üzenetsorból hello segítségével `receiveQueueMessage` hello metódusa **ServiceBusService** objektum. Alapértelmezés szerint az üzenetek törlődnek hello várólistából, olvasott; azonban (betekintés) olvashatja és üdvözlőüzenetére zárolása hello nem kötelező paraméter beállítása hello várólista törlése nélkül `isPeekLock` túl**igaz**.

hello olvasási alapértelmezett viselkedését, és üdvözlőüzenetére törlése, hello részét fogadási művelethez hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban. toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt. Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.

Ha hello `isPeekLock` paraméter értéke túl**igaz**, hello kap két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz. A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása. Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával `deleteMessage` metódus és a hello üzenet toobe paraméterként törölték. Hello `deleteMessage` metódus hello üzenetet, feldolgozottként jelöli meg, és eltávolítja a hello várólista.

hello következő példa bemutatja, hogyan tooreceive és folyamat-üzenetek használatával `receiveQueueMessage`. hello példa először kap, és törli az üzenetet, és majd kap egy üzenetet használatával `isPeekLock` beállítása túl**igaz**, majd törli hello üzenetet `deleteMessage`:

```javascript
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hogyan toohandle omlik össze és nem olvasható üzenetek
Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít. Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlockMessage` hello metódusa **ServiceBusService** objektum. Ezzel a Service Bus toounlock okozhat a hello várólista üzenet és révén elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.

Emellett van hello várólistában lévő társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess hello előtt hello zárolás időtúllépését lejárta (pl. Ha hello alkalmazás összeomlik), akkor a Service Bus automatikusan feloldást köszönőüzenetei és a rendelkezésre álló toobe újbóli fogadását.

A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `deleteMessage` metódust, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazás, amikor újraindul. Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet. Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését. Ez gyakran érhető el hello segítségével **MessageId** hello üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.

## <a name="next-steps"></a>Következő lépések
További információ az üzenetsorok, toolearn lásd: a következő erőforrások hello.

* [Üzenetsorok, témakörök és előfizetések][Queues, topics, and subscriptions]
* [Az Azure SDK csomópont] [ Azure SDK for Node] GitHub tárházából
* [Node.js fejlesztői központ](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application tooan Azure Website]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
