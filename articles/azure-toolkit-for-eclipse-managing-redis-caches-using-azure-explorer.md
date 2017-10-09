---
title: "Azure-kezelőjét az Eclipse aaaManaging Redis Cache-gyorsítótárak használatával hello |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage az Azure redis gyorsítótárazza az Eclipse hello Azure Explorer használatával."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 06/14/2017
ms.author: robmcm
ms.openlocfilehash: aa0c38862bda7919a3fc6c53c2fdaf555dd64bff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-redis-caches-using-hello-azure-explorer-for-eclipse"></a><span data-ttu-id="531c5-103">Redis Cache-gyorsítótárak hello Azure Explorer használata az Eclipse kezelése</span><span class="sxs-lookup"><span data-stu-id="531c5-103">Managing Redis Caches using hello Azure Explorer for Eclipse</span></span>

<span data-ttu-id="531c5-104">hello Azure-kezelővel, amely hello Azure eszköztára Eclipse része, biztosít a Java-fejlesztők egy könnyen kezelhető megoldás az kezelése redis gyorsítótár belül a Azure fiók hello Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="531c5-104">hello Azure Explorer, which is part of hello Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing redis caches in their Azure account from inside hello Eclipse IDE.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-eclipse"></a><span data-ttu-id="531c5-105">Hozzon létre egy Redis gyorsítótárhoz eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="531c5-105">Create a Redis Cache by using Eclipse</span></span>

<span data-ttu-id="531c5-106">hello lépések végigvezetik Önt hello lépéseket toocreate a redis gyorsítótár hello Azure Explorer használatával.</span><span class="sxs-lookup"><span data-stu-id="531c5-106">hello following steps walk you through hello steps toocreate a redis cache using hello Azure Explorer.</span></span>

1. <span data-ttu-id="531c5-107">Jelentkezzen be tooyour hello lépésekkel hello Azure-fiók [bejelentkezési az utasítások hello Azure eszköztára Eclipse] cikk.</span><span class="sxs-lookup"><span data-stu-id="531c5-107">Sign in tooyour Azure account using hello steps in hello [Sign In Instructions for hello Azure Toolkit for Eclipse] article.</span></span>

1. <span data-ttu-id="531c5-108">A hello **Azure Explorer** ablak eszköze, bontsa ki a hello **Azure** csomópontot, kattintson a jobb gombbal **Redis Cache-gyorsítótárak**, és kattintson a **Redis Cache létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="531c5-108">In hello **Azure Explorer** tool window, expand hello **Azure** node, right-click **Redis Caches**, and then click **Create Redis Cache**.</span></span>

   ![Redis gyorsítótár menü létrehozása][CR01]

1. <span data-ttu-id="531c5-110">Ha hello **új Redis Cache** párbeszédpanel, adja meg az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="531c5-110">When hello **New Redis Cache** dialog box appears, specify hello following options:</span></span>

   ![Hozzon létre új Redis gyorsítótár párbeszédpanel][CR02]

   <span data-ttu-id="531c5-112">a.</span><span class="sxs-lookup"><span data-stu-id="531c5-112">a.</span></span> <span data-ttu-id="531c5-113">**DNS-név**: Adja meg a DNS altartományában hello hello új redis gyorsítótárat, amely túl van $a a ". redis.cache.windows .net"; például: *wingtiptoys.redis.cache.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="531c5-113">**DNS Name**: Specifies hello DNS subdomain for hello new redis cache, which is prepended too".redis.cache.windows.net"; for example: *wingtiptoys.redis.cache.windows.net*.</span></span>

   <span data-ttu-id="531c5-114">b.</span><span class="sxs-lookup"><span data-stu-id="531c5-114">b.</span></span> <span data-ttu-id="531c5-115">**Előfizetés**: hello toouse hello új redis gyorsítótár kívánt Azure-előfizetés határozza meg.</span><span class="sxs-lookup"><span data-stu-id="531c5-115">**Subscription**: Specifies hello Azure subscription you want toouse for hello new redis cache.</span></span>

   <span data-ttu-id="531c5-116">c.</span><span class="sxs-lookup"><span data-stu-id="531c5-116">c.</span></span> <span data-ttu-id="531c5-117">**Erőforráscsoport**: Adja meg a redis gyorsítótár hello erőforráscsoport; toochoose az alábbi beállítások hello van szüksége:</span><span class="sxs-lookup"><span data-stu-id="531c5-117">**Resource Group**: Specifies hello resource group for your redis cache; you need toochoose one of hello following options:</span></span>
      * <span data-ttu-id="531c5-118">**Hozzon létre új**: megadhatja, hogy kívánja-e toocreate egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="531c5-118">**Create New**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="531c5-119">**Használja a már meglévő**: meghatározza a fog választani az erőforráscsoportok az Azure-fiókjával társított listájából.</span><span class="sxs-lookup"><span data-stu-id="531c5-119">**Use Existing**: Specifies that you will choose from a list of resource groups associated with your Azure account.</span></span>

   <span data-ttu-id="531c5-120">d.</span><span class="sxs-lookup"><span data-stu-id="531c5-120">d.</span></span> <span data-ttu-id="531c5-121">**Hely**: a redis gyorsítótár létrehozási helyének; hello helyét adja meg például *USA nyugati régiója*.</span><span class="sxs-lookup"><span data-stu-id="531c5-121">**Location**: Specifies hello location where your redis cache is created; for example, *West US*.</span></span>

   <span data-ttu-id="531c5-122">e.</span><span class="sxs-lookup"><span data-stu-id="531c5-122">e.</span></span> <span data-ttu-id="531c5-123">**IP-címek**: Adja meg a redis gyorsítótárat használ tarifacsomagtól; Ez a beállítás határozza meg, hogy hello ügyfélkapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="531c5-123">**Pricing Tier**: Specifies which pricing tier your redis cache uses; this setting determines hello number of client connections.</span></span> <span data-ttu-id="531c5-124">(További információkért lásd: [Redis gyorsítótár árképzési].)</span><span class="sxs-lookup"><span data-stu-id="531c5-124">(For more information, see [Redis Cache Pricing].)</span></span>

   <span data-ttu-id="531c5-125">f.</span><span class="sxs-lookup"><span data-stu-id="531c5-125">f.</span></span> <span data-ttu-id="531c5-126">**A nem SSL port**: Megadja, hogy a redis gyorsítótár lehetővé teszi, hogy az SSL kapcsolatok; alapértelmezés szerint csak az SSL-kapcsolatok engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="531c5-126">**Non-SSL port**: Specifies whether your redis cache allows non-SSL connections; by default, only SSL connections are allowed.</span></span>

