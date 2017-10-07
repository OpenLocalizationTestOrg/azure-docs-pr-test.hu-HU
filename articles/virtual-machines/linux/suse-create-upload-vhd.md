---
title: "aaaCreate és feltöltése az Azure-ban SUSE Linux virtuális merevlemez"
description: "Toocreate ismerje meg, és töltse fel az Azure virtuális merevlemez (VHD) a SUSE Linux operációs rendszert tartalmazó."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 066d01a6-2a54-4718-bcd0-90fe7a5303a1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: szark
ms.openlocfilehash: 9185c7e67279357f00db0f43e944e96c58f0dd60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>SLES- vagy openSUSE-alapú virtuális gép előkészítése Azure-beli használatra
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy már telepítve van a SUSE vagy openSUSE Linux operációs rendszer tooa virtuális merevlemez. Több különféle eszköz toocreate .vhd fájlok, például egy hálózatvirtualizálási megoldás például a Hyper-V létezik. Útmutatásért lásd: [hello Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE telepítési megjegyzések
* Is lásd: [általános Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további információkat az Azure Linux előkészítése.
* hello VHDX formátum nem támogatott az Azure csak **rögzített VHD**.  Alakítsa át a Hyper-V kezelőjével hello tooVHD formátummal, vagy convert-vhd parancsmag hello.
* Hello Linux rendszer telepítésekor LVM (gyakran hello alapértelmezett sok telepítés), hanem szabványos partíciók használata ajánlott. LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációs rendszer lemezén legalább egyszer kell csatolt toobe tooanother VM hibaelhárításhoz. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ha előnyben részesített adatlemezek használható.
* Ne konfiguráljon egy swap partíció hello operációsrendszer-lemezzel. Linux-ügynök hello konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet.  További információk a hello lépéseket találhatók.
* Az összes hello virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.

## <a name="use-suse-studio"></a>SUSE studióval
[SUSE Studio](http://www.susestudio.com) könnyen létrehozása és a SLES és openSUSE lemezképek kezelése az Azure és a Hyper-V. Ez az ajánlott megközelítést alkalmazva a saját SLES és openSUSE lemezképek testreszabása a hello.

Egy alternatív toobuilding, a saját virtuális merevlemez, SUSE is közzéteszi (Bring Your saját előfizetés) saját lemezképek a következő SLES [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>SUSE Linux Enterprise Server 11 SP4 előkészítése
1. Hyper-V kezelője hello középső ablaktáblában jelölje ki a hello virtuális gépet.
2. Kattintson a **Connect** tooopen hello ablak hello virtuális géphez.
3. A SUSE Linux Enterprise rendszer tooallow regisztrálja azt toodownload frissítések és a csomagok telepítése.
4. Hello rendszer frissítése a legújabb javítások hello:
   
        # sudo zypper update
5. Hello Azure Linux ügynök telepítése SLES adattárból hello:
   
        # sudo zypper install WALinuxAgent
6. Annak ellenőrzése, ha a waagent túl van-e beállítva "on" a chkconfig, és ha nem, engedélyezze az automatikus indítása:
   
        # sudo chkconfig waagent on
7. Ellenőrizze, hogy a waagent-szolgáltatás fut, és ha nem, indítsa el: 
   
        # sudo service waagent start
8. Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. a nyitott toodo "/ boot/grub/menu.lst" egy szövegszerkesztőben, és győződjön meg arról, hogy hello alapértelmezett kernel tartalmazza a következő paraméterek hello:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    Ezzel biztosíthatja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.
9. Ellenőrizze, hogy /boot/grub/menu.lst és a/etc/fstab mindkét hivatkozás hello lemez az UUID (által-uuid) helyett hello Lemezazonosítót (-azonosító szerint). 
   
    Lemezek UUID azonosítója beolvasása
   
        # ls /dev/disk/by-uuid/
   
    Ha /dev/disk/by-id / hello megfelelő által-uuid-értékkel rendelkező /boot/grub/menu.lst mind a/etc/fstab használt, frissítése
   
    Változás előtt
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    Módosítás után
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. Módosítsa a udev szabályok tooavoid hello Ethernet adaptert statikus szabályainak létrehozásakor. Ezek a szabályok problémákat okozhat, ha a Microsoft Azure- vagy Hyper-v virtuális gép klónozása
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. Ajánlott tooedit hello fájl "/ etc/sysconfig/hálózati/dhcp", és módosítsa a hello `DHCLIENT_SET_HOSTNAME` paraméter toohello következő:
    
     DHCLIENT_SET_HOSTNAME = "nem"
12. A "/ etc/sudoers" megjegyzéssé, vagy távolítsa el az alábbi sorokat, ha vannak ilyenek hello:
    
     Alapértelmezés szerint targetpw # hello célként megadott felhasználó összes ALL=(ALL) összes azaz gyökér hello jelszó kérése # figyelmeztetés! Csak ezzel együtt a 'Alapértelmezett targetpw'!
13. Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.  Ez általában akkor hello alapértelmezett.
14. Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.
    
    hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után. Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén. Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben), módosítsa megfelelően a következő paraméterek a /etc/waagent.conf hello:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Megjegyzés: a szükséges toobe toowhatever beállítása.
15. Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:
    
    # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-force - deprovision
    # <a name="export-histsize0"></a>exportálja a HISTSIZE = 0
    # <a name="logout"></a>Kijelentkezés
16. Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében. A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.

- - -
## <a name="prepare-opensuse-131"></a>OpenSUSE 13.1 + előkészítése
1. Hyper-V kezelője hello középső ablaktáblában jelölje ki a hello virtuális gépet.
2. Kattintson a **Connect** tooopen hello ablak hello virtuális géphez.
3. Hello rendszerhéj parancsot hello "`zypper lr`". Ha ez a parancs visszaadja a kimeneti hasonló toohello követve, majd várt a módosítás nélkül szükségesek hello adattárak vannak konfigurálva (vegye figyelembe, hogy verziószáma változhat):
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    Ha hello parancs visszaadja a "Nincs definiálva... adattárak" használja a következő parancsok tooadd hello ezek repók:
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    Ezután ellenőrizheti hello adattárak hello parancs futtatásával hozzáadott "`zypper lr`" újra. Abban az esetben, ha egy hello vonatkozó update adattárak nincs engedélyezve, engedélyezze a következő paranccsal:
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. Hello kernel toohello elérhető legújabb verzióra frissíteni:
   
        # sudo zypper up kernel-default
   
    Vagy tooupdate hello rendszer hello legújabb javításokat:
   
        # sudo zypper update
5. Hello Azure Linux ügynök telepítése.
   
   # <a name="sudo-zypper-install-walinuxagent"></a>sudo zypper telepítés WALinuxAgent
6. Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. toodo a, nyissa meg "/ boot/grub/menu.lst" egy szövegszerkesztőben, és győződjön meg arról, hogy hello alapértelmezett kernel tartalmazza a következő paraméterek hello:
   
     konzol ttyS0 earlyprintk = ttyS0 rootdelay = = 300
   
   Ezzel biztosíthatja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására. Ezenkívül távolítsa el a következő paraméterek hello kernel rendszerindító sor, ha vannak ilyenek hello:
   
     libata.atapi_enabled=0 tartalék = 0x1f0, 0x8
7. Ajánlott tooedit hello fájl "/ etc/sysconfig/hálózati/dhcp", és módosítsa a hello `DHCLIENT_SET_HOSTNAME` paraméter toohello következő:
   
     DHCLIENT_SET_HOSTNAME = "nem"
8. **Fontos:** "/ etc/sudoers", a megjegyzéssé, vagy távolítsa el az alábbi sorokat, ha vannak ilyenek hello:
   
     Alapértelmezés szerint targetpw # hello célként megadott felhasználó összes ALL=(ALL) összes azaz gyökér hello jelszó kérése # figyelmeztetés! Csak ezzel együtt a 'Alapértelmezett targetpw'!
9. Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.  Ez általában akkor hello alapértelmezett.
10. Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.
    
    hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után. Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén. Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben), módosítsa megfelelően a következő paraméterek a /etc/waagent.conf hello:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Megjegyzés: a szükséges toobe toowhatever beállítása.
11. Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:
    
    # <a name="sudo-waagent--force--deprovision"></a>sudo waagent-force - deprovision
    # <a name="export-histsize0"></a>exportálja a HISTSIZE = 0
    # <a name="logout"></a>Kijelentkezés
12. Ellenőrizze, hogy hello Azure Linux ügynök fut, a rendszer indításakor:
    
        # sudo systemctl enable waagent.service
13. Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében. A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.

## <a name="next-steps"></a>Következő lépések
Ön éppen most már készen áll a toouse a SUSE Linux virtuális merevlemez toocreate új virtuális gépeket az Azure-ban. Ha ez hello első alkalom, hogy feltöltendő hello .vhd fájl tooAzure, tekintse meg a 2. és 3 [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

