---
title: "Példa – aaaDMZ egy Szegélyhálózaton tooprotect olyan alkalmazásokat hozhat létre egy tűzfal és az NSG-k |} Microsoft Docs"
description: "A tűzfal és a hálózati biztonsági csoportokkal (NSG) DMZ összeállítása"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: c78491c7-54ac-4469-851c-b35bfed0f528
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: 18f116dc3897567bff14a509ae8c13f449182bfb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="example-2--build-a-dmz-tooprotect-applications-with-a-firewall-and-nsgs"></a>2 – példa egy Szegélyhálózaton tooprotect olyan alkalmazásokat hozhat létre egy tűzfal és az NSG-k
[Visszatérési toohello biztonsági határ bevált gyakorlatok lap][HOME]

Ebben a példában a DMZ hozzon létre egy tűzfal, négy windows Server-kiszolgálók és hálózati biztonsági csoportok. Azt is haladhat végig hello vonatkozó parancsok tooprovide bemutatják az egyes lépéseket. Szerepel továbbá egy forgalom forgatókönyv szakasz tooprovide egy részletesebb lépésenkénti hogyan forgalom halad keresztül hello szolgálhat a hello DMZ. Végezetül hello references szakaszában van hello teljes kód látható és utasítás toobuild a környezet tootest és a különböző forgatókönyvekben a kísérletet. 

![NVA és NSG bejövő DMZ][1]

## <a name="environment-description"></a>Környezet leírása
Ebben a példában nincs olyan előfizetést, amelyet hello következő tartalmazza:

* A felhőalapú szolgáltatások két: "FrontEnd001" és "BackEnd001"
* A virtuális hálózati "CorpNetwork", két alhálózattal rendelkező: "Előtér" és "Háttér"
* Egy egyetlen hálózati biztonsági csoport, amely alkalmazott tooboth alhálózatok
* Ebben a példában a Barracuda NextGen tűzfal, a hálózati virtuális készülék toohello Frontend alhálózathoz csatlakoztatva
* A Windows Server egy webalkalmazás-kiszolgáló ("IIS01") jelölő
* Két windows Server kiszolgálókon, amelyek megfelelnek az alkalmazás biztonsági end-kiszolgálók ("AppVM01", "AppVM02")
* A Windows server DNS-kiszolgáló ("DNS01") jelölő

> [!NOTE]
> Bár ebben a példában a Barracuda NextGen tűzfal, ebben a példában a különböző hálózati virtuális készülékek használhatnak hello számos használ.
> 
> 

Hello references szakaszában alatt van egy PowerShell-parancsfájlt, amely a fent leírt hello környezetben a legtöbb fog létrehozni. Épület hello virtuális gépek és virtuális hálózatok, bár hello a példaként megadott parancsfájlt, által végzett dokumentum nem ismerteti a jelen dokumentum részletesen.

toobuild hello rendelkező környezetben:

1. Hello hálózati konfigurációs XML-fájl (nevét, helyét és IP címeket toomatch megadott hello forgatókönyv frissíti) hello references szakaszában szereplő mentése
2. Frissítés hello felhasználói változók hello parancsfájl toomatch hello környezet hello parancsfájlban toobe futtatni (előfizetések, service nevét stb.)
3. PowerShell hello parancsfájlok végrehajtását

**Megjegyzés:**: hello régió hello PowerShell-parancsfájl esetében meg kell egyeznie a hello hálózati konfigurációs XML-fájlban szereplő hello régióban.

Miután hello parancsprogram sikeresen lefut hello utáni parancsfájl lépések tehetők:

1. Állítson be hello tűzfalszabályokat, ezzel kapcsolatban lásd: hello című az alábbi: tűzfalszabályokat.
2. Opcionálisan hello references szakaszában vannak két parancsfájlok tooset hello webkiszolgáló és egy egyszerű webes alkalmazás tooallow e DMZ konfiguráció tesztelése az alkalmazások kiszolgálói.

hello következő szakasz ismerteti a legtöbb hello parancsfájlok utasítások relatív tooNetwork biztonsági csoportokat.

## <a name="network-security-groups-nsg"></a>Hálózati biztonsági csoportok (NSG)
Az ebben a példában egy NSG-csoport összeállítása és hat szabályokkal majd betölteni. 

