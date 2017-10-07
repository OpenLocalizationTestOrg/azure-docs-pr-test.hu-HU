---
title: "Azure Service Bus aaaHow toouse üzenettémák és előfizetések a Node.js |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Service Bus üzenettémák és előfizetések az Azure-ban a Node.js-alkalmazásokat."
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: e8f6e7ad6ed16d844c408337ac9e50f990e3fafd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-nodejs"></a>Hogyan tooUse Service Bus üzenettémák és előfizetések a Node.js

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ez az útmutató ismerteti, hogyan toouse Service Bus üzenettémák és előfizetések a Node.js-alkalmazások. hello tárgyalt forgatókönyvekben szerepel a **üzenettémák és előfizetések létrehozása**, **előfizetés-szűrők létrehozása**, **üzenetküldésre** tooa témakör **fogadása előfizetés üzeneteit**, és **üzenettémák és előfizetések törlése**. Üzenettémakörökkel és előfizetésekkel kapcsolatos további információkért lásd: hello [további lépések](#next-steps) szakasz.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Node.js alkalmazás létrehozása
Üres Node.js-alkalmazás létrehozása. Node.js-alkalmazás létrehozása, lásd: [létrehozása és központi telepítése egy Node.js-alkalmazás tooan Azure webhelyére], [Node.js felhőalapú szolgáltatás] [ Node.js Cloud Service] Windows használatával PowerShell, vagy a webhelyet a WebMatrix.

## <a name="configure-your-application-toouse-service-bus"></a>Az alkalmazás toouse Service Bus konfigurálása
a Service Bus toouse hello Node.js Azure-csomag letöltése. Ez a csomag tartalmaz egy szalagtár szerepel, amely hello Service Bus REST szolgáltatásokkal kommunikálni.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Csomópont Package Manager (NPM) tooobtain hello csomag használata
1. Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac) vagy **Bash** (Unix), keresse meg a toohello mappa, amelyben létrehozta a mintaalkalmazást.
2. Típus **npm telepítése azure** hello parancssori ablakban, amely eredményez a következő kimeneti hello:

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
3. Manuális futtatásával hello **ls** parancs tooverify, amely egy **csomópont\_modulok** mappa hozták létre. A mappában található a **azure** csomagot, amely tartalmaz hello szalagtárak tooaccess Service Bus-üzenettémakörök van szüksége.

### <a name="import-hello-module"></a>Importálás hello modul
A Jegyzettömbben vagy más szövegszerkesztőben, adja hozzá a következő toohello felső részén hello hello **server.js** hello alkalmazás fájlt:

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>A Service Bus-kapcsolat beállítása
hello Azure a modul olvassa be hello környezeti változók `AZURE_SERVICEBUS_NAMESPACE` és `AZURE_SERVICEBUS_ACCESS_KEY` információt tooconnect tooService Bus szükséges. Ha ezek a környezeti változók nem, meg kell adnia hello fiók meghívásakor `createServiceBusService`.

Hello környezeti változók beállítása az Azure-Felhőszolgáltatásban példát talál [Node.js Felhőszolgáltatás tárolási][Node.js Cloud Service with Storage].

Az Azure-webhely hello környezeti változók beállítása példát lásd: [Node.js-webalkalmazás tárolóval][Node.js Web Application with Storage].

## <a name="create-a-topic"></a>Üzenettémakör létrehozása
Hello **ServiceBusService** objektum teszi lehetővé a témakörök toowork. Az alábbi kód létrehoz egy **ServiceBusService** objektum. Adja hozzá az tetején hello **server.js** fájl hello utasítás tooimport hello azure modul után:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Meghívásával `createTopicIfNotExists` a hello **ServiceBusService** objektum, hello megadva, a témakör az eredmény (ha van ilyen), vagy egy új téma hello megadott névvel jön létre. hello következő kódban `createTopicIfNotExists` toocreate vagy kapcsolódási nevű toohello témakör `MyTopic`:

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

Hello `createServiceBusService` metódus is támogatja a további beállításokat, amelyek lehetővé teszik a témakör alapbeállítások toooverride például üzenet time to live vagy maximális témakör ezen méretét. hello alábbi mintakód hello maximális témakör mérete too5GB idővel rendelkező toolive 1 perces:

