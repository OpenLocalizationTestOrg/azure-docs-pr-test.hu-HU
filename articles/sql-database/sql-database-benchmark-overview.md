---
title: "aaaAzure SQL-adatbázis referenciaalap áttekintésében"
description: "Ez a témakör ismerteti a hello Azure SQL adatbázis teljesítményteszt használni a hello Azure SQL Database teljesítményét."
services: sql-database
documentationcenter: na
author: jan-eng
manager: jhubbard
editor: monicar
ms.assetid: e26f8a66-2c12-49d7-8297-45b4d48a5c01
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/21/2016
ms.author: janeng
ms.openlocfilehash: 1024e9ada511935f911cb1345b4dc5508997c702
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-benchmark-overview"></a><span data-ttu-id="f474f-103">Az Azure SQL Database teljesítménytesztek áttekintése</span><span class="sxs-lookup"><span data-stu-id="f474f-103">Azure SQL Database benchmark overview</span></span>
## <a name="overview"></a><span data-ttu-id="f474f-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f474f-104">Overview</span></span>
<span data-ttu-id="f474f-105">A Microsoft Azure SQL Database nyújt három [szolgáltatásszintek](sql-database-service-tiers.md) különböző teljesítményszintek.</span><span class="sxs-lookup"><span data-stu-id="f474f-105">Microsoft Azure SQL Database offers three [service tiers](sql-database-service-tiers.md) with multiple performance levels.</span></span> <span data-ttu-id="f474f-106">Minden egyes teljesítményszintet biztosít egy készletét erőforrásokhoz, vagy a "power" tervezett toodeliver egyre nagyobb teljesítmény növelése.</span><span class="sxs-lookup"><span data-stu-id="f474f-106">Each performance level provides an increasing set of resources, or ‘power’, designed toodeliver increasingly higher throughput.</span></span>

<span data-ttu-id="f474f-107">Fontos toobe képes tooquantify hogyan egyes növekvő hatványa hello fordítja le nagyobb adatbázis-teljesítményt is.</span><span class="sxs-lookup"><span data-stu-id="f474f-107">It is important toobe able tooquantify how hello increasing power of each performance level translates into increased database performance.</span></span> <span data-ttu-id="f474f-108">a Microsoft kifejlesztette toodo hello Azure SQL adatbázis Referenciaalap (ASDB).</span><span class="sxs-lookup"><span data-stu-id="f474f-108">toodo this Microsoft has developed hello Azure SQL Database Benchmark (ASDB).</span></span> <span data-ttu-id="f474f-109">hello teljesítményteszt gyakorolja vegyesen található összes OLTP-munkaterhelések az alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="f474f-109">hello benchmark exercises a mix of basic operations found in all OLTP workloads.</span></span> <span data-ttu-id="f474f-110">Mérjük hello átviteli teljesíteni az egyes futó adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="f474f-110">We measure hello throughput achieved for databases running in each performance level.</span></span>

<span data-ttu-id="f474f-111">hello erőforrások és a tápkábelek minden egyes szolgáltatást és teljesítményszintet szint szerint van megadva a [adatbázis-tranzakciós egységek (dtu-k)](sql-database-what-is-a-dtu.md).</span><span class="sxs-lookup"><span data-stu-id="f474f-111">hello resources and power of each service tier and performance level are expressed in terms of [Database Transaction Units (DTUs)](sql-database-what-is-a-dtu.md).</span></span> <span data-ttu-id="f474f-112">Dtu-k toodescribe hello relatív kapacitását a teljesítményszintet Processzor, memória, kevert mértéke alapján és olvasási és írási sebesség egyes által kínált adja meg.</span><span class="sxs-lookup"><span data-stu-id="f474f-112">DTUs provide a way toodescribe hello relative capacity of a performance level based on a blended measure of CPU, memory, and read and write rates offered by each performance level.</span></span> <span data-ttu-id="f474f-113">Így dupla hello DTU minősítés adatbázis csatlakozás toodoubling hello adatbázis power.</span><span class="sxs-lookup"><span data-stu-id="f474f-113">Doubling hello DTU rating of a database equates toodoubling hello database power.</span></span> <span data-ttu-id="f474f-114">hello teljesítményteszt lehetővé teszi tooassess hello hatása az adatbázis teljesítményének növelése által tényleges adatbázis-művelet gyakorló arányban adatbázis mérete, a felhasználók számát, és tranzakciós díjakat biztosítanak méretezés közben egyes által kínált power hello toohello biztosított erőforrások toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="f474f-114">hello benchmark allows us tooassess hello impact on database performance of hello increasing power offered by each performance level by exercising actual database operations, while scaling database size, number of users, and transaction rates in proportion toohello resources provided toohello database.</span></span>

<span data-ttu-id="f474f-115">Által hello átviteli hello alapvető szolgáltatásréteg tranzakciók-óránként használatával kifejező, hello szabványos szolgáltatásréteg tranzakciók perc és hello prémium szolgáltatásszintet tranzakciók másodpercenkénti, teszi, hogy könnyebben tooquickly használatával hello vonatkoznak minden egyes réteg toohello követelményei az alkalmazások teljesítményének lehetséges.</span><span class="sxs-lookup"><span data-stu-id="f474f-115">By expressing hello throughput of hello Basic service tier using transactions per-hour, hello Standard service tier using transactions per-minute, and hello Premium service tier using transactions per-second, it makes it easier tooquickly relate hello performance potential of each service tier toohello requirements of an application.</span></span>

