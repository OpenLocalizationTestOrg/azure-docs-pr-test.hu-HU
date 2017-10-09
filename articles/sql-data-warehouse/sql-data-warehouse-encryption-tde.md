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
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a><span data-ttu-id="c0c00-103">Az átlátszó adatok titkosítás (TDE) az SQL Data Warehouse első lépései</span><span class="sxs-lookup"><span data-stu-id="c0c00-103">Get started with Transparent Data Encryption (TDE) in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c0c00-104">Biztonság – áttekintés</span><span class="sxs-lookup"><span data-stu-id="c0c00-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="c0c00-105">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="c0c00-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="c0c00-106">Titkosítás (portál)</span><span class="sxs-lookup"><span data-stu-id="c0c00-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="c0c00-107">Titkosítás (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="c0c00-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a><span data-ttu-id="c0c00-108">Szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="c0c00-108">Required Permssions</span></span>
<span data-ttu-id="c0c00-109">tooenable átlátszó Data Encryption (TDE), a rendszergazda vagy a hello dbmanager szerepkör tagjának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c0c00-109">tooenable Transparent Data Encryption (TDE), you must be an administrator or a member of hello dbmanager role.</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="c0c00-110">Titkosítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c0c00-110">Enabling Encryption</span></span>
<span data-ttu-id="c0c00-111">az SQL Data Warehouse, TDE tooenable kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c0c00-111">tooenable TDE for a SQL Data Warehouse, follow hello steps below:</span></span>

1. <span data-ttu-id="c0c00-112">Megnyitás hello adatbázis hello [Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="c0c00-112">Open hello database in hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="c0c00-113">Hello adatbázis paneljén kattintson hello **beállítások** gomb</span><span class="sxs-lookup"><span data-stu-id="c0c00-113">In hello database blade, click hello **Settings** button</span></span>
3. <span data-ttu-id="c0c00-114">Jelölje be hello **átlátható adattitkosítás** beállítás![][1]</span><span class="sxs-lookup"><span data-stu-id="c0c00-114">Select hello **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="c0c00-115">Jelölje be hello **a** beállítás![][2]</span><span class="sxs-lookup"><span data-stu-id="c0c00-115">Select hello **On** setting ![][2]</span></span>
5. <span data-ttu-id="c0c00-116">Válassza ki **mentése**
   ![][3]</span><span class="sxs-lookup"><span data-stu-id="c0c00-116">Select **Save**
![][3]</span></span>  

## <a name="disabling-encryption"></a><span data-ttu-id="c0c00-117">Titkosításának letiltása</span><span class="sxs-lookup"><span data-stu-id="c0c00-117">Disabling Encryption</span></span>
<span data-ttu-id="c0c00-118">az SQL Data Warehouse, TDE toodisable kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c0c00-118">toodisable TDE for a SQL Data Warehouse, follow hello steps below:</span></span>

1. <span data-ttu-id="c0c00-119">Megnyitás hello adatbázis hello [Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="c0c00-119">Open hello database in hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="c0c00-120">Hello adatbázis paneljén kattintson hello **beállítások** gomb</span><span class="sxs-lookup"><span data-stu-id="c0c00-120">In hello database blade, click hello **Settings** button</span></span>
3. <span data-ttu-id="c0c00-121">Jelölje be hello **átlátható adattitkosítás** beállítás![][1]</span><span class="sxs-lookup"><span data-stu-id="c0c00-121">Select hello **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="c0c00-122">Jelölje be hello **ki** beállítás![][4]</span><span class="sxs-lookup"><span data-stu-id="c0c00-122">Select hello **Off** setting ![][4]</span></span>
5. <span data-ttu-id="c0c00-123">Válassza ki **mentése**
   ![][5]</span><span class="sxs-lookup"><span data-stu-id="c0c00-123">Select **Save**
![][5]</span></span>  

## <a name="encryption-dmvs"></a><span data-ttu-id="c0c00-124">Titkosítási dinamikus felügyeleti nézetek</span><span class="sxs-lookup"><span data-stu-id="c0c00-124">Encryption DMVs</span></span>
<span data-ttu-id="c0c00-125">A következő dinamikus felügyeleti nézetek hello titkosítási erősíthető meg:</span><span class="sxs-lookup"><span data-stu-id="c0c00-125">Encryption can be confirmed with hello following DMVs:</span></span>

* <span data-ttu-id="c0c00-126">[sys.Databases]</span><span class="sxs-lookup"><span data-stu-id="c0c00-126">[sys.databases]</span></span>
* <span data-ttu-id="c0c00-127">[sys.dm_pdw_nodes_database_encryption_keys]</span><span class="sxs-lookup"><span data-stu-id="c0c00-127">[sys.dm_pdw_nodes_database_encryption_keys]</span></span>

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
