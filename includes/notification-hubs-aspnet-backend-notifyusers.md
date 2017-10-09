## <a name="create-hello-webapi-project"></a>Hello WebAPI projekt létrehozása
Egy új ASP.NET WebAPI háttérrendszerből hello szakaszokban szereplő jön létre, és hogy fog rendelkezni a három fő célja:

1. **Ügyfelek hitelesítése**: egy üzenetkezelő hozzáadódik a későbbi tooauthenticate ügyfélkérelmek és társítása hello felhasználói hello kérelemmel.
2. **Ügyfél-értesítési regisztrációk**: később adhat egy tartományvezérlő toohandle új regisztrációk egy ügyfél eszköz tooreceive értesítéseket. hello hitelesített felhasználó nevét a rendszer automatikusan hozzáadja toohello eszközregisztrációs szerint egy [címke](https://msdn.microsoft.com/library/azure/dn530749.aspx).
3. **Értesítések tooClients küldése**: később is hozzáadhat egy vezérlő így az a felhasználó tootrigger egy biztonságos leküldéses toodevices tooprovide és hello címke társított ügyfelek. 

hello lépések bemutatják, hogyan toocreate hello új ASP.NET WebAPI háttérrendszerből: 

> [!IMPORTANT]
> Visual Studio 2015 használata vagy a korábban, az oktatóanyag elindítása előtt győződjön meg arról, hogy telepítette a NuGet-Csomagkezelő hello hello legújabb verzióját. toocheck, kezdő Visual Studio. A hello **eszközök** menüben kattintson a **bővítmények és frissítések**. Keresse meg **NuGet-Csomagkezelő** Visual Studio, és győződjön meg arról, hogy verziójának hello legújabb verziójával rendelkezik. Ha nem, távolítsa el, majd telepítse újra a NuGet-Csomagkezelő hello.
> 
> ![][B4]
> 
> [!NOTE]
> Győződjön meg arról, hogy telepítette a Visual Studio hello [Azure SDK](https://azure.microsoft.com/downloads/) webhely központi telepítéshez.
> 
> 

1. Indítsa el a Visual Studiót vagy a Visual Studio Expresst. Kattintson a **Server Explorer** , jelentkezzen be Azure-fiók tooyour. A Visual Studio kell toocreate hello webhely erőforrások fiókját a bejelentkezés.
2. A Visual Studióban kattintson **fájl**, majd kattintson a **új**, majd **projekt**, bontsa ki a **sablonok**, **Visual C#**, majd kattintson a **webes** és **ASP.NET Web Application**, hello típusnév **AppBackend**, és kattintson a **OK**. 
   
    ![][B1]
3. A hello **új ASP.NET projekt** párbeszédpanel, kattintson **Web API**, majd kattintson a **OK**.
   
    ![][B2]
4. A hello **konfigurálása a Microsoft Azure Web Apps** párbeszédpanelen válasszon egy előfizetést, és egy **App Service-csomag** már létrehozott. Másik lehetőségként **hozzon létre egy új app service-csomag** , és hozzon létre egy hello párbeszédpanelről. Az oktatóanyag elvégzéséhez nincs szükség adatbázisra. Miután kiválasztotta az app service-csomag, kattintson a **OK** toocreate hello projekt.
   
    ![][B5]

## <a name="authenticating-clients-toohello-webapi-backend"></a>Ügyfelek toohello WebAPI háttérrendszerből hitelesítése
Ebben a szakaszban egy új üzenet kezelő osztályt hoz létre **AuthenticationTestHandler** hello új háttérrendszerének. Ez az osztály van származtatva [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) és fel van véve egy üzenetkezelő, hello háttér beérkező összes kérelmet tud feldolgozni. 

1. A Megoldáskezelőben kattintson a jobb gombbal hello **AppBackend** projektre, kattintson **Hozzáadás**, majd kattintson a **osztály**. Hello új osztály neve **AuthenticationTestHandler.cs**, és kattintson a **Hozzáadás** toogenerate hello osztály. Ez az osztály lesz használt tooauthenticate felhasználók *az egyszerű hitelesítés* az egyszerűség érdekében. Vegye figyelembe, hogy az alkalmazása bármilyen hitelesítési séma alkalmazására képes.
2. A AuthenticationTestHandler.cs, adja hozzá a hello következő `using` utasításokat:
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

3. A AuthenticationTestHandler.cs hello cseréje `AuthenticationTestHandler` definition hello a következő kódot az osztály. 
   
    A kezelő engedélyeztetéséhez használandó hello kérelem a következő három feltétel hello fennállása esetén:
   
   * hello kérelem része egy *engedélyezési* fejléc. 
   * hello kérelem használ *alapvető* hitelesítés. 
   * hello felhasználói név karakterláncát és hello jelszóval hello ugyanazt a karakterláncot.
     
     Ellenkező esetben hello kérés elutasításra. Ez nem egy valós hitelesítési és engedélyezési módszer, csak egy nagyon egyszerű példa az oktatóanyag céljára.
     
     Ha hello kérelemüzenet hitelesítése és engedélyezése hello által `AuthenticationTestHandler`, akkor hello az egyszerű hitelesítés felhasználói lesz csatolt toohello aktuális kérelem hello [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx). Felhasználói adatok hello HttpContext-objektum egy másik vezérlő (RegisterController) által használható újabb tooadd egy [címke](https://msdn.microsoft.com/library/azure/dn530749.aspx) toohello értesítési regisztrációs kérelmet.
     
       public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();
     
               if (authorizationHeader != null && authorizationHeader
                   .StartsWith("Basic ", StringComparison.InvariantCultureIgnoreCase))
               {
                   string authorizationUserAndPwdBase64 =
                       authorizationHeader.Substring("Basic ".Length);
                   string authorizationUserAndPwd = Encoding.Default
                       .GetString(Convert.FromBase64String(authorizationUserAndPwdBase64));
                   string user = authorizationUserAndPwd.Split(':')[0];
                   string password = authorizationUserAndPwd.Split(':')[1];
     
                   if (verifyUserAndPwd(user, password))
                   {
                       // Attach hello new principal object toohello current HttpContext object
                       HttpContext.Current.User =
                           new GenericPrincipal(new GenericIdentity(user), new string[0]);
                       System.Threading.Thread.CurrentPrincipal =
                           System.Web.HttpContext.Current.User;
                   }
                   else return Unauthorized();
               }
               else return Unauthorized();
     
               return base.SendAsync(request, cancellationToken);
           }
     
           private bool verifyUserAndPwd(string user, string password)
           {
               // This is not a real authentication scheme.
               return user == password;
           }
     
           private Task<HttpResponseMessage> Unauthorized()
           {
               var response = new HttpResponseMessage(HttpStatusCode.Forbidden);
               var tsc = new TaskCompletionSource<HttpResponseMessage>();
               tsc.SetResult(response);
               return tsc.Task;
           }
       }
     
     > [!NOTE]
     > **Biztonsági megjegyzés**: hello `AuthenticationTestHandler` osztály nem biztosítja az IGAZ hitelesítés. Az egyszerű hitelesítés használt csak toomimic, és nem biztonságos. Az éles alkalmazásokban és szolgáltatásokban implementálnia kell egy biztonságos hitelesítési mechanizmust.                
     > 
     > 
4. Adja hozzá a következő kód hello hello végén hello `Register` metódus a hello **App_Start/WebApiConfig.cs** osztály tooregister hello üzenetkezelő:
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());
5. Mentse a módosításokat.

