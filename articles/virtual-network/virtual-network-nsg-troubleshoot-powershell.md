---
title: "Hálózati biztonsági csoport – aaaTroubleshoot PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot hálózati biztonsági csoportok hello Azure Resource Manager telepítési modell Azure PowerShell használatával."
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
ms.openlocfilehash: 95fd60fa72cf6d17fa990e3c3eb7d980878f7c15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a><span data-ttu-id="9c60e-103">Hálózati biztonsági csoportok az Azure PowerShell hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="9c60e-103">Troubleshoot Network Security Groups using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9c60e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9c60e-104">Azure Portal</span></span>](virtual-network-nsg-troubleshoot-portal.md)
> * [<span data-ttu-id="9c60e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9c60e-105">PowerShell</span></span>](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

<span data-ttu-id="9c60e-106">Ha konfigurálva a hálózati biztonsági csoportokkal (NSG-k) a virtuális gépen (VM), és virtuális gép kapcsolódási problémákat tapasztal, ez a cikk áttekintést diagnosztikai képességek az NSG-k toohelp további hibaelhárítást kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="9c60e-106">If you configured Network Security Groups (NSGs) on your virtual machine (VM) and are experiencing VM connectivity issues, this article provides an overview of diagnostics capabilities for NSGs toohelp troubleshoot further.</span></span>

<span data-ttu-id="9c60e-107">Az NSG-k lehetővé teszik a toocontrol hello típusú forgalom, hogy a virtuális gépek (VM) mindkét folyamata.</span><span class="sxs-lookup"><span data-stu-id="9c60e-107">NSGs enable you toocontrol hello types of traffic that flow in and out of your virtual machines (VMs).</span></span> <span data-ttu-id="9c60e-108">NSG-ket lehet alkalmazott toosubnets egy Azure virtuális hálózatot (VNet), hálózati adapterek (NIC) vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="9c60e-108">NSGs can be applied toosubnets in an Azure Virtual Network (VNet), network interfaces (NIC), or both.</span></span> <span data-ttu-id="9c60e-109">hello hatékony szabályokat tooa hálózati adapter létező hello alkalmazott NSG-k tooa hálózati adapter és hello alhálózaton van csatlakoztatva hello szabály összesítést.</span><span class="sxs-lookup"><span data-stu-id="9c60e-109">hello effective rules applied tooa NIC are an aggregation of hello rules that exist in hello NSGs applied tooa NIC and hello subnet it is connected to.</span></span> <span data-ttu-id="9c60e-110">Ezek az NSG-k között szabályok néha ütköznek egymással, és hatással lehet a virtuális gépek hálózati kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="9c60e-110">Rules across these NSGs can sometimes conflict with each other and impact a VM's network connectivity.</span></span>  

<span data-ttu-id="9c60e-111">Az NSG-k, az összes hello hatékony biztonsági szabály tekintheti meg a virtuális gép hálózati adapterek alkalmazott.</span><span class="sxs-lookup"><span data-stu-id="9c60e-111">You can view all hello effective security rules from your NSGs, as applied on your VM's NICs.</span></span> <span data-ttu-id="9c60e-112">Ez a cikk bemutatja, hogyan tootroubleshoot VM ezeket szabályok használata a problémák hello Azure Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="9c60e-112">This article shows how tootroubleshoot VM connectivity issues using these rules in hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="9c60e-113">Ha még nem ismeri a virtuális hálózat és NSG fogalmakat, olvassa el a hello [virtuális hálózati](virtual-networks-overview.md) és [hálózati biztonsági csoportok](virtual-networks-nsg.md) áttekintése cikkeket.</span><span class="sxs-lookup"><span data-stu-id="9c60e-113">If you're not familiar with VNet and NSG concepts, read hello [Virtual network](virtual-networks-overview.md) and [Network security groups](virtual-networks-nsg.md) overview articles.</span></span>

## <a name="using-effective-security-rules-tootroubleshoot-vm-traffic-flow"></a><span data-ttu-id="9c60e-114">Hatékony biztonsági szabályok tootroubleshoot VM adatforgalmat használatával</span><span class="sxs-lookup"><span data-stu-id="9c60e-114">Using Effective Security Rules tootroubleshoot VM traffic flow</span></span>
<span data-ttu-id="9c60e-115">hello-forgatókönyvekben, amelyek a következő gyakori kapcsolati probléma példája:</span><span class="sxs-lookup"><span data-stu-id="9c60e-115">hello scenario that follows is an example of a common connection problem:</span></span>

<span data-ttu-id="9c60e-116">A virtuális gépek nevű *VM1* nevű alhálózat része *Alhalozat_1* nevű egy Vneten belül *WestUS-VNet1*.</span><span class="sxs-lookup"><span data-stu-id="9c60e-116">A VM named *VM1* is part of a subnet named *Subnet1* within a VNet named *WestUS-VNet1*.</span></span> <span data-ttu-id="9c60e-117">Sikertelen a virtuális gépet RDP a 3389-es TCP-porton keresztül egy kísérlet tooconnect toohello.</span><span class="sxs-lookup"><span data-stu-id="9c60e-117">An attempt tooconnect toohello VM using RDP over TCP port 3389 fails.</span></span> <span data-ttu-id="9c60e-118">Az NSG-k mindkét hello NIC szintjén alkalmazhatóak *VM1-NIC1* és alhálózati hello *Alhalozat_1*.</span><span class="sxs-lookup"><span data-stu-id="9c60e-118">NSGs are applied at both hello NIC *VM1-NIC1* and hello subnet *Subnet1*.</span></span> <span data-ttu-id="9c60e-119">Forgalom tooTCP 3389-es port használata engedélyezett hello hello hálózati interfészhez társított NSG *VM1-NIC1*, azonban a TCP Pingelje meg tooVM1 port 3389 sikertelen.</span><span class="sxs-lookup"><span data-stu-id="9c60e-119">Traffic tooTCP port 3389 is allowed in hello NSG associated with hello network interface *VM1-NIC1*, however TCP ping tooVM1's port 3389 fails.</span></span>

<span data-ttu-id="9c60e-120">Ebben a példában a 3389-es portot használja, amíg hello lépéseket követve lehet használt toodetermine bejövő és kimenő kapcsolódási hibák bármely porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="9c60e-120">While this example uses TCP port 3389, hello following steps can be used toodetermine inbound and outbound connection failures over any port.</span></span>

## <a name="detailed-troubleshooting-steps"></a><span data-ttu-id="9c60e-121">Részletes hibaelhárítási lépéseket</span><span class="sxs-lookup"><span data-stu-id="9c60e-121">Detailed Troubleshooting Steps</span></span>
<span data-ttu-id="9c60e-122">Hajtsa végre a következő lépések tootroubleshoot NSG-ket a virtuális gépek hello:</span><span class="sxs-lookup"><span data-stu-id="9c60e-122">Complete hello following steps tootroubleshoot NSGs for a VM:</span></span>

1. <span data-ttu-id="9c60e-123">Indítsa el az Azure PowerShell-munkamenet és bejelentkezési tooAzure.</span><span class="sxs-lookup"><span data-stu-id="9c60e-123">Start an Azure PowerShell session and login tooAzure.</span></span> <span data-ttu-id="9c60e-124">Ha nem ismeri az Azure PowerShell használatával, olvassa el a hello [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk.</span><span class="sxs-lookup"><span data-stu-id="9c60e-124">If you're not familiar with using Azure PowerShell, read hello [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="9c60e-125">Adja meg a következő parancs tooreturn minden NSG-szabályok alkalmazása tooa nevű hálózati hello *VM1-NIC1* hello erőforráscsoportban *RG1*:</span><span class="sxs-lookup"><span data-stu-id="9c60e-125">Enter hello following command tooreturn all NSG rules applied tooa NIC named *VM1-NIC1* in hello resource group *RG1*:</span></span>
   
        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > <span data-ttu-id="9c60e-126">Ha egy hálózati adapter hello neve nem tudja, adja meg a következő parancs tooretrieve hello nevét egy erőforráscsoportban található hálózati adapterek összes hello:</span><span class="sxs-lookup"><span data-stu-id="9c60e-126">If you don't know hello name of a NIC, enter hello following command tooretrieve hello names of all NICs in a resource group:</span></span> 
   > 
   > `Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`
   > 
   > 
   
    <span data-ttu-id="9c60e-127">hello következő szövege minta hello hatékony szabályok kimenete hello vissza *VM1-NIC1* NIC:</span><span class="sxs-lookup"><span data-stu-id="9c60e-127">hello following text is a sample of hello effective rules output returned for hello *VM1-NIC1* NIC:</span></span>
   
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
   
    <span data-ttu-id="9c60e-128">Vegye figyelembe a következő információ hello kimenet hello:</span><span class="sxs-lookup"><span data-stu-id="9c60e-128">Note hello following information in hello output:</span></span>
   
   * <span data-ttu-id="9c60e-129">Két **hálózati biztonsági csoporthoz tartozik** szakaszok: tartozik hozzá egy alhálózat (*Alhalozat_1*) és egy hálózati Adapterhez társított (*VM1-NIC1*).</span><span class="sxs-lookup"><span data-stu-id="9c60e-129">There are two **NetworkSecurityGroup** sections: One is associated with a subnet (*Subnet1*) and one is associated with a NIC (*VM1-NIC1*).</span></span> <span data-ttu-id="9c60e-130">Ebben a példában egy NSG alkalmazott tooeach volt.</span><span class="sxs-lookup"><span data-stu-id="9c60e-130">In this example, an NSG has been applied tooeach.</span></span>
   * <span data-ttu-id="9c60e-131">**Társítás** hello erőforrás (alhálózati vagy hálózati adapter) jeleníti meg egy adott NSG társítva.</span><span class="sxs-lookup"><span data-stu-id="9c60e-131">**Association** shows hello resource (subnet or NIC) a given NSG is associated with.</span></span> <span data-ttu-id="9c60e-132">Ha hello NSG-erőforrás áthelyezése vagy hozzárendelésének megszüntetése a parancs futtatása előtt azonnal, szükséges toowait néhány másodpercen belül hello módosítása tooreflect hello parancs kimenetében.</span><span class="sxs-lookup"><span data-stu-id="9c60e-132">If hello NSG resource is moved/disassociated immediately before running this command, you may need toowait a few seconds for hello change tooreflect in hello command output.</span></span> 
   * <span data-ttu-id="9c60e-133">hello vannak végrehajtásával kerüli meg a szabály neve *defaultSecurityRules*: amikor egy NSG jön létre, néhány alapértelmezett biztonsági szabályok jönnek létre benne.</span><span class="sxs-lookup"><span data-stu-id="9c60e-133">hello rule names that are prefaced with *defaultSecurityRules*: When an NSG is created, several default security rules are created within it.</span></span> <span data-ttu-id="9c60e-134">Alapértelmezett szabályok nem távolítható el, de magasabb prioritású szabályokkal felülbírálható lesz.</span><span class="sxs-lookup"><span data-stu-id="9c60e-134">Default rules can't be removed, but they can be overridden with higher priority rules.</span></span>
     <span data-ttu-id="9c60e-135">Olvasási hello [NSG áttekintése](virtual-networks-nsg.md#default-rules) NSG kapcsolatos további információkért a cikk toolearn alapértelmezett biztonsági szabályokat.</span><span class="sxs-lookup"><span data-stu-id="9c60e-135">Read hello [NSG overview](virtual-networks-nsg.md#default-rules) article toolearn more about NSG default security rules.</span></span>
   * <span data-ttu-id="9c60e-136">**ExpandedAddressPrefix** hello címelőtagokat NSG alapértelmezett címkék bővíti.</span><span class="sxs-lookup"><span data-stu-id="9c60e-136">**ExpandedAddressPrefix** expands hello address prefixes for NSG default tags.</span></span> <span data-ttu-id="9c60e-137">Címkék több címelőtagokat jelölik.</span><span class="sxs-lookup"><span data-stu-id="9c60e-137">Tags represent multiple address prefixes.</span></span> <span data-ttu-id="9c60e-138">Hello címkék bővítése akkor lehet hasznos, ha adott címelőtagokat és a virtuális gép hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="9c60e-138">Expansion of hello tags can be useful when troubleshooting VM connectivity to/from specific address prefixes.</span></span> <span data-ttu-id="9c60e-139">A VNETBEN társviszony-létesítést, hogy VIRTUAL_NETWORK címke például bővíti tooshow társviszonyban VNet-előtagok hello előző kimenet.</span><span class="sxs-lookup"><span data-stu-id="9c60e-139">For example, with VNET peering, VIRTUAL_NETWORK tag expands tooshow peered VNet prefixes in hello previous output.</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="9c60e-140">hello parancs csak látható hatékony szabályokat, ha az NSG tartozik alhálózat, egy hálózati Adaptert, vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="9c60e-140">hello command only shows effective rules if an NSG is associated with either a subnet, a NIC, or both.</span></span> <span data-ttu-id="9c60e-141">Előfordulhat, hogy a virtuális gépek több hálózati adapterrel alkalmazott különböző NSG.</span><span class="sxs-lookup"><span data-stu-id="9c60e-141">A VM may have multiple NICs with different NSGs applied.</span></span> <span data-ttu-id="9c60e-142">Hibaelhárításához, hello parancs futtatása az egyes hálózati adapterhez.</span><span class="sxs-lookup"><span data-stu-id="9c60e-142">When troubleshooting, run hello command for each NIC.</span></span>
     > 
     > 
3. <span data-ttu-id="9c60e-143">NSG-szabályok, hogy a nagyobb számú szűrés tooease adja meg a következő további parancsok tootroubleshoot hello:</span><span class="sxs-lookup"><span data-stu-id="9c60e-143">tooease filtering over larger number of NSG rules, enter hello following commands tootroubleshoot further:</span></span> 
   
        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView
   
    <span data-ttu-id="9c60e-144">Az RDP-forgalmát (TCP-port 3389-es), a szűrő alkalmazása toohello rácsnézete, ahogy az alábbi képen hello:</span><span class="sxs-lookup"><span data-stu-id="9c60e-144">A filter for RDP traffic (TCP port 3389), is applied toohello grid view, as shown in hello following picture:</span></span>
   
    ![Szabályok listája](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
4. <span data-ttu-id="9c60e-146">Ahogy látja, hello táblázatos nézetben, vannak-e mindkét engedélyezése és megtagadási szabályoknak RDP.</span><span class="sxs-lookup"><span data-stu-id="9c60e-146">As you can see in hello grid view, there are both allow and deny rules for RDP.</span></span> <span data-ttu-id="9c60e-147">hello kimenetét a 2. lépés bemutatja, hogy hello *DenyRDP* szabály hello alkalmazott NSG toohello alhálózat szerepel.</span><span class="sxs-lookup"><span data-stu-id="9c60e-147">hello output from step 2 shows that hello *DenyRDP* rule is in hello NSG applied toohello subnet.</span></span> <span data-ttu-id="9c60e-148">A bejövő szabályok esetében alkalmazott NSG-k toohello alhálózati feldolgozása először.</span><span class="sxs-lookup"><span data-stu-id="9c60e-148">For inbound rules, NSGs applied toohello subnet are processed first.</span></span> <span data-ttu-id="9c60e-149">Ha van egyezés, hello alkalmazott NSG toohello hálózati illesztő nem lett feldolgozva.</span><span class="sxs-lookup"><span data-stu-id="9c60e-149">If a match is found, hello NSG applied toohello network interface is not processed.</span></span> <span data-ttu-id="9c60e-150">Ebben az esetben hello *DenyRDP* hello alhálózatból szabály blokkolja a virtuális gép RDP-toohello (**VM1**).</span><span class="sxs-lookup"><span data-stu-id="9c60e-150">In this case, hello *DenyRDP* rule from hello subnet blocks RDP toohello VM (**VM1**).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9c60e-151">Előfordulhat, hogy a virtuális gépek több hálózati adapter csatolt tooit.</span><span class="sxs-lookup"><span data-stu-id="9c60e-151">A VM may have multiple NICs attached tooit.</span></span> <span data-ttu-id="9c60e-152">Minden egyes lehet csatlakoztatott tooa alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="9c60e-152">Each may be connected tooa different subnet.</span></span> <span data-ttu-id="9c60e-153">Mivel hello parancs hello előző lépésekben futtatása ellen egy hálózati Adaptert, fontos megadott tooensure hello hálózati adapter a hello csatlakozási hiba lépett fel.</span><span class="sxs-lookup"><span data-stu-id="9c60e-153">Since hello commands in hello previous steps are run against a NIC, it's important tooensure that you specify hello NIC you're having hello connectivity failure to.</span></span> <span data-ttu-id="9c60e-154">Ha nem biztos benne, is minden esetben futtathatók hello parancsok minden csatlakoztatott hálózati toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="9c60e-154">If you're not sure, you can always run hello commands against each NIC attached toohello VM.</span></span>
   > 
   > 
5. <span data-ttu-id="9c60e-155">a VM1, módosítás hello tooRDP *megtagadása RDP (3389-es)* szabály túl*RDP(3389) engedélyezése* a hello **Alhalozat_1-NSG** NSG.</span><span class="sxs-lookup"><span data-stu-id="9c60e-155">tooRDP into VM1, change hello *Deny RDP (3389)* rule too*Allow RDP(3389)* in hello **Subnet1-NSG** NSG.</span></span> <span data-ttu-id="9c60e-156">Győződjön meg arról, hogy 3389-es TCP-port meg nyitva egy RDP-kapcsolat toohello VM megnyitásával, vagy hello PsPing eszköz használata.</span><span class="sxs-lookup"><span data-stu-id="9c60e-156">Confirm that TCP port 3389 is open by opening an RDP connection toohello VM or using hello PsPing tool.</span></span> <span data-ttu-id="9c60e-157">Terhelésekről további információt a PsPing által olvasása hello [PsPing letöltési oldala](https://technet.microsoft.com/sysinternals/psping.aspx)</span><span class="sxs-lookup"><span data-stu-id="9c60e-157">You can learn more about PsPing by reading hello [PsPing download page](https://technet.microsoft.com/sysinternals/psping.aspx)</span></span>
   
    <span data-ttu-id="9c60e-158">Lehet, vagy távolítsa el a szabályok egy NSG-t a használatával a következő parancs hello hello kimenete hello információi alapján:</span><span class="sxs-lookup"><span data-stu-id="9c60e-158">You can or remove rules from an NSG by using hello information in hello output from hello following command:</span></span>
   
        Get-Help *-AzureRmNetworkSecurityRuleConfig

## <a name="considerations"></a><span data-ttu-id="9c60e-159">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="9c60e-159">Considerations</span></span>
<span data-ttu-id="9c60e-160">Vegye figyelembe a következő pontok csatlakozási problémák elhárításakor hello:</span><span class="sxs-lookup"><span data-stu-id="9c60e-160">Consider hello following points when troubleshooting connectivity problems:</span></span>

* <span data-ttu-id="9c60e-161">Alapértelmezett NSG-szabályok blokkolja a bejövő hello hozzáférést internet és a virtuális hálózat engedély csak a bejövő forgalom.</span><span class="sxs-lookup"><span data-stu-id="9c60e-161">Default NSG rules will block inbound access from hello internet and only permit VNet inbound traffic.</span></span> <span data-ttu-id="9c60e-162">Szabályok kell adható hozzá közvetlen módon tooallow bejövő Internet, a szükséges hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="9c60e-162">Rules should be explicitly added tooallow inbound access from Internet, as required.</span></span>
* <span data-ttu-id="9c60e-163">Ha nincsenek, amely a virtuális gép hálózati kapcsolat toofail NSG biztonsági szabályok, hello a probléma oka a következő lehet:</span><span class="sxs-lookup"><span data-stu-id="9c60e-163">If there are no NSG security rules causing a VM’s network connectivity toofail, hello problem may be due to:</span></span>
  * <span data-ttu-id="9c60e-164">Hello virtuális gép operációs rendszerben futó tűzfal szoftver</span><span class="sxs-lookup"><span data-stu-id="9c60e-164">Firewall software running within hello VM's operating system</span></span>
  * <span data-ttu-id="9c60e-165">A virtuális készülékek vagy a helyszíni forgalmi útvonalak.</span><span class="sxs-lookup"><span data-stu-id="9c60e-165">Routes configured for virtual appliances or on-premises traffic.</span></span> <span data-ttu-id="9c60e-166">Internetes forgalom átirányított tooon helyszíni kényszerített bújtatás keresztül lehet.</span><span class="sxs-lookup"><span data-stu-id="9c60e-166">Internet traffic can be redirected tooon-premises via forced-tunneling.</span></span> <span data-ttu-id="9c60e-167">Az RDP/SSH-kapcsolat a hello Internet tooyour VM nem feltétlenül ezt a beállítást, attól függően, hogy miként hello a helyszíni hálózati hardver kezeli a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="9c60e-167">An RDP/SSH connection from hello Internet tooyour VM may not work with this setting, depending on how hello on-premises network hardware handles this traffic.</span></span> <span data-ttu-id="9c60e-168">Olvasási hello [hibaelhárítási útvonalak](virtual-network-routes-troubleshoot-powershell.md) cikk toolearn hogyan toodiagnose útvonal is lehet akadályozó problémákat hello és bezárja a forgalom hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="9c60e-168">Read hello [Troubleshooting Routes](virtual-network-routes-troubleshoot-powershell.md) article toolearn how toodiagnose route problems that may be impeding hello flow of traffic in and out of hello VM.</span></span> 
* <span data-ttu-id="9c60e-169">Vnetek, alapértelmezés szerint rendelkezik társviszonyban, ha a hello VIRTUAL_NETWORK címke automatikusan tooinclude előtagok bontsa a társviszonyban Vnetek esetében.</span><span class="sxs-lookup"><span data-stu-id="9c60e-169">If you have peered VNets, by default, hello VIRTUAL_NETWORK tag will automatically expand tooinclude prefixes for peered VNets.</span></span> <span data-ttu-id="9c60e-170">Ezeket az előtagokat megtekintheti a hello **ExpandedAddressPrefix** listájában, tootroubleshoot bármely társviszony-létesítés kapcsolódási problémák kapcsolódó tooVNet.</span><span class="sxs-lookup"><span data-stu-id="9c60e-170">You can view these prefixes in hello **ExpandedAddressPrefix** list, tootroubleshoot any issues related tooVNet peering connectivity.</span></span> 
* <span data-ttu-id="9c60e-171">Hatékony biztonsági szabályok csak jelennek-e hello VM hálózati adapter és, vagy alhálózat társított egy NSG.</span><span class="sxs-lookup"><span data-stu-id="9c60e-171">Effective security rules are only shown if there is an NSG associated with hello VM’s NIC and or subnet.</span></span> 
* <span data-ttu-id="9c60e-172">Ha nincs hálózati hello társított NSG-ket, vagy alhálózat és rendelkezik a nyilvános IP-cím hozzárendelése a virtuális gép tooyour, minden port nyissa meg a bejövő és kimenő hozzáférést lesz.</span><span class="sxs-lookup"><span data-stu-id="9c60e-172">If there are no NSGs associated with hello NIC or subnet and you have a public IP address assigned tooyour VM, all ports will be open for inbound and outbound access.</span></span> <span data-ttu-id="9c60e-173">Ha hello VM egy nyilvános IP-címmel rendelkezik, alkalmazza az NSG-k toohello hálózati adapter vagy az alhálózat erősen ajánlott.</span><span class="sxs-lookup"><span data-stu-id="9c60e-173">If hello VM has a public IP address, applying NSGs toohello NIC or subnet is strongly recommended.</span></span>  

