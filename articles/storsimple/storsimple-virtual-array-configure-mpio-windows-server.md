---
title: "gazdagép MPIO aaaConfigure csatlakoztatott virtuális tömb tooStorSimple |} Microsoft Docs"
description: "Ismerteti, hogyan tooconfigure többutas I/O (MPIO) a StorSimple virtuális tömb csatlakoztatva tooa gazdagépet, amelyen fut a Windows Server 2012 R2."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5b7a7f99-ee5b-4b7d-ab32-483a5a1fa504
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/01/2017
ms.author: alkohli
ms.openlocfilehash: 0e6df23bba29395329685cbf2c968675abb04cfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multipath-io-on-windows-server-host-for-hello-storsimple-virtual-array"></a>Többutas I/O konfigurálása Windows Server-gazdagépen hello StorSimple virtuális tömb
## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan tooinstall többutas I/O szolgáltatást (MPIO) a Windows Server-állomáson, csak a StorSimple-kötetek megadott konfigurációs beállítások alkalmazásához, és ellenőrizze a többutas I/O a StorSimple-köteteket. hello eljárás azt feltételezi, hogy a StorSimple 1200 két hálózati adapterrel rendelkező virtuális tömb tooa csatlakoztatott két hálózati adapterrel rendelkező Windows Server-állomáson. Ebben a cikkben szereplő hello információ csak toohello virtuális tömb érvényes. Információ a StorSimple 8000 sorozat eszközeire: túl[StorSimple gazdagép MPIO konfigurálása](storsimple-configure-mpio-windows-server.md). 

