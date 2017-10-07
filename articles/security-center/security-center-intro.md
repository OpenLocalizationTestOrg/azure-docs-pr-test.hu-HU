---
title: a Security Center aaaIntroduction tooAzure |} Microsoft Docs
description: "Információk az Azure Security Centerről, annak főbb funkcióiról és működéséről."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 287dbaaa7e2004c522f103595bc316261daf05b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-security-center"></a><span data-ttu-id="a413d-103">A Security Center bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="a413d-103">Introduction tooAzure Security Center</span></span>
<span data-ttu-id="a413d-104">Információk az Azure Security Centerről, annak főbb funkcióiról és működéséről.</span><span class="sxs-lookup"><span data-stu-id="a413d-104">Learn about Azure Security Center, its key capabilities, and how it works.</span></span>

> [!NOTE]
> <span data-ttu-id="a413d-105">Korai. június 2017 verziótól kezdve a Security Center használnak hello Microsoft Monitoring Agent toocollect, illetve adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="a413d-105">Beginning in early June 2017, Security Center will use hello Microsoft Monitoring Agent toocollect and store data.</span></span> <span data-ttu-id="a413d-106">Lásd: [Azure Security Center Platform áttelepítési](security-center-platform-migration.md) további toolearn.</span><span class="sxs-lookup"><span data-stu-id="a413d-106">See [Azure Security Center Platform Migration](security-center-platform-migration.md) toolearn more.</span></span> <span data-ttu-id="a413d-107">a cikkben szereplő információkat hello Security Center funkció átmenet toohello Microsoft Monitoring Agent után jelöli.</span><span class="sxs-lookup"><span data-stu-id="a413d-107">hello information in this article represents Security Center functionality after transition toohello Microsoft Monitoring Agent.</span></span>
>
>

## <a name="what-is-azure-security-center"></a><span data-ttu-id="a413d-108">Mi az az Azure Security Center?</span><span class="sxs-lookup"><span data-stu-id="a413d-108">What is Azure Security Center?</span></span>
 <span data-ttu-id="a413d-109">A Security Center segítséget nyújt a megakadályozása, észlelésében és kezelésében toothreats láthatóság növelésével és az Azure-erőforrások hello biztonságát vezérelheti.</span><span class="sxs-lookup"><span data-stu-id="a413d-109">Security Center helps you prevent, detect, and respond toothreats with increased visibility into and control over hello security of your Azure resources.</span></span> <span data-ttu-id="a413d-110">Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.</span><span class="sxs-lookup"><span data-stu-id="a413d-110">It provides integrated security monitoring and policy management across your Azure subscriptions, helps detect threats that might otherwise go unnoticed, and works with a broad ecosystem of security solutions.</span></span>

## <a name="key-capabilities"></a><span data-ttu-id="a413d-111">Főbb képességek</span><span class="sxs-lookup"><span data-stu-id="a413d-111">Key capabilities</span></span>
 <span data-ttu-id="a413d-112">A Security Center könnyen használható és hatékony megelőzését, felderítését és a válasz szolgáltatás képességeket nyújt tooAzure beépített.</span><span class="sxs-lookup"><span data-stu-id="a413d-112">Security Center delivers easy-to-use and effective threat prevention, detection, and response capabilities that are built in tooAzure.</span></span> <span data-ttu-id="a413d-113">A főbb funkciók a következők:</span><span class="sxs-lookup"><span data-stu-id="a413d-113">Key capabilities are:</span></span>

