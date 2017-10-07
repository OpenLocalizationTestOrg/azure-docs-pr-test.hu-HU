---
title: Azure Redis Cache Python aaaHow toouse |} Microsoft Docs
description: "Bevezetés az Azure Redis Cache használatába Python alkalmazásával"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: v-lincan
ms.assetid: f186202c-fdad-4398-af8c-aee91ec96ba3
ms.service: cache
ms.devlang: python
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
ms.openlocfilehash: 74c03eb4ce17ff3574595fd2bb37e399d71c6eb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-redis-cache-with-python"></a>Hogyan toouse Azure Redis Cache-gyorsítótár Python
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Ez a témakör bemutatja, hogyan tooget el az Azure Redis Cache pythonos környezetekben.

## <a name="prerequisites"></a>Előfeltételek
Telepítse a [redis-py](https://github.com/andymccurdy/redis-py) ügyfelet.

## <a name="create-a-redis-cache-on-azure"></a>Redis Cache gyorsítótár létrehozása az Azure-ban
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a>Hello állomás neve vagy a hozzáférési kulcsok beolvasása
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="enable-hello-non-ssl-endpoint"></a>Hello-SSL végpont engedélyezése
Néhány Redis-ügyfelek nem rendelkezik SSL-támogatással, és az alapértelmezett hello [nem SSL port le van tiltva, az új Azure Redis Cache példány](cache-configure.md#access-ports). Írásának hello időpontban hello [redis-másolása](https://github.com/andymccurdy/redis-py) ügyfél nem támogatja az SSL. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-toohello-cache-and-retrieve-it"></a>Hozzáadás toohello gyorsítótárazása és lekéréséhez
    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Cserélje le a `<name>` elemet a gyorsítótár nevére, és a `key` elemet a hívóbetűre.

<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