> [!TIP]
> Általánosságban véve kell először hozza létre az adott "Engedélyezés" szabályokat, és majd hello utolsó általánosabbá "Deny" szabályokat. hello prioritású határozzák meg, hogy mely szabályokat értékeli ki a rendszer először. Forgalom található tooapply tooa meghatározott szabály, ha nincsenek további szabályok kiértékelése. NSG-szabályok alkalmazhat vagy hello bejövő vagy kimenő irányban (szempontjából hello hello alhálózati).
> 
> 

A következő szabályok hello deklaratív módon, a bejövő forgalom alatt beépített:

1. Belső DNS-forgalom (53-as port) engedélyezve van
2. RDP-forgalom (3389-es port) a hello Internet tooany VM engedélyezve van
3. Engedélyezett HTTP-forgalom (80-as port) a hello Internet toohello NVA (tűzfal)
4. Engedélyezett IIS01 tooAppVM1 minden forgalmat (minden porthoz)
5. Teljes virtuális hálózatot (alhálózatok mindkét) megtagadva hello Internet toohello minden forgalmat (minden porthoz)
6. Minden forgalmat (minden porthoz) hello Frontend alhálózathoz toohello Backend alhálózathoz megtagadva

E szabályok kötött tooeach alhálózattal, ha a HTTP-kérelmek bejövő webkiszolgálóról hello Internet toohello, mindkét szabályok 3 (engedélyezése) és 5 (megtagadni) szeretné alkalmazni, de mivel a szabály 3 egy magasabb prioritással bír, csak azt csak akkor vonatkozik, és 5 szabály nem lesz play kerülhet. Így toohello tűzfal hello HTTP-kérelem szeretné tenni. Ha ugyanaz a forgalom próbált tooreach hello DNS01 server, a szabály 5 (Megtagadás) lenne hello első tooapply és hello forgalom volna nem engedélyezett toopass toohello kiszolgáló. 6 (Megtagadás) blokkok hello Frontend alhálózathoz van szó toohello Backend alhálózathoz (kivéve a engedélyezett forgalom szabályokban 1 és 4) a szabályt, ez védelmet nyújt hello háttér hálózathoz, abban az esetben, ha egy támadó biztonság sérüléseinek hello webalkalmazás hello előtér, hello támadónak korlátozott hozzáférés toohello háttérbeli "védett" hálózati (csak hello AppVM01 kiszolgálón kitett tooresources).

Nincs olyan alapértelmezett kimenő szabály, amely lehetővé teszi, hogy a toohello kimenő forgalom internet. Ebben a példában a Microsoft most átengedi a kimenő forgalmat, és nem módosítja az egyetlen kimenő szabályok. toolock le mindkét irányú forgalmát, felhasználói definiált útválasztás szükséges, a rendszer írja le egy másik példa a hello található [fő biztonsági határ dokumentum][HOME].

a fenti hello tárgyalt NSG-szabályok nagyon hasonló toohello NSG-szabályok a [1 -. példa az NSG-ket egy egyszerű DMZ Build][Example1]. Tekintse át az egyes NSG-szabályokat és az attribútumok részletes tekintse meg a dokumentum az NSG leírás hello.

## <a name="firewall-rules"></a>Tűzfalszabályok
Egy olyan kezelőügyfelet toobe PC toomanage hello tűzfal telepítve kell, és a szükséges hello-konfigurációk létrehozása. Hogyan toomanage hello eszköz hello dokumentáció a tűzfal (vagy más NVA) szállító talál. Ez a szakasz többi hello hello konfigurációs hello tűzfal, hello szállító felügyeleti ügyfél (azaz nem hello Azure-portál vagy PowerShell) segítségével ismerteti.

