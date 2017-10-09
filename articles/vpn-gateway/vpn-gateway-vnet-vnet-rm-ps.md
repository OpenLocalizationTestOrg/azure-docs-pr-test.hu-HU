---
title: "Csatlakozás az Azure-beli virtuális hálózat tooanother VNet: PowerShell |} Microsoft Docs"
description: "Ez a cikk lépésről lépésre bemutatja, hogyan csatlakoztathatók a virtuális hálózatok egymáshoz az Azure Resource Manager és a PowerShell használatával."
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
ms.openlocfilehash: 2da30c76867cc3f71d040e63e0dd15d153e15c10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-powershell"></a><span data-ttu-id="5ee47-103">Virtuális hálózatok közötti VPN Gateway-kapcsolat konfigurálása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="5ee47-103">Configure a VNet-to-VNet VPN gateway connection using PowerShell</span></span>

<span data-ttu-id="5ee47-104">Ez a cikk bemutatja, hogyan toocreate virtuális hálózatok közötti VPN gateway-kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="5ee47-104">This article shows you how toocreate a VPN gateway connection between virtual networks.</span></span> <span data-ttu-id="5ee47-105">hello virtuális hálózatok lehetnek azonos vagy különböző régiókban hello, és a hello azonos vagy különböző előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="5ee47-105">hello virtual networks can be in hello same or different regions, and from hello same or different subscriptions.</span></span> <span data-ttu-id="5ee47-106">Amikor másik előfizetésből, hello előfizetések csatlakozó Vnetek nem kell társított toobe hello azonos Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="5ee47-106">When connecting VNets from different subscriptions, hello subscriptions do not need toobe associated with hello same Active Directory tenant.</span></span> 

