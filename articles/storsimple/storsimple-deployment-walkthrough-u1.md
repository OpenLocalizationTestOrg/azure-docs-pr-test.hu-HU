---
title: "aaaDeploy a StorSimple eszköz (1. frissítés) |} Microsoft Docs"
description: "Hello lépés és ajánlott eljárás hello 1. frissítés StorSimple eszköz és a szolgáltatás üzembe helyezésének ismerteti."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: ac631d3c-3c53-4c9e-9e4a-5c61c0cd8167
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 339b68f29a73bb77670e76e454cf271c7de4a6e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-on-premises-storsimple-device-update-1"></a>A helyszíni StorSimple eszköz (1. frissítés) üzembe helyezése
> [!div class="op_single_selector"]
> * [2. frissítés](storsimple-deployment-walkthrough-u2.md)
> * [1. frissítés](storsimple-deployment-walkthrough-u1.md)
> * [GA kiadás](storsimple-deployment-walkthrough.md)
> 
> 

## <a name="overview"></a>Áttekintés
Üdvözli a tooMicrosoft Azure StorSimple eszköztelepítő útmutatójában. Ezek az üzembehelyezési oktatóanyagok alkalmazása tooStorSimple 8000 Series 1.0-s frissítésében. Ez az oktatóanyag-sorozat ismerteti, hogyan tooconfigure a StorSimple eszközt, és tartalmazza a konfigurációs ellenőrzőlistát, konfigurációs előfeltételeket és részletes konfigurációs lépéseket.

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
hello a következő szakaszok ismertetik hello konfigurációs előfeltételeit a StorSimple Manager szolgáltatás és a StorSimple eszközt.

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

## <a name="step-by-step-deployment"></a>Részletes üzembe helyezés
Használja a következő lépésről toodeploy hello a StorSimple eszköz hello adatközpontban.

