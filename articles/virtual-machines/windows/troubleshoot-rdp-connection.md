---
title: "aaaCannot csatlakozás RDP tooa Windows Azure-ban |} Microsoft Docs"
description: "Ha nem kapcsolódnak a tooyour Windows rendszerű virtuális gép távoli asztali kapcsolat segítségével a problémák elhárítása"
keywords: "Távoli asztali hiba, a távoli asztali kapcsolat hiba, nem lehet kapcsolódni a tooVM, távoli asztal – hibaelhárítás"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 0d740f8e-98b8-4e55-bb02-520f604f5b18
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: support-article
ms.date: 07/25/2017
ms.author: genli
ms.openlocfilehash: bbb36136e7a4b187fe8deea621d2b8d46d8aa102
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-remote-desktop-connections-tooan-azure-virtual-machine"></a>Távoli asztali kapcsolatok tooan Azure virtuális gép hibaelhárítása
hello Remote Desktop Protocol (RDP) kapcsolat tooyour Windows-alapú Azure virtuális gép (VM) különböző okokból, így nem tooaccess meghiúsulhatnak, a virtuális Gépet. a távoli asztal szolgáltatás hello VM, hello hálózati kapcsolat vagy hello távoli asztali ügyfél a gazdaszámítógépen hello hello probléma lehet. Ez a cikk végigvezeti Önt néhány hello leggyakrabban használt módszerek tooresolve RDP kapcsolódási problémák. 

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure és a Stack Overflow fórumok hello](https://azure.microsoft.com/support/forums/). Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) válassza ki **támogatja az beszerzése**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Első hibaelhárítási lépések
Minden hibaelhárítási lépés után próbálkozzon a virtuális gép toohello újracsatlakozás:

1. Visszaállítja a távoli asztal-konfigurációt.
2. Ellenőrizze a hálózati biztonsági csoport szabályainak / felhőalapú szolgáltatások végpontok.
3. Tekintse át a virtuális gép konzol naplókat.
4. A virtuális gép hello NIC hello alaphelyzetbe.
5. Ellenőrizze a virtuális gép Resource Health hello.
6. A virtuális gép jelszavát.
7. Indítsa újra a virtuális Gépet.
8. Telepítse újra a virtuális Gépet.

Továbbra is olvasási, ha a részletes lépéseket és magyarázata van szükség. Győződjön meg arról, hogy helyi hálózati eszközökre, például az útválasztók és tűzfalak nem blokkolják a kimenő 3389-es TCP-port leírtaknak megfelelően [hibakeresésekhez RDP részletes](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!TIP]
> Ha hello **Connect** gombra kattint, a virtuális Gépet ki használhatók hello portálon, és nem áll keresztül csatlakoztatott tooAzure egy [Express Route](../../expressroute/expressroute-introduction.md) vagy [telephelyek közötti VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) kapcsolat van szüksége toocreate és előtt oldani a virtuális Gépet egy nyilvános IP-cím hozzárendelése RDP használhatja. További információ: [nyilvános IP-címek az Azure-ban](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-tootroubleshoot-rdp-issues"></a>Többféleképpen tootroubleshoot RDP-problémák
Hello a következő módszerek egyikét használva hello Resource Manager üzembe helyezési modellben használatával létrehozott virtuális gépek hibaelhárítását hajthatja végre:

* [Azure-portálon](#using-the-azure-portal) – Ha tooquickly kell nagy hello RDP vagy felhasználói hitelesítő adatok alaphelyzetbe állítása, és nem az Azure-eszközök telepítve hello.
* [Az Azure PowerShell](#using-azure-powershell) – Ha ismeri a Feladatkezelő egy PowerShell-parancssorba, gyorsan alaphelyzetbe állítása hello RDP vagy felhasználói hitelesítő adatok hello Azure PowerShell-parancsmagok használatával.

Is megkeresheti a hello használatával létrehozott virtuális gépek hibaelhárítási lépéseket [klasszikus telepítési modell](#troubleshoot-vms-created-using-the-classic-deployment-model).

<a id="fix-common-remote-desktop-errors"></a>

## <a name="troubleshoot-using-hello-azure-portal"></a>Hibaelhárítás a hello Azure-portál használatával
Hibaelhárítási lépések, után próbáljon tooyour VM újra. Ha továbbra sem sikerül kapcsolódni, próbálja hello következő lépésre.

1. **Az RDP-kapcsolat alaphelyzetbe állítására**. Ez a hibaelhárítási lépés távoli kapcsolatok le vannak tiltva, vagy a Windows tűzfalszabályok blokkolják RDP, például alaphelyzetbe állítja a hello RDP-konfigurációját.
   
    Válassza ki a virtuális gép hello Azure-portálon. Görgessen lefelé hello-beállítások panelen toohello **támogatási + hibaelhárítás** szakasz közelében hello lista aljára. Kattintson a hello **jelszó-átállítási** gombra. Set hello **mód** túl**alaphelyzetbe állítása konfigurációs csak** majd hello **frissítés** gombra:
   
    ![Visszaállítja hello RDP-konfigurációt a hello Azure-portálon](./media/troubleshoot-rdp-connection/reset-rdp.png)
2. **Ellenőrizze hálózati biztonsági csoportszabályok**. Használjon [IP folyamata ellenőrizze](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) tooconfirm, ha egy szabály a hálózati biztonsági csoport egy virtuális gép forgalom tooor blokkolja. Emellett áttekintheti a hatékony biztonsági csoport tooensure a "Engedélyezése" NSG bejövő szabályok szabály létezik-e, és a rendszer előrébb RDP-porthoz (3389-es alapértelmezett). További információkért lásd: [hatékony biztonsági szabályok használatával tootroubleshoot VM forgalom bonyolódjon](../../virtual-network/virtual-network-nsg-troubleshoot-portal.md#using-effective-security-rules-to-troubleshoot-vm-traffic-flow).

3. **Tekintse át a virtuális gép rendszerindítási diagnosztika**. Ez a hibaelhárítási lépés hello VM konzol naplók toodetermine ellenőrzi, hogy ha hello VM kapcsolatos problémát jelez. Nem minden virtuális gép rendelkezik engedélyezve van, a rendszerindítási diagnosztika, ezért lehet, hogy ez a hibaelhárítási lépés nem kötelező.
   
    Adott hibaelhárítási lépéseket ez a cikk hello terjed, de, amely érinti az RDP-kapcsolat szélesebb problémájára utalhat. Hello konzol naplói és a virtuális gép képernyőkép áttekintésével további információkért lásd: [rendszerindítási diagnosztika a virtuális gépek](boot-diagnostics.md).

4. **A virtuális gép hello hálózati adapter alaphelyzetbe állítása hello**. További információkért lásd: [hogyan tooreset az Azure Windows virtuális hálózati adapter](reset-network-interface.md).
5. **Ellenőrizze a virtuális gép Resource Health hello**. Ez a hibaelhárítási lépés ellenőrzi, hogy nincsenek ismert problémák a hello Azure platformon, amely hatással lehet a virtuális gép kapcsolat toohello.
   
    Válassza ki a virtuális gép hello Azure-portálon. Görgessen lefelé hello-beállítások panelen toohello **támogatási + hibaelhárítás** szakasz közelében hello lista aljára. Kattintson a hello **erőforrás állapota** gombra. A megfelelő virtuális gépek jelenti, hogy **elérhető**:
   
    ![Ellenőrizze a VM erőforrás állapota a hello Azure-portálon](./media/troubleshoot-rdp-connection/check-resource-health.png)
6. **Felhasználói hitelesítő adatok alaphelyzetbe állítása**. Ez a hibaelhárítási lépés egy helyi rendszergazdai fiók hello jelszavának alaphelyzetbe állítása, ha bizonytalan, vagy elfelejtette hello hitelesítő adatokat.
   
    Válassza ki a virtuális gép hello Azure-portálon. Görgessen lefelé hello-beállítások panelen toohello **támogatási + hibaelhárítás** szakasz közelében hello lista aljára. Kattintson a hello **jelszó-átállítási** gombra. Győződjön meg arról, hogy hello **mód** értéke túl**jelszó-átállítási** és írja be a felhasználónevet és egy új jelszót. Végül kattintson a hello **frissítés** gombra:
   
    ![Az Azure-portálon hello hello felhasználói hitelesítő adatok alaphelyzetbe állítása](./media/troubleshoot-rdp-connection/reset-password.png)
7. **Indítsa újra a virtuális gép**. Ez a hibaelhárítási lépés kijavíthatja a mögöttes probléma merül fel hello virtuális gépért nehézségekkel.
   
    Válassza ki a virtuális gép hello Azure-portálon, majd kattintson a hello **áttekintése** fülre. Kattintson a hello **indítsa újra a** gombra:
   
    ![Indítsa újra a virtuális gép hello hello Azure-portálon](./media/troubleshoot-rdp-connection/restart-vm.png)
8. **Telepítse újra a virtuális gép**. Ez a hibaelhárítási lépés redeploys a virtuális gép tooanother gazdagépen belül Azure toocorrect alapul szolgáló platform vagy hálózati probléma merül fel.
   
    Válassza ki a virtuális gép hello Azure-portálon. Görgessen lefelé hello-beállítások panelen toohello **támogatási + hibaelhárítás** szakasz közelében hello lista aljára. Kattintson a hello **újratelepíteni** gombra, majd **újratelepíteni**:
   
    ![Telepítse újra a virtuális gép hello hello Azure-portálon a](./media/troubleshoot-rdp-connection/redeploy-vm.png)
   
    Ez a művelet befejezése után a rövid élettartamú lemezadatokat elveszett és dinamikus VM frissítése hello társított IP-címeket.

Ha RDP problémák továbbra is találkozik, akkor [támogatási kérést nyithat](https://azure.microsoft.com/support/options/) vagy olvasási [RDP hibaelhárítással kapcsolatos fogalmak és a lépések részletes](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-using-azure-powershell"></a>Hibaelhárítás az Azure PowerShell használatával
Ha még nem tette, [telepítése és konfigurálása legújabb Azure PowerShell hello](/powershell/azure/overview).

hello alábbi példák a változókkal például `myResourceGroup`, `myVM`, és `myVMAccessExtension`. A változó nevét és helyét. cserélje le a saját értékeit.

> [!NOTE]
> Hello felhasználói hitelesítő adatokat és hello RDP-konfigurációjának visszaállítása hello segítségével [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell-parancsmagot. A következő példákban hello `myVMAccessExtension` hello folyamat részeként megadott név. Ha előzőleg dolgozott hello VMAccessAgent, hello meglévő bővítmény hello nevét beszerezheti a `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` hello VM toocheck hello tulajdonságait. tooview hello neve, tekintse meg a hello kimeneti hello "Extensions" részében található.

Hibaelhárítási lépések, után próbáljon tooyour VM újra. Ha továbbra sem sikerül kapcsolódni, próbálja hello következő lépésre.

1. **Az RDP-kapcsolat alaphelyzetbe állítására**. Ez a hibaelhárítási lépés távoli kapcsolatok le vannak tiltva, vagy a Windows tűzfalszabályok blokkolják RDP, például alaphelyzetbe állítja a hello RDP-konfigurációját.
   
    hello alábbi példában alaphelyzetbe állítja a nevű virtuális gép RDP-kapcsolatokat hello `myVM` a hello `WestUS` helyét és nevű hello erőforráscsoportban `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```
2. **Ellenőrizze hálózati biztonsági csoportszabályok**. Ez a hibaelhárítási lépés ellenőrzi, hogy rendelkezik-e a szabály az a hálózati biztonsági csoport toopermit RDP-forgalmát. hello alapértelmezett portját RDP a 3389-es TCP-port. A szabály toopermit RDP-forgalmát előfordulhat, hogy nem automatikusan létrejön a virtuális gép létrehozásakor.
   
    Először, rendelje hozzá a hálózati biztonsági csoport toohello összes hello konfigurációs adatok `$rules` változó. hello alábbi példa információt szerez hello hálózati biztonsági csoport nevű `myNetworkSecurityGroup` nevű hello erőforráscsoportban `myResourceGroup`:
   
    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```
   
    Most hello szabályok megtekintése, a hálózati biztonsági csoporthoz vannak beállítva. Győződjön meg arról, hogy létezik olyan szabály tooallow TCP 3389-es portot a bejövő kapcsolatok az alábbiak szerint:
   
    ```powershell
    $rules.SecurityRules
    ```
   
    hello következő példa bemutatja egy érvényes biztonsági szabályt, amely lehetővé teszi az RDP-forgalmát. Látható `Protocol`, `DestinationPortRange`, `Access`, és `Direction` megfelelően vannak konfigurálva:
   
    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```
   
    Ha nem rendelkezik olyan szabály, amely lehetővé teszi, hogy RDP-forgalmát, [hozzon létre egy hálózati biztonsági szabály](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Engedélyezi a 3389-es TCP-port.
3. **Felhasználói hitelesítő adatok alaphelyzetbe állítása**. Ez a hibaelhárítási lépés hello jelszavának hello a helyi rendszergazdai fiókra, amely a, ha biztos abban, vagy elfelejtette hello hitelesítő adatok alaphelyzetbe állítása.
   
    Először adja meg a felhasználónév hello és egy új jelszót hozzárendelésével hitelesítő adatok toohello `$cred` változót az alábbiak szerint:
   
    ```powershell
    $cred=Get-Credential
    ```
   
    Most frissítse a virtuális gép hello-felhasználó hitelesítő adatai. hello alábbi példa frissíti a virtuális gép nevű hello hitelesítő `myVM` a hello `WestUS` helyét és nevű hello erőforráscsoportban `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```
4. **Indítsa újra a virtuális gép**. Ez a hibaelhárítási lépés kijavíthatja a mögöttes probléma merül fel hello virtuális gépért nehézségekkel.
   
    a következő példa újraindítást hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:
   
    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```
5. **Telepítse újra a virtuális gép**. Ez a hibaelhárítási lépés redeploys a virtuális gép tooanother gazdagépen belül Azure toocorrect alapul szolgáló platform vagy hálózati probléma merül fel.
   
    a következő példa redeploys hello hello nevű virtuális gép `myVM` a hello `WestUS` helyét és nevű hello erőforráscsoportban `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Ha RDP problémák továbbra is találkozik, akkor [támogatási kérést nyithat](https://azure.microsoft.com/support/options/) vagy olvasási [RDP hibaelhárítással kapcsolatos fogalmak és a lépések részletes](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-vms-created-using-hello-classic-deployment-model"></a>Virtuális gépek hello klasszikus telepítési modellel készült hibaelhárítása
Hibaelhárítási lépések, után toohello VM újracsatlakozás próbálja.

1. **Az RDP-kapcsolat alaphelyzetbe állítására**. Ez a hibaelhárítási lépés távoli kapcsolatok le vannak tiltva, vagy a Windows tűzfalszabályok blokkolják RDP, például alaphelyzetbe állítja a hello RDP-konfigurációját.
   
    Válassza ki a virtuális gép hello Azure-portálon. Kattintson a hello **... További** gombra, majd kattintson az **távelérés alaphelyzetbe**:
   
    ![Visszaállítja hello RDP-konfigurációt a hello Azure-portálon](./media/troubleshoot-rdp-connection/classic-reset-rdp.png)
2. **Cloud Services végpontok ellenőrzése**. Ez a hibaelhárítási lépés ellenőrzi, hogy rendelkezik a Felhőszolgáltatások toopermit RDP-forgalmat a végpontok. hello alapértelmezett portját RDP a 3389-es TCP-port. A szabály toopermit RDP-forgalmát előfordulhat, hogy nem automatikusan létrejön a virtuális gép létrehozásakor.
   
   Válassza ki a virtuális gép hello Azure-portálon. Kattintson a hello **végpontok** gombra kattint, a virtuális gép jelenleg konfigurált tooview hello végpontok. Győződjön meg arról, hogy a végpontok léteznek, amely engedélyezi az RDP a 3389-es TCP-porton.
   
   a következő példa hello érvényes végpontok, amelyek lehetővé teszik az RDP-forgalmát jeleníti meg:
   
   ![Az Azure-portálon hello Felhőszolgáltatások végpontok ellenőrzése](./media/troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)
   
   Ha nem rendelkezik olyan végponttal, amely lehetővé teszi, hogy RDP-forgalmát, [Felhőszolgáltatások-végpont létrehozása](classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). TCP tooprivate 3389-es port engedélyezéséhez.
3. **Tekintse át a virtuális gép rendszerindítási diagnosztika**. Ez a hibaelhárítási lépés hello VM konzol naplók toodetermine ellenőrzi, hogy ha hello VM kapcsolatos problémát jelez. Nem minden virtuális gép rendelkezik engedélyezve van, a rendszerindítási diagnosztika, ezért lehet, hogy ez a hibaelhárítási lépés nem kötelező.
   
    Adott hibaelhárítási lépéseket ez a cikk hello terjed, de, amely érinti az RDP-kapcsolat szélesebb problémájára utalhat. Hello konzol naplói és a virtuális gép képernyőkép áttekintésével további információkért lásd: [rendszerindítási diagnosztika a virtuális gépek](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).
4. **Ellenőrizze a virtuális gép Resource Health hello**. Ez a hibaelhárítási lépés ellenőrzi, hogy nincsenek ismert problémák a hello Azure platformon, amely hatással lehet a virtuális gép kapcsolat toohello.
   
    Válassza ki a virtuális gép hello Azure-portálon. Görgessen lefelé hello-beállítások panelen toohello **támogatási + hibaelhárítás** szakasz közelében hello lista aljára. Kattintson a hello **Resource Health** gombra. A megfelelő virtuális gépek jelenti, hogy **elérhető**:
   
    ![Ellenőrizze a VM erőforrás állapota a hello Azure-portálon](./media/troubleshoot-rdp-connection/classic-check-resource-health.png)
5. **Felhasználói hitelesítő adatok alaphelyzetbe állítása**. Ez a hibaelhárítási lépés hello jelszavának hello helyi rendszergazdai fiók megadása, ha nem ismeri, vagy elfelejtette hello hitelesítő adatok alaphelyzetbe állítása.
   
    Válassza ki a virtuális gép hello Azure-portálon. Görgessen lefelé hello-beállítások panelen toohello **támogatási + hibaelhárítás** szakasz közelében hello lista aljára. Kattintson a hello **jelszó-átállítási** gombra. Adja meg a felhasználónevet és egy új jelszót. Végül kattintson a hello **mentése** gombra:
   
    ![Az Azure-portálon hello hello felhasználói hitelesítő adatok alaphelyzetbe állítása](./media/troubleshoot-rdp-connection/classic-reset-password.png)
6. **Indítsa újra a virtuális gép**. Ez a hibaelhárítási lépés kijavíthatja a mögöttes probléma merül fel hello virtuális gépért nehézségekkel.
   
    Válassza ki a virtuális gép hello Azure-portálon, majd kattintson a hello **áttekintése** fülre. Kattintson a hello **indítsa újra a** gombra:
   
    ![Indítsa újra a virtuális gép hello hello Azure-portálon](./media/troubleshoot-rdp-connection/classic-restart-vm.png)

Ha RDP problémák továbbra is találkozik, akkor [támogatási kérést nyithat](https://azure.microsoft.com/support/options/) vagy olvasási [RDP hibaelhárítással kapcsolatos fogalmak és a lépések részletes](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-specific-rdp-errors"></a>Adott RDP-hibák elhárítása
Egy adott hibaüzenetet tooconnect tooyour virtuális gép RDP-kapcsolaton keresztül tett kísérlet során fordulhatnak elő. Az alábbiakban hello hello leggyakoribb hibaüzenetek:

* [hello távoli munkamenet meg lett szakítva, mert nincs elérhető távoli asztali licenckiszolgálókat tooprovide licenc](troubleshoot-specific-rdp-errors.md#rdplicense).
* [A távoli asztal nem találja a hello "számítógépnév"](troubleshoot-specific-rdp-errors.md#rdpname).
* [Hitelesítési hiba történt. hello helyi biztonsági szervezet nem érhető el](troubleshoot-specific-rdp-errors.md#rdpauth).
* [Windows biztonsági hiba: A hitelesítő adatok nem működött](troubleshoot-specific-rdp-errors.md#wincred).
* [Ezen a számítógépen nem lehet kapcsolódni a távoli számítógép toohello](troubleshoot-specific-rdp-errors.md#rdpconnect).

## <a name="additional-resources"></a>További források
Ha ezek a hibák egyike sem történt, és továbbra sem sikerül kapcsolódni toohello VM távoli asztalon keresztül, olvassa el a részletes hello [hibaelhárítási útmutatója a távoli asztal](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Című témakörben leírt lépéseket a virtuális gép futó alkalmazásokhoz való hozzáférés [kapcsolatos problémák elhárítása access tooan alkalmazást egy Azure virtuális Gépen futó](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Ha a Secure Shell (SSH) tooconnect tooa Linux virtuális gép használata az Azure-ban problémát tapasztal, tekintse meg [hibaelhárítása SSH kapcsolatok tooa Linux virtuális gép az Azure-ban](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

