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
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-java"></a><span data-ttu-id="ef719-103">Hogyan toouse Service Bus üzenettémák és előfizetések Java</span><span class="sxs-lookup"><span data-stu-id="ef719-103">How toouse Service Bus topics and subscriptions with Java</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="ef719-104">Ez az útmutató ismerteti, hogyan toouse Service Bus üzenettémák és előfizetések.</span><span class="sxs-lookup"><span data-stu-id="ef719-104">This guide describes how toouse Service Bus topics and subscriptions.</span></span> <span data-ttu-id="ef719-105">hello minták Java nyelven íródtak, és használja a hello [Javához készült Azure SDK][Azure SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="ef719-105">hello samples are written in Java and use hello [Azure SDK for Java][Azure SDK for Java].</span></span> <span data-ttu-id="ef719-106">hello tárgyalt forgatókönyvekben szerepel a **üzenettémák és előfizetések létrehozása**, **előfizetés-szűrők létrehozása**, **tooa témakör üzenetek küldésekor**, **fogadása előfizetés üzeneteit**, és **üzenettémák és előfizetések törlése**.</span><span class="sxs-lookup"><span data-stu-id="ef719-106">hello scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages tooa topic**, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span>

## <a name="what-are-service-bus-topics-and-subscriptions"></a><span data-ttu-id="ef719-107">Mik azok a Service Bus-üzenettémák és -előfizetések?</span><span class="sxs-lookup"><span data-stu-id="ef719-107">What are Service Bus topics and subscriptions?</span></span>
<span data-ttu-id="ef719-108">A Service Bus-üzenettémák és -előfizetések *közzétételi/előfizetési* modellt biztosítanak az üzenettovábbításhoz.</span><span class="sxs-lookup"><span data-stu-id="ef719-108">Service Bus topics and subscriptions support a *publish/subscribe* messaging communication model.</span></span> <span data-ttu-id="ef719-109">Üzenettémák és előfizetések használata esetén az elosztott alkalmazások összetevői nem közvetlenül egymással kommunikálnak, hanem egy közvetítőként szolgáló üzenettémakörön keresztül.</span><span class="sxs-lookup"><span data-stu-id="ef719-109">When using topics and subscriptions, components of a distributed application do not communicate directly with each other; instead they exchange messages via a topic, which acts as an intermediary.</span></span>

