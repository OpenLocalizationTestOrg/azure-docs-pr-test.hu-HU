---
title: "aaaMultiple IP-címek az Azure virtuális gépek - PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan tooassign több IP-címek tooa virtuális gépet a PowerShell használatával |} Erőforrás-kezelő."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c44ea62f-7e54-4e3b-81ef-0b132111f1f8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/24/2017
ms.author: jdial;annahar
ms.openlocfilehash: df54c4386ce13521e660a3e7208c8c1ab1459bc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-powershell"></a><span data-ttu-id="1263f-103">Több IP-cím hozzárendelése toovirtual gépek PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="1263f-103">Assign multiple IP addresses toovirtual machines using PowerShell</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

<span data-ttu-id="1263f-104">Ez a cikk azt ismerteti, hogyan toocreate egy virtuális gép (VM) keresztül hello Azure Resource Manager telepítési modell PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="1263f-104">This article explains how toocreate a virtual machine (VM) through hello Azure Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="1263f-105">Több IP-cím hello klasszikus telepítési modell használatával létrehozott tooresources nem lehet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="1263f-105">Multiple IP addresses cannot be assigned tooresources created through hello classic deployment model.</span></span> <span data-ttu-id="1263f-106">További információk az Azure üzembe helyezési modellel, olvassa el a hello toolearn [üzembe helyezési modellel megértéséhez](../resource-manager-deployment-model.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="1263f-106">toolearn more about Azure deployment models, read hello [Understand deployment models](../resource-manager-deployment-model.md) article.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <span data-ttu-id="1263f-107"><a name = "create"></a>Több IP-címekkel rendelkező virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1263f-107"><a name = "create"></a>Create a VM with multiple IP addresses</span></span>

<span data-ttu-id="1263f-108">hello következő lépések azt ismertetik, hogyan toocreate például VM több IP-címek, hello forgatókönyvben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="1263f-108">hello steps that follow explain how toocreate an example VM with multiple IP addresses, as described in hello scenario.</span></span> <span data-ttu-id="1263f-109">A végrehajtásához szükséges változók értékeinek módosítása.</span><span class="sxs-lookup"><span data-stu-id="1263f-109">Change variable values as required for your implementation.</span></span>

1. <span data-ttu-id="1263f-110">Nyisson meg egy PowerShell-parancssort, és teljes hello hátralévő lépéseket ebben a szakaszban egy PowerShell-munkameneten belül.</span><span class="sxs-lookup"><span data-stu-id="1263f-110">Open a PowerShell command prompt and complete hello remaining steps in this section within a single PowerShell session.</span></span> <span data-ttu-id="1263f-111">Ha még nem rendelkezik a PowerShell telepítése és konfigurálása, teljes hello hello szükséges lépések [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk.</span><span class="sxs-lookup"><span data-stu-id="1263f-111">If you don't already have PowerShell installed and configured, complete hello steps in hello [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="1263f-112">Bejelentkezési tooyour fiókot hello `login-azurermaccount` parancsot.</span><span class="sxs-lookup"><span data-stu-id="1263f-112">Login tooyour account with hello `login-azurermaccount` command.</span></span>
3. <span data-ttu-id="1263f-113">Cserélje le *myResourceGroup* és *westus* a nevet és a helyet.</span><span class="sxs-lookup"><span data-stu-id="1263f-113">Replace *myResourceGroup* and *westus* with a name and location of your choosing.</span></span> <span data-ttu-id="1263f-114">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="1263f-114">Create a resource group.</span></span> <span data-ttu-id="1263f-115">Az erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="1263f-115">A resource group is a logical container into which Azure resources are deployed and managed.</span></span>

    ```powershell
    $RgName   = "MyResourceGroup"
    $Location = "westus"

    New-AzureRmResourceGroup `
    -Name $RgName `
    -Location $Location
    ```

4. <span data-ttu-id="1263f-116">Hozzon létre egy virtuális hálózathoz (VNet) és az alhálózati hello hello erőforráscsoport és ugyanazon a helyen:</span><span class="sxs-lookup"><span data-stu-id="1263f-116">Create a virtual network (VNet) and subnet in hello same location as hello resource group:</span></span>

    ```powershell
    
    # Create a subnet configuration
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name MySubnet `
    -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $VNet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyVNet `
    -AddressPrefix 10.0.0.0/16 `
    -Subnet $subnetConfig

    # Get hello subnet object
    $Subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
    ```

5. <span data-ttu-id="1263f-117">A hálózati biztonsági csoport (NSG) és a szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1263f-117">Create a network security group (NSG) and a rule.</span></span> <span data-ttu-id="1263f-118">hello NSG biztosítja a virtuális gép hello bejövő és kimenő szabályok használatával.</span><span class="sxs-lookup"><span data-stu-id="1263f-118">hello NSG secures hello VM using inbound and outbound rules.</span></span> <span data-ttu-id="1263f-119">Ebben az esetben létrejön egy bejövő szabály a 3389-es porthoz, amely lehetővé teszi a bejövő távoli asztali kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="1263f-119">In this case, an inbound rule is created for port 3389, which allows incoming remote desktop connections.</span></span>

    ```powershell
    
    # Create an inbound network security group rule for port 3389

    $NSGRule = New-AzureRmNetworkSecurityRuleConfig `
    -Name MyNsgRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow
    
    # Create a network security group
    $NSG = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyNetworkSecurityGroup `
    -SecurityRules $NSGRule
    ```

6. <span data-ttu-id="1263f-120">Adja meg az elsődleges IP-konfiguráció hello hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="1263f-120">Define hello primary IP configuration for hello NIC.</span></span> <span data-ttu-id="1263f-121">Változás 10.0.0.4 tooa érvényes cím hello hozott létre, ha korábban definiált hello érték nem az alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="1263f-121">Change 10.0.0.4 tooa valid address in hello subnet you created, if you didn't use hello value defined previously.</span></span> <span data-ttu-id="1263f-122">Egy statikus IP-cím hozzárendelése, előtt ajánlott, hogy először megerősíti nem már használatban van.</span><span class="sxs-lookup"><span data-stu-id="1263f-122">Before assigning a static IP address, it's recommended that you first confirm it's not already in use.</span></span> <span data-ttu-id="1263f-123">Adja meg a hello parancs `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`.</span><span class="sxs-lookup"><span data-stu-id="1263f-123">Enter hello command `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`.</span></span> <span data-ttu-id="1263f-124">Ha hello címkészletében nem áll rendelkezésre, a hello kimeneti értéket ad vissza *igaz*.</span><span class="sxs-lookup"><span data-stu-id="1263f-124">If hello address is available, hello output returns *True*.</span></span> <span data-ttu-id="1263f-125">Ha nem érhető el, a hello kimeneti értéket ad vissza *hamis* és a rendelkezésre álló címek listáját.</span><span class="sxs-lookup"><span data-stu-id="1263f-125">If it's not available, hello output returns *False* and a list of addresses that are available.</span></span> 

    <span data-ttu-id="1263f-126">A következő parancsokat, hello **hello egyedi DNS-név toouse cserélje le < csere-az-a-egyedi-neve >.**</span><span class="sxs-lookup"><span data-stu-id="1263f-126">In hello following commands, **Replace <replace-with-your-unique-name> with hello unique DNS name toouse.**</span></span> <span data-ttu-id="1263f-127">hello nevének egyedinek kell lennie a kívánt Azure-régiót belül minden nyilvános IP-címek között.</span><span class="sxs-lookup"><span data-stu-id="1263f-127">hello name must be unique across all public IP addresses within an Azure region.</span></span> <span data-ttu-id="1263f-128">Ez nem kötelező paraméter.</span><span class="sxs-lookup"><span data-stu-id="1263f-128">This is an optional parameter.</span></span> <span data-ttu-id="1263f-129">El kell távolítani, ha kizárólag tooconnect toohello VM hello nyilvános IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="1263f-129">It can be removed if you only want tooconnect toohello VM using hello public IP address.</span></span>

    ```powershell
    
    # Create a public IP address
    $PublicIP1 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP1" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -DomainNameLabel <replace-with-your-unique-name> `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign hello public IP ddress tooit
    $IpConfigName1 = "IPConfig-1"
    $IpConfig1     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName1 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.4 `
    -PublicIpAddress $PublicIP1 `
    -Primary
    ```

    <span data-ttu-id="1263f-130">Ha több IP-konfigurációk tooa hálózati adapter, egy konfigurációs hozzá kell rendelni, hello *-elsődleges*.</span><span class="sxs-lookup"><span data-stu-id="1263f-130">When you assign multiple IP configurations tooa NIC, one configuration must be assigned as hello *-Primary*.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1263f-131">Nyilvános IP-címek névleges díjat kell.</span><span class="sxs-lookup"><span data-stu-id="1263f-131">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="1263f-132">tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap.</span><span class="sxs-lookup"><span data-stu-id="1263f-132">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="1263f-133">Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello.</span><span class="sxs-lookup"><span data-stu-id="1263f-133">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="1263f-134">További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.</span><span class="sxs-lookup"><span data-stu-id="1263f-134">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

7. <span data-ttu-id="1263f-135">Adja meg a másodlagos IP-konfigurációk hello hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="1263f-135">Define hello secondary IP configurations for hello NIC.</span></span> <span data-ttu-id="1263f-136">Adja hozzá, vagy távolítsa el a szükséges konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="1263f-136">You can add or remove configurations as necessary.</span></span> <span data-ttu-id="1263f-137">Minden IP-konfiguráció egy privát IP-cím kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1263f-137">Each IP configuration must have a private IP address assigned.</span></span> <span data-ttu-id="1263f-138">Minden egyes konfigurációs szükség lehet egy nyilvános IP-cím hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="1263f-138">Each configuration can optionally have one public IP address assigned.</span></span>

    ```powershell
    
    # Create a public IP address
    $PublicIP2 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP2" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign hello public IP ddress tooit
    $IpConfigName2 = "IPConfig-2"
    $IpConfig2     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName2 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.5 `
    -PublicIpAddress $PublicIP2
        
    $IpConfigName3 = "IpConfig-3"
    $IpConfig3 = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IPConfigName3 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.6
    ```

8. <span data-ttu-id="1263f-139">A hálózati adapter hello létrehoz, hello három IP-konfigurációk tooit:</span><span class="sxs-lookup"><span data-stu-id="1263f-139">Create hello NIC and associate hello three IP configurations tooit:</span></span>

    ```powershell
    
    $NIC = New-AzureRmNetworkInterface `
    -Name MyNIC `
    -ResourceGroupName $RgName `
    -Location $Location `
    -NetworkSecurityGroupId $NSG.Id `
    -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
    ```

    >[!NOTE]
    ><span data-ttu-id="1263f-140">Bár az összes konfiguráció hozzárendelt hálózati adapter tooone ebben a cikkben, rendelhet hozzá több IP konfigurációk tooevery csatlakoztatott hálózati toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="1263f-140">Though all configurations are assigned tooone NIC in this article, you can assign multiple IP configurations tooevery NIC attached toohello VM.</span></span> <span data-ttu-id="1263f-141">hogyan olvassa el a virtuális gép több hálózati adaptert, és toocreate toolearn hello [több hálózati adapterrel rendelkező virtuális gép létrehozása](virtual-network-deploy-multinic-arm-ps.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="1263f-141">toolearn how toocreate a VM with multiple NICs, read hello [Create a VM with multiple NICs](virtual-network-deploy-multinic-arm-ps.md) article.</span></span>

9. <span data-ttu-id="1263f-142">Hozza létre a virtuális gép hello hello a következő parancsok beírásával:</span><span class="sxs-lookup"><span data-stu-id="1263f-142">Create hello VM by entering hello following commands:</span></span>

    ```powershell
    
    # Define a credential object. When you run these commands, you're prompted tooenter a sername and password for hello VM you're reating.
    $cred = Get-Credential
    
    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
    -VMName MyVM `
    -VMSize Standard_DS1_v2 | `
    Set-AzureRmVMOperatingSystem -Windows `
    -ComputerName MyVM `
    -Credential $cred | `
    Set-AzureRmVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
    Add-AzureRmVMNetworkInterface `
    -Id $NIC.Id
    
    # Create hello VM
    New-AzureRmVM `
    -ResourceGroupName $RgName `
    -Location $Location `
    -VM $VmConfig
    ```

10. <span data-ttu-id="1263f-143">Hozzáadás hello privát IP-címek toohello virtuális gép operációs rendszer hello lépések végrehajtásával, az operációs rendszernek a hello [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="1263f-143">Add hello private IP addresses toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="1263f-144">Hello nyilvános IP-címek toohello operációs rendszere nem adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="1263f-144">Do not add hello public IP addresses toohello operating system.</span></span>

## <span data-ttu-id="1263f-145"><a name="add"></a>Adja hozzá az IP-címek tooa méretű VM</span><span class="sxs-lookup"><span data-stu-id="1263f-145"><a name="add"></a>Add IP addresses tooa VM</span></span>

<span data-ttu-id="1263f-146">Magán- és IP címeket tooa NIC hello következő lépések végrehajtásával adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="1263f-146">You can add private and public IP addresses tooa NIC by completing hello steps that follow.</span></span> <span data-ttu-id="1263f-147">hello hello a következő szakaszokban szereplő példák azt feltételezik, hogy már van egy virtuális gép hello három IP-konfigurációk hello ismertetett [forgatókönyv](#Scenario) ezen cikk, de nem szükséges, hogy végezzen.</span><span class="sxs-lookup"><span data-stu-id="1263f-147">hello examples in hello following sections assume that you already have a VM with hello three IP configurations described in hello [scenario](#Scenario) in this article, but it's not required that you do.</span></span>

1. <span data-ttu-id="1263f-148">Nyisson meg egy PowerShell-parancssort, és teljes hello hátralévő lépéseket ebben a szakaszban egy PowerShell-munkameneten belül.</span><span class="sxs-lookup"><span data-stu-id="1263f-148">Open a PowerShell command prompt and complete hello remaining steps in this section within a single PowerShell session.</span></span> <span data-ttu-id="1263f-149">Ha még nem rendelkezik a PowerShell telepítése és konfigurálása, teljes hello hello szükséges lépések [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk.</span><span class="sxs-lookup"><span data-stu-id="1263f-149">If you don't already have PowerShell installed and configured, complete hello steps in hello [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="1263f-150">A következő $Variables toohello neve hello tooadd IP cím tooand hello erőforrás csoport és hely hello hálózati adapter létezik a kívánt hálózati hello "értékek" hello módosítása:</span><span class="sxs-lookup"><span data-stu-id="1263f-150">Change hello "values" of hello following $Variables toohello name of hello NIC you want tooadd IP address tooand hello resource group and location hello NIC exists in:</span></span>

    ```powershell
    $NicName  = "MyNIC"
    $RgName   = "MyResourceGroup"
    $Location = "westus"
    ```

    <span data-ttu-id="1263f-151">Ha nem tudja hello hello toochange kívánt hálózati adapter nevét, adja meg a következő parancsok hello, majd módosítsa a hello hello előző változók értékeit:</span><span class="sxs-lookup"><span data-stu-id="1263f-151">If you don't know hello name of hello NIC you want toochange, enter hello following commands, then change hello values of hello previous variables:</span></span>

    ```powershell
    Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location
    ```
3. <span data-ttu-id="1263f-152">Hozzon létre egy változót, és állítsa be úgy a hálózati adapter meglévő hello a következő parancs beírásával toohello:</span><span class="sxs-lookup"><span data-stu-id="1263f-152">Create a variable and set it toohello existing NIC by typing hello following command:</span></span>

    ```powershell
    $MyNIC = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName
    ```
4. <span data-ttu-id="1263f-153">A következő parancsok hello, módosítsa *MyVNet* és *MySubnet* toohello nevei hello VNet és alhálózat hello hálózati adapter csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="1263f-153">In hello following commands, change *MyVNet* and *MySubnet* toohello names of hello VNet and subnet hello NIC is connected to.</span></span> <span data-ttu-id="1263f-154">Adja meg a hello parancsok tooretrieve hello VNet és alhálózat objektumok hello hálózati adapter csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="1263f-154">Enter hello commands tooretrieve hello VNet and subnet objects hello NIC is connected to:</span></span>

    ```powershell
    $MyVNet = Get-AzureRMVirtualnetwork -Name MyVNet -ResourceGroupName $RgName
    $Subnet = $MyVnet.Subnets | Where-Object { $_.Name -eq "MySubnet" }
    ```
    <span data-ttu-id="1263f-155">Ha nem tudja hello virtuális hálózat vagy az alhálózat neve hello hálózati adapter csatlakoztatva van, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="1263f-155">If you don't know hello VNet or subnet name hello NIC is connected to, enter hello following command:</span></span>
    ```powershell
    $MyNIC.IpConfigurations
    ```
    <span data-ttu-id="1263f-156">A hello kimeneti keresse meg a következő egy példa a kimenetre szöveg hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="1263f-156">In hello output, look for text similar toohello following example output:</span></span>
    
    ```
    "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet/subnets/MySubnet"
    ```
    <span data-ttu-id="1263f-157">A kimenet *MyVnet* hello VNet és *MySubnet* hello alhálózati hello hálózati adapter van csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="1263f-157">In this output, *MyVnet* is hello VNet and *MySubnet* is hello subnet hello NIC is connected to.</span></span>

5. <span data-ttu-id="1263f-158">Végezze el a következő szakaszok a követelmények alapján hello egyikében hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1263f-158">Complete hello steps in one of hello following sections, based on your requirements:</span></span>

    <span data-ttu-id="1263f-159">**Adja hozzá a magánhálózati IP-cím**</span><span class="sxs-lookup"><span data-stu-id="1263f-159">**Add a private IP address**</span></span>

    <span data-ttu-id="1263f-160">egy privát IP-cím tooa NIC tooadd, létre kell hoznia IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="1263f-160">tooadd a private IP address tooa NIC, you must create an IP configuration.</span></span> <span data-ttu-id="1263f-161">hello következő parancs létrehoz egy konfigurációs 10.0.0.7 statikus IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="1263f-161">hello following command creates a configuration with a static IP address of 10.0.0.7.</span></span> <span data-ttu-id="1263f-162">Egy statikus IP-cím megadása esetén egy nem használt hello alhálózati címet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1263f-162">When specifying a static IP address, it must be an unused address for hello subnet.</span></span> <span data-ttu-id="1263f-163">Javasoljuk, hogy először tesztelje hello cím tooensure érhető el hello megadásával `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` parancsot.</span><span class="sxs-lookup"><span data-stu-id="1263f-163">It's recommended that you first test hello address tooensure it's available by entering hello `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` command.</span></span> <span data-ttu-id="1263f-164">Hello IP-cím áll rendelkezésre, ha a hello értéket ad eredményül kimenetet *igaz*.</span><span class="sxs-lookup"><span data-stu-id="1263f-164">If hello IP address is available, hello output returns *True*.</span></span> <span data-ttu-id="1263f-165">Ha nem érhető el, a hello kimeneti értéket ad vissza *hamis*, és a rendelkezésre álló címek listáját.</span><span class="sxs-lookup"><span data-stu-id="1263f-165">If it's not available, hello output returns *False*, and a list of addresses that are available.</span></span>

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
    $MyNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
    ```
    <span data-ttu-id="1263f-166">Tetszőleges számú konfigurációk összes kívánt beállítást, egyedi konfigurációs nevek és privát IP-címek használata (a statikus IP-címekkel rendelkező konfigurációk) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1263f-166">Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).</span></span>

    <span data-ttu-id="1263f-167">Adja hozzá a hello titkos IP-cím toohello virtuális gép operációs rendszere hello lépések végrehajtásával, az operációs rendszernek a hello [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="1263f-167">Add hello private IP address toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span>

    <span data-ttu-id="1263f-168">**A nyilvános IP-cím hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="1263f-168">**Add a public IP address**</span></span>

    <span data-ttu-id="1263f-169">A nyilvános IP-cím jelenik meg egy új IP-konfigurációt vagy egy meglévő IP-konfigurációja nyilvános IP-cím erőforrás tooeither társításával.</span><span class="sxs-lookup"><span data-stu-id="1263f-169">A public IP address is added by associating a public IP address resource tooeither a new IP configuration or an existing IP configuration.</span></span> <span data-ttu-id="1263f-170">Hajtsa végre hello egyik hello szakaszokban szereplő, az összes kívánt beállítást.</span><span class="sxs-lookup"><span data-stu-id="1263f-170">Complete hello steps in one of hello sections that follow, as you require.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1263f-171">Nyilvános IP-címek névleges díjat kell.</span><span class="sxs-lookup"><span data-stu-id="1263f-171">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="1263f-172">tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap.</span><span class="sxs-lookup"><span data-stu-id="1263f-172">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="1263f-173">Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello.</span><span class="sxs-lookup"><span data-stu-id="1263f-173">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="1263f-174">További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.</span><span class="sxs-lookup"><span data-stu-id="1263f-174">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
    >

    - <span data-ttu-id="1263f-175">**Rendeljen hozzá hello nyilvános IP-cím erőforrás tooa új IP konfigurációja**</span><span class="sxs-lookup"><span data-stu-id="1263f-175">**Associate hello public IP address resource tooa new IP configuration**</span></span>
    
        <span data-ttu-id="1263f-176">Egy nyilvános IP-címet ad hozzá egy új IP-konfiguráció, amikor is hozzá kell magánhálózati IP-cím, mert az összes IP-konfiguráció magánhálózati IP-címnek kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1263f-176">Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address.</span></span> <span data-ttu-id="1263f-177">Egy meglévő nyilvános IP-cím erőforrás hozzáadása, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="1263f-177">You can either add an existing public IP address resource, or create a new one.</span></span> <span data-ttu-id="1263f-178">toocreate egy új, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="1263f-178">toocreate a new one, enter hello following command:</span></span>
    
        ```powershell
        $myPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "myPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location `
        -AllocationMethod Static
        ```

        <span data-ttu-id="1263f-179">egy új IP-beállítását egy statikus magánhálózati IP-cím és a kapcsolódó hello toocreate *myPublicIp3* nyilvános IP-cím erőforrás címet, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="1263f-179">toocreate a new IP configuration with a static private IP address and hello associated *myPublicIp3* public IP address resource, enter hello following command:</span></span>

        ```powershell
        Add-AzureRmNetworkInterfaceIpConfig `
        -Name IPConfig-4 `
        -NetworkInterface $myNIC `
        -Subnet $Subnet `
        -PrivateIpAddress 10.0.0.7 `
        -PublicIpAddress $myPublicIp3
        ```

    - <span data-ttu-id="1263f-180">**Rendeljen hozzá hello nyilvános IP-cím erőforrás tooan meglévő IP konfigurációja**</span><span class="sxs-lookup"><span data-stu-id="1263f-180">**Associate hello public IP address resource tooan existing IP configuration**</span></span>

        <span data-ttu-id="1263f-181">A nyilvános IP-cím erőforrás csak lehet társított tooan IP-konfigurációja, amely még nincs társítva.</span><span class="sxs-lookup"><span data-stu-id="1263f-181">A public IP address resource can only be associated tooan IP configuration that doesn't already have one associated.</span></span> <span data-ttu-id="1263f-182">Segítségével meghatározhatja, hogy rendelkezik-e IP-konfigurációt tartozó nyilvános IP-cím hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="1263f-182">You can determine whether an IP configuration has an associated public IP address by entering hello following command:</span></span>

        ```powershell
        $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
        ```

        <span data-ttu-id="1263f-183">Kimeneti hasonló toohello következő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="1263f-183">You see output similar toohello following:</span></span>

        ```     
        Name       PrivateIpAddress PublicIpAddress                                           Primary
        
        IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
        IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
        IpConfig-3 10.0.0.6                                                                     False
        ```

        <span data-ttu-id="1263f-184">Hello óta **PublicIpAddress** oszlopában *IpConfig-3* van üres, nem nyilvános IP-cím erőforrás jelenleg társított tooit.</span><span class="sxs-lookup"><span data-stu-id="1263f-184">Since hello **PublicIpAddress** column for *IpConfig-3* is blank, no public IP address resource is currently associated tooit.</span></span> <span data-ttu-id="1263f-185">Adja hozzá egy meglévő nyilvános IP cím erőforrás tooIpConfig-3, vagy adja meg a következő parancs toocreate egy hello:</span><span class="sxs-lookup"><span data-stu-id="1263f-185">You can add an existing public IP address resource tooIpConfig-3, or enter hello following command toocreate one:</span></span>

        ```powershell
        $MyPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "MyPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location -AllocationMethod Static
        ```

        <span data-ttu-id="1263f-186">Adja meg a következő parancs tooassociate hello nyilvános IP-cím erőforrás toohello meglévő IP-konfiguráció hello *IpConfig-3*:</span><span class="sxs-lookup"><span data-stu-id="1263f-186">Enter hello following command tooassociate hello public IP address resource toohello existing IP configuration named *IpConfig-3*:</span></span>
    
        ```powershell
        Set-AzureRmNetworkInterfaceIpConfig `
        -Name IpConfig-3 `
        -NetworkInterface $mynic `
        -Subnet $Subnet `
        -PublicIpAddress $myPublicIp3
        ```

6. <span data-ttu-id="1263f-187">Állítsa be a hálózati adapter hello hello új IP-konfigurációval hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="1263f-187">Set hello NIC with hello new IP configuration by entering hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $MyNIC
    ```

7. <span data-ttu-id="1263f-188">Hello magánhálózati IP-címek megtekintése és hello nyilvános IP cím erőforrások hozzárendelt toohello megadásával NIC hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="1263f-188">View hello private IP addresses and hello public IP address resources assigned toohello NIC by entering hello following command:</span></span>

    ```powershell   
    $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```
8. <span data-ttu-id="1263f-189">Adja hozzá a hello titkos IP-cím toohello virtuális gép operációs rendszere hello lépések végrehajtásával, az operációs rendszernek a hello [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="1263f-189">Add hello private IP address toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="1263f-190">Hello nyilvános IP-cím toohello operációs rendszere nem adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="1263f-190">Do not add hello public IP address toohello operating system.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
