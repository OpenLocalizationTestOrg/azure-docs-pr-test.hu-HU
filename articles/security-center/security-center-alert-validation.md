---
title: "aaaAlerts ellenőrzése az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum segít toovalidate hello biztonsági riasztások az Azure Security Centerben."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: yurid
ms.openlocfilehash: 030e9e74303758192eedaf517f1cb0d2e4a7852e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="alerts-validation-in-azure-security-center"></a><span data-ttu-id="76eca-103">Riasztások érvényesítése az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="76eca-103">Alerts Validation in Azure Security Center</span></span>
<span data-ttu-id="76eca-104">Ez a dokumentum segítségével megtudhatja, hogyan tooverify, ha a rendszer az Azure Security Center riasztásait megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="76eca-104">This document helps you learn how tooverify if your system is properly configured for Azure Security Center alerts.</span></span>

## <a name="what-are-security-alerts"></a><span data-ttu-id="76eca-105">Mik azok a biztonsági riasztások?</span><span class="sxs-lookup"><span data-stu-id="76eca-105">What are security alerts?</span></span>
<span data-ttu-id="76eca-106">A Security Center automatikusan gyűjti, elemzi és integrálja az Azure-erőforrások, hello és hálózati összekapcsolt partneri megoldások, például a tűzfal és az endpoint protection megoldások, a toodetect és a riasztás akkor toothreats naplóadatait.</span><span class="sxs-lookup"><span data-stu-id="76eca-106">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, hello network, and connected partner solutions, like firewall and endpoint protection solutions, toodetect and alert you toothreats.</span></span> <span data-ttu-id="76eca-107">Olvasási [az Azure Security Centerben riasztások kezelése és válaszol toosecurity](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) tájékozódhat a biztonsági riasztásokat, és olvasási [az Azure Security Center biztonsági riasztások ismertetése](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) további toolearn kapcsolatos riasztások különböző hello.</span><span class="sxs-lookup"><span data-stu-id="76eca-107">Read [Managing and responding toosecurity alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) for more information about security alerts, and read [Understanding security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) toolearn more about hello different types of alerts.</span></span>

## <a name="alert-validation"></a><span data-ttu-id="76eca-108">Riasztások érvényesítése</span><span class="sxs-lookup"><span data-stu-id="76eca-108">Alert validation</span></span>
<span data-ttu-id="76eca-109">Miután a Security Center-ügynök telepítve van a számítógépen, lépésekkel hello alatt hello számítógépről toobe hello megtámadott erőforrások hello riasztás kívánt:</span><span class="sxs-lookup"><span data-stu-id="76eca-109">After Security Center agent is installed on your computer, follow hello steps below from hello computer where you want toobe hello attacked resource of hello alert:</span></span>

1. <span data-ttu-id="76eca-110">Másolja át egy végrehajtható (a példa calc.exe) toohello asztal, vagy más könyvtárához, a felhasználók kényelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="76eca-110">Copy an executable (for example calc.exe) toohello computer’s desktop, or other directory of your convenience.</span></span>
2. <span data-ttu-id="76eca-111">A fájl átnevezése túl**ASC_AlertTest_662jfi039N.exe**.</span><span class="sxs-lookup"><span data-stu-id="76eca-111">Rename this file too**ASC_AlertTest_662jfi039N.exe**.</span></span>
3. <span data-ttu-id="76eca-112">Nyisson meg hello parancssort, és a fájl egy argumentummal (csak egy hamis argumentum nevét), például a végrehajtása: *ASC_AlertTest_662jfi039N.exe - PEL*</span><span class="sxs-lookup"><span data-stu-id="76eca-112">Open hello command prompt and execute this file with an argument (just a fake argument name), such as: *ASC_AlertTest_662jfi039N.exe -foo*</span></span>
4. <span data-ttu-id="76eca-113">Várjon 5 too10 percet, és nyissa meg a Security Center riasztásait.</span><span class="sxs-lookup"><span data-stu-id="76eca-113">Wait 5 too10 minutes and open Security Center Alerts.</span></span> <span data-ttu-id="76eca-114">Nincs látnia kell egy riasztási hasonló toofollowing egyikét:</span><span class="sxs-lookup"><span data-stu-id="76eca-114">There you should find an alert similar toofollowing one:</span></span>

    ![Riasztások érvényesítése](./media/security-center-alert-validation/security-center-alert-validation-fig1.png)

