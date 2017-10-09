---
title: "a klasszikus virtuális gép tooan ARM kezelt lemez VM aaaMigrate |} Microsoft Docs"
description: "Egy Azure virtuális át hello klasszikus telepítési modell tooManaged lemezek hello Resource Manager üzembe helyezési modellben."
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
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: d8c4b9431f5dd8a071fcbc2ee36581a33f76ba62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manually-migrate-a-classic-vm-tooa-new-arm-managed-disk-vm-from-hello-vhd"></a>Manuálisan telepítse át a klasszikus virtuális gép tooa új ARM kezelt lemez virtuális gép virtuális merevlemez hello 


Ez a szakasz elősegíti, toomigrate a meglévő Azure virtuális gépek hello klasszikus telepítési modellből túl[kezelt lemezek](managed-disks-overview.md) hello Resource Manager üzembe helyezési modellben.


## <a name="plan-for-hello-migration-toomanaged-disks"></a>Hello áttelepítés tervezése tooManaged lemezek

Ez a szakasz segítséget nyújt toomake hello legjobb döntést a virtuális gép és a lemez típusok.


### <a name="location"></a>Hely

Jelölje ki a helyet, ahol Azure felügyelt lemezek érhetőek el. TooPremium kezelt lemezek telepít át, ha is győződjön meg arról, hogy prémium szintű storage elérhető hello régióban, ha azt tervezi, hogy toomigrate. Lásd: [Azure Services byRegion](https://azure.microsoft.com/regions/#services) elérhető helyről naprakész tájékoztatást.

### <a name="vm-sizes"></a>A virtuális gépek mérete

TooPremium kezelt lemezek telepít át, ha vannak tooupdate hello mérete hello VM tooPremium képes tárméret elérhető hello régió, ahol a virtuális gép. Tekintse át, amelyek képesek a prémium szintű Storage hello Virtuálisgép-méretek. hello Azure virtuális gép mérete specifikációk szereplő [virtuális gépek méretei](sizes.md).
Tekintse át a virtuális gépek, amely együttműködik a prémium szintű Storage, és válassza ki a hello leginkább megfelelő virtuális gép méretét, amely a legjobban megfelel a számítási feladatok hello teljesítményétől. Győződjön meg arról, hogy nincs elegendő sávszélesség érhető el a virtuális gép toodrive hello lemez forgalom.

### <a name="disk-sizes"></a>Lemezméretek

**Prémium szintű felügyelt lemez**

Hét különböző típusú premium felügyelt lemezek, amelyek együtt a virtuális Gépet, és mindegyik rendelkezik-e adott iops-érték és átviteli korlátok. Vegye figyelembe a működés felső korlátjának hello prémium szintű lemez típusát a virtuális gép alapján az alkalmazás kapacitása, teljesítmény, méretezhetőség hello igényeinek, és a maximális betölti kiválasztása.

| Prémium szintű lemezek típusa  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Lemezméret           | 128 GB| 512 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS-érték lemezenként       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Adattovábbítás lemezenként | 25 MB / s  | 50 MB / s  | 100 MB / s | 150 MB / s | 200 MB / s | 250 MB / s | 250 MB / s | 

**Standard szintű felügyelt lemez**

Standard szintű felügyelt lemez, amely a virtuális gép használható hét típusa van. Azok a különböző kapacitással rendelkeznek, de azonos IOPS és átviteli sebességének korlátai. Válassza ki az alkalmazás hello kapacitásigények alapján standard szintű felügyelt lemez hello típusú.

| Standard lemez típusa  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Lemezméret           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2048 GB (2TB)    | 4095 GB (4 TB)   | 
| IOPS-érték lemezenként       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Adattovábbítás lemezenként | 60 MB / s | 60 MB / s | 60 MB / s | 60 MB / s | 60 MB / s | 60 MB / s | 60 MB / s | 


### <a name="disk-caching-policy"></a>Lemez gyorsítótárazási házirend 

**Prémium szintű felügyelt lemez**

Alapértelmezés szerint a gyorsítótárazási házirend lemez van *írásvédett* az összes hello prémium adatlemezek, és *írható-olvasható* hello prémium operációsrendszer-lemez csatolni a virtuális gép toohello. A konfigurációs beállítás ajánlott tooachieve hello optimális teljesítménye az alkalmazás IOs-hez. Írási műveleteket vagy a csak írható adatlemezek (köztük SQL Server) tiltsa le a lemezt gyorsítótárazás, hogy az alkalmazás jobb teljesítményt érhet el.

### <a name="pricing"></a>Díjszabás

Felülvizsgálati hello [kezelt lemezek árképzési](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Prémium szintű felügyelt lemez árképzési legyen, mint hello nem felügyelt Premium lemezek. Azonban a standard szintű felügyelt lemez árképzési más nem felügyelt Standard lemezeknél.


## <a name="checklist"></a>Feladatlista

1.  Ha tooPremium kezelt lemezek telepít át, győződjön meg arról érhető el hello régió telepít át.

2.  Döntse el, hello új Virtuálisgép-sorozat fog használni. A prémium szintű Storage képes kell, ha telepít át tooPremium kezelt lemezek.

3.  Döntse el, hello pontos Virtuálisgép-méretet használandó hello régió telepít át a rendelkezésre álló. Virtuálisgép-méretet kell toobe elég nagy toosupport hello rendelkezik adatlemezek száma. Például ha négy adatlemezek, hello virtuális gép két vagy több maggal kell rendelkeznie. Fontolja meg is, a feldolgozási kapacitása, memória, és a hálózati sávszélesség igényeinek megfelelően.

4.  Hello aktuális virtuális gép adatai lesz szüksége, beleértve a lemezek és a megfelelő VHD-blobok hello listája rendelkezik.

Készítse elő az állásidő alkalmazását. egy tiszta áttelepítési toodo, vannak toostop összes hello feldolgozási hello aktuális rendszer. Csak ezután beszerezheti tooconsistent állapotát, amely áttelepíthető toohello új platformon. Állásidő időtartama hello adatmennyiséget a hello lemezek toomigrate függ.


## <a name="migrate-hello-vm"></a>Telepítse át a virtuális gép hello

Készítse elő az állásidő alkalmazását. egy tiszta áttelepítési toodo, vannak toostop összes hello feldolgozási hello aktuális rendszer. Csak ezután beszerezheti tooconsistent állapotát, amely áttelepíthető toohello új platformon. Állásidő időtartama hello adatmennyiséget a hello lemezek toomigrate függ.


1.  Első lépésként állítsa be az általános paraméterek hello:

    ```powershell
    $resourceGroupName = 'yourResourceGroupName'
    
    $location = 'your location' 
    
    $virtualNetworkName = 'yourExistingVirtualNetworkName'
    
    $virtualMachineName = 'yourVMName'
    
    $virtualMachineSize = 'Standard_DS3'
    
    $adminUserName = "youradminusername"
    
    $adminPassword = "yourpassword" | ConvertTo-SecureString -AsPlainText -Force
    
    $imageName = 'yourImageName'
    
    $osVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'
    
    $dataVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk1.vhd'
    
    $dataDiskName = 'dataDisk1'
    ```

2.  Hozzon létre egy felügyelt operációsrendszer-lemez használatával hello VHD hello a klasszikus virtuális gép.

    Győződjön meg arról, hogy rendelkezik a megadott hello végezze el az operációs rendszer virtuális merevlemez toohello $osVhdUri paraméter hello URI Azonosítóját. Emellett adja meg **- AccountType** , **PremiumLRS** vagy **StandardLRS** alapú lemezek (prémium és Standard) típusú végzi az áttelepítést.

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $osVhdUri) '
    -ResourceGroupName $resourceGroupName
    ```

3.  Az operációs rendszer hello lemez toohello csatolása új virtuális Gépet.

    ```powershell
    $VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $osDisk.Id '
    -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

4.  Felügyelt adatlemezt készíteni hello VHD-fájlt, és adja hozzá toohello új virtuális Gépet.

    ```powershell
    $dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreationDataCreateOption Import '
    -SourceUri $dataVhdUri ) -ResourceGroupName $resourceGroupName
    
    $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName '
    -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
    ```

5.  Hozzon létre új virtuális gép hello úgy, hogy nyilvános IP-, a virtuális hálózat és a hálózati adaptert.

    ```powershell
    $publicIp = New-AzureRmPublicIpAddress -Name ($VirtualMachineName.ToLower()+'_ip') '
    -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
    
    $vnet = Get-AzureRmVirtualNetwork -Name $virtualNetworkName -ResourceGroupName $resourceGroupName
    
    $nic = New-AzureRmNetworkInterface -Name ($VirtualMachineName.ToLower()+'_nic') '
    -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id '
    -PublicIpAddressId $publicIp.Id
    
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nic.Id
    
    New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $resourceGroupName -Location $location
    ```

> [!NOTE]
>Előfordulhat, hogy további lépéseket szükséges toosupport az alkalmazás, amely nem elegendő az útmutatóban.
>
>

## <a name="next-steps"></a>Következő lépések

- Csatlakoztassa a toohello virtuális gépet. Útmutatásért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

