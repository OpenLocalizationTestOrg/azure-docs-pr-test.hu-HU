---
title: "aaaAzure DMZ példa – Build az NSG-ket egy egyszerű DMZ |} Microsoft Docs"
description: "A hálózati biztonsági csoportokkal (NSG) DMZ összeállítása"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 11c5c6026da30fbc9c5e585f5c16e2d411d6fd80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a><span data-ttu-id="47886-103">1 – például egy egyszerű DMZ NSG-k használata az Azure Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="47886-103">Example 1 – Build a simple DMZ using NSGs with an Azure Resource Manager template</span></span>
<span data-ttu-id="47886-104">[Visszatérési toohello biztonsági határ bevált gyakorlatok lap][HOME]</span><span class="sxs-lookup"><span data-stu-id="47886-104">[Return toohello Security Boundary Best Practices Page][HOME]</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="47886-105">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="47886-105">Resource Manager Template</span></span>](virtual-networks-dmz-nsg.md)
> * [<span data-ttu-id="47886-106">Klasszikus - PowerShell</span><span class="sxs-lookup"><span data-stu-id="47886-106">Classic - PowerShell</span></span>](virtual-networks-dmz-nsg-asm.md)
> 
>

<span data-ttu-id="47886-107">Ebben a példában egy egyszerű DMZ négy Windows-kiszolgálók és hálózati biztonsági csoportokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="47886-107">This example creates a primitive DMZ with four Windows servers and Network Security Groups.</span></span> <span data-ttu-id="47886-108">Ez a példa bemutatja az egyes hello megfelelő sablont szakaszok tooprovide bemutatják az egyes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="47886-108">This example describes each of hello relevant template sections tooprovide a deeper understanding of each step.</span></span> <span data-ttu-id="47886-109">Nincs is a forgalom forgatókönyv szakasz tooprovide egy hogyan forgalom halad keresztül hello hello DMZ a védelmi részletesebb lépésenkénti tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="47886-109">There is also a Traffic Scenario section tooprovide an in-depth step-by-step look at how traffic proceeds through hello layers of defense in hello DMZ.</span></span> <span data-ttu-id="47886-110">Végezetül hello references szakaszában van hello teljes sablon kód és utasítások toobuild a környezet tootest és a különböző forgatókönyvekben a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="47886-110">Finally, in hello references section is hello complete template code and instructions toobuild this environment tootest and experiment with various scenarios.</span></span> 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

