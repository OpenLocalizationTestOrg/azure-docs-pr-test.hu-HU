---
title: "hibrid felhő tesztkörnyezet aaaSimulated |} Microsoft Docs"
description: "Hozzon létre egy szimulált hibrid felhőalapú környezetet informatikai pro vagy fejlesztői tesztelni, két Azure virtuális hálózat és VNet – VNet-kapcsolatot."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ca5bf294-8172-44a9-8fed-d6f38d345364
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.openlocfilehash: 6063022e373c2b3564e040c4fbee122e2438974b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a><span data-ttu-id="5f657-103">Szimulált hibridfelhő-környezet kialakítása teszteléshez</span><span class="sxs-lookup"><span data-stu-id="5f657-103">Set up a simulated hybrid cloud environment for testing</span></span>
<span data-ttu-id="5f657-104">Ez a cikk végigvezeti egy szimulált hibrid felhőkörnyezet létrehozása a Microsoft Azure-ban két Azure virtuális hálózatot használ.</span><span class="sxs-lookup"><span data-stu-id="5f657-104">This article steps you through creating a simulated hybrid cloud environment with Microsoft Azure using two Azure virtual networks.</span></span> <span data-ttu-id="5f657-105">Itt található hello létrejövő konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="5f657-105">Here is hello resulting configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

<span data-ttu-id="5f657-106">Hibrid felhő éles környezetben szimulálja, és a következő tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5f657-106">This simulates a hybrid cloud production environment and consists of:</span></span>

* <span data-ttu-id="5f657-107">A szimulált és egyszerűsített helyszíni hálózatot tárolva az Azure virtuális hálózat (hello TestLab virtuális hálózat).</span><span class="sxs-lookup"><span data-stu-id="5f657-107">A simulated and simplified on-premises network hosted in an Azure virtual network (hello TestLab virtual network).</span></span>
* <span data-ttu-id="5f657-108">(TestVNET) Azure-ban üzemeltetett szimulált létesítmények közötti virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="5f657-108">A simulated cross-premises virtual network hosted in Azure (TestVNET).</span></span>
* <span data-ttu-id="5f657-109">VNet – VNet hello két virtuális hálózatok közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="5f657-109">A VNet-to-VNet connection between hello two virtual networks.</span></span>
* <span data-ttu-id="5f657-110">A másodlagos tartományvezérlő hello TestVNET virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="5f657-110">A secondary domain controller in hello TestVNET virtual network.</span></span>

<span data-ttu-id="5f657-111">Ez biztosít, amelyben pont alapja és közös indítása:</span><span class="sxs-lookup"><span data-stu-id="5f657-111">This provides a basis and common starting point from which you can:</span></span>

* <span data-ttu-id="5f657-112">Alkalmazások fejlesztéséhez és teszteléséhez a szimulált hibrid felhőkörnyezetekben.</span><span class="sxs-lookup"><span data-stu-id="5f657-112">Develop and test applications in a simulated hybrid cloud environment.</span></span>
* <span data-ttu-id="5f657-113">Vizsgálati beállítások számítógépek, néhány hello TestLab virtuális hálózaton belül, és néhány toosimulate hibrid felhőalapú informatikai munkaterhelések hello TestVNET virtuális hálózaton belül létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5f657-113">Create test configurations of computers, some within hello TestLab virtual network and some within hello TestVNET virtual network, toosimulate hybrid cloud-based IT workloads.</span></span>

<span data-ttu-id="5f657-114">A hibrid felhő tesztkörnyezet négy fő fázisokat toosetting van:</span><span class="sxs-lookup"><span data-stu-id="5f657-114">There are four major phases toosetting up this hybrid cloud test environment:</span></span>

1. <span data-ttu-id="5f657-115">Hello TestLab virtuális hálózat konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="5f657-115">Configure hello TestLab virtual network.</span></span>
2. <span data-ttu-id="5f657-116">Hello létesítmények közötti virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5f657-116">Create hello cross-premises virtual network.</span></span>
3. <span data-ttu-id="5f657-117">Hello VNet – VNet VPN-kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5f657-117">Create hello VNet-to-VNet VPN connection.</span></span>
4. <span data-ttu-id="5f657-118">A DC2 konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="5f657-118">Configure DC2.</span></span> 

