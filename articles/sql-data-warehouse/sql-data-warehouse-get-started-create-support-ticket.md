---
title: "az SQL Data Warehouse egy támogatási jegy aaaHow toocreate |} Microsoft Docs"
description: "Hogyan toocreate támogatási jegyet az Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: a91d1f53-03cb-464b-9d5b-4a9c1a194ed3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 72f7eac82112fb7f1bfb05abca4ce40aeb3c828c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-support-ticket-for-sql-data-warehouse"></a><span data-ttu-id="faafb-103">Hogyan toocreate támogatási jegyet az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="faafb-103">How toocreate a support ticket for SQL Data Warehouse</span></span>
<span data-ttu-id="faafb-104">Ha bármilyen probléma merül fel az SQL Data Warehouses-szal kapcsolatban, hozzon létre egy támogatási jegyet, hogy mérnöki csapatunk a segítségére lehessen.</span><span class="sxs-lookup"><span data-stu-id="faafb-104">If you are having any issues with your SQL Data Warehouse, please create a support ticket so that our engineering team can assist you.</span></span>

> [!NOTE] 
> <span data-ttu-id="faafb-105">Azure-portálon hello hello erőforrás állapot-ellenőrzéssel nincs pontos 12/20/2016.</span><span class="sxs-lookup"><span data-stu-id="faafb-105">As of 12/20/2016, hello resource health check in hello Azure portal is not accurate.</span></span> <span data-ttu-id="faafb-106">Folyamatosan dolgozunk a toofix probléma.</span><span class="sxs-lookup"><span data-stu-id="faafb-106">We are actively working toofix this issue.</span></span> 


