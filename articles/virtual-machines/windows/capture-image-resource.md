---
title: "az Azure-ban egy felügyelt képre aaaCreate |} Microsoft Docs"
description: "Egy felügyelt képre egy általánosított virtuális gép vagy egy virtuális merevlemez létrehozása az Azure-ban. Lemezképek lehet használt toocreate, a felügyelt lemezeket használó több virtuális géphez."
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
ms.date: 02/27/2017
ms.author: cynthn
ms.openlocfilehash: d8cd6c2ce8c5d704de2c845abced85139944d682
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a>Egy felügyelt képre egy általánosított virtuális gép létrehozása az Azure-ban

Egy felügyelt képre erőforrás vagy egy felügyelt lemezes, vagy egy nem felügyelt lemezek tárfiókokban tárolt általánosított virtuális gép alapján hozhatók létre. kép is hello akkor használt toocreate több virtuális géphez. 


## <a name="generalize-hello-windows-vm-using-sysprep"></a>Generalize hello Windows virtuális gépet a Sysprep

A Sysprep eltávolítja a személyes adatok, többek között, és előkészíti a hello gép toobe képként használni. A Sysprep kapcsolatos részletekért lásd: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).

Ellenőrizze, hogy fut a gépen hello hello kiszolgálói szerepkörök Sysprep által támogatott. További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Ha futtatja a Sysprep eszközt a virtuális merevlemez tooAzure hello az első alkalommal feltöltés előtt, győződjön meg arról, hogy [a virtuális gép előkészített](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a Sysprep futtatása előtt. 
> 
> 

1. Bejelentkezés toohello Windows rendszerű virtuális gép.
2. Nyissa meg a hello parancssort rendszergazdaként. Hello könyvtárváltás túl**%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.
3. A hello **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy hello **Generalize** jelölőnégyzet be van jelölve.
4. A **leállítási beállítások**, jelölje be **leállítási**.
5. Kattintson az **OK** gombra.
   
    ![Indítsa el a Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. A Sysprep befejezését követően hello virtuális gép leáll. Ne indítsa újra a virtuális gép hello.


## <a name="create-a-managed-image-in-hello-portal"></a>Hozzon létre egy felügyelt képre hello portálon 

1. Nyissa meg hello [portal](https://portal.azure.com).
2. Kattintson az új erőforrás hello toocreate plusz jelre.
3. Írja be a hello szűrő keresési, **kép**.
4. Válassza ki **kép** hello eredménybe.
5. A hello **kép** panelen kattintson a **létrehozása**.
6. A **neve**, adjon meg egy nevet hello kép.
7. Ha egynél több előfizetéssel rendelkezik, válasszon hello hello a megfelelő sablon **előfizetés** legördülő listán.
7. A **erőforráscsoport** válassza **hozzon létre új** , és adjon meg egy nevet, vagy válasszon **meglévő** hello legördülő listában válassza ki az egy erőforrás csoport toouse.
8. A **hely**, válassza ki az erőforráscsoport hello helyét.
9. A **operációsrendszer-típus** hello Windows vagy Linux operációs rendszer típusa.
11. A **tárolási blob**, kattintson a **Tallózás** toolook a hello VHD az Azure-tárfiókba.
12. A **fiók típusa** Standard_LRS vagy Premium_LRS kiválasztása. Szabványos merevlemez-meghajtókat használ, és a prémium SSD meghajtókat használ. Mindkettő használja a helyileg redundáns tárolás.
13. A **lemez gyorsítótárazás** válasszon hello megfelelő lemezt gyorsítótárazási beállítását. hello beállítások **nincs**, **írásvédett** és **olvasási\írási**.
14. Választható lehetőség: Is hozzáadhat egy meglévő adatok toohello lemezképét kattintva **+ Hozzáadás adatlemez**.  
15. Ha végzett a Kiválasztás gombra **létrehozása**.
16. Hello lemezkép létrehozása után látni fogja, hogy egy **kép** hello az erőforrások listájához a hello erőforráscsoport úgy döntött, hogy az erőforrás.



## <a name="create-a-managed-image-of-a-vm-using-powershell"></a>Hozzon létre egy felügyelt képre a virtuális gépek Powershell használatával

Lemezkép létrehozása a közvetlenül a virtuális gép biztosítja, hogy hello kép hello tartalmazza az összes társított virtuális gép, beleértve az operációsrendszer-lemez hello és bármely adatlemezek hello hello lemezek.


Mielőtt elkezdené, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját. Futtatás hello parancs tooinstall követően.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).


1. Bizonyos változókat létrehozni.

    ```powershell
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. Győződjön meg arról, hogy hello VM fel lettek szabadítva.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Hello állapot hello virtuális gép beállítása túl**Generalized**. 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. Hello virtuális gép beolvasása. 

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```

5. Hello lemezkép-konfiguráció létrehozása.

    ```powershell
    $image = New-AzureRmImageConfig -Location $location -SourceVirtualMachineId $vm.ID 
    ```
6. Hello lemezkép létrehozása.

    ```powershell
    New-AzureRmImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 



## <a name="create-a-managed-image-of-a-vhd-in-powershell"></a>Hozzon létre egy felügyelt képre egy virtuális merevlemez a PowerShellben

Hozzon létre egy felügyelt képre az operációs rendszer általánosított példányát VHD használatával.


1.  Első lépésként állítsa be az általános paraméterek hello:

    ```powershell
    $rgName = "myResourceGroupName"
    $vmName = "myVM"
    $location = "West Central US" 
    $imageName = "yourImageName"
    $osVhdUri = "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd"
    ```
2. Step\deallocate hello virtuális gép.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Virtuális gép általánosítva, hello megjelölni.

    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized 
    ```
4.  Az operációs rendszer általánosított példányát VHD hello lemezkép létrehozása.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```


## <a name="create-a-managed-image-from-a-snapshot-using-powershell"></a>Hozzon létre egy felügyelt képre egy pillanatképből Powershell használatával

Egy felügyelt képre hello egy általánosított virtuális gép virtuális merevlemez egy pillanatképből is létrehozhat.

    
1. Bizonyos változókat létrehozni. 

    ```powershell
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. Hello pillanatkép beolvasása.

   ```powershell
   $snapshot = Get-AzureRmSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. Hello lemezkép-konfiguráció létrehozása.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. Hello lemezkép létrehozása.

    ```powershell
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 
    

## <a name="next-steps"></a>Következő lépések
- Most is [hozzon létre egy virtuális gép általánosítva hello felügyelt lemezképből](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).    

