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
# <a name="how-toouse-service-bus-queues-with-java"></a>Hogyan toouse Service Bus Java várólisták
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ez a cikk ismerteti, hogyan toouse Service Bus-üzenetsorok. hello minták Java nyelven íródtak, és használja a hello [Javához készült Azure SDK][Azure SDK for Java]. hello tárgyalt forgatókönyvekben szerepel a **üzenetsorok létrehozása**, **üzenetek küldése és fogadása**, és **várólisták törlése**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-toouse-service-bus"></a>Az alkalmazás toouse Service Bus konfigurálása
Győződjön meg arról, hogy telepítette a hello [Javához készült Azure SDK] [ Azure SDK for Java] Ez a minta létrehozása előtt. Eclipse használatakor telepítése hello [Eclipse Azure eszköztára] [ Azure Toolkit for Eclipse] hello Azure SDK for Java foglal magában. Ezután hozzáadhat hello **Microsoft Azure-könyvtárakban Java** tooyour projekt:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Adja hozzá a következő hello `import` hello Java fájl elejéhez utasítások toohello:

```java
// Include hello following imports toouse Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Üzenetsor létrehozása
Felügyeleti műveletek a Service Bus-üzenetsorok használatával is kell végezni a **ServiceBusContract** osztály. A **ServiceBusContract** objektum a megfelelő konfiguráció, amely magában foglalja az engedélyek toomanage SAS-jogkivonat és hello összeállított **ServiceBusContract** osztály az egyetlen pont, hello a kommunikáció az Azure-ral.

Hello **ServiceBusService** osztály módszerek toocreate biztosít, enumerálásához és törléséhez várólisták. az alábbi példa hogyan hello egy **ServiceBusService** objektum egy várólista nevű használt toocreate lehet `TestQueue`, nevű névtérrel `HowToSample`:

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

Módszer áll rendelkezésre a `QueueInfo` , amelyek lehetővé teszik a hello várólista toobe beállított tulajdonságait (például: tooset hello alapértelmezett--élettartama (TTL) értéke alkalmazott toobe toomessages küldött toohello várólista). hello következő példa bemutatja, hogyan toocreate várólista nevű `TestQueue` 5 GB maximális mérettel:

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Vegye figyelembe, hogy használhatja-e hello `listQueues` metódusa **ServiceBusContract** toocheck objektumokat, ha a várólista megadott névvel már létezik egy szolgáltatásnévtérben.

## <a name="send-messages-tooa-queue"></a>Üzenetek tooa várólista küldése
toosend üzenet tooa Service Bus-üzenetsorba, az alkalmazás beolvassa a **ServiceBusContract** objektum. a következő kód bemutatja hogyan hello toosend hello üzenetet `TestQueue` hello a korábban létrehozott várólista `HowToSample` névtér:

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

Küldött üzeneteket, és kapott a Service Bus üzenetsorok hello példánya [BrokeredMessage] [ BrokeredMessage] osztály. [BrokeredMessage] [ BrokeredMessage] objektumok rendelkeznek egy szabványos tulajdonságkészlettel (például [címke](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) és [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), amely egyéni használt toohold dictionary az alkalmazás-specifikus tulajdonságait, és egy tetszőleges alkalmazásadatokból álló törzzsel. Az alkalmazás beállíthatja hello üzenet törzsét hello bármilyen szerializálható objektumnak hello hello konstruktorának való átadásával [BrokeredMessage][BrokeredMessage], és a megfelelő szerializáló hello használja tooserialize hello objektum. Másik lehetőségként megadhat egy **java. IO. InputStream** objektum.

hello következő példa bemutatja, hogyan toosend öt teszt üzenetek toothe `TestQueue` **MessageSender** azt beolvasott hello előző kódrészletet:

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

Service Bus-üzenetsorok támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md). hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet. Az üzenetsorban tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello üzenetsor által tárolt hello üzenetek teljes mérete. Az üzenetsor ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.

## <a name="receive-messages-from-a-queue"></a>Üzenetek fogadása egy üzenetsorból
hello elsődleges módon tooreceive üzenetek várólistából való várólistából toouse egy **ServiceBusContract** objektum. A fogadott üzenetek a két különböző módban tudnak működni: **ReceiveAndDelete** és **PeekLock**.

Hello használatakor **ReceiveAndDelete** mód, kap egy egylépéses művelet – Ez azt jelenti, hogy amikor a Service Bus egy olvasási kérést kap a sorhoz, azt hello üzenetet, feldolgozottként jelöli meg és toohello alkalmazás visszaadja. **ReceiveAndDelete** módban (amely hello alapértelmezett mód) hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban. toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt.
Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.

A **PeekLock** mód, kap egy két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz. A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása. Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával **törlése** hello érkezett üzenet. Amikor a Service Bus látja a hello **törlése** hívás, azt feldolgozottként hello üzenetet, és távolítsa el azt a hello várólista.

hello következő példa bemutatja, hogyan lehet üzeneteket fogadni, és a feldolgozott használatával **PeekLock** mód nem hello alapértelmezett. hello az alábbi példa nem végtelen hurkot és üzeneteket dolgozza fel a megérkeznek a `TestQueue`:

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hogyan toohandle omlik össze és nem olvasható üzenetek
Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít. Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello **unlockMessage** metódust a fogadott üdvözlőüzenetére (helyett hello **deleteMessage** metódus). A Service Bus toounlock hello üzenet-várólista hello belül, és tegye elérhetővé toobe ismételt fogadását oka vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.

Még nincs hozzárendelve az üzenetsorban lévő időtúllépés, és ha hello alkalmazás sikertelen tooprocess hello üzenet járjon le a zárolás időtúllépését (például ha hello alkalmazás összeomlik), majd a Service Bus automatikusan feloldja az üdvözlőüzenetére és rendelkezésre álló toobe újbóli fogadását teszi.

A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello **deleteMessage** kérés kiadása, majd hello üzenet újbóli kézbesítése toohello alkalmazás amikor újraindul. Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*; Ez azt jelenti, hogy minden üzenet legalább egyszer fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet. Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését. Ez gyakran érhető el hello segítségével **getMessageId** metódus hello üzenet, amely állandó marad a kézbesítési kísérletek során.

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Service Bus-üzenetsorok alapjait hello, lásd: [üzenetsorok, témakörök és előfizetések] [ Queues, topics, and subscriptions] további információt.

További információkért lásd: hello [Java fejlesztői központ](https://azure.microsoft.com/develop/java/).

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