## <a name="create-a-support-ticket"></a><span data-ttu-id="faafb-107">Támogatási jegy létrehozása</span><span class="sxs-lookup"><span data-stu-id="faafb-107">Create a support ticket</span></span>
1. <span data-ttu-id="faafb-108">Nyissa meg hello [Azure-portálon][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="faafb-108">Open hello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="faafb-109">Hello kezdőképernyőn kattintson hello **súgó + támogatás** csempére.</span><span class="sxs-lookup"><span data-stu-id="faafb-109">On hello Home screen, click hello **Help + support** tile.</span></span>
   
    ![Súgó és támogatás](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. <span data-ttu-id="faafb-111">A hello Súgó és támogatás panelen, kattintson **támogatási kérelem létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="faafb-111">On hello Help + Support blade, click **Create support request**.</span></span>
   
    ![Új támogatási kérelem](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. <span data-ttu-id="faafb-113">Jelölje be hello **kérelem típus**.</span><span class="sxs-lookup"><span data-stu-id="faafb-113">Select hello **Request Type**.</span></span>
   
    ![Kérelemtípus](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > <span data-ttu-id="faafb-115">Alapértelmezés szerint minden SQL-kiszolgáló (például a myserver.database.windows.net) rendelkezik egy 45 000 egységnyi **DTU-kvótával**.</span><span class="sxs-lookup"><span data-stu-id="faafb-115">By default, each SQL server (e.g. myserver.database.windows.net) has a **DTU Quota** of 45,000.</span></span> <span data-ttu-id="faafb-116">Ez a kvóta egyszerűen egy biztonsági korlát.</span><span class="sxs-lookup"><span data-stu-id="faafb-116">This quota is simply a safety limit.</span></span> <span data-ttu-id="faafb-117">A támogatási jegy létrehozása, majd válassza a kvóta növelhető *kvóta* hello kérelem típusa.</span><span class="sxs-lookup"><span data-stu-id="faafb-117">You can increase your quota by creating a support ticket and selecting *Quota* as hello request type.</span></span> <span data-ttu-id="faafb-118">a DTU, meg kell szorozni toocalculate 7.5 hello teljes hello által [DWU] [ DWU] szükséges.</span><span class="sxs-lookup"><span data-stu-id="faafb-118">toocalculate your DTU needs, multiply hello 7.5 by hello total [DWU][DWU] needed.</span></span> <span data-ttu-id="faafb-119">Például azt szeretné, a két toohost egyetlen SQL Servert, majd DW6000s igényeljen egy 90,000 a DTU-kvótát.</span><span class="sxs-lookup"><span data-stu-id="faafb-119">For example, you would like toohost two DW6000s on one SQL server, then you should request a DTU quota of 90,000.</span></span>  <span data-ttu-id="faafb-120">Az aktuális DTU-használat hello SQL server panelen hello portálon tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="faafb-120">You can view your current DTU consumption from hello SQL server blade in hello portal.</span></span> <span data-ttu-id="faafb-121">Felfüggesztett, mind a nem felfüggesztett adatbázisok száma felé hello DTU-kvótát.</span><span class="sxs-lookup"><span data-stu-id="faafb-121">Both paused and un-paused databases count toward hello DTU quota.</span></span> 
   > 
   > 
5. <span data-ttu-id="faafb-122">Jelölje be hello **előfizetés** , hogy a gazdagépek hello hello hiba az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="faafb-122">Select hello **Subscription** that hosts hello database with hello problem you are reporting.</span></span>
   
    ![Előfizetés](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. <span data-ttu-id="faafb-124">Válassza ki **SQL Data Warehouse** , hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="faafb-124">Select **SQL Data Warehouse** as hello Resource.</span></span>
   
    ![Erőforrás](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. <span data-ttu-id="faafb-126">Jelölje ki az [Azure támogatási csomagot][Azure support plan].</span><span class="sxs-lookup"><span data-stu-id="faafb-126">Select your [Azure support plan][Azure support plan].</span></span>
   
   * <span data-ttu-id="faafb-127">A **számlázással, kvótával és az előfizetés kezelésével** kapcsolatos támogatás minden támogatási szinten elérhető.</span><span class="sxs-lookup"><span data-stu-id="faafb-127">**Billing, quota and subscription management** support is available at all support levels.</span></span>
   * <span data-ttu-id="faafb-128">A **javítás/csere** támogatás a [Fejlesztői][Developer], a [Standard][Standard], a [Közvetlen professzionális támogatás][Professional Direct] vagy a [Premier szintű támogatás][Premier] esetén érhető el.</span><span class="sxs-lookup"><span data-stu-id="faafb-128">**Break-fix** support is provided through [Developer][Developer], [Standard][Standard], [Professional Direct][Professional Direct] or [Premier][Premier] support.</span></span> <span data-ttu-id="faafb-129">Javítás / Csere problémák az Azure használata során az ügyfelek által tapasztalt problémák esetén egy ésszerű elvárásokat adott okozott Microsoft hello problémát.</span><span class="sxs-lookup"><span data-stu-id="faafb-129">Break-fix issues are problems experienced by customers while using Azure where there is a reasonable expectation that Microsoft caused hello problem.</span></span>
   * <span data-ttu-id="faafb-130">**Fejlesztői mentorálás** és **tanácsadási szolgáltatás** érhetők el hello [közvetlen professzionális] [ Professional Direct] és [Premier] [ Premier] szintek.</span><span class="sxs-lookup"><span data-stu-id="faafb-130">**Developer mentoring** and **advisory services** are available at hello [Professional Direct][Professional Direct] and [Premier][Premier] support levels.</span></span> 
     
     <span data-ttu-id="faafb-131">Ha a Premier szintű támogatás megléte, az SQL Data Warehouse is jelentheti kapcsolatos hiba lépett fel a hello [Microsoft Premier online portálon][Microsoft Premier online portal].</span><span class="sxs-lookup"><span data-stu-id="faafb-131">If you have a Premier support plan, you can also report SQL Data Warehouse related issues on hello [Microsoft Premier online portal][Microsoft Premier online portal].</span></span>  <span data-ttu-id="faafb-132">Lásd: [Azure-támogatás ügyfeleknek] [ Azure support plan] további információk a különböző támogatja a csomagokat, beleértve azok hatókörét, a válaszidőt, díjszabást hello toolearn stb.  Az Azure-támogatással kapcsolatos gyakori kérdéseket az [Azure-támogatás – gyakori kérdések][Azure support FAQs] című témakör tekinti át.</span><span class="sxs-lookup"><span data-stu-id="faafb-132">See [Azure support plans][Azure support plan] toolearn more about hello various support plans, including scope, response times, pricing, etc.  For frequently asked questions about Azure support, see [Azure support FAQs][Azure support FAQs].</span></span>  
     
     ![Támogatási csomag](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. <span data-ttu-id="faafb-134">Jelölje be hello **problématípust** és **kategória**.</span><span class="sxs-lookup"><span data-stu-id="faafb-134">Select hello **Problem Type** and **Category**.</span></span> <span data-ttu-id="faafb-135">Ebben a példában azt választotta, "Eszközök" hello probléma típusa és "Client tools" hello kategória szerint.</span><span class="sxs-lookup"><span data-stu-id="faafb-135">In this example, we have chosen "Tools" as hello Problem type and "Client tools" as hello category.</span></span> 
   
    ![Probléma típusa és kategóriája](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. <span data-ttu-id="faafb-137">Hello probléma leíró, és válassza ki azt az üzletmenetre gyakorolt hatás hello szintet.</span><span class="sxs-lookup"><span data-stu-id="faafb-137">Describe hello problem and choose hello level of business impact.</span></span>
   
    ![A probléma leírása](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. <span data-ttu-id="faafb-139">A támogatási jegyen előre ki lesznek töltve a **kapcsolattartási adatok**.</span><span class="sxs-lookup"><span data-stu-id="faafb-139">Your **contact information** for this support ticket will be pre-filled.</span></span> <span data-ttu-id="faafb-140">Ha szükséges, pontosítsa azokat.</span><span class="sxs-lookup"><span data-stu-id="faafb-140">Update this if necessary.</span></span>
    
    ![Kapcsolattartási adatok](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. <span data-ttu-id="faafb-142">Kattintson a **létrehozása** toosubmit hello támogatási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="faafb-142">Click **Create** toosubmit hello support request.</span></span>

## <a name="monitor-a-support-ticket"></a><span data-ttu-id="faafb-143">Támogatási jegy nyomon követése</span><span class="sxs-lookup"><span data-stu-id="faafb-143">Monitor a support ticket</span></span>
<span data-ttu-id="faafb-144">Miután hello támogatási kérelmet elküldte, hello Azure támogatási csapata kapcsolatba lép Önnel.</span><span class="sxs-lookup"><span data-stu-id="faafb-144">After you have submitted hello support request, hello Azure support team will contact you.</span></span> <span data-ttu-id="faafb-145">toocheck a kérelem állapotának és részleteinek, kattintson a **támogatási kérelmek kezelése** hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="faafb-145">toocheck your request status and details, click **Manage support requests** on hello dashboard.</span></span>

![Állapot ellenőrzése](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a><span data-ttu-id="faafb-147">Egyéb források</span><span class="sxs-lookup"><span data-stu-id="faafb-147">Other Resources</span></span>
<span data-ttu-id="faafb-148">Ezenkívül kapcsolatba léphet az SQL Data Warehouse-Közösséggel hello a [Stack Overflow] [ Stack Overflow] vagy hello [Azure SQL Data Warehouse MSDN fórumon] [ Azure SQL Data Warehouse MSDN forum].</span><span class="sxs-lookup"><span data-stu-id="faafb-148">Additionally, you can connect with hello SQL Data Warehouse community on [Stack Overflow][Stack Overflow] or on hello [Azure SQL Data Warehouse MSDN forum][Azure SQL Data Warehouse MSDN forum].</span></span>

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Azure support plan]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure support FAQs]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online portal]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

