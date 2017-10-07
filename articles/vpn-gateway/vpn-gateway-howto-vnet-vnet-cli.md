---
title: "Csatlakozás a virtuális hálózati tooanother VNet: Azure parancssori Felülettel |} Microsoft Docs"
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
ms.openlocfilehash: 70113914bcae03c80f9ad133ff081d1cf37fc309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-azure-cli"></a><span data-ttu-id="44b2f-103">Virtuális hálózatok közötti VPN Gateway-kapcsolat konfigurálása az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="44b2f-103">Configure a VNet-to-VNet VPN gateway connection using Azure CLI</span></span>

<span data-ttu-id="44b2f-104">Ez a cikk bemutatja, hogyan toocreate virtuális hálózatok közötti VPN gateway-kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="44b2f-104">This article shows you how toocreate a VPN gateway connection between virtual networks.</span></span> <span data-ttu-id="44b2f-105">hello virtuális hálózatok lehetnek azonos vagy különböző régiókban hello, és a hello azonos vagy különböző előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="44b2f-105">hello virtual networks can be in hello same or different regions, and from hello same or different subscriptions.</span></span> <span data-ttu-id="44b2f-106">Amikor másik előfizetésből, hello előfizetések csatlakozó Vnetek nem kell társított toobe hello azonos Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="44b2f-106">When connecting VNets from different subscriptions, hello subscriptions do not need toobe associated with hello same Active Directory tenant.</span></span> 