## <a name="registering-for-notifications-using-hello-webapi-backend"></a>A hello WebAPI háttérrendszerből használatával értesítések regisztrálása
Ebben a szakaszban adunk hozzá egy új tartományvezérlő toohello WebAPI háttér toohandle kérelmek tooregister a felhasználó-eszköz az értesítések a notification hubs hello ügyféloldali kódtár segítségével. hello vezérlő ad hozzá egy felhasználói címkét hello olyan felhasználóhoz, hitelesítési és toohello HttpContext hello által csatlakoztatott `AuthenticationTestHandler`. hello címke lesz hello karakterlánc-formátum `"username:<actual username>"`.

1. A Megoldáskezelőben kattintson a jobb gombbal hello **AppBackend** projektre, majd kattintson **NuGet-csomagok kezelése**.
2. Kattintson a bal oldalon hello, **Online**, keresse meg a **Microsoft.Azure.NotificationHubs** a hello **keresési** mezőbe.
3. Hello eredmények listájában kattintson **Microsoft Azure Notification Hubs**, és kattintson a **telepítése**. Hello telepítés befejeződését, majd zárja be a hello NuGet package manager ablakát.
   
    Ezzel hozzáad egy hivatkozást toohello Azure Notification Hubs SDK használatával hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomag</a>.
4. Most létrehozunk egy új osztály fájlt, amely a notification hub használt toosend értesítések hello kapcsolatot jelöli. A Solution Explorer hello, kattintson a jobb gombbal hello **modellek** mappát, kattintson a **hozzáadása**, majd kattintson a **osztály**. Hello új osztály neve **Notifications.cs**, majd kattintson a **Hozzáadás** toogenerate hello osztály. 
   
    ![][B6]
5. A Notifications.cs, adja hozzá a hello következő `using` hello fájl hello tetején utasítást:
   
        using Microsoft.Azure.NotificationHubs;
