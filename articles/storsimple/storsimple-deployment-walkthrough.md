---
title: "egy helyszíni StorSimple eszköz aaaDeploy |} Microsoft Docs"
description: "Hello lépés és ajánlott eljárás rendszerbe állítása hello StorSimple eszköz és a szolgáltatás ismerteti. (TooMicrosoft Azure StorSimple.3 verziója vonatkozik, és korábban.)"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b27f87a2-1363-4e0d-90f7-37b5dd1f21c9
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: d45abba1786ceae586a99ca77b90de3f290c2f0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-on-premises-storsimple-device"></a>A helyszíni StorSimple eszköz üzembe helyezése
> [!div class="op_single_selector"]
> * [2. frissítés](storsimple-deployment-walkthrough-u2.md)
> * [1. frissítés](storsimple-deployment-walkthrough-u1.md)
> * [GA kiadás](storsimple-deployment-walkthrough.md)
> 
> 

## <a name="overview"></a>Áttekintés
Üdvözli a tooMicrosoft Azure StorSimple eszköztelepítő útmutatójában. Ezek az üzembehelyezési oktatóanyagok alkalmazása tooStorSimple 8000 Series kiadási verziójára, a StorSimple 8000 Series Update 0,1, a StorSimple 8000 Series Update 0,2 és a StorSimple 8000 Series Update 0,3. Ez az oktatóanyag-sorozat ismerteti, hogyan tooconfigure a StorSimple eszközt, és tartalmazza a konfigurációs ellenőrzőlistát, konfigurációs előfeltételeket és részletes konfigurációs lépéseket.

hello szereplő információk feltételezi, hogy áttekintése hello biztonsági óvintézkedéseket, és kicsomagolása, oktatóanyagban, és bekábelezte a StorSimple eszközt. Ha továbbra is szükséges tooperform azokat a feladatokat, kezdje hello megtekintésével [biztonsági óvintézkedéseket](storsimple-safety.md). Az eszköz modelltől függően ezután kicsomagolhatja, állványra szerelheti és bekábelezheti hello utasításait követve:

* [A StorSimple 8100 kicsomagolása, állványra szerelése és bekábelezése](storsimple-8100-hardware-installation.md)
* [A StorSimple 8600 kicsomagolása, állványra szerelése és bekábelezése](storsimple-8600-hardware-installation.md)

Szüksége lesz a rendszergazdai jogosultságokkal toocomplete hello beállítása és konfigurációja folyamat. Ajánlott áttekinteni hello konfigurációs ellenőrzőlista megkezdése előtt. hello üzembe helyezési és konfigurálási folyamat eltarthat néhány alkalommal toocomplete.

