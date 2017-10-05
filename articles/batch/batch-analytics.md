---
title: Az Azure Batch Analytics |} Microsoft Docs
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
ms.openlocfilehash: 7d634e1bb595a84b8af339e5bc5a483a7849e7f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="batch-analytics"></a><span data-ttu-id="450c6-103">Kötegelt elemzés</span><span class="sxs-lookup"><span data-stu-id="450c6-103">Batch Analytics</span></span>
<span data-ttu-id="450c6-104">Kötegelt elemzés témakörei az események és riasztások érhető el a Batch szolgáltatás erőforrásainak referencia jellegű információt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="450c6-104">The topics in Batch Analytics contain reference information for the events and alerts available for Batch service resources.</span></span>

<span data-ttu-id="450c6-105">Lásd: [Azure Batch diagnosztikai naplózás](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) engedélyezése és kötegelt diagnosztikai naplók fel további információt.</span><span class="sxs-lookup"><span data-stu-id="450c6-105">See [Azure Batch diagnostic logging](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) for more information on enabling and consuming Batch diagnostic logs.</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="450c6-106">Diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="450c6-106">Diagnostic logs</span></span>

<span data-ttu-id="450c6-107">Az Azure Batch szolgáltatás a következő diagnosztikai naplóeseményeket bizonyos kötegelt erőforrások élettartama során bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="450c6-107">The Azure Batch service emits the following diagnostic log events during the lifetime of certain Batch resources.</span></span>

<span data-ttu-id="450c6-108">**Szolgáltatás bejelentkezési események**</span><span class="sxs-lookup"><span data-stu-id="450c6-108">**Service Log events**</span></span>
* [<span data-ttu-id="450c6-109">Alkalmazáskészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="450c6-109">Pool create</span></span>](batch-pool-create-event.md)
* [<span data-ttu-id="450c6-110">Készlet törlése indítása</span><span class="sxs-lookup"><span data-stu-id="450c6-110">Pool delete start</span></span>](batch-pool-delete-start-event.md)
* [<span data-ttu-id="450c6-111">Teljes készlet törlése</span><span class="sxs-lookup"><span data-stu-id="450c6-111">Pool delete complete</span></span>](batch-pool-delete-complete-event.md)
* [<span data-ttu-id="450c6-112">Készlet átméretezési indítása</span><span class="sxs-lookup"><span data-stu-id="450c6-112">Pool resize start</span></span>](batch-pool-resize-start-event.md)
* [<span data-ttu-id="450c6-113">Teljes készlet átméretezése</span><span class="sxs-lookup"><span data-stu-id="450c6-113">Pool resize complete</span></span>](batch-pool-resize-complete-event.md)
* [<span data-ttu-id="450c6-114">A feladat indítása</span><span class="sxs-lookup"><span data-stu-id="450c6-114">Task start</span></span>](batch-task-start-event.md)
* [<span data-ttu-id="450c6-115">A feladat befejezése</span><span class="sxs-lookup"><span data-stu-id="450c6-115">Task complete</span></span>](batch-task-complete-event.md)
* [<span data-ttu-id="450c6-116">A feladat sikertelen</span><span class="sxs-lookup"><span data-stu-id="450c6-116">Task fail</span></span>](batch-task-fail-event.md)