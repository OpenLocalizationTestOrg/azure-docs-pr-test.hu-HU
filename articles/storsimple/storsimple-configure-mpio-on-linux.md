---
title: "a StorSimple Linux gazdagép MPIO aaaConfigure |} Microsoft Docs"
description: "A csatlakoztatott StorSimple tooa Linux gazdagépen futó CentOS 6.6 az MPIO konfigurálása"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: tysonn
ms.assetid: ca289eed-12b7-4e2e-9117-adf7e2034f2f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: alkohli
ms.openlocfilehash: d9f7e02903243494c909313fb2c33ac690764274
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a>Az MPIO konfigurálása a StorSimple gazdagépen fut a CentOS
Ez a cikk ismerteti a hello lépéseket szükséges tooconfigure többutas I/O (MPIO) a Centos 6.6 gazdagép-kiszolgálón. hello gazdagép-kiszolgálón található csatlakoztatott tooyour Microsoft Azure StorSimple eszköz iSCSI-kezdeményezők keresztül magas rendelkezésre állásra. A részletes hello automatikus felderítés többutas eszközről és hello adott telepítő csak a StorSimple-köteteket ismerteti.

Ez az eljárás akkor alkalmazható tooall hello modelljei a StorSimple 8000 sorozat eszközeire.

> [!NOTE]
> Ez az eljárás nem használható a StorSimple virtuális eszköz. További információkért lásd: hogyan tooconfigure állomáskiszolgáló a virtuális eszköz.
> 
> 

## <a name="about-multipathing"></a>Többutas kapcsolatos
hello többutas funkció lehetővé teszi tooconfigure a kiszolgáló és a tárolóeszköz közötti több i/o-útvonal. Az i/o-elérési utak, amelyek külön kábelek, a kapcsolók, a hálózati adapterek és a tartományvezérlők fizikai SAN-kapcsolatokat. Többutas hello i/o elérési utak, egy új eszközt társított összes összesített hello görbét tooconfigure összesíti.

hello többutas célja kettős:

* **Magas rendelkezésre állású**: hello i/o-elérési utat (például egy kábel, kapcsoló, hálózati adapter vagy tartományvezérlő) bármely elemének meghibásodásakor egy alternatív útvonalat biztosít.
* **Terheléselosztás**: a tárolóeszköz hello konfigurációjától függően az hello a jobb teljesítmény érdekében terhelések hello i/o-elérési utak a észlelésére, és dinamikusan újraelosztás e terhelések.

### <a name="about-multipathing-components"></a>Többutas összetevőivel kapcsolatos
A Linux többutas áll kernel és felhasználói térben összetevői, az alábbi táblázatban látható.

* **Kernel**: hello fő összetevője hello *eszköz-leképező* , amely reroutes i/o és elérési utak és elérési útja csoportok támogatja a feladatátvételt.

* **Felhasználói térben**: ezek a *Többutas-eszközök* , amely multipathed eszközök kezelése modulban a hello eszköz-többutas leképezőmodulja milyen toodo. hello eszközök foglalják magukban:
   
   * **Többutas**: sorolja fel, és beállítja az multipathed eszközöket.
   * **Multipathd**: démon, amely végrehajtja a többutas és a figyelők hello elérési utat.
   * **Devmap-name**: rendelkezik egy jelentéssel bíró eszköznév tooudev devmaps.
   * **Kpartx**: lineáris devmaps toodevice partíciók toomake többutas maps particionálható rendeli.
   * **MultiPath.conf**: ki, hogy a használt toooverwrite hello beépített konfigurációs tábla többutas démon konfigurációs fájlt.

### <a name="about-hello-multipathconf-configuration-file"></a>Hello multipath.conf konfigurációs fájllal kapcsolatos
hello konfigurációs fájl `/etc/multipath.conf` teszi hello számos többutas szolgáltatások felhasználó által konfigurálható. Hello `multipath` parancs és hello kernel démon `multipathd` használja a fájlban található információkat. hello fájl van csak a hello Többutas eszközök hello konfigurálása során megkeresett. Győződjön meg arról, hogy minden módosításai hello futtatása előtt `multipath` parancsot. Ha hello fájl módosítása ezután lesz toostop kell és indítsa el újra az hello módosítások tootake hatás multipathd.

