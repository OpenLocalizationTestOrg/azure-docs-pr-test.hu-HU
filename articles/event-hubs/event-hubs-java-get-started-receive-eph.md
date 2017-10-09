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
# <a name="receive-events-from-azure-event-hubs-using-java"></a>Események fogadása az Azure Event Hubs Java használatával


## <a name="introduction"></a>Bevezetés
Az Event Hubs egy kiválóan méretezhető fogadórendszer, melyek több millió eseményt másodpercenként, egy alkalmazás tooprocess engedélyezése és elemezheti az adatokat a csatlakoztatott eszközök és alkalmazások által létrehozott nagy mennyiségű hello. Az adatoknak az Event Hubs szolgáltatásban való összegyűjtését követően, az adatokat átalakíthatja és tárolhatja bármilyen valós idejű elemzési szolgáltató vagy tárolási fürt használatával.

További információkért lásd: hello [Event Hubs – áttekintés][Event Hubs overview].

Ez az oktatóanyag bemutatja, hogyan használja a Java nyelven írt konzolalkalmazással eseményközpontnak tooreceive események.

## <a name="prerequisites"></a>Előfeltételek

A sorrend toocomplete ebben az oktatóanyagban kell hello a következő előfeltételek:

* A Java-fejlesztőkörnyezet. Ebben az oktatóanyagban feltételezzük, hogy [Eclipse](https://www.eclipse.org/).
* Aktív Azure-fiók. <br/>Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot. További információkért lásd: <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Ingyenes Azure-fiók létrehozása</a>.

## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Üzenetek fogadása az EventProcessorHost használatával Javában

**EventProcessorHost** egy Java-osztály, amely leegyszerűsíti az események fogadását az Event Hubs kezeli az állandó ellenőrzőpontokat és párhuzamos fogadásokat az adott Event hubs Eseményközpontokból. EventProcessorHost használ, akkor is feloszthatja az eseményeket több fogadóra, akkor is, ha különböző csomópontokon üzemelnek. A példa bemutatja, hogyan toouse EventProcessorHost egyetlen fogadóhoz.

### <a name="create-a-storage-account"></a>Create a storage account
toouse EventProcessorHost, rendelkeznie kell egy [Azure Storage-fiók][Azure Storage account]:

1. Jelentkezzen be toohello [Azure-portálon][Azure portal], és kattintson a **+ új** hello hello képernyő bal oldalán.
2. Kattintson a **Tárolás**, majd a **Tárfiók** elemre. A hello **storage-fiók létrehozása** panelen adja meg a hello storage-fiók nevét. Végezze el a többi hello hello mezők, válassza ki a kívánt régiót, és kattintson **létrehozása**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Kattintson az újonnan létrehozott hello tárfiókra, és kattintson a **elérési kulcsok kezelése**:
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Hello elsődleges elérési kulcs tooa ideiglenes helyre, az oktatóanyag későbbi részében toouse másolja.

### <a name="create-a-java-project-using-hello-eventprocessor-host"></a>Hozzon létre egy Java-projektet hello EventProcessor állomás használata
hello Java ügyféloldali kódtára a Event Hubs Maven-projektek a hello használható [Maven központi tárházban][Maven Package], és a függőségi deklaráció található a következő hello használatával lehet hivatkozni a Maven project fájlt:    

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

A különböző típusú buildkörnyezeteket, explicit módon szerezhet be legfrissebb kiadott hello JAR fájlok hello [Maven központi tárházban] [ Maven Package] vagy [hello kiadás terjesztési pontot a GitHub](https://github.com/Azure/azure-event-hubs/releases).  

1. A következő minta hello először létre kell hoznia egy új Maven project konzol/rendszerhéj alkalmazáshoz a kedvenc Java fejlesztői környezetben. hello osztály `ErrorNotificationHandler`.     
   
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
2. Használjon hello következő kódot egy új osztályt nevű toocreate `EventProcessor`.
   
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
3. Hozzon létre egy további osztályt `EventProcessorSample`a következő kód hello.
   
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
4. Cserélje le a következő mezők hello event hub és a storage-fiók létrehozásakor használt hello értékekkel hello.
   
    ```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
   
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
   
    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [!NOTE]
> Ez az oktatóprogram az EventProcessorHost egyetlen példányát használja. tooincrease átviteli, ajánlott a futtatott EventProcessorHost, több példánya lehetőleg külön gépeken.  Ez biztosítja a redundanciát is. Ezekben az esetekben hello különböző példányok automatikusan egyeztessen egymáshoz rendelés tooload egyenleg hello a fogadott események. Ha azt szeretné, hogy több fogadóval tooeach folyamat *összes* események, hello hello kell használnia **ConsumerGroup** fogalom. Események fogadása különböző számítógépeken, ha azokat a lehet hasznos toospecify EventProcessorHost példányok hello gépeken (vagy szerepkörökön) alapuló neveket.
> 
> 

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)
* [Event Hub létrehozása](event-hubs-create.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
