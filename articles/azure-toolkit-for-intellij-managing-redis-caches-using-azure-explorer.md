---
title: "Redis Cache-gyorsítótárak az intellij-t Azure Explorerrel kezelése |} Microsoft Docs"
description: "Útmutató az Azure redis gyorsítótár felügyelni az intellij-t az Azure-kezelővel használatával."
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
ms.openlocfilehash: 9ab8ae17ee2a92b5b16d2210366c00b5b8023fa8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="868e0-103">Redis Cache-gyorsítótárak az intellij-t Azure Explorerrel kezelése</span><span class="sxs-lookup"><span data-stu-id="868e0-103">Managing Redis Caches using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="868e0-104">Az Azure-kezelővel, amely IntelliJ Azure eszköztára része biztosít a Java fejlesztők egy könnyen használható megoldást a redis-gyorsítótárak a Azure fiók belül az IntelliJ IDE.</span><span class="sxs-lookup"><span data-stu-id="868e0-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing redis caches in their Azure account from inside the IntelliJ IDE.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a><span data-ttu-id="868e0-105">A Redis Cache létrehozása az IntelliJ használatával</span><span class="sxs-lookup"><span data-stu-id="868e0-105">Create a Redis Cache by using IntelliJ</span></span>

<span data-ttu-id="868e0-106">A következő lépések végigvezetik Önt az Azure-kezelővel redis gyorsítótár létrehozásához szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="868e0-106">The following steps walk you through the steps to create a redis cache using the Azure Explorer.</span></span>

1. <span data-ttu-id="868e0-107">A lépések az Azure-fiókjával jelentkezzen be a [bejelentkezési az utasítások az intellij-t Azure eszköztára] cikk.</span><span class="sxs-lookup"><span data-stu-id="868e0-107">Sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="868e0-108">Az a **Azure Explorer** ablak eszköze, bontsa ki a **Azure** csomópontot, kattintson a jobb gombbal **Redis Cache-gyorsítótárak**, és kattintson a **Redis Cache létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="868e0-108">In the **Azure Explorer** tool window, expand the **Azure** node, right-click **Redis Caches**, and then click **Create Redis Cache**.</span></span>

   ![Redis gyorsítótár menü létrehozása][CR01]

1. <span data-ttu-id="868e0-110">Ha a **új Redis Cache** párbeszédpanel, adja meg a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="868e0-110">When the **New Redis Cache** dialog box appears, specify the following options:</span></span>

   ![Hozzon létre új Redis Cache párbeszédpanel][CR02]

   <span data-ttu-id="868e0-112">a.</span><span class="sxs-lookup"><span data-stu-id="868e0-112">a.</span></span> <span data-ttu-id="868e0-113">**DNS-név**: Adja meg a DNS altartományában a új redis gyorsítótár, amely a rendszer $a ". redis.cache.windows.net"; például: *wingtiptoys.redis.cache.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="868e0-113">**DNS Name**: Specifies the DNS subdomain for the new redis cache, which are prepended to ".redis.cache.windows.net"; for example: *wingtiptoys.redis.cache.windows.net*.</span></span>

   <span data-ttu-id="868e0-114">b.</span><span class="sxs-lookup"><span data-stu-id="868e0-114">b.</span></span> <span data-ttu-id="868e0-115">**Előfizetés**: Adja meg az új redis gyorsítótár használni kívánt Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="868e0-115">**Subscription**: Specifies the Azure subscription you want to use for the new redis cache.</span></span>

   <span data-ttu-id="868e0-116">c.</span><span class="sxs-lookup"><span data-stu-id="868e0-116">c.</span></span> <span data-ttu-id="868e0-117">**Erőforráscsoport**: Adja meg az erőforráscsoport a redis gyorsítótár; meg kell adnia az alábbi lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="868e0-117">**Resource Group**: Specifies the resource group for your redis cache; you need to choose one of the following options:</span></span>
      * <span data-ttu-id="868e0-118">**Hozzon létre új**: megadhatja, hogy kívánja-e egy új erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="868e0-118">**Create New**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="868e0-119">**Használja a már meglévő**: meghatározza a fog választani az erőforráscsoportok az Azure-fiókjával társított listájából.</span><span class="sxs-lookup"><span data-stu-id="868e0-119">**Use Existing**: Specifies that you will choose from a list of resource groups associated with your Azure account.</span></span>

   <span data-ttu-id="868e0-120">d.</span><span class="sxs-lookup"><span data-stu-id="868e0-120">d.</span></span> <span data-ttu-id="868e0-121">**Hely**: Adja meg a helyet, ahol a redis gyorsítótárat létrehozni; például *USA nyugati régiója*.</span><span class="sxs-lookup"><span data-stu-id="868e0-121">**Location**: Specifies the location where your redis cache is created; for example, *West US*.</span></span>

   <span data-ttu-id="868e0-122">e.</span><span class="sxs-lookup"><span data-stu-id="868e0-122">e.</span></span> <span data-ttu-id="868e0-123">**IP-címek**: Adja meg a redis gyorsítótárat használ tarifacsomagtól; Ez a beállítás meghatározza, hogy az ügyfélkapcsolatok számát.</span><span class="sxs-lookup"><span data-stu-id="868e0-123">**Pricing Tier**: Specifies which pricing tier your redis cache uses; this setting determines the number of client connections.</span></span> <span data-ttu-id="868e0-124">(További információkért lásd: [Redis gyorsítótár árképzési].)</span><span class="sxs-lookup"><span data-stu-id="868e0-124">(For more information, see [Redis Cache Pricing].)</span></span>

   <span data-ttu-id="868e0-125">f.</span><span class="sxs-lookup"><span data-stu-id="868e0-125">f.</span></span> <span data-ttu-id="868e0-126">**A nem SSL port**: Megadja, hogy a redis gyorsítótár lehetővé teszi, hogy az SSL kapcsolatok; alapértelmezés szerint csak az SSL-kapcsolatok engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="868e0-126">**Non-SSL port**: Specifies whether your redis cache allows non-SSL connections; by default, only SSL connections are allowed.</span></span>

