---
title: "az Azure Security Center aaaRemediate az operációs rendszer biztonsági réseivel |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** szervizelje az operációs rendszer biztonsági rések **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 704103f7fb15835943d74b665d2bd56cb5e0a36d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a><span data-ttu-id="42d37-103">Az Azure Security Center az operációs rendszer biztonsági réseivel kapcsolat javítása</span><span class="sxs-lookup"><span data-stu-id="42d37-103">Remediate OS vulnerabilities in Azure Security Center</span></span>
<span data-ttu-id="42d37-104">Az Azure Security Center elemzi naponta a virtuális gép (VM) operációs rendszereket a konfigurációk, hogy a virtuális gép hello sebezhetőbbé tooattack és javasolja konfiguráció módosításait tooaddress biztonsági rések.</span><span class="sxs-lookup"><span data-stu-id="42d37-104">Azure Security Center analyzes daily your virtual machine (VM) operating system (OS) for configurations that could make hello VM more vulnerable tooattack and recommends configuration changes tooaddress these vulnerabilities.</span></span> <span data-ttu-id="42d37-105">A Security Center azt javasolja, hogy a biztonsági rések oldható fel, ha a virtuális gép operációs rendszer konfigurációja nem felel meg a konfigurációs szabályok ajánlott hello.</span><span class="sxs-lookup"><span data-stu-id="42d37-105">Security Center recommends that you resolve vulnerabilities when your VM’s OS configuration does not match hello recommended configuration rules.</span></span>

