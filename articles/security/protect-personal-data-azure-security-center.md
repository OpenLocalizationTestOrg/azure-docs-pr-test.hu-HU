---
title: "Az Azure Security Center személyes adatok védelme |} Microsoft Docs"
description: "az Azure security Centerben személyes adatok védelme"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 3a941389713a4d3dbffbbfe8a717409927d85c6d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a><span data-ttu-id="0802e-103">Személyes adatok védelmét a problémák és támadások: az Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="0802e-103">Protect personal data from breaches and attacks: Azure Security Center</span></span>

<span data-ttu-id="0802e-104">Ez a cikk segít megvédeni a személyes adatokat a problémák és támadások az Azure Security Center használatának megismerése.</span><span class="sxs-lookup"><span data-stu-id="0802e-104">This article will help you understand how to use Azure Security Center to protect personal data from breaches and attacks.</span></span>

## <a name="scenario"></a><span data-ttu-id="0802e-105">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="0802e-105">Scenario</span></span> 

<span data-ttu-id="0802e-106">Egy nagy körutazás cég, az Amerikai Egyesült Államokban telephelyének bővíti a mediterrán és Balti tengerek, valamint a Brit-szigetekre útvonalak biztosítani a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="0802e-106">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="0802e-107">Az adott erőfeszítéseket érdekében több kisebb körutazás sorok Olaszország, német, Dánia és az Egyesült szerzett</span><span class="sxs-lookup"><span data-stu-id="0802e-107">To help in those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark, and the U.K.</span></span>

<span data-ttu-id="0802e-108">A vállalat a Microsoft Azure használ a felhőben tárolt vállalati adatokat.</span><span class="sxs-lookup"><span data-stu-id="0802e-108">The company uses Microsoft Azure to store corporate data in the cloud.</span></span> <span data-ttu-id="0802e-109">Ez magában foglalja a személyes azonosításra alkalmas adatok, például a nevét, a címét, a telefonszámokat és a hitelkártya adatait.</span><span class="sxs-lookup"><span data-stu-id="0802e-109">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information.</span></span> <span data-ttu-id="0802e-110">Az adatokat is tartalmaz az emberi erőforrások, mint:</span><span class="sxs-lookup"><span data-stu-id="0802e-110">It also includes Human Resources information such as:</span></span>

- <span data-ttu-id="0802e-111">Címek</span><span class="sxs-lookup"><span data-stu-id="0802e-111">Addresses</span></span>
- <span data-ttu-id="0802e-112">telefonszámok</span><span class="sxs-lookup"><span data-stu-id="0802e-112">Phone numbers</span></span>
- <span data-ttu-id="0802e-113">azonosító számokat</span><span class="sxs-lookup"><span data-stu-id="0802e-113">Tax identification numbers</span></span>
- <span data-ttu-id="0802e-114">orvosi adatok</span><span class="sxs-lookup"><span data-stu-id="0802e-114">Medical information</span></span>

<span data-ttu-id="0802e-115">A körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagjainak.</span><span class="sxs-lookup"><span data-stu-id="0802e-115">The cruise line also maintains a large database of reward and loyalty program members.</span></span> <span data-ttu-id="0802e-116">Vállalati alkalmazottak hozzáférjenek a hálózathoz, a távoli iroda a vállalat, és a világ utazás ügynökök rendelkezésre elérni egyes vállalati erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="0802e-116">Corporate employees access the network from the company’s remote offices and travel agents located around the world have access to some company resources.</span></span>
<span data-ttu-id="0802e-117">Személyes adatokat a hálózaton keresztül halad, ezeket a helyeket és a Microsoft-adatközpont között.</span><span class="sxs-lookup"><span data-stu-id="0802e-117">Personal data travels across the network between these locations and the Microsoft data center.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="0802e-118">Probléma leírása</span><span class="sxs-lookup"><span data-stu-id="0802e-118">Problem statement</span></span>

