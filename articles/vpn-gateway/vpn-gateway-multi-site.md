---
title: "Csatlakozás a virtuális hálózati toomultiple helyek VPN-átjáró és a PowerShell használatával: klasszikus |} Microsoft Docs"
description: "Ez a cikk végigvezeti csatlakozás több helyszíni helyi helyek tooa virtuális hálózat VPN-átjáró hello klasszikus telepítési modell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-service-management
ms.assetid: b043df6e-f1e8-4a4d-8467-c06079e2c093
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: yushwang
ms.openlocfilehash: 5404b1c55ed3453b4dbc94dfd93e47c0812025f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-site-to-site-connection-tooa-vnet-with-an-existing-vpn-gateway-connection-classic"></a><span data-ttu-id="cf97b-103">Adja hozzá a pont-pont kapcsolat tooa virtuális hálózatot egy meglévő VPN-kapcsolattal átjáró (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="cf97b-103">Add a Site-to-Site connection tooa VNet with an existing VPN gateway connection (classic)</span></span>

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [<span data-ttu-id="cf97b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="cf97b-104">Azure portal</span></span>](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="cf97b-105">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="cf97b-105">PowerShell (classic)</span></span>](vpn-gateway-multi-site.md)
>
>

<span data-ttu-id="cf97b-106">Ez a cikk bemutatja, hogyan PowerShell tooadd webhelyek (közötti S2S) kapcsolatok tooa VPN-átjáró, amely rendelkezik egy létező kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="cf97b-106">This article walks you through using PowerShell tooadd Site-to-Site (S2S) connections tooa VPN gateway that has an existing connection.</span></span> <span data-ttu-id="cf97b-107">Ez a kapcsolat típus gyakran hivatkozott tooas egy "több" Helykonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="cf97b-107">This type of connection is often referred tooas a "multi-site" configuration.</span></span> <span data-ttu-id="cf97b-108">hello a cikkben ismertetett alkalmazása hello klasszikus üzembe helyezési modellel (más néven szolgáltatásfelügyelet) használatával létrehozott toovirtual hálózatok.</span><span class="sxs-lookup"><span data-stu-id="cf97b-108">hello steps in this article apply toovirtual networks created using hello classic deployment model (also known as Service Management).</span></span> <span data-ttu-id="cf97b-109">Ezeket a lépéseket ne alkalmazza a tooExpressRoute/pont-pont vizsgálatát a kísérő kapcsolat konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="cf97b-109">These steps do not apply tooExpressRoute/Site-to-Site coexisting connection configurations.</span></span>

### <a name="deployment-models-and-methods"></a><span data-ttu-id="cf97b-110">Üzemi modellek és módszerek</span><span class="sxs-lookup"><span data-stu-id="cf97b-110">Deployment models and methods</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="cf97b-111">Ebben a táblázatban, új cikket frissítjük, és további eszközök ehhez a konfigurációhoz elérhetővé válnak.</span><span class="sxs-lookup"><span data-stu-id="cf97b-111">We update this table as new articles and additional tools become available for this configuration.</span></span> <span data-ttu-id="cf97b-112">Egy cikk nem érhető el, hogy hivatkozás közvetlenül tooit ebből a táblázatból.</span><span class="sxs-lookup"><span data-stu-id="cf97b-112">When an article is available, we link directly tooit from this table.</span></span>

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a><span data-ttu-id="cf97b-113">Csatlakozás</span><span class="sxs-lookup"><span data-stu-id="cf97b-113">About connecting</span></span>

<span data-ttu-id="cf97b-114">Több helyszíni helyek tooa egyetlen virtuális hálózat is elérheti.</span><span class="sxs-lookup"><span data-stu-id="cf97b-114">You can connect multiple on-premises sites tooa single virtual network.</span></span> <span data-ttu-id="cf97b-115">Ez egy különösen vonzó hibrid felhőalapú megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cf97b-115">This is especially attractive for building hybrid cloud solutions.</span></span> <span data-ttu-id="cf97b-116">Többhelyes kapcsolat tooyour Azure virtuális hálózati átjáró létrehozása hasonló toocreating van más webhelyek kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="cf97b-116">Creating a multi-site connection tooyour Azure virtual network gateway is similar toocreating other Site-to-Site connections.</span></span> <span data-ttu-id="cf97b-117">Valójában használhatja egy meglévő Azure VPN gateway mindaddig, amíg hello átjáró dinamikus (útválasztó-alapú).</span><span class="sxs-lookup"><span data-stu-id="cf97b-117">In fact, you can use an existing Azure VPN gateway, as long as hello gateway is dynamic (route-based).</span></span>

<span data-ttu-id="cf97b-118">Ha már rendelkezik egy statikus átjáró csatlakoztatott tooyour virtuális hálózatot, hello átjáró típusa toodynamic rendelés tooaccommodate több helyen toorebuild hello virtuális hálózati anélkül módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="cf97b-118">If you already have a static gateway connected tooyour virtual network, you can change hello gateway type toodynamic without needing toorebuild hello virtual network in order tooaccommodate multi-site.</span></span> <span data-ttu-id="cf97b-119">Hello útválasztási típus módosítása, előtt győződjön meg arról, hogy a helyszíni VPN-átjáró támogatja-e az útvonalalapú VPN konfigurációival.</span><span class="sxs-lookup"><span data-stu-id="cf97b-119">Before changing hello routing type, make sure that your on-premises VPN gateway supports route-based VPN configurations.</span></span>

<span data-ttu-id="cf97b-120">![többhelyes diagram](./media/vpn-gateway-multi-site/multisite.png "többhelyes")</span><span class="sxs-lookup"><span data-stu-id="cf97b-120">![multi-site diagram](./media/vpn-gateway-multi-site/multisite.png "multi-site")</span></span>

## <a name="points-tooconsider"></a><span data-ttu-id="cf97b-121">Pontok tooconsider</span><span class="sxs-lookup"><span data-stu-id="cf97b-121">Points tooconsider</span></span>

<span data-ttu-id="cf97b-122">**Nem fog tudni toouse hello portál toomake módosítások toothis virtuális hálózat.**</span><span class="sxs-lookup"><span data-stu-id="cf97b-122">**You won't be able toouse hello portal toomake changes toothis virtual network.**</span></span> <span data-ttu-id="cf97b-123">Toomake módosítások toohello hálózati konfigurációs fájl helyett hello portál használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="cf97b-123">You need toomake changes toohello network configuration file instead of using hello portal.</span></span> <span data-ttu-id="cf97b-124">Ha módosítja a hello portálon, a többhelyes hivatkozás beállításai a virtuális hálózat lesz felülírja.</span><span class="sxs-lookup"><span data-stu-id="cf97b-124">If you make changes in hello portal, they'll overwrite your multi-site reference settings for this virtual network.</span></span>

<span data-ttu-id="cf97b-125">Úgy kell érzi, hogy kényelmes hello hálózati konfigurációs fájl használatával hello idő hello többhelyes eljárás regisztrációja befejeződött.</span><span class="sxs-lookup"><span data-stu-id="cf97b-125">You should feel comfortable using hello network configuration file by hello time you've completed hello multi-site procedure.</span></span> <span data-ttu-id="cf97b-126">Azonban ha több személy dolgozik a hálózati konfigurációt, szüksége lesz a toomake meg arról, hogy mindenki ismer ezt a korlátozást is.</span><span class="sxs-lookup"><span data-stu-id="cf97b-126">However, if you have multiple people working on your network configuration, you'll need toomake sure that everyone knows about this limitation.</span></span> <span data-ttu-id="cf97b-127">Ez nem jelenti azt, hogy hello portal egyáltalán nem használható.</span><span class="sxs-lookup"><span data-stu-id="cf97b-127">This doesn't mean that you can't use hello portal at all.</span></span> <span data-ttu-id="cf97b-128">Minden más, kivéve, hogy konfigurációs módosításokat toothis adott virtuális hálózati használhatja.</span><span class="sxs-lookup"><span data-stu-id="cf97b-128">You can use it for everything else, except making configuration changes toothis particular virtual network.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="cf97b-129">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="cf97b-129">Before you begin</span></span>

<span data-ttu-id="cf97b-130">A konfigurálás elkezdése előtt győződjön meg arról, hogy rendelkezik-e hello következő:</span><span class="sxs-lookup"><span data-stu-id="cf97b-130">Before you begin configuration, verify that you have hello following:</span></span>

* <span data-ttu-id="cf97b-131">Az egyes hardver VPN helyszíni hely.</span><span class="sxs-lookup"><span data-stu-id="cf97b-131">Compatible VPN hardware for each on-premises location.</span></span> <span data-ttu-id="cf97b-132">Ellenőrizze [kapcsolatos VPN-eszközök a virtuális hálózati kapcsolatra](vpn-gateway-about-vpn-devices.md) tooverify Ha hello eszköz, amelyet az toouse valamit, amely kompatibilis toobe ismert.</span><span class="sxs-lookup"><span data-stu-id="cf97b-132">Check [About VPN Devices for Virtual Network Connectivity](vpn-gateway-about-vpn-devices.md) tooverify if hello device that you want toouse is something that is known toobe compatible.</span></span>
* <span data-ttu-id="cf97b-133">Külsőleg irányuló nyilvános IPv4-alapú IP-címet minden egyes VPN-eszközön.</span><span class="sxs-lookup"><span data-stu-id="cf97b-133">An externally facing public IPv4 IP address for each VPN device.</span></span> <span data-ttu-id="cf97b-134">a NAT mögött hello IP-cím nem található</span><span class="sxs-lookup"><span data-stu-id="cf97b-134">hello IP address cannot be located behind a NAT.</span></span> <span data-ttu-id="cf97b-135">Ez a követelmény.</span><span class="sxs-lookup"><span data-stu-id="cf97b-135">This is requirement.</span></span>
* <span data-ttu-id="cf97b-136">Tooinstall hello legújabb verziójára hello Azure PowerShell-parancsmagok lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="cf97b-136">You'll need tooinstall hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="cf97b-137">Győződjön meg arról, továbbá toohello erőforrás-kezelő verziójában hello Service Management (SM) verzióját telepíti.</span><span class="sxs-lookup"><span data-stu-id="cf97b-137">Make sure you install hello Service Management (SM) version in addition toohello Resource Manager version.</span></span> <span data-ttu-id="cf97b-138">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) további információt.</span><span class="sxs-lookup"><span data-stu-id="cf97b-138">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information.</span></span>
* <span data-ttu-id="cf97b-139">Személynek értelemben haladó szinten konfigurálása a VPN-hardver álljanak.</span><span class="sxs-lookup"><span data-stu-id="cf97b-139">Someone who is proficient at configuring your VPN hardware.</span></span> <span data-ttu-id="cf97b-140">Összekapcsolta toohave erős megismernie az tooconfigure a VPN-eszköz vagy valakinek, aki a használata.</span><span class="sxs-lookup"><span data-stu-id="cf97b-140">You'll have toohave a strong understanding of how tooconfigure your VPN device, or work with someone who does.</span></span>
* <span data-ttu-id="cf97b-141">hello IP-címtartományok, amelyet toouse a virtuális hálózat (Ha még nem már létre).</span><span class="sxs-lookup"><span data-stu-id="cf97b-141">hello IP address ranges that you want toouse for your virtual network (if you haven't already created one).</span></span>
* <span data-ttu-id="cf97b-142">hello IP-címtartományok az egyes hello helyi hálózati helyek, amely meg fog csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="cf97b-142">hello IP address ranges for each of hello local network sites that you'll be connecting to.</span></span> <span data-ttu-id="cf97b-143">Arról, hogy hello IP-címtartományát egyes hello helyi hálózati helyek, amelyet az tooconnect toodo nem átfedés toomake lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="cf97b-143">You'll need toomake sure that hello IP address ranges for each of hello local network sites that you want tooconnect toodo not overlap.</span></span> <span data-ttu-id="cf97b-144">Ellenkező esetben hello portálon vagy REST API hello el fogják utasítani feltöltendő hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="cf97b-144">Otherwise, hello portal or hello REST API will reject hello configuration being uploaded.</span></span><br><span data-ttu-id="cf97b-145">Például ha két helyi hálózati helyek, hogy mindkettő rendelkezik hello IP-cím tartomány 10.2.3.0/24, és rendelkezik a cél címe 10.2.3.3 csomaghoz, Azure nem tudja toosend hello csomag toobecause hello címtartományok vannak kívánt hely átfedő.</span><span class="sxs-lookup"><span data-stu-id="cf97b-145">For example, if you have two local network sites that both contain hello IP address range 10.2.3.0/24 and you have a package with a destination address 10.2.3.3, Azure wouldn't know which site you want toosend hello package toobecause hello address ranges are overlapping.</span></span> <span data-ttu-id="cf97b-146">tooprevent útválasztási problémák, Azure nem engedélyezi tooupload átfedő tartományokat rendelkező konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="cf97b-146">tooprevent routing issues, Azure doesn't allow you tooupload a configuration file that has overlapping ranges.</span></span>

## <a name="1-create-a-site-to-site-vpn"></a><span data-ttu-id="cf97b-147">1. Helyek közötti VPN létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf97b-147">1. Create a Site-to-Site VPN</span></span>
<span data-ttu-id="cf97b-148">Ha már rendelkezik egy dinamikus útválasztási átjáró, pont-pont VPN nagy!</span><span class="sxs-lookup"><span data-stu-id="cf97b-148">If you already have a Site-to-Site VPN with a dynamic routing gateway, great!</span></span> <span data-ttu-id="cf97b-149">A Folytatás túl[hello virtuális hálózati konfigurációs beállítások exportálása](#export).</span><span class="sxs-lookup"><span data-stu-id="cf97b-149">You can proceed too[Export hello virtual network configuration settings](#export).</span></span> <span data-ttu-id="cf97b-150">Ha nem, a hello, a következő:</span><span class="sxs-lookup"><span data-stu-id="cf97b-150">If not, do hello following:</span></span>

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a><span data-ttu-id="cf97b-151">Ha a pont-pont virtuális hálózat már rendelkezik, de statikus (házirend-alapú) útválasztó átjáró rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="cf97b-151">If you already have a Site-to-Site virtual network, but it has a static (policy-based) routing gateway:</span></span>
1. <span data-ttu-id="cf97b-152">Módosítsa az átjáró típusa toodynamic útválasztást.</span><span class="sxs-lookup"><span data-stu-id="cf97b-152">Change your gateway type toodynamic routing.</span></span> <span data-ttu-id="cf97b-153">Többhelyes VPN dinamikus (más néven útválasztó-alapú) útválasztó átjáró szükséges.</span><span class="sxs-lookup"><span data-stu-id="cf97b-153">A multi-site VPN requires a dynamic (also known as route-based) routing gateway.</span></span> <span data-ttu-id="cf97b-154">toochange az átjáró típus, akkor lesz toofirst delete hello meglévő átjáró kell, majd hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="cf97b-154">toochange your gateway type, you'll need toofirst delete hello existing gateway, then create a new one.</span></span> <span data-ttu-id="cf97b-155">Útmutatásért lásd: [hogyan toochange hello VPN-útválasztási típus az átjáró](vpn-gateway-configure-vpn-gateway-mp.md).</span><span class="sxs-lookup"><span data-stu-id="cf97b-155">For instructions, see [How toochange hello VPN routing type for your gateway](vpn-gateway-configure-vpn-gateway-mp.md).</span></span>  
2. <span data-ttu-id="cf97b-156">Az új átjáró konfigurálása, és a VPN-alagút létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cf97b-156">Configure your new gateway and create your VPN tunnel.</span></span> <span data-ttu-id="cf97b-157">Útmutatásért lásd: [VPN-átjáró konfigurálása a klasszikus Azure portál hello](vpn-gateway-configure-vpn-gateway-mp.md).</span><span class="sxs-lookup"><span data-stu-id="cf97b-157">For instructions, see [Configure a VPN Gateway in hello Azure Classic Portal](vpn-gateway-configure-vpn-gateway-mp.md).</span></span> <span data-ttu-id="cf97b-158">Először módosítsa az átjáró típusa toodynamic útválasztást.</span><span class="sxs-lookup"><span data-stu-id="cf97b-158">First, change your gateway type toodynamic routing.</span></span>

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a><span data-ttu-id="cf97b-159">Ha még nem rendelkezik pont-pont virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="cf97b-159">If you don't have a Site-to-Site virtual network:</span></span>
1. <span data-ttu-id="cf97b-160">A pont-pont virtuális hálózat létrehozása ezeket az utasításokat: [virtuális hálózat létrehozása a klasszikus Azure portál hello pont-pont VPN-kapcsolattal](vpn-gateway-site-to-site-create.md).</span><span class="sxs-lookup"><span data-stu-id="cf97b-160">Create your Site-to-Site virtual network using these instructions: [Create a Virtual Network with a Site-to-Site VPN Connection in hello Azure Classic Portal](vpn-gateway-site-to-site-create.md).</span></span>  
2. <span data-ttu-id="cf97b-161">A jelen útmutatást dinamikus útválasztó átjáró konfigurálása: [VPN-átjáró konfigurálása](vpn-gateway-configure-vpn-gateway-mp.md).</span><span class="sxs-lookup"><span data-stu-id="cf97b-161">Configure a dynamic routing gateway using these instructions: [Configure a VPN Gateway](vpn-gateway-configure-vpn-gateway-mp.md).</span></span> <span data-ttu-id="cf97b-162">Lehet, hogy tooselect **dinamikus útválasztási** az átjáró típusának.</span><span class="sxs-lookup"><span data-stu-id="cf97b-162">Be sure tooselect **dynamic routing** for your gateway type.</span></span>

## <span data-ttu-id="cf97b-163"><a name="export"></a>2. Hello hálózati konfigurációs fájl exportálása</span><span class="sxs-lookup"><span data-stu-id="cf97b-163"><a name="export"></a>2. Export hello network configuration file</span></span>
<span data-ttu-id="cf97b-164">Az Azure-hálózat konfigurációs fájl exportálása hello a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="cf97b-164">Export your Azure network configuration file by running hello following command.</span></span> <span data-ttu-id="cf97b-165">Hello helyét hello fájl tooexport tooa máshová szükség esetén módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="cf97b-165">You can change hello location of hello file tooexport tooa different location if necessary.</span></span>

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-hello-network-configuration-file"></a><span data-ttu-id="cf97b-166">3. Nyissa meg hello hálózati konfigurációs fájl</span><span class="sxs-lookup"><span data-stu-id="cf97b-166">3. Open hello network configuration file</span></span>
<span data-ttu-id="cf97b-167">Nyissa meg a letöltött hello hálózati konfigurációs fájllal hello utolsó lépésében megadja.</span><span class="sxs-lookup"><span data-stu-id="cf97b-167">Open hello network configuration file that you downloaded in hello last step.</span></span> <span data-ttu-id="cf97b-168">Bármilyen XML-szerkesztő, amely tetszés használja.</span><span class="sxs-lookup"><span data-stu-id="cf97b-168">Use any xml editor that you like.</span></span> <span data-ttu-id="cf97b-169">hello fájl hasonló toohello következő kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="cf97b-169">hello file should look similar toohello following:</span></span>

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a><span data-ttu-id="cf97b-170">4. Több mutató hivatkozások hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cf97b-170">4. Add multiple site references</span></span>
<span data-ttu-id="cf97b-171">Amikor hozzá, vagy távolítsa el a webhely referencia jellegű információt, lesz konfigurációmódosításokat toohello ConnectionsToLocalNetwork/LocalNetworkSiteRef.</span><span class="sxs-lookup"><span data-stu-id="cf97b-171">When you add or remove site reference information, you'll make configuration changes toohello ConnectionsToLocalNetwork/LocalNetworkSiteRef.</span></span> <span data-ttu-id="cf97b-172">Azure toocreate hozzáadása egy új helyi hivatkozás elindítja egy új csatornán.</span><span class="sxs-lookup"><span data-stu-id="cf97b-172">Adding a new local site reference triggers Azure toocreate a new tunnel.</span></span> <span data-ttu-id="cf97b-173">Hello az alábbi példában a hello hálózati konfiguráció egyetlen helyen kapcsolat van.</span><span class="sxs-lookup"><span data-stu-id="cf97b-173">In hello example below, hello network configuration is for a single-site connection.</span></span> <span data-ttu-id="cf97b-174">Mentse a hello fájlt, a változtatások befejezése után.</span><span class="sxs-lookup"><span data-stu-id="cf97b-174">Save hello file once you have finished making your changes.</span></span>

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

<span data-ttu-id="cf97b-175">(a többhelyes konfiguráció létrehozása) tooadd további webhelyekre mutató hivatkozások egyszerűen adja hozzá a további "LocalNetworkSiteRef" sorok, a hello az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="cf97b-175">tooadd additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in hello example below:</span></span>

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-hello-network-configuration-file"></a><span data-ttu-id="cf97b-176">5. Importálás hello hálózati konfigurációs fájl</span><span class="sxs-lookup"><span data-stu-id="cf97b-176">5. Import hello network configuration file</span></span>
<span data-ttu-id="cf97b-177">Importálja a hello hálózati konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="cf97b-177">Import hello network configuration file.</span></span> <span data-ttu-id="cf97b-178">Amikor importálja ezt a fájlt hello módosításokkal, hello új alagutak lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="cf97b-178">When you import this file with hello changes, hello new tunnels will be added.</span></span> <span data-ttu-id="cf97b-179">hello alagutak hello dinamikus átjáró korábban létrehozott fogja használni.</span><span class="sxs-lookup"><span data-stu-id="cf97b-179">hello tunnels will use hello dynamic gateway that you created earlier.</span></span> <span data-ttu-id="cf97b-180">Használhatja a hello klasszikus portál vagy PowerShell tooimport hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="cf97b-180">You can either use hello classic portal, or PowerShell tooimport hello file.</span></span>

## <a name="6-download-keys"></a><span data-ttu-id="cf97b-181">6. Töltse le a kulcsok</span><span class="sxs-lookup"><span data-stu-id="cf97b-181">6. Download keys</span></span>
<span data-ttu-id="cf97b-182">Az új alagutak hozzáadása után használja a hello PowerShell parancsmag "Get-AzureVNetGatewayKey" tooget hello IPsec/IKE előmegosztott kulccsal minden alagúthoz.</span><span class="sxs-lookup"><span data-stu-id="cf97b-182">Once your new tunnels have been added, use hello PowerShell cmdlet 'Get-AzureVNetGatewayKey' tooget hello IPsec/IKE pre-shared keys for each tunnel.</span></span>

<span data-ttu-id="cf97b-183">Példa:</span><span class="sxs-lookup"><span data-stu-id="cf97b-183">For example:</span></span>

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

<span data-ttu-id="cf97b-184">Tetszés szerint használhatja hello *első virtuális hálózati átjáró megosztott kulcsos* REST API tooget hello előmegosztott kulccsal.</span><span class="sxs-lookup"><span data-stu-id="cf97b-184">If you prefer, you can also use hello *Get Virtual Network Gateway Shared Key* REST API tooget hello pre-shared keys.</span></span>

## <a name="7-verify-your-connections"></a><span data-ttu-id="cf97b-185">7. Kapcsolatok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="cf97b-185">7. Verify your connections</span></span>
<span data-ttu-id="cf97b-186">Ellenőrizze a hello többhelyes alagút állapotát.</span><span class="sxs-lookup"><span data-stu-id="cf97b-186">Check hello multi-site tunnel status.</span></span> <span data-ttu-id="cf97b-187">Hello kulcsok minden egyes alagúthoz a letöltés után érdemes tooverify kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="cf97b-187">After downloading hello keys for each tunnel, you'll want tooverify connections.</span></span> <span data-ttu-id="cf97b-188">Használja a "Get-AzureVnetConnection" tooget bújtatja virtuális hálózati listáját, az alábbi hello példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="cf97b-188">Use 'Get-AzureVnetConnection' tooget a list of virtual network tunnels, as shown in hello example below.</span></span> <span data-ttu-id="cf97b-189">VNet1 hello hello VNet neve.</span><span class="sxs-lookup"><span data-stu-id="cf97b-189">VNet1 is hello name of hello VNet.</span></span>

```powershell
Get-AzureVnetConnection -VNetName VNET1
```

<span data-ttu-id="cf97b-190">A példa visszatérési:</span><span class="sxs-lookup"><span data-stu-id="cf97b-190">Example return:</span></span>

```
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : hello connectivity state for hello local network site 'Site1' changed from Not Connected tooConnected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : hello connectivity state for hello local network site 'Site2' changed from Not Connected tooConnected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
```

## <a name="next-steps"></a><span data-ttu-id="cf97b-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cf97b-191">Next steps</span></span>

<span data-ttu-id="cf97b-192">További információ a VPN-átjárók, toolearn lásd [VPN-átjárók](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="cf97b-192">toolearn more about VPN Gateways, see [About VPN Gateways](vpn-gateway-about-vpngateways.md).</span></span>