<span data-ttu-id="5f657-119">Ez a konfiguráció Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="5f657-119">This configuration requires an Azure subscription.</span></span> <span data-ttu-id="5f657-120">Ha MSDN vagy a Visual Studio-előfizetéssel rendelkezik, tekintse meg [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="5f657-120">If you have an MSDN or Visual Studio subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

> [!NOTE]
> <span data-ttu-id="5f657-121">Virtuális gépek és virtuális hálózati átjárók az Azure-ban fel Önnek egy folyamatban lévő a költség, amikor futnak.</span><span class="sxs-lookup"><span data-stu-id="5f657-121">Virtual machines and virtual network gateways in Azure incur an ongoing monetary cost when they are running.</span></span> <span data-ttu-id="5f657-122">Az ehhez kapcsolódó költséget szemben az MSDN Számlázva vagy -előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="5f657-122">This cost is billed against your MSDN or paid subscription.</span></span> <span data-ttu-id="5f657-123">Az Azure VPN gateway, két Azure virtuális gépek halmaza lett megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="5f657-123">An Azure VPN gateway is implemented as a set of two Azure virtual machines.</span></span> <span data-ttu-id="5f657-124">toominimize hello költségek, hello tesztelési környezet létrehozása, és végezze el a szükséges tesztelés és bemutató lehető leggyorsabban tegye.</span><span class="sxs-lookup"><span data-stu-id="5f657-124">toominimize hello costs, create hello test environment and perform your needed testing and demonstration as quickly as possible.</span></span>
> 
> 

## <a name="phase-1-configure-hello-testlab-virtual-network"></a><span data-ttu-id="5f657-125">1. fázis: Hello TestLab virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5f657-125">Phase 1: Configure hello TestLab virtual network</span></span>
<span data-ttu-id="5f657-126">Hello utasítások használata hello [alapkonfiguráció tesztkörnyezetben](https://technet.microsoft.com/library/mt771177.aspx) témakör tooconfigure hello DC1, APP1 és CLIENT1 számítógépek hello nevű TestLab Azure virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="5f657-126">Use hello instructions in hello [Base Configuration test environment](https://technet.microsoft.com/library/mt771177.aspx) topic tooconfigure hello DC1, APP1, and CLIENT1 computers in hello Azure virtual network named TestLab.</span></span> 

<span data-ttu-id="5f657-127">Ezután indítsa el az Azure PowerShell-promptot.</span><span class="sxs-lookup"><span data-stu-id="5f657-127">Next, start an Azure PowerShell prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="5f657-128">hello a következő parancs úgy állítja be az Azure PowerShell 1.0-ás vagy újabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="5f657-128">hello following command sets use Azure PowerShell 1.0 and later.</span></span>
> 
> 

<span data-ttu-id="5f657-129">Jelentkezzen be tooyour fiókjával.</span><span class="sxs-lookup"><span data-stu-id="5f657-129">Sign in tooyour account.</span></span>

    Login-AzureRMAccount

<span data-ttu-id="5f657-130">Az előfizetés nevét, a következő parancs hello segítségével beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5f657-130">Get your subscription name using hello following command.</span></span>

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

<span data-ttu-id="5f657-131">Állítsa be az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="5f657-131">Set your Azure subscription.</span></span> <span data-ttu-id="5f657-132">Használja az azonos hello toobuild 1. szakasza az alapkonfiguráció használt előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5f657-132">Use hello same subscription that you used toobuild your base configuration in Phase 1.</span></span> <span data-ttu-id="5f657-133">Cserélje le mindent, ami hello idézőjelek között, beleértve a hello < és > karakter, helyes hello nevét.</span><span class="sxs-lookup"><span data-stu-id="5f657-133">Replace everything within hello quotes, including hello < and > characters, with hello correct name.</span></span>

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

<span data-ttu-id="5f657-134">Ezután adja hozzá egy átjáró alhálózati toohello TestLab virtuális hálózat a alapkonfiguráció lépéseit, amely használt toohost hello Azure átjáró lesz.</span><span class="sxs-lookup"><span data-stu-id="5f657-134">Next, add a gateway subnet toohello TestLab virtual network of your base configuration, which will be used toohost hello Azure gateway.</span></span>

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed hello TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

<span data-ttu-id="5f657-135">A következő kérelmet egy nyilvános IP-cím tooassign toohello átjáró hello TestLab virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="5f657-135">Next, request a public IP address tooassign toohello gateway for hello TestLab virtual network.</span></span>

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

<span data-ttu-id="5f657-136">Ezután hozzon létre az átjárót.</span><span class="sxs-lookup"><span data-stu-id="5f657-136">Next, create your gateway.</span></span>

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

<span data-ttu-id="5f657-137">Ne feledje, hogy új átjárók is igénybe vehet 20 perc vagy több toocreate.</span><span class="sxs-lookup"><span data-stu-id="5f657-137">Keep in mind that new gateways can take 20 minutes or more toocreate.</span></span>

<span data-ttu-id="5f657-138">Hello Azure-portál a helyi számítógépen, a csatlakozás tooDC1 hello corp\felhasználó1 hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="5f657-138">From hello Azure portal on your local computer, connect tooDC1 with hello CORP\User1 credentials.</span></span> <span data-ttu-id="5f657-139">tooconfigure hello CORP tartományhoz, hogy a felhasználók és számítógépek használja a hitelesítéshez, a helyi tartományvezérlő futtassa ezeket a parancsokat egy rendszergazda szintű Windows PowerShell parancssorból a DC1-en.</span><span class="sxs-lookup"><span data-stu-id="5f657-139">tooconfigure hello CORP domain so that computers and users use their local domain controller for authentication, run these commands from an administrator-level Windows PowerShell command prompt on DC1.</span></span>

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

<span data-ttu-id="5f657-140">Ez az az aktuális konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="5f657-140">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)

## <a name="phase-2-create-hello-testvnet-virtual-network"></a><span data-ttu-id="5f657-141">2. fázis: Hello TestVNET virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f657-141">Phase 2: Create hello TestVNET virtual network</span></span>
<span data-ttu-id="5f657-142">Először hello TestVNET virtuális hálózat létrehozása, és a hálózati biztonsági csoport védelmét.</span><span class="sxs-lookup"><span data-stu-id="5f657-142">First, create hello TestVNET virtual network and protect it with a network security group.</span></span>

    $rgName="<name of hello resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed hello TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP tooall VMs on hello subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

<span data-ttu-id="5f657-143">A következő kérelmet egy nyilvános IP-cím toobe toohello átjáró hello TestVNET virtuális hálózat számára lefoglalt, és létrehozhatja az átjárót.</span><span class="sxs-lookup"><span data-stu-id="5f657-143">Next, request a public IP address toobe allocated toohello gateway for hello TestVNET virtual network and create your gateway.</span></span>

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

<span data-ttu-id="5f657-144">Ez az az aktuális konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="5f657-144">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)

