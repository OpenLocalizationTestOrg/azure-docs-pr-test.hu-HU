---
title: "az Azure virtuális gépek aaaCommon PowerShell-parancsok |} Microsoft Docs"
description: "Általános PowerShell-parancsok tooget indítása létrehozása és kezelése a Windows virtuális gépek Azure-ban."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ba3839a2-f3d5-4e19-a5de-95bfb1c0e61e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: 3de9bd4d20259ced2c0aa8ef7a3f7d9520a071d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>A létrehozása és kezelése az Azure virtuális gépek közös PowerShell-parancsok

Ez a cikk ismertet néhány hello Azure PowerShell-parancsok, hogy toocreate használja, és kezelhetik a virtuális gépek Azure-előfizetése.  Segítségért parancssori kapcsolókkal és a lehetőségek részletes, használhat **Get-Help** *parancs*.

Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello Azure PowerShell legújabb verziójának telepítése, az előfizetés kiválasztásával és tooyour fiók bejelentkezés kapcsolatos információkat.

Ezek a változók akkor lehet hasznos, ha futtató több hello parancsok ebben a cikkben:

- $location - hello hello virtuális gép helyét. Használhat [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) toofind egy [földrajzi régió](https://azure.microsoft.com/regions/) meg működik.
- $myResourceGroup - hello virtuális gépet tartalmazó erőforráscsoport hello hello nevét.
- $myVM - hello hello virtuális gép nevét.

## <a name="create-a-vm"></a>Virtuális gép létrehozása

| Tevékenység | Parancs |
| ---- | ------- |
| A Virtuálisgép-konfiguráció létrehozása |$vm = [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) – VMName $myVM - VMSize "Standard_D1_v1"<BR></BR><BR></BR>hello a Virtuálisgép-konfigurációhoz hello VM használt toodefine vagy az update beállításait. hello konfiguráció inicializálása hello VM hello nevét és a [mérete](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Konfigurációs beállítások hozzáadása |$vm = [set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) - VM $vm-Windows - ComputerName $myVM-$cred - ProvisionVMAgent hitelesítőadat - EnableAutoUpdate<BR></BR><BR></BR>Operációs rendszer beállításait, beleértve a [hitelesítő adatok](https://technet.microsoft.com/library/hh849815.aspx) ad hozzá a korábban létrehozott új-AzureRmVMConfig toohello konfigurációs objektum. |
| Hálózati adapter hozzáadása |$vm = [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Add-AzureRmVMNetworkInterface) - VM $vm-azonosító $ hálózati adaptert. Azonosítója<BR></BR><BR></BR>Rendelkeznie kell egy virtuális Gépet egy [hálózati illesztő](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toocommunicate virtuális hálózatban. Is [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) tooretrieve egy meglévő hálózati illesztő objektum. |
| Adja meg a platformlemezkép |$vm = [set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) - VM $vm - PublisherName "publisher_name"-"publisher_offer" ajánlat - SKU "product_sku"-"legújabb" verzió<BR></BR><BR></BR>[Kép információk](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello konfigurációs objektum, amely a korábban létrehozott új-AzureRmVMConfig kerül. Ez a parancs által visszaadott hello objektum csak akkor használatos, ha úgy állítja be az operációs rendszer hello lemez toouse platformlemezkép. |
| Operációsrendszer-lemez toouse platformlemezkép beállítása |$vm = [set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk) - VM $vm-Name "myOSDisk" - VhdUri "http://mystore1.blob.core.windows.net/vhds/myOSDisk.vhd" - CreateOption FromImage<BR></BR><BR></BR>hello nevét hello operációsrendszer-lemez és a helyére [tárolási](../../storage/common/storage-powershell-guide-full.md) kerül toohello konfigurációs objektumot, amelyet korábban hozott létre. |
| Állítsa be az operációs rendszer lemez toouse általános lemezkép |$vm = $vm set-AzureRmVMOSDisk - VM – Name "myOSDisk" - SourceImageUri "https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk. {guid} .vhd"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage-Windows<BR></BR><BR></BR>hello nevét hello operációsrendszer-lemez, hello forráslemezkép hello helyét és hello szabad hely a [tárolási](../../storage/common/storage-powershell-guide-full.md) toohello konfigurációs objektum kerül. |
| Állítsa be az operációs rendszer lemez toouse speciális kép |$vm = $vm set-AzureRmVMOSDisk - VM-Name "myOSDisk" - VhdUri "http://mystore1.blob.core.windows.net/vhds/" - CreateOption csatolása - Windows |
| Virtuális gép létrehozása |[Új AzureRmVM]() - ResourceGroupName $myResourceGroup-hely $location - VM $vm<BR></BR><BR></BR>Az összes erőforrás hozott létre egy [erőforráscsoport](../../azure-resource-manager/powershell-azure-resource-manager.md). Ez a parancs futtatása előtt futtassa a New-AzureRmVMConfig, a Set-AzureRmVMOperatingSystem, a Set-AzureRmVMSourceImage, a Add-AzureRmVMNetworkInterface és a Set-AzureRmVMOSDisk. |

## <a name="get-information-about-vms"></a>Virtuális gépek adatainak beolvasása

| Tevékenység | Parancs |
| ---- | ------- |
| Lista virtuális gépek egy előfizetésben található |[Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) |
| Lista virtuális gépek egy erőforráscsoportban található |Get-AzureRmVM - ResourceGroupName $myResourceGroup<BR></BR><BR></BR>tooget az előfizetésében szereplő csoportok erőforrás listáját, használja a [Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresourcegroup). |
| Virtuális gép adatainak lekérése |Get-AzureRmVM - ResourceGroupName $myResourceGroup-$myVM neve |

## <a name="manage-vms"></a>Virtuális gépek kezelése
| Tevékenység | Parancs |
| --- | --- |
| Virtuális gép elindítása |[Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) - ResourceGroupName $myResourceGroup-$myVM neve |
| Virtuális gép leállítása |[STOP-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) - ResourceGroupName $myResourceGroup-$myVM neve |
| A virtuális gép újraindítása |[Újraindítás-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/restart-azurermvm) - ResourceGroupName $myResourceGroup-$myVM neve |
| Virtuális gép törlése |[Remove-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm) - ResourceGroupName $myResourceGroup-$myVM neve |
| A virtuális gépek generalize |[Set-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvm) - ResourceGroupName $myResourceGroup-Name $myVM – általános<BR></BR><BR></BR>Futtassa ezt a parancsot, mielőtt elindítaná a Save-AzureRmVMImage. |
| Virtuális gép rögzítése |[Mentés-AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) - ResourceGroupName $myResourceGroup – VMName $myVM - DestinationContainerName "myImageContainer" - VHDNamePrefix "myImagePrefix"-"C:\filepath\filename.json" elérési útja<BR></BR><BR></BR>A virtuális gépnek [elő, állítsa le és általánosítva](prepare-for-upload-vhd-image.md) használt toobe toocreate lemezkép. Ez a parancs futtatása előtt futtassa a Set-AzureRmVm. |
| A virtuális gép frissítése |[Frissítés-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) - ResourceGroupName $myResourceGroup - VM $vm<BR></BR><BR></BR>Első hello aktuális Virtuálisgép-konfiguráció használata a Get-AzureRmVM hello Virtuálisgép-objektum a konfigurációs beállításokat módosítaná, és futtassa a parancsot. |
| Adja hozzá a adatok lemez tooa méretű VM |[Adja hozzá AzureRmVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk) - VM $vm-neve "myDataDisk" - VhdUri "https://mystore1.blob.core.windows.net/vhds/myDataDisk.vhd" - LUN #-gyorsítótárazás ReadWrite - DiskSizeinGB # - CreateOption üres<BR></BR><BR></BR>Használja a Get-AzureRmVM tooget hello Virtuálisgép-objektum. Adja meg a hello logikai egységek számát és a hello hello lemez méretét. Futtassa az Update-AzureRmVM tooapply hello konfigurációs módosítások toohello virtuális gép. hello lemez hozzáadott nincs inicializálva. |
| Adatlemez eltávolítása egy virtuális gépből |[Remove-AzureRmVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmdatadisk) - VM $vm-Name "myDataDisk"<BR></BR><BR></BR>Használja a Get-AzureRmVM tooget hello Virtuálisgép-objektum. Futtassa az Update-AzureRmVM tooapply hello konfigurációs módosítások toohello virtuális gép. |
| Egy bővítmény tooa VM hozzáadása |[Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) - ResourceGroupName $myResourceGroup-hely $location - VMName $myVM-Name "bővítménynév"-közzétevő "publisherName"-"extensionType" típus - TypeHandlerVersion "#. #"-beállítások $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>A parancs futtatásához a megfelelő hello [konfigurációs adatok](template-description.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#extensions) , amelyet az tooinstall hello bővítmény. |
| Virtuálisgép-bővítmény eltávolítása |[Remove-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmextension) - ResourceGroupName $myResourceGroup-Name "bővítménynév" – VMName $myVM |

## <a name="next-steps"></a>Következő lépések
* Lásd: hello alapvető lépéseit a virtuális gép létrehozása [erőforrás-kezelő és a PowerShell használatával Windows virtuális gép létrehozása](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

