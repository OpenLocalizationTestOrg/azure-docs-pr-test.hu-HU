---
title: "Példa – aaaDMZ Build egy Szegélyhálózaton tooProtect hálózatokat és a tűzfalon, UDR és NSG |} Microsoft Docs"
description: "Build tűzfallal DMZ, felhasználó által definiált útválasztási (UDR) és a hálózati biztonsági csoportokkal (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: dc01ccfb-27b0-4887-8f0b-2792f770ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: cc121f9cd5fe3c3e9ac2c70fbb7d982a80bb345d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="example-3--build-a-dmz-tooprotect-networks-with-a-firewall-udr-and-nsg"></a><span data-ttu-id="2524f-103">Build a DMZ tooProtect hálózatokat és a tűzfalon, UDR és NSG 3 – példa</span><span class="sxs-lookup"><span data-stu-id="2524f-103">Example 3 – Build a DMZ tooProtect Networks with a Firewall, UDR, and NSG</span></span>
<span data-ttu-id="2524f-104">[Visszatérési toohello biztonsági határ bevált gyakorlatok lap][HOME]</span><span class="sxs-lookup"><span data-stu-id="2524f-104">[Return toohello Security Boundary Best Practices Page][HOME]</span></span>

<span data-ttu-id="2524f-105">Ebben a példában az tűzfal, négy windows Server kiszolgálókon, felhasználói definiált útválasztó, IP-továbbítást, és hálózati biztonsági csoportok hoz létre a Szegélyhálózaton.</span><span class="sxs-lookup"><span data-stu-id="2524f-105">This example will create a DMZ with a firewall, four windows servers, User Defined Routing, IP Forwarding, and Network Security Groups.</span></span> <span data-ttu-id="2524f-106">Azt is haladhat végig hello vonatkozó parancsok tooprovide bemutatják az egyes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2524f-106">It will also walk through each of hello relevant commands tooprovide a deeper understanding of each step.</span></span> <span data-ttu-id="2524f-107">Szerepel továbbá egy forgalom forgatókönyv szakasz tooprovide egy részletesebb lépésenkénti hogyan forgalom halad keresztül hello szolgálhat a hello DMZ.</span><span class="sxs-lookup"><span data-stu-id="2524f-107">There is also a Traffic Scenario section tooprovide an in-depth step-by-step how traffic proceeds through hello layers of defense in hello DMZ.</span></span> <span data-ttu-id="2524f-108">Végezetül hello references szakaszában van hello teljes kód látható és utasítás toobuild a környezet tootest és a különböző forgatókönyvekben a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="2524f-108">Finally, in hello references section is hello complete code and instruction toobuild this environment tootest and experiment with various scenarios.</span></span> 

<span data-ttu-id="2524f-109">![Kétirányú DMZ NVA, NSG és UDR][1]</span><span class="sxs-lookup"><span data-stu-id="2524f-109">![Bi-directional DMZ with NVA, NSG, and UDR][1]</span></span>

## <a name="environment-setup"></a><span data-ttu-id="2524f-110">Környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="2524f-110">Environment Setup</span></span>
<span data-ttu-id="2524f-111">Ebben a példában nincs olyan előfizetést, amelyet hello következő tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="2524f-111">In this example there is a subscription that contains hello following:</span></span>

