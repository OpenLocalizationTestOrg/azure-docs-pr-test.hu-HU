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
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a><span data-ttu-id="05943-103">A Stretch Database az Azure-on átlátható adattitkosítás (TDE) engedélyezése</span><span class="sxs-lookup"><span data-stu-id="05943-103">Enable Transparent Data Encryption (TDE) for Stretch Database on Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="05943-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="05943-104">Azure portal</span></span>](sql-server-stretch-database-encryption-tde.md)
> * [<span data-ttu-id="05943-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="05943-105">TSQL</span></span>](sql-server-stretch-database-tde-tsql.md)
>
>

<span data-ttu-id="05943-106">Transzparens Data Encryption (TDE) segítségével anélkül, hogy a módosítások toohello valós idejű titkosítási és visszafejtési hello adatbázis, a társított biztonsági másolatok és a tranzakciós naplófájlok nyugalmi elvégzésével kártevő szándékú tevékenységek hello fenyegetés elleni védelem az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="05943-106">Transparent Data Encryption (TDE) helps protect against hello threat of malicious activity by performing real-time encryption and decryption of hello database, associated backups, and transaction log files at rest without requiring changes toohello application.</span></span>

<span data-ttu-id="05943-107">TDE hello tárolása teljes adatbázis szimmetrikus kulcs hívott hello adatbázis-titkosítási kulcs használatával titkosítja.</span><span class="sxs-lookup"><span data-stu-id="05943-107">TDE encrypts hello storage of an entire database by using a symmetric key called hello database encryption key.</span></span> <span data-ttu-id="05943-108">adatbázis-titkosítási kulcs hello védi egy beépített kiszolgálói tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="05943-108">hello database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="05943-109">hello beépített kiszolgálói tanúsítvány egyedi minden Azure-kiszolgálóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="05943-109">hello built-in server certificate is unique for each Azure server.</span></span> <span data-ttu-id="05943-110">Microsoft legalább 90 naponta automatikusan elforgatja ezeket a tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="05943-110">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="05943-111">TDE általános ismertetését lásd: [átlátszó Data Encryption (TDE)].</span><span class="sxs-lookup"><span data-stu-id="05943-111">For a general description of TDE, see [Transparent Data Encryption (TDE)].</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="05943-112">Titkosítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="05943-112">Enabling Encryption</span></span>
<span data-ttu-id="05943-113">egy Azure-adatbázis, amely egy SQL Server Stretch-kompatibilis adatbázisból át adatok hello tárolja TDE tooenable hello művelet a következő:</span><span class="sxs-lookup"><span data-stu-id="05943-113">tooenable TDE for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="05943-114">Megnyitás hello adatbázis hello [Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="05943-114">Open hello database in hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="05943-115">Hello adatbázis paneljén kattintson hello **beállítások** gomb</span><span class="sxs-lookup"><span data-stu-id="05943-115">In hello database blade, click hello **Settings** button</span></span>
3. <span data-ttu-id="05943-116">Jelölje be hello **átlátható adattitkosítás** beállítás![][1]</span><span class="sxs-lookup"><span data-stu-id="05943-116">Select hello **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="05943-117">Jelölje be hello **a** beállításával, és válassza ki **mentése**
   ![][2]</span><span class="sxs-lookup"><span data-stu-id="05943-117">Select hello **On** setting, and then select **Save**
![][2]</span></span>

## <a name="disabling-encryption"></a><span data-ttu-id="05943-118">Titkosításának letiltása</span><span class="sxs-lookup"><span data-stu-id="05943-118">Disabling Encryption</span></span>
<span data-ttu-id="05943-119">egy Azure-adatbázis, amely egy SQL Server Stretch-kompatibilis adatbázisból át adatok hello tárolja TDE toodisable hello művelet a következő:</span><span class="sxs-lookup"><span data-stu-id="05943-119">toodisable TDE for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="05943-120">Megnyitás hello adatbázis hello [Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="05943-120">Open hello database in hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="05943-121">Hello adatbázis paneljén kattintson hello **beállítások** gomb</span><span class="sxs-lookup"><span data-stu-id="05943-121">In hello database blade, click hello **Settings** button</span></span>
3. <span data-ttu-id="05943-122">Jelölje be hello **átlátható adattitkosítás** beállítás</span><span class="sxs-lookup"><span data-stu-id="05943-122">Select hello **Transparent data encryption** option</span></span>
4. <span data-ttu-id="05943-123">Jelölje be hello **ki** beállításával, és válassza ki **mentése**</span><span class="sxs-lookup"><span data-stu-id="05943-123">Select hello **Off** setting, and then select **Save**</span></span>

<!--Anchors-->
[átlátszó Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