6. Cserélje le a hello `Notifications` hello következőre definition osztály, győződjön meg arról, hogy tooreplace hello két helyőrzőt hello kapcsolati karakterlánc (teljes hozzáférés) az értesítési központ és hello a központ nevét (rendelkezésre álló [klasszikus Azure portálon ](http://manage.windowsazure.com)):
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. A következőkben egy új vezérlőt fogunk létrehozni **RegisterController** néven. A Megoldáskezelőben kattintson a jobb gombbal hello **tartományvezérlők** mappát, majd kattintson a **hozzáadása**, kattintson a **vezérlő**. Kattintson a hello **Web API 2-es vezérlőhöz – üres** elemet, és kattintson a **Hozzáadás**. Hello új osztály neve **RegisterController**, és kattintson a **Hozzáadás** újra toogenerate hello vezérlő.
   
    ![][B7]
   
    ![][B8]
8. A RegisterController.cs, adja hozzá a hello következő `using` utasításokat:
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
9. Adja hozzá a következő kódot hello hello `RegisterController` definition osztályban. Vegye figyelembe, hogy ez a kód azt hozzá ez hello felhasználó felhasználói címke csatolt toohello HttpContext. hello felhasználó hitelesített, és csatlakoztatta toohello HttpContext osztály által hozzáadott, hello üzenetszűrő `AuthenticationTestHandler`. Azt is megteheti, hogy a felhasználó hello opcionális ellenőrzés tooverify legyen jogosultsága a hello tooregister kért címkék.
   
        private NotificationHubClient hub;
   
        public RegisterController()
        {
            hub = Notifications.Instance.Hub;
        }
   
        public class DeviceRegistration
        {
            public string Platform { get; set; }
            public string Handle { get; set; }
            public string[] Tags { get; set; }
        }
   
        // POST api/register
        // This creates a registration id
        public async Task<string> Post(string handle = null)
        {
            string newRegistrationId = null;
   
            // make sure there are no existing registrations for this push handle (used for iOS and Android)
            if (handle != null)
            {
                var registrations = await hub.GetRegistrationsByChannelAsync(handle, 100);
   
                foreach (RegistrationDescription registration in registrations)
                {
                    if (newRegistrationId == null)
                    {
                        newRegistrationId = registration.RegistrationId;
                    }
                    else
                    {
                        await hub.DeleteRegistrationAsync(registration);
                    }
                }
            }
   
            if (newRegistrationId == null) 
                newRegistrationId = await hub.CreateRegistrationIdAsync();
   
            return newRegistrationId;
        }
   
        // PUT api/register/5
        // This creates or updates a registration (with provided channelURI) at hello specified id
        public async Task<HttpResponseMessage> Put(string id, DeviceRegistration deviceUpdate)
        {
            RegistrationDescription registration = null;
            switch (deviceUpdate.Platform)
            {
                case "mpns":
                    registration = new MpnsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "wns":
                    registration = new WindowsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "apns":
                    registration = new AppleRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "gcm":
                    registration = new GcmRegistrationDescription(deviceUpdate.Handle);
                    break;
                default:
                    throw new HttpResponseException(HttpStatusCode.BadRequest);
            }
   
            registration.RegistrationId = id;
            var username = HttpContext.Current.User.Identity.Name;
   
            // add check if user is allowed tooadd these tags
            registration.Tags = new HashSet<string>(deviceUpdate.Tags);
            registration.Tags.Add("username:" + username);
   
            try
            {
                await hub.CreateOrUpdateRegistrationAsync(registration);
            }
            catch (MessagingException e)
            {
                ReturnGoneIfHubResponseIsGone(e);
            }
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        // DELETE api/register/5
        public async Task<HttpResponseMessage> Delete(string id)
        {
            await hub.DeleteRegistrationAsync(id);
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        private static void ReturnGoneIfHubResponseIsGone(MessagingException e)
        {
            var webex = e.InnerException as WebException;
            if (webex.Status == WebExceptionStatus.ProtocolError)
            {
                var response = (HttpWebResponse)webex.Response;
                if (response.StatusCode == HttpStatusCode.Gone)
                    throw new HttpRequestException(HttpStatusCode.Gone.ToString());
            }
        }
10. Mentse a módosításokat.

## <a name="sending-notifications-from-hello-webapi-backend"></a>Értesítések küldése a hello WebAPI háttérrendszerből
Ebben a szakaszban vegyen fel egy új vezérlőt, amely elérhetővé teszi lehetővé az ügyfél eszközök toosend hello felhasználónév címke Azure Notification Hubs szolgáltatás könyvtár használatát hello ASP.NET WebAPI háttérrendszerből alapján értesítést.

1. Hozzon létre egy másik új vezérlőt **NotificationsController** néven. Hozzon létre hello hello létrehozott ugyanúgy **RegisterController** hello előző szakaszban.
2. A NotificationsController.cs, adja hozzá a hello következő `using` utasításokat:
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
3. Adja hozzá a következő metódus toohello hello **NotificationsController** osztály.
   
    Ez a kód küldése egy értesítés típusa hello Platform Notification szolgáltatás (PNS) alapuló `pns` paraméter. hello értékének `to_tag` használt tooset hello van *felhasználónév* üdvözlőüzenetére a címkét. A címkének egyeznie kell egy aktív értesítésiközpont-regisztráció felhasználónév-címkéjével. hello értesítési üzenet hello POST kérelem törzse hello lekért és hello cél PNS van formázva. 
   
    Hello Platform Notification szolgáltatás (PNS), a támogatott eszközök tooreceive értesítések használja-e, attól függően különböző értesítések támogatott különböző formátumokban használatával. Windows rendszerű eszközökön például használhat [bejelentési értesítéseket a WNS formátummal](https://msdn.microsoft.com/library/windows/apps/br230849.aspx), amelyet egy másik PNS nem támogat közvetlenül. Így háttéralkalmazásának tooformat hello értesítési be támogatott értesítést kell az eszköz PNS hello toosupport tervezi. Kövesse a hello hello megfelelő küldési API [NotificationHubClient osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)
   
        public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag)
        {
            var user = HttpContext.Current.User.Identity.Name;
            string[] userTag = new string[2];
            userTag[0] = "username:" + to_tag;
            userTag[1] = "from:" + user;
   
            Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;
            HttpStatusCode ret = HttpStatusCode.InternalServerError;
   
            switch (pns.ToLower())
            {
                case "wns":
                    // Windows 8.1 / Windows Phone 8.1
                    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" + 
                                "From " + user + ": " + message + "</text></binding></visual></toast>";
                    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
                    break;
                case "apns":
                    // iOS
                    var alert = "{\"aps\":{\"alert\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(alert, userTag);
                    break;
                case "gcm":
                    // Android
                    var notif = "{ \"data\" : {\"message\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendGcmNativeNotificationAsync(notif, userTag);
                    break;
            }
   
            if (outcome != null)
            {
                if (!((outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Abandoned) ||
                    (outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Unknown)))
                {
                    ret = HttpStatusCode.OK;
                }
            }
   
            return Request.CreateResponse(ret);
        }
4. Nyomja le az **F5** toorun hello alkalmazás- és tooensure hello munkája pontosságának eddig. hello app kell elindítani a webböngészőt, és hello ASP.NET kezdőlap megjelenítése. 

## <a name="publish-hello-new-webapi-backend"></a>Közzététel új WebAPI háttérrendszerből hello
1. Most már az alkalmazás tooan Azure-webhely rendelés toomake fogjuk üzembe helyezni az összes eszköz érhető el. Kattintson a jobb gombbal a hello **AppBackend** projektre, és válassza ki **közzététel**.
2. Közzétételi célként válassza a **Microsoft Azure App Service** lehetőséget, majd kattintson a **New** (Új) gombra. Ekkor megnyílik a hello App Service létrehozása párbeszédpanel, amely segítséget nyújt az összes hello szükséges Azure-erőforrások toorun hello ASP.NET-webalkalmazás létrehozása az Azure-ban.

    ![][B15]
3. A hello **létrehozása az App Service** párbeszédpanelen válassza ki az Azure-fiókjával. Kattintson a **Change Type** (Típus módosítása) elemre, és válassza a **Web App** (Webalkalmazás) lehetőséget. Tartsa hello **webalkalmazásnév** adott és select hello **előfizetés**, **erőforráscsoport**, és **App Service-csomag**.  Kattintson a **Create** (Létrehozás) gombra.

4. Jegyezze fel a hello **webhely URL-címe** hello tulajdonság **összegzés** szakasz. Toothis URL-CÍMÉRE hivatkozik a *háttér végpont* ebben az oktatóanyagban később. Kattintson a **Publish** (Közzététel) gombra.

5. Hello varázsló befejezése után hello ASP.NET web app tooAzure teszi közzé, és ezután elindítja hello app hello alapértelmezett böngészőben.  Az alkalmazását az Azure App Servicesben tekintheti meg.

hello URL-cím hello webes alkalmazás neve a megadott korábban hello formátum http://<app_name>.azurewebsites.net használja.

[B1]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push1.png
[B2]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push2.png
[B3]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push3.png
[B4]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push4.png
[B5]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push5.png
[B6]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push6.png
[B7]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push7.png
[B8]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push8.png
[B14]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push14.png
[B15]: ./media/notification-hubs-aspnet-backend-notifyusers/publish-to-app-service.png
[B16]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users16.PNG
[B18]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users18.PNG
