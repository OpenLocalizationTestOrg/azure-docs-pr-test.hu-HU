---
title: "az elosztott terhelésű készletek MySQL aaaClusterize |} Microsoft Docs"
description: "Állítson be egy elosztott terhelésű, a magas rendelkezésre állású hello klasszikus üzembe helyezési modellt az Azure-on Linux MySQL-fürt létrehozása"
services: virtual-machines-linux
documentationcenter: 
author: bureado
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 6c413a16-e9b5-4ffe-a8a3-ae67046bbdf3
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/14/2015
ms.author: jparrel
ms.openlocfilehash: 1829fd877c4b0ed177b23a8e3404dbb3db746561
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-load-balanced-sets-tooclusterize-mysql-on-linux"></a>Elosztott terhelésű készletek tooclusterize MySQL használata Linux rendszeren
> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager](../../../resource-manager-deployment-model.md) és klasszikus. Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. A [Resource Manager-sablon](https://azure.microsoft.com/documentation/templates/mysql-replication/) érhető el, ha a MySQL-fürt toodeploy van szüksége.

Ez a cikk ismerteti, és hello különböző szempontok elérhető toodeploy magas rendelkezésre állású Linux-alapú szolgáltatásokhoz a Microsoft Azure-böngészést a MySQL-kiszolgáló magas rendelkezésre állás mint egy ismertetése mutatja be. Ez a megközelítés bemutató videó megtalálható [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

Azt fogja szerkezeti osztott, két csomópontos, egyszeri MySQL magas rendelkezésre állású megoldás DRBD, Corosync és támasztja alapján. Csak egy csomópont MySQL fut egyszerre. A hello DRBD erőforrás nem is egy korlátozott tooonly-csomópont egyszerre.

Nincs szükség az VIP-megoldások HÉ, például mert fogja használni a Microsoft Azure tooprovide ciklikus multiplexelés funkcionalitásra vagy a végpont észlelési, eltávolítás és hello VIP szabályos helyreállítása elosztott terhelésű készletek. hello VIP hello felhőalapú szolgáltatás létrehozásakor, a Microsoft Azure által kiosztott globálisan irányíthatóvá teszi IPv4-cím.

Nincsenek elérhető, mint egy virtuális gép MySQL, beleértve a NBD fürt, Percona, Galera és több köztes megoldások, beleértve a legalább egy másik lehetséges architektúrához [VM raktár számára](http://vmdepot.msopentech.com). Mindaddig, amíg ezek a megoldások képes replikálni, amely a csoportos küldést vagy szórásos és egyedi küldéses, és nem függ a megosztott tárhelytől vagy több hálózati adapterrel, hello forgatókönyvek a Microsoft Azure könnyen toodeploy kell lennie.

Ezek a fürtszolgáltatás architektúrák kiterjeszthető tooother termékek például PostgreSQL és OpenLDAP hasonló módon. Például a megosztás terheléselosztás eljárás sikeresen tesztelték, több főkiszolgálós OpenLDAP, és figyelheti a Channel 9 blogjában.

## <a name="get-ready"></a>Felkészülés
Hello a következőkre lesz szüksége erőforrások és képességek:

  - A Microsoft Azure fiók egy érvényes előfizetéssel, képes toocreate legalább két virtuális gépek (ebben a példában a XS lett megadva)
  - A hálózat és alhálózat
  - Az affinitáscsoportokban
  - Rendelkezésre állási csoportok
  - hello képességét toocreate VHD-k a hello megegyező régióban hello felhőalapú szolgáltatás, és csatolja azokat toohello Linux virtuális gépek

### <a name="tested-environment"></a>Tesztelt környezet
* Ubuntu 13.10
  * DRBD
  * MySQL-kiszolgáló
  * Corosync és támasztja

### <a name="affinity-group"></a>Affinitáscsoportok
Ha bejelentkezik a klasszikus Azure-portál toohello hello megoldás affinitáscsoportok létrehozása kiválasztása **beállítások**, és egy affinitáscsoporthoz létrehozása. A lefoglalt erőforrásokat újabb toothis affinitáscsoport lesz hozzárendelve.

### <a name="networks"></a>Hálózatok
Egy új hálózatot hoznak létre, és egy alhálózat hello hálózaton belül jön létre. A példa 10.10.10.0/24 hálózaton belül csak egy /24 alhálózattal.

### <a name="virtual-machines"></a>Virtual machines (Virtuális gépek)
első Ubuntu 13.10 VM Endorsed Ubuntu gyűjtemény lemezkép használatával hozták létre, és nevezik hello `hadb01`. Új felhőalapú szolgáltatás hello folyamat, amely a hadb jön létre. Ez a név hello megosztott, elosztott terhelésű ideiglenesek, amely hello szolgáltatást fog rendelkezni, hogy több erőforrást hozzáadják mutatja be. hello létrehozása `hadb01` Eseménytelen és befejezett hello portál használatával. Az SSH a végpont automatikusan létrejön, és hello új hálózati van kiválasztva. Most már rendelkezésre állási készlet hello virtuális gépek is létrehozhat.

Létrehozása után hello első virtuális gép létrehozása (technikailag, létrehozásakor hello felhőalapú szolgáltatás), második VM hello `hadb02`. Hello második virtuális gép, Ubuntu 13.10 VM hello gyűjteménye a hello portál használatával használni, de egy meglévő felhőszolgáltatáshoz `hadb.cloudapp.net`, egy új létrehozása helyett. hello hálózati és a rendelkezésre állási készlet ki automatikusan. Egy SSH-végpontot rendel, túl.

Mindkét virtuális gép létrehozása után tekintse meg az SSH-port hello `hadb01` (22-es TCP) és `hadb02` (Ez automatikusan hozzárendelve az Azure-ban).

### <a name="attached-storage"></a>Csatlakoztatott tároló
Egy új virtuális lemez tooboth csatolja, és 5 GB-os hello folyamat létrehozásához. hello lemezek tárolt VHD-tároló hello a fő operációsrendszer-lemezek használatban. Miután lemezek létrehozni és csatolni, nincs nincs szükség toorestart Linux mert hello kernel hello új eszköz jelenik meg. Ez az eszköz megfelel általában `/dev/sdc`. Ellenőrizze `dmesg` hello kimenet.

Minden egyes virtuális gépen, hozzon létre egy partíciót használatával `cfdisk` (elsődleges, Linux partíció) írási és olvasási hello új partíciós tábla. Ne hozzon létre egy fájlrendszer ezt a partíciót.

## <a name="set-up-hello-cluster"></a>Hello fürt beállítása
Használja a APT tooinstall Corosync támasztja és DRBD mindkét Ubuntu virtuális gépeken. toodo így `apt-get`- ben futtassa hello következő kódot:

    sudo apt-get install corosync pacemaker drbd8-utils.

Ne telepítse a MySQL most. Debian és Ubuntu telepítési parancsfájlokat inicializálja a MySQL adatkönyvtára a `/var/lib/mysql`, de hello directory a rendszer egy DRBD fájlrendszer váltotta fel, mert szükség van-e MySQL tooinstall később.

Győződjön meg arról (használatával `/sbin/ifconfig`), hogy mindkét virtuális gépeket használ hello 10.10.10.0/24 alhálózati cím és, hogy azok a ping paranccsal elérhető másik név szerint. Is `ssh-keygen` és `ssh-copy-id` toomake meg arról, hogy mindkét virtuális gépek kommunikálhatnak SSH-kapcsolaton keresztül anélkül, hogy a jelszót.

### <a name="set-up-drbd"></a>DRBD beállítása
Hozzon létre egy DRBD erőforrást, amely használja az alapul szolgáló hello `/dev/sdc1` tooproduce partícióazonosító egy `/dev/drbd1` ext3 fájlrendszerrel formázott és az elsődleges és másodlagos csomópontok használt erőforrást.

1. Nyissa meg `/etc/drbd.d/r0.res` és másolási hello erőforrás-definíció következő mindkét virtuális gépeken:

        resource r0 {
          on `hadb01` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.4:7789;
            meta-disk internal;
          }
          on `hadb02` {
            device  /dev/drbd1;
            disk   /dev/sdc1;
            address  10.10.10.5:7789;
            meta-disk internal;
          }
        }

2. Hello erőforrás inicializálása használatával `drbdadm` mindkét virtuális gépeken:

        sudo drbdadm -c /etc/drbd.conf role r0
        sudo drbdadm up r0

3. Az elsődleges virtuális gép hello (`hadb01`), hello DRBD erőforrás (elsődleges) tulajdonjogát kényszerítése:

        sudo drbdadm primary --force r0

Ha megvizsgálja/proc/drbd hello tartalmát (`sudo cat /proc/drbd`) mindkét virtuális gépeken, megtekintheti az `Primary/Secondary` a `hadb01` és `Secondary/Primary` a `hadb02`, ezen a ponton hello megoldás konzisztens. hello 5 GB-os lemezre, nem kell fizetni toocustomers hello 10.10.10.0/24 hálózaton keresztül szinkronizálódnak.

Hello lemez van szinkronizálása után létrehozhat hello fájlrendszer `hadb01`. Tesztelési célokra ext2 használtuk, de a következő kód hello létrehoz egy ext3 fájlrendszert:

    mkfs.ext3 /dev/drbd1

### <a name="mount-hello-drbd-resource"></a>Hello DRBD erőforrás csatlakoztatása
Most már készen áll a toomount hello DRBD erőforrások még `hadb01`. Debian és származékaik használata `/var/lib/mysql` MySQL tartozó adatok könyvtár. Ha még nem telepítette a MySQL, mert hello könyvtár létrehozása, és a csatlakoztatási hello DRBD erőforrás. tooperform ezt a beállítást, futtassa a következő kódot hello `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="set-up-mysql"></a>MySQL beállítása
Most már készen áll a tooinstall MySQL még a `hadb01`:

    sudo apt-get install mysql-server

A `hadb02`, két lehetőség közül választhat. Mysql-kiszolgáló, amely /var/lib/mysql létrehozása, töltse ki az új adatok könyvtárat, és távolítsa el a hello tartalma is telepítheti. tooperform ezt a beállítást, futtassa a következő kódot hello `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

hello második lehetőség toofailover túl`hadb02` és telepítse a mysql-kiszolgáló létezik. Telepítési parancsfájlokat megfigyelheti hello példánya, és nem touch.

Futtatási hello hibakód a következő `hadb01`:

    sudo drbdadm secondary –force r0

Futtatási hello hibakód a következő `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Ha toofailover DRBD most nem kíván, hello első lehetőség bár késései kevésbé elegáns egyszerűbb. Beállítása után ez, megkezdheti a MySQL-adatbázis használata. Futtatási hello hibakód a következő `hadb02` (vagy bármelyik hello kiszolgálók egyikét aktív, tooDRBD szerint):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* tooroot;

> [!WARNING]
> Az utolsó utasítás gyakorlatilag letiltja az ebben a táblázatban szereplő hello gyökér szintű felhasználó számára hitelesítést. Ez a termelési szintű kell helyettesíteni utasításokat ADNIA és meg adva csak szemléltetési célokat szolgál.

Ha azt szeretné, hogy a külső hello virtuális gépek (Ez az útmutató célja hello) toomake lekérdezések, a MySQL hálózati tooenable is kell. Mindkét virtuális gépeken, nyissa meg a `/etc/mysql/my.cnf` , és túl`bind-address`. Hello címének módosítása a 127.0.0.1 too0.0.0.0. Hello fájl mentése után adja ki a `sudo service mysql restart` a jelenlegi elsődleges.

### <a name="create-hello-mysql-load-balanced-set"></a>Hello MySQL elosztott terhelésű készlet létrehozása
Lépjen vissza toohello portal, nyissa meg túl`hadb01`, és válassza a **végpontok**. egy végpont, toocreate választhat MySQL (TCP 3306) hello legördülő listából, és válassza **hozzon létre új elosztott terhelésű készlet**. Név hello elosztott terhelésű végpont `lb-mysql`. Állítsa be **idő** too5 másodperc, a minimális.

Hello végpont létrehozása után nyissa meg túl`hadb02`, válassza a **végpontok**, és hozzon létre egy végpontot. Válasszon `lb-mysql`, majd válassza ki a MySQL hello legördülő listából. Ebben a lépésben hello Azure CLI használhatja.

Most már rendelkezik minden hello fürt kézi művelet szükséges.

### <a name="test-hello-load-balanced-set"></a>Elosztott terhelésű készlet hello tesztelése
Tesztek külső gépből hajtható végre a MySQL-ügyfélprogrammal, vagy bizonyos alkalmazások, például egy Azure-webhelyen fut phpMyAdmin használatával. Ebben az esetben egy másik Linux mezőben használt MySQL a parancssori eszköz:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Manuális feladatátvétele
Feladatátvétel szimulálhatja MySQL leállítása, DRBD tartozó elsődleges váltás, és újból elindítani a MySQL.

tooperform Ez a feladat, futtassa a következő kódot a hadb01 hello:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Majd a hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

Miután a rendszer átadja manuálisan, megismételheti a távoli lekérdezés, és tökéletesen működnek.

## <a name="set-up-corosync"></a>Corosync beállítása
Corosync hello támasztja toowork szükséges alapul szolgáló fürt infrastruktúra. A szívverés (és egyéb módszerek, például Ultramonkey) Corosync hello CRM funkciók, valószínűségét, míg támasztja marad több hasonló tooHeartbeat funkciót.

hello fő Corosync az Azure pedig az, hogy egyedi küldésű kommunikációs keresztül Corosync inkább az szórás keresztül csoportos küldés, de a Microsoft Azure-hálózat csak az egyedi küldéses támogatja.

Szerencsére Corosync van egy működő egyedi küldéses módot. hello csak valós pedig az, hogy minden csomópont nem kommunikál egymással, mert kell toodefine hello csomópontok a konfigurációs fájlokban, beleértve az IP-címét. Az egyedi küldés és a módosítás kötési cím, a csomópont listák és a naplózási könyvtár használhatjuk hello Corosync példa fájlok (Ubuntu használja `/var/log/corosync` hello példa fájlok használata közben `/var/log/cluster`), és kvórum eszközök engedélyezése.

> [!NOTE]
> Hello következő `transport: udpu` irányelv és hello kézzel definiált mindkét csomópont IP-címet.

Futtatási hello hibakód a következő `/etc/corosync/corosync.conf` mindkét csomópontok:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

Másolja a konfigurációs fájl mindkét virtuális gépeken, és mindkét csomópont Corosync elindításához:

    sudo service start corosync

Röviddel a hello szolgáltatás elindítása után hello fürt hello aktuális gyűrűben kell létrehozni, és kvórum kell kialakítani. Ez a funkció jelenleg ellenőrizheti a naplók megtekintésével, vagy futtassa a következő kód hello:

    sudo corosync-quorumtool –l

Kimeneti hasonló toohello kép a következő jelenik meg:

![corosync-quorumtool - l minta kimenet](./media/mysql-cluster/image001.png)

## <a name="set-up-pacemaker"></a>Támasztja beállítása
Támasztja által használt erőforrások a fürt toomonitor hello, adja meg, ha az elsődleges leáll, és ezen erőforrások toosecondaries válthat. Erőforrások elérhető parancsfájlokkal, illetve LSB (init-bA), parancsfájlok egyéb lehetőségek között definiálhatók.

Azt szeretnénk, ha támasztja túl "saját" hello DRBD erőforrást, hello csatlakoztatási pontot és hello MySQL-szolgáltatás. Támasztja kapcsolhatja be és ki DRBD, ha csatlakoztatása és leválasztása, majd indítsa el és leállítani a MySQL hello jobb, ha valami rossz sorrendben fordul elő, ha hello elsődleges, a telepítés befejeződése.

Támasztja első telepítésekor a konfiguráció legyen egyszerű, elég, például:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

1. Hello konfigurációjának ellenőrzése futtatásával `sudo crm configure show`.
2. Ezután hozzon létre egy fájlt (például `/tmp/cluster.conf`) a következő erőforrások hello:

        primitive drbd_mysql ocf:linbit:drbd \
              params drbd_resource="r0" \
              op monitor interval="29s" role="Master" \
              op monitor interval="31s" role="Slave"

        ms ms_drbd_mysql drbd_mysql \
              meta master-max="1" master-node-max="1" \
                clone-max="2" clone-node-max="1" \
                notify="true"

        primitive fs_mysql ocf:heartbeat:Filesystem \
              params device="/dev/drbd/by-res/r0" \
              directory="/var/lib/mysql" fstype="ext3"

        primitive mysqld lsb:mysql

        group mysql fs_mysql mysqld

        colocation mysql_on_drbd \
               inf: mysql ms_drbd_mysql:Master

        order mysql_after_drbd \
               inf: ms_drbd_mysql:promote mysql:start

        property stonith-enabled=false

        property no-quorum-policy=ignore

3. Betöltési hello hello konfigurációs fájlból. Csak akkor kell toodo Ez egy csomópontban.

        sudo crm configure
          load update /tmp/cluster.conf
          commit
          exit

4. Győződjön meg arról, hogy támasztja kezdődik-e a mindkét csomópont indítása:

        sudo update-rc.d pacemaker defaults

5. A `sudo crm_mon –L`, győződjön meg arról, hogy a csomópontok egyikét hello fürt hello főkiszolgálójának és erőforrások hello fut-e. Csatlakozási és ps toocheck hello erőforrások futtató is használhatja.

a következő képernyőfelvételen látható hello `crm_mon` egy csomópont leállt (Ctrl + C kiválasztásával kilépési):

![crm_mon csomópont leállítása](./media/mysql-cluster/image002.png)

Ezen a képernyőfelvételen látható csomópontok, a egy fő és egy alárendelt jeleníti meg:

![crm_mon műveleti főkiszolgáló/alárendelt](./media/mysql-cluster/image003.png)

## <a name="testing"></a>Tesztelés
Készen áll az Automatikus feladatátvétel szimuláció. Nincsenek két módon toodo ez: letölthető és rögzített.

hello enyhe módon hello fürt leállítás függvényt használja: ``crm_standby -U `uname -n` -v on``. Ha ezzel hello fő, hello alárendelt élvez. Ne feledje tooset a hátsó toooff. Ha ezt elmulasztja, akkor crm_mon készenléti egy csomópont jelenjen meg.

hello rögzített módon leáll le hello elsődleges virtuális gép (hadb01) hello portálon keresztül, vagy módosítsa úgy a virtuális gép (Ez azt jelenti, hogy halt, leállítási) hello paraméterben megadott futtatási szint hello. Segít Corosync és támasztja által jelzés adott hello fő folyamatos le. Ez tesztelheti (hasznos a karbantartási időszakok), de hello forgatókönyv szerint hello VM zárolása is kényszeríthető.

## <a name="stonith"></a>STONITH
A virtuális gép leállítása hello Azure parancssori felület helyett egy fizikai eszközt vezérlő STONITH parancsfájl keresztül lehetséges tooissue kell lennie. Használhat `/usr/lib/stonith/plugins/external/ssh` talál és hello fürt konfigurációjába STONITH engedélyezése. Az Azure CLI globálisan kell telepíteni, és a hello közzétételi beállítások és hello fürt felhasználói profil kell betölteni.

Mintakód hello erőforrás érhető el a [GitHub](https://github.com/bureado/aztonith). Hello fürt konfigurációjának módosítása hello túl a következő hozzáadásával`sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

> [!NOTE]
> hello parancsfájl nem hajtható végre fel/le ellenőrzi. hello eredeti SSH erőforrás volt a 15 ping-ellenőrzéseket, de egy Azure virtuális gép helyreállítási idő további változó lehet.

## <a name="limitations"></a>Korlátozások
hello a következő korlátozások vonatkoznak:

* hello linbit DRBD erőforrást parancsprogram DRBD kezelő erőforrásként támasztja használja a `drbdadm down` Ha egy csomópont leáll, akkor is, ha hello csomópont csak készenléti van folyamatban. Ennek oka nem ideális hello alárendelt fog nem lehet szinkronizálása hello DRBD erőforrás hello fő lekérdezi az írási műveletek során. Ha hello fő nem graciously sikertelen, a hello alárendelt akár egy régebbi fájlrendszer állapota is eltarthat. Ez megoldására két lehetséges módja van:
  * Érvényesítési egy `drbdadm up r0` a fürt összes csomópontján keresztül egy helyi (nem clusterized)-figyelő
  * Hello linbit DRBD parancsfájl szerkesztése, ügyelve arra, hogy `down` nem hívják meg`/usr/lib/ocf/resource.d/linbit/drbd`
* hello terheléselosztónak legalább öt másodpercenként toorespond kell, így az alkalmazások fürttámogató legyen, és jobban tűrik a timeout kell. Egyéb-architektúrák esetén az-app várólisták és a lekérdezés middlewares, például segíthet is.
* MySQL hangolása szükséges, amely írás kezelhető ütemben történik tooensure pedig gyorsítótárak kiürített toodisk gyakorisággal lehetséges toominimize memória adatvesztés.
* Az írási teljesítmény függő a virtuális gép virtuális kapcsoló hello kapcsolódnak össze, mivel ez hello mechanizmus DRBD tooreplicate hello eszköz által használt.
