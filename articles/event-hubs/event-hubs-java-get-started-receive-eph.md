---
title: "Események fogadása az Azure Event Hubs Java használatával |} Microsoft Docs"
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
ms.openlocfilehash: 3c1b455e6298367dc50f0943b58f6cf1e7f1c5fd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a><span data-ttu-id="a6e40-103">Események fogadása az Azure Event Hubs Java használatával</span><span class="sxs-lookup"><span data-stu-id="a6e40-103">Receive events from Azure Event Hubs using Java</span></span>


## <a name="introduction"></a><span data-ttu-id="a6e40-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="a6e40-104">Introduction</span></span>
<span data-ttu-id="a6e40-105">Az Event Hubs egy kiválóan méretezhető fogadórendszer, amely is több millió eseményt másodpercenként, az alkalmazás engedélyezése feldolgozni, és elemezze a nagy mennyiségű adatot a csatlakoztatott eszközök és alkalmazások által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="a6e40-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application to process and analyze the massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="a6e40-106">Az adatoknak az Event Hubs szolgáltatásban való összegyűjtését követően, az adatokat átalakíthatja és tárolhatja bármilyen valós idejű elemzési szolgáltató vagy tárolási fürt használatával.</span><span class="sxs-lookup"><span data-stu-id="a6e40-106">Once collected into Event Hubs, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="a6e40-107">További információkért lásd: a [Event Hubs – áttekintés][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="a6e40-107">For more information, see the [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="a6e40-108">Ez az oktatóanyag bemutatja, hogyan használja a Java nyelven írt konzolalkalmazással eseményközpontnak események fogadásához.</span><span class="sxs-lookup"><span data-stu-id="a6e40-108">This tutorial shows how to receive events into an event hub using a console application written in Java.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6e40-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a6e40-109">Prerequisites</span></span>

