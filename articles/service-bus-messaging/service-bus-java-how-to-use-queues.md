---
title: "Azure Service Bus-üzenetsorok használata Java |} Microsoft Docs"
description: "Ismerje meg, hogyan használhatók a Service Bus-üzenetsorok az Azure-ban. A Kódminták Java nyelven."
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
ms.assetid: f701439c-553e-402c-94a7-64400f997d59
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 170f431525ffdc93a01fc085e48e69c3a774968e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-queues-with-java"></a><span data-ttu-id="bb79c-104">Service Bus-üzenetsorok használata Java</span><span class="sxs-lookup"><span data-stu-id="bb79c-104">How to use Service Bus queues with Java</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="bb79c-105">Ez a cikk a Service Bus-üzenetsorok használatát ismerteti.</span><span class="sxs-lookup"><span data-stu-id="bb79c-105">This article describes how to use Service Bus queues.</span></span> <span data-ttu-id="bb79c-106">A mintákat a Java és -felhasználási nyelven íródtak a [Javához készült Azure SDK][Azure SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="bb79c-106">The samples are written in Java and use the [Azure SDK for Java][Azure SDK for Java].</span></span> <span data-ttu-id="bb79c-107">Az ismertetett forgatókönyvek **üzenetsorok létrehozása**, **üzenetek küldése és fogadása**, és **várólisták törlése**.</span><span class="sxs-lookup"><span data-stu-id="bb79c-107">The scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a><span data-ttu-id="bb79c-108">Állítsa be az alkalmazását, Service Bus használatára</span><span class="sxs-lookup"><span data-stu-id="bb79c-108">Configure your application to use Service Bus</span></span>
<span data-ttu-id="bb79c-109">Győződjön meg arról, hogy telepítette a [Javához készült Azure SDK] [ Azure SDK for Java] Ez a minta létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="bb79c-109">Make sure you have installed the [Azure SDK for Java][Azure SDK for Java] before building this sample.</span></span> <span data-ttu-id="bb79c-110">Ha az Eclipse használja, telepítheti a [Eclipse Azure eszköztára] [ Azure Toolkit for Eclipse] , amely tartalmazza az Azure SDK for Java.</span><span class="sxs-lookup"><span data-stu-id="bb79c-110">If you are using Eclipse, you can install the [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse] that includes the Azure SDK for Java.</span></span> <span data-ttu-id="bb79c-111">Ezután a **Microsoft Azure-könyvtárakban Java** a projekthez:</span><span class="sxs-lookup"><span data-stu-id="bb79c-111">You can then add the **Microsoft Azure Libraries for Java** to your project:</span></span>

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

<span data-ttu-id="bb79c-112">Adja hozzá a következő `import` utasítást, hogy a Java-fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="bb79c-112">Add the following `import` statements to the top of the Java file:</span></span>

