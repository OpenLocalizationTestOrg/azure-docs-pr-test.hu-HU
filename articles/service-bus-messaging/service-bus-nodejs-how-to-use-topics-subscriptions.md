---
title: "Azure Service Bus-üzenettémák és előfizetések használata Node.js |} Microsoft Docs"
description: "Megtudhatja, hogyan használja a Service Bus-üzenettémák és előfizetések az Azure-ban from a Node.js-alkalmazás."
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
ms.openlocfilehash: 24ae9b80f75531c5e4a84c3b4a6666a6f8a83d2c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-nodejs"></a><span data-ttu-id="0baa1-103">Hogyan használja a Service Bus-üzenettémák és előfizetések a Node.js</span><span class="sxs-lookup"><span data-stu-id="0baa1-103">How to Use Service Bus topics and subscriptions with Node.js</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="0baa1-104">Ez az útmutató ismerteti a Service Bus-üzenettémák és előfizetések a Node.js-alkalmazások használatáról.</span><span class="sxs-lookup"><span data-stu-id="0baa1-104">This guide describes how to use Service Bus topics and subscriptions from Node.js applications.</span></span> <span data-ttu-id="0baa1-105">Az ismertetett forgatókönyvek **üzenettémák és előfizetések létrehozása**, **előfizetés-szűrők létrehozása**, **üzenetküldésre** egy témakörbe **fogadása előfizetés üzeneteit**, és **üzenettémák és előfizetések törlése**.</span><span class="sxs-lookup"><span data-stu-id="0baa1-105">The scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages** to a topic, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="0baa1-106">Az üzenettémakörökkel és előfizetésekkel kapcsolatos további információkért lásd a [További lépések](#next-steps) szakaszt.</span><span class="sxs-lookup"><span data-stu-id="0baa1-106">For more information about topics and subscriptions, see the [Next steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="0baa1-107">Node.js alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0baa1-107">Create a Node.js application</span></span>
<span data-ttu-id="0baa1-108">Üres Node.js-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0baa1-108">Create a blank Node.js application.</span></span> <span data-ttu-id="0baa1-109">A Node.js-alkalmazás létrehozása, lásd: [létrehozása és központi telepítése egy Node.js-alkalmazás az Azure webhelyén], [Node.js felhőalapú szolgáltatás] [ Node.js Cloud Service] Windows használatával PowerShell, vagy a webhelyet a WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="0baa1-109">For instructions on creating a Node.js application, see [Create and deploy a Node.js application to an Azure Web Site], [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell, or Web Site with WebMatrix.</span></span>

## <a name="configure-your-application-to-use-service-bus"></a><span data-ttu-id="0baa1-110">Állítsa be az alkalmazását, Service Bus használatára</span><span class="sxs-lookup"><span data-stu-id="0baa1-110">Configure your application to use Service Bus</span></span>
<span data-ttu-id="0baa1-111">A Service Bus használatához le kell töltenie a Node.js Azure-csomag.</span><span class="sxs-lookup"><span data-stu-id="0baa1-111">To use Service Bus, download the Node.js Azure package.</span></span> <span data-ttu-id="0baa1-112">Ez a csomag tartalmaz egy szalagtár szerepel, amely a Service Bus REST-szolgáltatásokkal kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="0baa1-112">This package includes a set of libraries that communicate with the Service Bus REST services.</span></span>

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a><span data-ttu-id="0baa1-113">Csomópont Package Manager (NPM) használja a csomag beszerzése</span><span class="sxs-lookup"><span data-stu-id="0baa1-113">Use Node Package Manager (NPM) to obtain the package</span></span>
1. <span data-ttu-id="0baa1-114">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac) vagy **Bash** (Unix), lépjen abba a mappába, amelyben létrehozta a mintaalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="0baa1-114">Use a command-line interface such as **PowerShell** (Windows,) **Terminal** (Mac,) or **Bash** (Unix), navigate to the folder where you created your sample application.</span></span>
2. <span data-ttu-id="0baa1-115">Típus **npm telepítése azure** a parancsablakban, amely eredményez a következő kimeneti:</span><span class="sxs-lookup"><span data-stu-id="0baa1-115">Type **npm install azure** in the command window, which should result in the following output:</span></span>

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
3. <span data-ttu-id="0baa1-116">Manuálisan futtatható a **ls** parancs futtatásával ellenőrizze, hogy egy **csomópont\_modulok** mappa hozták létre.</span><span class="sxs-lookup"><span data-stu-id="0baa1-116">You can manually run the **ls** command to verify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="0baa1-117">A mappában található a **azure** csomag, amely tartalmazza a Service Bus-üzenettémakörök eléréséhez szükséges könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="0baa1-117">Inside that folder find the **azure** package, which contains the libraries you need to access Service Bus topics.</span></span>

