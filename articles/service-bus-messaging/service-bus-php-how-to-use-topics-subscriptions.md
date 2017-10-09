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
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-php"></a><span data-ttu-id="26ac8-103">Hogyan toouse Service Bus üzenettémák és előfizetések a PHP</span><span class="sxs-lookup"><span data-stu-id="26ac8-103">How toouse Service Bus topics and subscriptions with PHP</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="26ac8-104">Ez a cikk bemutatja, hogyan toouse Service Bus üzenettémák és előfizetések.</span><span class="sxs-lookup"><span data-stu-id="26ac8-104">This article shows you how toouse Service Bus topics and subscriptions.</span></span> <span data-ttu-id="26ac8-105">hello minták PHP nyelven íródtak, és használja a hello [Azure SDK for PHP](../php-download-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="26ac8-105">hello samples are written in PHP and use hello [Azure SDK for PHP](../php-download-sdk.md).</span></span> <span data-ttu-id="26ac8-106">hello tárgyalt forgatókönyvekben szerepel a **üzenettémák és előfizetések létrehozása**, **előfizetés-szűrők létrehozása**, **tooa témakör üzenetek küldésekor**, **fogadása előfizetés üzeneteit**, és **üzenettémák és előfizetések törlése**.</span><span class="sxs-lookup"><span data-stu-id="26ac8-106">hello scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages tooa topic**, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="26ac8-107">PHP-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="26ac8-107">Create a PHP application</span></span>
<span data-ttu-id="26ac8-108">egyetlen, aki hozzáfér hello Azure Blob szolgáltatáshoz PHP-alkalmazások létrehozására vonatkozó követelmény az hello tooreference osztályokat hello [Azure SDK for PHP](../php-download-sdk.md) a a kód.</span><span class="sxs-lookup"><span data-stu-id="26ac8-108">hello only requirement for creating a PHP application that accesses hello Azure Blob service is tooreference classes in hello [Azure SDK for PHP](../php-download-sdk.md) from within your code.</span></span> <span data-ttu-id="26ac8-109">A fejlesztői eszközök toocreate is használhatja, az alkalmazás vagy a Jegyzettömb alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="26ac8-109">You can use any development tools toocreate your application, or Notepad.</span></span>

