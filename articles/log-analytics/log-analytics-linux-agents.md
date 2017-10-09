---
redirect_url: /azure/log-analytics/log-analytics-agent-linux
redirect_document_id: True
ROBOTS: NOINDEX
ms.openlocfilehash: 8b526144cd565f6750368e12970f008e66cc2023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-linux-computers-toolog-analytics"></a>Csatlakozás a Linux rendszerű számítógépek tooLog elemzés
Log Analytics használ, összegyűjtése és Linux rendszerű számítógépek a létrehozott adatok rájuk. A Linux tooOMS során gyűjtött adatok hozzáadása lehetővé teszi az toomanage Linux rendszerek és tároló-megoldásokkal Docker, például a számítógépek helyétől függetlenül – pontjáról. Adatforrások lehet, hogy a helyszíni adatközpontban fizikai kiszolgálóként, például az Amazon Web Services (AWS) vagy a Microsoft Azure-felhőben üzemeltetett szolgáltatás a virtuális gépek találhatók, vagy még hello nehezedő hordozható. Emellett OMS is adatokat gyűjt a Windows rendszerű számítógépek hasonlóan, az támogatja-e egy valóban hibrid informatikai környezetben.

Megtekintheti és adatok kezelésében az összes egy egyetlen felügyeleti portállal forrásokra az OMS szolgáltatáshoz. Ez csökkenti a hello kell toomonitor használ a különböző rendszerek, így könnyen tooconsume, és adatokat toowhatever üzleti elemzési megoldások vagy a rendszer, amely már tetszés exportálhatja.

