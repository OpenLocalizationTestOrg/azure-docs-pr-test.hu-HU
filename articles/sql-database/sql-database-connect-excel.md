---
title: "aaaConnect Excel tooSQL adatbázis |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconnect Microsoft Excel tooAzure SQL adatbázis-hello felhőben. Adatok importálása Excelbe jelentésekhez és adatok áttekintéséhez."
services: sql-database
keywords: "Kapcsolódás excel-toosql, tooexcel adatok importálása"
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
ms.openlocfilehash: 0048849432023145bd1009d45b6d9b64a9c7ac3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-tooan-azure-sql-database-and-create-a-report"></a><span data-ttu-id="aacbc-105">Kapcsolódás Excel tooan Azure SQL-adatbázis és a jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="aacbc-105">Connect Excel tooan Azure SQL database and create a report</span></span>

<span data-ttu-id="aacbc-106">Kapcsolódás Excel tooa SQL-adatbázis hello felhőben, és adatokat importálhat, és létre táblázatokat és diagramokat hello adatbázisban levő értékek alapján.</span><span class="sxs-lookup"><span data-stu-id="aacbc-106">Connect Excel tooa SQL database in hello cloud and import data and create tables and charts based on values in hello database.</span></span> <span data-ttu-id="aacbc-107">Ebben az oktatóanyagban hello kapcsolatot az Excel és egy adatbázis tábláinak beállításához hello fájlt, amely tárolja az adatokat és hello Excelre vonatkozó kapcsolatadatokat menteni, és ezután kimutatásdiagram létrehozása az hello adatbázis értékeiből.</span><span class="sxs-lookup"><span data-stu-id="aacbc-107">In this tutorial you will set up hello connection between Excel and a database table, save hello file that stores data and hello connection information for Excel, and then create a pivot chart from hello database values.</span></span>

<span data-ttu-id="aacbc-108">Ezekhez a műveletekhez szükség van egy Azure SQL-adatbázisra.</span><span class="sxs-lookup"><span data-stu-id="aacbc-108">You'll need a SQL database in Azure before you get started.</span></span> <span data-ttu-id="aacbc-109">Ha még nincs fiókja, lásd: [az első SQL-adatbázis létrehozása](sql-database-get-started-portal.md) tooget egy adatbázist mintaadatokkal, majd futtassa néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="aacbc-109">If you don't have one, see [Create your first SQL database](sql-database-get-started-portal.md) tooget a database with sample data up and running in a few minutes.</span></span> <span data-ttu-id="aacbc-110">Ebben a cikkben lesz importál mintaadatokat az Excelbe, ha a cikkben, de a lépésekkel hasonló a saját adataival.</span><span class="sxs-lookup"><span data-stu-id="aacbc-110">In this article, you'll import sample data into Excel from that article, but you can follow similar steps with your own data.</span></span>

<span data-ttu-id="aacbc-111">Az Excelnek is telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="aacbc-111">You'll also need a copy of Excel.</span></span> <span data-ttu-id="aacbc-112">Ebben a cikkben a [Microsoft Excel 2016](https://products.office.com/) verziót vettük alapul.</span><span class="sxs-lookup"><span data-stu-id="aacbc-112">This article uses [Microsoft Excel 2016](https://products.office.com/).</span></span>

## <a name="connect-excel-tooa-sql-database-and-create-an-odc-file"></a><span data-ttu-id="aacbc-113">Kapcsolódás Excel tooa SQL adatbázishoz és odc-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="aacbc-113">Connect Excel tooa SQL database and create an odc file</span></span>
1. <span data-ttu-id="aacbc-114">tooconnect Excel tooSQL adatbázis, nyissa meg az Excel és hozhat létre egy új munkafüzetet, vagy nyisson meg egy meglévő Excel-munkafüzet.</span><span class="sxs-lookup"><span data-stu-id="aacbc-114">tooconnect Excel tooSQL database, open Excel and then create a new workbook or open an existing Excel workbook.</span></span>
2. <span data-ttu-id="aacbc-115">Hello menüsávon hello oldal hello tetején kattintson **adatok**, kattintson a **egyéb forrásokból származó**, és kattintson a **az SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="aacbc-115">In hello menu bar at hello top of hello page click **Data**, click **From Other Sources**, and then click **From SQL Server**.</span></span>
   
   ![Adatforrás kiválasztása: Excel tooSQL adatbázis csatlakozzon.](./media/sql-database-connect-excel/excel_data_source.png)
   
   <span data-ttu-id="aacbc-117">Megnyílik a hello Adatkapcsolat varázsló.</span><span class="sxs-lookup"><span data-stu-id="aacbc-117">hello Data Connection Wizard opens.</span></span>