<span data-ttu-id="0802e-119">A vállalat aggódik az Azure-erőforrások elleni támadások veszélye.</span><span class="sxs-lookup"><span data-stu-id="0802e-119">The company is concerned about the threat of attacks on their Azure resources.</span></span> <span data-ttu-id="0802e-120">Szeretnék ügyfelek és az alkalmazottak a személyes adatok veszélyeknek való kitettség megelőzése jogosulatlan személyek számára.</span><span class="sxs-lookup"><span data-stu-id="0802e-120">They want to prevent exposure of customers’ and employees’ personal data to unauthorized persons.</span></span> <span data-ttu-id="0802e-121">Szeretnék útmutatást is megelőzése és a válasz/szervizelés, valamint a felhőalapú erőforrások a biztonság folyamatos figyelését hatékony módot.</span><span class="sxs-lookup"><span data-stu-id="0802e-121">They want guidance on both prevention and response/remediation, as well as an effective way to monitor the ongoing security of their cloud resources.</span></span>
<span data-ttu-id="0802e-122">Egy erős elleni védelmi vonalat mai valamint támadók van szükségük.</span><span class="sxs-lookup"><span data-stu-id="0802e-122">They need a strong line of defense against today’s sophisticated and organized attackers.</span></span>

## <a name="company-goal"></a><span data-ttu-id="0802e-123">Vállalati cél</span><span class="sxs-lookup"><span data-stu-id="0802e-123">Company goal</span></span>

<span data-ttu-id="0802e-124">A vállalati célok egyik fenyegetésekkel szembeni hatékony megvédi az ügyfelek és az alkalmazottak a személyes adatok biztonsága érdekében.</span><span class="sxs-lookup"><span data-stu-id="0802e-124">One of the company’s goals is to ensure the privacy of customers’ and employees’ personal data by protecting it from threats.</span></span> <span data-ttu-id="0802e-125">A cél egyik azonnal reagálni a biztonsági szabályok megsértésére jeleit csökkenthető az káros hatása.</span><span class="sxs-lookup"><span data-stu-id="0802e-125">One of their goals is to respond immediately to signs of breach to mitigate the impact.</span></span> <span data-ttu-id="0802e-126">Biztonsági aktuális állapotát, sebezhető felismerésében és elháríthatja a úgy van szüksége.</span><span class="sxs-lookup"><span data-stu-id="0802e-126">It requires a way to assess the current state of security, identify vulnerable configurations, and remediate them.</span></span>

## <a name="solutions"></a><span data-ttu-id="0802e-127">Megoldások</span><span class="sxs-lookup"><span data-stu-id="0802e-127">Solutions</span></span>

<span data-ttu-id="0802e-128">A Microsoft Azure Security Center (ASC) az integrált biztonsági monitorozást és felügyeleti megoldást nyújt.</span><span class="sxs-lookup"><span data-stu-id="0802e-128">Microsoft Azure Security Center (ASC) provides an integrated security monitoring and policy management solution.</span></span> <span data-ttu-id="0802e-129">Könnyen használható és hatékony megelőzését, észlelését és ellenállási képességeket kínál biztosítja.</span><span class="sxs-lookup"><span data-stu-id="0802e-129">It delivers easy-to-use and effective threat prevention, detection, and response capabilities.</span></span>

### <a name="prevention"></a><span data-ttu-id="0802e-130">Megelőzés</span><span class="sxs-lookup"><span data-stu-id="0802e-130">Prevention</span></span>

<span data-ttu-id="0802e-131">ASC segít megelőzni, hogy a megsértése azáltal, hogy a biztonsági házirendek beállítása, közvetlenül az időponthoz kötött hozzáférést és biztonsági ajánlások megvalósításával.</span><span class="sxs-lookup"><span data-stu-id="0802e-131">ASC helps you prevent breaches by enabling you to set security policies, provide just-in-time access, and implement security recommendations.</span></span>

<span data-ttu-id="0802e-132">Egy biztonsági házirend határozza meg az adott előfizetésen belüli erőforrásokon ajánlott meg.</span><span class="sxs-lookup"><span data-stu-id="0802e-132">A security policy defines the set of controls recommended for resources within the specified subscription.</span></span> <span data-ttu-id="0802e-133">Csak a időben való hozzáférésre is használható az Azure virtuális gépeken, támadásoknak való kitettség csökkentése bejövő forgalmát zárolását.</span><span class="sxs-lookup"><span data-stu-id="0802e-133">Just in time access can be used to lock down inbound traffic to your Azure VMs, reducing exposure to attacks.</span></span> <span data-ttu-id="0802e-134">Biztonsági javaslatok az Azure-erőforrások biztonsági állapotát elemzése után ASC hozza létre.</span><span class="sxs-lookup"><span data-stu-id="0802e-134">Security recommendations are created by ASC after analyzing the security state of your Azure resources.</span></span>

#### <a name="how-do-i-set-security-policies-in-asc"></a><span data-ttu-id="0802e-135">Biztonsági házirendek beállítása a ASC</span><span class="sxs-lookup"><span data-stu-id="0802e-135">How do I set security policies in ASC?</span></span>

