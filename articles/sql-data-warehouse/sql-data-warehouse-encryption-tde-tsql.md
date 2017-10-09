---
title: "az SQL Data Warehouse (T-SQL) adattitkosítás aaaTransparent |} Microsoft Docs"
description: "Az SQL Data Warehouse (T-SQL) átlátható adattitkosítás (TDE)"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: barbkess
editor: 
ms.assetid: 8ccefef3-1308-41ee-b336-5e491d1098ae
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 3894431c76f14b217f3a6b9a42dbf2f4d216bad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Ismerkedés a transzparens adatok titkosítás (TDE)
> [!div class="op_single_selector"]
> * [Biztonság – áttekintés](sql-data-warehouse-overview-manage-security.md)
> * [Hitelesítés](sql-data-warehouse-authentication.md)
> * [Titkosítás (portál)](sql-data-warehouse-encryption-tde.md)
> * [Titkosítás (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Szükséges engedélyek
tooenable átlátszó Data Encryption (TDE), a rendszergazda vagy a hello dbmanager szerepkör tagjának kell lennie.

## <a name="enabling-encryption"></a>Titkosítás engedélyezése
Hajtsa végre az alábbi lépéseket tooenable TDE SQL Data Warehouse:

1. Csatlakozás toohello *fő* adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy másik hello hello adatbázisát üzemeltető hello kiszolgálón **dbmanager** hello főadatbázisban szerepkör
2. A következő utasítás tooencrypt hello adatbázis hello hajtható végre.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Titkosításának letiltása
Hajtsa végre az alábbi lépéseket toodisable TDE SQL Data Warehouse:

1. Csatlakozás toohello *fő* adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy másik hello **dbmanager** hello főadatbázisban szerepkör
2. A következő utasítás tooencrypt hello adatbázis hello hajtható végre.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Felfüggesztett SQL Data Warehouse toohello TDE beállítások módosítása előtt kell folytatni.
> 
> 

## <a name="verifying-encryption"></a>Titkosítási ellenőrzése
tooverify titkosítás állapotát az SQL Data Warehouse, kövesse az alábbi hello lépéseket:

1. Csatlakozás toohello *fő* vagy példány adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy másik hello **dbmanager** hello főadatbázisban szerepkör
2. A következő utasítás tooencrypt hello adatbázis hello hajtható végre.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Eredménye ```1``` jelzi egy titkosított adatbázis ```0``` nem titkosított adatbázis jelzi.

## <a name="encryption-dmvs"></a>Titkosítási dinamikus felügyeleti nézetek
* [sys.Databases][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
