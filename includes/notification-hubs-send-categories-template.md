
Ez a szakasz bemutatja, hogyan toosend legfrissebb hírek, címkézett sablon értesítések .NET-konzolalkalmazásból.

Mobile Apps használata tekintse meg a toohello [leküldéses értesítések hozzáadása Mobile Apps] oktatóanyagot, és válassza ki a platformot hello tetején.

Ha azt szeretné, hogy toouse Java vagy PHP tekintse meg a túl[hogyan toouse Notification Hubs Java/php-ből]. Bármely háttér használatával értesítéseket küldhet a [Notification hub REST-felület].

Hagyja ki a 1-3 Ha hello Konzolalkalmazás értesítések küldésének befejezése létrehozott [Ismerkedés a Notification Hubs].

1. A Visual Studióban hozzon létre egy új Visual C#-konzolalkalmazást:
   
       ![][13]
2. A hello Visual Studio fő kattintson **eszközök**, **Kódtárcsomag-kezelő**, és **Csomagkezelő konzol**, hello console ablakban írja be a következőt, majd nyomja le az **Meg**:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Ezzel hozzáad egy hivatkozást toohello Azure Notification Hubs SDK használatával hello [Microsoft.Azure.Notification Hubs NuGet-csomag].
3. Nyissa meg a Program.cs hello fájlt, és adja hozzá a következő hello `using` utasítást:
   
        using Microsoft.Azure.NotificationHubs;
4. A hello `Program` osztály, adja hozzá a következő metódus hello, vagy cserélje le, ha már létezik:
   
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
   
    Ez a kód sablon értesítést küld az egyes hello hat címkék hello karakterlánc-tömbben. hello címkék teszi, hogy a eszközök csak regisztrált hello kategóriák értesítéseket kapjanak.
5. A fenti kódot hello, cserélje le a hello `<hub name>` és `<connection string with full access>` helyőrzőket az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultFullSharedAccessSignature* hello irányítópultról az értesítési központ .
6. Adja hozzá a következő sorokat hello hello **fő** módszert:
   
         SendTemplateNotificationAsync();
         Console.ReadLine();
7. Hello konzol alkalmazás elkészítésére.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Ismerkedés a Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Notification hub REST-felület]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[leküldéses értesítések hozzáadása Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[hogyan toouse Notification Hubs Java/php-ből]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[Microsoft.Azure.Notification Hubs NuGet-csomag]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
