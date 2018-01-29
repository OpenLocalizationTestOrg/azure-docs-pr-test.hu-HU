---
title: "Az MPIO konfigurálása a StorSimple eszköz |} Microsoft Docs"
description: "A StorSimple eszköz egy Windows Server 2012 R2 rendszerű gazdagép csatlakozik a többutas I/O (MPIO) beállításának módját ismerteti."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 9fe3fa3a2df63d111de742ecb48b1469aad543cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="configure-multipath-io-for-your-storsimple-device"></a>Többutas I/O konfigurálása a StorSimple eszköz

Ez az oktatóanyag ismerteti, hogyan telepítheti és használhatja a többutas I/O (MPIO) szolgáltatást futtató Windows Server 2012 R2 nyújtanak, és a StorSimple fizikai eszköz csatlakozik. A StorSimple 8000 series csak a fizikai eszközök vonatkozik az útmutató. Az MPIO jelenleg nem támogatott a StorSimple-felhő készüléken.

A Microsoft beépített támogatása a többutas I/O (MPIO) szolgáltatást a Windows Server a Súgó build magas rendelkezésre állású, nagy hibatűrésű SAN beállításokat. Az MPIO használ a redundáns összetevők – adapterek, kábelek és kapcsolók – a kiszolgáló és a tárolóeszköz közötti logikai útvonalak létrehozásához. Ha sikertelen egy összetevő, és ezáltal a logikai útvonal is meghibásodik, többutas logika egy alternatív útvonalat használ az i/o, hogy az alkalmazások továbbra is hozzáférjenek az adataikhoz. Emellett a konfigurációtól függően az MPIO is a jobb teljesítmény érdekében a terhelés újraelosztás közötti összes elérési utat. További információkért lásd: [MPIO áttekintése](https://technet.microsoft.com/library/cc725907.aspx "MPIO áttekintése és a szolgáltatások").

A magas rendelkezésre állású a StorSimple megoldás, a többutas I/O konfigurálnia kell a StorSimple eszközt. Ha az MPIO telepítve van a Windows Server 2012 R2 rendszert futtató kiszolgálók, a kiszolgálók majd tűri egy hivatkozás, hálózati vagy adapterhibákat.

## <a name="mpio-configuration-summary"></a>Az MPIO konfigurációja összefoglaló

Az MPIO választható lehetőség a Windows Server, és alapértelmezés szerint nincs telepítve. Szolgáltatásként lehet telepíteni a Kiszolgálókezelőn keresztül.

Kövesse az alábbi lépéseket az MPIO konfigurálása a StorSimple eszköz:

* 1. lépés: Telepítse a Windows Server-gazdagép MPIO
* 2. lépés: Az MPIO konfigurálása a StorSimple-köteteket
* 3. lépés: Csatlakoztatási StorSimple-köteteket a gazdagépen
* 4. lépés: Az MPIO konfigurálása a magas rendelkezésre állás érdekében és terheléselosztás

Az alábbi szakaszok az előző lépések mindegyikét ismertet.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>1. lépés: Telepítse a Windows Server-gazdagép MPIO

A szolgáltatás telepítéséhez a Windows Server-állomáson, hajtsa végre az alábbi eljárást.

#### <a name="to-install-mpio-on-the-host"></a>A gazdagép MPIO telepítése

1. Nyissa meg a Kiszolgálókezelőt a Windows Server-állomáson. Alapértelmezés szerint a Kiszolgálókezelő akkor kezdődik, amikor egy tagja a Rendszergazdák csoport bejelentkezik egy Windows Server 2012 R2 vagy Windows Server 2012 rendszert futtató számítógép. Ha a Kiszolgálókezelő még nincs megnyitva, kattintson a **Start > Kiszolgálókezelő**.
   
   ![Kiszolgálókezelő](./media/storsimple-configure-mpio-windows-server/IC740997.png)

2. Kattintson a **Kiszolgálókezelő > irányítópult > szerepkörök és szolgáltatások hozzáadása**. Ekkor elindul a **szerepkörök és szolgáltatások hozzáadása** varázsló.
   
   ![Szerepkörök és szolgáltatások hozzáadása varázsló 1](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. Az a **szerepkörök és szolgáltatások hozzáadása** varázsló, hajtsa végre a következő lépéseket:
   
   1. Az a **megkezdése előtt** kattintson **következő**.
   2. Az a **telepítés típusának kiválasztása** lap, fogadja el az alapértelmezett **szerepköralapú vagy szolgáltatásalapú** telepítését. Kattintson a **Tovább** gombra.
   
       ![Szerepkörök és szolgáltatások hozzáadása varázsló 2. régiója](./media/storsimple-configure-mpio-windows-server/IC740999.png)
   3. Az a **jelölje be a célkiszolgáló** lapon, válassza ki **kiszolgáló kijelölése a kiszolgálókészletből**. A gazdagép-kiszolgálón kell automatikusan észlelhetők. Kattintson a **Tovább** gombra.
   4. Az a **kiszolgálói szerepkörök kiválasztása** kattintson **következő**.
   5. A a **szolgáltatások kiválasztása** lapon, hogy melyik **többutas i/o**, és kattintson a **következő**.
   
       ![Szerepkörök és szolgáltatások hozzáadása varázsló 5](./media/storsimple-configure-mpio-windows-server/IC741000.png)
   6. Az a **erősítse meg a telepítendő** lapon ellenőrizze a kijelölést, majd válassza ki **a célkiszolgáló automatikus újraindítása, ha szükséges**lent látható módon. Kattintson az **Install** (Telepítés) gombra.
   
       ![Szerepkörök és szolgáltatások hozzáadása varázsló 8](./media/storsimple-configure-mpio-windows-server/IC741001.png)
   7. Értesítést kap a telepítés befejeződött. A varázsló bezárásához kattintson a **Bezárás** gombra.
   
       ![Szerepkörök és szolgáltatások hozzáadása varázsló 9](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>2. lépés: Az MPIO konfigurálása a StorSimple-köteteket

Az MPIO kell állítani a StorSimple-köteteket azonosításához. MPIO-t ismeri fel a StorSimple-köteteket konfigurálásához hajtsa végre az alábbi lépéseket.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>Az MPIO konfigurálása a StorSimple-köteteket

1. Nyissa meg a **az MPIO konfigurációja**. Kattintson a **Kiszolgálókezelő > irányítópult > eszközök > MPIO**.
2. Az a **többutas I/O tulajdonságai** párbeszédpanelen jelölje ki a **felderítése többutas** fülre.
3. Válassza ki **az iSCSI-eszközök támogatását**, és kattintson a **Hozzáadás**.  
   ![Többutas I/O tulajdonságai többféle elérési utak felderítése](./media/storsimple-configure-mpio-windows-server/IC741003.png)
4. Indítsa újra a kiszolgálót, amikor a rendszer kéri.
5. Az a **többutas I/O tulajdonságai** párbeszédpanel, kattintson a **MPIO eszközök** fülre. Kattintson az **Hozzáadás** parancsra.
    </br>![Az MPIO tulajdonságok többutas I/O eszköz](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. Az a **MPIO-támogatás hozzáadása** párbeszédpanel **eszköz Hardverazonosító**, adja meg az eszköz sorozatszámát. Ahhoz, hogy az eszköz sorozatszámát, a StorSimple Device Manager szolgáltatáshoz. Navigáljon a **eszközök > irányítópult**. A jobb oldalon jelenik meg az eszköz sorozatszámát **gyors áttekintése** az eszköz irányítópult panelén.
    </br>
    ![Az MPIO támogatását](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Indítsa újra a kiszolgálót, amikor a rendszer kéri.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>3. lépés: Csatlakoztatási StorSimple-köteteket a gazdagépen

Az MPIO konfigurálása után a Windows Server, a StorSimple eszközön létre kötet(ek) lehet csatlakoztatni, és majd kihasználhatja az MPIO a redundancia érdekében. Kötet csatlakoztatása, hajtsa végre a következő lépéseket.

#### <a name="to-mount-volumes-on-the-host"></a>Kötet csatlakoztatása a gazdagépen

1. Nyissa meg a **iSCSI-kezdeményező tulajdonságai** ablakban, a Windows Server-állomáson. Kattintson a **Kiszolgálókezelő > irányítópult > eszközök > iSCSI-kezdeményező**.
2. Az a **iSCSI-kezdeményező tulajdonságai** párbeszédpanel, kattintson a felderítés lapon, majd **tárolókapu felderítése**.
3. Az a **tárolókapu felderítése** párbeszédpanelen hajtsa végre az alábbi lépéseket:
   
   1. Adja meg az adatok port a StorSimple eszköz IP-címét (például írja be a DATA 0).
   2. Kattintson a **OK** való visszatéréshez a **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához.
     
     > [!IMPORTANT]
     > **Egy magánhálózaton használatakor az iSCSI-kapcsolatok adja meg az adatok portot, amelyet csatlakozik-e a magánhálózati IP-címét.**
    
4. Ismételje meg a 2-3 a második hálózati adapter (például az 1.) az eszközön. Ne feledje, hogy ezek az adapterek iSCSI engedélyezni kell. További információkért lásd: [módosítása a hálózati adapterek](storsimple-8000-modify-device-config.md#modify-network-interfaces).
5. Válassza ki a **célok** lapra a **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához. A StorSimple eszköz kell megjelennie az IQN cél **Felderített tárolók**.

   ![iSCSI kezdeményező tulajdonságai cél lap](./media/storsimple-configure-mpio-windows-server/IC741007.png)
   
6. Kattintson a **Connect** egy iSCSI-munkamenetet létrehozni a StorSimple eszköz számára. A **cél kapcsolódás** párbeszédpanel jelenik meg.
7. Az a **cél kapcsolódás** párbeszédpanelen jelölje ki a **engedélyezése többutas** jelölőnégyzetet. Kattintson a **speciális**.
8. Az a **speciális beállítások** párbeszédpanelen hajtsa végre az alábbi lépéseket:
   
   1. Az a **helyi Adapter** legördülő listában válassza **Microsoft iSCSI-kezdeményező**.
   2. Az a **kezdeményező IP** legördülő listára, válassza ki a gazdagép IP-címét.
   3. Az a **tárolókapu** IP legördülő listára, válassza ki az IP-címe eszköz felületén.
   4. Kattintson a **OK** való visszatéréshez a **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához.
9. Kattintson a **Tulajdonságok** elemre. Az a **tulajdonságok** párbeszédpanel, kattintson a **munkamenet hozzáadása**.
10. Az a **cél kapcsolódás** párbeszédpanelen jelölje ki a **engedélyezése többutas** jelölőnégyzetet. Kattintson a **speciális**.
11. Az a **speciális beállítások** párbeszédpanel:

    1. Az a **helyi adapter** legördülő listában válassza ki a Microsoft iSCSI-kezdeményező.
    2. Az a **kezdeményező IP** legördülő listára, válassza ki a gazdagép megfelelő IP-cím. Ebben az esetben az eszköz két hálózati adapterrel való csatlakozáskor az állomáson egyetlen hálózati adapterrel. Ezért ez az interfész, ugyanaz, mint az első munkamenete az előírt.
    3. Az a **cél portál IP-címet** legördülő listáról, válassza ki az IP-cím, a második adatok kapcsolat engedélyezve van az eszközön.
    4. Kattintson a **OK** való visszatéréshez az iSCSI-kezdeményező tulajdonságai párbeszédpanel megnyitásához. A cél hozzáadta a második munkamenet.
12. Nyissa meg **számítógép-kezelés** útvonalon **Kiszolgálókezelő > irányítópult > Számítógép-kezelés**. Kattintson a bal oldali ablaktáblában **tárolási > Lemezkezelés**. Megjelenik a kötet létrehozása a StorSimple eszközön, ez a gazdagép számára látható **Lemezkezelés** , új lemez(ek).
13. Végezze el a lemez inicializálását, és hozzon létre egy új kötetet. A formátum során válassza a 64 KB-os blokkméretet.
    
    ![Lemezkezelés](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. A **Lemezkezelés**, kattintson a jobb gombbal a **lemez** válassza **tulajdonságok**.
15. A StorSimple modell ### **többutas lemez eszköztulajdonságok** párbeszédpanel, kattintson a **MPIO** fülre.
    
    ![A StorSimple 8100 többutas lemez DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)
16. Az a **DSM neve** területén kattintson **részletek** , és ellenőrizze, hogy a paraméterek vannak-e beállítva az alapértelmezett paramétereket. Az alapértelmezett paraméterek a következők:
    
    * Elérési út ellenőrizze időszak = 30
    * Újbóli próbálkozások száma = 3
    * OEM eltávolítása időszak = 20
    * Újrapróbálkozási időköz = 1
    * Elérési út ellenőrzése engedélyezve = nincs bejelölve.

> [!NOTE]
> **Ne módosítsa az alapértelmezett paramétereket.**


## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>4. lépés: Az MPIO konfigurálása a magas rendelkezésre állás érdekében és terheléselosztás

Többutas alapú magas rendelkezésre állás és a terheléselosztás, több munkamenetet kell kézzel felvenni segítségével deklarálja azt az elérési útja eltér érhető el. Például ha a gazdagép két csatolót San hálózathoz csatlakoznak az eszköz felületei két San hálózathoz csatlakoznak, majd négy munkamenetek (csak két munkamenet lesz szükség, ha minden adatillesztő állomás felület egy másik IP-alhálózat és nem irányítható) megfelelő elérési kombinációinak konfigurálni kell.

**Azt javasoljuk, hogy rendelkezik-e legalább 8 aktív párhuzamos munkamenetek az eszköz és az alkalmazás-állomás között.** Ez a Windows Server rendszer csatolóinak 4 hálózati engedélyezésével elérhető. Használja a hálózati virtualizálási technológiák keresztül fizikai hálózati illesztőkre vagy virtuális a hardver vagy operációs rendszer szintjén, a Windows Server-állomáson. A két hálózati adapterrel az eszközön, az ebben a konfigurációban 8 aktív munkamenetek eredményezne. Ez a konfiguráció segít, hogy az eszköz és a felhő teljesítmény optimalizálása.

> [!IMPORTANT]
> **Azt javasoljuk, hogy azt ne keverje 1 gbe-s és a 10 GbE hálózati adapterrel. Ha két hálózati adaptert használ, a két felülethez a azonos típusúnak kellene lennie.**

Az alábbi eljárás ismerteti, hogyan hozzáadása munkamenetek két hálózati adapterrel egy olyan gazdagépre a két hálózati adapterrel a StorSimple eszköz csatlakoztatásakor. Ez lehetővé teszi csak 4 munkamenetek. A StorSimple eszköz négy hálózati adapterrel rendelkező a gazdagéphez csatlakoztatott két hálózati adapterrel rendelkező ugyanezt az eljárást használ. Szüksége lesz az itt leírt 4 munkamenetek helyett 8 konfigurálása.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>Az MPIO konfigurálása a magas rendelkezésre állás érdekében és terheléselosztás

1. Hajtsa végre a felderítés a cél: az a **iSCSI-kezdeményező tulajdonságai** párbeszédpanel a **felderítési** lapra, majd **kapu felderítése**.
2. Az a **cél kapcsolódás** párbeszédpanelen adja meg az IP-címét az egyik eszköz hálózati adapterhez.
3. Kattintson a **OK** való visszatéréshez a **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához.
4. Az a **iSCSI-kezdeményező tulajdonságai** párbeszédpanelen jelölje ki a **célok** fülre, jelölje ki a felderített célt, majd **Connect**. A **cél kapcsolódás** párbeszédpanel jelenik meg.
5. Az a **cél kapcsolódás** párbeszédpanel:
   
   1. Hagyja az alapértelmezett cél beállítás a kijelölt **adja hozzá ezt a kapcsolatot** a kedvenc tárolók listája. Így az eszköz automatikusan megpróbálja újraindítani a kapcsolat minden alkalommal, amikor a számítógép újraindul.
   2. Válassza ki a **engedélyezése többutas** jelölőnégyzetet.
   3. Kattintson a **speciális**.
6. Az a **speciális beállítások** párbeszédpanel:
   
   1. Az a **helyi Adapter** legördülő listában válassza **Microsoft iSCSI-kezdeményező**.
   2. Az a **kezdeményező IP** legördülő listára, válassza ki a gazdagép IP-címét.
   3. Az a **cél portál IP-címet** legördülő listában válassza az IP-címét az felületen engedélyezve van az eszközön.
   4. Kattintson a **OK** való visszatéréshez az iSCSI-kezdeményező tulajdonságai párbeszédpanel megnyitásához.
7. Kattintson a **tulajdonságok**, majd a a **tulajdonságok** párbeszédpanel, kattintson a **munkamenet hozzáadása**.
8. Az a **cél kapcsolódás** párbeszédpanelen jelölje ki a **engedélyezése többutas** jelölőnégyzetet, majd kattintson a **speciális**.
9. Az a **speciális beállítások** párbeszédpanel:
   
   1. Az a **helyi adapter** legördülő listában válassza **Microsoft iSCSI-kezdeményező**.
   2. Az a **kezdeményező IP** legördülő listára, válassza ki a második illesztőfelülete a gazdagép a megfelelő IP-cím.
   3. Az a **cél portál IP-címet** legördülő listáról, válassza ki az IP-cím, a második adatok kapcsolat engedélyezve van az eszközön.
   4. Kattintson a **OK** való visszatéréshez a **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához. Ezzel hozzáadta a második munkamenet a cél.
10. Adja hozzá a további munkamenetek (elérési út) a cél ismételje meg a 8 – 10. Két adapter áll rendelkezésükre a gazdagépen és a két az eszközön összesen négy munkamenetek is hozzáadhat.
11. A kívánt munkamenetek (elérési útja), a hozzáadása után a **iSCSI-kezdeményező tulajdonságai** párbeszédpanel mezőben jelölje ki a cél, és kattintson **tulajdonságok**. A munkamenetek lapján a **tulajdonságok** párbeszédpanel, vegye figyelembe a négy munkamenet a lehetséges útvonal kombinációinak megfelelő. A munkamenet megszakítása, jelölje be a munkamenet-azonosítót melletti jelölőnégyzetet, majd **Disconnect**.
12. Munkamenetek belül eszközök megtekintéséhez jelölje ki a **eszközök** fülre. A többutas I/O házirend a kiválasztott eszköz konfigurálásához kattintson **MPIO**. A **eszközadatok** párbeszédpanel jelenik meg. Az a **MPIO** lapon kiválaszthatja a megfelelő **terheléselosztási házirend** beállításait. Megtekintheti továbbá a **aktív** vagy **készenléti** elérési út típusa.

## <a name="next-steps"></a>Következő lépések

További információ [a StorSimple eszköz konfigurációs módosítja a StorSimple Device Manager szolgáltatás segítségével](storsimple-8000-modify-device-config.md).

