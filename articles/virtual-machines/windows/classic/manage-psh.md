---
title: "aaaManage a virtuális gépek Azure PowerShell használatával |} Microsoft Docs"
description: "Ismerje meg, hogy használható-e tooautomate feladatokat a virtuális gépek kezelése a parancsok."
services: virtual-machines-windows
documentationcenter: windows
author: singhkays
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 7cdf9bd3-6578-4069-8a95-e8585f04a394
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2016
ms.author: kasing
ms.openlocfilehash: e4ca6f098519243a321eac98b6692790fe18c22c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>A virtuális gépek kezelése az Azure PowerShell-lel
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Általános PowerShell-parancsok hello Resource Manager modellt használja, lásd: [Itt](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

A virtuális gépek minden nap toomanage teszi számos feladatot Azure PowerShell-parancsmagok segítségével automatizálható. Ez a cikk lehetővé teszi az Példaparancsok egyszerűbb feladatokat, és hivatkozások tooarticles, amelyek megjelenítik az összetettebb feladatok hello parancsok.

> [!NOTE]
> Ha még nem telepített és konfigurált Azure PowerShell, még hello cikkben szereplő útmutatást kaphat [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
> 
> 

## <a name="how-toouse-hello-example-commands"></a>Hogyan toouse hello Példaparancsok
Néhány hello hello szöveg, amely a környezetnek megfelelő szöveggel parancsok tooreplace lesz szüksége. hello < és > szimbólumok jelzi a szöveg tooreplace van szüksége. Hello szöveg cseréjekor hello szimbólumok eltávolítása, de hagyja hello ajánlat jelek helyen.

## <a name="get-a-vm"></a>A virtuális gép beolvasása
Ez a gyakran használt egyszerű feladat. Használja a virtuális gépek tooget információt, feladatokat a virtuális gép vagy kimeneti toostore lekérni egy változóban.

tooget információ a virtuális gép, hello futtassa ezt a parancsot, minden hello idézőjelben, beleértve a hello < és > karakter cseréje:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Futtassa egy $vm változóban kimeneti toostore hello:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-tooa-windows-based-vm"></a>Windows-alapú virtuális gép tooa bejelentkezés
Futtassa az alábbi parancsokat:

> [!NOTE]
> Hello virtuális gép és a felhőszolgáltatás neve lekérheti hello hello megjelenítését **Get-AzureVM** parancsot.
> 
> $svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "< meghajtó és mappa helye toostore letöltött hello RDP-fájlt, például: c:\temp >" $localFile = $localPath + "\" $vmname +".rdp"Get-AzureRemoteDesktopFile - Szolgáltatásnév $svcName-Name $vmName - LocalPath $localFile-elindítása
> 
> 

## <a name="stop-a-vm"></a>Virtuális gép leállítása
Futtassa ezt a parancsot:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> Használja a paraméter tookeep hello virtuális a IP-cím (VIP) a felhő hello szolgáltatást, abban az esetben, ha az utolsó virtuális gép, amely a felhőszolgáltatás a hello. <br><br> Ha hello StayProvisioned paraméter használata esetén a fogjuk továbbra is számlázni hello virtuális gép.
> 
> 

## <a name="start-a-vm"></a>Virtuális gép elindítása
Futtassa ezt a parancsot:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Adatlemez csatolása
Ez a feladat néhány lépést igényel. Először használja a hello x Add-AzureDataDisk x parancsmag tooadd hello toohello $vm objektumát. Ezután **frissítés-AzureVM** hello VM parancsmag tooupdate hello konfigurációját.

Meg kell toodecide e tooattach új lemez vagy egy tartalmazó adatokat. Egy új lemez hello parancs hello .vhd fájlt hoz létre, és csatolja.

egy új lemezt tooattach futtassa ezt a parancsot:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

egy meglévő adatlemez tooattach futtassa ezt a parancsot:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

a blob Storage tárolóban, meglévő .vhd fájl tooattach adatlemezek futtassa ezt a parancsot:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Windows-alapú virtuális gép létrehozása
új Windows-alapú virtuális gépet az Azure használatát hello utasításait toocreate [Azure PowerShell használata toocreate és előre konfigurálása a Windows-alapú virtuális gépek](create-powershell.md). Ez a témakör lépéseit hello létrehozása az Azure PowerShell paranccsal állítsa be, amely végigvezeti létrehoz egy Windows-alapú virtuális Gépet, amely megfelelően előre beállíthatók:

* Az Active Directory tartományi tagságát.
* A további lemezek.
* Elosztott terhelésű meglévő tagjaként beállítása.
* Statikus IP-címmel.

