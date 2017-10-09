---
title: "a Security Center fenyegetés az eszközintelligencia-jelentés aaaAzure |} Microsoft Docs"
description: "Ez a dokumentum segít toouse Azure Security Center fenyegetés intelligens jelentések egy vizsgálat toofind során egy biztonsági riasztás kapcsolatos további információt."
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
ms.openlocfilehash: c888cfac1dd8b057616a6b8e6c6f6b67b552f2e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-threat-intelligence-report"></a><span data-ttu-id="b6b48-103">Azure Security Center fenyegetésfelderítési jelentés</span><span class="sxs-lookup"><span data-stu-id="b6b48-103">Azure Security Center Threat Intelligence Report</span></span>
<span data-ttu-id="b6b48-104">Ez a dokumentum ismerteti, hogy az Azure Security Center fenyegetésfelderítési jelentés segítségével hogyan tudhat meg többet az olyan fenyegetésekről, amelyek biztonsági riasztást hoztak létre.</span><span class="sxs-lookup"><span data-stu-id="b6b48-104">This document explains how Azure Security Center Threat Intelligent Reports can help you learn more about a threat that generated a security alert.</span></span>

## <a name="what-is-a-threat-intelligence-report"></a><span data-ttu-id="b6b48-105">Mi a fenyegetésfelderítési jelentés?</span><span class="sxs-lookup"><span data-stu-id="b6b48-105">What is a threat intelligence report?</span></span>
<span data-ttu-id="b6b48-106">A Security Center fenyegetésészlelés működése figyelése az Azure-erőforrások, hello és hálózati összekapcsolt partnermegoldások biztonsági adatait.</span><span class="sxs-lookup"><span data-stu-id="b6b48-106">Security Center threat detection works by monitoring security information from your Azure resources, hello network, and connected partner solutions.</span></span> <span data-ttu-id="b6b48-107">Ezt az információt, gyakran adatok az adatokat több forrásból tooidentify fenyegetések megvizsgálja.</span><span class="sxs-lookup"><span data-stu-id="b6b48-107">It analyzes this information, often correlating information from multiple sources, tooidentify threats.</span></span> <span data-ttu-id="b6b48-108">Ez a folyamat része a Security Center hello [az észlelési képességek](security-center-detection-capabilities.md).</span><span class="sxs-lookup"><span data-stu-id="b6b48-108">This process is part of hello Security Center [detection capabilities](security-center-detection-capabilities.md).</span></span>

<span data-ttu-id="b6b48-109">Ha a Security Center azonosít egy fenyegetést, akkor az [biztonsági riasztást](security-center-managing-and-responding-alerts.md) vált ki, amely részletes információkat tartalmaz az adott eseménnyel kapcsolatban, beleértve az elhárítási javaslatokat is.</span><span class="sxs-lookup"><span data-stu-id="b6b48-109">When Security Center identifies a threat, it will trigger a [security alert](security-center-managing-and-responding-alerts.md), which contains detailed information regarding a particular event, including suggestions for remediation.</span></span> <span data-ttu-id="b6b48-110">tooassist incidensválasz-csoportok vizsgálja meg, és szervizelheti azokat a fenyegetéseket, a Security Center tartalmaz egy fenyegetés az eszközintelligencia-jelentést hello fenyegetést észlelt, például a párbeszédeket kapcsolatos információkat tartalmazó a:</span><span class="sxs-lookup"><span data-stu-id="b6b48-110">tooassist incident response teams investigate and remediate threats, Security Center includes a threat intelligence report that contains information about hello threat that was detected, including information such as the:</span></span>

* <span data-ttu-id="b6b48-111">A támadó identitása vagy társításai (ha ez az információ elérhető)</span><span class="sxs-lookup"><span data-stu-id="b6b48-111">Attacker’s identity or associations (if this information is available)</span></span>
* <span data-ttu-id="b6b48-112">A támadó céljai</span><span class="sxs-lookup"><span data-stu-id="b6b48-112">Attackers’ objectives</span></span>
* <span data-ttu-id="b6b48-113">Jelenlegi és korábbi támadássorozatok (ha ez az információ elérhető)</span><span class="sxs-lookup"><span data-stu-id="b6b48-113">Current and historical attack campaigns (if this information is available)</span></span>
* <span data-ttu-id="b6b48-114">A támadó taktikája, eszközei és eljárásai</span><span class="sxs-lookup"><span data-stu-id="b6b48-114">Attackers’ tactics, tools and procedures</span></span>
* <span data-ttu-id="b6b48-115">A sérüléssel kapcsolatos mutatók (IoC), például URL-címek és fájlkivonatok</span><span class="sxs-lookup"><span data-stu-id="b6b48-115">Associated indicators of compromise (IoC) such as URLs and file hashes</span></span>
* <span data-ttu-id="b6b48-116">Victimology, amely hello iparág és földrajzi elterjedtségének tooassist meghatározhatja, hogy az Azure-erőforrások fenyegeti a</span><span class="sxs-lookup"><span data-stu-id="b6b48-116">Victimology, which is hello industry and geographic prevalence tooassist you in determining if your Azure resources are at risk</span></span>
* <span data-ttu-id="b6b48-117">Kezelési és elhárítási információk</span><span class="sxs-lookup"><span data-stu-id="b6b48-117">Mitigation and remediation information</span></span>