### <a name="import-the-module"></a><span data-ttu-id="0baa1-118">A modul importálása</span><span class="sxs-lookup"><span data-stu-id="0baa1-118">Import the module</span></span>
<span data-ttu-id="0baa1-119">A Jegyzettömbben vagy más szövegszerkesztőben, adja hozzá a következő elejéhez a **server.js** fájl tartalmát:</span><span class="sxs-lookup"><span data-stu-id="0baa1-119">Using Notepad or another text editor, add the following to the top of the **server.js** file of the application:</span></span>

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="0baa1-120">A Service Bus-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="0baa1-120">Set up a Service Bus connection</span></span>
<span data-ttu-id="0baa1-121">Az Azure modul olvassa be a környezeti változók `AZURE_SERVICEBUS_NAMESPACE` és `AZURE_SERVICEBUS_ACCESS_KEY` a Service Bus való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="0baa1-121">The Azure module reads the environment variables `AZURE_SERVICEBUS_NAMESPACE` and `AZURE_SERVICEBUS_ACCESS_KEY` for information required to connect to Service Bus.</span></span> <span data-ttu-id="0baa1-122">Ha ezek a környezeti változók nem, meg kell adnia a fiókadatokat, meghívásakor `createServiceBusService`.</span><span class="sxs-lookup"><span data-stu-id="0baa1-122">If these environment variables are not set, you must specify the account information when calling `createServiceBusService`.</span></span>

<span data-ttu-id="0baa1-123">A környezeti változók beállítása az Azure-Felhőszolgáltatásban példát talál [Node.js Felhőszolgáltatás tárolási][Node.js Cloud Service with Storage].</span><span class="sxs-lookup"><span data-stu-id="0baa1-123">For an example of setting the environment variables for an Azure Cloud Service, see [Node.js Cloud Service with Storage][Node.js Cloud Service with Storage].</span></span>

<span data-ttu-id="0baa1-124">Például egy Azure-webhely környezeti változókkal, lásd: [Node.js-webalkalmazás tárolóval][Node.js Web Application with Storage].</span><span class="sxs-lookup"><span data-stu-id="0baa1-124">For an example of setting the environment variables for an Azure Website, see [Node.js Web Application with Storage][Node.js Web Application with Storage].</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="0baa1-125">Üzenettémakör létrehozása</span><span class="sxs-lookup"><span data-stu-id="0baa1-125">Create a topic</span></span>
<span data-ttu-id="0baa1-126">A **ServiceBusService** objektum lehetővé teszi a témakörök együttműködni.</span><span class="sxs-lookup"><span data-stu-id="0baa1-126">The **ServiceBusService** object enables you to work with topics.</span></span> <span data-ttu-id="0baa1-127">Az alábbi kód létrehoz egy **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="0baa1-127">The following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="0baa1-128">Az tetején adja hozzá a **server.js** fájl, az utasítást, hogy az azure modul importálása után:</span><span class="sxs-lookup"><span data-stu-id="0baa1-128">Add it near the top of the **server.js** file, after the statement to import the azure module:</span></span>

```javascript
var serviceBusService = azure.createServiceBusService();
```

