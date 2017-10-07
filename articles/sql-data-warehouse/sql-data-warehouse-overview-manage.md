---
title: "az Azure SQL Data Warehouse aaaManage adatbázisok |} Microsoft Docs"
description: "SQL Data Warehouse-adatbázisokban kezelésének áttekintése. Felügyeleti eszközök, a dwu-k és kibővített teljesítmény, lekérdezési teljesítményt, a helyes biztonsági házirendek létrehozása, és egy adatbázis visszaállításához adatsérülés akár regionális kimaradás hibaelhárítási tartalmazza."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 8576fdb3-71fe-4b3b-a4e0-5e8a7f148acf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: caec6572c4ab395107c3b095adc69a53eae8bd88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-databases-in-azure-sql-data-warehouse"></a><span data-ttu-id="6f77a-104">Az Azure SQL Data Warehouse adatbázisok kezelése</span><span class="sxs-lookup"><span data-stu-id="6f77a-104">Manage databases in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="6f77a-105">Az SQL Data Warehouse automatizálja az adatbázisok kezelése sok aspektusait.</span><span class="sxs-lookup"><span data-stu-id="6f77a-105">SQL Data Warehouse automates many aspects of managing your databases.</span></span> <span data-ttu-id="6f77a-106">Például csak kell tooadjust és díj ellenében hello jobb szintű számítási erőforrásokat, és hagyja meg az SQL Data Warehouse tooscale teljesítmény munkájuk összes hello kiterjesztése és a méretezés vissza.</span><span class="sxs-lookup"><span data-stu-id="6f77a-106">For example, tooscale performance you only need tooadjust and pay for hello right level of compute resources, and then let SQL Data Warehouse do all hello work of scaling out and scaling back.</span></span>

<span data-ttu-id="6f77a-107">Kétségtelenül érdemes toomonitor a munkaterhelés tooidentify a teljesítmény szükséges, valamint a hosszan futó lekérdezések hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="6f77a-107">You will undoubtedly want toomonitor your workload tooidentify your performance needs as well as troubleshoot long-running queries.</span></span> <span data-ttu-id="6f77a-108">Is szüksége lesz tooperform néhány biztonsági feladatok toomanage engedélyek egyes felhasználókhoz és bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="6f77a-108">You will also need tooperform a few security tasks toomanage permissions for users and logins.</span></span>

<span data-ttu-id="6f77a-109">Ez az Áttekintés ezeket az jellemzőket SQL Data Warehouse felügyeletét tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="6f77a-109">This overview covers these aspects of managing SQL Data Warehouse.</span></span>

* <span data-ttu-id="6f77a-110">Felügyeleti eszközök</span><span class="sxs-lookup"><span data-stu-id="6f77a-110">Management tools</span></span>
* <span data-ttu-id="6f77a-111">Skála számítási</span><span class="sxs-lookup"><span data-stu-id="6f77a-111">Scale Compute</span></span>
* <span data-ttu-id="6f77a-112">Szüneteltetése és folytatása</span><span class="sxs-lookup"><span data-stu-id="6f77a-112">Pause and Resume</span></span>
* <span data-ttu-id="6f77a-113">Teljesítmény gyakorlati tanácsok</span><span class="sxs-lookup"><span data-stu-id="6f77a-113">Performance Best Practices</span></span>
* <span data-ttu-id="6f77a-114">Lekérdezés figyelése</span><span class="sxs-lookup"><span data-stu-id="6f77a-114">Query Monitoring</span></span>
* <span data-ttu-id="6f77a-115">Biztonság</span><span class="sxs-lookup"><span data-stu-id="6f77a-115">Security</span></span>
* <span data-ttu-id="6f77a-116">Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="6f77a-116">Backup and restore</span></span>

