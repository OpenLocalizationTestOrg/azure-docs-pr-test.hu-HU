---
title: "Virtuális gépek magas rendelkezésre állás a SUSE Linux Enterprise Server SAP NetWeaver az SAP-alkalmazásokból aaaAzure |} Microsoft Docs"
description: "Magas rendelkezésre állású útmutatója az SAP NetWeaver a SUSE Linux Enterprise Server SAP-alkalmazásokból"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: sedusch
ms.openlocfilehash: e944103df92d5ffec9196189f138e25972bea79f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a>Magas rendelkezésre állás a SAP NetWeaver a SUSE Linux Enterprise Server Azure virtuális gépeken az SAP-alkalmazásokból

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
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

Ez a cikk ismerteti, hogyan toodeploy hello virtuális gépek, virtuális gépek hello konfigurálása, hello fürt keretrendszer telepítése és egy magas rendelkezésre állású SAP NetWeaver 7.50 rendszer telepítéséhez.
Hello például konfigurációk telepítési parancsok stb. Asc példányszámának 00, SSZON példányszámának 02 és SAP rendszer azonosító NWS szolgál. hello nevek hello erőforrások (például virtuális gépek, virtuális hálózatok) hello példa feltételezi hello használt [sablon összevont] [ template-converged] SAP rendszer azonosító NWS toocreate hello erőforrásokkal.

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
* [SAP HANA SR teljesítményre optimalizált forgatókönyv][suse-hana-ha-guide]  
  hello útmutató tartalmazza az összes szükséges információk tooset helyszíni SAP HANA replikációs fel. Ez az útmutató használja kiindulópontként.
* [Magas rendelkezésre álló NFS tár DRBD és támasztja] [ suse-drbd-guide] hello útmutató tartalmazza az összes szükséges információt tooset NFS magas rendelkezésre állású kiszolgáló. Ez az útmutató használja kiindulópontként.


## <a name="overview"></a>Áttekintés

tooachieve magas rendelkezésre állású, SAP NetWeaver az NFS-kiszolgáló szükséges. hello NFS-kiszolgáló egy külön fürtben lett konfigurálva, és több SAP-rendszerek által használható.

![SAP NetWeaver magas rendelkezésre állás – Áttekintés](./media/high-availability-guide-suse/img_001.png)

hello NFS-kiszolgáló, a SAP NetWeaver ASC, SAP NetWeaver SCS, SAP NetWeaver SSZON és hello SAP HANA-adatbázisból virtuális állomásnév és a virtuális IP-címek használata. A Azure-ban egy terhelés-kiegyenlítő szükség toouse egy virtuális IP-címet. hello alábbi lista mutatja azokat hello terheléselosztó hello konfigurációja.

### <a name="nfs-server"></a>NFS-kiszolgáló
* Előtérbeli konfigurációja
  * IP-cím 10.0.0.4
* Háttérkonfiguráció
  * Az összes virtuális gépet, amely hello NFS fürt részét kell képezniük tooprimary hálózati adapterek csatlakoztatva
* Mintavételi portot
  * Port 61000
* Terheléselosztás szabályok
  * 2049 TCP 
  * 2049 UDP

### <a name="ascs"></a>(A) SCS
* Előtérbeli konfigurációja
  * IP-cím 10.0.0.10
* Háttérkonfiguráció
  * Az összes virtuális gépet, amely hello (A) SCS/SSZON fürt részét kell képezniük tooprimary csatlakoztatott hálózati illesztők
* Mintavételi portot
  * Port 620**&lt;nr&gt;**
* Terheléselosztás szabályok
  * 32**&lt;nr&gt;**  TCP
  * 36**&lt;nr&gt;**  TCP
  * 39**&lt;nr&gt;**  TCP
  * 81-es**&lt;nr&gt;**  TCP
  * 5**&lt;nr&gt;**13 TCP
  * 5**&lt;nr&gt;**14 TCP
  * 5**&lt;nr&gt;**16 TCP

### <a name="ers"></a>SSZON
* Előtérbeli konfigurációja
  * IP-cím 10.0.0.11
* Háttérkonfiguráció
  * Az összes virtuális gépet, amely hello (A) SCS/SSZON fürt részét kell képezniük tooprimary csatlakoztatott hálózati illesztők
* Mintavételi portot
  * Port 621**&lt;nr&gt;**
* Terheléselosztás szabályok
  * 33**&lt;nr&gt;**  TCP
  * 5**&lt;nr&gt;**13 TCP
  * 5**&lt;nr&gt;**14 TCP
  * 5**&lt;nr&gt;**16 TCP

### <a name="sap-hana"></a>SAP HANA
* Előtérbeli konfigurációja
  * 10.0.0.12 IP-cím
* Háttérkonfiguráció
  * Az összes virtuális gépet, amely hello HANA fürt részét kell képezniük tooprimary hálózati adapterek csatlakoztatva
* Mintavételi portot
  * Port a 625**&lt;nr&gt;**
* Terheléselosztás szabályok
  * 3**&lt;nr&gt;**15 TCP
  * 3**&lt;nr&gt;**17 TCP

## <a name="setting-up-a-highly-available-nfs-server"></a>Egy magas rendelkezésre állású NFS-kiszolgáló beállítása

### <a name="deploying-linux"></a>Linux telepítése

hello Azure piactér SUSE Linux Enterprise Server SAP alkalmazások 12 használható toodeploy új virtuális gépek a kép tartalmazza.
Használhat hello gyors üzembe helyezési sablonok a githubon toodeploy minden szükséges erőforrásokat. hello sablon telepíti hello virtuális gépek, a terheléselosztó hello, a rendelkezésre állási csoport stb. Kövesse a lépéseket toodeploy hello sablon:

