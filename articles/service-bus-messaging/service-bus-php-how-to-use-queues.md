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
# <a name="how-toouse-service-bus-queues-with-php"></a><span data-ttu-id="b27bc-104">Hogyan toouse Service Bus várólisták a PHP</span><span class="sxs-lookup"><span data-stu-id="b27bc-104">How toouse Service Bus queues with PHP</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="b27bc-105">Ez az útmutató bemutatja, hogyan toouse Service Bus-üzenetsorok.</span><span class="sxs-lookup"><span data-stu-id="b27bc-105">This guide shows you how toouse Service Bus queues.</span></span> <span data-ttu-id="b27bc-106">hello minták PHP nyelven íródtak, és használja a hello [Azure SDK for PHP](../php-download-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="b27bc-106">hello samples are written in PHP and use hello [Azure SDK for PHP](../php-download-sdk.md).</span></span> <span data-ttu-id="b27bc-107">hello tárgyalt forgatókönyvekben szerepel a **üzenetsorok létrehozása**, **üzenetek küldése és fogadása**, és **várólisták törlése**.</span><span class="sxs-lookup"><span data-stu-id="b27bc-107">hello scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="b27bc-108">PHP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b27bc-108">Create a PHP application</span></span>
<span data-ttu-id="b27bc-109">egyetlen, aki hozzáfér hello Azure Blob szolgáltatáshoz PHP-alkalmazások létrehozására vonatkozó követelmény az hello hello hello osztályokat való hivatkozás [Azure SDK for PHP](../php-download-sdk.md) a a kód.</span><span class="sxs-lookup"><span data-stu-id="b27bc-109">hello only requirement for creating a PHP application that accesses hello Azure Blob service is hello referencing of classes in hello [Azure SDK for PHP](../php-download-sdk.md) from within your code.</span></span> <span data-ttu-id="b27bc-110">A fejlesztői eszközök toocreate is használhatja, az alkalmazás vagy a Jegyzettömb alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="b27bc-110">You can use any development tools toocreate your application, or Notepad.</span></span>