## <a name="management-tools"></a><span data-ttu-id="6f77a-117">Felügyeleti eszközök</span><span class="sxs-lookup"><span data-stu-id="6f77a-117">Management tools</span></span>
<span data-ttu-id="6f77a-118">Az SQL Data Warehouse számos eszközök toomanage adatbázissal is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6f77a-118">You can use a variety of tools toomanage databases in SQL Data Warehouse.</span></span> <span data-ttu-id="6f77a-119">Adatbázisok kezelése közben, során elkészít-e az eszköz beállítások az egyes feladatok tooperform kell.</span><span class="sxs-lookup"><span data-stu-id="6f77a-119">As you manage databases, you will develop tool preferences for each type of task you need tooperform.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="6f77a-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6f77a-120">Azure portal</span></span>
<span data-ttu-id="6f77a-121">Hello [Azure-portálon] [ Azure portal] egy webes portál, ahol létrehozása, frissítése, és törölni az adatbázisokat és adatbázis-erőforrások figyelése.</span><span class="sxs-lookup"><span data-stu-id="6f77a-121">hello [Azure portal][Azure portal] is a web-based portal where you can create, update, and delete databases and monitor database resources.</span></span> <span data-ttu-id="6f77a-122">Ez az eszköz kiváló akkor, ha Ön most használatának megkezdéséhez Azure, az adatraktár-adatbázisokat kis számú kezelése vagy kell tooquickly foglalkozhat.</span><span class="sxs-lookup"><span data-stu-id="6f77a-122">This tool is great is if you're just getting started with Azure, managing a small number of data warehouse databases, or need tooquickly do something.</span></span>

<span data-ttu-id="6f77a-123">az Azure-portálon hello lépései tooget lásd: [(Azure-portál) az SQL Data Warehouse létrehozása][Create a SQL Data Warehouse (Azure portal)].</span><span class="sxs-lookup"><span data-stu-id="6f77a-123">tooget started with hello Azure portal, see [Create a SQL Data Warehouse (Azure portal)][Create a SQL Data Warehouse (Azure portal)].</span></span>

### <a name="sql-server-data-tools-in-visual-studio"></a><span data-ttu-id="6f77a-124">A Visual Studio SQL Server Data Tools összetevővel</span><span class="sxs-lookup"><span data-stu-id="6f77a-124">SQL Server Data Tools in Visual Studio</span></span>
<span data-ttu-id="6f77a-125">[SQL Server Data Tools] [ SQL Server Data Tools] (SSDT) a Visual Studio lehetővé teszi a kezelése, és az adatbázis tooconnect.</span><span class="sxs-lookup"><span data-stu-id="6f77a-125">[SQL Server Data Tools][SQL Server Data Tools] (SSDT) in Visual Studio allows you tooconnect to, manage, and develop your databases.</span></span> <span data-ttu-id="6f77a-126">Ha ismeri a Visual Studio vagy más integrált fejlesztési környezetekben (IDEs) alkalmazásfejlesztő, próbálkozzon az SSDT a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="6f77a-126">If you're an application developer familiar with Visual Studio or other integrated development environments (IDEs), try using SSDT in Visual Studio.</span></span>

<span data-ttu-id="6f77a-127">SSDT magában foglalja az SQL Server Object Explorer programban, amely lehetővé teszi toovisualize hello csatlakozás és adatbázisok az SQL Data Warehouse parancsprogramok hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="6f77a-127">SSDT includes hello SQL Server Object Explorer which enables you toovisualize, connect, and execute scripts against SQL Data Warehouse databases.</span></span> <span data-ttu-id="6f77a-128">tooquickly csatlakozzon az adatraktár tooSQL, egyszerűen kattintson hello **Megnyitás Visual Studio** hello parancssávon, amikor hello adatbázis részletek megtekintése a klasszikus Azure portál hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="6f77a-128">tooquickly connect tooSQL Data Warehouse, you can simply click hello **Open in Visual Studio** button in hello command bar when viewing hello database details in hello Azure Classic Portal.</span></span>  

