---
title: "Támogatási jegy keresztül StorSimple Device Manager naplózása |} Microsoft Docs"
description: "Ismerteti a StorSimple Device Manager funkció felderítésére, és ismerteti a StorSimple virtuális tömb hibaelhárítás céljából."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: a0c394df-957b-49b3-a283-38824f8847fd
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 658afbc178814389fefd7941e2ca030741bd08e8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-log-a-support-request-for-the-storsimple-virtual-array"></a><span data-ttu-id="82a83-103">A StorSimple Device Manager szolgáltatás használatára egy támogatási kérést a StorSimple virtuális tömb</span><span class="sxs-lookup"><span data-stu-id="82a83-103">Use the StorSimple Device Manager service to log a Support request for the StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="82a83-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="82a83-104">Overview</span></span>

<span data-ttu-id="82a83-105">A StorSimple Device Manager lehetővé teszi a **új támogatási kérelem jelentkezzen** belül a szolgáltatás összefoglaló panelre.</span><span class="sxs-lookup"><span data-stu-id="82a83-105">The StorSimple Device Manager provides the capability to **log a new support request** within the service summary blade.</span></span> <span data-ttu-id="82a83-106">Ez a cikk azt ismerteti, hogyan új támogatási kérelem jelentkezhetnek, és a portálon az életciklus kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="82a83-106">This article explains how you can log a new support request and manage its lifecycle from within the portal.</span></span>

## <a name="new-support-request"></a><span data-ttu-id="82a83-107">Új támogatási kérelem</span><span class="sxs-lookup"><span data-stu-id="82a83-107">New support request</span></span>

