---
title: "Azure Security Center fenyegetésfelderítési jelentés | Microsoft Docs"
description: "Ebből a dokumentumból megtudhatja hogyan használhatja az Azure Security Center fenyegetésfelderítési jelentést a vizsgálat során, amikor a biztonsági riasztásokkal kapcsolatos információkat keres."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 5662e312-e8c2-4736-974e-576eeb333484
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: b4310cf4e6849c67031b3ec8b1fd5957e35f7ea6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-security-center-threat-intelligence-report"></a><span data-ttu-id="af8d2-103">Azure Security Center fenyegetésfelderítési jelentés</span><span class="sxs-lookup"><span data-stu-id="af8d2-103">Azure Security Center Threat Intelligence Report</span></span>
<span data-ttu-id="af8d2-104">Ez a dokumentum ismerteti, hogy az Azure Security Center fenyegetésfelderítési jelentés segítségével hogyan tudhat meg többet az olyan fenyegetésekről, amelyek biztonsági riasztást hoztak létre.</span><span class="sxs-lookup"><span data-stu-id="af8d2-104">This document explains how Azure Security Center Threat Intelligent Reports can help you learn more about a threat that generated a security alert.</span></span>

## <a name="what-is-a-threat-intelligence-report"></a><span data-ttu-id="af8d2-105">Mi a fenyegetésfelderítési jelentés?</span><span class="sxs-lookup"><span data-stu-id="af8d2-105">What is a threat intelligence report?</span></span>
<span data-ttu-id="af8d2-106">A Security Center fenyegetésészlelése úgy működik, hogy figyeli az Azure-erőforrások, a hálózat, és a partnermegoldások biztonsági információit.</span><span class="sxs-lookup"><span data-stu-id="af8d2-106">Security Center threat detection works by monitoring security information from your Azure resources, the network, and connected partner solutions.</span></span> <span data-ttu-id="af8d2-107">A fenyegetések azonosításához elemzi ezeket az információkat, és gyakran megvizsgálja a különböző forrásokból származó adatok közötti összefüggéseket.</span><span class="sxs-lookup"><span data-stu-id="af8d2-107">It analyzes this information, often correlating information from multiple sources, to identify threats.</span></span> <span data-ttu-id="af8d2-108">Ez a folyamat része a Security Center [észlelési képességeinek](security-center-detection-capabilities.md).</span><span class="sxs-lookup"><span data-stu-id="af8d2-108">This process is part of the Security Center [detection capabilities](security-center-detection-capabilities.md).</span></span>

<span data-ttu-id="af8d2-109">Ha a Security Center azonosít egy fenyegetést, akkor az [biztonsági riasztást](security-center-managing-and-responding-alerts.md) vált ki, amely részletes információkat tartalmaz az adott eseménnyel kapcsolatban, beleértve az elhárítási javaslatokat is.</span><span class="sxs-lookup"><span data-stu-id="af8d2-109">When Security Center identifies a threat, it will trigger a [security alert](security-center-managing-and-responding-alerts.md), which contains detailed information regarding a particular event, including suggestions for remediation.</span></span> <span data-ttu-id="af8d2-110">A Security Center úgy segíti az incidensmegoldó csapatokat a vizsgálatban és az elhárításban, hogy tartalmaz egy fenyegetésfelderítési jelentést, amely információkat ad az észlelt fenyegetésről, beleértve ebbe a következőket:</span><span class="sxs-lookup"><span data-stu-id="af8d2-110">To assist incident response teams investigate and remediate threats, Security Center includes a threat intelligence report that contains information about the threat that was detected, including information such as the:</span></span>

* <span data-ttu-id="af8d2-111">A támadó identitása vagy társításai (ha ez az információ elérhető)</span><span class="sxs-lookup"><span data-stu-id="af8d2-111">Attacker’s identity or associations (if this information is available)</span></span>
* <span data-ttu-id="af8d2-112">A támadó céljai</span><span class="sxs-lookup"><span data-stu-id="af8d2-112">Attackers’ objectives</span></span>
* <span data-ttu-id="af8d2-113">Jelenlegi és korábbi támadássorozatok (ha ez az információ elérhető)</span><span class="sxs-lookup"><span data-stu-id="af8d2-113">Current and historical attack campaigns (if this information is available)</span></span>
* <span data-ttu-id="af8d2-114">A támadó taktikája, eszközei és eljárásai</span><span class="sxs-lookup"><span data-stu-id="af8d2-114">Attackers’ tactics, tools and procedures</span></span>
* <span data-ttu-id="af8d2-115">A sérüléssel kapcsolatos mutatók (IoC), például URL-címek és fájlkivonatok</span><span class="sxs-lookup"><span data-stu-id="af8d2-115">Associated indicators of compromise (IoC) such as URLs and file hashes</span></span>
* <span data-ttu-id="af8d2-116">A viktimológia, azaz az ipari és földrajzi gyakoriság segít annak meghatározásában, hogy az Ön Azure-erőforrásai veszélyeztetettek-e</span><span class="sxs-lookup"><span data-stu-id="af8d2-116">Victimology, which is the industry and geographic prevalence to assist you in determining if your Azure resources are at risk</span></span>
* <span data-ttu-id="af8d2-117">Kezelési és elhárítási információk</span><span class="sxs-lookup"><span data-stu-id="af8d2-117">Mitigation and remediation information</span></span>