<span data-ttu-id="6f77a-129">a Visual Studióban az SSDT használatába tooget lásd: [lekérdezés Azure SQL Data Warehouse a Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="6f77a-129">tooget started with SSDT in Visual Studio, see [Query Azure SQL Data Warehouse with Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].</span></span>

### <a name="command-line-tools"></a><span data-ttu-id="6f77a-130">Parancssori eszközök</span><span class="sxs-lookup"><span data-stu-id="6f77a-130">Command-line tools</span></span>
<span data-ttu-id="6f77a-131">A parancssori eszközök alkalmasak a feladatok automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="6f77a-131">Command line tools are ideal for automating your workloads.</span></span>  <span data-ttu-id="6f77a-132">PowerShell és sqlcmd két nagyszerű lehetőséget tooautomate a folyamatok.</span><span class="sxs-lookup"><span data-stu-id="6f77a-132">PowerShell and sqlcmd are two great ways tooautomate your processes.</span></span>  <span data-ttu-id="6f77a-133">Azt javasoljuk, hogy ezek az eszközök nagy számú logikai kiszolgáló kezelése és központi telepítése éles környezetben az erőforrás-változása hello szükséges feladatok parancsprogrammal létrehozva, és majd automatikus.</span><span class="sxs-lookup"><span data-stu-id="6f77a-133">We recommend these tools for managing a large number of logical servers and deploying resource changes in a production environment as hello tasks necessary can be scripted and then automated.</span></span>

### <a name="dynamic-management-views"></a><span data-ttu-id="6f77a-134">Dinamikus felügyeleti nézetekkel</span><span class="sxs-lookup"><span data-stu-id="6f77a-134">Dynamic management views</span></span>
<span data-ttu-id="6f77a-135">Dinamikus felügyeleti nézetek hello be és vaj felügyelete az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6f77a-135">DMVs are hello bread and butter of managing SQL Data Warehouse.</span></span> <span data-ttu-id="6f77a-136">Szinte minden olyan információt, amely felfedi a hello portálon dinamikus felügyeleti nézetek támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="6f77a-136">Almost all information that surfaces in hello portal relies on DMVs.</span></span> <span data-ttu-id="6f77a-137">toosee SQL Data Warehouse dinamikus felügyeleti nézetek, listáját lásd: [SQL Data Warehouse rendszernézetek][SQL Data Warehouse system views].</span><span class="sxs-lookup"><span data-stu-id="6f77a-137">toosee a list of SQL Data Warehouse DMVs, see [SQL Data Warehouse system views][SQL Data Warehouse system views].</span></span>

<span data-ttu-id="6f77a-138">tooget indult el, lásd: [kapcsolódás és lekérdezés az Sqlcmd][Connect and query with sqlcmd], és [hozzon létre egy adatbázist (PowerShell)][Create a database (PowerShell)].</span><span class="sxs-lookup"><span data-stu-id="6f77a-138">tooget started, see [Connect and query with sqlcmd][Connect and query with sqlcmd], and [Create a database (PowerShell)][Create a database (PowerShell)].</span></span>

## <a name="scale-compute"></a><span data-ttu-id="6f77a-139">Skála számítási</span><span class="sxs-lookup"><span data-stu-id="6f77a-139">Scale compute</span></span>
<span data-ttu-id="6f77a-140">Az SQL Data Warehouse teljesítményét, vagy vissza bővítése vagy csökkentése a számítási erőforrásokat Processzor, memória és i/o műveletek sávszélességétől gyors vertikális.</span><span class="sxs-lookup"><span data-stu-id="6f77a-140">In SQL Data Warehouse, you can quickly scale performance out or back by increasing or decreasing compute resources of CPU, memory, and I/O bandwidth.</span></span> <span data-ttu-id="6f77a-141">tooscale teljesítmény toodo szüksége adattárházegységek (dwu-k), hogy az SQL Data Warehouse foglal le, mely tooyour adatbázis hello számának beállítása.</span><span class="sxs-lookup"><span data-stu-id="6f77a-141">tooscale performance, all you need toodo is adjust hello number of data warehouse units (DWUs) that SQL Data Warehouse allocates tooyour database.</span></span> <span data-ttu-id="6f77a-142">Az SQL Data Warehouse gyorsan hello módosítása lehetővé teszi, és kezeli az összes hello alapul szolgáló módosítások toohardware vagy szoftver.</span><span class="sxs-lookup"><span data-stu-id="6f77a-142">SQL Data Warehouse quickly makes hello change and handles all hello underlying changes toohardware or software.</span></span>

<span data-ttu-id="6f77a-143">toolearn dwu-k, skálázás kapcsolatos további információkért lásd: [méretezhető teljesítmény].</span><span class="sxs-lookup"><span data-stu-id="6f77a-143">toolearn more about scaling DWUs, see [Scale performance].</span></span>

## <a name="pause-and-resume"></a><span data-ttu-id="6f77a-144">Szünet és folytatás</span><span class="sxs-lookup"><span data-stu-id="6f77a-144">Pause and resume</span></span>
<span data-ttu-id="6f77a-145">toosave költségek, is szüneteltetése és folytatása számítási erőforrások igény.</span><span class="sxs-lookup"><span data-stu-id="6f77a-145">toosave costs, you can pause and resume compute resources on-demand.</span></span> <span data-ttu-id="6f77a-146">Például ha nem használ hello adatbázis hello éjszakai során, és a hétvégekre, után ilyen alkalmakkor szünetelteti, is fogják folytatni azt hello nap alatt.</span><span class="sxs-lookup"><span data-stu-id="6f77a-146">For example, if you won't be using hello database during hello night and on weekends, you can pause it during those times, and resume it during hello day.</span></span> <span data-ttu-id="6f77a-147">Ön nem számlázni dwu-k közben hello adatbázis fel van függesztve.</span><span class="sxs-lookup"><span data-stu-id="6f77a-147">You won't be charged for DWUs while hello database is paused.</span></span>

<span data-ttu-id="6f77a-148">További információkért lásd: [számítási szüneteltetése][Pause compute], és [folytathatja a számítást][Resume compute].</span><span class="sxs-lookup"><span data-stu-id="6f77a-148">For more information, see [Pause compute][Pause compute], and [Resume compute][Resume compute].</span></span>

## <a name="performance-best-practices"></a><span data-ttu-id="6f77a-149">Teljesítmény gyakorlati tanácsok</span><span class="sxs-lookup"><span data-stu-id="6f77a-149">Performance Best Practices</span></span>
<span data-ttu-id="6f77a-150">Ha az első lépések egy új technológiával, hello tippek és legjobb jobb hello indítás működő trükkök felderítéséhez takaríthat meg rengeteg időt.</span><span class="sxs-lookup"><span data-stu-id="6f77a-150">When getting started with a new technology, discovering hello tips and tricks that work best right from hello start can save you lots of time.</span></span>  <span data-ttu-id="6f77a-151">Gyakorlati tanácsok a témakörök számos teljes találja.</span><span class="sxs-lookup"><span data-stu-id="6f77a-151">You will find best practices throughout many of our topics.</span></span>

<span data-ttu-id="6f77a-152">toosee hello egyik legfontosabb szempontja a számítási feladathoz fejleszt sok összefoglalását lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="6f77a-152">toosee many a summary of hello most important considerations when developing your workload, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

## <a name="query-monitoring"></a><span data-ttu-id="6f77a-153">Lekérdezés figyelése</span><span class="sxs-lookup"><span data-stu-id="6f77a-153">Query Monitoring</span></span>
<span data-ttu-id="6f77a-154">Néha egy lekérdezése túl hosszú, de még nem meg arról, hogy melyik hello hibát okozó van.</span><span class="sxs-lookup"><span data-stu-id="6f77a-154">Sometimes a query is running too long, but you aren't sure of which one is hello culprit.</span></span> <span data-ttu-id="6f77a-155">Az SQL Data Warehouse (dinamikus felügyeleti nézetek) használható toofigure dinamikus felügyeleti nézetekkel rendelkezik, ahonnan a lekérdezés túl sokáig tart.</span><span class="sxs-lookup"><span data-stu-id="6f77a-155">SQL Data Warehouse has dynamic management views (DMVs) that you can use toofigure out which query is taking too long.</span></span>

<span data-ttu-id="6f77a-156">toofind hosszan futó lekérdezések bővebb ismertetése a [a dinamikus felügyeleti nézetek használatával számítási feladat figyeléséhez][Monitor your workload using DMVs].</span><span class="sxs-lookup"><span data-stu-id="6f77a-156">toofind long-running queries, see [Monitor your workload using DMVs][Monitor your workload using DMVs].</span></span>

## <a name="security"></a><span data-ttu-id="6f77a-157">Biztonság</span><span class="sxs-lookup"><span data-stu-id="6f77a-157">Security</span></span>
<span data-ttu-id="6f77a-158">toomaintain biztonságos rendszer, kell lenniük hello figyelmeztető riasztás, és bármilyen típusú jogosulatlan hozzáférés elleni védelmet.</span><span class="sxs-lookup"><span data-stu-id="6f77a-158">toomaintain a secure system, you must be on hello alert and guard against any type of unauthorized access.</span></span> <span data-ttu-id="6f77a-159">A biztonsági rendszer toomake, a tűzfalszabályok teljesülnek, csak a jogosult IP-címek képesek-e csatlakozni kell.</span><span class="sxs-lookup"><span data-stu-id="6f77a-159">A security system needs toomake sure firewall rules are in place so only authorized IP addresses can connect.</span></span> <span data-ttu-id="6f77a-160">A felhasználói hitelesítő adatok a megfelelő hitelesítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="6f77a-160">It needs proper authentication of user credentials.</span></span> <span data-ttu-id="6f77a-161">Miután egy felhasználó kapcsolódott toohello adatbázis, hello felhasználói csak kell engedélyek tooperform műveletek minimális száma.</span><span class="sxs-lookup"><span data-stu-id="6f77a-161">After a user has connected toohello database, hello user should only have permissions tooperform a minimal number of actions.</span></span> <span data-ttu-id="6f77a-162">toosecure adatokat, a titkosítási használhatja.</span><span class="sxs-lookup"><span data-stu-id="6f77a-162">toosecure data, you can use encryption.</span></span> <span data-ttu-id="6f77a-163">Akkor is vizsgálati és nyomkövetési eseményeket is végig, ha a gyanús tevékenységeket, fontos toohave.</span><span class="sxs-lookup"><span data-stu-id="6f77a-163">It's also important toohave auditing and tracking so you can retrace events if there is any suspicious activity.</span></span>

<span data-ttu-id="6f77a-164">biztonsági okokból toohello keresztül head kezelésével kapcsolatos toolearn [biztonsági áttekintése][Security overview].</span><span class="sxs-lookup"><span data-stu-id="6f77a-164">toolearn about managing security, head over toohello [Security overview][Security overview].</span></span>

## <a name="backup-and-restore"></a><span data-ttu-id="6f77a-165">Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="6f77a-165">Backup and restore</span></span>
<span data-ttu-id="6f77a-166">Az adatok megbízható backps, akkor minden éles adatbázis nagyon fontos részét képezik.</span><span class="sxs-lookup"><span data-stu-id="6f77a-166">Having reliable backps of your data is an essential part of any production database.</span></span> <span data-ttu-id="6f77a-167">Az SQL Data Warehouse biztosítja az adatok biztonságos automatikusan biztonsági mentést kell készíteni a aktív adatbázisainak rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="6f77a-167">SQL Data Warehouse keeps your data safe by automatically backing up your active databases at regular intervals.</span></span> <span data-ttu-id="6f77a-168">Ezek a biztonsági másolatok teszik lehetővé a forgatókönyvekben, ahol korábban az adatok sérült, vagy véletlenül a az adatok vagy az adatbázis eldobása toorecover.</span><span class="sxs-lookup"><span data-stu-id="6f77a-168">These backups allow you toorecover from scenarios where you've corrupted your data or accidentally dropped your data or database.</span></span>  <span data-ttu-id="6f77a-169">Az adatmegőrzési hello adatok biztonsági mentés ütemezése és hogyan toorestore, adatbázis: [a pillanatkép-visszaállítás][Restore from snapshot].</span><span class="sxs-lookup"><span data-stu-id="6f77a-169">For hello data backup schedule, retention policy and how toorestore a database, see [Restore from snapshot][Restore from snapshot].</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f77a-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6f77a-170">Next steps</span></span>
<span data-ttu-id="6f77a-171">Jó adatbázis tervezési alapelvek segítségével teszi egyszerűbbé toomanage az SQL Data Warehouse az adatbázisokat.</span><span class="sxs-lookup"><span data-stu-id="6f77a-171">Using good database design principles will make it easier toomanage your databases in SQL Data Warehouse.</span></span> <span data-ttu-id="6f77a-172">További, toolearn keresztül toohello head [fejlesztői áttekintés][Development overview].</span><span class="sxs-lookup"><span data-stu-id="6f77a-172">toolearn more, head over toohello [Development overview][Development overview].</span></span>

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Create a database (PowerShell)]: sql-data-warehouse-get-started-provision-powershell.md
[connection]: sql-data-warehouse-develop-connections.md
[Query Azure SQL Data Warehouse with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Connect and query with sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Development overview]: sql-data-warehouse-overview-develop.md
[Monitor your workload using DMVs]: sql-data-warehouse-manage-monitor.md
[Pause compute]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Restore from snapshot]: sql-data-warehouse-restore-database-overview.md
[Resume compute]: sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[méretezhető teljesítmény]: sql-data-warehouse-manage-compute-overview.md#scale-compute
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SQL Data Warehouse Best Practices]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse system views]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
