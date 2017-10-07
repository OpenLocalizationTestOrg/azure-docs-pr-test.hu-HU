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
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-powershell"></a>Több IP-cím hozzárendelése toovirtual gépek PowerShell használatával

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Ez a cikk azt ismerteti, hogyan toocreate egy virtuális gép (VM) keresztül hello Azure Resource Manager telepítési modell PowerShell használatával. Több IP-cím hello klasszikus telepítési modell használatával létrehozott tooresources nem lehet hozzárendelni. További információk az Azure üzembe helyezési modellel, olvassa el a hello toolearn [üzembe helyezési modellel megértéséhez](../resource-manager-deployment-model.md) cikk.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Több IP-címekkel rendelkező virtuális gép létrehozása

hello következő lépések azt ismertetik, hogyan toocreate például VM több IP-címek, hello forgatókönyvben leírtak szerint. A végrehajtásához szükséges változók értékeinek módosítása.

1. Nyisson meg egy PowerShell-parancssort, és teljes hello hátralévő lépéseket ebben a szakaszban egy PowerShell-munkameneten belül. Ha még nem rendelkezik a PowerShell telepítése és konfigurálása, teljes hello hello szükséges lépések [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk.
2. Bejelentkezési tooyour fiókot hello `login-azurermaccount` parancsot.
3. Cserélje le *myResourceGroup* és *westus* a nevet és a helyet. Hozzon létre egy erőforráscsoportot. Az erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.

    ```powershell
    $RgName   = "MyResourceGroup"
    $Location = "westus"

    New-AzureRmResourceGroup `
    -Name $RgName `
    -Location $Location
    ```

4. Hozzon létre egy virtuális hálózathoz (VNet) és az alhálózati hello hello erőforráscsoport és ugyanazon a helyen:

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

5. A hálózati biztonsági csoport (NSG) és a szabály létrehozása. hello NSG biztosítja a virtuális gép hello bejövő és kimenő szabályok használatával. Ebben az esetben létrejön egy bejövő szabály a 3389-es porthoz, amely lehetővé teszi a bejövő távoli asztali kapcsolatokat.

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

6. Adja meg az elsődleges IP-konfiguráció hello hello hálózati adaptert. Változás 10.0.0.4 tooa érvényes cím hello hozott létre, ha korábban definiált hello érték nem az alhálózaton. Egy statikus IP-cím hozzárendelése, előtt ajánlott, hogy először megerősíti nem már használatban van. Adja meg a hello parancs `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`. Ha hello címkészletében nem áll rendelkezésre, a hello kimeneti értéket ad vissza *igaz*. Ha nem érhető el, a hello kimeneti értéket ad vissza *hamis* és a rendelkezésre álló címek listáját. 

    A következő parancsokat, hello **hello egyedi DNS-név toouse cserélje le < csere-az-a-egyedi-neve >.** hello nevének egyedinek kell lennie a kívánt Azure-régiót belül minden nyilvános IP-címek között. Ez nem kötelező paraméter. El kell távolítani, ha kizárólag tooconnect toohello VM hello nyilvános IP-címet használja.

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

    Ha több IP-konfigurációk tooa hálózati adapter, egy konfigurációs hozzá kell rendelni, hello *-elsődleges*.

    > [!NOTE]
    > Nyilvános IP-címek névleges díjat kell. tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap. Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello. További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.

7. Adja meg a másodlagos IP-konfigurációk hello hello hálózati adaptert. Adja hozzá, vagy távolítsa el a szükséges konfigurációk. Minden IP-konfiguráció egy privát IP-cím kell rendelkeznie. Minden egyes konfigurációs szükség lehet egy nyilvános IP-cím hozzárendelve.

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

8. A hálózati adapter hello létrehoz, hello három IP-konfigurációk tooit:

    ```powershell
    
    $NIC = New-AzureRmNetworkInterface `
    -Name MyNIC `
    -ResourceGroupName $RgName `
    -Location $Location `
    -NetworkSecurityGroupId $NSG.Id `
    -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
    ```

    >[!NOTE]
    >Bár az összes konfiguráció hozzárendelt hálózati adapter tooone ebben a cikkben, rendelhet hozzá több IP konfigurációk tooevery csatlakoztatott hálózati toohello virtuális gép. hogyan olvassa el a virtuális gép több hálózati adaptert, és toocreate toolearn hello [több hálózati adapterrel rendelkező virtuális gép létrehozása](virtual-network-deploy-multinic-arm-ps.md) cikk.

9. Hozza létre a virtuális gép hello hello a következő parancsok beírásával:

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

10. Hozzáadás hello privát IP-címek toohello virtuális gép operációs rendszer hello lépések végrehajtásával, az operációs rendszernek a hello [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát. Hello nyilvános IP-címek toohello operációs rendszere nem adja hozzá.

## <a name="add"></a>Adja hozzá az IP-címek tooa méretű VM

Magán- és IP címeket tooa NIC hello következő lépések végrehajtásával adhat hozzá. hello hello a következő szakaszokban szereplő példák azt feltételezik, hogy már van egy virtuális gép hello három IP-konfigurációk hello ismertetett [forgatókönyv](#Scenario) ezen cikk, de nem szükséges, hogy végezzen.

1. Nyisson meg egy PowerShell-parancssort, és teljes hello hátralévő lépéseket ebben a szakaszban egy PowerShell-munkameneten belül. Ha még nem rendelkezik a PowerShell telepítése és konfigurálása, teljes hello hello szükséges lépések [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk.
2. A következő $Variables toohello neve hello tooadd IP cím tooand hello erőforrás csoport és hely hello hálózati adapter létezik a kívánt hálózati hello "értékek" hello módosítása:

    ```powershell
    $NicName  = "MyNIC"
    $RgName   = "MyResourceGroup"
    $Location = "westus"
    ```

    Ha nem tudja hello hello toochange kívánt hálózati adapter nevét, adja meg a következő parancsok hello, majd módosítsa a hello hello előző változók értékeit:

    ```powershell
    Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location
    ```
3. Hozzon létre egy változót, és állítsa be úgy a hálózati adapter meglévő hello a következő parancs beírásával toohello:

    ```powershell
    $MyNIC = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName
    ```
4. A következő parancsok hello, módosítsa *MyVNet* és *MySubnet* toohello nevei hello VNet és alhálózat hello hálózati adapter csatlakozik. Adja meg a hello parancsok tooretrieve hello VNet és alhálózat objektumok hello hálózati adapter csatlakozik:

    ```powershell
    $MyVNet = Get-AzureRMVirtualnetwork -Name MyVNet -ResourceGroupName $RgName
    $Subnet = $MyVnet.Subnets | Where-Object { $_.Name -eq "MySubnet" }
    ```
    Ha nem tudja hello virtuális hálózat vagy az alhálózat neve hello hálózati adapter csatlakoztatva van, adja meg a következő parancs hello:
    ```powershell
    $MyNIC.IpConfigurations
    ```
    A hello kimeneti keresse meg a következő egy példa a kimenetre szöveg hasonló toohello:
    
    ```
    "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet/subnets/MySubnet"
    ```
    A kimenet *MyVnet* hello VNet és *MySubnet* hello alhálózati hello hálózati adapter van csatlakoztatva van.

5. Végezze el a következő szakaszok a követelmények alapján hello egyikében hello lépéseket:

    **Adja hozzá a magánhálózati IP-cím**

    egy privát IP-cím tooa NIC tooadd, létre kell hoznia IP-konfigurációt. hello következő parancs létrehoz egy konfigurációs 10.0.0.7 statikus IP-címmel. Egy statikus IP-cím megadása esetén egy nem használt hello alhálózati címet kell lennie. Javasoljuk, hogy először tesztelje hello cím tooensure érhető el hello megadásával `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` parancsot. Hello IP-cím áll rendelkezésre, ha a hello értéket ad eredményül kimenetet *igaz*. Ha nem érhető el, a hello kimeneti értéket ad vissza *hamis*, és a rendelkezésre álló címek listáját.

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
    $MyNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
    ```
    Tetszőleges számú konfigurációk összes kívánt beállítást, egyedi konfigurációs nevek és privát IP-címek használata (a statikus IP-címekkel rendelkező konfigurációk) létrehozása.

    Adja hozzá a hello titkos IP-cím toohello virtuális gép operációs rendszere hello lépések végrehajtásával, az operációs rendszernek a hello [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát.

    **A nyilvános IP-cím hozzáadása**

    A nyilvános IP-cím jelenik meg egy új IP-konfigurációt vagy egy meglévő IP-konfigurációja nyilvános IP-cím erőforrás tooeither társításával. Hajtsa végre hello egyik hello szakaszokban szereplő, az összes kívánt beállítást.

    > [!NOTE]
    > Nyilvános IP-címek névleges díjat kell. tudnivalók az IP-cím díjszabást toolearn olvasási hello [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses) lap. Nincs előfizetés használható nyilvános IP-címek számának korlátozása toohello. További információk hello korlátok, olvassa el a hello toolearn [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.
    >

    - **Rendeljen hozzá hello nyilvános IP-cím erőforrás tooa új IP konfigurációja**
    
        Egy nyilvános IP-címet ad hozzá egy új IP-konfiguráció, amikor is hozzá kell magánhálózati IP-cím, mert az összes IP-konfiguráció magánhálózati IP-címnek kell rendelkeznie. Egy meglévő nyilvános IP-cím erőforrás hozzáadása, vagy hozzon létre egy újat. toocreate egy új, adja meg a következő parancs hello:
    
        ```powershell
        $myPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "myPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location `
        -AllocationMethod Static
        ```

        egy új IP-beállítását egy statikus magánhálózati IP-cím és a kapcsolódó hello toocreate *myPublicIp3* nyilvános IP-cím erőforrás címet, adja meg a következő parancs hello:

        ```powershell
        Add-AzureRmNetworkInterfaceIpConfig `
        -Name IPConfig-4 `
        -NetworkInterface $myNIC `
        -Subnet $Subnet `
        -PrivateIpAddress 10.0.0.7 `
        -PublicIpAddress $myPublicIp3
        ```

    - **Rendeljen hozzá hello nyilvános IP-cím erőforrás tooan meglévő IP konfigurációja**

        A nyilvános IP-cím erőforrás csak lehet társított tooan IP-konfigurációja, amely még nincs társítva. Segítségével meghatározhatja, hogy rendelkezik-e IP-konfigurációt tartozó nyilvános IP-cím hello a következő parancs beírásával:

        ```powershell
        $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
        ```

        Kimeneti hasonló toohello következő jelenik meg:

        ```     
        Name       PrivateIpAddress PublicIpAddress                                           Primary
        
        IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
        IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
        IpConfig-3 10.0.0.6                                                                     False
        ```

        Hello óta **PublicIpAddress** oszlopában *IpConfig-3* van üres, nem nyilvános IP-cím erőforrás jelenleg társított tooit. Adja hozzá egy meglévő nyilvános IP cím erőforrás tooIpConfig-3, vagy adja meg a következő parancs toocreate egy hello:

        ```powershell
        $MyPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "MyPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location -AllocationMethod Static
        ```

        Adja meg a következő parancs tooassociate hello nyilvános IP-cím erőforrás toohello meglévő IP-konfiguráció hello *IpConfig-3*:
    
        ```powershell
        Set-AzureRmNetworkInterfaceIpConfig `
        -Name IpConfig-3 `
        -NetworkInterface $mynic `
        -Subnet $Subnet `
        -PublicIpAddress $myPublicIp3
        ```

6. Állítsa be a hálózati adapter hello hello új IP-konfigurációval hello a következő parancs beírásával:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $MyNIC
    ```

7. Hello magánhálózati IP-címek megtekintése és hello nyilvános IP cím erőforrások hozzárendelt toohello megadásával NIC hello a következő parancsot:

    ```powershell   
    $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```
8. Adja hozzá a hello titkos IP-cím toohello virtuális gép operációs rendszere hello lépések végrehajtásával, az operációs rendszernek a hello [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát. Hello nyilvános IP-cím toohello operációs rendszere nem adja hozzá.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