<span data-ttu-id="76eca-116">Ha ez a riasztás megtekintésével ellenőrizze, hogy argumentumok naplózását engedélyezni hello mező jelenik meg az igaz.</span><span class="sxs-lookup"><span data-stu-id="76eca-116">When reviewing this alert, make sure hello field Arguments Auditing Enabled appears as true.</span></span> <span data-ttu-id="76eca-117">Azt illusztrálja, hamis, ha szüksége tooenable naplózás parancssori argumentumokat.</span><span class="sxs-lookup"><span data-stu-id="76eca-117">If it shows false, you need tooenable command-line arguments auditing.</span></span> <span data-ttu-id="76eca-118">Engedélyezheti ezt a beállítást, a következő parancssori hello használata:</span><span class="sxs-lookup"><span data-stu-id="76eca-118">You can enable this option using hello following command line:</span></span>

<span data-ttu-id="76eca-119">*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*</span><span class="sxs-lookup"><span data-stu-id="76eca-119">*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*</span></span>


## <a name="see-also"></a><span data-ttu-id="76eca-120">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="76eca-120">See also</span></span>
<span data-ttu-id="76eca-121">Ez a cikk bevezetett toohello riasztások érvényesítési folyamata.</span><span class="sxs-lookup"><span data-stu-id="76eca-121">This article introduced you toohello alerts validation process.</span></span> <span data-ttu-id="76eca-122">Most, hogy ismeri az ellenőrzés, próbálja meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="76eca-122">Now that you're familiar with this validation, try hello following articles:</span></span>

* <span data-ttu-id="76eca-123">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).</span><span class="sxs-lookup"><span data-stu-id="76eca-123">[Managing and responding toosecurity alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).</span></span> <span data-ttu-id="76eca-124">Megtudhatja, hogyan toomanage riasztásokat, és válaszoljon toosecurity incidensek a Security Center.</span><span class="sxs-lookup"><span data-stu-id="76eca-124">Learn how toomanage alerts, and respond toosecurity incidents in Security Center.</span></span>
* <span data-ttu-id="76eca-125">[Biztonsági állapotfigyelés az Azure Security Centerben](security-center-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="76eca-125">[Security health monitoring in Azure Security Center](security-center-monitoring.md).</span></span> <span data-ttu-id="76eca-126">Ismerje meg, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="76eca-126">Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="76eca-127">[Az Azure Security Center biztonsági riasztásainak megismerése](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).</span><span class="sxs-lookup"><span data-stu-id="76eca-127">[Understanding security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).</span></span> <span data-ttu-id="76eca-128">További információk a biztonsági riasztások különböző típusú hello.</span><span class="sxs-lookup"><span data-stu-id="76eca-128">Learn about hello different types of security alerts.</span></span>
* <span data-ttu-id="76eca-129">[Azure Security Center – Hibaelhárítási útmutató](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide).</span><span class="sxs-lookup"><span data-stu-id="76eca-129">[Azure Security Center Troubleshooting Guide](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide).</span></span> <span data-ttu-id="76eca-130">Ismerje meg, hogyan tootroubleshoot közös állít ki a biztonsági központban.</span><span class="sxs-lookup"><span data-stu-id="76eca-130">Learn how tootroubleshoot common issues in Security Center.</span></span> 
* <span data-ttu-id="76eca-131">[Azure Security Center – gyakori kérdések](security-center-faq.md)</span><span class="sxs-lookup"><span data-stu-id="76eca-131">[Azure Security Center FAQ](security-center-faq.md).</span></span> <span data-ttu-id="76eca-132">Hello szolgáltatással kapcsolatos gyakran ismételt kérdések található.</span><span class="sxs-lookup"><span data-stu-id="76eca-132">Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="76eca-133">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)</span><span class="sxs-lookup"><span data-stu-id="76eca-133">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/).</span></span> <span data-ttu-id="76eca-134">Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="76eca-134">Find blog posts about Azure security and compliance.</span></span>

