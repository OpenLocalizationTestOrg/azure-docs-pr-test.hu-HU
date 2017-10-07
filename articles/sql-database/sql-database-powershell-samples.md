---
title: "SQL-adatbázis aaaAzure PowerShell-parancsfájl példák |} Microsoft Docs"
description: "Az Azure PowerShell parancsfájl példák toohelp létrehozása és kezelése az Azure SQL Database-kiszolgálók, rugalmas készletek, adatbázisok és tűzfalak."
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: overview-samples
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 1130ffb0e1c2b94c676d564ad5c4eb3b86374dbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Az Azure SQL Database az Azure PowerShell-példák

hello következő táblázat tartalmazza a hivatkozások toosample Azure PowerShell-parancsfájlok az Azure SQL Database.

| |  |
|---|---|
|**Hozzon létre egy önálló adatbázis és a rugalmas készlethez**||
| [Hozzon létre egy adatbázist, és a tűzfalszabályok konfigurálása](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | A PowerShell-parancsfájlt hoz létre egy Azure SQL-adatbázis, és konfigurálja egy kiszolgálószintű tűzfalszabályt. |
| [A rugalmas készlet létrehozása és készletezett adatbázisok áthelyezése](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | A PowerShell-parancsfájlt hoz létre az Azure SQL Database rugalmas készletek, és készletezett adatbázisok helyezi át, és teljesítményszintek vált.|
|**Georeplikálási és a feladatátvétel konfigurálása**||
| [Konfigurálja és feladatátvételi egy önálló adatbázis aktív georeplikációt használ](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| A PowerShell-parancsfájl egy Azure SQL-adatbázis aktív georeplikáció konfigurálja, és sikertelen, akkor a másodlagos másodpéldány toohello keresztül. |
| [Konfigurálja és feladatátvételi egy készletezett adatbázis aktív georeplikációt használ](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| A PowerShell parancsfájl konfigurálja az Azure SQL-adatbázis aktív georeplikáció a rugalmas SQL-készletet, és sikertelen, akkor a másodlagos másodpéldány toohello keresztül. |
| [Konfigurálja és a feladatátvételi feladatátvételi csoport számára egy adatbázist (előzetes verzió)](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | A PowerShell parancsfájl konfigurálása az Azure SQL Database server-példány, egy feladatátvételi csoport adatbázis toohello feladatátvételi csoport hozzáadása, és sikertelen, akkor a másodlagos kiszolgáló toohello keresztül |
|**Egy önálló adatbázisok és rugalmas készletek méretezése**||
| [Egy önálló adatbázis méretezése](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | A PowerShell parancsfájl hello teljesítménymutatók egy Azure SQL adatbázis figyeli, méretezi, magasabb teljesítményszintre tooa, és egy riasztási szabályt hoz létre egy hello teljesítménymutatók. |
| [Egy rugalmas készlet méretezése](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | A PowerShell parancsfájl hello teljesítménymutatók egy Azure SQL Database rugalmas készlet figyeli, méretezi, magasabb teljesítményszintre tooa és hello teljesítménymutatók egyik egy riasztási szabályt hoz létre.  |
| **Naplózás és a fenyegetések észlelése** |
| [Naplózás és fenyegetésészlelés konfigurálása](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| A PowerShell parancsfájl naplózás és a fenyegetések észlelése házirendek az Azure SQL-adatbázis konfigurálása. |
| **Állítsa vissza, másolása és adatbázis importálása**||
| [Adatbázis helyreállítása](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| A PowerShell parancsfájl visszaállítja az Azure SQL-adatbázis egy georedundáns biztonsági másolatból, és visszaállítja a törölt Azure SQL adatbázis toohello legfrissebb biztonsági másolatát. |
| [Másolja a toonew adatbázis-kiszolgáló](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| A PowerShell-parancsfájl egy létező Azure SQL-adatbázis másolatát egy új Azure SQL Server hoz létre. |
| [Adatbázis bacpac-fájlból való importálása](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| A PowerShell parancsfájl adatbázis tooan Azure SQL-kiszolgáló bacpac-fájlból importálhatja. |
| **Adatbázisok között szinkronizálja az adatokat**||
| [SQL-adatbázisok között szinkronizálja az adatokat](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | A PowerShell-parancsprogram több Azure SQL-adatbázisok közötti adatszinkronizálás toosync konfigurálja. |
| [SQL Database és SQL Server helyszíni között szinkronizálja az adatokat](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | A PowerShell parancsfájl konfigurálja az Azure SQL-adatbázis és a helyszíni SQL Server-adatbázisok közötti adatszinkronizálás toosync. |
|||
|||
