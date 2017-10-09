---
title: "aaaProtect személyes adatokat az Azure Security Center |} Microsoft Docs"
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
ms.openlocfilehash: 8e2c893d13318392f47fa912089d52618f9e7b45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a><span data-ttu-id="2c53f-103">Személyes adatok védelmét a problémák és támadások: az Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="2c53f-103">Protect personal data from breaches and attacks: Azure Security Center</span></span>

<span data-ttu-id="2c53f-104">Ez a cikk segít megérteni, hogyan feltöri a toouse az Azure Security Center tooprotect személyes adatait, és támadásokat.</span><span class="sxs-lookup"><span data-stu-id="2c53f-104">This article will help you understand how toouse Azure Security Center tooprotect personal data from breaches and attacks.</span></span>

## <a name="scenario"></a><span data-ttu-id="2c53f-105">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="2c53f-105">Scenario</span></span> 

<span data-ttu-id="2c53f-106">A nagy körutazás vállalat, az Amerikai Egyesült Államokban, hello telephelyének bővíti a műveletek toooffer útvonalak hello mediterrán, és Balti tengerek, valamint hello Brit-szigetekre.</span><span class="sxs-lookup"><span data-stu-id="2c53f-106">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="2c53f-107">az adott erőfeszítéseket toohelp, szerzett több kisebb körutazás sorok Olaszország, német, Dánia és hello brit</span><span class="sxs-lookup"><span data-stu-id="2c53f-107">toohelp in those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark, and hello U.K.</span></span>

<span data-ttu-id="2c53f-108">hello vállalati hello felhő Microsoft Azure toostore vállalati adatait használja.</span><span class="sxs-lookup"><span data-stu-id="2c53f-108">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="2c53f-109">Ez magában foglalja a személyes azonosításra alkalmas adatok, például a nevét, a címét, a telefonszámokat és a hitelkártya adatait.</span><span class="sxs-lookup"><span data-stu-id="2c53f-109">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information.</span></span> <span data-ttu-id="2c53f-110">Az adatokat is tartalmaz az emberi erőforrások, mint:</span><span class="sxs-lookup"><span data-stu-id="2c53f-110">It also includes Human Resources information such as:</span></span>

- <span data-ttu-id="2c53f-111">Címek</span><span class="sxs-lookup"><span data-stu-id="2c53f-111">Addresses</span></span>
- <span data-ttu-id="2c53f-112">telefonszámok</span><span class="sxs-lookup"><span data-stu-id="2c53f-112">Phone numbers</span></span>
- <span data-ttu-id="2c53f-113">azonosító számokat</span><span class="sxs-lookup"><span data-stu-id="2c53f-113">Tax identification numbers</span></span>
- <span data-ttu-id="2c53f-114">orvosi adatok</span><span class="sxs-lookup"><span data-stu-id="2c53f-114">Medical information</span></span>

<span data-ttu-id="2c53f-115">hello körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagjainak.</span><span class="sxs-lookup"><span data-stu-id="2c53f-115">hello cruise line also maintains a large database of reward and loyalty program members.</span></span> <span data-ttu-id="2c53f-116">Vállalati alkalmazottak hozzáférési hello hálózati hello vállalati távoli iroda és utazás ügynökök található hello világ rendelkezik toosome vállalati erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="2c53f-116">Corporate employees access hello network from hello company’s remote offices and travel agents located around hello world have access toosome company resources.</span></span>
<span data-ttu-id="2c53f-117">Személyes adatok között ezeket a helyeket és hello Microsoft adatközpont hello hálózaton áthaladó.</span><span class="sxs-lookup"><span data-stu-id="2c53f-117">Personal data travels across hello network between these locations and hello Microsoft data center.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="2c53f-118">Probléma leírása</span><span class="sxs-lookup"><span data-stu-id="2c53f-118">Problem statement</span></span>

<span data-ttu-id="2c53f-119">hello vállalat aggódik az Azure-erőforrások elleni támadások veszélye hello.</span><span class="sxs-lookup"><span data-stu-id="2c53f-119">hello company is concerned about hello threat of attacks on their Azure resources.</span></span> <span data-ttu-id="2c53f-120">Kívánják-ügyfelek és az alkalmazottak a személyes adatok toounauthorized személyek tooprevent kapta.</span><span class="sxs-lookup"><span data-stu-id="2c53f-120">They want tooprevent exposure of customers’ and employees’ personal data toounauthorized persons.</span></span> <span data-ttu-id="2c53f-121">Szeretnék egyaránt megelőzése és a válasz/szervizelés, valamint egy hatékony módot toomonitor hello a biztonság folyamatos a felhőalapú erőforrások útmutatást.</span><span class="sxs-lookup"><span data-stu-id="2c53f-121">They want guidance on both prevention and response/remediation, as well as an effective way toomonitor hello ongoing security of their cloud resources.</span></span>
<span data-ttu-id="2c53f-122">Egy erős elleni védelmi vonalat mai valamint támadók van szükségük.</span><span class="sxs-lookup"><span data-stu-id="2c53f-122">They need a strong line of defense against today’s sophisticated and organized attackers.</span></span>