<span data-ttu-id="82a83-108">Attól függően, a [támogatási csomag](https://azure.microsoft.com/support/plans/), egy problémát a támogatási jegyeket hozhatja létre a StorSimple virtuális tömbben közvetlenül a StorSimple Device Manager szolgáltatás összefoglaló panelre.</span><span class="sxs-lookup"><span data-stu-id="82a83-108">Depending upon your [support plan](https://azure.microsoft.com/support/plans/), you can create support tickets for an issue on your StorSimple Virtual array directly from the StorSimple Device Manager service summary blade.</span></span>

#### <a name="to-log-a-new-request"></a><span data-ttu-id="82a83-109">Egy új kérelmet bejelentkezni</span><span class="sxs-lookup"><span data-stu-id="82a83-109">To log a new request</span></span>

1. <span data-ttu-id="82a83-110">Nyissa meg a StorSimple-eszközkezelő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="82a83-110">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="82a83-111">A Szolgáltatásbeállítások összefoglaló panelen keresse meg **támogatási + hibaelhárítás** szakaszt, és kattintson a **új támogatja a kérelem**.</span><span class="sxs-lookup"><span data-stu-id="82a83-111">In the service summary blade settings, go to **SUPPORT + TROUBLESHOOTING** section and then click **New support request**.</span></span>
   
    ![Új támogatási kérelem](./media/storsimple-virtual-array-log-support-ticket/log-support-ticket1.png)

2. <span data-ttu-id="82a83-113">Az a **alapjai** panelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="82a83-113">In the **Basics** blade, do the following:</span></span>

    1. <span data-ttu-id="82a83-114">Az a **típusú** legördülő listában válassza ki **műszaki**.</span><span class="sxs-lookup"><span data-stu-id="82a83-114">From the **Issue type** dropdown list, select **Technical**.</span></span> 
    
    2. <span data-ttu-id="82a83-115">Az aktuális **előfizetés**, **szolgáltatás** típus, és a **erőforrás** (a StorSimple eszköz kezelő szolgáltatás) közül automatikusan választ.</span><span class="sxs-lookup"><span data-stu-id="82a83-115">The current **Subscription**, **Service** type, and the **Resource** (StorSimple Device Manager service) are automatically chosen.</span></span> 

    3. <span data-ttu-id="82a83-116">Adjon meg egy vagy több eszköz regisztrálva érintő problémákat tapasztal a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="82a83-116">Specify one or more devices registered to your service that are experiencing issues.</span></span>

    4. <span data-ttu-id="82a83-117">Válasszon egy megfelelő **támogatási csomag** Ha több tervek az Ön előfizetéséhez rendelve van.</span><span class="sxs-lookup"><span data-stu-id="82a83-117">Choose an appropriate **support plan** if you have multiple plans associated with your subscription.</span></span> <span data-ttu-id="82a83-118">Szüksége van ahhoz, hogy technikai támogatási szolgálatával fizetős támogatási csomagot.</span><span class="sxs-lookup"><span data-stu-id="82a83-118">You need a paid support plan to enable Technical Support.</span></span>

3. <span data-ttu-id="82a83-119">A **2. lépés**, válassza ki a **súlyossági** , és adja meg, ha a probléma a tömb vagy a StorSimple Device Manager szolgáltatással kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="82a83-119">In **Step 2**, choose the **Severity** and specify if the issue is related to the array or the StorSimple Device Manager service.</span></span> <span data-ttu-id="82a83-120">Válassza ki, egy **kategória** ehhez adja ki, és adjon meg több **részletek** a problémával kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="82a83-120">Also, choose a **Category** for this issue and provide more **Details** about the issue.</span></span>
   
    ![Új támogatási kérelem](./media/storsimple-virtual-array-log-support-ticket/log-support-ticket2.png)

4. <span data-ttu-id="82a83-122">A **3. lépés**, adja meg a kapcsolattartási adatait.</span><span class="sxs-lookup"><span data-stu-id="82a83-122">In **Step 3**, provide your contact information.</span></span> <span data-ttu-id="82a83-123">Microsoft Support ezen információk használatával érheti el, további információt, a diagnosztikai és a megoldás.</span><span class="sxs-lookup"><span data-stu-id="82a83-123">Microsoft Support will use this information to reach out to you for further information, diagnosis, and resolution.</span></span>
   
    ![Új támogatási kérelem](./media/storsimple-virtual-array-log-support-ticket/log-support-ticket3.png)

## <a name="manage-a-support-request"></a><span data-ttu-id="82a83-125">Egy támogatási kérést kezelése</span><span class="sxs-lookup"><span data-stu-id="82a83-125">Manage a support request</span></span>

<span data-ttu-id="82a83-126">A támogatási jegy létrehozása után a jegyet a teljes életciklusán keresztül kezelheti a portálon.</span><span class="sxs-lookup"><span data-stu-id="82a83-126">After creating a support ticket, you can manage the lifecycle of the ticket from within the portal.</span></span>

#### <a name="to-manage-your-support-requests"></a><span data-ttu-id="82a83-127">A támogatási kérelmek kezelése</span><span class="sxs-lookup"><span data-stu-id="82a83-127">To manage your support requests</span></span>

<span data-ttu-id="82a83-128">Ahhoz, hogy a Súgó és támogatás oldalra, keresse meg **Tallózás > Súgó + támogatás**.</span><span class="sxs-lookup"><span data-stu-id="82a83-128">To get to the help and support page, navigate to **Browse > Help + support**.</span></span>

![Támogatási kérelmek kezelése](./media/storsimple-virtual-array-log-support-ticket/manage-support-tickets.png)

## <a name="next-steps"></a><span data-ttu-id="82a83-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="82a83-130">Next steps</span></span>

<span data-ttu-id="82a83-131">Megtudhatja, hogyan [diagnosztizálhatja és a StorSimple virtuális tömb kapcsolatos problémák megoldásához](storsimple-virtual-array-diagnose-problems.md)</span><span class="sxs-lookup"><span data-stu-id="82a83-131">Learn how to [diagnose and solve problems related to your StorSimple Virtual array](storsimple-virtual-array-diagnose-problems.md)</span></span>