## <a name="phase-3-create-hello-vnet-to-vnet-connection"></a><span data-ttu-id="5f657-145">3. fázis: Hello VNet – VNet-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f657-145">Phase 3: Create hello VNet-to-VNet connection</span></span>
<span data-ttu-id="5f657-146">Először szerezze be a véletlenszerű, erős titkosítással, 32 karakterből álló előmegosztott kulccsal, a hálózati vagy biztonsági rendszergazdától.</span><span class="sxs-lookup"><span data-stu-id="5f657-146">First, obtain a random, cryptographically strong, 32-character pre-shared key from your network or security administrator.</span></span> <span data-ttu-id="5f657-147">Alternatív megoldásként használjon hello információt [hozzon létre egy véletlenszerű karakterlánc egy IPsec előmegosztott kulcs](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) tooobtain előmegosztott kulcs.</span><span class="sxs-lookup"><span data-stu-id="5f657-147">Alternately, use hello information at [Create a random string for an IPsec preshared key](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) tooobtain a pre-shared key.</span></span>

<span data-ttu-id="5f657-148">A következő ezen parancsok toocreate hello VNet – VNet VPN-kapcsolat használata, ami eltarthat némi időt toocomplete.</span><span class="sxs-lookup"><span data-stu-id="5f657-148">Next, use these commands toocreate hello VNet-to-VNet VPN connection, which can take some time toocomplete.</span></span>

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

