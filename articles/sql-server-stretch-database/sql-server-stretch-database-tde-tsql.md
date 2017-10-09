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
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a><span data-ttu-id="8dde6-103">A Stretch Database az Azure (Transact-SQL) átlátható adattitkosítás (TDE) engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8dde6-103">Enable Transparent Data Encryption (TDE) for Stretch Database on Azure (Transact-SQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8dde6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8dde6-104">Azure portal</span></span>](sql-server-stretch-database-encryption-tde.md)
> * [<span data-ttu-id="8dde6-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="8dde6-105">TSQL</span></span>](sql-server-stretch-database-tde-tsql.md)
>
>

<span data-ttu-id="8dde6-106">Transzparens Data Encryption (TDE) segítségével anélkül, hogy a módosítások toohello valós idejű titkosítási és visszafejtési hello adatbázis, a társított biztonsági másolatok és a tranzakciós naplófájlok nyugalmi elvégzésével kártevő szándékú tevékenységek hello fenyegetés elleni védelem az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8dde6-106">Transparent Data Encryption (TDE) helps protect against hello threat of malicious activity by performing real-time encryption and decryption of hello database, associated backups, and transaction log files at rest without requiring changes toohello application.</span></span>

<span data-ttu-id="8dde6-107">TDE hello tárolása teljes adatbázis szimmetrikus kulcs hívott hello adatbázis-titkosítási kulcs használatával titkosítja.</span><span class="sxs-lookup"><span data-stu-id="8dde6-107">TDE encrypts hello storage of an entire database by using a symmetric key called hello database encryption key.</span></span> <span data-ttu-id="8dde6-108">adatbázis-titkosítási kulcs hello védi egy beépített kiszolgálói tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="8dde6-108">hello database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="8dde6-109">hello beépített kiszolgálói tanúsítvány egyedi minden Azure-kiszolgálóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8dde6-109">hello built-in server certificate is unique for each Azure server.</span></span> <span data-ttu-id="8dde6-110">Microsoft legalább 90 naponta automatikusan elforgatja ezeket a tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="8dde6-110">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="8dde6-111">TDE általános ismertetését lásd: [átlátszó Data Encryption (TDE)].</span><span class="sxs-lookup"><span data-stu-id="8dde6-111">For a general description of TDE, see [Transparent Data Encryption (TDE)].</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="8dde6-112">Titkosítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8dde6-112">Enabling Encryption</span></span>
<span data-ttu-id="8dde6-113">egy Azure-adatbázis, amely egy SQL Server Stretch-kompatibilis adatbázisból át adatok hello tárolja TDE tooenable hello művelet a következő:</span><span class="sxs-lookup"><span data-stu-id="8dde6-113">tooenable TDE for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="8dde6-114">Csatlakozás toohello *fő* hello Azure-kiszolgáló üzemeltetési hello adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy másik hello adatbázis **dbmanager** hello főadatbázisban szerepkör</span><span class="sxs-lookup"><span data-stu-id="8dde6-114">Connect toohello *master* database on hello Azure server hosting hello database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="8dde6-115">A következő utasítás tooencrypt hello adatbázis hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="8dde6-115">Execute hello following statement tooencrypt hello database.</span></span>

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a><span data-ttu-id="8dde6-116">Titkosításának letiltása</span><span class="sxs-lookup"><span data-stu-id="8dde6-116">Disabling Encryption</span></span>
<span data-ttu-id="8dde6-117">egy Azure-adatbázis, amely egy SQL Server Stretch-kompatibilis adatbázisból át adatok hello tárolja TDE toodisable hello művelet a következő:</span><span class="sxs-lookup"><span data-stu-id="8dde6-117">toodisable TDE for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="8dde6-118">Csatlakozás toohello *fő* adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy másik hello **dbmanager** hello főadatbázisban szerepkör</span><span class="sxs-lookup"><span data-stu-id="8dde6-118">Connect toohello *master* database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="8dde6-119">A következő utasítás tooencrypt hello adatbázis hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="8dde6-119">Execute hello following statement tooencrypt hello database.</span></span>

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a><span data-ttu-id="8dde6-120">Titkosítási ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="8dde6-120">Verifying Encryption</span></span>
<span data-ttu-id="8dde6-121">egy Azure-adatbázis, amely egy SQL Server Stretch-kompatibilis adatbázisból át adatok hello tárolja tooverify titkosítási állapot hello művelet a következő:</span><span class="sxs-lookup"><span data-stu-id="8dde6-121">tooverify encryption status for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="8dde6-122">Csatlakozás toohello *fő* vagy példány adatbázis használata a bejelentkezési azonosítót, amely a rendszergazda vagy egy másik hello **dbmanager** hello főadatbázisban szerepkör</span><span class="sxs-lookup"><span data-stu-id="8dde6-122">Connect toohello *master* or instance database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="8dde6-123">A következő utasítás tooencrypt hello adatbázis hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="8dde6-123">Execute hello following statement tooencrypt hello database.</span></span>

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

<span data-ttu-id="8dde6-124">Eredménye ```1``` jelzi egy titkosított adatbázis ```0``` nem titkosított adatbázis jelzi.</span><span class="sxs-lookup"><span data-stu-id="8dde6-124">A result of ```1``` indicates an encrypted database, ```0``` indicates a non-encrypted database.</span></span>

<!--Anchors-->
[átlátszó Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
