---
title: "az Azure Security Center biztonsági riasztások aaaHandling |} Microsoft Docs"
description: "Ez a dokumentum segít toouse az Azure Security Center képességek toohandle biztonsági eseményekre."
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
ms.openlocfilehash: edb911c298a2ce93cd0ea5b22ce002005040090f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="handling-security-incidents-in-azure-security-center"></a><span data-ttu-id="bea90-103">Biztonsági incidensek kezelése az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="bea90-103">Handling Security Incidents in Azure Security Center</span></span>
<span data-ttu-id="bea90-104">Triaging és biztonsági riasztások kivizsgálásának sok időt vesz igénybe a még akkor is, hello legtöbb gyakorlott biztonsági elemzőknek. lehet, és számos rögzített tooeven tudja, honnan toobegin.</span><span class="sxs-lookup"><span data-stu-id="bea90-104">Triaging and investigating security alerts can be time consuming for even hello most skilled security analysts, and for many it is hard tooeven know where toobegin.</span></span> <span data-ttu-id="bea90-105">A [analytics](security-center-detection-capabilities.md) tooconnect hello információk között a különböző [biztonsági riasztások](security-center-managing-and-responding-alerts.md), a Security Center adja meg a egy támadás kampány egyetlen nézetben, és az összes hello kapcsolódó riasztások – is gyorsan ismerje meg, milyen műveleteket hello támadó tartott, és milyen erőforrásokat hatással volt.</span><span class="sxs-lookup"><span data-stu-id="bea90-105">By using [analytics](security-center-detection-capabilities.md) tooconnect hello information between distinct [security alerts](security-center-managing-and-responding-alerts.md), Security Center can provide you with a single view of an attack campaign and all of hello related alerts – you can quickly understand what actions hello attacker took and what resources were impacted.</span></span>

<span data-ttu-id="bea90-106">Ez a dokumentum azt ismerteti, hogyan toouse biztonsági riasztásokat a Security Center tooassist funkció biztonsági incidensek kezelése.</span><span class="sxs-lookup"><span data-stu-id="bea90-106">This document discusses how toouse security alert capability in Security Center tooassist you handling security incidents.</span></span>

## <a name="what-is-a-security-incident"></a><span data-ttu-id="bea90-107">Mi az a biztonsági incidens?</span><span class="sxs-lookup"><span data-stu-id="bea90-107">What is a security incident?</span></span>
<span data-ttu-id="bea90-108">A Security Centerben egy biztonsági incidens az adott erőforráshoz tartozó összes olyan riasztás együttese, amelyek egy [támadási folyamatba](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) illeszthetők.</span><span class="sxs-lookup"><span data-stu-id="bea90-108">In Security Center, a security incident is an aggregation of all alerts for a resource that align with [kill chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) patterns.</span></span> <span data-ttu-id="bea90-109">Incidensek megjelennek a hello [biztonsági riasztások](security-center-managing-and-responding-alerts.md) csempe és panelen.</span><span class="sxs-lookup"><span data-stu-id="bea90-109">Incidents appear in hello [Security Alerts](security-center-managing-and-responding-alerts.md) tile and blade.</span></span> <span data-ttu-id="bea90-110">Incidens jelenítenek meg hello kapcsolódó riasztások listája, amely lehetővé teszi, hogy tooobtain további információ a minden egyes előfordulásakor.</span><span class="sxs-lookup"><span data-stu-id="bea90-110">An Incident will reveal hello list of related alerts, which enables you tooobtain more information about each occurrence.</span></span>

## <a name="managing-security-incidents"></a><span data-ttu-id="bea90-111">Biztonsági incidensek kezelése</span><span class="sxs-lookup"><span data-stu-id="bea90-111">Managing security incidents</span></span>
<span data-ttu-id="bea90-112">Az aktuális biztonsági események megtekintésével hello biztonsági riasztások csempén tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="bea90-112">You can review your current security incidents by looking at hello security alerts tile.</span></span> <span data-ttu-id="bea90-113">Hello Azure portál eléréséhez, és kövesse az alábbi toosee hello lépéseket minden biztonsági esemény kapcsolatos további részletekért:</span><span class="sxs-lookup"><span data-stu-id="bea90-113">Access hello Azure Portal and follow hello steps below toosee more details about each security incident:</span></span>

1. <span data-ttu-id="bea90-114">A Security Center irányítópultjának hello, látni fogja a hello **biztonsági riasztások** csempére.</span><span class="sxs-lookup"><span data-stu-id="bea90-114">On hello Security Center dashboard, you will see hello **Security alerts** tile.</span></span>

    ![Biztonsági riasztások csempe a Security Centerben](./media/security-center-incident/security-center-incident-fig1.png)

2. <span data-ttu-id="bea90-116">Kattintson a a csempe tooexpand, és ha egy biztonsági incidens észlel, akkor jelenik meg a hello biztonsági riasztások graph alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="bea90-116">Click on this tile tooexpand it and if a security incident is detected, it will appear under hello security alerts graph as shown  below:</span></span>

    ![Biztonsági incidens](./media/security-center-incident/security-center-incident-fig2.png)

