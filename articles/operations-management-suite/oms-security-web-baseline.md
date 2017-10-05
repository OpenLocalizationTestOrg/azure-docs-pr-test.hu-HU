---
title: "Operations Management Suite biztonsági és auditálási megoldás webes alapkonfigurációja | Microsoft Docs"
description: "Ez a dokumentum ismerteti, hogyan lehet használni az OMS biztonsági és auditálási megoldást az összes figyelt webkiszolgáló webes alapkonfigurációjának megfelelőségi és biztonsági célú értékelésére."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ROBOTS: NOINDEX
redirect_url: https://www.microsoft.com/cloud-platform/security-and-compliance
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: 0d4707dc0c0ffbf40d0d10a6d12b9709a9655258
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a><span data-ttu-id="27691-103">A webes alapkonfiguráció értékelése az Operations Management Suite biztonsági és auditálási megoldásában</span><span class="sxs-lookup"><span data-stu-id="27691-103">Web Baseline Assessment in Operations Management Suite Security and Audit Solution</span></span>
<span data-ttu-id="27691-104">Ez a dokumentum segít az [Operations Management Suite (OMS) biztonsági és auditálási megoldás](operations-management-suite-overview.md) webes alapkonfiguráció-értékelési képességeinek használatában, hogy hozzáférhessen a figyelt erőforrások biztonsági állapotához.</span><span class="sxs-lookup"><span data-stu-id="27691-104">This document helps you to use [Operations Management Suite (OMS) Security and Audit Solution](operations-management-suite-overview.md) web baseline assessment capabilities to access the secure state of your monitored resources.</span></span>

## <a name="what-is-web-baseline-assessment"></a><span data-ttu-id="27691-105">Mi az a webes alapkonfiguráció-értékelés?</span><span class="sxs-lookup"><span data-stu-id="27691-105">What is Web Baseline Assessment?</span></span>
<span data-ttu-id="27691-106">Jelenleg az OMS biztonsági megoldása biztosít alapkonfiguráció-értékelést az operációs rendszerekhez.</span><span class="sxs-lookup"><span data-stu-id="27691-106">Currently OMS Security provides security baseline assessment for operating systems.</span></span> <span data-ttu-id="27691-107">A kiszolgálók operációsrendszer-beállításait 24 óránként ellenőrzi, és lehetővé teszi a potenciálisan sebezhető beállítások áttekintését.</span><span class="sxs-lookup"><span data-stu-id="27691-107">It scans the operating system settings of your servers every 24 hours and provides a view into potentially vulnerable settings.</span></span> <span data-ttu-id="27691-108">Ezzel kapcsolatban további információkat olvashat [az alapkonfiguráció az Operations Management Suite biztonsági és auditálási megoldásában történő értékelését](oms-security-baseline.md) ismertető cikkben.</span><span class="sxs-lookup"><span data-stu-id="27691-108">Read [Baseline Assessment in Operations Management Suite Security and Audit Solution](oms-security-baseline.md) for more information on this.</span></span>

<span data-ttu-id="27691-109">A webes alapkonfiguráció-értékelés célja a potenciálisan sebezhető webkiszolgáló-beállítások megkeresése.</span><span class="sxs-lookup"><span data-stu-id="27691-109">The goal of the web baseline assessment is to find potentially vulnerable web server settings.</span></span> <span data-ttu-id="27691-110">A három elsődleges forrás a webes alapkonfigurációkhoz a .NET-, az ASP.NET- és az IIS-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="27691-110">The three primary sources for the web baseline configurations are: .NET, ASP.NET, and IIS configuration.</span></span>  <span data-ttu-id="27691-111">Az operációs rendszer alapkonfigurációjának értékeléséhez hasonlóan az OMS biztonsági megoldása 24 óránként fogja ellenőrizni a webkiszolgálóit, majd lehetővé teszi azok biztonsági állapotának áttekintését.</span><span class="sxs-lookup"><span data-stu-id="27691-111">Just like the operating system baseline assessment, OMS Security is going to scan your web servers every 24hrs and provide a view into security state of them.</span></span>  <span data-ttu-id="27691-112">Az Internet Information Services-ben (IIS) a konfigurációk nagy mértékben testreszabhatók, ez pedig lehetővé teszi számos hely- és alkalmazásszint felülbírálását.</span><span class="sxs-lookup"><span data-stu-id="27691-112">In Internet Information Service (IIS), configurations are highly customizable, which allows various site and application levels to be overridden.</span></span> <span data-ttu-id="27691-113">A vizsgáló az alapértelmezett gyökérszinten felül minden alkalmazás-/helyszinten ellenőrzi a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="27691-113">The scanner checks the settings at each application/site level in addition to the default root level.</span></span> <span data-ttu-id="27691-114">Ez segít a potenciálisan sebezhető beállítások helyét azonosítani, valamint azokat gyorsan kijavítani.</span><span class="sxs-lookup"><span data-stu-id="27691-114">This helps you to identify potential vulnerability settings locations and quickly remediate.</span></span>


