---
title: "A Notification Hubs - vállalati leküldéses architektúrája"
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
ms.openlocfilehash: ae7c1c9644ecfe7fe4ad6e332cc0683a3b5df22f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enterprise-push-architectural-guidance"></a><span data-ttu-id="ca5af-103">Útmutató a vállalati leküldési architektúrákhoz</span><span class="sxs-lookup"><span data-stu-id="ca5af-103">Enterprise push architectural guidance</span></span>
<span data-ttu-id="ca5af-104">A vállalatok ma egyre fokozatosan áthelyezése felé létrehozása vagy az alkalmazásokat a végfelhasználók számára (külső) vagy (belső) az alkalmazottak számára.</span><span class="sxs-lookup"><span data-stu-id="ca5af-104">Enterprises today are gradually moving towards creating mobile applications for either their end users (external) or for the employees (internal).</span></span> <span data-ttu-id="ca5af-105">Létező háttérrendszerek helyen kell azt Nagyszámítógépek vagy néhány LoB-alkalmazások, amelyek integrálni kell a mobilalkalmazás-architektúra rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="ca5af-105">They have existing backend systems in place be it mainframes or some LoB applications which must be integrated into the mobile application architecture.</span></span> <span data-ttu-id="ca5af-106">Ez az útmutató előadás hogyan legjobb, ha ez az integráció ajánló lehetséges megoldás a gyakori forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="ca5af-106">This guide will talk about how best to do this integration recommending possible solution to common scenarios.</span></span>

<span data-ttu-id="ca5af-107">Gyakori követelmény van a leküldéses értesítések küldését a felhasználók számára a mobilalkalmazás keresztül, a háttér-rendszer egyik fontos esemény bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="ca5af-107">A frequent requirement is for sending push notification to the users through their mobile application when an event of interest occurs in the backend systems.</span></span> <span data-ttu-id="ca5af-108">Például</span><span class="sxs-lookup"><span data-stu-id="ca5af-108">E.g.</span></span> <span data-ttu-id="ca5af-109">egy bank ügyfél, a bank banki alkalmazást az iPhone rendelkező szeretne értesítést tartozik szeretné tenni saját fiók vagy egy intranetes forgatókönyvet, ahol a pénzügyi részleg egy alkalmazott, a költségvetési jóváhagyást alkalmazások a Windows Phone rendelkező szeretné nem lehet egy bizonyos összeg felett Ha a jóváhagyási kérelem kapjuk egadott.</span><span class="sxs-lookup"><span data-stu-id="ca5af-109">a bank customer who has the bank's banking app on her iPhone wants to be notified when a debit is made above a certain amount from her account or an intranet scenario where an employee from finance department who has a budget approval app on his Windows Phone wants to be notified when he gets an approval request.</span></span>

<span data-ttu-id="ca5af-110">A banki vagy jóváhagyási feldolgozási valószínű, hogy néhány háttérrendszer, amely egy leküldéses a felhasználónak kell kezdeményeznie végezhető.</span><span class="sxs-lookup"><span data-stu-id="ca5af-110">The bank account or approval processing is likely to be done in some backend system which must initiate a push to the user.</span></span> <span data-ttu-id="ca5af-111">Előfordulhat, hogy több ilyen kell az összes build az eseményt akkor váltja ki egy értesítést leküldéses megvalósítási logika ugyanolyan típusú háttér rendszereket.</span><span class="sxs-lookup"><span data-stu-id="ca5af-111">There may be multiple such backend systems which must all build the same kind of logic to implement push when an event triggers a notification.</span></span> <span data-ttu-id="ca5af-112">Itt összetettségét létrejönnie egy egyetlen leküldéses rendszert, ahol lehetséges, hogy a végfelhasználók számára különböző értesítések előfizetett és még akkor is előfordulhat, például intranet mobilalkalmazások esetében több mobilalkalmazások Ha az egyik több háttérrendszerek integrálása mobilalkalmazás előfordulhat, hogy értesítést szeretne kapni a több ilyen háttérrendszerek.</span><span class="sxs-lookup"><span data-stu-id="ca5af-112">The complexity here lies in integrating several backend systems together with a single push system where the end users may have subscribed to different notifications and there may even be multiple mobile applications e.g. in the case of intranet mobile apps where one mobile application may want to receive notifications from multiple such backend systems.</span></span> <span data-ttu-id="ca5af-113">A háttérrendszerek nem ismeri vagy tudniuk kell, hogy leküldéses szemantikáját/technológia, így a közös megoldást-ra volt egy összetevő, amely lekérdezi a háttérrendszerek bármely események számára, és a leküldéses üzenetek küldéséért felelős bevezetni a az ügyfél.</span><span class="sxs-lookup"><span data-stu-id="ca5af-113">The backend systems do not know or need to know of push semantics/technology so a common solution here traditionally has been to introduce a component which polls the backend systems for any events of interest and is responsible for sending the push messages to the client.</span></span>
<span data-ttu-id="ca5af-114">Itt előadás egy használó Azure Service Bus - témakört/előfizetést modell, amely csökkenti a összetettségét a megoldás méretezhető tétele közben is jobb megoldás.</span><span class="sxs-lookup"><span data-stu-id="ca5af-114">Here we will talk about an even better solution using Azure Service Bus - Topic/Subscription model which will reduce the complexity while making the solution scalable.</span></span>

