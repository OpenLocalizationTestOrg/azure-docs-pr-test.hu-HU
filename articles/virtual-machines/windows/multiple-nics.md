---
title: "aaaCreate és kezelése Windows virtuális gépek Azure-ban több hálózati adaptert használó |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és a Windows Azure PowerShell vagy a Resource Manager-sablonok segítségével több hálózati adapter csatolt tooit rendelkező virtuális gépek kezeléséhez."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: c3c7d7569aca6f047238146d84b2ffccf05d4079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a>Létrehozása és kezelése a Windows rendszerű virtuális gép, amely több hálózati adapterrel rendelkezik.
Azure virtuális gépek (VM) több virtuális hálózati illesztő kártyák (NIC) kapcsolódó toothem rendelkezhet. Egy általános forgatókönyv toohave különböző alhálózatokon a előtér- és hálózati kapcsolatot, vagy egy hálózati dedikált tooa figyelési vagy biztonsági mentési megoldás. Ez a cikk részletesen, hogyan toocreate több hálózati adapterrel rendelkező virtuális gép csatlakoztatott tooit. Azt is megtudhatja, hogyan tooadd, vagy távolítsa el a meglévő virtuális hálózati adapter. Különböző [Virtuálisgép-méretek](sizes.md) több hálózati adapter támogatja, így méretezés ennek megfelelően a virtuális Gépet.

Részletes információ, beleértve a hogyan toocreate saját PowerShell-parancsfájlok belül több hálózati adapter: [több hálózati Adapterből virtuális gépek telepítése](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).

## <a name="prerequisites"></a>Előfeltételek
Győződjön meg arról, hogy rendelkezik-e hello [legújabb Azure PowerShell-verzió telepítve és konfigurálva](/powershell/azure/overview).

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők *myResourceGroup*, *myVnet*, és *myVM*.


## <a name="create-a-vm-with-multiple-nics"></a>Több hálózati adapterrel rendelkező virtuális gép létrehozása
Először hozzon létre egy erőforráscsoportot. hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *EastUs* helye:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a>Virtuális hálózati és alhálózatok létrehozásával
Egy általános forgatókönyv a virtuális hálózati toohave van két vagy több alhálózatból. Egy alhálózat előtér-forgalom esetén a háttér-forgalom hello lehet. tooconnect tooboth alhálózatokat, majd használja több hálózati adaptert a virtuális gépen.

