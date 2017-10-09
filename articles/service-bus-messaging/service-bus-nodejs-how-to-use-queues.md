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
# <a name="how-toouse-service-bus-queues-with-nodejs"></a><span data-ttu-id="12da5-103">Hogyan toouse Service Bus várólisták a Node.js</span><span class="sxs-lookup"><span data-stu-id="12da5-103">How toouse Service Bus queues with Node.js</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="12da5-104">Ez a cikk ismerteti, hogyan toouse Service Bus várólisták a Node.js.</span><span class="sxs-lookup"><span data-stu-id="12da5-104">This article describes how toouse Service Bus queues with Node.js.</span></span> <span data-ttu-id="12da5-105">hello minták JavaScript nyelven íródtak, és Node.js Azure modul hello használata.</span><span class="sxs-lookup"><span data-stu-id="12da5-105">hello samples are written in JavaScript and use hello Node.js Azure module.</span></span> <span data-ttu-id="12da5-106">hello tárgyalt forgatókönyvekben szerepel a **üzenetsorok létrehozása**, **üzenetek küldése és fogadása**, és **várólisták törlése**.</span><span class="sxs-lookup"><span data-stu-id="12da5-106">hello scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span> <span data-ttu-id="12da5-107">A várólisták további információkért lásd: hello [további lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="12da5-107">For more information on queues, see hello [Next steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="12da5-108">Node.js alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="12da5-108">Create a Node.js application</span></span>
<span data-ttu-id="12da5-109">Üres Node.js-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="12da5-109">Create a blank Node.js application.</span></span> <span data-ttu-id="12da5-110">Útmutatást toocreate egy Node.js-alkalmazást, lásd: [létrehozása és központi telepítése egy Node.js-alkalmazás tooan Azure-webhely][Create and deploy a Node.js application tooan Azure Website], vagy [Node.js Felhőszolgáltatás] [ Node.js Cloud Service] Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="12da5-110">For instructions on how toocreate a Node.js application, see [Create and deploy a Node.js application tooan Azure Website][Create and deploy a Node.js application tooan Azure Website], or [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell.</span></span>

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="12da5-111">Az alkalmazás toouse Service Bus konfigurálása</span><span class="sxs-lookup"><span data-stu-id="12da5-111">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="12da5-112">Azure Service Bus toouse töltse le, és hello Node.js Azure csomagot használja.</span><span class="sxs-lookup"><span data-stu-id="12da5-112">toouse Azure Service Bus, download and use hello Node.js Azure package.</span></span> <span data-ttu-id="12da5-113">Ez a csomag tartalmaz egy szalagtár szerepel, amely hello Service Bus REST szolgáltatásokkal kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="12da5-113">This package includes a set of libraries that communicate with hello Service Bus REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="12da5-114">Csomópont Package Manager (NPM) tooobtain hello csomag használata</span><span class="sxs-lookup"><span data-stu-id="12da5-114">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="12da5-115">Használjon hello **Windows PowerShell, a Node.js** parancs ablak toonavigate toohello **c:\\csomópont\\sbqueues\\WebRole1** mappa, amelyben létrehozta a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="12da5-115">Use hello **Windows PowerShell for Node.js** command window toonavigate toohello **c:\\node\\sbqueues\\WebRole1** folder in which you created your sample application.</span></span>
2. <span data-ttu-id="12da5-116">Típus **npm telepítése azure** hello parancssori ablakban, amely kell eredményeznie kimeneti hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="12da5-116">Type **npm install azure** in hello command window, which should result in output similar toohello following:</span></span>

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
3. <span data-ttu-id="12da5-117">Manuális futtatásával hello **ls** parancs tooverify, amely egy **node_modules** mappa hozták létre.</span><span class="sxs-lookup"><span data-stu-id="12da5-117">You can manually run hello **ls** command tooverify that a **node_modules** folder was created.</span></span> <span data-ttu-id="12da5-118">A mappában található hello belül **azure** csomagot, amely tartalmazza a Service Bus-üzenetsorok tooaccess kell hello szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="12da5-118">Inside that folder find hello **azure** package, which contains hello libraries you need tooaccess Service Bus queues.</span></span>

### <a name="import-hello-module"></a><span data-ttu-id="12da5-119">Importálás hello modul</span><span class="sxs-lookup"><span data-stu-id="12da5-119">Import hello module</span></span>
<span data-ttu-id="12da5-120">A Jegyzettömbben vagy más szövegszerkesztőben, adja hozzá a következő toohello felső részén hello hello **server.js** hello alkalmazás fájlt:</span><span class="sxs-lookup"><span data-stu-id="12da5-120">Using Notepad or another text editor, add hello following toohello top of hello **server.js** file of hello application:</span></span>

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a><span data-ttu-id="12da5-121">Az Azure Service Bus-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="12da5-121">Set up an Azure Service Bus connection</span></span>
<span data-ttu-id="12da5-122">hello Azure a modul olvassa be hello környezeti változó `AZURE_SERVICEBUS_CONNECTION_STRING` szükséges tooconnect tooService Bus tooobtain információt.</span><span class="sxs-lookup"><span data-stu-id="12da5-122">hello Azure module reads hello environment variable `AZURE_SERVICEBUS_CONNECTION_STRING` tooobtain information required tooconnect tooService Bus.</span></span> <span data-ttu-id="12da5-123">Ha e környezeti változó nincs beállítva, meg kell adnia hello fiók meghívásakor `createServiceBusService`.</span><span class="sxs-lookup"><span data-stu-id="12da5-123">If this environment variable is not set, you must specify hello account information when calling `createServiceBusService`.</span></span>

<span data-ttu-id="12da5-124">A konfigurációs fájlban hello környezeti változók beállítása az Azure Cloud Service példát lásd: [Node.js Felhőszolgáltatás tárolási][Node.js Cloud Service with Storage].</span><span class="sxs-lookup"><span data-stu-id="12da5-124">For an example of setting hello environment variables in a configuration file for an Azure Cloud Service, see [Node.js Cloud Service with Storage][Node.js Cloud Service with Storage].</span></span>

<span data-ttu-id="12da5-125">Példa a hello hello környezeti változók beállítása [Azure-portálon] [ Azure portal] egy Azure-webhelyre, lásd: [Node.js-webalkalmazás tárolóval] [ Node.js Web Application with Storage].</span><span class="sxs-lookup"><span data-stu-id="12da5-125">For an example of setting hello environment variables in hello [Azure portal][Azure portal] for an Azure Website, see [Node.js Web Application with Storage][Node.js Web Application with Storage].</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="12da5-126">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="12da5-126">Create a queue</span></span>
<span data-ttu-id="12da5-127">Hello **ServiceBusService** objektum lehetővé teszi a Service Bus-üzenetsorok toowork.</span><span class="sxs-lookup"><span data-stu-id="12da5-127">hello **ServiceBusService** object enables you toowork with Service Bus queues.</span></span> <span data-ttu-id="12da5-128">hello alábbi kód létrehoz egy **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="12da5-128">hello following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="12da5-129">Hello hello tetején adja hozzá **server.js** fájl hello utasítás tooimport hello Azure modul után:</span><span class="sxs-lookup"><span data-stu-id="12da5-129">Add it near hello top of hello **server.js** file, after hello statement tooimport hello Azure module:</span></span>

```javascript
var serviceBusService = azure.createServiceBusService();
```

<span data-ttu-id="12da5-130">Meghívásával `createQueueIfNotExists` a hello **ServiceBusService** objektum, hello megadott várólista ad vissza (ha van ilyen), vagy létrehoz egy új várólistát hello megadott névvel.</span><span class="sxs-lookup"><span data-stu-id="12da5-130">By calling `createQueueIfNotExists` on hello **ServiceBusService** object, hello specified queue is returned (if it exists), or a new queue with hello specified name is created.</span></span> <span data-ttu-id="12da5-131">hello következő kódban `createQueueIfNotExists` toocreate vagy kapcsolódási nevű toohello várólista `myqueue`:</span><span class="sxs-lookup"><span data-stu-id="12da5-131">hello following code uses `createQueueIfNotExists` toocreate or connect toohello queue named `myqueue`:</span></span>

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

<span data-ttu-id="12da5-132">Hello `createServiceBusService` metódus is támogatja a további beállításokat, amelyek lehetővé teszik a toooverride alapértelmezett várólista-beállításokat például üzenet ideje toolive és maximális várólista-méret.</span><span class="sxs-lookup"><span data-stu-id="12da5-132">hello `createServiceBusService` method also supports additional options, which enable you toooverride default queue settings such as message time toolive or maximum queue size.</span></span> <span data-ttu-id="12da5-133">hello alábbi mintakód hello várólista maximális mérete too5 GB, és egy időt toolive (TTL) értéke 1 perc:</span><span class="sxs-lookup"><span data-stu-id="12da5-133">hello following example sets hello maximum queue size too5 GB, and a time toolive (TTL) value of 1 minute:</span></span>

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

### <a name="filters"></a><span data-ttu-id="12da5-134">Szűrők</span><span class="sxs-lookup"><span data-stu-id="12da5-134">Filters</span></span>
<span data-ttu-id="12da5-135">Választható szűrési műveletek lehet segítségével alkalmazott toooperations **ServiceBusService**.</span><span class="sxs-lookup"><span data-stu-id="12da5-135">Optional filtering operations can be applied toooperations performed using **ServiceBusService**.</span></span> <span data-ttu-id="12da5-136">Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. Szűrők hello aláírással metódus megvalósító objektumok a következők:</span><span class="sxs-lookup"><span data-stu-id="12da5-136">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```javascript
function handle (requestOptions, next)
```

<span data-ttu-id="12da5-137">Ezután az előzetes feldolgozás hello kérelem lehetőségekről, hello metódust kell hívni `next`, egy visszahívási átadja a aláírását hello:</span><span class="sxs-lookup"><span data-stu-id="12da5-137">After doing its pre-processing on hello request options, hello method must call `next`, passing a callback with hello following signature:</span></span>

```javascript
function (returnObject, finalCallback, next)
```

<span data-ttu-id="12da5-138">A visszahívási, és feldolgozás hello követően `returnObject` (hello hello kérelem toohello kiszolgálótól kapott válasz), hello visszahívási vagy kell meghívni `next` Ha azt egyéb szűrők feldolgozása toocontinue létezik, vagy egyszerűen meghívása `finalCallback`, mely vége hello szolgáltatás hívása.</span><span class="sxs-lookup"><span data-stu-id="12da5-138">In this callback, and after processing hello `returnObject` (hello response from hello request toohello server), hello callback must either invoke `next` if it exists toocontinue processing other filters, or simply invoke `finalCallback`, which ends hello service invocation.</span></span>

<span data-ttu-id="12da5-139">Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el a hello Azure SDK for Node.js, `ExponentialRetryPolicyFilter` és `LinearRetryPolicyFilter`.</span><span class="sxs-lookup"><span data-stu-id="12da5-139">Two filters that implement retry logic are included with hello Azure SDK for Node.js, `ExponentialRetryPolicyFilter` and `LinearRetryPolicyFilter`.</span></span> <span data-ttu-id="12da5-140">hello alábbi kód létrehoz egy `ServiceBusService` hello használó objektum `ExponentialRetryPolicyFilter`:</span><span class="sxs-lookup"><span data-stu-id="12da5-140">hello following code creates a `ServiceBusService` object that uses hello `ExponentialRetryPolicyFilter`:</span></span>

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-tooa-queue"></a><span data-ttu-id="12da5-141">Üzenetek tooa várólista küldése</span><span class="sxs-lookup"><span data-stu-id="12da5-141">Send messages tooa queue</span></span>
<span data-ttu-id="12da5-142">toosend üzenet tooa Service Bus-üzenetsorba, az alkalmazás meghívja hello `sendQueueMessage` hello metódusa **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="12da5-142">toosend a message tooa Service Bus queue, your application calls hello `sendQueueMessage` method on hello **ServiceBusService** object.</span></span> <span data-ttu-id="12da5-143">Üzenetek túl elküldött (és a fogadott) Service Bus-üzenetsor **BrokeredMessage** objektumokat, és rendelkezik egy szabványos tulajdonságkészlettel (például **címke** és **TimeToLive**), egy amely használt toohold egyéni alkalmazásspecifikus tulajdonságokat, valamint egy tetszőleges alkalmazásadatokból álló törzzsel szótárban.</span><span class="sxs-lookup"><span data-stu-id="12da5-143">Messages sent too(and received from) Service Bus queues are **BrokeredMessage** objects, and have a set of standard properties (such as **Label** and **TimeToLive**), a dictionary that is used toohold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="12da5-144">Az alkalmazás beállíthatja hello üzenet törzsét hello úgy, hogy egy karakterláncot hello üzenetben.</span><span class="sxs-lookup"><span data-stu-id="12da5-144">An application can set hello body of hello message by passing a string as hello message.</span></span> <span data-ttu-id="12da5-145">Egyéb szabványos tulajdonságokat a rendszer feltölti az alapértelmezett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="12da5-145">Any required standard properties are populated with default values.</span></span>

<span data-ttu-id="12da5-146">hello következő példa bemutatja, hogyan toosend egy teszt üzenetsor toohello nevű `myqueue` használatával `sendQueueMessage`:</span><span class="sxs-lookup"><span data-stu-id="12da5-146">hello following example demonstrates how toosend a test message toohello queue named `myqueue` using `sendQueueMessage`:</span></span>

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

<span data-ttu-id="12da5-147">Service Bus-üzenetsorok támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="12da5-147">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="12da5-148">hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="12da5-148">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="12da5-149">Az üzenetsorban tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello üzenetsor által tárolt hello üzenetek teljes mérete.</span><span class="sxs-lookup"><span data-stu-id="12da5-149">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="12da5-150">Az üzenetsor ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="12da5-150">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="12da5-151">Kvóták kapcsolatos további információkért lásd: [Service Bus kvóták][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="12da5-151">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="12da5-152">Üzenetek fogadása egy üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="12da5-152">Receive messages from a queue</span></span>
<span data-ttu-id="12da5-153">Üzenetek fogadása egy üzenetsorból hello segítségével `receiveQueueMessage` hello metódusa **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="12da5-153">Messages are received from a queue using hello `receiveQueueMessage` method on hello **ServiceBusService** object.</span></span> <span data-ttu-id="12da5-154">Alapértelmezés szerint az üzenetek törlődnek hello várólistából, olvasott; azonban (betekintés) olvashatja és üdvözlőüzenetére zárolása hello nem kötelező paraméter beállítása hello várólista törlése nélkül `isPeekLock` túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="12da5-154">By default, messages are deleted from hello queue as they are read; however, you can read (peek) and lock hello message without deleting it from hello queue by setting hello optional parameter `isPeekLock` too**true**.</span></span>

<span data-ttu-id="12da5-155">hello olvasási alapértelmezett viselkedését, és üdvözlőüzenetére törlése, hello részét fogadási művelethez hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban.</span><span class="sxs-lookup"><span data-stu-id="12da5-155">hello default behavior of reading and deleting hello message as part of hello receive operation is hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="12da5-156">toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="12da5-156">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="12da5-157">Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.</span><span class="sxs-lookup"><span data-stu-id="12da5-157">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="12da5-158">Ha hello `isPeekLock` paraméter értéke túl**igaz**, hello kap két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz.</span><span class="sxs-lookup"><span data-stu-id="12da5-158">If hello `isPeekLock` parameter is set too**true**, hello receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="12da5-159">A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása.</span><span class="sxs-lookup"><span data-stu-id="12da5-159">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="12da5-160">Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával `deleteMessage` metódus és a hello üzenet toobe paraméterként törölték.</span><span class="sxs-lookup"><span data-stu-id="12da5-160">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling `deleteMessage` method and providing hello message toobe deleted as a parameter.</span></span> <span data-ttu-id="12da5-161">Hello `deleteMessage` metódus hello üzenetet, feldolgozottként jelöli meg, és eltávolítja a hello várólista.</span><span class="sxs-lookup"><span data-stu-id="12da5-161">hello `deleteMessage` method marks hello message as being consumed and removes it from hello queue.</span></span>

<span data-ttu-id="12da5-162">hello következő példa bemutatja, hogyan tooreceive és folyamat-üzenetek használatával `receiveQueueMessage`.</span><span class="sxs-lookup"><span data-stu-id="12da5-162">hello following example demonstrates how tooreceive and process messages using `receiveQueueMessage`.</span></span> <span data-ttu-id="12da5-163">hello példa először kap, és törli az üzenetet, és majd kap egy üzenetet használatával `isPeekLock` beállítása túl**igaz**, majd törli hello üzenetet `deleteMessage`:</span><span class="sxs-lookup"><span data-stu-id="12da5-163">hello example first receives and deletes a message, and then receives a message using `isPeekLock` set too**true**, then deletes hello message using `deleteMessage`:</span></span>

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="12da5-164">Hogyan toohandle omlik össze és nem olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="12da5-164">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="12da5-165">Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít.</span><span class="sxs-lookup"><span data-stu-id="12da5-165">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="12da5-166">Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlockMessage` hello metódusa **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="12da5-166">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlockMessage` method on hello **ServiceBusService** object.</span></span> <span data-ttu-id="12da5-167">Ezzel a Service Bus toounlock okozhat a hello várólista üzenet és révén elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.</span><span class="sxs-lookup"><span data-stu-id="12da5-167">This will cause Service Bus toounlock the message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="12da5-168">Emellett van hello várólistában lévő társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess hello előtt hello zárolás időtúllépését lejárta (pl. Ha hello alkalmazás összeomlik), akkor a Service Bus automatikusan feloldást köszönőüzenetei és a rendelkezésre álló toobe újbóli fogadását.</span><span class="sxs-lookup"><span data-stu-id="12da5-168">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (e.g., if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="12da5-169">A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `deleteMessage` metódust, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazás, amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="12da5-169">In hello event that hello application crashes after processing hello message but before hello `deleteMessage` method is called, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="12da5-170">Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet.</span><span class="sxs-lookup"><span data-stu-id="12da5-170">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="12da5-171">Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését.</span><span class="sxs-lookup"><span data-stu-id="12da5-171">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="12da5-172">Ez gyakran érhető el hello segítségével **MessageId** hello üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="12da5-172">This is often achieved using hello **MessageId** property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12da5-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12da5-173">Next steps</span></span>
<span data-ttu-id="12da5-174">További információ az üzenetsorok, toolearn lásd: a következő erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="12da5-174">toolearn more about queues, see hello following resources.</span></span>

* <span data-ttu-id="12da5-175">[Üzenetsorok, témakörök és előfizetések][Queues, topics, and subscriptions]</span><span class="sxs-lookup"><span data-stu-id="12da5-175">[Queues, topics, and subscriptions][Queues, topics, and subscriptions]</span></span>
* <span data-ttu-id="12da5-176">[Az Azure SDK csomópont] [ Azure SDK for Node] GitHub tárházából</span><span class="sxs-lookup"><span data-stu-id="12da5-176">[Azure SDK for Node][Azure SDK for Node] repository on GitHub</span></span>
* [<span data-ttu-id="12da5-177">Node.js fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="12da5-177">Node.js Developer Center</span></span>](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application tooan Azure Website]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