* <span data-ttu-id="2524f-112">A felhőalapú szolgáltatások három: "SecSvc001", "FrontEnd001" és "BackEnd001"</span><span class="sxs-lookup"><span data-stu-id="2524f-112">Three cloud services: “SecSvc001”, “FrontEnd001”, and “BackEnd001”</span></span>
* <span data-ttu-id="2524f-113">A virtuális hálózati "CorpNetwork", három alhálózatokon: "SecNet", "Előtér" és "Háttér"</span><span class="sxs-lookup"><span data-stu-id="2524f-113">A Virtual Network “CorpNetwork”, with three subnets: “SecNet”, “FrontEnd”, and “BackEnd”</span></span>
* <span data-ttu-id="2524f-114">Ebben a példában a tűzfal, a hálózati virtuális készülék toohello SecNet alhálózati csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="2524f-114">A network virtual appliance, in this example a firewall, connected toohello SecNet subnet</span></span>
* <span data-ttu-id="2524f-115">A Windows Server egy webalkalmazás-kiszolgáló ("IIS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="2524f-115">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="2524f-116">Két windows Server kiszolgálókon, amelyek megfelelnek az alkalmazás biztonsági end-kiszolgálók ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="2524f-116">Two windows servers that represent application back end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="2524f-117">A Windows server DNS-kiszolgáló ("DNS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="2524f-117">A Windows server that represents a DNS server (“DNS01”)</span></span>

<span data-ttu-id="2524f-118">Hello references szakaszában alatt van egy PowerShell-parancsfájlt, amely a fent leírt hello környezetben a legtöbb fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2524f-118">In hello references section below there is a PowerShell script that will build most of hello environment described above.</span></span> <span data-ttu-id="2524f-119">Épület hello virtuális gépek és virtuális hálózatok, bár hello a példaként megadott parancsfájlt, által végzett dokumentum nem ismerteti a jelen dokumentum részletesen.</span><span class="sxs-lookup"><span data-stu-id="2524f-119">Building hello VMs and Virtual Networks, although are done by hello example script, are not described in detail in this document.</span></span>

<span data-ttu-id="2524f-120">toobuild hello rendelkező környezetben:</span><span class="sxs-lookup"><span data-stu-id="2524f-120">toobuild hello environment:</span></span>

1. <span data-ttu-id="2524f-121">Hello hálózati konfigurációs XML-fájl (nevét, helyét és IP címeket toomatch megadott hello forgatókönyv frissíti) hello references szakaszában szereplő mentése</span><span class="sxs-lookup"><span data-stu-id="2524f-121">Save hello network config xml file included in hello references section (updated with names, location, and IP addresses toomatch hello given scenario)</span></span>
2. <span data-ttu-id="2524f-122">Frissítés hello felhasználói változók hello parancsfájl toomatch hello környezet hello parancsfájlban toobe futtatni (előfizetések, service nevét stb.)</span><span class="sxs-lookup"><span data-stu-id="2524f-122">Update hello user variables in hello script toomatch hello environment hello script is toobe run against (subscriptions, service names, etc)</span></span>
3. <span data-ttu-id="2524f-123">PowerShell hello parancsfájlok végrehajtását</span><span class="sxs-lookup"><span data-stu-id="2524f-123">Execute hello script in PowerShell</span></span>

<span data-ttu-id="2524f-124">**Megjegyzés:**: hello régió hello PowerShell-parancsfájl esetében meg kell egyeznie a hello hálózati konfigurációs XML-fájlban szereplő hello régióban.</span><span class="sxs-lookup"><span data-stu-id="2524f-124">**Note**: hello region signified in hello PowerShell script must match hello region signified in hello network configuration xml file.</span></span>

<span data-ttu-id="2524f-125">Miután hello parancsprogram sikeresen lefut hello utáni parancsfájl lépések tehetők:</span><span class="sxs-lookup"><span data-stu-id="2524f-125">Once hello script runs successfully hello following post-script steps may be taken:</span></span>

1. <span data-ttu-id="2524f-126">Állítson be hello tűzfalszabályokat, ezzel kapcsolatban lásd: hello című az alábbi: tűzfal szabály leírása.</span><span class="sxs-lookup"><span data-stu-id="2524f-126">Set up hello firewall rules, this is covered in hello section below titled: Firewall Rule Description.</span></span>
2. <span data-ttu-id="2524f-127">Opcionálisan hello references szakaszában vannak két parancsfájlok tooset hello webkiszolgáló és egy egyszerű webes alkalmazás tooallow e DMZ konfiguráció tesztelése az alkalmazások kiszolgálói.</span><span class="sxs-lookup"><span data-stu-id="2524f-127">Optionally in hello references section are two scripts tooset up hello web server and app server with a simple web application tooallow testing with this DMZ configuration.</span></span>

<span data-ttu-id="2524f-128">Miután hello parancsprogram sikeresen lefut hello tűzfalszabályokat kell toobe befejeződött, ez hello című kapcsolatban lásd: tűzfalszabályokat.</span><span class="sxs-lookup"><span data-stu-id="2524f-128">Once hello script runs successfully hello firewall rules will need toobe completed, this is covered in hello section titled: Firewall Rules.</span></span>

## <a name="user-defined-routing-udr"></a><span data-ttu-id="2524f-129">Felhasználó által definiált útválasztási (UDR)</span><span class="sxs-lookup"><span data-stu-id="2524f-129">User Defined Routing (UDR)</span></span>
<span data-ttu-id="2524f-130">Alapértelmezés szerint a következő rendszerútvonalak hello is meg van adva:</span><span class="sxs-lookup"><span data-stu-id="2524f-130">By default, hello following system routes are defined as:</span></span>

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

<span data-ttu-id="2524f-131">hello VNETLocal mindig definiálva hello cím előtag(ok) a VNet hello (azaz azt állapotúról attól függően, hogy hogyan definiálva van-e az egyes konkrét VNet hálózatok tooVNet) adott hálózat.</span><span class="sxs-lookup"><span data-stu-id="2524f-131">hello VNETLocal is always hello defined address prefix(es) of hello VNet for that specific network (ie it will change from VNet tooVNet depending on how each specific VNet is defined).</span></span> <span data-ttu-id="2524f-132">hello fennmaradó rendszerútvonalak statikusak, és a fenti alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="2524f-132">hello remaining system routes are static and default as above.</span></span>

<span data-ttu-id="2524f-133">Prioritás, mint útvonalak dolgoznak fel hello a leghosszabb előtag-megfeleltetés (LPM) módszerrel, így hello legjobban megfelelő útvonal hello tábla csak akkor vonatkozik célcím megadott tooa.</span><span class="sxs-lookup"><span data-stu-id="2524f-133">As for priority, routes are processed via hello Longest Prefix Match (LPM) method, thus hello most specific route in hello table would apply tooa given destination address.</span></span>

<span data-ttu-id="2524f-134">Ezért forgalom (például a toohello DNS01 server, 10.0.2.4) szánt hello helyi hálózati (10.0.0.0/16) keresztül hello VNet tooits cél miatt toohello 10.0.0.0/16 útvonal lesz átirányítva.</span><span class="sxs-lookup"><span data-stu-id="2524f-134">Therefore, traffic (for example toohello DNS01 server, 10.0.2.4) intended for hello local network (10.0.0.0/16) would be routed across hello VNet tooits destination due toohello 10.0.0.0/16 route.</span></span> <span data-ttu-id="2524f-135">Más szóval a 10.0.2.4, hello 10.0.0.0/16 útvonal út hello legjobban megfelel, annak ellenére, hogy hello 10.0.0.0/8 és 0.0.0.0/0 is beállíthat, de mivel kevesebb adott nincsenek hatással a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="2524f-135">In other words, for 10.0.2.4, hello 10.0.0.0/16 route is hello most specific route, even though hello 10.0.0.0/8 and 0.0.0.0/0 also could apply, but since they are less specific they don’t affect this traffic.</span></span> <span data-ttu-id="2524f-136">Így hello forgalom too10.0.2.4 lenne a következő ugrás a virtuális helyi hálózat hello és egyszerűen útvonal-toohello cél.</span><span class="sxs-lookup"><span data-stu-id="2524f-136">Thus hello traffic too10.0.2.4 would have a next hop of hello local VNet and simply route toohello destination.</span></span>

<span data-ttu-id="2524f-137">Ha forgalom történő készült 10.1.1.1 például hello 10.0.0.0/16 útvonal nem érvényes, de hello 10.0.0.0/8 lenne hello legjobban megfelel, és hello forgalmat lenne eldobott ("fekete furatos"), mert hello következő ugrás értéke Null.</span><span class="sxs-lookup"><span data-stu-id="2524f-137">If traffic was intended for 10.1.1.1 for example, hello 10.0.0.0/16 route wouldn’t apply, but hello 10.0.0.0/8 would be hello most specific, and this hello traffic would be dropped (“black holed”) since hello next hop is Null.</span></span> 

<span data-ttu-id="2524f-138">Ha hello cél hello Null előtagok vagy hello VNETLocal előtagok tooany nem lett alkalmazva, akkor azt végrehajtania a legkevésbé egyedi hello továbbítani, 0.0.0.0/0 és átirányítás kimenő Internet hello következő ugrásként toohello, így Azure internet peremhálózati ki.</span><span class="sxs-lookup"><span data-stu-id="2524f-138">If hello destination didn’t apply tooany of hello Null prefixes or hello VNETLocal prefixes, then it would follow hello least specific route, 0.0.0.0/0 and be routed out toohello Internet as hello next hop and thus out Azure’s internet edge.</span></span>

<span data-ttu-id="2524f-139">Ha két azonos előtagok szerepelnek hello útválasztási táblázatot, hello az alábbiakban olvashat hello útvonalak "forrás" attribútum alapján hello sorrendben:</span><span class="sxs-lookup"><span data-stu-id="2524f-139">If there are two identical prefixes in hello route table, hello following is hello order of preference based on hello routes “source” attribute:</span></span>

1. <span data-ttu-id="2524f-140">"VirtualAppliance" egy felhasználó által definiált útvonaltábla manuálisan hozzáadott toohello =</span><span class="sxs-lookup"><span data-stu-id="2524f-140">"VirtualAppliance" = A User Defined Route manually added toohello table</span></span>
2. <span data-ttu-id="2524f-141">"A(z)" (BGP hibrid hálózatok használata esetén), dinamikus útvonal = dinamikus hálózati protokoll által hozzáadott, ezeket az útvonalakat idővel megváltozhat, hello dinamikus protokoll automatikusan módosításoknak társviszonyban hálózat</span><span class="sxs-lookup"><span data-stu-id="2524f-141">“VPNGateway” = A Dynamic Route (BGP when used with hybrid networks), added by a dynamic network protocol, these routes may change over time as hello dynamic protocol automatically reflects changes in peered network</span></span>
3. <span data-ttu-id="2524f-142">"Alapértelmezett" = hello Rendszerútvonalak, helyi hálózatok és hello statikus bejegyzések hello hello útválasztási táblázatot a fenti ábrán.</span><span class="sxs-lookup"><span data-stu-id="2524f-142">“Default” = hello System Routes, hello local VNet and hello static entries as shown in hello route table above.</span></span>

> [!NOTE]
> <span data-ttu-id="2524f-143">Ezután már használhatja felhasználói definiált útválasztás (UDR) az ExpressRoute- és VPN-átjárók tooforce kimenő és bejövő létesítmények közötti forgalom toobe irányított tooa hálózati virtuális készülék (NVA).</span><span class="sxs-lookup"><span data-stu-id="2524f-143">You can now use User Defined Routing (UDR) with ExpressRoute and VPN Gateways tooforce outbound and inbound cross-premises traffic toobe routed tooa network virtual appliance (NVA).</span></span>
> 
> 

#### <a name="creating-hello-local-routes"></a><span data-ttu-id="2524f-144">Hello helyi útvonalak létrehozása</span><span class="sxs-lookup"><span data-stu-id="2524f-144">Creating hello local routes</span></span>
<span data-ttu-id="2524f-145">Ebben a példában két útvonaltáblát van szükség, egy minden hello előtér- és háttérszolgáltatások alhálózatok esetében.</span><span class="sxs-lookup"><span data-stu-id="2524f-145">In this example, two routing tables are needed, one each for hello Frontend and Backend subnets.</span></span> <span data-ttu-id="2524f-146">Minden tábla megfelelő a megadott alhálózati hello statikus útvonalakkal be van töltve.</span><span class="sxs-lookup"><span data-stu-id="2524f-146">Each table is loaded with static routes appropriate for hello given subnet.</span></span> <span data-ttu-id="2524f-147">Ebben a példában a hello célból minden táblának három útvonalakat:</span><span class="sxs-lookup"><span data-stu-id="2524f-147">For hello purpose of this example, each table has three routes:</span></span>

1. <span data-ttu-id="2524f-148">A következő ugrás a helyi alhálózat forgalmának definiált tooallow helyi alhálózati forgalom toobypass hello tűzfal</span><span class="sxs-lookup"><span data-stu-id="2524f-148">Local subnet traffic with no Next Hop defined tooallow local subnet traffic toobypass hello firewall</span></span>
2. <span data-ttu-id="2524f-149">Virtuális hálózati forgalmat a következő ugrásaként definiálva, tűzfal, hello alapértelmezett szabály, amely lehetővé teszi a helyi VNet forgalmát tooroute közvetlenül a rendszer felülírja</span><span class="sxs-lookup"><span data-stu-id="2524f-149">Virtual Network traffic with a Next Hop defined as firewall, this overrides hello default rule that allows local VNet traffic tooroute directly</span></span>
3. <span data-ttu-id="2524f-150">Az összes többi forgalom (0/0) a következő ugrásaként megadva, a tűzfal hello</span><span class="sxs-lookup"><span data-stu-id="2524f-150">All remaining traffic (0/0) with a Next Hop defined as hello firewall</span></span>

<span data-ttu-id="2524f-151">Ha létrehozta a hello útválasztási táblázataiba kötött tootheir alhálózatok.</span><span class="sxs-lookup"><span data-stu-id="2524f-151">Once hello routing tables are created they are bound tootheir subnets.</span></span> <span data-ttu-id="2524f-152">Hello előtér alhálózati útválasztási táblázat Miután létrehozott és a kötött toohello alhálózati kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="2524f-152">For hello Frontend subnet routing table, once created and bound toohello subnet should look like this:</span></span>

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


<span data-ttu-id="2524f-153">Ehhez a példához hello következő parancsok használt toobuild hello útvonaltábla, adjon hozzá egy felhasználó által megadott útvonalat, és majd kötési hello útvonal tábla tooa alhálózati (Megjegyzés; dollárjelet kezdődő alatti elemek (pl.: $BESubnet) hello parancsfájlból a felhasználó által definiált változók hello útmutató szakaszban a jelen dokumentum):</span><span class="sxs-lookup"><span data-stu-id="2524f-153">For this example, hello following commands are used toobuild hello route table, add a user defined route, and then bind hello route table tooa subnet (Note; any items below beginning with a dollar sign (e.g.: $BESubnet) are user defined variables from hello script in hello reference section of this document):</span></span>

1. <span data-ttu-id="2524f-154">Első hello útválasztási alaptábla kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2524f-154">First hello base routing table must be created.</span></span> <span data-ttu-id="2524f-155">A kódrészletben láthatja a hello Backend alhálózathoz hello tábla hello létrehozását.</span><span class="sxs-lookup"><span data-stu-id="2524f-155">This snippet shows hello creation of hello table for hello Backend subnet.</span></span> <span data-ttu-id="2524f-156">Hello parancsfájlban egy megfelelő táblázatot is létrejön a hello Frontend alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="2524f-156">In hello script, a corresponding table is also created for hello Frontend subnet.</span></span>
   
     <span data-ttu-id="2524f-157">Új AzureRouteTable-$BERouteTableName neve "</span><span class="sxs-lookup"><span data-stu-id="2524f-157">New-AzureRouteTable -Name $BERouteTableName \`</span></span>
   
         -Location $DeploymentLocation `
         -Label "Route table for $BESubnet subnet"
2. <span data-ttu-id="2524f-158">Hello útvonaltábla létrehozása után a megadott felhasználó által megadott útvonalakat is hozzáadhatók.</span><span class="sxs-lookup"><span data-stu-id="2524f-158">Once hello route table is created, specific user defined routes can be added.</span></span> <span data-ttu-id="2524f-159">Ezen parancsmaghoz minden forgalmat (0.0.0.0/0) hello virtuális készülék (egy változót, $VMIP [0], az a hello IP-cím hello virtuális készülék hello parancsfájl a korábbi létrehozásakor használt toopass) keresztül továbbítódik.</span><span class="sxs-lookup"><span data-stu-id="2524f-159">In this snipped, all traffic (0.0.0.0/0) will be routed through hello virtual appliance (a variable, $VMIP[0], is used toopass in hello IP address assigned when hello virtual appliance was created earlier in hello script).</span></span> <span data-ttu-id="2524f-160">Hello parancsfájl, a megfelelő létrejön egy szabály is hello előtér táblában.</span><span class="sxs-lookup"><span data-stu-id="2524f-160">In hello script, a corresponding rule is also created in hello Frontend table.</span></span>
   
     <span data-ttu-id="2524f-161">Get-AzureRouteTable $BERouteTableName |} \`</span><span class="sxs-lookup"><span data-stu-id="2524f-161">Get-AzureRouteTable $BERouteTableName | \`</span></span>
   
         Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
         -NextHopType VirtualAppliance `
         -NextHopIpAddress $VMIP[0]
3. <span data-ttu-id="2524f-162">hello fent útvonal bejegyzés felülírják hello alapértelmezett "0.0.0.0/0" útvonal, de továbbra is meglévő, amelyek lehetővé teszik a hello alapértelmezett 10.0.0.0/16 szabály forgalom belül hello VNet tooroute közvetlenül toohello a cél és a nem hálózati virtuális készülék toohello.</span><span class="sxs-lookup"><span data-stu-id="2524f-162">hello above route entry will override hello default "0.0.0.0/0" route, but hello default 10.0.0.0/16 rule still existing which would allow traffic within hello VNet tooroute directly toohello destination and not toohello Network Virtual Appliance.</span></span> <span data-ttu-id="2524f-163">Ez a viselkedés hello kövesse szabály hozzá kell adni toocorrect.</span><span class="sxs-lookup"><span data-stu-id="2524f-163">toocorrect this behavior hello follow rule must be added.</span></span>
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
4. <span data-ttu-id="2524f-164">Ezen a ponton nincs a választott toobe végzett.</span><span class="sxs-lookup"><span data-stu-id="2524f-164">At this point there is a choice toobe made.</span></span> <span data-ttu-id="2524f-165">A fenti két útvonalak hello minden forgalom átirányítja az toohello tűzfal értékelésére, még akkor is, forgalmat egy egyetlen alhálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="2524f-165">With hello above two routes all traffic will route toohello firewall for assessment, even traffic within a single subnet.</span></span> <span data-ttu-id="2524f-166">Ez kívánatos lehet, de tooallow forgalom belül egy alhálózat tooroute helyileg egy harmadik, olyan speciális szabályt is hozzáadhatók hello tűzfaltól használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="2524f-166">This may be desired, however tooallow traffic within a subnet tooroute locally without involvement from hello firewall a third, very specific rule can be added.</span></span> <span data-ttu-id="2524f-167">Ez az útvonal bármely cím destine hello helyi alhálózati is csak az állapotok közvetlenül útvonal létezik (NextHopType = VNETLocal).</span><span class="sxs-lookup"><span data-stu-id="2524f-167">This route states that any address destine for hello local subnet can just route there directly (NextHopType = VNETLocal).</span></span>
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
5. <span data-ttu-id="2524f-168">Végezetül hello útvonaltábla létrehozása, és feltölti a felhasználó által megadott útvonalakat, hello tábla most kell a kötött tooa alhálózat.</span><span class="sxs-lookup"><span data-stu-id="2524f-168">Finally, with hello routing table created and populated with a user defined routes, hello table must now be bound tooa subnet.</span></span> <span data-ttu-id="2524f-169">Hello parancsfájlban hello előtér útvonaltábla egyben kötött toohello Frontend alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="2524f-169">In hello script, hello front end route table is also bound toohello Frontend subnet.</span></span> <span data-ttu-id="2524f-170">Itt található hello kötés parancsfájl hello háttér-alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="2524f-170">Here is hello binding script for hello back end subnet.</span></span>
   
     <span data-ttu-id="2524f-171">Set-AzureSubnetRouteTable - VirtualNetworkName $VNetName "</span><span class="sxs-lookup"><span data-stu-id="2524f-171">Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName \`</span></span>
   
        -SubnetName $BESubnet `
        -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a><span data-ttu-id="2524f-172">IP-továbbítás</span><span class="sxs-lookup"><span data-stu-id="2524f-172">IP Forwarding</span></span>
<span data-ttu-id="2524f-173">A kiegészítő funkció tooUDR, az IP-továbbítást.</span><span class="sxs-lookup"><span data-stu-id="2524f-173">A companion feature tooUDR, is IP Forwarding.</span></span> <span data-ttu-id="2524f-174">Ez egy beállítást a virtuális készülék, amely lehetővé teszi nem kifejezetten tooreceive forgalom címzett toohello készüléknek, majd továbbítja az adott forgalom tooits végső célja.</span><span class="sxs-lookup"><span data-stu-id="2524f-174">This is a setting on a Virtual Appliance that allows it tooreceive traffic not specifically addressed toohello appliance and then forward that traffic tooits ultimate destination.</span></span>

<span data-ttu-id="2524f-175">Tegyük fel AppVM01 forgalmát kiszolgálóvá egy kérelem toohello DNS01, ha UDR szeretné továbbítani a toohello tűzfal.</span><span class="sxs-lookup"><span data-stu-id="2524f-175">As an example, if traffic from AppVM01 makes a request toohello DNS01 server, UDR would route this toohello firewall.</span></span> <span data-ttu-id="2524f-176">Az IP-továbbítás engedélyezve van hello forgalom hello DNS01 cél (10.0.2.4) a rendszer elfogadta hello készülék (10.0.0.4), majd továbbítja a végső rendeltetési tooits (10.0.2.4).</span><span class="sxs-lookup"><span data-stu-id="2524f-176">With IP Forwarding enabled, hello traffic for hello DNS01 destination (10.0.2.4) will be accepted by hello appliance (10.0.0.4) and then forwarded tooits ultimate destination (10.0.2.4).</span></span> <span data-ttu-id="2524f-177">Anélkül, hogy az IP-továbbítás engedélyezve a tűzfal hello forgalom volna nem fogadja el hello készülék annak ellenére, hogy hello útvonaltábla legyen hello tűzfal hello következő ugrásként.</span><span class="sxs-lookup"><span data-stu-id="2524f-177">Without IP Forwarding enabled on hello Firewall, traffic would not be accepted by hello appliance even though hello route table has hello firewall as hello next hop.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="2524f-178">Kritikus tooremember tooenable IP-továbbítás felhasználó definiált útválasztási együtt.</span><span class="sxs-lookup"><span data-stu-id="2524f-178">It’s critical tooremember tooenable IP Forwarding in conjunction with User Defined Routing.</span></span>
> 
> 

<span data-ttu-id="2524f-179">Az IP-továbbítás beállítása egy parancs, és a virtuális gép létrehozás időpontjában végezhető.</span><span class="sxs-lookup"><span data-stu-id="2524f-179">Setting up IP Forwarding is a single command and can be done at VM creation time.</span></span> <span data-ttu-id="2524f-180">Hello folyamata a következő példa hello kódrészletet hello parancsfájl hello vége felé, és hello UDR parancsok csoportosítva:</span><span class="sxs-lookup"><span data-stu-id="2524f-180">For hello flow of this example hello code snippet is towards hello end of hello script and grouped with hello UDR commands:</span></span>

1. <span data-ttu-id="2524f-181">Hívás, amely a virtuális készülékre hello Virtuálisgép-példány, hello ebben az esetben a tűzfal és az IP-továbbítás engedélyezése (Megjegyzés; valamely elemére dollárjelet piros kezdődő (pl.: $VMName[0]) egy felhasználó által definiált változó hello parancsfájlból hello hivatkozás szakaszban ebben a dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="2524f-181">Call hello VM instance that is your virtual appliance, hello firewall in this case, and enable IP Forwarding (Note; any item in red beginning with a dollar sign (e.g.: $VMName[0]) is a user defined variable from hello script in hello reference section of this document.</span></span> <span data-ttu-id="2524f-182">hello nulla szögletes [0], jelöli hello hello tömb hello példa parancsfájl toowork módosítás nélkül a virtuális gépek, az első virtuális gép, első virtuális gép (VM 0) kell lennie hello hello tűzfal):</span><span class="sxs-lookup"><span data-stu-id="2524f-182">hello zero in brackets, [0], represents hello first VM in hello array of VMs, for hello example script toowork without modification, hello first VM (VM 0) must be hello firewall):</span></span>
   
     <span data-ttu-id="2524f-183">Get-AzureVM-Name $VMName [0] - szolgáltatásnév $ServiceName [0] |} \`</span><span class="sxs-lookup"><span data-stu-id="2524f-183">Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | \`</span></span>
   
        Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a><span data-ttu-id="2524f-184">Hálózati biztonsági csoportok (NSG)</span><span class="sxs-lookup"><span data-stu-id="2524f-184">Network Security Groups (NSG)</span></span>
<span data-ttu-id="2524f-185">Ebben a példában egy NSG-csoport összeállítása és kezelhető egyetlen szabállyal majd betölteni.</span><span class="sxs-lookup"><span data-stu-id="2524f-185">In this example, a NSG group is built and then loaded with a single rule.</span></span> <span data-ttu-id="2524f-186">Ez a csoport van, majd kötött csak toohello előtér- és háttérszolgáltatások alhálózatok (nem a SecNet hello).</span><span class="sxs-lookup"><span data-stu-id="2524f-186">This group is then bound only toohello Frontend and Backend subnets (not hello SecNet).</span></span> <span data-ttu-id="2524f-187">Deklaratív módon a következő szabály hello éppen készül:</span><span class="sxs-lookup"><span data-stu-id="2524f-187">Declaratively hello following rule is being built:</span></span>

1. <span data-ttu-id="2524f-188">Teljes virtuális hálózatot (az összes alhálózatot) megtagadva hello Internet toohello minden forgalmat (minden porthoz)</span><span class="sxs-lookup"><span data-stu-id="2524f-188">Any traffic (all ports) from hello Internet toohello entire VNet (all subnets) is Denied</span></span>

<span data-ttu-id="2524f-189">Bár ebben a példában NSG-ket használ, annak fő célja azoknak a manuális helytelen konfiguráció másodlagos rétegként.</span><span class="sxs-lookup"><span data-stu-id="2524f-189">Although NSGs are used in this example, it’s main purpose is as a secondary layer of defense against manual misconfiguration.</span></span> <span data-ttu-id="2524f-190">Azt szeretnénk, tooblock hello internet tooeither hello előtér minden bejövő forgalom vagy háttér alhálózatok felderítéséhez, a forgalom csak áramlása hello SecNet alhálózati toohello tűzfal (és toohello előtér- vagy háttéradatbázis alhálózatok majd ha szükséges).</span><span class="sxs-lookup"><span data-stu-id="2524f-190">We want tooblock all inbound traffic from hello internet tooeither hello Frontend or Backend subnets, traffic should only flow through hello SecNet subnet toohello firewall (and then if appropriate on toohello Frontend or Backend subnets).</span></span> <span data-ttu-id="2524f-191">Plus hello UDR szabályokkal helyen, minden forgalom, amelyek adott hello az előtér- vagy háttéradatbázis alhálózatok volna átirányítja kimenő toohello tűzfal (Köszönjük tooUDR).</span><span class="sxs-lookup"><span data-stu-id="2524f-191">Plus, with hello UDR rules in place, any traffic that did make it into hello Frontend or Backend subnets would be directed out toohello firewall (thanks tooUDR).</span></span> <span data-ttu-id="2524f-192">hello tűzfal jelenik meg ez az aszimmetrikus folyamatként és hello kimenő forgalom volna eldobni.</span><span class="sxs-lookup"><span data-stu-id="2524f-192">hello firewall would see this as an asymmetric flow and would drop hello outbound traffic.</span></span> <span data-ttu-id="2524f-193">Így nincsenek biztonsági védelmet nyújtó hello előtér három réteg és a háttérkiszolgáló alhálózatok; 1.) a forgalom nincs nyitott végpontok hello FrontEnd001 és BackEnd001 felhőszolgáltatások, 2) az NSG-k megtagadása hello Internet, 3) hello tűzfal aszimmetrikus forgalom eldobását.</span><span class="sxs-lookup"><span data-stu-id="2524f-193">Thus there are three layers of security protecting hello Frontend and Backend subnets; 1) no open endpoints on hello FrontEnd001 and BackEnd001 cloud services, 2) NSGs denying traffic from hello Internet, 3) hello firewall dropping asymmetric traffic.</span></span>

<span data-ttu-id="2524f-194">Ebben a példában a hálózati biztonsági csoport hello vonatkozó egy érdekes pont, hogy az tartalmazza csak egy szabály, az alábbi ábrán látható, amely toodeny internetes forgalom toohello teljes virtuális hálózat, amely magában foglalja a hello biztonság – alhálózati.</span><span class="sxs-lookup"><span data-stu-id="2524f-194">One interesting point regarding hello Network Security Group in this example is that it contains only one rule, shown below, which is toodeny internet traffic toohello entire virtual network which would include hello Security subnet.</span></span> 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet `
        from hello Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

<span data-ttu-id="2524f-195">Azonban mivel hello NSG csak van kötve, toohello előtér- és háttér alhálózatok, hello szabály nem feldolgozott forgalom toohello biztonság – alhálózati bejövő.</span><span class="sxs-lookup"><span data-stu-id="2524f-195">However, since hello NSG is only bound toohello Frontend and Backend subnets, hello rule isn’t processed on traffic inbound toohello Security subnet.</span></span> <span data-ttu-id="2524f-196">Ennek eredményeképpen annak ellenére, hogy hello NSG-szabály nem internetes forgalom tooany címet szereplő ellenőrzőösszeg hello virtuális hálózatot, mert hello NSG soha nem volt kötve toohello biztonság – alhálózati a forgalom halad toohello biztonság – alhálózati.</span><span class="sxs-lookup"><span data-stu-id="2524f-196">As a result, even though hello NSG rule says no Internet traffic tooany address on hello VNet, because hello NSG was never bound toohello Security subnet, traffic will flow toohello Security subnet.</span></span>

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a><span data-ttu-id="2524f-197">Tűzfalszabályok</span><span class="sxs-lookup"><span data-stu-id="2524f-197">Firewall Rules</span></span>
<span data-ttu-id="2524f-198">Az hello tűzfal szabályok továbbítás kell toobe létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2524f-198">On hello firewall, forwarding rules will need toobe created.</span></span> <span data-ttu-id="2524f-199">Mivel a hello tűzfal blokkolja vagy az összes bejövő, kimenő továbbítási és intra-VNet forgalom sok tűzfalszabályok van szükség.</span><span class="sxs-lookup"><span data-stu-id="2524f-199">Since hello firewall is blocking or forwarding all inbound, outbound, and intra-VNet traffic many firewall rules are needed.</span></span> <span data-ttu-id="2524f-200">Emellett minden bejövő forgalom elért hello biztonsági szolgáltatás nyilvános IP-cím (különböző portok), toobe feldolgozva hello tűzfal.</span><span class="sxs-lookup"><span data-stu-id="2524f-200">Also, all inbound traffic will hit hello Security Service public IP address (on different ports), toobe processed by hello firewall.</span></span> <span data-ttu-id="2524f-201">Ajánlott toodiagram hello logikai adatfolyamok hello alhálózatok és a tűzfalon szabályok tooavoid átdolgozás később beállítása előtt.</span><span class="sxs-lookup"><span data-stu-id="2524f-201">A best practice is toodiagram hello logical flows before setting up hello subnets and firewall rules tooavoid rework later.</span></span> <span data-ttu-id="2524f-202">hello alábbi ábrán is látható az ebben a példában hello tűzfalszabályok logikai nézetében:</span><span class="sxs-lookup"><span data-stu-id="2524f-202">hello following figure is a logical view of hello firewall rules for this example:</span></span>

<span data-ttu-id="2524f-203">![Tűzfalszabályok hello logikai ábrázolása][2]</span><span class="sxs-lookup"><span data-stu-id="2524f-203">![Logical View of hello Firewall Rules][2]</span></span>

> [!NOTE]
> <span data-ttu-id="2524f-204">A virtuális hálózati berendezések használt hello alapján, hello kezelőportjai függ.</span><span class="sxs-lookup"><span data-stu-id="2524f-204">Based on hello Network Virtual Appliance used, hello management ports will vary.</span></span> <span data-ttu-id="2524f-205">Ebben a példában a Barracuda NextGen tűzfal hivatkozott használó 22, 801 és 807 portok.</span><span class="sxs-lookup"><span data-stu-id="2524f-205">In this example a Barracuda NextGen Firewall is referenced which uses ports 22, 801, and 807.</span></span> <span data-ttu-id="2524f-206">További részleteket hello készülék gyártói dokumentációjában toofind hello pontos használt portok használt hello eszköz kezelését.</span><span class="sxs-lookup"><span data-stu-id="2524f-206">Please consult hello appliance vendor documentation toofind hello exact ports used for management of hello device being used.</span></span>
> 
> 

### <a name="logical-rule-description"></a><span data-ttu-id="2524f-207">Logikai szabály leírása</span><span class="sxs-lookup"><span data-stu-id="2524f-207">Logical Rule Description</span></span>
<span data-ttu-id="2524f-208">Hello logikai diagramon fenti hello biztonság – alhálózati nem látható, mivel hello tűzfal hello csak erőforrás adott alhálózaton, és ezen a diagramon látható hello tűzfal-szabályok és hogyan azok logikailag engedélyezi vagy megtagadja a forgalmat, és nem hello tényleges irányított elérési.</span><span class="sxs-lookup"><span data-stu-id="2524f-208">In hello logical diagram above, hello security subnet is not shown since hello firewall is hello only resource on that subnet, and this diagram is showing hello firewall rules and how they logically allow or deny traffic flows and not hello actual routed path.</span></span> <span data-ttu-id="2524f-209">Is, hello külső portok kiválasztott hello RDP-forgalmát magasabb címkiosztási portok (8014 – 8026), és volt kijelölt toosomewhat igazodni hello utolsó két oktetben hello helyi IP-cím könnyebb olvashatóság érdekében (pl. helyi kiszolgáló címe 10.0.1.4 társítva van külső port 8014), azonban bármely magasabb nem ütköző portok is használható.</span><span class="sxs-lookup"><span data-stu-id="2524f-209">Also, hello external ports selected for hello RDP traffic are higher ranged ports (8014 – 8026) and were selected toosomewhat align with hello last two octets of hello local IP address for easier readability (e.g. local server address 10.0.1.4 is associated with external port 8014), however any higher non-conflicting ports could be used.</span></span>

<span data-ttu-id="2524f-210">Az ebben a példában 7 típusú szabályokat kell, ezek a szabályok típusai a következők ismertetett:</span><span class="sxs-lookup"><span data-stu-id="2524f-210">For this example, we need 7 types of rules, these rule types are described as follows:</span></span>

* <span data-ttu-id="2524f-211">Külső szabályainak (bejövő forgalom):</span><span class="sxs-lookup"><span data-stu-id="2524f-211">External Rules (for inbound traffic):</span></span>
  1. <span data-ttu-id="2524f-212">Felügyeleti tűzfalszabály: Az alkalmazás átirányítási szabály lehetővé teszi a forgalom toopass toohello kezelőportjai hello hálózati virtuális készülék.</span><span class="sxs-lookup"><span data-stu-id="2524f-212">Firewall Management Rule: This App Redirect rule allows traffic toopass toohello management ports of hello network virtual appliance.</span></span>
  2. <span data-ttu-id="2524f-213">RDP-szabályok (az egyes windows-kiszolgálók): A négy szabályok (kiszolgálónként egyet) lehetővé teszi hello kezelését az egyes kiszolgálók RDP-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="2524f-213">RDP Rules (for each windows server): These four rules (one for each server) will allow management of hello individual servers via RDP.</span></span> <span data-ttu-id="2524f-214">Ezt követően sikerült kell is kötegelt, attól függően, hogy hello hálózati használt virtuális készülék képességeinek hello egy szabályt a.</span><span class="sxs-lookup"><span data-stu-id="2524f-214">This could also be bundled into one rule depending on hello capabilities of hello Network Virtual Appliance being used.</span></span>
  3. <span data-ttu-id="2524f-215">Alkalmazás-forgalomra vonatkozó szabályok: Nincsenek két alkalmazás forgalomra vonatkozó szabályok, először a hello előtérbeli webes forgalomban hello és hello második adatforgalomhoz hello háttér (pl. web server toodata réteg).</span><span class="sxs-lookup"><span data-stu-id="2524f-215">Application Traffic Rules: There are two Application Traffic Rules, hello first for hello front end web traffic, and hello second for hello back end traffic (eg web server toodata tier).</span></span> <span data-ttu-id="2524f-216">Ezek a szabályok hello konfigurálása hello hálózati architektúra (ahol a kiszolgálók kerülnek) és a forgalom (melyik irányba hello forgalmat, és mely portokat kell használni) függ.</span><span class="sxs-lookup"><span data-stu-id="2524f-216">hello configuration of these rules will depend on hello network architecture (where your servers are placed) and traffic flows (which direction hello traffic flows, and which ports are used).</span></span>
     * <span data-ttu-id="2524f-217">hello első szabály lehetővé teszi a hello tényleges alkalmazás forgalom tooreach hello alkalmazáskiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="2524f-217">hello first rule will allow hello actual application traffic tooreach hello application server.</span></span> <span data-ttu-id="2524f-218">Hello más szabályok engedélyezik a biztonsági, felügyeleti stb, amíg az alkalmazás szabályok lettek, mi engedélyezése külső felhasználók vagy szolgáltatások tooaccess hello alkalmazás(ok).</span><span class="sxs-lookup"><span data-stu-id="2524f-218">While hello other rules allow for security, management, etc., Application Rules are what allow external users or services tooaccess hello application(s).</span></span> <span data-ttu-id="2524f-219">Ebben a példában egyetlen webkiszolgálón való üzemeltetésekor 80-as porton van, így egyetlen alkalmazás tűzfalszabály átirányítja a bejövő forgalom toohello külső IP-toohello webes kiszolgálók belső IP-címet.</span><span class="sxs-lookup"><span data-stu-id="2524f-219">For this example, there is a single web server on port 80, thus a single firewall application rule will redirect inbound traffic toohello external IP, toohello web servers internal IP address.</span></span> <span data-ttu-id="2524f-220">hello átirányítva forgalom munkamenet NAT szeretné venni toohello belső kiszolgálóhiba.</span><span class="sxs-lookup"><span data-stu-id="2524f-220">hello redirected traffic session would be NAT’d toohello internal server.</span></span>
     * <span data-ttu-id="2524f-221">második alkalmazás forgalmi szabály hello hello háttér szabály tooallow hello webkiszolgáló tootalk toohello AppVM01 kiszolgáló (de nem AppVM02) bármely porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="2524f-221">hello second Application Traffic Rule is hello back end rule tooallow hello Web Server tootalk toohello AppVM01 server (but not AppVM02) via any port.</span></span>
* <span data-ttu-id="2524f-222">Belső szabályainak (intra-VNet-forgalom)</span><span class="sxs-lookup"><span data-stu-id="2524f-222">Internal Rules (for intra-VNet traffic)</span></span>
  1. <span data-ttu-id="2524f-223">Kimenő tooInternet szabály: Ez a szabály lehetővé teszi a hálózati forgalom toopass kijelölt toohello hálózatok.</span><span class="sxs-lookup"><span data-stu-id="2524f-223">Outbound tooInternet Rule: This rule will allow traffic from any network toopass toohello selected networks.</span></span> <span data-ttu-id="2524f-224">Ez a szabály az általában egy alapértelmezett szabály már a hello tűzfalat, de a letiltott állapotú.</span><span class="sxs-lookup"><span data-stu-id="2524f-224">This rule is usually a default rule already on hello firewall, but in a disabled state.</span></span> <span data-ttu-id="2524f-225">Ez a szabály ehhez a példához engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="2524f-225">This rule should be enabled for this example.</span></span>
  2. <span data-ttu-id="2524f-226">DNS-szabály: Ez a szabály lehetővé teszi, hogy csak a DNS (53-as port) forgalmat toopass toohello DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="2524f-226">DNS Rule: This rule allows only DNS (port 53) traffic toopass toohello DNS server.</span></span> <span data-ttu-id="2524f-227">Az ebben a környezetben a legtöbb forgalmat hello előtér toohello háttér le van tiltva ez a szabály kifejezetten engedélyezi DNS a helyi alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="2524f-227">For this environment most traffic from hello Frontend toohello Backend is blocked, this rule specifically allows DNS from any local subnet.</span></span>
  3. <span data-ttu-id="2524f-228">Alhálózati tooSubnet szabály: Ez a szabály tooallow bármely kiszolgáló hello vissza a záró alhálózat tooconnect tooany server hello előtér alhálózaton (de nem a fordított hello) szerepel.</span><span class="sxs-lookup"><span data-stu-id="2524f-228">Subnet tooSubnet Rule: This rule is tooallow any server on hello back end subnet tooconnect tooany server on hello front end subnet (but not hello reverse).</span></span>
* <span data-ttu-id="2524f-229">Hibamentes (forgalomra vonatkozó szabály, amely nem felel meg a fenti hello bármely):</span><span class="sxs-lookup"><span data-stu-id="2524f-229">Fail-safe Rule (for traffic that doesn’t meet any of hello above):</span></span>
  1. <span data-ttu-id="2524f-230">Minden forgalmi szabály megtagadása: Ez mindig legyen hello végső szabály (tekintetében prioritás), és használja, így ha a forgalom sikertelen lesz toomatch bármelyik hello megelőző szabályokat a rendszer eldobja a szabály által.</span><span class="sxs-lookup"><span data-stu-id="2524f-230">Deny All Traffic Rule: This should always be hello final rule (in terms of priority), and as such if a traffic flows fails toomatch any of hello preceding rules it will be dropped by this rule.</span></span> <span data-ttu-id="2524f-231">Ez az alapértelmezett szabály, és általában aktív, módosítás nélkül általában szükség van.</span><span class="sxs-lookup"><span data-stu-id="2524f-231">This is a default rule and usually activated, no modifications are generally needed.</span></span>

> [!TIP]
> <span data-ttu-id="2524f-232">Hello második alkalmazás forgalmi szabályt minden port engedélyezett könnyen ebben a példában, egy valós forgatókönyv hello legjobban megfelelő portok és címtartományok használt tooreduce hello támadási felületét Ez a szabály kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2524f-232">On hello second application traffic rule, any port is allowed for easy of this example, in a real scenario hello most specific port and address ranges should be used tooreduce hello attack surface of this rule.</span></span>
> 
> 

<br />

> [!IMPORTANT]
> <span data-ttu-id="2524f-233">Miután az összes fenti szabályok hello jönnek létre, fontos, minden egyes szabály tooensure forgalom tooreview hello rangsorának engedélyezett vagy a kívánt módon megtagadva.</span><span class="sxs-lookup"><span data-stu-id="2524f-233">Once all of hello above rules are created, it’s important tooreview hello priority of each rule tooensure traffic will be allowed or denied as desired.</span></span> <span data-ttu-id="2524f-234">Ehhez a példához hello szabályok prioritási sorrendben szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="2524f-234">For this example, hello rules are in priority order.</span></span> <span data-ttu-id="2524f-235">Egyszerű toobe hello tűzfal toomis rendelt szabályok miatt tévedéssel is.</span><span class="sxs-lookup"><span data-stu-id="2524f-235">It's easy toobe locked out of hello firewall due toomis-ordered rules.</span></span> <span data-ttu-id="2524f-236">Minimális győződjön meg arról hello felügyeleti hello tűzfal maga mindig hello abszolút legmagasabb prioritású szabály.</span><span class="sxs-lookup"><span data-stu-id="2524f-236">At a minimum, ensure hello management for hello firewall itself is always hello absolute highest priority rule.</span></span>
> 
> 

### <a name="rule-prerequisites"></a><span data-ttu-id="2524f-237">A szabály Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2524f-237">Rule Prerequisites</span></span>
<span data-ttu-id="2524f-238">Valamelyik előfeltétel hello virtuális gép futó hello tűzfal olyan nyilvános végpontok.</span><span class="sxs-lookup"><span data-stu-id="2524f-238">One prerequisite for hello Virtual Machine running hello firewall are public endpoints.</span></span> <span data-ttu-id="2524f-239">Hello tűzfal tooprocess forgalom hello megfelelő nyilvános végpontok nyitva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2524f-239">For hello firewall tooprocess traffic hello appropriate public endpoints must be open.</span></span> <span data-ttu-id="2524f-240">Három típusú forgalom ebben a példában; 1.) felügyeleti forgalom toocontrol hello tűzfal és tűzfalszabályok, 2) RDP forgalom toocontrol hello windows-kiszolgálók és a 3.) alkalmazás-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="2524f-240">There are three types of traffic in this example; 1) Management traffic toocontrol hello firewall and firewall rules, 2) RDP traffic toocontrol hello windows servers, and 3) Application Traffic.</span></span> <span data-ttu-id="2524f-241">Ezek a hello három oszlopot a felső hello a forgalomtípusok fele hello tűzfalszabályok fent logikai nézetében.</span><span class="sxs-lookup"><span data-stu-id="2524f-241">These are hello three columns of traffic types in hello upper half of logical view of hello firewall rules above.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2524f-242">A kulcs takeway tooremember, amely **minden** forgalmat fog érkezni hello tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="2524f-242">A key takeway here is tooremember that **all** traffic will come through hello firewall.</span></span> <span data-ttu-id="2524f-243">Igen tooremote asztali toohello IIS01 kiszolgáló, akkor is, ha hello első End felhőalapú szolgáltatás, valamint hello előtér alhálózati tooaccess ehhez a kiszolgálóhoz a tooRDP toohello tűzfal fel kell port 8014, és majd belső az hello tűzfal tooroute hello RDP-kérés engedélyezése toohello IIS01 RDP-portjára.</span><span class="sxs-lookup"><span data-stu-id="2524f-243">So tooremote desktop toohello IIS01 server, even though it's in hello Front End Cloud Service and on hello Front End subnet, tooaccess this server we will need tooRDP toohello firewall on port 8014, and then allow hello firewall tooroute hello RDP request internally toohello IIS01 RDP Port.</span></span> <span data-ttu-id="2524f-244">hello Azure portal "Csatlakozás" gomb nem fog működni, mert nincs közvetlen RDP-elérési út tooIIS01 (szerint hello portal látható).</span><span class="sxs-lookup"><span data-stu-id="2524f-244">hello Azure portal's "Connect" button won't work because there is no direct RDP path tooIIS01 (as far as hello portal can see).</span></span> <span data-ttu-id="2524f-245">Ez azt jelenti, hogy az összes kapcsolat hello internet lesz toohello biztonsági szolgáltatás és a Port, pl. secscv001.cloudapp.net:xxxx (hello hivatkozás a fenti diagram hello megfeleltetése külső Port és a belső IP-cím és a Port).</span><span class="sxs-lookup"><span data-stu-id="2524f-245">This means all connections from hello internet will be toohello Security Service and a Port, e.g. secscv001.cloudapp.net:xxxx (reference hello above diagram for hello mapping of External Port and Internal IP and Port).</span></span>
> 
> 

<span data-ttu-id="2524f-246">A végpont használatával megnyitható vagy a Virtuálisgép-létrehozási hello időpontjában vagy post build hello a példaként megadott parancsfájlt végezheti el, és a következő kódrészletet az alább látható módon (Megjegyzés; bármely dollárjelet elem kezdődő (pl.: $VMName[$i]) hello parancsfájlból hello referen a felhasználó által definiált változó a dokumentum CE szakaszát.</span><span class="sxs-lookup"><span data-stu-id="2524f-246">An endpoint can be opened either at hello time of VM creation or post build as is done in hello example script and shown below in this code snippet (Note; any item beginning with a dollar sign (e.g.: $VMName[$i]) is a user defined variable from hello script in hello reference section of this document.</span></span> <span data-ttu-id="2524f-247">hello "$i" szögletes [$i] jelöli hello tömb számát egy adott virtuális gép virtuális gépek tömbben):</span><span class="sxs-lookup"><span data-stu-id="2524f-247">hello “$i” in brackets, [$i], represents hello array number of a specific VM in an array of VMs):</span></span>

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

<span data-ttu-id="2524f-248">Bár nem jelenik meg világosan itt miatt vannak-e a változókat, de a végpontok toohello használatát **csak** megnyitnia azon hello biztonsági felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2524f-248">Although not clearly shown here due toohello use of variables, but endpoints are **only** opened on hello Security Cloud Service.</span></span> <span data-ttu-id="2524f-249">Ez az, hogy minden bejövő forgalom kezelése tooensure (továbbítani, NAT, csökkent) hello tűzfal.</span><span class="sxs-lookup"><span data-stu-id="2524f-249">This is tooensure that all inbound traffic is handled (routed, NAT'd, dropped) by hello firewall.</span></span>

<span data-ttu-id="2524f-250">Egy olyan kezelőügyfelet toobe PC toomanage hello tűzfal telepítve kell, és a szükséges hello-konfigurációk létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2524f-250">A management client will need toobe installed on a PC toomanage hello firewall and create hello configurations needed.</span></span> <span data-ttu-id="2524f-251">Hogyan toomanage hello eszköz hello dokumentáció a tűzfal (vagy más NVA) szállító talál.</span><span class="sxs-lookup"><span data-stu-id="2524f-251">See hello documentation from your firewall (or other NVA) vendor on how toomanage hello device.</span></span> <span data-ttu-id="2524f-252">Ez a szakasz és hello következő szakaszt, tűzfal-szabályok létrehozását, hello maradéka hello konfigurációs hello tűzfal, hello szállító felügyeleti ügyfél (azaz nem hello Azure-portál vagy PowerShell) segítségével ismerteti.</span><span class="sxs-lookup"><span data-stu-id="2524f-252">hello remainder of this section and hello next section, Firewall Rules Creation, will describe hello configuration of hello firewall itself, through hello vendors management client (i.e. not hello Azure portal or PowerShell).</span></span>

<span data-ttu-id="2524f-253">A letölthető ügyfél és a kapcsolódó toohello ebben a példában használt Barracuda utasítások itt találhatók: [Barracuda NG rendszergazda](https://techlib.barracuda.com/NG61/NGAdmin)</span><span class="sxs-lookup"><span data-stu-id="2524f-253">Instructions for client download and connecting toohello Barracuda used in this example can be found here: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span></span>

<span data-ttu-id="2524f-254">Miután bejelentkezett hello tűzfal alakzatot, de a tűzfalszabályok létrehozása előtt, vannak-e két előfeltétel objektumosztályok megkönnyítheti létrehozása hello szabályok; Hálózati & szolgáltatás objektumok.</span><span class="sxs-lookup"><span data-stu-id="2524f-254">Once logged onto hello firewall but before creating firewall rules, there are two prerequisite object classes that can make creating hello rules easier; Network & Service objects.</span></span>

<span data-ttu-id="2524f-255">Ebben a példában három elnevezett hálózati objektumok meghatározott (egy hello Frontend alhálózathoz és hello Backend alhálózathoz is egy hálózati objektum hello DNS-kiszolgáló IP-címhez hello) kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="2524f-255">For this example, three named network objects should be defined (one for hello Frontend subnet and hello Backend subnet, also a network object for hello IP address of hello DNS server).</span></span> <span data-ttu-id="2524f-256">toocreate elnevezett hálózati; hello Barracuda NG felügyeleti ügyfél irányítópult-től kezdődő, toohello konfiguráció lapon keresse meg, a hello működési konfigurációs szakaszban kattintson a Ruleset, majd kattintson "Hálózat" hello Tűzfalobjektumokat menüben, majd új hello hálózatok Szerkesztés menü.</span><span class="sxs-lookup"><span data-stu-id="2524f-256">toocreate a named network; starting from hello Barracuda NG Admin client dashboard, navigate toohello configuration tab, in hello Operational Configuration section click Ruleset, then click “Networks” under hello Firewall Objects menu, then click New in hello Edit Networks menu.</span></span> <span data-ttu-id="2524f-257">hello hálózati objektum most hozhatók létre, hello nevét és hello előtag hozzáadásával:</span><span class="sxs-lookup"><span data-stu-id="2524f-257">hello network object can now be created, by adding hello name and hello prefix:</span></span>

<span data-ttu-id="2524f-258">![Hozzon létre egy előtér-hálózati objektumot][3]</span><span class="sxs-lookup"><span data-stu-id="2524f-258">![Create a FrontEnd Network Object][3]</span></span>

<span data-ttu-id="2524f-259">Ezzel létrehoz egy nevesített hálózati hello FrontEnd alhálózathoz, egy hasonló objektum hello BackEnd alhálózathoz is létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="2524f-259">This will create a named network for hello FrontEnd subnet, a similar object should be created for hello BackEnd subnet as well.</span></span> <span data-ttu-id="2524f-260">Most hello alhálózatok könnyebben hivatkozható hello tűzfalszabályok név szerint.</span><span class="sxs-lookup"><span data-stu-id="2524f-260">Now hello subnets can be more easily referenced by name in hello firewall rules.</span></span>

<span data-ttu-id="2524f-261">A DNS-kiszolgáló objektum hello:</span><span class="sxs-lookup"><span data-stu-id="2524f-261">For hello DNS Server Object:</span></span>

<span data-ttu-id="2524f-262">![Hozzon létre egy DNS-kiszolgáló objektum][4]</span><span class="sxs-lookup"><span data-stu-id="2524f-262">![Create a DNS Server Object][4]</span></span>

<span data-ttu-id="2524f-263">Az egyetlen IP-címre való hivatkozás a DNS-szabályban hello dokumentum későbbi szakaszában fog történni.</span><span class="sxs-lookup"><span data-stu-id="2524f-263">This single IP address reference will be utilized in a DNS rule later in hello document.</span></span>

<span data-ttu-id="2524f-264">hello második előfeltétel objektumok olyan szolgáltatások objektumok.</span><span class="sxs-lookup"><span data-stu-id="2524f-264">hello second prerequisite objects are Services objects.</span></span> <span data-ttu-id="2524f-265">Ezek képviselik hello RDP csatlakozási porttal kiszolgálónként.</span><span class="sxs-lookup"><span data-stu-id="2524f-265">These will represent hello RDP connection ports for each server.</span></span> <span data-ttu-id="2524f-266">Mivel hello meglévő RDP szolgáltatás objektumhoz kötött toohello alapértelmezett RDP-portjára, 3389-es, az új szolgáltatásokat is létrehozható tooallow forgalom hello külső portot (8014-8026).</span><span class="sxs-lookup"><span data-stu-id="2524f-266">Since hello existing RDP service object is bound toohello default RDP port, 3389, new Services can be created tooallow traffic from hello external ports (8014-8026).</span></span> <span data-ttu-id="2524f-267">hello új portok is vehető toohello meglévő RDP szolgáltatást, de a bemutató megkönnyítése érdekében, az egyes szabályok kiszolgálónként is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="2524f-267">hello new ports could also be added toohello existing RDP service, but for ease of demonstration, an individual rule for each server can be created.</span></span> <span data-ttu-id="2524f-268">kiszolgáló; új RDP szabály toocreate hello Barracuda NG felügyeleti ügyfél irányítópult-től kezdődő toohello konfiguráció lapon lépjen, a hello működési konfigurációs szakasz szabálykészletben, kattintson, majd kattintson a "Szolgáltatások" hello Tűzfalobjektumokat menü, görgessen lefelé hello szolgáltatás listája alatt hello kiválasztása "RDP" szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2524f-268">toocreate a new RDP rule for a server; starting from hello Barracuda NG Admin client dashboard, navigate toohello configuration tab, in hello Operational Configuration section click Ruleset, then click “Services” under hello Firewall Objects menu, scroll down hello list of services and select hello “RDP” service.</span></span> <span data-ttu-id="2524f-269">Kattintson a jobb gombbal és válassza a másolás, majd kattintson a jobb gombbal, és válassza ki a beillesztés.</span><span class="sxs-lookup"><span data-stu-id="2524f-269">Right-click and select copy, then right-click and select Paste.</span></span> <span data-ttu-id="2524f-270">Már létezik egy RDP-Copy1 szolgáltatás objektum, amely szerkeszthető.</span><span class="sxs-lookup"><span data-stu-id="2524f-270">There is now a RDP-Copy1 Service Object that can be edited.</span></span> <span data-ttu-id="2524f-271">Kattintson a jobb gombbal az RDP-Copy1, és válassza a Szerkesztés, hello szerkesztése szolgáltatás objektum ablak jelenik meg, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="2524f-271">Right-click RDP-Copy1 and select Edit, hello Edit Service Object window will pop up as shown here:</span></span>

<span data-ttu-id="2524f-272">![Alapértelmezett RDP-szabály másolása][5]</span><span class="sxs-lookup"><span data-stu-id="2524f-272">![Copy of Default RDP Rule][5]</span></span>

<span data-ttu-id="2524f-273">hello értékek majd lehet szerkesztett toorepresent hello RDP-szolgáltatás egy adott kiszolgáló számára.</span><span class="sxs-lookup"><span data-stu-id="2524f-273">hello values can then be edited toorepresent hello RDP service for a specific server.</span></span> <span data-ttu-id="2524f-274">AppVM01 hello alapértelmezett feletti RDP szabály legyen módosított tooreflect egy új szolgáltatás nevét, leírását, és a 8. ábra diagram hello használt külső RDP-portjára (Megjegyzés: hello portok vannak változása hello RDP alapértelmezett 3389 toohello külső port ezt használja: adott kiszolgáló hello esetben AppVM01 hello külső port 8025) hello módosított szolgáltatás alább találja:</span><span class="sxs-lookup"><span data-stu-id="2524f-274">For AppVM01 hello above default RDP rule should be modified tooreflect a new Service Name, Description, and external RDP Port used in hello Figure 8 diagram (Note: hello ports are changed from hello RDP default of 3389 toohello external port being used for this specific server, in hello case of AppVM01 hello external Port is 8025) hello modified service is shown below:</span></span>

<span data-ttu-id="2524f-275">![AppVM01 szabály][6]</span><span class="sxs-lookup"><span data-stu-id="2524f-275">![AppVM01 Rule][6]</span></span>

<span data-ttu-id="2524f-276">Ez a folyamat ismételt toocreate RDP NFS-kiszolgálók; fennmaradó hello kell lennie. AppVM02 DNS01 és IIS01.</span><span class="sxs-lookup"><span data-stu-id="2524f-276">This process must be repeated toocreate RDP Services for hello remaining servers; AppVM02, DNS01, and IIS01.</span></span> <span data-ttu-id="2524f-277">Ezek a szolgáltatások hello létrehozását teszi hello szabályok létrehozása egyszerűbb és viszonylag kézenfekvő hello a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="2524f-277">hello creation of these Services will make hello Rule creation simpler and more obvious in hello next section.</span></span>

> [!NOTE]
> <span data-ttu-id="2524f-278">Hello tűzfal RDP szolgáltatásnak nincs szükség két okból; 1.) első hello tűzfal virtuális gép egy Linux-alapú lemezkép, ezért SSH port 22-es VM Management helyett RDP és a 2.) 22-es portot használják, és más felügyeleti két portok engedélyezettek hello tooallow kezelési kapcsolatot az alább ismertetett első felügyeleti szabály.</span><span class="sxs-lookup"><span data-stu-id="2524f-278">An RDP service for hello Firewall is not needed for two reasons; 1) first hello firewall VM is a Linux based image so SSH would be used on port 22 for VM management instead of RDP, and 2) port 22, and two other management ports are allowed in hello first management rule described below tooallow for management connectivity.</span></span>
> 
> 