## <a name="correlating-benchmark-results-tooreal-world-database-performance"></a><span data-ttu-id="f474f-116">Adatok teljesítményteszt eredmények tooreal globális adatbázis teljesítménye</span><span class="sxs-lookup"><span data-stu-id="f474f-116">Correlating benchmark results tooreal world database performance</span></span>
<span data-ttu-id="f474f-117">Ez fontos toounderstand adott ASDB, például az összes referenciaalapokhoz képest, a reprezentatív és tájékoztató csak.</span><span class="sxs-lookup"><span data-stu-id="f474f-117">It is important toounderstand that ASDB, like all benchmarks, is representative and indicative only.</span></span> <span data-ttu-id="f474f-118">hello tranzakció hello teljesítményteszt alkalmazással elért díjszabás fog nem lehet hello ugyanúgy, előfordulhat, hogy érhető el, egyéb alkalmazásokkal.</span><span class="sxs-lookup"><span data-stu-id="f474f-118">hello transaction rates achieved with hello benchmark application will not be hello same as those that might be achieved with other applications.</span></span> <span data-ttu-id="f474f-119">hello teljesítményteszt típusok futtatni egy séma tartalmazó táblák és adattípusokat számos különböző tranzakció gyűjteményét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f474f-119">hello benchmark comprises a collection of different transaction types run against a schema containing a range of tables and data types.</span></span> <span data-ttu-id="f474f-120">Hello teljesítményteszt gyakorlatok hello alapszintű műveletek közös tooall az OLTP-munkaterhelések, amíg nem jelent az adatbázis vagy az alkalmazás bármely adott osztály.</span><span class="sxs-lookup"><span data-stu-id="f474f-120">While hello benchmark exercises hello same basic operations that are common tooall OLTP workloads, it does not represent any specific class of database or application.</span></span> <span data-ttu-id="f474f-121">hello hello teljesítményteszt célja tooprovide egy ésszerű útmutató toohello relatív várható amikor felfelé vagy lefelé skálázás teljesítményszintek között adatbázis teljesítménye.</span><span class="sxs-lookup"><span data-stu-id="f474f-121">hello goal of hello benchmark is tooprovide a reasonable guide toohello relative performance of a database that might be expected when scaling up or down between performance levels.</span></span> <span data-ttu-id="f474f-122">A valóságban adatbázisok különböző méretű és összetettségét, munkaterhelések különböző keverékei előforduló, és különböző módokon reagál.</span><span class="sxs-lookup"><span data-stu-id="f474f-122">In reality, databases are of different sizes and complexity, encounter different mixes of workloads, and will respond in different ways.</span></span> <span data-ttu-id="f474f-123">Például egy IO-igényes alkalmazás előfordulhat, hogy hamarabb találati IO küszöbértékeket, vagy egy processzorigényes alkalmazás előfordulhat, hogy hamarabb találati CPU-korlátok.</span><span class="sxs-lookup"><span data-stu-id="f474f-123">For example, an IO-intensive application may hit IO thresholds sooner, or a CPU-intensive application may hit CPU limits sooner.</span></span> <span data-ttu-id="f474f-124">Nem biztos, hogy bármely adott adatbázis méretezni a azonos módon növelésével hello alapként betöltése hello van.</span><span class="sxs-lookup"><span data-stu-id="f474f-124">There is no guarantee that any particular database will scale in hello same way as hello benchmark under increasing load.</span></span>

<span data-ttu-id="f474f-125">hello teljesítményteszt és annak módszereket ismertetik részletesebben az alábbi.</span><span class="sxs-lookup"><span data-stu-id="f474f-125">hello benchmark and its methodology are described in more detail below.</span></span>

## <a name="benchmark-summary"></a><span data-ttu-id="f474f-126">Teljesítményteszt összegzése</span><span class="sxs-lookup"><span data-stu-id="f474f-126">Benchmark summary</span></span>
<span data-ttu-id="f474f-127">ASDB méri az online tranzakció-feldolgozási (OLTP) munkaterhelések leggyakrabban előforduló alapszintű adatbázis-műveletek vegyesen hello teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="f474f-127">ASDB measures hello performance of a mix of basic database operations which occur most frequently in online transaction processing (OLTP) workloads.</span></span> <span data-ttu-id="f474f-128">Bár hello teljesítményteszt célja a felhőalapú informatika szem előtt, hello adatbázisséma, az adatok feltöltése és a tranzakciók voltak tervezett toobe széleskörűen képviseli az OLTP-munkaterhelések leggyakrabban használt alapvető elemei hello.</span><span class="sxs-lookup"><span data-stu-id="f474f-128">Although hello benchmark is designed with cloud computing in mind, hello database schema, data population, and transactions have been designed toobe broadly representative of hello basic elements most commonly used in OLTP workloads.</span></span>

