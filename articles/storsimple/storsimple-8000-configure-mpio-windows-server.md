---
title: "a StorSimple eszköz MPIO aaaConfigure |} Microsoft Docs"
description: "Ismerteti, hogyan tooconfigure többutas I/O (MPIO) a StorSimple eszköz csatlakoztatva tooa gazdagépet, amelyen fut a Windows Server 2012 R2."
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
ms.openlocfilehash: 7796524edc739826ba1e977161fc9988c6d316e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multipath-io-for-your-storsimple-device"></a>Többutas I/O konfigurálása a StorSimple eszköz

Ez az oktatóanyag leírja hello lépéseket kell követni tooinstall, és hello többutas I/O (MPIO) szolgáltatást használja a Windows Server 2012 R2 és a csatlakoztatott tooa StorSimple fizikai eszköz rendszerű gazdagépen. hello útmutató tooStorSimple 8000 sorozat fizikai eszközökre vonatkozik. Az MPIO jelenleg nem támogatott a StorSimple-felhő készüléken.

Microsoft hello többutas I/O (MPIO) szolgáltatást a Windows Server toohelp támogatása beépített magas rendelkezésre állású, nagy hibatűrésű SAN-konfigurációk létrehozása. Az MPIO használ a redundáns összetevők – adapterek, kábelek és kapcsolók – toocreate hello kiszolgáló és a hello tárolóeszköz közötti logikai útvonalak. Egy összetevő meghibásodása esetén, amely a logikai elérési utat toofail többutas logika egy alternatív útvonalat használ az i/o, hogy az alkalmazások továbbra is hozzáférjenek az adataikhoz. Emellett a konfigurációtól függően az MPIO is a jobb teljesítmény érdekében hello terhelés újraelosztás közötti összes elérési utat. További információkért lásd: [MPIO áttekintése](https://technet.microsoft.com/library/cc725907.aspx "MPIO áttekintése és a szolgáltatások").

Hello magas rendelkezésre állású a StorSimple megoldás, a többutas I/O konfigurálnia kell a StorSimple eszközt. Ha az MPIO telepítve van a Windows Server 2012 R2 rendszert futtató kiszolgálók, hello kiszolgálók majd tűri egy hivatkozás, hálózati vagy adapterhibákat.

## <a name="mpio-configuration-summary"></a>Az MPIO konfigurációja összefoglaló

Az MPIO választható lehetőség a Windows Server, és alapértelmezés szerint nincs telepítve. Szolgáltatásként lehet telepíteni a Kiszolgálókezelőn keresztül.

Kövesse ezeket a lépéseket tooconfigure MPIO a StorSimple eszköz:

* 1. lépés: Telepítse a hello Windows Server rendszerű gazdagép MPIO
* 2. lépés: Az MPIO konfigurálása a StorSimple-köteteket
* 3. lépés: Csatlakoztatási StorSimple-köteteket hello gazdagépen
* 4. lépés: Az MPIO konfigurálása a magas rendelkezésre állás érdekében és terheléselosztás

A következő szakaszok hello minden előző lépésekben hello ismertet.

## <a name="step-1-install-mpio-on-hello-windows-server-host"></a>1. lépés: Telepítse a hello Windows Server rendszerű gazdagép MPIO

tooinstall ezt a funkciót a Windows Server-állomáson, végezze el az eljárás következő hello.

#### <a name="tooinstall-mpio-on-hello-host"></a>hello gazdagép MPIO tooinstall

1. Nyissa meg a Kiszolgálókezelőt a Windows Server-állomáson. Alapértelmezés szerint a Kiszolgálókezelő hello rendszergazdák csoport tagjának bejelentkezésekor a Windows Server 2012 R2 vagy Windows Server 2012 rendszert futtató számítógép tooa elindul. Ha a Kiszolgálókezelő hello még nincs megnyitva, kattintson a **Start > Kiszolgálókezelő**.
   
   ![Kiszolgálókezelő](./media/storsimple-configure-mpio-windows-server/IC740997.png)

2. Kattintson a **Kiszolgálókezelő > irányítópult > szerepkörök és szolgáltatások hozzáadása**. Ekkor elindul a hello **szerepkörök és szolgáltatások hozzáadása** varázsló.
   
   ![Szerepkörök és szolgáltatások hozzáadása varázsló 1](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. A hello **szerepkörök és szolgáltatások hozzáadása** varázsló, hajtsa végre az alábbi lépésekkel hello:
   
   1. A hello **megkezdése előtt** kattintson **következő**.
   2. A hello **telepítés típusának kiválasztása** lap, fogadja el az alapértelmezett beállítását hello **szerepköralapú vagy szolgáltatásalapú** telepítését. Kattintson a **Tovább** gombra.
   
       ![Szerepkörök és szolgáltatások hozzáadása varázsló 2. régiója](./media/storsimple-configure-mpio-windows-server/IC740999.png)
   3. A hello **jelölje be a célkiszolgáló** lapon, válassza ki **jelöljön ki egy kiszolgálót hello kiszolgálókészletből**. A gazdagép-kiszolgálón kell automatikusan észlelhetők. Kattintson a **Tovább** gombra.
   4. A hello **kiszolgálói szerepkörök kiválasztása** kattintson **következő**.
   5. A hello **szolgáltatások kiválasztása** lapon, hogy melyik **többutas i/o**, és kattintson a **következő**.
   
       ![Szerepkörök és szolgáltatások hozzáadása varázsló 5](./media/storsimple-configure-mpio-windows-server/IC741000.png)
   6. A hello **erősítse meg a telepítendő** lapon ellenőrizze a hello kijelölést, majd válassza ki **újraindítás hello célkiszolgáló szükség esetén automatikusan**lent látható módon. Kattintson az **Install** (Telepítés) gombra.
   
       ![Szerepkörök és szolgáltatások hozzáadása varázsló 8](./media/storsimple-configure-mpio-windows-server/IC741001.png)
   7. Értesítést kap hello telepítés befejeződött. Kattintson a **Bezárás** tooclose hello varázsló.
   
       ![Szerepkörök és szolgáltatások hozzáadása varázsló 9](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>2. lépés: Az MPIO konfigurálása a StorSimple-köteteket

Az MPIO konfigurált tooidentify StorSimple-köteteket kell lennie. tooconfigure MPIO toorecognize StorSimple-köteteket, hajtsa végre a lépéseket követve hello.

#### <a name="tooconfigure-mpio-for-storsimple-volumes"></a>a StorSimple-köteteket MPIO tooconfigure

1. Nyissa meg hello **az MPIO konfigurációja**. Kattintson a **Kiszolgálókezelő > irányítópult > eszközök > MPIO**.
2. A hello **többutas I/O tulajdonságai** párbeszédpanel megnyitásához, jelölje be hello **felderítése többutas** fülre.
3. Válassza ki **az iSCSI-eszközök támogatását**, és kattintson a **Hozzáadás**.  
   ![Többutas I/O tulajdonságai többféle elérési utak felderítése](./media/storsimple-configure-mpio-windows-server/IC741003.png)
4. Amikor a rendszer kéri hello kiszolgáló újraindul.
5. A hello **többutas I/O tulajdonságai** párbeszédpanelen kattintson hello **MPIO eszközök** lapon. Kattintson az **Hozzáadás** parancsra.
    </br>![Az MPIO tulajdonságok többutas I/O eszköz](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. A hello **MPIO-támogatás hozzáadása** párbeszédpanel **eszköz Hardverazonosító**, adja meg az eszköz sorozatszámát. tooget hello eszköz serial number, hozzáférés a StorSimple eszköz Manager szolgáltatást. Keresse meg a túl**eszközök > irányítópult**. hello eszközsorozatszámot megjelennek a jobb oldali hello **gyors áttekintése** hello eszköz irányítópult panelén.
    </br>
    ![Az MPIO támogatását](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Amikor a rendszer kéri hello kiszolgáló újraindul.

## <a name="step-3-mount-storsimple-volumes-on-hello-host"></a>3. lépés: Csatlakoztatási StorSimple-köteteket hello gazdagépen

Az MPIO konfigurálása a Windows Server után létrehozott hello StorSimple eszközön kötet(ek) lehet csatlakoztatni, és majd kihasználhatja az MPIO a redundancia érdekében. toomount egy köteten, hajtsa végre a lépéseket követve hello.

#### <a name="toomount-volumes-on-hello-host"></a>hello gazdagépen toomount kötetek

1. Nyissa meg hello **iSCSI-kezdeményező tulajdonságai** ablak hello Windows Server-állomáson. Kattintson a **Kiszolgálókezelő > irányítópult > eszközök > iSCSI-kezdeményező**.
2. A hello **iSCSI-kezdeményező tulajdonságai** párbeszédpanelen kattintson hello felderítés lapon, majd **tárolókapu felderítése**.
3. A hello **tárolókapu felderítése** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
   1. Adja meg a hello hello adatok port a StorSimple eszköz IP-címét (például írja be a DATA 0).
   2. Kattintson a **OK** tooreturn toohello **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához.
     
     > [!IMPORTANT]
     > **Egy magánhálózaton használatakor az iSCSI-kapcsolatok adja meg a hello hello adatok portot, amelyet csatlakoztatott toohello magánhálózati IP-címét.**
    
4. Ismételje meg a 2-3 a második hálózati adapter (például az 1.) az eszközön. Ne feledje, hogy ezek az adapterek iSCSI engedélyezni kell. További információkért lásd: [módosítása a hálózati adapterek](storsimple-8000-modify-device-config.md#modify-network-interfaces).
5. Jelölje be hello **célok** hello lapján **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához. Hello StorSimple eszközt kell megjelennie az IQN cél **Felderített tárolók**.

   ![iSCSI kezdeményező tulajdonságai cél lap](./media/storsimple-configure-mpio-windows-server/IC741007.png)
   
6. Kattintson a **Connect** tooestablish egy iSCSI-munkamenetet a StorSimple eszközhöz. A **tooTarget csatlakozás** párbeszédpanel jelenik meg.
7. A hello **tooTarget csatlakozás** párbeszédpanel megnyitásához, jelölje be hello **engedélyezése többutas** jelölőnégyzetet. Kattintson a **speciális**.
8. A hello **speciális beállítások** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
   1. A hello **helyi Adapter** legördülő listában válassza **Microsoft iSCSI-kezdeményező**.
   2. A hello **kezdeményező IP** legördülő listából válassza ki, jelölje be hello hello állomás IP-címét.
   3. A hello **tárolókapu** IP-legördülő listájában válassza hello IP-eszköz kapcsolat.
   4. Kattintson a **OK** tooreturn toohello **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához.
9. Kattintson a **Tulajdonságok** elemre. A hello **tulajdonságok** párbeszédpanel, kattintson a **munkamenet hozzáadása**.
10. A hello **tooTarget csatlakozás** párbeszédpanel megnyitásához, jelölje be hello **engedélyezése többutas** jelölőnégyzetet. Kattintson a **speciális**.
11. A hello **speciális beállítások** párbeszédpanel:

    1. A hello **helyi adapter** legördülő listában válassza ki a Microsoft iSCSI-kezdeményező.
    2. A hello **kezdeményező IP** legördülő listában, jelölje be hello IP-cím megfelelő toohello állomás. Ebben az esetben két hálózati adapterrel hello eszköz tooa egyetlen hálózati adapteren hello gazdagépen csatlakozik. Ezért ez az interfész van hello ugyanaz, mint az első munkamenet hello előírt.
    3. A hello **cél portál IP-címet** legördülő listából válassza ki, jelölje be hello IP-címet hello második adatillesztő hello eszközön engedélyezve van.
    4. Kattintson a **OK** tooreturn toohello iSCSI-kezdeményező tulajdonságai párbeszédpanel. Hozzáadta a második munkamenet toohello cél.
12. Nyissa meg **számítógép-kezelés** túl navigálva**Kiszolgálókezelő > irányítópult > Számítógép-kezelés**. Hello bal oldali ablaktáblában kattintson **tárolási > Lemezkezelés**. hello StorSimple eszközön, amelyek látható toothis állomás létrehozott hello kötet jelenik meg az **Lemezkezelés** , új lemez(ek).
13. Hello lemez inicializálása, és hozzon létre egy új kötetet. Hello formátum folyamat során válassza a 64 KB-os blokkméretet.
    
    ![Lemezkezelés](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. A **Lemezkezelés**, kattintson a jobb gombbal hello **lemez** válassza **tulajdonságok**.
15. A StorSimple modell hello ### **többutas lemez eszköztulajdonságok** párbeszédpanel hello kattintson **MPIO** fülre.
    
    ![A StorSimple 8100 többutas lemez DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)
16. A hello **DSM neve** kattintson **részletek** , és ellenőrizze, hogy hello paraméterek toohello alapértelmezett paraméterek beállítása. hello alapértelmezett paraméterek a következők:
    
    * Elérési út ellenőrizze időszak = 30
    * Újbóli próbálkozások száma = 3
    * OEM eltávolítása időszak = 20
    * Újrapróbálkozási időköz = 1
    * Elérési út ellenőrzése engedélyezve = nincs bejelölve.

> [!NOTE]
> **Ne módosítsa a hello alapértelmezett paraméterek.**


## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>4. lépés: Az MPIO konfigurálása a magas rendelkezésre állás érdekében és terheléselosztás

A több utas-alapú magas rendelkezésre állás és a terheléselosztás, több munkamenetet kell kézzel felvenni toodeclare hello elérési útja eltér érhető el. Például ha hello gazdagép csatlakoztatott két csatolót tooSAN és hello eszköznek két felületek csatlakoztatott tooSAN, négy munkamenetek (csak két munkamenet lesz szükség minden egyes felületen és a gazdagép felület esetén megfelelő elérési kombinációinak konfigurálni kell használnia egy másik IP-alhálózat, és nem irányítható).

**Azt javasoljuk, hogy rendelkezik-e legalább 8 aktív párhuzamos munkamenetek hello eszköz és az alkalmazás-állomás között.** Ez a Windows Server rendszer csatolóinak 4 hálózati engedélyezésével elérhető. Használja a hálózati virtualizálási technológiák keresztül fizikai hálózati illesztőkre vagy virtuális hello hardver vagy operációs rendszer szintjén a Windows Server-állomáson. Ez a konfiguráció hello két hálózati adapterrel hello eszközön, a 8 aktív munkamenetek eredményezne. Ez a konfiguráció segít hello eszköz és a felhő teljesítmény optimalizálása.

> [!IMPORTANT]
> **Azt javasoljuk, hogy azt ne keverje 1 gbe-s és a 10 GbE hálózati adapterrel. Ha két hálózati adaptert használ, a két felülethez hello azonos típusúnak kellene lennie.**

hello alábbi eljárás ismerteti, hogyan tooadd munkamenetek tooa csatlakoztatott két hálózati adapterrel rendelkező StorSimple eszköz esetén a két hálózati adapterrel rendelkező állomásra. Ez lehetővé teszi csak 4 munkamenetek. A StorSimple eszköz két hálózati illesztők csatlakoztatott tooa gazdagéphez négy hálózati adapterrel rendelkező ugyanezt az eljárást használhatja. Az itt ismertetett 4 munkamenetek kell tooconfigure 8 hello helyett.

### <a name="tooconfigure-mpio-for-high-availability-and-load-balancing"></a>az MPIO tooconfigure magas rendelkezésre állású és terheléselosztás

1. Hello válaszol-e a felderítés végrehajtása érdekében: a hello **iSCSI-kezdeményező tulajdonságai** párbeszédpanel hello **felderítési** lapra, majd **kapu felderítése**.
2. A hello **tooTarget csatlakozás** párbeszédpanelen adja meg a hello IP-cím hello eszköz hálózati adapterek egyikének.
3. Kattintson a **OK** tooreturn toohello **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához.
4. A hello **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához, jelölje be hello **célok** lapra, jelölje ki a felderített hello célként, és kattintson **Connect**. Hello **tooTarget csatlakozás** párbeszédpanel jelenik meg.
5. A hello **tooTarget csatlakozás** párbeszédpanel:
   
   1. Hagyja hello alapértelmezett cél beállítás a kijelölt **adja hozzá ezt a kapcsolatot** toohello kedvenc tárolók listája. Így hello eszköz automatikusan megkísérelt toorestart hello kapcsolat, minden alkalommal, amikor a számítógép újraindul.
   2. Jelölje be hello **engedélyezése többutas** jelölőnégyzetet.
   3. Kattintson a **speciális**.
6. A hello **speciális beállítások** párbeszédpanel:
   
   1. A hello **helyi Adapter** legördülő listában válassza **Microsoft iSCSI-kezdeményező**.
   2. A hello **kezdeményező IP** legördülő listából válassza ki, jelölje be hello hello állomás IP-címét.
   3. A hello **cél portál IP-címet** legördülő listából válassza ki, jelölje be hello IP-címe hello adatillesztő hello eszközön engedélyezve van.
   4. Kattintson a **OK** tooreturn toohello iSCSI-kezdeményező tulajdonságai párbeszédpanel.
7. Kattintson a **tulajdonságok**, és a hello **tulajdonságok** párbeszédpanel, kattintson a **munkamenet hozzáadása**.
8. A hello **tooTarget csatlakozás** párbeszédpanel megnyitásához, jelölje be hello **engedélyezése többutas** jelölőnégyzetet, majd kattintson a **speciális**.
9. A hello **speciális beállítások** párbeszédpanel:
   
   1. A hello **helyi adapter** legördülő listában válassza **Microsoft iSCSI-kezdeményező**.
   2. A hello **kezdeményező IP** legördülő listában, jelölje be hello IP-cím megfelelő toohello második összeköttetés hello gazdagépen.
   3. A hello **cél portál IP-címet** legördülő listából válassza ki, jelölje be hello IP-címet hello második adatillesztő hello eszközön engedélyezve van.
   4. Kattintson a **OK** tooreturn toohello **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához. Ezzel hozzáadta a második munkamenet toohello cél.
10. Ismételje meg a 8 – 10. lépéseket tooadd további munkamenetek (elérési út) toohello cél. A két csatolót hello gazdagépen és a két hello eszközön összesen négy munkamenetek is hozzáadhat.
11. A hello hello szükséges munkamenetek (az elérési utak) hozzáadása után **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához, válassza hello cél, és kattintson **tulajdonságok**. Hello munkamenetek lapján hello **tulajdonságok** Megjegyzés hello négy munkamenet azonosítók, amelyek megfelelnek a toohello elérési lehetséges kombinációinak párbeszédpanelen. toocancel-munkamenet, válassza ki a hello jelölőnégyzetet következő tooa munkamenet-azonosítót, és kattintson a **Disconnect**.
12. jelenik meg a munkamenetek, jelölje be hello belül tooview eszközök **eszközök** külön-külön tooconfigure hello többutas I/O házirend a kiválasztott eszköz kattintson **MPIO**. Hello **eszközadatok** párbeszédpanel jelenik meg. A hello **MPIO** lapon kiválaszthatja a megfelelő hello **terheléselosztási házirend** beállításait. Emellett megtekinthet különböző hello **aktív** vagy **készenléti** elérési út típusa.

## <a name="next-steps"></a>Következő lépések

További információ [használatával hello StorSimple Device Manager szolgáltatás toomodify a StorSimple eszköz konfigurációs](storsimple-8000-modify-device-config.md).