<span data-ttu-id="ca5af-115">Ez a megoldás általános architektúrájának (több mobilalkalmazásokkal általános, de egyaránt alkalmazható, ha csak egy mobilalkalmazás)</span><span class="sxs-lookup"><span data-stu-id="ca5af-115">Here is the general architecture of the solution (generalized with multiple mobile apps but equally applicable when there is only one mobile app)</span></span>

## <a name="architecture"></a><span data-ttu-id="ca5af-116">Architektúra</span><span class="sxs-lookup"><span data-stu-id="ca5af-116">Architecture</span></span>
![][1]

<span data-ttu-id="ca5af-117">A kulcs darab a architekturális diagram Azure Service Bus, amely a témakörök/előfizetések programozási modellt biztosít (további sablonokat a [Service Bus Pub/Sub programozási]).</span><span class="sxs-lookup"><span data-stu-id="ca5af-117">The key piece in this architectural diagram is Azure Service Bus which provides a topics/subscriptions programming model (more on it at [Service Bus Pub/Sub programming]).</span></span> <span data-ttu-id="ca5af-118">A fogadó, amely ebben az esetben a mobil-háttéralkalmazást (általában [Azure Mobile Services mobilszolgáltatás], amely indít el a mobilalkalmazások leküldéses) kapott üzenetek közvetlenül a háttérrendszerről, de ehelyett egy köztes tudunk hardverabsztrakciós réteg által biztosított [Azure Service Bus] lehetővé teszi a mobil-háttéralkalmazást üzenetek fogadása egy vagy több háttérrendszerek.</span><span class="sxs-lookup"><span data-stu-id="ca5af-118">The receiver, which in this case, is the Mobile backend (typically [Azure Mobile Service], which will initiate a push to the mobile apps) does not receive messages directly from the backend systems but instead we have an intermediate abstraction layer provided by [Azure Service Bus] which enables mobile backend to receive messages from one or more backend systems.</span></span> <span data-ttu-id="ca5af-119">A Service Bus-témakörbe kell létrehozni az egyes a háttérrendszerek pl. fiók HR, pénzügyi, amelyek alapvetően "témakörök" iránt, amelyek indít el, leküldéses értesítést küldeni.</span><span class="sxs-lookup"><span data-stu-id="ca5af-119">A Service Bus Topic needs to be created for each of the backend systems e.g. Account, HR, Finance which are basically "topics" of interest which will initiate messages to be sent as push notification.</span></span> <span data-ttu-id="ca5af-120">A háttérrendszerek témákhoz üzenetet szeretne küldeni.</span><span class="sxs-lookup"><span data-stu-id="ca5af-120">The backend systems will send messages to these topics.</span></span> <span data-ttu-id="ca5af-121">A mobil-háttéralkalmazás kérhet le egy vagy több ilyen témakörök által a Service Bus-előfizetés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ca5af-121">A Mobile Backend can subscribe to one or more such topics by creating a Service Bus subscription.</span></span> <span data-ttu-id="ca5af-122">Ez feljogosítja a mobil-háttéralkalmazást is megkapja az értesítéseket a megfelelő háttér rendszerből.</span><span class="sxs-lookup"><span data-stu-id="ca5af-122">This will entitle the mobile backend to receive a notification from the corresponding backend system.</span></span> <span data-ttu-id="ca5af-123">Mobil-háttéralkalmazást továbbra is az üzeneteket az előfizetések és, amint egy üzenet érkezik, akkor kapcsolja vissza, és elküldi az értesítési központba értesítésként.</span><span class="sxs-lookup"><span data-stu-id="ca5af-123">Mobile backend continues to listen for messages on their subscriptions and as soon as a message arrives, it turns back and sends it as notification to its notification hub.</span></span> <span data-ttu-id="ca5af-124">A Notification hubs majd végül kézbesíti az üzenetet a mobilalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ca5af-124">Notification hubs then eventually delivers the message to the mobile app.</span></span> <span data-ttu-id="ca5af-125">Úgy, hogy összesítse a legfontosabb összetevők, vezetünk be:</span><span class="sxs-lookup"><span data-stu-id="ca5af-125">So to summarize the key components, we have:</span></span>