<span data-ttu-id="0802e-136">Az egyes előfizetésekhez külön-külön biztonsági szabályzatot állíthat be.</span><span class="sxs-lookup"><span data-stu-id="0802e-136">You can configure security policies for each subscription.</span></span> <span data-ttu-id="0802e-137">Biztonsági szabályzat módosításához az előfizetésben tulajdonos vagy közreműködő szerepkörrel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="0802e-137">To modify a security policy, you must be an owner or contributor of that subscription.</span></span> <span data-ttu-id="0802e-138">Az Azure portálon tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0802e-138">In the Azure portal, do the following:</span></span>

1. <span data-ttu-id="0802e-139">Válassza ki **házirend** ASC irányítópultján.</span><span class="sxs-lookup"><span data-stu-id="0802e-139">Select **Policy** in the ASC dashboard.</span></span>

2. <span data-ttu-id="0802e-140">Válassza ki az előfizetést, amely engedélyezi a házirendet kívánja.</span><span class="sxs-lookup"><span data-stu-id="0802e-140">Select the subscription on which you want to enable the policy.</span></span>

3. <span data-ttu-id="0802e-141">Válasszon **megakadályozási szabályzat** előfizetésenként házirendek konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="0802e-141">Choose **Prevention policy** to configure policies per subscription.</span></span> <span data-ttu-id="0802e-142">**Adatgyűjtés a virtuális gépek** meg **meg.**</span><span class="sxs-lookup"><span data-stu-id="0802e-142">**Collect data from virtual machines** should be set to **On.**</span></span>

4. <span data-ttu-id="0802e-143">Az a **megakadályozási szabályzat** beállításnál válassza **a** a biztonsági javaslatok láthatók, amelyek kapcsolódnak az előfizetés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0802e-143">In the **Prevention policy** options, select **On** to enable the security recommendations that are relevant for the subscription.</span></span>

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

<span data-ttu-id="0802e-144">Részletesebb utasításokat és engedélyezhető házirend ajánlások előzetesben, lásd: [biztonsági házirendek beállítása az Azure Security Centerben](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).</span><span class="sxs-lookup"><span data-stu-id="0802e-144">For more detailed instructions and an explanation of each of the policy recommendations that can be enabled, see [Set security policies in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).</span></span>

#### <a name="how-do-i-configure-just-in-time-access-jit"></a><span data-ttu-id="0802e-145">Hogyan konfigurálhatók a csak az idő Access (JIT)?</span><span class="sxs-lookup"><span data-stu-id="0802e-145">How do I configure Just in Time Access (JIT)?</span></span>

<span data-ttu-id="0802e-146">Igény szerinti engedélyezésekor a rendszer a Security Center zárolja az Azure virtuális gépek bejövő forgalmát az NSG-szabály által.</span><span class="sxs-lookup"><span data-stu-id="0802e-146">When JIT is enabled, Security Center locks down inbound traffic to your Azure VMs by creating an NSG rule.</span></span> <span data-ttu-id="0802e-147">Kiválaszthatja a a virtuális Gépre, amelyre a bejövő forgalom lesz zárolva.</span><span class="sxs-lookup"><span data-stu-id="0802e-147">You select the ports on the VM to which inbound traffic will be locked down.</span></span> <span data-ttu-id="0802e-148">Igény szerinti hozzáférés használatára, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0802e-148">To use JIT access, do the following:</span></span>

1. <span data-ttu-id="0802e-149">Válassza ki a **idő VM hozzáférés csempén csak** a ASC panelen.</span><span class="sxs-lookup"><span data-stu-id="0802e-149">Select the **Just in time VM access tile** on the ASC blade.</span></span>

2. <span data-ttu-id="0802e-150">Válassza ki a **ajánlott** fülre.</span><span class="sxs-lookup"><span data-stu-id="0802e-150">Select the **Recommended** tab.</span></span>

3. <span data-ttu-id="0802e-151">A **virtuális gépek**, válassza ki az engedélyezni kívánt virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="0802e-151">Under **VMs**, select the VMs that you want to enable.</span></span> <span data-ttu-id="0802e-152">Ez helyezi a virtuális gépek melletti négyzetet.</span><span class="sxs-lookup"><span data-stu-id="0802e-152">This puts a checkmark next to a VM.</span></span> 
4. <span data-ttu-id="0802e-153">Válassza ki **JIT engedélyezése** virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="0802e-153">Select **Enable JIT** on VMs.</span></span>
5. <span data-ttu-id="0802e-154">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="0802e-154">Select **Save**.</span></span>

