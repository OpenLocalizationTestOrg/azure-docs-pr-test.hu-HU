---
title: "egy általánosított virtuális Gépet az Azure-ban nem felügyelt képe aaaCreate |} Microsoft Docs"
description: "Egy általános windowsos virtuális gép toouse toocreate unmanged képe több példányt hoz létre a virtuális gépek Azure-ban."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: b8a044d20313efa6dd9b9757e61154f922134476
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-unmanaged-vm-image-from-an-azure-vm"></a>Hogyan toocreate egy nem felügyelt virtuális Gépre az Azure virtuális gép kép

Ez a cikk ismerteti a storage-fiókok használatával. Javasoljuk, hogy használjon felügyelt lemezek és a felügyelt képek helyett egy tárfiókot. További információkért lásd: [egy általánosított virtuális Gépet az Azure-ban a felügyelt lemezképének](capture-image-resource.md).

Ez a cikk bemutatja, hogyan toouse Azure PowerShell toocreate tárfiókot használ általánosított Azure virtuális gép lemezképét. Hello kép toocreate másik virtuális gép majd használni. hello kép hello operációsrendszer-lemez és, amelyek a virtuális géphez csatolt toohello hello adatlemezt tartalmaz. hello kép nem tartalmaz hello virtuális hálózati erőforrások, ezért meg kell tooset fel ezeket az erőforrásokat, amikor hoz létre új virtuális gép hello. 

## <a name="prerequisites"></a>Előfeltételek
Azure PowerShell-verzió toohave kell 1.0.x vagy újabb verzióját telepítették. Ha még nem telepítette a PowerShell, olvassa el [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) telepítési lépéseit.

## <a name="generalize-hello-vm"></a>Virtuális gép hello generalize 
Ez a szakasz bemutatja, hogyan toogeneralize képként használható a Windows virtuális géphez. A virtuális gépek normalizálása eltávolítja a személyes adatok, többek között, és előkészíti a hello gép toobe képként használni. A Sysprep kapcsolatos részletekért lásd: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).

