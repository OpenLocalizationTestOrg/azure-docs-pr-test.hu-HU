---
title: "Példa – aaaDMZ egy Szegélyhálózaton tooprotect olyan alkalmazásokat hozhat létre egy tűzfal és az NSG-k |} Microsoft Docs"
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
ms.openlocfilehash: 18f116dc3897567bff14a509ae8c13f449182bfb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="example-2--build-a-dmz-tooprotect-applications-with-a-firewall-and-nsgs"></a><span data-ttu-id="025b8-103">2 – példa egy Szegélyhálózaton tooprotect olyan alkalmazásokat hozhat létre egy tűzfal és az NSG-k</span><span class="sxs-lookup"><span data-stu-id="025b8-103">Example 2 – Build a DMZ tooprotect applications with a Firewall and NSGs</span></span>
<span data-ttu-id="025b8-104">[Visszatérési toohello biztonsági határ bevált gyakorlatok lap][HOME]</span><span class="sxs-lookup"><span data-stu-id="025b8-104">[Return toohello Security Boundary Best Practices Page][HOME]</span></span>

<span data-ttu-id="025b8-105">Ebben a példában a DMZ hozzon létre egy tűzfal, négy windows Server-kiszolgálók és hálózati biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="025b8-105">This example will create a DMZ with a firewall, four windows servers, and Network Security Groups.</span></span> <span data-ttu-id="025b8-106">Azt is haladhat végig hello vonatkozó parancsok tooprovide bemutatják az egyes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="025b8-106">It will also walk through each of hello relevant commands tooprovide a deeper understanding of each step.</span></span> <span data-ttu-id="025b8-107">Szerepel továbbá egy forgalom forgatókönyv szakasz tooprovide egy részletesebb lépésenkénti hogyan forgalom halad keresztül hello szolgálhat a hello DMZ.</span><span class="sxs-lookup"><span data-stu-id="025b8-107">There is also a Traffic Scenario section tooprovide an in-depth step-by-step how traffic proceeds through hello layers of defense in hello DMZ.</span></span> <span data-ttu-id="025b8-108">Végezetül hello references szakaszában van hello teljes kód látható és utasítás toobuild a környezet tootest és a különböző forgatókönyvekben a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="025b8-108">Finally, in hello references section is hello complete code and instruction toobuild this environment tootest and experiment with various scenarios.</span></span> 

<span data-ttu-id="025b8-109">![NVA és NSG bejövő DMZ][1]</span><span class="sxs-lookup"><span data-stu-id="025b8-109">![Inbound DMZ with NVA and NSG][1]</span></span>

## <a name="environment-description"></a><span data-ttu-id="025b8-110">Környezet leírása</span><span class="sxs-lookup"><span data-stu-id="025b8-110">Environment Description</span></span>
<span data-ttu-id="025b8-111">Ebben a példában nincs olyan előfizetést, amelyet hello következő tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="025b8-111">In this example there is a subscription that contains hello following:</span></span>

