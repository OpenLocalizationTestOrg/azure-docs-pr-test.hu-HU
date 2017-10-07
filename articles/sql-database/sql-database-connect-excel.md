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
# <a name="connect-excel-tooan-azure-sql-database-and-create-a-report"></a>Kapcsolódás Excel tooan Azure SQL-adatbázis és a jelentés létrehozása

Kapcsolódás Excel tooa SQL-adatbázis hello felhőben, és adatokat importálhat, és létre táblázatokat és diagramokat hello adatbázisban levő értékek alapján. Ebben az oktatóanyagban hello kapcsolatot az Excel és egy adatbázis tábláinak beállításához hello fájlt, amely tárolja az adatokat és hello Excelre vonatkozó kapcsolatadatokat menteni, és ezután kimutatásdiagram létrehozása az hello adatbázis értékeiből.

Ezekhez a műveletekhez szükség van egy Azure SQL-adatbázisra. Ha még nincs fiókja, lásd: [az első SQL-adatbázis létrehozása](sql-database-get-started-portal.md) tooget egy adatbázist mintaadatokkal, majd futtassa néhány perc múlva. Ebben a cikkben lesz importál mintaadatokat az Excelbe, ha a cikkben, de a lépésekkel hasonló a saját adataival.

Az Excelnek is telepítve kell lennie. Ebben a cikkben a [Microsoft Excel 2016](https://products.office.com/) verziót vettük alapul.

## <a name="connect-excel-tooa-sql-database-and-create-an-odc-file"></a>Kapcsolódás Excel tooa SQL adatbázishoz és odc-fájl létrehozása
1. tooconnect Excel tooSQL adatbázis, nyissa meg az Excel és hozhat létre egy új munkafüzetet, vagy nyisson meg egy meglévő Excel-munkafüzet.
2. Hello menüsávon hello oldal hello tetején kattintson **adatok**, kattintson a **egyéb forrásokból származó**, és kattintson a **az SQL Server**.
   
   ![Adatforrás kiválasztása: Excel tooSQL adatbázis csatlakozzon.](./media/sql-database-connect-excel/excel_data_source.png)
   
   Megnyílik a hello Adatkapcsolat varázsló.
3. A hello **tooDatabase Server csatlakozás** párbeszédpanelen típus hello SQL-adatbázis **kiszolgálónév** tooconnect tooin hello űrlapot <*kiszolgálónév* > **. database.windows.net**. Például: **adworkserver.database.windows.net**.
4. A **azonosító adatok**, kattintson a **a következő felhasználónevet és jelszót használja hello**, típus hello **felhasználónév** és **jelszó** vonatkozóan beállított SQL-adatbáziskiszolgáló hello létrehozása után, és kattintson a **következő**.
   
   ![Írja be a hello kiszolgálónév és hitelesítő adatait](./media/sql-database-connect-excel/connect-to-server.png)
   
   > [!TIP]
   > A hálózati környezettől függően nem fogja tudni tooconnect, vagy ha hello SQL adatbázis-kiszolgáló nem engedélyezi az ügyfél IP-címről érkező forgalom hello kapcsolat megszakad. Nyissa meg toohello [Azure-portálon](https://portal.azure.com/), kattintson az SQL Server, kattintson arra a kiszolgálóra, kattintson a beállítások a tűzfal és az ügyfél IP-cím hozzáadása. Lásd: [hogyan tooconfigure tűzfalbeállítások](sql-database-configure-firewall-settings.md) részleteiről.
   > 
   > 
5. A hello **adatbázis és tábla kijelölése** párbeszédpanelen jelölje be hello adatbázis azt szeretné, hogy a toowork hello listából, és kattintson a hello táblák vagy nézetek azt szeretné, hogy a toowork (választottuk **vGetAllCategories**), majd Kattintson a **következő**.
   
    ![Válasszon ki egy adatbázist és egy táblát.](./media/sql-database-connect-excel/select-database-and-table.png)
   
    Hello **Adatkapcsolatfájl mentése és befejezése** párbeszédpanel megnyitása, ahol meg kell adnia hello Office adatbázis-kapcsolati (*.odc) fájl, amely Excel-használ kapcsolatos információkat. Meghagyhatja hello alapértelmezett beállításokat, vagy testre szabhatja a beállításokat.
6. Meghagyhatja hello alapértelmezett beállításokat, de a Megjegyzés hello **Fájlnév** különösen. A **leírás**, egy **rövid név**, és **keresési kulcsszavakat** segítséget és a többi felhasználónak abban, hogy milyen kapcsolat tooand hello kapcsolat található. Kattintson a **mindig kísérlet toouse a fájladatok toorefresh** Ha kapcsolatadatokat menteni hello odc-fájlt, hogy tooit csatlakozni, és kattintson a kívánt **Befejezés**.
   
    ![ODC-fájl mentése](./media/sql-database-connect-excel/save-odc-file.png)
   
    Hello **adatimportálás** párbeszédpanel jelenik meg.

## <a name="import-hello-data-into-excel-and-create-a-pivot-chart"></a>Hello adatok importálása Excelbe és kimutatásdiagram létrehozása
Most, hogy létrehozta a hello kapcsolat és a létrehozott hello fájl és a kapcsolati adatok, tooimport hello adatok olvasása közben.

1. A hello **és adatokat importálhat** párbeszédpanel, kattintson az adatok megjelenítésére hello munkalapon kívánt hello beállítást, majd **OK**. Válassza a **Kimutatásdiagram** lehetőséget. Másik lehetőségként toocreate egy **új munkalapra lesznek** vagy túl**ezen adatok tooa adatmodell hozzáadása**. Az adatmodellekről további információkat az [Adatmodell létrehozása Excelben](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B) részben talál. Kattintson a **tulajdonságok** tooexplore információ hello ODC-fájl hello előző lépés és toochoose beállításainak kiválasztásáért hello adatok létrehozott.
   
    ![Az Excel adatok formátumát hello kiválasztása](./media/sql-database-connect-excel/import-data.png)
   
    hello munkalapon most már rendelkezik egy üres Kimutatástábla és egy diagram.
2. A **kimutatástábla mezői**, válassza ki az összes hello jelölőnégyzetét hello tooview kívánt mezőket.
   
    ![Konfigurálja az adatbázis-jelentést.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> Tooconnect más Excel-munkafüzeteket és munkalapokat toohello adatbázis, kattintson a **adatok**, kattintson a **kapcsolatok**, kattintson a **Hozzáadás**, válassza ki a létrehozott hello kapcsolatot hello listában, és kattintson a **nyitott**.
> ![Kapcsolat megnyitása másik munkafüzetből](./media/sql-database-connect-excel/open-from-another-workbook.png)
> 
> 

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[tooSQL adatbázis csatlakozzon az SQL Server Management Studio](sql-database-connect-query-ssms.md) speciális lekérdezés és elemzés céljából.
* További tudnivalók a hello előnyeinek [rugalmas készletek](sql-database-elastic-pool.md).
* Ismerje meg, hogyan túl[hozzon létre egy webalkalmazást, amely összeköti a hello háttér-adatbázis tooSQL](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).

