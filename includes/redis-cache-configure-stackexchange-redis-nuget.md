<span data-ttu-id="66657-101">.NET-alkalmazásokban használható hello **StackExchange.Redis** gyorsítótárügyfél, a Visual Studio NuGet-csomagot, amely leegyszerűsíti a hello konfigurációs gyorsítótár ügyfélalkalmazások használatával állítható be.</span><span class="sxs-lookup"><span data-stu-id="66657-101">.NET applications can use hello **StackExchange.Redis** cache client, which can be configured in Visual Studio using a NuGet package that simplifies hello configuration of cache client applications.</span></span> 

> [!NOTE]
> <span data-ttu-id="66657-102">További információkért lásd: hello [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) github lap és hello [StackExchange.Redis gyorsítótár ügyfél dokumentáció](http://github.com/StackExchange/StackExchange.Redis#documentation).</span><span class="sxs-lookup"><span data-stu-id="66657-102">For more information, see hello [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) github page and  hello [StackExchange.Redis cache client documentation](http://github.com/StackExchange/StackExchange.Redis#documentation).</span></span>
> 
> 

<span data-ttu-id="66657-103">tooconfigure egy ügyfélalkalmazást, a Visual Studio használatával hello StackExchange.Redis NuGet-csomagot, kattintson a jobb gombbal a projekt hello **Megoldáskezelőben** válassza **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="66657-103">tooconfigure a client application in Visual Studio using hello StackExchange.Redis NuGet package, right-click hello project in **Solution Explorer** and choose **Manage NuGet Packages**.</span></span> 

![NuGet-csomagok kezelése](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

<span data-ttu-id="66657-105">Típus **StackExchange.Redis** vagy **StackExchange.Redis.StrongName** szöveg hello keresőmezőbe, válassza ki a kívánt verziójával hello hello eredmények közül, majd kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="66657-105">Type **StackExchange.Redis** or **StackExchange.Redis.StrongName** into hello search text box, select hello desired version from hello results, and click **Install**.</span></span>

> [!NOTE]
> <span data-ttu-id="66657-106">Ha inkább toouse hello erős névvel ellátott verziója **StackExchange.Redis** ügyféloldali kódtár válassza **StackExchange.Redis.StrongName**; ellenkező esetben válassza **StackExchange.Redis**.</span><span class="sxs-lookup"><span data-stu-id="66657-106">If you prefer toouse a strong-named version of hello **StackExchange.Redis** client library, choose **StackExchange.Redis.StrongName**; otherwise choose **StackExchange.Redis**.</span></span>
> 
> 

![StackExchange.Redis NuGet-csomag](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

<span data-ttu-id="66657-108">hello NuGet csomag tölti le, és hozzáadja a hello szükséges összeállítási referenciát az ügyfél alkalmazás tooaccess Azure Redis Cache hello StackExchange.Redis gyorsítótár-ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="66657-108">hello NuGet package downloads and adds hello required assembly references for your client application tooaccess Azure Redis Cache with hello StackExchange.Redis cache client.</span></span>

> [!NOTE]
> <span data-ttu-id="66657-109">Ha korábban már konfigurálta a projekt toouse StackExchange.Redis, ellenőrizze, hogy frissítéseket toohello csomagot hello **NuGet Package Manager**.</span><span class="sxs-lookup"><span data-stu-id="66657-109">If you have previously configured your project toouse StackExchange.Redis, you can check for updates toohello package from hello **NuGet Package Manager**.</span></span> <span data-ttu-id="66657-110">a toocheck és telepítse a frissített verziói hello StackExchange.Redis NuGet-csomagot, kattintson **frissítések** a hello hello **NuGet-Csomagkezelő** ablak.</span><span class="sxs-lookup"><span data-stu-id="66657-110">toocheck for and install updated versions of hello StackExchange.Redis NuGet package, click **Updates** in hello hello **NuGet Package Manager** window.</span></span> <span data-ttu-id="66657-111">Ha egy frissítés toohello StackExchange.Redis NuGet-csomag nem érhető el, frissítheti a projekt toouse hello frissített verziója.</span><span class="sxs-lookup"><span data-stu-id="66657-111">If an update toohello StackExchange.Redis NuGet package is available, you can update your project toouse hello updated version.</span></span>
> 
> 

<span data-ttu-id="66657-112">Hello StackExchange.Redis NuGet-csomag kattintva is telepíthet **NuGet-Csomagkezelő**, **Csomagkezelő konzol** a hello **eszközök** menüt, és futó hello parancs követően – hello **Csomagkezelő konzol** ablak.</span><span class="sxs-lookup"><span data-stu-id="66657-112">You can also install hello StackExchange.Redis NuGet package by clicking **NuGet Package Manager**, **Package Manager Console** from hello **Tools** menu, and running hello following command from hello **Package Manager Console** window.</span></span>
    
```
Install-Package StackExchange.Redis
```
