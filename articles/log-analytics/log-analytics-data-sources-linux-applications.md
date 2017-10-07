---
title: "Linux-alkalmazás teljesítményének az OMS szolgáltatáshoz aaaCollect |} Microsoft Docs"
description: "Ez a cikk a Linux toocollect teljesítményszámlálókkal OMS-ügynököt hello beállítása a MySQL és Apache HTTP Server részletes adatokat biztosít."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 51105c6add5c7941a004570a76a4d94c02fc8a71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-performance-counters-for-linux-applications-in-log-analytics"></a>Linux Log Analytics-alkalmazások a teljesítményszámlálók adatainak összegyűjtése 
Ez a cikk részletesen hello konfigurálása [Linux OMS-ügynököt](https://github.com/Microsoft/OMS-Agent-for-Linux) toocollect teljesítményszámlálók adott alkalmazásokhoz.  Ebben a cikkben szereplő hello alkalmazások a következők:  

- [MySQL](#MySQL)
- [Apache HTTP Server](#apache-http-server)

## <a name="mysql"></a>MySQL
Ha MySQL vagy MariaDB Server hello számítógépen van telepítve amikor hello OMS-ügynök telepítve van, egy Teljesítményfigyelő szolgáltató MySQL-kiszolgáló automatikusan települ. Ez a szolgáltató toohello helyi MySQL/MariaDB kiszolgáló tooexpose teljesítményének statisztikájára csatlakozik. MySQL felhasználói hitelesítő adatok érdekében, hogy a hello szolgáltató hello MySQL kiszolgálón kell konfigurálni.

### <a name="configure-mysql-credentials"></a>MySQL hitelesítő adatok beállítása
hello MySQL OMI szolgáltató előre konfigurált MySQL felhasználónak, és MySQL klienskódtárak segítségével telepített rendelés tooquery hello teljesítmény- és állapotadatokat hello MySQL példány.  Ezeket a hitelesítő adatokat hello Linux-ügynök tárolt hitelesítési fájlban vannak tárolva.  hello hitelesítési fájl határozza meg, milyen bind-cím és port hello MySQL példány figyel a következőn:, és milyen hitelesítő adatok toouse toogather metrikákat.  

Hello OMS-ügynököt a Linux hello MySQL OMI telepítése során szolgáltató keresni kezdi a MySQL my.cnf konfigurációs fájljait (alapértelmezett hely) bind-cím és port, valamint részlegesen set hello MySQL OMI hitelesítési fájlt.

hello MySQL hitelesítési fájl tárolja `/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth`.


### <a name="authentication-file-format"></a>Hitelesítési fájlformátum
Az alábbiakban látható hello hello MySQL OMI hitelesítési fájl formátuma

    [Port]=[Bind-Address], [username], [Base64 encoded Password]
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    AutoUpdate=[true|false]

hello bejegyzés hello hitelesítési fájl hello a következő táblázat ismerteti.

| Tulajdonság | Leírás |
|:--|:--|
| Port | Hello aktuális port hello példánya által figyelt MySQL jelöli. 0-s port határozza meg, hogy hello tulajdonságait a következő alapértelmezett példányát használja. |
| A kötés-cím| Aktuális MySQL bind-cím. |
| felhasználónév| MySQL felhasználói toouse toomonitor hello MySQL server-példány használja. |
| A Base64 kódolású jelszó| Jelszó hello MySQL figyelési felhasználó Base64 kódolású. |
| Automatikus frissítés| Határozza meg, hogy a toorescan hello my.cnf fájl módosításait, és felülírja a hello MySQL OMI hitelesítési fájl frissítésekor hello MySQL OMI szolgáltató. |

### <a name="default-instance"></a>Alapértelmezett példány
hello MySQL OMI hitelesítési fájl definiálhat egy alapértelmezett példány és a port száma toomake több MySQL példányt egy Linux gazdagép könnyebb kezelése.  hello alapértelmezett példány helyén 0-s port példány. Minden további példányokat öröklik tulajdonsága nincs beállítva hello alapértelmezett példányból, kivéve, ha ezek különböző értékeket megadni. Például "3308" porton figyel a MySQL-példány kerül hozzáadásra, ha hello alapértelmezett példány bind-cím, a felhasználónév és a Base64-kódolású jelszó lesz használt tootry kell és figyeli a 3308 hello példány figyelése. Ha 3308 hello példánya kötött tooanother cím és MySQL felhasználónévvel hello jelszó pár csak hello bind-cím van szükség, és hello egyéb tulajdonságok öröklik a.

a következő táblázat hello példa példány beállításokkal rendelkezik. 

| Leírás | Fájl |
|:--|:--|
| Alapértelmezett példány és -port 3308 példány. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=, ,`<br>`AutoUpdate=true` |
| Alapértelmezett példány és -példány port 3308 és más felhasználónevet és jelszót. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=127.0.1.1, myuser2,cGluaGVhZA==`<br>`AutoUpdate=true` |


### <a name="mysql-omi-authentication-file-program"></a>MySQL OMI hitelesítési fájl Program
Hello MySQL OMI hello telepítésének részét képező szolgáltató egy MySQL OMI hitelesítési fájl programot, amely lehet használt tooedit hello MySQL OMI hitelesítési fájlt. hello hitelesítési fájl program hello helye a következő helyen találhatók meg.

    /opt/microsoft/mysql-cimprov/bin/mycimprovauth

> [!NOTE]
> hello hitelesítőadat-fájlt hello omsagent fiókkal olvashatónak kell lennie. Hello mycimprovauth parancs futtatásához használt omsgent ajánlott.

hello következő tábla részletes adatokat biztosít a hello szintaxis mycimprovauth használatával.

| Művelet | Példa | Leírás
|:--|:--|:--|
| automatikus frissítés * false\|Igaz * | mycimprovauth autoupdate hamis | Készlet-e hello hitelesítési fájl automatikusan frissíti a indítsa újra, vagy frissíteni. |
| alapértelmezett *bind-cím felhasználónév-jelszó* | mycimprovauth alapértelmezett 127.0.0.1 legfelső szintű pwd | Készletek hello hello MySQL OMI hitelesítési fájl az alapértelmezett példány.<br>hello jelszó mező egyszerű szövegként kell megadni – – hello MySQL OMI hitelesítési fájl hello jelszót Base 64 kódolású. |
| törlés * default\|port_num * | mycimprovauth 3308 | Törli a hello megadott példányt, vagy alapértelmezett, vagy portszámot. |
| segítség | mycimprov Súgó | Parancsok toouse felsorolása. |
| Nyomtatás | nyomtatási mycimprov | Egy egyszerű tooread MySQL OMI hitelesítési fájl kinyomtatása. |
| Frissítse a port_num *bind-cím felhasználónév-jelszó* | mycimprov frissítés 3307 127.0.0.1 legfelső szintű pwd | Frissíti a megadott példány hello, vagy hozzáadja hello példány, ha nem létezik. |

hello következő Példaparancsok egy alapértelmezett felhasználói fiók megadása hello MySQL-kiszolgáló – localhost.  hello jelszó mező egyszerű szövegként kell megadni – hello MySQL OMI hitelesítési fájl hello jelszó lesz Base 64 kódolású

    sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'
    sudo /opt/omi/bin/service_control restart

### <a name="database-permissions-required-for-mysql-performance-counters"></a>A MySQL-teljesítményszámlálók szükséges adatbázis-engedélyek
hello MySQL felhasználói lekérdezések toocollect MySQL Server teljesítményadatokat a következő hozzáférési toohello igényel. 

    SHOW GLOBAL STATUS;
    SHOW GLOBAL VARIABLES:


hello MySQL felhasználói is szükséges a következő alapértelmezett táblák VÁLASSZA hozzáférés toohello.

- entitástulajdonos
- a MySQL. 

Ezek a jogosultságok hello következő grant parancsok futtatásával kaphatja meg.

    GRANT SELECT ON information_schema.* too‘monuser’@’localhost’;
    GRANT SELECT ON mysql.* too‘monuser’@’localhost’;


> [!NOTE]
> toogrant engedélyek tooa felhasználói felhasználói hello figyelési MySQL rendelkeznie kell hello "GRANT option" jogosultsággal, valamint az engedély hello jogosultság.

### <a name="define-performance-counters"></a>Adja meg a teljesítményszámlálók

Ha megfelelően konfigurált hello OMS-ügynököt a Linux toosend adatok tooLog elemzés, konfigurálnia kell a hello teljesítmény számlálók toocollect.  A hello eljárással [Naplóelemzési a Windows és Linux teljesítmény adatforrások](log-analytics-data-sources-windows-events.md) hello számlálók a következő táblázat hello.

| Objektum neve | Számláló neve |
|:--|:--|
| MySQL-adatbázis | Szabad lemezterület (bájt) |
| MySQL-adatbázis | Táblák |
| MySQL-kiszolgáló | Megszakadt a kapcsolat Pct |
| MySQL-kiszolgáló | Kapcsolat használata Pct |
| MySQL-kiszolgáló | Lemezterület-használat (bájt) |
| MySQL-kiszolgáló | A tábla teljes vizsgálat Pct |
| MySQL-kiszolgáló | InnoDB pufferkészlet találati Pct |
| MySQL-kiszolgáló | InnoDB puffer készlet használata százalékos |
| MySQL-kiszolgáló | InnoDB puffer készlet használata százalékos |
| MySQL-kiszolgáló | A Pct kulcs gyorsítótár-találat |
| MySQL-kiszolgáló | Kulcs gyorsítótárában használata Pct |
| MySQL-kiszolgáló | Kulcs gyorsítótárában írási Pct |
| MySQL-kiszolgáló | Lekérdezés-gyorsítótárának találati Pct |
| MySQL-kiszolgáló | Lekérdezés gyorsítótár szilva Pct |
| MySQL-kiszolgáló | Lekérdezés gyorsítótár használata Pct |
| MySQL-kiszolgáló | A Pct tábla gyorsítótár-találat |
| MySQL-kiszolgáló | Tábla gyorsítótár használata Pct |
| MySQL-kiszolgáló | Tábla zárolási versenyt Pct |

## <a name="apache-http-server"></a>Apache HTTP Server 
Apache HTTP Server észlel hello számítógépen hello omsagent csomag telepítésekor, ha egy teljesítményfigyelés-szolgáltató az Apache HTTP Server automatikusan telepíti. Ez a szolgáltató egy Apache modul, amely töltse be az Apache HTTP Server hello rendelés tooaccess teljesítményadatok támaszkodik. a parancs a következő hello hello modul tölthetők be:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

toounload hello Apache figyelőmodult, futtassa a következő parancs hello:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```

### <a name="define-performance-counters"></a>Adja meg a teljesítményszámlálók

Ha megfelelően konfigurált hello OMS-ügynököt a Linux toosend adatok tooLog elemzés, konfigurálnia kell a hello teljesítmény számlálók toocollect.  A hello eljárással [Naplóelemzési a Windows és Linux teljesítmény adatforrások](log-analytics-data-sources-windows-events.md) hello számlálók a következő táblázat hello.

| Objektum neve | Számláló neve |
|:--|:--|
| Apache HTTP Server | Foglalt munkavállalók |
| Apache HTTP Server | Üresjárati munkavállalók |
| Apache HTTP Server | A PCT foglalt munkavállalók |
| Apache HTTP Server | Teljes Pct Processzor |
| Apache virtuális állomás | Hibák száma percenként - ügyfél |
| Apache virtuális állomás | Hibák száma percenként - kiszolgáló |
| Apache virtuális állomás | Kérelmenként KB |
| Apache virtuális állomás | KB kérelmek / másodperc |
| Apache virtuális állomás | Kérelmek / másodperc |



## <a name="next-steps"></a>Következő lépések
* [A teljesítményszámlálók adatainak összegyűjtése](log-analytics-data-sources-performance-counters.md) a Linux-ügynököt.
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat. 