### <a name="firewall-rules-creation"></a><span data-ttu-id="2524f-279">Tűzfal-szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="2524f-279">Firewall Rules Creation</span></span>
<span data-ttu-id="2524f-280">Ebben a példában használt tűzfal-szabályok három különböző, azok összes rendelkeznek, különböző ikonok:</span><span class="sxs-lookup"><span data-stu-id="2524f-280">There are three types of firewall rules used in this example, they all have distinct icons:</span></span>

<span data-ttu-id="2524f-281">hello alkalmazás átirányítási szabály: ![alkalmazás átirányítási ikonja][7]</span><span class="sxs-lookup"><span data-stu-id="2524f-281">hello Application Redirect rule: ![Application Redirect Icon][7]</span></span>

<span data-ttu-id="2524f-282">hello cél NAT-szabály: ![cél NAT ikon][8]</span><span class="sxs-lookup"><span data-stu-id="2524f-282">hello Destination NAT rule: ![Destination NAT Icon][8]</span></span>

<span data-ttu-id="2524f-283">hello hozzáférési szabály: ![átadni ikon][9]</span><span class="sxs-lookup"><span data-stu-id="2524f-283">hello Pass rule: ![Pass Icon][9]</span></span>

<span data-ttu-id="2524f-284">Ezek a szabályok bővebben hello Barracuda webhelyen található.</span><span class="sxs-lookup"><span data-stu-id="2524f-284">More information on these rules can be found at hello Barracuda web site.</span></span>

