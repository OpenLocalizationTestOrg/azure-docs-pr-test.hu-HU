---
title: "Felügyeleti csomag biztonsági és naplózási megoldás webes alapvető aaaOperations |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan toouse OMS biztonsági és hitelesítési megoldás tooperform megfelelőségi és biztonsági célra minden figyelt webkiszolgálók webes alapkonfiguráció értékelését."
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
ms.openlocfilehash: 8aa87fa404ff97ab549dda3f9bebb75766055963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a><span data-ttu-id="9a98d-103">A webes alapkonfiguráció értékelése az Operations Management Suite biztonsági és auditálási megoldásában</span><span class="sxs-lookup"><span data-stu-id="9a98d-103">Web Baseline Assessment in Operations Management Suite Security and Audit Solution</span></span>
<span data-ttu-id="9a98d-104">Ez a dokumentum segít toouse [Operations Management Suite (OMS) biztonsági és naplózási megoldás](operations-management-suite-overview.md) webalkalmazás-alapkonfiguráció értékelése képességek tooaccess hello a figyelt erőforrások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="9a98d-104">This document helps you toouse [Operations Management Suite (OMS) Security and Audit Solution](operations-management-suite-overview.md) web baseline assessment capabilities tooaccess hello secure state of your monitored resources.</span></span>

## <a name="what-is-web-baseline-assessment"></a><span data-ttu-id="9a98d-105">Mi az a webes alapkonfiguráció-értékelés?</span><span class="sxs-lookup"><span data-stu-id="9a98d-105">What is Web Baseline Assessment?</span></span>
<span data-ttu-id="9a98d-106">Jelenleg az OMS biztonsági megoldása biztosít alapkonfiguráció-értékelést az operációs rendszerekhez.</span><span class="sxs-lookup"><span data-stu-id="9a98d-106">Currently OMS Security provides security baseline assessment for operating systems.</span></span> <span data-ttu-id="9a98d-107">Hello operációsrendszer-beállításokat a kiszolgálók 24 óránként keres, és sebezhető beállítások nézetét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9a98d-107">It scans hello operating system settings of your servers every 24 hours and provides a view into potentially vulnerable settings.</span></span> <span data-ttu-id="9a98d-108">Ezzel kapcsolatban további információkat olvashat [az alapkonfiguráció az Operations Management Suite biztonsági és auditálási megoldásában történő értékelését](oms-security-baseline.md) ismertető cikkben.</span><span class="sxs-lookup"><span data-stu-id="9a98d-108">Read [Baseline Assessment in Operations Management Suite Security and Audit Solution](oms-security-baseline.md) for more information on this.</span></span>

<span data-ttu-id="9a98d-109">hello hello webes alapkonfiguráció értékelése célja toofind sebezhető webkiszolgálói beállítások.</span><span class="sxs-lookup"><span data-stu-id="9a98d-109">hello goal of hello web baseline assessment is toofind potentially vulnerable web server settings.</span></span> <span data-ttu-id="9a98d-110">három elsődleges források hello a hello webes alapterv konfigurációk a következők: .NET, az ASP.NET és az IIS konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="9a98d-110">hello three primary sources for hello web baseline configurations are: .NET, ASP.NET, and IIS configuration.</span></span>  <span data-ttu-id="9a98d-111">Ugyanúgy, mint az operációs rendszer alapkonfiguráció értékelése hello, OMS biztonsági érintetlen tooscan a webkiszolgálón minden 24hrs és azok biztonsági állapotának áttekintése.</span><span class="sxs-lookup"><span data-stu-id="9a98d-111">Just like hello operating system baseline assessment, OMS Security is going tooscan your web servers every 24hrs and provide a view into security state of them.</span></span>  <span data-ttu-id="9a98d-112">Az Internet Information Service (IIS), konfigurációk olyan nagy mértékben testre szabható, amely lehetővé teszi, hogy a különböző helyek és alkalmazások szintek toobe felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="9a98d-112">In Internet Information Service (IIS), configurations are highly customizable, which allows various site and application levels toobe overridden.</span></span> <span data-ttu-id="9a98d-113">hello képolvasó hello beállítások hozzáadása toohello alapértelmezett gyökérszinten minden egyes alkalmazás-vagy helyvédelmi szinten ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="9a98d-113">hello scanner checks hello settings at each application/site level in addition toohello default root level.</span></span> <span data-ttu-id="9a98d-114">Ez segít tooidentify lehetséges biztonsági beállítások helyeket, és gyorsan kijavítani.</span><span class="sxs-lookup"><span data-stu-id="9a98d-114">This helps you tooidentify potential vulnerability settings locations and quickly remediate.</span></span>


