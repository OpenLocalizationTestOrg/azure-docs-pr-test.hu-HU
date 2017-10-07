---
title: "aaaReset hello jelszó vagy egy Windows Azure-ban a távoli asztal konfigurálásának |} Microsoft Docs"
description: "Ismerje meg, hogyan egy fiók jelszavát, vagy a távoli asztali szolgáltatások a Windows virtuális gép létrehozott hello klasszikus telepítési modell segítségével tooreset hello Azure-portálon vagy az Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: 1721a91fc6c89b46df74e76dfcf918b1c4c77a4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-hello-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-hello-classic-deployment-model"></a>Hogyan tooreset hello távoli asztal szolgáltatás vagy egy Windows virtuális gépre bejelentkezési jelszava létrehozott hello klasszikus telepítési modell
> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Emellett [hello Resource Manager üzembe helyezési modellben létrehozott virtuális gépek a következő lépések](../reset-rdp.md).

Ha nem sikerül tooa Windows rendszerű virtuális gép (VM), hello helyi rendszergazda jelszavát, vagy állítsa hello távoli asztal szolgáltatás konfigurációját. Az Azure PowerShell tooreset hello jelszó vagy hello Azure portál vagy hello VM hozzáférési bővítményét is használhatja.

## <a name="ways-tooreset-configuration-or-credentials"></a>Többféleképpen tooreset konfigurációját vagy a hitelesítő adatok
Alaphelyzetbe állíthatja a távoli asztali szolgáltatások és a hitelesítő adatokat néhány más-más módon igényeitől függően:

- [Hello Azure portál segítségével vissza](#azure-portal)
- [Alaphelyzetbe állítja az Azure PowerShell használatával](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure Portal
Használhatja a hello [Azure-portálon](https://portal.azure.com) tooreset hello a távoli asztal szolgáltatás. tooexpand hello portál menüjében kattintson a három hello sávok hello bal felső sarokban található, és kattintson a **virtuális gépek (klasszikus)**:

![Tallózással keresse meg az Azure virtuális gép](./media/reset-rdp/Portal-Select-Classic-VM.png)

Válassza ki a Windows virtuális gépet, és kattintson a **távoli alaphelyzetbe állítása...** . hello alábbi párbeszédpanel megjelenik tooreset hello távoli asztal konfigurálásának:

![RDP-konfiguráció lapon alaphelyzetbe állítása](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

Hello felhasználónév és jelszó hello helyi rendszergazdai fiók is alaphelyzetbe állíthatja. A virtuális Gépet, kattintson **támogatási + hibaelhárítás** > **jelszó-átállítási**. hello jelszó alaphelyzetbe állítása panelen jelennek meg:

![Jelszó alaphelyzetbe állítása lap](./media/reset-rdp/Portal-PW-Reset-Windows.png)

Miután megadta a hello új felhasználónevet és jelszót, kattintson a **mentése**.

## <a name="vmaccess-extension-and-powershell"></a>VMAccess bővítmény és a PowerShell
Győződjön meg arról, hogy hello hello virtuális gépen a Virtuálisgép-ügynök telepítve van. hello VMAccess bővítmény nem szükséges toobe telepítve előtt is használhatja, mindaddig, amíg hello Virtuálisgép-ügynök érhető el. Győződjön meg arról, hogy hello Virtuálisgép-ügynök már telepítve van a következő parancs hello segítségével. (Helyére "myCloudService" és "myVM" hello nevét a felhőszolgáltatás és a virtuális Gépet, illetve. Ezeket a neveket is megtudhat futtatásával `Get-AzureVM` paraméter nélkül.)

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

Ha hello **write-host** parancs megjeleníti **igaz**, hello Virtuálisgép-ügynök telepítve van. Ha megjeleníti **hamis**, hello utasításokat, és a hivatkozás toohello, töltse le a hello [ügynök és Virtuálisgép-bővítmények – 2. rész](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blogbejegyzést.

Ha a virtuális gép hello hello portál használatával hozott létre, ellenőrizze, hogy `$vm.GetInstance().ProvisionGuestAgent` adja vissza **igaz**. Ha nem, ez a parancs használatával állíthatja be:

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

Ez a parancs megakadályozza, hogy a következő hiba, ha a számítógépén hello hello **Set-AzureVMExtension** hello lépések parancsot: "Rendelkezés Vendégügynök engedélyezni kell hello Virtuálisgép-objektum infrastruktúra-szolgáltatási virtuális gép hozzáférési bővítmény beállítása előtt."

### <a name="reset-hello-local-administrator-account-password"></a>**Hello helyi rendszergazdai fiók jelszavának visszaállítása**
A bejelentkezési hitelesítő adatok létrehozása a hello aktuális helyi rendszergazda fiók nevét és egy új jelszót, és futtassa a hello `Set-AzureVMAccessExtension` az alábbiak szerint.

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

Ha neve eltér a hello jelenlegi fiókot, hello VMAccess bővítmény átnevezi a helyi rendszergazdai fiók hello hello toothat fiókot hozzárendeli és kijelentkezési a távoli asztal kiállított. Ha hello helyi rendszergazdai fiók le van tiltva, a VMAccess bővítmény hello lehetővé teszi.

Ezek a parancsok is alaphelyzetbe állíthatja a hello távoli asztal szolgáltatás konfigurációját.

### <a name="reset-hello-remote-desktop-service-configuration"></a>**Állítsa hello távoli asztal szolgáltatás konfigurációját**
tooreset hello távoli asztal szolgáltatás konfigurációját, futtassa a következő parancs hello:

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

hello VMAccess bővítmény hello virtuális gépen két parancsot futtatja:

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

Ez a parancs lehetővé teszi, hogy a hello beépített Windows tűzfal csoport, amely lehetővé teszi a távoli asztal bejövő forgalmat, amely a 3389-es TCP-portot használja.

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

Ez a parancs beállítja a hello fDenyTSConnections beállításjegyzékbeli érték too0, amely lehetővé teszi a távoli asztali kapcsolatok.

## <a name="next-steps"></a>Következő lépések
Ha hello Azure virtuális gép hozzáférési bővítmény nem válaszol, és nem tooreset hello jelszót, akkor [hello helyi Windows jelszó-átállítási offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ez a módszer egy speciális folyamat, és szükségessé teszi tooconnect hello virtuális merevlemezt, hello problematikus VM tooanother virtuális gép. Kövesse a jelen cikkben leírt először hello lépéseket, és csak a hello offline jelszó alaphelyzetbe állítása módszer utolsó lehetőségként kísérlet.

[Az Azure Virtuálisgép-bővítmények és szolgáltatások](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Csatlakozás tooan Azure virtuális gép RDP- vagy SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gép hibakeresése](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

