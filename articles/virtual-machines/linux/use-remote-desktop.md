---
title: "aaaUse távoli asztal tooa Linux virtuális gép az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall és a távoli asztal (xrdp) tooconnect tooa Linux virtuális gép konfigurálása az Azure-grafikus eszközök használatával"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: iainfou
ms.openlocfilehash: 64d30be101ceeb49fc05bb10293ad63db358efe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-remote-desktop-tooconnect-tooa-linux-vm-in-azure"></a>Telepítse és konfigurálja a távoli asztal tooconnect tooa Linux virtuális gép az Azure-ban
Secure shell (SSH-) kapcsolaton keresztül hello parancssorból általában felügyelt Linux virtuális gépek (VM) az Azure-ban. Amikor új tooLinux vagy gyors hibaelhárítási forgatókönyvek esetén hello használata a távoli asztal könnyebb lehet. Ez a következő cikket: részletek hogyan tooinstall és egy asztali környezet konfigurálása ([xfce](https://www.xfce.org)) és a távoli asztal ([xrdp](http://www.xrdp.org)) a Linux virtuális gép hello Resource Manager üzembe helyezési modellben a.


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk az Azure-ban meglévő Linux virtuális gép szükséges. Ha egy virtuális gép toocreate van szüksége, használja a következő módszerek hello egyikét:

- Hello [Azure CLI 2.0](quick-create-cli.md)
- Hello [Azure-portálon](quick-create-portal.md)


## <a name="install-a-desktop-environment-on-your-linux-vm"></a>Egy asztali környezet telepítése a Linux virtuális gépen
A legtöbb Linux virtuális gépet az Azure-ban nem rendelkeznek egy asztali környezetben alapértelmezés szerint telepítve. Linux virtuális gépek általában felügyelt asztali környezet helyett SSH-kapcsolatokhoz. Nincsenek a Linux, amelyek segítségével különböző asztali környezetekhez. Attól függően, hogy a munkakörnyezet választott akkor előfordulhat, hogy használnak egy too2 GB szabad lemezterület, és 5 too10 perc tooinstall érvénybe és konfigurálja az összes szükséges hello csomagok.

hello következő példa telepíti a hello lightweight [xfce4](https://www.xfce.org/) Ubuntu virtuális gép az asztali környezetet. Az egyéb terjesztéseket némileg eltérőek a parancsokat (használja `yum` tooinstall Red Hat Enterprise Linux és megfelelő konfigurálása `selinux` szabályokat, vagy használja `zypper` például a SUSE, tooinstall).

Első, SSH tooyour virtuális gép. hello alábbi példa csatlakozik toohello nevű virtuális gép *myvm.westus.cloudapp.azure.com* hello a felhasználónevet használva a *azureuser*:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Ha Windows használ, és az SSH használatával további információra van szüksége, tekintse meg [hogyan toouse SSH-kulcsok Windows](ssh-from-windows.md).

Következő lépésként telepítse a xfce használatával `apt` az alábbiak szerint:

```bash
sudo apt-get update
sudo apt-get install xfce4
```

## <a name="install-and-configure-a-remote-desktop-server"></a>A távoli asztali kiszolgáló telepítése és konfigurálása
Most, hogy egy asztali környezetben telepített, konfigurálhatja a bejövő kapcsolatok a távoli asztali szolgáltatás toolisten. [xrdp](http://xrdp.org) egy nyílt forráskódú Remote Desktop Protocol (RDP) kiszolgáló, amely elérhető a legtöbb Linux terjesztéseket, és jól xfce működik. Telepíthető xrdp az Ubuntu virtuális gép az alábbiak szerint:

```bash
sudo apt-get install xrdp
```

Mondja el xrdp milyen asztali környezetben toouse a munkamenet indításakor. Beállítása xrdp toouse xfce az asztali környezetet az alábbiak szerint:

```bash
echo xfce4-session >~/.xsession
```

Indítsa újra a hello xrdp szolgáltatást hello módosítások tootake hatása az alábbiak szerint:

```bash
sudo service xrdp restart
```


## <a name="set-a-local-user-account-password"></a>Egy helyi felhasználói fiók jelszavának beállítása
Ha létrehozott egy jelszót a felhasználói fiók létrehozása után a virtuális gép, kihagyhatja ezt a lépést. Ha csak SSH-kulcs hitelesítést használ, és nem rendelkezik a helyi fiók jelszavának megadásához adjon meg egy jelszót, a virtuális gép tooyour xrdp toolog használata előtt. xrdp nem fogadja el az SSH-kulcsok a hitelesítéshez. hello alábbi példa meghatározza, hogy egy hello felhasználói fiók jelszavát *azureuser*:

```bash
sudo passwd azureuser
```

> [!NOTE]
> A jelszó megadása nem frissíti a a SSHD konfigurációs toopermit jelszavas bejelentkezéseket, ha jelenleg nem létezik. Biztonsági szempontból előfordulhat, hogy tooconnect tooyour VM kívánja az SSH-alagút-alapú hitelesítéssel rendelkező, és csatlakoztassa a tooxrdp. Ha igen, ugorjon a következő lépés a hálózati biztonsági csoport szabály tooallow távoli asztali forgalom létrehozásával hello.


## <a name="create-a-network-security-group-rule-for-remote-desktop-traffic"></a>A távoli asztal forgalmat hálózati biztonsági csoport szabály létrehozása
tooallow távoli asztal forgalom tooreach a Linux virtuális gép, egy hálózati biztonsági szabály kell toobe létrehozása, amely lehetővé teszi a TCP port 3389 tooreach a virtuális Gépet. További információ a hálózati biztonsági csoportszabályok: [Mi az a hálózati biztonsági csoport?](../../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Emellett [használata hello Azure portál toocreate egy hálózati biztonsági szabály](../windows/nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

hello alábbi példák hozzon létre egy hálózati biztonsági csoport szabály [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) nevű *myNetworkSecurityGroupRule* túl*engedélyezése* forgalom*tcp* port *3389-es*.

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1010 \
    --destination-port-range 3389
```


## <a name="connect-your-linux-vm-with-a-remote-desktop-client"></a>A Linux virtuális gép csatlakozzon a távoli asztali ügyfél
Nyissa meg a helyi távoli asztali ügyfél, és csatlakozzon a toohello IP-cím vagy a Linux virtuális Gépet DNS-nevét. Írja be hello felhasználónév és jelszó hello felhasználói fiók a virtuális Gépet a következőképpen:

![Csatlakozzon a távoli asztali ügyfél használatával tooxrdp](./media/use-remote-desktop/remote-desktop-client.png)

Hitelesítés után hello xfce asztali környezetben betölteni, és keresse meg a következő példa hasonló toohello:

![asztali környezetben xfce xrdp keresztül](./media/use-remote-desktop/xfce-desktop-environment.png)


## <a name="troubleshoot"></a>Hibaelhárítás
Ha tooyour Linux virtuális gépet egy távoli asztali ügyfél nem tud csatlakozni, használja `netstat` a a Linux virtuális gép tooverify, amely a virtuális Gépet a figyeli az RDP-kapcsolatok az alábbiak szerint:

```bash
sudo netstat -plnt | grep rdp
```

hello a következő példa azt mutatja meg a várt módon 3389-es TCP-porton VM hello:

```bash
tcp     0     0      127.0.0.1:3350     0.0.0.0:*     LISTEN     53192/xrdp-sesman
tcp     0     0      0.0.0.0:3389       0.0.0.0:*     LISTEN     53188/xrdp
```

Ha hello xrdp szolgáltatás nem figyel, az Ubuntu virtuális gép indítsa újra hello szolgáltatást az alábbiak szerint:

```bash
sudo service xrdp restart
```

Tekintse át naplók */var/log*Thug a Ubuntu virtuális gépén az jelzések toowhy hello szolgáltatás nem válaszol. Ugyanígy figyelheti hello syslog során egy távoli asztali kapcsolat kísérlet tooview hibák:

```bash
tail -f /var/log/syslog
```

Más például Red Hat Enterprise Linux és SUSE Linux terjesztésekről különböző módokon toorestart szolgáltatások és a másodlagos napló fájl helyek tooreview állhat.

Ha a távoli asztali ügyfél nem kapott választ, és nem látható eseményeket hello rendszernaplóba, ez a viselkedés azt jelzi, hogy a távoli asztali forgalom nem érhető el a virtuális gép hello. Tekintse át a hálózati biztonsági csoport szabályok tooensure, hogy rendelkezik-e a szabály toopermit TCP 3389-es porton. További információkért lásd: [alkalmazás csatlakozási problémák](../windows/troubleshoot-app-connection.md).


## <a name="next-steps"></a>Következő lépések
Létrehozásával és az SSH-kulcsok használata a Linux virtuális gépek kapcsolatos további információkért lásd: [hozzon létre SSH-kulcsok Linux virtuális gépek Azure-ban](mac-create-ssh-keys.md).

Az SSH használata a Windows további információkért lásd: [hogyan toouse SSH-kulcsok Windows](ssh-from-windows.md).

