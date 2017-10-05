---
title: "Azure Security Center bemutatása |} Microsoft Docs"
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
ms.openlocfilehash: 8951167213da6ab5341c1ca420353ec625ef5424
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-azure-security-center"></a><span data-ttu-id="3a7d1-103">Az Azure Security Center bemutatása</span><span class="sxs-lookup"><span data-stu-id="3a7d1-103">Introduction to Azure Security Center</span></span>
<span data-ttu-id="3a7d1-104">Információk az Azure Security Centerről, annak főbb funkcióiról és működéséről.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-104">Learn about Azure Security Center, its key capabilities, and how it works.</span></span>

> [!NOTE]
> <span data-ttu-id="3a7d1-105">2017 júniusának elejétől kezdve a Security Center a Microsoft Monitoring Agent használatával gyűjti össze és tárolja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-105">Beginning in early June 2017, Security Center will use the Microsoft Monitoring Agent to collect and store data.</span></span> <span data-ttu-id="3a7d1-106">További információk: [Az Azure Security Center Platform migrálása](security-center-platform-migration.md).</span><span class="sxs-lookup"><span data-stu-id="3a7d1-106">See [Azure Security Center Platform Migration](security-center-platform-migration.md) to learn more.</span></span> <span data-ttu-id="3a7d1-107">A jelen cikkben található információk a Security Center a Microsoft Monitoring Agentre való váltás után elérhető funkcióit ismertetik.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-107">The information in this article represents Security Center functionality after transition to the Microsoft Monitoring Agent.</span></span>
>
>

## <a name="what-is-azure-security-center"></a><span data-ttu-id="3a7d1-108">Mi az az Azure Security Center?</span><span class="sxs-lookup"><span data-stu-id="3a7d1-108">What is Azure Security Center?</span></span>
 <span data-ttu-id="3a7d1-109">A Security Center az Azure-erőforrások biztonsági felügyeletének átláthatóbbá és szabályozhatóbbá tételével megkönnyíti a fenyegetések megelőzését, észlelését és elhárítását.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-109">Security Center helps you prevent, detect, and respond to threats with increased visibility into and control over the security of your Azure resources.</span></span> <span data-ttu-id="3a7d1-110">Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-110">It provides integrated security monitoring and policy management across your Azure subscriptions, helps detect threats that might otherwise go unnoticed, and works with a broad ecosystem of security solutions.</span></span>

## <a name="key-capabilities"></a><span data-ttu-id="3a7d1-111">Főbb funkciók</span><span class="sxs-lookup"><span data-stu-id="3a7d1-111">Key capabilities</span></span>
 <span data-ttu-id="3a7d1-112">A Security Center az Azure portál beépített funkcióiként biztosítja a fenyegetések egyszerű és hatékony megelőzését, észlelését és az azokra való reagálást.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-112">Security Center delivers easy-to-use and effective threat prevention, detection, and response capabilities that are built in to Azure.</span></span> <span data-ttu-id="3a7d1-113">A főbb funkciók a következők:</span><span class="sxs-lookup"><span data-stu-id="3a7d1-113">Key capabilities are:</span></span>

