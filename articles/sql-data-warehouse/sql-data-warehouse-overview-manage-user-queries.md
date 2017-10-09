---
title: "az Azure SQL Data Warehouse aaaMonitor felhasználói lekérdezések |} Microsoft Docs"
description: "Hello szempontok, ajánlott eljárások és feladatok az Azure SQL Data Warehouse felhasználói lekérdezések Figyelés áttekintése"
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 1d0960db-5dcf-4a08-b1dc-6c5d3d5a616d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 67639e81b04635452e1ed844fe2d7245aa96a4fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-user-queries-in-azure-sql-data-warehouse"></a><span data-ttu-id="95c3d-103">Az Azure SQL Data Warehouse felhasználói lekérdezések figyelése</span><span class="sxs-lookup"><span data-stu-id="95c3d-103">Monitor user queries in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="95c3d-104">Hello szempontok, ajánlott eljárások és feladatok felhasználói lekérdezés az SQL Data Warehouse Figyelés áttekintése.</span><span class="sxs-lookup"><span data-stu-id="95c3d-104">Overview of hello considerations, best practices, and tasks for monitoring user queries in SQL Data Warehouse.</span></span>

| <span data-ttu-id="95c3d-105">Kategória</span><span class="sxs-lookup"><span data-stu-id="95c3d-105">Category</span></span> | <span data-ttu-id="95c3d-106">A feladat vagy szempont</span><span class="sxs-lookup"><span data-stu-id="95c3d-106">Task or consideration</span></span> | <span data-ttu-id="95c3d-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="95c3d-107">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="95c3d-108">Lassú teljesítmény</span><span class="sxs-lookup"><span data-stu-id="95c3d-108">Slow performance</span></span> |<span data-ttu-id="95c3d-109">Olyan hosszan futó felhasználói lekérdezés</span><span class="sxs-lookup"><span data-stu-id="95c3d-109">Find a long-running user query</span></span> |<span data-ttu-id="95c3d-110">[Hosszan futó lekérdezések keresése][Find long-running queries]</span><span class="sxs-lookup"><span data-stu-id="95c3d-110">[Find long-running queries][Find long-running queries]</span></span> |
| <span data-ttu-id="95c3d-111">Egyidejűség</span><span class="sxs-lookup"><span data-stu-id="95c3d-111">Concurrency</span></span> |<span data-ttu-id="95c3d-112">Egyidejű erőforrások toouser lekérdezések hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="95c3d-112">Assign concurrent resources toouser queries</span></span> |<span data-ttu-id="95c3d-113">[Párhuzamossági és munkaterhelés-kezelés][Concurrency and workload management]</span><span class="sxs-lookup"><span data-stu-id="95c3d-113">[Concurrency and workload management][Concurrency and workload management]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="95c3d-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="95c3d-114">Next steps</span></span>
<span data-ttu-id="95c3d-115">További felügyeleti tippek látogasson toohello [kezelése-áttekintés][Management overview].</span><span class="sxs-lookup"><span data-stu-id="95c3d-115">For more management tips, head over toohello [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Find long-running queries]: sql-data-warehouse-manage-monitor.md
[Concurrency and workload management]: sql-data-warehouse-develop-concurrency.md
[Management overview]: sql-data-warehouse-overview-manage.md

<!--MSDN references-->


<!--Other Web references-->