## <a name="schema"></a><span data-ttu-id="f474f-129">Séma</span><span class="sxs-lookup"><span data-stu-id="f474f-129">Schema</span></span>
<span data-ttu-id="f474f-130">hello séma tervezett toohave van elegendő különböző és a bonyolultságot toosupport műveletek széles körét.</span><span class="sxs-lookup"><span data-stu-id="f474f-130">hello schema is designed toohave enough variety and complexity toosupport a broad range of operations.</span></span> <span data-ttu-id="f474f-131">hello teljesítményteszt fut egy adatbázist hat táblák áll.</span><span class="sxs-lookup"><span data-stu-id="f474f-131">hello benchmark runs against a database comprised of six tables.</span></span> <span data-ttu-id="f474f-132">hello táblák három kategóriába sorolhatók: rögzített méretű, méretezés és növekedését.</span><span class="sxs-lookup"><span data-stu-id="f474f-132">hello tables fall into three categories: fixed-size, scaling, and growing.</span></span> <span data-ttu-id="f474f-133">Két rögzített méretű táblák; három méretezési táblák; és egy egyre bővülő tábla.</span><span class="sxs-lookup"><span data-stu-id="f474f-133">There are two fixed-size tables; three scaling tables; and one growing table.</span></span> <span data-ttu-id="f474f-134">Rögzített méretű sorok számának állandó kell.</span><span class="sxs-lookup"><span data-stu-id="f474f-134">Fixed-size tables have a constant number of rows.</span></span> <span data-ttu-id="f474f-135">Méretezési arányos toodatabase teljesítmény, de nem módosítja a hello teljesítménytesztek során számosságuk kell.</span><span class="sxs-lookup"><span data-stu-id="f474f-135">Scaling tables have a cardinality that is proportional toodatabase performance, but doesn’t change during hello benchmark.</span></span> <span data-ttu-id="f474f-136">például egy méretezési táblázat első betöltéskor tábla növekvő hello mérete, de majd hello számossága módosításait hello állomásokon futó hello teljesítményteszt sort beszúrni, és törölni.</span><span class="sxs-lookup"><span data-stu-id="f474f-136">hello growing table is sized like a scaling table on initial load, but then hello cardinality changes in hello course of running hello benchmark as rows are inserted and deleted.</span></span>

<span data-ttu-id="f474f-137">hello séma tartalmaz vegyesen adattípusok, beleértve az egész szám, numerikus, a karakter, és a dátum/idő.</span><span class="sxs-lookup"><span data-stu-id="f474f-137">hello schema includes a mix of data types, including integer, numeric, character, and date/time.</span></span> <span data-ttu-id="f474f-138">hello séma tartalmaz az elsődleges és másodlagos kulcsok, de nem idegen kulcsokkal – Ez azt jelenti, hogy nincsenek hivatkozási integritási megkötés táblák között.</span><span class="sxs-lookup"><span data-stu-id="f474f-138">hello schema includes primary and secondary keys, but not any foreign keys - that is, there are no referential integrity constraints between tables.</span></span>

<span data-ttu-id="f474f-139">Adatok generálása program hello adatokat hello kezdeti adatbázis állít elő.</span><span class="sxs-lookup"><span data-stu-id="f474f-139">A data generation program generates hello data for hello initial database.</span></span> <span data-ttu-id="f474f-140">Az egész és numerikus adatokat a különböző stratégiák jön létre.</span><span class="sxs-lookup"><span data-stu-id="f474f-140">Integer and numeric data is generated with various strategies.</span></span> <span data-ttu-id="f474f-141">Bizonyos esetekben az értékek vannak osztva véletlenszerűen tartományban.</span><span class="sxs-lookup"><span data-stu-id="f474f-141">In some cases, values are distributed randomly over a range.</span></span> <span data-ttu-id="f474f-142">Más esetekben az értékek egy halmazát, hogy megőrződjön-e egy adott terjesztési véletlenszerűen a(z) tooensure.</span><span class="sxs-lookup"><span data-stu-id="f474f-142">In other cases, a set of values is randomly permuted tooensure that a specific distribution is maintained.</span></span> <span data-ttu-id="f474f-143">Szövegmező szavak tooproduce reális kinézetű adatok súlyozott listájából jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="f474f-143">Text fields are generated from a weighted list of words tooproduce realistic looking data.</span></span>

<span data-ttu-id="f474f-144">hello adatbázis mérete alapján egy "Mérettényező."</span><span class="sxs-lookup"><span data-stu-id="f474f-144">hello database is sized based on a “scale factor.”</span></span> <span data-ttu-id="f474f-145">hello méretezési tényező (KB rövidítése) a méretezés és táblák növekvő hello hello számossága határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f474f-145">hello scale factor (abbreviated as SF) determines hello cardinality of hello scaling and growing tables.</span></span> <span data-ttu-id="f474f-146">Az alábbiakban leírtak hello szakasz felhasználók és Pacing, hello adatbázis mérete, a felhasználók számát, és minden egyéb arányban tooeach méretezése a legjobb teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="f474f-146">As described below in hello section Users and Pacing, hello database size, number of users, and maximum performance all scale in proportion tooeach other.</span></span>