## <a name="web-security-baseline-assessment"></a><span data-ttu-id="27691-115">A webes biztonsági alapkonfiguráció értékelése</span><span class="sxs-lookup"><span data-stu-id="27691-115">Web Security Baseline Assessment</span></span>
<span data-ttu-id="27691-116">Ehhez az előzetes verzióhoz ez a szolgáltatás az OMS keresési szolgáltatásán keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="27691-116">For this preview this feature is going to be accessed using the OMS Search option.</span></span> <span data-ttu-id="27691-117">A megfelelő lekérdezés végrehajtásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="27691-117">Follow the steps below to perform the appropriated query:</span></span>

1. <span data-ttu-id="27691-118">A **Microsoft Operations Management Suite** fő irányítópultján kattintson a **Biztonság és auditálás** csempére.</span><span class="sxs-lookup"><span data-stu-id="27691-118">In the **Microsoft Operations Management Suite** main dashboard click **Security and Audit** tile.</span></span>
2. <span data-ttu-id="27691-119">A **Biztonság és auditálás** irányítópulton kattintson a **Naplók keresése** gombra.</span><span class="sxs-lookup"><span data-stu-id="27691-119">In the **Security and Audit** dashboard, click **Log Search** button.</span></span>
3. <span data-ttu-id="27691-120">Az első használható lekérdezés a **Webes alapkonfiguráció-értékelés összegzése**.</span><span class="sxs-lookup"><span data-stu-id="27691-120">The first query that you can use is the **Web Baseline Assessment Summary**.</span></span> <span data-ttu-id="27691-121">A **Kezdje itt a keresést** mezőbe írja be a következő lekérdezést: Type*=SecurityBaselineSummary BaselineType=web*.</span><span class="sxs-lookup"><span data-stu-id="27691-121">In the **Begin search here** field, type this query: Type*=SecurityBaselineSummary BaselineType=web*.</span></span> <span data-ttu-id="27691-122">A következő képernyő egy kimeneti mintát jelenít meg:</span><span class="sxs-lookup"><span data-stu-id="27691-122">The following screen has an output sample:</span></span>

![Webes alapkonfiguráció-értékelés összegzése](./media/oms-security-web-baseline/oms-security-web-baseline-fig1-new.png)

> [!NOTE]
> <span data-ttu-id="27691-124">Ebben a lekérdezésben minden rekord egy kiszolgáló értékelési összegzését jelöli.</span><span class="sxs-lookup"><span data-stu-id="27691-124">In this query, each record indicates assessment summary on a single server.</span></span>

<span data-ttu-id="27691-125">Ha már a **Naplók keresése** területen van, különböző lekérdezéseket megadva több információt szerezhet be a webes alapkonfiguráció-értékelésről.</span><span class="sxs-lookup"><span data-stu-id="27691-125">Once you are in the **Log Search**, you can type different queries to obtain more information about the web baseline assessment.</span></span> <span data-ttu-id="27691-126">Az előző lekérdezésen túl ebben az előzetes verzióban az alábbiakat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="27691-126">In addition to the previous query, you can also use the following ones in this preview.</span></span>