<span data-ttu-id="0baa1-129">Meghívásával `createTopicIfNotExists` a a **ServiceBusService** objektum, a megadott témakör az eredmény (ha van ilyen), vagy egy új téma a megadott néven jön létre.</span><span class="sxs-lookup"><span data-stu-id="0baa1-129">By calling `createTopicIfNotExists` on the **ServiceBusService** object, the specified topic will be returned (if it exists,) or a new topic with the specified name will be created.</span></span> <span data-ttu-id="0baa1-130">A következő kódban `createTopicIfNotExists` létrehozása vagy csatlakozás nevű üzenettémakör `MyTopic`:</span><span class="sxs-lookup"><span data-stu-id="0baa1-130">The following code uses `createTopicIfNotExists` to create or connect to the topic named `MyTopic`:</span></span>

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

<span data-ttu-id="0baa1-131">A `createServiceBusService` metódus is támogatja a további beállításokat, amelyek lehetővé teszik a bírálja felül az alapértelmezett témakör beállításokat, például üzenet time to live vagy maximális témakör ezen méretét.</span><span class="sxs-lookup"><span data-stu-id="0baa1-131">The `createServiceBusService` method also supports additional options, which enable you to override default topic settings such as message time to live or maximum topic size.</span></span> <span data-ttu-id="0baa1-132">Az alábbi példa méretét állítja be maximális témakör 5GB 1 perces élő időpontja:</span><span class="sxs-lookup"><span data-stu-id="0baa1-132">The following example sets the maximum topic size to 5GB with a time to live of 1 minute:</span></span>

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

### <a name="filters"></a><span data-ttu-id="0baa1-133">Szűrők</span><span class="sxs-lookup"><span data-stu-id="0baa1-133">Filters</span></span>
<span data-ttu-id="0baa1-134">Választható szűrési műveletek használatával végrehajtott műveletek alkalmazhatók **ServiceBusService**.</span><span class="sxs-lookup"><span data-stu-id="0baa1-134">Optional filtering operations can be applied to operations performed using **ServiceBusService**.</span></span> <span data-ttu-id="0baa1-135">Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. A metódus aláírása megvalósító objektumok szűrők a következők:</span><span class="sxs-lookup"><span data-stu-id="0baa1-135">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```javascript
function handle (requestOptions, next)
```

<span data-ttu-id="0baa1-136">Előfeldolgozása kérelmet a beállítások elvégzése után a metódushívások `next`, átadja a következő aláírással rendelkező visszahívás:</span><span class="sxs-lookup"><span data-stu-id="0baa1-136">After performing preprocessing on the request options, the method calls `next`, passing a callback with the following signature:</span></span>

```javascript
function (returnObject, finalCallback, next)
```

<span data-ttu-id="0baa1-137">A visszahívási, és feldolgozás után a `returnObject` (válasza a kérés a kiszolgáló), a visszahívás kell rá folytatni más szűrők mellett invoke vagy a meghívása `finalCallback` ellenkező esetben a szolgáltatás hívás befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="0baa1-137">In this callback, and after processing the `returnObject` (the response from the request to the server), the callback needs to either invoke next if it exists to continue processing other filters or invoke `finalCallback` otherwise, to end the service invocation.</span></span>

<span data-ttu-id="0baa1-138">Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el az Azure SDK for Node.js, a **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="0baa1-138">Two filters that implement retry logic are included with the Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="0baa1-139">A következő létrehoz egy **ServiceBusService** objektum, amely használja a **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="0baa1-139">The following creates a **ServiceBusService** object that uses the **ExponentialRetryPolicyFilter**:</span></span>

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a><span data-ttu-id="0baa1-140">Előfizetések létrehozása</span><span class="sxs-lookup"><span data-stu-id="0baa1-140">Create subscriptions</span></span>
<span data-ttu-id="0baa1-141">Üzenettémakör-előfizetéseket is jönnek létre a **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="0baa1-141">Topic subscriptions are also created with the **ServiceBusService** object.</span></span> <span data-ttu-id="0baa1-142">Előfizetések vannak nevezve, és rendelkezhetnek olyan szűrőkkel, amelyek az előfizetés virtuális üzenetsorában kézbesített üzenetek korlátoz.</span><span class="sxs-lookup"><span data-stu-id="0baa1-142">Subscriptions are named and can have an optional filter that restricts the set of messages delivered to the subscription's virtual queue.</span></span>