> [!NOTE]
> <span data-ttu-id="26ac8-110">A PHP-telepítés is rendelkeznie kell a hello [OpenSSL bővítmény](http://php.net/openssl) telepítve és engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="26ac8-110">Your PHP installation must also have hello [OpenSSL extension](http://php.net/openssl) installed and enabled.</span></span>
> 
> 

<span data-ttu-id="26ac8-111">Ez a cikk ismerteti, hogyan toouse szolgáltatás funkciókat, amelyek a PHP-alkalmazás helyileg, vagy egy Azure webes szerepkörről, a feldolgozói szerepkör vagy a webhely belül futó kód hívása.</span><span class="sxs-lookup"><span data-stu-id="26ac8-111">This article describes how toouse service features that can be called within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="26ac8-112">Hello Azure ügyfélkódtárai beolvasása</span><span class="sxs-lookup"><span data-stu-id="26ac8-112">Get hello Azure client libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="26ac8-113">Az alkalmazás toouse Service Bus konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26ac8-113">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="26ac8-114">toouse hello Service Bus API-kat:</span><span class="sxs-lookup"><span data-stu-id="26ac8-114">toouse hello Service Bus APIs:</span></span>

1. <span data-ttu-id="26ac8-115">Hello robotjába referenciafájlban hello segítségével [require_once] [ require-once] utasítást.</span><span class="sxs-lookup"><span data-stu-id="26ac8-115">Reference hello autoloader file using hello [require_once][require-once] statement.</span></span>
2. <span data-ttu-id="26ac8-116">Bármely osztályok segítségével lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="26ac8-116">Reference any classes you might use.</span></span>

<span data-ttu-id="26ac8-117">hello következő példa bemutatja, hogyan tooinclude hello robotjába fájl- és referenciainformációkat hello **ServiceBusService** osztály.</span><span class="sxs-lookup"><span data-stu-id="26ac8-117">hello following example shows how tooinclude hello autoloader file and reference hello **ServiceBusService** class.</span></span>

> [!NOTE]
> <span data-ttu-id="26ac8-118">Ebben a példában (és más, a cikkben szereplő példák) azt feltételezi, hogy telepítette a hello PHP Klienskódtárak segítségével az Azure-szerkesztő használatával.</span><span class="sxs-lookup"><span data-stu-id="26ac8-118">This example (and other examples in this article) assumes you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="26ac8-119">Ha manuálisan vagy KÖRTEFÁK csomag hello szalagtárak telepítette, hivatkoznia hello **WindowsAzure.php** robotjába fájlt.</span><span class="sxs-lookup"><span data-stu-id="26ac8-119">If you installed hello libraries manually or as a PEAR package, you must reference hello **WindowsAzure.php** autoloader file.</span></span>
> 
> 

```php
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="26ac8-120">A következő példák hello, hello `require_once` utasítás mindig megjelennek, de csak hello osztályok hello példa tooexecute szükséges hivatkozott.</span><span class="sxs-lookup"><span data-stu-id="26ac8-120">In hello following examples, hello `require_once` statement will always be shown, but only hello classes necessary for hello example tooexecute are referenced.</span></span>

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="26ac8-121">A Service Bus-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="26ac8-121">Set up a Service Bus connection</span></span>
<span data-ttu-id="26ac8-122">tooinstantiate egy Service Bus-ügyfélalkalmazást, előbb rendelkezik érvényes kapcsolati karakterláncot a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="26ac8-122">tooinstantiate a Service Bus client you must first have a valid connection string in this format:</span></span>

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

<span data-ttu-id="26ac8-123">Ha `Endpoint` formátuma általában hello formátum `https://[yourNamespace].servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="26ac8-123">Where `Endpoint` is typically of hello format `https://[yourNamespace].servicebus.windows.net`.</span></span>

<span data-ttu-id="26ac8-124">toocreate bármely hello kell használnia az Azure szolgáltatás ügyfele `ServicesBuilder` osztály.</span><span class="sxs-lookup"><span data-stu-id="26ac8-124">toocreate any Azure service client you must use hello `ServicesBuilder` class.</span></span> <span data-ttu-id="26ac8-125">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="26ac8-125">You can:</span></span>

* <span data-ttu-id="26ac8-126">Hello kapcsolatot adjon át közvetlenül tooit karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="26ac8-126">Pass hello connection string directly tooit.</span></span>
* <span data-ttu-id="26ac8-127">hello használata **CloudConfigurationManager (CCM)** toocheck több külső adatforrások hello kapcsolati karakterláncot kell megadnia:</span><span class="sxs-lookup"><span data-stu-id="26ac8-127">Use hello **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="26ac8-128">Alapértelmezés szerint az tartalmaz egy külső adatforrás - környezeti változók támogatása.</span><span class="sxs-lookup"><span data-stu-id="26ac8-128">By default it comes with support for one external source - environmental variables.</span></span>
  * <span data-ttu-id="26ac8-129">Hozzáadhat új források hello kiterjesztésével `ConnectionStringSource` osztály.</span><span class="sxs-lookup"><span data-stu-id="26ac8-129">You can add new sources by extending hello `ConnectionStringSource` class.</span></span>

<span data-ttu-id="26ac8-130">Itt vázolt hello példákért hello kapcsolati karakterlánc lett átadva közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="26ac8-130">For hello examples outlined here, hello connection string is passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a><span data-ttu-id="26ac8-131">Üzenettémakör létrehozása</span><span class="sxs-lookup"><span data-stu-id="26ac8-131">Create a topic</span></span>
<span data-ttu-id="26ac8-132">A Service Bus-üzenettémakörök hello használatával kezelési műveleteket hajthat végre `ServiceBusRestProxy` osztály.</span><span class="sxs-lookup"><span data-stu-id="26ac8-132">You can perform management operations for Service Bus topics via hello `ServiceBusRestProxy` class.</span></span> <span data-ttu-id="26ac8-133">A `ServiceBusRestProxy` objektum összeállított keresztül hello `ServicesBuilder::createServiceBusService` gyári módszer egy megfelelő kapcsolati karakterlánccal, amely magában foglalja a hello token engedélyek toomanage azt.</span><span class="sxs-lookup"><span data-stu-id="26ac8-133">A `ServiceBusRestProxy` object is constructed via hello `ServicesBuilder::createServiceBusService` factory method with an appropriate connection string that encapsulates hello token permissions toomanage it.</span></span>

<span data-ttu-id="26ac8-134">a következő példa azt mutatja meg hogyan hello tooinstantiate egy `ServiceBusRestProxy` , és hívja meg `ServiceBusRestProxy->createTopic` toocreate nevű témakör `mytopic` belül egy `MySBNamespace` névtér:</span><span class="sxs-lookup"><span data-stu-id="26ac8-134">hello following example shows how tooinstantiate a `ServiceBusRestProxy` and call `ServiceBusRestProxy->createTopic` toocreate a topic named `mytopic` within a `MySBNamespace` namespace:</span></span>

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
> <span data-ttu-id="26ac8-135">Használhatja a hello `listTopics` metódusa `ServiceBusRestProxy` toocheck objektumokat, ha a témakör a megadott névvel már létezik egy szolgáltatásnévtérben.</span><span class="sxs-lookup"><span data-stu-id="26ac8-135">You can use hello `listTopics` method on `ServiceBusRestProxy` objects toocheck if a topic with a specified name already exists within a service namespace.</span></span>
> 
> 

## <a name="create-a-subscription"></a><span data-ttu-id="26ac8-136">Előfizetés létrehozása</span><span class="sxs-lookup"><span data-stu-id="26ac8-136">Create a subscription</span></span>
<span data-ttu-id="26ac8-137">Üzenettémakör-előfizetéseket is jönnek létre hello `ServiceBusRestProxy->createSubscription` metódust.</span><span class="sxs-lookup"><span data-stu-id="26ac8-137">Topic subscriptions are also created with hello `ServiceBusRestProxy->createSubscription` method.</span></span> <span data-ttu-id="26ac8-138">Előfizetések vannak nevezve, és rendelkezhetnek olyan szűrőkkel, amelyek toohello előfizetés virtuális üzenetsorának átadott üzenetek készletét hello korlátozza.</span><span class="sxs-lookup"><span data-stu-id="26ac8-138">Subscriptions are named and can have an optional filter that restricts hello set of messages passed toohello subscription's virtual queue.</span></span>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="26ac8-139">Előfizetés létrehozása hello alapértelmezett (MatchAll) szűrővel</span><span class="sxs-lookup"><span data-stu-id="26ac8-139">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="26ac8-140">Hello **MatchAll** szűrő hello alapértelmezett szűrő, amely akkor használatos, ha nincs meghatározva szűrő egy új előfizetés létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="26ac8-140">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="26ac8-141">Ha hello **MatchAll** szűrővel, minden üzenetek közzétett toohello témakör kerülnek hello előfizetés virtuális üzenetsorában.</span><span class="sxs-lookup"><span data-stu-id="26ac8-141">When hello **MatchAll** filter is used, all messages published toohello topic are placed in hello subscription's virtual queue.</span></span> <span data-ttu-id="26ac8-142">hello alábbi példakód létrehozza a "mysubscription" nevű előfizetést, és használja az alapértelmezett hello **MatchAll** szűrő.</span><span class="sxs-lookup"><span data-stu-id="26ac8-142">hello following example creates a subscription named 'mysubscription' and uses hello default **MatchAll** filter.</span></span>

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

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="26ac8-143">Előfizetések létrehozása szűrőkkel</span><span class="sxs-lookup"><span data-stu-id="26ac8-143">Create subscriptions with filters</span></span>
<span data-ttu-id="26ac8-144">Is beállíthat szűrőket, amelyek lehetővé teszik a toospecify üzenetet küldött tooa témakör belül egy adott üzenettémakör-előfizetésben kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="26ac8-144">You can also set up filters that enable you toospecify which messages sent tooa topic should appear within a specific topic subscription.</span></span> <span data-ttu-id="26ac8-145">hello szűrő előfizetések által támogatott legrugalmasabb típusú hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), amely az SQL92 egy részhalmazát valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="26ac8-145">hello most flexible type of filter supported by subscriptions is hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), which implements a subset of SQL92.</span></span> <span data-ttu-id="26ac8-146">Az SQL-szűrők hello tulajdonságait, amelyek a közzétett toohello témakör köszönőüzenetei működik.</span><span class="sxs-lookup"><span data-stu-id="26ac8-146">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="26ac8-147">SqlFilters kapcsolatos további információkért lásd: [SqlFilter.SqlExpression tulajdonság][sqlfilter].</span><span class="sxs-lookup"><span data-stu-id="26ac8-147">For more information about SqlFilters, see [SqlFilter.SqlExpression Property][sqlfilter].</span></span>

