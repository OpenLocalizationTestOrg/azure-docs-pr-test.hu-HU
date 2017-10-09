
<span data-ttu-id="c582b-101">Ez a szakasz bemutatja, hogyan toosend legfrissebb hírek, címkézett sablon értesítések .NET-konzolalkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="c582b-101">This section shows how toosend breaking news as tagged template notifications from a .NET console app.</span></span>

<span data-ttu-id="c582b-102">Mobile Apps használata tekintse meg a toohello [leküldéses értesítések hozzáadása Mobile Apps] oktatóanyagot, és válassza ki a platformot hello tetején.</span><span class="sxs-lookup"><span data-stu-id="c582b-102">If you are using Mobile Apps please refer toohello [Add push notifications for Mobile Apps] tutorial and select your platform at hello top.</span></span>

<span data-ttu-id="c582b-103">Ha azt szeretné, hogy toouse Java vagy PHP tekintse meg a túl[hogyan toouse Notification Hubs Java/php-ből].</span><span class="sxs-lookup"><span data-stu-id="c582b-103">If you want toouse Java or PHP refer too[How toouse Notification Hubs from Java/PHP].</span></span> <span data-ttu-id="c582b-104">Bármely háttér használatával értesítéseket küldhet a [Notification hub REST-felület].</span><span class="sxs-lookup"><span data-stu-id="c582b-104">You can send notifications from any backend using the [Notification Hubs REST interface].</span></span>

<span data-ttu-id="c582b-105">Hagyja ki a 1-3 Ha hello Konzolalkalmazás értesítések küldésének befejezése létrehozott [Ismerkedés a Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="c582b-105">Skip steps 1-3 if you created hello console app for sending notifications when you completed [Get started with Notification Hubs].</span></span>

1. <span data-ttu-id="c582b-106">A Visual Studióban hozzon létre egy új Visual C#-konzolalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="c582b-106">In Visual Studio create a new Visual C# console application:</span></span>
   
       ![][13]
2. <span data-ttu-id="c582b-107">A hello Visual Studio fő kattintson **eszközök**, **Kódtárcsomag-kezelő**, és **Csomagkezelő konzol**, hello console ablakban írja be a következőt, majd nyomja le az **Meg**:</span><span class="sxs-lookup"><span data-stu-id="c582b-107">In hello Visual Studio main menu, click **Tools**, **Library Package Manager**, and **Package Manager Console**, then in hello console window type the  following and press **Enter**:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="c582b-108">Ezzel hozzáad egy hivatkozást toohello Azure Notification Hubs SDK használatával hello [Microsoft.Azure.Notification Hubs NuGet-csomag].</span><span class="sxs-lookup"><span data-stu-id="c582b-108">This adds a reference toohello Azure Notification Hubs SDK using hello [Microsoft.Azure.Notification Hubs NuGet package].</span></span>
3. <span data-ttu-id="c582b-109">Nyissa meg a Program.cs hello fájlt, és adja hozzá a következő hello `using` utasítást:</span><span class="sxs-lookup"><span data-stu-id="c582b-109">Open hello file Program.cs and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
4. <span data-ttu-id="c582b-110">A hello `Program` osztály, adja hozzá a következő metódus hello, vagy cserélje le, ha már létezik:</span><span class="sxs-lookup"><span data-stu-id="c582b-110">In hello `Program` class, add hello following method, or replace it if it already exists:</span></span>
   
        private static async void SendTemplateNotificationAsync()
        {
            // Define hello notification hub.
            NotificationHubClient hub =
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");
   
            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business",
                                            "Technology", "Science", "Sports"};
   
            // Sending hello notification as a template notification. All template registrations that contain
            // "messageParam" and hello proper tags will receive hello notifications.
            // This includes APNS, GCM, WNS, and MPNS template registrations.
   
            Dictionary<string, string> templateParams = new Dictionary<string, string>();
   
            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";
                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
         }
   
    <span data-ttu-id="c582b-111">Ez a kód sablon értesítést küld az egyes hello hat címkék hello karakterlánc-tömbben.</span><span class="sxs-lookup"><span data-stu-id="c582b-111">This code sends a template notification for each of hello six tags in hello string array.</span></span> <span data-ttu-id="c582b-112">hello címkék teszi, hogy a eszközök csak regisztrált hello kategóriák értesítéseket kapjanak.</span><span class="sxs-lookup"><span data-stu-id="c582b-112">hello use of tags makes sure that devices receive notifications only for hello registered categories.</span></span>
5. <span data-ttu-id="c582b-113">A fenti kódot hello, cserélje le a hello `<hub name>` és `<connection string with full access>` helyőrzőket az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultFullSharedAccessSignature* hello irányítópultról az értesítési központ .</span><span class="sxs-lookup"><span data-stu-id="c582b-113">In hello above code, replace hello `<hub name>` and `<connection string with full access>` placeholders with your notification hub name and hello connection  string for *DefaultFullSharedAccessSignature* from hello dashboard of your notification hub.</span></span>
6. <span data-ttu-id="c582b-114">Adja hozzá a következő sorokat hello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="c582b-114">Add hello following lines in hello **Main** method:</span></span>
   
         SendTemplateNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="c582b-115">Hello konzol alkalmazás elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="c582b-115">Build hello console app.</span></span>

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Ismerkedés a Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Notification hub REST-felület]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[leküldéses értesítések hozzáadása Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[hogyan toouse Notification Hubs Java/php-ből]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[Microsoft.Azure.Notification Hubs NuGet-csomag]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
