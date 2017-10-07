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
# <a name="how-toouse-azure-redis-cache-with-nodejs"></a>Hogyan toouse Azure Redis Cache-gyorsítótár a Node.js
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis Cache által biztosított hozzáférés tooa biztonságos, dedikált Redis gyorsítótár, a Microsoft kezeli. A gyorsítótár a Microsoft Azure összes alkalmazásából elérhető.

Ez a témakör bemutatja, hogyan tooget el az Azure Redis Cache Node.js segítségével. Egy másik példa az Azure Redis Cache használatára Node.js környezetben: [Node.js-csevegőalkalmazás létrehozása a Socket.IO segítségével egy Azure-webhelyen](../app-service-web/web-sites-nodejs-chat-app-socketio.md).

## <a name="prerequisites"></a>Előfeltételek
Telepítse a [node_redis](https://github.com/mranney/node_redis) ügyfelet:

    npm install redis

Ez az oktatóanyag a [node_redis](https://github.com/mranney/node_redis) ügyfelet használja. Példák a más Node.js-ügyfél használatával, a dokumentációban hello egyedi hello Node.js ügyfelek megtalálható a [Node.js Redis-ügyfelek](http://redis.io/clients#nodejs).

## <a name="create-a-redis-cache-on-azure"></a>Redis Cache gyorsítótár létrehozása az Azure-ban
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a>Hello állomás neve vagy a hozzáférési kulcsok beolvasása
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-toohello-cache-securely-using-ssl"></a>Csatlakozás toohello gyorsítótár biztonságosan az SSL használata
hello legújabb-buildekről [node_redis](https://github.com/mranney/node_redis) támogatást nyújt a Redis Cache tooAzure kapcsolódás SSL használatával. hello a következő példa bemutatja, hogyan tooconnect tooAzure Redis Cache segítségével hello 6380 az SSL-végponton. Cserélje le `<name>` hello nevet, a gyorsítótár és `<key>` sem az elsődleges vagy másodlagos kulcsot a hello előző [lekéréséhez hello állomás neve vagy a hozzáférési kulcsok](#retrieve-the-host-name-and-access-keys) szakasz.

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> hello nem SSL port az új Azure Redis Cache példány le van tiltva. Ha egy másik ügyféltől, amely nem támogatja az SSL használ, tekintse meg [hogyan tooenable hello nem SSL port](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-toohello-cache-and-retrieve-it"></a>Hozzáadás toohello gyorsítótárazása és lekéréséhez
a következő példa azt mutatja meg, hogyan tooconnect tooan Azure Redis gyorsítótár példányt, és tárolásához és lekéréséhez elem hello gyorsítótárból hello. További példák a Redis használata hello [node_redis](https://github.com/mranney/node_redis) ügyfél, lásd: [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });

    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Kimenet:

    OK
    value


## <a name="next-steps"></a>Következő lépések
* [Gyorsítótár-diagnosztika engedélyezése](cache-how-to-monitor.md#enable-cache-diagnostics) így [figyelő](cache-how-to-monitor.md) hello a gyorsítótár állapotát.
* Olvasási hello hivatalos [dokumentáció Redis](http://redis.io/documentation).

