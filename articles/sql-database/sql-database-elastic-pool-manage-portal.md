---
title: "Azure-portálon: egy SQL Database rugalmas készlet kezelése & létrehozása |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure-portál és az SQL Database beépített funkciói toomanage, a figyelő és a megfelelő méretének kiválasztásában egy méretezhető rugalmas készlet toooptimize adatbázis teljesítménye és költségek kezelésére."
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
ms.openlocfilehash: e0de952bc0c91177f64c04363630783d72435741
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-an-elastic-pool-with-hello-azure-portal"></a><span data-ttu-id="f2afc-103">Létrehozása és kezelése az Azure-portálon hello rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="f2afc-103">Create and manage an elastic pool with hello Azure portal</span></span>
<span data-ttu-id="f2afc-104">Ez a témakör bemutatja, hogyan toocreate és kezelheti a méretezhető [rugalmas készletek](sql-database-elastic-pool.md) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f2afc-104">This topic shows you how toocreate and manage scalable [elastic pools](sql-database-elastic-pool.md) with hello Azure portal.</span></span> <span data-ttu-id="f2afc-105">Is létrehozása és kezelése az Azure rugalmas készletek [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API-t vagy [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="f2afc-105">You can also create and manage an Azure elastic pool with [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="f2afc-106">Is létrehozhat és adatbázisok áthelyezése esetében bejövő és kimenő használatával rugalmas készletek [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="f2afc-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>

## <a name="create-an-elastic-pool"></a><span data-ttu-id="f2afc-107">Rugalmas készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2afc-107">Create an elastic pool</span></span> 

<span data-ttu-id="f2afc-108">Kétféleképpen hozhat létre egy rugalmas készlet.</span><span class="sxs-lookup"><span data-stu-id="f2afc-108">There are two ways you can create an elastic pool.</span></span> <span data-ttu-id="f2afc-109">Ezt megteheti a teljesen Ha hello javasolni, vagy kezdje hello szolgáltatás ajánlása tudja.</span><span class="sxs-lookup"><span data-stu-id="f2afc-109">You can do it from scratch if you know hello pool setup you want, or start with a recommendation from hello service.</span></span> <span data-ttu-id="f2afc-110">SQL Database beépített funkciói képesek készletbeállítást egy rugalmas készlet telepítő, ha gazdaságosabb múltbeli használat telemetriai adatai az adatbázisok esetében hello alapján automatikusan rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f2afc-110">SQL Database has built-in intelligence that recommends an elastic pool setup if it's more cost-efficient for you based on hello past usage telemetry for your databases.</span></span>

<span data-ttu-id="f2afc-111">Egy kiszolgálón több készletet is létrehozhat, de a hello különböző kiszolgálókról származó adatbázisok nem adhat azonos erőforráskészletben.</span><span class="sxs-lookup"><span data-stu-id="f2afc-111">You can create multiple pools on a server, but you can't add databases from different servers into hello same pool.</span></span> 

> [!NOTE]
> <span data-ttu-id="f2afc-112">A rugalmas készletek minden Azure-régióban általánosan elérhetők, kivéve Nyugat-Indiát, ahol a szolgáltatás jelenleg előzetes verzióként érhető el.</span><span class="sxs-lookup"><span data-stu-id="f2afc-112">Elastic pools are generally available (GA) in all Azure regions except West India where it is currently in preview.</span></span>  <span data-ttu-id="f2afc-113">A rugalmas készletek a lehető leghamarabb általánosan elérhetők lesznek ebben a régióban.</span><span class="sxs-lookup"><span data-stu-id="f2afc-113">GA of elastic pools in this region will occur as soon as possible.</span></span>
>

### <a name="step-1-create-an-elastic-pool"></a><span data-ttu-id="f2afc-114">1. lépés: Egy rugalmas készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2afc-114">Step 1: Create an elastic pool</span></span>

<span data-ttu-id="f2afc-115">Egy rugalmas készlet létrehozása a meglévő **server** hello portál panel hello legegyszerűbb módja toomove meglévő adatbázisok rugalmas készletbe.</span><span class="sxs-lookup"><span data-stu-id="f2afc-115">Creating an elastic pool from an existing **server** blade in hello portal is hello easiest way toomove existing databases into an elastic pool.</span></span>

> [!NOTE]
> <span data-ttu-id="f2afc-116">Rugalmas készletek keresve is létrehozhat **SQL rugalmas készlet** a hello **piactér** vagy kattint **+ Hozzáadás** a hello **SQL rugalmas készletek**keresse meg a panelt.</span><span class="sxs-lookup"><span data-stu-id="f2afc-116">You can also create an elastic pool by searching **SQL elastic pool** in hello **Marketplace** or clicking **+Add** in hello **SQL elastic pools** browse blade.</span></span> <span data-ttu-id="f2afc-117">Biztosan tudja toospecify egy új vagy meglévő kiszolgáló ezzel a készlettel munkafolyamat kiépítés keresztül.</span><span class="sxs-lookup"><span data-stu-id="f2afc-117">You are able toospecify a new or existing server through this pool provisioning workflow.</span></span>
>
>

