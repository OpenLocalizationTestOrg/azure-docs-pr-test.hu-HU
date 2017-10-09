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
# <a name="enterprise-push-architectural-guidance"></a>Útmutató a vállalati leküldési architektúrákhoz
A vállalatok ma egyre fokozatosan áthelyezése vagy hello alkalmazottak (belső) létrehozásához vagy az alkalmazásokat a végfelhasználók számára (külső) felé. Létező háttérrendszerek helyen kell azt Nagyszámítógépek rendelkeznek, vagy bizonyos LoB-alkalmazások, amelyek kell integrálni hello mobilalkalmazás architektúra. Ez az útmutató előadás a legjobb módja toodo ezt az integrációt ajánló lehetséges megoldás toocommon forgatókönyvek.

Gyakori követelmény van a leküldéses értesítési toohello felhasználókon a mobilalkalmazás hello háttérrendszerek az egyik fontos esemény bekövetkezésekor. Például az iPhone hello banki banki alkalmazás rendelkező banki ügyfél szeretne toobe értesíti, ha tartozik történik egy saját fiókot vagy egy intranetes forgatókönyvet, ahol a pénzügyi részleg egy alkalmazott, a költségvetési jóváhagyást alkalmazások a Windows Phone rendelkező szeretne toobe bizonyos összeg felett értesíti, ha a jóváhagyási kérelem kapjuk.

hello banki vagy jóváhagyási feldolgozási valószínűleg toobe néhány háttér rendszert, amely egy leküldéses toohello felhasználónak kell kezdeményeznie végezheti el. Előfordulhat, hogy több ilyen háttér kell az összes build rendszereket hello logika tooimplement leküldéses ugyanolyan típusú, ha az eseményt akkor váltja ki egy értesítést. Itt hello összetettsége létrejönnie egy egyetlen leküldéses rendszert, ahol hello végfelhasználók előfordulhat, hogy előfizetett toodifferent értesítéseket, és lehet, hogy van még lehet például az intranet mobilalkalmazások hello esetben több mobilalkalmazások több háttérrendszerek integrálása egy mobilalkalmazás azt szeretné, ha több ilyen háttérrendszerek tooreceive értesítéseket. hello háttérrendszerek nem ismeri vagy kell tooknow leküldéses szemantikáját/technológia, így a közös megoldást-ra volt toointroduce kérdezze le az hello háttérrendszerek bármely események számára, és leküldéses köszönőüzenetei küldéséért felelős összetevő toohello ügyfél.
Itt előadás egy használó Azure Service Bus - témakört/előfizetést modell, amely csökkenti a hello összetettsége hello megoldás méretezhető tétele közben is jobb megoldás.

Ez hello általános architektúra hello megoldás (több mobilalkalmazásokkal általános, de egyaránt alkalmazható, ha csak egy mobilalkalmazás)

## <a name="architecture"></a>Architektúra
![][1]

a architekturális diagramja hello kulcs darab Azure Service Bus, amely a témakörök/előfizetések programozási modellt biztosít (további sablonokat a [Service Bus Pub/Sub programozási]). hello fogadó, amely ebben az esetben hello mobil-háttéralkalmazást (általában [Azure Mobile Services mobilszolgáltatás], amely indít el egy leküldéses toohello mobilalkalmazások) kapott üzenetek közvetlenül hello háttérrendszerek, de ehelyett tudunk egy köztes hardverabsztrakciós réteg által biztosított [Azure Service Bus] lehetővé teszi a mobil-háttéralkalmazást tooreceive üzeneteit egy vagy több háttérrendszerek. A Service Bus-témakörbe létre minden egyes hello háttér rendszerek pl. fiók HR, pénzügyi, amelyek alapvetően "témakörök" iránt, amelyek fogja elindítani leküldéses értesítéseket küldött üzenetek toobe toobe kell. hello háttérrendszerek üzenetet szeretne küldeni toothese témaköröket. A mobil-háttéralkalmazás fizethet tooone vagy több ilyen témakörök által a Service Bus-előfizetés létrehozása. Ez feljogosítja hello mobil-háttéralkalmazást tooreceive hello megfelelő háttérrendszer értesítéseinek. Mobil-háttéralkalmazást üzenetek toolisten folytatja az előfizetések és, amint egy üzenet érkezik, akkor kapcsolja vissza, és elküldi azt értesítési tooits értesítési központot. A Notification hubs felé kézbesíti hello üzenet toohello mobilalkalmazás. Ezért toosummarize hello legfontosabb összetevők, vezetünk be:

1. A háttérrendszerek (LoB/örökölt rendszerek)
   * Service Bus-témakörbe létrehozása
   * Üzenet küldése
2. Mobil háttérszolgáltatás
   * Szolgáltatás-előfizetést hoz létre
   * Üzenet fogadása (a háttérrendszer)
   * Elküldi az értesítést tooclients (az Azure Notification Hub) keresztül
3. Mobilalkalmazás
   * Kap, és értesítést jelenít meg

### <a name="benefits"></a>Előnyei:
1. hello szétválasztását hello fogadó (app/mobilszolgáltatás Notification Hub használatával) és a feladó (háttérrendszerek) lehetővé teszi, hogy további háttérrendszerek integrálva van a minimális módosítása.
2. Így több mobilalkalmazások éppen egy vagy több háttérrendszerek képes tooreceive eseményeinek hello a forgatókönyvet.  