> [!NOTE]
> <span data-ttu-id="26ac8-148">Minden egyes szabály egy előfizetésben dolgozza fel a bejövő üzenetek egymástól függetlenül, az eredmény üzenetek toohello előfizetés hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="26ac8-148">Each rule on a subscription processes incoming messages independently, adding their result messages toohello subscription.</span></span> <span data-ttu-id="26ac8-149">Emellett minden egyes új előfizetés rendelkezik egy alapértelmezett **szabály** hello témakör toohello előfizetésből minden üzenetet hozzáadó szűrő objektum.</span><span class="sxs-lookup"><span data-stu-id="26ac8-149">In addition, each new subscription has a default **Rule** object with a filter that adds all messages from hello topic toohello subscription.</span></span> <span data-ttu-id="26ac8-150">tooreceive csak üzenetek a szűrőnek megfelelő, el kell távolítania hello alapértelmezett szabály.</span><span class="sxs-lookup"><span data-stu-id="26ac8-150">tooreceive only messages matching your filter, you must remove hello default rule.</span></span> <span data-ttu-id="26ac8-151">Alapértelmezett szabály hello távolíthatja hello használatával `ServiceBusRestProxy->deleteRule` metódust.</span><span class="sxs-lookup"><span data-stu-id="26ac8-151">You can remove hello default rule by using hello `ServiceBusRestProxy->deleteRule` method.</span></span>
> 
> 

