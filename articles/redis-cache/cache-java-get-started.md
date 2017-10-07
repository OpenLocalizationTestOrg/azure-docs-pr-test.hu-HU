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
# <a name="how-toouse-azure-redis-cache-with-java"></a>Hogyan toouse Azure Redis Cache-gyorsítótár Java
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis Cache hozzáférést tud biztosítani dedikált tooa Redis gyorsítótár, a Microsoft kezeli. A gyorsítótár a Microsoft Azure összes alkalmazásából elérhető.

Ez a témakör bemutatja, hogyan tooget el az Azure Redis Cache Java használatával.

## <a name="prerequisites"></a>Előfeltételek
[Jedis](https://github.com/xetorthio/jedis) - Java-ügyfél a Redishez

Ez az oktatóanyag a Jedis használatát mutatja be, de a [http://redis.io/clients](http://redis.io/clients) webhelyen felsorolt Java-ügyfelek bármelyike használható.

## <a name="create-a-redis-cache-on-azure"></a>Redis Cache gyorsítótár létrehozása az Azure-ban
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a>Hello állomás neve vagy a hozzáférési kulcsok beolvasása
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-toohello-cache-securely-using-ssl"></a>Csatlakozás toohello gyorsítótár biztonságosan az SSL használata
hello legújabb-buildekről [jedis](https://github.com/xetorthio/jedis) támogatást nyújt a Redis Cache tooAzure kapcsolódás SSL használatával. hello a következő példa bemutatja, hogyan tooconnect tooAzure Redis Cache segítségével hello 6380 az SSL-végponton. Cserélje le `<name>` hello nevet, a gyorsítótár és `<key>` sem az elsődleges vagy másodlagos kulcsot a hello előző [lekéréséhez hello állomás neve vagy a hozzáférési kulcsok](#retrieve-the-host-name-and-access-keys) szakasz.

    boolean useSsl = true;
    /* In this line, replace <name> with your cache name: */
    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */

> [!NOTE]
> hello nem SSL port az új Azure Redis Cache példány le van tiltva. Ha egy másik ügyféltől, amely nem támogatja az SSL használ, tekintse meg [hogyan tooenable hello nem SSL port](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-toohello-cache-and-retrieve-it"></a>Hozzáadás toohello gyorsítótárazása és lekéréséhez
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


## <a name="next-steps"></a>Következő lépések
* [Gyorsítótár-diagnosztika engedélyezése](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) így [figyelő](https://msdn.microsoft.com/library/azure/dn763945.aspx) hello a gyorsítótár állapotát.
* Olvasási hello hivatalos [dokumentáció Redis](http://redis.io/documentation).
