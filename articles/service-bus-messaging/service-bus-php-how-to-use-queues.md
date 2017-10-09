---
title: "a PHP várólisták aaaHow toouse Service Bus |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Service Bus várólisták az Azure-ban. Kódminták PHP."
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e29c829b-44c5-4350-8f2e-39e0c380a9f2
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 8cf233176029b679d172eaf713632087beca5e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-php"></a>Hogyan toouse Service Bus várólisták a PHP
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ez az útmutató bemutatja, hogyan toouse Service Bus-üzenetsorok. hello minták PHP nyelven íródtak, és használja a hello [Azure SDK for PHP](../php-download-sdk.md). hello tárgyalt forgatókönyvekben szerepel a **üzenetsorok létrehozása**, **üzenetek küldése és fogadása**, és **várólisták törlése**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>PHP-alkalmazás létrehozása
egyetlen, aki hozzáfér hello Azure Blob szolgáltatáshoz PHP-alkalmazások létrehozására vonatkozó követelmény az hello hello hello osztályokat való hivatkozás [Azure SDK for PHP](../php-download-sdk.md) a a kód. A fejlesztői eszközök toocreate is használhatja, az alkalmazás vagy a Jegyzettömb alkalmazásba.

> [!NOTE]
> A PHP-telepítés is rendelkeznie kell a hello [OpenSSL bővítmény](http://php.net/openssl) telepítve és engedélyezve van.
> 
> 

Ez az útmutató hívható a PHP-alkalmazás helyileg, vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó szolgáltatás funkciók fogja használni.

## <a name="get-hello-azure-client-libraries"></a>Hello Azure ügyfélkódtárai beolvasása
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-toouse-service-bus"></a>Az alkalmazás toouse Service Bus konfigurálása
toouse hello Service Bus-üzenetsorba API-k, hello a következő:

1. Hello robotjába referenciafájlban hello segítségével [require_once] [ require_once] utasítást.
2. Bármely osztályok segítségével lehet hivatkozni.

hello következő példa bemutatja, hogyan tooinclude hello robotjába fájl- és referenciainformációkat hello `ServicesBuilder` osztály.

> [!NOTE]
> Ebben a példában (és más, a cikkben szereplő példák) azt feltételezi, hogy telepítette a hello PHP Klienskódtárak segítségével az Azure-szerkesztő használatával. Ha manuálisan vagy KÖRTEFÁK csomag hello szalagtárak telepítette, hivatkoznia hello **WindowsAzure.php** robotjába fájlt.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Hello a következő példa a hello `require_once` utasítás mindig megjelennek, de csak hello osztályok hello példa tooexecute szükséges hivatkozott.

## <a name="set-up-a-service-bus-connection"></a>A Service Bus-kapcsolat beállítása
tooinstantiate egy Service Bus-ügyfélalkalmazást, először rendelkeznie kell érvényes kapcsolati karakterláncot a következő formátumban:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Ha `Endpoint` formátuma általában hello formátum `[yourNamespace].servicebus.windows.net`.

toocreate bármely hello kell használnia az Azure szolgáltatás ügyfele `ServicesBuilder` osztály. A következőket teheti:

* Hello kapcsolatot adjon át közvetlenül tooit karakterlánc.
* hello használata **CloudConfigurationManager (CCM)** toocheck több külső adatforrások hello kapcsolati karakterláncot kell megadnia:
  * Alapértelmezés szerint azt egy külső adatforrás - környezeti változók támogatása tartalmaz
  * Hozzáadhat új források hello kiterjesztésével `ConnectionStringSource` osztály

Itt vázolt hello példákért hello kapcsolati karakterlánc lett átadva közvetlenül.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a>Üzenetsor létrehozása
A Service Bus-üzenetsorok használatával hello kezelési műveleteket hajthat végre `ServiceBusRestProxy` osztály. A `ServiceBusRestProxy` objektum összeállított keresztül hello `ServicesBuilder::createServiceBusService` gyári módszer egy megfelelő kapcsolati karakterlánccal, amely magában foglalja a hello token engedélyek toomanage azt.

a következő példa azt mutatja meg hogyan hello tooinstantiate egy `ServiceBusRestProxy` , és hívja meg `ServiceBusRestProxy->createQueue` toocreate nevű várólista `myqueue` belül egy `MySBNamespace` szolgáltatásnévtér:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    $queueInfo = new QueueInfo("myqueue");

    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
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
> Használhatja a hello `listQueues` metódusa `ServiceBusRestProxy` toocheck objektumokat, ha a várólista megadott névvel már létezik egy adott névtéren belül.
> 
> 

## <a name="send-messages-tooa-queue"></a>Üzenetek tooa várólista küldése
toosend üzenet tooa Service Bus-üzenetsorba, az alkalmazás meghívja hello `ServiceBusRestProxy->sendQueueMessage` metódust. a következő kód bemutatja hogyan hello toosend egy üzenet toohello `myqueue` várólista korábban jött létre a `MySBNamespace` szolgáltatás névtere.

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
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
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

Üzenetek túl elküldött (és a fogadott) Service Bus-üzenetsorok hello példánya [BrokeredMessage] [ BrokeredMessage] osztály. [BrokeredMessage] [ BrokeredMessage] objektumok szabványos módszerek és tulajdonságait, amelyek használt toohold egyéni alkalmazásspecifikus tulajdonságokat, és egy tetszőleges alkalmazásadatokból álló törzzsel rendelkeznek.

Service Bus-üzenetsorok támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md). hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet. Az üzenetsorban tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello üzenetsor által tárolt hello üzenetek teljes mérete. A várólista hossza a felső korlátja 5 GB.

