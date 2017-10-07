---
title: "aaaMigrate a megoldás tooSQL adatraktár |} Microsoft Docs"
description: "Áttelepítési útmutató annak érdekében, hogy a megoldás tooAzure SQL Data Warehouse platform."
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
ms.openlocfilehash: 27b51f15247603f054070f360ede7f24541c0288
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-solution-tooazure-sql-data-warehouse"></a><span data-ttu-id="8b18f-103">A megoldás tooAzure SQL Data Warehouse áttelepítése</span><span class="sxs-lookup"><span data-stu-id="8b18f-103">Migrate your solution tooAzure SQL Data Warehouse</span></span>
<span data-ttu-id="8b18f-104">Tekintse meg, mi részt vesz egy meglévő adatbázis megoldás tooAzure SQL Data Warehouse áttelepítése.</span><span class="sxs-lookup"><span data-stu-id="8b18f-104">See what's involved in migrating an existing database solution tooAzure SQL Data Warehouse.</span></span> 

## <a name="profile-your-workload"></a><span data-ttu-id="8b18f-105">Profilhoz, a számítási feladatok</span><span class="sxs-lookup"><span data-stu-id="8b18f-105">Profile your workload</span></span>
<span data-ttu-id="8b18f-106">Az áttelepítést, mielőtt azt szeretné, toobe arról, hogy az SQL Data Warehouse hello ideális megoldás a terhelést.</span><span class="sxs-lookup"><span data-stu-id="8b18f-106">Before migrating, you want toobe certain SQL Data Warehouse is hello right solution for your workload.</span></span> <span data-ttu-id="8b18f-107">Az SQL Data Warehouse egy elosztott rendszert tooperform analytics nagy.</span><span class="sxs-lookup"><span data-stu-id="8b18f-107">SQL Data Warehouse is a distributed system designed tooperform analytics on large data.</span></span>  <span data-ttu-id="8b18f-108">Áttelepítése tooSQL adatraktár néhány tervezési módosításaival, amelyek nem túl szigorú toounderstand, de eltarthat néhány alkalommal tooimplement igényel.</span><span class="sxs-lookup"><span data-stu-id="8b18f-108">Migrating tooSQL Data Warehouse requires some design changes that are not too hard toounderstand but might take some time tooimplement.</span></span> <span data-ttu-id="8b18f-109">Ha a vállalat által igényelt egy vállalati szintű data warehouse-ba, hello előnyöket hello tevékenységi-érdemes vannak.</span><span class="sxs-lookup"><span data-stu-id="8b18f-109">If your business requires an enterprise-class data warehouse, hello benefits are worth hello effort.</span></span> <span data-ttu-id="8b18f-110">Azonban az SQL Data Warehouse hello power nincs szüksége, esetén további költséghatékony toouse SQL Server vagy az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="8b18f-110">However, if you don't need hello power of SQL Data Warehouse, it is more cost-effective toouse SQL Server or Azure SQL Database.</span></span>

<span data-ttu-id="8b18f-111">Fontolja meg az SQL Data Warehouse amikor Ön:</span><span class="sxs-lookup"><span data-stu-id="8b18f-111">Consider using SQL Data Warehouse when you:</span></span>
- <span data-ttu-id="8b18f-112">Rendelkezik egy vagy több terabájtos adatkészleteket</span><span class="sxs-lookup"><span data-stu-id="8b18f-112">Have one or more Terabytes of data</span></span>
- <span data-ttu-id="8b18f-113">Terv toorun analytics a nagy mennyiségű adat</span><span class="sxs-lookup"><span data-stu-id="8b18f-113">Plan toorun analytics on large amounts of data</span></span>
- <span data-ttu-id="8b18f-114">Hello képességét tooscale számítási és tárolási kell</span><span class="sxs-lookup"><span data-stu-id="8b18f-114">Need hello ability tooscale compute and storage</span></span> 
- <span data-ttu-id="8b18f-115">Szeretné, hogy toosave költségek felfüggesztésével számítási erőforrásokat, amikor már nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="8b18f-115">Want toosave costs by pausing compute resources when you don't need them.</span></span>

