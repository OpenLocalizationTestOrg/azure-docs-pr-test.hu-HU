---
title: "Riasztások érvényesítése az Azure Security Centerben | Microsoft Docs"
description: "Ez a dokumentum az Azure Security Center biztonsági riasztásainak érvényesítését ismerteti."
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
ms.openlocfilehash: 121b5d8f023a9b663d0e7af26dce8f81db27672c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="alerts-validation-in-azure-security-center"></a><span data-ttu-id="da4f1-103">Riasztások érvényesítése az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="da4f1-103">Alerts Validation in Azure Security Center</span></span>
<span data-ttu-id="da4f1-104">A dokumentum ismerteti, hogyan ellenőrizheti, hogy a rendszere megfelelően konfigurálva van-e az Azure Security Center riasztásaihoz.</span><span class="sxs-lookup"><span data-stu-id="da4f1-104">This document helps you learn how to verify if your system is properly configured for Azure Security Center alerts.</span></span>

## <a name="what-are-security-alerts"></a><span data-ttu-id="da4f1-105">Mik azok a biztonsági riasztások?</span><span class="sxs-lookup"><span data-stu-id="da4f1-105">What are security alerts?</span></span>
<span data-ttu-id="da4f1-106">A Security Center automatikusan gyűjti, elemzi és integrálja az Azure-erőforrások, a hálózat és a csatlakoztatott partnermegoldások, például a tűzfalak és a végpontvédelmi megoldások naplóadatait a fenyegetések észlelése és riasztások küldése érdekében.</span><span class="sxs-lookup"><span data-stu-id="da4f1-106">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, the network, and connected partner solutions, like firewall and endpoint protection solutions, to detect and alert you to threats.</span></span> <span data-ttu-id="da4f1-107">A biztonsági riasztásokkal kapcsolatos további információkért olvassa el a [Biztonsági riasztások kezelése és válaszadás a riasztásokra az Azure Security Centerben](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) című cikket, a különböző típusú riasztásokról kapcsolatos további információkért pedig tekintse meg a [Az Azure Security Center biztonsági riasztásainak megismerése](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) című cikket.</span><span class="sxs-lookup"><span data-stu-id="da4f1-107">Read [Managing and responding to security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) for more information about security alerts, and read [Understanding security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) to learn more about the different types of alerts.</span></span>

## <a name="alert-validation"></a><span data-ttu-id="da4f1-108">Riasztások érvényesítése</span><span class="sxs-lookup"><span data-stu-id="da4f1-108">Alert validation</span></span>
<span data-ttu-id="da4f1-109">Miután telepítette a számítógépére a Security Centert, kövesse az alábbi lépéseket azon a számítógépen, ahol Ön szeretne lenni a riasztásban jelentett megtámadott erőforrás:</span><span class="sxs-lookup"><span data-stu-id="da4f1-109">After Security Center agent is installed on your computer, follow the steps below from the computer where you want to be the attacked resource of the alert:</span></span>

1. <span data-ttu-id="da4f1-110">Másoljon egy végrehajtható fájlt (például calc.exe) a számítógép asztalára vagy egy másik tetszőleges könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="da4f1-110">Copy an executable (for example calc.exe) to the computer’s desktop, or other directory of your convenience.</span></span>
2. <span data-ttu-id="da4f1-111">Nevezze át ezt a fájlt **ASC_AlertTest_662jfi039N.exe** névre.</span><span class="sxs-lookup"><span data-stu-id="da4f1-111">Rename this file to **ASC_AlertTest_662jfi039N.exe**.</span></span>
3. <span data-ttu-id="da4f1-112">Nyissa meg a parancssort, és hajtsa végre a fájlt egy argumentummal (egy hamis argumentumnévvel), például: *ASC_AlertTest_662jfi039N.exe -foo*</span><span class="sxs-lookup"><span data-stu-id="da4f1-112">Open the command prompt and execute this file with an argument (just a fake argument name), such as: *ASC_AlertTest_662jfi039N.exe -foo*</span></span>
4. <span data-ttu-id="da4f1-113">Várjon 5-10 percet, és nyissa meg a Security Center riasztásait.</span><span class="sxs-lookup"><span data-stu-id="da4f1-113">Wait 5 to 10 minutes and open Security Center Alerts.</span></span> <span data-ttu-id="da4f1-114">Ott az alábbihoz hasonló riasztást fog találni:</span><span class="sxs-lookup"><span data-stu-id="da4f1-114">There you should find an alert similar to following one:</span></span>

    ![Riasztások érvényesítése](./media/security-center-alert-validation/security-center-alert-validation-fig1.png)