A letölthető ügyfél és a kapcsolódó toohello ebben a példában használt Barracuda utasítások itt találhatók: [Barracuda NG rendszergazda](https://techlib.barracuda.com/NG61/NGAdmin)

Az hello tűzfal szabályok továbbítás kell toobe létrehozni. Ebben a példában csak továbbítja az internetes forgalmat az adathoz kötött toohello tűzfal és toohello webkiszolgáló, mert csak egy továbbító NAT-szabály van szükség. A hello Barracuda NextGen tűzfal szerepel ebben a példában hello szabály lenne a cél NAT-szabály ("nyári időszámítás NAT") toopass a forgalmat.

toocreate hello következő szabály (vagy ellenőrizze a meglévő alapértelmezett szabályok), hello Barracuda NG felügyeleti ügyfél irányítópult-től kezdődő, keresse meg a toohello konfiguráció lapon, a hello működési konfigurációs szakaszban kattintson a Ruleset. A rács nevű, "Main szabályok" jeleníti meg a hello tűzfal aktív és inaktív szabályok meglévő hello. A hello jobb felső sarkában a rácson a kisméretű, zöld "+" gombra, kattintson az új szabály toocreate (Megjegyzés: a tűzfal "zárolva" a változásokat, ha a gomb megjelölt "Lock", és nem toocreate, vagy a szabályok szerkesztése, kattintson erre a gombra kattintva túl "zárolásának feloldásához" hello szabálykészletben és szerkeszthető). Ha tooedit meglévő szabály, válassza ki, hogy a szabály, kattintson a jobb gombbal, és válassza ki a szabály szerkesztése.

Hozzon létre egy új szabályt, és adjon meg egy nevet, például a "WebTraffic". 

hello cél NAT-szabály ikon néz ki: ![cél NAT ikon][2]

maga hello szabály például ehhez hasonló lenne:

![Tűzfalszabály][3]

Itt bármely, hogy a találatok hello tooreach HTTP (80-as vagy 443-as HTTPS port) tett kísérlet tűzfal bejövő cím elküldi hello tűzfal "DHCP1 helyi IP-címe" illesztőfelület és átirányított toohello Web Server hello 10.0.1.5 IP-címét. Mivel a hello forgalom érkezik, 80-as porton és folyamatos toohello webkiszolgáló 80-as porton nem port módosítása volt szükség. Azonban a cél lista lehetett volna 10.0.1.5:8080 Ha a webalkalmazás-kiszolgáló figyel a port 8080-as így fordítása hello hello bejövő port 80-as porton hello tűzfal tooinbound 8080 hello webkiszolgálón.

A kapcsolódási módszert kell is kell szereplő, a cél szabály hello hello Internet, a "Dinamikus SNAT" legmegfelelőbb. 

Habár egyetlen szabály jött létre fontos, hogy helyesen van-e állítva a hozzá tartozó prioritás. Ha a szabályok hello tűzfalon hello alsó (alatt hello "BLOCKALL" szabály) a rendszer az új szabály hello rácsban soha nem érkezni fog szerepet. Győződjön meg arról, hogy webes forgalomban hello az újonnan létrehozott szabály hello BLOCKALL szabály felett van.

Hello szabály létrehozása után kell lennie toohello tűzfal leküldött és majd aktiválni, ha ez nem történik meg a hello szabály módosítása nem lépnek érvénybe. hello leküldéses és az aktiválási folyamat hello a következő szakaszban ismertetett.

## <a name="rule-activation"></a>A szabály aktiválás
Hello a ruleset módosítva lett tooadd Ez a szabály, hello szabálykészletben kell feltöltött toohello tűzfal- és aktiválva.

![Tűzfal szabály aktiválás][4]

Hello felügyeleti ügyfél hello jobb felső sarkában a fürtöket gombok. Hello "küldése módosítások" gomb toosend módosított hello szabályok toohello tűzfal elemre, majd kattintson a hello "Aktiválás" gombra.

A hello tűzfal ruleset hello aktiválásával e példa környezet build befejeződött. Másik lehetőségként hello post build hello a szakasz lehet hivatkozások futnak a parancsfájlok tooadd egy alkalmazás toothis környezet tootest hello forgalom forgatókönyvek alatt.

> [!IMPORTANT]
> Fontos, hogy Ön nem elért hello webkiszolgáló közvetlenül kritikus toorealize. Ha egy böngészőben kér FrontEnd001.CloudApp.Net egy HTTP-lap, a forgalom toohello tűzfal nem hello hello HTTP végpont (80-as port) fázisok webkiszolgáló. hello tűzfal, akkor a fenti, toohello webkiszolgáló kérő NAT toohello szabály szerint létrehozott.
> 
> 

## <a name="traffic-scenarios"></a>Forgalom forgatókönyvek
#### <a name="allowed-web-tooweb-server-through-firewall"></a>(Engedélyezett) Webes tooWeb Server tűzfalon keresztül
1. Internetes felhasználói kérelmek HTTP oldal FrontEnd001.CloudApp.Net (Internet Facing Felhőszolgáltatás)
2. Felhőalapú szolgáltatás fázisok forgalmat a 80-as port toofirewall helyi illesztőfelületen 10.0.1.4:80 a nyitott végpontok keresztül
3. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
   1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
   2. NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály
   3. NSG 3. szabály (Internet tooFirewall) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
4. Forgalom találatok hello tűzfal (10.0.1.4) belső IP-címe
5. Tűzfal továbbítási szabály tekintse meg a 80-as portot a forgalom, átirányítja azt toohello webkiszolgáló IIS01
6. IIS01 a webes forgalom figyeli, ezt a kérelmet kap, és elindítja a hello kérés feldolgozása
7. IIS01 hello SQL Server a AppVM01 adatokat kéri
8. Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van
9. hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
   1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
   2. NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály
   3. NSG 3. szabály (Internet tooFirewall) nem vonatkozik, helyezze át a toonext szabály
   4. NSG 4. szabály (IIS01 tooAppVM01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
10. AppVM01 hello SQL-lekérdezést kap, és válaszolhat
11. Mivel nincsenek engedélyezve van-e a hello Backend alhálózathoz hello válasz nem kimenő szabályok
12. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
    1. Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály
    2. hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van.
13. hello IIS server hello SQL-válasz fogadása és hello HTTP-válasz befejeződik, és elküldi a toohello kérelmező
14. Mivel ez a NAT-munkamenet hello tűzfaltól, hello válasz (kezdetben) célja a hello tűzfal
15. hello tűzfal hello webkiszolgáló hello választ kap, és továbbítja a hátsó toohello internetes felhasználó
16. Mivel nincsenek hello Frontend alhálózathoz hello válasz kimenő szabályait nem engedélyezett, és internetes felhasználó hello kap a kért weblap hello.

#### <a name="allowed-rdp-toobackend"></a>(Engedélyezett) RDP tooBackend
1. Server Admin interneten kérelmek RDP munkamenet tooAppVM01 BackEnd001.CloudApp.Net:xxxxx, ahol xxxxx az RDP tooAppVM01 véletlenszerűen hello portszámát a (hozzárendelt hello port található hello Azure portál vagy a PowerShell segítségével)
2. Hello tűzfal csak figyel a következőn: hello FrontEnd001.CloudApp.Net cím, mert nem részt vesz a forgalom áramlását az
3. Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
   1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
   2. NSG szabály 2 (RDP) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
4. A nem kimenő szabályokat alapértelmezett szabályokat alkalmazni, és a forgalom engedélyezve van
5. RDP-munkamenetbe engedélyezve van
6. Felhasználói nevének jelszavának megadását kéri az AppVM01

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Engedélyezett) Web Server DNS-címkeresés DNS-kiszolgálón
1. Webalkalmazás-kiszolgáló, IIS01, kell egy adatcsatorna www.data.gov, azonban kell tooresolve hello cím.
2. hello tartozó fürthálózat-konfiguráció hello hálózatok listája DNS01 (a hello Backend alhálózathoz 10.0.2.4) hello elsődleges DNS-kiszolgálóként, IIS01 küldi hello DNS kérelem tooDNS01
3. Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van
4. Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
   1. NSG szabály 1 (DNS) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
5. DNS-kiszolgáló hello kérést kap.
6. DNS-kiszolgáló nem rendelkezik gyorsítótárazott hello címmel és egy legfelső szintű DNS-kiszolgáló kéri a hello internet
7. Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van
8. Internetes DNS-kiszolgáló válaszol, mivel ehhez a munkamenethez belső kezdeményezték, hello válasz engedélyezett
9. DNS-kiszolgáló gyorsítótárazza hello választ, és toohello kezdeti kérés hátsó tooIIS01 válaszol
10. Nincs kimenő szabályok a Backend alhálózathoz forgalom engedélyezve van
11. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
    1. Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály
    2. hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van
12. IIS01 hello választ kap DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Engedélyezett) Server-hozzáférés fájl AppVM01
1. IIS01 kér AppVM01 fájlba
2. Nincs kimenő szabályok Frontend alhálózaton, forgalom engedélyezve van
3. hello Backend alhálózathoz bejövő szabály feldolgozása kezdődik:
   1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
   2. NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály
   3. NSG 3. szabály (Internet tooFirewall) nem vonatkozik, helyezze át a toonext szabály
   4. NSG 4. szabály (IIS01 tooAppVM01) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