<span data-ttu-id="5ee47-107">a cikkben ismertetett hello toohello Resource Manager üzembe helyezési modellben vonatkoznak, és PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="5ee47-107">hello steps in this article apply toohello Resource Manager deployment model and use PowerShell.</span></span> <span data-ttu-id="5ee47-108">Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:</span><span class="sxs-lookup"><span data-stu-id="5ee47-108">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5ee47-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5ee47-109">Azure portal</span></span>](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [<span data-ttu-id="5ee47-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5ee47-110">PowerShell</span></span>](vpn-gateway-vnet-vnet-rm-ps.md)
> * [<span data-ttu-id="5ee47-111">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5ee47-111">Azure CLI</span></span>](vpn-gateway-howto-vnet-vnet-cli.md)
> * [<span data-ttu-id="5ee47-112">(Klasszikus) Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5ee47-112">Azure portal (classic)</span></span>](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [<span data-ttu-id="5ee47-113">Különböző üzemi modellek összekapcsolása – Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5ee47-113">Connect different deployment models - Azure portal</span></span>](vpn-gateway-connect-different-deployment-models-portal.md)
> * [<span data-ttu-id="5ee47-114">Különböző üzemi modellek összekapcsolása – PowerShell</span><span class="sxs-lookup"><span data-stu-id="5ee47-114">Connect different deployment models - PowerShell</span></span>](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

<span data-ttu-id="5ee47-115">Csatlakozás a virtuális hálózati tooanother virtuális hálózatot (VNet – VNet) hasonló tooconnecting egy VNet tooan helyszíni hely.</span><span class="sxs-lookup"><span data-stu-id="5ee47-115">Connecting a virtual network tooanother virtual network (VNet-to-VNet) is similar tooconnecting a VNet tooan on-premises site location.</span></span> <span data-ttu-id="5ee47-116">Mindkét csatlakozási típusok használja a VPN-átjáró tooprovide IPsec/IKE használatával biztonságos alagutat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-116">Both connectivity types use a VPN gateway tooprovide a secure tunnel using IPsec/IKE.</span></span> <span data-ttu-id="5ee47-117">Ha a Vnetek hello ugyanabban a régióban, érdemes lehet tooconsider csatlakoztatja őket a Vnetben társviszony-létesítés használatával.</span><span class="sxs-lookup"><span data-stu-id="5ee47-117">If your VNets are in hello same region, you may want tooconsider connecting them using VNet Peering.</span></span> <span data-ttu-id="5ee47-118">A virtuális hálózatok közötti társviszony nem használ VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="5ee47-118">VNet peering does not use a VPN gateway.</span></span> <span data-ttu-id="5ee47-119">További információ: [Társviszony létesítése virtuális hálózatok között](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5ee47-119">For more information, see [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span>

<span data-ttu-id="5ee47-120">A virtuális hálózatok közötti kommunikáció kombinálható többhelyes konfigurációkkal.</span><span class="sxs-lookup"><span data-stu-id="5ee47-120">VNet-to-VNet communication can be combined with multi-site configurations.</span></span> <span data-ttu-id="5ee47-121">Ez lehetővé teszi a felhasználó közötti kapcsolatot nyújthassanak közötti virtuális hálózati kapcsolattal rendelkező hálózati topológiák létrehozása a hello a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="5ee47-121">This lets you establish network topologies that combine cross-premises connectivity with inter-virtual network connectivity, as shown in hello following diagram:</span></span>

![Tudnivalók a kapcsolatokról](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)

### <a name="why-connect-virtual-networks"></a><span data-ttu-id="5ee47-123">Miért érdemes összekapcsolni a virtuális hálózatokat?</span><span class="sxs-lookup"><span data-stu-id="5ee47-123">Why connect virtual networks?</span></span>

<span data-ttu-id="5ee47-124">A következő okok miatt hello érdemes lehet tooconnect virtuális hálózatok:</span><span class="sxs-lookup"><span data-stu-id="5ee47-124">You may want tooconnect virtual networks for hello following reasons:</span></span>

* <span data-ttu-id="5ee47-125">**Georedundancia és földrajzi jelenlét több régióban**</span><span class="sxs-lookup"><span data-stu-id="5ee47-125">**Cross region geo-redundancy and geo-presence**</span></span>

  * <span data-ttu-id="5ee47-126">Beállíthatja a saját georeplikációját vagy szinkronizálását biztonságos kapcsolaton át, internetes végpontok használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="5ee47-126">You can set up your own geo-replication or synchronization with secure connectivity without going over Internet-facing endpoints.</span></span>
  * <span data-ttu-id="5ee47-127">Az Azure Traffic Manager és a Load Balancer segítségével magas rendelkezésre állású munkaterhelést állíthat be georedundanciával több Azure-régióban.</span><span class="sxs-lookup"><span data-stu-id="5ee47-127">With Azure Traffic Manager and Load Balancer, you can set up highly available workload with geo-redundancy across multiple Azure regions.</span></span> <span data-ttu-id="5ee47-128">Egy fontos példája tooset fel SQL Always On rendelkezésre állási csoportokkal több Azure-régiók keresztül terjednek.</span><span class="sxs-lookup"><span data-stu-id="5ee47-128">One important example is tooset up SQL Always On with Availability Groups spreading across multiple Azure regions.</span></span>
* <span data-ttu-id="5ee47-129">**Regionális többrétegű alkalmazások elkülönítéssel vagy felügyeleti határral**</span><span class="sxs-lookup"><span data-stu-id="5ee47-129">**Regional multi-tier applications with isolation or administrative boundary**</span></span>

  * <span data-ttu-id="5ee47-130">Belül hello azonos régióban, beállíthat több rétegből álló alkalmazások összekapcsolása tooisolation vagy felügyeleti követelmények miatt több virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="5ee47-130">Within hello same region, you can set up multi-tier applications with multiple virtual networks connected together due tooisolation or administrative requirements.</span></span>

<span data-ttu-id="5ee47-131">VNet – VNet kapcsolatokhoz kapcsolatos további információkért lásd: hello [VNet – VNet – gyakori kérdések](#faq) hello Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="5ee47-131">For more information about VNet-to-VNet connections, see hello [VNet-to-VNet FAQ](#faq) at hello end of this article.</span></span>

## <a name="which-set-of-steps-should-i-use"></a><span data-ttu-id="5ee47-132">Melyik eljárást használjam?</span><span class="sxs-lookup"><span data-stu-id="5ee47-132">Which set of steps should I use?</span></span>

<span data-ttu-id="5ee47-133">Ebben a cikkben kétféle lépéssorozatot láthat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-133">In this article, you see two different sets of steps.</span></span> <span data-ttu-id="5ee47-134">Lépéseit egy készletét [lévő Vnetek hello ugyanahhoz az előfizetéshez](#samesub), majd egy másikat a [Vnetekhez különböző előfizetések lévő](#difsub).</span><span class="sxs-lookup"><span data-stu-id="5ee47-134">One set of steps for [VNets that reside in hello same subscription](#samesub), and another for [VNets that reside in different subscriptions](#difsub).</span></span> <span data-ttu-id="5ee47-135">hello fő különbség hello beállítása, hogy hozhat létre, és konfigurálja a virtuális hálózat és az átjáró belül minden erőforrás hello ugyanazon PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="5ee47-135">hello key difference between hello sets is whether you can create and configure all virtual network and gateway resources within hello same PowerShell session.</span></span>

<span data-ttu-id="5ee47-136">hello a cikkben ismertetett változókkal minden szakasz elején hello deklarál.</span><span class="sxs-lookup"><span data-stu-id="5ee47-136">hello steps in this article use variables that are declared at hello beginning of each section.</span></span> <span data-ttu-id="5ee47-137">Ha már dolgozunk meglévő Vnetek, módosítsa a hello változók tooreflect hello beállításait a saját környezetében.</span><span class="sxs-lookup"><span data-stu-id="5ee47-137">If you already are working with existing VNets, modify hello variables tooreflect hello settings in your own environment.</span></span> <span data-ttu-id="5ee47-138">Ha névfeloldással szeretné ellátni a virtuális hálózatokat, tekintse meg a [Névfeloldás](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="5ee47-138">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

## <span data-ttu-id="5ee47-139"><a name="samesub"></a>Hogyan tooconnect Vnetek, amelyek a hello ugyanahhoz az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="5ee47-139"><a name="samesub"></a>How tooconnect VNets that are in hello same subscription</span></span>

![v2v ábra](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a><span data-ttu-id="5ee47-141">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="5ee47-141">Before you begin</span></span>

<span data-ttu-id="5ee47-142">Megkezdése előtt szüksége tooinstall hello legújabb verziójára hello Azure Resource Manager PowerShell-parancsmagok, legalább 4.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="5ee47-142">Before beginning, you need tooinstall hello latest version of hello Azure Resource Manager PowerShell cmdlets, at least 4.0 or later.</span></span> <span data-ttu-id="5ee47-143">PowerShell-parancsmagok hello telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5ee47-143">For more information about installing hello PowerShell cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <span data-ttu-id="5ee47-144"><a name="Step1"></a>1. lépés – Az IP-címtartományok megtervezése</span><span class="sxs-lookup"><span data-stu-id="5ee47-144"><a name="Step1"></a>Step 1 - Plan your IP address ranges</span></span>

<span data-ttu-id="5ee47-145">Az alábbi lépésekkel hello létrehozhatunk két virtuális hálózatok a megfelelő átjáró alhálózatok és konfigurációk mellett.</span><span class="sxs-lookup"><span data-stu-id="5ee47-145">In hello following steps, we create two virtual networks along with their respective gateway subnets and configurations.</span></span> <span data-ttu-id="5ee47-146">Majd létrehozhatunk hello közötti VPN-kapcsolat két Vnetek.</span><span class="sxs-lookup"><span data-stu-id="5ee47-146">We then create a VPN connection between hello two VNets.</span></span> <span data-ttu-id="5ee47-147">Fontos tooplan hello IP-címtartományok esetében a hálózati konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="5ee47-147">It’s important tooplan hello IP address ranges for your network configuration.</span></span> <span data-ttu-id="5ee47-148">Ügyeljen arra, hogy a virtuális hálózata tartományai és a helyi hálózata tartományai ne legyenek átfedésben egymással.</span><span class="sxs-lookup"><span data-stu-id="5ee47-148">Keep in mind that you must make sure that none of your VNet ranges or local network ranges overlap in any way.</span></span> <span data-ttu-id="5ee47-149">Ezekben a példákban nem szerepel DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="5ee47-149">In these examples, we do not include a DNS server.</span></span> <span data-ttu-id="5ee47-150">Ha névfeloldással szeretné ellátni a virtuális hálózatokat, tekintse meg a [Névfeloldás](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="5ee47-150">If you want name resolution for your virtual networks, see [Name resolution](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

<span data-ttu-id="5ee47-151">A következő értékek hello példákban hello használjuk:</span><span class="sxs-lookup"><span data-stu-id="5ee47-151">We use hello following values in hello examples:</span></span>

<span data-ttu-id="5ee47-152">**Értékek a TestVNet1-hez:**</span><span class="sxs-lookup"><span data-stu-id="5ee47-152">**Values for TestVNet1:**</span></span>

* <span data-ttu-id="5ee47-153">Virtuális hálózat neve: TestVNet1</span><span class="sxs-lookup"><span data-stu-id="5ee47-153">VNet Name: TestVNet1</span></span>
* <span data-ttu-id="5ee47-154">Erőforráscsoport: TestRG1</span><span class="sxs-lookup"><span data-stu-id="5ee47-154">Resource Group: TestRG1</span></span>
* <span data-ttu-id="5ee47-155">Hely: East US</span><span class="sxs-lookup"><span data-stu-id="5ee47-155">Location: East US</span></span>
* <span data-ttu-id="5ee47-156">TestVNet1: 10.11.0.0/16 és 10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="5ee47-156">TestVNet1: 10.11.0.0/16 & 10.12.0.0/16</span></span>
* <span data-ttu-id="5ee47-157">Előtér: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="5ee47-157">FrontEnd: 10.11.0.0/24</span></span>
* <span data-ttu-id="5ee47-158">Háttér: 10.12.0.0/24</span><span class="sxs-lookup"><span data-stu-id="5ee47-158">BackEnd: 10.12.0.0/24</span></span>
* <span data-ttu-id="5ee47-159">Átjáróalhálózat: 10.12.255.0/27</span><span class="sxs-lookup"><span data-stu-id="5ee47-159">GatewaySubnet: 10.12.255.0/27</span></span>
* <span data-ttu-id="5ee47-160">Átjáró neve: VNet1GW</span><span class="sxs-lookup"><span data-stu-id="5ee47-160">GatewayName: VNet1GW</span></span>
* <span data-ttu-id="5ee47-161">Nyilvános IP-cím: VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="5ee47-161">Public IP: VNet1GWIP</span></span>
* <span data-ttu-id="5ee47-162">VPN típusa: RouteBased</span><span class="sxs-lookup"><span data-stu-id="5ee47-162">VPNType: RouteBased</span></span>
* <span data-ttu-id="5ee47-163">Kapcsolat (1–4): VNet1toVNet4</span><span class="sxs-lookup"><span data-stu-id="5ee47-163">Connection(1to4): VNet1toVNet4</span></span>
* <span data-ttu-id="5ee47-164">Kapcsolat (1–5): VNet1toVNet5</span><span class="sxs-lookup"><span data-stu-id="5ee47-164">Connection(1to5): VNet1toVNet5</span></span>
* <span data-ttu-id="5ee47-165">Kapcsolat típusa: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="5ee47-165">ConnectionType: VNet2VNet</span></span>

<span data-ttu-id="5ee47-166">**Értékek a TestVNet4-hez:**</span><span class="sxs-lookup"><span data-stu-id="5ee47-166">**Values for TestVNet4:**</span></span>

* <span data-ttu-id="5ee47-167">Virtuális hálózat neve: TestVNet4</span><span class="sxs-lookup"><span data-stu-id="5ee47-167">VNet Name: TestVNet4</span></span>
* <span data-ttu-id="5ee47-168">TestVNet2: 10.41.0.0/16 és 10.42.0.0/16</span><span class="sxs-lookup"><span data-stu-id="5ee47-168">TestVNet2: 10.41.0.0/16 & 10.42.0.0/16</span></span>
* <span data-ttu-id="5ee47-169">Előtér: 10.41.0.0/24</span><span class="sxs-lookup"><span data-stu-id="5ee47-169">FrontEnd: 10.41.0.0/24</span></span>
* <span data-ttu-id="5ee47-170">Háttér: 10.42.0.0/24</span><span class="sxs-lookup"><span data-stu-id="5ee47-170">BackEnd: 10.42.0.0/24</span></span>
* <span data-ttu-id="5ee47-171">Átjáróalhálózat: 10.42.255.0/27</span><span class="sxs-lookup"><span data-stu-id="5ee47-171">GatewaySubnet: 10.42.255.0/27</span></span>
* <span data-ttu-id="5ee47-172">Erőforráscsoport: TestRG4</span><span class="sxs-lookup"><span data-stu-id="5ee47-172">Resource Group: TestRG4</span></span>
* <span data-ttu-id="5ee47-173">Hely: West US</span><span class="sxs-lookup"><span data-stu-id="5ee47-173">Location: West US</span></span>
* <span data-ttu-id="5ee47-174">Átjáró neve: VNet4GW</span><span class="sxs-lookup"><span data-stu-id="5ee47-174">GatewayName: VNet4GW</span></span>
* <span data-ttu-id="5ee47-175">Nyilvános IP-cím: VNet4GWIP</span><span class="sxs-lookup"><span data-stu-id="5ee47-175">Public IP: VNet4GWIP</span></span>
* <span data-ttu-id="5ee47-176">VPN típusa: RouteBased</span><span class="sxs-lookup"><span data-stu-id="5ee47-176">VPNType: RouteBased</span></span>
* <span data-ttu-id="5ee47-177">Kapcsolat: VNet4toVNet1</span><span class="sxs-lookup"><span data-stu-id="5ee47-177">Connection: VNet4toVNet1</span></span>
* <span data-ttu-id="5ee47-178">Kapcsolat típusa: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="5ee47-178">ConnectionType: VNet2VNet</span></span>


### <span data-ttu-id="5ee47-179"><a name="Step2"></a>2. lépés – A TestVNet1 létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5ee47-179"><a name="Step2"></a>Step 2 - Create and configure TestVNet1</span></span>

1. <span data-ttu-id="5ee47-180">Deklarálja a változókat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-180">Declare your variables.</span></span> <span data-ttu-id="5ee47-181">Ez a példa hello értékekkel ehhez a gyakorlathoz hello változók deklarálja.</span><span class="sxs-lookup"><span data-stu-id="5ee47-181">This example declares hello variables using hello values for this exercise.</span></span> <span data-ttu-id="5ee47-182">A legtöbb esetben azt le kell cserélni hello értékeket a saját.</span><span class="sxs-lookup"><span data-stu-id="5ee47-182">In most cases, you should replace hello values with your own.</span></span> <span data-ttu-id="5ee47-183">Azonban ezek változókat is használhat, ha ismeri a konfiguráció az ilyen típusú hello lépéseket toobecome keresztül futtatja.</span><span class="sxs-lookup"><span data-stu-id="5ee47-183">However, you can use these variables if you are running through hello steps toobecome familiar with this type of configuration.</span></span> <span data-ttu-id="5ee47-184">Hello változók, ha szükséges, módosítsa, majd másolja és illessze be a PowerShell-konzolban.</span><span class="sxs-lookup"><span data-stu-id="5ee47-184">Modify hello variables if needed, then copy and paste them into your PowerShell console.</span></span>

  ```powershell
  $Sub1 = "Replace_With_Your_Subcription_Name"
  $RG1 = "TestRG1"
  $Location1 = "East US"
  $VNetName1 = "TestVNet1"
  $FESubName1 = "FrontEnd"
  $BESubName1 = "Backend"
  $GWSubName1 = "GatewaySubnet"
  $VNetPrefix11 = "10.11.0.0/16"
  $VNetPrefix12 = "10.12.0.0/16"
  $FESubPrefix1 = "10.11.0.0/24"
  $BESubPrefix1 = "10.12.0.0/24"
  $GWSubPrefix1 = "10.12.255.0/27"
  $GWName1 = "VNet1GW"
  $GWIPName1 = "VNet1GWIP"
  $GWIPconfName1 = "gwipconf1"
  $Connection14 = "VNet1toVNet4"
  $Connection15 = "VNet1toVNet5"
  ```

2. <span data-ttu-id="5ee47-185">Csatlakozás tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="5ee47-185">Connect tooyour account.</span></span> <span data-ttu-id="5ee47-186">A következő példa toohelp csatlakozás hello használata:</span><span class="sxs-lookup"><span data-stu-id="5ee47-186">Use hello following example toohelp you connect:</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="5ee47-187">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="5ee47-187">Check hello subscriptions for hello account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

  <span data-ttu-id="5ee47-188">Adja meg, hogy szeretné-e toouse hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5ee47-188">Specify hello subscription that you want toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub1
  ```
3. <span data-ttu-id="5ee47-189">Hozzon létre egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5ee47-189">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG1 -Location $Location1
  ```
4. <span data-ttu-id="5ee47-190">Hozzon létre hello TestVNet1 alhálózati konfigurációi.</span><span class="sxs-lookup"><span data-stu-id="5ee47-190">Create hello subnet configurations for TestVNet1.</span></span> <span data-ttu-id="5ee47-191">Ez a példa létrehoz egy TestVNet1 nevű virtuális hálózatot és három alhálózatot, amelyek neve a következő: GatewaySubnet, FrontEnd és Backend.</span><span class="sxs-lookup"><span data-stu-id="5ee47-191">This example creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="5ee47-192">Az értékek behelyettesítésekor fontos, hogy az átjáróalhálózat neve mindenképp GatewaySubnet legyen.</span><span class="sxs-lookup"><span data-stu-id="5ee47-192">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="5ee47-193">Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="5ee47-193">If you name it something else, your gateway creation fails.</span></span>

  <span data-ttu-id="5ee47-194">hello alábbi példa hello változók korábban beállított.</span><span class="sxs-lookup"><span data-stu-id="5ee47-194">hello following example uses hello variables that you set earlier.</span></span> <span data-ttu-id="5ee47-195">Ebben a példában hello átjáróalhálózatot egy /27 használ.</span><span class="sxs-lookup"><span data-stu-id="5ee47-195">In this example, hello gateway subnet is using a /27.</span></span> <span data-ttu-id="5ee47-196">Habár lehetséges toocreate mérete /29 legyen egy átjáró-alhálózatot, azt javasoljuk, hogy hozzon létre egy nagyobb alhálózat több címet legalább /28 vagy /27 kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="5ee47-196">While it is possible toocreate a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting at least /28 or /27.</span></span> <span data-ttu-id="5ee47-197">Ez lehetővé teszi elég címek tooaccommodate lehetséges olyan beállításokat, amelyeket érdemes lehet a hello jövőbeli.</span><span class="sxs-lookup"><span data-stu-id="5ee47-197">This will allow for enough addresses tooaccommodate possible additional configurations that you may want in hello future.</span></span>

  ```powershell
  $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
  $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
  $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1
  ```
5. <span data-ttu-id="5ee47-198">Hozza létre a TestVNet1-et.</span><span class="sxs-lookup"><span data-stu-id="5ee47-198">Create TestVNet1.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
  ```
6. <span data-ttu-id="5ee47-199">A nyilvános IP-cím toobe lefoglalt toohello átjáró fog létrehozni a Vnethez tartozó kérelmet.</span><span class="sxs-lookup"><span data-stu-id="5ee47-199">Request a public IP address toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="5ee47-200">Figyelje meg, hogy hello AllocationMethod dinamikus.</span><span class="sxs-lookup"><span data-stu-id="5ee47-200">Notice that hello AllocationMethod is Dynamic.</span></span> <span data-ttu-id="5ee47-201">Nem adható meg, hogy szeretné-e toouse hello IP-címet.</span><span class="sxs-lookup"><span data-stu-id="5ee47-201">You cannot specify hello IP address that you want toouse.</span></span> <span data-ttu-id="5ee47-202">Fontos a dinamikusan kiosztott tooyour átjáró.</span><span class="sxs-lookup"><span data-stu-id="5ee47-202">It's dynamically allocated tooyour gateway.</span></span> 

  ```powershell
  $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
  ```
7. <span data-ttu-id="5ee47-203">Hozzon létre hello átjáró konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="5ee47-203">Create hello gateway configuration.</span></span> <span data-ttu-id="5ee47-204">hello átjáró konfigurációs hello alhálózatot és a hello nyilvános IP-cím toouse határoz meg.</span><span class="sxs-lookup"><span data-stu-id="5ee47-204">hello gateway configuration defines hello subnet and hello public IP address toouse.</span></span> <span data-ttu-id="5ee47-205">Hello példa toocreate az átjáró konfigurációját használni.</span><span class="sxs-lookup"><span data-stu-id="5ee47-205">Use hello example toocreate your gateway configuration.</span></span>

  ```powershell
  $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
  $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```
8. <span data-ttu-id="5ee47-206">Hozzon létre hello átjáró TestVNet1.</span><span class="sxs-lookup"><span data-stu-id="5ee47-206">Create hello gateway for TestVNet1.</span></span> <span data-ttu-id="5ee47-207">Ebben a lépésben a TestVNet1 hozza létre hello virtuális hálózati átjárót.</span><span class="sxs-lookup"><span data-stu-id="5ee47-207">In this step, you create hello virtual network gateway for your TestVNet1.</span></span> <span data-ttu-id="5ee47-208">A virtuális hálózatok közötti konfigurációkhoz a VpnType paraméternek RouteBased értékűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5ee47-208">VNet-to-VNet configurations require a RouteBased VpnType.</span></span> <span data-ttu-id="5ee47-209">Átjáró létrehozása gyakran eltarthat 45 perc vagy több, attól függően, hogy a kiválasztott átjáróeszközön hello Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-209">Creating a gateway can often take 45 minutes or more, depending on hello selected gateway SKU.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-3---create-and-configure-testvnet4"></a><span data-ttu-id="5ee47-210">3. lépés – A TestVNet4 létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5ee47-210">Step 3 - Create and configure TestVNet4</span></span>

<span data-ttu-id="5ee47-211">A TestVNet1 konfigurálása után a hozza létre a TestVNet4 virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="5ee47-211">Once you've configured TestVNet1, create TestVNet4.</span></span> <span data-ttu-id="5ee47-212">Hello kövesse az alábbi hello értékeket cserélje le a saját, amikor szükséges.</span><span class="sxs-lookup"><span data-stu-id="5ee47-212">Follow hello steps below, replacing hello values with your own when needed.</span></span> <span data-ttu-id="5ee47-213">Ebben a lépésben végezhető hello belül azonos PowerShell-munkamenetben mert hello azonos előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="5ee47-213">This step can be done within hello same PowerShell session because it is in hello same subscription.</span></span>

1. <span data-ttu-id="5ee47-214">Deklarálja a változókat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-214">Declare your variables.</span></span> <span data-ttu-id="5ee47-215">Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="5ee47-215">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

  ```powershell
  $RG4 = "TestRG4"
  $Location4 = "West US"
  $VnetName4 = "TestVNet4"
  $FESubName4 = "FrontEnd"
  $BESubName4 = "Backend"
  $GWSubName4 = "GatewaySubnet"
  $VnetPrefix41 = "10.41.0.0/16"
  $VnetPrefix42 = "10.42.0.0/16"
  $FESubPrefix4 = "10.41.0.0/24"
  $BESubPrefix4 = "10.42.0.0/24"
  $GWSubPrefix4 = "10.42.255.0/27"
  $GWName4 = "VNet4GW"
  $GWIPName4 = "VNet4GWIP"
  $GWIPconfName4 = "gwipconf4"
  $Connection41 = "VNet4toVNet1"
  ```
2. <span data-ttu-id="5ee47-216">Hozzon létre egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5ee47-216">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG4 -Location $Location4
  ```
3. <span data-ttu-id="5ee47-217">Hozzon létre hello TestVNet4 alhálózati konfigurációi.</span><span class="sxs-lookup"><span data-stu-id="5ee47-217">Create hello subnet configurations for TestVNet4.</span></span>

  ```powershell
  $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
  $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
  $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4
  ```
4. <span data-ttu-id="5ee47-218">Hozza létre a TestVNet4-et.</span><span class="sxs-lookup"><span data-stu-id="5ee47-218">Create TestVNet4.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4
  ```
5. <span data-ttu-id="5ee47-219">Kérjen egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="5ee47-219">Request a public IP address.</span></span>

  ```powershell
  $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AllocationMethod Dynamic
  ```
6. <span data-ttu-id="5ee47-220">Hozzon létre hello átjáró konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="5ee47-220">Create hello gateway configuration.</span></span>

  ```powershell
  $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
  $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
  $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4
  ```
7. <span data-ttu-id="5ee47-221">Hello TestVNet4 átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5ee47-221">Create hello TestVNet4 gateway.</span></span> <span data-ttu-id="5ee47-222">Átjáró létrehozása gyakran eltarthat 45 perc vagy több, attól függően, hogy a kiválasztott átjáróeszközön hello Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-222">Creating a gateway can often take 45 minutes or more, depending on hello selected gateway SKU.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
  -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-4---create-hello-connections"></a><span data-ttu-id="5ee47-223">4. lépés – hello kapcsolatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ee47-223">Step 4 - Create hello connections</span></span>

1. <span data-ttu-id="5ee47-224">Hozza létre a virtuális hálózati átjárókat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-224">Get both virtual network gateways.</span></span> <span data-ttu-id="5ee47-225">Ha mindkét hello átjárók hello ugyanahhoz az előfizetéshez, mivel ezek hello példában, hajthatja végre ezt a lépést a hello ugyanazon PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="5ee47-225">If both of hello gateways are in hello same subscription, as they are in hello example, you can complete this step in hello same PowerShell session.</span></span>

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4
  ```
2. <span data-ttu-id="5ee47-226">Hello TestVNet1 tooTestVNet4 kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5ee47-226">Create hello TestVNet1 tooTestVNet4 connection.</span></span> <span data-ttu-id="5ee47-227">Ebben a lépésben készíthet TestVNet1 tooTestVNet4 hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-227">In this step, you create hello connection from TestVNet1 tooTestVNet4.</span></span> <span data-ttu-id="5ee47-228">Látni fogja, a megosztott kulcs hello példák hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="5ee47-228">You'll see a shared key referenced in hello examples.</span></span> <span data-ttu-id="5ee47-229">Hello megosztott kulcs saját értékeket is használhat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-229">You can use your own values for hello shared key.</span></span> <span data-ttu-id="5ee47-230">fontos dolog hello megosztott kulcs hello egyeznie kell mindkét kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="5ee47-230">hello important thing is that hello shared key must match for both connections.</span></span> <span data-ttu-id="5ee47-231">Kapcsolat létrehozása során toocomplete rövid is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="5ee47-231">Creating a connection can take a short while toocomplete.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
3. <span data-ttu-id="5ee47-232">Hello TestVNet4 tooTestVNet1 kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5ee47-232">Create hello TestVNet4 tooTestVNet1 connection.</span></span> <span data-ttu-id="5ee47-233">Ez a lépés akkor hasonló toohello valamelyik fenti, kivéve a TestVNet4 tooTestVNet1 hello kapcsolatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5ee47-233">This step is similar toohello one above, except you are creating hello connection from TestVNet4 tooTestVNet1.</span></span> <span data-ttu-id="5ee47-234">Ellenőrizze, hogy megosztott hello kulcsok felel meg.</span><span class="sxs-lookup"><span data-stu-id="5ee47-234">Make sure hello shared keys match.</span></span> <span data-ttu-id="5ee47-235">hello kapcsolat jön létre, néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="5ee47-235">hello connection will be established after a few minutes.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
  -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. <span data-ttu-id="5ee47-236">Ellenőrizze a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="5ee47-236">Verify your connection.</span></span> <span data-ttu-id="5ee47-237">Című rész hello [hogyan tooverify a kapcsolat](#verify).</span><span class="sxs-lookup"><span data-stu-id="5ee47-237">See hello section [How tooverify your connection](#verify).</span></span>

## <span data-ttu-id="5ee47-238"><a name="difsub"></a>Hogyan tooconnect Vnetek, amelyek különböző előfizetésekhez</span><span class="sxs-lookup"><span data-stu-id="5ee47-238"><a name="difsub"></a>How tooconnect VNets that are in different subscriptions</span></span>

![v2v ábra](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

<span data-ttu-id="5ee47-240">Ebben a forgatókönyvben csatlakoztatjuk a TestVNet1 és a TestVNet5 virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="5ee47-240">In this scenario, we connect TestVNet1 and TestVNet5.</span></span> <span data-ttu-id="5ee47-241">A TestVNet1 és a TestVNet5 eltérő előfizetésekben található.</span><span class="sxs-lookup"><span data-stu-id="5ee47-241">TestVNet1 and TestVNet5 reside in a different subscription.</span></span> <span data-ttu-id="5ee47-242">hello előfizetések nem kell hello társított toobe azonos Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="5ee47-242">hello subscriptions do not need toobe associated with hello same Active Directory tenant.</span></span> <span data-ttu-id="5ee47-243">hello ezeket a lépéseket és hello előző készletet közötti különbség, hogy egyes hello konfigurációs lépések szükség van egy külön PowerShell-munkamenetben hello második előfizetés hello környezetben végrehajtott toobe.</span><span class="sxs-lookup"><span data-stu-id="5ee47-243">hello difference between these steps and hello previous set is that some of hello configuration steps need toobe performed in a separate PowerShell session in hello context of hello second subscription.</span></span> <span data-ttu-id="5ee47-244">Különösen amikor hello két előfizetések tartoznak toodifferent szervezetek.</span><span class="sxs-lookup"><span data-stu-id="5ee47-244">Especially when hello two subscriptions belong toodifferent organizations.</span></span>

### <a name="step-5---create-and-configure-testvnet1"></a><span data-ttu-id="5ee47-245">5. lépés – A TestVNet1 létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5ee47-245">Step 5 - Create and configure TestVNet1</span></span>

<span data-ttu-id="5ee47-246">Meg kell adnia a [1. lépés](#Step1) és [2. lépés](#Step2) az előző hello toocreate szakasz és a VPN-átjáró hello TestVNet1 és TestVNet1 konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="5ee47-246">You must complete [Step 1](#Step1) and [Step 2](#Step2) from hello previous section toocreate and configure TestVNet1 and hello VPN Gateway for TestVNet1.</span></span> <span data-ttu-id="5ee47-247">Ehhez a konfigurációhoz akkor használják, nem szükséges toocreate TestVNet4 az előző szakasz hello, bár hoz létre, ha ez nem ütközik ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="5ee47-247">For this configuration, you are not required toocreate TestVNet4 from hello previous section, although if you do create it, it will not conflict with these steps.</span></span> <span data-ttu-id="5ee47-248">1. lépés és a 2. lépés befejezése után folytassa a 6. lépés toocreate TestVNet5.</span><span class="sxs-lookup"><span data-stu-id="5ee47-248">Once you complete Step 1 and Step 2, continue with Step 6 toocreate TestVNet5.</span></span> 

### <a name="step-6---verify-hello-ip-address-ranges"></a><span data-ttu-id="5ee47-249">6. lépés - hello IP-címtartományok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="5ee47-249">Step 6 - Verify hello IP address ranges</span></span>

<span data-ttu-id="5ee47-250">Arról, hogy hello IP-címtér hello új virtuális hálózat TestVNet5, nem fedi át a virtuális hálózat tartományok vagy a helyi hálózati átjáró tartományok egyik fontos toomake.</span><span class="sxs-lookup"><span data-stu-id="5ee47-250">It is important toomake sure that hello IP address space of hello new virtual network, TestVNet5, does not overlap with any of your VNet ranges or local network gateway ranges.</span></span> <span data-ttu-id="5ee47-251">Ebben a példában a virtuális hálózatok hello tartozhat toodifferent szervezetek.</span><span class="sxs-lookup"><span data-stu-id="5ee47-251">In this example, hello virtual networks may belong toodifferent organizations.</span></span> <span data-ttu-id="5ee47-252">Ebben a gyakorlatban a következő értékek hello TestVNet5 hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="5ee47-252">For this exercise, you can use hello following values for hello TestVNet5:</span></span>

<span data-ttu-id="5ee47-253">**Értékek a TestVNet5-höz:**</span><span class="sxs-lookup"><span data-stu-id="5ee47-253">**Values for TestVNet5:**</span></span>

* <span data-ttu-id="5ee47-254">Virtuális hálózat neve: TestVNet5</span><span class="sxs-lookup"><span data-stu-id="5ee47-254">VNet Name: TestVNet5</span></span>
* <span data-ttu-id="5ee47-255">Erőforráscsoport: TestRG5</span><span class="sxs-lookup"><span data-stu-id="5ee47-255">Resource Group: TestRG5</span></span>
* <span data-ttu-id="5ee47-256">Hely: Japan East</span><span class="sxs-lookup"><span data-stu-id="5ee47-256">Location: Japan East</span></span>
* <span data-ttu-id="5ee47-257">TestVNet5: 10.51.0.0/16 és 10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="5ee47-257">TestVNet5: 10.51.0.0/16 & 10.52.0.0/16</span></span>
* <span data-ttu-id="5ee47-258">Előtér: 10.51.0.0/24</span><span class="sxs-lookup"><span data-stu-id="5ee47-258">FrontEnd: 10.51.0.0/24</span></span>
* <span data-ttu-id="5ee47-259">Háttér: 10.52.0.0/24</span><span class="sxs-lookup"><span data-stu-id="5ee47-259">BackEnd: 10.52.0.0/24</span></span>
* <span data-ttu-id="5ee47-260">Átjáróalhálózat: 10.52.255.0.0/27</span><span class="sxs-lookup"><span data-stu-id="5ee47-260">GatewaySubnet: 10.52.255.0.0/27</span></span>
* <span data-ttu-id="5ee47-261">Átjáró neve: VNet5GW</span><span class="sxs-lookup"><span data-stu-id="5ee47-261">GatewayName: VNet5GW</span></span>
* <span data-ttu-id="5ee47-262">Nyilvános IP-cím: VNet5GWIP</span><span class="sxs-lookup"><span data-stu-id="5ee47-262">Public IP: VNet5GWIP</span></span>
* <span data-ttu-id="5ee47-263">VPN típusa: RouteBased</span><span class="sxs-lookup"><span data-stu-id="5ee47-263">VPNType: RouteBased</span></span>
* <span data-ttu-id="5ee47-264">Kapcsolat: VNet5toVNet1</span><span class="sxs-lookup"><span data-stu-id="5ee47-264">Connection: VNet5toVNet1</span></span>
* <span data-ttu-id="5ee47-265">Kapcsolat típusa: VNet2VNet</span><span class="sxs-lookup"><span data-stu-id="5ee47-265">ConnectionType: VNet2VNet</span></span>

### <a name="step-7---create-and-configure-testvnet5"></a><span data-ttu-id="5ee47-266">7. lépés – A TestVNet5 létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5ee47-266">Step 7 - Create and configure TestVNet5</span></span>

<span data-ttu-id="5ee47-267">Ez a lépés hello új előfizetés hello környezetében kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="5ee47-267">This step must be done in hello context of hello new subscription.</span></span> <span data-ttu-id="5ee47-268">Ez a kijelző hello hello előfizetés tulajdonosa egy másik szervezet rendszergazdája hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="5ee47-268">This part may be performed by hello administrator in a different organization that owns hello subscription.</span></span>

1. <span data-ttu-id="5ee47-269">Deklarálja a változókat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-269">Declare your variables.</span></span> <span data-ttu-id="5ee47-270">Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="5ee47-270">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

  ```powershell
  $Sub5 = "Replace_With_the_New_Subcription_Name"
  $RG5 = "TestRG5"
  $Location5 = "Japan East"
  $VnetName5 = "TestVNet5"
  $FESubName5 = "FrontEnd"
  $BESubName5 = "Backend"
  $GWSubName5 = "GatewaySubnet"
  $VnetPrefix51 = "10.51.0.0/16"
  $VnetPrefix52 = "10.52.0.0/16"
  $FESubPrefix5 = "10.51.0.0/24"
  $BESubPrefix5 = "10.52.0.0/24"
  $GWSubPrefix5 = "10.52.255.0/27"
  $GWName5 = "VNet5GW"
  $GWIPName5 = "VNet5GWIP"
  $GWIPconfName5 = "gwipconf5"
  $Connection51 = "VNet5toVNet1"
  ```
2. <span data-ttu-id="5ee47-271">Csatlakozás toosubscription 5.</span><span class="sxs-lookup"><span data-stu-id="5ee47-271">Connect toosubscription 5.</span></span> <span data-ttu-id="5ee47-272">Nyissa meg a PowerShell-konzolt, és csatlakozzon a tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="5ee47-272">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="5ee47-273">A következő minta toohelp csatlakozás hello használata:</span><span class="sxs-lookup"><span data-stu-id="5ee47-273">Use hello following sample toohelp you connect:</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="5ee47-274">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="5ee47-274">Check hello subscriptions for hello account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

  <span data-ttu-id="5ee47-275">Adja meg, hogy szeretné-e toouse hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5ee47-275">Specify hello subscription that you want toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub5
  ```
3. <span data-ttu-id="5ee47-276">Hozzon létre egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5ee47-276">Create a new resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name $RG5 -Location $Location5
  ```
4. <span data-ttu-id="5ee47-277">Hozzon létre hello TestVNet5 alhálózati konfigurációi.</span><span class="sxs-lookup"><span data-stu-id="5ee47-277">Create hello subnet configurations for TestVNet5.</span></span>

  ```powershell
  $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
  $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
  $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5
  ```
5. <span data-ttu-id="5ee47-278">Hozza létre a TestVNet5-öt.</span><span class="sxs-lookup"><span data-stu-id="5ee47-278">Create TestVNet5.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
  -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5
  ```
6. <span data-ttu-id="5ee47-279">Kérjen egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="5ee47-279">Request a public IP address.</span></span>

  ```powershell
  $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
  -Location $Location5 -AllocationMethod Dynamic
  ```
7. <span data-ttu-id="5ee47-280">Hozzon létre hello átjáró konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="5ee47-280">Create hello gateway configuration.</span></span>

  ```powershell
  $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
  $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
  $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5
  ```
8. <span data-ttu-id="5ee47-281">Hello TestVNet5 átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5ee47-281">Create hello TestVNet5 gateway.</span></span>

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
  -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-8---create-hello-connections"></a><span data-ttu-id="5ee47-282">8. lépés – hello kapcsolatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ee47-282">Step 8 - Create hello connections</span></span>

<span data-ttu-id="5ee47-283">Ebben a példában hello átjárók hello különböző előfizetésekhez, mert azt már ossza fel ezt a lépést két PowerShell-munkamenetekben jelölésű [előfizetés 1] és [előfizetés 5].</span><span class="sxs-lookup"><span data-stu-id="5ee47-283">In this example, because hello gateways are in hello different subscriptions, we've split this step into two PowerShell sessions marked as [Subscription 1] and [Subscription 5].</span></span>

1. <span data-ttu-id="5ee47-284">**[1. előfizetés]**  Hello virtuális hálózati átjáró előfizetés 1.</span><span class="sxs-lookup"><span data-stu-id="5ee47-284">**[Subscription 1]** Get hello virtual network gateway for Subscription 1.</span></span> <span data-ttu-id="5ee47-285">Jelentkezzen be, és tooSubscription 1 csatlakozzon a következő példa hello futtatása előtt:</span><span class="sxs-lookup"><span data-stu-id="5ee47-285">Log in and connect tooSubscription 1 before running hello following example:</span></span>

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  ```

  <span data-ttu-id="5ee47-286">Másolja a következő elemek hello hello kimenetét, és küldjön e-mailben vagy más módszerrel előfizetés 5 ezek toohello rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="5ee47-286">Copy hello output of hello following elements and send these toohello administrator of Subscription 5 via email or another method.</span></span>

  ```powershell
  $vnet1gw.Name
  $vnet1gw.Id
  ```

  <span data-ttu-id="5ee47-287">Ez a két elem értékek hasonló toohello egy példa a kimenetre a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="5ee47-287">These two elements will have values similar toohello following example output:</span></span>

  ```
  PS D:\> $vnet1gw.Name
  VNet1GW
  PS D:\> $vnet1gw.Id
  /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```
2. <span data-ttu-id="5ee47-288">**[Előfizetés 5]**  Hello virtuális hálózati átjáró előfizetés 5.</span><span class="sxs-lookup"><span data-stu-id="5ee47-288">**[Subscription 5]** Get hello virtual network gateway for Subscription 5.</span></span> <span data-ttu-id="5ee47-289">Jelentkezzen be, és tooSubscription 5 csatlakozzon a következő példa hello futtatása előtt:</span><span class="sxs-lookup"><span data-stu-id="5ee47-289">Log in and connect tooSubscription 5 before running hello following example:</span></span>

  ```powershell
  $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5
  ```

  <span data-ttu-id="5ee47-290">Másolja a következő elemek hello hello kimenetét, és küldjön e-mailben vagy más módszerrel előfizetés 1 ezek toohello rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="5ee47-290">Copy hello output of hello following elements and send these toohello administrator of Subscription 1 via email or another method.</span></span>

  ```powershell
  $vnet5gw.Name
  $vnet5gw.Id
  ```

  <span data-ttu-id="5ee47-291">Ez a két elem értékek hasonló toohello egy példa a kimenetre a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="5ee47-291">These two elements will have values similar toohello following example output:</span></span>

  ```
  PS C:\> $vnet5gw.Name
  VNet5GW
  PS C:\> $vnet5gw.Id
  /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```
3. <span data-ttu-id="5ee47-292">**[1. előfizetés]**  Hello TestVNet1 tooTestVNet5 kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5ee47-292">**[Subscription 1]** Create hello TestVNet1 tooTestVNet5 connection.</span></span> <span data-ttu-id="5ee47-293">Ebben a lépésben készíthet TestVNet1 tooTestVNet5 hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-293">In this step, you create hello connection from TestVNet1 tooTestVNet5.</span></span> <span data-ttu-id="5ee47-294">hello itt különbség, hogy $vnet5gw nem lehet közvetlenül beolvasni, mert egy másik előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="5ee47-294">hello difference here is that $vnet5gw cannot be obtained directly because it is in a different subscription.</span></span> <span data-ttu-id="5ee47-295">A fenti lépések hello előfizetés 1 közölt hello értékekkel kell toocreate egy új PowerShell-objektum.</span><span class="sxs-lookup"><span data-stu-id="5ee47-295">You will need toocreate a new PowerShell object with hello values communicated from Subscription 1 in hello steps above.</span></span> <span data-ttu-id="5ee47-296">Az alábbi példa hello használata.</span><span class="sxs-lookup"><span data-stu-id="5ee47-296">Use hello example below.</span></span> <span data-ttu-id="5ee47-297">Hello nevét, a azonosítója és a megosztott kulcs cserélje le a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="5ee47-297">Replace hello Name, Id, and shared key with your own values.</span></span> <span data-ttu-id="5ee47-298">fontos dolog hello megosztott kulcs hello egyeznie kell mindkét kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="5ee47-298">hello important thing is that hello shared key must match for both connections.</span></span> <span data-ttu-id="5ee47-299">Kapcsolat létrehozása során toocomplete rövid is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="5ee47-299">Creating a connection can take a short while toocomplete.</span></span>

  <span data-ttu-id="5ee47-300">Csatlakozás tooSubscription 1 hello például a következő futtatása előtt:</span><span class="sxs-lookup"><span data-stu-id="5ee47-300">Connect tooSubscription 1 before running hello following example:</span></span>

  ```powershell
  $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet5gw.Name = "VNet5GW"
  $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
  $Connection15 = "VNet1toVNet5"
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. <span data-ttu-id="5ee47-301">**[Előfizetés 5]**  Hello TestVNet5 tooTestVNet1 kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5ee47-301">**[Subscription 5]** Create hello TestVNet5 tooTestVNet1 connection.</span></span> <span data-ttu-id="5ee47-302">Ez a lépés akkor hasonló toohello valamelyik fenti, kivéve a TestVNet5 tooTestVNet1 hello kapcsolatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5ee47-302">This step is similar toohello one above, except you are creating hello connection from TestVNet5 tooTestVNet1.</span></span> <span data-ttu-id="5ee47-303">hello azonos előfizetés 1-től kapott hello értékek alapján PowerShell-objektum létrehozásának folyamatát, valamint alkalmazza itt.</span><span class="sxs-lookup"><span data-stu-id="5ee47-303">hello same process of creating a PowerShell object based on hello values obtained from Subscription 1 applies here as well.</span></span> <span data-ttu-id="5ee47-304">Ebben a lépésben ügyeljen, hogy megfelel-e a megosztott hello kulcsok.</span><span class="sxs-lookup"><span data-stu-id="5ee47-304">In this step, be sure that hello shared keys match.</span></span>

  <span data-ttu-id="5ee47-305">Csatlakozás tooSubscription 5 hello például a következő futtatása előtt:</span><span class="sxs-lookup"><span data-stu-id="5ee47-305">Connect tooSubscription 5 before running hello following example:</span></span>

  ```powershell
  $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet1gw.Name = "VNet1GW"
  $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```

## <span data-ttu-id="5ee47-306"><a name="verify"></a>Hogyan tooverify kapcsolat</span><span class="sxs-lookup"><span data-stu-id="5ee47-306"><a name="verify"></a>How tooverify a connection</span></span>

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <span data-ttu-id="5ee47-307"><a name="faq"></a>Virtuális hálózatok közötti kapcsolat – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="5ee47-307"><a name="faq"></a>VNet-to-VNet FAQ</span></span>

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="5ee47-308">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ee47-308">Next steps</span></span>

* <span data-ttu-id="5ee47-309">Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-309">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="5ee47-310">Lásd: hello [Virtual Machines – dokumentáció](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) további információt.</span><span class="sxs-lookup"><span data-stu-id="5ee47-310">See hello [Virtual Machines documentation](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) for more information.</span></span>
* <span data-ttu-id="5ee47-311">A BGP kapcsolatos információkért lásd: hello [BGP áttekintése](vpn-gateway-bgp-overview.md) és [hogyan tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="5ee47-311">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
