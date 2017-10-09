---
title: "Csatlakozás a helyi hálózati tooan Azure-beli virtuális hálózat: telephelyek közötti VPN: CLI |} Microsoft Docs"
description: "Az IPsec-kapcsolat a helyszíni hálózati tooan Azure-beli virtuális hálózat felett lépéseket toocreate hello nyilvános internethez. Ezen lépéseket követve létrehozhat egy helyek közötti VPN-átjáró kapcsolatot a parancssori felület segítségével."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: c652cf2caf3928cdeb19d7dc329f6db101e5ed90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a><span data-ttu-id="0baf3-104">Virtuális hálózat létrehozása helyek közötti VPN-kapcsolattal a parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="0baf3-104">Create a virtual network with a Site-to-Site VPN connection using CLI</span></span>

<span data-ttu-id="0baf3-105">Ez a cikk bemutatja, hogyan toouse hello Azure CLI toocreate a helyszíni hálózati toohello VNet a pont-pont VPN gateway-kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="0baf3-105">This article shows you how toouse hello Azure CLI toocreate a Site-to-Site VPN gateway connection from your on-premises network toohello VNet.</span></span> <span data-ttu-id="0baf3-106">a cikkben ismertetett hello alkalmazása toohello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="0baf3-106">hello steps in this article apply toohello Resource Manager deployment model.</span></span> <span data-ttu-id="0baf3-107">Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:</span><span class="sxs-lookup"><span data-stu-id="0baf3-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span><br>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0baf3-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0baf3-108">Azure portal</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="0baf3-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0baf3-109">PowerShell</span></span>](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [<span data-ttu-id="0baf3-110">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="0baf3-110">CLI</span></span>](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [<span data-ttu-id="0baf3-111">(Klasszikus) Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0baf3-111">Azure portal (classic)</span></span>](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>


![Helyek közötti VPN Gateway létesítmények közötti kapcsolathoz – diagram](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-diagram.png)

<span data-ttu-id="0baf3-113">Pont-pont VPN gateway-kapcsolattal használt tooconnect a helyszíni hálózati tooan Azure-beli virtuális hálózat (IKEv1 vagy IKEv2) IPsec/IKE VPN-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="0baf3-113">A Site-to-Site VPN gateway connection is used tooconnect your on-premises network tooan Azure virtual network over an IPsec/IKE (IKEv1 or IKEv2) VPN tunnel.</span></span> <span data-ttu-id="0baf3-114">Ilyen típusú kapcsolat egy VPN található helyszíni Eszközkezelési, amely rendelkezik egy külsőleg irányuló nyilvános IP-cím tooit igényel.</span><span class="sxs-lookup"><span data-stu-id="0baf3-114">This type of connection requires a VPN device located on-premises that has an externally facing public IP address assigned tooit.</span></span> <span data-ttu-id="0baf3-115">További információk a VPN-átjárókról: [Információk a VPN Gatewayről](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="0baf3-115">For more information about VPN gateways, see [About VPN gateway](vpn-gateway-about-vpngateways.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="0baf3-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="0baf3-116">Before you begin</span></span>

<span data-ttu-id="0baf3-117">Győződjön meg arról, hogy teljesül-e az hello feltételek konfigurációs megkezdése előtt a következő:</span><span class="sxs-lookup"><span data-stu-id="0baf3-117">Verify that you have met hello following criteria before beginning configuration:</span></span>

* <span data-ttu-id="0baf3-118">Győződjön meg arról, hogy egy kompatibilis VPN-eszköz és a rendszer képes tooconfigure azt.</span><span class="sxs-lookup"><span data-stu-id="0baf3-118">Make sure you have a compatible VPN device and someone who is able tooconfigure it.</span></span> <span data-ttu-id="0baf3-119">További információk a kompatibilis VPN-eszközökről és az eszközkonfigurációról: [Tudnivalók a VPN-eszközökről](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="0baf3-119">For more information about compatible VPN devices and device configuration, see [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span>
* <span data-ttu-id="0baf3-120">Győződjön meg arról, hogy rendelkezik egy kifelé irányuló, nyilvános IPv4-címmel a VPN-eszköz számára.</span><span class="sxs-lookup"><span data-stu-id="0baf3-120">Verify that you have an externally facing public IPv4 address for your VPN device.</span></span> <span data-ttu-id="0baf3-121">Ez az IP-cím nem lehet NAT mögötti.</span><span class="sxs-lookup"><span data-stu-id="0baf3-121">This IP address cannot be located behind a NAT.</span></span>
* <span data-ttu-id="0baf3-122">Ha nem ismeri a hello IP-címtartományok található, a helyszíni hálózati konfiguráció, a szükséges toocoordinate ezeket az információkat is tartalmazza, aki.</span><span class="sxs-lookup"><span data-stu-id="0baf3-122">If you are unfamiliar with hello IP address ranges located in your on-premises network configuration, you need toocoordinate with someone who can provide those details for you.</span></span> <span data-ttu-id="0baf3-123">Ez a konfiguráció létrehozásakor meg kell adnia hello IP-címtartomány címelőtagokat, hogy Azure tooyour helyszíni hely fogja átirányítani.</span><span class="sxs-lookup"><span data-stu-id="0baf3-123">When you create this configuration, you must specify hello IP address range prefixes that Azure will route tooyour on-premises location.</span></span> <span data-ttu-id="0baf3-124">A helyszíni hálózat alhálózatainak hello egyike is lap tooconnect a kívánt hello virtuális hálózati alhálózatokon keresztül.</span><span class="sxs-lookup"><span data-stu-id="0baf3-124">None of hello subnets of your on-premises network can over lap with hello virtual network subnets that you want tooconnect to.</span></span>
* <span data-ttu-id="0baf3-125">Győződjön meg arról, hogy telepítette-e az hello parancssori felület parancsait (2.0-s vagy újabb) legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="0baf3-125">Verify that you have installed latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="0baf3-126">Hello parancssori felület parancsait telepítésével kapcsolatos információkért lásd: [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli) és [Ismerkedés az Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0baf3-126">For information about installing hello CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli) and [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>

### <span data-ttu-id="0baf3-127"><a name="example"></a>Példaértékek</span><span class="sxs-lookup"><span data-stu-id="0baf3-127"><a name="example"></a>Example values</span></span>

<span data-ttu-id="0baf3-128">A következő értékek toocreate egy tesztkörnyezetben hello használhatja, vagy tekintse meg a toothese értékek toobetter megérteni a cikkben szereplő példák hello:</span><span class="sxs-lookup"><span data-stu-id="0baf3-128">You can use hello following values toocreate a test environment, or refer toothese values toobetter understand hello examples in this article:</span></span>

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.0.0/24 
GatewaySubnet           = 10.11.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <span data-ttu-id="0baf3-129"><a name="Login"></a>1. Csatlakozás tooyour előfizetés</span><span class="sxs-lookup"><span data-stu-id="0baf3-129"><a name="Login"></a>1. Connect tooyour subscription</span></span>

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

## <span data-ttu-id="0baf3-130"><a name="rg"></a>2. Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="0baf3-130"><a name="rg"></a>2. Create a resource group</span></span>

<span data-ttu-id="0baf3-131">hello alábbi példa létrehoz egy erőforráscsoportot "TestRG1" hello "eastus" helyen.</span><span class="sxs-lookup"><span data-stu-id="0baf3-131">hello following example creates a resource group named 'TestRG1' in hello 'eastus' location.</span></span> <span data-ttu-id="0baf3-132">Ha már rendelkezik egy erőforráscsoport, amelyet toocreate a virtuális hálózat hello régióban, használhatja, hogy egy helyette.</span><span class="sxs-lookup"><span data-stu-id="0baf3-132">If you already have a resource group in hello region that you want toocreate your VNet, you can use that one instead.</span></span>

```azurecli
az group create --name TestRG1 --location eastus
```

## <span data-ttu-id="0baf3-133"><a name="VNet"></a>3. Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="0baf3-133"><a name="VNet"></a>3. Create a virtual network</span></span>

<span data-ttu-id="0baf3-134">Ha még nem rendelkezik virtuális hálózat, hozzon létre egyet hello segítségével [az hálózati vnet létrehozása](/cli/azure/network/vnet#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="0baf3-134">If you don't already have a virtual network, create one using hello [az network vnet create](/cli/azure/network/vnet#create) command.</span></span> <span data-ttu-id="0baf3-135">Amikor egy virtuális hálózat létrehozásával, győződjön meg arról, hogy megadott hello címterek nem fedhetnek át olyan hello címterek, hogy rendelkezik a helyi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="0baf3-135">When creating a virtual network, make sure that hello address spaces you specify don't overlap any of hello address spaces that you have on your on-premises network.</span></span>

<span data-ttu-id="0baf3-136">hello alábbi példa létrehoz egy virtuális hálózatot "TestVNet1" és az alhálózatot, "Alhalozat_1" nevű.</span><span class="sxs-lookup"><span data-stu-id="0baf3-136">hello following example creates a virtual network named 'TestVNet1' and a subnet, 'Subnet1'.</span></span>

```azurecli
az network vnet create --name TestVNet1 --resource-group TestRG1 --address-prefix 10.11.0.0/16 --location eastus --subnet-name Subnet1 --subnet-prefix 10.11.0.0/24
```

## <span data-ttu-id="0baf3-137">4. <a name="gwsub"></a>Hozzon létre hello átjáró-alhálózatot</span><span class="sxs-lookup"><span data-stu-id="0baf3-137">4. <a name="gwsub"></a>Create hello gateway subnet</span></span>

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

<span data-ttu-id="0baf3-138">Ehhez a konfigurációhoz átjáróalhálózat is szükséges.</span><span class="sxs-lookup"><span data-stu-id="0baf3-138">For this configuration, you also need a gateway subnet.</span></span> <span data-ttu-id="0baf3-139">hello virtuális hálózati átjáró egy átjáró-alhálózatot, amely tartalmazza a hello hello VPN gateway szolgáltatások által használt IP-címeket használ.</span><span class="sxs-lookup"><span data-stu-id="0baf3-139">hello virtual network gateway uses a gateway subnet that contains hello IP addresses that are used by hello VPN gateway services.</span></span> <span data-ttu-id="0baf3-140">A létrehozott átjáróalhálózatnak a „GatewaySubnet” nevet kell adnia.</span><span class="sxs-lookup"><span data-stu-id="0baf3-140">When you create a gateway subnet, it must be named 'GatewaySubnet'.</span></span> <span data-ttu-id="0baf3-141">Ha ezt az alhálózatot máshogy nevezi el, az létrejön ugyan, de az Azure nem kezeli átjáró-alhálózatként.</span><span class="sxs-lookup"><span data-stu-id="0baf3-141">If you name it something else, you create a subnet, but Azure won't treat it as a gateway subnet.</span></span>

<span data-ttu-id="0baf3-142">hello átjáróalhálózatot megadott hello mérete függ hello VPN-átjáró konfigurációs, amelyet az toocreate.</span><span class="sxs-lookup"><span data-stu-id="0baf3-142">hello size of hello gateway subnet that you specify depends on hello VPN gateway configuration that you want toocreate.</span></span> <span data-ttu-id="0baf3-143">Habár lehetséges toocreate mérete /29 legyen egy átjáró-alhálózatot, azt javasoljuk, hogy hozzon létre egy nagyobb alhálózat több címet /27 vagy /28 kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="0baf3-143">While it is possible toocreate a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting /27 or /28.</span></span> <span data-ttu-id="0baf3-144">Egy nagyobb átjáróalhálózat használata lehetővé teszi a elegendő IP-címek tooaccommodate lehetséges jövőbeli konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="0baf3-144">Using a larger gateway subnet allows for enough IP addresses tooaccommodate possible future configurations.</span></span>

<span data-ttu-id="0baf3-145">Használjon hello [az alhálózaton virtuális hálózat létrehozása](/cli/azure/network/vnet/subnet#create) parancs toocreate hello átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="0baf3-145">Use hello [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) command toocreate hello gateway subnet.</span></span>

```azurecli
az network vnet subnet create --address-prefix 10.11.255.0/27 --name GatewaySubnet --resource-group TestRG1 --vnet-name TestVNet1
```

## <span data-ttu-id="0baf3-146"><a name="localnet"></a>5. Hello helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="0baf3-146"><a name="localnet"></a>5. Create hello local network gateway</span></span>

<span data-ttu-id="0baf3-147">hello helyi hálózati átjáró általában tooyour helyszíni helyre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="0baf3-147">hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="0baf3-148">Adjon hello hely egy nevet, amellyel Azure is tekintse meg a tooit, majd adja meg hello IP-címet a hello a helyszíni VPN-eszköz toowhich létrehoz egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="0baf3-148">You give hello site a name by which Azure can refer tooit, then specify hello IP address of hello on-premises VPN device toowhich you will create a connection.</span></span> <span data-ttu-id="0baf3-149">Is meg hello IP-cím előtagokat hello VPN gateway toohello VPN-eszközön keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="0baf3-149">You also specify hello IP address prefixes that will be routed through hello VPN gateway toohello VPN device.</span></span> <span data-ttu-id="0baf3-150">megadott hello címelőtagokat a helyszíni hálózaton található hello előtagok.</span><span class="sxs-lookup"><span data-stu-id="0baf3-150">hello address prefixes you specify are hello prefixes located on your on-premises network.</span></span> <span data-ttu-id="0baf3-151">Ha módosítja a helyszíni hálózat, hello előtagok könnyen frissítheti.</span><span class="sxs-lookup"><span data-stu-id="0baf3-151">If your on-premises network changes, you can easily update hello prefixes.</span></span>

<span data-ttu-id="0baf3-152">A következő értékek hello használata:</span><span class="sxs-lookup"><span data-stu-id="0baf3-152">Use hello following values:</span></span>

* <span data-ttu-id="0baf3-153">Hello *– ip-átjárócím* hello IP-címe a helyszíni VPN-eszköz.</span><span class="sxs-lookup"><span data-stu-id="0baf3-153">hello *--gateway-ip-address* is hello IP address of your on-premises VPN device.</span></span> <span data-ttu-id="0baf3-154">A VPN-eszköz nem lehet NAT mögött.</span><span class="sxs-lookup"><span data-stu-id="0baf3-154">Your VPN device cannot be located behind a NAT.</span></span>
* <span data-ttu-id="0baf3-155">Hello *– helyi címelőtagokat* vannak a helyszíni címtér.</span><span class="sxs-lookup"><span data-stu-id="0baf3-155">hello *--local-address-prefixes* are your on-premises address spaces.</span></span>

<span data-ttu-id="0baf3-156">Használjon hello [az hálózati helyi-átjáró létrehozása](/cli/azure/network/local-gateway#create) parancs tooadd egy helyi hálózati átjáró több címelőtagok:</span><span class="sxs-lookup"><span data-stu-id="0baf3-156">Use hello [az network local-gateway create](/cli/azure/network/local-gateway#create) command tooadd a local network gateway with multiple address prefixes:</span></span>

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --resource-group TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <span data-ttu-id="0baf3-157"><a name="PublicIP"></a>6. Nyilvános IP-cím kérése</span><span class="sxs-lookup"><span data-stu-id="0baf3-157"><a name="PublicIP"></a>6. Request a Public IP address</span></span>

<span data-ttu-id="0baf3-158">Egy VPN Gateway-nek rendelkeznie kell nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="0baf3-158">A VPN gateway must have a Public IP address.</span></span> <span data-ttu-id="0baf3-159">Először igényelnie hello IP-cím erőforrás, és ezután tooit hivatkozni, a virtuális hálózati átjáró létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="0baf3-159">You first request hello IP address resource, and then refer tooit when creating your virtual network gateway.</span></span> <span data-ttu-id="0baf3-160">hello IP-cím dinamikusan toohello erőforrás van hozzárendelve, hello VPN-átjáró létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="0baf3-160">hello IP address is dynamically assigned toohello resource when hello VPN gateway is created.</span></span> <span data-ttu-id="0baf3-161">A VPN Gateway jelenleg csak a *Dinamikus* nyilvános IP-cím lefoglalását támogatja.</span><span class="sxs-lookup"><span data-stu-id="0baf3-161">VPN Gateway currently only supports *Dynamic* Public IP address allocation.</span></span> <span data-ttu-id="0baf3-162">Nem kérheti statikus IP-cím hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="0baf3-162">You cannot request a Static Public IP address assignment.</span></span> <span data-ttu-id="0baf3-163">Ez azonban nem jelenti azt, hogy hello IP-cím hozzá van rendelve tooyour VPN-átjáró után módosítja.</span><span class="sxs-lookup"><span data-stu-id="0baf3-163">However, this does not mean that hello IP address changes after it has been assigned tooyour VPN gateway.</span></span> <span data-ttu-id="0baf3-164">hello egyetlen alkalom hello nyilvános IP-cím módosításainak mikor van hello átjáró törlődik, és újból létrehozza.</span><span class="sxs-lookup"><span data-stu-id="0baf3-164">hello only time hello Public IP address changes is when hello gateway is deleted and re-created.</span></span> <span data-ttu-id="0baf3-165">Nem módosul átméretezés, alaphelyzetbe állítás, illetve a VPN Gateway belső karbantartása/frissítése során.</span><span class="sxs-lookup"><span data-stu-id="0baf3-165">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of your VPN gateway.</span></span>

<span data-ttu-id="0baf3-166">Használjon hello [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create) parancs toorequest dinamikus nyilvános IP-címnek.</span><span class="sxs-lookup"><span data-stu-id="0baf3-166">Use hello [az network public-ip create](/cli/azure/network/public-ip#create) command toorequest a Dynamic Public IP address.</span></span>

```azurecli
az network public-ip create --name VNet1GWIP --resource-group TestRG1 --allocation-method Dynamic
```

## <span data-ttu-id="0baf3-167"><a name="CreateGateway"></a>7. Hello VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="0baf3-167"><a name="CreateGateway"></a>7. Create hello VPN gateway</span></span>

<span data-ttu-id="0baf3-168">Hello virtuális hálózat VPN-átjáró létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0baf3-168">Create hello virtual network VPN gateway.</span></span> <span data-ttu-id="0baf3-169">VPN-átjáró létrehozása akár is igénybe vehet too45 perc vagy több toocomplete.</span><span class="sxs-lookup"><span data-stu-id="0baf3-169">Creating a VPN gateway can take up too45 minutes or more toocomplete.</span></span>

<span data-ttu-id="0baf3-170">A következő értékek hello használata:</span><span class="sxs-lookup"><span data-stu-id="0baf3-170">Use hello following values:</span></span>

* <span data-ttu-id="0baf3-171">Hello *– átjáró típusú* konfiguráció esetén a pont-pont *Vpn*.</span><span class="sxs-lookup"><span data-stu-id="0baf3-171">hello *--gateway-type* for a Site-to-Site configuration is *Vpn*.</span></span> <span data-ttu-id="0baf3-172">hello átjáró típus mindig valósít meg adott toohello-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="0baf3-172">hello gateway type is always specific toohello configuration that you are implementing.</span></span> <span data-ttu-id="0baf3-173">További információért lásd: [Átjárótípusok](vpn-gateway-about-vpn-gateway-settings.md#gwtype).</span><span class="sxs-lookup"><span data-stu-id="0baf3-173">For more information, see [Gateway types](vpn-gateway-about-vpn-gateway-settings.md#gwtype).</span></span>
* <span data-ttu-id="0baf3-174">Hello *– vpn-típus* lehet *RouteBased* (említett tooas bizonyos dokumentációkban dinamikus átjáró), vagy *PolicyBased* (említett tooas egyes statikus átjáró a dokumentációt).</span><span class="sxs-lookup"><span data-stu-id="0baf3-174">hello *--vpn-type* can be *RouteBased* (referred tooas a Dynamic Gateway in some documentation), or *PolicyBased* (referred tooas a Static Gateway in some documentation).</span></span> <span data-ttu-id="0baf3-175">hello beállítás adott toorequirements hello eszköz, amely csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="0baf3-175">hello setting is specific toorequirements of hello device that you are connecting to.</span></span> <span data-ttu-id="0baf3-176">További információk a VPN-átjárótípusokról: [Információk a VPN Gateway konfigurációs beállításairól](vpn-gateway-about-vpn-gateway-settings.md#vpntype).</span><span class="sxs-lookup"><span data-stu-id="0baf3-176">For more information about VPN gateway types, see [About VPN Gateway configuration settings](vpn-gateway-about-vpn-gateway-settings.md#vpntype).</span></span>
* <span data-ttu-id="0baf3-177">Válassza ki a megjeleníteni kívánt toouse átjáró-Termékváltozat hello.</span><span class="sxs-lookup"><span data-stu-id="0baf3-177">Select hello Gateway SKU that you want toouse.</span></span> <span data-ttu-id="0baf3-178">Egyes termékváltozatok konfigurációs korlátokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="0baf3-178">There are configuration limitations for certain SKUs.</span></span> <span data-ttu-id="0baf3-179">További információkért lásd: [Az átjárók termékváltozatai](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="0baf3-179">For more information, see [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span>

<span data-ttu-id="0baf3-180">Hozzon létre hello VPN-átjáró hello segítségével [az hálózati vnet-átjáró létrehozása](/cli/azure/network/vnet-gateway#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="0baf3-180">Create hello VPN gateway using hello [az network vnet-gateway create](/cli/azure/network/vnet-gateway#create) command.</span></span> <span data-ttu-id="0baf3-181">A parancs futtatásakor hello használatával – nincs - wait paramétert, ha bármely visszajelzést vagy kimeneti nem látható.</span><span class="sxs-lookup"><span data-stu-id="0baf3-181">If you run this command using hello '--no-wait' parameter, you don't see any feedback or output.</span></span> <span data-ttu-id="0baf3-182">Ez a paraméter lehetővé teszi, hogy hello átjáró toocreate hello háttérben.</span><span class="sxs-lookup"><span data-stu-id="0baf3-182">This parameter allows hello gateway toocreate in hello background.</span></span> <span data-ttu-id="0baf3-183">Körülbelül 45 percig toocreate átjáró szükséges.</span><span class="sxs-lookup"><span data-stu-id="0baf3-183">It takes around 45 minutes toocreate a gateway.</span></span>

```azurecli
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWIP --resource-group TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku VpnGw1 --no-wait 
```

## <span data-ttu-id="0baf3-184"><a name="VPNDevice"></a>8. VPN-eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0baf3-184"><a name="VPNDevice"></a>8. Configure your VPN device</span></span>

<span data-ttu-id="0baf3-185">Pont-pont kapcsolatok tooan a helyi hálózaton egy VPN-eszköz szükség.</span><span class="sxs-lookup"><span data-stu-id="0baf3-185">Site-to-Site connections tooan on-premises network require a VPN device.</span></span> <span data-ttu-id="0baf3-186">Ebben a lépésben a VPN-eszköz konfigurálása következik.</span><span class="sxs-lookup"><span data-stu-id="0baf3-186">In this step, you configure your VPN device.</span></span> <span data-ttu-id="0baf3-187">A VPN-eszköz konfigurálásakor hello a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="0baf3-187">When configuring your VPN device, you need hello following:</span></span>

- <span data-ttu-id="0baf3-188">Megosztott kulcs.</span><span class="sxs-lookup"><span data-stu-id="0baf3-188">A shared key.</span></span> <span data-ttu-id="0baf3-189">Ez az azonos megosztott hello a telephelyek közötti VPN-kapcsolat létrehozásakor megadott kulcs.</span><span class="sxs-lookup"><span data-stu-id="0baf3-189">This is hello same shared key that you specify when creating your Site-to-Site VPN connection.</span></span> <span data-ttu-id="0baf3-190">A példákban alapvető megosztott kulcsot használunk.</span><span class="sxs-lookup"><span data-stu-id="0baf3-190">In our examples, we use a basic shared key.</span></span> <span data-ttu-id="0baf3-191">Azt javasoljuk, hogy létrehozhat egy összetett kulcs toouse.</span><span class="sxs-lookup"><span data-stu-id="0baf3-191">We recommend that you generate a more complex key toouse.</span></span>
- <span data-ttu-id="0baf3-192">hello a virtuális hálózati átjáró nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="0baf3-192">hello Public IP address of your virtual network gateway.</span></span> <span data-ttu-id="0baf3-193">Hello nyilvános IP-cím hello Azure-portálon, a PowerShell vagy a parancssori felület használatával tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="0baf3-193">You can view hello public IP address by using hello Azure portal, PowerShell, or CLI.</span></span> <span data-ttu-id="0baf3-194">toofind hello nyilvános IP-címet a virtuális hálózati átjáró használata hello [az nyilvános ip-lista](/cli/azure/network/public-ip#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="0baf3-194">toofind hello public IP address of your virtual network gateway, use hello [az network public-ip list](/cli/azure/network/public-ip#list) command.</span></span> <span data-ttu-id="0baf3-195">Olvasás, hello eredménye a nyilvános IP-címek táblázatos formátumú formázott toodisplay hello listája.</span><span class="sxs-lookup"><span data-stu-id="0baf3-195">For easy reading, hello output is formatted toodisplay hello list of public IPs in table format.</span></span>

  ```azurecli
  az network public-ip list --resource-group TestRG1 --output table
  ```


[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <span data-ttu-id="0baf3-196"><a name="CreateConnection"></a>9. Hello VPN-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="0baf3-196"><a name="CreateConnection"></a>9. Create hello VPN connection</span></span>

<span data-ttu-id="0baf3-197">A virtuális hálózati átjáró és a helyszíni VPN-eszköz közötti pont-pont hello VPN-kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0baf3-197">Create hello Site-to-Site VPN connection between your virtual network gateway and your on-premises VPN device.</span></span> <span data-ttu-id="0baf3-198">Fizetési különös figyelmet toohello megosztott kulcs értékét, amelynek meg kell egyeznie a konfigurált hello megosztott kulcs értéke a VPN-eszköz.</span><span class="sxs-lookup"><span data-stu-id="0baf3-198">Pay particular attention toohello shared key value, which must match hello configured shared key value for your VPN device.</span></span>

<span data-ttu-id="0baf3-199">Hello hello szolgáltatás kapcsolatának létrehozása [az hálózati vpn-kapcsolat létrehozása](/cli/azure/network/vpn-connection#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="0baf3-199">Create hello connection using hello [az network vpn-connection create](/cli/azure/network/vpn-connection#create) command.</span></span>

```azurecli
az network vpn-connection create --name VNet1toSite2 -resource-group TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

<span data-ttu-id="0baf3-200">Hello közben rövid után kapcsolat jön létre.</span><span class="sxs-lookup"><span data-stu-id="0baf3-200">After a short while, hello connection will be established.</span></span>

## <span data-ttu-id="0baf3-201"><a name="toverify"></a>10. Hello VPN-kapcsolat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="0baf3-201"><a name="toverify"></a>10. Verify hello VPN connection</span></span>

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

<span data-ttu-id="0baf3-202">Ha egy másik módszer tooverify toouse szeretné a hálózati kapcsolatot, lásd: [ellenőrizni a VPN-átjáró kapcsolatot](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="0baf3-202">If you want toouse another method tooverify your connection, see [Verify a VPN Gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>

## <span data-ttu-id="0baf3-203"><a name="connectVM"></a>tooconnect tooa virtuális gép</span><span class="sxs-lookup"><span data-stu-id="0baf3-203"><a name="connectVM"></a>tooconnect tooa virtual machine</span></span>

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <span data-ttu-id="0baf3-204"><a name="tasks"></a>Gyakori feladatok</span><span class="sxs-lookup"><span data-stu-id="0baf3-204"><a name="tasks"></a>Common tasks</span></span>

<span data-ttu-id="0baf3-205">Ez a szakasz a helyek közötti konfigurációk használatakor hasznos gyakori parancsokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0baf3-205">This section contains common commands that are helpful when working with site-to-site configurations.</span></span> <span data-ttu-id="0baf3-206">A hálózati parancssori felület parancsait hello teljes listáját, lásd: [Azure CLI - hálózati](/cli/azure/network).</span><span class="sxs-lookup"><span data-stu-id="0baf3-206">For hello full list of CLI networking commands, see [Azure CLI - Networking](/cli/azure/network).</span></span>

[!INCLUDE [local network gateway common tasks](../../includes/vpn-gateway-common-tasks-cli-include.md)]

## <a name="next-steps"></a><span data-ttu-id="0baf3-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0baf3-207">Next steps</span></span>

* <span data-ttu-id="0baf3-208">Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="0baf3-208">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="0baf3-209">További információkért lásd: [Virtuális gépek](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="0baf3-209">For more information, see [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="0baf3-210">A BGP kapcsolatos információkért lásd: hello [BGP áttekintése](vpn-gateway-bgp-overview.md) és [hogyan tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="0baf3-210">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
* <span data-ttu-id="0baf3-211">Információk a kényszerített bújtatásról: [Információk a kényszerített bújtatásról](vpn-gateway-forced-tunneling-rm.md).</span><span class="sxs-lookup"><span data-stu-id="0baf3-211">For information about Forced Tunneling, see [About Forced Tunneling](vpn-gateway-forced-tunneling-rm.md).</span></span>
* <span data-ttu-id="0baf3-212">Információk a magas rendelkezésre állású aktív-aktív kapcsolatokról: [Magas rendelkezésre állású kapcsolatok létesítmények, illetve virtuális hálózatok között](vpn-gateway-highlyavailable.md).</span><span class="sxs-lookup"><span data-stu-id="0baf3-212">For information about Highly Available Active-Active connections, see [Highly Available cross-premises and VNet-to-VNet connectivity](vpn-gateway-highlyavailable.md).</span></span>
* <span data-ttu-id="0baf3-213">Az Azure CLI hálózati parancsainak listáját lásd: [Azure CLI](https://docs.microsoft.com/cli/azure/network).</span><span class="sxs-lookup"><span data-stu-id="0baf3-213">For a list of networking Azure CLI commands, see [Azure CLI](https://docs.microsoft.com/cli/azure/network).</span></span>