---
title: "aaaHow toouse Azure Service Bus-üzenettémakörök Java |} Microsoft Docs"
description: "Használja a Service Bus-üzenettémák és előfizetések az Azure-ban."
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 63d6c8bd-8a22-4292-befc-545ffb52e8eb
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 06/28/2017
ms.author: sethm
ms.openlocfilehash: 1aad16fdb5d68a5782b85c8dfda9d695babd57ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-java"></a>Hogyan toouse Service Bus üzenettémák és előfizetések Java

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ez az útmutató ismerteti, hogyan toouse Service Bus üzenettémák és előfizetések. hello minták Java nyelven íródtak, és használja a hello [Javához készült Azure SDK][Azure SDK for Java]. hello tárgyalt forgatókönyvekben szerepel a **üzenettémák és előfizetések létrehozása**, **előfizetés-szűrők létrehozása**, **tooa témakör üzenetek küldésekor**, **fogadása előfizetés üzeneteit**, és **üzenettémák és előfizetések törlése**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Mik azok a Service Bus-üzenettémák és -előfizetések?
A Service Bus-üzenettémák és -előfizetések *közzétételi/előfizetési* modellt biztosítanak az üzenettovábbításhoz. Üzenettémák és előfizetések használata esetén az elosztott alkalmazások összetevői nem közvetlenül egymással kommunikálnak, hanem egy közvetítőként szolgáló üzenettémakörön keresztül.

