---
title: "Az SQL Data Warehouse (T-SQL) átlátható adattitkosítás |} Microsoft Docs"
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
ms.openlocfilehash: 74c9032aababdce91ed617cd7a4c628915b42504
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-transparent-data-encryption-tde"></a><span data-ttu-id="c7b3f-103">Ismerkedés a transzparens adatok titkosítás (TDE)</span><span class="sxs-lookup"><span data-stu-id="c7b3f-103">Get started with Transparent Data Encryption (TDE)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c7b3f-104">Biztonság – áttekintés</span><span class="sxs-lookup"><span data-stu-id="c7b3f-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="c7b3f-105">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="c7b3f-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="c7b3f-106">Titkosítás (portál)</span><span class="sxs-lookup"><span data-stu-id="c7b3f-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="c7b3f-107">Titkosítás (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="c7b3f-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a><span data-ttu-id="c7b3f-108">Szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="c7b3f-108">Required Permssions</span></span>
<span data-ttu-id="c7b3f-109">Ahhoz, hogy az átlátszó Data Encryption (TDE), a rendszergazda vagy a dbmanager szerepkör tagjának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c7b3f-109">To enable Transparent Data Encryption (TDE), you must be an administrator or a member of the dbmanager role.</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="c7b3f-110">Titkosítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c7b3f-110">Enabling Encryption</span></span>
<span data-ttu-id="c7b3f-111">Kövesse az alábbi lépéseket az SQL Data Warehouse TDE engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="c7b3f-111">Follow these steps to enable TDE for a SQL Data Warehouse:</span></span>

1. <span data-ttu-id="c7b3f-112">Csatlakozás a *fő* adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy másik adatbázist futtató kiszolgálón a **dbmanager** a master adatbázisban szerepkör</span><span class="sxs-lookup"><span data-stu-id="c7b3f-112">Connect to the *master* database on the server hosting the database using a login that is an administrator or a member of the **dbmanager** role in the master database</span></span>
2. <span data-ttu-id="c7b3f-113">Hajtsa végre a következő utasítást az adatbázis titkosításához.</span><span class="sxs-lookup"><span data-stu-id="c7b3f-113">Execute the following statement to encrypt the database.</span></span>

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a><span data-ttu-id="c7b3f-114">Titkosításának letiltása</span><span class="sxs-lookup"><span data-stu-id="c7b3f-114">Disabling Encryption</span></span>
<span data-ttu-id="c7b3f-115">Kövesse az alábbi lépéseket az SQL Data Warehouse TDE letiltása:</span><span class="sxs-lookup"><span data-stu-id="c7b3f-115">Follow these steps to disable TDE for a SQL Data Warehouse:</span></span>

1. <span data-ttu-id="c7b3f-116">Csatlakozás a *fő* adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy tagja a **dbmanager** a master adatbázisban szerepkör</span><span class="sxs-lookup"><span data-stu-id="c7b3f-116">Connect to the *master* database using a login that is an administrator or a member of the **dbmanager** role in the master database</span></span>
2. <span data-ttu-id="c7b3f-117">Hajtsa végre a következő utasítást az adatbázis titkosításához.</span><span class="sxs-lookup"><span data-stu-id="c7b3f-117">Execute the following statement to encrypt the database.</span></span>

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> <span data-ttu-id="c7b3f-118">Felfüggesztett SQL Data Warehouse a TDE beállítások módosítása előtt kell folytatni.</span><span class="sxs-lookup"><span data-stu-id="c7b3f-118">A paused SQL Data Warehouse must be resumed before making changes to the TDE settings.</span></span>
> 
> 

## <a name="verifying-encryption"></a><span data-ttu-id="c7b3f-119">Titkosítási ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c7b3f-119">Verifying Encryption</span></span>
<span data-ttu-id="c7b3f-120">SQL Data Warehouse titkosítási állapot ellenőrzéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c7b3f-120">To verify encryption status for a SQL Data Warehouse, follow the steps below:</span></span>

1. <span data-ttu-id="c7b3f-121">Csatlakozás a *fő* vagy egy bejelentkezési azonosítót, amely a rendszergazda vagy egy másik példány adatbázist a **dbmanager** a master adatbázisban szerepkör</span><span class="sxs-lookup"><span data-stu-id="c7b3f-121">Connect to the *master* or instance database using a login that is an administrator or a member of the **dbmanager** role in the master database</span></span>
2. <span data-ttu-id="c7b3f-122">Hajtsa végre a következő utasítást az adatbázis titkosításához.</span><span class="sxs-lookup"><span data-stu-id="c7b3f-122">Execute the following statement to encrypt the database.</span></span>

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

<span data-ttu-id="c7b3f-123">Eredménye ```1``` jelzi egy titkosított adatbázis ```0``` nem titkosított adatbázis jelzi.</span><span class="sxs-lookup"><span data-stu-id="c7b3f-123">A result of ```1``` indicates an encrypted database, ```0``` indicates a non-encrypted database.</span></span>

## <a name="encryption-dmvs"></a><span data-ttu-id="c7b3f-124">Titkosítási dinamikus felügyeleti nézetek</span><span class="sxs-lookup"><span data-stu-id="c7b3f-124">Encryption DMVs</span></span>
* <span data-ttu-id="c7b3f-125">[sys.Databases][sys.databases]</span><span class="sxs-lookup"><span data-stu-id="c7b3f-125">[sys.databases][sys.databases]</span></span> 
* <span data-ttu-id="c7b3f-126">[sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]</span><span class="sxs-lookup"><span data-stu-id="c7b3f-126">[sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]</span></span>

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
