---
title: "aaaGetting (előzetes verzió) Azure SQL adatszinkronizálás használatába |} Microsoft Docs"
description: "Ez az oktatóanyag segítséget nyújt a Ismerkedés az Azure SQL adatszinkronizálás (előzetes verzió)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: a295a768-7ff2-4a86-a253-0090281c8efa
ms.service: sql-database
ms.custom: load & move data
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: douglasl
ms.openlocfilehash: 666d09237e42acc23ae3c8c81e60734a413f5949
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-sql-data-sync-preview"></a><span data-ttu-id="2a68f-103">Ismerkedés az Azure SQL adatszinkronizálás (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="2a68f-103">Getting Started with Azure SQL Data Sync (Preview)</span></span>
<span data-ttu-id="2a68f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan tooset be Azure SQL adatszinkronizálás hozzon létre egy hibrid szinkronizálású csoport, amely tartalmazza az Azure SQL Database és az SQL Server-példány.</span><span class="sxs-lookup"><span data-stu-id="2a68f-104">In this tutorial, you learn how tooset up Azure SQL Data Sync by creating a hybrid sync group that contains both Azure SQL Database and SQL Server instances.</span></span> <span data-ttu-id="2a68f-105">hello új szinkronizálású csoport teljes van beállítva, és a beállított ütemezés hello szinkronizálja.</span><span class="sxs-lookup"><span data-stu-id="2a68f-105">hello new sync group is fully configured and synchronizes on hello schedule you set.</span></span>

<span data-ttu-id="2a68f-106">Ez az oktatóanyag feltételezi, hogy rendelkezik-e legalább némi tapasztalattal az SQL Database és SQL-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="2a68f-106">This tutorial assumes that you have at least some prior experience with SQL Database and with SQL Server.</span></span> 

<span data-ttu-id="2a68f-107">SQL adatszinkronizálás áttekintését lásd: [szinkronizálni az adatokat](sql-database-sync-data.md).</span><span class="sxs-lookup"><span data-stu-id="2a68f-107">For an overview of SQL Data Sync, see [Sync data](sql-database-sync-data.md).</span></span>

<span data-ttu-id="2a68f-108">A teljes PowerShell példák hogyan tooconfigure SQL adatszinkronizálás, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="2a68f-108">For complete PowerShell examples that show how tooconfigure SQL Data Sync, see hello following articles:</span></span>
-   [<span data-ttu-id="2a68f-109">PowerShell toosync több Azure SQL-adatbázist használja</span><span class="sxs-lookup"><span data-stu-id="2a68f-109">Use PowerShell toosync between multiple Azure SQL databases</span></span>](scripts/sql-database-sync-data-between-sql-databases.md)
-   [<span data-ttu-id="2a68f-110">Egy Azure SQL-adatbázis és a helyszíni SQL Server-adatbázisok közötti PowerShell toosync használata</span><span class="sxs-lookup"><span data-stu-id="2a68f-110">Use PowerShell toosync between an Azure SQL Database and a SQL Server on-premises database</span></span>](scripts/sql-database-sync-data-between-azure-onprem.md)

> [!NOTE]
> <span data-ttu-id="2a68f-111">hello teljes műszaki dokumentáció az Azure SQL Data szinkronizálás, korábban, az MSDN-en, állítsa be áll rendelkezésre, mert egy. PDF-dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="2a68f-111">hello complete technical documentation set for Azure SQL Data Sync, formerly located on MSDN, is available as a .PDF document.</span></span> <span data-ttu-id="2a68f-112">Töltse le [Itt](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).</span><span class="sxs-lookup"><span data-stu-id="2a68f-112">Download it [here](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).</span></span>

## <a name="step-1---create-sync-group"></a><span data-ttu-id="2a68f-113">1. lépés – a sync-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="2a68f-113">Step 1 - Create sync group</span></span>

### <a name="locate-hello-data-sync-settings"></a><span data-ttu-id="2a68f-114">Keresse meg a beállítások hello adatok szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="2a68f-114">Locate hello Data Sync settings</span></span>

1.  <span data-ttu-id="2a68f-115">A böngészőben nyissa meg a toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2a68f-115">In your browser, navigate toohello Azure portal.</span></span>

