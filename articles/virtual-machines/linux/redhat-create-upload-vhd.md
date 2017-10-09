---
title: "aaaCreate, és töltse fel a Red Hat Enterprise Linux virtuális merevlemez használata az Azure-ban |} Microsoft Docs"
description: "Toocreate ismerje meg, és töltse fel az Azure virtuális merevlemez (VHD) a Red Hat Linux operációs rendszert tartalmazó."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 6c6b8f72-32d3-47fa-be94-6cb54537c69f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/28/2017
ms.author: szark
ms.openlocfilehash: bdd1bbfbee55b5cc61d69a09b06b6bd2c3de362b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Red Hat-alapú virtuális gép előkészítése Azure-beli használatra
Ebből a cikkből megtudhatja, hogyan használható az Azure-ban Red Hat Enterprise Linux (RHEL) virtuális gép tooprepare. hello verziói RHEL ebben a cikkben ismertetett 6.7 + és 7.1 +. hello hipervizorok, ebben a cikkben ismertetett előkészítéséhez a Hyper-V, a kernel-alapú virtuális gép (KVM), és a VMware. Red Hat Felhőelérést programban való részvételre vonatkozó jogosultság követelményeivel kapcsolatos további információkért lásd: [Red Hat Felhőelérést webhely](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) és [az Azure-on futó RHEL](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>A Hyper-V kezelőjéből a Red Hat-alapú virtuális gép előkészítése

### <a name="prerequisites"></a>Előfeltételek
Ez a szakasz azt feltételezi, hogy ha már beszerezte az ISO-fájl hello Red Hat webhely és a telepített hello RHEL kép tooa virtuális merevlemez (VHD). További részletes információt toouse Hyper-V kezelője tooinstall az operációs rendszer lemezképét, lásd: [hello Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](http://technet.microsoft.com/library/hh846766.aspx).

**RHEL telepítési megjegyzések**

* Azure nem támogatja a hello VHDX formátumú. Az Azure által támogatott csak rögzített VHD-t. Használhatja a Hyper-V kezelője tooVHD tooconvert hello lemezformátumot, vagy hello convert-vhd parancsmagot használhatja. Ha VirtualBox használja, jelölje be **mérete rögzített** mint toohello alapértelmezett számára dinamikusan kiosztható beállítás hello lemezek létrehozása során.
* Azure csak az 1. generációs virtuális gépek támogatja. 1. generációs virtuális gépek VHDX toohello VHD formátumú és eltávolítása dinamikusan bővülő tooa rögzített méretű lemez válthat. Nem módosíthatja a virtuális gép generációját. További információkért lásd: [érdemes létrehozni 1 vagy 2. generációs virtuális gépek a Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
* hello maximális megengedett hello virtuális merevlemez mérete 1,023 GB.
* Hello Linux operációs rendszer telepítésekor azt javasoljuk, hogy Ön normál partíció helyett használjon logikai kötet Manager (LVM), ez utóbbi érték gyakran hello alapértelmezett sok telepítés. Ezzel a gyakorlattal elkerülheti LVM neve ütközik a klónozott virtuális gépek, különösen akkor, ha az operációs rendszer lemez tooanother azonos virtuális gép tooattach hibaelhárítási átállítására lenne szükség. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) adatlemezek használható.
* Univerzális lemez formátum (UDF) fájlrendszerek csatlakoztatására kernel támogatására szükség. Az Azure, hello UDF formátumú adathordozón, amely első rendszerindításkor csatolt toohello Vendég hello létesítési konfiguráció toohello Linux virtuális gép továbbítja. hello Azure Linux ügynök kell tudni toomount hello UDF fájl rendszer tooread konfigurációját és rendszerű hello virtuális gép.
* Hello Linux kernel 2.6.37 verziónál korábbi verziói nem támogatják nem egységes memóriaelérés (NUMA) a Hyper-V a nagyobb virtuális gépek méretét. A probléma főként hatások régebbi azokat a terjesztéseket, amelyek hello előtt Red Hat 2.6.32 kernel és javítását a RHEL 6.6 (kernel-2.6.32-504). Egyéni kernelek futtatják, amelyek régebbiek 2.6.37 vagy RHEL-alapú kernelek 2.6.32-504 régebbi értéke hello `numa=off` paraméter rendszerindító hello kernel parancssorában grub.conf. További információkért tekintse meg a Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Ne konfiguráljon egy swap partíció hello operációsrendszer-lemez. Linux-ügynök hello konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet.  További információ található a következő lépéseket hello.
* Minden virtuális merevlemez, amely többszörösei 1 MB méretű kell rendelkeznie.

### <a name="prepare-a-rhel-6-virtual-machine-from-hyper-v-manager"></a>A Hyper-V kezelőjéből a RHEL 6 virtuális gép előkészítése

1. A Hyper-V kezelőjében válassza ki a hello virtuális gépet.

2. Kattintson a **Connect** tooopen a konzolablakban hello virtuális géphez.

3. Az RHEL 6 NetworkManager zavarhatja hello Azure Linux ügynök. Ez a csomag eltávolítása hello a következő parancs futtatásával:
   
        # sudo rpm -e --nodeps NetworkManager

4. Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network` fájlt, és adja hozzá a következő szöveg hello:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network-scripts/ifcfg-eth0` fájlt, és adja hozzá a következő szöveg hello:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. Helyezze át vagy távolítsa el hello udev szabályok tooavoid hello Ethernet-adapter statikus szabályok létrehozása. Ezek a szabályok problémákat okozhat, ha a Microsoft Azure- vagy Hyper-v egy virtuális gép klónozását

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Győződjön meg arról, hogy hello hálózati szolgáltatás rendszerindításkor megkezdődik hello a következő parancs futtatásával:

        # sudo chkconfig network on

8. A Red Hat előfizetés tooenable hello telepített RHEL adattárból hello csomagok regisztrálása hello a következő parancs futtatásával:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9. hello WALinuxAgent csomag, `WALinuxAgent-<version>`, toohello Red Hat kiegészítő funkciók tárház lett leküldve. Engedélyezze hello kiegészítő funkciók tárház hello a következő parancs futtatásával:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. toodo módosítást, nyissa meg `/boot/grub/menu.lst` egy szövegszerkesztőben, és győződjön meg arról, hogy hello alapértelmezett kernel tartalmazza a következő paraméterek hello:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Ez biztosítja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.
    
    Ezenkívül azt javasoljuk, hogy távolítsa el a következő paraméterek hello:
    
        rhgb quiet crashkernel=auto
    
    Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.  Meghagyhatja hello `crashkernel` beállítás konfigurálva, ha szükséges. Vegye figyelembe, hogy ez a paraméter csökkenti a hello rendelkezésre álló memória mennyiségétől hello virtuális gépen legalább 128 MB. Ez a konfiguráció kisebb virtuálisgép-méretek a problémás lehet.

    >[!Important]
    RHEL 6.5-ös vagy korábbi is be kell állítani hello `numa=off` kernel paraméter. Tekintse meg a Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

11. Győződjön meg arról, hogy hello secure shell (SSH) kiszolgáló telepítve van, és rendszerindítás, amely általában hello alapértelmezett toostart konfigurálva. Módosítsa a következő sor /etc/ssh/sshd_config tooinclude hello:

        ClientAliveInterval 180

12. Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

    Telepítés hello WALinuxAgent csomag hello NetworkManager és NetworkManager-gnome csomagok eltávolítja, ha nem már eltávolította azokat a 3. lépésben.

13. Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.

    hello Azure Linux ügynök automatikusan konfigurálhatja lapozóterület hello helyi erőforrás lemezen, amely virtuális géphez csatolt toohello Azure hello virtuális gép kiépítése után. Vegye figyelembe, hogy helyi erőforrás egy ideiglenes lemez és, hogy, előfordulhat, hogy ki kell üríteni amikor hello virtuális gép van platformelőfizetés hello. Telepítése után hello Azure Linux ügynök hello előző lépésben, módosítsa megfelelően a következő paraméterek a /etc/waagent.conf hello:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

14. Hello előfizetés regisztrációját (ha szükséges) hello a következő parancs futtatásával:

        # sudo subscription-manager unregister

15. Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

16. Kattintson a **művelet** > **leállítása** a Hyper-V kezelőjében. A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.


### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>A Hyper-V kezelőjéből a RHEL 7 virtuális gép előkészítése

1. A Hyper-V kezelőjében válassza ki a hello virtuális gépet.

2. Kattintson a **Connect** tooopen a konzolablakban hello virtuális géphez.

3. Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network` fájlt, és adja hozzá a következő szöveg hello:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network-scripts/ifcfg-eth0` fájlt, és adja hozzá a következő szöveg hello:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. Győződjön meg arról, hogy hello hálózati szolgáltatás rendszerindításkor megkezdődik hello a következő parancs futtatásával:

        # sudo chkconfig network on

6. A Red Hat előfizetés tooenable hello telepített RHEL adattárból hello csomagok regisztrálása hello a következő parancs futtatásával:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. toodo módosítást, nyissa meg `/etc/default/grub` egy szövegszerkesztőben, és a Szerkesztés hello `GRUB_CMDLINE_LINUX` paraméter. Példa:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Ez biztosítja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására. Ez a konfiguráció is kikapcsolja hello új RHEL 7 elnevezési konvenciói a hálózati adapterek. Emellett javasoljuk, hogy távolítsa el a következő paraméterek hello:
   
        rhgb quiet crashkernel=auto
   
    Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak. Meghagyhatja hello `crashkernel` beállítás konfigurálva, ha szükséges. Vegye figyelembe, hogy ez a paraméter csökkenti hello rendelkezésre álló memória mennyiségétől hello virtuális gépen legalább 128 MB, amelyek kisebb virtuálisgép-méretek a problémás lehet.

8. Miután befejezte az szerkesztési `/etc/default/grub`- ben futtassa hello következő toorebuild hello lárvajárat konfigurálása:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9. Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás, amely általában hello alapértelmezett toostart konfigurálva. Módosítsa `/etc/ssh/sshd_config` tooinclude hello a következő sort:

        ClientAliveInterval 180

10. hello WALinuxAgent csomag, `WALinuxAgent-<version>`, toohello Red Hat kiegészítő funkciók tárház lett leküldve. Engedélyezze hello kiegészítő funkciók tárház hello a következő parancs futtatásával:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

12. Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.

    hello Azure Linux ügynök automatikusan konfigurálhatja lapozóterület hello helyi erőforrás lemezen, amely virtuális géphez csatolt toohello Azure hello virtuális gép kiépítése után. Vegye figyelembe, hogy hello helyi erőforrás egy ideiglenes lemez, és előfordulhat, hogy lehet üríteni, megszüntetett hello virtuális gép esetén. Telepítése után hello Azure Linux ügynök az előző lépésben hello, módosítsa a következő paramétereket a hello `/etc/waagent.conf` megfelelően:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. Toounregister hello előfizetés tetszés szerint futtassa a következő parancs hello:

        # sudo subscription-manager unregister

14. Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. Kattintson a **művelet** > **leállítása** a Hyper-V kezelőjében. A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Készítse elő a Red Hat-alapú virtuális gépek KVM a
### <a name="prepare-a-rhel-6-virtual-machine-from-kvm"></a>A KVM RHEL 6 virtuális gép előkészítése

1. Töltse le az RHEL 6 hello KVM képe hello Red Hat webhelyről.

2. Legfelső szintű jelszót állíthat be.

    Egy titkosított jelszót létrehozni, és másolja a hello parancs kimenetében hello:

        # openssl passwd -1 changeme

    Állítsa be a guestfish egy gyökérszintű jelszót:
        
        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Változás hello második mezője hello gyökér szintű felhasználó a "!" toohello titkosított jelszót.

3. Hozzon létre egy virtuális gép KVM hello qcow2 lemezképéről. Hello lemez típusának beállítása túl**qcow2**, és állítsa be a virtuális hálózati illesztő eszközmodell hello túl**virtio**. Ezt követően indítsa el a hello virtuális gép, és jelentkezzen be rendszergazdaként.

4. Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network` fájlt, és adja hozzá a következő szöveg hello:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network-scripts/ifcfg-eth0` fájlt, és adja hozzá a következő szöveg hello:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. Helyezze át vagy távolítsa el hello udev szabályok tooavoid hello Ethernet-adapter statikus szabályok létrehozása. Ezek a szabályok problémákat okozhat, ha az Azure- vagy Hyper-v egy virtuális gép klónozását

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Győződjön meg arról, hogy hello hálózati szolgáltatás rendszerindításkor megkezdődik hello a következő parancs futtatásával:

        # chkconfig network on

8. A Red Hat előfizetés tooenable hello telepített RHEL adattárból hello csomagok regisztrálása hello a következő parancs futtatásával:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9. Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. toodo ezt a konfigurációt, nyissa meg `/boot/grub/menu.lst` egy szövegszerkesztőben, és győződjön meg arról, hogy hello alapértelmezett kernel tartalmazza a következő paraméterek hello:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Ez biztosítja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.
    
    Emellett javasoljuk, hogy távolítsa el a következő paraméterek hello:
    
        rhgb quiet crashkernel=auto
    
    Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak. Meghagyhatja hello `crashkernel` beállítás konfigurálva, ha szükséges. Vegye figyelembe, hogy ez a paraméter csökkenti hello rendelkezésre álló memória mennyiségétől hello virtuális gépen legalább 128 MB, amelyek kisebb virtuálisgép-méretek a problémás lehet.

    >[!Important]
    RHEL 6.5-ös vagy korábbi is be kell állítani hello `numa=off` kernel paraméter. Tekintse meg a Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

10. Adja hozzá a Hyper-V modulok tooinitramfs:  

    Szerkesztés `/etc/dracut.conf`, és adja hozzá a tartalom a következő hello:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Építse újra initramfs:

        # dracut -f -v

11. Távolítsa el a felhő inicializálás:

        # yum remove cloud-init

12. Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva:

        # chkconfig sshd on

    Módosítsa az alábbi /etc/ssh/sshd_config tooinclude hello:

        PasswordAuthentication yes
        ClientAliveInterval 180

13. hello WALinuxAgent csomag, `WALinuxAgent-<version>`, toohello Red Hat kiegészítő funkciók tárház lett leküldve. Engedélyezze hello kiegészítő funkciók tárház hello a következő parancs futtatásával:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:

        # yum install WALinuxAgent

        # chkconfig waagent on

15. hello Azure Linux ügynök automatikusan konfigurálhatja lapozóterület hello helyi erőforrás lemezen, amely virtuális géphez csatolt toohello Azure hello virtuális gép kiépítése után. Vegye figyelembe, hogy hello helyi erőforrás egy ideiglenes lemez, és előfordulhat, hogy lehet üríteni, megszüntetett hello virtuális gép esetén. Telepítése után hello Azure Linux ügynök az előző lépésben hello, módosítsa a következő paramétereket a hello **/etc/waagent.conf** megfelelően:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. Hello előfizetés regisztrációját (ha szükséges) hello a következő parancs futtatásával:

        # subscription-manager unregister

17. Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:

        # waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. Állítsa le a virtuális gép hello KVM.

19. Az átalakítás hello qcow2 kép toohello VHD formátummal.

    Először alakítsa át hello tooraw képformátum:

        # qemu-img convert -f qcow2 -O raw rhel-6.8.qcow2 rhel-6.8.raw

    Győződjön meg arról, hogy hello nyers lemezkép hello mérete 1 MB igazodik. Ellenkező esetben kerekíteni hello mérete tooalign 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Alakítsa át a nyers tooa hello rögzített méretű virtuális merevlemez:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd


### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a>A KVM RHEL 7 virtuális gép előkészítése

1. Töltse le az RHEL 7 hello KVM képe hello Red Hat webhelyről. Ez az eljárás RHEL 7 hello példaként használja.

2. Legfelső szintű jelszót állíthat be.

    Egy titkosított jelszót létrehozni, és másolja a hello parancs kimenetében hello:

        # openssl passwd -1 changeme

    Állítsa be a guestfish egy gyökérszintű jelszót:

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Változás hello második mezője gyökér szintű felhasználó a "!" toohello titkosított jelszót.

3. Hozzon létre egy virtuális gép KVM hello qcow2 lemezképéről. Hello lemez típusának beállítása túl**qcow2**, és állítsa be a virtuális hálózati illesztő eszközmodell hello túl**virtio**. Ezt követően indítsa el a hello virtuális gép, és jelentkezzen be rendszergazdaként.

4. Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network` fájlt, és adja hozzá a következő szöveg hello:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network-scripts/ifcfg-eth0` fájlt, és adja hozzá a következő szöveg hello:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

6. Győződjön meg arról, hogy hello hálózati szolgáltatás rendszerindításkor megkezdődik hello a következő parancs futtatásával:

        # chkconfig network on

7. A Red Hat előfizetés tooenable telepített RHEL adattárból hello csomagok regisztrálása hello a következő parancs futtatásával:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8. Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. toodo ezt a konfigurációt, nyissa meg `/etc/default/grub` egy szövegszerkesztőben, és a Szerkesztés hello `GRUB_CMDLINE_LINUX` paraméter. Példa:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Ez a parancs is biztosítja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására. Hello parancsot is a hálózati adapterek hello új RHEL 7 elnevezési konvenciókat kikapcsolása. Emellett javasoljuk, hogy távolítsa el a következő paraméterek hello:
   
        rhgb quiet crashkernel=auto
   
    Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak. Meghagyhatja hello `crashkernel` beállítás konfigurálva, ha szükséges. Vegye figyelembe, hogy ez a paraméter csökkenti hello rendelkezésre álló memória mennyiségétől hello virtuális gépen legalább 128 MB, amelyek kisebb virtuálisgép-méretek a problémás lehet.

9. Miután befejezte az szerkesztési `/etc/default/grub`- ben futtassa hello következő toorebuild hello lárvajárat konfigurálása:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Vegyen fel Hyper-V-modulokkal initramfs.

    Szerkesztés `/etc/dracut.conf` , és adja hozzá a tartalomhoz:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Építse újra initramfs:

        # dracut -f -v

11. Távolítsa el a felhő inicializálás:

        # yum remove cloud-init

12. Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva:

        # systemctl enable sshd

    Módosítsa az alábbi /etc/ssh/sshd_config tooinclude hello:

        PasswordAuthentication yes
        ClientAliveInterval 180

13. hello WALinuxAgent csomag, `WALinuxAgent-<version>`, toohello Red Hat kiegészítő funkciók tárház lett leküldve. Engedélyezze hello kiegészítő funkciók tárház hello a következő parancs futtatásával:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:

        # yum install WALinuxAgent

    Hello waagent szolgáltatás engedélyezése:

        # systemctl enable waagent.service

15. Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.

    hello Azure Linux ügynök automatikusan konfigurálhatja lapozóterület hello helyi erőforrás lemezen, amely virtuális géphez csatolt toohello Azure hello virtuális gép kiépítése után. Vegye figyelembe, hogy hello helyi erőforrás egy ideiglenes lemez, és előfordulhat, hogy lehet üríteni, megszüntetett hello virtuális gép esetén. Telepítése után hello Azure Linux ügynök az előző lépésben hello, módosítsa a következő paramétereket a hello `/etc/waagent.conf` megfelelően:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. Hello előfizetés regisztrációját (ha szükséges) hello a következő parancs futtatásával:

        # subscription-manager unregister

17. Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. Állítsa le a virtuális gép hello KVM.

19. Az átalakítás hello qcow2 kép toohello VHD formátummal.

    Először alakítsa át hello tooraw képformátum:

        # qemu-img convert -f qcow2 -O raw rhel-7.3.qcow2 rhel-7.3.raw

    Győződjön meg arról, hogy hello nyers lemezkép hello mérete 1 MB igazodik. Ellenkező esetben kerekíteni hello mérete tooalign 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Alakítsa át a nyers tooa hello rögzített méretű virtuális merevlemez:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>A VMware Red Hat-alapú virtuális gép előkészítése
### <a name="prerequisites"></a>Előfeltételek
Ez a szakasz azt feltételezi, hogy még telepítette RHEL virtuális gép VMware-ben. Hogyan tooinstall VMware, operációs rendszer: vonatkozó további információért [VMware vendég operációs rendszer telepítési útmutató](http://partnerweb.vmware.com/GOSIG/home.html).

* Hello Linux operációs rendszer telepítésekor azt javasoljuk, hogy Ön normál partíció helyett használjon LVM, ez utóbbi érték gyakran hello alapértelmezett sok telepítés. LVM neve ütközik a klónozott virtuális gépet, így elkerülhető, különösen akkor, ha a hibaelhárítási legalább egyszer kell csatolt toobe tooanother virtuális gép operációsrendszer-lemez. LVM vagy RAID használható adatlemezek Ha előnyben részesített.
* Ne konfiguráljon egy swap partíció hello operációsrendszer-lemez. Hello Linux ügynök toocreate hello ideiglenes erőforrás lemezen a lapozófájl konfigurálhatja. További információk a találhatók hello szükséges lépésekről.
* Hello virtuális merevlemez létrehozásakor válassza ki a **tároló virtuális lemez egyetlen fájlként**.

### <a name="prepare-a-rhel-6-virtual-machine-from-vmware"></a>A VMware RHEL 6 virtuális gép előkészítése
1. Az RHEL 6 NetworkManager zavarhatja hello Azure Linux ügynök. Ez a csomag eltávolítása hello a következő parancs futtatásával:
   
        # sudo rpm -e --nodeps NetworkManager

2. Hozzon létre egy fájlt **hálózati** hello etc/sysconfig/könyvtárban, amely tartalmazza a következő szöveg hello:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3. Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network-scripts/ifcfg-eth0` fájlt, és adja hozzá a következő szöveg hello:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4. Helyezze át vagy távolítsa el hello udev szabályok tooavoid hello Ethernet-adapter statikus szabályok létrehozása. Ezek a szabályok problémákat okozhat, ha az Azure- vagy Hyper-v egy virtuális gép klónozását

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

5. Győződjön meg arról, hogy hello hálózati szolgáltatás rendszerindításkor megkezdődik hello a következő parancs futtatásával:

        # sudo chkconfig network on

6. A Red Hat előfizetés tooenable hello telepített RHEL adattárból hello csomagok regisztrálása hello a következő parancs futtatásával:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. hello WALinuxAgent csomag, `WALinuxAgent-<version>`, toohello Red Hat kiegészítő funkciók tárház lett leküldve. Engedélyezze hello kiegészítő funkciók tárház hello a következő parancs futtatásával:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8. Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. toodo a, nyissa meg `/etc/default/grub` egy szövegszerkesztőben, és a Szerkesztés hello `GRUB_CMDLINE_LINUX` paraméter. Példa:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
   
   Ez biztosítja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására. Emellett javasoljuk, hogy távolítsa el a következő paraméterek hello:
   
        rhgb quiet crashkernel=auto
   
    Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak. Meghagyhatja hello `crashkernel` beállítás konfigurálva, ha szükséges. Vegye figyelembe, hogy ez a paraméter csökkenti hello rendelkezésre álló memória mennyiségétől hello virtuális gépen legalább 128 MB, amelyek kisebb virtuálisgép-méretek a problémás lehet.

9. Adja hozzá a Hyper-V modulok tooinitramfs:

    Szerkesztés `/etc/dracut.conf`, és adja hozzá a tartalom a következő hello:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Építse újra initramfs:

        # dracut -f -v

10. Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás, amely általában hello alapértelmezett toostart konfigurálva. Módosítsa `/etc/ssh/sshd_config` tooinclude hello a következő sort:

    ClientAliveInterval 180

11. Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

12. Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.

    hello Azure Linux ügynök automatikusan konfigurálhatja lapozóterület hello helyi erőforrás lemezen, amely virtuális géphez csatolt toohello Azure hello virtuális gép kiépítése után. Vegye figyelembe, hogy hello helyi erőforrás egy ideiglenes lemez, és előfordulhat, hogy lehet üríteni, megszüntetett hello virtuális gép esetén. Telepítése után hello Azure Linux ügynök az előző lépésben hello, módosítsa a következő paramétereket a hello `/etc/waagent.conf` megfelelően:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. Hello előfizetés regisztrációját (ha szükséges) hello a következő parancs futtatásával:

        # sudo subscription-manager unregister

14. Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. Hello virtuális gép leállítása és hello VMDK fájlt tooa .vhd-fájllá alakítsa át.

    Először alakítsa át hello tooraw képformátum:

        # qemu-img convert -f vmdk -O raw rhel-6.8.vmdk rhel-6.8.raw

    Győződjön meg arról, hogy hello nyers lemezkép hello mérete 1 MB igazodik. Ellenkező esetben kerekíteni hello mérete tooalign 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Alakítsa át a nyers tooa hello rögzített méretű virtuális merevlemez:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd

### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>A VMware RHEL 7 virtuális gép előkészítése
1. Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network` fájlt, és adja hozzá a következő szöveg hello:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2. Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network-scripts/ifcfg-eth0` fájlt, és adja hozzá a következő szöveg hello:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

3. Győződjön meg arról, hogy hello hálózati szolgáltatás rendszerindításkor megkezdődik hello a következő parancs futtatásával:

        # sudo chkconfig network on

4. A Red Hat előfizetés tooenable hello telepített RHEL adattárból hello csomagok regisztrálása hello a következő parancs futtatásával:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5. Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. toodo módosítást, nyissa meg `/etc/default/grub` egy szövegszerkesztőben, és a Szerkesztés hello `GRUB_CMDLINE_LINUX` paraméter. Példa:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Ez a konfiguráció is biztosítja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására. Azt is kikapcsolja hello új RHEL 7 elnevezési konvenciói a hálózati adapterek. Emellett javasoljuk, hogy távolítsa el a következő paraméterek hello:
   
        rhgb quiet crashkernel=auto
   
    Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak. Meghagyhatja hello `crashkernel` beállítás konfigurálva, ha szükséges. Vegye figyelembe, hogy ez a paraméter csökkenti hello rendelkezésre álló memória mennyiségétől hello virtuális gépen legalább 128 MB, amelyek kisebb virtuálisgép-méretek a problémás lehet.

6. Miután befejezte az szerkesztési `/etc/default/grub`- ben futtassa hello következő toorebuild hello lárvajárat konfigurálása:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7. Adja hozzá a Hyper-V modulok tooinitramfs.

    Szerkesztés `/etc/dracut.conf`, adja hozzá a tartalomhoz:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Építse újra initramfs:

        # dracut -f -v

8. Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva. Ez a beállítás általában hello alapértelmezett beállítás. Módosítsa `/etc/ssh/sshd_config` tooinclude hello a következő sort:

        ClientAliveInterval 180

9. hello WALinuxAgent csomag, `WALinuxAgent-<version>`, toohello Red Hat kiegészítő funkciók tárház lett leküldve. Engedélyezze hello kiegészítő funkciók tárház hello a következő parancs futtatásával:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

11. Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.

    hello Azure Linux ügynök automatikusan konfigurálhatja lapozóterület hello helyi erőforrás lemezen, amely virtuális géphez csatolt toohello Azure hello virtuális gép kiépítése után. Vegye figyelembe, hogy hello helyi erőforrás egy ideiglenes lemez, és előfordulhat, hogy lehet üríteni, megszüntetett hello virtuális gép esetén. Telepítése után hello Azure Linux ügynök az előző lépésben hello, módosítsa a következő paramétereket a hello `/etc/waagent.conf` megfelelően:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

12. Toounregister hello előfizetés tetszés szerint futtassa a következő parancs hello:

        # sudo subscription-manager unregister

13. Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

14. Hello virtuális gép leállítása, és alakítsa át a hello VMDK-fájl toohello VHD formátumú.

    Először alakítsa át hello tooraw képformátum:

        # qemu-img convert -f vmdk -O raw rhel-7.3.vmdk rhel-7.3.raw

    Győződjön meg arról, hogy hello nyers lemezkép hello mérete 1 MB igazodik. Ellenkező esetben kerekíteni hello mérete tooalign 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Alakítsa át a nyers tooa hello rögzített méretű virtuális merevlemez:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Készítse elő a Red Hat-alapú virtuális gép ISO-Lemezképet a automatikusan kickstart fájl használatával
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a>Kickstart fájlból RHEL 7 virtuális gép előkészítése

1.  Hozzon létre egy kickstart fájlt, amely tartalmazza a következő tartalmat hello, és mentse hello fájlt. Kickstart telepítésével kapcsolatos részletekért lásd: hello [Kickstart a telepítési útmutató](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
          auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run hello Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear hello MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down hello machine after install
        poweroff

        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable hello root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set hello cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build hello grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2. Helyezze a hello kickstart fájlt, ahol hello telepítési rendszer tudja-e érni.

3. A Hyper-V kezelőjében hozzon létre egy új virtuális gépet. A hello **virtuális merevlemez csatlakoztatása** lapon jelölje be **virtuális merevlemez csatlakoztatása később**, és a teljes hello új virtuális gép varázsló.

4. Nyissa meg a hello virtuális gép beállításait:

    a.  Rendeljen a virtuális merevlemez toohello új virtuális gépet. Győződjön meg arról, hogy tooselect **VHD formátumú** és **rögzített méretű**.

    b.  Hello telepítési ISO toohello DVD-meghajtó csatlakoztatása.

    c.  Állítsa be a hello BIOS tooboot CD-ről.

5. Hello virtuális gép elindításához. Amikor megjelenik a hello telepítési útmutatót, nyomja meg **lapon** tooconfigure hello indítási beállítások.

6. Adjon meg `inst.ks=<hello location of hello kickstart file>` hello indítási beállítások, és nyomja le az hello végén **Enter**.

7. Várjon, amíg hello telepítési toofinish. Amikor elkészült, a program automatikusan leáll hello virtuális gép. A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.

## <a name="known-issues"></a>Ismert problémák
### <a name="hello-hyper-v-driver-could-not-be-included-in-hello-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>hello Hyper-V illesztőprogram sikerült sem szerepelhet hello kezdeti RAM-lemez nem Hyper-V hipervizort használatakor

Bizonyos esetekben Linux telepítők nem hello illesztőprogramokat tartalmazhatnak Hyper-V a hello kezdeti RAM-lemez (initrd vagy initramfs) kivéve, ha a Linux észleli, hogy fut-e a Hyper-V környezetben.

Ha a Linux-lemezképet használ egy eltérő virtualizációs (Ez azt jelenti, hogy Virtualbox, Xen, stb.) a rendszer tooprepare, szükség lehet, hogy legalább hello hv_vmbus toorebuild initrd tooensure, és a hv_storvsc kernel modulok hello kezdeti RAM-lemez elérhető. Ez az egy ismert probléma legalább hello fölérendelt Red Hat terjesztési alapuló rendszereken.

tooresolve probléma, adja hozzá a Hyper-V modulok tooinitramfs, és azt újjáépítenie:

Szerkesztés `/etc/dracut.conf`, és adja hozzá a tartalom a következő hello:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

Építse újra initramfs:

        # dracut -f -v

További részletekért hello információk: [initramfs újraépítése](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Következő lépések
Ön éppen most már készen áll a toouse a Red Hat Enterprise Linux virtuális merevlemez toocreate új virtuális gépeket az Azure-ban. Ha ez hello első alkalom, hogy feltöltendő hello .vhd fájl tooAzure, tekintse meg a 2. és 3 [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

További információt, amelyek hello hipervizorok hitelesített toorun Red Hat Enterprise Linux, a következő témakörben: [hello Red Hat webhely](https://access.redhat.com/certified-hypervisors).