4. AppVM01 hello kérelmet kap, és válaszol, a fájl (feltéve, hogy van engedélyezve)
5. Mivel nincsenek engedélyezve van-e a hello Backend alhálózathoz hello válasz nem kimenő szabályok
6. Frontend alhálózathoz bejövő szabály feldolgozása kezdődik:
   1. Nincs alkalmazó tooInbound forgalom hello háttér alhálózati toohello Frontend alhálózatból, így ha hello NSG-szabályok egyike NSG szabály
   2. hello alapértelmezett rendszerszabály, amely lehetővé teszi az alhálózatok közötti forgalmat lehetővé tenné a forgalmat, így hello forgalom engedélyezve van.
7. hello IIS kiszolgáló kap hello fájl

#### <a name="denied-web-direct-tooweb-server"></a>(Megtagadva) Webalkalmazás-kiszolgáló közvetlen tooWeb
Hello webkiszolgáló IIS01 és hello tűzfal óta vannak a hello ugyanazt a felhőalapú szolgáltatás közös hello azonos nyilvános felé néző IP-címet. Így minden HTTP-forgalom lesz átirányítja toohello tűzfal. Hello kérelem sikeresen kiszolgálta lenne, amíg azt nem tud közvetlenül toohello webkiszolgáló, az átadott, célja, keresztül hello tűzfal először. Lásd: hello első forgatókönyv ebben a szakaszban a hello adatforgalmat.