```java
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a><span data-ttu-id="bb79c-113">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="bb79c-113">Create a queue</span></span>
<span data-ttu-id="bb79c-114">Felügyeleti műveletek a Service Bus-üzenetsorok használatával is kell végezni a **ServiceBusContract** osztály.</span><span class="sxs-lookup"><span data-stu-id="bb79c-114">Management operations for Service Bus queues can be performed via the **ServiceBusContract** class.</span></span> <span data-ttu-id="bb79c-115">A **ServiceBusContract** objektum van kialakítani, amely magában foglalja a SAS-jogkivonat való kezelése, a megfelelő konfiguráció és a **ServiceBusContract** osztálya: Azure kommunikáció egyetlen pont.</span><span class="sxs-lookup"><span data-stu-id="bb79c-115">A **ServiceBusContract** object is constructed with an appropriate configuration that encapsulates the SAS token with permissions to manage it, and the **ServiceBusContract** class is the sole point of communication with Azure.</span></span>

<span data-ttu-id="bb79c-116">A **ServiceBusService** osztály létrehozásához, enumerálásához és törléséhez várólisták metódusokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="bb79c-116">The **ServiceBusService** class provides methods to create, enumerate, and delete queues.</span></span> <span data-ttu-id="bb79c-117">Az alábbi példa hogyan egy **ServiceBusService** objektum segítségével létrehoz egy sort nevű `TestQueue`, nevű névtérrel `HowToSample`:</span><span class="sxs-lookup"><span data-stu-id="bb79c-117">The example below shows how a **ServiceBusService** object can be used to create a queue named `TestQueue`, with a namespace named `HowToSample`:</span></span>

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

<span data-ttu-id="bb79c-118">Módszer áll rendelkezésre a `QueueInfo` , amelyek lehetővé teszik a kell beállítani a várólista tulajdonságainak (például: alapértelmezett--élettartama (TTL) értékének beállítására a várólistára küldött üzenetek alkalmazandó).</span><span class="sxs-lookup"><span data-stu-id="bb79c-118">There are methods on `QueueInfo` that allow properties of the queue to be tuned (for example: to set the default time-to-live (TTL) value to be applied to messages sent to the queue).</span></span> <span data-ttu-id="bb79c-119">A következő példa bemutatja, hogyan nevű várólista létrehozása `TestQueue` 5 GB maximális mérettel:</span><span class="sxs-lookup"><span data-stu-id="bb79c-119">The following example shows how to create a queue named `TestQueue` with a maximum size of 5GB:</span></span>

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

<span data-ttu-id="bb79c-120">Vegye figyelembe, hogy használhatja a `listQueues` metódusa **ServiceBusContract** objektumok kereséséhez, ha a megadott névvel már létezik sor egy szolgáltatásnévtérben.</span><span class="sxs-lookup"><span data-stu-id="bb79c-120">Note that you can use the `listQueues` method on **ServiceBusContract** objects to check if a queue with a specified name already exists within a service namespace.</span></span>

## <a name="send-messages-to-a-queue"></a><span data-ttu-id="bb79c-121">Üzenetek küldése egy üzenetsorba</span><span class="sxs-lookup"><span data-stu-id="bb79c-121">Send messages to a queue</span></span>
<span data-ttu-id="bb79c-122">A Service Bus-üzenetsorba való üzenetküldéshez, az alkalmazás beolvassa a **ServiceBusContract** objektum.</span><span class="sxs-lookup"><span data-stu-id="bb79c-122">To send a message to a Service Bus queue, your application obtains a **ServiceBusContract** object.</span></span> <span data-ttu-id="bb79c-123">A következő kód bemutatja, hogyan küldhető a `TestQueue` a korábban létrehozott várólista a `HowToSample` névtér:</span><span class="sxs-lookup"><span data-stu-id="bb79c-123">The following code shows how to send a message for the `TestQueue` queue previously created in the `HowToSample` namespace:</span></span>

```java
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

