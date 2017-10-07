---
title: "az Azure Security Center biztonsági riasztások aaaManage |} Microsoft Docs"
description: "Ez a dokumentum segít, toouse az Azure Security Center képességek toomanage, és toosecurity riasztások válaszol."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: yurid
ms.openlocfilehash: f1cb7e4770776827b75ed15893914678c1f44216
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-and-responding-toosecurity-alerts-in-azure-security-center"></a><span data-ttu-id="e75c5-103">Kezelése és reagálás toosecurity értesítések az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="e75c5-103">Managing and responding toosecurity alerts in Azure Security Center</span></span>
<span data-ttu-id="e75c5-104">Ez a dokumentum segítséget nyújt a használja az Azure Security Center toomanage és toosecurity riasztások válaszol.</span><span class="sxs-lookup"><span data-stu-id="e75c5-104">This document helps you use Azure Security Center toomanage and respond toosecurity alerts.</span></span>

> [!NOTE]
> <span data-ttu-id="e75c5-105">Speciális tooenable észlelések, a Security Center szabványos frissítési tooAzure.</span><span class="sxs-lookup"><span data-stu-id="e75c5-105">tooenable advanced detections, upgrade tooAzure Security Center Standard.</span></span> <span data-ttu-id="e75c5-106">A 60 napos próbaverzió ingyenes.</span><span class="sxs-lookup"><span data-stu-id="e75c5-106">A free 60-day trial is available.</span></span> <span data-ttu-id="e75c5-107">tooupgrade, hello válassza árképzési szintjében [biztonsági házirend](security-center-policies.md).</span><span class="sxs-lookup"><span data-stu-id="e75c5-107">tooupgrade, select Pricing Tier in hello [Security Policy](security-center-policies.md).</span></span> <span data-ttu-id="e75c5-108">Lásd: [az Azure Security Center árképzési](security-center-pricing.md) további toolearn.</span><span class="sxs-lookup"><span data-stu-id="e75c5-108">See [Azure Security Center pricing](security-center-pricing.md) toolearn more.</span></span>
>
>

## <a name="what-are-security-alerts"></a><span data-ttu-id="e75c5-109">Mik azok a biztonsági riasztások?</span><span class="sxs-lookup"><span data-stu-id="e75c5-109">What are security alerts?</span></span>
<span data-ttu-id="e75c5-110">A Security Center automatikusan gyűjti, elemzi, és integrálja az Azure-erőforrások, hello hálózati naplóadatait és partneri megoldások, például tűzfal és az endpoint protection megoldások, toodetect valós fenyegetések csatlakoztatva és vakriasztások számának csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="e75c5-110">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, hello network, and connected partner solutions, like firewall and endpoint protection solutions, toodetect real threats and reduce false positives.</span></span> <span data-ttu-id="e75c5-111">A rangsorolt biztonsági riasztások listája látható a Security Center együtt hello tooquickly szükséges információk vizsgálata hello probléma és módjára vonatkozó javaslatokkal tooremediate támadás.</span><span class="sxs-lookup"><span data-stu-id="e75c5-111">A list of prioritized security alerts is shown in Security Center along with hello information you need tooquickly investigate hello problem and recommendations for how tooremediate an attack.</span></span>