## <a name="company-goal"></a><span data-ttu-id="2c53f-123">Vállalati cél</span><span class="sxs-lookup"><span data-stu-id="2c53f-123">Company goal</span></span>

<span data-ttu-id="2c53f-124">Hello vállalati célok egyik tooensure hello adatvédelmi ügyfelek és az alkalmazottak a személyes adatok védelmével fenyegetésekkel szembeni hatékony által.</span><span class="sxs-lookup"><span data-stu-id="2c53f-124">One of hello company’s goals is tooensure hello privacy of customers’ and employees’ personal data by protecting it from threats.</span></span> <span data-ttu-id="2c53f-125">A cél egyik toorespond azonnal a biztonsági szabályok megsértésére toomitigate toosigns hello hatása.</span><span class="sxs-lookup"><span data-stu-id="2c53f-125">One of their goals is toorespond immediately toosigns of breach toomitigate hello impact.</span></span> <span data-ttu-id="2c53f-126">Tooassess hello biztonsági aktuális állapotát úgy van szükség, sebezhető felismerésében, és elháríthatja a.</span><span class="sxs-lookup"><span data-stu-id="2c53f-126">It requires a way tooassess hello current state of security, identify vulnerable configurations, and remediate them.</span></span>

## <a name="solutions"></a><span data-ttu-id="2c53f-127">Megoldások</span><span class="sxs-lookup"><span data-stu-id="2c53f-127">Solutions</span></span>

<span data-ttu-id="2c53f-128">A Microsoft Azure Security Center (ASC) az integrált biztonsági monitorozást és felügyeleti megoldást nyújt.</span><span class="sxs-lookup"><span data-stu-id="2c53f-128">Microsoft Azure Security Center (ASC) provides an integrated security monitoring and policy management solution.</span></span> <span data-ttu-id="2c53f-129">Könnyen használható és hatékony megelőzését, észlelését és ellenállási képességeket kínál biztosítja.</span><span class="sxs-lookup"><span data-stu-id="2c53f-129">It delivers easy-to-use and effective threat prevention, detection, and response capabilities.</span></span>

### <a name="prevention"></a><span data-ttu-id="2c53f-130">Megelőzés</span><span class="sxs-lookup"><span data-stu-id="2c53f-130">Prevention</span></span>

<span data-ttu-id="2c53f-131">ASC segítségével megakadályozása megsértése azáltal, hogy engedélyezi tooset biztonsági házirendek, közvetlenül az időponthoz kötött hozzáférést biztosítanak, és biztonsági ajánlások megvalósításával.</span><span class="sxs-lookup"><span data-stu-id="2c53f-131">ASC helps you prevent breaches by enabling you tooset security policies, provide just-in-time access, and implement security recommendations.</span></span>

<span data-ttu-id="2c53f-132">A biztonsági házirend ajánlott a megadott hello erőforrások vezérlők hello csoportját határozza meg előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2c53f-132">A security policy defines hello set of controls recommended for resources within hello specified subscription.</span></span> <span data-ttu-id="2c53f-133">Igény szerint hozzáférést lehet használt toolock le a bejövő forgalom tooyour Azure virtuális gépeken, tooattacks veszélyeztetettségének csökkentése.</span><span class="sxs-lookup"><span data-stu-id="2c53f-133">Just in time access can be used toolock down inbound traffic tooyour Azure VMs, reducing exposure tooattacks.</span></span> <span data-ttu-id="2c53f-134">Biztonsági javaslatok az Azure-erőforrások biztonsági állapotának hello elemzése után ASC hozza létre.</span><span class="sxs-lookup"><span data-stu-id="2c53f-134">Security recommendations are created by ASC after analyzing hello security state of your Azure resources.</span></span>

#### <a name="how-do-i-set-security-policies-in-asc"></a><span data-ttu-id="2c53f-135">Biztonsági házirendek beállítása a ASC</span><span class="sxs-lookup"><span data-stu-id="2c53f-135">How do I set security policies in ASC?</span></span>

