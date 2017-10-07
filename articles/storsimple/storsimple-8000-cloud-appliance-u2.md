---
title: "Felhő készülék Update 3 aaaStorSimple |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate, telepítését és kezelését egy StorSimple felhő készülék egy Microsoft Azure virtuális hálózatban. (Csak érvényes tooStorSimple Update 3 és újabb)."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/10/2017
ms.author: alkohli
ms.openlocfilehash: ba60a629f1f4b8f0d4566eeb45bae8696f50d0af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-a-storsimple-cloud-appliance-in-azure-update-3-and-later"></a>A StorSimple Cloud Appliance üzembe helyezése és kezelése az Azure-ban (3. frissítés és újabb)

## <a name="overview"></a>Áttekintés

a StorSimple 8000 Series felhő készülék hello a Microsoft Azure StorSimple megoldáshoz egy további lehetőség. StorSimple felhő készülék hello egy Microsoft Azure virtuális hálózatot a virtuális gépen fut, és használhatja fel tooback és a Klónozás adatokat a gazdagépekről.

Ez a cikk hello részletes folyamat toodeploy ismerteti, és kezelheti a StorSimple felhő készülék az Azure-ban. A cikk elolvasása után:

* Ismerje meg, hogy miben különbözik hello felhő készülék hello fizikai eszközről.
* Képes toocreate és hello felhő készülék konfigurálása.
* Csatlakozás toohello felhő készüléket.
* Ismerje meg, hogyan toowork hello rendelkező felhőalapú készüléket.

Ez az oktatóanyag vonatkozik tooall hello StorSimple felhő szükséges, amelyen a frissítési 3 és újabb verziók.

#### <a name="cloud-appliance-model-comparison"></a>Felhőalapú készülék-modellek összehasonlítása

hello StorSimple felhő készülék érhető el a két modell egy hagyományos 8010-es (korábbi nevén hello 1100) és a prémium 8020-as modell (a 2. frissítés verzióban jelent). a következő táblázat hello hello két modell összehasonlítása mutatja be.

