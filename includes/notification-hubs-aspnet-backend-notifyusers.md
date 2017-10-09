## <a name="create-hello-webapi-project"></a><span data-ttu-id="a3969-101">Hello WebAPI projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3969-101">Create hello WebAPI Project</span></span>
<span data-ttu-id="a3969-102">Egy új ASP.NET WebAPI háttérrendszerből hello szakaszokban szereplő jön létre, és hogy fog rendelkezni a három fő célja:</span><span class="sxs-lookup"><span data-stu-id="a3969-102">A new ASP.NET WebAPI backend will be created in hello sections that follow and it will have three main purposes:</span></span>

1. <span data-ttu-id="a3969-103">**Ügyfelek hitelesítése**: egy üzenetkezelő hozzáadódik a későbbi tooauthenticate ügyfélkérelmek és társítása hello felhasználói hello kérelemmel.</span><span class="sxs-lookup"><span data-stu-id="a3969-103">**Authenticating Clients**: A message handler will be added later tooauthenticate client requests and associate hello user with hello request.</span></span>
2. <span data-ttu-id="a3969-104">**Ügyfél-értesítési regisztrációk**: később adhat egy tartományvezérlő toohandle új regisztrációk egy ügyfél eszköz tooreceive értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="a3969-104">**Client Notification Registrations**: Later, you will add a controller toohandle new registrations for a client device tooreceive notifications.</span></span> <span data-ttu-id="a3969-105">hello hitelesített felhasználó nevét a rendszer automatikusan hozzáadja toohello eszközregisztrációs szerint egy [címke](https://msdn.microsoft.com/library/azure/dn530749.aspx).</span><span class="sxs-lookup"><span data-stu-id="a3969-105">hello authenticated user name will automatically be added toohello registration as a [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx).</span></span>
3. <span data-ttu-id="a3969-106">**Értesítések tooClients küldése**: később is hozzáadhat egy vezérlő így az a felhasználó tootrigger egy biztonságos leküldéses toodevices tooprovide és hello címke társított ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="a3969-106">**Sending Notifications tooClients**: Later, you will also add a controller tooprovide a way for a user tootrigger a secure push toodevices and clients associated with hello tag.</span></span> 

<span data-ttu-id="a3969-107">hello lépések bemutatják, hogyan toocreate hello új ASP.NET WebAPI háttérrendszerből:</span><span class="sxs-lookup"><span data-stu-id="a3969-107">hello following steps show how toocreate hello new ASP.NET WebAPI backend:</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="a3969-108">Visual Studio 2015 használata vagy a korábban, az oktatóanyag elindítása előtt győződjön meg arról, hogy telepítette a NuGet-Csomagkezelő hello hello legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="a3969-108">If you are using Visual Studio 2015 or earlier, before starting this tutorial, please ensure that you have installed hello latest version of hello NuGet Package Manager.</span></span> <span data-ttu-id="a3969-109">toocheck, kezdő Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a3969-109">toocheck, start Visual Studio.</span></span> <span data-ttu-id="a3969-110">A hello **eszközök** menüben kattintson a **bővítmények és frissítések**.</span><span class="sxs-lookup"><span data-stu-id="a3969-110">From hello **Tools** menu, click **Extensions and Updates**.</span></span> <span data-ttu-id="a3969-111">Keresse meg **NuGet-Csomagkezelő** Visual Studio, és győződjön meg arról, hogy verziójának hello legújabb verziójával rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a3969-111">Search for **NuGet Package Manager** for your version of Visual Studio, and make sure you have hello latest version.</span></span> <span data-ttu-id="a3969-112">Ha nem, távolítsa el, majd telepítse újra a NuGet-Csomagkezelő hello.</span><span class="sxs-lookup"><span data-stu-id="a3969-112">If not, please uninstall, then reinstall hello NuGet Package Manager.</span></span>
> 
> ![][B4]
> 
> [!NOTE]
> <span data-ttu-id="a3969-113">Győződjön meg arról, hogy telepítette a Visual Studio hello [Azure SDK](https://azure.microsoft.com/downloads/) webhely központi telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="a3969-113">Make sure you have installed hello Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/) for website deployment.</span></span>
> 
> 

1. <span data-ttu-id="a3969-114">Indítsa el a Visual Studiót vagy a Visual Studio Expresst.</span><span class="sxs-lookup"><span data-stu-id="a3969-114">Start Visual Studio or Visual Studio Express.</span></span> <span data-ttu-id="a3969-115">Kattintson a **Server Explorer** , jelentkezzen be Azure-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="a3969-115">Click **Server Explorer** and sign in tooyour Azure account.</span></span> <span data-ttu-id="a3969-116">A Visual Studio kell toocreate hello webhely erőforrások fiókját a bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="a3969-116">Visual Studio will need you signed in toocreate hello web site resources on your account.</span></span>
2. <span data-ttu-id="a3969-117">A Visual Studióban kattintson **fájl**, majd kattintson a **új**, majd **projekt**, bontsa ki a **sablonok**, **Visual C#**, majd kattintson a **webes** és **ASP.NET Web Application**, hello típusnév **AppBackend**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="a3969-117">In Visual Studio, click **File**, then click **New**, then **Project**, expand **Templates**, **Visual C#**, then click **Web** and **ASP.NET Web Application**, type hello name **AppBackend**, and then click **OK**.</span></span> 
   
    ![][B1]
3. <span data-ttu-id="a3969-118">A hello **új ASP.NET projekt** párbeszédpanel, kattintson **Web API**, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="a3969-118">In hello **New ASP.NET Project** dialog, click **Web API**, then click **OK**.</span></span>
   
    ![][B2]
4. <span data-ttu-id="a3969-119">A hello **konfigurálása a Microsoft Azure Web Apps** párbeszédpanelen válasszon egy előfizetést, és egy **App Service-csomag** már létrehozott.</span><span class="sxs-lookup"><span data-stu-id="a3969-119">In hello **Configure Microsoft Azure Web App** dialog, choose a subscription, and an **App Service plan** you have already created.</span></span> <span data-ttu-id="a3969-120">Másik lehetőségként **hozzon létre egy új app service-csomag** , és hozzon létre egy hello párbeszédpanelről.</span><span class="sxs-lookup"><span data-stu-id="a3969-120">You can also choose **Create a new app service plan** and create one from hello dialog.</span></span> <span data-ttu-id="a3969-121">Az oktatóanyag elvégzéséhez nincs szükség adatbázisra.</span><span class="sxs-lookup"><span data-stu-id="a3969-121">You do not need a database for this tutorial.</span></span> <span data-ttu-id="a3969-122">Miután kiválasztotta az app service-csomag, kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="a3969-122">Once you have selected your app service plan, click **OK** toocreate hello project.</span></span>
   
    ![][B5]

## <a name="authenticating-clients-toohello-webapi-backend"></a><span data-ttu-id="a3969-123">Ügyfelek toohello WebAPI háttérrendszerből hitelesítése</span><span class="sxs-lookup"><span data-stu-id="a3969-123">Authenticating Clients toohello WebAPI Backend</span></span>
<span data-ttu-id="a3969-124">Ebben a szakaszban egy új üzenet kezelő osztályt hoz létre **AuthenticationTestHandler** hello új háttérrendszerének.</span><span class="sxs-lookup"><span data-stu-id="a3969-124">In this section, you will create a new message handler class named **AuthenticationTestHandler** for hello new backend.</span></span> <span data-ttu-id="a3969-125">Ez az osztály van származtatva [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) és fel van véve egy üzenetkezelő, hello háttér beérkező összes kérelmet tud feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="a3969-125">This class is derived from [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) and added as a message handler so it can process all requests coming into hello backend.</span></span> 

1. <span data-ttu-id="a3969-126">A Megoldáskezelőben kattintson a jobb gombbal hello **AppBackend** projektre, kattintson **Hozzáadás**, majd kattintson a **osztály**.</span><span class="sxs-lookup"><span data-stu-id="a3969-126">In Solution Explorer, right-click hello **AppBackend** project, click **Add**, then click **Class**.</span></span> <span data-ttu-id="a3969-127">Hello új osztály neve **AuthenticationTestHandler.cs**, és kattintson a **Hozzáadás** toogenerate hello osztály.</span><span class="sxs-lookup"><span data-stu-id="a3969-127">Name hello new class **AuthenticationTestHandler.cs**, and click **Add** toogenerate hello class.</span></span> <span data-ttu-id="a3969-128">Ez az osztály lesz használt tooauthenticate felhasználók *az egyszerű hitelesítés* az egyszerűség érdekében.</span><span class="sxs-lookup"><span data-stu-id="a3969-128">This class will be used tooauthenticate users using *Basic Authentication* for simplicity.</span></span> <span data-ttu-id="a3969-129">Vegye figyelembe, hogy az alkalmazása bármilyen hitelesítési séma alkalmazására képes.</span><span class="sxs-lookup"><span data-stu-id="a3969-129">Note that your app can use any authentication scheme.</span></span>
2. <span data-ttu-id="a3969-130">A AuthenticationTestHandler.cs, adja hozzá a hello következő `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="a3969-130">In AuthenticationTestHandler.cs, add hello following `using` statements:</span></span>
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

3. <span data-ttu-id="a3969-131">A AuthenticationTestHandler.cs hello cseréje `AuthenticationTestHandler` definition hello a következő kódot az osztály.</span><span class="sxs-lookup"><span data-stu-id="a3969-131">In AuthenticationTestHandler.cs, replacing hello `AuthenticationTestHandler` class definition with hello following code.</span></span> 
   
    <span data-ttu-id="a3969-132">A kezelő engedélyeztetéséhez használandó hello kérelem a következő három feltétel hello fennállása esetén:</span><span class="sxs-lookup"><span data-stu-id="a3969-132">This handler will authorize hello request when hello following three conditions are all true:</span></span>
   
   * <span data-ttu-id="a3969-133">hello kérelem része egy *engedélyezési* fejléc.</span><span class="sxs-lookup"><span data-stu-id="a3969-133">hello request included an *Authorization* header.</span></span> 
   * <span data-ttu-id="a3969-134">hello kérelem használ *alapvető* hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="a3969-134">hello request uses *basic* authentication.</span></span> 
   * <span data-ttu-id="a3969-135">hello felhasználói név karakterláncát és hello jelszóval hello ugyanazt a karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="a3969-135">hello user name string and hello password string are hello same string.</span></span>
     
     <span data-ttu-id="a3969-136">Ellenkező esetben hello kérés elutasításra.</span><span class="sxs-lookup"><span data-stu-id="a3969-136">Otherwise, hello request will be rejected.</span></span> <span data-ttu-id="a3969-137">Ez nem egy valós hitelesítési és engedélyezési módszer,</span><span class="sxs-lookup"><span data-stu-id="a3969-137">This is not a true authentication and authorization approach.</span></span> <span data-ttu-id="a3969-138">csak egy nagyon egyszerű példa az oktatóanyag céljára.</span><span class="sxs-lookup"><span data-stu-id="a3969-138">It is just a very simple example for this tutorial.</span></span>
     
     <span data-ttu-id="a3969-139">Ha hello kérelemüzenet hitelesítése és engedélyezése hello által `AuthenticationTestHandler`, akkor hello az egyszerű hitelesítés felhasználói lesz csatolt toohello aktuális kérelem hello [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx).</span><span class="sxs-lookup"><span data-stu-id="a3969-139">If hello request message is authenticated and authorized by hello `AuthenticationTestHandler`, then hello basic authentication user will be attached toohello current request on hello [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx).</span></span> <span data-ttu-id="a3969-140">Felhasználói adatok hello HttpContext-objektum egy másik vezérlő (RegisterController) által használható újabb tooadd egy [címke](https://msdn.microsoft.com/library/azure/dn530749.aspx) toohello értesítési regisztrációs kérelmet.</span><span class="sxs-lookup"><span data-stu-id="a3969-140">User information in hello HttpContext will be used by another controller (RegisterController) later tooadd a [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx) toohello notification registration request.</span></span>
     
       <span data-ttu-id="a3969-141">public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();</span><span class="sxs-lookup"><span data-stu-id="a3969-141">public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();</span></span>
     
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
       <span data-ttu-id="a3969-142">}</span><span class="sxs-lookup"><span data-stu-id="a3969-142">}</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="a3969-143">**Biztonsági megjegyzés**: hello `AuthenticationTestHandler` osztály nem biztosítja az IGAZ hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="a3969-143">**Security Note**: hello `AuthenticationTestHandler` class does not provide true authentication.</span></span> <span data-ttu-id="a3969-144">Az egyszerű hitelesítés használt csak toomimic, és nem biztonságos.</span><span class="sxs-lookup"><span data-stu-id="a3969-144">It is used only toomimic basic authentication and is not secure.</span></span> <span data-ttu-id="a3969-145">Az éles alkalmazásokban és szolgáltatásokban implementálnia kell egy biztonságos hitelesítési mechanizmust.</span><span class="sxs-lookup"><span data-stu-id="a3969-145">You must implement a secure authentication mechanism in your production applications and services.</span></span>                
     > 
     > 
4. <span data-ttu-id="a3969-146">Adja hozzá a következő kód hello hello végén hello `Register` metódus a hello **App_Start/WebApiConfig.cs** osztály tooregister hello üzenetkezelő:</span><span class="sxs-lookup"><span data-stu-id="a3969-146">Add hello following code at hello end of hello `Register` method in hello **App_Start/WebApiConfig.cs** class tooregister hello message handler:</span></span>
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());
5. <span data-ttu-id="a3969-147">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="a3969-147">Save your changes.</span></span>

## <a name="registering-for-notifications-using-hello-webapi-backend"></a><span data-ttu-id="a3969-148">A hello WebAPI háttérrendszerből használatával értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="a3969-148">Registering for Notifications using hello WebAPI Backend</span></span>
<span data-ttu-id="a3969-149">Ebben a szakaszban adunk hozzá egy új tartományvezérlő toohello WebAPI háttér toohandle kérelmek tooregister a felhasználó-eszköz az értesítések a notification hubs hello ügyféloldali kódtár segítségével.</span><span class="sxs-lookup"><span data-stu-id="a3969-149">In this section, we will add a new controller toohello WebAPI backend toohandle requests tooregister a user and device for notifications using hello client library for notification hubs.</span></span> <span data-ttu-id="a3969-150">hello vezérlő ad hozzá egy felhasználói címkét hello olyan felhasználóhoz, hitelesítési és toohello HttpContext hello által csatlakoztatott `AuthenticationTestHandler`.</span><span class="sxs-lookup"><span data-stu-id="a3969-150">hello controller will add a user tag for hello user that was authenticated and attached toohello HttpContext by hello `AuthenticationTestHandler`.</span></span> <span data-ttu-id="a3969-151">hello címke lesz hello karakterlánc-formátum `"username:<actual username>"`.</span><span class="sxs-lookup"><span data-stu-id="a3969-151">hello tag will have hello string format, `"username:<actual username>"`.</span></span>

1. <span data-ttu-id="a3969-152">A Megoldáskezelőben kattintson a jobb gombbal hello **AppBackend** projektre, majd kattintson **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="a3969-152">In Solution Explorer, right-click hello **AppBackend** project and then click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="a3969-153">Kattintson a bal oldalon hello, **Online**, keresse meg a **Microsoft.Azure.NotificationHubs** a hello **keresési** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="a3969-153">On hello left-hand side, click **Online**, and search for **Microsoft.Azure.NotificationHubs** in hello **Search** box.</span></span>
3. <span data-ttu-id="a3969-154">Hello eredmények listájában kattintson **Microsoft Azure Notification Hubs**, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="a3969-154">In hello results list, click **Microsoft Azure Notification Hubs**, and then click **Install**.</span></span> <span data-ttu-id="a3969-155">Hello telepítés befejeződését, majd zárja be a hello NuGet package manager ablakát.</span><span class="sxs-lookup"><span data-stu-id="a3969-155">Complete hello installation, then close hello NuGet package manager window.</span></span>
   
    <span data-ttu-id="a3969-156">Ezzel hozzáad egy hivatkozást toohello Azure Notification Hubs SDK használatával hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomag</a>.</span><span class="sxs-lookup"><span data-stu-id="a3969-156">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
4. <span data-ttu-id="a3969-157">Most létrehozunk egy új osztály fájlt, amely a notification hub használt toosend értesítések hello kapcsolatot jelöli.</span><span class="sxs-lookup"><span data-stu-id="a3969-157">We will now create a new class file that represents hello connection with notification hub used toosend notifications.</span></span> <span data-ttu-id="a3969-158">A Solution Explorer hello, kattintson a jobb gombbal hello **modellek** mappát, kattintson a **hozzáadása**, majd kattintson a **osztály**.</span><span class="sxs-lookup"><span data-stu-id="a3969-158">In hello Solution Explorer, right-click hello **Models** folder, click **Add**, then click **Class**.</span></span> <span data-ttu-id="a3969-159">Hello új osztály neve **Notifications.cs**, majd kattintson a **Hozzáadás** toogenerate hello osztály.</span><span class="sxs-lookup"><span data-stu-id="a3969-159">Name hello new class **Notifications.cs**, then click **Add** toogenerate hello class.</span></span> 
   
    ![][B6]
5. <span data-ttu-id="a3969-160">A Notifications.cs, adja hozzá a hello következő `using` hello fájl hello tetején utasítást:</span><span class="sxs-lookup"><span data-stu-id="a3969-160">In Notifications.cs, add hello following `using` statement at hello top of hello file:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
6. <span data-ttu-id="a3969-161">Cserélje le a hello `Notifications` hello következőre definition osztály, győződjön meg arról, hogy tooreplace hello két helyőrzőt hello kapcsolati karakterlánc (teljes hozzáférés) az értesítési központ és hello a központ nevét (rendelkezésre álló [klasszikus Azure portálon ](http://manage.windowsazure.com)):</span><span class="sxs-lookup"><span data-stu-id="a3969-161">Replace hello `Notifications` class definition with hello following and make sure tooreplace hello two placeholders with hello connection string (with full access) for your notification hub, and hello hub name (available at [Azure Classic Portal](http://manage.windowsazure.com)):</span></span>
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. <span data-ttu-id="a3969-162">A következőkben egy új vezérlőt fogunk létrehozni **RegisterController** néven.</span><span class="sxs-lookup"><span data-stu-id="a3969-162">Next we will create a new controller named **RegisterController**.</span></span> <span data-ttu-id="a3969-163">A Megoldáskezelőben kattintson a jobb gombbal hello **tartományvezérlők** mappát, majd kattintson a **hozzáadása**, kattintson a **vezérlő**.</span><span class="sxs-lookup"><span data-stu-id="a3969-163">In Solution Explorer, right-click hello **Controllers** folder, then click **Add**, then click **Controller**.</span></span> <span data-ttu-id="a3969-164">Kattintson a hello **Web API 2-es vezérlőhöz – üres** elemet, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a3969-164">Click hello **Web API 2 Controller -- Empty** item, and then click **Add**.</span></span> <span data-ttu-id="a3969-165">Hello új osztály neve **RegisterController**, és kattintson a **Hozzáadás** újra toogenerate hello vezérlő.</span><span class="sxs-lookup"><span data-stu-id="a3969-165">Name hello new class **RegisterController**, and then click **Add** again toogenerate hello controller.</span></span>
   
    ![][B7]
   
    ![][B8]
8. <span data-ttu-id="a3969-166">A RegisterController.cs, adja hozzá a hello következő `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="a3969-166">In RegisterController.cs, add hello following `using` statements:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
9. <span data-ttu-id="a3969-167">Adja hozzá a következő kódot hello hello `RegisterController` definition osztályban.</span><span class="sxs-lookup"><span data-stu-id="a3969-167">Add hello following code inside hello `RegisterController` class definition.</span></span> <span data-ttu-id="a3969-168">Vegye figyelembe, hogy ez a kód azt hozzá ez hello felhasználó felhasználói címke csatolt toohello HttpContext.</span><span class="sxs-lookup"><span data-stu-id="a3969-168">Note that in this code, we add a user tag for hello user this is attached toohello HttpContext.</span></span> <span data-ttu-id="a3969-169">hello felhasználó hitelesített, és csatlakoztatta toohello HttpContext osztály által hozzáadott, hello üzenetszűrő `AuthenticationTestHandler`.</span><span class="sxs-lookup"><span data-stu-id="a3969-169">hello user was authenticated and attached toohello HttpContext by hello message filter we added, `AuthenticationTestHandler`.</span></span> <span data-ttu-id="a3969-170">Azt is megteheti, hogy a felhasználó hello opcionális ellenőrzés tooverify legyen jogosultsága a hello tooregister kért címkék.</span><span class="sxs-lookup"><span data-stu-id="a3969-170">You can also add optional checks tooverify that hello user has rights tooregister for hello requested tags.</span></span>
   
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
10. <span data-ttu-id="a3969-171">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="a3969-171">Save your changes.</span></span>

## <a name="sending-notifications-from-hello-webapi-backend"></a><span data-ttu-id="a3969-172">Értesítések küldése a hello WebAPI háttérrendszerből</span><span class="sxs-lookup"><span data-stu-id="a3969-172">Sending Notifications from hello WebAPI Backend</span></span>
<span data-ttu-id="a3969-173">Ebben a szakaszban vegyen fel egy új vezérlőt, amely elérhetővé teszi lehetővé az ügyfél eszközök toosend hello felhasználónév címke Azure Notification Hubs szolgáltatás könyvtár használatát hello ASP.NET WebAPI háttérrendszerből alapján értesítést.</span><span class="sxs-lookup"><span data-stu-id="a3969-173">In this section you add a new controller that exposes a way for client devices toosend a notification based on hello username tag using Azure Notification Hubs Service Management Library in hello ASP.NET WebAPI backend.</span></span>

1. <span data-ttu-id="a3969-174">Hozzon létre egy másik új vezérlőt **NotificationsController** néven.</span><span class="sxs-lookup"><span data-stu-id="a3969-174">Create another new controller named **NotificationsController**.</span></span> <span data-ttu-id="a3969-175">Hozzon létre hello hello létrehozott ugyanúgy **RegisterController** hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="a3969-175">Create it hello same way you created hello **RegisterController** in hello previous section.</span></span>
2. <span data-ttu-id="a3969-176">A NotificationsController.cs, adja hozzá a hello következő `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="a3969-176">In NotificationsController.cs, add hello following `using` statements:</span></span>
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
3. <span data-ttu-id="a3969-177">Adja hozzá a következő metódus toohello hello **NotificationsController** osztály.</span><span class="sxs-lookup"><span data-stu-id="a3969-177">Add hello following method toohello **NotificationsController** class.</span></span>
   
    <span data-ttu-id="a3969-178">Ez a kód küldése egy értesítés típusa hello Platform Notification szolgáltatás (PNS) alapuló `pns` paraméter.</span><span class="sxs-lookup"><span data-stu-id="a3969-178">This code send a notification type based on hello Platform Notification Service (PNS) `pns` parameter.</span></span> <span data-ttu-id="a3969-179">hello értékének `to_tag` használt tooset hello van *felhasználónév* üdvözlőüzenetére a címkét.</span><span class="sxs-lookup"><span data-stu-id="a3969-179">hello value of `to_tag` is used tooset hello *username* tag on hello message.</span></span> <span data-ttu-id="a3969-180">A címkének egyeznie kell egy aktív értesítésiközpont-regisztráció felhasználónév-címkéjével.</span><span class="sxs-lookup"><span data-stu-id="a3969-180">This tag must match a username tag of an active notification hub registration.</span></span> <span data-ttu-id="a3969-181">hello értesítési üzenet hello POST kérelem törzse hello lekért és hello cél PNS van formázva.</span><span class="sxs-lookup"><span data-stu-id="a3969-181">hello notification message is pulled from hello body of hello POST request and formatted for hello target PNS.</span></span> 
   
    <span data-ttu-id="a3969-182">Hello Platform Notification szolgáltatás (PNS), a támogatott eszközök tooreceive értesítések használja-e, attól függően különböző értesítések támogatott különböző formátumokban használatával.</span><span class="sxs-lookup"><span data-stu-id="a3969-182">Depending on hello Platform Notification Service (PNS) that your supported devices use tooreceive notifications, different notifications are supported using different formats.</span></span> <span data-ttu-id="a3969-183">Windows rendszerű eszközökön például használhat [bejelentési értesítéseket a WNS formátummal](https://msdn.microsoft.com/library/windows/apps/br230849.aspx), amelyet egy másik PNS nem támogat közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="a3969-183">For example on Windows devices, you could use a [toast notification with WNS](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) that isn't directly supported by another PNS.</span></span> <span data-ttu-id="a3969-184">Így háttéralkalmazásának tooformat hello értesítési be támogatott értesítést kell az eszköz PNS hello toosupport tervezi.</span><span class="sxs-lookup"><span data-stu-id="a3969-184">So your backend would need tooformat hello notification into a supported notification for hello PNS of devices you plan toosupport.</span></span> <span data-ttu-id="a3969-185">Kövesse a hello hello megfelelő küldési API [NotificationHubClient osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)</span><span class="sxs-lookup"><span data-stu-id="a3969-185">Then use hello appropriate send API on hello [NotificationHubClient class](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)</span></span>
   
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
4. <span data-ttu-id="a3969-186">Nyomja le az **F5** toorun hello alkalmazás- és tooensure hello munkája pontosságának eddig.</span><span class="sxs-lookup"><span data-stu-id="a3969-186">Press **F5** toorun hello application and tooensure hello accuracy of your work so far.</span></span> <span data-ttu-id="a3969-187">hello app kell elindítani a webböngészőt, és hello ASP.NET kezdőlap megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="a3969-187">hello app should launch a web browser and display hello ASP.NET home page.</span></span> 

## <a name="publish-hello-new-webapi-backend"></a><span data-ttu-id="a3969-188">Közzététel új WebAPI háttérrendszerből hello</span><span class="sxs-lookup"><span data-stu-id="a3969-188">Publish hello new WebAPI Backend</span></span>
1. <span data-ttu-id="a3969-189">Most már az alkalmazás tooan Azure-webhely rendelés toomake fogjuk üzembe helyezni az összes eszköz érhető el.</span><span class="sxs-lookup"><span data-stu-id="a3969-189">Now we will deploy this app tooan Azure Website in order toomake it accessible from all devices.</span></span> <span data-ttu-id="a3969-190">Kattintson a jobb gombbal a hello **AppBackend** projektre, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="a3969-190">Right-click on hello **AppBackend** project and select **Publish**.</span></span>
2. <span data-ttu-id="a3969-191">Közzétételi célként válassza a **Microsoft Azure App Service** lehetőséget, majd kattintson a **New** (Új) gombra.</span><span class="sxs-lookup"><span data-stu-id="a3969-191">Select **Microsoft Azure App Service** as your publish target and click **Publish**.</span></span> <span data-ttu-id="a3969-192">Ekkor megnyílik a hello App Service létrehozása párbeszédpanel, amely segítséget nyújt az összes hello szükséges Azure-erőforrások toorun hello ASP.NET-webalkalmazás létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="a3969-192">This opens hello Create App Service dialog, which helps you create all hello necessary Azure resources toorun hello ASP.NET web app in Azure.</span></span>

    ![][B15]
3. <span data-ttu-id="a3969-193">A hello **létrehozása az App Service** párbeszédpanelen válassza ki az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="a3969-193">In hello **Create App Service** dialog, select your Azure account.</span></span> <span data-ttu-id="a3969-194">Kattintson a **Change Type** (Típus módosítása) elemre, és válassza a **Web App** (Webalkalmazás) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a3969-194">Click **Change Type** and select **Web App**.</span></span> <span data-ttu-id="a3969-195">Tartsa hello **webalkalmazásnév** adott és select hello **előfizetés**, **erőforráscsoport**, és **App Service-csomag**.</span><span class="sxs-lookup"><span data-stu-id="a3969-195">Keep hello **Web App Name** given and select hello **Subscription**, **Resource Group**, and **App Service Plan**.</span></span>  <span data-ttu-id="a3969-196">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a3969-196">Click **Create**.</span></span>

4. <span data-ttu-id="a3969-197">Jegyezze fel a hello **webhely URL-címe** hello tulajdonság **összegzés** szakasz.</span><span class="sxs-lookup"><span data-stu-id="a3969-197">Make a note of hello **Site URL** property in hello **Summary** section.</span></span> <span data-ttu-id="a3969-198">Toothis URL-CÍMÉRE hivatkozik a *háttér végpont* ebben az oktatóanyagban később.</span><span class="sxs-lookup"><span data-stu-id="a3969-198">We will refer toothis URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="a3969-199">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="a3969-199">Click **Publish**.</span></span>

5. <span data-ttu-id="a3969-200">Hello varázsló befejezése után hello ASP.NET web app tooAzure teszi közzé, és ezután elindítja hello app hello alapértelmezett böngészőben.</span><span class="sxs-lookup"><span data-stu-id="a3969-200">Once hello wizard completes, it publishes hello ASP.NET web app tooAzure, and then launches hello app in hello default browser.</span></span>  <span data-ttu-id="a3969-201">Az alkalmazását az Azure App Servicesben tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="a3969-201">Your application will be viewable in Azure App Services.</span></span>

<span data-ttu-id="a3969-202">hello URL-cím hello webes alkalmazás neve a megadott korábban hello formátum http://<app_name>.azurewebsites.net használja.</span><span class="sxs-lookup"><span data-stu-id="a3969-202">hello URL uses hello web app name that you specified earlier, with hello format http://<app_name>.azurewebsites.net.</span></span>

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
