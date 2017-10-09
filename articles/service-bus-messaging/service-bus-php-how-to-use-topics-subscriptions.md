---
title: "aaaHow toouse Service Bus-üzenettémakörök a PHP |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Service Bus-üzenettémakörök a PHP az Azure-ban."
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: faaa4bbd-f6ef-42ff-aca7-fc4353976449
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/27/2017
ms.author: sethm
ms.openlocfilehash: 0ca8625fa3edc5854c0d6c1c2f6adab6a2d42f91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-php"></a>Hogyan toouse Service Bus üzenettémák és előfizetések a PHP

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ez a cikk bemutatja, hogyan toouse Service Bus üzenettémák és előfizetések. hello minták PHP nyelven íródtak, és használja a hello [Azure SDK for PHP](../php-download-sdk.md). hello tárgyalt forgatókönyvekben szerepel a **üzenettémák és előfizetések létrehozása**, **előfizetés-szűrők létrehozása**, **tooa témakör üzenetek küldésekor**, **fogadása előfizetés üzeneteit**, és **üzenettémák és előfizetések törlése**.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>PHP-alkalmazás létrehozása
egyetlen, aki hozzáfér hello Azure Blob szolgáltatáshoz PHP-alkalmazások létrehozására vonatkozó követelmény az hello tooreference osztályokat hello [Azure SDK for PHP](../php-download-sdk.md) a a kód. A fejlesztői eszközök toocreate is használhatja, az alkalmazás vagy a Jegyzettömb alkalmazásba.

> [!NOTE]
> A PHP-telepítés is rendelkeznie kell a hello [OpenSSL bővítmény](http://php.net/openssl) telepítve és engedélyezve van.
> 
> 

Ez a cikk ismerteti, hogyan toouse szolgáltatás funkciókat, amelyek a PHP-alkalmazás helyileg, vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó kód hívása.

## <a name="get-hello-azure-client-libraries"></a>Hello Azure ügyfélkódtárai beolvasása
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-toouse-service-bus"></a>Az alkalmazás toouse Service Bus konfigurálása
toouse hello Service Bus API-kat:

1. Hello robotjába referenciafájlban hello segítségével [require_once] [ require-once] utasítást.
2. Bármely osztályok segítségével lehet hivatkozni.

hello következő példa bemutatja, hogyan tooinclude hello robotjába fájl- és referenciainformációkat hello **ServiceBusService** osztály.

> [!NOTE]
> Ebben a példában (és más, a cikkben szereplő példák) azt feltételezi, hogy telepítette a hello PHP Klienskódtárak segítségével az Azure-szerkesztő használatával. Ha manuálisan vagy KÖRTEFÁK csomag hello szalagtárak telepítette, hivatkoznia hello **WindowsAzure.php** robotjába fájlt.
> 
> 

```php
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

A következő példák hello, hello `require_once` utasítás mindig megjelennek, de csak hello osztályok hello példa tooexecute szükséges hivatkozott.

## <a name="set-up-a-service-bus-connection"></a>A Service Bus-kapcsolat beállítása
tooinstantiate egy Service Bus-ügyfélalkalmazást, előbb rendelkezik érvényes kapcsolati karakterláncot a következő formátumban:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Ha `Endpoint` formátuma általában hello formátum `https://[yourNamespace].servicebus.windows.net`.

toocreate bármely hello kell használnia az Azure szolgáltatás ügyfele `ServicesBuilder` osztály. A következőket teheti:

* Hello kapcsolatot adjon át közvetlenül tooit karakterlánc.
* hello használata **CloudConfigurationManager (CCM)** toocheck több külső adatforrások hello kapcsolati karakterláncot kell megadnia:
  * Alapértelmezés szerint az tartalmaz egy külső adatforrás - környezeti változók támogatása.
  * Hozzáadhat új források hello kiterjesztésével `ConnectionStringSource` osztály.

Itt vázolt hello példákért hello kapcsolati karakterlánc lett átadva közvetlenül.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Üzenettémakör létrehozása
A Service Bus-üzenettémakörök hello használatával kezelési műveleteket hajthat végre `ServiceBusRestProxy` osztály. A `ServiceBusRestProxy` objektum összeállított keresztül hello `ServicesBuilder::createServiceBusService` gyári módszer egy megfelelő kapcsolati karakterlánccal, amely magában foglalja a hello token engedélyek toomanage azt.

a következő példa azt mutatja meg hogyan hello tooinstantiate egy `ServiceBusRestProxy` , és hívja meg `ServiceBusRestProxy->createTopic` toocreate nevű témakör `mytopic` belül egy `MySBNamespace` névtér:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> Használhatja a hello `listTopics` metódusa `ServiceBusRestProxy` toocheck objektumokat, ha a témakör a megadott névvel már létezik egy szolgáltatásnévtérben.
> 
> 

## <a name="create-a-subscription"></a>Előfizetés létrehozása
Üzenettémakör-előfizetéseket is jönnek létre hello `ServiceBusRestProxy->createSubscription` metódust. Előfizetések vannak nevezve, és rendelkezhetnek olyan szűrőkkel, amelyek toohello előfizetés virtuális üzenetsorának átadott üzenetek készletét hello korlátozza.

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Előfizetés létrehozása hello alapértelmezett (MatchAll) szűrővel
Hello **MatchAll** szűrő hello alapértelmezett szűrő, amely akkor használatos, ha nincs meghatározva szűrő egy új előfizetés létrehozásakor. Ha hello **MatchAll** szűrővel, minden üzenetek közzétett toohello témakör kerülnek hello előfizetés virtuális üzenetsorában. hello alábbi példakód létrehozza a "mysubscription" nevű előfizetést, és használja az alapértelmezett hello **MatchAll** szűrő.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Előfizetések létrehozása szűrőkkel
Is beállíthat szűrőket, amelyek lehetővé teszik a toospecify üzenetet küldött tooa témakör belül egy adott üzenettémakör-előfizetésben kell megjelennie. hello szűrő előfizetések által támogatott legrugalmasabb típusú hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), amely az SQL92 egy részhalmazát valósítja meg. Az SQL-szűrők hello tulajdonságait, amelyek a közzétett toohello témakör köszönőüzenetei működik. SqlFilters kapcsolatos további információkért lásd: [SqlFilter.SqlExpression tulajdonság][sqlfilter].