hello multipath.conf öt részből áll:

- **Rendszer alapbeállításainak** *(alapértelmezett)*: rendszer alapbeállításainak felülbírálható.
- **Eszközök feketelistára teszi** *(blacklist)*: hello eszköz-leképező nem kell vezérelnie eszközök listáját is megadhat.
- **Kivételek tiltólistára** *(blacklist_exceptions)*: azonosíthatja, konkrét eszközökhöz toobe Többutas eszközök számít, még akkor is, ha hello blacklist szerepel.
- **Tárolási vezérlő adott beállítások** *(eszközök)*: megadhatja a szállító és a termékkulcsot rendelkező alkalmazott toodevices lesz konfigurációs beállításait.
- **Adott eszközbeállítások** *(multipaths)*: Ez a szakasz toofine-hangolási hello konfigurációs beállítások különálló logikaiegység-számok használható.

## <a name="configure-multipathing-on-storsimple-connected-toolinux-host"></a>Többutas konfigurálása a StorSimple csatlakoztatott tooLinux gazdagépen
A StorSimple-eszköz csatlakoztatásakor tooa Linux-állomáshoz konfigurálhatja a magas rendelkezésre állású, és a terheléselosztás. Például akkor, ha hello Linux-állomáshoz csatlakoztatott toohello SAN és hello eszköznek két csatolót két csatolót csatlakoztatva toohello SAN úgy, hogy ezek interfészekre hello ugyanazon az alhálózaton, akkor nem lesznek 4 útvonal érhető el. Azonban ha minden adatillesztő hello eszköz és a gazdagép illesztőn a különböző IP-alhálózat (és nem irányítható), majd csak 2 elérési utak lesz elérhető. Beállíthatja használjon többutas tooautomatically hello elérhető görbékhez felderítése, válassza ki az adott elérési útján a terheléselosztási algoritmust, csak a StorSimple-köteteket, megadott konfigurációs beállítások alkalmazása engedélyezése és többutas ellenőrzése.

hello alábbi eljárás leírja, hogy ha két hálózati adapterrel rendelkező StorSimple eszköz tooconfigure többutas csatlakoztatott tooa gazdagépet két hálózati adapterrel rendelkezik.

## <a name="prerequisites"></a>Előfeltételek
Ez a szakasz részletesen hello konfigurációs előfeltételeit CentOS kiszolgáló és a StorSimple eszközt.

### <a name="on-centos-host"></a>A CentOS gazdagépen
1. Győződjön meg arról, hogy a CentOS gazdagépen engedélyezett 2 hálózati illesztőt. Típus:
   
    `ifconfig`
   
    hello következő példa bemutatja, hello kimeneti amikor két hálózati illesztőt (`eth0` és `eth1`) hello gazdagépen találhatók.
   
        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
          inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
         RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)
   
        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
          inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)
   
        loLink encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)
2. Telepítés *iSCSI-kezdeményező-utils* a CentOS kiszolgálón. Hajtsa végre a következő lépéseket tooinstall hello *iSCSI-kezdeményező-utils*.
   
   1. Jelentkezzen be `root` a CentOS gazdagép be.
   2. Telepítse a hello *iSCSI-kezdeményező-utils*. Típus:
      
       `yum install iscsi-initiator-utils`
   3. Hello után *iSCSI-kezdeményező-utils* sikeresen van telepítve, indítsa el a hello iSCSI szolgáltatást. Típus:
      
       `service iscsid start`
      
       Alkalommal `iscsid` előfordulhat, hogy ténylegesen nem start és hello `--force` beállítás szükséges
   4. hogy az iSCSI-kezdeményező engedélyezve van-e rendszerindító időszakban használata hello tooensure `chkconfig` tooenable hello szolgáltatás parancsot.
      
       `chkconfig iscsi on`
   5. tooverify, hogy megfelelően lett, a telepítő hello parancsot:
      
       `chkconfig --list | grep iscsi`
      
       Az alábbiakban egy példa látható a kimenetre.
      
           iscsi   0:off   1:off   2:on3:on4:on5:on6:off
           iscsid  0:off   1:off   2:on3:on4:on5:on6:off
      
       A fenti példa hello láthatja, hogy az iSCSI-környezetben működnek rendszerindítás futtatási szinten, 2, 3, 4 és 5.
