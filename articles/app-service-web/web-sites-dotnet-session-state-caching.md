---
title: "az Azure Redis cache az Azure App Service aaaSession állapota"
description: "Ismerje meg, hogyan toouse hello Azure Cache Service toosupport az ASP.NET munkamenet-állapotának gyorsítótárazásához."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
manager: erikre
editor: none
ms.assetid: 4f98d289-2698-464d-85cd-99ec40fad1f2
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 06/27/2016
ms.author: riande
ms.openlocfilehash: f689b6754ea072aa195f822ab6482f4bf2748375
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a><span data-ttu-id="49d6b-103">Munkamenet-állapot az Azure Redis Cache-ben, az Azure App Service szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="49d6b-103">Session state with Azure Redis cache in Azure App Service</span></span>
<span data-ttu-id="49d6b-104">Ez a témakör azt ismerteti, hogyan toouse hello Azure Redis Cache szolgáltatás a munkamenet-állapot.</span><span class="sxs-lookup"><span data-stu-id="49d6b-104">This topic explains how toouse hello Azure Redis Cache Service for session state.</span></span>

<span data-ttu-id="49d6b-105">Ha az ASP.NET webalkalmazás munkamenet-állapotot használ, akkor tooconfigure egy külső állapotszolgáltatót (hello Redis Cache Service vagy egy SQL Server munkamenetállapot-szolgáltatóját).</span><span class="sxs-lookup"><span data-stu-id="49d6b-105">If your ASP.NET web app uses session state, you will need tooconfigure an external session state provider (either hello Redis Cache Service or a SQL Server session state provider).</span></span> <span data-ttu-id="49d6b-106">Munkamenet-állapotot használ, és nem használ külső szolgáltatót, ha a webalkalmazás korlátozott tooone példányát fogja.</span><span class="sxs-lookup"><span data-stu-id="49d6b-106">If you use session state, and don't use an external provider, you will be limited tooone instance of your web app.</span></span> <span data-ttu-id="49d6b-107">hello Redis Cache Service hello leggyorsabb és legegyszerűbb tooenable.</span><span class="sxs-lookup"><span data-stu-id="49d6b-107">hello Redis Cache Service is hello fastest and simplest tooenable.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="49d6b-108"><a id="createcache"></a>Hello gyorsítótár létrehozása</span><span class="sxs-lookup"><span data-stu-id="49d6b-108"><a id="createcache"></a>Create hello Cache</span></span>
<span data-ttu-id="49d6b-109">Hajtsa végre a [ezt az útmutatót](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) toocreate hello gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="49d6b-109">Follow [these directions](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) toocreate hello cache.</span></span>

## <span data-ttu-id="49d6b-110"><a id="configureproject"></a>Hello RedisSessionStateProvider NuGet csomag tooyour webalkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="49d6b-110"><a id="configureproject"></a>Add hello RedisSessionStateProvider NuGet package tooyour web app</span></span>
<span data-ttu-id="49d6b-111">Telepítse a hello NuGet `RedisSessionStateProvider` csomag.</span><span class="sxs-lookup"><span data-stu-id="49d6b-111">Install hello NuGet `RedisSessionStateProvider` package.</span></span>  <span data-ttu-id="49d6b-112">Használjon hello következő parancsot a tooinstall hello package manager konzolról (**eszközök** > **NuGet-Csomagkezelő** > **Csomagkezelő konzol**):</span><span class="sxs-lookup"><span data-stu-id="49d6b-112">Use hello following command tooinstall from hello package manager console (**Tools** > **NuGet Package Manager** > **Package Manager Console**):</span></span>

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`

<span data-ttu-id="49d6b-113">a tooinstall **eszközök** > **NuGet-Csomagkezelő** > **Nuget-csomagok kezelése megoldáshoz**, keressen `RedisSessionStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="49d6b-113">tooinstall from **Tools** > **NuGet Package Manager** > **Manage NugGet Packages for Solution**, search for `RedisSessionStateProvider`.</span></span>

<span data-ttu-id="49d6b-114">További információ: hello [NuGet RedisSessionStateProvider oldal](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) és [hello gyorsítótárügyfél konfigurálása](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).</span><span class="sxs-lookup"><span data-stu-id="49d6b-114">For more information see hello [NuGet RedisSessionStateProvider page](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) and [Configure hello cache client](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).</span></span>

## <span data-ttu-id="49d6b-115"><a id="configurewebconfig"></a>Hello Web.Config fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="49d6b-115"><a id="configurewebconfig"></a>Modify hello Web.Config File</span></span>
<span data-ttu-id="49d6b-116">Ezenkívül toomaking szerelvény által hivatkozott gyorsítótár, a NuGet-csomag hello csonkbejegyzéseket ad a hello *web.config* fájlt.</span><span class="sxs-lookup"><span data-stu-id="49d6b-116">In addition toomaking assembly references for Cache, hello NuGet package adds stub entries in hello *web.config* file.</span></span> 