## <a name="web-security-baseline-assessment"></a><span data-ttu-id="9a98d-115">A webes biztonsági alapkonfiguráció értékelése</span><span class="sxs-lookup"><span data-stu-id="9a98d-115">Web Security Baseline Assessment</span></span>
<span data-ttu-id="9a98d-116">Ez a szolgáltatás lesz az előzetes verzió toobe hello OMS keresési lehetőség használatával érhető el.</span><span class="sxs-lookup"><span data-stu-id="9a98d-116">For this preview this feature is going toobe accessed using hello OMS Search option.</span></span> <span data-ttu-id="9a98d-117">Kövesse az alábbi tooperform sajátíthatja hello lekérdezés hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9a98d-117">Follow hello steps below tooperform hello appropriated query:</span></span>

1. <span data-ttu-id="9a98d-118">A hello **a Microsoft Operations Management Suite** fő irányítópultján kattintson **biztonsági és naplózási** csempére.</span><span class="sxs-lookup"><span data-stu-id="9a98d-118">In hello **Microsoft Operations Management Suite** main dashboard click **Security and Audit** tile.</span></span>
2. <span data-ttu-id="9a98d-119">A hello **biztonsági és naplózási** irányítópultján kattintson **naplófájl-keresési** gombra.</span><span class="sxs-lookup"><span data-stu-id="9a98d-119">In hello **Security and Audit** dashboard, click **Log Search** button.</span></span>
3. <span data-ttu-id="9a98d-120">hello első lekérdezés használható hello **webes alapterv felmérésének összegzése**.</span><span class="sxs-lookup"><span data-stu-id="9a98d-120">hello first query that you can use is hello **Web Baseline Assessment Summary**.</span></span> <span data-ttu-id="9a98d-121">A hello **Begin Keresés itt** mezőbe írja be a lekérdezést: típus*SecurityBaselineSummary BaselineType = webes =*.</span><span class="sxs-lookup"><span data-stu-id="9a98d-121">In hello **Begin search here** field, type this query: Type*=SecurityBaselineSummary BaselineType=web*.</span></span> <span data-ttu-id="9a98d-122">a következő képernyő hello egy kimeneti minta rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="9a98d-122">hello following screen has an output sample:</span></span>

![Webes alapkonfiguráció-értékelés összegzése](./media/oms-security-web-baseline/oms-security-web-baseline-fig1-new.png)

> [!NOTE]
> <span data-ttu-id="9a98d-124">Ebben a lekérdezésben minden rekord egy kiszolgáló értékelési összegzését jelöli.</span><span class="sxs-lookup"><span data-stu-id="9a98d-124">In this query, each record indicates assessment summary on a single server.</span></span>

<span data-ttu-id="9a98d-125">Miután belépett hello **naplófájl-keresési**, beírhatja különböző lekérdezéseket tooobtain hello webes alapkonfiguráció értékelése további információt.</span><span class="sxs-lookup"><span data-stu-id="9a98d-125">Once you are in hello **Log Search**, you can type different queries tooobtain more information about hello web baseline assessment.</span></span> <span data-ttu-id="9a98d-126">Továbbá toohello előző lekérdezést, használhatja a következő azokat, ebben az előzetes verzióban hello.</span><span class="sxs-lookup"><span data-stu-id="9a98d-126">In addition toohello previous query, you can also use hello following ones in this preview.</span></span>