3. Telepítés *eszköz-leképező-többutas*. Típus:
   
    `yum install device-mapper-multipath`
   
    hello telepítés indul el. Típus **Y** toocontinue, amikor a rendszer megerősítést kér.

### <a name="on-storsimple-device"></a>A StorSimple eszköz
A StorSimple eszköz kell rendelkezniük:

* Legalább két csatolót engedélyezni kell az iSCSI. tooverify, hogy két felületet a StorSimple eszköz iSCSI engedélyezve a következő lépéseket a klasszikus Azure portálon, a StorSimple eszköz hello hello hajtsa végre:
  
  1. Jelentkezzen be a StorSimple eszköz hello a klasszikus portálon.
  2. Jelölje ki azt a StorSimple Manager szolgáltatás **eszközök** , és válassza a hello StorSimple eszközre. Kattintson a **konfigurálása** és hello hálózati kapcsolat beállításainak ellenőrzése. A képernyőfelvételen két iSCSI-kompatibilis hálózati adapterrel rendelkező alább láthatók. Itt DATA 2 és a DATA 3, mind 10 GbE adapterek iSCSI engedélyezve vannak.
     
      ![Az MPIO StorsSimple DATA 2 config](./media/storsimple-configure-mpio-on-linux/IC761347.png)
     
      ![Az MPIO StorSimple adatok 3 Config](./media/storsimple-configure-mpio-on-linux/IC761348.png)
     
      A hello **konfigurálása** lap
     
     1. Győződjön meg arról, hogy mindkét hálózati adapterek iSCSI engedélyezve. Hello **iSCSI engedélyezve** mezőt állítsa túl**Igen**.
     2. Győződjön meg arról, hogy hello hálózati adapternek hello azonos sebességű is 1 gbe-s vagy 10 GbE kell lennie.
     3. Vegye figyelembe a hello-adapterek iSCSI-kompatibilis hello IPv4-címeket, és mentse hello gazdagépen későbbi használatra.
* a StorSimple eszköz iSCSI csatolóinak hello hello CentOS kiszolgálóról elérhetőnek kell lennie.
      tooverify, tooprovide hello IP-címek az StorSimple iSCSI-kompatibilis hálózati adapterek van szüksége a gazdagép-kiszolgálón. hello használt parancsok és adat2 megfelelő kimenet hello (10.126.162.25) és DATA3 (10.126.162.26) az alábbiakban tekintheti meg:
  
        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target

### <a name="hardware-configuration"></a>Hardverkonfiguráció
Azt javasoljuk, hogy csatlakozni, hello két iSCSI hálózati illesztők külön útvonalak a redundancia érdekében. hello az alábbi ábra hello ajánlott hardverkonfigurációja a magas rendelkezésre állás érdekében és terheléselosztás többutas a CentOS server és a StorSimple eszközt.

![A StorSimple tooLinux gazdagép MPIO hardverkonfiguráció](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

Ahogy az ábra megelőző hello:

* A StorSimple eszköz van egy aktív-passzív konfigurációban két vezérlőkkel.
* Két SAN kapcsolók csatlakoztatott tooyour eszközvezérlők.
* Két iSCSI-kezdeményezők engedélyezve vannak a StorSimple eszközt.
* Két hálózati adapterrel engedélyezve vannak a CentOS állomáson.

hello fent konfigurációs eredményez 4 külön útvonalak az eszköz és hello-állomás között, ha hello állomás és az adatok felületek irányítható.

> [!IMPORTANT]
> * Azt javasoljuk, hogy azt ne keverje 1 gbe-s és a 10 GbE hálózati adaptert használjon a többutas. Ha két hálózati adapterrel, akkor mindkét hello felületek hello azonos típusúnak kellene lennie.
> * A StorSimple eszköz DATA0, adat1, DATA4 és DATA5 1 GbE felületek mivel adat2 és DATA3 10 GbE hálózati adapterrel. |}
> 
> 

