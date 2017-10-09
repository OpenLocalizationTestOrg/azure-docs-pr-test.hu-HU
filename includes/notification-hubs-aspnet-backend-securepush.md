## <a name="webapi-project"></a>WebAPI projekt
1. A Visual Studióban nyissa meg a hello **AppBackend** hello létrehozott projekt **felhasználók értesítése** oktatóanyag.
2. A Notifications.cs, teljes név felülírandó hello **értesítések** a következő kód hello osztályt. Lehet, hogy tooreplace hello helyőrzőt a kapcsolati karakterlánc (teljes hozzáférés) az értesítési központot, és hello központ nevét. Ezt úgy szerezheti be ezeket az értékeket a hello [klasszikus Azure portál](http://manage.windowsazure.com). Ez a modul most hello különböző biztonságos értesítések küldendő jelöli. Egy teljes megvalósításában hello értesítések tárolódnak adatbázis; Az egyszerűség kedvéért ebben az esetben tároljuk őket a memóriában.
   
        public class Notification
        {
            public int Id { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications
        {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",     "{hub name}");
            }

            public Notification CreateNotification(string payload)
            {
                var notification = new Notification() {
                Id = notifications.Count,
                Payload = payload,
                Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Notification ReadNotification(int id)
            {
                return notifications.ElementAt(id);
            }
        }

1. NotificationsController.cs, cserélje le hello hello kódot **NotificationsController** definition hello a következő kódot az osztály. Ez az összetevő úgy hello eszköz tooretrieve hello értesítési biztonságosan megvalósítja és is tartalmaz egy módja (a jelen oktatóanyag céljából hello) tootrigger egy biztonságos leküldéses tooyour eszközök. Vegye figyelembe, hogy hello értesítésiközpont toohello értesítést küld, amikor csak értesítést fog kapni, a nyers hello azonosítójú hello értesítési (és nem tényleges üzenet):
   
       public NotificationsController()
       {
           Notifications.Instance.CreateNotification("This is a secure notification!");
       }
   
       // GET api/notifications/id
       public Notification Get(int id)
       {
           return Notifications.Instance.ReadNotification(id);
       }
   
       public async Task<HttpResponseMessage> Post()
       {
           var secureNotificationInTheBackend = Notifications.Instance.CreateNotification("Secure confirmation.");
           var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
           // windows
           var rawNotificationToBeSent = new Microsoft.Azure.NotificationHubs.WindowsNotification(secureNotificationInTheBackend.Id.ToString(),
                           new Dictionary<string, string> {
                               {"X-WNS-Type", "wns/raw"}
                           });
           await Notifications.Instance.Hub.SendNotificationAsync(rawNotificationToBeSent, usernameTag);
   
           // apns
           await Notifications.Instance.Hub.SendAppleNativeNotificationAsync("{\"aps\": {\"content-available\": 1}, \"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}", usernameTag);
   
           // gcm
           await Notifications.Instance.Hub.SendGcmNativeNotificationAsync("{\"data\": {\"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}}", usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }


Vegye figyelembe, hogy hello `Post` metódus most nem küld egy bejelentési értesítést. Csak a hello értesítés azonosítója és a nem bizalmas tartalmat tartalmazó nyers értesítést küld. Továbbá győződjön meg arról, hogy toocomment hello küldési művelethez hello platformokhoz, amelynek nincs konfigurálva az értesítési központ, a hitelesítő adatokat, azok hibákat eredményez.

1. Most fogjuk újra üzembe helyezni az alkalmazás tooan Azure-webhely rendelés toomake azt minden eszköz érhető el. Kattintson a jobb gombbal a hello **AppBackend** projektre, és válassza ki **közzététel**.
2. A közzétételi célként válassza az Azure webhelyén. Jelentkezzen be az Azure-fiókjával, és válassza ki a meglévő vagy új webhely létrehozása, és jegyezze fel a hello **URL-címre** hello tulajdonság **kapcsolat** fülre. Toothis URL-CÍMÉRE hivatkozik a *háttér végpont* ebben az oktatóanyagban később. Kattintson a **Publish** (Közzététel) gombra.

