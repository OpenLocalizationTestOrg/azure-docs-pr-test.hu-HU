---
title: "Gyorsítótár az ASP.NET kimeneti gyorsítótár-szolgáltató"
description: "Útmutató: Azure Redis Cache használatának ASP.NET lap kimeneti gyorsítótár"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 78469a66-0829-484f-8660-b2598ec60fbf
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/14/2017
ms.author: sdanie
ms.openlocfilehash: 845f25637a0e48460fc76c1ee36060274b3cec38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a><span data-ttu-id="a45f7-103">Az ASP.NET kimeneti gyorsítótár-szolgáltató az Azure Redis gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="a45f7-103">ASP.NET Output Cache Provider for Azure Redis Cache</span></span>
<span data-ttu-id="a45f7-104">A Redis kimeneti gyorsítótár-szolgáltató egy olyan folyamaton-tárolási mechanizmus a kimeneti gyorsítótár adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="a45f7-104">The Redis Output Cache Provider is an out-of-process storage mechanism for output cache data.</span></span> <span data-ttu-id="a45f7-105">Ezek az adatok kifejezetten a teljes HTTP-válaszok van (a kimeneti gyorsítótár oldalon).</span><span class="sxs-lookup"><span data-stu-id="a45f7-105">This data is specifically for full HTTP responses (page output caching).</span></span> <span data-ttu-id="a45f7-106">A szolgáltató csatlakozik az új kimeneti gyorsítótár szolgáltató bővítési pontot, az ASP.NET 4 lett bevezetve.</span><span class="sxs-lookup"><span data-stu-id="a45f7-106">The provider plugs into the new output cache provider extensibility point that was introduced in ASP.NET 4.</span></span>

