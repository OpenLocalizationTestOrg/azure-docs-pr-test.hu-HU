---
title: "rendelkezésre állás az SAP HANA Azure virtuális gépek (VM) aaaHigh |} Microsoft Docs"
description: "Magas rendelkezésre állású SAP HANA az Azure virtuális gépek (VM) létrehozásához."
services: virtual-machines-linux
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: sedusch
ms.openlocfilehash: dcb9bb70594f9d97f8a888cec76300bcbe0bf1ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a>Magas rendelkezésre állású SAP HANA az Azure virtuális gépek (VM)

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

A helyszíni vagy HANA replikációs használhatja, vagy használja a megosztott tároló tooestablish magas rendelkezésre állású SAP Hana.
Sajnos jelenleg csak a támogatott HANA replikációs beállítása az Azure-on. SAP HANA-replikáció egy főcsomópont és legalább egy alárendelt csomópont áll. Adatok hello fő csomóponton módosítások toohello toohello alárendelt csomópontok replikálja a szinkron vagy aszinkron módon.

Ez a cikk ismerteti, hogyan toodeploy hello virtuális gépek, virtuális gépek hello konfigurálása, hello fürt keretrendszer telepítése, telepítése és konfigurálása SAP HANA replikációs.
Hello például konfigurációk telepítési parancsok stb példányszámának 03 és HANA rendszer azonosító HDB szolgál.

Olvassa el a következő SAP megjegyzések és által írt cikkeket először hello

* SAP Megjegyzés [1928533], amelynek van:
  * Hello SAP szoftver központi telepítése által támogatott Azure Virtuálisgép-méretek listáját
  * Az Azure Virtuálisgép-méretek fontos készletkapacitás információival
  * Támogatott SAP szoftver, és az operációs rendszer és az adatbázis kombinációját
  * A Windows és a Microsoft Azure Linux szükséges SAP kernel verziója
* SAP Megjegyzés [2015553] a SAP-támogatott SAP szoftverek központi telepítése az Azure-ban szükséges előfeltételeket ismerteti.
* SAP Megjegyzés [2205917] javasolt a SUSE Linux Enterprise Server operációs rendszer beállításait az SAP-alkalmazásokból
* SAP Megjegyzés [1944799] SUSE Linux Enterprise Server SAP HANA-irányelvek rendelkezik az SAP-alkalmazásokból
* SAP Megjegyzés [2178632] tartalmaz részletes információkat az Azure-ban SAP jelentett összes figyelési metrikákat.
* SAP Megjegyzés [2191498] hello szükséges SAP állomás ügynök verziója Linux az Azure-ban.
* SAP Megjegyzés [2243692] SAP Azure Linux licenceléssel kapcsolatos információt tartalmaz.
* SAP Megjegyzés [1984787] SUSE Linux Enterprise Server 12 vonatkozó általános információkat tartalmaz.
* SAP Megjegyzés [1999351] hello Azure fokozott Figyelőbővítmény az SAP további információkat.
* [SAP közösségi WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) rendelkezik az összes szükséges SAP megjegyzések Linux.
* [Azure virtuális gépek tervezési és megvalósítási az SAP Linux rendszeren][planning-guide]
* [Az Azure virtuális gépek telepítése az SAP, Linux (Ez a cikk)][deployment-guide]
* [Az SAP Linux Azure virtuális gépek DBMS-telepítés][dbms-guide]
* [SAP HANA SR teljesítmény optimalizált forgatókönyv] [ suse-hana-ha-guide] hello útmutató tartalmazza az összes szükséges információk tooset helyszíni SAP HANA replikációs fel. Ez az útmutató használja kiindulópontként.

## <a name="deploying-linux"></a>Linux telepítése

SUSE Linux Enterprise Server hello erőforrás ügynök SAP Hana tartalmazza az SAP-alkalmazásokból.
hello Azure piactér SUSE Linux Enterprise Server SAP alkalmazások 12 a saját (Bring Your saját előfizetés), amelyeket felhasználhat toodeploy új virtuális gépek a kép tartalmazza.

### <a name="manual-deployment"></a>Manuális telepítése

