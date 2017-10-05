---
title: "Megoldás migrálása az SQL Data Warehouse |} Microsoft Docs"
description: "Áttelepítési útmutató az Azure SQL Data Warehouse platform, hogy a megoldás."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 198365eb-7451-4222-b99c-d1d9ef687f1b
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/27/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: 771b9456e66b8a1e41f72340b695b19e2adaf793
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-solution-to-azure-sql-data-warehouse"></a><span data-ttu-id="4f1e3-103">Megoldás migrálása az Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4f1e3-103">Migrate your solution to Azure SQL Data Warehouse</span></span>
<span data-ttu-id="4f1e3-104">Tekintse meg a meglévő adatbázis megoldás áttelepítése az Azure SQL Data Warehouse szerepet játszanak.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-104">See what's involved in migrating an existing database solution to Azure SQL Data Warehouse.</span></span> 

## <a name="profile-your-workload"></a><span data-ttu-id="4f1e3-105">Profilhoz, a számítási feladatok</span><span class="sxs-lookup"><span data-stu-id="4f1e3-105">Profile your workload</span></span>
<span data-ttu-id="4f1e3-106">Az áttelepítést, mielőtt kívánt legyen egyes SQL Data Warehouse a munkaterheléshez ideális megoldás.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-106">Before migrating, you want to be certain SQL Data Warehouse is the right solution for your workload.</span></span> <span data-ttu-id="4f1e3-107">Az SQL Data Warehouse egy olyan elosztott rendszer készült elemzés végrehajtásához, nagy.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-107">SQL Data Warehouse is a distributed system designed to perform analytics on large data.</span></span>  <span data-ttu-id="4f1e3-108">Az SQL Data Warehouse áttelepítése igényel az egyes módosításokat nem túl merevlemez annak megértése, de eltarthat egy ideig megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-108">Migrating to SQL Data Warehouse requires some design changes that are not too hard to understand but might take some time to implement.</span></span> <span data-ttu-id="4f1e3-109">Ha a vállalat által igényelt egy vállalati szintű data warehouse-ba, a következő előnyöket is megéri.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-109">If your business requires an enterprise-class data warehouse, the benefits are worth the effort.</span></span> <span data-ttu-id="4f1e3-110">Azonban az SQL Data Warehouse power nincs szüksége, esetén költséghatékonyabb SQL Server vagy az Azure SQL Database használatához.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-110">However, if you don't need the power of SQL Data Warehouse, it is more cost-effective to use SQL Server or Azure SQL Database.</span></span>

<span data-ttu-id="4f1e3-111">Fontolja meg az SQL Data Warehouse amikor Ön:</span><span class="sxs-lookup"><span data-stu-id="4f1e3-111">Consider using SQL Data Warehouse when you:</span></span>
- <span data-ttu-id="4f1e3-112">Rendelkezik egy vagy több terabájtos adatkészleteket</span><span class="sxs-lookup"><span data-stu-id="4f1e3-112">Have one or more Terabytes of data</span></span>
- <span data-ttu-id="4f1e3-113">Szeretné futtatni az analytics a nagy mennyiségű adat</span><span class="sxs-lookup"><span data-stu-id="4f1e3-113">Plan to run analytics on large amounts of data</span></span>
- <span data-ttu-id="4f1e3-114">Képes számítási és tárolási méretek kell</span><span class="sxs-lookup"><span data-stu-id="4f1e3-114">Need the ability to scale compute and storage</span></span> 
- <span data-ttu-id="4f1e3-115">Szeretné költségeket takaríthat felfüggesztése számítási erőforrásokat, amikor már nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-115">Want to save costs by pausing compute resources when you don't need them.</span></span>

