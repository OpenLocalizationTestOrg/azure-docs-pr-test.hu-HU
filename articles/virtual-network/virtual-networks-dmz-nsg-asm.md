---
title: "Az Azure DMZ példa – Build az NSG-ket egy egyszerű DMZ |} Microsoft Docs"
description: "A hálózati biztonsági csoportokkal (NSG) DMZ összeállítása"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: f8622b1d-c07d-4ea6-b41c-4ae98d998fff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: ed172d552e1e4c9ee27c58abcd7ad2d98df21579
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-classic-powershell"></a><span data-ttu-id="991f7-103">1 – például egy egyszerű DMZ NSG-k használata a klasszikus PowerShell összeállítása</span><span class="sxs-lookup"><span data-stu-id="991f7-103">Example 1 – Build a simple DMZ using NSGs with classic PowerShell</span></span>
<span data-ttu-id="991f7-104">[A biztonsági határ bevált gyakorlatok laphoz való visszatéréshez][HOME]</span><span class="sxs-lookup"><span data-stu-id="991f7-104">[Return to the Security Boundary Best Practices Page][HOME]</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="991f7-105">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="991f7-105">Resource Manager Template</span></span>](virtual-networks-dmz-nsg.md)
> * [<span data-ttu-id="991f7-106">Klasszikus - PowerShell</span><span class="sxs-lookup"><span data-stu-id="991f7-106">Classic - PowerShell</span></span>](virtual-networks-dmz-nsg-asm.md)
> 
>

<span data-ttu-id="991f7-107">Ebben a példában egy egyszerű DMZ négy Windows-kiszolgálók és hálózati biztonsági csoportokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="991f7-107">This example creates a primitive DMZ with four Windows servers and Network Security Groups.</span></span> <span data-ttu-id="991f7-108">Ez a példa bemutatja az egyes bemutatják az egyes lépések, így a megfelelő PowerShell-parancsokat.</span><span class="sxs-lookup"><span data-stu-id="991f7-108">This example describes each of the relevant PowerShell commands to provide a deeper understanding of each step.</span></span> <span data-ttu-id="991f7-109">Is van a forgalom forgatókönyv részt, hogy egy részletesebb lépésenkénti hogyan forgalom halad keresztül a Szegélyhálózaton lévő védelmi réteget.</span><span class="sxs-lookup"><span data-stu-id="991f7-109">There is also a Traffic Scenario section to provide an in-depth step-by-step how traffic proceeds through the layers of defense in the DMZ.</span></span> <span data-ttu-id="991f7-110">Végezetül a hivatkozások szakasz: a teljes kód látható, míg utasítás ebben a környezetben, teszteléséhez, és a különböző forgatókönyvekben kísérletezhet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="991f7-110">Finally, in the references section is the complete code and instruction to build this environment to test and experiment with various scenarios.</span></span> 