<span data-ttu-id="bb79c-124">Küldött üzeneteket, és kapott a Service Bus üzenetsorok példánya a [BrokeredMessage] [ BrokeredMessage] osztály.</span><span class="sxs-lookup"><span data-stu-id="bb79c-124">Messages sent to, and received from Service Bus queues are instances of the [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="bb79c-125">[BrokeredMessage] [ BrokeredMessage] objektumok rendelkeznek egy szabványos tulajdonságkészlettel (például [címke](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) és [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), egyéni tárolására használt dictionary az alkalmazás-specifikus tulajdonságait, és egy tetszőleges alkalmazásadatokból álló törzzsel.</span><span class="sxs-lookup"><span data-stu-id="bb79c-125">[BrokeredMessage][BrokeredMessage] objects have a set of standard properties (such as [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) and [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), a dictionary that is used to hold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="bb79c-126">Az alkalmazás beállíthatja az üzenet törzsét bármilyen szerializálható objektumnak konstruktorának való átadásával a [BrokeredMessage][BrokeredMessage], és a megfelelő szerializáló majd használható szerializálja az objektumot.</span><span class="sxs-lookup"><span data-stu-id="bb79c-126">An application can set the body of the message by passing any serializable object into the constructor of the [BrokeredMessage][BrokeredMessage], and the appropriate serializer will then be used to serialize the object.</span></span> <span data-ttu-id="bb79c-127">Másik lehetőségként megadhat egy **java. IO. InputStream** objektum.</span><span class="sxs-lookup"><span data-stu-id="bb79c-127">Alternatively, you can provide a **java.IO.InputStream** object.</span></span>

<span data-ttu-id="bb79c-128">A következő példa bemutatja, hogyan küldhető öt tesztüzenet az a `TestQueue` **MessageSender** azt a korábbi kódrészletben kapott:</span><span class="sxs-lookup"><span data-stu-id="bb79c-128">The following example demonstrates how to send five test messages to the `TestQueue` **MessageSender** we obtained in the previous code snippet:</span></span>

```java
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

<span data-ttu-id="bb79c-129">A Service Bus-üzenetsorok a [Standard csomagban](service-bus-premium-messaging.md) legfeljebb 256 KB, a [Prémium csomagban](service-bus-premium-messaging.md) legfeljebb 1 MB méretű üzeneteket támogatnak.</span><span class="sxs-lookup"><span data-stu-id="bb79c-129">Service Bus queues support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="bb79c-130">A szabványos és az egyéni alkalmazástulajdonságokat tartalmazó fejléc mérete legfeljebb 64 KB lehet.</span><span class="sxs-lookup"><span data-stu-id="bb79c-130">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="bb79c-131">Az üzenetsorban tárolt üzenetek száma korlátlan, az üzenetsor által tárolt üzenetek teljes mérete azonban korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="bb79c-131">There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue.</span></span> <span data-ttu-id="bb79c-132">Az üzenetsor ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="bb79c-132">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="bb79c-133">Üzenetek fogadása egy üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="bb79c-133">Receive messages from a queue</span></span>
<span data-ttu-id="bb79c-134">Az elsődleges üzenetek fogadása egy üzenetsorból módja használja egy **ServiceBusContract** objektum.</span><span class="sxs-lookup"><span data-stu-id="bb79c-134">The primary way to receive messages from a queue is to use a **ServiceBusContract** object.</span></span> <span data-ttu-id="bb79c-135">A fogadott üzenetek a két különböző módban tudnak működni: **ReceiveAndDelete** és **PeekLock**.</span><span class="sxs-lookup"><span data-stu-id="bb79c-135">Received messages can work in two different modes: **ReceiveAndDelete** and **PeekLock**.</span></span>

<span data-ttu-id="bb79c-136">Használatakor a **ReceiveAndDelete** mód, kap egy egylépéses művelet – Ez azt jelenti, hogy amikor a Service Bus egy olvasási kérést kap a sorhoz, azt az üzenetet, feldolgozottként jelöli meg, és visszaadja az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="bb79c-136">When using the **ReceiveAndDelete** mode, receive is a single-shot operation - that is, when Service Bus receives a read request for a message in a queue, it marks the message as being consumed and returns it to the application.</span></span> <span data-ttu-id="bb79c-137">**ReceiveAndDelete** módban (amely az alapértelmezett mód) a legegyszerűbb modell, és működik a legjobban forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet hiba esetén.</span><span class="sxs-lookup"><span data-stu-id="bb79c-137">**ReceiveAndDelete** mode (which is the default mode) is the simplest model and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="bb79c-138">Ennek megértéséhez képzeljen el egy forgatókönyvet, amelyben a fogyasztó kiad egy fogadási kérést, majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="bb79c-138">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span>
<span data-ttu-id="bb79c-139">Mivel a Service Bus csak fel az üzenetet, feldolgozottként, majd az alkalmazás újraindításakor és megkezdése az üzenetek fel újra, amikor ki fogja hagyni a az összeomlás előtt feldolgozott üzenetet.</span><span class="sxs-lookup"><span data-stu-id="bb79c-139">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="bb79c-140">A **PeekLock** mód, kap egy két szakaszból álló művelet, amely lehetővé teszi az alkalmazások támogatását, amelyek működését zavarják a hiányzó üzenetek lesz.</span><span class="sxs-lookup"><span data-stu-id="bb79c-140">In **PeekLock** mode, receive becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="bb79c-141">Amikor a Service Bus fogad egy kérést, megkeresi és zárolja a következő feldolgozandó üzenetet, hogy más fogyasztók ne tudják fogadni, majd visszaadja az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="bb79c-141">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="bb79c-142">Miután az alkalmazás befejezi az üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja a második a fogadási folyamat meghívásával **törlése** metódus a fogadott üzenethez.</span><span class="sxs-lookup"><span data-stu-id="bb79c-142">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling **Delete** on the received message.</span></span> <span data-ttu-id="bb79c-143">Amikor a Service Bus látja a **törlése** hívás, azt az üzenetet használnak, és távolítsa el a sorból.</span><span class="sxs-lookup"><span data-stu-id="bb79c-143">When Service Bus sees the **Delete** call, it will mark the message as being consumed and remove it from the queue.</span></span>

<span data-ttu-id="bb79c-144">A következő példa bemutatja, hogyan lehet üzeneteket fogadni, és a feldolgozott használatával **PeekLock** mode (nem az alapértelmezett mód).</span><span class="sxs-lookup"><span data-stu-id="bb79c-144">The following example demonstrates how messages can be received and processed using **PeekLock** mode (not the default mode).</span></span> <span data-ttu-id="bb79c-145">Az alábbi példában nem végtelen hurkot, és feldolgozza a üzenetek megérkeznek a `TestQueue`:</span><span class="sxs-lookup"><span data-stu-id="bb79c-145">The example below does an infinite loop and processes messages as they arrive into our `TestQueue`:</span></span>

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                 service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="bb79c-146">Az alkalmazás-összeomlások és nem olvasható üzenetek kezelése</span><span class="sxs-lookup"><span data-stu-id="bb79c-146">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="bb79c-147">A Service Bus olyan funkciókat biztosít, amelyekkel zökkenőmentesen helyreállíthatja az alkalmazás hibáit vagy az üzenetek feldolgozásának nehézségeit.</span><span class="sxs-lookup"><span data-stu-id="bb79c-147">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="bb79c-148">Ha egy fogadó alkalmazás nem tudja feldolgozni az üzenetet valamilyen okból kifolyólag, majd akkor meghívhatja a **unlockMessage** metódust a fogadott üzenethez (ahelyett, hogy a **deleteMessage** metódus).</span><span class="sxs-lookup"><span data-stu-id="bb79c-148">If a receiver application is unable to process the message for some reason, then it can call the **unlockMessage** method on the received message (instead of the **deleteMessage** method).</span></span> <span data-ttu-id="bb79c-149">Ennek hatására a Service Bus feloldja az üzenet zárolását az üzenetsoron belül, és lehetővé teszi az ugyanazon vagy egy másik fogyasztó alkalmazás általi ismételt fogadását.</span><span class="sxs-lookup"><span data-stu-id="bb79c-149">This causes Service Bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="bb79c-150">Még nincs hozzárendelve az üzenetsorban lévő időtúllépés, és ha az alkalmazás nem tudja feldolgozni az üzenetet, mielőtt a zárolás időtúllépését lejárta (például, ha az alkalmazás összeomlik), akkor a Service Bus automatikusan feloldja az üzenet, és lehetővé teszi az elérhető ismételt fogadását.</span><span class="sxs-lookup"><span data-stu-id="bb79c-150">There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus unlocks the message automatically and makes it available to be received again.</span></span>

<span data-ttu-id="bb79c-151">Abban az esetben, ha az alkalmazás összeomlik, de előtte az üzenet feldolgozása után a **deleteMessage** kérés kiadása, akkor az üzenet újból kézbesítve az alkalmazásnak, amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="bb79c-151">In the event that the application crashes after processing the message but before the **deleteMessage** request is issued, then the message is redelivered to the application when it restarts.</span></span> <span data-ttu-id="bb79c-152">Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*, ez azt jelenti, hogy minden üzenet legalább egyszer fel, de bizonyos helyzetekben előfordulhat ugyanazon üzenet előfordulhat, hogy újbóli kézbesítése..</span><span class="sxs-lookup"><span data-stu-id="bb79c-152">This is often called *At Least Once Processing*; that is, each message is processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="bb79c-153">Ha a forgatókönyvben nem lehetségesek a duplikált üzenetek, akkor az alkalmazásfejlesztőnek további logikát kell az alkalmazásba építenie az üzenetek ismételt kézbesítésének kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="bb79c-153">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="bb79c-154">Ez gyakran érhető el, használja a **getMessageId** metódus az üzenet, amely állandó marad a kézbesítési kísérletek során.</span><span class="sxs-lookup"><span data-stu-id="bb79c-154">This is often achieved using the **getMessageId** method of the message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb79c-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bb79c-155">Next Steps</span></span>
<span data-ttu-id="bb79c-156">Most, hogy megismerte a Service Bus-üzenetsorok alapjait, lásd: [üzenetsorok, témakörök és előfizetések] [ Queues, topics, and subscriptions] további információt.</span><span class="sxs-lookup"><span data-stu-id="bb79c-156">Now that you've learned the basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

<span data-ttu-id="bb79c-157">További információ: [Java fejlesztői központ](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="bb79c-157">For more information, see the [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
