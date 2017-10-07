---
title: "Figyelés megoldás Naplóelemzési aaaVMware |} Microsoft Docs"
description: "További információk a hello VMware figyelésére szolgáló megoldás hogyan segíthet naplóinak kezeléséhez és az ESXi-gazdagépek figyelésére."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 16516639-cc1e-465c-a22f-022f3be297f1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.openlocfilehash: 959d5c2201fc5c7947f8b8870d055cdf9eea8e01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>A Naplóelemzési megoldás VMware figyelése (előzetes verzió)

![VMware szimbólum](./media/log-analytics-vmware/vmware-symbol.png)

hello VMware figyelésére szolgáló megoldás a Naplóelemzési olyan megoldás, amely segítségével hozhat létre a központi naplózás és a nagy VMware-naplók figyelési megközelítést. Ez a cikk ismerteti, hogyan hibaelhárítása, rögzítése és hello ESXi-gazdagépek hello segítségével egy helyen kezelheti. Hello megoldás révén láthatja részletes adatok egy helyen az ESXi gazdagépek. Felső esemény száma, az állapot és a trendeket a virtuális gép és az ESXi gazdagépek hello ESXi-gazdagép naplók keresztül tekintheti meg. Elháríthatja az megtekintésével és központosított ESXi-gazdagép naplók keresése. És riasztások napló keresési lekérdezések alapján is létrehozhat.

hello megoldást használja, natív syslog hello ESXi állomás toopush adatok tooa cél virtuális gép, amelyen OMS-ügynököt. Azonban az hello megoldás nem írni az fájlok az syslog hello a cél virtuális gép. hello OMS-ügynököt 1514 portot nyit meg, és toothis figyeli. Hello adatok kap, miután hello OMS-ügynököt az OMS leküldéses értesítések hello adatokat.

## <a name="installing-and-configuring-hello-solution"></a>Telepítése és konfigurálása hello megoldás
A következő információk tooinstall hello használja, és hello megoldás konfigurálása.

* Adja hozzá a hello VMware figyelési megoldást tooyour ismertetett eljárással hello OMS-munkaterület [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).

#### <a name="supported-vmware-esxi-hosts"></a>Támogatott VMware ESXi-gazdagépek
a vSphere ESXi-gazdagép 5.5 és 6.0

