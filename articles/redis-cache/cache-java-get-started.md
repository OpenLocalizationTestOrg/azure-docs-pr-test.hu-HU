---
title: Azure Redis Cache Java aaaHow toouse |} Microsoft Docs
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
ms.openlocfilehash: 7768e879d71f61585b59cf4bd6634ba3f12e001d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-redis-cache-with-java"></a><span data-ttu-id="26345-103">Hogyan toouse Azure Redis Cache-gyorsítótár Java</span><span class="sxs-lookup"><span data-stu-id="26345-103">How toouse Azure Redis Cache with Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="26345-104">.NET</span><span class="sxs-lookup"><span data-stu-id="26345-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="26345-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="26345-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="26345-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="26345-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="26345-107">Java</span><span class="sxs-lookup"><span data-stu-id="26345-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="26345-108">Python</span><span class="sxs-lookup"><span data-stu-id="26345-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="26345-109">Azure Redis Cache hozzáférést tud biztosítani dedikált tooa Redis gyorsítótár, a Microsoft kezeli.</span><span class="sxs-lookup"><span data-stu-id="26345-109">Azure Redis Cache gives you access tooa dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="26345-110">A gyorsítótár a Microsoft Azure összes alkalmazásából elérhető.</span><span class="sxs-lookup"><span data-stu-id="26345-110">Your cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="26345-111">Ez a témakör bemutatja, hogyan tooget el az Azure Redis Cache Java használatával.</span><span class="sxs-lookup"><span data-stu-id="26345-111">This topic shows you how tooget started with Azure Redis Cache using Java.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26345-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="26345-112">Prerequisites</span></span>
<span data-ttu-id="26345-113">[Jedis](https://github.com/xetorthio/jedis) - Java-ügyfél a Redishez</span><span class="sxs-lookup"><span data-stu-id="26345-113">[Jedis](https://github.com/xetorthio/jedis) - Java client for Redis</span></span>

<span data-ttu-id="26345-114">Ez az oktatóanyag a Jedis használatát mutatja be, de a [http://redis.io/clients](http://redis.io/clients) webhelyen felsorolt Java-ügyfelek bármelyike használható.</span><span class="sxs-lookup"><span data-stu-id="26345-114">This tutorial uses Jedis, but you can use any Java client listed at [http://redis.io/clients](http://redis.io/clients).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="26345-115">Redis Cache gyorsítótár létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="26345-115">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a><span data-ttu-id="26345-116">Hello állomás neve vagy a hozzáférési kulcsok beolvasása</span><span class="sxs-lookup"><span data-stu-id="26345-116">Retrieve hello host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-toohello-cache-securely-using-ssl"></a><span data-ttu-id="26345-117">Csatlakozás toohello gyorsítótár biztonságosan az SSL használata</span><span class="sxs-lookup"><span data-stu-id="26345-117">Connect toohello cache securely using SSL</span></span>
<span data-ttu-id="26345-118">hello legújabb-buildekről [jedis](https://github.com/xetorthio/jedis) támogatást nyújt a Redis Cache tooAzure kapcsolódás SSL használatával.</span><span class="sxs-lookup"><span data-stu-id="26345-118">hello latest builds of [jedis](https://github.com/xetorthio/jedis) provide support for connecting tooAzure Redis Cache using SSL.</span></span> <span data-ttu-id="26345-119">hello a következő példa bemutatja, hogyan tooconnect tooAzure Redis Cache segítségével hello 6380 az SSL-végponton.</span><span class="sxs-lookup"><span data-stu-id="26345-119">hello following example shows how tooconnect tooAzure Redis Cache using hello SSL endpoint of 6380.</span></span> <span data-ttu-id="26345-120">Cserélje le `<name>` hello nevet, a gyorsítótár és `<key>` sem az elsődleges vagy másodlagos kulcsot a hello előző [lekéréséhez hello állomás neve vagy a hozzáférési kulcsok](#retrieve-the-host-name-and-access-keys) szakasz.</span><span class="sxs-lookup"><span data-stu-id="26345-120">Replace `<name>` with hello name of your cache and `<key>` with either your primary or secondary key as described in hello previous [Retrieve hello host name and access keys](#retrieve-the-host-name-and-access-keys) section.</span></span>

    boolean useSsl = true;
    /* In this line, replace <name> with your cache name: */
    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */

> [!NOTE]
> <span data-ttu-id="26345-121">hello nem SSL port az új Azure Redis Cache példány le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="26345-121">hello non-SSL port is disabled for new Azure Redis Cache instances.</span></span> <span data-ttu-id="26345-122">Ha egy másik ügyféltől, amely nem támogatja az SSL használ, tekintse meg [hogyan tooenable hello nem SSL port](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="26345-122">If you are using a different client that doesn't support SSL, see [How tooenable hello non-SSL port](cache-configure.md#access-ports).</span></span>
> 
> 

## <a name="add-something-toohello-cache-and-retrieve-it"></a><span data-ttu-id="26345-123">Hozzáadás toohello gyorsítótárazása és lekéréséhez</span><span class="sxs-lookup"><span data-stu-id="26345-123">Add something toohello cache and retrieve it</span></span>
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


## <a name="next-steps"></a><span data-ttu-id="26345-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="26345-124">Next steps</span></span>
* <span data-ttu-id="26345-125">[Gyorsítótár-diagnosztika engedélyezése](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) így [figyelő](https://msdn.microsoft.com/library/azure/dn763945.aspx) hello a gyorsítótár állapotát.</span><span class="sxs-lookup"><span data-stu-id="26345-125">[Enable cache diagnostics](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) so you can [monitor](https://msdn.microsoft.com/library/azure/dn763945.aspx) hello health of your cache.</span></span>
* <span data-ttu-id="26345-126">Olvasási hello hivatalos [dokumentáció Redis](http://redis.io/documentation).</span><span class="sxs-lookup"><span data-stu-id="26345-126">Read hello official [Redis documentation](http://redis.io/documentation).</span></span>