## <a name="step-1-create-a-new-service"></a>1. lépés: Új szolgáltatás létrehozása
A StorSimple Manager szolgáltatás több StorSimple eszközt is tud kezelni. Hajtsa végre a következő lépéseket toocreate hello StorSimple Manager szolgáltatás egy új példányát hello.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [!IMPORTANT]
> Ha nem engedélyezte a tárfiók automatikus létrehozását hello a szolgáltatással, szüksége lesz a toocreate legalább egy tárfiókot, miután sikeresen létrehozott egy szolgáltatást. Ezt a tárfiókot akkor fogja használni a rendszer, amikor egy kötettárolót hoz létre.
> 
> * Ha nem hozott létre egy tárfiókot automatikusan, nyissa meg túl[hello szolgáltatást egy új tárfiók konfigurálása](#configure-a-new-storage-account-for-the-service) részletes információkra van szüksége.
> * Ha engedélyezte a tárfiók automatikus létrehozását hello, nyissa meg túl[2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](#step-2-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-hello-service-registration-key"></a>2. lépés: Hello Szolgáltatásregisztrációs kulcs lekérése
Miután hello StorSimple Manager szolgáltatás fut, akkor tooget hello Szolgáltatásregisztrációs kulcs. Ez a kulcs használt tooregister, és csatlakoztathatja StorSimple eszközét hello szolgáltatásban.

Hajtsa végre a következő lépéseket a klasszikus Azure portálon hello hello.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-hello-device-through-windows-powershell-for-storsimple"></a>3. lépés: Eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple
A Windows PowerShell használata a StorSimple toocomplete hello kezdeti telepítése a StorSimple eszköz eljárást követő hello leírtak szerint. Ezzel a lépéssel kell toouse terminálemulációs szoftver toocomplete. További információkért lásd: [PuTTY használata tooconnect toohello eszköz soros konzoljához](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>4. lépés: Minimális eszközbeállítások végrehajtása.
A StorSimple eszköz hello minimális eszköz konfigurálásához akkor szükségesek:

* Hello másodlagos DNS-kiszolgáló beállítása.
* Engedélyezze az iSCSI-t legalább egy hálózati adapteren.
* Rendeljen rögzített IP-címek tooboth hello tartományvezérlők.

Hajtsa végre a következő lépéseket a hello Azure klasszikus portál toocomplete hello minimális eszközbeállítások hello.

[!INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>5. lépés: Kötettároló létrehozása
A kötettároló tárfiók, sávszélesség és a benne tárolt összes hello kötet titkosítási beállításait. A kötettároló toocreate kell ahhoz, hogy elkezdhessen köteteket a StorSimple eszköz.

Hajtsa végre a következő lépéseket a klasszikus portál Azure toocreate kötettároló hello hello.

[!INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>6. lépés: Kötet létrehozása
A kötettároló létrehozása után a kiszolgálók oszthat hello StorSimple eszközön tárolókötetet. Hajtsa végre a következő lépéseket az Azure klasszikus portál toocreate kötet hello hello.

> [!IMPORTANT]
> A StorSimple Manager csak dinamikusan kiosztott köteteket képes létrehozni. Nem hozhat létre teljesen vagy részben kiosztott köteteket.
> 
> 

[!INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>7. lépés: Kötet csatlakoztatása, inicializálása és formázása
a lépéseket követve hello készül a Windows Server-állomáson.

> [!IMPORTANT]
> * A hello magas rendelkezésre állás érdekében a StorSimple megoldás azt javasoljuk, hogy a gazdagép (nem kötelező) kiszolgálók előzetes tooconfiguring iSCSI MPIO konfigurálását. Az MPIO konfigurációja lévő gazdakiszolgálókra biztosítja, hogy hello kiszolgálók képesek legyenek tűrni a hivatkozásra, a hálózati vagy az adapterhibákat.
> * Az MPIO és az iSCSI telepítési és konfigurálási utasításait a Windows Server-állomáson, nyissa meg túl[MPIO konfigurálása a StorSimple eszköz](storsimple-configure-mpio-windows-server.md). Ezek közé tartozik a hello lépéseket toomount, inicializálása és is formázza a StorSimple-köteteket.
> * Az MPIO és az iSCSI telepítési és konfigurálási utasításait a Linux-gazdagépre, nyissa meg túl[a StorSimple Linux gazdagép MPIO konfigurálása](storsimple-configure-mpio-on-linux.md)
> 
> 

Ha döntse el, nem tooconfigure MPIO, hajtsa végre a következő lépéseket toomount hello, inicializálja és formázza a StorSimple-köteteket egy Windows Server-állomáson.

[!INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>8. lépés: Biztonsági mentés készítése
Az adott időpontban mentett biztonsági másolatok védelmet biztosítanak a kötetek számára, továbbá javítják a rendelkezésre álló helyreállítási lehetőségeket, miközben a helyreállítási időt csökkentik. A StorSimple eszközén kétféle biztonsági mentést készíthet: helyi pillanatképeket és felhőbeli pillanatképeket. Mind a kétféle biztonsági mentés lehet **Ütemezett** vagy **Manuális**.

Hajtsa végre a következő lépéseket az Azure klasszikus portál toocreate ütemezett biztonsági mentés hello hello.

[!INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Manuális biztonsági mentést bármikor létrehozhat. Az eljárások, nyissa meg túl[manuális biztonsági mentés létrehozása](#create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-hello-service"></a>Hello szolgáltatást egy új tárfiók konfigurálása
Ez az egy opcionális lépés csak akkor, ha a szolgáltatással nem engedélyezte a tárfiók automatikus létrehozását hello tooperform kell. A Microsoft Azure storage-fiók szükséges toocreate StorSimple-kötettároló.

Ha Azure-tárfiók más régióban toocreate van szüksége, tekintse meg [kapcsolatos Azure Storage-fiókok](../storage/common/storage-create-storage-account.md) részletes útmutatásait.

Hajtsa végre a következő lépéseket a klasszikus Azure portálon, a hello hello hello **StorSimple Manager szolgáltatás** lap.

[!INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]

## <a name="use-putty-tooconnect-toohello-device-serial-console"></a>PuTTY tooconnect toohello eszköz soros konzoljához használata
tooconnect tooWindows PowerShell-lel, mint a PuTTY toouse terminálemulációs szoftverre kell. A PuTTY segítségével használhatja, ha hello eszközhöz való hozzáféréshez hello soros konzolon keresztül közvetlenül vagy egy telnet-munkamenet távoli számítógépről történő megnyitásával.

[!INCLUDE [Use PuTTY tooconnect toohello device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Frissítések keresése és telepítése
Az eszköz frissítése több órát is igénybe vehet. Hajtsa végre a következő lépéseket tooscan a hello, és alkalmazza a frissítéseket az eszközön.
<!--can take 1-4 hours-->

<!--If you have a gateway configured on a network interface other than Data 0, you will need toodisable Data 2 and Data 3 network interfaces before installing hello update. Go too**Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after hello device is updated.-->

#### <a name="tooupdate-your-device"></a>tooupdate az eszköz
1. Hello eszközön **gyors üzembe helyezés** kattintson **eszközök**. Jelölje ki azt a fizikai eszköz hello **karbantartási** majd **frissítések keresése**.  
2. Létrejön egy feladat tooscan az elérhető frissítések. Ha frissítések érhetők el, hello **frissítések keresése** túl változik**frissítések telepítése**. Kattintson a **Frissítések telepítése** elemre.
3. A rendszer létrehoz egy frissítési feladatot. A frissítés hello állapotának figyelése túl navigálva**feladatok**.
   
   > [!NOTE]
   > Hello frissítési feladat elindulásakor azonnal megjeleníti hello állapotát 50 % lesz. hello állapota too100 százalék csak hello frissítési feladat befejeződése után. Nincs hello frissítési folyamat nem valós idejű állapotát.
   > 
   > 
4. Hello eszköz sikeres frissítése után engedélyezze Data 2 és a Data 3 hálózati adaptereket, ha ezek le voltak tiltva.

<!-- In step 2, you may be requested toodisable Data 2 and Data 3 prior tooinstalling hello updates. You must disable these network interfaces or hello updates may fail.-->

## <a name="get-hello-iqn-of-a-windows-server-host"></a>Hello egy Windows Server-állomás IQN-Nevének lekérése
Hajtsa végre a hello a következő lépéseket tooget hello iSCSI minősített nevét (IQN) egy Windows Server® 2012 rendszert futtató Windows-állomás.

[!INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Manuális biztonsági mentés létrehozása
Hajtsa végre a StorSimple eszköz hello Azure klasszikus portál toocreate igény szerinti manuális biztonsági mentés egyetlen kötetén lépések hello.

[!INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="configure-mpio"></a>Az MPIO konfigurálása
A többutas I/O (MPIO) egy opcionális szolgáltatás, és alapesetben nincs telepítve a Windows Serveren. Szolgáltatásként lehet telepíteni a Kiszolgálókezelőn keresztül. Az MPIO telepítési utasításait, nyissa meg túl[MPIO konfigurálása a StorSimple eszköz](storsimple-configure-mpio-windows-server.md).

Az MPIO telepítési utasításait a StorSimple eszköz a Linux-állomáshoz csatlakoztatott tooa nyissa meg túl[MPIO konfigurálása a Linux-állomáshoz](storsimple-configure-mpio-on-linux.md).

> [!NOTE]
> StorSimple virtuális eszközökön az MPIO használata nem támogatott.
> 
> 

## <a name="next-steps"></a>Következő lépések
* [Virtuális eszköz](storsimple-virtual-device-u2.md) konfigurálása.
* Használjon hello [StorSimple Manager szolgáltatás](storsimple-manager-service-administration.md) toomanage a StorSimple eszközt.

