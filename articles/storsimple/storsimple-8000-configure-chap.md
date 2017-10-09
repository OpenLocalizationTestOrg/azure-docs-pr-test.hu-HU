---
title: "a StorSimple 8000 series eszköz CHAP aaaConfigure |} Microsoft Docs"
description: "Ismerteti, hogyan tooconfigure hello a StorSimple eszközön Challenge Handshake Authentication Protocol (CHAP)."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 3351184b0317da7e3deae398bc0d63c3e5bd930f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a>A CHAP konfigurálása a StorSimple eszköz

Ez az oktatóanyag azt ismerteti, hogyan tooconfigure CHAP a StorSimple eszközhöz. Ebben a cikkben ismertetett hello eljárás érvényes tooStorSimple 8000 sorozat eszközeire.

A CHAP hitelesítési protokoll jelenti. Olyan kiszolgálók toovalidate hello identitásának távoli ügyfelek által használt hitelesítési sémát. hello ellenőrzési megosztott jelszó vagy titkos kulcs alapul. A CHAP biztosítható egy (egyirányú) vagy a kölcsönös (kétirányú). Egyirányú CHAP esetén hello cél hitelesíti az kezdeményező. A kölcsönös vagy fordított CHAP hello cél hitelesíti hello kezdeményező, és hogy hello kezdeményező majd hitelesíti a hello cél. Kezdeményező hitelesítés nélkül cél hitelesítés valósítható meg. Cél hitelesítési esetén alkalmazhatóak azonban csak akkor, ha a kezdeményező hitelesítési is tartalmazzák.

Ajánlott eljárásként azt javasoljuk, hogy használja-e CHAP tooenhance iSCSI biztonsági.

> [!NOTE]
> Ne feledje, hogy IPSEC jelenleg nem támogatott a StorSimple eszközökhöz.

a következő módokon hello hello CHAP beállításokat hello StorSimple eszközön konfigurálható:

* Egyirányú vagy egyirányú hitelesítés
* Kétirányú vagy fordított vagy kölcsönös hitelesítés

Az egyes ezekben az esetekben hello portal hello eszköz és hello server iSCSI-kezdeményező szoftver kell toobe konfigurálva. hello részletes leírást ehhez a konfigurációhoz részelemcímkék ismertetését a következő oktatóanyag hello.

## <a name="unidirectional-or-one-way-authentication"></a>Egyirányú vagy egyirányú hitelesítés

Az egyirányú hitelesítési hello cél hello kezdeményező hitelesíti. A hitelesítéshez, hogy a beállításokat hello CHAP kezdeményező hello StorSimple eszköz és hello iSCSI-kezdeményező szoftver hello gazdagépen. hello részletesen ismertetik a StorSimple eszköz, és ezután ismerteti a Windows-állomás.

#### <a name="tooconfigure-your-device-for-one-way-authentication"></a>tooconfigure az eszköz egyirányú hitelesítéshez