<span data-ttu-id="2c53f-136">Az egyes előfizetésekhez külön-külön biztonsági szabályzatot állíthat be.</span><span class="sxs-lookup"><span data-stu-id="2c53f-136">You can configure security policies for each subscription.</span></span> <span data-ttu-id="2c53f-137">a biztonsági házirend toomodify tulajdonosának vagy közreműködőjének előfizetésben kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2c53f-137">toomodify a security policy, you must be an owner or contributor of that subscription.</span></span> <span data-ttu-id="2c53f-138">Az hello hello Azure-portálon, a következő:</span><span class="sxs-lookup"><span data-stu-id="2c53f-138">In hello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="2c53f-139">Válassza ki **házirend** hello ASC irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="2c53f-139">Select **Policy** in hello ASC dashboard.</span></span>

2. <span data-ttu-id="2c53f-140">Válassza ki kívánja tooenable hello házirend hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="2c53f-140">Select hello subscription on which you want tooenable hello policy.</span></span>

3. <span data-ttu-id="2c53f-141">Válasszon **megakadályozási szabályzat** előfizetésenként tooconfigure házirendek.</span><span class="sxs-lookup"><span data-stu-id="2c53f-141">Choose **Prevention policy** tooconfigure policies per subscription.</span></span> <span data-ttu-id="2c53f-142">**Adatgyűjtés a virtuális gépek** túl meg kell**a.**</span><span class="sxs-lookup"><span data-stu-id="2c53f-142">**Collect data from virtual machines** should be set too**On.**</span></span>

4. <span data-ttu-id="2c53f-143">A hello **megakadályozási szabályzat** beállításnál válassza **a** tooenable hello biztonsági javaslatok szempontjából releváns hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2c53f-143">In hello **Prevention policy** options, select **On** tooenable hello security recommendations that are relevant for hello subscription.</span></span>

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

<span data-ttu-id="2c53f-144">Részletesebb utasításokat és az előzetesben hello házirend ajánlások engedélyezhető, lásd: [biztonsági házirendek beállítása az Azure Security Centerben](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).</span><span class="sxs-lookup"><span data-stu-id="2c53f-144">For more detailed instructions and an explanation of each of hello policy recommendations that can be enabled, see [Set security policies in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).</span></span>

#### <a name="how-do-i-configure-just-in-time-access-jit"></a><span data-ttu-id="2c53f-145">Hogyan konfigurálhatók a csak az idő Access (JIT)?</span><span class="sxs-lookup"><span data-stu-id="2c53f-145">How do I configure Just in Time Access (JIT)?</span></span>

<span data-ttu-id="2c53f-146">Igény szerinti engedélyezésekor a rendszer a Security Center zárolja bejövő forgalom tooyour Azure virtuális gépek egy NSG-szabály létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="2c53f-146">When JIT is enabled, Security Center locks down inbound traffic tooyour Azure VMs by creating an NSG rule.</span></span> <span data-ttu-id="2c53f-147">Kiválaszthatja hello hello VM toowhich a bejövő forgalom lesz zárolva.</span><span class="sxs-lookup"><span data-stu-id="2c53f-147">You select hello ports on hello VM toowhich inbound traffic will be locked down.</span></span> <span data-ttu-id="2c53f-148">igény szerinti toouse érni, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2c53f-148">toouse JIT access, do hello following:</span></span>

1. <span data-ttu-id="2c53f-149">Jelölje be hello **idő VM hozzáférés csempén csak** hello ASC panelen.</span><span class="sxs-lookup"><span data-stu-id="2c53f-149">Select hello **Just in time VM access tile** on hello ASC blade.</span></span>

2. <span data-ttu-id="2c53f-150">Jelölje be hello **ajánlott** fülre.</span><span class="sxs-lookup"><span data-stu-id="2c53f-150">Select hello **Recommended** tab.</span></span>

3. <span data-ttu-id="2c53f-151">A **virtuális gépek**, válassza ki a megjeleníteni kívánt tooenable hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="2c53f-151">Under **VMs**, select hello VMs that you want tooenable.</span></span> <span data-ttu-id="2c53f-152">A pipa következő tooa VM ez helyezi.</span><span class="sxs-lookup"><span data-stu-id="2c53f-152">This puts a checkmark next tooa VM.</span></span> 
4. <span data-ttu-id="2c53f-153">Válassza ki **JIT engedélyezése** virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="2c53f-153">Select **Enable JIT** on VMs.</span></span>
5. <span data-ttu-id="2c53f-154">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="2c53f-154">Select **Save**.</span></span>