```javascript
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Szűrők
Választható szűrési műveletek lehet segítségével alkalmazott toooperations **ServiceBusService**. Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. Szűrők hello aláírással metódus megvalósító objektumok a következők:

```javascript
function handle (requestOptions, next)
```

A hello lehetőségek előfeldolgozása elvégzése után hello metódushívások `next`, egy visszahívási átadja a aláírását hello:

```javascript
function (returnObject, finalCallback, next)
```

A visszahívási, és feldolgozás hello követően `returnObject` (hello hello kérelem toohello kiszolgálótól kapott válasz), hello visszahívási kell tooeither következő meghívni, ha más szűrők feldolgozása toocontinue létezik, vagy meghívása `finalCallback` ellenkező esetben tooend hello szolgáltatás hívása.

Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el a hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**. hello következő létrehoz egy **ServiceBusService** hello használó objektum **ExponentialRetryPolicyFilter**:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a>Előfizetések létrehozása
Üzenettémakör-előfizetéseket is jönnek létre hello **ServiceBusService** objektum. Előfizetések vannak nevezve, és rendelkezhetnek olyan szűrőkkel, amely korlátozza a hello toohello előfizetés virtuális üzenetsorában kézbesített üzenetek készletét.

> [!NOTE]
> Előfizetések állandó, és amíg vagy azok továbbra is tooexist, vagy hello témakör azok tartoznak, a rendszer törli. Ha az alkalmazás logika toocreate előfizetés tartalmaz, akkor először győződjön meg arról hogy hello előfizetés már létezik-e a a `getSubscription` metódust.
>
>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Előfizetés létrehozása hello alapértelmezett (MatchAll) szűrővel
Hello **MatchAll** szűrő hello alapértelmezett szűrő, amely akkor használatos, ha nincs meghatározva szűrő egy új előfizetés létrehozásakor. Ha hello **MatchAll** szűrővel, minden üzenetek közzétett toohello témakör kerülnek, az előfizetés virtuális üzenetsorában. hello alábbi példa létrehoz egy "AllMessages" nevű előfizetést, és használja az alapértelmezett hello **MatchAll** szűrő.

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Előfizetések létrehozása szűrőkkel
Is létrehozhat, amelyek lehetővé teszik üzenetet küldött tooa témakör belül egy adott üzenettémakör-előfizetésben kell megjelenítése tooscope szűrők.

hello szűrő előfizetések által támogatott legrugalmasabb típusú van a **SqlFilter**, amely az SQL92 egy részhalmazát valósítja meg. Az SQL-szűrők hello tulajdonságait, amelyek a közzétett toohello témakör köszönőüzenetei működik. Az SQL-szűrőkkel használható hello kifejezések kapcsolatos további tudnivalókért tekintse meg a hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] szintaxist.

Szűrők felveheti tooa előfizetés hello segítségével `createRule` hello metódusában **ServiceBusService** objektum. Ez a módszer új szűrők tooan meglévő előfizetés hozzáadását teszi lehetővé.

> [!NOTE]
> Mert hello alapértelmezett szűrőhöz automatikusan tooall új előfizetésekre, először el kell távolítania hello alapértelmezett szűrő vagy a **MatchAll** felülírják más szűrőket is megadhat. Alapértelmezett szabály hello távolíthatja hello használatával `deleteRule` metódusában a **ServiceBusService** objektum.
>
>

hello alábbi példa egy nevű előfizetést hoz létre `HighMessages` rendelkező egy **SqlFilter** , amely csak választja ki, amelyek egyéni üzenetek `messagenumber` tulajdonságának értéke nagyobb, mint 3:

```javascript
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'HighMessages',
            'HighMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Ehhez hasonlóan hello alábbi példa egy nevű előfizetést hoz létre `LowMessages` rendelkező egy **SqlFilter** , amely csak rendelkező üzeneteket választja ki egy `messagenumber` tulajdonságának értéke kisebb vagy egyenlő, mint too3:

```javascript
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'LowMessages',
            'LowMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Ha egy most üzenettel túl`MyTopic`, azt mindig kézbesíti a rendszer a fogadók előfizetett toohello `AllMessages` üzenettémakör-előfizetésre, és szelektív módon kézbesíti az előfizetett tooreceivers toohello `HighMessages` és `LowMessages` üzenettémakör (hello üzenet tartalma függően).

## <a name="how-toosend-messages-tooa-topic"></a>Hogyan toosend üzenetek tooa témakör
egy üzenet tooa Service Bus-témakörbe toosend, az alkalmazást kell használnia a `sendTopicMessage` hello metódusában **ServiceBusService** objektum.
Üzenetek küldése tooService Bus-témaköröket **BrokeredMessage** objektumok.
**BrokeredMessage** objektumok rendelkeznek egy szabványos tulajdonságkészlettel (például `Label` és `TimeToLive`), amely használt toohold egyéni alkalmazásspecifikus tulajdonságokat, valamint egy karakterlánc típusú adatok törzsét. Az alkalmazás beállíthatja hello üzenet törzsét hello úgy, hogy egy karakterláncértéket hello `sendTopicMessage` és a szabványos tulajdonságait tölti fel a alapértelmezett értékek szerint szükséges.

hello következő példa bemutatja, hogyan toosend öt tesztüzenet `MyTopic`. Vegye figyelembe, hogy hello `messagenumber` egyes üzenetek tulajdonság értéke a hello iterációs hello hurok változik (Ez határozza meg, hogy melyik előfizetések fogják megkapni):

```javascript
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Service Bus-üzenettémakörök támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md). hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet. A témakörökben tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello, a témakörök által tárolt hello üzenetek teljes mérete. A témakör ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Üzenetek fogadása egy előfizetés
Üzenetek fogadása egy előfizetés használatával a `receiveSubscriptionMessage` hello metódusa **ServiceBusService** objektum. Alapértelmezés szerint az üzenetek törlődnek hello előfizetésből, olvasott; azonban (betekintés) olvashatja és üdvözlőüzenetére zárolása hello nem kötelező paraméter beállítása szerint a hello előfizetés törlése nélkül `isPeekLock` túl**igaz**.

hello alapértelmezett viselkedését olvasását és üdvözlőüzenetére törlése a fogadási művelet részeként hello legegyszerűbb modell, és működik a legjobban az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet hello esetre, ha nem található. toounderstand, ez egy olyan esetet, amelyben a fogyasztó problémák hello kérés fogadásához, és majd összeomlik a feldolgozása előtt. Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.

Ha hello `isPeekLock` paraméter értéke túl**igaz**, hello kap két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz. A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása.
Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második a fogadási folyamat meghívásával **deleteMessage** módszerről és az üzenet toobe biztosítása törölt paraméterként. Hello **deleteMessage** metódus lesz hello üzenetet feldolgozottként, és távolítsa el a hello előfizetés.

hello következő példa bemutatja, hogyan lehet üzeneteket fogadni, és a feldolgozott használatával `receiveSubscriptionMessage`. hello példa először a következő mintához hello "LowMessages" előfizetés törli az üzenetet, és egy üzenetet kap hello "HighMessages" előfizetés használatával `isPeekLock` tootrue beállítása. Majd törli a hello üzenet használatával `deleteMessage`:

```javascript
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hogyan toohandle omlik össze és nem olvasható üzenetek
Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít. Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlockMessage` metódust a **ServiceBusService** objektum. Ezzel a Service Bus toounlock hello előfizetésen belül az üzenet miatt és révén elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.

Is van az előfizetés lévő társított időtúllépés, és ha hello alkalmazás nem tooprocess üdvözlőüzenetére előtt hello zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus feloldja köszönőüzenetei automatikusan és lehetővé teszi az újbóli fogadását elérhető toobe.

A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `deleteMessage` metódust, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazás, amikor újraindul. Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet. Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését. Ez gyakran érhető el, használja a **MessageId** hello üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.

## <a name="delete-topics-and-subscriptions"></a>Témakörök és előfizetések törlése
Üzenettémák és előfizetések következetesek, és explicit módon kell törölni vagy hello [Azure-portálon] [ Azure portal] vagy programon keresztül.
hello következő példa bemutatja, hogyan toodelete hello témakör nevű `MyTopic`:

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

Egy témakör törlése hello témakörben regisztrált előfizetésekkel is törli. Az előfizetések független módon is törölhetők. A következő példa bemutatja, hogyan toodelete előfizetés nevű `HighMessages` a hello `MyTopic` témakör:

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Service Bus-üzenettémakörök hello alapjait, kövesse az alábbi hivatkozások további toolearn.

* Lásd: [üzenetsorok, témakörök és előfizetések][Queues, topics, and subscriptions].
* Az [SqlFilter][SqlFilter] API-referenciája.
* A Microsoft hello [Azure SDK for csomópont] [ Azure SDK for Node] GitHub tárházából.

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[létrehozása és központi telepítése egy Node.js-alkalmazás tooan Azure webhelyére]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