<span data-ttu-id="a45f7-107">A kimeneti gyorsítótár Redis szolgáltatót használja, először konfigurálja a gyorsítótárhoz, és konfigurálja az ASP.NET-alkalmazás a Redis kimeneti gyorsítótár szolgáltató NuGet csomag segítségével.</span><span class="sxs-lookup"><span data-stu-id="a45f7-107">To use the Redis Output Cache Provider, first configure your cache, and then configure your ASP.NET application using the Redis Output Cache Provider NuGet package.</span></span> <span data-ttu-id="a45f7-108">Ez a témakör az alkalmazás használhatja a Redis kimeneti gyorsítótár-szolgáltató konfigurálása nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="a45f7-108">This topic provides guidance on configuring your application to use the Redis Output Cache Provider.</span></span> <span data-ttu-id="a45f7-109">Létrehozásával és az Azure Redis Cache példány konfigurálásával kapcsolatos további információkért lásd: [gyorsítótár létrehozásához](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span><span class="sxs-lookup"><span data-stu-id="a45f7-109">For more information about creating and configuring an Azure Redis Cache instance, see [Create a cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span></span>

## <a name="store-aspnet-page-output-in-the-cache"></a><span data-ttu-id="a45f7-110">ASP.NET lap kimenete tárolja a gyorsítótárban</span><span class="sxs-lookup"><span data-stu-id="a45f7-110">Store ASP.NET page output in the cache</span></span>
<span data-ttu-id="a45f7-111">Ügyfélalkalmazás konfigurálása a Visual Studio használatával a Redis gyorsítótár munkamenet állapota NuGet-csomagot, kattintson a **NuGet-Csomagkezelő**, **Csomagkezelő konzol** a a **eszközök** menü.</span><span class="sxs-lookup"><span data-stu-id="a45f7-111">To configure a client application in Visual Studio using the Redis Cache Session State NuGet package, click **NuGet Package Manager**, **Package Manager Console** from the **Tools** menu.</span></span>

<span data-ttu-id="a45f7-112">Futtassa az alábbi parancsot a `Package Manager Console` ablakából.</span><span class="sxs-lookup"><span data-stu-id="a45f7-112">Run the following command from the `Package Manager Console` window.</span></span>
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

<span data-ttu-id="a45f7-113">A Redis kimeneti gyorsítótár szolgáltató NuGet-csomagot a StackExchange.Redis.StrongName csomag függőség rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a45f7-113">The Redis Output Cache Provider NuGet package has a dependency on the StackExchange.Redis.StrongName package.</span></span> <span data-ttu-id="a45f7-114">Ha a StackExchange.Redis.StrongName csomag nincs jelen a projektben, telepíti a rendszer.</span><span class="sxs-lookup"><span data-stu-id="a45f7-114">If the StackExchange.Redis.StrongName package is not present in your project, it is installed.</span></span> <span data-ttu-id="a45f7-115">A Redis kimeneti gyorsítótár szolgáltató NuGet-csomag kapcsolatos további információkért tekintse meg a [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet lap.</span><span class="sxs-lookup"><span data-stu-id="a45f7-115">For more information about the Redis Output Cache Provider NuGet package, see the [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet page.</span></span>

>[!NOTE]
><span data-ttu-id="a45f7-116">Az erős névvel ellátott StackExchange.Redis.StrongName csomag mellett is van a StackExchange.Redis nem-erős névvel ellátott verzióra.</span><span class="sxs-lookup"><span data-stu-id="a45f7-116">In addition to the strong-named StackExchange.Redis.StrongName package, there is also the StackExchange.Redis non-strong-named version.</span></span> <span data-ttu-id="a45f7-117">A projekt távolítsa el a nem-erős névvel ellátott StackExchange.Redis verzióját használja, ha más módon meg beolvasása ütközése a projektben.</span><span class="sxs-lookup"><span data-stu-id="a45f7-117">If your project is using the non-strong-named StackExchange.Redis version you must uninstall it, otherwise you get naming conflicts in your project.</span></span> <span data-ttu-id="a45f7-118">Ezeket a csomagokat kapcsolatos további információkért lásd: [konfigurálása .NET-gyorsítótárazási ügyfelek számára](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).</span><span class="sxs-lookup"><span data-stu-id="a45f7-118">For more information about these packages, see [Configure .NET cache clients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).</span></span>
>
>

<span data-ttu-id="a45f7-119">A NuGet-csomagot tölti le, és hozzáadja a szükséges összeállítási referenciát, és hozzáadja az alábbi szakasz a web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="a45f7-119">The NuGet package downloads and adds the required assembly references and adds the following section into your web.config file.</span></span> <span data-ttu-id="a45f7-120">Ez a szakasz a kötelező beállítani az ASP.NET-alkalmazás a Redis kimeneti gyorsítótár-szolgáltató használatára.</span><span class="sxs-lookup"><span data-stu-id="a45f7-120">This section contains the required configuration for your ASP.NET application to use the Redis Output Cache Provider.</span></span>

```xml
<caching>
  <outputCachedefault Provider="MyRedisOutputCache">
    <providers>
      <!--
      <add name="MyRedisOutputCache"
        host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "5000" [number]
      />
      -->
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
    </providers>
  </outputCache>
</caching>
```

<span data-ttu-id="a45f7-121">A megjegyzésként szakasz és a minta-beállítások minden attribútum példaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="a45f7-121">The commented section provides an example of the attributes and sample settings for each attribute.</span></span>

<span data-ttu-id="a45f7-122">Az attribútumok konfigurálása a Microsoft Azure-portálon a gyorsítótár paneljét értékeivel, és konfigurálja a többi érték tetszés szerint.</span><span class="sxs-lookup"><span data-stu-id="a45f7-122">Configure the attributes with the values from your cache blade in the Microsoft Azure portal, and configure the other values as desired.</span></span> <span data-ttu-id="a45f7-123">A gyorsítótár tulajdonságai elérése, lásd: [konfigurálása Redis gyorsítótár beállításainak](cache-configure.md#configure-redis-cache-settings).</span><span class="sxs-lookup"><span data-stu-id="a45f7-123">For instructions on accessing your cache properties, see [Configure Redis cache settings](cache-configure.md#configure-redis-cache-settings).</span></span>

* <span data-ttu-id="a45f7-124">**állomás** – adja meg a gyorsítótár végpontjához.</span><span class="sxs-lookup"><span data-stu-id="a45f7-124">**host** – specify your cache endpoint.</span></span>
* <span data-ttu-id="a45f7-125">**port** – használja a nem SSL port vagy az SSL-port, attól függően, hogy az ssl-beállítások.</span><span class="sxs-lookup"><span data-stu-id="a45f7-125">**port** – use either your non-SSL port or your SSL port, depending on the ssl settings.</span></span>
* <span data-ttu-id="a45f7-126">**accessKey** – az elsődleges vagy másodlagos kulcsot használja a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="a45f7-126">**accessKey** – use either the primary or secondary key for your cache.</span></span>
* <span data-ttu-id="a45f7-127">**SSL** – igaz, ha azt szeretné, hogy biztonságos SSL/gyorsítótárügyfél kommunikációhoz; ellenkező esetben hamis.</span><span class="sxs-lookup"><span data-stu-id="a45f7-127">**ssl** – true if you want to secure cache/client communications with ssl; otherwise false.</span></span> <span data-ttu-id="a45f7-128">Mindenképp adja meg a megfelelő porthoz.</span><span class="sxs-lookup"><span data-stu-id="a45f7-128">Be sure to specify the correct port.</span></span>
  * <span data-ttu-id="a45f7-129">A nem SSL port az új gyorsítótárakhoz alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="a45f7-129">The non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="a45f7-130">Adja meg az IGAZ állítja ezt a beállítást, az SSL-port használatára.</span><span class="sxs-lookup"><span data-stu-id="a45f7-130">Specify true for this setting to use the SSL port.</span></span> <span data-ttu-id="a45f7-131">A nem SSL port engedélyezésével kapcsolatos további információkért lásd: a [hozzáférési portok](cache-configure.md#access-ports) szakasz a [gyorsítótár konfigurálása](cache-configure.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="a45f7-131">For more information about enabling the non-SSL port, see the [Access Ports](cache-configure.md#access-ports) section in the [Configure a cache](cache-configure.md) topic.</span></span>
* <span data-ttu-id="a45f7-132">**databaseId** – megadott gyorsítótár használandó adatbázis kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="a45f7-132">**databaseId** – Specified which database to use for cache output data.</span></span> <span data-ttu-id="a45f7-133">Ha nincs megadva, az alapértelmezett érték a 0 szolgál.</span><span class="sxs-lookup"><span data-stu-id="a45f7-133">If not specified, the default value of 0 is used.</span></span>
* <span data-ttu-id="a45f7-134">**applicationName** – kulcsok vannak tárolva, a redis `<AppName>_<SessionId>_Data`.</span><span class="sxs-lookup"><span data-stu-id="a45f7-134">**applicationName** – Keys are stored in redis as `<AppName>_<SessionId>_Data`.</span></span> <span data-ttu-id="a45f7-135">Az elnevezési sémát lehetővé teszi, hogy több alkalmazás közösen ugyanazzal a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="a45f7-135">This naming scheme enables multiple applications to share the same key.</span></span> <span data-ttu-id="a45f7-136">Ez a paraméter nem kötelező, és ha nem ad meg, egy alapértelmezett értéket használja.</span><span class="sxs-lookup"><span data-stu-id="a45f7-136">This parameter is optional and if you do not provide it a default value is used.</span></span>
* <span data-ttu-id="a45f7-137">**connectionTimeoutInMilliseconds** – Ez a beállítás lehetővé teszi a StackExchange.Redis-ügyfélben connectTimeout-beállításának felülbírálása.</span><span class="sxs-lookup"><span data-stu-id="a45f7-137">**connectionTimeoutInMilliseconds** – This setting allows you to override the connectTimeout setting in the StackExchange.Redis client.</span></span> <span data-ttu-id="a45f7-138">Ha nincs megadva, az alapértelmezett connectTimeout 5000 beállítással.</span><span class="sxs-lookup"><span data-stu-id="a45f7-138">If not specified, the default connectTimeout setting of 5000 is used.</span></span> <span data-ttu-id="a45f7-139">További információkért lásd: [StackExchange.Redis konfigurációs modell](http://go.microsoft.com/fwlink/?LinkId=398705).</span><span class="sxs-lookup"><span data-stu-id="a45f7-139">For more information, see [StackExchange.Redis configuration model](http://go.microsoft.com/fwlink/?LinkId=398705).</span></span>
* <span data-ttu-id="a45f7-140">**operationTimeoutInMilliseconds** – Ez a beállítás lehetővé teszi a StackExchange.Redis-ügyfélben syncTimeout-beállításának felülbírálása.</span><span class="sxs-lookup"><span data-stu-id="a45f7-140">**operationTimeoutInMilliseconds** – This setting allows you to override the syncTimeout setting in the StackExchange.Redis client.</span></span> <span data-ttu-id="a45f7-141">Ha nincs megadva, az alapértelmezett syncTimeout 1000 beállítással.</span><span class="sxs-lookup"><span data-stu-id="a45f7-141">If not specified, the default syncTimeout setting of 1000 is used.</span></span> <span data-ttu-id="a45f7-142">További információkért lásd: [StackExchange.Redis konfigurációs modell](http://go.microsoft.com/fwlink/?LinkId=398705).</span><span class="sxs-lookup"><span data-stu-id="a45f7-142">For more information, see [StackExchange.Redis configuration model](http://go.microsoft.com/fwlink/?LinkId=398705).</span></span>

<span data-ttu-id="a45f7-143">Minden oldalon, amelynek a kimeneti gyorsítótár kívánja hozzáadása az OutputCache direktívában.</span><span class="sxs-lookup"><span data-stu-id="a45f7-143">Add an OutputCache directive to each page for which you wish to cache the output.</span></span>

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

<span data-ttu-id="a45f7-144">A korábbi példában a gyorsítótárazott oldaladatokat a gyorsítótárban marad 60 másodpercen keresztül, és minden egyes paraméter kombináció gyorsítótárazza a lap egy másik verziója.</span><span class="sxs-lookup"><span data-stu-id="a45f7-144">In the previous example, the cached page data remains in the cache for 60 seconds, and a different version of the page is cached for each parameter combination.</span></span> <span data-ttu-id="a45f7-145">Az OutputCache direktívában kapcsolatos további információkért lásd: [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).</span><span class="sxs-lookup"><span data-stu-id="a45f7-145">For more information about the OutputCache directive, see [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).</span></span>

<span data-ttu-id="a45f7-146">A lépések elvégzése után az alkalmazás a Redis kimeneti gyorsítótár-szolgáltató használatára van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="a45f7-146">Once these steps are performed, your application is configured to use the Redis Output Cache Provider.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a45f7-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a45f7-147">Next steps</span></span>
<span data-ttu-id="a45f7-148">Tekintse meg a [ASP.NET munkamenetállapot-szolgáltatóját az Azure Redis Cache](cache-aspnet-session-state-provider.md).</span><span class="sxs-lookup"><span data-stu-id="a45f7-148">Check out the [ASP.NET Session State Provider for Azure Redis Cache](cache-aspnet-session-state-provider.md).</span></span>

