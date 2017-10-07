---
title: "aaaManaging partneri megoldások az Azure Security Centerben |} Microsoft Docs"
description: "A dokumentumból megtudhatja, hogyan az Azure Security Center lehetővé teszi, hogy egy pillanat alatt hello állapotát az Azure-előfizetésében integrált partneri megoldások a figyelő."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: terrylan
ms.openlocfilehash: fc97aedf709b9044bfd3d4ecae0b58d5fa716bbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-partner-solutions-with-azure-security-center"></a><span data-ttu-id="fd6cb-103">Partnermegoldások figyelése az Azure Security Centerrel</span><span class="sxs-lookup"><span data-stu-id="fd6cb-103">Monitoring partner solutions with Azure Security Center</span></span>
<span data-ttu-id="fd6cb-104">A dokumentumból megtudhatja, hogyan toomonitor hello az Azure Security Centerben a partnermegoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-104">This document walks you through how toomonitor hello health status of your partner solutions in Azure Security Center.</span></span>

> [!NOTE]
> <span data-ttu-id="fd6cb-105">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-105">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="fd6cb-106">Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-106">This document is not a step-by-step guide.</span></span>
>
>

## <a name="monitoring-partner-solutions"></a><span data-ttu-id="fd6cb-107">Partnermegoldások figyelése</span><span class="sxs-lookup"><span data-stu-id="fd6cb-107">Monitoring partner solutions</span></span>
<span data-ttu-id="fd6cb-108">Hello **partneri megoldások** hello csempét **Security Center** panel segítségével egy pillanat alatt hello állapotát az Azure-előfizetésében integrált partneri megoldások a figyelheti.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-108">hello **Partner solutions** tile on hello **Security Center** blade lets you monitor at a glance hello health status of your partner solutions that are integrated with your Azure subscription.</span></span>

![Partnermegoldások csempe][1]

<span data-ttu-id="fd6cb-110">Hello **partneri megoldások** csempe az előfizetésében integrált partneri megoldások hello számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-110">hello **Partner solutions** tile displays hello number of partner solutions integrated with your subscription.</span></span> <span data-ttu-id="fd6cb-111">Ha nincsenek partnermegoldások integrálva, hello csempe hello száma 0 jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-111">If there are no solutions integrated, hello tile displays hello number zero.</span></span>

<span data-ttu-id="fd6cb-112">Partneri megoldások tooview hello állapotát:</span><span class="sxs-lookup"><span data-stu-id="fd6cb-112">tooview hello health of your partner solutions:</span></span>

1. <span data-ttu-id="fd6cb-113">Jelölje be hello **partneri megoldások** csempére.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-113">Select hello **Partner solutions** tile.</span></span> <span data-ttu-id="fd6cb-114">Hello **partneri megoldások** partneri megoldások listáját megjelenítő panel megnyitása tooSecurity Center csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-114">hello **Partner solutions** blade opens displaying a list of your partner solutions connected tooSecurity Center.</span></span>

   ![Partneri megoldások][3]

   <span data-ttu-id="fd6cb-116">egy partneri megoldást hello állapota lehet:</span><span class="sxs-lookup"><span data-stu-id="fd6cb-116">hello status of a partner solution can be:</span></span>

   * <span data-ttu-id="fd6cb-117">Védett (zöld) – nincs az állapottal kapcsolatos probléma.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-117">Protected (green) - there is no health issue.</span></span>
   * <span data-ttu-id="fd6cb-118">Nem megfelelő (piros) – azonnali figyelmet igénylő állapottal kapcsolatos probléma.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-118">Unhealthy (red) - there is a health issue that requires immediate attention.</span></span>
   * <span data-ttu-id="fd6cb-119">Leállított jelentéskészítési (narancs) – a hello megoldás leállt állapotára.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-119">Stopped reporting (orange) - hello solution has stopped reporting its health.</span></span>
   * <span data-ttu-id="fd6cb-120">Ismeretlen védelmi állapot (narancs) – hello megoldás hello állapota jelenleg ismeretlen miatt nem sikerült tooa egy új erőforrás toohello meglévő megoldás-Hozzáadás folyamata.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-120">Unknown protection status (orange) - hello health of hello solution is unknown at this time due tooa failed process of adding a new resource toohello existing solution.</span></span>
   * <span data-ttu-id="fd6cb-121">Nem jelentett (szürke) – hello megoldás még nem jelentett semmit, még a megoldás állapota lehet nem jelentett, ha nemrég csatlakozott, és még telepítés.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-121">Not reported (gray) - hello solution has not reported anything yet, a solution's status may be unreported if it has recently been connected and is still deploying.</span></span>

2. <span data-ttu-id="fd6cb-122">Válasszon ki egy partneri megoldást.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-122">Select a partner solution.</span></span> <span data-ttu-id="fd6cb-123">Ebben a példában válassza hello segítségével **Qualys** megoldás.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-123">In this example, lets select hello **Qualys** solution.</span></span>  <span data-ttu-id="fd6cb-124">Ekkor megnyílik egy panel megjelenítő hello partneri megoldás és hello megoldás hello állapotának kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-124">A blade opens showing you hello status of hello partner solution and hello solution's associated resources.</span></span> <span data-ttu-id="fd6cb-125">Válassza ki **megoldáskonzol** tooopen hello partner felület ebben a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-125">Select **Solution console** tooopen hello partner management experience for this solution.</span></span>

   ![Partneri megoldás részletei][4]
3. <span data-ttu-id="fd6cb-127">Lépjen vissza toohello **Qualys** panelhez, és válassza **hivatkozás VM**.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-127">Go back toohello **Qualys** blade and select **Link VM**.</span></span> <span data-ttu-id="fd6cb-128">Hello **összekapcsolás** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-128">hello **Link Applications** blade opens.</span></span> <span data-ttu-id="fd6cb-129">Itt csatlakoztathatja erőforrások toohello partneri megoldás.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-129">Here you can connect resources toohello partner solution.</span></span>

   ![Hivatkozás erőforrások toopartner megoldás][5]

## <a name="next-steps"></a><span data-ttu-id="fd6cb-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fd6cb-131">Next steps</span></span>
<span data-ttu-id="fd6cb-132">Ebben a dokumentumban bevezetett toohello volt **partneri megoldások** Security Center csempére.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-132">In this document, you were introduced toohello **Partner Solutions** tile in Security Center.</span></span> <span data-ttu-id="fd6cb-133">További információ a Security Center toolearn tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="fd6cb-133">toolearn more about Security Center, see hello following articles:</span></span>

* <span data-ttu-id="fd6cb-134">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – további hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-134">[Setting security policies in Azure Security Center](security-center-policies.md) — Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="fd6cb-135">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-135">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="fd6cb-136">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-136">[Security health monitoring in Azure Security Center](security-center-monitoring.md) — Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="fd6cb-137">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – további hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-137">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) — Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="fd6cb-138">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-138">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="fd6cb-139">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="fd6cb-139">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
