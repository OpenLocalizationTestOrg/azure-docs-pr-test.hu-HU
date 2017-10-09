---
title: "Csatlakozás a helyi hálózati tooan Azure-beli virtuális hálózat: telephelyek közötti VPN: portál |} Microsoft Docs"
description: "Az IPsec-kapcsolat a helyszíni hálózati tooan Azure-beli virtuális hálózat felett lépéseket toocreate hello nyilvános internethez. A lépések segítségével hozhat létre egy hello portál használatával létesítmények közötti pont-pont VPN Gateway-kapcsolatot."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 6f0acbaf1bf016026cefade048a116e94686103d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-site-to-site-connection-in-hello-azure-portal"></a><span data-ttu-id="72bf5-104">Pont-pont kapcsolat létrehozása hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="72bf5-104">Create a Site-to-Site connection in hello Azure portal</span></span>

<span data-ttu-id="72bf5-105">Ez a cikk bemutatja, hogyan toouse hello Azure portál toocreate a helyszíni hálózati toohello VNet a pont-pont VPN gateway-kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="72bf5-105">This article shows you how toouse hello Azure portal toocreate a Site-to-Site VPN gateway connection from your on-premises network toohello VNet.</span></span> <span data-ttu-id="72bf5-106">a cikkben ismertetett hello alkalmazása toohello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="72bf5-106">hello steps in this article apply toohello Resource Manager deployment model.</span></span> <span data-ttu-id="72bf5-107">Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:</span><span class="sxs-lookup"><span data-stu-id="72bf5-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="72bf5-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="72bf5-108">Azure portal</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="72bf5-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="72bf5-109">PowerShell</span></span>](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [<span data-ttu-id="72bf5-110">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="72bf5-110">CLI</span></span>](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [<span data-ttu-id="72bf5-111">(Klasszikus) Azure Portal</span><span class="sxs-lookup"><span data-stu-id="72bf5-111">Azure portal (classic)</span></span>](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

