---
title: "Alaphelyzetbe állítja a jelszót, vagy a távoli asztali konfigurálása a Windows virtuális gép |} Microsoft Docs"
description: "Útmutató új egy fiók jelszavát, vagy a távoli asztali szolgáltatások a Windows virtuális gép az Azure portálon vagy az Azure PowerShell használatával."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 2e002e3f336422b8fa1eceece889cd083e355a68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>A távoli asztal szolgáltatás vagy egy Windows virtuális gépre a bejelentkezés jelszó alaphelyzetbe állítása
Ha nem tud csatlakozni a Windows rendszerű virtuális gép (VM), a helyi rendszergazda jelszavát, vagy állítsa a távoli asztal szolgáltatás konfigurációját. Használhatja az Azure-portálon vagy a virtuális gép hozzáférési bővítményét az Azure PowerShell a jelszó alaphelyzetbe állításához. Ha a PowerShell használata esetén győződjön meg arról, hogy rendelkezik a [legújabb PowerShell-modul telepítve és konfigurálva](/powershell/azure/overview) és az Azure-előfizetéssel jelentkezik be. Emellett [hajtsa végre ezeket a lépéseket a klasszikus telepítési modellel létrehozott virtuális gépek](reset-rdp.md).

## <a name="ways-to-reset-configuration-or-credentials"></a>Konfiguráció vagy a hitelesítő adatok alaphelyzetbe módjai
Alaphelyzetbe állíthatja a távoli asztali szolgáltatások és a hitelesítő adatokat néhány más-más módon igényeitől függően:

- [Az Azure portál segítségével vissza](#azure-portal)
- [Alaphelyzetbe állítja az Azure PowerShell használatával](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure Portal
Bontsa ki a portál menüjében, kattintson a három sávok bal felső sarokban, és kattintson a **virtuális gépek**:

![Tallózással keresse meg az Azure virtuális gép](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-the-local-administrator-account-password"></a>**A helyi rendszergazdai fiók jelszavának visszaállítása**

Válassza ki a Windows virtuális gépet, majd kattintson az **támogatási + hibaelhárítás** > **jelszó-átállítási**. A jelszó alaphelyzetbe állítása panelen jelennek meg:

![Jelszó alaphelyzetbe állítása lap](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

Adja meg a felhasználónevet és egy új jelszót, majd kattintson a **frissítés**. Próbáljon újra kapcsolódni a virtuális Gépet.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Állítsa a távoli asztal szolgáltatás konfigurációját.**

Válassza ki a Windows virtuális gépet, majd kattintson az **támogatási + hibaelhárítás** > **jelszó-átállítási**. A jelszó alaphelyzetbe állítása panel jelenik meg. 

![RDP-konfigurációjának visszaállítása](./media/reset-rdp/Portal-RM-RDP-Reset.png)

Válassza ki **alaphelyzetbe állítása konfigurációs csak** a legördülő menüből, majd kattintson a **frissítés**. Próbáljon újra kapcsolódni a virtuális Gépet.


## <a name="vmaccess-extension-and-powershell"></a>VMAccess bővítmény és a PowerShell
Győződjön meg arról, hogy rendelkezik a [legújabb PowerShell-modul telepítve és konfigurálva](/powershell/azure/overview) van bejelentkezve az Azure-előfizetése és a `Login-AzureRmAccount` parancsmag.

### <a name="reset-the-local-administrator-account-password"></a>**A helyi rendszergazdai fiók jelszavának visszaállítása**
Alaphelyzetbe állítja a rendszergazdai jelszót vagy a felhasználó nevét a [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-parancsmagot. Hozzon létre a fiók hitelesítő adatait az alábbiak szerint:

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> Ha a virtuális Gépet a neve eltér az aktuális helyi rendszergazdai fiók, a VMAccess bővítmény átnevezi a helyi rendszergazdai fiók, rendeli hozzá a megadott jelszó ezekhez a fiókokhoz, és kibocsát egy távoli asztali kijelentkezési esemény. Ha a helyi rendszergazdai fiókhoz az a virtuális gép le van tiltva, a VMAccess bővítmény lehetővé teszi.

Az alábbi példa frissíti a virtuális gép nevű `myVM` az erőforráscsoport neve `myResourceGroup` megadott hitelesítő adatokhoz.

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-the-remote-desktop-service-configuration"></a>**Állítsa a távoli asztal szolgáltatás konfigurációját.**
A virtuális Gépet, a távelérés alaphelyzetbe állításával a [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-parancsmagot. Az alábbi példa alaphelyzetbe állítja a hozzáférés bővítmény nevű `myVMAccess` nevű virtuális gépen `myVM` a a `myResourceGroup` erőforráscsoport:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> Bármikor a virtuális gép is van csak egyetlen virtuális gép hozzáférés ügynök. A virtuális gép hozzáférés ügynök tulajdonságainak beállítása sikeres legyen, a `-ForceRerun` beállítás használható. Használata esetén `-ForceRerun`, ügyeljen arra, hogy a Virtuálisgép-hozzáférés ügynök az előző parancs alkalmazott ugyanazt a nevet használja.

Ha még nem lehet csatlakozni távolról a virtuális gép, tekintse meg a további lépéseket [távoli asztali kapcsolatainak hibaelhárítása a Windows-alapú Azure virtuális gépekhez](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


## <a name="next-steps"></a>Következő lépések
Ha az Azure virtuális gép hozzáférési bővítmény nem válaszol, és nem tudja alaphelyzetbe állítani a jelszót, akkor [a helyi Windows-jelszó alaphelyzetbe állítása offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ez a módszer egy speciális folyamat, és megköveteli, hogy a virtuális merevlemez a problematikus VM kapcsolódjon egy másik virtuális gép. Kövesse a jelen cikkben leírt első lépéseket, és csak kísérlet a kapcsolat nélküli jelszó alaphelyzetbe állítása módszer utolsó lehetőségként.

[Az Azure Virtuálisgép-bővítmények és szolgáltatások](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Egy Azure virtuális gép RDP- vagy SSH kapcsolódni](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[A Windows-alapú Azure virtuális géphez a távoli asztali kapcsolatok hibáinak elhárítása](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

