---
title: "SQL-adatbázisok az Azure Security Centerben a naplózás és a fenyegetések észlelésére engedélyezése |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan valósítja meg az Azure Security Center ajánlás ** engedélyezése az SQL adatbázisok ** naplózás és a fenyegetések észlelésére."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 224b6755-2b36-4ecd-9af8-139a198e0df1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: 8f4febdaa4497fee0dc690b59cd6eaa415c5e5cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a><span data-ttu-id="59db9-103">SQL-adatbázisok az Azure Security Centerben a naplózás és a fenyegetések észlelésére engedélyezése</span><span class="sxs-lookup"><span data-stu-id="59db9-103">Enable auditing and threat detection on SQL databases in Azure Security Center</span></span>
<span data-ttu-id="59db9-104">Az Azure Security Center azt javasolja, hogy bekapcsolja a naplózást, és a fenyegetések észlelésére az összes SQL-adatbázisok ha naplózás és fenyegetésészlelés nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="59db9-104">Azure Security Center will recommend that you turn on auditing and threat detection for all SQL databases if auditing and threat detection is not already enabled.</span></span> <span data-ttu-id="59db9-105">Naplózás és a fenyegetések észlelése segítségével törvényi megfelelőség fenntartásában, ismerje meg adatbázis-tevékenység, és betekintést azok az eltérések és rendellenességek, amelyek üzleti problémát jelenthetnek, vagy a biztonság megsértésére.</span><span class="sxs-lookup"><span data-stu-id="59db9-105">Auditing and threat detection can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

<span data-ttu-id="59db9-106">Után a naplózás bekapcsolta a Fenyegetésészlelés beállítások és az e-maileket a biztonsági riasztások állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="59db9-106">Once you’ve turned on auditing you can configure Threat Detection settings and emails to receive security alerts.</span></span> <span data-ttu-id="59db9-107">A Fenyegetésészlelés az adatbázist érintő rendellenes tevékenységeket, amelyek esetleges biztonsági fenyegetéseket jelezhetnek a észleli.</span><span class="sxs-lookup"><span data-stu-id="59db9-107">Threat Detection detects anomalous database activities indicating potential security threats to the database.</span></span> <span data-ttu-id="59db9-108">Ez lehetővé teszi, hogy észlelje és azok bekövetkezésekor reagáljon a lehetséges veszélyforrásokra.</span><span class="sxs-lookup"><span data-stu-id="59db9-108">This enables you to detect and respond to potential threats as they occur.</span></span>

<span data-ttu-id="59db9-109">Ez a javaslat vonatkozik az Azure SQL-szolgáltatás csak; a virtuális gépeken futó SQL nem tartoznak bele.</span><span class="sxs-lookup"><span data-stu-id="59db9-109">This recommendation applies to the Azure SQL service only; it doesn't include SQL running on your virtual machines.</span></span>

> [!NOTE]
> <span data-ttu-id="59db9-110">Ez a dokumentum egy üzembe helyezést szemléltető példa segítségével mutatja be a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="59db9-110">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="59db9-111">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="59db9-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="59db9-112">A javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="59db9-112">Implement the recommendation</span></span>
1. <span data-ttu-id="59db9-113">Az a **javaslatok** panelen válassza **naplózás engedélyezését & Threat detection SQL-adatbázisok**.</span><span class="sxs-lookup"><span data-stu-id="59db9-113">In the **Recommendations** blade, select **Enable Auditing & Threat detection on SQL databases**.</span></span>  <span data-ttu-id="59db9-114">Ekkor megnyílik a **naplózás engedélyezését & Threat detection SQL-adatbázisok** panelen.</span><span class="sxs-lookup"><span data-stu-id="59db9-114">This opens the **Enable Auditing & Threat detection on SQL databases** blade.</span></span>

   ![SQL-adatbázis naplózásának engedélyezése][1]
