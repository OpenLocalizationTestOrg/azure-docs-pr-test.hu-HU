---
title: "virtuális eszköz aaaStorSimple 2. frissítés |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate, telepítéséhez és felügyeletéhez a StorSimple virtuális eszköz egy Microsoft Azure virtuális hálózatban. (2. frissítés tooStorSimple vonatkozik)."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: f37752a5-cd0c-479b-bef2-ac2c724bcc37
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: alkohli
ms.openlocfilehash: 8d2a0520f1ed30ebec929c2bdabb4838691b8ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>A StorSimple virtuális eszköz üzembe helyezése és kezelése az Azure-ban
## <a name="overview"></a>Áttekintés
hello a StorSimple 8000 sorozatú virtuális eszköz a Microsoft Azure StorSimple megoldáshoz egy további lehetőség. hello StorSimple virtuális eszköz egy Microsoft Azure virtuális hálózatot a virtuális gépen fut, és használhatja fel tooback és a Klónozás adatokat a gazdagépekről. Ez az oktatóanyag leírja, hogyan toodeploy és az Azure-ban a virtuális eszköz felügyelete, és megfelelő tooall hello frissítési szoftververziót futtató, 2 és alsó virtuális eszközök.

#### <a name="virtual-device-model-comparison"></a>Virtuáliseszköz-modellek összehasonlítása
hello StorSimple virtuális eszköz rendelkezésre áll a két modell egy hagyományos 8010-es (korábbi nevén hello 1100) és a prémium 8020-as modell (a 2. frissítés verzióban jelent). Hello két modell összehasonlítása az alábbi táblázatban.

