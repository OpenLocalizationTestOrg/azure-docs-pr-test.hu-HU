---
title: "Az Azure Redis Cache használata Pythonnal | Microsoft Docs"
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
ms.openlocfilehash: cdbee52574d0ffbe82ef3dc98f2848f4d00ba2ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-redis-cache-with-python"></a><span data-ttu-id="02fc9-103">Az Azure Redis Cache használata Pythonnal</span><span class="sxs-lookup"><span data-stu-id="02fc9-103">How to use Azure Redis Cache with Python</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="02fc9-104">.NET</span><span class="sxs-lookup"><span data-stu-id="02fc9-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="02fc9-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="02fc9-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="02fc9-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="02fc9-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="02fc9-107">Java</span><span class="sxs-lookup"><span data-stu-id="02fc9-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="02fc9-108">Python</span><span class="sxs-lookup"><span data-stu-id="02fc9-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="02fc9-109">Ez a témakör segítséget nyújt az első lépések megtételében az Azure Redis Cache és a Python használatakor.</span><span class="sxs-lookup"><span data-stu-id="02fc9-109">This topic shows you how to get started with Azure Redis Cache using Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="02fc9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="02fc9-110">Prerequisites</span></span>
<span data-ttu-id="02fc9-111">Telepítse a [redis-py](https://github.com/andymccurdy/redis-py) ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="02fc9-111">Install [redis-py](https://github.com/andymccurdy/redis-py).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="02fc9-112">Redis Cache gyorsítótár létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="02fc9-112">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a><span data-ttu-id="02fc9-113">Állomásnév és hívóbetűk lekérése</span><span class="sxs-lookup"><span data-stu-id="02fc9-113">Retrieve the host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="enable-the-non-ssl-endpoint"></a><span data-ttu-id="02fc9-114">Nem SSL végpont engedélyezése</span><span class="sxs-lookup"><span data-stu-id="02fc9-114">Enable the non-SSL endpoint</span></span>
<span data-ttu-id="02fc9-115">Egyes Redis-ügyfelek nem támogatják az SSL-t, és alapértelmezés szerint a [nem SSL port le van tiltva az új Azure Redis Cache-példányokban](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="02fc9-115">Some Redis clients don't support SSL, and by default the [non-SSL port is disabled for new Azure Redis Cache instances](cache-configure.md#access-ports).</span></span> <span data-ttu-id="02fc9-116">Az oktatóanyag összeállításakor a [redis-py](https://github.com/andymccurdy/redis-py) nem támogatja az SSL-t.</span><span class="sxs-lookup"><span data-stu-id="02fc9-116">At the time of this writing, the [redis-py](https://github.com/andymccurdy/redis-py) client doesn't support SSL.</span></span> 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-to-the-cache-and-retrieve-it"></a><span data-ttu-id="02fc9-117">Elemek hozzáadása és lekérése a gyorsítótárból</span><span class="sxs-lookup"><span data-stu-id="02fc9-117">Add something to the cache and retrieve it</span></span>
    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


<span data-ttu-id="02fc9-118">Cserélje le a `<name>` elemet a gyorsítótár nevére, és a `key` elemet a hívóbetűre.</span><span class="sxs-lookup"><span data-stu-id="02fc9-118">Replace `<name>` with your cache name and `key` with your access key.</span></span>

<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