* <span data-ttu-id="025b8-112">A felhőalapú szolgáltatások két: "FrontEnd001" és "BackEnd001"</span><span class="sxs-lookup"><span data-stu-id="025b8-112">Two cloud services: “FrontEnd001” and “BackEnd001”</span></span>
* <span data-ttu-id="025b8-113">A virtuális hálózati "CorpNetwork", két alhálózattal rendelkező: "Előtér" és "Háttér"</span><span class="sxs-lookup"><span data-stu-id="025b8-113">A Virtual Network “CorpNetwork”, with two subnets: “FrontEnd” and “BackEnd”</span></span>
* <span data-ttu-id="025b8-114">Egy egyetlen hálózati biztonsági csoport, amely alkalmazott tooboth alhálózatok</span><span class="sxs-lookup"><span data-stu-id="025b8-114">A single Network Security Group that is applied tooboth subnets</span></span>
* <span data-ttu-id="025b8-115">Ebben a példában a Barracuda NextGen tűzfal, a hálózati virtuális készülék toohello Frontend alhálózathoz csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="025b8-115">A network virtual appliance, in this example a Barracuda NextGen Firewall, connected toohello Frontend subnet</span></span>
* <span data-ttu-id="025b8-116">A Windows Server egy webalkalmazás-kiszolgáló ("IIS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="025b8-116">A Windows Server that represents an application web server (“IIS01”)</span></span>
* <span data-ttu-id="025b8-117">Két windows Server kiszolgálókon, amelyek megfelelnek az alkalmazás biztonsági end-kiszolgálók ("AppVM01", "AppVM02")</span><span class="sxs-lookup"><span data-stu-id="025b8-117">Two windows servers that represent application back end servers (“AppVM01”, “AppVM02”)</span></span>
* <span data-ttu-id="025b8-118">A Windows server DNS-kiszolgáló ("DNS01") jelölő</span><span class="sxs-lookup"><span data-stu-id="025b8-118">A Windows server that represents a DNS server (“DNS01”)</span></span>

> [!NOTE]
> <span data-ttu-id="025b8-119">Bár ebben a példában a Barracuda NextGen tűzfal, ebben a példában a különböző hálózati virtuális készülékek használhatnak hello számos használ.</span><span class="sxs-lookup"><span data-stu-id="025b8-119">Although this example uses a Barracuda NextGen Firewall, many of hello different Network Virtual Appliances could be used for this example.</span></span>
> 
> 

<span data-ttu-id="025b8-120">Hello references szakaszában alatt van egy PowerShell-parancsfájlt, amely a fent leírt hello környezetben a legtöbb fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="025b8-120">In hello references section below there is a PowerShell script that will build most of hello environment described above.</span></span> <span data-ttu-id="025b8-121">Épület hello virtuális gépek és virtuális hálózatok, bár hello a példaként megadott parancsfájlt, által végzett dokumentum nem ismerteti a jelen dokumentum részletesen.</span><span class="sxs-lookup"><span data-stu-id="025b8-121">Building hello VMs and Virtual Networks, although are done by hello example script, are not described in detail in this document.</span></span>

<span data-ttu-id="025b8-122">toobuild hello rendelkező környezetben:</span><span class="sxs-lookup"><span data-stu-id="025b8-122">toobuild hello environment:</span></span>

1. <span data-ttu-id="025b8-123">Hello hálózati konfigurációs XML-fájl (nevét, helyét és IP címeket toomatch megadott hello forgatókönyv frissíti) hello references szakaszában szereplő mentése</span><span class="sxs-lookup"><span data-stu-id="025b8-123">Save hello network config xml file included in hello references section (updated with names, location, and IP addresses toomatch hello given scenario)</span></span>
2. <span data-ttu-id="025b8-124">Frissítés hello felhasználói változók hello parancsfájl toomatch hello környezet hello parancsfájlban toobe futtatni (előfizetések, service nevét stb.)</span><span class="sxs-lookup"><span data-stu-id="025b8-124">Update hello user variables in hello script toomatch hello environment hello script is toobe run against (subscriptions, service names, etc)</span></span>
3. <span data-ttu-id="025b8-125">PowerShell hello parancsfájlok végrehajtását</span><span class="sxs-lookup"><span data-stu-id="025b8-125">Execute hello script in PowerShell</span></span>

<span data-ttu-id="025b8-126">**Megjegyzés:**: hello régió hello PowerShell-parancsfájl esetében meg kell egyeznie a hello hálózati konfigurációs XML-fájlban szereplő hello régióban.</span><span class="sxs-lookup"><span data-stu-id="025b8-126">**Note**: hello region signified in hello PowerShell script must match hello region signified in hello network configuration xml file.</span></span>

<span data-ttu-id="025b8-127">Miután hello parancsprogram sikeresen lefut hello utáni parancsfájl lépések tehetők:</span><span class="sxs-lookup"><span data-stu-id="025b8-127">Once hello script runs successfully hello following post-script steps may be taken:</span></span>

1. <span data-ttu-id="025b8-128">Állítson be hello tűzfalszabályokat, ezzel kapcsolatban lásd: hello című az alábbi: tűzfalszabályokat.</span><span class="sxs-lookup"><span data-stu-id="025b8-128">Set up hello firewall rules, this is covered in hello section below titled: Firewall Rules.</span></span>
2. <span data-ttu-id="025b8-129">Opcionálisan hello references szakaszában vannak két parancsfájlok tooset hello webkiszolgáló és egy egyszerű webes alkalmazás tooallow e DMZ konfiguráció tesztelése az alkalmazások kiszolgálói.</span><span class="sxs-lookup"><span data-stu-id="025b8-129">Optionally in hello references section are two scripts tooset up hello web server and app server with a simple web application tooallow testing with this DMZ configuration.</span></span>

<span data-ttu-id="025b8-130">hello következő szakasz ismerteti a legtöbb hello parancsfájlok utasítások relatív tooNetwork biztonsági csoportokat.</span><span class="sxs-lookup"><span data-stu-id="025b8-130">hello next section explains most of hello scripts statements relative tooNetwork Security Groups.</span></span>

## <a name="network-security-groups-nsg"></a><span data-ttu-id="025b8-131">Hálózati biztonsági csoportok (NSG)</span><span class="sxs-lookup"><span data-stu-id="025b8-131">Network Security Groups (NSG)</span></span>
<span data-ttu-id="025b8-132">Az ebben a példában egy NSG-csoport összeállítása és hat szabályokkal majd betölteni.</span><span class="sxs-lookup"><span data-stu-id="025b8-132">For this example, a NSG group is built and then loaded with six rules.</span></span> 

> [!TIP]
> <span data-ttu-id="025b8-133">Általánosságban véve kell először hozza létre az adott "Engedélyezés" szabályokat, és majd hello utolsó általánosabbá "Deny" szabályokat.</span><span class="sxs-lookup"><span data-stu-id="025b8-133">Generally speaking, you should create your specific “Allow” rules first and then hello more generic “Deny” rules last.</span></span> <span data-ttu-id="025b8-134">hello prioritású határozzák meg, hogy mely szabályokat értékeli ki a rendszer először.</span><span class="sxs-lookup"><span data-stu-id="025b8-134">hello assigned priority dictates which rules are evaluated first.</span></span> <span data-ttu-id="025b8-135">Forgalom található tooapply tooa meghatározott szabály, ha nincsenek további szabályok kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="025b8-135">Once traffic is found tooapply tooa specific rule, no further rules are evaluated.</span></span> <span data-ttu-id="025b8-136">NSG-szabályok alkalmazhat vagy hello bejövő vagy kimenő irányban (szempontjából hello hello alhálózati).</span><span class="sxs-lookup"><span data-stu-id="025b8-136">NSG rules can apply in either in hello inbound or outbound direction (from hello perspective of hello subnet).</span></span>
> 
> 

<span data-ttu-id="025b8-137">A következő szabályok hello deklaratív módon, a bejövő forgalom alatt beépített:</span><span class="sxs-lookup"><span data-stu-id="025b8-137">Declaratively, hello following rules are being built for inbound traffic:</span></span>

1. <span data-ttu-id="025b8-138">Belső DNS-forgalom (53-as port) engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="025b8-138">Internal DNS traffic (port 53) is allowed</span></span>
2. <span data-ttu-id="025b8-139">RDP-forgalom (3389-es port) a hello Internet tooany VM engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="025b8-139">RDP traffic (port 3389) from hello Internet tooany VM is allowed</span></span>
3. <span data-ttu-id="025b8-140">Engedélyezett HTTP-forgalom (80-as port) a hello Internet toohello NVA (tűzfal)</span><span class="sxs-lookup"><span data-stu-id="025b8-140">HTTP traffic (port 80) from hello Internet toohello NVA (firewall) is allowed</span></span>
4. <span data-ttu-id="025b8-141">Engedélyezett IIS01 tooAppVM1 minden forgalmat (minden porthoz)</span><span class="sxs-lookup"><span data-stu-id="025b8-141">Any traffic (all ports) from IIS01 tooAppVM1 is allowed</span></span>
5. <span data-ttu-id="025b8-142">Teljes virtuális hálózatot (alhálózatok mindkét) megtagadva hello Internet toohello minden forgalmat (minden porthoz)</span><span class="sxs-lookup"><span data-stu-id="025b8-142">Any traffic (all ports) from hello Internet toohello entire VNet (both subnets) is Denied</span></span>
6. <span data-ttu-id="025b8-143">Minden forgalmat (minden porthoz) hello Frontend alhálózathoz toohello Backend alhálózathoz megtagadva</span><span class="sxs-lookup"><span data-stu-id="025b8-143">Any traffic (all ports) from hello Frontend subnet toohello Backend subnet is Denied</span></span>

<span data-ttu-id="025b8-144">E szabályok kötött tooeach alhálózattal, ha a HTTP-kérelmek bejövő webkiszolgálóról hello Internet toohello, mindkét szabályok 3 (engedélyezése) és 5 (megtagadni) szeretné alkalmazni, de mivel a szabály 3 egy magasabb prioritással bír, csak azt csak akkor vonatkozik, és 5 szabály nem lesz play kerülhet.</span><span class="sxs-lookup"><span data-stu-id="025b8-144">With these rules bound tooeach subnet, if a HTTP request was inbound from hello Internet toohello web server, both rules 3 (allow) and 5 (deny) would apply, but since rule 3 has a higher priority only it would apply and rule 5 would not come into play.</span></span> <span data-ttu-id="025b8-145">Így toohello tűzfal hello HTTP-kérelem szeretné tenni.</span><span class="sxs-lookup"><span data-stu-id="025b8-145">Thus hello HTTP request would be allowed toohello firewall.</span></span> <span data-ttu-id="025b8-146">Ha ugyanaz a forgalom próbált tooreach hello DNS01 server, a szabály 5 (Megtagadás) lenne hello első tooapply és hello forgalom volna nem engedélyezett toopass toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="025b8-146">If that same traffic was trying tooreach hello DNS01 server, rule 5 (Deny) would be hello first tooapply and hello traffic would not be allowed toopass toohello server.</span></span> <span data-ttu-id="025b8-147">6 (Megtagadás) blokkok hello Frontend alhálózathoz van szó toohello Backend alhálózathoz (kivéve a engedélyezett forgalom szabályokban 1 és 4) a szabályt, ez védelmet nyújt hello háttér hálózathoz, abban az esetben, ha egy támadó biztonság sérüléseinek hello webalkalmazás hello előtér, hello támadónak korlátozott hozzáférés toohello háttérbeli "védett" hálózati (csak hello AppVM01 kiszolgálón kitett tooresources).</span><span class="sxs-lookup"><span data-stu-id="025b8-147">Rule 6 (Deny) blocks hello Frontend subnet from talking toohello Backend subnet (except for allowed traffic in rules 1 and 4), this protects hello Backend network in case an attacker compromises hello web application on hello Frontend, hello attacker would have limited access toohello Backend “protected” network (only tooresources exposed on hello AppVM01 server).</span></span>

<span data-ttu-id="025b8-148">Nincs olyan alapértelmezett kimenő szabály, amely lehetővé teszi, hogy a toohello kimenő forgalom internet.</span><span class="sxs-lookup"><span data-stu-id="025b8-148">There is a default outbound rule that allows traffic out toohello internet.</span></span> <span data-ttu-id="025b8-149">Ebben a példában a Microsoft most átengedi a kimenő forgalmat, és nem módosítja az egyetlen kimenő szabályok.</span><span class="sxs-lookup"><span data-stu-id="025b8-149">For this example, we’re allowing outbound traffic and not modifying any outbound rules.</span></span> <span data-ttu-id="025b8-150">toolock le mindkét irányú forgalmát, felhasználói definiált útválasztás szükséges, a rendszer írja le egy másik példa a hello található [fő biztonsági határ dokumentum][HOME].</span><span class="sxs-lookup"><span data-stu-id="025b8-150">toolock down traffic in both directions, User Defined Routing is required, this is explored in a different example that can found in hello [main security boundary document][HOME].</span></span>

<span data-ttu-id="025b8-151">a fenti hello tárgyalt NSG-szabályok nagyon hasonló toohello NSG-szabályok a [1 -. példa az NSG-ket egy egyszerű DMZ Build][Example1].</span><span class="sxs-lookup"><span data-stu-id="025b8-151">hello above discussed NSG rules are very similar toohello NSG rules in [Example 1 - Build a Simple DMZ with NSGs][Example1].</span></span> <span data-ttu-id="025b8-152">Tekintse át az egyes NSG-szabályokat és az attribútumok részletes tekintse meg a dokumentum az NSG leírás hello.</span><span class="sxs-lookup"><span data-stu-id="025b8-152">Please review hello NSG Description in that document for a detailed look at each NSG rule and it's attributes.</span></span>

## <a name="firewall-rules"></a><span data-ttu-id="025b8-153">Tűzfalszabályok</span><span class="sxs-lookup"><span data-stu-id="025b8-153">Firewall Rules</span></span>
<span data-ttu-id="025b8-154">Egy olyan kezelőügyfelet toobe PC toomanage hello tűzfal telepítve kell, és a szükséges hello-konfigurációk létrehozása.</span><span class="sxs-lookup"><span data-stu-id="025b8-154">A management client will need toobe installed on a PC toomanage hello firewall and create hello configurations needed.</span></span> <span data-ttu-id="025b8-155">Hogyan toomanage hello eszköz hello dokumentáció a tűzfal (vagy más NVA) szállító talál.</span><span class="sxs-lookup"><span data-stu-id="025b8-155">See hello documentation from your firewall (or other NVA) vendor on how toomanage hello device.</span></span> <span data-ttu-id="025b8-156">Ez a szakasz többi hello hello konfigurációs hello tűzfal, hello szállító felügyeleti ügyfél (azaz nem hello Azure-portál vagy PowerShell) segítségével ismerteti.</span><span class="sxs-lookup"><span data-stu-id="025b8-156">hello remainder of this section will describe hello configuration of hello firewall itself, through hello vendors management client (i.e. not hello Azure portal or PowerShell).</span></span>

<span data-ttu-id="025b8-157">A letölthető ügyfél és a kapcsolódó toohello ebben a példában használt Barracuda utasítások itt találhatók: [Barracuda NG rendszergazda](https://techlib.barracuda.com/NG61/NGAdmin)</span><span class="sxs-lookup"><span data-stu-id="025b8-157">Instructions for client download and connecting toohello Barracuda used in this example can be found here: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)</span></span>

<span data-ttu-id="025b8-158">Az hello tűzfal szabályok továbbítás kell toobe létrehozni.</span><span class="sxs-lookup"><span data-stu-id="025b8-158">On hello firewall, forwarding rules will need toobe created.</span></span> <span data-ttu-id="025b8-159">Ebben a példában csak továbbítja az internetes forgalmat az adathoz kötött toohello tűzfal és toohello webkiszolgáló, mert csak egy továbbító NAT-szabály van szükség.</span><span class="sxs-lookup"><span data-stu-id="025b8-159">Since this example only routes internet traffic in-bound toohello firewall and then toohello web server, only one forwarding NAT rule is needed.</span></span> <span data-ttu-id="025b8-160">A hello Barracuda NextGen tűzfal szerepel ebben a példában hello szabály lenne a cél NAT-szabály ("nyári időszámítás NAT") toopass a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="025b8-160">On hello Barracuda NextGen Firewall used in this example hello rule would be a Destination NAT rule (“Dst NAT”) toopass this traffic.</span></span>

<span data-ttu-id="025b8-161">toocreate hello következő szabály (vagy ellenőrizze a meglévő alapértelmezett szabályok), hello Barracuda NG felügyeleti ügyfél irányítópult-től kezdődő, keresse meg a toohello konfiguráció lapon, a hello működési konfigurációs szakaszban kattintson a Ruleset.</span><span class="sxs-lookup"><span data-stu-id="025b8-161">toocreate hello following rule (or verify existing default rules), starting from hello Barracuda NG Admin client dashboard, navigate toohello configuration tab, in hello Operational Configuration section click Ruleset.</span></span> <span data-ttu-id="025b8-162">A rács nevű, "Main szabályok" jeleníti meg a hello tűzfal aktív és inaktív szabályok meglévő hello.</span><span class="sxs-lookup"><span data-stu-id="025b8-162">A grid called, “Main Rules” will show hello existing active and deactivated rules on hello firewall.</span></span> <span data-ttu-id="025b8-163">A hello jobb felső sarkában a rácson a kisméretű, zöld "+" gombra, kattintson az új szabály toocreate (Megjegyzés: a tűzfal "zárolva" a változásokat, ha a gomb megjelölt "Lock", és nem toocreate, vagy a szabályok szerkesztése, kattintson erre a gombra kattintva túl "zárolásának feloldásához" hello szabálykészletben és szerkeszthető).</span><span class="sxs-lookup"><span data-stu-id="025b8-163">In hello upper right corner of this grid is a small, green “+” button, click this toocreate a new rule (Note: your firewall may be “locked” for changes, if you see a button marked “Lock” and you are unable toocreate or edit rules, click this button too“unlock” hello ruleset and allow editing).</span></span> <span data-ttu-id="025b8-164">Ha tooedit meglévő szabály, válassza ki, hogy a szabály, kattintson a jobb gombbal, és válassza ki a szabály szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="025b8-164">If you wish tooedit an existing rule, select that rule, right-click and select Edit Rule.</span></span>

<span data-ttu-id="025b8-165">Hozzon létre egy új szabályt, és adjon meg egy nevet, például a "WebTraffic".</span><span class="sxs-lookup"><span data-stu-id="025b8-165">Create a new rule and provide a name, such as "WebTraffic".</span></span> 

<span data-ttu-id="025b8-166">hello cél NAT-szabály ikon néz ki: ![cél NAT ikon][2]</span><span class="sxs-lookup"><span data-stu-id="025b8-166">hello Destination NAT rule icon looks like this: ![Destination NAT Icon][2]</span></span>

<span data-ttu-id="025b8-167">maga hello szabály például ehhez hasonló lenne:</span><span class="sxs-lookup"><span data-stu-id="025b8-167">hello rule itself would look something like this:</span></span>

<span data-ttu-id="025b8-168">![Tűzfalszabály][3]</span><span class="sxs-lookup"><span data-stu-id="025b8-168">![Firewall Rule][3]</span></span>

<span data-ttu-id="025b8-169">Itt bármely, hogy a találatok hello tooreach HTTP (80-as vagy 443-as HTTPS port) tett kísérlet tűzfal bejövő cím elküldi hello tűzfal "DHCP1 helyi IP-címe" illesztőfelület és átirányított toohello Web Server hello 10.0.1.5 IP-címét.</span><span class="sxs-lookup"><span data-stu-id="025b8-169">Here any inbound address that hits hello Firewall trying tooreach HTTP (port 80 or 443 for HTTPS) will be sent out hello Firewall’s “DHCP1 Local IP” interface and redirected toohello Web Server with hello IP Address of 10.0.1.5.</span></span> <span data-ttu-id="025b8-170">Mivel a hello forgalom érkezik, 80-as porton és folyamatos toohello webkiszolgáló 80-as porton nem port módosítása volt szükség.</span><span class="sxs-lookup"><span data-stu-id="025b8-170">Since hello traffic is coming in on port 80 and going toohello web server on port 80 no port change was needed.</span></span> <span data-ttu-id="025b8-171">Azonban a cél lista lehetett volna 10.0.1.5:8080 Ha a webalkalmazás-kiszolgáló figyel a port 8080-as így fordítása hello hello bejövő port 80-as porton hello tűzfal tooinbound 8080 hello webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="025b8-171">However, hello Target List could have been 10.0.1.5:8080 if our Web Server listened on port 8080 thus translating hello inbound port 80 on hello firewall tooinbound port 8080 on hello web server.</span></span>

<span data-ttu-id="025b8-172">A kapcsolódási módszert kell is kell szereplő, a cél szabály hello hello Internet, a "Dinamikus SNAT" legmegfelelőbb.</span><span class="sxs-lookup"><span data-stu-id="025b8-172">A Connection Method should also be signified, for hello Destination Rule from hello Internet, "Dynamic SNAT" is most appropriate.</span></span> 

<span data-ttu-id="025b8-173">Habár egyetlen szabály jött létre fontos, hogy helyesen van-e állítva a hozzá tartozó prioritás.</span><span class="sxs-lookup"><span data-stu-id="025b8-173">Although only one rule has been created it's important that its priority is set correctly.</span></span> <span data-ttu-id="025b8-174">Ha a szabályok hello tűzfalon hello alsó (alatt hello "BLOCKALL" szabály) a rendszer az új szabály hello rácsban soha nem érkezni fog szerepet.</span><span class="sxs-lookup"><span data-stu-id="025b8-174">If in hello grid of all rules on hello firewall this new rule is on hello bottom (below hello "BLOCKALL" rule) it will never come into play.</span></span> <span data-ttu-id="025b8-175">Győződjön meg arról, hogy webes forgalomban hello az újonnan létrehozott szabály hello BLOCKALL szabály felett van.</span><span class="sxs-lookup"><span data-stu-id="025b8-175">Ensure hello newly created rule for web traffic is above hello BLOCKALL rule.</span></span>

<span data-ttu-id="025b8-176">Hello szabály létrehozása után kell lennie toohello tűzfal leküldött és majd aktiválni, ha ez nem történik meg a hello szabály módosítása nem lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="025b8-176">Once hello rule is created, it must be pushed toohello firewall and then activated, if this is not done hello rule change will not take effect.</span></span> <span data-ttu-id="025b8-177">hello leküldéses és az aktiválási folyamat hello a következő szakaszban ismertetett.</span><span class="sxs-lookup"><span data-stu-id="025b8-177">hello push and activation process is described in hello next section.</span></span>

## <a name="rule-activation"></a><span data-ttu-id="025b8-178">A szabály aktiválás</span><span class="sxs-lookup"><span data-stu-id="025b8-178">Rule Activation</span></span>
<span data-ttu-id="025b8-179">Hello a ruleset módosítva lett tooadd Ez a szabály, hello szabálykészletben kell feltöltött toohello tűzfal- és aktiválva.</span><span class="sxs-lookup"><span data-stu-id="025b8-179">With hello ruleset modified tooadd this rule, hello ruleset must be uploaded toohello firewall and activated.</span></span>

<span data-ttu-id="025b8-180">![Tűzfal szabály aktiválás][4]</span><span class="sxs-lookup"><span data-stu-id="025b8-180">![Firewall Rule Activation][4]</span></span>

<span data-ttu-id="025b8-181">Hello felügyeleti ügyfél hello jobb felső sarkában a fürtöket gombok.</span><span class="sxs-lookup"><span data-stu-id="025b8-181">In hello upper right hand corner of hello management client are a cluster of buttons.</span></span> <span data-ttu-id="025b8-182">Hello "küldése módosítások" gomb toosend módosított hello szabályok toohello tűzfal elemre, majd kattintson a hello "Aktiválás" gombra.</span><span class="sxs-lookup"><span data-stu-id="025b8-182">Click hello “Send Changes” button toosend hello modified rules toohello firewall, then click hello “Activate” button.</span></span>

<span data-ttu-id="025b8-183">A hello tűzfal ruleset hello aktiválásával e példa környezet build befejeződött.</span><span class="sxs-lookup"><span data-stu-id="025b8-183">With hello activation of hello firewall ruleset this example environment build is complete.</span></span> <span data-ttu-id="025b8-184">Másik lehetőségként hello post build hello a szakasz lehet hivatkozások futnak a parancsfájlok tooadd egy alkalmazás toothis környezet tootest hello forgalom forgatókönyvek alatt.</span><span class="sxs-lookup"><span data-stu-id="025b8-184">Optionally, hello post build scripts in hello References section can be run tooadd an application toothis environment tootest hello below traffic scenarios.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="025b8-185">Fontos, hogy Ön nem elért hello webkiszolgáló közvetlenül kritikus toorealize.</span><span class="sxs-lookup"><span data-stu-id="025b8-185">It is critical toorealize that you will not hit hello web server directly.</span></span> <span data-ttu-id="025b8-186">Ha egy böngészőben kér FrontEnd001.CloudApp.Net egy HTTP-lap, a forgalom toohello tűzfal nem hello hello HTTP végpont (80-as port) fázisok webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="025b8-186">When a browser requests an HTTP page from FrontEnd001.CloudApp.Net, hello HTTP endpoint (port 80) passes this traffic toohello firewall not hello web server.</span></span> <span data-ttu-id="025b8-187">hello tűzfal, akkor a fenti, toohello webkiszolgáló kérő NAT toohello szabály szerint létrehozott.</span><span class="sxs-lookup"><span data-stu-id="025b8-187">hello firewall then, according toohello rule created above, NATs that request toohello Web Server.</span></span>
> 
> 

## <a name="traffic-scenarios"></a><span data-ttu-id="025b8-188">Forgalom forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="025b8-188">Traffic Scenarios</span></span>
#### <a name="allowed-web-tooweb-server-through-firewall"></a><span data-ttu-id="025b8-189">(Engedélyezett) Webes tooWeb Server tűzfalon keresztül</span><span class="sxs-lookup"><span data-stu-id="025b8-189">(Allowed) Web tooWeb Server through Firewall</span></span>
1. <span data-ttu-id="025b8-190">Internetes felhasználói kérelmek HTTP oldal FrontEnd001.CloudApp.Net (Internet Facing Felhőszolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="025b8-190">Internet user requests HTTP page from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="025b8-191">Felhőalapú szolgáltatás fázisok forgalmat a 80-as port toofirewall helyi illesztőfelületen 10.0.1.4:80 a nyitott végpontok keresztül</span><span class="sxs-lookup"><span data-stu-id="025b8-191">Cloud service passes traffic through open endpoint on port 80 toofirewall local interface on 10.0.1.4:80</span></span>
3. <span data-ttu-id="025b8-192">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="025b8-192">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="025b8-193">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-193">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="025b8-194">NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-194">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="025b8-195">NSG 3. szabály (Internet tooFirewall) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="025b8-195">NSG Rule 3 (Internet tooFirewall) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="025b8-196">Forgalom találatok hello tűzfal (10.0.1.4) belső IP-címe</span><span class="sxs-lookup"><span data-stu-id="025b8-196">Traffic hits internal IP address of hello firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="025b8-197">Tűzfal továbbítási szabály tekintse meg a 80-as portot a forgalom, átirányítja azt toohello webkiszolgáló IIS01</span><span class="sxs-lookup"><span data-stu-id="025b8-197">Firewall forwarding rule see this is port 80 traffic, redirects it toohello web server IIS01</span></span>
6. <span data-ttu-id="025b8-198">IIS01 a webes forgalom figyeli, ezt a kérelmet kap, és elindítja a hello kérés feldolgozása</span><span class="sxs-lookup"><span data-stu-id="025b8-198">IIS01 is listening for web traffic, receives this request and starts processing hello request</span></span>
7. <span data-ttu-id="025b8-199">IIS01 hello SQL Server a AppVM01 adatokat kéri</span><span class="sxs-lookup"><span data-stu-id="025b8-199">IIS01 asks hello SQL Server on AppVM01 for information</span></span>
8. <span data-ttu-id="025b8-200">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="025b8-200">No outbound rules on Frontend subnet, traffic is allowed</span></span>
9. <span data-ttu-id="025b8-201">hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="025b8-201">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="025b8-202">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-202">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="025b8-203">NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-203">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="025b8-204">NSG 3. szabály (Internet tooFirewall) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-204">NSG Rule 3 (Internet tooFirewall) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="025b8-205">NSG 4. szabály (IIS01 tooAppVM01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="025b8-205">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
10. <span data-ttu-id="025b8-206">AppVM01 hello SQL-lekérdezést kap, és válaszolhat</span><span class="sxs-lookup"><span data-stu-id="025b8-206">AppVM01 receives hello SQL Query and responds</span></span>
11. <span data-ttu-id="025b8-207">Mivel nincsenek engedélyezve van-e a hello Backend alhálózathoz hello válasz nem kimenő szabályok</span><span class="sxs-lookup"><span data-stu-id="025b8-207">Since there are no outbound rules on hello Backend subnet hello response is allowed</span></span>
12. <span data-ttu-id="025b8-208">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="025b8-208">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="025b8-209">Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-209">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="025b8-210">hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="025b8-210">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
13. <span data-ttu-id="025b8-211">hello IIS server hello SQL-válasz fogadása és hello HTTP-válasz befejeződik, és elküldi a toohello kérelmező</span><span class="sxs-lookup"><span data-stu-id="025b8-211">hello IIS server receives hello SQL response and completes hello HTTP response and sends toohello requestor</span></span>
14. <span data-ttu-id="025b8-212">Mivel ez a NAT-munkamenet hello tűzfaltól, hello válasz (kezdetben) célja a hello tűzfal</span><span class="sxs-lookup"><span data-stu-id="025b8-212">Since this is a NAT session from hello firewall, hello response destination (initially) is for hello Firewall</span></span>
15. <span data-ttu-id="025b8-213">hello tűzfal hello webkiszolgáló hello választ kap, és továbbítja a hátsó toohello internetes felhasználó</span><span class="sxs-lookup"><span data-stu-id="025b8-213">hello firewall receives hello response from hello Web Server and forwards back toohello Internet User</span></span>
16. <span data-ttu-id="025b8-214">Mivel nincsenek hello Frontend alhálózathoz hello válasz kimenő szabályait nem engedélyezett, és internetes felhasználó hello kap a kért weblap hello.</span><span class="sxs-lookup"><span data-stu-id="025b8-214">Since there are no outbound rules on hello Frontend subnet hello response is allowed, and hello Internet User receives hello web page requested.</span></span>

#### <a name="allowed-rdp-toobackend"></a><span data-ttu-id="025b8-215">(Engedélyezett) RDP tooBackend</span><span class="sxs-lookup"><span data-stu-id="025b8-215">(Allowed) RDP tooBackend</span></span>
1. <span data-ttu-id="025b8-216">Server Admin interneten kérelmek RDP munkamenet tooAppVM01 BackEnd001.CloudApp.Net:xxxxx, ahol xxxxx az RDP tooAppVM01 véletlenszerűen hello portszámát a (hozzárendelt hello port található hello Azure portál vagy a PowerShell segítségével)</span><span class="sxs-lookup"><span data-stu-id="025b8-216">Server Admin on internet requests RDP session tooAppVM01 on BackEnd001.CloudApp.Net:xxxxx where xxxxx is hello randomly assigned port number for RDP tooAppVM01 (hello assigned port can be found on hello Azure Portal or via PowerShell)</span></span>
2. <span data-ttu-id="025b8-217">Hello tűzfal csak figyel a következőn: hello FrontEnd001.CloudApp.Net cím, mert nem részt vesz a forgalom áramlását az</span><span class="sxs-lookup"><span data-stu-id="025b8-217">Since hello Firewall is only listening on hello FrontEnd001.CloudApp.Net address, it is not involved with this traffic flow</span></span>
3. <span data-ttu-id="025b8-218">Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="025b8-218">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="025b8-219">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-219">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="025b8-220">NSG szabály 2 (RDP) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="025b8-220">NSG Rule 2 (RDP) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="025b8-221">A nem kimenő szabályokat alapértelmezett szabályokat alkalmazni, és a forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="025b8-221">With no outbound rules, default rules apply and return traffic is allowed</span></span>
5. <span data-ttu-id="025b8-222">RDP-munkamenetbe engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="025b8-222">RDP session is enabled</span></span>
6. <span data-ttu-id="025b8-223">Felhasználói nevének jelszavának megadását kéri az AppVM01</span><span class="sxs-lookup"><span data-stu-id="025b8-223">AppVM01 prompts for user name password</span></span>

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a><span data-ttu-id="025b8-224">(Engedélyezett) Web Server DNS-címkeresés DNS-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="025b8-224">(Allowed) Web Server DNS lookup on DNS server</span></span>
1. <span data-ttu-id="025b8-225">Webalkalmazás-kiszolgáló, IIS01, kell egy adatcsatorna www.data.gov, azonban kell tooresolve hello cím.</span><span class="sxs-lookup"><span data-stu-id="025b8-225">Web Server, IIS01, needs a data feed at www.data.gov, but needs tooresolve hello address.</span></span>
2. <span data-ttu-id="025b8-226">hello tartozó fürthálózat-konfiguráció hello hálózatok listája DNS01 (a hello Backend alhálózathoz 10.0.2.4) hello elsődleges DNS-kiszolgálóként, IIS01 küldi hello DNS kérelem tooDNS01</span><span class="sxs-lookup"><span data-stu-id="025b8-226">hello network configuration for hello VNet lists DNS01 (10.0.2.4 on hello Backend subnet) as hello primary DNS server, IIS01 sends hello DNS request tooDNS01</span></span>
3. <span data-ttu-id="025b8-227">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="025b8-227">No outbound rules on Frontend subnet, traffic is allowed</span></span>
4. <span data-ttu-id="025b8-228">Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="025b8-228">Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="025b8-229">NSG szabály 1 (DNS) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="025b8-229">NSG Rule 1 (DNS) does apply, traffic is allowed, stop rule processing</span></span>
5. <span data-ttu-id="025b8-230">DNS-kiszolgáló hello kérést kap.</span><span class="sxs-lookup"><span data-stu-id="025b8-230">DNS server receives hello request</span></span>
6. <span data-ttu-id="025b8-231">DNS-kiszolgáló nem rendelkezik gyorsítótárazott hello címmel és egy legfelső szintű DNS-kiszolgáló kéri a hello internet</span><span class="sxs-lookup"><span data-stu-id="025b8-231">DNS server doesn’t have hello address cached and asks a root DNS server on hello internet</span></span>
7. <span data-ttu-id="025b8-232">Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="025b8-232">No outbound rules on Backend subnet, traffic is allowed</span></span>
8. <span data-ttu-id="025b8-233">Internetes DNS-kiszolgáló válaszol, mivel ehhez a munkamenethez belső kezdeményezték, hello válasz engedélyezett</span><span class="sxs-lookup"><span data-stu-id="025b8-233">Internet DNS server responds, since this session was initiated internally, hello response is allowed</span></span>
9. <span data-ttu-id="025b8-234">DNS-kiszolgáló gyorsítótárazza hello választ, és toohello kezdeti kérés hátsó tooIIS01 válaszol</span><span class="sxs-lookup"><span data-stu-id="025b8-234">DNS server caches hello response, and responds toohello initial request back tooIIS01</span></span>
10. <span data-ttu-id="025b8-235">Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="025b8-235">No outbound rules on Backend subnet, traffic is allowed</span></span>
11. <span data-ttu-id="025b8-236">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="025b8-236">Frontend subnet begins inbound rule processing:</span></span>
    1. <span data-ttu-id="025b8-237">Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-237">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
    2. <span data-ttu-id="025b8-238">hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="025b8-238">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed</span></span>
12. <span data-ttu-id="025b8-239">IIS01 hello választ kap DNS01</span><span class="sxs-lookup"><span data-stu-id="025b8-239">IIS01 receives hello response from DNS01</span></span>

#### <a name="allowed-web-server-access-file-on-appvm01"></a><span data-ttu-id="025b8-240">(Engedélyezett) Server-hozzáférés fájl AppVM01</span><span class="sxs-lookup"><span data-stu-id="025b8-240">(Allowed) Web Server access file on AppVM01</span></span>
1. <span data-ttu-id="025b8-241">IIS01 kér AppVM01 fájlba</span><span class="sxs-lookup"><span data-stu-id="025b8-241">IIS01 asks for a file on AppVM01</span></span>
2. <span data-ttu-id="025b8-242">Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="025b8-242">No outbound rules on Frontend subnet, traffic is allowed</span></span>
3. <span data-ttu-id="025b8-243">hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="025b8-243">hello Backend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="025b8-244">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-244">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="025b8-245">NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-245">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="025b8-246">NSG 3. szabály (Internet tooFirewall) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-246">NSG Rule 3 (Internet tooFirewall) doesn’t apply, move toonext rule</span></span>
   4. <span data-ttu-id="025b8-247">NSG 4. szabály (IIS01 tooAppVM01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="025b8-247">NSG Rule 4 (IIS01 tooAppVM01) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="025b8-248">AppVM01 hello kérelmet kap, és válaszol, a fájl (feltéve, hogy van engedélyezve)</span><span class="sxs-lookup"><span data-stu-id="025b8-248">AppVM01 receives hello request and responds with file (assuming access is authorized)</span></span>
5. <span data-ttu-id="025b8-249">Mivel nincsenek engedélyezve van-e a hello Backend alhálózathoz hello válasz nem kimenő szabályok</span><span class="sxs-lookup"><span data-stu-id="025b8-249">Since there are no outbound rules on hello Backend subnet hello response is allowed</span></span>
6. <span data-ttu-id="025b8-250">Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:</span><span class="sxs-lookup"><span data-stu-id="025b8-250">Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="025b8-251">Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-251">There is no NSG rule that applies tooInbound traffic from hello Backend subnet toohello Frontend subnet, so none of hello NSG rules apply</span></span>
   2. <span data-ttu-id="025b8-252">hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="025b8-252">hello default system rule allowing traffic between subnets would allow this traffic so hello traffic is allowed.</span></span>
7. <span data-ttu-id="025b8-253">hello IIS kiszolgáló kap hello fájl</span><span class="sxs-lookup"><span data-stu-id="025b8-253">hello IIS server receives hello file</span></span>

#### <a name="denied-web-direct-tooweb-server"></a><span data-ttu-id="025b8-254">(Megtagadva) Webalkalmazás-kiszolgáló közvetlen tooWeb</span><span class="sxs-lookup"><span data-stu-id="025b8-254">(Denied) Web direct tooWeb Server</span></span>
<span data-ttu-id="025b8-255">Hello webkiszolgáló IIS01 és hello tűzfal óta vannak a hello ugyanazt a felhőalapú szolgáltatás közös hello azonos nyilvános felé néző IP-címet.</span><span class="sxs-lookup"><span data-stu-id="025b8-255">Since hello Web Server, IIS01, and hello Firewall are in hello same Cloud Service they share hello same public facing IP address.</span></span> <span data-ttu-id="025b8-256">Így minden HTTP-forgalom lesz átirányítja toohello tűzfal.</span><span class="sxs-lookup"><span data-stu-id="025b8-256">Thus any HTTP traffic would be directed toohello firewall.</span></span> <span data-ttu-id="025b8-257">Hello kérelem sikeresen kiszolgálta lenne, amíg azt nem tud közvetlenül toohello webkiszolgáló, az átadott, célja, keresztül hello tűzfal először.</span><span class="sxs-lookup"><span data-stu-id="025b8-257">While hello request would be successfully served, it cannot go directly toohello Web Server, it passed, as designed, through hello Firewall first.</span></span> <span data-ttu-id="025b8-258">Lásd: hello első forgatókönyv ebben a szakaszban a hello adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="025b8-258">See hello first Scenario in this section for hello traffic flow.</span></span>

#### <a name="denied-web-toobackend-server"></a><span data-ttu-id="025b8-259">(Megtagadva) Webalkalmazás-kiszolgáló tooBackend</span><span class="sxs-lookup"><span data-stu-id="025b8-259">(Denied) Web tooBackend Server</span></span>
1. <span data-ttu-id="025b8-260">Internetes felhasználó megpróbál tooaccess keresztül hello BackEnd001.CloudApp.Net szolgáltatás AppVM01 fájlba</span><span class="sxs-lookup"><span data-stu-id="025b8-260">Internet user tries tooaccess a file on AppVM01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="025b8-261">Mivel nincsenek nyissa meg a fájlmegosztás nincsenek végpontok, ez nem lenne továbbítja hello felhőalapú szolgáltatás, és nem tudnák elérni hello kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="025b8-261">Since there are no endpoints open for file share, this would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="025b8-262">Ha valamilyen okból hello végpontok nyitott, NSG szabály 5 (Internet tooVNet) blokkolná-e a forgalom</span><span class="sxs-lookup"><span data-stu-id="025b8-262">If hello endpoints were open for some reason, NSG rule 5 (Internet tooVNet) would block this traffic</span></span>

#### <a name="denied-web-dns-lookup-on-dns-server"></a><span data-ttu-id="025b8-263">(Megtagadva) Webes DNS-címkeresés DNS-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="025b8-263">(Denied) Web DNS lookup on DNS server</span></span>
1. <span data-ttu-id="025b8-264">Internetes felhasználó megpróbál toolookup egy belső DNS-rekordot a DNS01 keresztül hello BackEnd001.CloudApp.Net szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="025b8-264">Internet user tries toolookup an internal DNS record on DNS01 through hello BackEnd001.CloudApp.Net service</span></span>
2. <span data-ttu-id="025b8-265">Mivel nincsenek végpontok nyissa meg a DNS, ez nem lenne továbbítja hello felhőalapú szolgáltatás, és így elérni hello kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="025b8-265">Since there are no endpoints open for DNS, this would not pass hello Cloud Service and wouldn’t reach hello server</span></span>
3. <span data-ttu-id="025b8-266">Ha valamilyen okból hello végpontok nyitott, NSG szabály 5 (Internet tooVNet) blokkolná-e a forgalom (Megjegyzés: két okból nem kell alkalmazni, hogy a szabály 1 (DNS), első hello forráscím van hello internet, ez a szabály csak érvényes toohello hello mint virtuális helyi hálózat is forrás Ez az egy olyan engedélyezési szabály, akkor soha nem letiltsa a forgalom)</span><span class="sxs-lookup"><span data-stu-id="025b8-266">If hello endpoints were open for some reason, NSG rule 5 (Internet tooVNet) would block this traffic (Note: that Rule 1 (DNS) would not apply for two reasons, first hello source address is hello internet, this rule only applies toohello local VNet as hello source, also this is an Allow rule, so it would never deny traffic)</span></span>

#### <a name="denied-web-toosql-access-through-firewall"></a><span data-ttu-id="025b8-267">(Megtagadva) Webes tooSQL hozzáférés tűzfalon keresztül</span><span class="sxs-lookup"><span data-stu-id="025b8-267">(Denied) Web tooSQL access through Firewall</span></span>
1. <span data-ttu-id="025b8-268">Internetes felhasználó SQL adatokat kér a FrontEnd001.CloudApp.Net (Internet Facing Felhőszolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="025b8-268">Internet user requests SQL data from FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)</span></span>
2. <span data-ttu-id="025b8-269">Mivel nincsenek végpontok nyissa meg az SQL, ez nem lenne továbbítja hello felhőalapú szolgáltatás, és nem tudnák elérni hello tűzfal</span><span class="sxs-lookup"><span data-stu-id="025b8-269">Since there are no endpoints open for SQL, this would not pass hello Cloud Service and wouldn’t reach hello firewall</span></span>
3. <span data-ttu-id="025b8-270">Ha valamilyen okból végpontok voltak nyitva, hello Frontend alhálózathoz megkezdi a bejövő szabály feldolgozása:</span><span class="sxs-lookup"><span data-stu-id="025b8-270">If endpoints were open for some reason, hello Frontend subnet begins inbound rule processing:</span></span>
   1. <span data-ttu-id="025b8-271">NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-271">NSG Rule 1 (DNS) doesn’t apply, move toonext rule</span></span>
   2. <span data-ttu-id="025b8-272">NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály</span><span class="sxs-lookup"><span data-stu-id="025b8-272">NSG Rule 2 (RDP) doesn’t apply, move toonext rule</span></span>
   3. <span data-ttu-id="025b8-273">NSG 2. szabály (Internet tooFirewall) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása</span><span class="sxs-lookup"><span data-stu-id="025b8-273">NSG Rule 2 (Internet tooFirewall) does apply, traffic is allowed, stop rule processing</span></span>
4. <span data-ttu-id="025b8-274">Forgalom találatok hello tűzfal (10.0.1.4) belső IP-címe</span><span class="sxs-lookup"><span data-stu-id="025b8-274">Traffic hits internal IP address of hello firewall (10.0.1.4)</span></span>
5. <span data-ttu-id="025b8-275">Tűzfal nincs továbbítási szabályaik rendelkezik SQL és elhagyta hello forgalom</span><span class="sxs-lookup"><span data-stu-id="025b8-275">Firewall has no forwarding rules for SQL and drops hello traffic</span></span>

## <a name="conclusion"></a><span data-ttu-id="025b8-276">Összegzés</span><span class="sxs-lookup"><span data-stu-id="025b8-276">Conclusion</span></span>
<span data-ttu-id="025b8-277">Ez viszonylag egyszerű módja az alkalmazás egy tűzfal védi és hello háttér alhálózathoz a bejövő forgalom elkülönítésére.</span><span class="sxs-lookup"><span data-stu-id="025b8-277">This is a relatively straight forward way of protecting your application with a firewall and isolating hello back end subnet from inbound traffic.</span></span>

<span data-ttu-id="025b8-278">További példákat és a hálózati biztonsági határokat egy áttekintést talál [Itt][HOME].</span><span class="sxs-lookup"><span data-stu-id="025b8-278">More examples and an overview of network security boundaries can be found [here][HOME].</span></span>

## <a name="references"></a><span data-ttu-id="025b8-279">Referencia</span><span class="sxs-lookup"><span data-stu-id="025b8-279">References</span></span>
### <a name="main-script-and-network-config"></a><span data-ttu-id="025b8-280">Fő parancsfájlt és a hálózati konfiguráció</span><span class="sxs-lookup"><span data-stu-id="025b8-280">Main Script and Network Config</span></span>
<span data-ttu-id="025b8-281">Hello teljes parancsfájl menthető egy olyan PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="025b8-281">Save hello Full Script in a PowerShell script file.</span></span> <span data-ttu-id="025b8-282">Hálózati konfiguráció hello "NetworkConf2.xml" fájlba mentése.</span><span class="sxs-lookup"><span data-stu-id="025b8-282">Save hello Network Config into a file named “NetworkConf2.xml”.</span></span>
<span data-ttu-id="025b8-283">Szükség szerint módosítsa a hello felhasználó által definiált változókat.</span><span class="sxs-lookup"><span data-stu-id="025b8-283">Modify hello user defined variables as needed.</span></span> <span data-ttu-id="025b8-284">Hello parancsfájl futtatása, majd kövesse a hello tűzfal szabály telepítő utasítás fenti.</span><span class="sxs-lookup"><span data-stu-id="025b8-284">Run hello script, then follow hello Firewall rule setup instruction above.</span></span>

#### <a name="full-script"></a><span data-ttu-id="025b8-285">Teljes körű parancsfájl</span><span class="sxs-lookup"><span data-stu-id="025b8-285">Full Script</span></span>
<span data-ttu-id="025b8-286">Fogja ezt a parancsfájlt, a felhasználó által definiált változóknak hello alapján:</span><span class="sxs-lookup"><span data-stu-id="025b8-286">This script will, based on hello user defined variables:</span></span>

1. <span data-ttu-id="025b8-287">Csatlakozás Azure-előfizetés tooan</span><span class="sxs-lookup"><span data-stu-id="025b8-287">Connect tooan Azure subscription</span></span>
2. <span data-ttu-id="025b8-288">Új tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="025b8-288">Create a new storage account</span></span>
3. <span data-ttu-id="025b8-289">Hozzon létre egy új virtuális hálózat és két alhálózat hello hálózati konfigurációs fájlban meghatározottak szerint</span><span class="sxs-lookup"><span data-stu-id="025b8-289">Create a new VNet and two subnets as defined in hello Network Config file</span></span>
4. <span data-ttu-id="025b8-290">4 a windows server virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="025b8-290">Build 4 windows server VMs</span></span>
5. <span data-ttu-id="025b8-291">Adja meg, beleértve az NSG:</span><span class="sxs-lookup"><span data-stu-id="025b8-291">Configure NSG including:</span></span>
   * <span data-ttu-id="025b8-292">Egy NSG létrehozása</span><span class="sxs-lookup"><span data-stu-id="025b8-292">Creating a NSG</span></span>
   * <span data-ttu-id="025b8-293">Azt a szabályoknak feltöltése</span><span class="sxs-lookup"><span data-stu-id="025b8-293">Populating it with rules</span></span>
   * <span data-ttu-id="025b8-294">Kötési hello NSG toohello megfelelő alhálózatokat</span><span class="sxs-lookup"><span data-stu-id="025b8-294">Binding hello NSG toohello appropriate subnets</span></span>

<span data-ttu-id="025b8-295">A PowerShell parancsfájl fusson helyben egy internethez csatlakoztatott PC vagy a kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="025b8-295">This PowerShell script should be run locally on an internet connected PC or server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="025b8-296">Ezt a parancsfájlt, amikor előfordulhat figyelmeztetések vagy a PowerShellben pop más tájékoztató üzeneteit.</span><span class="sxs-lookup"><span data-stu-id="025b8-296">When this script is run, there may be warnings or other informational messages that pop in PowerShell.</span></span> <span data-ttu-id="025b8-297">Piros csak hibaüzenetek aggodalomra.</span><span class="sxs-lookup"><span data-stu-id="025b8-297">Only error messages in red are cause for concern.</span></span>
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
       - One server on hello FrontEnd Subnet (plus hello NVA on hello FrontEnd subnet)
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
       myFirewall - 10.0.1.4
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
        # Note: tooensure proper NSG Rule creation later in this script:
        #       - hello Web Server must be VM 1
        #       - hello AppVM1 Server must be VM 2
        #       - hello DNS server must be VM 4
        #
        #       Otherwise hello NSG rules in hello last section of this
        #       script will need toobe changed toomatch hello modified
        #       VM array numbers ($i) so hello NSG Rule IP addresses
        #       are aligned toohello associated VM IP addresses.

        # VM 0 - hello Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 1 - hello Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"

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
                # Note: Web traffic goes through hello firewall, so we'll need tooset up a HTTP endpoint.
                #       Also, hello firewall will be redirecting web traffic tooa new IP and Port in a
                #       forwarding rule, so hello HTTP endpoint here will have hello same public and local
                #       port and hello firewall will do hello NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when hello appliance is created.
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
        Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

      # Build hello NSG
        Write-Host "Building hello NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rules
        Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet too$($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) too$($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
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


#### <a name="network-config-file"></a><span data-ttu-id="025b8-298">Hálózati konfigurációs fájl</span><span class="sxs-lookup"><span data-stu-id="025b8-298">Network Config File</span></span>
<span data-ttu-id="025b8-299">Az XML-fájl mentése frissített hellyel rendelkező, és adja hozzá a hello hivatkozás toothis fájl toohello $NetworkConfigFile változó a fenti hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="025b8-299">Save this xml file with updated location and add hello link toothis file toohello $NetworkConfigFile variable in hello script above.</span></span>

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

#### <a name="sample-application-scripts"></a><span data-ttu-id="025b8-300">Alkalmazás mintaparancsfájlok</span><span class="sxs-lookup"><span data-stu-id="025b8-300">Sample Application Scripts</span></span>
<span data-ttu-id="025b8-301">Ha a tooinstall mintaalkalmazás ezzel és más DMZ példák, egy adtak meg, a következő hivatkozás hello: [minta alkalmazás-parancsfájl][SampleApp]</span><span class="sxs-lookup"><span data-stu-id="025b8-301">If you wish tooinstall a sample application for this, and other DMZ Examples, one has been provided at hello following link: [Sample Application Script][SampleApp]</span></span>

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Az NSG bejövő DMZ"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Cél NAT ikon"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Tűzfalszabály"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Tűzfal szabály aktiválás"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
