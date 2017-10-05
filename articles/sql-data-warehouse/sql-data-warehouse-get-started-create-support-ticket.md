---
title: "Támogatási jegy létrehozása az SQL Data Warehouse-hoz| Microsoft Docs"
description: "Támogatási jegy létrehozása az SQL Data Warehouse-hoz."
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
ms.openlocfilehash: 058ff1229acee5d03db7c0305c5565ae95a85758
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a><span data-ttu-id="94194-103">Támogatási jegy létrehozása az SQL Data Warehouse-hoz</span><span class="sxs-lookup"><span data-stu-id="94194-103">How to create a support ticket for SQL Data Warehouse</span></span>
<span data-ttu-id="94194-104">Ha bármilyen probléma merül fel az SQL Data Warehouses-szal kapcsolatban, hozzon létre egy támogatási jegyet, hogy mérnöki csapatunk a segítségére lehessen.</span><span class="sxs-lookup"><span data-stu-id="94194-104">If you are having any issues with your SQL Data Warehouse, please create a support ticket so that our engineering team can assist you.</span></span>

> [!NOTE] 
> <span data-ttu-id="94194-105">2016. december 20-tól az Azure Portal erőforrásállapot-ellenőrzése nem biztosít pontos eredményeket.</span><span class="sxs-lookup"><span data-stu-id="94194-105">As of 12/20/2016, the resource health check in the Azure portal is not accurate.</span></span> <span data-ttu-id="94194-106">Folyamatosan dolgozunk a probléma megoldásán.</span><span class="sxs-lookup"><span data-stu-id="94194-106">We are actively working to fix this issue.</span></span> 