<span data-ttu-id="0802e-155">Láthatja majd ASC javasolja a rendszer engedélyezi az igény szerinti alapértelmezett portokat.</span><span class="sxs-lookup"><span data-stu-id="0802e-155">Then you can see the default ports that ASC recommends being enabled for JIT.</span></span> <span data-ttu-id="0802e-156">Is hozzáadhat és engedélyezése a csak akkor szeretne új port konfigurálása idő megoldásban.</span><span class="sxs-lookup"><span data-stu-id="0802e-156">You can also add and configure a new port on which you want to enable the just in time solution.</span></span> <span data-ttu-id="0802e-157">A **közvetlenül a virtuális gép elérhető** csempe a Security Center az igény szerinti hozzáférés konfigurált virtuális gépek számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="0802e-157">The **Just in time VM access** tile in the Security Center shows the number of VMs configured for JIT access.</span></span> <span data-ttu-id="0802e-158">Azt is bemutatja a múlt héten engedélyezett hozzáférési kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="0802e-158">It also shows the number of approved access requests made in the last week.</span></span>

![](media/protect-personal-data-azure-security-center/jit-vm.png)

<span data-ttu-id="0802e-159">Ehhez útmutatást és az elérhető legyen, csak további információt lásd: [JIT-virtuális gép hozzáférés kezelése.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)</span><span class="sxs-lookup"><span data-stu-id="0802e-159">For instructions on how to do this, and additional information about Just in Time access, see [Manage virtual machine access using just in time.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)</span></span>

#### <a name="how-do-i-implement-asc-security-recommendations"></a><span data-ttu-id="0802e-160">Hogyan valósítja meg a biztonsági javaslatok ASC?</span><span class="sxs-lookup"><span data-stu-id="0802e-160">How do I implement ASC security recommendations?</span></span>

<span data-ttu-id="0802e-161">A Security Center javaslatokat hoz létre, amikor lehetséges biztonsági réseket észlel.</span><span class="sxs-lookup"><span data-stu-id="0802e-161">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="0802e-162">A javaslatok végigvezetik Önt a szükséges vezérlők konfigurálásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="0802e-162">The recommendations guide you through the process of configuring the needed controls.</span></span> 
1. <span data-ttu-id="0802e-163">Válassza ki a **javaslatok** a ASC irányítópult csempét.</span><span class="sxs-lookup"><span data-stu-id="0802e-163">Select the **Recommendations** tile on the ASC dashboard.</span></span>

2. <span data-ttu-id="0802e-164">Tekintse meg a javaslatok, amelyben soronként jelöli egy javaslat táblázatos formában látható.</span><span class="sxs-lookup"><span data-stu-id="0802e-164">View the recommendations, which are shown in a table format where each line represents one recommendation.</span></span>

3. <span data-ttu-id="0802e-165">Szűrő ajánlásokat, jelölje be a **szűrő** és válassza ki a súlyosság és állapotának értékeket látni szeretné.</span><span class="sxs-lookup"><span data-stu-id="0802e-165">To filter recommendations, select **Filter** and select the severity and state values you wish to see.</span></span>

4. <span data-ttu-id="0802e-166">Azt ajánljuk, hogy nincs megfelelő elvetéséhez kattintson a jobb gombbal és válassza ki **leállítási.**</span><span class="sxs-lookup"><span data-stu-id="0802e-166">To dismiss a recommendation that is not applicable, you can right click and select **Dismiss.**</span></span>

5. <span data-ttu-id="0802e-167">Értékelje ki, melyik javaslat előbb alkalmazni kell.</span><span class="sxs-lookup"><span data-stu-id="0802e-167">Evaluate which recommendation should be applied first.</span></span>

6. <span data-ttu-id="0802e-168">A javaslatok a prioritásuk szerinti sorrendben alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="0802e-168">Apply the recommendations in order of priority.</span></span>

<span data-ttu-id="0802e-169">Lehetséges ajánlásokat, és hogyan kell alkalmazni az egyes útmutatók listájáért lásd: [biztonsági javaslatok kezelése az Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)</span><span class="sxs-lookup"><span data-stu-id="0802e-169">For a list of possible recommendations and walk-throughs on how to apply each, see [Managing security recommendations in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)</span></span>

