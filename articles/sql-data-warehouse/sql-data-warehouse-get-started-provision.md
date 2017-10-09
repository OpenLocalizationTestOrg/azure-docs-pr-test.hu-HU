---
title: "hello Azure-portálon az SQL Data Warehouse aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Azure SQL Data Warehouse a hello Azure-portálon"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: 552e496e-d560-419c-9996-6bbc80c521cb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: f5be6e3f2936e3af9d099854a468f8ce66fd8fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-sql-data-warehouse"></a><span data-ttu-id="2cd9f-103">Azure SQL Data Warehouse létrehozása</span><span class="sxs-lookup"><span data-stu-id="2cd9f-103">Create an Azure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2cd9f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2cd9f-104">Azure portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="2cd9f-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="2cd9f-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="2cd9f-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2cd9f-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="2cd9f-107">Ez az oktatóanyag az Azure portál toocreate, amely egy AdventureWorksDW mintaadatbázist tartalmaz SQL Data Warehouse hello használja.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-107">This tutorial uses hello Azure portal toocreate a SQL Data Warehouse that contains an AdventureWorksDW sample database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2cd9f-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2cd9f-108">Prerequisites</span></span>
<span data-ttu-id="2cd9f-109">tooget elindult, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="2cd9f-109">tooget started, you need:</span></span>