2. <span data-ttu-id="59db9-116">Válassza ki az SQL-adatbázis is engedélyezi a naplózást.</span><span class="sxs-lookup"><span data-stu-id="59db9-116">Select a SQL database to enable auditing on.</span></span> <span data-ttu-id="59db9-117">Ekkor megnyílik a **naplózás és Fenyegetésészlelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="59db9-117">This opens the **Auditing & Threat Detection** blade.</span></span>

3. <span data-ttu-id="59db9-118">Az a **naplózás és Fenyegetésészlelés** panelen válassza **ON** alatt **naplózási**.</span><span class="sxs-lookup"><span data-stu-id="59db9-118">On the **Auditing & Threat Detection** blade, select **ON** under **Auditing**.</span></span>

   ![Kapcsolja be a naplózás és a fenyegetések észlelése][2]
4. <span data-ttu-id="59db9-120">Kövesse a [SQL adatbázis Fenyegetésészlelés az Azure portálon](../sql-database/sql-database-threat-detection-portal.md) bekapcsolásának és konfigurálásának fenyegetések észlelése és konfigurálhatja az e-maileket, a rendellenes tevékenységek észlelésekor a biztonsági riasztásokat fogadó listáját.</span><span class="sxs-lookup"><span data-stu-id="59db9-120">Follow the steps in [SQL Database Threat Detection in the Azure portal](../sql-database/sql-database-threat-detection-portal.md) to turn on and configure Threat Detection and to configure the list of emails that will receive security alerts upon detection of anomalous activities.</span></span>

## <a name="see-also"></a><span data-ttu-id="59db9-121">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="59db9-121">See also</span></span>
<span data-ttu-id="59db9-122">Ez a cikk bemutatta megvalósításához a Security Center javaslat "engedélyezése naplózási & Threat detection SQL-adatbázisok."</span><span class="sxs-lookup"><span data-stu-id="59db9-122">This article showed you how to implement the Security Center recommendation "Enable Auditing & Threat detection on SQL databases."</span></span> <span data-ttu-id="59db9-123">SQL-adatbázisok védelme kapcsolatos további tudnivalókért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="59db9-123">To learn more about securing your SQL database, see the following:</span></span>

* [<span data-ttu-id="59db9-124">Az SQL Database-adatbázis védelme</span><span class="sxs-lookup"><span data-stu-id="59db9-124">Securing your SQL Database</span></span>](../sql-database/sql-database-security-overview.md)

<span data-ttu-id="59db9-125">A Security Centerrel kapcsolatos további információkért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="59db9-125">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="59db9-126">[Biztonsági szabályzatok beállítása az Azure Security Centerben](security-center-policies.md) – Ez a cikk bemutatja, hogyan konfigurálhat biztonsági házirendeket Azure-előfizetései és -erőforráscsoportjai számára.</span><span class="sxs-lookup"><span data-stu-id="59db9-126">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="59db9-127">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="59db9-127">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="59db9-128">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – útmutató az Azure-erőforrások állapotának figyelésére.</span><span class="sxs-lookup"><span data-stu-id="59db9-128">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="59db9-129">[Biztonsági riasztások kezelése és válaszadás a riasztásokra az Azure Security Centerben](security-center-managing-and-responding-alerts.md) – A biztonsági riasztások kezelése és az azokra való reagálás.</span><span class="sxs-lookup"><span data-stu-id="59db9-129">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="59db9-130">[Partneri megoldások monitorozása az Azure Security Centerrel](security-center-partner-solutions.md) – Útmutató a partneri megoldások biztonsági állapotának monitorozásához.</span><span class="sxs-lookup"><span data-stu-id="59db9-130">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="59db9-131">[Azure Security Center – gyakran ismételt kérdések](security-center-faq.md) – Gyakran ismételt kérdések a szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="59db9-131">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="59db9-132">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) --az Azure biztonsági legfrissebb hírek és információ.</span><span class="sxs-lookup"><span data-stu-id="59db9-132">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
