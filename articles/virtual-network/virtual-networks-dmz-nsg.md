---
title: "Az Azure DMZ példa – Build az NSG-ket egy egyszerű DMZ |} Microsoft Docs"
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
ms.openlocfilehash: ec29e6b250f927a3a4a94ffdf83d6c7c0e325722
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a><span data-ttu-id="a4fff-103">1 – például egy egyszerű DMZ NSG-k használata az Azure Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="a4fff-103">Example 1 – Build a simple DMZ using NSGs with an Azure Resource Manager template</span></span>
<span data-ttu-id="a4fff-104">[A biztonsági határ bevált gyakorlatok laphoz való visszatéréshez][HOME]</span><span class="sxs-lookup"><span data-stu-id="a4fff-104">[Return to the Security Boundary Best Practices Page][HOME]</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a4fff-105">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="a4fff-105">Resource Manager Template</span></span>](virtual-networks-dmz-nsg.md)
> * [<span data-ttu-id="a4fff-106">Klasszikus - PowerShell</span><span class="sxs-lookup"><span data-stu-id="a4fff-106">Classic - PowerShell</span></span>](virtual-networks-dmz-nsg-asm.md)
> 
>

<span data-ttu-id="a4fff-107">Ebben a példában egy egyszerű DMZ négy Windows-kiszolgálók és hálózati biztonsági csoportokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a4fff-107">This example creates a primitive DMZ with four Windows servers and Network Security Groups.</span></span> <span data-ttu-id="a4fff-108">Ez a példa bemutatja az egyes bemutatják az egyes lépések, így a megfelelő template szakaszokban.</span><span class="sxs-lookup"><span data-stu-id="a4fff-108">This example describes each of the relevant template sections to provide a deeper understanding of each step.</span></span> <span data-ttu-id="a4fff-109">Adjon meg egy hogyan forgalom halad keresztül a Szegélyhálózaton lévő védelmi réteget részletesebb lépésenkénti tekintse meg a forgalom forgatókönyv szakasz is van.</span><span class="sxs-lookup"><span data-stu-id="a4fff-109">There is also a Traffic Scenario section to provide an in-depth step-by-step look at how traffic proceeds through the layers of defense in the DMZ.</span></span> <span data-ttu-id="a4fff-110">Végezetül az hivatkozások szakaszban a teljes sablon kódot és az ebben a környezetben, teszteléséhez, és a különböző forgatókönyvekben kísérletezhet összeállítására vonatkozó útmutatást.</span><span class="sxs-lookup"><span data-stu-id="a4fff-110">Finally, in the references section is the complete template code and instructions to build this environment to test and experiment with various scenarios.</span></span> 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

