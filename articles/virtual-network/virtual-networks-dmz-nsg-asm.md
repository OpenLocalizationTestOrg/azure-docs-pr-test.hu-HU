---
title: "aaaAzure DMZ példa – Build az NSG-ket egy egyszerű DMZ |} Microsoft Docs"
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
ms.openlocfilehash: 32a40a8dc7539c4c7293988e6c36e5e32ef11045
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-classic-powershell"></a><span data-ttu-id="89013-103">1 – például egy egyszerű DMZ NSG-k használata a klasszikus PowerShell összeállítása</span><span class="sxs-lookup"><span data-stu-id="89013-103">Example 1 – Build a simple DMZ using NSGs with classic PowerShell</span></span>
<span data-ttu-id="89013-104">[Visszatérési toohello biztonsági határ bevált gyakorlatok lap][HOME]</span><span class="sxs-lookup"><span data-stu-id="89013-104">[Return toohello Security Boundary Best Practices Page][HOME]</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="89013-105">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="89013-105">Resource Manager Template</span></span>](virtual-networks-dmz-nsg.md)
> * [<span data-ttu-id="89013-106">Klasszikus - PowerShell</span><span class="sxs-lookup"><span data-stu-id="89013-106">Classic - PowerShell</span></span>](virtual-networks-dmz-nsg-asm.md)
> 
>

<span data-ttu-id="89013-107">Ebben a példában egy egyszerű DMZ négy Windows-kiszolgálók és hálózati biztonsági csoportokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="89013-107">This example creates a primitive DMZ with four Windows servers and Network Security Groups.</span></span> <span data-ttu-id="89013-108">Ez a példa bemutatja az egyes hello vonatkozó PowerShell parancsok tooprovide bemutatják az egyes lépések.</span><span class="sxs-lookup"><span data-stu-id="89013-108">This example describes each of hello relevant PowerShell commands tooprovide a deeper understanding of each step.</span></span> <span data-ttu-id="89013-109">Szerepel továbbá egy forgalom forgatókönyv szakasz tooprovide egy részletesebb lépésenkénti hogyan forgalom halad keresztül hello szolgálhat a hello DMZ.</span><span class="sxs-lookup"><span data-stu-id="89013-109">There is also a Traffic Scenario section tooprovide an in-depth step-by-step how traffic proceeds through hello layers of defense in hello DMZ.</span></span> <span data-ttu-id="89013-110">Végezetül hello references szakaszában van hello teljes kód látható és utasítás toobuild a környezet tootest és a különböző forgatókönyvekben a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="89013-110">Finally, in hello references section is hello complete code and instruction toobuild this environment tootest and experiment with various scenarios.</span></span> 