<span data-ttu-id="44b2f-107">a cikkben ismertetett hello toohello Resource Manager üzembe helyezési modellben vonatkoznak, és az Azure parancssori felület használata.</span><span class="sxs-lookup"><span data-stu-id="44b2f-107">hello steps in this article apply toohello Resource Manager deployment model and use Azure CLI.</span></span> <span data-ttu-id="44b2f-108">Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:</span><span class="sxs-lookup"><span data-stu-id="44b2f-108">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="44b2f-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="44b2f-109">Azure portal</span></span>](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [<span data-ttu-id="44b2f-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="44b2f-110">PowerShell</span></span>](vpn-gateway-vnet-vnet-rm-ps.md)
> * [<span data-ttu-id="44b2f-111">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="44b2f-111">Azure CLI</span></span>](vpn-gateway-howto-vnet-vnet-cli.md)
> * [<span data-ttu-id="44b2f-112">(Klasszikus) Azure Portal</span><span class="sxs-lookup"><span data-stu-id="44b2f-112">Azure portal (classic)</span></span>](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [<span data-ttu-id="44b2f-113">Különböző üzemi modellek összekapcsolása – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="44b2f-113">Connect different deployment models - Azure portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="44b2f-114">Különböző üzemi modellek összekapcsolása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="44b2f-114">Connect different deployment models - PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

<span data-ttu-id="44b2f-115">Csatlakozás a virtuális hálózati tooanother virtuális hálózatot (VNet – VNet) hasonló tooconnecting egy VNet tooan helyszíni hely.</span><span class="sxs-lookup"><span data-stu-id="44b2f-115">Connecting a virtual network tooanother virtual network (VNet-to-VNet) is similar tooconnecting a VNet tooan on-premises site location.</span></span> <span data-ttu-id="44b2f-116">Mindkét csatlakozási típusok használja a VPN-átjáró tooprovide IPsec/IKE használatával biztonságos alagutat.</span><span class="sxs-lookup"><span data-stu-id="44b2f-116">Both connectivity types use a VPN gateway tooprovide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="44b2f-117">Ha a Vnetek hello ugyanabban a régióban, érdemes lehet tooconsider csatlakoztatja őket a Vnetben társviszony-létesítés használatával.</span><span class="sxs-lookup"><span data-stu-id="44b2f-117">If your VNets are in hello same region, you may want tooconsider connecting them using VNet Peering.</span></span> <span data-ttu-id="44b2f-118">A virtuális hálózatok közötti társviszony nem használ VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="44b2f-118">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="44b2f-119">További információ: [Társviszony létesítése virtuális hálózatok között](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="44b2f-119">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span>

<span data-ttu-id="44b2f-120">A virtuális hálózatok közötti kommunikáció kombinálható többhelyes konfigurációkkal.</span><span class="sxs-lookup"><span data-stu-id="44b2f-120">VNet-to-VNet communication can be combined with multi-site configurations.</span></span> <span data-ttu-id="44b2f-121">Ez lehetővé teszi a felhasználó közötti kapcsolatot nyújthassanak közötti virtuális hálózati kapcsolattal rendelkező hálózati topológiák létrehozása a hello a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="44b2f-121">This lets you establish network topologies that combine cross-premises connectivity with inter-virtual network connectivity, as shown in hello following diagram:</span></span>

![Tudnivalók a kapcsolatokról](./media/vpn-gateway-howto-vnet-vnet-cli/aboutconnections.png)

### <span data-ttu-id="44b2f-123"><a name="why"></a>Miért érdemes összekapcsolni a virtuális hálózatokat?</span><span class="sxs-lookup"><span data-stu-id="44b2f-123"><a name="why"></a>Why connect virtual networks?</span></span>

<span data-ttu-id="44b2f-124">A következő okok miatt hello érdemes lehet tooconnect virtuális hálózatok:</span><span class="sxs-lookup"><span data-stu-id="44b2f-124">You may want tooconnect virtual networks for hello following reasons:</span></span>

* <span data-ttu-id="44b2f-125">**Georedundancia és földrajzi jelenlét több régióban**</span><span class="sxs-lookup"><span data-stu-id="44b2f-125">**Cross region geo-redundancy and geo-presence**</span></span>

  * <span data-ttu-id="44b2f-126">Beállíthatja a saját georeplikációját vagy szinkronizálását biztonságos kapcsolaton át, internetes végpontok használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="44b2f-126">You can set up your own geo-replication or synchronization with secure connectivity without going over Internet-facing endpoints.</span></span>
  * <span data-ttu-id="44b2f-127">Az Azure Traffic Manager és a Load Balancer segítségével magas rendelkezésre állású munkaterhelést állíthat be georedundanciával több Azure-régióban.</span><span class="sxs-lookup"><span data-stu-id="44b2f-127">With Azure Traffic Manager and Load Balancer, you can set up highly available workload with geo-redundancy across multiple Azure regions.</span></span> <span data-ttu-id="44b2f-128">Egy fontos példája tooset fel SQL Always On rendelkezésre állási csoportokkal több Azure-régiók keresztül terjednek.</span><span class="sxs-lookup"><span data-stu-id="44b2f-128">One important example is tooset up SQL Always On with Availability Groups spreading across multiple Azure regions.</span></span>
* <span data-ttu-id="44b2f-129">**Regionális többrétegű alkalmazások elkülönítéssel vagy felügyeleti határral**</span><span class="sxs-lookup"><span data-stu-id="44b2f-129">**Regional multi-tier applications with isolation or administrative boundary**</span></span>

  * <span data-ttu-id="44b2f-130">Belül hello azonos régióban, beállíthat több rétegből álló alkalmazások összekapcsolása tooisolation vagy felügyeleti követelmények miatt több virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="44b2f-130">Within hello same region, you can set up multi-tier applications with multiple virtual networks connected together due tooisolation or administrative requirements.</span></span>

<span data-ttu-id="44b2f-131">VNet – VNet kapcsolatokhoz kapcsolatos további információkért lásd: hello [VNet – VNet – gyakori kérdések](#faq) hello Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="44b2f-131">For more information about VNet-to-VNet connections, see hello [VNet-to-VNet FAQ](#faq) at hello end of this article.</span></span>

### <a name="which-set-of-steps-should-i-use"></a><span data-ttu-id="44b2f-132">Melyik eljárást használjam?</span><span class="sxs-lookup"><span data-stu-id="44b2f-132">Which set of steps should I use?</span></span>

<span data-ttu-id="44b2f-133">Ebben a cikkben kétféle lépéssorozatot láthat.</span><span class="sxs-lookup"><span data-stu-id="44b2f-133">In this article, you see two different sets of steps.</span></span> <span data-ttu-id="44b2f-134">Lépéseit egy készletét [lévő Vnetek hello ugyanahhoz az előfizetéshez](#samesub), majd egy másikat a [Vnetekhez különböző előfizetések lévő](#difsub).</span><span class="sxs-lookup"><span data-stu-id="44b2f-134">One set of steps for [VNets that reside in hello same subscription](#samesub), and another for [VNets that reside in different subscriptions](#difsub).</span></span>

## <span data-ttu-id="44b2f-135"><a name="samesub"></a>Csatlakozzon a hello Vnetek ugyanahhoz az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="44b2f-135"><a name="samesub"></a>Connect VNets that are in hello same subscription</span></span>

![v2v ábra](./media/vpn-gateway-howto-vnet-vnet-cli/v2vrmps.png)

### <a name="before-you-begin"></a><span data-ttu-id="44b2f-137">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="44b2f-137">Before you begin</span></span>

<span data-ttu-id="44b2f-138">Megkezdése előtt telepítse a legújabb verziójának hello hello parancssori felület parancsait (2.0-s vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="44b2f-138">Before beginning, install hello latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="44b2f-139">Hello parancssori felület parancsait telepítésével kapcsolatos információkért lásd: [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="44b2f-139">For information about installing hello CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

### <span data-ttu-id="44b2f-140"><a name="Plan"></a>Az IP-címtartományok megtervezése</span><span class="sxs-lookup"><span data-stu-id="44b2f-140"><a name="Plan"></a>Plan your IP address ranges</span></span>

<span data-ttu-id="44b2f-141">Az alábbi lépésekkel hello létrehozhatunk két virtuális hálózatok a megfelelő átjáró alhálózatok és konfigurációk mellett.</span><span class="sxs-lookup"><span data-stu-id="44b2f-141">In hello following steps, we create two virtual networks along with their respective gateway subnets and configurations.</span></span> <span data-ttu-id="44b2f-142">Majd létrehozhatunk hello közötti VPN-kapcsolat két Vnetek.</span><span class="sxs-lookup"><span data-stu-id="44b2f-142">We then create a VPN connection between hello two VNets.</span></span> <span data-ttu-id="44b2f-143">Fontos tooplan hello IP-címtartományok esetében a hálózati konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="44b2f-143">It’s important tooplan hello IP address ranges for your network configuration.</span></span> <span data-ttu-id="44b2f-144">Ügyeljen arra, hogy a virtuális hálózata tartományai és a helyi hálózata tartományai ne legyenek átfedésben egymással.</span><span class="sxs-lookup"><span data-stu-id="44b2f-144">Keep in mind that you must make sure that none of your VNet ranges or local network ranges overlap in any way.</span></span> <span data-ttu-id="44b2f-145">Ezekben a példákban nem szerepel DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="44b2f-145">In these examples, we do not include a DNS server.</span></span> <span data-ttu-id="44b2f-146">Ha névfeloldással szeretné ellátni a virtuális hálózatokat, tekintse meg a [Névfeloldás](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="44b2f-146">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

<span data-ttu-id="44b2f-147">A következő értékek hello példákban hello használjuk:</span><span class="sxs-lookup"><span data-stu-id="44b2f-147">We use hello following values in hello examples:</span></span>

<span data-ttu-id="44b2f-148">**Értékek a TestVNet1-hez:**</span><span class="sxs-lookup"><span data-stu-id="44b2f-148">**Values for TestVNet1:**</span></span>

* <span data-ttu-id="44b2f-149">Virtuális hálózat neve: TestVNet1</span><span class="sxs-lookup"><span data-stu-id="44b2f-149">VNet Name: TestVNet1</span></span>
* <span data-ttu-id="44b2f-150">Erőforráscsoport: TestRG1</span><span class="sxs-lookup"><span data-stu-id="44b2f-150">Resource Group: TestRG1</span></span>
* <span data-ttu-id="44b2f-151">Hely: East US</span><span class="sxs-lookup"><span data-stu-id="44b2f-151">Location: East US</span></span>
* <span data-ttu-id="44b2f-152">TestVNet1: 10.11.0.0/16 és 10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="44b2f-152">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span></span>
* <span data-ttu-id="44b2f-153">Előtér: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="44b2f-153">FrontEnd: 10.11.0.0/24</span></span>
* <span data-ttu-id="44b2f-154">Háttér: 10.12.0.0/24</span><span class="sxs-lookup"><span data-stu-id="44b2f-154">BackEnd: 10.12.0.0/24</span></span>
* <span data-ttu-id="44b2f-155">Átjáróalhálózat: 10.12.255.0/27</span><span class="sxs-lookup"><span data-stu-id="44b2f-155">GatewaySubnet: 10.12.255.0/27</span></span>
* <span data-ttu-id="44b2f-156">Átjáró neve: VNet1GW</span><span class="sxs-lookup"><span data-stu-id="44b2f-156">GatewayName: VNet1GW</span></span>
* <span data-ttu-id="44b2f-157">Nyilvános IP-cím: VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="44b2f-157">Public IP: VNet1GWIP</span></span>
* <span data-ttu-id="44b2f-158">VPN típusa: RouteBased</span><span class="sxs-lookup"><span data-stu-id="44b2f-158">VPNType: RouteBased</span></span>
* <span data-ttu-id="44b2f-159">Kapcsolat (1–4): VNet1toVNet4</span><span class="sxs-lookup"><span data-stu-id="44b2f-159">Connection(1to4): VNet1toVNet4</span></span>
* <span data-ttu-id="44b2f-160">Kapcsolat (1–5): VNet1toVNet5</span><span class="sxs-lookup"><span data-stu-id="44b2f-160">Connection(1to5): VNet1toVNet5</span></span>
* <span data-ttu-id="44b2f-161">Kapcsolat típusa: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="44b2f-161">ConnectionType: VNet2VNet</span></span>

<span data-ttu-id="44b2f-162">**Értékek a TestVNet4-hez:**</span><span class="sxs-lookup"><span data-stu-id="44b2f-162">**Values for TestVNet4:**</span></span>

* <span data-ttu-id="44b2f-163">Virtuális hálózat neve: TestVNet4</span><span class="sxs-lookup"><span data-stu-id="44b2f-163">VNet Name: TestVNet4</span></span>
* <span data-ttu-id="44b2f-164">TestVNet2: 10.41.0.0/16 és 10.42.0.0/16</span><span class="sxs-lookup"><span data-stu-id="44b2f-164">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span></span>
* <span data-ttu-id="44b2f-165">Előtér: 10.41.0.0/24</span><span class="sxs-lookup"><span data-stu-id="44b2f-165">FrontEnd: 10.41.0.0/24</span></span>
* <span data-ttu-id="44b2f-166">Háttér: 10.42.0.0/24</span><span class="sxs-lookup"><span data-stu-id="44b2f-166">BackEnd: 10.42.0.0/24</span></span>
* <span data-ttu-id="44b2f-167">Átjáróalhálózat: 10.42.255.0/27</span><span class="sxs-lookup"><span data-stu-id="44b2f-167">GatewaySubnet: 10.42.255.0/27</span></span>
* <span data-ttu-id="44b2f-168">Erőforráscsoport: TestRG4</span><span class="sxs-lookup"><span data-stu-id="44b2f-168">Resource Group: TestRG4</span></span>
* <span data-ttu-id="44b2f-169">Hely: West US</span><span class="sxs-lookup"><span data-stu-id="44b2f-169">Location: West US</span></span>
* <span data-ttu-id="44b2f-170">Átjáró neve: VNet4GW</span><span class="sxs-lookup"><span data-stu-id="44b2f-170">GatewayName: VNet4GW</span></span>
* <span data-ttu-id="44b2f-171">Nyilvános IP-cím: VNet4GWIP</span><span class="sxs-lookup"><span data-stu-id="44b2f-171">Public IP: VNet4GWIP</span></span>
* <span data-ttu-id="44b2f-172">VPN típusa: RouteBased</span><span class="sxs-lookup"><span data-stu-id="44b2f-172">VPNType: RouteBased</span></span>
* <span data-ttu-id="44b2f-173">Kapcsolat: VNet4toVNet1</span><span class="sxs-lookup"><span data-stu-id="44b2f-173">Connection: VNet4toVNet1</span></span>
* <span data-ttu-id="44b2f-174">Kapcsolat típusa: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="44b2f-174">ConnectionType: VNet2VNet</span></span>


### <span data-ttu-id="44b2f-175"><a name="Connect"></a>1. lépés – kapcsolódás tooyour előfizetés</span><span class="sxs-lookup"><span data-stu-id="44b2f-175"><a name="Connect"></a>Step 1 - Connect tooyour subscription</span></span>

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-numbers-include.md)]

### <span data-ttu-id="44b2f-176"><a name="TestVNet1"></a>2. lépés – A TestVNet1 létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="44b2f-176"><a name="TestVNet1"></a>Step 2 - Create and configure TestVNet1</span></span>

1. <span data-ttu-id="44b2f-177">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="44b2f-177">Create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG1  -l eastus
  ```
2. <span data-ttu-id="44b2f-178">Hozzon létre TestVNet1 és hello alhálózat TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="44b2f-178">Create TestVNet1 and hello subnets for TestVNet1.</span></span> <span data-ttu-id="44b2f-179">Ez a példa létrehoz egy TestVNet1 nevű virtuális hálózatot és egy FrontEnd nevű alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="44b2f-179">This example creates a virtual network named TestVNet1 and a subnet named FrontEnd.</span></span>

  ```azurecli
  az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24
  ```
3. <span data-ttu-id="44b2f-180">Létrehozhat egy további címtartományt a hello backend alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="44b2f-180">Create an additional address space for hello backend subnet.</span></span> <span data-ttu-id="44b2f-181">Figyelje meg, hogy ebben a lépésben azt adja meg a korábban létrehozott mindkét hello címtér, és hogy szeretnénk tooadd további címtartományok hello.</span><span class="sxs-lookup"><span data-stu-id="44b2f-181">Notice that in this step, we specify both hello address space that we created earlier, and hello additional address space that we want tooadd.</span></span> <span data-ttu-id="44b2f-182">Ennek az az oka hello [az hálózat virtuális hálózat frissítése](https://docs.microsoft.com/cli/azure/network/vnet#update) parancs felülírja az előző hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="44b2f-182">This is because hello [az network vnet update](https://docs.microsoft.com/cli/azure/network/vnet#update) command overwrites hello previous settings.</span></span> <span data-ttu-id="44b2f-183">Győződjön meg arról, hogy toospecify minden hello címelőtag a parancs használatakor.</span><span class="sxs-lookup"><span data-stu-id="44b2f-183">Make sure toospecify all of hello address prefixes when using this command.</span></span>

  ```azurecli
  az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestRG1
  ```
4. <span data-ttu-id="44b2f-184">Hozzon létre hello backend alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="44b2f-184">Create hello backend subnet.</span></span>
  
  ```azurecli
  az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestRG1 --address-prefix 10.12.0.0/24 
  ```
5. <span data-ttu-id="44b2f-185">Hozzon létre hello átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="44b2f-185">Create hello gateway subnet.</span></span> <span data-ttu-id="44b2f-186">Figyelje meg, hogy hello átjáró alhálózatának elnevezése "GatewaySubnet".</span><span class="sxs-lookup"><span data-stu-id="44b2f-186">Notice that hello gateway subnet is named 'GatewaySubnet'.</span></span> <span data-ttu-id="44b2f-187">A név megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="44b2f-187">This name is required.</span></span> <span data-ttu-id="44b2f-188">Ebben a példában hello átjáróalhálózatot egy /27 használ.</span><span class="sxs-lookup"><span data-stu-id="44b2f-188">In this example, hello gateway subnet is using a /27.</span></span> <span data-ttu-id="44b2f-189">Habár lehetséges toocreate mérete /29 legyen egy átjáró-alhálózatot, azt javasoljuk, hogy hozzon létre egy nagyobb alhálózat több címet legalább /28 vagy /27 kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="44b2f-189">While it is possible toocreate a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting at least /28 or /27.</span></span> <span data-ttu-id="44b2f-190">Ez lehetővé teszi elég címek tooaccommodate lehetséges olyan beállításokat, amelyeket érdemes lehet a hello jövőbeli.</span><span class="sxs-lookup"><span data-stu-id="44b2f-190">This will allow for enough addresses tooaccommodate possible additional configurations that you may want in hello future.</span></span>

  ```azurecli 
  az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestRG1 --address-prefix 10.12.255.0/27
  ```
6. <span data-ttu-id="44b2f-191">A nyilvános IP-cím toobe lefoglalt toohello átjáró fog létrehozni a Vnethez tartozó kérelmet.</span><span class="sxs-lookup"><span data-stu-id="44b2f-191">Request a public IP address toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="44b2f-192">Figyelje meg, hogy hello AllocationMethod dinamikus.</span><span class="sxs-lookup"><span data-stu-id="44b2f-192">Notice that hello AllocationMethod is Dynamic.</span></span> <span data-ttu-id="44b2f-193">Nem adható meg, hogy szeretné-e toouse hello IP-címet.</span><span class="sxs-lookup"><span data-stu-id="44b2f-193">You cannot specify hello IP address that you want toouse.</span></span> <span data-ttu-id="44b2f-194">Fontos a dinamikusan kiosztott tooyour átjáró.</span><span class="sxs-lookup"><span data-stu-id="44b2f-194">It's dynamically allocated tooyour gateway.</span></span>

  ```azurecli
  az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
  ```
7. <span data-ttu-id="44b2f-195">TestVNet1 hello virtuális hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="44b2f-195">Create hello virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="44b2f-196">A virtuális hálózatok közötti konfigurációkhoz a VpnType paraméternek RouteBased értékűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="44b2f-196">VNet-to-VNet configurations require a RouteBased VpnType.</span></span> <span data-ttu-id="44b2f-197">A parancs futtatásakor hello használatával – nincs - wait paramétert, ha bármely visszajelzést vagy kimeneti nem látható.</span><span class="sxs-lookup"><span data-stu-id="44b2f-197">If you run this command using hello '--no-wait' parameter, you don't see any feedback or output.</span></span> <span data-ttu-id="44b2f-198">hello – nincs - wait paramétert lehetővé teszi, hogy hello átjáró toocreate hello háttérben.</span><span class="sxs-lookup"><span data-stu-id="44b2f-198">hello '--no-wait' parameter allows hello gateway toocreate in hello background.</span></span> <span data-ttu-id="44b2f-199">Azt nem jelenti azt, hogy hello VPN-átjáró végzett a létrehozással azonnal.</span><span class="sxs-lookup"><span data-stu-id="44b2f-199">It does not mean that hello VPN gateway finishes creating immediately.</span></span> <span data-ttu-id="44b2f-200">Átjáró létrehozása gyakran eltarthat 45 percig vagy tovább hello gateway SKU, amelyekkel függően.</span><span class="sxs-lookup"><span data-stu-id="44b2f-200">Creating a gateway can often take 45 minutes or more, depending on hello gateway SKU that you use.</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="44b2f-201"><a name="TestVNet4"></a>3. lépés – A TestVNet4 létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="44b2f-201"><a name="TestVNet4"></a>Step 3 - Create and configure TestVNet4</span></span>

1. <span data-ttu-id="44b2f-202">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="44b2f-202">Create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG4  -l westus
  ```
2. <span data-ttu-id="44b2f-203">Hozza létre a TestVNet4-et.</span><span class="sxs-lookup"><span data-stu-id="44b2f-203">Create TestVNet4.</span></span>

  ```azurecli
  az network vnet create -n TestVNet4 -g TestRG4 --address-prefix 10.41.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.41.0.0/24
  ```

3. <span data-ttu-id="44b2f-204">Hozzon létre további alhálózatokat a TestVNet4-hez.</span><span class="sxs-lookup"><span data-stu-id="44b2f-204">Create additional subnets for TestVNet4.</span></span>

  ```azurecli
  az network vnet update -n TestVNet4 --address-prefixes 10.41.0.0/16 10.42.0.0/16 -g TestRG4 
  az network vnet subnet create --vnet-name TestVNet4 -n BackEnd -g TestRG4 --address-prefix 10.42.0.0/24 
  ```
4. <span data-ttu-id="44b2f-205">Hozzon létre hello átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="44b2f-205">Create hello gateway subnet.</span></span>

  ```azurecli
   az network vnet subnet create --vnet-name TestVNet4 -n GatewaySubnet -g TestRG4 --address-prefix 10.42.255.0/27
  ```
5. <span data-ttu-id="44b2f-206">Kérjen egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="44b2f-206">Request a Public IP address.</span></span>

  ```azurecli
  az network public-ip create -n VNet4GWIP -g TestRG4 --allocation-method Dynamic
  ```
6. <span data-ttu-id="44b2f-207">Hello TestVNet4 virtuális hálózati átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="44b2f-207">Create hello TestVNet4 virtual network gateway.</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet4GW -l westus --public-ip-address VNet4GWIP -g TestRG4 --vnet TestVNet4 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="44b2f-208"><a name="createconnect"></a>4. lépés – hello kapcsolatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="44b2f-208"><a name="createconnect"></a>Step 4 - Create hello connections</span></span>

<span data-ttu-id="44b2f-209">Most már két, VPN-átjáróval rendelkező virtuális hálózata van.</span><span class="sxs-lookup"><span data-stu-id="44b2f-209">You now have two VNets with VPN gateways.</span></span> <span data-ttu-id="44b2f-210">következő lépés hello toocreate VPN-átjáró kapcsolatok hello virtuális hálózati átjárók között.</span><span class="sxs-lookup"><span data-stu-id="44b2f-210">hello next step is toocreate VPN gateway connections between hello virtual network gateways.</span></span> <span data-ttu-id="44b2f-211">Ha követte a fenti példákban hello, a virtuális hálózat átjárók különböző erőforráscsoportokra szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="44b2f-211">If you used hello examples above, your VNet gateways are in different resource groups.</span></span> <span data-ttu-id="44b2f-212">Ha az átjárók különböző erőforráscsoportok vannak, ehhez tooidentify kell és hello erőforrás-azonosítók minden átjáró, ha a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="44b2f-212">When gateways are in different resource groups, you need tooidentify and specify hello resource IDs for each gateway when making a connection.</span></span> <span data-ttu-id="44b2f-213">Ha a Vnetek hello ugyanabban az erőforráscsoportban, használhatja a hello [utasításokat második együttesét](#samerg) mert toospecify hello erőforrás-azonosítók nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="44b2f-213">If your VNets are in hello same resource group, you can use hello [second set of instructions](#samerg) because you don't need toospecify hello resource IDs.</span></span>

### <span data-ttu-id="44b2f-214"><a name="diffrg"></a>tooconnect Vnetekhez különböző erőforráscsoportokra található</span><span class="sxs-lookup"><span data-stu-id="44b2f-214"><a name="diffrg"></a>tooconnect VNets that reside in different resource groups</span></span>

1. <span data-ttu-id="44b2f-215">Hello VNet1GW Azonosítójú erőforrás lekérése a következő parancs hello hello kimenete:</span><span class="sxs-lookup"><span data-stu-id="44b2f-215">Get hello Resource ID of VNet1GW from hello output of hello following command:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  <span data-ttu-id="44b2f-216">Hello kimenet, megkeresi a hello "azonosító:" sor.</span><span class="sxs-lookup"><span data-stu-id="44b2f-216">In hello output, find hello "id:" line.</span></span> <span data-ttu-id="44b2f-217">hello hello idézőjelek értékei szükséges toocreate hello kapcsolat hello a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="44b2f-217">hello values within hello quotes are needed toocreate hello connection in hello next section.</span></span> <span data-ttu-id="44b2f-218">Ezen értékek tooa szövegszerkesztőben, például a Jegyzettömbben másolja, így könnyen illessze be őket a kapcsolat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="44b2f-218">Copy these values tooa text editor, such as Notepad, so that you can easily paste them when creating your connection.</span></span>

  <span data-ttu-id="44b2f-219">Példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="44b2f-219">Example output:</span></span>

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

  <span data-ttu-id="44b2f-220">Miután hello értékek másolása **"id":** hello ajánlatok belül.</span><span class="sxs-lookup"><span data-stu-id="44b2f-220">Copy hello values after **"id":** within hello quotes.</span></span>

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
 ```

2. <span data-ttu-id="44b2f-221">Első hello VNet4GW erőforrás azonosítója, és másolja hello értékek tooa szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="44b2f-221">Get hello Resource ID of VNet4GW and copy hello values tooa text editor.</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet4GW -g TestRG4
  ```

3. <span data-ttu-id="44b2f-222">Hello TestVNet1 tooTestVNet4 kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="44b2f-222">Create hello TestVNet1 tooTestVNet4 connection.</span></span> <span data-ttu-id="44b2f-223">Ebben a lépésben készíthet TestVNet1 tooTestVNet4 hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="44b2f-223">In this step, you create hello connection from TestVNet1 tooTestVNet4.</span></span> <span data-ttu-id="44b2f-224">Nincs a megosztott kulcs hello példák hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="44b2f-224">There is a shared key referenced in hello examples.</span></span> <span data-ttu-id="44b2f-225">Hello megosztott kulcs saját értékeket is használhat.</span><span class="sxs-lookup"><span data-stu-id="44b2f-225">You can use your own values for hello shared key.</span></span> <span data-ttu-id="44b2f-226">fontos dolog hello megosztott kulcs hello egyeznie kell mindkét kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="44b2f-226">hello important thing is that hello shared key must match for both connections.</span></span> <span data-ttu-id="44b2f-227">Toocomplete közben rövid kapcsolat létrehozása időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="44b2f-227">Creating a connection takes a short while toocomplete.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW 
  ```
4. <span data-ttu-id="44b2f-228">Hello TestVNet4 tooTestVNet1 kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="44b2f-228">Create hello TestVNet4 tooTestVNet1 connection.</span></span> <span data-ttu-id="44b2f-229">Ez a lépés akkor hasonló toohello valamelyik fenti, kivéve a TestVNet4 tooTestVNet1 hello kapcsolatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="44b2f-229">This step is similar toohello one above, except you are creating hello connection from TestVNet4 tooTestVNet1.</span></span> <span data-ttu-id="44b2f-230">Ellenőrizze, hogy megosztott hello kulcsok felel meg.</span><span class="sxs-lookup"><span data-stu-id="44b2f-230">Make sure hello shared keys match.</span></span> <span data-ttu-id="44b2f-231">Ez néhány percet vesz igénybe tooestablish hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="44b2f-231">It takes a few minutes tooestablish hello connection.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG4 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW -l westus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1G
  ```
5. <span data-ttu-id="44b2f-232">Ellenőrizze a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="44b2f-232">Verify your connections.</span></span> <span data-ttu-id="44b2f-233">Lásd: [A kapcsolat ellenőrzése](#verify).</span><span class="sxs-lookup"><span data-stu-id="44b2f-233">See [Verify your connection](#verify).</span></span>

### <span data-ttu-id="44b2f-234"><a name="samerg"></a>hello megtalálható azonos Vnetek tooconnect erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="44b2f-234"><a name="samerg"></a>tooconnect VNets that reside in hello same resource group</span></span>

1. <span data-ttu-id="44b2f-235">Hello TestVNet1 tooTestVNet4 kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="44b2f-235">Create hello TestVNet1 tooTestVNet4 connection.</span></span> <span data-ttu-id="44b2f-236">Ebben a lépésben készíthet TestVNet1 tooTestVNet4 hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="44b2f-236">In this step, you create hello connection from TestVNet1 tooTestVNet4.</span></span> <span data-ttu-id="44b2f-237">Értesítés hello erőforráscsoportok vannak hello hello példákban azonos.</span><span class="sxs-lookup"><span data-stu-id="44b2f-237">Notice hello resource groups are hello same in hello examples.</span></span> <span data-ttu-id="44b2f-238">Látható egy megosztott kulcsot hivatkozott hello példákban is.</span><span class="sxs-lookup"><span data-stu-id="44b2f-238">You also see a shared key referenced in hello examples.</span></span> <span data-ttu-id="44b2f-239">Hello megosztott kulcs saját értékeket is használhat, azonban a hello megosztott kulcs egyeznie kell mindkét kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="44b2f-239">You can use your own values for hello shared key, however, hello shared key must match for both connections.</span></span> <span data-ttu-id="44b2f-240">Toocomplete közben rövid kapcsolat létrehozása időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="44b2f-240">Creating a connection takes a short while toocomplete.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet4GW
  ```
2. <span data-ttu-id="44b2f-241">Hello TestVNet4 tooTestVNet1 kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="44b2f-241">Create hello TestVNet4 tooTestVNet1 connection.</span></span> <span data-ttu-id="44b2f-242">Ez a lépés akkor hasonló toohello valamelyik fenti, kivéve a TestVNet4 tooTestVNet1 hello kapcsolatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="44b2f-242">This step is similar toohello one above, except you are creating hello connection from TestVNet4 tooTestVNet1.</span></span> <span data-ttu-id="44b2f-243">Ellenőrizze, hogy megosztott hello kulcsok felel meg.</span><span class="sxs-lookup"><span data-stu-id="44b2f-243">Make sure hello shared keys match.</span></span> <span data-ttu-id="44b2f-244">Ez néhány percet vesz igénybe tooestablish hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="44b2f-244">It takes a few minutes tooestablish hello connection.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG1 --vnet-gateway1 VNet4GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet1GW
  ```
3. <span data-ttu-id="44b2f-245">Ellenőrizze a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="44b2f-245">Verify your connections.</span></span> <span data-ttu-id="44b2f-246">Lásd: [A kapcsolat ellenőrzése](#verify).</span><span class="sxs-lookup"><span data-stu-id="44b2f-246">See [Verify your connection](#verify).</span></span>

## <span data-ttu-id="44b2f-247"><a name="difsub"></a>Különböző előfizetésben található virtuális hálózatok összekapcsolása</span><span class="sxs-lookup"><span data-stu-id="44b2f-247"><a name="difsub"></a>Connect VNets that are in different subscriptions</span></span>

![v2v ábra](./media/vpn-gateway-howto-vnet-vnet-cli/v2vdiffsub.png)

<span data-ttu-id="44b2f-249">Ebben a forgatókönyvben csatlakoztatjuk a TestVNet1 és a TestVNet5 virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="44b2f-249">In this scenario, we connect TestVNet1 and TestVNet5.</span></span> <span data-ttu-id="44b2f-250">hello Vnetekhez különböző előfizetések találhatók.</span><span class="sxs-lookup"><span data-stu-id="44b2f-250">hello VNets reside different subscriptions.</span></span> <span data-ttu-id="44b2f-251">hello előfizetések nem kell hello társított toobe azonos Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="44b2f-251">hello subscriptions do not need toobe associated with hello same Active Directory tenant.</span></span> <span data-ttu-id="44b2f-252">Ehhez a konfigurációhoz hello lépéseket rendelés tooconnect TestVNet1 tooTestVNet5 egy további VNet – VNet-kapcsolatot hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="44b2f-252">hello steps for this configuration add an additional VNet-to-VNet connection in order tooconnect TestVNet1 tooTestVNet5.</span></span>

### <span data-ttu-id="44b2f-253"><a name="TestVNet1diff"></a>5. lépés – A TestVNet1 létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="44b2f-253"><a name="TestVNet1diff"></a>Step 5 - Create and configure TestVNet1</span></span>

<span data-ttu-id="44b2f-254">Ezek az utasítások továbbra is a fentebbi szakaszokban hello hello lépéseit.</span><span class="sxs-lookup"><span data-stu-id="44b2f-254">These instructions continue from hello steps in hello preceding sections.</span></span> <span data-ttu-id="44b2f-255">Meg kell adnia a [1. lépés](#Connect) és [2. lépés](#TestVNet1) toocreate TestVNet1 hello VPN Gateway és TestVNet1 konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="44b2f-255">You must complete [Step 1](#Connect) and [Step 2](#TestVNet1) toocreate and configure TestVNet1 and hello VPN Gateway for TestVNet1.</span></span> <span data-ttu-id="44b2f-256">Ehhez a konfigurációhoz akkor használják, nem szükséges toocreate TestVNet4 az előző szakasz hello, bár hoz létre, ha ez nem ütközik ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="44b2f-256">For this configuration, you are not required toocreate TestVNet4 from hello previous section, although if you do create it, it will not conflict with these steps.</span></span> <span data-ttu-id="44b2f-257">Miután elvégezte az 1. lépést és a 2. lépést, folytassa a 6. lépéssel (lásd alább).</span><span class="sxs-lookup"><span data-stu-id="44b2f-257">Once you complete Step 1 and Step 2, continue with Step 6 (below).</span></span>

### <span data-ttu-id="44b2f-258"><a name="verifyranges"></a>6. lépés - hello IP-címtartományok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="44b2f-258"><a name="verifyranges"></a>Step 6 - Verify hello IP address ranges</span></span>

<span data-ttu-id="44b2f-259">További kapcsolatok létrehozása esetén, amelyek hello IP-címtér hello új virtuális hálózat nem fedi át a többi virtuális hálózat tartomány vagy helyi hálózati átjáró tartományok egyik fontos tooverify.</span><span class="sxs-lookup"><span data-stu-id="44b2f-259">When creating additional connections, it's important tooverify that hello IP address space of hello new virtual network does not overlap with any of your other VNet ranges or local network gateway ranges.</span></span> <span data-ttu-id="44b2f-260">Ebben a gyakorlatban a következő értékek hello TestVNet5 hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="44b2f-260">For this exercise, you can use hello following values for hello TestVNet5:</span></span>

<span data-ttu-id="44b2f-261">**Értékek a TestVNet5-höz:**</span><span class="sxs-lookup"><span data-stu-id="44b2f-261">**Values for TestVNet5:**</span></span>

* <span data-ttu-id="44b2f-262">Virtuális hálózat neve: TestVNet5</span><span class="sxs-lookup"><span data-stu-id="44b2f-262">VNet Name: TestVNet5</span></span>
* <span data-ttu-id="44b2f-263">Erőforráscsoport: TestRG5</span><span class="sxs-lookup"><span data-stu-id="44b2f-263">Resource Group: TestRG5</span></span>
* <span data-ttu-id="44b2f-264">Hely: Japan East</span><span class="sxs-lookup"><span data-stu-id="44b2f-264">Location: Japan East</span></span>
* <span data-ttu-id="44b2f-265">TestVNet5: 10.51.0.0/16 és 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="44b2f-265">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span></span>
* <span data-ttu-id="44b2f-266">Előtér: 10.51.0.0/24</span><span class="sxs-lookup"><span data-stu-id="44b2f-266">FrontEnd: 10.51.0.0/24</span></span>
* <span data-ttu-id="44b2f-267">Háttér: 10.52.0.0/24</span><span class="sxs-lookup"><span data-stu-id="44b2f-267">BackEnd: 10.52.0.0/24</span></span>
* <span data-ttu-id="44b2f-268">Átjáróalhálózat: 10.52.255.0.0/27</span><span class="sxs-lookup"><span data-stu-id="44b2f-268">GatewaySubnet: 10.52.255.0.0/27</span></span>
* <span data-ttu-id="44b2f-269">Átjáró neve: VNet5GW</span><span class="sxs-lookup"><span data-stu-id="44b2f-269">GatewayName: VNet5GW</span></span>
* <span data-ttu-id="44b2f-270">Nyilvános IP-cím: VNet5GWIP</span><span class="sxs-lookup"><span data-stu-id="44b2f-270">Public IP: VNet5GWIP</span></span>
* <span data-ttu-id="44b2f-271">VPN típusa: RouteBased</span><span class="sxs-lookup"><span data-stu-id="44b2f-271">VPNType: RouteBased</span></span>
* <span data-ttu-id="44b2f-272">Kapcsolat: VNet5toVNet1</span><span class="sxs-lookup"><span data-stu-id="44b2f-272">Connection: VNet5toVNet1</span></span>
* <span data-ttu-id="44b2f-273">Kapcsolat típusa: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="44b2f-273">ConnectionType: VNet2VNet</span></span>

### <span data-ttu-id="44b2f-274"><a name="TestVNet5"></a>7. lépés – A TestVNet5 létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="44b2f-274"><a name="TestVNet5"></a>Step 7 - Create and configure TestVNet5</span></span>

<span data-ttu-id="44b2f-275">Ez a lépés hello új előfizetés, előfizetés 5 hello környezetében kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="44b2f-275">This step must be done in hello context of hello new subscription, Subscription 5.</span></span> <span data-ttu-id="44b2f-276">Ez a kijelző hello hello előfizetés tulajdonosa egy másik szervezet rendszergazdája hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="44b2f-276">This part may be performed by hello administrator in a different organization that owns hello subscription.</span></span> <span data-ttu-id="44b2f-277">előfizetések használata között tooswitch "az fióklista--minden" toolist hello előfizetések elérhető tooyour fiókot, majd használja "az fiók beállítása – előfizetési <subscriptionID>" tooswitch toohello előfizetést, amelyet az toouse.</span><span class="sxs-lookup"><span data-stu-id="44b2f-277">tooswitch between subscriptions use 'az account list --all' toolist hello subscriptions available tooyour account, then use 'az account set --subscription <subscriptionID>' tooswitch toohello subscription that you want toouse.</span></span>

1. <span data-ttu-id="44b2f-278">Győződjön meg arról, hogy csatlakoztatott tooSubscription 5, akkor hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="44b2f-278">Make sure you are connected tooSubscription 5, then create a resource group.</span></span>

  ```azurecli
  az group create -n TestRG5  -l japaneast
  ```
2. <span data-ttu-id="44b2f-279">Hozza létre a TestVNet5-öt.</span><span class="sxs-lookup"><span data-stu-id="44b2f-279">Create TestVNet5.</span></span>

  ```azurecli
  az network vnet create -n TestVNet5 -g TestRG5 --address-prefix 10.51.0.0/16 -l japaneast --subnet-name FrontEnd --subnet-prefix 10.51.0.0/24
  ```

3. <span data-ttu-id="44b2f-280">Adjon hozzá alhálózatokat.</span><span class="sxs-lookup"><span data-stu-id="44b2f-280">Add subnets.</span></span>

  ```azurecli
  az network vnet update -n TestVNet5 --address-prefixes 10.51.0.0/16 10.52.0.0/16 -g TestRG5
  az network vnet subnet create --vnet-name TestVNet5 -n BackEnd -g TestRG5 --address-prefix 10.52.0.0/24
  ```

4. <span data-ttu-id="44b2f-281">Hello átjáró alhálózatának hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="44b2f-281">Add hello gateway subnet.</span></span>

  ```azurecli
  az network vnet subnet create --vnet-name TestVNet5 -n GatewaySubnet -g TestRG5 --address-prefix 10.52.255.0/27
  ```

5. <span data-ttu-id="44b2f-282">Kérjen egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="44b2f-282">Request a public IP address.</span></span>

  ```azurecli
  az network public-ip create -n VNet5GWIP -g TestRG5 --allocation-method Dynamic
  ```
6. <span data-ttu-id="44b2f-283">Hello TestVNet5 átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="44b2f-283">Create hello TestVNet5 gateway</span></span>

  ```azurecli
  az network vnet-gateway create -n VNet5GW -l japaneast --public-ip-address VNet5GWIP -g TestRG5 --vnet TestVNet5 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <span data-ttu-id="44b2f-284"><a name="connections5"></a>8. lépés – hello kapcsolatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="44b2f-284"><a name="connections5"></a>Step 8 - Create hello connections</span></span>

<span data-ttu-id="44b2f-285">Azt ossza fel ezt a lépést olyan jelölésű két parancssori munkamenetbe **[előfizetés 1]**, és **[előfizetés 5]** mert hello átjárók hello különböző előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="44b2f-285">We split this step into two CLI sessions marked as **[Subscription 1]**, and **[Subscription 5]** because hello gateways are in hello different subscriptions.</span></span> <span data-ttu-id="44b2f-286">előfizetések használata között tooswitch "az fióklista--minden" toolist hello előfizetések elérhető tooyour fiókot, majd használja "az fiók beállítása – előfizetési <subscriptionID>" tooswitch toohello előfizetést, amelyet az toouse.</span><span class="sxs-lookup"><span data-stu-id="44b2f-286">tooswitch between subscriptions use 'az account list --all' toolist hello subscriptions available tooyour account, then use 'az account set --subscription <subscriptionID>' tooswitch toohello subscription that you want toouse.</span></span>

1. <span data-ttu-id="44b2f-287">**[1. előfizetés]**  Jelentkezzen be, és csatlakozzon a tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="44b2f-287">**[Subscription 1]** Log in and connect tooSubscription 1.</span></span> <span data-ttu-id="44b2f-288">Futtatási hello következő parancsot a tooget hello nevét és Azonosítóját hello hello kimenetből átjáró:</span><span class="sxs-lookup"><span data-stu-id="44b2f-288">Run hello following command tooget hello name and ID of hello Gateway from hello output:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  <span data-ttu-id="44b2f-289">A hello kimenetének másolása HTML "azonosító:".</span><span class="sxs-lookup"><span data-stu-id="44b2f-289">Copy hello output for "id:".</span></span> <span data-ttu-id="44b2f-290">Küldési hello azonosítója és neve hello hello hálózatok átjáró (VNet1GW) toohello rendszergazda előfizetés 5 e-mailben vagy más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="44b2f-290">Send hello ID and hello name of hello VNet gateway (VNet1GW) toohello administrator of Subscription 5 via email or another method.</span></span>

  <span data-ttu-id="44b2f-291">Példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="44b2f-291">Example output:</span></span>

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
  ```

2. <span data-ttu-id="44b2f-292">**[Előfizetés 5]**  Jelentkezzen be, és csatlakozzon a tooSubscription 5.</span><span class="sxs-lookup"><span data-stu-id="44b2f-292">**[Subscription 5]** Log in and connect tooSubscription 5.</span></span> <span data-ttu-id="44b2f-293">Futtatási hello következő parancsot a tooget hello nevét és Azonosítóját hello hello kimenetből átjáró:</span><span class="sxs-lookup"><span data-stu-id="44b2f-293">Run hello following command tooget hello name and ID of hello Gateway from hello output:</span></span>

  ```azurecli
  az network vnet-gateway show -n VNet5GW -g TestRG5
  ```

  <span data-ttu-id="44b2f-294">A hello kimenetének másolása HTML "azonosító:".</span><span class="sxs-lookup"><span data-stu-id="44b2f-294">Copy hello output for "id:".</span></span> <span data-ttu-id="44b2f-295">Küldjön hello azonosítója és neve hello hello VNet átjáró (VNet5GW) toohello rendszergazda az előfizetés 1 e-mailben vagy más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="44b2f-295">Send hello ID and hello name of hello VNet gateway (VNet5GW) toohello administrator of Subscription 1 via email or another method.</span></span>

3. <span data-ttu-id="44b2f-296">**[1. előfizetés]**  Ebben a lépésben hoz létre hello kapcsolat TestVNet1 tooTestVNet5.</span><span class="sxs-lookup"><span data-stu-id="44b2f-296">**[Subscription 1]** In this step, you create hello connection from TestVNet1 tooTestVNet5.</span></span> <span data-ttu-id="44b2f-297">Hello megosztott kulcs saját értékeket is használhat, azonban a hello megosztott kulcs egyeznie kell mindkét kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="44b2f-297">You can use your own values for hello shared key, however, hello shared key must match for both connections.</span></span> <span data-ttu-id="44b2f-298">Kapcsolat létrehozása során toocomplete rövid is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="44b2f-298">Creating a connection can take a short while toocomplete.</span></span> <span data-ttu-id="44b2f-299">Ellenőrizze, hogy tooSubscription 1 kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="44b2f-299">Make sure you connect tooSubscription 1.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet5 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```

4. <span data-ttu-id="44b2f-300">**[Előfizetés 5]**  Ezt a lépést nem hasonló toohello valamelyik fenti, kivéve a TestVNet5 tooTestVNet1 hello kapcsolatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="44b2f-300">**[Subscription 5]** This step is similar toohello one above, except you are creating hello connection from TestVNet5 tooTestVNet1.</span></span> <span data-ttu-id="44b2f-301">Győződjön meg arról, hogy hello megosztott kulcsok megfeleltetéséhez és a tooSubscription 5 csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="44b2f-301">Make sure that hello shared keys match and that you connect tooSubscription 5.</span></span>

  ```azurecli
  az network vpn-connection create -n VNet5ToVNet1 -g TestRG5 --vnet-gateway1 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW -l japaneast --shared-key "eeffgg" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```

## <span data-ttu-id="44b2f-302"><a name="verify"></a>Hello kapcsolatok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="44b2f-302"><a name="verify"></a>Verify hello connections</span></span>
[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections v2v cli](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <span data-ttu-id="44b2f-303"><a name="faq"></a>Virtuális hálózatok közötti kapcsolat – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="44b2f-303"><a name="faq"></a>VNet-to-VNet FAQ</span></span>
[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="44b2f-304">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="44b2f-304">Next steps</span></span>

* <span data-ttu-id="44b2f-305">Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="44b2f-305">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="44b2f-306">További információkért lásd: hello [Virtual Machines – dokumentáció](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="44b2f-306">For more information, see hello [Virtual Machines documentation](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="44b2f-307">A BGP kapcsolatos információkért lásd: hello [BGP áttekintése](vpn-gateway-bgp-overview.md) és [hogyan tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="44b2f-307">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