3. <span data-ttu-id="aacbc-118">A hello **tooDatabase Server csatlakozás** párbeszédpanelen típus hello SQL-adatbázis **kiszolgálónév** tooconnect tooin hello űrlapot <*kiszolgálónév* > **. database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="aacbc-118">In hello **Connect tooDatabase Server** dialog box, type hello SQL Database **Server name** you want tooconnect tooin hello form <*servername*>**.database.windows.net**.</span></span> <span data-ttu-id="aacbc-119">Például: **adworkserver.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="aacbc-119">For example, **adworkserver.database.windows.net**.</span></span>
4. <span data-ttu-id="aacbc-120">A **azonosító adatok**, kattintson a **a következő felhasználónevet és jelszót használja hello**, típus hello **felhasználónév** és **jelszó** vonatkozóan beállított SQL-adatbáziskiszolgáló hello létrehozása után, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="aacbc-120">Under **Log on credentials**, click **Use hello following User Name and Password**, type hello **User Name** and **Password** you set up for hello SQL Database server when you created it, and then click **Next**.</span></span>
   
   ![Írja be a hello kiszolgálónév és hitelesítő adatait](./media/sql-database-connect-excel/connect-to-server.png)
   
   > [!TIP]
   > <span data-ttu-id="aacbc-122">A hálózati környezettől függően nem fogja tudni tooconnect, vagy ha hello SQL adatbázis-kiszolgáló nem engedélyezi az ügyfél IP-címről érkező forgalom hello kapcsolat megszakad.</span><span class="sxs-lookup"><span data-stu-id="aacbc-122">Depending on your network environment, you may not be able tooconnect or you may lose hello connection if hello SQL Database server doesn't allow traffic from your client IP address.</span></span> <span data-ttu-id="aacbc-123">Nyissa meg toohello [Azure-portálon](https://portal.azure.com/), kattintson az SQL Server, kattintson arra a kiszolgálóra, kattintson a beállítások a tűzfal és az ügyfél IP-cím hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="aacbc-123">Go toohello [Azure portal](https://portal.azure.com/), click SQL servers, click your server, click firewall under settings and add your client IP address.</span></span> <span data-ttu-id="aacbc-124">Lásd: [hogyan tooconfigure tűzfalbeállítások](sql-database-configure-firewall-settings.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="aacbc-124">See [How tooconfigure firewall settings](sql-database-configure-firewall-settings.md) for details.</span></span>
   > 
   > 
5. <span data-ttu-id="aacbc-125">A hello **adatbázis és tábla kijelölése** párbeszédpanelen jelölje be hello adatbázis azt szeretné, hogy a toowork hello listából, és kattintson a hello táblák vagy nézetek azt szeretné, hogy a toowork (választottuk **vGetAllCategories**), majd Kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="aacbc-125">In hello **Select Database and Table** dialog, select hello database you want toowork with from hello list, and then click hello tables or views you want toowork with (we chose **vGetAllCategories**), and then click **Next**.</span></span>
   
    ![Válasszon ki egy adatbázist és egy táblát.](./media/sql-database-connect-excel/select-database-and-table.png)
   
    <span data-ttu-id="aacbc-127">Hello **Adatkapcsolatfájl mentése és befejezése** párbeszédpanel megnyitása, ahol meg kell adnia hello Office adatbázis-kapcsolati (*.odc) fájl, amely Excel-használ kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="aacbc-127">hello **Save Data Connection File and Finish** dialog box opens, where you provide information about hello Office database connection (*.odc) file that Excel uses.</span></span> <span data-ttu-id="aacbc-128">Meghagyhatja hello alapértelmezett beállításokat, vagy testre szabhatja a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="aacbc-128">You can leave hello defaults or customize your selections.</span></span>
6. <span data-ttu-id="aacbc-129">Meghagyhatja hello alapértelmezett beállításokat, de a Megjegyzés hello **Fájlnév** különösen.</span><span class="sxs-lookup"><span data-stu-id="aacbc-129">You can leave hello defaults, but note hello **File Name** in particular.</span></span> <span data-ttu-id="aacbc-130">A **leírás**, egy **rövid név**, és **keresési kulcsszavakat** segítséget és a többi felhasználónak abban, hogy milyen kapcsolat tooand hello kapcsolat található.</span><span class="sxs-lookup"><span data-stu-id="aacbc-130">A **Description**, a **Friendly Name**, and **Search Keywords** help you and other users remember what you're connecting tooand find hello connection.</span></span> <span data-ttu-id="aacbc-131">Kattintson a **mindig kísérlet toouse a fájladatok toorefresh** Ha kapcsolatadatokat menteni hello odc-fájlt, hogy tooit csatlakozni, és kattintson a kívánt **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="aacbc-131">Click **Always attempt toouse this file toorefresh data** if you want connection information stored in hello odc file so it can update when you connect tooit, and then click **Finish**.</span></span>
   
    ![ODC-fájl mentése](./media/sql-database-connect-excel/save-odc-file.png)
   
    <span data-ttu-id="aacbc-133">Hello **adatimportálás** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="aacbc-133">hello **Import data** dialog box appears.</span></span>

