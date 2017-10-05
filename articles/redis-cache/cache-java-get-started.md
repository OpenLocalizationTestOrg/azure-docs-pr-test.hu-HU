---
title: "Az Azure Redis Cache használata Javával | Microsoft Docs"
description: "Bevezetés az Azure Redis Cache és a Java együttes használatába"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 29275a5e-2e39-4ef2-804f-7ecc5161eab9
ms.service: cache
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 04/13/2017
ms.author: sdanie
ms.openlocfilehash: 3cfad3a7279b5f9bbff1e6cd9794c492e3544752
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-redis-cache-with-java"></a><span data-ttu-id="baae5-103">Az Azure Redis Cache használata Javával</span><span class="sxs-lookup"><span data-stu-id="baae5-103">How to use Azure Redis Cache with Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="baae5-104">.NET</span><span class="sxs-lookup"><span data-stu-id="baae5-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="baae5-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="baae5-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="baae5-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="baae5-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="baae5-107">Java</span><span class="sxs-lookup"><span data-stu-id="baae5-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="baae5-108">Python</span><span class="sxs-lookup"><span data-stu-id="baae5-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="baae5-109">Az Azure Redis Cache hozzáférést biztosít egy dedikált Redis Cache gyorsítótárhoz, amelyet a Microsoft felügyel.</span><span class="sxs-lookup"><span data-stu-id="baae5-109">Azure Redis Cache gives you access to a dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="baae5-110">A gyorsítótár a Microsoft Azure összes alkalmazásából elérhető.</span><span class="sxs-lookup"><span data-stu-id="baae5-110">Your cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="baae5-111">Ez a témakör segítséget nyújt az első lépések megtételében a Javát alkalmazó Azure Redis Cache használatakor.</span><span class="sxs-lookup"><span data-stu-id="baae5-111">This topic shows you how to get started with Azure Redis Cache using Java.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="baae5-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="baae5-112">Prerequisites</span></span>
<span data-ttu-id="baae5-113">[Jedis](https://github.com/xetorthio/jedis) - Java-ügyfél a Redishez</span><span class="sxs-lookup"><span data-stu-id="baae5-113">[Jedis](https://github.com/xetorthio/jedis) - Java client for Redis</span></span>

<span data-ttu-id="baae5-114">Ez az oktatóanyag a Jedis használatát mutatja be, de a [http://redis.io/clients](http://redis.io/clients) webhelyen felsorolt Java-ügyfelek bármelyike használható.</span><span class="sxs-lookup"><span data-stu-id="baae5-114">This tutorial uses Jedis, but you can use any Java client listed at [http://redis.io/clients](http://redis.io/clients).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="baae5-115">Redis Cache gyorsítótár létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="baae5-115">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a><span data-ttu-id="baae5-116">Állomásnév és hívóbetűk lekérése</span><span class="sxs-lookup"><span data-stu-id="baae5-116">Retrieve the host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a><span data-ttu-id="baae5-117">Biztonságos csatlakozás a gyorsítótárhoz SSL használatával</span><span class="sxs-lookup"><span data-stu-id="baae5-117">Connect to the cache securely using SSL</span></span>
<span data-ttu-id="baae5-118">A [jedis](https://github.com/xetorthio/jedis) legújabb buildjei támogatást nyújtanak ahhoz, ha SSL-lel szeretne kapcsolódni az Azure Redis Cache-hez.</span><span class="sxs-lookup"><span data-stu-id="baae5-118">The latest builds of [jedis](https://github.com/xetorthio/jedis) provide support for connecting to Azure Redis Cache using SSL.</span></span> <span data-ttu-id="baae5-119">Az alábbi példa bemutatja, hogyan csatlakozhat az Azure Redis Cache-hez a 6380-as SSL-végpont használatával.</span><span class="sxs-lookup"><span data-stu-id="baae5-119">The following example shows how to connect to Azure Redis Cache using the SSL endpoint of 6380.</span></span> <span data-ttu-id="baae5-120">A `<name>` helyére a gyorsítótár nevét, a `<key>` helyére pedig az előző, [Állomásnév és hívóbetűk lekérése](#retrieve-the-host-name-and-access-keys) című szakaszban ismertetett elsődleges vagy másodlagos kulcsot írja be.</span><span class="sxs-lookup"><span data-stu-id="baae5-120">Replace `<name>` with the name of your cache and `<key>` with either your primary or secondary key as described in the previous [Retrieve the host name and access keys](#retrieve-the-host-name-and-access-keys) section.</span></span>

    boolean useSsl = true;
    /* In this line, replace <name> with your cache name: */
    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */

> [!NOTE]
> <span data-ttu-id="baae5-121">A nem SSL-port le van tiltva az új Azure Redis Cache-példányokban.</span><span class="sxs-lookup"><span data-stu-id="baae5-121">The non-SSL port is disabled for new Azure Redis Cache instances.</span></span> <span data-ttu-id="baae5-122">Ha az SSL-t nem támogató, egyéb ügyfelet használ, tekintse meg a következőt: [A nem SSL-port engedélyezése](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="baae5-122">If you are using a different client that doesn't support SSL, see [How to enable the non-SSL port](cache-configure.md#access-ports).</span></span>
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a><span data-ttu-id="baae5-123">Elemek hozzáadása és lekérése a gyorsítótárból</span><span class="sxs-lookup"><span data-stu-id="baae5-123">Add something to the cache and retrieve it</span></span>
    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    public class App
    {
      public static void main( String[] args )
      {
        boolean useSsl = true;
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a><span data-ttu-id="baae5-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="baae5-124">Next steps</span></span>
* <span data-ttu-id="baae5-125">[Engedélyezze a gyorsítótár-diagnosztikát,](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) hogy [megfigyelhesse](https://msdn.microsoft.com/library/azure/dn763945.aspx) a gyorsítótár állapotát.</span><span class="sxs-lookup"><span data-stu-id="baae5-125">[Enable cache diagnostics](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) so you can [monitor](https://msdn.microsoft.com/library/azure/dn763945.aspx) the health of your cache.</span></span>
* <span data-ttu-id="baae5-126">Olvassa el a hivatalos [Redis dokumentációt](http://redis.io/documentation).</span><span class="sxs-lookup"><span data-stu-id="baae5-126">Read the official [Redis documentation](http://redis.io/documentation).</span></span>
