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
ms.openlocfilehash: 3ac65c93ecb47e93e963dd9869a7af70f73b19c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a><span data-ttu-id="47889-103">Stream Analytics-feladat megbízhatóságának biztosítása szolgáltatás frissítései során</span><span class="sxs-lookup"><span data-stu-id="47889-103">Guarantee Stream Analytics job reliability during service updates</span></span>

<span data-ttu-id="47889-104">Egy teljes körűen felügyelt szolgáltatás, amely nem része hello funkció toointroduce új szolgáltatás funkcióit és fejlesztéseit gyors ütemben.</span><span class="sxs-lookup"><span data-stu-id="47889-104">Part of being a fully managed service is hello capability toointroduce new service functionality and improvements at a rapid pace.</span></span> <span data-ttu-id="47889-105">Ennek eredményeképpen a Stream Analytics heti (vagy gyakoribb) telepítése szolgáltatásfrissítés is rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="47889-105">As a result, Stream Analytics can have a service update deploy on a weekly (or more frequent) basis.</span></span> <span data-ttu-id="47889-106">Függetlenül történik, hogy mekkora tesztelése nincs továbbra is fennáll a kockázata, hogy egy meglévő, futó feladat megszakadhat toohello bevezetése egy hiba miatt.</span><span class="sxs-lookup"><span data-stu-id="47889-106">No matter how much testing is done there is still a risk that an existing, running job may break due toohello introduction of a bug.</span></span> <span data-ttu-id="47889-107">Az ügyfelek, akik kritikus adatfolyam feldolgozási feladatok futtatása a kockázatok kell toobe kerülni.</span><span class="sxs-lookup"><span data-stu-id="47889-107">For customers who run critical streaming processing jobs these risks need toobe avoided.</span></span> <span data-ttu-id="47889-108">A mechanizmus az ügyfelek használhatják tooreduce ennek a veszélyét Azure  **[párosított régió](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  modell.</span><span class="sxs-lookup"><span data-stu-id="47889-108">A mechanism customers can use tooreduce this risk is Azure’s **[paired region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** model.</span></span> 

## <a name="how-do-azure-paired-regions-address-this-concern"></a><span data-ttu-id="47889-109">Hogyan hajtsa végre az Azure párosított régiók ezen probléma megoldásának?</span><span class="sxs-lookup"><span data-stu-id="47889-109">How do Azure paired regions address this concern?</span></span>

<span data-ttu-id="47889-110">A Stream Analytics biztosítja, hogy külön párosított régiókban feladatok frissülnek.</span><span class="sxs-lookup"><span data-stu-id="47889-110">Stream Analytics guarantees jobs in paired regions are updated in separate batches.</span></span> <span data-ttu-id="47889-111">Ennek eredményeképpen nincs elegendő idő hiány között hello frissítések tooidentify lehetséges legfrissebb hibák elhárítása, és elháríthatja a.</span><span class="sxs-lookup"><span data-stu-id="47889-111">As a result there is a sufficient time gap between hello updates tooidentify potential breaking bugs and remediate them.</span></span>

<span data-ttu-id="47889-112">_Közép-Indiában hello kivételével_ (amelynek párosított régió, Dél-Indiában és nem rendelkezik Stream Analytics jelenléte), egy frissítés tooStream elemzés nem lépne hello: hello telepítését azonos idő párosított régiók meg.</span><span class="sxs-lookup"><span data-stu-id="47889-112">_With hello exception of Central India_ (whose paired region, South India, does not have Stream Analytics presence), hello deployment of an update tooStream Analytics would not occur at hello same time in a set of paired regions.</span></span> <span data-ttu-id="47889-113">A központi telepítések több régióba **hello a ugyanabban a csoportban** fordulhat elő, **: hello ugyanannyi időt vesz igénybe**.</span><span class="sxs-lookup"><span data-stu-id="47889-113">Deployments in multiple regions **in hello same group** may occur **at hello same time**.</span></span>

<span data-ttu-id="47889-114">hello foglalkozó  **[rendelkezésre állását és párosított régiók](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  hello legtöbb naprakész információt, amelyen a régiók párosítva van.</span><span class="sxs-lookup"><span data-stu-id="47889-114">hello article on **[availability and paired regions](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** has hello most up-to-date information on which regions are paired.</span></span>

<span data-ttu-id="47889-115">Elkeverni toodeploy azonos feladatok párosítva tooboth régiók azokat a felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="47889-115">Customers are advised toodeploy identical jobs tooboth paired regions.</span></span> <span data-ttu-id="47889-116">Továbbá a belső figyelési képességek, az ügyfelek Analytics megtalálhatók tooStream ajánlott toomonitor hello feladatok, mintha **mindkét** éles feladat.</span><span class="sxs-lookup"><span data-stu-id="47889-116">In addition tooStream Analytics internal monitoring capabilities, customers are also advised toomonitor hello jobs as if **both** are production jobs.</span></span> <span data-ttu-id="47889-117">Ha szünet azonosított toobe hello Stream Analytics szolgáltatás frissítés eredménye, eszkalálása megfelelően, és alsóbb rétegbeli fogyasztók toohello kifogástalan feladat kimenete a feladatátvétel.</span><span class="sxs-lookup"><span data-stu-id="47889-117">If a break is identified toobe a result of hello Stream Analytics service update, escalate appropriately and fail over any downstream consumers toohello healthy job output.</span></span> <span data-ttu-id="47889-118">Eszkalációs toosupport hello párosított régió megakadályozása hello új központi telepítési vonatkozzon, és a párosítva hello feladatok hello integritásának fenntartása.</span><span class="sxs-lookup"><span data-stu-id="47889-118">Escalation toosupport will prevent hello paired region from being affected by hello new deployment and maintain hello integrity of hello paired jobs.</span></span>