#### <a name="prepare-a-linux-server"></a>Linux-kiszolgáló előkészítése
Hozzon létre egy Linux operációs rendszer virtuális gép tooreceive összes syslog-adatot hello ESXi-gazdagépek. Hello [OMS Linux-ügynök](log-analytics-linux-agents.md) hello gyűjtemény pontja minden ESXi állomás syslog-adatot. Több ESXi gazdagépek tooforward naplók tooa Linux egykiszolgálós, mint például a következő hello is használhatja.  

   ![Syslog folyamat](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Syslog gyűjtemény konfigurálása
1. A VSphere syslog-továbbító beállítása. Részletes információk a syslog-továbbító beállítása toohelp, lásd: [syslog konfigurálása ESXi 5.x és 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Nyissa meg túl**ESXi-gazdagép-konfigurálás** > **szoftver** > **speciális beállítások** > **Syslog**.
   ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  
2. A hello *Syslog.global.logHost* mezőben adja meg a Linux-kiszolgáló és a hello portszámot *1514*. Például `tcp://hostname:1514` vagy`tcp://123.456.789.101:1514`
3. Nyissa meg az ESXi-gazdagép tűzfalának hello a syslog. **ESXi-gazdagép-konfigurálás** > **szoftver** > **biztonsági profil** > **tűzfal** , és nyissa meg **tulajdonságok**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  
4. Ellenőrizze, hogy a syslog helyesen van beállítva hello vSphere konzol tooverify. Erősítse meg a hello ESXI-gazdagép, hogy a port **1514** van konfigurálva.
5. Töltse le és Linux hello OMS-ügynököt hello Linux-kiszolgálóra telepíthető. További információkért lásd: hello [Linux az OMS-ügynököt dokumentációjában](https://github.com/Microsoft/OMS-Agent-for-Linux).
6. Hello Linux OMS-ügynök telepítése után nyissa meg toohello /etc/opt/microsoft/omsagent/sysconf/omsagent.d és másolási hello vmware_esxi.conf fájl toohello /etc/opt/microsoft/omsagent/conf/omsagent.d könyvtárat, és hello hello tulajdonost/csoport módosítása és hello fájl engedélyeit. Példa:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
   sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```
7. Indítsa újra az OMS-ügynököt hello Linux futtatásával `sudo /opt/microsoft/omsagent/bin/service_control restart`.
8. Hello Linux kiszolgáló és az ESXi-állomáson hello között hello kapcsolat tesztelése hello segítségével `nc` parancs hello ESXi-állomáson. Példa:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection too123.456.789.101 1514 port [tcp/*] succeeded!
    ```

9. Az OMS-portálon hello, hajtsa végre a napló keresése `Type=VMware_CL`. OMS hello syslog adatokat gyűjt, hello syslog formátumba megmaradnak. Hello portálon bizonyos területeken a rendszer rögzíti, például a *állomásnév* és *Folyamatnév*.  

    ![type](./media/log-analytics-vmware/type.png)  

    Ha a napló keresési eredmények megtekintése a fenti hasonló toohello kép, toouse hello OMS VMware figyelési megoldást irányítópult beállítás kész.  

## <a name="vmware-data-collection-details"></a>VMware az gyűjtemény adatait
hello VMware figyelésére szolgáló megoldás különböző metrikák és a napló teljesítményadatokat gyűjt ESXi-gazdagépek hello OMS-ügynököt használ, Linux, amelyeken engedélyezve.

hello következő táblázatban adatgyűjtési módszerek és egyéb adatok gyűjtése hogyan részleteit.

| Platform | Linux OMS-ügynök | SCOM-ügynököt | Azure Storage | SCOM szükséges? | Felügyeleti csoport keresztül küldött SCOM ügynök adatok | Gyűjtemény gyakorisága |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |&#8226; |  |  |  |  |át 3 percenként |

a következő táblázat hello példák hello VMware figyelésére szolgáló megoldás által gyűjtött adatok mezők megjelenítése:

| Mező neve | leírás |
| --- | --- |
| Device_s |VMware-tárolóeszközök |
| ESXIFailure_s |Hiba típusa |
| EventTime_t |idő, amikor esemény történt |
| HostName_s |ESXi-gazdagép neve |
| Operation_s |Hozzon létre virtuális Gépet vagy virtuális gép törlése |
| ProcessName_s |esemény neve |
| ResourceId_s |hello VMware gazdagép neve |
| ResourceLocation_s |VMware |
| ResourceName_s |VMware |
| ResourceType_s |Hyper-V |
| SCSIStatus_s |VMware SCSI-állapot |
| SyslogMessage_s |Syslog-adatot |
| UserName_s |felhasználó létrehozása vagy törlése a virtuális gép |
| VMName_s |a virtuális gép neve |
| Computer |számítógép |
| TimeGenerated |hello időadatok jött létre. |
| DataCenter_s |VMware datacenter |
| StorageLatency_s |tárolási késés (ms) |

## <a name="vmware-monitoring-solution-overview"></a>VMware-figyelés megoldás áttekintése
hello VMware csempe hello OMS-portálon jelenik meg. Az esetleges hibákat magas szintű áttekintést nyújt a. Hello csempére kattint, amikor belép egy irányítópult-nézet.

![Mozaik elrendezés](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-hello-dashboard-view"></a>Keresse meg a hello irányítópult-nézet
A hello **VMware** irányítópult-nézethez paneleken szerint vannak rendszerezve:

* Hibaszámláló állapota
* Esemény felső állomások számolják
* Első esemény száma
* Virtuálisgép-tevékenységek
* ESXi-állomáson, lemez-események

![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Kattintson a panel tooopen Naplóelemzési keresési ablaktáblán, amely kifejezetten az hello panel részletes információit jeleníti meg.

Itt szerkesztheti hello keresési lekérdezés toomodify valamilyen konkrét azt. Hello alapjait OMS keresési oktatóanyag, tekintse meg a hello [OMS napló keresési oktatóanyag.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>Olyan esemény megkeresése ESXi állomás
Egyetlen ESXi-gazdagép több naplókat, a folyamatok alapján hoz létre. hello VMware figyelésére szolgáló megoldás központosítja azokat, és hello esemény számát foglalja össze. Ez a nézet központi segítségével megismerheti, hogy mely ESXi-állomáson van egy nagy mennyiségű esemény, és milyen eseményeket fordulnak elő a leggyakrabban a környezetben.

![Esemény](./media/log-analytics-vmware/events.png)

Megtekintheti az ESXi-állomáson, vagy egy eseménytípust kattintva további.

Ha egy ESXi állomásnév gombra kattint, meg adott ESXi-állomáson, adatait is megtekintheti. Ha a hello eseménytípus toonarrow eredményét, adja hozzá `“ProcessName_s=EVENT TYPE”` a keresési lekérdezés. Kiválaszthatja **Folyamatnév** hello keresési szűrő. Hello az adatokat, amelyek szűkíthető.

![Részletezés](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Nagy méretű tevékenységek keresése
A virtuális gépek hozható létre és bármely ESXi-állomáson, a rendszer törölheti. Akkor célszerű egy rendszergazda tooidentify az ESXi-állomáson hoz létre, hány virtuális gépeket. Amely szolgálna, toounderstand teljesítmény és kapacitás-tervezési. Virtuális gép tevékenységesemények szerinti nyomon követést elengedhetetlen a környezet kezeléséhez.

![Részletezés](./media/log-analytics-vmware/vmactivities1.png)

Toosee további ESXi-állomáson, virtuális gép létrehozási adatait, kattintson az ESXi-állomás neve.

![Részletezés](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Közös keresési lekérdezések
hello megoldás tartalmaz további hasznos lekérdezések, amelyek segítségével kezelheti az ESXi-gazdagépek például magas tárolóhelyet, a tárolóeszközök késése és elérési hiba.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Lekérdezések](./media/log-analytics-vmware/queries.png)


#### <a name="save-queries"></a>Lekérdezések mentése
Keresési lekérdezéseket elmenti az OMS szabványos szolgáltatása, és segítenek megőrizni, amely hasznos talált lekérdezéseket. Miután létrehozott egy lekérdezést, amely akkor hasznosak, mentse hello kattintva **Kedvencek**. Egy korábban mentett lekérdezés lehetővé teszi, hogy könnyen később újra felhasználhatja a hello [saját irányítópult](log-analytics-dashboards.md) lap, ahol a saját egyéni irányítópultok hozhatók létre.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Riasztások lekérdezések létrehozása
Miután létrehozta a lekérdezéseket, érdemes lehet a toouse hello lekérdezések tooalert meg adott események bekövetkezése esetén. Lásd: [Naplóelemzési riasztások](log-analytics-alerts.md) információ toocreate riasztásokat. A riasztások lekérdezéseket és egyéb lekérdezés példák, tekintse meg a hello [használatával az OMS szolgáltatáshoz figyelőt VMware](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) blogbejegyzést.

## <a name="frequently-asked-questions"></a>Gyakori kérdések
### <a name="what-do-i-need-toodo-on-hello-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Mit kell toodo hello ESXi állomás beállítását? Milyen hatással legyen benne az aktuális környezetben?
hello megoldás hello natív ESXi-gazdagép Syslog továbbítási mechanizmust alkalmaz. Nincs szükség semmilyen további Microsoft-szoftverek hello ESXi-állomáson toocapture hello naplókat. Ennek tartalmaznia kell egy kis hatás tooyour meglévő környezetben. Azonban meg kell tooset syslog továbbítási ESXI funkciót.

### <a name="do-i-need-toorestart-my-esxi-host"></a>Szükséges toorestart az ESXi-állomáson?
Nem. Ez a folyamat nem igényel újraindítást. Egyes esetekben vSphere megfelelően frissíteni hello syslog. Ebben az esetben jelentkezzen be toohello ESXi-állomáson, majd töltse be újra a hello syslog. Ebben az esetben toorestart hello gazdagépre, nem rendelkezik, így ez a folyamat nem zavaró tooyour környezet.

### <a name="can-i-increase-or-decrease-hello-volume-of-log-data-sent-toooms"></a>Növeli vagy csökkenti a naplóadatok tooOMS küldi hello kötet?
Igen is. A vSphere hello ESXi-gazdagép naplózási szint beállításokat is használhatja. Hello alapuló naplógyűjtést *információ* szintjét. Ezért, ha azt szeretné, tooaudit virtuális gép létrehozásakor vagy törlésekor, kell tookeep hello *info* Hostd szinten. További információkért lásd: hello [VMware Tudásbázis](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-toooms-my-log-setting-is-set-tooinfo"></a>Miért Hostd nem szolgáltat adatokat tooOMS? A napló tooinfo van állítva.
Hiba történt egy ESXi-gazdagép hiba az hello syslog időbélyegzési. További információkért lásd: hello [VMware Tudásbázis](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Hello megoldás telepítése után, általában Hostd kell működni.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-tooa-single-vm-with-omsagent"></a>Beállítható, hogy több ESXi gazdagépek syslog adatok tooa továbbítása VM egyszeri omsagent rendelkező?
Igen. Akkor is több ESXi gazdagépek tooa továbbítási egyetlen VM omsagent a.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Miért nem látom az OMS áramló adatokat?
Több oka is lehet:

* hello ESXi-állomáson, nem megfelelően küldését adatok toohello virtuális gép futó omsagent. tootest, hajtsa végre az alábbi lépésekkel hello:

  1. tooconfirm, jelentkezzen be az ssh használatával toohello ESXi-állomáson, és futtassa a következő parancs hello:`nc -z ipaddressofVM 1514`

      Ha ez nem sikeres, speciális konfiguráció várhatóan hello vSphere szolgáltatásbeállításai nem szünteti meg. Lásd: [syslog gyűjtemény konfigurálása](#configure-syslog-collection) hogyan mentése hello ESXi tooset syslog továbbítási állomásról kapcsolatos információkat.
  2. Ha syslog port kapcsolat sikeres, de még nem lát adatokat, majd töltse be újra hello syslog hello ESXi-állomáson használatával ssh parancs a következő toorun hello:` esxcli system syslog reload`
* hello OMS-ügynököt a virtuális gép helytelenül van beállítva. tootest, hajtsa végre az alábbi lépésekkel hello:

  1. OMS 1514 toohello portot figyeli, és leküldéses értesítések az adatok az OMS Szolgáltatáshoz. tooverify, hogy meg nyitva, futtassa a következő parancs hello:`netstat -a | grep 1514`
  2. Megtekintheti az port `1514/tcp` megnyitásához. Ha nem így tesz, győződjön meg arról, hogy hello omsagent megfelelően van-e telepítve. Ha nem látja hello portinformációkat, majd hello syslog port nincs nyitva a virtuális gép hello.

     1. Győződjön meg arról, hogy OMS-ügynök fut-e a hello `ps -ef | grep oms`. Ha nem fut, indítsa el hello folyamat hello parancs futtatásával` sudo /opt/microsoft/omsagent/bin/service_control start`
     2. Nyissa meg hello `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` fájlt.

         Győződjön meg arról, hogy hello megfelelő felhasználói és tárolócsoport-beállítás érvényes, hasonló:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

         Ha hello fájl nem létezik, vagy hello felhasználó és csoport beállítása nem megfelelő, intézkedéseket által [egy Linux-kiszolgálót elő kell készíteni](#prepare-a-linux-server).

## <a name="next-steps"></a>Következő lépések
* Használjon [napló keresések](log-analytics-log-searches.md) Naplóelemzési tooview részletes VMware-adatok tárolására.
* [Hozzon létre egy saját irányítópultok](log-analytics-dashboards.md) VMware gazdagép adatok jelennek meg.
* [Hozzon létre a riasztások](log-analytics-alerts.md) amikor adott VMware gazdagép esemény következik be.
