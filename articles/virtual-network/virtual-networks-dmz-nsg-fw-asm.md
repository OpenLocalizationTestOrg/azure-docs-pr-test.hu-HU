---
title: "DMZ példa – összeállítása a tűzfal és az NSG-alkalmazások védelmét DMZ |} Microsoft Docs"
description: "A tűzfal és a hálózati biztonsági csoportokkal (NSG) DMZ összeállítása"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: c78491c7-54ac-4469-851c-b35bfed0f528
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: cc0e8a3fa749eb2e6f65ef92c2d3cb404cfc8bc0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a><span data-ttu-id="e4f38-103">2 – példa egy tűzfal és az NSG-alkalmazások védelmét DMZ összeállítása</span><span class="sxs-lookup"><span data-stu-id="e4f38-103">Example 2 – Build a DMZ to protect applications with a Firewall and NSGs</span></span>
<span data-ttu-id="e4f38-104">[A biztonsági határ bevált gyakorlatok laphoz való visszatéréshez][HOME]</span><span class="sxs-lookup"><span data-stu-id="e4f38-104">[Return to the Security Boundary Best Practices Page][HOME]</span></span>

<span data-ttu-id="e4f38-105">Ebben a példában a DMZ hozzon létre egy tűzfal, négy windows Server-kiszolgálók és hálózati biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="e4f38-105">This example will create a DMZ with a firewall, four windows servers, and Network Security Groups.</span></span> <span data-ttu-id="e4f38-106">Azt is haladhat végig a megfelelő parancsok bemutatják az egyes lépések, adja meg.</span><span class="sxs-lookup"><span data-stu-id="e4f38-106">It will also walk through each of the relevant commands to provide a deeper understanding of each step.</span></span> <span data-ttu-id="e4f38-107">Is van a forgalom forgatókönyv részt, hogy egy részletesebb lépésenkénti hogyan forgalom halad keresztül a Szegélyhálózaton lévő védelmi réteget.</span><span class="sxs-lookup"><span data-stu-id="e4f38-107">There is also a Traffic Scenario section to provide an in-depth step-by-step how traffic proceeds through the layers of defense in the DMZ.</span></span> <span data-ttu-id="e4f38-108">Végezetül a hivatkozások szakasz: a teljes kód látható, míg utasítás ebben a környezetben, teszteléséhez, és a különböző forgatókönyvekben kísérletezhet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e4f38-108">Finally, in the references section is the complete code and instruction to build this environment to test and experiment with various scenarios.</span></span> 

