---
title: az Azure-ban aaaIntroduction tooLinux |} Microsoft Docs
description: "Tudnivalók Azure Linux virtuális gépek használatával."
services: virtual-machines-linux
documentationcenter: python
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: b13bf305-87bf-4df3-815e-e8f6337aa6ea
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: szark
ms.openlocfilehash: 3a931447ee23ce7000174ca314c3e10abc6b8e74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toolinux-on-azure"></a>Bevezetés tooLinux az Azure-on
Ez a témakör áttekintést a Linux virtuális gépek használatát hello Azure felhőben egyes funkcióit. A Linux virtuális gép telepítését egy egyszerű folyamatot a lemezkép használatával hello gyűjteményből.

## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Hitelesítési: Felhasználónevek, jelszavak és SSH-kulcsok
Történő létrehozásakor a Linux virtuális gépek hello Azure-portálon, a rendszer felkéri tooprovide vagy felhasználónév és jelszó vagy nyilvános SSH-kulcsot. hello egy felhasználónévvel, egy Linux virtuális gépet az Azure telepítéséhez választott nem a következő megkötés tulajdonos toohello: rendszer fiókok nevét (UID < 100) hello már szerepel a virtuális gép nem engedélyezettek, "Gyökér" például.

* Lásd: [Linuxot futtató virtuális gép létrehozása](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Lásd: [hogyan tooUse Azure Linux SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="obtaining-superuser-privileges-using-sudo"></a>Nem sikerült beolvasni felügyelő jogosultságok használata`sudo`
hello Azure virtuálisgép-példány telepítése során megadott felhasználói fiók egy rendszerjogosultságú fiókot. Ez a fiók konfigurálható hello Azure Linux ügynök toobe képes tooelevate jogosultságokkal tooroot (felügyelő fiók) használatával hello `sudo` segédprogram. Miután bejelentkezett a felhasználói fiókkal, legfelső szintű hello parancsszintaxis segítségével fogja képes toorun parancsokat:

    # sudo <COMMAND>

Opcionálisan beszerezhet egy legfelső szintű rendszerhéj használatával **sudo -s**.

* Lásd: [root jogosultságot használata a Linux virtuális gépek Azure-ban](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="firewall-configuration"></a>Tűzfal-konfiguráció
Azure biztosít egy bejövő csomag szűrőt, amely korlátozza a kapcsolat tooports megadott hello Azure-portálon. Alapértelmezés szerint hello csak engedélyezett port az SSH. Megnyitható másolatot hozzáférési tooadditional portok a Linux virtuális gép végpontok konfigurálásával hello Azure-portálon:

* Lásd: [hogyan tooSet be végpontok tooa virtuális gép](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

hello Linux képek hello Azure-katalógus nem engedélyezi a hello *iptables* tűzfal alapértelmezés szerint. Igény szerint konfigurálható hello tűzfal tooprovide további szűrést.

## <a name="hostname-changes"></a>Állomásnév változások
A Linux-lemezkép egy példányát telepít elkülönítésével szükséges tooprovide egy hello virtuálisgép-nevet. Ha hello virtuális gép fut, ez az állomásnév közzétett toohello platform DNS-kiszolgálók több csatlakozó virtuális gépek tooeach más IP-cím keresések segítségével hostnames hajthat végre.

Igény állomásnév módosítások vannak egy virtuális gép telepítését követően, használjon hello parancs

    # sudo hostname <newname>

hello Azure Linux ügynök funkcióit tartalmazza: tooautomatically észleli a név megváltoztatása és megfelelő konfigurálása hello virtuális gép toopersist ezt a változtatást és közzététele a módosítás toohello platform DNS-kiszolgálók.

* [Azure Linux ügynök felhasználói útmutatója](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="cloud-init"></a>Felhő inicializálás
**Ubuntu** és **CoreOS** képek felhő inicializálás Azure, amely további képességeket biztosít a virtuális gépek rendszerindítása használják.

* [Hogyan tooInject egyéni adatok](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Egyéni adatok és a Microsoft Azure felhőbe inicializálás](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
* [Partíciók létrehozása a Azure Swap használatával felhő inicializálás](https://wiki.ubuntu.com/AzureSwapPartitions)
* [Hogyan tooUse CoreOS az Azure-on](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="virtual-machine-image-capture"></a>Virtuális gép lemezképének rögzítése
Azure hello képességét toocapture hello egy meglévő virtuális gép állapotát, ezt követően lehet használt toodeploy további virtuálisgép-példányok lemezképekbe biztosít. hello Azure Linux ügynök lehet néhány hello hello kiépítési folyamat során végrehajtott testreszabási használt toorollback. Előfordulhat, hogy kövesse az alábbi toocapture egy virtuális gép hello lépéseket képként:

1. Futtatás **waagent-deprovision** testreszabási kiépítés tooundo. Vagy **waagent-deprovision + felhasználói** toooptionally hello felhasználói fiók megadva során üzembe helyezési és minden kapcsolódó adatok törlése.
2. Állítsa le vagy kapcsolja ki hello virtuális gépet.
3. Kattintson a **rögzítése** hello Azure portál vagy használata hello PowerShell vagy a parancssori eszközök toocapture hello virtuális gép képként.
   
   * Lásd: [hogyan tooCapture egy Linux virtuális gép tooUse sablonként](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="attaching-disks"></a>Lemez csatolása
Minden virtuális gép van egy ideiglenes, helyi *erőforrás lemez* csatolva. Egy erőforrás tárolt adatokat nem lehet tartós újraindítások, mert gyakran használják az alkalmazások és az átmeneti hello virtuális gépen futó folyamatok és **ideiglenes** adatok tárolására. Akkor is használt toostore hello lap vagy swap hello operációs rendszer fájljait.

Linux, hello erőforrás lemez általában hello Azure Linux ügynök által felügyelt, és automatikusan csatlakoztatott túl**/mnt és az erőforrások** (vagy **/mnt** Ubuntu képek).

> [!NOTE]
> Vegye figyelembe, hogy hello erőforrás lemez egy **ideiglenes** lemez, és előfordulhat, hogy törli, majd amikor hello virtuális gép újraindítása után formázni.
> 
> 

Linux hello adatlemez, hello kernel neve lehet `/dev/sdc`, és toopartition, a felhasználóknak kell formázni, és csatlakoztassa az adott erőforrás. Ez vonatkozik hello oktatóprogram lépésről lépésre: [hogyan tooAttach egy virtuális gép adatlemez tooa](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

* **További információ:** [szoftveres RAID Linux konfigurálása](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [LVM konfigurálása a Linux virtuális gép az Azure-ban](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