<span data-ttu-id="26ac8-152">hello alábbi példa egy nevű előfizetést hoz létre `HighMessages` rendelkező egy **SqlFilter** , amely csak választja ki, amelyek egyéni üzenetek `MessageNumber` 3-nál nagyobb tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="26ac8-152">hello following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `MessageNumber` property greater than 3.</span></span> <span data-ttu-id="26ac8-153">Lásd: [küldési üzenetek tooa témakör](#send-messages-to-a-topic) egyéni tulajdonságok toomessages hozzáadásával kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="26ac8-153">See [Send messages tooa topic](#send-messages-to-a-topic) for information about adding custom properties toomessages.</span></span>

```php
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

<span data-ttu-id="26ac8-154">Vegye figyelembe, hogy ez a kód egy további névtér hello használatát igényli: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.</span><span class="sxs-lookup"><span data-stu-id="26ac8-154">Note that this code requires hello use of an additional namespace: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.</span></span>

<span data-ttu-id="26ac8-155">Ehhez hasonlóan hello alábbi példa egy nevű előfizetést hoz létre `LowMessages` rendelkező egy `SqlFilter` , amely csak rendelkező üzeneteket választja ki egy `MessageNumber` tulajdonságának értéke kisebb vagy egyenlő, mint too3.</span><span class="sxs-lookup"><span data-stu-id="26ac8-155">Similarly, hello following example creates a subscription named `LowMessages` with a `SqlFilter` that only selects messages that have a `MessageNumber` property less than or equal too3.</span></span>

```php
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

<span data-ttu-id="26ac8-156">Mostantól, ha egy üzenetet kap toohello `mytopic` témakör, az előfizetett tooreceivers toohello mindig biztosítását `mysubscription` előfizetés, és szelektív módon kézbesíti az előfizetett tooreceivers toohello `HighMessages` és `LowMessages` előfizetések () attól függően, hello üzenettartalom).</span><span class="sxs-lookup"><span data-stu-id="26ac8-156">Now, when a message is sent toohello `mytopic` topic, it is always delivered tooreceivers subscribed toohello `mysubscription` subscription, and selectively delivered tooreceivers subscribed toohello `HighMessages` and `LowMessages` subscriptions (depending upon hello message content).</span></span>

