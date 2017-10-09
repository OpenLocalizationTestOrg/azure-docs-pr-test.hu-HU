---
title: "aaaNotification Hubs - vállalati leküldéses architektúrája"
description: "Útmutató az Azure Notification Hubs használatával vállalati környezetben"
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 903023e9-9347-442a-924b-663af85e05c6
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: c3afb83de1ba0882bf99e10f38cca40cb42d07a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-push-architectural-guidance"></a><span data-ttu-id="f25fc-103">Útmutató a vállalati leküldési architektúrákhoz</span><span class="sxs-lookup"><span data-stu-id="f25fc-103">Enterprise push architectural guidance</span></span>
<span data-ttu-id="f25fc-104">A vállalatok ma egyre fokozatosan áthelyezése vagy hello alkalmazottak (belső) létrehozásához vagy az alkalmazásokat a végfelhasználók számára (külső) felé.</span><span class="sxs-lookup"><span data-stu-id="f25fc-104">Enterprises today are gradually moving towards creating mobile applications for either their end users (external) or for hello employees (internal).</span></span> <span data-ttu-id="f25fc-105">Létező háttérrendszerek helyen kell azt Nagyszámítógépek rendelkeznek, vagy bizonyos LoB-alkalmazások, amelyek kell integrálni hello mobilalkalmazás architektúra.</span><span class="sxs-lookup"><span data-stu-id="f25fc-105">They have existing backend systems in place be it mainframes or some LoB applications which must be integrated into hello mobile application architecture.</span></span> <span data-ttu-id="f25fc-106">Ez az útmutató előadás a legjobb módja toodo ezt az integrációt ajánló lehetséges megoldás toocommon forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="f25fc-106">This guide will talk about how best toodo this integration recommending possible solution toocommon scenarios.</span></span>

<span data-ttu-id="f25fc-107">Gyakori követelmény van a leküldéses értesítési toohello felhasználókon a mobilalkalmazás hello háttérrendszerek az egyik fontos esemény bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="f25fc-107">A frequent requirement is for sending push notification toohello users through their mobile application when an event of interest occurs in hello backend systems.</span></span> <span data-ttu-id="f25fc-108">Például</span><span class="sxs-lookup"><span data-stu-id="f25fc-108">E.g.</span></span> <span data-ttu-id="f25fc-109">az iPhone hello banki banki alkalmazás rendelkező banki ügyfél szeretne toobe értesíti, ha tartozik történik egy saját fiókot vagy egy intranetes forgatókönyvet, ahol a pénzügyi részleg egy alkalmazott, a költségvetési jóváhagyást alkalmazások a Windows Phone rendelkező szeretne toobe bizonyos összeg felett értesíti, ha a jóváhagyási kérelem kapjuk.</span><span class="sxs-lookup"><span data-stu-id="f25fc-109">a bank customer who has hello bank's banking app on her iPhone wants toobe notified when a debit is made above a certain amount from her account or an intranet scenario where an employee from finance department who has a budget approval app on his Windows Phone wants toobe notified when he gets an approval request.</span></span>

