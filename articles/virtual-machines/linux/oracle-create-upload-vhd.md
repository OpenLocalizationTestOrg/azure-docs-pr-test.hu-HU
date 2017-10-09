---
title: "aaaCreate és az Oracle Linux virtuális merevlemez feltöltéséhez |} Microsoft Docs"
description: "Toocreate ismerje meg, és töltse fel az Azure virtuális merevlemez (VHD), amely tartalmazza az Oracle Linux operációs rendszer."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: dd96f771-26eb-4391-9a89-8c8b6d691822
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: szark
ms.openlocfilehash: be9cf284d7f5e7122a106506a343e53e9f1ac56e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Oracle Linux-alapú virtuális gép előkészítése Azure-beli használatra
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy az Oracle Linux operációs rendszer tooa virtuális merevlemez már telepítve van. Több különféle eszköz toocreate .vhd fájlok, például egy hálózatvirtualizálási megoldás például a Hyper-V létezik. Útmutatásért lásd: [hello Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="oracle-linux-installation-notes"></a>Oracle Linux telepítési megjegyzések
* Is lásd: [általános Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további információkat az Azure Linux előkészítése.
* Red Hat kompatibilis kernel Oracle és azok UEK3 (szoros vállalati Kernel) is támogatott a Hyper-V és az Azure. A legjobb eredmények elérése érdekében Felhívjuk meg arról, hogy tooupdate toohello legújabb kernel az Oracle Linux virtuális merevlemez előkészítése során.
* Oracle UEK2 nem támogatott a Hyper-V és az Azure, nem tartalmazza a szükséges hello illesztőprogramokat.
* hello VHDX formátum nem támogatott az Azure csak **rögzített VHD**.  Alakítsa át a Hyper-V kezelőjével hello tooVHD formátummal, vagy convert-vhd parancsmag hello.
* Hello Linux rendszer telepítésekor LVM (gyakran hello alapértelmezett sok telepítés), hanem szabványos partíciók használata ajánlott. LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációs rendszer lemezén legalább egyszer kell csatolt toobe tooanother VM hibaelhárításhoz. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ha előnyben részesített adatlemezek használható.
* NUMA nagyobb Virtuálisgép-méretek Linux kernel verziójánál régebbi 2.6.37 tooa hibája miatt nem támogatott. Ez elsősorban az azokat a terjesztéseket használatával hello fölérendelt piros hatások ki Hat 2.6.32 kernel. Manuális telepítés hello Azure Linux-ügynök (waagent) automatikusan letiltja a NUMA hello Linux kernel hello LÁRVAJÁRAT konfigurációja. További információk a hello lépéseket találhatók.
* Ne konfiguráljon egy swap partíció hello operációsrendszer-lemezzel. Linux-ügynök hello konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet.  További információk a hello lépéseket találhatók.
* Az összes hello virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.
* Győződjön meg arról, hogy hello `Addons` tárház engedélyezve van. Hello fájl szerkesztésével `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) vagy `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), és módosítsa a hello sor `enabled=0` túl`enabled=1` alatt **[ol6_addons]** vagy **[ol7_addons]** ezen fájl.

## <a name="oracle-linux-64"></a>Oracle Linux 6.4 +
Hello operációs rendszer hello virtuális gép toorun az Azure-ban az adott konfigurációs lépések elvégzése után.

1. Hyper-V kezelője hello középső ablaktáblában jelölje ki a hello virtuális gépet.
2. Kattintson a **Connect** tooopen hello ablak hello virtuális géphez.
3. Távolítsa el a NetworkManager hello a következő parancs futtatásával:
   
        # sudo rpm -e --nodeps NetworkManager
   
    **Megjegyzés:** hello csomag nincs telepítve, ha ez a parancs a hibaüzenettel meghiúsul. Ez várható.
4. Hozzon létre egy fájlt **hálózati** a hello `/etc/sysconfig/` hello a következő szöveget tartalmazó könyvtár:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. Hozzon létre egy fájlt **ifcfg-eth0** a hello `/etc/sysconfig/network-scripts/` hello a következő szöveget tartalmazó könyvtár:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
6. Módosítsa a udev szabályok tooavoid hello Ethernet adaptert statikus szabályainak létrehozásakor. Ezek a szabályok problémákat okozhat, ha a Microsoft Azure- vagy Hyper-v virtuális gép klónozása
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
7. Győződjön meg arról, hello hálózati szolgáltatás indul rendszerindítás hello a következő parancs futtatásával:
   
        # chkconfig network on
8. Telepítse a python-pyasn1 hello a következő parancs futtatásával:
   
        # sudo yum install python-pyasn1
9. Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. a nyitott toodo "/ boot/grub/menu.lst" egy szövegszerkesztőben, és győződjön meg arról, hogy hello alapértelmezett kernel tartalmazza a következő paraméterek hello:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   Ez biztosítja az összes konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására. Ezzel a lépéssel letiltja NUMA Oracle Red Hat kompatibilis kernel tooa hiba miatt.
   
   Továbbá a fenti toohello, ajánlott túl*eltávolítása* hello a következő paramétereket:
   
        rhgb quiet crashkernel=auto
   
   Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.
   
   Hello `crashkernel` beállítás lehet, hogy szükség esetén konfigurálva balra, de vegye figyelembe, hogy ez a paraméter csökkenti a hello VM legalább 128 MB, rendelkezésre álló memória mennyisége hello esetleg problémás hello kisebb Virtuálisgép-méretek a.
10. Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.  Ez általában akkor hello alapértelmezett.
11. Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával. hello legfrissebb verzió: 2.0.15-ös.
    
        # sudo yum install WALinuxAgent
    
    Ne feledje, hogy telepíteni hello WALinuxAgent csomag eltávolítja hello NetworkManager NetworkManager-gnome csomagok Ha nem már eltávolította azokat lásd a 2. lépés.
12. Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.
    
    hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után. Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén. Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben), módosítsa megfelelően a következő paraméterek a /etc/waagent.conf hello:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.
13. Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
14. Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében. A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.

- - -
## <a name="oracle-linux-70"></a>Oracle Linux 7.0 +
**Oracle Linux 7 változásai**

Az Oracle Linux 7 virtuális gép előkészítése az Azure-nagyon hasonló tooOracle Linux 6, azonban nincsenek érdemes megjegyezni számos fontos különbség:

* Red Hat kompatibilis kernel hello és Oracle UEK3 is támogatottak az Azure-ban.  hello UEK3 kernel ajánlott.
* hello NetworkManager csomag már nem ütközik az hello Azure Linux ügynök. Alapértelmezés szerint ez a csomag telepítve van, és azt javasoljuk, hogy nem törli.
* GRUB2 most használatos az alapértelmezett rendszertöltő hello így hello eljárás kernel paraméterek szerkesztésre megváltozott (lásd alább).
* XFS már hello alapértelmezett fájlrendszert. hello ext4 fájlrendszer továbbra is használható, ha szükséges.

**Konfigurációs lépések**

1. A Hyper-V kezelőjében válassza ki a hello virtuális gépet.
2. Kattintson a **Connect** tooopen a konzolablakban hello virtuális géphez.
3. Hozzon létre egy fájlt **hálózati** a hello `/etc/sysconfig/` hello a következő szöveget tartalmazó könyvtár:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. Hozzon létre egy fájlt **ifcfg-eth0** a hello `/etc/sysconfig/network-scripts/` hello a következő szöveget tartalmazó könyvtár:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. Módosítsa a udev szabályok tooavoid hello Ethernet adaptert statikus szabályainak létrehozásakor. Ezek a szabályok problémákat okozhat, ha a Microsoft Azure- vagy Hyper-v virtuális gép klónozása
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. Győződjön meg arról, hello hálózati szolgáltatás indul rendszerindítás hello a következő parancs futtatásával:
   
        # sudo chkconfig network on
7. Hello python-pyasn1 csomag telepítése hello a következő parancs futtatásával:
   
        # sudo yum install python-pyasn1
8. Futtassa a következő parancs tooclear hello aktuális yum metaadatok hello, és telepítse a frissítéseket:
   
        # sudo yum clean all
        # sudo yum -y update
9. Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. a nyitott "/ etc/alapértelmezett/lárvajárat" toodo az a szöveg-szerkesztő és a Szerkesztés hello `GRUB_CMDLINE_LINUX` paraméter, például:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Ez biztosítja az összes konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására. Azt is kikapcsolja hello új OEL 7 elnevezési konvenciói a hálózati adapterek. Továbbá a fenti toohello, ajánlott túl*eltávolítása* hello a következő paramétereket:
   
       rhgb quiet crashkernel=auto
   
   Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.
   
   Hello `crashkernel` beállítás lehet, hogy szükség esetén konfigurálva balra, de vegye figyelembe, hogy ez a paraméter csökkenti a hello VM legalább 128 MB, rendelkezésre álló memória mennyisége hello esetleg problémás hello kisebb Virtuálisgép-méretek a.
10. Miután a szerkesztési "/ etc/alapértelmezett/lárvajárat" / fent, futtassa a hello toorebuild hello lárvajárat konfigurálása a következő:
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.  Ez általában akkor hello alapértelmezett.
12. Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.
    
    hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után. Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén. Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben hello), módosítsa megfelelően a következő paraméterek a /etc/waagent.conf hello:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.
14. Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében. A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.

## <a name="next-steps"></a>Következő lépések
Ön éppen most már készen áll a toouse az Oracle Linux .vhd toocreate új virtuális gépek Azure-ban. Ha ez hello első alkalom, hogy feltöltendő hello .vhd fájl tooAzure, tekintse meg a 2. és 3 [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