![Az üzenettémakörök alapfogalmai](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

A Service Bus-üzenetsorokkal ellentétben, amely esetében minden üzenetet egy-egy fogyasztó dolgoz fel, az üzenettémák és előfizetések a közzététel/előfizetés minta használatával „egy a többhöz” típusú kommunikációt biztosítanak. Akkor lehet több előfizetések tooa témakör regisztrálni. Egy üzenet elküldésekor tooa témakör, majd teszi elérhetővé tooeach előfizetés toohandle/folyamat függetlenül.

Egy előfizetés tooa témakör hasonlít egy virtuális üzenetsorra, amely hello távozó üzeneteinek toohello témakör példányait megkapja. Akkor is szűrőszabályok előfizetésenként történő beállítására, amely lehetővé teszi toofilter/korlátozása mely üzenetek tooa témakör fogadásának melyik előfizetések kaphatják meg témakör.

Service Bus-üzenettémák és előfizetések lehetővé teszik tooscale tooprocess üzenetek nagyon nagy számú nagyon nagy számú felhasználók és az alkalmazások között.

## <a name="create-a-service-namespace"></a>Szolgáltatásnévtér létrehozása
az Azure Service Bus-üzenettémák és előfizetések használata toobegin először létre kell hoznia egy névtér egy hatókörkezelési tárolót biztosít az alkalmazáson belül a Service Bus erőforrásainak kezeléséhez.

egy névtér toocreate:

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-toouse-service-bus"></a>Az alkalmazás toouse Service Bus konfigurálása
Győződjön meg arról, hogy telepítette a hello [Javához készült Azure SDK] [ Azure SDK for Java] Ez a minta létrehozása előtt. Eclipse használatakor telepítése hello [Eclipse Azure eszköztára] [ Azure Toolkit for Eclipse] hello Azure SDK for Java foglal magában. Ezután hozzáadhat hello **Microsoft Azure-könyvtárakban Java** tooyour projekt:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Adja hozzá a következő hello `import` hello Java fájl elejéhez utasítások toohello:

```java
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Adja hozzá a Java tooyour hello Azure-könyvtárakban elérési út létrehozása, és adja hozzá a projekt központi telepítési szerelvényben.

## <a name="create-a-topic"></a>Üzenettémakör létrehozása
Felügyeleti műveletek a Service Bus-témakörök használatával is kell végezni a **ServiceBusContract** osztály. A **ServiceBusContract** objektum a megfelelő konfiguráció, amely magában foglalja az engedélyek toomanage SAS-jogkivonat és hello összeállított **ServiceBusContract** osztály az egyetlen pont, hello a kommunikáció az Azure-ral.

Hello **ServiceBusService** osztály módszerek toocreate biztosít, enumerálásához és törléséhez témaköröket. a következő példa azt mutatja meg hogyan hello egy **ServiceBusService** objektum lehet egy témakör nevű használt toocreate `TestTopic`, nevű névtérrel `HowToSample`:

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
      "HowToSample",
      "RootManageSharedAccessKey",
      "SAS_key_value",
      ".servicebus.windows.net"
      );

ServiceBusContract service = ServiceBusService.create(config);
TopicInfo topicInfo = new TopicInfo("TestTopic");
try  
{
    CreateTopicResult result = service.createTopic(topicInfo);
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Módszer áll rendelkezésre a **TopicInfo** , amelyek lehetővé kell beállítani a hello témakör tulajdonságainak (például: tooset hello alapértelmezett--élettartama (TTL) értéke alkalmazott toobe toomessages küldött toohello témakör). hello következő példa bemutatja, hogyan toocreate témakör nevű `TestTopic` 5 GB maximális mérettel:

```java
long maxSizeInMegabytes = 5120;  
TopicInfo topicInfo = new TopicInfo("TestTopic");  
topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateTopicResult result = service.createTopic(topicInfo);
```

Vegye figyelembe, hogy használhatja-e hello **listTopics** metódusa **ServiceBusContract** toocheck objektumokat, ha a témakör a megadott névvel már létezik egy szolgáltatásnévtérben.

## <a name="create-subscriptions"></a>Előfizetések létrehozása
Előfizetések tootopics is jönnek létre hello **ServiceBusService** osztály. Előfizetések vannak nevezve, és rendelkezhetnek olyan szűrőkkel, amelyek toohello előfizetés virtuális üzenetsorának átadott üzenetek készletét hello korlátozza.

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Előfizetés létrehozása hello alapértelmezett (MatchAll) szűrővel
Hello **MatchAll** szűrő hello alapértelmezett szűrő, amely akkor használatos, ha nincs meghatározva szűrő egy új előfizetés létrehozásakor. Ha hello **MatchAll** szűrővel, minden üzenetek közzétett toohello témakör kerülnek, az előfizetés virtuális üzenetsorában. hello alábbi példa létrehoz egy "AllMessages" nevű előfizetést, és használja az alapértelmezett hello **MatchAll** szűrő.

```java
SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
CreateSubscriptionResult result =
    service.createSubscription("TestTopic", subInfo);
```

### <a name="create-subscriptions-with-filters"></a>Előfizetések létrehozása szűrőkkel
Szűrőket, amelyek lehetővé teszik a üzenetet küldött tooa témakör belül egy adott üzenettémakör-előfizetésben kell megjelenítése tooscope is létrehozhat.

hello szűrő előfizetések által támogatott legrugalmasabb típusú van a [SqlFilter][SqlFilter], amely az SQL92 egy részhalmazát valósítja meg. Az SQL-szűrők hello tulajdonságait, amelyek a közzétett toohello témakör köszönőüzenetei működik. Az SQL-szűrőkkel használható hello kifejezések kapcsolatos további tudnivalókért tekintse meg a hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] szintaxist.

hello alábbi példa egy nevű előfizetést hoz létre `HighMessages` rendelkező egy [SqlFilter] [ SqlFilter] objektum, amely csak kiválasztja az üzeneteket, amelyek egyéni **MessageNumber** a tulajdonság 3-nál nagyobb:

```java
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete hello default rule, otherwise hello new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Ehhez hasonlóan hello alábbi példa egy nevű előfizetést hoz létre `LowMessages` rendelkező egy [SqlFilter] [ SqlFilter] objektum, amely csak kiválasztja az üzeneteket, amelyek rendelkeznek egy **MessageNumber** tulajdonságának értéke kisebb vagy egyenlő, mint too3:

```java
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete hello default rule, otherwise hello new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Ha egy most üzenettel túl`TestTopic`, azt mindig kézbesíti a rendszer az előfizetett tooreceivers toohello `AllMessages` előfizetés, és szelektív módon kézbesíti az előfizetett tooreceivers toohello `HighMessages` és `LowMessages` előfizetések () attól függően, az üzenet tartalma).

## <a name="send-messages-tooa-topic"></a>Üzenetek tooa témakör küldése
toosend üzenet tooa Service Bus-témakörbe, az alkalmazás beolvassa a **ServiceBusContract** objektum. hello következő kód bemutatja, hogyan toosend hello üzenetet `TestTopic` belül hello korábban létrehozott témakör `HowToSample` névtér:

```java
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

TooService Bus-üzenettémakörök küldött üzenetek példánya a [BrokeredMessage] [ BrokeredMessage] osztály. [BrokeredMessage][BrokeredMessage]* objektumok rendelkeznek egy szabványos módszerek (mint például **setLabel** és **TimeToLive**), amely egyéni használt toohold dictionary az alkalmazás-specifikus tulajdonságait, és egy tetszőleges alkalmazásadatokból álló törzzsel. Az alkalmazás beállíthatja az üzenet törzsét hello bármilyen szerializálható objektumnak hello konstruktorának való átadásával a [BrokeredMessage][BrokeredMessage], és megfelelő hello **DataContractSerializer**  használt tooserialize hello objektum lesz. Másik megoldásként egy **java.io.InputStream** tartalmazhat.

hello következő példa bemutatja, hogyan toosend öt teszt üzenetek toothe `TestTopic` **MessageSender** jelenleg a fenti hello kódrészletet kapott.
Megjegyzés: hogyan hello **MessageNumber** egyes üzenetek tulajdonság értéke a hello iterációs hello hurok változik (Ez határozza meg, hogy melyik előfizetések fogják megkapni):

```java
for (int i=0; i<5; i++)  {
// Create message, passing a string message for hello body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message toohello topic
service.sendTopicMessage("TestTopic", message);
}
```

Service Bus-üzenettémakörök támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md). hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet. A témakörökben tárolt üzenetek hello száma nincs korlátozva, de a hello teljes mérete, a témakörök által tárolt köszönőüzenetei korlátozva van. A témakör ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.

