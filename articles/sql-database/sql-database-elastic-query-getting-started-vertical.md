---
title: "aaaGet (a vertikális particionálás) közötti adatbázis-lekérdezések használatába |} Microsoft Docs"
description: "a rugalmas adatbázis-lekérdezés toouse függőleges particionálásáról adatbázisok"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: e5b44b10-c432-4f96-b20e-08615ff4d5dd
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: torsteng
ms.openlocfilehash: 9e6183268e8bf87e3ac28f502711fcc05a7a3f52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Első lépések (a vertikális particionálás) közötti adatbázis-lekérdezések (előzetes verzió)
A rugalmas adatbázis-lekérdezés (előzetes verzió) Azure SQL-adatbázis lehetővé teszi toorun T-SQL lekérdezések, amelyek több több adatbázis egyetlen kapcsolódási pontot. Ez a témakör vonatkozik túl[függőleges particionálva az adatbázisok](sql-database-elastic-query-vertical-partitioning.md).  

Befejezése után lesz: megtudhatja, hogyan tooconfigure és használja az Azure SQL Database tooperform lekérdezi, hogy span több kapcsolódó adatbázis. 

Hello rugalmas adatbázis-lekérdezés szolgáltatással kapcsolatos további információkért lásd: [Azure SQL Database rugalmas adatbázis-lekérdezés áttekintése](sql-database-elastic-query-overview.md). 

## <a name="prerequisites"></a>Előfeltételek

Az ALTER ANY külső ADATFORRÁS engedéllyel kell rendelkeznie. Ez az engedély megtalálható hello ALTER DATABASE engedéllyel. Az ALTER ANY külső ADATFORRÁS-engedélyek az alapul szolgáló adatforrás szükséges toorefer toohello is.

## <a name="create-hello-sample-databases"></a>Hello minta adatbázisok létrehozása
a toostart, igazolnia kell a két adatbázis toocreate, **ügyfelek** és **rendelések**, vagy ugyanazon vagy másik logikai kiszolgáló hello.   

Hajtható végre a következő lekérdezéseket a hello hello **rendelések** adatbázis toocreate hello **OrderInformation** tábla és a bemeneti hello mintaadatok. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Most, hajtható végre a következő lekérdezés a hello **ügyfelek** adatbázis toocreate hello **CustomerInformation** tábla és a bemeneti hello mintaadatok. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Adatbázis-objektumok létrehozása
### <a name="database-scoped-master-key-and-credentials"></a>Adatbázis hatókörű főkulcs és a hitelesítő adatok
1. Nyissa meg az SQL Server Management Studio vagy Visual Studio SQL Server Data Tools összetevővel.
2. Csatlakozás toohello rendelések adatbázist, és hajtsa végre a következő T-SQL parancsokkal hello:
   
        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  
   
    hello "felhasználónév" és a "password" kell hello felhasználónév és jelszó toologin hello ügyfelek adatbázisba.
    Hitelesítés az Azure Active Directoryval rugalmas lekérdezési jelenleg nem támogatott.

### <a name="external-data-sources"></a>Külső adatforrások
egy külső adatforrásból toocreate hello parancs hello rendelések adatbázist a következő hajtható végre: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Külső táblák
Létrehoz egy külső táblát hello rendelések adatbázison, amely megfelel a hello CustomerInformation tábla hello definíciója:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Egy minta rugalmas adatbázis T-SQL-lekérdezés végrehajtása
A külső adatforrást és a külső táblák meghatározása után most már használhatja T-SQL tooquery a külső táblákra. Hello rendelések adatbázison hajtsa végre a lekérdezést: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Költségek
Hello rugalmas adatbázis-lekérdezés szolgáltatása jelenleg szerepel az az Azure SQL Database hello költségét.  

Díjszabási információkért lásd: [SQL Database – díjszabás](https://azure.microsoft.com/pricing/details/sql-database). 

## <a name="next-steps"></a>Következő lépések

* Rugalmas lekérdezési áttekintését lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).
* A szintaxis és a minta lekérdezések függőleges particionált adatok, lásd: [adatok lekérdezése függőleges particionálva)](sql-database-elastic-query-vertical-partitioning.md)
* Vízszintes particionálás (horizontális) oktatóanyagért lásd a [Ismerkedés a vízszintes particionálására (horizontális) rugalmas lekérdezési](sql-database-elastic-query-getting-started.md).
* A szintaxis és a minta lekérdezések vízszintesen particionált adatok, lásd: [adatok vízszintesen lekérdezése particionálva)](sql-database-elastic-query-horizontal-partitioning.md)
* Lásd: [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) tárolt eljárás, amely végrehajtja a Transact-SQL-utasítás egy egyetlen távoli Azure SQL Database vagy az adatbázisok egy vízszintes particionálási sémát a szilánkok szolgál.