| <span data-ttu-id="a413d-114">Fázis</span><span class="sxs-lookup"><span data-stu-id="a413d-114">Stage</span></span> | <span data-ttu-id="a413d-115">Képesség</span><span class="sxs-lookup"><span data-stu-id="a413d-115">Capability</span></span> |
| --- | --- |
| <span data-ttu-id="a413d-116">Megelőzés</span><span class="sxs-lookup"><span data-stu-id="a413d-116">Prevent</span></span> |<span data-ttu-id="a413d-117">Figyelők hello az Azure-erőforrások biztonsági állapota</span><span class="sxs-lookup"><span data-stu-id="a413d-117">Monitors hello security state of your Azure resources</span></span> |
| <span data-ttu-id="a413d-118">Megelőzés</span><span class="sxs-lookup"><span data-stu-id="a413d-118">Prevent</span></span> | <span data-ttu-id="a413d-119">Az Azure-előfizetések a vállalat biztonsági követelményeinek, hello típusú alkalmazást használja, és hello az adatok érzékenysége alapján szabályzatokat határoz meg</span><span class="sxs-lookup"><span data-stu-id="a413d-119">Defines policies for your Azure subscriptions based on your company’s security requirements, hello types of applications that you use, and hello sensitivity of your data</span></span> |
| <span data-ttu-id="a413d-120">Megelőzés</span><span class="sxs-lookup"><span data-stu-id="a413d-120">Prevent</span></span> | <span data-ttu-id="a413d-121">A szükséges szabályozási szabályzaton alapuló biztonsági javaslatok tooguide szolgáltatások tulajdonosait hello folyamat végrehajtásán</span><span class="sxs-lookup"><span data-stu-id="a413d-121">Uses policy-driven security recommendations tooguide service owners through hello process of implementing needed controls</span></span> |
| <span data-ttu-id="a413d-122">Megelőzés</span><span class="sxs-lookup"><span data-stu-id="a413d-122">Prevent</span></span> | <span data-ttu-id="a413d-123">Gyorsan telepíti a Microsoft és a partnerei biztonsági szolgáltatásait és készülékeit</span><span class="sxs-lookup"><span data-stu-id="a413d-123">Rapidly deploys security services and appliances from Microsoft and partners</span></span> |
| <span data-ttu-id="a413d-124">Észlelés</span><span class="sxs-lookup"><span data-stu-id="a413d-124">Detect</span></span> |<span data-ttu-id="a413d-125">Automatikusan gyűjti és elemzi az Azure-erőforrások, a hello hálózati és a partneri megoldások, például kártevőirtó-programok és tűzfalak biztonsági adatait</span><span class="sxs-lookup"><span data-stu-id="a413d-125">Automatically collects and analyzes security data from your Azure resources, hello network, and partner solutions like antimalware programs and firewalls</span></span> |
| <span data-ttu-id="a413d-126">Észlelés</span><span class="sxs-lookup"><span data-stu-id="a413d-126">Detect</span></span> | <span data-ttu-id="a413d-127">Globális használja a információkat a Microsoft termékei és szolgáltatásai hello Microsoft Digital Crimes Unit (DCU), hello Microsoft biztonsági válasz Center (MSRC), és a külső hírcsatornák fenyegetés</span><span class="sxs-lookup"><span data-stu-id="a413d-127">Uses global threat intelligence from Microsoft products and services, hello Microsoft Digital Crimes Unit (DCU), hello Microsoft Security Response Center (MSRC), and external feeds</span></span> |
| <span data-ttu-id="a413d-128">Észlelés</span><span class="sxs-lookup"><span data-stu-id="a413d-128">Detect</span></span> | <span data-ttu-id="a413d-129">Bővített analitikát alkalmaz, beleértve a gépi tanulást és a viselkedéselemzést</span><span class="sxs-lookup"><span data-stu-id="a413d-129">Applies advanced analytics, including machine learning and behavioral analysis</span></span> |
| <span data-ttu-id="a413d-130">Válasz</span><span class="sxs-lookup"><span data-stu-id="a413d-130">Respond</span></span> |<span data-ttu-id="a413d-131">Rangsorolt biztonsági eseményeket/riasztásokat tartalmaz</span><span class="sxs-lookup"><span data-stu-id="a413d-131">Provides prioritized security incidents/alerts</span></span> |
| <span data-ttu-id="a413d-132">Válasz</span><span class="sxs-lookup"><span data-stu-id="a413d-132">Respond</span></span> | <span data-ttu-id="a413d-133">Hello támadás és az érintett erőforrásokról hello forrását betekintést nyújt</span><span class="sxs-lookup"><span data-stu-id="a413d-133">Offers insights into hello source of hello attack and impacted resources</span></span> |
| <span data-ttu-id="a413d-134">Válasz</span><span class="sxs-lookup"><span data-stu-id="a413d-134">Respond</span></span> | <span data-ttu-id="a413d-135">Toostop hello fennálló fenyegetés megszüntetésére és a későbbi támadások megelőzése érdekében módszereket javasol</span><span class="sxs-lookup"><span data-stu-id="a413d-135">Suggests ways toostop hello current attack and help prevent future attacks</span></span> |

## <a name="introductory-walkthrough"></a><span data-ttu-id="a413d-136">Bevezető áttekintés</span><span class="sxs-lookup"><span data-stu-id="a413d-136">Introductory walkthrough</span></span>

