---
title: "az Azure Event Hubs Java használatával aaaReceive események |} Microsoft Docs"
description: "Első lépések fogadását az Event Hubs Java használatával"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 38e3be53-251c-488f-a856-9a500f41b6ca
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 05414a22e6616296752c678bb0af887d6f070c12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a><span data-ttu-id="30108-103">Események fogadása az Azure Event Hubs Java használatával</span><span class="sxs-lookup"><span data-stu-id="30108-103">Receive events from Azure Event Hubs using Java</span></span>


## <a name="introduction"></a><span data-ttu-id="30108-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="30108-104">Introduction</span></span>
<span data-ttu-id="30108-105">Az Event Hubs egy kiválóan méretezhető fogadórendszer, melyek több millió eseményt másodpercenként, egy alkalmazás tooprocess engedélyezése és elemezheti az adatokat a csatlakoztatott eszközök és alkalmazások által létrehozott nagy mennyiségű hello.</span><span class="sxs-lookup"><span data-stu-id="30108-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application tooprocess and analyze hello massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="30108-106">Az adatoknak az Event Hubs szolgáltatásban való összegyűjtését követően, az adatokat átalakíthatja és tárolhatja bármilyen valós idejű elemzési szolgáltató vagy tárolási fürt használatával.</span><span class="sxs-lookup"><span data-stu-id="30108-106">Once collected into Event Hubs, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="30108-107">További információkért lásd: hello [Event Hubs – áttekintés][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="30108-107">For more information, see hello [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="30108-108">Ez az oktatóanyag bemutatja, hogyan használja a Java nyelven írt konzolalkalmazással eseményközpontnak tooreceive események.</span><span class="sxs-lookup"><span data-stu-id="30108-108">This tutorial shows how tooreceive events into an event hub using a console application written in Java.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30108-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="30108-109">Prerequisites</span></span>