> [!NOTE]
> <span data-ttu-id="b27bc-111">A PHP-telepítés is rendelkeznie kell a hello [OpenSSL bővítmény](http://php.net/openssl) telepítve és engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="b27bc-111">Your PHP installation must also have hello [OpenSSL extension](http://php.net/openssl) installed and enabled.</span></span>
> 
> 

<span data-ttu-id="b27bc-112">Ez az útmutató hívható a PHP-alkalmazás helyileg, vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó szolgáltatás funkciók fogja használni.</span><span class="sxs-lookup"><span data-stu-id="b27bc-112">In this guide, you will use service features which can be called from within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="b27bc-113">Hello Azure ügyfélkódtárai beolvasása</span><span class="sxs-lookup"><span data-stu-id="b27bc-113">Get hello Azure client libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="b27bc-114">Az alkalmazás toouse Service Bus konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b27bc-114">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="b27bc-115">toouse hello Service Bus-üzenetsorba API-k, hello a következő:</span><span class="sxs-lookup"><span data-stu-id="b27bc-115">toouse hello Service Bus queue APIs, do hello following:</span></span>

1. <span data-ttu-id="b27bc-116">Hello robotjába referenciafájlban hello segítségével [require_once] [ require_once] utasítást.</span><span class="sxs-lookup"><span data-stu-id="b27bc-116">Reference hello autoloader file using hello [require_once][require_once] statement.</span></span>
2. <span data-ttu-id="b27bc-117">Bármely osztályok segítségével lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="b27bc-117">Reference any classes you might use.</span></span>

<span data-ttu-id="b27bc-118">hello következő példa bemutatja, hogyan tooinclude hello robotjába fájl- és referenciainformációkat hello `ServicesBuilder` osztály.</span><span class="sxs-lookup"><span data-stu-id="b27bc-118">hello following example shows how tooinclude hello autoloader file and reference hello `ServicesBuilder` class.</span></span>

> [!NOTE]
> <span data-ttu-id="b27bc-119">Ebben a példában (és más, a cikkben szereplő példák) azt feltételezi, hogy telepítette a hello PHP Klienskódtárak segítségével az Azure-szerkesztő használatával.</span><span class="sxs-lookup"><span data-stu-id="b27bc-119">This example (and other examples in this article) assumes you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="b27bc-120">Ha manuálisan vagy KÖRTEFÁK csomag hello szalagtárak telepítette, hivatkoznia hello **WindowsAzure.php** robotjába fájlt.</span><span class="sxs-lookup"><span data-stu-id="b27bc-120">If you installed hello libraries manually or as a PEAR package, you must reference hello **WindowsAzure.php** autoloader file.</span></span>
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="b27bc-121">Hello a következő példa a hello `require_once` utasítás mindig megjelennek, de csak hello osztályok hello példa tooexecute szükséges hivatkozott.</span><span class="sxs-lookup"><span data-stu-id="b27bc-121">In hello examples below, hello `require_once` statement will always be shown, but only hello classes necessary for hello example tooexecute are referenced.</span></span>

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="b27bc-122">A Service Bus-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="b27bc-122">Set up a Service Bus connection</span></span>
<span data-ttu-id="b27bc-123">tooinstantiate egy Service Bus-ügyfélalkalmazást, először rendelkeznie kell érvényes kapcsolati karakterláncot a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="b27bc-123">tooinstantiate a Service Bus client, you must first have a valid connection string in this format:</span></span>

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

<span data-ttu-id="b27bc-124">Ha `Endpoint` formátuma általában hello formátum `[yourNamespace].servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="b27bc-124">Where `Endpoint` is typically of hello format `[yourNamespace].servicebus.windows.net`.</span></span>

<span data-ttu-id="b27bc-125">toocreate bármely hello kell használnia az Azure szolgáltatás ügyfele `ServicesBuilder` osztály.</span><span class="sxs-lookup"><span data-stu-id="b27bc-125">toocreate any Azure service client you must use hello `ServicesBuilder` class.</span></span> <span data-ttu-id="b27bc-126">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="b27bc-126">You can:</span></span>

* <span data-ttu-id="b27bc-127">Hello kapcsolatot adjon át közvetlenül tooit karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b27bc-127">Pass hello connection string directly tooit.</span></span>
* <span data-ttu-id="b27bc-128">hello használata **CloudConfigurationManager (CCM)** toocheck több külső adatforrások hello kapcsolati karakterláncot kell megadnia:</span><span class="sxs-lookup"><span data-stu-id="b27bc-128">Use hello **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="b27bc-129">Alapértelmezés szerint azt egy külső adatforrás - környezeti változók támogatása tartalmaz</span><span class="sxs-lookup"><span data-stu-id="b27bc-129">By default it comes with support for one external source - environmental variables</span></span>
  * <span data-ttu-id="b27bc-130">Hozzáadhat új források hello kiterjesztésével `ConnectionStringSource` osztály</span><span class="sxs-lookup"><span data-stu-id="b27bc-130">You can add new sources by extending hello `ConnectionStringSource` class</span></span>

<span data-ttu-id="b27bc-131">Itt vázolt hello példákért hello kapcsolati karakterlánc lett átadva közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="b27bc-131">For hello examples outlined here, hello connection string is passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a><span data-ttu-id="b27bc-132">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="b27bc-132">Create a queue</span></span>
<span data-ttu-id="b27bc-133">A Service Bus-üzenetsorok használatával hello kezelési műveleteket hajthat végre `ServiceBusRestProxy` osztály.</span><span class="sxs-lookup"><span data-stu-id="b27bc-133">You can perform management operations for Service Bus queues via hello `ServiceBusRestProxy` class.</span></span> <span data-ttu-id="b27bc-134">A `ServiceBusRestProxy` objektum összeállított keresztül hello `ServicesBuilder::createServiceBusService` gyári módszer egy megfelelő kapcsolati karakterlánccal, amely magában foglalja a hello token engedélyek toomanage azt.</span><span class="sxs-lookup"><span data-stu-id="b27bc-134">A `ServiceBusRestProxy` object is constructed via hello `ServicesBuilder::createServiceBusService` factory method with an appropriate connection string that encapsulates hello token permissions toomanage it.</span></span>

<span data-ttu-id="b27bc-135">a következő példa azt mutatja meg hogyan hello tooinstantiate egy `ServiceBusRestProxy` , és hívja meg `ServiceBusRestProxy->createQueue` toocreate nevű várólista `myqueue` belül egy `MySBNamespace` szolgáltatásnévtér:</span><span class="sxs-lookup"><span data-stu-id="b27bc-135">hello following example shows how tooinstantiate a `ServiceBusRestProxy` and call `ServiceBusRestProxy->createQueue` toocreate a queue named `myqueue` within a `MySBNamespace` service namespace:</span></span>

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
> <span data-ttu-id="b27bc-136">Használhatja a hello `listQueues` metódusa `ServiceBusRestProxy` toocheck objektumokat, ha a várólista megadott névvel már létezik egy adott névtéren belül.</span><span class="sxs-lookup"><span data-stu-id="b27bc-136">You can use hello `listQueues` method on `ServiceBusRestProxy` objects toocheck if a queue with a specified name already exists within a namespace.</span></span>
> 
> 

## <a name="send-messages-tooa-queue"></a><span data-ttu-id="b27bc-137">Üzenetek tooa várólista küldése</span><span class="sxs-lookup"><span data-stu-id="b27bc-137">Send messages tooa queue</span></span>
<span data-ttu-id="b27bc-138">toosend üzenet tooa Service Bus-üzenetsorba, az alkalmazás meghívja hello `ServiceBusRestProxy->sendQueueMessage` metódust.</span><span class="sxs-lookup"><span data-stu-id="b27bc-138">toosend a message tooa Service Bus queue, your application calls hello `ServiceBusRestProxy->sendQueueMessage` method.</span></span> <span data-ttu-id="b27bc-139">a következő kód bemutatja hogyan hello toosend egy üzenet toohello `myqueue` várólista korábban jött létre a `MySBNamespace` szolgáltatás névtere.</span><span class="sxs-lookup"><span data-stu-id="b27bc-139">hello following code shows how toosend a message toohello `myqueue` queue previously created within the `MySBNamespace` service namespace.</span></span>

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

<span data-ttu-id="b27bc-140">Üzenetek túl elküldött (és a fogadott) Service Bus-üzenetsorok hello példánya [BrokeredMessage] [ BrokeredMessage] osztály.</span><span class="sxs-lookup"><span data-stu-id="b27bc-140">Messages sent too(and received from ) Service Bus queues are instances of hello [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="b27bc-141">[BrokeredMessage] [ BrokeredMessage] objektumok szabványos módszerek és tulajdonságait, amelyek használt toohold egyéni alkalmazásspecifikus tulajdonságokat, és egy tetszőleges alkalmazásadatokból álló törzzsel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="b27bc-141">[BrokeredMessage][BrokeredMessage] objects have a set of standard methods and properties that are used toohold custom application-specific properties, and a body of arbitrary application data.</span></span>

<span data-ttu-id="b27bc-142">Service Bus-üzenetsorok támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="b27bc-142">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="b27bc-143">hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="b27bc-143">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="b27bc-144">Az üzenetsorban tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello üzenetsor által tárolt hello üzenetek teljes mérete.</span><span class="sxs-lookup"><span data-stu-id="b27bc-144">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="b27bc-145">A várólista hossza a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="b27bc-145">This upper limit on queue size is 5 GB.</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="b27bc-146">Üzenetek fogadása egy üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="b27bc-146">Receive messages from a queue</span></span>

<span data-ttu-id="b27bc-147">hello legjobb módja tooreceive üzenetek várólistából való várólistából toouse egy `ServiceBusRestProxy->receiveQueueMessage` metódust.</span><span class="sxs-lookup"><span data-stu-id="b27bc-147">hello best way tooreceive messages from a queue is toouse a `ServiceBusRestProxy->receiveQueueMessage` method.</span></span> <span data-ttu-id="b27bc-148">Két különböző módban a lehet üzeneteket fogadni: [ *ReceiveAndDelete* ](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) és [ *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock).</span><span class="sxs-lookup"><span data-stu-id="b27bc-148">Messages can be received in two different modes: [*ReceiveAndDelete*](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) and [*PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock).</span></span> <span data-ttu-id="b27bc-149">**PeekLock** hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="b27bc-149">**PeekLock** is hello default.</span></span>

<span data-ttu-id="b27bc-150">Használata esetén [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mód, kap egy egylépéses művelet; Ez azt jelenti, hogy amikor a Service Bus egy olvasási kérést kap a sorhoz, azt hello üzenetet, feldolgozottként jelöli meg és toohello alkalmazás visszaadja.</span><span class="sxs-lookup"><span data-stu-id="b27bc-150">When using [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mode, receive is a single-shot operation; that is, when Service Bus receives a read request for a message in a queue, it marks hello message as being consumed and returns it toohello application.</span></span> <span data-ttu-id="b27bc-151">[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mód hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban.</span><span class="sxs-lookup"><span data-stu-id="b27bc-151">[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mode is hello simplest model and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="b27bc-152">toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="b27bc-152">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="b27bc-153">Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.</span><span class="sxs-lookup"><span data-stu-id="b27bc-153">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="b27bc-154">Hello alapértelmezett [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) mód, egy üzenet fogadását két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz.</span><span class="sxs-lookup"><span data-stu-id="b27bc-154">In hello default [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) mode, receiving a message becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="b27bc-155">A Service Bus kérelmet kap, ha azt hello felhasznált tovább üzenet toobe talál, tooprevent zárolja, fogadjon más felhasználói és toohello alkalmazás visszaadja.</span><span class="sxs-lookup"><span data-stu-id="b27bc-155">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers from receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="b27bc-156">Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata hello fogadott üzenet túl átadásával`ServiceBusRestProxy->deleteMessage`.</span><span class="sxs-lookup"><span data-stu-id="b27bc-156">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by passing hello received message too`ServiceBusRestProxy->deleteMessage`.</span></span> <span data-ttu-id="b27bc-157">Amikor a Service Bus látja a hello `deleteMessage` hívás, azt feldolgozottként hello üzenetet, és távolítsa el azt a hello várólista.</span><span class="sxs-lookup"><span data-stu-id="b27bc-157">When Service Bus sees hello `deleteMessage` call, it will mark hello message as being consumed and remove it from hello queue.</span></span>

<span data-ttu-id="b27bc-158">a következő példa azt mutatja meg hogyan hello tooreceive és feldolgozni egy üzenetet használatával [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) hello alapértelmezett üzemmódra.</span><span class="sxs-lookup"><span data-stu-id="b27bc-158">hello following example shows how tooreceive and process a message using [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) mode (hello default mode).</span></span>

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="b27bc-159">Hogyan toohandle omlik össze és nem olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="b27bc-159">How toohandle application crashes and unreadable messages</span></span>

<span data-ttu-id="b27bc-160">Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít.</span><span class="sxs-lookup"><span data-stu-id="b27bc-160">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="b27bc-161">Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlockMessage` metódust a fogadott üdvözlőüzenetére (helyett hello `deleteMessage` metódus).</span><span class="sxs-lookup"><span data-stu-id="b27bc-161">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlockMessage` method on hello received message (instead of hello `deleteMessage` method).</span></span> <span data-ttu-id="b27bc-162">Ezzel a Service Bus toounlock hello üzenet hello várólistában és révén elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.</span><span class="sxs-lookup"><span data-stu-id="b27bc-162">This will cause Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="b27bc-163">Emellett van hello várólistában lévő társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess üdvözlőüzenetére előtt hello zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus feloldást köszönőüzenetei automatikusan, és teszi elérhetővé toobe újbóli fogadását.</span><span class="sxs-lookup"><span data-stu-id="b27bc-163">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="b27bc-164">A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `deleteMessage` kérés kiadása, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazást, amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="b27bc-164">In hello event that hello application crashes after processing hello message but before hello `deleteMessage` request is issued, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="b27bc-165">Ezt gyakran nevezik *legalább egyszeri* feldolgozása; Ez azt jelenti, hogy minden üzenet legalább egyszer fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet.</span><span class="sxs-lookup"><span data-stu-id="b27bc-165">This is often called *At Least Once* processing; that is, each message is processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="b27bc-166">Ha duplikált feldolgozása hello forgatókönyvben nem lehetségesek, akkor további logikát tooapplications toohandle ismétlődő üzenetkézbesítést hozzáadása ajánlott.</span><span class="sxs-lookup"><span data-stu-id="b27bc-166">If hello scenario cannot tolerate duplicate processing, then adding additional logic tooapplications toohandle duplicate message delivery is recommended.</span></span> <span data-ttu-id="b27bc-167">Ez gyakran érhető el hello segítségével `getMessageId` metódus hello üzenet, amely állandó marad a kézbesítési kísérletek során.</span><span class="sxs-lookup"><span data-stu-id="b27bc-167">This is often achieved using hello `getMessageId` method of hello message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b27bc-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b27bc-168">Next steps</span></span>
<span data-ttu-id="b27bc-169">Most, hogy megismerte a Service Bus-üzenetsorok alapjait hello, lásd: [üzenetsorok, témakörök és előfizetések] [ Queues, topics, and subscriptions] további információt.</span><span class="sxs-lookup"><span data-stu-id="b27bc-169">Now that you've learned hello basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

<span data-ttu-id="b27bc-170">További információ is a Microsoft hello [PHP fejlesztői központ](https://azure.microsoft.com/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="b27bc-170">For more information, also visit hello [PHP Developer Center](https://azure.microsoft.com/develop/php/).</span></span>

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once