<span data-ttu-id="89013-111">![Az NSG bejövő DMZ][1]</span><span class="sxs-lookup"><span data-stu-id="89013-111">![Inbound DMZ with NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="89013-112">Környezet leírása</span><span class="sxs-lookup"><span data-stu-id="89013-112">Environment description</span></span>
<span data-ttu-id="89013-113">Ebben a példában egy előfizetés tartalmazza a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="89013-113">In this example a subscription contains hello following resources:</span></span>

* <span data-ttu-id="89013-114">A felhőalapú szolgáltatások két: "FrontEnd001" és "BackEnd001"</span><span class="sxs-lookup"><span data-stu-id="89013-114">Two cloud services: “FrontEnd001” and “BackEnd001”</span></span>
* <span data-ttu-id="89013-115">A virtuális hálózat, a "CorpNetwork", a két alhálózat; "Előtér" és "Háttér"</span><span class="sxs-lookup"><span data-stu-id="89013-115">A Virtual Network, “CorpNetwork”, with two subnets; “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="89013-116">A hálózati biztonsági csoport, amely alkalmazott tooboth alhálózatok</span><span class="sxs-lookup"><span data-stu-id="89013-116">A Network Security Group that is applied tooboth subnets</span></span>
* <span data-ttu-id="89013-117">A Windows Server egy webalkalmazás-kiszolgáló ("IIS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="89013-117">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="89013-118">Két windows Server kiszolgálókon, amelyek megfelelnek az alkalmazás háttérkiszolgálók ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="89013-118">Two windows servers that represent application back-end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="89013-119">A Windows server DNS-kiszolgáló ("DNS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="89013-119">A Windows server that represents a DNS server (“DNS01”)</span></span>

<span data-ttu-id="89013-120">Hello references szakaszában egy PowerShell-parancsfájlt, amely ebben a példában leírt hello környezetben a legtöbb buildek van.</span><span class="sxs-lookup"><span data-stu-id="89013-120">In hello references section, there is a PowerShell script that builds most of hello environment described in this example.</span></span> <span data-ttu-id="89013-121">Épület hello virtuális gépek és virtuális hálózatok, bár hello a példaként megadott parancsfájlt, által végzett dokumentum nem ismerteti a jelen dokumentum részletesen.</span><span class="sxs-lookup"><span data-stu-id="89013-121">Building hello VMs and Virtual Networks, although are done by hello example script, are not described in detail in this document.</span></span> 

<span data-ttu-id="89013-122">toobuild hello környezet;</span><span class="sxs-lookup"><span data-stu-id="89013-122">toobuild hello environment;</span></span>

1. <span data-ttu-id="89013-123">Hello hálózati konfigurációs XML-fájl (nevét, helyét és IP címeket toomatch megadott hello forgatókönyv frissíti) hello references szakaszában szereplő mentése</span><span class="sxs-lookup"><span data-stu-id="89013-123">Save hello network config xml file included in hello references section (updated with names, location, and IP addresses toomatch hello given scenario)</span></span>
2. <span data-ttu-id="89013-124">Frissítés hello felhasználói változók hello parancsfájl toomatch hello környezet hello parancsfájlban toobe futtatni (előfizetések, service nevét stb.)</span><span class="sxs-lookup"><span data-stu-id="89013-124">Update hello user variables in hello script toomatch hello environment hello script is toobe run against (subscriptions, service names, etc.)</span></span>
3. <span data-ttu-id="89013-125">PowerShell hello parancsfájlok végrehajtását</span><span class="sxs-lookup"><span data-stu-id="89013-125">Execute hello script in PowerShell</span></span>

>[!Note]
><span data-ttu-id="89013-126">hello régió hello PowerShell-parancsfájl esetében meg kell egyeznie a hello hálózati konfigurációs XML-fájlban szereplő hello régióban.</span><span class="sxs-lookup"><span data-stu-id="89013-126">hello region signified in hello PowerShell script must match hello region signified in hello network configuration xml file.</span></span>
>
>

<span data-ttu-id="89013-127">Miután hello parancsfájl futtatása sikeresen további választható lépéseket lehet tenni, hello references szakaszában két parancsfájlok tooset hello webkiszolgáló és egy egyszerű webes alkalmazás tooallow e DMZ konfiguráció tesztelése az alkalmazások kiszolgálói.</span><span class="sxs-lookup"><span data-stu-id="89013-127">Once hello script runs successfully additional optional steps may be taken, in hello references section are two scripts tooset up hello web server and app server with a simple web application tooallow testing with this DMZ configuration.</span></span>

<span data-ttu-id="89013-128">hello következő szakaszokban részletes leírása a hálózati biztonsági csoportok és működésének ehhez a példához keresztül hello PowerShell parancsfájl sorok érdekében.</span><span class="sxs-lookup"><span data-stu-id="89013-128">hello following sections provide a detailed description of Network Security Groups and how they function for this example by walking through key lines of hello PowerShell script.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="89013-129">Hálózati biztonsági csoportok (NSG)</span><span class="sxs-lookup"><span data-stu-id="89013-129">Network Security Groups (NSG)</span></span>
<span data-ttu-id="89013-130">Az ebben a példában egy NSG-csoport összeállítása és hat szabályokkal majd betölteni.</span><span class="sxs-lookup"><span data-stu-id="89013-130">For this example, an NSG group is built and then loaded with six rules.</span></span> 

> [!TIP]
> <span data-ttu-id="89013-131">Általánosságban véve kell először hozza létre az adott "Engedélyezés" szabályokat, és majd hello utolsó általánosabbá "Deny" szabályokat.</span><span class="sxs-lookup"><span data-stu-id="89013-131">Generally speaking, you should create your specific “Allow” rules first and then hello more generic “Deny” rules last.</span></span> <span data-ttu-id="89013-132">hello prioritású határozzák meg, hogy mely szabályokat értékeli ki a rendszer először.</span><span class="sxs-lookup"><span data-stu-id="89013-132">hello assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="89013-133">Forgalom található tooapply tooa meghatározott szabály, ha nincsenek további szabályok kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="89013-133">Once traffic is found tooapply tooa specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="89013-134">NSG-szabályok alkalmazhat vagy hello bejövő vagy kimenő irányban (szempontjából hello hello alhálózati).</span><span class="sxs-lookup"><span data-stu-id="89013-134">NSG rules can apply in either in hello inbound or outbound direction (from hello perspective of hello subnet).</span></span>
> 
> 

<span data-ttu-id="89013-135">A következő szabályok hello deklaratív módon, a bejövő forgalom alatt beépített:</span><span class="sxs-lookup"><span data-stu-id="89013-135">Declaratively, hello following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="89013-136">Belső DNS-forgalom (53-as port) engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="89013-136">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="89013-137">RDP-forgalom (3389-es port) a hello Internet tooany VM engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="89013-137">RDP traffic (port 3389) from hello Internet tooany VM is allowed</span></span>
3. <span data-ttu-id="89013-138">HTTP-forgalom (80-as port) hello internetes tooweb kiszolgáló (IIS01) engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="89013-138">HTTP traffic (port 80) from hello Internet tooweb server (IIS01) is allowed</span></span>
4. <span data-ttu-id="89013-139">Engedélyezett IIS01 tooAppVM1 minden forgalmat (minden porthoz)</span><span class="sxs-lookup"><span data-stu-id="89013-139">Any traffic (all ports) from IIS01 tooAppVM1 is allowed</span></span>
5. <span data-ttu-id="89013-140">Teljes virtuális hálózatot (alhálózatok mindkét) megtagadva hello Internet toohello minden forgalmat (minden porthoz)</span><span class="sxs-lookup"><span data-stu-id="89013-140">Any traffic (all ports) from hello Internet toohello entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="89013-141">Minden forgalmat (minden porthoz) hello Frontend alhálózathoz toohello Backend alhálózathoz megtagadva</span><span class="sxs-lookup"><span data-stu-id="89013-141">Any traffic (all ports) from hello Frontend subnet toohello Backend subnet is Denied</span></span>

<span data-ttu-id="89013-142">E szabályok kötött tooeach alhálózattal, ha a HTTP-kérelem nem bejövő webkiszolgálóról hello Internet toohello, mindkét szabályok 3 (engedélyezése) és 5 (megtagadni) szeretné alkalmazni, de mivel a szabály 3 egy magasabb prioritással bír, csak azt csak akkor vonatkozik, és 5 szabály nem lesz play kerülhet.</span><span class="sxs-lookup"><span data-stu-id="89013-142">With these rules bound tooeach subnet, if an HTTP request was inbound from hello Internet toohello web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="89013-143">Így hello HTTP-kérelem volna engedélyezett toohello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="89013-143">Thus hello HTTP request would be allowed toohello web server.</span></span> <span data-ttu-id="89013-144">Ha ugyanaz a forgalom próbált tooreach hello DNS01 server, a szabály 5 (Megtagadás) lenne hello első tooapply és hello forgalom volna nem engedélyezett toopass toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="89013-144">If that same traffic was trying tooreach hello DNS01 server, rule 5 (Deny) would be hello first tooapply and hello traffic would not be allowed toopass toohello server.</span></span> <span data-ttu-id="89013-145">6 (Megtagadás) szabály blokkolja hello Frontend alhálózathoz a toohello Backend alhálózathoz (kivéve a engedélyezett forgalom szabályokban 1 és 4) van szó, a szabálykészlet hello háttér hálózathoz védi, abban az esetben, ha egy támadó biztonság sérüléseinek hello webalkalmazás hello előtér, hello támadó volna korlátozott hozzáférés toohello háttérbeli "védett" hálózati (csak hello AppVM01 kiszolgálón kitett tooresources).</span><span class="sxs-lookup"><span data-stu-id="89013-145">Rule 6 (Deny) blocks hello Frontend subnet from talking toohello Backend subnet (except for allowed traffic in rules 1 and 4), this rule-set protects hello Backend network in case an attacker compromises hello web application on hello Frontend, hello attacker would have limited access toohello Backend “protected” network (only tooresources exposed on hello AppVM01 server).</span></span>

<span data-ttu-id="89013-146">Nincs olyan alapértelmezett kimenő szabály, amely lehetővé teszi, hogy a toohello kimenő forgalom internet.</span><span class="sxs-lookup"><span data-stu-id="89013-146">There is a default outbound rule that allows traffic out toohello internet.</span></span> <span data-ttu-id="89013-147">Ebben a példában a Microsoft most átengedi a kimenő forgalmat, és nem módosítja az egyetlen kimenő szabályok.</span><span class="sxs-lookup"><span data-stu-id="89013-147">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="89013-148">toolock le mindkét irányú forgalmát, felhasználói definiált útválasztás szükség, és a"3" hello a felfedezte van [biztonsági határ bevált gyakorlatok lap][HOME].</span><span class="sxs-lookup"><span data-stu-id="89013-148">toolock down traffic in both directions, User Defined Routing is required and is explored in “Example 3” on hello [Security Boundary Best Practices Page][HOME].</span></span>

<span data-ttu-id="89013-149">Minden egyes szabály tárgyalt részletesebben az alábbiak szerint (**Megjegyzés**: valamely elemére a következő lista kezdődő dollárjelet hello (például: $NSGName) egy felhasználó által definiált változó hello parancsfájlból hello hivatkozás szakaszban a jelen dokumentum):</span><span class="sxs-lookup"><span data-stu-id="89013-149">Each rule is discussed in more detail as follows (**Note**: any item in hello following list beginning with a dollar sign (for example: $NSGName) is a user-defined variable from hello script in hello reference section of this document):</span></span>

1. <span data-ttu-id="89013-150">Először egy hálózati biztonsági csoportot kell kialakítani, toohold hello szabályok:</span><span class="sxs-lookup"><span data-stu-id="89013-150">First a Network Security Group must be built toohold hello rules:</span></span>

    ```PowerShell
    New-AzureNetworkSecurityGroup -Name $NSGName `
        -Location $DeploymentLocation `
        -Label "Security group for $VNetName subnets in $DeploymentLocation"
    ```

2. <span data-ttu-id="89013-151">Ebben a példában a hello első szabály lehetővé teszi, hogy a DNS-forgalom hello backend alhálózathoz összes belső hálózatok toohello DNS-kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="89013-151">hello first rule in this example allows DNS traffic between all internal networks toohello DNS server on hello backend subnet.</span></span> <span data-ttu-id="89013-152">hello szabály néhány fontos paraméterekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="89013-152">hello rule has some important parameters:</span></span>
   
   * <span data-ttu-id="89013-153">"Típus" azt jelzi, hogy a forgalom iránya a szabály lép érvénybe.</span><span class="sxs-lookup"><span data-stu-id="89013-153">“Type” signifies in which direction of traffic flow this rule takes effect.</span></span> <span data-ttu-id="89013-154">hello használata hello szempontjából hello alhálózati vagy a virtuális gép (attól függően, ahol az NSG kötve van).</span><span class="sxs-lookup"><span data-stu-id="89013-154">hello direction is from hello perspective of hello subnet or Virtual Machine (depending on where this NSG is bound).</span></span> <span data-ttu-id="89013-155">Így ha típus: "Bejövő" és a forgalom hello alhálózati írja be, hello szabályt alkalmazni és hello alhálózatot elhagyó forgalomra ne befolyásolja az e szabály által.</span><span class="sxs-lookup"><span data-stu-id="89013-155">Thus if Type is “Inbound” and traffic is entering hello subnet, hello rule would apply and traffic leaving hello subnet would not be affected by this rule.</span></span>
   * <span data-ttu-id="89013-156">"Prioritás", amelyben a forgalom áramlását ki lesz értékelve hello sorrend beállítása.</span><span class="sxs-lookup"><span data-stu-id="89013-156">“Priority” sets hello order in which a traffic flow is evaluated.</span></span> <span data-ttu-id="89013-157">hello alacsonyabb hello számú hello hello elsőbbséget.</span><span class="sxs-lookup"><span data-stu-id="89013-157">hello lower hello number hello higher hello priority.</span></span> <span data-ttu-id="89013-158">Ha egy szabály érvényes tooa adott adatforgalmat, nincsenek további szabályok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="89013-158">When a rule applies tooa specific traffic flow, no further rules are processed.</span></span> <span data-ttu-id="89013-159">Így ha 1 prioritású szabály lehetővé teszi, hogy a forgalmat, és 2 prioritású szabály megtagadja a forgalmat, és mindkét szabályokat alkalmazni tootraffic akkor hello forgalom lesz engedélyezett tooflow (mivel szabály 1 kellett magasabb prioritású hatás tartott, és nincsenek további szabályok alkalmazása megtörtént).</span><span class="sxs-lookup"><span data-stu-id="89013-159">Thus if a rule with priority 1 allows traffic, and a rule with priority 2 denies traffic, and both rules apply tootraffic then hello traffic would be allowed tooflow (since rule 1 had a higher priority it took effect and no further rules were applied).</span></span>
   * <span data-ttu-id="89013-160">"Action" azt jelzi, hogy ha a szabály által érintett forgalom vagy tiltott.</span><span class="sxs-lookup"><span data-stu-id="89013-160">“Action” signifies if traffic affected by this rule is blocked or allowed.</span></span>

    ```PowerShell    
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
        -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[4] `
        -DestinationPortRange '53' `
        -Protocol *
    ```

3. <span data-ttu-id="89013-161">Ez a szabály lehetővé teszi, hogy RDP forgalom tooflow hello internet toohello RDP-portjára hello bármely kiszolgálón az alhálózati kötött.</span><span class="sxs-lookup"><span data-stu-id="89013-161">This rule allows RDP traffic tooflow from hello internet toohello RDP port on any server on hello bound subnet.</span></span> <span data-ttu-id="89013-162">Ez a szabály címelőtagokat; különleges kétféle használ. "VIRTUAL_NETWORK" és "INTERNET".</span><span class="sxs-lookup"><span data-stu-id="89013-162">This rule uses two special types of address prefixes; “VIRTUAL_NETWORK” and “INTERNET.”</span></span> <span data-ttu-id="89013-163">Ezek a címkék egy egyszerűen tooaddress címelőtagokat nagyobb kategóriáját.</span><span class="sxs-lookup"><span data-stu-id="89013-163">These tags are an easy way tooaddress a larger category of address prefixes.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" `
         -Type Inbound -Priority 110 -Action Allow `
         -SourceAddressPrefix INTERNET -SourcePortRange '*' `
         -DestinationAddressPrefix VIRTUAL_NETWORK `
         -DestinationPortRange '3389' `
         -Protocol *
    ```

4. <span data-ttu-id="89013-164">Ez a szabály lehetővé teszi, hogy a bejövő internet forgalom toohit hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="89013-164">This rule allows inbound internet traffic toohit hello web server.</span></span> <span data-ttu-id="89013-165">Ez a szabály nem változtatja meg a hello útválasztási viselkedés.</span><span class="sxs-lookup"><span data-stu-id="89013-165">This rule does not change hello routing behavior.</span></span> <span data-ttu-id="89013-166">hello szabály csak a IIS01 toopass szánt-forgalmát engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="89013-166">hello rule only allows traffic destined for IIS01 toopass.</span></span> <span data-ttu-id="89013-167">Így ha hello internetes forgalom volt-e a célként, ez a szabály akkor engedélyezi-e, és további szabályok feldolgozásának leállítása hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="89013-167">Thus if traffic from hello Internet had hello web server as its destination this rule would allow it and stop processing further rules.</span></span> <span data-ttu-id="89013-168">(Hello szabályban prioritással 140 összes egyéb bejövő forgalmat le van tiltva).</span><span class="sxs-lookup"><span data-stu-id="89013-168">(In hello rule at priority 140 all other inbound internet traffic is blocked).</span></span> <span data-ttu-id="89013-169">Ha csak a HTTP-forgalom éppen feldolgozás, ez a szabály lehet további korlátozott tooonly engedélyezi a 80-as Port cél.</span><span class="sxs-lookup"><span data-stu-id="89013-169">If you're only processing HTTP traffic, this rule could be further restricted tooonly allow Destination Port 80.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable Internet too$VMName[0]" `
         -Type Inbound -Priority 120 -Action Allow `
         -SourceAddressPrefix Internet -SourcePortRange '*' `
         -DestinationAddressPrefix $VMIP[0] `
         -DestinationPortRange '*' `
         -Protocol *
    ```

5. <span data-ttu-id="89013-170">Ez a szabály lehetővé teszi, hogy forgalom toopass hello IIS01 kiszolgálóról toohello AppVM01 kiszolgáló, egy újabb szabály blokkolja a más előtér tooBackend forgalmat.</span><span class="sxs-lookup"><span data-stu-id="89013-170">This rule allows traffic toopass from hello IIS01 server toohello AppVM01 server, a later rule blocks all other Frontend tooBackend traffic.</span></span> <span data-ttu-id="89013-171">Ez a szabály, ha hello port ismert, hogy hozzá kell adni tooimprove.</span><span class="sxs-lookup"><span data-stu-id="89013-171">tooimprove this rule, if hello port is known that should be added.</span></span> <span data-ttu-id="89013-172">Például ha hello IIS-kiszolgálón van elérte-e a AppVM01 csak SQL Server, hello Célporttartomány módosítani kell a "*" (minden) too1433 (hello SQL-port) így egy kisebb bejövő támadási felületét AppVM01 kell hello webalkalmazás legalább egyszer utaló jeleket.</span><span class="sxs-lookup"><span data-stu-id="89013-172">For example, if hello IIS server is hitting only SQL Server on AppVM01, hello Destination Port Range should be changed from “*” (Any) too1433 (hello SQL port) thus allowing a smaller inbound attack surface on AppVM01 should hello web application ever be compromised.</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] too$VMName[2]" `
        -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
    ```

6. <span data-ttu-id="89013-173">Ez a szabály megtagadja a kiszolgálókról hello internet tooany hello hálózati forgalom.</span><span class="sxs-lookup"><span data-stu-id="89013-173">This rule denies traffic from hello internet tooany servers on hello network.</span></span> <span data-ttu-id="89013-174">A 110-es és 120 prioritással hello szabályokat hello hatás tooallow csak bejövő internetes forgalom toohello tűzfal és a kiszolgálók RDP portok és blokkol minden más.</span><span class="sxs-lookup"><span data-stu-id="89013-174">With hello rules at priority 110 and 120, hello effect is tooallow only inbound internet traffic toohello firewall and RDP ports on servers and blocks everything else.</span></span> <span data-ttu-id="89013-175">Ez a szabály szerepel a "biztonságos" szabály tooblock összes váratlan adatfolyamok.</span><span class="sxs-lookup"><span data-stu-id="89013-175">This rule is a "fail-safe" rule tooblock all unexpected flows.</span></span>
    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate hello $VNetName VNet from hello Internet" `
        -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *
    ```
7. <span data-ttu-id="89013-176">hello végső szabály megtagadja a forgalmat a hello Frontend alhálózathoz toohello Backend alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="89013-176">hello final rule denies traffic from hello Frontend subnet toohello Backend subnet.</span></span> <span data-ttu-id="89013-177">Mivel ez a szabály egy bejövő forgalomra vonatkozó csak szabály, akkor (érkező hello háttér toohello előtér) egy fordított forgalom nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="89013-177">Since this rule is an Inbound only rule, reverse traffic is allowed (from hello Backend toohello Frontend).</span></span>

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" `
        -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix `
        -DestinationPortRange '*' `
        -Protocol * 
    ```

## <a name="traffic-scenarios"></a><span data-ttu-id="89013-178">Forgalom forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="89013-178">Traffic scenarios</span></span>
#### <a name="allowed-internet-tooweb-server"></a><span data-ttu-id="89013-179">(*Engedélyezett*) tooweb internetkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="89013-179">(*Allowed*) Internet tooweb server</span></span>
1. <span data-ttu-id="89013-180">Az internetes felhasználó egy HTTP-lap kér FrontEnd001.CloudApp.Net (Internet Facing Felhőszolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="89013-180">An internet user requests an HTTP page from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="89013-181">Cloud service fázisok forgalom nyitott végpontok felé IIS01 80-as porton keresztül (hello webkiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="89013-181">Cloud service passes traffic through open endpoint on port 80 towards IIS01 (hello web server)</span></span>
3. <span data-ttu-id="89013-182">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="89013-182">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="89013-183">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="89013-183">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="89013-184">NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="89013-184">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="89013-185">NSG 3. szabály (Internet tooIIS01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="89013-185">NSG Rule 3 (Internet tooIIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="89013-186">Forgalom találatok száma a belső kiszolgáló IP-címének hello webes IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="89013-186">Traffic hits internal IP address of hello web server IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="89013-187">IIS01 a webes forgalom figyeli, ezt a kérelmet kap, és elindítja a hello kérés feldolgozása</span><span class="sxs-lookup"><span data-stu-id="89013-187">IIS01 is listening for web traffic, receives this request and starts processing hello request</span></span>
6. <span data-ttu-id="89013-188">IIS01 hello SQL Server a AppVM01 adatokat kéri</span><span class="sxs-lookup"><span data-stu-id="89013-188">IIS01 asks hello SQL Server on AppVM01 for information</span></span>
7. <span data-ttu-id="89013-189">Nincsenek kimenő szabályok a Frontend alhálózathoz, mert a forgalom engedélyezve van-e</span><span class="sxs-lookup"><span data-stu-id="89013-189">Since there are no outbound rules on Frontend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="89013-190">hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="89013-190">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="89013-191">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="89013-191">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="89013-192">NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="89013-192">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="89013-193">NSG 3. szabály (Internet tooFirewall) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="89013-193">NSG Rule 3 (Internet tooFirewall) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="89013-194">NSG 4. szabály (IIS01 tooAppVM01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="89013-194">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
9. <span data-ttu-id="89013-195">AppVM01 hello SQL-lekérdezést kap, és válaszolhat</span><span class="sxs-lookup"><span data-stu-id="89013-195">AppVM01 receives hello SQL Query and responds</span></span>
10. <span data-ttu-id="89013-196">Nincsenek kimenő szabályok a hello Backend alhálózathoz, mert engedélyezve van-e a hello válasz</span><span class="sxs-lookup"><span data-stu-id="89013-196">Since there are no outbound rules on hello Backend subnet, hello response is allowed</span></span>
11. <span data-ttu-id="89013-197">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="89013-197">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="89013-198">Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály</span><span class="sxs-lookup"><span data-stu-id="89013-198">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="89013-199">hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="89013-199">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
12. <span data-ttu-id="89013-200">hello IIS server hello SQL-válasz fogadása és hello HTTP-válasz befejeződik, és elküldi a toohello kérelmező</span><span class="sxs-lookup"><span data-stu-id="89013-200">hello IIS server receives hello SQL response and completes hello HTTP response and sends toohello requestor</span></span>
13. <span data-ttu-id="89013-201">Mivel nincsenek kimenő szabályok hello Frontend alhálózaton hello választ kap, és hello internet felhasználó kap a kért weblap hello.</span><span class="sxs-lookup"><span data-stu-id="89013-201">Since there are no outbound rules on hello Frontend subnet hello response is allowed, and hello internet User receives hello web page requested.</span></span>

#### <a name="allowed-rdp-toobackend"></a><span data-ttu-id="89013-202">(*Engedélyezett*) RDP toobackend</span><span class="sxs-lookup"><span data-stu-id="89013-202">(*Allowed*) RDP toobackend</span></span>
1. <span data-ttu-id="89013-203">Server Admin interneten kérelmek RDP munkamenet tooAppVM01 BackEnd001.CloudApp.Net:xxxxx, ahol xxxxx az RDP tooAppVM01 véletlenszerűen hello portszámát a (hozzárendelt hello port található hello Azure-portálon vagy a PowerShell segítségével)</span><span class="sxs-lookup"><span data-stu-id="89013-203">Server Admin on internet requests RDP session tooAppVM01 on BackEnd001.CloudApp.Net:xxxxx where xxxxx is hello randomly assigned port number for RDP tooAppVM01 (hello assigned port can be found on hello Azure portal or via PowerShell)</span></span>
2. <span data-ttu-id="89013-204">Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="89013-204">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="89013-205">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="89013-205">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="89013-206">NSG szabály 2 (RDP) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="89013-206">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
3. <span data-ttu-id="89013-207">A nem kimenő szabályokat alapértelmezett szabályokat alkalmazni, és a forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="89013-207">With no outbound rules, default rules apply and return traffic is allowed</span></span>
4. <span data-ttu-id="89013-208">RDP-munkamenetbe engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="89013-208">RDP session is enabled</span></span>
5. <span data-ttu-id="89013-209">Hello felhasználónevet és jelszót kér AppVM01</span><span class="sxs-lookup"><span data-stu-id="89013-209">AppVM01 prompts for hello user name and password</span></span>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a><span data-ttu-id="89013-210">(*Engedélyezett*) a DNS-kiszolgáló Web server a DNS szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="89013-210">(*Allowed*) Web server DNS look-up on DNS server</span></span>
1. <span data-ttu-id="89013-211">Webalkalmazás-kiszolgáló, IIS01, kell egy adatcsatorna www.data.gov, azonban kell tooresolve hello cím.</span><span class="sxs-lookup"><span data-stu-id="89013-211">Web Server, IIS01, needs a data feed at www.data.gov, but needs tooresolve hello address.</span></span>
2. <span data-ttu-id="89013-212">hello tartozó fürthálózat-konfiguráció hello hálózatok listája DNS01 (a hello Backend alhálózathoz 10.0.2.4) hello elsődleges DNS-kiszolgálóként, IIS01 küldi hello DNS kérelem tooDNS01</span><span class="sxs-lookup"><span data-stu-id="89013-212">hello network configuration for hello VNet lists DNS01 (10.0.2.4 on hello Backend subnet) as hello primary DNS server, IIS01 sends hello DNS request tooDNS01</span></span>
3. <span data-ttu-id="89013-213">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="89013-213">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="89013-214">Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="89013-214">Backend subnet begins inbound rule processing:</span></span>
   * <span data-ttu-id="89013-215">NSG szabály 1 (DNS) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="89013-215">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="89013-216">DNS-kiszolgáló hello kérést kap.</span><span class="sxs-lookup"><span data-stu-id="89013-216">DNS server receives hello request</span></span>
6. <span data-ttu-id="89013-217">DNS-kiszolgáló nem rendelkezik gyorsítótárazott hello címmel és egy legfelső szintű DNS-kiszolgáló kéri a hello internet</span><span class="sxs-lookup"><span data-stu-id="89013-217">DNS server doesn’t have hello address cached and asks a root DNS server on hello internet</span></span>
7. <span data-ttu-id="89013-218">Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="89013-218">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="89013-219">Internetes DNS-kiszolgáló válaszol, mivel ehhez a munkamenethez belső kezdeményezték, hello válasz engedélyezett</span><span class="sxs-lookup"><span data-stu-id="89013-219">Internet DNS server responds, since this session was initiated internally, hello response is allowed</span></span>
9. <span data-ttu-id="89013-220">DNS-kiszolgáló gyorsítótárazza hello választ, és toohello kezdeti kérés hátsó tooIIS01 válaszol</span><span class="sxs-lookup"><span data-stu-id="89013-220">DNS server caches hello response, and responds toohello initial request back tooIIS01</span></span>
10. <span data-ttu-id="89013-221">Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="89013-221">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="89013-222">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="89013-222">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="89013-223">Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály</span><span class="sxs-lookup"><span data-stu-id="89013-223">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="89013-224">hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="89013-224">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed</span></span>
12. <span data-ttu-id="89013-225">IIS01 hello választ kap DNS01</span><span class="sxs-lookup"><span data-stu-id="89013-225">IIS01 receives hello response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="89013-226">(*Engedélyezett*) server-hozzáférés fájl AppVM01</span><span class="sxs-lookup"><span data-stu-id="89013-226">(*Allowed*) Web server access file on AppVM01</span></span>
1. <span data-ttu-id="89013-227">IIS01 kér AppVM01 fájlba</span><span class="sxs-lookup"><span data-stu-id="89013-227">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="89013-228">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="89013-228">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="89013-229">hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="89013-229">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="89013-230">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="89013-230">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="89013-231">NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="89013-231">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="89013-232">NSG 3. szabály (Internet tooIIS01) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="89013-232">NSG Rule 3 (Internet tooIIS01) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="89013-233">NSG 4. szabály (IIS01 tooAppVM01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="89013-233">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="89013-234">AppVM01 hello kérelmet kap, és válaszol, a fájl (feltéve, hogy van engedélyezve)</span><span class="sxs-lookup"><span data-stu-id="89013-234">AppVM01 receives hello request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="89013-235">Nincsenek kimenő szabályok a hello Backend alhálózathoz, mert engedélyezve van-e a hello válasz</span><span class="sxs-lookup"><span data-stu-id="89013-235">Since there are no outbound rules on hello Backend subnet, hello response is allowed</span></span>
6. <span data-ttu-id="89013-236">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="89013-236">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="89013-237">Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály</span><span class="sxs-lookup"><span data-stu-id="89013-237">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
   2. <span data-ttu-id="89013-238">hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="89013-238">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
7. <span data-ttu-id="89013-239">hello IIS kiszolgáló kap hello fájl</span><span class="sxs-lookup"><span data-stu-id="89013-239">hello IIS server receives hello file</span></span>

#### <a name="denied-web-toobackend-server"></a><span data-ttu-id="89013-240">(*Megtagadva*) toobackend webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="89013-240">(*Denied*) Web toobackend server</span></span>
1. <span data-ttu-id="89013-241">Az internetes felhasználó megpróbál tooaccess keresztül hello BackEnd001.CloudApp.Net szolgáltatás AppVM01 fájlba</span><span class="sxs-lookup"><span data-stu-id="89013-241">An internet user tries tooaccess a file on AppVM01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="89013-242">Mivel nincsenek nyissa meg a fájlmegosztás nincsenek végpontok, a forgalom hello felhőalapú szolgáltatás nem lesz továbbítja, és így elérni hello kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="89013-242">Since there are no endpoints open for file share, this traffic would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="89013-243">Ha valamilyen okból hello végpontok nyitott, NSG szabály 5 (Internet tooVNet) blokkolná-e a forgalom</span><span class="sxs-lookup"><span data-stu-id="89013-243">If hello endpoints were open for some reason, NSG rule 5 (Internet tooVNet) would block this traffic</span></span>

#### <a name="denied-web-dns-look-up-on-dns-server"></a><span data-ttu-id="89013-244">(*Megtagadva*) DNS-kiszolgálón a webes DNS szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="89013-244">(*Denied*) Web DNS look-up on DNS server</span></span>
1. <span data-ttu-id="89013-245">Az internetes felhasználó megpróbál toolook fel egy belső DNS-rekordot a DNS01 keresztül hello BackEnd001.CloudApp.Net szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="89013-245">An internet user tries toolook up an internal DNS record on DNS01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="89013-246">Mivel nincsenek végpontok nyissa meg a DNS, a forgalom hello felhőalapú szolgáltatás nem lesz továbbítja, és így elérni hello kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="89013-246">Since there are no endpoints open for DNS, this traffic would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="89013-247">Ha valamilyen okból hello végpontok nyitott, NSG szabály 5 (Internet tooVNet) blokkolná-e a forgalom (Megjegyzés: két okból nem kell alkalmazni, hogy a szabály 1 (DNS), első hello forráscím van hello internet, ez a szabály csak érvényes toohello hello mint virtuális helyi hálózat is forrás Ez a szabály szerepel egy olyan engedélyezési szabály, akkor soha nem letiltsa a forgalom)</span><span class="sxs-lookup"><span data-stu-id="89013-247">If hello endpoints were open for some reason, NSG rule 5 (Internet tooVNet) would block this traffic (Note: that Rule 1 (DNS) would not apply for two reasons, first hello source address is hello internet, this rule only applies toohello local VNet as hello source, also this rule is an Allow rule, so it would never deny traffic)</span></span>

#### <a name="denied-web-toosql-access-through-firewall"></a><span data-ttu-id="89013-248">(*Megtagadva*) webalkalmazás-tooSQL hozzáférés tűzfalon keresztül</span><span class="sxs-lookup"><span data-stu-id="89013-248">(*Denied*) Web tooSQL access through firewall</span></span>
1. <span data-ttu-id="89013-249">Az internetes felhasználó SQL adatokat kér FrontEnd001.CloudApp.Net (Internet Facing Felhőszolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="89013-249">An internet user requests SQL data from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="89013-250">Mivel nincsenek végpontok nyissa meg az SQL, a forgalom hello felhőalapú szolgáltatás nem lesz továbbítja, és így elérni hello tűzfal</span><span class="sxs-lookup"><span data-stu-id="89013-250">Since there are no endpoints open for SQL, this traffic would not pass hello Cloud Service and wouldn’t reach hello firewall</span></span>
3. <span data-ttu-id="89013-251">Ha valamilyen okból végpontok voltak nyitva, hello Frontend alhálózathoz megkezdi a bejövő szabály feldolgozása:</span><span class="sxs-lookup"><span data-stu-id="89013-251">If endpoints were open for some reason, hello Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="89013-252">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="89013-252">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="89013-253">NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="89013-253">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="89013-254">NSG 3. szabály (Internet tooIIS01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="89013-254">NSG Rule 3 (Internet tooIIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="89013-255">Forgalom találatok belső IP-címe hello IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="89013-255">Traffic hits internal IP address of hello IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="89013-256">IIS01 1433-as portot, ezért nincs válasz toohello kérelmet nem figyeli.</span><span class="sxs-lookup"><span data-stu-id="89013-256">IIS01 isn't listening on port 1433, so no response toohello request</span></span>

## <a name="conclusion"></a><span data-ttu-id="89013-257">Összegzés</span><span class="sxs-lookup"><span data-stu-id="89013-257">Conclusion</span></span>
<span data-ttu-id="89013-258">Ez a példa egy viszonylag egyszerű és egyszerű mód a hello háttér-alhálózathoz, a bejövő forgalom elkülönítésére.</span><span class="sxs-lookup"><span data-stu-id="89013-258">This example is a relatively simple and straight forward way of isolating hello back-end subnet from inbound traffic.</span></span>

<span data-ttu-id="89013-259">További példákat és a hálózati biztonsági határokat egy áttekintést talál [Itt][HOME].</span><span class="sxs-lookup"><span data-stu-id="89013-259">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="89013-260">Referencia</span><span class="sxs-lookup"><span data-stu-id="89013-260">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="89013-261">Fő parancsfájlt és a hálózati konfiguráció</span><span class="sxs-lookup"><span data-stu-id="89013-261">Main script and network config</span></span>
<span data-ttu-id="89013-262">Hello teljes parancsfájl menthető egy olyan PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="89013-262">Save hello Full Script in a PowerShell script file.</span></span> <span data-ttu-id="89013-263">Hálózati konfiguráció hello "NetworkConf1.xml." fájlba mentése</span><span class="sxs-lookup"><span data-stu-id="89013-263">Save hello Network Config into a file named “NetworkConf1.xml.”</span></span>
<span data-ttu-id="89013-264">Hello felhasználói változók módosításához szükséges és futtatási hello parancsfájlként.</span><span class="sxs-lookup"><span data-stu-id="89013-264">Modify hello user-defined variables as needed and run hello script.</span></span>

#### <a name="full-script"></a><span data-ttu-id="89013-265">Teljes szkript</span><span class="sxs-lookup"><span data-stu-id="89013-265">Full script</span></span>
<span data-ttu-id="89013-266">Ez a parancsfájl fog hello felhasználói változók; alapján</span><span class="sxs-lookup"><span data-stu-id="89013-266">This script will, based on hello user-defined variables;</span></span>

1. <span data-ttu-id="89013-267">Csatlakozás Azure-előfizetés tooan</span><span class="sxs-lookup"><span data-stu-id="89013-267">Connect tooan Azure subscription</span></span>
2. <span data-ttu-id="89013-268">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="89013-268">Create a storage account</span></span>
3. <span data-ttu-id="89013-269">Hozzon létre egy virtuális hálózat és két alhálózat hello hálózati konfigurációs fájlban meghatározottak szerint</span><span class="sxs-lookup"><span data-stu-id="89013-269">Create a VNet and two subnets as defined in hello Network Config file</span></span>
4. <span data-ttu-id="89013-270">Négy windows server virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="89013-270">Build four windows server VMs</span></span>
5. <span data-ttu-id="89013-271">Adja meg, beleértve az NSG:</span><span class="sxs-lookup"><span data-stu-id="89013-271">Configure NSG including:</span></span>
   * <span data-ttu-id="89013-272">Az NSG létrehozása</span><span class="sxs-lookup"><span data-stu-id="89013-272">Creating an NSG</span></span>
   * <span data-ttu-id="89013-273">Azt a szabályoknak feltöltése</span><span class="sxs-lookup"><span data-stu-id="89013-273">Populating it with rules</span></span>
   * <span data-ttu-id="89013-274">Kötési hello NSG toohello megfelelő alhálózatokat</span><span class="sxs-lookup"><span data-stu-id="89013-274">Binding hello NSG toohello appropriate subnets</span></span>

<span data-ttu-id="89013-275">A PowerShell parancsfájl fusson helyben egy internethez csatlakoztatott PC vagy a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="89013-275">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="89013-276">Ezt a parancsfájlt, amikor előfordulhat figyelmeztetések vagy a PowerShellben pop más tájékoztató üzeneteit.</span><span class="sxs-lookup"><span data-stu-id="89013-276">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="89013-277">Piros csak hibaüzenetek aggodalomra.</span><span class="sxs-lookup"><span data-stu-id="89013-277">Only error messages in red are cause for concern.</span></span>
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
   - One server on hello FrontEnd Subnet
   - Three Servers on hello BackEnd Subnet
   - Network Security Groups tooallow/deny traffic patterns as declared

  Before running script, ensure hello network configuration file is created in
  hello directory referenced by $NetworkConfigFile variable (or update the
  variable tooreflect hello path and file name of hello config file being used).

 .Notes
  Security requirements are different for each use case and can be addressed in a
  myriad of ways. Please be sure that any sensitive data or applications are behind
  hello appropriate layer(s) of protection. This script serves as an example of some
  of hello techniques that can be used, but should not be used for all scenarios. You
  are responsible tooassess your security needs and hello appropriate protections
  needed, and then effectively implement those protections.

  FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
   IIS01      - 10.0.1.5

  BackEnd Service (BackEnd subnet 10.0.2.0/24)
   DNS01      - 10.0.2.4
   AppVM01    - 10.0.2.5
   AppVM02    - 10.0.2.6

#>

# Fixed Variables
    $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password toobe used for all VMs"
    $VMName = @()
    $ServiceName = @()
    $VMFamily = @()
    $img = @()
    $size = @()
    $SubnetName = @()
    $VMIP = @()

# User-Defined Global Variables
  # These should be changes tooreflect your subscription and services
  # Invalid options will fail in hello validation section

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
    # Note: tooensure proper NSG Rule creation later in this script:
    #       - hello Web Server must be VM 0
    #       - hello AppVM1 Server must be VM 1
    #       - hello DNS server must be VM 3
    #
    #       Otherwise hello NSG rules in hello last section of this
    #       script will need toobe changed toomatch hello modified
    #       VM array numbers ($i) so hello NSG Rule IP addresses
    #       are aligned toohello associated VM IP addresses.

    # VM 0 - hello Web Server
      $VMName += "IIS01"
      $ServiceName += $FrontEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $FESubnet
      $VMIP += "10.0.1.5"

    # VM 1 - hello First Application Server
      $VMName += "AppVM01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.5"

    # VM 2 - hello Second Application Server
      $VMName += "AppVM02"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.6"

    # VM 3 - hello DNS Server
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

  # Update Subscription Pointer tooNew Storage Account
    Write-Host "Updating Subscription Pointer tooNew Storage Account" -ForegroundColor Cyan 
    Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

# Validation
$FatalError = $false

If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
     Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
     $FatalError = $true}

If (Test-AzureName -Service -Name $FrontEndService) { 
    Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

If (Test-AzureName -Service -Name $BackEndService) { 
    Write-Host "hello BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "hello BackEndService service name is valid for use." -ForegroundColor Green}

If (-Not (Test-Path $NetworkConfigFile)) { 
    Write-Host 'hello network config file was not found, please update hello $NetworkConfigFile variable toopoint toohello network config xml file.' -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "hello network configuration file was found" -ForegroundColor Green
        If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
            Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation variable is correct and hello network config file matches.' -ForegroundColor Yellow
            $FatalError = $true}
        Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

If ($FatalError) {
    Write-Host "A fatal error has occurred, please see hello above messages for more information." -ForegroundColor Red
    Return}
Else { Write-Host "Validation passed, now building hello environment." -ForegroundColor Green}

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
    Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

  # Build hello NSG
    Write-Host "Building hello NSG" -ForegroundColor Cyan
    New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

  # Add NSG Rules
    Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet too$($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
        -SourceAddressPrefix Internet -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) too$($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
        -Protocol *

    # Assign hello NSG toohello Subnets
        Write-Host "Binding hello NSG tooboth subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

# Optional Post-script Manual Configuration
  # Install Test Web App (Run Post-Build Script on hello IIS Server)
  # Install Backend resource (Run Post-Build Script on hello AppVM01)
  Write-Host
  Write-Host "Build Complete!" -ForegroundColor Green
  Write-Host
  Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
  Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
  Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
  Write-Host
```

#### <a name="network-config-file"></a><span data-ttu-id="89013-278">Hálózati konfigurációs fájl</span><span class="sxs-lookup"><span data-stu-id="89013-278">Network config file</span></span>
<span data-ttu-id="89013-279">Az XML-fájl mentése frissített hellyel rendelkező, és hello hivatkozás toothis fájl toohello $NetworkConfigFile változó hozzáadása a hello parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="89013-279">Save this xml file with updated location and add hello link toothis file toohello $NetworkConfigFile variable in hello preceding script.</span></span>

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

#### <a name="sample-application-scripts"></a><span data-ttu-id="89013-280">Alkalmazás mintaparancsfájlok</span><span class="sxs-lookup"><span data-stu-id="89013-280">Sample application scripts</span></span>
<span data-ttu-id="89013-281">Ha a tooinstall mintaalkalmazás ezzel és más DMZ példák, egy adtak meg, a következő hivatkozás hello: [minta alkalmazás-parancsfájl][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="89013-281">If you wish tooinstall a sample application for this, and other DMZ Examples, one has been provided at hello following link: [Sample Application Script][SampleApp]</span></span>

## <a name="next-steps"></a><span data-ttu-id="89013-282">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="89013-282">Next steps</span></span>
* <span data-ttu-id="89013-283">Frissítse és mentse az XML-fájl</span><span class="sxs-lookup"><span data-stu-id="89013-283">Update and save XML file</span></span>
* <span data-ttu-id="89013-284">Hello PowerShell parancsfájl toobuild hello környezetet</span><span class="sxs-lookup"><span data-stu-id="89013-284">Run hello PowerShell script toobuild hello environment</span></span>
* <span data-ttu-id="89013-285">Hello mintaalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="89013-285">Install hello sample application</span></span>
* <span data-ttu-id="89013-286">A DMZ keresztül különböző forgalom tesztelése</span><span class="sxs-lookup"><span data-stu-id="89013-286">Test different traffic flows through this DMZ</span></span>

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Az NSG bejövő DMZ"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