## <a name="create-a-support-ticket"></a><span data-ttu-id="94194-107">Támogatási jegy létrehozása</span><span class="sxs-lookup"><span data-stu-id="94194-107">Create a support ticket</span></span>
1. <span data-ttu-id="94194-108">Nyissa meg az [Azure Portalt][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="94194-108">Open the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="94194-109">A kezdőképernyőn kattintson a **Súgó és támogatás** csempére.</span><span class="sxs-lookup"><span data-stu-id="94194-109">On the Home screen, click the **Help + support** tile.</span></span>
   
    ![Súgó és támogatás](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. <span data-ttu-id="94194-111">A Súgó és támogatás panelen kattintson a **Támogatási kérelem létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="94194-111">On the Help + Support blade, click **Create support request**.</span></span>
   
    ![Új támogatási kérelem](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. <span data-ttu-id="94194-113">Válassza ki a **Kérelemtípust**.</span><span class="sxs-lookup"><span data-stu-id="94194-113">Select the **Request Type**.</span></span>
   
    ![Kérelemtípus](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > <span data-ttu-id="94194-115">Alapértelmezés szerint minden SQL-kiszolgáló (például a myserver.database.windows.net) rendelkezik egy 45 000 egységnyi **DTU-kvótával**.</span><span class="sxs-lookup"><span data-stu-id="94194-115">By default, each SQL server (e.g. myserver.database.windows.net) has a **DTU Quota** of 45,000.</span></span> <span data-ttu-id="94194-116">Ez a kvóta egyszerűen egy biztonsági korlát.</span><span class="sxs-lookup"><span data-stu-id="94194-116">This quota is simply a safety limit.</span></span> <span data-ttu-id="94194-117">A kvótát egy támogatási jegy létrehozásával, és a kérelem típusaként a *Kvóta* kiválasztásával növelheti meg.</span><span class="sxs-lookup"><span data-stu-id="94194-117">You can increase your quota by creating a support ticket and selecting *Quota* as the request type.</span></span> <span data-ttu-id="94194-118">A DTU-igény kiszámításához szorozza meg 7,5-tel az összes szükséges [DWU][DWU] értékét.</span><span class="sxs-lookup"><span data-stu-id="94194-118">To calculate your DTU needs, multiply the 7.5 by the total [DWU][DWU] needed.</span></span> <span data-ttu-id="94194-119">Ha például két DW6000-et szeretne üzemeltetni egy SQL-kiszolgálón, akkor 90 000 egységnyi DTU-kvótát kell igényelnie.</span><span class="sxs-lookup"><span data-stu-id="94194-119">For example, you would like to host two DW6000s on one SQL server, then you should request a DTU quota of 90,000.</span></span>  <span data-ttu-id="94194-120">Az aktuális DTU-felhasználást az SQL-kiszolgáló panelen tekintheti meg a portálon.</span><span class="sxs-lookup"><span data-stu-id="94194-120">You can view your current DTU consumption from the SQL server blade in the portal.</span></span> <span data-ttu-id="94194-121">A DTU-kvótába a szüneteltetett és a nem szüneteltetett adatbázisok is beleszámítanak.</span><span class="sxs-lookup"><span data-stu-id="94194-121">Both paused and un-paused databases count toward the DTU quota.</span></span> 
   > 
   > 
5. <span data-ttu-id="94194-122">Válassza ki az **Előfizetést**, amely alatt az az adatbázis fut, amellyel kapcsolatban támogatást kér.</span><span class="sxs-lookup"><span data-stu-id="94194-122">Select the **Subscription** that hosts the database with the problem you are reporting.</span></span>
   
    ![Előfizetést](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. <span data-ttu-id="94194-124">Erőforrásként adja meg a **SQL Data Warehouse** szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="94194-124">Select **SQL Data Warehouse** as the Resource.</span></span>
   
    ![Erőforrás](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. <span data-ttu-id="94194-126">Jelölje ki az [Azure támogatási csomagot][Azure support plan].</span><span class="sxs-lookup"><span data-stu-id="94194-126">Select your [Azure support plan][Azure support plan].</span></span>
   
   * <span data-ttu-id="94194-127">A **számlázással, kvótával és az előfizetés kezelésével** kapcsolatos támogatás minden támogatási szinten elérhető.</span><span class="sxs-lookup"><span data-stu-id="94194-127">**Billing, quota and subscription management** support is available at all support levels.</span></span>
   * <span data-ttu-id="94194-128">A **javítás/csere** támogatás a [Fejlesztői][Developer], a [Standard][Standard], a [Közvetlen professzionális támogatás][Professional Direct] vagy a [Premier szintű támogatás][Premier] esetén érhető el.</span><span class="sxs-lookup"><span data-stu-id="94194-128">**Break-fix** support is provided through [Developer][Developer], [Standard][Standard], [Professional Direct][Professional Direct] or [Premier][Premier] support.</span></span> <span data-ttu-id="94194-129">A javítás/csere típusú problémákkal az Azure használata során fellépő hibák esetén lehet a támogatáshoz fordulni, ha a hibát nagy valószínűséggel a Microsoft terméke okozta.</span><span class="sxs-lookup"><span data-stu-id="94194-129">Break-fix issues are problems experienced by customers while using Azure where there is a reasonable expectation that Microsoft caused the problem.</span></span>
   * <span data-ttu-id="94194-130">A **fejlesztői mentorálás** és a **tanácsadási szolgáltatás** [Közvetlen professzionális támogatás][Professional Direct] és [Premier szintű támogatás][Premier] esetén érhető el.</span><span class="sxs-lookup"><span data-stu-id="94194-130">**Developer mentoring** and **advisory services** are available at the [Professional Direct][Professional Direct] and [Premier][Premier] support levels.</span></span> 
     
     <span data-ttu-id="94194-131">Premier szintű támogatás megléte esetén az SQL Data Warehouse-szal kapcsolatos problémákat is jelentheti a [Microsoft Premier online portálon][Microsoft Premier online portal].</span><span class="sxs-lookup"><span data-stu-id="94194-131">If you have a Premier support plan, you can also report SQL Data Warehouse related issues on the [Microsoft Premier online portal][Microsoft Premier online portal].</span></span>  <span data-ttu-id="94194-132">Az [Azure-támogatás ügyfeleknek][Azure support plan] című témakör részletesen bemutatja a támogatási csomagokat, beleértve azok hatókörét, a válaszidőt, a díjszabást és egyéb információkat.  Az Azure-támogatással kapcsolatos gyakori kérdéseket az [Azure-támogatás – gyakori kérdések][Azure support FAQs] című témakör tekinti át.</span><span class="sxs-lookup"><span data-stu-id="94194-132">See [Azure support plans][Azure support plan] to learn more about the various support plans, including scope, response times, pricing, etc.  For frequently asked questions about Azure support, see [Azure support FAQs][Azure support FAQs].</span></span>  
     
     ![Támogatási csomag](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. <span data-ttu-id="94194-134">Válassza ki a **Problem Type** (Probléma típusa) és a **Category** (Kategória) mezőt.</span><span class="sxs-lookup"><span data-stu-id="94194-134">Select the **Problem Type** and **Category**.</span></span> <span data-ttu-id="94194-135">Ebben a példában a választott problématípus az „Eszközök”, a kategória pedig az „Ügyféleszközök”.</span><span class="sxs-lookup"><span data-stu-id="94194-135">In this example, we have chosen "Tools" as the Problem type and "Client tools" as the category.</span></span> 
   
    ![Probléma típusa és kategóriája](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. <span data-ttu-id="94194-137">Írja le a problémát, és hogy az milyen mértékben hat az üzleti működésre.</span><span class="sxs-lookup"><span data-stu-id="94194-137">Describe the problem and choose the level of business impact.</span></span>
   
    ![A probléma leírása](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. <span data-ttu-id="94194-139">A támogatási jegyen előre ki lesznek töltve a **kapcsolattartási adatok**.</span><span class="sxs-lookup"><span data-stu-id="94194-139">Your **contact information** for this support ticket will be pre-filled.</span></span> <span data-ttu-id="94194-140">Ha szükséges, pontosítsa azokat.</span><span class="sxs-lookup"><span data-stu-id="94194-140">Update this if necessary.</span></span>
    
    ![Kapcsolattartási adatok](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. <span data-ttu-id="94194-142">Kattintson a**Create** (Létrehozás) gombra a támogatási kérelem elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="94194-142">Click **Create** to submit the support request.</span></span>

## <a name="monitor-a-support-ticket"></a><span data-ttu-id="94194-143">Támogatási jegy nyomon követése</span><span class="sxs-lookup"><span data-stu-id="94194-143">Monitor a support ticket</span></span>
<span data-ttu-id="94194-144">A támogatási kérelem elküldése után az Azure támogatási csapata kapcsolatba lép Önnel.</span><span class="sxs-lookup"><span data-stu-id="94194-144">After you have submitted the support request, the Azure support team will contact you.</span></span> <span data-ttu-id="94194-145">A kérelem állapotának és részleteinek megtekintéséhez kattintson a **Támogatási kérelmek kezelése** elemre az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="94194-145">To check your request status and details, click **Manage support requests** on the dashboard.</span></span>

![Állapot ellenőrzése](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a><span data-ttu-id="94194-147">Egyéb források</span><span class="sxs-lookup"><span data-stu-id="94194-147">Other Resources</span></span>
<span data-ttu-id="94194-148">Azt is megteheti, hogy kapcsolatba lép az SQL Data Warehouse-közösséggel a [Stack Overflow][Stack Overflow] webhelyen vagy az [Azure SQL Data Warehouse MSDN fórumon][Azure SQL Data Warehouse MSDN forum].</span><span class="sxs-lookup"><span data-stu-id="94194-148">Additionally, you can connect with the SQL Data Warehouse community on [Stack Overflow][Stack Overflow] or on the [Azure SQL Data Warehouse MSDN forum][Azure SQL Data Warehouse MSDN forum].</span></span>

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