<span data-ttu-id="5f657-149">Néhány perc elteltével hello kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5f657-149">After a few minutes, hello connection should be established.</span></span>

<span data-ttu-id="5f657-150">Ez az az aktuális konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="5f657-150">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)

## <a name="phase-4-configure-dc2"></a><span data-ttu-id="5f657-151">4. fázis: A DC2 konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5f657-151">Phase 4: Configure DC2</span></span>
<span data-ttu-id="5f657-152">Először hozzon létre egy virtuális gép dc2 tartományvezérlő számára.</span><span class="sxs-lookup"><span data-stu-id="5f657-152">First, create a virtual machine for DC2.</span></span> <span data-ttu-id="5f657-153">A parancsok hello Azure PowerShell parancssorába a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5f657-153">Run these commands at hello Azure PowerShell command prompt on your local computer.</span></span>

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<hello storage account name for hello base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="5f657-154">Ezután csatlakoztassa toohello új DC2 virtuális gép hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="5f657-154">Next, connect toohello new DC2 virtual machine from hello Azure portal.</span></span>

<span data-ttu-id="5f657-155">Ezután konfigurálja a hálózati kapcsolat tesztelése a Windows tűzfal szabály tooallow forgalom.</span><span class="sxs-lookup"><span data-stu-id="5f657-155">Next, configure a Windows Firewall rule tooallow traffic for basic connectivity testing.</span></span> <span data-ttu-id="5f657-156">Egy rendszergazda szintű Windows PowerShell parancssorból a dc2 számítógépen futtassa az alábbi parancsokat.</span><span class="sxs-lookup"><span data-stu-id="5f657-156">From an administrator-level Windows PowerShell command prompt on DC2, run these commands.</span></span>

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

<span data-ttu-id="5f657-157">hello ping parancs négy sikeres választ a következő IP-10.0.0.4 eredményez.</span><span class="sxs-lookup"><span data-stu-id="5f657-157">hello ping command should result in four successful replies from IP address 10.0.0.4.</span></span> <span data-ttu-id="5f657-158">Ez egy tesztművelet forgalom hello VNet – VNet-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="5f657-158">This is a test of traffic across hello VNet-to-VNet connection.</span></span>

<span data-ttu-id="5f657-159">Ezután adja hozzá a hello további adatlemezt a dc2 számítógépen hello meghajtóbetűjellel F: új kötetként.</span><span class="sxs-lookup"><span data-stu-id="5f657-159">Next, add hello extra data disk on DC2 as a new volume with hello drive letter F:.</span></span>

1. <span data-ttu-id="5f657-160">Hello bal oldali ablaktáblán Kiszolgálókezelő kattintson **fájl- és tárolási szolgáltatások**, és kattintson a **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="5f657-160">In hello left pane of Server Manager, click **File and Storage Services**, and then click **Disks**.</span></span>
2. <span data-ttu-id="5f657-161">Hello Tartalom ablaktáblán, a hello **lemezek** csoportjában kattintson a **2 lemez** (a hello **partíció** túl beállítása**ismeretlen**).</span><span class="sxs-lookup"><span data-stu-id="5f657-161">In hello contents pane, in hello **Disks** group, click **disk 2** (with hello **Partition** set too**Unknown**).</span></span>
3. <span data-ttu-id="5f657-162">Kattintson a **feladatok**, és kattintson a **új kötet**.</span><span class="sxs-lookup"><span data-stu-id="5f657-162">Click **Tasks**, and then click **New Volume**.</span></span>
4. <span data-ttu-id="5f657-163">A hello hello új kötet varázsló oldalán elkezdéséhez kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5f657-163">On hello Before you begin page of hello New Volume Wizard, click **Next**.</span></span>
5. <span data-ttu-id="5f657-164">Hello válassza hello kiszolgáló és lemez oldal, kattintson a **Disk 2**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="5f657-164">On hello Select hello server and disk page, click **Disk 2**, and then click **Next**.</span></span> <span data-ttu-id="5f657-165">Amikor a rendszer kéri, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f657-165">When prompted, click **OK**.</span></span>
6. <span data-ttu-id="5f657-166">A hello hello méret megadása hello kötet lap, kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5f657-166">On hello Specify hello size of hello volume page, click **Next**.</span></span>
7. <span data-ttu-id="5f657-167">Oldalon hello hozzárendelése tooa meghajtó betűjele vagy a mappát, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="5f657-167">On hello Assign tooa drive letter or folder page, click **Next**.</span></span>
8. <span data-ttu-id="5f657-168">Hello fájl kijelölése rendszer beállítások lapon kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5f657-168">On hello Select file system settings page, click **Next**.</span></span>
9. <span data-ttu-id="5f657-169">Hello megerősítése összetevők megerősítése oldalon kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5f657-169">On hello Confirm selections page, click **Create**.</span></span>
10. <span data-ttu-id="5f657-170">Amikor végzett, kattintson **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="5f657-170">When complete, click **Close**.</span></span>