<span data-ttu-id="30108-110">A sorrend toocomplete ebben az oktatóanyagban kell hello a következő előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="30108-110">In order toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="30108-111">A Java-fejlesztőkörnyezet.</span><span class="sxs-lookup"><span data-stu-id="30108-111">A Java development environment.</span></span> <span data-ttu-id="30108-112">Ebben az oktatóanyagban feltételezzük, hogy [Eclipse](https://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="30108-112">For this tutorial, we assume [Eclipse](https://www.eclipse.org/).</span></span>
* <span data-ttu-id="30108-113">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="30108-113">An active Azure account.</span></span> <br/><span data-ttu-id="30108-114">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot.</span><span class="sxs-lookup"><span data-stu-id="30108-114">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="30108-115">További információkért lásd: <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Ingyenes Azure-fiók létrehozása</a>.</span><span class="sxs-lookup"><span data-stu-id="30108-115">For details, see <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Free Trial</a>.</span></span>

## <a name="receive-messages-with-eventprocessorhost-in-java"></a><span data-ttu-id="30108-116">Üzenetek fogadása az EventProcessorHost használatával Javában</span><span class="sxs-lookup"><span data-stu-id="30108-116">Receive messages with EventProcessorHost in Java</span></span>

<span data-ttu-id="30108-117">**EventProcessorHost** egy Java-osztály, amely leegyszerűsíti az események fogadását az Event Hubs kezeli az állandó ellenőrzőpontokat és párhuzamos fogadásokat az adott Event hubs Eseményközpontokból.</span><span class="sxs-lookup"><span data-stu-id="30108-117">**EventProcessorHost** is a Java class that simplifies receiving events from Event Hubs by managing persistent checkpoints and parallel receives from those Event Hubs.</span></span> <span data-ttu-id="30108-118">EventProcessorHost használ, akkor is feloszthatja az eseményeket több fogadóra, akkor is, ha különböző csomópontokon üzemelnek.</span><span class="sxs-lookup"><span data-stu-id="30108-118">Using EventProcessorHost, you can split events across multiple receivers, even when hosted in different nodes.</span></span> <span data-ttu-id="30108-119">A példa bemutatja, hogyan toouse EventProcessorHost egyetlen fogadóhoz.</span><span class="sxs-lookup"><span data-stu-id="30108-119">This example shows how toouse EventProcessorHost for a single receiver.</span></span>

### <a name="create-a-storage-account"></a><span data-ttu-id="30108-120">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="30108-120">Create a storage account</span></span>
<span data-ttu-id="30108-121">toouse EventProcessorHost, rendelkeznie kell egy [Azure Storage-fiók][Azure Storage account]:</span><span class="sxs-lookup"><span data-stu-id="30108-121">toouse EventProcessorHost, you must have an [Azure Storage account][Azure Storage account]:</span></span>

1. <span data-ttu-id="30108-122">Jelentkezzen be toohello [Azure-portálon][Azure portal], és kattintson a **+ új** hello hello képernyő bal oldalán.</span><span class="sxs-lookup"><span data-stu-id="30108-122">Log on toohello [Azure portal][Azure portal], and click **+ New** on hello left-hand side of hello screen.</span></span>
2. <span data-ttu-id="30108-123">Kattintson a **Tárolás**, majd a **Tárfiók** elemre.</span><span class="sxs-lookup"><span data-stu-id="30108-123">Click **Storage**, then click **Storage account**.</span></span> <span data-ttu-id="30108-124">A hello **storage-fiók létrehozása** panelen adja meg a hello storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="30108-124">In hello **Create storage account** blade, type a name for hello storage account.</span></span> <span data-ttu-id="30108-125">Végezze el a többi hello hello mezők, válassza ki a kívánt régiót, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="30108-125">Complete hello rest of hello fields, select your desired region, and then click **Create**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. <span data-ttu-id="30108-126">Kattintson az újonnan létrehozott hello tárfiókra, és kattintson a **elérési kulcsok kezelése**:</span><span class="sxs-lookup"><span data-stu-id="30108-126">Click hello newly created storage account, and then click **Manage Access Keys**:</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    <span data-ttu-id="30108-127">Hello elsődleges elérési kulcs tooa ideiglenes helyre, az oktatóanyag későbbi részében toouse másolja.</span><span class="sxs-lookup"><span data-stu-id="30108-127">Copy hello primary access key tooa temporary location, toouse later in this tutorial.</span></span>

### <a name="create-a-java-project-using-hello-eventprocessor-host"></a><span data-ttu-id="30108-128">Hozzon létre egy Java-projektet hello EventProcessor állomás használata</span><span class="sxs-lookup"><span data-stu-id="30108-128">Create a Java project using hello EventProcessor Host</span></span>
<span data-ttu-id="30108-129">hello Java ügyféloldali kódtára a Event Hubs Maven-projektek a hello használható [Maven központi tárházban][Maven Package], és a függőségi deklaráció található a következő hello használatával lehet hivatkozni a Maven project fájlt:</span><span class="sxs-lookup"><span data-stu-id="30108-129">hello Java client library for Event Hubs is available for use in Maven projects from hello [Maven Central Repository][Maven Package], and can be referenced using hello following dependency declaration inside your Maven project file:</span></span>    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-eventhubs-eph</artifactId>
  <version>0.14.0</version>
</dependency>
```

<span data-ttu-id="30108-130">A különböző típusú buildkörnyezeteket, explicit módon szerezhet be legfrissebb kiadott hello JAR fájlok hello [Maven központi tárházban] [ Maven Package] vagy [hello kiadás terjesztési pontot a GitHub](https://github.com/Azure/azure-event-hubs/releases).</span><span class="sxs-lookup"><span data-stu-id="30108-130">For different types of build environments, you can explicitly obtain hello latest released JAR files from hello [Maven Central Repository][Maven Package] or from [hello release distribution point on GitHub](https://github.com/Azure/azure-event-hubs/releases).</span></span>  

1. <span data-ttu-id="30108-131">A következő minta hello először létre kell hoznia egy új Maven project konzol/rendszerhéj alkalmazáshoz a kedvenc Java fejlesztői környezetben.</span><span class="sxs-lookup"><span data-stu-id="30108-131">For hello following sample, first create a new Maven project for a console/shell application in your favorite Java development environment.</span></span> <span data-ttu-id="30108-132">hello osztály `ErrorNotificationHandler`.</span><span class="sxs-lookup"><span data-stu-id="30108-132">hello class is called `ErrorNotificationHandler`.</span></span>     
   
    ```java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   
    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```
2. <span data-ttu-id="30108-133">Használjon hello következő kódot egy új osztályt nevű toocreate `EventProcessor`.</span><span class="sxs-lookup"><span data-stu-id="30108-133">Use hello following code toocreate a new class called `EventProcessor`.</span></span>
   
    ```java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;
   
    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;
   
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }
   
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
   
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }
   
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
   
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```
3. <span data-ttu-id="30108-134">Hozzon létre egy további osztályt `EventProcessorSample`a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="30108-134">Create one more class called `EventProcessorSample`, using hello following code.</span></span>
   
    ```java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;
   
    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";
   
            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
   
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
   
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
   
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }
   
            System.out.println("Press enter toostop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
   
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
   
            System.out.println("End of sample");
        }
    }
    ```
4. <span data-ttu-id="30108-135">Cserélje le a következő mezők hello event hub és a storage-fiók létrehozásakor használt hello értékekkel hello.</span><span class="sxs-lookup"><span data-stu-id="30108-135">Replace hello following fields with hello values used when you created hello event hub and storage account.</span></span>
   
    ```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
   
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
   
    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [!NOTE]