1. A hello Azure-portálon válassza a tooyour StorSimple Device Manager szolgáltatást. Kattintson a **eszközök** , és válassza ki, és kattintson arra az eszközre tooconfigure CHAP kívánja a. Nyissa meg túl**eszközbeállítások > biztonsági**. A hello **biztonsági beállítások** panelen kattintson a **CHAP**.
   
    ![A CHAP-kezdeményező](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. A hello **CHAP** panelt, és a hello **CHAP-kezdeményező** szakasz:
   
   1. Adjon meg egy felhasználónevet a CHAP-kezdeményező.
   2. Adjon meg egy jelszót a CHAP-kezdeményező.
      
    > [!IMPORTANT]
    > hello CHAP-felhasználónév kevesebb mint 233 karaktert kell tartalmaznia. hello CHAP-jelszó 12 és 16 karakter hosszúságú lehet. Hosszabb felhasználónevet vagy jelszót hello Windows gazdagépen hitelesítési hibát eredményez.
   
   3. Hello jelszót.

       ![A CHAP-kezdeményező](./media/storsimple-8000-configure-chap/configure-chap6.png)
3. Kattintson a **Save** (Mentés) gombra. Egy megerősítő üzenet jelenik meg. Kattintson a **OK** toosave hello módosításokat.

#### <a name="tooconfigure-one-way-authentication-on-hello-windows-host-server"></a>hello Windows-hitelesítés egyirányú tooconfigure gazdagép-kiszolgálón
1. A Windows hello gazdakiszolgálón indítsa el a hello iSCSI-kezdeményező.
2. A hello **iSCSI-kezdeményező tulajdonságai** ablak, hajtsa végre az alábbi lépésekkel hello:
   
   1. Kattintson a hello **felderítési** fülre.
      
       ![iSCSI-kezdeményező tulajdonságai](./media/storsimple-configure-chap/IC740944.png)
   2. Kattintson a **Portal felderítése**.
3. A hello **tárolókapu felderítése** párbeszédpanel:
   
   1. Adja meg az eszköz hello IP-címét.
   2. Kattintson a **speciális**.
      
       ![Tárolókapu felderítése](./media/storsimple-configure-chap/IC740945.png)
4. A hello **speciális beállítások** párbeszédpanel:
   
   1. Jelölje be hello **CHAP engedélyezése bejelentkezés** jelölőnégyzetet.
   2. A hello **neve** mező, adjon meg hello felhasználónév hello CHAP-kezdeményező hello a klasszikus portálon megadott.
   3. A hello **cél titkos** mező, adjon meg hello jelszó hello CHAP-kezdeményező hello a klasszikus portálon megadott.
   4. Kattintson az **OK** gombra.
      
       ![Általános speciális beállítások](./media/storsimple-configure-chap/IC740946.png)
5. A hello **célok** hello lapján **iSCSI-kezdeményező tulajdonságai** ablakban hello eszköz állapotúnak kell lennie **csatlakoztatva**. A StorSimple 1200-as eszközt használ, ha majd minden olyan kötetre, iSCSI-tároló van csatlakoztatva. Ezért 3 – 4. lépést kell toobe ismétlődik minden kötete esetében.
   
    ![Külön célként csatlakoztatott kötetek](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > Ha módosítja a hello iSCSI nevét, hello új nevet használja az új iSCSI-munkameneteket. Új beállításai nem érvényesek a meglévő munkamenetekhez, amíg a Kijelentkezés és bejelentkezés újra.

További információ a CHAP konfigurálása hello Windows gazdagép-kiszolgálón nyissa meg túl[további szempontok](#additional-considerations).

## <a name="bidirectional-or-mutual-authentication"></a>Kétirányú vagy a kölcsönös hitelesítés

Kétirányú hitelesítést hello cél hello kezdeményező hitelesíti, és majd hello kezdeményező hitelesíti hello cél. Ehhez az eljáráshoz szükséges hello felhasználói tooconfigure hello CHAP kezdeményező beállításait, a fordított irányú CHAP beállításainak hello eszközön, és az iSCSI-kezdeményező szoftver hello gazdagépen. hello következő eljárásokat hello lépéseket tooconfigure kölcsönös hitelesítés hello eszköz és a Windows hello-állomás.

#### <a name="tooconfigure-your-device-for-mutual-authentication"></a>tooconfigure az eszköz a kölcsönös hitelesítés

1. A hello Azure-portálon válassza a tooyour StorSimple Device Manager szolgáltatást. Kattintson a **eszközök** , és válassza ki, és kattintson arra az eszközre tooconfigure CHAP kívánja a. Nyissa meg túl**eszközbeállítások > biztonsági**. A hello **biztonsági beállítások** panelen kattintson a **CHAP**.
   
    ![A CHAP-cél](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. Görgessen lefelé, ezen a lapon, és a hello **CHAP-cél** szakasz:
   
   1. Adjon meg egy **fordított CHAP-felhasználónév** az eszközhöz.
   2. Adjon meg egy **fordított CHAP-jelszó** az eszközhöz.
   3. Hello jelszót.
3. A hello **CHAP-kezdeményező** szakasz:
   
   1. Adjon meg egy **felhasználónév** az eszközhöz.
   2. Adjon meg egy **jelszó** az eszközhöz.
   3. Hello jelszót.

       ![A CHAP-kezdeményező](./media/storsimple-8000-configure-chap/configure-chap11.png)
4. Kattintson a **Save** (Mentés) gombra. Egy megerősítő üzenet jelenik meg. Kattintson a **OK** toosave hello módosításokat.

#### <a name="tooconfigure-bidirectional-authentication-on-hello-windows-host-server"></a>a hello Windows tooconfigure kétirányú hitelesítést gazdagép-kiszolgálón

1. A Windows hello gazdakiszolgálón indítsa el a hello iSCSI-kezdeményező.
2. A hello **iSCSI-kezdeményező tulajdonságai** ablakában kattintson hello **konfigurációs** lapon.
3. Kattintson a **CHAP**.
4. A hello **iSCSI kezdeményező kölcsönös CHAP titkos** párbeszédpanel:
   
   1. Típus hello **fordított CHAP-jelszó** konfigurált hello Azure-portálon.
   2. Kattintson az **OK** gombra.
      
       ![iSCSI kezdeményező kölcsönös CHAP titkos kulcs](./media/storsimple-configure-chap/IC740949.png)
5. Kattintson a hello **célok** fülre.
6. Kattintson a hello **Connect** gombra. 
7. A hello **tooTarget csatlakozás** párbeszédpanel, kattintson a **speciális**.
8. A hello **speciális tulajdonságok** párbeszédpanel:
   
   1. Jelölje be hello **CHAP engedélyezése bejelentkezés** jelölőnégyzetet.
   2. A hello **neve** mező, adjon meg hello felhasználónév hello CHAP-kezdeményező hello a klasszikus portálon megadott.
   3. A hello **cél titkos** mező, adjon meg hello jelszó hello CHAP-kezdeményező hello a klasszikus portálon megadott.
   4. Jelölje be hello **kölcsönös hitelesítés végrehajtása** jelölőnégyzetet.
      
       ![Speciális beállítások kölcsönös hitelesítés](./media/storsimple-configure-chap/IC740950.png)
   5. Kattintson a **OK** toocomplete hello CHAP konfigurálása

További információ a CHAP konfigurálása hello Windows gazdagép-kiszolgálón nyissa meg túl[további szempontok](#additional-considerations).

## <a name="additional-considerations"></a>Néhány fontos megjegyzés

Hello **gyors csatlakozás** funkció nem támogatja a kapcsolatokat, amelyek CHAP engedélyezve van. Ha CHAP engedélyezve van, győződjön meg arról, hogy használja-e hello **Connect** hello elérhető gomb **célok** lapon tooconnect tooa cél.

![Csatlakozás tootarget](./media/storsimple-configure-chap/IC740947.png)

A hello **tooTarget csatlakozás** nyújtani, jelölje be hello párbeszédpanel **adja hozzá a kedvenc tárolók kapcsolat toohello listája** jelölőnégyzetet. Ez a beállítás biztosítja, hogy minden alkalommal, amikor újraindítás hello kísérlet toorestore hello kapcsolat toohello iSCSI-kedvenc tárolók.

## <a name="errors-during-configuration"></a>Konfigurálása során hibák

Ha a CHAP konfigurációja nem megfelelő, akkor valószínűleg toosee áll egy **hitelesítési hiba** hibaüzenet jelenik meg.

## <a name="verification-of-chap-configuration"></a>A CHAP-konfiguráció ellenőrzése

Ellenőrizheti, hogy a CHAP használatos hello lépések elvégzésével.

#### <a name="tooverify-your-chap-configuration"></a>tooverify a CHAP konfigurálása
1. Kattintson a **kedvenc célok**.
2. Jelölje ki a hitelesítés használatára jogosult hello cél.
3. Kattintson a **részletek**.
   
    ![iSCSI-kezdeményező tulajdonságai kedvenc tárolók](./media/storsimple-configure-chap/IC740951.png)
4. A hello **kedvenc cél részletek** párbeszédpanelen Megjegyzés hello bejegyzést hello **hitelesítési** mező. Ha hello konfiguráció sikeres volt, akkor kell kapnia **CHAP**.
   
    ![Kedvenc cél részletei](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Következő lépések

* További információ [StorSimple biztonsági](storsimple-8000-security.md).
* További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