> [!NOTE]
> <span data-ttu-id="0baa1-143">Előfizetések állandó, és továbbra is létezik, vagy csak, vagy a következő témakörben társítva, a rendszer törli.</span><span class="sxs-lookup"><span data-stu-id="0baa1-143">Subscriptions are persistent and will continue to exist until either they, or the topic they are associated with, are deleted.</span></span> <span data-ttu-id="0baa1-144">Ha az alkalmazás előfizetés létrehozása a logikát tartalmaz, azt kell először ellenőrizze, hogy az előfizetés már léteznek használatával a `getSubscription` metódust.</span><span class="sxs-lookup"><span data-stu-id="0baa1-144">If your application contains logic to create a subscription, it should first check if the subscription already exists by using the `getSubscription` method.</span></span>
>
>

### <a name="create-a-subscription-with-the-default-matchall-filter"></a><span data-ttu-id="0baa1-145">Előfizetés létrehozása az alapértelmezett (MatchAll) szűrővel</span><span class="sxs-lookup"><span data-stu-id="0baa1-145">Create a subscription with the default (MatchAll) filter</span></span>
<span data-ttu-id="0baa1-146">A **MatchAll** szűrő az alapértelmezett szűrő, amely akkor használatos, ha nincs meghatározva szűrő egy új előfizetés létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="0baa1-146">The **MatchAll** filter is the default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="0baa1-147">Ha a **MatchAll** szűrővel, a témakörbe közzétett összes üzenetet kerülnek, az előfizetés virtuális üzenetsorában.</span><span class="sxs-lookup"><span data-stu-id="0baa1-147">When the **MatchAll** filter is used, all messages published to the topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="0baa1-148">A következő példa egy "AllMessages" nevű előfizetést hoz létre, és használja az alapértelmezett **MatchAll** szűrő.</span><span class="sxs-lookup"><span data-stu-id="0baa1-148">The following example creates a subscription named 'AllMessages' and uses the default **MatchAll** filter.</span></span>

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="0baa1-149">Előfizetések létrehozása szűrőkkel</span><span class="sxs-lookup"><span data-stu-id="0baa1-149">Create subscriptions with filters</span></span>
<span data-ttu-id="0baa1-150">Szűrők, amelyek lehetővé teszik, hogy a hatókör, amely egy témakörbe küldött üzenetek kell jelenik meg egy adott üzenettémakör-előfizetésben belül is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="0baa1-150">You can also create filters that allow you to scope which messages sent to a topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="0baa1-151">A szűrő előfizetések által támogatott legrugalmasabb típusú a **SqlFilter**, amely az SQL92 egy részhalmazát valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="0baa1-151">The most flexible type of filter supported by subscriptions is the **SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="0baa1-152">Az SQL-szűrők az üzenettémába közzétett üzenetek tulajdonságain működnek.</span><span class="sxs-lookup"><span data-stu-id="0baa1-152">SQL filters operate on the properties of the messages that are published to the topic.</span></span> <span data-ttu-id="0baa1-153">Az SQL-szűrőkkel használható kifejezésekkel kapcsolatos további információkért tekintse át a [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] szintaxist.</span><span class="sxs-lookup"><span data-stu-id="0baa1-153">For more details about the expressions that can be used with a SQL filter, review the [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="0baa1-154">Szűrők használatával is hozzáadhatók előfizetés a `createRule` metódusában a **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="0baa1-154">Filters can be added to a subscription by using the `createRule` method of the **ServiceBusService** object.</span></span> <span data-ttu-id="0baa1-155">Ez a módszer lehetővé teszi új szűrők hozzáadása egy meglévő előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="0baa1-155">This method allows you to add new filters to an existing subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="0baa1-156">Mivel minden új előfizetés automatikusan alkalmazza az alapértelmezett szűrő, távolítsa el az alapértelmezett szűrő vagy a **MatchAll** felülírják más szűrőket is megadhat.</span><span class="sxs-lookup"><span data-stu-id="0baa1-156">Because the default filter is applied automatically to all new subscriptions, you must first remove the default filter or the **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="0baa1-157">Az alapértelmezett szabály eltávolításához használja a `deleteRule` metódusában a **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="0baa1-157">You can remove the default rule by using the `deleteRule` method of the **ServiceBusService** object.</span></span>
>
>

<span data-ttu-id="0baa1-158">A következő példa egy nevű előfizetést hoz létre `HighMessages` rendelkező egy **SqlFilter** , amely csak választja ki, amelyek egyéni üzenetek `messagenumber` tulajdonságának értéke nagyobb, mint 3:</span><span class="sxs-lookup"><span data-stu-id="0baa1-158">The following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `messagenumber` property greater than 3:</span></span>

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

<span data-ttu-id="0baa1-159">Hasonlóképpen, a következő példa egy nevű előfizetést hoz létre `LowMessages` rendelkező egy **SqlFilter** , amely csak rendelkező üzeneteket választja ki egy `messagenumber` tulajdonságának értéke kisebb vagy egyenlő, mint 3:</span><span class="sxs-lookup"><span data-stu-id="0baa1-159">Similarly, the following example creates a subscription named `LowMessages` with a **SqlFilter** that only selects messages that have a `messagenumber` property less than or equal to 3:</span></span>

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

<span data-ttu-id="0baa1-160">Ha egy most üzenettel `MyTopic`, azt mindig kézbesíti a rendszer az előfizetett fogadó számára a `AllMessages` üzenettémakör-előfizetésre, és szelektív módon kézbesíti a a `HighMessages` és `LowMessages` témakör előfizetések () attól függően, az üzenet tartalma).</span><span class="sxs-lookup"><span data-stu-id="0baa1-160">When a message is now sent to `MyTopic`, it will always be delivered to receivers subscribed to the `AllMessages` topic subscription, and selectively delivered to receivers subscribed to the `HighMessages` and `LowMessages` topic subscriptions (depending upon the message content).</span></span>