3. <span data-ttu-id="bea90-118">Figyelje meg, hogy rendelkezik-e más ikon hello biztonsági esemény leírása tooother riasztások képest.</span><span class="sxs-lookup"><span data-stu-id="bea90-118">Notice that hello security incident description has a different icon compared tooother alerts.</span></span> <span data-ttu-id="bea90-119">Kattintson rá tooview több ezt az incidenst vonatkozó részletek.</span><span class="sxs-lookup"><span data-stu-id="bea90-119">Click on it tooview more details about this incident.</span></span>

    ![Biztonsági incidens](./media/security-center-incident/security-center-incident-fig3.png)

4. <span data-ttu-id="bea90-121">A hello **incidens** panel jelenik meg több részleteivel kapcsolatban a biztonsági incidens, amely tartalmazza a teljes leírását, a súlyosság (amely ebben az esetben magas), a jelenlegi állapotában (ebben az esetben van még *aktív*, amiből hello felhasználó még nem vett egy művelet tooit – hello incidenst hello a jobb gombbal kattintva ehhez **biztonsági riasztások** panel), hello megtámadott erőforrások (ebben az esetben *VM1*), hello hello incidens javítási lépéseket, és hello alsó ablaktáblában hello riasztások ezt az incidenst foglalt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bea90-121">On hello **incident** blade you will see more details about this security incident, which includes its full description, its severity (which in this case is high), its current state (in this case it is still *active*, which implies hello user hasn't taken an action tooit - this can be done by right clicking on hello incident in hello **Security alerts** blade), hello attacked resource (in this case *VM1*), hello remediation steps for hello incident, and in hello bottom pane you have hello alerts that were included in this incident.</span></span> <span data-ttu-id="bea90-122">Tooobtain további információt az egyes riasztások, kattintson a, és egy újabb panel fog nyissa meg, alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="bea90-122">If you want tooobtain more information on each alert, just click on it and another blade will open, as shown below:</span></span>

    ![Biztonsági incidens](./media/security-center-incident/security-center-incident-fig4.png)

<span data-ttu-id="bea90-124">hello információkat a panel konfigurációtól függően toohello riasztás.</span><span class="sxs-lookup"><span data-stu-id="bea90-124">hello information on this blade will vary according toohello alert.</span></span> <span data-ttu-id="bea90-125">Olvasási [az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) további információt a toomanage ezeket a riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="bea90-125">Read [Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) for more information on how toomanage these alerts.</span></span> <span data-ttu-id="bea90-126">Ezzel a képességgel kapcsolatos fontos szempontok:</span><span class="sxs-lookup"><span data-stu-id="bea90-126">Some important considerations regarding this capability:</span></span>

* <span data-ttu-id="bea90-127">Új szűrő lehetővé teszi a nézet tooIncident csak, riasztásokat csak toocustomize, vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="bea90-127">A new filter enables you toocustomize your view tooIncident only, Alerts only, or both.</span></span>
* <span data-ttu-id="bea90-128">hello ugyanabból a riasztásból létezhet egy incidens (ha van ilyen), valamint a toobe látható, mint egy különálló riasztás részeként.</span><span class="sxs-lookup"><span data-stu-id="bea90-128">hello same alert can exist as part of an Incident (if applicable), as well as toobe visible as a standalone alert.</span></span>

## <a name="see-also"></a><span data-ttu-id="bea90-129">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="bea90-129">See also</span></span>
<span data-ttu-id="bea90-130">Ebből a dokumentumból megtanulta, hogyan toouse hello Security Center biztonsági incidens képességét.</span><span class="sxs-lookup"><span data-stu-id="bea90-130">In this document, you learned how toouse hello security incident capability in Security Center.</span></span> <span data-ttu-id="bea90-131">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="bea90-131">toolearn more about Security Center, see hello following:</span></span>

* [<span data-ttu-id="bea90-132">Kezelése és reagálás toosecurity értesítések az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="bea90-132">Managing and responding toosecurity alerts in Azure Security Center</span></span>](security-center-managing-and-responding-alerts.md)
* [<span data-ttu-id="bea90-133">Az Azure Security Center észlelési funkciói</span><span class="sxs-lookup"><span data-stu-id="bea90-133">Azure Security Center Detection Capabilities</span></span>](security-center-detection-capabilities.md)
* [<span data-ttu-id="bea90-134">Útmutató az Azure Security Center tervezéséhez és működtetéséhez</span><span class="sxs-lookup"><span data-stu-id="bea90-134">Azure Security Center Planning and Operations Guide</span></span>](security-center-planning-and-operations-guide.md)
* [<span data-ttu-id="bea90-135">Kezelése és reagálás toosecurity értesítések az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="bea90-135">Managing and responding toosecurity alerts in Azure Security Center</span></span>](security-center-managing-and-responding-alerts.md)
* <span data-ttu-id="bea90-136">[Azure Security Center: GYIK](security-center-faq.md)– gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="bea90-136">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="bea90-137">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="bea90-137">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Find blog posts about Azure security and compliance.</span></span>
