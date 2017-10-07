---
title: "Csatlakozás a helyi hálózati tooan Azure-beli virtuális hálózat: telephelyek közötti VPN: PowerShell |} Microsoft Docs"
description: "Az IPsec-kapcsolat a helyszíni hálózati tooan Azure-beli virtuális hálózat felett lépéseket toocreate hello nyilvános internethez. Ezen lépéseket követve létrehozhat egy helyek közötti VPN-átjáró kapcsolatot a PowerShell segítségével."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fcc2fda5-4493-4c15-9436-84d35adbda8e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: cb8db1dab3a5488816a7f7e8e63908a4c02f55db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a><span data-ttu-id="aee19-104">Helyek közötti VPN-kapcsolattal rendelkező virtuális hálózat létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="aee19-104">Create a VNet with a Site-to-Site VPN connection using PowerShell</span></span>

<span data-ttu-id="aee19-105">Ez a cikk bemutatja, hogyan toouse PowerShell toocreate a pont-pont VPN gateway-kapcsolatot a helyszíni hálózati toohello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="aee19-105">This article shows you how toouse PowerShell toocreate a Site-to-Site VPN gateway connection from your on-premises network toohello VNet.</span></span> <span data-ttu-id="aee19-106">a cikkben ismertetett hello alkalmazása toohello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="aee19-106">hello steps in this article apply toohello Resource Manager deployment model.</span></span> <span data-ttu-id="aee19-107">Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:</span><span class="sxs-lookup"><span data-stu-id="aee19-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="aee19-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="aee19-108">Azure portal</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="aee19-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="aee19-109">PowerShell</span></span>](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [<span data-ttu-id="aee19-110">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="aee19-110">CLI</span></span>](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [<span data-ttu-id="aee19-111">(Klasszikus) Azure Portal</span><span class="sxs-lookup"><span data-stu-id="aee19-111">Azure portal (classic)</span></span>](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [<span data-ttu-id="aee19-112">Klasszikus portál (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="aee19-112">Classic portal (classic)</span></span>](vpn-gateway-site-to-site-create.md)
> 
>


