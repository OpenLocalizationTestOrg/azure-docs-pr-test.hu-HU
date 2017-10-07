---
title: "az Azure Analysis Services támogatott aaaData források |} Microsoft Docs"
description: "Az Azure Analysis Services adatmodellekben támogatott adatforrások ismerteti."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 6ec63319-ff9b-4b01-a1cd-274481dc8995
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 2902d7d3c3bcf086419822fa826193bd247bde61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Az Azure Analysis Services támogatott adatforrások
Azure Analysis Services-kiszolgálók hello felhőben és a szervezet helyszíni összekötő toodata források támogatja. További támogatott adatforrások közé éppen felvett összes hello idő. Gyakran ellátogatnia webhelyünkre. 

a következő adatforrások hello jelenleg támogatottak:

| Felhő  |
|---|
| Az Azure Blob Storage *  |
| Azure SQL Database  |
| Az Azure Data warehouse-bA |


| Helyszíni követelmények  |   |   |   |
|---|---|---|---|
| Access-adatbázishoz  | Mappa * | Oracle Database  | Teradata-adatbázis |
| Active Directory *  | JSON-dokumentum *  | Postgre SQL adatbázis *  |XML tábla * |
| Analysis Services  | A bináris * sorok  | SAP HANA *  |
| Analytics Platform System  | MySQL-adatbázis  | SAP Business Warehouse *  | |
| Dynamics CRM *  | Az OData-adatcsatorna *  | SharePoint *  |
| Az Excel-munkafüzet  | Az ODBC-lekérdezés  | SQL Database  |
| Exchange *  | OLE DB RENDSZERHEZ  | Sybase-adatbázishoz  |

\*A táblázatos 1 400 modellek csak. 

> [!IMPORTANT]
> Összekötő tooon helyszíni adatok források szükséges egy [helyszíni adatátjáró](analysis-services-gateway.md) környezetét a számítógépen.

## <a name="data-providers"></a>Adatszolgáltatók

Az Azure Analysis Services adatmodellekben különféle adatszolgáltatóktól igényelhet, toocertain adatforrások kapcsolódáskor. Bizonyos esetekben csatlakozás toodata adatforrások, például az SQL Server Native Client (SQLNCLI11) natív szolgáltatók használata táblázatos modellek hibát adhat vissza.

Például az Azure SQL Database forrás tooa felhőbeli adatát csatlakozó adatmodellekben, natív szolgáltatók eltérő SQLOLEDB használatakor hibaüzenet jelenhet: **"hello provider" "SQLNCLI11.1" nincs regisztrálva.** Vagy, ha a kapcsolódó tooon helyi adatforrások, a modell DirectQuery natív szolgáltatók használatakor hibaüzenet jelenhet: **"hiba történt a sorkészlet OLE DB létrehozásakor. Helytelen szintaxis a(z) "KORLÁTJA" közelében "**.

a következő adatforrás szolgáltatók hello a memóriában vagy a DirectQuery-modellekre adatok támogatottak, ha a kapcsolódó toodata hello felhő vagy a helyszíni adatforrások:

### <a name="cloud"></a>Felhő
| **Adatforrás** | **Memóriabeli** | **DirectQuery** |
|  --- | --- | --- |
| Azure SQL Data Warehouse |.NET framework Data Provider az SQL Server |.NET framework Data Provider az SQL Server |
| Azure SQL Database |.NET framework Data Provider az SQL Server |.NET framework Data Provider az SQL Server | |

### <a name="on-premises-via-gateway"></a>A helyszíni (átjáró) keresztül
|**Adatforrás** | **Memóriabeli** | **DirectQuery** |
|  --- | --- | --- |
| SQL Server |SQL Server natív ügyfél 11.0 |.NET framework Data Provider az SQL Server |
| SQL Server |Microsoft OLE DB-szolgáltató az SQL Server |.NET framework Data Provider az SQL Server | |
| SQL Server |.NET framework Data Provider az SQL Server |.NET framework Data Provider az SQL Server | |
| Oracle |Microsoft OLE DB-szolgáltató az Oracle rendszerhez |Oracle-adatszolgáltatóban a .NET-hez | |
| Oracle |Oracle-adatszolgáltatóban a .NET-hez |Oracle-adatszolgáltatóban a .NET-hez | |
| Teradata |OLE DB-szolgáltató a teradata rendszerhez |Teradata-adatszolgáltatója a .NET-hez | |
| Teradata |Teradata-adatszolgáltatója a .NET-hez |Teradata-adatszolgáltatója a .NET-hez | |
| Analytics Platform System |.NET framework Data Provider az SQL Server |.NET framework Data Provider az SQL Server | |

> [!NOTE]
> Győződjön meg arról, 64 bites szolgáltató van telepítve, a helyszíni átjáró használatakor.
> 
> 

Egy helyi SQL Server Analysis Services táblázatos modell tooAzure Analysis Services áttelepítésekor lehet szükséges toochange hello szolgáltató.

**toospecify egy adatforrás-szolgáltatója**

1. Az SSDT > **táblázatos modell Explorer** > **adatforrások**, kattintson a jobb gombbal az adatforrás-kapcsolat, és kattintson a **adatforrás szerkesztése**.
2. A **kapcsolat szerkesztése**, kattintson a **speciális** tooopen hello előzetes tulajdonságai ablakban.
3. A **speciális tulajdonságok** > **szolgáltatók**, majd válassza ki a megfelelő szolgáltató hello.

## <a name="impersonation"></a>A megszemélyesítés
Bizonyos esetekben egy másik megszemélyesítési fiókhoz szükséges toospecify lehet. Megszemélyesítési fiókhoz SSDT vagy SSMS adható meg.

A helyszíni adatforrások:

* Ha az SQL-hitelesítést használ, a megszemélyesítési szolgáltatás fióknak kell lennie.
* Ha Windows-hitelesítést használ, állítsa be a Windows felhasználói/jelszó. Az SQL Server Windows-hitelesítés egy adott megszemélyesítési fiókhoz csak támogatott a memóriában levő modellek.

Felhő adatforrások:

* Ha az SQL-hitelesítést használ, a megszemélyesítési szolgáltatás fióknak kell lennie.

## <a name="next-steps"></a>Következő lépések
Ha a helyszíni adatforrások, lehet, hogy tooinstall hello [helyszíni átjáró](analysis-services-gateway.md).   
További információ az SSDT vagy SSMS, a kiszolgáló kezelése toolearn lásd [kezelheti a kiszolgálót](analysis-services-manage.md).