<span data-ttu-id="2524f-285">toocreate szabályainak hello (vagy ellenőrizze a meglévő alapértelmezett szabályok), hello Barracuda NG felügyeleti ügyfél irányítópult-től kezdődő, keresse meg a toohello konfiguráció lapon, a hello működési konfigurációs szakaszban kattintson a Ruleset.</span><span class="sxs-lookup"><span data-stu-id="2524f-285">toocreate hello following rules (or verify existing default rules), starting from hello Barracuda NG Admin client dashboard, navigate toohello configuration tab, in hello Operational Configuration section click Ruleset.</span></span> <span data-ttu-id="2524f-286">A rács nevű, "Main szabályok" jeleníti meg a tűzfalon az aktív és inaktív szabályok meglévő hello.</span><span class="sxs-lookup"><span data-stu-id="2524f-286">A grid called, “Main Rules” will show hello existing active and deactivated rules on this firewall.</span></span> <span data-ttu-id="2524f-287">A hello jobb felső sarkában a rácson a kisméretű, zöld "+" gombra, kattintson az új szabály toocreate (Megjegyzés: a tűzfal "zárolva" a változásokat, ha a gomb "Lock" jelölésű, és nem toocreate, vagy a szabályok szerkesztése, kattintson erre a gombra túl szabály se "feloldásához" hello t és szerkeszthető).</span><span class="sxs-lookup"><span data-stu-id="2524f-287">In hello upper right corner of this grid is a small, green “+” button, click this toocreate a new rule (Note: your firewall may be “locked” for changes, if you see a button marked “Lock” and you are unable toocreate or edit rules, click this button too“unlock” hello rule set and allow editing).</span></span> <span data-ttu-id="2524f-288">Ha tooedit meglévő szabály, válassza ki, hogy a szabály, kattintson a jobb gombbal, és válassza ki a szabály szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="2524f-288">If you wish tooedit an existing rule, select that rule, right-click and select Edit Rule.</span></span>

<span data-ttu-id="2524f-289">Ha a szabályok létrehozása és/vagy módosítva, fel kell toohello tűzfal leküldött és majd aktiválni, ha ez nem történik meg a módosítások életbe hello szabály.</span><span class="sxs-lookup"><span data-stu-id="2524f-289">Once your rules are created and/or modified, they must be pushed toohello firewall and then activated, if this is not done hello rule changes will not take effect.</span></span> <span data-ttu-id="2524f-290">hello leküldéses és az aktiválási folyamat alábbiakban hello részletek szabály leírását.</span><span class="sxs-lookup"><span data-stu-id="2524f-290">hello push and activation process is described below hello details rule descriptions.</span></span>

<span data-ttu-id="2524f-291">minden egyes szabály hello mintaadatokról szükséges toocomplete ebben a példában az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="2524f-291">hello specifics of each rule required toocomplete this example are described as follows:</span></span>

* <span data-ttu-id="2524f-292">**Tűzfal-kezelési szabály**: az alkalmazás átirányítási szabály forgalmát toopass toohello kezelőportjai hello hálózati virtuális készülék, ebben a példában a Barracuda NextGen tűzfal engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="2524f-292">**Firewall Management Rule**: This App Redirect rule allows traffic toopass toohello management ports of hello network virtual appliance, in this example a Barracuda NextGen Firewall.</span></span> <span data-ttu-id="2524f-293">hello kezelőportjai meg 801, a 807 és opcionálisan 22.</span><span class="sxs-lookup"><span data-stu-id="2524f-293">hello management ports are 801, 807 and optionally 22.</span></span> <span data-ttu-id="2524f-294">hello külső és belső portok vannak hello (azaz nem port fordítási) azonos.</span><span class="sxs-lookup"><span data-stu-id="2524f-294">hello external and internal ports are hello same (i.e. no port translation).</span></span> <span data-ttu-id="2524f-295">A szabály, a telepítő-MGMT-hozzáférés alapértelmezett szabály, amely alapértelmezés szerint engedélyezett (a Barracuda NextGen tűzfal 6.1-es verzió).</span><span class="sxs-lookup"><span data-stu-id="2524f-295">This rule, SETUP-MGMT-ACCESS, is a default rule and enabled by default (in Barracuda NextGen Firewall version 6.1).</span></span>
  
    <span data-ttu-id="2524f-296">![Felügyeleti tűzfalszabály][10]</span><span class="sxs-lookup"><span data-stu-id="2524f-296">![Firewall Management Rule][10]</span></span>

> [!TIP]
> <span data-ttu-id="2524f-297">hello Ez a szabály forrás-címtér ilyen, ha hello ismert felügyeleti IP-címtartományok, csökkenti a hatókör volna is csökkentse hello támadási felület toohello felügyeleti portokat.</span><span class="sxs-lookup"><span data-stu-id="2524f-297">hello source address space in this rule is Any, if hello management IP address ranges are known, reducing this scope would also reduce hello attack surface toohello management ports.</span></span>
> 
> 

* <span data-ttu-id="2524f-298">**RDP-szabályok**: ezek cél NAT-szabályok lehetővé teszi hello kezelését az egyes kiszolgálók RDP-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="2524f-298">**RDP Rules**:  These Destination NAT rules will allow management of hello individual servers via RDP.</span></span>
  <span data-ttu-id="2524f-299">Nincsenek négy szükséges kritikus mezők toocreate Ez a szabály:</span><span class="sxs-lookup"><span data-stu-id="2524f-299">There are four critical fields needed toocreate this rule:</span></span>
  
  1. <span data-ttu-id="2524f-300">Forrás – tooallow RDP helyről való munkavégzéséből és hello hivatkozás "A" hello mezőjének használatban van.</span><span class="sxs-lookup"><span data-stu-id="2524f-300">Source – tooallow RDP from anywhere, hello reference “Any” is used in hello Source field.</span></span>
  2. <span data-ttu-id="2524f-301">Szolgáltatás – használja a megfelelő objektumot korábban létrehozott ebben az esetben "AppVM01 RDP" hello, hello külső portok átirányítási toohello kiszolgálók helyi IP-cím és tooport 3386 (hello alapértelmezett RDP-portjára).</span><span class="sxs-lookup"><span data-stu-id="2524f-301">Service – use hello appropriate Service Object created earlier, in this case “AppVM01 RDP”, hello external ports redirect toohello servers local IP address and tooport 3386 (hello default RDP port).</span></span> <span data-ttu-id="2524f-302">Ez a szabály az RDP-hozzáférést tooAppVM01 van.</span><span class="sxs-lookup"><span data-stu-id="2524f-302">This specific rule is for RDP access tooAppVM01.</span></span>
  3. <span data-ttu-id="2524f-303">Cél-kell hello *hello tűzfalon helyi port*, "DCHP 1 helyi IP-címe" vagy eth0 statikus IP-címek használata.</span><span class="sxs-lookup"><span data-stu-id="2524f-303">Destination – should be hello *local port on hello firewall*, “DCHP 1 Local IP” or eth0 if using static IPs.</span></span> <span data-ttu-id="2524f-304">hello sorszámmal (eth0, eth1 stb.) Ha a hálózati berendezések több helyi kapcsolatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="2524f-304">hello ordinal number (eth0, eth1, etc) may be different if your network appliance has multiple local interfaces.</span></span> <span data-ttu-id="2524f-305">Hello portja hello tűzfal küldi ki az (lehet hello ugyanaz, mint fogadó port hello), hello tényleges irányított célobjektuma hello Céllista mezőben.</span><span class="sxs-lookup"><span data-stu-id="2524f-305">This is hello port hello firewall is sending out from (may be hello same as hello receiving port), hello actual routed destination is in hello Target List field.</span></span>
  4. <span data-ttu-id="2524f-306">Átirányítás – Ez a szakasz azt ismerteti hello virtuális készülék ahol tooultimately a forgalom átirányítására.</span><span class="sxs-lookup"><span data-stu-id="2524f-306">Redirection – this section tells hello virtual appliance where tooultimately redirect this traffic.</span></span> <span data-ttu-id="2524f-307">hello legegyszerűbb átirányítási tooplace hello IP-cím és Port (nem kötelező) hello Céllista mezőben.</span><span class="sxs-lookup"><span data-stu-id="2524f-307">hello simplest redirection is tooplace hello IP and Port (optional) in hello Target List field.</span></span> <span data-ttu-id="2524f-308">Ha nincs port használt hello célport hello bejövő kérés lesz (azaz nem fordítási), használható, ha a port van kijelölve hello port lesz NAT volna együtt hello IP cím.</span><span class="sxs-lookup"><span data-stu-id="2524f-308">If no port is used hello destination port on hello inbound request will be used (ie no translation), if a port is designated hello port will also be NAT’d along with hello IP address.</span></span>
     
     <span data-ttu-id="2524f-309">![RDP tűzfalszabály][11]</span><span class="sxs-lookup"><span data-stu-id="2524f-309">![Firewall RDP Rule][11]</span></span>
     
     <span data-ttu-id="2524f-310">Összesen négy RDP-szabályok létrehozása toobe lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="2524f-310">A total of four RDP rules will need toobe created:</span></span> 
     
     | <span data-ttu-id="2524f-311">Szabály neve</span><span class="sxs-lookup"><span data-stu-id="2524f-311">Rule Name</span></span> | <span data-ttu-id="2524f-312">Kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2524f-312">Server</span></span> | <span data-ttu-id="2524f-313">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="2524f-313">Service</span></span> | <span data-ttu-id="2524f-314">Cél lista</span><span class="sxs-lookup"><span data-stu-id="2524f-314">Target List</span></span> |
     | --- | --- | --- | --- |
     | <span data-ttu-id="2524f-315">RDP-IIS01</span><span class="sxs-lookup"><span data-stu-id="2524f-315">RDP-to-IIS01</span></span> |<span data-ttu-id="2524f-316">IIS01</span><span class="sxs-lookup"><span data-stu-id="2524f-316">IIS01</span></span> |<span data-ttu-id="2524f-317">IIS01 RDP</span><span class="sxs-lookup"><span data-stu-id="2524f-317">IIS01 RDP</span></span> |<span data-ttu-id="2524f-318">10.0.1.4:3389</span><span class="sxs-lookup"><span data-stu-id="2524f-318">10.0.1.4:3389</span></span> |
     | <span data-ttu-id="2524f-319">RDP-DNS01</span><span class="sxs-lookup"><span data-stu-id="2524f-319">RDP-to-DNS01</span></span> |<span data-ttu-id="2524f-320">DNS01</span><span class="sxs-lookup"><span data-stu-id="2524f-320">DNS01</span></span> |<span data-ttu-id="2524f-321">DNS01 RDP</span><span class="sxs-lookup"><span data-stu-id="2524f-321">DNS01 RDP</span></span> |<span data-ttu-id="2524f-322">10.0.2.4:3389</span><span class="sxs-lookup"><span data-stu-id="2524f-322">10.0.2.4:3389</span></span> |
     | <span data-ttu-id="2524f-323">RDP-AppVM01</span><span class="sxs-lookup"><span data-stu-id="2524f-323">RDP-to-AppVM01</span></span> |<span data-ttu-id="2524f-324">AppVM01</span><span class="sxs-lookup"><span data-stu-id="2524f-324">AppVM01</span></span> |<span data-ttu-id="2524f-325">AppVM01 RDP</span><span class="sxs-lookup"><span data-stu-id="2524f-325">AppVM01 RDP</span></span> |<span data-ttu-id="2524f-326">10.0.2.5:3389</span><span class="sxs-lookup"><span data-stu-id="2524f-326">10.0.2.5:3389</span></span> |
     | <span data-ttu-id="2524f-327">RDP-AppVM02</span><span class="sxs-lookup"><span data-stu-id="2524f-327">RDP-to-AppVM02</span></span> |<span data-ttu-id="2524f-328">AppVM02</span><span class="sxs-lookup"><span data-stu-id="2524f-328">AppVM02</span></span> |<span data-ttu-id="2524f-329">AppVm02 RDP</span><span class="sxs-lookup"><span data-stu-id="2524f-329">AppVm02 RDP</span></span> |<span data-ttu-id="2524f-330">10.0.2.6:3389</span><span class="sxs-lookup"><span data-stu-id="2524f-330">10.0.2.6:3389</span></span> |

