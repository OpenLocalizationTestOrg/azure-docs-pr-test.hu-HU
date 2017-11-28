---
title: "Biztonsági riasztások kezelése az Azure Security Centerben | Microsoft Docs"
description: "Ebből a dokumentumból elsajátíthatja az Azure Security Center a biztonsági incidensek kezeléséhez szükséges képességeinek alkalmazását."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: e8feb669-8f30-49eb-ba38-046edf3f9656
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: a302f8cb2555eef469a24da2523fdd9b97cc5730
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="handling-security-incidents-in-azure-security-center"></a><span data-ttu-id="ca451-103">Biztonsági incidensek kezelése az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="ca451-103">Handling Security Incidents in Azure Security Center</span></span>
<span data-ttu-id="ca451-104">A biztonsági riasztások osztályba sorolása és kivizsgálása még a legképzettebb biztonsági elemzők számára is időigényes feladat lehet, sokak számára pedig már annak megtalálása is nehézséget okoz, hogy hol kezdjenek hozzá.</span><span class="sxs-lookup"><span data-stu-id="ca451-104">Triaging and investigating security alerts can be time consuming for even the most skilled security analysts, and for many it is hard to even know where to begin.</span></span> <span data-ttu-id="ca451-105">A különálló [biztonsági riasztások](security-center-managing-and-responding-alerts.md) adatait összekapcsoló [elemzési szolgáltatások](security-center-detection-capabilities.md) alkalmazásával a Security Center a támadássorozatot és az összes kapcsolódó riasztást egyetlen nézetben jeleníti meg, így gyorsan áttekinthetővé válnak a támadó által végrehajtott műveletek és az érintett erőforrások.</span><span class="sxs-lookup"><span data-stu-id="ca451-105">By using [analytics](security-center-detection-capabilities.md) to connect the information between distinct [security alerts](security-center-managing-and-responding-alerts.md), Security Center can provide you with a single view of an attack campaign and all of the related alerts – you can quickly understand what actions the attacker took and what resources were impacted.</span></span>

<span data-ttu-id="ca451-106">Ebben a dokumentumban megismerkedhet a Security Center biztonságiriasztás-kezelési funkcióinak használatával.</span><span class="sxs-lookup"><span data-stu-id="ca451-106">This document discusses how to use security alert capability in Security Center to assist you handling security incidents.</span></span>

## <a name="what-is-a-security-incident"></a><span data-ttu-id="ca451-107">Mi az a biztonsági incidens?</span><span class="sxs-lookup"><span data-stu-id="ca451-107">What is a security incident?</span></span>
<span data-ttu-id="ca451-108">A Security Centerben egy biztonsági incidens az adott erőforráshoz tartozó összes olyan riasztás együttese, amelyek egy [támadási folyamatba](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) illeszthetők.</span><span class="sxs-lookup"><span data-stu-id="ca451-108">In Security Center, a security incident is an aggregation of all alerts for a resource that align with [kill chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) patterns.</span></span> <span data-ttu-id="ca451-109">Ezek az incidensek megjelennek a [Security Alerts](security-center-managing-and-responding-alerts.md) (Biztonsági riasztások) csempén és panelen is.</span><span class="sxs-lookup"><span data-stu-id="ca451-109">Incidents appear in the [Security Alerts](security-center-managing-and-responding-alerts.md) tile and blade.</span></span> <span data-ttu-id="ca451-110">Az incidensek megnyitásakor megjelenik a kapcsolódó riasztások listája, amelyből további információkat kaphat az egyes riasztásokról.</span><span class="sxs-lookup"><span data-stu-id="ca451-110">An Incident will reveal the list of related alerts, which enables you to obtain more information about each occurrence.</span></span>

## <a name="managing-security-incidents"></a><span data-ttu-id="ca451-111">Biztonsági incidensek kezelése</span><span class="sxs-lookup"><span data-stu-id="ca451-111">Managing security incidents</span></span>
<span data-ttu-id="ca451-112">A Biztonsági riasztások csempén áttekintheti az aktuális biztonsági incidenseket.</span><span class="sxs-lookup"><span data-stu-id="ca451-112">You can review your current security incidents by looking at the security alerts tile.</span></span> <span data-ttu-id="ca451-113">Nyissa meg az Azure Portalt, és kövesse az alábbi lépéseket az egyes biztonsági incidensek részleteinek megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="ca451-113">Access the Azure Portal and follow the steps below to see more details about each security incident:</span></span>

1. <span data-ttu-id="ca451-114">A Security Center irányítópultján található a **Biztonsági riasztások** csempe.</span><span class="sxs-lookup"><span data-stu-id="ca451-114">On the Security Center dashboard, you will see the **Security alerts** tile.</span></span>

    ![Biztonsági riasztások csempe a Security Centerben](./media/security-center-incident/security-center-incident-fig1.png)

2. <span data-ttu-id="ca451-116">A csempére kattintva bontsa ki azt, ha pedig a rendszer biztonsági incidenst észlel, az a biztonsági riasztások diagram alatt jelenik meg, ahogyan az az alábbi ábrán látható:</span><span class="sxs-lookup"><span data-stu-id="ca451-116">Click on this tile to expand it and if a security incident is detected, it will appear under the security alerts graph as shown  below:</span></span>

    ![Biztonsági incidens](./media/security-center-incident/security-center-incident-fig2.png)