<span data-ttu-id="9a98d-127">**Webes alapkonfigurációsszabály-értékelés**: Minden rekord egy kiszolgáló webes alapkonfigurációsszabály-értékelését jelöli.</span><span class="sxs-lookup"><span data-stu-id="9a98d-127">**Web Baseline Rule Assessment**: Each record represents a single web baseline rule evaluation on a single server.</span></span> <span data-ttu-id="9a98d-128">Ez magában foglalja a hello szabály, hely, hello várt és tényleges értékét hello összes adatát.</span><span class="sxs-lookup"><span data-stu-id="9a98d-128">It includes all data for hello rule, location, hello expected result, and hello actual result.</span></span>

<span data-ttu-id="9a98d-129">**Lekérdezés**: Type*=SecurityBaseline BaselineType=web*</span><span class="sxs-lookup"><span data-stu-id="9a98d-129">**Query**: Type*=SecurityBaseline BaselineType=web*</span></span>

![Webes alapkonfigurációsszabály-értékelés](./media/oms-security-web-baseline/oms-security-web-baseline-fig2.png)

<span data-ttu-id="9a98d-131">**Egy adott kiszolgálóhoz tartozó összes találat megjelenítése**: Ez a lekérdezés bemutatja, hogyan toosee eredménye a megadott kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="9a98d-131">**Show all results for a specific server**: This query shows how toosee results of a specific server.</span></span>

<span data-ttu-id="9a98d-132">**Lekérdezés**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*</span><span class="sxs-lookup"><span data-stu-id="9a98d-132">**Query**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*</span></span>

![Minden eredmény](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

<span data-ttu-id="9a98d-134">Ezen rekordok/lekérdezések toocreate használhatja saját irányítópultokat, jelentéseket és riasztásokat is.</span><span class="sxs-lookup"><span data-stu-id="9a98d-134">You can also use these records/queries toocreate your own dashboards, reports, or alerts.</span></span> <span data-ttu-id="9a98d-135">az alábbi üdvözlő képernyőt rendelkezik egy minta felhasználói felületének vezérlői tooyour irányítópult adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="9a98d-135">hello screen below has a sample UI control that you can add tooyour dashboard.</span></span> <span data-ttu-id="9a98d-136">Azt is megtudhatja, hogyan toovisualize OMS Nézettervező használata esetén az adatok [Itt](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/).</span><span class="sxs-lookup"><span data-stu-id="9a98d-136">You can learn how toovisualize your data using OMS View Designer [here](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/).</span></span> <span data-ttu-id="9a98d-137">az alábbi üdvözlő képernyőt például hogyan hello csempe fog megjelenni a testreszabás után.</span><span class="sxs-lookup"><span data-stu-id="9a98d-137">hello screen below is an example of how hello tile will look like once you make this customization.</span></span>

![Mintául szolgáló felhasználói felület](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

> [!NOTE]
> <span data-ttu-id="9a98d-139">Ha azt szeretné tooknow hello beállításokat ellenőrzi hello alapkonfiguráció értékelése, letöltheti [az Excel-táblázat](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) , amely tartalmazza ezeket a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="9a98d-139">If you would like tooknow hello settings that are checked for hello baseline assessment, you can download [this Excel spreadsheet](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) that contains these settings.</span></span>

## <a name="see-also"></a><span data-ttu-id="9a98d-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="9a98d-140">See also</span></span>
<span data-ttu-id="9a98d-141">Ebben a dokumentumban az OMS biztonsági és auditálási megoldásának webes alapkonfiguráció-értékeléséről olvashatott.</span><span class="sxs-lookup"><span data-stu-id="9a98d-141">In this document, you learned about OMS Security and Audit web baseline assessment.</span></span> <span data-ttu-id="9a98d-142">További információ az OMS biztonsági toolearn tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="9a98d-142">toolearn more about OMS Security, see hello following articles:</span></span>

* [<span data-ttu-id="9a98d-143">Az Operations Management Suite (OMS) áttekintése</span><span class="sxs-lookup"><span data-stu-id="9a98d-143">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="9a98d-144">Figyelés és a válaszoló tooSecurity riasztásait az Operations Management Suite biztonsági és naplózási megoldás</span><span class="sxs-lookup"><span data-stu-id="9a98d-144">Monitoring and Responding tooSecurity Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="9a98d-145">Az erőforrások figyelése az Operations Management Suite biztonsági és auditálási megoldásban</span><span class="sxs-lookup"><span data-stu-id="9a98d-145">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