## <a name="transactions"></a><span data-ttu-id="f474f-147">Tranzakciók</span><span class="sxs-lookup"><span data-stu-id="f474f-147">Transactions</span></span>
<span data-ttu-id="f474f-148">hello munkaterhelés áll kilenc tranzakciótípusok, ahogy az az alábbi táblázat hello.</span><span class="sxs-lookup"><span data-stu-id="f474f-148">hello workload consists of nine transaction types, as shown in hello table below.</span></span> <span data-ttu-id="f474f-149">Minden tranzakció tervezett toohighlight egy meghatározott hello adatbázis-motor és a rendszer hardver jellemzői, kontrasztos megjelenítés a a hello más tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="f474f-149">Each transaction is designed toohighlight a particular set of system characteristics in hello database engine and system hardware, with high contrast from hello other transactions.</span></span> <span data-ttu-id="f474f-150">Ez a megközelítés lehetővé teszi különböző összetevői toooverall teljesítmény könnyebb tooassess hello hatása.</span><span class="sxs-lookup"><span data-stu-id="f474f-150">This approach makes it easier tooassess hello impact of different components toooverall performance.</span></span> <span data-ttu-id="f474f-151">Például "Olvasás nehéz" hello tranzakció eredménye olyan jelentős lemezről az olvasási műveletek.</span><span class="sxs-lookup"><span data-stu-id="f474f-151">For example, hello transaction “Read Heavy” produces a significant number of read operations from disk.</span></span>

| <span data-ttu-id="f474f-152">Tranzakció típusa</span><span class="sxs-lookup"><span data-stu-id="f474f-152">Transaction Type</span></span> | <span data-ttu-id="f474f-153">Leírás</span><span class="sxs-lookup"><span data-stu-id="f474f-153">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f474f-154">Olvassa el a Lite</span><span class="sxs-lookup"><span data-stu-id="f474f-154">Read Lite</span></span> |<span data-ttu-id="f474f-155">VÁLASSZA KI A; a memóriában; csak olvasható</span><span class="sxs-lookup"><span data-stu-id="f474f-155">SELECT; in-memory; read-only</span></span> |
| <span data-ttu-id="f474f-156">Olvasási közepes</span><span class="sxs-lookup"><span data-stu-id="f474f-156">Read Medium</span></span> |<span data-ttu-id="f474f-157">VÁLASSZA KI A; legtöbbször a memóriában; csak olvasható</span><span class="sxs-lookup"><span data-stu-id="f474f-157">SELECT; mostly in-memory; read-only</span></span> |
| <span data-ttu-id="f474f-158">Olvasási gyakori</span><span class="sxs-lookup"><span data-stu-id="f474f-158">Read Heavy</span></span> |<span data-ttu-id="f474f-159">VÁLASSZA KI A; többnyire nincs a memóriában; csak olvasható</span><span class="sxs-lookup"><span data-stu-id="f474f-159">SELECT; mostly not in-memory; read-only</span></span> |
| <span data-ttu-id="f474f-160">A frissítés Lite</span><span class="sxs-lookup"><span data-stu-id="f474f-160">Update Lite</span></span> |<span data-ttu-id="f474f-161">FRISSÍTÉS; a memóriában; olvasási és írási</span><span class="sxs-lookup"><span data-stu-id="f474f-161">UPDATE; in-memory; read-write</span></span> |
| <span data-ttu-id="f474f-162">Gyakori frissítése</span><span class="sxs-lookup"><span data-stu-id="f474f-162">Update Heavy</span></span> |<span data-ttu-id="f474f-163">FRISSÍTÉS; többnyire nincs a memóriában; olvasási és írási</span><span class="sxs-lookup"><span data-stu-id="f474f-163">UPDATE; mostly not in-memory; read-write</span></span> |
| <span data-ttu-id="f474f-164">INSERT Lite</span><span class="sxs-lookup"><span data-stu-id="f474f-164">Insert Lite</span></span> |<span data-ttu-id="f474f-165">INSERT; a memóriában; olvasási és írási</span><span class="sxs-lookup"><span data-stu-id="f474f-165">INSERT; in-memory; read-write</span></span> |
| <span data-ttu-id="f474f-166">Gyakori beszúrása</span><span class="sxs-lookup"><span data-stu-id="f474f-166">Insert Heavy</span></span> |<span data-ttu-id="f474f-167">INSERT; többnyire nincs a memóriában; olvasási és írási</span><span class="sxs-lookup"><span data-stu-id="f474f-167">INSERT; mostly not in-memory; read-write</span></span> |
| <span data-ttu-id="f474f-168">Törlés</span><span class="sxs-lookup"><span data-stu-id="f474f-168">Delete</span></span> |<span data-ttu-id="f474f-169">TÖRLÉS; a memóriában, és nem memóriában; olvasási és írási</span><span class="sxs-lookup"><span data-stu-id="f474f-169">DELETE; mix of in-memory and not in-memory; read-write</span></span> |
| <span data-ttu-id="f474f-170">CPU gyakori</span><span class="sxs-lookup"><span data-stu-id="f474f-170">CPU Heavy</span></span> |<span data-ttu-id="f474f-171">VÁLASSZA KI A; a memóriában; CPU-terhelés viszonylag nagy mennyiségű; csak olvasható</span><span class="sxs-lookup"><span data-stu-id="f474f-171">SELECT; in-memory; relatively heavy CPU load; read-only</span></span> |