1. Adja meg a két virtuális hálózati alhálózat [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). hello alábbi példa meghatározza hello alhálózatok *mySubnetFrontEnd* és *mySubnetBackEnd*:

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. Hozza létre a virtuális hálózat és alhálózat [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet*:

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a>Több hálózati adapter létrehozása
Hozzon létre két hálózati adapterrel [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Rendeljen egy hálózati adapter toohello előtér-alhálózatot és egy hálózati adapter toohello háttér-alhálózatot. hello alábbi példakód létrehozza nevű hálózati adaptert *myNic1* és *myNic2*:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

Általában akkor is létrehozhat egy [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) vagy [terheléselosztó](../../load-balancer/load-balancer-overview.md) toohelp kezelése és a forgalom szétosztását a virtuális gépek. Hello [több hálózati Adapterből VM részletes](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) a cikk végigvezeti a hálózati biztonsági csoportok létrehozása és hozzárendelése a hálózati adapterek.

### <a name="create-hello-virtual-machine"></a>Hello virtuális gép létrehozása
Toobuild most elindítja a virtuális gép konfigurációt. Minden egyes Virtuálisgép-méretet a vehet fel tooa virtuális gép hálózati adapterek száma hello van korlátozva. További információkért lásd: [Windows Virtuálisgép-méretek](sizes.md).

1. Állítsa be a virtuális gép hitelesítő adatok toohello `$cred` változót az alábbiak szerint:

    ```powershell
    $cred = Get-Credential
    ```

2. Adja meg a virtuális Gépet a [új AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). hello alábbi példa meghatározza egy nevű virtuális gép *myVM* és használ, amely támogatja a több mint két hálózati adapter Virtuálisgép-méretet (*Standard_DS3_v2*):

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. Hozzon létre a Virtuálisgép-konfiguráció, a többi hello [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) és [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage). a következő példa hello egy Windows Server 2016 virtuális Gépet hoz létre:

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. Hello két hálózati adaptert, amely a korábban létrehozott csatolása [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. Végezetül hozza létre a virtuális Gépet a [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

## <a name="add-a-nic-tooan-existing-vm"></a>Egy meglévő virtuális gép hálózati tooan hozzáadása
a virtuális hálózati adapter tooan a meglévő virtuális gép felszabadítása hello VM, tooadd hozzáadása a virtuális hálózati adapter hello, majd indítsa el a virtuális gép hello.

1. Felszabadítani a virtuális gép és hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). hello alábbi példa felszabadítja a hello nevű virtuális gép *myVM* a *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Meglévő konfigurációját hello hello VM lekérése a [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). hello alábbi példa lekérdezi hello nevű virtuális gép adatai *myVM* a *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. hello alábbi példakód létrehozza a virtuális hálózati adapter a [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) nevű *myNic3* túl társított*mySubnetBackEnd*. a virtuális hálózati adapter van, majd hello csatolt toohello nevű virtuális gép *myVM* a *myResourceGroup* rendelkező [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    # Get info for hello back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get hello ID of hello new virtual NIC and add tooVM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a>Elsődleges virtuális hálózati adapter
    Egy hálózati adaptert a virtuális gép több hálózati Adapterrel hello kell toobe elsődleges. Ha egy meglévő virtuális hálózati adapter hello hello a virtuális gép már be van állítva az elsődleges, kihagyhatja ezt a lépést. hello alábbi példa feltételezi, hogy két virtuális hálózati adaptert a virtuális gép található, illetve tooadd kívánja hello első hálózati adapter (`[0]`), elsődleges hello:
        
    ```powershell
    # List existing NICs on hello VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 toobe primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update hello VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. Indítsa el a virtuális gép hello [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

## <a name="remove-a-nic-from-an-existing-vm"></a>Egy meglévő virtuális gép egy hálózati adapter eltávolítása
a virtuális hálózati adapter tooremove egy meglévő virtuális gépből hello virtuális gép felszabadítása, hello távolítsa el a virtuális hálózati Adapterre, majd a start hello virtuális gép.

1. Felszabadítani a virtuális gép és hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). hello alábbi példa felszabadítja a hello nevű virtuális gép *myVM* a *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Meglévő konfigurációját hello hello VM lekérése a [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). hello alábbi példa lekérdezi hello nevű virtuális gép adatai *myVM* a *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Hálózati adapter eltávolítása a hello adatainak beolvasása [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface). hello alábbi példa lekérdezi, *myNic3*:

    ```powershell
    # List existing NICs on hello VM if you need toodetermine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. Távolítsa el a hálózati adapter hello [Remove-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) és hello tulajdonsággal rendelkező virtuális gépet, majd frissítés [frissítés-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm). hello következő példában eltávolítjuk *myNic3* módon nyert `$nicId` az előző lépésben hello:

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. Indítsa el a virtuális gép hello [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a>Több hálózati adapterrel létrehozott sablonok
Az Azure Resource Manager-sablonok adjon meg egy módon toocreate erőforrás több példánya központi telepítést végez, például több hálózati adapter létrehozása során. Resource Manager-sablonok használata deklaratív JSON-fájlok toodefine a környezetben. További információkért lásd: [áttekintése Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Használhat *másolási* példányok toocreate toospecify hello száma:

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

További információkért lásd: [használatával több példány létrehozásával *másolási*](../../resource-group-create-multiple.md). 

Is `copyIndex()` tooappend számú tooa erőforrás neve. Ezután létrehozhat *myNic1*, *MyNic2* és így tovább. hello alábbira példáját mutatja be fűznek hello súgóindex-értéket:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Átfogó példát olvasható [több hálózati adapter létrehozása a Resource Manager-sablonok segítségével](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Következő lépések
Felülvizsgálati [Windows Virtuálisgép-méretek](sizes.md) amikor próbált toocreate több hálózati adapterrel rendelkező virtuális gép. Nagy figyelmet toohello maximális hálózati adapterek száma, amely támogatja az egyes Virtuálisgép-méretet. 