> [!NOTE]
> <span data-ttu-id="af8d2-118">Az információ mennyisége minden jelentésben más, a részletesség pedig a kártevő tevékenységén és gyakoriságán alapul.</span><span class="sxs-lookup"><span data-stu-id="af8d2-118">The amount of information in any particular report will vary; the level of detail is based on the malware’s activity and prevalence.</span></span>
>
>

<span data-ttu-id="af8d2-119">A Security Centerben három típusa van a fenyegetési jelentéseknek, amelyek a támadástól függően változnak.</span><span class="sxs-lookup"><span data-stu-id="af8d2-119">Security Center has three types of threat reports, which can vary according to the attack.</span></span> <span data-ttu-id="af8d2-120">Az elérhető jelentések a következők:</span><span class="sxs-lookup"><span data-stu-id="af8d2-120">The reports available are:</span></span>

* <span data-ttu-id="af8d2-121">**Tevékenységi csoport jelentés**: mélyreható információkat biztosít a támadókról, a céljaikról és a taktikáikról.</span><span class="sxs-lookup"><span data-stu-id="af8d2-121">**Activity Group Report**: provides deep dives into attackers, their objectives and tactics.</span></span>
* <span data-ttu-id="af8d2-122">**Kampányjelentés**: adott támadási kampányok részleteire összpontosít.</span><span class="sxs-lookup"><span data-stu-id="af8d2-122">**Campaign Report**: focuses on details of specific attack campaigns.</span></span>
* <span data-ttu-id="af8d2-123">**Fenyegetésösszegző jelentés**: az előző két jelentés összes elemét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="af8d2-123">**Threat Summary Report**: covers all of the items in the previous two reports.</span></span>

<span data-ttu-id="af8d2-124">Az ilyen jellegű információk nagyon hasznosak az [incidensmegoldási](security-center-incident-response.md) folyamatban, ahol folyamatos vizsgálat folyik a támadások forrásának és a támadó indítékainak megismeréséért, illetve azért, hogy mit kell tenni a probléma enyhítése és a továbblépés érdekében.</span><span class="sxs-lookup"><span data-stu-id="af8d2-124">This type of information is very useful during the [incident response](security-center-incident-response.md) process, where there is an ongoing investigation to understand the source of the attack, the attacker’s motivations, and what to do to mitigate this issue moving forward.</span></span>

## <a name="how-to-access-the-threat-intelligence-report"></a><span data-ttu-id="af8d2-125">Hogyan értheti el a fenyegetésfelderítési jelentést?</span><span class="sxs-lookup"><span data-stu-id="af8d2-125">How to access the threat intelligence report?</span></span>
<span data-ttu-id="af8d2-126">A **Biztonsági riasztások** csempén áttekintheti az aktuális riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="af8d2-126">You can review your current alerts by looking at the **Security alerts** tile.</span></span> <span data-ttu-id="af8d2-127">Nyissa meg az Azure Portalt, és kövesse az alábbi lépéseket az egyes riasztások részleteinek megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="af8d2-127">Open the Azure Portal and follow the steps below to see more details about each alert:</span></span>

1. <span data-ttu-id="af8d2-128">A Security Center irányítópultján található a **Biztonsági riasztások** csempe.</span><span class="sxs-lookup"><span data-stu-id="af8d2-128">On the Security Center dashboard, you will see the **Security alerts** tile.</span></span>
2. <span data-ttu-id="af8d2-129">Kattintson a csempére, hogy megnyissa a **Biztonsági riasztások** panelt, amely további részleteket tartalmaz a riasztásokról, majd kattintson arra a biztonsági riasztásra, amelyről többet szeretne megtudni.</span><span class="sxs-lookup"><span data-stu-id="af8d2-129">Click the tile to open the **Security alerts** blade that contains more details about the alerts and click in the security alert that you want to obtain more information about.</span></span>

    ![Biztonsági riasztások](./media/security-center-threat-report/security-center-threat-report-fig1.png)
3. <span data-ttu-id="af8d2-131">Ebben az esetben a **Gyanús végrehajtott folyamat** panel a riasztás részleteit mutatja, ahogy azt az alábbi ábrán is látható:</span><span class="sxs-lookup"><span data-stu-id="af8d2-131">In this case the **Suspicious process executed** blade shows the details about the alert as shown in the figure below:</span></span>

    ![Biztonsági riasztás részletei](./media/security-center-threat-report/security-center-threat-report-fig2.png)
