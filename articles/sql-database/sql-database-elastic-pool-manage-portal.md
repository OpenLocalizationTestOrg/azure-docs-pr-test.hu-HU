---
title: "Azure-portálon: egy SQL Database rugalmas készlet kezelése & létrehozása |} Microsoft Docs"
description: "Ismerje meg, hogyan használható az Azure portál és az SQL Database beépített funkciói kezelése, figyelheti és az adatbázis teljesítményének optimalizálása és költségek kezelésére méretezhető rugalmas készlet megfelelő méretének."
keywords: 
services: sql-database
documentationcenter: 
author: ninarn
manager: jhubbard
editor: cgronlun
ms.assetid: 3dc9b7a3-4b10-423a-8e44-9174aca5cf3d
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.date: 06/06/2016
ms.author: ninarn
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 4ffd1db31f42967dc7f07aa979898dddbb333641
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-an-elastic-pool-with-the-azure-portal"></a><span data-ttu-id="0677e-103">Létrehozása és kezelése az Azure-portálon a rugalmas készlethez</span><span class="sxs-lookup"><span data-stu-id="0677e-103">Create and manage an elastic pool with the Azure portal</span></span>
<span data-ttu-id="0677e-104">Ez a témakör bemutatja, hogyan hozhatja létre és kezelheti méretezhető [rugalmas készletek](sql-database-elastic-pool.md) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="0677e-104">This topic shows you how to create and manage scalable [elastic pools](sql-database-elastic-pool.md) with the Azure portal.</span></span> <span data-ttu-id="0677e-105">Is létrehozása és kezelése az Azure rugalmas készletek [PowerShell](sql-database-elastic-pool-manage-powershell.md), a REST API-t vagy [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="0677e-105">You can also create and manage an Azure elastic pool with [PowerShell](sql-database-elastic-pool-manage-powershell.md), the REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="0677e-106">Is létrehozhat és adatbázisok áthelyezése esetében bejövő és kimenő használatával rugalmas készletek [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="0677e-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>

## <a name="create-an-elastic-pool"></a><span data-ttu-id="0677e-107">Rugalmas készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="0677e-107">Create an elastic pool</span></span> 

<span data-ttu-id="0677e-108">Kétféleképpen hozhat létre egy rugalmas készlet.</span><span class="sxs-lookup"><span data-stu-id="0677e-108">There are two ways you can create an elastic pool.</span></span> <span data-ttu-id="0677e-109">Létrehozhatja a készletet a nulláról is, ha tisztában van a használni kívánt beállításokkal, de alapul veheti a szolgáltatás javaslatait is.</span><span class="sxs-lookup"><span data-stu-id="0677e-109">You can do it from scratch if you know the pool setup you want, or start with a recommendation from the service.</span></span> <span data-ttu-id="0677e-110">SQL Database beépített funkciói képesek készletbeállítást egy rugalmas készlet telepítő, ha több költséghatékony, az adatbázisokat a múltbeli használat telemetriai adatai alapján van.</span><span class="sxs-lookup"><span data-stu-id="0677e-110">SQL Database has built-in intelligence that recommends an elastic pool setup if it's more cost-efficient for you based on the past usage telemetry for your databases.</span></span>

<span data-ttu-id="0677e-111">Egy kiszolgálón több készletet is létrehozhat, de egy készlethez különböző kiszolgálókról származó adatbázisok nem adhat.</span><span class="sxs-lookup"><span data-stu-id="0677e-111">You can create multiple pools on a server, but you can't add databases from different servers into the same pool.</span></span> 

> [!NOTE]
> <span data-ttu-id="0677e-112">A rugalmas készletek minden Azure-régióban általánosan elérhetők, kivéve Nyugat-Indiát, ahol a szolgáltatás jelenleg előzetes verzióként érhető el.</span><span class="sxs-lookup"><span data-stu-id="0677e-112">Elastic pools are generally available (GA) in all Azure regions except West India where it is currently in preview.</span></span>  <span data-ttu-id="0677e-113">A rugalmas készletek a lehető leghamarabb általánosan elérhetők lesznek ebben a régióban.</span><span class="sxs-lookup"><span data-stu-id="0677e-113">GA of elastic pools in this region will occur as soon as possible.</span></span>
>

### <a name="step-1-create-an-elastic-pool"></a><span data-ttu-id="0677e-114">1. lépés: Egy rugalmas készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="0677e-114">Step 1: Create an elastic pool</span></span>

<span data-ttu-id="0677e-115">Egy rugalmas készlet létrehozása a meglévő **server** a portálon legkönnyebben meglévő adatbázisok áthelyezése rugalmas készletbe.</span><span class="sxs-lookup"><span data-stu-id="0677e-115">Creating an elastic pool from an existing **server** blade in the portal is the easiest way to move existing databases into an elastic pool.</span></span>

> [!NOTE]
> <span data-ttu-id="0677e-116">Rugalmas készletek is létrehozhat keresve **SQL rugalmas készlet** a a **piactér** vagy kattint **+ Hozzáadás** a a **SQL rugalmas készletek** Keresse meg a panelt.</span><span class="sxs-lookup"><span data-stu-id="0677e-116">You can also create an elastic pool by searching **SQL elastic pool** in the **Marketplace** or clicking **+Add** in the **SQL elastic pools** browse blade.</span></span> <span data-ttu-id="0677e-117">Tudunk adjon meg egy új vagy meglévő kiszolgáló ezzel a készlettel munkafolyamat kiépítés keresztül.</span><span class="sxs-lookup"><span data-stu-id="0677e-117">You are able to specify a new or existing server through this pool provisioning workflow.</span></span>
>
>

1. <span data-ttu-id="0677e-118">A a [Azure-portálon](http://portal.azure.com/), kattintson a **további szolgáltatások**  **>**  **SQL Server-kiszolgálók**, és kattintson a kiszolgáló, amely tartalmazza a adatbázisok rugalmas készlethez hozzáadni kívánt.</span><span class="sxs-lookup"><span data-stu-id="0677e-118">In the [Azure portal](http://portal.azure.com/), click **More services** **>** **SQL servers**, and then click the server that contains the databases you want to add to an elastic pool.</span></span>
2. <span data-ttu-id="0677e-119">Kattintson a **Új készlet** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="0677e-119">Click **New pool**.</span></span>

    ![Készlet hozzáadása a kiszolgálóhoz](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    <span data-ttu-id="0677e-121">**-VAGY-**</span><span class="sxs-lookup"><span data-stu-id="0677e-121">**-OR-**</span></span>

    <span data-ttu-id="0677e-122">Megjelenik egy üzenet, amely tájékoztatja, hogy a kiszolgáló rugalmas készletek használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="0677e-122">You may see a message saying there are recommended elastic pools for the server.</span></span> <span data-ttu-id="0677e-123">Kattintson az üzenetre a korábbi adatbázis-használat telemetriai adatai alapján javasolt készletek megtekintéséhez, majd kattintson a csomagra a további részletek megjelenítéséhez és a készlet testre szabásához.</span><span class="sxs-lookup"><span data-stu-id="0677e-123">Click the message to see the recommended pools based on historical database usage telemetry, and then click the tier to see more details and customize the pool.</span></span> <span data-ttu-id="0677e-124">A javaslatokkal kapcsolatos további információkért olvassa el a témakör későbbi részében található [A készlettel kapcsolatos javaslatok megértése](#understand-elastic-pool-recommendations) című részt.</span><span class="sxs-lookup"><span data-stu-id="0677e-124">See [Understand pool recommendations](#understand-elastic-pool-recommendations) later in this topic for how the recommendation is made.</span></span>

    ![javasolt készlet](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. <span data-ttu-id="0677e-126">A **rugalmas készlet** panel jelenik meg, amely, amelyben meg kell határoznia a beállítások a készlethez.</span><span class="sxs-lookup"><span data-stu-id="0677e-126">The **elastic pool** blade appears, which is where you specify the settings for your pool.</span></span> <span data-ttu-id="0677e-127">Ha rákattintott **új készletet** az előző lépésben a tarifacsomag megadása **szabványos** alapértelmezett és az adatbázisok nem van jelölve.</span><span class="sxs-lookup"><span data-stu-id="0677e-127">If you clicked **New pool** in the previous step, the pricing tier is **Standard** by default and no databases is selected.</span></span> <span data-ttu-id="0677e-128">Létrehozhat egy üres készletet, vagy megadhatja a kiszolgálón már megtalálható adatbázisok egy készletét, amelyet át szeretne helyezni a készletbe.</span><span class="sxs-lookup"><span data-stu-id="0677e-128">You can create an empty pool, or specify a set of existing databases from that server to move into the pool.</span></span> <span data-ttu-id="0677e-129">A javasolt készlet létrehozásakor, a javasolt tarifacsomag Teljesítménybeállítások és az adatbázisok listája előre feltöltve, de továbbra is módosíthatja őket.</span><span class="sxs-lookup"><span data-stu-id="0677e-129">If you are creating a recommended pool, the recommended pricing tier, performance settings, and list of databases are prepopulated, but you can still change them.</span></span>

    ![A rugalmas készlet konfigurálása](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. <span data-ttu-id="0677e-131">Adjon nevet a rugalmas készletnek, vagy hagyja meg az alapértelmezett nevet.</span><span class="sxs-lookup"><span data-stu-id="0677e-131">Specify a name for the elastic pool, or leave it as the default.</span></span>

### <a name="step-2-choose-a-pricing-tier"></a><span data-ttu-id="0677e-132">2. lépés: Tarifacsomag kiválasztása</span><span class="sxs-lookup"><span data-stu-id="0677e-132">Step 2: Choose a pricing tier</span></span>

<span data-ttu-id="0677e-133">A készlet tarifacsomagjának módosítása a funkciók érhetők el a készletet, és Edtu (eDTU MAX) és tárhelyet (GB) az egyes adatbázisok számára elérhető maximális számának elastics határozza meg.</span><span class="sxs-lookup"><span data-stu-id="0677e-133">The pool's pricing tier determines the features available to the elastics in the pool, and the maximum number of eDTUs (eDTU MAX), and storage (GBs) available to each database.</span></span> <span data-ttu-id="0677e-134">A részletekért lásd a tarifacsomagokról szóló cikket.</span><span class="sxs-lookup"><span data-stu-id="0677e-134">For details, see Service Tiers.</span></span>

<span data-ttu-id="0677e-135">A készlet tarifacsomagjának módosításához kattintson a **Tarifacsomag** elemre, a kívánt tarifacsomagra, majd a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0677e-135">To change the pricing tier for the pool, click **Pricing tier**, click the pricing tier you want, and then click **Select**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0677e-136">Miután kiválasztotta a tarifacsomagot, és az **OK** gombra kattintva mentette a módosításokat az utolsó lépésnél, már nem fogja tudni megváltoztatni a készlet tarifacsomagját.</span><span class="sxs-lookup"><span data-stu-id="0677e-136">After you choose the pricing tier and commit your changes by clicking **OK** in the last step, you won't be able to change the pricing tier of the pool.</span></span> <span data-ttu-id="0677e-137">Egy meglévő rugalmas készlet tarifacsomagjának módosításához hozzon létre egy rugalmas készlet a kívánt tarifacsomagot, és az adatbázisok áttelepítése az új készletbe.</span><span class="sxs-lookup"><span data-stu-id="0677e-137">To change the pricing tier for an existing elastic pool, create an elastic pool in the desired pricing tier and migrate the databases to this new pool.</span></span>
>

![Tarifacsomag kiválasztása](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-the-pool"></a><span data-ttu-id="0677e-139">3. lépés: A készlet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0677e-139">Step 3: Configure the pool</span></span>

<span data-ttu-id="0677e-140">Az árképzési szint beállítása után kattintson konfigurálása készlet adatbázisok, a set készlet edtu-k és tárhely (GB-ban) vehet fel, és a készlet a minimális és maximális edtu-k számára a elastics állíthatja.</span><span class="sxs-lookup"><span data-stu-id="0677e-140">After setting the pricing tier, click Configure pool where you add databases, set pool eDTUs and storage (pool GBs), and where you set the min and max eDTUs for the elastics in the pool.</span></span>

1. <span data-ttu-id="0677e-141">Kattintson a **Készlet beállítása** elemre.</span><span class="sxs-lookup"><span data-stu-id="0677e-141">Click **Configure pool**</span></span>
2. <span data-ttu-id="0677e-142">Válassza ki a készletbe felvenni kívánt adatbázisokat.</span><span class="sxs-lookup"><span data-stu-id="0677e-142">Select the databases you want to add to the pool.</span></span> <span data-ttu-id="0677e-143">Ezt a lépést nem kötelező a készlet létrehozása során elvégezni.</span><span class="sxs-lookup"><span data-stu-id="0677e-143">This step is optional while creating the pool.</span></span> <span data-ttu-id="0677e-144">Adatbázisokat a készlet létrehozását követően is fel lehet venni.</span><span class="sxs-lookup"><span data-stu-id="0677e-144">Databases can be added after the pool has been created.</span></span>
    <span data-ttu-id="0677e-145">Az adatbázisok hozzáadásához kattintson az **Adatbázis hozzáadása** gombra, kattintson a felvenni kívánt adatbázisokra, majd a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0677e-145">To add databases, click **Add database**, click the databases that you want to add, and then click the **Select** button.</span></span>

    ![Adatbázisok hozzáadása](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    <span data-ttu-id="0677e-147">Ha a felvenni kívánt adatbázisokhoz elegendő korábbi használati telemetriai adat áll rendelkezésre, a rendszer frissíti az **Estimated eDTU and GB usage** (Becsült eDTU- és GB-használat) diagramot és az **Actual eDTU usage** (Tényleges eDTU-használat) sávdiagramot, amelyek segítenek Önnek meghozni a konfigurációval kapcsolatos döntéseket.</span><span class="sxs-lookup"><span data-stu-id="0677e-147">If the databases you're working with have enough historical usage telemetry, the **Estimated eDTU and GB usage** graph and the **Actual eDTU usage** bar chart update to help you make configuration decisions.</span></span> <span data-ttu-id="0677e-148">Ezenfelül egyes esetekben a szolgáltatás javaslatot tartalmazó üzenetet is megjelenít, amely segít a készlet megfelelő méretének kiválasztásában.</span><span class="sxs-lookup"><span data-stu-id="0677e-148">Also, the service may give you a recommendation message to help you right-size the pool.</span></span> <span data-ttu-id="0677e-149">Lásd: [Dinamikus javaslatok](#understand-elastic-pool-recommendations).</span><span class="sxs-lookup"><span data-stu-id="0677e-149">See [Dynamic Recommendations](#understand-elastic-pool-recommendations).</span></span>

3. <span data-ttu-id="0677e-150">A **Készlet beállítása** lapon elérhető vezérlők segítségével áttekintheti a beállításokat, és konfigurálhatja a készletet.</span><span class="sxs-lookup"><span data-stu-id="0677e-150">Use the controls on the **Configure pool** page to explore settings and configure your pool.</span></span> <span data-ttu-id="0677e-151">Lásd: [rugalmas készletek korlátok](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) további részletes információt az egyes szolgáltatásszinteken határértékeit, és tanulmányozza a [rugalmas készletek ára és teljesítménye szempontjai](sql-database-elastic-pool.md) részletes útmutatást megfelelő méretének kiválasztását a rugalmas készletekben.</span><span class="sxs-lookup"><span data-stu-id="0677e-151">See [Elastic pools limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for more detail about limits for each service tier, and see [Price and performance considerations for elastic pools](sql-database-elastic-pool.md) for detailed guidance on right-sizing an elastic pool.</span></span> <span data-ttu-id="0677e-152">Készlet beállításaival kapcsolatos további információkért lásd: [rugalmas készlet tulajdonságok](sql-database-elastic-pool.md#database-properties-for-pooled-databases).</span><span class="sxs-lookup"><span data-stu-id="0677e-152">For more information about pool settings, see [Elastic pool properties](sql-database-elastic-pool.md#database-properties-for-pooled-databases).</span></span>

    ![A rugalmas készlet konfigurálása](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. <span data-ttu-id="0677e-154">Ha módosította a beállításokat, kattintson a **Készlet beállítása** panel **Kiválasztás** elemére.</span><span class="sxs-lookup"><span data-stu-id="0677e-154">Click **Select** in the **Configure Pool** blade after changing settings.</span></span>
5. <span data-ttu-id="0677e-155">A készlet létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="0677e-155">Click **OK** to create the pool.</span></span>

## <a name="understand-elastic-pool-recommendations"></a><span data-ttu-id="0677e-156">Rugalmas készletekkel kapcsolatos javaslatok megértése</span><span class="sxs-lookup"><span data-stu-id="0677e-156">Understand elastic pool recommendations</span></span>

<span data-ttu-id="0677e-157">Az SQL Database szolgáltatás a használati előzmények elemzésével megállapítja, hogy megéri-e önálló adatbázisok helyett készleteket használni, és ha igen, javasol egy vagy több készletet.</span><span class="sxs-lookup"><span data-stu-id="0677e-157">The SQL Database service evaluates usage history and recommends one or more pools when it is more cost-effective than using single databases.</span></span> <span data-ttu-id="0677e-158">A javaslatokat a rendszer a kiszolgáló adatbázisainak a készlethez leginkább illő egyedi részhalmazával konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="0677e-158">Each recommendation is configured with a unique subset of the server's databases that best fit the pool.</span></span>

![javasolt készlet](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

<span data-ttu-id="0677e-160">A készletjavaslat a következőkből áll:</span><span class="sxs-lookup"><span data-stu-id="0677e-160">The pool recommendation comprises:</span></span>

- <span data-ttu-id="0677e-161">A készlet (alapszintű, Standard, Premium vagy Premium RS) tarifacsomagot</span><span class="sxs-lookup"><span data-stu-id="0677e-161">A pricing tier for the pool (Basic, Standard, Premium, or Premium RS)</span></span>
- <span data-ttu-id="0677e-162">A megfelelő **POOL eDTU** száma (amelyet készletenkénti maximális eDTU-ként is meg lehet határozni)</span><span class="sxs-lookup"><span data-stu-id="0677e-162">Appropriate **POOL eDTUs** (also called Max eDTUs per pool)</span></span>
- <span data-ttu-id="0677e-163">Az adatbázisonkénti **eDTU MAX** és **eDTU Min** érték</span><span class="sxs-lookup"><span data-stu-id="0677e-163">The **eDTU MAX** and **eDTU Min** per database</span></span>
- <span data-ttu-id="0677e-164">A készletbe javasolt adatbázisok listája</span><span class="sxs-lookup"><span data-stu-id="0677e-164">The list of recommended databases for the pool</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0677e-165">A szolgáltatás az elmúlt 30 nap telemetriai adatai alapján javasol készleteket.</span><span class="sxs-lookup"><span data-stu-id="0677e-165">The service takes the last 30 days of telemetry into account when recommending pools.</span></span> <span data-ttu-id="0677e-166">Rugalmas készletek jelöltként figyelembe kell venni egy adatbázist akkor léteznie kell legalább 7 napig.</span><span class="sxs-lookup"><span data-stu-id="0677e-166">For a database to be considered as a candidate for an elastic pool, it must exist for at least 7 days.</span></span> <span data-ttu-id="0677e-167">Azokat az adatbázisokat, amelyeket korábban már elhelyezett egy másik rugalmas készletben, a rendszer nem javasolja újabb rugalmas készletbe való bevonásra.</span><span class="sxs-lookup"><span data-stu-id="0677e-167">Databases that are already in an elastic pool are not considered as candidates for elastic pool recommendations.</span></span>
>

<span data-ttu-id="0677e-168">A szolgáltatás értékeli az erőforrásigényeket, illetve azt, hogy megéri-e a különböző csomagokhoz tartozó önálló adatbázisokat ugyanahhoz a csomaghoz tartozó készletekbe vonni.</span><span class="sxs-lookup"><span data-stu-id="0677e-168">The service evaluates resource needs and cost effectiveness of moving the single databases in each service tier into pools of the same tier.</span></span> <span data-ttu-id="0677e-169">A rendszer például megvizsgálja, hogy érdemes-e a kiszolgálón található Standard adatbázisokat Standard rugalmas készletté alakítani.</span><span class="sxs-lookup"><span data-stu-id="0677e-169">For example, all Standard databases on a server are assessed for their fit into a Standard Elastic Pool.</span></span> <span data-ttu-id="0677e-170">Ez azt is jelenti, hogy a szolgáltatás különböző csomagokat tartalmazó javaslatokat nem tesz, azaz soha nem javasolja például, hogy Prémium készletbe helyezzen egy Standard adatbázist.</span><span class="sxs-lookup"><span data-stu-id="0677e-170">This means the service does not make cross-tier recommendations such as moving a Standard database into a Premium pool.</span></span>

<span data-ttu-id="0677e-171">Adatbázisok hozzáadása a készlethez, után javaslatok dinamikusan jönnek létre a kiválasztott adatbázisok korábbi használati alapján.</span><span class="sxs-lookup"><span data-stu-id="0677e-171">After adding databases to the pool, recommendations are dynamically generated based on the historical usage of the databases you have selected.</span></span> <span data-ttu-id="0677e-172">Ezek a javaslatok láthatók, az eDTU- és GB-használati diagramon, és a javaslat fejléc tetején a **készlet beállítása** panelen.</span><span class="sxs-lookup"><span data-stu-id="0677e-172">These recommendations are shown in the eDTU and GB usage chart and in a recommendation banner at the top of the **Configure pool** blade.</span></span> <span data-ttu-id="0677e-173">Ezek a javaslatok célja, hogy az Ön konkrét adatbázisaihoz optimalizált rugalmas készletek létrehozását.</span><span class="sxs-lookup"><span data-stu-id="0677e-173">These recommendations are intended to assist you in creating an elastic pool optimized for your specific databases.</span></span>

![dinamikus javaslatok](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## <a name="manage-and-monitor-an-elastic-pool"></a><span data-ttu-id="0677e-175">Kezelni és megfigyelni a rugalmas készlethez</span><span class="sxs-lookup"><span data-stu-id="0677e-175">Manage and monitor an elastic pool</span></span>

<span data-ttu-id="0677e-176">Az Azure portál segítségével rugalmas készletek és a készletben lévő adatbázisok felügyeletét és kezelését.</span><span class="sxs-lookup"><span data-stu-id="0677e-176">You can use the Azure portal to monitor and manage an elastic pool and the databases in the pool.</span></span> <span data-ttu-id="0677e-177">A portálról figyelheti a rugalmas készletek és a készlethez tartozó adatbázis-felhasználását.</span><span class="sxs-lookup"><span data-stu-id="0677e-177">From the portal, you can monitor the utilization of an elastic pool and the databases within that pool.</span></span> <span data-ttu-id="0677e-178">Módosítások készlete teheti a rugalmas készlethez és egyszerre az összes változtatás is.</span><span class="sxs-lookup"><span data-stu-id="0677e-178">You can also make a set of changes to your elastic pool and submit all changes at the same time.</span></span> <span data-ttu-id="0677e-179">Ezen változtatások közé tartozik a Hozzáadás, adatbázisok, a rugalmas készlet beállításainak módosítása, vagy nem módosíthatja az adatbázis-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="0677e-179">These changes include adding or removing databases, changing your elastic pool settings, or changing your database settings.</span></span>

<span data-ttu-id="0677e-180">A következő ábrán látható egy példa a rugalmas készlet.</span><span class="sxs-lookup"><span data-stu-id="0677e-180">The following graphic shows an example elastic pool.</span></span> <span data-ttu-id="0677e-181">A nézet tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="0677e-181">The view includes:</span></span>

*  <span data-ttu-id="0677e-182">A rugalmas készlet és a készletben lévő adatbázisok mind az erőforrás-használatát figyelés diagramokat.</span><span class="sxs-lookup"><span data-stu-id="0677e-182">Charts for monitoring resource usage of both the elastic pool and the databases contained in the pool.</span></span>
*  <span data-ttu-id="0677e-183">A **konfigurálása** készlet gombra kattintva módosíthatja a rugalmas készlethez.</span><span class="sxs-lookup"><span data-stu-id="0677e-183">The **Configure** pool button to make changes to the elastic pool.</span></span>
*  <span data-ttu-id="0677e-184">A **adatbázis létrehozása** , amely adatbázist hoz létre, és hozzáadja a jelenlegi rugalmas készlet gombra.</span><span class="sxs-lookup"><span data-stu-id="0677e-184">The **Create database** button that creates a database and adds it to the current elastic pool.</span></span>
*  <span data-ttu-id="0677e-185">Rugalmas feladat, amelyek segítenek adatbázisok nagy számú egy listán szereplő összes adatbázisokhoz Transact-SQL-parancsprogramok futtatásával kezelhető.</span><span class="sxs-lookup"><span data-stu-id="0677e-185">Elastic jobs that help you manage large numbers of databases by running Transact SQL scripts against all databases in a list.</span></span>

![Készlet megtekintése][2]

<span data-ttu-id="0677e-187">Lépjen egy adott alkalmazáskészlet az erőforrás-használat megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0677e-187">You can go to a particular pool to see its resource utilization.</span></span> <span data-ttu-id="0677e-188">Alapértelmezés szerint a be van állítva az tárolási és eDTU-használat megjelenítése az elmúlt egy óra.</span><span class="sxs-lookup"><span data-stu-id="0677e-188">By default, the pool is configured to show storage and eDTU usage for the last hour.</span></span> <span data-ttu-id="0677e-189">A diagram beállítható úgy, hogy különböző metrikák megjelenítése különböző idő windows keresztül.</span><span class="sxs-lookup"><span data-stu-id="0677e-189">The chart can be configured to show different metrics over various time windows.</span></span>

1. <span data-ttu-id="0677e-190">Válassza ki a rugalmas készlethez történő együttműködésre.</span><span class="sxs-lookup"><span data-stu-id="0677e-190">Select an elastic pool to work with.</span></span>
2. <span data-ttu-id="0677e-191">A **rugalmas készlet figyelése** feliratú diagram **erőforrás-használat**.</span><span class="sxs-lookup"><span data-stu-id="0677e-191">Under **Elastic Pool Monitoring** is a chart labeled **Resource utilization**.</span></span> <span data-ttu-id="0677e-192">Kattintson a diagramban.</span><span class="sxs-lookup"><span data-stu-id="0677e-192">Click the chart.</span></span>

    ![A rugalmas készlet figyelése][3]

    <span data-ttu-id="0677e-194">A **metrika** panel nyílik meg, a megadott metrikák részletes nézete megjeleníti a megadott időszak során.</span><span class="sxs-lookup"><span data-stu-id="0677e-194">The **Metric** blade opens, showing a detailed view of the specified metrics over the specified time window.</span></span>   

    ![Metrika panel][9]

### <a name="to-customize-the-chart-display"></a><span data-ttu-id="0677e-196">A diagram megjelenítéséhez</span><span class="sxs-lookup"><span data-stu-id="0677e-196">To customize the chart display</span></span>

<span data-ttu-id="0677e-197">A diagram és más metrikákkal, például a Processzor százalékos, adat IO százalékos és napló IO százalékos használt megjelenítendő mérték panel szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="0677e-197">You can edit the chart and the metric blade to display other metrics such as CPU percentage, data IO percentage, and log IO percentage used.</span></span>

1. <span data-ttu-id="0677e-198">A metrika paneljén kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="0677e-198">On the metric blade, click **Edit**.</span></span>

    ![Kattintson a Szerkesztés][6]

2. <span data-ttu-id="0677e-200">Az a **diagram szerkesztése lehetőséget** panelen válasszon ki egy időtartományt (óránként, napjainkban túlra vagy elmúlt hét), vagy kattintson **egyéni** az elmúlt két hétben semmilyen dátumtartomány kijelöléséhez.</span><span class="sxs-lookup"><span data-stu-id="0677e-200">In the **Edit Chart** blade, select a time range (past hour, today, or past week), or click **custom** to select any date range in the last two weeks.</span></span> <span data-ttu-id="0677e-201">Válassza ki a diagram (vonal vagy sáv), majd válassza ki az erőforrásokat a figyelheti.</span><span class="sxs-lookup"><span data-stu-id="0677e-201">Select the chart type (bar or line), then select the resources to monitor.</span></span>

   > [!Note]
   > <span data-ttu-id="0677e-202">Mértékegység azonos mértékek csak a diagramon megjeleníthető egy időben.</span><span class="sxs-lookup"><span data-stu-id="0677e-202">Only metrics with the same unit of measure can be displayed in the chart at the same time.</span></span> <span data-ttu-id="0677e-203">Például ha "eDTU százaléka" majd csak választhat más metrikákkal érintő mértékegysége.</span><span class="sxs-lookup"><span data-stu-id="0677e-203">For example, if you select "eDTU percentage" then you can only select other metrics with percentage as the unit of measure.</span></span>
   >

    ![Kattintson a Szerkesztés](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

3. <span data-ttu-id="0677e-205">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="0677e-205">Then click **OK**.</span></span>

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a><span data-ttu-id="0677e-206">Kezelni és megfigyelni a adatbázisok rugalmas készlethez</span><span class="sxs-lookup"><span data-stu-id="0677e-206">Manage and monitor databases in an elastic pool</span></span>

<span data-ttu-id="0677e-207">Az egyes adatbázisok is figyelhetők meg potenciális problémák.</span><span class="sxs-lookup"><span data-stu-id="0677e-207">Individual databases can also be monitored for potential trouble.</span></span>

1. <span data-ttu-id="0677e-208">A **rugalmas adatbázis-figyelési**, öt adatbázisok metrikáját megjelenítő diagram.</span><span class="sxs-lookup"><span data-stu-id="0677e-208">Under **Elastic Database Monitoring**, there is a chart that displays metrics for five databases.</span></span> <span data-ttu-id="0677e-209">Alapértelmezés szerint a diagramot jelenít meg a felső 5 adatbázisok a készlet átlagos edtu-k által az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="0677e-209">By default, the chart displays the top 5 databases in the pool by average eDTU usage in the past hour.</span></span> <span data-ttu-id="0677e-210">Kattintson a diagramban.</span><span class="sxs-lookup"><span data-stu-id="0677e-210">Click the chart.</span></span>

    ![A rugalmas készlet figyelése][4]

2. <span data-ttu-id="0677e-212">A **adatbázis erőforrás-használat** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0677e-212">The **Database Resource Utilization** blade appears.</span></span> <span data-ttu-id="0677e-213">Ez az adatbázis-használat a készletben található részletes áttekintést nyújt a.</span><span class="sxs-lookup"><span data-stu-id="0677e-213">This provides a detailed view of the database usage in the pool.</span></span> <span data-ttu-id="0677e-214">A rács a panel alsó részén használ, választhatja adatbázisoknak a tárolókészlet megjeleníti a használatát a diagramban (legfeljebb 5 adatbázisok).</span><span class="sxs-lookup"><span data-stu-id="0677e-214">Using the grid in the lower part of the blade, you can select any databases in the pool to display its usage in the chart (up to 5 databases).</span></span> <span data-ttu-id="0677e-215">Testre szabhatja a gombra kattintva a diagramon megjelenő metrikák és idő ablak **diagram szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="0677e-215">You can also customize the metrics and time window displayed in the chart by clicking **Edit chart**.</span></span>

    ![Adatbázis erőforráspaneljének kihasználtsága][8]

### <a name="to-customize-the-view"></a><span data-ttu-id="0677e-217">A nézet testreszabásához</span><span class="sxs-lookup"><span data-stu-id="0677e-217">To customize the view</span></span>

1. <span data-ttu-id="0677e-218">Az a **adatbázis-erőforrás-használat** panelen kattintson a **diagram szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="0677e-218">In the **Database resource utilization** blade, click **Edit chart**.</span></span>

    ![Kattintson a diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. <span data-ttu-id="0677e-220">Az a **szerkesztése** diagram panelen jelölje be egy időtartományt (óránként túlra vagy elmúlt 24 óra), vagy kattintson a **egyéni** különböző naponta az elmúlt 2 hét megjelenítéséhez jelölje ki.</span><span class="sxs-lookup"><span data-stu-id="0677e-220">In the **Edit** chart blade, select a time range (past hour or past 24 hours), or click **custom** to select a different day in the past 2 weeks to display.</span></span>

    ![Kattintson az egyéni](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. <span data-ttu-id="0677e-222">Kattintson a **hasonlítsa össze az adatbázisok által** jelöljön ki egy másik metrikát adatbázisok összehasonlításakor használandó a legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="0677e-222">Click the **Compare databases by** dropdown to select a different metric to use when comparing databases.</span></span>

    ![A diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a><span data-ttu-id="0677e-224">Jelölje be az adatbázisok figyelése</span><span class="sxs-lookup"><span data-stu-id="0677e-224">To select databases to monitor</span></span>

<span data-ttu-id="0677e-225">Az adatbázis listáján, a **adatbázis erőforrás-használat** panelen található adott adatbázisok között a listában lévő lapokat vagy egy adatbázis nevében beírásával.</span><span class="sxs-lookup"><span data-stu-id="0677e-225">In the database list in the **Database Resource Utilization** blade, you can find particular databases by looking through the pages in the list or by typing in the name of a database.</span></span> <span data-ttu-id="0677e-226">A jelölőnégyzet segítségével válassza ki az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="0677e-226">Use the checkbox to select the database.</span></span>

![Adatbázisok figyelése keresése][7]


## <a name="add-an-alert-to-an-elastic-pool-resource"></a><span data-ttu-id="0677e-228">Riasztás egy rugalmas készlet erőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0677e-228">Add an alert to an elastic pool resource</span></span>

<span data-ttu-id="0677e-229">Szabályokat adhat hozzá egy rugalmas készlet, amely e-mailt küld URL-cím végpontok személyek vagy riasztás karakterláncokkal, amikor a rugalmas készlet találatok egy Ön által beállított használati küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="0677e-229">You can add rules to an elastic pool that send email to people or alert strings to URL endpoints when the elastic pool hits a utilization threshold that you set up.</span></span>

<span data-ttu-id="0677e-230">**Bármilyen olyan erőforrás riasztást hozzáadása:**</span><span class="sxs-lookup"><span data-stu-id="0677e-230">**To add an alert to any resource:**</span></span>

1. <span data-ttu-id="0677e-231">Kattintson a **erőforrás-használat** a diagram a **metrika** panelen kattintson a **riasztás hozzáadása**, majd adja ki a **riasztásiszabályfelvétele** panel (**erőforrás** automatikusan be kell állítani a készlet dolgozunk kell).</span><span class="sxs-lookup"><span data-stu-id="0677e-231">Click the **Resource utilization** chart to open the **Metric** blade, click **Add alert**, and then fill out the information in the **Add an alert rule** blade (**Resource** is automatically set up to be the pool you're working with).</span></span>
2. <span data-ttu-id="0677e-232">Adjon meg egy **neve** és **leírás** , amely azonosítja a kívánt riasztást, és a címzetteket.</span><span class="sxs-lookup"><span data-stu-id="0677e-232">Type a **Name** and **Description** that identifies the alert to you and the recipients.</span></span>
3. <span data-ttu-id="0677e-233">Válasszon egy **metrika** , amelyet szeretne riasztást a listából.</span><span class="sxs-lookup"><span data-stu-id="0677e-233">Choose a **Metric** that you want to alert from the list.</span></span>

    <span data-ttu-id="0677e-234">A diagram dinamikusan segítségével válassza ki a küszöbértéket, hogy a metrika erőforrás-használat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="0677e-234">The chart dynamically shows resource utilization for that metric to help you choose a threshold.</span></span>

4. <span data-ttu-id="0677e-235">Válasszon egy **feltétel** (nagyobb, kisebb, mint, stb) és egy **küszöbérték**.</span><span class="sxs-lookup"><span data-stu-id="0677e-235">Choose a **Condition** (greater than, less than, etc.) and a **Threshold**.</span></span>
5. <span data-ttu-id="0677e-236">Válasszon egy **időszak** idő a metrika szabály a riasztási eseményindítók előtt kell biztosítani.</span><span class="sxs-lookup"><span data-stu-id="0677e-236">Choose a **Period** of time that the metric rule must be satisfied before the alert triggers.</span></span>
6. <span data-ttu-id="0677e-237">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="0677e-237">Click **OK**.</span></span>

<span data-ttu-id="0677e-238">További információkért lásd: [SQL-adatbázis figyelmeztetések létrehozása az Azure-portálon](sql-database-insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0677e-238">For more information, see [create SQL Database alerts in Azure portal](sql-database-insights-alerts-portal.md).</span></span>

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="0677e-239">Egy adatbázis áthelyezése rugalmas készletbe</span><span class="sxs-lookup"><span data-stu-id="0677e-239">Move a database into an elastic pool</span></span>

<span data-ttu-id="0677e-240">Adja hozzá, vagy távolítsa el az adatbázisokat egy meglévő készletből.</span><span class="sxs-lookup"><span data-stu-id="0677e-240">You can add or remove databases from an existing pool.</span></span> <span data-ttu-id="0677e-241">Az adatbázisok más készletek is szerepelhet.</span><span class="sxs-lookup"><span data-stu-id="0677e-241">The databases can be in other pools.</span></span> <span data-ttu-id="0677e-242">Azonban csak adhat hozzá adatbázisok, amelyek ugyanazon a logikai kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="0677e-242">However, you can only add databases that are on the same logical server.</span></span>

1. <span data-ttu-id="0677e-243">A készlet panelen a **rugalmas adatbázisok** kattintson **készlet beállítása**.</span><span class="sxs-lookup"><span data-stu-id="0677e-243">In the blade for the pool, under **Elastic databases** click **Configure pool**.</span></span>

    ![Kattintson a készlet konfigurálása][1]

2. <span data-ttu-id="0677e-245">Az a **készlet beállítása** panelen kattintson a **hozzá készlethez**.</span><span class="sxs-lookup"><span data-stu-id="0677e-245">In the **Configure pool** blade, click **Add to pool**.</span></span>

    ![Kattintson a Hozzáadás gombra a készlethez](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. <span data-ttu-id="0677e-247">Az a **adatbázisok hozzáadása** panelen válassza ki az adatbázist vagy -adatbázisokat adhat hozzá a készlethez.</span><span class="sxs-lookup"><span data-stu-id="0677e-247">In the **Add databases** blade, select the database or databases to add to the pool.</span></span> <span data-ttu-id="0677e-248">Kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="0677e-248">Then click **Select**.</span></span>

    ![Válassza ki az adatbázisok hozzáadása](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    <span data-ttu-id="0677e-250">A **készlet beállítása** panel most sorolja fel lehet hozzáadni, az állapot beállítása a kijelölt adatbázis **függőben lévő**.</span><span class="sxs-lookup"><span data-stu-id="0677e-250">The **Configure pool** blade now lists the database you selected to be added, with its status set to **Pending**.</span></span>

    ![Függőben lévő készlet elemek felvétele](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. <span data-ttu-id="0677e-252">Az a **konfigurálás készlet paneljén**, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="0677e-252">In the **Configure pool blade**, click **Save**.</span></span>

    ![Kattintson a Save (Mentés) gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a><span data-ttu-id="0677e-254">Egy adatbázis áthelyezése rugalmas készletek kívül</span><span class="sxs-lookup"><span data-stu-id="0677e-254">Move a database out of an elastic pool</span></span>

1. <span data-ttu-id="0677e-255">Az a **készlet beállítása** panelen válassza ki az adatbázis vagy az adatbázis eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="0677e-255">In the **Configure pool** blade, select the database or databases to remove.</span></span>

    ![adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. <span data-ttu-id="0677e-257">Kattintson a **Eltávolítás a készletből**.</span><span class="sxs-lookup"><span data-stu-id="0677e-257">Click **Remove from pool**.</span></span>

    ![adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    <span data-ttu-id="0677e-259">A **készlet beállítása** panel most már tartalmazza az adatbázis állapotának beállítása az eltávolításra kijelölt **függőben lévő**.</span><span class="sxs-lookup"><span data-stu-id="0677e-259">The **Configure pool** blade now lists the database you selected to be removed with its status set to **Pending**.</span></span>

    ![előzetes adatbázis hozzáadásának és eltávolításának](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. <span data-ttu-id="0677e-261">Az a **konfigurálás készlet paneljén**, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="0677e-261">In the **Configure pool blade**, click **Save**.</span></span>

    ![Kattintson a Save (Mentés) gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a><span data-ttu-id="0677e-263">Egy rugalmas készlet teljesítmény beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="0677e-263">Change performance settings of an elastic pool</span></span>

<span data-ttu-id="0677e-264">Ahogy figyeli az erőforrás-használat rugalmas készlet, azt tapasztalhatja, hogy szükség van-e módosításra.</span><span class="sxs-lookup"><span data-stu-id="0677e-264">As you monitor the resource utilization of an elastic pool, you may discover that some adjustments are needed.</span></span> <span data-ttu-id="0677e-265">Lehet, hogy a készletben kell a teljesítményt és a tárolást korlátok változását.</span><span class="sxs-lookup"><span data-stu-id="0677e-265">Maybe the pool needs a change in the performance or storage limits.</span></span> <span data-ttu-id="0677e-266">Esetleg módosítani szeretné az adatbázis-beállításai a készletben.</span><span class="sxs-lookup"><span data-stu-id="0677e-266">Possibly you want to change the database settings in the pool.</span></span> <span data-ttu-id="0677e-267">A telepítő a készlet lekérni a legjobb egyenlege teljesítményének és költséghatékonyságának bármikor módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="0677e-267">You can change the setup of the pool at any time to get the best balance of performance and cost.</span></span> <span data-ttu-id="0677e-268">Lásd: [amikor rugalmas készletek használandó?](sql-database-elastic-pool.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="0677e-268">See [When should an elastic pool be used?](sql-database-elastic-pool.md) for more information.</span></span>

<span data-ttu-id="0677e-269">A edtu-inak vagy tárolási korlátai készletenként és edtu-k adatbázisonkénti módosítása:</span><span class="sxs-lookup"><span data-stu-id="0677e-269">To change the eDTUs or storage limits per pool, and eDTUs per database:</span></span>

1. <span data-ttu-id="0677e-270">Nyissa meg a **készlet beállítása** panelen.</span><span class="sxs-lookup"><span data-stu-id="0677e-270">Open the **Configure pool** blade.</span></span>

    <span data-ttu-id="0677e-271">A **rugalmas készlet beállítások**, vagy a csúszka segítségével módosítsa a készlet beállításait.</span><span class="sxs-lookup"><span data-stu-id="0677e-271">Under **elastic pool settings**, use either slider to change the pool settings.</span></span>

    ![A rugalmas készlet erőforrás-használat](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. <span data-ttu-id="0677e-273">Ha módosítja a beállítást, a megjelenítési a módosítás következményeivel becsült havi költségét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="0677e-273">When the setting changes, the display shows the estimated monthly cost of the change.</span></span>

    ![Egy rugalmas készlet és új havi költségét frissítése folyamatban](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a><span data-ttu-id="0677e-275">A rugalmas készlet műveletek várakozási ideje</span><span class="sxs-lookup"><span data-stu-id="0677e-275">Latency of elastic pool operations</span></span>
* <span data-ttu-id="0677e-276">Másodpercenkénti adatbázis vagy a maximális edtu-k adatbázisonkénti minimális edtu-k általában módosítása befejeződött, kevesebb mint 5 perc alatt.</span><span class="sxs-lookup"><span data-stu-id="0677e-276">Changing the min eDTUs per database or max eDTUs per database typically completes in 5 minutes or less.</span></span>
* <span data-ttu-id="0677e-277">Módosítása edtu-inak száma attól függ, hogy mekkora a készletben lévő összes adatbázisok által felhasznált lemezterület mérete.</span><span class="sxs-lookup"><span data-stu-id="0677e-277">Changing the eDTUs per pool depends on the total amount of space used by all databases in the pool.</span></span> <span data-ttu-id="0677e-278">A módosítások átlagosan 100 gigabájtonként legfeljebb 90 percet vesznek igénybe.</span><span class="sxs-lookup"><span data-stu-id="0677e-278">Changes average 90 minutes or less per 100 GB.</span></span> <span data-ttu-id="0677e-279">Például által használt a teljes lemezterület a készletben lévő összes adatbázisok esetén 200 GB-os, akkor a készlet eDTU-készlet módosítására a várt várakozási 3 óra vagy annál kisebb.</span><span class="sxs-lookup"><span data-stu-id="0677e-279">For example, if the total space used by all databases in the pool is 200 GB, then the expected latency for changing the pool eDTU per pool is 3 hours or less.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0677e-280">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0677e-280">Next steps</span></span>

- <span data-ttu-id="0677e-281">Ismertetése mi rugalmas készletek esetén: [SQL Database rugalmas készlet](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="0677e-281">To understand what an elastic pool is, see [SQL Database elastic pool](sql-database-elastic-pool.md).</span></span>
- <span data-ttu-id="0677e-282">A rugalmas készleteket használó útmutatóért lásd: [rugalmas készletek ára és teljesítménye szempontjai](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="0677e-282">For guidance on using elastic pools, see [Price and performance considerations for elastic pools](sql-database-elastic-pool.md).</span></span>
- <span data-ttu-id="0677e-283">Rugalmas feladat segítségével futtassa a Transact-SQL-szkriptek használatát a készletben lévő adatbázisok tetszőleges számú, lásd: [rugalmas feladatok áttekintése](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0677e-283">To use elastic jobs to run Transact-SQL scripts against any number of databases in the pool, see [Elastic jobs overview](sql-database-elastic-jobs-overview.md).</span></span>
- <span data-ttu-id="0677e-284">A készletben lévő adatbázisok tetszőleges számú átfogó lekérdezése, lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0677e-284">To query across any number of databases in the pool, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
- <span data-ttu-id="0677e-285">Tetszőleges számú adatbázishoz a tárolókészletben, lásd: tranzakciók [rugalmas tranzakciók](sql-database-elastic-transactions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0677e-285">For transactions any number of databases in the pool, see [Elastic transactions](sql-database-elastic-transactions-overview.md).</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