1. <span data-ttu-id="531c5-127">A redis gyorsítótár beállításainak megadását, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="531c5-127">When you have specified all your redis cache settings, click **OK**.</span></span>

<span data-ttu-id="531c5-128">A redis gyorsítótár létrehozása után megjelenő hello Azure-kezelővel történik.</span><span class="sxs-lookup"><span data-stu-id="531c5-128">After your redis cache has been created, it will be displayed in hello Azure Explorer.</span></span>

   ![Redis gyorsítótár Azure Explorerben][CR03]

> [!NOTE]
>
> <span data-ttu-id="531c5-130">Az Azure konfigurálásával kapcsolatos további információkat a redis gyorsítótár-beállítások című [hogyan tooconfigure Azure Redis Cache].</span><span class="sxs-lookup"><span data-stu-id="531c5-130">For more information about configuring your Azure redis cache settings, see [How tooconfigure Azure Redis Cache].</span></span>
>

## <a name="display-hello-properties-for-your-redis-cache-in-eclipse"></a><span data-ttu-id="531c5-131">Az eclipse-ben a Redis gyorsítótár hello tulajdonságok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="531c5-131">Display hello properties for your Redis Cache in Eclipse</span></span>

1. <span data-ttu-id="531c5-132">A hello Azure-kezelővel, kattintson a jobb gombbal a redis gyorsítótárt, és kattintson a **tulajdonságainak megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="531c5-132">In hello Azure Explorer, right-click your redis cache and click **Show properties**.</span></span>

   ![Az Azure Explorer helyi menü toodisplay tulajdonságai egy redis gyorsítótárhoz][SP01]

1. <span data-ttu-id="531c5-134">hello Azure-kezelővel a redis gyorsítótár hello tulajdonságainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="531c5-134">hello Azure Explorer displays hello properties for your redis cache.</span></span>

   ![A Redis Cache-gyorsítótár tulajdonságai][SP02]

## <a name="delete-your-redis-cache-by-using-eclipse"></a><span data-ttu-id="531c5-136">Törölje a Redis Cache Eclipse használatával</span><span class="sxs-lookup"><span data-stu-id="531c5-136">Delete your Redis Cache by using Eclipse</span></span>

1. <span data-ttu-id="531c5-137">A hello Azure-kezelővel, kattintson a jobb gombbal a redis gyorsítótárt, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="531c5-137">In hello Azure Explorer, right-click your redis cache and click **Delete**.</span></span>

   ![Az Azure Explorer helyi menü toodelete a redis gyorsítótár][DE01]

1. <span data-ttu-id="531c5-139">Kattintson a **OK** amikor toodelete a redis gyorsítótár kéri.</span><span class="sxs-lookup"><span data-stu-id="531c5-139">Click **OK** when prompted toodelete your redis cache.</span></span>

   ![Redis gyorsítótár kérdés törlése][DE02]

## <a name="next-steps"></a><span data-ttu-id="531c5-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="531c5-141">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="531c5-142">Azure redis gyorsítótár, a konfigurációs beállításokat és a díjszabás kapcsolatos további információkért tekintse meg a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="531c5-142">For more information about Azure redis caches, configuration settings and pricing, see hello following links:</span></span>

* <span data-ttu-id="531c5-143">[Azure Redis Cache]</span><span class="sxs-lookup"><span data-stu-id="531c5-143">[Azure Redis Cache]</span></span>
* <span data-ttu-id="531c5-144">[Redis gyorsítótár dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="531c5-144">[Redis Cache Documentation]</span></span>
* <span data-ttu-id="531c5-145">[Redis gyorsítótár árképzési]</span><span class="sxs-lookup"><span data-stu-id="531c5-145">[Redis Cache Pricing]</span></span>
* <span data-ttu-id="531c5-146">[hogyan tooconfigure Azure Redis Cache]</span><span class="sxs-lookup"><span data-stu-id="531c5-146">[How tooconfigure Azure Redis Cache]</span></span>

<!-- URL List -->

[Redis gyorsítótár árképzési]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Redis gyorsítótár dokumentáció]: ./redis-cache/index.md
[hogyan tooconfigure Azure Redis Cache]: ./redis-cache/cache-configure.md
[bejelentkezési az utasítások hello Azure eszköztára Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/DE02.png