1. <span data-ttu-id="ca5af-126">A háttérrendszerek (LoB/örökölt rendszerek)</span><span class="sxs-lookup"><span data-stu-id="ca5af-126">Backend systems (LoB/Legacy systems)</span></span>
   * <span data-ttu-id="ca5af-127">Service Bus-témakörbe létrehozása</span><span class="sxs-lookup"><span data-stu-id="ca5af-127">Creates Service Bus Topic</span></span>
   * <span data-ttu-id="ca5af-128">Üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="ca5af-128">Sends Message</span></span>
2. <span data-ttu-id="ca5af-129">Mobil háttérszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ca5af-129">Mobile backend</span></span>
   * <span data-ttu-id="ca5af-130">Szolgáltatás-előfizetést hoz létre</span><span class="sxs-lookup"><span data-stu-id="ca5af-130">Creates Service Subscription</span></span>
   * <span data-ttu-id="ca5af-131">Üzenet fogadása (a háttérrendszer)</span><span class="sxs-lookup"><span data-stu-id="ca5af-131">Receives Message (from Backend system)</span></span>
   * <span data-ttu-id="ca5af-132">Értesítés küldése (az Azure Notification Hub) keresztül ügyfelek</span><span class="sxs-lookup"><span data-stu-id="ca5af-132">Sends notification to clients (via Azure Notification Hub)</span></span>
3. <span data-ttu-id="ca5af-133">Mobilalkalmazás</span><span class="sxs-lookup"><span data-stu-id="ca5af-133">Mobile Application</span></span>
   * <span data-ttu-id="ca5af-134">Kap, és értesítést jelenít meg</span><span class="sxs-lookup"><span data-stu-id="ca5af-134">Receives and display notification</span></span>

### <a name="benefits"></a><span data-ttu-id="ca5af-135">Előnyei:</span><span class="sxs-lookup"><span data-stu-id="ca5af-135">Benefits:</span></span>
1. <span data-ttu-id="ca5af-136">A fogadó (app/mobilszolgáltatás Notification Hub használatával) és a feladó (háttérrendszerek) közötti leválasztásával lehetővé teszi, hogy további háttérrendszerek integrálva van a minimális módosítása.</span><span class="sxs-lookup"><span data-stu-id="ca5af-136">The decoupling between the receiver (mobile app/service via Notification Hub) and sender (backend systems) enables additional backend systems being integrated with minimal change.</span></span>
2. <span data-ttu-id="ca5af-137">Így a több mobilalkalmazások tudnak majd egy vagy több háttérrendszerről származó események fogadására a forgatókönyvet.</span><span class="sxs-lookup"><span data-stu-id="ca5af-137">This also makes the scenario of multiple mobile apps being able to receive events from one or more backend systems.</span></span>  

