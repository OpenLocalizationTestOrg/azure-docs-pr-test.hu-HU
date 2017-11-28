---
title: "Az Azure Stream Analytics-feladatok folyamatossága |} Microsoft Docs"
description: "Így a Stream Analytics útmutatást frissítés rugalmas feladatok."
services: stream-analytics
documentationCenter: 
authors: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 8dc19e1b37082c87d2990ad910d1af786f8b9280
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a><span data-ttu-id="f3e99-103">Stream Analytics-feladat megbízhatóságának biztosítása szolgáltatás frissítései során</span><span class="sxs-lookup"><span data-stu-id="f3e99-103">Guarantee Stream Analytics job reliability during service updates</span></span>

<span data-ttu-id="f3e99-104">Egy teljes körűen felügyelt szolgáltatás, amely nem része a funkció új szolgáltatás funkcióit és fejlesztéseit gyors ütemben bevezetése.</span><span class="sxs-lookup"><span data-stu-id="f3e99-104">Part of being a fully managed service is the capability to introduce new service functionality and improvements at a rapid pace.</span></span> <span data-ttu-id="f3e99-105">Ennek eredményeképpen a Stream Analytics heti (vagy gyakoribb) telepítése szolgáltatásfrissítés is rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="f3e99-105">As a result, Stream Analytics can have a service update deploy on a weekly (or more frequent) basis.</span></span> <span data-ttu-id="f3e99-106">Függetlenül történik, hogy mekkora tesztelése nincs továbbra is fennáll a kockázata, hogy egy meglévő, futó feladat megszakadhat bevezetése egy hiba miatt.</span><span class="sxs-lookup"><span data-stu-id="f3e99-106">No matter how much testing is done there is still a risk that an existing, running job may break due to the introduction of a bug.</span></span> <span data-ttu-id="f3e99-107">Az ügyfelek, akik kritikus adatfolyam feldolgozási feladatok futtatása a kockázatok kell kerülni kell.</span><span class="sxs-lookup"><span data-stu-id="f3e99-107">For customers who run critical streaming processing jobs these risks need to be avoided.</span></span> <span data-ttu-id="f3e99-108">Az ügyfelek használhatják a kockázatok csökkentése mechanizmusra Azure  **[párosított régió](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  modell.</span><span class="sxs-lookup"><span data-stu-id="f3e99-108">A mechanism customers can use to reduce this risk is Azure’s **[paired region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** model.</span></span> 

## <a name="how-do-azure-paired-regions-address-this-concern"></a><span data-ttu-id="f3e99-109">Hogyan hajtsa végre az Azure párosított régiók ezen probléma megoldásának?</span><span class="sxs-lookup"><span data-stu-id="f3e99-109">How do Azure paired regions address this concern?</span></span>

<span data-ttu-id="f3e99-110">A Stream Analytics biztosítja, hogy külön párosított régiókban feladatok frissülnek.</span><span class="sxs-lookup"><span data-stu-id="f3e99-110">Stream Analytics guarantees jobs in paired regions are updated in separate batches.</span></span> <span data-ttu-id="f3e99-111">Ennek eredményeképpen nincs elegendő idő hiány között lehetséges legfrissebb hibák azonosítása és javítása azokat a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="f3e99-111">As a result there is a sufficient time gap between the updates to identify potential breaking bugs and remediate them.</span></span>

<span data-ttu-id="f3e99-112">_Közép-Indiában kivételével_ (amelynek párosított régió, Dél-Indiában és nem rendelkezik Stream Analytics jelenléte), a Stream Analytics egy frissítés telepítése nem lépne párosított régiók meg egyszerre.</span><span class="sxs-lookup"><span data-stu-id="f3e99-112">_With the exception of Central India_ (whose paired region, South India, does not have Stream Analytics presence), the deployment of an update to Stream Analytics would not occur at the same time in a set of paired regions.</span></span> <span data-ttu-id="f3e99-113">A központi telepítések több régióba **ugyanabban a csoportban** fordulhat **egyszerre**.</span><span class="sxs-lookup"><span data-stu-id="f3e99-113">Deployments in multiple regions **in the same group** may occur **at the same time**.</span></span>

<span data-ttu-id="f3e99-114">A cikk a  **[rendelkezésre állását és párosított régiók](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  a legfrissebb információk, amelyen a régiók párosítva van.</span><span class="sxs-lookup"><span data-stu-id="f3e99-114">The article on **[availability and paired regions](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** has the most up-to-date information on which regions are paired.</span></span>

<span data-ttu-id="f3e99-115">Ügyfelek központi telepítése azonos feladatok mindkét párosított régió végigvitelével.</span><span class="sxs-lookup"><span data-stu-id="f3e99-115">Customers are advised to deploy identical jobs to both paired regions.</span></span> <span data-ttu-id="f3e99-116">A Stream Analytics belső figyelési képességek, valamint az ügyfelek is javasolja, hogy a feladatok figyelése, mintha **mindkét** éles feladat.</span><span class="sxs-lookup"><span data-stu-id="f3e99-116">In addition to Stream Analytics internal monitoring capabilities, customers are also advised to monitor the jobs as if **both** are production jobs.</span></span> <span data-ttu-id="f3e99-117">Ha szünet kell lennie a Stream Analytics szolgáltatás frissítés eredménye, eszkalálása megfelelően, és a megfelelő feladat kimeneti alárendelt fogyasztóval feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="f3e99-117">If a break is identified to be a result of the Stream Analytics service update, escalate appropriately and fail over any downstream consumers to the healthy job output.</span></span> <span data-ttu-id="f3e99-118">Eszkalációs támogatásához a párosított régió megakadályozza az új központi telepítés vonatkozzon, és a párosított feladatot integritásának fenntartása.</span><span class="sxs-lookup"><span data-stu-id="f3e99-118">Escalation to support will prevent the paired region from being affected by the new deployment and maintain the integrity of the paired jobs.</span></span>