## <a name="how-to-send-messages-to-a-topic"></a><span data-ttu-id="0baa1-161">Üzenetek küldése egy témakörbe</span><span class="sxs-lookup"><span data-stu-id="0baa1-161">How to send messages to a topic</span></span>
<span data-ttu-id="0baa1-162">A Service Bus-témakörbe való üzenetküldéshez az alkalmazást kell használnia a `sendTopicMessage` metódusában a **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="0baa1-162">To send a message to a Service Bus topic, your application must use the `sendTopicMessage` method of the **ServiceBusService** object.</span></span>
<span data-ttu-id="0baa1-163">Service Bus-üzenettémakörök küldött üzenetek **BrokeredMessage** objektumok.</span><span class="sxs-lookup"><span data-stu-id="0baa1-163">Messages sent to Service Bus topics are **BrokeredMessage** objects.</span></span>
<span data-ttu-id="0baa1-164">**BrokeredMessage** objektumok rendelkeznek egy szabványos tulajdonságkészlettel (például `Label` és `TimeToLive`), amellyel az egyéni alkalmazásspecifikus tulajdonságokat, valamint egy karakterláncadatokat törzsét.</span><span class="sxs-lookup"><span data-stu-id="0baa1-164">**BrokeredMessage** objects have a set of standard properties (such as `Label` and `TimeToLive`), a dictionary that is used to hold custom application-specific properties, and a body of string data.</span></span> <span data-ttu-id="0baa1-165">Az alkalmazás beállíthatja az üzenet törzsét úgy, hogy a karakterlánc-érték a `sendTopicMessage` és a szabványos tulajdonságait tölti fel a alapértelmezett értékek szerint szükséges.</span><span class="sxs-lookup"><span data-stu-id="0baa1-165">An application can set the body of the message by passing a string value to the `sendTopicMessage` and any required standard properties will be populated by default values.</span></span>