1. Erőforráscsoport létrehozása
1. Virtuális hálózat létrehozása
1. Két Storage-fiókok létrehozása
1. Egy rendelkezésre állási csoport létrehozása  
   Készlet maximális frissítési tartomány
1. Hozzon létre egy terhelés-kiegyenlítő (belső)  
   Válassza ki a fenti lépést a virtuális hálózat
1. 1 virtuális gép létrehozása  
   https://Portal.Azure.com/#Create/SUSE-byos.sles-for-SAP-byos12-SP1  
   SLES SAP alkalmazások 12 SP1 (saját)  
   Válassza ki a Tárfiók 1  
   A rendelkezésre állási csoport kiválasztása  
1. 2. virtuális gép létrehozása  
   https://Portal.Azure.com/#Create/SUSE-byos.sles-for-SAP-byos12-SP1  
   SLES SAP alkalmazások 12 SP1 (saját)  
   Válassza ki a Tárfiók 2   
   A rendelkezésre állási csoport kiválasztása  
1. Adatlemez hozzáadása
1. Terheléselosztó hello konfigurálása
    1. Előtérbeli IP-készlet létrehozása
        1. Nyissa meg a hello terheléselosztó előtérbeli IP-készlet között, kattintson a Hozzáadás gombra
        1. Adja meg a hello hello új előtérbeli IP-készlet neve (például hana-előtér)
       1. Kattintson az OK gombra
        1. Hello új előtérbeli IP-készlet létrehozása után jegyezze fel az IP-címe
    1. Háttér-készlet létrehozása
        1. Nyissa meg a terheléselosztó hello, válassza ki a háttérkészlet, kattintson a Hozzáadás gombra
        1. Adjon meg hello hello új háttérkészlet (például hana-háttérrendszer)
        1. Kattintson a Hozzáadás gombra a virtuális gépek
        1. Válassza ki a rendelkezésre állási csoport a korábban létrehozott hello
        1. Válassza ki a hello virtuális gépek hello SAP HANA-fürt
        1. Kattintson az OK gombra
    1. Hozzon létre egy állapotmintáihoz
       1. Nyissa meg a terheléselosztó hello, válassza ki a állapotteljesítmény, kattintson a Hozzáadás gombra
        1. Adjon meg hello hello új állapotmintáihoz (például hana-hp)
        1. Válassza ki a TCP protokoll, port 625**03**, időköz 5 és sérült küszöbérték 2
        1. Kattintson az OK gombra
    1. Terheléselosztási szabályok létrehozása
        1. Nyissa meg a terheléselosztó hello, válassza ki a terheléselosztási szabályok, kattintson a Hozzáadás gombra
        1. Adja meg a hello új terheléselosztási szabály hello nevét (például hana-lb-3**03**15)
        1. SELECT hello front-end IP-címet, a háttérkészlet és a rendszerállapot mintavételi, korábban létrehozott (például hana-előtér)
        1. Tartsa a TCP protokoll, adja meg azt a portot 3**03**15
        1. Üresjárati időtúllépés too30 perc növelése
       1. **Győződjön meg arról, hogy tooenable fix IP-Címek**
        1. Kattintson az OK gombra
        1. Ismételje meg a hello fent port 3**03**17

### <a name="deploy-with-template"></a>Üzembe helyezés sablon használatával
Használhat hello gyors üzembe helyezési sablonok a githubon toodeploy minden szükséges erőforrásokat. hello sablon telepíti hello virtuális gépek, a terheléselosztó hello, a rendelkezésre állási csoport stb. Kövesse a lépéseket toodeploy hello sablon:

