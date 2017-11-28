---
title: "Hálózati biztonsági csoport – PowerShell hibaelhárítása |} Microsoft Docs"
description: "Ismerje meg a hálózati biztonsági csoportok hibaelhárítása az Azure PowerShell használata Azure Resource Manager üzembe helyezési modellben."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 4c732bb7-5cb1-40af-9e6d-a2a307c2a9c4
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 5edaf7197576ac1c0bd1fc6bed21fd65ed135106
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a><span data-ttu-id="c7c53-103">Hálózati biztonsági csoportok az Azure PowerShell hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="c7c53-103">Troubleshoot Network Security Groups using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c7c53-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c7c53-104">Azure Portal</span></span>](virtual-network-nsg-troubleshoot-portal.md)
> * [<span data-ttu-id="c7c53-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c7c53-105">PowerShell</span></span>](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

<span data-ttu-id="c7c53-106">Ha konfigurálva a hálózati biztonsági csoportokkal (NSG-k) a virtuális gépen (VM), és virtuális gép kapcsolódási problémákat tapasztal, ez a cikk áttekintést diagnosztika képességet biztosít a További hibaelhárítás elősegítése érdekében az NSG-ket.</span><span class="sxs-lookup"><span data-stu-id="c7c53-106">If you configured Network Security Groups (NSGs) on your virtual machine (VM) and are experiencing VM connectivity issues, this article provides an overview of diagnostics capabilities for NSGs to help troubleshoot further.</span></span>

<span data-ttu-id="c7c53-107">Az NSG-k lehetővé teszik a típusú forgalom, hogy a virtuális gépek (VM) mindkét folyamata.</span><span class="sxs-lookup"><span data-stu-id="c7c53-107">NSGs enable you to control the types of traffic that flow in and out of your virtual machines (VMs).</span></span> <span data-ttu-id="c7c53-108">NSG-ket alhálózatokra egy Azure virtuális hálózatot (VNet), hálózati adapterek (NIC) vagy mindkettőt alkalmazhatók.</span><span class="sxs-lookup"><span data-stu-id="c7c53-108">NSGs can be applied to subnets in an Azure Virtual Network (VNet), network interfaces (NIC), or both.</span></span> <span data-ttu-id="c7c53-109">A hatékony szabályokat egy hálózati adapter egyszerűsítése érdekében a szabályokat, amelyek szerepelnek az NSG-ket egy hálózati adapterre alkalmazza, és az alhálózat van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="c7c53-109">The effective rules applied to a NIC are an aggregation of the rules that exist in the NSGs applied to a NIC and the subnet it is connected to.</span></span> <span data-ttu-id="c7c53-110">Ezek az NSG-k között szabályok néha ütköznek egymással, és hatással lehet a virtuális gépek hálózati kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="c7c53-110">Rules across these NSGs can sometimes conflict with each other and impact a VM's network connectivity.</span></span>  

<span data-ttu-id="c7c53-111">Az NSG-k, az összes hatékony biztonsági szabály tekintheti meg a virtuális gép hálózati adapterek alkalmazott.</span><span class="sxs-lookup"><span data-stu-id="c7c53-111">You can view all the effective security rules from your NSGs, as applied on your VM's NICs.</span></span> <span data-ttu-id="c7c53-112">Ez a cikk bemutatja, hogyan VM csatlakozási problémák ezek a szabályok használatával az Azure Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="c7c53-112">This article shows how to troubleshoot VM connectivity issues using these rules in the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="c7c53-113">Ha még nem ismeri a virtuális hálózat és NSG fogalmakat, olvassa el a [virtuális hálózati](virtual-networks-overview.md) és [hálózati biztonsági csoportok](virtual-networks-nsg.md) áttekintése cikkeket.</span><span class="sxs-lookup"><span data-stu-id="c7c53-113">If you're not familiar with VNet and NSG concepts, read the [Virtual network](virtual-networks-overview.md) and [Network security groups](virtual-networks-nsg.md) overview articles.</span></span>

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a><span data-ttu-id="c7c53-114">Hatékony biztonsági szabályok használata hibák elhárításához a virtuális gép forgalom áramlását</span><span class="sxs-lookup"><span data-stu-id="c7c53-114">Using Effective Security Rules to troubleshoot VM traffic flow</span></span>
<span data-ttu-id="c7c53-115">A következő forgatókönyv gyakori kapcsolati probléma példája:</span><span class="sxs-lookup"><span data-stu-id="c7c53-115">The scenario that follows is an example of a common connection problem:</span></span>

<span data-ttu-id="c7c53-116">A virtuális gépek nevű *VM1* nevű alhálózat része *Alhalozat_1* nevű egy Vneten belül *WestUS-VNet1*.</span><span class="sxs-lookup"><span data-stu-id="c7c53-116">A VM named *VM1* is part of a subnet named *Subnet1* within a VNet named *WestUS-VNet1*.</span></span> <span data-ttu-id="c7c53-117">A virtuális gép RDP Funkciót használnak a 3389-es TCP-porton keresztül történő csatlakozásra tett kísérlet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="c7c53-117">An attempt to connect to the VM using RDP over TCP port 3389 fails.</span></span> <span data-ttu-id="c7c53-118">Az NSG-k két hálózati adapter szintjén alkalmazhatóak *VM1-NIC1* és az alhálózati *Alhalozat_1*.</span><span class="sxs-lookup"><span data-stu-id="c7c53-118">NSGs are applied at both the NIC *VM1-NIC1* and the subnet *Subnet1*.</span></span> <span data-ttu-id="c7c53-119">Forgalom 3389-es TCP-port engedélyezve van a hálózati interfészhez társított NSG *VM1-NIC1*, azonban a TCP ping VM1 a következőre port 3389 meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="c7c53-119">Traffic to TCP port 3389 is allowed in the NSG associated with the network interface *VM1-NIC1*, however TCP ping to VM1's port 3389 fails.</span></span>

<span data-ttu-id="c7c53-120">Ebben a példában a 3389-es portot használja, amíg az alábbi lépések segítségével határozza meg a bejövő és kimenő kapcsolódási hibák bármely porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="c7c53-120">While this example uses TCP port 3389, the following steps can be used to determine inbound and outbound connection failures over any port.</span></span>

## <a name="detailed-troubleshooting-steps"></a><span data-ttu-id="c7c53-121">Részletes hibaelhárítási lépéseket</span><span class="sxs-lookup"><span data-stu-id="c7c53-121">Detailed Troubleshooting Steps</span></span>
<span data-ttu-id="c7c53-122">Az alábbi lépésekkel hibáinak elhárítása az NSG-ket a virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="c7c53-122">Complete the following steps to troubleshoot NSGs for a VM:</span></span>

1. <span data-ttu-id="c7c53-123">Indítsa el az Azure PowerShell-munkamenetet és a bejelentkezés az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="c7c53-123">Start an Azure PowerShell session and login to Azure.</span></span> <span data-ttu-id="c7c53-124">Ha nem ismeri az Azure PowerShell használatával, olvassa el a [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview) cikk.</span><span class="sxs-lookup"><span data-stu-id="c7c53-124">If you're not familiar with using Azure PowerShell, read the [How to install and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="c7c53-125">Adja meg a következő parancs sikeresen lefut egy nevű hálózati adapterre alkalmazza minden NSG-szabályok *VM1-NIC1* erőforráscsoportban *RG1*:</span><span class="sxs-lookup"><span data-stu-id="c7c53-125">Enter the following command to return all NSG rules applied to a NIC named *VM1-NIC1* in the resource group *RG1*:</span></span>
   
        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > <span data-ttu-id="c7c53-126">Ha egy hálózati adapter neve nem tudja, adjon meg egy erőforráscsoportot az összes hálózati adapter nevének beolvasása a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c7c53-126">If you don't know the name of a NIC, enter the following command to retrieve the names of all NICs in a resource group:</span></span> 
   > 
   > `Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`
   > 
   > 
   
    <span data-ttu-id="c7c53-127">A következő szöveg látható egy minta vissza hatékony szabályok kimenetét a *VM1-NIC1* NIC:</span><span class="sxs-lookup"><span data-stu-id="c7c53-127">The following text is a sample of the effective rules output returned for the *VM1-NIC1* NIC:</span></span>
   
        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
   
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]
   
    <span data-ttu-id="c7c53-128">Vegye figyelembe a kimenete a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="c7c53-128">Note the following information in the output:</span></span>
   
   * <span data-ttu-id="c7c53-129">Két **hálózati biztonsági csoporthoz tartozik** szakaszok: tartozik hozzá egy alhálózat (*Alhalozat_1*) és egy hálózati Adapterhez társított (*VM1-NIC1*).</span><span class="sxs-lookup"><span data-stu-id="c7c53-129">There are two **NetworkSecurityGroup** sections: One is associated with a subnet (*Subnet1*) and one is associated with a NIC (*VM1-NIC1*).</span></span> <span data-ttu-id="c7c53-130">Ebben a példában egy NSG-t az egyes telepítve van.</span><span class="sxs-lookup"><span data-stu-id="c7c53-130">In this example, an NSG has been applied to each.</span></span>
   * <span data-ttu-id="c7c53-131">**Társítás** jeleníti meg az erőforrás (alhálózati vagy hálózati), egy adott NSG társítva.</span><span class="sxs-lookup"><span data-stu-id="c7c53-131">**Association** shows the resource (subnet or NIC) a given NSG is associated with.</span></span> <span data-ttu-id="c7c53-132">Ha az NSG-erőforrás áthelyezése vagy hozzárendelésének megszüntetése a parancs futtatása előtt azonnal, szükség lehet Várjon néhány másodpercet, hogy tükrözze a parancs kimenetében a változtatás.</span><span class="sxs-lookup"><span data-stu-id="c7c53-132">If the NSG resource is moved/disassociated immediately before running this command, you may need to wait a few seconds for the change to reflect in the command output.</span></span> 
   * <span data-ttu-id="c7c53-133">A szabály nevét, amelyek végrehajtásával kerüli meg a *defaultSecurityRules*: amikor egy NSG jön létre, néhány alapértelmezett biztonsági szabályok jönnek létre benne.</span><span class="sxs-lookup"><span data-stu-id="c7c53-133">The rule names that are prefaced with *defaultSecurityRules*: When an NSG is created, several default security rules are created within it.</span></span> <span data-ttu-id="c7c53-134">Alapértelmezett szabályok nem távolítható el, de magasabb prioritású szabályokkal felülbírálható lesz.</span><span class="sxs-lookup"><span data-stu-id="c7c53-134">Default rules can't be removed, but they can be overridden with higher priority rules.</span></span>
     <span data-ttu-id="c7c53-135">Olvassa el a [NSG áttekintése](virtual-networks-nsg.md#default-rules) cikk további NSG bővebben alapértelmezett biztonsági szabályokat.</span><span class="sxs-lookup"><span data-stu-id="c7c53-135">Read the [NSG overview](virtual-networks-nsg.md#default-rules) article to learn more about NSG default security rules.</span></span>
   * <span data-ttu-id="c7c53-136">**ExpandedAddressPrefix** bővíti a címelőtagokat NSG alapértelmezett címkék.</span><span class="sxs-lookup"><span data-stu-id="c7c53-136">**ExpandedAddressPrefix** expands the address prefixes for NSG default tags.</span></span> <span data-ttu-id="c7c53-137">Címkék több címelőtagokat jelölik.</span><span class="sxs-lookup"><span data-stu-id="c7c53-137">Tags represent multiple address prefixes.</span></span> <span data-ttu-id="c7c53-138">A címkék bővítése akkor lehet hasznos, amikor adott címelőtagokat és a virtuális gép hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="c7c53-138">Expansion of the tags can be useful when troubleshooting VM connectivity to/from specific address prefixes.</span></span> <span data-ttu-id="c7c53-139">Például a VNETBEN társviszony-létesítést, VIRTUAL_NETWORK címke bontja ki társviszonyban VNet-előtagok az előző kimenet megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="c7c53-139">For example, with VNET peering, VIRTUAL_NETWORK tag expands to show peered VNet prefixes in the previous output.</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="c7c53-140">A parancs csak azt mutatja be hatékony szabályokat Ha egy NSG tartozik alhálózat, egy hálózati Adaptert, vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="c7c53-140">The command only shows effective rules if an NSG is associated with either a subnet, a NIC, or both.</span></span> <span data-ttu-id="c7c53-141">Előfordulhat, hogy a virtuális gépek több hálózati adapterrel alkalmazott különböző NSG.</span><span class="sxs-lookup"><span data-stu-id="c7c53-141">A VM may have multiple NICs with different NSGs applied.</span></span> <span data-ttu-id="c7c53-142">Hibaelhárításához, futtassa a parancsot az egyes hálózati adapterhez.</span><span class="sxs-lookup"><span data-stu-id="c7c53-142">When troubleshooting, run the command for each NIC.</span></span>
     > 
     > 
3. <span data-ttu-id="c7c53-143">Megkönnyítése érdekében NSG-szabályok nagyobb számú szűrés, adja meg a további hibaelhárítást kell végeznie a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="c7c53-143">To ease filtering over larger number of NSG rules, enter the following commands to troubleshoot further:</span></span> 
   
        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView
   
    <span data-ttu-id="c7c53-144">Az RDP-forgalmát (TCP-port 3389-es), a szűrő alkalmazása a Rács nézet, az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="c7c53-144">A filter for RDP traffic (TCP port 3389), is applied to the grid view, as shown in the following picture:</span></span>
   
    ![Szabályok listája](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
4. <span data-ttu-id="c7c53-146">Ahogy látja, a táblázatos nézetben, vannak-e mindkét engedélyezése és megtagadási szabályoknak RDP.</span><span class="sxs-lookup"><span data-stu-id="c7c53-146">As you can see in the grid view, there are both allow and deny rules for RDP.</span></span> <span data-ttu-id="c7c53-147">A kimenet a 2. lépésben azt mutatja, hogy a *DenyRDP* szabály van alkalmazva az NSG.</span><span class="sxs-lookup"><span data-stu-id="c7c53-147">The output from step 2 shows that the *DenyRDP* rule is in the NSG applied to the subnet.</span></span> <span data-ttu-id="c7c53-148">A bejövő szabályok NSG-ket alkalmazva először dolgoznak.</span><span class="sxs-lookup"><span data-stu-id="c7c53-148">For inbound rules, NSGs applied to the subnet are processed first.</span></span> <span data-ttu-id="c7c53-149">Ha a program egyezést talál, a hálózati illesztő alkalmazott NSG nem lett feldolgozva.</span><span class="sxs-lookup"><span data-stu-id="c7c53-149">If a match is found, the NSG applied to the network interface is not processed.</span></span> <span data-ttu-id="c7c53-150">Ebben az esetben a *DenyRDP* az alhálózatból szabály blokkolja a virtuális gép RDP (**VM1**).</span><span class="sxs-lookup"><span data-stu-id="c7c53-150">In this case, the *DenyRDP* rule from the subnet blocks RDP to the VM (**VM1**).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c7c53-151">Előfordulhat, hogy a virtuális gépek több hálózati adapter nem csatlakoztatható.</span><span class="sxs-lookup"><span data-stu-id="c7c53-151">A VM may have multiple NICs attached to it.</span></span> <span data-ttu-id="c7c53-152">Minden egyes lehet, hogy kapcsolódik egy másik alhálózat.</span><span class="sxs-lookup"><span data-stu-id="c7c53-152">Each may be connected to a different subnet.</span></span> <span data-ttu-id="c7c53-153">Mivel a parancs az előző lépésben futtatása ellen egy hálózati Adaptert, fontos győződjön meg arról, hogy megadja a hálózati kapcsolat kapcsolatos hibát tapasztal.</span><span class="sxs-lookup"><span data-stu-id="c7c53-153">Since the commands in the previous steps are run against a NIC, it's important to ensure that you specify the NIC you're having the connectivity failure to.</span></span> <span data-ttu-id="c7c53-154">Ha nem biztos benne, mindig a parancsok alapján futtathatók egyes hálózati adapterek, a virtuális Géphez csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="c7c53-154">If you're not sure, you can always run the commands against each NIC attached to the VM.</span></span>
   > 
   > 
5. <span data-ttu-id="c7c53-155">Távoli asztali eléréséhez VM1, módosítsa a *megtagadása RDP (3389-es)* történő *engedélyezése RDP(3389)* a a **Alhalozat_1-NSG** NSG.</span><span class="sxs-lookup"><span data-stu-id="c7c53-155">To RDP into VM1, change the *Deny RDP (3389)* rule to *Allow RDP(3389)* in the **Subnet1-NSG** NSG.</span></span> <span data-ttu-id="c7c53-156">Győződjön meg arról, hogy a virtuális gép egy RDP-kapcsolat megnyitásával, vagy a PsPing eszköz használata nyitva-e 3389-es TCP-port.</span><span class="sxs-lookup"><span data-stu-id="c7c53-156">Confirm that TCP port 3389 is open by opening an RDP connection to the VM or using the PsPing tool.</span></span> <span data-ttu-id="c7c53-157">További tudnivalók a PsPing olvasásával a [PsPing letöltési oldala](https://technet.microsoft.com/sysinternals/psping.aspx)</span><span class="sxs-lookup"><span data-stu-id="c7c53-157">You can learn more about PsPing by reading the [PsPing download page](https://technet.microsoft.com/sysinternals/psping.aspx)</span></span>
   
    <span data-ttu-id="c7c53-158">Lehet, vagy a szabályok eltávolítása egy NSG-t a következő parancs kimenetében található információk segítségével:</span><span class="sxs-lookup"><span data-stu-id="c7c53-158">You can or remove rules from an NSG by using the information in the output from the following command:</span></span>
   
        Get-Help *-AzureRmNetworkSecurityRuleConfig

## <a name="considerations"></a><span data-ttu-id="c7c53-159">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="c7c53-159">Considerations</span></span>
<span data-ttu-id="c7c53-160">Kapcsolódási problémák elhárításakor, vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="c7c53-160">Consider the following points when troubleshooting connectivity problems:</span></span>

* <span data-ttu-id="c7c53-161">Alapértelmezett NSG-szabályok lesz az internetről bejövő hozzáférésének letiltására, és csak a virtuális hálózat engedélyezése érkező bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="c7c53-161">Default NSG rules will block inbound access from the internet and only permit VNet inbound traffic.</span></span> <span data-ttu-id="c7c53-162">Szabályok explicit módon meg kell adni a bejövő hozzáférés engedélyezése az internetről, szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="c7c53-162">Rules should be explicitly added to allow inbound access from Internet, as required.</span></span>
* <span data-ttu-id="c7c53-163">Ha nincs NSG biztonsági szabály, amely a virtuális gépek hálózati kapcsolattal sikertelen lesz, a probléma oka a következő lehet:</span><span class="sxs-lookup"><span data-stu-id="c7c53-163">If there are no NSG security rules causing a VM’s network connectivity to fail, the problem may be due to:</span></span>
  * <span data-ttu-id="c7c53-164">A virtuális gép operációs rendszerben futó tűzfal szoftver</span><span class="sxs-lookup"><span data-stu-id="c7c53-164">Firewall software running within the VM's operating system</span></span>
  * <span data-ttu-id="c7c53-165">A virtuális készülékek vagy a helyszíni forgalmi útvonalak.</span><span class="sxs-lookup"><span data-stu-id="c7c53-165">Routes configured for virtual appliances or on-premises traffic.</span></span> <span data-ttu-id="c7c53-166">Internetes forgalmat a helyszíni kényszerített bújtatás keresztül lehet átirányítani.</span><span class="sxs-lookup"><span data-stu-id="c7c53-166">Internet traffic can be redirected to on-premises via forced-tunneling.</span></span> <span data-ttu-id="c7c53-167">Az RDP/SSH-kapcsolatot az internetről a virtuális gép nem feltétlenül ezt a beállítást, attól függően, hogy miként kezeli a helyszíni hálózati hardver a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="c7c53-167">An RDP/SSH connection from the Internet to your VM may not work with this setting, depending on how the on-premises network hardware handles this traffic.</span></span> <span data-ttu-id="c7c53-168">Olvassa el a [hibaelhárítási útvonalak](virtual-network-routes-troubleshoot-powershell.md) cikk áttekintésével megismerheti, hogyan, előfordulhat, hogy a forgalmat a virtuális Gépet a bejövő és kimenő adatforgalma kell akadályozó útvonal problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="c7c53-168">Read the [Troubleshooting Routes](virtual-network-routes-troubleshoot-powershell.md) article to learn how to diagnose route problems that may be impeding the flow of traffic in and out of the VM.</span></span> 
* <span data-ttu-id="c7c53-169">Ha Vnetek, alapértelmezés szerint rendelkezik társviszonyban, VIRTUAL_NETWORK címke automatikusan bővített előtagok tartalmazza az társviszonyban Vnetek.</span><span class="sxs-lookup"><span data-stu-id="c7c53-169">If you have peered VNets, by default, the VIRTUAL_NETWORK tag will automatically expand to include prefixes for peered VNets.</span></span> <span data-ttu-id="c7c53-170">Ezeket az előtagok megtekintheti a **ExpandedAddressPrefix** lista bármely Vnetben társviszony-létesítési kapcsolat kapcsolatos problémák elhárítása.</span><span class="sxs-lookup"><span data-stu-id="c7c53-170">You can view these prefixes in the **ExpandedAddressPrefix** list, to troubleshoot any issues related to VNet peering connectivity.</span></span> 
* <span data-ttu-id="c7c53-171">Hatékony biztonsági szabályok csak jelennek-e a virtuális hálózati adapter és vagy alhálózat társított NSG-t.</span><span class="sxs-lookup"><span data-stu-id="c7c53-171">Effective security rules are only shown if there is an NSG associated with the VM’s NIC and or subnet.</span></span> 
* <span data-ttu-id="c7c53-172">Ha nincs hálózati adapter társított NSG-k, vagy alhálózat és rendelkezik a nyilvános IP-cím, a virtuális Géphez rendelt, minden port nyissa meg a bejövő és kimenő hozzáférést lesz.</span><span class="sxs-lookup"><span data-stu-id="c7c53-172">If there are no NSGs associated with the NIC or subnet and you have a public IP address assigned to your VM, all ports will be open for inbound and outbound access.</span></span> <span data-ttu-id="c7c53-173">Ha a virtuális gép egy nyilvános IP-címmel rendelkezik, erősen ajánlott NSG-ket alkalmaz a hálózati adapter vagy az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="c7c53-173">If the VM has a public IP address, applying NSGs to the NIC or subnet is strongly recommended.</span></span>  