#### <a name="denied-web-toobackend-server"></a>(Megtagadva) Webalkalmazás-kiszolgáló tooBackend
1. Internetes felhasználó megpróbál tooaccess keresztül hello BackEnd001.CloudApp.Net szolgáltatás AppVM01 fájlba
2. Mivel nincsenek nyissa meg a fájlmegosztás nincsenek végpontok, ez nem lenne továbbítja hello felhőalapú szolgáltatás, és nem tudnák elérni hello kiszolgálót
3. Ha valamilyen okból hello végpontok nyitott, NSG szabály 5 (Internet tooVNet) blokkolná-e a forgalom

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Megtagadva) Webes DNS-címkeresés DNS-kiszolgálón
1. Internetes felhasználó megpróbál toolookup egy belső DNS-rekordot a DNS01 keresztül hello BackEnd001.CloudApp.Net szolgáltatás
2. Mivel nincsenek végpontok nyissa meg a DNS, ez nem lenne továbbítja hello felhőalapú szolgáltatás, és így elérni hello kiszolgálót
3. Ha valamilyen okból hello végpontok nyitott, NSG szabály 5 (Internet tooVNet) blokkolná-e a forgalom (Megjegyzés: két okból nem kell alkalmazni, hogy a szabály 1 (DNS), első hello forráscím van hello internet, ez a szabály csak érvényes toohello hello mint virtuális helyi hálózat is forrás Ez az egy olyan engedélyezési szabály, akkor soha nem letiltsa a forgalom)

#### <a name="denied-web-toosql-access-through-firewall"></a>(Megtagadva) Webes tooSQL hozzáférés tűzfalon keresztül
1. Internetes felhasználó SQL adatokat kér a FrontEnd001.CloudApp.Net (Internet Facing Felhőszolgáltatás)
2. Mivel nincsenek végpontok nyissa meg az SQL, ez nem lenne továbbítja hello felhőalapú szolgáltatás, és nem tudnák elérni hello tűzfal
3. Ha valamilyen okból végpontok voltak nyitva, hello Frontend alhálózathoz megkezdi a bejövő szabály feldolgozása:
   1. NSG szabály 1 (DNS) nem vonatkozik, helyezze át a toonext szabály
   2. NSG szabály 2 (RDP) nem vonatkozik, helyezze át a toonext szabály
   3. NSG 2. szabály (Internet tooFirewall) alkalmazza, akkor engedélyezett, befejezése szabály feldolgozása
4. Forgalom találatok hello tűzfal (10.0.1.4) belső IP-címe
5. Tűzfal nincs továbbítási szabályaik rendelkezik SQL és elhagyta hello forgalom

## <a name="conclusion"></a>Összegzés
Ez viszonylag egyszerű módja az alkalmazás egy tűzfal védi és hello háttér alhálózathoz a bejövő forgalom elkülönítésére.

További példákat és a hálózati biztonsági határokat egy áttekintést talál [Itt][HOME].