<span data-ttu-id="f25fc-110">hello banki vagy jóváhagyási feldolgozási valószínűleg toobe néhány háttér rendszert, amely egy leküldéses toohello felhasználónak kell kezdeményeznie végezheti el.</span><span class="sxs-lookup"><span data-stu-id="f25fc-110">hello bank account or approval processing is likely toobe done in some backend system which must initiate a push toohello user.</span></span> <span data-ttu-id="f25fc-111">Előfordulhat, hogy több ilyen háttér kell az összes build rendszereket hello logika tooimplement leküldéses ugyanolyan típusú, ha az eseményt akkor váltja ki egy értesítést.</span><span class="sxs-lookup"><span data-stu-id="f25fc-111">There may be multiple such backend systems which must all build hello same kind of logic tooimplement push when an event triggers a notification.</span></span> <span data-ttu-id="f25fc-112">Itt hello összetettsége létrejönnie egy egyetlen leküldéses rendszert, ahol hello végfelhasználók előfordulhat, hogy előfizetett toodifferent értesítéseket, és lehet, hogy van még lehet például az intranet mobilalkalmazások hello esetben több mobilalkalmazások több háttérrendszerek integrálása egy mobilalkalmazás azt szeretné, ha több ilyen háttérrendszerek tooreceive értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="f25fc-112">hello complexity here lies in integrating several backend systems together with a single push system where hello end users may have subscribed toodifferent notifications and there may even be multiple mobile applications e.g. in hello case of intranet mobile apps where one mobile application may want tooreceive notifications from multiple such backend systems.</span></span> <span data-ttu-id="f25fc-113">hello háttérrendszerek nem ismeri vagy kell tooknow leküldéses szemantikáját/technológia, így a közös megoldást-ra volt toointroduce kérdezze le az hello háttérrendszerek bármely események számára, és leküldéses köszönőüzenetei küldéséért felelős összetevő toohello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="f25fc-113">hello backend systems do not know or need tooknow of push semantics/technology so a common solution here traditionally has been toointroduce a component which polls hello backend systems for any events of interest and is responsible for sending hello push messages toohello client.</span></span>
<span data-ttu-id="f25fc-114">Itt előadás egy használó Azure Service Bus - témakört/előfizetést modell, amely csökkenti a hello összetettsége hello megoldás méretezhető tétele közben is jobb megoldás.</span><span class="sxs-lookup"><span data-stu-id="f25fc-114">Here we will talk about an even better solution using Azure Service Bus - Topic/Subscription model which will reduce hello complexity while making hello solution scalable.</span></span>

<span data-ttu-id="f25fc-115">Ez hello általános architektúra hello megoldás (több mobilalkalmazásokkal általános, de egyaránt alkalmazható, ha csak egy mobilalkalmazás)</span><span class="sxs-lookup"><span data-stu-id="f25fc-115">Here is hello general architecture of hello solution (generalized with multiple mobile apps but equally applicable when there is only one mobile app)</span></span>

## <a name="architecture"></a><span data-ttu-id="f25fc-116">Architektúra</span><span class="sxs-lookup"><span data-stu-id="f25fc-116">Architecture</span></span>
![][1]

<span data-ttu-id="f25fc-117">a architekturális diagramja hello kulcs darab Azure Service Bus, amely a témakörök/előfizetések programozási modellt biztosít (további sablonokat a [Service Bus Pub/Sub programozási]).</span><span class="sxs-lookup"><span data-stu-id="f25fc-117">hello key piece in this architectural diagram is Azure Service Bus which provides a topics/subscriptions programming model (more on it at [Service Bus Pub/Sub programming]).</span></span> <span data-ttu-id="f25fc-118">hello fogadó, amely ebben az esetben hello mobil-háttéralkalmazást (általában [Azure Mobile Services mobilszolgáltatás], amely indít el egy leküldéses toohello mobilalkalmazások) kapott üzenetek közvetlenül hello háttérrendszerek, de ehelyett tudunk egy köztes hardverabsztrakciós réteg által biztosított [Azure Service Bus] lehetővé teszi a mobil-háttéralkalmazást tooreceive üzeneteit egy vagy több háttérrendszerek.</span><span class="sxs-lookup"><span data-stu-id="f25fc-118">hello receiver, which in this case, is hello Mobile backend (typically [Azure Mobile Service], which will initiate a push toohello mobile apps) does not receive messages directly from hello backend systems but instead we have an intermediate abstraction layer provided by [Azure Service Bus] which enables mobile backend tooreceive messages from one or more backend systems.</span></span> <span data-ttu-id="f25fc-119">A Service Bus-témakörbe létre minden egyes hello háttér rendszerek pl. fiók HR, pénzügyi, amelyek alapvetően "témakörök" iránt, amelyek fogja elindítani leküldéses értesítéseket küldött üzenetek toobe toobe kell.</span><span class="sxs-lookup"><span data-stu-id="f25fc-119">A Service Bus Topic needs toobe created for each of hello backend systems e.g. Account, HR, Finance which are basically "topics" of interest which will initiate messages toobe sent as push notification.</span></span> <span data-ttu-id="f25fc-120">hello háttérrendszerek üzenetet szeretne küldeni toothese témaköröket.</span><span class="sxs-lookup"><span data-stu-id="f25fc-120">hello backend systems will send messages toothese topics.</span></span> <span data-ttu-id="f25fc-121">A mobil-háttéralkalmazás fizethet tooone vagy több ilyen témakörök által a Service Bus-előfizetés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f25fc-121">A Mobile Backend can subscribe tooone or more such topics by creating a Service Bus subscription.</span></span> <span data-ttu-id="f25fc-122">Ez feljogosítja hello mobil-háttéralkalmazást tooreceive hello megfelelő háttérrendszer értesítéseinek.</span><span class="sxs-lookup"><span data-stu-id="f25fc-122">This will entitle hello mobile backend tooreceive a notification from hello corresponding backend system.</span></span> <span data-ttu-id="f25fc-123">Mobil-háttéralkalmazást üzenetek toolisten folytatja az előfizetések és, amint egy üzenet érkezik, akkor kapcsolja vissza, és elküldi azt értesítési tooits értesítési központot.</span><span class="sxs-lookup"><span data-stu-id="f25fc-123">Mobile backend continues toolisten for messages on their subscriptions and as soon as a message arrives, it turns back and sends it as notification tooits notification hub.</span></span> <span data-ttu-id="f25fc-124">A Notification hubs felé kézbesíti hello üzenet toohello mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f25fc-124">Notification hubs then eventually delivers hello message toohello mobile app.</span></span> <span data-ttu-id="f25fc-125">Ezért toosummarize hello legfontosabb összetevők, vezetünk be:</span><span class="sxs-lookup"><span data-stu-id="f25fc-125">So toosummarize hello key components, we have:</span></span>

