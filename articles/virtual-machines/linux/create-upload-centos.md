---
title: "aaaCreate és feltöltése az Azure-ban CentOS alapú Linux virtuális"
description: "Toocreate megtudhatja, és töltse fel az Azure virtuális merevlemez (VHD), amely tartalmazza a CentOS-alapú Linux operációs rendszert."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 0e518e92-e981-43f4-b12c-9cba1064c4bb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 78f36be1d36e3d44cb836c912d143f2c9a72aab5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>CentOS-alapú virtuális gép előkészítése Azure-beli használatra
* [Az Azure-bA a CentOS 6.x virtuális gép előkészítése](#centos-6x)
* [Az Azure-bA a CentOS 7.0 + virtuális gép előkészítése](#centos-70)

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy a CentOS már telepítve van (vagy hasonló származtatott) Linux operációs rendszer tooa virtuális merevlemez. Több különféle eszköz toocreate .vhd fájlok, például egy hálózatvirtualizálási megoldás például a Hyper-V létezik. Útmutatásért lásd: [hello Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](http://technet.microsoft.com/library/hh846766.aspx).

**Telepítési jegyzetek centOS**

* Is lásd: [általános Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további információkat az Azure Linux előkészítése.
* hello VHDX formátum nem támogatott az Azure csak **rögzített VHD**.  Alakítsa át a Hyper-V kezelőjével hello tooVHD formátummal, vagy convert-vhd parancsmag hello. VirtualBox rendszer használata esetén ez azt jelenti, hogy kiválasztásával **mérete rögzített** , ellenezte toohello alapértelmezett dinamikusan lefoglalt hello lemez létrehozásakor.
* Ha telepíti a hello Linux rendszer *ajánlott* LVM (gyakran hello alapértelmezett sok telepítés), hanem szabványos partíciók használja. LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációsrendszer-lemez valaha is szüksége van a csatolt toobe tooanother hibaelhárítási azonos virtuális gép. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) adatlemezek használható.
* Kernel támogatása UDF fájlrendszerek csatlakoztatására szükség. Az Azure hello első rendszerindításkor létesítési konfiguráció lett átadva a toohello Linux virtuális gép UDF formátumú adathordozó csatolt toohello Vendég keresztül. hello Azure Linux ügynök legyen képes toomount hello UDF fájl rendszer tooread konfigurációját és hello virtuális gép kiépítéséhez.
* Linux kernel verziójánál régebbi 2.6.37 nem támogatott a Hyper-V nagyobb Virtuálisgép-méretek a. A probléma főként hatások régebbi azokat a terjesztéseket hello használata előtt Red Hat 2.6.32 kernel és javítását a RHEL 6.6 (kernel-2.6.32-504). Rendszerekre egyéni kernelek régebbi, mint 2.6.37 vagy régebbi RHEL alapú kernelek 2.6.32-504 kell beállítani, mint a rendszerindítási paraméter hello `numa=off` a hello kernel grub.conf a parancssori. További információ: a Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Ne konfiguráljon egy swap partíció hello operációsrendszer-lemezzel. Linux-ügynök hello konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet.  További információk a hello lépéseket találhatók.
* Az összes hello virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.

## <a name="centos-6x"></a>CentOS 6.x

1. A Hyper-V kezelőjében válassza ki a hello virtuális gépet.

2. Kattintson a **Connect** tooopen a konzolablakban hello virtuális géphez.

3. A CentOS 6 NetworkManager zavarhatja hello Azure Linux ügynök. Ez a csomag eltávolítása hello a következő parancs futtatásával:
   
        # sudo rpm -e --nodeps NetworkManager

4. Hozzon létre vagy szerkeszthet hello fájlt `/etc/sysconfig/network` , és adja hozzá a következő szöveg hello:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Hozzon létre vagy szerkeszthet hello fájlt `/etc/sysconfig/network-scripts/ifcfg-eth0` , és adja hozzá a következő szöveg hello:
   
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
   
        # sudo chkconfig network on

8. Ha szeretné toouse hello OpenLogic tükör hello Azure adatközpontjaiban belül futó, akkor cserélje le hello `/etc/yum.repos.d/CentOS-Base.repo` hello tárházak találhatók a következő fájl.  A művelet továbbá felvesz hello **[openlogic]** tárház, például a hello Azure Linux ügynök további csomagokat tartalmazza:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
        
        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    >[!Note]
    hello Ez az útmutató többi fogja feltételezni, hogy azok be legalább hello `[openlogic]` tárház, amely használt tooinstall hello Azure Linux ügynök az alábbi lesz.


9. Adja hozzá a következő sor too/etc/yum.conf hello:
    
        http_caching=packages

10. Futtassa a következő parancs tooclear hello aktuális yum metaadatok és a frissítési hello rendszer hello legújabb csomagokkal hello:
    
        # yum clean all

    Kivéve, ha a régebbi verziójú CentOS egy kép létrehozásakor tooupdate összes hello csomagok toohello legújabb ajánlott:

        # sudo yum -y update

    A parancs futtatása után a rendszer újraindítása lehet szükség.

11. (Választható) Telepítse a Linux integrációs szolgáltatások (LIS) hello hello illesztőprogramjait.
   
    >[!IMPORTANT]
    hello lépés **szükséges** a CentOS 6.3 és korábbi, és kötelező megadni a későbbi kibocsátásokban megtörténik.

        # sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
        # sudo yum install microsoft-hyper-v

    Azt is megteheti, kövesse a hello manuális telepítési utasításokat a hello [LIS letöltési oldalát](https://go.microsoft.com/fwlink/?linkid=403033) tooinstall hello RPM, a virtuális Gépet.
 
12. Hello Azure Linux ügynök és a függőségek telepítése:
    
        # sudo yum install python-pyasn1 WALinuxAgent
    
    hello WALinuxAgent csomag eltávolítja hello NetworkManager és NetworkManager-gnome csomagok, ha nem már eltávolította azokat a 3. lépésben leírtak szerint.


13. Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. toodo a, nyissa meg `/boot/grub/menu.lst` egy szövegszerkesztőben, és győződjön meg arról, hogy hello alapértelmezett kernel tartalmazza a következő paraméterek hello:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Ez biztosítja az összes konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.
    
    Továbbá a fenti toohello, ajánlott túl*eltávolítása* hello a következő paramétereket:
    
        rhgb quiet crashkernel=auto
    
    Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.  Hello `crashkernel` beállítás lehet, hogy szükség esetén konfigurálva balra, de vegye figyelembe, hogy ez a paraméter csökkenti a hello VM legalább 128 MB, rendelkezésre álló memória mennyisége hello esetleg problémás hello kisebb Virtuálisgép-méretek a.

    >[!Important]
    CentOS 6.5-ös vagy korábbi is be kell állítani hello kernel paraméter `numa=off`. Tekintse meg a Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

14. Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.  Ez általában akkor hello alapértelmezett.

15. Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.
    
    hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után. Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén. Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben), módosítsa a paramétereket a következő hello `/etc/waagent.conf` megfelelően:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében. A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.


- - -
## <a name="centos-70"></a>CentOS 7.0 +
**Változások a CentOS 7 (és hasonló származékai)**

A CentOS 7 virtuális gép előkészítése az Azure-nagyon hasonló tooCentOS 6, azonban nincsenek érdemes megjegyezni számos fontos különbség:

* hello NetworkManager csomag már nem ütközik az hello Azure Linux ügynök. Alapértelmezés szerint ez a csomag telepítve van, és azt javasoljuk, hogy nem törli.
* GRUB2 most használatos az alapértelmezett rendszertöltő hello így hello eljárás kernel paraméterek szerkesztésre megváltozott (lásd alább).
* XFS már hello alapértelmezett fájlrendszert. hello ext4 fájlrendszer továbbra is használható, ha szükséges.

**Konfigurációs lépések**

1. A Hyper-V kezelőjében válassza ki a hello virtuális gépet.

2. Kattintson a **Connect** tooopen a konzolablakban hello virtuális géphez.

3. Hozzon létre vagy szerkeszthet hello fájlt `/etc/sysconfig/network` , és adja hozzá a következő szöveg hello:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. Hozzon létre vagy szerkeszthet hello fájlt `/etc/sysconfig/network-scripts/ifcfg-eth0` , és adja hozzá a következő szöveg hello:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. Módosítsa a udev szabályok tooavoid hello Ethernet adaptert statikus szabályainak létrehozásakor. Ezek a szabályok problémákat okozhat, ha a Microsoft Azure- vagy Hyper-v virtuális gép klónozása
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Ha szeretné toouse hello OpenLogic tükör hello Azure adatközpontjaiban belül futó, akkor cserélje le hello `/etc/yum.repos.d/CentOS-Base.repo` hello tárházak találhatók a következő fájl.  A művelet továbbá felvesz hello **[openlogic]** tárház, amely tartalmazza az Azure Linux ügynök hello csomagok:
   
        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
        
        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

    >[!Note]
    hello Ez az útmutató többi fogja feltételezni, hogy azok be legalább hello `[openlogic]` tárház, amely használt tooinstall hello Azure Linux ügynök az alábbi lesz.

7. Futtassa a következő parancs tooclear hello aktuális yum metaadatok hello, és telepítse a frissítéseket:
   
        # sudo yum clean all

    Kivéve, ha a régebbi verziójú CentOS egy kép létrehozásakor tooupdate összes hello csomagok toohello legújabb ajánlott:

        # sudo yum -y update

    Lehet, hogy ez a parancs futtatása után a szükséges újraindítás.

8. Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. toodo a, nyissa meg `/etc/default/grub` az a szöveg-szerkesztő és a Szerkesztés hello `GRUB_CMDLINE_LINUX` paraméter, például:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Ez biztosítja az összes konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására. Azt is kikapcsolja hello új CentOS 7 elnevezési konvenciói a hálózati adapterek. Továbbá a fenti toohello, ajánlott túl*eltávolítása* hello a következő paramétereket:
   
        rhgb quiet crashkernel=auto
   
    Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak. Hello `crashkernel` beállítás lehet, hogy szükség esetén konfigurálva balra, de vegye figyelembe, hogy ez a paraméter csökkenti a hello VM legalább 128 MB, rendelkezésre álló memória mennyisége hello esetleg problémás hello kisebb Virtuálisgép-méretek a.

9. Miután a szerkesztési `/etc/default/grub` / fent, futtassa a hello toorebuild hello lárvajárat konfigurálása a következő:
   
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

10. Ha a hello lemezkép **VMWare, VirtualBox vagy KVM:** ellenőrizze, hogy a Hyper-V hello illesztőprogramok hello initramfs szerepelnek:
   
   Szerkesztés `/etc/dracut.conf`, adja hozzá a tartalomhoz:
   
        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”
   
   Építse újra hello initramfs:
   
        # sudo dracut –f -v

11. Hello Azure Linux ügynök és a függőségek telepítése:

        # sudo yum install python-pyasn1 WALinuxAgent
        # sudo systemctl enable waagent

12. Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.
   
   hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után. Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén. Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben), módosítsa a paramétereket a következő hello `/etc/waagent.conf` megfelelően:
   
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

## <a name="next-steps"></a>Következő lépések
Ön éppen most már készen áll a toouse a CentOS Linux virtuális merevlemez toocreate új virtuális gépeket az Azure-ban. Ha ez hello első alkalom, hogy feltöltendő hello .vhd fájl tooAzure, tekintse meg a 2. és 3 [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

