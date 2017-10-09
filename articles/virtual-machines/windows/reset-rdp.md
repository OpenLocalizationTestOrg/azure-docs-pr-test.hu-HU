---
title: "aaaReset jelszó vagy a távoli asztal konfigurálásának a virtuális gép Windows hello |} Microsoft Docs"
description: "Ismerje meg, hogyan tooreset egy fiók jelszavát, vagy egy Windows virtuális gép használata a távoli asztali szolgáltatások hello Azure-portálon vagy az Azure PowerShell."
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
ms.openlocfilehash: 5258df7196621f0adb50debd08dd248922a966de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-hello-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Hogyan tooreset hello a távoli asztal szolgáltatás vagy egy Windows virtuális gépre bejelentkezési jelszava
Ha nem sikerül tooa Windows rendszerű virtuális gép (VM), hello helyi rendszergazda jelszavát, vagy állítsa hello távoli asztal szolgáltatás konfigurációját. Az Azure PowerShell tooreset hello jelszó vagy hello Azure portál vagy hello VM hozzáférési bővítményét is használhatja. Ha a PowerShell használata esetén győződjön meg arról, hogy rendelkezik-e hello [legújabb PowerShell-modul telepítve és konfigurálva](/powershell/azure/overview) és tooyour Azure-előfizetés van bejelentkezve. Emellett [hello klasszikus telepítési modellel létrehozott virtuális gépek a következő lépések](reset-rdp.md).

## <a name="ways-tooreset-configuration-or-credentials"></a>Többféleképpen tooreset konfigurációját vagy a hitelesítő adatok
Alaphelyzetbe állíthatja a távoli asztali szolgáltatások és a hitelesítő adatokat néhány más-más módon igényeitől függően:

- [Hello Azure portál segítségével vissza](#azure-portal)
- [Alaphelyzetbe állítja az Azure PowerShell használatával](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure Portal
tooexpand hello portál menüjében kattintson a három hello sávok hello bal felső sarokban található, és kattintson a **virtuális gépek**:

![Tallózással keresse meg az Azure virtuális gép](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-hello-local-administrator-account-password"></a>**Hello helyi rendszergazdai fiók jelszavának visszaállítása**

Válassza ki a Windows virtuális gépet, majd kattintson az **támogatási + hibaelhárítás** > **jelszó-átállítási**. hello jelszó alaphelyzetbe állítása panelen jelennek meg:

![Jelszó alaphelyzetbe állítása lap](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

Adja meg a hello felhasználónevet és egy új jelszót, majd kattintson a **frissítés**. Próbáljon újra kapcsolódni a virtuális gép tooyour.

### <a name="reset-hello-remote-desktop-service-configuration"></a>**Állítsa hello távoli asztal szolgáltatás konfigurációját**

Válassza ki a Windows virtuális gépet, majd kattintson az **támogatási + hibaelhárítás** > **jelszó-átállítási**. hello jelszó alaphelyzetbe állítása panel jelenik meg. 

![RDP-konfigurációjának visszaállítása](./media/reset-rdp/Portal-RM-RDP-Reset.png)

Válassza ki **alaphelyzetbe állítása konfigurációs csak** hello legördülő menüből, majd kattintson a **frissítés**. Próbáljon újra kapcsolódni a virtuális gép tooyour.


## <a name="vmaccess-extension-and-powershell"></a>VMAccess bővítmény és a PowerShell
Győződjön meg arról, hogy rendelkezik-e hello [legújabb PowerShell-modul telepítve és konfigurálva](/powershell/azure/overview) és jelentkezik be Azure előfizetés hello tooyour `Login-AzureRmAccount` parancsmag.

### <a name="reset-hello-local-administrator-account-password"></a>**Hello helyi rendszergazdai fiók jelszavának visszaállítása**
Alaphelyzetbe állítása hello rendszergazdai jelszót vagy a felhasználó nevét hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-parancsmagot. Hozzon létre a fiók hitelesítő adatait az alábbiak szerint:

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> Ha neve hello aktuális helyi rendszergazdai fiók eltér a virtuális gépen, a VMAccess bővítmény hello átnevezi hello helyi rendszergazdai fiók, rendeli hozzá a megadott jelszó toothat fiók és kibocsát egy távoli asztali kijelentkezési esemény. A virtuális gépen hello helyi rendszergazdai fiók le van tiltva, ha a VMAccess bővítmény hello lehetővé teszi.

a következő példa frissítések hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup` toohello megadott hitelesítő adatokat.

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-hello-remote-desktop-service-configuration"></a>**Állítsa hello távoli asztal szolgáltatás konfigurációját**
Alaphelyzetbe állítja a távelérés tooyour VM a hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-parancsmagot. hello alábbi példa visszaállítja hello hozzáférési bővítmény nevű `myVMAccess` hello nevű virtuális gép a `myVM` a hello `myResourceGroup` erőforráscsoport:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> Bármikor a virtuális gép is van csak egyetlen virtuális gép hozzáférés ügynök. sikeresen megtörtént, hello tooset hello VM ügynök tulajdonságait `-ForceRerun` beállítás használható. Használata esetén `-ForceRerun`, ellenőrizze, hogy toouse hello hello VM ügynök az előző parancs alkalmazott ugyanazt a nevet.

Ha még nem sikerül távolról tooyour virtuális gépet, tekintse meg a következő további lépéseket tootry [hibaelhárítása távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gép](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


## <a name="next-steps"></a>Következő lépések
Ha hello Azure virtuális gép hozzáférési bővítmény nem válaszol, és nem tooreset hello jelszót, akkor [hello helyi Windows jelszó-átállítási offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ez a módszer egy speciális folyamat, és szükségessé teszi tooconnect hello virtuális merevlemezt, hello problematikus VM tooanother virtuális gép. Kövesse a jelen cikkben leírt először hello lépéseket, és csak a hello offline jelszó alaphelyzetbe állítása módszer utolsó lehetőségként kísérlet.

[Az Azure Virtuálisgép-bővítmények és szolgáltatások](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Csatlakozás tooan Azure virtuális gép RDP- vagy SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gép hibakeresése](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

