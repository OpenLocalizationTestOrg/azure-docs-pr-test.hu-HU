---
title: "Az Azure Redis Cache használata a Node.js környezettel | Microsoft Docs"
description: "Bevezetés az Azure Redis Cache használatába a Node.js és a node_redis alkalmazásával."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: v-lincan
ms.assetid: 06fddc95-8029-4a8d-83f5-ebd5016891d9
ms.service: cache
ms.devlang: nodejs
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
ms.openlocfilehash: 530191637b1aa91ee1d7fe5b5bb032c60983f7dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-redis-cache-with-nodejs"></a><span data-ttu-id="8fb79-103">Az Azure Redis Cache használata a Node.js környezettel</span><span class="sxs-lookup"><span data-stu-id="8fb79-103">How to use Azure Redis Cache with Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8fb79-104">.NET</span><span class="sxs-lookup"><span data-stu-id="8fb79-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="8fb79-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8fb79-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="8fb79-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="8fb79-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="8fb79-107">Java</span><span class="sxs-lookup"><span data-stu-id="8fb79-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="8fb79-108">Python</span><span class="sxs-lookup"><span data-stu-id="8fb79-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="8fb79-109">Az Azure Redis Cache hozzáférést biztosít egy biztonságos, dedikált Redis Cache gyorsítótárhoz, amelyet a Microsoft felügyel.</span><span class="sxs-lookup"><span data-stu-id="8fb79-109">Azure Redis Cache gives you access to a secure, dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="8fb79-110">A gyorsítótár a Microsoft Azure összes alkalmazásából elérhető.</span><span class="sxs-lookup"><span data-stu-id="8fb79-110">Your cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="8fb79-111">Ez a témakör segítséget nyújt az első lépések megtételében az Azure Redis Cache használatakor Node.js környezetben.</span><span class="sxs-lookup"><span data-stu-id="8fb79-111">This topic shows you how to get started with Azure Redis Cache using Node.js.</span></span> <span data-ttu-id="8fb79-112">Egy másik példa az Azure Redis Cache használatára Node.js környezetben: [Node.js-csevegőalkalmazás létrehozása a Socket.IO segítségével egy Azure-webhelyen](../app-service-web/web-sites-nodejs-chat-app-socketio.md).</span><span class="sxs-lookup"><span data-stu-id="8fb79-112">For another example of using Azure Redis Cache with Node.js, see [Build a Node.js Chat Application with Socket.IO on an Azure Website](../app-service-web/web-sites-nodejs-chat-app-socketio.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8fb79-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8fb79-113">Prerequisites</span></span>
<span data-ttu-id="8fb79-114">Telepítse a [node_redis](https://github.com/mranney/node_redis) ügyfelet:</span><span class="sxs-lookup"><span data-stu-id="8fb79-114">Install [node_redis](https://github.com/mranney/node_redis):</span></span>

    npm install redis

<span data-ttu-id="8fb79-115">Ez az oktatóanyag a [node_redis](https://github.com/mranney/node_redis) ügyfelet használja.</span><span class="sxs-lookup"><span data-stu-id="8fb79-115">This tutorial uses [node_redis](https://github.com/mranney/node_redis).</span></span> <span data-ttu-id="8fb79-116">Az egyéb Node.js-ügyfeleket használó példákért tekintse meg az egyes Node.js-ügyfelek dokumentációját a [Node.js Redis-ügyfeleket](http://redis.io/clients#nodejs) felsoroló weblapon.</span><span class="sxs-lookup"><span data-stu-id="8fb79-116">For examples of using other Node.js clients, see the individual documentation for the Node.js clients listed at [Node.js Redis clients](http://redis.io/clients#nodejs).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="8fb79-117">Redis Cache gyorsítótár létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="8fb79-117">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a><span data-ttu-id="8fb79-118">Állomásnév és hívóbetűk lekérése</span><span class="sxs-lookup"><span data-stu-id="8fb79-118">Retrieve the host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a><span data-ttu-id="8fb79-119">Biztonságos csatlakozás a gyorsítótárhoz SSL használatával</span><span class="sxs-lookup"><span data-stu-id="8fb79-119">Connect to the cache securely using SSL</span></span>
<span data-ttu-id="8fb79-120">A [node_redis](https://github.com/mranney/node_redis) legújabb buildjei támogatást nyújtanak az Azure Redis Cache-hez SSL használatával való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="8fb79-120">The latest builds of [node_redis](https://github.com/mranney/node_redis) provide support for connecting to Azure Redis Cache using SSL.</span></span> <span data-ttu-id="8fb79-121">Az alábbi példa bemutatja, hogyan csatlakozhat az Azure Redis Cache-hez a 6380-as SSL-végpont használatával.</span><span class="sxs-lookup"><span data-stu-id="8fb79-121">The following example shows how to connect to Azure Redis Cache using the SSL endpoint of 6380.</span></span> <span data-ttu-id="8fb79-122">A `<name>` helyére a gyorsítótár nevét, a `<key>` helyére pedig az előző, [Állomásnév és hívóbetűk lekérése](#retrieve-the-host-name-and-access-keys) című szakaszban ismertetett elsődleges vagy másodlagos kulcsot írja be.</span><span class="sxs-lookup"><span data-stu-id="8fb79-122">Replace `<name>` with the name of your cache and `<key>` with either your primary or secondary key as described in the previous [Retrieve the host name and access keys](#retrieve-the-host-name-and-access-keys) section.</span></span>

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> <span data-ttu-id="8fb79-123">A nem SSL-port le van tiltva az új Azure Redis Cache-példányokban.</span><span class="sxs-lookup"><span data-stu-id="8fb79-123">The non-SSL port is disabled for new Azure Redis Cache instances.</span></span> <span data-ttu-id="8fb79-124">Ha az SSL-t nem támogató, egyéb ügyfelet használ, tekintse meg a következőt: [A nem SSL-port engedélyezése](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="8fb79-124">If you are using a different client that doesn't support SSL, see [How to enable the non-SSL port](cache-configure.md#access-ports).</span></span>
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a><span data-ttu-id="8fb79-125">Elemek hozzáadása és lekérése a gyorsítótárból</span><span class="sxs-lookup"><span data-stu-id="8fb79-125">Add something to the cache and retrieve it</span></span>
<span data-ttu-id="8fb79-126">Az alábbi példa bemutatja, hogyan csatlakozhat egy Azure Redis Cache-példányhoz, valamint hogyan menthet egy elemet a gyorsítótárban, majd kérheti le azt onnan.</span><span class="sxs-lookup"><span data-stu-id="8fb79-126">The following example shows you how to connect to an Azure Redis Cache instance, and store and retrieve an item from the cache.</span></span> <span data-ttu-id="8fb79-127">További példák a Redis használatára a [node_redis](https://github.com/mranney/node_redis) ügyféllel: [http://redis.js.org/](http://redis.js.org/).</span><span class="sxs-lookup"><span data-stu-id="8fb79-127">For more examples of using Redis with the [node_redis](https://github.com/mranney/node_redis) client, see [http://redis.js.org/](http://redis.js.org/).</span></span>

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });

    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

<span data-ttu-id="8fb79-128">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="8fb79-128">Output:</span></span>

    OK
    value


## <a name="next-steps"></a><span data-ttu-id="8fb79-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8fb79-129">Next steps</span></span>
* <span data-ttu-id="8fb79-130">[Engedélyezze a gyorsítótár-diagnosztikát,](cache-how-to-monitor.md#enable-cache-diagnostics) hogy [megfigyelhesse](cache-how-to-monitor.md) a gyorsítótár állapotát.</span><span class="sxs-lookup"><span data-stu-id="8fb79-130">[Enable cache diagnostics](cache-how-to-monitor.md#enable-cache-diagnostics) so you can [monitor](cache-how-to-monitor.md) the health of your cache.</span></span>
* <span data-ttu-id="8fb79-131">Olvassa el a hivatalos [Redis dokumentációt](http://redis.io/documentation).</span><span class="sxs-lookup"><span data-stu-id="8fb79-131">Read the official [Redis documentation](http://redis.io/documentation).</span></span>

