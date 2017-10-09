---
title: "Átlátható adattitkosítás a Stretch Database TSQL - Azure aaaEnable |} Microsoft Docs"
description: "Az SQL Server Stretch Database Azure TSQL átlátható adattitkosítás (TDE) engedélyezése"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 27753d91-9ca2-4d47-b34d-b5e2c2f029bb
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: anvang
ms.openlocfilehash: a9ba23649656fb344480d79438a1115f0eb353bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>A Stretch Database az Azure (Transact-SQL) átlátható adattitkosítás (TDE) engedélyezése
> [!div class="op_single_selector"]
> * [Azure Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Transzparens Data Encryption (TDE) segítségével anélkül, hogy a módosítások toohello valós idejű titkosítási és visszafejtési hello adatbázis, a társított biztonsági másolatok és a tranzakciós naplófájlok nyugalmi elvégzésével kártevő szándékú tevékenységek hello fenyegetés elleni védelem az alkalmazás.

TDE hello tárolása teljes adatbázis szimmetrikus kulcs hívott hello adatbázis-titkosítási kulcs használatával titkosítja. adatbázis-titkosítási kulcs hello védi egy beépített kiszolgálói tanúsítványt. hello beépített kiszolgálói tanúsítvány egyedi minden Azure-kiszolgálóval rendelkezik. Microsoft legalább 90 naponta automatikusan elforgatja ezeket a tanúsítványokat. TDE általános ismertetését lásd: [átlátszó Data Encryption (TDE)].

## <a name="enabling-encryption"></a>Titkosítás engedélyezése
egy Azure-adatbázis, amely egy SQL Server Stretch-kompatibilis adatbázisból át adatok hello tárolja TDE tooenable hello művelet a következő:

1. Csatlakozás toohello *fő* hello Azure-kiszolgáló üzemeltetési hello adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy másik hello adatbázis **dbmanager** hello főadatbázisban szerepkör
2. A következő utasítás tooencrypt hello adatbázis hello hajtható végre.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Titkosításának letiltása
egy Azure-adatbázis, amely egy SQL Server Stretch-kompatibilis adatbázisból át adatok hello tárolja TDE toodisable hello művelet a következő:

1. Csatlakozás toohello *fő* adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy másik hello **dbmanager** hello főadatbázisban szerepkör
2. A következő utasítás tooencrypt hello adatbázis hello hajtható végre.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a>Titkosítási ellenőrzése
egy Azure-adatbázis, amely egy SQL Server Stretch-kompatibilis adatbázisból át adatok hello tárolja tooverify titkosítási állapot hello művelet a következő:

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

<!--Anchors-->
[átlátszó Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