Ez a cikk egy gyors üzembe helyezési útmutató, amely segít a Linux rendszerű számítógépek OMS-ügynököt hello használata Linux adatok gyűjtésére és kezelésére. Például a proxykiszolgáló-konfigurációt, az CollectD metrikákat, és egyéni JSON-adatforrások további technikai részleteket, hogy információkat találhat [OMS-ügynököt Linux – áttekintés](https://github.com/Microsoft/OMS-Agent-for-Linux) és [OMS-ügynököt Linux teljes dokumentációja](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) a Githubon.

Jelenleg típusú adatokat a Linux rendszerű számítógépek következő hello hozhatja létre:

* Teljesítménymértékeket
* Syslog-események
* Nagios és Zabbix-riasztások
* Docker-tároló teljesítménymutatók, a készlet és a naplók

## <a name="supported-linux-versions"></a>Támogatott Linux-verziók
X86 és az x64 verziója Linux terjesztésekről számos hivatalosan támogatottak. Azonban hello Linux OMS-ügynököt előfordulhat, hogy is futtathatja más azokat a terjesztéseket nem szerepel a listában.

* Amazon Linux 2012.09 2015.09 keresztül
* CentOS Linux 5, 6 és 7
* Oracle Linux 5, 6 és 7
* Red Hat Enterprise Linux Server 5, 6, 7
* Debian GNU/Linux 6, 7 és 8
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10
* SUSE Linux Enterprise Server 11 és 12

## <a name="oms-agent-for-linux"></a>Linux OMS-ügynök
Operations Management Suite-ügynök Linux hello több csomagot tartalmazza. hello kiadás fájl tartalmazza a következő csomagok, a futó hello rendszerhéj köteg által elérhető hello `--extract`.

| **Csomag** | **Verzió** | **Leírás** |
| --- | --- | --- |
| omsagent |1.1.0 |Operations Management Suite-ügynök Linux hello |
| omsconfig |1.1.1 |Az OMS-ügynököt hello konfiguráció ügynök |
| OMI |1.0.8.3 |Open Management Infrastructure (OMI) – egy egyszerűsített CIM-kiszolgáló |
| az scx |1.6.2 |Operációs rendszer teljesítménymutatók OMI a CIM-szolgáltatók |
| Apache-cimprov |1.0.0 |Apache HTTP Server teljesítményfigyelés-szolgáltató az OMI. Csak akkor telepíthetők, ha a rendszer észleli az Apache HTTP Server. |
| MySQL-cimprov |1.0.0 |MySQL-kiszolgáló teljesítményfigyelés-szolgáltató az OMI. Csak akkor telepíthetők, ha a MySQL/MariaDB kiszolgáló észleli. |
| docker-cimprov |0.1.0 |Docker-szolgáltató az OMI. Csak akkor telepíthetők, ha a Docker észleli. |

### <a name="additional-installation-artifacts"></a>További telepítési összetevők
Miután telepítette a Linux-csomagok hello OMS-ügynököt, hello következő további rendszerszintű konfigurációs a módosításai érvényesek lesznek. Ezen összetevők hello omsagent csomag eltávolításakor a rendszer törli.

* Egy jogosulatlan felhasználó nevű: `omsagent` jön létre. Ez a hello fiók hello omsagent démon fut
* Egy sudoers "tartalmaz" fájl jön létre, /etc/sudoers.d/omsagent Ez engedélyezi a omsagent toorestart hello syslog és omsagent démonok. Ha a sudo "tartalmaz" direktívák nem támogatottak a sudo hello telepített verziója, ezek a bejegyzések kerülnek túl/etc/sudoers.
* hello Rendszernaplózás konfigurálásánál módosított tooforward események toohello ügynök egy részét. További információkért lásd: hello **adatok gyűjtésének konfigurálása** az alábbi szakasz

### <a name="linux-data-collection-details"></a>Linux az gyűjtemény adatait
hello következő táblázatban adatgyűjtési módszerek és egyéb adatok gyűjtése hogyan részleteit.

| Forrás | Közvetlen ügynök | SCOM-ügynököt | Azure Storage | SCOM szükséges? | Felügyeleti csoport keresztül küldött SCOM ügynök adatok | Gyűjtemény gyakorisága |
| --- | --- | --- | --- | --- | --- | --- |
| Zabbix |![Igen](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |1 perc |
| Nagios |![Igen](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |érkezésükkor |
| syslog |![Igen](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |az Azure storage: 10 perc; az ügynök: érkezésükkor |
| Linux-teljesítményszámlálók |![Igen](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |Ütemezés szerint, a 10 másodperces minimális |
| a változáskövetés |![Igen](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Nem](./media/log-analytics-linux-agents/oms-bullet-red.png) |óránként |

### <a name="package-requirements"></a>Csomag követelmények
| **Szükséges csomag** | **Leírás** | **Minimális verzió** |
| --- | --- | --- |
| Glibc |GNU C-függvénytár |2.5-12 |
| Openssl |OpenSSL-függvénytárak |0.9.8e vagy 1.0 |
| Curl |cURL-webügyfél |7.15.5 |
| Python-ctypes |függvény függvénytárak |n/a |
| A PAM |Cserélhető hitelesítési modulok |n/a |

> [!NOTE]
> Rsyslog vagy a syslog-ng van szükség toocollect syslog-üzeneteket. syslog-események gyűjtése nem támogatott hello alapértelmezett syslog démon a Red Hat Enterprise Linux-, CentOS, és Oracle Linux verziója (sysklog) 5-ös verzióját. ezek azokat a terjesztéseket, a jelen verziójában a syslog-adatot toocollect hello rsyslog démon kell telepíteni, és tooreplace sysklog konfigurálva.
>
>

## <a name="quick-install"></a>Gyors telepítés
Futtassa a következő parancsok toodownload hello omsagent hello, hello ellenőrzőösszeg, majd telepítse és előkészítésére hello ügynök ellenőrzése. Parancsok vannak a 64 bites. hello munkaterület azonosítója és az elsődleges kulcs megtalálható az OMS-portálon hello **beállítások** a hello **csatlakoztatott források** fülre.

![munkaterület részletei](./media/log-analytics-linux-agents/oms-direct-agent-primary-key.png)

```
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

Számos egyéb módszerek tooinstall ügynök hello és frissíteni. További róluk, [lépéseket tooinstall hello Linux OMS-ügynököt](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).

Emellett megtekinthet különböző hello [Azure bemutató videó](https://www.youtube.com/watch?v=mF1wtHPEzT0).

## <a name="choose-your-linux-data-collection-method"></a>A Linux-adatok gyűjtési módjának kiválasztása
Hogyan kezeli a hello adattípusok, például toocollect, attól függ, hogy szeretné-e toouse hello OMS-portálon, vagy ha azt szeretné, hogy közvetlenül a Linux-ügyfelek a különböző konfigurációs fájlok szerkeszthetők. Ha úgy dönt, hogy toouse hello portál, hello konfigurációs küld tooall a Linux-ügyfelek automatikusan. Ha különböző Linux-ügyfelek különböző konfigurációt, vagy tooedit ügyfélfájlok külön-külön – kell lesz, például a PowerShell DSC, Chef vagy Puppet helyett használja.

Megadhat hello syslog-események és teljesítményszámlálók, amelyet az toocollect hello Linux-számítógépeken konfigurációs fájlokat használja. *Ha úgy dönt, tooconfigure adatgyűjtés ügynök konfigurációs fájlok szerkesztésével, le kell tiltania hello központi konfigurációs.*  Útmutatás alább tooconfigure adatgyűjtés hello ügynök konfigurációs fájlokat, valamint toodisable központi konfigurálása-az összes OMS-ügynök a Linux vagy különálló számítógépeket.

### <a name="disable-oms-management-for-an-individual-linux-computer"></a>Tiltsa le az OMS egyéni Linux számítógép-kezelés
Konfigurációs adatok gyűjtésének központi le van tiltva a hello OMS_MetaConfigHelper.py parancsfájl egyes Linux számítógépeken. Ez akkor lehet hasznos, ha a számítógépek egy részhalmazát kell rendelkeznie egy speciális konfigurációja.

központi konfiguráció toodisable:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```

központi konfiguráció toore engedélyezése:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py –enable
```

## <a name="linux-performance-counters"></a>Linux-teljesítményszámlálók
Linux-teljesítményszámlálók hasonló tooWindows teljesítményszámlálók – is hasonló módon fog működni. A következő eljárások tooadd hello használja, és konfigurálja őket. Miután hozzáadták azokat tooOMS, adatgyűjtés számukra 30 másodpercenként.

### <a name="tooadd-a-linux-performance-counter-in-oms"></a>tooadd OMS Linux teljesítményszámláló
1. hello OMS-portálon Linux ügynökök OMS tooconfigure, adhat hozzá Linux teljesítményszámlálók hello beállítások lapján kattintson a **adatok**.  
2. A hello **beállítások** lapon az **adatok** , kattintson a **Linux teljesítményszámlálók** , majd válassza ki vagy típus hello név hello számláló tooadd szeretné.  
    ![adatok](./media/log-analytics-linux-agents/oms-settings-data01.png)
3. Ha nem tudja hello teljes hello számláló nevét, egy részleges név indítható, és elérhető számlálók listája jelenik meg. Ha megtalálta hello számláló meg tooadd szeretné, kattintson hello listában hello nevére, majd hello plusz ikon tooadd hello számláló.
4. Hello Számláló hozzáadása után színű sáv a kijelölt számlálók hello listája jelenik meg.
5. Alapértelmezés szerint hello **alábbi konfigurációs toomy számítógépek alkalmaz** beállítást. Ha azt szeretné, hogy a konfigurációs adatok küldése toodisable, törölje a jelet hello.
6. Ha befejezte az módosítása teljesítményszámlálók alján hello hello kattintson **mentése** toofinalize a módosításokat. hello végrehajtott konfigurációs módosításokat Linux általában 5 percnél regisztrált OMS-ben, majd küldése tooall hello OMS-ügynököt.

### <a name="configure-linux-performance-counters-in-oms"></a>Linux-teljesítményszámlálók OMS konfigurálása
Windows-teljesítményszámlálókat kiválaszthatja az egyes teljesítményszámlálókhoz bizonyos példányainak. Azonban Linux teljesítményszámlálókat, a számláló az Ön által választott példány tooall gyermek számlálók hello szülő számláló vonatkozik. hello következő táblázatban hello közös példányok elérhető tooboth Linux és a Windows a teljesítményszámlálókat.

| **Példány neve** | **Jelentése** |
| --- | --- |
| \_Összesen |Hello példányainak száma |
| \* |Minden példány |
| (/ &#124; / var) |Példány neve megegyezik: / vagy /var |

Ehhez hasonlóan hello mintavételi időköznek az Ön által a szülő számláló érvényes tooall a gyermek számlálókat. Ez azt jelenti az összes hello gyermek számláló minta időközök és példányok vannak társítva együtt.

### <a name="add-and-configure-performance-metrics-with-linux"></a>Hozzáadása és a Linux és metrikák konfigurálása
Teljesítmény-mérőszámok toocollect hello konfigurációs az/etc/opt/microsoft/omsagent által vezérelt/&lt;munkaterület azonosítója&gt;/conf/omsagent.conf. Lásd: [elérhetővé teljesítménymutatók](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) elérhető osztályok és metrikáinak Linux OMS-ügynököt hello.

Minden objektumot, vagy a teljesítmény-mérőszámok toocollect kategóriáját definiálni kell egy hello konfigurációs fájlban `<source>` elemet. hello szintaxisa az alábbi hello mintát követi.

```
<source>
  type oms_omi  
  object_name "Processor"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

Ennek az elemnek a hello konfigurálható paraméterek a következők:

* **Objektum\_neve**: hello gyűjtemény hello objektum nevét.
* **Példány\_regex**: egy *reguláris kifejezés* mely példányok toocollect meghatározása. érték hello: `.*` határozza meg az összes példányát. csak hello toocollect processzor metrikáját \_teljes példányt kell megadni `_Total`. a toocollect folyamatmetrikák csak hello crond vagy sshd példány, meg kell megadni: `(crond|sshd)`.
* **A számláló\_neve\_regex**: egy *reguláris kifejezés* mely számlálói (hello objektum) toocollect meghatározása. toocollect számlálók hello objektumhoz, adja meg: `.*`. toocollect terület számlálók csak felcserélése hello memória objektumhoz tartozó, kell megadni:`.+Swap.+`
* **Időköz:**: hello gyakorisága, mely hello objektum számlálók összegyűjtése.

a metrikák hello alapértelmezett konfigurációja a következő:

```
<source>
  type oms_omi
  object_name "Physical Disk"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Logical Disk"
  instance_regex ".*
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Processor"
  instance_regex ".*
  counter_name_regex ".*"
  interval 30s
</source>

<source>
  type oms_omi
  object_name "Memory"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

### <a name="enable-mysql-performance-counters-using-linux-commands"></a>Linux-parancsok használatával MySQL teljesítményszámlálók engedélyezése
MySQL vagy MariaDB Server észlelésekor hello számítógépen hello omsagent csomag telepítésekor a rendszer automatikusan telepíti a Teljesítményfigyelő MySQL kiszolgáló-szolgáltatója. Ez a szolgáltató toohello helyi MySQL/MariaDB kiszolgáló tooexpose teljesítményének statisztikájára csatlakozik. Érdekében, hogy a hello szolgáltató hello MySQL Server szüksége tooconfigure MySQL felhasználói hitelesítő adatokat.

toodefine egy alapértelmezett felhasználói fiókot a hello MySQL server – localhost, a következő parancs használata hello.

> [!NOTE]
> hello hitelesítőadat-fájlt hello omsagent fiókkal olvashatónak kell lennie. Hello mycimprovauth parancs futtatásához használt omsgent ajánlott.
>
>

```
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>

sudo /opt/omi/bin/service_control restart
```


Másik megoldásként megadhatja, hello MySQL hitelesítő adatokat egy fájlban szükséges hello fájl létrehozásával: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. Hello mysql-alapú hitelesítés fájl figyeléshez MySQL hitelesítő adatai kezelésével kapcsolatos további információkért lásd: [figyelési hello hitelesítési fájlt a hitelesítő adatok kezelése MySQL](#manage-mysql-monitoring-credentials-in-the-authentication-file).

Lásd: [MySQL teljesítményszámlálókkal szükséges engedélyek adatbázis](#database-permissions-required-for-mysql-performance-counters) hello MySQL felhasználói toocollect MySQL kiszolgáló teljesítményadatait szükséges objektum engedélyek vonatkozó további információért.

### <a name="enable-apache-http-server-performance-counters-using-linux-commands"></a>Linux-parancsok használatával Apache HTTP Server teljesítményszámlálói engedélyezése
Apache HTTP Server észlelésekor hello számítógépen hello omsagent csomag telepítésekor a rendszer automatikusan telepíti a teljesítményfigyelés-szolgáltató az Apache HTTP Server. Ez a szolgáltató egy Apache "module", amely töltse be az Apache HTTP Server hello rendelés tooaccess teljesítményadatok támaszkodik.

Hello modul is betöltése a következő parancsot a következő hello:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

toounload hello Apache figyelőmodult, futtassa a következő parancs hello:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```
### <a name="tooview-performance-data-with-log-analytics"></a>a Naplóelemzési tooview teljesítményadatok
1. A hello Operations Management Suite portálját kattintson a hello napló keresése csempe.
2. Hello keresősávban, írja be a `* (Type=Perf)` tooview teljesítményszámlálók.

Mivel OMS Windows teljesítményszámláló-adatokat is gyűjti, meg kell hatókör-le nyilas hello keresési tooLinux vonatkozó adatokat. Igen hello alábbi példa látható teljesítmény adatok adott tooan példa Linux server Chorizo21 nevű.

```
Type=Perf Computer=chorizo*
```

![a találatok között látható példa kiszolgáló](./media/log-analytics-linux-agents/oms-perfsearch01.png)

Hello eredményekben kattintson **metrikák** tooview hello számlálók az összegyűjtött adatokat. Valós idejű adatok jelenik meg az egyes számlálóit diagramjait.

![metrics](./media/log-analytics-linux-agents/oms-perfmetrics01.png)

## <a name="syslog"></a>Rendszernapló
Syslog egy eseménynaplózás protokoll hasonló tooWindows eseménynaplók – egyaránt működéséhez hasonlóan az OMS Szolgáltatáshoz.

### <a name="tooadd-a-new-linux-syslog-facility-in-oms"></a>egy új Linux syslog-szolgáltatást az OMS tooadd
1. A hello **beállítások** lapon az **adatok** , kattintson a **Syslog** és toohello balra hello plusz ikonra, írja be hello hello syslog-szolgáltatást, amelyet az tooadd.
    ![Linux rendszernaplójába bejegyzett](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)
2. Ha nem tudja hello létesítmény hello teljes nevét, egy részleges név indítható, és elérhető syslog létesítményekben listája jelenik meg. Ha megtalálta hello syslog-szolgáltatást, hogy meg szeretné, hogy tooadd, kattintson hello listában hello nevére, majd hello plusz ikon tooadd hello syslog-szolgáltatást.
3. Miután hello létesítmény hello listája megjelenik a kijelölt színű sáv. Ezután válassza ki a hello fokozatok (kategóriák syslog-létesítményt a személyes adatok), amelyet az toocollect.
4. Hello lap hello alján kattintson **mentése** toofinalize a módosításokat. hello végrehajtott konfigurációs módosításokat Linux általában 5 percnél regisztrált OMS-ben, majd küldése tooall hello OMS-ügynököt.

### <a name="configure-linux-syslog-facilities-in-linux"></a>Linux syslog létesítményekben lévő Linux konfigurálása
Syslog-események küldi hello a syslog démon, például rsyslog vagy a syslog-ng, helyi port tooa adott hello ügynök figyeli. Alapértelmezés szerint port 25224. Ha hello ügynök telepítve van, egy alapértelmezett Rendszernaplózás konfigurálásánál alkalmazva. Ez a következő címen található:

Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf

Syslog-ng: /etc/syslog-ng/syslog-ng.conf

hello alapértelmezett OMS ügynök Rendszernaplózás konfigurálásánál feltölti a syslog-események az összes létesítményekben kiegészített egy figyelmeztetés vagy magasabb.

> [!NOTE]
> Hello Rendszernaplózás konfigurálásánál szerkesztése esetén újra kell indítani a syslog démon hello hello módosítások tootake hatás.
>
>

hello alapértelmezett syslog beállítása az OMS-ügynököt hello Linux OMS történik:

#### <a name="rsyslog"></a>Rsyslog
```
kern.warning       @127.0.0.1:25224
user.warning       @127.0.0.1:25224
daemon.warning     @127.0.0.1:25224
auth.warning       @127.0.0.1:25224
syslog.warning     @127.0.0.1:25224
uucp.warning       @127.0.0.1:25224
authpriv.warning   @127.0.0.1:25224
ftp.warning        @127.0.0.1:25224
cron.warning       @127.0.0.1:25224
local0.warning     @127.0.0.1:25224
local1.warning     @127.0.0.1:25224
local2.warning     @127.0.0.1:25224
local3.warning     @127.0.0.1:25224
local4.warning     @127.0.0.1:25224
local5.warning     @127.0.0.1:25224
local6.warning     @127.0.0.1:25224
local7.warning     @127.0.0.1:25224
```

#### <a name="syslog-ng"></a>Syslog-ng
```
#OMS_facility = all
filter f_warning_oms { level(warning); };
destination warning_oms { tcp("127.0.0.1" port(25224)); };
log { source(src); filter(f_warning_oms); destination(warning_oms); };
```

### <a name="tooview-all-syslog-events-with-log-analytics"></a>tooview Naplóelemzési a Syslog-események
1. A hello Operations Management Suite portálját, kattintson a hello **naplófájl-keresési** csempére.
2. A hello **napló felügyeleti** csoportosítás, válasszon egy előre meghatározott syslog keresést, és válassza ki egy toorun azt.

Ez a példa bemutatja az összes Syslog-események.

![Syslog-események megjelennek a keresési napló](./media/log-analytics-linux-agents/oms-linux-syslog.png)

Most részletezhető keresési eredmények között.

## <a name="linux-alerts"></a>Linux-riasztások
Ha a Linux-gépeket, majd az OMS ezekhez az eszközökhöz által előállított hello riasztásokat fogadhat Nagios vagy Zabbix toomanage. Azonban nincs jelenleg metódus tooconfigure bejövő riasztási adat hello OMS-portálon. Ehelyett szüksége lesz egy konfigurációs fájl toostart küldő riasztások tooOMS tooedit.

### <a name="collect-alerts-from-nagios"></a>Nagios riasztások gyűjtése
toocollect riasztások Nagios-kiszolgálótól, a következő konfigurációs módosításainak toomake hello kell.

1. Támogatás hello felhasználói **omsagent** olvasási hozzáférés toohello Nagios naplófájl (azaz /var/log/nagios/nagios.log). Feltéve, hogy hello nagios.log fájl hello csoport tulajdonosa **nagios** , hozzáadhat hello felhasználót **omsagent** toohello **nagios** csoport.

    ```
    sudo usermod –a -G nagios omsagent
    ```
2. Hello omsagent.confconfiguration fájl módosítása (/ etc/opt/microsoft/omsagent/&lt;munkaterület azonosítója&gt;/conf/omsagent.conf). Győződjön meg arról, bejegyzéseket a következő hello jelen, és nem megjegyzésként ki:

    ```
    <source>
    type tail
    #Update path toopoint tooyour nagios.log
    path /var/log/nagios/nagios.log
    format none
    tag oms.nagios
    </source>

    <filter oms.nagios>
    type filter_nagios_log
    </filter>
    ```
3. Indítsa újra a hello omsagent démon:

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="collect-alerts-from-zabbix"></a>Zabbix riasztások gyűjtése
Nagios fenti, hasonló lépéseket toothose kivételével toospecify egy felhasználó és a jelszó lesz szüksége lesz hajtsa végre a Zabbix kiszolgálóról toocollect riasztások *törölje a szöveget*. Ez nem ideális, de hamarosan valószínűleg változik. tooaddress probléma, javasoljuk, hogy hello felhasználó létrehozása, és csak az engedély toomonitor biztosítania.

Egy hello omsagent.conf konfigurációs fájl példa részét (/ etc/opt/microsoft/omsagent/&lt;munkaterület azonosítója&gt;/conf/omsagent.conf) a Zabbix hello következő kell hasonlítania:

```
<source>
  type zabbix_alerts
  run_interval 1m
  tag oms.zabbix
  zabbix_url http://localhost/zabbix/api_jsonrpc.php
  zabbix_username Admin
  zabbix_password zabbix
</source>

```

### <a name="view-alerts-in-log-analytics-search"></a>A Naplóelemzési keresési riasztások megtekintése
Miután konfigurálta a Linux rendszerű számítógépek toosend riasztások tooOMS, néhány egyszerű napló keresési lekérdezések tooview hello értesítések is használhatja. hello következő keresési lekérdezés példa adja vissza az összes rögzített hello riasztás. Például ha valamilyen probléma lép fel az informatikai infrastruktúra, majd jár-e a következő példában a lekérdezés jelezhetik, ahol előfordulhat, hogy származnak hello probléma hello. És egyszerűen megtekintheti a toohello riasztások forrás rendszer toohelp keskeny által a vizsgálat. hello előnyt az jelenti, hogy nem szükségszerűen rendelkezik toogo toovarious felügyeleti rendszerekkel hello indítás – feltéve, hogy az értesítések küldése tooOMS megkezdése van.

```
Type=Alert
```

#### <a name="tooview-all-nagios-alerts-with-log-analytics"></a>minden Nagios riasztásokkal Naplóelemzési tooview
1. A hello Operations Management Suite portálját, kattintson a hello **naplófájl-keresési** csempére.
2. Hello lekérdezés sávon írja be a következő keresési lekérdezés hello

    ```
    Type=Alert SourceSystem=Nagios
    ```
   ![Naplófájl-keresési látható Nagios-riasztások](./media/log-analytics-linux-agents/oms-linux-nagios-alerts.png)

Miután meggyőződött arról, hogy a keresési eredmények hello, részletezhető további részleteket például *AlertState*.

### <a name="tooview-all-zabbix-alerts-with-log-analytics"></a>tooview Naplóelemzési összes Zabbix riasztások
1. A hello Operations Management Suite portálját, kattintson a hello **naplófájl-keresési** csempére.
2. Hello lekérdezés sávon írja be a következő keresési lekérdezés hello

    ```
    Type=Alert SourceSystem=Zabbix
    ```
   ![Naplófájl-keresési látható Zabbix-riasztások](./media/log-analytics-linux-agents/oms-linux-zabbix-alerts.png)

Miután meggyőződött arról, hogy a keresési eredmények hello, részletezhető további részleteket például *AlertName*.

## <a name="compatibility-with-system-center-operations-manager"></a>A System Center Operations Manager kompatibilitása
hello Linux OMS-ügynököt hello System Center Operations Manager ügynök ügynök bináris fájlokat osztanak meg. Hello OMS-ügynököt Linux telepítését felügyelete jelenleg az Operations Manager rendszerre frissítés hello OMI és az SCX-csomagokat a hello tooa újabb verziójú. hello OMS-ügynököt a Linux és a System Center 2012 R2 által támogatott. Azonban **System Center 2012 SP1 és a korábbi verziók jelenleg nem kompatibilis vagy nem támogatott a hello Linux OMS-ügynököt.**

> [!NOTE]
> Ha hello Linux OMS-ügynököt, amely jelenleg nem kezeli az Operations Manager telepített tooa számítógép, és később szeretné toomanage hello számítógépen az Operations Manager, módosítania kell hello OMI konfigurációs előtt hello számítógép felderítése. **Ez a lépés hello Operations Manager-ügynök van telepítve előtt hello OMS-ügynököt Linux esetén nincs szükség.**
>
>

### <a name="tooenable-hello-oms-agent-for-linux-toocommunicate-with-operations-manager"></a>a Linux toocommunicate az Operations Manager OMS-ügynököt tooenable hello
1. Hello fájl /etc/opt/omi/conf/omiserver.conf szerkesztése
2. Győződjön meg arról, hogy hello kezdődő **httpsport =** hello 1270-es port határozza meg. Többek között`httpsport=1270`
3. Indítsa újra a hello OMI-kiszolgálón:

    ```
    sudo /opt/omi/bin/service_control restart
    ```

## <a name="database-permissions-required-for-mysql-performance-counters"></a>A MySQL-teljesítményszámlálók szükséges adatbázis-engedélyek
toogrant engedélyek tooa MySQL figyelési felhasználó, hello támogatást nyújtó felhasználónak rendelkeznie kell hello "GRANT option" jogosultsággal, valamint az engedély hello jogosultság.

Ahhoz, hogy hello MySQL felhasználói tooreturn teljesítmény adatok hello felhasználói kell érik el a következő lekérdezések toohello:

```
SHOW GLOBAL STATUS;
SHOW GLOBAL VARIABLES:
```

Ezenkívül toothese lekérdezések hello MySQL felhasználói megköveteli a következő alapértelmezett táblák VÁLASSZA hozzáférés toohello:

* entitástulajdonos
* MySQL

Ezek a jogosultságok hello következő grant parancsok futtatásával kaphatja meg.

```
GRANT SELECT ON information_schema.* too‘monuser’@’localhost’;
GRANT SELECT ON mysql.* too‘monuser’@’localhost’;
```

## <a name="manage-mysql-monitoring-credentials-in-hello-authentication-file"></a>MySQL figyelési hello hitelesítési fájlt a hitelesítő adatok kezelése
hello alábbi szakaszokban MySQL hitelesítő adatok kezelése.

### <a name="configure-hello-mysql-omi-provider"></a>Hello MySQL OMI szolgáltató konfigurálása
hello MySQL OMI szolgáltató előre konfigurált MySQL felhasználónak, és MySQL klienskódtárak segítségével telepített tooquery hello teljesítményállapotot/rendelésinformációkat hello MySQL példányból.

### <a name="mysql-omi-authentication-file"></a>MySQL OMI hitelesítési fájl
MySQL OMI szolgáltató egy hitelesítési fájl toodetermine milyen bind-cím és port hello MySQL példány figyeli és a mi toouse toogather metrikák hitelesítő adatait használja. Telepítési hello MySQL OMI során szolgáltató keresni kezdi a MySQL my.cnf konfigurációs fájljait (alapértelmezett hely) bind-cím és port, valamint részlegesen set hello MySQL OMI hitelesítési fájlt.

előre generált MySQL OMI hitelesítési fájl toocomplete MySQL server-példány, figyelés felvétele hello megfelelő könyvtárba.

### <a name="authentication-file-format"></a>Hitelesítési fájlformátum
hello MySQL OMI hitelesítési fájl egy szövegfájlt, amely kapcsolatos információkat tartalmaz:

* Port
* A kötés-cím
* MySQL-felhasználónév
* A Base64 kódolású jelszó

hello MySQL OMI hitelesítési fájl csak jön létre az olvasási/írási toohello Linux felhasználói jogosultságokat biztosít.

```
[Port]=[Bind-Address], [username], [Base64 encoded Password]
(Port)=(Bind-Address), (username), (Base64 encoded Password)
(Port)=(Bind-Address), (username), (Base64 encoded Password)
AutoUpdate=[true|false]
```

Egy alapértelmezett MySQL OMI hitelesítési fájl tartalmaz egy alapértelmezett példány és egy portszámot, attól függően, hogy milyen információ elérhető és a MySQL-konfigurációs fájlt talált hello elemzett.

hello alapértelmezett példány egy egyszerűbb egy Linux gazdagép több MySQL példány kezelése azt jelenti, hogy toomake, és 0-s port hello példány helyén. Minden hozzáadott példány tulajdonsága nincs beállítva alapértelmezett példányból hello öröklik. Például "3308" porton figyel a MySQL-példány kerül hozzáadásra, ha hello alapértelmezett példány bind-cím, a felhasználónév és a Base64-kódolású jelszó lesz használt tootry kell és figyeli a 3308 hello példány figyelése. Ha 3308 hello példánya kötve tooanother cím és használ hello azonos MySQL felhasználónév és jelszó pár csak hello respecification hello, bind-cím van szükség, és hello egyéb tulajdonságok öröklik.

Példák hello hitelesítési fájl hello alábbihoz.

Alapértelmezett példány és -port 3308 példány:

```
0=127.0.0.1, myuser, cnBwdA==3308=, ,AutoUpdate=true
```

Alapértelmezett példány és -példány portja 3308 + különböző Base 64 kódolású jelszó:

```
0=127.0.0.1, myuser, cnBwdA==3308=127.0.1.1, , AutoUpdate=true
```


| **Tulajdonság** | **Leírás** |
| --- | --- |
| Port |Port hello aktuális port hello példánya által figyelt MySQL jelöli.  hello port 0 azt jelenti, hogy hello tulajdonságait a következő alapértelmezett példányát használja. |
| A kötés-cím |hello címet kötni az hello aktuális MySQL bind-cím |
| felhasználónév |A hello felhasználó felhasználóneve, hello MySQL toouse toomonitor hello MySQL server-példányt kívánja. |
| A Base64 kódolású jelszó |Ez a hello MySQL figyelési felhasználó Base64 kódolású hello jelszavát. |
| Automatikus frissítés |Hello MySQL OMI szolgáltató frissítésekor hello szolgáltató megkeresheti a módosításokat a hello my.cnf fájlban, és felülírja hello MySQL OMI hitelesítési fájlt. Állítsa be a jelző tootrue és hamis értéket, attól függően, hogy a szükséges frissítések toohello MySQL OMI hitelesítési fájlt. |

#### <a name="authentication-file-location"></a>Hitelesítési fájl helye
legyen hello a következő helyen található, és "mysql-alapú hitelesítés" nevű hello MySQL OMI hitelesítési fájlt:

/var/OPT/Microsoft/MySQL-cimprov/auth/omsagent/MySQL-auth

hello fájl (és a hitelesítési/omsagent könyvtár) hello omsagent felhasználói kell tartoznia.

## <a name="agent-logs"></a>Ügynök naplók
hello naplókat hello Linux OMS-ügynököt a jelenleg:

/ var/opt/microsoft/omsagent/&lt;munkaterület azonosítója&gt;/log/

hello hello naplókat omsconfig (ügynök konfigurációjának) program Linux OMS-ügynök van:

/ var/opt/microsoft/omsconfig/log /

Hello OMI és az SCX-összetevők naplóinak (amely teljesítményadatokat metrikák) jelenleg:

/ var/opt/omi/log/és /var/opt/microsoft/scx/log

## <a name="troubleshooting-hello-oms-agent-for-linux"></a>Hibaelhárítási hello OMS-ügynököt a Linux
A következő információk toodiagnose hello használja, és a gyakori problémák elhárításához.

Hibaelhárítási információkat ebben a szakaszban hello egyike sem segít, ha használhatja hello a következő erőforrások toohelp hárítsa el a problémát.

* Az ügyfelek a Premier szintű támogatási be tud jelentkezni egy támogatási esetet keresztül [Premier](https://premier.microsoft.com/)
* Az Azure támogatási szerződésből rendelkező ügyfelek is bejelentkezhetnek támogatási eseteket hello [Azure-portálon](https://manage.windowsazure.com/?getsupport=true)
* Fájl egy [GitHub-probléma](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)
* Visszajelzési fórumon ötleteket és a hibajelentés toocreate [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)

### <a name="important-log-locations"></a>Fontos naplók helye
| Fájl | Elérési út |
| --- | --- |
| Linux-naplófájl OMS-ügynököt |`/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log ` |
| OMS-ügynök konfigurációs naplófájl |`/var/opt/microsoft/omsconfig/omsconfig.log` |

### <a name="important-configuration-files"></a>Fontos konfigurációs fájlok
| Catergory | Fájl helye |
| --- | --- |
| Rendszernapló |`/etc/syslog-ng/syslog-ng.conf`vagy `/etc/rsyslog.conf` vagy`/etc/rsyslog.d/95-omsagent.conf` |
| Teljesítmény, Nagios, Zabbix, OMS kimeneti és általános ügynök |`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` |
| További konfigurációk |`/etc/opt/microsoft/omsagent/<workspace id>/omsagent.d/*.conf` |

> [!NOTE]
> Ha az OMS-portálon konfigurációs engedélyezve van, a rendszer felülírja az teljesítményszámlálók és syslog szerkesztési konfigurációs fájlokat. Letilthatja az OMS-portálon hello (az összes csomópont) konfigurációs vagy az egyetlen csomópont hello következő futtatásával:
>
>

```
sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```


### <a name="enable-debug-logging"></a>A hibakeresési naplózást engedélyező
tooenable hibakeresési naplózást, használhat hello OMS kimeneti beépülő modul és részletes kimenet.

#### <a name="oms-output-plugin"></a>OMS kimeneti beépülő modul
FluentD lehetővé teszi, hogy hello beépülő modul toospecify naplózási szintjeinek bemenetekhez és kimenetekhez a különböző naplózási szintet. a különböző naplózási szint OMS kimeneti toospecify hello általános ügynök konfigurációjának hello szerkesztése `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` fájlt.

Kis hatótávolságú hello alsó hello konfigurációs fájl, módosítsa a hello `log_level` tulajdonságot `info` túl`debug`.

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

A hibakeresési naplózás lehetővé teszi a kötegelni toosee feltöltések toohello típusát, számát adatelemek és az idő toosend elválasztott OMS szolgáltatáshoz.

*Példa engedélyezett hibakeresési napló:*

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

#### <a name="verbose-output"></a>Részletes kimenet
Hello OMS kimeneti beépülő modul helyett is a kimenetre küldheti adatelemek közvetlenül túl`stdout`, ez az OMS-ügynököt hello Linux naplófájl látható.

Hello OMS általános ügynök konfigurációs fájlban a következő `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, Megjegyzés kibővített hello OMS kimeneti beépülő modul hozzáadásával egy `#` minden sor elé.

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

Az alábbiakban hello kimeneti beépülő modul, hello Megjegyzés eltávolítása a következő szakasz hello eltávolításával hello `#` szimbólum minden sor hello elején.

```
<match **>
  type stdout
</match>
```

### <a name="forwarded-syslog-messages-do-not-appear-in-hello-log"></a>Továbbított Syslog-üzeneteket nem jelennek meg hello napló
#### <a name="probable-causes"></a>Lehetséges okok
* hello alkalmazott konfiguráció toohello Linux kiszolgálón nem küldött hello létesítmények gyűjtemény engedélyezése és/vagy szintek naplózása
* Syslog nem nem lesznek továbbítva megfelelően toohello Linux kiszolgáló
* üzenetek száma másodpercenként továbbított hello száma túl nagyok hello OMS-ügynököt a Linux toohandle hello-alapkonfiguráció

#### <a name="resolutions"></a>Megoldások
* Győződjön meg arról, hogy az OMS-portálon hello hello a konfigurálás a Syslog van minden hello létesítményekben és hello megfelelő naplózási szintek
  * **OMS-portálon > Beállítások > adatok > Syslog**
* Győződjön meg arról, hogy natív syslog üzenetküldési démonok (`rsyslog`, `syslog-ng`) is képes tooreceive továbbított köszönőüzenetei
* Ellenőrizze a tűzfal beállításait hello Syslog server tooensure, hogy üzenetek nem blokkolja a
* A Syslog-üzenet hello segítségével tooOMS szimulálása `logger` paranccsal – például:
  * `logger -p local0.err "This is my test message"`

### <a name="problems-connecting-toooms-when-using-a-proxy"></a>Problémái tooOMS proxy használata esetén
#### <a name="probable-causes"></a>Lehetséges okok
* hello proxy határozni, ha telepítése és konfigurálása hello ügynök helytelen
* hello OMS végpontok nincsenek az adatközpontban található whitelistested

#### <a name="resolutions"></a>Megoldások
* Telepítse újra a hello OMS-ügynököt a következő parancsot hello kapcsolóval hello használata Linux `-v` engedélyezve van. Ez lehetővé teszi a részletes kimenet hello ügynök hello proxy toohello OMS szolgáltatás keresztül kapcsolódik.
  * `/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`
  * Tekintse meg az OMS-proxy: hello dokumentációt [hello ügynökök konfigurálása HTTP-proxykiszolgáló való használatra](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)
* Ellenőrizze, hogy a következő OMS Szolgáltatásvégpontok hello szerepel az engedélyezési listán

| Ügynök erőforrása | Portok |
| --- | --- |
| &#42;. ods.opinsights.Azure.com |443-as port |
| &#42;. OMS.opinsights.Azure.com |443-as port |
| ods.systemcenteradvisor.com |443-as port |
| &#42;.blob.core.windows.net/ |443-as port |

### <a name="a-403-error-is-displayed-when-onboarding"></a>A 403-as hibaüzenet jelenik meg ha bevezetése
#### <a name="probable-causes"></a>Lehetséges okok
* hello dátum és idő nem megfelelő Linux-kiszolgálón
* hello munkaterületének Azonosítóját és kulcsát használt helytelenek

#### <a name="resolution"></a>Megoldás:
* Ellenőrizze a Linux-kiszolgálóra a hello hello idő `date` parancsot. Ha adatok érték nagyobb, mint hello vagy kevesebb mint 15 percet a hello a jelenlegi időpontnál, akkor bevezetési sikertelen lesz. toocorrect, hello dátumot és/vagy a Linux-kiszolgálóra az időzóna frissítése.
* hello Linux OMS-ügynök legújabb verziójának hello értesítést küld, ha időeltérése bevezetési hibája okozza.
* Újra-beépített használatával hello megfelelő munkaterület Azonosítóját és kulcsát. Lásd: [hello parancssorból bevezetési](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) további információt.

### <a name="a-500-error-or-404-error-appears-in-hello-log-file-after-onboarding"></a>500 hiba vagy 404-es hiba után jelenik meg hello naplófájlban bevezetése
Ez az egy ismert hiba, amely a hello első az adatok feltöltése a Linux az OMS-munkaterület során fordul elő. Ez nem befolyásolja, küldött adatok mennyisége vagy az egyéb problémákat. Kezdeti hello hibák figyelmen kívül hagyhatja bevezetése.

### <a name="nagios-data-does-not-appear-in-hello-oms-portal"></a>Nagios-adatok nem jelennek meg hello OMS-portálon
#### <a name="probable-causes"></a>Lehetséges okok
* hello omsagent felhasználó nem rendelkezik engedélyekkel tooread hello Nagios naplófájlból
* továbbra is megjegyzésként hello omsagent.conf fájlban hello Nagios forrás és a szűrő szakasz

#### <a name="resolutions"></a>Megoldások
* Adja hozzá a hello omsagent felhasználói rendelés tooread hello Nagios fájlból. Lásd: [Nagios riasztások](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) további információt.
* A Linux általános konfigurációs fájlban a következő OMS-ügynököt hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, ügyeljen arra, hogy **mindkét** Nagios forrás- és a szűrő szakaszok észrevételeit eltávolítva, a következő példa hasonló toohello hello.

```
<source>
  type tail
  path /var/log/nagios/nagios.log
  format none
  tag oms.nagios
</source>

<filter oms.nagios>
  type filter_nagios_log
</filter>
```


### <a name="linux-data-doesnt-appear-in-hello-oms-portal"></a>Linux-adatok nem jelennek meg hello OMS-portálon
#### <a name="probable-causes"></a>Lehetséges okok
* Bevezetési toohello OMS-szolgáltatás nem tudta
* Kapcsolat toohello OMS szolgáltatás le van tiltva.
* hello OMS-ügynököt a Linux-adatok biztonsági másolatban

#### <a name="resolutions"></a>Megoldások
* Ellenőrizze, hogy bevezetési toohello OMS szolgáltatás sikeres volt ellenőrzi, hogy hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` létezik-e.
* Újra-beépített használatával hello omsadmin.sh parancssor. Lásd: [hello parancssorból bevezetési](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) további információt.
* Ha proxyt használ, akkor hello proxy hibaelhárítási fent leírt lépésekkel
* Bizonyos esetekben hello Linux OMS-ügynököt hello OMS szolgáltatás nem tud kommunikálni hello ügynök adatok esetén a biztonsági mentésben toohello teljes puffer mérete 50 MB. Indítsa újra az OMS-ügynököt hello Linux hello futtatásával `/opt/microsoft/omsagent/bin/service_control restart` parancsot.
  >[AZURE.NOTE] Ez a probléma fennáll ügynök verziója 1.1.0-28 és újabb verziók.

### <a name="syslog-linux-performance-counter-configuration-is-not-applied-in-hello-oms-portal"></a>Syslog Linux teljesítmény számláló konfiguráció nem alkalmazható az hello OMS-portálon
#### <a name="probable-causes"></a>Lehetséges okok
* hello konfigurációs ügynök hello Linux OMS-ügynököt nem rendelkezik hello legfrissebb konfigurációt lekért hello OMS-portálon.
* hello hello portálon módosított beállítások a rendszer nem alkalmazta

#### <a name="resolutions"></a>Megoldások
`omsconfig`hello konfigurációs ügynök hello OMS-ügynököt a szolgál, amely lekéri az OMS-portálon konfigurációs módosítások 5 percenként Linux. Ez a konfiguráció nem található alkalmazott toohello OMS-ügynököt a Linux konfigurációs fájlok `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.

* Bizonyos esetekben hello OMS-ügynököt a Linux-ügynök konfigurációs nem feltétlenül tudja toocommunicate hello beállítása szolgáltatással sem alkalmazza a legújabb konfigurációt eredményez.
* Győződjön meg arról, hogy hello `omsconfig` -ügynök telepítve van a hello alábbira:

  * `dpkg --list omsconfig` vagy `rpm -qi omsconfig`
  * Ha nincs telepítve, telepítse újra a legújabb verziójának hello hello OMS-ügynököt Linux
* Győződjön meg arról, hogy hello `omsconfig` ügynök kommunikálhatnak hello OMS szolgáltatáshoz

  * Futtassa a hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` parancs
    * hello fenti adja vissza hello konfigurálása az ügynök lekéri a hello portal, beleértve a Syslog-beállítások, Linux teljesítményszámlálók és egyéni naplókat
    * Ha a fenti hello parancs sikertelen, futtassa a hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` parancsot. Ez a parancs kényszeríti hello omsconfig ügynök toocommunicate hello OMS szolgáltatás tooretrieve hello legújabb konfigurációval.

### <a name="custom-linux-log-data-does-not-appear-in-hello-oms-portal"></a>Egyéni Linux naplóadatokat nem jelenik meg hello OMS-portálon
#### <a name="probable-causes"></a>Lehetséges okok
* Bevezetési tooOMS szolgáltatás nem tudta
* Hello **konfigurációs toomy Linux-kiszolgálókon a következő Apply hello** beállítás nincs kiválasztva
* omsconfig nem kiválasztotta hello legújabb egyéni napló hello portálról
* Hello `omsagent` használata nem tooaccess hello egyéni napló tooa engedélyek probléma miatt vagy `omsagent` nem található. Ebben az esetben látni fogja a következő kimeneti hello:
  * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
  * `[DATETIME] [error]: file not accessible by omsagent.`
* Ez az hello hello OMS-ügynököt a Linux-verzió 1.1.0-217 javított versenyhelyzet ismert problémái

#### <a name="resolutions"></a>Megoldások
* Győződjön meg arról, hogy sikeresen előkészítve, hogy hello meghatározásával `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` fájl létezik-e.
  * Ha szükséges, előkészítésére újra a hello omsadmin.sh parancssor használatával. Lásd: [hello parancssorból bevezetési](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) további információt.
* A hello OMS-portálon, a **beállítások** a hello **adatok** lapra, győződjön meg arról, hogy hello **konfigurációs toomy Linux-kiszolgálókon a következő Apply hello** beállítás  
  ![konfigurációjának alkalmazása](./media/log-analytics-linux-agents/customloglinuxenabled.png)
* Győződjön meg arról, hogy hello `omsconfig` ügynök kommunikálhatnak hello OMS szolgáltatáshoz

  * Futtassa a hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` parancs
  * hello fenti adja vissza hello konfigurálása az ügynök lekéri az hello Portal, beleértve a Syslog-beállítások, Linux teljesítményszámlálók és egyéni naplókat
  * Ha a fenti hello parancs sikertelen, futtassa a hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` parancsot. Ez a parancs kényszeríti hello omsconfig ügynök toocommunicate OMS szolgáltatással, és hello legfrissebb konfigurációt beolvasása.

Linux felhasználói jogosultsággal rendelkező felhasználóként fut az OMS-ügynököt hello helyett `root`, hello OMS-ügynököt, a Linux fut a hello `omsagent` felhasználó. A legtöbb esetben kifejezett engedélyekké kell megadott sorrendben tooread toohello felhasználó bizonyos fájlokat.

toogrant engedély túl`omsagent` felhasználói, futtassa a következő parancsok hello:

1. Adja hozzá a hello `omsagent` felhasználói tooa adott csoportban található`sudo usermod -a -G <GROUPNAME> <USERNAME>`
2. Általános olvasási hozzáférést toohello fájl szükséges a engedélyezése`sudo chmod -R ugo+rw <FILE DIRECTORY>`

Egy ismert probléma az hello hello OMS-ügynököt a Linux-verzió 1.1.0-217 javított versenyhelyzet van. Toohello legújabb ügynök frissítése, után futtassa a következő parancs tooget hello legújabb verziójának hello kimeneti beépülő modul hello:

```
sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf
```

## <a name="known-limitations"></a>Ismert korlátozásai
Tekintse át a következő szakaszok toolearn kapcsolatos Linux hello OMS-ügynököt a jelenlegi korlátozások hello.

### <a name="azure-diagnostics"></a>Azure Diagnostics
Az Azure-ban futó Linux virtuális gépek emellett további lépésekre lehet szükség tooallow adatgyűjtés Azure Diagnostics és az Operations Management Suite. **2.2-es verziója** hello Linux diagnosztika kiterjesztése az hello OMS-ügynököt a Linux-való kompatibilitás szükséges.

Telepítésének és konfigurálásának hello diagnosztikai bővítmény Linux további információkért lásd: [hello Azure CLI parancs tooenable Linux diagnosztikai bővítmény használatára](../virtual-machines/linux/classic/diagnostic-extension-v2.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).

**2.0 too2.2 Azure CLI ASM hello Linux diagnosztika bővítmény frissítése:**

```
azure vm extension set -u <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.0
azure vm extension set <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

**ARM**

```
azure vm extension set -u <resource-group> <vm-name> Microsoft.Insights.VMDiagnosticsSettings Microsoft.OSTCExtensions 2.0
azure vm extension set <resource-group> <vm-name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

A parancs példákat hivatkozhatnak PrivateConfig.json nevű fájlt. hello formátuma, hogy a fájl a következő minta hello kell hasonlítania.

```
    {
    "storageAccountName":"hello storage account tooreceive data",
    "storageAccountKey":"hello key of hello account"
    }
```

### <a name="sysklog-is-not-supported"></a>Sysklog nem támogatott.
Rsyslog vagy a syslog-ng van szükség toocollect syslog-üzeneteket. syslog-események gyűjtése nem támogatott hello alapértelmezett syslog démon a Red Hat Enterprise Linux-, CentOS, és Oracle Linux verziója (sysklog) 5-ös verzióját. ezek azokat a terjesztéseket, a jelen verziójában a syslog-adatot toocollect hello rsyslog démon kell telepíteni, és tooreplace sysklog konfigurálva. A sysklog lecserélését rsyslog további információkért lásd: [telepítése RPM újonnan létrehozott hello rsyslog](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).

## <a name="next-steps"></a>Következő lépések
* [Log Analytics-megoldások hozzáadása hello megoldások gyűjtemény](log-analytics-add-solutions.md) tooadd funkciókat és összefog adatokat.
* Ismerkedjen meg [keresések jelentkezzen](log-analytics-log-searches.md) tooview részletes megoldások által összegyűjtött adatokat.
* Használjon [irányítópultok](log-analytics-dashboards.md) toosave és megjelenítheti a saját egyéni keres.
