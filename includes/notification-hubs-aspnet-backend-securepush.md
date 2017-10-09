## <a name="webapi-project"></a><span data-ttu-id="f1670-101">WebAPI projekt</span><span class="sxs-lookup"><span data-stu-id="f1670-101">WebAPI Project</span></span>
1. <span data-ttu-id="f1670-102">A Visual Studióban nyissa meg a hello **AppBackend** hello létrehozott projekt **felhasználók értesítése** oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="f1670-102">In Visual Studio, open hello **AppBackend** project that you created in hello **Notify Users** tutorial.</span></span>
2. <span data-ttu-id="f1670-103">A Notifications.cs, teljes név felülírandó hello **értesítések** a következő kód hello osztályt.</span><span class="sxs-lookup"><span data-stu-id="f1670-103">In Notifications.cs, replace hello whole **Notifications** class with hello following code.</span></span> <span data-ttu-id="f1670-104">Lehet, hogy tooreplace hello helyőrzőt a kapcsolati karakterlánc (teljes hozzáférés) az értesítési központot, és hello központ nevét.</span><span class="sxs-lookup"><span data-stu-id="f1670-104">Be sure tooreplace hello placeholders with your connection string (with full access) for your notification hub, and hello hub name.</span></span> <span data-ttu-id="f1670-105">Ezt úgy szerezheti be ezeket az értékeket a hello [klasszikus Azure portál](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="f1670-105">You can obtain these values from hello [Azure Classic Portal](http://manage.windowsazure.com).</span></span> <span data-ttu-id="f1670-106">Ez a modul most hello különböző biztonságos értesítések küldendő jelöli.</span><span class="sxs-lookup"><span data-stu-id="f1670-106">This module now represents hello different secure notifications that will be sent.</span></span> <span data-ttu-id="f1670-107">Egy teljes megvalósításában hello értesítések tárolódnak adatbázis; Az egyszerűség kedvéért ebben az esetben tároljuk őket a memóriában.</span><span class="sxs-lookup"><span data-stu-id="f1670-107">In a complete implementation, hello notifications will be stored in a database; for simplicity, in this case we store them in memory.</span></span>
   
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

1. <span data-ttu-id="f1670-108">NotificationsController.cs, cserélje le hello hello kódot **NotificationsController** definition hello a következő kódot az osztály.</span><span class="sxs-lookup"><span data-stu-id="f1670-108">In NotificationsController.cs, replace hello code inside hello **NotificationsController** class definition with hello following code.</span></span> <span data-ttu-id="f1670-109">Ez az összetevő úgy hello eszköz tooretrieve hello értesítési biztonságosan megvalósítja és is tartalmaz egy módja (a jelen oktatóanyag céljából hello) tootrigger egy biztonságos leküldéses tooyour eszközök.</span><span class="sxs-lookup"><span data-stu-id="f1670-109">This component implements a way for hello device tooretrieve hello notification securely, and also provides a way (for hello purposes of this tutorial) tootrigger a secure push tooyour devices.</span></span> <span data-ttu-id="f1670-110">Vegye figyelembe, hogy hello értesítésiközpont toohello értesítést küld, amikor csak értesítést fog kapni, a nyers hello azonosítójú hello értesítési (és nem tényleges üzenet):</span><span class="sxs-lookup"><span data-stu-id="f1670-110">Note that when sending hello notification toohello notification hub, we only send a raw notification with hello ID of hello notification (and no actual message):</span></span>
   
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


<span data-ttu-id="f1670-111">Vegye figyelembe, hogy hello `Post` metódus most nem küld egy bejelentési értesítést.</span><span class="sxs-lookup"><span data-stu-id="f1670-111">Note that hello `Post` method now does not send a toast notification.</span></span> <span data-ttu-id="f1670-112">Csak a hello értesítés azonosítója és a nem bizalmas tartalmat tartalmazó nyers értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="f1670-112">It sends a raw notification that contains only hello notification ID, and not any sensitive content.</span></span> <span data-ttu-id="f1670-113">Továbbá győződjön meg arról, hogy toocomment hello küldési művelethez hello platformokhoz, amelynek nincs konfigurálva az értesítési központ, a hitelesítő adatokat, azok hibákat eredményez.</span><span class="sxs-lookup"><span data-stu-id="f1670-113">Also, make sure toocomment hello send operation for hello platforms for which you do not have credentials configured on your notification hub, as they will result in errors.</span></span>

1. <span data-ttu-id="f1670-114">Most fogjuk újra üzembe helyezni az alkalmazás tooan Azure-webhely rendelés toomake azt minden eszköz érhető el.</span><span class="sxs-lookup"><span data-stu-id="f1670-114">Now we will re-deploy this app tooan Azure Website in order toomake it accessible from all devices.</span></span> <span data-ttu-id="f1670-115">Kattintson a jobb gombbal a hello **AppBackend** projektre, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="f1670-115">Right-click on hello **AppBackend** project and select **Publish**.</span></span>
2. <span data-ttu-id="f1670-116">A közzétételi célként válassza az Azure webhelyén.</span><span class="sxs-lookup"><span data-stu-id="f1670-116">Select Azure Website as your publish target.</span></span> <span data-ttu-id="f1670-117">Jelentkezzen be az Azure-fiókjával, és válassza ki a meglévő vagy új webhely létrehozása, és jegyezze fel a hello **URL-címre** hello tulajdonság **kapcsolat** fülre. Toothis URL-CÍMÉRE hivatkozik a *háttér végpont* ebben az oktatóanyagban később.</span><span class="sxs-lookup"><span data-stu-id="f1670-117">Log in with your Azure account and select an existing or new Website, and make a note of hello **destination URL** property in hello **Connection** tab. We will refer toothis URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="f1670-118">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="f1670-118">Click **Publish**.</span></span>