<span data-ttu-id="991f7-111">![Az NSG bejövő DMZ][1]</span><span class="sxs-lookup"><span data-stu-id="991f7-111">![Inbound DMZ with NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="991f7-112">Környezet leírása</span><span class="sxs-lookup"><span data-stu-id="991f7-112">Environment description</span></span>
<span data-ttu-id="991f7-113">Ebben a példában az előfizetés a következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="991f7-113">In this example a subscription contains the following resources:</span></span>

* <span data-ttu-id="991f7-114">A felhőalapú szolgáltatások két: "FrontEnd001" és "BackEnd001"</span><span class="sxs-lookup"><span data-stu-id="991f7-114">Two cloud services: “FrontEnd001” and “BackEnd001”</span></span>
* <span data-ttu-id="991f7-115">A virtuális hálózat, a "CorpNetwork", a két alhálózat; "Előtér" és "Háttér"</span><span class="sxs-lookup"><span data-stu-id="991f7-115">A Virtual Network, “CorpNetwork”, with two subnets; “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="991f7-116">Mindkét alhálózat alkalmazott hálózati biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="991f7-116">A Network Security Group that is applied to both subnets</span></span>
* <span data-ttu-id="991f7-117">A Windows Server egy webalkalmazás-kiszolgáló ("IIS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="991f7-117">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="991f7-118">Két windows Server kiszolgálókon, amelyek megfelelnek az alkalmazás háttérkiszolgálók ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="991f7-118">Two windows servers that represent application back-end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="991f7-119">A Windows server DNS-kiszolgáló ("DNS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="991f7-119">A Windows server that represents a DNS server (“DNS01”)</span></span>

<span data-ttu-id="991f7-120">A references szakaszában egy PowerShell-parancsfájlt az ebben a példában leírt környezetben a legtöbb épülő van.</span><span class="sxs-lookup"><span data-stu-id="991f7-120">In the references section, there is a PowerShell script that builds most of the environment described in this example.</span></span> <span data-ttu-id="991f7-121">A virtuális gépek és virtuális hálózatok, bár a példaként megadott parancsfájlt, által végzett dokumentum nem ismerteti a jelen dokumentum részletesen.</span><span class="sxs-lookup"><span data-stu-id="991f7-121">Building the VMs and Virtual Networks, although are done by the example script, are not described in detail in this document.</span></span> 

<span data-ttu-id="991f7-122">A környezet; létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="991f7-122">To build the environment;</span></span>

1. <span data-ttu-id="991f7-123">Mentse a hálózati konfigurációs XML-fájlt a references szakaszában szereplő (frissítődik nevét, helyét és IP-címeket az adott forgatókönyv esetén)</span><span class="sxs-lookup"><span data-stu-id="991f7-123">Save the network config xml file included in the references section (updated with names, location, and IP addresses to match the given scenario)</span></span>
2. <span data-ttu-id="991f7-124">A felhasználói változók a parancsfájl felel meg a parancsfájl-hoz (előfizetések, service nevét stb.) kell futtatni a környezet frissítése</span><span class="sxs-lookup"><span data-stu-id="991f7-124">Update the user variables in the script to match the environment the script is to be run against (subscriptions, service names, etc.)</span></span>
3. <span data-ttu-id="991f7-125">A parancsfájl végrehajtása a PowerShellben</span><span class="sxs-lookup"><span data-stu-id="991f7-125">Execute the script in PowerShell</span></span>

>[!Note]
><span data-ttu-id="991f7-126">A régió, a PowerShell-parancsfájl esetében meg kell egyeznie a hálózati konfigurációs XML-fájl esetében.</span><span class="sxs-lookup"><span data-stu-id="991f7-126">The region signified in the PowerShell script must match the region signified in the network configuration xml file.</span></span>
>
>

<span data-ttu-id="991f7-127">Miután a parancsfájl futtatása sikeresen további választható lépéseket lehet tenni, és a references szakaszában két parancsfájlok állíthat be a webkiszolgáló és egy egyszerű webes alkalmazás tesztelése a DMZ-konfiguráció engedélyezése az alkalmazások kiszolgálói.</span><span class="sxs-lookup"><span data-stu-id="991f7-127">Once the script runs successfully additional optional steps may be taken, in the references section are two scripts to set up the web server and app server with a simple web application to allow testing with this DMZ configuration.</span></span>

<span data-ttu-id="991f7-128">A következő szakaszokban részletes leírása a hálózati biztonsági csoportok és működésének ehhez a példához a PowerShell parancsfájl sorok keresztül érdekében.</span><span class="sxs-lookup"><span data-stu-id="991f7-128">The following sections provide a detailed description of Network Security Groups and how they function for this example by walking through key lines of the PowerShell script.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="991f7-129">Hálózati biztonsági csoportok (NSG)</span><span class="sxs-lookup"><span data-stu-id="991f7-129">Network Security Groups (NSG)</span></span>
<span data-ttu-id="991f7-130">Az ebben a példában egy NSG-csoport összeállítása és hat szabályokkal majd betölteni.</span><span class="sxs-lookup"><span data-stu-id="991f7-130">For this example, an NSG group is built and then loaded with six rules.</span></span> 

> [!TIP]
> <span data-ttu-id="991f7-131">Általánosságban véve az "Engedélyezés" speciális szabályok először hozzon létre, és majd a általánosabbá "Deny" szabályok utolsó.</span><span class="sxs-lookup"><span data-stu-id="991f7-131">Generally speaking, you should create your specific “Allow” rules first and then the more generic “Deny” rules last.</span></span> <span data-ttu-id="991f7-132">A prioritási megkövetel amelyek szabályokat első értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="991f7-132">The assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="991f7-133">Forgalom kiderül, hogy egy adott szabály vonatkozik, ha nincsenek további szabályok kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="991f7-133">Once traffic is found to apply to a specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="991f7-134">NSG-szabályok egyaránt (az alhálózati szempontjából) a bejövő vagy kimenő irányban is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="991f7-134">NSG rules can apply in either in the inbound or outbound direction (from the perspective of the subnet).</span></span>
> 
> 

<span data-ttu-id="991f7-135">Deklaratív módon a bejövő forgalom beépített folyamatban a következő szabályokat:</span><span class="sxs-lookup"><span data-stu-id="991f7-135">Declaratively, the following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="991f7-136">Belső DNS-forgalom (53-as port) engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="991f7-136">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="991f7-137">RDP-forgalmát (3389-es port) az internetről bármely virtuális géphez engedélyezett</span><span class="sxs-lookup"><span data-stu-id="991f7-137">RDP traffic (port 3389) from the Internet to any VM is allowed</span></span>
3. <span data-ttu-id="991f7-138">HTTP-forgalom (80-as port) az internetről (IIS01) webkiszolgálón engedélyezett</span><span class="sxs-lookup"><span data-stu-id="991f7-138">HTTP traffic (port 80) from the Internet to web server (IIS01) is allowed</span></span>
4. <span data-ttu-id="991f7-139">Minden forgalmat (minden porthoz) IIS01 AppVM1 engedélyezett</span><span class="sxs-lookup"><span data-stu-id="991f7-139">Any traffic (all ports) from IIS01 to AppVM1 is allowed</span></span>
5. <span data-ttu-id="991f7-140">Az összes bejövő forgalom (minden porthoz) az internetről a teljes virtuális hálózatot (alhálózatok mindkét) megtagadva</span><span class="sxs-lookup"><span data-stu-id="991f7-140">Any traffic (all ports) from the Internet to the entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="991f7-141">Az összes bejövő forgalom (minden porthoz) a Frontend alhálózatból a Backend alhálózathoz megtagadva</span><span class="sxs-lookup"><span data-stu-id="991f7-141">Any traffic (all ports) from the Frontend subnet to the Backend subnet is Denied</span></span>

<span data-ttu-id="991f7-142">A következő szabályok kötve alhálózatok, ha a webkiszolgálónak, a szabályokat is 3 az internetről bejövő HTTP-kérelem (engedélyezése) és 5 (megtagadni) szeretné alkalmazni, de mivel a szabály 3 egy magasabb prioritással bír, csak azt csak akkor vonatkozik, és 5 szabály nem lesz play kerülhet.</span><span class="sxs-lookup"><span data-stu-id="991f7-142">With these rules bound to each subnet, if an HTTP request was inbound from the Internet to the web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="991f7-143">Így a HTTP-kérelem szeretné engedélyezni kell a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="991f7-143">Thus the HTTP request would be allowed to the web server.</span></span> <span data-ttu-id="991f7-144">Ha ugyanaz a forgalom próbál elérni az DNS01 kiszolgálót, szabály 5 (Megtagadás) lenne az első alkalmazni, és szeretné, hogy a kiszolgáló nem engedélyezett a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="991f7-144">If that same traffic was trying to reach the DNS01 server, rule 5 (Deny) would be the first to apply and the traffic would not be allowed to pass to the server.</span></span> <span data-ttu-id="991f7-145">6 (Megtagadás) szabály blokkolja a Frontend alhálózathoz van szó, a Backend alhálózathoz (kivéve a engedélyezett forgalom szabályokban 1 és 4) a, a szabálykészlet abban az esetben, ha egy támadó biztonság sérüléseinek előtér, a támadó a webalkalmazás volna korlátozott védelmet nyújt a háttér hálózathoz a háttérrendszer elérésére "védett" hálózaton (csak a AppVM01 kiszolgálón kitett erőforrások).</span><span class="sxs-lookup"><span data-stu-id="991f7-145">Rule 6 (Deny) blocks the Frontend subnet from talking to the Backend subnet (except for allowed traffic in rules 1 and 4), this rule-set protects the Backend network in case an attacker compromises the web application on the Frontend, the attacker would have limited access to the Backend “protected” network (only to resources exposed on the AppVM01 server).</span></span>

<span data-ttu-id="991f7-146">Nincs olyan alapértelmezett kimenő szabály, amely lehetővé teszi, hogy a kimenő forgalom az internethez.</span><span class="sxs-lookup"><span data-stu-id="991f7-146">There is a default outbound rule that allows traffic out to the internet.</span></span> <span data-ttu-id="991f7-147">Ebben a példában a Microsoft most átengedi a kimenő forgalmat, és nem módosítja az egyetlen kimenő szabályok.</span><span class="sxs-lookup"><span data-stu-id="991f7-147">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="991f7-148">Forgalom mindkét irányban zárolását, felhasználói definiált útválasztási szükség, és a"3" a felfedezte van a [biztonsági határ bevált gyakorlatok lap][HOME].</span><span class="sxs-lookup"><span data-stu-id="991f7-148">To lock down traffic in both directions, User Defined Routing is required and is explored in “Example 3” on the [Security Boundary Best Practices Page][HOME].</span></span>

<span data-ttu-id="991f7-149">Minden egyes szabály tárgyalt részletesebben az alábbiak szerint (**Megjegyzés**: valamely elemére a következő lista-tól kezdődően dollárjelet (például: $NSGName) egy felhasználó által definiált változó a hivatkozás szakaszban, a jelen dokumentum a parancsfájlból):</span><span class="sxs-lookup"><span data-stu-id="991f7-149">Each rule is discussed in more detail as follows (**Note**: any item in the following list beginning with a dollar sign (for example: $NSGName) is a user-defined variable from the script in the reference section of this document):</span></span>

1. <span data-ttu-id="991f7-150">Hálózati biztonsági csoport először kell kialakítani, hogy a szabályok tárolására:</span><span class="sxs-lookup"><span data-stu-id="991f7-150">First a Network Security Group must be built to hold the rules:</span></span>

    ```PowerShell
    New-AzureNetworkSecurityGroup -Name $NSGName `
        -Location $DeploymentLocation `
        -Label "Security group for $VNetName subnets in $DeploymentLocation"
    ```

2. <span data-ttu-id="991f7-151">Ebben a példában az első szabály lehetővé teszi, hogy a DNS-forgalom a DNS-kiszolgálót a backend alhálózathoz az összes belső hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="991f7-151">The first rule in this example allows DNS traffic between all internal networks to the DNS server on the backend subnet.</span></span> <span data-ttu-id="991f7-152">A szabály néhány fontos paraméterekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="991f7-152">The rule has some important parameters:</span></span>
   
   * <span data-ttu-id="991f7-153">"Típus" azt jelzi, hogy a forgalom iránya a szabály lép érvénybe.</span><span class="sxs-lookup"><span data-stu-id="991f7-153">“Type” signifies in which direction of traffic flow this rule takes effect.</span></span> <span data-ttu-id="991f7-154">Irányát (attól függően, ahol az NSG kötött) az alhálózatra vagy a virtuális gép szempontjából van.</span><span class="sxs-lookup"><span data-stu-id="991f7-154">The direction is from the perspective of the subnet or Virtual Machine (depending on where this NSG is bound).</span></span> <span data-ttu-id="991f7-155">Így ha típus: "Bejövő" és a forgalom írja be az alhálózat, a szabályt alkalmazni és az alhálózatot elhagyó forgalomra ne befolyásolja az e szabály által.</span><span class="sxs-lookup"><span data-stu-id="991f7-155">Thus if Type is “Inbound” and traffic is entering the subnet, the rule would apply and traffic leaving the subnet would not be affected by this rule.</span></span>
   * <span data-ttu-id="991f7-156">"Prioritás" állítja be, amelyben a forgalom áramlását kiértékelésének.</span><span class="sxs-lookup"><span data-stu-id="991f7-156">“Priority” sets the order in which a traffic flow is evaluated.</span></span> <span data-ttu-id="991f7-157">Minél kisebb számot annál magasabb a prioritás.</span><span class="sxs-lookup"><span data-stu-id="991f7-157">The lower the number the higher the priority.</span></span> <span data-ttu-id="991f7-158">A szabály vonatkozik egy adott adatforgalmat, ha nincsenek további szabályok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="991f7-158">When a rule applies to a specific traffic flow, no further rules are processed.</span></span> <span data-ttu-id="991f7-159">Így ha 1 prioritású szabály lehetővé teszi, hogy a forgalmat, és 2 prioritású szabály megtagadja a forgalmat, és szabályokat is vonatkoznak majd a forgalmat szeretné engedélyezni szeretné a flow (mivel szabály 1 kellett magasabb prioritású hatás tartott, és nincsenek további szabályok alkalmazása megtörtént).</span><span class="sxs-lookup"><span data-stu-id="991f7-159">Thus if a rule with priority 1 allows traffic, and a rule with priority 2 denies traffic, and both rules apply to traffic then the traffic would be allowed to flow (since rule 1 had a higher priority it took effect and no further rules were applied).</span></span>
   * <span data-ttu-id="991f7-160">"Action" azt jelzi, hogy ha a szabály által érintett forgalom vagy tiltott.</span><span class="sxs-lookup"><span data-stu-id="991f7-160">“Action” signifies if traffic affected by this rule is blocked or allowed.</span></span>

    ```PowerShell    
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
        -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[4] `
        -DestinationPortRange '53' `
        -Protocol *
    ```

3. <span data-ttu-id="991f7-161">Ez a szabály lehetővé teszi, hogy az internetről érkező RDP-portjára kötött alhálózaton egyetlen kiszolgálón történő flow RDP-forgalmát.</span><span class="sxs-lookup"><span data-stu-id="991f7-161">This rule allows RDP traffic to flow from the internet to the RDP port on any server on the bound subnet.</span></span> <span data-ttu-id="991f7-162">Ez a szabály címelőtagokat; különleges kétféle használ. "VIRTUAL_NETWORK" és "INTERNET".</span><span class="sxs-lookup"><span data-stu-id="991f7-162">This rule uses two special types of address prefixes; “VIRTUAL_NETWORK” and “INTERNET.”</span></span> <span data-ttu-id="991f7-163">Ezekkel a címkékkel egyszerűen egy nagyobb kategóriáját címelőtag.</span><span class="sxs-lookup"><span data-stu-id="991f7-163">These tags are an easy way to address a larger category of address prefixes.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
         -Type Inbound -Priority 110 -Action Allow `
         -SourceAddressPrefix INTERNET -SourcePortRange '*' `
         -DestinationAddressPrefix VIRTUAL_NETWORK `
         -DestinationPortRange '3389' `
         -Protocol *
    ```

4. <span data-ttu-id="991f7-164">Ez a szabály lehetővé teszi, hogy a webalkalmazás-kiszolgáló elérte a bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="991f7-164">This rule allows inbound internet traffic to hit the web server.</span></span> <span data-ttu-id="991f7-165">Ez a szabály nem változtatja meg az útválasztási viselkedés.</span><span class="sxs-lookup"><span data-stu-id="991f7-165">This rule does not change the routing behavior.</span></span> <span data-ttu-id="991f7-166">A szabály csak lehetővé teszi, hogy a IIS01 továbbítani adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="991f7-166">The rule only allows traffic destined for IIS01 to pass.</span></span> <span data-ttu-id="991f7-167">Így ha az internetről érkező forgalom volt-e a célként, ez a szabály akkor engedélyezi-e, és további szabályok feldolgozásának leállítása a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="991f7-167">Thus if traffic from the Internet had the web server as its destination this rule would allow it and stop processing further rules.</span></span> <span data-ttu-id="991f7-168">(A szabályban prioritással 140 összes egyéb bejövő forgalmat le van tiltva).</span><span class="sxs-lookup"><span data-stu-id="991f7-168">(In the rule at priority 140 all other inbound internet traffic is blocked).</span></span> <span data-ttu-id="991f7-169">Ha csak a HTTP-forgalom éppen feldolgozás, ez a szabály sikertelen további korlátozva csak engedélyezi a 80-as Port cél.</span><span class="sxs-lookup"><span data-stu-id="991f7-169">If you're only processing HTTP traffic, this rule could be further restricted to only allow Destination Port 80.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
         -Type Inbound -Priority 120 -Action Allow `
         -SourceAddressPrefix Internet -SourcePortRange '*' `
         -DestinationAddressPrefix $VMIP[0] `
         -DestinationPortRange '*' `
         -Protocol *
    ```

5. <span data-ttu-id="991f7-170">Ez a szabály lehetővé teszi, hogy a forgalom a IIS01 kiszolgáló számára a AppVM01 kiszolgáló egy újabb szabály blokkolja a háttér-forgalom minden más előtér.</span><span class="sxs-lookup"><span data-stu-id="991f7-170">This rule allows traffic to pass from the IIS01 server to the AppVM01 server, a later rule blocks all other Frontend to Backend traffic.</span></span> <span data-ttu-id="991f7-171">Ez a szabály akkor javíthatja, ha a port ismert, hogy hozzá kell adni.</span><span class="sxs-lookup"><span data-stu-id="991f7-171">To improve this rule, if the port is known that should be added.</span></span> <span data-ttu-id="991f7-172">Például, ha az IIS-kiszolgálón van elérte-e a AppVM01 csak SQL Server, a Célporttartomány módosítani kell a "*" (minden) 1433 (az SQL-port), így a kisebb bejövő támadási felületét AppVM01 kell a webalkalmazás legalább egyszer utaló jeleket.</span><span class="sxs-lookup"><span data-stu-id="991f7-172">For example, if the IIS server is hitting only SQL Server on AppVM01, the Destination Port Range should be changed from “*” (Any) to 1433 (the SQL port) thus allowing a smaller inbound attack surface on AppVM01 should the web application ever be compromised.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
        -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
    ```

6. <span data-ttu-id="991f7-173">Ez a szabály az internetről érkező forgalom megtagadja a hálózaton lévő kiszolgálók számára.</span><span class="sxs-lookup"><span data-stu-id="991f7-173">This rule denies traffic from the internet to any servers on the network.</span></span> <span data-ttu-id="991f7-174">A 110-es és 120 prioritással szabályokat a hatás hoz csak a bejövő internet-kezelési forgalom engedélyezése a tűzfal és a kiszolgálókon és blokkok RDP-portok minden más.</span><span class="sxs-lookup"><span data-stu-id="991f7-174">With the rules at priority 110 and 120, the effect is to allow only inbound internet traffic to the firewall and RDP ports on servers and blocks everything else.</span></span> <span data-ttu-id="991f7-175">Ez a szabály szerepel egy "hibamentes" szabályt, amely minden váratlan forgalom blokkolása.</span><span class="sxs-lookup"><span data-stu-id="991f7-175">This rule is a "fail-safe" rule to block all unexpected flows.</span></span>
    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate the $VNetName VNet from the Internet" `
        -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *
    ```
7. <span data-ttu-id="991f7-176">A végső szabály pedig megtagadja forgalom a Frontend alhálózatból való a Backend alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="991f7-176">The final rule denies traffic from the Frontend subnet to the Backend subnet.</span></span> <span data-ttu-id="991f7-177">Mivel ez a szabály egy bejövő forgalomra vonatkozó csak szabály, akkor (érkező a Háttéralkalmazását az az előtérbeli) egy fordított forgalom nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="991f7-177">Since this rule is an Inbound only rule, reverse traffic is allowed (from the Backend to the Frontend).</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
        -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix `
        -DestinationPortRange '*' `
        -Protocol * 
    ```

## <a name="traffic-scenarios"></a><span data-ttu-id="991f7-178">Forgalom forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="991f7-178">Traffic scenarios</span></span>
#### <a name="allowed-internet-to-web-server"></a><span data-ttu-id="991f7-179">(*Engedélyezett*) webkiszolgálón Internet</span><span class="sxs-lookup"><span data-stu-id="991f7-179">(*Allowed*) Internet to web server</span></span>
1. <span data-ttu-id="991f7-180">Az internetes felhasználó egy HTTP-lap kér FrontEnd001.CloudApp.Net (Internet Facing Felhőszolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="991f7-180">An internet user requests an HTTP page from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="991f7-181">Cloud service fázisok forgalom nyitott végpontok felé IIS01 80-as porton keresztül (webkiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="991f7-181">Cloud service passes traffic through open endpoint on port 80 towards IIS01 (the web server)</span></span>
3. <span data-ttu-id="991f7-182">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="991f7-182">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="991f7-183">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="991f7-183">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="991f7-184">NSG szabály 2 (RDP) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="991f7-184">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="991f7-185">NSG 3. szabály (IIS01 interneten) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="991f7-185">NSG Rule 3 (Internet to IIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="991f7-186">Forgalom találatok száma a belső kiszolgáló IP-címét a webes IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="991f7-186">Traffic hits internal IP address of the web server IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="991f7-187">IIS01 figyeli a webes forgalom, ezt a kérelmet kap, és elindítja a kérés feldolgozása</span><span class="sxs-lookup"><span data-stu-id="991f7-187">IIS01 is listening for web traffic, receives this request and starts processing the request</span></span>
6. <span data-ttu-id="991f7-188">IIS01 az SQL Server a AppVM01 adatokat kéri</span><span class="sxs-lookup"><span data-stu-id="991f7-188">IIS01 asks the SQL Server on AppVM01 for information</span></span>
7. <span data-ttu-id="991f7-189">Nincsenek kimenő szabályok a Frontend alhálózathoz, mert a forgalom engedélyezve van-e</span><span class="sxs-lookup"><span data-stu-id="991f7-189">Since there are no outbound rules on Frontend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="991f7-190">A Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="991f7-190">The Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="991f7-191">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="991f7-191">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="991f7-192">NSG szabály 2 (RDP) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="991f7-192">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="991f7-193">NSG 3. szabály (Internet tűzfalhoz) nem teljesül, a közvetkező szabályának áthelyezése</span><span class="sxs-lookup"><span data-stu-id="991f7-193">NSG Rule 3 (Internet to Firewall) doesn’t apply, move to next rule</span></span>
   4. <span data-ttu-id="991f7-194">NSG 4. szabály (a AppVM01 IIS01) teljesül, a forgalom engedélyezve van, akkor állítsa le a szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="991f7-194">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
9. <span data-ttu-id="991f7-195">AppVM01 dokumentálásáért és az SQL-lekérdezést kap</span><span class="sxs-lookup"><span data-stu-id="991f7-195">AppVM01 receives the SQL Query and responds</span></span>
10. <span data-ttu-id="991f7-196">Mivel a Backend alhálózathoz nem kimenő szabályok, engedélyezett-e a válasz</span><span class="sxs-lookup"><span data-stu-id="991f7-196">Since there are no outbound rules on the Backend subnet, the response is allowed</span></span>
11. <span data-ttu-id="991f7-197">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="991f7-197">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="991f7-198">Nincs érvényes bejövő szabály NSG a Frontend alhálózathoz, így nincs az NSG-szabályok vonatkoznak a Backend alhálózathoz-forgalom</span><span class="sxs-lookup"><span data-stu-id="991f7-198">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
    2. <span data-ttu-id="991f7-199">A alapértelmezett rendszerszintű szabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, a forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="991f7-199">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
12. <span data-ttu-id="991f7-200">Az IIS-kiszolgálót az SQL-válasz fogadása és a HTTP-válasz befejezése és a kérelmező küld</span><span class="sxs-lookup"><span data-stu-id="991f7-200">The IIS server receives the SQL response and completes the HTTP response and sends to the requestor</span></span>
13. <span data-ttu-id="991f7-201">Mivel nincsenek kimenő szabályok a válasz a Frontend alhálózathoz van engedélyezve, és az interneten a felhasználó kap a kért weblap.</span><span class="sxs-lookup"><span data-stu-id="991f7-201">Since there are no outbound rules on the Frontend subnet the response is allowed, and the internet User receives the web page requested.</span></span>

#### <a name="allowed-rdp-to-backend"></a><span data-ttu-id="991f7-202">(*Engedélyezett*) RDP háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="991f7-202">(*Allowed*) RDP to backend</span></span>
1. <span data-ttu-id="991f7-203">Server Admin interneten kérelmek AppVM01 BackEnd001.CloudApp.Net:xxxxx, ahol xxxxx az RDP számára (a hozzárendelt port található az Azure portálon vagy a PowerShell segítségével) AppVM01 véletlenszerűen hozzárendelt portszámot az RDP-munkamenetet</span><span class="sxs-lookup"><span data-stu-id="991f7-203">Server Admin on internet requests RDP session to AppVM01 on BackEnd001.CloudApp.Net:xxxxx where xxxxx is the randomly assigned port number for RDP to AppVM01 (the assigned port can be found on the Azure portal or via PowerShell)</span></span>
2. <span data-ttu-id="991f7-204">Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="991f7-204">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="991f7-205">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="991f7-205">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="991f7-206">NSG szabály 2 (RDP) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="991f7-206">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
3. <span data-ttu-id="991f7-207">A nem kimenő szabályokat alapértelmezett szabályokat alkalmazni, és a forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="991f7-207">With no outbound rules, default rules apply and return traffic is allowed</span></span>
4. <span data-ttu-id="991f7-208">RDP-munkamenetbe engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="991f7-208">RDP session is enabled</span></span>
5. <span data-ttu-id="991f7-209">A felhasználónevet és jelszót kér AppVM01</span><span class="sxs-lookup"><span data-stu-id="991f7-209">AppVM01 prompts for the user name and password</span></span>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a><span data-ttu-id="991f7-210">(*Engedélyezett*) a DNS-kiszolgáló Web server a DNS szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="991f7-210">(*Allowed*) Web server DNS look-up on DNS server</span></span>
1. <span data-ttu-id="991f7-211">Webalkalmazás-kiszolgálón, IIS01, egy adatcsatorna www.data.gov: igényeinek, de a címek feloldására igényeinek.</span><span class="sxs-lookup"><span data-stu-id="991f7-211">Web Server, IIS01, needs a data feed at www.data.gov, but needs to resolve the address.</span></span>
2. <span data-ttu-id="991f7-212">A hálózati konfigurációt a VNet listák DNS01 (a háttér alhálózaton 10.0.2.4) elsődleges DNS-kiszolgálóként, IIS01 küld a DNS-kérelem DNS01</span><span class="sxs-lookup"><span data-stu-id="991f7-212">The network configuration for the VNet lists DNS01 (10.0.2.4 on the Backend subnet) as the primary DNS server, IIS01 sends the DNS request to DNS01</span></span>
3. <span data-ttu-id="991f7-213">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="991f7-213">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="991f7-214">Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="991f7-214">Backend subnet begins inbound rule processing:</span></span>
   * <span data-ttu-id="991f7-215">NSG szabály 1 (DNS) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="991f7-215">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="991f7-216">DNS-kiszolgáló a kérelmet kap.</span><span class="sxs-lookup"><span data-stu-id="991f7-216">DNS server receives the request</span></span>
6. <span data-ttu-id="991f7-217">DNS-kiszolgáló nem rendelkezik a gyorsítótárazott címmel és egy legfelső szintű DNS-kiszolgáló kéri az interneten</span><span class="sxs-lookup"><span data-stu-id="991f7-217">DNS server doesn’t have the address cached and asks a root DNS server on the internet</span></span>
7. <span data-ttu-id="991f7-218">Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="991f7-218">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="991f7-219">Internetes DNS-kiszolgáló válaszol, mivel ehhez a munkamenethez belső kezdeményezett, a válasz engedélyezett</span><span class="sxs-lookup"><span data-stu-id="991f7-219">Internet DNS server responds, since this session was initiated internally, the response is allowed</span></span>
9. <span data-ttu-id="991f7-220">DNS-kiszolgáló gyorsítótárazza a választ, és a kezdeti kérés vissza IIS01 válaszol</span><span class="sxs-lookup"><span data-stu-id="991f7-220">DNS server caches the response, and responds to the initial request back to IIS01</span></span>
10. <span data-ttu-id="991f7-221">Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="991f7-221">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="991f7-222">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="991f7-222">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="991f7-223">Nincs érvényes bejövő szabály NSG a Frontend alhálózathoz, így nincs az NSG-szabályok vonatkoznak a Backend alhálózathoz-forgalom</span><span class="sxs-lookup"><span data-stu-id="991f7-223">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
    2. <span data-ttu-id="991f7-224">A alapértelmezett rendszerszintű szabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így a forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="991f7-224">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed</span></span>
12. <span data-ttu-id="991f7-225">IIS01 megkapja válaszát DNS01</span><span class="sxs-lookup"><span data-stu-id="991f7-225">IIS01 receives the response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="991f7-226">(*Engedélyezett*) server-hozzáférés fájl AppVM01</span><span class="sxs-lookup"><span data-stu-id="991f7-226">(*Allowed*) Web server access file on AppVM01</span></span>
1. <span data-ttu-id="991f7-227">IIS01 kér AppVM01 fájlba</span><span class="sxs-lookup"><span data-stu-id="991f7-227">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="991f7-228">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="991f7-228">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="991f7-229">A Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="991f7-229">The Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="991f7-230">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="991f7-230">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="991f7-231">NSG szabály 2 (RDP) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="991f7-231">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="991f7-232">NSG 3. szabály (IIS01 interneten) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="991f7-232">NSG Rule 3 (Internet to IIS01) doesn’t apply, move to next rule</span></span>
   4. <span data-ttu-id="991f7-233">NSG 4. szabály (a AppVM01 IIS01) teljesül, a forgalom engedélyezve van, akkor állítsa le a szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="991f7-233">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="991f7-234">AppVM01 a kérelmet kap, és válaszol, a fájl (feltéve, hogy van engedélyezve)</span><span class="sxs-lookup"><span data-stu-id="991f7-234">AppVM01 receives the request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="991f7-235">Mivel a Backend alhálózathoz nem kimenő szabályok, engedélyezett-e a válasz</span><span class="sxs-lookup"><span data-stu-id="991f7-235">Since there are no outbound rules on the Backend subnet, the response is allowed</span></span>
6. <span data-ttu-id="991f7-236">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="991f7-236">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="991f7-237">Nincs érvényes bejövő szabály NSG a Frontend alhálózathoz, így nincs az NSG-szabályok vonatkoznak a Backend alhálózathoz-forgalom</span><span class="sxs-lookup"><span data-stu-id="991f7-237">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
   2. <span data-ttu-id="991f7-238">A alapértelmezett rendszerszintű szabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, a forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="991f7-238">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
7. <span data-ttu-id="991f7-239">Az IIS-kiszolgálót kap a fájl</span><span class="sxs-lookup"><span data-stu-id="991f7-239">The IIS server receives the file</span></span>

#### <a name="denied-web-to-backend-server"></a><span data-ttu-id="991f7-240">(*Megtagadva*) háttérkiszolgálóra webes</span><span class="sxs-lookup"><span data-stu-id="991f7-240">(*Denied*) Web to backend server</span></span>
1. <span data-ttu-id="991f7-241">Az internetes felhasználó megpróbál hozzáférni az AppVM01 fájlba a BackEnd001.CloudApp.Net szolgáltatáson keresztül</span><span class="sxs-lookup"><span data-stu-id="991f7-241">An internet user tries to access a file on AppVM01 through the BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="991f7-242">Mivel nincsenek nyissa meg a fájlmegosztás nincsenek végpontok, a forgalom nem lenne továbbítja a felhőalapú szolgáltatás, és így elérni a kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="991f7-242">Since there are no endpoints open for file share, this traffic would not pass the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="991f7-243">Ha valamilyen okból a végpontok nyitott, NSG szabály 5 (Internet virtuális hálózatba) blokkolná-e a forgalom</span><span class="sxs-lookup"><span data-stu-id="991f7-243">If the endpoints were open for some reason, NSG rule 5 (Internet to VNet) would block this traffic</span></span>

#### <a name="denied-web-dns-look-up-on-dns-server"></a><span data-ttu-id="991f7-244">(*Megtagadva*) DNS-kiszolgálón a webes DNS szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="991f7-244">(*Denied*) Web DNS look-up on DNS server</span></span>
1. <span data-ttu-id="991f7-245">Az internetes felhasználó megpróbálja kereshet meg egy belső DNS-rekordot a DNS01 a BackEnd001.CloudApp.Net szolgáltatáson keresztül</span><span class="sxs-lookup"><span data-stu-id="991f7-245">An internet user tries to look up an internal DNS record on DNS01 through the BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="991f7-246">Mivel nincsenek végpontok nyissa meg a DNS, a forgalom nem lenne továbbítja a felhőalapú szolgáltatás, és így elérni a kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="991f7-246">Since there are no endpoints open for DNS, this traffic would not pass the Cloud Service and wouldn’t reach the server</span></span>
3. <span data-ttu-id="991f7-247">Ha valamilyen okból a végpontok nyitott, NSG szabály 5 (Internet virtuális hálózatba) blokkolná-e a forgalom (Megjegyzés: csak akkor nem vonatkozik két okból szabály 1 (DNS), először a forrás címe az interneten, ez a szabály csak a helyi virtuális hálózat, mint a forrás vonatkozik Ez a szabály a rendszer egy olyan engedélyezési szabály, akkor soha nem letiltsa a forgalom)</span><span class="sxs-lookup"><span data-stu-id="991f7-247">If the endpoints were open for some reason, NSG rule 5 (Internet to VNet) would block this traffic (Note: that Rule 1 (DNS) would not apply for two reasons, first the source address is the internet, this rule only applies to the local VNet as the source, also this rule is an Allow rule, so it would never deny traffic)</span></span>

#### <a name="denied-web-to-sql-access-through-firewall"></a><span data-ttu-id="991f7-248">(*Megtagadva*) webes SQL-hozzáférés tűzfalon keresztül</span><span class="sxs-lookup"><span data-stu-id="991f7-248">(*Denied*) Web to SQL access through firewall</span></span>
1. <span data-ttu-id="991f7-249">Az internetes felhasználó SQL adatokat kér FrontEnd001.CloudApp.Net (Internet Facing Felhőszolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="991f7-249">An internet user requests SQL data from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="991f7-250">Mivel nincsenek végpontok nyissa meg az SQL, a forgalom nem lenne továbbítja a felhőalapú szolgáltatás, és a tűzfal nem tudnák elérni</span><span class="sxs-lookup"><span data-stu-id="991f7-250">Since there are no endpoints open for SQL, this traffic would not pass the Cloud Service and wouldn’t reach the firewall</span></span>
3. <span data-ttu-id="991f7-251">Ha valamilyen okból végpontok voltak nyitva, a Frontend alhálózathoz megkezdi a bejövő szabály feldolgozása:</span><span class="sxs-lookup"><span data-stu-id="991f7-251">If endpoints were open for some reason, the Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="991f7-252">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="991f7-252">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
   2. <span data-ttu-id="991f7-253">NSG szabály 2 (RDP) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="991f7-253">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
   3. <span data-ttu-id="991f7-254">NSG 3. szabály (IIS01 interneten) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="991f7-254">NSG Rule 3 (Internet to IIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="991f7-255">Forgalom találatok a IIS01 belső IP-címét (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="991f7-255">Traffic hits internal IP address of the IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="991f7-256">IIS01 1433-as portot, ezért nem kérelemre adott válasz nem figyel.</span><span class="sxs-lookup"><span data-stu-id="991f7-256">IIS01 isn't listening on port 1433, so no response to the request</span></span>

## <a name="conclusion"></a><span data-ttu-id="991f7-257">Összegzés</span><span class="sxs-lookup"><span data-stu-id="991f7-257">Conclusion</span></span>
<span data-ttu-id="991f7-258">Ez a példa egy viszonylag egyszerű és egyszerű mód a a háttér-alhálózathoz, a bejövő forgalom elkülönítésére.</span><span class="sxs-lookup"><span data-stu-id="991f7-258">This example is a relatively simple and straight forward way of isolating the back-end subnet from inbound traffic.</span></span>

<span data-ttu-id="991f7-259">További példákat és a hálózati biztonsági határokat egy áttekintést talál [Itt][HOME].</span><span class="sxs-lookup"><span data-stu-id="991f7-259">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="991f7-260">Referencia</span><span class="sxs-lookup"><span data-stu-id="991f7-260">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="991f7-261">Fő parancsfájlt és a hálózati konfiguráció</span><span class="sxs-lookup"><span data-stu-id="991f7-261">Main script and network config</span></span>
<span data-ttu-id="991f7-262">A teljes parancsfájl menthető egy olyan PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="991f7-262">Save the Full Script in a PowerShell script file.</span></span> <span data-ttu-id="991f7-263">A hálózati konfiguráció mentése fájlba "NetworkConf1.xml."</span><span class="sxs-lookup"><span data-stu-id="991f7-263">Save the Network Config into a file named “NetworkConf1.xml.”</span></span>
<span data-ttu-id="991f7-264">A felhasználói változók szükség szerinti módosításához, és futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="991f7-264">Modify the user-defined variables as needed and run the script.</span></span>

#### <a name="full-script"></a><span data-ttu-id="991f7-265">Teljes körű parancsfájl</span><span class="sxs-lookup"><span data-stu-id="991f7-265">Full script</span></span>
<span data-ttu-id="991f7-266">Ez a parancsfájl lesz, a felhasználó által definiált változókat; alapján</span><span class="sxs-lookup"><span data-stu-id="991f7-266">This script will, based on the user-defined variables;</span></span>

1. <span data-ttu-id="991f7-267">Csatlakozás Azure-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="991f7-267">Connect to an Azure subscription</span></span>
2. <span data-ttu-id="991f7-268">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="991f7-268">Create a storage account</span></span>
3. <span data-ttu-id="991f7-269">Hozzon létre egy virtuális hálózat és a hálózati konfigurációs fájljában definiált két alhálózat</span><span class="sxs-lookup"><span data-stu-id="991f7-269">Create a VNet and two subnets as defined in the Network Config file</span></span>
4. <span data-ttu-id="991f7-270">Négy windows server virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="991f7-270">Build four windows server VMs</span></span>
5. <span data-ttu-id="991f7-271">Adja meg, beleértve az NSG:</span><span class="sxs-lookup"><span data-stu-id="991f7-271">Configure NSG including:</span></span>
   * <span data-ttu-id="991f7-272">Az NSG létrehozása</span><span class="sxs-lookup"><span data-stu-id="991f7-272">Creating an NSG</span></span>
   * <span data-ttu-id="991f7-273">Azt a szabályoknak feltöltése</span><span class="sxs-lookup"><span data-stu-id="991f7-273">Populating it with rules</span></span>
   * <span data-ttu-id="991f7-274">Az NSG kötése a megfelelő alhálózatokat</span><span class="sxs-lookup"><span data-stu-id="991f7-274">Binding the NSG to the appropriate subnets</span></span>

<span data-ttu-id="991f7-275">A PowerShell parancsfájl fusson helyben egy internethez csatlakoztatott PC vagy a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="991f7-275">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="991f7-276">Ezt a parancsfájlt, amikor előfordulhat figyelmeztetések vagy a PowerShellben pop más tájékoztató üzeneteit.</span><span class="sxs-lookup"><span data-stu-id="991f7-276">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="991f7-277">Piros csak hibaüzenetek aggodalomra.</span><span class="sxs-lookup"><span data-stu-id="991f7-277">Only error messages in red are cause for concern.</span></span>
> 
>

```PowerShell
<# 
 .SYNOPSIS
  Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)

 .DESCRIPTION
  This script will build out a sample DMZ setup containing:
   - A default storage account for VM disks
   - Two new cloud services
   - Two Subnets (FrontEnd and BackEnd subnets)
   - One server on the FrontEnd Subnet
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

# User-Defined Global Variables
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
    $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"

  # VM Base Disk Image Details
    $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

  # NSG Details
    $NSGName = "MyVNetSG"

# User-Defined VM Specific Configuration
    # Note: To ensure proper NSG Rule creation later in this script:
    #       - The Web Server must be VM 0
    #       - The AppVM1 Server must be VM 1
    #       - The DNS server must be VM 3
    #
    #       Otherwise the NSG rules in the last section of this
    #       script will need to be changed to match the modified
    #       VM array numbers ($i) so the NSG Rule IP addresses
    #       are aligned to the associated VM IP addresses.

    # VM 0 - The Web Server
      $VMName += "IIS01"
      $ServiceName += $FrontEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $FESubnet
      $VMIP += "10.0.1.5"

    # VM 1 - The First Application Server
      $VMName += "AppVM01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.5"

    # VM 2 - The Second Application Server
      $VMName += "AppVM02"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.6"

    # VM 3 - The DNS Server
      $VMName += "DNS01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.4"

# ----------------------------- #
# No User-Defined Variables or  #
# Configuration past this point #
# ----------------------------- #    

  # Get your Azure accounts
    Add-AzureAccount
    Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
    Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

  # Create Storage Account
    If (Test-AzureName -Storage -Name $StorageAccountName) { 
        Write-Host "Fatal Error: This storage account name is already in use, please pick a different name." -ForegroundColor Red
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
Else { Write-Host "The network configuration file was found" -ForegroundColor Green
        If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
            Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation variable is correct and the network config file matches.' -ForegroundColor Yellow
            $FatalError = $true}
        Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

If ($FatalError) {
    Write-Host "A fatal error has occurred, please see the above messages for more information." -ForegroundColor Red
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
        New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
            Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
            Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
            Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
            Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
            Remove-AzureEndpoint -Name "PowerShell" | `
            New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
            # Note: A Remote Desktop endpoint is automatically created when each VM is created.
        $i++
    }
    # Add HTTP Endpoint for IIS01
    Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM

# Configure NSG
    Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

  # Build the NSG
    Write-Host "Building the NSG" -ForegroundColor Cyan
    New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

  # Add NSG Rules
    Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
        -SourceAddressPrefix Internet -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
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
  # Install Test Web App (Run Post-Build Script on the IIS Server)
  # Install Backend resource (Run Post-Build Script on the AppVM01)
  Write-Host
  Write-Host "Build Complete!" -ForegroundColor Green
  Write-Host
  Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
  Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
  Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
  Write-Host
```

#### <a name="network-config-file"></a><span data-ttu-id="991f7-278">Hálózati konfigurációs fájl</span><span class="sxs-lookup"><span data-stu-id="991f7-278">Network config file</span></span>
<span data-ttu-id="991f7-279">Az XML-fájl mentése frissített hellyel rendelkező, és ehhez a fájlhoz, a fenti parancsfájlban a $NetworkConfigFile változóhoz a hivatkozás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="991f7-279">Save this xml file with updated location and add the link to this file to the $NetworkConfigFile variable in the preceding script.</span></span>

```XML
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
```

#### <a name="sample-application-scripts"></a><span data-ttu-id="991f7-280">Alkalmazás mintaparancsfájlok</span><span class="sxs-lookup"><span data-stu-id="991f7-280">Sample application scripts</span></span>
<span data-ttu-id="991f7-281">Szeretne telepíteni egy mintaalkalmazás ezzel és más DMZ példák, ha egy adtak meg, a következő hivatkozásra: [minta alkalmazás-parancsfájl][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="991f7-281">If you wish to install a sample application for this, and other DMZ Examples, one has been provided at the following link: [Sample Application Script][SampleApp]</span></span>

## <a name="next-steps"></a><span data-ttu-id="991f7-282">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="991f7-282">Next steps</span></span>
* <span data-ttu-id="991f7-283">Frissítse és mentse az XML-fájl</span><span class="sxs-lookup"><span data-stu-id="991f7-283">Update and save XML file</span></span>
* <span data-ttu-id="991f7-284">A környezet létrehozása a PowerShell-parancsprogram futtatása</span><span class="sxs-lookup"><span data-stu-id="991f7-284">Run the PowerShell script to build the environment</span></span>
* <span data-ttu-id="991f7-285">A mintaalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="991f7-285">Install the sample application</span></span>
* <span data-ttu-id="991f7-286">A DMZ keresztül különböző forgalom tesztelése</span><span class="sxs-lookup"><span data-stu-id="991f7-286">Test different traffic flows through this DMZ</span></span>

<!--Image References-->
<span data-ttu-id="991f7-287">[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Az NSG bejövő DMZ"</span><span class="sxs-lookup"><span data-stu-id="991f7-287">[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Inbound DMZ with NSG"</span></span>

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