<span data-ttu-id="2c53f-155">Láthatja majd ASC javasolja a rendszer engedélyezi az igény szerinti hello alapértelmezett portokat.</span><span class="sxs-lookup"><span data-stu-id="2c53f-155">Then you can see hello default ports that ASC recommends being enabled for JIT.</span></span> <span data-ttu-id="2c53f-156">Is hozzáadhat, és amikor azt szeretné csak az idő-megoldások tooenable hello új port konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2c53f-156">You can also add and configure a new port on which you want tooenable hello just in time solution.</span></span> <span data-ttu-id="2c53f-157">Hello **közvetlenül a virtuális gép elérhető** csempe hello Security Center az igény szerinti hozzáférési konfigurált virtuális géppel hello számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2c53f-157">hello **Just in time VM access** tile in hello Security Center shows hello number of VMs configured for JIT access.</span></span> <span data-ttu-id="2c53f-158">Is hello engedélyezett hozzáférési kérelmek száma a hello múlt héten jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2c53f-158">It also shows hello number of approved access requests made in hello last week.</span></span>

![](media/protect-personal-data-azure-security-center/jit-vm.png)

<span data-ttu-id="2c53f-159">Útmutatást toodo, és csak további információt a időben való hozzáférésre, lásd: [JIT-virtuális gép hozzáférés kezelése.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)</span><span class="sxs-lookup"><span data-stu-id="2c53f-159">For instructions on how toodo this, and additional information about Just in Time access, see [Manage virtual machine access using just in time.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)</span></span>

#### <a name="how-do-i-implement-asc-security-recommendations"></a><span data-ttu-id="2c53f-160">Hogyan valósítja meg a biztonsági javaslatok ASC?</span><span class="sxs-lookup"><span data-stu-id="2c53f-160">How do I implement ASC security recommendations?</span></span>

<span data-ttu-id="2c53f-161">A Security Center javaslatokat hoz létre, amikor lehetséges biztonsági réseket észlel.</span><span class="sxs-lookup"><span data-stu-id="2c53f-161">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="2c53f-162">hello javaslatok végigvezetik hello hello szükséges vezérlők konfigurálásának lépésein.</span><span class="sxs-lookup"><span data-stu-id="2c53f-162">hello recommendations guide you through hello process of configuring hello needed controls.</span></span> 
1. <span data-ttu-id="2c53f-163">Jelölje be hello **javaslatok** hello ASC irányítópult csempét.</span><span class="sxs-lookup"><span data-stu-id="2c53f-163">Select hello **Recommendations** tile on hello ASC dashboard.</span></span>

2. <span data-ttu-id="2c53f-164">Nézet hello javaslatok, amelyben soronként jelöli egy javaslat táblázatos formában látható.</span><span class="sxs-lookup"><span data-stu-id="2c53f-164">View hello recommendations, which are shown in a table format where each line represents one recommendation.</span></span>

3. <span data-ttu-id="2c53f-165">toofilter ajánlásokat, válassza ki **szűrő** és toosee kívánja hello súlyossági és állapot értékek.</span><span class="sxs-lookup"><span data-stu-id="2c53f-165">toofilter recommendations, select **Filter** and select hello severity and state values you wish toosee.</span></span>

4. <span data-ttu-id="2c53f-166">azt ajánljuk, hogy nincs megfelelő toodismiss, is kattintson a jobb gombbal és **leállítási.**</span><span class="sxs-lookup"><span data-stu-id="2c53f-166">toodismiss a recommendation that is not applicable, you can right click and select **Dismiss.**</span></span>

5. <span data-ttu-id="2c53f-167">Értékelje ki, melyik javaslat előbb alkalmazni kell.</span><span class="sxs-lookup"><span data-stu-id="2c53f-167">Evaluate which recommendation should be applied first.</span></span>

6. <span data-ttu-id="2c53f-168">Hello javaslatok a prioritásuk szerinti sorrendben alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="2c53f-168">Apply hello recommendations in order of priority.</span></span>

<span data-ttu-id="2c53f-169">Lehetséges követelményekkel, és hogyan útmutatók listája tooapply minden, lásd: [biztonsági javaslatok kezelése az Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)</span><span class="sxs-lookup"><span data-stu-id="2c53f-169">For a list of possible recommendations and walk-throughs on how tooapply each, see [Managing security recommendations in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)</span></span>

### <a name="detection-and-response"></a><span data-ttu-id="2c53f-170">Felderítését és a válasz</span><span class="sxs-lookup"><span data-stu-id="2c53f-170">Detection and Response</span></span>

