---
title: aaaConnect tooAzure SQL Data Warehouse sqlcmd |} Microsoft Docs
description: "[Sqlcmd] [sqlcmd] parancssori segédprogram tooconnect tooand lekérdezés Azure SQL Data Warehouse használata."
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 6e2b69e5-4806-4e91-9ea1-e2b63bf28c46
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 0334df7b969da1966ba29c97f835a2dc9e383e29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-sqlcmd"></a>Adatraktár tooSQL csatlakozni az Sqlcmd használatával
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Használjon [sqlcmd] [ sqlcmd] parancssori segédprogram tooconnect tooand lekérdezése az Azure SQL Data Warehouse.  

## <a name="1-connect"></a>1. Kapcsolódás
tooget használatába [sqlcmd][sqlcmd], nyissa meg a hello parancssort, és írja be **sqlcmd** hello kapcsolati karakterláncot az SQL Data Warehouse-adatbázis követ. hello kapcsolati karakterlánc a következő paraméterek hello van szükség:

* **Server (-S):** hello kiszolgálónak `<`kiszolgálónév`>`. database.windows.net
* **Database (-d):** Az adatbázis neve.
* **Enable Quoted Identifiers (-I):** Határolójeles azonosítókat engedélyezett tooconnect tooa SQL Data Warehouse példánynak kell lennie.

SQL Server-hitelesítés toouse, kell tooadd hello felhasználónév/jelszó Paraméterek:

* **Felhasználó (-U):** kiszolgálói felhasználó hello formában `<`felhasználó`>`
* **Jelszó (-P):** hello felhasználói társított jelszót.

A kapcsolati karakterlánc például hello következő látható:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

toouse Azure Active Directory integrált hitelesítést, tooadd hello Azure Active Directory paraméterek szükségesek:

* **Azure Active Directory Authentication (-G):** az Azure Active Directory használata a hitelesítéshez

A kapcsolati karakterlánc például hello következő látható:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Túl kell[engedélyezése az Azure Active Directory-hitelesítéssel](sql-data-warehouse-authentication.md) tooauthenticate Active Directory használatával.
> 
> 

## <a name="2-query"></a>2. Lekérdezés
Kapcsolódás után bármely támogatott Transact-SQL utasítások ellenőrzés hello adhat ki.  Ebben a példában a lekérdezések elküldése interaktív módban történik.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

A következő példák azt szemléltetik, hogyan futtathat a lekérdezések hello -Q beállítással, vagy az SQL toosqlcmd ismertetett kötegelt módban.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Következő lépések
Lásd: [sqlcmd dokumentációjában olvashat] [ sqlcmd] további információk az sqlcmd elérhető hello lehetőségeinek részleteit.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