> [!NOTE]
> <span data-ttu-id="b6b48-118">bármely adott jelentésben szereplő információk mennyisége hello változó; hello részletességi hello kártevő tevékenységet és az elterjedtségének alapul.</span><span class="sxs-lookup"><span data-stu-id="b6b48-118">hello amount of information in any particular report will vary; hello level of detail is based on hello malware’s activity and prevalence.</span></span>
>
>

<span data-ttu-id="b6b48-119">A Security Center háromféle fenyegetés jelentéseit, amelyek függően toohello támadás eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="b6b48-119">Security Center has three types of threat reports, which can vary according toohello attack.</span></span> <span data-ttu-id="b6b48-120">hello elérhető jelentéseket a következők:</span><span class="sxs-lookup"><span data-stu-id="b6b48-120">hello reports available are:</span></span>

* <span data-ttu-id="b6b48-121">**Tevékenységi csoport jelentés**: mélyreható információkat biztosít a támadókról, a céljaikról és a taktikáikról.</span><span class="sxs-lookup"><span data-stu-id="b6b48-121">**Activity Group Report**: provides deep dives into attackers, their objectives and tactics.</span></span>
* <span data-ttu-id="b6b48-122">**Kampányjelentés**: adott támadási kampányok részleteire összpontosít.</span><span class="sxs-lookup"><span data-stu-id="b6b48-122">**Campaign Report**: focuses on details of specific attack campaigns.</span></span>
* <span data-ttu-id="b6b48-123">**A fenyegetés összegző jelentés**: az előző két jelentés hello hello elemeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b6b48-123">**Threat Summary Report**: covers all of hello items in hello previous two reports.</span></span>

<span data-ttu-id="b6b48-124">Az ilyen információkat nagyon hasznos során hello [incidensválasz](security-center-incident-response.md) folyamatok, ahol van egy folyamatban lévő vizsgálat toounderstand hello forrás hello támadás, hello támadó célokat, és mi toodo toomitigate Ez a probléma soron.</span><span class="sxs-lookup"><span data-stu-id="b6b48-124">This type of information is very useful during hello [incident response](security-center-incident-response.md) process, where there is an ongoing investigation toounderstand hello source of hello attack, hello attacker’s motivations, and what toodo toomitigate this issue moving forward.</span></span>

## <a name="how-tooaccess-hello-threat-intelligence-report"></a><span data-ttu-id="b6b48-125">Hogyan tooaccess hello fenyegetés az eszközintelligencia-jelentés?</span><span class="sxs-lookup"><span data-stu-id="b6b48-125">How tooaccess hello threat intelligence report?</span></span>
<span data-ttu-id="b6b48-126">Hello bármikor áttekintheti az aktuális riasztásokat **biztonsági riasztások** csempére.</span><span class="sxs-lookup"><span data-stu-id="b6b48-126">You can review your current alerts by looking at hello **Security alerts** tile.</span></span> <span data-ttu-id="b6b48-127">Nyissa meg a hello Azure portálon, és lépések hello toosee alatt az egyes riasztásokkal kapcsolatos további részletek:</span><span class="sxs-lookup"><span data-stu-id="b6b48-127">Open hello Azure Portal and follow hello steps below toosee more details about each alert:</span></span>

1. <span data-ttu-id="b6b48-128">A Security Center irányítópultjának hello, látni fogja a hello **biztonsági riasztások** csempére.</span><span class="sxs-lookup"><span data-stu-id="b6b48-128">On hello Security Center dashboard, you will see hello **Security alerts** tile.</span></span>
2. <span data-ttu-id="b6b48-129">Kattintson a hello csempe tooopen hello **biztonsági riasztások** panel, amelyen hello riasztások további információkat tartalmaz, és kattintson az lehetőségre hello biztonsági riasztást, amelyet az tooobtain további információt kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b6b48-129">Click hello tile tooopen hello **Security alerts** blade that contains more details about hello alerts and click in hello security alert that you want tooobtain more information about.</span></span>

    ![Biztonsági riasztások](./media/security-center-threat-report/security-center-threat-report-fig1.png)