1. <span data-ttu-id="f25fc-126">A háttérrendszerek (LoB/örökölt rendszerek)</span><span class="sxs-lookup"><span data-stu-id="f25fc-126">Backend systems (LoB/Legacy systems)</span></span>
   * <span data-ttu-id="f25fc-127">Service Bus-témakörbe létrehozása</span><span class="sxs-lookup"><span data-stu-id="f25fc-127">Creates Service Bus Topic</span></span>
   * <span data-ttu-id="f25fc-128">Üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="f25fc-128">Sends Message</span></span>
2. <span data-ttu-id="f25fc-129">Mobil háttérszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f25fc-129">Mobile backend</span></span>
   * <span data-ttu-id="f25fc-130">Szolgáltatás-előfizetést hoz létre</span><span class="sxs-lookup"><span data-stu-id="f25fc-130">Creates Service Subscription</span></span>
   * <span data-ttu-id="f25fc-131">Üzenet fogadása (a háttérrendszer)</span><span class="sxs-lookup"><span data-stu-id="f25fc-131">Receives Message (from Backend system)</span></span>
   * <span data-ttu-id="f25fc-132">Elküldi az értesítést tooclients (az Azure Notification Hub) keresztül</span><span class="sxs-lookup"><span data-stu-id="f25fc-132">Sends notification tooclients (via Azure Notification Hub)</span></span>
3. <span data-ttu-id="f25fc-133">Mobilalkalmazás</span><span class="sxs-lookup"><span data-stu-id="f25fc-133">Mobile Application</span></span>
   * <span data-ttu-id="f25fc-134">Kap, és értesítést jelenít meg</span><span class="sxs-lookup"><span data-stu-id="f25fc-134">Receives and display notification</span></span>

### <a name="benefits"></a><span data-ttu-id="f25fc-135">Előnyei:</span><span class="sxs-lookup"><span data-stu-id="f25fc-135">Benefits:</span></span>
1. <span data-ttu-id="f25fc-136">hello szétválasztását hello fogadó (app/mobilszolgáltatás Notification Hub használatával) és a feladó (háttérrendszerek) lehetővé teszi, hogy további háttérrendszerek integrálva van a minimális módosítása.</span><span class="sxs-lookup"><span data-stu-id="f25fc-136">hello decoupling between hello receiver (mobile app/service via Notification Hub) and sender (backend systems) enables additional backend systems being integrated with minimal change.</span></span>
2. <span data-ttu-id="f25fc-137">Így több mobilalkalmazások éppen egy vagy több háttérrendszerek képes tooreceive eseményeinek hello a forgatókönyvet.</span><span class="sxs-lookup"><span data-stu-id="f25fc-137">This also makes hello scenario of multiple mobile apps being able tooreceive events from one or more backend systems.</span></span>  