Ellenőrizze, hogy fut a gépen hello hello kiszolgálói szerepkörök Sysprep által támogatott. További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Ha a virtuális merevlemez tooAzure hello az első alkalommal, győződjön meg arról, hogy [a virtuális gép előkészített](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a Sysprep futtatása előtt. 
> 
> 

Linux virtuális gépet is általánosítása `sudo waagent -deprovision+user` , majd PowerShell toocapture hello virtuális gép. Hello CLI toocapture a virtuális gépek használatával kapcsolatos információkért lásd: [hogyan toogeneralize és rögzítése egy Linux virtuális gép használt hello Azure CLI ](../linux/capture-image.md).


1. Bejelentkezés toohello Windows rendszerű virtuális gép.
2. Nyissa meg a hello parancssort rendszergazdaként. Hello könyvtárváltás túl**%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.
3. A hello **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy hello **Generalize** jelölőnégyzet be van jelölve.
4. A **leállítási beállítások**, jelölje be **leállítási**.
5. Kattintson az **OK** gombra.
   
    ![Indítsa el a Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. A Sysprep befejezését követően hello virtuális gép leáll. 

> [!IMPORTANT]
> Újraindítás hello virtuális gép még nem kész feltöltése hello VHD tooAzure vagy kép hello VM történő létrehozását. Ha a virtuális gép hello véletlenül lekérdezi az újraindítás futtassa a Sysprep toogeneralize azt újra.
> 
> 

## <a name="log-in-tooazure-powershell"></a>Jelentkezzen be tooAzure PowerShell
1. Nyissa meg az Azure PowerShell, és jelentkezzen be Azure-fiók tooyour.
   
    ```powershell
    Login-AzureRmAccount
    ```
   
    Egy előugró ablak nyílik meg tooenter az az Azure-fiók hitelesítő adatait.
2. Az elérhető előfizetések beolvasása hello előfizetési azonosítók.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Állítsa be a megfelelő előfizetés hello hello előfizetés-azonosítójával.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-hello-vm-and-set-hello-state-toogeneralized"></a>Hello virtuális gép felszabadítása és hello állapot toogeneralized beállítása
1. Virtuálisgép-erőforrások hello felszabadítani.
   
    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    Hello *állapot* a virtuális gép hello a hello Azure portal-ről változik **leállítva** túl**leállítva (felszabadítva)**.
2. Hello állapot hello virtuális gép beállítása túl**Generalized**. 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. Virtuális gép hello hello állapotának ellenőrzéséhez. Hello **OSState/általánosítva** hello VM kell rendelkeznie a hello szakasz **DisplayStatus** túl beállítása**virtuális gép általánosítva**.  
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-hello-image"></a>Hello lemezkép létrehozása

Hozzon létre egy nem felügyelt virtuálisgép-lemezkép hello cél tárolót használja a következő parancsot. hello lemezkép létrehozása hello azonos módon az eredeti virtuális gép hello tárfiók. Hello `-Path` paraméter hello forrás virtuális gép tooyour helyi számítógép hello JSON-sablon másolatának mentése. Hello `-DestinationContainerName` paraméter hello tároló, amelyet toohold a képek hello neve. Ha hello tároló nem létezik, az Ön létrejön.
   
```powershell
Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
A kép URL-címe hello hello JSON-fájl sablon az kérheti le. Nyissa meg toohello **erőforrások** > **storageProfile** > **osDisk** > **kép**  >  **uri** a lemezkép szakasza hello teljes elérési útja. hello hello kép URL-címe a következőhöz hasonló: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
   
Hello URI hello portálon is ellenőrizheti. hello kép egy nevű másolt tooa tároló **rendszer** tárfiókba. 

## <a name="create-a-vm-from-hello-image"></a>Hozzon létre egy virtuális Gépet hello lemezképből

Egy vagy több virtuális gépek most hello nem felügyelt lemezképből hozhat létre.

### <a name="set-hello-uri-of-hello-vhd"></a>Állítsa be a hello hello VHD URI-val

a virtuális merevlemez toouse hello URI hello hello formátumban kell megadni: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd. Ebben a példában a virtuális merevlemez nevű hello **myVHD** hello tárfiók van **mystorageaccount** hello tárolóban **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a>Virtuális hálózat létrehozása
Hozzon létre hello vNet és alhálózat a hello [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).

1. Hozzon létre hello alhálózatot. hello alábbi minta létrehoz egy nevű alhálózat **mySubnet** hello erőforráscsoportban **myResourceGroup** a címelőtagot hello **10.0.0.0/24**.  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Hello virtuális hálózat létrehozása. hello alábbi minta létrehoz egy virtuális hálózati nevű **myVnet** a hello **USA nyugati régiója** hellyel, amely címelőtagot hello **10.0.0.0/16**.  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Hozzon létre egy nyilvános IP-cím és a hálózati kapcsolat
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

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Hello hálózati biztonsági csoport és egy RDP-szabály létrehozása
toobe képes toolog tooyour a virtuális gép RDP, kell toohave egy biztonsági szabályt, amely lehetővé teszi a hozzáférést RDP a 3389-es porton. 

Ez a példa létrehoz egy NSG nevű **myNsg** nevű szabályt tartalmaz, amelyek **myRdpRule** RDP-forgalmát, amely engedélyezi a 3389-es porton keresztül. Az NSG-k kapcsolatos további információkért lásd: [portok tooa VM megnyitása az Azure-ban a PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-hello-virtual-network"></a>Hozzon létre egy változót hello virtuális hálózat
Hozzon létre egy változót, virtuális hálózat hello befejeződött. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-hello-vm"></a>Hello virtuális gép létrehozása
hello következő PowerShell hello virtuálisgép-konfiguráció befejezése és a nem felügyelt lemezképet használja hello forrásként hello új telepítés.

</br>

```powershell
    # Enter a new user name and password toouse as hello local administrator account 
    # for remotely accessing hello VM.
    $cred = Get-Credential

    # Name of hello storage account where hello VHD is located. This example sets hello 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of hello virtual machine. This example sets hello VM name as "myVM".
    $vmName = "myVM"

    # Size of hello virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See hello VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for hello VM. This examples sets hello computer name as "myComputer".
    $computerName = "myComputer"

    # Name of hello disk that holds hello OS. This example sets hello 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets hello SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get hello storage account where hello uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set hello VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set hello Windows operating system configuration and add hello NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create hello OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure hello OS disk toobe created from hello existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create hello new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

### <a name="verify-that-hello-vm-was-created"></a>Győződjön meg arról, hogy létrejött a virtuális gép hello
Amikor végzett, láthatja az újonnan létrehozott virtuális gép hello hello [Azure-portálon](https://portal.azure.com) alatt **Tallózás** > **virtuális gépek**, vagy a következő hello használatával PowerShell-parancsokkal:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Következő lépések
toomanage az új virtuális gépet az Azure PowerShell, lásd: [kezelése az Azure Resource Manager és a PowerShell használatával virtuális gépek](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