1. Nyissa meg hello [SAP fájl server sablon] [ template-file-server] a hello Azure-portálon   
1. Adja meg a következő paraméterek hello
   1. Erőforrás-előtag  
      Adja meg a kívánt toouse hello előtag. hello érték előtagjaként hello telepített erőforrások esetén használatos.
   2. Operációs rendszer típusa  
      Válasszon ki egy hello Linux terjesztéseket. Ehhez a példához válassza ki a SLES 12 rendszert
   3. Rendszergazda felhasználónevét és a rendszergazdai jelszó  
      Új felhasználó jön létre, amely lehet használt toolog toohello gépen.
   4. Alhálózati azonosító  
      hello azonosító hello alhálózati toowhich hello virtuális gépek csatlakoznia kell. Hagyja üresen, ha szeretné, hogy egy új virtuális hálózat toocreate, vagy jelölje ki a VPN- vagy Express Route virtuális hálózati tooconnect hello virtuális gép tooyour a helyszíni hálózat hello alhálózatot. hello azonosítója általában a következőképpen néz következő**&lt;előfizetés-azonosító&gt;**/resourceGroups/**&lt;erőforráscsoport-név&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuálishálózat-név&gt;**/subnets/**&lt;alhálózat neve&gt;**

### <a name="installation"></a>Telepítés

hello következő elemek fűzve előtagként vagy **[A]** -alkalmazható tooall csomópontok **[1]** -csak a megfelelő toonode 1 vagy **[2]** -csak a megfelelő toonode 2.

1. **[A]**  SLES frissítése

   <pre><code>
   sudo zypper update
   </code></pre>

1. **[1]**  Ssh hozzáférés engedélyezése

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. **[2]**  Ssh hozzáférés engedélyezése

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. **[1]**  Ssh hozzáférés engedélyezése

   <pre><code>
   # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]**  Telepítése magas rendelkezésre ÁLLÁSÚ bővítmény
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. **[A]**  Állomásnév beállítása   

   DNS-kiszolgálót használjon, vagy módosítsa a hello/etc/hosts minden csomóponton. Ez a példa bemutatja, hogyan toouse hello/Etc/Hosts fájlt.
   Cserélje le a hello IP-cím és a következő parancsok hello hello állomásnév

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   Helyezze be a következő sorokat túl/etc/hosts hello. Hello IP cím és az állomásnév toomatch a környezet módosítása   
   
   <pre><code>
   # IP address of hello load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   </code></pre>

1. **[1]**  Fürt telepítése
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want toocontinue anyway? [y/N] -> y
   # Network address toobind too(for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish toouse SBD? [y/N] -> N
   # Do you wish tooconfigure an administration IP? [y/N] -> N
   </code></pre>

1. **[2]**  Csomópont toocluster hozzáadása
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured toostart at system boot.
   # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
   # Do you want toocontinue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. **[A]**  Módosítása hacluster jelszó toohello ugyanazt a jelszót

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. **[A]**  Corosync toouse más átviteli konfigurálása, és adja hozzá a csomópontlista. Fürt egyébként nem működik.
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

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
      # IP address of <b>prod-nfs-0</b>
      ring0_addr:10.0.0.5
     }
     node {
      # IP address of <b>prod-nfs-1</b>
      ring0_addr:10.0.0.6
     } 
   }</b>
   logging {
     [...]
   </code></pre>

   Indítsa újra hello corosync szolgáltatás

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. **[A]**  Drbd összetevőinek telepítése

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. **[A]**  Hozzon létre egy partíciót hello drbd eszköz

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. **[A]**  Létrehozása LVM konfigurációk

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_NFS /dev/sdc1
   sudo lvcreate -l 100%FREE -n <b>NWS</b> vg_NFS
   </code></pre>

1. **[A]**  Létrehozása hello NFS drbd eszköz

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_nfs.res
   </code></pre>

   Helyezze be a hello új drbd eszköz és a kilépési hello konfigurálása

   <pre><code>
   resource <b>NWS</b>_nfs {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>prod-nfs-0</b> {
         address   <b>10.0.0.5</b>:7790;
         device    /dev/drbd0;
         disk      /dev/vg_NFS/NWS;
         meta-disk internal;
      }
      on <b>prod-nfs-1</b> {
         address   <b>10.0.0.6</b>:7790;
         device    /dev/drbd0;
         disk      /dev/vg_NFS/NWS;
         meta-disk internal;
      }
   }
   </code></pre>

   Hello drbd eszköz létrehozása, és indítsa el

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_nfs
   sudo drbdadm up <b>NWS</b>_nfs
   </code></pre>

1. **[1]**  Kihagyása a kezdeti szinkronizálás

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_nfs
   </code></pre>

1. **[1]**  Set hello elsődleges csomópont

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_nfs
   </code></pre>

1. **[1]**  Várjon, amíg a hello új drbd eszköz szinkronizált

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:Connected ro:Primary/Secondary ds:UpToDate/UpToDate C r-----
   #    ns:0 nr:0 dw:0 dr:912 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. **[1]**  Fájlrendszerek hozható létre hello drbd eszközök

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   </code></pre>


### <a name="configure-cluster-framework"></a>Fürt keretrendszer konfigurálása

1. **[1]**  Hello alapértelmezett beállításainak módosítása

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Hozzáadás hello NFS drbd eszköz toohello fürtkonfiguráció

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_nfs \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_nfs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_nfs drbd_<b>NWS</b>_nfs \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true" interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Létrehozása hello NFS-kiszolgáló

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive nfsserver \
     systemd:nfs-server \
     op monitor interval="30s"

   crm(live)configure# clone cl-nfsserver nfsserver interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Hello NFS fájlrendszert erőforrások létrehozása

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive fs_<b>NWS</b>_sapmnt \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/srv/nfs/<b>NWS</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# group g-<b>NWS</b>_nfs fs_<b>NWS</b>_sapmnt

   crm(live)configure# order o-<b>NWS</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NWS</b>_nfs:promote g-<b>NWS</b>_nfs:start
   
   crm(live)configure# colocation col-<b>NWS</b>_nfs_on_drbd inf: \
     g-<b>NWS</b>_nfs ms-drbd_<b>NWS</b>_nfs:Master

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Hello NFS kivitel létrehozása

   <pre><code>
   sudo mkdir /srv/nfs/<b>NWS</b>/sidsys
   sudo mkdir /srv/nfs/<b>NWS</b>/sapmntsid
   sudo mkdir /srv/nfs/<b>NWS</b>/trans

   sudo crm configure

   crm(live)configure# primitive exportfs_<b>NWS</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NWS</b>" \
     options="rw,no_root_squash" \
     clientspec="*" fsid=0 \
     wait_for_leasetime_on_stop=true \
     op monitor interval="30s"

   crm(live)configure# modgroup g-<b>NWS</b>_nfs add exportfs_<b>NWS</b>

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. **[1]**  Hozzon létre egy virtuális IP- és állapot-mintavételi hello belső terheléselosztóhoz

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive vip_<b>NWS</b>_nfs IPaddr2 \
     params ip=<b>10.0.0.4</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_nfs anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 610<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0

   crm(live)configure# modgroup g-<b>NWS</b>_nfs add nc_<b>NWS</b>_nfs
   crm(live)configure# modgroup g-<b>NWS</b>_nfs add vip_<b>NWS</b>_nfs

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

### <a name="create-stonith-device"></a>STONITH eszköz létrehozása

hello STONITH eszköz által használt egy egyszerű tooauthorize Microsoft Azure ellen. Kövesse ezeket a lépéseket toocreate egy egyszerű szolgáltatást.

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

#### <a name="1-create-hello-stonith-devices"></a>**[1]**  Hello STONITH eszközök létrehozása

Után szerkeszteni hello engedélyek hello virtuális gépekhez, beállíthatja a hello STONITH eszközök hello fürtben.

<pre><code>
sudo crm configure

# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-hello-use-of-a-stonith-device"></a>**[1]**  STONITH eszköz hello használatának engedélyezése

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="setting-up-ascs"></a>(A) SCS beállítása

### <a name="deploying-linux"></a>Linux telepítése

hello Azure piactér SUSE Linux Enterprise Server SAP alkalmazások 12 használható toodeploy új virtuális gépek a kép tartalmazza. hello Piactéri lemezképhez hello erőforrás ügynök az SAP NetWeaver tartalmazza.

Használhat hello gyors üzembe helyezési sablonok a githubon toodeploy minden szükséges erőforrásokat. hello sablon telepíti hello virtuális gépek, a terheléselosztó hello, a rendelkezésre állási csoport stb. Kövesse a lépéseket toodeploy hello sablon:

1. Nyissa meg hello [ASC/SCS Multi SID sablon] [ template-multisid-xscs] vagy hello [sablon összevont] [ template-converged] hello Azure portál hello ASC/SCS sablon csak hello terheléselosztási szabályok hello SAP NetWeaver ASC/SCS és SSZON (csak Linux) példányok hoz létre, mivel hello átszervezett sablon is (például a Microsoft SQL Server vagy az SAP HANA) adatbázis hello terheléselosztási szabályokat hoz létre. Ha azt tervezi, hogy az SAP NetWeaver-alapú rendszerek tooinstall és tooinstall hello adatbázis is érdemes a hello azonos gépek, használja a hello [sablon összevont][template-converged].
1. Adja meg a következő paraméterek hello
   1. Erőforrás-előtag (csak a sablon ASC/SCS Multi SID)  
      Adja meg a kívánt toouse hello előtag. hello érték előtagjaként hello telepített erőforrások esetén használatos.
   3. SAP rendszerazonosító (csak az átszervezett sablon)  
      Adja meg a hello SAP rendszer azonosító hello tooinstall kívánt SAP rendszer. hello azonosító előtagjaként hello telepített erőforrások esetén használható.
   4. A készlet típusa  
      Hello SAP NetWeaver verem típusának kiválasztása
   5. Operációs rendszer típusa  
      Válasszon ki egy hello Linux terjesztéseket. Ehhez a példához válassza ki a SLES 12 saját
   6. DB típusa  
      Válassza ki a HANA
   7. SAP mérete  
      SAP hello új rendszer hello mennyisége biztosít. Ha nem biztos abban, hogy hány SAP hello rendszer igényel, kérje meg a SAP technológia Partner vagy a rendszer integráló
   8. Rendszer rendelkezésre állás  
      Válassza ki a magas rendelkezésre ÁLLÁSÚ
   9. Rendszergazda felhasználónevét és a rendszergazdai jelszó  
      Új felhasználó jön létre, amely lehet használt toolog toohello gépen.
   10. Alhálózati azonosító  
   hello azonosító hello alhálózati toowhich hello virtuális gépek csatlakoznia kell.  Hagyja üresen, ha azt szeretné, hogy egy új virtuális hálózat toocreate, vagy válasszon hello ugyanazon az alhálózaton, amely használja, vagy hozza létre hello NFS-kiszolgáló központi telepítésének részeként. hello azonosítója általában a következőképpen néz következő**&lt;előfizetés-azonosító&gt;**/resourceGroups/**&lt;erőforráscsoport-név&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuálishálózat-név&gt;**/subnets/**&lt;alhálózat neve&gt;**

### <a name="installation"></a>Telepítés

hello következő elemek fűzve előtagként vagy **[A]** -alkalmazható tooall csomópontok **[1]** -csak a megfelelő toonode 1 vagy **[2]** -csak a megfelelő toonode 2.

1. **[A]**  SLES frissítése

   <pre><code>
   sudo zypper update
   </code></pre>

1. **[1]**  Ssh hozzáférés engedélyezése

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. **[2]**  Ssh hozzáférés engedélyezése

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. **[1]**  Ssh hozzáférés engedélyezése

   <pre><code>
   # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. **[A]**  Telepítése magas rendelkezésre ÁLLÁSÚ bővítmény
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. **[A]**  Frissítés SAP erőforrás ügynökök  
   
   Egy javítást a hello erőforrás-ügynökök csomag szükséges toouse hello új konfigurációt, ebben a cikkben ismertetett. Ellenőrizheti, ha hello javítás már telepítve van a következő parancs hello

   <pre><code>
   sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   hello kimeneti hasonlónak kell lennie.

   <pre><code>
   &lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   Hello grep parancs nem található hello IS_ERS paraméter, ha szüksége van-e a felsorolt tooinstall hello javítás [hello SUSE letöltési oldala](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)

   <pre><code>
   # example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. **[A]**  Állomásnév beállítása   

   DNS-kiszolgálót használjon, vagy módosítsa a hello/etc/hosts minden csomóponton. Ez a példa bemutatja, hogyan toouse hello/Etc/Hosts fájlt.
   Cserélje le a hello IP-cím és a következő parancsok hello hello állomásnév

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   Helyezze be a következő sorokat túl/etc/hosts hello. Hello IP cím és az állomásnév toomatch a környezet módosítása   
   
   <pre><code>
   # IP address of hello load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of hello load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   </code></pre>

1. **[1]**  Fürt telepítése
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want toocontinue anyway? [y/N] -> y
   # Network address toobind too(for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish toouse SBD? [y/N] -> N
   # Do you wish tooconfigure an administration IP? [y/N] -> N
   </code></pre>

1. **[2]**  Csomópont toocluster hozzáadása
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured toostart at system boot.
   # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
   # Do you want toocontinue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. **[A]**  Módosítása hacluster jelszó toohello ugyanazt a jelszót

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. **[A]**  Corosync toouse más átviteli konfigurálása, és adja hozzá a csomópontlista. Fürt egyébként nem működik.
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

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
      # IP address of <b>nws-cl-0</b>
      ring0_addr:     10.0.0.14
     }
     node {
      # IP address of <b>nws-cl-1</b>
      ring0_addr:     10.0.0.13
     } 
   }</b>
   logging {
     [...]
   </code></pre>

   Indítsa újra hello corosync szolgáltatás

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. **[A]**  Drbd összetevőinek telepítése

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. **[A]**  Hozzon létre egy partíciót hello drbd eszköz

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. **[A]**  Létrehozása LVM konfigurációk

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_<b>NWS</b> /dev/sdc1
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ASCS vg_<b>NWS</b>
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ERS vg_<b>NWS</b>
   </code></pre>

1. **[A]**  Létrehozása hello SCS drbd eszköz

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ascs.res
   </code></pre>

   Helyezze be a hello új drbd eszköz és a kilépési hello konfigurálása

   <pre><code>
   resource <b>NWS</b>_ascs {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>nws-cl-0</b> {
         address   <b>10.0.0.14</b>:7791;
         device    /dev/drbd0;
         disk      /dev/vg_NWS/NWS_ASCS;
         meta-disk internal;
      }
      on <b>nws-cl-1</b> {
         address   <b>10.0.0.13</b>:7791;
         device    /dev/drbd0;
         disk      /dev/vg_NWS/NWS_ASCS;
         meta-disk internal;
      }
   }
   </code></pre>

   Hello drbd eszköz létrehozása, és indítsa el

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ascs
   sudo drbdadm up <b>NWS</b>_ascs
   </code></pre>

1. **[A]**  Létrehozása hello SSZON drbd eszköz

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ers.res
   </code></pre>

   Helyezze be a hello új drbd eszköz és a kilépési hello konfigurálása

   <pre><code>
   resource <b>NWS</b>_ers {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>nws-cl-0</b> {
         address   <b>10.0.0.14</b>:7792;
         device    /dev/drbd1;
         disk      /dev/vg_NWS/NWS_ERS;
         meta-disk internal;
      }
      on <b>nws-cl-1</b> {
         address   <b>10.0.0.13</b>:7792;
         device    /dev/drbd1;
         disk      /dev/vg_NWS/NWS_ERS;
         meta-disk internal;
      }
   }
   </code></pre>

   Hello drbd eszköz létrehozása, és indítsa el

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ers
   sudo drbdadm up <b>NWS</b>_ers
   </code></pre>

1. **[1]**  Kihagyása a kezdeti szinkronizálás

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ascs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ers
   </code></pre>

1. **[1]**  Set hello elsődleges csomópont

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_ascs
   sudo drbdadm primary --force <b>NWS</b>_ers
   </code></pre>

1. **[1]**  Várjon, amíg a hello új drbd eszköz szinkronizált

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:93991268 nr:0 dw:93991268 dr:93944920 al:383 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   # 1: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:6047920 nr:0 dw:6047920 dr:6039112 al:34 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   # 2: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:5142732 nr:0 dw:5142732 dr:5133924 al:30 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. **[1]**  Fájlrendszerek hozható létre hello drbd eszközök

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   sudo mkfs.xfs /dev/drbd1
   </code></pre>


### <a name="configure-cluster-framework"></a>Fürt keretrendszer konfigurálása

**[1]**  Hello alapértelmezett beállításainak módosítása

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a>SAP NetWeaver telepítésének előkészítése

1. **[A]**  Létrehozása hello megosztott könyvtárak

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NWS</b>/SYS

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NWS</b>/SYS
   </code></pre>

1. **[A]**  Autofs konfigurálása
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add hello following line toohello file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Fájl létrehozása

   <pre><code>
   sudo vi /etc/auto.direct

   # Add hello following lines toohello file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   /usr/sap/<b>NWS</b>/SYS -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sidsys
   </code></pre>

   Indítsa újra a autofs toomount hello új megosztások

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. **[A]**  Fájl FELCSERÉLÉSE konfigurálása
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set hello property ResourceDisk.EnableSwap tooy
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set hello size of hello SWAP file with property ResourceDisk.SwapSizeMB
   # hello free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check hello SWAP space with command swapon
   # Size of hello swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   Indítsa újra a hello ügynök tooactivate hello módosítása

   <pre><code>
   sudo service waagent restart
   </code></pre>

### <a name="installing-sap-netweaver-ascsers"></a>SAP NetWeaver ASC/SSZON telepítése

1. **[1]**  Hozzon létre egy virtuális IP- és állapot-mintavételi hello belső terheléselosztóhoz

   <pre><code>
   sudo crm node standby <b>nws-cl-1</b>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_ASCS \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_ascs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_ASCS drbd_<b>NWS</b>_ASCS \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true"

   crm(live)configure# primitive fs_<b>NWS</b>_ASCS \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/usr/sap/<b>NWS</b>/ASCS<b>00</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# primitive vip_<b>NWS</b>_ASCS IPaddr2 \
     params ip=<b>10.0.0.10</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_ASCS anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 620<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   crm(live)configure# group g-<b>NWS</b>_ASCS nc_<b>NWS</b>_ASCS vip_<b>NWS</b>_ASCS fs_<b>NWS</b>_ASCS \
      meta resource-stickiness=3000

   crm(live)configure# order o-<b>NWS</b>_drbd_before_ASCS inf: \
     ms-drbd_<b>NWS</b>_ASCS:promote g-<b>NWS</b>_ASCS:start
   
   crm(live)configure# colocation col-<b>NWS</b>_ASCS_on_drbd inf: \
     ms-drbd_<b>NWS</b>_ASCS:Master g-<b>NWS</b>_ASCS
   
   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

   Győződjön meg arról, hogy hello fürt állapot rendben, és, hogy az összes erőforrás indulnak el. Nem fontos a csomópont hello erőforrások futnak.

   <pre><code>
   sudo crm_mon -r

   # Node nws-cl-1: standby
   # <b>Online: [ nws-cl-0 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      Stopped: [ nws-cl-1 ]
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   </code></pre>

1. **[1]**  SAP NetWeaver ASC telepítése  

   SAP NetWeaver ASC telepítse a legfelső szintű használatával egy virtuális állomásnevet, amely hello terheléselosztó előtér-konfiguráció hello ASC toohello IP-címe például hello első csomóponton <b>nws-ASC</b>, <b>10.0.0.10</b>és például használt hello mintavétel hello terheléselosztó hello példányszámának <b>00</b>.

   Hello sapinst paraméter SAPINST_REMOTE_ACCESS_USER tooallow a nem gyökér szintű felhasználó tooconnect toosapinst is használhatja.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. **[1]**  Hozzon létre egy virtuális IP- és állapot-mintavételi hello belső terheléselosztóhoz

   <pre><code>
   sudo crm node standby <b>nws-cl-0</b>
   sudo crm node online <b>nws-cl-1</b>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_ERS \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_ers" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_ERS drbd_<b>NWS</b>_ERS \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true"

   crm(live)configure# primitive fs_<b>NWS</b>_ERS \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd1 \
     directory=/usr/sap/<b>NWS</b>/ERS<b>02</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# primitive vip_<b>NWS</b>_ERS IPaddr2 \
     params ip=<b>10.0.0.11</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_ERS anything \
    params binfile="/usr/bin/nc" cmdline_options="-l -k 621<b>02</b>" \
    op monitor timeout=20s interval=10 depth=0

   crm(live)configure# group g-<b>NWS</b>_ERS nc_<b>NWS</b>_ERS vip_<b>NWS</b>_ERS fs_<b>NWS</b>_ERS

   crm(live)configure# order o-<b>NWS</b>_drbd_before_ERS inf: \
     ms-drbd_<b>NWS</b>_ERS:promote g-<b>NWS</b>_ERS:start
   
   crm(live)configure# colocation col-<b>NWS</b>_ERS_on_drbd inf: \
     ms-drbd_<b>NWS</b>_ERS:Master g-<b>NWS</b>_ERS
   
   crm(live)configure# commit
   # WARNING: Resources nc_NWS_ASCS,nc_NWS_ERS,nc_NWS_nfs violate uniqueness for parameter "binfile": "/usr/bin/nc"
   # Do you still want toocommit (y/n)? y

   crm(live)configure# exit
   
   </code></pre>
 
   Győződjön meg arról, hogy hello fürt állapot rendben, és, hogy az összes erőforrás indulnak el. Nem fontos a csomópont hello erőforrások futnak.

   <pre><code>
   sudo crm_mon -r

   # Node <b>nws-cl-0: standby</b>
   # <b>Online: [ nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      Stopped: [ nws-cl-0 ]
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      Stopped: [ nws-cl-0 ]
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   </code></pre>

1. **[2]**  SAP NetWeaver SSZON telepítése  

   SAP NetWeaver SSZON telepítése hello a második csomópont használatával egy virtuális állomásnevet, amely hello terheléselosztó előtér-konfiguráció hello SSZON toohello IP-címe például a legfelső szintű <b>nws-sszon</b>, <b>10.0.0.11</b> és például használt hello mintavétel hello terheléselosztó hello példányszámának <b>02</b>.

   Hello sapinst paraméter SAPINST_REMOTE_ACCESS_USER tooallow a nem gyökér szintű felhasználó tooconnect toosapinst is használhatja.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > SWPM SP 20 PL 05 vagy újabb verzióját használja. Korábbi verziók nem hello engedélyek helyesen beállítva, és hello telepítése sikertelen lesz.
   > 

1. **[1]**  Hello ASC/SCS és SSZON példány profilok igazítja
 
   * Asc/SCS profil

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_<b>ASCS00</b>_<b>nws-ascs</b>

   # Change hello restart command tooa start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)

   # Add hello following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector

   # Add hello keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * SSZON profil

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>

   # Add hello following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   </code></pre>


1. **[A]**  Keep-Alive konfigurálása

   SAP NetWeaver alkalmazáskiszolgáló hello és az ASC/SCS hello hello kommunikációját szoftveres terheléselosztóként üzemeljen keresztül történik. hello terheléselosztó inaktív kapcsolatok leválasztása után konfigurálható időtúllépés. tooprevent ez tooset hello SAP NetWeaver ASC/SCS profil egyik paraméterének kell és hello Linux rendszer beállításainak módosítására. Kérjük, olvassa el [SAP Megjegyzés 1410736] [ 1410736] további információt.
   
   hello ASC/SCS profil paraméter célzó/encni/set_so_keepalive már felvették hello utolsó lépésében megadja.

   <pre><code> 
   # Change hello Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. **[A]**  Hello SAP felhasználók konfigurálása hello telepítése után
 
   <pre><code>
   # Add sidadm toohello haclient group
   sudo usermod -aG haclient <b>nws</b>adm   
   </code></pre>

1. **[1]**  Hello ASC és SSZON SAP szolgáltatások toohello sapservice fájl

   Adja hozzá a hello ASC szolgáltatás bejegyzés toohello második csomópontnak, illetve másolási hello SSZON szolgáltatás bejegyzés toohello első csomópontnak.

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nws-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nws-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. **[1]**  Hello SAP-fürterőforrás létrehozása

   <pre><code>
   sudo crm configure property maintenance-mode="true"

   sudo crm configure

   crm(live)configure# primitive rsc_sap_<b>NWS</b>_ASCS<b>00</b> SAPInstance \
    operations $id=rsc_sap_<b>NWS</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NWS</b>_ASCS<b>00</b>_<b>nws-ascs</b> START_PROFILE="/sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ASCS<b>00</b>_<b>nws-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10

   crm(live)configure# primitive rsc_sap_<b>NWS</b>_ERS<b>02</b> SAPInstance \
    operations $id=rsc_sap_<b>NWS</b>_ERS<b>02</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b> START_PROFILE="/sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>" AUTOMATIC_RECOVER=false IS_ERS=true \
    meta priority=1000

   crm(live)configure# modgroup g-<b>NWS</b>_ASCS add rsc_sap_<b>NWS</b>_ASCS<b>00</b>
   crm(live)configure# modgroup g-<b>NWS</b>_ERS add rsc_sap_<b>NWS</b>_ERS<b>02</b>

   crm(live)configure# colocation col_sap_<b>NWS</b>_no_both -5000: g-<b>NWS</b>_ERS g-<b>NWS</b>_ASCS
   crm(live)configure# location loc_sap_<b>NWS</b>_failover_to_ers rsc_sap_<b>NWS</b>_ASCS<b>00</b> rule 2000: runs_ers_<b>NWS</b> eq 1
   crm(live)configure# order ord_sap_<b>NWS</b>_first_start_ascs Optional: rsc_sap_<b>NWS</b>_ASCS<b>00</b>:start rsc_sap_<b>NWS</b>_ERS<b>02</b>:stop symmetrical=false

   crm(live)configure# commit
   crm(live)configure# exit

   sudo crm configure property maintenance-mode="false"
   sudo crm node online <b>nws-cl-0</b>
   </code></pre>

   Győződjön meg arról, hogy hello fürt állapot rendben, és, hogy az összes erőforrás indulnak el. Nem fontos a csomópont hello erőforrások futnak.

   <pre><code>
   sudo crm_mon -r

   # Online: <b>[ nws-cl-0 nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   #      rsc_sap_NWS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-0</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      <b>Slaves: [ nws-cl-0 ]</b>
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #      rsc_sap_NWS_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-1</b>
   </code></pre>

### <a name="create-stonith-device"></a>STONITH eszköz létrehozása

hello STONITH eszköz által használt egy egyszerű tooauthorize Microsoft Azure ellen. Kövesse ezeket a lépéseket toocreate egy egyszerű szolgáltatást.

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

#### <a name="1-create-hello-stonith-devices"></a>**[1]**  Hello STONITH eszközök létrehozása

Után szerkeszteni hello engedélyek hello virtuális gépekhez, beállíthatja a hello STONITH eszközök hello fürtben.

<pre><code>
sudo crm configure

# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-hello-use-of-a-stonith-device"></a>**[1]**  STONITH eszköz hello használatának engedélyezése

Hello STONITH eszköz használatának engedélyezése

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="install-database"></a>A telepítési adatbázis

Ebben a példában egy SAP HANA replikációs telepítve és konfigurálva van. SAP HANA hello azonos hello SAP NetWeaver ASC/SCS és SSZON fürtöt fog futni. SAP HANA egy dedikált fürtön is telepíthet. Lásd: [magas rendelkezésre állás az SAP HANA Azure virtuális gépek (VM)] [ sap-hana-ha] további információt.

### <a name="prepare-for-sap-hana-installation"></a>SAP HANA-telepítés előkészítése

Általában javasoljuk LVM kötetek, amelyek adatokat tárolhatnak, és a naplófájlok. Tesztelési célokra toostore hello adatainak és naplókönyvtárainak közvetlenül egy egyszerű lemezen fájl is kiválaszthatja.

1. **[A]**  LVM  
   hello az alábbi példa azt feltételezi, hogy hello virtuális gépek rendelkezik-e a négy adatlemezt csatolni, amelyeket használt toocreate két kötet.
   
   Az összes lemezt, amelyet az toouse fizikai köteteket hozhat létre.
   
   <pre><code>
   sudo pvcreate /dev/sdd
   sudo pvcreate /dev/sde
   sudo pvcreate /dev/sdf
   sudo pvcreate /dev/sdg
   </code></pre>
   
   Hozzon létre egy kötet csoport hello adatfájlok, hello naplófájlok egy kötet csoport és egy hello megosztott könyvtárában SAP HANA
   
   <pre><code>
   sudo vgcreate vg_hana_data /dev/sdd /dev/sde
   sudo vgcreate vg_hana_log /dev/sdf
   sudo vgcreate vg_hana_shared /dev/sdg
   </code></pre>
   
   Hello logikai köteteket hozhat létre
   
   <pre><code>
   sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
   sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
   sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
   sudo mkfs.xfs /dev/vg_hana_data/hana_data
   sudo mkfs.xfs /dev/vg_hana_log/hana_log
   sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
   </code></pre>
   
   Hello csatlakoztatási könyvtárak létrehozása, és másolja az összes logikai kötet UUID hello
   
   <pre><code>
   sudo mkdir -p /hana/data
   sudo mkdir -p /hana/log
   sudo mkdir -p /hana/shared
   sudo chattr +i /hana/data
   sudo chattr +i /hana/log
   sudo chattr +i /hana/shared
   # write down hello id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
   sudo blkid
   </code></pre>
   
   Három logikai kötetek hello autofs bejegyzéseket létrehozni
   
   <pre><code>
   sudo vi /etc/auto.direct
   </code></pre>
   
   A sor toosudo vi /etc/auto.direct beszúrása
   
   <pre><code>
   /hana/data -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b>
   /hana/log -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b>
   /hana/shared -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b>
   </code></pre>
   
   Csatlakoztassa hello új köteteket
   
   <pre><code>
   sudo service autofs restart 
   </code></pre>

1. **[A]**  Egyszerű lemez  

   A kis vagy bemutató rendszerek, elhelyezhet egy lemezt a HANA adatainak és naplókönyvtárainak fájlokat. hello következő parancsok /dev/sdc hozza létre a partíciót, és formázza xfs.
   ```bash
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdd'
   sudo mkfs.xfs /dev/sdd1
   
   # write down hello id of /dev/sdd1
   sudo /sbin/blkid
   sudo vi /etc/auto.direct
   ```
   
   A sor too/etc/auto.direct beszúrása
   <pre><code>
   /hana -fstype=xfs :UUID=<b>&lt;UUID&gt;</b>
   </code></pre>
   
   Hello célkönyvtár és azt csatlakoztatja a hello lemez.
   
   <pre><code>
   sudo mkdir /hana
   sudo chattr +i /hana
   sudo service autofs restart
   </code></pre>

### <a name="installing-sap-hana"></a>SAP HANA telepítése

hello lépések alapuló hello 4 fejezete [SAP HANA SR teljesítmény optimalizált forgatókönyv útmutató] [ suse-hana-ha-guide] tooinstall SAP HANA replikációs. Olvassa el, mielőtt hello telepítés folytatásához.

1. **[A]**  Hdblcm hello HANA DVD-ről futtatva
   
   <pre><code>
   sudo hdblcm --sid=<b>HDB</b> --number=<b>03</b> --action=install --batch --password=<b>&lt;password&gt;</b> --system_user_password=<b>&lt;password for system user&gt;</b>

   sudo /hana/shared/<b>HDB</b>/hdblcm/hdblcm --action=configure_internal_network --listen_interface=internal --internal_network=<b>10.0.0/24</b> --password=<b>&lt;password for system user&gt;</b> --batch
   </code></pre>

1. **[A]**  SAP állomás ügynökök frissítése

   Hello legújabb SAP a gazdagép ügynöke archív letöltését hello [SAP Softwarecenter] [ sap-swcenter] és futtatási hello parancs tooupgrade hello ügynök következő. Cserélje le a hello elérési toohello toopoint toohello archívumfájl letöltött.
   <pre><code>
   sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <b>&lt;path tooSAP Host Agent SAR&gt;</b> 
   </code></pre>

1. **[1]**  Létrehozása HANA replikációs (rendszergazdaként)  

   Futtassa a következő parancs hello. Győződjön meg arról, hogy tooreplace félkövér karakterláncok (HANA rendszer azonosító HDB és példány újrahasznosítása 03) a SAP HANA-telepítés hello értékekkel.
   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
   hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN too<b>hdb</b>hasync' 
   hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
   </code></pre>

1. **[A]**  Keystore bejegyzés (rendszergazdaként) létrehozása

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>&lt;passwd&gt;</b>
   </code></pre>

1. **[1]**  Adatbázis biztonsági másolata (rendszergazdaként)

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
   </code></pre>

1. **[1]**  Toohello HANA sapsid felhasználói váltson, és hozzon létre hello elsődleges hely.

   <pre><code>
   su - <b>hdb</b>adm
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. **[2]**  Toohello HANA sapsid felhasználói váltson, és hello másodlagos hely létrehozásához.

   <pre><code>
   su - <b>hdb</b>adm
   sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>nws-cl-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

1. **[1]**  Fürterőforrások SAP HANA létrehozása

   Először hozzon létre hello topológia.
   
   <pre><code>
   sudo crm configure

   # replace hello bold string with your instance number and HANA system id
   
   crm(live)configure# primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b>   ocf:suse:SAPHanaTopology \
     operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
     op monitor interval="10" timeout="600" \
     op start interval="0" timeout="600" \
     op stop interval="0" timeout="300" \
     params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"
    
   crm(live)configure# clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
     meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>
   
   Ezután hozzon létre hello HANA erőforrások
   
   <pre><code>
   sudo crm configure

   # replace hello bold string with your instance number, HANA system id and hello frontend IP address of hello Azure load balancer. 
    
   crm(live)configure# primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
     operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
     op start interval="0" timeout="3600" \
     op stop interval="0" timeout="3600" \
     op promote interval="0" timeout="3600" \
     op monitor interval="60" role="Master" timeout="700" \
     op monitor interval="61" role="Slave" timeout="700" \
     params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
     DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"
    
   crm(live)configure# ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
     meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
     target-role="Started" interleave="true"
    
   crm(live)configure# primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
     meta target-role="Started" is-managed="true" \ 
     operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
     op monitor interval="10s" timeout="20s" \ 
     params ip="<b>10.0.0.12</b>" 

   crm(live)configure# primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
     params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
     op monitor timeout=20s interval=10 depth=0 

   crm(live)configure# group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
    
   crm(live)configure# colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
     msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  

   crm(live)configure# order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
     msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
    
   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

   Győződjön meg arról, hogy hello fürt állapot rendben, és, hogy az összes erőforrás indulnak el. Nem fontos a csomópont hello erőforrások futnak.

   <pre><code>
   sudo crm_mon -r

   # <b>Online: [ nws-cl-0 nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      <b>Slaves: [ nws-cl-0 ]</b>
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #      rsc_sap_NWS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-1</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   #      rsc_sap_NWS_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-0</b>
   #  Clone Set: cln_SAPHanaTopology_HDB_HDB03 [rsc_SAPHanaTopology_HDB_HDB03]
   #      <b>Started: [ nws-cl-0 nws-cl-1 ]</b>
   #  Master/Slave Set: msl_SAPHana_HDB_HDB03 [rsc_SAPHana_HDB_HDB03]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g_ip_HDB_HDB03
   #      rsc_ip_HDB_HDB03   (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      rsc_nc_HDB_HDB03   (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   # rsc_st_azure_1  (stonith:fence_azure_arm):      <b>Started nws-cl-0</b>
   # rsc_st_azure_2  (stonith:fence_azure_arm):      <b>Started nws-cl-1</b>
   </code></pre>

1. **[1]**  Telepítés hello SAP NetWeaver adatbázispéldány

   Telepítés hello SAP NetWeaver adatbázispéldány legfelső szintű használatával egy virtuális állomásnevet, amely hello terheléselosztó előtér-konfiguráció hello adatbázis toohello IP-címe például <b>nws-db</b> és <b>10.0.0.12</b>.

   Hello sapinst paraméter SAPINST_REMOTE_ACCESS_USER tooallow a nem gyökér szintű felhasználó tooconnect toosapinst is használhatja.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a>SAP NetWeaver alkalmazáskiszolgáló telepítése

Kövesse ezeket a lépéseket tooinstall SAP alkalmazáskiszolgáló. hello lépéseket alábbi azt feltételezik, hogy hello alkalmazáskiszolgáló hello ASC/SCS eltérő kiszolgálóra és HANA kiszolgálók telepíteni. Ellenkező esetben egyes (például a gazdagép névfeloldásához konfigurálása) alatt hello lépések nem szükségesek.

1. A telepítő állomásnév    
   DNS-kiszolgálót használjon, vagy módosítsa a hello/etc/hosts minden csomóponton. Ez a példa bemutatja, hogyan toouse hello/Etc/Hosts fájlt.
   Cserélje le a hello IP-cím és a következő parancsok hello hello állomásnév
   ```bash
   sudo vi /etc/hosts
   ```
   Helyezze be a következő sorokat túl/etc/hosts hello. Hello IP cím és az állomásnév toomatch a környezet módosítása    
    
   <pre><code>
   # IP address of hello load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of hello load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   # IP address of hello application server
   <b>10.0.0.8 nws-di-0</b>
   </code></pre>

1. Hello sapmnt könyvtár létrehozása

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. Autofs konfigurálása
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add hello following line toohello file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   Új fájl létrehozása

   <pre><code>
   sudo vi /etc/auto.direct

   # Add hello following lines toohello file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   </code></pre>

   Indítsa újra a autofs toomount hello új megosztások

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. Lapozófájl konfigurálása
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set hello property ResourceDisk.EnableSwap tooy
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set hello size of hello SWAP file with property ResourceDisk.SwapSizeMB
   # hello free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check hello SWAP space with command swapon
   # Size of hello swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   Indítsa újra a hello ügynök tooactivate hello módosítása

   <pre><code>
   sudo service waagent restart
   </code></pre>

1. SAP NetWeaver alkalmazáskiszolgáló telepítése

   Elsődleges vagy további SAP NetWeaver alkalmazások kiszolgáló telepítése.

   Hello sapinst paraméter SAPINST_REMOTE_ACCESS_USER tooallow a nem gyökér szintű felhasználó tooconnect toosapinst is használhatja.

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. Biztonságos tár SAP HANA frissítése

   Frissítés hello SAP HANA biztonságos tárolására toopoint toohello virtuális nevét hello SAP HANA replikációs beállítás.
   <pre><code>
   su - <b>nws</b>adm
   hdbuserstore SET DEFAULT <b>nws-db</b>:3<b>03</b>15 <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="next-steps"></a>Következő lépések
* [Az Azure virtuális gépek tervezési és megvalósítási az SAP][planning-guide]
* [Az SAP Azure virtuális gépek telepítése][deployment-guide]
* [Az SAP Azure virtuális gépek adatbázis-kezelő telepítése][dbms-guide]
* Hogyan tooestablish magas rendelkezésre állású és az Azure (nagy példányokat), az SAP HANA vész-helyreállítási terv: toolearn [SAP HANA (nagy példányok) magas rendelkezésre állási és vészhelyreállítási helyreállítási Azure](hana-overview-high-availability-disaster-recovery.md).
* Hogyan tooestablish magas rendelkezésre állású és az Azure virtuális gépeken, a SAP HANA vész-helyreállítási terv: toolearn [magas rendelkezésre állás az SAP HANA Azure virtuális gépek (VM)][sap-hana-ha]
