



<span data-ttu-id="7aae3-101">Csak meg kell adnia a tulajdonságait sablon értesítéseket küld, amikor a mi esetünkben küldünk a tulajdonságkészletbe honosított verziója az aktuális híreket tartalmazó példány:</span><span class="sxs-lookup"><span data-stu-id="7aae3-101">When you send template notifications you only need to provide a set of properties, in our case we will send the set of properties containing the localized version of the current news, for instance:</span></span>

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


<span data-ttu-id="7aae3-102">Ez a szakasz bemutatja, hogyan küldhetők értesítések egy konzolalkalmazás használatával</span><span class="sxs-lookup"><span data-stu-id="7aae3-102">This section shows how to send notifications using a console app</span></span>

<span data-ttu-id="7aae3-103">A befoglalt kódot közzéteszi Windows áruház és az iOS-eszközökre, mivel a háttérkiszolgálón is szórási bármely, a támogatott eszközök.</span><span class="sxs-lookup"><span data-stu-id="7aae3-103">The included code broadcasts to both Windows Store and iOS devices, since the backend can broadcast to any of the supported devices.</span></span>

### <a name="to-send-notifications-using-a-c-console-app"></a><span data-ttu-id="7aae3-104">Egy C# Konzolalkalmazás használatával az értesítések küldéséhez</span><span class="sxs-lookup"><span data-stu-id="7aae3-104">To send notifications using a C# console app</span></span>
<span data-ttu-id="7aae3-105">Módosítsa a `SendTemplateNotificationAsync` módszer a korábban létrehozott kódot a következő konzol alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7aae3-105">Modify the `SendTemplateNotificationAsync` method in the console app you previously created with the following code.</span></span> <span data-ttu-id="7aae3-106">Figyelje meg, hogyan ebben az esetben nincs szükség a különböző területi beállításokhoz és platformokra több értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="7aae3-106">Notice how in this case there is no need to send multiple notifications for different locales and platforms.</span></span>

        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");

            // Sending the notification as a template notification. All template registrations that contain 
            // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
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


<span data-ttu-id="7aae3-107">Vegye figyelembe, hogy a egyszerű hívás fog továbbítani az honosított adat híreket, **összes** az eszközök, függetlenül a platform, az értesítési központ hoz létre, és kézbesíti az összes eszközt egy adott előfizetni a megfelelő natív forgalma címke.</span><span class="sxs-lookup"><span data-stu-id="7aae3-107">Note that this simple call will deliver the localized piece of news to **all** your devices, irrespective of the platform, as your Notification Hub builds and delivers the correct native payload to all the devices subscribed to a specific tag.</span></span>

### <a name="sending-the-notification-with-mobile-services"></a><span data-ttu-id="7aae3-108">A Mobile Services értesítés küldése</span><span class="sxs-lookup"><span data-stu-id="7aae3-108">Sending the notification with Mobile Services</span></span>
<span data-ttu-id="7aae3-109">A Mobile Service ütemező a következő parancsfájlt használhatja:</span><span class="sxs-lookup"><span data-stu-id="7aae3-109">In your Mobile Service scheduler, you can use the following script:</span></span>

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