> [!NOTE]
> Minden egyes szabály egy előfizetésben dolgozza fel a bejövő üzenetek egymástól függetlenül, az eredmény üzenetek toohello előfizetés hozzáadása. Emellett minden egyes új előfizetés rendelkezik egy alapértelmezett **szabály** hello témakör toohello előfizetésből minden üzenetet hozzáadó szűrő objektum. tooreceive csak üzenetek a szűrőnek megfelelő, el kell távolítania hello alapértelmezett szabály. Alapértelmezett szabály hello távolíthatja hello használatával `ServiceBusRestProxy->deleteRule` metódust.
> 
> 

hello alábbi példa egy nevű előfizetést hoz létre `HighMessages` rendelkező egy **SqlFilter** , amely csak választja ki, amelyek egyéni üzenetek `MessageNumber` 3-nál nagyobb tulajdonság. Lásd: [küldési üzenetek tooa témakör](#send-messages-to-a-topic) egyéni tulajdonságok toomessages hozzáadásával kapcsolatos információkat.

```php
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Vegye figyelembe, hogy ez a kód egy további névtér hello használatát igényli: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Ehhez hasonlóan hello alábbi példa egy nevű előfizetést hoz létre `LowMessages` rendelkező egy `SqlFilter` , amely csak rendelkező üzeneteket választja ki egy `MessageNumber` tulajdonságának értéke kisebb vagy egyenlő, mint too3.

```php
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Mostantól, ha egy üzenetet kap toohello `mytopic` témakör, az előfizetett tooreceivers toohello mindig biztosítását `mysubscription` előfizetés, és szelektív módon kézbesíti az előfizetett tooreceivers toohello `HighMessages` és `LowMessages` előfizetések () attól függően, hello üzenettartalom).

