---
title: "Excel csatlakoztatása SQL Database adatbázishoz| Microsoft Docs"
description: "Útmutató a Microsoft Excel Azure SQL adatbázishoz való csatlakoztatásához a felhőben. Adatok importálása Excelbe jelentésekhez és adatok áttekintéséhez."
services: sql-database
keywords: "excel csatlakoztatása sql-hez, adatok importálása excelbe"
documentationcenter: 
author: joseidz
manager: jhubbard
editor: 
ms.assetid: 906924bc-2707-48d3-bac6-397976a0409d
ms.service: sql-database
ms.custom: develop apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: jhubbard
ms.openlocfilehash: 97344d7c0be38b3092a3224074d486b5bb984176
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-excel-to-an-azure-sql-database-and-create-a-report"></a><span data-ttu-id="f809c-105">Excel csatlakoztatása Azure SQL-adatbázis és a jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="f809c-105">Connect Excel to an Azure SQL database and create a report</span></span>

<span data-ttu-id="f809c-106">Excel csatlakoztatása SQL-adatbázis a felhőben, és adatokat importálhat, és létre táblázatokat és diagramokat az adatbázisban levő értékek alapján.</span><span class="sxs-lookup"><span data-stu-id="f809c-106">Connect Excel to a SQL database in the cloud and import data and create tables and charts based on values in the database.</span></span> <span data-ttu-id="f809c-107">Ebben az oktatóanyagban csatlakoztatjuk az Excelt az adatbázistáblához, elmentjük az adatokat tároló fájlt és az Excelre vonatkozó kapcsolatadatokat, és kimutatásdiagramot hozunk létre az adatbázis értékeiből.</span><span class="sxs-lookup"><span data-stu-id="f809c-107">In this tutorial you will set up the connection between Excel and a database table, save the file that stores data and the connection information for Excel, and then create a pivot chart from the database values.</span></span>

<span data-ttu-id="f809c-108">Ezekhez a műveletekhez szükség van egy Azure SQL-adatbázisra.</span><span class="sxs-lookup"><span data-stu-id="f809c-108">You'll need a SQL database in Azure before you get started.</span></span> <span data-ttu-id="f809c-109">Ha nincs ilyen, az [Első SQL-adatbázis létrehozása](sql-database-get-started-portal.md) alapján hozzon létre egy adatbázist mintaadatokkal, majd futtassa - ez csupán néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="f809c-109">If you don't have one, see [Create your first SQL database](sql-database-get-started-portal.md) to get a database with sample data up and running in a few minutes.</span></span> <span data-ttu-id="f809c-110">Ebben a cikkben lesz importál mintaadatokat az Excelbe, ha a cikkben, de a lépésekkel hasonló a saját adataival.</span><span class="sxs-lookup"><span data-stu-id="f809c-110">In this article, you'll import sample data into Excel from that article, but you can follow similar steps with your own data.</span></span>

<span data-ttu-id="f809c-111">Az Excelnek is telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f809c-111">You'll also need a copy of Excel.</span></span> <span data-ttu-id="f809c-112">Ebben a cikkben a [Microsoft Excel 2016](https://products.office.com/) verziót vettük alapul.</span><span class="sxs-lookup"><span data-stu-id="f809c-112">This article uses [Microsoft Excel 2016](https://products.office.com/).</span></span>

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a><span data-ttu-id="f809c-113">Excel csatlakoztatása SQL-adatbázishoz és ODC-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="f809c-113">Connect Excel to a SQL database and create an odc file</span></span>
1. <span data-ttu-id="f809c-114">Az Excel SQL-adatbázishoz való csatlakoztatásához nyissa meg az Excelt, majd hozzon létre egy új munkafüzetet, vagy nyisson meg egy meglévő Excel-munkafüzetet.</span><span class="sxs-lookup"><span data-stu-id="f809c-114">To connect Excel to SQL database, open Excel and then create a new workbook or open an existing Excel workbook.</span></span>
2. <span data-ttu-id="f809c-115">Az oldal tetején található menüsávon kattintson az **Adatok**, majd az**Egyéb forrásokból származó**, majd az**SQL szerverről származó** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="f809c-115">In the menu bar at the top of the page click **Data**, click **From Other Sources**, and then click **From SQL Server**.</span></span>
   
   ![Adatforrás kiválasztása: Excel csatlakoztatása SQL-adatbázishoz.](./media/sql-database-connect-excel/excel_data_source.png)
   
   <span data-ttu-id="f809c-117">Megnyílik az Adatkapcsolat varázsló.</span><span class="sxs-lookup"><span data-stu-id="f809c-117">The Data Connection Wizard opens.</span></span>
