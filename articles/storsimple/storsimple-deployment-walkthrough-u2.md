---
title: "aaaDeploy a StorSimple eszköz (2. frissítés) |} Microsoft Docs"
description: "Hello lépéseket és a StorSimple 2. frissítés hello eszköz és a szolgáltatás központi telepítésének ajánlott eljárásai ismerteti."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 7dff0612-617b-4fc8-a3fe-994c24bc7c51
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: alkohli
ms.openlocfilehash: 5906cc3c41f03c426905ef91be37852608ae9393
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-on-premises-storsimple-device-update-2"></a>A helyszíni StorSimple eszköz (2. frissítés) üzembe helyezése
> [!div class="op_single_selector"]
> * [2. és azt követő frissítések ](storsimple-deployment-walkthrough-u2.md)
> * [1. frissítés](storsimple-deployment-walkthrough-u1.md)
> * [GA kiadás](storsimple-deployment-walkthrough.md)
> 
> 

## <a name="overview"></a>Áttekintés
Üdvözli a tooMicrosoft Azure StorSimple eszköztelepítő útmutatójában. Ezek az üzembehelyezési oktatóanyagok alkalmazása tooStorSimple 8000 Series Update 2. Ez az oktatóanyag-sorozat tartalmazza a StorSimple eszköz konfigurálásához szükséges ellenőrzőlistát, előfeltételeket és részletes lépéseket.

hello szereplő információk feltételezi, hogy áttekintése hello biztonsági óvintézkedéseket, és kicsomagolása, oktatóanyagban, és bekábelezte a StorSimple eszközt. Ha továbbra is szükséges tooperform azokat a feladatokat, kezdje hello megtekintésével [biztonsági óvintézkedéseket](storsimple-safety.md). Útmutatás alapján hello eszköz adott toounpack, állványra szerelése és bekábelezése az eszközt.

* [A StorSimple 8100 kicsomagolása, állványra szerelése és bekábelezése](storsimple-8100-hardware-installation.md)
* [A StorSimple 8600 kicsomagolása, állványra szerelése és bekábelezése](storsimple-8600-hardware-installation.md)

Szüksége lesz a rendszergazdai jogosultságokkal toocomplete hello beállítása és konfigurációja folyamat. Ajánlott áttekinteni hello konfigurációs ellenőrzőlista megkezdése előtt. hello üzembe helyezési és konfigurálási folyamat eltarthat néhány alkalommal toocomplete.

