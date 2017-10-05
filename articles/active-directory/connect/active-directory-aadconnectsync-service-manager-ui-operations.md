---
title: "Az Azure AD Connect Synchronization Service Managert műveletek |} Microsoft Docs"
description: "Ismerje meg a műveletek fülre a Synchronization Service Managert, az Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1475e4fcd11eb008badba49665f4af6029a1697
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="using-the-sync-service-manager-operations-tab"></a><span data-ttu-id="ba002-103">A Service Manager szinkronizálási műveletek lapján</span><span class="sxs-lookup"><span data-stu-id="ba002-103">Using the Sync Service Manager Operations tab</span></span>

![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

<span data-ttu-id="ba002-105">A műveletek lapon a legutóbbi művelet eredményei láthatók.</span><span class="sxs-lookup"><span data-stu-id="ba002-105">The operations tab shows the results from the most recent operations.</span></span> <span data-ttu-id="ba002-106">Ezen a lapon az kulcs megértéséhez, valamint a problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="ba002-106">This tab is key to understand and troubleshoot issues.</span></span>

## <a name="understand-the-information-visible-in-the-operations-tab"></a><span data-ttu-id="ba002-107">A műveletek lapon látható információk ismertetése</span><span class="sxs-lookup"><span data-stu-id="ba002-107">Understand the information visible in the operations tab</span></span>
<span data-ttu-id="ba002-108">Felső részén látható minden futtatásakor időrendi sorrendben.</span><span class="sxs-lookup"><span data-stu-id="ba002-108">The top half shows all runs in chronological order.</span></span> <span data-ttu-id="ba002-109">Alapértelmezés szerint a műveletek az elmúlt hét napban tartja információinak naplózásához, de ez a beállítás használatával módosítható a [Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md).</span><span class="sxs-lookup"><span data-stu-id="ba002-109">By default, the operations log keeps information about the last seven days, but this setting can be changed with the [scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span></span> <span data-ttu-id="ba002-110">Bármely futtatása, amelyek nem szerepelnek egy sikeres állapotnak keresni szeretne.</span><span class="sxs-lookup"><span data-stu-id="ba002-110">You want to look for any run that does not show a success status.</span></span> <span data-ttu-id="ba002-111">Rendezés a fejlécek kattintva módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="ba002-111">You can change the sorting by clicking the headers.</span></span>

<span data-ttu-id="ba002-112">A **állapot** oszlop a legfontosabb adatokat, és a legsúlyosabb károkat okozó problémát futtató jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ba002-112">The **Status** column is the most important information and shows the most severe problem for a run.</span></span> <span data-ttu-id="ba002-113">A leggyakoribb állapotokról a prioritásuk szerinti sorrendben kell vizsgálni gyors összegzése (ahol * több lehetséges hiba karakterláncok jelzi).</span><span class="sxs-lookup"><span data-stu-id="ba002-113">Here is a quick summary of the most common statuses in order of priority to investigate (where * indicate several possible error strings).</span></span>

| <span data-ttu-id="ba002-114">status</span><span class="sxs-lookup"><span data-stu-id="ba002-114">Status</span></span> | <span data-ttu-id="ba002-115">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="ba002-115">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="ba002-116">leállított-*</span><span class="sxs-lookup"><span data-stu-id="ba002-116">stopped-*</span></span> |<span data-ttu-id="ba002-117">A Futtatás nem sikerült befejezni.</span><span class="sxs-lookup"><span data-stu-id="ba002-117">The run could not complete.</span></span> <span data-ttu-id="ba002-118">Ha például a távoli rendszer nem működik, és nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="ba002-118">For example, if the remote system is down and cannot be contacted.</span></span> |
| <span data-ttu-id="ba002-119">leállt hiba-korlát</span><span class="sxs-lookup"><span data-stu-id="ba002-119">stopped-error-limit</span></span> |<span data-ttu-id="ba002-120">Több mint 5000 hibák vannak.</span><span class="sxs-lookup"><span data-stu-id="ba002-120">There are more than 5,000 errors.</span></span> <span data-ttu-id="ba002-121">A Futtatás automatikusan hibák nagy száma miatt le lett állítva.</span><span class="sxs-lookup"><span data-stu-id="ba002-121">The run was automatically stopped due to the large number of errors.</span></span> |
| <span data-ttu-id="ba002-122">Befejezett -\*-hibák</span><span class="sxs-lookup"><span data-stu-id="ba002-122">completed-\*-errors</span></span> |<span data-ttu-id="ba002-123">A Futtatás befejeződött, de hibák (kevesebb mint 5000) meg kell vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="ba002-123">The run completed, but there are errors (fewer than 5,000) that should be investigated.</span></span> |
| <span data-ttu-id="ba002-124">Befejezett -\*-figyelmeztetések</span><span class="sxs-lookup"><span data-stu-id="ba002-124">completed-\*-warnings</span></span> |<span data-ttu-id="ba002-125">A Futtatás befejeződött, de bizonyos adatok nem várt állapotban van.</span><span class="sxs-lookup"><span data-stu-id="ba002-125">The run completed, but some data is not in the expected state.</span></span> <span data-ttu-id="ba002-126">Ha hibákat, majd ezt az üzenetet az általában csak az egyik oka.</span><span class="sxs-lookup"><span data-stu-id="ba002-126">If you have errors, then this message is usually only a symptom.</span></span> <span data-ttu-id="ba002-127">Mindaddig, amíg meg kell oldani hibák, figyelmeztetések nem ki kell vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="ba002-127">Until you have addressed errors, you should not investigate warnings.</span></span> |
| <span data-ttu-id="ba002-128">sikeres</span><span class="sxs-lookup"><span data-stu-id="ba002-128">success</span></span> |<span data-ttu-id="ba002-129">Nincs probléma.</span><span class="sxs-lookup"><span data-stu-id="ba002-129">No issues.</span></span> |

<span data-ttu-id="ba002-130">Amikor kiválaszt egy sorra, alsó frissítései futtató részleteinek megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="ba002-130">When you select a row, the bottom updates to show the details of that run.</span></span> <span data-ttu-id="ba002-131">A bal alsó a, lehetséges, hogy egy lista bármelyiket **lépés #**.</span><span class="sxs-lookup"><span data-stu-id="ba002-131">To the far left of the bottom, you might have a list saying **Step #**.</span></span> <span data-ttu-id="ba002-132">Ez a lista csak akkor jelenik meg, ha több tartomány az erdőben, ahol minden egyes tartományhoz egy lépés képviseli.</span><span class="sxs-lookup"><span data-stu-id="ba002-132">This list only appears if you have multiple domains in your forest where each domain is represented by a step.</span></span> <span data-ttu-id="ba002-133">A tartománynév címszó alatt található **partíció**.</span><span class="sxs-lookup"><span data-stu-id="ba002-133">The domain name can be found under the heading **Partition**.</span></span> <span data-ttu-id="ba002-134">A **szinkronizálási statisztika**, található további információ a feldolgozott módosítások számát.</span><span class="sxs-lookup"><span data-stu-id="ba002-134">Under **Synchronization Statistics**, you can find more information about the number of changes that were processed.</span></span> <span data-ttu-id="ba002-135">Kattintson az alábbi hivatkozásokat a módosított objektumok listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ba002-135">You can click the links to get a list of the changed objects.</span></span> <span data-ttu-id="ba002-136">Ha hibákkal objektummal rendelkezik, azokat a hibákat az Eseménynaplón **szinkronizálási hibák**.</span><span class="sxs-lookup"><span data-stu-id="ba002-136">If you have objects with errors, those errors show up under **Synchronization Errors**.</span></span>

<span data-ttu-id="ba002-137">További információkért lásd: [olyan objektum, amely nem szinkronizál hibaelhárítása](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)</span><span class="sxs-lookup"><span data-stu-id="ba002-137">For more information, see [troubleshoot an object that is not synchronizing](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba002-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ba002-138">Next steps</span></span>
<span data-ttu-id="ba002-139">További információ a [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="ba002-139">Learn more about the [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="ba002-140">További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="ba002-140">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