## <a name="sample"></a><span data-ttu-id="ca5af-138">Minta:</span><span class="sxs-lookup"><span data-stu-id="ca5af-138">Sample:</span></span>
### <a name="prerequisites"></a><span data-ttu-id="ca5af-139">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ca5af-139">Prerequisites</span></span>
<span data-ttu-id="ca5af-140">El kell végeznie az alábbi oktatóanyagok megismerkednie a fogalmakat, valamint közös létrehozása és konfigurációs lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ca5af-140">You should complete the following tutorials to familiarize with the concepts as well as common creation & configuration steps:</span></span>

1. <span data-ttu-id="ca5af-141">[Service Bus Pub/Sub programozási] -ebben a részben működik-e a Service Bus témakörök/előfizetések, részleteit tartalmazó témakörök/előfizetésekre, a névtér létrehozása hogyan üzeneteket fogadjon & küldése.</span><span class="sxs-lookup"><span data-stu-id="ca5af-141">[Service Bus Pub/Sub programming] - This explains the details of working with Service Bus Topics/Subscriptions, how to create a namespace to contain topics/subscriptions, how to send & receive messages from them.</span></span>
2. <span data-ttu-id="ca5af-142">[A Notification Hubs - oktatóanyag univerzális Windows-] -ezt ismerteti, hogyan állítson be egy Windows Áruházbeli alkalmazást és regisztrálja, és ezután kapni az értesítések a Notification Hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="ca5af-142">[Notification Hubs - Windows Universal tutorial] - This explains how to set up a Windows Store app and use Notification Hubs to register and then receive notifications.</span></span>

### <a name="sample-code"></a><span data-ttu-id="ca5af-143">Mintakód</span><span class="sxs-lookup"><span data-stu-id="ca5af-143">Sample code</span></span>
<span data-ttu-id="ca5af-144">A teljes minta kódja megtalálható [Notification Hub minták].</span><span class="sxs-lookup"><span data-stu-id="ca5af-144">The full sample code is available at [Notification Hub Samples].</span></span> <span data-ttu-id="ca5af-145">A oszlik három összetevővel:</span><span class="sxs-lookup"><span data-stu-id="ca5af-145">It is split into three components:</span></span>