## <a name="configuration-steps"></a>Konfigurációs lépések
a többutas hello konfigurációs lépéseket tartalmaz, amely hello elérhető elérési utak hello terheléselosztási algoritmus toouse, többutas engedélyezése, és végül a hello konfigurációjának ellenőrzése megadása automatikus felderítés konfigurálása. A lépések részletei a következő részekben hello ismertet.

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a>1. lépés: Az automatikus felderítést használjon többutas konfigurálása
hello többutas által támogatott eszközök automatikusan felderíthető és konfigurálva.

1. Inicializálni `/etc/multipath.conf` fájlt. Típus:
   
     `mpathconf --enable`
   
    hello fent parancs létrehoz egy `sample/etc/multipath.conf` fájlt.
2. A többutas szolgáltatás elindítása. Típus:
   
    `service multipathd start`
   
    A következő kimeneti hello jelenik meg:
   
    `Starting multipathd daemon:`
3. Multipaths automatikus észleléséhez. Típus:
   
    `mpathconf --find_multipaths y`
   
    Hello alapértelmezett szakasza módosítani fogja a `multipath.conf` alább látható módon:
   
        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a>2. lépés: A StorSimple-köteteket többutas konfigurálása
Alapértelmezés szerint minden eszköz fekete hello multipath.conf fájlban felsorolt és művelet megkerülését eredményezte. Szüksége lesz toocreate blacklist kivételek tooallow többutas kötetek StorSimple eszközökhöz.

1. Hello szerkesztése `/etc/mulitpath.conf` fájlt. Típus:
   
    `vi /etc/multipath.conf`
2. Keresse meg a hello blacklist_exceptions szakasz hello multipath.conf fájlban. A StorSimple eszköz kell toobe ebben a szakaszban blacklist kivételként szerepel. Akkor is állítsa vissza megfelelő sort a fájl toomodify azt (használata csak hello adott modell hello eszköz használ) alább látható módon:
   
        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a>3. lépés: Ciklikus multiplexelés többutas konfigurálása
A terheléselosztási algoritmust minden hello elérhető multipaths toohello aktív vezérlő egy elosztott terhelésű, a ciklikus multiplexelési módon használja.

1. Hello szerkesztése `/etc/multipath.conf` fájlt. Típus:
   
    `vi /etc/multipath.conf`
2. A hello `defaults` szakaszban, a set hello `path_grouping_policy` túl`multibus`. Hello `path_grouping_policy` hello alapértelmezett elérési út csoportosítási házirend tooapply toounspecified multipaths határozza meg. hello alapértelmezett szakasz jelenik meg, mint a lent látható módon.
   
        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }

> [!NOTE]
> a leggyakrabban használt értékek hello `path_grouping_policy` tartalmazza:
> 
> * feladatátvételi prioritási csoportonként 1 elérési útja =
> * multibus = 1 prioritás csoportban lévő összes érvényes elérési utat
> 
> 

### <a name="step-4-enable-multipathing"></a>4. lépés: Engedélyezze többutas
1. Indítsa újra a hello `multipathd` démon. Típus:
   
    `service multipathd restart`
2. hello kimeneti lesz a lent látható módon:
   
        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]