## <a name="send-messages-tooa-topic"></a><span data-ttu-id="26ac8-157">Üzenetek tooa témakör küldése</span><span class="sxs-lookup"><span data-stu-id="26ac8-157">Send messages tooa topic</span></span>
<span data-ttu-id="26ac8-158">toosend üzenet tooa Service Bus-témakörbe, az alkalmazás meghívja hello `ServiceBusRestProxy->sendTopicMessage` metódust.</span><span class="sxs-lookup"><span data-stu-id="26ac8-158">toosend a message tooa Service Bus topic, your application calls hello `ServiceBusRestProxy->sendTopicMessage` method.</span></span> <span data-ttu-id="26ac8-159">a következő kód bemutatja hogyan hello toosend egy üzenet toohello `mytopic` korábban jött létre a témakör a `MySBNamespace` szolgáltatás névtere.</span><span class="sxs-lookup"><span data-stu-id="26ac8-159">hello following code shows how toosend a message toohello `mytopic` topic previously created within the `MySBNamespace` service namespace.</span></span>

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

<span data-ttu-id="26ac8-160">Üzenetek küldése tooService Bus-üzenettémakörök hello példánya [BrokeredMessage] [ BrokeredMessage] osztály.</span><span class="sxs-lookup"><span data-stu-id="26ac8-160">Messages sent tooService Bus topics are instances of hello [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="26ac8-161">[BrokeredMessage] [ BrokeredMessage] objektumok rendelkeznek szokásos tulajdonságok és módszerek, valamint a használt toohold egyéni alkalmazásspecifikus tulajdonságokat lehet tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="26ac8-161">[BrokeredMessage][BrokeredMessage] objects have a set of standard properties and methods, as well as properties that can be used toohold custom application-specific properties.</span></span> <span data-ttu-id="26ac8-162">hello következő példa bemutatja, hogyan toosend 5 teszt üzenetek toohello `mytopic` a témakörben korábban hozott létre.</span><span class="sxs-lookup"><span data-stu-id="26ac8-162">hello following example shows how toosend 5 test messages toohello `mytopic` topic previously created.</span></span> <span data-ttu-id="26ac8-163">Hello `setProperty` metódus használt tooadd egyéni tulajdonságot (`MessageNumber`) tooeach üzenet.</span><span class="sxs-lookup"><span data-stu-id="26ac8-163">hello `setProperty` method is used tooadd a custom property (`MessageNumber`) tooeach message.</span></span> <span data-ttu-id="26ac8-164">Vegye figyelembe, hogy hello `MessageNumber` tulajdonság értéke változik a minden üzenet (hello ismertetett módon használhatja a érték toodetermine melyik előfizetések fogják megkapni, [előfizetés létrehozása](#create-a-subscription) szakaszt):</span><span class="sxs-lookup"><span data-stu-id="26ac8-164">Note that hello `MessageNumber` property value varies on each message (you can use this value toodetermine which subscriptions receive it, as shown in hello [Create a subscription](#create-a-subscription) section):</span></span>

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

<span data-ttu-id="26ac8-165">Service Bus-üzenettémakörök támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="26ac8-165">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="26ac8-166">hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="26ac8-166">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="26ac8-167">A témakörökben tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello, a témakörök által tárolt hello üzenetek teljes mérete.</span><span class="sxs-lookup"><span data-stu-id="26ac8-167">There is no limit on hello number of messages held in a topic but there is a cap on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="26ac8-168">Ez a témakör mérete felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="26ac8-168">This upper limit on topic size is 5 GB.</span></span> <span data-ttu-id="26ac8-169">Kvóták kapcsolatos további információkért lásd: [Service Bus kvóták][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="26ac8-169">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="26ac8-170">Üzenetek fogadása egy előfizetés</span><span class="sxs-lookup"><span data-stu-id="26ac8-170">Receive messages from a subscription</span></span>
<span data-ttu-id="26ac8-171">hello legjobb módja tooreceive üzenetek előfizetésből toouse egy `ServiceBusRestProxy->receiveSubscriptionMessage` metódust.</span><span class="sxs-lookup"><span data-stu-id="26ac8-171">hello best way tooreceive messages from a subscription is toouse a `ServiceBusRestProxy->receiveSubscriptionMessage` method.</span></span> <span data-ttu-id="26ac8-172">Két különböző módban a lehet üzeneteket fogadni: [ *ReceiveAndDelete* és *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode).</span><span class="sxs-lookup"><span data-stu-id="26ac8-172">Messages can be received in two different modes: [*ReceiveAndDelete* and *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode).</span></span> <span data-ttu-id="26ac8-173">**PeekLock** hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="26ac8-173">**PeekLock** is hello default.</span></span>

<span data-ttu-id="26ac8-174">Hello használatakor [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mód, kap egy egylépéses művelet; Ez azt jelenti, hogy amikor a Service Bus egy olvasási kérést kap egy előfizetésben, akkor hello üzenetet, feldolgozottként jelöli meg és visszaadja toohello az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="26ac8-174">When using hello [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, receive is a single-shot operation; that is, when Service Bus receives a read request for a message in a subscription, it marks hello message as being consumed and returns it toohello application.</span></span> <span data-ttu-id="26ac8-175">[ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * mód hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban.</span><span class="sxs-lookup"><span data-stu-id="26ac8-175">[ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * mode is hello simplest model and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="26ac8-176">toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="26ac8-176">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="26ac8-177">Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.</span><span class="sxs-lookup"><span data-stu-id="26ac8-177">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="26ac8-178">Hello alapértelmezett [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mód, egy üzenet fogadását két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz.</span><span class="sxs-lookup"><span data-stu-id="26ac8-178">In hello default [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, receiving a message becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="26ac8-179">A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása.</span><span class="sxs-lookup"><span data-stu-id="26ac8-179">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="26ac8-180">Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata hello fogadott üzenet túl átadásával`ServiceBusRestProxy->deleteMessage`.</span><span class="sxs-lookup"><span data-stu-id="26ac8-180">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by passing hello received message too`ServiceBusRestProxy->deleteMessage`.</span></span> <span data-ttu-id="26ac8-181">Amikor a Service Bus látja a hello `deleteMessage` hívás, azt feldolgozottként hello üzenetet, és távolítsa el azt a hello várólista.</span><span class="sxs-lookup"><span data-stu-id="26ac8-181">When Service Bus sees hello `deleteMessage` call, it will mark hello message as being consumed and remove it from hello queue.</span></span>

<span data-ttu-id="26ac8-182">a következő példa azt mutatja meg hogyan hello tooreceive és feldolgozni egy üzenetet használatával [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) hello alapértelmezett üzemmódra.</span><span class="sxs-lookup"><span data-stu-id="26ac8-182">hello following example shows how tooreceive and process a message using [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mode (hello default mode).</span></span> 

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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="26ac8-183">Hogyan: alkalmazás-összeomlások és nem olvasható üzenetek kezelése</span><span class="sxs-lookup"><span data-stu-id="26ac8-183">How to: handle application crashes and unreadable messages</span></span>
<span data-ttu-id="26ac8-184">Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít.</span><span class="sxs-lookup"><span data-stu-id="26ac8-184">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="26ac8-185">Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlockMessage` metódust a fogadott üdvözlőüzenetére (helyett hello `deleteMessage` metódus).</span><span class="sxs-lookup"><span data-stu-id="26ac8-185">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlockMessage` method on hello received message (instead of hello `deleteMessage` method).</span></span> <span data-ttu-id="26ac8-186">Ezzel a Service Bus toounlock hello üzenet hello várólistában és révén elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.</span><span class="sxs-lookup"><span data-stu-id="26ac8-186">This will cause Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="26ac8-187">Emellett van hello várólistában lévő társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess üdvözlőüzenetére előtt hello zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus feloldást köszönőüzenetei automatikusan, és teszi elérhetővé toobe újbóli fogadását.</span><span class="sxs-lookup"><span data-stu-id="26ac8-187">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="26ac8-188">A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `deleteMessage` kérés kiadása, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazást, amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="26ac8-188">In hello event that hello application crashes after processing hello message but before hello `deleteMessage` request is issued, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="26ac8-189">Ezt gyakran nevezik *legalább egyszeri* feldolgozása; Ez azt jelenti, hogy minden üzenet legalább egyszer fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet.</span><span class="sxs-lookup"><span data-stu-id="26ac8-189">This is often called *At Least Once* processing; that is, each message is processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="26ac8-190">Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tooapplications toohandle ismétlődő üzenetkézbesítését.</span><span class="sxs-lookup"><span data-stu-id="26ac8-190">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tooapplications toohandle duplicate message delivery.</span></span> <span data-ttu-id="26ac8-191">Ez gyakran érhető el hello segítségével `getMessageId` metódus hello üzenet, amely állandó marad a kézbesítési kísérletek során.</span><span class="sxs-lookup"><span data-stu-id="26ac8-191">This is often achieved using hello `getMessageId` method of hello message, which remains constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="26ac8-192">Témakörök és előfizetések törlése</span><span class="sxs-lookup"><span data-stu-id="26ac8-192">Delete topics and subscriptions</span></span>
<span data-ttu-id="26ac8-193">a témakör és előfizetés, használjon hello toodelete `ServiceBusRestProxy->deleteTopic` vagy hello `ServiceBusRestProxy->deleteSubscripton` módszerek, illetve.</span><span class="sxs-lookup"><span data-stu-id="26ac8-193">toodelete a topic or a subscription, use hello `ServiceBusRestProxy->deleteTopic` or hello `ServiceBusRestProxy->deleteSubscripton` methods, respectively.</span></span> <span data-ttu-id="26ac8-194">Vegye figyelembe, hogy egy témakör törlése összes előfizetést is törli hello témakörben regisztrált.</span><span class="sxs-lookup"><span data-stu-id="26ac8-194">Note that deleting a topic also deletes any subscriptions that are registered with hello topic.</span></span>

<span data-ttu-id="26ac8-195">hello következő példa bemutatja, hogyan toodelete témakör nevű `mytopic` és a regisztrált előfizetések.</span><span class="sxs-lookup"><span data-stu-id="26ac8-195">hello following example shows how toodelete a topic named `mytopic` and its registered subscriptions.</span></span>

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

<span data-ttu-id="26ac8-196">Hello segítségével `deleteSubscription` módszer, egymástól függetlenül előfizetés törlése:</span><span class="sxs-lookup"><span data-stu-id="26ac8-196">By using hello `deleteSubscription` method, you can delete a subscription independently:</span></span>

```php
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a><span data-ttu-id="26ac8-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="26ac8-197">Next steps</span></span>
<span data-ttu-id="26ac8-198">Most, hogy megismerte a Service Bus-üzenetsorok alapjait hello, lásd: [üzenetsorok, témakörök és előfizetések] [ Queues, topics, and subscriptions] további információt.</span><span class="sxs-lookup"><span data-stu-id="26ac8-198">Now that you've learned hello basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

[BrokeredMessage]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter
[require-once]: http://php.net/require_once
[Service Bus quotas]: service-bus-quotas.md
