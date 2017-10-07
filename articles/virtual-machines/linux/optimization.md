---
title: "aaaOptimize a Linux virtuális Gépet az Azure-on |} Microsoft Docs"
description: "Ismerje meg, néhány Optimalizálási tippek toomake meg arról, hogy meg van adva a Linux virtuális Gépet az optimális teljesítmény érdekében az Azure-on"
keywords: "Linux virtuális gépek, virtuális gép linux ubuntu virtuális gép"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 8baa30c8-d40e-41ac-93d0-74e96fe18d4c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: rclaus
ms.openlocfilehash: 89a9ac022928a2801a9a15e1c172340352745354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-linux-vm-on-azure"></a>Linuxos virtuális gép optimalizálása az Azure-ban
A Linux virtuális gépek (VM) létrehozására könnyen toodo hello parancssorból vagy hello portálról. Az oktatóanyag bemutatja, hogyan tooensure beállítása azt toooptimize a teljesítményét hello a Microsoft Azure platformon. Ez a témakör az Ubuntu Server virtuális gép használja, de is létrehozhat Linux virtuális gép használt [a saját lemezképek sablonként](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="prerequisites"></a>Előfeltételek
Ez a témakör azt feltételezi, hogy már rendelkezik Azure-előfizetés ([ingyenes próbaidőszakra](https://azure.microsoft.com/pricing/free-trial/)) és a virtuális gép már létrehozta az Azure-előfizetése. Győződjön meg arról, hogy rendelkezik hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és az Azure-előfizetés tooyour bejelentkezett [az bejelentkezési](/cli/azure/#login) előtt [hozzon létre egy virtuális Gépet](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-os-disk"></a>Az Azure operációsrendszer-lemez
Miután létrehozott egy Linux virtuális Gépet az Azure-ban, két lemez társítva van. **/ dev/sda** az operációsrendszer-lemez van **/dev/sdb** az ideiglenes lemez.  Ne használjon hello fő operációsrendszer-lemez (**/dev/sda**) a hello operációs rendszer, mert kivételével bármely más virtuális gép gyors rendszerindítás van optimalizálva, és nem biztosítja a megfelelő teljesítmény a munkaterhelések. Azt szeretné, hogy egy tooattach, vagy további lemezterület tooyour VM tooget állandó és optimalizált az adatok tárolása. 

## <a name="adding-disks-for-size-and-performance-targets"></a>A lemezek hozzáadása a méret és Teljesítménycélok
Virtuálisgép-méretet hello alapján, csatolhat mentése too16 további egy A-sorozatú, a D sorozat lemezein 32 és 64 lemezek G-sorozat gépen - mindegyik mentése too1 TB-nál. Szükség esetén a hely és IOps követelmények hozzáadhat további lemezeket. Minden lemezhez tartozik 500 IOps teljesítményt célja standard szintű tárolót vagy magasabb too5000 iops-érték lemezenként a prémium szintű Storage.  Prémium szintű Storage lemezek kapcsolatos további információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást az Azure virtuális gépeken](../../storage/common/storage-premium-storage.md)

tooachieve hello prémium szintű Storage lemezeken, ahol a gyorsítótár-beállítások be tooeither legmagasabb IOps **ReadOnly** vagy **nincs**, le kell tiltania **korlátok** közben Linux hello fájlrendszert csatlakoztatni. Mivel hello írása tooPremium tárolási biztonsági lemezek tartós, a beállítások nem kell korlátok.

* Ha **reiserFS**, a letiltás hello csatlakoztatási beállítás használatával `barrier=none` (korlátok engedélyezése esetén használja a `barrier=flush`)
* Ha **ext3/ext4**, a letiltás hello csatlakoztatási beállítás használatával `barrier=0` (korlátok engedélyezése esetén használja a `barrier=1`)
* Ha **XFS**, a letiltás hello csatlakoztatási lehetőséggel `nobarrier` (engedélyezésének akadályok, használja a hello beállítást `barrier`)

## <a name="unmanaged-storage-account-considerations"></a>Nem felügyelt tárolási fiókokkal kapcsolatos megfontolások
hello alapértelmezett művelet, ha egy virtuális gép létrehozásához hello Azure CLI 2.0 toouse Azure felügyelt lemezek.  Ezeknek a lemezeknek hello Azure platform kezeli, és nem igényelnek bármely előkészítő vagy a hely toostore őket.  Nem felügyelt lemezek tárfiók szükséges, és néhány további teljesítmény szempontot kell figyelembe venni.  További információ a felügyelt lemezekről: [Azure Managed Disks – áttekintés](../windows/managed-disks-overview.md).  hello alábbi szakasz ismerteti teljesítménnyel kapcsolatos szempontok csak akkor, ha a nem felügyelt lemezek használja.  Ebben az esetben hello az alapértelmezett és ajánlott tárolási megoldás felügyelt toouse lemezek.

Ha egy virtuális Gépet hoz létre nem felügyelt lemezek, ellenőrizze, hogy csatlakoztassa a lemezeket a storage-fiókok hello élő azonos régióban legyen, mint a virtuális gép tooensure közelében, és a hálózati késés csökkentése érdekében érdemes.  Minden standard szintű tárfiók van maximum 20 IOps és 500 TB kapacitású mérete.  Ezt a határt beleértve hello operációsrendszer-lemez és bármely hoz létre az adatlemezek tooapproximately 40 fokozottan használt lemezek megfelelően működik. Prémium szintű Storage-fiókok maximális IOps korlátozva van, de nincs a egy 32 TB-os méretkorlátját. 

Ha a magas IOps-munkaterhelések és kezelésével standard szintű tárolót választotta a lemezen, szükség lehet toosplit hello lemezek több tároló fiókok toomake meg arról, hogy nem érte hello 20 000 IOps korlátozza, Standard szintű Storage-fiókok között. A virtuális Gépet tartalmazhatnak lemezei vegyesen különböző storage-fiókok és a tárolási fiók típusok tooachieve az optimális konfigurációját.
 

## <a name="your-vm-temporary-drive"></a>A virtuális gép ideiglenes meghajtó
Alapértelmezés szerint a virtuális gépek létrehozásakor Azure biztosít egy operációsrendszer-lemez (**/dev/sda**) és egy ideiglenes lemez (**/dev/sdb**).  Minden további lemezek megjelenítése mentése másként hozzáadása **/dev/sdc**, **/dev/sdd**, **/dev/sde** és így tovább. Ideiglenes tárolt összes adat (**/dev/sdb**) nincs tartós, és elvesznek, ha a meghatározott események, például a átméretezése virtuális gép újbóli üzembe helyezése, vagy egy karbantartási újraindítja a virtuális Gépet.  hello mérete és az ideiglenes lemez típusa a központi telepítéskor választott kapcsolódó toohello Virtuálisgép-méretet. Egy helyi SSD mentése too48k IOps teljesítésének további által támogatott összes hello prémium méretű virtuális gépek (DS, a G és DS_V2 sorozat) hello ideiglenes meghajtó. 

## <a name="linux-swap-file"></a>Linux lapozófájl
Ha az Azure virtuális gép Ubuntu vagy CoreOS lemezképből, majd használhatja CustomData toosend egy felhő-config toocloud inicializálás. Ha Ön [Linux egyéni lemezkép feltöltése](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) használó felhő inicializálás, is konfigurálhatja a felhő inicializálás használó swap partíciókkal.

Ubuntu felhő lemezképek, a felhő inicializálás tooconfigure hello swap partíció kell használnia. További információkért lásd: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

A lemezképek felhő inicializálás támogatás nélkül hello Azure piactér alapján telepített Virtuálisgép-rendszerképek van integrálva van az operációs rendszer hello VM Linux ügynök. Ez az ügynök lehetővé teszi, hogy a hello VM toointeract különböző Azure-szolgáltatásokkal. Feltéve, hogy telepítette a szabványos hello Azure Piactérről származó lemezkép, toodo kell a következő toocorrectly hello konfigurálása a Linux lapozófájl beállításait:

Keresse meg, és módosítsa a két bejegyzések hello **/etc/waagent.conf** fájlt. Dedikált lapozófájl hello megléte és hello lapozófájl méretétől azok ellenőrzése. keresett toomodify hello paraméterek `ResourceDisk.EnableSwap=N` és`ResourceDisk.SwapSizeMB=0` 

Hello paraméterek toohello a következő beállításokat módosíthatja:

* ResourceDisk.EnableSwap=Y
* A MB toomeet ResourceDisk.SwapSizeMB={size igényeinek} 

Ha módosítása hello létrejött, toorestart hello waagent kell, vagy indítsa újra a Linux virtuális gép tooreflect ezeket a módosításokat.  Hello módosításokat végrehajtották, és lapozófájl létrejött hello használatakor tudja `free` parancs tooview szabad terület. hello alábbi példa rendelkezik hello módosítása során létrejött 512MB lapozófájl **waagent.conf** fájlt:

```bash
azuseruser@myVM:~$ free
            total       used       free     shared    buffers     cached
Mem:       3525156     804168    2720988        408       8428     633192
-/+ buffers/cache:     162548    3362608
Swap:       524284          0     524284
```

## <a name="io-scheduling-algorithm-for-premium-storage"></a>Prémium szintű Storage i/o ütemezési algoritmus
A hello 2.6.18 Linux kernel hello alapértelmezett i/o-ütemezési algoritmus határidő tooCFQ (teljesen valós Üzenetsor-kezelés algoritmus) értékűre változott. Közvetlen elérésű i/o-mintára van elhanyagolható teljesítmény különbségek CFQ és a határidő közötti különbség.  Ahol hello lemez i/o-mintát túlnyomórészt az SSD-alapú lemezek szekvenciális vált vissza toohello NOOP vagy határidő algoritmus érhető el jobb i/o-teljesítmény.

### <a name="view-hello-current-io-scheduler"></a>Nézet hello aktuális i/o-ütemező
A következő parancs hello használata:  

```bash
cat /sys/block/sda/queue/scheduler
```

Megjelenik a következő kimeneti, amely megadja, hogy a jelenlegi Feladatütemező hello.  

```bash
noop [deadline] cfq
```

### <a name="change-hello-current-device-devsda-of-io-scheduling-algorithm"></a>Hello aktuális eszköz (/ dev/sda) i/o algoritmus ütemezés módosítása
A következő parancsok hello használata:  

```bash
azureuser@myVM:~$ sudo su -
root@myVM:~# echo "noop" >/sys/block/sda/queue/scheduler
root@myVM:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
root@myVM:~# update-grub
```

> [!NOTE]
> Ez a beállítás alkalmazása **/dev/sda** különálló nincs hasznos. Minden adatlemezek beállítani ahol szekvenciális i/o egység dominálja hello i/o-mintát.  

A következő kimeneti, amely jelzi, hogy hello kell megjelennie **grub.cfg** sikeresen újraépítve, és adott hello alapértelmezett Feladatütemező frissített tooNOOP.  

```bash
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.13.0-34-generic
Found initrd image: /boot/initrd.img-3.13.0-34-generic
Found linux image: /boot/vmlinuz-3.13.0-32-generic
Found initrd image: /boot/initrd.img-3.13.0-32-generic
Found memtest86+ image: /memtest86+.elf
Found memtest86+ image: /memtest86+.bin
done
```

Hello Redhat terjesztési termékcsalád csak van szüksége a következő parancs hello:   

```bash
echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local
```

## <a name="using-software-raid-tooachieve-higher-iops"></a>Használatával a szoftverek RAID tooachieve magasabb I / Ops
Ha a munkaterhelések magasabb iops-értéket, mint egyetlen lemez biztosíthat igényelnek, meg kell toouse egy szoftver több lemez, RAID-konfigurációt. Azure már lemez rugalmassági hello helyi háló rétegben hajt végre, mert hello legmagasabb szintű RAID-0 csíkozást konfigurációról teljesítmény elérése érdekében.  Kiépítése és hello Azure környezetben a lemezek létrehozásához, majd csatolja azokat tooyour Linux virtuális gép particionálás, formázás és csatlakoztatása meghajtók hello előtt.  További részleteket a RAID Szoftvertelepítés konfigurálása a Linux virtuális Gépet az azure-ban található hello  **[szoftver RAID konfigurálása Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**  dokumentum.

## <a name="next-steps"></a>Következő lépések
Ne feledje, hogy az összes optimalizálási vitafórum tooperform vizsgálatok előtt kell, és minden módosítás toomeasure hello hatás után hello módosítás rendelkezik.  Optimalizálás rendkívül részletes folyamat, amely rendelkezik a különböző eredmények a környezetben az egyes számítógépek közötti.  Mi működik egy konfiguráció mások esetében nem használható.

Néhány hasznos hivatkozások tooadditional erőforrások: 

* [Premium Storage: Nagy teljesítményű tárolási szolgáltatás Azure-beli virtuális gépek számítási feladataihoz](../../storage/common/storage-premium-storage.md)
* [Azure Linux ügynök felhasználói útmutatója](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Az Azure Linux virtuális gépeken futó MySQL teljesítményének optimalizálása](classic/optimize-mysql.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Szoftveres RAID Linux konfigurálása](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