> [!TIP]
> <span data-ttu-id="2524f-331">Hello körét hello forrás és a szolgáltatás mezőket szűkíteni csökkenti hello támadási felületét.</span><span class="sxs-lookup"><span data-stu-id="2524f-331">Narrowing down hello scope of hello Source and Service fields will reduce hello attack surface.</span></span> <span data-ttu-id="2524f-332">a korlátozott hello hatókör, amely lehetővé teszi a funkció kell használni.</span><span class="sxs-lookup"><span data-stu-id="2524f-332">hello most limited scope that will allow functionality should be used.</span></span>
> 
> 

* <span data-ttu-id="2524f-333">**Alkalmazás forgalomra vonatkozó szabályok**: nincsenek a két alkalmazás forgalomra vonatkozó szabályok, először a hello előtérbeli webes forgalomban hello és hello második adatforgalomhoz hello háttér (pl. web server toodata réteg).</span><span class="sxs-lookup"><span data-stu-id="2524f-333">**Application Traffic Rules**: There are two Application Traffic Rules, hello first for hello front end web traffic, and hello second for hello back end traffic (eg web server toodata tier).</span></span> <span data-ttu-id="2524f-334">Ezek a szabályok hello hálózati architektúra (ahol a kiszolgálók kerülnek) és a forgalom (melyik irányba hello forgalmat, és mely portokat kell használni) függ.</span><span class="sxs-lookup"><span data-stu-id="2524f-334">These rules will depend on hello network architecture (where your servers are placed) and traffic flows (which direction hello traffic flows, and which ports are used).</span></span>
  
    <span data-ttu-id="2524f-335">Először ismertetjük, hogy webes forgalomban hello előtér szabály:</span><span class="sxs-lookup"><span data-stu-id="2524f-335">First discussed is hello front end rule for web traffic:</span></span>
  
    <span data-ttu-id="2524f-336">![Webes tűzfalszabály][12]</span><span class="sxs-lookup"><span data-stu-id="2524f-336">![Firewall Web Rule][12]</span></span>
  
    <span data-ttu-id="2524f-337">A cél NAT-szabály engedélyezi, hogy hello tényleges alkalmazás forgalom tooreach hello alkalmazáskiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="2524f-337">This Destination NAT rule allows hello actual application traffic tooreach hello application server.</span></span> <span data-ttu-id="2524f-338">Hello más szabályok engedélyezik a biztonsági, felügyeleti stb, amíg az alkalmazás szabályok lettek, mi engedélyezése külső felhasználók vagy szolgáltatások tooaccess hello alkalmazás(ok).</span><span class="sxs-lookup"><span data-stu-id="2524f-338">While hello other rules allow for security, management, etc., Application Rules are what allow external users or services tooaccess hello application(s).</span></span> <span data-ttu-id="2524f-339">Ebben a példában egyetlen webkiszolgálón való üzemeltetésekor 80-as porton van, így hello egyetlen alkalmazás tűzfalszabály átirányítja a bejövő forgalom toohello külső IP-toohello webes kiszolgálók belső IP-címet.</span><span class="sxs-lookup"><span data-stu-id="2524f-339">For this example, there is a single web server on port 80, thus hello single firewall application rule will redirect inbound traffic toohello external IP, toohello web servers internal IP address.</span></span>
  
    <span data-ttu-id="2524f-340">**Megjegyzés:**:, hogy nincs hozzárendelve hello Céllista mezőben port, így hello bejövő port 80-as (vagy 443-as hello szolgáltatás kiválasztva) használja a hello átirányítási hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="2524f-340">**Note**: that there is no port assigned in hello Target List field, thus hello inbound port 80 (or 443 for hello Service selected) will be used in hello redirection of hello web server.</span></span> <span data-ttu-id="2524f-341">Hello webalkalmazás-kiszolgáló egy másik portot figyeli, ha például a 8080-as portra, hello cél lista mező frissített too10.0.1.4:8080 tooallow hello Port átirányítási is lehet.</span><span class="sxs-lookup"><span data-stu-id="2524f-341">If hello web server is listening on a different port, for example port 8080, hello Target List field could be updated too10.0.1.4:8080 tooallow for hello Port redirection as well.</span></span>
  
    <span data-ttu-id="2524f-342">Tovább alkalmazás forgalmi szabály hello hello háttér szabály tooallow hello webkiszolgáló tootalk toohello AppVM01 kiszolgáló (de nem AppVM02) bármely szolgáltatáson keresztül:</span><span class="sxs-lookup"><span data-stu-id="2524f-342">hello next Application Traffic Rule is hello back end rule tooallow hello Web Server tootalk toohello AppVM01 server (but not AppVM02) via Any service:</span></span>
  
    <span data-ttu-id="2524f-343">![Tűzfalszabály AppVM01][13]</span><span class="sxs-lookup"><span data-stu-id="2524f-343">![Firewall AppVM01 Rule][13]</span></span>
  
    <span data-ttu-id="2524f-344">A hozzáférési szabály lehetővé teszi, hogy a bármely IIS-kiszolgálót a hello Frontend alhálózathoz tooreach hello AppVM01 (IP-cím 10.0.2.5) bármely porton hello webes alkalmazás által igényelt protokoll tooaccess adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="2524f-344">This Pass rule allows any IIS server on hello Frontend subnet tooreach hello AppVM01 (IP Address 10.0.2.5) on Any port, using any Protocol tooaccess data needed by hello web application.</span></span>
  
    <span data-ttu-id="2524f-345">Ezen a képernyőn megtekinthet egy "\<explicit cél\>" hello cél mező toosignify 10.0.2.5 hello célként használatos.</span><span class="sxs-lookup"><span data-stu-id="2524f-345">In this screen shot an "\<explicit-dest\>" is used in hello Destination field toosignify 10.0.2.5 as hello destination.</span></span> <span data-ttu-id="2524f-346">Ennek oka lehet, vagy explicit látható módon, vagy nevű hálózati objektum (ahogy úgy történt, a DNS-kiszolgáló hello hello előfeltételei).</span><span class="sxs-lookup"><span data-stu-id="2524f-346">This could be either explicit as shown or a named Network Object (as was done in hello prerequisites for hello DNS server).</span></span> <span data-ttu-id="2524f-347">Ez működik-e hello tűzfal toohello rendszergazdája, toowhich metódus lesz használva.</span><span class="sxs-lookup"><span data-stu-id="2524f-347">This is up toohello administrator of hello firewall as toowhich method will be used.</span></span> <span data-ttu-id="2524f-348">tooadd 10.0.2.5 egy Explict Desitnation, kattintson duplán a hello első üres sor alatt \<explicit cél\> és hello ablakban hello címet adjon meg.</span><span class="sxs-lookup"><span data-stu-id="2524f-348">tooadd 10.0.2.5 as an Explict Desitnation, double-click on hello first blank row under \<explicit-dest\> and enter hello address in hello window that pops up.</span></span>
  
    <span data-ttu-id="2524f-349">A átadni szabálynak nem NAT van szükség, mert ez belső forgalmat, így hello kapcsolódási módszert túl állítható be "No SNAT".</span><span class="sxs-lookup"><span data-stu-id="2524f-349">With this Pass Rule, no NAT is needed since this is internal traffic, so hello Connection Method can be set too"No SNAT".</span></span>
  
    <span data-ttu-id="2524f-350">**Megjegyzés:**: hello Forráshálózat ebben a szabályban hello FrontEnd alhálózaton bármilyen olyan erőforrás értéke, ha csak lesz egy, és a webkiszolgálók, a hálózati objektum erőforrás lehet ismert adott számú létrehozott toobe pontosabb toothose pontos IP-címek helyett hello teljes Frontend alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="2524f-350">**Note**: hello Source network in this rule is any resource on hello FrontEnd subnet, if there will only be one, or a known specific number of web servers, a Network Object resource could be created toobe more specific toothose exact IP addresses instead of hello entire Frontend subnet.</span></span>

> [!TIP]
> <span data-ttu-id="2524f-351">Ez a szabály "Bármely" toomake hello minta alkalmazás könnyebb toosetup, és használjon hello szolgáltatást használ, ez lehetővé teszi a is ICMPv4 (ping) lévő egyetlen szabályt.</span><span class="sxs-lookup"><span data-stu-id="2524f-351">This rule uses hello service “Any” toomake hello sample application easier toosetup and use, this will also allow ICMPv4 (ping) in a single rule.</span></span> <span data-ttu-id="2524f-352">Ez azonban nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="2524f-352">However, this is not a recommended practice.</span></span> <span data-ttu-id="2524f-353">hello portok és protokollok ("szolgáltatások") szűkített toohello minimális előfordulhat, hogy lehetővé teszi, hogy az alkalmazás műveletet tooreduce hello támadási felületét ehhez a határhoz között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2524f-353">hello ports and protocols (“Services”) should be narrowed toohello minimum possible that allows application operation tooreduce hello attack surface across this boundary.</span></span>
> 
> 

<br />

> [!TIP]
> <span data-ttu-id="2524f-354">Bár ez a szabály azt mutatja, hogy egy explicit cél hivatkozást használja, a konzisztens megközelítés hello tűzfal konfigurálása során minden kell használni.</span><span class="sxs-lookup"><span data-stu-id="2524f-354">Although this rule shows an explicit-dest reference being used, a consistent approach should be used throughout hello firewall configuration.</span></span> <span data-ttu-id="2524f-355">Javasoljuk, hogy hálózati objektum nevű hello használni a teljes könnyebb olvashatóság és támogatás.</span><span class="sxs-lookup"><span data-stu-id="2524f-355">It is recommended that hello named Network Object be used throughout for easier readability and supportability.</span></span> <span data-ttu-id="2524f-356">hello explicit-cél használt Itt csak tooshow egy alternatív módszert, és általában nem ajánlott (különösen a összetett konfigurációk).</span><span class="sxs-lookup"><span data-stu-id="2524f-356">hello explicit-dest is used here only tooshow an alternative reference method and is not generally recommended (especially for complex configurations).</span></span>
> 
> 

* <span data-ttu-id="2524f-357">**Kimenő tooInternet szabály**: ehhez adja át a szabály lehetővé teszi a forrás hálózati toopass toohello kiválasztott cél hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="2524f-357">**Outbound tooInternet Rule**: This Pass rule will allow traffic from any Source network toopass toohello selected Destination networks.</span></span> <span data-ttu-id="2524f-358">Ez a szabály egy alapértelmezett szabály már általában hello Barracuda NextGen tűzfalon, de a letiltott állapotú.</span><span class="sxs-lookup"><span data-stu-id="2524f-358">This rule is a default rule usually already on hello Barracuda NextGen firewall, but is in a disabled state.</span></span> <span data-ttu-id="2524f-359">Kattintson a jobb gombbal a szabály a hello aktiválása szabály parancs férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="2524f-359">Right-clicking on this rule can access hello Activate Rule command.</span></span> <span data-ttu-id="2524f-360">Itt látható hello szabály lett módosított tooadd hello két helyi alhálózatok utalás, ez a szabály a dokumentum toohello forrásattribútumának hello Előfeltételek szakaszában létrehozott.</span><span class="sxs-lookup"><span data-stu-id="2524f-360">hello rule shown here has been modified tooadd hello two local subnets that were created as references in hello prerequisite section of this document toohello Source attribute of this rule.</span></span>
  
    <span data-ttu-id="2524f-361">![Kimenő tűzfalszabály][14]</span><span class="sxs-lookup"><span data-stu-id="2524f-361">![Firewall Outbound Rule][14]</span></span>
* <span data-ttu-id="2524f-362">**DNS-szabály**: ehhez adja át a szabály lehetővé teszi, hogy csak DNS (53-as port) forgalmat toopass toohello DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="2524f-362">**DNS Rule**: This Pass rule allows only DNS (port 53) traffic toopass toohello DNS server.</span></span> <span data-ttu-id="2524f-363">Ebben a környezetben a legtöbb forgalmat hello előtér toohello háttér le van tiltva ez a szabály kifejezetten lehetővé teszi a DNS.</span><span class="sxs-lookup"><span data-stu-id="2524f-363">For this environment most traffic from hello FrontEnd toohello BackEnd is blocked, this rule specifically allows DNS.</span></span>
  
    <span data-ttu-id="2524f-364">![DNS tűzfalszabály][15]</span><span class="sxs-lookup"><span data-stu-id="2524f-364">![Firewall DNS Rule][15]</span></span>
  
    <span data-ttu-id="2524f-365">**Megjegyzés:**: Ez a képernyő hibaüzenetet hello kapcsolódási módszert szerepel.</span><span class="sxs-lookup"><span data-stu-id="2524f-365">**Note**: In this screen shot hello Connection Method is included.</span></span> <span data-ttu-id="2524f-366">Mivel ez a szabály belső IP toointernal IP-cím forgalom, nem NATing szükség, a hello csatlakozási mód beállítása túl "No SNAT" a hozzáférési szabályhoz.</span><span class="sxs-lookup"><span data-stu-id="2524f-366">Because this rule is for internal IP toointernal IP address traffic, no NATing is required, this hello Connection Method is set too“No SNAT” for this Pass rule.</span></span>
* <span data-ttu-id="2524f-367">**Alhálózati tooSubnet szabály**: szabály ez adjon át egy alapértelmezett szabály, amely aktiválva lett, és zárulnak alhálózati tooconnect tooany kiszolgáló bármely kiszolgáló hello vissza a módosított tooallow hello előtérben lévő alhálózat alhálózat.</span><span class="sxs-lookup"><span data-stu-id="2524f-367">**Subnet tooSubnet Rule**: This Pass rule is a default rule that has been activated and modified tooallow any server on hello back end subnet tooconnect tooany server on hello front end subnet.</span></span> <span data-ttu-id="2524f-368">Ez a szabály az összes belső forgalom, így a csatlakozási módszerhez hello tooNo SNAT állítható be.</span><span class="sxs-lookup"><span data-stu-id="2524f-368">This rule is all internal traffic so hello Connection Method can be set tooNo SNAT.</span></span>
  
    <span data-ttu-id="2524f-369">![Tűzfal Intra-VNet szabály][16]</span><span class="sxs-lookup"><span data-stu-id="2524f-369">![Firewall Intra-VNet Rule][16]</span></span>
  
    <span data-ttu-id="2524f-370">**Megjegyzés:**: hello kétirányú jelölőnégyzet nincs bejelölve (se nem be van jelölve, a legtöbb szabályok), ez a szabály jelentős abban, hogy ez a szabály az "egy irányított" teszi, a kapcsolatot a számítógép hello háttér alhálózati toohello előtérhálózat, a azonban nem hello névkeresési.</span><span class="sxs-lookup"><span data-stu-id="2524f-370">**Note**: hello Bi-directional checkbox is not checked (nor is it checked in most rules), this is significant for this rule in that it makes this rule “one directional”, a connection can be initiated from hello back end subnet toohello front end network, but not hello reverse.</span></span> <span data-ttu-id="2524f-371">Ha a jelölőnégyzet be lett jelölve, ez a szabály lehetővé tenné kétirányú forgalmat, amely az a logikai diagram nem kívánatos.</span><span class="sxs-lookup"><span data-stu-id="2524f-371">If that checkbox was checked, this rule would enable bi-directional traffic, which from our logical diagram is not desired.</span></span>