## <a name="workload-mix"></a><span data-ttu-id="f474f-172">Munkaterhelés-vegyes</span><span class="sxs-lookup"><span data-stu-id="f474f-172">Workload mix</span></span>
<span data-ttu-id="f474f-173">Tranzakciók vannak véletlenszerűen kiválasztott egy súlyozott terjesztési hello a következő általános kettőn együtt.</span><span class="sxs-lookup"><span data-stu-id="f474f-173">Transactions are selected at random from a weighted distribution with hello following overall mix.</span></span> <span data-ttu-id="f474f-174">hello általános vegyes van egy olvasási/írási arányt körülbelül 2:1.</span><span class="sxs-lookup"><span data-stu-id="f474f-174">hello overall mix has a read/write ratio of approximately 2:1.</span></span>

| <span data-ttu-id="f474f-175">Tranzakció típusa</span><span class="sxs-lookup"><span data-stu-id="f474f-175">Transaction Type</span></span> | <span data-ttu-id="f474f-176">Vegyes %</span><span class="sxs-lookup"><span data-stu-id="f474f-176">% of Mix</span></span> |
| --- | --- |
| <span data-ttu-id="f474f-177">Olvassa el a Lite</span><span class="sxs-lookup"><span data-stu-id="f474f-177">Read Lite</span></span> |<span data-ttu-id="f474f-178">35</span><span class="sxs-lookup"><span data-stu-id="f474f-178">35</span></span> |
| <span data-ttu-id="f474f-179">Olvasási közepes</span><span class="sxs-lookup"><span data-stu-id="f474f-179">Read Medium</span></span> |<span data-ttu-id="f474f-180">20</span><span class="sxs-lookup"><span data-stu-id="f474f-180">20</span></span> |
| <span data-ttu-id="f474f-181">Olvasási gyakori</span><span class="sxs-lookup"><span data-stu-id="f474f-181">Read Heavy</span></span> |<span data-ttu-id="f474f-182">5</span><span class="sxs-lookup"><span data-stu-id="f474f-182">5</span></span> |
| <span data-ttu-id="f474f-183">A frissítés Lite</span><span class="sxs-lookup"><span data-stu-id="f474f-183">Update Lite</span></span> |<span data-ttu-id="f474f-184">20</span><span class="sxs-lookup"><span data-stu-id="f474f-184">20</span></span> |
| <span data-ttu-id="f474f-185">Gyakori frissítése</span><span class="sxs-lookup"><span data-stu-id="f474f-185">Update Heavy</span></span> |<span data-ttu-id="f474f-186">3</span><span class="sxs-lookup"><span data-stu-id="f474f-186">3</span></span> |
| <span data-ttu-id="f474f-187">INSERT Lite</span><span class="sxs-lookup"><span data-stu-id="f474f-187">Insert Lite</span></span> |<span data-ttu-id="f474f-188">3</span><span class="sxs-lookup"><span data-stu-id="f474f-188">3</span></span> |
| <span data-ttu-id="f474f-189">Gyakori beszúrása</span><span class="sxs-lookup"><span data-stu-id="f474f-189">Insert Heavy</span></span> |<span data-ttu-id="f474f-190">2</span><span class="sxs-lookup"><span data-stu-id="f474f-190">2</span></span> |
| <span data-ttu-id="f474f-191">Törlés</span><span class="sxs-lookup"><span data-stu-id="f474f-191">Delete</span></span> |<span data-ttu-id="f474f-192">2</span><span class="sxs-lookup"><span data-stu-id="f474f-192">2</span></span> |
| <span data-ttu-id="f474f-193">CPU gyakori</span><span class="sxs-lookup"><span data-stu-id="f474f-193">CPU Heavy</span></span> |<span data-ttu-id="f474f-194">10</span><span class="sxs-lookup"><span data-stu-id="f474f-194">10</span></span> |

## <a name="users-and-pacing"></a><span data-ttu-id="f474f-195">Felhasználók és pacing</span><span class="sxs-lookup"><span data-stu-id="f474f-195">Users and pacing</span></span>
<span data-ttu-id="f474f-196">hello teljesítményteszt munkaterhelés célja a egy eszköz, amellyel a tranzakciók kapcsolatok toosimulate hello viselkedését számos egyidejű felhasználók csoportja között.</span><span class="sxs-lookup"><span data-stu-id="f474f-196">hello benchmark workload is driven from a tool that submits transactions across a set of connections toosimulate hello behavior of a number of concurrent users.</span></span> <span data-ttu-id="f474f-197">Annak ellenére, hogy az összes hello kapcsolatok és a tranzakciók gép jön létre, az egyszerűség kedvéért irányítjuk toothese kapcsolatok felhasználóként"."</span><span class="sxs-lookup"><span data-stu-id="f474f-197">Although all of hello connections and transactions are machine generated, for simplicity we refer toothese connections as “users.”</span></span> <span data-ttu-id="f474f-198">Bár minden felhasználó működése független a többi felhasználó, a minden felhasználó hello hajtsa végre az alábbi lépéseket azonos ciklus:</span><span class="sxs-lookup"><span data-stu-id="f474f-198">Although each user operates independently of all other users, all users perform hello same cycle of steps shown below:</span></span>