> [!NOTE]
> <span data-ttu-id="a413d-137">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="a413d-137">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="a413d-138">Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="a413d-138">This document is not a step-by-step guide.</span></span>
>
>

 <span data-ttu-id="a413d-139">A Security Center elérje hello [Azure-portálon](https://azure.microsoft.com/features/azure-portal/).</span><span class="sxs-lookup"><span data-stu-id="a413d-139">You access Security Center from hello [Azure portal](https://azure.microsoft.com/features/azure-portal/).</span></span> <span data-ttu-id="a413d-140">[Jelentkezzen be toohello portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a413d-140">[Sign in toohello portal](https://portal.azure.com).</span></span> <span data-ttu-id="a413d-141">Hello fő portál menüjében, görgessen a toohello **Security Center** lehetőséget, vagy válassza hello **Security Center** korábban már toohello portál Irányítópultjára rögzített csempén.</span><span class="sxs-lookup"><span data-stu-id="a413d-141">Under hello main portal menu, scroll toohello **Security Center** option or select hello **Security Center** tile that you previously pinned toohello portal dashboard.</span></span>

![Biztonság csempe az Azure Portalon][1]

<span data-ttu-id="a413d-143">A Security Centerből biztonsági szabályzatokat állíthat be, figyelheti a biztonsági konfigurációkat, és megtekintheti a biztonsági riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="a413d-143">From Security Center, you can set security policies, monitor security configurations, and view security alerts.</span></span>

### <a name="security-policies"></a><span data-ttu-id="a413d-144">Biztonsági szabályzatok</span><span class="sxs-lookup"><span data-stu-id="a413d-144">Security policies</span></span>
<span data-ttu-id="a413d-145">Az Azure-előfizetések tooyour a vállalat biztonsági követelményeinek megfelelően szabályzatokat adhat meg.</span><span class="sxs-lookup"><span data-stu-id="a413d-145">You can define policies for your Azure subscriptions according tooyour company's security requirements.</span></span> <span data-ttu-id="a413d-146">Akkor is szabhatja toohello típusú használata alkalmazások vagy hello adatok érzékenységének toohello minden előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="a413d-146">You can also tailor them toohello types of applications you're using or toohello sensitivity of hello data in each subscription.</span></span> <span data-ttu-id="a413d-147">Például különböző biztonsági követelmények vonatkozhatnak a fejlesztésben vagy tesztelésben, illetve az éles környezetben használt erőforrásokra.</span><span class="sxs-lookup"><span data-stu-id="a413d-147">For example, resources used for development or testing may have different security requirements than those used for production applications.</span></span> <span data-ttu-id="a413d-148">A szabályozott adatokkal (például személyes adatokkal) működő alkalmazások is magasabb szintű biztonságot követelhetnek meg.</span><span class="sxs-lookup"><span data-stu-id="a413d-148">Likewise, applications with regulated data like PII may require a higher level of security.</span></span>

> [!NOTE]
> <span data-ttu-id="a413d-149">a biztonsági házirend toomodify biztonsági rendszergazdai vagy hello előfizetés tulajdonosának vagy Közreműködőjének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a413d-149">toomodify a security policy, you must be a Security Administrator or hello subscription's Owner or Contributor.</span></span> <span data-ttu-id="a413d-150">További információ a szerepkörök és a Security Center engedélyezett műveletek toolearn lásd [engedélyek az Azure Security Centerben](security-center-permissions.md).</span><span class="sxs-lookup"><span data-stu-id="a413d-150">toolearn more about roles and allowed actions in Security Center, see [Permissions in Azure Security Center](security-center-permissions.md).</span></span>
>
>

<span data-ttu-id="a413d-151">A hello **Security Center** panelen, jelölje be hello **házirend** csempére az előfizetések és az erőforráscsoportok listáját.</span><span class="sxs-lookup"><span data-stu-id="a413d-151">On hello **Security Center** blade, select hello **Policy** tile for a list of your subscriptions and resource groups.</span></span>   

![Security Center panel][2]

<span data-ttu-id="a413d-153">A hello **biztonsági házirend** panelen válassza ki egy előfizetés tooview hello házirend részletei.</span><span class="sxs-lookup"><span data-stu-id="a413d-153">On hello **Security policy** blade, select a subscription tooview hello policy details.</span></span>

<span data-ttu-id="a413d-154">**Adatgyűjtés** lehetővé teszi az adatok gyűjtése az olyan biztonsági házirenddel.</span><span class="sxs-lookup"><span data-stu-id="a413d-154">**Data collection** enables data collection for a security policy.</span></span> <span data-ttu-id="a413d-155">Az engedélyezés esetén a következő műveletekre kerül sor:</span><span class="sxs-lookup"><span data-stu-id="a413d-155">Enabling provides:</span></span>

* <span data-ttu-id="a413d-156">Napi vizsgálata az összes támogatott virtuális gépek (VM) biztonsági figyelést és javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="a413d-156">Daily scanning of all supported virtual machines (VMs) for security monitoring and recommendations.</span></span>
* <span data-ttu-id="a413d-157">Biztonsági események gyűjtése elemzési célokra és a fenyegetések észlelésére.</span><span class="sxs-lookup"><span data-stu-id="a413d-157">Collection of security events for analysis and threat detection.</span></span>

> [!NOTE]
> <span data-ttu-id="a413d-158">Adatgyűjtés hello előfizetés szintjén állítható be.</span><span class="sxs-lookup"><span data-stu-id="a413d-158">Data collection is configured at hello subscription level.</span></span>
>
>

<span data-ttu-id="a413d-159">Válassza ki **megakadályozási szabályzat** tooopen hello **megakadályozási szabályzat** panelen.</span><span class="sxs-lookup"><span data-stu-id="a413d-159">Select **Prevention policy** tooopen hello **Prevention policy** blade.</span></span> <span data-ttu-id="a413d-160">**Javaslatok megjelenítése** lehetővé teszi, hogy válassza ki a megjeleníteni kívánt hello biztonsági igényeinek megfelelően hello erőforrások hello előfizetésen belül toosee toomonitor és hello javaslatok kívánt hello biztonsági vezérlőket.</span><span class="sxs-lookup"><span data-stu-id="a413d-160">**Show recommendations for** lets you choose hello security controls that you want toomonitor and hello recommendations that you want toosee based on hello security needs of hello resources within hello subscription.</span></span>

### <a name="security-recommendations"></a><span data-ttu-id="a413d-161">Biztonsági javaslatok</span><span class="sxs-lookup"><span data-stu-id="a413d-161">Security recommendations</span></span>
 <span data-ttu-id="a413d-162">A Security Center elemzi az Azure-erőforrások tooidentify potenciális biztonsági hiányosságok hello biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="a413d-162">Security Center analyzes hello security state of your Azure resources tooidentify potential security vulnerabilities.</span></span> <span data-ttu-id="a413d-163">A javaslatok listája végigvezeti hello szükséges szabályozási konfigurálásának lépésein.</span><span class="sxs-lookup"><span data-stu-id="a413d-163">A list of recommendations guides you through hello process of configuring needed controls.</span></span> <span data-ttu-id="a413d-164">Példák erre vonatkozóan:</span><span class="sxs-lookup"><span data-stu-id="a413d-164">Examples include:</span></span>

* <span data-ttu-id="a413d-165">Üzembe helyezési kártevőirtó toohelp azonosítása és eltávolítani a kártevő szoftvereket</span><span class="sxs-lookup"><span data-stu-id="a413d-165">Provisioning antimalware toohelp identify and remove malicious software</span></span>
* <span data-ttu-id="a413d-166">Hálózati biztonsági csoportok és a szabályok toocontrol forgalom tooVMs konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a413d-166">Configuring network security groups and rules toocontrol traffic tooVMs</span></span>
* <span data-ttu-id="a413d-167">A webes alkalmazások célzó támadások elleni védelemre toohelp webalkalmazási tűzfalak kiépítése</span><span class="sxs-lookup"><span data-stu-id="a413d-167">Provisioning of web application firewalls toohelp defend against attacks that target your web applications</span></span>
* <span data-ttu-id="a413d-168">Hiányzó rendszerfrissítések telepítése</span><span class="sxs-lookup"><span data-stu-id="a413d-168">Deploying missing system updates</span></span>
* <span data-ttu-id="a413d-169">Operációs rendszer azon konfigurációit, amelyek nem felelnek meg a hello címzési ajánlott alaptervek</span><span class="sxs-lookup"><span data-stu-id="a413d-169">Addressing OS configurations that do not match hello recommended baselines</span></span>

<span data-ttu-id="a413d-170">Kattintson a hello **javaslatok** csempére a javaslatok listája.</span><span class="sxs-lookup"><span data-stu-id="a413d-170">Click hello **Recommendations** tile for a list of recommendations.</span></span> <span data-ttu-id="a413d-171">Kattintson az egyes ajánlás tooview további információkat vagy tootake művelet tooresolve hello probléma.</span><span class="sxs-lookup"><span data-stu-id="a413d-171">Click each recommendation tooview additional information or tootake action tooresolve hello issue.</span></span>

![Biztonsági javaslatok az Azure Security Centerben][5]

### <a name="security-state-of-azure-resources"></a><span data-ttu-id="a413d-173">Az Azure-erőforrások biztonsági állapota</span><span class="sxs-lookup"><span data-stu-id="a413d-173">Security state of Azure resources</span></span>
<span data-ttu-id="a413d-174">Hello **megelőzési** hello irányítópult a szakasz azt mutatja be hello erőforrástípusok szerint, beleértve a virtuális gépek, webes alkalmazások és más erőforrások hello környezet általános biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="a413d-174">hello **Prevention** section of hello dashboard shows hello overall security posture of hello environment by resource type, including VMs, web applications, and other resources.</span></span>   

<span data-ttu-id="a413d-175">Válasszon ki egy erőforrástípust a **megelőzési** tooview további információt, beleértve a potenciális biztonsági hiányosságok azonosított listáját.</span><span class="sxs-lookup"><span data-stu-id="a413d-175">Select a resource type under **Prevention** tooview more information, including a list of any potential security vulnerabilities that have been identified.</span></span> <span data-ttu-id="a413d-176">(**Számítási** hello az alábbi példa az van kiválasztva.)</span><span class="sxs-lookup"><span data-stu-id="a413d-176">(**Compute** is selected in hello example below.)</span></span>

![Az Erőforrások állapota csempe][6]

### <a name="security-alerts"></a><span data-ttu-id="a413d-178">Biztonsági riasztások</span><span class="sxs-lookup"><span data-stu-id="a413d-178">Security alerts</span></span>
 <span data-ttu-id="a413d-179">A Security Center automatikusan gyűjti, elemzi és integrálja az Azure-erőforrások, a hello hálózati és a partneri megoldások, például kártevőirtó-programok és tűzfalak naplóadatait.</span><span class="sxs-lookup"><span data-stu-id="a413d-179">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, hello network, and partner solutions like antimalware programs and firewalls.</span></span> <span data-ttu-id="a413d-180">Fenyegetések észlelése esetén a központ biztonsági riasztást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a413d-180">When threats are detected, a security alert is created.</span></span> <span data-ttu-id="a413d-181">Példák fenyegetés észlelésére:</span><span class="sxs-lookup"><span data-stu-id="a413d-181">Examples include detection of:</span></span>

* <span data-ttu-id="a413d-182">Feltört virtuális gépek kommunikál az ismert kártékony IP-címek</span><span class="sxs-lookup"><span data-stu-id="a413d-182">Compromised VMs communicating with known malicious IP addresses</span></span>
* <span data-ttu-id="a413d-183">Windows hibajelentés által észlelt speciális kártevő</span><span class="sxs-lookup"><span data-stu-id="a413d-183">Advanced malware detected by using Windows error reporting</span></span>
* <span data-ttu-id="a413d-184">Virtuális gépek elleni találgatásos támadások ellen</span><span class="sxs-lookup"><span data-stu-id="a413d-184">Brute force attacks against VMs</span></span>
* <span data-ttu-id="a413d-185">Az integrált kártevőirtó programok és a tűzfalak biztonsági riasztásai</span><span class="sxs-lookup"><span data-stu-id="a413d-185">Security alerts from integrated antimalware programs and firewalls</span></span>

<span data-ttu-id="a413d-186">Gombra kattintva hello **biztonsági riasztások** csempe megjeleníti a rangsorolt riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="a413d-186">Clicking hello **Security alerts** tile displays a list of prioritized alerts.</span></span>

![Biztonsági riasztások][7]

<span data-ttu-id="a413d-188">Egy riasztás kiválasztásával további információ a hello támadás és javaslatokat arról, hogyan tooremediate azt.</span><span class="sxs-lookup"><span data-stu-id="a413d-188">Selecting an alert shows more information about hello attack and suggestions for how tooremediate it.</span></span>

![Biztonsági figyelmeztetés részletei][8]

### <a name="partner-solutions"></a><span data-ttu-id="a413d-190">Partneri megoldások</span><span class="sxs-lookup"><span data-stu-id="a413d-190">Partner solutions</span></span>
<span data-ttu-id="a413d-191">Hello **partneri megoldások** csempe segítségével figyelheti a partnermegoldások egy pillanat alatt hello biztonsági állapotát, integrálva van az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="a413d-191">hello **Partner solutions** tile lets you monitor at a glance hello security state of your partner solutions integrated with your Azure subscription.</span></span> <span data-ttu-id="a413d-192">A Security Center hello megoldások riasztásait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a413d-192">Security Center displays alerts coming from hello solutions.</span></span>

<span data-ttu-id="a413d-193">Jelölje be hello **partneri megoldások** csempére.</span><span class="sxs-lookup"><span data-stu-id="a413d-193">Select hello **Partner solutions** tile.</span></span> <span data-ttu-id="a413d-194">Ekkor megnyílik egy panel, amely a központtal összekapcsolt partnermegoldások listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a413d-194">A blade opens displaying a list of all connected partner solutions.</span></span>

![Partneri megoldások][9]

## <a name="get-started"></a><span data-ttu-id="a413d-196">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="a413d-196">Get started</span></span>
<span data-ttu-id="a413d-197">a Security Center használatába tooget, szüksége van egy előfizetési tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="a413d-197">tooget started with Security Center, you need a subscription tooMicrosoft Azure.</span></span> <span data-ttu-id="a413d-198">A Security Center engedélyezve van Azure- előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="a413d-198">Security Center is enabled with your Azure subscription.</span></span> <span data-ttu-id="a413d-199">Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a413d-199">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

 <span data-ttu-id="a413d-200">A Security Center elérje hello [Azure-portálon](https://azure.microsoft.com/features/azure-portal/).</span><span class="sxs-lookup"><span data-stu-id="a413d-200">You access Security Center from hello [Azure portal](https://azure.microsoft.com/features/azure-portal/).</span></span> <span data-ttu-id="a413d-201">Lásd: hello [portál dokumentációjában](https://azure.microsoft.com/documentation/services/azure-portal/) további toolearn.</span><span class="sxs-lookup"><span data-stu-id="a413d-201">See hello [portal documentation](https://azure.microsoft.com/documentation/services/azure-portal/) toolearn more.</span></span>

<span data-ttu-id="a413d-202">[Ismerkedés az Azure Security Center](security-center-get-started.md) gyorsan megismerheti a Security Center hello biztonsági biztonságfelügyeleti és szabályzat-kezelési összetevőit.</span><span class="sxs-lookup"><span data-stu-id="a413d-202">[Getting started with Azure Security Center](security-center-get-started.md) quickly guides you through hello security-monitoring and policy-management components of Security Center.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a413d-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a413d-203">Next steps</span></span>
<span data-ttu-id="a413d-204">Ebben a dokumentumban volt a bevezetett tooSecurity Center, annak főbb funkcióiról, és hogyan tooget.</span><span class="sxs-lookup"><span data-stu-id="a413d-204">In this document, you were introduced tooSecurity Center, its key capabilities, and how tooget started.</span></span> <span data-ttu-id="a413d-205">toolearn több, tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="a413d-205">toolearn more, see hello following resources:</span></span>

* <span data-ttu-id="a413d-206">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – további hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="a413d-206">[Setting security policies in Azure Security Center](security-center-policies.md) — Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="a413d-207">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="a413d-207">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="a413d-208">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="a413d-208">[Security health monitoring in Azure Security Center](security-center-monitoring.md) — Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="a413d-209">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – további hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="a413d-209">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) — Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="a413d-210">[Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="a413d-210">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) — Learn how toomonitor hello health status of your partner solutions.</span></span>
- <span data-ttu-id="a413d-211">[Az Azure Security Center adatainak biztonsági](security-center-data-security.md) -megtudhatja, hogyan adatok felügyelt és a Security Center védelmét.</span><span class="sxs-lookup"><span data-stu-id="a413d-211">[Azure Security Center data security](security-center-data-security.md) - Learn how data is managed and safeguarded in Security Center.</span></span>
* <span data-ttu-id="a413d-212">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a413d-212">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="a413d-213">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – hello legújabb Azure biztonsági hírek és információ lekérése.</span><span class="sxs-lookup"><span data-stu-id="a413d-213">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