## <a name="how-tooreceive-messages-from-a-subscription"></a>Hogyan tooreceive üzenetek előfizetésből
tooreceive üzenetek előfizetésből, használja a **ServiceBusContract** objektum. A fogadott üzenetek a két különböző módban tudnak működni: **ReceiveAndDelete** és **PeekLock**.

Hello használatakor **ReceiveAndDelete** mód, kap egy egylépéses művelet – Ez azt jelenti, hogy a Service Bus egy olvasási kérést üzenetet kap, ha azt hello üzenetet, feldolgozottként jelöli meg és toothe alkalmazás visszaadja. **ReceiveAndDelete** mód hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban. toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt. A Service Bus csak fel az üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.

A **PeekLock** mód, kap egy két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz. A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása. Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával **törlése** hello érkezett üzenet. Amikor a Service Bus látja a hello **törlése** hívás, azt feldolgozottként hello üzenetet, és távolítsa el a témakör hello.

hello az alábbi példa bemutatja, hogyan lehet üzeneteket fogadni, és a feldolgozott használatával **PeekLock** mód nem hello alapértelmezett. hello az alábbi példa hurkot hajt végre, és feldolgozza hello "HighMessages" előfizetés üzeneteit és majd kilép, amikor nincsenek további üzenetek (azt is megteheti, hogy sikerült beállítani új üzenetek toowait).

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display hello topic message.
            System.out.print("From topic: ");
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
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
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
Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít. Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello **unlockMessage** metódust a fogadott üdvözlőüzenetére (helyett hello **deleteMessage** metódus). Ezzel a Service Bus toounlock hello üzenet hello témakör és révén elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.

Is van a következő témakörben lévő társított időtúllépés, és ha hello alkalmazás nem tooprocess üdvözlőüzenetére előtt a zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus automatikusan feloldást köszönőüzenetei és a rendelkezésre álló toobe újbóli fogadását.

A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello **deleteMessage** kérés kiadása, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazást, amikor újraindul. Ezt gyakran nevezik **legalább egyszeri feldolgozásnak**, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet. Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését. Ez gyakran érhető el hello segítségével **getMessageId** üdvözlőüzenetére, amely állandó marad a kézbesítési kísérletek metódusában.

## <a name="delete-topics-and-subscriptions"></a>Témakörök és előfizetések törlése
hello elsődleges módon toodelete témakörök és előfizetések toouse egy **ServiceBusContract** objektum. Egy témakör törlése hello témakörben regisztrált előfizetésekkel is törli. Az előfizetések független módon is törölhetők.

```java
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Service Bus-üzenetsorok alapjait hello, lásd: [Service Bus-üzenetsorok, témakörök és előfizetések] [ Service Bus queues, topics, and subscriptions] további információt.

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter 
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#Microsoft_ServiceBus_Messaging_SqlFilter_SqlExpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

[0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
[2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
[3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