> [!NOTE]
> <span data-ttu-id="42d37-106">Által figyelt hello konfigurációkkal kapcsolatban lásd: hello [ajánlott konfiguráció szabályok listája](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span><span class="sxs-lookup"><span data-stu-id="42d37-106">For more information on hello specific configurations being monitored, see hello [list of recommended configuration rules](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="42d37-107">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="42d37-107">Implement hello recommendation</span></span>

> [!NOTE]
> <span data-ttu-id="42d37-108">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="42d37-108">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="42d37-109">Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="42d37-109">This document is not a step-by-step guide.</span></span>
>
>

1. <span data-ttu-id="42d37-110">A hello **javaslatok** panelen válassza **szervizelje az operációs rendszer biztonsági rések**.</span><span class="sxs-lookup"><span data-stu-id="42d37-110">In hello **Recommendations** blade, select **Remediate OS vulnerabilities**.</span></span>
   <span data-ttu-id="42d37-111">![Operációs rendszerek sebezhetőségeinek javítása][1]</span><span class="sxs-lookup"><span data-stu-id="42d37-111">![Remediate OS vulnerabilities][1]</span></span>

    <span data-ttu-id="42d37-112">Hello **szervizelje az operációs rendszer biztonsági rések** panel megnyílik, és a virtuális gépek operációs rendszer azon konfigurációit, amelyek nem felelnek meg a konfigurációs szabályok ajánlott hello sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="42d37-112">hello **Remediate OS vulnerabilities** blade opens and lists your VMs with OS configurations that do not match hello recommended configuration rules.</span></span>  <span data-ttu-id="42d37-113">Az egyes virtuális gépek hello panel azonosítja:</span><span class="sxs-lookup"><span data-stu-id="42d37-113">For each VM, hello blade identifies:</span></span>

   * <span data-ttu-id="42d37-114">**Nem sikerült szabályok** – sikertelen szabályokat, amelyek a virtuális gép operációs rendszer konfigurációja hello hello száma.</span><span class="sxs-lookup"><span data-stu-id="42d37-114">**FAILED RULES** -- hello number of rules that hello VM's OS configuration failed.</span></span>
   * <span data-ttu-id="42d37-115">**LEGUTÓBBI ellenőrzés időpontja** – hello dátum és idő, hogy a Security Center legutóbb ellenőrizte hello virtuális gép operációs rendszer konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="42d37-115">**LAST SCAN TIME** -- hello date and time that Security Center last scanned hello VM’s OS configuration.</span></span>
   * <span data-ttu-id="42d37-116">**ÁLLAPOT** – hello hello biztonsági rés aktuális állapota:</span><span class="sxs-lookup"><span data-stu-id="42d37-116">**STATE** -- hello current state of hello vulnerability:</span></span>

     * <span data-ttu-id="42d37-117">Nyissa meg: hello biztonsági rés még nem foglalkoztak még</span><span class="sxs-lookup"><span data-stu-id="42d37-117">Open: hello vulnerability has not been addressed yet</span></span>
     * <span data-ttu-id="42d37-118">Folyamatban: Hello biztonsági rés jelenleg vonatkozik, nincs szükség felhasználói műveletek is</span><span class="sxs-lookup"><span data-stu-id="42d37-118">In Progress: hello vulnerability is currently being applied, no action is required by you</span></span>
     * <span data-ttu-id="42d37-119">Megoldott: hello biztonsági rés már címezték (ha hello problémát sikerült megoldani, hello bejegyzés szürkén jelenik meg)</span><span class="sxs-lookup"><span data-stu-id="42d37-119">Resolved: hello vulnerability was already addressed (when hello issue has been resolved, hello entry is grayed out)</span></span>
   * <span data-ttu-id="42d37-120">**SÚLYOSSÁGI** – összes biztonsági rés állítottak tooa súlyossága alacsony, ami azt jelenti, a biztonsági rés kell tömni, de nem igényel azonnali beavatkozást.</span><span class="sxs-lookup"><span data-stu-id="42d37-120">**SEVERITY** -- All vulnerabilities are set tooa severity of Low, meaning a vulnerability should be addressed but does not require immediate attention.</span></span>

2. <span data-ttu-id="42d37-121">Jelöljön ki egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="42d37-121">Select a VM.</span></span> <span data-ttu-id="42d37-122">Egy panel, amely a virtuális gép megnyílik, és azokat, amelyek nem tudták hello szabályokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="42d37-122">A blade for that VM opens and displays hello rules that have failed.</span></span>
   <span data-ttu-id="42d37-123">![Konfigurációs szabályok, amelyek nem tudták][2]</span><span class="sxs-lookup"><span data-stu-id="42d37-123">![Configuration rules that have failed][2]</span></span>

3. <span data-ttu-id="42d37-124">Jelöljön ki egy szabályt.</span><span class="sxs-lookup"><span data-stu-id="42d37-124">Select a rule.</span></span> <span data-ttu-id="42d37-125">Ebben a példában lehetővé teszi, hogy válasszon **jelszónak meg kell felelnie a bonyolultsági feltételeknek**.</span><span class="sxs-lookup"><span data-stu-id="42d37-125">In this example, lets select **Password must meet complexity requirements**.</span></span> <span data-ttu-id="42d37-126">Megnyílik egy panel leíró hello sikertelen volt a szabály és hello hatása.</span><span class="sxs-lookup"><span data-stu-id="42d37-126">A blade opens describing hello failed rule and hello impact.</span></span> <span data-ttu-id="42d37-127">Hello adatokat, és fontolja meg, hogyan alkalmazza a rendszer operációsrendszer-konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="42d37-127">Review hello details and consider how operating system configurations are applied.</span></span>
  <span data-ttu-id="42d37-128">![Hello leírását meghiúsult (szabály)][3]</span><span class="sxs-lookup"><span data-stu-id="42d37-128">![Description for hello failed rule][3]</span></span>

  <span data-ttu-id="42d37-129">A Security Center a konfigurációs szabályok Common Configuration Enumeration (CCE) tooassign egyedi azonosítót használ.</span><span class="sxs-lookup"><span data-stu-id="42d37-129">Security Center uses Common Configuration Enumeration (CCE) tooassign unique identifiers for configuration rules.</span></span> <span data-ttu-id="42d37-130">a panel hello a következő információkat biztosítja:</span><span class="sxs-lookup"><span data-stu-id="42d37-130">hello following information is provided on this blade:</span></span>

  - <span data-ttu-id="42d37-131">NÉV – Szabály neve</span><span class="sxs-lookup"><span data-stu-id="42d37-131">NAME -- Name of rule</span></span>
  - <span data-ttu-id="42d37-132">SÚLYOSSÁG – CCE súlyosságérték kritikus, fontos vagy figyelmeztető</span><span class="sxs-lookup"><span data-stu-id="42d37-132">SEVERITY -- CCE severity value of critical, important, or warning</span></span>
  - <span data-ttu-id="42d37-133">CCIED--CCE egyedi azonosítója hello szabály</span><span class="sxs-lookup"><span data-stu-id="42d37-133">CCIED -- CCE unique identifier for hello rule</span></span>
  - <span data-ttu-id="42d37-134">Leírás – A szabály leírása</span><span class="sxs-lookup"><span data-stu-id="42d37-134">DESCRIPTION -- Description of rule</span></span>
  - <span data-ttu-id="42d37-135">A biztonsági RÉS--Magyarázat biztonsági rés vagy kockázat, ha a szabály nem alkalmazható</span><span class="sxs-lookup"><span data-stu-id="42d37-135">VULNERABILITY -- Explanation of vulnerability or risk if rule is not applied</span></span>
  - <span data-ttu-id="42d37-136">Gyakorolt hatás – Szabály alkalmazása Üzletmenetre gyakorolt hatás</span><span class="sxs-lookup"><span data-stu-id="42d37-136">IMPACT -- Business impact when rule is applied</span></span>
  - <span data-ttu-id="42d37-137">VÁRT érték--Értéket várt, amikor a Security Center elemzi a virtuális gép operációs rendszere konfigurációs hello szabállyal szemben</span><span class="sxs-lookup"><span data-stu-id="42d37-137">EXPECTED VALUE -- Value expected when Security Center analyzes your VM OS configuration against hello rule</span></span>
  - <span data-ttu-id="42d37-138">--A szabály szabály művelet során a virtuális gép operációs rendszere konfigurációs hello szabállyal szemben elemzése a Security Center által használt</span><span class="sxs-lookup"><span data-stu-id="42d37-138">RULE OPERATION -- Rule operation used by Security Center during analysis of your VM OS configuration against hello rule</span></span>
  - <span data-ttu-id="42d37-139">TÉNYLEGES érték--Értéket adott vissza az elemzést, a virtuális gép operációs rendszere konfigurációs hello szabállyal szemben</span><span class="sxs-lookup"><span data-stu-id="42d37-139">ACTUAL VALUE -- Value returned after analysis of your VM OS configuration against hello rule</span></span>
  - <span data-ttu-id="42d37-140">KIÉRTÉKELÉSÉNEK eredménye –-elemzés eredménye: átadni, sikertelen</span><span class="sxs-lookup"><span data-stu-id="42d37-140">EVALUATION RESULT –- Result of analysis: Pass, Fail</span></span>

## <a name="see-also"></a><span data-ttu-id="42d37-141">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="42d37-141">See also</span></span>
<span data-ttu-id="42d37-142">Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "szervizelése az operációs rendszer biztonsági rések."</span><span class="sxs-lookup"><span data-stu-id="42d37-142">This article showed you how tooimplement hello Security Center recommendation "Remediate OS vulnerabilities."</span></span> <span data-ttu-id="42d37-143">Tekintse át a konfigurációs szabályok készletét hello [Itt](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span><span class="sxs-lookup"><span data-stu-id="42d37-143">You can review hello set of configuration rules [here](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span></span> <span data-ttu-id="42d37-144">A Security Center a konfigurációs szabályok CCE (közös konfigurációs enumerálási) tooassign egyedi azonosítót használ.</span><span class="sxs-lookup"><span data-stu-id="42d37-144">Security Center uses CCE (Common Configuration Enumeration) tooassign unique identifiers for configuration rules.</span></span> <span data-ttu-id="42d37-145">A Microsoft hello [CCE](https://nvd.nist.gov/cce/index.cfm) webhelyen olvashat.</span><span class="sxs-lookup"><span data-stu-id="42d37-145">Visit hello [CCE](https://nvd.nist.gov/cce/index.cfm) site for more information.</span></span>

<span data-ttu-id="42d37-146">További információ a Security Center toolearn lásd: a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="42d37-146">toolearn more about Security Center, see hello following resources:</span></span>

* <span data-ttu-id="42d37-147">[Az Azure Security Center által támogatott platformok](security-center-os-coverage.md) -támogatott Windows és Linux virtuális gépek listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="42d37-147">[Supported platforms in Azure Security Center](security-center-os-coverage.md) - Provides a list of supported Windows and Linux VMs.</span></span>
* <span data-ttu-id="42d37-148">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) -megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="42d37-148">[Setting security policies in Azure Security Center](security-center-policies.md) - Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="42d37-149">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) -megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="42d37-149">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) - Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="42d37-150">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) -megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="42d37-150">[Security health monitoring in Azure Security Center](security-center-monitoring.md) - Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="42d37-151">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) -megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="42d37-151">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) - Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="42d37-152">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) -megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="42d37-152">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) - Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="42d37-153">[Azure Security Center: GYIK](security-center-faq.md) -gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="42d37-153">[Azure Security Center FAQ](security-center-faq.md) - Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="42d37-154">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="42d37-154">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) - Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
