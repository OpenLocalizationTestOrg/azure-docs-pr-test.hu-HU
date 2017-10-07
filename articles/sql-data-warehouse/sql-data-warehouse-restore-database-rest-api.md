---
title: Azure SQL Data Warehouse (REST API-t) aaaRestore |} Microsoft Docs
description: "Azure SQL Data Warehouse visszaállítására feladatok REST API-t."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: fca922c6-b675-49c7-907e-5dcf26d451dd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: cf6678d71aafff71b1ea715f447e41e25f20d1b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a><span data-ttu-id="94d63-103">Állítsa vissza az Azure SQL Data Warehouse (REST API-t)</span><span class="sxs-lookup"><span data-stu-id="94d63-103">Restore an Azure SQL Data Warehouse (REST API)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="94d63-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="94d63-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="94d63-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="94d63-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="94d63-106">[PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="94d63-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="94d63-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="94d63-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="94d63-108">Ebben a cikkben megtudhatja, hogyan egy Azure SQL Data Warehouse használatával toorestore hello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="94d63-108">In this article you will learn how toorestore an Azure SQL Data Warehouse using hello REST API.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="94d63-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="94d63-109">Before you begin</span></span>
<span data-ttu-id="94d63-110">**A DTU-kapacitásának ellenőrzése.**</span><span class="sxs-lookup"><span data-stu-id="94d63-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="94d63-111">Minden egyes SQL Data Warehouse egy SQL server (pl. myserver.database.windows.net), amely rendelkezik egy alapértelmezett DTU-kvótáról üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="94d63-111">Each SQL Data Warehouse is hosted by a SQL server (e.g. myserver.database.windows.net) which has a default DTU quota.</span></span>  <span data-ttu-id="94d63-112">SQL Data Warehouse próbál visszaállítani, győződjön meg arról, hogy az SQL Servert futtató hello adatbázis visszaállítása folyamatban elég fennmaradó DTU-kvótáról hello.</span><span class="sxs-lookup"><span data-stu-id="94d63-112">Before you can restore a SQL Data Warehouse, verify that hello your SQL server has enough remaining DTU quota for hello database being restored.</span></span> <span data-ttu-id="94d63-113">toolearn hogyan toocalculate DTU szükséges vagy toorequest több DTU, lásd: [DTU-kvótát Módosítás kérése][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="94d63-113">toolearn how toocalculate DTU needed or toorequest more DTU, see [Request a DTU quota change][Request a DTU quota change].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="94d63-114">Az aktív vagy szüneteltetett adatbázis visszaállítása</span><span class="sxs-lookup"><span data-stu-id="94d63-114">Restore an active or paused database</span></span>
<span data-ttu-id="94d63-115">egy adatbázis toorestore:</span><span class="sxs-lookup"><span data-stu-id="94d63-115">toorestore a database:</span></span>

1. <span data-ttu-id="94d63-116">Adatbázis visszaállítási pontok hello Get adatbázis visszaállítási pontok művelet használatával hello listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="94d63-116">Get hello list of database restore points using hello Get Database Restore Points operation.</span></span>
2. <span data-ttu-id="94d63-117">A visszaállítás megkezdéséhez hello segítségével [létrehozása adatbázis visszaállítási kérelem] [ Create database restore request] műveletet.</span><span class="sxs-lookup"><span data-stu-id="94d63-117">Begin your restore by using hello [Create database restore request][Create database restore request] operation.</span></span>
3. <span data-ttu-id="94d63-118">A visszaállítási hello állapotának nyomon hello használatával [adatbázis-művelet állapota] [ Database operation status] műveletet.</span><span class="sxs-lookup"><span data-stu-id="94d63-118">Track hello status of your restore by using hello [Database operation status][Database operation status] operation.</span></span>

> [!NOTE]
> <span data-ttu-id="94d63-119">Miután hello visszaállítás befejeződött, a helyreállított adatbázis követve konfigurálhatja [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="94d63-119">After hello restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-a-deleted-database"></a><span data-ttu-id="94d63-120">Törölt adatbázis visszaállítása</span><span class="sxs-lookup"><span data-stu-id="94d63-120">Restore a deleted database</span></span>
<span data-ttu-id="94d63-121">a törölt adatbázisok toorestore:</span><span class="sxs-lookup"><span data-stu-id="94d63-121">toorestore a deleted database:</span></span>

1. <span data-ttu-id="94d63-122">Listázza az összes, a törölt, visszaállítható adatbázisok hello segítségével [lista visszaállítható adatbázisok eldobott] [ List restorable dropped databases] műveletet.</span><span class="sxs-lookup"><span data-stu-id="94d63-122">List all of your restorable deleted databases by using hello [List restorable dropped databases][List restorable dropped databases] operation.</span></span>
2. <span data-ttu-id="94d63-123">Segítségével könnyebben nyerhet hello részletek kívánt hello törölt adatbázis toorestore hello [visszaállítható Get adatbázis eldobása] [ Get restorable dropped database] műveletet.</span><span class="sxs-lookup"><span data-stu-id="94d63-123">Get hello details for hello deleted database you want toorestore by using hello [Get restorable dropped database][Get restorable dropped database] operation.</span></span>
3. <span data-ttu-id="94d63-124">A visszaállítás megkezdéséhez hello segítségével [létrehozása adatbázis visszaállítási kérelem] [ Create database restore request] műveletet.</span><span class="sxs-lookup"><span data-stu-id="94d63-124">Begin your restore by using hello [Create database restore request][Create database restore request] operation.</span></span>
4. <span data-ttu-id="94d63-125">A visszaállítási hello állapotának nyomon hello használatával [adatbázis-művelet állapota] [ Database operation status] műveletet.</span><span class="sxs-lookup"><span data-stu-id="94d63-125">Track hello status of your restore by using hello [Database operation status][Database operation status] operation.</span></span>

> [!NOTE]
> <span data-ttu-id="94d63-126">tooconfigure hello visszaállítás befejezése után az adatbázis lásd [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="94d63-126">tooconfigure your database after hello restore has completed, see [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="94d63-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="94d63-127">Next steps</span></span>
<span data-ttu-id="94d63-128">toolearn kapcsolatos hello üzleti folytonosságot biztosító szolgáltatásokat az Azure SQL Database kiadásának, olvassa el a hello [Azure SQL Database üzleti folytonosság – áttekintés][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="94d63-128">toolearn about hello business continuity features of Azure SQL Database editions, please read hello [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Create database restore request]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Database operation status]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Get restorable dropped database]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[List restorable dropped databases]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