<span data-ttu-id="0baa1-166">A következő példa bemutatja, hogyan küldhető öt tesztüzenet az `MyTopic`.</span><span class="sxs-lookup"><span data-stu-id="0baa1-166">The following example demonstrates how to send five test messages to `MyTopic`.</span></span> <span data-ttu-id="0baa1-167">Vegye figyelembe, hogy a `messagenumber` minden üzenetet tulajdonság értékének meg az a ciklus ismétléseinek számától függően változik (Ez határozza meg, hogy melyik előfizetések fogják megkapni):</span><span class="sxs-lookup"><span data-stu-id="0baa1-167">Note that the `messagenumber` property value of each message varies on the iteration of the loop (this will determine which subscriptions receive it):</span></span>

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

<span data-ttu-id="0baa1-168">A Service Bus-üzenettémakörök a [Standard csomagban](service-bus-premium-messaging.md) legfeljebb 256 KB, a [Prémium csomagban](service-bus-premium-messaging.md) legfeljebb 1 MB méretű üzeneteket támogatnak.</span><span class="sxs-lookup"><span data-stu-id="0baa1-168">Service Bus topics support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="0baa1-169">A szabványos és az egyéni alkalmazástulajdonságokat tartalmazó fejléc mérete legfeljebb 64 KB lehet.</span><span class="sxs-lookup"><span data-stu-id="0baa1-169">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="0baa1-170">A témakörökben tárolt üzenetek száma korlátlan, a témakörök által tárolt üzenetek teljes mérete azonban korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="0baa1-170">There is no limit on the number of messages held in a topic but there is a cap on the total size of the messages held by a topic.</span></span> <span data-ttu-id="0baa1-171">A témakör ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="0baa1-171">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="0baa1-172">Üzenetek fogadása egy előfizetés</span><span class="sxs-lookup"><span data-stu-id="0baa1-172">Receive messages from a subscription</span></span>
<span data-ttu-id="0baa1-173">Üzenetek fogadása egy előfizetés használatával a `receiveSubscriptionMessage` metódust a **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="0baa1-173">Messages are received from a subscription using the `receiveSubscriptionMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="0baa1-174">Alapértelmezés szerint az üzenetek törlődnek az előfizetésből, olvasott; azonban (betekintés) olvasni és zárolni az üzenetet úgy, hogy a nem kötelező paraméter az előfizetés törlése nélkül `isPeekLock` való **igaz**.</span><span class="sxs-lookup"><span data-stu-id="0baa1-174">By default, messages are deleted from the subscription as they are read; however, you can read (peek) and lock the message without deleting it from the subscription by setting the optional parameter `isPeekLock` to **true**.</span></span>

<span data-ttu-id="0baa1-175">Olvasási és az üzenet törlése a fogadási művelet részeként alapértelmezett viselkedése a legegyszerűbb modell, és forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet hiba esetén a legjobban.</span><span class="sxs-lookup"><span data-stu-id="0baa1-175">The default behavior of reading and deleting the message as part of the receive operation is the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="0baa1-176">Ennek megértéséhez képzeljen el egy forgatókönyvet, amelyben a fogyasztó kiad egy fogadási kérést, majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="0baa1-176">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="0baa1-177">Mivel a Service Bus csak fel az üzenetet, feldolgozottként, majd az alkalmazás újraindításakor és megkezdése az üzenetek fel újra, amikor ki fogja hagyni a az összeomlás előtt feldolgozott üzenetet.</span><span class="sxs-lookup"><span data-stu-id="0baa1-177">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="0baa1-178">Ha a `isPeekLock` paraméter értéke **igaz**, a receive két szakaszból álló művelet, amely lehetővé teszi az alkalmazások támogatását, amelyek működését zavarják a hiányzó üzenetek lesz.</span><span class="sxs-lookup"><span data-stu-id="0baa1-178">If the `isPeekLock` parameter is set to **true**, the receive becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="0baa1-179">Amikor a Service Bus fogad egy kérést, megkeresi és zárolja a következő feldolgozandó üzenetet, hogy más fogyasztók ne tudják fogadni, majd visszaadja az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="0baa1-179">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span>
<span data-ttu-id="0baa1-180">Miután az alkalmazás befejezi az üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja a második a fogadási folyamat meghívásával **deleteMessage** metódus és a törlendő az üzenet egy a paraméter.</span><span class="sxs-lookup"><span data-stu-id="0baa1-180">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling **deleteMessage** method and providing the message to be deleted as a parameter.</span></span> <span data-ttu-id="0baa1-181">A **deleteMessage** metódus lesz használnak az üzenetet, és távolítsa el az előfizetésből.</span><span class="sxs-lookup"><span data-stu-id="0baa1-181">The **deleteMessage** method will mark the message as being consumed and remove it from the subscription.</span></span>

<span data-ttu-id="0baa1-182">A következő példa bemutatja, hogyan lehet üzeneteket fogadni, és a feldolgozott használatával `receiveSubscriptionMessage`.</span><span class="sxs-lookup"><span data-stu-id="0baa1-182">The following example demonstrates how messages can be received and processed using `receiveSubscriptionMessage`.</span></span> <span data-ttu-id="0baa1-183">A példa első fogadása és üzenet törlése a "LowMessages" előfizetés, és majd üzenetet kap a "HighMessages" előfizetést a `isPeekLock` igaz értékre állítva.</span><span class="sxs-lookup"><span data-stu-id="0baa1-183">The example first receives and deletes a message from the 'LowMessages' subscription, and then receives a message from the 'HighMessages' subscription using `isPeekLock` set to true.</span></span> <span data-ttu-id="0baa1-184">Majd törli az üzenet használatával `deleteMessage`:</span><span class="sxs-lookup"><span data-stu-id="0baa1-184">It then deletes the message using `deleteMessage`:</span></span>

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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="0baa1-185">Az alkalmazás-összeomlások és nem olvasható üzenetek kezelése</span><span class="sxs-lookup"><span data-stu-id="0baa1-185">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="0baa1-186">A Service Bus olyan funkciókat biztosít, amelyekkel zökkenőmentesen helyreállíthatja az alkalmazás hibáit vagy az üzenetek feldolgozásának nehézségeit.</span><span class="sxs-lookup"><span data-stu-id="0baa1-186">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="0baa1-187">Ha egy fogadó alkalmazás nem tudja feldolgozni az üzenetet valamilyen okból kifolyólag, majd akkor meghívhatja a `unlockMessage` metódust a **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="0baa1-187">If a receiver application is unable to process the message for some reason, then it can call the `unlockMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="0baa1-188">Ennek hatására a Service Bus feloldja az üzenet az előfizetésen belüli zárolását, és lehetővé teszi a felhasználó az alkalmazás vagy egy másik fogyasztó alkalmazás általi ismételt fogadását.</span><span class="sxs-lookup"><span data-stu-id="0baa1-188">This will cause Service Bus to unlock the message within the subscription and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="0baa1-189">Emellett van lévő az előfizetéshez társított időtúllépés, és ha az alkalmazás nem tudja feldolgozni az üzenetet a zárolási előtt időkorlát lejárta (például, ha az alkalmazás összeomlik), majd a Service Bus automatikusan feloldja az üzenet és lehetővé teszi az újbóli fogadását.</span><span class="sxs-lookup"><span data-stu-id="0baa1-189">There is also a timeout associated with a message locked within the subscription, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus unlocks the message automatically and makes it available to be received again.</span></span>