<span data-ttu-id="e4f38-109">![NVA és NSG bejövő DMZ][1]</span><span class="sxs-lookup"><span data-stu-id="e4f38-109">![Inbound DMZ with NVA and NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="e4f38-110">Környezet leírása</span><span class="sxs-lookup"><span data-stu-id="e4f38-110">Environment Description</span></span>
<span data-ttu-id="e4f38-111">Ebben a példában nincs olyan előfizetést, amelyet a következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="e4f38-111">In this example there is a subscription that contains the following:</span></span>

* <span data-ttu-id="e4f38-112">A felhőalapú szolgáltatások két: "FrontEnd001" és "BackEnd001"</span><span class="sxs-lookup"><span data-stu-id="e4f38-112">Two cloud services: “FrontEnd001” and “BackEnd001”</span></span>
* <span data-ttu-id="e4f38-113">A virtuális hálózati "CorpNetwork", két alhálózattal rendelkező: "Előtér" és "Háttér"</span><span class="sxs-lookup"><span data-stu-id="e4f38-113">A Virtual Network “CorpNetwork”, with two subnets: “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="e4f38-114">Mindkét alhálózat alkalmazott egyetlen hálózati biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="e4f38-114">A single Network Security Group that is applied to both subnets</span></span>
* <span data-ttu-id="e4f38-115">Ebben a példában a Barracuda NextGen tűzfal, a hálózati virtuális készülék a Frontend alhálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="e4f38-115">A network virtual appliance, in this example a Barracuda NextGen Firewall, connected to the Frontend subnet</span></span>
* <span data-ttu-id="e4f38-116">A Windows Server egy webalkalmazás-kiszolgáló ("IIS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="e4f38-116">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="e4f38-117">Két windows Server kiszolgálókon, amelyek megfelelnek az alkalmazás biztonsági end-kiszolgálók ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="e4f38-117">Two windows servers that represent application back end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="e4f38-118">A Windows server DNS-kiszolgáló ("DNS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="e4f38-118">A Windows server that represents a DNS server (“DNS01”)</span></span>

> [!NOTE]
> <span data-ttu-id="e4f38-119">Bár ebben a példában Barracuda NextGen tűzfalat használ, a másik virtuális hálózati berendezések számos ehhez a példához is használható.</span><span class="sxs-lookup"><span data-stu-id="e4f38-119">Although this example uses a Barracuda NextGen Firewall, many of the different Network Virtual Appliances could be used for this example.</span></span>
> 
> 

<span data-ttu-id="e4f38-120">Az alábbi hivatkozások részben van egy PowerShell-parancsfájlt, amely a fent leírt környezetben a legtöbb fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e4f38-120">In the references section below there is a PowerShell script that will build most of the environment described above.</span></span> <span data-ttu-id="e4f38-121">A virtuális gépek és virtuális hálózatok, bár a példaként megadott parancsfájlt, által végzett dokumentum nem ismerteti a jelen dokumentum részletesen.</span><span class="sxs-lookup"><span data-stu-id="e4f38-121">Building the VMs and Virtual Networks, although are done by the example script, are not described in detail in this document.</span></span>

<span data-ttu-id="e4f38-122">A környezet létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="e4f38-122">To build the environment:</span></span>

1. <span data-ttu-id="e4f38-123">Mentse a hálózati konfigurációs XML-fájlt a references szakaszában szereplő (frissítődik nevét, helyét és IP-címeket az adott forgatókönyv esetén)</span><span class="sxs-lookup"><span data-stu-id="e4f38-123">Save the network config xml file included in the references section (updated with names, location, and IP addresses to match the given scenario)</span></span>
2. <span data-ttu-id="e4f38-124">A felhasználói változók a parancsfájl felel meg a parancsfájl-hoz (előfizetések, service nevét stb.) kell futtatni a környezet frissítése</span><span class="sxs-lookup"><span data-stu-id="e4f38-124">Update the user variables in the script to match the environment the script is to be run against (subscriptions, service names, etc)</span></span>
3. <span data-ttu-id="e4f38-125">A parancsfájl végrehajtása a PowerShellben</span><span class="sxs-lookup"><span data-stu-id="e4f38-125">Execute the script in PowerShell</span></span>

<span data-ttu-id="e4f38-126">**Megjegyzés:**: A régió, a PowerShell-parancsfájl esetében meg kell egyeznie a hálózati konfigurációs XML-fájl esetében.</span><span class="sxs-lookup"><span data-stu-id="e4f38-126">**Note**: The region signified in the PowerShell script must match the region signified in the network configuration xml file.</span></span>

<span data-ttu-id="e4f38-127">Miután a parancsfájl sikeresen fut a következő utáni parancsfájl lépések tehetők:</span><span class="sxs-lookup"><span data-stu-id="e4f38-127">Once the script runs successfully the following post-script steps may be taken:</span></span>

1. <span data-ttu-id="e4f38-128">Állítsa be a tűzfalszabályok, ezzel kapcsolatban lásd: című részt az alábbi: tűzfalszabályokat.</span><span class="sxs-lookup"><span data-stu-id="e4f38-128">Set up the firewall rules, this is covered in the section below titled: Firewall Rules.</span></span>
2. <span data-ttu-id="e4f38-129">Opcionálisan a references szakaszában vannak két parancsfájlok állíthat be a webkiszolgáló és egy egyszerű webes alkalmazás tesztelése a DMZ-konfiguráció engedélyezése az alkalmazások kiszolgálói.</span><span class="sxs-lookup"><span data-stu-id="e4f38-129">Optionally in the references section are two scripts to set up the web server and app server with a simple web application to allow testing with this DMZ configuration.</span></span>

<span data-ttu-id="e4f38-130">Az alábbi szakasz ismerteti a legtöbb hálózati biztonsági csoportok viszonyítva parancsfájlok utasítás.</span><span class="sxs-lookup"><span data-stu-id="e4f38-130">The next section explains most of the scripts statements relative to Network Security Groups.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="e4f38-131">Hálózati biztonsági csoportok (NSG)</span><span class="sxs-lookup"><span data-stu-id="e4f38-131">Network Security Groups (NSG)</span></span>
<span data-ttu-id="e4f38-132">Az ebben a példában egy NSG-csoport összeállítása és hat szabályokkal majd betölteni.</span><span class="sxs-lookup"><span data-stu-id="e4f38-132">For this example, a NSG group is built and then loaded with six rules.</span></span> 

> [!TIP]
> <span data-ttu-id="e4f38-133">Általánosságban véve az "Engedélyezés" speciális szabályok először hozzon létre, és majd a általánosabbá "Deny" szabályok utolsó.</span><span class="sxs-lookup"><span data-stu-id="e4f38-133">Generally speaking, you should create your specific “Allow” rules first and then the more generic “Deny” rules last.</span></span> <span data-ttu-id="e4f38-134">A prioritási megkövetel amelyek szabályokat első értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="e4f38-134">The assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="e4f38-135">Forgalom kiderül, hogy egy adott szabály vonatkozik, ha nincsenek további szabályok kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="e4f38-135">Once traffic is found to apply to a specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="e4f38-136">NSG-szabályok egyaránt (az alhálózati szempontjából) a bejövő vagy kimenő irányban is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="e4f38-136">NSG rules can apply in either in the inbound or outbound direction (from the perspective of the subnet).</span></span>
> 
> 

<span data-ttu-id="e4f38-137">Deklaratív módon a bejövő forgalom beépített folyamatban a következő szabályokat:</span><span class="sxs-lookup"><span data-stu-id="e4f38-137">Declaratively, the following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="e4f38-138">Belső DNS-forgalom (53-as port) engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="e4f38-138">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="e4f38-139">RDP-forgalmát (3389-es port) az internetről bármely virtuális géphez engedélyezett</span><span class="sxs-lookup"><span data-stu-id="e4f38-139">RDP traffic (port 3389) from the Internet to any VM is allowed</span></span>
3. <span data-ttu-id="e4f38-140">Engedélyezett HTTP-forgalom (80-as port) az internethez az NVA (tűzfal)</span><span class="sxs-lookup"><span data-stu-id="e4f38-140">HTTP traffic (port 80) from the Internet to the NVA (firewall) is allowed</span></span>
4. <span data-ttu-id="e4f38-141">Minden forgalmat (minden porthoz) IIS01 AppVM1 engedélyezett</span><span class="sxs-lookup"><span data-stu-id="e4f38-141">Any traffic (all ports) from IIS01 to AppVM1 is allowed</span></span>
5. <span data-ttu-id="e4f38-142">Az összes bejövő forgalom (minden porthoz) az internetről a teljes virtuális hálózatot (alhálózatok mindkét) megtagadva</span><span class="sxs-lookup"><span data-stu-id="e4f38-142">Any traffic (all ports) from the Internet to the entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="e4f38-143">Az összes bejövő forgalom (minden porthoz) a Frontend alhálózatból a Backend alhálózathoz megtagadva</span><span class="sxs-lookup"><span data-stu-id="e4f38-143">Any traffic (all ports) from the Frontend subnet to the Backend subnet is Denied</span></span>

<span data-ttu-id="e4f38-144">A következő szabályok kötött alhálózatok, ha a HTTP-kérelem a webkiszolgálón, 3 szabályokat is az internetről bejövő (engedélyezése) és 5 (megtagadni) szeretné alkalmazni, de mivel a szabály 3 egy magasabb prioritással bír, csak azt csak akkor vonatkozik, és 5 szabály nem lesz play kerülhet.</span><span class="sxs-lookup"><span data-stu-id="e4f38-144">With these rules bound to each subnet, if a HTTP request was inbound from the Internet to the web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="e4f38-145">A HTTP-kérelem így szeretné tenni, hogy a tűzfal.</span><span class="sxs-lookup"><span data-stu-id="e4f38-145">Thus the HTTP request would be allowed to the firewall.</span></span> <span data-ttu-id="e4f38-146">Ha ugyanaz a forgalom próbál elérni az DNS01 kiszolgálót, szabály 5 (Megtagadás) lenne az első alkalmazni, és szeretné, hogy a kiszolgáló nem engedélyezett a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="e4f38-146">If that same traffic was trying to reach the DNS01 server, rule 5 (Deny) would be the first to apply and the traffic would not be allowed to pass to the server.</span></span> <span data-ttu-id="e4f38-147">6 (Megtagadás) szabály blokkolja a Frontend alhálózathoz van szó, a Backend alhálózathoz (kivéve a engedélyezett forgalom szabályokban 1 és 4) a, ez védelmet nyújt a háttér hálózathoz, abban az esetben, ha egy támadó biztonság sérüléseinek előtér, a támadó a webalkalmazás volna korlátozott hozzáféréssel a háttérrendszer "védett" hálózaton (csak a AppVM01 kiszolgálón kitett erőforrások).</span><span class="sxs-lookup"><span data-stu-id="e4f38-147">Rule 6 (Deny) blocks the Frontend subnet from talking to the Backend subnet (except for allowed traffic in rules 1 and 4), this protects the Backend network in case an attacker compromises the web application on the Frontend, the attacker would have limited access to the Backend “protected” network (only to resources exposed on the AppVM01 server).</span></span>

<span data-ttu-id="e4f38-148">Nincs olyan alapértelmezett kimenő szabály, amely lehetővé teszi, hogy a kimenő forgalom az internethez.</span><span class="sxs-lookup"><span data-stu-id="e4f38-148">There is a default outbound rule that allows traffic out to the internet.</span></span> <span data-ttu-id="e4f38-149">Ebben a példában a Microsoft most átengedi a kimenő forgalmat, és nem módosítja az egyetlen kimenő szabályok.</span><span class="sxs-lookup"><span data-stu-id="e4f38-149">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="e4f38-150">A forgalom zárolását, mindkét irányban, a felhasználó definiált útválasztási szükség, ez van írja le egy másik példa is található a [fő biztonsági határ dokumentum][HOME].</span><span class="sxs-lookup"><span data-stu-id="e4f38-150">To lock down traffic in both directions, User Defined Routing is required, this is explored in a different example that can found in the [main security boundary document][HOME].</span></span>

<span data-ttu-id="e4f38-151">A fent tárgyaltuk NSG-szabályok nagyon hasonlóak az NSG-szabályok [1 -. példa az NSG-ket egy egyszerű DMZ Build][Example1].</span><span class="sxs-lookup"><span data-stu-id="e4f38-151">The above discussed NSG rules are very similar to the NSG rules in [Example 1 - Build a Simple DMZ with NSGs][Example1].</span></span> <span data-ttu-id="e4f38-152">Tekintse át az egyes NSG-szabályokat és az attribútumok részletes tekintse meg a dokumentum a NSG leírását.</span><span class="sxs-lookup"><span data-stu-id="e4f38-152">Please review the NSG Description in that document for a detailed look at each NSG rule and it's attributes.</span></span>

## <a name="firewall-rules"></a><span data-ttu-id="e4f38-153">Tűzfalszabályok</span><span class="sxs-lookup"><span data-stu-id="e4f38-153">Firewall Rules</span></span>
<span data-ttu-id="e4f38-154">A kezelési ügynök a számítógépen a tűzfal felügyeletéhez, és a szükséges konfigurációk létrehozásához telepíteni kell.</span><span class="sxs-lookup"><span data-stu-id="e4f38-154">A management client will need to be installed on a PC to manage the firewall and create the configurations needed.</span></span> <span data-ttu-id="e4f38-155">A dokumentáció a tűzfal (vagy más NVA) szállítójával tekintse meg az eszközök felügyelete.</span><span class="sxs-lookup"><span data-stu-id="e4f38-155">See the documentation from your firewall (or other NVA) vendor on how to manage the device.</span></span> <span data-ttu-id="e4f38-156">Ez a szakasz többi magát, a tűzfal a szállító felügyeleti ügyfél (azaz nem az Azure portál vagy PowerShell) keresztül lesz konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="e4f38-156">The remainder of this section will describe the configuration of the firewall itself, through the vendors management client (i.e. not the Azure portal or PowerShell).</span></span>

<span data-ttu-id="e4f38-157">Ügyfél letöltése és az ebben a példában használt Barracuda kapcsolódás itt található: [Barracuda NG rendszergazda](https://techlib.barracuda.com/NG61/NGAdmin)</span><span class="sxs-lookup"><span data-stu-id="e4f38-157">Instructions for client download and connecting to the Barracuda used in this example can be found here: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span></span>

<span data-ttu-id="e4f38-158">A tűzfalon szabályok továbbítási kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e4f38-158">On the firewall, forwarding rules will need to be created.</span></span> <span data-ttu-id="e4f38-159">Ebben a példában csak az internetes forgalmat az adathoz kötött a tűzfal, majd a webkiszolgáló irányítja, mert csak egy továbbító NAT-szabály van szükség.</span><span class="sxs-lookup"><span data-stu-id="e4f38-159">Since this example only routes internet traffic in-bound to the firewall and then to the web server, only one forwarding NAT rule is needed.</span></span> <span data-ttu-id="e4f38-160">A Barracuda NextGen tűzfalon használt ebben a példában a szabály lenne egy cél NAT-szabály ("nyári időszámítás NAT") továbbítani a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="e4f38-160">On the Barracuda NextGen Firewall used in this example the rule would be a Destination NAT rule (“Dst NAT”) to pass this traffic.</span></span>

<span data-ttu-id="e4f38-161">Hozza létre a következő szabályt (vagy ellenőrizze a meglévő alapértelmezett szabályok), a Barracuda NG felügyeleti ügyfél irányítópultról indítása a konfiguráció lapjának megjelenítéséhez, a működési konfigurációs szakaszban kattintson szabálykészletben.</span><span class="sxs-lookup"><span data-stu-id="e4f38-161">To create the following rule (or verify existing default rules), starting from the Barracuda NG Admin client dashboard, navigate to the configuration tab, in the Operational Configuration section click Ruleset.</span></span> <span data-ttu-id="e4f38-162">A rács nevű, "Main szabályok" jelennek meg a meglévő aktív és inaktív szabályok a tűzfalon.</span><span class="sxs-lookup"><span data-stu-id="e4f38-162">A grid called, “Main Rules” will show the existing active and deactivated rules on the firewall.</span></span> <span data-ttu-id="e4f38-163">A rács jobb felső sarkában a kisméretű, zöld "+" gombra, ide kattintva hozzon létre egy új szabályt (Megjegyzés: a tűzfal "zárolva" a változásokat, ha a gomb "Lock" megjelölve, és nem tudja létrehozni vagy szabályok szerkesztése, kattintson erre a gombra kattintva "zárolásának feloldásához" a ruleset és Szerkesztés engedélyezése).</span><span class="sxs-lookup"><span data-stu-id="e4f38-163">In the upper right corner of this grid is a small, green “+” button, click this to create a new rule (Note: your firewall may be “locked” for changes, if you see a button marked “Lock” and you are unable to create or edit rules, click this button to “unlock” the ruleset and allow editing).</span></span> <span data-ttu-id="e4f38-164">Meglévő szabály szerkesztése, válassza ki, hogy a szabály, kattintson a jobb gombbal és válassza ki a szabály szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="e4f38-164">If you wish to edit an existing rule, select that rule, right-click and select Edit Rule.</span></span>

<span data-ttu-id="e4f38-165">Hozzon létre egy új szabályt, és adjon meg egy nevet, például a "WebTraffic".</span><span class="sxs-lookup"><span data-stu-id="e4f38-165">Create a new rule and provide a name, such as "WebTraffic".</span></span> 

<span data-ttu-id="e4f38-166">A cél NAT-szabály ikon néz ki: ![cél NAT ikon][2]</span><span class="sxs-lookup"><span data-stu-id="e4f38-166">The Destination NAT rule icon looks like this: ![Destination NAT Icon][2]</span></span>

<span data-ttu-id="e4f38-167">A szabály maga hasonló ehhez hasonló lenne:</span><span class="sxs-lookup"><span data-stu-id="e4f38-167">The rule itself would look something like this:</span></span>

<span data-ttu-id="e4f38-168">![Tűzfalszabály][3]</span><span class="sxs-lookup"><span data-stu-id="e4f38-168">![Firewall Rule][3]</span></span>

<span data-ttu-id="e4f38-169">Itt a bejövő címet, amely a tűzfal találatok elérni HTTP (80-as vagy a HTTPS-hez a 443-as port) fog kell elküldését a tűzfal "DHCP1 helyi IP-címe" illesztőfelület és átirányítja a Web Server 10.0.1.5 IP-címét.</span><span class="sxs-lookup"><span data-stu-id="e4f38-169">Here any inbound address that hits the Firewall trying to reach HTTP (port 80 or 443 for HTTPS) will be sent out the Firewall’s “DHCP1 Local IP” interface and redirected to the Web Server with the IP Address of 10.0.1.5.</span></span> <span data-ttu-id="e4f38-170">Mivel a forgalom 80-as porton várható, és továbblép a webkiszolgálónak a 80-as port nem port módosítása volt szükség.</span><span class="sxs-lookup"><span data-stu-id="e4f38-170">Since the traffic is coming in on port 80 and going to the web server on port 80 no port change was needed.</span></span> <span data-ttu-id="e4f38-171">Azonban a cél lista sikerült lett 10.0.1.5:8080 Ha így fordítása a bejövő 80-as port a tűzfalon a webkiszolgálón a 8080-as a bejövő portot a 8080-as porton figyel a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e4f38-171">However, the Target List could have been 10.0.1.5:8080 if our Web Server listened on port 8080 thus translating the inbound port 80 on the firewall to inbound port 8080 on the web server.</span></span>

<span data-ttu-id="e4f38-172">A kapcsolódás módját kell is lehet szereplő, az internetről, a cél szabály "Dinamikus SNAT" legmegfelelőbb.</span><span class="sxs-lookup"><span data-stu-id="e4f38-172">A Connection Method should also be signified, for the Destination Rule from the Internet, "Dynamic SNAT" is most appropriate.</span></span> 

<span data-ttu-id="e4f38-173">Habár egyetlen szabály jött létre fontos, hogy helyesen van-e állítva a hozzá tartozó prioritás.</span><span class="sxs-lookup"><span data-stu-id="e4f38-173">Although only one rule has been created it's important that its priority is set correctly.</span></span> <span data-ttu-id="e4f38-174">Ha a szabályok a tűzfalon az új szabály van alján (alatt a "BLOCKALL" szabály) a rácsban soha nem érkezni fog szerepet.</span><span class="sxs-lookup"><span data-stu-id="e4f38-174">If in the grid of all rules on the firewall this new rule is on the bottom (below the "BLOCKALL" rule) it will never come into play.</span></span> <span data-ttu-id="e4f38-175">Győződjön meg arról, a webes forgalomban az újonnan létrehozott szabály a BLOCKALL szabály felett van.</span><span class="sxs-lookup"><span data-stu-id="e4f38-175">Ensure the newly created rule for web traffic is above the BLOCKALL rule.</span></span>

<span data-ttu-id="e4f38-176">Ha a szabály jön létre, kell leküldeni a tűzfalat, és majd aktiválni, ha ez nem történik a szabály módosítása nem lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="e4f38-176">Once the rule is created, it must be pushed to the firewall and then activated, if this is not done the rule change will not take effect.</span></span> <span data-ttu-id="e4f38-177">A leküldéses és az aktiválási folyamat a következő szakaszban ismertetett.</span><span class="sxs-lookup"><span data-stu-id="e4f38-177">The push and activation process is described in the next section.</span></span>

## <a name="rule-activation"></a><span data-ttu-id="e4f38-178">A szabály aktiválás</span><span class="sxs-lookup"><span data-stu-id="e4f38-178">Rule Activation</span></span>
<span data-ttu-id="e4f38-179">A ruleset módosíthatja, hogy ez a szabály hozzáadása, az a ruleset kell feltölthetők a tűzfalhoz és aktiválva.</span><span class="sxs-lookup"><span data-stu-id="e4f38-179">With the ruleset modified to add this rule, the ruleset must be uploaded to the firewall and activated.</span></span>

<span data-ttu-id="e4f38-180">![Tűzfal szabály aktiválás][4]</span><span class="sxs-lookup"><span data-stu-id="e4f38-180">![Firewall Rule Activation][4]</span></span>

<span data-ttu-id="e4f38-181">A felügyeleti ügyfél jobb felső sarkában található egy fürt gombok vannak.</span><span class="sxs-lookup"><span data-stu-id="e4f38-181">In the upper right hand corner of the management client are a cluster of buttons.</span></span> <span data-ttu-id="e4f38-182">A módosított szabályok és a tűzfalon küldendő a "Küldési módosítások" gombra, majd kattintson az "Aktiválás" gombra.</span><span class="sxs-lookup"><span data-stu-id="e4f38-182">Click the “Send Changes” button to send the modified rules to the firewall, then click the “Activate” button.</span></span>

<span data-ttu-id="e4f38-183">Az az aktiválás, a tűzfal szabálykészletben ez példa környezet build befejeződött.</span><span class="sxs-lookup"><span data-stu-id="e4f38-183">With the activation of the firewall ruleset this example environment build is complete.</span></span> <span data-ttu-id="e4f38-184">Szükség esetén a References szakaszában, a feladás egy vagy több build parancsfájlok futtatásával hozzáadhat egy alkalmazást ebben a környezetben tesztelheti a forgalom forgatókönyvek alatt.</span><span class="sxs-lookup"><span data-stu-id="e4f38-184">Optionally, the post build scripts in the References section can be run to add an application to this environment to test the below traffic scenarios.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e4f38-185">Nagyon fontos, hogy rendszer nem kattint a webkiszolgáló közvetlenül megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="e4f38-185">It is critical to realize that you will not hit the web server directly.</span></span> <span data-ttu-id="e4f38-186">Ha egy böngészőben kér FrontEnd001.CloudApp.Net egy HTTP-lap, a HTTP-végpont (80-as port) továbbítja a forgalmat a tűzfal nem a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e4f38-186">When a browser requests an HTTP page from FrontEnd001.CloudApp.Net, the HTTP endpoint (port 80) passes this traffic to the firewall not the web server.</span></span> <span data-ttu-id="e4f38-187">A tűzfal, majd a szabály megfelelően a fenti létrehozott, a webkiszolgáló kérő NAT.</span><span class="sxs-lookup"><span data-stu-id="e4f38-187">The firewall then, according to the rule created above, NATs that request to the Web Server.</span></span>
> 
> 

## <a name="traffic-scenarios"></a><span data-ttu-id="e4f38-188">Forgalom forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="e4f38-188">Traffic Scenarios</span></span>
#### <a name="allowed-web-to-web-server-through-firewall"></a><span data-ttu-id="e4f38-189">(Engedélyezett) Webes a webkiszolgáló tűzfalon keresztül</span><span class="sxs-lookup"><span data-stu-id="e4f38-189">(Allowed) Web to Web Server through Firewall</span></span>
1. <span data-ttu-id="e4f38-190">Internetes felhasználói kérelmek HTTP oldal FrontEnd001.CloudApp.Net (Internet Facing Felhőszolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="e4f38-190">Internet user requests HTTP page from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="e4f38-191">Felhőalapú szolgáltatás fázisok forgalmat a 80-as port a helyi tűzfal megfelelő felületéről 10.0.1.4:80 nyitott végpontok keresztül</span><span class="sxs-lookup"><span data-stu-id="e4f38-191">Cloud service passes traffic through open endpoint on port 80 to firewall local interface on 10.0.1.4:80</span></span>
3. <span data-ttu-id="e4f38-192">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="e4f38-192">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e4f38-193">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="e4f38-193">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="e4f38-194">NSG szabály 2 (RDP) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="e4f38-194">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="e4f38-195">NSG 3. szabály (Internet tűzfalhoz) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="e4f38-195">NSG Rule 3 (Internet to Firewall) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="e4f38-196">Forgalom találatok belső IP-címet a tűzfal (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="e4f38-196">Traffic hits internal IP address of the firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="e4f38-197">Továbbító tűzfalszabály tekintse meg a 80-as portot a forgalom, irányítja át a webkiszolgáló IIS01</span><span class="sxs-lookup"><span data-stu-id="e4f38-197">Firewall forwarding rule see this is port 80 traffic, redirects it to the web server IIS01</span></span>
6. <span data-ttu-id="e4f38-198">IIS01 figyeli a webes forgalom, ezt a kérelmet kap, és elindítja a kérés feldolgozása</span><span class="sxs-lookup"><span data-stu-id="e4f38-198">IIS01 is listening for web traffic, receives this request and starts processing the request</span></span>
7. <span data-ttu-id="e4f38-199">IIS01 az SQL Server a AppVM01 adatokat kéri</span><span class="sxs-lookup"><span data-stu-id="e4f38-199">IIS01 asks the SQL Server on AppVM01 for information</span></span>
8. <span data-ttu-id="e4f38-200">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="e4f38-200">No outbound rules on Frontend subnet, traffic is allowed</span></span>
9. <span data-ttu-id="e4f38-201">A Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="e4f38-201">The Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e4f38-202">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="e4f38-202">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="e4f38-203">NSG szabály 2 (RDP) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="e4f38-203">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="e4f38-204">NSG 3. szabály (Internet tűzfalhoz) nem teljesül, a közvetkező szabályának áthelyezése</span><span class="sxs-lookup"><span data-stu-id="e4f38-204">NSG Rule 3 (Internet to Firewall) doesn’t apply, move to next rule</span></span>
   4. <span data-ttu-id="e4f38-205">NSG 4. szabály (a AppVM01 IIS01) teljesül, a forgalom engedélyezve van, akkor állítsa le a szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="e4f38-205">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
10. <span data-ttu-id="e4f38-206">AppVM01 dokumentálásáért és az SQL-lekérdezést kap</span><span class="sxs-lookup"><span data-stu-id="e4f38-206">AppVM01 receives the SQL Query and responds</span></span>
11. <span data-ttu-id="e4f38-207">A válasz engedélyezett, mert nincsenek meg a Backend alhálózathoz kimenő szabályok</span><span class="sxs-lookup"><span data-stu-id="e4f38-207">Since there are no outbound rules on the Backend subnet the response is allowed</span></span>
12. <span data-ttu-id="e4f38-208">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="e4f38-208">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="e4f38-209">Nincs érvényes bejövő szabály NSG a Frontend alhálózathoz, így nincs az NSG-szabályok vonatkoznak a Backend alhálózathoz-forgalom</span><span class="sxs-lookup"><span data-stu-id="e4f38-209">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
    2. <span data-ttu-id="e4f38-210">A alapértelmezett rendszerszintű szabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, a forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="e4f38-210">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
13. <span data-ttu-id="e4f38-211">Az IIS-kiszolgálót az SQL-válasz fogadása és a HTTP-válasz befejezése és a kérelmező küld</span><span class="sxs-lookup"><span data-stu-id="e4f38-211">The IIS server receives the SQL response and completes the HTTP response and sends to the requestor</span></span>
14. <span data-ttu-id="e4f38-212">Mivel ez a tűzfal NAT munkamenetének, a válasz a cél (kezdetben) nem a tűzfal</span><span class="sxs-lookup"><span data-stu-id="e4f38-212">Since this is a NAT session from the firewall, the response destination (initially) is for the Firewall</span></span>
15. <span data-ttu-id="e4f38-213">A válasz fogadása a webkiszolgáló a tűzfalat, és továbbítja azokat a Internet felhasználónak</span><span class="sxs-lookup"><span data-stu-id="e4f38-213">The firewall receives the response from the Web Server and forwards back to the Internet User</span></span>
16. <span data-ttu-id="e4f38-214">Mivel nincsenek a Frontend alhálózaton a válasz nem kimenő szabályok engedélyezett, és az internetes felhasználói kap, a kért weblap.</span><span class="sxs-lookup"><span data-stu-id="e4f38-214">Since there are no outbound rules on the Frontend subnet the response is allowed, and the Internet User receives the web page requested.</span></span>

#### <a name="allowed-rdp-to-backend"></a><span data-ttu-id="e4f38-215">(Engedélyezett) RDP háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="e4f38-215">(Allowed) RDP to Backend</span></span>
1. <span data-ttu-id="e4f38-216">Server Admin interneten kérelmek AppVM01 BackEnd001.CloudApp.Net:xxxxx, ahol xxxxx az RDP számára (a hozzárendelt port található az Azure-portál vagy a PowerShell segítségével) AppVM01 véletlenszerűen hozzárendelt portszámot az RDP-munkamenetet</span><span class="sxs-lookup"><span data-stu-id="e4f38-216">Server Admin on internet requests RDP session to AppVM01 on BackEnd001.CloudApp.Net:xxxxx where xxxxx is the randomly assigned port number for RDP to AppVM01 (the assigned port can be found on the Azure Portal or via PowerShell)</span></span>
2. <span data-ttu-id="e4f38-217">A tűzfalon csak a FrontEnd001.CloudApp.Net címet figyeli, mivel nem részt vesz a forgalom áramlását az</span><span class="sxs-lookup"><span data-stu-id="e4f38-217">Since the Firewall is only listening on the FrontEnd001.CloudApp.Net address, it is not involved with this traffic flow</span></span>
3. <span data-ttu-id="e4f38-218">Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="e4f38-218">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e4f38-219">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="e4f38-219">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="e4f38-220">NSG szabály 2 (RDP) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="e4f38-220">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="e4f38-221">A nem kimenő szabályokat alapértelmezett szabályokat alkalmazni, és a forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="e4f38-221">With no outbound rules, default rules apply and return traffic is allowed</span></span>
5. <span data-ttu-id="e4f38-222">RDP-munkamenetbe engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="e4f38-222">RDP session is enabled</span></span>
6. <span data-ttu-id="e4f38-223">Felhasználói nevének jelszavának megadását kéri az AppVM01</span><span class="sxs-lookup"><span data-stu-id="e4f38-223">AppVM01 prompts for user name password</span></span>

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a><span data-ttu-id="e4f38-224">(Engedélyezett) Web Server DNS-címkeresés DNS-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="e4f38-224">(Allowed) Web Server DNS lookup on DNS server</span></span>
1. <span data-ttu-id="e4f38-225">Webalkalmazás-kiszolgálón, IIS01, egy adatcsatorna www.data.gov: igényeinek, de a címek feloldására igényeinek.</span><span class="sxs-lookup"><span data-stu-id="e4f38-225">Web Server, IIS01, needs a data feed at www.data.gov, but needs to resolve the address.</span></span>
2. <span data-ttu-id="e4f38-226">A hálózati konfigurációt a VNet listák DNS01 (a háttér alhálózaton 10.0.2.4) elsődleges DNS-kiszolgálóként, IIS01 küld a DNS-kérelem DNS01</span><span class="sxs-lookup"><span data-stu-id="e4f38-226">The network configuration for the VNet lists DNS01 (10.0.2.4 on the Backend subnet) as the primary DNS server, IIS01 sends the DNS request to DNS01</span></span>
3. <span data-ttu-id="e4f38-227">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="e4f38-227">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="e4f38-228">Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="e4f38-228">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e4f38-229">NSG szabály 1 (DNS) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="e4f38-229">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="e4f38-230">DNS-kiszolgáló a kérelmet kap.</span><span class="sxs-lookup"><span data-stu-id="e4f38-230">DNS server receives the request</span></span>
6. <span data-ttu-id="e4f38-231">DNS-kiszolgáló nem rendelkezik a gyorsítótárazott címmel és egy legfelső szintű DNS-kiszolgáló kéri az interneten</span><span class="sxs-lookup"><span data-stu-id="e4f38-231">DNS server doesn’t have the address cached and asks a root DNS server on the internet</span></span>
7. <span data-ttu-id="e4f38-232">Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="e4f38-232">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="e4f38-233">Internetes DNS-kiszolgáló válaszol, mivel ehhez a munkamenethez belső kezdeményezett, a válasz engedélyezett</span><span class="sxs-lookup"><span data-stu-id="e4f38-233">Internet DNS server responds, since this session was initiated internally, the response is allowed</span></span>
9. <span data-ttu-id="e4f38-234">DNS-kiszolgáló gyorsítótárazza a választ, és a kezdeti kérés vissza IIS01 válaszol</span><span class="sxs-lookup"><span data-stu-id="e4f38-234">DNS server caches the response, and responds to the initial request back to IIS01</span></span>
10. <span data-ttu-id="e4f38-235">Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="e4f38-235">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="e4f38-236">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="e4f38-236">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="e4f38-237">Nincs érvényes bejövő szabály NSG a Frontend alhálózathoz, így nincs az NSG-szabályok vonatkoznak a Backend alhálózathoz-forgalom</span><span class="sxs-lookup"><span data-stu-id="e4f38-237">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
    2. <span data-ttu-id="e4f38-238">A alapértelmezett rendszerszintű szabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így a forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="e4f38-238">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed</span></span>
12. <span data-ttu-id="e4f38-239">IIS01 megkapja válaszát DNS01</span><span class="sxs-lookup"><span data-stu-id="e4f38-239">IIS01 receives the response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="e4f38-240">(Engedélyezett) Server-hozzáférés fájl AppVM01</span><span class="sxs-lookup"><span data-stu-id="e4f38-240">(Allowed) Web Server access file on AppVM01</span></span>
1. <span data-ttu-id="e4f38-241">IIS01 kér AppVM01 fájlba</span><span class="sxs-lookup"><span data-stu-id="e4f38-241">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="e4f38-242">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="e4f38-242">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="e4f38-243">A Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="e4f38-243">The Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e4f38-244">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="e4f38-244">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="e4f38-245">NSG szabály 2 (RDP) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="e4f38-245">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="e4f38-246">NSG 3. szabály (Internet tűzfalhoz) nem teljesül, a közvetkező szabályának áthelyezése</span><span class="sxs-lookup"><span data-stu-id="e4f38-246">NSG Rule 3 (Internet to Firewall) doesn’t apply, move to next rule</span></span>
   4. <span data-ttu-id="e4f38-247">NSG 4. szabály (a AppVM01 IIS01) teljesül, a forgalom engedélyezve van, akkor állítsa le a szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="e4f38-247">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="e4f38-248">AppVM01 a kérelmet kap, és válaszol, a fájl (feltéve, hogy van engedélyezve)</span><span class="sxs-lookup"><span data-stu-id="e4f38-248">AppVM01 receives the request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="e4f38-249">A válasz engedélyezett, mert nincsenek meg a Backend alhálózathoz kimenő szabályok</span><span class="sxs-lookup"><span data-stu-id="e4f38-249">Since there are no outbound rules on the Backend subnet the response is allowed</span></span>
6. <span data-ttu-id="e4f38-250">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="e4f38-250">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e4f38-251">Nincs érvényes bejövő szabály NSG a Frontend alhálózathoz, így nincs az NSG-szabályok vonatkoznak a Backend alhálózathoz-forgalom</span><span class="sxs-lookup"><span data-stu-id="e4f38-251">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
   2. <span data-ttu-id="e4f38-252">A alapértelmezett rendszerszintű szabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, a forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="e4f38-252">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
7. <span data-ttu-id="e4f38-253">Az IIS-kiszolgálót kap a fájl</span><span class="sxs-lookup"><span data-stu-id="e4f38-253">The IIS server receives the file</span></span>

#### <a name="denied-web-direct-to-web-server"></a><span data-ttu-id="e4f38-254">(Megtagadva) Közvetlenül a webkiszolgálón webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="e4f38-254">(Denied) Web direct to Web Server</span></span>
<span data-ttu-id="e4f38-255">Mivel a webkiszolgálón, IIS01 és a tűzfalon az azonos felhőalapú szolgáltatás nyilvános felé néző IP-cím-os osztoznak.</span><span class="sxs-lookup"><span data-stu-id="e4f38-255">Since the Web Server, IIS01, and the Firewall are in the same Cloud Service they share the same public facing IP address.</span></span> <span data-ttu-id="e4f38-256">Így minden HTTP-forgalom volna átirányítja a tűzfalon.</span><span class="sxs-lookup"><span data-stu-id="e4f38-256">Thus any HTTP traffic would be directed to the firewall.</span></span> <span data-ttu-id="e4f38-257">A kérelem sikeresen kiszolgálta lenne, amíg nem tud közvetlenül lépni a webkiszolgáló, az átadott, úgy tervezték, a tűzfalon keresztül először.</span><span class="sxs-lookup"><span data-stu-id="e4f38-257">While the request would be successfully served, it cannot go directly to the Web Server, it passed, as designed, through the Firewall first.</span></span> <span data-ttu-id="e4f38-258">Ebben a szakaszban a forgalom áramlását az első forgatókönyvben talál.</span><span class="sxs-lookup"><span data-stu-id="e4f38-258">See the first Scenario in this section for the traffic flow.</span></span>

#### <a name="denied-web-to-backend-server"></a><span data-ttu-id="e4f38-259">(Megtagadva) Webes háttérkiszolgálóra</span><span class="sxs-lookup"><span data-stu-id="e4f38-259">(Denied) Web to Backend Server</span></span>
1. <span data-ttu-id="e4f38-260">Internetes felhasználó megpróbál hozzáférni egy fájl AppVM01 a BackEnd001.CloudApp.Net szolgáltatáson keresztül</span><span class="sxs-lookup"><span data-stu-id="e4f38-260">Internet user tries to access a file on AppVM01 through the BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="e4f38-261">Mivel nincsenek nyissa meg a fájlmegosztás nincsenek végpontok, ez nem lenne továbbítja a felhőalapú szolgáltatás, és így elérni a kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="e4f38-261">Since there are no endpoints open for file share, this would not pass the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="e4f38-262">Ha valamilyen okból a végpontok nyitott, NSG szabály 5 (Internet virtuális hálózatba) blokkolná-e a forgalom</span><span class="sxs-lookup"><span data-stu-id="e4f38-262">If the endpoints were open for some reason, NSG rule 5 (Internet to VNet) would block this traffic</span></span>

#### <a name="denied-web-dns-lookup-on-dns-server"></a><span data-ttu-id="e4f38-263">(Megtagadva) Webes DNS-címkeresés DNS-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="e4f38-263">(Denied) Web DNS lookup on DNS server</span></span>
1. <span data-ttu-id="e4f38-264">Internetes felhasználó megpróbálja megkeresni az egy belső DNS-rekordot a DNS01 a BackEnd001.CloudApp.Net szolgáltatáson keresztül</span><span class="sxs-lookup"><span data-stu-id="e4f38-264">Internet user tries to lookup an internal DNS record on DNS01 through the BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="e4f38-265">Mivel nincsenek végpontok nyissa meg a DNS, ez nem lenne továbbítja a felhőalapú szolgáltatás, és így elérni a kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="e4f38-265">Since there are no endpoints open for DNS, this would not pass the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="e4f38-266">Ha valamilyen okból a végpontok nyitott, NSG szabály 5 (Internet virtuális hálózatba) blokkolná-e a forgalom (Megjegyzés: csak akkor nem vonatkozik két okból szabály 1 (DNS), először a forrás címe az interneten, ez a szabály csak a helyi virtuális hálózat, mint a forrás vonatkozik Ez a egy olyan engedélyezési szabály, akkor soha nem letiltsa a forgalom)</span><span class="sxs-lookup"><span data-stu-id="e4f38-266">If the endpoints were open for some reason, NSG rule 5 (Internet to VNet) would block this traffic (Note: that Rule 1 (DNS) would not apply for two reasons, first the source address is the internet, this rule only applies to the local VNet as the source, also this is an Allow rule, so it would never deny traffic)</span></span>

#### <a name="denied-web-to-sql-access-through-firewall"></a><span data-ttu-id="e4f38-267">(Megtagadva) Webes SQL-hozzáférés tűzfalon keresztül</span><span class="sxs-lookup"><span data-stu-id="e4f38-267">(Denied) Web to SQL access through Firewall</span></span>
1. <span data-ttu-id="e4f38-268">Internetes felhasználó SQL adatokat kér a FrontEnd001.CloudApp.Net (Internet Facing Felhőszolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="e4f38-268">Internet user requests SQL data from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="e4f38-269">Mivel nincsenek végpontok nyissa meg az SQL, ez nem lenne továbbítja a felhőalapú szolgáltatás, és a tűzfal nem tudnák elérni</span><span class="sxs-lookup"><span data-stu-id="e4f38-269">Since there are no endpoints open for SQL, this would not pass the Cloud Service and wouldn’t reach the firewall</span></span>
3. <span data-ttu-id="e4f38-270">Ha valamilyen okból végpontok voltak nyitva, a Frontend alhálózathoz megkezdi a bejövő szabály feldolgozása:</span><span class="sxs-lookup"><span data-stu-id="e4f38-270">If endpoints were open for some reason, the Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="e4f38-271">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="e4f38-271">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="e4f38-272">NSG szabály 2 (RDP) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="e4f38-272">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="e4f38-273">NSG 2. szabály (Internet tűzfalhoz) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="e4f38-273">NSG Rule 2 (Internet to Firewall) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="e4f38-274">Forgalom találatok belső IP-címet a tűzfal (10.0.1.4)</span><span class="sxs-lookup"><span data-stu-id="e4f38-274">Traffic hits internal IP address of the firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="e4f38-275">Tűzfal SQL nincs továbbítási szabályaik rendelkezik, és a forgalom esik</span><span class="sxs-lookup"><span data-stu-id="e4f38-275">Firewall has no forwarding rules for SQL and drops the traffic</span></span>

## <a name="conclusion"></a><span data-ttu-id="e4f38-276">Összegzés</span><span class="sxs-lookup"><span data-stu-id="e4f38-276">Conclusion</span></span>
<span data-ttu-id="e4f38-277">Ez viszonylag egyszerű módja az alkalmazás egy tűzfal védi, és a háttér-alhálózathoz, a bejövő forgalom elkülönítésére.</span><span class="sxs-lookup"><span data-stu-id="e4f38-277">This is a relatively straight forward way of protecting your application with a firewall and isolating the back end subnet from inbound traffic.</span></span>

<span data-ttu-id="e4f38-278">További példákat és a hálózati biztonsági határokat egy áttekintést talál [Itt][HOME].</span><span class="sxs-lookup"><span data-stu-id="e4f38-278">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="e4f38-279">Referencia</span><span class="sxs-lookup"><span data-stu-id="e4f38-279">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="e4f38-280">Fő parancsfájlt és a hálózati konfiguráció</span><span class="sxs-lookup"><span data-stu-id="e4f38-280">Main Script and Network Config</span></span>
<span data-ttu-id="e4f38-281">A teljes parancsfájl menthető egy olyan PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="e4f38-281">Save the Full Script in a PowerShell script file.</span></span> <span data-ttu-id="e4f38-282">A hálózati konfiguráció mentése "NetworkConf2.xml" fájlba.</span><span class="sxs-lookup"><span data-stu-id="e4f38-282">Save the Network Config into a file named “NetworkConf2.xml”.</span></span>
<span data-ttu-id="e4f38-283">Szükség szerint módosítsa a felhasználó által definiált változókat.</span><span class="sxs-lookup"><span data-stu-id="e4f38-283">Modify the user defined variables as needed.</span></span> <span data-ttu-id="e4f38-284">Futtassa a parancsfájlt, majd kövesse a tűzfal szabály beállítása a fenti.</span><span class="sxs-lookup"><span data-stu-id="e4f38-284">Run the script, then follow the Firewall rule setup instruction above.</span></span>

#### <a name="full-script"></a><span data-ttu-id="e4f38-285">Teljes körű parancsfájl</span><span class="sxs-lookup"><span data-stu-id="e4f38-285">Full Script</span></span>
<span data-ttu-id="e4f38-286">Ez a parancsfájl lesz, a felhasználó által definiált változóknak alapján:</span><span class="sxs-lookup"><span data-stu-id="e4f38-286">This script will, based on the user defined variables:</span></span>

1. <span data-ttu-id="e4f38-287">Csatlakozás Azure-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="e4f38-287">Connect to an Azure subscription</span></span>
2. <span data-ttu-id="e4f38-288">Új tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4f38-288">Create a new storage account</span></span>
3. <span data-ttu-id="e4f38-289">Hozzon létre egy új virtuális hálózat és a hálózati konfigurációs fájljában definiált két alhálózat</span><span class="sxs-lookup"><span data-stu-id="e4f38-289">Create a new VNet and two subnets as defined in the Network Config file</span></span>
4. <span data-ttu-id="e4f38-290">4 a windows server virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4f38-290">Build 4 windows server VMs</span></span>
5. <span data-ttu-id="e4f38-291">Adja meg, beleértve az NSG:</span><span class="sxs-lookup"><span data-stu-id="e4f38-291">Configure NSG including:</span></span>
   * <span data-ttu-id="e4f38-292">Egy NSG létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4f38-292">Creating a NSG</span></span>
   * <span data-ttu-id="e4f38-293">Azt a szabályoknak feltöltése</span><span class="sxs-lookup"><span data-stu-id="e4f38-293">Populating it with rules</span></span>
   * <span data-ttu-id="e4f38-294">Az NSG kötése a megfelelő alhálózatokat</span><span class="sxs-lookup"><span data-stu-id="e4f38-294">Binding the NSG to the appropriate subnets</span></span>

<span data-ttu-id="e4f38-295">A PowerShell parancsfájl fusson helyben egy internethez csatlakoztatott PC vagy a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="e4f38-295">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e4f38-296">Ezt a parancsfájlt, amikor előfordulhat figyelmeztetések vagy a PowerShellben pop más tájékoztató üzeneteit.</span><span class="sxs-lookup"><span data-stu-id="e4f38-296">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="e4f38-297">Piros csak hibaüzenetek aggodalomra.</span><span class="sxs-lookup"><span data-stu-id="e4f38-297">Only error messages in red are cause for concern.</span></span>
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared

      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       myFirewall - 10.0.1.4
       IIS01      - 10.0.1.5

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.

        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"

        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }

    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *

        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a><span data-ttu-id="e4f38-298">Hálózati konfigurációs fájl</span><span class="sxs-lookup"><span data-stu-id="e4f38-298">Network Config File</span></span>
<span data-ttu-id="e4f38-299">Az XML-fájl mentése frissített hellyel rendelkező, és a hivatkozás hozzáadása ehhez a fájlhoz a $NetworkConfigFile változó a parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="e4f38-299">Save this xml file with updated location and add the link to this file to the $NetworkConfigFile variable in the script above.</span></span>

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a><span data-ttu-id="e4f38-300">Alkalmazás mintaparancsfájlok</span><span class="sxs-lookup"><span data-stu-id="e4f38-300">Sample Application Scripts</span></span>
<span data-ttu-id="e4f38-301">Szeretne telepíteni egy mintaalkalmazás ezzel és más DMZ példák, ha egy adtak meg, a következő hivatkozásra: [minta alkalmazás-parancsfájl][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="e4f38-301">If you wish to install a sample application for this, and other DMZ Examples, one has been provided at the following link: [Sample Application Script][SampleApp]</span></span>

<!--Image References-->
<span data-ttu-id="e4f38-302">[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Az NSG bejövő DMZ"</span><span class="sxs-lookup"><span data-stu-id="e4f38-302">[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Inbound DMZ with NSG"</span></span>
<span data-ttu-id="e4f38-303">[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Cél NAT ikon"</span><span class="sxs-lookup"><span data-stu-id="e4f38-303">[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Destination NAT Icon"</span></span>
<span data-ttu-id="e4f38-304">[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Tűzfalszabály"</span><span class="sxs-lookup"><span data-stu-id="e4f38-304">[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Firewall Rule"</span></span>
<span data-ttu-id="e4f38-305">[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Tűzfal szabály aktiválás"</span><span class="sxs-lookup"><span data-stu-id="e4f38-305">[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Firewall Rule Activation"</span></span>

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
