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
# <a name="create-an-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse létrehozása
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

Ez az oktatóanyag az Azure portál toocreate, amely egy AdventureWorksDW mintaadatbázist tartalmaz SQL Data Warehouse hello használja.

## <a name="prerequisites"></a>Előfeltételek
tooget elindult, lesz szüksége:

* **Azure-fiók**: keresse fel [Azure ingyenes próbaverzió] [ Azure Free Trial] vagy [MSDN Azure-Krediteket] [ MSDN Azure Credits] toocreate fiókkal.
* **Azure SQL-kiszolgáló**: lásd: [hello Azure-portálon hozzon létre egy Azure SQL-adatbázis] [ Create an Azure SQL database in hello Azure portal] további részleteket.

> [!NOTE]
> Egy SQL Data Warehouse létrehozása egy új számlázható szolgáltatás létrejöttét eredményezheti.  További információ: [SQL Data Warehouse díjszabása][SQL Data Warehouse pricing].
>
>

## <a name="create-a-sql-data-warehouse"></a>SQL Data Warehouse létrehozása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **+ Új** > **Adatbázisok** > **SQL Data Warehouse** lehetőségre.

    ![Létrehozás](./media/sql-data-warehouse-get-started-provision/create-sample.gif)
3. A hello **SQL Data Warehouse** panelen adja meg a hello fordítás, majd nyomja le az "Létrehozás" toocreate.

    ![Adatbázis létrehozása](./media/sql-data-warehouse-get-started-provision/create-database.png)

   * **Kiszolgáló**: Javasoljuk, hogy először válassza ki a kiszolgálót.  
   * **Az adatbázisnév**: használt tooreference hello SQL Data Warehouse hello nevet.  Egyedi toohello kiszolgálónak kell lennie.
   * **Teljesítmény**: Javasoljuk, hogy kiindulásként 400 [DWU][DWU]-t adjon meg. Áthelyezheti hello csúszkát toohello bal vagy jobb létrehozása után a data warehouse-ba, vagy a skála tooadjust hello teljesítményét felfelé vagy lefelé.  toolearn dwu-k, kapcsolatos további információkért olvassa el a dokumentációt a [skálázás](sql-data-warehouse-manage-compute-overview.md) vagy a [árképzést ismertető oldalra][SQL Data Warehouse pricing].
   * **Előfizetés**: Select hello [előfizetés] , amely az adott SQL Data Warehouse számlázza.
   * **Erőforráscsoport**: [erőforráscsoportok] [ Resource group] tárolók, amelyek toohelp kezelheti az Azure-erőforrások gyűjteménye van. További információk az [erőforráscsoportokról](../azure-resource-manager/resource-group-overview.md).
   * **Forrás kiválasztása**: Kattintson a **Forrás kiválasztása** > **Minta** lehetőségre. Azure automatikusan feltölti a hello **minta kiválasztása** beállítást az adventureworksdw elemmel.

   > [!NOTE]
   > SQL Data Warehouse hello alapértelmezett rendezését SQL_Latin1_General_CP1_CI_AS. Ha eltérő rendezést van szükség, [T-SQL] [ T-SQL] eltérő rendezést használt toocreate hello adatbázis is lehet.
   >
   >

1. Kattintson a **létrehozása** toocreate az SQL Data Warehouse.
2. Várjon néhány percet. Amikor készen áll az adatraktár, akkor vissza kell adni az toohello [Azure-portálon](https://portal.azure.com). Az SQL Data Warehouse az irányítópulton, az SQL Database adatbázisok listájában található, vagy hello erőforrás csoportosítás, hogy Ön használt toocreate azt.

    ![portál nézet](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Következő lépések
Most, hogy az SQL Data Warehouse létrehozott, készen áll túl[Connect](sql-data-warehouse-connect-overview.md) és lekérdezések indítására.

az SQL Data Warehouse tooload adatok, lásd: hello [betöltést áttekintő](sql-data-warehouse-overview-load.md).

Ha egy meglévő adatbázis tooSQL adatraktár toomigrate, lásd: hello [áttelepítése – áttekintés](sql-data-warehouse-overview-migrate.md) , vagy használjon [áttelepítő segédprogramot](sql-data-warehouse-migrate-migration-utility.md).

Tűzfalszabályok is a Transact-SQL segítségével konfigurálhatók. További információ: [sp_set_firewall_rule][sp_set_firewall_rule] és [sp_set_database_firewall_rule][sp_set_database_firewall_rule].

Egyúttal a legjobb ötlet toolook: hello [ajánlott eljárások][Best practices].

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