3. <span data-ttu-id="b6b48-131">Ebben az esetben hello **gyanús folyamat végrehajtása** panel hello riasztást, ahogy az az alábbi ábra hello hello részleteit jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="b6b48-131">In this case hello **Suspicious process executed** blade shows hello details about hello alert as shown in hello figure below:</span></span>

    ![Biztonsági riasztás részletei](./media/security-center-threat-report/security-center-threat-report-fig2.png)
4. <span data-ttu-id="b6b48-133">információk érhetők el az egyes biztonsági riasztások hello mennyiségű riasztás toohello típusától függően változnak.</span><span class="sxs-lookup"><span data-stu-id="b6b48-133">hello amount of information available for each security alert will vary according toohello type of alert.</span></span> <span data-ttu-id="b6b48-134">A hello **jelentések** mező a hivatkozás toohello fenyegetés eszközintelligencia jelentést.</span><span class="sxs-lookup"><span data-stu-id="b6b48-134">In hello **REPORTS** field you have a link toohello threat intelligence report.</span></span> <span data-ttu-id="b6b48-135">Kattintson a hivatkozásra, és egy PDF-fájl jelenik meg egy új böngészőablakban.</span><span class="sxs-lookup"><span data-stu-id="b6b48-135">Click on it and another browser window will appear with PDF file.</span></span>

   ![Tároló kiválasztása](./media/security-center-threat-report/security-center-threat-report-fig3.png)

<span data-ttu-id="b6b48-137">Itt a rendszer észlelte a jelentésben és tájékozódhat a hello biztonsági probléma, amely olvasási PDF hello letöltése, és a cselekvéshez hello foglalt információk alapján.</span><span class="sxs-lookup"><span data-stu-id="b6b48-137">From here you can download hello PDF for this report and read more about hello security issue that was detected and take actions based on hello information provided.</span></span>

## <a name="see-also"></a><span data-ttu-id="b6b48-138">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b6b48-138">See also</span></span>
<span data-ttu-id="b6b48-139">Ebben a dokumentumban megismerkedhetett azzal, hogyan segítheti Önt az Azure Security Center fenyegetésfelderítési jelentés a biztonsági riasztások vizsgálata során.</span><span class="sxs-lookup"><span data-stu-id="b6b48-139">In this document, you learned how Azure Security Center Threat Intelligent Reports can help during an investigation about security alerts.</span></span> <span data-ttu-id="b6b48-140">További információ az Azure Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="b6b48-140">toolearn more about Azure Security Center, see hello following:</span></span>

* <span data-ttu-id="b6b48-141">[Azure Security Center – gyakori kérdések](security-center-faq.md)</span><span class="sxs-lookup"><span data-stu-id="b6b48-141">[Azure Security Center FAQ](security-center-faq.md).</span></span> <span data-ttu-id="b6b48-142">Hello szolgáltatással kapcsolatos gyakran ismételt kérdések található.</span><span class="sxs-lookup"><span data-stu-id="b6b48-142">Find frequently asked questions about using hello service.</span></span>
* [<span data-ttu-id="b6b48-143">Az Azure Security Center használata incidensmegoldásra</span><span class="sxs-lookup"><span data-stu-id="b6b48-143">Leveraging Azure Security Center for Incident Response</span></span>](security-center-incident-response.md)
* [<span data-ttu-id="b6b48-144">Az Azure Security Center észlelési képességei</span><span class="sxs-lookup"><span data-stu-id="b6b48-144">Azure Security Center detection capabilities</span></span>](security-center-detection-capabilities.md)
* <span data-ttu-id="b6b48-145">[Útmutató az Azure Security Center tervezéséhez és működtetéséhez](security-center-planning-and-operations-guide.md).</span><span class="sxs-lookup"><span data-stu-id="b6b48-145">[Azure Security Center planning and operations guide](security-center-planning-and-operations-guide.md).</span></span> <span data-ttu-id="b6b48-146">Megtudhatja, hogyan tooplan és hello kialakítási szempontok tooadopt az Azure Security Center ismertetése.</span><span class="sxs-lookup"><span data-stu-id="b6b48-146">Learn how tooplan and understand hello design considerations tooadopt Azure Security Center.</span></span>
* <span data-ttu-id="b6b48-147">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="b6b48-147">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md).</span></span> <span data-ttu-id="b6b48-148">Megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="b6b48-148">Learn how toomanage and respond toosecurity alerts.</span></span>
* [<span data-ttu-id="b6b48-149">Biztonsági incidensek kezelése az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="b6b48-149">Handling Security Incident in Azure Security Center</span></span>](security-center-incident.md)
* <span data-ttu-id="b6b48-150">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)</span><span class="sxs-lookup"><span data-stu-id="b6b48-150">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/).</span></span> <span data-ttu-id="b6b48-151">Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="b6b48-151">Find blog posts about Azure security and compliance.</span></span>