1. <span data-ttu-id="ca5af-146">**EnterprisePushBackendSystem**</span><span class="sxs-lookup"><span data-stu-id="ca5af-146">**EnterprisePushBackendSystem**</span></span>
   
    <span data-ttu-id="ca5af-147">a.</span><span class="sxs-lookup"><span data-stu-id="ca5af-147">a.</span></span> <span data-ttu-id="ca5af-148">Ez a projekt használja a *WindowsAzure.ServiceBus* Nuget-csomag alapozva [Service Bus Pub/Sub programozási].</span><span class="sxs-lookup"><span data-stu-id="ca5af-148">This project uses the *WindowsAzure.ServiceBus* Nuget package and is  based on [Service Bus Pub/Sub programming].</span></span>
   
    <span data-ttu-id="ca5af-149">b.</span><span class="sxs-lookup"><span data-stu-id="ca5af-149">b.</span></span> <span data-ttu-id="ca5af-150">Ez az egy egyszerű C# Konzolalkalmazás egy LoB szimulálására kezdeményező a mobilalkalmazás küldendő üzenet.</span><span class="sxs-lookup"><span data-stu-id="ca5af-150">This is a simple C# console app to simulate an LoB system which initiates the message to be delivered to the mobile app.</span></span>
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the topic where we will send notifications
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    <span data-ttu-id="ca5af-151">c.</span><span class="sxs-lookup"><span data-stu-id="ca5af-151">c.</span></span> <span data-ttu-id="ca5af-152">`CreateTopic`létrehozásához használt a Service Bus-témakörbe ahol üzenetet küldünk.</span><span class="sxs-lookup"><span data-stu-id="ca5af-152">`CreateTopic` is used to create the Service Bus topic where we will send messages.</span></span>
   
        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already
   
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }
   
    <span data-ttu-id="ca5af-153">d.</span><span class="sxs-lookup"><span data-stu-id="ca5af-153">d.</span></span> <span data-ttu-id="ca5af-154">`SendMessage`az üzenetek küldése a Service Bus-témakörbe szolgál.</span><span class="sxs-lookup"><span data-stu-id="ca5af-154">`SendMessage` is used to send the messages to this Service Bus Topic.</span></span> <span data-ttu-id="ca5af-155">Itt azt egyszerűen küldenek véletlenszerű üzenetek készleteit a témakörhöz, rendszeres időközönként a minta céljából.</span><span class="sxs-lookup"><span data-stu-id="ca5af-155">Here we are simply sending a set of random messages to the topic periodically for the purpose of the sample.</span></span> <span data-ttu-id="ca5af-156">Általában nem lesznek egy háttér-rendszert, amely üzenetet szeretne küldeni, ha az esemény akkor következik be.</span><span class="sxs-lookup"><span data-stu-id="ca5af-156">Normally there will be a backend system which will send messages when an event occurs.</span></span>
   
        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);
   
            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
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
2. <span data-ttu-id="ca5af-157">**ReceiveAndSendNotification**</span><span class="sxs-lookup"><span data-stu-id="ca5af-157">**ReceiveAndSendNotification**</span></span>
   
    <span data-ttu-id="ca5af-158">a.</span><span class="sxs-lookup"><span data-stu-id="ca5af-158">a.</span></span> <span data-ttu-id="ca5af-159">Ez a projekt használja a *WindowsAzure.ServiceBus* és *Microsoft.Web.WebJobs.Publish* Nuget a csomagok és alapuló [Service Bus Pub/Sub programozási].</span><span class="sxs-lookup"><span data-stu-id="ca5af-159">This project uses the *WindowsAzure.ServiceBus* and *Microsoft.Web.WebJobs.Publish* Nuget packages and is based on [Service Bus Pub/Sub programming].</span></span>
   
    <span data-ttu-id="ca5af-160">b.</span><span class="sxs-lookup"><span data-stu-id="ca5af-160">b.</span></span> <span data-ttu-id="ca5af-161">Ez az egy másik C# konzolalkalmazást, amely jelenleg fut egy [Azure webjobs-feladat] mivel folyamatosan figyelni a LoB-/ háttérrendszerek üzeneteit futtatásához rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ca5af-161">This is another C# console app which we will run as an [Azure WebJob] since it has to run continuously to listen for messages from the LoB/backend systems.</span></span> <span data-ttu-id="ca5af-162">Ez a mobil-háttéralkalmazást része lesz.</span><span class="sxs-lookup"><span data-stu-id="ca5af-162">This will be part of your Mobile backend.</span></span>
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the subscription which will receive messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    <span data-ttu-id="ca5af-163">c.</span><span class="sxs-lookup"><span data-stu-id="ca5af-163">c.</span></span> <span data-ttu-id="ca5af-164">`CreateSubscription`segítségével hozzon létre egy Service Bus-előfizetést, a témakör ahol a háttérrendszer fog üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="ca5af-164">`CreateSubscription` is used to create a Service Bus subscription for the topic where the backend system will send messages.</span></span> <span data-ttu-id="ca5af-165">Attól függően, hogy az üzleti terület Ez az összetevő hoz létre egy vagy több előfizetés (pl. néhány lehetséges, hogy lehet üzenetek fogadása HR rendszer, egyes pénzügyi rendszer, és így tovább) kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="ca5af-165">Depending on the business scenario, this component will create one or more subscriptions to corresponding topics (e.g. some may be receiving messages from HR system, some from Finance system, and so on)</span></span>
   
        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }
   
    <span data-ttu-id="ca5af-166">d.</span><span class="sxs-lookup"><span data-stu-id="ca5af-166">d.</span></span> <span data-ttu-id="ca5af-167">Az üzenet olvasni a témakör az előfizetést, és ha az olvasás sikeres majd kézműipari (forgatókönyvében a minta egy Windows natív bejelentési értesítés) értesítést a mobilalkalmazás használata Azure küldendő ReceiveMessageAndSendNotification szolgál Notification hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="ca5af-167">ReceiveMessageAndSendNotification is used to read the message from the topic using its subscription and if the read is successful then craft a notification (in the sample scenario a Windows native toast notification) to be sent to the mobile application using Azure Notification Hubs.</span></span>
   
        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");
   
            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);
   
            Client.Receive();
   
            // Continuously process messages received from the subscription
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
   
    <span data-ttu-id="ca5af-168">e.</span><span class="sxs-lookup"><span data-stu-id="ca5af-168">e.</span></span> <span data-ttu-id="ca5af-169">Közzététel ezt egy **webjobs-feladat**, a Visual Studio-megoldásban kattintson a jobb gombbal, és válassza ki **webjobs-feladat közzététel**</span><span class="sxs-lookup"><span data-stu-id="ca5af-169">For publishing this as a **WebJob**, right click on the solution in Visual Studio and select **Publish as WebJob**</span></span>
   
    ![][2]
   
    <span data-ttu-id="ca5af-170">f.</span><span class="sxs-lookup"><span data-stu-id="ca5af-170">f.</span></span> <span data-ttu-id="ca5af-171">Válassza ki a közzétételi profilt, és hozzon létre egy új Azure-webhelyre, ha még nem létezik már fogja tárolni, amely a webjobs-feladat, és ha a webhely majd **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="ca5af-171">Select your publishing profile and create a new Azure WebSite if it doesnt exist already which will host this WebJob and once you have the WebSite then **Publish**.</span></span>
   
    ![][3]
   
    <span data-ttu-id="ca5af-172">g.</span><span class="sxs-lookup"><span data-stu-id="ca5af-172">g.</span></span> <span data-ttu-id="ca5af-173">A "Folyamatos Futtatás" kell, hogy mikor jelentkezik be a feladat a [klasszikus Azure portál] látnia kell a következőhöz:</span><span class="sxs-lookup"><span data-stu-id="ca5af-173">Configure the job to be "Run Continuously" so that when you log in to the [Azure Classic Portal] you should see something like the following:</span></span>
   
    ![][4]