> [!NOTE]
> <span data-ttu-id="e75c5-112">Ha részletes tájékoztatást szeretne kapni a Security Center észlelési funkcióinak működéséről, olvassa el [Az Azure Security Center észlelési funkciói](security-center-detection-capabilities.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="e75c5-112">For more information about how Security Center detection capabilities work, read [Azure Security Center Detection Capabilities](security-center-detection-capabilities.md).</span></span>
>
>

## <a name="managing-security-alerts"></a><span data-ttu-id="e75c5-113">Biztonsági riasztások kezelése</span><span class="sxs-lookup"><span data-stu-id="e75c5-113">Managing security alerts</span></span>
<span data-ttu-id="e75c5-114">Hello bármikor áttekintheti az aktuális riasztásokat **biztonsági riasztások** csempére.</span><span class="sxs-lookup"><span data-stu-id="e75c5-114">You can review your current alerts by looking at hello **Security alerts** tile.</span></span> <span data-ttu-id="e75c5-115">Azure portál megnyitásához és a lépések hello toosee alatt az egyes riasztásokkal kapcsolatos további részletek:</span><span class="sxs-lookup"><span data-stu-id="e75c5-115">Open Azure Portal and follow hello steps below toosee more details about each alert:</span></span>

1. <span data-ttu-id="e75c5-116">A Security Center irányítópultjának hello, látni fogja a hello **biztonsági riasztások** csempére.</span><span class="sxs-lookup"><span data-stu-id="e75c5-116">On hello Security Center dashboard, you will see hello **Security alerts** tile.</span></span>

    ![Biztonsági riasztások csempe a Security Centerben](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2. <span data-ttu-id="e75c5-118">Kattintson a hello csempe tooopen hello **biztonsági riasztások** hello további információkat tartalmazó panel figyelmezteti a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="e75c5-118">Click hello tile tooopen hello **Security alerts** blade that contains more details about hello alerts as shown below.</span></span>

   ![hello biztonsági riasztások panel az Security Centerben](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

<span data-ttu-id="e75c5-120">A panel alsó részén hello vannak az egyes riasztások részletei hello.</span><span class="sxs-lookup"><span data-stu-id="e75c5-120">In hello bottom part of this blade are hello details for each alert.</span></span> <span data-ttu-id="e75c5-121">toosort, kattintson a hello oszlop toosort által használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="e75c5-121">toosort, click hello column that you want toosort by.</span></span> <span data-ttu-id="e75c5-122">az alábbi táblázat a hello minden oszlop definíciója:</span><span class="sxs-lookup"><span data-stu-id="e75c5-122">hello definition for each column is given below:</span></span>

* <span data-ttu-id="e75c5-123">**Leírás**: hello riasztás rövid leírása.</span><span class="sxs-lookup"><span data-stu-id="e75c5-123">**Description**: A brief explanation of hello alert.</span></span>
* <span data-ttu-id="e75c5-124">**Szám:** az adott típusú riasztások listája egy adott napra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="e75c5-124">**Count**: A list of all alerts of this specific type that were detected on a specific day.</span></span>
* <span data-ttu-id="e75c5-125">**Által észlelt**: hello hello riasztás kiváltásáért felelős szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e75c5-125">**Detected by**: hello service that was responsible for triggering hello alert.</span></span>
* <span data-ttu-id="e75c5-126">**Dátum**: hello dátum, hogy hello esemény történt.</span><span class="sxs-lookup"><span data-stu-id="e75c5-126">**Date**: hello date that hello event occurred.</span></span>
* <span data-ttu-id="e75c5-127">**Állapot**: hello riasztás aktuális állapota.</span><span class="sxs-lookup"><span data-stu-id="e75c5-127">**State**: hello current state for that alert.</span></span> <span data-ttu-id="e75c5-128">Kétféle állapot létezik:</span><span class="sxs-lookup"><span data-stu-id="e75c5-128">There are two types of states:</span></span>
  * <span data-ttu-id="e75c5-129">**Aktív**: hello biztonsági riasztást észlelték.</span><span class="sxs-lookup"><span data-stu-id="e75c5-129">**Active**: hello security alert has been detected.</span></span>
* <span data-ttu-id="e75c5-130">**Súlyossági**: hello súlyossági szintet, amely magas, közepes vagy alacsony lehet.</span><span class="sxs-lookup"><span data-stu-id="e75c5-130">**Severity**: hello severity level, which can be high, medium or low.</span></span>

### <a name="filtering-alerts"></a><span data-ttu-id="e75c5-131">A riasztások szűrése</span><span class="sxs-lookup"><span data-stu-id="e75c5-131">Filtering alerts</span></span>
<span data-ttu-id="e75c5-132">A riasztások dátum, állapot és súlyosság alapján szűrhetők.</span><span class="sxs-lookup"><span data-stu-id="e75c5-132">You can filter alerts based on date, state, and severity.</span></span> <span data-ttu-id="e75c5-133">A riasztások szűrését forgatókönyvek esetén, ha a biztonsági riasztások megjelenítése toonarrow hello hatókörében kell hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="e75c5-133">Filtering alerts can be useful for scenarios where you need toonarrow hello scope of security alerts show.</span></span> <span data-ttu-id="e75c5-134">Például akkor lehet, hogy azt szeretné, hogy tooaddress történt biztonsági riasztásokat hello az elmúlt 24 órában, mert egy történő lehetséges behatolást vizsgál hello rendszerben.</span><span class="sxs-lookup"><span data-stu-id="e75c5-134">For example, you might you want tooaddress security alerts that occurred in hello last 24 hours because you are investigating a potential breach in hello system.</span></span>

1. <span data-ttu-id="e75c5-135">Kattintson a **szűrő** a hello **biztonsági riasztások** panelen.</span><span class="sxs-lookup"><span data-stu-id="e75c5-135">Click **Filter** on hello **Security Alerts** blade.</span></span> <span data-ttu-id="e75c5-136">Hello **szűrő** panel megnyitása és toosee kívánja hello dátum, állapot és súlyosság értékek választja.</span><span class="sxs-lookup"><span data-stu-id="e75c5-136">hello **Filter** blade opens and you select hello date, state, and severity values you wish toosee.</span></span>

    ![A riasztások szűrése a Security Centerben](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-2017.png)

### <a name="respond-toosecurity-alerts"></a><span data-ttu-id="e75c5-138">Válaszoljon toosecurity riasztások</span><span class="sxs-lookup"><span data-stu-id="e75c5-138">Respond toosecurity alerts</span></span>
<span data-ttu-id="e75c5-139">Válassza ki a biztonsági riasztás toolearn hello esemény, ha vannak ilyenek, lépéseket és mit, melyik hello riasztás kiváltó többet kell tootake tooremediate támadás.</span><span class="sxs-lookup"><span data-stu-id="e75c5-139">Select a security alert toolearn more about hello event(s) that triggered hello alert and what, if any, steps you need tootake tooremediate an attack.</span></span> <span data-ttu-id="e75c5-140">A biztonsági riasztások típus és dátum szerint vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="e75c5-140">Security alerts are grouped by type and date.</span></span> <span data-ttu-id="e75c5-141">Egy biztonsági riasztás kattintva megnyílik egy hello csoportosított riasztások listáját tartalmazó panel.</span><span class="sxs-lookup"><span data-stu-id="e75c5-141">Clicking a security alert will open a blade containing a list of hello grouped alerts.</span></span>

![Válaszoljon az Azure Security Centerben toosecurity riasztások](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

<span data-ttu-id="e75c5-143">Ebben az esetben hello riasztások váltotta tekintse meg a toosuspicious Remote Desktop Protocol (RDP) tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="e75c5-143">In this case, hello alerts that were triggered refer toosuspicious Remote Desktop Protocol (RDP) activity.</span></span> <span data-ttu-id="e75c5-144">hello az első oszlopban látható kitett erőforrások; hello második bemutatja, hogy hányszor hello erőforrás támadásnak kitett erőforrásra; hello harmadik hello időt hello támadások; jeleníti meg hello negyedik hello riasztás; állapotának megjelenítése és hello ötödik hello támadás hello súlyosságát mutatja.</span><span class="sxs-lookup"><span data-stu-id="e75c5-144">hello first column shows which resources were attacked; hello second shows how many times hello resource was attacked; hello third shows hello time of hello attack; hello fourth shows state of hello alert; and hello fifth shows hello severity of hello attack.</span></span> <span data-ttu-id="e75c5-145">Ezek az információk áttekintése után kattintson a támadásnak kitett erőforrásra hello erőforrás és egy új panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="e75c5-145">After reviewing this information, click hello resource that was attacked and a new blade will open.</span></span>

![Javaslatok az Azure Security Center milyen toodo biztonsággal kapcsolatos riasztások](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

<span data-ttu-id="e75c5-147">A hello **leírás** mezőjét ezen a panelen, ez az esemény további információt talál.</span><span class="sxs-lookup"><span data-stu-id="e75c5-147">In hello **Description** field of this blade you will find more details about this event.</span></span> <span data-ttu-id="e75c5-148">Ezek az információk milyen kiváltott hello biztonsági riasztást, hello cél erőforráson, betekintést ajánlatot a megfelelő hello forrás IP-címet, és a fenyegetés elhárítására vonatkozó javaslatokról tooremediate.</span><span class="sxs-lookup"><span data-stu-id="e75c5-148">These additional details offer insight into what triggered hello security alert, hello target resource, when applicable hello source IP address, and recommendations about how tooremediate.</span></span>  <span data-ttu-id="e75c5-149">Bizonyos esetekben hello forrás IP-címe üres lesz (nem érhető el), mert nem minden Windows biztonsági eseménynaplója tartalmazza hello IP-címet.</span><span class="sxs-lookup"><span data-stu-id="e75c5-149">In some instances, hello source IP address will be empty (not available) because not all Windows security events logs include hello IP address.</span></span>

<span data-ttu-id="e75c5-150">a Security Center által javasolt elhárítási műveletek hello konfigurációtól függően toohello biztonsági riasztást.</span><span class="sxs-lookup"><span data-stu-id="e75c5-150">hello remediation suggested by Security Center will vary according toohello security alert.</span></span> <span data-ttu-id="e75c5-151">Bizonyos esetekben előfordulhat, hogy toouse más Azure-képességek tooimplement hello szervizelési ajánlott.</span><span class="sxs-lookup"><span data-stu-id="e75c5-151">In some cases, you may have toouse other Azure capabilities tooimplement hello recommended remediation.</span></span> <span data-ttu-id="e75c5-152">Az ilyen jellegű támadás tooblacklist hello IP-címet, hogy a támadás használatával például hello a szervizelési egy [hálózati hozzáférés-vezérlési lista](../virtual-network/virtual-networks-acl.md) vagy egy [hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) szabály.</span><span class="sxs-lookup"><span data-stu-id="e75c5-152">For example, hello remediation for this attack is tooblacklist hello IP address that is generating this attack by using a [network ACL](../virtual-network/virtual-networks-acl.md) or a [network security group](../virtual-network/virtual-networks-nsg.md) rule.</span></span>

> [!NOTE]
> <span data-ttu-id="e75c5-153">További információk a riasztások különböző típusú hello, [típusonként az Azure Security Center biztonsági riasztások](security-center-alerts-type.md).</span><span class="sxs-lookup"><span data-stu-id="e75c5-153">For more information on hello different types of alerts, read [Security Alerts by Type in Azure Security Center](security-center-alerts-type.md).</span></span>
>
>

## <a name="see-also"></a><span data-ttu-id="e75c5-154">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e75c5-154">See also</span></span>
<span data-ttu-id="e75c5-155">Ebben a dokumentumban, megtudta, hogyan tooconfigure biztonsági házirendek segítségével a Security Center.</span><span class="sxs-lookup"><span data-stu-id="e75c5-155">In this document, you learned how tooconfigure security policies in Security Center.</span></span> <span data-ttu-id="e75c5-156">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="e75c5-156">toolearn more about Security Center, see hello following:</span></span>

* [<span data-ttu-id="e75c5-157">Biztonsági incidensek kezelése az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="e75c5-157">Handling Security Incident in Azure Security Center</span></span>](security-center-incident.md)
* [<span data-ttu-id="e75c5-158">Az Azure Security Center észlelési funkciói</span><span class="sxs-lookup"><span data-stu-id="e75c5-158">Azure Security Center Detection Capabilities</span></span>](security-center-detection-capabilities.md)
* [<span data-ttu-id="e75c5-159">Útmutató az Azure Security Center tervezéséhez és működtetéséhez</span><span class="sxs-lookup"><span data-stu-id="e75c5-159">Azure Security Center Planning and Operations Guide</span></span>](security-center-planning-and-operations-guide.md)
* <span data-ttu-id="e75c5-160">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e75c5-160">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="e75c5-161">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="e75c5-161">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Find blog posts about Azure security and compliance.</span></span>