| Eszközmodell | 8010<sup>1</sup> | 8020 |
| --- | --- | --- |
| **Maximális kapacitás** |30 TB |64 TB |
| **Azure virtuális gép** |Standard_A3 (4 mag, 7 GB memória) |Standard_DS3 (4 mag, 14 GB memória) |
| **Verziók kompatibilitása** |A 2. frissítés előzetes verzióját vagy újabb verziókat futtató verziók |A 2. frissítést vagy újabb verziókat futtató verziók |
| **Régiónkénti elérhetőség** |Minden Azure-régió |A Premium Storage-ot és a DS3 csomagú Azure-beli virtuális gépeket támogató összes Azure-régió<br></br> Használjon [ebben a listában](https://azure.microsoft.com/en-us/regions/services) toosee, ha mindkét *virtuális gépek > DS sorozatnak* és *tárolási > tároló lemez* érhetők el az adott régióban. |
| **Tárolás típusa** |A helyi lemezeken Azure Standard szintű tárolást használ<br></br> Ismerje meg, hogyan túl[szabványos Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md) |A helyi lemezeken Azure Premium szintű tárolást használ<sup>2</sup> <br></br>Ismerje meg, hogyan túl[prémium szintű Storage-fiók létrehozása](../storage/common/storage-premium-storage.md) |
| **Útmutató a számítási feladatokhoz** |A fájlok elemszintű lekérése a biztonsági másolatokból |Felhőalapú fejlesztési és tesztelési forgatókönyvek, kis késés, nagyobb teljesítményű számítási feladatok <br></br>Másodlagos vészhelyreállítási eszköz |

<sup>1</sup> *nevén hello 1100*.

<sup>2</sup> *mindkét hello 8010-es és a 8020-as modellt használja. Ha Azure standard szintű tárolást hello felhő réteg hello különbség csak hello helyi rétegen belüli hello eszköz szerepel*.

Ez a cikk ismerteti az Azure-ban a StorSimple virtuális eszköz telepítési részletes folyamata hello. A cikk elolvasása után:

* Ismerje meg, hogyan különbözik hello virtuális eszköz hello fizikai eszköztől.
* Képes toocreate és hello virtuális eszköz konfigurálása.
* Csatlakoztassa a virtuális eszköz toohello.
* Megtudhatja, hogyan toowork hello virtuális eszközzel.

Ez az oktatóanyag tooall hello StorSimple virtuális eszközökön futó Update 2 és újabb rendszer vonatkozik.

## <a name="how-hello-virtual-device-differs-from-hello-physical-device"></a>Hogyan különbözik hello virtuális eszköz hello fizikai eszköz
hello StorSimple virtuális eszköz egy csak szoftveres StorSimple, amely a Microsoft Azure virtuális gép egyetlen csomópontján fut verziója. hello virtuális eszköz támogatja a vész-helyreállítási eljárással, amelyben a fizikai eszköz nem érhető el, és alkalmas a elemszintű lekérése a biztonsági mentésekből, helyszíni vészhelyreállításhoz, és a felhőalapú fejlesztési és tesztelési forgatókönyvek.

#### <a name="differences-from-hello-physical-device"></a>Eltérések a hello fizikai eszköz
hello következő táblázatban hello StorSimple virtuális eszköz és hello StorSimple fizikai eszköz közötti fontosabb különbségeket.

|  | Fizikai eszköz | Virtuális eszköz |
| --- | --- | --- |
| **Hely** |Hello adatközpontban található. |Az Azure-ban fut. |
| **Hálózati illesztők** |Hat hálózati adapterrel rendelkezik: DATA 0-tól DATA 5 számozásig. |Csak egy hálózati adapterrel rendelkezik: DATA 0. |
| **Regisztráció** |Regisztrált hello konfigurációs lépés során. |A regisztráció egy külön feladat. |
| **Szolgáltatásadat-titkosítási kulcs** |Újragenerálja hello fizikai eszközön, és frissítse a virtuális eszköz hello hello új kulccsal. |Hello virtuális eszközről nem tud újra létrejönni. |

## <a name="prerequisites-for-hello-virtual-device"></a>Hello virtuális eszköz előfeltételei
hello alábbi szakaszok ismertetik a StorSimple virtuális eszköz hello konfigurációs előfeltételeit. Előzetes toodeploying egy virtuális eszközt, tekintse át a hello [a virtuális eszközök használatának biztonsági szempontjait](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Azure-követelmények
Hello virtuális eszköz kiépítése előtt a következő műveleteket az Azure környezetben toomake hello szüksége:

* Hello virtuális eszköz [Azure virtuális hálózat konfigurálása](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Premium szintű Storage használata esetén a Premium szintű Storage-ot támogató Azure-régióban kell létrehoznia egy virtuális hálózatot. hello prémium szintű storage-régiók, amelyek megfelelnek a toohello sort régiók *tároló lemez* hello listájában [Azure-szolgáltatások régiónként](https://azure.microsoft.com/en-us/regions/services).
* Ajánlott toouse hello alapértelmezett DNS-kiszolgáló a saját DNS-kiszolgálónév megadása helyett az Azure által biztosított. Ha a DNS-kiszolgáló neve nem érvényes, vagy ha hello DNS-kiszolgáló nem tudja tooresolve IP-címek megfelelően, hello hello virtuális eszköz létrehozása sikertelen lesz.
* A végpont és telephely közötti, valamint a telephely és telephely közötti lehetőségek választhatóak, de nem kötelezőek. Igény szerint ezeket a lehetőségeket speciális forgatókönyvekhez is konfigurálhatja.
* Létrehozhat [Azure virtuális gépek](../virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (gazdakiszolgálókat), amely a virtuális eszköz hello által elérhetővé tett hello kötetek hello a virtuális hálózatban. Ezek a kiszolgálók hello követelményeknek kell megfelelniük:                             

  * Windows vagy Linux rendszerű virtuális gépnek kell lenniük, amelyen telepítve van az iSCSI-kezdeményező szoftver.
  * Hello futni hello virtuális eszköz megegyező virtuális hálózatban.
  * Képes tooconnect toohello iSCSI-tároló hello virtuális eszköz belső IP-címe hello hello virtuális eszközön keresztül kell.
* Ellenőrizze, hogy konfigurálta támogatását, az iSCSI és a felhőalapú forgalom hello az azonos virtuális hálózaton.

#### <a name="storsimple-requirements"></a>A StorSimple követelményei
Ellenőrizze a virtuális eszköz létrehozása előtt a következő frissítések tooyour Azure StorSimple szolgáltatás hello:

* Adja hozzá [hozzáférés-vezérlési rekordokat](storsimple-manage-acrs.md) hello virtuális gépeket, hogy a virtuális eszköz toobe kiszolgálók számára.
* Használja a [tárfiók](storsimple-manage-storage-accounts.md#add-a-storage-account) a hello hello virtuális eszköz és ugyanabban a régióban. Különböző régiókban lévő tárfiókok használata esetén a teljesítmény gyenge lehet. Egy Standard vagy prémium szintű Storage-fiók hello virtuális eszköz használható. További információt toocreate egy [standard szintű tárfiók](../storage/common/storage-create-storage-account.md) vagy egy [prémium szintű Storage-fiók](../storage/common/storage-premium-storage.md)
* Egy másik tárolási fiókot használni a hello egy adatait a virtuális eszköz létrehozásához. Ugyanazt a tárfiókot teljesítménycsökkenést eredményezhet hello segítségével.

Győződjön meg arról, hogy rendelkezik-e megkezdése előtt információkat a következő hello:

* A klasszikus Azure portálbeli fiókja és a hozzáférési hitelesítő adatai.
* Hello szolgáltatásadat-titkosítási kulcs a fizikai eszközről egy példányát.

## <a name="create-and-configure-hello-virtual-device"></a>Hozza létre és konfigurálja a virtuális eszköz hello
Az eljárások végrehajtása előtt győződjön meg arról, hogy teljesül-e az hello [hello virtuális eszköz előfeltételei](#prerequisites-for-the-virtual-device).

Miután létrehozott egy virtuális hálózatot, konfigurálta a StorSimple Manager szolgáltatást, és a fizikai StorSimple eszköz hello szolgáltatásban regisztrált, a következő lépéseket toocreate hello használja, és a StorSimple virtuális eszköz konfigurálása.

### <a name="step-1-create-a-virtual-device"></a>1. lépés: Virtuális eszköz létrehozása
Hajtsa végre a következő lépéseket toocreate hello StorSimple virtuális eszköz hello.

[!INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Ebben a lépésben hello hello virtuális eszköz létrehozása sikertelen lesz, ha nincs kapcsolat toohello Internet. További információ: túl[Internet csatlakozási hibák elhárítása](#troubleshoot-internet-connectivity-errors) a virtuális eszköz létrehozása során.

### <a name="step-2-configure-and-register-hello-virtual-device"></a>2. lépés: Konfigurálása és regisztrálása hello virtuális eszköz
Ez az eljárás megkezdése előtt győződjön meg arról, hogy rendelkezik-e hello szolgáltatásadat-titkosítási kulcs másolatával. hello szolgáltatásadat-titkosítási kulcs jött létre, amikor az első StorSimple eszköz konfigurálásakor és Ön utasításai toosave azt egy biztonságos helyre. Ha nem rendelkezik hello szolgáltatásadat-titkosítási kulcs biztonsági másolatát, segítségért lépjen kapcsolatba Microsoft Support.

Hajtsa végre a következő lépéseket tooconfigure hello, és a StorSimple virtuális eszköz regisztrálása.

[!INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-hello-device-configuration-settings"></a>3. lépés: Konfiguráció (nem kötelező) módosítás hello eszközbeállítások
hello alábbi szakasz ismerteti eszközkonfigurációs beállítások hello hello StorSimple virtuális eszköz szükséges, ha szeretné, hogy a StorSimple Snapshot Manager CHAP, toouse vagy hello eszköz-rendszergazdai jelszó módosítása.

#### <a name="configure-hello-chap-initiator"></a>Hello CHAP-kezdeményező konfigurálása
Ez a paraméter tartalmazza a hello hitelesítő adatokat, amelyek a virtuális eszköz (cél), melyek tooaccess hello kötetek hello kezdeményezőktől (kiszolgálóktól) vár. hello kezdeményezők egy CHAP-felhasználónév és egy CHAP-jelszó tooidentify maguk tooyour eszköz a hitelesítés során. A részletes lépéseket lásd túl[CHAP konfigurálása az eszközhöz](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-hello-chap-target"></a>Hello CHAP céljának konfigurálása
Ez a paraméter tartalmazza a hello hitelesítő adatokat, amelyek a virtuális eszközt használ, ha egy CHAP-támogatással rendelkező kezdeményező kölcsönös vagy kétirányú hitelesítést kér. A virtuális eszköz használandó felhasználónevet fordított irányú CHAP és fordított CHAP-jelszó tooidentify maga toohello kezdeményező a hitelesítési folyamat során. Vegye figyelembe, hogy a CHAP-cél beállításai globálisak. Ha alkalmazza ezeket a beállításokat, minden hello kötetek csatlakoztatott toohello virtuális tárolóeszköz használja a CHAP-hitelesítés. A részletes lépéseket lásd túl[CHAP konfigurálása az eszközhöz](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-hello-storsimple-snapshot-manager-password"></a>Hello StorSimple Snapshot Manager jelszavának konfigurálása
StorSimple Snapshot Manager szoftver a Windows-gazdagépen helyezkedik el, és lehetővé teszi, hogy a rendszergazdák a StorSimple eszköz hello formában a helyi és felhőbeli pillanatképeket toomanage biztonsági másolatait.

> [!NOTE]
> Hello virtuális eszköz a Windows-állomás egy Azure virtuális gépen.
>
>

Amikor konfigurál egy eszközt a StorSimple Snapshot Manager hello, kérni fogja tooprovide hello StorSimple eszköz IP cím és jelszó tooauthenticate a tárolóeszköz. A részletes lépéseket lásd túl[konfigurálása a StorSimple Snapshot Manager jelszava](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

#### <a name="change-hello-device-administrator-password"></a>Változás hello eszköz rendszergazdai jelszava
Ha használja hello Windows PowerShell felületet tooaccess hello virtuális eszköz, szükséges tooenter egy eszköz rendszergazdai jelszava lehet. Hello biztonsági adatait, áll szükséges toochange ezt a jelszót, mielőtt hello virtuális eszköz használható. A részletes lépéseket lásd túl[eszközadminisztrátori jelszó konfigurálása](storsimple-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-toohello-virtual-device"></a>Távoli csatlakozás a virtuális eszköz toohello
Alapértelmezés szerint nincs engedélyezve a távelérés tooyour virtuális eszközt hello Windows PowerShell felületén keresztül. Tooenable távfelügyeletet a virtuális eszköz hello először kell, és majd engedélyezni a hello ügyfél, amely használt tooaccess lesz a virtuális eszköz.

hello kétlépéses folyamat tooconnect távolról részleteit az alábbiakban láthatja.

### <a name="step-1-configure-remote-management"></a>1. lépés: A távfelügyelet konfigurálása
Hajtsa végre a következő lépéseket tooconfigure a távoli felügyelete a StorSimple virtuális eszköz hello.

[!INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-hello-virtual-device"></a>2. lépés: Távoli hello virtuális eszközhöz való hozzáféréshez
Miután engedélyezte távfelügyeletet a hello StorSimple eszköz konfigurációs lapján, a Windows PowerShell távoli eljáráshívás tooconnect toohello virtuális eszköz hello belül egy másik virtuális gépről is használhatja ugyanazt a virtuális hálózatot; például csatlakoztathatja hello konfigurálva, és tooconnect iSCSI használt virtuális gép gazdagépről. A legtöbb üzemelő példányban hogy ekkor már megnyitott egy nyilvános végpontot tooaccess a hello virtuális eszköz eléréséhez használt virtuális gazdagépe.

> [!WARNING]
> **A fokozott biztonság érdekében javasoljuk toohello végpontok csatlakozás során használja a HTTPS, és azután törölje hello végpontok, távoli PowerShell-munkamenet befejezése után.**
>
>

Ajánlott eljárások hello [tooyour StorSimple-eszköz távoli kapcsolódás](storsimple-remote-connect.md) tooset fel a virtuális eszköz távoli eljáráshívási.

## <a name="connect-directly-toohello-virtual-device"></a>Csatlakozzon közvetlenül toohello virtuális eszköz
Elérhetők közvetlenül toohello virtuális eszközt. Ha tooconnect közvetlen toohello virtuális eszköz hello virtuális hálózat vagy a külső hello Microsoft Azure-környezeten kívül egy másik számítógépről, toocreate további végpontok eljárást követő hello leírtak szüksége.

Hajtsa végre a következő lépéseket toocreate egy nyilvános végpontot hello virtuális eszközön hello.

[!INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Javasoljuk, hogy csatlakozni egy másik virtuális gépről hello belül azonos virtuális hálózattal, mert így csökkenthető a virtuális hálózaton nyilvános végpontok hello száma. Ha ezt a módszert használja, egyszerűen csatlakoztassa a távoli asztali kapcsolaton keresztül toohello virtuális gépet, és majd konfigurálhatók a virtuális gép számára, mint bármely más Windows-ügyfél a helyi hálózaton. Nem kell tooappend hello nyilvános port számát, mert hello port már ismert lesz.

## <a name="work-with-hello-storsimple-virtual-device"></a>Hello StorSimple virtuális eszköz használata
Most, hogy létrehozta és hello StorSimple virtuális eszköz konfigurált, készen áll dolgozni vele toostart áll. Dolgozhat kötettárolók, kötetek és biztonsági mentési házirendek egy virtuális eszközön ugyanúgy, mint egy fizikai StorSimple eszközön; hello egyetlen különbség, hogy kell-e az eszközök listájában kattintson a virtuális eszköz hello toomake. Tekintse meg a túl[hello StorSimple Manager szolgáltatás toomanage a virtuális eszköz használata](storsimple-manager-service-administration.md) hello hello virtuális eszköz különböző kezelési feladatainak részletes eljárásait.

hello alábbi szakaszok ismertetik az egyes hello különbségeit az hello virtuális eszköz használatakor.

### <a name="maintain-a-storsimple-virtual-device"></a>A StorSimple virtuális eszköz karbantartása
Mivel egy csak szoftveres eszköz, hello virtuális eszköz karbantartása kell minimális Ha toomaintenance hello fizikai eszközhöz képest. Hello alábbi beállítások közül választhat:

* **Szoftverfrissítések** – hello szoftver legutolsó frissítésének, minden frissítés állapotüzenetek együtt hello dátumát tekintheti meg. Használhatja a hello **frissítések vizsgálata** gomb hello lap tooperform hello alján manuális keresést Ha toocheck az új frissítéseket. Ha frissítések érhetők el, kattintson a **frissítések telepítése** tooinstall. Mivel hello virtuális eszközön csak egyetlen felülettel, ez azt jelenti, hogy nem lesznek enyhe szolgáltatáskiesést frissítések alkalmazása. virtuális eszköz hello fog leállítása és újraindítása (ha szükséges) tooapply kiadott frissítéseket. Lépésenkénti útmutatót, nyissa meg túl[az eszköz frissítéséhez](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
* **Támogatási csomag** – hozhat létre, és egy támogatási csomag toohelp Microsoft Support feltöltés problémák elhárítása a virtuális eszköz. Lépésenkénti útmutatót, nyissa meg túl[létrehozását és kezelését egy támogatási csomag](storsimple-create-manage-support-package.md).

### <a name="storage-accounts-for-a-virtual-device"></a>Tárfiókok létrehozása egy virtuális eszközhöz
Storage-fiókok hello StorSimple Manager szolgáltatás által, hello virtuális eszköz és hello fizikai eszköz használatra jönnek létre. A tárfiókok létrehozásakor azt javasoljuk, hogy használja-e egy régiót azonosító rövid nevet toohelp hello győződjön meg arról hello régió egységességét az összes hello rendszer összetevőit. A virtuális eszköz esetében fontos, hogy az összes összetevői lehetnek a hello hello ugyanabban a régióban tooprevent teljesítményproblémákat.

Lépésenkénti útmutatót, nyissa meg túl[tárfiók hozzáadása](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>A StorSimple virtuális eszköz inaktiválása
A virtuális eszköz inaktiválása törli a virtuális gép hello és a kiépítésekor létrejött hello erőforrásokat. Hello virtuális eszközt az Inaktiválás után nem lehet visszaállítani tooits korábbi állapotába. Hello virtuális eszköz inaktiválása előtt győződjön meg arról, hogy toostop, vagy törölje az ügyfelek és a tőle függő gazdagépek.

A következő műveletek hello eredményezi, hogy a virtuális eszköz inaktiválása:

* hello virtuális eszközt a rendszer eltávolítja.
* hello operációsrendszer-lemez és adatlemezek hello virtuális rendszerhez létrehozott törlődnek.
* hello üzemeltetett szolgáltatás és a kiépítés során létrehozott virtuális hálózat megmarad. Ha ezeket nem használja, akkor manuálisan kell törölnie őket.
* Hello virtuális rendszerhez létrehozott felhőalapú pillanatfelvételek megmaradnak.

Lépésenkénti útmutatót, nyissa meg túl[inaktiválja és törölje a StorSimple eszköz](storsimple-deactivate-and-delete-device.md).

Amint hello virtuális eszköz inaktiváltként jelenik meg hello StorSimple Manager szolgáltatás lapján, törölheti hello eszköz listából hello virtuális eszköz **eszközök** lap.

### <a name="start-stop-and-restart-a-virtual-device"></a>A virtuális eszköz indítása, leállítása és újraindítása
Hello StorSimple fizikai eszközzel ellentétben nincs energiát be- vagy kikapcsoló gombbal toopush egy StorSimple virtuális eszközön. Azonban lehetnek olyan esetekben, ahol toostop kell, és indítsa újra a virtuális eszköz hello. Például egyes frissítések előfordulhat, hogy indítani, hogy virtuális gép lehet hello toofinish hello frissítési folyamatot. hello meg toostart, állítsa le és indítsa újra a virtuális eszköz legkönnyebben toouse hello Virtual Machines Management Console.

Ha megnézi a Management Console hello, hello virtuális eszköz állapota **futtató** mert elindul alapértelmezés szerint létrehozásukat követően. A virtuális eszközt bármikor elindíthatja, leállíthatja és újraindíthatja.

[!INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-toofactory-defaults"></a>Toofactory visszaállítása
Ha úgy dönt, hogy csak szeretné toostart keresztül a virtuális eszközzel, egyszerűen inaktiválja és törölje azt, és ezután hozzon létre egy újat. Csakúgy, mint amikor a fizikai eszköz alaphelyzetbe állítása az új virtuális eszközön nem kell; telepített frissítések Győződjön meg arról, hogy toocheck frissítések annak használata előtt.

## <a name="fail-over-toohello-virtual-device"></a>Feladatok átadása toohello virtuális eszköz
Vészhelyreállítás (DR) az egyik hello főbb forgatókönyvek hello StorSimple virtuális eszközt tervezték. Ebben az esetben a fizikai StorSimple eszköz hello, vagy egész adatközpont nem feltétlenül érhető el. Szerencsére a virtuális eszköz toorestore műveletek is használhatja egy másik helyen. A Vészhelyreállítás során hello forrás eszközről hello kötettárolók tulajdonjogai módosulnak és átvitt toohello virtuális eszköz. hello vész-Helyreállítási előfeltételei a következők, hogy hello virtuális eszköz létrehozása és konfigurálása, belül hello kötettároló összes hello kötetek offline került sor, és hello kötettároló a hozzárendelt felhő-pillanatfelvételt.

> [!NOTE]
> * Ha a virtuális eszköz hello másodlagos eszközként a vész-Helyreállítási, vegye figyelembe, hogy a 8010-es hello 30 TB standard szintű tárterülettel rendelkezik, és 8020-as modell 64 TB prémium szintű tárterülettel rendelkezik. hello nagyobb kapacitás 8020 virtuális eszköz egy Vészhelyreállítási forgatókönyvhöz alkalmasabb lehet.
> * Feladatátvétele sikertelen, vagy a Klónozás futtató eszközről 1 frissítés előtti szoftvert futtató 2 tooa-eszköz frissítése. Azonban végrehajthat feladatátvételt 1. frissítést (1.1-es vagy 1.2-es) futtató eszköznek az Update 2 tooa futtató eszközök
>
>

Lépésenkénti útmutatót, nyissa meg túl[feladatátvételi tooa virtuális eszköz](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).

## <a name="shut-down-or-delete-hello-virtual-device"></a>Állítsa le vagy hello virtuális eszköz törlése
Ha korábban konfigurált és használt egy StorSimple virtuális eszköz használatáért további számítási költségeket keletkezhetnek toostop, hello virtuális eszköz leállíthat. Hello virtuális eszköz leállítása az operációs rendszer vagy a tároló adatlemezei nem törli. Akkor az nem előfizetéséhez keletkezhetnek költségek, de az operációs rendszer hello tárolás költségekkel és adatlemezek továbbra is.

Ha törli vagy hello virtuális eszköz leállítása, akkor jelenik meg **Offline** hello hello StorSimple Manager szolgáltatás eszközök lapján. Választhat, toodeactivate vagy hello eszköz törlése, ha is toodelete hello virtuális eszköz által létrehozott hello biztonsági mentéseket. További információkért lásd [a StorSimple eszköz inaktiválását vagy törlését](storsimple-deactivate-and-delete-device.md) ismertető szakaszt.

[!INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[!INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

## <a name="troubleshoot-internet-connectivity-errors"></a>Az internetkapcsolat hibáinak elhárítása
A virtuális eszköz hello létrehozása, ha nincs kapcsolat toohello Internet, hello létrehozása lépés sikertelen lesz. tootroubleshoot Ha hello hiba miatt az internetkapcsolat működését, végezze el a következő lépéseket a klasszikus Azure portálon hello hello:

1. Hozzon létre egy Windows Server 2012 virtuális gépet az Azure-ban. A virtuális gép használja ugyanazt a tárfiókot, virtuális hálózat és a virtuális eszköz által használt alhálózati hello. Ha már rendelkezik egy meglévő Azure használata a Windows Server-állomáson hello ugyanazt a tárfiókot, Vnet és alhálózat, akkor is használható tootroubleshoot hello internetkapcsolat.
2. Távoli bejelentkezési hello az előző lépésben létrehozott hello virtuális géppé.
3. Nyisson meg egy parancsablakot hello virtuális gépen belüli (Win + K, majd írja be `cmd`).
4. Futtassa a következő hello parancssorba cmd hello.

    `nslookup windows.net`
5. Ha `nslookup` nem sikerül, akkor Internet csatlakozási hiba megakadályozza hello virtuális eszköz regisztrációja toohello StorSimple Manager szolgáltatás.
6. Módosításokat hello szükséges, hogy a virtuális eszköz hello tooyour virtuális hálózati tooensure van képes tooaccess Azure helyeken, például a "windows.net".

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[hello StorSimple Manager szolgáltatás toomanage a virtuális eszköz használata](storsimple-manager-service-administration.md).
* Hogyan túl megértéséhez[a StorSimple-kötet visszaállítása egy biztonságimásolat-készlet](storsimple-restore-from-backup-set.md).