1. <span data-ttu-id="868e0-127">A redis gyorsítótár beállításainak megadását, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="868e0-127">When you have specified all your redis cache settings, click **OK**.</span></span>

<span data-ttu-id="868e0-128">A redis gyorsítótár létrehozása után megjelenik az Azure-kezelővel történik.</span><span class="sxs-lookup"><span data-stu-id="868e0-128">After your redis cache has been created, it will be displayed in the Azure Explorer.</span></span>

   ![Redis gyorsítótár Azure Explorerben][CR03]

> [!NOTE]
>
> <span data-ttu-id="868e0-130">Az Azure konfigurálásával kapcsolatos további információkat a redis gyorsítótár-beállítások című [konfigurálása az Azure Redis Cache].</span><span class="sxs-lookup"><span data-stu-id="868e0-130">For more information about configuring your Azure redis cache settings, see [How to configure Azure Redis Cache].</span></span>
>

## <a name="display-the-properties-for-your-redis-cache-in-intellij"></a><span data-ttu-id="868e0-131">A Redis Cache-ben az IntelliJ tulajdonságainak megjelenítése</span><span class="sxs-lookup"><span data-stu-id="868e0-131">Display the properties for your Redis Cache in IntelliJ</span></span>

1. <span data-ttu-id="868e0-132">Az Azure-kezelővel, kattintson a jobb gombbal a redis gyorsítótárt, és kattintson a **tulajdonságainak megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="868e0-132">In the Azure Explorer, right-click your redis cache and click **Show properties**.</span></span>

   ![Az Azure Explorer helyi menü megjelenítése a redis gyorsítótár tulajdonságai][SP01]

1. <span data-ttu-id="868e0-134">Az Azure-kezelővel a redis gyorsítótárat a Tulajdonságok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="868e0-134">The Azure Explorer displays the properties for your redis cache.</span></span>

   ![A Redis Cache-gyorsítótár tulajdonságai][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a><span data-ttu-id="868e0-136">Törölje a Redis Cache IntelliJ használatával</span><span class="sxs-lookup"><span data-stu-id="868e0-136">Delete your Redis Cache by using IntelliJ</span></span>

1. <span data-ttu-id="868e0-137">Az Azure-kezelővel, kattintson a jobb gombbal a redis gyorsítótárt, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="868e0-137">In the Azure Explorer, right-click your redis cache and click **Delete**.</span></span>

   ![A redis gyorsítótár törléséhez az Azure Explorer helyi menü][DE01]

1. <span data-ttu-id="868e0-139">Kattintson a **Igen** amikor a rendszer kéri a redis gyorsítótár törléséhez.</span><span class="sxs-lookup"><span data-stu-id="868e0-139">Click **Yes** when prompted to delete your redis cache.</span></span>

   ![Redis gyorsítótár kérdés törlése][DE02]

## <a name="next-steps"></a><span data-ttu-id="868e0-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="868e0-141">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="868e0-142">Azure redis gyorsítótár, a konfigurációs beállításokat és a díjszabás kapcsolatos további információkért tekintse meg a következőket:</span><span class="sxs-lookup"><span data-stu-id="868e0-142">For more information about Azure redis caches, configuration settings and pricing, see the following links:</span></span>

* <span data-ttu-id="868e0-143">[Azure Redis Cache]</span><span class="sxs-lookup"><span data-stu-id="868e0-143">[Azure Redis Cache]</span></span>
* <span data-ttu-id="868e0-144">[Redis gyorsítótár dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="868e0-144">[Redis Cache Documentation]</span></span>
* <span data-ttu-id="868e0-145">[Redis gyorsítótár árképzési]</span><span class="sxs-lookup"><span data-stu-id="868e0-145">[Redis Cache Pricing]</span></span>
* <span data-ttu-id="868e0-146">[konfigurálása az Azure Redis Cache]</span><span class="sxs-lookup"><span data-stu-id="868e0-146">[How to configure Azure Redis Cache]</span></span>

<!-- URL List -->

<span data-ttu-id="868e0-147">[Redis gyorsítótár árképzési]: https://azure.microsoft.com/pricing/details/cache/</span><span class="sxs-lookup"><span data-stu-id="868e0-147">[Redis Cache Pricing]: https://azure.microsoft.com/pricing/details/cache/</span></span>
<span data-ttu-id="868e0-148">[Azure Redis Cache]: https://azure.microsoft.com/services/cache/</span><span class="sxs-lookup"><span data-stu-id="868e0-148">[Azure Redis Cache]: https://azure.microsoft.com/services/cache/</span></span>
<span data-ttu-id="868e0-149">[Redis gyorsítótár dokumentáció]: ./redis-cache/index.md</span><span class="sxs-lookup"><span data-stu-id="868e0-149">[Redis Cache Documentation]: ./redis-cache/index.md</span></span>
<span data-ttu-id="868e0-150">[konfigurálása az Azure Redis Cache]: ./redis-cache/cache-configure.md</span><span class="sxs-lookup"><span data-stu-id="868e0-150">[How to configure Azure Redis Cache]: ./redis-cache/cache-configure.md</span></span>
<span data-ttu-id="868e0-151">[bejelentkezési az utasítások az intellij-t Azure eszköztára]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="868e0-151">[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
