---
title: "Service Bus-üzenetsorok használata a node.js |} Microsoft Docs"
description: "Útmutató a Service Bus-üzenetsorok használata Node.js-alkalmazás az Azure-ban."
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
ms.openlocfilehash: fe2c02534996d99c190593a419a4823888f03d31
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-service-bus-queues-with-nodejs"></a><span data-ttu-id="06530-103">Service Bus-üzenetsorok használata Node.js</span><span class="sxs-lookup"><span data-stu-id="06530-103">How to use Service Bus queues with Node.js</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="06530-104">Ez a cikk ismerteti a Service Bus-üzenetsorok használata Node.js.</span><span class="sxs-lookup"><span data-stu-id="06530-104">This article describes how to use Service Bus queues with Node.js.</span></span> <span data-ttu-id="06530-105">A minták JavaScript nyelven íródtak, és a Node.js Azure modullal.</span><span class="sxs-lookup"><span data-stu-id="06530-105">The samples are written in JavaScript and use the Node.js Azure module.</span></span> <span data-ttu-id="06530-106">Az ismertetett forgatókönyvek **üzenetsorok létrehozása**, **üzenetek küldése és fogadása**, és **várólisták törlése**.</span><span class="sxs-lookup"><span data-stu-id="06530-106">The scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span> <span data-ttu-id="06530-107">A várólisták további információkért lásd: a [további lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="06530-107">For more information on queues, see the [Next steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="06530-108">Node.js alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="06530-108">Create a Node.js application</span></span>
<span data-ttu-id="06530-109">Üres Node.js-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="06530-109">Create a blank Node.js application.</span></span> <span data-ttu-id="06530-110">A Node.js-alkalmazás létrehozása, lásd: [létrehozása és központi telepítése egy Node.js-alkalmazás Azure-webhely][Create and deploy a Node.js application to an Azure Website], vagy [Node.js Felhőszolgáltatás] [ Node.js Cloud Service] Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="06530-110">For instructions on how to create a Node.js application, see [Create and deploy a Node.js application to an Azure Website][Create and deploy a Node.js application to an Azure Website], or [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell.</span></span>

## <a name="configure-your-application-to-use-service-bus"></a><span data-ttu-id="06530-111">Állítsa be az alkalmazását, Service Bus használatára</span><span class="sxs-lookup"><span data-stu-id="06530-111">Configure your application to use Service Bus</span></span>
<span data-ttu-id="06530-112">Azure Service Bus használatára, töltse le, és a Node.js Azure csomagot használja.</span><span class="sxs-lookup"><span data-stu-id="06530-112">To use Azure Service Bus, download and use the Node.js Azure package.</span></span> <span data-ttu-id="06530-113">Ez a csomag tartalmaz egy szalagtár szerepel, amely a Service Bus REST-szolgáltatásokkal kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="06530-113">This package includes a set of libraries that communicate with the Service Bus REST services.</span></span>

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a><span data-ttu-id="06530-114">Csomópont Package Manager (NPM) használja a csomag beszerzése</span><span class="sxs-lookup"><span data-stu-id="06530-114">Use Node Package Manager (NPM) to obtain the package</span></span>
1. <span data-ttu-id="06530-115">Használja a **Windows PowerShell, a Node.js** parancssori ablakban navigáljon a **c:\\csomópont\\sbqueues\\WebRole1** mappa, amelyben a minta létrehozása az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="06530-115">Use the **Windows PowerShell for Node.js** command window to navigate to the **c:\\node\\sbqueues\\WebRole1** folder in which you created your sample application.</span></span>
2. <span data-ttu-id="06530-116">Típus **npm telepítése azure** a parancsablakban, amely eredményez a kimenet az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="06530-116">Type **npm install azure** in the command window, which should result in output similar to the following:</span></span>

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
3. <span data-ttu-id="06530-117">Manuálisan futtatható a **ls** parancs futtatásával ellenőrizze, hogy egy **node_modules** mappa hozták létre.</span><span class="sxs-lookup"><span data-stu-id="06530-117">You can manually run the **ls** command to verify that a **node_modules** folder was created.</span></span> <span data-ttu-id="06530-118">A mappában található a **azure** csomag, amely tartalmazza a Service Bus-üzenetsorok eléréséhez szükséges könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="06530-118">Inside that folder find the **azure** package, which contains the libraries you need to access Service Bus queues.</span></span>

### <a name="import-the-module"></a><span data-ttu-id="06530-119">A modul importálása</span><span class="sxs-lookup"><span data-stu-id="06530-119">Import the module</span></span>
<span data-ttu-id="06530-120">A Jegyzettömbben vagy más szövegszerkesztőben, adja hozzá a következő elejéhez a **server.js** fájl tartalmát:</span><span class="sxs-lookup"><span data-stu-id="06530-120">Using Notepad or another text editor, add the following to the top of the **server.js** file of the application:</span></span>

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a><span data-ttu-id="06530-121">Az Azure Service Bus-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="06530-121">Set up an Azure Service Bus connection</span></span>
<span data-ttu-id="06530-122">Az Azure modul olvassa be a következő környezeti változó `AZURE_SERVICEBUS_CONNECTION_STRING` beszerzése a Service Bus való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="06530-122">The Azure module reads the environment variable `AZURE_SERVICEBUS_CONNECTION_STRING` to obtain information required to connect to Service Bus.</span></span> <span data-ttu-id="06530-123">Ha e környezeti változó nincs beállítva, meg kell adnia a fiókadatokat, meghívásakor `createServiceBusService`.</span><span class="sxs-lookup"><span data-stu-id="06530-123">If this environment variable is not set, you must specify the account information when calling `createServiceBusService`.</span></span>

<span data-ttu-id="06530-124">A környezeti változók beállítása a konfigurációs fájlban az Azure-Felhőszolgáltatásban példát lásd: [Node.js Felhőszolgáltatás tárolási][Node.js Cloud Service with Storage].</span><span class="sxs-lookup"><span data-stu-id="06530-124">For an example of setting the environment variables in a configuration file for an Azure Cloud Service, see [Node.js Cloud Service with Storage][Node.js Cloud Service with Storage].</span></span>

<span data-ttu-id="06530-125">A környezeti változók beállítása példát a [Azure-portálon] [ Azure portal] egy Azure-webhelyre, lásd: [Node.js-webalkalmazás tárolóval] [ Node.js Web Application with Storage].</span><span class="sxs-lookup"><span data-stu-id="06530-125">For an example of setting the environment variables in the [Azure portal][Azure portal] for an Azure Website, see [Node.js Web Application with Storage][Node.js Web Application with Storage].</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="06530-126">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="06530-126">Create a queue</span></span>
<span data-ttu-id="06530-127">A **ServiceBusService** objektum lehetővé teszi, hogy a Service Bus-üzenetsorok használata.</span><span class="sxs-lookup"><span data-stu-id="06530-127">The **ServiceBusService** object enables you to work with Service Bus queues.</span></span> <span data-ttu-id="06530-128">Az alábbi kód létrehoz egy **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="06530-128">The following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="06530-129">Az tetején adja hozzá a **server.js** fájl, az utasítást, hogy az Azure modul importálása után:</span><span class="sxs-lookup"><span data-stu-id="06530-129">Add it near the top of the **server.js** file, after the statement to import the Azure module:</span></span>

```javascript
var serviceBusService = azure.createServiceBusService();
```

<span data-ttu-id="06530-130">Meghívásával `createQueueIfNotExists` a a **ServiceBusService** objektum, a megadott várólista ad vissza (ha van ilyen), vagy létrehoz egy új várólistát a megadott névvel.</span><span class="sxs-lookup"><span data-stu-id="06530-130">By calling `createQueueIfNotExists` on the **ServiceBusService** object, the specified queue is returned (if it exists), or a new queue with the specified name is created.</span></span> <span data-ttu-id="06530-131">A következő kódban `createQueueIfNotExists` létrehozása vagy csatlakozás nevű üzenetsor `myqueue`:</span><span class="sxs-lookup"><span data-stu-id="06530-131">The following code uses `createQueueIfNotExists` to create or connect to the queue named `myqueue`:</span></span>

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

<span data-ttu-id="06530-132">A `createServiceBusService` metódus is támogatja a további beállításokat, amelyek lehetővé teszik a bírálja felül az alapértelmezett üzenet time to live vagy maximális Várólistaméret például várólista-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="06530-132">The `createServiceBusService` method also supports additional options, which enable you to override default queue settings such as message time to live or maximum queue size.</span></span> <span data-ttu-id="06530-133">Az alábbi példa 5 GB, és egyesével élettartam (TTL) értéke 1 perc állítja a várólista maximális hossza:</span><span class="sxs-lookup"><span data-stu-id="06530-133">The following example sets the maximum queue size to 5 GB, and a time to live (TTL) value of 1 minute:</span></span>

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

### <a name="filters"></a><span data-ttu-id="06530-134">Szűrők</span><span class="sxs-lookup"><span data-stu-id="06530-134">Filters</span></span>
<span data-ttu-id="06530-135">Választható szűrési műveletek használatával végrehajtott műveletek alkalmazhatók **ServiceBusService**.</span><span class="sxs-lookup"><span data-stu-id="06530-135">Optional filtering operations can be applied to operations performed using **ServiceBusService**.</span></span> <span data-ttu-id="06530-136">Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. A metódus aláírása megvalósító objektumok szűrők a következők:</span><span class="sxs-lookup"><span data-stu-id="06530-136">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```javascript
function handle (requestOptions, next)
```

<span data-ttu-id="06530-137">Ezután a kérelem a beállítások előzetes feldolgozás, a metódust kell hívni `next`, átadja a következő aláírással rendelkező visszahívás:</span><span class="sxs-lookup"><span data-stu-id="06530-137">After doing its pre-processing on the request options, the method must call `next`, passing a callback with the following signature:</span></span>

```javascript
function (returnObject, finalCallback, next)
```

<span data-ttu-id="06530-138">A visszahívási, és feldolgozás után a `returnObject` (válasza a kérés a kiszolgáló), a visszahívás vagy kell meghívni `next` folytatni többi szűrőt, vagy egyszerűen meghívása létezésének `finalCallback`, amely karakterlánccal végződik-e a szolgáltatás hívása.</span><span class="sxs-lookup"><span data-stu-id="06530-138">In this callback, and after processing the `returnObject` (the response from the request to the server), the callback must either invoke `next` if it exists to continue processing other filters, or simply invoke `finalCallback`, which ends the service invocation.</span></span>

<span data-ttu-id="06530-139">Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el az Azure SDK for Node.js, a `ExponentialRetryPolicyFilter` és `LinearRetryPolicyFilter`.</span><span class="sxs-lookup"><span data-stu-id="06530-139">Two filters that implement retry logic are included with the Azure SDK for Node.js, `ExponentialRetryPolicyFilter` and `LinearRetryPolicyFilter`.</span></span> <span data-ttu-id="06530-140">Az alábbi kód létrehoz egy `ServiceBusService` objektum, amely használja a `ExponentialRetryPolicyFilter`:</span><span class="sxs-lookup"><span data-stu-id="06530-140">The following code creates a `ServiceBusService` object that uses the `ExponentialRetryPolicyFilter`:</span></span>

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a><span data-ttu-id="06530-141">Üzenetek küldése egy üzenetsorba</span><span class="sxs-lookup"><span data-stu-id="06530-141">Send messages to a queue</span></span>
<span data-ttu-id="06530-142">Egy üzenet küldhető egy Service Bus-üzenetsorba, az alkalmazás hívások a `sendQueueMessage` metódust a **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="06530-142">To send a message to a Service Bus queue, your application calls the `sendQueueMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="06530-143">Üzenetek küldenek (és a fogadott) Service Bus-üzenetsor **BrokeredMessage** objektumokat, és rendelkezik egy szabványos tulajdonságkészlettel (például **címke** és **TimeToLive**), egy egyéni alkalmazásspecifikus tulajdonságokat, és egy tetszőleges alkalmazásadatokból álló törzzsel tárolására használt szótárban.</span><span class="sxs-lookup"><span data-stu-id="06530-143">Messages sent to (and received from) Service Bus queues are **BrokeredMessage** objects, and have a set of standard properties (such as **Label** and **TimeToLive**), a dictionary that is used to hold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="06530-144">Az alkalmazás beállíthatja az üzenet törzsét úgy, hogy egy karakterláncot az üzenetben.</span><span class="sxs-lookup"><span data-stu-id="06530-144">An application can set the body of the message by passing a string as the message.</span></span> <span data-ttu-id="06530-145">Egyéb szabványos tulajdonságokat a rendszer feltölti az alapértelmezett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="06530-145">Any required standard properties are populated with default values.</span></span>

<span data-ttu-id="06530-146">A következő példa bemutatja, hogyan tesztüzenet küldése az üzenetsorba nevű `myqueue` használatával `sendQueueMessage`:</span><span class="sxs-lookup"><span data-stu-id="06530-146">The following example demonstrates how to send a test message to the queue named `myqueue` using `sendQueueMessage`:</span></span>

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

<span data-ttu-id="06530-147">A Service Bus-üzenetsorok a [Standard csomagban](service-bus-premium-messaging.md) legfeljebb 256 KB, a [Prémium csomagban](service-bus-premium-messaging.md) legfeljebb 1 MB méretű üzeneteket támogatnak.</span><span class="sxs-lookup"><span data-stu-id="06530-147">Service Bus queues support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="06530-148">A szabványos és az egyéni alkalmazástulajdonságokat tartalmazó fejléc mérete legfeljebb 64 KB lehet.</span><span class="sxs-lookup"><span data-stu-id="06530-148">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="06530-149">Az üzenetsorban tárolt üzenetek száma korlátlan, az üzenetsor által tárolt üzenetek teljes mérete azonban korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="06530-149">There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue.</span></span> <span data-ttu-id="06530-150">Az üzenetsor ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="06530-150">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="06530-151">Kvóták kapcsolatos további információkért lásd: [Service Bus kvóták][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="06530-151">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="06530-152">Üzenetek fogadása egy üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="06530-152">Receive messages from a queue</span></span>
<span data-ttu-id="06530-153">Üzenetek fogadása egy várólista használja a `receiveQueueMessage` metódust a **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="06530-153">Messages are received from a queue using the `receiveQueueMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="06530-154">Alapértelmezés szerint az üzenetek törlődnek a várólistából, olvasott; azonban (betekintés) olvasni és zárolni az üzenet törlése az üzenetsorból úgy, hogy a nem kötelező paraméter nélkül `isPeekLock` való **igaz**.</span><span class="sxs-lookup"><span data-stu-id="06530-154">By default, messages are deleted from the queue as they are read; however, you can read (peek) and lock the message without deleting it from the queue by setting the optional parameter `isPeekLock` to **true**.</span></span>

<span data-ttu-id="06530-155">Olvasási és az üzenet törlése a fogadási művelet részeként alapértelmezett viselkedése a legegyszerűbb modell, és forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet hiba esetén a legjobban.</span><span class="sxs-lookup"><span data-stu-id="06530-155">The default behavior of reading and deleting the message as part of the receive operation is the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="06530-156">Ennek megértéséhez képzeljen el egy forgatókönyvet, amelyben a fogyasztó kiad egy fogadási kérést, majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="06530-156">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="06530-157">Mivel a Service Bus csak fel az üzenetet, feldolgozottként, majd az alkalmazás újraindításakor és megkezdése az üzenetek fel újra, amikor ki fogja hagyni a az összeomlás előtt feldolgozott üzenetet.</span><span class="sxs-lookup"><span data-stu-id="06530-157">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="06530-158">Ha a `isPeekLock` paraméter értéke **igaz**, a receive két szakaszból álló művelet, amely lehetővé teszi az alkalmazások támogatását, amelyek működését zavarják a hiányzó üzenetek lesz.</span><span class="sxs-lookup"><span data-stu-id="06530-158">If the `isPeekLock` parameter is set to **true**, the receive becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="06530-159">Amikor a Service Bus fogad egy kérést, megkeresi és zárolja a következő feldolgozandó üzenetet, hogy más fogyasztók ne tudják fogadni, majd visszaadja az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="06530-159">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="06530-160">Miután az alkalmazás befejezi az üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja a második a fogadási folyamat meghívásával `deleteMessage` metódus és a törlendő paraméterként message.</span><span class="sxs-lookup"><span data-stu-id="06530-160">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling `deleteMessage` method and providing the message to be deleted as a parameter.</span></span> <span data-ttu-id="06530-161">A `deleteMessage` metódus az üzenetet, feldolgozottként jelöli meg, és eltávolítja az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="06530-161">The `deleteMessage` method marks the message as being consumed and removes it from the queue.</span></span>

<span data-ttu-id="06530-162">A következő példa bemutatja, hogyan használatával üzenetek fogadása és feldolgozása `receiveQueueMessage`.</span><span class="sxs-lookup"><span data-stu-id="06530-162">The following example demonstrates how to receive and process messages using `receiveQueueMessage`.</span></span> <span data-ttu-id="06530-163">A példa első fogadja és törli az üzenetet, és majd kap egy üzenetet használatával `isPeekLock` beállítása **igaz**, majd törli az üzenet használatával `deleteMessage`:</span><span class="sxs-lookup"><span data-stu-id="06530-163">The example first receives and deletes a message, and then receives a message using `isPeekLock` set to **true**, then deletes the message using `deleteMessage`:</span></span>

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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="06530-164">Az alkalmazás-összeomlások és nem olvasható üzenetek kezelése</span><span class="sxs-lookup"><span data-stu-id="06530-164">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="06530-165">A Service Bus olyan funkciókat biztosít, amelyekkel zökkenőmentesen helyreállíthatja az alkalmazás hibáit vagy az üzenetek feldolgozásának nehézségeit.</span><span class="sxs-lookup"><span data-stu-id="06530-165">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="06530-166">Ha egy fogadó alkalmazás nem tudja feldolgozni az üzenetet valamilyen okból kifolyólag, majd akkor meghívhatja a `unlockMessage` metódust a **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="06530-166">If a receiver application is unable to process the message for some reason, then it can call the `unlockMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="06530-167">Ennek hatására a Service Bus feloldja az üzenet a várólistában zárolását, és lehetővé teszi a felhasználó az alkalmazás vagy egy másik fogyasztó alkalmazás általi ismételt fogadását.</span><span class="sxs-lookup"><span data-stu-id="06530-167">This will cause Service Bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="06530-168">Még nincs hozzárendelve az üzenetsorban lévő időtúllépés, és ha az alkalmazás nem tudja feldolgozni az üzenetet, mielőtt a zárolás időtúllépését lejárta (pl., ha az alkalmazás összeomlik), akkor a Service Bus automatikusan feloldja az üzenet zárolását, és meg fogja teszi elérhető ismételt fogadását.</span><span class="sxs-lookup"><span data-stu-id="06530-168">There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (e.g., if the application crashes), then Service Bus will unlock the message automatically and make it available to be received again.</span></span>

<span data-ttu-id="06530-169">Abban az esetben, ha az alkalmazás összeomlik, de előtte az üzenet feldolgozása után a `deleteMessage` metódust, akkor az üzenet újból kézbesítve lesz az alkalmazás amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="06530-169">In the event that the application crashes after processing the message but before the `deleteMessage` method is called, then the message will be redelivered to the application when it restarts.</span></span> <span data-ttu-id="06530-170">Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben a a ugyanazon üzenet újbóli kézbesítése is lehet.</span><span class="sxs-lookup"><span data-stu-id="06530-170">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="06530-171">Ha a forgatókönyvben nem lehetségesek a duplikált üzenetek, akkor az alkalmazásfejlesztőnek további logikát kell az alkalmazásba építenie az üzenetek ismételt kézbesítésének kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="06530-171">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="06530-172">Ez gyakran érhető el, használja a **MessageId** az üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="06530-172">This is often achieved using the **MessageId** property of the message, which will remain constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="06530-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06530-173">Next steps</span></span>
<span data-ttu-id="06530-174">A várólisták kapcsolatos további tudnivalókért lásd a következőket.</span><span class="sxs-lookup"><span data-stu-id="06530-174">To learn more about queues, see the following resources.</span></span>

* <span data-ttu-id="06530-175">[Üzenetsorok, témakörök és előfizetések][Queues, topics, and subscriptions]</span><span class="sxs-lookup"><span data-stu-id="06530-175">[Queues, topics, and subscriptions][Queues, topics, and subscriptions]</span></span>
* <span data-ttu-id="06530-176">[Az Azure SDK csomópont] [ Azure SDK for Node] GitHub tárházából</span><span class="sxs-lookup"><span data-stu-id="06530-176">[Azure SDK for Node][Azure SDK for Node] repository on GitHub</span></span>
* [<span data-ttu-id="06530-177">Node.js fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="06530-177">Node.js Developer Center</span></span>](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application to an Azure Website]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
