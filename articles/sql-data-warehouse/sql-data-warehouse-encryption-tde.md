---
title: "az SQL Data Warehouse (portál) adattitkosítás aaaTransparent |} Microsoft Docs"
description: "Az SQL Data Warehouse átlátható adattitkosítás (TDE)"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: fabf75d3-9bbf-4e0d-9b31-8b5a8713f08d
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 8233886ecf170844104e0d1459e2a829cafa9b8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Az átlátszó adatok titkosítás (TDE) az SQL Data Warehouse első lépései
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
az SQL Data Warehouse, TDE tooenable kövesse hello alábbi lépéseket:

1. Megnyitás hello adatbázis hello [Azure-portálon](https://portal.azure.com)
2. Hello adatbázis paneljén kattintson hello **beállítások** gomb
3. Jelölje be hello **átlátható adattitkosítás** beállítás![][1]
4. Jelölje be hello **a** beállítás![][2]
5. Válassza ki **mentése**
   ![][3]  

## <a name="disabling-encryption"></a>Titkosításának letiltása
az SQL Data Warehouse, TDE toodisable kövesse hello alábbi lépéseket:

1. Megnyitás hello adatbázis hello [Azure-portálon](https://portal.azure.com)
2. Hello adatbázis paneljén kattintson hello **beállítások** gomb
3. Jelölje be hello **átlátható adattitkosítás** beállítás![][1]
4. Jelölje be hello **ki** beállítás![][4]
5. Válassza ki **mentése**
   ![][5]  

## <a name="encryption-dmvs"></a>Titkosítási dinamikus felügyeleti nézetek
A következő dinamikus felügyeleti nézetek hello titkosítási erősíthető meg:

* [sys.Databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