<span data-ttu-id="72bf5-112">Pont-pont VPN gateway-kapcsolattal használt tooconnect a helyszíni hálózati tooan Azure-beli virtuális hálózat (IKEv1 vagy IKEv2) IPsec/IKE VPN-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="72bf5-112">A Site-to-Site VPN gateway connection is used tooconnect your on-premises network tooan Azure virtual network over an IPsec/IKE (IKEv1 or IKEv2) VPN tunnel.</span></span> <span data-ttu-id="72bf5-113">Ilyen típusú kapcsolat egy VPN található helyszíni Eszközkezelési, amely rendelkezik egy külsőleg irányuló nyilvános IP-cím tooit igényel.</span><span class="sxs-lookup"><span data-stu-id="72bf5-113">This type of connection requires a VPN device located on-premises that has an externally facing public IP address assigned tooit.</span></span> <span data-ttu-id="72bf5-114">További információk a VPN-átjárókról: [Információk a VPN Gatewayről](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="72bf5-114">For more information about VPN gateways, see [About VPN gateway](vpn-gateway-about-vpngateways.md).</span></span>

![Helyek közötti VPN Gateway létesítmények közötti kapcsolathoz – diagram](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a><span data-ttu-id="72bf5-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="72bf5-116">Before you begin</span></span>

<span data-ttu-id="72bf5-117">Győződjön meg arról, hogy teljesül-e az hello feltételeket a konfigurációs megkezdése előtt a következő:</span><span class="sxs-lookup"><span data-stu-id="72bf5-117">Verify that you have met hello following criteria before beginning your configuration:</span></span>

* <span data-ttu-id="72bf5-118">Győződjön meg arról, hogy egy kompatibilis VPN-eszköz és a rendszer képes tooconfigure azt.</span><span class="sxs-lookup"><span data-stu-id="72bf5-118">Make sure you have a compatible VPN device and someone who is able tooconfigure it.</span></span> <span data-ttu-id="72bf5-119">További információk a kompatibilis VPN-eszközökről és az eszközkonfigurációról: [Tudnivalók a VPN-eszközökről](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="72bf5-119">For more information about compatible VPN devices and device configuration, see [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span>
* <span data-ttu-id="72bf5-120">Győződjön meg arról, hogy rendelkezik egy kifelé irányuló, nyilvános IPv4-címmel a VPN-eszköz számára.</span><span class="sxs-lookup"><span data-stu-id="72bf5-120">Verify that you have an externally facing public IPv4 address for your VPN device.</span></span> <span data-ttu-id="72bf5-121">Ez az IP-cím nem lehet NAT mögötti.</span><span class="sxs-lookup"><span data-stu-id="72bf5-121">This IP address cannot be located behind a NAT.</span></span>
* <span data-ttu-id="72bf5-122">Ha nem ismeri a hello IP-címtartományok található, a helyszíni hálózati konfiguráció, a szükséges toocoordinate ezeket az információkat is tartalmazza, aki.</span><span class="sxs-lookup"><span data-stu-id="72bf5-122">If you are unfamiliar with hello IP address ranges located in your on-premises network configuration, you need toocoordinate with someone who can provide those details for you.</span></span> <span data-ttu-id="72bf5-123">Ez a konfiguráció létrehozásakor meg kell adnia hello IP-címtartomány címelőtagokat, hogy Azure tooyour helyszíni hely fogja átirányítani.</span><span class="sxs-lookup"><span data-stu-id="72bf5-123">When you create this configuration, you must specify hello IP address range prefixes that Azure will route tooyour on-premises location.</span></span> <span data-ttu-id="72bf5-124">A helyszíni hálózat alhálózatainak hello egyike is lap tooconnect a kívánt hello virtuális hálózati alhálózatokon keresztül.</span><span class="sxs-lookup"><span data-stu-id="72bf5-124">None of hello subnets of your on-premises network can over lap with hello virtual network subnets that you want tooconnect to.</span></span> 

### <span data-ttu-id="72bf5-125"><a name="values"></a>Példaértékek</span><span class="sxs-lookup"><span data-stu-id="72bf5-125"><a name="values"></a>Example values</span></span>

<span data-ttu-id="72bf5-126">a cikkben szereplő példák hello hello a következő értékeket használja.</span><span class="sxs-lookup"><span data-stu-id="72bf5-126">hello examples in this article use hello following values.</span></span> <span data-ttu-id="72bf5-127">Ezen értékek toocreate egy tesztkörnyezetben használhatja, vagy tekintse meg a toothem toobetter hello jelen cikk példái a megismeréséhez.</span><span class="sxs-lookup"><span data-stu-id="72bf5-127">You can use these values toocreate a test environment, or refer toothem toobetter understand hello examples in this article.</span></span>

* <span data-ttu-id="72bf5-128">**Virtuális hálózat neve:** TestVNet1</span><span class="sxs-lookup"><span data-stu-id="72bf5-128">**VNet Name:** TestVNet1</span></span>
* <span data-ttu-id="72bf5-129">**Címtér:**</span><span class="sxs-lookup"><span data-stu-id="72bf5-129">**Address Space:**</span></span> 
  * <span data-ttu-id="72bf5-130">10.11.0.0/16</span><span class="sxs-lookup"><span data-stu-id="72bf5-130">10.11.0.0/16</span></span>
  * <span data-ttu-id="72bf5-131">10.12.0.0/16 (nem kötelező ehhez a gyakorlathoz)</span><span class="sxs-lookup"><span data-stu-id="72bf5-131">10.12.0.0/16 (optional for this exercise)</span></span>
* <span data-ttu-id="72bf5-132">**Alhálózatok:**</span><span class="sxs-lookup"><span data-stu-id="72bf5-132">**Subnets:**</span></span>
  * <span data-ttu-id="72bf5-133">Előtér: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="72bf5-133">FrontEnd: 10.11.0.0/24</span></span>
  * <span data-ttu-id="72bf5-134">Háttér: 10.12.0.0/24 (nem kötelező ehhez a gyakorlathoz)</span><span class="sxs-lookup"><span data-stu-id="72bf5-134">BackEnd: 10.12.0.0/24 (optional for this exercise)</span></span>
* <span data-ttu-id="72bf5-135">**Átjáróalhálózat:** 10.11.255.0/27</span><span class="sxs-lookup"><span data-stu-id="72bf5-135">**GatewaySubnet:** 10.11.255.0/27</span></span>
* <span data-ttu-id="72bf5-136">**Erőforráscsoport:** TestRG1</span><span class="sxs-lookup"><span data-stu-id="72bf5-136">**Resource Group:** TestRG1</span></span>
* <span data-ttu-id="72bf5-137">**Hely:** az USA keleti régiója</span><span class="sxs-lookup"><span data-stu-id="72bf5-137">**Location:** East US</span></span>
* <span data-ttu-id="72bf5-138">**DNS-kiszolgáló:** Nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="72bf5-138">**DNS Server:** Optional.</span></span> <span data-ttu-id="72bf5-139">a DNS-kiszolgáló hello IP-címe.</span><span class="sxs-lookup"><span data-stu-id="72bf5-139">hello IP address of your DNS server.</span></span>
* <span data-ttu-id="72bf5-140">**Virtuális hálózati átjáró neve:** VNet1GW</span><span class="sxs-lookup"><span data-stu-id="72bf5-140">**Virtual Network Gateway Name:** VNet1GW</span></span>
* <span data-ttu-id="72bf5-141">**Nyilvános IP-cím:** VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="72bf5-141">**Public IP:** VNet1GWIP</span></span>
* <span data-ttu-id="72bf5-142">**VPN típusa:** útvonalalapú</span><span class="sxs-lookup"><span data-stu-id="72bf5-142">**VPN Type:** Route-based</span></span>
* <span data-ttu-id="72bf5-143">**Kapcsolat típusa:** helyek közötti kapcsolat (IPsec)</span><span class="sxs-lookup"><span data-stu-id="72bf5-143">**Connection Type:** Site-to-site (IPsec)</span></span>
* <span data-ttu-id="72bf5-144">**Átjáró típusa:** VPN</span><span class="sxs-lookup"><span data-stu-id="72bf5-144">**Gateway Type:** VPN</span></span>
* <span data-ttu-id="72bf5-145">**Helyi hálózati átjáró neve:** Site2</span><span class="sxs-lookup"><span data-stu-id="72bf5-145">**Local Network Gateway Name:** Site2</span></span>
* <span data-ttu-id="72bf5-146">**Kapcsolat neve:** VNet1toSite2</span><span class="sxs-lookup"><span data-stu-id="72bf5-146">**Connection Name:** VNet1toSite2</span></span>

## <span data-ttu-id="72bf5-147"><a name="CreatVNet"></a>1. Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="72bf5-147"><a name="CreatVNet"></a>1. Create a virtual network</span></span>

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-s2s-rm-portal-include.md)]

## <span data-ttu-id="72bf5-148"><a name="dns"></a>2. DNS-kiszolgáló megadása</span><span class="sxs-lookup"><span data-stu-id="72bf5-148"><a name="dns"></a>2. Specify a DNS server</span></span>

<span data-ttu-id="72bf5-149">DNS nincs szükség toocreate a pont-pont kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="72bf5-149">DNS is not required toocreate a Site-to-Site connection.</span></span> <span data-ttu-id="72bf5-150">Ha szeretne toohave névfeloldás telepített tooyour virtuális hálózati erőforrásokhoz, a DNS-kiszolgáló kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="72bf5-150">However, if you want toohave name resolution for resources that are deployed tooyour virtual network, you should specify a DNS server.</span></span> <span data-ttu-id="72bf5-151">Ezzel a beállítással adható meg, amelyet az toouse névfeloldás a virtuális hálózat hello DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="72bf5-151">This setting lets you specify hello DNS server that you want toouse for name resolution for this virtual network.</span></span> <span data-ttu-id="72bf5-152">A beállítás nem hoz létre új DNS-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="72bf5-152">It does not create a DNS server.</span></span> <span data-ttu-id="72bf5-153">A névfeloldással kapcsolatos további információkért tekintse meg [A virtuális gépek és szerepkörpéldányok névfeloldása](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="72bf5-153">For more information about name resolution, see [Name Resolution for VMs and role instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <span data-ttu-id="72bf5-154"><a name="gatewaysubnet"></a>3. Hozzon létre hello átjáró-alhálózatot</span><span class="sxs-lookup"><span data-stu-id="72bf5-154"><a name="gatewaysubnet"></a>3. Create hello gateway subnet</span></span>

[!INCLUDE [vpn-gateway-aboutgwsubnet](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-s2s-rm-portal-include.md)]

## <span data-ttu-id="72bf5-155"><a name="VNetGateway"></a>4. Hello VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="72bf5-155"><a name="VNetGateway"></a>4. Create hello VPN gateway</span></span>

[!INCLUDE [vpn-gateway-add-gw-s2s-rm-portal](../../includes/vpn-gateway-add-gw-s2s-rm-portal-include.md)]

## <span data-ttu-id="72bf5-156"><a name="LocalNetworkGateway"></a>5. Hello helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="72bf5-156"><a name="LocalNetworkGateway"></a>5. Create hello local network gateway</span></span>

<span data-ttu-id="72bf5-157">hello helyi hálózati átjáró általában tooyour helyszíni helyre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="72bf5-157">hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="72bf5-158">Adjon hello hely egy nevet, amellyel Azure is tekintse meg a tooit, majd adja meg hello IP-címet a hello a helyszíni VPN-eszköz toowhich létrehoz egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="72bf5-158">You give hello site a name by which Azure can refer tooit, then specify hello IP address of hello on-premises VPN device toowhich you will create a connection.</span></span> <span data-ttu-id="72bf5-159">Is meg hello IP-cím előtagokat hello VPN gateway toohello VPN-eszközön keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="72bf5-159">You also specify hello IP address prefixes that will be routed through hello VPN gateway toohello VPN device.</span></span> <span data-ttu-id="72bf5-160">megadott hello címelőtagokat a helyszíni hálózaton található hello előtagok.</span><span class="sxs-lookup"><span data-stu-id="72bf5-160">hello address prefixes you specify are hello prefixes located on your on-premises network.</span></span> <span data-ttu-id="72bf5-161">Ha a helyszíni hálózati módosítások vagy toochange hello nyilvános IP-cím szükséges hello VPN-eszközön, később könnyen frissítheti hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="72bf5-161">If your on-premises network changes or you need toochange hello public IP address for hello VPN device, you can easily update hello values later.</span></span>

[!INCLUDE [Add local network gateway](../../includes/vpn-gateway-add-lng-s2s-rm-portal-include.md)]

## <span data-ttu-id="72bf5-162"><a name="VPNDevice"></a>6. VPN-eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="72bf5-162"><a name="VPNDevice"></a>6. Configure your VPN device</span></span>

<span data-ttu-id="72bf5-163">Pont-pont kapcsolatok tooan a helyi hálózaton egy VPN-eszköz szükség.</span><span class="sxs-lookup"><span data-stu-id="72bf5-163">Site-to-Site connections tooan on-premises network require a VPN device.</span></span> <span data-ttu-id="72bf5-164">Ebben a lépésben a VPN-eszköz konfigurálása következik.</span><span class="sxs-lookup"><span data-stu-id="72bf5-164">In this step, you configure your VPN device.</span></span> <span data-ttu-id="72bf5-165">A VPN-eszköz konfigurálásakor hello a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="72bf5-165">When configuring your VPN device, you need hello following:</span></span>

- <span data-ttu-id="72bf5-166">Megosztott kulcs.</span><span class="sxs-lookup"><span data-stu-id="72bf5-166">A shared key.</span></span> <span data-ttu-id="72bf5-167">Ez az azonos megosztott hello a telephelyek közötti VPN-kapcsolat létrehozásakor megadott kulcs.</span><span class="sxs-lookup"><span data-stu-id="72bf5-167">This is hello same shared key that you specify when creating your Site-to-Site VPN connection.</span></span> <span data-ttu-id="72bf5-168">A példákban alapvető megosztott kulcsot használunk.</span><span class="sxs-lookup"><span data-stu-id="72bf5-168">In our examples, we use a basic shared key.</span></span> <span data-ttu-id="72bf5-169">Azt javasoljuk, hogy létrehozhat egy összetett kulcs toouse.</span><span class="sxs-lookup"><span data-stu-id="72bf5-169">We recommend that you generate a more complex key toouse.</span></span>
- <span data-ttu-id="72bf5-170">hello a virtuális hálózati átjáró nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="72bf5-170">hello Public IP address of your virtual network gateway.</span></span> <span data-ttu-id="72bf5-171">Hello nyilvános IP-cím hello Azure-portálon, a PowerShell vagy a parancssori felület használatával tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="72bf5-171">You can view hello public IP address by using hello Azure portal, PowerShell, or CLI.</span></span> <span data-ttu-id="72bf5-172">toofind hello nyilvános IP-cím a VPN-átjáró használatával hello Azure-portálon lépjen túl**virtuális hálózati átjárók**, majd kattintson az átjárója nevére hello.</span><span class="sxs-lookup"><span data-stu-id="72bf5-172">toofind hello Public IP address of your VPN gateway using hello Azure portal, navigate too**Virtual network gateways**, then click hello name of your gateway.</span></span>

[!INCLUDE [Configure a VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <span data-ttu-id="72bf5-173"><a name="CreateConnection"></a>7. Hello VPN-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="72bf5-173"><a name="CreateConnection"></a>7. Create hello VPN connection</span></span>

<span data-ttu-id="72bf5-174">A virtuális hálózati átjáró és a helyszíni VPN-eszköz közötti pont-pont hello VPN-kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="72bf5-174">Create hello Site-to-Site VPN connection between your virtual network gateway and your on-premises VPN device.</span></span>

[!INCLUDE [Add connections](../../includes/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include.md)]

## <span data-ttu-id="72bf5-175"><a name="VerifyConnection"></a>8. Hello VPN-kapcsolat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="72bf5-175"><a name="VerifyConnection"></a>8. Verify hello VPN connection</span></span>

[!INCLUDE [Verify - Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <span data-ttu-id="72bf5-176"><a name="connectVM"></a>tooconnect tooa virtuális gép</span><span class="sxs-lookup"><span data-stu-id="72bf5-176"><a name="connectVM"></a>tooconnect tooa virtual machine</span></span>

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <span data-ttu-id="72bf5-177"><a name="reset"></a>Hogyan tooreset VPN-átjáró</span><span class="sxs-lookup"><span data-stu-id="72bf5-177"><a name="reset"></a>How tooreset a VPN gateway</span></span>

<span data-ttu-id="72bf5-178">Az Azure VPN Gateway alaphelyzetbe állítása akkor hasznos, ha egy vagy több helyek közötti VPN-alagúton elveszíti a létesítmények közötti VPN-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="72bf5-178">Resetting an Azure VPN gateway is helpful if you lose cross-premises VPN connectivity on one or more Site-to-Site VPN tunnels.</span></span> <span data-ttu-id="72bf5-179">Ebben a helyzetben a helyszíni VPN-eszközök minden megfelelően működik-e, de nem tudta tooestablish hello Azure VPN gatewayek az IPsec-alagutak.</span><span class="sxs-lookup"><span data-stu-id="72bf5-179">In this situation, your on-premises VPN devices are all working correctly, but are not able tooestablish IPsec tunnels with hello Azure VPN gateways.</span></span> <span data-ttu-id="72bf5-180">A lépéseket lásd: [VPN Gateway alaphelyzetbe állítása](vpn-gateway-resetgw-classic.md).</span><span class="sxs-lookup"><span data-stu-id="72bf5-180">For steps, see [Reset a VPN gateway](vpn-gateway-resetgw-classic.md).</span></span>

## <span data-ttu-id="72bf5-181"><a name="resize"></a>Hogyan toochange egy átjáró-Termékváltozat (átméretezési átjáró)</span><span class="sxs-lookup"><span data-stu-id="72bf5-181"><a name="resize"></a>How toochange a gateway SKU (resize a gateway)</span></span>

<span data-ttu-id="72bf5-182">Hello lépések toochange egy átjáró-Termékváltozat, a következő témakörben: [Gateway SKU-n](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="72bf5-182">For hello steps toochange a gateway SKU, see [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span>

## <a name="next-steps"></a><span data-ttu-id="72bf5-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="72bf5-183">Next steps</span></span>

* <span data-ttu-id="72bf5-184">A BGP kapcsolatos információkért lásd: hello [BGP áttekintése](vpn-gateway-bgp-overview.md) és [hogyan tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="72bf5-184">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
* <span data-ttu-id="72bf5-185">Információk a kényszerített bújtatásról: [Információk a kényszerített bújtatásról](vpn-gateway-forced-tunneling-rm.md).</span><span class="sxs-lookup"><span data-stu-id="72bf5-185">For information about Forced Tunneling, see [About Forced Tunneling](vpn-gateway-forced-tunneling-rm.md).</span></span>
* <span data-ttu-id="72bf5-186">Információk a magas rendelkezésre állású aktív-aktív kapcsolatokról: [Magas rendelkezésre állású kapcsolatok létesítmények, illetve virtuális hálózatok között](vpn-gateway-highlyavailable.md).</span><span class="sxs-lookup"><span data-stu-id="72bf5-186">For information about Highly Available Active-Active connections, see [Highly Available cross-premises and VNet-to-VNet connectivity](vpn-gateway-highlyavailable.md).</span></span>
