---
title: "a Stretch Database - Azure átlátható adattitkosítási aaaEnable |} Microsoft Docs"
description: "Az SQL Server a Stretch Database az Azure-on átlátható adattitkosítás (TDE) engedélyezése"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: barbkess
editor: 
ms.assetid: a44ed8f5-b416-4c41-9b1e-b7271f10bdc3
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: douglasl
ms.openlocfilehash: 1d6bff455030ac8851b2184c1e8097afd61361d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>A Stretch Database az Azure-on átlátható adattitkosítás (TDE) engedélyezése
> [!div class="op_single_selector"]
> * [Azure Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Transzparens Data Encryption (TDE) segítségével anélkül, hogy a módosítások toohello valós idejű titkosítási és visszafejtési hello adatbázis, a társított biztonsági másolatok és a tranzakciós naplófájlok nyugalmi elvégzésével kártevő szándékú tevékenységek hello fenyegetés elleni védelem az alkalmazás.

TDE hello tárolása teljes adatbázis szimmetrikus kulcs hívott hello adatbázis-titkosítási kulcs használatával titkosítja. adatbázis-titkosítási kulcs hello védi egy beépített kiszolgálói tanúsítványt. hello beépített kiszolgálói tanúsítvány egyedi minden Azure-kiszolgálóval rendelkezik. Microsoft legalább 90 naponta automatikusan elforgatja ezeket a tanúsítványokat. TDE általános ismertetését lásd: [átlátszó Data Encryption (TDE)].

## <a name="enabling-encryption"></a>Titkosítás engedélyezése
egy Azure-adatbázis, amely egy SQL Server Stretch-kompatibilis adatbázisból át adatok hello tárolja TDE tooenable hello művelet a következő:

1. Megnyitás hello adatbázis hello [Azure-portálon](https://portal.azure.com)
2. Hello adatbázis paneljén kattintson hello **beállítások** gomb
3. Jelölje be hello **átlátható adattitkosítás** beállítás![][1]
4. Jelölje be hello **a** beállításával, és válassza ki **mentése**
   ![][2]

## <a name="disabling-encryption"></a>Titkosításának letiltása
egy Azure-adatbázis, amely egy SQL Server Stretch-kompatibilis adatbázisból át adatok hello tárolja TDE toodisable hello művelet a következő:

1. Megnyitás hello adatbázis hello [Azure-portálon](https://portal.azure.com)
2. Hello adatbázis paneljén kattintson hello **beállítások** gomb
3. Jelölje be hello **átlátható adattitkosítás** beállítás
4. Jelölje be hello **ki** beállításával, és válassza ki **mentése**

<!--Anchors-->
[átlátszó Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