![Az üzenettémakörök alapfogalmai](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

<span data-ttu-id="ef719-111">A Service Bus-üzenetsorokkal ellentétben, amely esetében minden üzenetet egy-egy fogyasztó dolgoz fel, az üzenettémák és előfizetések a közzététel/előfizetés minta használatával „egy a többhöz” típusú kommunikációt biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="ef719-111">In contrast with Service Bus queues, in which each message is processed by a single consumer, topics and subscriptions provide a "one-to-many" form of communication, using a publish/subscribe pattern.</span></span> <span data-ttu-id="ef719-112">Akkor lehet több előfizetések tooa témakör regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="ef719-112">It is possible to register multiple subscriptions tooa topic.</span></span> <span data-ttu-id="ef719-113">Egy üzenet elküldésekor tooa témakör, majd teszi elérhetővé tooeach előfizetés toohandle/folyamat függetlenül.</span><span class="sxs-lookup"><span data-stu-id="ef719-113">When a message is sent tooa topic, it is then made available tooeach subscription toohandle/process independently.</span></span>

<span data-ttu-id="ef719-114">Egy előfizetés tooa témakör hasonlít egy virtuális üzenetsorra, amely hello távozó üzeneteinek toohello témakör példányait megkapja.</span><span class="sxs-lookup"><span data-stu-id="ef719-114">A subscription tooa topic resembles a virtual queue that receives copies of hello messages that were sent toohello topic.</span></span> <span data-ttu-id="ef719-115">Akkor is szűrőszabályok előfizetésenként történő beállítására, amely lehetővé teszi toofilter/korlátozása mely üzenetek tooa témakör fogadásának melyik előfizetések kaphatják meg témakör.</span><span class="sxs-lookup"><span data-stu-id="ef719-115">You can optionally register filter rules for a topic on a per-subscription basis, which allows you toofilter/restrict which messages tooa topic are received by which topic subscriptions.</span></span>

<span data-ttu-id="ef719-116">Service Bus-üzenettémák és előfizetések lehetővé teszik tooscale tooprocess üzenetek nagyon nagy számú nagyon nagy számú felhasználók és az alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="ef719-116">Service Bus topics and subscriptions enable you tooscale tooprocess a very large number of messages across a very large number of users and applications.</span></span>

## <a name="create-a-service-namespace"></a><span data-ttu-id="ef719-117">Szolgáltatásnévtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef719-117">Create a service namespace</span></span>
<span data-ttu-id="ef719-118">az Azure Service Bus-üzenettémák és előfizetések használata toobegin először létre kell hoznia egy névtér egy hatókörkezelési tárolót biztosít az alkalmazáson belül a Service Bus erőforrásainak kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="ef719-118">toobegin using Service Bus topics and subscriptions in Azure, you must first create a namespace, which provides a scoping container for addressing Service Bus resources within your application.</span></span>

<span data-ttu-id="ef719-119">egy névtér toocreate:</span><span class="sxs-lookup"><span data-stu-id="ef719-119">toocreate a namespace:</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="ef719-120">Az alkalmazás toouse Service Bus konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ef719-120">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="ef719-121">Győződjön meg arról, hogy telepítette a hello [Javához készült Azure SDK] [ Azure SDK for Java] Ez a minta létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="ef719-121">Make sure you have installed hello [Azure SDK for Java][Azure SDK for Java] before building this sample.</span></span> <span data-ttu-id="ef719-122">Eclipse használatakor telepítése hello [Eclipse Azure eszköztára] [ Azure Toolkit for Eclipse] hello Azure SDK for Java foglal magában.</span><span class="sxs-lookup"><span data-stu-id="ef719-122">If you are using Eclipse, you can install hello [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse] that includes hello Azure SDK for Java.</span></span> <span data-ttu-id="ef719-123">Ezután hozzáadhat hello **Microsoft Azure-könyvtárakban Java** tooyour projekt:</span><span class="sxs-lookup"><span data-stu-id="ef719-123">You can then add hello **Microsoft Azure Libraries for Java** tooyour project:</span></span>

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

<span data-ttu-id="ef719-124">Adja hozzá a következő hello `import` hello Java fájl elejéhez utasítások toohello:</span><span class="sxs-lookup"><span data-stu-id="ef719-124">Add hello following `import` statements toohello top of hello Java file:</span></span>

```java
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

<span data-ttu-id="ef719-125">Adja hozzá a Java tooyour hello Azure-könyvtárakban elérési út létrehozása, és adja hozzá a projekt központi telepítési szerelvényben.</span><span class="sxs-lookup"><span data-stu-id="ef719-125">Add hello Azure Libraries for Java tooyour build path and include it in your project deployment assembly.</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="ef719-126">Üzenettémakör létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef719-126">Create a topic</span></span>
<span data-ttu-id="ef719-127">Felügyeleti műveletek a Service Bus-témakörök használatával is kell végezni a **ServiceBusContract** osztály.</span><span class="sxs-lookup"><span data-stu-id="ef719-127">Management operations for Service Bus topics can be performed via the **ServiceBusContract** class.</span></span> <span data-ttu-id="ef719-128">A **ServiceBusContract** objektum a megfelelő konfiguráció, amely magában foglalja az engedélyek toomanage SAS-jogkivonat és hello összeállított **ServiceBusContract** osztály az egyetlen pont, hello a kommunikáció az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="ef719-128">A **ServiceBusContract** object is constructed with an appropriate configuration that encapsulates the SAS token with permissions toomanage it, and hello **ServiceBusContract** class is hello sole point of communication with Azure.</span></span>

<span data-ttu-id="ef719-129">Hello **ServiceBusService** osztály módszerek toocreate biztosít, enumerálásához és törléséhez témaköröket.</span><span class="sxs-lookup"><span data-stu-id="ef719-129">hello **ServiceBusService** class provides methods toocreate, enumerate, and delete topics.</span></span> <span data-ttu-id="ef719-130">a következő példa azt mutatja meg hogyan hello egy **ServiceBusService** objektum lehet egy témakör nevű használt toocreate `TestTopic`, nevű névtérrel `HowToSample`:</span><span class="sxs-lookup"><span data-stu-id="ef719-130">hello following example shows how a **ServiceBusService** object can be used toocreate a topic named `TestTopic`, with a namespace called `HowToSample`:</span></span>

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

<span data-ttu-id="ef719-131">Módszer áll rendelkezésre a **TopicInfo** , amelyek lehetővé kell beállítani a hello témakör tulajdonságainak (például: tooset hello alapértelmezett--élettartama (TTL) értéke alkalmazott toobe toomessages küldött toohello témakör).</span><span class="sxs-lookup"><span data-stu-id="ef719-131">There are methods on **TopicInfo** that enable properties of hello topic to be set (for example: tooset hello default time-to-live (TTL) value toobe applied toomessages sent toohello topic).</span></span> <span data-ttu-id="ef719-132">hello következő példa bemutatja, hogyan toocreate témakör nevű `TestTopic` 5 GB maximális mérettel:</span><span class="sxs-lookup"><span data-stu-id="ef719-132">hello following example shows how toocreate a topic named `TestTopic` with a maximum size of 5 GB:</span></span>

```java
long maxSizeInMegabytes = 5120;  
TopicInfo topicInfo = new TopicInfo("TestTopic");  
topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateTopicResult result = service.createTopic(topicInfo);
```

<span data-ttu-id="ef719-133">Vegye figyelembe, hogy használhatja-e hello **listTopics** metódusa **ServiceBusContract** toocheck objektumokat, ha a témakör a megadott névvel már létezik egy szolgáltatásnévtérben.</span><span class="sxs-lookup"><span data-stu-id="ef719-133">Note that you can use hello **listTopics** method on **ServiceBusContract** objects toocheck if a topic with a specified name already exists within a service namespace.</span></span>

## <a name="create-subscriptions"></a><span data-ttu-id="ef719-134">Előfizetések létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef719-134">Create subscriptions</span></span>
<span data-ttu-id="ef719-135">Előfizetések tootopics is jönnek létre hello **ServiceBusService** osztály.</span><span class="sxs-lookup"><span data-stu-id="ef719-135">Subscriptions tootopics are also created with hello **ServiceBusService** class.</span></span> <span data-ttu-id="ef719-136">Előfizetések vannak nevezve, és rendelkezhetnek olyan szűrőkkel, amelyek toohello előfizetés virtuális üzenetsorának átadott üzenetek készletét hello korlátozza.</span><span class="sxs-lookup"><span data-stu-id="ef719-136">Subscriptions are named and can have an optional filter that restricts hello set of messages passed toohello subscription's virtual queue.</span></span>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="ef719-137">Előfizetés létrehozása hello alapértelmezett (MatchAll) szűrővel</span><span class="sxs-lookup"><span data-stu-id="ef719-137">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="ef719-138">Hello **MatchAll** szűrő hello alapértelmezett szűrő, amely akkor használatos, ha nincs meghatározva szűrő egy új előfizetés létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="ef719-138">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="ef719-139">Ha hello **MatchAll** szűrővel, minden üzenetek közzétett toohello témakör kerülnek, az előfizetés virtuális üzenetsorában.</span><span class="sxs-lookup"><span data-stu-id="ef719-139">When hello **MatchAll** filter is used, all messages published toohello topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="ef719-140">hello alábbi példa létrehoz egy "AllMessages" nevű előfizetést, és használja az alapértelmezett hello **MatchAll** szűrő.</span><span class="sxs-lookup"><span data-stu-id="ef719-140">hello following example creates a subscription named "AllMessages" and uses hello default **MatchAll** filter.</span></span>

```java
SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
CreateSubscriptionResult result =
    service.createSubscription("TestTopic", subInfo);
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="ef719-141">Előfizetések létrehozása szűrőkkel</span><span class="sxs-lookup"><span data-stu-id="ef719-141">Create subscriptions with filters</span></span>
<span data-ttu-id="ef719-142">Szűrőket, amelyek lehetővé teszik a üzenetet küldött tooa témakör belül egy adott üzenettémakör-előfizetésben kell megjelenítése tooscope is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="ef719-142">You can also create filters that enable you tooscope which messages sent tooa topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="ef719-143">hello szűrő előfizetések által támogatott legrugalmasabb típusú van a [SqlFilter][SqlFilter], amely az SQL92 egy részhalmazát valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="ef719-143">hello most flexible type of filter supported by subscriptions is the [SqlFilter][SqlFilter], which implements a subset of SQL92.</span></span> <span data-ttu-id="ef719-144">Az SQL-szűrők hello tulajdonságait, amelyek a közzétett toohello témakör köszönőüzenetei működik.</span><span class="sxs-lookup"><span data-stu-id="ef719-144">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="ef719-145">Az SQL-szűrőkkel használható hello kifejezések kapcsolatos további tudnivalókért tekintse meg a hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] szintaxist.</span><span class="sxs-lookup"><span data-stu-id="ef719-145">For more details about hello expressions that can be used with a SQL filter, review hello [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="ef719-146">hello alábbi példa egy nevű előfizetést hoz létre `HighMessages` rendelkező egy [SqlFilter] [ SqlFilter] objektum, amely csak kiválasztja az üzeneteket, amelyek egyéni **MessageNumber** a tulajdonság 3-nál nagyobb:</span><span class="sxs-lookup"><span data-stu-id="ef719-146">hello following example creates a subscription named `HighMessages` with a [SqlFilter][SqlFilter] object that only selects messages that have a custom **MessageNumber** property greater than 3:</span></span>

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

<span data-ttu-id="ef719-147">Ehhez hasonlóan hello alábbi példa egy nevű előfizetést hoz létre `LowMessages` rendelkező egy [SqlFilter] [ SqlFilter] objektum, amely csak kiválasztja az üzeneteket, amelyek rendelkeznek egy **MessageNumber** tulajdonságának értéke kisebb vagy egyenlő, mint too3:</span><span class="sxs-lookup"><span data-stu-id="ef719-147">Similarly, hello following example creates a subscription named `LowMessages` with a [SqlFilter][SqlFilter] object that only selects messages that have a **MessageNumber** property less than or equal too3:</span></span>

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

<span data-ttu-id="ef719-148">Ha egy most üzenettel túl`TestTopic`, azt mindig kézbesíti a rendszer az előfizetett tooreceivers toohello `AllMessages` előfizetés, és szelektív módon kézbesíti az előfizetett tooreceivers toohello `HighMessages` és `LowMessages` előfizetések () attól függően, az üzenet tartalma).</span><span class="sxs-lookup"><span data-stu-id="ef719-148">When a message is now sent too`TestTopic`, it will always be delivered tooreceivers subscribed toohello `AllMessages` subscription, and selectively delivered tooreceivers subscribed toohello `HighMessages` and `LowMessages` subscriptions (depending upon the message content).</span></span>

