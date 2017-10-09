---
title: "aaaApply rendszerfrissítések az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslatait ** alkalmazza a rendszer frissítések ** és ** rendszer frissítések ** után indítsa újra."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: 02024f1558b4758c09141fe1934c2e1a9845cc96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apply-system-updates-in-azure-security-center"></a><span data-ttu-id="ab1c4-103">Alkalmazza a rendszer frissítéseket az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="ab1c4-103">Apply system updates in Azure Security Center</span></span>
<span data-ttu-id="ab1c4-104">Az Azure Security Center naponta figyeli a Windows és Linux virtuális gépek (VM) operációsrendszer-frissítések hiányoznak.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-104">Azure Security Center monitors daily Windows and  Linux virtual machines (VMs) for missing operating system updates.</span></span> <span data-ttu-id="ab1c4-105">A Security Center lekér egy listát az elérhető biztonsági és kritikus frissítések a Windows Update webhelyről vagy a Windows Server Update Services (WSUS), attól függően, amelyek a szolgáltatás úgy van konfigurálva a Windows virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-105">Security Center retrieves a list of available security and critical updates from Windows Update or Windows Server Update Services (WSUS), depending on which service is configured on a Windows VM.</span></span>  <span data-ttu-id="ab1c4-106">A Security Center is ellenőrzi a legújabb frissítéseket hello Linux rendszereken.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-106">Security Center also checks for hello latest updates in Linux systems.</span></span> <span data-ttu-id="ab1c4-107">Ha a virtuális gép hiányzik a rendszer frissítését, a Security Center javasolni fogja-e, hogy a rendszer frissítéseinek alkalmazása</span><span class="sxs-lookup"><span data-stu-id="ab1c4-107">If your VM is missing a system update, Security Center will recommend that you apply system updates</span></span>

> [!NOTE]
> <span data-ttu-id="ab1c4-108">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-108">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="ab1c4-109">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="ab1c4-110">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="ab1c4-110">Implement hello recommendation</span></span>
1. <span data-ttu-id="ab1c4-111">A hello **javaslatok** panelen válassza **rendszer frissítéseinek alkalmazása**.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-111">In hello **Recommendations** blade, select **Apply system updates**.</span></span>

   ![Rendszerfrissítések alkalmazása][1]
2. <span data-ttu-id="ab1c4-113">Hello **rendszer frissítéseinek alkalmazása** panel nyílik meg, egy hiányzó rendszerfrissítések virtuális gépek listájának megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-113">hello **Apply system updates** blade opens displaying a list of VMs missing system updates.</span></span> <span data-ttu-id="ab1c4-114">Jelöljön ki egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-114">Select a VM.</span></span>

   ![Jelöljön ki egy virtuális Gépet][2]
3. <span data-ttu-id="ab1c4-116">Ekkor megnyílik egy panel hiányzó frissítések listájának ezt a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-116">A blade opens displaying a list of missing updates for that VM.</span></span> <span data-ttu-id="ab1c4-117">Válassza ki a rendszer frissítését.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-117">Select a system update.</span></span> <span data-ttu-id="ab1c4-118">Ebben a példában válassza ki most KB3156016.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-118">In this example, let’s select KB3156016.</span></span>

   ![Hiányzó biztonsági frissítések][3]

4. <span data-ttu-id="ab1c4-120">Hello kövesse hello **biztonsági frissítések** panel tooapply hello hiányzó frissítés.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-120">Follow hello steps in hello **Security Update** blade tooapply hello missing update.</span></span>

   ![Biztonsági frissítés][4]

## <a name="reboot-after-system-updates"></a><span data-ttu-id="ab1c4-122">Rendszerfrissítések utáni újraindítás</span><span class="sxs-lookup"><span data-stu-id="ab1c4-122">Reboot after system updates</span></span>
1. <span data-ttu-id="ab1c4-123">Térjen vissza a toohello **javaslatok** panelen.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-123">Return toohello **Recommendations** blade.</span></span> <span data-ttu-id="ab1c4-124">Egy új bejegyzést készítésének rendszerfrissítések hívása után **rendszerfrissítések után indítsa újra**.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-124">A new entry was generated after you applied system updates, called **Reboot after system updates**.</span></span> <span data-ttu-id="ab1c4-125">Ez a bejegyzés lehetővé teszi, hogy kell-e tooreboot hello VM toocomplete hello folyamat a rendszer frissítéseinek alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-125">This entry lets you know that you need tooreboot hello VM toocomplete hello process of applying system updates.</span></span>

   ![Rendszerfrissítések utáni újraindítás][5]
2. <span data-ttu-id="ab1c4-127">Válassza ki **rendszerfrissítések után indítsa újra**.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-127">Select **Reboot after system updates**.</span></span> <span data-ttu-id="ab1c4-128">Ekkor megnyílik **újraindítás függőben lévő toocomplete rendszerfrissítések** , hogy kell-e toorestart toocomplete hello virtuális gépek listáját megjelenítő panelen alkalmazza a rendszer folyamat.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-128">This opens **A restart is pending toocomplete system updates** blade displaying a list of VMs that you need toorestart toocomplete hello apply system updates process.</span></span>

   ![Újraindítása függőben van.][6]

<span data-ttu-id="ab1c4-130">Indítsa újra a virtuális gép hello Azure toocomplete hello folyamat során.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-130">Restart hello VM from Azure toocomplete hello process.</span></span>

## <a name="see-also"></a><span data-ttu-id="ab1c4-131">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="ab1c4-131">See also</span></span>
<span data-ttu-id="ab1c4-132">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="ab1c4-132">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="ab1c4-133">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-133">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="ab1c4-134">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-134">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="ab1c4-135">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-135">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="ab1c4-136">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-136">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="ab1c4-137">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-137">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="ab1c4-138">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-138">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="ab1c4-139">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="ab1c4-139">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
