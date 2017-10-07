---
title: "az IntelliJ aaaManaging Redis Cache-gyorsítótárak használatával hello Azure-kezelővel |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage az Azure redis gyorsítótárazza az IntelliJ hello Azure Explorer használatával."
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
ms.openlocfilehash: 76ba37a2a35c26d0045e17003181108992eb957d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-redis-caches-using-hello-azure-explorer-for-intellij"></a><span data-ttu-id="e16fe-103">Redis Cache-gyorsítótárak hello Azure Explorer használatával az IntelliJ kezelése</span><span class="sxs-lookup"><span data-stu-id="e16fe-103">Managing Redis Caches using hello Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="e16fe-104">hello Azure-kezelővel, amely hello Azure eszköztára IntelliJ része, biztosít a Java-fejlesztők egy könnyen kezelhető megoldás az kezelése redis gyorsítótár belül a Azure fiók hello IntelliJ IDE.</span><span class="sxs-lookup"><span data-stu-id="e16fe-104">hello Azure Explorer, which is part of hello Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing redis caches in their Azure account from inside hello IntelliJ IDE.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a><span data-ttu-id="e16fe-105">A Redis Cache létrehozása az IntelliJ használatával</span><span class="sxs-lookup"><span data-stu-id="e16fe-105">Create a Redis Cache by using IntelliJ</span></span>

<span data-ttu-id="e16fe-106">hello lépések végigvezetik Önt hello lépéseket toocreate a redis gyorsítótár hello Azure Explorer használatával.</span><span class="sxs-lookup"><span data-stu-id="e16fe-106">hello following steps walk you through hello steps toocreate a redis cache using hello Azure Explorer.</span></span>