<span data-ttu-id="8b18f-116">Ne használjon az SQL Data Warehouse működési (OLTP) munkaterhelések esetén, amelyek rendelkeznek:</span><span class="sxs-lookup"><span data-stu-id="8b18f-116">Don't use SQL Data Warehouse for operational (OLTP) workloads that have:</span></span>
- <span data-ttu-id="8b18f-117">Nagy gyakoriságú olvasása és írása</span><span class="sxs-lookup"><span data-stu-id="8b18f-117">High frequency reads and writes</span></span>
- <span data-ttu-id="8b18f-118">Egypéldányos nagy számú kiválasztása</span><span class="sxs-lookup"><span data-stu-id="8b18f-118">Large numbers of singleton selects</span></span>
- <span data-ttu-id="8b18f-119">Egyetlen sor beszúrása jelentős mennyiségű</span><span class="sxs-lookup"><span data-stu-id="8b18f-119">High volumes of single row inserts</span></span>
- <span data-ttu-id="8b18f-120">A soronkénti igényeinek feldolgozása</span><span class="sxs-lookup"><span data-stu-id="8b18f-120">Row by row processing needs</span></span>
- <span data-ttu-id="8b18f-121">Inkompatibilis formátumban (JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="8b18f-121">Incompatible formats (JSON, XML)</span></span>


## <a name="plan-hello-migration"></a><span data-ttu-id="8b18f-122">Hello áttelepítés tervezése</span><span class="sxs-lookup"><span data-stu-id="8b18f-122">Plan hello migration</span></span>

<span data-ttu-id="8b18f-123">Miután eldöntötte, hogy egy meglévő megoldás tooSQL adatraktár toomigrate, akkor fontos tooplan hello áttelepítés elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="8b18f-123">Once you have decided toomigrate an existing solution tooSQL Data Warehouse, it is important tooplan hello migration before getting started.</span></span> 

<span data-ttu-id="8b18f-124">A tervezés egy célja tooensure az adatokat, a táblasémákat és a kódot az SQL Data Warehouse szolgáltatással kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="8b18f-124">One goal of planning is tooensure your data, your table schemas, and your code are compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="8b18f-125">Bizonyos kompatibilitási különbségek toowork, körül a jelenlegi rendszer és az SQL Data Warehouse között van.</span><span class="sxs-lookup"><span data-stu-id="8b18f-125">There are some compatibility differences toowork around between your current system and SQL Data Warehouse.</span></span> <span data-ttu-id="8b18f-126">Ráadásul nagy mennyiségű adatok tooAzure időt vesz áttelepítése.</span><span class="sxs-lookup"><span data-stu-id="8b18f-126">Plus, migrating large amounts of data tooAzure takes time.</span></span> <span data-ttu-id="8b18f-127">Gondosan meg kell tervezni a adatok tooAzure első gyorsítja.</span><span class="sxs-lookup"><span data-stu-id="8b18f-127">Careful planning expedites getting your data tooAzure.</span></span> 

<span data-ttu-id="8b18f-128">Egy másik tervezési célja, a megoldás kihasználja az SQL Data warehouse hello a lekérdezési teljesítmény módosításának tooensure tervezett tooprovide toomake Tervező.</span><span class="sxs-lookup"><span data-stu-id="8b18f-128">Another goal of planning is toomake design adjustments tooensure your solution takes advantage of hello high query performance SQL Data Warehouse is designed tooprovide.</span></span> <span data-ttu-id="8b18f-129">Bővített adatraktárak tervezése vezet be a különböző kialakítási minta, és így hagyományos megközelítés nem mindig hello legjobban.</span><span class="sxs-lookup"><span data-stu-id="8b18f-129">Designing data warehouses for scale introduces different design patterns and so traditional approaches aren't always hello best.</span></span> <span data-ttu-id="8b18f-130">Bár bizonyos tervezési módosításokra áttelepítés után, későbbi módosítások hamarabb hello folyamat menti.</span><span class="sxs-lookup"><span data-stu-id="8b18f-130">Although you can make some design adjustments after migration, making changes sooner in hello process will save time later.</span></span>

<span data-ttu-id="8b18f-131">a sikeres áttelepítéshez a tooperform, kell toomigrate a táblasémákat, a kódot és az adatokat.</span><span class="sxs-lookup"><span data-stu-id="8b18f-131">tooperform a successful migration, you need toomigrate your table schemas, your code, and your data.</span></span> <span data-ttu-id="8b18f-132">Az alábbi áttelepítési témakörök útmutatást lásd:</span><span class="sxs-lookup"><span data-stu-id="8b18f-132">For guidance on these migration topics, see:</span></span>

-  [<span data-ttu-id="8b18f-133">Telepítse át a sémák</span><span class="sxs-lookup"><span data-stu-id="8b18f-133">Migrate your schemas</span></span>](sql-data-warehouse-migrate-schema.md)
-  [<span data-ttu-id="8b18f-134">Telepítse át a kódot</span><span class="sxs-lookup"><span data-stu-id="8b18f-134">Migrate your code</span></span>](sql-data-warehouse-migrate-code.md)
-  <span data-ttu-id="8b18f-135">[Adatok áttelepítése](sql-data-warehouse-migrate-data.md).</span><span class="sxs-lookup"><span data-stu-id="8b18f-135">[Migrate your data](sql-data-warehouse-migrate-data.md).</span></span> 

<!--
## Perform hello migration


## Deploy hello solution


## Validate hello migration

-->

## <a name="next-steps"></a><span data-ttu-id="8b18f-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8b18f-136">Next steps</span></span>
<span data-ttu-id="8b18f-137">hello CAT (Ügyféltanácsadói csapatának) is rendelkezik néhány nagy az SQL Data Warehouse útmutatást, amelyek ezek teszik közzé a blogok keresztül.</span><span class="sxs-lookup"><span data-stu-id="8b18f-137">hello CAT (Customer Advisory Team) also has some great SQL Data Warehouse guidance, which they publish through blogs.</span></span>  <span data-ttu-id="8b18f-138">Tekintse meg a cikk [áttelepítése adatok tooAzure SQL Data Warehouse a gyakorlatban] [ Migrating data tooAzure SQL Data Warehouse in practice] további útmutatást nyújt az áttelepítési.</span><span class="sxs-lookup"><span data-stu-id="8b18f-138">Take a look at their article, [Migrating data tooAzure SQL Data Warehouse in practice][Migrating data tooAzure SQL Data Warehouse in practice] for additional guidance on migration.</span></span>

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data tooAzure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
