---
title: "A helyszíni hálózat csatlakoztatása egy Azure-beli virtuális hálózathoz: Helyek közötti VPN: parancssori felület | Microsoft Docs"
description: "A helyszíni hálózatot az Azure-beli virtuális hálózattal a nyilvános interneten keresztül összekötő IPsec-kapcsolat létrehozásának lépései. Ezen lépéseket követve létrehozhat egy helyek közötti VPN-átjáró kapcsolatot a parancssori felület segítségével."
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
ms.openlocfilehash: 019c5421dc470b18c9087417b93c241cc5730f77
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a><span data-ttu-id="a3eaf-104">Virtuális hálózat létrehozása helyek közötti VPN-kapcsolattal a parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="a3eaf-104">Create a virtual network with a Site-to-Site VPN connection using CLI</span></span>

<span data-ttu-id="a3eaf-105">Ez a cikk bemutatja, hogyan használhatja az Azure CLI-t egy helyek közötti VPN-átjárókapcsolat létrehozására egy helyszíni hálózat és a Vnet között.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-105">This article shows you how to use the Azure CLI to create a Site-to-Site VPN gateway connection from your on-premises network to the VNet.</span></span> <span data-ttu-id="a3eaf-106">A cikkben ismertetett lépések a Resource Manager-alapú üzemi modellre vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-106">The steps in this article apply to the Resource Manager deployment model.</span></span> <span data-ttu-id="a3eaf-107">Ezt a konfigurációt más üzembehelyezési eszközzel vagy üzemi modellel is létrehozhatja, ha egy másik lehetőséget választ az alábbi listáról:</span><span class="sxs-lookup"><span data-stu-id="a3eaf-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from the following list:</span></span><br>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a3eaf-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a3eaf-108">Azure portal</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="a3eaf-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3eaf-109">PowerShell</span></span>](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [<span data-ttu-id="a3eaf-110">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="a3eaf-110">CLI</span></span>](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [<span data-ttu-id="a3eaf-111">(Klasszikus) Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a3eaf-111">Azure portal (classic)</span></span>](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>


![Helyek közötti VPN Gateway létesítmények közötti kapcsolathoz – diagram](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-diagram.png)

<span data-ttu-id="a3eaf-113">A helyek közötti VPN-átjárókapcsolat használatával kapcsolat hozható létre a helyszíni hálózat és egy Azure-beli virtuális hálózat között egy IPsec/IKE (IKEv1 vagy IKEv2) VPN-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-113">A Site-to-Site VPN gateway connection is used to connect your on-premises network to an Azure virtual network over an IPsec/IKE (IKEv1 or IKEv2) VPN tunnel.</span></span> <span data-ttu-id="a3eaf-114">Az ilyen típusú kapcsolatokhoz egy helyszíni VPN-eszközre van szükség, amelyhez hozzá van rendelve egy kifelé irányuló, nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-114">This type of connection requires a VPN device located on-premises that has an externally facing public IP address assigned to it.</span></span> <span data-ttu-id="a3eaf-115">További információk a VPN-átjárókról: [Információk a VPN Gatewayről](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-115">For more information about VPN gateways, see [About VPN gateway](vpn-gateway-about-vpngateways.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a3eaf-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="a3eaf-116">Before you begin</span></span>

<span data-ttu-id="a3eaf-117">A konfigurálás megkezdése előtt győződjön meg a következő feltételek teljesüléséről:</span><span class="sxs-lookup"><span data-stu-id="a3eaf-117">Verify that you have met the following criteria before beginning configuration:</span></span>

* <span data-ttu-id="a3eaf-118">Győződjön meg arról, hogy rendelkezésre áll egy kompatibilis VPN-eszköz és egy azt konfigurálni képes személy.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-118">Make sure you have a compatible VPN device and someone who is able to configure it.</span></span> <span data-ttu-id="a3eaf-119">További információk a kompatibilis VPN-eszközökről és az eszközkonfigurációról: [Tudnivalók a VPN-eszközökről](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-119">For more information about compatible VPN devices and device configuration, see [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span>
* <span data-ttu-id="a3eaf-120">Győződjön meg arról, hogy rendelkezik egy kifelé irányuló, nyilvános IPv4-címmel a VPN-eszköz számára.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-120">Verify that you have an externally facing public IPv4 address for your VPN device.</span></span> <span data-ttu-id="a3eaf-121">Ez az IP-cím nem lehet NAT mögötti.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-121">This IP address cannot be located behind a NAT.</span></span>
* <span data-ttu-id="a3eaf-122">Ha nem ismeri a helyszíni hálózati konfigurációjában található IP-címtereket, egyeztessen valakivel, aki ezeket az adatokat megadhatja Önnek.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-122">If you are unfamiliar with the IP address ranges located in your on-premises network configuration, you need to coordinate with someone who can provide those details for you.</span></span> <span data-ttu-id="a3eaf-123">Amikor létrehozza ezt a konfigurációt, meg kell határoznia az IP-címtartományok előtagjait, amelyeket az Azure majd a helyszínre irányít.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-123">When you create this configuration, you must specify the IP address range prefixes that Azure will route to your on-premises location.</span></span> <span data-ttu-id="a3eaf-124">A helyszíni hálózat egyik alhálózata sem lehet átfedésben azokkal a virtuális alhálózatokkal, amelyekhez csatlakozni kíván.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-124">None of the subnets of your on-premises network can over lap with the virtual network subnets that you want to connect to.</span></span>
* <span data-ttu-id="a3eaf-125">Győződjön meg arról, hogy telepítette a CLI-parancsok legújabb verzióját (2.0-s vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-125">Verify that you have installed latest version of the CLI commands (2.0 or later).</span></span> <span data-ttu-id="a3eaf-126">Információk a CLI-parancsok telepítéséről: [Az Azure CLI 2.0-s verziójának telepítése](/cli/azure/install-azure-cli) és [Bevezetés az Azure CLI 2.0-s verziójának használatába](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-126">For information about installing the CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli) and [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>

### <span data-ttu-id="a3eaf-127"><a name="example"></a>Példaértékek</span><span class="sxs-lookup"><span data-stu-id="a3eaf-127"><a name="example"></a>Example values</span></span>

<span data-ttu-id="a3eaf-128">Az alábbi értékek használatával létrehozhat egy tesztkörnyezetet, vagy segítségükkel értelmezheti a cikkben szereplő példákat:</span><span class="sxs-lookup"><span data-stu-id="a3eaf-128">You can use the following values to create a test environment, or refer to these values to better understand the examples in this article:</span></span>

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

## <span data-ttu-id="a3eaf-129"><a name="Login"></a>1. Csatlakozás az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="a3eaf-129"><a name="Login"></a>1. Connect to your subscription</span></span>

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

## <span data-ttu-id="a3eaf-130"><a name="rg"></a>2. Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="a3eaf-130"><a name="rg"></a>2. Create a resource group</span></span>

<span data-ttu-id="a3eaf-131">A következő példában létrehozunk egy „TestRG1” nevű erőforráscsoportot az „eastus” helyen.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-131">The following example creates a resource group named 'TestRG1' in the 'eastus' location.</span></span> <span data-ttu-id="a3eaf-132">Ha már rendelkezik erőforráscsoporttal abban a régióban, ahol létre kívánja hozni a virtuális hálózatát, használhatja azt is.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-132">If you already have a resource group in the region that you want to create your VNet, you can use that one instead.</span></span>

```azurecli
az group create --name TestRG1 --location eastus
```

## <span data-ttu-id="a3eaf-133"><a name="VNet"></a>3. Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3eaf-133"><a name="VNet"></a>3. Create a virtual network</span></span>

<span data-ttu-id="a3eaf-134">Ha még nem rendelkezik virtuális hálózattal, akkor hozzon létre egyet az [az network vnet create](/cli/azure/network/vnet#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-134">If you don't already have a virtual network, create one using the [az network vnet create](/cli/azure/network/vnet#create) command.</span></span> <span data-ttu-id="a3eaf-135">Virtuális hálózat létrehozásakor győződjön meg róla, hogy a megadott címterek nincsenek átfedésben a helyszíni hálózaton található egyéb címterekkel.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-135">When creating a virtual network, make sure that the address spaces you specify don't overlap any of the address spaces that you have on your on-premises network.</span></span>

<span data-ttu-id="a3eaf-136">Az alábbi példa létrehoz egy „TestVNet1” nevű virtuális hálózatot és egy „Subnet-1” nevű alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-136">The following example creates a virtual network named 'TestVNet1' and a subnet, 'Subnet1'.</span></span>

```azurecli
az network vnet create --name TestVNet1 --resource-group TestRG1 --address-prefix 10.11.0.0/16 --location eastus --subnet-name Subnet1 --subnet-prefix 10.11.0.0/24
```

## <span data-ttu-id="a3eaf-137">4. <a name="gwsub"></a>Az átjáróalhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3eaf-137">4. <a name="gwsub"></a>Create the gateway subnet</span></span>

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

<span data-ttu-id="a3eaf-138">Ehhez a konfigurációhoz átjáróalhálózat is szükséges.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-138">For this configuration, you also need a gateway subnet.</span></span> <span data-ttu-id="a3eaf-139">A virtuális hálózati átjáró átjáróalhálózatot használ, amely a VPN Gateway szolgáltatások által használt IP-címeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-139">The virtual network gateway uses a gateway subnet that contains the IP addresses that are used by the VPN gateway services.</span></span> <span data-ttu-id="a3eaf-140">A létrehozott átjáróalhálózatnak a „GatewaySubnet” nevet kell adnia.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-140">When you create a gateway subnet, it must be named 'GatewaySubnet'.</span></span> <span data-ttu-id="a3eaf-141">Ha ezt az alhálózatot máshogy nevezi el, az létrejön ugyan, de az Azure nem kezeli átjáró-alhálózatként.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-141">If you name it something else, you create a subnet, but Azure won't treat it as a gateway subnet.</span></span>

<span data-ttu-id="a3eaf-142">Az Ön által megadott átjáróalhálózat mérete a létrehozni kívánt VPN-átjárókonfigurációtól függ.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-142">The size of the gateway subnet that you specify depends on the VPN gateway configuration that you want to create.</span></span> <span data-ttu-id="a3eaf-143">Bár akár /29-es átjáróalhálózatot is létrehozhat, javasolt egy ennél nagyobb, több címmel rendelkező alhálózatot létrehozni: /27-eset vagy /28-asat.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-143">While it is possible to create a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting /27 or /28.</span></span> <span data-ttu-id="a3eaf-144">Nagyobb átjáróalhálózat használatával elegendő IP-cím áll rendelkezésre az esetleges jövőbeni konfigurációk megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-144">Using a larger gateway subnet allows for enough IP addresses to accommodate possible future configurations.</span></span>

<span data-ttu-id="a3eaf-145">Az átjáróalhálózat létrehozásához használja az [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-145">Use the [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) command to create the gateway subnet.</span></span>

```azurecli
az network vnet subnet create --address-prefix 10.11.255.0/27 --name GatewaySubnet --resource-group TestRG1 --vnet-name TestVNet1
```

## <span data-ttu-id="a3eaf-146"><a name="localnet"></a>5. A helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3eaf-146"><a name="localnet"></a>5. Create the local network gateway</span></span>

<span data-ttu-id="a3eaf-147">A helyi hálózati átjáró általában a helyszínt jelenti.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-147">The local network gateway typically refers to your on-premises location.</span></span> <span data-ttu-id="a3eaf-148">Olyan nevet adjon a helynek, amellyel az Azure hivatkozhat rá, majd határozza meg annak a helyszíni VPN-eszköznek az IP-címét, amellyel létre kívánja hozni a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-148">You give the site a name by which Azure can refer to it, then specify the IP address of the on-premises VPN device to which you will create a connection.</span></span> <span data-ttu-id="a3eaf-149">Emellett megadhatja azokat az IP-címelőtagokat, amelyek a VPN-átjárón keresztül a VPN-eszközre lesznek irányítva.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-149">You also specify the IP address prefixes that will be routed through the VPN gateway to the VPN device.</span></span> <span data-ttu-id="a3eaf-150">Az Ön által meghatározott címelőtagok a helyszíni hálózatán található előtagok.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-150">The address prefixes you specify are the prefixes located on your on-premises network.</span></span> <span data-ttu-id="a3eaf-151">A helyszíni hálózat módosításakor az előtagok egyszerűen frissíthetők.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-151">If your on-premises network changes, you can easily update the prefixes.</span></span>

<span data-ttu-id="a3eaf-152">Használja a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="a3eaf-152">Use the following values:</span></span>

* <span data-ttu-id="a3eaf-153">A helyszíni VPN-eszköz IP-címe: *--gateway-ip-address*.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-153">The *--gateway-ip-address* is the IP address of your on-premises VPN device.</span></span> <span data-ttu-id="a3eaf-154">A VPN-eszköz nem lehet NAT mögött.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-154">Your VPN device cannot be located behind a NAT.</span></span>
* <span data-ttu-id="a3eaf-155">A helyszíni címterek: *--local-address-prefixes*.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-155">The *--local-address-prefixes* are your on-premises address spaces.</span></span>

<span data-ttu-id="a3eaf-156">Az [az network local-gateway create](/cli/azure/network/local-gateway#create) paranccsal hozzáadhat egy helyi hálózati átjárót több címelőtaggal:</span><span class="sxs-lookup"><span data-stu-id="a3eaf-156">Use the [az network local-gateway create](/cli/azure/network/local-gateway#create) command to add a local network gateway with multiple address prefixes:</span></span>

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --resource-group TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <span data-ttu-id="a3eaf-157"><a name="PublicIP"></a>6. Nyilvános IP-cím kérése</span><span class="sxs-lookup"><span data-stu-id="a3eaf-157"><a name="PublicIP"></a>6. Request a Public IP address</span></span>

<span data-ttu-id="a3eaf-158">Egy VPN Gateway-nek rendelkeznie kell nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-158">A VPN gateway must have a Public IP address.</span></span> <span data-ttu-id="a3eaf-159">Először az IP-cím típusú erőforrást kell kérnie, majd hivatkoznia kell arra, amikor létrehozza a virtuális hálózati átjárót.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-159">You first request the IP address resource, and then refer to it when creating your virtual network gateway.</span></span> <span data-ttu-id="a3eaf-160">Az IP-címet a rendszer dinamikusan rendeli hozzá az erőforráshoz a VPN Gateway létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-160">The IP address is dynamically assigned to the resource when the VPN gateway is created.</span></span> <span data-ttu-id="a3eaf-161">A VPN Gateway jelenleg csak a *Dinamikus* nyilvános IP-cím lefoglalását támogatja.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-161">VPN Gateway currently only supports *Dynamic* Public IP address allocation.</span></span> <span data-ttu-id="a3eaf-162">Nem kérheti statikus IP-cím hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-162">You cannot request a Static Public IP address assignment.</span></span> <span data-ttu-id="a3eaf-163">Ez azonban nem jelenti azt, hogy az IP-cím módosul a VPN Gateway-hez való hozzárendelése után.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-163">However, this does not mean that the IP address changes after it has been assigned to your VPN gateway.</span></span> <span data-ttu-id="a3eaf-164">A nyilvános IP-cím kizárólag abban az esetben változik, ha az átjárót törli, majd újra létrehozza.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-164">The only time the Public IP address changes is when the gateway is deleted and re-created.</span></span> <span data-ttu-id="a3eaf-165">Nem módosul átméretezés, alaphelyzetbe állítás, illetve a VPN Gateway belső karbantartása/frissítése során.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-165">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of your VPN gateway.</span></span>

<span data-ttu-id="a3eaf-166">Az [az network public-ip create](/cli/azure/network/public-ip#create) paranccsal kérhet dinamikus nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-166">Use the [az network public-ip create](/cli/azure/network/public-ip#create) command to request a Dynamic Public IP address.</span></span>

```azurecli
az network public-ip create --name VNet1GWIP --resource-group TestRG1 --allocation-method Dynamic
```

## <span data-ttu-id="a3eaf-167"><a name="CreateGateway"></a>7. A VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3eaf-167"><a name="CreateGateway"></a>7. Create the VPN gateway</span></span>

<span data-ttu-id="a3eaf-168">Hozza létre a virtuális hálózat VPN-átjáróját.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-168">Create the virtual network VPN gateway.</span></span> <span data-ttu-id="a3eaf-169">A VPN-átjáró létrehozása akár 45 percet vagy többet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-169">Creating a VPN gateway can take up to 45 minutes or more to complete.</span></span>

<span data-ttu-id="a3eaf-170">Használja a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="a3eaf-170">Use the following values:</span></span>

* <span data-ttu-id="a3eaf-171">A helyek közötti konfiguráció *--gateway-type* paraméterének értéke: *Vpn*.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-171">The *--gateway-type* for a Site-to-Site configuration is *Vpn*.</span></span> <span data-ttu-id="a3eaf-172">Az átjáró típusa mindig a kiépítendő konfigurációra jellemző.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-172">The gateway type is always specific to the configuration that you are implementing.</span></span> <span data-ttu-id="a3eaf-173">További információért lásd: [Átjárótípusok](vpn-gateway-about-vpn-gateway-settings.md#gwtype).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-173">For more information, see [Gateway types](vpn-gateway-about-vpn-gateway-settings.md#gwtype).</span></span>
* <span data-ttu-id="a3eaf-174">A *--vpn-type* a következők valamelyike lehet: *RouteBased* (egyes dokumentumokban Dinamikus átjáró néven szerepel) vagy *PolicyBased* (egyes dokumentumokban Statikus átjáró néven szerepel).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-174">The *--vpn-type* can be *RouteBased* (referred to as a Dynamic Gateway in some documentation), or *PolicyBased* (referred to as a Static Gateway in some documentation).</span></span> <span data-ttu-id="a3eaf-175">A beállítás azon eszköz követelményeire vonatkozik, amelyhez csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-175">The setting is specific to requirements of the device that you are connecting to.</span></span> <span data-ttu-id="a3eaf-176">További információk a VPN-átjárótípusokról: [Információk a VPN Gateway konfigurációs beállításairól](vpn-gateway-about-vpn-gateway-settings.md#vpntype).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-176">For more information about VPN gateway types, see [About VPN Gateway configuration settings](vpn-gateway-about-vpn-gateway-settings.md#vpntype).</span></span>
* <span data-ttu-id="a3eaf-177">Válassza ki az átjáró használni kívánt termékváltozatát.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-177">Select the Gateway SKU that you want to use.</span></span> <span data-ttu-id="a3eaf-178">Egyes termékváltozatok konfigurációs korlátokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-178">There are configuration limitations for certain SKUs.</span></span> <span data-ttu-id="a3eaf-179">További információkért lásd: [Az átjárók termékváltozatai](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-179">For more information, see [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span>

<span data-ttu-id="a3eaf-180">Hozza létre a VPN Gateway-t az [az network vnet-gateway create](/cli/azure/network/vnet-gateway#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-180">Create the VPN gateway using the [az network vnet-gateway create](/cli/azure/network/vnet-gateway#create) command.</span></span> <span data-ttu-id="a3eaf-181">Ha ezt a parancsot a „--no-wait” paraméterrel futtatja, nem jelenik meg visszajelzés vagy kimenet.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-181">If you run this command using the '--no-wait' parameter, you don't see any feedback or output.</span></span> <span data-ttu-id="a3eaf-182">Ez a paraméter lehetővé teszi, hogy az átjáró a háttérben jöjjön létre.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-182">This parameter allows the gateway to create in the background.</span></span> <span data-ttu-id="a3eaf-183">Egy átjáró létrehozása nagyjából 45 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-183">It takes around 45 minutes to create a gateway.</span></span>

```azurecli
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWIP --resource-group TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku VpnGw1 --no-wait 
```

## <span data-ttu-id="a3eaf-184"><a name="VPNDevice"></a>8. VPN-eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a3eaf-184"><a name="VPNDevice"></a>8. Configure your VPN device</span></span>

<span data-ttu-id="a3eaf-185">A helyszíni hálózaton a helyek közötti kapcsolatok létesítéséhez VPN-eszközre van szükség.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-185">Site-to-Site connections to an on-premises network require a VPN device.</span></span> <span data-ttu-id="a3eaf-186">Ebben a lépésben a VPN-eszköz konfigurálása következik.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-186">In this step, you configure your VPN device.</span></span> <span data-ttu-id="a3eaf-187">A VPN-eszköz konfigurálásakor a következőkre van szüksége:</span><span class="sxs-lookup"><span data-stu-id="a3eaf-187">When configuring your VPN device, you need the following:</span></span>

- <span data-ttu-id="a3eaf-188">Megosztott kulcs.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-188">A shared key.</span></span> <span data-ttu-id="a3eaf-189">Ez ugyanaz a megosztott kulcs, amelyet a helyek közötti VPN-kapcsolat létrehozásakor ad meg.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-189">This is the same shared key that you specify when creating your Site-to-Site VPN connection.</span></span> <span data-ttu-id="a3eaf-190">A példákban alapvető megosztott kulcsot használunk.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-190">In our examples, we use a basic shared key.</span></span> <span data-ttu-id="a3eaf-191">Javasoljuk egy ennél összetettebb kulcs létrehozását.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-191">We recommend that you generate a more complex key to use.</span></span>
- <span data-ttu-id="a3eaf-192">A virtuális hálózati átjáró nyilvános IP-címe.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-192">The Public IP address of your virtual network gateway.</span></span> <span data-ttu-id="a3eaf-193">A nyilvános IP-címet az Azure Portalon, valamint a PowerShell vagy a CLI használatával is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-193">You can view the public IP address by using the Azure portal, PowerShell, or CLI.</span></span> <span data-ttu-id="a3eaf-194">A virtuális hálózati átjáró IP-címét az [az network public-ip list](/cli/azure/network/public-ip#list) paranccsal keresheti meg.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-194">To find the public IP address of your virtual network gateway, use the [az network public-ip list](/cli/azure/network/public-ip#list) command.</span></span> <span data-ttu-id="a3eaf-195">Az olvashatóság érdekében a kimenet táblázatos formában jeleníti meg a nyilvános IP-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-195">For easy reading, the output is formatted to display the list of public IPs in table format.</span></span>

  ```azurecli
  az network public-ip list --resource-group TestRG1 --output table
  ```


[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <span data-ttu-id="a3eaf-196"><a name="CreateConnection"></a>9. VPN-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3eaf-196"><a name="CreateConnection"></a>9. Create the VPN connection</span></span>

<span data-ttu-id="a3eaf-197">Hozzon létre egy helyek közötti VPN-kapcsolatot a virtuális hálózati átjáró és a helyszíni VPN-eszköz között.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-197">Create the Site-to-Site VPN connection between your virtual network gateway and your on-premises VPN device.</span></span> <span data-ttu-id="a3eaf-198">Különösen figyeljen oda a megosztott kulcs értékére, amelynek meg kell egyeznie a VPN-eszköz konfigurált megosztottkulcs-értékével.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-198">Pay particular attention to the shared key value, which must match the configured shared key value for your VPN device.</span></span>

<span data-ttu-id="a3eaf-199">Hozza létre a kapcsolatot az [az network vnet-connection create](/cli/azure/network/vpn-connection#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-199">Create the connection using the [az network vpn-connection create](/cli/azure/network/vpn-connection#create) command.</span></span>

```azurecli
az network vpn-connection create --name VNet1toSite2 -resource-group TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

<span data-ttu-id="a3eaf-200">A kapcsolat rövid időn belül létrejön.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-200">After a short while, the connection will be established.</span></span>

## <span data-ttu-id="a3eaf-201"><a name="toverify"></a>10. A VPN-kapcsolat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a3eaf-201"><a name="toverify"></a>10. Verify the VPN connection</span></span>

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

<span data-ttu-id="a3eaf-202">Ha másik módszerrel kívánja ellenőrizni a kapcsolatot: [VPN Gateway-kapcsolat ellenőrzése](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-202">If you want to use another method to verify your connection, see [Verify a VPN Gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>

## <span data-ttu-id="a3eaf-203"><a name="connectVM"></a>Csatlakozás virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="a3eaf-203"><a name="connectVM"></a>To connect to a virtual machine</span></span>

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <span data-ttu-id="a3eaf-204"><a name="tasks"></a>Gyakori feladatok</span><span class="sxs-lookup"><span data-stu-id="a3eaf-204"><a name="tasks"></a>Common tasks</span></span>

<span data-ttu-id="a3eaf-205">Ez a szakasz a helyek közötti konfigurációk használatakor hasznos gyakori parancsokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-205">This section contains common commands that are helpful when working with site-to-site configurations.</span></span> <span data-ttu-id="a3eaf-206">A CLI hálózati parancsainak teljes listájáért lásd az [Azure CLI-hálózatkezelést](/cli/azure/network) bemutató cikket.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-206">For the full list of CLI networking commands, see [Azure CLI - Networking](/cli/azure/network).</span></span>

[!INCLUDE [local network gateway common tasks](../../includes/vpn-gateway-common-tasks-cli-include.md)]

## <a name="next-steps"></a><span data-ttu-id="a3eaf-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3eaf-207">Next steps</span></span>

* <span data-ttu-id="a3eaf-208">Miután a kapcsolat létrejött, hozzáadhat virtuális gépeket a virtuális hálózataihoz.</span><span class="sxs-lookup"><span data-stu-id="a3eaf-208">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="a3eaf-209">További információkért lásd: [Virtuális gépek](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-209">For more information, see [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="a3eaf-210">Információk a BGP-ről: [A BGP áttekintése](vpn-gateway-bgp-overview.md) és [A BGP konfigurálása](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-210">For information about BGP, see the [BGP Overview](vpn-gateway-bgp-overview.md) and [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
* <span data-ttu-id="a3eaf-211">Információk a kényszerített bújtatásról: [Információk a kényszerített bújtatásról](vpn-gateway-forced-tunneling-rm.md).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-211">For information about Forced Tunneling, see [About Forced Tunneling](vpn-gateway-forced-tunneling-rm.md).</span></span>
* <span data-ttu-id="a3eaf-212">Információk a magas rendelkezésre állású aktív-aktív kapcsolatokról: [Magas rendelkezésre állású kapcsolatok létesítmények, illetve virtuális hálózatok között](vpn-gateway-highlyavailable.md).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-212">For information about Highly Available Active-Active connections, see [Highly Available cross-premises and VNet-to-VNet connectivity](vpn-gateway-highlyavailable.md).</span></span>
* <span data-ttu-id="a3eaf-213">Az Azure CLI hálózati parancsainak listáját lásd: [Azure CLI](https://docs.microsoft.com/cli/azure/network).</span><span class="sxs-lookup"><span data-stu-id="a3eaf-213">For a list of networking Azure CLI commands, see [Azure CLI](https://docs.microsoft.com/cli/azure/network).</span></span>