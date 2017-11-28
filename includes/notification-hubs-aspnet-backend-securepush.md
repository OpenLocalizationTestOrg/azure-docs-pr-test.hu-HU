## <a name="webapi-project"></a><span data-ttu-id="f377f-101">WebAPI projekt</span><span class="sxs-lookup"><span data-stu-id="f377f-101">WebAPI Project</span></span>
1. <span data-ttu-id="f377f-102">A Visual Studióban nyissa meg a **AppBackend** a projekt a **felhasználók értesítése** oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="f377f-102">In Visual Studio, open the **AppBackend** project that you created in the **Notify Users** tutorial.</span></span>
2. <span data-ttu-id="f377f-103">Notifications.cs, cserélje le a teljes **értesítések** osztály a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="f377f-103">In Notifications.cs, replace the whole **Notifications** class with the following code.</span></span> <span data-ttu-id="f377f-104">Ne felejtse el a helyőrzőket cserélje le a kapcsolati karakterlánc (teljes hozzáférés) az értesítési központot, és a központ nevét.</span><span class="sxs-lookup"><span data-stu-id="f377f-104">Be sure to replace the placeholders with your connection string (with full access) for your notification hub, and the hub name.</span></span> <span data-ttu-id="f377f-105">Ezt úgy szerezheti be ezeket az értékeket a [klasszikus Azure portál](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="f377f-105">You can obtain these values from the [Azure Classic Portal](http://manage.windowsazure.com).</span></span> <span data-ttu-id="f377f-106">Ez a modul most a különböző biztonságos értesítések küldendő jelöli.</span><span class="sxs-lookup"><span data-stu-id="f377f-106">This module now represents the different secure notifications that will be sent.</span></span> <span data-ttu-id="f377f-107">A megvalósítás az értesítések tárolódnak adatbázis; Az egyszerűség kedvéért ebben az esetben tároljuk őket a memóriában.</span><span class="sxs-lookup"><span data-stu-id="f377f-107">In a complete implementation, the notifications will be stored in a database; for simplicity, in this case we store them in memory.</span></span>
   
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

1. <span data-ttu-id="f377f-108">NotificationsController.cs, cserélje le a kódot a **NotificationsController** osztály definícióját a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="f377f-108">In NotificationsController.cs, replace the code inside the **NotificationsController** class definition with the following code.</span></span> <span data-ttu-id="f377f-109">Ez az összetevő valósítja meg az értesítés biztonságosan beolvasása az eszköz olyan módon, és is lehetővé teszi a (a jelen oktatóanyag céljából) való biztonságos leküldéses az eszközökön.</span><span class="sxs-lookup"><span data-stu-id="f377f-109">This component implements a way for the device to retrieve the notification securely, and also provides a way (for the purposes of this tutorial) to trigger a secure push to your devices.</span></span> <span data-ttu-id="f377f-110">Vegye figyelembe, hogy az értesítési központnak küldött, amikor csak értesítést fog kapni, a nyers azonosítójú az értesítés (és nem tényleges üzenet):</span><span class="sxs-lookup"><span data-stu-id="f377f-110">Note that when sending the notification to the notification hub, we only send a raw notification with the ID of the notification (and no actual message):</span></span>
   
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


<span data-ttu-id="f377f-111">Vegye figyelembe, hogy a `Post` metódus most nem küld egy bejelentési értesítést.</span><span class="sxs-lookup"><span data-stu-id="f377f-111">Note that the `Post` method now does not send a toast notification.</span></span> <span data-ttu-id="f377f-112">Az értesítés-azonosítója és a nem bizalmas tartalmat tartalmazó nyers értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="f377f-112">It sends a raw notification that contains only the notification ID, and not any sensitive content.</span></span> <span data-ttu-id="f377f-113">Emellett győződjön meg arról, a küldési művelet, amelynek nincs konfigurálva az értesítési központ, a hitelesítő adatokat, azok hibákat eredményez a platformok megtételére.</span><span class="sxs-lookup"><span data-stu-id="f377f-113">Also, make sure to comment the send operation for the platforms for which you do not have credentials configured on your notification hub, as they will result in errors.</span></span>

1. <span data-ttu-id="f377f-114">Most azt újra telepíti ezt a webalkalmazást az Azure-webhely annak érdekében, hogy minden eszköz érhető el.</span><span class="sxs-lookup"><span data-stu-id="f377f-114">Now we will re-deploy this app to an Azure Website in order to make it accessible from all devices.</span></span> <span data-ttu-id="f377f-115">Kattintson jobb gombbal az **AppBackend** projektre, és válassza a **Publish** (Közzététel) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f377f-115">Right-click on the **AppBackend** project and select **Publish**.</span></span>
2. <span data-ttu-id="f377f-116">A közzétételi célként válassza az Azure webhelyén.</span><span class="sxs-lookup"><span data-stu-id="f377f-116">Select Azure Website as your publish target.</span></span> <span data-ttu-id="f377f-117">Jelentkezzen be az Azure-fiókjával, és válassza ki a meglévő vagy új webhely létrehozása, és jegyezze fel a a **URL-címre** tulajdonságot a **kapcsolat** fülre.</span><span class="sxs-lookup"><span data-stu-id="f377f-117">Log in with your Azure account and select an existing or new Website, and make a note of the **destination URL** property in the **Connection** tab.</span></span> <span data-ttu-id="f377f-118">Az oktatóanyag további részében erre az URL-címre fogunk hivatkozni a *háttérrendszer végpontjaként*.</span><span class="sxs-lookup"><span data-stu-id="f377f-118">We will refer to this URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="f377f-119">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="f377f-119">Click **Publish**.</span></span>