<span data-ttu-id="aee19-113">Pont-pont VPN gateway-kapcsolattal használt tooconnect a helyszíni hálózati tooan Azure-beli virtuális hálózat (IKEv1 vagy IKEv2) IPsec/IKE VPN-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="aee19-113">A Site-to-Site VPN gateway connection is used tooconnect your on-premises network tooan Azure virtual network over an IPsec/IKE (IKEv1 or IKEv2) VPN tunnel.</span></span> <span data-ttu-id="aee19-114">Ilyen típusú kapcsolat egy VPN található helyszíni Eszközkezelési, amely rendelkezik egy külsőleg irányuló nyilvános IP-cím tooit igényel.</span><span class="sxs-lookup"><span data-stu-id="aee19-114">This type of connection requires a VPN device located on-premises that has an externally facing public IP address assigned tooit.</span></span> <span data-ttu-id="aee19-115">További információk a VPN-átjárókról: [Információk a VPN Gatewayről](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="aee19-115">For more information about VPN gateways, see [About VPN gateway](vpn-gateway-about-vpngateways.md).</span></span>

![Helyek közötti VPN Gateway létesítmények közötti kapcsolathoz – diagram](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-diagram.png)

## <span data-ttu-id="aee19-117"><a name="before"></a>Előkészületek</span><span class="sxs-lookup"><span data-stu-id="aee19-117"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="aee19-118">Győződjön meg arról, hogy teljesül-e az hello feltételeket a konfigurációs megkezdése előtt a következő:</span><span class="sxs-lookup"><span data-stu-id="aee19-118">Verify that you have met hello following criteria before beginning your configuration:</span></span>

* <span data-ttu-id="aee19-119">Győződjön meg arról, hogy egy kompatibilis VPN-eszköz és a rendszer képes tooconfigure azt.</span><span class="sxs-lookup"><span data-stu-id="aee19-119">Make sure you have a compatible VPN device and someone who is able tooconfigure it.</span></span> <span data-ttu-id="aee19-120">További információk a kompatibilis VPN-eszközökről és az eszközkonfigurációról: [Tudnivalók a VPN-eszközökről](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="aee19-120">For more information about compatible VPN devices and device configuration, see [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span>
* <span data-ttu-id="aee19-121">Győződjön meg arról, hogy rendelkezik egy kifelé irányuló, nyilvános IPv4-címmel a VPN-eszköz számára.</span><span class="sxs-lookup"><span data-stu-id="aee19-121">Verify that you have an externally facing public IPv4 address for your VPN device.</span></span> <span data-ttu-id="aee19-122">Ez az IP-cím nem lehet NAT mögötti.</span><span class="sxs-lookup"><span data-stu-id="aee19-122">This IP address cannot be located behind a NAT.</span></span>
* <span data-ttu-id="aee19-123">Ha nem ismeri a hello IP-címtartományok található, a helyszíni hálózati konfiguráció, a szükséges toocoordinate ezeket az információkat is tartalmazza, aki.</span><span class="sxs-lookup"><span data-stu-id="aee19-123">If you are unfamiliar with hello IP address ranges located in your on-premises network configuration, you need toocoordinate with someone who can provide those details for you.</span></span> <span data-ttu-id="aee19-124">Ez a konfiguráció létrehozásakor meg kell adnia hello IP-címtartomány címelőtagokat, hogy Azure tooyour helyszíni hely fogja átirányítani.</span><span class="sxs-lookup"><span data-stu-id="aee19-124">When you create this configuration, you must specify hello IP address range prefixes that Azure will route tooyour on-premises location.</span></span> <span data-ttu-id="aee19-125">A helyszíni hálózat alhálózatainak hello egyike is lap tooconnect a kívánt hello virtuális hálózati alhálózatokon keresztül.</span><span class="sxs-lookup"><span data-stu-id="aee19-125">None of hello subnets of your on-premises network can over lap with hello virtual network subnets that you want tooconnect to.</span></span>
* <span data-ttu-id="aee19-126">Hello hello Azure Resource Manager PowerShell-parancsmagok legújabb verziójának telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="aee19-126">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="aee19-127">PowerShell-parancsmagok gyakran frissül, és általában szüksége lesz tooupdate a PowerShell parancsmagok tooget hello legújabb funkcióhoz.</span><span class="sxs-lookup"><span data-stu-id="aee19-127">PowerShell cmdlets are updated frequently and you will typically need tooupdate your PowerShell cmdlets tooget hello latest feature functionality.</span></span> <span data-ttu-id="aee19-128">Ha nem frissíti a PowerShell-parancsmagjai, a megadott hello értékek sikertelen lehet.</span><span class="sxs-lookup"><span data-stu-id="aee19-128">If you don't update your PowerShell cmdlets, hello values specified may fail.</span></span> <span data-ttu-id="aee19-129">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) letöltése és telepítése a PowerShell-parancsmagokkal kapcsolatos információkért.</span><span class="sxs-lookup"><span data-stu-id="aee19-129">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about downloading and installing PowerShell cmdlets.</span></span>

### <span data-ttu-id="aee19-130"><a name="example"></a>Példaértékek</span><span class="sxs-lookup"><span data-stu-id="aee19-130"><a name="example"></a>Example values</span></span>

<span data-ttu-id="aee19-131">a cikkben szereplő példák hello hello a következő értékeket használja.</span><span class="sxs-lookup"><span data-stu-id="aee19-131">hello examples in this article use hello following values.</span></span> <span data-ttu-id="aee19-132">Ezen értékek toocreate egy tesztkörnyezetben használhatja, vagy tekintse meg a toothem toobetter hello jelen cikk példái a megismeréséhez.</span><span class="sxs-lookup"><span data-stu-id="aee19-132">You can use these values toocreate a test environment, or refer toothem toobetter understand hello examples in this article.</span></span>

```
#Example values

VnetName                = TestVNet1
ResourceGroup           = TestRG1
Location                = East US 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.1.0/28 
GatewaySubnet           = 10.11.0.0/27
LocalNetworkGatewayName = Site2
LNG Public IP           = <VPN device IP address> 
Local Address Prefixes  = 10.0.0.0/24, 20.0.0.0/24
Gateway Name            = VNet1GW
PublicIP                = VNet1GWIP
Gateway IP Config       = gwipconfig1 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2

```


## <span data-ttu-id="aee19-133"><a name="Login"></a>1. Csatlakozás tooyour előfizetés</span><span class="sxs-lookup"><span data-stu-id="aee19-133"><a name="Login"></a>1. Connect tooyour subscription</span></span>

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <span data-ttu-id="aee19-134"><a name="VNet"></a>2. Virtuális hálózat és átjáró-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="aee19-134"><a name="VNet"></a>2. Create a virtual network and a gateway subnet</span></span>

<span data-ttu-id="aee19-135">Ha még nem rendelkezik virtuális hálózattal, akkor hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="aee19-135">If you don't already have a virtual network, create one.</span></span> <span data-ttu-id="aee19-136">Amikor egy virtuális hálózat létrehozásával, győződjön meg arról, hogy megadott hello címterek nem fedhetnek át olyan hello címterek, hogy rendelkezik a helyi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="aee19-136">When creating a virtual network, make sure that hello address spaces you specify don't overlap any of hello address spaces that you have on your on-premises network.</span></span>

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [No NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

### <span data-ttu-id="aee19-137"><a name="vnet"></a>toocreate egy virtuális hálózatot és egy átjáró-alhálózatot</span><span class="sxs-lookup"><span data-stu-id="aee19-137"><a name="vnet"></a>toocreate a virtual network and a gateway subnet</span></span>

<span data-ttu-id="aee19-138">Ebben a példában egy virtuális hálózatot és egy átjáróalhálózatot hozunk létre.</span><span class="sxs-lookup"><span data-stu-id="aee19-138">This example creates a virtual network and a gateway subnet.</span></span> <span data-ttu-id="aee19-139">Ha már van egy virtuális hálózatot, amelyekre szüksége van egy átjáróalhálózat megjelenítéséhez, tooadd [tooadd átjáró alhálózati tooa virtuális hálózat már létrehozott](#gatewaysubnet).</span><span class="sxs-lookup"><span data-stu-id="aee19-139">If you already have a virtual network that you need tooadd a gateway subnet to, see [tooadd a gateway subnet tooa virtual network you have already created](#gatewaysubnet).</span></span>

<span data-ttu-id="aee19-140">Hozzon létre egy erőforráscsoportot:</span><span class="sxs-lookup"><span data-stu-id="aee19-140">Create a resource group:</span></span>

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location 'East US'
```

<span data-ttu-id="aee19-141">Hozza létre a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="aee19-141">Create your virtual network.</span></span>

1. <span data-ttu-id="aee19-142">Hello változók megadása.</span><span class="sxs-lookup"><span data-stu-id="aee19-142">Set hello variables.</span></span>

  ```powershell
  $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.11.0.0/27
  $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix 10.11.1.0/28
  ```
2. <span data-ttu-id="aee19-143">Hello virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="aee19-143">Create hello VNet.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name TestVNet1 -ResourceGroupName TestRG1 `
  -Location 'East US' -AddressPrefix 10.11.0.0/16 -Subnet $subnet1, $subnet2
  ```

### <span data-ttu-id="aee19-144"><a name="gatewaysubnet"></a>átjáró alhálózati tooa virtuális hálózat már létrehozott tooadd</span><span class="sxs-lookup"><span data-stu-id="aee19-144"><a name="gatewaysubnet"></a>tooadd a gateway subnet tooa virtual network you have already created</span></span>

1. <span data-ttu-id="aee19-145">Hello változók megadása.</span><span class="sxs-lookup"><span data-stu-id="aee19-145">Set hello variables.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG1 -Name TestVet1
  ```
2. <span data-ttu-id="aee19-146">Hozzon létre hello átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="aee19-146">Create hello gateway subnet.</span></span>

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.11.0.0/27 -VirtualNetwork $vnet
  ```
3. <span data-ttu-id="aee19-147">Hello konfigurációjának beállítása</span><span class="sxs-lookup"><span data-stu-id="aee19-147">Set hello configuration.</span></span>

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

## <span data-ttu-id="aee19-148">3. <a name="localnet"></a>Hello helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="aee19-148">3. <a name="localnet"></a>Create hello local network gateway</span></span>

<span data-ttu-id="aee19-149">hello helyi hálózati átjáró általában tooyour helyszíni helyre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="aee19-149">hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="aee19-150">Adjon hello hely egy nevet, amellyel Azure is tekintse meg a tooit, majd adja meg hello IP-címet a hello a helyszíni VPN-eszköz toowhich létrehoz egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="aee19-150">You give hello site a name by which Azure can refer tooit, then specify hello IP address of hello on-premises VPN device toowhich you will create a connection.</span></span> <span data-ttu-id="aee19-151">Is meg hello IP-cím előtagokat hello VPN gateway toohello VPN-eszközön keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="aee19-151">You also specify hello IP address prefixes that will be routed through hello VPN gateway toohello VPN device.</span></span> <span data-ttu-id="aee19-152">megadott hello címelőtagokat a helyszíni hálózaton található hello előtagok.</span><span class="sxs-lookup"><span data-stu-id="aee19-152">hello address prefixes you specify are hello prefixes located on your on-premises network.</span></span> <span data-ttu-id="aee19-153">Ha módosítja a helyszíni hálózat, hello előtagok könnyen frissítheti.</span><span class="sxs-lookup"><span data-stu-id="aee19-153">If your on-premises network changes, you can easily update hello prefixes.</span></span>

<span data-ttu-id="aee19-154">A következő értékek hello használata:</span><span class="sxs-lookup"><span data-stu-id="aee19-154">Use hello following values:</span></span>

* <span data-ttu-id="aee19-155">Hello *GatewayIPAddress* hello IP-címe a helyszíni VPN-eszköz.</span><span class="sxs-lookup"><span data-stu-id="aee19-155">hello *GatewayIPAddress* is hello IP address of your on-premises VPN device.</span></span> <span data-ttu-id="aee19-156">A VPN-eszköz nem lehet NAT mögött.</span><span class="sxs-lookup"><span data-stu-id="aee19-156">Your VPN device cannot be located behind a NAT.</span></span>
* <span data-ttu-id="aee19-157">Hello *címelőtagja* van a helyszíni címtér.</span><span class="sxs-lookup"><span data-stu-id="aee19-157">hello *AddressPrefix* is your on-premises address space.</span></span>

<span data-ttu-id="aee19-158">a helyi hálózati átjáró egyetlen címelőtaggal rendelkező tooadd:</span><span class="sxs-lookup"><span data-stu-id="aee19-158">tooadd a local network gateway with a single address prefix:</span></span>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.0.0.0/24'
  ```

<span data-ttu-id="aee19-159">a helyi hálózati átjáró több címelőtagokat a tooadd:</span><span class="sxs-lookup"><span data-stu-id="aee19-159">tooadd a local network gateway with multiple address prefixes:</span></span>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')
  ```

<span data-ttu-id="aee19-160">IP-címelőtagokat toomodify a helyi hálózati átjáró:</span><span class="sxs-lookup"><span data-stu-id="aee19-160">toomodify IP address prefixes for your local network gateway:</span></span><br>
<span data-ttu-id="aee19-161">A helyi hálózati átjáró előtagjai bizonyos esetekben változnak.</span><span class="sxs-lookup"><span data-stu-id="aee19-161">Sometimes your local network gateway prefixes change.</span></span> <span data-ttu-id="aee19-162">hello lépések végrehajtása toomodify az IP-cím előtagokat, attól függ, hogy létrehozott egy VPN gateway-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="aee19-162">hello steps you take toomodify your IP address prefixes depend on whether you have created a VPN gateway connection.</span></span> <span data-ttu-id="aee19-163">Lásd: hello [módosítása IP-cím előtagokat helyi hálózati átjáró](#modify) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="aee19-163">See hello [Modify IP address prefixes for a local network gateway](#modify) section of this article.</span></span>

## <span data-ttu-id="aee19-164"><a name="PublicIP"></a>4. Nyilvános IP-cím kérése</span><span class="sxs-lookup"><span data-stu-id="aee19-164"><a name="PublicIP"></a>4. Request a Public IP address</span></span>

<span data-ttu-id="aee19-165">Egy VPN Gateway-nek rendelkeznie kell nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="aee19-165">A VPN gateway must have a Public IP address.</span></span> <span data-ttu-id="aee19-166">Először igényelnie hello IP-cím erőforrás, és ezután tooit hivatkozni, a virtuális hálózati átjáró létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="aee19-166">You first request hello IP address resource, and then refer tooit when creating your virtual network gateway.</span></span> <span data-ttu-id="aee19-167">hello IP-cím dinamikusan toohello erőforrás van hozzárendelve, hello VPN-átjáró létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="aee19-167">hello IP address is dynamically assigned toohello resource when hello VPN gateway is created.</span></span> <span data-ttu-id="aee19-168">A VPN Gateway jelenleg csak a *Dinamikus* nyilvános IP-cím lefoglalását támogatja.</span><span class="sxs-lookup"><span data-stu-id="aee19-168">VPN Gateway currently only supports *Dynamic* Public IP address allocation.</span></span> <span data-ttu-id="aee19-169">Nem kérheti statikus IP-cím hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="aee19-169">You cannot request a Static Public IP address assignment.</span></span> <span data-ttu-id="aee19-170">Ez azonban nem jelenti azt, hogy hello IP-cím hozzá van rendelve tooyour VPN-átjáró után módosítja.</span><span class="sxs-lookup"><span data-stu-id="aee19-170">However, this does not mean that hello IP address changes after it has been assigned tooyour VPN gateway.</span></span> <span data-ttu-id="aee19-171">hello egyetlen alkalom hello nyilvános IP-cím módosításainak mikor van hello átjáró törlődik, és újból létrehozza.</span><span class="sxs-lookup"><span data-stu-id="aee19-171">hello only time hello Public IP address changes is when hello gateway is deleted and re-created.</span></span> <span data-ttu-id="aee19-172">Nem módosul átméretezés, alaphelyzetbe állítás, illetve a VPN Gateway belső karbantartása/frissítése során.</span><span class="sxs-lookup"><span data-stu-id="aee19-172">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of your VPN gateway.</span></span>

<span data-ttu-id="aee19-173">A hozzárendelt tooyour virtuális hálózat VPN-átjáró nyilvános IP-cím kérése.</span><span class="sxs-lookup"><span data-stu-id="aee19-173">Request a Public IP address that will be assigned tooyour virtual network VPN gateway.</span></span>

```powershell
$gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName TestRG1 -Location 'East US' -AllocationMethod Dynamic
```

## <span data-ttu-id="aee19-174"><a name="GatewayIPConfig"></a>5. Hello átjáró IP-konfiguráció címzési létrehozása</span><span class="sxs-lookup"><span data-stu-id="aee19-174"><a name="GatewayIPConfig"></a>5. Create hello gateway IP addressing configuration</span></span>

<span data-ttu-id="aee19-175">hello átjáró konfigurációs hello alhálózatot és a hello nyilvános IP-cím toouse határoz meg.</span><span class="sxs-lookup"><span data-stu-id="aee19-175">hello gateway configuration defines hello subnet and hello public IP address toouse.</span></span> <span data-ttu-id="aee19-176">A következő példa toocreate hello használata az átjáró konfigurációját:</span><span class="sxs-lookup"><span data-stu-id="aee19-176">Use hello following example toocreate your gateway configuration:</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name TestVNet1 -ResourceGroupName TestRG1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```

## <span data-ttu-id="aee19-177"><a name="CreateGateway"></a>6. Hello VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="aee19-177"><a name="CreateGateway"></a>6. Create hello VPN gateway</span></span>

<span data-ttu-id="aee19-178">Hello virtuális hálózat VPN-átjáró létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="aee19-178">Create hello virtual network VPN gateway.</span></span> <span data-ttu-id="aee19-179">VPN-átjáró létrehozása akár is igénybe vehet too45 perc vagy több toocomplete.</span><span class="sxs-lookup"><span data-stu-id="aee19-179">Creating a VPN gateway can take up too45 minutes or more toocomplete.</span></span>

<span data-ttu-id="aee19-180">A következő értékek hello használata:</span><span class="sxs-lookup"><span data-stu-id="aee19-180">Use hello following values:</span></span>

* <span data-ttu-id="aee19-181">Hello *- GatewayType* konfiguráció esetén a pont-pont *Vpn*.</span><span class="sxs-lookup"><span data-stu-id="aee19-181">hello *-GatewayType* for a Site-to-Site configuration is *Vpn*.</span></span> <span data-ttu-id="aee19-182">hello átjáró típus mindig valósít meg adott toohello-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="aee19-182">hello gateway type is always specific toohello configuration that you are implementing.</span></span> <span data-ttu-id="aee19-183">Más átjáró-konfigurációk például -GatewayType ExpressRoute típus alkalmazását igényelhetik.</span><span class="sxs-lookup"><span data-stu-id="aee19-183">For example, other gateway configurations may require -GatewayType ExpressRoute.</span></span>
* <span data-ttu-id="aee19-184">Hello *– VpnType* lehet *RouteBased* (említett tooas bizonyos dokumentációkban dinamikus átjáró), vagy *PolicyBased* (említett tooas bizonyos dokumentációkban statikus átjáró ).</span><span class="sxs-lookup"><span data-stu-id="aee19-184">hello *-VpnType* can be *RouteBased* (referred tooas a Dynamic Gateway in some documentation), or *PolicyBased* (referred tooas a Static Gateway in some documentation).</span></span> <span data-ttu-id="aee19-185">További információk a VPN-átjárótípusokról: [Információk a VPN Gateway-ről](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="aee19-185">For more information about VPN gateway types, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="aee19-186">Válassza ki a megjeleníteni kívánt toouse átjáró-Termékváltozat hello.</span><span class="sxs-lookup"><span data-stu-id="aee19-186">Select hello Gateway SKU that you want toouse.</span></span> <span data-ttu-id="aee19-187">Egyes termékváltozatok konfigurációs korlátokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="aee19-187">There are configuration limitations for certain SKUs.</span></span> <span data-ttu-id="aee19-188">További információkért lásd: [Az átjárók termékváltozatai](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="aee19-188">For more information, see [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span> <span data-ttu-id="aee19-189">Ha hibaüzenetet kap hello VPN-átjáróval kapcsolatos hello létrehozásakor - GatewaySku, győződjön meg arról, hogy telepítette-e az hello hello PowerShell-parancsmagok legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="aee19-189">If you get an error when creating hello VPN gateway regarding hello -GatewaySku, verify that you have installed hello latest version of hello PowerShell cmdlets.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'East US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1
```

## <span data-ttu-id="aee19-190"><a name="ConfigureVPNDevice"></a>7. VPN-eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="aee19-190"><a name="ConfigureVPNDevice"></a>7. Configure your VPN device</span></span>

<span data-ttu-id="aee19-191">Pont-pont kapcsolatok tooan a helyi hálózaton egy VPN-eszköz szükség.</span><span class="sxs-lookup"><span data-stu-id="aee19-191">Site-to-Site connections tooan on-premises network require a VPN device.</span></span> <span data-ttu-id="aee19-192">Ebben a lépésben a VPN-eszköz konfigurálása következik.</span><span class="sxs-lookup"><span data-stu-id="aee19-192">In this step, you configure your VPN device.</span></span> <span data-ttu-id="aee19-193">A VPN-eszköz konfigurálásakor hello a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="aee19-193">When configuring your VPN device, you need hello following:</span></span>

- <span data-ttu-id="aee19-194">Megosztott kulcs.</span><span class="sxs-lookup"><span data-stu-id="aee19-194">A shared key.</span></span> <span data-ttu-id="aee19-195">Ez az azonos megosztott hello a telephelyek közötti VPN-kapcsolat létrehozásakor megadott kulcs.</span><span class="sxs-lookup"><span data-stu-id="aee19-195">This is hello same shared key that you specify when creating your Site-to-Site VPN connection.</span></span> <span data-ttu-id="aee19-196">A példákban alapvető megosztott kulcsot használunk.</span><span class="sxs-lookup"><span data-stu-id="aee19-196">In our examples, we use a basic shared key.</span></span> <span data-ttu-id="aee19-197">Azt javasoljuk, hogy létrehozhat egy összetett kulcs toouse.</span><span class="sxs-lookup"><span data-stu-id="aee19-197">We recommend that you generate a more complex key toouse.</span></span>
- <span data-ttu-id="aee19-198">hello a virtuális hálózati átjáró nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="aee19-198">hello Public IP address of your virtual network gateway.</span></span> <span data-ttu-id="aee19-199">Hello nyilvános IP-cím hello Azure-portálon, a PowerShell vagy a parancssori felület használatával tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="aee19-199">You can view hello public IP address by using hello Azure portal, PowerShell, or CLI.</span></span> <span data-ttu-id="aee19-200">toofind hello a PowerShell, a következő példa használata hello használó virtuális hálózati átjáró nyilvános IP-cím:</span><span class="sxs-lookup"><span data-stu-id="aee19-200">toofind hello Public IP address of your virtual network gateway using PowerShell, use hello following example:</span></span>

  ```powershell
  Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG1
  ```

[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <span data-ttu-id="aee19-201"><a name="CreateConnection"></a>8. Hello VPN-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="aee19-201"><a name="CreateConnection"></a>8. Create hello VPN connection</span></span>

<span data-ttu-id="aee19-202">Ezután hozzon létre hello telephelyek közötti VPN kapcsolatot a virtuális hálózati átjáró és a VPN-eszköz.</span><span class="sxs-lookup"><span data-stu-id="aee19-202">Next, create hello Site-to-Site VPN connection between your virtual network gateway and your VPN device.</span></span> <span data-ttu-id="aee19-203">Lehet, hogy tooreplace hello értékeket a saját.</span><span class="sxs-lookup"><span data-stu-id="aee19-203">Be sure tooreplace hello values with your own.</span></span> <span data-ttu-id="aee19-204">hello megosztott kulcsot meg kell egyeznie a VPN-eszköz konfigurációjában használt hello érték.</span><span class="sxs-lookup"><span data-stu-id="aee19-204">hello shared key must match hello value you used for your VPN device configuration.</span></span> <span data-ttu-id="aee19-205">Figyelje meg, hogy hello "-ConnectionType" hely-hely *IPsec*.</span><span class="sxs-lookup"><span data-stu-id="aee19-205">Notice that hello '-ConnectionType' for Site-to-Site is *IPsec*.</span></span>

1. <span data-ttu-id="aee19-206">Hello változók megadása.</span><span class="sxs-lookup"><span data-stu-id="aee19-206">Set hello variables.</span></span>
  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
  $local = Get-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1
  ```

2. <span data-ttu-id="aee19-207">Hello kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="aee19-207">Create hello connection.</span></span>
  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite2 -ResourceGroupName TestRG1 `
  -Location 'East US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

<span data-ttu-id="aee19-208">Hello közben rövid után kapcsolat jön létre.</span><span class="sxs-lookup"><span data-stu-id="aee19-208">After a short while, hello connection will be established.</span></span>

## <span data-ttu-id="aee19-209"><a name="toverify"></a>9. Hello VPN-kapcsolat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="aee19-209"><a name="toverify"></a>9. Verify hello VPN connection</span></span>

<span data-ttu-id="aee19-210">Van néhány különböző módokon tooverify a VPN-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="aee19-210">There are a few different ways tooverify your VPN connection.</span></span>

[!INCLUDE [Verify connection](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <span data-ttu-id="aee19-211"><a name="connectVM"></a>tooconnect tooa virtuális gép</span><span class="sxs-lookup"><span data-stu-id="aee19-211"><a name="connectVM"></a>tooconnect tooa virtual machine</span></span>

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]


## <span data-ttu-id="aee19-212"><a name="modify"></a>Helyi hálózati átjáró IP-címelőtagjainak módosítása</span><span class="sxs-lookup"><span data-stu-id="aee19-212"><a name="modify"></a>Modify IP address prefixes for a local network gateway</span></span>

<span data-ttu-id="aee19-213">Ha hello IP-cím előtagokat, amelyet irányítása tooyour helyszíni hely módosításához hello helyi hálózati átjáró módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="aee19-213">If hello IP address prefixes that you want routed tooyour on-premises location change, you can modify hello local network gateway.</span></span> <span data-ttu-id="aee19-214">Két utasításcsoportot adtunk meg.</span><span class="sxs-lookup"><span data-stu-id="aee19-214">Two sets of instructions are provided.</span></span> <span data-ttu-id="aee19-215">hello utasításokat úgy dönt, attól függ, hogy már létrehozta a gateway-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="aee19-215">hello instructions you choose depend on whether you have already created your gateway connection.</span></span>

[!INCLUDE [Modify prefixes](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <span data-ttu-id="aee19-216"><a name="modifygwipaddress"></a>Hello átjáró IP-címet a helyi hálózati átjáró módosítása</span><span class="sxs-lookup"><span data-stu-id="aee19-216"><a name="modifygwipaddress"></a>Modify hello gateway IP address for a local network gateway</span></span>

[!INCLUDE [Modify gateway IP address](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="aee19-217">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aee19-217">Next steps</span></span>

*  <span data-ttu-id="aee19-218">Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="aee19-218">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="aee19-219">További információkért lásd: [Virtuális gépek](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="aee19-219">For more information, see [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="aee19-220">A BGP kapcsolatos információkért lásd: hello [BGP áttekintése](vpn-gateway-bgp-overview.md) és [hogyan tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="aee19-220">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
