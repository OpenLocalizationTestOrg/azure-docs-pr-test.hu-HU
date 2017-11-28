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
# <a name="how-toouse-azure-redis-cache-with-python"></a><span data-ttu-id="1d94f-103">Hogyan toouse Azure Redis Cache-gyorsítótár Python</span><span class="sxs-lookup"><span data-stu-id="1d94f-103">How toouse Azure Redis Cache with Python</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1d94f-104">.NET</span><span class="sxs-lookup"><span data-stu-id="1d94f-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="1d94f-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1d94f-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="1d94f-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="1d94f-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="1d94f-107">Java</span><span class="sxs-lookup"><span data-stu-id="1d94f-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="1d94f-108">Python</span><span class="sxs-lookup"><span data-stu-id="1d94f-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="1d94f-109">Ez a témakör bemutatja, hogyan tooget el az Azure Redis Cache pythonos környezetekben.</span><span class="sxs-lookup"><span data-stu-id="1d94f-109">This topic shows you how tooget started with Azure Redis Cache using Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d94f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1d94f-110">Prerequisites</span></span>
<span data-ttu-id="1d94f-111">Telepítse a [redis-py](https://github.com/andymccurdy/redis-py) ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="1d94f-111">Install [redis-py](https://github.com/andymccurdy/redis-py).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="1d94f-112">Redis Cache gyorsítótár létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="1d94f-112">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a><span data-ttu-id="1d94f-113">Hello állomás neve vagy a hozzáférési kulcsok beolvasása</span><span class="sxs-lookup"><span data-stu-id="1d94f-113">Retrieve hello host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="enable-hello-non-ssl-endpoint"></a><span data-ttu-id="1d94f-114">Hello-SSL végpont engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1d94f-114">Enable hello non-SSL endpoint</span></span>
<span data-ttu-id="1d94f-115">Néhány Redis-ügyfelek nem rendelkezik SSL-támogatással, és az alapértelmezett hello [nem SSL port le van tiltva, az új Azure Redis Cache példány](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="1d94f-115">Some Redis clients don't support SSL, and by default hello [non-SSL port is disabled for new Azure Redis Cache instances](cache-configure.md#access-ports).</span></span> <span data-ttu-id="1d94f-116">Írásának hello időpontban hello [redis-másolása](https://github.com/andymccurdy/redis-py) ügyfél nem támogatja az SSL.</span><span class="sxs-lookup"><span data-stu-id="1d94f-116">At hello time of this writing, hello [redis-py](https://github.com/andymccurdy/redis-py) client doesn't support SSL.</span></span> 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-toohello-cache-and-retrieve-it"></a><span data-ttu-id="1d94f-117">Hozzáadás toohello gyorsítótárazása és lekéréséhez</span><span class="sxs-lookup"><span data-stu-id="1d94f-117">Add something toohello cache and retrieve it</span></span>
    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


<span data-ttu-id="1d94f-118">Cserélje le a `<name>` elemet a gyorsítótár nevére, és a `key` elemet a hívóbetűre.</span><span class="sxs-lookup"><span data-stu-id="1d94f-118">Replace `<name>` with your cache name and `key` with your access key.</span></span>

<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