### <a name="detection-and-response"></a><span data-ttu-id="0802e-170">Felderítését és a válasz</span><span class="sxs-lookup"><span data-stu-id="0802e-170">Detection and Response</span></span>

<span data-ttu-id="0802e-171">Felderítését és a válasz Ugrás együtt, ahányat csak szeretne válaszolni minél gyorsabban után fenyegetést észlel.</span><span class="sxs-lookup"><span data-stu-id="0802e-171">Detection and response go together, as you want to respond as quickly as possible after a threat is detected.</span></span>
<span data-ttu-id="0802e-172">ASC fenyegetésészlelés működik, automatikusan az Azure-erőforrások, a hálózati és az összekapcsolt partnermegoldások a biztonsági információk begyűjtése.</span><span class="sxs-lookup"><span data-stu-id="0802e-172">ASC threat detection works by automatically collecting security information from your Azure resources, the network, and connected partner solutions.</span></span> <span data-ttu-id="0802e-173">ASC is gyorsan frissíti az észlelési algoritmusokat támadók kiadás új és egyre kifinomult biztonsági réseket.</span><span class="sxs-lookup"><span data-stu-id="0802e-173">ASC can rapidly update its detection algorithms as attackers release new and increasingly sophisticated exploits.</span></span> <span data-ttu-id="0802e-174">További információk a ASC tartozó fenyegetésészlelés működése, a következő témakörben: [az Azure Security Center az észlelési képességek](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).</span><span class="sxs-lookup"><span data-stu-id="0802e-174">For more detailed information on how ASC’s threat detection works, see [Azure Security Center detection capabilities](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).</span></span>

#### <a name="how-do-i-manage-and-respond-to-security-alerts"></a><span data-ttu-id="0802e-175">Hogyan kezelése és válaszadás a biztonsági riasztásokra?</span><span class="sxs-lookup"><span data-stu-id="0802e-175">How do I manage and respond to security alerts?</span></span>

<span data-ttu-id="0802e-176">A Security Center szüksége gyorsan vizsgálja meg a problémát az információk mellett jelenik meg a rangsorolt biztonsági riasztások listája.</span><span class="sxs-lookup"><span data-stu-id="0802e-176">A list of prioritized security alerts is shown in Security Center along with the information you need to quickly investigate the problem.</span></span> <span data-ttu-id="0802e-177">A Security Center emellett a támadás elhárítására vonatkozóan javaslatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0802e-177">Security Center also includes recommendations for how to remediate an attack.</span></span> <span data-ttu-id="0802e-178">A biztonsági riasztások kezelése, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0802e-178">To manage your security alerts, do the following:</span></span>

1. <span data-ttu-id="0802e-179">Válassza ki a **biztonsági riasztások** ASC irányítópultján csempére.</span><span class="sxs-lookup"><span data-stu-id="0802e-179">Select the **Security alerts** tile in the ASC dashboard.</span></span> <span data-ttu-id="0802e-180">Ez azt jelenti, az egyes riasztások részletei.</span><span class="sxs-lookup"><span data-stu-id="0802e-180">This shows details for each alert.</span></span>

2. <span data-ttu-id="0802e-181">A riasztások dátum, állapot és súlyosság alapján szűrheti, jelölje be **szűrő** , és válassza ki a megjeleníteni kívánt értékeket.</span><span class="sxs-lookup"><span data-stu-id="0802e-181">To filter alerts based on date, state, and severity, select **Filter** and then select the values you want to see.</span></span>

3. <span data-ttu-id="0802e-182">Riasztást válaszolni, válassza ki azt, tekintse át az információkat, majd válassza ki a támadásnak kitett erőforrásra erőforrást.</span><span class="sxs-lookup"><span data-stu-id="0802e-182">To respond to an alert, select it and review the information, then select the resource that was attacked.</span></span>

4. <span data-ttu-id="0802e-183">Az a **leírás** mezőben adatait, többek között az ajánlott javítási láthatja.</span><span class="sxs-lookup"><span data-stu-id="0802e-183">In the **Description** field, you’ll see details, including recommended remediation.</span></span>

![](media/protect-personal-data-azure-security-center/security-alerts.png)

<span data-ttu-id="0802e-184">Részletes utasítások a biztonsági riasztásokra való reagálásról, lásd: [kezelése és a biztonsági riasztásokra az Azure Security Center válaszol.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)</span><span class="sxs-lookup"><span data-stu-id="0802e-184">For more detailed instructions on responding to security alerts, see [Managing and responding to security alerts in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)</span></span>

