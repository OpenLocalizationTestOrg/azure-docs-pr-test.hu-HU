---
title: "SSH-kapcsolat aaaTroubleshoot problémák tooan Azure virtuális gép |} Microsoft Docs"
description: "Hogyan tootroubleshoot állít ki például az \"SSH-kapcsolat sikertelen\" vagy \"Az SSH-kapcsolat elutasítva\" egy Azure virtuális gép Linux operációs rendszert futtató."
keywords: "ssh kapcsolat visszautasította, ssh hiba és az azure-ssh, SSH-kapcsolat sikertelen"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: dcb82e19-29b2-47bb-99f2-900d4cfb5bbb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: dfb4e75e571c8306edf5f300c4e0f07a5fe7750a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-ssh-connections-tooan-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>SSH-kapcsolatok tooan Azure Linux virtuális Gépet, amely nem sikerül, hibák, vagy elutasítják hibaelhárítása
Oka lehet különböző, hogy Secure Shell (SSH) hibák, az SSH-kapcsolódási hibák, vagy az SSH a rendszer elutasította a rendszer, amikor megpróbál tooconnect tooa Linux virtuális gép (VM). Ez a cikk segít keresés és a megfelelő hello problémákat. Linux tootroubleshoot hello Azure-portálon az Azure parancssori felület vagy a Virtuálisgép-hozzáférés bővítmény használatát, és a csatlakozási problémák megoldását.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure és a Stack Overflow fórumok hello](http://azure.microsoft.com/support/forums/). Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](http://azure.microsoft.com/support/options/) válassza **segítségre van szüksége**. Támogatja az Azure használatával kapcsolatos információkért olvassa el a hello [Microsoft Azure-támogatás – gyakori kérdések](http://azure.microsoft.com/support/faq/).

## <a name="quick-troubleshooting-steps"></a>Első hibaelhárítási lépések
Hibaelhárítási lépések, után toohello VM újracsatlakozás próbálja.

1. Visszaállítja a hello SSH-konfigurációt.
2. Alaphelyzetbe állítja a hello hello felhasználó hitelesítő adatai.
3. Ellenőrizze a hello [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) szabályok lehetővé teszik az SSH-forgalmat.
   * Győződjön meg arról, hogy létezik-e a hálózati biztonsági csoport szabály toopermit SSH-forgalom (ami alapértelmezés szerint a 22-es TCP-port).
   * Nem használhat port átirányítási / leképezése egy Azure terheléselosztó használata nélkül.
4. Ellenőrizze a hello [VM erőforrás állapota](../../resource-health/resource-health-overview.md). 
   * Győződjön meg arról, hogy hello VM jelentéseket, hogy kifogástalan.
   * Ha engedélyezve van a rendszerindítási diagnosztika, győződjön meg arról, hello virtuális gép nem jelent rendszerindítási hibák hello rögzít.
5. Indítsa újra a virtuális gép hello.
6. Telepítse újra a virtuális gép hello.

Részletesebb hibaelhárítási lépéseket és magyarázatokat olvasási továbbra is.

## <a name="available-methods-tootroubleshoot-ssh-connection-issues"></a>Rendelkezésre álló metódusok tootroubleshoot SSH-kapcsolódási problémák
Új hitelesítő adatokat, vagy SSH-konfigurációt hello a következő módszerek egyikével:

* [Azure-portálon](#use-the-azure-portal) – nagy Ha tooquickly kell hello SSH-konfigurációt vagy SSH-kulcs visszaállítása, és nem az Azure-eszközök telepítve hello.
* [Az Azure CLI 2.0](#use-the-azure-cli-20) – Ha már hello parancssorban, gyorsan alaphelyzetbe állítása hello SSH-konfigurációt vagy a hitelesítő adatokat. Is használhatja a hello [Azure CLI 1.0](#use-the-azure-cli-10)
* [Az Azure VMAccessForLinux bővítmény](#use-the-vmaccess-extension) - létrehozása, és újra felhasználhatja json definíciós fájlok tooreset hello SSH konfigurációs vagy felhasználói hitelesítő adatok.

Hibaelhárítási lépések, után próbáljon tooyour VM újra. Ha továbbra sem sikerül kapcsolódni, próbálja hello következő lépésre.

## <a name="use-hello-azure-portal"></a>Hello Azure portál használata
hello Azure portál egy gyorsan tooreset hello SSH konfigurációs vagy felhasználói hitelesítő adatok nélkül biztosít bármely eszközök telepítése a helyi számítógépen.

Válassza ki a virtuális gép hello Azure-portálon. Görgessen lefelé toohello **támogatási + hibaelhárítás** válassza ki azt **jelszó-átállítási** beállítani, mint például a következő hello:

![SSH-konfigurációt vagy hello Azure-portálon a hitelesítő adatok alaphelyzetbe állítása](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-hello-ssh-configuration"></a>Hello SSH-konfigurációjának visszaállítása
Első lépésként válassza ki a `Reset configuration only` a hello **mód** legördülő menü képernyőképe, megelőző hello a kattintson a hello **alaphelyzetbe** gombra. Ez a művelet befejezése után próbálja tooaccess a virtuális Gépet újra.

### <a name="reset-ssh-credentials-for-a-user"></a>Egy felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása
Válassza egy meglévő felhasználó tooreset hello hitelesítő adatok `Reset SSH public key` vagy `Reset password` a hello **mód** legördülő menü képernyőképe megelőző hello hasonlóan. Hello felhasználónév és egy SSH-kulcs vagy új jelszót adjon meg, majd kattintson a hello **alaphelyzetbe** gombra.

A felhasználó sudo jogosultsági szintű hello VM ebből a menüből is létrehozhat. Adjon meg egy új felhasználónevet és a kapcsolódó jelszó vagy SSH-kulcs, és kattintson a hello **alaphelyzetbe** gombra.

## <a name="use-hello-azure-cli-20"></a>Azure CLI 2.0 hello használata
Ha még nem tette meg, telepítse hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).

Ha elkészült, és a feltöltött Linux egyéni lemezképet, győződjön meg arról, hogy hello [Microsoft Azure Linux ügynök](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 2.0.5 verzió vagy újabb verziója. A gyűjtemény lemezképei használatával létrehozott virtuális gép esetében a hozzáférés bővítmény már telepített és konfigurálást.

### <a name="reset-ssh-configuration"></a>SSH-konfigurációjának visszaállítása
Először próbálja meg visszaállítása folyamatban hello SSH konfigurációs toodefault értékek, illetve lehet újraindítás hello SSH-kiszolgálót a virtuális gép hello. Vegye figyelembe, hogy ez nem módosítja hello felhasználói fiók nevét, a jelszó vagy SSH-kulcsok.
hello alábbi példában [az vm felhasználói alaphelyzetbe állítása-ssh](/cli/azure/vm/user#reset-ssh) tooreset hello SSH-konfigurációt a hello nevű virtuális gép `myVM` a `myResourceGroup`. A saját értékeit a következőképpen használhatja:

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a>Egy felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása
hello alábbi példában [az vm felhasználói frissítése](/cli/azure/vm/user#update) tooreset hello hitelesítő adatait, `myUsername` toohello érték van megadva a `myPassword`, a hello nevű virtuális gép `myVM` a `myResourceGroup`. A saját értékeit a következőképpen használhatja:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Ha SSH-kulcs hitelesítést használ, alaphelyzetbe állíthatja a hello SSH-kulcs az adott felhasználó számára. hello alábbi példában **az vm hozzáférés set-linux-felhasználói** tooupdate hello SSH-kulcs tárolható `~/.ssh/id_rsa.pub` nevű hello felhasználó `myUsername`, a hello nevű virtuális gép `myVM` a `myResourceGroup`. A saját értékeit a következőképpen használhatja:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-hello-vmaccess-extension"></a>Hello VMAccess bővítmény használatára
a json-fájl, amely meghatározza a kimenő műveletek toocarry hello Linux virtuális gép hozzáférési bővítményével olvassa be. Ilyen műveletek közé tartoznak a SSHD alaphelyzetbe állítása, egy SSH-kulcsának visszaállítása vagy egy felhasználót. Hello Azure CLI toocall hello VMAccess bővítmény továbbra is használhatja, de is felhasználhatja hello json-fájlok több virtuális gépek között, ha szükséges. Ez a megközelítés lehetővé teszi majd hívható forgatókönyvek a megadott json-fájlok tára toocreate.

### <a name="reset-sshd"></a>SSHD alaphelyzetbe állítása
Hozzon létre egy fájlt `settings.json` a tartalom a következő hello:

```json
{  
    "reset_ssh":"True"
}
```

Hello Azure parancssori felület használatával, majd meghívja a hello `VMAccessForLinux` bővítmény tooreset a SSHD kapcsolat megadásával a json-fájl. hello alábbi példában [az virtuálisgép-bővítmény készlet](/cli/azure/vm/extension#set) tooreset SSHD hello nevű virtuális gép a `myVM` a `myResourceGroup`. A saját értékeit a következőképpen használhatja:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>Egy felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása
Ha SSHD toofunction megfelelően jelenik meg, alaphelyzetbe állíthatja a hello előállító felhasználó hitelesítő adatait. egy felhasználó tooreset hello jelszavát, hozzon létre egy fájlt `settings.json`. hello alábbi példa visszaállítja hello hitelesítő adatainak `myUsername` toohello érték van megadva a `myPassword`. Adja meg a következő sorokat hello a `settings.json` fájl, a saját értékekkel:

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

Vagy a felhasználó az SSH-kulcs hello, először hozzon létre egy fájlt tooreset `settings.json`. hello alábbi példa visszaállítja hello hitelesítő adatainak `myUsername` toohello érték van megadva a `myPassword`, a hello nevű virtuális gép `myVM` a `myResourceGroup`. Adja meg a következő sorokat hello a `settings.json` fájl, a saját értékekkel:

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

A json-fájl létrehozása után használja a hello Azure CLI toocall hello `VMAccessForLinux` bővítmény tooreset az SSH-felhasználó hitelesítő adatait a json-fájl megadásával. hello alábbi példa visszaállítja hello nevű VM-felhasználó hitelesítő adatai `myVM` a `myResourceGroup`. A saját értékeit a következőképpen használhatja:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-hello-azure-cli-10"></a>Hello Azure CLI 1.0 használata
Ha még nem tette, [hello Azure CLI 1.0 telepítése, és csatlakozzon az Azure-előfizetés tooyour](../../cli-install-nodejs.md). Győződjön meg arról, hogy a Resource Manager módot az alábbiak szerint:

```azurecli
azure config mode arm
```

Ha elkészült, és a feltöltött Linux egyéni lemezképet, győződjön meg arról, hogy hello [Microsoft Azure Linux ügynök](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 2.0.5 verzió vagy újabb verziója. A gyűjtemény lemezképei használatával létrehozott virtuális gép esetében a hozzáférés bővítmény már telepített és konfigurálást.

### <a name="reset-ssh-configuration"></a>SSH-konfigurációjának visszaállítása
hello SSHD konfigurációs maga nincs megfelelően konfigurálva, vagy hello szolgáltatás hibát észlelt. Alaphelyzetbe állíthatja a SSHD toomake, hogy érvényes-e a saját magát hello SSH-konfigurációt. SSHD alaphelyzetbe kell hello első hibaelhárítási lépés végrehajtása.

hello alábbi példa visszaállítja a virtuális gép neve SSHD `myVM` nevű hello erőforráscsoportban `myResourceGroup`. A saját virtuális gép és erőforrás-neveket használja az alábbiak szerint:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>Egy felhasználó a SSH hitelesítő adatok alaphelyzetbe állítása
Ha SSHD toofunction megfelelően jelenik meg, hello előállító felhasználó jelszavának alaphelyzetbe állíthatja. hello alábbi példa visszaállítja hello hitelesítő adatainak `myUsername` toohello érték van megadva a `myPassword`, a hello nevű virtuális gép `myVM` a `myResourceGroup`. A saját értékeit a következőképpen használhatja:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

Ha SSH-kulcs hitelesítést használ, alaphelyzetbe állíthatja a hello SSH-kulcs az adott felhasználó számára. a következő példa frissítések hello hello SSH-kulcs tárolható `~/.ssh/id_rsa.pub` nevű hello felhasználó `myUsername`, a hello nevű virtuális gép `myVM` a `myResourceGroup`. A saját értékeit a következőképpen használhatja:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```


## <a name="restart-a-vm"></a>Virtuális gép újraindítása
Ha hello SSH konfigurációs és a felhasználói hitelesítő adatok alaphelyzetbe állítása, vagy ennek során hibába ütközött, próbálkozhat újraindítással hello VM tooaddress az alapul szolgáló számítási problémákat.

### <a name="azure-portal"></a>Azure Portal
a virtuális gép használatával toorestart hello Azure portál, válassza ki a virtuális Gépet, és kattintson hello **indítsa újra a** gomb, mint például a következő hello:

![Indítsa újra a virtuális gép hello Azure-portálon](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli-10"></a>Azure CLI 1.0
a következő példa újraindítást hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`. A saját értékeit a következőképpen használhatja:

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
hello alábbi példában [az virtuális gép újraindítása](/cli/azure/vm#restart) toorestart hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`. A saját értékeit a következőképpen használhatja:

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Virtuális gép ismételt üzembe helyezése
Az Azure, amely előfordulhat, hogy javítsa ki a mögöttes hálózati probléma merül fel a virtuális gép tooanother csomópont központilag telepítheti. További információ a virtuális gép újbóli: [telepítse újra a virtuális gép toonew Azure csomópont](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Ez a művelet befejezése után rövid élettartamú lemezen tárolt adatok ekkor elvesznek, és dinamikus IP-címek hello virtuális géphez társított frissülni fog.
> 
> 

### <a name="azure-portal"></a>Azure Portal
a virtuális gép használatával tooredeploy hello Azure portál, válassza ki a virtuális Gépet, és görgessen lefelé toohello **támogatási + hibaelhárítás** szakasz. Kattintson a hello **újratelepíteni** gomb, mint például a következő hello:

![Telepítse újra a virtuális gépek a hello Azure-portálon](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli-10"></a>Azure CLI 1.0
a következő példa redeploys hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`. A saját értékeit a következőképpen használhatja:

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
használja például következő hello [az vm helyezze üzembe újra](/cli/azure/vm#redeploy) tooredeploy hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`. A saját értékeit a következőképpen használhatja:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-hello-classic-deployment-model"></a>Hello klasszikus telepítési modell használatával létrehozott virtuális gépek
Ezen lépések tooresolve hello leggyakoribb SSH kapcsolódási hibák próbálja hello klasszikus telepítési modell használatával létrehozott virtuális gépek esetén. Minden lépés után toohello VM újracsatlakozás próbálja.

* Hello a távelérés alaphelyzetbe állítása [Azure-portálon](https://portal.azure.com). A hello Azure-portálon, válassza ki a virtuális Gépet, és kattintson hello **távoli alaphelyzetbe állítása...**  gombra.
* Indítsa újra a virtuális gép hello. A hello [Azure-portálon](https://portal.azure.com), válassza ki a virtuális Gépet, majd kattintson a hello **indítsa újra a** gombra.
    
* Telepítse újra a hello VM tooa új Azure csomópont. További információ a virtuális gépek tooredeploy lásd: [telepítse újra a virtuális gép toonew Azure csomópont](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
    Ez a művelet befejezése után rövid élettartamú lemezen tárolt adatok ekkor elvesznek, és dinamikus IP-címek hello virtuális géphez társított frissülni fog.
* Hello utasításait követve [hogyan tooreset jelszó vagy SSH a Linux-alapú virtuális gépek](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) számára:
  
  * Jelszó-átállítási hello vagy SSH-kulcs.
  * Hozzon létre egy *sudo* felhasználói fiókot.
  * Visszaállítja a hello SSH-konfigurációt.
* A platform probléma merül fel hello VM erőforrás állapotának ellenőrzése.<br>
     Válassza ki a virtuális Gépet, és görgessen lefelé **beállítások** > **állapotának ellenőrzése**.

## <a name="additional-resources"></a>További források
* Ha egy virtuális gép továbbra sem tudja tooSSH tooyour következő hello után lépéseket, tekintse meg a [részletes hibaelhárítási lépéseket](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooreview további lépések tooresolve a problémát.
* Alkalmazás-hozzáférés hibaelhárítással kapcsolatos további információkért lásd: [kapcsolatos problémák elhárítása access tooan alkalmazást egy Azure virtuális gépen futó](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Hello klasszikus telepítési modell használatával létrehozott virtuális gépek hibaelhárítással kapcsolatos további információkért lásd: [hogyan tooreset jelszó vagy SSH a Linux-alapú virtuális gépek](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