> <span data-ttu-id="30108-136">Ez az oktatóprogram az EventProcessorHost egyetlen példányát használja.</span><span class="sxs-lookup"><span data-stu-id="30108-136">This tutorial uses a single instance of EventProcessorHost.</span></span> <span data-ttu-id="30108-137">tooincrease átviteli, ajánlott a futtatott EventProcessorHost, több példánya lehetőleg külön gépeken.</span><span class="sxs-lookup"><span data-stu-id="30108-137">tooincrease throughput, it is recommended that you run multiple instances of EventProcessorHost, preferably on separate machines.</span></span>  <span data-ttu-id="30108-138">Ez biztosítja a redundanciát is.</span><span class="sxs-lookup"><span data-stu-id="30108-138">This provides redundancy as well.</span></span> <span data-ttu-id="30108-139">Ezekben az esetekben hello különböző példányok automatikusan egyeztessen egymáshoz rendelés tooload egyenleg hello a fogadott események.</span><span class="sxs-lookup"><span data-stu-id="30108-139">In those cases, hello various instances automatically coordinate with each other in order tooload balance hello received events.</span></span> <span data-ttu-id="30108-140">Ha azt szeretné, hogy több fogadóval tooeach folyamat *összes* események, hello hello kell használnia **ConsumerGroup** fogalom.</span><span class="sxs-lookup"><span data-stu-id="30108-140">If you want multiple receivers tooeach process *all* hello events, you must use hello **ConsumerGroup** concept.</span></span> <span data-ttu-id="30108-141">Események fogadása különböző számítógépeken, ha azokat a lehet hasznos toospecify EventProcessorHost példányok hello gépeken (vagy szerepkörökön) alapuló neveket.</span><span class="sxs-lookup"><span data-stu-id="30108-141">When receiving events from different machines, it might be useful toospecify names for EventProcessorHost instances based on hello machines (or roles) in which they are deployed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="30108-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="30108-142">Next steps</span></span>
<span data-ttu-id="30108-143">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="30108-143">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="30108-144">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="30108-144">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="30108-145">Event Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="30108-145">Create an Event Hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="30108-146">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="30108-146">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
