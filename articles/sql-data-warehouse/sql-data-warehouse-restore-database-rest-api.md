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
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Állítsa vissza az Azure SQL Data Warehouse (REST API-t)
> [!div class="op_single_selector"]
> * [– Áttekintés][Overview]
> * [Portál][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

Ebben a cikkben megtudhatja, hogyan egy Azure SQL Data Warehouse használatával toorestore hello REST API-t.

## <a name="before-you-begin"></a>Előkészületek
**A DTU-kapacitásának ellenőrzése.** Minden egyes SQL Data Warehouse egy SQL server (pl. myserver.database.windows.net), amely rendelkezik egy alapértelmezett DTU-kvótáról üzemelteti.  SQL Data Warehouse próbál visszaállítani, győződjön meg arról, hogy az SQL Servert futtató hello adatbázis visszaállítása folyamatban elég fennmaradó DTU-kvótáról hello. toolearn hogyan toocalculate DTU szükséges vagy toorequest több DTU, lásd: [DTU-kvótát Módosítás kérése][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Az aktív vagy szüneteltetett adatbázis visszaállítása
egy adatbázis toorestore:

1. Adatbázis visszaállítási pontok hello Get adatbázis visszaállítási pontok művelet használatával hello listájának beolvasása.
2. A visszaállítás megkezdéséhez hello segítségével [létrehozása adatbázis visszaállítási kérelem] [ Create database restore request] műveletet.
3. A visszaállítási hello állapotának nyomon hello használatával [adatbázis-művelet állapota] [ Database operation status] műveletet.

> [!NOTE]
> Miután hello visszaállítás befejeződött, a helyreállított adatbázis követve konfigurálhatja [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Törölt adatbázis visszaállítása
a törölt adatbázisok toorestore:

1. Listázza az összes, a törölt, visszaállítható adatbázisok hello segítségével [lista visszaállítható adatbázisok eldobott] [ List restorable dropped databases] műveletet.
2. Segítségével könnyebben nyerhet hello részletek kívánt hello törölt adatbázis toorestore hello [visszaállítható Get adatbázis eldobása] [ Get restorable dropped database] műveletet.
3. A visszaállítás megkezdéséhez hello segítségével [létrehozása adatbázis visszaállítási kérelem] [ Create database restore request] műveletet.
4. A visszaállítási hello állapotának nyomon hello használatával [adatbázis-művelet állapota] [ Database operation status] műveletet.

> [!NOTE]
> tooconfigure hello visszaállítás befejezése után az adatbázis lásd [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].
> 
> 

## <a name="next-steps"></a>Következő lépések
toolearn kapcsolatos hello üzleti folytonosságot biztosító szolgáltatásokat az Azure SQL Database kiadásának, olvassa el a hello [Azure SQL Database üzleti folytonosság – áttekintés][Azure SQL Database business continuity overview].

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
