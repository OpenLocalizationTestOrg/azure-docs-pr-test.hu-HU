---
title: "aaaDeploy a StorSimple 8000 series eszköz az Azure portálon |} Microsoft Docs"
description: "Hello lépéseket és Update 3 és újabb verziók, és a StorSimple Device Manager szolgáltatást futtató hello StorSimple 8000 series eszköz központi telepítésének ajánlott eljárásai ismerteti."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 28d32d6c0d37169902d9db91701deee4fd99dc4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-on-premises-storsimple-device-update-3-and-later"></a>A helyszíni StorSimple-eszköz (3. frissítés vagy újabb) üzembe helyezése

## <a name="overview"></a>Áttekintés
Üdvözli a tooMicrosoft Azure StorSimple eszköztelepítő útmutatójában. Ezek az üzembehelyezési oktatóanyagok alkalmazása tooStorSimple 8000 Series Update 3-as vagy újabb. Ez az oktatóanyag-sorozat tartalmazza a StorSimple eszköz konfigurálásához szükséges ellenőrzőlistát, előfeltételeket és részletes lépéseket.

hello szereplő információk feltételezi, hogy áttekintése hello biztonsági óvintézkedéseket, és kicsomagolása, oktatóanyagban, és bekábelezte a StorSimple eszközt. Ha továbbra is szükséges tooperform azokat a feladatokat, kezdje hello megtekintésével [biztonsági óvintézkedéseket](storsimple-8000-safety.md). Kövesse a hello eszközspecifikus utasításokat toounpack, állványra szerelése és bekábelezése az eszközt.

* [A StorSimple 8100 kicsomagolása, állványra szerelése és bekábelezése](storsimple-8100-hardware-installation.md)
* [A StorSimple 8600 kicsomagolása, állványra szerelése és bekábelezése](storsimple-8600-hardware-installation.md)

Rendszergazdai jogosultságok toocomplete hello beállítása és konfigurációja folyamat van szüksége. Ajánlott áttekinteni hello konfigurációs ellenőrzőlista megkezdése előtt. hello üzembe helyezési és konfigurálási folyamat eltarthat néhány alkalommal toocomplete.

