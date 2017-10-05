---
title: "Az Azure Data Factory használja az SQL Data Warehouse szolgáltatással |} Microsoft Docs"
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
ms.openlocfilehash: 7cd113f4a92635bc68253c2beb165ad1f0c96569
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-data-factory-with-sql-data-warehouse"></a><span data-ttu-id="a8b15-103">Az Azure Data Factory használja az SQL Data Warehouse szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="a8b15-103">Use Azure Data Factory with SQL Data Warehouse</span></span>
<span data-ttu-id="a8b15-104">Az Azure Data Factory lehetővé teszi a teljes körűen felügyelt koordinálása az adatok átvitelét és az SQL Data warehouse tárolt eljárások végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="a8b15-104">Azure Data Factory provides a fully managed method for orchestrating the transfer of data and execution of stored procedures on SQL Data Warehouse.</span></span>  <span data-ttu-id="a8b15-105">Ez lehetővé teszi a könnyebb beállítás és ütemezése összetett bontsa ki az átalakítási és betöltési (ETL) eljárások az SQL Data Warehouse szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="a8b15-105">This will allow you to more easily set-up and schedule complex Extract Transform and Load (ETL) procedures with SQL Data Warehouse.</span></span> <span data-ttu-id="a8b15-106">Azure Data Factory részletesebb áttekintéséért lásd: a [Azure Data Factory dokumentáció][Azure Data Factory documentation].</span><span class="sxs-lookup"><span data-stu-id="a8b15-106">For a more complete overview of Azure Data Factory, see the [Azure Data Factory documentation][Azure Data Factory documentation].</span></span>

## <a name="data-movement"></a><span data-ttu-id="a8b15-107">Adatáthelyezés</span><span class="sxs-lookup"><span data-stu-id="a8b15-107">Data Movement</span></span>
<span data-ttu-id="a8b15-108">Az Azure Data Factory lehetővé teszi, hogy a helyszíni adatforrások és a különböző Azure-szolgáltatások közötti adatátvitelt jelölik.</span><span class="sxs-lookup"><span data-stu-id="a8b15-108">Azure Data Factory enables data movement between both on-premises sources and different Azure services.</span></span>  <span data-ttu-id="a8b15-109">Azure Data Factory átfogó, a jelenlegi integráció az alábbi helyekről érkező vagy oda irányuló adatmozgás támogatja:</span><span class="sxs-lookup"><span data-stu-id="a8b15-109">Overall, current integration with Azure Data Factory supports data movement to and from the following locations:</span></span>

* <span data-ttu-id="a8b15-110">Az Azure blob-tároló</span><span class="sxs-lookup"><span data-stu-id="a8b15-110">Azure blob storage</span></span>
* <span data-ttu-id="a8b15-111">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="a8b15-111">Azure SQL Database</span></span>
* <span data-ttu-id="a8b15-112">A helyszíni SQL Server</span><span class="sxs-lookup"><span data-stu-id="a8b15-112">On-premises SQL Server</span></span>
* <span data-ttu-id="a8b15-113">SQL Server IaaS</span><span class="sxs-lookup"><span data-stu-id="a8b15-113">SQL Server on IaaS</span></span>

<span data-ttu-id="a8b15-114">Információk az adatok beállítása a másolási tevékenység: [másolja az adatokat az Azure Data Factoryvel][Copy data with Azure Data Factory]</span><span class="sxs-lookup"><span data-stu-id="a8b15-114">For information on how to set up a data copy activity see [Copy data with Azure Data Factory][Copy data with Azure Data Factory]</span></span>

## <a name="stored-procedures"></a><span data-ttu-id="a8b15-115">Tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="a8b15-115">Stored Procedures</span></span>
 <span data-ttu-id="a8b15-116">Megegyező módon használható adatátvitel ütemezése Azure Data Factory használatával is lehet levezényelni a tárolt eljárások végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="a8b15-116">In the same way it can be used to schedule data transfer, Azure Data Factory can also be used to orchestrate the execution of stored procedures.</span></span>  <span data-ttu-id="a8b15-117">Így összetettebb kimenetátirányítási hozható létre, és kibővíti az Azure Data Factory képes használni az SQL Data Warehouse számítási teljesítménnyel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a8b15-117">This allows more complex pipelines to be created and extends Azure Data Factory's ability to leverage the computational power of SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8b15-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a8b15-118">Next steps</span></span>
<span data-ttu-id="a8b15-119">Az integráció áttekintéséért lásd: [SQL Data Warehouse integrációjának áttekintése][SQL Data Warehouse integration overview].</span><span class="sxs-lookup"><span data-stu-id="a8b15-119">For an overview of integration, see [SQL Data Warehouse integration overview][SQL Data Warehouse integration overview].</span></span>
<span data-ttu-id="a8b15-120">További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="a8b15-120">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

<!--Article references-->

[Copy data with Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory documentation]:https://azure.microsoft.com/documentation/services/data-factory/

