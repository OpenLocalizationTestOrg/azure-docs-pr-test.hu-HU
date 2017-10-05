---
title: "Virtuális hálózat csatlakoztatása másik virtuális hálózathoz: Azure CLI | Microsoft Docs"
description: "Ez a cikk lépésről lépésre bemutatja, hogyan csatlakoztathatók a virtuális hálózatok egymáshoz az Azure Resource Manager és az Azure CLI használatával."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0683c664-9c03-40a4-b198-a6529bf1ce8b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: ae42f661b39e8b6170fd415d758404fb33009ccc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-azure-cli"></a><span data-ttu-id="39d6b-103">Virtuális hálózatok közötti VPN Gateway-kapcsolat konfigurálása az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="39d6b-103">Configure a VNet-to-VNet VPN gateway connection using Azure CLI</span></span>

<span data-ttu-id="39d6b-104">Ebből a cikkből megtudhatja, hogyan hozható létre VPN Gateway-kapcsolat virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="39d6b-104">This article shows you how to create a VPN gateway connection between virtual networks.</span></span> <span data-ttu-id="39d6b-105">A virtuális hálózatok lehetnek azonos vagy eltérő régiókban, illetve azonos vagy eltérő előfizetésekben.</span><span class="sxs-lookup"><span data-stu-id="39d6b-105">The virtual networks can be in the same or different regions, and from the same or different subscriptions.</span></span> <span data-ttu-id="39d6b-106">Amikor különböző előfizetésekről csatlakoztat virtuális hálózatokat, az előfizetéseket nem kell társítani ugyanazzal az Active Directory-bérlővel.</span><span class="sxs-lookup"><span data-stu-id="39d6b-106">When connecting VNets from different subscriptions, the subscriptions do not need to be associated with the same Active Directory tenant.</span></span> 