3. <span data-ttu-id="f809c-118">A **Kapcsolódás adatbázis-kiszolgálóhoz** párbeszédpanelen írja be annak az SQL Database adatbázisnak a **Kiszolgálónevét**, amelyhez csatlakozni szeretne <*kiszolgálónév*>**. database.windows.net** formában.</span><span class="sxs-lookup"><span data-stu-id="f809c-118">In the **Connect to Database Server** dialog box, type the SQL Database **Server name** you want to connect to in the form <*servername*>**.database.windows.net**.</span></span> <span data-ttu-id="f809c-119">Például: **adworkserver.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="f809c-119">For example, **adworkserver.database.windows.net**.</span></span>
4. <span data-ttu-id="f809c-120">A **Hitelesítő adatok** alatt kattintson a **Következő felhasználónév és jelszó használata** lehetőségre, adja meg azt a **Felhasználónevet** és **Jelszót**, amelyet az SQL Database-kiszolgáló létrehozásakor beállított, majd kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="f809c-120">Under **Log on credentials**, click **Use the following User Name and Password**, type the **User Name** and **Password** you set up for the SQL Database server when you created it, and then click **Next**.</span></span>
   
   ![Kiszolgálónév és hitelesítő adatok megadása](./media/sql-database-connect-excel/connect-to-server.png)
   
   > [!TIP]
   > <span data-ttu-id="f809c-122">A hálózati környezettől függően előfordulhat, hogy nem tud csatlakozni, vagy megszakad a kapcsolat, ha az SQL Database-kiszolgáló nem engedélyezi az ügyfél IP-címről érkező forgalmat.</span><span class="sxs-lookup"><span data-stu-id="f809c-122">Depending on your network environment, you may not be able to connect or you may lose the connection if the SQL Database server doesn't allow traffic from your client IP address.</span></span> <span data-ttu-id="f809c-123">Lépjen az [Azure Portalhoz](https://portal.azure.com/), kattintson az SQL Server-példányok lehetőségre, majd a saját kiszolgálójára, ezután a beállítások alatt a tűzfalra, és adja hozzá ügyfél IP-címét.</span><span class="sxs-lookup"><span data-stu-id="f809c-123">Go to the [Azure portal](https://portal.azure.com/), click SQL servers, click your server, click firewall under settings and add your client IP address.</span></span> <span data-ttu-id="f809c-124">A részleteket a [Tűzfal beállításainak konfigurálása](sql-database-configure-firewall-settings.md) részben találja meg.</span><span class="sxs-lookup"><span data-stu-id="f809c-124">See [How to configure firewall settings](sql-database-configure-firewall-settings.md) for details.</span></span>
   > 
   > 
5. <span data-ttu-id="f809c-125">Az **Adatbázis és tábla kijelölése** párbeszédpanelen válassza a listából azt az adatbázist, amellyel dolgozni szeretne, majd kattintson azokra a táblákra vagy nézetekre, amelyekkel dolgozni szeretne (ebben a példában a **vGetAllCategories**), majd kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="f809c-125">In the **Select Database and Table** dialog, select the database you want to work with from the list, and then click the tables or views you want to work with (we chose **vGetAllCategories**), and then click **Next**.</span></span>
   
    ![Válasszon ki egy adatbázist és egy táblát.](./media/sql-database-connect-excel/select-database-and-table.png)
   
    <span data-ttu-id="f809c-127">Megnyílik az **Adatkapcsolatfájl mentése és befejezése** párbeszédpanel, ahol meg kell adnia az Excel által alkalmazott Office adatbázis-kapcsolat fájlra (*.odc) vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="f809c-127">The **Save Data Connection File and Finish** dialog box opens, where you provide information about the Office database connection (*.odc) file that Excel uses.</span></span> <span data-ttu-id="f809c-128">Meghagyhatja az alapértelmezett beállításokat, vagy testre szabhatja a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="f809c-128">You can leave the defaults or customize your selections.</span></span>
6. <span data-ttu-id="f809c-129">Meghagyhatja az alapértelmezett beállításokat, de jegyezze meg a **Fájlnevet**.</span><span class="sxs-lookup"><span data-stu-id="f809c-129">You can leave the defaults, but note the **File Name** in particular.</span></span> <span data-ttu-id="f809c-130">A **Leírás**, a **Rövid név** és a **Kulcsszavak** segítenek Önnek és a többi felhasználónak abban, hogy mihez csatlakozik és a kapcsolat megtalálásában.</span><span class="sxs-lookup"><span data-stu-id="f809c-130">A **Description**, a **Friendly Name**, and **Search Keywords** help you and other users remember what you're connecting to and find the connection.</span></span> <span data-ttu-id="f809c-131">Kattintson az **Adatfrissítéshez kísérelje meg mindig ezt a fájlt használni** lehetőségre, ha a kapcsolatadatokat menteni szeretné az ODC-fájlba, hogy az adatok a kapcsolódáskor frissüljenek, majd kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="f809c-131">Click **Always attempt to use this file to refresh data** if you want connection information stored in the odc file so it can update when you connect to it, and then click **Finish**.</span></span>
   
    ![ODC-fájl mentése](./media/sql-database-connect-excel/save-odc-file.png)
   
    <span data-ttu-id="f809c-133">Megjelenik az **Adatimportálás** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f809c-133">The **Import data** dialog box appears.</span></span>

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a><span data-ttu-id="f809c-134">Adatok importálása Excelbe és kimutatásdiagram létrehozása</span><span class="sxs-lookup"><span data-stu-id="f809c-134">Import the data into Excel and create a pivot chart</span></span>
<span data-ttu-id="f809c-135">Most, hogy létrehozta a kapcsolatot és az adatokat, illetve kapcsolatadatokat tartalmazó fájlt, az adatok importálása következik.</span><span class="sxs-lookup"><span data-stu-id="f809c-135">Now that you've established the connection and created the file with data and connection information, you're reading to import the data.</span></span>