## <a name="import-hello-data-into-excel-and-create-a-pivot-chart"></a><span data-ttu-id="aacbc-134">Hello adatok importálása Excelbe és kimutatásdiagram létrehozása</span><span class="sxs-lookup"><span data-stu-id="aacbc-134">Import hello data into Excel and create a pivot chart</span></span>
<span data-ttu-id="aacbc-135">Most, hogy létrehozta a hello kapcsolat és a létrehozott hello fájl és a kapcsolati adatok, tooimport hello adatok olvasása közben.</span><span class="sxs-lookup"><span data-stu-id="aacbc-135">Now that you've established hello connection and created hello file with data and connection information, you're reading tooimport hello data.</span></span>

1. <span data-ttu-id="aacbc-136">A hello **és adatokat importálhat** párbeszédpanel, kattintson az adatok megjelenítésére hello munkalapon kívánt hello beállítást, majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="aacbc-136">In hello **Import Data** dialog, click hello option you want for presenting your data in hello worksheet and then click **OK**.</span></span> <span data-ttu-id="aacbc-137">Válassza a **Kimutatásdiagram** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="aacbc-137">We chose **PivotChart**.</span></span> <span data-ttu-id="aacbc-138">Másik lehetőségként toocreate egy **új munkalapra lesznek** vagy túl**ezen adatok tooa adatmodell hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="aacbc-138">You can also choose toocreate a **New worksheet** or too**Add this data tooa Data Model**.</span></span> <span data-ttu-id="aacbc-139">Az adatmodellekről további információkat az [Adatmodell létrehozása Excelben](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B) részben talál.</span><span class="sxs-lookup"><span data-stu-id="aacbc-139">For more information on Data Models, see [Create a data model in Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B).</span></span> <span data-ttu-id="aacbc-140">Kattintson a **tulajdonságok** tooexplore információ hello ODC-fájl hello előző lépés és toochoose beállításainak kiválasztásáért hello adatok létrehozott.</span><span class="sxs-lookup"><span data-stu-id="aacbc-140">Click **Properties** tooexplore information about hello odc file you created in hello previous step and toochoose options for refreshing hello data.</span></span>
   
    ![Az Excel adatok formátumát hello kiválasztása](./media/sql-database-connect-excel/import-data.png)
   
    <span data-ttu-id="aacbc-142">hello munkalapon most már rendelkezik egy üres Kimutatástábla és egy diagram.</span><span class="sxs-lookup"><span data-stu-id="aacbc-142">hello worksheet now has an empty pivot table and chart.</span></span>
2. <span data-ttu-id="aacbc-143">A **kimutatástábla mezői**, válassza ki az összes hello jelölőnégyzetét hello tooview kívánt mezőket.</span><span class="sxs-lookup"><span data-stu-id="aacbc-143">Under **PivotTable Fields**, select all hello check-boxes for hello fields you want tooview.</span></span>
   
    ![Konfigurálja az adatbázis-jelentést.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> <span data-ttu-id="aacbc-145">Tooconnect más Excel-munkafüzeteket és munkalapokat toohello adatbázis, kattintson a **adatok**, kattintson a **kapcsolatok**, kattintson a **Hozzáadás**, válassza ki a létrehozott hello kapcsolatot hello listában, és kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="aacbc-145">If you want tooconnect other Excel workbooks and worksheets toohello database, click **Data**, click **Connections**, click **Add**, choose hello connection you created from hello list, and then click **Open**.</span></span>
> <span data-ttu-id="aacbc-146">![Kapcsolat megnyitása másik munkafüzetből](./media/sql-database-connect-excel/open-from-another-workbook.png)</span><span class="sxs-lookup"><span data-stu-id="aacbc-146">![Open a connection from another workbook](./media/sql-database-connect-excel/open-from-another-workbook.png)</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="aacbc-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aacbc-147">Next steps</span></span>
* <span data-ttu-id="aacbc-148">Ismerje meg, hogyan túl[tooSQL adatbázis csatlakozzon az SQL Server Management Studio](sql-database-connect-query-ssms.md) speciális lekérdezés és elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="aacbc-148">Learn how too[Connect tooSQL Database with SQL Server Management Studio](sql-database-connect-query-ssms.md) for advanced querying and analysis.</span></span>
* <span data-ttu-id="aacbc-149">További tudnivalók a hello előnyeinek [rugalmas készletek](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="aacbc-149">Learn about hello benefits of [elastic pools](sql-database-elastic-pool.md).</span></span>
* <span data-ttu-id="aacbc-150">Ismerje meg, hogyan túl[hozzon létre egy webalkalmazást, amely összeköti a hello háttér-adatbázis tooSQL](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="aacbc-150">Learn how too[create a web application that connects tooSQL Database on hello back-end](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span></span>