> [!NOTE]
> hello hello Microsoft Azure webhelyen közzétett StorSimple üzembehelyezési információk érvényes tooStorSimple 8000 sorozat eszközeire érvényesek. A 7000-es hello adatsorozat eszközökkel kapcsolatos részletes információkért lásd: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). A 7000-es sorozatra vonatkozó üzembehelyezési információkat lásd: hello [StorSimple rendszer gyors üzembehelyezési útmutatójában](http://onlinehelp.storsimple.com/111_Appliance/). 


## <a name="deployment-steps"></a>A központi telepítés lépései
Hajtsa végre a szükséges lépéseket tooconfigure a StorSimple eszközt, és csatlakoztassa tooyour StorSimple Device Manager szolgáltatást. Ezenkívül toohello szükséges lépéseket, opcionális lépések és eljárások hello központi telepítése során szükség lehet. hello részletes üzembehelyezési utasítások jelzik, ha egyes opcionális lépéseket végre kell hajtania.

| Lépés | Leírás |
| --- | --- |
| **ELŐFELTÉTELEK** |Ezek az üzembe helyezésre hello előkészítésekor kell végezni. |
| [Üzembe helyezési konfigurációs ellenőrzőlista](#deployment-configuration-checklist) |A feladatlista toogather-rekord információkat használhatja előtt és hello üzembe helyezése során. |
| [Üzembe helyezési előfeltételek](#deployment-prerequisites) |Ezek ellenőrzik, hogy hello környezet készen áll a központi telepítés. |
|  | |
| **RÉSZLETES ÜZEMBE HELYEZÉS** |Ezeket a lépéseket a StorSimple eszköz éles környezetben vannak a szükséges toodeploy. |
| [1. lépés: Új szolgáltatás létrehozása](#step-1-create-a-new-service) |A felhőfelügyelet és a felhőalapú tárolás beállítása a StorSimple-eszközhöz. *Hagyja ki ezt a lépést, ha már rendelkezik meglévő szolgáltatással más StorSimple eszközökhöz*. |
| [2. lépés: Hello Szolgáltatásregisztrációs kulcs lekérése](#step-2-get-the-service-registration-key) |A kulcs tooregister használja a & csatlakoztathatja a StorSimple eszköz hello felügyeleti szolgáltatáshoz. |
| [3. lépés: Eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |toocomplete hello hello management szolgáltatás beállítása, hello eszköz tooyour hálózat, és regisztrálja az Azure. |
| [4. lépés: Minimális eszközbeállítások végrehajtása](#step-4-complete-minimum-device-setup)</br>[Választható lehetőség: A StorSimple eszköz frissítése](#scan-for-and-apply-updates) |Hello management toocomplete hello eszköz telepítése és tooprovide tárolási engedélyezheti azt. |
| [5. lépés: Kötettároló létrehozásar](#step-5-create-a-volume-container) |Hozzon létre egy tároló tooprovision köteteket. A kötettároló tárfiók, sávszélesség és a benne tárolt összes hello kötet titkosítási beállításait. |
| [6. lépés: Kötet létrehozása](#step-6-create-a-volume) |Tároló kötetek kiépítéséhez hello StorSimple eszközön a kiszolgálók számára. |
| [7. lépés: Kötet csatlakoztatása, inicializálása és formázása](#step-7-mount-initialize-and-format-a-volume)</br>[Választható lehetőség: Az MPIO konfigurálása](storsimple-8000-configure-mpio-windows-server.md) |Csatlakoztassa a kiszolgálókat toohello hello eszköz által biztosított iSCSI-tárolóhoz. Választható lehetőségként konfigurálhatja az MPIO tooensure, hogy a kiszolgálók képesek legyenek tűrni kapcsolati, a hálózati és az adapterhibákat. |
| [8. lépés: Biztonsági mentés készítése](#step-8-take-a-backup) |Az adatok a biztonsági mentési házirend tooprotect beállítása |
|  | |
| **EGYÉB ELJÁRÁSOK** |Szükség lehet toorefer toothese eljárások a megoldás központi telepítésekor. |
| [Hello szolgáltatást egy új tárfiók konfigurálása](#configure-a-new-storage-account-for-the-service) | |
| [PuTTY tooconnect toohello eszköz soros konzoljához használata](#use-putty-to-connect-to-the-device-serial-console) | |
| [Hello egy Windows Server-állomás IQN-Nevének lekérése](#get-the-iqn-of-a-windows-server-host) | |
| [Manuális biztonsági mentés létrehozása](#create-a-manual-backup) | |


## <a name="deployment-configuration-checklist"></a>Üzembehelyezési konfigurációs ellenőrzőlista
Az eszköz üzembe helyezése, meg kell toocollect információk tooconfigure hello szoftver a StorSimple eszköz. Érdekében hello folyamat hello StorSimple eszközt az adott környezetben történő telepítésének előkészítése előre idő segít az adatok egy részét. Töltse le és használja a feladatlista toonote le hello konfigurációs adatait az eszköz üzembe helyezése.

* [StorSimple üzembehelyezési konfigurációs ellenőrzőlista letöltése](http://www.microsoft.com/download/details.aspx?id=49159)

## <a name="deployment-prerequisites"></a>Üzembehelyezési előfeltételek
hello a következő szakaszok ismertetik hello konfigurációs előfeltételeit a StorSimple eszköz Manager szolgáltatás és a StorSimple eszközt.

### <a name="for-hello-storsimple-device-manager-service"></a>A StorSimple eszköz Manager szolgáltatás hello
Mielőtt hozzákezd, győződjön meg az alábbiakról:

* Rendelkezik Microsoft-fiókkal és a hozzá szükséges hozzáférési hitelesítő adatokkal.
* Rendelkezik Microsoft Azure Storage-fiókkal és a hozzá szükséges hozzáférési hitelesítő adatokkal.
* A Microsoft Azure-előfizetés hello StorSimple Device Manager szolgáltatás engedélyezve van. Az előfizetés hello keresztül kell megvásárolni [nagyvállalati szerződés](https://azure.microsoft.com/pricing/enterprise-agreement/).
* Hozzáférés tooterminal emuláció szoftvert, például a PuTTY rendelkezik.

### <a name="for-hello-device-in-hello-datacenter"></a>Hello adatközpontban hello eszköz
Hello eszköz konfigurálása előtt győződjön meg arról, hogy az eszköz teljes mértékben kicsomagolása, állványra van rögzítve és teljesen bekábelezte a tápellátáshoz, a hálózati és a soros hozzáféréshez leírtaknak megfelelően:

* [A StorSimple 8100 kicsomagolása, állványra szerelése és bekábelezése](storsimple-8100-hardware-installation.md)
* [A StorSimple 8600 kicsomagolása, állványra szerelése és bekábelezése](storsimple-8600-hardware-installation.md)

### <a name="for-hello-network-in-hello-datacenter"></a>Hello adatközpontban hello hálózat
Mielőtt hozzákezd, győződjön meg az alábbiakról:

* hello az Adatközpont tűzfalának portjait iSCSI és a felhőalapú forgalom megnyitott tooallow leírtak [a StorSimple eszköz hálózatkezelési követelményei](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device).

## <a name="step-by-step-deployment"></a>Részletes üzembe helyezés
Használja a következő lépésről toodeploy hello a StorSimple eszköz hello adatközpontban.

## <a name="step-1-create-a-new-service"></a>1. lépés: Új szolgáltatás létrehozása
A StorSimple-eszközkezelő szolgáltatás több StorSimple-eszközt is tud kezelni. Hajtsa végre a következő lépéseket toocreate hello StorSimple Device Manager szolgáltatás egy példánya hello.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]

> [!IMPORTANT]
> Ha nem engedélyezte a tárfiók automatikus létrehozását hello a szolgáltatással, szüksége lesz a toocreate legalább egy tárfiókot, miután sikeresen létrehozott egy szolgáltatást. Ezt a tárfiókot akkor használja a rendszer, amikor egy kötettárolót hoz létre.
>
> * Ha nem hozott létre egy tárfiókot automatikusan, nyissa meg túl[hello szolgáltatást egy új tárfiók konfigurálása](#configure-a-new-storage-account-for-the-service) részletes információkra van szüksége.
> * Ha engedélyezte a tárfiók automatikus létrehozását hello, nyissa meg túl[2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](#step-2-get-the-service-registration-key).


## <a name="step-2-get-hello-service-registration-key"></a>2. lépés: Hello Szolgáltatásregisztrációs kulcs lekérése
Miután hello StorSimple Device Manager szolgáltatás fut, akkor tooget hello Szolgáltatásregisztrációs kulcs. Ez a kulcs használt tooregister, és csatlakoztathatja StorSimple eszközét hello szolgáltatásban.

Hajtsa végre a következő lépéseket az Azure-portálon hello hello.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-hello-device-through-windows-powershell-for-storsimple"></a>3. lépés: Eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple
A Windows PowerShell használata a StorSimple toocomplete hello kezdeti telepítése a StorSimple eszköz eljárást követő hello leírtak szerint. Toouse terminálemulációs szoftver toocomplete kell ezt a lépést. További információkért lásd: [PuTTY használata tooconnect toohello eszköz soros konzoljához](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-8000-configure-and-register-device-u2](../../includes/storsimple-8000-configure-and-register-device-u2.md)]

## <a name="step-4-complete-minimum-device-setup"></a>4. lépés: Minimális eszközbeállítások végrehajtása.
A StorSimple eszköz hello minimális eszköz konfigurálásához akkor szükségesek: 

* Adjon egy valódi (értelmezhető) nevet az eszköznek.
* Hello eszköz időzóna beállítása.
* Rendeljen rögzített IP-címek tooboth hello tartományvezérlők.

Hajtsa végre a következő lépéseket a hello Azure portál toocomplete hello minimális eszközbeállítások hello.

[!INCLUDE [storsimple-8000-complete-minimum-device-setup-u2](../../includes/storsimple-8000-complete-minimum-device-setup-u2.md)]

## <a name="step-5-create-a-volume-container"></a>5. lépés: Kötettároló létrehozása
A kötettároló tárfiók, sávszélesség és a benne tárolt összes hello kötet titkosítási beállításait. A kötettároló toocreate kell ahhoz, hogy elkezdhessen köteteket a StorSimple eszköz.

Hajtsa végre a következő lépéseket az Azure portál toocreate kötettároló hello hello.

[!INCLUDE [storsimple-8000-create-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>6. lépés: Kötet létrehozása
A kötettároló létrehozása után a kiszolgálók oszthat hello StorSimple eszközön tárolókötetet. Hajtsa végre a következő lépéseket az Azure portál toocreate kötet hello hello.

> [!IMPORTANT]
> A StorSimple-eszközkezelő dinamikusan és teljesen kiosztott köteteket is képes létrehozni. Azonban nem hozhat létre részben kiosztott köteteket.


[!INCLUDE [storsimple-8000-create-volume](../../includes/storsimple-8000-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>7. lépés: Kötet csatlakoztatása, inicializálása és formázása
a lépéseket követve hello készül a Windows Server-állomáson.

> [!IMPORTANT]
> * A hello magas rendelkezésre állás érdekében a StorSimple megoldás azt javasoljuk, hogy a gazdagép (nem kötelező) kiszolgálók előzetes tooconfiguring iSCSI MPIO konfigurálását. Az MPIO konfigurációja lévő gazdakiszolgálókra biztosítja, hogy hello kiszolgálók képesek legyenek tűrni a hivatkozásra, a hálózati vagy az adapterhibákat.
> * Az MPIO és az iSCSI telepítési és konfigurálási utasításait a Windows Server-állomáson, nyissa meg túl[MPIO konfigurálása a StorSimple eszköz](storsimple-8000-configure-mpio-windows-server.md). Ide tartoznak hello lépéseket toomount, inicializálja és formázza a StorSimple-köteteket.
> * Az MPIO és az iSCSI telepítési és konfigurálási utasításait a Linux-gazdagépre, nyissa meg túl[a StorSimple Linux gazdagép MPIO konfigurálása](storsimple-configure-mpio-on-linux.md)

Ha döntse el, nem tooconfigure MPIO, hajtsa végre a következő lépéseket toomount hello, inicializálja és formázza a StorSimple-köteteket egy Windows Server-állomáson.

[!INCLUDE [storsimple-8000-mount-initialize-format-volume](../../includes/storsimple-8000-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>8. lépés: Biztonsági mentés készítése
Az adott időpontban mentett biztonsági másolatok védelmet biztosítanak a kötetek számára, továbbá javítják a rendelkezésre álló helyreállítási lehetőségeket, miközben a helyreállítási időt csökkentik. A StorSimple eszközén kétféle biztonsági mentést készíthet: helyi pillanatképeket és felhőbeli pillanatképeket. Mind a kétféle biztonsági mentés lehet **Ütemezett** vagy **Manuális**.

Hajtsa végre a következő lépéseket az Azure portál toocreate ütemezett biztonsági mentés hello hello.

[!INCLUDE [storsimple-8000-take-backup](../../includes/storsimple-8000-take-backup.md)]

Manuális biztonsági mentést bármikor létrehozhat. Az eljárások, nyissa meg túl[manuális biztonsági mentés létrehozása](#create-a-manual-backup).

Hello eszközök konfigurálása befejeződött.

## <a name="configure-a-new-storage-account-for-hello-service"></a>Hello szolgáltatást egy új tárfiók konfigurálása
Ez az egy opcionális lépés csak akkor, ha a szolgáltatással nem engedélyezte a tárfiók automatikus létrehozását hello tooperform kell. A Microsoft Azure storage-fiók szükséges toocreate StorSimple-kötettároló.

Ha Azure-tárfiók más régióban toocreate van szüksége, tekintse meg [kapcsolatos Azure Storage-fiókok](../storage/common/storage-create-storage-account.md) részletes útmutatásait.

Hajtsa végre a következő lépéseket az Azure portál, a hello hello hello **StorSimple Device Manager szolgáltatás** lap.

[!INCLUDE [storsimple-8000-configure-new-storage-account-u2](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

## <a name="use-putty-tooconnect-toohello-device-serial-console"></a>PuTTY tooconnect toohello eszköz soros konzoljához használata
tooconnect tooWindows PowerShell-lel, mint a PuTTY toouse terminálemulációs szoftverre kell. A PuTTY segítségével használhatja, ha hello eszközhöz való hozzáféréshez hello soros konzolon keresztül közvetlenül vagy egy telnet-munkamenet távoli számítógépről történő megnyitásával.

[!INCLUDE [Use PuTTY tooconnect toohello device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Frissítések keresése és telepítése
Az eszköz frissítése több órát is igénybe vehet. A részletes lépéseket, hogyan tooinstall hello legújabb frissítés, a go túl[Update 4 telepítése](storsimple-8000-install-update-4.md).


## <a name="get-hello-iqn-of-a-windows-server-host"></a>Hello egy Windows Server-állomás IQN-Nevének lekérése
Hajtsa végre a hello a következő lépéseket tooget hello iSCSI minősített nevét (IQN) egy Windows Server® 2012 rendszert futtató Windows-állomás.

[!INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Manuális biztonsági mentés létrehozása
Hajtsa végre a StorSimple eszköz hello Azure portál toocreate igény szerinti manuális biztonsági mentés egyetlen kötetén lépések hello.

[!INCLUDE [Create a manual backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Következő lépések
* [A StorSimple Cloud Appliance konfigurálása](storsimple-8000-cloud-appliance-u2.md).
* [Használjon hello StorSimple Device Manager szolgáltatás toomanage a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