1. <span data-ttu-id="49d6b-117">Nyissa meg hello *web.config* és hello hello található **sessionState** elemet.</span><span class="sxs-lookup"><span data-stu-id="49d6b-117">Open hello *web.config* and find hello hello **sessionState** element.</span></span>
2. <span data-ttu-id="49d6b-118">Adja meg a hello értékeket `host`, `accessKey`, `port` (hello SSL-port legyen 6380), és állítsa be `SSL` túl`true`.</span><span class="sxs-lookup"><span data-stu-id="49d6b-118">Enter hello values for `host`, `accessKey`, `port` (hello SSL port should be 6380), and set `SSL` too`true`.</span></span> <span data-ttu-id="49d6b-119">Ezek az értékek hello szerezhető [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) a gyorsítótárpéldány panelen.</span><span class="sxs-lookup"><span data-stu-id="49d6b-119">These values can be obtained from hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) blade for your cache instance.</span></span> <span data-ttu-id="49d6b-120">További információkért lásd: [toohello gyorsítótár csatlakozás](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache).</span><span class="sxs-lookup"><span data-stu-id="49d6b-120">For more information, see [Connect toohello cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache).</span></span> <span data-ttu-id="49d6b-121">Vegye figyelembe, hogy az új gyorsítótárakhoz alapértelmezés szerint le van-e tiltva hello nem SSL port.</span><span class="sxs-lookup"><span data-stu-id="49d6b-121">Note that hello non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="49d6b-122">Hello nem SSL port engedélyezésével kapcsolatos további információkért lásd: hello [hozzáférési portok](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) hello szakasz [gyorsítótár konfigurálása az Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="49d6b-122">For more information about enabling hello non-SSL port, see hello [Access Ports](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) section in hello [Configure a cache in Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) topic.</span></span> <span data-ttu-id="49d6b-123">hello következő kódban láthatók hello módosítások toohello *web.config* fájlt, kifejezetten hello módosítások túl*port*, *állomás*, accessKey *, és *ssl*.</span><span class="sxs-lookup"><span data-stu-id="49d6b-123">hello following markup shows hello changes toohello *web.config* file, specifically hello changes too*port*, *host*, accessKey*, and *ssl*.</span></span>
   
          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;

## <span data-ttu-id="49d6b-124"><a id="usesessionobject"></a>Hello munkamenet-objektum használata a kódban</span><span class="sxs-lookup"><span data-stu-id="49d6b-124"><a id="usesessionobject"></a> Use hello Session Object in Code</span></span>
<span data-ttu-id="49d6b-125">utolsó lépésként hello toobegin hello munkamenet-objektum használatát az ASP.NET-kódban.</span><span class="sxs-lookup"><span data-stu-id="49d6b-125">hello final step is toobegin using hello Session object in your ASP.NET code.</span></span> <span data-ttu-id="49d6b-126">Objektumok toosession állapot hello segítségével adja hozzá **Session.Add** metódust.</span><span class="sxs-lookup"><span data-stu-id="49d6b-126">You add objects toosession state by using hello **Session.Add** method.</span></span> <span data-ttu-id="49d6b-127">Ez a metódus kulcs-érték párok toostore elemeket használja a hello munkamenetállapot-gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="49d6b-127">This method uses key-value pairs toostore items in hello session state cache.</span></span>

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

<span data-ttu-id="49d6b-128">hello a következő kód lekéri ezt az értéket a munkamenet-állapotból.</span><span class="sxs-lookup"><span data-stu-id="49d6b-128">hello following code retrieves this value from session state.</span></span>

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue;    

<span data-ttu-id="49d6b-129">A webes alkalmazás is használhatja hello Redis Cache toocache objektumok.</span><span class="sxs-lookup"><span data-stu-id="49d6b-129">You can also use hello Redis Cache toocache objects in your web app.</span></span> <span data-ttu-id="49d6b-130">További információk: [MVC movie app with Azure Redis Cache in 15 minutes](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/) (MVC filmalkalmazás 15 perc alatt az Azure Redis Cache segítségével).</span><span class="sxs-lookup"><span data-stu-id="49d6b-130">For more info, see [MVC movie app with Azure Redis Cache in 15 minutes](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).</span></span>
<span data-ttu-id="49d6b-131">További részletes információt toouse az ASP.NET munkamenet-állapot, lásd: [ASP.NET munkamenet-állapot – áttekintés][ASP.NET Session State Overview].</span><span class="sxs-lookup"><span data-stu-id="49d6b-131">For more details about how toouse ASP.NET session state, see [ASP.NET Session State Overview][ASP.NET Session State Overview].</span></span>

> [!NOTE]
> <span data-ttu-id="49d6b-132">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="49d6b-132">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="49d6b-133">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="49d6b-133">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="49d6b-134">A változások</span><span class="sxs-lookup"><span data-stu-id="49d6b-134">What's changed</span></span>
* <span data-ttu-id="49d6b-135">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="49d6b-135">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>
  
  <span data-ttu-id="49d6b-136">*Szerző: [Rick Anderson](https://twitter.com/RickAndMSFT)*</span><span class="sxs-lookup"><span data-stu-id="49d6b-136">*By [Rick Anderson](https://twitter.com/RickAndMSFT)*</span></span>

[installed hello latest]: http://www.windowsazure.com/downloads/?sdk=net  
[ASP.NET Session State Overview]: http://msdn.microsoft.com/library/ms178581.aspx

[NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
[NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
[CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
[NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
[OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
[CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
[EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
[ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png