## <a name="references"></a>Referencia
### <a name="main-script-and-network-config"></a>Fő parancsfájlt és a hálózati konfiguráció
Hello teljes parancsfájl menthető egy olyan PowerShell-parancsfájlt. Hálózati konfiguráció hello "NetworkConf2.xml" fájlba mentése.
Szükség szerint módosítsa a hello felhasználó által definiált változókat. Hello parancsfájl futtatása, majd kövesse a hello tűzfal szabály telepítő utasítás fenti.

#### <a name="full-script"></a>Teljes körű parancsfájl
Fogja ezt a parancsfájlt, a felhasználó által definiált változóknak hello alapján:

1. Csatlakozás Azure-előfizetés tooan
2. Új tárfiók létrehozása
3. Hozzon létre egy új virtuális hálózat és két alhálózat hello hálózati konfigurációs fájlban meghatározottak szerint
4. 4 a windows server virtuális gépek létrehozása
5. Adja meg, beleértve az NSG:
   * Egy NSG létrehozása
   * Azt a szabályoknak feltöltése
   * Kötési hello NSG toohello megfelelő alhálózatokat

A PowerShell parancsfájl fusson helyben egy internethez csatlakoztatott PC vagy a kiszolgáló.

> [!IMPORTANT]
> Ezt a parancsfájlt, amikor előfordulhat figyelmeztetések vagy a PowerShellben pop más tájékoztató üzeneteit. Piros csak hibaüzenetek aggodalomra.
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on hello FrontEnd Subnet (plus hello NVA on hello FrontEnd subnet)
       - Three Servers on hello BackEnd Subnet
       - Network Security Groups tooallow/deny traffic patterns as declared

      Before running script, ensure hello network configuration file is created in
      hello directory referenced by $NetworkConfigFile variable (or update the
      variable tooreflect hello path and file name of hello config file being used).

     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      hello appropriate layer(s) of protection. This script serves as an example of some
      of hello techniques that can be used, but should not be used for all scenarios. You
      are responsible tooassess your security needs and hello appropriate protections
      needed, and then effectively implement those protections.

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       myFirewall - 10.0.1.4
       IIS01      - 10.0.1.5

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password toobe used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes tooreflect your subscription and services
      # Invalid options will fail in hello validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: tooensure proper NSG Rule creation later in this script:
        #       - hello Web Server must be VM 1
        #       - hello AppVM1 Server must be VM 2
        #       - hello DNS server must be VM 4
        #
        #       Otherwise hello NSG rules in hello last section of this
        #       script will need toobe changed toomatch hello modified
        #       VM array numbers ($i) so hello NSG Rule IP addresses
        #       are aligned toohello associated VM IP addresses.

        # VM 0 - hello Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 1 - hello Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"

        # VM 2 - hello First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - hello Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - hello DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer tooNew Storage Account
        Write-Host "Updating Subscription Pointer tooNew Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "hello BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'hello network config file was not found, please update hello $NetworkConfigFile variable toopoint toohello network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation varible is correct and hello netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occured, please see hello above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building hello environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all hello EndPoints we'll need once we're up and running
                # Note: Web traffic goes through hello firewall, so we'll need tooset up a HTTP endpoint.
                #       Also, hello firewall will be redirecting web traffic tooa new IP and Port in a
                #       forwarding rule, so hello HTTP endpoint here will have hello same public and local
                #       port and hello firewall will do hello NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when hello appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }

    # Configure NSG
        Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

      # Build hello NSG
        Write-Host "Building hello NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rules
        Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet too$($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) too$($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *

        # Assign hello NSG toohello Subnets
            Write-Host "Binding hello NSG tooboth subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on hello IIS Server)
      # Install Backend resource (Run Post-Build Script on hello AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a>Hálózati konfigurációs fájl
Az XML-fájl mentése frissített hellyel rendelkező, és adja hozzá a hello hivatkozás toothis fájl toohello $NetworkConfigFile változó a fenti hello parancsfájl.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Alkalmazás mintaparancsfájlok
Ha a tooinstall mintaalkalmazás ezzel és más DMZ példák, egy adtak meg, a következő hivatkozás hello: [minta alkalmazás-parancsfájl][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Az NSG bejövő DMZ"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Cél NAT ikon"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Tűzfalszabály"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Tűzfal szabály aktiválás"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