> [!NOTE]
> hello hello Microsoft Azure webhelyen közzétett StorSimple üzembehelyezési információk érvényes tooStorSimple 8000 sorozat eszközeire érvényesek. A hello 5000 és a 7000-es adatsorozat eszközökkel kapcsolatos részletes információkért lásd: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 5000-es és 7000-es sorozatra vonatkozó üzembehelyezési információkat lásd: hello [StorSimple rendszer gyors üzembehelyezési útmutatójában](http://onlinehelp.storsimple.com/111_Appliance/).
> 
> 

## <a name="deployment-steps"></a>A központi telepítés lépései
Hajtsa végre a szükséges lépéseket tooconfigure a StorSimple eszközt, és csatlakoztassa tooyour StorSimple Manager szolgáltatás. Ezenkívül toohello szükséges lépéseket, opcionális lépések és eljárások hello központi telepítése során szükség lehet. hello részletes üzembehelyezési utasítások jelzik, ha egyes opcionális lépéseket végre kell hajtania.

| Lépés | Leírás |
| --- | --- |
| **ELŐFELTÉTELEK** |Ezeknek kell toobe hello üzembe helyezésre felkészülés során. |
| Üzembehelyezési konfigurációs ellenőrzőlista. |A feladatlista toogather-rekord információk előzetes tooand hello telepítéskor használni. |
| Üzembehelyezési előfeltételek. |Ezek ellenőrzik, hogy hello környezet készen áll a központi telepítés. |
|  | |
| **RÉSZLETES ÜZEMBE HELYEZÉS** |Ezeket a lépéseket a StorSimple eszköz éles környezetben vannak a szükséges toodeploy. |
| 1. lépés: Új szolgáltatás létrehozása. |A felhőfelügyelet és a felhőalapú tárolás beállítása a StorSimple eszközhöz. Hagyja ki ezt a lépést, ha már rendelkezik meglévő szolgáltatással más StorSimple eszközökhöz. |
| 2. lépés: Hello Szolgáltatásregisztrációs kulcs lekérése. |A kulcs tooregister használja a & csatlakoztathatja a StorSimple eszköz hello felügyeleti szolgáltatáshoz. |
| 3. lépés: Eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple. |Hello eszköz tooyour hálózat, és regisztrálhatja azt az Azure toocomplete hello beállítása hello management szolgáltatás. |
| 4. lépés: Minimális eszközbeállítások végrehajtása.</br>Választható lehetőség: A StorSimple eszköz frissítése. |Hello management toocomplete hello eszköz telepítése és tooprovide tárolási engedélyezheti azt. |
| 5. lépés: Kötettároló létrehozása. |Hozzon létre egy tároló tooprovision köteteket. A kötettároló tárfiók, sávszélesség és a benne tárolt összes hello kötet titkosítási beállításait. |
| 6. lépés: Kötet létrehozása. |Tárkötet hello StorSimple eszközön a kiszolgálók számára. |
| 7. lépés: Kötet csatlakoztatása, inicializálása és formázása.</br>Választható lehetőség: Az MPIO konfigurálása. |Csatlakoztassa a kiszolgálókat toohello hello eszköz által biztosított iSCSI-tárolóhoz. Választható lehetőségként konfigurálhatja az MPIO tooensure, hogy a kiszolgálók képesek legyenek tűrni kapcsolati, a hálózati és az adapterhibákat. |
| 8. lépés: Biztonsági mentés készítése. |Az adatok a biztonsági mentési házirend tooprotect beállítása |
|  | |
| **EGYÉB ELJÁRÁSOK** |Szükség lehet toorefer toothese eljárások a megoldás központi telepítésekor. |
| Hello szolgáltatást egy új tárfiók konfigurálása. | |
| Használja a PuTTY tooconnect toohello eszköz soros konzoljához. | |
| Hello egy Windows Server-állomás IQN-Nevének beolvasása. | |
| Manuális biztonsági mentés létrehozása. | |

## <a name="deployment-configuration-checklist"></a>Üzembehelyezési konfigurációs ellenőrzőlista
a következő üzembehelyezési konfigurációs ellenőrzőlista hello toocollect kell előtt, és a StorSimple eszköz konfigurálásakor hello szoftver hello információkat ismerteti. Felkészülés az információk időben történő előkészítésével leegyszerűsíthető hello hello StorSimple eszköz környezetében telepítési folyamata. Használja a feladatlista tooalso jegyezze fel hello konfigurációs adatait az eszköz üzembe helyezése.

| Fázis | Paraméter | Részletek | Értékek |
| --- | --- | --- | --- |
| **Az eszköz bekábelezése** |Soros hozzáférés |Az eszköz kezdeti konfigurációja |Igen/nem |
|  | | | |
| **Az eszköz konfigurálása és regisztrálása** |Data 0 hálózati beállítások |Data 0 IP-címe:</br>Alhálózati maszk:</br>Átjáró:</br>Elsődleges DNS-kiszolgáló:</br>Elsődleges NTP-kiszolgáló:</br>IP/FQDN webes proxykiszolgáló (opcionális):</br>Webes proxy portja: | |
| &nbsp; |Az eszköz rendszergazdai jelszava |A jelszónak 8–15 karakter hosszúságúnak kell lennie, és tartalmaznia kell kisbetűt, nagybetűt, számot és speciális karaktert. | |
| &nbsp; |StorSimple Snapshot Manager jelszava |A jelszónak 14 vagy 15 karakter hosszúságúnak kell lennie, és tartalmaznia kell kisbetűt, nagybetűt, számot és speciális karaktert. | |
| &nbsp; |Szolgáltatásregisztrációs kulcs |Ez a kulcs hello a klasszikus Azure portálon jön létre. | |
| &nbsp; |Szolgáltatásadat-titkosítási kulcs |Ez a kulcs akkor jön létre, hello eszközt a StorSimple hello felügyeleti szolgáltatás hello Windows PowerShell használatával való regisztrálásakor. Másolja ki ezt a kulcsot, és mentse egy biztonságos helyre. | |
|  | | | |
| **Minimális eszközbeállítások végrehajtása** |Rövid név megadása az eszközhöz |Ez az eszköz hello egy leíró nevet. | |
| &nbsp; |Időzóna |Az eszköz minden ütemezett művelethez ezt az időzónát használja. | |
| &nbsp; |Másodlagos DNS-kiszolgáló |Ez egy szükséges konfiguráció. | |
| &nbsp; |Hálózati adapter: Data 0 vezérlő rögzített IP-címei |Ezek az IP-címeknek irányítható toohello Internet kell lennie.</br>0. vezérlő rögzített IP-címe:</br>1. vezérlő rögzített IP-címe: | |
|  | | | |
| **További hálózatiadapter-beállítások** |Hálózati adapter: Data 1</br>Ha az iSCSI engedélyezve van, ne konfiguráljon hello átjáró. |Felhasználási cél: Felhő/iSCSI/nincs használatban</br>IP-cím:</br>Alhálózati maszk:</br>Átjáró: | |
| &nbsp; |Hálózati adapter: Data 2</br>Ha az iSCSI engedélyezve van, ne konfiguráljon hello átjáró. |Felhasználási cél: Felhő/iSCSI/nincs használatban</br>IP-cím:</br>Alhálózati maszk:</br>Átjáró: | |
| &nbsp; |Hálózati adapter: Data 3</br>Ha az iSCSI engedélyezve van, ne konfiguráljon hello átjáró. |Felhasználási cél: Felhő/iSCSI/nincs használatban</br>IP-cím:</br>Alhálózati maszk:</br>Átjáró: | |
| &nbsp; |Hálózati adapter: Data 4</br>Ha az iSCSI engedélyezve van, ne konfiguráljon hello átjáró. |Felhasználási cél: Felhő/iSCSI/nincs használatban</br>IP-cím:</br>Alhálózati maszk:</br>Átjáró: | |
| &nbsp; |Hálózati adapter: Data 5</br>Ha az iSCSI engedélyezve van, ne konfiguráljon hello átjáró. |Felhasználási cél: Felhő/iSCSI/nincs használatban</br>IP-cím:</br>Alhálózati maszk:</br>Átjáró: | |
|  | | | |
| **Kötettároló létrehozása** |Kötettároló neve: |Hello a tároló neve | |
| &nbsp; |Azure Storage-fiók: |Tárolási fiók neve és elérési kulcs tooassociate az ehhez a kötettárolóhoz | |
| &nbsp; |Felhőalapú tárolás titkosítási kulcsa: |Titkosítási kulcs az egyes tárolókban való tároláshoz | |
|  | | | |
| **Kötet létrehozása** |Az egyes kötetek részletei |Kötet neve: | |
| &nbsp; |&nbsp; |Méret: | |
| &nbsp; |&nbsp; |Használat típusa: | |
| &nbsp; |&nbsp; |ACR neve: | |
| &nbsp; |&nbsp; |Alapértelmezett biztonsági mentési házirend: | |
|  | | | |
| **Kötet csatlakoztatása, inicializálása és formázása** |Toohello tárolási csatlakozó egyes gazdakiszolgálók részletei |Windows Server neve: | |
| &nbsp; |&nbsp; |Windows Server IQN: | |
| &nbsp; |&nbsp; |Windows Server kötetneve: | |
| &nbsp; |&nbsp; |NTFS csatlakozási pont/meghajtóbetűjel: | |

## <a name="deployment-prerequisites"></a>Üzembehelyezési előfeltételek
a következő szakaszok hello hello konfigurációs előfeltételeit a StorSimple Manager szolgáltatás, a StorSimple eszköz és az Adatközpont hálózatának hello ismertetik.

### <a name="for-hello-storsimple-manager-service"></a>A StorSimple Manager szolgáltatás hello
Mielőtt hozzákezd, győződjön meg az alábbiakról:

* Rendelkezik Microsoft-fiókkal és a hozzá szükséges hozzáférési hitelesítő adatokkal.
* Rendelkezik Microsoft Azure Storage-fiókkal és a hozzá szükséges hozzáférési hitelesítő adatokkal.
* A Microsoft Azure-előfizetés hello StorSimple Manager szolgáltatás engedélyezve van. Az előfizetés hello keresztül kell megvásárolni [nagyvállalati szerződés](https://azure.microsoft.com/pricing/enterprise-agreement/).
* Hozzáférés tooterminal emuláció szoftvert, például a PuTTY rendelkezik.

### <a name="for-hello-device-in-hello-datacenter"></a>Hello adatközpontban hello eszköz
Mielőtt konfigurálná a hello eszköz, győződjön meg arról, hogy:

* Az eszköz teljesen ki van csomagolva, állványra van rögzítve és minden kábel be van kötve a tápellátáshoz, a hálózathoz és a soros hozzáféréshez, a következő helyen leírtak szerint:
  
  * [A StorSimple 8100 kicsomagolása, állványra szerelése és bekábelezése](storsimple-8100-hardware-installation.md)
  * [A StorSimple 8600 kicsomagolása, állványra szerelése és bekábelezése](storsimple-8600-hardware-installation.md)

### <a name="for-hello-network-in-hello-datacenter"></a>Hello adatközpontban hello hálózat
Mielőtt hozzákezd, győződjön meg az alábbiakról:

* hello az Adatközpont tűzfalának portjait iSCSI és a felhőalapú forgalom megnyitott tooallow leírtak [a StorSimple eszköz hálózatkezelési követelményei](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* az adatközpontjában található eszköz hello toooutside hálózati is elérheti. Futtassa a következő hello [Windows PowerShell 4.0](http://www.microsoft.com/download/details.aspx?id=40855) (a lenti) parancsmagok toovalidate hello kapcsolat toohello hálózaton kívülről. Az érvényesítést hajt végre (az Adatközpont hálózatán) számítógépen a kapcsolat tooAzure és hol a StorSimple eszköz fog telepíteni.  

| Ehhez a paraméterhez… | toocheck hello érvényességi... | Ezeket a parancsokat/parancsmagokat futtassa. |
| --- | --- | --- |
| **IP**</br>**Alhálózat**</br>**Átjáró** |Ez egy érvényes IPv4- vagy IPv6-cím?</br>Ez egy érvényes alhálózat?</br>Ez egy érvényes átjáró?</br>Ez egy ismétlődő IP-cím a hálózaton? |`ping ip`</br>`arp -a`</br>Hello `ping` és `arp` parancsok sikertelenek, hogy nincs-e telepítve, amely használja az IP-hello adatközpont hálózatán jelző. |
|  | | |
| **DNS** |Ez egy érvényes DNS, amely képes Azure URL-címeket feloldani? |`Resolve-DnsName -Name www.bing.com -Server <DNS server IP address>` </br>Alternatív parancsként használható a következő:</br>`nslookup --dns-ip=<DNS server IP address> www.bing.com` |
| &nbsp; |Ellenőrizze, hogy nyitva van-e az 53-as port. Ez csak abban az esetben érvényes, ha az eszközhöz külső DNS-t használ. Belső DNS automatikusan oldja fel az hello külső URL-címeket. |`Test-Port -comp dc1 -port 53 -udp -UDPtimeout 10000`  </br>[További információk erről a parancsmagról](http://learn-powershell.net/2011/02/21/querying-udp-ports-with-powershell/) |
|  | | |
| **NTP** |Az NTP-kiszolgáló megadása után azonnal elindítunk egy időszinkronizálást. A `time.windows.com` vagy nyilvános időkiszolgálók megadásakor ellenőrizze, hogy nyitva van-e a 123-as port. |[Töltse le és használja ezt a parancsfájlt](https://gallery.technet.microsoft.com/scriptcenter/Get-Network-NTP-Time-with-07b216ca). |
|  | | |
| **Proxy (opcionális)** |Érvényes ez a proxy URI és port? </br> A helyes hello hitelesítési mód? |<code>wget http://bing.com &#124; % {$_.StatusCode}</code></br>Ezt a parancsot közvetlenül a webproxy konfigurálása után kell futtatni. Ha a 200-as állapotkódot lett visszaadva, akkor azt jelzi, hogy hello kapcsolat sikeres. |
| &nbsp; |Irányítható proxyn keresztül a forgalom? |Futtassa a hello DNS-ellenőrzést, az NTP-ellenőrzést vagy a HTTP-ellenőrzést egyszer, Miután konfigurálta a proxyt az eszközén. Ezáltal világossá válik, hogy blokkolva van-e a forgalom a proxynál vagy máshol. |
|  | | |
| **Regisztráció** |Ellenőrizze, hogy a 443-as, 80-as és 9354-es kimeneti TCP-portok nyitva vannak-e. |`Test-NetConnection -Port   443 -InformationLevel Detailed`</br>[További információ a Test-NetConnection parancsmagról](https://technet.microsoft.com/library/dn372891.aspx) |

## <a name="step-by-step-deployment"></a>Részletes üzembe helyezés
Használja a következő lépésről toodeploy hello a StorSimple eszköz hello adatközpontban.

## <a name="step-1-create-a-new-service"></a>1. lépés: Új szolgáltatás létrehozása
A StorSimple Manager szolgáltatás több StorSimple eszközt is tud kezelni. Az első StorSimple eszköz hello telepítését szüksége lesz egy új StorSimple Manager szolgáltatás toocreate.

> [!IMPORTANT]
> Kihagyhatja ezt a lépést, ha egy meglévő StorSimple Manager szolgáltatás és a StorSimple eszközét ezzel toodeploy szeretné.
> 
> 

Hajtsa végre a következő lépéseket toocreate hello StorSimple Manager szolgáltatás egy új példányát hello.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [!IMPORTANT]
> Ha nem engedélyezte a tárfiók automatikus létrehozását hello a szolgáltatással, szüksége lesz a toocreate legalább egy tárfiókot, miután sikeresen létrehozott egy szolgáltatást. Ezt a tárfiókot akkor fogja használni a rendszer, amikor egy kötettárolót hoz létre.
> 
> Ha nem hozott létre egy tárfiókot automatikusan, nyissa meg túl[hello szolgáltatást egy új tárfiók konfigurálása](#configure-a-new-storage-account-for-the-service) részletes információkra van szüksége.
> Ha engedélyezte a tárfiók automatikus létrehozását hello, nyissa meg túl[2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](#step-2:-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-hello-service-registration-key"></a>2. lépés: Hello Szolgáltatásregisztrációs kulcs lekérése
Miután hello StorSimple Manager szolgáltatás fut, akkor tooget hello Szolgáltatásregisztrációs kulcs. Ez a kulcs használt tooregister, és csatlakoztathatja StorSimple eszközét hello szolgáltatásban.

Hajtsa végre a következő lépéseket a klasszikus Azure portálon hello hello.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-hello-device-through-windows-powershell-for-storsimple"></a>3. lépés: Eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple
> [!IMPORTANT]
> Előzetes tooperforming ebben a konfigurációban kihúzásával hello hálózati adaptereken DATA 0 kivételével mindkét (aktív és passzív) hello tartományvezérlőn.
> 
> 

A Windows PowerShell használata a StorSimple toocomplete hello kezdeti telepítése a StorSimple eszköz eljárást követő hello leírtak szerint. Ezzel a lépéssel kell toouse terminálemulációs szoftver toocomplete. További információkért lásd: [PuTTY használata tooconnect toohello eszköz soros konzoljához](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-configure-and-register-device](../../includes/storsimple-configure-and-register-device.md)]

## <a name="step-4-complete-minimum-device-setup"></a>4. lépés: Minimális eszközbeállítások végrehajtása.
A StorSimple eszköz hello minimális eszköz konfigurálásához akkor szükségesek:

* Hello másodlagos DNS-kiszolgáló beállítása.
* Engedélyezze az iSCSI-t legalább egy hálózati adapteren.
* Rendeljen rögzített IP-címek tooboth hello tartományvezérlők.

Hajtsa végre a következő lépéseket a hello Azure klasszikus portál toocomplete hello minimális eszközbeállítások hello.

[!INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup.md)]

Hello eszközkonfiguráció befejezése után, a frissítések keresése, és elérhető, ha a frissítések telepítése. hello frissítéseket is igénybe vehet néhány óra toocomplete. Hello utasításait követve [keresése és telepítése a frissítések](#scan-for-and-apply-updates).

## <a name="step-5-create-a-volume-container"></a>5. lépés: Kötettároló létrehozása
A kötettároló tárfiók, sávszélesség és a benne tárolt összes hello kötet titkosítási beállításait. A kötettároló toocreate kell ahhoz, hogy elkezdhessen köteteket a StorSimple eszköz.

Hajtsa végre a következő lépéseket a klasszikus portál Azure toocreate kötettároló hello hello.

[!INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>6. lépés: Kötet létrehozása
A kötettároló létrehozása után a kiszolgálók oszthat hello StorSimple eszközön tárolókötetet. Hajtsa végre a következő lépéseket az Azure klasszikus portál toocreate kötet hello hello.

> [!IMPORTANT]
> A StorSimple Manager csak dinamikusan kiosztott köteteket képes létrehozni.  Nem hozhat létre teljesen vagy részben kiosztott köteteket.
> 
> 

[!INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>7. lépés: Kötet csatlakoztatása, inicializálása és formázása
> [!IMPORTANT]
> * A hello magas rendelkezésre állás érdekében a StorSimple megoldás azt javasoljuk, hogy MPIO konfigurálását a Windows Server-állomás (nem kötelező) előzetes tooconfiguring iSCSI a Windows Server-állomáson. Az MPIO konfigurációja lévő gazdakiszolgálókra biztosítja, hogy hello kiszolgálók képesek legyenek tűrni a hivatkozásra, a hálózati vagy az adapterhibákat.
> * Az MPIO és az iSCSI telepítési és konfigurálási utasításait, nyissa meg túl[MPIO konfigurálása a StorSimple eszköz](storsimple-configure-mpio-windows-server.md). Ezek közé tartozik a hello lépéseket toomount, inicializálása és is formázza a StorSimple-köteteket.
> 
> 

Ha döntse el, nem tooconfigure MPIO, hajtsa végre a következő lépéseket toomount hello, inicializálja és formázza a StorSimple-köteteket.

[!INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>8. lépés: Biztonsági mentés készítése
Az adott időpontban mentett biztonsági másolatok védelmet biztosítanak a kötetek számára, továbbá javítják a rendelkezésre álló helyreállítási lehetőségeket, miközben a helyreállítási időt csökkentik. A StorSimple eszközén kétféle biztonsági mentést készíthet: helyi pillanatképeket és felhőbeli pillanatképeket. Mind a kétféle biztonsági mentés lehet **Ütemezett** vagy **Manuális**.

Hajtsa végre a következő lépéseket az Azure klasszikus portál toocreate ütemezett biztonsági mentés hello hello.

[!INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Manuális biztonsági mentést bármikor létrehozhat. Az eljárások, nyissa meg túl[manuális biztonsági mentés létrehozása](#Create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-hello-service"></a>Hello szolgáltatást egy új tárfiók konfigurálása
Ez az egy opcionális lépés csak akkor, ha a szolgáltatással nem engedélyezte a tárfiók automatikus létrehozását hello tooperform kell. A Microsoft Azure storage-fiók szükséges toocreate StorSimple-kötettároló.

Ha Azure-tárfiók más régióban toocreate van szüksége, tekintse meg [kapcsolatos Azure Storage-fiókok](../storage/common/storage-create-storage-account.md) részletes útmutatásait.

Hajtsa végre a következő lépéseket a klasszikus Azure portálon, a hello hello hello **StorSimple Manager szolgáltatás** lap.

[!INCLUDE [storsimple-configure-new-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="use-putty-tooconnect-toohello-device-serial-console"></a>PuTTY tooconnect toohello eszköz soros konzoljához használata
tooconnect tooWindows PowerShell-lel, mint a PuTTY toouse terminálemulációs szoftverre kell. A PuTTY segítségével használhatja, ha hello eszközhöz való hozzáféréshez hello soros konzolon keresztül közvetlenül vagy egy telnet-munkamenet távoli számítógépről történő megnyitásával.

[!INCLUDE [Use PuTTY tooconnect toohello device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Frissítések keresése és telepítése
Az eszköz frissítése 1-4 órát is igénybe vehet. Hajtsa végre a következő lépéseket tooscan a hello, és alkalmazza a frissítéseket az eszközön.

> [!NOTE]
> Ha egy átjáró konfigurálva, mint a Data 0 hálózati adapteren, szüksége lesz toodisable Data 2 és a Data 3 hálózati adaptereket hello frissítés telepítése előtt. Nyissa meg túl**eszközök > Konfigurálás** , és tiltsa le a Data 2 és a Data 3 adaptereket. Ezek a kapcsolatok hello eszköz frissítése után újra engedélyezheti.
> 
> 

#### <a name="tooupdate-your-device"></a>tooupdate az eszköz
1. Hello eszközön **gyors üzembe helyezés** kattintson **eszközök**. Jelölje ki azt a fizikai eszköz hello **karbantartási** majd **frissítések keresése**.  
2. Létrejön egy feladat tooscan az elérhető frissítések. Ha frissítések érhetők el, hello **frissítések keresése** túl változik**frissítések telepítése**. Kattintson a **Frissítések telepítése** elemre. Előfordulhat, hogy kért toodisable Data 2 és a Data 3 előzetes tooinstalling hello frissítéseket. Le kell tiltania ezeket a hálózati illesztőket vagy hello frissítések sikertelenek lehetnek.
3. A rendszer létrehoz egy frissítési feladatot. A frissítés hello állapotának figyelése túl navigálva**feladatok**.
   
   > [!NOTE]
   > Hello frissítési feladat elindulásakor azonnal megjeleníti hello állapotát 50 % lesz. hello majd állapota too100 százalék csak hello frissítési feladat befejeződése után. Nincs hello frissítések folyamat nem valós idejű állapotát.
   > 
   > 
4. Hello eszköz sikeres frissítése után engedélyezze Data 2 és a Data 3 hálózati adaptereket, ha ezek le voltak tiltva.

## <a name="get-hello-iqn-of-a-windows-server-host"></a>Hello egy Windows Server-állomás IQN-Nevének lekérése
Hajtsa végre a hello a következő lépéseket tooget hello iSCSI minősített nevét (IQN) egy Windows Server 2012 rendszert futtató Windows-állomás.

[!INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Manuális biztonsági mentés létrehozása
Hajtsa végre a StorSimple eszköz hello Azure klasszikus portál toocreate igény szerinti manuális biztonsági mentés egyetlen kötetén lépések hello.

[!INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="next-steps"></a>Következő lépések
* [Virtuális eszköz](storsimple-virtual-device-u2.md) konfigurálása.
* Használjon hello [StorSimple Manager szolgáltatás](https://msdn.microsoft.com/library/azure/dn772396.aspx) toomanage a StorSimple eszközt.