<span data-ttu-id="4f1e3-116">Ne használjon az SQL Data Warehouse működési (OLTP) munkaterhelések esetén, amelyek rendelkeznek:</span><span class="sxs-lookup"><span data-stu-id="4f1e3-116">Don't use SQL Data Warehouse for operational (OLTP) workloads that have:</span></span>
- <span data-ttu-id="4f1e3-117">Nagy gyakoriságú olvasása és írása</span><span class="sxs-lookup"><span data-stu-id="4f1e3-117">High frequency reads and writes</span></span>
- <span data-ttu-id="4f1e3-118">Egypéldányos nagy számú kiválasztása</span><span class="sxs-lookup"><span data-stu-id="4f1e3-118">Large numbers of singleton selects</span></span>
- <span data-ttu-id="4f1e3-119">Egyetlen sor beszúrása jelentős mennyiségű</span><span class="sxs-lookup"><span data-stu-id="4f1e3-119">High volumes of single row inserts</span></span>
- <span data-ttu-id="4f1e3-120">A soronkénti igényeinek feldolgozása</span><span class="sxs-lookup"><span data-stu-id="4f1e3-120">Row by row processing needs</span></span>
- <span data-ttu-id="4f1e3-121">Inkompatibilis formátumban (JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="4f1e3-121">Incompatible formats (JSON, XML)</span></span>


## <a name="plan-the-migration"></a><span data-ttu-id="4f1e3-122">Az áttelepítés tervezése</span><span class="sxs-lookup"><span data-stu-id="4f1e3-122">Plan the migration</span></span>

<span data-ttu-id="4f1e3-123">Miután eldöntötte, hogy egy meglévő megoldás áttelepítéséhez az SQL Data Warehouse, fontos tervezze meg az áttelepítés elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-123">Once you have decided to migrate an existing solution to SQL Data Warehouse, it is important to plan the migration before getting started.</span></span> 

<span data-ttu-id="4f1e3-124">Egy tervezési célja az adatok, a táblasémákat és a kód kompatibilisek-e az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-124">One goal of planning is to ensure your data, your table schemas, and your code are compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="4f1e3-125">Bizonyos kompatibilitási különbségek vannak a jelenlegi rendszer és az SQL Data Warehouse közötti megoldása.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-125">There are some compatibility differences to work around between your current system and SQL Data Warehouse.</span></span> <span data-ttu-id="4f1e3-126">Áttelepíti, valamint időt Azure vesz nagy mennyiségű adat.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-126">Plus, migrating large amounts of data to Azure takes time.</span></span> <span data-ttu-id="4f1e3-127">Gondosan meg kell tervezni gyorsítja olvashatja be adatait az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-127">Careful planning expedites getting your data to Azure.</span></span> 

<span data-ttu-id="4f1e3-128">Egy másik tervezési célja annak érdekében, hogy a megoldás kihasználja a célja, hogy az SQL Data Warehouse biztosításához a lekérdezési teljesítmény tervezési módosításokra.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-128">Another goal of planning is to make design adjustments to ensure your solution takes advantage of the high query performance SQL Data Warehouse is designed to provide.</span></span> <span data-ttu-id="4f1e3-129">Az adatraktárak bővített tervezése vezet be a különböző kialakítási minták és így hagyományos módszerekkel nem mindig a legjobb.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-129">Designing data warehouses for scale introduces different design patterns and so traditional approaches aren't always the best.</span></span> <span data-ttu-id="4f1e3-130">Bár bizonyos tervezési módosításokra áttelepítés után, a folyamat hamarabb módosítása menti a későbbi.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-130">Although you can make some design adjustments after migration, making changes sooner in the process will save time later.</span></span>

<span data-ttu-id="4f1e3-131">A sikeres áttelepítéshez szüksége a táblasémákat, a kódot és az adatok áttelepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-131">To perform a successful migration, you need to migrate your table schemas, your code, and your data.</span></span> <span data-ttu-id="4f1e3-132">Az alábbi áttelepítési témakörök útmutatást lásd:</span><span class="sxs-lookup"><span data-stu-id="4f1e3-132">For guidance on these migration topics, see:</span></span>

-  [<span data-ttu-id="4f1e3-133">Telepítse át a sémák</span><span class="sxs-lookup"><span data-stu-id="4f1e3-133">Migrate your schemas</span></span>](sql-data-warehouse-migrate-schema.md)
-  [<span data-ttu-id="4f1e3-134">Telepítse át a kódot</span><span class="sxs-lookup"><span data-stu-id="4f1e3-134">Migrate your code</span></span>](sql-data-warehouse-migrate-code.md)
-  <span data-ttu-id="4f1e3-135">[Adatok áttelepítése](sql-data-warehouse-migrate-data.md).</span><span class="sxs-lookup"><span data-stu-id="4f1e3-135">[Migrate your data](sql-data-warehouse-migrate-data.md).</span></span> 

<!--
## Perform the migration


## Deploy the solution


## Validate the migration

-->

## <a name="next-steps"></a><span data-ttu-id="4f1e3-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f1e3-136">Next steps</span></span>
<span data-ttu-id="4f1e3-137">A CAT (Ügyféltanácsadói csapatának) is rendelkezik néhány nagy az SQL Data Warehouse útmutatást, amelyek ezek teszik közzé a blogok keresztül.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-137">The CAT (Customer Advisory Team) also has some great SQL Data Warehouse guidance, which they publish through blogs.</span></span>  <span data-ttu-id="4f1e3-138">Tekintse meg a cikk [adatok áttelepítése az Azure SQL Data Warehouse a gyakorlatban] [ Migrating data to Azure SQL Data Warehouse in practice] további útmutatást nyújt az áttelepítési.</span><span class="sxs-lookup"><span data-stu-id="4f1e3-138">Take a look at their article, [Migrating data to Azure SQL Data Warehouse in practice][Migrating data to Azure SQL Data Warehouse in practice] for additional guidance on migration.</span></span>

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data to Azure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
