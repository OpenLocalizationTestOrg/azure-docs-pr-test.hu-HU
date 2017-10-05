---
title: "Ismerkedés az Azure SQL adatszinkronizálás (előzetes verzió) |} Microsoft Docs"
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
ms.openlocfilehash: 2d0f9d7f32ad79f49d58165d734b9df4af862835
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="getting-started-with-azure-sql-data-sync-preview"></a><span data-ttu-id="04154-103">Ismerkedés az Azure SQL adatszinkronizálás (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="04154-103">Getting Started with Azure SQL Data Sync (Preview)</span></span>
<span data-ttu-id="04154-104">Ebben az oktatóanyagban elsajátíthatja, hogyan Azure SQL Data szinkronizálás beállítása az Azure SQL Database és az SQL Server-példányokat tartalmazó hibrid szinkronizálási csoportok létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="04154-104">In this tutorial, you learn how to set up Azure SQL Data Sync by creating a hybrid sync group that contains both Azure SQL Database and SQL Server instances.</span></span> <span data-ttu-id="04154-105">Az új szinkronizálási csoport teljes van konfigurálva, és a beállított ütemezés szerint szinkronizálja.</span><span class="sxs-lookup"><span data-stu-id="04154-105">The new sync group is fully configured and synchronizes on the schedule you set.</span></span>

<span data-ttu-id="04154-106">Ez az oktatóanyag feltételezi, hogy rendelkezik-e legalább némi tapasztalattal az SQL Database és SQL-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="04154-106">This tutorial assumes that you have at least some prior experience with SQL Database and with SQL Server.</span></span> 

<span data-ttu-id="04154-107">SQL adatszinkronizálás áttekintését lásd: [szinkronizálni az adatokat](sql-database-sync-data.md).</span><span class="sxs-lookup"><span data-stu-id="04154-107">For an overview of SQL Data Sync, see [Sync data](sql-database-sync-data.md).</span></span>

<span data-ttu-id="04154-108">Teljes PowerShell-példák bemutatják, hogyan konfigurálja az SQL adatszinkronizálás tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="04154-108">For complete PowerShell examples that show how to configure SQL Data Sync, see the following articles:</span></span>
-   [<span data-ttu-id="04154-109">A PowerShell szolgáltatás használatával több Azure SQL-adatbázisok közötti szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="04154-109">Use PowerShell to sync between multiple Azure SQL databases</span></span>](scripts/sql-database-sync-data-between-sql-databases.md)
-   [<span data-ttu-id="04154-110">Egy Azure SQL-adatbázis és a helyszíni SQL Server-adatbázisok közötti szinkronizálása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="04154-110">Use PowerShell to sync between an Azure SQL Database and a SQL Server on-premises database</span></span>](scripts/sql-database-sync-data-between-azure-onprem.md)

> [!NOTE]
> <span data-ttu-id="04154-111">Az Azure SQL Data szinkronizálás, korábban, az MSDN-en, állítsa be a teljes műszaki dokumentáció áll rendelkezésre, egy. PDF-dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="04154-111">The complete technical documentation set for Azure SQL Data Sync, formerly located on MSDN, is available as a .PDF document.</span></span> <span data-ttu-id="04154-112">Töltse le [Itt](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).</span><span class="sxs-lookup"><span data-stu-id="04154-112">Download it [here](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).</span></span>

## <a name="step-1---create-sync-group"></a><span data-ttu-id="04154-113">1. lépés – a sync-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="04154-113">Step 1 - Create sync group</span></span>

### <a name="locate-the-data-sync-settings"></a><span data-ttu-id="04154-114">Keresse meg az adatok szinkronizálási beállítások</span><span class="sxs-lookup"><span data-stu-id="04154-114">Locate the Data Sync settings</span></span>

1.  <span data-ttu-id="04154-115">A böngészőben navigáljon az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="04154-115">In your browser, navigate to the Azure portal.</span></span>

2.  <span data-ttu-id="04154-116">Keresse meg az SQL-adatbázisok az irányítópultra, illetve az SQL-adatbázisok ikon a portál az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="04154-116">In the portal, locate your SQL databases from your Dashboard or from the SQL Databases icon on the toolbar.</span></span>

    ![Azure SQL-adatbázisok listája](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  <span data-ttu-id="04154-118">Az a **SQL-adatbázisok** panelen válassza ki a meglévő SQL-adatbázis adatszinkronizálás hub-adatbázisként használni kívánt. Az SQL-adatbázis panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="04154-118">On the **SQL databases** blade, select the existing SQL database that you want to use as the hub database for Data Sync. The SQL database blade opens.</span></span>

4.  <span data-ttu-id="04154-119">A kiválasztott adatbázishoz az SQL adatbázis paneljén válassza **az egyéb adatbázisokra szinkronizálási**.</span><span class="sxs-lookup"><span data-stu-id="04154-119">On the SQL database blade for the selected database, select **Sync to other databases**.</span></span> <span data-ttu-id="04154-120">Az adatszinkronizálás panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="04154-120">The Data Sync blade opens.</span></span>

    ![Szinkronizálni kívánt más adatbázisok beállítás](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a><span data-ttu-id="04154-122">Hozzon létre egy új szinkronizálási csoportot</span><span class="sxs-lookup"><span data-stu-id="04154-122">Create a new Sync Group</span></span>

1.  <span data-ttu-id="04154-123">Az adatszinkronizálás panelen válassza ki a **új szinkronizálású csoport**.</span><span class="sxs-lookup"><span data-stu-id="04154-123">On the Data Sync blade, select **New Sync Group**.</span></span> <span data-ttu-id="04154-124">A **új szinkronizálású csoport** panel nyílik meg az 1. lépésben, **szinkronizálási csoport létrehozása**, kijelölt.</span><span class="sxs-lookup"><span data-stu-id="04154-124">The **New sync group** blade opens with Step 1, **Create sync group**, highlighted.</span></span> <span data-ttu-id="04154-125">A **adatok szinkronizálása csoport létrehozása** panel is megnyílik.</span><span class="sxs-lookup"><span data-stu-id="04154-125">The **Create Data Sync Group** blade also opens.</span></span>

2.  <span data-ttu-id="04154-126">Az a **adatok szinkronizálása csoport létrehozása** panelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="04154-126">On the **Create Data Sync Group** blade, do the following things:</span></span>

    1.  <span data-ttu-id="04154-127">Az a **szinkronizálási csoportnév** mezőbe írja be az új szinkronizálási csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="04154-127">In the **Sync Group Name** field, enter a name for the new sync group.</span></span>

    2.  <span data-ttu-id="04154-128">Az a **szinkronizálási metaadatokat tároló adatbázis** területen válassza ki, hogy hozzon létre egy új adatbázist (ajánlott), vagy használjon már meglévő adatbázist.</span><span class="sxs-lookup"><span data-stu-id="04154-128">In the **Sync Metadata Database** section, choose whether to create a new database (recommended) or to use an existing database.</span></span>

        > [!NOTE]
        > <span data-ttu-id="04154-129">A Microsoft azt javasolja, hogy hozzon létre egy új, üres adatbázis kívánja használni, mint a szinkronizálás metaadatokat tároló adatbázis.</span><span class="sxs-lookup"><span data-stu-id="04154-129">Microsoft recommends that you create a new, empty database to use as the Sync Metadata Database.</span></span> <span data-ttu-id="04154-130">Adatszinkronizálás hoz létre táblák az adatbázisban, és a gyakori munkaterhelés futtatja.</span><span class="sxs-lookup"><span data-stu-id="04154-130">Data Sync creates tables in this database and runs a frequent workload.</span></span> <span data-ttu-id="04154-131">Ez az adatbázis automatikusan megosztva a szinkronizálási metaadatokat tároló adatbázis az összes, a szinkronizálási csoportok a kiválasztott régióban.</span><span class="sxs-lookup"><span data-stu-id="04154-131">This database is automatically shared as the Sync Metadata Database for all of your Sync groups in the selected region.</span></span> <span data-ttu-id="04154-132">A szinkronizálás metaadatokat tároló adatbázis, a neve vagy a szolgáltatási szint nélkül eldobása nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="04154-132">You can't change the Sync Metadata Database, its name, or its service level without dropping it.</span></span>

        <span data-ttu-id="04154-133">Ha úgy döntött, hogy **új adatbázis**, jelölje be **új adatbázis létrehozása.**</span><span class="sxs-lookup"><span data-stu-id="04154-133">If you chose **New database**, select **Create new database.**</span></span> <span data-ttu-id="04154-134">A **SQL-adatbázis** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="04154-134">The **SQL Database** blade opens.</span></span> <span data-ttu-id="04154-135">Az a **SQL-adatbázis** panelen nevet, és konfigurálja az új adatbázist.</span><span class="sxs-lookup"><span data-stu-id="04154-135">On the **SQL Database** blade, name and configure the new database.</span></span> <span data-ttu-id="04154-136">Válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="04154-136">Then select **OK**.</span></span>

        <span data-ttu-id="04154-137">Ha úgy döntött, hogy **létező adatbázis használata**, a listáról válassza ki az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="04154-137">If you chose **Use existing database**, select the database from the list.</span></span>

    3.  <span data-ttu-id="04154-138">Az a **automatikus szinkronizálás** szakaszban, először válassza **a** vagy **ki**.</span><span class="sxs-lookup"><span data-stu-id="04154-138">In the **Automatic Sync** section, first select **On** or **Off**.</span></span>

        <span data-ttu-id="04154-139">Ha úgy döntött, hogy **a**, a a **szinkronizálási gyakoriság** szakaszt, adjon meg egy számot, és válassza ki a másodperc, perc, órák vagy napok.</span><span class="sxs-lookup"><span data-stu-id="04154-139">If you chose **On**, in the **Sync Frequency** section, enter a number and select Seconds, Minutes, Hours, or Days.</span></span>

        ![Adja meg a szinkronizálás gyakorisága](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  <span data-ttu-id="04154-141">Az a **ütközésének feloldása** területen válassza ki a "Hub wins" vagy "Tag wins."</span><span class="sxs-lookup"><span data-stu-id="04154-141">In the **Conflict Resolution** section, select "Hub wins" or "Member wins."</span></span>

        ![Adja meg az ütközések feloldása](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  <span data-ttu-id="04154-143">Válassza ki **OK** , és várja meg az új szinkronizálási csoport létrehozása és telepítése.</span><span class="sxs-lookup"><span data-stu-id="04154-143">Select **OK** and wait for the new sync group to be created and deployed.</span></span>

## <a name="step-2---add-sync-members"></a><span data-ttu-id="04154-144">2. lépés – szinkronizálás tagok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="04154-144">Step 2 - Add sync members</span></span>

<span data-ttu-id="04154-145">Után az új szinkronizálási csoport létrehozása és telepítése, 2. lépés **szinkronizálási tagok hozzáadása**, kijelölt a **új szinkronizálású csoport** panelen.</span><span class="sxs-lookup"><span data-stu-id="04154-145">After the new sync group is created and deployed, Step 2, **Add sync members**, is highlighted in the **New sync group** blade.</span></span>

<span data-ttu-id="04154-146">Az a **Hub adatbázis** területen adja meg a meglévő hitelesítő adatait az SQL-adatbázis kiszolgálóra, amelyen a központ adatbázis található.</span><span class="sxs-lookup"><span data-stu-id="04154-146">In the **Hub Database** section, enter the existing credentials for the SQL Database server on which the hub database is located.</span></span> <span data-ttu-id="04154-147">Ne adjon meg *új* ebben a szakaszban a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="04154-147">Don't enter *new* credentials in this section.</span></span>

![Központi adatbázis szinkronizálása csoport hozzáadva](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

## <a name="add-an-azure-sql-database"></a><span data-ttu-id="04154-149">Egy Azure SQL adatbázis hozzáadása</span><span class="sxs-lookup"><span data-stu-id="04154-149">Add an Azure SQL Database</span></span>

<span data-ttu-id="04154-150">Az a **Tagadatbázis** szakaszban, és opcionálisan adja hozzá az Azure SQL Database szinkronizálási csoporthoz kiválasztásával **hozzáadása egy Azure-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="04154-150">In the **Member Database** section, optionally add an Azure SQL Database to the sync group by selecting **Add an Azure Database**.</span></span> <span data-ttu-id="04154-151">A **Azure-adatbázis beállítása** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="04154-151">The **Configure Azure Database** blade opens.</span></span>

<span data-ttu-id="04154-152">Az a **Azure-adatbázis beállítása** panelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="04154-152">On the **Configure Azure Database** blade, do the following things:</span></span>

1.  <span data-ttu-id="04154-153">Az a **szinkronizálási tagneve** mezőbe, adjon meg egy nevet az új szinkronizálási tag.</span><span class="sxs-lookup"><span data-stu-id="04154-153">In the **Sync Member Name** field, provide a name for the new sync member.</span></span> <span data-ttu-id="04154-154">Ez a név nem azonos azzal maga az adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="04154-154">This name is distinct from the name of the database itself.</span></span>

2.  <span data-ttu-id="04154-155">Az a **előfizetés** mező mellett válassza ki a társított Azure-előfizetés más célra.</span><span class="sxs-lookup"><span data-stu-id="04154-155">In the **Subscription** field, select the associated Azure subscription for billing purposes.</span></span>

3.  <span data-ttu-id="04154-156">Az a **Azure SQL Server** mező mellett válassza ki a meglévő SQL adatbázis-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="04154-156">In the **Azure SQL Server** field, select the existing SQL database server.</span></span>

4.  <span data-ttu-id="04154-157">Az a **Azure SQL Database** mező mellett válassza ki a meglévő SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="04154-157">In the **Azure SQL Database** field, select the existing SQL database.</span></span>

5.  <span data-ttu-id="04154-158">Az a **szinkronizálási irányban** mező, jelölje be kétirányú szinkronizálás, a központi vagy a központ.</span><span class="sxs-lookup"><span data-stu-id="04154-158">In the **Sync Directions** field, select Bi-directional Sync, To the Hub, or From the Hub.</span></span>

    ![Új SQL-adatbázis szinkronizálási tag hozzáadása](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  <span data-ttu-id="04154-160">Az a **felhasználónév** és **jelszó** mezők, adja meg a meglévő hitelesítő adatait az SQL-adatbázis kiszolgálóra, amelyen a tagok adatbázisa található.</span><span class="sxs-lookup"><span data-stu-id="04154-160">In the **Username** and **Password** fields, enter the existing credentials for the SQL Database server on which the member database is located.</span></span> <span data-ttu-id="04154-161">Ne adjon meg *új* ebben a szakaszban a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="04154-161">Don't enter *new* credentials in this section.</span></span>

7.  <span data-ttu-id="04154-162">Válassza ki **OK** , és várja meg az új szinkronizálási tag létrehozása és telepítése.</span><span class="sxs-lookup"><span data-stu-id="04154-162">Select **OK** and wait for the new sync member to be created and deployed.</span></span>

    ![Új SQL-adatbázis szinkronizálási tag hozzá lett adva.](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

## <a name="add-an-on-premises-sql-server-database"></a><span data-ttu-id="04154-164">A helyszíni SQL Server adatbázis hozzáadása</span><span class="sxs-lookup"><span data-stu-id="04154-164">Add an on-premises SQL Server database</span></span>

<span data-ttu-id="04154-165">Az a **Tagadatbázis** szakaszban, és opcionálisan adja hozzá egy helyi SQL Server a szinkronizálási csoport kiválasztásával **adja hozzá az On-Premises adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="04154-165">In the **Member Database** section, optionally add an on-premises SQL Server to the sync group by selecting **Add an On-Premises Database**.</span></span> <span data-ttu-id="04154-166">A **konfigurálása a helyszíni** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="04154-166">The **Configure On-Premises** blade opens.</span></span>

<span data-ttu-id="04154-167">Az a **konfigurálása a helyszíni** panelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="04154-167">On the **Configure On-Premises** blade, do the following things:</span></span>

1.  <span data-ttu-id="04154-168">Válassza ki **válassza ki a szinkronizálási ügynök átjáró**.</span><span class="sxs-lookup"><span data-stu-id="04154-168">Select **Choose the Sync Agent Gateway**.</span></span> <span data-ttu-id="04154-169">A **válassza ki a szinkronizálási ügynök** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="04154-169">The **Select Sync Agent** blade opens.</span></span>

    ![A szinkronizálási ügynök átjáró kiválasztása](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  <span data-ttu-id="04154-171">A a **válassza ki a szinkronizálási ügynök átjáró** panelen válassza ki, hogy egy meglévő ügynököt, vagy hozzon létre egy új ügynököt.</span><span class="sxs-lookup"><span data-stu-id="04154-171">On the **Choose the Sync Agent Gateway** blade, choose whether to use an existing agent or create a new agent.</span></span>

    <span data-ttu-id="04154-172">Ha úgy döntött, hogy **meglévő ügynökök**, válassza ki a meglévő ügynököt a listából.</span><span class="sxs-lookup"><span data-stu-id="04154-172">If you chose **Existing agents**, select the existing agent from the list.</span></span>

    <span data-ttu-id="04154-173">Ha úgy döntött, hogy **hozzon létre egy új ügynök**, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="04154-173">If you chose **Create a new agent**, do the following things:</span></span>

    1.  <span data-ttu-id="04154-174">A szinkronizálási ügynök ügyfélszoftver töltse le a megadott, és telepítse azt a számítógépet, amelyen az SQL Server is található.</span><span class="sxs-lookup"><span data-stu-id="04154-174">Download the client sync agent software from the link provided and install it on the computer where the SQL Server is located.</span></span>
 
        > [!IMPORTANT]
        > <span data-ttu-id="04154-175">Meg kell nyitnia az 1433-as kimenő TCP port ahhoz, hogy az ügyfélügynök kommunikálni a kiszolgálóval a tűzfalon.</span><span class="sxs-lookup"><span data-stu-id="04154-175">You have to open outbound TCP port 1433 in the firewall to let the client agent communicate with the server.</span></span>


    2.  <span data-ttu-id="04154-176">Adjon meg egy nevet az ügynök.</span><span class="sxs-lookup"><span data-stu-id="04154-176">Enter a name for the agent.</span></span>

    3.  <span data-ttu-id="04154-177">Válassza ki **létrehozása és a kulcs létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="04154-177">Select **Create and Generate Key**.</span></span>

    4.  <span data-ttu-id="04154-178">Az ügynök kulcs másolása a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="04154-178">Copy the agent key to the clipboard.</span></span>
        
        ![Új szinkronizálási ügynök létrehozása](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  <span data-ttu-id="04154-180">Válassza ki **OK** bezárásához a **válassza ki a szinkronizálási ügynök** panelen.</span><span class="sxs-lookup"><span data-stu-id="04154-180">Select **OK** to close the **Select Sync Agent** blade.</span></span>

    6.  <span data-ttu-id="04154-181">Keresse meg az SQL Server-számítógépen és az ügyfél-szinkronizálási ügynök alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="04154-181">On the SQL Server computer, locate and run the Client Sync Agent app.</span></span>

        ![Az adatok szinkronizálása az ügyfélalkalmazás ügynök](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  <span data-ttu-id="04154-183">Válassza ki a szinkronizálási ügynök alkalmazás **nyújt ügynök kulcs**.</span><span class="sxs-lookup"><span data-stu-id="04154-183">In the sync agent app, select **Submit Agent Key**.</span></span> <span data-ttu-id="04154-184">A **szinkronizálási metaadatok adatbázis konfigurációja** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="04154-184">The **Sync Metadata Database Configuration** dialog box opens.</span></span>

    8.  <span data-ttu-id="04154-185">Az a **szinkronizálási metaadatok adatbázis konfigurációja** párbeszédpanelen illessze be az ügynök kulcs másolása az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="04154-185">In the **Sync Metadata Database Configuration** dialog box, paste in the agent key copied from the Azure portal.</span></span> <span data-ttu-id="04154-186">A meglévő hitelesítő adatok is biztosítanak a Azure SQL Database kiszolgálón, amelyen a metaadatokat tároló adatbázis található.</span><span class="sxs-lookup"><span data-stu-id="04154-186">Also provide the existing credentials for the Azure SQL Database server on which the metadata database is located.</span></span> <span data-ttu-id="04154-187">(Ha létrehozott egy új metaadatokat tároló adatbázis, az adatbázis van a központ adatbázis ugyanazon a kiszolgálón.) Válassza ki **OK** , és várja meg a konfiguráció befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="04154-187">(If you created a new metadata database, this database is on the same server as the hub database.) Select **OK** and wait for the configuration to finish.</span></span>

        ![Adja meg az ügynök kulcsot és a kiszolgáló hitelesítő adatait](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   <span data-ttu-id="04154-189">Ha a tűzfal hibaüzenet jelenik meg ezen a ponton, hogy egy tűzfalszabály létrehozása az Azure SQL Server-számítógépen a bejövő forgalom engedélyezésére.</span><span class="sxs-lookup"><span data-stu-id="04154-189">If you get a firewall error at this point, you have to create a firewall rule on Azure to allow incoming traffic from the SQL Server computer.</span></span> <span data-ttu-id="04154-190">A szabály manuálisan hozhat létre, a portálon, de előfordulhat, hogy ez egyszerűbbé teszi az SQL Server Management Studio (SSMS) létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="04154-190">You can create the rule manually in the portal, but you may find it easier to create it in SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="04154-191">Az SSMS próbálja meg az Azure-on a központ adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="04154-191">In SSMS, try to connect to the hub database on Azure.</span></span> <span data-ttu-id="04154-192">Adja meg a nevét, \<hub_database_name\>. database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="04154-192">Enter its name as \<hub_database_name\>.database.windows.net.</span></span> <span data-ttu-id="04154-193">Kövesse a párbeszédpanelen konfigurálhatja az Azure tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="04154-193">Follow the steps in the dialog box to configure the Azure firewall rule.</span></span> <span data-ttu-id="04154-194">Térjen vissza az ügyfél-szinkronizálási ügynök alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="04154-194">Then return to the Client Sync Agent app.</span></span>

    9.  <span data-ttu-id="04154-195">Az ügyfél-szinkronizálási ügynök alkalmazásban kattintson **regisztrálása** egy SQL Server-adatbázis regisztrálására az ügynököt.</span><span class="sxs-lookup"><span data-stu-id="04154-195">In the Client Sync Agent app, click **Register** to register a SQL Server database with the agent.</span></span> <span data-ttu-id="04154-196">A **SQL Server-konfigurációs** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="04154-196">The **SQL Server Configuration** dialog box opens.</span></span>

        ![Hozzáadása és az SQL Server-adatbázisok konfigurálása](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. <span data-ttu-id="04154-198">Az a **SQL Server-konfigurációs** párbeszédpanelen válassza ki, hogy az SQL Server-hitelesítés vagy a Windows-hitelesítés használatával csatlakoznak-e.</span><span class="sxs-lookup"><span data-stu-id="04154-198">In the **SQL Server Configuration** dialog box, choose whether to connect by using SQL Server authentication or Windows authentication.</span></span> <span data-ttu-id="04154-199">Ha SQL Server-hitelesítést választja, adja meg a meglévő hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="04154-199">If you chose SQL Server authentication, enter the existing credentials.</span></span> <span data-ttu-id="04154-200">Adja meg az SQL Server nevét és a szinkronizálni kívánt adatbázis nevét. Válassza ki **tesztkapcsolat** a beállításainak ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="04154-200">Provide the SQL Server name and the name of the database that you want to sync. Select **Test connection** to test your settings.</span></span> <span data-ttu-id="04154-201">Válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="04154-201">Then select **Save**.</span></span> <span data-ttu-id="04154-202">A regisztrált adatbázis listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="04154-202">The registered database appears in the list.</span></span>

        ![SQL Server-adatbázis most már regisztrálva van](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. <span data-ttu-id="04154-204">Most már bezárhatja az ügyfél-szinkronizálási ügynök alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="04154-204">You can now close the Client Sync Agent app.</span></span>

    12. <span data-ttu-id="04154-205">A portálon a a **konfigurálása a helyszíni** panelen válassza **válassza ki az adatbázist.**</span><span class="sxs-lookup"><span data-stu-id="04154-205">In the portal, on the **Configure On-Premises** blade, select **Select the Database.**</span></span> <span data-ttu-id="04154-206">A **adatbázis kijelölése** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="04154-206">The **Select Database** blade opens.</span></span>

    13. <span data-ttu-id="04154-207">Az a **adatbázis kijelölése** panelen, a a **szinkronizálási tagneve** mezőbe, adjon meg egy nevet az új szinkronizálási tag.</span><span class="sxs-lookup"><span data-stu-id="04154-207">On the **Select Database** blade, in the **Sync Member Name** field, provide a name for the new sync member.</span></span> <span data-ttu-id="04154-208">Ez a név nem azonos azzal maga az adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="04154-208">This name is distinct from the name of the database itself.</span></span> <span data-ttu-id="04154-209">A listáról válassza ki az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="04154-209">Select the database from the list.</span></span> <span data-ttu-id="04154-210">Az a **szinkronizálási irányban** mező, jelölje be kétirányú szinkronizálás, a központi vagy a központ.</span><span class="sxs-lookup"><span data-stu-id="04154-210">In the **Sync Directions** field, select Bi-directional Sync, To the Hub, or From the Hub.</span></span>

        ![Jelölje ki a helyi adatbázist](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. <span data-ttu-id="04154-212">Válassza ki **OK** bezárásához a **adatbázis kijelölése** panelen.</span><span class="sxs-lookup"><span data-stu-id="04154-212">Select **OK** to close the **Select Database** blade.</span></span> <span data-ttu-id="04154-213">Válassza ki **OK** bezárásához a **konfigurálása a helyszíni** panel megnyitásához, és várja meg az új szinkronizálási tag létrehozása és telepítése.</span><span class="sxs-lookup"><span data-stu-id="04154-213">Then select **OK** to close the **Configure On-Premises** blade and wait for the new sync member to be created and deployed.</span></span> <span data-ttu-id="04154-214">Végezetül kattintson **OK** bezárásához a **szinkronizálási tagok kiválasztása** panelen.</span><span class="sxs-lookup"><span data-stu-id="04154-214">Finally, click **OK** to close the **Select sync members** blade.</span></span>

        ![A helyi adatbázis szinkronizálási csoportba felvett](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  <span data-ttu-id="04154-216">Csatlakozás SQL adatszinkronizálás és a helyi ügynök, adja hozzá a felhasználónevet a szerepkörhöz `DataSync_Executor`.</span><span class="sxs-lookup"><span data-stu-id="04154-216">To connect to SQL Data Sync and the local agent, add your user name to the role `DataSync_Executor`.</span></span> <span data-ttu-id="04154-217">Adatszinkronizálás ezt a szerepkört az SQL Server-példányt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="04154-217">Data Sync creates this role on the SQL Server instance.</span></span>

## <a name="step-3---configure-sync-group"></a><span data-ttu-id="04154-218">3. lépés - a szinkronizálási csoport konfigurálása</span><span class="sxs-lookup"><span data-stu-id="04154-218">Step 3 - Configure sync group</span></span>

<span data-ttu-id="04154-219">Miután az új szinkronizálási csoporttagok létrehozott és telepített, 3. lépés, **konfigurálása szinkronizálású csoport**, kijelölt a **új szinkronizálású csoport** panelen.</span><span class="sxs-lookup"><span data-stu-id="04154-219">After the new sync group members are created and deployed, Step 3, **Configure sync group**, is highlighted in the **New sync group** blade.</span></span>

1.  <span data-ttu-id="04154-220">Az a **táblák** panelen válassza a szinkronizálási listájáról adatbázis csoport tagjai, és válassza **frissítési séma**.</span><span class="sxs-lookup"><span data-stu-id="04154-220">On the **Tables** blade, select a database from the list of sync group members, and then select **Refresh schema**.</span></span>

2.  <span data-ttu-id="04154-221">Az elérhető táblák listájában jelölje ki a szinkronizálni kívánt táblák.</span><span class="sxs-lookup"><span data-stu-id="04154-221">From the list of available tables, select the tables that you want to sync.</span></span>

    ![Válassza ki a szinkronizálandó táblák](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  <span data-ttu-id="04154-223">Alapértelmezés szerint a táblázat összes oszlopa van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="04154-223">By default, all columns in the table are selected.</span></span> <span data-ttu-id="04154-224">Ha nem szeretné szinkronizálni az oszlopok, tiltsa le az oszlopok nem kívánt szinkronizálására vonatkozó négyzet jelölését. Ne feledje, hogy a kiválasztott elsődleges kulcsként megadott oszlop.</span><span class="sxs-lookup"><span data-stu-id="04154-224">If you don't want to sync all the columns, disable the checkbox for the columns that you don't want to sync. Be sure to leave the primary key column selected.</span></span>

    ![Jelöljön ki mezőket szinkronizálása](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  <span data-ttu-id="04154-226">Végül válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="04154-226">Finally, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="04154-227">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="04154-227">Next steps</span></span>
<span data-ttu-id="04154-228">Gratulálok.</span><span class="sxs-lookup"><span data-stu-id="04154-228">Congratulations.</span></span> <span data-ttu-id="04154-229">Létrehozott egy szinkronizálású csoport, amely tartalmazza az SQL Database-példányt, és egy SQL Server-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="04154-229">You have created a sync group that includes both a SQL Database instance and a SQL Server database.</span></span>

<span data-ttu-id="04154-230">SQL Database és SQL adatszinkronizálás kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="04154-230">For more info about SQL Database and SQL Data Sync, see:</span></span>

-   [<span data-ttu-id="04154-231">A teljes SQL adatszinkronizálás műszaki dokumentáció letöltése</span><span class="sxs-lookup"><span data-stu-id="04154-231">Download the complete SQL Data Sync technical documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)
-   [<span data-ttu-id="04154-232">Töltse le az SQL Data szinkronizálási REST API dokumentációja</span><span class="sxs-lookup"><span data-stu-id="04154-232">Download the SQL Data Sync REST API documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)
-   [<span data-ttu-id="04154-233">SQL-adatbázis – áttekintés</span><span class="sxs-lookup"><span data-stu-id="04154-233">SQL Database Overview</span></span>](sql-database-technical-overview.md)
-   [<span data-ttu-id="04154-234">Adatbázis életciklusának kezelésére</span><span class="sxs-lookup"><span data-stu-id="04154-234">Database Lifecycle Management</span></span>](https://msdn.microsoft.com/library/jj907294.aspx)