* <span data-ttu-id="2524f-372">**Minden forgalmi szabály megtagadása**: hello végső szabály (tekintetében prioritás) mindig legyen, és használja, így ha a forgalom sikertelen toomatch szabályok megelőző hello bármelyikét, a program eltávolítja a szabály által.</span><span class="sxs-lookup"><span data-stu-id="2524f-372">**Deny All Traffic Rule**: This should always be hello final rule (in terms of priority), and as such if a traffic flows fails toomatch any of hello preceding rules it will be dropped by this rule.</span></span> <span data-ttu-id="2524f-373">Ez az alapértelmezett szabály, és általában aktív, módosítás nélkül általában szükség van.</span><span class="sxs-lookup"><span data-stu-id="2524f-373">This is a default rule and usually activated, no modifications are generally needed.</span></span> 
  
    <span data-ttu-id="2524f-374">![Tűzfal megtagadási szabály][17]</span><span class="sxs-lookup"><span data-stu-id="2524f-374">![Firewall Deny Rule][17]</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2524f-375">Miután az összes fenti szabályok hello jönnek létre, fontos, minden egyes szabály tooensure forgalom tooreview hello rangsorának engedélyezett vagy a kívánt módon megtagadva.</span><span class="sxs-lookup"><span data-stu-id="2524f-375">Once all of hello above rules are created, it’s important tooreview hello priority of each rule tooensure traffic will be allowed or denied as desired.</span></span> <span data-ttu-id="2524f-376">Ehhez a példához hello szabályok hello hello Barracuda felügyeleti ügyfél szabályai továbbításának fő rács látható kell hello sorrendben szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="2524f-376">For this example, hello rules are in hello order they should appear in hello Main Grid of forwarding rules in hello Barracuda Management Client.</span></span>
> 
> 

## <a name="rule-activation"></a><span data-ttu-id="2524f-377">A szabály aktiválás</span><span class="sxs-lookup"><span data-stu-id="2524f-377">Rule Activation</span></span>
<span data-ttu-id="2524f-378">Hello módosított szabálykészletben toohello megadását hello rajz, hello szabálykészletben kell toohello tűzfal feltöltött, és a majd aktiválja.</span><span class="sxs-lookup"><span data-stu-id="2524f-378">With hello ruleset modified toohello specification of hello logic diagram, hello ruleset must be uploaded toohello firewall and then activated.</span></span>

<span data-ttu-id="2524f-379">![Tűzfal szabály aktiválás][18]</span><span class="sxs-lookup"><span data-stu-id="2524f-379">![Firewall Rule Activation][18]</span></span>

<span data-ttu-id="2524f-380">Hello felügyeleti ügyfél hello jobb felső sarkában a fürtöket gombok.</span><span class="sxs-lookup"><span data-stu-id="2524f-380">In hello upper right hand corner of hello management client are a cluster of buttons.</span></span> <span data-ttu-id="2524f-381">Hello "küldése módosítások" gomb toosend módosított hello szabályok toohello tűzfal elemre, majd kattintson a hello "Aktiválás" gombra.</span><span class="sxs-lookup"><span data-stu-id="2524f-381">Click hello “Send Changes” button toosend hello modified rules toohello firewall, then click hello “Activate” button.</span></span>

<span data-ttu-id="2524f-382">A hello tűzfal ruleset hello aktiválásával e példa környezet build befejeződött.</span><span class="sxs-lookup"><span data-stu-id="2524f-382">With hello activation of hello firewall ruleset this example environment build is complete.</span></span>

## <a name="traffic-scenarios"></a><span data-ttu-id="2524f-383">Forgalom forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="2524f-383">Traffic Scenarios</span></span>
> [!IMPORTANT]
> <span data-ttu-id="2524f-384">A kulcs takeway tooremember, amely **minden** forgalmat fog érkezni hello tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="2524f-384">A key takeway is tooremember that **all** traffic will come through hello firewall.</span></span> <span data-ttu-id="2524f-385">Igen tooremote asztali toohello IIS01 kiszolgáló, akkor is, ha hello első End felhőalapú szolgáltatás, valamint hello előtér alhálózati tooaccess ehhez a kiszolgálóhoz a tooRDP toohello tűzfal fel kell port 8014, és majd belső az hello tűzfal tooroute hello RDP-kérés engedélyezése toohello IIS01 RDP-portjára.</span><span class="sxs-lookup"><span data-stu-id="2524f-385">So tooremote desktop toohello IIS01 server, even though it's in hello Front End Cloud Service and on hello Front End subnet, tooaccess this server we will need tooRDP toohello firewall on port 8014, and then allow hello firewall tooroute hello RDP request internally toohello IIS01 RDP Port.</span></span> <span data-ttu-id="2524f-386">hello Azure portal "Csatlakozás" gomb nem fog működni, mert nincs közvetlen RDP-elérési út tooIIS01 (szerint hello portal látható).</span><span class="sxs-lookup"><span data-stu-id="2524f-386">hello Azure portal's "Connect" button won't work because there is no direct RDP path tooIIS01 (as far as hello portal can see).</span></span> <span data-ttu-id="2524f-387">Ez azt jelenti, hogy az összes kapcsolat hello internet lesz toohello biztonsági szolgáltatás és a Port, pl. secscv001.cloudapp.net:xxxx.</span><span class="sxs-lookup"><span data-stu-id="2524f-387">This means all connections from hello internet will be toohello Security Service and a Port, e.g. secscv001.cloudapp.net:xxxx.</span></span>
> 
> 

<span data-ttu-id="2524f-388">Ezek a forgatókönyvek a következő tűzfalszabályok hello helyen legyen:</span><span class="sxs-lookup"><span data-stu-id="2524f-388">For these scenarios, hello following firewall rules should be in place:</span></span>

1. <span data-ttu-id="2524f-389">Tűzfalkezelés</span><span class="sxs-lookup"><span data-stu-id="2524f-389">Firewall Management</span></span>
2. <span data-ttu-id="2524f-390">RDP tooIIS01</span><span class="sxs-lookup"><span data-stu-id="2524f-390">RDP tooIIS01</span></span>
3. <span data-ttu-id="2524f-391">RDP tooDNS01</span><span class="sxs-lookup"><span data-stu-id="2524f-391">RDP tooDNS01</span></span>
4. <span data-ttu-id="2524f-392">RDP tooAppVM01</span><span class="sxs-lookup"><span data-stu-id="2524f-392">RDP tooAppVM01</span></span>
5. <span data-ttu-id="2524f-393">RDP tooAppVM02</span><span class="sxs-lookup"><span data-stu-id="2524f-393">RDP tooAppVM02</span></span>
6. <span data-ttu-id="2524f-394">Webes alkalmazás forgalom toohello</span><span class="sxs-lookup"><span data-stu-id="2524f-394">App Traffic toohello Web</span></span>
7. <span data-ttu-id="2524f-395">Alkalmazás forgalom tooAppVM01</span><span class="sxs-lookup"><span data-stu-id="2524f-395">App Traffic tooAppVM01</span></span>
8. <span data-ttu-id="2524f-396">Kimenő toohello Internet</span><span class="sxs-lookup"><span data-stu-id="2524f-396">Outbound toohello Internet</span></span>
9. <span data-ttu-id="2524f-397">Előtérbeli tooDNS01</span><span class="sxs-lookup"><span data-stu-id="2524f-397">Frontend tooDNS01</span></span>
10. <span data-ttu-id="2524f-398">Helyen belüli forgalmat (csak háttér toofront záró)</span><span class="sxs-lookup"><span data-stu-id="2524f-398">Intra-Subnet Traffic (back end toofront end only)</span></span>
11. <span data-ttu-id="2524f-399">Az összes megtagadása</span><span class="sxs-lookup"><span data-stu-id="2524f-399">Deny All</span></span>

<span data-ttu-id="2524f-400">hello tényleges tűzfal szabálykészletben valószínűleg lesz számos más szabályok hozzáadása toothese, bármely adott tűzfal hello szabályok is rendelkezik különböző prioritást mint hello azokat, az itt felsorolt.</span><span class="sxs-lookup"><span data-stu-id="2524f-400">hello actual firewall ruleset will most likely have many other rules in addition toothese, hello rules on any given firewall will also have different priority numbers than hello ones listed here.</span></span> <span data-ttu-id="2524f-401">A lista és a hozzájuk társított számokat tooprovide relevanciájának között csak e 11 szabályok és a relatív prioritását hello közülük.</span><span class="sxs-lookup"><span data-stu-id="2524f-401">This list and associated numbers are tooprovide relevance between just these eleven rules and hello relative priority amongst them.</span></span> <span data-ttu-id="2524f-402">Más szóval; hello tényleges tűzfalon hello "RDP tooIIS01" lehet, hogy lehet szabály szám 5, de mindaddig, amíg volna az ebben a listában hello szándéka igazodni hello "RDP tooDNS01" szabály felett és hello "Tűzfal kezelése" szabály alatt van.</span><span class="sxs-lookup"><span data-stu-id="2524f-402">In other words; on hello actual firewall, hello “RDP tooIIS01” may be rule number 5, but as long as it’s below hello “Firewall Management” rule and above hello “RDP tooDNS01” rule it would align with hello intention of this list.</span></span> <span data-ttu-id="2524f-403">hello lista lesz is segítséget nyújtanak az alábbi forgatókönyvek kivonatosan mutatja; így hello például "FW szabály 9 (DNS)".</span><span class="sxs-lookup"><span data-stu-id="2524f-403">hello list will also aid in hello below scenarios allowing brevity; e.g. “FW Rule 9 (DNS)”.</span></span> <span data-ttu-id="2524f-404">Is kivonatosan mutatja négy RDP-szabályok együttesen lesznek hello nevű, "hello RDP szabályok" független tooRDP hello forgalom forgatókönyv esetén.</span><span class="sxs-lookup"><span data-stu-id="2524f-404">Also for brevity, hello four RDP rules will be collectively called, “hello RDP rules” when hello traffic scenario is unrelated tooRDP.</span></span>

<span data-ttu-id="2524f-405">Visszahívása is, hogy a hálózati biztonsági csoportok helyben a bejövő forgalmat a hello előtér és háttér alhálózatok-e.</span><span class="sxs-lookup"><span data-stu-id="2524f-405">Also recall that Network Security Groups are in-place for inbound internet traffic on hello Frontend and Backend subnets.</span></span>

