---
title: "SQL Data Warehouse gyakran ismételt kérdések aaaAzure |} Microsoft Docs"
description: "Ez a cikk felsorolja, ügyfelek és a fejlesztők Azure SQL Data Warehouse kapcsolatos gyakran ismételt kérdések"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: 812CA525-3BF3-49DF-8DF3-FB4342464F4F
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 3/1/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: 09fd3f65d9507b09fcb8f477742c7d020add2755
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-frequently-asked-questions"></a><span data-ttu-id="7188b-103">SQL Data Warehouse gyakran ismételt kérdések</span><span class="sxs-lookup"><span data-stu-id="7188b-103">SQL Data Warehouse Frequently asked questions</span></span>

## <a name="general"></a><span data-ttu-id="7188b-104">Általános kérdések</span><span class="sxs-lookup"><span data-stu-id="7188b-104">General</span></span>

<span data-ttu-id="7188b-105">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-105">Q.</span></span> <span data-ttu-id="7188b-106">Mi nem SQL DW ajánlat adatbiztonságot?</span><span class="sxs-lookup"><span data-stu-id="7188b-106">What does SQL DW offer for data security?</span></span>

<span data-ttu-id="7188b-107">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-107">A.</span></span> <span data-ttu-id="7188b-108">SQL DW TDE például az adatok védelmének és naplózási számos megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="7188b-108">SQL DW offers several solutions for protecting data such as TDE and auditing.</span></span> <span data-ttu-id="7188b-109">További információkért lásd: [biztonsági].</span><span class="sxs-lookup"><span data-stu-id="7188b-109">For more information, see [Security].</span></span>

<span data-ttu-id="7188b-110">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-110">Q.</span></span> <span data-ttu-id="7188b-111">Hol található, milyen jogi vagy üzleti szabványok SQL DW megfelelő?</span><span class="sxs-lookup"><span data-stu-id="7188b-111">Where can I find out what legal or business standards is SQL DW compliant with?</span></span>

<span data-ttu-id="7188b-112">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-112">A.</span></span> <span data-ttu-id="7188b-113">A Microsoft hello [Microsoft Compliance] például SOC és ISO termék különböző megfelelőségi ajánlatok lapját.</span><span class="sxs-lookup"><span data-stu-id="7188b-113">Visit hello [Microsoft Compliance] page for various compliance offerings by product such as SOC and ISO.</span></span> <span data-ttu-id="7188b-114">Először megfelelőségi cím akkor válassza ki, majd bontsa ki az Azure hello Microsoft a-hatókör felhőalapú szolgáltatások szakaszában hello jobb oldalán hello lap toosee milyen szolgáltatásokat Azure-szolgáltatások rendszer megfelelőek.</span><span class="sxs-lookup"><span data-stu-id="7188b-114">First choose by Compliance title, then expand Azure in hello Microsoft in-scope cloud services section on hello right side of hello page toosee what services are Azure services are compliant.</span></span>

<span data-ttu-id="7188b-115">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-115">Q.</span></span> <span data-ttu-id="7188b-116">Kapcsolódhatnak a powerbi-ban?</span><span class="sxs-lookup"><span data-stu-id="7188b-116">Can I connect PowerBI?</span></span>

<span data-ttu-id="7188b-117">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-117">A.</span></span> <span data-ttu-id="7188b-118">Igen!</span><span class="sxs-lookup"><span data-stu-id="7188b-118">Yes!</span></span> <span data-ttu-id="7188b-119">Bár a Power bi támogatja a közvetlen lekérdezés SQL DW, nem célja a felhasználók vagy a valós idejű adatok nagy számú.</span><span class="sxs-lookup"><span data-stu-id="7188b-119">Though PowerBI supports direct query with SQL DW, it’s not intended for large number of users or real-time data.</span></span> <span data-ttu-id="7188b-120">A Power bi üzemi használatra ajánlott Azure Analysis Services vagy az Analysis Service IaaS Power bi használatával.</span><span class="sxs-lookup"><span data-stu-id="7188b-120">For production use of PowerBI, we recommend using PowerBI on top of Azure Analysis Services or Analysis Service IaaS.</span></span> 