### <a name="step-5-verify-multipathing"></a>5. lépés: Ellenőrizze, hogy többutas
1. Először győződjön meg arról, hogy az iSCSI-kapcsolatot létesíteni az alábbiak szerint hello StorSimple eszköz:
   
   a. A StorSimple eszköz felderítése. Típus:
      
    ```
    iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on hello device>:<iSCSI port on StorSimple device>
    ```
    
    Ha IP-címet DATA0 10.126.162.25 és 3260-as port meg van nyitva, a kimenő iSCSI forgalomhoz hello StorSimple eszközön hello kimeneti van alább látható módon:
    
    ```
    10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    ```

    Másolás hello IQN-Nevének a StorSimple eszköz `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, a kimeneti megelőző hello.

   b. Csatlakoztassa a cél IQN toohello-eszközt. hello StorSimple eszköz hello iSCSI-tároló itt. Típus:

    ```
    iscsiadm -m node --login -T <IQN of iSCSI target>
    ```

    hello következő példa bemutatja a célkiszolgálóhoz IQN kimeneti a `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`. hello kimeneti azt jelzi, hogy sikeresen csatlakozott-e be a toohello két iSCSI-kompatibilis hálózati adapterek az eszközön.

    ```
    Logging in too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Logging in too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Login too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    Login too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    ```

    Ha csak egy állomás kapcsolat és két terveket itt látható, akkor szüksége tooenable két hello felülethez gazdagép iSCSI. Hajtsa végre az hello [részletes utasításokat Linux dokumentációjának](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).

2. A kötet az elérhetőségi toohello CentOS kiszolgáló hello StorSimple eszközön. További információkért lásd: [6. lépés: kötet létrehozása](storsimple-deployment-walkthrough.md#step-6-create-a-volume) hello a StorSimple eszközt a klasszikus Azure portálon keresztül.

3. Ellenőrizze a hello elérhető elérési utakat. Típus:

      ```
      multipath –l
      ```

      a következő példa hello két hálózati adapterrel hello kimenetét mutatja be egy StorSimple eszköz csatlakoztatott tooa egyetlen hálózati adapteren két elérhető elérési útját.

        ```
        mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 7:0:0:1 sdc 8:32 active undef running
        `- 6:0:0:1 sdd 8:48 active undef running
        ```

        hello following example shows hello output for two network interfaces on a StorSimple device connected tootwo host network interfaces with four available paths.

        ```
        mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 17:0:0:0 sdb 8:16 active undef running
        |- 15:0:0:0 sdd 8:48 active undef running
        |- 14:0:0:0 sdc 8:32 active undef running
        `- 16:0:0:0 sde 8:64 active undef running
        ```

        After hello paths are configured, refer toohello specific instructions on your host operating system (Centos 6.6) toomount and format this volume.

## <a name="troubleshoot-multipathing"></a>Többutas hibaelhárítása
Ez a szakasz néhány hasznos tipp nyújt, ha probléma merül fel többutas konfiguráció során.

Q. Nem látom hello változásai `multipath.conf` fájl lépnek érvénybe.

A. Ha bármely módosítások toohello végrehajtott `multipath.conf` fájl, akkor toorestart hello többutas szolgáltatás. Írja be a következő parancs hello:

    service multipathd restart

Q. Hello StorSimple eszközön két hálózati adapterrel és két hálózati adapterrel hello gazdagépen engedélyezve van. Hello elérhető útvonalak felsorolásához a csak két elérési útnak jelenik meg. Várt toosee négy elérhető elérési utakat.

A. Ellenőrizze, hogy a két hello elérési utak a hello ugyanazon az alhálózaton és irányítható. Ha hello hálózati adapterek különböző VLAN-on, és nem irányítható, látni fogja csak két elérési útnak. Egyirányú tooverify Ez az toomake meg arról, hogy egy adott hálózati csatoló hello StorSimple eszközön mindkét hello állomás illesztőket érhető el. Szüksége lesz túl[forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) , ez az ellenőrzés csak akkor lehet elvégezni a támogatási munkameneten keresztül.

Q. Amikor a rendelkezésre álló útvonalak felsorolásához kimenetet nem látható.

A. Általában nem jelennek meg minden multipathed elérési utak javasol hello többutas démon probléma, amely a legvalószínűbb ok az, hogy van-e bármilyen probléma hello a `multipath.conf` fájlt.

Érdemes ellenőrzése, hogy ténylegesen megjelenik egyes lemezek csatlakoztassa a toohello célként, mivel nincs válasz a hello többutas listaelemek is jelentheti, nem rendelkezik olyan lemezek is lenne.

* A következő parancs toorescan hello SCSI-busz hello használata:
  
    `$ rescan-scsi-bus.sh `(a sg3_utils csomag részeként)
* Írja be a következő parancsok hello:
  
    `$ dmesg | grep sd*`
     
     Vagy
  
    `$ fdisk –l`
  
    Ezek visszaadható nemrégiben felvett lemezek adatait.
* toodetermine hogy StorSimple lemez-e használni a következő parancsok hello:
  
    `cat /sys/block/<DISK>/device/model`
  
    Ezzel visszatér a karakterláncot, amely határozza meg, hogy a StorSimple lemez esetén.

A kevésbé valószínű, de lehetséges oka is lehet elavult iscsid azonosítója (PID). Használja ki a hello iSCSI-munkameneteket a következő parancs toolog hello:

    iscsiadm -m node --logout -p <Target_IP>

Ismételje meg ezt a parancsot minden hello csatlakoztatott hálózati adapterek hello iSCSI-tároló, amely a StorSimple eszközt. Miután az összes hello iSCSI-munkameneteket kijelentkezett hello iSCSI cél IQN tooreestablish hello iSCSI-munkamenetet használjon. Írja be a következő parancs hello:

    iscsiadm -m node --login -T <TARGET_IQN>


Q. Nem biztos, ha az eszköz nem szerepel az engedélyezési listán.

A. tooverify-e az eszköz szerepel az engedélyezési listán, használja a következő hibaelhárítási interaktív parancs hello:

    multipathd –k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


További információ: túl[hibaelhárítási többutas interaktív parancsot használja](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).

## <a name="list-of-useful-commands"></a>Hasznos parancsok listája
| Típus | Parancs | Leírás |
| --- | --- | --- |
| **iSCSI** |`service iscsid start` |ISCSI szolgáltatás elindítása |
| &nbsp; |`service iscsid stop` |ISCSI szolgáltatás leállítása |
| &nbsp; |`service iscsid restart` |ISCSI szolgáltatás újraindítása |
| &nbsp; |`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>` |A megadott hello elérhető tárolók felderítése cím |
| &nbsp; |`iscsiadm -m node --login -T <TARGET_IQN>` |Jelentkezzen be az iSCSI-tároló toohello |
| &nbsp; |`iscsiadm -m node --logout -p <Target_IP>` |Jelentkezzen ki a hello iSCSI-tároló |
| &nbsp; |`cat /etc/iscsi/initiatorname.iscsi` |Nyomtatási iSCSI-kezdeményező neve |
| &nbsp; |`iscsiadm –m session –s <sessionid> -P 3` |Hello iSCSI-munkamenetet, és hello gazdagépen felderített kötet hello állapotának ellenőrzéséhez. |
| &nbsp; |`iscsi –m session` |Megjeleníti az összes hello iSCSI-munkameneteket hello állomás és hello StorSimple eszköz között |
|  | | |
| **Többutas** |`service multipathd start` |Indítsa el a többutas démont |
| &nbsp; |`service multipathd stop` |A többutas démon leállítása |
| &nbsp; |`service multipathd restart` |Indítsa újra a többutas démon |
| &nbsp; |`chkconfig multipathd on` </br> VAGY </br> `mpathconf –with_chkconfig y` |Engedélyezze a többutas démon toostart rendszerindítás |
| &nbsp; |`multipathd –k` |Indítsa el az hello interaktív konzolját a hibaelhárításhoz |
| &nbsp; |`multipath –l` |Lista többutas kapcsolatok és eszközök |
| &nbsp; |`mpathconf --enable` |A minta mulitpath.conf fájl létrehozása`/etc/mulitpath.conf` |
|  | | |

## <a name="next-steps"></a>Következő lépések
Konfigurálja az MPIO Linux-gazdagépre, mert a következő CentoS 6.6 dokumentumok toorefer toohello is szükség lehet:

* [A CentOS MPIO beállítása](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
* [Linux-képzési útmutató](http://linux-training.be/files/books/LinuxAdm.pdf)