1. <span data-ttu-id="f2afc-118">A hello [Azure-portálon](http://portal.azure.com/), kattintson a **további szolgáltatások**  **>**  **SQL Server-kiszolgálók**, és kattintson a hello tartalmazó hello kiszolgálón tooadd tooan rugalmas készlet kívánt adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="f2afc-118">In hello [Azure portal](http://portal.azure.com/), click **More services** **>** **SQL servers**, and then click hello server that contains hello databases you want tooadd tooan elastic pool.</span></span>
2. <span data-ttu-id="f2afc-119">Kattintson a **Új készlet** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="f2afc-119">Click **New pool**.</span></span>

    ![Készlet tooa kiszolgáló hozzáadása](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    <span data-ttu-id="f2afc-121">**-VAGY-**</span><span class="sxs-lookup"><span data-stu-id="f2afc-121">**-OR-**</span></span>

    <span data-ttu-id="f2afc-122">Megjelenik egy üzenet, amely tájékoztatja, hogy ajánlott rugalmas készletek hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f2afc-122">You may see a message saying there are recommended elastic pools for hello server.</span></span> <span data-ttu-id="f2afc-123">Hello üzenet toosee hello készletek korábbi adatbázis használat telemetriai adatai alapján javasolt kattintson, majd kattintson a hello réteg toosee további részleteket, és testre szabhatja a hello készlet.</span><span class="sxs-lookup"><span data-stu-id="f2afc-123">Click hello message toosee hello recommended pools based on historical database usage telemetry, and then click hello tier toosee more details and customize hello pool.</span></span> <span data-ttu-id="f2afc-124">Lásd: [készlettel kapcsolatos javaslatok megértése](#understand-elastic-pool-recommendations) a témakör későbbi részében a hello javaslatokkal módját.</span><span class="sxs-lookup"><span data-stu-id="f2afc-124">See [Understand pool recommendations](#understand-elastic-pool-recommendations) later in this topic for how hello recommendation is made.</span></span>

    ![javasolt készlet](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. <span data-ttu-id="f2afc-126">Hello **rugalmas készlet** panel jelenik meg, amely, amelyben meg kell határoznia hello-beállítások a készlet.</span><span class="sxs-lookup"><span data-stu-id="f2afc-126">hello **elastic pool** blade appears, which is where you specify hello settings for your pool.</span></span> <span data-ttu-id="f2afc-127">Kattintott **új készletet** hello előző lépést, az IP-címek hello az **szabványos** alapértelmezett és az adatbázisok nem van jelölve.</span><span class="sxs-lookup"><span data-stu-id="f2afc-127">If you clicked **New pool** in hello previous step, hello pricing tier is **Standard** by default and no databases is selected.</span></span> <span data-ttu-id="f2afc-128">Hozzon létre egy üres címkészletet, vagy adjon meg egy adott kiszolgáló toomove meglévő adatbázisok hello készletbe.</span><span class="sxs-lookup"><span data-stu-id="f2afc-128">You can create an empty pool, or specify a set of existing databases from that server toomove into hello pool.</span></span> <span data-ttu-id="f2afc-129">A javasolt készlet létrehozásakor, hello ajánlott IP-címek, teljesítménybeállításokat, és az adatbázisok listája előre van feltöltve, de továbbra is módosíthatja őket.</span><span class="sxs-lookup"><span data-stu-id="f2afc-129">If you are creating a recommended pool, hello recommended pricing tier, performance settings, and list of databases are prepopulated, but you can still change them.</span></span>

    ![A rugalmas készlet konfigurálása](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. <span data-ttu-id="f2afc-131">Adjon meg egy nevet a rugalmas készlet hello, vagy hagyja meg az alapértelmezett hello.</span><span class="sxs-lookup"><span data-stu-id="f2afc-131">Specify a name for hello elastic pool, or leave it as hello default.</span></span>

### <a name="step-2-choose-a-pricing-tier"></a><span data-ttu-id="f2afc-132">2. lépés: Tarifacsomag kiválasztása</span><span class="sxs-lookup"><span data-stu-id="f2afc-132">Step 2: Choose a pricing tier</span></span>

<span data-ttu-id="f2afc-133">hello készlet árképzési szint határozza meg hello funkció elérhető toohello elastics hello készlet és hello legfeljebb hány Edtu (eDTU MAX) és tárhelyet (GB) elérhető tooeach adatbázis.</span><span class="sxs-lookup"><span data-stu-id="f2afc-133">hello pool's pricing tier determines hello features available toohello elastics in hello pool, and hello maximum number of eDTUs (eDTU MAX), and storage (GBs) available tooeach database.</span></span> <span data-ttu-id="f2afc-134">A részletekért lásd a tarifacsomagokról szóló cikket.</span><span class="sxs-lookup"><span data-stu-id="f2afc-134">For details, see Service Tiers.</span></span>

<span data-ttu-id="f2afc-135">toochange hello tarifacsomag hello készlet, kattintson a **tarifacsomag**, kattintson az IP-címek, és kattintson hello **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-135">toochange hello pricing tier for hello pool, click **Pricing tier**, click hello pricing tier you want, and then click **Select**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f2afc-136">Hello tarifacsomag kiválasztása és a változtatások véglegesítése a határidő kattintva után **OK** hello utolsó lépésként hello készlet tarifacsomagjának képes toochange hello nem lesz.</span><span class="sxs-lookup"><span data-stu-id="f2afc-136">After you choose hello pricing tier and commit your changes by clicking **OK** in hello last step, you won't be able toochange hello pricing tier of hello pool.</span></span> <span data-ttu-id="f2afc-137">toochange hello egy meglévő rugalmas készlet tarifacsomagjának, rugalmas készletet létrehozni hello kívánt tarifacsomagot, és át hello adatbázisok toothis új készletet.</span><span class="sxs-lookup"><span data-stu-id="f2afc-137">toochange hello pricing tier for an existing elastic pool, create an elastic pool in hello desired pricing tier and migrate hello databases toothis new pool.</span></span>
>

![Tarifacsomag kiválasztása](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-hello-pool"></a><span data-ttu-id="f2afc-139">3. lépés: Hello készlet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f2afc-139">Step 3: Configure hello pool</span></span>

<span data-ttu-id="f2afc-140">IP-címek hello beállítása után kattintson konfigurálása készlet adatbázisok, a set készlet edtu-k és tárhely (GB-ban) vehet fel, és hello minimális és maximális edtu-k a hello elastics hello készletben állíthatja.</span><span class="sxs-lookup"><span data-stu-id="f2afc-140">After setting hello pricing tier, click Configure pool where you add databases, set pool eDTUs and storage (pool GBs), and where you set hello min and max eDTUs for hello elastics in hello pool.</span></span>

1. <span data-ttu-id="f2afc-141">Kattintson a **Készlet beállítása** elemre.</span><span class="sxs-lookup"><span data-stu-id="f2afc-141">Click **Configure pool**</span></span>
2. <span data-ttu-id="f2afc-142">Válassza ki a kívánt tooadd toohello készlet hello adatbázisokat.</span><span class="sxs-lookup"><span data-stu-id="f2afc-142">Select hello databases you want tooadd toohello pool.</span></span> <span data-ttu-id="f2afc-143">Ez a lépés nem kötelező hello alkalmazáskészlet létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="f2afc-143">This step is optional while creating hello pool.</span></span> <span data-ttu-id="f2afc-144">Adatbázisok hello készlet létrehozása után adhatók hozzá.</span><span class="sxs-lookup"><span data-stu-id="f2afc-144">Databases can be added after hello pool has been created.</span></span>
    <span data-ttu-id="f2afc-145">tooadd adatbázisokat, kattintson a **adatbázis hozzáadása**, hello adatbázisok, hogy szeretné, hogy tooadd, és kattintson a hello kattintson **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="f2afc-145">tooadd databases, click **Add database**, click hello databases that you want tooadd, and then click hello **Select** button.</span></span>

    ![Adatbázisok hozzáadása](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    <span data-ttu-id="f2afc-147">Ha dolgozunk hello adatbázisokhoz elegendő korábbi használati telemetriai adat, hello **becsült eDTU- és GB-használati** grafikon és hello **tényleges edtu-k** sávdiagram frissítés toohelp elvégezte a konfiguráció döntéseket.</span><span class="sxs-lookup"><span data-stu-id="f2afc-147">If hello databases you're working with have enough historical usage telemetry, hello **Estimated eDTU and GB usage** graph and hello **Actual eDTU usage** bar chart update toohelp you make configuration decisions.</span></span> <span data-ttu-id="f2afc-148">Emellett hello szolgáltatást is megjelenít, egy javaslat üzenet toohelp készlet hello akkor megfelelő méretének kiválasztásában.</span><span class="sxs-lookup"><span data-stu-id="f2afc-148">Also, hello service may give you a recommendation message toohelp you right-size hello pool.</span></span> <span data-ttu-id="f2afc-149">Lásd: [Dinamikus javaslatok](#understand-elastic-pool-recommendations).</span><span class="sxs-lookup"><span data-stu-id="f2afc-149">See [Dynamic Recommendations](#understand-elastic-pool-recommendations).</span></span>

3. <span data-ttu-id="f2afc-150">Hello hello vezérlők használhatók **készlet beállítása** tooexplore beállítások lapon, és konfigurálhatja a készletet.</span><span class="sxs-lookup"><span data-stu-id="f2afc-150">Use hello controls on hello **Configure pool** page tooexplore settings and configure your pool.</span></span> <span data-ttu-id="f2afc-151">Lásd: [rugalmas készletek korlátok](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) további részletes információt az egyes szolgáltatásszinteken határértékeit, és tanulmányozza a [rugalmas készletek ára és teljesítménye szempontjai](sql-database-elastic-pool.md) részletes útmutatást megfelelő méretének kiválasztását a rugalmas készletekben.</span><span class="sxs-lookup"><span data-stu-id="f2afc-151">See [Elastic pools limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for more detail about limits for each service tier, and see [Price and performance considerations for elastic pools](sql-database-elastic-pool.md) for detailed guidance on right-sizing an elastic pool.</span></span> <span data-ttu-id="f2afc-152">Készlet beállításaival kapcsolatos további információkért lásd: [rugalmas készlet tulajdonságok](sql-database-elastic-pool.md#database-properties-for-pooled-databases).</span><span class="sxs-lookup"><span data-stu-id="f2afc-152">For more information about pool settings, see [Elastic pool properties](sql-database-elastic-pool.md#database-properties-for-pooled-databases).</span></span>

    ![A rugalmas készlet konfigurálása](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. <span data-ttu-id="f2afc-154">Kattintson a **válasszon** a hello **készlet beállítása** panel beállításainak módosítása után.</span><span class="sxs-lookup"><span data-stu-id="f2afc-154">Click **Select** in hello **Configure Pool** blade after changing settings.</span></span>
5. <span data-ttu-id="f2afc-155">Kattintson a **OK** toocreate hello készlet.</span><span class="sxs-lookup"><span data-stu-id="f2afc-155">Click **OK** toocreate hello pool.</span></span>

## <a name="understand-elastic-pool-recommendations"></a><span data-ttu-id="f2afc-156">Rugalmas készletekkel kapcsolatos javaslatok megértése</span><span class="sxs-lookup"><span data-stu-id="f2afc-156">Understand elastic pool recommendations</span></span>

<span data-ttu-id="f2afc-157">hello SQL Database szolgáltatás használati előzmények elemzésével, és azt javasolja, hogy egy vagy több készlethez helyett önálló adatbázisok használata esetén.</span><span class="sxs-lookup"><span data-stu-id="f2afc-157">hello SQL Database service evaluates usage history and recommends one or more pools when it is more cost-effective than using single databases.</span></span> <span data-ttu-id="f2afc-158">Minden ajánlást hello server-adatbázisok hello készlet leginkább illő egyedi részhalmazával van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="f2afc-158">Each recommendation is configured with a unique subset of hello server's databases that best fit hello pool.</span></span>

![javasolt készlet](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

<span data-ttu-id="f2afc-160">hello készletjavaslat:</span><span class="sxs-lookup"><span data-stu-id="f2afc-160">hello pool recommendation comprises:</span></span>

- <span data-ttu-id="f2afc-161">Tarifacsomag (alapszintű, Standard, Premium vagy Premium RS) hello készlet</span><span class="sxs-lookup"><span data-stu-id="f2afc-161">A pricing tier for hello pool (Basic, Standard, Premium, or Premium RS)</span></span>
- <span data-ttu-id="f2afc-162">A megfelelő **POOL eDTU** száma (amelyet készletenkénti maximális eDTU-ként is meg lehet határozni)</span><span class="sxs-lookup"><span data-stu-id="f2afc-162">Appropriate **POOL eDTUs** (also called Max eDTUs per pool)</span></span>
- <span data-ttu-id="f2afc-163">Hello **eDTU MAX** és **eDTU, minimális érték** adatbázisonként</span><span class="sxs-lookup"><span data-stu-id="f2afc-163">hello **eDTU MAX** and **eDTU Min** per database</span></span>
- <span data-ttu-id="f2afc-164">hello hello készletbe javasolt adatbázisok listája</span><span class="sxs-lookup"><span data-stu-id="f2afc-164">hello list of recommended databases for hello pool</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f2afc-165">hello szolgáltatás hello telemetriai adatok az elmúlt 30 napban figyelembe veszi amikor ajánló készletek.</span><span class="sxs-lookup"><span data-stu-id="f2afc-165">hello service takes hello last 30 days of telemetry into account when recommending pools.</span></span> <span data-ttu-id="f2afc-166">A rugalmas készletek javaslatokba adatbázis toobe akkor léteznie kell legalább 7 napig.</span><span class="sxs-lookup"><span data-stu-id="f2afc-166">For a database toobe considered as a candidate for an elastic pool, it must exist for at least 7 days.</span></span> <span data-ttu-id="f2afc-167">Azokat az adatbázisokat, amelyeket korábban már elhelyezett egy másik rugalmas készletben, a rendszer nem javasolja újabb rugalmas készletbe való bevonásra.</span><span class="sxs-lookup"><span data-stu-id="f2afc-167">Databases that are already in an elastic pool are not considered as candidates for elastic pool recommendations.</span></span>
>

<span data-ttu-id="f2afc-168">hello szolgáltatás értékeli az erőforrásigényeivel és költséghatékonyságát is egyetlen áthelyezése hello adatbázist az egyes szolgáltatásszinteken hello készletekbe azonos szint.</span><span class="sxs-lookup"><span data-stu-id="f2afc-168">hello service evaluates resource needs and cost effectiveness of moving hello single databases in each service tier into pools of hello same tier.</span></span> <span data-ttu-id="f2afc-169">A rendszer például megvizsgálja, hogy érdemes-e a kiszolgálón található Standard adatbázisokat Standard rugalmas készletté alakítani.</span><span class="sxs-lookup"><span data-stu-id="f2afc-169">For example, all Standard databases on a server are assessed for their fit into a Standard Elastic Pool.</span></span> <span data-ttu-id="f2afc-170">Ez azt jelenti, hogy hello szolgáltatást nem ajánlásokat eltérő szintű például a Standard adatbázis áthelyezése prémium készletbe.</span><span class="sxs-lookup"><span data-stu-id="f2afc-170">This means hello service does not make cross-tier recommendations such as moving a Standard database into a Premium pool.</span></span>

<span data-ttu-id="f2afc-171">A felvett adatbázisok toohello készlet, javaslatok dinamikusan jönnek létre hello hello kiválasztott adatbázisok korábbi használati alapján.</span><span class="sxs-lookup"><span data-stu-id="f2afc-171">After adding databases toohello pool, recommendations are dynamically generated based on hello historical usage of hello databases you have selected.</span></span> <span data-ttu-id="f2afc-172">Ezek az ajánlások a hello eDTU- és GB-használati diagramon, és a javaslat fejléc hello hello tetején látható **készlet beállítása** panelen.</span><span class="sxs-lookup"><span data-stu-id="f2afc-172">These recommendations are shown in hello eDTU and GB usage chart and in a recommendation banner at hello top of hello **Configure pool** blade.</span></span> <span data-ttu-id="f2afc-173">Ezek a javaslatok még a rugalmas készletek létrehozása az Ön konkrét adatbázisaihoz optimalizált tervezett tooassist.</span><span class="sxs-lookup"><span data-stu-id="f2afc-173">These recommendations are intended tooassist you in creating an elastic pool optimized for your specific databases.</span></span>

![dinamikus javaslatok](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## <a name="manage-and-monitor-an-elastic-pool"></a><span data-ttu-id="f2afc-175">Kezelni és megfigyelni a rugalmas készlethez</span><span class="sxs-lookup"><span data-stu-id="f2afc-175">Manage and monitor an elastic pool</span></span>

<span data-ttu-id="f2afc-176">Az Azure portál toomonitor hello használja, és rugalmas készletek és hello adatbázisok hello készlet kezelése.</span><span class="sxs-lookup"><span data-stu-id="f2afc-176">You can use hello Azure portal toomonitor and manage an elastic pool and hello databases in hello pool.</span></span> <span data-ttu-id="f2afc-177">Hello portálról figyelheti a rugalmas készletek és a készlethez tartozó hello adatbázis hello felhasználását.</span><span class="sxs-lookup"><span data-stu-id="f2afc-177">From hello portal, you can monitor hello utilization of an elastic pool and hello databases within that pool.</span></span> <span data-ttu-id="f2afc-178">Is teheti módosítások készlete tooyour rugalmas készlet és küldje el az összes módosulnak hello azonos idő.</span><span class="sxs-lookup"><span data-stu-id="f2afc-178">You can also make a set of changes tooyour elastic pool and submit all changes at hello same time.</span></span> <span data-ttu-id="f2afc-179">Ezen változtatások közé tartozik a Hozzáadás, adatbázisok, a rugalmas készlet beállításainak módosítása, vagy nem módosíthatja az adatbázis-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="f2afc-179">These changes include adding or removing databases, changing your elastic pool settings, or changing your database settings.</span></span>

<span data-ttu-id="f2afc-180">a következő ábra hello látható példa rugalmas készlethez.</span><span class="sxs-lookup"><span data-stu-id="f2afc-180">hello following graphic shows an example elastic pool.</span></span> <span data-ttu-id="f2afc-181">hello nézet tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="f2afc-181">hello view includes:</span></span>

*  <span data-ttu-id="f2afc-182">Diagramok figyelés hello rugalmas készlet és a hello adatbázisok hello készletben található az erőforrás-használatát.</span><span class="sxs-lookup"><span data-stu-id="f2afc-182">Charts for monitoring resource usage of both hello elastic pool and hello databases contained in hello pool.</span></span>
*  <span data-ttu-id="f2afc-183">Hello **konfigurálása** készlet gomb toomake toohello rugalmas készlet változik.</span><span class="sxs-lookup"><span data-stu-id="f2afc-183">hello **Configure** pool button toomake changes toohello elastic pool.</span></span>
*  <span data-ttu-id="f2afc-184">Hello **adatbázis létrehozása** gomb, amely adatbázist hoz létre, és hozzáadja azt toohello aktuális rugalmas készlet.</span><span class="sxs-lookup"><span data-stu-id="f2afc-184">hello **Create database** button that creates a database and adds it toohello current elastic pool.</span></span>
*  <span data-ttu-id="f2afc-185">Rugalmas feladat, amelyek segítenek adatbázisok nagy számú egy listán szereplő összes adatbázisokhoz Transact-SQL-parancsprogramok futtatásával kezelhető.</span><span class="sxs-lookup"><span data-stu-id="f2afc-185">Elastic jobs that help you manage large numbers of databases by running Transact SQL scripts against all databases in a list.</span></span>

![Készlet megtekintése][2]

<span data-ttu-id="f2afc-187">Az erőforrás-használat lépjen tooa adott készlet toosee.</span><span class="sxs-lookup"><span data-stu-id="f2afc-187">You can go tooa particular pool toosee its resource utilization.</span></span> <span data-ttu-id="f2afc-188">Alapértelmezés szerint hello akkor hello elmúlt egy órában konfigurált tooshow tárolási és edtu-k használatát.</span><span class="sxs-lookup"><span data-stu-id="f2afc-188">By default, hello pool is configured tooshow storage and eDTU usage for hello last hour.</span></span> <span data-ttu-id="f2afc-189">hello diagram konfigurált tooshow különböző metrikák lehet különböző idő windows keresztül.</span><span class="sxs-lookup"><span data-stu-id="f2afc-189">hello chart can be configured tooshow different metrics over various time windows.</span></span>

1. <span data-ttu-id="f2afc-190">Válassza ki az egy rugalmas készlet toowork.</span><span class="sxs-lookup"><span data-stu-id="f2afc-190">Select an elastic pool toowork with.</span></span>
2. <span data-ttu-id="f2afc-191">A **rugalmas készlet figyelése** feliratú diagram **erőforrás-használat**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-191">Under **Elastic Pool Monitoring** is a chart labeled **Resource utilization**.</span></span> <span data-ttu-id="f2afc-192">Kattintson a hello diagram.</span><span class="sxs-lookup"><span data-stu-id="f2afc-192">Click hello chart.</span></span>

    ![A rugalmas készlet figyelése][3]

    <span data-ttu-id="f2afc-194">Hello **metrika** panel nyílik meg, metrikák hello részletes nézete megjelenítő megadott hello megadott időszak alatt.</span><span class="sxs-lookup"><span data-stu-id="f2afc-194">hello **Metric** blade opens, showing a detailed view of hello specified metrics over hello specified time window.</span></span>   

    ![Metrika panel][9]

### <a name="toocustomize-hello-chart-display"></a><span data-ttu-id="f2afc-196">toocustomize hello diagram megjelenítése</span><span class="sxs-lookup"><span data-stu-id="f2afc-196">toocustomize hello chart display</span></span>

<span data-ttu-id="f2afc-197">Hello diagram és szerkesztheti hello metrika panel toodisplay más mutatókat, például a Processzor százalékos, adat IO százalékos és használt napló IO százalékot.</span><span class="sxs-lookup"><span data-stu-id="f2afc-197">You can edit hello chart and hello metric blade toodisplay other metrics such as CPU percentage, data IO percentage, and log IO percentage used.</span></span>

1. <span data-ttu-id="f2afc-198">Hello metrika paneljén kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-198">On hello metric blade, click **Edit**.</span></span>

    ![Kattintson a Szerkesztés][6]

2. <span data-ttu-id="f2afc-200">A hello **diagram szerkesztése lehetőséget** panelen válasszon ki egy időtartományt (óránként, napjainkban túlra vagy elmúlt hét), vagy kattintson **egyéni** tooselect bármely dátum között hello az elmúlt két hétben.</span><span class="sxs-lookup"><span data-stu-id="f2afc-200">In hello **Edit Chart** blade, select a time range (past hour, today, or past week), or click **custom** tooselect any date range in hello last two weeks.</span></span> <span data-ttu-id="f2afc-201">Válasszon hello diagramtípus (vonal vagy sáv), majd hello erőforrások toomonitor.</span><span class="sxs-lookup"><span data-stu-id="f2afc-201">Select hello chart type (bar or line), then select hello resources toomonitor.</span></span>

   > [!Note]
   > <span data-ttu-id="f2afc-202">Csak a diagram mértékegység azonos hello olvasható hello metrikák: hello azonos idő.</span><span class="sxs-lookup"><span data-stu-id="f2afc-202">Only metrics with hello same unit of measure can be displayed in hello chart at hello same time.</span></span> <span data-ttu-id="f2afc-203">Például "eDTU százaléka" választásakor majd csak választhat más metrikákkal érintő mérték hello egységként.</span><span class="sxs-lookup"><span data-stu-id="f2afc-203">For example, if you select "eDTU percentage" then you can only select other metrics with percentage as hello unit of measure.</span></span>
   >

    ![Kattintson a Szerkesztés](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

3. <span data-ttu-id="f2afc-205">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f2afc-205">Then click **OK**.</span></span>

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a><span data-ttu-id="f2afc-206">Kezelni és megfigyelni a adatbázisok rugalmas készlethez</span><span class="sxs-lookup"><span data-stu-id="f2afc-206">Manage and monitor databases in an elastic pool</span></span>

<span data-ttu-id="f2afc-207">Az egyes adatbázisok is figyelhetők meg potenciális problémák.</span><span class="sxs-lookup"><span data-stu-id="f2afc-207">Individual databases can also be monitored for potential trouble.</span></span>

1. <span data-ttu-id="f2afc-208">A **rugalmas adatbázis-figyelési**, öt adatbázisok metrikáját megjelenítő diagram.</span><span class="sxs-lookup"><span data-stu-id="f2afc-208">Under **Elastic Database Monitoring**, there is a chart that displays metrics for five databases.</span></span> <span data-ttu-id="f2afc-209">Alapértelmezés szerint hello diagram alapján jeleníti meg hello felső 5 adatbázisok hello készletben átlagos edtu-k hello az elmúlt egy órában.</span><span class="sxs-lookup"><span data-stu-id="f2afc-209">By default, hello chart displays hello top 5 databases in hello pool by average eDTU usage in hello past hour.</span></span> <span data-ttu-id="f2afc-210">Kattintson a hello diagram.</span><span class="sxs-lookup"><span data-stu-id="f2afc-210">Click hello chart.</span></span>

    ![A rugalmas készlet figyelése][4]

2. <span data-ttu-id="f2afc-212">Hello **adatbázis erőforrás-használat** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f2afc-212">hello **Database Resource Utilization** blade appears.</span></span> <span data-ttu-id="f2afc-213">Ez az adatbázis-használat hello hello készletben részletes nézetét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f2afc-213">This provides a detailed view of hello database usage in hello pool.</span></span> <span data-ttu-id="f2afc-214">Hello rács hello panel alsó részén hello segítségével, igény szerint adatbázisoknak a hello készlet toodisplay a hello diagramon (felfelé too5 adatbázisok) használatát.</span><span class="sxs-lookup"><span data-stu-id="f2afc-214">Using hello grid in hello lower part of hello blade, you can select any databases in hello pool toodisplay its usage in hello chart (up too5 databases).</span></span> <span data-ttu-id="f2afc-215">Testre szabhatja a metrikák és idő kattintva hello diagramon látható ablak hello **diagram szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-215">You can also customize hello metrics and time window displayed in hello chart by clicking **Edit chart**.</span></span>

    ![Adatbázis erőforráspaneljének kihasználtsága][8]

### <a name="toocustomize-hello-view"></a><span data-ttu-id="f2afc-217">toocustomize hello megtekintése</span><span class="sxs-lookup"><span data-stu-id="f2afc-217">toocustomize hello view</span></span>

1. <span data-ttu-id="f2afc-218">A hello **adatbázis-erőforrás-használat** panelen kattintson a **diagram szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-218">In hello **Database resource utilization** blade, click **Edit chart**.</span></span>

    ![Kattintson a diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. <span data-ttu-id="f2afc-220">A hello **szerkesztése** diagram panelen jelölje be egy időtartományt (óránként túlra vagy elmúlt 24 óra), vagy kattintson a **egyéni** különböző naponta az elmúlt 2 hét toodisplay hello tooselect.</span><span class="sxs-lookup"><span data-stu-id="f2afc-220">In hello **Edit** chart blade, select a time range (past hour or past 24 hours), or click **custom** tooselect a different day in hello past 2 weeks toodisplay.</span></span>

    ![Kattintson az egyéni](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. <span data-ttu-id="f2afc-222">Kattintson a hello **hasonlítsa össze az adatbázisok által** legördülő tooselect egy másik metrika toouse adatbázisok összehasonlításakor.</span><span class="sxs-lookup"><span data-stu-id="f2afc-222">Click hello **Compare databases by** dropdown tooselect a different metric toouse when comparing databases.</span></span>

    ![Hello diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="tooselect-databases-toomonitor"></a><span data-ttu-id="f2afc-224">tooselect adatbázisok toomonitor</span><span class="sxs-lookup"><span data-stu-id="f2afc-224">tooselect databases toomonitor</span></span>

<span data-ttu-id="f2afc-225">Hello adatbázis listáján, hello **adatbázis erőforrás-használat** panelen található adott adatbázisok hello listában hello lapok között, vagy írja be a hello az adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="f2afc-225">In hello database list in hello **Database Resource Utilization** blade, you can find particular databases by looking through hello pages in hello list or by typing in hello name of a database.</span></span> <span data-ttu-id="f2afc-226">Hello jelölőnégyzet tooselect hello adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="f2afc-226">Use hello checkbox tooselect hello database.</span></span>

![Adatbázisok toomonitor keresése][7]


## <a name="add-an-alert-tooan-elastic-pool-resource"></a><span data-ttu-id="f2afc-228">Riasztási tooan rugalmas készlet erőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f2afc-228">Add an alert tooan elastic pool resource</span></span>

<span data-ttu-id="f2afc-229">Szabályok tooan rugalmas készlet, amely küldött e-mailek toopeople vagy riasztás karakterláncok tooURL végpontok hello rugalmas készlet találatok egy Ön által beállított használati küszöbértéket is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="f2afc-229">You can add rules tooan elastic pool that send email toopeople or alert strings tooURL endpoints when hello elastic pool hits a utilization threshold that you set up.</span></span>

<span data-ttu-id="f2afc-230">**egy riasztás tooany erőforrás tooadd:**</span><span class="sxs-lookup"><span data-stu-id="f2afc-230">**tooadd an alert tooany resource:**</span></span>

1. <span data-ttu-id="f2afc-231">Hello kattintson **erőforrás-használat** diagram tooopen hello **metrika** panelen kattintson a **riasztás hozzáadása**, majd adja meg hello hello információkat **értesítések hozzáadása a szabály** panel (**erőforrás** automatikusan toobe hello készlet dolgozunk be van állítva).</span><span class="sxs-lookup"><span data-stu-id="f2afc-231">Click hello **Resource utilization** chart tooopen hello **Metric** blade, click **Add alert**, and then fill out hello information in hello **Add an alert rule** blade (**Resource** is automatically set up toobe hello pool you're working with).</span></span>
2. <span data-ttu-id="f2afc-232">Adjon meg egy **neve** és **leírás** , amely azonosítja a hello riasztási tooyou és hello címzettjeit.</span><span class="sxs-lookup"><span data-stu-id="f2afc-232">Type a **Name** and **Description** that identifies hello alert tooyou and hello recipients.</span></span>
3. <span data-ttu-id="f2afc-233">Válasszon egy **metrika** , amelyet az tooalert hello listából.</span><span class="sxs-lookup"><span data-stu-id="f2afc-233">Choose a **Metric** that you want tooalert from hello list.</span></span>

    <span data-ttu-id="f2afc-234">hello diagram dinamikusan jeleníti meg, hogy metrika toohelp erőforrás-használat úgy dönt, hogy a küszöbérték.</span><span class="sxs-lookup"><span data-stu-id="f2afc-234">hello chart dynamically shows resource utilization for that metric toohelp you choose a threshold.</span></span>

4. <span data-ttu-id="f2afc-235">Válasszon egy **feltétel** (nagyobb, kisebb, mint, stb) és egy **küszöbérték**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-235">Choose a **Condition** (greater than, less than, etc.) and a **Threshold**.</span></span>
5. <span data-ttu-id="f2afc-236">Válasszon egy **időszak** metrika hello idő szabály kell teljesíteni hello riasztási eseményindítók előtt.</span><span class="sxs-lookup"><span data-stu-id="f2afc-236">Choose a **Period** of time that hello metric rule must be satisfied before hello alert triggers.</span></span>
6. <span data-ttu-id="f2afc-237">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f2afc-237">Click **OK**.</span></span>

<span data-ttu-id="f2afc-238">További információkért lásd: [SQL-adatbázis figyelmeztetések létrehozása az Azure-portálon](sql-database-insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f2afc-238">For more information, see [create SQL Database alerts in Azure portal](sql-database-insights-alerts-portal.md).</span></span>

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="f2afc-239">Egy adatbázis áthelyezése rugalmas készletbe</span><span class="sxs-lookup"><span data-stu-id="f2afc-239">Move a database into an elastic pool</span></span>

<span data-ttu-id="f2afc-240">Adja hozzá, vagy távolítsa el az adatbázisokat egy meglévő készletből.</span><span class="sxs-lookup"><span data-stu-id="f2afc-240">You can add or remove databases from an existing pool.</span></span> <span data-ttu-id="f2afc-241">hello adatbázisok más készletek is szerepelhet.</span><span class="sxs-lookup"><span data-stu-id="f2afc-241">hello databases can be in other pools.</span></span> <span data-ttu-id="f2afc-242">Azonban csak akkor adhat hozzá adatbázisok vannak a hello azonos logikai kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f2afc-242">However, you can only add databases that are on hello same logical server.</span></span>

1. <span data-ttu-id="f2afc-243">Hello panelen hello készlet alatt **rugalmas adatbázisok** kattintson **készlet beállítása**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-243">In hello blade for hello pool, under **Elastic databases** click **Configure pool**.</span></span>

    ![Kattintson a készlet konfigurálása][1]

2. <span data-ttu-id="f2afc-245">A hello **készlet beállítása** panelen kattintson a **toopool hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-245">In hello **Configure pool** blade, click **Add toopool**.</span></span>

    ![Kattintson a Hozzáadás toopool](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. <span data-ttu-id="f2afc-247">A hello **adatbázisok hozzáadása** panelen, jelölje be hello adatbázis vagy adatbázisok tooadd toohello készlet.</span><span class="sxs-lookup"><span data-stu-id="f2afc-247">In hello **Add databases** blade, select hello database or databases tooadd toohello pool.</span></span> <span data-ttu-id="f2afc-248">Kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-248">Then click **Select**.</span></span>

    ![Válassza ki az adatbázisok tooadd](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    <span data-ttu-id="f2afc-250">Hello **készlet beállítása** panel most listák hello hozzá, az állapot beállítása túl toobe kijelölt adatbázis**függőben lévő**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-250">hello **Configure pool** blade now lists hello database you selected toobe added, with its status set too**Pending**.</span></span>

    ![Függőben lévő készlet elemek felvétele](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. <span data-ttu-id="f2afc-252">A hello **konfigurálás készlet paneljén**, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-252">In hello **Configure pool blade**, click **Save**.</span></span>

    ![Kattintson a Save (Mentés) gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a><span data-ttu-id="f2afc-254">Egy adatbázis áthelyezése rugalmas készletek kívül</span><span class="sxs-lookup"><span data-stu-id="f2afc-254">Move a database out of an elastic pool</span></span>

1. <span data-ttu-id="f2afc-255">A hello **készlet beállítása** panelen, jelölje be hello adatbázis vagy adatbázisok tooremove.</span><span class="sxs-lookup"><span data-stu-id="f2afc-255">In hello **Configure pool** blade, select hello database or databases tooremove.</span></span>

    ![adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. <span data-ttu-id="f2afc-257">Kattintson a **Eltávolítás a készletből**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-257">Click **Remove from pool**.</span></span>

    ![adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    <span data-ttu-id="f2afc-259">Hello **készlet beállítása** panel most listák hello toobe eltávolítása állapotának beállítása túl, a kijelölt adatbázis**függőben lévő**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-259">hello **Configure pool** blade now lists hello database you selected toobe removed with its status set too**Pending**.</span></span>

    ![előzetes adatbázis hozzáadásának és eltávolításának](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. <span data-ttu-id="f2afc-261">A hello **konfigurálás készlet paneljén**, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="f2afc-261">In hello **Configure pool blade**, click **Save**.</span></span>

    ![Kattintson a Save (Mentés) gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a><span data-ttu-id="f2afc-263">Egy rugalmas készlet teljesítmény beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="f2afc-263">Change performance settings of an elastic pool</span></span>

<span data-ttu-id="f2afc-264">Ahogy figyeli a rugalmas készlet hello erőforrás-használat, azt tapasztalhatja, hogy szükség van-e módosításra.</span><span class="sxs-lookup"><span data-stu-id="f2afc-264">As you monitor hello resource utilization of an elastic pool, you may discover that some adjustments are needed.</span></span> <span data-ttu-id="f2afc-265">Lehet, hogy a hello-készletben hello teljesítményt és a tárolást korlátok változása van szükség.</span><span class="sxs-lookup"><span data-stu-id="f2afc-265">Maybe hello pool needs a change in hello performance or storage limits.</span></span> <span data-ttu-id="f2afc-266">Esetleg érdemes toochange hello adatbázis beállításainak hello készletben.</span><span class="sxs-lookup"><span data-stu-id="f2afc-266">Possibly you want toochange hello database settings in hello pool.</span></span> <span data-ttu-id="f2afc-267">Minden alkalommal tooget hello legjobb egyenlege teljesítményének és költséghatékonyságának hello készlet hello beállítása módosítható.</span><span class="sxs-lookup"><span data-stu-id="f2afc-267">You can change hello setup of hello pool at any time tooget hello best balance of performance and cost.</span></span> <span data-ttu-id="f2afc-268">Lásd: [amikor rugalmas készletek használandó?](sql-database-elastic-pool.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="f2afc-268">See [When should an elastic pool be used?](sql-database-elastic-pool.md) for more information.</span></span>

<span data-ttu-id="f2afc-269">toochange hello edtu-inak vagy tárolási korlátokat címkészletet, és az adatbázisonkénti edtu-k száma:</span><span class="sxs-lookup"><span data-stu-id="f2afc-269">toochange hello eDTUs or storage limits per pool, and eDTUs per database:</span></span>

1. <span data-ttu-id="f2afc-270">Nyissa meg hello **készlet beállítása** panelen.</span><span class="sxs-lookup"><span data-stu-id="f2afc-270">Open hello **Configure pool** blade.</span></span>

    <span data-ttu-id="f2afc-271">A **rugalmas készlet beállítások**, vagy csúszkát toochange hello készlet beállításait használja.</span><span class="sxs-lookup"><span data-stu-id="f2afc-271">Under **elastic pool settings**, use either slider toochange hello pool settings.</span></span>

    ![A rugalmas készlet erőforrás-használat](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. <span data-ttu-id="f2afc-273">Hello beállítás megváltozásakor hello megjelenítési hello becsült hello módosítás havi költségét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f2afc-273">When hello setting changes, hello display shows hello estimated monthly cost of hello change.</span></span>

    ![Egy rugalmas készlet és új havi költségét frissítése folyamatban](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a><span data-ttu-id="f2afc-275">A rugalmas készlet műveletek várakozási ideje</span><span class="sxs-lookup"><span data-stu-id="f2afc-275">Latency of elastic pool operations</span></span>
* <span data-ttu-id="f2afc-276">Hello minimális Edtu / adatbázis vagy a maximális edtu-k adatbázisonkénti módosítása általában befejezi a kevesebb mint 5 perc alatt.</span><span class="sxs-lookup"><span data-stu-id="f2afc-276">Changing hello min eDTUs per database or max eDTUs per database typically completes in 5 minutes or less.</span></span>
* <span data-ttu-id="f2afc-277">Minden adatbázis hello készletben használt terület teljes mennyisége hello függ készletenként hello edtu-k módosítását.</span><span class="sxs-lookup"><span data-stu-id="f2afc-277">Changing hello eDTUs per pool depends on hello total amount of space used by all databases in hello pool.</span></span> <span data-ttu-id="f2afc-278">A módosítások átlagosan 100 gigabájtonként legfeljebb 90 percet vesznek igénybe.</span><span class="sxs-lookup"><span data-stu-id="f2afc-278">Changes average 90 minutes or less per 100 GB.</span></span> <span data-ttu-id="f2afc-279">Például hello teljes terület által használt összes adatbázis hello készletben esetén 200 GB-os, majd hello várt késése hello készlet eDTU-készlet módosítása 3 óra vagy annál kisebb.</span><span class="sxs-lookup"><span data-stu-id="f2afc-279">For example, if hello total space used by all databases in hello pool is 200 GB, then hello expected latency for changing hello pool eDTU per pool is 3 hours or less.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2afc-280">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f2afc-280">Next steps</span></span>

- <span data-ttu-id="f2afc-281">egy rugalmas készlet van, lásd: toounderstand [SQL Database rugalmas készlet](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="f2afc-281">toounderstand what an elastic pool is, see [SQL Database elastic pool](sql-database-elastic-pool.md).</span></span>
- <span data-ttu-id="f2afc-282">A rugalmas készleteket használó útmutatóért lásd: [rugalmas készletek ára és teljesítménye szempontjai](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="f2afc-282">For guidance on using elastic pools, see [Price and performance considerations for elastic pools](sql-database-elastic-pool.md).</span></span>
- <span data-ttu-id="f2afc-283">toouse rugalmas feladatok toorun Transact-SQL-szkriptek használatát tetszőleges számú adatbázishoz hello készletben, lásd: [rugalmas feladatok áttekintése](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f2afc-283">toouse elastic jobs toorun Transact-SQL scripts against any number of databases in hello pool, see [Elastic jobs overview](sql-database-elastic-jobs-overview.md).</span></span>
- <span data-ttu-id="f2afc-284">tooquery keresztül tetszőleges számú adatbázishoz hello készletben, lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f2afc-284">tooquery across any number of databases in hello pool, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
- <span data-ttu-id="f2afc-285">Tetszőleges számú adatbázishoz hello készletben, lásd: tranzakciók [rugalmas tranzakciók](sql-database-elastic-transactions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f2afc-285">For transactions any number of databases in hello pool, see [Elastic transactions](sql-database-elastic-transactions-overview.md).</span></span>


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
