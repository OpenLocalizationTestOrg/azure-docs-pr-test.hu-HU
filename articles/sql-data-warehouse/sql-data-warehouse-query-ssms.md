---
title: aaaConnect tooAzure SQL Data Warehouse - SSMS |} Microsoft Docs
description: "Használja az SQL Server Management Studio (SSMS) tooconnect tooand lekérdezést Azure SQL Data warehouse-bA."
services: sql-data-warehouse
documentationcenter: 
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 299e50b3-e68a-471c-8aee-b0b9874781bd
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: bcbaf7139d2e5183b388b8d58c015cf5ad726722
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-sql-server-management-studio-ssms"></a>Csatlakozás tooSQL adatraktár SQL Server Management Studio (SSMS)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Használja az SQL Server Management Studio (SSMS) tooconnect tooand lekérdezést Azure SQL Data warehouse-bA. 

## <a name="prerequisites"></a>Előfeltételek
toouse ebben az oktatóanyagban szüksége:

* Egy létező SQL Data Warehouse. toocreate, lásd: [SQL Data Warehouse létrehozása][Create a SQL Data Warehouse].
* SQL Server Management Studio (SSMS) telepítve. [Telepítse az SSMS] [ Install SSMS] szabad, ha már nincs.
* hello teljesen minősített SQL-kiszolgáló neve. toofind a, lásd: [csatlakozzon az adatraktár tooSQL][Connect tooSQL Data Warehouse].

## <a name="1-connect-tooyour-sql-data-warehouse"></a>1. Csatlakozás az SQL Data Warehouse tooyour
1. Nyissa meg a szolgáltatáshoz az SSMS.
2. Nyissa meg az Object Explorert. toodo a, válassza ki **fájl** > **Object Explorerben csatlakozzon**.
   
    ![SQL Server Object Explorer][1]
3. Hello Connect tooServer ablakban hello mezők kitöltésével.
   
    ![Csatlakozás tooServer][2]
   
   * **Kiszolgálónév**. Adja meg a hello **kiszolgálónév** korábban azonosított.
   * **Hitelesítés**. Válassza az **SQL Server Authentication** (SQL Server-hitelesítés) vagy az **Active Directory Integrated Authentication** (Active Directory beépített hitelesítés) lehetőséget.
   * **Felhasználónév** és **Jelszó**. Amennyiben az SQL Server-hitelesítést választotta, adja meg felhasználónevét és jelszavát.
   * Kattintson a **Connect** (Csatlakozás) gombra.
4. tooexplore, bontsa ki az Azure SQL-kiszolgálót. Hello kiszolgálóhoz társított hello adatbázisok tekintheti meg. Bontsa ki az AdventureWorksDW toosee hello táblák a mintaadatbázis.
   
    ![Az AdventureWorksDW áttekintése][3]

## <a name="2-run-a-sample-query"></a>2. Mintalekérdezés futtatása
Most, hogy a kapcsolat már meglévő tooyour adatbázis, ideje lefuttatni egy lekérdezést.

1. Kattintson a jobb gombbal az adatbázisára az SQL Server Object Explorer alatt.
2. Válassza a **New Query** (Új lekérdezés) lehetőséget. Megnyílik egy új lekérdezési ablak.
   
    ![Új lekérdezés][4]
3. Másolja a TSQL-lekérdezést hello lekérdezési ablakba:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Hello lekérdezés futtatása. toodo, kattintson `Execute` vagy hello használja a következő helyi: `F5`.
   
    ![A lekérdezés futtatása][5]
5. Tekintse meg hello lekérdezés eredményeit. Ebben a példában a hello FactInternetSales táblának 60 398 sora van.
   
    ![Lekérdezés eredményei][6]

## <a name="next-steps"></a>Következő lépések
Most, hogy kapcsolódási, és a lekérdezési, próbálja [megjeleníteni hello adatokat a powerbi-jal][visualizing hello data with PowerBI].

tooconfigure a környezetet az Azure Active Directory-hitelesítés, lásd: [tooSQL adatraktár hitelesítéséhez][Authenticate tooSQL Data Warehouse].

<!--Arcticles-->
[Connect tooSQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authenticate tooSQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing hello data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

<!--Other-->
[Azure portal]: https://portal.azure.com
[Install SSMS]: https://msdn.microsoft.com/en-US/library/hh213248.aspx


<!--Image references-->

[1]: media/sql-data-warehouse-query-ssms/connect-object-explorer.png
[2]: media/sql-data-warehouse-query-ssms/connect-object-explorer1.png
[3]: media/sql-data-warehouse-query-ssms/explore-tables.png
[4]: media/sql-data-warehouse-query-ssms/new-query.png
[5]: media/sql-data-warehouse-query-ssms/execute-query.png
[6]: media/sql-data-warehouse-query-ssms/results.png