<span data-ttu-id="47886-111">![Az NSG bejövő DMZ][1]</span><span class="sxs-lookup"><span data-stu-id="47886-111">![Inbound DMZ with NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="47886-112">Környezet leírása</span><span class="sxs-lookup"><span data-stu-id="47886-112">Environment description</span></span>
<span data-ttu-id="47886-113">Ebben a példában egy előfizetés tartalmazza a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="47886-113">In this example a subscription contains hello following resources:</span></span>

* <span data-ttu-id="47886-114">Egyetlen erőforráscsoportként működnek</span><span class="sxs-lookup"><span data-stu-id="47886-114">A single resource group</span></span>
* <span data-ttu-id="47886-115">Egy virtuális hálózatot, két alhálózattal; "Előtér" és "Háttér"</span><span class="sxs-lookup"><span data-stu-id="47886-115">A Virtual Network with two subnets; “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="47886-116">A hálózati biztonsági csoport, amely alkalmazott tooboth alhálózatok</span><span class="sxs-lookup"><span data-stu-id="47886-116">A Network Security Group that is applied tooboth subnets</span></span>
* <span data-ttu-id="47886-117">A Windows Server egy webalkalmazás-kiszolgáló ("IIS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="47886-117">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="47886-118">Két windows Server kiszolgálókon, amelyek megfelelnek az alkalmazás háttérkiszolgálók ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="47886-118">Two windows servers that represent application back-end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="47886-119">A Windows server DNS-kiszolgáló ("DNS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="47886-119">A Windows server that represents a DNS server (“DNS01”)</span></span>
* <span data-ttu-id="47886-120">A nyilvános IP-cím hello alkalmazás webkiszolgáló társított</span><span class="sxs-lookup"><span data-stu-id="47886-120">A public IP address associated with hello application web server</span></span>

<span data-ttu-id="47886-121">Hello references szakaszában nincs hivatkozás tooan Azure Resource Manager sablon hello környezet ebben a példában leírt épülő.</span><span class="sxs-lookup"><span data-stu-id="47886-121">In hello references section, there is a link tooan Azure Resource Manager template that builds hello environment described in this example.</span></span> <span data-ttu-id="47886-122">Épület hello virtuális gépek és virtuális hálózatok, bár hello példa-sablon által nem ismerteti a jelen dokumentum részletesen.</span><span class="sxs-lookup"><span data-stu-id="47886-122">Building hello VMs and Virtual Networks, although done by hello example template, are not described in detail in this document.</span></span> 

<span data-ttu-id="47886-123">**toobuild ebben a környezetben** (részletes utasításokat amelyek hello references szakaszában, a jelen dokumentum);</span><span class="sxs-lookup"><span data-stu-id="47886-123">**toobuild this environment** (detailed instructions are in hello references section of this document);</span></span>

1. <span data-ttu-id="47886-124">Telepítése Azure Resource Manager sablon hello: [Azure gyors üzembe helyezés sablonok][Template]</span><span class="sxs-lookup"><span data-stu-id="47886-124">Deploy hello Azure Resource Manager Template at: [Azure Quickstart Templates][Template]</span></span>
2. <span data-ttu-id="47886-125">Hello minta alkalmazást telepíteni: [minta alkalmazás-parancsfájl][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="47886-125">Install hello sample application at: [Sample Application Script][SampleApp]</span></span>

>[!NOTE]
><span data-ttu-id="47886-126">tooRDP tooany háttérkiszolgálók ebben a példában hello IIS-kiszolgálón használt egy "ugrást mező."</span><span class="sxs-lookup"><span data-stu-id="47886-126">tooRDP tooany back-end servers in this instance, hello IIS server is used as a "jump box."</span></span> <span data-ttu-id="47886-127">Első RDP toohello IIS-kiszolgálót és majd hello IIS Server RDP toohello háttér-kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="47886-127">First RDP toohello IIS server and then from hello IIS Server RDP toohello back-end server.</span></span> <span data-ttu-id="47886-128">Másik lehetőség egy nyilvános IP-cím társítható minden kiszolgáló egyszerűbb az RDP a hálózati Adapternek.</span><span class="sxs-lookup"><span data-stu-id="47886-128">Alternately a Public IP can be associated with each server NIC for easier RDP.</span></span>
> 
>

<span data-ttu-id="47886-129">hello következő szakaszokban hello hálózati biztonsági csoport, és hogyan működik ez a példa az útmutató alapján kulcs sornyi hello Azure Resource Manager-sablon által részletes leírását.</span><span class="sxs-lookup"><span data-stu-id="47886-129">hello following sections provide a detailed description of hello Network Security Group and how it functions for this example by walking through key lines of hello Azure Resource Manager Template.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="47886-130">Hálózati biztonsági csoportok (NSG)</span><span class="sxs-lookup"><span data-stu-id="47886-130">Network Security Groups (NSG)</span></span>
<span data-ttu-id="47886-131">Az ebben a példában egy NSG-csoport összeállítása és hat szabályokkal majd betölteni.</span><span class="sxs-lookup"><span data-stu-id="47886-131">For this example, an NSG group is built and then loaded with six rules.</span></span> 

>[!TIP]
><span data-ttu-id="47886-132">Általánosságban véve kell először hozza létre az adott "Engedélyezés" szabályokat, és majd hello utolsó általánosabbá "Deny" szabályokat.</span><span class="sxs-lookup"><span data-stu-id="47886-132">Generally speaking, you should create your specific “Allow” rules first and then hello more generic “Deny” rules last.</span></span> <span data-ttu-id="47886-133">hello prioritású határozzák meg, hogy mely szabályokat értékeli ki a rendszer először.</span><span class="sxs-lookup"><span data-stu-id="47886-133">hello assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="47886-134">Forgalom található tooapply tooa meghatározott szabály, ha nincsenek további szabályok kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="47886-134">Once traffic is found tooapply tooa specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="47886-135">NSG-szabályok alkalmazhat vagy hello bejövő vagy kimenő irányban (szempontjából hello hello alhálózati).</span><span class="sxs-lookup"><span data-stu-id="47886-135">NSG rules can apply in either in hello inbound or outbound direction (from hello perspective of hello subnet).</span></span>
>
>

<span data-ttu-id="47886-136">A következő szabályok hello deklaratív módon, a bejövő forgalom alatt beépített:</span><span class="sxs-lookup"><span data-stu-id="47886-136">Declaratively, hello following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="47886-137">Belső DNS-forgalom (53-as port) engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="47886-137">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="47886-138">RDP-forgalom (3389-es port) a hello Internet tooany VM engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="47886-138">RDP traffic (port 3389) from hello Internet tooany VM is allowed</span></span>
3. <span data-ttu-id="47886-139">HTTP-forgalom (80-as port) hello internetes tooweb kiszolgáló (IIS01) engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="47886-139">HTTP traffic (port 80) from hello Internet tooweb server (IIS01) is allowed</span></span>
4. <span data-ttu-id="47886-140">Engedélyezett IIS01 tooAppVM1 minden forgalmat (minden porthoz)</span><span class="sxs-lookup"><span data-stu-id="47886-140">Any traffic (all ports) from IIS01 tooAppVM1 is allowed</span></span>
5. <span data-ttu-id="47886-141">Teljes virtuális hálózatot (alhálózatok mindkét) megtagadva hello Internet toohello minden forgalmat (minden porthoz)</span><span class="sxs-lookup"><span data-stu-id="47886-141">Any traffic (all ports) from hello Internet toohello entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="47886-142">Minden forgalmat (minden porthoz) hello Frontend alhálózathoz toohello Backend alhálózathoz megtagadva</span><span class="sxs-lookup"><span data-stu-id="47886-142">Any traffic (all ports) from hello Frontend subnet toohello Backend subnet is Denied</span></span>

<span data-ttu-id="47886-143">E szabályok kötött tooeach alhálózattal, ha a HTTP-kérelem nem bejövő webkiszolgálóról hello Internet toohello, mindkét szabályok 3 (engedélyezése) és 5 (megtagadni) szeretné alkalmazni, de mivel a szabály 3 egy magasabb prioritással bír, csak azt csak akkor vonatkozik, és 5 szabály nem lesz play kerülhet.</span><span class="sxs-lookup"><span data-stu-id="47886-143">With these rules bound tooeach subnet, if an HTTP request was inbound from hello Internet toohello web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="47886-144">Így hello HTTP-kérelem volna engedélyezett toohello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="47886-144">Thus hello HTTP request would be allowed toohello web server.</span></span> <span data-ttu-id="47886-145">Ha ugyanaz a forgalom próbált tooreach hello DNS01 server, a szabály 5 (Megtagadás) lenne hello első tooapply és hello forgalom volna nem engedélyezett toopass toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="47886-145">If that same traffic was trying tooreach hello DNS01 server, rule 5 (Deny) would be hello first tooapply and hello traffic would not be allowed toopass toohello server.</span></span> <span data-ttu-id="47886-146">6 (Megtagadás) szabály blokkolja hello Frontend alhálózathoz a toohello Backend alhálózathoz (kivéve a engedélyezett forgalom szabályokban 1 és 4) van szó, a szabálykészlet hello háttér hálózathoz védi, abban az esetben, ha egy támadó biztonság sérüléseinek hello webalkalmazás hello előtér, hello támadó volna korlátozott hozzáférés toohello háttérbeli "védett" hálózati (csak hello AppVM01 kiszolgálón kitett tooresources).</span><span class="sxs-lookup"><span data-stu-id="47886-146">Rule 6 (Deny) blocks hello Frontend subnet from talking toohello Backend subnet (except for allowed traffic in rules 1 and 4), this rule-set protects hello Backend network in case an attacker compromises hello web application on hello Frontend, hello attacker would have limited access toohello Backend “protected” network (only tooresources exposed on hello AppVM01 server).</span></span>

<span data-ttu-id="47886-147">Nincs olyan alapértelmezett kimenő szabály, amely lehetővé teszi, hogy a toohello kimenő forgalom internet.</span><span class="sxs-lookup"><span data-stu-id="47886-147">There is a default outbound rule that allows traffic out toohello internet.</span></span> <span data-ttu-id="47886-148">Ebben a példában a Microsoft most átengedi a kimenő forgalmat, és nem módosítja az egyetlen kimenő szabályok.</span><span class="sxs-lookup"><span data-stu-id="47886-148">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="47886-149">tooapply biztonsági házirend tootraffic mindkét irányban, felhasználói definiált útválasztás szükség, és a"3" hello a felfedezte van [biztonsági határ bevált gyakorlatok lap][HOME].</span><span class="sxs-lookup"><span data-stu-id="47886-149">tooapply security policy tootraffic in both directions, User Defined Routing is required and is explored in “Example 3” on hello [Security Boundary Best Practices Page][HOME].</span></span>

<span data-ttu-id="47886-150">Minden egyes szabály az alábbiak szerint tárgyalja részletesen:</span><span class="sxs-lookup"><span data-stu-id="47886-150">Each rule is discussed in more detail as follows:</span></span>

1. <span data-ttu-id="47886-151">A hálózati biztonsági csoport erőforrás példányként létrehozott toohold hello szabályokat kell lennie:</span><span class="sxs-lookup"><span data-stu-id="47886-151">A Network Security Group resource must be instantiated toohold hello rules:</span></span>

    ```JSON
    "resources": [
      {
        "apiVersion": "2015-05-01-preview",
        "type": "Microsoft.Network/networkSecurityGroups",
        "name": "[variables('NSGName')]",
        "location": "[resourceGroup().location]",
        "properties": { }
      }
    ]
    ``` 

2. <span data-ttu-id="47886-152">Ebben a példában a hello első szabály lehetővé teszi, hogy a DNS-forgalom hello backend alhálózathoz összes belső hálózatok toohello DNS-kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="47886-152">hello first rule in this example allows DNS traffic between all internal networks toohello DNS server on hello backend subnet.</span></span> <span data-ttu-id="47886-153">hello szabály néhány fontos paraméterekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="47886-153">hello rule has some important parameters:</span></span>
  * <span data-ttu-id="47886-154">"destinationAddressPrefix" - szabályok használhatja az "alapértelmezett címke" nevű címelőtag különleges típusú, és ezekkel a címkékkel rendszer által biztosított azonosítók, amelyek lehetővé teszik egy egyszerű módot tooaddress címelőtagokat nagyobb kategóriáját.</span><span class="sxs-lookup"><span data-stu-id="47886-154">"destinationAddressPrefix" - Rules can use a special type of address prefix called a "Default Tag", these tags are system-provided identifiers that allow an easy way tooaddress a larger category of address prefixes.</span></span> <span data-ttu-id="47886-155">Ez a szabály által használt alapértelmezett címke "Internet" hello toosignify bármely cím hello virtuális hálózaton kívül.</span><span class="sxs-lookup"><span data-stu-id="47886-155">This rule uses hello Default Tag “Internet” toosignify any address outside of hello VNet.</span></span> <span data-ttu-id="47886-156">Más előtag feliratai VirtualNetwork és AzureLoadBalancer.</span><span class="sxs-lookup"><span data-stu-id="47886-156">Other prefix labels are VirtualNetwork and AzureLoadBalancer.</span></span>
  * <span data-ttu-id="47886-157">"Irány" azt jelzi, hogy a forgalom iránya a szabály lép érvénybe.</span><span class="sxs-lookup"><span data-stu-id="47886-157">“Direction” signifies in which direction of traffic flow this rule takes effect.</span></span> <span data-ttu-id="47886-158">hello használata hello szempontjából hello alhálózati vagy a virtuális gép (attól függően, ahol az NSG kötve van).</span><span class="sxs-lookup"><span data-stu-id="47886-158">hello direction is from hello perspective of hello subnet or Virtual Machine (depending on where this NSG is bound).</span></span> <span data-ttu-id="47886-159">Így ha irány "Bejövő" és a forgalom hello alhálózati írja be, hello szabályt alkalmazni és hello alhálózatot elhagyó forgalomra ne befolyásolja az e szabály által.</span><span class="sxs-lookup"><span data-stu-id="47886-159">Thus if Direction is “Inbound” and traffic is entering hello subnet, hello rule would apply and traffic leaving hello subnet would not be affected by this rule.</span></span>
  * <span data-ttu-id="47886-160">"Prioritás", amelyben a forgalom áramlását ki lesz értékelve hello sorrend beállítása.</span><span class="sxs-lookup"><span data-stu-id="47886-160">“Priority” sets hello order in which a traffic flow is evaluated.</span></span> <span data-ttu-id="47886-161">hello alacsonyabb hello számú hello hello elsőbbséget.</span><span class="sxs-lookup"><span data-stu-id="47886-161">hello lower hello number hello higher hello priority.</span></span> <span data-ttu-id="47886-162">Ha egy szabály érvényes tooa adott adatforgalmat, nincsenek további szabályok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="47886-162">When a rule applies tooa specific traffic flow, no further rules are processed.</span></span> <span data-ttu-id="47886-163">Így ha 1 prioritású szabály lehetővé teszi, hogy a forgalmat, és 2 prioritású szabály megtagadja a forgalmat, és mindkét szabályokat alkalmazni tootraffic akkor hello forgalom lesz engedélyezett tooflow (mivel szabály 1 kellett magasabb prioritású hatás tartott, és nincsenek további szabályok alkalmazása megtörtént).</span><span class="sxs-lookup"><span data-stu-id="47886-163">Thus if a rule with priority 1 allows traffic, and a rule with priority 2 denies traffic, and both rules apply tootraffic then hello traffic would be allowed tooflow (since rule 1 had a higher priority it took effect and no further rules were applied).</span></span>
  * <span data-ttu-id="47886-164">"Access" jelzi, hogy a szabály által érintett forgalom-e letiltott ("Deny") vagy engedélyezett ("Engedélyezés").</span><span class="sxs-lookup"><span data-stu-id="47886-164">“Access” signifies if traffic affected by this rule is blocked ("Deny") or allowed ("Allow").</span></span>

    ```JSON
    "properties": {
    "securityRules": [
      {
        "name": "enable_dns_rule",
        "properties": {
          "description": "Enable Internal DNS",
          "protocol": "*",
          "sourcePortRange": "*",
          "destinationPortRange": "53",
          "sourceAddressPrefix": "VirtualNetwork",
          "destinationAddressPrefix": "10.0.2.4",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
    ```

3. <span data-ttu-id="47886-165">Ez a szabály lehetővé teszi, hogy RDP forgalom tooflow hello internet toohello RDP-portjára hello bármely kiszolgálón az alhálózati kötött.</span><span class="sxs-lookup"><span data-stu-id="47886-165">This rule allows RDP traffic tooflow from hello internet toohello RDP port on any server on hello bound subnet.</span></span> 

    ```JSON
    {
      "name": "enable_rdp_rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 110,
        "direction": "Inbound"
      }
    },
    ```

4. <span data-ttu-id="47886-166">Ez a szabály lehetővé teszi, hogy a bejövő internet forgalom toohit hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="47886-166">This rule allows inbound internet traffic toohit hello web server.</span></span> <span data-ttu-id="47886-167">Ez a szabály nem változtatja meg a hello útválasztási viselkedés.</span><span class="sxs-lookup"><span data-stu-id="47886-167">This rule does not change hello routing behavior.</span></span> <span data-ttu-id="47886-168">hello szabály csak a IIS01 toopass szánt-forgalmát engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="47886-168">hello rule only allows traffic destined for IIS01 toopass.</span></span> <span data-ttu-id="47886-169">Így ha hello internetes forgalom volt-e a célként, ez a szabály akkor engedélyezi-e, és további szabályok feldolgozásának leállítása hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="47886-169">Thus if traffic from hello Internet had hello web server as its destination this rule would allow it and stop processing further rules.</span></span> <span data-ttu-id="47886-170">(Hello szabályban prioritással 140 összes egyéb bejövő forgalmat le van tiltva).</span><span class="sxs-lookup"><span data-stu-id="47886-170">(In hello rule at priority 140 all other inbound internet traffic is blocked).</span></span> <span data-ttu-id="47886-171">Ha csak a HTTP-forgalom éppen feldolgozás, ez a szabály lehet további korlátozott tooonly engedélyezi a 80-as Port cél.</span><span class="sxs-lookup"><span data-stu-id="47886-171">If you're only processing HTTP traffic, this rule could be further restricted tooonly allow Destination Port 80.</span></span>

    ```JSON
    {
      "name": "enable_web_rule",
      "properties": {
        "description": "Enable Internet too[variables('VM01Name')]",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "10.0.1.5",
        "access": "Allow",
        "priority": 120,
        "direction": "Inbound"
        }
      },
    ```

5. <span data-ttu-id="47886-172">Ez a szabály lehetővé teszi, hogy forgalom toopass hello IIS01 kiszolgálóról toohello AppVM01 kiszolgáló, egy újabb szabály blokkolja a más előtér tooBackend forgalmat.</span><span class="sxs-lookup"><span data-stu-id="47886-172">This rule allows traffic toopass from hello IIS01 server toohello AppVM01 server, a later rule blocks all other Frontend tooBackend traffic.</span></span> <span data-ttu-id="47886-173">Ez a szabály, ha hello port ismert, hogy hozzá kell adni tooimprove.</span><span class="sxs-lookup"><span data-stu-id="47886-173">tooimprove this rule, if hello port is known that should be added.</span></span> <span data-ttu-id="47886-174">Például ha hello IIS-kiszolgálón van elérte-e a AppVM01 csak SQL Server, hello Célporttartomány módosítani kell a "*" (minden) too1433 (hello SQL-port) így egy kisebb bejövő támadási felületét AppVM01 kell hello webalkalmazás legalább egyszer utaló jeleket.</span><span class="sxs-lookup"><span data-stu-id="47886-174">For example, if hello IIS server is hitting only SQL Server on AppVM01, hello Destination Port Range should be changed from “*” (Any) too1433 (hello SQL port) thus allowing a smaller inbound attack surface on AppVM01 should hello web application ever be compromised.</span></span>

    ```JSON
    {
      "name": "enable_app_rule",
      "properties": {
        "description": "Enable [variables('VM01Name')] too[variables('VM02Name')]",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "10.0.1.5",
        "destinationAddressPrefix": "10.0.2.5",
        "access": "Allow",
        "priority": 130,
        "direction": "Inbound"
      }
    },
     ```

6. <span data-ttu-id="47886-175">Ez a szabály megtagadja a kiszolgálókról hello internet tooany hello hálózati forgalom.</span><span class="sxs-lookup"><span data-stu-id="47886-175">This rule denies traffic from hello internet tooany servers on hello network.</span></span> <span data-ttu-id="47886-176">A 110-es és 120 prioritással hello szabályokat hello hatás tooallow csak bejövő internetes forgalom toohello tűzfal és a kiszolgálók RDP portok és blokkol minden más.</span><span class="sxs-lookup"><span data-stu-id="47886-176">With hello rules at priority 110 and 120, hello effect is tooallow only inbound internet traffic toohello firewall and RDP ports on servers and blocks everything else.</span></span> <span data-ttu-id="47886-177">Ez a szabály szerepel a "biztonságos" szabály tooblock összes váratlan adatfolyamok.</span><span class="sxs-lookup"><span data-stu-id="47886-177">This rule is a "fail-safe" rule tooblock all unexpected flows.</span></span>

    ```JSON
    {
      "name": "deny_internet_rule",
      "properties": {
        "description": "Isolate hello [variables('VNetName')] VNet from hello Internet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Deny",
        "priority": 140,
        "direction": "Inbound"
      }
    },
     ```

7. <span data-ttu-id="47886-178">hello végső szabály megtagadja a forgalmat a hello Frontend alhálózathoz toohello Backend alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="47886-178">hello final rule denies traffic from hello Frontend subnet toohello Backend subnet.</span></span> <span data-ttu-id="47886-179">Mivel ez a szabály egy bejövő forgalomra vonatkozó csak szabály, akkor (érkező hello háttér toohello előtér) egy fordított forgalom nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="47886-179">Since this rule is an Inbound only rule, reverse traffic is allowed (from hello Backend toohello Frontend).</span></span>

    ```JSON
    {
      "name": "deny_frontend_rule",
      "properties": {
        "description": "Isolate hello [variables('Subnet1Name')] subnet from hello [variables('Subnet2Name')] subnet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "[variables('Subnet1Prefix')]",
        "destinationAddressPrefix": "[variables('Subnet2Prefix')]",
        "access": "Deny",
        "priority": 150,
        "direction": "Inbound"
      }
    }
    ```

## <a name="traffic-scenarios"></a><span data-ttu-id="47886-180">Forgalom forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="47886-180">Traffic scenarios</span></span>
#### <a name="allowed-internet-tooweb-server"></a><span data-ttu-id="47886-181">(*Engedélyezett*) tooweb internetkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="47886-181">(*Allowed*) Internet tooweb server</span></span>
1. <span data-ttu-id="47886-182">Az internetes felhasználó egy HTTP-lap kér hello IIS01 NIC társított hálózati adapter hello hello nyilvános IP-címe</span><span class="sxs-lookup"><span data-stu-id="47886-182">An internet user requests an HTTP page from hello public IP address of hello NIC associated with hello IIS01 NIC</span></span>
2. <span data-ttu-id="47886-183">Nyilvános IP-cím hello forgalom toohello VNet IIS01 felé továbbítja (hello webkiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="47886-183">hello Public IP address passes traffic toohello VNet towards IIS01 (hello web server)</span></span>
3. <span data-ttu-id="47886-184">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="47886-184">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="47886-185">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="47886-185">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="47886-186">NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="47886-186">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
  3. <span data-ttu-id="47886-187">NSG 3. szabály (Internet tooIIS01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="47886-187">NSG Rule 3 (Internet tooIIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="47886-188">Forgalom találatok száma a belső kiszolgáló IP-címének hello webes IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="47886-188">Traffic hits internal IP address of hello web server IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="47886-189">IIS01 a webes forgalom figyeli, ezt a kérelmet kap, és elindítja a hello kérés feldolgozása</span><span class="sxs-lookup"><span data-stu-id="47886-189">IIS01 is listening for web traffic, receives this request and starts processing hello request</span></span>
6. <span data-ttu-id="47886-190">IIS01 hello SQL Server a AppVM01 adatokat kéri</span><span class="sxs-lookup"><span data-stu-id="47886-190">IIS01 asks hello SQL Server on AppVM01 for information</span></span>
7. <span data-ttu-id="47886-191">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="47886-191">No outbound rules on Frontend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="47886-192">hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="47886-192">hello Backend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="47886-193">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="47886-193">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="47886-194">NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="47886-194">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
  3. <span data-ttu-id="47886-195">NSG 3. szabály (Internet tooFirewall) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="47886-195">NSG Rule 3 (Internet tooFirewall) doesn’t apply, move toonext rule</span></span>
  4. <span data-ttu-id="47886-196">NSG 4. szabály (IIS01 tooAppVM01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="47886-196">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
9. <span data-ttu-id="47886-197">AppVM01 hello SQL-lekérdezést kap, és válaszolhat</span><span class="sxs-lookup"><span data-stu-id="47886-197">AppVM01 receives hello SQL Query and responds</span></span>
10. <span data-ttu-id="47886-198">Nincsenek kimenő szabályok a hello Backend alhálózathoz, mert engedélyezve van-e a hello válasz</span><span class="sxs-lookup"><span data-stu-id="47886-198">Since there are no outbound rules on hello Backend subnet, hello response is allowed</span></span>
11. <span data-ttu-id="47886-199">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="47886-199">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="47886-200">Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály</span><span class="sxs-lookup"><span data-stu-id="47886-200">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
  2. <span data-ttu-id="47886-201">hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="47886-201">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
12. <span data-ttu-id="47886-202">hello IIS server hello SQL-válasz fogadása és hello HTTP-válasz befejeződik, és elküldi a toohello kérelmező</span><span class="sxs-lookup"><span data-stu-id="47886-202">hello IIS server receives hello SQL response and completes hello HTTP response and sends toohello requester</span></span>
13. <span data-ttu-id="47886-203">Mivel nincsenek kimenő szabályok hello Frontend alhálózaton, hello választ kap, és a hello internetes felhasználó megkapja a kért weblap hello.</span><span class="sxs-lookup"><span data-stu-id="47886-203">Since there are no outbound rules on hello Frontend subnet, hello response is allowed and hello Internet User receives hello web page requested.</span></span>

#### <a name="allowed-rdp-tooiis-server"></a><span data-ttu-id="47886-204">(*Engedélyezett*) RDP tooIIS kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="47886-204">(*Allowed*) RDP tooIIS server</span></span>
1. <span data-ttu-id="47886-205">Egy kiszolgálói rendszergazda interneten kér egy RDP-munkamenet tooIIS01 a nyilvános IP-címe hello hello hello IIS01 hálózati adapter (a nyilvános IP-cím található hello portál vagy a PowerShell használatával) társított hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="47886-205">A Server Admin on internet requests an RDP session tooIIS01 on hello public IP address of hello NIC associated with hello IIS01 NIC (this public IP address can be found via hello Portal or PowerShell)</span></span>
2. <span data-ttu-id="47886-206">Nyilvános IP-cím hello forgalom toohello VNet IIS01 felé továbbítja (hello webkiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="47886-206">hello Public IP address passes traffic toohello VNet towards IIS01 (hello web server)</span></span>
3. <span data-ttu-id="47886-207">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="47886-207">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="47886-208">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="47886-208">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="47886-209">NSG szabály 2 (RDP) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="47886-209">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="47886-210">A nem kimenő szabályokat alapértelmezett szabályokat alkalmazni, és a forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="47886-210">With no outbound rules, default rules apply and return traffic is allowed</span></span>
5. <span data-ttu-id="47886-211">RDP-munkamenetbe engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="47886-211">RDP session is enabled</span></span>
6. <span data-ttu-id="47886-212">Hello felhasználónevet és jelszót kér IIS01</span><span class="sxs-lookup"><span data-stu-id="47886-212">IIS01 prompts for hello user name and password</span></span>

>[!NOTE]
><span data-ttu-id="47886-213">tooRDP tooany háttérkiszolgálók ebben a példában hello IIS-kiszolgálón használt egy "ugrást mező."</span><span class="sxs-lookup"><span data-stu-id="47886-213">tooRDP tooany back-end servers in this instance, hello IIS server is used as a "jump box."</span></span> <span data-ttu-id="47886-214">Első RDP toohello IIS-kiszolgálót és majd hello IIS Server RDP toohello háttér-kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="47886-214">First RDP toohello IIS server and then from hello IIS Server RDP toohello back-end server.</span></span>
>
>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a><span data-ttu-id="47886-215">(*Engedélyezett*) a DNS-kiszolgáló Web server a DNS szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="47886-215">(*Allowed*) Web server DNS look-up on DNS server</span></span>
1. <span data-ttu-id="47886-216">Webalkalmazás-kiszolgáló, IIS01, kell egy adatcsatorna www.data.gov, azonban kell tooresolve hello cím.</span><span class="sxs-lookup"><span data-stu-id="47886-216">Web Server, IIS01, needs a data feed at www.data.gov, but needs tooresolve hello address.</span></span>
2. <span data-ttu-id="47886-217">hello tartozó fürthálózat-konfiguráció hello hálózatok listája DNS01 (a hello Backend alhálózathoz 10.0.2.4) hello elsődleges DNS-kiszolgálóként, IIS01 küldi hello DNS kérelem tooDNS01</span><span class="sxs-lookup"><span data-stu-id="47886-217">hello network configuration for hello VNet lists DNS01 (10.0.2.4 on hello Backend subnet) as hello primary DNS server, IIS01 sends hello DNS request tooDNS01</span></span>
3. <span data-ttu-id="47886-218">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="47886-218">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="47886-219">Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="47886-219">Backend subnet begins inbound rule processing:</span></span>
  * <span data-ttu-id="47886-220">NSG szabály 1 (DNS) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="47886-220">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="47886-221">DNS-kiszolgáló hello kérést kap.</span><span class="sxs-lookup"><span data-stu-id="47886-221">DNS server receives hello request</span></span>
6. <span data-ttu-id="47886-222">DNS-kiszolgáló nem rendelkezik gyorsítótárazott hello címmel és egy legfelső szintű DNS-kiszolgáló kéri a hello internet</span><span class="sxs-lookup"><span data-stu-id="47886-222">DNS server doesn’t have hello address cached and asks a root DNS server on hello internet</span></span>
7. <span data-ttu-id="47886-223">Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="47886-223">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="47886-224">Internetes DNS-kiszolgáló válaszol, mivel ehhez a munkamenethez belső kezdeményezték, hello válasz engedélyezett</span><span class="sxs-lookup"><span data-stu-id="47886-224">Internet DNS server responds, since this session was initiated internally, hello response is allowed</span></span>
9. <span data-ttu-id="47886-225">DNS-kiszolgáló gyorsítótárazza hello választ, és toohello kezdeti kérés hátsó tooIIS01 válaszol</span><span class="sxs-lookup"><span data-stu-id="47886-225">DNS server caches hello response, and responds toohello initial request back tooIIS01</span></span>
10. <span data-ttu-id="47886-226">Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="47886-226">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="47886-227">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="47886-227">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="47886-228">Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály</span><span class="sxs-lookup"><span data-stu-id="47886-228">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
  2. <span data-ttu-id="47886-229">hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="47886-229">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed</span></span>
12. <span data-ttu-id="47886-230">IIS01 hello választ kap DNS01</span><span class="sxs-lookup"><span data-stu-id="47886-230">IIS01 receives hello response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="47886-231">(*Engedélyezett*) server-hozzáférés fájl AppVM01</span><span class="sxs-lookup"><span data-stu-id="47886-231">(*Allowed*) Web server access file on AppVM01</span></span>
1. <span data-ttu-id="47886-232">IIS01 kér AppVM01 fájlba</span><span class="sxs-lookup"><span data-stu-id="47886-232">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="47886-233">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="47886-233">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="47886-234">hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="47886-234">hello Backend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="47886-235">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="47886-235">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="47886-236">NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="47886-236">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
  3. <span data-ttu-id="47886-237">NSG 3. szabály (Internet tooIIS01) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="47886-237">NSG Rule 3 (Internet tooIIS01) doesn’t apply, move toonext rule</span></span>
  4. <span data-ttu-id="47886-238">NSG 4. szabály (IIS01 tooAppVM01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="47886-238">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="47886-239">AppVM01 hello kérelmet kap, és válaszol, a fájl (feltéve, hogy van engedélyezve)</span><span class="sxs-lookup"><span data-stu-id="47886-239">AppVM01 receives hello request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="47886-240">Nincsenek kimenő szabályok a hello Backend alhálózathoz, mert engedélyezve van-e a hello válasz</span><span class="sxs-lookup"><span data-stu-id="47886-240">Since there are no outbound rules on hello Backend subnet, hello response is allowed</span></span>
6. <span data-ttu-id="47886-241">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="47886-241">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="47886-242">Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály</span><span class="sxs-lookup"><span data-stu-id="47886-242">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
  2. <span data-ttu-id="47886-243">hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="47886-243">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
7. <span data-ttu-id="47886-244">hello IIS kiszolgáló kap hello fájl</span><span class="sxs-lookup"><span data-stu-id="47886-244">hello IIS server receives hello file</span></span>

#### <a name="denied-rdp-toobackend"></a><span data-ttu-id="47886-245">(*Megtagadva*) RDP toobackend</span><span class="sxs-lookup"><span data-stu-id="47886-245">(*Denied*) RDP toobackend</span></span>
1. <span data-ttu-id="47886-246">Az internetes felhasználó megpróbál tooRDP tooserver AppVM01</span><span class="sxs-lookup"><span data-stu-id="47886-246">An internet user tries tooRDP tooserver AppVM01</span></span>
2. <span data-ttu-id="47886-247">Mivel nincs a kiszolgálók hálózati társított nyilvános IP-címek, ez az adatforgalom hello VNet soha ne adjon meg, és így elérni hello kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="47886-247">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter hello VNet and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="47886-248">Azonban valamilyen okból engedélyezve volt a nyilvános IP-címnek, 2 (RDP) NSG-szabály lehetővé teszi az ilyen típusú adatforgalom</span><span class="sxs-lookup"><span data-stu-id="47886-248">However if a Public IP address was enabled for some reason, NSG rule 2 (RDP) would allow this traffic</span></span>

>[!NOTE]
><span data-ttu-id="47886-249">tooRDP tooany háttérkiszolgálók ebben a példában hello IIS-kiszolgálón használt egy "ugrást mező."</span><span class="sxs-lookup"><span data-stu-id="47886-249">tooRDP tooany back-end servers in this instance, hello IIS server is used as a "jump box."</span></span> <span data-ttu-id="47886-250">Első RDP toohello IIS-kiszolgálót és majd hello IIS Server RDP toohello háttér-kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="47886-250">First RDP toohello IIS server and then from hello IIS Server RDP toohello back-end server.</span></span>
>
>

#### <a name="denied-web-toobackend-server"></a><span data-ttu-id="47886-251">(*Megtagadva*) toobackend webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="47886-251">(*Denied*) Web toobackend server</span></span>
1. <span data-ttu-id="47886-252">Az internetes felhasználó megpróbál tooaccess AppVM01 fájlba</span><span class="sxs-lookup"><span data-stu-id="47886-252">An internet user tries tooaccess a file on AppVM01</span></span>
2. <span data-ttu-id="47886-253">Mivel nincs a kiszolgálók hálózati társított nyilvános IP-címek, ez az adatforgalom hello VNet soha ne adjon meg, és így elérni hello kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="47886-253">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter hello VNet and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="47886-254">Valamilyen okból engedélyezve volt a nyilvános IP-címnek, NSG szabály 5 (Internet tooVNet) blokkolná-e a forgalom</span><span class="sxs-lookup"><span data-stu-id="47886-254">If a Public IP address was enabled for some reason, NSG rule 5 (Internet tooVNet) would block this traffic</span></span>

#### <a name="denied-web-dns-look-up-on-dns-server"></a><span data-ttu-id="47886-255">(*Megtagadva*) DNS-kiszolgálón a webes DNS szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="47886-255">(*Denied*) Web DNS look-up on DNS server</span></span>
1. <span data-ttu-id="47886-256">Az internetes felhasználó megpróbál egy belső DNS-rekordot a DNS01 mentése toolook</span><span class="sxs-lookup"><span data-stu-id="47886-256">An internet user tries toolook up an internal DNS record on DNS01</span></span>
2. <span data-ttu-id="47886-257">Mivel nincs a kiszolgálók hálózati társított nyilvános IP-címek, ez az adatforgalom hello VNet soha ne adjon meg, és így elérni hello kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="47886-257">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter hello VNet and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="47886-258">Valamilyen okból engedélyezve volt a nyilvános IP-címnek, NSG szabály 5 (Internet tooVNet) blokkolná-e a forgalom (Megjegyzés: hello internet és 1. szabály csak toohello vonatkozik, hogy szabály 1 (DNS) szeretné alkalmazni, mert hello kérelmek forráscím hello forrásaként virtuális helyi hálózat)</span><span class="sxs-lookup"><span data-stu-id="47886-258">If a Public IP address was enabled for some reason, NSG rule 5 (Internet tooVNet) would block this traffic (Note: that Rule 1 (DNS) would not apply because hello requests source address is hello internet and Rule 1 only applies toohello local VNet as hello source)</span></span>

#### <a name="denied-sql-access-on-hello-web-server"></a><span data-ttu-id="47886-259">(*Megtagadva*) SQL-hozzáférés hello webkiszolgálón való üzemeltetésekor</span><span class="sxs-lookup"><span data-stu-id="47886-259">(*Denied*) SQL access on hello web server</span></span>
1. <span data-ttu-id="47886-260">Az internetes felhasználó SQL adatokat kér IIS01</span><span class="sxs-lookup"><span data-stu-id="47886-260">An internet user requests SQL data from IIS01</span></span>
2. <span data-ttu-id="47886-261">Mivel nincs a kiszolgálók hálózati társított nyilvános IP-címek, ez az adatforgalom hello VNet soha ne adjon meg, és így elérni hello kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="47886-261">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter hello VNet and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="47886-262">Ha egy nyilvános IP-cím valamilyen okból engedélyezve lett, hello Frontend alhálózathoz kezdődik bejövő szabály feldolgozása:</span><span class="sxs-lookup"><span data-stu-id="47886-262">If a Public IP address was enabled for some reason, hello Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="47886-263">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="47886-263">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
  2. <span data-ttu-id="47886-264">NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="47886-264">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
  3. <span data-ttu-id="47886-265">NSG 3. szabály (Internet tooIIS01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="47886-265">NSG Rule 3 (Internet tooIIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="47886-266">Forgalom találatok belső IP-címe hello IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="47886-266">Traffic hits internal IP address of hello IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="47886-267">IIS01 1433-as portot, ezért nincs válasz toohello kérelmet nem figyeli.</span><span class="sxs-lookup"><span data-stu-id="47886-267">IIS01 isn't listening on port 1433, so no response toohello request</span></span>

## <a name="conclusion"></a><span data-ttu-id="47886-268">Összegzés</span><span class="sxs-lookup"><span data-stu-id="47886-268">Conclusion</span></span>
<span data-ttu-id="47886-269">Ez a példa egy viszonylag egyszerű és egyszerű mód a hello háttér-alhálózathoz, a bejövő forgalom elkülönítésére.</span><span class="sxs-lookup"><span data-stu-id="47886-269">This example is a relatively simple and straight forward way of isolating hello back-end subnet from inbound traffic.</span></span>

<span data-ttu-id="47886-270">További példákat és a hálózati biztonsági határokat egy áttekintést talál [Itt][HOME].</span><span class="sxs-lookup"><span data-stu-id="47886-270">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="47886-271">Referencia</span><span class="sxs-lookup"><span data-stu-id="47886-271">References</span></span>
### <a name="azure-resource-manager-template"></a><span data-ttu-id="47886-272">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="47886-272">Azure Resource Manager template</span></span>
<span data-ttu-id="47886-273">Ez a példa egy előre meghatározott Azure Resource Manager-sablon használja a Microsoft által kezelt GitHub-tárházban, és nyissa meg a toohello közösségi.</span><span class="sxs-lookup"><span data-stu-id="47886-273">This example uses a predefined Azure Resource Manager template in a GitHub repository maintained by Microsoft and open toohello community.</span></span> <span data-ttu-id="47886-274">Ez a sablon rögtön kívül a github webhelyen, telepíthetők, vagy letöltött és módosított toofit igényeinek.</span><span class="sxs-lookup"><span data-stu-id="47886-274">This template can be deployed straight out of GitHub, or downloaded and modified toofit your needs.</span></span> 

<span data-ttu-id="47886-275">hello fő sablon megtalálható a hello fájlt "azuredeploy.json."</span><span class="sxs-lookup"><span data-stu-id="47886-275">hello main template is in hello file named "azuredeploy.json."</span></span> <span data-ttu-id="47886-276">Ez a sablon küldheti el PowerShell vagy a parancssori felületen keresztül (fájllal hello társított "azuredeploy.parameters.json") toodeploy ezt a sablont.</span><span class="sxs-lookup"><span data-stu-id="47886-276">This template can be submitted via PowerShell or CLI (with hello associated "azuredeploy.parameters.json" file) toodeploy this template.</span></span> <span data-ttu-id="47886-277">Hello legegyszerűbb módja toouse hello "Központi telepítés tooAzure" gombra hello README.md oldalán a github webhelyen található.</span><span class="sxs-lookup"><span data-stu-id="47886-277">I find hello easiest way is toouse hello "Deploy tooAzure" button on hello README.md page at GitHub.</span></span>

<span data-ttu-id="47886-278">Ebben a példában a Githubról, majd hello Azure-portálon épülő toodeploy hello sablon kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="47886-278">toodeploy hello template that builds this example from GitHub and hello Azure portal, follow these steps:</span></span>

1. <span data-ttu-id="47886-279">Egy böngészőből keresse meg a toohello [sablon][Template]</span><span class="sxs-lookup"><span data-stu-id="47886-279">From a browser, navigate toohello [Template][Template]</span></span>
2. <span data-ttu-id="47886-280">Kattintson hello "Központi telepítés tooAzure" (vagy hello "Visualize" gomb toosee sablon grafikus ábrázolása)</span><span class="sxs-lookup"><span data-stu-id="47886-280">Click hello "Deploy tooAzure" button (or hello "Visualize" button toosee a graphical representation of this template)</span></span>
3. <span data-ttu-id="47886-281">Hello Storage-fiók, felhasználónév és jelszó hello (paraméterek) panelen adja meg, majd kattintson a **OK**</span><span class="sxs-lookup"><span data-stu-id="47886-281">Enter hello Storage Account, User Name, and Password in hello Parameters blade, then click **OK**</span></span>
5. <span data-ttu-id="47886-282">Hozzon létre egy erőforráscsoportot a központi telepítés (egy meglévőt is használhat, de egy újat a legjobb eredmények elérése érdekében javasolt)</span><span class="sxs-lookup"><span data-stu-id="47886-282">Create a Resource Group for this deployment (You can use an existing one, but I recommend a new one for best results)</span></span>
6. <span data-ttu-id="47886-283">Szükség esetén módosítsa a virtuális hálózat hello előfizetésben és helyen beállításait.</span><span class="sxs-lookup"><span data-stu-id="47886-283">If necessary, change hello Subscription and Location settings for your VNet.</span></span>
7. <span data-ttu-id="47886-284">Kattintson a **tekintse át a jogi feltételeket**, olvassa el hello feltételeit, és kattintson **beszerzési** tooagree.</span><span class="sxs-lookup"><span data-stu-id="47886-284">Click **Review legal terms**, read hello terms, and click **Purchase** tooagree.</span></span>
8. <span data-ttu-id="47886-285">Kattintson a **létrehozása** toobegin hello telepítési a sablonból.</span><span class="sxs-lookup"><span data-stu-id="47886-285">Click **Create** toobegin hello deployment of this template.</span></span>
9. <span data-ttu-id="47886-286">Miután hello központi telepítés sikeresen befejeződött, keresse meg a központi telepítés toosee hello erőforrások konfigurált belül létrehozott erőforráscsoport toohello.</span><span class="sxs-lookup"><span data-stu-id="47886-286">Once hello deployment finishes successfully, navigate toohello Resource Group created for this deployment toosee hello resources configured inside.</span></span>

>[!NOTE]
><span data-ttu-id="47886-287">Ez a sablon lehetővé teszi az RDP toohello IIS01 kiszolgáló csak (Keresés hello IIS01 nyilvános IP-Címek a portál hello).</span><span class="sxs-lookup"><span data-stu-id="47886-287">This template enables RDP toohello IIS01 server only (find hello Public IP for IIS01 on hello Portal).</span></span> <span data-ttu-id="47886-288">tooRDP tooany háttérkiszolgálók ebben a példában hello IIS-kiszolgálón használt egy "ugrást mező."</span><span class="sxs-lookup"><span data-stu-id="47886-288">tooRDP tooany back-end servers in this instance, hello IIS server is used as a "jump box."</span></span> <span data-ttu-id="47886-289">Első RDP toohello IIS-kiszolgálót és majd hello IIS Server RDP toohello háttér-kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="47886-289">First RDP toohello IIS server and then from hello IIS Server RDP toohello back-end server.</span></span>
>
>

<span data-ttu-id="47886-290">tooremove a telepítés, a delete hello erőforráscsoportot és az összes gyermek-erőforrás is törlődik.</span><span class="sxs-lookup"><span data-stu-id="47886-290">tooremove this deployment, delete hello Resource Group and all child resources will also be deleted.</span></span>

#### <a name="sample-application-scripts"></a><span data-ttu-id="47886-291">Alkalmazás mintaparancsfájlok</span><span class="sxs-lookup"><span data-stu-id="47886-291">Sample application scripts</span></span>
<span data-ttu-id="47886-292">Hello sablon sikeres futtatása után állíthat be hello webkiszolgáló és az alkalmazáskiszolgáló rendelkező egy egyszerű webes alkalmazás tooallow e DMZ konfiguráció tesztelése.</span><span class="sxs-lookup"><span data-stu-id="47886-292">Once hello template runs successfully, you can set up hello web server and app server with a simple web application tooallow testing with this DMZ configuration.</span></span> <span data-ttu-id="47886-293">Ezzel és más DMZ példák mintaalkalmazás tooinstall, egy adtak meg, a következő hivatkozás hello: [minta alkalmazás-parancsfájl][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="47886-293">tooinstall a sample application for this, and other DMZ Examples, one has been provided at hello following link: [Sample Application Script][SampleApp]</span></span>

## <a name="next-steps"></a><span data-ttu-id="47886-294">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47886-294">Next steps</span></span>

* <span data-ttu-id="47886-295">Ez a példa központi telepítése</span><span class="sxs-lookup"><span data-stu-id="47886-295">Deploy this example</span></span>
* <span data-ttu-id="47886-296">Build hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="47886-296">Build hello sample application</span></span>
* <span data-ttu-id="47886-297">A DMZ keresztül különböző forgalom tesztelése</span><span class="sxs-lookup"><span data-stu-id="47886-297">Test different traffic flows through this DMZ</span></span>

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Az NSG bejövő DMZ"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md