3. <span data-ttu-id="ca5af-174">**EnterprisePushMobileApp**</span><span class="sxs-lookup"><span data-stu-id="ca5af-174">**EnterprisePushMobileApp**</span></span>
   
    <span data-ttu-id="ca5af-175">a.</span><span class="sxs-lookup"><span data-stu-id="ca5af-175">a.</span></span> <span data-ttu-id="ca5af-176">Ez az egy Windows Áruházbeli alkalmazással, amely bejelentési értesítések fogadása a webjobs-feladat a mobil-háttéralkalmazást részeként fut, és megjeleníti azt.</span><span class="sxs-lookup"><span data-stu-id="ca5af-176">This is a Windows Store application which will receive toast notifications from the WebJob running as part of your Mobile backend and display it.</span></span> <span data-ttu-id="ca5af-177">Ez a alapul [Notification Hubs - oktatóanyag univerzális Windows-].</span><span class="sxs-lookup"><span data-stu-id="ca5af-177">This is based on [Notification Hubs - Windows Universal tutorial].</span></span>  
   
    <span data-ttu-id="ca5af-178">b.</span><span class="sxs-lookup"><span data-stu-id="ca5af-178">b.</span></span> <span data-ttu-id="ca5af-179">Győződjön meg arról, hogy az alkalmazás engedélyezve van-e bejelentési értesítéseket fogadni.</span><span class="sxs-lookup"><span data-stu-id="ca5af-179">Ensure that your application is enabled to receive toast notifications.</span></span>
   
    <span data-ttu-id="ca5af-180">c.</span><span class="sxs-lookup"><span data-stu-id="ca5af-180">c.</span></span> <span data-ttu-id="ca5af-181">Győződjön meg arról, hogy a következő Notification Hubs regisztrációs kód lett meghívva, az alkalmazás elindításához (miután kicserélte a *HubName* és *DefaultListenSharedAccessSignature*:</span><span class="sxs-lookup"><span data-stu-id="ca5af-181">Ensure that the following Notification Hubs registration code is being called at the App start up (after replacing the *HubName* and *DefaultListenSharedAccessSignature*:</span></span>
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a><span data-ttu-id="ca5af-182">A minta fut:</span><span class="sxs-lookup"><span data-stu-id="ca5af-182">Running sample:</span></span>
1. <span data-ttu-id="ca5af-183">Győződjön meg arról, hogy a webjobs-feladat sikeresen fut, és a "Folyamatos Futtatás" ütemezett.</span><span class="sxs-lookup"><span data-stu-id="ca5af-183">Ensure that your WebJob is running successfully and scheduled to "Run Continuously".</span></span>
2. <span data-ttu-id="ca5af-184">Futtassa a **EnterprisePushMobileApp** amely indul el a Windows áruház egy alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="ca5af-184">Run the **EnterprisePushMobileApp** which will start the Windows Store app.</span></span>
3. <span data-ttu-id="ca5af-185">Futtassa a **EnterprisePushBackendSystem** konzolalkalmazást, amely szimulálni fogja a LoB háttér és küldésének megkezdése üzeneteket, és a következő megjelenő bejelentési értesítést kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="ca5af-185">Run the **EnterprisePushBackendSystem** console application which will simulate the LoB backend and will start sending messages and you should see toast notifications appearing like the following:</span></span>
   
    ![][5]
4. <span data-ttu-id="ca5af-186">Service Bus-témaköröktől, amely a Service Bus-előfizetések műveleteit a webes projekt által figyelt eredetileg elküldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="ca5af-186">The messages were originally sent to Service Bus topics which was being monitored by Service Bus subscriptions in your Web Job.</span></span> <span data-ttu-id="ca5af-187">Üzenet érkezett, miután értesítést létrejött, de a mobilalkalmazás küldött.</span><span class="sxs-lookup"><span data-stu-id="ca5af-187">Once a message was received, a notification was created and sent to the mobile app.</span></span> <span data-ttu-id="ca5af-188">A webjobs-feladat bejegyzéseit, amelyek a naplók hivatkozásra lépve, győződjön meg arról, hogy a feldolgozási keresztül meggyőződhet [klasszikus Azure portál] a webes projekt:</span><span class="sxs-lookup"><span data-stu-id="ca5af-188">You can look through the WebJob logs to confirm the processing when you go to the Logs link in [Azure Classic Portal] for your Web Job:</span></span>
   
    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
<span data-ttu-id="ca5af-189">[Értesítési központ minták]: https://github.com/Azure/azure-notificationhubs-samples</span><span class="sxs-lookup"><span data-stu-id="ca5af-189">[Notification Hub Samples]: https://github.com/Azure/azure-notificationhubs-samples</span></span>
<span data-ttu-id="ca5af-190">[Azure-Mobilszolgáltatásból]: http://azure.microsoft.com/documentation/services/mobile-services/</span><span class="sxs-lookup"><span data-stu-id="ca5af-190">[Azure Mobile Service]: http://azure.microsoft.com/documentation/services/mobile-services/</span></span>
<span data-ttu-id="ca5af-191">[Az Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/</span><span class="sxs-lookup"><span data-stu-id="ca5af-191">[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/</span></span>
<span data-ttu-id="ca5af-192">[Service Bus Pub/Sub programozás]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/</span><span class="sxs-lookup"><span data-stu-id="ca5af-192">[Service Bus Pub/Sub programming]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/</span></span>
<span data-ttu-id="ca5af-193">[Azure webjobs-feladat]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/</span><span class="sxs-lookup"><span data-stu-id="ca5af-193">[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/</span></span>
<span data-ttu-id="ca5af-194">[A Notification Hubs - oktatóanyag univerzális Windows]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/</span><span class="sxs-lookup"><span data-stu-id="ca5af-194">[Notification Hubs - Windows Universal tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/</span></span>
<span data-ttu-id="ca5af-195">[Klasszikus Azure portál]: https://manage.windowsazure.com/</span><span class="sxs-lookup"><span data-stu-id="ca5af-195">[Azure Classic Portal]: https://manage.windowsazure.com/</span></span>