1. Nyissa meg hello [adatbázis sablon] [ template-multisid-db] vagy hello [sablon összevont] [ template-converged] hello Azure Portal hello adatbázis-sablon csak létrehoz hello terheléselosztási szabályok adatbázis mivel hello átszervezett sablon is létrehozza a terheléselosztási szabályok hello egy ASC/SCS és SSZON (csak Linux) példányát. Ha azt tervezi, hogy az SAP NetWeaver-alapú rendszerek tooinstall, és azt is szeretné tooinstall hello ASC/SCS példányhoz hello azonos gépek, használja a hello [sablon összevont][template-converged].
1. Adja meg a következő paraméterek hello
    1. SAP azonosító  
       Adja meg a hello SAP rendszer azonosító hello tooinstall kívánt SAP rendszer. hello azonosító használandó előtagjaként hello telepített erőforrások esetén.
    1. A készlet típusa (csak akkor érvényes, ha hello átszervezett sablont használ)  
       Hello SAP NetWeaver verem típusának kiválasztása
    1. Operációs rendszer típusa  
       Válasszon ki egy hello Linux terjesztéseket. Ehhez a példához válassza ki a SLES 12 saját
    1. DB típusa  
       Válassza ki a HANA
    1. SAP mérete  
       SAP hello új rendszer hello mennyisége ad meg. Ha nem biztos abban, hogy hány SAP hello rendszer igényel, kérje meg a SAP technológia Partner vagy a rendszer integráló
    1. Rendszer rendelkezésre állás  
       Válassza ki a magas rendelkezésre ÁLLÁSÚ
    1. Rendszergazda felhasználónevét és a rendszergazdai jelszó  
       Új felhasználó jön létre, amely lehet használt toolog toohello gépen.
    1. Új vagy meglévő alhálózati  
       Meghatározza, hogy egy új virtuális hálózat és alhálózat kell létrehozni, vagy használjon egy létező alhálózatot. Ha már van egy virtuális hálózatot, amely csatlakoztatott tooyour a helyszíni hálózat, válasszon a meglévő.
    1. Alhálózati azonosító  
    hello azonosító hello alhálózati toowhich hello virtuális gépek csatlakoznia kell. Válassza ki a VPN- vagy Express Route virtuális hálózati tooconnect hello virtuális gép tooyour a helyszíni hálózat hello alhálózat. hello azonosítója általában a következőképpen néz következő`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

## <a name="setting-up-linux-ha"></a>Beállítása Linux magas rendelkezésre ÁLLÁSÚ

a következő elemek hello fűzve előtagként vagy [A] - alkalmazható tooall csomópontok, toonode 2 [1] - 1 vagy a(z) [2] csak érvényes toonode - csak érvényes.

1. [A] az SAP - csak a saját regisztrálása SLES toobe képes toouse hello adattárak SLES
1. [A] SLES az SAP saját csak - nyilvános felhő modul hozzá lesz adva
1. [A] SLES frissítése
    ```bash
    sudo zypper update

    ```

1. [1] ssh hozzáférés engedélyezése
    ```bash
    sudo ssh-keygen -tdsa
    
    # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy hello public key
    sudo cat /root/.ssh/id_dsa.pub
    ```

2. [2] ssh hozzáférés engedélyezése
    ```bash
    sudo ssh-keygen -tdsa

    # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
    sudo vi /root/.ssh/authorized_keys
    
    # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy hello public key    
    sudo cat /root/.ssh/id_dsa.pub
    ```

1. [1] ssh hozzáférés engedélyezése
    ```bash
    # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
    sudo vi /root/.ssh/authorized_keys
    
    ```

1. [A] magas rendelkezésre ÁLLÁSÚ-kiterjesztés telepítése
    ```bash
    sudo zypper install sle-ha-release fence-agents
    
    ```

1. [A] telepítő lemez elrendezése
    1. LVM  
    Általánosságban javasolt toouse LVM kötetek, amelyek adatokat tárolhatnak, és a naplófájlok. hello az alábbi példa azt feltételezi, hogy hello virtuális gépek rendelkezik-e a négy adatlemezt csatolni, amelyeket használt toocreate két kötet.
        * Az összes lemezt, amelyet az toouse fizikai köteteket hozhat létre.
    <pre><code>
    sudo pvcreate /dev/sdc
    sudo pvcreate /dev/sdd
    sudo pvcreate /dev/sde
    sudo pvcreate /dev/sdf
    </code></pre>
        * Hozzon létre egy kötet csoport hello adatfájlok, hello naplófájlok egy kötet csoport és egy hello megosztott könyvtárában SAP HANA
    <pre><code>
    sudo vgcreate vg_hana_data /dev/sdc /dev/sdd
    sudo vgcreate vg_hana_log /dev/sde
    sudo vgcreate vg_hana_shared /dev/sdf
    </code></pre>
        * Hello logikai köteteket hozhat létre
    <pre><code>
    sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
    sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
    sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
    sudo mkfs.xfs /dev/vg_hana_data/hana_data
    sudo mkfs.xfs /dev/vg_hana_log/hana_log
    sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
    </code></pre>
        * Hello csatlakoztatási könyvtárak létrehozása, és másolja az összes logikai kötet UUID hello
    <pre><code>
    sudo mkdir -p /hana/data
    sudo mkdir -p /hana/log
    sudo mkdir -p /hana/shared
    # write down hello id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
    sudo blkid
    </code></pre>
        * Három logikai kötetek hello fstab bejegyzéseket létrehozni
    <pre><code>
    sudo vi /etc/fstab
    </code></pre>
    A sor túl/etc/fstab beszúrása
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b> /hana/data xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b> /hana/log xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b> /hana/shared xfs  defaults,nofail  0  2
    </code></pre>
        * Csatlakoztassa hello új köteteket
    <pre><code>
    sudo mount -a
    </code></pre>
    1. Egyszerű lemez  
       A kis vagy bemutató rendszerek, elhelyezhet egy lemezt a HANA adatainak és naplókönyvtárainak fájlokat. hello következő parancsok /dev/sdc hozza létre a partíciót, és formázza xfs.
    ```bash
    sudo fdisk /dev/sdc
    sudo mkfs.xfs /dev/sdc1
    
    # <a name="write-down-hello-id-of-devsdc1"></a>Írja le /dev/sdc1 hello azonosítója
    sudo/sbin/blkid sudo vi/etc/fstab
    ```

    Insert this line too/etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
    </code></pre>

    Create hello target directory and mount hello disk.

    ```bash
    sudo mkdir /hana
    sudo mount -a
    ```

1. [A] állomásnév telepítési állomások  
    DNS-kiszolgálót használjon, vagy módosítsa a hello/etc/hosts minden csomóponton. Ez a példa bemutatja, hogyan toouse hello/Etc/Hosts fájlt.
   Cserélje le a hello IP-cím és a következő parancsok hello hello állomásnév
    ```bash
    sudo vi /etc/hosts
    ```
    Helyezze be a következő sorokat túl/etc/hosts hello. Hello IP cím és az állomásnév toomatch a környezet módosítása    
    
    <pre><code>
    <b>&lt;IP address of host 1&gt; &lt;hostname of host 1&gt;</b>
    <b>&lt;IP address of host 2&gt; &lt;hostname of host 2&gt;</b>
    </code></pre>

1. [1] fürt telepítése
    ```bash
    sudo ha-cluster-init
    
    # Do you want toocontinue anyway? [y/N] -> y
    # Network address toobind too(e.g.: 192.168.1.0) [10.79.227.0] -> ENTER
    # Multicast address (e.g.: 239.x.x.x) [239.174.218.125] -> ENTER
    # Multicast port [5405] -> ENTER
    # Do you wish toouse SBD? [y/N] -> N
    # Do you wish tooconfigure an administration IP? [y/N] -> N
    ```
        
1. [2] csomópont toocluster hozzáadása
    ```bash
    sudo ha-cluster-join
        
    # WARNING: NTP is not configured toostart at system boot.
    # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
    # Do you want toocontinue anyway? [y/N] -> y
    # IP address or hostname of existing node (e.g.: 192.168.1.1) [] -> IP address of node 1 e.g. 10.0.0.5
    # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
    ```

1. [A] módosítása hacluster jelszó toohello ugyanazt a jelszót
    ```bash
    sudo passwd hacluster
    
    ```

1. [A] konfigurálása corosync toouse egyéb átvitelt, és adja hozzá a csomópontlista. Fürt egyébként nem működik.
    ```bash
    sudo vi /etc/corosync/corosync.conf    
    
    ```

    Adja hozzá a következő félkövér tartalom toohello fájl hello.
    
    <pre><code> 
    [...]
      interface { 
          [...] 
      }
      <b>transport:      udpu</b>
    } 
    <b>nodelist {
      node {
        ring0_addr:     < ip address of node 1 >
      }
      node {
        ring0_addr:     < ip address of node 2 > 
      } 
    }</b>
    logging {
      [...]
    </code></pre>

    Indítsa újra hello corosync szolgáltatás

    ```bash
    sudo service corosync restart
    
    ```

1. [A] HANA magas rendelkezésre ÁLLÁSÚ csomagok telepítése  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

## <a name="installing-sap-hana"></a>SAP HANA telepítése

Hajtsa végre a hello 4 fejezete [SAP HANA SR teljesítmény optimalizált forgatókönyv útmutató] [ suse-hana-ha-guide] tooinstall SAP HANA replikációs.

1. [A] hdblcm-ről futtatva hello HANA DVD-ről
    * Válassza ki a telepítés-1 >
    * Válassza ki a további összetevők telepítésének 1->
    * Adja meg a telepítési útvonalat [/ hana/megosztott]: -> adjon meg
    * Adja meg a helyi állomás nevét [.]: -> adjon meg
    * Szeretné tooadd további állomások toohello rendszer? (i/n) [n]: -> adjon meg
    * Adja meg az SAP HANA-azonosító:<SID of HANA e.g. HDB>
    * Adja meg a [00] száma:   
  HANA példányszámának. 03 használja, ha követte a fenti példa hello vagy használt hello Azure-sablon
    * Válassza ki az adatbázis-mód / Index [1] adja meg: -> adjon meg
    * Válassza ki a rendszer használati / adja meg az Index [4]:  
  Válassza ki a hello rendszer kihasználtsága
    * Adja meg a helyet, az adatkötetek [/ hana/data/HDB]: -> adjon meg
    * Adja meg a naplózási kötetek [/ hana/napló/HDB] helyet: ENTER ->
    * Maximális memória kiosztása korlátozása? [n]: -> adjon meg
    * Adja meg a tanúsítvány gazdagép neve a gazdagép "..." []: -> Adjon meg
    * Adja meg a SAP ügynök felhasználói (sapadm) jelszavát:
    * SAP ügynök felhasználói (sapadm) jelszó megerősítése:
    * Adja meg a rendszergazda (hdbadm) jelszavát:
    * Erősítse meg a rendszergazda (hdbadm) jelszavát:
    * Adja meg a rendszer rendszergazdai kezdőkönyvtár [/ usr/sap/HDB/home]: -> adjon meg
    * Adja meg a rendszer rendszergazdai bejelentkezési rendszerhéj [/ bin/sh]: -> adjon meg
    * Adja meg a rendszergazda felhasználói Azonosítóját [1001]: -> adjon meg
    * Adjon meg azonosító a felhasználói csoport (sapsys) [79]: -> adjon meg
    * Adja meg az adatbázis jelszó (rendszer):
    * Adatbázis (rendszer) felhasználói jelszó megerősítése:
    * Számítógép újraindítása után indítsa újra a rendszert? [n]: -> adjon meg
    * Szeretné toocontinue? (i/n):  
  Hello összefoglaló, és írja be a y toocontinue
1. [A] frissítési SAP Gazdagépügynöke  
  Hello legújabb SAP a gazdagép ügynöke archív letöltését hello [SAP Softwarecenter] [ sap-swcenter] és futtatási hello parancs tooupgrade hello ügynök következő. Cserélje le a hello elérési toohello toopoint toohello archívumfájl letöltött.
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path tooSAP Host Agent SAR>
    ```