<span data-ttu-id="a6e40-110">Az oktatóanyag teljesítéséhez szüksége van a következő előfeltételek teljesülését:</span><span class="sxs-lookup"><span data-stu-id="a6e40-110">In order to complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="a6e40-111">A Java-fejlesztőkörnyezet.</span><span class="sxs-lookup"><span data-stu-id="a6e40-111">A Java development environment.</span></span> <span data-ttu-id="a6e40-112">Ebben az oktatóanyagban feltételezzük, hogy [Eclipse](https://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="a6e40-112">For this tutorial, we assume [Eclipse](https://www.eclipse.org/).</span></span>
* <span data-ttu-id="a6e40-113">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="a6e40-113">An active Azure account.</span></span> <br/><span data-ttu-id="a6e40-114">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot.</span><span class="sxs-lookup"><span data-stu-id="a6e40-114">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="a6e40-115">További információkért lásd: <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Ingyenes Azure-fiók létrehozása</a>.</span><span class="sxs-lookup"><span data-stu-id="a6e40-115">For details, see <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Free Trial</a>.</span></span>

## <a name="receive-messages-with-eventprocessorhost-in-java"></a><span data-ttu-id="a6e40-116">Üzenetek fogadása az EventProcessorHost használatával Javában</span><span class="sxs-lookup"><span data-stu-id="a6e40-116">Receive messages with EventProcessorHost in Java</span></span>

<span data-ttu-id="a6e40-117">**EventProcessorHost** egy Java-osztály, amely leegyszerűsíti az események fogadását az Event Hubs kezeli az állandó ellenőrzőpontokat és párhuzamos fogadásokat az adott Event hubs Eseményközpontokból.</span><span class="sxs-lookup"><span data-stu-id="a6e40-117">**EventProcessorHost** is a Java class that simplifies receiving events from Event Hubs by managing persistent checkpoints and parallel receives from those Event Hubs.</span></span> <span data-ttu-id="a6e40-118">EventProcessorHost használ, akkor is feloszthatja az eseményeket több fogadóra, akkor is, ha különböző csomópontokon üzemelnek.</span><span class="sxs-lookup"><span data-stu-id="a6e40-118">Using EventProcessorHost, you can split events across multiple receivers, even when hosted in different nodes.</span></span> <span data-ttu-id="a6e40-119">Ez a példa bemutatja, hogyan használható az EventProcessorHost egyetlen fogadóhoz.</span><span class="sxs-lookup"><span data-stu-id="a6e40-119">This example shows how to use EventProcessorHost for a single receiver.</span></span>

### <a name="create-a-storage-account"></a><span data-ttu-id="a6e40-120">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="a6e40-120">Create a storage account</span></span>
<span data-ttu-id="a6e40-121">EventProcessorHost használatához rendelkeznie kell egy [Azure Storage-fiók][Azure Storage account]:</span><span class="sxs-lookup"><span data-stu-id="a6e40-121">To use EventProcessorHost, you must have an [Azure Storage account][Azure Storage account]:</span></span>

1. <span data-ttu-id="a6e40-122">Jelentkezzen be a [Azure-portálon][Azure portal], és kattintson a **+ új** a képernyő bal oldalán.</span><span class="sxs-lookup"><span data-stu-id="a6e40-122">Log on to the [Azure portal][Azure portal], and click **+ New** on the left-hand side of the screen.</span></span>
2. <span data-ttu-id="a6e40-123">Kattintson a **Tárolás**, majd a **Tárfiók** elemre.</span><span class="sxs-lookup"><span data-stu-id="a6e40-123">Click **Storage**, then click **Storage account**.</span></span> <span data-ttu-id="a6e40-124">A **Tárfiókok létrehozása** panelen írja be a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="a6e40-124">In the **Create storage account** blade, type a name for the storage account.</span></span> <span data-ttu-id="a6e40-125">Fejezze be a mezőket, válassza ki a kívánt régiót, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a6e40-125">Complete the rest of the fields, select your desired region, and then click **Create**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. <span data-ttu-id="a6e40-126">Kattintson az imént létrehozott tárfiókra, majd a **Manage Access Keys** (Elérési kulcsok kezelése) lehetőségre:</span><span class="sxs-lookup"><span data-stu-id="a6e40-126">Click the newly created storage account, and then click **Manage Access Keys**:</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    <span data-ttu-id="a6e40-127">Másolja az elsődleges elérési kulcsot egy ideiglenes helyre, az oktatóanyag későbbi részében használni.</span><span class="sxs-lookup"><span data-stu-id="a6e40-127">Copy the primary access key to a temporary location, to use later in this tutorial.</span></span>

### <a name="create-a-java-project-using-the-eventprocessor-host"></a><span data-ttu-id="a6e40-128">Java-projekt létrehozása az EventProcessor Hosttal</span><span class="sxs-lookup"><span data-stu-id="a6e40-128">Create a Java project using the EventProcessor Host</span></span>
<span data-ttu-id="a6e40-129">Az Event Hubs Java ügyfélkódtár a Maven-projektek használható a [Maven központi tárházban][Maven Package], és a következő függőségi nyilatkozat az Maven project fájl használatával lehet hivatkozni:</span><span class="sxs-lookup"><span data-stu-id="a6e40-129">The Java client library for Event Hubs is available for use in Maven projects from the [Maven Central Repository][Maven Package], and can be referenced using the following dependency declaration inside your Maven project file:</span></span>    

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

<span data-ttu-id="a6e40-130">A különböző típusú buildkörnyezeteket, explicit módon beszerezheti a legfrissebb kiadott JAR fájlok a [Maven központi tárházban] [ Maven Package] vagy [a kiadási terjesztési pontot a Githubon](https://github.com/Azure/azure-event-hubs/releases).</span><span class="sxs-lookup"><span data-stu-id="a6e40-130">For different types of build environments, you can explicitly obtain the latest released JAR files from the [Maven Central Repository][Maven Package] or from [the release distribution point on GitHub](https://github.com/Azure/azure-event-hubs/releases).</span></span>  

1. <span data-ttu-id="a6e40-131">A következő mintában először hozzon létre egy új Maven-projektet egy konzol/felületalkalmazáshoz a kedvenc Java-fejlesztőkörnyezetében.</span><span class="sxs-lookup"><span data-stu-id="a6e40-131">For the following sample, first create a new Maven project for a console/shell application in your favorite Java development environment.</span></span> <span data-ttu-id="a6e40-132">Az osztály `ErrorNotificationHandler`.</span><span class="sxs-lookup"><span data-stu-id="a6e40-132">The class is called `ErrorNotificationHandler`.</span></span>     
   
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
2. <span data-ttu-id="a6e40-133">A következő kóddal hozzon létre egy `EventProcessor` nevű új osztályt.</span><span class="sxs-lookup"><span data-stu-id="a6e40-133">Use the following code to create a new class called `EventProcessor`.</span></span>
   
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
3. <span data-ttu-id="a6e40-134">Hozzon létre egy további osztályt `EventProcessorSample`, az alábbi kód használatával.</span><span class="sxs-lookup"><span data-stu-id="a6e40-134">Create one more class called `EventProcessorSample`, using the following code.</span></span>
   
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
   
            System.out.println("Press enter to stop");
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
4. <span data-ttu-id="a6e40-135">Cserélje le a következő mezőket a event hub és a tárolási fiók létrehozásakor használt értékek.</span><span class="sxs-lookup"><span data-stu-id="a6e40-135">Replace the following fields with the values used when you created the event hub and storage account.</span></span>
   
    ```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
   
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
   
    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [!NOTE]
> <span data-ttu-id="a6e40-136">Ez az oktatóprogram az EventProcessorHost egyetlen példányát használja.</span><span class="sxs-lookup"><span data-stu-id="a6e40-136">This tutorial uses a single instance of EventProcessorHost.</span></span> <span data-ttu-id="a6e40-137">Átviteli sebesség növelése érdekében ajánlott, hogy futtatja az EventProcessorHost, több példánya lehetőleg külön gépeken.</span><span class="sxs-lookup"><span data-stu-id="a6e40-137">To increase throughput, it is recommended that you run multiple instances of EventProcessorHost, preferably on separate machines.</span></span>  <span data-ttu-id="a6e40-138">Ez biztosítja a redundanciát is.</span><span class="sxs-lookup"><span data-stu-id="a6e40-138">This provides redundancy as well.</span></span> <span data-ttu-id="a6e40-139">Ilyen esetekben a különböző példányok automatikusan koordinálnak egymással a fogadott események terhelésének kiegyenlítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a6e40-139">In those cases, the various instances automatically coordinate with each other in order to load balance the received events.</span></span> <span data-ttu-id="a6e40-140">Ha több fogadóval szeretné feldolgoztatni az *összes* eseményt, a **ConsumerGroup** szolgáltatást kell használnia.</span><span class="sxs-lookup"><span data-stu-id="a6e40-140">If you want multiple receivers to each process *all* the events, you must use the **ConsumerGroup** concept.</span></span> <span data-ttu-id="a6e40-141">Ha több gépről fogad eseményeket, célszerű lehet az azokat futtató gépeken (vagy szerepkörökön) alapuló neveket adni az EventProcessorHost példányoknak.</span><span class="sxs-lookup"><span data-stu-id="a6e40-141">When receiving events from different machines, it might be useful to specify names for EventProcessorHost instances based on the machines (or roles) in which they are deployed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="a6e40-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a6e40-142">Next steps</span></span>
<span data-ttu-id="a6e40-143">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="a6e40-143">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="a6e40-144">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="a6e40-144">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="a6e40-145">Event Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6e40-145">Create an Event Hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="a6e40-146">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="a6e40-146">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
