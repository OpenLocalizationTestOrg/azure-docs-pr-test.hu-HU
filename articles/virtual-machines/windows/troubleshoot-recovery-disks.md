---
title: "a virtuális gép az Azure PowerShell hibaelhárítás Windows aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot Windows virtuális gép állít ki az Azure-ban csatlakozzon az operációs rendszer hello lemez tooa helyreállítási virtuális Gépet az Azure PowerShell"
services: virtual-machines-windows
documentationCenter: 
authors: genlin
manager: timlt
editor: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 7a6a76f64824fe5d06dc4286cb1d87ab8bb794e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-azure-powershell"></a>A Windows virtuális gépek hello OS lemez tooa helyreállítási virtuális Gépet az Azure PowerShell csatolásával hibaelhárítása
Ha a Windows rendszerű virtuális gép (VM) az Azure-ban rendszerindító vagy a lemez hibát tapasztal, szükség lehet a tooperform hibaelhárítási hello virtuális merevlemezen magát. Ilyenek például a sikertelen frissítés, amely megakadályozza hello méretű képes tooboot sikeresen lenne. Ez a következő cikket: részletek hogyan toouse Azure PowerShell tooconnect a virtuális merevlemez tooanother Windows virtuális gép toofix hibáit, majd hozza létre az eredeti virtuális gép.


## <a name="recovery-process-overview"></a>Helyreállítási folyamat áttekintése
hibaelhárítási folyamatának hello a következőképpen történik:

1. Törölje a hello VM észlelt problémák, hello virtuális merevlemezek tartása.
2. Csatolja, és a csatlakoztatási hello virtuális merevlemez tooanother Windows virtuális gép hibaelhárítási célból.
3. Csatlakoztassa a VM hibaelhárítási toohello. Szerkesztheti a fájlokat, vagy futtassa az olyan eszközöket toofix problémák hello eredeti virtuális merevlemez.
4. Válassza le a lemezképet, és válassza le hello virtuális merevlemezt a virtuális gép hibakeresési hello.
5. Hello eredeti virtuális merevlemez virtuális gép létrehozása.

Győződjön meg arról, hogy rendelkezik [legújabb Azure PowerShell hello](/powershell/azure/overview) telepítve és tooyour előfizetés bejelentkezve:

```powershell
Login-AzureRMAccount
```

Hello alábbi példák cserélje le a paraméter nevét a saját értékeit. Példa paraméter nevek a következők `myResourceGroup`, `mystorageaccount`, és `myVM`.


## <a name="determine-boot-issues"></a>Határozza meg a rendszerindítási problémák
Megtekintheti a képernyőkép a virtuális gép az Azure-ban toohelp a rendszerindítási problémák elhárításához. Ezen a képernyőfelvételen látható segítségével azonosíthatók, ezért a virtuális gépek tooboot sikertelen lesz. hello alábbi példa lekérdezi hello képernyőfelvétel a hello nevű Windows virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

Tekintse át a hello képernyőkép toodetermine miért hello VM tooboot sikertelen. Megjegyzés: a vonatkozó hibaüzeneteket vagy a megadott hibakódok.


## <a name="view-existing-virtual-hard-disk-details"></a>Meglévő virtuális merevlemez részleteinek megtekintése
A virtuális merevlemez tooanother VM csatolhat, meg kell tooidentify hello neve hello virtuális merevlemez (VHD).

hello alábbi példa lekérdezi hello nevű virtuális gép adatai `myVM` nevű hello erőforráscsoportban `myResourceGroup`:

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Keressen `Vhd URI` belül hello `StorageProfile` parancs megelőző hello szakasza hello kimenetből. hello következő csonkolt példa kimenet látható hello `Vhd URI` hello kódblokk hello vége felé:

```powershell
RequestId                     : 8a134642-2f01-4e08-bb12-d89b5b81a0a0
StatusCode                    : OK
ResourceGroupName             : myResourceGroup
Id                            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
Name                          : myVM
Type                          : Microsoft.Compute/virtualMachines
...
StorageProfile                :
  ImageReference              :
    Publisher                 : MicrosoftWindowsServer
    Offer                     : WindowsServer
    Sku                       : 2016-Datacenter
    Version                   : latest
  OsDisk                      :
    OsType                    : Windows
    Name                      : myVM
    Vhd                       :
      Uri                     : https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    Caching                   : ReadWrite
    CreateOption              : FromImage
```


## <a name="delete-existing-vm"></a>Meglévő virtuális gép törlése
A virtuális merevlemezek és a virtuális gépek az Azure-erőforrások két különböző típusa. A virtuális merevlemez hello operációs rendszert illeti, alkalmazások és konfigurációk tárolására. hello virtuális gépért csak metaadatokat, amelyek hello vagy a hely határozza meg, és hivatkozik arra az erőforrások, például egy virtuális merevlemezt vagy virtuális hálózati kártya (NIC). Minden virtuális merevlemez rendelkezik-e létrehozásakor kell csatolni a címbérlet tooa virtuális gép. Bár adatlemezt csatolni, és leválasztott hello virtuális gép futása közben is, hello operációsrendszer-lemez nem választható le, kivéve, ha a virtuális gép erőforrásához hello törlődik. hello bérleti tooassociate hello OS lemezt a virtuális gépek továbbra is, akkor is, ha ezt a virtuális Gépet felszabadított és leállított állapotban van.

első lépés toorecover hello a virtuális gép toodelete hello VM erőforrás magát. Virtuális gép törlése hello hello virtuális merevlemezek elhagyja a tárfiókban lévő. Hello a virtuális gép törlődik, miután hello virtuális merevlemez tooanother VM tootroubleshoot csatolja, és hárítsa el a hello hibákat.