> [!NOTE]
> hello hello Microsoft Azure webhelyen közzétett StorSimple üzembehelyezési információk érvényes tooStorSimple 8000 sorozat eszközeire érvényesek. A 7000-es hello adatsorozat eszközökkel kapcsolatos részletes információkért lásd: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). A 7000-es sorozatra vonatkozó üzembehelyezési információkat lásd: hello [StorSimple rendszer gyors üzembehelyezési útmutatójában](http://onlinehelp.storsimple.com/111_Appliance/).
> 
> 

## <a name="deployment-steps"></a>A központi telepítés lépései
Hajtsa végre a szükséges lépéseket tooconfigure a StorSimple eszközt, és csatlakoztassa tooyour StorSimple Manager szolgáltatás. Ezenkívül toohello szükséges lépéseket, opcionális lépések és eljárások hello központi telepítése során szükség lehet. hello részletes üzembehelyezési utasítások jelzik, ha egyes opcionális lépéseket végre kell hajtania.

| Lépés | Leírás |
| --- | --- |
| **ELŐFELTÉTELEK** |Ezeknek kell toobe hello üzembe helyezésre felkészülés során. |
| [Üzembe helyezési konfigurációs ellenőrzőlista](#deployment-configuration-checklist) |A feladatlista toogather-rekord információk előzetes tooand hello telepítéskor használni. |
| [Üzembe helyezési előfeltételek](#deployment-prerequisites) |Ezek ellenőrzik, hogy hello környezet készen áll a központi telepítés. |
|  | |
| **RÉSZLETES ÜZEMBE HELYEZÉS** |Ezeket a lépéseket a StorSimple eszköz éles környezetben vannak a szükséges toodeploy. |
| [1. lépés: Új szolgáltatás létrehozása](#step-1-create-a-new-service) |A felhőfelügyelet és a felhőalapú tárolás beállítása a StorSimple eszközhöz. *Hagyja ki ezt a lépést, ha már rendelkezik meglévő szolgáltatással más StorSimple eszközökhöz*. |
| [2. lépés: Hello Szolgáltatásregisztrációs kulcs lekérése](#step-2-get-the-service-registration-key) |A kulcs tooregister használja a & csatlakoztathatja a StorSimple eszköz hello felügyeleti szolgáltatáshoz. |
| [3. lépés: Eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |Hello eszköz tooyour hálózat, és regisztrálhatja azt az Azure toocomplete hello beállítása hello management szolgáltatás. |
| [4. lépés: Minimális eszközbeállítások végrehajtása](#step-4-complete-minimum-device-setup)</br>[Választható lehetőség: A StorSimple eszköz frissítése](#scan-for-and-apply-updates) |Hello management toocomplete hello eszköz telepítése és tooprovide tárolási engedélyezheti azt. |
| [5. lépés: Kötettároló létrehozásar](#step-5-create-a-volume-container) |Hozzon létre egy tároló tooprovision köteteket. A kötettároló tárfiók, sávszélesség és a benne tárolt összes hello kötet titkosítási beállításait. |
| [6. lépés: Kötet létrehozása](#step-6-create-a-volume) |Tárkötet hello StorSimple eszközön a kiszolgálók számára. |
| [7. lépés: Kötet csatlakoztatása, inicializálása és formázása](#step-7-mount-initialize-and-format-a-volume)</br>[Választható lehetőség: Az MPIO konfigurálása](storsimple-configure-mpio-windows-server.md) |Csatlakoztassa a kiszolgálókat toohello hello eszköz által biztosított iSCSI-tárolóhoz. Választható lehetőségként konfigurálhatja az MPIO tooensure, hogy a a kiszolgálók képesek legyenek tűrni a kapcsolati, a hálózati és a kapcsolat hibája. |
| [8. lépés: Biztonsági mentés készítése](#step-8-take-a-backup) |Az adatok a biztonsági mentési házirend tooprotect beállítása |
|  | |
| **EGYÉB ELJÁRÁSOK** |Szükség lehet toorefer toothese eljárások a megoldás központi telepítésekor. |
| [Hello szolgáltatást egy új tárfiók konfigurálása](#configure-a-new-storage-account-for-the-service) | |
| [PuTTY tooconnect toohello eszköz soros konzoljához használata](#use-putty-to-connect-to-the-device-serial-console) | |
| [Hello egy Windows Server-állomás IQN-Nevének lekérése](#get-the-iqn-of-a-windows-server-host) | |
| [Manuális biztonsági mentés létrehozása](#create-a-manual-backup) | |

## <a name="deployment-configuration-checklist"></a>Üzembehelyezési konfigurációs ellenőrzőlista
Mielőtt az eszköz üzembe helyezése, szüksége lesz toocollect információk tooconfigure hello szoftver a StorSimple eszköz. Felkészülés az információk időben történő előkészítésével leegyszerűsíthető hello hello StorSimple eszköz környezetében telepítési folyamata. Töltse le és használja a feladatlista toonote le hello konfigurációs adatait az eszköz üzembe helyezése.

* [StorSimple üzembehelyezési konfigurációs ellenőrzőlista letöltése](http://www.microsoft.com/download/details.aspx?id=49159)

## <a name="deployment-prerequisites"></a>Üzembehelyezési előfeltételek
hello a következő szakaszok ismertetik hello konfigurációs előfeltételeit a StorSimple Manager szolgáltatás és a StorSimple eszközt.

### <a name="for-hello-storsimple-manager-service"></a>A StorSimple Manager szolgáltatás hello
Mielőtt hozzákezd, győződjön meg az alábbiakról:

* Rendelkezik Microsoft-fiókkal és a hozzá szükséges hozzáférési hitelesítő adatokkal.
* Rendelkezik Microsoft Azure Storage-fiókkal és a hozzá szükséges hozzáférési hitelesítő adatokkal.
* A Microsoft Azure-előfizetés hello StorSimple Manager szolgáltatás engedélyezve van. Az előfizetés hello keresztül kell megvásárolni [nagyvállalati szerződés](https://azure.microsoft.com/pricing/enterprise-agreement/).
* Hozzáférés tooterminal emuláció szoftvert, például a PuTTY rendelkezik.

### <a name="for-hello-device-in-hello-datacenter"></a>Hello adatközpontban hello eszköz
Hello eszköz konfigurálása előtt győződjön meg arról, hogy az eszköz teljes mértékben kicsomagolása, állványra van rögzítve és teljesen bekábelezte a tápellátáshoz, a hálózati és a soros hozzáféréshez leírtaknak megfelelően:

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

Hajtsa végre a következő lépéseket a felügyeleti portálon hello hello.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-hello-device-through-windows-powershell-for-storsimple"></a>3. lépés: Eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple
A Windows PowerShell használata a StorSimple toocomplete hello kezdeti telepítése a StorSimple eszköz eljárást követő hello leírtak szerint. Ezzel a lépéssel kell toouse terminálemulációs szoftver toocomplete. További információkért lásd: [PuTTY használata tooconnect toohello eszköz soros konzoljához](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>4. lépés: Minimális eszközbeállítások végrehajtása.
A StorSimple eszköz hello minimális eszköz konfigurálásához akkor szükségesek: 

* Hello másodlagos DNS-kiszolgáló beállítása.
* Engedélyezze az iSCSI-t legalább egy hálózati adapteren.
* Rendeljen rögzített IP-címek tooboth hello tartományvezérlők.

Hajtsa végre a következő lépéseket a hello kezelési portál toocomplete hello minimális eszközbeállítások hello.

[!INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>5. lépés: Kötettároló létrehozása
A kötettároló tárfiók, sávszélesség és a benne tárolt összes hello kötet titkosítási beállításait. A kötettároló toocreate kell ahhoz, hogy elkezdhessen köteteket a StorSimple eszköz. 

Hajtsa végre a következő lépéseket a hello kezelési portál toocreate kötettároló hello.

[!INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>6. lépés: Kötet létrehozása
A kötettároló létrehozása után a kiszolgálók oszthat hello StorSimple eszközön tárolókötetet. Hajtsa végre a hello hello kezelési portál toocreate kötet lépései.

> [!IMPORTANT]
> A StorSimple Manager dinamikusan és teljesen kiosztott köteteket is képes létrehozni. Azonban nem hozhat létre részben kiosztott köteteket. 
> 
> 

[!INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume-u2.md)]

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

Hajtsa végre a hello lépései hello kezelési portál toocreate ütemezett biztonsági mentés.

[!INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Manuális biztonsági mentést bármikor létrehozhat. Az eljárások, nyissa meg túl[manuális biztonsági mentés létrehozása](#create-a-manual-backup). 

## <a name="configure-a-new-storage-account-for-hello-service"></a>Hello szolgáltatást egy új tárfiók konfigurálása
Ez az egy opcionális lépés csak akkor, ha a szolgáltatással nem engedélyezte a tárfiók automatikus létrehozását hello tooperform kell. A Microsoft Azure storage-fiók szükséges toocreate StorSimple-kötettároló.

Ha Azure-tárfiók más régióban toocreate van szüksége, tekintse meg [kapcsolatos Azure Storage-fiókok](../storage/common/storage-create-storage-account.md) részletes útmutatásait.

Hajtsa végre a következő lépéseket a felügyeleti portálon hello a hello hello **StorSimple Manager szolgáltatás** lap.

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
Hajtsa végre a StorSimple eszköz hello kezelési portál toocreate igény szerinti manuális biztonsági mentés egyetlen kötetén lépések hello.

[!INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="next-steps"></a>Következő lépések
* [Virtuális eszköz](storsimple-virtual-device-u2.md) konfigurálása.
* Használjon hello [StorSimple Manager szolgáltatás](storsimple-manager-service-administration.md) toomanage a StorSimple eszközt.