## <a name="sample"></a>Minta:
### <a name="prerequisites"></a>Előfeltételek
El kell végeznie az alábbi oktatóanyagok toofamiliarize hello fogalmait, valamint a gyakori létrehozása a & konfigurációs lépések hello:

1. [Service Bus Pub/Sub programozási] -ez magyarázattal hello részleteit működő Service Bus témakörök/előfizetések, hogyan toocreate névtér toocontain témakörök/előfizetések, hogyan toosend & üzeneteket fogadjon.
2. [A Notification Hubs - oktatóanyag univerzális Windows-] -ismerteti, hogyan tooset be a Windows Áruházbeli alkalmazások és a Notification Hubs tooregister használja, és majd értesítéseket.

### <a name="sample-code"></a>Mintakód
hello teljes mintakód érhető el: [Notification Hub minták]. A oszlik három összetevővel:

1. **EnterprisePushBackendSystem**
   
    a. A projekt által használt hello *WindowsAzure.ServiceBus* Nuget-csomag alapozva [Service Bus Pub/Sub programozási].
   
    b. Ez az egy egyszerű C# konzol app toosimulate egy LoB rendszer kezdeményező hello üzenet toobe toohello mobilalkalmazás kézbesíteni.
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create hello topic where we will send notifications
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    c. `CreateTopic`Ha az üzeneteket kapni fog van használt toocreate hello Service Bus-témakörbe.
   
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
   
    d. `SendMessage`használt toosend hello üzenetek toothis Service Bus-témakörbe. Itt azt egyszerűen küldenek véletlenszerű üzenetek toohello témakör rendszeresen hello célra hello minta készlete. Általában nem lesznek egy háttér-rendszert, amely üzenetet szeretne küldeni, ha az esemény akkor következik be.
   
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
2. **ReceiveAndSendNotification**
   
    a. A projekt által használt hello *WindowsAzure.ServiceBus* és *Microsoft.Web.WebJobs.Publish* Nuget a csomagok és alapuló [Service Bus Pub/Sub programozási].
   
    b. Ez az egy másik C# konzolalkalmazást, amely jelenleg fut egy [Azure webjobs-feladat] mert toorun folyamatosan rendelkezik a toolisten hello LoB-/ háttérrendszerek érkező üzenetek-e. Ez a mobil-háttéralkalmazást része lesz.
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create hello subscription which will receive messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    c. `CreateSubscription`van használt toocreate hello a témakör a Service Bus előfizetést ahol hello háttérrendszer fog üzeneteket küldeni. Attól függően, hogy hello üzleti helyzethez Ez az összetevő hoz létre egy vagy több előfizetés (pl. néhány lehetséges, hogy lehet üzenetek fogadása HR rendszer, egyes pénzügyi rendszer, és így tovább) toocorresponding kapcsolatos témakörök
   
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
   
    d. ReceiveMessageAndSendNotification hello témakör segítségével az általa használt tooread hello üzenetét, és ha olvasható hello sikeres majd kézműipari (a hello mintaforgatókönyv Windows natív bejelentési értesítés) értesítési küldött toobe toohello mobil az alkalmazás Azure Notification Hubs használatával.
   
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
   
    e. Közzététel ezt egy **webjobs-feladat**, kattintson jobb gombbal a hello megoldást a Visual Studióban, és válassza ki **webjobs-feladat közzététel**
   
    ![][2]
   
    f. Válassza ki a közzétételi profilt, és hozzon létre egy új Azure-webhely, ha még nem létezik már fogja tárolni, amely a webjobs-feladat, és ha hello webhely majd **közzététel**.
   
    ![][3]
   
    g. Hello feladat "Folyamatos Futtatás" toobe konfigurálja, hogy mikor jelentkezik be a toohello [klasszikus Azure portál] hello következő hasonlót kell megjelennie:
   
    ![][4]
3. **EnterprisePushMobileApp**
   
    a. Ez az egy Windows Áruházbeli alkalmazással, amely bejelentési értesítéseket fogadni a mobil-háttéralkalmazást részeként hello webjobs-feladat futását, és megjeleníti azt. Ez a alapul [A Notification Hubs - oktatóanyag univerzális Windows-].  
   
    b. Győződjön meg arról, hogy az alkalmazás engedélyezett tooreceive bejelentési értesítést.
   
    c. Győződjön meg arról, hogy a következő kódot a Notification Hubs regisztrációs hello: hello App elindításához lett meghívva (hello cseréje után *HubName* és *DefaultListenSharedAccessSignature*:
   
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

### <a name="running-sample"></a>A minta fut:
1. Győződjön meg arról, hogy a webjobs-feladat sikeresen fut, valamint ütemezett, túl "folyamatos Futtatás".
2. Futtassa a hello **EnterprisePushMobileApp** amely hello Windows Áruházbeli alkalmazás elindul.
3. Futtassa a hello **EnterprisePushBackendSystem** konzolalkalmazást, amely szimulálni fogja hello LoB háttér és küldésének megkezdése üzeneteket, és szereplő hello hasonló bejelentési értesítést kell megjelennie:
   
    ![][5]
4. hello eredetileg üzeneteket tooService Bus-témaköröktől, amely a Service Bus-előfizetések műveleteit a webes projekt által figyelt. Amennyiben az üzenet érkezett, egy értesítés létrejött, de toohello mobilalkalmazás küldött. A webjobs-feladat naplózza tooconfirm hello feldolgozása naplók hivatkozásra toohello váltáskor hello meggyőződhet [klasszikus Azure portál] a webes projekt:
   
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
