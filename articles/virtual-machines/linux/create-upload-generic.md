---
title: "aaaCreate és feltöltése az Azure Linux virtuális merevlemez"
description: "Toocreate ismerje meg, és töltse fel az Azure virtuális merevlemez (VHD) a Linux operációs rendszert tartalmazó."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: d351396c-95a0-4092-b7bf-c6aae0bbd112
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 208e15c035edb5520fc29ba8e4e71c42b041864d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="information-for-non-endorsed-distributions"></a>Nem támogatott disztribúciókkal kapcsolatos tudnivalók
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

hello Azure platformon SLA vonatkozik hello Linux operációs rendszert futtató, csak ha egy futó toovirtual gépek hello a [által támogatott disztribúciók](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) szolgál. Az összes Linux terjesztéseket, feltéve, hogy a hello Azure kép gyűjteménye, azokat a terjesztéseket, a szükséges konfigurációval hello záradékkal.

* [Az Azure - Linux által támogatott Disztribúciók](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [A Microsoft Azure Linux lemezképek támogatása](https://support.microsoft.com/kb/2941892)

Azure-on futó összes terjesztéseket toomeet Előfeltételek toohave egy alkalommal tooproperly hello futtatására számos kell.  Ez a cikk célja semmiképpen sem átfogó, amelyben minden terjesztési különbözik; és elég lehetséges, hogy akkor is, ha Ön feltételeknek megfelelő összes hello alábbi is szüksége lesz toosignificantly tudjon végezni a Linux rendszer tooensure, amely a megfelelően fut hello platformon.

Az az oka, hogy azt javasoljuk, hogy az egyik megkezdése a [Azure támogatott Disztribúciókkal Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) amikor lehetséges. hello következő cikkeket végigvezeti Önt hogyan tooprepare hello különböző által támogatott Azure által támogatott Linux-disztribúciók:

* **[CentOS-alapú Disztribúciók](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Ez a cikk többi hello általános útmutatást a Linux-disztribúció Azure-on futó összpontosítanak.

## <a name="general-linux-installation-notes"></a>Jelzi, hogy általános Linux rendszerhez – telepítés
* hello VHDX formátum nem támogatott az Azure csak **rögzített VHD**.  Alakítsa át a Hyper-V kezelőjével hello tooVHD formátummal, vagy convert-vhd parancsmag hello. VirtualBox rendszer használata esetén ez azt jelenti, hogy kiválasztásával **mérete rögzített** , ellenezte toohello alapértelmezett dinamikusan lefoglalt hello lemez létrehozásakor.
* Azure csak az 1. generációs virtuális gépek támogatja. 1 virtuális gép VHDX toohello VHD formátumú és eltávolítása dinamikusan bővülő tooa rögzített méretű lemez generáció válthat. De nem módosíthatja a virtuális gép generációját. További információkért lásd: [érdemes létrehozni 1 vagy 2. generációs virtuális gépek a Hyper-V?](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)
* maximális méret hello engedélyezett hello VHD 1,023 GB.
* Ha telepíti a hello Linux rendszer *ajánlott* LVM (gyakran hello alapértelmezett sok telepítés), hanem szabványos partíciók használja. LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációsrendszer-lemez valaha is szüksége van a csatolt toobe tooanother hibaelhárítási azonos virtuális gép. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) adatlemezek használható.
* Kernel támogatása UDF fájlrendszerek csatlakoztatására szükség. Az Azure hello első rendszerindításkor létesítési konfiguráció lett átadva a toohello Linux virtuális gép UDF formátumú adathordozó csatolt toohello Vendég keresztül. hello Azure Linux ügynök legyen képes toomount hello UDF fájl rendszer tooread konfigurációját és hello virtuális gép kiépítéséhez.
* Linux kernel verziójánál régebbi 2.6.37 nem támogatott a Hyper-V nagyobb Virtuálisgép-méretek a. A probléma főként hatások régebbi azokat a terjesztéseket hello használata előtt Red Hat 2.6.32 kernel és javítását a RHEL 6.6 (kernel-2.6.32-504). Rendszerekre egyéni kernelek régebbi, mint 2.6.37 vagy régebbi RHEL alapú kernelek 2.6.32-504 kell beállítani, mint a rendszerindítási paraméter hello `numa=off` a hello kernel grub.conf a parancssori. További információ: a Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Ne konfiguráljon egy swap partíció hello operációsrendszer-lemezzel. Linux-ügynök hello konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet.  További információk a hello lépéseket találhatók.
* Az összes hello virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.

### <a name="installing-kernel-modules-without-hyper-v"></a>Hyper-V nélkül kernel-modulok telepítése
Azure fut hello Hyper-V hipervizort, így Linux megköveteli, hogy az egyes kernel modulok telepítve vannak-e a rendelés toorun az Azure-ban. Ha egy virtuális Gépet, amely a Hyper-V kívül készült, hello Linux telepítők nem tartalmazhatnak hello illesztőprogramok Hyper-v a kezdeti ramdisk hello (initrd vagy initramfs) kivéve, ha azt észleli, hogy fut-e egy Hyper-V környezetben. Egy eltérő virtualizációs (azaz Virtualbox, KVM, stb.) a rendszer tooprepare a Linux-lemezkép használata esetén szükség lehet a toorebuild hello initrd tooensure legalább hello `hv_vmbus` és `hv_storvsc` kernel modulokra hello kezdeti ramdisk érhető el.  Ez az egy ismert probléma legalább hello fölérendelt Red Hat terjesztési alapú rendszereken.

hello mechanizmus a hello initrd vagy initramfs kép újjáépítését hello terjesztési függően változhat. A terjesztési dokumentációt, vagy hello megfelelő eljárás támogatása.  Íme egy példa a hogyan toorebuild hello hello segítségével initrd `mkinitrd` segédprogram:

Először készítsen biztonsági másolatot hello meglévő initrd lemezképet:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Ezután építse újra a hello hello initrd `hv_vmbus` és `hv_storvsc` kernel modulok:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Virtuális merevlemezek átméretezése
Az Azure virtuális merevlemez képek rendelkeznie kell egy virtuális mérete igazítva too1MB.  Általában a Hyper-v-vel létrehozott virtuális merevlemezeket már igazítását megfelelően.  Ha a virtuális merevlemez hello nincs megfelelően igazítva megfelelően akkor is megjelenhet egy hiba üzenet hasonló toohello a következő toocreate tett kísérlet során egy *kép* a virtuális merevlemezről:

    "hello VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. hello size must be a whole number (in MBs).”

tooremedy ez átméretezheti hello hello Hyper-V Manager konzol vagy hello használó virtuális gépek [átméretezési-VHD](http://technet.microsoft.com/library/hh848535.aspx) Powershell-parancsmagot.  Ha nem futnak Windows környezetben javasolt toouse qemu-img tooconvert (ha szükséges), és hello virtuális merevlemez átméretezése.

> [!NOTE]
> Egy ismert hiba van qemu-img verzió > = 2.2.1-en, amely egy nem megfelelően formázott virtuális merevlemez eredményez. hello hiba kijavítása QEMU 2.6. Ajánlott toouse qemu-img 2.2.0 vagy alsó vagy frissítés too2.6 vagy újabb verzióját. Hivatkozás: https://bugs.launchpad.net/qemu/+bug/1490611.
> 
> 

1. Közvetlenül eszközökkel például a virtuális merevlemez átméretezése hello `qemu-img` vagy `vbox-manage` azt eredményezheti, egy indíthatatlanná VHD-t.  Ezért ajánlott toofirst convert hello VHD tooa nyers lemezképet.  Ha Virtuálisgép-lemezkép hello már létre lett hozva nyers lemezképet (az egyes Hipervizorok, például a KVM hello alapértelmezett), majd előfordulhat, hogy kihagyja ezt a lépést:
   
       # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

2. Kiszámítja a hello szükséges virtuális mérete hello hello lemez kép tooensure mérete igazított too1MB.  a következő héjparancsfájlt bash hello ezzel segít a.  hello parancsfájl a "`qemu-img info`" toodetermine hello hello lemezképet virtuális méretét, és számítja ki a hello mérete toohello tovább 1 MB:
   
       rawdisk="MyLinuxVM.raw"
       vhddisk="MyLinuxVM.vhd"
   
       MB=$((1024*1024))
       size=$(qemu-img info -f raw --output json "$rawdisk" | \
              gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
   
       rounded_size=$((($size/$MB + 1)*$MB))
       echo "Rounded Size = $rounded_size"

3. Automatikus oszlopszélesség hello nyers lemez $rounded_size használja, mint a fenti parancsfájl hello beállítása:
   
       # qemu-img resize MyLinuxVM.raw $rounded_size

4. Most, alakítsa át a nyers hátsó tooa hello rögzített méretű virtuális merevlemez:
   
       # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd

   Vagy a verziójával qemu **2.6 +** közé tartozik a hello `force_size` lehetőséget:

       # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc MyLinuxVM.raw MyLinuxVM.vhd

## <a name="linux-kernel-requirements"></a>Linux Kernel követelmények
hello Hyper-V és az Azure Linux integrációs szolgáltatások (LIS) illesztőprogramjait közzé közvetlenül toohello fölérendelt Linux kernel. A Linux kernel közelmúltban (azaz 3.x) tartalmazó terjesztések ezeket az illesztőprogramokat elérhető már rendelkezik, vagy ellenkező esetben adja meg ezeket az illesztőprogramokat backported verziói a kernelek a.  Ezeket az illesztőprogramokat folyamatosan változik a felsőbb rétegbeli kernel hello az új javításokat és szolgáltatásokat, amikor lehetséges célszerű toorun egy [által támogatott terjesztési](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , amely tartalmazza ezeket a javításokat és frissítéseit.

Ha futtatja a Red Hat Enterprise Linux-verziók variant **6.0-6.3**, a Hyper-V akkor kell tooinstall hello legújabb LIS illesztőprogramokat. hello illesztőprogramok található [ezen a helyen](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). RHEL frissítésétől **6.4 +** (és származékai) hello LIS illesztőprogramok már érhetők el a hello kernel, és ezért nincs további telepítési csomag szükséges toorun ezekhez a rendszerekhez Azure vannak.

Ha egy egyéni kernel szükség, a rendszer ajánlott toouse újabb verziójára kernel (azaz **3.8 +**). Ezeket a felosztás vagy szállítók, akik a saját kernel karbantartása néhány elérhető lesz szükség tooregularly backport hello LIS illesztőprogramok hello fölérendelt kernel tooyour egyéni kernel.  Akkor is, ha már viszonylag új kernel-verziót futtat, ajánlott tookeep követése bármely előtt javítások hello LIS illesztőprogramok és backport azokat szükség szerint. hello hello LIS illesztőprogram forrásfájljainak helye elérhető hello [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) hello Linux kernel forrás fában fájlt:

    F:    arch/x86/include/asm/mshyperv.h
    F:    arch/x86/include/uapi/asm/hyperv.h
    F:    arch/x86/kernel/cpu/mshyperv.c
    F:    drivers/hid/hid-hyperv.c
    F:    drivers/hv/
    F:    drivers/input/serio/hyperv-keyboard.c
    F:    drivers/net/hyperv/
    F:    drivers/scsi/storvsc_drv.c
    F:    drivers/video/fbdev/hyperv_fb.c
    F:    include/linux/hyperv.h
    F:    tools/hv/

Egy nagyon minimális a következő javítások hello hello hiányában az előzőekben toocause problémák az Azure-on, és így ezek szerepelnie kell a hello kernel. Ebben a listában, de semmiképpen sem teljes körű teljes biztosít az összes disztribúciók számára:

* [ata_piix: lemezek Hyper-V toohello illesztőprogramok késleltető alapértelmezés szerint](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
* [storvsc: fiókot használja az átvitel közbeni csomagok hello VISSZAÁLLÍTÁSI elérési úton](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
* [storvsc: WRITE_SAME használatának elkerülése érdekében](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
* [storvsc: RAID és virtuális állomás adapter illesztőprogramjai azonos írási letiltása](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
* [storvsc: NULL mutatót javítás kereshet vissza.](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
* [storvsc: ring pufferbe hibákat eredményezhet i/o-rögzítés](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
* [scsi_sysfs: __scsi_remove_device dupla végrehajtásának elleni védelem érdekében](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)

## <a name="hello-azure-linux-agent"></a>hello Azure Linux ügynök
Hello [Azure Linux ügynök](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (waagent) szükség tooproperly rendszerű Linux virtuális gép az Azure-ban. Is hello letöltéséhez, problémák fájlt, vagy küldje el a lekérési kérelmek hello [Linux-ügynök GitHub-tárház](https://github.com/Azure/WALinuxAgent).

* Linux-ügynök hello hello Apache 2.0 licence szabadul fel. Terjesztések már nyújtanak RPM vagy deb csomagok hello ügynök, és így egyes esetekben ez telepíthetők és minimális erőfeszítéssel frissítve.
* hello Azure Linux ügynök szükséges Python v2.6 +.
* hello ügynök hello python-pyasn1 modul is szükséges. A legtöbb terjesztéseket adja meg, ez különálló csomagként telepíthető.
* Bizonyos esetekben hello Azure Linux-ügynök nem lehet NetworkManager kompatibilis. Azokat a terjesztéseket által biztosított hello RPM/Deb-csomagok számos NetworkManager ütközés toohello waagent csomag konfigurálása, és így is el fogja távolítani NetworkManager hello Linux ügynök csomag telepítését.

## <a name="general-linux-system-requirements"></a>Általános Linux rendszerre vonatkozó követelmények

* Hello kernel rendszerindító sort a következő paraméterek LÁRVAJÁRAT vagy GRUB2 tooinclude hello módosítása. Ez biztosítja az összes konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására:
  
        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300
  
    Ez biztosítja az összes konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.
  
    Továbbá a fenti toohello, ajánlott túl*eltávolítása* hello a következő paramétereket, ha vannak ilyenek:
  
        rhgb quiet crashkernel=auto
  
    Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak. Hello `crashkernel` beállítás lehet, hogy szükség esetén konfigurálva balra, de vegye figyelembe, hogy ez a paraméter csökkenti a hello VM legalább 128 MB, rendelkezésre álló memória mennyisége hello esetleg problémás hello kisebb Virtuálisgép-méretek a.

* Hello Azure Linux ügynök telepítése
  
    hello Azure Linux-ügynök szükség a Linux-lemezkép az Azure-on történő üzembe helyezéséhez.  Terjesztések hello ügynök RPM vagy Deb-csomagként adja meg (hello csomag neve általában "WALinuxAgent" vagy "walinuxagent").  hello ügynök is telepítheti manuálisan hello hello utasításait követve [Linux-ügynök útmutató](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

* Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.  Ez általában akkor hello alapértelmezett.

* Ne hozzon létre lapozóterület a hello operációsrendszer-lemez
  
    hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után. Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén. Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben), módosítsa megfelelően a következő paraméterek a /etc/waagent.conf hello:
  
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

* Utolsó lépésként futtassa a következő parancsok toodeprovision hello virtuális gép hello:
  
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
  
  > [!NOTE]
  > Virtualbox a következő hiba futtatása után hello jelenhetnek meg "waagent-force - deprovision": `[Errno 5] Input/output error`. Ez a hibaüzenet nem fontos, és figyelmen kívül lesz hagyva.
  > 
  > 

* Akkor lesz majd tooshut hello virtuális gépét le kell, és töltse fel a hello VHD tooAzure.

