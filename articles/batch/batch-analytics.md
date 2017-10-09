---
title: "Kötegelt elemzés aaaAzure |} Microsoft Docs"
description: "Az Azure Batch Analytics hivatkozását."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 462fbad1ac522b485ae18c1e8891b9d2cabd45e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="batch-analytics"></a><span data-ttu-id="2f52a-103">Kötegelt elemzés</span><span class="sxs-lookup"><span data-stu-id="2f52a-103">Batch Analytics</span></span>
<span data-ttu-id="2f52a-104">Kötegelt elemzés hello témakörei hello események és riasztások érhető el a Batch szolgáltatás erőforrások referencia jellegű információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2f52a-104">hello topics in Batch Analytics contain reference information for hello events and alerts available for Batch service resources.</span></span>

<span data-ttu-id="2f52a-105">Lásd: [Azure Batch diagnosztikai naplózás](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) engedélyezése és kötegelt diagnosztikai naplók fel további információt.</span><span class="sxs-lookup"><span data-stu-id="2f52a-105">See [Azure Batch diagnostic logging](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) for more information on enabling and consuming Batch diagnostic logs.</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="2f52a-106">Diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="2f52a-106">Diagnostic logs</span></span>

<span data-ttu-id="2f52a-107">Azure Batch szolgáltatás hello diagnosztikai naplóeseményeket bizonyos kötegelt erőforrások hello élettartama során a következő hello bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="2f52a-107">hello Azure Batch service emits hello following diagnostic log events during hello lifetime of certain Batch resources.</span></span>

<span data-ttu-id="2f52a-108">**Szolgáltatás bejelentkezési események**</span><span class="sxs-lookup"><span data-stu-id="2f52a-108">**Service Log events**</span></span>
* [<span data-ttu-id="2f52a-109">Alkalmazáskészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f52a-109">Pool create</span></span>](batch-pool-create-event.md)
* [<span data-ttu-id="2f52a-110">Készlet törlése indítása</span><span class="sxs-lookup"><span data-stu-id="2f52a-110">Pool delete start</span></span>](batch-pool-delete-start-event.md)
* [<span data-ttu-id="2f52a-111">Teljes készlet törlése</span><span class="sxs-lookup"><span data-stu-id="2f52a-111">Pool delete complete</span></span>](batch-pool-delete-complete-event.md)
* [<span data-ttu-id="2f52a-112">Készlet átméretezési indítása</span><span class="sxs-lookup"><span data-stu-id="2f52a-112">Pool resize start</span></span>](batch-pool-resize-start-event.md)
* [<span data-ttu-id="2f52a-113">Teljes készlet átméretezése</span><span class="sxs-lookup"><span data-stu-id="2f52a-113">Pool resize complete</span></span>](batch-pool-resize-complete-event.md)
* [<span data-ttu-id="2f52a-114">A feladat indítása</span><span class="sxs-lookup"><span data-stu-id="2f52a-114">Task start</span></span>](batch-task-start-event.md)
* [<span data-ttu-id="2f52a-115">A feladat befejezése</span><span class="sxs-lookup"><span data-stu-id="2f52a-115">Task complete</span></span>](batch-task-complete-event.md)
* [<span data-ttu-id="2f52a-116">A feladat sikertelen</span><span class="sxs-lookup"><span data-stu-id="2f52a-116">Task fail</span></span>](batch-task-fail-event.md)