<span data-ttu-id="2c53f-171">Felderítését és a válasz Ugrás együtt, ahányat csak szeretne toorespond minél gyorsabban után fenyegetést észlel.</span><span class="sxs-lookup"><span data-stu-id="2c53f-171">Detection and response go together, as you want toorespond as quickly as possible after a threat is detected.</span></span>
<span data-ttu-id="2c53f-172">ASC fenyegetésészlelés működik, automatikusan az Azure-erőforrások, hello és hálózati összekapcsolt partnermegoldások a biztonsági információk begyűjtése.</span><span class="sxs-lookup"><span data-stu-id="2c53f-172">ASC threat detection works by automatically collecting security information from your Azure resources, hello network, and connected partner solutions.</span></span> <span data-ttu-id="2c53f-173">ASC is gyorsan frissíti az észlelési algoritmusokat támadók kiadás új és egyre kifinomult biztonsági réseket.</span><span class="sxs-lookup"><span data-stu-id="2c53f-173">ASC can rapidly update its detection algorithms as attackers release new and increasingly sophisticated exploits.</span></span> <span data-ttu-id="2c53f-174">További információk a ASC tartozó fenyegetésészlelés működése, a következő témakörben: [az Azure Security Center az észlelési képességek](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).</span><span class="sxs-lookup"><span data-stu-id="2c53f-174">For more detailed information on how ASC’s threat detection works, see [Azure Security Center detection capabilities](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).</span></span>

#### <a name="how-do-i-manage-and-respond-toosecurity-alerts"></a><span data-ttu-id="2c53f-175">Hogyan kezelheti és toosecurity riasztások válaszolni?</span><span class="sxs-lookup"><span data-stu-id="2c53f-175">How do I manage and respond toosecurity alerts?</span></span>

<span data-ttu-id="2c53f-176">A rangsorolt biztonsági riasztások listája látható a Security Center együtt hello tooquickly szükséges információk, vizsgálja meg hello problémát.</span><span class="sxs-lookup"><span data-stu-id="2c53f-176">A list of prioritized security alerts is shown in Security Center along with hello information you need tooquickly investigate hello problem.</span></span> <span data-ttu-id="2c53f-177">A Security Center is módjára vonatkozó javaslatokkal tooremediate támadás.</span><span class="sxs-lookup"><span data-stu-id="2c53f-177">Security Center also includes recommendations for how tooremediate an attack.</span></span> <span data-ttu-id="2c53f-178">a biztonsági riasztások, hajtsa végre a következő hello toomanage:</span><span class="sxs-lookup"><span data-stu-id="2c53f-178">toomanage your security alerts, do hello following:</span></span>

1. <span data-ttu-id="2c53f-179">Jelölje be hello **biztonsági riasztások** hello ASC irányítópult csempére.</span><span class="sxs-lookup"><span data-stu-id="2c53f-179">Select hello **Security alerts** tile in hello ASC dashboard.</span></span> <span data-ttu-id="2c53f-180">Ez azt jelenti, az egyes riasztások részletei.</span><span class="sxs-lookup"><span data-stu-id="2c53f-180">This shows details for each alert.</span></span>

2. <span data-ttu-id="2c53f-181">Válassza ki a toofilter riasztások dátum, állapot és súlyosság alapján **szűrő** , és válassza a kívánt toosee hello értékek.</span><span class="sxs-lookup"><span data-stu-id="2c53f-181">toofilter alerts based on date, state, and severity, select **Filter** and then select hello values you want toosee.</span></span>

3. <span data-ttu-id="2c53f-182">toorespond tooan riasztásra, válassza ki azt hello tekintse át, majd válassza ki a támadásnak kitett erőforrásra hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="2c53f-182">toorespond tooan alert, select it and review hello information, then select hello resource that was attacked.</span></span>

4. <span data-ttu-id="2c53f-183">A hello **leírás** mezőben adatait, többek között az ajánlott javítási láthatja.</span><span class="sxs-lookup"><span data-stu-id="2c53f-183">In hello **Description** field, you’ll see details, including recommended remediation.</span></span>

![](media/protect-personal-data-azure-security-center/security-alerts.png)

<span data-ttu-id="2c53f-184">Részletes utasítások válaszol toosecurity riasztások, lásd: [kezelése és válaszol toosecurity az Azure Security Center riasztást küld.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)</span><span class="sxs-lookup"><span data-stu-id="2c53f-184">For more detailed instructions on responding toosecurity alerts, see [Managing and responding toosecurity alerts in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)</span></span>

