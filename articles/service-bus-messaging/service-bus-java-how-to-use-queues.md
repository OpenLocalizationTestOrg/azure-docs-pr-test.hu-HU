---
title: "a Java várólisták aaaHow toouse Azure Service Bus |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Service Bus várólisták az Azure-ban. A Kódminták Java nyelven."
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
ms.openlocfilehash: f68e941438134090c5eee53459e7667bda13ff3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-java"></a><span data-ttu-id="fdfd7-104">Hogyan toouse Service Bus Java várólisták</span><span class="sxs-lookup"><span data-stu-id="fdfd7-104">How toouse Service Bus queues with Java</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="fdfd7-105">Ez a cikk ismerteti, hogyan toouse Service Bus-üzenetsorok.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-105">This article describes how toouse Service Bus queues.</span></span> <span data-ttu-id="fdfd7-106">hello minták Java nyelven íródtak, és használja a hello [Javához készült Azure SDK][Azure SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="fdfd7-106">hello samples are written in Java and use hello [Azure SDK for Java][Azure SDK for Java].</span></span> <span data-ttu-id="fdfd7-107">hello tárgyalt forgatókönyvekben szerepel a **üzenetsorok létrehozása**, **üzenetek küldése és fogadása**, és **várólisták törlése**.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-107">hello scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="fdfd7-108">Az alkalmazás toouse Service Bus konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fdfd7-108">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="fdfd7-109">Győződjön meg arról, hogy telepítette a hello [Javához készült Azure SDK] [ Azure SDK for Java] Ez a minta létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-109">Make sure you have installed hello [Azure SDK for Java][Azure SDK for Java] before building this sample.</span></span> <span data-ttu-id="fdfd7-110">Eclipse használatakor telepítése hello [Eclipse Azure eszköztára] [ Azure Toolkit for Eclipse] hello Azure SDK for Java foglal magában.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-110">If you are using Eclipse, you can install hello [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse] that includes hello Azure SDK for Java.</span></span> <span data-ttu-id="fdfd7-111">Ezután hozzáadhat hello **Microsoft Azure-könyvtárakban Java** tooyour projekt:</span><span class="sxs-lookup"><span data-stu-id="fdfd7-111">You can then add hello **Microsoft Azure Libraries for Java** tooyour project:</span></span>

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

<span data-ttu-id="fdfd7-112">Adja hozzá a következő hello `import` hello Java fájl elejéhez utasítások toohello:</span><span class="sxs-lookup"><span data-stu-id="fdfd7-112">Add hello following `import` statements toohello top of hello Java file:</span></span>

```java
// Include hello following imports toouse Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a><span data-ttu-id="fdfd7-113">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="fdfd7-113">Create a queue</span></span>
<span data-ttu-id="fdfd7-114">Felügyeleti műveletek a Service Bus-üzenetsorok használatával is kell végezni a **ServiceBusContract** osztály.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-114">Management operations for Service Bus queues can be performed via the **ServiceBusContract** class.</span></span> <span data-ttu-id="fdfd7-115">A **ServiceBusContract** objektum a megfelelő konfiguráció, amely magában foglalja az engedélyek toomanage SAS-jogkivonat és hello összeállított **ServiceBusContract** osztály az egyetlen pont, hello a kommunikáció az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-115">A **ServiceBusContract** object is constructed with an appropriate configuration that encapsulates the SAS token with permissions toomanage it, and hello **ServiceBusContract** class is hello sole point of communication with Azure.</span></span>

<span data-ttu-id="fdfd7-116">Hello **ServiceBusService** osztály módszerek toocreate biztosít, enumerálásához és törléséhez várólisták.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-116">hello **ServiceBusService** class provides methods toocreate, enumerate, and delete queues.</span></span> <span data-ttu-id="fdfd7-117">az alábbi példa hogyan hello egy **ServiceBusService** objektum egy várólista nevű használt toocreate lehet `TestQueue`, nevű névtérrel `HowToSample`:</span><span class="sxs-lookup"><span data-stu-id="fdfd7-117">hello example below shows how a **ServiceBusService** object can be used toocreate a queue named `TestQueue`, with a namespace named `HowToSample`:</span></span>

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

<span data-ttu-id="fdfd7-118">Módszer áll rendelkezésre a `QueueInfo` , amelyek lehetővé teszik a hello várólista toobe beállított tulajdonságait (például: tooset hello alapértelmezett--élettartama (TTL) értéke alkalmazott toobe toomessages küldött toohello várólista).</span><span class="sxs-lookup"><span data-stu-id="fdfd7-118">There are methods on `QueueInfo` that allow properties of hello queue toobe tuned (for example: tooset hello default time-to-live (TTL) value toobe applied toomessages sent toohello queue).</span></span> <span data-ttu-id="fdfd7-119">hello következő példa bemutatja, hogyan toocreate várólista nevű `TestQueue` 5 GB maximális mérettel:</span><span class="sxs-lookup"><span data-stu-id="fdfd7-119">hello following example shows how toocreate a queue named `TestQueue` with a maximum size of 5GB:</span></span>

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

<span data-ttu-id="fdfd7-120">Vegye figyelembe, hogy használhatja-e hello `listQueues` metódusa **ServiceBusContract** toocheck objektumokat, ha a várólista megadott névvel már létezik egy szolgáltatásnévtérben.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-120">Note that you can use hello `listQueues` method on **ServiceBusContract** objects toocheck if a queue with a specified name already exists within a service namespace.</span></span>

## <a name="send-messages-tooa-queue"></a><span data-ttu-id="fdfd7-121">Üzenetek tooa várólista küldése</span><span class="sxs-lookup"><span data-stu-id="fdfd7-121">Send messages tooa queue</span></span>
<span data-ttu-id="fdfd7-122">toosend üzenet tooa Service Bus-üzenetsorba, az alkalmazás beolvassa a **ServiceBusContract** objektum.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-122">toosend a message tooa Service Bus queue, your application obtains a **ServiceBusContract** object.</span></span> <span data-ttu-id="fdfd7-123">a következő kód bemutatja hogyan hello toosend hello üzenetet `TestQueue` hello a korábban létrehozott várólista `HowToSample` névtér:</span><span class="sxs-lookup"><span data-stu-id="fdfd7-123">hello following code shows how toosend a message for hello `TestQueue` queue previously created in hello `HowToSample` namespace:</span></span>

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

<span data-ttu-id="fdfd7-124">Küldött üzeneteket, és kapott a Service Bus üzenetsorok hello példánya [BrokeredMessage] [ BrokeredMessage] osztály.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-124">Messages sent to, and received from Service Bus queues are instances of hello [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="fdfd7-125">[BrokeredMessage] [ BrokeredMessage] objektumok rendelkeznek egy szabványos tulajdonságkészlettel (például [címke](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) és [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), amely egyéni használt toohold dictionary az alkalmazás-specifikus tulajdonságait, és egy tetszőleges alkalmazásadatokból álló törzzsel.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-125">[BrokeredMessage][BrokeredMessage] objects have a set of standard properties (such as [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) and [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), a dictionary that is used toohold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="fdfd7-126">Az alkalmazás beállíthatja hello üzenet törzsét hello bármilyen szerializálható objektumnak hello hello konstruktorának való átadásával [BrokeredMessage][BrokeredMessage], és a megfelelő szerializáló hello használja tooserialize hello objektum.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-126">An application can set hello body of hello message by passing any serializable object into hello constructor of hello [BrokeredMessage][BrokeredMessage], and hello appropriate serializer will then be used tooserialize hello object.</span></span> <span data-ttu-id="fdfd7-127">Másik lehetőségként megadhat egy **java. IO. InputStream** objektum.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-127">Alternatively, you can provide a **java.IO.InputStream** object.</span></span>

<span data-ttu-id="fdfd7-128">hello következő példa bemutatja, hogyan toosend öt teszt üzenetek toothe `TestQueue` **MessageSender** azt beolvasott hello előző kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="fdfd7-128">hello following example demonstrates how toosend five test messages toothe `TestQueue` **MessageSender** we obtained in hello previous code snippet:</span></span>

```java
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for hello body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message toohello queue
     service.sendQueueMessage("TestQueue", message);
}
```

<span data-ttu-id="fdfd7-129">Service Bus-üzenetsorok támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="fdfd7-129">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="fdfd7-130">hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-130">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="fdfd7-131">Az üzenetsorban tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello üzenetsor által tárolt hello üzenetek teljes mérete.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-131">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="fdfd7-132">Az üzenetsor ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-132">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="fdfd7-133">Üzenetek fogadása egy üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="fdfd7-133">Receive messages from a queue</span></span>
<span data-ttu-id="fdfd7-134">hello elsődleges módon tooreceive üzenetek várólistából való várólistából toouse egy **ServiceBusContract** objektum.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-134">hello primary way tooreceive messages from a queue is toouse a **ServiceBusContract** object.</span></span> <span data-ttu-id="fdfd7-135">A fogadott üzenetek a két különböző módban tudnak működni: **ReceiveAndDelete** és **PeekLock**.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-135">Received messages can work in two different modes: **ReceiveAndDelete** and **PeekLock**.</span></span>

<span data-ttu-id="fdfd7-136">Hello használatakor **ReceiveAndDelete** mód, kap egy egylépéses művelet – Ez azt jelenti, hogy amikor a Service Bus egy olvasási kérést kap a sorhoz, azt hello üzenetet, feldolgozottként jelöli meg és toohello alkalmazás visszaadja.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-136">When using hello **ReceiveAndDelete** mode, receive is a single-shot operation - that is, when Service Bus receives a read request for a message in a queue, it marks hello message as being consumed and returns it toohello application.</span></span> <span data-ttu-id="fdfd7-137">**ReceiveAndDelete** módban (amely hello alapértelmezett mód) hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-137">**ReceiveAndDelete** mode (which is hello default mode) is hello simplest model and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="fdfd7-138">toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-138">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span>
<span data-ttu-id="fdfd7-139">Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-139">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="fdfd7-140">A **PeekLock** mód, kap egy két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-140">In **PeekLock** mode, receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="fdfd7-141">A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-141">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="fdfd7-142">Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával **törlése** hello érkezett üzenet.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-142">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling **Delete** on hello received message.</span></span> <span data-ttu-id="fdfd7-143">Amikor a Service Bus látja a hello **törlése** hívás, azt feldolgozottként hello üzenetet, és távolítsa el azt a hello várólista.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-143">When Service Bus sees hello **Delete** call, it will mark hello message as being consumed and remove it from hello queue.</span></span>

<span data-ttu-id="fdfd7-144">hello következő példa bemutatja, hogyan lehet üzeneteket fogadni, és a feldolgozott használatával **PeekLock** mód nem hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-144">hello following example demonstrates how messages can be received and processed using **PeekLock** mode (not hello default mode).</span></span> <span data-ttu-id="fdfd7-145">hello az alábbi példa nem végtelen hurkot és üzeneteket dolgozza fel a megérkeznek a `TestQueue`:</span><span class="sxs-lookup"><span data-stu-id="fdfd7-145">hello example below does an infinite loop and processes messages as they arrive into our `TestQueue`:</span></span>

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
            // Display hello queue message.
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
            // Added toohandle no more messages.
            // Could instead wait for more messages toobe added.
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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="fdfd7-146">Hogyan toohandle omlik össze és nem olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="fdfd7-146">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="fdfd7-147">Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-147">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="fdfd7-148">Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello **unlockMessage** metódust a fogadott üdvözlőüzenetére (helyett hello **deleteMessage** metódus).</span><span class="sxs-lookup"><span data-stu-id="fdfd7-148">If a receiver application is unable tooprocess hello message for some reason, then it can call hello **unlockMessage** method on hello received message (instead of hello **deleteMessage** method).</span></span> <span data-ttu-id="fdfd7-149">A Service Bus toounlock hello üzenet-várólista hello belül, és tegye elérhetővé toobe ismételt fogadását oka vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-149">This causes Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="fdfd7-150">Még nincs hozzárendelve az üzenetsorban lévő időtúllépés, és ha hello alkalmazás sikertelen tooprocess hello üzenet járjon le a zárolás időtúllépését (például ha hello alkalmazás összeomlik), majd a Service Bus automatikusan feloldja az üdvözlőüzenetére és rendelkezésre álló toobe újbóli fogadását teszi.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-150">There is also a timeout associated with a message locked within the queue, and if hello application fails tooprocess hello message before the lock timeout expires (for example, if hello application crashes), then Service Bus unlocks hello message automatically and makes it available toobe received again.</span></span>

<span data-ttu-id="fdfd7-151">A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello **deleteMessage** kérés kiadása, majd hello üzenet újbóli kézbesítése toohello alkalmazás amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-151">In hello event that hello application crashes after processing hello message but before hello **deleteMessage** request is issued, then hello message is redelivered toohello application when it restarts.</span></span> <span data-ttu-id="fdfd7-152">Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*; Ez azt jelenti, hogy minden üzenet legalább egyszer fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-152">This is often called *At Least Once Processing*; that is, each message is processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="fdfd7-153">Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-153">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="fdfd7-154">Ez gyakran érhető el hello segítségével **getMessageId** metódus hello üzenet, amely állandó marad a kézbesítési kísérletek során.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-154">This is often achieved using hello **getMessageId** method of hello message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fdfd7-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdfd7-155">Next Steps</span></span>
<span data-ttu-id="fdfd7-156">Most, hogy megismerte a Service Bus-üzenetsorok alapjait hello, lásd: [üzenetsorok, témakörök és előfizetések] [ Queues, topics, and subscriptions] további információt.</span><span class="sxs-lookup"><span data-stu-id="fdfd7-156">Now that you've learned hello basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

<span data-ttu-id="fdfd7-157">További információkért lásd: hello [Java fejlesztői központ](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="fdfd7-157">For more information, see hello [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
