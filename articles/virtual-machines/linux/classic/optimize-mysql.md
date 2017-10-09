---
title: "MySQL-teljesítmény Linux aaaOptimize |} Microsoft Docs"
description: "Megtudhatja, hogyan toooptimize MySQL futó Linux operációs rendszert futtató Azure virtuális géphez (VM)."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0c1c7fc5-a528-4d84-b65d-2df225f2233f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: ningk
ms.openlocfilehash: 9e6458723233721e06f30b9de33635d403eefcba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a>Az Azure Linux virtuális gépeken futó MySQL teljesítményének optimalizálása
Nincsenek számos tényező befolyásolja az Azure, mind a virtuális hardver kiválasztása és szoftverkonfigurációt MySQL teljesítményére. Ez a cikk foglalkozik, a tárolás, a rendszer és a Helyadatbázis-konfigurációk keresztül optimalizálás teljesítményét.

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager](../../../resource-manager-deployment-model.md) és klasszikus. Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. További információ a Linux virtuális gép optimalizálásokat hello Resource Manager modellt: [optimalizálhatja a Linux virtuális Gépet az Azure-on](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="utilize-raid-on-an-azure-virtual-machine"></a>Egy Azure virtuális gépen RAID használata
Storage egy hello kulcsfontosságú tényező, amely befolyásolja az adatbázis teljesítményének felhőalapú környezetben. Összehasonlított tooa egyetlen lemez, RAID keresztül párhuzamossági gyorsabb hozzáférést biztosíthat. További információkért lásd: [szabványos RAID-szintek](http://en.wikipedia.org/wiki/Standard_RAID_levels).   

Lemezes i/o-átviteli és az Azure-ban i/o-válaszidő RAID javítható ki. Labor tesztek megjelenítése, hogy a lemez i/o-átviteli meg kell duplázni és i/o-válaszidő csökkenthető fele átlagosan RAID lemezek hello száma nő (a két toofour négy tooeight, stb.) Ha. Lásd: [függelék](#AppendixA) részleteiről.  

Továbbá toodisk i/o, MySQL teljesítmény növeli a hello milyen RAID-szinteket növelésével.  Lásd: [B függelék](#AppendixB) részleteiről.  

Érdemes lehet tooconsider hello adatrészlet méretének. Általában még nagyobb adattömbméretet, nyílik meg alsó terhelés, különösen a nagy írási műveleteket. Azonban ha hello adatrészlet mérete túl nagy, adhat, amely megakadályozza, hogy kihasználja a RAID többletterhelést. hello jelenlegi alapértelmezett méret 512 KB, ami bizonyítása toobe optimális legáltalánosabb éles környezetekben. Lásd: [C függelék](#AppendixC) részleteiről.   

Nincsenek korlátozások is hozzáadhat más virtuális gépek típusainak hány lemezeken. Ezek a korlátozások részletes leírást talál [virtuális gép és felhő mérete](http://msdn.microsoft.com/library/azure/dn197896.aspx). De megadható tooset mentése RAID kevesebb lemezzel kell négy csatolt adatok lemezek toofollow hello RAID példa ebben a cikkben.  

Ez a cikk azt feltételezi, hogy már létrehozott egy Linux virtuális gép és a MYSQL telepítette és konfigurálta. A bevezetés további információkért lásd: hogyan tooinstall MySQL az Azure-on.  

### <a name="set-up-raid-on-azure"></a>Az Azure-on RAID beállítása
hello következő lépések bemutatják, hogyan toocreate RAID-Azure hello Azure-portál használatával. Is állíthatja be RAID Windows PowerShell-parancsfájlok használatával.
Ebben a példában azt konfigurálja RAID 0 négy lemezzel.  

#### <a name="add-a-data-disk-tooyour-virtual-machine"></a>Adatok lemez tooyour virtuális gép hozzáadása
Az hello Azure-portálon lépjen toohello irányítópult, és válassza ki a kívánt tooadd adatlemezt hello virtuális gép toowhich. Ebben a példában a hello virtuális gép olyan mysqlnode1.  

<!--![Virtual machines][1]-->

Kattintson a **lemezek** majd **új csatolása**.

![Lemez hozzáadása a virtuális gépek](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

Hozzon létre egy új 500 GB lemezterület. Győződjön meg arról, hogy **állomás gyorsítótár preferencia** értéke túl**nincs**.  Amikor végzett, kattintson a **OK**.

![Üres lemez csatolása](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


Ez hozzáadja egy üres lemez a virtuális gép. Ismételje meg ezt a három még többször, hogy négy adatlemezek a RAID.  

Megtekintheti a hozzáadott hello meghajtókat a hello virtuális gép hello kernel üzenet napló megtekintésével. Például toosee Ez az Ubuntu, a következő parancs használata hello:  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-hello-additional-disks"></a>RAID hozzon létre hello további lemezek
hello következő lépések bemutatják, hogyan túl[szoftveres RAID Linux konfigurálása](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> Ha hello XFS fájlrendszert használ, a végrehajtást hello RAID létrehozása után a következő lépéseket.
>
>

tooinstall XFS Debian, Ubuntu vagy Linux menta használata hello a következő parancsot:  

    apt-get -y install xfsprogs  

tooinstall XFS Fedora, CentOS vagy RHEL, használjon hello a következő parancsot:  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a>Állítson be egy új. tárolási elérési útja
A következő parancs tooset be egy új. tárolási elérési útja hello használata:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-hello-original-data-toohello-new-storage-path"></a>Másolás hello eredeti toohello új tároló elérési útja
A következő parancs toocopy toohello új tároló elérési útja hello használata:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-hello-data-disk"></a>Módosítsa az engedélyeket, MySQL hozzáférhet (olvasási és írási) hello adatlemez
Használja az alábbi parancs toomodify engedélyek hello:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-hello-disk-io-scheduling-algorithm"></a>Hello lemez i/o algoritmus ütemezés módosítása
Linux megvalósítja négy különböző ütemezése algoritmusok i/o:  

* NOOP algoritmus (nincs művelet)
* Határidő algoritmus (határidő)
* Teljesen valós Üzenetsor-kezelés algoritmus (CFQ)
* Keret időszak algoritmus (Anticipatory)  

A különböző alkalmazási helyzetek toooptimize teljesítmény különböző i/o-bejegyzéstípusait választhatja ki. Teljesen elérésű környezetben nincs hello CFQ és a határidő algoritmusok teljesítményének jelentős különbsége. Azt javasoljuk, hogy állítsa a hello MySQL adatbázis környezet tooDeadline stabilitását. Ha nagy mennyiségű szekvenciális i/o, CFQ csökkentheti a lemezek i/o műveleteinek teljesítménye.   

Az SSD és egyéb eszközökről NOOP vagy határidő érhető el hello alapértelmezett Feladatütemező jobb teljesítményt.   

Előzetes toohello kernel 2.5, hello alapértelmezett i/o algoritmus ütemezés határidő. Hello kernel 2.6.18 verziótól kezdődően a CFQ hello alapértelmezett i/o-ütemezési algoritmus inaktívvá vált.  Adja meg ezt a beállítást, kernel rendszerindítás közben, vagy hello rendszer futtatásakor dinamikusan módosítsa ezt a beállítást.  

hello a következő példa bemutatja, hogyan toocheck és a készlet hello alapértelmezett Feladatütemező toohello NOOP algoritmus hello Debian terjesztési termékcsalád.  

### <a name="view-hello-current-io-scheduler"></a>Nézet hello aktuális i/o-ütemező
tooview hello Feladatütemező futtassa a következő parancs hello:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

A következő kimeneti, amely megadja, hogy a jelenlegi Feladatütemező hello jelenik meg:  

    noop [deadline] cfq


### <a name="change-hello-current-device-devsda-of-hello-io-scheduling-algorithm"></a>Hello aktuális eszköz (/ dev/sda) hello i/o-ütemezési algoritmus módosítása
Futtassa a következő parancsok toochange hello aktuális eszköz hello:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> Beállítása ez /dev/sda csak akkor nem hasznos. Kell beállítani az összes adat lemez ahol hello adatbázis található.  
>
>

A következő kimeneti, jelezve, hogy grub.cfg sikeresen újraépítették, és adott hello alapértelmezett Feladatütemező lett frissített tooNOOP hello kell megjelennie:  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

A Red Hat terjesztési termékcsalád hello meg kell csak hello a következő parancsot:

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a>Fájl műveletek rendszerbeállításainak konfigurálására
Egy ajánlott toodisable hello *atime* hello fájlrendszerben funkciót. Atime hello utolsó hozzáférés időpontjának. Amikor egy fájl megnyitásakor, hello rendszer rekordjainak hello időbélyeg hello naplóban. Ezek az információk azonban igen ritkán alkalmazzák. Ha letiltja, ha nincs szüksége, amely csökkenti a teljes lemez hozzáférés idejét.  

toodisable atime naplózás szükségesek toomodify hello fájl rendszer konfigurációs fájl /etc/ fstab, és adja hozzá a hello **noatime** lehetőséget.  

Hello vim /etc/fstab fájl, hello noatime hozzáadása, ahogy az a következő minta hello például szerkesztése:  

    # CLOUD_IMG: This file was created/modified by hello Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add hello “noatime” option below toodisable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Majd csatlakoztassa újra hello fájlrendszer a hello a következő parancsot:  

    mount -o remount /RAID0

Teszt hello eredmény módosítani. Hello tesztfájl módosításakor hello hozzáférés időpontja nem frissül. hello a következő példák szemléltetik milyen hello kódot a következőképpen néz módosítás előtti és utáni.

Előtte:        

![Code access módosítás előtt][5]

Utána:

![Code access módosítás után][6]

## <a name="increase-hello-maximum-number-of-system-handles-for-high-concurrency"></a>Növelje a nagy egyidejű kezeli hello maximális számát
MySQL egy olyan nagy feldolgozási adatbázis. egyidejű leíróinak száma hello alapértelmezett érték 1024 Linux, amely nem mindig elegendő. A következő lépéseket tooincrease hello maximális párhuzamos leírókat hello rendszer toosupport magas egyidejűségi beállítása pedig MySQL hello használata.

### <a name="modify-hello-limitsconf-file"></a>Hello limits.conf fájl módosítása
tooincrease hello maximális engedélyezett egyidejű kezeli, vegye fel a következő négy hello /etc/security/limits.conf fájl sorainak hello. Vegye figyelembe, hogy a 65536 értékű hello rendszer által támogatott hello maximális számát.   

    * a 65536 értékű enyhe nofile
    * a 65536 értékű rögzített nofile
    * a 65536 értékű enyhe nproc
    * a 65536 értékű rögzített nproc

### <a name="update-hello-system-for-hello-new-limits"></a>Hello rendszert hello új korlátok
tooupdate hello rendszer hello a következő parancsokat:  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-hello-limits-are-updated-at-boot-time"></a>Győződjön meg arról, hogy hello korlátok rendszerindítás frissítése
Helyezze el a következő indítási parancsok hello /etc/rc.local fájlban, akkor lép érvénybe rendszerindítás hello.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a>MySQL-adatbázis optimalizálása
Azure-beli MySQL tooconfigure, használhatja az a helyi számítógépen, amelyekkel azonos teljesítményének hangolása stratégia hello.  

hello fő i/o-optimalizálás szabályok a következők:   

* Hello gyorsítótár méretének növelése.
* I/o-válaszidő csökkentése.  

toooptimize MySQL-kiszolgáló beállításait, frissítheti hello my.cnf fájl, amely hello alapértelmezett konfigurációs fájl a kiszolgáló és az ügyfélszámítógépek számára.  

hello következő konfigurációs elemek olyan hello fő tényezőt jelent a MySQL teljesítmény:  

* **innodb_buffer_pool_size**: hello pufferkészlet pufferelt adatok és hello indexet tartalmaz. Általában a fizikai memória too70 százalékos értéke.
* **innodb_log_file_size**: hello visszaállítási napló mérete. Ismét: naplók tooensure, hogy az írási műveletek gyors, megbízható és helyreállításhoz legyenek rendszerösszeomlás után használhatja. Adja meg elég hely a naplózás írási műveletek too512 MB értéke.
* **max_connections**: néha alkalmazások ne zárja be kapcsolatok megfelelően. Nagyobb értéket ad hello server toorecycle idled kapcsolatok több időt. hello kapcsolatok maximális száma 10 000-re, de hello ajánlott legfeljebb 5 000.
* **Innodb_file_per_table**: Ez a beállítás engedélyezi vagy letiltja a InnoDB toostore táblák külön fájlban hello képességét. Kapcsolja be a hello beállítás tooensure, hogy számos speciális felügyeleti műveletek hatékonyan alkalmazható. A teljesítmény szempontjából az hello tábla terület átviteli felgyorsítása és hello tekintetében felügyeleti teljesítményének optimalizálásához. hello ajánlott beállítás be Kapcsolva.</br></br>
A MySQL 5.6 hello alapértelmezett beállítás be Kapcsolva, nincs teendője. Korábbi verzióihoz hello alapértelmezett beállítás értéke OFF. hello úgy kell módosítani, mielőtt adatok betöltése, mert az csak az újonnan létrehozott táblák is érint.
* **innodb_flush_log_at_trx_commit**: hello alapértelmezett értéke 1, a hello hatókör beállítása too0 ~ 2. hello alapértelmezett értéke különálló MySQL-adatbázis hello legmegfelelőbb beállítást. hello 2 lehetővé teszi, hogy a legtöbb adatintegritás hello és MySQL-fürt főkiszolgálójának alkalmas. hello 0 beállítással adatvesztés, ami hatással lehet a megbízhatóság (egyes esetekben nagyobb teljesítményű), és az alárendelt MySQL-fürt alkalmas.
* **Innodb_log_buffer_size**: hello napló puffer lehetővé teszi, hogy a tranzakciók toorun anélkül, hogy tooflush hello napló toodisk hello tranzakciók véglegesítés előtt. Azonban ha nagy bináris objektum vagy szövegmezőt, gyorsan hello gyorsítótár fognak használni, és gyakori lemez i/o indul. Fontos, hogy jobban hello pufferméret növelése, ha Innodb_log_waits állapot változó nem 0.
* **query_cache_size**: hello legjobb lehetőség toodisable hello kezdettől azt. (Ez az hello alapértelmezett beállítása a MySQL 5.6) query_cache_size too0, és fel a lekérdezést más módszerek toospeed használ.  

Lásd: [D függelék](#AppendixD) előtt és után hello optimalizálási teljesítmény összehasonlítása.

## <a name="turn-on-hello-mysql-slow-query-log-for-analyzing-hello-performance-bottleneck"></a>Hello MySQL lassú lekérdezés napló hello teljesítménybeli szűk keresztmetszetek elemzéséhez bekapcsolása
hello MySQL lassú lekérdezés naplók segíthetnek a MySQL hello lassú lekérdezések azonosítja. Miután engedélyezte a hello MySQL lassú lekérdezés napló, használhatja a MySQL-eszközök például **mysqldumpslow** tooidentify hello teljesítménybeli szűk keresztmetszetek.  

Alapértelmezés szerint ez nincs engedélyezve. Hello lassú lekérdezés napló bekapcsolása, előfordulhat, hogy néhány Processzor-erőforrások felhasználását. Javasoljuk, hogy engedélyezze a ideiglenesen szűk keresztmetszetek hibaelhárításhoz. a hello lassú lekérdezés napló tooturn:

1. Módosítsa a hello my.cnf fájlt adja hozzá a következő sorokat toohello end hello:

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. Indítsa újra a hello MySQL-kiszolgálót.

        service  mysql  restart

3. Ellenőrizze, hogy hello beállítás érvénybe tart hello segítségével **megjelenítése** parancsot.

![Lassú lekérdezés-log ON][7]   

![Lassú lekérdezés-napló eredmények][8]

Ebben a példában láthatja, lassú lekérdezés hello szolgáltatást be van kapcsolva. Hello segítségével **mysqldumpslow** toodetermine szűk keresztmetszetek eszközt, és a teljesítmény, mint indexek optimalizálása érdekében.

## <a name="appendices"></a>Mellékletek
Az alábbiakban hello minta teszt teljesítményadatok előállított célzott laborkörnyezetben. Hangolási módszerek különböző teljesítménnyel adathordozóira hello teljesítmény adatok trend általános. a környezet vagy a termék különböző verziói hello eredmények függően változhat.

### <a name="AppendixA"></a>A függelék  
**Lemez teljesítménye (IOPS) a különböző RAID-szintek**

![Különböző RAID-szintek IOPS lemez][9]

**Teszt parancsok**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> Ebben a tesztben hello munkaterhelés 64 szál tooreach hello felső korlátja RAID közben használ.
>
>

### <a name="AppendixB"></a>B függelék  
**Különböző RAID-szintek MySQL (teljesítmény) teljesítmény összehasonlítása**   
(XFS fájlrendszer)

![Különböző RAID-szintek MySQL teljesítmény összehasonlítása][10]  
![Különböző RAID-szintek MySQL teljesítmény összehasonlítása][11]

**Teszt parancsok**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**Különböző RAID-szintek MySQL (OLTP) teljesítmény összehasonlítása**  
![Különböző RAID-szintek MySQL (OLTP) teljesítmény összehasonlítása][12]

**Teszt parancsok**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <a name="AppendixC"></a>C függelék   
**A különböző adatrészlet mérete a lemez teljesítménye (IOPS) összehasonlítását**  
(XFS fájlrendszer)

![][13]

**Teszt parancsok**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

a teszteléshez használt hello fájlméret 30 és 1 GB-os, rendre, XFS fájlrendszer a RAID 0 (4 lemezek).

### <a name="AppendixD"></a>D függelék:  
**MySQL teljesítmény (teljesítmény) összehasonlítás előtt és után optimalizálása**  
(XFS fájlrendszer)

![MySQL teljesítmény (teljesítmény) összehasonlítás előtt és után optimalizálása][14]

**Teszt parancsok**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**hello konfigurációs beállítás az alapértelmezett és optimalizálás a következőképpen történik:**

| Paraméterek | Alapértelmezett | Optimalizálás |
| --- | --- | --- |
| **innodb_buffer_pool_size** |None |7 GB |
| **innodb_log_file_size** |5 MB |512 MB |
| **max_connections** |100 |5000 |
| **innodb_file_per_table** |0 |1 |
| **innodb_flush_log_at_trx_commit** |1 |2 |
| **innodb_log_buffer_size** |8 MB |128 MB |
| **query_cache_size** |16 MB |0 |

További részletes [optimalizálási konfigurációs paraméterek](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), tekintse meg a toohello [MySQL hivatalos utasításokat](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).  

  **Tesztkörnyezet**  

| Hardver | Részletek |
| --- | --- |
| CPU |AMD Opteron(tm) processzor 4171 Helykiszolgálójához / 4 magos |
| Memory (Memória) |14 GB |
| Lemez |10 GB/lemez |
| Operációs rendszer |Ubuntu 14.04.1 LTS |

[1]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png