1. <span data-ttu-id="f809c-136">Az **Adatok importálása** párbeszédpanelen kattintson az adatok munkalapon történő megjelenítéséhez használni kívánt beállításra, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f809c-136">In the **Import Data** dialog, click the option you want for presenting your data in the worksheet and then click **OK**.</span></span> <span data-ttu-id="f809c-137">Válassza a **Kimutatásdiagram** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f809c-137">We chose **PivotChart**.</span></span> <span data-ttu-id="f809c-138">Az **Új munkalap**kiválasztásával új munkalapot is létrehozhat, vagy választhatja az **Adatok hozzáadása adatmodellhez** beállítást is.</span><span class="sxs-lookup"><span data-stu-id="f809c-138">You can also choose to create a **New worksheet** or to **Add this data to a Data Model**.</span></span> <span data-ttu-id="f809c-139">Az adatmodellekről további információkat az [Adatmodell létrehozása Excelben](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B) részben talál.</span><span class="sxs-lookup"><span data-stu-id="f809c-139">For more information on Data Models, see [Create a data model in Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B).</span></span> <span data-ttu-id="f809c-140">Kattintson a **Tulajdonságok** lehetőségre az előző lépésben létrehozott ODC-fájllal kapcsolatos információkért és az adatfrissítés beállításainak kiválasztásáért.</span><span class="sxs-lookup"><span data-stu-id="f809c-140">Click **Properties** to explore information about the odc file you created in the previous step and to choose options for refreshing the data.</span></span>
   
    ![Az Excelben szereplő adatok formátumának kiválasztása](./media/sql-database-connect-excel/import-data.png)
   
    <span data-ttu-id="f809c-142">A munkalapon most egy üres kimutatástábla és -diagram van.</span><span class="sxs-lookup"><span data-stu-id="f809c-142">The worksheet now has an empty pivot table and chart.</span></span>
2. <span data-ttu-id="f809c-143">A **Kimutatástábla mezői** alatt jelölje ki az összes megtekinteni kívánt mező jelölőnégyzetét.</span><span class="sxs-lookup"><span data-stu-id="f809c-143">Under **PivotTable Fields**, select all the check-boxes for the fields you want to view.</span></span>
   
    ![Konfigurálja az adatbázis-jelentést.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> <span data-ttu-id="f809c-145">Ha más Excel-munkafüzeteket és munkalapokat szeretne csatlakoztatni az adatbázishoz, kattintson az **Adatok**, majd a **Kapcsolatok** és a **Hozzáadás** gombra, válassza a listából a létrehozott kapcsolatot, és kattintson **Megnyitás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f809c-145">If you want to connect other Excel workbooks and worksheets to the database, click **Data**, click **Connections**, click **Add**, choose the connection you created from the list, and then click **Open**.</span></span>
> <span data-ttu-id="f809c-146">![Kapcsolat megnyitása másik munkafüzetből](./media/sql-database-connect-excel/open-from-another-workbook.png)</span><span class="sxs-lookup"><span data-stu-id="f809c-146">![Open a connection from another workbook](./media/sql-database-connect-excel/open-from-another-workbook.png)</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="f809c-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f809c-147">Next steps</span></span>
* <span data-ttu-id="f809c-148">[Kapcsolódás az SQL Database adatbázishoz az SQL Server Management Studio használatával](sql-database-connect-query-ssms.md) speciális lekérdezés és elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="f809c-148">Learn how to [Connect to SQL Database with SQL Server Management Studio](sql-database-connect-query-ssms.md) for advanced querying and analysis.</span></span>
* <span data-ttu-id="f809c-149">Tudjon meg többet a [rugalmas készletek](sql-database-elastic-pool.md) előnyeiről.</span><span class="sxs-lookup"><span data-stu-id="f809c-149">Learn about the benefits of [elastic pools](sql-database-elastic-pool.md).</span></span>
* <span data-ttu-id="f809c-150">[A háttérben SQL Database adatbázishoz kapcsolódó webalkalmazás létrehozása](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="f809c-150">Learn how to [create a web application that connects to SQL Database on the back-end](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span></span>