<span data-ttu-id="0baa1-190">Abban az esetben, ha az alkalmazás összeomlik, de előtte az üzenet feldolgozása után a `deleteMessage` metódust, akkor az üzenet újból kézbesítve lesz az alkalmazás amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="0baa1-190">In the event that the application crashes after processing the message but before the `deleteMessage` method is called, then the message will be redelivered to the application when it restarts.</span></span> <span data-ttu-id="0baa1-191">Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben a a ugyanazon üzenet újbóli kézbesítése is lehet.</span><span class="sxs-lookup"><span data-stu-id="0baa1-191">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="0baa1-192">Ha a forgatókönyvben nem lehetségesek a duplikált üzenetek, akkor az alkalmazásfejlesztőnek további logikát kell az alkalmazásba építenie az üzenetek ismételt kézbesítésének kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="0baa1-192">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="0baa1-193">Ez gyakran érhető el, használja a **MessageId** az üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="0baa1-193">This is often achieved using the **MessageId** property of the message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="0baa1-194">Témakörök és előfizetések törlése</span><span class="sxs-lookup"><span data-stu-id="0baa1-194">Delete topics and subscriptions</span></span>
<span data-ttu-id="0baa1-195">Üzenettémák és előfizetések következetesek, és explicit módon kell lennie vagy törölte a [Azure-portálon] [ Azure portal] vagy programon keresztül.</span><span class="sxs-lookup"><span data-stu-id="0baa1-195">Topics and subscriptions are persistent, and must be explicitly deleted either through the [Azure portal][Azure portal] or programmatically.</span></span>
<span data-ttu-id="0baa1-196">A következő példa bemutatja, hogyan nevű üzenettémakör törlése `MyTopic`:</span><span class="sxs-lookup"><span data-stu-id="0baa1-196">The following example demonstrates how to delete the topic named `MyTopic`:</span></span>

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

