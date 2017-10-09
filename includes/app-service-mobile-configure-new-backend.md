
1. <span data-ttu-id="2d061-101">Kattintson a hello **alkalmazásszolgáltatások** gombra, válassza ki a Mobile Apps háttér, válassza ki **gyors üzembe helyezés**, majd válassza ki az ügyfél-platform (iOS, Android, Xamarin, Cordova).</span><span class="sxs-lookup"><span data-stu-id="2d061-101">Click hello **App Services** button, select your Mobile Apps back end, select **Quickstart**, and then select your client platform (iOS, Android, Xamarin, Cordova).</span></span>

    ![Azure Portal a kiemelt Mobile Apps Quickstarttal][quickstart]

2. <span data-ttu-id="2d061-103">Ha az adatbázis-kapcsolat nincs konfigurálva, hozzon létre egyet hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="2d061-103">If a database connection is not configured, create one by doing hello following:</span></span>

    ![Azure Mobile Apps Connect toodatabase-portálon][connect]

    <span data-ttu-id="2d061-105">a.</span><span class="sxs-lookup"><span data-stu-id="2d061-105">a.</span></span> <span data-ttu-id="2d061-106">Hozzon létre egy új SQL-adatbázist és -kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="2d061-106">Create a new SQL database and server.</span></span>

    ![Azure Portal Mobile Appsszel – új adatbázis és kiszolgáló létrehozása][server]

    <span data-ttu-id="2d061-108">b.</span><span class="sxs-lookup"><span data-stu-id="2d061-108">b.</span></span> <span data-ttu-id="2d061-109">Várjon, amíg hello adatkapcsolat sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="2d061-109">Wait until hello data connection is successfully created.</span></span>

    ![Az Azure Portal értesítése az adatkapcsolat sikeres létrejöttéről][notification]

    <span data-ttu-id="2d061-111">c.</span><span class="sxs-lookup"><span data-stu-id="2d061-111">c.</span></span> <span data-ttu-id="2d061-112">Az adatkapcsolatnak sikeresnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2d061-112">Data connection must be successful.</span></span>

    ![Az Azure Portal értesítése: „Már van adatkapcsolata”][already-connection]

3. <span data-ttu-id="2d061-114">A **2. Tábla API létrehozása** elemnél a **Háttérrendszer nyelveként** válassza a Node.js-t.</span><span class="sxs-lookup"><span data-stu-id="2d061-114">Under **2. Create a table API**, select Node.js for **Backend language**.</span></span> 
 
4. <span data-ttu-id="2d061-115">Fogadja el a hello visszaigazolás, és válassza ki **TodoItem tábla létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="2d061-115">Accept hello acknowledgment, and then select **Create TodoItem table**.</span></span>  
    <span data-ttu-id="2d061-116">Ez a művelet létrehoz egy új teendőtáblát az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="2d061-116">This action creates a new to-do item table in your database.</span></span> 

    >[!IMPORTANT]
    > <span data-ttu-id="2d061-117">Egy meglévő háttér tooNode.js váltás felülírja az összes tartalmát.</span><span class="sxs-lookup"><span data-stu-id="2d061-117">Switching an existing back end tooNode.js overwrites all contents.</span></span> <span data-ttu-id="2d061-118">a .NET háttérből helyett, lásd: toocreate [használható az SDK hello .NET háttér-kiszolgáló a Mobile Apps][instructions].</span><span class="sxs-lookup"><span data-stu-id="2d061-118">toocreate a .NET back end instead, see [Work with hello .NET back-end server SDK for Mobile Apps][instructions].</span></span>

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