<span data-ttu-id="39d6b-107">A cikkben ismertetett lépések a Resource Manager-alapú üzemi modellre vonatkoznak, és az Azure CLI-t használják.</span><span class="sxs-lookup"><span data-stu-id="39d6b-107">The steps in this article apply to the Resource Manager deployment model and use Azure CLI.</span></span> <span data-ttu-id="39d6b-108">Ezt a konfigurációt más üzembehelyezési eszközzel vagy üzemi modellel is létrehozhatja, ha egy másik lehetőséget választ az alábbi listáról:</span><span class="sxs-lookup"><span data-stu-id="39d6b-108">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="39d6b-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="39d6b-109">Azure portal</span></span>](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [<span data-ttu-id="39d6b-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="39d6b-110">PowerShell</span></span>](vpn-gateway-vnet-vnet-rm-ps.md)
> * [<span data-ttu-id="39d6b-111">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="39d6b-111">Azure CLI</span></span>](vpn-gateway-howto-vnet-vnet-cli.md)
> * [<span data-ttu-id="39d6b-112">(Klasszikus) Azure Portal</span><span class="sxs-lookup"><span data-stu-id="39d6b-112">Azure portal (classic)</span></span>](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [<span data-ttu-id="39d6b-113">Különböző üzemi modellek összekapcsolása – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="39d6b-113">Connect different deployment models - Azure portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="39d6b-114">Különböző üzemi modellek összekapcsolása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="39d6b-114">Connect different deployment models - PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

<span data-ttu-id="39d6b-115">Egy virtuális hálózat egy másikkal való összekapcsolása (a virtuális hálózatok közötti kapcsolat) nagyon hasonlít egy virtuális hálózat egy helyszíni helyhez való csatlakoztatásához.</span><span class="sxs-lookup"><span data-stu-id="39d6b-115">Connecting a virtual network to another virtual network (VNet-to-VNet) is similar to connecting a VNet to an on-premises site location.</span></span> <span data-ttu-id="39d6b-116">Mindkét kapcsolattípus egy VPN-átjárót használ a biztonságos alagút IPsec/IKE használatával való kialakításához.</span><span class="sxs-lookup"><span data-stu-id="39d6b-116">Both connectivity types use a VPN gateway to provide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="39d6b-117">Ha a virtuális hálózatai azonos régióban találhatóak, fontolja meg az összekötésüket virtuális hálózatok közötti társviszony útján.</span><span class="sxs-lookup"><span data-stu-id="39d6b-117">If your VNets are in the same region, you may want to consider connecting them using VNet Peering.</span></span> <span data-ttu-id="39d6b-118">A virtuális hálózatok közötti társviszony nem használ VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="39d6b-118">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="39d6b-119">További információ: [Társviszony létesítése virtuális hálózatok között](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="39d6b-119">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span>

<span data-ttu-id="39d6b-120">A virtuális hálózatok közötti kommunikáció kombinálható többhelyes konfigurációkkal.</span><span class="sxs-lookup"><span data-stu-id="39d6b-120">VNet-to-VNet communication can be combined with multi-site configurations.</span></span> <span data-ttu-id="39d6b-121">Így létrehozhat olyan hálózati topológiákat, amelyek a létesítmények közötti kapcsolatokat a virtuális hálózatok közötti kapcsolatokkal egyesítik, ahogyan azt a következő diagram mutatja:</span><span class="sxs-lookup"><span data-stu-id="39d6b-121">This lets you establish network topologies that combine cross-premises connectivity with inter-virtual network connectivity, as shown in the following diagram:</span></span>

![Tudnivalók a kapcsolatokról](./media/vpn-gateway-howto-vnet-vnet-cli/aboutconnections.png)

### <span data-ttu-id="39d6b-123"><a name="why"></a>Miért érdemes összekapcsolni a virtuális hálózatokat?</span><span class="sxs-lookup"><span data-stu-id="39d6b-123"><a name="why"></a>Why connect virtual networks?</span></span>

<span data-ttu-id="39d6b-124">A virtuális hálózatokat a következő okokból érdemes összekapcsolni:</span><span class="sxs-lookup"><span data-stu-id="39d6b-124">You may want to connect virtual networks for the following reasons:</span></span>

* <span data-ttu-id="39d6b-125">**Georedundancia és földrajzi jelenlét több régióban**</span><span class="sxs-lookup"><span data-stu-id="39d6b-125">**Cross region geo-redundancy and geo-presence**</span></span>

  * <span data-ttu-id="39d6b-126">Beállíthatja a saját georeplikációját vagy szinkronizálását biztonságos kapcsolaton át, internetes végpontok használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="39d6b-126">You can set up your own geo-replication or synchronization with secure connectivity without going over Internet-facing endpoints.</span></span>
  * <span data-ttu-id="39d6b-127">Az Azure Traffic Manager és a Load Balancer segítségével magas rendelkezésre állású munkaterhelést állíthat be georedundanciával több Azure-régióban.</span><span class="sxs-lookup"><span data-stu-id="39d6b-127">With Azure Traffic Manager and Load Balancer, you can set up highly available workload with geo-redundancy across multiple Azure regions.</span></span> <span data-ttu-id="39d6b-128">Például beállíthat folyamatosan működő SQL-t több Azure-régióban található rendelkezésre állási csoportokkal.</span><span class="sxs-lookup"><span data-stu-id="39d6b-128">One important example is to set up SQL Always On with Availability Groups spreading across multiple Azure regions.</span></span>
* <span data-ttu-id="39d6b-129">**Regionális többrétegű alkalmazások elkülönítéssel vagy felügyeleti határral**</span><span class="sxs-lookup"><span data-stu-id="39d6b-129">**Regional multi-tier applications with isolation or administrative boundary**</span></span>

  * <span data-ttu-id="39d6b-130">Egy régión belül beállíthat többrétegű alkalmazásokat több, elkülönítéssel vagy felügyeleti követelményekkel összekapcsolt virtuális hálózatokkal.</span><span class="sxs-lookup"><span data-stu-id="39d6b-130">Within the same region, you can set up multi-tier applications with multiple virtual networks connected together due to isolation or administrative requirements.</span></span>

<span data-ttu-id="39d6b-131">A virtuális hálózatok közötti kapcsolatokról további információt a cikk végén, a [Virtuális hálózatok közötti kapcsolat – gyakori kérdések](#faq) című részben talál.</span><span class="sxs-lookup"><span data-stu-id="39d6b-131">For more information about VNet-to-VNet connections, see the [VNet-to-VNet FAQ](#faq) at the end of this article.</span></span>

### <a name="which-set-of-steps-should-i-use"></a><span data-ttu-id="39d6b-132">Melyik eljárást használjam?</span><span class="sxs-lookup"><span data-stu-id="39d6b-132">Which set of steps should I use?</span></span>

<span data-ttu-id="39d6b-133">Ebben a cikkben kétféle lépéssorozatot láthat.</span><span class="sxs-lookup"><span data-stu-id="39d6b-133">In this article, you see two different sets of steps.</span></span> <span data-ttu-id="39d6b-134">Az egyik lépéssorozat az [azonos előfizetésben található virtuális hálózatokra](#samesub), a másik az [eltérő előfizetésekben található virtuális hálózatokra](#difsub) vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="39d6b-134">One set of steps for [VNets that reside in the same subscription](#samesub), and another for [VNets that reside in different subscriptions](#difsub).</span></span>

## <span data-ttu-id="39d6b-135"><a name="samesub"></a>Azonos előfizetésben található virtuális hálózatok összekapcsolása</span><span class="sxs-lookup"><span data-stu-id="39d6b-135"><a name="samesub"></a>Connect VNets that are in the same subscription</span></span>

![v2v ábra](./media/vpn-gateway-howto-vnet-vnet-cli/v2vrmps.png)

### <a name="before-you-begin"></a><span data-ttu-id="39d6b-137">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="39d6b-137">Before you begin</span></span>

<span data-ttu-id="39d6b-138">A folyamat elkezdése előtt telepítse a CLI-parancsok legújabb verzióit (2.0-s vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="39d6b-138">Before beginning, install the latest version of the CLI commands (2.0 or later).</span></span> <span data-ttu-id="39d6b-139">Információk a CLI-parancsok telepítéséről: [Az Azure CLI 2.0-s verziójának telepítése](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="39d6b-139">For information about installing the CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

### <span data-ttu-id="39d6b-140"><a name="Plan"></a>Az IP-címtartományok megtervezése</span><span class="sxs-lookup"><span data-stu-id="39d6b-140"><a name="Plan"></a>Plan your IP address ranges</span></span>

<span data-ttu-id="39d6b-141">A következő lépések során két virtuális hálózatot fogunk létrehozni az azokhoz tartozó átjáróalhálózatokkal és konfigurációkkal.</span><span class="sxs-lookup"><span data-stu-id="39d6b-141">In the following steps, we create two virtual networks along with their respective gateway subnets and configurations.</span></span> <span data-ttu-id="39d6b-142">Ezután létrehozunk egy VPN-kapcsolatot a két virtuális hálózat között.</span><span class="sxs-lookup"><span data-stu-id="39d6b-142">We then create a VPN connection between the two VNets.</span></span> <span data-ttu-id="39d6b-143">Fontos, hogy megtervezze a hálózati konfiguráció IP-címtartományát.</span><span class="sxs-lookup"><span data-stu-id="39d6b-143">It’s important to plan the IP address ranges for your network configuration.</span></span> <span data-ttu-id="39d6b-144">Ügyeljen arra, hogy a virtuális hálózata tartományai és a helyi hálózata tartományai ne legyenek átfedésben egymással.</span><span class="sxs-lookup"><span data-stu-id="39d6b-144">Keep in mind that you must make sure that none of your VNet ranges or local network ranges overlap in any way.</span></span> <span data-ttu-id="39d6b-145">Ezekben a példákban nem szerepel DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="39d6b-145">In these examples, we do not include a DNS server.</span></span> <span data-ttu-id="39d6b-146">Ha névfeloldással szeretné ellátni a virtuális hálózatokat, tekintse meg a [Névfeloldás](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="39d6b-146">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

<span data-ttu-id="39d6b-147">A példákban a következő értékeket használjuk:</span><span class="sxs-lookup"><span data-stu-id="39d6b-147">We use the following values in the examples:</span></span>

<span data-ttu-id="39d6b-148">**Értékek a TestVNet1-hez:**</span><span class="sxs-lookup"><span data-stu-id="39d6b-148">**Values for TestVNet1:**</span></span>

* <span data-ttu-id="39d6b-149">Virtuális hálózat neve: TestVNet1</span><span class="sxs-lookup"><span data-stu-id="39d6b-149">VNet Name: TestVNet1</span></span>
* <span data-ttu-id="39d6b-150">Erőforráscsoport: TestRG1</span><span class="sxs-lookup"><span data-stu-id="39d6b-150">Resource Group: TestRG1</span></span>
* <span data-ttu-id="39d6b-151">Hely: East US</span><span class="sxs-lookup"><span data-stu-id="39d6b-151">Location: East US</span></span>
* <span data-ttu-id="39d6b-152">TestVNet1: 10.11.0.0/16 és 10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="39d6b-152">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span></span>
* <span data-ttu-id="39d6b-153">Előtér: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="39d6b-153">FrontEnd: 10.11.0.0/24</span></span>
* <span data-ttu-id="39d6b-154">Háttér: 10.12.0.0/24</span><span class="sxs-lookup"><span data-stu-id="39d6b-154">BackEnd: 10.12.0.0/24</span></span>
* <span data-ttu-id="39d6b-155">Átjáróalhálózat: 10.12.255.0/27</span><span class="sxs-lookup"><span data-stu-id="39d6b-155">GatewaySubnet: 10.12.255.0/27</span></span>
* <span data-ttu-id="39d6b-156">Átjáró neve: VNet1GW</span><span class="sxs-lookup"><span data-stu-id="39d6b-156">GatewayName: VNet1GW</span></span>
* <span data-ttu-id="39d6b-157">Nyilvános IP-cím: VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="39d6b-157">Public IP: VNet1GWIP</span></span>
* <span data-ttu-id="39d6b-158">VPN típusa: RouteBased</span><span class="sxs-lookup"><span data-stu-id="39d6b-158">VPNType: RouteBased</span></span>
* <span data-ttu-id="39d6b-159">Kapcsolat (1–4): VNet1toVNet4</span><span class="sxs-lookup"><span data-stu-id="39d6b-159">Connection(1to4): VNet1toVNet4</span></span>
* <span data-ttu-id="39d6b-160">Kapcsolat (1–5): VNet1toVNet5</span><span class="sxs-lookup"><span data-stu-id="39d6b-160">Connection(1to5): VNet1toVNet5</span></span>
* <span data-ttu-id="39d6b-161">Kapcsolat típusa: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="39d6b-161">ConnectionType: VNet2VNet</span></span>

<span data-ttu-id="39d6b-162">**Értékek a TestVNet4-hez:**</span><span class="sxs-lookup"><span data-stu-id="39d6b-162">**Values for TestVNet4:**</span></span>

* <span data-ttu-id="39d6b-163">Virtuális hálózat neve: TestVNet4</span><span class="sxs-lookup"><span data-stu-id="39d6b-163">VNet Name: TestVNet4</span></span>
* <span data-ttu-id="39d6b-164">TestVNet2: 10.41.0.0/16 és 10.42.0.0/16</span><span class="sxs-lookup"><span data-stu-id="39d6b-164">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span></span>
* <span data-ttu-id="39d6b-165">Előtér: 10.41.0.0/24</span><span class="sxs-lookup"><span data-stu-id="39d6b-165">FrontEnd: 10.41.0.0/24</span></span>
* <span data-ttu-id="39d6b-166">Háttér: 10.42.0.0/24</span><span class="sxs-lookup"><span data-stu-id="39d6b-166">BackEnd: 10.42.0.0/24</span></span>
* <span data-ttu-id="39d6b-167">Átjáróalhálózat: 10.42.255.0/27</span><span class="sxs-lookup"><span data-stu-id="39d6b-167">GatewaySubnet: 10.42.255.0/27</span></span>
* <span data-ttu-id="39d6b-168">Erőforráscsoport: TestRG4</span><span class="sxs-lookup"><span data-stu-id="39d6b-168">Resource Group: TestRG4</span></span>
* <span data-ttu-id="39d6b-169">Hely: West US</span><span class="sxs-lookup"><span data-stu-id="39d6b-169">Location: West US</span></span>
* <span data-ttu-id="39d6b-170">Átjáró neve: VNet4GW</span><span class="sxs-lookup"><span data-stu-id="39d6b-170">GatewayName: VNet4GW</span></span>
* <span data-ttu-id="39d6b-171">Nyilvános IP-cím: VNet4GWIP</span><span class="sxs-lookup"><span data-stu-id="39d6b-171">Public IP: VNet4GWIP</span></span>
* <span data-ttu-id="39d6b-172">VPN típusa: RouteBased</span><span class="sxs-lookup"><span data-stu-id="39d6b-172">VPNType: RouteBased</span></span>
* <span data-ttu-id="39d6b-173">Kapcsolat: VNet4toVNet1</span><span class="sxs-lookup"><span data-stu-id="39d6b-173">Connection: VNet4toVNet1</span></span>
* <span data-ttu-id="39d6b-174">Kapcsolat típusa: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="39d6b-174">ConnectionType: VNet2VNet</span></span>


### <span data-ttu-id="39d6b-175"><a name="Connect"></a>1. lépés – Csatlakozás az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="39d6b-175"><a name="Connect"></a>Step 1 - Connect to your subscription</span></span>

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-numbers-include.md)]

### <span data-ttu-id="39d6b-176"><a name="TestVNet1"></a>2. lépés – A TestVNet1 létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="39d6b-176"><a name="TestVNet1"></a>Step 2 - Create and configure TestVNet1</span></span>

1. <span data-ttu-id="39d6b-177">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="39d6b-177">Create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG1  -l eastus
  ```
2. <span data-ttu-id="39d6b-178">Hozza létre a TestVNet1-et és az alhálózatait.</span><span class="sxs-lookup"><span data-stu-id="39d6b-178">Create TestVNet1 and the subnets for TestVNet1.</span></span> <span data-ttu-id="39d6b-179">Ez a példa létrehoz egy TestVNet1 nevű virtuális hálózatot és egy FrontEnd nevű alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="39d6b-179">This example creates a virtual network named TestVNet1 and a subnet named FrontEnd.</span></span>

  ```azurecli
  az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24
  ```
3. <span data-ttu-id="39d6b-180">Hozzon létre egy új címteret a háttérrendszer alhálózatának.</span><span class="sxs-lookup"><span data-stu-id="39d6b-180">Create an additional address space for the backend subnet.</span></span> <span data-ttu-id="39d6b-181">Megfigyelheti, hogy ebben a lépésben a korábban létrehozott címteret és a hozzáadni kívánt új címteret is meghatározzuk.</span><span class="sxs-lookup"><span data-stu-id="39d6b-181">Notice that in this step, we specify both the address space that we created earlier, and the additional address space that we want to add.</span></span> <span data-ttu-id="39d6b-182">Ennek az az oka, hogy az [az network vnet update](https://docs.microsoft.com/cli/azure/network/vnet#update) parancs felülírja az előző beállításokat.</span><span class="sxs-lookup"><span data-stu-id="39d6b-182">This is because the [az network vnet update](https://docs.microsoft.com/cli/azure/network/vnet#update) command overwrites the previous settings.</span></span> <span data-ttu-id="39d6b-183">Győződjön meg róla, hogy a parancs használatakor az összes címelőtagot meghatározza.</span><span class="sxs-lookup"><span data-stu-id="39d6b-183">Make sure to specify all of the address prefixes when using this command.</span></span>

  ```azurecli
  az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestRG1
  ```
4. <span data-ttu-id="39d6b-184">Hozza létre a háttérrendszer alhálózatát.</span><span class="sxs-lookup"><span data-stu-id="39d6b-184">Create the backend subnet.</span></span>
  
  ```azurecli
  az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestRG1 --address-prefix 10.12.0.0/24 
  ```
5. <span data-ttu-id="39d6b-185">Hozza létre az átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="39d6b-185">Create the gateway subnet.</span></span> <span data-ttu-id="39d6b-186">Figyelje meg, hogy az átjáró alhálózatának neve „GatewaySubnet”.</span><span class="sxs-lookup"><span data-stu-id="39d6b-186">Notice that the gateway subnet is named 'GatewaySubnet'.</span></span> <span data-ttu-id="39d6b-187">A név megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="39d6b-187">This name is required.</span></span> <span data-ttu-id="39d6b-188">A példában az átjáróalhálózat /27-es alhálózatot használ.</span><span class="sxs-lookup"><span data-stu-id="39d6b-188">In this example, the gateway subnet is using a /27.</span></span> <span data-ttu-id="39d6b-189">Bár lehetséges akár /29-es átjáróalhálózatot is létrehozni, javasolt egy ennél nagyobb, több címmel rendelkező alhálózatot létrehozni: legalább /28-asat vagy /27-eset.</span><span class="sxs-lookup"><span data-stu-id="39d6b-189">While it is possible to create a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting at least /28 or /27.</span></span> <span data-ttu-id="39d6b-190">Ez elegendő címet biztosít az esetleges későbbi konfigurációkhoz.</span><span class="sxs-lookup"><span data-stu-id="39d6b-190">This will allow for enough addresses to accommodate possible additional configurations that you may want in the future.</span></span>

  ```azurecli 
  az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestRG1 --address-prefix 10.12.255.0/27
  ```
6. <span data-ttu-id="39d6b-191">Kérje egy nyilvános IP-cím kiosztását a virtuális hálózat számára létrehozni kívánt átjáróhoz.</span><span class="sxs-lookup"><span data-stu-id="39d6b-191">Request a public IP address to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="39d6b-192">Vegye figyelembe, hogy az AllocationMethod értéke Dynamic.</span><span class="sxs-lookup"><span data-stu-id="39d6b-192">Notice that the AllocationMethod is Dynamic.</span></span> <span data-ttu-id="39d6b-193">A használni kívánt IP-címet nem adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="39d6b-193">You cannot specify the IP address that you want to use.</span></span> <span data-ttu-id="39d6b-194">Ennek kiosztása az átjáró számára dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="39d6b-194">It's dynamically allocated to your gateway.</span></span>

  ```azurecli
  az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
  ```
7. <span data-ttu-id="39d6b-195">Hozza létre a TestVNet1 virtuális hálózati átjáróját.</span><span class="sxs-lookup"><span data-stu-id="39d6b-195">Create the virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="39d6b-196">A virtuális hálózatok közötti konfigurációkhoz a VpnType paraméternek RouteBased értékűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="39d6b-196">VNet-to-VNet configurations require a RouteBased VpnType.</span></span> <span data-ttu-id="39d6b-197">Ha ezt a parancsot a „--no-wait” paraméterrel futtatja, nem jelenik meg visszajelzés vagy kimenet.</span><span class="sxs-lookup"><span data-stu-id="39d6b-197">If you run this command using the '--no-wait' parameter, you don't see any feedback or output.</span></span> <span data-ttu-id="39d6b-198">A „--no-wait” paraméter lehetővé teszi, hogy az átjáró a háttérben jöjjön létre.</span><span class="sxs-lookup"><span data-stu-id="39d6b-198">The '--no-wait' parameter allows the gateway to create in the background.</span></span> <span data-ttu-id="39d6b-199">Ez nem jelenti azt, hogy a VPN-átjáró azonnal elkészült.</span><span class="sxs-lookup"><span data-stu-id="39d6b-199">It does not mean that the VPN gateway finishes creating immediately.</span></span> <span data-ttu-id="39d6b-200">Az átjáró létrehozása akár 45 percet is igénybe vehet, az átjáró termékváltozatától függően.</span><span class="sxs-lookup"><span data-stu-id="39d6b-200">Creating a gateway can often take 45 minutes or more, depending on the gateway SKU that you use.</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="39d6b-201"><a name="TestVNet4"></a>3. lépés – A TestVNet4 létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="39d6b-201"><a name="TestVNet4"></a>Step 3 - Create and configure TestVNet4</span></span>

1. <span data-ttu-id="39d6b-202">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="39d6b-202">Create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG4  -l westus
  ```
2. <span data-ttu-id="39d6b-203">Hozza létre a TestVNet4-et.</span><span class="sxs-lookup"><span data-stu-id="39d6b-203">Create TestVNet4.</span></span>

  ```azurecli
  az network vnet create -n TestVNet4 -g TestRG4 --address-prefix 10.41.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.41.0.0/24
  ```

3. <span data-ttu-id="39d6b-204">Hozzon létre további alhálózatokat a TestVNet4-hez.</span><span class="sxs-lookup"><span data-stu-id="39d6b-204">Create additional subnets for TestVNet4.</span></span>

  ```azurecli
  az network vnet update -n TestVNet4 --address-prefixes 10.41.0.0/16 10.42.0.0/16 -g TestRG4 
  az network vnet subnet create --vnet-name TestVNet4 -n BackEnd -g TestRG4 --address-prefix 10.42.0.0/24 
  ```
4. <span data-ttu-id="39d6b-205">Hozza létre az átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="39d6b-205">Create the gateway subnet.</span></span>

  ```azurecli
   az network vnet subnet create --vnet-name TestVNet4 -n GatewaySubnet -g TestRG4 --address-prefix 10.42.255.0/27
  ```
5. <span data-ttu-id="39d6b-206">Kérjen egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="39d6b-206">Request a Public IP address.</span></span>

  ```azurecli
  az network public-ip create -n VNet4GWIP -g TestRG4 --allocation-method Dynamic
  ```
6. <span data-ttu-id="39d6b-207">Hozza létre a TestVNet4 virtuális hálózati átjáróját.</span><span class="sxs-lookup"><span data-stu-id="39d6b-207">Create the TestVNet4 virtual network gateway.</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet4GW -l westus --public-ip-address VNet4GWIP -g TestRG4 --vnet TestVNet4 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="39d6b-208"><a name="createconnect"></a>4. lépés – A kapcsolatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="39d6b-208"><a name="createconnect"></a>Step 4 - Create the connections</span></span>

<span data-ttu-id="39d6b-209">Most már két, VPN-átjáróval rendelkező virtuális hálózata van.</span><span class="sxs-lookup"><span data-stu-id="39d6b-209">You now have two VNets with VPN gateways.</span></span> <span data-ttu-id="39d6b-210">A következő lépésként létre kell hoznia a VPN-átjáró kapcsolatait a virtuális hálózat átjárói között.</span><span class="sxs-lookup"><span data-stu-id="39d6b-210">The next step is to create VPN gateway connections between the virtual network gateways.</span></span> <span data-ttu-id="39d6b-211">Ha a fenti példákat követte, a virtuális hálózat átjárói eltérő erőforráscsoportokban találhatók.</span><span class="sxs-lookup"><span data-stu-id="39d6b-211">If you used the examples above, your VNet gateways are in different resource groups.</span></span> <span data-ttu-id="39d6b-212">Ha az átjárók eltérő erőforráscsoportokban vannak, a kapcsolatok létrehozásakor meg kell határoznia és azonosítania kell minden átjáró erőforrás-azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="39d6b-212">When gateways are in different resource groups, you need to identify and specify the resource IDs for each gateway when making a connection.</span></span> <span data-ttu-id="39d6b-213">Ha a virtuális hálózatok ugyanabban az erőforráscsoportban találhatók, követheti a [második útmutatót](#samerg), ugyanis nem kell meghatároznia az erőforrás-azonosítókat.</span><span class="sxs-lookup"><span data-stu-id="39d6b-213">If your VNets are in the same resource group, you can use the [second set of instructions](#samerg) because you don't need to specify the resource IDs.</span></span>

### <span data-ttu-id="39d6b-214"><a name="diffrg"></a>Eltérő erőforráscsoportokban található virtuális hálózatok csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="39d6b-214"><a name="diffrg"></a>To connect VNets that reside in different resource groups</span></span>

1. <span data-ttu-id="39d6b-215">Az alábbi parancs kimenetéből olvassa le a VNet1GW erőforrás-azonosítóját:</span><span class="sxs-lookup"><span data-stu-id="39d6b-215">Get the Resource ID of VNet1GW from the output of the following command:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  <span data-ttu-id="39d6b-216">A kimenetben keresse meg az „id:” sort.</span><span class="sxs-lookup"><span data-stu-id="39d6b-216">In the output, find the "id:" line.</span></span> <span data-ttu-id="39d6b-217">A csatlakozás a következő szakaszban való létrehozásához az idézőjelek közötti értékre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="39d6b-217">The values within the quotes are needed to create the connection in the next section.</span></span> <span data-ttu-id="39d6b-218">Másolja ezeket az értékeket egy szövegszerkesztőbe, például a Jegyzettömbbe, így a csatlakozás létrehozásakor könnyen beillesztheti őket.</span><span class="sxs-lookup"><span data-stu-id="39d6b-218">Copy these values to a text editor, such as Notepad, so that you can easily paste them when creating your connection.</span></span>

  <span data-ttu-id="39d6b-219">Példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="39d6b-219">Example output:</span></span>

  ```
  "activeActive": false, 
  "bgpSettings": { 
    "asn": 65515, 
    "bgpPeeringAddress": "10.12.255.30", 
    "peerWeight": 0 
   }, 
  "enableBgp": false, 
  "etag": "W/\"ecb42bc5-c176-44e1-802f-b0ce2962ac04\"", 
  "gatewayDefaultSite": null, 
  "gatewayType": "Vpn", 
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW", 
  "ipConfigurations":
  ```

  <span data-ttu-id="39d6b-220">Másolja ki az **„id”:** kifejezés utáni idézőjelek közti értékeket.</span><span class="sxs-lookup"><span data-stu-id="39d6b-220">Copy the values after **"id":** within the quotes.</span></span>

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
 ```

2. <span data-ttu-id="39d6b-221">Olvassa le a VNet4GW erőforrás-azonosítóját, és másolja az értékeket egy szövegszerkesztőbe.</span><span class="sxs-lookup"><span data-stu-id="39d6b-221">Get the Resource ID of VNet4GW and copy the values to a text editor.</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet4GW -g TestRG4
  ```

3. <span data-ttu-id="39d6b-222">Hozza létre a TestVNet1–TestVNet4 kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="39d6b-222">Create the TestVNet1 to TestVNet4 connection.</span></span> <span data-ttu-id="39d6b-223">Ebben a lépésben a TestVNet1 felől a TestVNet4 felé irányuló kapcsolatot hozza létre.</span><span class="sxs-lookup"><span data-stu-id="39d6b-223">In this step, you create the connection from TestVNet1 to TestVNet4.</span></span> <span data-ttu-id="39d6b-224">A példák egy megosztott kulcsra is hivatkoznak.</span><span class="sxs-lookup"><span data-stu-id="39d6b-224">There is a shared key referenced in the examples.</span></span> <span data-ttu-id="39d6b-225">A megosztott kulcshoz használhatja a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="39d6b-225">You can use your own values for the shared key.</span></span> <span data-ttu-id="39d6b-226">Fontos, hogy a megosztott kulcs azonos legyen mindkét kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="39d6b-226">The important thing is that the shared key must match for both connections.</span></span> <span data-ttu-id="39d6b-227">A kapcsolat létrehozása egy kis időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="39d6b-227">Creating a connection takes a short while to complete.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW 
  ```
4. <span data-ttu-id="39d6b-228">Hozza létre a TestVNet4–TestVNet1 kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="39d6b-228">Create the TestVNet4 to TestVNet1 connection.</span></span> <span data-ttu-id="39d6b-229">Ez a lépés az előzőhöz hasonló. A különbség annyi, hogy ezúttal a TestVNet4 felől a TestVNet1 felé irányuló kapcsolatot hozza létre.</span><span class="sxs-lookup"><span data-stu-id="39d6b-229">This step is similar to the one above, except you are creating the connection from TestVNet4 to TestVNet1.</span></span> <span data-ttu-id="39d6b-230">Ügyeljen arra, hogy a megosztott kulcsok megegyezzenek.</span><span class="sxs-lookup"><span data-stu-id="39d6b-230">Make sure the shared keys match.</span></span> <span data-ttu-id="39d6b-231">A kapcsolat létrehozása néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="39d6b-231">It takes a few minutes to establish the connection.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG4 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW -l westus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1G
  ```
5. <span data-ttu-id="39d6b-232">Ellenőrizze a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="39d6b-232">Verify your connections.</span></span> <span data-ttu-id="39d6b-233">Lásd: [A kapcsolat ellenőrzése](#verify).</span><span class="sxs-lookup"><span data-stu-id="39d6b-233">See [Verify your connection](#verify).</span></span>

### <span data-ttu-id="39d6b-234"><a name="samerg"></a>Azonos erőforráscsoportokban található virtuális hálózatok csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="39d6b-234"><a name="samerg"></a>To connect VNets that reside in the same resource group</span></span>

1. <span data-ttu-id="39d6b-235">Hozza létre a TestVNet1–TestVNet4 kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="39d6b-235">Create the TestVNet1 to TestVNet4 connection.</span></span> <span data-ttu-id="39d6b-236">Ebben a lépésben a TestVNet1 felől a TestVNet4 felé irányuló kapcsolatot hozza létre.</span><span class="sxs-lookup"><span data-stu-id="39d6b-236">In this step, you create the connection from TestVNet1 to TestVNet4.</span></span> <span data-ttu-id="39d6b-237">Figyelje meg, hogy a példák erőforráscsoportjai megegyeznek.</span><span class="sxs-lookup"><span data-stu-id="39d6b-237">Notice the resource groups are the same in the examples.</span></span> <span data-ttu-id="39d6b-238">A példák egy megosztott kulcsra is hivatkoznak.</span><span class="sxs-lookup"><span data-stu-id="39d6b-238">You also see a shared key referenced in the examples.</span></span> <span data-ttu-id="39d6b-239">A megosztott kulcshoz saját értékeket is használhat, azonban a kulcsnak mindkét kapcsolat esetében azonosnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="39d6b-239">You can use your own values for the shared key, however, the shared key must match for both connections.</span></span> <span data-ttu-id="39d6b-240">A kapcsolat létrehozása egy kis időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="39d6b-240">Creating a connection takes a short while to complete.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet4GW
  ```
2. <span data-ttu-id="39d6b-241">Hozza létre a TestVNet4–TestVNet1 kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="39d6b-241">Create the TestVNet4 to TestVNet1 connection.</span></span> <span data-ttu-id="39d6b-242">Ez a lépés az előzőhöz hasonló. A különbség annyi, hogy ezúttal a TestVNet4 felől a TestVNet1 felé irányuló kapcsolatot hozza létre.</span><span class="sxs-lookup"><span data-stu-id="39d6b-242">This step is similar to the one above, except you are creating the connection from TestVNet4 to TestVNet1.</span></span> <span data-ttu-id="39d6b-243">Ügyeljen arra, hogy a megosztott kulcsok megegyezzenek.</span><span class="sxs-lookup"><span data-stu-id="39d6b-243">Make sure the shared keys match.</span></span> <span data-ttu-id="39d6b-244">A kapcsolat létrehozása néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="39d6b-244">It takes a few minutes to establish the connection.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG1 --vnet-gateway1 VNet4GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet1GW
  ```
3. <span data-ttu-id="39d6b-245">Ellenőrizze a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="39d6b-245">Verify your connections.</span></span> <span data-ttu-id="39d6b-246">Lásd: [A kapcsolat ellenőrzése](#verify).</span><span class="sxs-lookup"><span data-stu-id="39d6b-246">See [Verify your connection](#verify).</span></span>

## <span data-ttu-id="39d6b-247"><a name="difsub"></a>Különböző előfizetésben található virtuális hálózatok összekapcsolása</span><span class="sxs-lookup"><span data-stu-id="39d6b-247"><a name="difsub"></a>Connect VNets that are in different subscriptions</span></span>

![v2v ábra](./media/vpn-gateway-howto-vnet-vnet-cli/v2vdiffsub.png)

<span data-ttu-id="39d6b-249">Ebben a forgatókönyvben csatlakoztatjuk a TestVNet1 és a TestVNet5 virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="39d6b-249">In this scenario, we connect TestVNet1 and TestVNet5.</span></span> <span data-ttu-id="39d6b-250">A virtuális hálózatok eltérő előfizetésben találhatók.</span><span class="sxs-lookup"><span data-stu-id="39d6b-250">The VNets reside different subscriptions.</span></span> <span data-ttu-id="39d6b-251">Az előfizetéseket nem kell társítani ugyanazzal az Active Directory bérlővel.</span><span class="sxs-lookup"><span data-stu-id="39d6b-251">The subscriptions do not need to be associated with the same Active Directory tenant.</span></span> <span data-ttu-id="39d6b-252">A konfiguráláshoz használt lépések egy további, virtuális hálózatok közötti kapcsolatot hoznak létre a TestVNet1 és a TestVNet5 összekapcsolásához.</span><span class="sxs-lookup"><span data-stu-id="39d6b-252">The steps for this configuration add an additional VNet-to-VNet connection in order to connect TestVNet1 to TestVNet5.</span></span>

### <span data-ttu-id="39d6b-253"><a name="TestVNet1diff"></a>5. lépés – A TestVNet1 létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="39d6b-253"><a name="TestVNet1diff"></a>Step 5 - Create and configure TestVNet1</span></span>

<span data-ttu-id="39d6b-254">Ezek az utasítások az előző szakaszok lépéseit folytatják.</span><span class="sxs-lookup"><span data-stu-id="39d6b-254">These instructions continue from the steps in the preceding sections.</span></span> <span data-ttu-id="39d6b-255">Az [1. lépés](#Connect) és a [2. lépés](#TestVNet1) elvégzésével hozza létre és konfigurálja a TestVNet1-et, valamint a TestVNet1 VPN-átjáróját.</span><span class="sxs-lookup"><span data-stu-id="39d6b-255">You must complete [Step 1](#Connect) and [Step 2](#TestVNet1) to create and configure TestVNet1 and the VPN Gateway for TestVNet1.</span></span> <span data-ttu-id="39d6b-256">Ehhez a konfigurációhoz nem kell létrehoznia az előző szakaszban a TestVNet4-et, azonban ha már megtette, az sem akadályozza az alábbi lépések végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="39d6b-256">For this configuration, you are not required to create TestVNet4 from the previous section, although if you do create it, it will not conflict with these steps.</span></span> <span data-ttu-id="39d6b-257">Miután elvégezte az 1. lépést és a 2. lépést, folytassa a 6. lépéssel (lásd alább).</span><span class="sxs-lookup"><span data-stu-id="39d6b-257">Once you complete Step 1 and Step 2, continue with Step 6 (below).</span></span>

### <span data-ttu-id="39d6b-258"><a name="verifyranges"></a>6. lépés – Az IP-címtartományok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="39d6b-258"><a name="verifyranges"></a>Step 6 - Verify the IP address ranges</span></span>

<span data-ttu-id="39d6b-259">A további kapcsolatok létrehozásakor fontos ellenőrizni, hogy az új virtuális hálózat IP-címtere ne legyen átfedésben a többi virtuális hálózati vagy hálózati átjárói tartománnyal.</span><span class="sxs-lookup"><span data-stu-id="39d6b-259">When creating additional connections, it's important to verify that the IP address space of the new virtual network does not overlap with any of your other VNet ranges or local network gateway ranges.</span></span> <span data-ttu-id="39d6b-260">A gyakorlatban a TestVNet5 hálózathoz a következő értékeket használhatja:</span><span class="sxs-lookup"><span data-stu-id="39d6b-260">For this exercise, you can use the following values for the TestVNet5:</span></span>

<span data-ttu-id="39d6b-261">**Értékek a TestVNet5-höz:**</span><span class="sxs-lookup"><span data-stu-id="39d6b-261">**Values for TestVNet5:**</span></span>

* <span data-ttu-id="39d6b-262">Virtuális hálózat neve: TestVNet5</span><span class="sxs-lookup"><span data-stu-id="39d6b-262">VNet Name: TestVNet5</span></span>
* <span data-ttu-id="39d6b-263">Erőforráscsoport: TestRG5</span><span class="sxs-lookup"><span data-stu-id="39d6b-263">Resource Group: TestRG5</span></span>
* <span data-ttu-id="39d6b-264">Hely: Japan East</span><span class="sxs-lookup"><span data-stu-id="39d6b-264">Location: Japan East</span></span>
* <span data-ttu-id="39d6b-265">TestVNet5: 10.51.0.0/16 és 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="39d6b-265">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span></span>
* <span data-ttu-id="39d6b-266">Előtér: 10.51.0.0/24</span><span class="sxs-lookup"><span data-stu-id="39d6b-266">FrontEnd: 10.51.0.0/24</span></span>
* <span data-ttu-id="39d6b-267">Háttér: 10.52.0.0/24</span><span class="sxs-lookup"><span data-stu-id="39d6b-267">BackEnd: 10.52.0.0/24</span></span>
* <span data-ttu-id="39d6b-268">Átjáróalhálózat: 10.52.255.0.0/27</span><span class="sxs-lookup"><span data-stu-id="39d6b-268">GatewaySubnet: 10.52.255.0.0/27</span></span>
* <span data-ttu-id="39d6b-269">Átjáró neve: VNet5GW</span><span class="sxs-lookup"><span data-stu-id="39d6b-269">GatewayName: VNet5GW</span></span>
* <span data-ttu-id="39d6b-270">Nyilvános IP-cím: VNet5GWIP</span><span class="sxs-lookup"><span data-stu-id="39d6b-270">Public IP: VNet5GWIP</span></span>
* <span data-ttu-id="39d6b-271">VPN típusa: RouteBased</span><span class="sxs-lookup"><span data-stu-id="39d6b-271">VPNType: RouteBased</span></span>
* <span data-ttu-id="39d6b-272">Kapcsolat: VNet5toVNet1</span><span class="sxs-lookup"><span data-stu-id="39d6b-272">Connection: VNet5toVNet1</span></span>
* <span data-ttu-id="39d6b-273">Kapcsolat típusa: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="39d6b-273">ConnectionType: VNet2VNet</span></span>

### <span data-ttu-id="39d6b-274"><a name="TestVNet5"></a>7. lépés – A TestVNet5 létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="39d6b-274"><a name="TestVNet5"></a>Step 7 - Create and configure TestVNet5</span></span>

<span data-ttu-id="39d6b-275">Ezt a lépést az új előfizetés (5. előfizetés) környezetében kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="39d6b-275">This step must be done in the context of the new subscription, Subscription 5.</span></span> <span data-ttu-id="39d6b-276">Ezt a részt azon másik szervezet rendszergazdájának kell elvégeznie, amely az előfizetés tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="39d6b-276">This part may be performed by the administrator in a different organization that owns the subscription.</span></span> <span data-ttu-id="39d6b-277">Ha váltani szeretne az előfizetések között, használja az „az account list --all” parancsot a fiókban elérhető előfizetések listájának megjelenítéséhez, majd az „az account set --subscription <subscriptionID>” parancsot a használni kívánt előfizetésre váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="39d6b-277">To switch between subscriptions use 'az account list --all' to list the subscriptions available to your account, then use 'az account set --subscription <subscriptionID>' to switch to the subscription that you want to use.</span></span>

1. <span data-ttu-id="39d6b-278">Győződjön meg róla, hogy az 5. előfizetéshez csatlakozik, majd hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="39d6b-278">Make sure you are connected to Subscription 5, then create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG5  -l japaneast
  ```
2. <span data-ttu-id="39d6b-279">Hozza létre a TestVNet5-öt.</span><span class="sxs-lookup"><span data-stu-id="39d6b-279">Create TestVNet5.</span></span>

  ```azurecli
  az network vnet create -n TestVNet5 -g TestRG5 --address-prefix 10.51.0.0/16 -l japaneast --subnet-name FrontEnd --subnet-prefix 10.51.0.0/24
  ```

3. <span data-ttu-id="39d6b-280">Adjon hozzá alhálózatokat.</span><span class="sxs-lookup"><span data-stu-id="39d6b-280">Add subnets.</span></span>

  ```azurecli
  az network vnet update -n TestVNet5 --address-prefixes 10.51.0.0/16 10.52.0.0/16 -g TestRG5
  az network vnet subnet create --vnet-name TestVNet5 -n BackEnd -g TestRG5 --address-prefix 10.52.0.0/24
  ```

4. <span data-ttu-id="39d6b-281">Adja hozzá az átjáró alhálózatát.</span><span class="sxs-lookup"><span data-stu-id="39d6b-281">Add the gateway subnet.</span></span>

  ```azurecli
  az network vnet subnet create --vnet-name TestVNet5 -n GatewaySubnet -g TestRG5 --address-prefix 10.52.255.0/27
  ```

5. <span data-ttu-id="39d6b-282">Kérjen egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="39d6b-282">Request a public IP address.</span></span>

  ```azurecli
  az network public-ip create -n VNet5GWIP -g TestRG5 --allocation-method Dynamic
  ```
6. <span data-ttu-id="39d6b-283">A TestVNet5 átjárójának létrehozása</span><span class="sxs-lookup"><span data-stu-id="39d6b-283">Create the TestVNet5 gateway</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet5GW -l japaneast --public-ip-address VNet5GWIP -g TestRG5 --vnet TestVNet5 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="39d6b-284"><a name="connections5"></a>8. lépés – A kapcsolatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="39d6b-284"><a name="connections5"></a>Step 8 - Create the connections</span></span>

<span data-ttu-id="39d6b-285">Ezt a lépést két CLI-munkamenetre osztottuk fel, amelyek jelölése **[1. előfizetés]** és **[5. előfizetés]**, mivel az átjárók eltérő előfizetésekben találhatók.</span><span class="sxs-lookup"><span data-stu-id="39d6b-285">We split this step into two CLI sessions marked as **[Subscription 1]**, and **[Subscription 5]** because the gateways are in the different subscriptions.</span></span> <span data-ttu-id="39d6b-286">Ha váltani szeretne az előfizetések között, használja az „az account list --all” parancsot a fiókban elérhető előfizetések listájának megjelenítéséhez, majd az „az account set --subscription <subscriptionID>” parancsot a használni kívánt előfizetésre váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="39d6b-286">To switch between subscriptions use 'az account list --all' to list the subscriptions available to your account, then use 'az account set --subscription <subscriptionID>' to switch to the subscription that you want to use.</span></span>

1. <span data-ttu-id="39d6b-287">**[1. előfizetés]** Jelentkezzen be, és csatlakozzon az 1. előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="39d6b-287">**[Subscription 1]** Log in and connect to Subscription 1.</span></span> <span data-ttu-id="39d6b-288">Az alábbi parancs kimenetéből olvassa le az átjáró nevét és azonosítóját:</span><span class="sxs-lookup"><span data-stu-id="39d6b-288">Run the following command to get the name and ID of the Gateway from the output:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  <span data-ttu-id="39d6b-289">Másolja ki az „id:” kifejezés kimenetét.</span><span class="sxs-lookup"><span data-stu-id="39d6b-289">Copy the output for "id:".</span></span> <span data-ttu-id="39d6b-290">Küldje el e-mailben vagy másképp a VNet-átjáró (VNet1GW) azonosítóját és nevét az 5. előfizetés rendszergazdájának.</span><span class="sxs-lookup"><span data-stu-id="39d6b-290">Send the ID and the name of the VNet gateway (VNet1GW) to the administrator of Subscription 5 via email or another method.</span></span>

  <span data-ttu-id="39d6b-291">Példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="39d6b-291">Example output:</span></span>

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
  ```

2. <span data-ttu-id="39d6b-292">**[5. előfizetés]** Jelentkezzen be, és csatlakozzon az 5. előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="39d6b-292">**[Subscription 5]** Log in and connect to Subscription 5.</span></span> <span data-ttu-id="39d6b-293">Az alábbi parancs kimenetéből olvassa le az átjáró nevét és azonosítóját:</span><span class="sxs-lookup"><span data-stu-id="39d6b-293">Run the following command to get the name and ID of the Gateway from the output:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet5GW -g TestRG5
  ```

  <span data-ttu-id="39d6b-294">Másolja ki az „id:” kifejezés kimenetét.</span><span class="sxs-lookup"><span data-stu-id="39d6b-294">Copy the output for "id:".</span></span> <span data-ttu-id="39d6b-295">Küldje el e-mailben vagy másképp a VNet-átjáró (VNet5GW) azonosítóját és nevét az 1. előfizetés rendszergazdájának.</span><span class="sxs-lookup"><span data-stu-id="39d6b-295">Send the ID and the name of the VNet gateway (VNet5GW) to the administrator of Subscription 1 via email or another method.</span></span>

3. <span data-ttu-id="39d6b-296">**[1. előfizetés]** Ebben a lépésben a TestVNet1 felől a TestVNet5 felé irányuló kapcsolatot hozza létre.</span><span class="sxs-lookup"><span data-stu-id="39d6b-296">**[Subscription 1]** In this step, you create the connection from TestVNet1 to TestVNet5.</span></span> <span data-ttu-id="39d6b-297">A megosztott kulcshoz saját értékeket is használhat, azonban a kulcsnak mindkét kapcsolat esetében azonosnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="39d6b-297">You can use your own values for the shared key, however, the shared key must match for both connections.</span></span> <span data-ttu-id="39d6b-298">A kapcsolat létrehozása egy kis időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="39d6b-298">Creating a connection can take a short while to complete.</span></span> <span data-ttu-id="39d6b-299">Kapcsolódjon az 1. előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="39d6b-299">Make sure you connect to Subscription 1.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet5 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```

4. <span data-ttu-id="39d6b-300">**[5. előfizetés]** Ez a lépés az előzőhöz hasonló. A különbség annyi, hogy ezúttal a TestVNet5 felől a TestVNet1 felé irányuló kapcsolatot hozza létre.</span><span class="sxs-lookup"><span data-stu-id="39d6b-300">**[Subscription 5]** This step is similar to the one above, except you are creating the connection from TestVNet5 to TestVNet1.</span></span> <span data-ttu-id="39d6b-301">Győződjön meg róla, hogy a megosztott kulcsok egyeznek, valamint arról, hogy az 5. előfizetéshez csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="39d6b-301">Make sure that the shared keys match and that you connect to Subscription 5.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet5ToVNet1 -g TestRG5 --vnet-gateway1 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW -l japaneast --shared-key "eeffgg" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```

## <span data-ttu-id="39d6b-302"><a name="verify"></a>A kapcsolatok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="39d6b-302"><a name="verify"></a>Verify the connections</span></span>
[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections v2v cli](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <span data-ttu-id="39d6b-303"><a name="faq"></a>Virtuális hálózatok közötti kapcsolat – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="39d6b-303"><a name="faq"></a>VNet-to-VNet FAQ</span></span>
[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="39d6b-304">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="39d6b-304">Next steps</span></span>

* <span data-ttu-id="39d6b-305">Miután a kapcsolat létrejött, hozzáadhat virtuális gépeket a virtuális hálózataihoz.</span><span class="sxs-lookup"><span data-stu-id="39d6b-305">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="39d6b-306">További információkért tekintse meg a [Virtual Machines-dokumentációt](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="39d6b-306">For more information, see the [Virtual Machines documentation](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="39d6b-307">Információk a BGP-ről: [A BGP áttekintése](vpn-gateway-bgp-overview.md) és [A BGP konfigurálása](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="39d6b-307">For information about BGP, see the [BGP Overview](vpn-gateway-bgp-overview.md) and [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
