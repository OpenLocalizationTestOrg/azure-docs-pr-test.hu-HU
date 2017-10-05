---
title: "Az Azure SQL Data Warehouse felhasználói lekérdezések figyelése |} Microsoft Docs"
description: "A szempontok, ajánlott eljárások és a feladatokat az Azure SQL Data Warehouse felhasználói lekérdezések Figyelés áttekintése"
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
ms.openlocfilehash: 65509a65c2b34553822cc02d7a7fa5a614adc57f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-user-queries-in-azure-sql-data-warehouse"></a><span data-ttu-id="12181-103">Az Azure SQL Data Warehouse felhasználói lekérdezések figyelése</span><span class="sxs-lookup"><span data-stu-id="12181-103">Monitor user queries in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="12181-104">A szempontok, ajánlott eljárások és a feladatokat az SQL Data Warehouse felhasználói lekérdezések Figyelés áttekintése.</span><span class="sxs-lookup"><span data-stu-id="12181-104">Overview of the considerations, best practices, and tasks for monitoring user queries in SQL Data Warehouse.</span></span>

| <span data-ttu-id="12181-105">Kategória</span><span class="sxs-lookup"><span data-stu-id="12181-105">Category</span></span> | <span data-ttu-id="12181-106">A feladat vagy szempont</span><span class="sxs-lookup"><span data-stu-id="12181-106">Task or consideration</span></span> | <span data-ttu-id="12181-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="12181-107">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="12181-108">Lassú teljesítmény</span><span class="sxs-lookup"><span data-stu-id="12181-108">Slow performance</span></span> |<span data-ttu-id="12181-109">Olyan hosszan futó felhasználói lekérdezés</span><span class="sxs-lookup"><span data-stu-id="12181-109">Find a long-running user query</span></span> |<span data-ttu-id="12181-110">[Hosszan futó lekérdezések keresése][Find long-running queries]</span><span class="sxs-lookup"><span data-stu-id="12181-110">[Find long-running queries][Find long-running queries]</span></span> |
| <span data-ttu-id="12181-111">Egyidejűség</span><span class="sxs-lookup"><span data-stu-id="12181-111">Concurrency</span></span> |<span data-ttu-id="12181-112">Felhasználói lekérdezések párhuzamos erőforrások hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="12181-112">Assign concurrent resources to user queries</span></span> |<span data-ttu-id="12181-113">[Párhuzamossági és munkaterhelés-kezelés][Concurrency and workload management]</span><span class="sxs-lookup"><span data-stu-id="12181-113">[Concurrency and workload management][Concurrency and workload management]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="12181-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12181-114">Next steps</span></span>
<span data-ttu-id="12181-115">További tudnivalók az felügyeleti, látogasson el a [kezelése-áttekintés][Management overview].</span><span class="sxs-lookup"><span data-stu-id="12181-115">For more management tips, head over to the [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Find long-running queries]: sql-data-warehouse-manage-monitor.md
[Concurrency and workload management]: sql-data-warehouse-develop-concurrency.md
[Management overview]: sql-data-warehouse-overview-manage.md

<!--MSDN references-->


<!--Other Web references-->