<span data-ttu-id="a4fff-111">![Az NSG bejövő DMZ][1]</span><span class="sxs-lookup"><span data-stu-id="a4fff-111">![Inbound DMZ with NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="a4fff-112">Környezet leírása</span><span class="sxs-lookup"><span data-stu-id="a4fff-112">Environment description</span></span>
<span data-ttu-id="a4fff-113">Ebben a példában az előfizetés a következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="a4fff-113">In this example a subscription contains the following resources:</span></span>

* <span data-ttu-id="a4fff-114">Egyetlen erőforráscsoportként működnek</span><span class="sxs-lookup"><span data-stu-id="a4fff-114">A single resource group</span></span>
* <span data-ttu-id="a4fff-115">Egy virtuális hálózatot, két alhálózattal; "Előtér" és "Háttér"</span><span class="sxs-lookup"><span data-stu-id="a4fff-115">A Virtual Network with two subnets; “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="a4fff-116">Mindkét alhálózat alkalmazott hálózati biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="a4fff-116">A Network Security Group that is applied to both subnets</span></span>
* <span data-ttu-id="a4fff-117">A Windows Server egy webalkalmazás-kiszolgáló ("IIS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="a4fff-117">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="a4fff-118">Két windows Server kiszolgálókon, amelyek megfelelnek az alkalmazás háttérkiszolgálók ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="a4fff-118">Two windows servers that represent application back-end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="a4fff-119">A Windows server DNS-kiszolgáló ("DNS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="a4fff-119">A Windows server that represents a DNS server (“DNS01”)</span></span>
* <span data-ttu-id="a4fff-120">A webalkalmazás-kiszolgáló egy nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="a4fff-120">A public IP address associated with the application web server</span></span>

<span data-ttu-id="a4fff-121">A references szakaszában a környezet ebben a példában leírt épülő Azure Resource Manager sablont kapcsolat van.</span><span class="sxs-lookup"><span data-stu-id="a4fff-121">In the references section, there is a link to an Azure Resource Manager template that builds the environment described in this example.</span></span> <span data-ttu-id="a4fff-122">A virtuális gépek és virtuális hálózatok, bár végezhető el a példa sablon dokumentum nem ismerteti a jelen dokumentum részletesen.</span><span class="sxs-lookup"><span data-stu-id="a4fff-122">Building the VMs and Virtual Networks, although done by the example template, are not described in detail in this document.</span></span> 

<span data-ttu-id="a4fff-123">**Ebben a környezetben összeállításához** (részletes utasításokra van szüksége van a references szakaszában, a jelen dokumentum);</span><span class="sxs-lookup"><span data-stu-id="a4fff-123">**To build this environment** (detailed instructions are in the references section of this document);</span></span>

1. <span data-ttu-id="a4fff-124">Az Azure Resource Manager sablon telepítése: [Azure gyors üzembe helyezés sablonok][Template]</span><span class="sxs-lookup"><span data-stu-id="a4fff-124">Deploy the Azure Resource Manager Template at: [Azure Quickstart Templates][Template]</span></span>
2. <span data-ttu-id="a4fff-125">A következő mintaalkalmazás telepítése: [minta alkalmazás-parancsfájl][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="a4fff-125">Install the sample application at: [Sample Application Script][SampleApp]</span></span>

>[!NOTE]
><span data-ttu-id="a4fff-126">Távoli asztali ezt a példányt a háttér-kiszolgálók az IIS-kiszolgálón használt egy "ugrást mező."</span><span class="sxs-lookup"><span data-stu-id="a4fff-126">To RDP to any back-end servers in this instance, the IIS server is used as a "jump box."</span></span> <span data-ttu-id="a4fff-127">Az IIS-kiszolgálót, majd az IIS kiszolgáló RDP a háttér-kiszolgálóhoz az első RDP.</span><span class="sxs-lookup"><span data-stu-id="a4fff-127">First RDP to the IIS server and then from the IIS Server RDP to the back-end server.</span></span> <span data-ttu-id="a4fff-128">Másik lehetőség egy nyilvános IP-cím társítható minden kiszolgáló egyszerűbb az RDP a hálózati Adapternek.</span><span class="sxs-lookup"><span data-stu-id="a4fff-128">Alternately a Public IP can be associated with each server NIC for easier RDP.</span></span>
> 
>

<span data-ttu-id="a4fff-129">A következő szakaszokban a hálózati biztonsági csoport, és hogyan működik az ebben a példában az Azure Resource Manager sablon sorok keresztül érdekében részletes leírását.</span><span class="sxs-lookup"><span data-stu-id="a4fff-129">The following sections provide a detailed description of the Network Security Group and how it functions for this example by walking through key lines of the Azure Resource Manager Template.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="a4fff-130">Hálózati biztonsági csoportok (NSG)</span><span class="sxs-lookup"><span data-stu-id="a4fff-130">Network Security Groups (NSG)</span></span>
<span data-ttu-id="a4fff-131">Az ebben a példában egy NSG-csoport összeállítása és hat szabályokkal majd betölteni.</span><span class="sxs-lookup"><span data-stu-id="a4fff-131">For this example, an NSG group is built and then loaded with six rules.</span></span> 

>[!TIP]
><span data-ttu-id="a4fff-132">Általánosságban véve az "Engedélyezés" speciális szabályok először hozzon létre, és majd a általánosabbá "Deny" szabályok utolsó.</span><span class="sxs-lookup"><span data-stu-id="a4fff-132">Generally speaking, you should create your specific “Allow” rules first and then the more generic “Deny” rules last.</span></span> <span data-ttu-id="a4fff-133">A prioritási megkövetel amelyek szabályokat első értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="a4fff-133">The assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="a4fff-134">Forgalom kiderül, hogy egy adott szabály vonatkozik, ha nincsenek további szabályok kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="a4fff-134">Once traffic is found to apply to a specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="a4fff-135">NSG-szabályok egyaránt (az alhálózati szempontjából) a bejövő vagy kimenő irányban is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="a4fff-135">NSG rules can apply in either in the inbound or outbound direction (from the perspective of the subnet).</span></span>
>
>

<span data-ttu-id="a4fff-136">Deklaratív módon a bejövő forgalom beépített folyamatban a következő szabályokat:</span><span class="sxs-lookup"><span data-stu-id="a4fff-136">Declaratively, the following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="a4fff-137">Belső DNS-forgalom (53-as port) engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="a4fff-137">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="a4fff-138">RDP-forgalmát (3389-es port) az internetről bármely virtuális géphez engedélyezett</span><span class="sxs-lookup"><span data-stu-id="a4fff-138">RDP traffic (port 3389) from the Internet to any VM is allowed</span></span>
3. <span data-ttu-id="a4fff-139">HTTP-forgalom (80-as port) az internetről (IIS01) webkiszolgálón engedélyezett</span><span class="sxs-lookup"><span data-stu-id="a4fff-139">HTTP traffic (port 80) from the Internet to web server (IIS01) is allowed</span></span>
4. <span data-ttu-id="a4fff-140">Minden forgalmat (minden porthoz) IIS01 AppVM1 engedélyezett</span><span class="sxs-lookup"><span data-stu-id="a4fff-140">Any traffic (all ports) from IIS01 to AppVM1 is allowed</span></span>
5. <span data-ttu-id="a4fff-141">Az összes bejövő forgalom (minden porthoz) az internetről a teljes virtuális hálózatot (alhálózatok mindkét) megtagadva</span><span class="sxs-lookup"><span data-stu-id="a4fff-141">Any traffic (all ports) from the Internet to the entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="a4fff-142">Az összes bejövő forgalom (minden porthoz) a Frontend alhálózatból a Backend alhálózathoz megtagadva</span><span class="sxs-lookup"><span data-stu-id="a4fff-142">Any traffic (all ports) from the Frontend subnet to the Backend subnet is Denied</span></span>

<span data-ttu-id="a4fff-143">A következő szabályok kötve alhálózatok, ha a webkiszolgálónak, a szabályokat is 3 az internetről bejövő HTTP-kérelem (engedélyezése) és 5 (megtagadni) szeretné alkalmazni, de mivel a szabály 3 egy magasabb prioritással bír, csak azt csak akkor vonatkozik, és 5 szabály nem lesz play kerülhet.</span><span class="sxs-lookup"><span data-stu-id="a4fff-143">With these rules bound to each subnet, if an HTTP request was inbound from the Internet to the web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="a4fff-144">Így a HTTP-kérelem szeretné engedélyezni kell a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a4fff-144">Thus the HTTP request would be allowed to the web server.</span></span> <span data-ttu-id="a4fff-145">Ha ugyanaz a forgalom próbál elérni az DNS01 kiszolgálót, szabály 5 (Megtagadás) lenne az első alkalmazni, és szeretné, hogy a kiszolgáló nem engedélyezett a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="a4fff-145">If that same traffic was trying to reach the DNS01 server, rule 5 (Deny) would be the first to apply and the traffic would not be allowed to pass to the server.</span></span> <span data-ttu-id="a4fff-146">6 (Megtagadás) szabály blokkolja a Frontend alhálózathoz van szó, a Backend alhálózathoz (kivéve a engedélyezett forgalom szabályokban 1 és 4) a, a szabálykészlet abban az esetben, ha egy támadó biztonság sérüléseinek előtér, a támadó a webalkalmazás volna korlátozott védelmet nyújt a háttér hálózathoz a háttérrendszer elérésére "védett" hálózaton (csak a AppVM01 kiszolgálón kitett erőforrások).</span><span class="sxs-lookup"><span data-stu-id="a4fff-146">Rule 6 (Deny) blocks the Frontend subnet from talking to the Backend subnet (except for allowed traffic in rules 1 and 4), this rule-set protects the Backend network in case an attacker compromises the web application on the Frontend, the attacker would have limited access to the Backend “protected” network (only to resources exposed on the AppVM01 server).</span></span>

<span data-ttu-id="a4fff-147">Nincs olyan alapértelmezett kimenő szabály, amely lehetővé teszi, hogy a kimenő forgalom az internethez.</span><span class="sxs-lookup"><span data-stu-id="a4fff-147">There is a default outbound rule that allows traffic out to the internet.</span></span> <span data-ttu-id="a4fff-148">Ebben a példában a Microsoft most átengedi a kimenő forgalmat, és nem módosítja az egyetlen kimenő szabályok.</span><span class="sxs-lookup"><span data-stu-id="a4fff-148">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="a4fff-149">Szeretné alkalmazni a forgalom mindkét irányban biztonsági szabályzatot, felhasználói definiált útválasztási szükséges, és a"3" a felfedezte van a [biztonsági határ bevált gyakorlatok lap][HOME].</span><span class="sxs-lookup"><span data-stu-id="a4fff-149">To apply security policy to traffic in both directions, User Defined Routing is required and is explored in “Example 3” on the [Security Boundary Best Practices Page][HOME].</span></span>

<span data-ttu-id="a4fff-150">Minden egyes szabály az alábbiak szerint tárgyalja részletesen:</span><span class="sxs-lookup"><span data-stu-id="a4fff-150">Each rule is discussed in more detail as follows:</span></span>

1. <span data-ttu-id="a4fff-151">A hálózati biztonsági csoport erőforrás kell példányosítani ahhoz, hogy a szabályok:</span><span class="sxs-lookup"><span data-stu-id="a4fff-151">A Network Security Group resource must be instantiated to hold the rules:</span></span>

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

2. <span data-ttu-id="a4fff-152">Ebben a példában az első szabály lehetővé teszi, hogy a DNS-forgalom a DNS-kiszolgálót a backend alhálózathoz az összes belső hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="a4fff-152">The first rule in this example allows DNS traffic between all internal networks to the DNS server on the backend subnet.</span></span> <span data-ttu-id="a4fff-153">A szabály néhány fontos paraméterekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="a4fff-153">The rule has some important parameters:</span></span>
  * <span data-ttu-id="a4fff-154">"destinationAddressPrefix" - szabályok használhatja az "alapértelmezett címke" nevű címelőtag különleges típusú, és ezekkel a címkékkel rendszer által biztosított azonosítók, amelyek lehetővé teszik a címelőtag egy nagyobb kategóriáját egyszerűen.</span><span class="sxs-lookup"><span data-stu-id="a4fff-154">"destinationAddressPrefix" - Rules can use a special type of address prefix called a "Default Tag", these tags are system-provided identifiers that allow an easy way to address a larger category of address prefixes.</span></span> <span data-ttu-id="a4fff-155">Ez a szabály az alapértelmezett címke "Internet" használja a virtuális hálózaton kívül bármely cím jelölésére.</span><span class="sxs-lookup"><span data-stu-id="a4fff-155">This rule uses the Default Tag “Internet” to signify any address outside of the VNet.</span></span> <span data-ttu-id="a4fff-156">Más előtag feliratai VirtualNetwork és AzureLoadBalancer.</span><span class="sxs-lookup"><span data-stu-id="a4fff-156">Other prefix labels are VirtualNetwork and AzureLoadBalancer.</span></span>
  * <span data-ttu-id="a4fff-157">"Irány" azt jelzi, hogy a forgalom iránya a szabály lép érvénybe.</span><span class="sxs-lookup"><span data-stu-id="a4fff-157">“Direction” signifies in which direction of traffic flow this rule takes effect.</span></span> <span data-ttu-id="a4fff-158">Irányát (attól függően, ahol az NSG kötött) az alhálózatra vagy a virtuális gép szempontjából van.</span><span class="sxs-lookup"><span data-stu-id="a4fff-158">The direction is from the perspective of the subnet or Virtual Machine (depending on where this NSG is bound).</span></span> <span data-ttu-id="a4fff-159">Így ha irány "Bejövő" és a forgalom írja be az alhálózat, a szabályt alkalmazni és az alhálózatot elhagyó forgalomra ne befolyásolja az e szabály által.</span><span class="sxs-lookup"><span data-stu-id="a4fff-159">Thus if Direction is “Inbound” and traffic is entering the subnet, the rule would apply and traffic leaving the subnet would not be affected by this rule.</span></span>
  * <span data-ttu-id="a4fff-160">"Prioritás" állítja be, amelyben a forgalom áramlását kiértékelésének.</span><span class="sxs-lookup"><span data-stu-id="a4fff-160">“Priority” sets the order in which a traffic flow is evaluated.</span></span> <span data-ttu-id="a4fff-161">Minél kisebb számot annál magasabb a prioritás.</span><span class="sxs-lookup"><span data-stu-id="a4fff-161">The lower the number the higher the priority.</span></span> <span data-ttu-id="a4fff-162">A szabály vonatkozik egy adott adatforgalmat, ha nincsenek további szabályok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="a4fff-162">When a rule applies to a specific traffic flow, no further rules are processed.</span></span> <span data-ttu-id="a4fff-163">Így ha 1 prioritású szabály lehetővé teszi, hogy a forgalmat, és 2 prioritású szabály megtagadja a forgalmat, és szabályokat is vonatkoznak majd a forgalmat szeretné engedélyezni szeretné a flow (mivel szabály 1 kellett magasabb prioritású hatás tartott, és nincsenek további szabályok alkalmazása megtörtént).</span><span class="sxs-lookup"><span data-stu-id="a4fff-163">Thus if a rule with priority 1 allows traffic, and a rule with priority 2 denies traffic, and both rules apply to traffic then the traffic would be allowed to flow (since rule 1 had a higher priority it took effect and no further rules were applied).</span></span>
  * <span data-ttu-id="a4fff-164">"Access" jelzi, hogy a szabály által érintett forgalom-e letiltott ("Deny") vagy engedélyezett ("Engedélyezés").</span><span class="sxs-lookup"><span data-stu-id="a4fff-164">“Access” signifies if traffic affected by this rule is blocked ("Deny") or allowed ("Allow").</span></span>

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

3. <span data-ttu-id="a4fff-165">Ez a szabály lehetővé teszi, hogy az internetről érkező RDP-portjára kötött alhálózaton egyetlen kiszolgálón történő flow RDP-forgalmát.</span><span class="sxs-lookup"><span data-stu-id="a4fff-165">This rule allows RDP traffic to flow from the internet to the RDP port on any server on the bound subnet.</span></span> 

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

4. <span data-ttu-id="a4fff-166">Ez a szabály lehetővé teszi, hogy a webalkalmazás-kiszolgáló elérte a bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="a4fff-166">This rule allows inbound internet traffic to hit the web server.</span></span> <span data-ttu-id="a4fff-167">Ez a szabály nem változtatja meg az útválasztási viselkedés.</span><span class="sxs-lookup"><span data-stu-id="a4fff-167">This rule does not change the routing behavior.</span></span> <span data-ttu-id="a4fff-168">A szabály csak lehetővé teszi, hogy a IIS01 továbbítani adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="a4fff-168">The rule only allows traffic destined for IIS01 to pass.</span></span> <span data-ttu-id="a4fff-169">Így ha az internetről érkező forgalom volt-e a célként, ez a szabály akkor engedélyezi-e, és további szabályok feldolgozásának leállítása a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a4fff-169">Thus if traffic from the Internet had the web server as its destination this rule would allow it and stop processing further rules.</span></span> <span data-ttu-id="a4fff-170">(A szabályban prioritással 140 összes egyéb bejövő forgalmat le van tiltva).</span><span class="sxs-lookup"><span data-stu-id="a4fff-170">(In the rule at priority 140 all other inbound internet traffic is blocked).</span></span> <span data-ttu-id="a4fff-171">Ha csak a HTTP-forgalom éppen feldolgozás, ez a szabály sikertelen további korlátozva csak engedélyezi a 80-as Port cél.</span><span class="sxs-lookup"><span data-stu-id="a4fff-171">If you're only processing HTTP traffic, this rule could be further restricted to only allow Destination Port 80.</span></span>

    ```JSON
    {
      "name": "enable_web_rule",
      "properties": {
        "description": "Enable Internet to [variables('VM01Name')]",
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

5. <span data-ttu-id="a4fff-172">Ez a szabály lehetővé teszi, hogy a forgalom a IIS01 kiszolgáló számára a AppVM01 kiszolgáló egy újabb szabály blokkolja a háttér-forgalom minden más előtér.</span><span class="sxs-lookup"><span data-stu-id="a4fff-172">This rule allows traffic to pass from the IIS01 server to the AppVM01 server, a later rule blocks all other Frontend to Backend traffic.</span></span> <span data-ttu-id="a4fff-173">Ez a szabály akkor javíthatja, ha a port ismert, hogy hozzá kell adni.</span><span class="sxs-lookup"><span data-stu-id="a4fff-173">To improve this rule, if the port is known that should be added.</span></span> <span data-ttu-id="a4fff-174">Például, ha az IIS-kiszolgálón van elérte-e a AppVM01 csak SQL Server, a Célporttartomány módosítani kell a "*" (minden) 1433 (az SQL-port), így a kisebb bejövő támadási felületét AppVM01 kell a webalkalmazás legalább egyszer utaló jeleket.</span><span class="sxs-lookup"><span data-stu-id="a4fff-174">For example, if the IIS server is hitting only SQL Server on AppVM01, the Destination Port Range should be changed from “*” (Any) to 1433 (the SQL port) thus allowing a smaller inbound attack surface on AppVM01 should the web application ever be compromised.</span></span>

    ```JSON
    {
      "name": "enable_app_rule",
      "properties": {
        "description": "Enable [variables('VM01Name')] to [variables('VM02Name')]",
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

6. <span data-ttu-id="a4fff-175">Ez a szabály az internetről érkező forgalom megtagadja a hálózaton lévő kiszolgálók számára.</span><span class="sxs-lookup"><span data-stu-id="a4fff-175">This rule denies traffic from the internet to any servers on the network.</span></span> <span data-ttu-id="a4fff-176">A 110-es és 120 prioritással szabályokat a hatás hoz csak a bejövő internet-kezelési forgalom engedélyezése a tűzfal és a kiszolgálókon és blokkok RDP-portok minden más.</span><span class="sxs-lookup"><span data-stu-id="a4fff-176">With the rules at priority 110 and 120, the effect is to allow only inbound internet traffic to the firewall and RDP ports on servers and blocks everything else.</span></span> <span data-ttu-id="a4fff-177">Ez a szabály szerepel egy "hibamentes" szabályt, amely minden váratlan forgalom blokkolása.</span><span class="sxs-lookup"><span data-stu-id="a4fff-177">This rule is a "fail-safe" rule to block all unexpected flows.</span></span>

    ```JSON
    {
      "name": "deny_internet_rule",
      "properties": {
        "description": "Isolate the [variables('VNetName')] VNet from the Internet",
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

7. <span data-ttu-id="a4fff-178">A végső szabály pedig megtagadja forgalom a Frontend alhálózatból való a Backend alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="a4fff-178">The final rule denies traffic from the Frontend subnet to the Backend subnet.</span></span> <span data-ttu-id="a4fff-179">Mivel ez a szabály egy bejövő forgalomra vonatkozó csak szabály, akkor (érkező a Háttéralkalmazását az az előtérbeli) egy fordított forgalom nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="a4fff-179">Since this rule is an Inbound only rule, reverse traffic is allowed (from the Backend to the Frontend).</span></span>

    ```JSON
    {
      "name": "deny_frontend_rule",
      "properties": {
        "description": "Isolate the [variables('Subnet1Name')] subnet from the [variables('Subnet2Name')] subnet",
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

## <a name="traffic-scenarios"></a><span data-ttu-id="a4fff-180">Forgalom forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="a4fff-180">Traffic scenarios</span></span>
#### <a name="allowed-internet-to-web-server"></a><span data-ttu-id="a4fff-181">(*Engedélyezett*) webkiszolgálón Internet</span><span class="sxs-lookup"><span data-stu-id="a4fff-181">(*Allowed*) Internet to web server</span></span>
1. <span data-ttu-id="a4fff-182">Az internetes felhasználó egy HTTP-lap kér a nyilvános IP-címet a hálózati adapter IIS01 társított hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="a4fff-182">An internet user requests an HTTP page from the public IP address of the NIC associated with the IIS01 NIC</span></span>
2. <span data-ttu-id="a4fff-183">A nyilvános IP-cím forgalmat továbbítja a VNet IIS01 felé (webkiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="a4fff-183">The Public IP address passes traffic to the VNet towards IIS01 (the web server)</span></span>
3. <span data-ttu-id="a4fff-184">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="a4fff-184">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="a4fff-185">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="a4fff-185">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="a4fff-186">NSG szabály 2 (RDP) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="a4fff-186">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
  3. <span data-ttu-id="a4fff-187">NSG 3. szabály (IIS01 interneten) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="a4fff-187">NSG Rule 3 (Internet to IIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="a4fff-188">Forgalom találatok száma a belső kiszolgáló IP-címét a webes IIS01 (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="a4fff-188">Traffic hits internal IP address of the web server IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="a4fff-189">IIS01 figyeli a webes forgalom, ezt a kérelmet kap, és elindítja a kérés feldolgozása</span><span class="sxs-lookup"><span data-stu-id="a4fff-189">IIS01 is listening for web traffic, receives this request and starts processing the request</span></span>
6. <span data-ttu-id="a4fff-190">IIS01 az SQL Server a AppVM01 adatokat kéri</span><span class="sxs-lookup"><span data-stu-id="a4fff-190">IIS01 asks the SQL Server on AppVM01 for information</span></span>
7. <span data-ttu-id="a4fff-191">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="a4fff-191">No outbound rules on Frontend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="a4fff-192">A Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="a4fff-192">The Backend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="a4fff-193">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="a4fff-193">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="a4fff-194">NSG szabály 2 (RDP) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="a4fff-194">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
  3. <span data-ttu-id="a4fff-195">NSG 3. szabály (Internet tűzfalhoz) nem teljesül, a közvetkező szabályának áthelyezése</span><span class="sxs-lookup"><span data-stu-id="a4fff-195">NSG Rule 3 (Internet to Firewall) doesn’t apply, move to next rule</span></span>
  4. <span data-ttu-id="a4fff-196">NSG 4. szabály (a AppVM01 IIS01) teljesül, a forgalom engedélyezve van, akkor állítsa le a szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="a4fff-196">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
9. <span data-ttu-id="a4fff-197">AppVM01 dokumentálásáért és az SQL-lekérdezést kap</span><span class="sxs-lookup"><span data-stu-id="a4fff-197">AppVM01 receives the SQL Query and responds</span></span>
10. <span data-ttu-id="a4fff-198">Mivel a Backend alhálózathoz nem kimenő szabályok, engedélyezett-e a válasz</span><span class="sxs-lookup"><span data-stu-id="a4fff-198">Since there are no outbound rules on the Backend subnet, the response is allowed</span></span>
11. <span data-ttu-id="a4fff-199">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="a4fff-199">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="a4fff-200">Nincs érvényes bejövő szabály NSG a Frontend alhálózathoz, így nincs az NSG-szabályok vonatkoznak a Backend alhálózathoz-forgalom</span><span class="sxs-lookup"><span data-stu-id="a4fff-200">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
  2. <span data-ttu-id="a4fff-201">A alapértelmezett rendszerszintű szabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, a forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="a4fff-201">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
12. <span data-ttu-id="a4fff-202">Az IIS-kiszolgálót az SQL-válasz fogadása és a HTTP-válasz befejezése és a kérelmező küld</span><span class="sxs-lookup"><span data-stu-id="a4fff-202">The IIS server receives the SQL response and completes the HTTP response and sends to the requester</span></span>
13. <span data-ttu-id="a4fff-203">Nincsenek kimenő szabályok Frontend alhálózaton található, mert a választ kap, és az internetes felhasználói kap, a kért weblap.</span><span class="sxs-lookup"><span data-stu-id="a4fff-203">Since there are no outbound rules on the Frontend subnet, the response is allowed and the Internet User receives the web page requested.</span></span>

#### <a name="allowed-rdp-to-iis-server"></a><span data-ttu-id="a4fff-204">(*Engedélyezett*) RDP IIS-kiszolgálóra</span><span class="sxs-lookup"><span data-stu-id="a4fff-204">(*Allowed*) RDP to IIS server</span></span>
1. <span data-ttu-id="a4fff-205">Egy kiszolgálói rendszergazda interneten igényel a nyilvános IP-címhez a IIS01 hálózati adapter (a nyilvános IP-cím található a portál vagy a PowerShell használatával) társított hálózati adapter IIS01 az RDP-munkamenetet</span><span class="sxs-lookup"><span data-stu-id="a4fff-205">A Server Admin on internet requests an RDP session to IIS01 on the public IP address of the NIC associated with the IIS01 NIC (this public IP address can be found via the Portal or PowerShell)</span></span>
2. <span data-ttu-id="a4fff-206">A nyilvános IP-cím forgalmat továbbítja a VNet IIS01 felé (webkiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="a4fff-206">The Public IP address passes traffic to the VNet towards IIS01 (the web server)</span></span>
3. <span data-ttu-id="a4fff-207">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="a4fff-207">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="a4fff-208">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="a4fff-208">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="a4fff-209">NSG szabály 2 (RDP) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="a4fff-209">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="a4fff-210">A nem kimenő szabályokat alapértelmezett szabályokat alkalmazni, és a forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="a4fff-210">With no outbound rules, default rules apply and return traffic is allowed</span></span>
5. <span data-ttu-id="a4fff-211">RDP-munkamenetbe engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="a4fff-211">RDP session is enabled</span></span>
6. <span data-ttu-id="a4fff-212">A felhasználónevet és jelszót kér IIS01</span><span class="sxs-lookup"><span data-stu-id="a4fff-212">IIS01 prompts for the user name and password</span></span>

>[!NOTE]
><span data-ttu-id="a4fff-213">Távoli asztali ezt a példányt a háttér-kiszolgálók az IIS-kiszolgálón használt egy "ugrást mező."</span><span class="sxs-lookup"><span data-stu-id="a4fff-213">To RDP to any back-end servers in this instance, the IIS server is used as a "jump box."</span></span> <span data-ttu-id="a4fff-214">Az IIS-kiszolgálót, majd az IIS kiszolgáló RDP a háttér-kiszolgálóhoz az első RDP.</span><span class="sxs-lookup"><span data-stu-id="a4fff-214">First RDP to the IIS server and then from the IIS Server RDP to the back-end server.</span></span>
>
>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a><span data-ttu-id="a4fff-215">(*Engedélyezett*) a DNS-kiszolgáló Web server a DNS szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="a4fff-215">(*Allowed*) Web server DNS look-up on DNS server</span></span>
1. <span data-ttu-id="a4fff-216">Webalkalmazás-kiszolgálón, IIS01, egy adatcsatorna www.data.gov: igényeinek, de a címek feloldására igényeinek.</span><span class="sxs-lookup"><span data-stu-id="a4fff-216">Web Server, IIS01, needs a data feed at www.data.gov, but needs to resolve the address.</span></span>
2. <span data-ttu-id="a4fff-217">A hálózati konfigurációt a VNet listák DNS01 (a háttér alhálózaton 10.0.2.4) elsődleges DNS-kiszolgálóként, IIS01 küld a DNS-kérelem DNS01</span><span class="sxs-lookup"><span data-stu-id="a4fff-217">The network configuration for the VNet lists DNS01 (10.0.2.4 on the Backend subnet) as the primary DNS server, IIS01 sends the DNS request to DNS01</span></span>
3. <span data-ttu-id="a4fff-218">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="a4fff-218">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="a4fff-219">Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="a4fff-219">Backend subnet begins inbound rule processing:</span></span>
  * <span data-ttu-id="a4fff-220">NSG szabály 1 (DNS) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="a4fff-220">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="a4fff-221">DNS-kiszolgáló a kérelmet kap.</span><span class="sxs-lookup"><span data-stu-id="a4fff-221">DNS server receives the request</span></span>
6. <span data-ttu-id="a4fff-222">DNS-kiszolgáló nem rendelkezik a gyorsítótárazott címmel és egy legfelső szintű DNS-kiszolgáló kéri az interneten</span><span class="sxs-lookup"><span data-stu-id="a4fff-222">DNS server doesn’t have the address cached and asks a root DNS server on the internet</span></span>
7. <span data-ttu-id="a4fff-223">Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="a4fff-223">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="a4fff-224">Internetes DNS-kiszolgáló válaszol, mivel ehhez a munkamenethez belső kezdeményezett, a válasz engedélyezett</span><span class="sxs-lookup"><span data-stu-id="a4fff-224">Internet DNS server responds, since this session was initiated internally, the response is allowed</span></span>
9. <span data-ttu-id="a4fff-225">DNS-kiszolgáló gyorsítótárazza a választ, és a kezdeti kérés vissza IIS01 válaszol</span><span class="sxs-lookup"><span data-stu-id="a4fff-225">DNS server caches the response, and responds to the initial request back to IIS01</span></span>
10. <span data-ttu-id="a4fff-226">Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="a4fff-226">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="a4fff-227">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="a4fff-227">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="a4fff-228">Nincs érvényes bejövő szabály NSG a Frontend alhálózathoz, így nincs az NSG-szabályok vonatkoznak a Backend alhálózathoz-forgalom</span><span class="sxs-lookup"><span data-stu-id="a4fff-228">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
  2. <span data-ttu-id="a4fff-229">A alapértelmezett rendszerszintű szabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így a forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="a4fff-229">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed</span></span>
12. <span data-ttu-id="a4fff-230">IIS01 megkapja válaszát DNS01</span><span class="sxs-lookup"><span data-stu-id="a4fff-230">IIS01 receives the response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="a4fff-231">(*Engedélyezett*) server-hozzáférés fájl AppVM01</span><span class="sxs-lookup"><span data-stu-id="a4fff-231">(*Allowed*) Web server access file on AppVM01</span></span>
1. <span data-ttu-id="a4fff-232">IIS01 kér AppVM01 fájlba</span><span class="sxs-lookup"><span data-stu-id="a4fff-232">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="a4fff-233">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="a4fff-233">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="a4fff-234">A Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="a4fff-234">The Backend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="a4fff-235">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="a4fff-235">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="a4fff-236">NSG szabály 2 (RDP) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="a4fff-236">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
  3. <span data-ttu-id="a4fff-237">NSG 3. szabály (IIS01 interneten) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="a4fff-237">NSG Rule 3 (Internet to IIS01) doesn’t apply, move to next rule</span></span>
  4. <span data-ttu-id="a4fff-238">NSG 4. szabály (a AppVM01 IIS01) teljesül, a forgalom engedélyezve van, akkor állítsa le a szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="a4fff-238">NSG Rule 4 (IIS01 to AppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="a4fff-239">AppVM01 a kérelmet kap, és válaszol, a fájl (feltéve, hogy van engedélyezve)</span><span class="sxs-lookup"><span data-stu-id="a4fff-239">AppVM01 receives the request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="a4fff-240">Mivel a Backend alhálózathoz nem kimenő szabályok, engedélyezett-e a válasz</span><span class="sxs-lookup"><span data-stu-id="a4fff-240">Since there are no outbound rules on the Backend subnet, the response is allowed</span></span>
6. <span data-ttu-id="a4fff-241">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="a4fff-241">Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="a4fff-242">Nincs érvényes bejövő szabály NSG a Frontend alhálózathoz, így nincs az NSG-szabályok vonatkoznak a Backend alhálózathoz-forgalom</span><span class="sxs-lookup"><span data-stu-id="a4fff-242">There is no NSG rule that applies to Inbound traffic from the Backend subnet to the Frontend subnet, so none of the NSG rules apply</span></span>
  2. <span data-ttu-id="a4fff-243">A alapértelmezett rendszerszintű szabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, a forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="a4fff-243">The default system rule allowing traffic between subnets would allow this traffic so the traffic is allowed.</span></span>
7. <span data-ttu-id="a4fff-244">Az IIS-kiszolgálót kap a fájl</span><span class="sxs-lookup"><span data-stu-id="a4fff-244">The IIS server receives the file</span></span>

#### <a name="denied-rdp-to-backend"></a><span data-ttu-id="a4fff-245">(*Megtagadva*) RDP háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="a4fff-245">(*Denied*) RDP to backend</span></span>
1. <span data-ttu-id="a4fff-246">Az internetes felhasználó megpróbálja távoli asztali kiszolgáló AppVM01</span><span class="sxs-lookup"><span data-stu-id="a4fff-246">An internet user tries to RDP to server AppVM01</span></span>
2. <span data-ttu-id="a4fff-247">Mivel nincs a kiszolgálók hálózati társított nyilvános IP-címek, ez az adatforgalom soha ne adjon meg a virtuális hálózat, és így elérni a kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="a4fff-247">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter the VNet and wouldn’t reach the server</span></span>
3. <span data-ttu-id="a4fff-248">Azonban valamilyen okból engedélyezve volt a nyilvános IP-címnek, 2 (RDP) NSG-szabály lehetővé teszi az ilyen típusú adatforgalom</span><span class="sxs-lookup"><span data-stu-id="a4fff-248">However if a Public IP address was enabled for some reason, NSG rule 2 (RDP) would allow this traffic</span></span>

>[!NOTE]
><span data-ttu-id="a4fff-249">Távoli asztali ezt a példányt a háttér-kiszolgálók az IIS-kiszolgálón használt egy "ugrást mező."</span><span class="sxs-lookup"><span data-stu-id="a4fff-249">To RDP to any back-end servers in this instance, the IIS server is used as a "jump box."</span></span> <span data-ttu-id="a4fff-250">Az IIS-kiszolgálót, majd az IIS kiszolgáló RDP a háttér-kiszolgálóhoz az első RDP.</span><span class="sxs-lookup"><span data-stu-id="a4fff-250">First RDP to the IIS server and then from the IIS Server RDP to the back-end server.</span></span>
>
>

#### <a name="denied-web-to-backend-server"></a><span data-ttu-id="a4fff-251">(*Megtagadva*) háttérkiszolgálóra webes</span><span class="sxs-lookup"><span data-stu-id="a4fff-251">(*Denied*) Web to backend server</span></span>
1. <span data-ttu-id="a4fff-252">Az internetes felhasználó megpróbál hozzáférni az AppVM01 fájlba</span><span class="sxs-lookup"><span data-stu-id="a4fff-252">An internet user tries to access a file on AppVM01</span></span>
2. <span data-ttu-id="a4fff-253">Mivel nincs a kiszolgálók hálózati társított nyilvános IP-címek, ez az adatforgalom soha ne adjon meg a virtuális hálózat, és így elérni a kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="a4fff-253">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter the VNet and wouldn’t reach the server</span></span>
3. <span data-ttu-id="a4fff-254">Valamilyen okból engedélyezve volt a nyilvános IP-címnek, NSG szabály 5 (Internet virtuális hálózatba) blokkolná-e a forgalom</span><span class="sxs-lookup"><span data-stu-id="a4fff-254">If a Public IP address was enabled for some reason, NSG rule 5 (Internet to VNet) would block this traffic</span></span>

#### <a name="denied-web-dns-look-up-on-dns-server"></a><span data-ttu-id="a4fff-255">(*Megtagadva*) DNS-kiszolgálón a webes DNS szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="a4fff-255">(*Denied*) Web DNS look-up on DNS server</span></span>
1. <span data-ttu-id="a4fff-256">Az internetes felhasználó megpróbálja kereshet meg egy belső DNS-rekordot a DNS01</span><span class="sxs-lookup"><span data-stu-id="a4fff-256">An internet user tries to look up an internal DNS record on DNS01</span></span>
2. <span data-ttu-id="a4fff-257">Mivel nincs a kiszolgálók hálózati társított nyilvános IP-címek, ez az adatforgalom soha ne adjon meg a virtuális hálózat, és így elérni a kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="a4fff-257">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter the VNet and wouldn’t reach the server</span></span>
3. <span data-ttu-id="a4fff-258">Valamilyen okból engedélyezve volt a nyilvános IP-címnek, NSG szabály 5 (Internet virtuális hálózatba) blokkolná-e a forgalom (Megjegyzés:, hogy szabály 1 (DNS) szeretné alkalmazni, mert a kérelem forráscím az internethez, és 1. szabály csak a helyi virtuális hálózat, mint a forrás vonatkozik)</span><span class="sxs-lookup"><span data-stu-id="a4fff-258">If a Public IP address was enabled for some reason, NSG rule 5 (Internet to VNet) would block this traffic (Note: that Rule 1 (DNS) would not apply because the requests source address is the internet and Rule 1 only applies to the local VNet as the source)</span></span>

#### <a name="denied-sql-access-on-the-web-server"></a><span data-ttu-id="a4fff-259">(*Megtagadva*) SQL-hozzáférés a webkiszolgálón</span><span class="sxs-lookup"><span data-stu-id="a4fff-259">(*Denied*) SQL access on the web server</span></span>
1. <span data-ttu-id="a4fff-260">Az internetes felhasználó SQL adatokat kér IIS01</span><span class="sxs-lookup"><span data-stu-id="a4fff-260">An internet user requests SQL data from IIS01</span></span>
2. <span data-ttu-id="a4fff-261">Mivel nincs a kiszolgálók hálózati társított nyilvános IP-címek, ez az adatforgalom soha ne adjon meg a virtuális hálózat, és így elérni a kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="a4fff-261">Since there are no Public IP addresses associated with this servers NIC, this traffic would never enter the VNet and wouldn’t reach the server</span></span>
3. <span data-ttu-id="a4fff-262">Ha egy nyilvános IP-cím valamilyen okból engedélyezve lett, a Frontend alhálózathoz kezdődik bejövő szabály feldolgozása:</span><span class="sxs-lookup"><span data-stu-id="a4fff-262">If a Public IP address was enabled for some reason, the Frontend subnet begins inbound rule processing:</span></span>
  1. <span data-ttu-id="a4fff-263">NSG szabály 1 (DNS) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="a4fff-263">NSG Rule 1 (DNS) doesn’t apply, move to next rule</span></span>
  2. <span data-ttu-id="a4fff-264">NSG szabály 2 (RDP) nem teljesül, a következő szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="a4fff-264">NSG Rule 2 (RDP) doesn’t apply, move to next rule</span></span>
  3. <span data-ttu-id="a4fff-265">NSG 3. szabály (IIS01 interneten) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="a4fff-265">NSG Rule 3 (Internet to IIS01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="a4fff-266">Forgalom találatok a IIS01 belső IP-címét (10.0.1.5)</span><span class="sxs-lookup"><span data-stu-id="a4fff-266">Traffic hits internal IP address of the IIS01 (10.0.1.5)</span></span>
5. <span data-ttu-id="a4fff-267">IIS01 1433-as portot, ezért nem kérelemre adott válasz nem figyel.</span><span class="sxs-lookup"><span data-stu-id="a4fff-267">IIS01 isn't listening on port 1433, so no response to the request</span></span>

## <a name="conclusion"></a><span data-ttu-id="a4fff-268">Összegzés</span><span class="sxs-lookup"><span data-stu-id="a4fff-268">Conclusion</span></span>
<span data-ttu-id="a4fff-269">Ez a példa egy viszonylag egyszerű és egyszerű mód a a háttér-alhálózathoz, a bejövő forgalom elkülönítésére.</span><span class="sxs-lookup"><span data-stu-id="a4fff-269">This example is a relatively simple and straight forward way of isolating the back-end subnet from inbound traffic.</span></span>

<span data-ttu-id="a4fff-270">További példákat és a hálózati biztonsági határokat egy áttekintést talál [Itt][HOME].</span><span class="sxs-lookup"><span data-stu-id="a4fff-270">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="a4fff-271">Referencia</span><span class="sxs-lookup"><span data-stu-id="a4fff-271">References</span></span>
### <a name="azure-resource-manager-template"></a><span data-ttu-id="a4fff-272">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="a4fff-272">Azure Resource Manager template</span></span>
<span data-ttu-id="a4fff-273">Ez a példa egy előre meghatározott Azure Resource Manager-sablon használja a Microsoft által kezelt GitHub-tárházban, és a Közösség.</span><span class="sxs-lookup"><span data-stu-id="a4fff-273">This example uses a predefined Azure Resource Manager template in a GitHub repository maintained by Microsoft and open to the community.</span></span> <span data-ttu-id="a4fff-274">Ez a sablon telepített rögtön kívül a github webhelyen, vagy letölthető, és szükség szerint módosítva.</span><span class="sxs-lookup"><span data-stu-id="a4fff-274">This template can be deployed straight out of GitHub, or downloaded and modified to fit your needs.</span></span> 

<span data-ttu-id="a4fff-275">A fő sablon megtalálható-e az "azuredeploy.json." nevű fájlt.</span><span class="sxs-lookup"><span data-stu-id="a4fff-275">The main template is in the file named "azuredeploy.json."</span></span> <span data-ttu-id="a4fff-276">Ez a sablon küldheti el PowerShell vagy a parancssori felületen keresztül (fájllal a társított "azuredeploy.parameters.json") a sablon telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a4fff-276">This template can be submitted via PowerShell or CLI (with the associated "azuredeploy.parameters.json" file) to deploy this template.</span></span> <span data-ttu-id="a4fff-277">A legegyszerűbb módja a használja a "Központi telepítése az Azure-bA" gombot a README.md lap a github webhelyen található.</span><span class="sxs-lookup"><span data-stu-id="a4fff-277">I find the easiest way is to use the "Deploy to Azure" button on the README.md page at GitHub.</span></span>

<span data-ttu-id="a4fff-278">A sablon épülő ebben a példában a Githubról, majd az Azure-portálon történő üzembe helyezéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a4fff-278">To deploy the template that builds this example from GitHub and the Azure portal, follow these steps:</span></span>

1. <span data-ttu-id="a4fff-279">Egy böngészőből keresse meg a [sablon][Template]</span><span class="sxs-lookup"><span data-stu-id="a4fff-279">From a browser, navigate to the [Template][Template]</span></span>
2. <span data-ttu-id="a4fff-280">Kattintson a "Központi telepítése az Azure-bA" (vagy a "Megjelenítés" gombra a sablon grafikus ábrázolása)</span><span class="sxs-lookup"><span data-stu-id="a4fff-280">Click the "Deploy to Azure" button (or the "Visualize" button to see a graphical representation of this template)</span></span>
3. <span data-ttu-id="a4fff-281">A Storage-fiók, felhasználónév és jelszó a (paraméterek) panelen adja meg, majd kattintson a **OK**</span><span class="sxs-lookup"><span data-stu-id="a4fff-281">Enter the Storage Account, User Name, and Password in the Parameters blade, then click **OK**</span></span>
5. <span data-ttu-id="a4fff-282">Hozzon létre egy erőforráscsoportot a központi telepítés (egy meglévőt is használhat, de egy újat a legjobb eredmények elérése érdekében javasolt)</span><span class="sxs-lookup"><span data-stu-id="a4fff-282">Create a Resource Group for this deployment (You can use an existing one, but I recommend a new one for best results)</span></span>
6. <span data-ttu-id="a4fff-283">Szükség esetén a virtuális hálózat az egyes előfizetésekhez és helyekhez beállításainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="a4fff-283">If necessary, change the Subscription and Location settings for your VNet.</span></span>
7. <span data-ttu-id="a4fff-284">Kattintson a **tekintse át a jogi feltételeket**, olvassa el a feltételeket, és kattintson a **beszerzési** gombra kattintva elfogadja.</span><span class="sxs-lookup"><span data-stu-id="a4fff-284">Click **Review legal terms**, read the terms, and click **Purchase** to agree.</span></span>
8. <span data-ttu-id="a4fff-285">Kattintson a **létrehozása** a sablon telepítésének megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="a4fff-285">Click **Create** to begin the deployment of this template.</span></span>
9. <span data-ttu-id="a4fff-286">Miután a telepítés sikeresen befejeződött, nyissa meg az erőforráscsoport létrehozásánál ehhez a központi telepítéshez a konfigurált belüli erőforrások megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a4fff-286">Once the deployment finishes successfully, navigate to the Resource Group created for this deployment to see the resources configured inside.</span></span>

>[!NOTE]
><span data-ttu-id="a4fff-287">Ez a sablon lehetővé teszi, hogy csak a IIS01 kiszolgálóra RDP (Keresés a nyilvános IP-cím a IIS01 a portálon).</span><span class="sxs-lookup"><span data-stu-id="a4fff-287">This template enables RDP to the IIS01 server only (find the Public IP for IIS01 on the Portal).</span></span> <span data-ttu-id="a4fff-288">Távoli asztali ezt a példányt a háttér-kiszolgálók az IIS-kiszolgálón használt egy "ugrást mező."</span><span class="sxs-lookup"><span data-stu-id="a4fff-288">To RDP to any back-end servers in this instance, the IIS server is used as a "jump box."</span></span> <span data-ttu-id="a4fff-289">Az IIS-kiszolgálót, majd az IIS kiszolgáló RDP a háttér-kiszolgálóhoz az első RDP.</span><span class="sxs-lookup"><span data-stu-id="a4fff-289">First RDP to the IIS server and then from the IIS Server RDP to the back-end server.</span></span>
>
>

<span data-ttu-id="a4fff-290">Távolítsa el a telepítést, törölje a csoportot, és minden gyermek-erőforrás is törlődik.</span><span class="sxs-lookup"><span data-stu-id="a4fff-290">To remove this deployment, delete the Resource Group and all child resources will also be deleted.</span></span>

#### <a name="sample-application-scripts"></a><span data-ttu-id="a4fff-291">Alkalmazás mintaparancsfájlok</span><span class="sxs-lookup"><span data-stu-id="a4fff-291">Sample application scripts</span></span>
<span data-ttu-id="a4fff-292">A sablon sikeres futtatása után állíthat be a webkiszolgáló és egy egyszerű webes alkalmazás tesztelése a DMZ-konfiguráció engedélyezése az alkalmazások kiszolgálói.</span><span class="sxs-lookup"><span data-stu-id="a4fff-292">Once the template runs successfully, you can set up the web server and app server with a simple web application to allow testing with this DMZ configuration.</span></span> <span data-ttu-id="a4fff-293">Ez, és más DMZ példák mintaalkalmazás telepítéséhez egy adtak meg, az alábbi hivatkozásra: [minta alkalmazás-parancsfájl][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="a4fff-293">To install a sample application for this, and other DMZ Examples, one has been provided at the following link: [Sample Application Script][SampleApp]</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4fff-294">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a4fff-294">Next steps</span></span>

* <span data-ttu-id="a4fff-295">Ez a példa központi telepítése</span><span class="sxs-lookup"><span data-stu-id="a4fff-295">Deploy this example</span></span>
* <span data-ttu-id="a4fff-296">A minta-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a4fff-296">Build the sample application</span></span>
* <span data-ttu-id="a4fff-297">A DMZ keresztül különböző forgalom tesztelése</span><span class="sxs-lookup"><span data-stu-id="a4fff-297">Test different traffic flows through this DMZ</span></span>

<!--Image References-->
<span data-ttu-id="a4fff-298">[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Az NSG bejövő DMZ"</span><span class="sxs-lookup"><span data-stu-id="a4fff-298">[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Inbound DMZ with NSG"</span></span>

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md