<span data-ttu-id="da4f1-116">Amikor ellenőrzi ezt a riasztást, győződjön meg róla, hogy az Argumentumok naplózása beállítás értéke Igaz.</span><span class="sxs-lookup"><span data-stu-id="da4f1-116">When reviewing this alert, make sure the field Arguments Auditing Enabled appears as true.</span></span> <span data-ttu-id="da4f1-117">Ha hamisként jelenik meg, engedélyezze a parancssori argumentumok naplózását.</span><span class="sxs-lookup"><span data-stu-id="da4f1-117">If it shows false, you need to enable command-line arguments auditing.</span></span> <span data-ttu-id="da4f1-118">Ezt a beállítást a következő parancssorral engedélyezheti:</span><span class="sxs-lookup"><span data-stu-id="da4f1-118">You can enable this option using the following command line:</span></span>

<span data-ttu-id="da4f1-119">*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*</span><span class="sxs-lookup"><span data-stu-id="da4f1-119">*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*</span></span>


## <a name="see-also"></a><span data-ttu-id="da4f1-120">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="da4f1-120">See also</span></span>
<span data-ttu-id="da4f1-121">Ez a cikk a riasztások érvényesítési folyamatát mutatta be.</span><span class="sxs-lookup"><span data-stu-id="da4f1-121">This article introduced you to the alerts validation process.</span></span> <span data-ttu-id="da4f1-122">Most, hogy már ismeri az érvényesítést, tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="da4f1-122">Now that you're familiar with this validation, try the following articles:</span></span>

* <span data-ttu-id="da4f1-123">[Biztonsági riasztások kezelése és válaszadás a riasztásokra az Azure Security Centerben](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).</span><span class="sxs-lookup"><span data-stu-id="da4f1-123">[Managing and responding to security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).</span></span> <span data-ttu-id="da4f1-124">A Security Center-riasztások kezelését és a biztonsági eseményekre való válaszadást ismertető útmutató.</span><span class="sxs-lookup"><span data-stu-id="da4f1-124">Learn how to manage alerts, and respond to security incidents in Security Center.</span></span>
* <span data-ttu-id="da4f1-125">[Biztonsági állapotfigyelés az Azure Security Centerben](security-center-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="da4f1-125">[Security health monitoring in Azure Security Center](security-center-monitoring.md).</span></span> <span data-ttu-id="da4f1-126">Az Azure-erőforrások állapotának figyelését ismertető útmutató.</span><span class="sxs-lookup"><span data-stu-id="da4f1-126">Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="da4f1-127">[Az Azure Security Center biztonsági riasztásainak megismerése](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).</span><span class="sxs-lookup"><span data-stu-id="da4f1-127">[Understanding security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).</span></span> <span data-ttu-id="da4f1-128">A különböző típusú biztonsági riasztásokat ismertető útmutató.</span><span class="sxs-lookup"><span data-stu-id="da4f1-128">Learn about the different types of security alerts.</span></span>
* <span data-ttu-id="da4f1-129">[Azure Security Center – Hibaelhárítási útmutató](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide).</span><span class="sxs-lookup"><span data-stu-id="da4f1-129">[Azure Security Center Troubleshooting Guide](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide).</span></span> <span data-ttu-id="da4f1-130">A Security Center gyakori problémáinak elhárítását ismereti.</span><span class="sxs-lookup"><span data-stu-id="da4f1-130">Learn how to troubleshoot common issues in Security Center.</span></span> 
* <span data-ttu-id="da4f1-131">[Azure Security Center – gyakori kérdések](security-center-faq.md)</span><span class="sxs-lookup"><span data-stu-id="da4f1-131">[Azure Security Center FAQ](security-center-faq.md).</span></span> <span data-ttu-id="da4f1-132">Gyakori kérdések a szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="da4f1-132">Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="da4f1-133">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)</span><span class="sxs-lookup"><span data-stu-id="da4f1-133">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/).</span></span> <span data-ttu-id="da4f1-134">Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="da4f1-134">Find blog posts about Azure security and compliance.</span></span>