<span data-ttu-id="27691-127">**Webes alapkonfigurációsszabály-értékelés**: Minden rekord egy kiszolgáló webes alapkonfigurációsszabály-értékelését jelöli.</span><span class="sxs-lookup"><span data-stu-id="27691-127">**Web Baseline Rule Assessment**: Each record represents a single web baseline rule evaluation on a single server.</span></span> <span data-ttu-id="27691-128">Tartalmazza a szabály összes adatát, a helyét, a várt eredményt és az aktuális eredményt.</span><span class="sxs-lookup"><span data-stu-id="27691-128">It includes all data for the rule, location, the expected result, and the actual result.</span></span>

<span data-ttu-id="27691-129">**Lekérdezés**: Type*=SecurityBaseline BaselineType=web*</span><span class="sxs-lookup"><span data-stu-id="27691-129">**Query**: Type*=SecurityBaseline BaselineType=web*</span></span>

![Webes alapkonfigurációsszabály-értékelés](./media/oms-security-web-baseline/oms-security-web-baseline-fig2.png)

<span data-ttu-id="27691-131">**Az összes eredmény megjelenítése egy adott kiszolgálóhoz**:</span><span class="sxs-lookup"><span data-stu-id="27691-131">**Show all results for a specific server**: This query shows how to see results of a specific server.</span></span>

<span data-ttu-id="27691-132">**Lekérdezés**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*</span><span class="sxs-lookup"><span data-stu-id="27691-132">**Query**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*</span></span>

![Minden eredmény](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

<span data-ttu-id="27691-134">E rekordokkal és lekérdezésekkel létrehozhatja a saját irányítópultjait, jelentéseit vagy riasztásait.</span><span class="sxs-lookup"><span data-stu-id="27691-134">You can also use these records/queries to create your own dashboards, reports, or alerts.</span></span> <span data-ttu-id="27691-135">Az alábbi képernyőn egy mintát láthat egy felhasználói felületi vezérlőre, amelyet hozzáadhat az irányítópulthoz.</span><span class="sxs-lookup"><span data-stu-id="27691-135">The screen below has a sample UI control that you can add to your dashboard.</span></span> <span data-ttu-id="27691-136">[Itt](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/) megtudhatja, hogyan készíthet vizualizációt az adataiból az OMS Nézettervezőjével.</span><span class="sxs-lookup"><span data-stu-id="27691-136">You can learn how to visualize your data using OMS View Designer [here](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/).</span></span> <span data-ttu-id="27691-137">Az alábbi képernyő mintául szolgál arra, hogyan fog kinézni a csempe a testreszabás végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="27691-137">The screen below is an example of how the tile will look like once you make this customization.</span></span>

![Mintául szolgáló felhasználói felület](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

> [!NOTE]
> <span data-ttu-id="27691-139">Ha szeretné megismerni az alapkonfiguráció-értékelés során ellenőrzött beállításokat, letöltheti [ezt az Excel-táblázatot](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690), amely tartalmazza a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="27691-139">If you would like to know the settings that are checked for the baseline assessment, you can download [this Excel spreadsheet](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) that contains these settings.</span></span>

## <a name="see-also"></a><span data-ttu-id="27691-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="27691-140">See also</span></span>
<span data-ttu-id="27691-141">Ebben a dokumentumban az OMS biztonsági és auditálási megoldásának webes alapkonfiguráció-értékeléséről olvashatott.</span><span class="sxs-lookup"><span data-stu-id="27691-141">In this document, you learned about OMS Security and Audit web baseline assessment.</span></span> <span data-ttu-id="27691-142">Az OMS által nyújtott védelemmel kapcsolatos további információkat a következő cikkek tartalmaznak:</span><span class="sxs-lookup"><span data-stu-id="27691-142">To learn more about OMS Security, see the following articles:</span></span>

* [<span data-ttu-id="27691-143">Az Operations Management Suite (OMS) áttekintése</span><span class="sxs-lookup"><span data-stu-id="27691-143">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="27691-144">A biztonsági riasztások figyelése és megválaszolása az Operations Management Suite biztonsági és auditálási megoldásban</span><span class="sxs-lookup"><span data-stu-id="27691-144">Monitoring and Responding to Security Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="27691-145">Az erőforrások figyelése az Operations Management Suite biztonsági és auditálási megoldásban</span><span class="sxs-lookup"><span data-stu-id="27691-145">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