<span data-ttu-id="7188b-121">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-121">Q.</span></span> <span data-ttu-id="7188b-122">SQL Data Warehouse kapacitás korlátai</span><span class="sxs-lookup"><span data-stu-id="7188b-122">What are SQL Data Warehouse Capacity Limits?</span></span>

<span data-ttu-id="7188b-123">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-123">A.</span></span> <span data-ttu-id="7188b-124">Tekintse meg az aktuális [kapacitáskorlátait] lap.</span><span class="sxs-lookup"><span data-stu-id="7188b-124">See our current [capacity limits] page.</span></span> 

<span data-ttu-id="7188b-125">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-125">Q.</span></span> <span data-ttu-id="7188b-126">Miért a méretezési / / szüneteltet tart sokáig?</span><span class="sxs-lookup"><span data-stu-id="7188b-126">Why is my Scale/Pause/Resume taking so long?</span></span>

<span data-ttu-id="7188b-127">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-127">A.</span></span> <span data-ttu-id="7188b-128">Különböző tényezők befolyásolhatják a hello idő a számítási műveleteket.</span><span class="sxs-lookup"><span data-stu-id="7188b-128">A variety of factors can influence hello time for compute management operations.</span></span> <span data-ttu-id="7188b-129">Egy közös hosszú ideig futó műveletek: a tranzakciós visszaállítási eset.</span><span class="sxs-lookup"><span data-stu-id="7188b-129">A common case for  long running operations is transactional rollback.</span></span> <span data-ttu-id="7188b-130">A skála vagy a szüneteltetési művelet elindításakor az összes bejövő munkamenetek le vannak tiltva, és lekérdezések merül vannak-e le.</span><span class="sxs-lookup"><span data-stu-id="7188b-130">When a scale or pause operation is initiated, all incoming sessions are blocked and queries are drained.</span></span> <span data-ttu-id="7188b-131">Rendelés tooleave hello rendszer stabil állapotban, a tranzakciók kell állítható vissza a művelet megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="7188b-131">In order tooleave hello system in a stable state, transactions must be rolled back before an operation can commence.</span></span> <span data-ttu-id="7188b-132">hello nagyobb hello száma és tranzakciók nagyobb hello napló mérete, hello tooa stabil rendszerállapot helyreállítása hello hosszabb hello művelet rendszer leállt.</span><span class="sxs-lookup"><span data-stu-id="7188b-132">hello greater hello number and larger hello log size of transactions, hello longer hello operation will be stalled restoring hello system tooa stable state.</span></span>

## <a name="user-support"></a><span data-ttu-id="7188b-133">Felhasználói támogatás</span><span class="sxs-lookup"><span data-stu-id="7188b-133">User support</span></span>

<span data-ttu-id="7188b-134">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-134">Q.</span></span> <span data-ttu-id="7188b-135">Van a szolgáltatás kérelmet, ahol küldje el?</span><span class="sxs-lookup"><span data-stu-id="7188b-135">I have a feature request, where do I submit it?</span></span>

<span data-ttu-id="7188b-136">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-136">A.</span></span> <span data-ttu-id="7188b-137">Ha egy szolgáltatás kérése, küldje el a a [UserVoice] lap</span><span class="sxs-lookup"><span data-stu-id="7188b-137">If you have a feature request, submit it on our [UserVoice] page</span></span>

<span data-ttu-id="7188b-138">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-138">Q.</span></span> <span data-ttu-id="7188b-139">Hogyan tehetem x?</span><span class="sxs-lookup"><span data-stu-id="7188b-139">How can I do x?</span></span>

<span data-ttu-id="7188b-140">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-140">A.</span></span> <span data-ttu-id="7188b-141">Segítségre van az SQL Data Warehouse szolgáltatással fejleszt, a kérdéseit teheti fel a [Stack Overflow] lap.</span><span class="sxs-lookup"><span data-stu-id="7188b-141">For help in developing with SQL Data Warehouse, you can ask questions on our [Stack Overflow] page.</span></span> 

<span data-ttu-id="7188b-142">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-142">Q.</span></span> <span data-ttu-id="7188b-143">Hogyan küldhetek egy támogatási jegy?</span><span class="sxs-lookup"><span data-stu-id="7188b-143">How do I submit a support ticket?</span></span>

<span data-ttu-id="7188b-144">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-144">A.</span></span> <span data-ttu-id="7188b-145">[Támogatási jegy megjelenítése] nyilvántartásához Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="7188b-145">[Support Tickets] can be filed through Azure portal.</span></span>