## <a name="sample"></a><span data-ttu-id="f25fc-138">Minta:</span><span class="sxs-lookup"><span data-stu-id="f25fc-138">Sample:</span></span>
### <a name="prerequisites"></a><span data-ttu-id="f25fc-139">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f25fc-139">Prerequisites</span></span>
<span data-ttu-id="f25fc-140">El kell végeznie az alábbi oktatóanyagok toofamiliarize hello fogalmait, valamint a gyakori létrehozása a & konfigurációs lépések hello:</span><span class="sxs-lookup"><span data-stu-id="f25fc-140">You should complete hello following tutorials toofamiliarize with hello concepts as well as common creation & configuration steps:</span></span>

1. <span data-ttu-id="f25fc-141">[Service Bus Pub/Sub programozási] -ez magyarázattal hello részleteit működő Service Bus témakörök/előfizetések, hogyan toocreate névtér toocontain témakörök/előfizetések, hogyan toosend & üzeneteket fogadjon.</span><span class="sxs-lookup"><span data-stu-id="f25fc-141">[Service Bus Pub/Sub programming] - This explains hello details of working with Service Bus Topics/Subscriptions, how toocreate a namespace toocontain topics/subscriptions, how toosend & receive messages from them.</span></span>
2. <span data-ttu-id="f25fc-142">[A Notification Hubs - oktatóanyag univerzális Windows-] -ismerteti, hogyan tooset be a Windows Áruházbeli alkalmazások és a Notification Hubs tooregister használja, és majd értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="f25fc-142">[Notification Hubs - Windows Universal tutorial] - This explains how tooset up a Windows Store app and use Notification Hubs tooregister and then receive notifications.</span></span>

### <a name="sample-code"></a><span data-ttu-id="f25fc-143">Mintakód</span><span class="sxs-lookup"><span data-stu-id="f25fc-143">Sample code</span></span>
<span data-ttu-id="f25fc-144">hello teljes mintakód érhető el: [Notification Hub minták].</span><span class="sxs-lookup"><span data-stu-id="f25fc-144">hello full sample code is available at [Notification Hub Samples].</span></span> <span data-ttu-id="f25fc-145">A oszlik három összetevővel:</span><span class="sxs-lookup"><span data-stu-id="f25fc-145">It is split into three components:</span></span>