1. <span data-ttu-id="f474f-199">Egy adatbázis-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f474f-199">Establish a database connection.</span></span>
2. <span data-ttu-id="f474f-200">Ismételje meg a jelzett tooexit amíg:</span><span class="sxs-lookup"><span data-stu-id="f474f-200">Repeat until signaled tooexit:</span></span>
   * <span data-ttu-id="f474f-201">Véletlenszerű (a súlyozott terjesztési) egy olyan tranzakciót választott.</span><span class="sxs-lookup"><span data-stu-id="f474f-201">Select a transaction at random (from a weighted distribution).</span></span>
   * <span data-ttu-id="f474f-202">Hajtsa végre a kijelölt hello tranzakció és mérték hello válaszidő.</span><span class="sxs-lookup"><span data-stu-id="f474f-202">Perform hello selected transaction and measure hello response time.</span></span>
   * <span data-ttu-id="f474f-203">Várjon, amíg pacing késést.</span><span class="sxs-lookup"><span data-stu-id="f474f-203">Wait for a pacing delay.</span></span>
3. <span data-ttu-id="f474f-204">Zárja be az adatbázis-kapcsolat hello.</span><span class="sxs-lookup"><span data-stu-id="f474f-204">Close hello database connection.</span></span>
4. <span data-ttu-id="f474f-205">Kilépés.</span><span class="sxs-lookup"><span data-stu-id="f474f-205">Exit.</span></span>

<span data-ttu-id="f474f-206">késleltetése (2/c. lépés) pacing hello véletlenszerűen kiválasztott-e, de a terjesztési, amely rendelkezik 1.0-ás másodpercenként átlagosan.</span><span class="sxs-lookup"><span data-stu-id="f474f-206">hello pacing delay (in step 2c) is selected at random, but with a distribution that has an average of 1.0 second.</span></span> <span data-ttu-id="f474f-207">Így minden felhasználóhoz, átlagosan generálhat másodpercenként legfeljebb egy tranzakciót.</span><span class="sxs-lookup"><span data-stu-id="f474f-207">Thus each user can, on average, generate at most one transaction per second.</span></span>

## <a name="scaling-rules"></a><span data-ttu-id="f474f-208">Skálázási szabályok</span><span class="sxs-lookup"><span data-stu-id="f474f-208">Scaling rules</span></span>
<span data-ttu-id="f474f-209">felhasználók száma hello hello adatbázis mérete (Mérettényező egységekben) határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f474f-209">hello number of users is determined by hello database size (in scale-factor units).</span></span> <span data-ttu-id="f474f-210">Nincs minden öt Mérettényező egységek egy felhasználó.</span><span class="sxs-lookup"><span data-stu-id="f474f-210">There is one user for every five scale-factor units.</span></span> <span data-ttu-id="f474f-211">Hello pacing késleltetés, mert egy felhasználó hozhat létre a másodpercenként legfeljebb egy tranzakciót átlagosan.</span><span class="sxs-lookup"><span data-stu-id="f474f-211">Because of hello pacing delay, one user can generate at most one transaction per second, on average.</span></span>

<span data-ttu-id="f474f-212">Például egy méretezési tényezős 500 (KB = 500) adatbázis 100 felhasználót lesz, és a 100 TP-k maximális sebesség érhető el.</span><span class="sxs-lookup"><span data-stu-id="f474f-212">For example, a scale-factor of 500 (SF=500) database will have 100 users and can achieve a maximum rate of 100 TPS.</span></span> <span data-ttu-id="f474f-213">a magasabb TP-k mértékben toodrive szükséges további felhasználók és egy nagyobb adatbázist.</span><span class="sxs-lookup"><span data-stu-id="f474f-213">toodrive a higher TPS rate requires more users and a larger database.</span></span>

<span data-ttu-id="f474f-214">az alábbi táblázat hello felhasználók ténylegesen az egyes szolgáltatási szint és a teljesítmény szűk hello számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f474f-214">hello table below shows hello number of users actually sustained for each service tier and performance level.</span></span>

