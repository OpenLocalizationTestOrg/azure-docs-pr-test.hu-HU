---
title: "aaaEnable naplózás és a fenyegetések észlelésére az SQL-adatbázisok az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** engedélyezése az SQL adatbázisok ** naplózás és a fenyegetések észlelésére."
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
ms.openlocfilehash: c94140acf37cabaca3e681ba5db79d6827e7b9db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a><span data-ttu-id="f2fea-103">SQL-adatbázisok az Azure Security Centerben a naplózás és a fenyegetések észlelésére engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f2fea-103">Enable auditing and threat detection on SQL databases in Azure Security Center</span></span>
<span data-ttu-id="f2fea-104">Az Azure Security Center azt javasolja, hogy bekapcsolja a naplózást, és a fenyegetések észlelésére az összes SQL-adatbázisok ha naplózás és fenyegetésészlelés nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="f2fea-104">Azure Security Center will recommend that you turn on auditing and threat detection for all SQL databases if auditing and threat detection is not already enabled.</span></span> <span data-ttu-id="f2fea-105">Naplózás és a fenyegetések észlelése segítségével törvényi megfelelőség fenntartásában, ismerje meg adatbázis-tevékenység, és betekintést azok az eltérések és rendellenességek, amelyek üzleti problémát jelenthetnek, vagy a biztonság megsértésére.</span><span class="sxs-lookup"><span data-stu-id="f2fea-105">Auditing and threat detection can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

<span data-ttu-id="f2fea-106">Után a naplózás bekapcsolta a Fenyegetésészlelés beállítások és e-mailek tooreceive biztonsági riasztások állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="f2fea-106">Once you’ve turned on auditing you can configure Threat Detection settings and emails tooreceive security alerts.</span></span> <span data-ttu-id="f2fea-107">A Fenyegetésészlelés az adatbázist érintő rendellenes tevékenységeket utaló esetleges biztonsági fenyegetéseket toohello adatbázis észleli.</span><span class="sxs-lookup"><span data-stu-id="f2fea-107">Threat Detection detects anomalous database activities indicating potential security threats toohello database.</span></span> <span data-ttu-id="f2fea-108">Ez lehetővé teszi toodetect, és válaszoljon toopotential fenyegetéseket, azok bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="f2fea-108">This enables you toodetect and respond toopotential threats as they occur.</span></span>

<span data-ttu-id="f2fea-109">Ez a javaslat alkalmazása toohello Azure SQL-szolgáltatás csak; a virtuális gépeken futó SQL nem tartoznak bele.</span><span class="sxs-lookup"><span data-stu-id="f2fea-109">This recommendation applies toohello Azure SQL service only; it doesn't include SQL running on your virtual machines.</span></span>

> [!NOTE]
> <span data-ttu-id="f2fea-110">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="f2fea-110">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="f2fea-111">A dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="f2fea-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="f2fea-112">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="f2fea-112">Implement hello recommendation</span></span>
1. <span data-ttu-id="f2fea-113">A hello **javaslatok** panelen válassza **naplózás engedélyezését & Threat detection SQL-adatbázisok**.</span><span class="sxs-lookup"><span data-stu-id="f2fea-113">In hello **Recommendations** blade, select **Enable Auditing & Threat detection on SQL databases**.</span></span>  <span data-ttu-id="f2fea-114">Ekkor megnyílik a hello **naplózás engedélyezését & Threat detection SQL-adatbázisok** panelen.</span><span class="sxs-lookup"><span data-stu-id="f2fea-114">This opens hello **Enable Auditing & Threat detection on SQL databases** blade.</span></span>

   ![SQL-adatbázis naplózásának engedélyezése][1]
2. <span data-ttu-id="f2fea-116">Válassza ki, egy SQL adatbázis tooenable naplózást.</span><span class="sxs-lookup"><span data-stu-id="f2fea-116">Select a SQL database tooenable auditing on.</span></span> <span data-ttu-id="f2fea-117">Ekkor megnyílik a hello **naplózás és Fenyegetésészlelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="f2fea-117">This opens hello **Auditing & Threat Detection** blade.</span></span>

3. <span data-ttu-id="f2fea-118">A hello **naplózás és Fenyegetésészlelés** panelen válassza **ON** alatt **naplózási**.</span><span class="sxs-lookup"><span data-stu-id="f2fea-118">On hello **Auditing & Threat Detection** blade, select **ON** under **Auditing**.</span></span>

   ![Kapcsolja be a naplózás és a fenyegetések észlelése][2]
4. <span data-ttu-id="f2fea-120">Hello kövesse [SQL adatbázis Fenyegetésészlelés hello Azure-portálon](../sql-database/sql-database-threat-detection-portal.md) tooturn a fenyegetések észlelése és tooconfigure hello listáját, amelyek megkapják a biztonsági riasztásokat, a rendellenes tevékenységek észlelésekor e-mailek és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f2fea-120">Follow hello steps in [SQL Database Threat Detection in hello Azure portal](../sql-database/sql-database-threat-detection-portal.md) tooturn on and configure Threat Detection and tooconfigure hello list of emails that will receive security alerts upon detection of anomalous activities.</span></span>

## <a name="see-also"></a><span data-ttu-id="f2fea-121">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f2fea-121">See also</span></span>
<span data-ttu-id="f2fea-122">Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "Enable naplózási & Threat észlelési SQL-adatbázisok."</span><span class="sxs-lookup"><span data-stu-id="f2fea-122">This article showed you how tooimplement hello Security Center recommendation "Enable Auditing & Threat detection on SQL databases."</span></span> <span data-ttu-id="f2fea-123">További információ az SQL-adatbázis védelme toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="f2fea-123">toolearn more about securing your SQL database, see hello following:</span></span>

* [<span data-ttu-id="f2fea-124">Az SQL Database-adatbázis védelme</span><span class="sxs-lookup"><span data-stu-id="f2fea-124">Securing your SQL Database</span></span>](../sql-database/sql-database-security-overview.md)

<span data-ttu-id="f2fea-125">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="f2fea-125">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="f2fea-126">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="f2fea-126">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="f2fea-127">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="f2fea-127">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="f2fea-128">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="f2fea-128">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="f2fea-129">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="f2fea-129">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="f2fea-130">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="f2fea-130">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="f2fea-131">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f2fea-131">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="f2fea-132">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="f2fea-132">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