| Eszközmodell | 8010<sup>1</sup> | 8020 |
| --- | --- | --- |
| **Maximális kapacitás** |30 TB |64 TB |
| **Azure virtuális gép** |Standard_A3 (4 mag, 7 GB memória)| Standard_DS3 (4 mag, 14 GB memória)|
| **Régiónkénti elérhetőség** |Minden Azure-régió |A Premium Storage-ot támogató Azure-régiók és DS3 Azure-beli virtuális gépek<br></br>Használjon [ebben a listában](https://azure.microsoft.com/regions/services/) toosee, ha mindkét **virtuális gépek > DS sorozatnak** és **tárolási > tároló lemez** érhetők el az adott régióban. |
| **Tárolás típusa** |A helyi lemezeken Azure Standard szintű tárolást használ<br></br> Ismerje meg, hogyan túl[szabványos Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md) |A helyi lemezeken Azure Premium szintű tárolást használ<sup>2</sup> <br></br>Ismerje meg, hogyan túl[prémium szintű Storage-fiók létrehozása](../storage/common/storage-premium-storage.md) |
| **Útmutató a számítási feladatokhoz** |A fájlok elemszintű lekérése a biztonsági másolatokból |Felhőalapú fejlesztési és tesztelési forgatókönyvek <br></br>Kis késés és nagyobb teljesítményű számítási feladatok<br></br>Másodlagos vészhelyreállítási eszköz |

<sup>1</sup> *nevén hello 1100*.

<sup>2</sup> *mindkét hello 8010-es és a 8020-as modellt használja. Ha Azure standard szintű tárolást hello felhő réteg hello különbség csak hello helyi rétegen belüli hello eszköz szerepel*.

## <a name="how-hello-cloud-appliance-differs-from-hello-physical-device"></a>Hogyan eltér az hello felhő készülék hello fizikai eszköz

StorSimple felhő készülék hello egy csak szoftveres StorSimple, amely a Microsoft Azure virtuális gép egyetlen csomópontján fut verziója. hello felhő készülék támogatja a vész-helyreállítási eljárással, amelyben a fizikai eszköz nem érhető el. hello felhő készülék valójában megfelelő elemszintű lekérése a biztonsági mentésekből, helyszíni vészhelyreállításhoz és felhőalapú fejlesztési és tesztelési forgatókönyvekhez használható.

#### <a name="differences-from-hello-physical-device"></a>Eltérések a hello fizikai eszköz

hello következő táblázatban hello StorSimple felhő készülék és hello StorSimple fizikai eszköz közötti fontosabb különbségeket.

|  | Fizikai eszköz | Felhőalapú készülék |
| --- | --- | --- |
| **Hely** |Hello adatközpontban található. |Az Azure-ban fut. |
| **Hálózati illesztők** |Hat hálózati adapterrel rendelkezik: DATA 0-tól DATA 5 számozásig. |Csak egy hálózati adapterrel rendelkezik: DATA 0. |
| **Regisztráció** |Regisztrált hello kezdeti konfigurációs lépés során. |A regisztráció egy külön feladat. |
| **Szolgáltatásadat-titkosítási kulcs** |Generálja újra hello fizikai eszközön, és frissítse a hello felhő készülék hello új kulccsal. |Nem lehet újra létrehozni a hello felhő készülék. |
| **Támogatott kötettípusok** |A helyileg rögzített és a rétegzett köteteket is támogatja. |Csak a rétegzett köteteket támogatja. |

## <a name="prerequisites-for-hello-cloud-appliance"></a>Hello felhő készülék előfeltételei

hello a következő szakaszok ismertetik a StorSimple felhő készülék hello konfigurációs előfeltételeit. A felhő készülék telepítése előtt tekintse át a hello biztonsági szempontjai a felhő készülék segítségével.

[!INCLUDE [StorSimple Cloud Appliance security](../../includes/storsimple-8000-cloud-appliance-security.md)]

#### <a name="azure-requirements"></a>Azure-követelmények

Hello felhő készülék kiépítése előtt a következő műveleteket az Azure környezetben toomake hello szüksége:

* Győződjön meg arról, hogy az adatközpontban StorSimple 8000 sorozatú fizikai eszköz (8100-as vagy 8600-as modell) van üzembe helyezve és fut. Regisztrálja az eszközt a hello ugyanazt a StorSimple Device Manager szolgáltatást, melyet a StorSimple a felhő készülék toocreate.
* Hello felhő alapplatformjaként [Azure virtuális hálózat konfigurálása](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Premium szintű Storage használata esetén a Premium szintű Storage-ot támogató Azure-régióban kell létrehoznia egy virtuális hálózatot. prémium szintű Storage-régiók Hello, amelyek megfelelnek a lemezes tárolás a hello toohello sor régiók [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/services/).
* Azt javasoljuk, hogy használja-e hello alapértelmezett DNS-kiszolgáló a saját DNS-kiszolgálónév megadása helyett az Azure által biztosított. Ha a DNS-kiszolgáló neve nem érvényes, vagy ha hello DNS-kiszolgáló nem tudja tooresolve IP-címek megfelelően, hello felhő készülék hello létrehozása sikertelen lesz.
* A végpont és telephely közötti, valamint a telephely és telephely közötti lehetőségek választhatóak, de nem kötelezőek. Igény szerint ezeket a lehetőségeket speciális forgatókönyvekhez is konfigurálhatja.
* Létrehozhat [Azure virtuális gépek](../virtual-machines/virtual-machines-windows-quick-create-portal.md) (gazdakiszolgálókat), amely a hello felhő készülék által elérhetővé tett hello kötetek hello a virtuális hálózatban. Ezek a kiszolgálók hello követelményeknek kell megfelelniük:

  * Windows vagy Linux rendszerű virtuális gépnek kell lenniük, amelyen telepítve van az iSCSI-kezdeményező szoftver.
  * Hello futni hello felhő készülék megegyező virtuális hálózatban.
  * Képes tooconnect toohello iSCSI-tároló hello felhő készülék kell hello belső IP-cím hello felhő készülék keresztül.
  * Ellenőrizze, hogy konfigurálta támogatását, az iSCSI és a felhőalapú forgalom hello az azonos virtuális hálózaton.

#### <a name="storsimple-requirements"></a>A StorSimple követelményei

Hajtsa végre a következő frissítési tooyour StorSimple Device Manager szolgáltatáshoz, mielőtt létrehozna egy felhőalapú készülék hello:

* Adja hozzá [hozzáférés-vezérlési rekordokat](storsimple-8000-manage-acrs.md) hello virtuális gépek, amelyek folyamatos toobe hello kiszolgálók a felhő alapplatformjaként.
* Használja a [tárfiók](storsimple-8000-manage-storage-accounts.md#add-a-storage-account) a hello és hello felhő készüléknek ugyanabban a régióban. Különböző régiókban lévő tárfiókok használata esetén a teljesítmény gyenge lehet. Egy Standard vagy prémium szintű Storage-fiók hello felhő készülék használható. További információt toocreate egy [standard szintű tárfiók](../storage/common/storage-create-storage-account.md) vagy egy [prémium szintű Storage-fiók](../storage/common/storage-premium-storage.md)
* Felhő készülék létrehozásához az egy az adatok a hello során eltérő tárfiók használt. Ugyanazt a tárfiókot teljesítménycsökkenést eredményezhet hello segítségével.

Győződjön meg arról, hogy rendelkezik-e megkezdése előtt információkat a következő hello:

* Az Azure Portal-fiókja és a hozzáférési hitelesítő adatai.
* Hello szolgáltatásadat-titkosítási kulcs a fizikai eszközről másolatát regisztrált toohello StorSimple Device Manager szolgáltatást.

## <a name="create-and-configure-hello-cloud-appliance"></a>Hozza létre és konfigurálja a felhő készülék hello

Az eljárások végrehajtása előtt győződjön meg arról, hogy teljesül-e az hello [hello felhő készülék előfeltételei](#prerequisites-for-the-cloud-appliance).

Hajtsa végre a következő lépéseket toocreate egy StorSimple felhő készülék hello.

### <a name="step-1-create-a-cloud-appliance"></a>1. lépés: Felhőalapú készülék létrehozása

Hajtsa végre a következő lépéseket toocreate hello StorSimple felhő készülék hello.

[!INCLUDE [Create a cloud appliance](../../includes/storsimple-8000-create-cloud-appliance-u2.md)]

Hello felhő készülék hello létrehozása ebben a lépésben nem sikerül, ha nincs kapcsolat toohello Internet. További információ: túl[Internet csatlakozási hibák elhárítása](#troubleshoot-internet-connectivity-errors) egy felhőalapú készülék létrehozásakor.

### <a name="step-2-configure-and-register-hello-cloud-appliance"></a>2. lépés: Konfigurálása és regisztrálása hello felhő készülék

Ez az eljárás megkezdése előtt győződjön meg arról, hogy rendelkezik-e hello szolgáltatásadat-titkosítási kulcs másolatával. hello szolgáltatásadat-titkosítási kulcs jön létre, amikor az első StorSimple fizikai eszköz StorSimple Device Manager szolgáltatás hello regisztrált. Utasításai toosave elveszne, biztonságos helyen. Ha nem rendelkezik hello szolgáltatásadat-titkosítási kulcs biztonsági másolatát, segítségért lépjen kapcsolatba Microsoft Support.

Hajtsa végre a következő lépéseket tooconfigure hello és regisztrálása a StorSimple felhő készüléket.

[!INCLUDE [Configure and register a cloud appliance](../../includes/storsimple-8000-configure-register-cloud-appliance.md)]

### <a name="step-3-optional-modify-hello-device-configuration-settings"></a>3. lépés: Konfiguráció (nem kötelező) módosítás hello eszközbeállítások

hello alábbi szakasz ismerteti eszközkonfigurációs beállítások hello Ha hello eszköz rendszergazdai jelszavának módosítása vagy toouse CHAP, a StorSimple Snapshot Manager szeretné hello StorSimple felhő készülék szükséges.

#### <a name="configure-hello-chap-initiator"></a>Hello CHAP-kezdeményező konfigurálása

Ez a paraméter tartalmazza a hello hitelesítő adatokat, amelyeket a felhő készülék (cél), melyek tooaccess hello kötetek hello kezdeményezőktől (kiszolgálóktól) vár. hello kezdeményezők adjon meg egy CHAP-felhasználónév és egy CHAP-jelszó tooidentify maguk tooyour eszköz a hitelesítés során. A részletes lépéseket lásd túl[CHAP konfigurálása az eszközhöz](storsimple-8000-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-hello-chap-target"></a>Hello CHAP céljának konfigurálása

Ez a paraméter tartalmazza a hello hitelesítő adatokat, amelyeket a felhő készülék használja, amikor egy CHAP-támogatással rendelkező kezdeményező kölcsönös vagy kétirányú hitelesítést kér. A felhő készülék használ egy felhasználónevet fordított irányú CHAP és fordított CHAP-jelszó tooidentify maga toohello kezdeményező a hitelesítési folyamat során.

> [!NOTE]
> A CHAP-cél beállításai globálisak. Ha ezeket a beállításokat alkalmazzák, az összes hello kapcsolódó toohello felhő készülék CHAP hitelesítés használata.

A részletes lépéseket lásd túl[CHAP konfigurálása az eszközhöz](storsimple-8000-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-hello-storsimple-snapshot-manager-password"></a>Hello StorSimple Snapshot Manager jelszavának konfigurálása

StorSimple Snapshot Manager szoftver a Windows-gazdagépen helyezkedik el, és lehetővé teszi, hogy a rendszergazdák a StorSimple eszköz hello formában a helyi és felhőbeli pillanatképeket toomanage biztonsági másolatait.

> [!NOTE]
> Hello felhő alapplatformjaként a Windows-állomás egy Azure virtuális gépen.

Amikor konfigurál egy eszközt a StorSimple Snapshot Manager hello, tooprovide hello StorSimple eszköz IP cím és jelszó tooauthenticate a tárolóeszköz. A részletes lépéseket lásd túl[konfigurálása a StorSimple Snapshot Manager jelszava](storsimple-8000-change-passwords.md#set-the-storsimple-snapshot-manager-password).

#### <a name="change-hello-device-administrator-password"></a>Változás hello eszköz rendszergazdai jelszava

Használatakor hello Windows PowerShell felületet tooaccess hello felhő készüléknek, akkor szükség tooenter egy eszköz rendszergazdai jelszava. Hello biztonsági adatait módosítania kell a jelszót hello felhő készülék használata előtt. A részletes lépéseket lásd túl[eszközadminisztrátori jelszó konfigurálása](../storsimple/storsimple-8000-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-toohello-cloud-appliance"></a>Távoli csatlakozás toohello felhő készülék

Alapértelmezés szerint nincs engedélyezve a távelérés tooyour felhő készülék hello Windows PowerShell felületén keresztül. Először engedélyezze a távoli felügyeleti hello felhő készüléken, és majd hello az ügyfél felhasználta az tooaccess hello felhő készüléket.

hello kétlépéses eljárást ismerteti, hogyan tooconnect távolról tooyour felhőalapú készüléket.

### <a name="step-1-configure-remote-management"></a>1. lépés: A távfelügyelet konfigurálása

Hajtsa végre a következő lépéseket tooconfigure a távoli felügyelete a StorSimple felhő készülék hello.

[!INCLUDE [Configure remote management via HTTP for cloud appliance](../../includes/storsimple-8000-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-hello-cloud-appliance"></a>2. lépés: Távoli eléréséhez hello felhő készülék

Miután hello felhő készülék távoli felügyeletének engedélyezése, a Windows PowerShell távoli eljáráshívás tooconnect toohello készüléket hello belül egy másik virtuális gép ugyanazon a virtuális hálózaton. Például csatlakoztathatja hello konfigurálva, és tooconnect iSCSI használt virtuális gép gazdagépről. A legtöbb telepítés ekkor megnyílik egy nyilvános végpontot tooaccess, a gazdagép virtuális Gépet, amely hello felhő készülék eléréséhez használható.

> [!WARNING]
> **A fokozott biztonság érdekében javasoljuk toohello végpontok csatlakozás során használja a HTTPS, és azután törölje hello végpontok, távoli PowerShell-munkamenet befejezése után.**

A hello eljárás [tooyour StorSimple-eszköz távoli kapcsolódás](storsimple-8000-remote-connect.md) tooset be a felhő készülék távoli eljáráshívást.

## <a name="connect-directly-toohello-cloud-appliance"></a>Csatlakozzon közvetlenül toohello felhő készülék

Elérhetők közvetlenül toohello felhő készüléket. tooconnect közvetlen toohello felhőalapú kívül egy másik számítógépről készülék hello virtuális hálózat vagy a külső hello Microsoft Azure-környezethez, további végpontokat kell létrehoznia.

Hajtsa végre a következő lépéseket toocreate egy nyilvános végpontot hello felhő készüléken hello.

[!INCLUDE [Create public endpoints on a cloud appliance](../../includes/storsimple-8000-create-public-endpoints-cloud-appliance.md)]

Javasoljuk, hogy csatlakozni egy másik virtuális gépről hello belül azonos virtuális hálózattal, mert így csökkenthető a virtuális hálózaton nyilvános végpontok hello száma. Ebben az esetben toohello virtuális gép csatlakozni egy távoli asztali kapcsolaton keresztül, és majd konfigurálhatók a virtuális gép számára, mint bármely más Windows-ügyfél a helyi hálózaton. Nem kell tooappend hello nyilvános port számát, mert hello port már ismert.

## <a name="work-with-hello-storsimple-cloud-appliance"></a>StorSimple felhő készülék hello használata

Most, hogy létrehozta és konfigurált hello StorSimple felhő készüléknek, készen áll dolgozni vele toostart áll. Egy felhőalapú berendezésen ugyanúgy használhatja a kötettárolókat, köteteket és biztonsági mentési házirendeket, mint egy fizikai StorSimple eszközön. hello egyetlen különbség, hogy kell-e toomake arról, hogy kiválasztotta hello felhő készülék a listából. Tekintse meg a túl[hello StorSimple Device Manager szolgáltatás toomanage egy felhőalapú készülék használja](storsimple-8000-manager-service-administration.md) különféle felügyeleti feladatokat hello felhő készülék hello részletes eljárásait.

hello alábbi szakaszok ismertetik az hello felhő készülék használatakor előforduló hello különbségek némelyike.

### <a name="maintain-a-storsimple-cloud-appliance"></a>A StorSimple Cloud Appliance karbantartása

Mivel egy csak szoftveres eszköz, karbantartási hello felhő alapplatformjaként kell minimális Ha toomaintenance hello fizikai eszközhöz képest.

A felhőalapú berendezések nem frissíthetők. A szoftver toocreate egy új felhőalapú készülék hello legújabb verzióját használja.


### <a name="storage-accounts-for-a-cloud-appliance"></a>Tárfiókok létrehozása felhőalapú berendezéshez

Storage-fiókok hello StorSimple Device Manager szolgáltatás által, hello felhő készülék és hello fizikai eszköz használatra jönnek létre. A tárfiókok létrehozásakor hello rövid név egy régióazonosító használata ajánlott. Ez segít, győződjön meg arról, hogy hello régió egységességét az összes hello rendszer összetevőit. A felhő alapplatformjaként fontos, hogy az összes hello összetevő legyenek hello ugyanabban a régióban tooprevent teljesítményproblémákat.

Lépésenkénti útmutatót, nyissa meg túl[tárfiók hozzáadása](storsimple-8000-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-cloud-appliance"></a>A StorSimple Cloud Appliance inaktiválása

Ha egy felhő készülék inaktiválásához hello a művelet törli a hello virtuális gép és a kiépítésekor létrejött hello erőforrásokat. Hello felhő készüléknek az Inaktiválás után nem lehet visszaállítani tooits korábbi állapotába. Hello felhő készülék inaktiválása előtt győződjön meg arról, hogy toostop, vagy törölje az ügyfelek és a tőle függő gazdagépek.

A következő műveletek hello eredményezi, hogy a felhő készülék inaktiválása:

* hello felhő készülék törlődnek.
* hello operációsrendszer-lemez és adatlemezek hello felhő készülék készült törlődnek.
* hello üzemeltetett szolgáltatás és a kiépítés során létrehozott virtuális hálózat megmarad. Ha ezeket nem használja, akkor manuálisan kell törölnie őket.
* Hello felhő alapplatformjaként létrehozott felhőalapú pillanatfelvételek megmaradnak.

Lépésenkénti útmutatót, nyissa meg túl[inaktiválja és törölje a StorSimple eszköz](storsimple-8000-deactivate-and-delete-device.md).

Amint hello felhő készülék inaktiváltként jelenik hello StorSimple Device Manager szolgáltatás panelen, törölheti hello eszköz listából hello felhő készülék **eszközök** panelen.

### <a name="start-stop-and-restart-a-cloud-appliance"></a>A felhőalapú berendezés indítása, leállítása és újraindítása
Hello StorSimple fizikai eszközzel ellentétben nincs energiát be- vagy kikapcsoló gombbal toopush a StorSimple-felhő készüléken. Azonban lehetnek olyan esetekben, ahol toostop kell, és indítsa újra a hello felhő készüléket.

hello legegyszerűbben az Ön toostart, állítsa le és indítsa újra a felhő készülék hello keresztül, a virtuális gépek szolgáltatás panelre. Nyissa meg a hello virtuálisgép-szolgáltatás. Virtuális gépek hello listából hello VM megfelelő tooyour felhő készülék (néven) azonosítása, majd kattintson a hello virtuális gép nevét. Ha megnézi a virtuális gépek paneljét, hello felhő készülék állapota **futtató** mert elindul alapértelmezés szerint létrehozásukat követően. A virtuális eszközt bármikor elindíthatja, leállíthatja és újraindíthatja.

[!INCLUDE [Stop and restart cloud appliance](../../includes/storsimple-8000-stop-restart-cloud-appliance.md)]

### <a name="reset-toofactory-defaults"></a>Toofactory visszaállítása
Ha úgy dönt, amelyet toostart keresztül és a felhő készüléknek, inaktiválja és törölje azt, és majd hozzon létre egy újat.

## <a name="fail-over-toohello-cloud-appliance"></a>Feladatok átadása toohello felhő készülék
Vészhelyreállítás (DR) egyike a hello főbb forgatókönyvek, amelyek a StorSimple felhő készülék hello tervezték. Ebben az esetben a fizikai StorSimple eszköz hello, vagy egész adatközpont nem érhető el. Szerencsére a készülék toorestore felhőműveletek használható egy másik helyre. A Vészhelyreállítás során hello forrás eszközről hello kötettárolók tulajdonjogai módosulnak és átvitt toohello felhő készülék.

DR hello előfeltételei a következők:

* hello felhő készülék létrehozott és konfigurálva van.
* Hello kötettároló belül minden hello kötetek offline módban.
* a rendszer átadja, hello kötettároló tartozik egy felhőalapú pillanatfelvétel.

> [!NOTE]
> * Ha egy felhő készülék hello másodlagos eszközként a vész-Helyreállítási, vegye figyelembe, hogy a 8010-es hello 30 TB standard szintű tárterülettel rendelkezik, és 8020-as modell 64 TB prémium szintű tárterülettel rendelkezik. hello nagyobb kapacitás 8020-as modellt felhő készülék egy Vészhelyreállítási forgatókönyvhöz alkalmasabb lehet.

Lépésenkénti útmutatót, nyissa meg túl[tooa felhő készülék feladatátvételt](storsimple-8000-device-failover-cloud-appliance.md).

## <a name="delete-hello-cloud-appliance"></a>Hello felhő készülék törlése
Ha korábban konfigurált és használt egy StorSimple felhő készülék most toostop számoljanak fel Önnek a használatával további költségeket, hello felhő készülékre le kell állítania. Leállítása hello felhő készülék felszabadítja a virtuális gép hello. Ha ezt megteszi, nem keletkeznek további költségek az előfizetésén. tárolási költségek hello hello az operációs rendszer és adatlemezek azonban továbbra is.

összes hello toostop díja, törölnie kell az hello felhő készüléket. toodelete hello által létrehozott biztonsági mentéseket hello felhő készüléknek, akkor is inaktiválja vagy törölje az hello eszköz. További információkért lásd [a StorSimple eszköz inaktiválását vagy törlését](storsimple-8000-deactivate-and-delete-device.md) ismertető szakaszt.

[!INCLUDE [Delete a cloud appliance](../../includes/storsimple-8000-delete-cloud-appliance.md)]

## <a name="troubleshoot-internet-connectivity-errors"></a>Az internetkapcsolat hibáinak elhárítása
A felhő készülék hello létrehozása, ha nincs kapcsolat toohello Internet, hello létrehozása lépés sikertelen lesz. tootroubleshoot Internet kapcsolathibái, hajtsa végre a következő lépéseket az Azure-portálon hello hello:

1. [Hozzon létre egy Windows Server 2012-alapú virtuális gépet az Azure-ban](/articles/virtual-machines/windows/quick-create-portal.md). A virtuális gép használja ugyanazt a tárfiókot, virtuális hálózat és a felhő készülék által használt alhálózati hello. Ha egy meglévő Azure használata a Windows Server-állomáson hello ugyanazt a tárfiókot, VNet és alhálózat, is használhatja tootroubleshoot hello internetkapcsolat.
2. Távoli bejelentkezési hello az előző lépésben létrehozott hello virtuális géppé.
3. Nyisson meg egy parancsablakot hello virtuális gépen belüli (Win + K, majd írja be `cmd`).
4. Futtassa a következő hello parancssorba cmd hello.

    `nslookup windows.net`
5. Ha `nslookup` nem sikerül, akkor Internet csatlakozási hiba miatt nem tudja hello felhő készüléket toohello StorSimple Device Manager szolgáltatás regisztrálása.
6. Módosítások hello szükséges, hogy a felhő készülék hello tooyour virtuális hálózati tooensure van képes tooaccess Azure helyek például _windows.net_.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[hello StorSimple Device Manager szolgáltatás toomanage egy felhőalapú készülék használja](storsimple-8000-manager-service-administration.md).
* Hogyan túl megértéséhez[a StorSimple-kötet visszaállítása egy biztonságimásolat-készlet](storsimple-8000-restore-from-backup-set-u2.md).