* <span data-ttu-id="2cd9f-110">**Azure-fiók**: keresse fel [Azure ingyenes próbaverzió] [ Azure Free Trial] vagy [MSDN Azure-Krediteket] [ MSDN Azure Credits] toocreate fiókkal.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] toocreate an account.</span></span>
* <span data-ttu-id="2cd9f-111">**Azure SQL-kiszolgáló**: lásd: [hello Azure-portálon hozzon létre egy Azure SQL-adatbázis] [ Create an Azure SQL database in hello Azure portal] további részleteket.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-111">**Azure SQL server**:  See [Create an Azure SQL database with hello Azure portal][Create an Azure SQL database in hello Azure portal] for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="2cd9f-112">Egy SQL Data Warehouse létrehozása egy új számlázható szolgáltatás létrejöttét eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-112">Creating a SQL Data Warehouse might result in a new billable service.</span></span>  <span data-ttu-id="2cd9f-113">További információ: [SQL Data Warehouse díjszabása][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="2cd9f-113">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details.</span></span>
>
>

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="2cd9f-114">SQL Data Warehouse létrehozása</span><span class="sxs-lookup"><span data-stu-id="2cd9f-114">Create a SQL Data Warehouse</span></span>
1. <span data-ttu-id="2cd9f-115">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2cd9f-115">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2cd9f-116">Kattintson a **+ Új** > **Adatbázisok** > **SQL Data Warehouse** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-116">Click **+ New** > **Databases** > **SQL Data Warehouse**.</span></span>

    ![Létrehozás](./media/sql-data-warehouse-get-started-provision/create-sample.gif)
3. <span data-ttu-id="2cd9f-118">A hello **SQL Data Warehouse** panelen adja meg a hello fordítás, majd nyomja le az "Létrehozás" toocreate.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-118">In hello **SQL Data Warehouse** blade, fill in hello information needed, then press 'Create' toocreate.</span></span>

    ![Adatbázis létrehozása](./media/sql-data-warehouse-get-started-provision/create-database.png)

   * <span data-ttu-id="2cd9f-120">**Kiszolgáló**: Javasoljuk, hogy először válassza ki a kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-120">**Server**: We recommend you select your server first.</span></span>  
   * <span data-ttu-id="2cd9f-121">**Az adatbázisnév**: használt tooreference hello SQL Data Warehouse hello nevet.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-121">**Database name**: hello name that is used tooreference hello SQL Data Warehouse.</span></span>  <span data-ttu-id="2cd9f-122">Egyedi toohello kiszolgálónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-122">It must be unique toohello server.</span></span>
   * <span data-ttu-id="2cd9f-123">**Teljesítmény**: Javasoljuk, hogy kiindulásként 400 [DWU][DWU]-t adjon meg.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-123">**Performance**: We recommend starting with 400 [DWUs][DWU].</span></span> <span data-ttu-id="2cd9f-124">Áthelyezheti hello csúszkát toohello bal vagy jobb létrehozása után a data warehouse-ba, vagy a skála tooadjust hello teljesítményét felfelé vagy lefelé.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-124">You can move hello slider toohello left or right tooadjust hello performance of your data warehouse, or scale up or down after creation.</span></span>  <span data-ttu-id="2cd9f-125">toolearn dwu-k, kapcsolatos további információkért olvassa el a dokumentációt a [skálázás](sql-data-warehouse-manage-compute-overview.md) vagy a [árképzést ismertető oldalra][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="2cd9f-125">toolearn more about DWUs, see our documentation on [scaling](sql-data-warehouse-manage-compute-overview.md) or our [pricing page][SQL Data Warehouse pricing].</span></span>
   * <span data-ttu-id="2cd9f-126">**Előfizetés**: Select hello [előfizetés] , amely az adott SQL Data Warehouse számlázza.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-126">**Subscription**: Select hello [subscription] that this SQL Data Warehouse will bill to.</span></span>
   * <span data-ttu-id="2cd9f-127">**Erőforráscsoport**: [erőforráscsoportok] [ Resource group] tárolók, amelyek toohelp kezelheti az Azure-erőforrások gyűjteménye van.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-127">**Resource group**: [Resource groups][Resource group] are containers designed toohelp you manage a collection of Azure resources.</span></span> <span data-ttu-id="2cd9f-128">További információk az [erőforráscsoportokról](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2cd9f-128">Learn more about [resource groups](../azure-resource-manager/resource-group-overview.md).</span></span>
   * <span data-ttu-id="2cd9f-129">**Forrás kiválasztása**: Kattintson a **Forrás kiválasztása** > **Minta** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-129">**Select source**: Click **Select source** > **Sample**.</span></span> <span data-ttu-id="2cd9f-130">Azure automatikusan feltölti a hello **minta kiválasztása** beállítást az adventureworksdw elemmel.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-130">Azure automatically populates hello **Select sample** option with AdventureWorksDW.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2cd9f-131">SQL Data Warehouse hello alapértelmezett rendezését SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-131">hello default collation for a SQL Data Warehouse is SQL_Latin1_General_CP1_CI_AS.</span></span> <span data-ttu-id="2cd9f-132">Ha eltérő rendezést van szükség, [T-SQL] [ T-SQL] eltérő rendezést használt toocreate hello adatbázis is lehet.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-132">If a different collation is needed, [T-SQL][T-SQL] can be used toocreate hello database with a different collation.</span></span>
   >
   >

1. <span data-ttu-id="2cd9f-133">Kattintson a **létrehozása** toocreate az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-133">Click **Create** toocreate your SQL Data Warehouse.</span></span>
2. <span data-ttu-id="2cd9f-134">Várjon néhány percet.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-134">Wait for a few minutes.</span></span> <span data-ttu-id="2cd9f-135">Amikor készen áll az adatraktár, akkor vissza kell adni az toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2cd9f-135">When your data warehouse is ready, you should be returned toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="2cd9f-136">Az SQL Data Warehouse az irányítópulton, az SQL Database adatbázisok listájában található, vagy hello erőforrás csoportosítás, hogy Ön használt toocreate azt.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-136">You can find your SQL Data Warehouse on your dashboard, listed under your SQL Databases, or in hello resource group that you used toocreate it.</span></span>

    ![portál nézet](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="2cd9f-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2cd9f-138">Next steps</span></span>
<span data-ttu-id="2cd9f-139">Most, hogy az SQL Data Warehouse létrehozott, készen áll túl[Connect](sql-data-warehouse-connect-overview.md) és lekérdezések indítására.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-139">Now that you have created a SQL Data Warehouse, you are ready too[Connect](sql-data-warehouse-connect-overview.md) and begin querying.</span></span>

<span data-ttu-id="2cd9f-140">az SQL Data Warehouse tooload adatok, lásd: hello [betöltést áttekintő](sql-data-warehouse-overview-load.md).</span><span class="sxs-lookup"><span data-stu-id="2cd9f-140">tooload data into SQL Data Warehouse, see hello [loading overview](sql-data-warehouse-overview-load.md).</span></span>

<span data-ttu-id="2cd9f-141">Ha egy meglévő adatbázis tooSQL adatraktár toomigrate, lásd: hello [áttelepítése – áttekintés](sql-data-warehouse-overview-migrate.md) , vagy használjon [áttelepítő segédprogramot](sql-data-warehouse-migrate-migration-utility.md).</span><span class="sxs-lookup"><span data-stu-id="2cd9f-141">If you are trying toomigrate an existing database tooSQL Data Warehouse, see hello [Migration overview](sql-data-warehouse-overview-migrate.md) or use [Migration Utility](sql-data-warehouse-migrate-migration-utility.md).</span></span>

<span data-ttu-id="2cd9f-142">Tűzfalszabályok is a Transact-SQL segítségével konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="2cd9f-142">Firewall rules can also be configured using Transact-SQL.</span></span> <span data-ttu-id="2cd9f-143">További információ: [sp_set_firewall_rule][sp_set_firewall_rule] és [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span><span class="sxs-lookup"><span data-stu-id="2cd9f-143">For more information, see [sp_set_firewall_rule][sp_set_firewall_rule] and [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span></span>

<span data-ttu-id="2cd9f-144">Egyúttal a legjobb ötlet toolook: hello [ajánlott eljárások][Best practices].</span><span class="sxs-lookup"><span data-stu-id="2cd9f-144">It's also a great idea toolook at hello [Best practices][Best practices].</span></span>

<!--Article references-->
[Create an Azure SQL database in hello Azure portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[resource groups]: ../azure-resource-manager/resource-group-template-deploy-portal.md
[Best practices]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md
[előfizetés]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md

<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