#### <a name="allowed-internet-tooweb-server"></a><span data-ttu-id="2524f-406">(Engedélyezett) Internet tooWeb kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2524f-406">(Allowed) Internet tooWeb Server</span></span>
1. <span data-ttu-id="2524f-407">Internetes felhasználói kérelmek HTTP oldal SecSvc001.CloudApp.Net (Internet Facing Felhőszolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="2524f-407">Internet user requests HTTP page from SecSvc001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="2524f-408">Felhőalapú szolgáltatás fázisok forgalom nyitott végpontok 10.0.0.4:80 a 80-as port toofirewall kapcsolaton keresztül</span><span class="sxs-lookup"><span data-stu-id="2524f-408">Cloud service passes traffic through open endpoint on port 80 toofirewall interface on 10.0.0.4:80</span></span>
3. <span data-ttu-id="2524f-409">Nincs hozzárendelve tooSecurity alhálózat, a rendszer NSG-szabályok lehetővé teszik a forgalom toofirewall NSG</span><span class="sxs-lookup"><span data-stu-id="2524f-409">No NSG assigned tooSecurity subnet, so system NSG rules allow traffic toofirewall</span></span>
4. <span data-ttu-id="2524f-410">Forgalom találatok hello tűzfal (10.0.1.4) belső IP-címe</span><span class="sxs-lookup"><span data-stu-id="2524f-410">Traffic hits internal IP address of hello firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="2524f-411">Tűzfal szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-411">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="2524f-412">Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-412">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="2524f-413">2 – 5 (RDP szabályok) FW szabályok nem teljesül, a toonext szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="2524f-413">FW Rules 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="2524f-414">FW 6. szabály (App: webes) alkalmazásához forgalom engedélyezve van, tűzfal NAT azt too10.0.1.4 (IIS01)</span><span class="sxs-lookup"><span data-stu-id="2524f-414">FW Rule 6 (App: Web) does apply, traffic is allowed, firewall NATs it too10.0.1.4 (IIS01)</span></span>
6. <span data-ttu-id="2524f-415">hello Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-415">hello Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="2524f-416">NSG 1. szabály (elérésű internetes) nem vonatkozik (Ez az adatforgalom lett NAT hello tűzfal volna, így hello forráscím mostantól hello tűzfal, amelyek a biztonság – alhálózati hello és hello Frontend alhálózathoz NSG toobe "helyi" forgalom láthatók, és így engedélyezve van), toonext szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="2524f-416">NSG Rule 1 (Block Internet) doesn’t apply (this traffic was NAT’d by hello firewall, thus hello source address is now hello firewall which is on hello Security subnet and seen by hello Frontend subnet NSG toobe “local” traffic and is thus allowed), move toonext rule</span></span>
   2. <span data-ttu-id="2524f-417">Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="2524f-417">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
7. <span data-ttu-id="2524f-418">IIS01 a webes forgalom figyeli, ezt a kérelmet kap, és elindítja a hello kérés feldolgozása</span><span class="sxs-lookup"><span data-stu-id="2524f-418">IIS01 is listening for web traffic, receives this request and starts processing hello request</span></span>
8. <span data-ttu-id="2524f-419">IIS01 megpróbál egy FTP-munkamenet tooAppVM01 tooinitiates a Backend alhálózathoz</span><span class="sxs-lookup"><span data-stu-id="2524f-419">IIS01 attempts tooinitiates an FTP session tooAppVM01 on Backend subnet</span></span>
9. <span data-ttu-id="2524f-420">hello UDR útvonal Frontend alhálózaton teszi hello tűzfal hello következő ugrás</span><span class="sxs-lookup"><span data-stu-id="2524f-420">hello UDR route on Frontend subnet makes hello firewall hello next hop</span></span>
10. <span data-ttu-id="2524f-421">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="2524f-421">No outbound rules on Frontend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="2524f-422">Tűzfal szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-422">Firewall begins rule processing:</span></span>
    1. <span data-ttu-id="2524f-423">Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-423">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="2524f-424">Toonext szabály nem alkalmazza a FW szabály 2 – 5 (RDP szabályok), helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-424">FW Rule 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
    3. <span data-ttu-id="2524f-425">FW 6. szabály (App: webes) nem teljesül, a toonext szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="2524f-425">FW Rule 6 (App: Web) doesn’t apply, move toonext rule</span></span>
    4. <span data-ttu-id="2524f-426">FW szabály 7 (App: háttér) alkalmazásához forgalom engedélyezve van, a tűzfal továbbítja a forgalom too10.0.2.5 (AppVM01)</span><span class="sxs-lookup"><span data-stu-id="2524f-426">FW Rule 7 (App: Backend) does apply, traffic is allowed, firewall forwards traffic too10.0.2.5 (AppVM01)</span></span>
12. <span data-ttu-id="2524f-427">hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-427">hello Backend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="2524f-428">NSG 1. szabály (elérésű internetes) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="2524f-428">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="2524f-429">Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="2524f-429">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
13. <span data-ttu-id="2524f-430">AppVM01 hello kérelmet kap, és kezdeményezi hello munkamenet, és válaszol</span><span class="sxs-lookup"><span data-stu-id="2524f-430">AppVM01 receives hello request and initiates hello session and responds</span></span>
14. <span data-ttu-id="2524f-431">hello UDR útvonal a Backend alhálózathoz teszi hello tűzfal hello következő ugrás</span><span class="sxs-lookup"><span data-stu-id="2524f-431">hello UDR route on Backend subnet makes hello firewall hello next hop</span></span>
15. <span data-ttu-id="2524f-432">Mivel nincsenek engedélyezve van-e a nem kimenő NSG-szabályok hello Backend alhálózathoz hello válasz</span><span class="sxs-lookup"><span data-stu-id="2524f-432">Since there are no outbound NSG rules on hello Backend subnet hello response is allowed</span></span>
16. <span data-ttu-id="2524f-433">Mivel a forgalom Ez vissza egy munkamenetet hello tűzfal továbbítja hello válasz hátsó toohello web server (IIS01)</span><span class="sxs-lookup"><span data-stu-id="2524f-433">Because this is returning traffic on an established session hello firewall passes hello response back toohello web server (IIS01)</span></span>
17. <span data-ttu-id="2524f-434">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-434">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="2524f-435">NSG 1. szabály (elérésű internetes) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="2524f-435">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="2524f-436">Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="2524f-436">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
18. <span data-ttu-id="2524f-437">hello IIS server hello választ kap, AppVM01 hello tranzakció, és ha igen épület hello HTTP-válasz, a HTTP-válasz küldött toohello kérelmező</span><span class="sxs-lookup"><span data-stu-id="2524f-437">hello IIS server receives hello response, completes hello transaction with AppVM01, and then completes building hello HTTP response, this HTTP response is sent toohello requestor</span></span>
19. <span data-ttu-id="2524f-438">Mivel nincsenek engedélyezve van-e a nem kimenő NSG-szabályok hello Frontend alhálózathoz hello válasz</span><span class="sxs-lookup"><span data-stu-id="2524f-438">Since there are no outbound NSG rules on hello Frontend subnet hello response is allowed</span></span>
20. <span data-ttu-id="2524f-439">hello HTTP válasz találatok hello tűzfal, és mivel ez hello válasz tooan létrehozott NAT munkamenet elfogadható hello tűzfal</span><span class="sxs-lookup"><span data-stu-id="2524f-439">hello HTTP response hits hello firewall, and because this is hello response tooan established NAT session is accepted by hello firewall</span></span>
21. <span data-ttu-id="2524f-440">hello tűzfal majd átirányítja a felhasználókat hello válasz hátsó toohello internetes felhasználó</span><span class="sxs-lookup"><span data-stu-id="2524f-440">hello firewall then redirects hello response back toohello Internet User</span></span>
22. <span data-ttu-id="2524f-441">Mivel nincsenek kimenő NSG-szabályok vagy UDR ugrások hello Frontend alhálózaton hello választ kap, és hello internetes felhasználó megkapja a kért weblap hello.</span><span class="sxs-lookup"><span data-stu-id="2524f-441">Since there are no outbound NSG rules or UDR hops on hello Frontend subnet hello response is allowed, and hello Internet User receives hello web page requested.</span></span>

#### <a name="allowed-internet-rdp-toobackend"></a><span data-ttu-id="2524f-442">(Engedélyezett) Internet RDP tooBackend</span><span class="sxs-lookup"><span data-stu-id="2524f-442">(Allowed) Internet RDP tooBackend</span></span>
1. <span data-ttu-id="2524f-443">Server Admin interneten kérelmek RDP munkamenet tooAppVM01 keresztül SecSvc001.CloudApp.Net:8025, ahol 8025 az hello felhasználó lehet hozzárendelve hello "RDP tooAppVM01" tűzfalszabály portszáma</span><span class="sxs-lookup"><span data-stu-id="2524f-443">Server Admin on internet requests RDP session tooAppVM01 via SecSvc001.CloudApp.Net:8025, where 8025 is hello user assigned port number for hello “RDP tooAppVM01” firewall rule</span></span>
2. <span data-ttu-id="2524f-444">hello felhőszolgáltatás hello nyitott végpontok forgalmát továbbítja a port 8025 toofirewall felülete 10.0.0.4:8025</span><span class="sxs-lookup"><span data-stu-id="2524f-444">hello cloud service passes traffic through hello open endpoint on port 8025 toofirewall interface on 10.0.0.4:8025</span></span>
3. <span data-ttu-id="2524f-445">Nincs hozzárendelve tooSecurity alhálózat, a rendszer NSG-szabályok lehetővé teszik a forgalom toofirewall NSG</span><span class="sxs-lookup"><span data-stu-id="2524f-445">No NSG assigned tooSecurity subnet, so system NSG rules allow traffic toofirewall</span></span>
4. <span data-ttu-id="2524f-446">Tűzfal szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-446">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="2524f-447">Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-447">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="2524f-448">Toonext szabály FW 2. szabály (RDP IIS) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-448">FW Rule 2 (RDP IIS) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="2524f-449">Toonext szabály FW 3. szabály (RDP DNS01) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-449">FW Rule 3 (RDP DNS01) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="2524f-450">FW 4. szabály (RDP AppVM01) teljesül, a forgalom engedélyezve van, tűzfal NAT azt too10.0.2.5:3386 (RDP-portjához AppVM01)</span><span class="sxs-lookup"><span data-stu-id="2524f-450">FW Rule 4 (RDP AppVM01) does apply, traffic is allowed, firewall NATs it too10.0.2.5:3386 (RDP port on AppVM01)</span></span>
5. <span data-ttu-id="2524f-451">hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-451">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="2524f-452">NSG 1. szabály (elérésű internetes) nem vonatkozik (Ez az adatforgalom lett NAT hello tűzfal volna, így hello forráscím mostantól hello tűzfal, amelyek a biztonság – alhálózati hello és hello Backend alhálózathoz NSG toobe "helyi" forgalom láthatók, és így engedélyezve van), toonext szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="2524f-452">NSG Rule 1 (Block Internet) doesn’t apply (this traffic was NAT’d by hello firewall, thus hello source address is now hello firewall which is on hello Security subnet and seen by hello Backend subnet NSG toobe “local” traffic and is thus allowed), move toonext rule</span></span>
   2. <span data-ttu-id="2524f-453">Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="2524f-453">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
6. <span data-ttu-id="2524f-454">AppVM01 RDP-forgalmát a figyeli és reagál</span><span class="sxs-lookup"><span data-stu-id="2524f-454">AppVM01 is listening for RDP traffic and responds</span></span>
7. <span data-ttu-id="2524f-455">Nincsenek kimenő NSG-szabályok, az alapértelmezett szabályok vonatkoznak, és a forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="2524f-455">With no outbound NSG rules, default rules apply and return traffic is allowed</span></span>
8. <span data-ttu-id="2524f-456">UDR útvonalak kimenő forgalom toohello tűzfal hello következő ugrásként</span><span class="sxs-lookup"><span data-stu-id="2524f-456">UDR routes outbound traffic toohello firewall as hello next hop</span></span>
9. <span data-ttu-id="2524f-457">Mivel ez adott vissza forgalom a egy munkamenetet hello tűzfal továbbítja hello válasz hátsó toohello internetes felhasználó</span><span class="sxs-lookup"><span data-stu-id="2524f-457">Because this is returning traffic on an established session hello firewall passes hello response back toohello internet user</span></span>
10. <span data-ttu-id="2524f-458">RDP-munkamenetbe engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="2524f-458">RDP session is enabled</span></span>
11. <span data-ttu-id="2524f-459">Felhasználói nevének jelszavának megadását kéri az AppVM01</span><span class="sxs-lookup"><span data-stu-id="2524f-459">AppVM01 prompts for user name password</span></span>

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a><span data-ttu-id="2524f-460">(Engedélyezett) Web Server DNS-címkeresés DNS-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2524f-460">(Allowed) Web Server DNS lookup on DNS server</span></span>
1. <span data-ttu-id="2524f-461">Webalkalmazás-kiszolgáló, IIS01, kell egy adatcsatorna www.data.gov, azonban kell tooresolve hello cím.</span><span class="sxs-lookup"><span data-stu-id="2524f-461">Web Server, IIS01, needs a data feed at www.data.gov, but needs tooresolve hello address.</span></span>
2. <span data-ttu-id="2524f-462">hello tartozó fürthálózat-konfiguráció hello hálózatok listája DNS01 (a hello Backend alhálózathoz 10.0.2.4) hello elsődleges DNS-kiszolgálóként, IIS01 küldi hello DNS kérelem tooDNS01</span><span class="sxs-lookup"><span data-stu-id="2524f-462">hello network configuration for hello VNet lists DNS01 (10.0.2.4 on hello Backend subnet) as hello primary DNS server, IIS01 sends hello DNS request tooDNS01</span></span>
3. <span data-ttu-id="2524f-463">UDR útvonalak kimenő forgalom toohello tűzfal hello következő ugrásként</span><span class="sxs-lookup"><span data-stu-id="2524f-463">UDR routes outbound traffic toohello firewall as hello next hop</span></span>
4. <span data-ttu-id="2524f-464">Nincs kimenő NSG-szabályok a következők kötött toohello Frontend alhálózathoz, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="2524f-464">No outbound NSG rules are bound toohello Frontend subnet, traffic is allowed</span></span>
5. <span data-ttu-id="2524f-465">Tűzfal szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-465">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="2524f-466">Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-466">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="2524f-467">Toonext szabály nem alkalmazza a FW szabály 2 – 5 (RDP szabályok), helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-467">FW Rule 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="2524f-468">Nem alkalmazza a FW szabályok 6 és 7 (Alkalmazásszabályok), helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="2524f-468">FW Rules 6 & 7 (App Rules) don’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="2524f-469">Toonext szabály FW szabály 8 (tooInternet) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-469">FW Rule 8 (tooInternet) doesn’t apply, move toonext rule</span></span>
   5. <span data-ttu-id="2524f-470">FW szabály 9 (DNS) alkalmazza, a forgalom engedélyezve van, a tűzfal továbbítja a forgalom too10.0.2.4 (DNS01)</span><span class="sxs-lookup"><span data-stu-id="2524f-470">FW Rule 9 (DNS) does apply, traffic is allowed, firewall forwards traffic too10.0.2.4 (DNS01)</span></span>
6. <span data-ttu-id="2524f-471">hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-471">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="2524f-472">NSG 1. szabály (elérésű internetes) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="2524f-472">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="2524f-473">Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="2524f-473">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
7. <span data-ttu-id="2524f-474">DNS-kiszolgáló hello kérést kap.</span><span class="sxs-lookup"><span data-stu-id="2524f-474">DNS server receives hello request</span></span>
8. <span data-ttu-id="2524f-475">DNS-kiszolgáló nem rendelkezik gyorsítótárazott hello címmel és egy legfelső szintű DNS-kiszolgáló kéri a hello internet</span><span class="sxs-lookup"><span data-stu-id="2524f-475">DNS server doesn’t have hello address cached and asks a root DNS server on hello internet</span></span>
9. <span data-ttu-id="2524f-476">UDR útvonalak kimenő forgalom toohello tűzfal hello következő ugrásként</span><span class="sxs-lookup"><span data-stu-id="2524f-476">UDR routes outbound traffic toohello firewall as hello next hop</span></span>
10. <span data-ttu-id="2524f-477">Nincs kimenő NSG-szabályok a Backend alhálózathoz, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="2524f-477">No outbound NSG rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="2524f-478">Tűzfal szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-478">Firewall begins rule processing:</span></span>
    1. <span data-ttu-id="2524f-479">Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-479">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="2524f-480">Toonext szabály nem alkalmazza a FW szabály 2 – 5 (RDP szabályok), helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-480">FW Rule 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
    3. <span data-ttu-id="2524f-481">Nem alkalmazza a FW szabályok 6 és 7 (Alkalmazásszabályok), helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="2524f-481">FW Rules 6 & 7 (App Rules) don’t apply, move toonext rule</span></span>
    4. <span data-ttu-id="2524f-482">FW szabály 8 (tooInternet) alkalmazni, forgalom engedélyezve van, munkamenet SNAT tooroot hello internetes DNS-kiszolgáló ki.</span><span class="sxs-lookup"><span data-stu-id="2524f-482">FW Rule 8 (tooInternet) does apply, traffic is allowed, session is SNAT out tooroot DNS server on hello Internet</span></span>
12. <span data-ttu-id="2524f-483">Internetes DNS-kiszolgáló válaszol, mivel a munkamenet hello tűzfal kezdeményezték, hello válasz elfogadható hello tűzfal</span><span class="sxs-lookup"><span data-stu-id="2524f-483">Internet DNS server responds, since this session was initiated from hello firewall, hello response is accepted by hello firewall</span></span>
13. <span data-ttu-id="2524f-484">Ez ugyanis a munkamenetet, hello tűzfal továbbítja hello válasz toohello server DNS01 kezdeményezése</span><span class="sxs-lookup"><span data-stu-id="2524f-484">As this is an established session, hello firewall forwards hello response toohello initiating server, DNS01</span></span>
14. <span data-ttu-id="2524f-485">hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-485">hello Backend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="2524f-486">NSG 1. szabály (elérésű internetes) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="2524f-486">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="2524f-487">Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="2524f-487">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
15. <span data-ttu-id="2524f-488">hello DNS-kiszolgáló fogadja és gyorsítótárazza hello választ, és ezután válaszol toohello kezdeti kérés hátsó tooIIS01</span><span class="sxs-lookup"><span data-stu-id="2524f-488">hello DNS server receives and caches hello response, and then responds toohello initial request back tooIIS01</span></span>
16. <span data-ttu-id="2524f-489">hello UDR útvonal a backend alhálózathoz teszi hello tűzfal hello következő ugrás</span><span class="sxs-lookup"><span data-stu-id="2524f-489">hello UDR route on backend subnet makes hello firewall hello next hop</span></span>
17. <span data-ttu-id="2524f-490">Nincsenek kimenő NSG-szabályok szerepelnek a hello Backend alhálózathoz, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="2524f-490">No outbound NSG rules exist on hello Backend subnet, traffic is allowed</span></span>
18. <span data-ttu-id="2524f-491">Ez a hello tűzfal munkamenetet, hello válasz továbbítja hello tűzfal hátsó toohello IIS-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2524f-491">This is an established session on hello firewall, hello response is forwarded by hello firewall back toohello IIS server</span></span>
19. <span data-ttu-id="2524f-492">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-492">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="2524f-493">Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály</span><span class="sxs-lookup"><span data-stu-id="2524f-493">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="2524f-494">hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="2524f-494">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed</span></span>
20. <span data-ttu-id="2524f-495">IIS01 hello választ kap DNS01</span><span class="sxs-lookup"><span data-stu-id="2524f-495">IIS01 receives hello response from DNS01</span></span>

#### <a name="allowed-backend-server-toofrontend-server"></a><span data-ttu-id="2524f-496">(Engedélyezett) Kiszolgáló tooFrontend háttérkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2524f-496">(Allowed) Backend server tooFrontend server</span></span>
1. <span data-ttu-id="2524f-497">RDP-kapcsolaton keresztül tooAppVM02 bejelentkezett rendszergazda olyan fájlt kér, közvetlenül a hello IIS01 kiszolgálón windows Fájlkezelőben-n keresztül</span><span class="sxs-lookup"><span data-stu-id="2524f-497">An administrator logged on tooAppVM02 via RDP requests a file directly from hello IIS01 server via windows file explorer</span></span>
2. <span data-ttu-id="2524f-498">hello UDR útvonal a Backend alhálózathoz teszi hello tűzfal hello következő ugrás</span><span class="sxs-lookup"><span data-stu-id="2524f-498">hello UDR route on Backend subnet makes hello firewall hello next hop</span></span>
3. <span data-ttu-id="2524f-499">Mivel nincsenek engedélyezve van-e a nem kimenő NSG-szabályok hello Backend alhálózathoz hello válasz</span><span class="sxs-lookup"><span data-stu-id="2524f-499">Since there are no outbound NSG rules on hello Backend subnet hello response is allowed</span></span>
4. <span data-ttu-id="2524f-500">Tűzfal szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-500">Firewall begins rule processing:</span></span>
   1. <span data-ttu-id="2524f-501">Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-501">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="2524f-502">Toonext szabály nem alkalmazza a FW szabály 2 – 5 (RDP szabályok), helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-502">FW Rule 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="2524f-503">Nem alkalmazza a FW szabályok 6 és 7 (Alkalmazásszabályok), helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="2524f-503">FW Rules 6 & 7 (App Rules) don’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="2524f-504">Toonext szabály FW szabály 8 (tooInternet) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-504">FW Rule 8 (tooInternet) doesn’t apply, move toonext rule</span></span>
   5. <span data-ttu-id="2524f-505">Nem alkalmazható FW szabály 9 (DNS), helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="2524f-505">FW Rule 9 (DNS) doesn’t apply, move toonext rule</span></span>
   6. <span data-ttu-id="2524f-506">FW szabály 10 (Intra-alhálózat) alkalmazza, a forgalom engedélyezve van, a tűzfal továbbítja a forgalom too10.0.1.4 (IIS01)</span><span class="sxs-lookup"><span data-stu-id="2524f-506">FW Rule 10 (Intra-Subnet) does apply, traffic is allowed, firewall passes traffic too10.0.1.4 (IIS01)</span></span>
5. <span data-ttu-id="2524f-507">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-507">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="2524f-508">NSG 1. szabály (elérésű internetes) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="2524f-508">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="2524f-509">Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="2524f-509">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
6. <span data-ttu-id="2524f-510">Ha a megfelelő hitelesítési és engedélyezési, IIS01 hello kérést fogad és reagál</span><span class="sxs-lookup"><span data-stu-id="2524f-510">Assuming proper authentication and authorization, IIS01 accepts hello request and responds</span></span>
7. <span data-ttu-id="2524f-511">hello UDR útvonal Frontend alhálózaton teszi hello tűzfal hello következő ugrás</span><span class="sxs-lookup"><span data-stu-id="2524f-511">hello UDR route on Frontend subnet makes hello firewall hello next hop</span></span>
8. <span data-ttu-id="2524f-512">Mivel nincsenek engedélyezve van-e a nem kimenő NSG-szabályok hello Frontend alhálózathoz hello válasz</span><span class="sxs-lookup"><span data-stu-id="2524f-512">Since there are no outbound NSG rules on hello Frontend subnet hello response is allowed</span></span>
9. <span data-ttu-id="2524f-513">Ez ugyanis egy meglévő munkamenetben hello tűzfalon ezt a választ kap, és hello tűzfal hello válasz tooAppVM02 adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2524f-513">As this is an existing session on hello firewall this response is allowed and hello firewall returns hello response tooAppVM02</span></span>
10. <span data-ttu-id="2524f-514">Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="2524f-514">Backend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="2524f-515">NSG 1. szabály (elérésű internetes) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="2524f-515">NSG Rule 1 (Block Internet) doesn’t apply, move toonext rule</span></span>
    2. <span data-ttu-id="2524f-516">Alapértelmezett NSG-szabályok alhálózati toosubnet forgalom engedélyezéséhez forgalom engedélyezve van, akkor állítsa le az NSG-szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="2524f-516">Default NSG Rules allow subnet toosubnet traffic, traffic is allowed, stop NSG rule processing</span></span>
11. <span data-ttu-id="2524f-517">AppVM02 hello válasz fogadása</span><span class="sxs-lookup"><span data-stu-id="2524f-517">AppVM02 receives hello response</span></span>

#### <a name="denied-internet-direct-tooweb-server"></a><span data-ttu-id="2524f-518">(Megtagadva) Az Internet közvetlen tooWeb kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2524f-518">(Denied) Internet direct tooWeb Server</span></span>
1. <span data-ttu-id="2524f-519">Internetes felhasználó megpróbál tooaccess hello webkiszolgáló IIS01, hello FrontEnd001.CloudApp.Net szolgáltatás keresztül</span><span class="sxs-lookup"><span data-stu-id="2524f-519">Internet user tries tooaccess hello web server, IIS01, through hello FrontEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="2524f-520">Mivel nincsenek végpontok nyissa meg a HTTP-forgalom, ez nem lenne továbbítása hello felhőalapú szolgáltatás, és így elérni hello kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="2524f-520">Since there are no endpoints open for HTTP traffic, this would not pass through hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="2524f-521">Ha valamilyen okból hello végpontok nyitott, hello NSG-t (Internet blokk) hello Frontend alhálózaton megakadályozza a forgalom</span><span class="sxs-lookup"><span data-stu-id="2524f-521">If hello endpoints were open for some reason, hello NSG (Block Internet) on hello Frontend subnet would block this traffic</span></span>
4. <span data-ttu-id="2524f-522">Végül hello Frontend alhálózathoz UDR útvonal minden kimenő forgalom elküldése a IIS01 toohello tűzfal hello következő ugrásként, és hello tűzfal ehhez megjelenik ez az aszimmetrikus forgalmat, és dobja el a hello kimenő válasz legalább három független réteg van így védekezés hello közötti internetes és az IIS01 keresztül a felhőalapú szolgáltatás nem engedélyezett/nem megfelelő elérésének megakadályozásához.</span><span class="sxs-lookup"><span data-stu-id="2524f-522">Finally, hello Frontend subnet UDR route would send any outbound traffic from IIS01 toohello firewall as hello next hop, and hello firewall would see this as asymmetric traffic and drop hello outbound response Thus there are at least three independent layers of defense between hello internet and IIS01 via its cloud service preventing unauthorized/inappropriate access.</span></span>

#### <a name="denied-internet-toobackend-server"></a><span data-ttu-id="2524f-523">(Megtagadva) Internet tooBackend kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2524f-523">(Denied) Internet tooBackend Server</span></span>
1. <span data-ttu-id="2524f-524">Internetes felhasználó megpróbál tooaccess keresztül hello BackEnd001.CloudApp.Net szolgáltatás AppVM01 fájlba</span><span class="sxs-lookup"><span data-stu-id="2524f-524">Internet user tries tooaccess a file on AppVM01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="2524f-525">Mivel nincsenek nyissa meg a fájlmegosztás nincsenek végpontok, ez nem lenne továbbítja hello felhőalapú szolgáltatás, és nem tudnák elérni hello kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="2524f-525">Since there are no endpoints open for file share, this would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="2524f-526">Ha valamilyen okból hello végpontok nyitott, hello NSG-t (Internet blokk) megakadályozza a forgalom</span><span class="sxs-lookup"><span data-stu-id="2524f-526">If hello endpoints were open for some reason, hello NSG (Block Internet) would block this traffic</span></span>
4. <span data-ttu-id="2524f-527">Végül hello UDR útvonal minden kimenő forgalom elküldése a AppVM01 toohello tűzfal hello következő ugrásként, és hello tűzfal akkor megjelenik ez az aszimmetrikus adatforgalom és a drop hello kimenő válasz legalább három független rétegek közötti védelmi van így internetes és az AppVM01 hello segítségével a felhőalapú szolgáltatás nem engedélyezett/nem megfelelő elérésének megakadályozásához.</span><span class="sxs-lookup"><span data-stu-id="2524f-527">Finally, hello UDR route would send any outbound traffic from AppVM01 toohello firewall as hello next hop, and hello firewall would see this as asymmetric traffic and drop hello outbound response Thus there are at least three independent layers of defense between hello internet and AppVM01 via its cloud service preventing unauthorized/inappropriate access.</span></span>

#### <a name="denied-frontend-server-toobackend-server"></a><span data-ttu-id="2524f-528">(Megtagadva) Előtérbeli server tooBackend kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2524f-528">(Denied) Frontend server tooBackend Server</span></span>
1. <span data-ttu-id="2524f-529">Tegyük fel, IIS01 biztonsági szempontból sérült és fut-e rosszindulatú kódot próbált tooscan hello alhálózati háttérkiszolgálókhoz.</span><span class="sxs-lookup"><span data-stu-id="2524f-529">Assume IIS01 was compromised and is running malicious code trying tooscan hello Backend subnet servers.</span></span>
2. <span data-ttu-id="2524f-530">hello Frontend alhálózathoz UDR útvonal elküldése minden kimenő forgalom IIS01 toohello tűzfaltól hello következő ugrásként.</span><span class="sxs-lookup"><span data-stu-id="2524f-530">hello Frontend subnet UDR route would send any outbound traffic from IIS01 toohello firewall as hello next hop.</span></span> <span data-ttu-id="2524f-531">Ez nem lehet megváltoztatni egy hello VM sérült.</span><span class="sxs-lookup"><span data-stu-id="2524f-531">This is not something that can be altered by hello compromised VM.</span></span>
3. <span data-ttu-id="2524f-532">hello tűzfal szeretné feldolgozni hello forgalmat, ha hello kérelmező tooAppVM01 vagy toohello DNS-kiszolgáló DNS-keresések, amely a forgalom potenciálisan megengednek hello tűzfal (miatt tooFW szabályok 7 és 9).</span><span class="sxs-lookup"><span data-stu-id="2524f-532">hello firewall would process hello traffic, if hello request was tooAppVM01, or toohello DNS server for DNS lookups that traffic could potentially be allowed by hello firewall (due tooFW Rules 7 and 9).</span></span> <span data-ttu-id="2524f-533">Az összes többi forgalom FW szabály 11 (az összes megtagadása) lenne blokkolja.</span><span class="sxs-lookup"><span data-stu-id="2524f-533">All other traffic would be blocked by FW Rule 11 (Deny All).</span></span>
4. <span data-ttu-id="2524f-534">Ha speciális fenyegetésészlelés engedélyezve lett hello tűzfal (a jelen dokumentum nem ismerteti, az adott hálózati berendezések advanced threat képességek hello gyártói dokumentációjában), akkor is igaz, akkor engedélyezni által hello alapvető szabályok továbbítás forgalom Ezen tárgyalt dokumentum sikerült előzhető, ha hello forgalom ismert aláírások vagy mintáról olvashat, amelyek az advanced threat szabály jelzőt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2524f-534">If advanced threat detection was enabled on hello firewall (which is not covered in this document, see hello vendor documentation for your specific network appliance advanced threat capabilities), even traffic that would be allowed by hello basic forwarding rules discussed in this document could be prevented if hello traffic contained known signatures or patterns that flag an advanced threat rule.</span></span>

#### <a name="denied-internet-dns-lookup-on-dns-server"></a><span data-ttu-id="2524f-535">(Megtagadva) A DNS-kiszolgáló internetes DNS-címkeresés</span><span class="sxs-lookup"><span data-stu-id="2524f-535">(Denied) Internet DNS lookup on DNS server</span></span>
1. <span data-ttu-id="2524f-536">Internetes felhasználó megpróbál toolookup egy belső DNS-rekordot a DNS01 BackEnd001.CloudApp.Net szolgáltatáson keresztül</span><span class="sxs-lookup"><span data-stu-id="2524f-536">Internet user tries toolookup an internal DNS record on DNS01 through BackEnd001.CloudApp.Net service</span></span> 
2. <span data-ttu-id="2524f-537">Mivel nincsenek végpontok nyissa meg a DNS-forgalmat, ez nem lenne továbbítása hello felhőalapú szolgáltatás, és így elérni hello kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="2524f-537">Since there are no endpoints open for DNS traffic, this would not pass through hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="2524f-538">Ha valamilyen okból hello végpontok nyitott, hello NSG szabály (elérésű internetes) hello Frontend alhálózaton blokkolná-e a forgalom</span><span class="sxs-lookup"><span data-stu-id="2524f-538">If hello endpoints were open for some reason, hello NSG rule (Block Internet) on hello Frontend subnet would block this traffic</span></span>
4. <span data-ttu-id="2524f-539">Végül hello Backend alhálózathoz UDR útvonal minden kimenő forgalom elküldése a DNS01 toohello tűzfal hello következő ugrásként, és hello tűzfal ehhez megjelenik ez az aszimmetrikus forgalmat, és dobja el a hello kimenő válasz legalább három független réteg van így védekezés hello közötti internetes és az DNS01 keresztül a felhőalapú szolgáltatás nem engedélyezett/nem megfelelő elérésének megakadályozásához.</span><span class="sxs-lookup"><span data-stu-id="2524f-539">Finally, hello Backend subnet UDR route would send any outbound traffic from DNS01 toohello firewall as hello next hop, and hello firewall would see this as asymmetric traffic and drop hello outbound response Thus there are at least three independent layers of defense between hello internet and DNS01 via its cloud service preventing unauthorized/inappropriate access.</span></span>

#### <a name="denied-internet-toosql-access-through-firewall"></a><span data-ttu-id="2524f-540">(Megtagadva) Tűzfalon keresztüli internetkapcsolattal tooSQL</span><span class="sxs-lookup"><span data-stu-id="2524f-540">(Denied) Internet tooSQL access through Firewall</span></span>
1. <span data-ttu-id="2524f-541">Internetes felhasználó SQL adatokat kér a SecSvc001.CloudApp.Net (Internet Facing Felhőszolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="2524f-541">Internet user requests SQL data from SecSvc001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="2524f-542">Mivel nincsenek végpontok nyissa meg az SQL, ez nem lenne továbbítja hello felhőalapú szolgáltatás, és nem tudnák elérni hello tűzfal</span><span class="sxs-lookup"><span data-stu-id="2524f-542">Since there are no endpoints open for SQL, this would not pass hello Cloud Service and wouldn’t reach hello firewall</span></span>
3. <span data-ttu-id="2524f-543">Ha valamilyen okból SQL végpontok nyitott, hello tűzfal szabály feldolgozása volna megkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="2524f-543">If SQL endpoints were open for some reason, hello firewall would begin rule processing:</span></span>
   1. <span data-ttu-id="2524f-544">Toonext szabály FW 1. szabály (FW Mgmt) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-544">FW Rule 1 (FW Mgmt) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="2524f-545">2 – 5 (RDP szabályok) FW szabályok nem teljesül, a toonext szabály áthelyezése</span><span class="sxs-lookup"><span data-stu-id="2524f-545">FW Rules 2 - 5 (RDP Rules) don’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="2524f-546">Toonext szabály nem alkalmazza a FW szabály 6 és 7 (alkalmazás szabályok), helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-546">FW Rule 6 & 7 (Application Rules) don’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="2524f-547">Toonext szabály FW szabály 8 (tooInternet) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-547">FW Rule 8 (tooInternet) doesn’t apply, move toonext rule</span></span>
   5. <span data-ttu-id="2524f-548">Nem alkalmazható FW szabály 9 (DNS), helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="2524f-548">FW Rule 9 (DNS) doesn’t apply, move toonext rule</span></span>
   6. <span data-ttu-id="2524f-549">Toonext szabály FW szabály 10 (Intra-alhálózat) nem vonatkozik, helyezze át.</span><span class="sxs-lookup"><span data-stu-id="2524f-549">FW Rule 10 (Intra-Subnet) doesn’t apply, move toonext rule</span></span>
   7. <span data-ttu-id="2524f-550">FW szabály 11 (az összes megtagadása) alkalmazza, a forgalom letiltott, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="2524f-550">FW Rule 11 (Deny All) does apply, traffic is blocked, stop rule processing</span></span>

## <a name="references"></a><span data-ttu-id="2524f-551">Referencia</span><span class="sxs-lookup"><span data-stu-id="2524f-551">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="2524f-552">Fő parancsfájlt és a hálózati konfiguráció</span><span class="sxs-lookup"><span data-stu-id="2524f-552">Main Script and Network Config</span></span>
<span data-ttu-id="2524f-553">Hello teljes parancsfájl menthető egy olyan PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="2524f-553">Save hello Full Script in a PowerShell script file.</span></span> <span data-ttu-id="2524f-554">Hálózati konfiguráció hello "NetworkConf2.xml" fájlba mentése.</span><span class="sxs-lookup"><span data-stu-id="2524f-554">Save hello Network Config into a file named “NetworkConf2.xml”.</span></span>
<span data-ttu-id="2524f-555">Szükség szerint módosítsa a hello felhasználó által definiált változókat.</span><span class="sxs-lookup"><span data-stu-id="2524f-555">Modify hello user defined variables as needed.</span></span> <span data-ttu-id="2524f-556">Hello parancsfájl futtatása, majd kövesse a hello tűzfal szabály telepítő utasítás fenti.</span><span class="sxs-lookup"><span data-stu-id="2524f-556">Run hello script, then follow hello Firewall rule setup instruction above.</span></span>

#### <a name="full-script"></a><span data-ttu-id="2524f-557">Teljes körű parancsfájl</span><span class="sxs-lookup"><span data-stu-id="2524f-557">Full Script</span></span>
<span data-ttu-id="2524f-558">Fogja ezt a parancsfájlt, a felhasználó által definiált változóknak hello alapján:</span><span class="sxs-lookup"><span data-stu-id="2524f-558">This script will, based on hello user defined variables:</span></span>

1. <span data-ttu-id="2524f-559">Csatlakozás Azure-előfizetés tooan</span><span class="sxs-lookup"><span data-stu-id="2524f-559">Connect tooan Azure subscription</span></span>
2. <span data-ttu-id="2524f-560">Új tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="2524f-560">Create a new storage account</span></span>
3. <span data-ttu-id="2524f-561">Hozzon létre egy új virtuális hálózat és a hello hálózati konfigurációs fájljában definiált három alhálózatok</span><span class="sxs-lookup"><span data-stu-id="2524f-561">Create a new VNet and three subnets as defined in hello Network Config file</span></span>
4. <span data-ttu-id="2524f-562">Build öt virtuális gépek; 1 tűzfal és a 4 a windows server virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="2524f-562">Build five virtual machines; 1 firewall and 4 windows server VMs</span></span>
5. <span data-ttu-id="2524f-563">Adja meg UDR többek között:</span><span class="sxs-lookup"><span data-stu-id="2524f-563">Configure UDR including:</span></span>
   1. <span data-ttu-id="2524f-564">Két új útvonal-táblázatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="2524f-564">Creating two new route tables</span></span>
   2. <span data-ttu-id="2524f-565">Útvonalak toohello táblák hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2524f-565">Add routes toohello tables</span></span>
   3. <span data-ttu-id="2524f-566">Kötési táblák tooappropriate alhálózatok</span><span class="sxs-lookup"><span data-stu-id="2524f-566">Bind tables tooappropriate subnets</span></span>
6. <span data-ttu-id="2524f-567">Az IP-továbbítás hello NVA engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2524f-567">Enable IP Forwarding on hello NVA</span></span>
7. <span data-ttu-id="2524f-568">Adja meg, beleértve az NSG:</span><span class="sxs-lookup"><span data-stu-id="2524f-568">Configure NSG including:</span></span>
   1. <span data-ttu-id="2524f-569">Egy NSG létrehozása</span><span class="sxs-lookup"><span data-stu-id="2524f-569">Creating a NSG</span></span>
   2. <span data-ttu-id="2524f-570">Szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2524f-570">Adding a rule</span></span>
   3. <span data-ttu-id="2524f-571">Kötési hello NSG toohello megfelelő alhálózatokat</span><span class="sxs-lookup"><span data-stu-id="2524f-571">Binding hello NSG toohello appropriate subnets</span></span>

<span data-ttu-id="2524f-572">A PowerShell parancsfájl fusson helyben egy internethez csatlakoztatott PC vagy a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="2524f-572">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2524f-573">Ezt a parancsfájlt, amikor előfordulhat figyelmeztetések vagy a PowerShellben pop más tájékoztató üzeneteit.</span><span class="sxs-lookup"><span data-stu-id="2524f-573">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="2524f-574">Piros csak hibaüzenetek aggodalomra.</span><span class="sxs-lookup"><span data-stu-id="2524f-574">Only error messages in red are cause for concern.</span></span>
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on hello FrontEnd Subnet
       - Three Servers on hello BackEnd Subnet
       - IP Forwading from hello FireWall out toohello internet
       - User Defined Routing FrontEnd and BackEnd Subnets toohello NVA

      Before running script, ensure hello network configuration file is created in
      hello directory referenced by $NetworkConfigFile variable (or update the
      variable tooreflect hello path and file name of hello config file being used).

     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind hello appropriate
      layer(s) of protection. This script serves as an example of some of hello techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and hello appropriate protections needed, and then effectively
      implement those protections.

      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4

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

    # User Defined Global Variables
      # These should be changes tooreflect your subscription and services
      # Invalid options will fail in hello validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: tooensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be hello NVA.

        # VM 0 - hello Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"

        # VM 1 - hello Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 2 - hello First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - hello Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - hello DNS Server
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

      # Update Subscription Pointer tooNew Storage Account
        Write-Host "Updating Subscription Pointer tooNew Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "hello SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use" -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "hello BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'hello network config file was not found, please update hello $NetworkConfigFile variable toopoint toohello network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation varible is correct and hello netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occured, please see hello above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building hello environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
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
                # Set up all hello EndPoints we'll need once we're up and running
                # Note: All traffic goes through hello firewall, so we'll need tooset up all ports here.
                #       Also, hello firewall will be redirecting traffic tooa new IP and Port in a forwarding
                #       rule, so all of these endpoint have hello same public and local port and hello firewall
                #       will do hello mapping, NATing, and/or redirection as declared in hello firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when hello appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }

    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan

      # Create hello Route Tables
        Write-Host "Creating hello Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"

      # Add Routes tooRoute Tables
        Write-Host "Adding Routes toohello Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal

      # Assoicate hello Route Tables with hello Subnets
        Write-Host "Binding Route Tables toohello Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName

     # Enable IP Forwarding on hello Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable

    # Configure NSG
        Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

      # Build hello NSG
        Write-Host "Building hello NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rule
        Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

      # Assign hello NSG tootwo Subnets
        # hello NSG is *not* bound toohello Security Subnet. hello result
        # is that internet traffic flows only toohello Security subnet
        # since hello NSG bound toohello Frontend and Backback subnets
        # will Deny internet traffic toothose subnets.
        Write-Host "Binding hello NSG tootwo subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on hello IIS Server)
      # Install Backend resource (Run Post-Build Script on hello AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a><span data-ttu-id="2524f-575">Hálózati konfigurációs fájl</span><span class="sxs-lookup"><span data-stu-id="2524f-575">Network Config File</span></span>
<span data-ttu-id="2524f-576">Az XML-fájl mentése frissített hellyel rendelkező, és adja hozzá a hello hivatkozás toothis fájl toohello $NetworkConfigFile változó a fenti hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="2524f-576">Save this xml file with updated location and add hello link toothis file toohello $NetworkConfigFile variable in hello script above.</span></span>

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
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
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

#### <a name="sample-application-scripts"></a><span data-ttu-id="2524f-577">Alkalmazás mintaparancsfájlok</span><span class="sxs-lookup"><span data-stu-id="2524f-577">Sample Application Scripts</span></span>
<span data-ttu-id="2524f-578">Ha a tooinstall mintaalkalmazás ezzel és más DMZ példák, egy adtak meg, a következő hivatkozás hello: [minta alkalmazás-parancsfájl][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="2524f-578">If you wish tooinstall a sample application for this, and other DMZ Examples, one has been provided at hello following link: [Sample Application Script][SampleApp]</span></span>

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Kétirányú DMZ NVA, NSG és UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Tűzfalszabályok hello logikai ábrázolása"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Hozzon létre egy előtér-hálózati objektumot"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Hozzon létre egy DNS-kiszolgáló objektum"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Alapértelmezett RDP-szabály másolása"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 szabály"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Alkalmazás átirányítási ikonja"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Cél NAT ikon"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Pass ikon"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Felügyeleti tűzfalszabály"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "RDP tűzfalszabály"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Webes tűzfalszabály"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Tűzfalszabály AppVM01"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Kimenő tűzfalszabály"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "DNS tűzfalszabály"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Tűzfal Intra-VNet szabály"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Tűzfal megtagadási szabály"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Tűzfal szabály aktiválás"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
