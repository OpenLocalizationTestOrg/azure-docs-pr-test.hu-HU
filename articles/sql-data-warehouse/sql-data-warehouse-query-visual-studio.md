---
title: aaaConnect tooAzure SQL Data Warehouse - VSTS |} Microsoft Docs
description: "Az SQL Data Warehouse lekérdezése a Visual Studióval."
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: daace889-95e5-4826-b2fc-047eac9d6d95
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 55eef4dff3e0647be5a735295bc89b43eb456079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-visual-studio-and-ssdt"></a>Visual Studio és az SSDT tooSQL adatraktár csatlakozás
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Használja a Visual Studio tooquery Azure SQL Data Warehouse csak néhány perc múlva. Ez a módszer hello SQL Server Data Tools (SSDT) bővítményt a Visual Studio használja. 

## <a name="prerequisites"></a>Előfeltételek
toouse ebben az oktatóanyagban szüksége:

* Egy létező SQL Data Warehouse. toocreate, lásd: [SQL Data Warehouse létrehozása][Create a SQL Data Warehouse].
* SSDT a Visual Studióhoz. Ha rendelkezik a Visual Studióval, akkor valószínűleg már ezzel is. A telepítés menetéről és a beállításokról [a Visual Studio és az SSDT telepítését][Installing Visual Studio and SSDT] ismertető cikkben olvashat bővebben.
* hello teljesen minősített SQL-kiszolgáló neve. toofind a, lásd: [csatlakozzon az adatraktár tooSQL][Connect tooSQL Data Warehouse].

## <a name="1-connect-tooyour-sql-data-warehouse"></a>1. Csatlakozás az SQL Data Warehouse tooyour
1. Nyissa meg a Visual Studio 2013-at vagy 2015-öt.
2. Nyissa meg az SQL Server Object Explorert. toodo a, válassza ki **nézet** > **SQL Server Object Explorer**.
   
    ![SQL Server Object Explorer][1]
3. Kattintson a hello **SQL-kiszolgáló hozzáadása** ikonra.
   
    ![SQL Server hozzáadása][2]
4. Hello Connect tooServer ablakban hello mezők kitöltésével.
   
    ![Csatlakozás tooServer][3]
   
   * **Kiszolgálónév**. Adja meg a hello **kiszolgálónév** korábban azonosított.
   * **Hitelesítés**. Válassza az **SQL Server Authentication** (SQL Server-hitelesítés) vagy az **Active Directory Integrated Authentication** (Active Directory beépített hitelesítés) lehetőséget.
   * **Felhasználónév** és **Jelszó**. Amennyiben az SQL Server-hitelesítést választotta, adja meg felhasználónevét és jelszavát.
   * Kattintson a **Connect** (Csatlakozás) gombra.
5. tooexplore, bontsa ki az Azure SQL-kiszolgálót. Hello kiszolgálóhoz társított hello adatbázisok tekintheti meg. Bontsa ki az AdventureWorksDW toosee hello táblák a mintaadatbázis.
   
    ![Az AdventureWorksDW áttekintése][4]

## <a name="2-run-a-sample-query"></a>2. Mintalekérdezés futtatása
Most, hogy a kapcsolat már meglévő tooyour adatbázis, ideje lefuttatni egy lekérdezést.

1. Kattintson a jobb gombbal az adatbázisára az SQL Server Object Explorer alatt.
2. Válassza a **New Query** (Új lekérdezés) lehetőséget. Megnyílik egy új lekérdezési ablak.
   
    ![Új lekérdezés][5]
3. Másolja a TSQL-lekérdezést hello lekérdezési ablakba:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Hello lekérdezés futtatása. toodo, hello zöld nyílra, vagy használja a következő helyi hello: `CTRL` + `SHIFT` + `E`.
   
    ![A lekérdezés futtatása][6]
5. Tekintse meg hello lekérdezés eredményeit. Ebben a példában a hello FactInternetSales táblának 60 398 sora van.
   
    ![Lekérdezés eredményei][7]

## <a name="next-steps"></a>Következő lépések
Most, hogy kapcsolódási, és a lekérdezési, próbálja [megjeleníteni hello adatokat a powerbi-jal][visualizing hello data with PowerBI].

tooconfigure a környezetet az Azure Active Directory-hitelesítés, lásd: [tooSQL adatraktár hitelesítéséhez][Authenticate tooSQL Data Warehouse].

<!--Arcticles-->
[Connect tooSQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Authenticate tooSQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing hello data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
