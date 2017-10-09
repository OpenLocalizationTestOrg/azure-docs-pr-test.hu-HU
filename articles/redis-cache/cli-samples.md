---
title: "aaaAzure CLI Redis gyorsítótár minták |} Microsoft Docs"
description: "Az Azure CLI-példák Azure Redis Cache."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 8d2de145-50c0-4f76-bf8f-fdf679f03698
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: a2eb6f7267951faca599a43ff29b46beb05fea6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-azure-redis-cache"></a><span data-ttu-id="f942a-103">Az Azure CLI-példák Azure Redis gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="f942a-103">Azure CLI Samples for Azure Redis Cache</span></span>

<span data-ttu-id="f942a-104">hello alábbi táblázat tartalmaz hivatkozásokat toobash parancsfájlokat hello Azure parancssori felület használatával készített.</span><span class="sxs-lookup"><span data-stu-id="f942a-104">hello following table includes links toobash scripts built using hello Azure CLI.</span></span>

| | |
|---|---|
|<span data-ttu-id="f942a-105">**Gyorsítótár létrehozása**</span><span class="sxs-lookup"><span data-stu-id="f942a-105">**Create cache**</span></span>||
| [<span data-ttu-id="f942a-106">A gyorsítótár létrehozása</span><span class="sxs-lookup"><span data-stu-id="f942a-106">Create a cache</span></span>](./scripts/create-cache.md) | <span data-ttu-id="f942a-107">Létrehoz egy erőforráscsoport és az Azure Redis Cache alapvető réteget.</span><span class="sxs-lookup"><span data-stu-id="f942a-107">Creates a resource group and a basic tier Azure Redis Cache.</span></span> |
| [<span data-ttu-id="f942a-108">Prémium szintű gyorsítótár létrehozásához fürtözési</span><span class="sxs-lookup"><span data-stu-id="f942a-108">Create a premium cache with clustering</span></span>](./scripts/create-premium-cache-cluster.md) | <span data-ttu-id="f942a-109">Hoz létre egy erőforráscsoportot és a premium szint gyorsítótár fürtözési engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="f942a-109">Creates a resource group and a premium tier cache with clustering enabled.</span></span>|
| [<span data-ttu-id="f942a-110">Gyorsítótár-részletek</span><span class="sxs-lookup"><span data-stu-id="f942a-110">Get cache details</span></span>](./scripts/show-cache.md) | <span data-ttu-id="f942a-111">Lekérdezi az Azure Redis Cache példány, beleértve a kiépítési állapot részleteit.</span><span class="sxs-lookup"><span data-stu-id="f942a-111">Gets details of an Azure Redis Cache instance, including provisioning status.</span></span> |
| [<span data-ttu-id="f942a-112">Hello állomásnév, portokat és a kulcsok beszerzése</span><span class="sxs-lookup"><span data-stu-id="f942a-112">Get hello hostname, ports, and keys</span></span>](./scripts/cache-keys-ports.md) | <span data-ttu-id="f942a-113">Lekérdezi a hello állomásnév, portokat és a kulcsok Azure Redis Cache példányt.</span><span class="sxs-lookup"><span data-stu-id="f942a-113">Gets hello hostname, ports, and keys for an Azure Redis Cache instance.</span></span> |
|<span data-ttu-id="f942a-114">**A webalkalmazás és a gyorsítótár**</span><span class="sxs-lookup"><span data-stu-id="f942a-114">**Web app plus cache**</span></span>||
| [<span data-ttu-id="f942a-115">Csatlakozás a webes alkalmazás tooa redis gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="f942a-115">Connect a web app tooa redis cache</span></span>](./../app-service-web/scripts/app-service-cli-app-service-redis.md) | <span data-ttu-id="f942a-116">Azure-webalkalmazás és a redis gyorsítótár hoz létre, majd hozzáadja hello redis kapcsolódási részletek toohello Alkalmazásbeállítások.</span><span class="sxs-lookup"><span data-stu-id="f942a-116">Creates an Azure web app and a redis cache, then adds hello redis connection details toohello app settings.</span></span> |
|<span data-ttu-id="f942a-117">**Gyorsítótár törlése**</span><span class="sxs-lookup"><span data-stu-id="f942a-117">**Delete cache**</span></span>||
| [<span data-ttu-id="f942a-118">A gyorsítótár törlése</span><span class="sxs-lookup"><span data-stu-id="f942a-118">Delete a cache</span></span>](./scripts/delete-cache.md) | <span data-ttu-id="f942a-119">Törli az Azure Redis Cache példányt</span><span class="sxs-lookup"><span data-stu-id="f942a-119">Deletes an Azure Redis Cache instance</span></span>  |
| | |

<span data-ttu-id="f942a-120">Azure CLI 2.0 kapcsolatos további információkért lásd: [Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli) és [Ismerkedés az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f942a-120">For more information about Azure CLI 2.0, see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) and [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).</span></span>