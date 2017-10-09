---
title: aaaHow toouse Azure Service Bus hello WebJobs SDK a
description: "Ismerje meg, hogyan toouse Azure Service Bus-üzenetsorok és témakörök a hello WebJobs SDK."
services: app-service\web, service-bus
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 2114a934-135b-42b8-871c-6cc040214e76
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: cb801a9320a20c276da4f48c8941c09d3f09bb1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-service-bus-with-hello-webjobs-sdk"></a><span data-ttu-id="e6097-103">Hogyan toouse Azure Service Bus a hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="e6097-103">How toouse Azure Service Bus with hello WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="e6097-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e6097-104">Overview</span></span>
<span data-ttu-id="e6097-105">Ez az útmutató C# kódot, hogy hogyan minták tootrigger egy folyamatot, az Azure Service Bus-üzenet fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="e6097-105">This guide provides C# code samples that show how tootrigger a process when an Azure Service Bus message is received.</span></span> <span data-ttu-id="e6097-106">hello kód minták használata [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzió 1.x.</span><span class="sxs-lookup"><span data-stu-id="e6097-106">hello code samples use [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="e6097-107">hello az útmutató feltételezi, hogy tudja [hogyan toocreate egy webjobs-feladat projektet, a Visual Studio kapcsolati karakterláncok adott pont tooyour tárfiók](websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e6097-107">hello guide assumes you know [how toocreate a WebJob project in Visual Studio with connection strings that point tooyour storage account](websites-dotnet-webjobs-sdk-get-started.md).</span></span>

<span data-ttu-id="e6097-108">hello kódtöredékek csak megjelenítése funkciók nem hello hello létrehozó kód mellől `JobHost` objektum ebben a példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="e6097-108">hello code snippets only show functions, not hello code that creates hello `JobHost` object as in this example:</span></span>

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

<span data-ttu-id="e6097-109">A [teljes Service Bus-példakód](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) hello azure-webjobs-sdk-minták tárházban github.com van.</span><span class="sxs-lookup"><span data-stu-id="e6097-109">A [complete Service Bus code example](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) is in hello azure-webjobs-sdk-samples repository on GitHub.com.</span></span>