| <span data-ttu-id="f474f-215">Szolgáltatási réteg (teljesítményszint szükséges)</span><span class="sxs-lookup"><span data-stu-id="f474f-215">Service Tier (Performance Level)</span></span> | <span data-ttu-id="f474f-216">Felhasználók</span><span class="sxs-lookup"><span data-stu-id="f474f-216">Users</span></span> | <span data-ttu-id="f474f-217">Adatbázisméret</span><span class="sxs-lookup"><span data-stu-id="f474f-217">Database Size</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f474f-218">Basic</span><span class="sxs-lookup"><span data-stu-id="f474f-218">Basic</span></span> |<span data-ttu-id="f474f-219">5</span><span class="sxs-lookup"><span data-stu-id="f474f-219">5</span></span> |<span data-ttu-id="f474f-220">720 MB</span><span class="sxs-lookup"><span data-stu-id="f474f-220">720 MB</span></span> |
| <span data-ttu-id="f474f-221">Standard (S0)</span><span class="sxs-lookup"><span data-stu-id="f474f-221">Standard (S0)</span></span> |<span data-ttu-id="f474f-222">10</span><span class="sxs-lookup"><span data-stu-id="f474f-222">10</span></span> |<span data-ttu-id="f474f-223">1 GB</span><span class="sxs-lookup"><span data-stu-id="f474f-223">1 GB</span></span> |
| <span data-ttu-id="f474f-224">Standard (S1)</span><span class="sxs-lookup"><span data-stu-id="f474f-224">Standard (S1)</span></span> |<span data-ttu-id="f474f-225">20</span><span class="sxs-lookup"><span data-stu-id="f474f-225">20</span></span> |<span data-ttu-id="f474f-226">2.1 GB</span><span class="sxs-lookup"><span data-stu-id="f474f-226">2.1 GB</span></span> |
| <span data-ttu-id="f474f-227">Standard (s2 –)</span><span class="sxs-lookup"><span data-stu-id="f474f-227">Standard (S2)</span></span> |<span data-ttu-id="f474f-228">50</span><span class="sxs-lookup"><span data-stu-id="f474f-228">50</span></span> |<span data-ttu-id="f474f-229">7.1 GB</span><span class="sxs-lookup"><span data-stu-id="f474f-229">7.1 GB</span></span> |
| <span data-ttu-id="f474f-230">Prémium (P1)</span><span class="sxs-lookup"><span data-stu-id="f474f-230">Premium (P1)</span></span> |<span data-ttu-id="f474f-231">100</span><span class="sxs-lookup"><span data-stu-id="f474f-231">100</span></span> |<span data-ttu-id="f474f-232">14 GB</span><span class="sxs-lookup"><span data-stu-id="f474f-232">14 GB</span></span> |
| <span data-ttu-id="f474f-233">Prémium (P2)</span><span class="sxs-lookup"><span data-stu-id="f474f-233">Premium (P2)</span></span> |<span data-ttu-id="f474f-234">200</span><span class="sxs-lookup"><span data-stu-id="f474f-234">200</span></span> |<span data-ttu-id="f474f-235">28 GB</span><span class="sxs-lookup"><span data-stu-id="f474f-235">28 GB</span></span> |
| <span data-ttu-id="f474f-236">Prémium (P6/P3)</span><span class="sxs-lookup"><span data-stu-id="f474f-236">Premium (P6/P3)</span></span> |<span data-ttu-id="f474f-237">800</span><span class="sxs-lookup"><span data-stu-id="f474f-237">800</span></span> |<span data-ttu-id="f474f-238">114 GB</span><span class="sxs-lookup"><span data-stu-id="f474f-238">114 GB</span></span> |

## <a name="measurement-duration"></a><span data-ttu-id="f474f-239">Mérési időtartama</span><span class="sxs-lookup"><span data-stu-id="f474f-239">Measurement duration</span></span>
<span data-ttu-id="f474f-240">Egy érvényes teljesítményteszt futtatásához igényel a stabil állapot mérési időtartama legalább egy órát.</span><span class="sxs-lookup"><span data-stu-id="f474f-240">A valid benchmark run requires a steady-state measurement duration of at least one hour.</span></span>

## <a name="metrics"></a><span data-ttu-id="f474f-241">Mérőszámok</span><span class="sxs-lookup"><span data-stu-id="f474f-241">Metrics</span></span>
<span data-ttu-id="f474f-242">alapvető metrikákat hello hello teljesítményteszt olyan átviteli sebesség és a válaszidőt.</span><span class="sxs-lookup"><span data-stu-id="f474f-242">hello key metrics in hello benchmark are throughput and response time.</span></span>

* <span data-ttu-id="f474f-243">Átviteli sebesség hello alapvető teljesítménymérés hello teljesítményteszt.</span><span class="sxs-lookup"><span data-stu-id="f474f-243">Throughput is hello essential performance measure in hello benchmark.</span></span> <span data-ttu-id="f474f-244">Átviteli sebesség jelentésekről--időegység, minden tranzakciótípus számbavételi tranzakciókat.</span><span class="sxs-lookup"><span data-stu-id="f474f-244">Throughput is reported in transactions per unit-of-time, counting all transaction types.</span></span>
* <span data-ttu-id="f474f-245">Válaszidő teljesítmény kiszámíthatóságot mérőszáma.</span><span class="sxs-lookup"><span data-stu-id="f474f-245">Response time is a measure of performance predictability.</span></span> <span data-ttu-id="f474f-246">Válasz időkorlát hello szolgáltatást, magasabb osztályok szigorúbb válasz idő követelmény, hogy alább látható módon szolgáltatás osztály függ.</span><span class="sxs-lookup"><span data-stu-id="f474f-246">hello response time constraint varies with class of service, with higher classes of service having a more stringent response time requirement, as shown below.</span></span>

