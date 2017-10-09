



Csak akkor kell tooprovide tulajdonságait sablon értesítéseket küld, amikor a mi esetünkben küldünk hello tulajdonságkészletbe hello hello aktuális híreket, honosított verzióját tartalmazó példány:

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


Ez a szakasz bemutatja, hogyan toosend értesítések egy konzolalkalmazás használatával

mivel hello háttér terjesztéshez tooany hello támogatott eszközök hello kód szórás tooboth Windows áruház és az iOS-eszközök esetén tartalmazza.

### <a name="toosend-notifications-using-a-c-console-app"></a>toosend értesítéseket egy C# Konzolalkalmazás
Módosítsa a hello `SendTemplateNotificationAsync` hello a következő kódot a korábban létrehozott hello Konzolalkalmazás metódust. Figyelje meg, hogyan ebben az esetben nincs nincs szükség toosend különböző területi beállításokhoz és platformok több értesítés is érkezett.

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


Vegye figyelembe, hogy a egyszerű hívás funkció hírek hello honosított adat túl**összes** hello platform, függetlenül az eszközök az értesítési központ hoz létre, és kézbesíti hello megfelelő natív hasznos tooall hello eszközök előfizetve tooa adott címke.

### <a name="sending-hello-notification-with-mobile-services"></a>A Mobile Services hello értesítés küldése
A Mobile Service ütemező a következő parancsfájl hello is használhatja:

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