<span data-ttu-id="5f657-171">Ezután konfigurálja a DC2 hello corp.contoso.com tartományhoz tartozó replika tartományvezérlőként működik.</span><span class="sxs-lookup"><span data-stu-id="5f657-171">Next, configure DC2 as a replica domain controller for hello corp.contoso.com domain.</span></span> <span data-ttu-id="5f657-172">A parancsok Windows PowerShell-parancssorból hello a dc2 számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5f657-172">Run these commands from hello Windows PowerShell command prompt on DC2.</span></span>

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

<span data-ttu-id="5f657-173">Vegye figyelembe, hogy-e rákérdezéses toosupply mindkét corp\felhasználó1 jelszó és a Directory címtárszolgáltatások helyreállító módjában (DSRM) jelszavát, és a toorestart DC2 hello.</span><span class="sxs-lookup"><span data-stu-id="5f657-173">Note that you are prompted toosupply both hello CORP\User1 password and a Directory Services Restore Mode (DSRM) password, and toorestart DC2.</span></span>

<span data-ttu-id="5f657-174">Most, hogy hello TestVNET virtuális hálózatnak a saját DNS-kiszolgáló (DC2), a DNS-kiszolgáló hello TestVNET virtuális hálózati toouse kell konfigurálnia.</span><span class="sxs-lookup"><span data-stu-id="5f657-174">Now that hello TestVNET virtual network has its own DNS server (DC2), you must configure hello TestVNET virtual network toouse this DNS server.</span></span>

1. <span data-ttu-id="5f657-175">Hello hello Azure-portálon a bal oldali ablaktáblán, kattintson hello virtuális hálózatok ikonra, majd **TestVNET**.</span><span class="sxs-lookup"><span data-stu-id="5f657-175">In hello left pane of hello Azure portal, click hello virtual networks icon, and then click **TestVNET**.</span></span>
2. <span data-ttu-id="5f657-176">A hello **beállítások** lapra, majd **DNS-kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="5f657-176">On hello **Settings** tab, click **DNS servers**.</span></span>
3. <span data-ttu-id="5f657-177">A **elsődleges DNS-kiszolgáló**, típus **192.168.0.4** tooreplace 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="5f657-177">In **Primary DNS server**, type **192.168.0.4** tooreplace 10.0.0.4.</span></span>
4. <span data-ttu-id="5f657-178">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="5f657-178">Click **Save**.</span></span>

<span data-ttu-id="5f657-179">Ez az az aktuális konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="5f657-179">This is your current configuration.</span></span> 

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

<span data-ttu-id="5f657-180">A szimulált hibrid felhőkörnyezet most már készen áll a tesztelésre.</span><span class="sxs-lookup"><span data-stu-id="5f657-180">Your simulated hybrid cloud environment is now ready for testing.</span></span>

## <a name="next-step"></a><span data-ttu-id="5f657-181">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="5f657-181">Next step</span></span>
* <span data-ttu-id="5f657-182">Állítson be egy [web-alapú, üzletági alkalmazás](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ebben a környezetben.</span><span class="sxs-lookup"><span data-stu-id="5f657-182">Set up a [web-based, line of business application](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in this environment.</span></span>