4. <span data-ttu-id="af8d2-133">Az egyes biztonsági riasztásokhoz elérthető információ mennyisége a riasztás típusától függően változik.</span><span class="sxs-lookup"><span data-stu-id="af8d2-133">The amount of information available for each security alert will vary according to the type of alert.</span></span> <span data-ttu-id="af8d2-134">A **JELENTÉSEK** mezőben egy hivatkozás található a fenyegetésfelderítési jelentéshez.</span><span class="sxs-lookup"><span data-stu-id="af8d2-134">In the **REPORTS** field you have a link to the threat intelligence report.</span></span> <span data-ttu-id="af8d2-135">Kattintson a hivatkozásra, és egy PDF-fájl jelenik meg egy új böngészőablakban.</span><span class="sxs-lookup"><span data-stu-id="af8d2-135">Click on it and another browser window will appear with PDF file.</span></span>

   ![Tároló kiválasztása](./media/security-center-threat-report/security-center-threat-report-fig3.png)

<span data-ttu-id="af8d2-137">Innen letöltheti a jelentéshez tartozó PDF-fájlt, ahol további információkat olvashat az észlelt biztonsági problémáról, és a megadott információkat alapul véve intézkedhet.</span><span class="sxs-lookup"><span data-stu-id="af8d2-137">From here you can download the PDF for this report and read more about the security issue that was detected and take actions based on the information provided.</span></span>

## <a name="see-also"></a><span data-ttu-id="af8d2-138">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="af8d2-138">See also</span></span>
<span data-ttu-id="af8d2-139">Ebben a dokumentumban megismerkedhetett azzal, hogyan segítheti Önt az Azure Security Center fenyegetésfelderítési jelentés a biztonsági riasztások vizsgálata során.</span><span class="sxs-lookup"><span data-stu-id="af8d2-139">In this document, you learned how Azure Security Center Threat Intelligent Reports can help during an investigation about security alerts.</span></span> <span data-ttu-id="af8d2-140">Az Azure Security Centerrel kapcsolatos további információkért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="af8d2-140">To learn more about Azure Security Center, see the following:</span></span>

* <span data-ttu-id="af8d2-141">[Azure Security Center – gyakori kérdések](security-center-faq.md)</span><span class="sxs-lookup"><span data-stu-id="af8d2-141">[Azure Security Center FAQ](security-center-faq.md).</span></span> <span data-ttu-id="af8d2-142">Gyakori kérdések a szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="af8d2-142">Find frequently asked questions about using the service.</span></span>
* [<span data-ttu-id="af8d2-143">Az Azure Security Center használata incidensmegoldásra</span><span class="sxs-lookup"><span data-stu-id="af8d2-143">Leveraging Azure Security Center for Incident Response</span></span>](security-center-incident-response.md)
* [<span data-ttu-id="af8d2-144">Az Azure Security Center észlelési képességei</span><span class="sxs-lookup"><span data-stu-id="af8d2-144">Azure Security Center detection capabilities</span></span>](security-center-detection-capabilities.md)
* <span data-ttu-id="af8d2-145">[Útmutató az Azure Security Center tervezéséhez és működtetéséhez](security-center-planning-and-operations-guide.md).</span><span class="sxs-lookup"><span data-stu-id="af8d2-145">[Azure Security Center planning and operations guide](security-center-planning-and-operations-guide.md).</span></span> <span data-ttu-id="af8d2-146">A tervezési szempontokat ismertető és az azokat figyelembe vevő tervezési folyamatokban segítő útmutató, amely megkönnyíti az Azure Security Center használatát.</span><span class="sxs-lookup"><span data-stu-id="af8d2-146">Learn how to plan and understand the design considerations to adopt Azure Security Center.</span></span>
* <span data-ttu-id="af8d2-147">[Biztonsági riasztások kezelése és válaszadás a riasztásokra az Azure Security Centerben](security-center-managing-and-responding-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="af8d2-147">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md).</span></span> <span data-ttu-id="af8d2-148">A biztonsági riasztások kezelésének és a riasztásokra való válaszadás módját ismertető útmutató.</span><span class="sxs-lookup"><span data-stu-id="af8d2-148">Learn how to manage and respond to security alerts.</span></span>
* [<span data-ttu-id="af8d2-149">Biztonsági incidens kezelése az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="af8d2-149">Handling Security Incident in Azure Security Center</span></span>](security-center-incident.md)
* <span data-ttu-id="af8d2-150">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)</span><span class="sxs-lookup"><span data-stu-id="af8d2-150">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/).</span></span> <span data-ttu-id="af8d2-151">Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="af8d2-151">Find blog posts about Azure security and compliance.</span></span>
