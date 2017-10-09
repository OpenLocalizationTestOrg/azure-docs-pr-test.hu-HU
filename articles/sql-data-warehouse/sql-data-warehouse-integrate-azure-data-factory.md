---
title: "Azure Data Factory az SQL Data Warehouse szolgáltatással aaaUse |} Microsoft Docs"
description: "Tippek az Azure Data Factory (ADF) használata az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 492de762-c7a2-4cdb-943f-3135230e94f1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: d40a547830f9681504253d39ae3066800a955c04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Az Azure Data Factory használja az SQL Data Warehouse szolgáltatással
Az Azure Data Factory lehetővé teszi a teljes körűen felügyelt koordinálása hello adatátvitelt és az SQL Data warehouse tárolt eljárások végrehajtása.  Ez lehetővé teszi toomore könnyen beállításról és ütemezése összetett bontsa ki az átalakítási és betöltési (ETL) eljárások az SQL Data Warehouse szolgáltatással. Azure Data Factory részletesebb áttekintéséért lásd: hello [Azure Data Factory dokumentáció][Azure Data Factory documentation].

## <a name="data-movement"></a>Adatáthelyezés
Az Azure Data Factory lehetővé teszi, hogy a helyszíni adatforrások és a különböző Azure-szolgáltatások közötti adatátvitelt jelölik.  Azure Data Factory integráció átfogó, a jelenlegi adatokat a mozgás tooand az alábbi helyek hello támogatja:

* Az Azure blob-tároló
* Azure SQL Database
* A helyszíni SQL Server
* SQL Server IaaS

Hogyan tooset egy adatok másolási tevékenység információt [másolja az adatokat az Azure Data Factoryvel][Copy data with Azure Data Factory]

## <a name="stored-procedures"></a>Tárolt eljárások
 A hello azonos módon lehet tooschedule adatok átvitele Azure Data Factory is is használni kell használt tooorchestrate hello tárolt eljárások végrehajtása.  Ez lehetővé teszi, hogy a létrehozott összetettebb folyamatok toobe és kibővíti az Azure Data Factory képességét tooleverage hello számítási teljesítménnyel rendelkezik az SQL Data Warehouse.

## <a name="next-steps"></a>Következő lépések
Az integráció áttekintéséért lásd: [SQL Data Warehouse integrációjának áttekintése][SQL Data Warehouse integration overview].
További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].

<!--Image references-->

<!--Article references-->

[Copy data with Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory documentation]:https://azure.microsoft.com/documentation/services/data-factory/