| <span data-ttu-id="f474f-247">Szolgáltatás osztálya</span><span class="sxs-lookup"><span data-stu-id="f474f-247">Class of Service</span></span> | <span data-ttu-id="f474f-248">Átviteli sebesség mérték</span><span class="sxs-lookup"><span data-stu-id="f474f-248">Throughput Measure</span></span> | <span data-ttu-id="f474f-249">Válasz idő követelmény</span><span class="sxs-lookup"><span data-stu-id="f474f-249">Response Time Requirement</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f474f-250">Prémium</span><span class="sxs-lookup"><span data-stu-id="f474f-250">Premium</span></span> |<span data-ttu-id="f474f-251">Másodpercenkénti tranzakciók</span><span class="sxs-lookup"><span data-stu-id="f474f-251">Transactions per second</span></span> |<span data-ttu-id="f474f-252">95. százalékos érték 0,5 másodperc:</span><span class="sxs-lookup"><span data-stu-id="f474f-252">95th percentile at 0.5 seconds</span></span> |
| <span data-ttu-id="f474f-253">Standard</span><span class="sxs-lookup"><span data-stu-id="f474f-253">Standard</span></span> |<span data-ttu-id="f474f-254">Percenkénti tranzakciók</span><span class="sxs-lookup"><span data-stu-id="f474f-254">Transactions per minute</span></span> |<span data-ttu-id="f474f-255">90. százalékos érték 1.0-ás másodperc:</span><span class="sxs-lookup"><span data-stu-id="f474f-255">90th percentile at 1.0 seconds</span></span> |
| <span data-ttu-id="f474f-256">Basic</span><span class="sxs-lookup"><span data-stu-id="f474f-256">Basic</span></span> |<span data-ttu-id="f474f-257">Óránkénti tranzakciók</span><span class="sxs-lookup"><span data-stu-id="f474f-257">Transactions per hour</span></span> |<span data-ttu-id="f474f-258">80 PERCENTILIS 2.0 másodperc:</span><span class="sxs-lookup"><span data-stu-id="f474f-258">80th percentile at 2.0 seconds</span></span> |

## <a name="conclusion"></a><span data-ttu-id="f474f-259">Összegzés</span><span class="sxs-lookup"><span data-stu-id="f474f-259">Conclusion</span></span>
<span data-ttu-id="f474f-260">hello Azure SQL adatbázis teljesítményteszt relatív teljesítménye hello Azure SQL Database keresztül elérhető szolgáltatásszintek és teljesítményszintek hello számos méri.</span><span class="sxs-lookup"><span data-stu-id="f474f-260">hello Azure SQL Database Benchmark measures hello relative performance of Azure SQL Database running across hello range of available service tiers and performance levels.</span></span> <span data-ttu-id="f474f-261">hello teljesítményteszt alapszintű adatbázis-műveletek, az online tranzakció-feldolgozási (OLTP) munkaterhelések leggyakrabban előforduló vegyesen gyakorolja.</span><span class="sxs-lookup"><span data-stu-id="f474f-261">hello benchmark exercises a mix of basic database operations which occur most frequently in online transaction processing (OLTP) workloads.</span></span> <span data-ttu-id="f474f-262">Aktuális teljesítménye mérésével hello teljesítményteszt Ez a témakör a részletesebb elemzés hello hatással lehet az átviteli sebességének módosítása hello teljesítményszint szükséges, mint ha csak az egyes szinteken, például a Processzor sebessége, a memóriaméret és a IOPS hello erőforrásokat listázása .</span><span class="sxs-lookup"><span data-stu-id="f474f-262">By measuring actual performance, hello benchmark provides a more meaningful assessment of hello impact on throughput of changing hello performance level than is possible by just listing hello resources provided by each level such as CPU speed, memory size, and IOPS.</span></span> <span data-ttu-id="f474f-263">Hello jövőbeli, a rendszer továbbra is tooevolve hello teljesítményteszt toobroaden hatókörében és bontsa ki a megadott hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="f474f-263">In hello future, we will continue tooevolve hello benchmark toobroaden its scope and expand hello data provided.</span></span>

## <a name="resources"></a><span data-ttu-id="f474f-264">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="f474f-264">Resources</span></span>
[<span data-ttu-id="f474f-265">Bevezetés tooSQL adatbázis</span><span class="sxs-lookup"><span data-stu-id="f474f-265">Introduction tooSQL Database</span></span>](sql-database-technical-overview.md)

[<span data-ttu-id="f474f-266">Szolgáltatásszintek és teljesítményszintek</span><span class="sxs-lookup"><span data-stu-id="f474f-266">Service tiers and performance levels</span></span>](sql-database-service-tiers.md)

[<span data-ttu-id="f474f-267">Az önálló adatbázisok teljesítményének útmutató</span><span class="sxs-lookup"><span data-stu-id="f474f-267">Performance guidance for single databases</span></span>](sql-database-performance-guidance.md)