



<span data-ttu-id="8fef7-101">Csak akkor kell tooprovide tulajdonságait sablon értesítéseket küld, amikor a mi esetünkben küldünk hello tulajdonságkészletbe hello hello aktuális híreket, honosított verzióját tartalmazó példány:</span><span class="sxs-lookup"><span data-stu-id="8fef7-101">When you send template notifications you only need tooprovide a set of properties, in our case we will send hello set of properties containing hello localized version of hello current news, for instance:</span></span>

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


<span data-ttu-id="8fef7-102">Ez a szakasz bemutatja, hogyan toosend értesítések egy konzolalkalmazás használatával</span><span class="sxs-lookup"><span data-stu-id="8fef7-102">This section shows how toosend notifications using a console app</span></span>

<span data-ttu-id="8fef7-103">mivel hello háttér terjesztéshez tooany hello támogatott eszközök hello kód szórás tooboth Windows áruház és az iOS-eszközök esetén tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8fef7-103">hello included code broadcasts tooboth Windows Store and iOS devices, since hello backend can broadcast tooany of hello supported devices.</span></span>

### <a name="toosend-notifications-using-a-c-console-app"></a><span data-ttu-id="8fef7-104">toosend értesítéseket egy C# Konzolalkalmazás</span><span class="sxs-lookup"><span data-stu-id="8fef7-104">toosend notifications using a C# console app</span></span>
<span data-ttu-id="8fef7-105">Módosítsa a hello `SendTemplateNotificationAsync` hello a következő kódot a korábban létrehozott hello Konzolalkalmazás metódust.</span><span class="sxs-lookup"><span data-stu-id="8fef7-105">Modify hello `SendTemplateNotificationAsync` method in hello console app you previously created with hello following code.</span></span> <span data-ttu-id="8fef7-106">Figyelje meg, hogyan ebben az esetben nincs nincs szükség toosend különböző területi beállításokhoz és platformok több értesítés is érkezett.</span><span class="sxs-lookup"><span data-stu-id="8fef7-106">Notice how in this case there is no need toosend multiple notifications for different locales and platforms.</span></span>

        private static async void SendTemplateNotificationAsync()
        {
            // Define hello notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");

            // Sending hello notification as a template notification. All template registrations that contain 
            // "messageParam" or "News_<local selected>" and hello proper tags will receive hello notifications. 
            // This includes APNS, GCM, WNS, and MPNS template registrations.
            Dictionary<string, string> templateParams = new Dictionary<string, string>();

            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
            var locales = new string[] { "English", "French", "Mandarin" };

            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";

                // Sending localized News for each tag too...
                foreach( var locale in locales)
                {
                    string key = "News_" + locale;

                    // Your real localized news content would go here.
                    templateParams[key] = "Breaking " + category + " News in " + locale + "!";
                }

                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
        }


<span data-ttu-id="8fef7-107">Vegye figyelembe, hogy a egyszerű hívás funkció hírek hello honosított adat túl**összes** hello platform, függetlenül az eszközök az értesítési központ hoz létre, és kézbesíti hello megfelelő natív hasznos tooall hello eszközök előfizetve tooa adott címke.</span><span class="sxs-lookup"><span data-stu-id="8fef7-107">Note that this simple call will deliver hello localized piece of news too**all** your devices, irrespective of hello platform, as your Notification Hub builds and delivers hello correct native payload tooall hello devices subscribed tooa specific tag.</span></span>

### <a name="sending-hello-notification-with-mobile-services"></a><span data-ttu-id="8fef7-108">A Mobile Services hello értesítés küldése</span><span class="sxs-lookup"><span data-stu-id="8fef7-108">Sending hello notification with Mobile Services</span></span>
<span data-ttu-id="8fef7-109">A Mobile Service ütemező a következő parancsfájl hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="8fef7-109">In your Mobile Service scheduler, you can use hello following script:</span></span>

    var azure = require('azure');
    var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
    var notification = {
            "News_English": "World News in English!",
            "News_French": "World News in French!",
            "News_Mandarin", "World News in Mandarin!"
    }
    notificationHubService.send('World', notification, function(error) {
        if (!error) {
            console.warn("Notification successful");
        }
    });


