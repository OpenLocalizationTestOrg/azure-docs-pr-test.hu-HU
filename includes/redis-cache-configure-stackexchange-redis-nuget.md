<span data-ttu-id="f2bbd-101">A .NET-alkalmazások képesek használni a **StackExchange.Redis** gyorsítótárügyfelet, amely a Visual Studióban konfigurálható a gyorsítótár-ügyfélalkalmazások konfigurálását leegyszerűsítő NuGet-csomagokkal.</span><span class="sxs-lookup"><span data-stu-id="f2bbd-101">.NET applications can use the **StackExchange.Redis** cache client, which can be configured in Visual Studio using a NuGet package that simplifies the configuration of cache client applications.</span></span> 

> [!NOTE]
> <span data-ttu-id="f2bbd-102">További információkat a [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) github-oldalon és a [StackExchange.Redis gyorsítótárügyfél dokumentációjában](http://github.com/StackExchange/StackExchange.Redis#documentation) talál.</span><span class="sxs-lookup"><span data-stu-id="f2bbd-102">For more information, see the [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) github page and  the [StackExchange.Redis cache client documentation](http://github.com/StackExchange/StackExchange.Redis#documentation).</span></span>
> 
> 

<span data-ttu-id="f2bbd-103">Egy ügyfélalkalmazás a Visual Studióban a StackExchange.Redis NuGet-csomag használatával történő konfigurálásához kattintson a jobb gombbal a projektre a **Solution Explorer** (Megoldáskezelő) felületén, majd válassza a **Manage NuGet Packages** (NuGet-csomagok kezelése) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f2bbd-103">To configure a client application in Visual Studio using the StackExchange.Redis NuGet package, right-click the project in **Solution Explorer** and choose **Manage NuGet Packages**.</span></span> 

![NuGet-csomagok kezelése](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

<span data-ttu-id="f2bbd-105">Írja be a **StackExchange.Redis** vagy a **StackExchange.Redis.StrongName** kifejezést a keresőmezőbe, az eredmények közül válassza ki a kívánt verziót, majd kattintson az **Install** (Telepítés) gombra.</span><span class="sxs-lookup"><span data-stu-id="f2bbd-105">Type **StackExchange.Redis** or **StackExchange.Redis.StrongName** into the search text box, select the desired version from the results, and click **Install**.</span></span>

> [!NOTE]
> <span data-ttu-id="f2bbd-106">Ha inkább a **StackExchange.Redis** ügyfélkönyvtár erős elnevezésű verzióját kívánja használni, válassza a **StackExchange.Redis.StrongName**, ellenkező esetben pedig a **StackExchange.Redis** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f2bbd-106">If you prefer to use a strong-named version of the **StackExchange.Redis** client library, choose **StackExchange.Redis.StrongName**; otherwise choose **StackExchange.Redis**.</span></span>
> 
> 

![StackExchange.Redis NuGet-csomag](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

<span data-ttu-id="f2bbd-108">A NuGet-csomag letölti és hozzáadja az ügyfélalkalmazás számára szükséges szerelvényhivatkozásokat az Azure Redis Cache a StackExchange.Redis gyorsítótárügyféllel történő eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="f2bbd-108">The NuGet package downloads and adds the required assembly references for your client application to access Azure Redis Cache with the StackExchange.Redis cache client.</span></span>

> [!NOTE]
> <span data-ttu-id="f2bbd-109">Ha a projektet korábban a StackExchange.Redis használatára konfigurálta, a **NuGet-csomagkezelőben** ellenőrizheti, hogy elérhető-e új frissítés a csomaghoz.</span><span class="sxs-lookup"><span data-stu-id="f2bbd-109">If you have previously configured your project to use StackExchange.Redis, you can check for updates to the package from the **NuGet Package Manager**.</span></span> <span data-ttu-id="f2bbd-110">A StackExchange.Redis NuGet-csomag frissített verzióit a **NuGet-csomagkezelő** ablakában az **Updates** (Frissítések) elemre kattintva érheti el és telepítheti.</span><span class="sxs-lookup"><span data-stu-id="f2bbd-110">To check for and install updated versions of the StackExchange.Redis NuGet package, click **Updates** in the the **NuGet Package Manager** window.</span></span> <span data-ttu-id="f2bbd-111">Ha a StackExchange.Redis NuGet-csomaghoz elérhetővé válik egy frissítés, frissítheti a projektjét is, hogy az a csomag frissített verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="f2bbd-111">If an update to the StackExchange.Redis NuGet package is available, you can update your project to use the updated version.</span></span>
> 
> 

<span data-ttu-id="f2bbd-112">Úgy is telepítheti a StackExchange.Redis NuGet csomagot, hogy a **NuGet Package Manager** (NuGet-csomagkezelő) > **Package Manager Console** (Csomagkezelő konzol) elemre kattint a **Tools** (Eszközök) menüben, és a következő parancsot futtatja a **Package Manager Console** (Csomagkezelő konzol) ablakból.</span><span class="sxs-lookup"><span data-stu-id="f2bbd-112">You can also install the StackExchange.Redis NuGet package by clicking **NuGet Package Manager**, **Package Manager Console** from the **Tools** menu, and running the following command from the **Package Manager Console** window.</span></span>
    
```
Install-Package StackExchange.Redis
```