a következő példa törlések hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportból `myResourceGroup`:

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Várjon, amíg hello virtuális gép törlése előtt hello virtuális merevlemez tooanother virtuális gép csatlakoztatása befejeződött. hello virtuális merevlemezt, amely társítja azt hello VM hello bérlete hello virtuális merevlemez tooanother VM előtt kiadott toobe kell.


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a>Meglévő virtuális merevlemez tooanother VM csatolása
A hello ezután néhány lépést, akkor egy másik virtuális gép hibaelhárítási célból. Hello VM toobrowse hibaelhárítási meglévő virtuális merevlemez toothis csatolja, és hello lemez tartalmának szerkesztéséhez. Ez a folyamat toocorrect lehetővé teszi bármely konfigurációs hibák vagy a felülvizsgálati további alkalmazások vagy a rendszer naplófájljait, például. Válasszon, vagy hozzon létre egy másik virtuális gép toouse hibaelhárítási célból.

Hello meglévő virtuálismerevlemez-fájl csatolása, adjon meg hello URL-cím toohello lemezt előző hello beolvasott `Get-AzureRmVM` parancsot. hello alábbi példa csatolja egy meglévő virtuális merevlemez toohello nevű virtuális gép hibakeresési `myVMRecovery` nevű hello erőforráscsoportban `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> Lemez hozzáadása olyan toospecify hello hello lemez méretét. Mivel azt a meglévő lemez csatolása, hello `-DiskSizeInGB` megadott `$null`. Ez az érték biztosítja, hello adatlemez megfelelően csatlakozik, és hello nélkül kell toodetermine hello adatlemez igaz méretét.


## <a name="mount-hello-attached-data-disk"></a>Hello csatolt adatlemez csatlakoztatása

1. RDP tooyour hibaelhárítási hello megfelelő hitelesítő adatokat használó virtuális gépek. hello alábbi példa letölti hello RDP-kapcsolatfájl hello nevű virtuális gép `myVMRecovery` nevű hello erőforráscsoportban `myResourceGroup`, és letölti, túl`C:\Users\ops\Documents`"

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. hello adatlemez automatikusan észlelt és csatolva. Megtekintheti a csatlakoztatott kötetek toodetermine hello meghajtóbetűjelet hello listáját az alábbiak szerint:

    ```powershell
    Get-Disk
    ```

    a következő egy példa a kimenetre hello jeleníti meg, a csatlakoztatott lemez virtuális merevlemez hello **2**. (Is `Get-Volume` tooview hello meghajtóbetűjel):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a>Az eredeti virtuális merevlemez kapcsolatos problémák megoldása
Hello meglévő virtuális merevlemezzel csatlakoztatva is képes lemezvizsgálatok elvégzésére bármely karbantartási és hibaelhárítási lépéseket, igény szerint. Egyszer foglalkoztak hello problémák, folytassa a következő lépéseket hello.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Válassza le a lemezképet, és válassza le az eredeti virtuális merevlemez
Ha a hibák fakadó problémák megoldásával válassza le, és hello meglévő virtuális merevlemez válassza le a hibaelhárítási virtuális Gépet. Nem használhat a virtuális merevlemez más virtuális gép hello virtuális merevlemez toohello hibaelhárítás a virtuális gép csatlakoztatása hello bérleti kiadásáig.

1. Az az RDP-munkameneten belül leválasztása hello adatlemez a helyreállítási virtuális gép. Előző kell hello lemez szám hello `Get-Disk` parancsmag. Ezt követően `Set-Disk` tooset hello lemez offline állapotúként:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Győződjön meg róla, hello lemez most értéke offline használatával `Get-Disk` újra. hello következő egy példa a kimenetre látható rendszer hello lemez most már offline módúra állítja:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. Az RDP-munkamenetbe való kilépéshez. Az Azure PowerShell-munkamenetben távolítsa el a hello virtuális merevlemezt a virtuális gép hibakeresési hello.

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a>Virtuális gép eredeti merevlemez létrehozása
toocreate egy virtuális Gépet az eredeti virtuális merevlemez használata [Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). hello tényleges JSON-sablon jelenleg a következő hivatkozás hello:

- https://RAW.githubusercontent.com/Azure/Azure-quickstart-Templates/Master/201-VM-specialized-VHD-existing-vnet/azuredeploy.JSON

hello sablon létrehozni meglévő virtuális hálózatban, hello virtuális merevlemez URL-címet a hello segítségével központilag telepíti egy virtuális gép korábbi parancsot. hello következő példa telepíti hello sablon toohello nevű erőforráscsoport `myResourceGroup`:

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

Válasz hello hello sablont, például a virtuális gép neve, típusa és Virtuálisgép-méretet megadását kéri. Hello `osDiskVhdUri` azonos az előzőleg használt hello VM hibaelhárítási meglévő virtuális merevlemez toohello csatlakoztatása hello.


## <a name="re-enable-boot-diagnostics"></a>Engedélyezze újra a rendszerindítási diagnosztika

Hello létező virtuális merevlemezt a virtuális Gépet hoz létre, ha rendszerindítási diagnosztika automatikusan nem lehet engedélyezni. hello következő példa engedélyezi hello hello nevű virtuális gép diagnosztikai kiterjesztés `myVMDeployed` nevű hello erőforráscsoportban `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a>Következő lépések
Ha a Kapcsolódás a virtuális gép tooyour problémát tapasztal, tekintse meg [hibaelhárítása RDP-kapcsolatok tooan Azure virtuális gép](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). A virtuális gépen futó alkalmazások elérésével problémákkal kapcsolatban lásd: [alkalmazás csatlakozási problémák a Windows virtuális gép](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Erőforrás-kezelő használatával kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