## <a name="send-messages-tooa-topic"></a>Üzenetek tooa témakör küldése
toosend üzenet tooa Service Bus-témakörbe, az alkalmazás meghívja hello `ServiceBusRestProxy->sendTopicMessage` metódust. a következő kód bemutatja hogyan hello toosend egy üzenet toohello `mytopic` korábban jött létre a témakör a `MySBNamespace` szolgáltatás névtere.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Üzenetek küldése tooService Bus-üzenettémakörök hello példánya [BrokeredMessage] [ BrokeredMessage] osztály. [BrokeredMessage] [ BrokeredMessage] objektumok rendelkeznek szokásos tulajdonságok és módszerek, valamint a használt toohold egyéni alkalmazásspecifikus tulajdonságokat lehet tulajdonságokat. hello következő példa bemutatja, hogyan toosend 5 teszt üzenetek toohello `mytopic` a témakörben korábban hozott létre. Hello `setProperty` metódus használt tooadd egyéni tulajdonságot (`MessageNumber`) tooeach üzenet. Vegye figyelembe, hogy hello `MessageNumber` tulajdonság értéke változik a minden üzenet (hello ismertetett módon használhatja a érték toodetermine melyik előfizetések fogják megkapni, [előfizetés létrehozása](#create-a-subscription) szakaszt):

```php
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);

    // Set custom property.
    $message->setProperty("MessageNumber", $i);

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Service Bus-üzenettémakörök támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md). hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet. A témakörökben tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello, a témakörök által tárolt hello üzenetek teljes mérete. Ez a témakör mérete felső korlátja 5 GB. Kvóták kapcsolatos további információkért lásd: [Service Bus kvóták][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Üzenetek fogadása egy előfizetés
hello legjobb módja tooreceive üzenetek előfizetésből toouse egy `ServiceBusRestProxy->receiveSubscriptionMessage` metódust. Két különböző módban a lehet üzeneteket fogadni: [ *ReceiveAndDelete* és *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode). **PeekLock** hello alapértelmezett.

Hello használatakor [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mód, kap egy egylépéses művelet; Ez azt jelenti, hogy amikor a Service Bus egy olvasási kérést kap egy előfizetésben, akkor hello üzenetet, feldolgozottként jelöli meg és visszaadja toohello az alkalmazás. [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * mód hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban. toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt. Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.

Hello alapértelmezett [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mód, egy üzenet fogadását két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz. A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása. Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata hello fogadott üzenet túl átadásával`ServiceBusRestProxy->deleteMessage`. Amikor a Service Bus látja a hello `deleteMessage` hívás, azt feldolgozottként hello üzenetet, és távolítsa el azt a hello várólista.

a következő példa azt mutatja meg hogyan hello tooreceive és feldolgozni egy üzenetet használatával [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) hello alapértelmezett üzemmódra. 

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set receive mode tooPeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Hogyan: alkalmazás-összeomlások és nem olvasható üzenetek kezelése
Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít. Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlockMessage` metódust a fogadott üdvözlőüzenetére (helyett hello `deleteMessage` metódus). Ezzel a Service Bus toounlock hello üzenet hello várólistában és révén elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.

Emellett van hello várólistában lévő társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess üdvözlőüzenetére előtt hello zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus feloldást köszönőüzenetei automatikusan, és teszi elérhetővé toobe újbóli fogadását.

A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `deleteMessage` kérés kiadása, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazást, amikor újraindul. Ezt gyakran nevezik *legalább egyszeri* feldolgozása; Ez azt jelenti, hogy minden üzenet legalább egyszer fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet. Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tooapplications toohandle ismétlődő üzenetkézbesítését. Ez gyakran érhető el hello segítségével `getMessageId` metódus hello üzenet, amely állandó marad a kézbesítési kísérletek során.

## <a name="delete-topics-and-subscriptions"></a>Témakörök és előfizetések törlése
a témakör és előfizetés, használjon hello toodelete `ServiceBusRestProxy->deleteTopic` vagy hello `ServiceBusRestProxy->deleteSubscripton` módszerek, illetve. Vegye figyelembe, hogy egy témakör törlése összes előfizetést is törli hello témakörben regisztrált.

hello következő példa bemutatja, hogyan toodelete témakör nevű `mytopic` és a regisztrált előfizetések.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Hello segítségével `deleteSubscription` módszer, egymástól függetlenül előfizetés törlése:

```php
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Service Bus-üzenetsorok alapjait hello, lásd: [üzenetsorok, témakörök és előfizetések] [ Queues, topics, and subscriptions] további információt.

[BrokeredMessage]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter
[require-once]: http://php.net/require_once
[Service Bus quotas]: service-bus-quotas.md