Windows Server segítségével build magas rendelkezésre állású, és hibatűrő tárolási konfigurációk hello többutas I/O szolgáltatása. Az MPIO használ a redundáns összetevők – adapterek, kábelek és kapcsolók – toocreate hello kiszolgáló és a hello tárolóeszköz közötti logikai útvonalak. Egy összetevő meghibásodása esetén, amely a logikai elérési utat toofail többutas logika egy alternatív útvonalat használ az i/o, hogy az alkalmazások továbbra is hozzáférjenek az adataikhoz. Emellett a konfigurációtól függően az MPIO is a jobb teljesítmény érdekében újra hello terheléselosztás közötti összes elérési utat. További információkért lásd: [MPIO áttekintése](https://technet.microsoft.com/library/cc725907.aspx "MPIO áttekintése és a szolgáltatások").

Hello magas rendelkezésre állású a StorSimple megoldás, a Windows Server-gazdagépek csatlakoztatott tooyour StorSimple virtuális tömb (1200-as modell) hello MPIO konfigurálását. hello kiszolgálók majd tűri egy hivatkozás, hálózati vagy adapterhibákat. 

Ezen lépések tooconfigure MPIO toofollow lesz szüksége: 

* Konfigurációs előfeltételek
* 1. lépés: Telepítse a hello Windows Server rendszerű gazdagép MPIO
* 2. lépés: Az MPIO konfigurálása a StorSimple-köteteket
* 3. lépés: Csatlakoztatási StorSimple-köteteket hello gazdagépen

A következő szakaszok hello egyes hello a fenti lépéseket ismertet.

## <a name="prerequisites"></a>Előfeltételek
Ez a szakasz részletesen hello konfigurációs előfeltételeit hello Windows Server rendszerű gazdagép és a virtuális tömbjét.

### <a name="on-windows-server-host"></a>Windows Server-állomáson
* Győződjön meg arról, hogy a Windows Server-gazdagép 2 hálózati csatolót engedélyezni kell.

### <a name="on-storsimple-virtual-array"></a>A StorSimple virtuális tömb
* hello virtuális tömbhöz az iSCSI-kiszolgálóként kell konfigurálni. több, lásd: toolearn [állítsa be az iSCSI-kiszolgálóként virtuális tömb](storsimple-virtual-array-deploy3-iscsi-setup.md). Egy vagy több hálózati illesztőre hello tömb engedélyezni kell.   
* a virtuális tömb hello hálózati adapterek hello Windows Server gazdagépről elérhetőnek kell lennie.
* Egy vagy több kötetet kell létrehozni a StorSimple virtuális tömbjét. több, lásd: toolearn [kötet hozzáadása](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume) a StorSimple virtuális tömbben. Az eljárás 3 kötetek (1 helyileg rögzített és 2 rétegzett kötetek látható az alábbi) hello virtuális tömb létrehozott.
  
    ![mpio0](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>A StorSimple virtuális tömb hardverkonfiguráció
hello az alábbi ábra magas rendelkezésre állású hello hardverkonfigurációja és többutas terheléselosztás a Windows Server rendszerű gazdagép és a StorSimple virtuális tömb használja az alábbi eljárással.

![az MPIO hardverkonfiguráció](./media/storsimple-virtual-array-configure-mpio-windows-server/1200hardwareconfig.png)

Ahogy az ábra megelőző hello:

* A StorSimple virtuális tömb kiépítve a Hyper-V az iSCSI-kiszolgálóként konfigurált egycsomópontos aktív eszköz,
* A tömb két virtuális hálózati adaptere engedélyezve vannak. Hello helyi webes felhasználói felületén a virtuális tömb, győződjön meg arról, hogy két hálózati adapterrel engedélyezve vannak-e túl navigálva**hálózati beállítások** alább látható módon:
  
    ![Hálózati illesztők 1200 engedélyezve](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio9.png)
  
    Megjegyzés hello IPv4 cím hello engedélyezve van a hálózati illesztőt (Ethernet, Ethernet 2 alapértelmezés szerint), és mentse hello gazdagépen későbbi használatra.
* Két hálózati adapterrel engedélyezve vannak a Windows Server-állomáson. Ha hello csatlakoztatott adaptert használjon a gazdagépen és a tömb vannak-e a hello ugyanazon az alhálózaton, akkor nem lesznek 4 útvonal érhető el. Az alábbi eljárással hello eset volt. Azonban ha mindegyik hálózati interfész hello tárolótömböt és az állomás illesztőn a különböző IP-alhálózat (és nem irányítható), majd csak 2 elérési utak lesz elérhető.

## <a name="step-1-install-mpio-on-hello-windows-server-host"></a>1. lépés: Telepítse a hello Windows Server rendszerű gazdagép MPIO
Az MPIO választható lehetőség a Windows Server, és alapértelmezés szerint nincs telepítve. Szolgáltatásként lehet telepíteni a Kiszolgálókezelőn keresztül. tooinstall ezt a funkciót a Windows Server-állomáson, végezze el az eljárás következő hello.

[!INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>2. lépés: Az MPIO konfigurálása a StorSimple-köteteket
Az MPIO kell konfigurált toobe tooidentify StorSimple-köteteket. tooconfigure MPIO toorecognize StorSimple-köteteket, hajtsa végre a lépéseket követve hello.

[!INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-hello-host"></a>3. lépés: Csatlakoztatási StorSimple-köteteket hello gazdagépen
Az MPIO konfigurálása a Windows Server után létrehozott hello StorSimple tömb kötet(ek) lehet csatlakoztatni, és majd kihasználhatja az MPIO a redundancia érdekében. toomount egy köteten, hajtsa végre a lépéseket követve hello.

#### <a name="toomount-volumes-on-hello-host"></a>hello gazdagépen toomount kötetek
1. Nyissa meg hello **iSCSI-kezdeményező tulajdonságai** ablak hello Windows Server-állomáson. Nyissa meg túl**Kiszolgálókezelő > irányítópult > eszközök > iSCSI-kezdeményező**.
2. A hello **iSCSI-kezdeményező tulajdonságai** párbeszédpanel, kattintson a **felderítési**, és kattintson a **tárolókapu felderítése**.
3. A hello **tárolókapu felderítése** párbeszédpanel mezőbe hello a következő:
   
    1. Hello első engedélyezett hálózati interfész hello IP-címét adja meg a StorSimple virtuális tömbjét. Alapértelmezés szerint ez lenne **Ethernet**. 
    2. Kattintson a **OK** tooreturn toohello **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához.
     
    > [!IMPORTANT]
    > Egy magánhálózaton használatakor az iSCSI-kapcsolatok adja meg a hello hello adatok portot, amelyet csatlakoztatott toohello magánhálózati IP-címét.
     
4. Ismételje meg a 2-3 a második hálózati adapter (például Ethernet 2) a tömb. 
5. Jelölje be hello **célok** hello lapján **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához. A virtuális tömb kell megjelennie minden kötet felületet a cél **Felderített tárolók**. Ebben az esetben a három célok (megfelelő toothree kötetek) felderített lenne.
   
    ![mpio1](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio1.png)
6. Kattintson a **Connect** tooestablish egy iSCSI-munkamenetet a StorSimple virtuális tömbbel rendelkező. A **tooTarget csatlakozás** párbeszédpanel jelenik meg. Jelölje be hello **engedélyezése többutas** jelölőnégyzetet. Kattintson a **speciális**.
   
    ![mpio2](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio2.png)
7. A hello **speciális beállítások** párbeszédpanel mezőbe hello a következő:                                        
   
    1. A hello **helyi Adapter** legördülő listában válassza **Microsoft iSCSI-kezdeményező**.
    2. A hello **kezdeményező IP** legördülő listából válassza ki, jelölje be hello hello állomás IP-címét.
    3. A hello **tárolókapu** IP-legördülő listájában válassza hello IP tömb illesztőfelület.
    4. Kattintson a **OK** tooreturn toohello **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához.
     
        ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

8. Kattintson a **Tulajdonságok** elemre. 
   
    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)

9. A hello **tulajdonságok** párbeszédpanel, kattintson a **munkamenet hozzáadása**.
   
   ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)
10. A hello **tooTarget csatlakozás** párbeszédpanel megnyitásához, jelölje be hello **engedélyezése többutas** jelölőnégyzetet. Kattintson a **speciális**.
11. A hello **speciális beállítások** párbeszédpanel:                                        
    
    1. A hello **helyi adapter** legördülő listában válassza ki a Microsoft iSCSI-kezdeményező.

    2. A hello **kezdeményező IP** legördülő listában, jelölje be hello IP-cím megfelelő toohello állomás. Ebben az esetben két hálózati adapterrel hello tömb tooa egyetlen hálózati adapteren hello gazdagépen csatlakozik. Ezért ez az interfész van hello ugyanaz, mint az első munkamenet hello előírt.

    3. A hello **cél portál IP-címet** legördülő listából válassza ki, az IP-címet válassza hello hello második adatillesztő hello tömb engedélyezve.

    4. Kattintson a **OK** tooreturn toohello iSCSI-kezdeményező tulajdonságai párbeszédpanel. Hozzáadta a második munkamenet toohello cél.
      
       ![mpio11](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio11.png)
    
    5. A hello hello szükséges munkamenetek (az elérési utak) hozzáadása után **iSCSI-kezdeményező tulajdonságai** párbeszédpanel megnyitásához, válassza hello cél, és kattintson **tulajdonságok**. Hello munkamenetek lapján hello **tulajdonságok** Megjegyzés hello négy munkamenet azonosítók, amelyek megfelelnek a toohello elérési lehetséges kombinációinak párbeszédpanelen. toocancel-munkamenet, válassza ki a hello jelölőnégyzetet következő tooa munkamenet-azonosítót, és kattintson a **Disconnect**.

    6. jelenik meg a munkamenetek, jelölje be hello belül tooview eszközök **eszközök** külön-külön tooconfigure hello többutas I/O házirend a kiválasztott eszköz kattintson **MPIO**.

    7. Hello **részletek** párbeszédpanel jelenik meg. A hello **MPIO** lapon kiválaszthatja a megfelelő hello **terheléselosztási házirend** beállításait. Emellett megtekinthet különböző hello **aktív** vagy **készenléti** elérési út típusa.
12. Ismételje meg a 8 – 11. lépéseket tooadd további munkamenetek (elérési út) toohello cél. Két csatolót hello gazdagépen és a két hello virtuális tömbben összesen négy munkamenetek mindegyik tárolóhoz is hozzáadhat. 
    
    ![mpio14](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio14.png)
13. Szüksége lesz toorepeat ezeket a lépéseket minden olyan kötetre (cél felületek).
    
    ![mpio15](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio15.png)
14. Nyissa meg **számítógép-kezelés** túl navigálva**Kiszolgálókezelő > irányítópult > Számítógép-kezelés**. Hello bal oldali ablaktáblában kattintson **tárolási > Lemezkezelés**. hello létrehozni a StorSimple virtuális tömb hello kötetek, amelyek látható toothis gazdagép fog megjelenni a **Lemezkezelés** , új lemez(ek).
15. Hello lemez inicializálása, és hozzon létre egy új kötetet. Hello formátum folyamat során válassza ki az foglalásiegység-méret (Ausztráliai) 64 KB. Ismételje meg a műveletet hello hello összes rendelkezésre álló köteteken.
    
    ![Lemezkezelés](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio20.png)
16. A **Lemezkezelés**, kattintson a jobb gombbal hello **lemez** válassza **tulajdonságok**.
17. A hello **többutas lemez eszköztulajdonságok** párbeszédpanel hello kattintson **MPIO** fülre.
    
    ![Lemeztulajdonságok](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio21.png)
18. A hello **DSM neve** kattintson **részletek** , és ellenőrizze, hogy hello paraméterek toohello alapértelmezett paraméterek beállítása. hello alapértelmezett paraméterek a következők:
    
    * Elérési út ellenőrizze időszak = 30
    * Újbóli próbálkozások száma = 3
    * OEM eltávolítása időszak = 20
    * Újrapróbálkozási időköz = 1
    * Elérési út ellenőrzése engedélyezve = nincs bejelölve.
    
    > [!NOTE]
    > **Ne módosítsa a hello alapértelmezett paraméterek.**
   
## <a name="next-steps"></a>Következő lépések
További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple virtuális tömb](storsimple-virtual-array-manager-service-administration.md).

