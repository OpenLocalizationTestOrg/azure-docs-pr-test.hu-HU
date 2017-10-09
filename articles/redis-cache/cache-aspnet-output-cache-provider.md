---
title: "az ASP.NET kimeneti gyorsítótár-szolgáltató aaaCache"
description: "Megtudhatja, hogyan toocache az ASP.NET kimeneti Azure Redis Cache használata"
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
ms.openlocfilehash: fc38cc657604b351f55ad8febac383783ac29700
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a><span data-ttu-id="28bba-103">Az ASP.NET kimeneti gyorsítótár-szolgáltató az Azure Redis gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="28bba-103">ASP.NET Output Cache Provider for Azure Redis Cache</span></span>
<span data-ttu-id="28bba-104">hello Redis kimeneti gyorsítótár-szolgáltató egy olyan folyamaton-tárolási mechanizmus a kimeneti gyorsítótár adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="28bba-104">hello Redis Output Cache Provider is an out-of-process storage mechanism for output cache data.</span></span> <span data-ttu-id="28bba-105">Ezek az adatok kifejezetten a teljes HTTP-válaszok van (a kimeneti gyorsítótár oldalon).</span><span class="sxs-lookup"><span data-stu-id="28bba-105">This data is specifically for full HTTP responses (page output caching).</span></span> <span data-ttu-id="28bba-106">hello szolgáltató számítógépekbe hello új kimeneti gyorsítótár szolgáltató bővítési pontot az ASP.NET 4 lett bevezetve.</span><span class="sxs-lookup"><span data-stu-id="28bba-106">hello provider plugs into hello new output cache provider extensibility point that was introduced in ASP.NET 4.</span></span>