## <a name="send-messages-tooa-topic"></a><span data-ttu-id="ef719-149">Üzenetek tooa témakör küldése</span><span class="sxs-lookup"><span data-stu-id="ef719-149">Send messages tooa topic</span></span>
<span data-ttu-id="ef719-150">toosend üzenet tooa Service Bus-témakörbe, az alkalmazás beolvassa a **ServiceBusContract** objektum.</span><span class="sxs-lookup"><span data-stu-id="ef719-150">toosend a message tooa Service Bus topic, your application obtains a **ServiceBusContract** object.</span></span> <span data-ttu-id="ef719-151">hello következő kód bemutatja, hogyan toosend hello üzenetet `TestTopic` belül hello korábban létrehozott témakör `HowToSample` névtér:</span><span class="sxs-lookup"><span data-stu-id="ef719-151">hello following code demonstrates how toosend a message for hello `TestTopic` topic created previously within hello `HowToSample` namespace:</span></span>

```java
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

<span data-ttu-id="ef719-152">TooService Bus-üzenettémakörök küldött üzenetek példánya a [BrokeredMessage] [ BrokeredMessage] osztály.</span><span class="sxs-lookup"><span data-stu-id="ef719-152">Messages sent tooService Bus Topics are instances of the [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="ef719-153">[BrokeredMessage][BrokeredMessage]* objektumok rendelkeznek egy szabványos módszerek (mint például **setLabel** és **TimeToLive**), amely egyéni használt toohold dictionary az alkalmazás-specifikus tulajdonságait, és egy tetszőleges alkalmazásadatokból álló törzzsel.</span><span class="sxs-lookup"><span data-stu-id="ef719-153">[BrokeredMessage][BrokeredMessage]* objects have a set of standard methods (such as **setLabel** and **TimeToLive**), a dictionary that is used toohold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="ef719-154">Az alkalmazás beállíthatja az üzenet törzsét hello bármilyen szerializálható objektumnak hello konstruktorának való átadásával a [BrokeredMessage][BrokeredMessage], és megfelelő hello **DataContractSerializer**  használt tooserialize hello objektum lesz.</span><span class="sxs-lookup"><span data-stu-id="ef719-154">An application can set hello body of the message by passing any serializable object into hello constructor of the [BrokeredMessage][BrokeredMessage], and hello appropriate **DataContractSerializer** will then be used tooserialize hello object.</span></span> <span data-ttu-id="ef719-155">Másik megoldásként egy **java.io.InputStream** tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="ef719-155">Alternatively, a **java.io.InputStream** can be provided.</span></span>

<span data-ttu-id="ef719-156">hello következő példa bemutatja, hogyan toosend öt teszt üzenetek toothe `TestTopic` **MessageSender** jelenleg a fenti hello kódrészletet kapott.</span><span class="sxs-lookup"><span data-stu-id="ef719-156">hello following example demonstrates how toosend five test messages toothe `TestTopic` **MessageSender** we obtained in hello code snippet above.</span></span>
<span data-ttu-id="ef719-157">Megjegyzés: hogyan hello **MessageNumber** egyes üzenetek tulajdonság értéke a hello iterációs hello hurok változik (Ez határozza meg, hogy melyik előfizetések fogják megkapni):</span><span class="sxs-lookup"><span data-stu-id="ef719-157">Note how hello **MessageNumber** property value of each message varies on hello iteration of hello loop (this will determine which subscriptions receive it):</span></span>

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

<span data-ttu-id="ef719-158">Service Bus-üzenettémakörök támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="ef719-158">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="ef719-159">hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="ef719-159">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="ef719-160">A témakörökben tárolt üzenetek hello száma nincs korlátozva, de a hello teljes mérete, a témakörök által tárolt köszönőüzenetei korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="ef719-160">There is no limit on hello number of messages held in a topic but there is a limit on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="ef719-161">A témakör ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="ef719-161">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="how-tooreceive-messages-from-a-subscription"></a><span data-ttu-id="ef719-162">Hogyan tooreceive üzenetek előfizetésből</span><span class="sxs-lookup"><span data-stu-id="ef719-162">How tooreceive messages from a subscription</span></span>
<span data-ttu-id="ef719-163">tooreceive üzenetek előfizetésből, használja a **ServiceBusContract** objektum.</span><span class="sxs-lookup"><span data-stu-id="ef719-163">tooreceive messages from a subscription, use a **ServiceBusContract** object.</span></span> <span data-ttu-id="ef719-164">A fogadott üzenetek a két különböző módban tudnak működni: **ReceiveAndDelete** és **PeekLock**.</span><span class="sxs-lookup"><span data-stu-id="ef719-164">Received messages can work in two different modes: **ReceiveAndDelete** and **PeekLock**.</span></span>

<span data-ttu-id="ef719-165">Hello használatakor **ReceiveAndDelete** mód, kap egy egylépéses művelet – Ez azt jelenti, hogy a Service Bus egy olvasási kérést üzenetet kap, ha azt hello üzenetet, feldolgozottként jelöli meg és toothe alkalmazás visszaadja.</span><span class="sxs-lookup"><span data-stu-id="ef719-165">When using hello **ReceiveAndDelete** mode, receive is a single-shot operation - that is, when Service Bus receives a read request for a message, it marks hello message as being consumed and returns it toothe application.</span></span> <span data-ttu-id="ef719-166">**ReceiveAndDelete** mód hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban.</span><span class="sxs-lookup"><span data-stu-id="ef719-166">**ReceiveAndDelete** mode is hello simplest model and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="ef719-167">toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="ef719-167">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="ef719-168">A Service Bus csak fel az üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.</span><span class="sxs-lookup"><span data-stu-id="ef719-168">Because Service Bus will have marked the message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="ef719-169">A **PeekLock** mód, kap egy két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz.</span><span class="sxs-lookup"><span data-stu-id="ef719-169">In **PeekLock** mode, receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="ef719-170">A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása.</span><span class="sxs-lookup"><span data-stu-id="ef719-170">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="ef719-171">Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával **törlése** hello érkezett üzenet.</span><span class="sxs-lookup"><span data-stu-id="ef719-171">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling **Delete** on hello received message.</span></span> <span data-ttu-id="ef719-172">Amikor a Service Bus látja a hello **törlése** hívás, azt feldolgozottként hello üzenetet, és távolítsa el a témakör hello.</span><span class="sxs-lookup"><span data-stu-id="ef719-172">When Service Bus sees hello **Delete** call, it will mark hello message as being consumed and remove it from hello topic.</span></span>

<span data-ttu-id="ef719-173">hello az alábbi példa bemutatja, hogyan lehet üzeneteket fogadni, és a feldolgozott használatával **PeekLock** mód nem hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="ef719-173">hello example below demonstrates how messages can be received and processed using **PeekLock** mode (not hello default mode).</span></span> <span data-ttu-id="ef719-174">hello az alábbi példa hurkot hajt végre, és feldolgozza hello "HighMessages" előfizetés üzeneteit és majd kilép, amikor nincsenek további üzenetek (azt is megteheti, hogy sikerült beállítani új üzenetek toowait).</span><span class="sxs-lookup"><span data-stu-id="ef719-174">hello example below performs a loop and processes messages in hello "HighMessages" subscription and then exits when there are no more messages (alternatively, it could be set toowait for new messages).</span></span>

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="ef719-175">Hogyan toohandle omlik össze és nem olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="ef719-175">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="ef719-176">Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít.</span><span class="sxs-lookup"><span data-stu-id="ef719-176">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="ef719-177">Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello **unlockMessage** metódust a fogadott üdvözlőüzenetére (helyett hello **deleteMessage** metódus).</span><span class="sxs-lookup"><span data-stu-id="ef719-177">If a receiver application is unable tooprocess hello message for some reason, then it can call hello **unlockMessage** method on hello received message (instead of hello **deleteMessage** method).</span></span> <span data-ttu-id="ef719-178">Ezzel a Service Bus toounlock hello üzenet hello témakör és révén elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.</span><span class="sxs-lookup"><span data-stu-id="ef719-178">This will cause Service Bus toounlock hello message within hello topic and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="ef719-179">Is van a következő témakörben lévő társított időtúllépés, és ha hello alkalmazás nem tooprocess üdvözlőüzenetére előtt a zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus automatikusan feloldást köszönőüzenetei és a rendelkezésre álló toobe újbóli fogadását.</span><span class="sxs-lookup"><span data-stu-id="ef719-179">There is also a timeout associated with a message locked within the topic, and if hello application fails tooprocess hello message before the lock timeout expires (for example, if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="ef719-180">A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello **deleteMessage** kérés kiadása, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazást, amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="ef719-180">In hello event that hello application crashes after processing hello message but before hello **deleteMessage** request is issued, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="ef719-181">Ezt gyakran nevezik **legalább egyszeri feldolgozásnak**, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet.</span><span class="sxs-lookup"><span data-stu-id="ef719-181">This is often called **At Least Once Processing**, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="ef719-182">Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését.</span><span class="sxs-lookup"><span data-stu-id="ef719-182">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="ef719-183">Ez gyakran érhető el hello segítségével **getMessageId** üdvözlőüzenetére, amely állandó marad a kézbesítési kísérletek metódusában.</span><span class="sxs-lookup"><span data-stu-id="ef719-183">This is often achieved using hello **getMessageId** method of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="ef719-184">Témakörök és előfizetések törlése</span><span class="sxs-lookup"><span data-stu-id="ef719-184">Delete topics and subscriptions</span></span>
<span data-ttu-id="ef719-185">hello elsődleges módon toodelete témakörök és előfizetések toouse egy **ServiceBusContract** objektum.</span><span class="sxs-lookup"><span data-stu-id="ef719-185">hello primary way toodelete topics and subscriptions is toouse a **ServiceBusContract** object.</span></span> <span data-ttu-id="ef719-186">Egy témakör törlése hello témakörben regisztrált előfizetésekkel is törli.</span><span class="sxs-lookup"><span data-stu-id="ef719-186">Deleting a topic will also delete any subscriptions that are registered with hello topic.</span></span> <span data-ttu-id="ef719-187">Az előfizetések független módon is törölhetők.</span><span class="sxs-lookup"><span data-stu-id="ef719-187">Subscriptions can also be deleted independently.</span></span>

```java
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a><span data-ttu-id="ef719-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef719-188">Next Steps</span></span>
<span data-ttu-id="ef719-189">Most, hogy megismerte a Service Bus-üzenetsorok alapjait hello, lásd: [Service Bus-üzenetsorok, témakörök és előfizetések] [ Service Bus queues, topics, and subscriptions] további információt.</span><span class="sxs-lookup"><span data-stu-id="ef719-189">Now that you've learned hello basics of Service Bus queues, see [Service Bus queues, topics, and subscriptions][Service Bus queues, topics, and subscriptions] for more information.</span></span>

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter 
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#Microsoft_ServiceBus_Messaging_SqlFilter_SqlExpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

[0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
[2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
[3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