1. <span data-ttu-id="f25fc-146">**EnterprisePushBackendSystem**</span><span class="sxs-lookup"><span data-stu-id="f25fc-146">**EnterprisePushBackendSystem**</span></span>
   
    <span data-ttu-id="f25fc-147">a.</span><span class="sxs-lookup"><span data-stu-id="f25fc-147">a.</span></span> <span data-ttu-id="f25fc-148">A projekt által használt hello *WindowsAzure.ServiceBus* Nuget-csomag alapozva [Service Bus Pub/Sub programozási].</span><span class="sxs-lookup"><span data-stu-id="f25fc-148">This project uses hello *WindowsAzure.ServiceBus* Nuget package and is  based on [Service Bus Pub/Sub programming].</span></span>
   
    <span data-ttu-id="f25fc-149">b.</span><span class="sxs-lookup"><span data-stu-id="f25fc-149">b.</span></span> <span data-ttu-id="f25fc-150">Ez az egy egyszerű C# konzol app toosimulate egy LoB rendszer kezdeményező hello üzenet toobe toohello mobilalkalmazás kézbesíteni.</span><span class="sxs-lookup"><span data-stu-id="f25fc-150">This is a simple C# console app toosimulate an LoB system which initiates hello message toobe delivered toohello mobile app.</span></span>
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create hello topic where we will send notifications
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    <span data-ttu-id="f25fc-151">c.</span><span class="sxs-lookup"><span data-stu-id="f25fc-151">c.</span></span> <span data-ttu-id="f25fc-152">`CreateTopic`Ha az üzeneteket kapni fog van használt toocreate hello Service Bus-témakörbe.</span><span class="sxs-lookup"><span data-stu-id="f25fc-152">`CreateTopic` is used toocreate hello Service Bus topic where we will send messages.</span></span>
   
        public static void CreateTopic(string connectionString)
        {
            // Create hello topic if it does not exist already
   
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }
   
    <span data-ttu-id="f25fc-153">d.</span><span class="sxs-lookup"><span data-stu-id="f25fc-153">d.</span></span> <span data-ttu-id="f25fc-154">`SendMessage`használt toosend hello üzenetek toothis Service Bus-témakörbe.</span><span class="sxs-lookup"><span data-stu-id="f25fc-154">`SendMessage` is used toosend hello messages toothis Service Bus Topic.</span></span> <span data-ttu-id="f25fc-155">Itt azt egyszerűen küldenek véletlenszerű üzenetek toohello témakör rendszeresen hello célra hello minta készlete.</span><span class="sxs-lookup"><span data-stu-id="f25fc-155">Here we are simply sending a set of random messages toohello topic periodically for hello purpose of hello sample.</span></span> <span data-ttu-id="f25fc-156">Általában nem lesznek egy háttér-rendszert, amely üzenetet szeretne küldeni, ha az esemény akkor következik be.</span><span class="sxs-lookup"><span data-stu-id="f25fc-156">Normally there will be a backend system which will send messages when an event occurs.</span></span>
   
        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);
   
            // Sends random messages every 10 seconds toohello topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched tooa different team."
            };
   
            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);
   
                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);
   
                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);
   
                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }
2. <span data-ttu-id="f25fc-157">**ReceiveAndSendNotification**</span><span class="sxs-lookup"><span data-stu-id="f25fc-157">**ReceiveAndSendNotification**</span></span>
   
    <span data-ttu-id="f25fc-158">a.</span><span class="sxs-lookup"><span data-stu-id="f25fc-158">a.</span></span> <span data-ttu-id="f25fc-159">A projekt által használt hello *WindowsAzure.ServiceBus* és *Microsoft.Web.WebJobs.Publish* Nuget a csomagok és alapuló [Service Bus Pub/Sub programozási].</span><span class="sxs-lookup"><span data-stu-id="f25fc-159">This project uses hello *WindowsAzure.ServiceBus* and *Microsoft.Web.WebJobs.Publish* Nuget packages and is based on [Service Bus Pub/Sub programming].</span></span>
   
    <span data-ttu-id="f25fc-160">b.</span><span class="sxs-lookup"><span data-stu-id="f25fc-160">b.</span></span> <span data-ttu-id="f25fc-161">Ez az egy másik C# konzolalkalmazást, amely jelenleg fut egy [Azure webjobs-feladat] mert toorun folyamatosan rendelkezik a toolisten hello LoB-/ háttérrendszerek érkező üzenetek-e.</span><span class="sxs-lookup"><span data-stu-id="f25fc-161">This is another C# console app which we will run as an [Azure WebJob] since it has toorun continuously toolisten for messages from hello LoB/backend systems.</span></span> <span data-ttu-id="f25fc-162">Ez a mobil-háttéralkalmazást része lesz.</span><span class="sxs-lookup"><span data-stu-id="f25fc-162">This will be part of your Mobile backend.</span></span>
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create hello subscription which will receive messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    <span data-ttu-id="f25fc-163">c.</span><span class="sxs-lookup"><span data-stu-id="f25fc-163">c.</span></span> <span data-ttu-id="f25fc-164">`CreateSubscription`van használt toocreate hello a témakör a Service Bus előfizetést ahol hello háttérrendszer fog üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="f25fc-164">`CreateSubscription` is used toocreate a Service Bus subscription for hello topic where hello backend system will send messages.</span></span> <span data-ttu-id="f25fc-165">Attól függően, hogy hello üzleti helyzethez Ez az összetevő hoz létre egy vagy több előfizetés (pl. néhány lehetséges, hogy lehet üzenetek fogadása HR rendszer, egyes pénzügyi rendszer, és így tovább) toocorresponding kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="f25fc-165">Depending on hello business scenario, this component will create one or more subscriptions toocorresponding topics (e.g. some may be receiving messages from HR system, some from Finance system, and so on)</span></span>
   
        static void CreateSubscription(string connectionString)
        {
            // Create hello subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }
   
    <span data-ttu-id="f25fc-166">d.</span><span class="sxs-lookup"><span data-stu-id="f25fc-166">d.</span></span> <span data-ttu-id="f25fc-167">ReceiveMessageAndSendNotification hello témakör segítségével az általa használt tooread hello üzenetét, és ha olvasható hello sikeres majd kézműipari (a hello mintaforgatókönyv Windows natív bejelentési értesítés) értesítési küldött toobe toohello mobil az alkalmazás Azure Notification Hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="f25fc-167">ReceiveMessageAndSendNotification is used tooread hello message from hello topic using its subscription and if hello read is successful then craft a notification (in hello sample scenario a Windows native toast notification) toobe sent toohello mobile application using Azure Notification Hubs.</span></span>
   
        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize hello Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");
   
            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);
   
            Client.Receive();
   
            // Continuously process messages received from hello subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";
   
                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");
   
                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);
   
                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }
   
    <span data-ttu-id="f25fc-168">e.</span><span class="sxs-lookup"><span data-stu-id="f25fc-168">e.</span></span> <span data-ttu-id="f25fc-169">Közzététel ezt egy **webjobs-feladat**, kattintson jobb gombbal a hello megoldást a Visual Studióban, és válassza ki **webjobs-feladat közzététel**</span><span class="sxs-lookup"><span data-stu-id="f25fc-169">For publishing this as a **WebJob**, right click on hello solution in Visual Studio and select **Publish as WebJob**</span></span>
   
    ![][2]
   
    <span data-ttu-id="f25fc-170">f.</span><span class="sxs-lookup"><span data-stu-id="f25fc-170">f.</span></span> <span data-ttu-id="f25fc-171">Válassza ki a közzétételi profilt, és hozzon létre egy új Azure-webhely, ha még nem létezik már fogja tárolni, amely a webjobs-feladat, és ha hello webhely majd **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="f25fc-171">Select your publishing profile and create a new Azure WebSite if it doesnt exist already which will host this WebJob and once you have hello WebSite then **Publish**.</span></span>
   
    ![][3]
   
    <span data-ttu-id="f25fc-172">g.</span><span class="sxs-lookup"><span data-stu-id="f25fc-172">g.</span></span> <span data-ttu-id="f25fc-173">Hello feladat "Folyamatos Futtatás" toobe konfigurálja, hogy mikor jelentkezik be a toohello [klasszikus Azure portál] hello következő hasonlót kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="f25fc-173">Configure hello job toobe "Run Continuously" so that when you log in toohello [Azure Classic Portal] you should see something like hello following:</span></span>
   
    ![][4]
