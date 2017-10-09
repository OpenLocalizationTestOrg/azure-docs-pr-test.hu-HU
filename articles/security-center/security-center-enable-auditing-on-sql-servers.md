---
title: "aaaEnable naplózás és a fenyegetések észlelésére SQL-kiszolgálón az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** naplózás engedélyezését & Threat detection az SQL-kiszolgálók **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 042fca4d-7dab-4172-8614-e8c21ccb4960
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: b082c48cdbc386f14e677f4e13a7f306f37fd0e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-servers-in-azure-security-center"></a><span data-ttu-id="c9c8f-103">SQL-kiszolgálón az Azure Security Centerben a naplózás és a fenyegetések észlelésére engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c9c8f-103">Enable auditing and threat detection on SQL servers in Azure Security Center</span></span>
<span data-ttu-id="c9c8f-104">Azure Security Center javasolni fogja, hogy bekapcsolja a naplózást, és fenyegetésészlelés az összes olyan adatbázis az Azure SQL-kiszolgálón, ha a naplózás nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-104">Azure Security Center will recommend that you turn on auditing and threat detection for all databases on your Azure SQL servers if auditing is not already enabled.</span></span> <span data-ttu-id="c9c8f-105">Naplózás és a fenyegetések észlelése segítségével törvényi megfelelőség fenntartásában, ismerje meg adatbázis-tevékenység, és betekintést azok az eltérések és rendellenességek, amelyek üzleti problémát jelenthetnek, vagy a biztonság megsértésére.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-105">Auditing and threat detection can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

<span data-ttu-id="c9c8f-106">Után a naplózás bekapcsolta a Fenyegetésészlelés beállítások és e-mailek tooreceive biztonsági riasztások állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-106">Once you’ve turned on auditing you can configure Threat Detection settings and emails tooreceive security alerts.</span></span> <span data-ttu-id="c9c8f-107">A Fenyegetésészlelés az adatbázist érintő rendellenes tevékenységeket utaló esetleges biztonsági fenyegetéseket toohello adatbázis észleli.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-107">Threat Detection detects anomalous database activities indicating potential security threats toohello database.</span></span> <span data-ttu-id="c9c8f-108">Ez lehetővé teszi toodetect, és válaszoljon toopotential fenyegetéseket, azok bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-108">This enables you toodetect and respond toopotential threats as they occur.</span></span>

<span data-ttu-id="c9c8f-109">Ez a javaslat alkalmazása toohello Azure SQL-szolgáltatás csak; az Azure infrastruktúra-szolgáltatásokat (Azure IaaS) a virtuális gépeken futó SQL Server nem tartoznak bele.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-109">This recommendation applies toohello Azure SQL service only; it doesn’t include SQL Server running on your virtual machines in Azure Infrastructure Services (Azure IaaS).</span></span>

> [!NOTE]
> <span data-ttu-id="c9c8f-110">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-110">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="c9c8f-111">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="c9c8f-112">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="c9c8f-112">Implement hello recommendation</span></span>
1. <span data-ttu-id="c9c8f-113">A hello **javaslatok** panelen válassza **naplózás engedélyezését & Threat detection SQL-kiszolgálón**.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-113">In hello **Recommendations** blade, select **Enable Auditing & Threat detection on SQL servers**.</span></span>  <span data-ttu-id="c9c8f-114">Ekkor megnyílik a hello **naplózás engedélyezését & Threat detection SQL-kiszolgálón** panelen.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-114">This opens hello **Enable Auditing & Threat detection on SQL servers** blade.</span></span>

   ![SQL Serverek naplózásának engedélyezése][1]
2. <span data-ttu-id="c9c8f-116">Jelölje be egy SQL server tooenable naplózás és a fenyegetések észlelésére.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-116">Select a SQL server tooenable auditing and threat detection on.</span></span> <span data-ttu-id="c9c8f-117">Ekkor megnyílik a hello **naplózás és Fenyegetésészlelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-117">This opens hello **Auditing & Threat Detection** blade.</span></span>

3. <span data-ttu-id="c9c8f-118">A hello **naplózás és Fenyegetésészlelés** panelen válassza **ON** alatt **naplózási**.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-118">On hello **Auditing & Threat Detection** blade, select **ON** under **Auditing**.</span></span>

   ![Kapcsolja be a naplózási beállítások][2]
4. <span data-ttu-id="c9c8f-120">Hello kövesse [SQL adatbázis naplózásának hello Azure-portálon](../sql-database/sql-database-auditing-portal.md) tooconfigure tárolási a naplók tárolásához.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-120">Follow hello steps in [SQL database auditing in hello Azure portal](../sql-database/sql-database-auditing-portal.md) tooconfigure storage where your audit logs will be stored.</span></span> <span data-ttu-id="c9c8f-121">hello storage-fiók előfizetés-gyűjtemény hello alapértelmezett tárfiók.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-121">hello subscription's storage account for data collection is hello default storage account.</span></span>
5. <span data-ttu-id="c9c8f-122">Hello kövesse [Ismerkedés az SQL-adatbázis Fenyegetésészlelés](../sql-database/sql-database-threat-detection.md) tooturn a fenyegetések észlelése és tooconfigure hello listáját, amelyek megkapják a biztonsági riasztásokat, a rendellenes tevékenységek észlelésekor e-mailek és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-122">Follow hello steps in [Get started with SQL Database Threat Detection](../sql-database/sql-database-threat-detection.md) tooturn on and configure Threat Detection and tooconfigure hello list of emails that will receive security alerts upon detection of anomalous activities.</span></span>

## <a name="see-also"></a><span data-ttu-id="c9c8f-123">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c9c8f-123">See also</span></span>
<span data-ttu-id="c9c8f-124">Ez a cikk bemutatta, hogyan tooimplement hello-e a Security Center ajánlás "A naplózás engedélyezését és veszélyforrások detektálása SQL-kiszolgálón."</span><span class="sxs-lookup"><span data-stu-id="c9c8f-124">This article showed you how tooimplement hello Security Center recommendation "Enable auditing and threat detection on SQL servers."</span></span> <span data-ttu-id="c9c8f-125">További információ az SQL-adatbázis védelme toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="c9c8f-125">toolearn more about securing your SQL database, see hello following:</span></span>

* [<span data-ttu-id="c9c8f-126">Az SQL Database-adatbázis védelme</span><span class="sxs-lookup"><span data-stu-id="c9c8f-126">Securing your SQL Database</span></span>](../sql-database/sql-database-security-overview.md)

<span data-ttu-id="c9c8f-127">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="c9c8f-127">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="c9c8f-128">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-128">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="c9c8f-129">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-129">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="c9c8f-130">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-130">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="c9c8f-131">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-131">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="c9c8f-132">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-132">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="c9c8f-133">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-133">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="c9c8f-134">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="c9c8f-134">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
