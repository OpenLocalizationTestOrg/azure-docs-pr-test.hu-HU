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
# <a name="get-started-with-transparent-data-encryption-tde"></a><span data-ttu-id="61fa5-103">Ismerkedés a transzparens adatok titkosítás (TDE)</span><span class="sxs-lookup"><span data-stu-id="61fa5-103">Get started with Transparent Data Encryption (TDE)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="61fa5-104">Biztonság – áttekintés</span><span class="sxs-lookup"><span data-stu-id="61fa5-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="61fa5-105">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="61fa5-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="61fa5-106">Titkosítás (portál)</span><span class="sxs-lookup"><span data-stu-id="61fa5-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="61fa5-107">Titkosítás (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="61fa5-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a><span data-ttu-id="61fa5-108">Szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="61fa5-108">Required Permssions</span></span>
<span data-ttu-id="61fa5-109">tooenable átlátszó Data Encryption (TDE), a rendszergazda vagy a hello dbmanager szerepkör tagjának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="61fa5-109">tooenable Transparent Data Encryption (TDE), you must be an administrator or a member of hello dbmanager role.</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="61fa5-110">Titkosítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="61fa5-110">Enabling Encryption</span></span>
<span data-ttu-id="61fa5-111">Hajtsa végre az alábbi lépéseket tooenable TDE SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="61fa5-111">Follow these steps tooenable TDE for a SQL Data Warehouse:</span></span>

1. <span data-ttu-id="61fa5-112">Csatlakozás toohello *fő* adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy másik hello hello adatbázisát üzemeltető hello kiszolgálón **dbmanager** hello főadatbázisban szerepkör</span><span class="sxs-lookup"><span data-stu-id="61fa5-112">Connect toohello *master* database on hello server hosting hello database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="61fa5-113">A következő utasítás tooencrypt hello adatbázis hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="61fa5-113">Execute hello following statement tooencrypt hello database.</span></span>

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a><span data-ttu-id="61fa5-114">Titkosításának letiltása</span><span class="sxs-lookup"><span data-stu-id="61fa5-114">Disabling Encryption</span></span>
<span data-ttu-id="61fa5-115">Hajtsa végre az alábbi lépéseket toodisable TDE SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="61fa5-115">Follow these steps toodisable TDE for a SQL Data Warehouse:</span></span>

1. <span data-ttu-id="61fa5-116">Csatlakozás toohello *fő* adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy másik hello **dbmanager** hello főadatbázisban szerepkör</span><span class="sxs-lookup"><span data-stu-id="61fa5-116">Connect toohello *master* database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="61fa5-117">A következő utasítás tooencrypt hello adatbázis hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="61fa5-117">Execute hello following statement tooencrypt hello database.</span></span>

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> <span data-ttu-id="61fa5-118">Felfüggesztett SQL Data Warehouse toohello TDE beállítások módosítása előtt kell folytatni.</span><span class="sxs-lookup"><span data-stu-id="61fa5-118">A paused SQL Data Warehouse must be resumed before making changes toohello TDE settings.</span></span>
> 
> 

## <a name="verifying-encryption"></a><span data-ttu-id="61fa5-119">Titkosítási ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="61fa5-119">Verifying Encryption</span></span>
<span data-ttu-id="61fa5-120">tooverify titkosítás állapotát az SQL Data Warehouse, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="61fa5-120">tooverify encryption status for a SQL Data Warehouse, follow hello steps below:</span></span>

1. <span data-ttu-id="61fa5-121">Csatlakozás toohello *fő* vagy példány adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy másik hello **dbmanager** hello főadatbázisban szerepkör</span><span class="sxs-lookup"><span data-stu-id="61fa5-121">Connect toohello *master* or instance database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="61fa5-122">A következő utasítás tooencrypt hello adatbázis hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="61fa5-122">Execute hello following statement tooencrypt hello database.</span></span>

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

<span data-ttu-id="61fa5-123">Eredménye ```1``` jelzi egy titkosított adatbázis ```0``` nem titkosított adatbázis jelzi.</span><span class="sxs-lookup"><span data-stu-id="61fa5-123">A result of ```1``` indicates an encrypted database, ```0``` indicates a non-encrypted database.</span></span>

## <a name="encryption-dmvs"></a><span data-ttu-id="61fa5-124">Titkosítási dinamikus felügyeleti nézetek</span><span class="sxs-lookup"><span data-stu-id="61fa5-124">Encryption DMVs</span></span>
* <span data-ttu-id="61fa5-125">[sys.Databases][sys.databases]</span><span class="sxs-lookup"><span data-stu-id="61fa5-125">[sys.databases][sys.databases]</span></span> 
* <span data-ttu-id="61fa5-126">[sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]</span><span class="sxs-lookup"><span data-stu-id="61fa5-126">[sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]</span></span>

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