| <span data-ttu-id="3a7d1-114">Fázis</span><span class="sxs-lookup"><span data-stu-id="3a7d1-114">Stage</span></span> | <span data-ttu-id="3a7d1-115">Képesség</span><span class="sxs-lookup"><span data-stu-id="3a7d1-115">Capability</span></span> |
| --- | --- |
| <span data-ttu-id="3a7d1-116">Megelőzés</span><span class="sxs-lookup"><span data-stu-id="3a7d1-116">Prevent</span></span> |<span data-ttu-id="3a7d1-117">Az Azure-erőforrások biztonsági állapotának monitorozása</span><span class="sxs-lookup"><span data-stu-id="3a7d1-117">Monitors the security state of your Azure resources</span></span> |
| <span data-ttu-id="3a7d1-118">Megelőzés</span><span class="sxs-lookup"><span data-stu-id="3a7d1-118">Prevent</span></span> | <span data-ttu-id="3a7d1-119">Meghatározza az Azure-előfizetések alapján a vállalat biztonsági követelményeinek, milyen típusú alkalmazások, hogy használja, és az adatok érzékenysége házirendek</span><span class="sxs-lookup"><span data-stu-id="3a7d1-119">Defines policies for your Azure subscriptions based on your company’s security requirements, the types of applications that you use, and the sensitivity of your data</span></span> |
| <span data-ttu-id="3a7d1-120">Megelőzés</span><span class="sxs-lookup"><span data-stu-id="3a7d1-120">Prevent</span></span> | <span data-ttu-id="3a7d1-121">A szabályzaton alapuló biztonsági javaslatok használatával végigvezeti a szolgáltatások tulajdonosait a szükséges szabályozási folyamat végrehajtásán</span><span class="sxs-lookup"><span data-stu-id="3a7d1-121">Uses policy-driven security recommendations to guide service owners through the process of implementing needed controls</span></span> |
| <span data-ttu-id="3a7d1-122">Megelőzés</span><span class="sxs-lookup"><span data-stu-id="3a7d1-122">Prevent</span></span> | <span data-ttu-id="3a7d1-123">Gyorsan telepíti a Microsoft és a partnerei biztonsági szolgáltatásait és készülékeit</span><span class="sxs-lookup"><span data-stu-id="3a7d1-123">Rapidly deploys security services and appliances from Microsoft and partners</span></span> |
| <span data-ttu-id="3a7d1-124">Észlelés</span><span class="sxs-lookup"><span data-stu-id="3a7d1-124">Detect</span></span> |<span data-ttu-id="3a7d1-125">Automatikusan gyűjti és elemzi az Azure-erőforrások, a hálózat és a partneri megoldások, például kártevőirtó-programok és tűzfalak biztonsági adatait</span><span class="sxs-lookup"><span data-stu-id="3a7d1-125">Automatically collects and analyzes security data from your Azure resources, the network, and partner solutions like antimalware programs and firewalls</span></span> |
| <span data-ttu-id="3a7d1-126">Észlelés</span><span class="sxs-lookup"><span data-stu-id="3a7d1-126">Detect</span></span> | <span data-ttu-id="3a7d1-127">Eszközintelligencia, a Microsoft-termékek és szolgáltatások, a Microsoft Digital Crimes Unit (DCU), a Microsoft biztonsági válasz Center (MSRC), és külső hírcsatornák globális használja a fenyegetés</span><span class="sxs-lookup"><span data-stu-id="3a7d1-127">Uses global threat intelligence from Microsoft products and services, the Microsoft Digital Crimes Unit (DCU), the Microsoft Security Response Center (MSRC), and external feeds</span></span> |
| <span data-ttu-id="3a7d1-128">Észlelés</span><span class="sxs-lookup"><span data-stu-id="3a7d1-128">Detect</span></span> | <span data-ttu-id="3a7d1-129">Bővített analitikát alkalmaz, beleértve a gépi tanulást és a viselkedéselemzést</span><span class="sxs-lookup"><span data-stu-id="3a7d1-129">Applies advanced analytics, including machine learning and behavioral analysis</span></span> |
| <span data-ttu-id="3a7d1-130">Válasz</span><span class="sxs-lookup"><span data-stu-id="3a7d1-130">Respond</span></span> |<span data-ttu-id="3a7d1-131">Rangsorolt biztonsági eseményeket/riasztásokat tartalmaz</span><span class="sxs-lookup"><span data-stu-id="3a7d1-131">Provides prioritized security incidents/alerts</span></span> |
| <span data-ttu-id="3a7d1-132">Válasz</span><span class="sxs-lookup"><span data-stu-id="3a7d1-132">Respond</span></span> | <span data-ttu-id="3a7d1-133">Áttekintést nyújt a támadás forrásáról és az érintett erőforrásokról</span><span class="sxs-lookup"><span data-stu-id="3a7d1-133">Offers insights into the source of the attack and impacted resources</span></span> |
| <span data-ttu-id="3a7d1-134">Válasz</span><span class="sxs-lookup"><span data-stu-id="3a7d1-134">Respond</span></span> | <span data-ttu-id="3a7d1-135">Módszereket javasol a fennálló fenyegetés megszüntetésére és a jövőbeli fenyegetések megelőzésre</span><span class="sxs-lookup"><span data-stu-id="3a7d1-135">Suggests ways to stop the current attack and help prevent future attacks</span></span> |

