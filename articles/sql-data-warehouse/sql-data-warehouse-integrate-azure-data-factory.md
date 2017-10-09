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
# <a name="use-azure-data-factory-with-sql-data-warehouse"></a><span data-ttu-id="908d5-103">Az Azure Data Factory használja az SQL Data Warehouse szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="908d5-103">Use Azure Data Factory with SQL Data Warehouse</span></span>
<span data-ttu-id="908d5-104">Az Azure Data Factory lehetővé teszi a teljes körűen felügyelt koordinálása hello adatátvitelt és az SQL Data warehouse tárolt eljárások végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="908d5-104">Azure Data Factory provides a fully managed method for orchestrating hello transfer of data and execution of stored procedures on SQL Data Warehouse.</span></span>  <span data-ttu-id="908d5-105">Ez lehetővé teszi toomore könnyen beállításról és ütemezése összetett bontsa ki az átalakítási és betöltési (ETL) eljárások az SQL Data Warehouse szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="908d5-105">This will allow you toomore easily set-up and schedule complex Extract Transform and Load (ETL) procedures with SQL Data Warehouse.</span></span> <span data-ttu-id="908d5-106">Azure Data Factory részletesebb áttekintéséért lásd: hello [Azure Data Factory dokumentáció][Azure Data Factory documentation].</span><span class="sxs-lookup"><span data-stu-id="908d5-106">For a more complete overview of Azure Data Factory, see hello [Azure Data Factory documentation][Azure Data Factory documentation].</span></span>

## <a name="data-movement"></a><span data-ttu-id="908d5-107">Adatáthelyezés</span><span class="sxs-lookup"><span data-stu-id="908d5-107">Data Movement</span></span>
<span data-ttu-id="908d5-108">Az Azure Data Factory lehetővé teszi, hogy a helyszíni adatforrások és a különböző Azure-szolgáltatások közötti adatátvitelt jelölik.</span><span class="sxs-lookup"><span data-stu-id="908d5-108">Azure Data Factory enables data movement between both on-premises sources and different Azure services.</span></span>  <span data-ttu-id="908d5-109">Azure Data Factory integráció átfogó, a jelenlegi adatokat a mozgás tooand az alábbi helyek hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="908d5-109">Overall, current integration with Azure Data Factory supports data movement tooand from hello following locations:</span></span>

* <span data-ttu-id="908d5-110">Az Azure blob-tároló</span><span class="sxs-lookup"><span data-stu-id="908d5-110">Azure blob storage</span></span>
* <span data-ttu-id="908d5-111">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="908d5-111">Azure SQL Database</span></span>
* <span data-ttu-id="908d5-112">A helyszíni SQL Server</span><span class="sxs-lookup"><span data-stu-id="908d5-112">On-premises SQL Server</span></span>
* <span data-ttu-id="908d5-113">SQL Server IaaS</span><span class="sxs-lookup"><span data-stu-id="908d5-113">SQL Server on IaaS</span></span>

<span data-ttu-id="908d5-114">Hogyan tooset egy adatok másolási tevékenység információt [másolja az adatokat az Azure Data Factoryvel][Copy data with Azure Data Factory]</span><span class="sxs-lookup"><span data-stu-id="908d5-114">For information on how tooset up a data copy activity see [Copy data with Azure Data Factory][Copy data with Azure Data Factory]</span></span>

## <a name="stored-procedures"></a><span data-ttu-id="908d5-115">Tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="908d5-115">Stored Procedures</span></span>
 <span data-ttu-id="908d5-116">A hello azonos módon lehet tooschedule adatok átvitele Azure Data Factory is is használni kell használt tooorchestrate hello tárolt eljárások végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="908d5-116">In hello same way it can be used tooschedule data transfer, Azure Data Factory can also be used tooorchestrate hello execution of stored procedures.</span></span>  <span data-ttu-id="908d5-117">Ez lehetővé teszi, hogy a létrehozott összetettebb folyamatok toobe és kibővíti az Azure Data Factory képességét tooleverage hello számítási teljesítménnyel rendelkezik az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="908d5-117">This allows more complex pipelines toobe created and extends Azure Data Factory's ability tooleverage hello computational power of SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="908d5-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="908d5-118">Next steps</span></span>
<span data-ttu-id="908d5-119">Az integráció áttekintéséért lásd: [SQL Data Warehouse integrációjának áttekintése][SQL Data Warehouse integration overview].</span><span class="sxs-lookup"><span data-stu-id="908d5-119">For an overview of integration, see [SQL Data Warehouse integration overview][SQL Data Warehouse integration overview].</span></span>
<span data-ttu-id="908d5-120">További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="908d5-120">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

<!--Article references-->

[Copy data with Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory documentation]:https://azure.microsoft.com/documentation/services/data-factory/