3. <span data-ttu-id="f25fc-174">**EnterprisePushMobileApp**</span><span class="sxs-lookup"><span data-stu-id="f25fc-174">**EnterprisePushMobileApp**</span></span>
   
    <span data-ttu-id="f25fc-175">a.</span><span class="sxs-lookup"><span data-stu-id="f25fc-175">a.</span></span> <span data-ttu-id="f25fc-176">Ez az egy Windows Áruházbeli alkalmazással, amely bejelentési értesítéseket fogadni a mobil-háttéralkalmazást részeként hello webjobs-feladat futását, és megjeleníti azt.</span><span class="sxs-lookup"><span data-stu-id="f25fc-176">This is a Windows Store application which will receive toast notifications from hello WebJob running as part of your Mobile backend and display it.</span></span> <span data-ttu-id="f25fc-177">Ez a alapul [A Notification Hubs - oktatóanyag univerzális Windows-].</span><span class="sxs-lookup"><span data-stu-id="f25fc-177">This is based on [Notification Hubs - Windows Universal tutorial].</span></span>  
   
    <span data-ttu-id="f25fc-178">b.</span><span class="sxs-lookup"><span data-stu-id="f25fc-178">b.</span></span> <span data-ttu-id="f25fc-179">Győződjön meg arról, hogy az alkalmazás engedélyezett tooreceive bejelentési értesítést.</span><span class="sxs-lookup"><span data-stu-id="f25fc-179">Ensure that your application is enabled tooreceive toast notifications.</span></span>
   
    <span data-ttu-id="f25fc-180">c.</span><span class="sxs-lookup"><span data-stu-id="f25fc-180">c.</span></span> <span data-ttu-id="f25fc-181">Győződjön meg arról, hogy a következő kódot a Notification Hubs regisztrációs hello: hello App elindításához lett meghívva (hello cseréje után *HubName* és *DefaultListenSharedAccessSignature*:</span><span class="sxs-lookup"><span data-stu-id="f25fc-181">Ensure that hello following Notification Hubs registration code is being called at hello App start up (after replacing hello *HubName* and *DefaultListenSharedAccessSignature*:</span></span>
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays hello registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a><span data-ttu-id="f25fc-182">A minta fut:</span><span class="sxs-lookup"><span data-stu-id="f25fc-182">Running sample:</span></span>
1. <span data-ttu-id="f25fc-183">Győződjön meg arról, hogy a webjobs-feladat sikeresen fut, valamint ütemezett, túl "folyamatos Futtatás".</span><span class="sxs-lookup"><span data-stu-id="f25fc-183">Ensure that your WebJob is running successfully and scheduled too"Run Continuously".</span></span>
2. <span data-ttu-id="f25fc-184">Futtassa a hello **EnterprisePushMobileApp** amely hello Windows Áruházbeli alkalmazás elindul.</span><span class="sxs-lookup"><span data-stu-id="f25fc-184">Run hello **EnterprisePushMobileApp** which will start hello Windows Store app.</span></span>
3. <span data-ttu-id="f25fc-185">Futtassa a hello **EnterprisePushBackendSystem** konzolalkalmazást, amely szimulálni fogja hello LoB háttér és küldésének megkezdése üzeneteket, és szereplő hello hasonló bejelentési értesítést kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="f25fc-185">Run hello **EnterprisePushBackendSystem** console application which will simulate hello LoB backend and will start sending messages and you should see toast notifications appearing like hello following:</span></span>
   
    ![][5]
4. <span data-ttu-id="f25fc-186">hello eredetileg üzeneteket tooService Bus-témaköröktől, amely a Service Bus-előfizetések műveleteit a webes projekt által figyelt.</span><span class="sxs-lookup"><span data-stu-id="f25fc-186">hello messages were originally sent tooService Bus topics which was being monitored by Service Bus subscriptions in your Web Job.</span></span> <span data-ttu-id="f25fc-187">Amennyiben az üzenet érkezett, egy értesítés létrejött, de toohello mobilalkalmazás küldött.</span><span class="sxs-lookup"><span data-stu-id="f25fc-187">Once a message was received, a notification was created and sent toohello mobile app.</span></span> <span data-ttu-id="f25fc-188">A webjobs-feladat naplózza tooconfirm hello feldolgozása naplók hivatkozásra toohello váltáskor hello meggyőződhet [klasszikus Azure portál] a webes projekt:</span><span class="sxs-lookup"><span data-stu-id="f25fc-188">You can look through hello WebJob logs tooconfirm hello processing when you go toohello Logs link in [Azure Classic Portal] for your Web Job:</span></span>
   
    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Notification Hub minták]: https://github.com/Azure/azure-notificationhubs-samples
[Azure Mobile Services mobilszolgáltatás]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Pub/Sub programozási]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure webjobs-feladat]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[A Notification Hubs - oktatóanyag univerzális Windows-]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[klasszikus Azure portál]: https://manage.windowsazure.com/