## <a name="introductory-walkthrough"></a><span data-ttu-id="3a7d1-136">Bevezető áttekintés</span><span class="sxs-lookup"><span data-stu-id="3a7d1-136">Introductory walkthrough</span></span>

> [!NOTE]
> <span data-ttu-id="3a7d1-137">Ez a dokumentum egy üzembe helyezést szemléltető példa segítségével mutatja be a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-137">This document introduces the service by using an example deployment.</span></span> <span data-ttu-id="3a7d1-138">Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-138">This document is not a step-by-step guide.</span></span>
>
>

 <span data-ttu-id="3a7d1-139">A Security Center az [Azure Portalról](https://azure.microsoft.com/features/azure-portal/) érhető el.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-139">You access Security Center from the [Azure portal](https://azure.microsoft.com/features/azure-portal/).</span></span> <span data-ttu-id="3a7d1-140">[Jelentkezzen be a portálra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3a7d1-140">[Sign in to the portal](https://portal.azure.com).</span></span> <span data-ttu-id="3a7d1-141">A fő portál menüjében, görgessen a **Security Center** lehetőséget, vagy válassza ki a **Security Center** korábban már a portál Irányítópultjára rögzített csempén.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-141">Under the main portal menu, scroll to the **Security Center** option or select the **Security Center** tile that you previously pinned to the portal dashboard.</span></span>

![Biztonság csempe az Azure Portalon][1]

<span data-ttu-id="3a7d1-143">A Security Centerből biztonsági szabályzatokat állíthat be, figyelheti a biztonsági konfigurációkat, és megtekintheti a biztonsági riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-143">From Security Center, you can set security policies, monitor security configurations, and view security alerts.</span></span>

### <a name="security-policies"></a><span data-ttu-id="3a7d1-144">Biztonsági szabályzatok</span><span class="sxs-lookup"><span data-stu-id="3a7d1-144">Security policies</span></span>
<span data-ttu-id="3a7d1-145">Az Azure-előfizetések a vállalat biztonsági követelményeinek megfelelően szabályzatokat adhat meg.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-145">You can define policies for your Azure subscriptions according to your company's security requirements.</span></span> <span data-ttu-id="3a7d1-146">Ezeket testre is szabhatja az adott alkalmazás típusa vagy az egyes előfizetések adatainak érzékenysége alapján.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-146">You can also tailor them to the types of applications you're using or to the sensitivity of the data in each subscription.</span></span> <span data-ttu-id="3a7d1-147">Például különböző biztonsági követelmények vonatkozhatnak a fejlesztésben vagy tesztelésben, illetve az éles környezetben használt erőforrásokra.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-147">For example, resources used for development or testing may have different security requirements than those used for production applications.</span></span> <span data-ttu-id="3a7d1-148">A szabályozott adatokkal (például személyes adatokkal) működő alkalmazások is magasabb szintű biztonságot követelhetnek meg.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-148">Likewise, applications with regulated data like PII may require a higher level of security.</span></span>

> [!NOTE]
> <span data-ttu-id="3a7d1-149">Módosíthatja a biztonsági szabályzatot, a biztonsági rendszergazda vagy az előfizetés tulajdonosának vagy Közreműködőjének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-149">To modify a security policy, you must be a Security Administrator or the subscription's Owner or Contributor.</span></span> <span data-ttu-id="3a7d1-150">További információ a szerepkörök és engedélyezett műveletek a biztonsági központban, [engedélyek az Azure Security Centerben](security-center-permissions.md).</span><span class="sxs-lookup"><span data-stu-id="3a7d1-150">To learn more about roles and allowed actions in Security Center, see [Permissions in Azure Security Center](security-center-permissions.md).</span></span>
>
>

<span data-ttu-id="3a7d1-151">A **Security Center** panelen válassza az előfizetések és az erőforráscsoportok kívánt listájához tartozó **Szabályzat** csempét.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-151">On the **Security Center** blade, select the **Policy** tile for a list of your subscriptions and resource groups.</span></span>   

![Security Center panel][2]

<span data-ttu-id="3a7d1-153">Az a **biztonsági házirend** panelen válasszon ki egy előfizetést a szabályzat részleteinek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-153">On the **Security policy** blade, select a subscription to view the policy details.</span></span>

<span data-ttu-id="3a7d1-154">**Adatgyűjtés** lehetővé teszi az adatok gyűjtése az olyan biztonsági házirenddel.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-154">**Data collection** enables data collection for a security policy.</span></span> <span data-ttu-id="3a7d1-155">Az engedélyezés esetén a következő műveletekre kerül sor:</span><span class="sxs-lookup"><span data-stu-id="3a7d1-155">Enabling provides:</span></span>

* <span data-ttu-id="3a7d1-156">Napi vizsgálata az összes támogatott virtuális gépek (VM) biztonsági figyelést és javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-156">Daily scanning of all supported virtual machines (VMs) for security monitoring and recommendations.</span></span>
* <span data-ttu-id="3a7d1-157">Biztonsági események gyűjtése elemzési célokra és a fenyegetések észlelésére.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-157">Collection of security events for analysis and threat detection.</span></span>

> [!NOTE]
> <span data-ttu-id="3a7d1-158">Adatgyűjtés úgy van beállítva, az előfizetés szintjén.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-158">Data collection is configured at the subscription level.</span></span>
>
>

<span data-ttu-id="3a7d1-159">Válassza ki **megakadályozási szabályzat** megnyitásához a **megakadályozási szabályzat** panelen.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-159">Select **Prevention policy** to open the **Prevention policy** blade.</span></span> <span data-ttu-id="3a7d1-160">**Javaslatok megjelenítése** megadható, hogy a biztonsági funkciók, amelyek segítségével nyomon követni kívánt és a javaslatok, hogy meg szeretné tekinteni az előfizetésen belüli erőforrások biztonsági igényeinek alapján választja.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-160">**Show recommendations for** lets you choose the security controls that you want to monitor and the recommendations that you want to see based on the security needs of the resources within the subscription.</span></span>

### <a name="security-recommendations"></a><span data-ttu-id="3a7d1-161">Biztonsági javaslatok</span><span class="sxs-lookup"><span data-stu-id="3a7d1-161">Security recommendations</span></span>
 <span data-ttu-id="3a7d1-162">A Security Center a potenciális biztonsági hiányosságok azonosítása érdekében elemzi az Azure-erőforrások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-162">Security Center analyzes the security state of your Azure resources to identify potential security vulnerabilities.</span></span> <span data-ttu-id="3a7d1-163">A javaslatok listája végigvezeti Önt a szükséges szabályozási folyamatok konfigurálásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-163">A list of recommendations guides you through the process of configuring needed controls.</span></span> <span data-ttu-id="3a7d1-164">Példák erre vonatkozóan:</span><span class="sxs-lookup"><span data-stu-id="3a7d1-164">Examples include:</span></span>

* <span data-ttu-id="3a7d1-165">Kártevőirtók kiépítése a kártékony szoftverek azonosításához és eltávolításához</span><span class="sxs-lookup"><span data-stu-id="3a7d1-165">Provisioning antimalware to help identify and remove malicious software</span></span>
* <span data-ttu-id="3a7d1-166">Hálózati biztonsági csoportok és a virtuális gépek forgalmának ellenőrzésére vonatkozó szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3a7d1-166">Configuring network security groups and rules to control traffic to VMs</span></span>
* <span data-ttu-id="3a7d1-167">Webalkalmazási tűzfalak kiépítése a webalkalmazások ellen irányuló támadások elleni védelemre</span><span class="sxs-lookup"><span data-stu-id="3a7d1-167">Provisioning of web application firewalls to help defend against attacks that target your web applications</span></span>
* <span data-ttu-id="3a7d1-168">Hiányzó rendszerfrissítések telepítése</span><span class="sxs-lookup"><span data-stu-id="3a7d1-168">Deploying missing system updates</span></span>
* <span data-ttu-id="3a7d1-169">Az operációs rendszer azon konfigurációinak kezelése, amelyek nem felelnek meg a javasolt alapkonfigurációknak</span><span class="sxs-lookup"><span data-stu-id="3a7d1-169">Addressing OS configurations that do not match the recommended baselines</span></span>

<span data-ttu-id="3a7d1-170">Kattintson a **Javaslatok** csempére a javaslatok listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-170">Click the **Recommendations** tile for a list of recommendations.</span></span> <span data-ttu-id="3a7d1-171">Kattintson az egyes javaslatot a további információk megjelenítéséhez, vagy a probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-171">Click each recommendation to view additional information or to take action to resolve the issue.</span></span>

![Biztonsági javaslatok az Azure Security Centerben][5]

### <a name="security-state-of-azure-resources"></a><span data-ttu-id="3a7d1-173">Az Azure-erőforrások biztonsági állapota</span><span class="sxs-lookup"><span data-stu-id="3a7d1-173">Security state of Azure resources</span></span>
<span data-ttu-id="3a7d1-174">A **megelőzési** az irányítópult része mutatja meg erőforrástípusok szerint, a környezet általános biztonsági állapotát, beleértve a virtuális gépek, webes alkalmazások és más erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-174">The **Prevention** section of the dashboard shows the overall security posture of the environment by resource type, including VMs, web applications, and other resources.</span></span>   

<span data-ttu-id="3a7d1-175">Válasszon ki egy erőforrástípust a **megelőzési** további információk, beleértve a potenciális biztonsági hiányosságok azonosított listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-175">Select a resource type under **Prevention** to view more information, including a list of any potential security vulnerabilities that have been identified.</span></span> <span data-ttu-id="3a7d1-176">(**Számítási** az alábbi példában az van kiválasztva.)</span><span class="sxs-lookup"><span data-stu-id="3a7d1-176">(**Compute** is selected in the example below.)</span></span>

![Az Erőforrások állapota csempe][6]

### <a name="security-alerts"></a><span data-ttu-id="3a7d1-178">Biztonsági riasztások</span><span class="sxs-lookup"><span data-stu-id="3a7d1-178">Security alerts</span></span>
 <span data-ttu-id="3a7d1-179">A Security Center automatikusan gyűjti, elemzi és integrálja az Azure-erőforrások, a hálózat és az olyan partnermegoldások, mint például a kártevőirtó-programok és a tűzfalak naplóadatait.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-179">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, the network, and partner solutions like antimalware programs and firewalls.</span></span> <span data-ttu-id="3a7d1-180">Fenyegetések észlelése esetén a központ biztonsági riasztást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-180">When threats are detected, a security alert is created.</span></span> <span data-ttu-id="3a7d1-181">Példák fenyegetés észlelésére:</span><span class="sxs-lookup"><span data-stu-id="3a7d1-181">Examples include detection of:</span></span>

* <span data-ttu-id="3a7d1-182">Feltört virtuális gépek kommunikál az ismert kártékony IP-címek</span><span class="sxs-lookup"><span data-stu-id="3a7d1-182">Compromised VMs communicating with known malicious IP addresses</span></span>
* <span data-ttu-id="3a7d1-183">Windows hibajelentés által észlelt speciális kártevő</span><span class="sxs-lookup"><span data-stu-id="3a7d1-183">Advanced malware detected by using Windows error reporting</span></span>
* <span data-ttu-id="3a7d1-184">Virtuális gépek elleni találgatásos támadások ellen</span><span class="sxs-lookup"><span data-stu-id="3a7d1-184">Brute force attacks against VMs</span></span>
* <span data-ttu-id="3a7d1-185">Az integrált kártevőirtó programok és a tűzfalak biztonsági riasztásai</span><span class="sxs-lookup"><span data-stu-id="3a7d1-185">Security alerts from integrated antimalware programs and firewalls</span></span>

<span data-ttu-id="3a7d1-186">A **Biztonsági riasztások** csempére kattintva megjelenítheti a rangsorolt riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-186">Clicking the **Security alerts** tile displays a list of prioritized alerts.</span></span>

![Biztonsági riasztások][7]

<span data-ttu-id="3a7d1-188">Egy riasztás kiválasztásával további információkat tekinthet meg a támadásról, valamint javaslatokat kap a támadás elhárítására vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-188">Selecting an alert shows more information about the attack and suggestions for how to remediate it.</span></span>

![Biztonsági figyelmeztetés részletei][8]

### <a name="partner-solutions"></a><span data-ttu-id="3a7d1-190">Partneri megoldások</span><span class="sxs-lookup"><span data-stu-id="3a7d1-190">Partner solutions</span></span>
<span data-ttu-id="3a7d1-191">A **partneri megoldások** csempén egy pillanat alatt az Azure előfizetésében integrált partneri megoldások biztonsági állapotát.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-191">The **Partner solutions** tile lets you monitor at a glance the security state of your partner solutions integrated with your Azure subscription.</span></span> <span data-ttu-id="3a7d1-192">A Security Center itt a partnermegoldások riasztásait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-192">Security Center displays alerts coming from the solutions.</span></span>

<span data-ttu-id="3a7d1-193">Válassza a **Partneri megoldások** csempét.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-193">Select the **Partner solutions** tile.</span></span> <span data-ttu-id="3a7d1-194">Ekkor megnyílik egy panel, amely a központtal összekapcsolt partnermegoldások listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-194">A blade opens displaying a list of all connected partner solutions.</span></span>

![Partneri megoldások][9]

## <a name="get-started"></a><span data-ttu-id="3a7d1-196">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="3a7d1-196">Get started</span></span>
<span data-ttu-id="3a7d1-197">A kezdéshez a Security Center szüksége van a Microsoft Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-197">To get started with Security Center, you need a subscription to Microsoft Azure.</span></span> <span data-ttu-id="3a7d1-198">A Security Center engedélyezve van Azure- előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-198">Security Center is enabled with your Azure subscription.</span></span> <span data-ttu-id="3a7d1-199">Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3a7d1-199">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

 <span data-ttu-id="3a7d1-200">A Security Center az [Azure Portalról](https://azure.microsoft.com/features/azure-portal/) érhető el.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-200">You access Security Center from the [Azure portal](https://azure.microsoft.com/features/azure-portal/).</span></span> <span data-ttu-id="3a7d1-201">További információt a [portál dokumentációjában](https://azure.microsoft.com/documentation/services/azure-portal/) talál.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-201">See the [portal documentation](https://azure.microsoft.com/documentation/services/azure-portal/) to learn more.</span></span>

<span data-ttu-id="3a7d1-202">Az [Ismerkedés az Azure Security Centerrel](security-center-get-started.md) segítségével gyorsan megismerheti a Security Center biztonságfelügyeleti és szabályzat-kezelési összetevőit.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-202">[Getting started with Azure Security Center](security-center-get-started.md) quickly guides you through the security-monitoring and policy-management components of Security Center.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a7d1-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3a7d1-203">Next steps</span></span>
<span data-ttu-id="3a7d1-204">Ebben a dokumentumban megismerhette a Security Centert, annak főbb funkcióit és használatának első lépéseit.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-204">In this document, you were introduced to Security Center, its key capabilities, and how to get started.</span></span> <span data-ttu-id="3a7d1-205">További tudnivalókért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="3a7d1-205">To learn more, see the following resources:</span></span>

* <span data-ttu-id="3a7d1-206">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – útmutató az Azure-előfizetések és -erőforráscsoportok biztonsági szabályzatainak konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-206">[Setting security policies in Azure Security Center](security-center-policies.md) — Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="3a7d1-207">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-207">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="3a7d1-208">[Biztonsági állapotmonitorozás az Azure Security Centerben](security-center-monitoring.md) – Útmutató az Azure-erőforrások állapotának monitorozásához.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-208">[Security health monitoring in Azure Security Center](security-center-monitoring.md) — Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="3a7d1-209">[Kezelése és válaszadás a biztonsági riasztásokra az Azure Security Center](security-center-managing-and-responding-alerts.md) – útmutató kezelése és válaszadás a biztonsági riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-209">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) — Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="3a7d1-210">[Partneri megoldások monitorozása az Azure Security Centerrel](security-center-partner-solutions.md) – Útmutató a partneri megoldások biztonsági állapotának monitorozásához.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-210">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) — Learn how to monitor the health status of your partner solutions.</span></span>
- <span data-ttu-id="3a7d1-211">[Az Azure Security Center adatainak biztonsági](security-center-data-security.md) -megtudhatja, hogyan adatok felügyelt és a Security Center védelmét.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-211">[Azure Security Center data security](security-center-data-security.md) - Learn how data is managed and safeguarded in Security Center.</span></span>
* <span data-ttu-id="3a7d1-212">[Azure Security Center FAQ](security-center-faq.md) (Azure Security Center: Gyakran ismételt kérdések) – Válaszok a szolgáltatás használatára vonatkozó gyakori kérdésekre.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-212">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="3a7d1-213">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – a legújabb Azure biztonsági híreket és információkat.</span><span class="sxs-lookup"><span data-stu-id="3a7d1-213">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Get the latest Azure security news and information.</span></span>

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