## <a name="receive-messages-from-a-queue"></a>Üzenetek fogadása egy üzenetsorból

hello legjobb módja tooreceive üzenetek várólistából való várólistából toouse egy `ServiceBusRestProxy->receiveQueueMessage` metódust. Két különböző módban a lehet üzeneteket fogadni: [ *ReceiveAndDelete* ](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) és [ *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock). **PeekLock** hello alapértelmezett.

Használata esetén [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mód, kap egy egylépéses művelet; Ez azt jelenti, hogy amikor a Service Bus egy olvasási kérést kap a sorhoz, azt hello üzenetet, feldolgozottként jelöli meg és toohello alkalmazás visszaadja. [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mód hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban. toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt. Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.

Hello alapértelmezett [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) mód, egy üzenet fogadását két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz. A Service Bus kérelmet kap, ha azt hello felhasznált tovább üzenet toobe talál, tooprevent zárolja, fogadjon más felhasználói és toohello alkalmazás visszaadja. Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata hello fogadott üzenet túl átadásával`ServiceBusRestProxy->deleteMessage`. Amikor a Service Bus látja a hello `deleteMessage` hívás, azt feldolgozottként hello üzenetet, és távolítsa el azt a hello várólista.

a következő példa azt mutatja meg hogyan hello tooreceive és feldolgozni egy üzenetet használatával [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) hello alapértelmezett üzemmódra.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set hello receive mode tooPeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hogyan toohandle omlik össze és nem olvasható üzenetek

Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít. Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlockMessage` metódust a fogadott üdvözlőüzenetére (helyett hello `deleteMessage` metódus). Ezzel a Service Bus toounlock hello üzenet hello várólistában és révén elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.

Emellett van hello várólistában lévő társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess üdvözlőüzenetére előtt hello zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus feloldást köszönőüzenetei automatikusan, és teszi elérhetővé toobe újbóli fogadását.

A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `deleteMessage` kérés kiadása, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazást, amikor újraindul. Ezt gyakran nevezik *legalább egyszeri* feldolgozása; Ez azt jelenti, hogy minden üzenet legalább egyszer fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet. Ha duplikált feldolgozása hello forgatókönyvben nem lehetségesek, akkor további logikát tooapplications toohandle ismétlődő üzenetkézbesítést hozzáadása ajánlott. Ez gyakran érhető el hello segítségével `getMessageId` metódus hello üzenet, amely állandó marad a kézbesítési kísérletek során.

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Service Bus-üzenetsorok alapjait hello, lásd: [üzenetsorok, témakörök és előfizetések] [ Queues, topics, and subscriptions] további információt.

További információ is a Microsoft hello [PHP fejlesztői központ](https://azure.microsoft.com/develop/php/).

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once


