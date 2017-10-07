---
title: "az Azure-ban kezelt Virtuálisgép-lemezképet a virtuális gép aaaCreate |} Microsoft Docs"
description: "Hozzon létre egy Windows rendszerű virtuális gép egy általánosított felügyelt Virtuálisgép-lemezkép az Azure PowerShell, hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: cynthn
ms.openlocfilehash: 5036ef1533c144a9a328e94599b359e0166f337d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-from-a-managed-image"></a>Egy felügyelt képre a virtuális gép létrehozása

Létrehozhat több virtuális gép az Azure-ban kezelt Virtuálisgép-lemezképet. Egy felügyelt Virtuálisgép-lemezkép hello információ szükséges toocreate egy virtuális Gépet, beleértve a hello operációsrendszer- és adatlemezek tartalmazza. hello hello kép, beleértve az operációs rendszer hello lemezek és bármely adatlemezek alkotják, felügyelt lemezként tárolt virtuális merevlemezeket. 


## <a name="prerequisites"></a>Előfeltételek

Már szüksége toohave [felügyelt Virtuálisgép-lemezkép létrehozása](capture-image-resource.md) létrehozásához toouse hello új virtuális Gépet. 

Győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute és AzureRM.Network PowerShell modulok legújabb verzióját. Nyissa meg rendszergazdaként egy PowerShell-parancssorba, majd futtassa a következő parancs tooinstall hello őket.

```powershell
Install-Module AzureRM.Compute,AzureRM.Network
```
További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).



## <a name="collect-information-about-hello-image"></a>Hello lemezképére vonatkozó információ gyűjtése

Először igazolnia toogather alapszintű hello kép információra van szüksége, és hozzon létre egy változót hello lemezkép. Ez a példa egy felügyelt Virtuálisgép-lemezkép nevű **myImage** , amely a hello **myResourceGroup** hello erőforráscsoportja **nyugati középső Régiójában** helyét. 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a>Virtuális hálózat létrehozása
Hozzon létre hello vNet és alhálózat a hello [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).

1. Hozzon létre hello alhálózatot. Ez a példa-alhálózatot hoz létre nevű **mySubnet** a címelőtagot hello **10.0.0.0/24**.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Hello virtuális hálózat létrehozása. Ebben a példában egy virtuális hálózatot nevű hoz létre **myVnet** a címelőtagot hello **10.0.0.0/16**.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Hozzon létre egy nyilvános IP-cím és a hálózati kapcsolat

hello virtuális gép virtuális hálózati hello kommunikációs tooenable, van szüksége egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter.

1. Hozzon létre egy nyilvános IP-címet. Ez a példa létrehoz egy nyilvános IP-cím nevű **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Hozzon létre hello hálózati adaptert. Ez a példa létrehoz egy hálózati adapter nevű **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Hello hálózati biztonsági csoport és egy RDP-szabály létrehozása

toobe képes toolog tooyour a virtuális gép RDP, kell toohave hálózati biztonsági szabály (NSG), amely lehetővé teszi a hozzáférést RDP a 3389-es porton. 

Ez a példa létrehoz egy NSG nevű **myNsg** nevű szabályt tartalmaz, amelyek **myRdpRule** RDP-forgalmát, amely engedélyezi a 3389-es porton keresztül. Az NSG-k kapcsolatos további információkért lásd: [portok tooa VM megnyitása az Azure-ban a PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-hello-virtual-network"></a>Hozzon létre egy változót hello virtuális hálózat

Hozzon létre egy változót, virtuális hálózat hello befejeződött. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-hello-credentials-for-hello-vm"></a>A virtuális gép hello hello hitelesítő adatainak lekérése

hello következő parancsmag megnyílik egy ablak, ha beírja egy új felhasználói nevet és jelszót toouse hello helyi rendszergazda fiókot távolról a virtuális gép hello eléréséhez. 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-hello-vm-name-computer-name-and-hello-size-of-hello-vm"></a>Set változói hello virtuális gép neve, a számítógép nevét, és hello hello virtuális gép méretét

1. Hozzon létre változókat hello virtuális gép nevét és a számítógép nevét. Ez a példa beállítja hello virtuális gép neve, mint **myVM** és számítógépnévvel hello **Sajátgép**.

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. Virtuális gép hello hello méretének beállítása. Ez a példa létrehoz **Standard_DS1_v2** méretű virtuális gép. Lásd: hello [Virtuálisgép-méretek](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) dokumentációjában talál további információt.

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. Adja hozzá a hello virtuális gép neve és mérete toohello Virtuálisgép-konfiguráció.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-hello-vm-image-as-source-image-for-hello-new-vm"></a>Set hello Virtuálisgép-lemezkép forrás képként hello az új virtuális gép

Állítsa be a hello forráslemezkép hello felügyelt Virtuálisgép-lemezkép hello Azonosítóját használja.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-hello-os-configuration-and-add-hello-nic"></a>Állítsa be a hello operációs rendszer konfigurációja, és adja hozzá a hello hálózati adaptert.

Adja meg a hello tárolási típust (PremiumLRS vagy StandardLRS) és hello hello operációsrendszer-lemez méretét. Ez a példa beállítja hello fiók típusa túl**PremiumLRS**, lemez mérete túl hello**128 GB-os** és a lemez gyorsítótár túl**ReadWrite**.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-hello-vm"></a>Hello virtuális gép létrehozása

Hozzon létre új virtuális gép hello konfigurációval rendelkezik beépített és hello tárolt hello **$vm** változó.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-hello-vm-was-created"></a>Győződjön meg arról, hogy létrejött a virtuális gép hello
Amikor végzett, láthatja az újonnan létrehozott virtuális gép hello hello [Azure-portálon](https://portal.azure.com) alatt **Tallózás** > **virtuális gépek**, vagy a következő hello használatával PowerShell-parancsokkal:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Következő lépések
toomanage az új virtuális gépet az Azure PowerShell, lásd: [létrehozása és kezelése Windows virtuális gépek hello Azure PowerShell modul](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