3. <span data-ttu-id="ca451-118">A biztonsági incidensek leírásának a többi riasztástól eltérő ikonja van.</span><span class="sxs-lookup"><span data-stu-id="ca451-118">Notice that the security incident description has a different icon compared to other alerts.</span></span> <span data-ttu-id="ca451-119">Kattintson erre az ikonra az incidens részleteinek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="ca451-119">Click on it to view more details about this incident.</span></span>

    ![Biztonsági incidens](./media/security-center-incident/security-center-incident-fig3.png)

4. <span data-ttu-id="ca451-121">Az **incidens** panelen további részleteket tekinthet meg az adott biztonsági incidensről, többek között annak teljes leírását, súlyosságát (ez esetben magas), aktuális állapotát (ez esetben továbbra is *aktív*, ami azt jelenti, hogy a felhasználó még nem hajtott végre rajta műveletet – ezt úgy teheti meg, ha a jobb gombbal a **Biztonsági riasztások** panelen látható incidensre kattint), a megtámadott erőforrást (amely ez esetben a *VM1*), az incidens elhárításához szükséges lépéseket, az alsó panelen pedig az incidens során előfordult riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="ca451-121">On the **incident** blade you will see more details about this security incident, which includes its full description, its severity (which in this case is high), its current state (in this case it is still *active*, which implies the user hasn't taken an action to it - this can be done by right clicking on the incident in the **Security alerts** blade), the attacked resource (in this case *VM1*), the remediation steps for the incident, and in the bottom pane you have the alerts that were included in this incident.</span></span> <span data-ttu-id="ca451-122">Ha további információkat kíván megtudni az egyes riasztásokról, egyszerűen kattintson rájuk. Ekkor egy újabb panel nyílik meg, ahogy az az alábbi képen is látható:</span><span class="sxs-lookup"><span data-stu-id="ca451-122">If you want to obtain more information on each alert, just click on it and another blade will open, as shown below:</span></span>

    ![Biztonsági incidens](./media/security-center-incident/security-center-incident-fig4.png)

<span data-ttu-id="ca451-124">A panelen megjelenő információk a riasztástól függően változnak.</span><span class="sxs-lookup"><span data-stu-id="ca451-124">The information on this blade will vary according to the alert.</span></span> <span data-ttu-id="ca451-125">Ezen riasztások kezelésével kapcsolatos további információkért olvassa el a [Biztonsági riasztások kezelése és válaszadás a riasztásokra az Azure Security Centerben](security-center-managing-and-responding-alerts.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="ca451-125">Read [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) for more information on how to manage these alerts.</span></span> <span data-ttu-id="ca451-126">Ezzel a képességgel kapcsolatos fontos szempontok:</span><span class="sxs-lookup"><span data-stu-id="ca451-126">Some important considerations regarding this capability:</span></span>

* <span data-ttu-id="ca451-127">Egy új szűrő alkalmazásával testre szabhatja a nézetet csak incidensek, csak riasztások vagy mindkettő megjelenítésére.</span><span class="sxs-lookup"><span data-stu-id="ca451-127">A new filter enables you to customize your view to Incident only, Alerts only, or both.</span></span>
* <span data-ttu-id="ca451-128">Egy adott riasztás kezelhető egy incidens (ha van) részeként, valamint önálló riasztásként is.</span><span class="sxs-lookup"><span data-stu-id="ca451-128">The same alert can exist as part of an Incident (if applicable), as well as to be visible as a standalone alert.</span></span>

## <a name="see-also"></a><span data-ttu-id="ca451-129">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="ca451-129">See also</span></span>
<span data-ttu-id="ca451-130">Ebben a dokumentumban megismerkedhetett a Security Center biztonságiincidens-kezelési képességeinek használatával.</span><span class="sxs-lookup"><span data-stu-id="ca451-130">In this document, you learned how to use the security incident capability in Security Center.</span></span> <span data-ttu-id="ca451-131">A Security Centerrel kapcsolatos további információkért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="ca451-131">To learn more about Security Center, see the following:</span></span>

* [<span data-ttu-id="ca451-132">Biztonsági riasztások kezelése és válaszadás a riasztásokra az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="ca451-132">Managing and responding to security alerts in Azure Security Center</span></span>](security-center-managing-and-responding-alerts.md)
* [<span data-ttu-id="ca451-133">Az Azure Security Center észlelési funkciói</span><span class="sxs-lookup"><span data-stu-id="ca451-133">Azure Security Center Detection Capabilities</span></span>](security-center-detection-capabilities.md)
* [<span data-ttu-id="ca451-134">Útmutató az Azure Security Center tervezéséhez és működtetéséhez</span><span class="sxs-lookup"><span data-stu-id="ca451-134">Azure Security Center Planning and Operations Guide</span></span>](security-center-planning-and-operations-guide.md)
* [<span data-ttu-id="ca451-135">Biztonsági riasztások kezelése és válaszadás a riasztásokra az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="ca451-135">Managing and responding to security alerts in Azure Security Center</span></span>](security-center-managing-and-responding-alerts.md)
* <span data-ttu-id="ca451-136">[Azure Security Center – gyakran ismételt kérdések](security-center-faq.md) – Gyakran ismételt kérdések a szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="ca451-136">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="ca451-137">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="ca451-137">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Find blog posts about Azure security and compliance.</span></span>