<span data-ttu-id="0baa1-197">A témakör is törlése eltávolítja az adott témakörre regisztrált előfizetése.</span><span class="sxs-lookup"><span data-stu-id="0baa1-197">Deleting a topic will also delete any subscriptions that are registered with the topic.</span></span> <span data-ttu-id="0baa1-198">Az előfizetések független módon is törölhetők.</span><span class="sxs-lookup"><span data-stu-id="0baa1-198">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="0baa1-199">A következő példa bemutatja, hogyan nevű előfizetés törlése `HighMessages` a a `MyTopic` témakör:</span><span class="sxs-lookup"><span data-stu-id="0baa1-199">The following example shows how to delete a subscription named `HighMessages` from the `MyTopic` topic:</span></span>

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a><span data-ttu-id="0baa1-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0baa1-200">Next Steps</span></span>
<span data-ttu-id="0baa1-201">Most, hogy megismerte a Service Bus-üzenettémakörök alapjait, az alábbi hivatkozásokból további.</span><span class="sxs-lookup"><span data-stu-id="0baa1-201">Now that you've learned the basics of Service Bus topics, follow these links to learn more.</span></span>

* <span data-ttu-id="0baa1-202">Lásd: [üzenetsorok, témakörök és előfizetések][Queues, topics, and subscriptions].</span><span class="sxs-lookup"><span data-stu-id="0baa1-202">See [Queues, topics, and subscriptions][Queues, topics, and subscriptions].</span></span>
* <span data-ttu-id="0baa1-203">Az [SqlFilter][SqlFilter] API-referenciája.</span><span class="sxs-lookup"><span data-stu-id="0baa1-203">API reference for [SqlFilter][SqlFilter].</span></span>
* <span data-ttu-id="0baa1-204">Látogasson el a [Azure SDK for csomópont] [ Azure SDK for Node] GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="0baa1-204">Visit the [Azure SDK for Node][Azure SDK for Node] repository on GitHub.</span></span>

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[létrehozása és központi telepítése egy Node.js-alkalmazás az Azure webhelyén]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