1. <span data-ttu-id="e16fe-107">Jelentkezzen be tooyour hello lépésekkel hello Azure-fiók [bejelentkezési a utasítások az intellij-t Azure eszköztára hello] cikk.</span><span class="sxs-lookup"><span data-stu-id="e16fe-107">Sign in tooyour Azure account using hello steps in hello [Sign In Instructions for hello Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="e16fe-108">A hello **Azure Explorer** ablak eszköze, bontsa ki a hello **Azure** csomópontot, kattintson a jobb gombbal **Redis Cache-gyorsítótárak**, és kattintson a **Redis Cache létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="e16fe-108">In hello **Azure Explorer** tool window, expand hello **Azure** node, right-click **Redis Caches**, and then click **Create Redis Cache**.</span></span>

   ![Redis gyorsítótár menü létrehozása][CR01]

1. <span data-ttu-id="e16fe-110">Ha hello **új Redis Cache** párbeszédpanel, adja meg az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="e16fe-110">When hello **New Redis Cache** dialog box appears, specify hello following options:</span></span>

   ![Hozzon létre új Redis Cache párbeszédpanel][CR02]

   <span data-ttu-id="e16fe-112">a.</span><span class="sxs-lookup"><span data-stu-id="e16fe-112">a.</span></span> <span data-ttu-id="e16fe-113">**DNS-név**: Adja meg a DNS altartományában hello hello új redis gyorsítótárat, amely túl $a-e a ". redis.cache.windows .net"; például: *wingtiptoys.redis.cache.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="e16fe-113">**DNS Name**: Specifies hello DNS subdomain for hello new redis cache, which are prepended too".redis.cache.windows.net"; for example: *wingtiptoys.redis.cache.windows.net*.</span></span>

   <span data-ttu-id="e16fe-114">b.</span><span class="sxs-lookup"><span data-stu-id="e16fe-114">b.</span></span> <span data-ttu-id="e16fe-115">**Előfizetés**: hello toouse hello új redis gyorsítótár kívánt Azure-előfizetés határozza meg.</span><span class="sxs-lookup"><span data-stu-id="e16fe-115">**Subscription**: Specifies hello Azure subscription you want toouse for hello new redis cache.</span></span>

   <span data-ttu-id="e16fe-116">c.</span><span class="sxs-lookup"><span data-stu-id="e16fe-116">c.</span></span> <span data-ttu-id="e16fe-117">**Erőforráscsoport**: Adja meg a redis gyorsítótár hello erőforráscsoport; toochoose az alábbi beállítások hello van szüksége:</span><span class="sxs-lookup"><span data-stu-id="e16fe-117">**Resource Group**: Specifies hello resource group for your redis cache; you need toochoose one of hello following options:</span></span>
      * <span data-ttu-id="e16fe-118">**Hozzon létre új**: megadhatja, hogy kívánja-e toocreate egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="e16fe-118">**Create New**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="e16fe-119">**Használja a már meglévő**: meghatározza a fog választani az erőforráscsoportok az Azure-fiókjával társított listájából.</span><span class="sxs-lookup"><span data-stu-id="e16fe-119">**Use Existing**: Specifies that you will choose from a list of resource groups associated with your Azure account.</span></span>

   <span data-ttu-id="e16fe-120">d.</span><span class="sxs-lookup"><span data-stu-id="e16fe-120">d.</span></span> <span data-ttu-id="e16fe-121">**Hely**: a redis gyorsítótár létrehozási helyének; hello helyét adja meg például *USA nyugati régiója*.</span><span class="sxs-lookup"><span data-stu-id="e16fe-121">**Location**: Specifies hello location where your redis cache is created; for example, *West US*.</span></span>

   <span data-ttu-id="e16fe-122">e.</span><span class="sxs-lookup"><span data-stu-id="e16fe-122">e.</span></span> <span data-ttu-id="e16fe-123">**IP-címek**: Adja meg a redis gyorsítótárat használ tarifacsomagtól; Ez a beállítás határozza meg, hogy hello ügyfélkapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="e16fe-123">**Pricing Tier**: Specifies which pricing tier your redis cache uses; this setting determines hello number of client connections.</span></span> <span data-ttu-id="e16fe-124">(További információkért lásd: [Redis gyorsítótár árképzési].)</span><span class="sxs-lookup"><span data-stu-id="e16fe-124">(For more information, see [Redis Cache Pricing].)</span></span>

   <span data-ttu-id="e16fe-125">f.</span><span class="sxs-lookup"><span data-stu-id="e16fe-125">f.</span></span> <span data-ttu-id="e16fe-126">**A nem SSL port**: Megadja, hogy a redis gyorsítótár lehetővé teszi, hogy az SSL kapcsolatok; alapértelmezés szerint csak az SSL-kapcsolatok engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="e16fe-126">**Non-SSL port**: Specifies whether your redis cache allows non-SSL connections; by default, only SSL connections are allowed.</span></span>

1. <span data-ttu-id="e16fe-127">A redis gyorsítótár beállításainak megadását, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="e16fe-127">When you have specified all your redis cache settings, click **OK**.</span></span>

<span data-ttu-id="e16fe-128">A redis gyorsítótár létrehozása után megjelenő hello Azure-kezelővel történik.</span><span class="sxs-lookup"><span data-stu-id="e16fe-128">After your redis cache has been created, it will be displayed in hello Azure Explorer.</span></span>

   ![Redis gyorsítótár Azure Explorerben][CR03]

> [!NOTE]
>
> <span data-ttu-id="e16fe-130">Az Azure konfigurálásával kapcsolatos további információkat a redis gyorsítótár-beállítások című [hogyan tooconfigure Azure Redis Cache].</span><span class="sxs-lookup"><span data-stu-id="e16fe-130">For more information about configuring your Azure redis cache settings, see [How tooconfigure Azure Redis Cache].</span></span>
>

## <a name="display-hello-properties-for-your-redis-cache-in-intellij"></a><span data-ttu-id="e16fe-131">A Redis Cache-ben az IntelliJ hello tulajdonságok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="e16fe-131">Display hello properties for your Redis Cache in IntelliJ</span></span>

1. <span data-ttu-id="e16fe-132">A hello Azure-kezelővel, kattintson a jobb gombbal a redis gyorsítótárt, és kattintson a **tulajdonságainak megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="e16fe-132">In hello Azure Explorer, right-click your redis cache and click **Show properties**.</span></span>

   ![Az Azure Explorer helyi menü toodisplay tulajdonságai egy redis gyorsítótárhoz][SP01]

1. <span data-ttu-id="e16fe-134">hello Azure-kezelővel a redis gyorsítótár hello tulajdonságainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="e16fe-134">hello Azure Explorer displays hello properties for your redis cache.</span></span>

   ![A Redis Cache-gyorsítótár tulajdonságai][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a><span data-ttu-id="e16fe-136">Törölje a Redis Cache IntelliJ használatával</span><span class="sxs-lookup"><span data-stu-id="e16fe-136">Delete your Redis Cache by using IntelliJ</span></span>

1. <span data-ttu-id="e16fe-137">A hello Azure-kezelővel, kattintson a jobb gombbal a redis gyorsítótárt, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e16fe-137">In hello Azure Explorer, right-click your redis cache and click **Delete**.</span></span>

   ![Az Azure Explorer helyi menü toodelete a redis gyorsítótár][DE01]

1. <span data-ttu-id="e16fe-139">Kattintson a **Igen** amikor toodelete a redis gyorsítótár kéri.</span><span class="sxs-lookup"><span data-stu-id="e16fe-139">Click **Yes** when prompted toodelete your redis cache.</span></span>

   ![Redis gyorsítótár kérdés törlése][DE02]

## <a name="next-steps"></a><span data-ttu-id="e16fe-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e16fe-141">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="e16fe-142">Azure redis gyorsítótár, a konfigurációs beállításokat és a díjszabás kapcsolatos további információkért tekintse meg a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="e16fe-142">For more information about Azure redis caches, configuration settings and pricing, see hello following links:</span></span>

* <span data-ttu-id="e16fe-143">[Azure Redis Cache]</span><span class="sxs-lookup"><span data-stu-id="e16fe-143">[Azure Redis Cache]</span></span>
* <span data-ttu-id="e16fe-144">[Redis gyorsítótár dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="e16fe-144">[Redis Cache Documentation]</span></span>
* <span data-ttu-id="e16fe-145">[Redis gyorsítótár díjszabása]</span><span class="sxs-lookup"><span data-stu-id="e16fe-145">[Redis Cache Pricing]</span></span>
* <span data-ttu-id="e16fe-146">[Hogyan tooconfigure Azure Redis Cache-gyorsítótár]</span><span class="sxs-lookup"><span data-stu-id="e16fe-146">[How tooconfigure Azure Redis Cache]</span></span>

<!-- URL List -->

[Redis gyorsítótár díjszabása]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Redis gyorsítótár dokumentáció]: ./redis-cache/index.md
[Hogyan tooconfigure Azure Redis Cache-gyorsítótár]: ./redis-cache/cache-configure.md
[Bejelentkezés a utasításokat hello IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
