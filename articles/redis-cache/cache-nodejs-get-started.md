---
title: Azure Redis Cache a Node.js aaaHow toouse |} Microsoft Docs
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
ms.openlocfilehash: dc8732041d2c4e5793e684e0c80b87a1c9d17f34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-redis-cache-with-nodejs"></a><span data-ttu-id="98fe5-103">Hogyan toouse Azure Redis Cache-gyorsítótár a Node.js</span><span class="sxs-lookup"><span data-stu-id="98fe5-103">How toouse Azure Redis Cache with Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="98fe5-104">.NET</span><span class="sxs-lookup"><span data-stu-id="98fe5-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="98fe5-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="98fe5-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="98fe5-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="98fe5-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="98fe5-107">Java</span><span class="sxs-lookup"><span data-stu-id="98fe5-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="98fe5-108">Python</span><span class="sxs-lookup"><span data-stu-id="98fe5-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="98fe5-109">Azure Redis Cache által biztosított hozzáférés tooa biztonságos, dedikált Redis gyorsítótár, a Microsoft kezeli.</span><span class="sxs-lookup"><span data-stu-id="98fe5-109">Azure Redis Cache gives you access tooa secure, dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="98fe5-110">A gyorsítótár a Microsoft Azure összes alkalmazásából elérhető.</span><span class="sxs-lookup"><span data-stu-id="98fe5-110">Your cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="98fe5-111">Ez a témakör bemutatja, hogyan tooget el az Azure Redis Cache Node.js segítségével.</span><span class="sxs-lookup"><span data-stu-id="98fe5-111">This topic shows you how tooget started with Azure Redis Cache using Node.js.</span></span> <span data-ttu-id="98fe5-112">Egy másik példa az Azure Redis Cache használatára Node.js környezetben: [Node.js-csevegőalkalmazás létrehozása a Socket.IO segítségével egy Azure-webhelyen](../app-service-web/web-sites-nodejs-chat-app-socketio.md).</span><span class="sxs-lookup"><span data-stu-id="98fe5-112">For another example of using Azure Redis Cache with Node.js, see [Build a Node.js Chat Application with Socket.IO on an Azure Website](../app-service-web/web-sites-nodejs-chat-app-socketio.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98fe5-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="98fe5-113">Prerequisites</span></span>
<span data-ttu-id="98fe5-114">Telepítse a [node_redis](https://github.com/mranney/node_redis) ügyfelet:</span><span class="sxs-lookup"><span data-stu-id="98fe5-114">Install [node_redis](https://github.com/mranney/node_redis):</span></span>

    npm install redis

<span data-ttu-id="98fe5-115">Ez az oktatóanyag a [node_redis](https://github.com/mranney/node_redis) ügyfelet használja.</span><span class="sxs-lookup"><span data-stu-id="98fe5-115">This tutorial uses [node_redis](https://github.com/mranney/node_redis).</span></span> <span data-ttu-id="98fe5-116">Példák a más Node.js-ügyfél használatával, a dokumentációban hello egyedi hello Node.js ügyfelek megtalálható a [Node.js Redis-ügyfelek](http://redis.io/clients#nodejs).</span><span class="sxs-lookup"><span data-stu-id="98fe5-116">For examples of using other Node.js clients, see hello individual documentation for hello Node.js clients listed at [Node.js Redis clients](http://redis.io/clients#nodejs).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="98fe5-117">Redis Cache gyorsítótár létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="98fe5-117">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a><span data-ttu-id="98fe5-118">Hello állomás neve vagy a hozzáférési kulcsok beolvasása</span><span class="sxs-lookup"><span data-stu-id="98fe5-118">Retrieve hello host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-toohello-cache-securely-using-ssl"></a><span data-ttu-id="98fe5-119">Csatlakozás toohello gyorsítótár biztonságosan az SSL használata</span><span class="sxs-lookup"><span data-stu-id="98fe5-119">Connect toohello cache securely using SSL</span></span>
<span data-ttu-id="98fe5-120">hello legújabb-buildekről [node_redis](https://github.com/mranney/node_redis) támogatást nyújt a Redis Cache tooAzure kapcsolódás SSL használatával.</span><span class="sxs-lookup"><span data-stu-id="98fe5-120">hello latest builds of [node_redis](https://github.com/mranney/node_redis) provide support for connecting tooAzure Redis Cache using SSL.</span></span> <span data-ttu-id="98fe5-121">hello a következő példa bemutatja, hogyan tooconnect tooAzure Redis Cache segítségével hello 6380 az SSL-végponton.</span><span class="sxs-lookup"><span data-stu-id="98fe5-121">hello following example shows how tooconnect tooAzure Redis Cache using hello SSL endpoint of 6380.</span></span> <span data-ttu-id="98fe5-122">Cserélje le `<name>` hello nevet, a gyorsítótár és `<key>` sem az elsődleges vagy másodlagos kulcsot a hello előző [lekéréséhez hello állomás neve vagy a hozzáférési kulcsok](#retrieve-the-host-name-and-access-keys) szakasz.</span><span class="sxs-lookup"><span data-stu-id="98fe5-122">Replace `<name>` with hello name of your cache and `<key>` with either your primary or secondary key as described in hello previous [Retrieve hello host name and access keys](#retrieve-the-host-name-and-access-keys) section.</span></span>

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> <span data-ttu-id="98fe5-123">hello nem SSL port az új Azure Redis Cache példány le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="98fe5-123">hello non-SSL port is disabled for new Azure Redis Cache instances.</span></span> <span data-ttu-id="98fe5-124">Ha egy másik ügyféltől, amely nem támogatja az SSL használ, tekintse meg [hogyan tooenable hello nem SSL port](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="98fe5-124">If you are using a different client that doesn't support SSL, see [How tooenable hello non-SSL port](cache-configure.md#access-ports).</span></span>
> 
> 

## <a name="add-something-toohello-cache-and-retrieve-it"></a><span data-ttu-id="98fe5-125">Hozzáadás toohello gyorsítótárazása és lekéréséhez</span><span class="sxs-lookup"><span data-stu-id="98fe5-125">Add something toohello cache and retrieve it</span></span>
<span data-ttu-id="98fe5-126">a következő példa azt mutatja meg, hogyan tooconnect tooan Azure Redis gyorsítótár példányt, és tárolásához és lekéréséhez elem hello gyorsítótárból hello.</span><span class="sxs-lookup"><span data-stu-id="98fe5-126">hello following example shows you how tooconnect tooan Azure Redis Cache instance, and store and retrieve an item from hello cache.</span></span> <span data-ttu-id="98fe5-127">További példák a Redis használata hello [node_redis](https://github.com/mranney/node_redis) ügyfél, lásd: [http://redis.js.org/](http://redis.js.org/).</span><span class="sxs-lookup"><span data-stu-id="98fe5-127">For more examples of using Redis with hello [node_redis](https://github.com/mranney/node_redis) client, see [http://redis.js.org/](http://redis.js.org/).</span></span>

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });

    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

<span data-ttu-id="98fe5-128">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="98fe5-128">Output:</span></span>

    OK
    value


## <a name="next-steps"></a><span data-ttu-id="98fe5-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98fe5-129">Next steps</span></span>
* <span data-ttu-id="98fe5-130">[Gyorsítótár-diagnosztika engedélyezése](cache-how-to-monitor.md#enable-cache-diagnostics) így [figyelő](cache-how-to-monitor.md) hello a gyorsítótár állapotát.</span><span class="sxs-lookup"><span data-stu-id="98fe5-130">[Enable cache diagnostics](cache-how-to-monitor.md#enable-cache-diagnostics) so you can [monitor](cache-how-to-monitor.md) hello health of your cache.</span></span>
* <span data-ttu-id="98fe5-131">Olvasási hello hivatalos [dokumentáció Redis](http://redis.io/documentation).</span><span class="sxs-lookup"><span data-stu-id="98fe5-131">Read hello official [Redis documentation](http://redis.io/documentation).</span></span>

