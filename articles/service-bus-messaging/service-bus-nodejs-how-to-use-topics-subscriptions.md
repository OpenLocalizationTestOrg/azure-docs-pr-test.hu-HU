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
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-nodejs"></a><span data-ttu-id="c653d-103">Hogyan tooUse Service Bus üzenettémák és előfizetések a Node.js</span><span class="sxs-lookup"><span data-stu-id="c653d-103">How tooUse Service Bus topics and subscriptions with Node.js</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="c653d-104">Ez az útmutató ismerteti, hogyan toouse Service Bus üzenettémák és előfizetések a Node.js-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c653d-104">This guide describes how toouse Service Bus topics and subscriptions from Node.js applications.</span></span> <span data-ttu-id="c653d-105">hello tárgyalt forgatókönyvekben szerepel a **üzenettémák és előfizetések létrehozása**, **előfizetés-szűrők létrehozása**, **üzenetküldésre** tooa témakör **fogadása előfizetés üzeneteit**, és **üzenettémák és előfizetések törlése**.</span><span class="sxs-lookup"><span data-stu-id="c653d-105">hello scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages** tooa topic, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="c653d-106">Üzenettémakörökkel és előfizetésekkel kapcsolatos további információkért lásd: hello [további lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c653d-106">For more information about topics and subscriptions, see hello [Next steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="c653d-107">Node.js alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c653d-107">Create a Node.js application</span></span>
<span data-ttu-id="c653d-108">Üres Node.js-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c653d-108">Create a blank Node.js application.</span></span> <span data-ttu-id="c653d-109">Node.js-alkalmazás létrehozása, lásd: [létrehozása és központi telepítése egy Node.js-alkalmazás tooan Azure webhelyére], [Node.js felhőalapú szolgáltatás] [ Node.js Cloud Service] Windows használatával PowerShell, vagy a webhelyet a WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c653d-109">For instructions on creating a Node.js application, see [Create and deploy a Node.js application tooan Azure Web Site], [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell, or Web Site with WebMatrix.</span></span>

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="c653d-110">Az alkalmazás toouse Service Bus konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c653d-110">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="c653d-111">a Service Bus toouse hello Node.js Azure-csomag letöltése.</span><span class="sxs-lookup"><span data-stu-id="c653d-111">toouse Service Bus, download hello Node.js Azure package.</span></span> <span data-ttu-id="c653d-112">Ez a csomag tartalmaz egy szalagtár szerepel, amely hello Service Bus REST szolgáltatásokkal kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="c653d-112">This package includes a set of libraries that communicate with hello Service Bus REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="c653d-113">Csomópont Package Manager (NPM) tooobtain hello csomag használata</span><span class="sxs-lookup"><span data-stu-id="c653d-113">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="c653d-114">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac) vagy **Bash** (Unix), keresse meg a toohello mappa, amelyben létrehozta a mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c653d-114">Use a command-line interface such as **PowerShell** (Windows,) **Terminal** (Mac,) or **Bash** (Unix), navigate toohello folder where you created your sample application.</span></span>
2. <span data-ttu-id="c653d-115">Típus **npm telepítése azure** hello parancssori ablakban, amely eredményez a következő kimeneti hello:</span><span class="sxs-lookup"><span data-stu-id="c653d-115">Type **npm install azure** in hello command window, which should result in hello following output:</span></span>

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
3. <span data-ttu-id="c653d-116">Manuális futtatásával hello **ls** parancs tooverify, amely egy **csomópont\_modulok** mappa hozták létre.</span><span class="sxs-lookup"><span data-stu-id="c653d-116">You can manually run hello **ls** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="c653d-117">A mappában található a **azure** csomagot, amely tartalmaz hello szalagtárak tooaccess Service Bus-üzenettémakörök van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c653d-117">Inside that folder find the **azure** package, which contains hello libraries you need tooaccess Service Bus topics.</span></span>

### <a name="import-hello-module"></a><span data-ttu-id="c653d-118">Importálás hello modul</span><span class="sxs-lookup"><span data-stu-id="c653d-118">Import hello module</span></span>
<span data-ttu-id="c653d-119">A Jegyzettömbben vagy más szövegszerkesztőben, adja hozzá a következő toohello felső részén hello hello **server.js** hello alkalmazás fájlt:</span><span class="sxs-lookup"><span data-stu-id="c653d-119">Using Notepad or another text editor, add hello following toohello top of hello **server.js** file of hello application:</span></span>

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="c653d-120">A Service Bus-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="c653d-120">Set up a Service Bus connection</span></span>
<span data-ttu-id="c653d-121">hello Azure a modul olvassa be hello környezeti változók `AZURE_SERVICEBUS_NAMESPACE` és `AZURE_SERVICEBUS_ACCESS_KEY` információt tooconnect tooService Bus szükséges.</span><span class="sxs-lookup"><span data-stu-id="c653d-121">hello Azure module reads hello environment variables `AZURE_SERVICEBUS_NAMESPACE` and `AZURE_SERVICEBUS_ACCESS_KEY` for information required tooconnect tooService Bus.</span></span> <span data-ttu-id="c653d-122">Ha ezek a környezeti változók nem, meg kell adnia hello fiók meghívásakor `createServiceBusService`.</span><span class="sxs-lookup"><span data-stu-id="c653d-122">If these environment variables are not set, you must specify hello account information when calling `createServiceBusService`.</span></span>

<span data-ttu-id="c653d-123">Hello környezeti változók beállítása az Azure-Felhőszolgáltatásban példát talál [Node.js Felhőszolgáltatás tárolási][Node.js Cloud Service with Storage].</span><span class="sxs-lookup"><span data-stu-id="c653d-123">For an example of setting hello environment variables for an Azure Cloud Service, see [Node.js Cloud Service with Storage][Node.js Cloud Service with Storage].</span></span>

<span data-ttu-id="c653d-124">Az Azure-webhely hello környezeti változók beállítása példát lásd: [Node.js-webalkalmazás tárolóval][Node.js Web Application with Storage].</span><span class="sxs-lookup"><span data-stu-id="c653d-124">For an example of setting hello environment variables for an Azure Website, see [Node.js Web Application with Storage][Node.js Web Application with Storage].</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="c653d-125">Üzenettémakör létrehozása</span><span class="sxs-lookup"><span data-stu-id="c653d-125">Create a topic</span></span>
<span data-ttu-id="c653d-126">Hello **ServiceBusService** objektum teszi lehetővé a témakörök toowork.</span><span class="sxs-lookup"><span data-stu-id="c653d-126">hello **ServiceBusService** object enables you toowork with topics.</span></span> <span data-ttu-id="c653d-127">Az alábbi kód létrehoz egy **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="c653d-127">The following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="c653d-128">Adja hozzá az tetején hello **server.js** fájl hello utasítás tooimport hello azure modul után:</span><span class="sxs-lookup"><span data-stu-id="c653d-128">Add it near the top of hello **server.js** file, after hello statement tooimport hello azure module:</span></span>

```javascript
var serviceBusService = azure.createServiceBusService();
```

<span data-ttu-id="c653d-129">Meghívásával `createTopicIfNotExists` a hello **ServiceBusService** objektum, hello megadva, a témakör az eredmény (ha van ilyen), vagy egy új téma hello megadott névvel jön létre.</span><span class="sxs-lookup"><span data-stu-id="c653d-129">By calling `createTopicIfNotExists` on hello **ServiceBusService** object, hello specified topic will be returned (if it exists,) or a new topic with hello specified name will be created.</span></span> <span data-ttu-id="c653d-130">hello következő kódban `createTopicIfNotExists` toocreate vagy kapcsolódási nevű toohello témakör `MyTopic`:</span><span class="sxs-lookup"><span data-stu-id="c653d-130">hello following code uses `createTopicIfNotExists` toocreate or connect toohello topic named `MyTopic`:</span></span>

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

<span data-ttu-id="c653d-131">Hello `createServiceBusService` metódus is támogatja a további beállításokat, amelyek lehetővé teszik a témakör alapbeállítások toooverride például üzenet time to live vagy maximális témakör ezen méretét.</span><span class="sxs-lookup"><span data-stu-id="c653d-131">hello `createServiceBusService` method also supports additional options, which enable you toooverride default topic settings such as message time to live or maximum topic size.</span></span> <span data-ttu-id="c653d-132">hello alábbi mintakód hello maximális témakör mérete too5GB idővel rendelkező toolive 1 perces:</span><span class="sxs-lookup"><span data-stu-id="c653d-132">hello following example sets hello maximum topic size too5GB with a time toolive of 1 minute:</span></span>

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

### <a name="filters"></a><span data-ttu-id="c653d-133">Szűrők</span><span class="sxs-lookup"><span data-stu-id="c653d-133">Filters</span></span>
<span data-ttu-id="c653d-134">Választható szűrési műveletek lehet segítségével alkalmazott toooperations **ServiceBusService**.</span><span class="sxs-lookup"><span data-stu-id="c653d-134">Optional filtering operations can be applied toooperations performed using **ServiceBusService**.</span></span> <span data-ttu-id="c653d-135">Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. Szűrők hello aláírással metódus megvalósító objektumok a következők:</span><span class="sxs-lookup"><span data-stu-id="c653d-135">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```javascript
function handle (requestOptions, next)
```

<span data-ttu-id="c653d-136">A hello lehetőségek előfeldolgozása elvégzése után hello metódushívások `next`, egy visszahívási átadja a aláírását hello:</span><span class="sxs-lookup"><span data-stu-id="c653d-136">After performing preprocessing on hello request options, hello method calls `next`, passing a callback with hello following signature:</span></span>

```javascript
function (returnObject, finalCallback, next)
```

<span data-ttu-id="c653d-137">A visszahívási, és feldolgozás hello követően `returnObject` (hello hello kérelem toohello kiszolgálótól kapott válasz), hello visszahívási kell tooeither következő meghívni, ha más szűrők feldolgozása toocontinue létezik, vagy meghívása `finalCallback` ellenkező esetben tooend hello szolgáltatás hívása.</span><span class="sxs-lookup"><span data-stu-id="c653d-137">In this callback, and after processing hello `returnObject` (hello response from hello request toohello server), hello callback needs tooeither invoke next if it exists toocontinue processing other filters or invoke `finalCallback` otherwise, tooend hello service invocation.</span></span>

<span data-ttu-id="c653d-138">Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el a hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="c653d-138">Two filters that implement retry logic are included with hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="c653d-139">hello következő létrehoz egy **ServiceBusService** hello használó objektum **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="c653d-139">hello following creates a **ServiceBusService** object that uses hello **ExponentialRetryPolicyFilter**:</span></span>

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a><span data-ttu-id="c653d-140">Előfizetések létrehozása</span><span class="sxs-lookup"><span data-stu-id="c653d-140">Create subscriptions</span></span>
<span data-ttu-id="c653d-141">Üzenettémakör-előfizetéseket is jönnek létre hello **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="c653d-141">Topic subscriptions are also created with hello **ServiceBusService** object.</span></span> <span data-ttu-id="c653d-142">Előfizetések vannak nevezve, és rendelkezhetnek olyan szűrőkkel, amely korlátozza a hello toohello előfizetés virtuális üzenetsorában kézbesített üzenetek készletét.</span><span class="sxs-lookup"><span data-stu-id="c653d-142">Subscriptions are named and can have an optional filter that restricts hello set of messages delivered toohello subscription's virtual queue.</span></span>

> [!NOTE]
> <span data-ttu-id="c653d-143">Előfizetések állandó, és amíg vagy azok továbbra is tooexist, vagy hello témakör azok tartoznak, a rendszer törli.</span><span class="sxs-lookup"><span data-stu-id="c653d-143">Subscriptions are persistent and will continue tooexist until either they, or hello topic they are associated with, are deleted.</span></span> <span data-ttu-id="c653d-144">Ha az alkalmazás logika toocreate előfizetés tartalmaz, akkor először győződjön meg arról hogy hello előfizetés már létezik-e a a `getSubscription` metódust.</span><span class="sxs-lookup"><span data-stu-id="c653d-144">If your application contains logic toocreate a subscription, it should first check if hello subscription already exists by using the `getSubscription` method.</span></span>
>
>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="c653d-145">Előfizetés létrehozása hello alapértelmezett (MatchAll) szűrővel</span><span class="sxs-lookup"><span data-stu-id="c653d-145">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="c653d-146">Hello **MatchAll** szűrő hello alapértelmezett szűrő, amely akkor használatos, ha nincs meghatározva szűrő egy új előfizetés létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="c653d-146">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="c653d-147">Ha hello **MatchAll** szűrővel, minden üzenetek közzétett toohello témakör kerülnek, az előfizetés virtuális üzenetsorában.</span><span class="sxs-lookup"><span data-stu-id="c653d-147">When hello **MatchAll** filter is used, all messages published toohello topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="c653d-148">hello alábbi példa létrehoz egy "AllMessages" nevű előfizetést, és használja az alapértelmezett hello **MatchAll** szűrő.</span><span class="sxs-lookup"><span data-stu-id="c653d-148">hello following example creates a subscription named 'AllMessages' and uses hello default **MatchAll** filter.</span></span>

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="c653d-149">Előfizetések létrehozása szűrőkkel</span><span class="sxs-lookup"><span data-stu-id="c653d-149">Create subscriptions with filters</span></span>
<span data-ttu-id="c653d-150">Is létrehozhat, amelyek lehetővé teszik üzenetet küldött tooa témakör belül egy adott üzenettémakör-előfizetésben kell megjelenítése tooscope szűrők.</span><span class="sxs-lookup"><span data-stu-id="c653d-150">You can also create filters that allow you tooscope which messages sent tooa topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="c653d-151">hello szűrő előfizetések által támogatott legrugalmasabb típusú van a **SqlFilter**, amely az SQL92 egy részhalmazát valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="c653d-151">hello most flexible type of filter supported by subscriptions is the **SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="c653d-152">Az SQL-szűrők hello tulajdonságait, amelyek a közzétett toohello témakör köszönőüzenetei működik.</span><span class="sxs-lookup"><span data-stu-id="c653d-152">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="c653d-153">Az SQL-szűrőkkel használható hello kifejezések kapcsolatos további tudnivalókért tekintse meg a hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] szintaxist.</span><span class="sxs-lookup"><span data-stu-id="c653d-153">For more details about hello expressions that can be used with a SQL filter, review hello [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="c653d-154">Szűrők felveheti tooa előfizetés hello segítségével `createRule` hello metódusában **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="c653d-154">Filters can be added tooa subscription by using hello `createRule` method of hello **ServiceBusService** object.</span></span> <span data-ttu-id="c653d-155">Ez a módszer új szűrők tooan meglévő előfizetés hozzáadását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="c653d-155">This method allows you to add new filters tooan existing subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="c653d-156">Mert hello alapértelmezett szűrőhöz automatikusan tooall új előfizetésekre, először el kell távolítania hello alapértelmezett szűrő vagy a **MatchAll** felülírják más szűrőket is megadhat.</span><span class="sxs-lookup"><span data-stu-id="c653d-156">Because hello default filter is applied automatically tooall new subscriptions, you must first remove hello default filter or the **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="c653d-157">Alapértelmezett szabály hello távolíthatja hello használatával `deleteRule` metódusában a **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="c653d-157">You can remove hello default rule by using hello `deleteRule` method of the **ServiceBusService** object.</span></span>
>
>

<span data-ttu-id="c653d-158">hello alábbi példa egy nevű előfizetést hoz létre `HighMessages` rendelkező egy **SqlFilter** , amely csak választja ki, amelyek egyéni üzenetek `messagenumber` tulajdonságának értéke nagyobb, mint 3:</span><span class="sxs-lookup"><span data-stu-id="c653d-158">hello following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `messagenumber` property greater than 3:</span></span>

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

<span data-ttu-id="c653d-159">Ehhez hasonlóan hello alábbi példa egy nevű előfizetést hoz létre `LowMessages` rendelkező egy **SqlFilter** , amely csak rendelkező üzeneteket választja ki egy `messagenumber` tulajdonságának értéke kisebb vagy egyenlő, mint too3:</span><span class="sxs-lookup"><span data-stu-id="c653d-159">Similarly, hello following example creates a subscription named `LowMessages` with a **SqlFilter** that only selects messages that have a `messagenumber` property less than or equal too3:</span></span>

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

<span data-ttu-id="c653d-160">Ha egy most üzenettel túl`MyTopic`, azt mindig kézbesíti a rendszer a fogadók előfizetett toohello `AllMessages` üzenettémakör-előfizetésre, és szelektív módon kézbesíti az előfizetett tooreceivers toohello `HighMessages` és `LowMessages` üzenettémakör (hello üzenet tartalma függően).</span><span class="sxs-lookup"><span data-stu-id="c653d-160">When a message is now sent too`MyTopic`, it will always be delivered to receivers subscribed toohello `AllMessages` topic subscription, and selectively delivered tooreceivers subscribed toohello `HighMessages` and `LowMessages` topic subscriptions (depending upon hello message content).</span></span>

## <a name="how-toosend-messages-tooa-topic"></a><span data-ttu-id="c653d-161">Hogyan toosend üzenetek tooa témakör</span><span class="sxs-lookup"><span data-stu-id="c653d-161">How toosend messages tooa topic</span></span>
<span data-ttu-id="c653d-162">egy üzenet tooa Service Bus-témakörbe toosend, az alkalmazást kell használnia a `sendTopicMessage` hello metódusában **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="c653d-162">toosend a message tooa Service Bus topic, your application must use the `sendTopicMessage` method of hello **ServiceBusService** object.</span></span>
<span data-ttu-id="c653d-163">Üzenetek küldése tooService Bus-témaköröket **BrokeredMessage** objektumok.</span><span class="sxs-lookup"><span data-stu-id="c653d-163">Messages sent tooService Bus topics are **BrokeredMessage** objects.</span></span>
<span data-ttu-id="c653d-164">**BrokeredMessage** objektumok rendelkeznek egy szabványos tulajdonságkészlettel (például `Label` és `TimeToLive`), amely használt toohold egyéni alkalmazásspecifikus tulajdonságokat, valamint egy karakterlánc típusú adatok törzsét.</span><span class="sxs-lookup"><span data-stu-id="c653d-164">**BrokeredMessage** objects have a set of standard properties (such as `Label` and `TimeToLive`), a dictionary that is used toohold custom application-specific properties, and a body of string data.</span></span> <span data-ttu-id="c653d-165">Az alkalmazás beállíthatja hello üzenet törzsét hello úgy, hogy egy karakterláncértéket hello `sendTopicMessage` és a szabványos tulajdonságait tölti fel a alapértelmezett értékek szerint szükséges.</span><span class="sxs-lookup"><span data-stu-id="c653d-165">An application can set hello body of hello message by passing a string value to hello `sendTopicMessage` and any required standard properties will be populated by default values.</span></span>

<span data-ttu-id="c653d-166">hello következő példa bemutatja, hogyan toosend öt tesztüzenet `MyTopic`.</span><span class="sxs-lookup"><span data-stu-id="c653d-166">hello following example demonstrates how toosend five test messages to `MyTopic`.</span></span> <span data-ttu-id="c653d-167">Vegye figyelembe, hogy hello `messagenumber` egyes üzenetek tulajdonság értéke a hello iterációs hello hurok változik (Ez határozza meg, hogy melyik előfizetések fogják megkapni):</span><span class="sxs-lookup"><span data-stu-id="c653d-167">Note that hello `messagenumber` property value of each message varies on hello iteration of hello loop (this will determine which subscriptions receive it):</span></span>

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

<span data-ttu-id="c653d-168">Service Bus-üzenettémakörök támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="c653d-168">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="c653d-169">hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="c653d-169">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="c653d-170">A témakörökben tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello, a témakörök által tárolt hello üzenetek teljes mérete.</span><span class="sxs-lookup"><span data-stu-id="c653d-170">There is no limit on hello number of messages held in a topic but there is a cap on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="c653d-171">A témakör ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="c653d-171">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="c653d-172">Üzenetek fogadása egy előfizetés</span><span class="sxs-lookup"><span data-stu-id="c653d-172">Receive messages from a subscription</span></span>
<span data-ttu-id="c653d-173">Üzenetek fogadása egy előfizetés használatával a `receiveSubscriptionMessage` hello metódusa **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="c653d-173">Messages are received from a subscription using the `receiveSubscriptionMessage` method on hello **ServiceBusService** object.</span></span> <span data-ttu-id="c653d-174">Alapértelmezés szerint az üzenetek törlődnek hello előfizetésből, olvasott; azonban (betekintés) olvashatja és üdvözlőüzenetére zárolása hello nem kötelező paraméter beállítása szerint a hello előfizetés törlése nélkül `isPeekLock` túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="c653d-174">By default, messages are deleted from hello subscription as they are read; however, you can read (peek) and lock hello message without deleting it from hello subscription by setting hello optional parameter `isPeekLock` too**true**.</span></span>

<span data-ttu-id="c653d-175">hello alapértelmezett viselkedését olvasását és üdvözlőüzenetére törlése a fogadási művelet részeként hello legegyszerűbb modell, és működik a legjobban az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet hello esetre, ha nem található.</span><span class="sxs-lookup"><span data-stu-id="c653d-175">hello default behavior of reading and deleting hello message as part of the receive operation is hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="c653d-176">toounderstand, ez egy olyan esetet, amelyben a fogyasztó problémák hello kérés fogadásához, és majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="c653d-176">toounderstand this, consider a scenario in which the consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="c653d-177">Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.</span><span class="sxs-lookup"><span data-stu-id="c653d-177">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="c653d-178">Ha hello `isPeekLock` paraméter értéke túl**igaz**, hello kap két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz.</span><span class="sxs-lookup"><span data-stu-id="c653d-178">If hello `isPeekLock` parameter is set too**true**, hello receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="c653d-179">A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása.</span><span class="sxs-lookup"><span data-stu-id="c653d-179">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span>
<span data-ttu-id="c653d-180">Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második a fogadási folyamat meghívásával **deleteMessage** módszerről és az üzenet toobe biztosítása törölt paraméterként.</span><span class="sxs-lookup"><span data-stu-id="c653d-180">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of the receive process by calling **deleteMessage** method and providing the message toobe deleted as a parameter.</span></span> <span data-ttu-id="c653d-181">Hello **deleteMessage** metódus lesz hello üzenetet feldolgozottként, és távolítsa el a hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="c653d-181">hello **deleteMessage** method will mark hello message as being consumed and remove it from hello subscription.</span></span>

<span data-ttu-id="c653d-182">hello következő példa bemutatja, hogyan lehet üzeneteket fogadni, és a feldolgozott használatával `receiveSubscriptionMessage`.</span><span class="sxs-lookup"><span data-stu-id="c653d-182">hello following example demonstrates how messages can be received and processed using `receiveSubscriptionMessage`.</span></span> <span data-ttu-id="c653d-183">hello példa először a következő mintához hello "LowMessages" előfizetés törli az üzenetet, és egy üzenetet kap hello "HighMessages" előfizetés használatával `isPeekLock` tootrue beállítása.</span><span class="sxs-lookup"><span data-stu-id="c653d-183">hello example first receives and deletes a message from hello 'LowMessages' subscription, and then receives a message from hello 'HighMessages' subscription using `isPeekLock` set tootrue.</span></span> <span data-ttu-id="c653d-184">Majd törli a hello üzenet használatával `deleteMessage`:</span><span class="sxs-lookup"><span data-stu-id="c653d-184">It then deletes hello message using `deleteMessage`:</span></span>

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="c653d-185">Hogyan toohandle omlik össze és nem olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="c653d-185">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="c653d-186">Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít.</span><span class="sxs-lookup"><span data-stu-id="c653d-186">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="c653d-187">Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlockMessage` metódust a **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="c653d-187">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlockMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="c653d-188">Ezzel a Service Bus toounlock hello előfizetésen belül az üzenet miatt és révén elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.</span><span class="sxs-lookup"><span data-stu-id="c653d-188">This will cause Service Bus toounlock the message within hello subscription and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="c653d-189">Is van az előfizetés lévő társított időtúllépés, és ha hello alkalmazás nem tooprocess üdvözlőüzenetére előtt hello zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus feloldja köszönőüzenetei automatikusan és lehetővé teszi az újbóli fogadását elérhető toobe.</span><span class="sxs-lookup"><span data-stu-id="c653d-189">There is also a timeout associated with a message locked within the subscription, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus unlocks hello message automatically and makes it available toobe received again.</span></span>

<span data-ttu-id="c653d-190">A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `deleteMessage` metódust, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazás, amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="c653d-190">In hello event that hello application crashes after processing hello message but before hello `deleteMessage` method is called, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="c653d-191">Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet.</span><span class="sxs-lookup"><span data-stu-id="c653d-191">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="c653d-192">Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését.</span><span class="sxs-lookup"><span data-stu-id="c653d-192">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="c653d-193">Ez gyakran érhető el, használja a **MessageId** hello üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="c653d-193">This is often achieved using the **MessageId** property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="c653d-194">Témakörök és előfizetések törlése</span><span class="sxs-lookup"><span data-stu-id="c653d-194">Delete topics and subscriptions</span></span>
<span data-ttu-id="c653d-195">Üzenettémák és előfizetések következetesek, és explicit módon kell törölni vagy hello [Azure-portálon] [ Azure portal] vagy programon keresztül.</span><span class="sxs-lookup"><span data-stu-id="c653d-195">Topics and subscriptions are persistent, and must be explicitly deleted either through hello [Azure portal][Azure portal] or programmatically.</span></span>
<span data-ttu-id="c653d-196">hello következő példa bemutatja, hogyan toodelete hello témakör nevű `MyTopic`:</span><span class="sxs-lookup"><span data-stu-id="c653d-196">hello following example demonstrates how toodelete hello topic named `MyTopic`:</span></span>

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

<span data-ttu-id="c653d-197">Egy témakör törlése hello témakörben regisztrált előfizetésekkel is törli.</span><span class="sxs-lookup"><span data-stu-id="c653d-197">Deleting a topic will also delete any subscriptions that are registered with hello topic.</span></span> <span data-ttu-id="c653d-198">Az előfizetések független módon is törölhetők.</span><span class="sxs-lookup"><span data-stu-id="c653d-198">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="c653d-199">A következő példa bemutatja, hogyan toodelete előfizetés nevű `HighMessages` a hello `MyTopic` témakör:</span><span class="sxs-lookup"><span data-stu-id="c653d-199">The following example shows how toodelete a subscription named `HighMessages` from hello `MyTopic` topic:</span></span>

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a><span data-ttu-id="c653d-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c653d-200">Next Steps</span></span>
<span data-ttu-id="c653d-201">Most, hogy megismerte a Service Bus-üzenettémakörök hello alapjait, kövesse az alábbi hivatkozások további toolearn.</span><span class="sxs-lookup"><span data-stu-id="c653d-201">Now that you've learned hello basics of Service Bus topics, follow these links toolearn more.</span></span>

* <span data-ttu-id="c653d-202">Lásd: [üzenetsorok, témakörök és előfizetések][Queues, topics, and subscriptions].</span><span class="sxs-lookup"><span data-stu-id="c653d-202">See [Queues, topics, and subscriptions][Queues, topics, and subscriptions].</span></span>
* <span data-ttu-id="c653d-203">Az [SqlFilter][SqlFilter] API-referenciája.</span><span class="sxs-lookup"><span data-stu-id="c653d-203">API reference for [SqlFilter][SqlFilter].</span></span>
* <span data-ttu-id="c653d-204">A Microsoft hello [Azure SDK for csomópont] [ Azure SDK for Node] GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="c653d-204">Visit hello [Azure SDK for Node][Azure SDK for Node] repository on GitHub.</span></span>

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[létrehozása és központi telepítése egy Node.js-alkalmazás tooan Azure webhelyére]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