1. [1] replikációs HANA létrehozása (a legfelső szintű)  
    Futtassa a következő parancs hello. Győződjön meg arról, hogy tooreplace félkövér karakterláncok (HANA rendszer azonosító HDB és példány újrahasznosítása 03) a SAP HANA-telepítés hello értékekkel.
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN too<b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. [A] létrehozása keystore bejegyzés (root)
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>
1. [1] backup database (a legfelső szintű)
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
    </code></pre>
1. [1] felhasználóváltás toohello sapsid (például hdbadm), és hozzon létre hello elsődleges hely.
    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>
1. [2] váltson toohello sapsid felhasználói (például hdbadm), és hello másodlagos hely létrehozásához.
    <pre><code>
    su - <b>hdb</b>adm
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>saphanavm1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-cluster-framework"></a>Fürt keretrendszer konfigurálása

Hello alapértelmezett beállítások módosítása

<pre>
sudo vi crm-defaults.txt
# enter hello following toocrm-defaults.txt
<code>
property $id="cib-bootstrap-options" \
  no-quorum-policy="ignore" \
  stonith-enabled="true" \
  stonith-action="reboot" \
  stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
  resource-stickiness="1000" \
  migration-threshold="5000"
op_defaults $id="op-options" \
  timeout="600"
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a>most hello toohello fájlfürt betöltés
sudo crm terhelés frissítés crm-defaults.txt konfigurálása
</pre>