2.  <span data-ttu-id="2a68f-116">Hello portal keresse meg az SQL-adatbázisok az irányítópultra, illetve hello SQL-adatbázisok ikon hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="2a68f-116">In hello portal, locate your SQL databases from your Dashboard or from hello SQL Databases icon on hello toolbar.</span></span>

    ![Azure SQL-adatbázisok listája](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  <span data-ttu-id="2a68f-118">A hello **SQL-adatbázisok** panelen, jelölje be hello meglévő SQL-adatbázis, amelyet toouse, hello hub adatbázis az adatok szinkronizálása. hello SQL adatbázis panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2a68f-118">On hello **SQL databases** blade, select hello existing SQL database that you want toouse as hello hub database for Data Sync. hello SQL database blade opens.</span></span>

4.  <span data-ttu-id="2a68f-119">Hello SQL adatbázis paneljén hello kijelölt adatbázis kiválasztása **tooother adatbázisok szinkronizálása**.</span><span class="sxs-lookup"><span data-stu-id="2a68f-119">On hello SQL database blade for hello selected database, select **Sync tooother databases**.</span></span> <span data-ttu-id="2a68f-120">hello adatszinkronizálás panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2a68f-120">hello Data Sync blade opens.</span></span>

    ![Szinkronizálási tooother adatbázisok lehetősége](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a><span data-ttu-id="2a68f-122">Hozzon létre egy új szinkronizálási csoportot</span><span class="sxs-lookup"><span data-stu-id="2a68f-122">Create a new Sync Group</span></span>

1.  <span data-ttu-id="2a68f-123">Hello adatszinkronizálás paneljén válassza **új szinkronizálású csoport**.</span><span class="sxs-lookup"><span data-stu-id="2a68f-123">On hello Data Sync blade, select **New Sync Group**.</span></span> <span data-ttu-id="2a68f-124">Hello **új szinkronizálású csoport** panel nyílik meg az 1. lépésben, **szinkronizálási csoport létrehozása**, kijelölt.</span><span class="sxs-lookup"><span data-stu-id="2a68f-124">hello **New sync group** blade opens with Step 1, **Create sync group**, highlighted.</span></span> <span data-ttu-id="2a68f-125">Hello **adatok szinkronizálása csoport létrehozása** panel is megnyílik.</span><span class="sxs-lookup"><span data-stu-id="2a68f-125">hello **Create Data Sync Group** blade also opens.</span></span>

2.  <span data-ttu-id="2a68f-126">A hello **adatok szinkronizálása csoport létrehozása** panelen dolgok következő hello:</span><span class="sxs-lookup"><span data-stu-id="2a68f-126">On hello **Create Data Sync Group** blade, do hello following things:</span></span>

    1.  <span data-ttu-id="2a68f-127">A hello **szinkronizálási csoportnév** mezőbe írja be a hello új szinkronizálási csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="2a68f-127">In hello **Sync Group Name** field, enter a name for hello new sync group.</span></span>

    2.  <span data-ttu-id="2a68f-128">A hello **szinkronizálási metaadatokat tároló adatbázis** területen válassza ki, hogy toocreate egy új adatbázist (ajánlott) vagy toouse egy meglévő adatbázist.</span><span class="sxs-lookup"><span data-stu-id="2a68f-128">In hello **Sync Metadata Database** section, choose whether toocreate a new database (recommended) or toouse an existing database.</span></span>

        > [!NOTE]
        > <span data-ttu-id="2a68f-129">A Microsoft azt javasolja, hogy hozzon létre egy új, üres adatbázis toouse, hello szinkronizálási metaadatokat tároló adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2a68f-129">Microsoft recommends that you create a new, empty database toouse as hello Sync Metadata Database.</span></span> <span data-ttu-id="2a68f-130">Adatszinkronizálás hoz létre táblák az adatbázisban, és a gyakori munkaterhelés futtatja.</span><span class="sxs-lookup"><span data-stu-id="2a68f-130">Data Sync creates tables in this database and runs a frequent workload.</span></span> <span data-ttu-id="2a68f-131">Ez az adatbázis automatikusan megosztva hello szinkronizálási metaadatokat tároló adatbázis az összes, a szinkronizálási csoportok hello a kiválasztott régióban.</span><span class="sxs-lookup"><span data-stu-id="2a68f-131">This database is automatically shared as hello Sync Metadata Database for all of your Sync groups in hello selected region.</span></span> <span data-ttu-id="2a68f-132">Hello szinkronizálási metaadatokat tároló adatbázis, a neve vagy a szolgáltatási szint nélkül eldobása nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="2a68f-132">You can't change hello Sync Metadata Database, its name, or its service level without dropping it.</span></span>

        <span data-ttu-id="2a68f-133">Ha úgy döntött, hogy **új adatbázis**, jelölje be **új adatbázis létrehozása.**</span><span class="sxs-lookup"><span data-stu-id="2a68f-133">If you chose **New database**, select **Create new database.**</span></span> <span data-ttu-id="2a68f-134">Hello **SQL-adatbázis** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2a68f-134">hello **SQL Database** blade opens.</span></span> <span data-ttu-id="2a68f-135">A hello **SQL-adatbázis** panelen nevet, és konfigurálja a hello új adatbázist.</span><span class="sxs-lookup"><span data-stu-id="2a68f-135">On hello **SQL Database** blade, name and configure hello new database.</span></span> <span data-ttu-id="2a68f-136">Válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a68f-136">Then select **OK**.</span></span>

        <span data-ttu-id="2a68f-137">Ha úgy döntött, hogy **létező adatbázis használata**, jelölje be hello adatbázis hello listából.</span><span class="sxs-lookup"><span data-stu-id="2a68f-137">If you chose **Use existing database**, select hello database from hello list.</span></span>

    3.  <span data-ttu-id="2a68f-138">A hello **automatikus szinkronizálás** szakaszban, először válassza **a** vagy **ki**.</span><span class="sxs-lookup"><span data-stu-id="2a68f-138">In hello **Automatic Sync** section, first select **On** or **Off**.</span></span>

        <span data-ttu-id="2a68f-139">Ha úgy döntött, hogy **a**, a hello **szinkronizálási gyakoriság** szakaszt, adjon meg egy számot, és válassza ki a másodperc, perc, órák vagy napok.</span><span class="sxs-lookup"><span data-stu-id="2a68f-139">If you chose **On**, in hello **Sync Frequency** section, enter a number and select Seconds, Minutes, Hours, or Days.</span></span>

        ![Adja meg a szinkronizálás gyakorisága](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  <span data-ttu-id="2a68f-141">A hello **ütközésének feloldása** területen válassza ki a "Hub wins" vagy "Tag wins."</span><span class="sxs-lookup"><span data-stu-id="2a68f-141">In hello **Conflict Resolution** section, select "Hub wins" or "Member wins."</span></span>

        ![Adja meg az ütközések feloldása](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  <span data-ttu-id="2a68f-143">Válassza ki **OK** és várja meg, hello új szinkronizálási csoport toobe létrehozása és telepítése.</span><span class="sxs-lookup"><span data-stu-id="2a68f-143">Select **OK** and wait for hello new sync group toobe created and deployed.</span></span>

## <a name="step-2---add-sync-members"></a><span data-ttu-id="2a68f-144">2. lépés – szinkronizálás tagok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2a68f-144">Step 2 - Add sync members</span></span>

<span data-ttu-id="2a68f-145">Miután hello új szinkronizálású csoport létrehozása és telepítése, 2. lépés **szinkronizálási tagok hozzáadása**, hello kijelölt **új szinkronizálású csoport** panelen.</span><span class="sxs-lookup"><span data-stu-id="2a68f-145">After hello new sync group is created and deployed, Step 2, **Add sync members**, is highlighted in hello **New sync group** blade.</span></span>

<span data-ttu-id="2a68f-146">A hello **Hub adatbázis** területen adja meg a meglévő hitelesítő hello hello SQL adatbázis-kiszolgáló a mely hello hub adatbázisa található.</span><span class="sxs-lookup"><span data-stu-id="2a68f-146">In hello **Hub Database** section, enter hello existing credentials for hello SQL Database server on which hello hub database is located.</span></span> <span data-ttu-id="2a68f-147">Ne adjon meg *új* ebben a szakaszban a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="2a68f-147">Don't enter *new* credentials in this section.</span></span>

![Központi adatbázis toosync csoport hozzá lett adva](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

## <a name="add-an-azure-sql-database"></a><span data-ttu-id="2a68f-149">Egy Azure SQL adatbázis hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2a68f-149">Add an Azure SQL Database</span></span>

<span data-ttu-id="2a68f-150">A hello **Tagadatbázis** szakaszban, és opcionálisan adja hozzá az Azure SQL Database toohello szinkronizálású csoport kiválasztásával **hozzáadása egy Azure-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="2a68f-150">In hello **Member Database** section, optionally add an Azure SQL Database toohello sync group by selecting **Add an Azure Database**.</span></span> <span data-ttu-id="2a68f-151">Hello **Azure-adatbázis beállítása** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2a68f-151">hello **Configure Azure Database** blade opens.</span></span>

<span data-ttu-id="2a68f-152">A hello **Azure-adatbázis beállítása** panelen dolgok következő hello:</span><span class="sxs-lookup"><span data-stu-id="2a68f-152">On hello **Configure Azure Database** blade, do hello following things:</span></span>

1.  <span data-ttu-id="2a68f-153">A hello **szinkronizálási tagneve** mezőbe, adjon meg egy nevet hello új szinkronizálási tag.</span><span class="sxs-lookup"><span data-stu-id="2a68f-153">In hello **Sync Member Name** field, provide a name for hello new sync member.</span></span> <span data-ttu-id="2a68f-154">Ez a név nem azonos azzal hello maga hello adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="2a68f-154">This name is distinct from hello name of hello database itself.</span></span>

2.  <span data-ttu-id="2a68f-155">A hello **előfizetés** mezőjét, jelölje be hello társított Azure-előfizetés más célra.</span><span class="sxs-lookup"><span data-stu-id="2a68f-155">In hello **Subscription** field, select hello associated Azure subscription for billing purposes.</span></span>

3.  <span data-ttu-id="2a68f-156">A hello **Azure SQL Server** mezőjét, jelölje be hello meglévő SQL adatbázis-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="2a68f-156">In hello **Azure SQL Server** field, select hello existing SQL database server.</span></span>

4.  <span data-ttu-id="2a68f-157">A hello **Azure SQL Database** mezőjét, jelölje be hello meglévő SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2a68f-157">In hello **Azure SQL Database** field, select hello existing SQL database.</span></span>

5.  <span data-ttu-id="2a68f-158">A hello **szinkronizálási irányban** mező mellett válassza ki a kétirányú szinkronizálás, toohello Hub, vagy a központ hello.</span><span class="sxs-lookup"><span data-stu-id="2a68f-158">In hello **Sync Directions** field, select Bi-directional Sync, toohello Hub, or From hello Hub.</span></span>

    ![Új SQL-adatbázis szinkronizálási tag hozzáadása](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  <span data-ttu-id="2a68f-160">A hello **felhasználónév** és **jelszó** mezők, adja meg a hello meglévő hitelesítő adatait, mely hello tagadatbázis helyezkedik hello SQL adatbázis-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="2a68f-160">In hello **Username** and **Password** fields, enter hello existing credentials for hello SQL Database server on which hello member database is located.</span></span> <span data-ttu-id="2a68f-161">Ne adjon meg *új* ebben a szakaszban a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="2a68f-161">Don't enter *new* credentials in this section.</span></span>

7.  <span data-ttu-id="2a68f-162">Válassza ki **OK** és várja meg, hello új szinkronizálási tag toobe létrehozása és telepítése.</span><span class="sxs-lookup"><span data-stu-id="2a68f-162">Select **OK** and wait for hello new sync member toobe created and deployed.</span></span>

    ![Új SQL-adatbázis szinkronizálási tag hozzá lett adva.](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

## <a name="add-an-on-premises-sql-server-database"></a><span data-ttu-id="2a68f-164">A helyszíni SQL Server adatbázis hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2a68f-164">Add an on-premises SQL Server database</span></span>

<span data-ttu-id="2a68f-165">A hello **Tagadatbázis** szakaszban, és opcionálisan adja hozzá egy helyi SQL Server toohello szinkronizálású csoport kiválasztásával **adja hozzá az On-Premises adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="2a68f-165">In hello **Member Database** section, optionally add an on-premises SQL Server toohello sync group by selecting **Add an On-Premises Database**.</span></span> <span data-ttu-id="2a68f-166">Hello **konfigurálása a helyszíni** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2a68f-166">hello **Configure On-Premises** blade opens.</span></span>

<span data-ttu-id="2a68f-167">A hello **konfigurálása a helyszíni** panelen dolgok következő hello:</span><span class="sxs-lookup"><span data-stu-id="2a68f-167">On hello **Configure On-Premises** blade, do hello following things:</span></span>

1.  <span data-ttu-id="2a68f-168">Válassza ki **válasszon hello szinkronizálási ügynök átjáró**.</span><span class="sxs-lookup"><span data-stu-id="2a68f-168">Select **Choose hello Sync Agent Gateway**.</span></span> <span data-ttu-id="2a68f-169">Hello **válassza ki a szinkronizálási ügynök** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2a68f-169">hello **Select Sync Agent** blade opens.</span></span>

    ![Hello szinkronizálási ügynök átjáró kiválasztása](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  <span data-ttu-id="2a68f-171">A hello **válasszon hello szinkronizálási ügynök átjáró** panelen válassza ki e toouse egy meglévő ügynököt, vagy hozzon létre egy új ügynököt.</span><span class="sxs-lookup"><span data-stu-id="2a68f-171">On hello **Choose hello Sync Agent Gateway** blade, choose whether toouse an existing agent or create a new agent.</span></span>

    <span data-ttu-id="2a68f-172">Ha úgy döntött, hogy **meglévő ügynökök**, válassza ki a meglévő ügynököt hello listából hello.</span><span class="sxs-lookup"><span data-stu-id="2a68f-172">If you chose **Existing agents**, select hello existing agent from hello list.</span></span>

    <span data-ttu-id="2a68f-173">Ha úgy döntött, hogy **hozzon létre egy új ügynök**, dolgok következő hello:</span><span class="sxs-lookup"><span data-stu-id="2a68f-173">If you chose **Create a new agent**, do hello following things:</span></span>

    1.  <span data-ttu-id="2a68f-174">A megadott hivatkozás hello ügynök szinkronizálási hello ügyfélszoftver letöltése, és telepítse azt hello számítógép hello SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2a68f-174">Download hello client sync agent software from hello link provided and install it on hello computer where hello SQL Server is located.</span></span>
 
        > [!IMPORTANT]
        > <span data-ttu-id="2a68f-175">Hogy tooopen kimenő TCP 1433-as port a hello tűzfal toolet hello ügyfélügynök hello kiszolgálóval való kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="2a68f-175">You have tooopen outbound TCP port 1433 in hello firewall toolet hello client agent communicate with hello server.</span></span>


    2.  <span data-ttu-id="2a68f-176">Adjon meg egy nevet hello ügynök.</span><span class="sxs-lookup"><span data-stu-id="2a68f-176">Enter a name for hello agent.</span></span>

    3.  <span data-ttu-id="2a68f-177">Válassza ki **létrehozása és a kulcs létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="2a68f-177">Select **Create and Generate Key**.</span></span>

    4.  <span data-ttu-id="2a68f-178">Hello ügynök kulcs toohello vágólapra másolása.</span><span class="sxs-lookup"><span data-stu-id="2a68f-178">Copy hello agent key toohello clipboard.</span></span>
        
        ![Új szinkronizálási ügynök létrehozása](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  <span data-ttu-id="2a68f-180">Válassza ki **OK** tooclose hello **válassza ki a szinkronizálási ügynök** panelen.</span><span class="sxs-lookup"><span data-stu-id="2a68f-180">Select **OK** tooclose hello **Select Sync Agent** blade.</span></span>

    6.  <span data-ttu-id="2a68f-181">Hello SQL Server-számítógépen keresse meg és hello szinkronizálási Ügyfélügynök-alkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="2a68f-181">On hello SQL Server computer, locate and run hello Client Sync Agent app.</span></span>

        ![hello adatok szinkronizálása az ügyfélalkalmazás ügynök](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  <span data-ttu-id="2a68f-183">Hello szinkronizálási ügynök alkalmazásban, válassza ki a **nyújt ügynök kulcs**.</span><span class="sxs-lookup"><span data-stu-id="2a68f-183">In hello sync agent app, select **Submit Agent Key**.</span></span> <span data-ttu-id="2a68f-184">Hello **szinkronizálási metaadatok adatbázis konfigurációja** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2a68f-184">hello **Sync Metadata Database Configuration** dialog box opens.</span></span>

    8.  <span data-ttu-id="2a68f-185">A hello **szinkronizálási metaadatok adatbázis konfigurációja** párbeszédpanelen illessze be hello ügynök kulcs átmásolva hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2a68f-185">In hello **Sync Metadata Database Configuration** dialog box, paste in hello agent key copied from hello Azure portal.</span></span> <span data-ttu-id="2a68f-186">Emellett azokat a hello hello Azure SQL Database kiszolgálón, mely hello a metaadatokat tároló adatbázis található meglévő hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="2a68f-186">Also provide hello existing credentials for hello Azure SQL Database server on which hello metadata database is located.</span></span> <span data-ttu-id="2a68f-187">(Ha létrehozott egy új metaadatokat tároló adatbázis, az adatbázis nem a hello hello hub adatbázis ugyanazon a kiszolgálón.) Válassza ki **OK** és hello konfigurációs toofinish várja.</span><span class="sxs-lookup"><span data-stu-id="2a68f-187">(If you created a new metadata database, this database is on hello same server as hello hub database.) Select **OK** and wait for hello configuration toofinish.</span></span>

        ![Hello ügynök kulcsot és a kiszolgáló hitelesítő adatok megadása](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   <span data-ttu-id="2a68f-189">Ha a tűzfal hibaüzenet jelenik meg ezen a ponton, hogy toocreate tűzfalszabály Azure tooallow érkező forgalmat hello SQL Server-számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2a68f-189">If you get a firewall error at this point, you have toocreate a firewall rule on Azure tooallow incoming traffic from hello SQL Server computer.</span></span> <span data-ttu-id="2a68f-190">Hello szabály manuálisan is létrehozhat, hello portálon, de azt tapasztalhatja, könnyebben toocreate azt az SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="2a68f-190">You can create hello rule manually in hello portal, but you may find it easier toocreate it in SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="2a68f-191">A szolgáltatáshoz az SSMS próbálja ki a tooconnect toohello hub adatbázis Azure-on.</span><span class="sxs-lookup"><span data-stu-id="2a68f-191">In SSMS, try tooconnect toohello hub database on Azure.</span></span> <span data-ttu-id="2a68f-192">Adja meg a nevét, \<hub_database_name\>. database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="2a68f-192">Enter its name as \<hub_database_name\>.database.windows.net.</span></span> <span data-ttu-id="2a68f-193">Kövesse hello hello párbeszédpanel bezárásához tooconfigure hello Azure tűzfalszabály.</span><span class="sxs-lookup"><span data-stu-id="2a68f-193">Follow hello steps in hello dialog box tooconfigure hello Azure firewall rule.</span></span> <span data-ttu-id="2a68f-194">Térjen vissza toohello szinkronizálási Ügyfélügynök alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2a68f-194">Then return toohello Client Sync Agent app.</span></span>

    9.  <span data-ttu-id="2a68f-195">Hello szinkronizálási Ügyfélügynök alkalmazásban kattintson **regisztrálása** tooregister hello ügynököt az SQL Server-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2a68f-195">In hello Client Sync Agent app, click **Register** tooregister a SQL Server database with hello agent.</span></span> <span data-ttu-id="2a68f-196">Hello **SQL Server-konfigurációs** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2a68f-196">hello **SQL Server Configuration** dialog box opens.</span></span>

        ![Hozzáadása és az SQL Server-adatbázisok konfigurálása](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. <span data-ttu-id="2a68f-198">A hello **SQL Server-konfigurációs** párbeszédpanelen válassza ki azt hogy tooconnect SQL Server-hitelesítést vagy a Windows-hitelesítés használatával.</span><span class="sxs-lookup"><span data-stu-id="2a68f-198">In hello **SQL Server Configuration** dialog box, choose whether tooconnect by using SQL Server authentication or Windows authentication.</span></span> <span data-ttu-id="2a68f-199">Ha úgy dönt, hogy az SQL Server-hitelesítést, írja be a hello meglévő hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="2a68f-199">If you chose SQL Server authentication, enter hello existing credentials.</span></span> <span data-ttu-id="2a68f-200">Adja meg hello SQL Server nevét és hello hello adatbázis nevét, amelyet az toosync.</span><span class="sxs-lookup"><span data-stu-id="2a68f-200">Provide hello SQL Server name and hello name of hello database that you want toosync.</span></span> <span data-ttu-id="2a68f-201">Válassza ki **tesztkapcsolat** tootest a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="2a68f-201">Select **Test connection** tootest your settings.</span></span> <span data-ttu-id="2a68f-202">Válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2a68f-202">Then select **Save**.</span></span> <span data-ttu-id="2a68f-203">hello regisztrált adatbázis hello listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2a68f-203">hello registered database appears in hello list.</span></span>

        ![SQL Server-adatbázis most már regisztrálva van](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. <span data-ttu-id="2a68f-205">Most már bezárhatja hello szinkronizálási Ügyfélügynök alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2a68f-205">You can now close hello Client Sync Agent app.</span></span>

    12. <span data-ttu-id="2a68f-206">A hello hello portálról **konfigurálása a helyszíni** panelen válassza **válasszon hello adatbázis.**</span><span class="sxs-lookup"><span data-stu-id="2a68f-206">In hello portal, on hello **Configure On-Premises** blade, select **Select hello Database.**</span></span> <span data-ttu-id="2a68f-207">Hello **adatbázis kijelölése** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2a68f-207">hello **Select Database** blade opens.</span></span>

    13. <span data-ttu-id="2a68f-208">A hello **adatbázis kijelölése** paneljén, hello **szinkronizálási tagneve** mezőbe, adjon meg egy nevet hello új szinkronizálási tag.</span><span class="sxs-lookup"><span data-stu-id="2a68f-208">On hello **Select Database** blade, in hello **Sync Member Name** field, provide a name for hello new sync member.</span></span> <span data-ttu-id="2a68f-209">Ez a név nem azonos azzal hello maga hello adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="2a68f-209">This name is distinct from hello name of hello database itself.</span></span> <span data-ttu-id="2a68f-210">Válassza ki a hello adatbázis hello listából.</span><span class="sxs-lookup"><span data-stu-id="2a68f-210">Select hello database from hello list.</span></span> <span data-ttu-id="2a68f-211">A hello **szinkronizálási irányban** mező mellett válassza ki a kétirányú szinkronizálás, toohello Hub, vagy a központ hello.</span><span class="sxs-lookup"><span data-stu-id="2a68f-211">In hello **Sync Directions** field, select Bi-directional Sync, toohello Hub, or From hello Hub.</span></span>

        ![Válassza ki a helyi adatbázis hello](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. <span data-ttu-id="2a68f-213">Válassza ki **OK** tooclose hello **adatbázis kijelölése** panelen.</span><span class="sxs-lookup"><span data-stu-id="2a68f-213">Select **OK** tooclose hello **Select Database** blade.</span></span> <span data-ttu-id="2a68f-214">Válassza ki **OK** tooclose hello **konfigurálása a helyszíni** panel megnyitásához, és a hello várakozás új tag toobe létrehozott és telepített szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="2a68f-214">Then select **OK** tooclose hello **Configure On-Premises** blade and wait for hello new sync member toobe created and deployed.</span></span> <span data-ttu-id="2a68f-215">Végezetül kattintson **OK** tooclose hello **szinkronizálási tagok kiválasztása** panelen.</span><span class="sxs-lookup"><span data-stu-id="2a68f-215">Finally, click **OK** tooclose hello **Select sync members** blade.</span></span>

        ![A helyi adatbázis hozzáadta a toosync csoportot](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  <span data-ttu-id="2a68f-217">Adatszinkronizálás tooconnect tooSQL és hello helyi ügynök, vegye fel a szerepkörben toohello `DataSync_Executor`.</span><span class="sxs-lookup"><span data-stu-id="2a68f-217">tooconnect tooSQL Data Sync and hello local agent, add your user name toohello role `DataSync_Executor`.</span></span> <span data-ttu-id="2a68f-218">Adatszinkronizálás hello SQL Server-példányon hoz létre ehhez a szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="2a68f-218">Data Sync creates this role on hello SQL Server instance.</span></span>

## <a name="step-3---configure-sync-group"></a><span data-ttu-id="2a68f-219">3. lépés - a szinkronizálási csoport konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2a68f-219">Step 3 - Configure sync group</span></span>

<span data-ttu-id="2a68f-220">Miután új szinkronizálási csoporttagok hello létrehozott és telepített, 3. lépés, **konfigurálása szinkronizálású csoport**, hello kijelölt **új szinkronizálású csoport** panelen.</span><span class="sxs-lookup"><span data-stu-id="2a68f-220">After hello new sync group members are created and deployed, Step 3, **Configure sync group**, is highlighted in hello **New sync group** blade.</span></span>

1.  <span data-ttu-id="2a68f-221">A hello **táblák** panelen válassza egy adatbázis szinkronizálási hello listája csoport tagjai, és válassza **frissítési séma**.</span><span class="sxs-lookup"><span data-stu-id="2a68f-221">On hello **Tables** blade, select a database from hello list of sync group members, and then select **Refresh schema**.</span></span>

2.  <span data-ttu-id="2a68f-222">Rendelkezésre álló táblák hello listában jelölje ki, hogy szeretné-e toosync hello táblákat.</span><span class="sxs-lookup"><span data-stu-id="2a68f-222">From hello list of available tables, select hello tables that you want toosync.</span></span>

    ![Válassza ki a táblák toosync](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  <span data-ttu-id="2a68f-224">Alapértelmezés szerint hello táblázat összes oszlopa van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="2a68f-224">By default, all columns in hello table are selected.</span></span> <span data-ttu-id="2a68f-225">Ha toosync összes hello oszlopok nem szeretné, tiltsa le a hello oszlopok, hogy nem szeretné, hogy toosync hello jelölőnégyzetét.</span><span class="sxs-lookup"><span data-stu-id="2a68f-225">If you don't want toosync all hello columns, disable hello checkbox for hello columns that you don't want toosync.</span></span> <span data-ttu-id="2a68f-226">Győződjön meg arról, hogy a kiválasztott tooleave hello elsődleges kulcsként megadott oszlop.</span><span class="sxs-lookup"><span data-stu-id="2a68f-226">Be sure tooleave hello primary key column selected.</span></span>

    ![Válassza ki a mezőket toosync](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  <span data-ttu-id="2a68f-228">Végül válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2a68f-228">Finally, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a68f-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a68f-229">Next steps</span></span>
<span data-ttu-id="2a68f-230">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="2a68f-230">Congratulations.</span></span> <span data-ttu-id="2a68f-231">Létrehozott egy szinkronizálású csoport, amely tartalmazza az SQL Database-példányt, és egy SQL Server-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2a68f-231">You have created a sync group that includes both a SQL Database instance and a SQL Server database.</span></span>

<span data-ttu-id="2a68f-232">SQL Database és SQL adatszinkronizálás kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="2a68f-232">For more info about SQL Database and SQL Data Sync, see:</span></span>

-   [<span data-ttu-id="2a68f-233">Hello teljes adatszinkronizálás SQL műszaki dokumentáció letöltése</span><span class="sxs-lookup"><span data-stu-id="2a68f-233">Download hello complete SQL Data Sync technical documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)
-   [<span data-ttu-id="2a68f-234">Hello SQL adatok szinkronizálása REST API-dokumentáció letöltése</span><span class="sxs-lookup"><span data-stu-id="2a68f-234">Download hello SQL Data Sync REST API documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)
-   [<span data-ttu-id="2a68f-235">SQL-adatbázis – áttekintés</span><span class="sxs-lookup"><span data-stu-id="2a68f-235">SQL Database Overview</span></span>](sql-database-technical-overview.md)
-   [<span data-ttu-id="2a68f-236">Adatbázis életciklusának kezelésére</span><span class="sxs-lookup"><span data-stu-id="2a68f-236">Database Lifecycle Management</span></span>](https://msdn.microsoft.com/library/jj907294.aspx)