## <a name="sql-languagefeature-support"></a><span data-ttu-id="7188b-146">SQL-nyelv/szolgáltatás támogatás</span><span class="sxs-lookup"><span data-stu-id="7188b-146">SQL language/feature support</span></span> 

<span data-ttu-id="7188b-147">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-147">Q.</span></span> <span data-ttu-id="7188b-148">Mely adattípusokat támogatja az SQL Data Warehouse?</span><span class="sxs-lookup"><span data-stu-id="7188b-148">What datatypes does SQL Data Warehouse support?</span></span>

<span data-ttu-id="7188b-149">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-149">A.</span></span> <span data-ttu-id="7188b-150">Tekintse meg az SQL Data Warehouse [adattípusok].</span><span class="sxs-lookup"><span data-stu-id="7188b-150">See SQL Data Warehouse [data types].</span></span>

<span data-ttu-id="7188b-151">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-151">Q.</span></span> <span data-ttu-id="7188b-152">Milyen táblában funkciók támogatásához?</span><span class="sxs-lookup"><span data-stu-id="7188b-152">What table features do you support?</span></span>

<span data-ttu-id="7188b-153">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-153">A.</span></span> <span data-ttu-id="7188b-154">Míg az SQL Data Warehouse számos funkcióját támogatja, néhány nem támogatottak, és vannak dokumentálva [tábla nem támogatott funkciókat].</span><span class="sxs-lookup"><span data-stu-id="7188b-154">While SQL Data Warehouse supports many features, some are not supported and are documented in [Unsupported Table Features].</span></span>

## <a name="tooling-and-administration"></a><span data-ttu-id="7188b-155">Tooling és felügyelet</span><span class="sxs-lookup"><span data-stu-id="7188b-155">Tooling and administration</span></span>

<span data-ttu-id="7188b-156">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-156">Q.</span></span> <span data-ttu-id="7188b-157">Támogatott az adatbázis-projekteket a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7188b-157">Do you support Database projects in Visual Studio.</span></span>

<span data-ttu-id="7188b-158">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-158">A.</span></span> <span data-ttu-id="7188b-159">Jelenleg nem támogatott adatbázis-projekteket a Visual Studio az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7188b-159">We currently do not support Database projects in Visual Studio for SQL Data Warehouse.</span></span> <span data-ttu-id="7188b-160">Ha azt szeretné, hogy toocast a szavazás tooget ezt a beállítást, látogasson el a User Voice [adatbázisprojekteket funkció kérés].</span><span class="sxs-lookup"><span data-stu-id="7188b-160">If you'd like toocast a vote tooget this feature, visit our User Voice [Database projects feature request].</span></span>

<span data-ttu-id="7188b-161">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-161">Q.</span></span> <span data-ttu-id="7188b-162">Támogatja az SQL Data Warehouse REST API-kat?</span><span class="sxs-lookup"><span data-stu-id="7188b-162">Does SQL Data Warehouse support REST APIs?</span></span>

<span data-ttu-id="7188b-163">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-163">A.</span></span> <span data-ttu-id="7188b-164">Igen.</span><span class="sxs-lookup"><span data-stu-id="7188b-164">Yes.</span></span> <span data-ttu-id="7188b-165">A legtöbb REST funkciót az SQL Database használható érhető el az SQL Data Warehouse szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="7188b-165">Most REST functionality that can be used with SQL Database is also available with SQL Data Warehouse.</span></span> <span data-ttu-id="7188b-166">Belül dokumentációs oldalakat REST API információkat vagy [MSDN].</span><span class="sxs-lookup"><span data-stu-id="7188b-166">You can find API information within REST documentation pages or [MSDN].</span></span>


## <a name="loading"></a><span data-ttu-id="7188b-167">Betöltés</span><span class="sxs-lookup"><span data-stu-id="7188b-167">Loading</span></span>

<span data-ttu-id="7188b-168">Q.</span><span class="sxs-lookup"><span data-stu-id="7188b-168">Q.</span></span> <span data-ttu-id="7188b-169">Milyen ügyfél-illesztőprogram támogatja?</span><span class="sxs-lookup"><span data-stu-id="7188b-169">What client drivers do you support?</span></span>