### <a name="create-stonith-device"></a>STONITH eszköz létrehozása

hello STONITH eszköz által használt egy egyszerű tooauthorize Microsoft Azure ellen. Kérjük, kövesse ezeket a lépéseket toocreate egy egyszerű szolgáltatást.

1. Nyissa meg túl<https://portal.azure.com>
1. Nyissa meg hello Azure Active Directory panel  
   Nyissa meg tooProperties, és írja le hello Directory azonosítóját. Ez a hello **bérlőazonosító**.
1. Kattintson az alkalmazás-regisztráció
1. Kattintson az Add (Hozzáadás) parancsra
1. Adjon meg egy nevet, válassza ki a "Web app/API" alkalmazástípus, adja meg a bejelentkezési URL-címet (például http://localhost) és kattintson a Létrehozás gombra
1. hello bejelentkezési URL-címet nem használja, és bármilyen érvényes URL-CÍMEK lehetnek
1. Válassza ki az új alkalmazás hello és kattintson a kulcsok hello-beállítások lap
1. Adja meg egy új kulcs leírását, válassza a "Soha nem jár le", és kattintson a Mentés gombra
1. Írja le hello érték. Hello használják **jelszó** a hello szolgáltatás egyszerű
1. Írja le hello azonosítót. Hello felhasználónév, a rendszer (**bejelentkezési azonosító** hello lépéseket a) a hello szolgáltatás egyszerű

hello szolgáltatás egyszerű engedélyek tooaccess az Azure-erőforrások alapértelmezés szerint nem rendelkezik. Toogive hello szolgáltatás egyszerű engedélyek toostart van szüksége, és állítsa (felszabadítása) hello fürt összes virtuális gépet.

1. Nyissa meg toohttps://portal.azure.com
1. Nyissa meg az összes erőforrás panel hello
1. Válassza ki a virtuális gép hello
1. Kattintson a hozzáférés-vezérlés (IAM)
1. Kattintson az Add (Hozzáadás) parancsra
1. Válassza ki a hello szerepkör tulajdonosa
1. Adja meg az előbb létrehozott hello alkalmazás hello neve
1. Kattintson az OK gombra

Után szerkeszteni hello engedélyek hello virtuális gépekhez, beállíthatja a hello STONITH eszközök hello fürtben.

<pre>
sudo vi crm-fencing.txt
# enter hello following toocrm-fencing.txt
# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password
<code>
primitive rsc_st_azure_1 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

primitive rsc_st_azure_2 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a>most hello toohello fájlfürt betöltés
sudo crm terhelés frissítés crm-fencing.txt konfigurálása
</pre>

### <a name="create-sap-hana-resources"></a>SAP HANA-erőforrások létrehozása

<pre>
sudo vi crm-saphanatop.txt
# enter hello following toocrm-saphana.txt
# replace hello bold string with your instance number and HANA system id
<code>
primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
    operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
    op monitor interval="10" timeout="600" \
    op start interval="0" timeout="600" \
    op stop interval="0" timeout="300" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"

clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a>most hello toohello fájlfürt betöltés
sudo crm terhelés frissítés crm-saphanatop.txt konfigurálása
</pre>

<pre>
sudo vi crm-saphana.txt
# enter hello following toocrm-saphana.txt
# replace hello bold string with your instance number, HANA system id and hello frontend IP address of hello Azure load balancer. 
<code>
primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
    operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
    op start interval="0" timeout="3600" \
    op stop interval="0" timeout="3600" \
    op promote interval="0" timeout="3600" \
    op monitor interval="60" role="Master" timeout="700" \
    op monitor interval="61" role="Slave" timeout="700" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
    DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"

ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
    target-role="Started" interleave="true"

primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
    meta target-role="Started" is-managed="true" \ 
    operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
    op monitor interval="10s" timeout="20s" \ 
    params ip="<b>10.0.0.21</b>" 
primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
    params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
    op monitor timeout=20s interval=10 depth=0 
group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
 
colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  
order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a>most hello toohello fájlfürt betöltés
sudo crm terhelés frissítés crm-saphana.txt konfigurálása
</pre>

### <a name="test-cluster-setup"></a>Teszt fürt beállítása
a következő fejezet hello ismertetik, hogyan tesztelheti a telepítőt. Minden teszt feltételezi, hogy a legfelső szintű áll, és hello SAP HANA fő fut a virtuális gép saphanavm1 hello.

#### <a name="fencing-test"></a>Teszt kerítésépítés

A csomópont saphanavm1 hello hálózati adapter letiltásával hello beállítása hello kerítés ügynök tesztelheti.

<pre><code>
sudo ifdown eth0
</code></pre>

hello virtuális gép kell most beolvasása újraindult, vagy attól függően, hogy a fürt konfigurációját, leállt.
Ha hello stonith műveleti toooff, hello virtuális gép leáll és hello erőforrások áttelepített toohello virtuális gépet futtat.

Hello virtuális gép ismételt futtatása, ha hello SAP HANA-erőforrás sikertelen lesz toostart másodlagosként Ha AUTOMATED_REGISTER = "false". Ebben az esetben szüksége tooconfigure hello HANA példányához, másodlagos a következő hello a következő parancs futtatásával:

<pre><code>
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a>Manuális feladatátvétel tesztelése

Manuális feladatátvétel tesztelheti a csomópont saphanavm1 hello támasztja szolgáltatás leállításával.
<pre><code>
service pacemaker stop
</code></pre>

Hello feladatátvétel után elindíthatja hello szolgáltatást újra. hello saphanavm1 SAP HANA erőforrás sikertelen lesz toostart másodlagosként Ha AUTOMATED_REGISTER = "false". Ebben az esetben szüksége tooconfigure hello HANA példányához, másodlagos a következő hello a következő parancs futtatásával:

<pre><code>
service pacemaker start
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-migration"></a>Áttelepítés tesztelése

Hello SAP HANA-főcsomópont telepíthet át a következő hello a következő parancs futtatásával
<pre><code>
crm resource migrate msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
crm resource migrate g_ip_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
</code></pre>

Ez kell telepítenie a hello SAP HANA-főcsomópont és hello virtuális IP-cím toosaphanavm2 tartalmazó hello csoport.
hello saphanavm1 SAP HANA erőforrás sikertelen lesz toostart másodlagosként Ha AUTOMATED_REGISTER = "false". Ebben az esetben szüksége tooconfigure hello HANA példányához, másodlagos a következő hello a következő parancs futtatásával:

<pre><code>
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

hello áttelepítés törölt újra toobe igénylő hely contraints hoz létre.

<pre><code>
crm configure edited

# delete location contraints that are named like hello following contraint. You should have two contraints, one for hello SAP HANA resource and one for hello IP address group.
location cli-prefer-g_ip_<b>HDB</b>_HDB<b>03</b> g_ip_<b>HDB</b>_HDB<b>03</b> role=Started inf: <b>saphanavm2</b>
</code></pre>

Szükség hello másodlagos csomópont erőforrás toocleanup hello állapotát

<pre><code>
# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

## <a name="next-steps"></a>Következő lépések
* [Az Azure virtuális gépek tervezési és megvalósítási az SAP][planning-guide]
* [Az SAP Azure virtuális gépek telepítése][deployment-guide]
* [Az SAP Azure virtuális gépek adatbázis-kezelő telepítése][dbms-guide]
* Hogyan tooestablish magas rendelkezésre állású és az Azure (nagy példányokat), az SAP HANA vész-helyreállítási terv: toolearn [SAP HANA (nagy példányok) magas rendelkezésre állási és vészhelyreállítási helyreállítási Azure](hana-overview-high-availability-disaster-recovery.md). 