<span data-ttu-id="2c53f-185">További segítségért a biztonsági riasztások kivizsgálásának hello vállalati ASC riasztások integrálhatja a saját SIEM-megoldás használatával [Azure napló integrációs](https://aka.ms/AzLog).</span><span class="sxs-lookup"><span data-stu-id="2c53f-185">For further help in investigating security alerts, hello company can integrate ASC alerts with its own SIEM solution, using [Azure Log Integration](https://aka.ms/AzLog).</span></span>

#### <a name="how-do-i-manage-security-incidents"></a><span data-ttu-id="2c53f-186">Hogyan kezelheti a biztonsági események?</span><span class="sxs-lookup"><span data-stu-id="2c53f-186">How do I manage security incidents?</span></span>

<span data-ttu-id="2c53f-187">A ASC egy biztonsági incidens az összes riasztás erőforrás, amely megfelel-e a kill lánc minták összesítést.</span><span class="sxs-lookup"><span data-stu-id="2c53f-187">In ASC, a security incident is an aggregation of all alerts for a resource that align with kill chain patterns.</span></span> <span data-ttu-id="2c53f-188">Incidens jelenítenek meg hello kapcsolódó riasztások listája, amely lehetővé teszi, hogy tooobtain további információ a minden egyes előfordulásakor.</span><span class="sxs-lookup"><span data-stu-id="2c53f-188">An Incident will reveal hello list of related alerts, which enables you tooobtain more information about each occurrence.</span></span> <span data-ttu-id="2c53f-189">Incidensek megjelennek a biztonsági riasztások csempe hello és panelen.</span><span class="sxs-lookup"><span data-stu-id="2c53f-189">Incidents appear in hello Security Alerts tile and blade.</span></span>

<span data-ttu-id="2c53f-190">tooreview és biztonsági incidensek kezeléséhez, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2c53f-190">tooreview and manage security incidents, do hello following:</span></span>

1. <span data-ttu-id="2c53f-191">Jelölje be hello **biztonsági riasztások** csempére.</span><span class="sxs-lookup"><span data-stu-id="2c53f-191">Select hello **Security alerts** tile.</span></span> <span data-ttu-id="2c53f-192">Ha egy biztonsági incidens észlel, akkor jelenik meg hello biztonsági riasztások graph alatt.</span><span class="sxs-lookup"><span data-stu-id="2c53f-192">if a security incident is detected, it will appear under hello security alerts graph.</span></span> <span data-ttu-id="2c53f-193">Egy ikont, amely eltér a más riasztások lesz.</span><span class="sxs-lookup"><span data-stu-id="2c53f-193">It will have an icon that’s different from other alerts.</span></span>

2. <span data-ttu-id="2c53f-194">Válassza ki a hello incidens toosee további információt a biztonsági incidens.</span><span class="sxs-lookup"><span data-stu-id="2c53f-194">Select hello incident toosee more details about this security incident.</span></span> <span data-ttu-id="2c53f-195">További részletek közé tartoznak a teljes leírását, a súlyosság, a jelenlegi állapotában, megtámadott hello erőforrás, hello javítási lépéseket hello incidens, és ezt az incidenst foglalt riasztások hello.</span><span class="sxs-lookup"><span data-stu-id="2c53f-195">Additional details include its full description, its severity, its current state, hello attacked resource, hello remediation steps for hello incident, and hello alerts that were included in this incident.</span></span>

<span data-ttu-id="2c53f-196">Szűrheti a toosee **incidensek csak**, **riasztásokat csak**, vagy **mindkét**.</span><span class="sxs-lookup"><span data-stu-id="2c53f-196">You can filter toosee **incidents only**, **alerts only**, or **both**.</span></span>

#### <a name="how-do-i-access-hello-threat-intelligence-report"></a><span data-ttu-id="2c53f-197">Hogyan férhetek hello fenyegetés az Eszközintelligencia-jelentés?</span><span class="sxs-lookup"><span data-stu-id="2c53f-197">How do I access hello Threat Intelligence Report?</span></span>

<span data-ttu-id="2c53f-198">ASC elemzi az adatokat több forrásból tooidentify fenyegetésekkel szembeni hatékony.</span><span class="sxs-lookup"><span data-stu-id="2c53f-198">ASC analyzes information from multiple sources tooidentify threats.</span></span> <span data-ttu-id="2c53f-199">tooassist incidensválasz-csoportok vizsgálja meg, és szervizelheti azokat a fenyegetéseket, a Security Center tartalmaz, amely leírja hello fenyegetést észlelt fenyegetés eszközintelligencia jelentést.</span><span class="sxs-lookup"><span data-stu-id="2c53f-199">tooassist incident response teams investigate and remediate threats, Security Center includes a threat intelligence report that contains information about hello threat that was detected.</span></span>

<span data-ttu-id="2c53f-200">A Security Center háromféle fenyegetés jelentéseit, amelyek / támadás eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="2c53f-200">Security Center has three types of threat reports, which can vary per attack.</span></span>
<span data-ttu-id="2c53f-201">hello elérhető jelentéseket a következők:</span><span class="sxs-lookup"><span data-stu-id="2c53f-201">hello reports available are:</span></span>

- <span data-ttu-id="2c53f-202">Tevékenységcsoport a jelentés: támadó, a célok és taktikai mély dives tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2c53f-202">Activity Group Report: provides deep dives into attackers, their objectives and tactics.</span></span>

- <span data-ttu-id="2c53f-203">Kampány jelentés: adott támadás kampányok részleteit összpontosít.</span><span class="sxs-lookup"><span data-stu-id="2c53f-203">Campaign Report: focuses on details of specific attack campaigns.</span></span>

- <span data-ttu-id="2c53f-204">Fenyegetés összesítő jelentés: hello előző két jelentés az összes elemet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2c53f-204">Threat Summary Report: covers all items in hello previous two reports.</span></span>

<span data-ttu-id="2c53f-205">Az ilyen információkat nagyon hasznos hello incidensválasz-folyamata során, ahol egy folyamatban lévő vizsgálat toounderstand hello forrás hello támadások, hello támadó célokat, és milyen toodo toomitigate áthelyezése előre ez adja ki.</span><span class="sxs-lookup"><span data-stu-id="2c53f-205">This type of information is very useful during hello incident response process, where there is an ongoing investigation toounderstand hello source of hello attack, hello attacker’s motivations, and what toodo toomitigate this issue moving forward.</span></span>

1. <span data-ttu-id="2c53f-206">tooaccess hello fenyegetésfelderítési adataival jelentésből, és a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2c53f-206">tooaccess hello threat intelligence report, do hello following:</span></span>

2. <span data-ttu-id="2c53f-207">Jelölje be hello **biztonsági riasztások** hello ASC irányítópult csempét.</span><span class="sxs-lookup"><span data-stu-id="2c53f-207">Select hello **Security alerts** tile on hello ASC dashboard.</span></span>

3. <span data-ttu-id="2c53f-208">Jelölje ki a kívánt tooobtain hello biztonsági riasztást további információt.</span><span class="sxs-lookup"><span data-stu-id="2c53f-208">Select hello security alert for which you want tooobtain more information.</span></span>

4. <span data-ttu-id="2c53f-209">A hello **jelentések** , majd kattintson a hello hivatkozás toohello fenyegetés eszközintelligencia jelentés.</span><span class="sxs-lookup"><span data-stu-id="2c53f-209">In hello **Reports** field, click hello link toohello threat intelligence report.</span></span>

5. <span data-ttu-id="2c53f-210">Ekkor megnyílik hello PDF-fájl, amely letölthető.</span><span class="sxs-lookup"><span data-stu-id="2c53f-210">This will open hello PDF file, which you can download.</span></span>

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

<span data-ttu-id="2c53f-211">További információ a hello ASC fenyegetés az eszközintelligencia-jelentés: [Azure Security Center fenyegetés Eszközintelligencia jelentés.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)</span><span class="sxs-lookup"><span data-stu-id="2c53f-211">For additional information about hello ASC threat intelligence report, see [Azure Security Center Threat Intelligence Report.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)</span></span>

### <a name="assessment"></a><span data-ttu-id="2c53f-212">Értékelés</span><span class="sxs-lookup"><span data-stu-id="2c53f-212">Assessment</span></span>

<span data-ttu-id="2c53f-213">toohelp tesztelése, felmérési és a biztonságot értékelése, ASC biztosít integrált biztonsági réseinek értékelése Qualys a felhő ügynökök, a virtuális gép javaslatok összetevő részeként.</span><span class="sxs-lookup"><span data-stu-id="2c53f-213">toohelp with testing, assessment and evaluation of your security posture, ASC provides for integrated vulnerability assessment with Qualys cloud agents, as a part of its virtual machine recommendations component.</span></span>

<span data-ttu-id="2c53f-214">hello Qualys ügynök jelentéseket küld a biztonsági adatok toohello Qualys felügyeleti platform, amely ezután elküldi a biztonsági rés és állapotfigyelési adatokat biztonsági tooASC.</span><span class="sxs-lookup"><span data-stu-id="2c53f-214">hello Qualys agent reports vulnerability data toohello Qualys management platform, which then sends vulnerability and health monitoring data back tooASC.</span></span> <span data-ttu-id="2c53f-215">a biztonsági rés értékelése megoldás megjelenik a hello ajánlás tooadd hello **javaslatok** hello ASC irányítópult paneljét.</span><span class="sxs-lookup"><span data-stu-id="2c53f-215">hello recommendation tooadd a vulnerability assessment solution is displayed in hello **Recommendations** blade on hello ASC dashboard.</span></span>

<span data-ttu-id="2c53f-216">Hello biztonsági rés értékelési megoldás hello cél virtuális gép telepítését követően a Security Center vizsgálatok VM toodetect hello, és a rendszer- és biztonsági rések azonosítása.</span><span class="sxs-lookup"><span data-stu-id="2c53f-216">After hello vulnerability assessment solution is installed on hello target VM, Security Center scans hello VM toodetect and identify system and application vulnerabilities.</span></span> <span data-ttu-id="2c53f-217">Észlelt problémák hello alatt látható **virtuális gépek javaslatok** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2c53f-217">Detected issues are shown under hello **Virtual Machines Recommendations** option.</span></span>

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a><span data-ttu-id="2c53f-218">Hogyan valósítja meg a biztonsági rés értékelési megoldás?</span><span class="sxs-lookup"><span data-stu-id="2c53f-218">How do I implement a vulnerability assessment solution?</span></span> 

<span data-ttu-id="2c53f-219">Ha egy virtuális gép nem rendelkezik az integrált biztonsági rés értékelési megoldás már üzembe van helyezve, a Security Center azt javasolja, hogy telepíteni kell.</span><span class="sxs-lookup"><span data-stu-id="2c53f-219">If a Virtual Machine does not have an integrated vulnerability assessment solution already deployed, Security Center recommends that it be installed.</span></span>

1. <span data-ttu-id="2c53f-220">Hello ASC irányítópulton hello **javaslatok** panelen válassza **biztonsági rés értékelése megoldás hozzáadása.**</span><span class="sxs-lookup"><span data-stu-id="2c53f-220">In hello ASC dashboard, on hello **Recommendations** blade, select **Add a vulnerability assessment solution.**</span></span>

2. <span data-ttu-id="2c53f-221">Válassza ki a kívánt tooinstall hello biztonsági rés értékelési megoldás hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="2c53f-221">Select hello VMs where you want tooinstall hello vulnerability assessment solution.</span></span>

3. <span data-ttu-id="2c53f-222">Kattintson a **telepíthető [száma] virtuális gépeket.**</span><span class="sxs-lookup"><span data-stu-id="2c53f-222">Click on **Install on [number of] VMs.**</span></span>

4. <span data-ttu-id="2c53f-223">Válassza ki egy partneri megoldást hello Azure piactérről, illetve a **meglévő megoldással** válasszon **Qualys.**</span><span class="sxs-lookup"><span data-stu-id="2c53f-223">Select a partner solution in hello Azure Marketplace, or under **Use existing solution,** select **Qualys.**</span></span>

5. <span data-ttu-id="2c53f-224">Hello automatikus frissítési beállítások ki- és kikapcsolása a hello kapcsolhatja **partneri megoldások** panelen.</span><span class="sxs-lookup"><span data-stu-id="2c53f-224">You can turn hello auto update settings on or off in hello **Partner Solutions** blade.</span></span>

<span data-ttu-id="2c53f-225">További útmutatást tooimplement egy biztonsági rés megoldással, lásd: [biztonsági réseinek értékelése az Azure Security Centerben.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)</span><span class="sxs-lookup"><span data-stu-id="2c53f-225">For further instructions on how tooimplement a vulnerability assessment solution, see [Vulnerability Assessment in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c53f-226">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2c53f-226">Next steps</span></span>

- [<span data-ttu-id="2c53f-227">Az Azure Security Center bemutatása</span><span class="sxs-lookup"><span data-stu-id="2c53f-227">Azure Security Center quick start guide</span></span>](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [<span data-ttu-id="2c53f-228">A Security Center bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="2c53f-228">Introduction tooAzure Security Center</span></span>](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [<span data-ttu-id="2c53f-229">Azure Security Center riasztásait integrálása az Azure naplóelemzés integráció</span><span class="sxs-lookup"><span data-stu-id="2c53f-229">Integrating Azure Security Center alerts with Azure log integration</span></span>](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [<span data-ttu-id="2c53f-230">Program az Azure Security Center az integrált biztonsági réseinek értékelése</span><span class="sxs-lookup"><span data-stu-id="2c53f-230">Boost Azure Security Center with Integrated Vulnerability Assessment</span></span>](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/)