## <span data-ttu-id="e6097-110"><a id="prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e6097-110"><a id="prerequisites"></a> Prerequisites</span></span>
<span data-ttu-id="e6097-111">toowork Service Bus az informatikai részleg tooinstall hello [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet csomag továbbá toohello többi WebJobs SDK-csomagot.</span><span class="sxs-lookup"><span data-stu-id="e6097-111">toowork with Service Bus you have tooinstall hello [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet package in addition toohello other WebJobs SDK packages.</span></span> 

<span data-ttu-id="e6097-112">Továbbá toohello tárolási kapcsolati karakterláncok tooset hello AzureWebJobsServiceBus kapcsolati karakterláncot is.</span><span class="sxs-lookup"><span data-stu-id="e6097-112">You also have tooset hello AzureWebJobsServiceBus connection string in addition toohello storage connection strings.</span></span>  <span data-ttu-id="e6097-113">Ezt megteheti hello `connectionStrings` hello App.config fájl, ahogy az alábbi példa hello szakaszában:</span><span class="sxs-lookup"><span data-stu-id="e6097-113">You can do this in hello `connectionStrings` section of hello App.config file, as shown in hello following example:</span></span>

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

<span data-ttu-id="e6097-114">Egy minta-projektet, amely tartalmazza az hello Service Bus kapcsolati karakterlánc beállítása hello App.config fájlban, lásd: [példa Service Bus](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus).</span><span class="sxs-lookup"><span data-stu-id="e6097-114">For a sample project that includes hello Service Bus connection string setting in hello App.config file, see [Service Bus example](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus).</span></span> 

<span data-ttu-id="e6097-115">hello kapcsolati karakterláncok is megadható hello Azure futtatókörnyezetben, amely majd felülírja a hello App.config beállításokat, az Azure rendszerben; hello webjobs-feladat futtatásakor További információkért lásd: [Ismerkedés a WebJobs SDK hello](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).</span><span class="sxs-lookup"><span data-stu-id="e6097-115">hello connection strings can also be set in hello Azure runtime environment, which then overrides hello App.config settings when hello WebJob runs in Azure; for more information, see [Get Started with hello WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).</span></span>

## <span data-ttu-id="e6097-116"><a id="trigger"></a>Hogyan tootrigger egy függvényt, amikor egy Service Bus feldolgozási sor üzenetet érkezett</span><span class="sxs-lookup"><span data-stu-id="e6097-116"><a id="trigger"></a> How tootrigger a function when a Service Bus queue message is received</span></span>
<span data-ttu-id="e6097-117">toowrite egy függvényt, amely a WebJobs SDK hello meghívja a várólista-üzenet fogadásakor, használja a hello `ServiceBusTrigger` attribútum.</span><span class="sxs-lookup"><span data-stu-id="e6097-117">toowrite a function that hello WebJobs SDK calls when a queue message is received, use hello `ServiceBusTrigger` attribute.</span></span> <span data-ttu-id="e6097-118">az attribútum konstruktora hello paramétert hello várólista toopoll hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="e6097-118">hello attribute constructor takes a parameter that specifies hello name of hello queue toopoll.</span></span>

### <a name="how-servicebustrigger-works"></a><span data-ttu-id="e6097-119">ServiceBusTrigger működése</span><span class="sxs-lookup"><span data-stu-id="e6097-119">How ServiceBusTrigger works</span></span>
<span data-ttu-id="e6097-120">hello SDK üzenetet kap a `PeekLock` mód és a hívások `Complete` üdvözlőüzenetére Ha hello függvény futtatása sikeresen befejeződött, vagy a hívások `Abandon` hello függvény meghibásodásakor.</span><span class="sxs-lookup"><span data-stu-id="e6097-120">hello SDK receives a message in `PeekLock` mode and calls `Complete` on hello message if hello function finishes successfully, or calls `Abandon` if hello function fails.</span></span> <span data-ttu-id="e6097-121">Ha hello függvény futásakor hello hosszabb `PeekLock` időkorlát, hello zárolása automatikusan megújítják.</span><span class="sxs-lookup"><span data-stu-id="e6097-121">If hello function runs longer than hello `PeekLock` timeout, hello lock is automatically renewed.</span></span>

<span data-ttu-id="e6097-122">A Service Bus saját nem ellenőrzött vagy hello WebJobs SDK által konfigurált poison várólista kezelését végzi.</span><span class="sxs-lookup"><span data-stu-id="e6097-122">Service Bus does its own poison queue handling which cannot be controlled or configured by hello WebJobs SDK.</span></span> 

### <a name="string-queue-message"></a><span data-ttu-id="e6097-123">Karakterlánc üzenetsor</span><span class="sxs-lookup"><span data-stu-id="e6097-123">String queue message</span></span>
<span data-ttu-id="e6097-124">hello alábbi kódminta olvassa be, amely tartalmazza a karakterláncot, majd beírja hello karakterlánc toohello WebJobs SDK irányítópult várólista üzenet.</span><span class="sxs-lookup"><span data-stu-id="e6097-124">hello following code sample reads a queue message that contains a string and writes hello string toohello WebJobs SDK dashboard.</span></span>

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

<span data-ttu-id="e6097-125">**Megjegyzés:** létrehozásakor hello üzenetsor-üzeneteket egy alkalmazás, amely nem használja a WebJobs SDK hello, győződjön meg arról, hogy tooset [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) túl "text/plain".</span><span class="sxs-lookup"><span data-stu-id="e6097-125">**Note:** If you are creating hello queue messages in an application that doesn't use hello WebJobs SDK, make sure tooset [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) too"text/plain".</span></span>

### <a name="poco-queue-message"></a><span data-ttu-id="e6097-126">POCO üzenetsor</span><span class="sxs-lookup"><span data-stu-id="e6097-126">POCO queue message</span></span>
<span data-ttu-id="e6097-127">hello SDK automatikusan deszerializálni a egy üzenetsor-üzenetet, amely tartalmazza egy POCO JSON [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) típus.</span><span class="sxs-lookup"><span data-stu-id="e6097-127">hello SDK will automatically deserialize a queue message that contains JSON for a POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) type.</span></span> <span data-ttu-id="e6097-128">hello alábbi kódminta olvas egy üzenetsor-üzenetet, amely tartalmaz egy `BlobInformation` objektum, amely rendelkezik egy `BlobName` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="e6097-128">hello following code sample reads a queue message that contains a `BlobInformation` object which has a `BlobName` property:</span></span>

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

<span data-ttu-id="e6097-129">Hogyan hello POCO toowork blobok és táblák toouse tulajdonságainak hello azonos megjelenítő mintakódok működik, tekintse meg a hello [tárolási várólisták verziója, ez a cikk](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).</span><span class="sxs-lookup"><span data-stu-id="e6097-129">For code samples showing how toouse properties of hello POCO toowork with blobs and tables in hello same function, see hello [storage queues version of this article](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).</span></span>

<span data-ttu-id="e6097-130">Ha a várólista üdvözlőüzenetére létrehozó kód hello WebJobs SDK nem használ, akkor a kód hasonló toohello a következő példa:</span><span class="sxs-lookup"><span data-stu-id="e6097-130">If your code that creates hello queue message doesn't use hello WebJobs SDK, use code similar toohello following example:</span></span>

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a><span data-ttu-id="e6097-131">Típusok ServiceBusTrigger együttműködik</span><span class="sxs-lookup"><span data-stu-id="e6097-131">Types ServiceBusTrigger works with</span></span>
<span data-ttu-id="e6097-132">Módosításokon kívül `string` és POCO típusok hello használhatja `ServiceBusTrigger` attribútum egy bájttömböt vagy egy `BrokeredMessage` objektum.</span><span class="sxs-lookup"><span data-stu-id="e6097-132">Besides `string` and POCO  types, you can use hello `ServiceBusTrigger` attribute with a byte array or a `BrokeredMessage` object.</span></span>

## <span data-ttu-id="e6097-133"><a id="create"></a>Hogyan toocreate Service Bus várólistára üzenetek</span><span class="sxs-lookup"><span data-stu-id="e6097-133"><a id="create"></a> How toocreate Service Bus queue messages</span></span>
<span data-ttu-id="e6097-134">egy függvényt, amely létrehoz egy új sor üzenetet toowrite hello használata `ServiceBus` attribútum számára pedig hello várólista neve toohello attribútumok konstruktorában.</span><span class="sxs-lookup"><span data-stu-id="e6097-134">toowrite a function that creates a new queue message use hello `ServiceBus` attribute and pass in hello queue name toohello attribute constructor.</span></span> 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a><span data-ttu-id="e6097-135">Egyetlen üzenetsor nem aszinkron függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="e6097-135">Create a single queue message in a non-async function</span></span>
<span data-ttu-id="e6097-136">a következő példakód hello egy új üzenet hello várólista neve "outputqueue" hello "inputqueue" nevű hello várólista fogadott üzenet hello ugyanaz, mint a tartalom a kimeneti paraméter toocreate használja.</span><span class="sxs-lookup"><span data-stu-id="e6097-136">hello following code sample uses an output parameter toocreate a new message in hello queue named "outputqueue" with hello same content as hello message received in hello queue named "inputqueue".</span></span>

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

<span data-ttu-id="e6097-137">egyetlen üzenetsor létrehozásához hello kimeneti paraméter hello a következő típusok egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="e6097-137">hello output parameter for creating a single queue message can be any of hello following types:</span></span>

* `string`
* `byte[]`
* `BrokeredMessage`
* <span data-ttu-id="e6097-138">A szerializálható POCO típus, amely adhat meg.</span><span class="sxs-lookup"><span data-stu-id="e6097-138">A serializable POCO type that you define.</span></span> <span data-ttu-id="e6097-139">Automatikusan szerializálható JSON-ként.</span><span class="sxs-lookup"><span data-stu-id="e6097-139">Automatically serialized as JSON.</span></span>

<span data-ttu-id="e6097-140">POCO típusú paraméterrel egy üzenetsor mindig létrejön, amikor befejeződik hello függvény; hello paraméterek egyike null értékű, ha hello SDK egy NULL értékű lesz vissza, ha az üdvözlőüzenetére fogad és deszerializálni üzenetsor-üzenetet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e6097-140">For POCO type parameters, a queue message is always created when hello function ends; if hello parameter is null, hello SDK creates a queue message that will return null when hello message is received and deserialized.</span></span> <span data-ttu-id="e6097-141">Az egyéb hello, ha hello paraméterek egyike null értékű nincs várólista üzenet jön létre.</span><span class="sxs-lookup"><span data-stu-id="e6097-141">For hello other types, if hello parameter is null no queue message is created.</span></span>

### <a name="create-multiple-queue-messages-or-in-async-functions"></a><span data-ttu-id="e6097-142">Hozzon létre több üzenetek vagy aszinkron függvény</span><span class="sxs-lookup"><span data-stu-id="e6097-142">Create multiple queue messages or in async functions</span></span>
<span data-ttu-id="e6097-143">toocreate több üzenetet hello használata `ServiceBus` rendelkező attribútum `ICollector<T>` vagy `IAsyncCollector<T>`, ahogy az alábbi példakód hello:</span><span class="sxs-lookup"><span data-stu-id="e6097-143">toocreate multiple messages, use  hello `ServiceBus` attribute with `ICollector<T>` or `IAsyncCollector<T>`, as shown in hello following code sample:</span></span>

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="e6097-144">Minden várólista-üzenet létrehozásakor a rendszer azonnal hello `Add` metódust.</span><span class="sxs-lookup"><span data-stu-id="e6097-144">Each queue message is created immediately when hello `Add` method is called.</span></span>

## <span data-ttu-id="e6097-145"><a id="topics"></a>Hogyan toowork a Service Bus-témakörök</span><span class="sxs-lookup"><span data-stu-id="e6097-145"><a id="topics"></a>How toowork with Service Bus topics</span></span>
<span data-ttu-id="e6097-146">toowrite hello SDK függvény meghívja a Service Bus-témakörbe üzenet fogadásakor, használja a hello `ServiceBusTrigger` hello konstruktort témakör és előfizetés nevét, ahogy az alábbi példakód hello attribútum:</span><span class="sxs-lookup"><span data-stu-id="e6097-146">toowrite a function that hello SDK calls when a message is received on a Service Bus topic, use hello `ServiceBusTrigger` attribute with hello constructor that takes topic name and subscription name, as shown in hello following code sample:</span></span>

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

<span data-ttu-id="e6097-147">egy üzenettémakört, használjon hello üzenet toocreate `ServiceBus` attribútum egy témakör neve hello az azonos módon a várólistacímke együtt használja azt.</span><span class="sxs-lookup"><span data-stu-id="e6097-147">toocreate a message on a topic, use hello `ServiceBus` attribute with a topic name hello same way you use it with a queue name.</span></span>

## <a name="features-added-in-release-11"></a><span data-ttu-id="e6097-148">1.1-es verzióban hozzáadott funkciók</span><span class="sxs-lookup"><span data-stu-id="e6097-148">Features added in release 1.1</span></span>
<span data-ttu-id="e6097-149">a következő funkciók hello 1.1-es verzióban lettek hozzáadva:</span><span class="sxs-lookup"><span data-stu-id="e6097-149">hello following features were added in release 1.1:</span></span>

* <span data-ttu-id="e6097-150">A részletes üzenetek feldolgozási keresztül testreszabásának engedélyezése `ServiceBusConfiguration.MessagingProvider`.</span><span class="sxs-lookup"><span data-stu-id="e6097-150">Allow deep customization of message processing via `ServiceBusConfiguration.MessagingProvider`.</span></span>
* <span data-ttu-id="e6097-151">`MessagingProvider`támogatja a Service Bus hello testreszabása `MessagingFactory` és `NamespaceManager`.</span><span class="sxs-lookup"><span data-stu-id="e6097-151">`MessagingProvider` supports customization of hello Service Bus `MessagingFactory` and `NamespaceManager`.</span></span>
* <span data-ttu-id="e6097-152">A `MessageProcessor` stratégia mintát lehetővé teszi a várólista/témakör / processzort toospecify.</span><span class="sxs-lookup"><span data-stu-id="e6097-152">A `MessageProcessor` strategy pattern allows you toospecify a processor per queue/topic.</span></span>
* <span data-ttu-id="e6097-153">Üzenet feldolgozása párhuzamossági alapértelmezés szerint támogatott.</span><span class="sxs-lookup"><span data-stu-id="e6097-153">Message processing concurrency is supported by default.</span></span> 
* <span data-ttu-id="e6097-154">Egyszerű testreszabás `OnMessageOptions` keresztül `ServiceBusConfiguration.MessageOptions`.</span><span class="sxs-lookup"><span data-stu-id="e6097-154">Easy customization of `OnMessageOptions` via `ServiceBusConfiguration.MessageOptions`.</span></span>
* <span data-ttu-id="e6097-155">Engedélyezése [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) megadott toobe `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (a forgatókönyvekhez, amelyekben előfordulhat, hogy nem jogosultságaik kezelését).</span><span class="sxs-lookup"><span data-stu-id="e6097-155">Allow [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) toobe specified on `ServiceBusTriggerAttribute`/`ServiceBusAttribute` (for scenarios where you might not have Manage rights).</span></span> <span data-ttu-id="e6097-156">Ne feledje, hogy Azure webjobs-feladatok nem tooautomatically kiépítése nem létező üzenetsorok és témakörök AccessRights kezelése nélkül.</span><span class="sxs-lookup"><span data-stu-id="e6097-156">Note that Azure WebJobs is unable tooautomatically provision non-existent queues and topics without Manage AccessRights.</span></span>

## <span data-ttu-id="e6097-157"><a id="queues"></a>Hello tárolási várólisták hogyan-tooarticle hatálya kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="e6097-157"><a id="queues"></a>Related topics covered by hello storage queues how-tooarticle</span></span>
<span data-ttu-id="e6097-158">Nem adott tooService Bus, lásd: WebJobs SDK-forgatókönyvekkel kapcsolatos információk [hogyan toouse Azure várólista tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="e6097-158">For information about WebJobs SDK scenarios not specific tooService Bus, see [How toouse Azure queue storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="e6097-159">A cikkben szereplő témakörök hello alábbiakat foglalja magába:</span><span class="sxs-lookup"><span data-stu-id="e6097-159">Topics covered in that article include hello following:</span></span>

* <span data-ttu-id="e6097-160">Aszinkron funkciók</span><span class="sxs-lookup"><span data-stu-id="e6097-160">Async functions</span></span>
* <span data-ttu-id="e6097-161">Több példánya</span><span class="sxs-lookup"><span data-stu-id="e6097-161">Multiple instances</span></span>
* <span data-ttu-id="e6097-162">Biztonságos leállításának</span><span class="sxs-lookup"><span data-stu-id="e6097-162">Graceful shutdown</span></span>
* <span data-ttu-id="e6097-163">Egy függvény törzséhez hello a WebJobs SDK attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="e6097-163">Use WebJobs SDK attributes in hello body of a function</span></span>
* <span data-ttu-id="e6097-164">A kód hello SDK kapcsolati karakterláncok beállítása</span><span class="sxs-lookup"><span data-stu-id="e6097-164">Set hello SDK connection strings in code</span></span>
* <span data-ttu-id="e6097-165">Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot</span><span class="sxs-lookup"><span data-stu-id="e6097-165">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="e6097-166">Manuálisan kezdeményezi egy függvény</span><span class="sxs-lookup"><span data-stu-id="e6097-166">Trigger a function manually</span></span>
* <span data-ttu-id="e6097-167">Naplók írása</span><span class="sxs-lookup"><span data-stu-id="e6097-167">Write logs</span></span>

## <span data-ttu-id="e6097-168"><a id="nextsteps"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e6097-168"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="e6097-169">Ez az útmutató nyújtott a kódot, hogy hogyan minták toohandle gyakori forgatókönyvei Azure Service Bus használata.</span><span class="sxs-lookup"><span data-stu-id="e6097-169">This guide has provided code samples that show how toohandle common scenarios for working with Azure Service Bus.</span></span> <span data-ttu-id="e6097-170">További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="e6097-170">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