<span data-ttu-id="28bba-107">toouse hello Redis kimeneti gyorsítótár-szolgáltató, először konfigurálja a gyorsítótárhoz, és adja meg az ASP.NET-alkalmazás hello Redis kimeneti gyorsítótár szolgáltató NuGet csomag segítségével.</span><span class="sxs-lookup"><span data-stu-id="28bba-107">toouse hello Redis Output Cache Provider, first configure your cache, and then configure your ASP.NET application using hello Redis Output Cache Provider NuGet package.</span></span> <span data-ttu-id="28bba-108">Ez a témakör az alkalmazás toouse hello Redis kimeneti gyorsítótár-szolgáltató konfigurálása nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="28bba-108">This topic provides guidance on configuring your application toouse hello Redis Output Cache Provider.</span></span> <span data-ttu-id="28bba-109">Létrehozásával és az Azure Redis Cache példány konfigurálásával kapcsolatos további információkért lásd: [gyorsítótár létrehozásához](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span><span class="sxs-lookup"><span data-stu-id="28bba-109">For more information about creating and configuring an Azure Redis Cache instance, see [Create a cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span></span>

## <a name="store-aspnet-page-output-in-hello-cache"></a><span data-ttu-id="28bba-110">ASP.NET lap kimenete tárolása hello gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="28bba-110">Store ASP.NET page output in hello cache</span></span>
<span data-ttu-id="28bba-111">tooconfigure egy ügyfélalkalmazást, a Visual Studio használatával hello Redis gyorsítótár munkamenet állapota NuGet-csomagot, kattintson a **NuGet-Csomagkezelő**, **Csomagkezelő konzol** a hello **Eszközök** menü.</span><span class="sxs-lookup"><span data-stu-id="28bba-111">tooconfigure a client application in Visual Studio using hello Redis Cache Session State NuGet package, click **NuGet Package Manager**, **Package Manager Console** from hello **Tools** menu.</span></span>

<span data-ttu-id="28bba-112">Futtatási hello következő parancsot a hello `Package Manager Console` ablak.</span><span class="sxs-lookup"><span data-stu-id="28bba-112">Run hello following command from hello `Package Manager Console` window.</span></span>
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

<span data-ttu-id="28bba-113">hello Redis kimeneti gyorsítótár szolgáltató NuGet-csomag függőségi hello StackExchange.Redis.StrongName csomag kapcsolatban van.</span><span class="sxs-lookup"><span data-stu-id="28bba-113">hello Redis Output Cache Provider NuGet package has a dependency on hello StackExchange.Redis.StrongName package.</span></span> <span data-ttu-id="28bba-114">Hello StackExchange.Redis.StrongName csomag nincs jelen a projektben, ha telepíti a rendszer.</span><span class="sxs-lookup"><span data-stu-id="28bba-114">If hello StackExchange.Redis.StrongName package is not present in your project, it is installed.</span></span> <span data-ttu-id="28bba-115">Hello Redis kimeneti gyorsítótár szolgáltató NuGet-csomag kapcsolatos további információkért lásd: hello [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet lap.</span><span class="sxs-lookup"><span data-stu-id="28bba-115">For more information about hello Redis Output Cache Provider NuGet package, see hello [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet page.</span></span>

>[!NOTE]
><span data-ttu-id="28bba-116">Továbbá toohello erős névvel ellátott StackExchange.Redis.StrongName csomagban van is hello StackExchange.Redis nem-erős névvel ellátott verzió.</span><span class="sxs-lookup"><span data-stu-id="28bba-116">In addition toohello strong-named StackExchange.Redis.StrongName package, there is also hello StackExchange.Redis non-strong-named version.</span></span> <span data-ttu-id="28bba-117">Ha projektjéhez hello nem-erős névvel ellátott StackExchange.Redis verzióját el kell távolítani a használ, ellenkező esetben azt lekérése ütközése a projekt.</span><span class="sxs-lookup"><span data-stu-id="28bba-117">If your project is using hello non-strong-named StackExchange.Redis version you must uninstall it, otherwise you get naming conflicts in your project.</span></span> <span data-ttu-id="28bba-118">Ezeket a csomagokat kapcsolatos további információkért lásd: [konfigurálása .NET-gyorsítótárazási ügyfelek számára](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).</span><span class="sxs-lookup"><span data-stu-id="28bba-118">For more information about these packages, see [Configure .NET cache clients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).</span></span>
>
>

<span data-ttu-id="28bba-119">hello NuGet csomag tölti le, és hozzáadja a hello szerelvény hivatkozik, és hozzáadja a következő szakasz a web.config fájlba hello szükséges.</span><span class="sxs-lookup"><span data-stu-id="28bba-119">hello NuGet package downloads and adds hello required assembly references and adds hello following section into your web.config file.</span></span> <span data-ttu-id="28bba-120">Ez a szakasz az ASP.NET alkalmazás toouse hello Redis kimeneti gyorsítótár-szolgáltató hello kötelező beállítani.</span><span class="sxs-lookup"><span data-stu-id="28bba-120">This section contains hello required configuration for your ASP.NET application toouse hello Redis Output Cache Provider.</span></span>

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

<span data-ttu-id="28bba-121">hello megjegyzésként szakasz egy példát hello attribútumokat és minta beállításait minden attribútum.</span><span class="sxs-lookup"><span data-stu-id="28bba-121">hello commented section provides an example of hello attributes and sample settings for each attribute.</span></span>

<span data-ttu-id="28bba-122">A gyorsítótár paneljét hello Microsoft Azure-portálon hello értékeivel hello attribútumainak beállítása és konfigurálása hello tetszés szerint más értékek.</span><span class="sxs-lookup"><span data-stu-id="28bba-122">Configure hello attributes with hello values from your cache blade in hello Microsoft Azure portal, and configure hello other values as desired.</span></span> <span data-ttu-id="28bba-123">A gyorsítótár tulajdonságai elérése, lásd: [konfigurálása Redis gyorsítótár beállításainak](cache-configure.md#configure-redis-cache-settings).</span><span class="sxs-lookup"><span data-stu-id="28bba-123">For instructions on accessing your cache properties, see [Configure Redis cache settings](cache-configure.md#configure-redis-cache-settings).</span></span>

* <span data-ttu-id="28bba-124">**állomás** – adja meg a gyorsítótár végpontjához.</span><span class="sxs-lookup"><span data-stu-id="28bba-124">**host** – specify your cache endpoint.</span></span>
* <span data-ttu-id="28bba-125">**port** – használja a nem SSL port vagy az SSL-port, attól függően, hogy hello ssl-beállítások.</span><span class="sxs-lookup"><span data-stu-id="28bba-125">**port** – use either your non-SSL port or your SSL port, depending on hello ssl settings.</span></span>
* <span data-ttu-id="28bba-126">**accessKey** – a gyorsítótár vagy hello elsődleges vagy másodlagos kulcsot használja.</span><span class="sxs-lookup"><span data-stu-id="28bba-126">**accessKey** – use either hello primary or secondary key for your cache.</span></span>
* <span data-ttu-id="28bba-127">**SSL** – igaz, ha azt szeretné, hogy toosecure gyorsítótár vagy ügyfél-kommunikáció SSL; ellenkező esetben hamis.</span><span class="sxs-lookup"><span data-stu-id="28bba-127">**ssl** – true if you want toosecure cache/client communications with ssl; otherwise false.</span></span> <span data-ttu-id="28bba-128">Lehet, hogy toospecify hello portjának.</span><span class="sxs-lookup"><span data-stu-id="28bba-128">Be sure toospecify hello correct port.</span></span>
  * <span data-ttu-id="28bba-129">hello nem SSL port az új gyorsítótárakhoz alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="28bba-129">hello non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="28bba-130">Adja meg ezt a beállítást toouse hello SSL-port igaz.</span><span class="sxs-lookup"><span data-stu-id="28bba-130">Specify true for this setting toouse hello SSL port.</span></span> <span data-ttu-id="28bba-131">Hello nem SSL port engedélyezésével kapcsolatos további információkért lásd: hello [hozzáférési portok](cache-configure.md#access-ports) hello szakasz [gyorsítótár konfigurálása](cache-configure.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="28bba-131">For more information about enabling hello non-SSL port, see hello [Access Ports](cache-configure.md#access-ports) section in hello [Configure a cache](cache-configure.md) topic.</span></span>
* <span data-ttu-id="28bba-132">**databaseId** – megadott melyik adatbázis toouse gyorsítótár kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="28bba-132">**databaseId** – Specified which database toouse for cache output data.</span></span> <span data-ttu-id="28bba-133">Ha nincs megadva, hello alapértelmezett érték 0 lesz érvényben.</span><span class="sxs-lookup"><span data-stu-id="28bba-133">If not specified, hello default value of 0 is used.</span></span>
* <span data-ttu-id="28bba-134">**applicationName** – kulcsok vannak tárolva, a redis `<AppName>_<SessionId>_Data`.</span><span class="sxs-lookup"><span data-stu-id="28bba-134">**applicationName** – Keys are stored in redis as `<AppName>_<SessionId>_Data`.</span></span> <span data-ttu-id="28bba-135">Az elnevezési sémát lehetővé teszi, hogy több alkalmazások tooshare hello ugyanazzal a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="28bba-135">This naming scheme enables multiple applications tooshare hello same key.</span></span> <span data-ttu-id="28bba-136">Ez a paraméter nem kötelező, és ha nem ad meg, egy alapértelmezett értéket használja.</span><span class="sxs-lookup"><span data-stu-id="28bba-136">This parameter is optional and if you do not provide it a default value is used.</span></span>
* <span data-ttu-id="28bba-137">**connectionTimeoutInMilliseconds** – Ez a beállítás lehetővé teszi toooverride hello connectTimeout hello StackExchange.Redis ügyfél-ban történő beállítását.</span><span class="sxs-lookup"><span data-stu-id="28bba-137">**connectionTimeoutInMilliseconds** – This setting allows you toooverride hello connectTimeout setting in hello StackExchange.Redis client.</span></span> <span data-ttu-id="28bba-138">Ha nincs megadva, hello alapértelmezett connectTimeout 5000 beállítással.</span><span class="sxs-lookup"><span data-stu-id="28bba-138">If not specified, hello default connectTimeout setting of 5000 is used.</span></span> <span data-ttu-id="28bba-139">További információkért lásd: [StackExchange.Redis konfigurációs modell](http://go.microsoft.com/fwlink/?LinkId=398705).</span><span class="sxs-lookup"><span data-stu-id="28bba-139">For more information, see [StackExchange.Redis configuration model](http://go.microsoft.com/fwlink/?LinkId=398705).</span></span>
* <span data-ttu-id="28bba-140">**operationTimeoutInMilliseconds** – Ez a beállítás lehetővé teszi toooverride hello syncTimeout hello StackExchange.Redis ügyfél-ban történő beállítását.</span><span class="sxs-lookup"><span data-stu-id="28bba-140">**operationTimeoutInMilliseconds** – This setting allows you toooverride hello syncTimeout setting in hello StackExchange.Redis client.</span></span> <span data-ttu-id="28bba-141">Ha nincs megadva, hello alapértelmezett syncTimeout 1000 beállítással.</span><span class="sxs-lookup"><span data-stu-id="28bba-141">If not specified, hello default syncTimeout setting of 1000 is used.</span></span> <span data-ttu-id="28bba-142">További információkért lásd: [StackExchange.Redis konfigurációs modell](http://go.microsoft.com/fwlink/?LinkId=398705).</span><span class="sxs-lookup"><span data-stu-id="28bba-142">For more information, see [StackExchange.Redis configuration model](http://go.microsoft.com/fwlink/?LinkId=398705).</span></span>

<span data-ttu-id="28bba-143">Adja hozzá az OutputCache direktíva tooeach lap, amelynek toocache hello kimeneti kívánja.</span><span class="sxs-lookup"><span data-stu-id="28bba-143">Add an OutputCache directive tooeach page for which you wish toocache hello output.</span></span>

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

<span data-ttu-id="28bba-144">Hello előző példában hello lap adatok marad hello gyorsítótár gyorsítótárazott 60 másodpercen keresztül, és minden paraméter kombináció gyorsítótárazza hello lap egy másik verziója.</span><span class="sxs-lookup"><span data-stu-id="28bba-144">In hello previous example, hello cached page data remains in hello cache for 60 seconds, and a different version of hello page is cached for each parameter combination.</span></span> <span data-ttu-id="28bba-145">Hello OutputCache direktíva kapcsolatos további információkért lásd: [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).</span><span class="sxs-lookup"><span data-stu-id="28bba-145">For more information about hello OutputCache directive, see [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).</span></span>

<span data-ttu-id="28bba-146">A lépések elvégzése után az alkalmazás konfigurált toouse hello Redis kimeneti gyorsítótár-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="28bba-146">Once these steps are performed, your application is configured toouse hello Redis Output Cache Provider.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28bba-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="28bba-147">Next steps</span></span>
<span data-ttu-id="28bba-148">Tekintse meg a hello [ASP.NET munkamenetállapot-szolgáltatóját az Azure Redis Cache](cache-aspnet-session-state-provider.md).</span><span class="sxs-lookup"><span data-stu-id="28bba-148">Check out hello [ASP.NET Session State Provider for Azure Redis Cache](cache-aspnet-session-state-provider.md).</span></span>