<span data-ttu-id="7188b-170">A.</span><span class="sxs-lookup"><span data-stu-id="7188b-170">A.</span></span> <span data-ttu-id="7188b-171">DW illesztőprogram támogatása hello található [kapcsolati karakterláncok] lap</span><span class="sxs-lookup"><span data-stu-id="7188b-171">Driver support for DW can be found on hello [Connection Strings] page</span></span>

<span data-ttu-id="7188b-172">K: milyen fájlformátumok az SQL Data Warehouse PolyBase által támogatott?</span><span class="sxs-lookup"><span data-stu-id="7188b-172">Q: What file formats are supported by PolyBase with SQL Data Warehouse?</span></span>

<span data-ttu-id="7188b-173">V: Orc, RC, Parquet és strukturálatlan tagolt szövegfájl</span><span class="sxs-lookup"><span data-stu-id="7188b-173">A: Orc, RC, Parquet, and flat delimited text</span></span>

<span data-ttu-id="7188b-174">K: Mi is a PolyBase használatával toofrom SQL DW csatlakozom?</span><span class="sxs-lookup"><span data-stu-id="7188b-174">Q: What can I connect toofrom SQL DW using PolyBase?</span></span> 

<span data-ttu-id="7188b-175">V: [az Azure Data Lake Store] és [Azure Storage blobs szolgáltatásban]</span><span class="sxs-lookup"><span data-stu-id="7188b-175">A: [Azure Data Lake Store] and [Azure Storage Blobs]</span></span>

<span data-ttu-id="7188b-176">K: az számítási leküldéses közzététele lehetséges, tooAzure Storage Blobsba vagy ADLS kapcsolódáskor?</span><span class="sxs-lookup"><span data-stu-id="7188b-176">Q: Is computation pushdown possible  when connecting tooAzure Storage Blobs or ADLS?</span></span> 

<span data-ttu-id="7188b-177">A: SQL DW PolyBase nem, csak hello tárolási összetevőinek együttműködik.</span><span class="sxs-lookup"><span data-stu-id="7188b-177">A: No, SQL DW PolyBase only interacts hello storage components.</span></span> 

<span data-ttu-id="7188b-178">K: tooHDI csatlakozni?</span><span class="sxs-lookup"><span data-stu-id="7188b-178">Q: Can I connect tooHDI?</span></span>

<span data-ttu-id="7188b-179">V: HDI ADLS vagy a WASB használhat hello HDFS réteg.</span><span class="sxs-lookup"><span data-stu-id="7188b-179">A: HDI can use either ADLS or WASB as hello HDFS layer.</span></span> <span data-ttu-id="7188b-180">Ha a HDFS rétegként vagy, majd akkor is, hogy adatok betöltése az SQL DW.</span><span class="sxs-lookup"><span data-stu-id="7188b-180">If you have either as your HDFS layer, then you can load that data into SQL DW.</span></span> <span data-ttu-id="7188b-181">Azonban leküldéses közzététele számítási toohello HDI példánya nem hozható létre.</span><span class="sxs-lookup"><span data-stu-id="7188b-181">However, you cannot generate pushdown computation toohello HDI instance.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="7188b-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7188b-182">Next steps</span></span>
<span data-ttu-id="7188b-183">A teljes SQL Data warehouse további információkért lásd: a [áttekintése] lap.</span><span class="sxs-lookup"><span data-stu-id="7188b-183">For more information on SQL Data Warehouse as a whole, see our [Overview] page.</span></span>


<!-- Article references -->
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[kapcsolati karakterláncok]: ./sql-data-warehouse-connection-strings.md
[Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Támogatási jegy megjelenítése]: ./sql-data-warehouse-get-started-create-support-ticket.md
[biztonsági]: ./sql-data-warehouse-overview-manage-security.md
[Microsoft Compliance]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings
[kapacitáskorlátait]: ./sql-data-warehouse-service-capacity-limits.md
[adattípusok]: ./sql-data-warehouse-tables-data-types.md
[tábla nem támogatott funkciókat]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[az Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md
[Azure Storage blobs szolgáltatásban]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[adatbázisprojekteket funkció kérés]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
[MSDN]: https://msdn.microsoft.com/en-us/library/azure/mt163685.aspx
[áttekintése]: ./sql-data-warehouse-overview-faq.md