<span data-ttu-id="0802e-185">A biztonsági riasztások kivizsgálásának további segítséget a vállalati ASC riasztások integrálhatja a saját SIEM-megoldás használatával [Azure napló integrációs](https://aka.ms/AzLog).</span><span class="sxs-lookup"><span data-stu-id="0802e-185">For further help in investigating security alerts, the company can integrate ASC alerts with its own SIEM solution, using [Azure Log Integration](https://aka.ms/AzLog).</span></span>

#### <a name="how-do-i-manage-security-incidents"></a><span data-ttu-id="0802e-186">Hogyan kezelheti a biztonsági események?</span><span class="sxs-lookup"><span data-stu-id="0802e-186">How do I manage security incidents?</span></span>

<span data-ttu-id="0802e-187">A ASC egy biztonsági incidens az összes riasztás erőforrás, amely megfelel-e a kill lánc minták összesítést.</span><span class="sxs-lookup"><span data-stu-id="0802e-187">In ASC, a security incident is an aggregation of all alerts for a resource that align with kill chain patterns.</span></span> <span data-ttu-id="0802e-188">Az incidensek megnyitásakor megjelenik a kapcsolódó riasztások listája, amelyből további információkat kaphat az egyes riasztásokról.</span><span class="sxs-lookup"><span data-stu-id="0802e-188">An Incident will reveal the list of related alerts, which enables you to obtain more information about each occurrence.</span></span> <span data-ttu-id="0802e-189">Incidensek a biztonsági riasztások csempén és panelen jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="0802e-189">Incidents appear in the Security Alerts tile and blade.</span></span>

<span data-ttu-id="0802e-190">Tekintse át, és biztonsági incidensek kezeléséhez, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0802e-190">To review and manage security incidents, do the following:</span></span>

1. <span data-ttu-id="0802e-191">Válassza ki a **biztonsági riasztások** csempére.</span><span class="sxs-lookup"><span data-stu-id="0802e-191">Select the **Security alerts** tile.</span></span> <span data-ttu-id="0802e-192">Amennyiben egy biztonsági incidens észlel, akkor jelenik meg a biztonsági riasztások graph alatt.</span><span class="sxs-lookup"><span data-stu-id="0802e-192">if a security incident is detected, it will appear under the security alerts graph.</span></span> <span data-ttu-id="0802e-193">Egy ikont, amely eltér a más riasztások lesz.</span><span class="sxs-lookup"><span data-stu-id="0802e-193">It will have an icon that’s different from other alerts.</span></span>

2. <span data-ttu-id="0802e-194">Válassza ki az incidenst, hogy a biztonsági incidens kapcsolatos további részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="0802e-194">Select the incident to see more details about this security incident.</span></span> <span data-ttu-id="0802e-195">További részletek közé tartoznak a teljes leírását a súlyosság, a jelenlegi állapotában, a megtámadott erőforrások, az incidensek és a riasztások foglalt ezt az incidenst a javítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="0802e-195">Additional details include its full description, its severity, its current state, the attacked resource, the remediation steps for the incident, and the alerts that were included in this incident.</span></span>

<span data-ttu-id="0802e-196">Szűrheti, hogy **incidensek csak**, **riasztásokat csak**, vagy **mindkét**.</span><span class="sxs-lookup"><span data-stu-id="0802e-196">You can filter to see **incidents only**, **alerts only**, or **both**.</span></span>

#### <a name="how-do-i-access-the-threat-intelligence-report"></a><span data-ttu-id="0802e-197">Hogyan érhető el a fenyegetés az Eszközintelligencia-jelentés?</span><span class="sxs-lookup"><span data-stu-id="0802e-197">How do I access the Threat Intelligence Report?</span></span>

<span data-ttu-id="0802e-198">ASC elemzi a fenyegetések azonosítására különböző forrásokból származó információkat.</span><span class="sxs-lookup"><span data-stu-id="0802e-198">ASC analyzes information from multiple sources to identify threats.</span></span> <span data-ttu-id="0802e-199">Segít az incidensekre adott reakciók csapatok vizsgálja meg, és szervizelheti azokat a fenyegetéseket, a Security Center tartalmaz, amely leírja a fenyegetést észlelt fenyegetés eszközintelligencia jelentést.</span><span class="sxs-lookup"><span data-stu-id="0802e-199">To assist incident response teams investigate and remediate threats, Security Center includes a threat intelligence report that contains information about the threat that was detected.</span></span>

<span data-ttu-id="0802e-200">A Security Center háromféle fenyegetés jelentéseit, amelyek / támadás eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="0802e-200">Security Center has three types of threat reports, which can vary per attack.</span></span>
<span data-ttu-id="0802e-201">Az elérhető jelentések a következők:</span><span class="sxs-lookup"><span data-stu-id="0802e-201">The reports available are:</span></span>

- <span data-ttu-id="0802e-202">Tevékenységcsoport a jelentés: támadó, a célok és taktikai mély dives tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0802e-202">Activity Group Report: provides deep dives into attackers, their objectives and tactics.</span></span>

- <span data-ttu-id="0802e-203">Kampány jelentés: adott támadás kampányok részleteit összpontosít.</span><span class="sxs-lookup"><span data-stu-id="0802e-203">Campaign Report: focuses on details of specific attack campaigns.</span></span>

- <span data-ttu-id="0802e-204">Fenyegetés összesítő jelentés: az előző két jelentés az összes elemet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0802e-204">Threat Summary Report: covers all items in the previous two reports.</span></span>

<span data-ttu-id="0802e-205">Az ilyen információkat legyen nagyon hasznos incidensekre adott reakciók, ahol nincs folyamatban lévő vizsgálat tudni, hogy a támadás, a támadó célokat, és mi a teendő soron a probléma elhárítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="0802e-205">This type of information is very useful during the incident response process, where there is an ongoing investigation to understand the source of the attack, the attacker’s motivations, and what to do to mitigate this issue moving forward.</span></span>

1. <span data-ttu-id="0802e-206">A fenyegetés az eszközintelligencia-jelentés megnyitásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0802e-206">To access the threat intelligence report, do the following:</span></span>

2. <span data-ttu-id="0802e-207">Válassza ki a **biztonsági riasztások** a ASC irányítópult csempét.</span><span class="sxs-lookup"><span data-stu-id="0802e-207">Select the **Security alerts** tile on the ASC dashboard.</span></span>

3. <span data-ttu-id="0802e-208">Jelölje ki a biztonsági riasztást, amelyekhez további információkhoz juthat.</span><span class="sxs-lookup"><span data-stu-id="0802e-208">Select the security alert for which you want to obtain more information.</span></span>

4. <span data-ttu-id="0802e-209">Az a **jelentések** , majd kattintson a fenyegetés eszközintelligencia jelentésre mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="0802e-209">In the **Reports** field, click the link to the threat intelligence report.</span></span>

5. <span data-ttu-id="0802e-210">Ekkor megnyílik a PDF-fájl, amely letölthető.</span><span class="sxs-lookup"><span data-stu-id="0802e-210">This will open the PDF file, which you can download.</span></span>

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

<span data-ttu-id="0802e-211">A ASC fenyegetés az eszközintelligencia-jelentés kapcsolatos további információkért lásd: [Azure Security Center fenyegetés Eszközintelligencia jelentés.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)</span><span class="sxs-lookup"><span data-stu-id="0802e-211">For additional information about the ASC threat intelligence report, see [Azure Security Center Threat Intelligence Report.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)</span></span>

### <a name="assessment"></a><span data-ttu-id="0802e-212">Értékelés</span><span class="sxs-lookup"><span data-stu-id="0802e-212">Assessment</span></span>

<span data-ttu-id="0802e-213">Tesztelés, felmérési és a biztonságot értékelése érdekében ASC biztosít integrált biztonsági réseinek értékelése Qualys a felhő ügynökök, a virtuális gép javaslatok összetevő részeként.</span><span class="sxs-lookup"><span data-stu-id="0802e-213">To help with testing, assessment and evaluation of your security posture, ASC provides for integrated vulnerability assessment with Qualys cloud agents, as a part of its virtual machine recommendations component.</span></span>

<span data-ttu-id="0802e-214">A Qualys ügynök jelent a Qualys felügyeleti platform, amely ezután elküldi a biztonsági rés és állapotfigyelési adatokat biztonsági ASC biztonsági adatokat.</span><span class="sxs-lookup"><span data-stu-id="0802e-214">The Qualys agent reports vulnerability data to the Qualys management platform, which then sends vulnerability and health monitoring data back to ASC.</span></span> <span data-ttu-id="0802e-215">A javaslat hozzáadni egy biztonsági rés értékelési megoldás megjelenik a **javaslatok** a ASC irányítópult paneljét.</span><span class="sxs-lookup"><span data-stu-id="0802e-215">The recommendation to add a vulnerability assessment solution is displayed in the **Recommendations** blade on the ASC dashboard.</span></span>

<span data-ttu-id="0802e-216">Miután telepítette a biztonságirés-felmérési megoldást a célként megadott virtuális gépre, a Security Center átvizsgálja a virtuális gépet a rendszer és az alkalmazások biztonsági réseinek észlelése és azonosítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="0802e-216">After the vulnerability assessment solution is installed on the target VM, Security Center scans the VM to detect and identify system and application vulnerabilities.</span></span> <span data-ttu-id="0802e-217">Az észlelt problémák a **Virtuális gépekkel kapcsolatos javaslatok** beállítás alatt jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="0802e-217">Detected issues are shown under the **Virtual Machines Recommendations** option.</span></span>

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a><span data-ttu-id="0802e-218">Hogyan valósítja meg a biztonsági rés értékelési megoldás?</span><span class="sxs-lookup"><span data-stu-id="0802e-218">How do I implement a vulnerability assessment solution?</span></span> 

<span data-ttu-id="0802e-219">Ha egy virtuális gép nem rendelkezik az integrált biztonsági rés értékelési megoldás már üzembe van helyezve, a Security Center azt javasolja, hogy telepíteni kell.</span><span class="sxs-lookup"><span data-stu-id="0802e-219">If a Virtual Machine does not have an integrated vulnerability assessment solution already deployed, Security Center recommends that it be installed.</span></span>

1. <span data-ttu-id="0802e-220">Az ASC irányítópult a a **javaslatok** panelen válassza **biztonsági rés értékelése megoldás hozzáadása.**</span><span class="sxs-lookup"><span data-stu-id="0802e-220">In the ASC dashboard, on the **Recommendations** blade, select **Add a vulnerability assessment solution.**</span></span>

2. <span data-ttu-id="0802e-221">Válassza ki a virtuális gépeket, ahol a biztonsági rés értékelési megoldás telepíteni szeretné.</span><span class="sxs-lookup"><span data-stu-id="0802e-221">Select the VMs where you want to install the vulnerability assessment solution.</span></span>

3. <span data-ttu-id="0802e-222">Kattintson a **telepíthető [száma] virtuális gépeket.**</span><span class="sxs-lookup"><span data-stu-id="0802e-222">Click on **Install on [number of] VMs.**</span></span>

4. <span data-ttu-id="0802e-223">Válasszon ki egy partneri megoldást, az Azure piactéren vagy alatt **meglévő megoldással** válasszon **Qualys.**</span><span class="sxs-lookup"><span data-stu-id="0802e-223">Select a partner solution in the Azure Marketplace, or under **Use existing solution,** select **Qualys.**</span></span>

5. <span data-ttu-id="0802e-224">Ha bekapcsolja az automatikus frissítési beállítások ki- és kikapcsolása a **partneri megoldások** panelen.</span><span class="sxs-lookup"><span data-stu-id="0802e-224">You can turn the auto update settings on or off in the **Partner Solutions** blade.</span></span>

<span data-ttu-id="0802e-225">A biztonsági rés értékelési megoldás megvalósításához további utasításokért lásd: [biztonsági réseinek értékelése az Azure Security Centerben.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)</span><span class="sxs-lookup"><span data-stu-id="0802e-225">For further instructions on how to implement a vulnerability assessment solution, see [Vulnerability Assessment in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)</span></span>

## <a name="next-steps"></a><span data-ttu-id="0802e-226">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0802e-226">Next steps</span></span>

- [<span data-ttu-id="0802e-227">Az Azure Security Center bemutatása</span><span class="sxs-lookup"><span data-stu-id="0802e-227">Azure Security Center quick start guide</span></span>](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [<span data-ttu-id="0802e-228">Azure Security Center bemutatása</span><span class="sxs-lookup"><span data-stu-id="0802e-228">Introduction to Azure Security Center</span></span>](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [<span data-ttu-id="0802e-229">Azure Security Center riasztásait integrálása az Azure naplóelemzés integráció</span><span class="sxs-lookup"><span data-stu-id="0802e-229">Integrating Azure Security Center alerts with Azure log integration</span></span>](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [<span data-ttu-id="0802e-230">Program az Azure Security Center az integrált biztonsági réseinek értékelése</span><span class="sxs-lookup"><span data-stu-id="0802e-230">Boost Azure Security Center with Integrated Vulnerability Assessment</span></span>](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/)
