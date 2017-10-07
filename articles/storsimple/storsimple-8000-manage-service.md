---
title: "az Azure StorSimple Device Manager szolgáltatás aaaDeploy hello |} Microsoft Docs"
description: "Azt ismerteti, hogyan toocreate és delete hello hello Azure-portálon a StorSimple eszköz kezelő szolgáltatás, és ismerteti, hogyan toomanage hello szolgáltatás regisztrációs kulcsának."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: alkohli
ms.openlocfilehash: b84a907d6b735c8fee7bdc51f9c0074857297d2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>A StorSimple 8000 sorozat eszközeire hello StorSimple Device Manager szolgáltatás telepítése

## <a name="overview"></a>Áttekintés

hello StorSimple Device Manager szolgáltatás fut a Microsoft Azure-ban, és kapcsolódik toomultiple StorSimple eszközökhöz. Hello szolgáltatás létrehozása után is használhatja az összes hello eszköz csatlakoztatott toohello StorSimple Device Manager szolgáltatás toomanage egységes, központi helyről, ezáltal az adminisztratív terhek minimalizálása.

Ez az oktatóanyag a hello létrehozása, törlés, hello szolgáltatás és hello szolgáltatás regisztrációs kulcsának hello kezelése hello lépéseit ismerteti. Ebben a cikkben szereplő hello információ megfelelő csak tooStorSimple 8000 sorozat eszközeire. További információ a StorSimple virtuális tömbök: túl[a StorSimple virtuális tömb StorSimple Device Manager szolgáltatás telepítése](storsimple-virtual-array-manage-service.md).

## <a name="create-a-service"></a>Szolgáltatás létrehozása
a StorSimple eszköz Manager szolgáltatást. toocreate, toohave kell:

* Nagyvállalati szerződéssel rendelkező előfizetés
* Egy aktív Microsoft Azure storage-fiók
* hello kezelési használt számlázási adatokat

Egy nagyvállalati szerződés csak hello előfizetésekre engedélyezettek. A klasszikus Azure portálon hello rendelkező Microsoft Sponsorship előfizetések nem támogatottak a hello Azure-portálon. Hello egy nem támogatott előfizetés használatakor a következő üzenet jelenik meg:

![Nem érvényes előfizetés](./media/storsimple-8000-manage-service/subscription-not-valid.jpg)

Másik lehetőségként toogenerate egy alapértelmezett tárfiókot hello szolgáltatás létrehozásakor.

Egyetlen szolgáltatás több eszközöket kezelheti. Azonban egy eszköz nem terjedhetnek ki több szolgáltatásra. A nagyvállalatok rendelkezhet több szolgáltatás példányok toowork különböző előfizetések, a szervezetek vagy még a központi telepítési helyét. 

> [!NOTE]
> StorSimple Device Manager szolgáltatás toomanage StorSimple 8000 sorozat eszközeire és a StorSimple virtuális tömbök külön példányának van szüksége.

Hajtsa végre a következő lépéseket toocreate szolgáltatás hello.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


Minden egyes StorSimple Device Manager szolgáltatás a következő attribútumok hello létezik:

* **Név** – hello neve rendelt tooyour StorSimple Device Manager szolgáltatás létrehozásakor. **hello szolgáltatás neve nem lehet módosítani, hello szolgáltatás létrehozása után. Ez akkor is igaz, egyéb entitások, például az eszközök, a kötetek, a kötettárolók és a biztonsági mentési házirendek, amelyek nem lehet átnevezni a hello Azure-portálon.**
* **Állapot** – hello szolgáltatás, amely lehet állapotának hello **aktív**, **létrehozása**, vagy **Online**.
* **Hely** – hello földrajzi hely, mely hello StorSimple eszköz telepítve lesz.
* **Előfizetés** – hello a szolgáltatáshoz társított előfizetés számlázási.

## <a name="move-a-service-tooazure-portal"></a>Helyezze át egy tooAzure portálon
A StorSimple 8000 series most kezelheti a hello Azure-portálon. Ha egy meglévő service toomanage hello StorSimple eszközökhöz, azt javasoljuk, hogy a szolgáltatás toohello Azure-portálon helyezi át. hello hello StorSimple Manager szolgáltatás a klasszikus Azure portálon 2017. szeptember 30 után nem érhető el.

hello beállítás toomigrate toohello Azure-portálon szakaszában érhető el. Ha egy beállítás toomigrate tooAzure portál nem jelenik meg, de szeretné, hogy toomove, és tekintse át az áttelepítés hatása hello hello leírtak [átmenet szempontjai](#considerations-for-transition), akkor [igényelnie](https://aka.ms/ss8000-cx-signup).

### <a name="considerations-for-transition"></a>Átmenet szempontjai

Tekintse át a áttelepítése toohello új Azure-portálon hello hatását hello szolgáltatás áthelyezése előtt.

#### <a name="before-you-transition"></a>Áttérés előtt

* Az eszköz fut frissítés 3.0-s vagy újabb. Ha az eszköz egy régebbi verziója fut, telepítse a hello legújabb frissítéseit. További információ: túl[Update 4 telepítése](storsimple-8000-install-update-4.md). A StorSimple felhő készülék (8010-es/8020-as modell) használata esetén hozzon létre egy új felhőalapú készülék frissítés 4.0-s verzióját. 

* Miután állapotra váltott toohello új Azure-portálon, a StorSimple eszköz hello Azure klasszikus portál toomanage nem használható.

* hello áttűnés nem zavaró, és hello eszköz állásidő nélkül.

* Minden hello StorSimple eszköz kezelőnek hello alatt megadott előfizetés átalakulnak.

#### <a name="during-hello-transition"></a>Hello áttűnés során

* Az eszköz nem tudja kezelni a hello portálról.
* Műveletek, például a rétegezési és ütemezett biztonsági mentések folytatása toooccur.
* Ne törölje az hello régi StorSimple eszköz kezelői, amíg hello áttűnés folyamatban van.

#### <a name="after-hello-transition"></a>Hello áttűnés után

* Az eszközök már nem felügyelheti a hello klasszikus portálon.

* hello meglévő Azure Service Management (ASM) PowerShell-parancsmagok használata nem támogatott. Hello parancsfájlok toomanage frissítése a eszközökön hello Azure Resource Manager használatával.

* A szolgáltatás és eszköz konfigurációs megmaradnak. A kötetek és a biztonsági másolatok egyaránt állapotra váltott toohello Azure-portálon.

### <a name="begin-transition"></a>Átmenet megkezdése

Hajtsa végre a következő lépéseket tootransition hello a szolgáltatás toohello Azure-portálon.

1. Nyissa meg a meglévő StorSimple Manager szolgáltatás tooyour hello a klasszikus portálon.

2. Megjelenik egy értesítés, amely tájékoztatja, hogy hello StorSimple Device Manager szolgáltatás jelenleg elérhető hello Azure-portálon. Vegye figyelembe, hogy a hello Azure-portálon, hello szolgáltatás hivatkozott tooas StorSimple Device Manager szolgáltatást.

    ![Áttelepítési értesítés](./media/storsimple-8000-manage-service/service-transition1.jpg)

    1. Győződjön meg arról, hogy átolvasta hello teljes áttelepítés hatását.
    2. Tekintse át a StorSimple eszköz kezelői hello klasszikus portálon áthelyezett hello listáját.

3. Kattintson a **áttelepítése**. hello áttűnés kezdődik, és néhány perc toocomplete vesz igénybe.

Hello áttűnés végrehajtása után az eszközök hello StorSimple Device Manager szolgáltatás a hello Azure-portálon keresztül is kezelheti.

Hello Azure-portálon, a csak a StorSimple eszközök futtatása a frissítés 3.0 hello és az annál magasabb támogatottak. hello régebbi verzióit futtató eszközök csak korlátozottan támogatják. a következő tábla summrizes milyen műveletek támogatottak hello eszközön futó versios előzetes tooUpdate 3.0, miután áttelepítette a klasszikus toohello hello Azure-portálon hello.

| Művelet                                                                                                                       | Támogatott      |
|---------------------------------------------------------------------------------------------------------------------------------|----------------|
| Eszköz regisztrálása                                                                                                               | Igen            |
| Például az általános beállítások, a hálózati és a biztonság konfigurálása                                                                | Igen            |
| Vizsgálat, töltse le és telepítse a frissítéseket                                                                                             | Igen            |
| Eszköz inaktiválása                                                                                                               | Igen            |
| Eszköz törlése                                                                                                                   | Igen            |
| Létrehozása, módosítása és törlése a kötettároló                                                                                   | Nem             |
| Létrehozása, módosítása és kötet törlése                                                                                             | Nem             |
| Létrehozása, módosítása és törlése a biztonsági mentési házirend                                                                                      | Nem             |
| Manuális biztonsági mentés készítése                                                                                                            | Nem             |
| Ütemezett biztonsági mentés készítése                                                                                                         | Nem alkalmazható |
| Állítsa vissza egy Fájljelző                                                                                                        | Nem             |
| Klónozza a 3.0-s vagy újabb verzióját futtató frissítési tooa eszköz <br> hello forráseszközt verziója előzetes tooUpdate 3.0 fut.                                | Igen            |
| Klónozás tooa eszköz verziója előzetes tooUpdate 3.0 fut                                                                          | Nem             |
| Forrás eszközként feladatátvétel <br> (futtató eszközről verziója előzetes tooUpdate 3.0 tooa eszköz Update futtatása, 3.0-s és újabb verziók)                                                               | Igen            |
| Cél eszközként feladatátvétel <br> (tooa eszköz szoftver verziója előzetes tooUpdate 3.0 fut)                                                                                   | Nem             |
| Riasztás törlése                                                                                                                  | Igen            |
| Biztonsági mentési házirendek, biztonságimásolat-katalógus, kötetek, kötettárolók, figyelési diagramokat, feladatok és a klasszikus portálon létrehozott riasztások megtekintése | Igen            |
| És eszközvezérlők bekapcsolása                                                                                              | Igen            |


## <a name="delete-a-service"></a>A szolgáltatás törlése

A szolgáltatás törlése előtt győződjön meg arról, hogy nem csatlakoztatott eszközök-k használják. Ha hello szolgáltatást használja, inaktiválása hello csatlakoztatott eszközöket. hello inaktiválása művelet Server hello kapcsolatot hello eszköz és a hello szolgáltatást, de hello felhőben hello eszköz adatok megőrzése érdekében.

> [!IMPORTANT]
> Szolgáltatás törlését követően hello művelet nem vonható vissza. Minden olyan eszköz, amely hello szolgáltatás használatával lett egy másik szolgáltatás használata előtt meg kell toobe alaphelyzetbe állítása toofactory alapértelmezett értéke. Ebben a forgatókönyvben helyi adatok hello hello eszköz, valamint hello konfigurálása nem támogatott.

Hajtsa végre a következő lépéseket toodelete szolgáltatás hello.

### <a name="toodelete-a-service"></a>a szolgáltatás toodelete

1. Keresési hello szolgáltatás toodelete keresi. Kattintson a **erőforrások** ikonra, és ezután bemeneti hello megfelelő feltételek toosearch. Hello keresési eredmények között kattintson a kívánt toodelete hello szolgáltatást.

    ![Keresési szolgáltatás toodelete](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Ezzel megnyitná toohello StorSimple Device Manager szolgáltatás panelre. Kattintson a **Törlés** gombra.

    ![Szolgáltatás törlése](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Kattintson a **Igen** hello megerősítési értesítés. Hello szolgáltatás toobe törölt néhány percig is eltarthat.

    ![Törlés megerősítése](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-hello-service-registration-key"></a>Hello Szolgáltatásregisztrációs kulcs lekérése

Sikeresen létrehozott egy szolgáltatást, akkor a StorSimple eszköz hello szolgáltatásban tooregister. tooregister az első StorSimple eszköz meg fogja kell hello szolgáltatás regisztrációs kulcsának. egy meglévő StorSimple szolgáltatás további eszközök tooregister, kell hello regisztrációs kulcs és a hello szolgáltatásadat-titkosítási kulcs (amely a regisztráció során van hello első eszközön létre). További információ a szolgáltatásadat-titkosítási kulcs hello: [StorSimple biztonsági](storsimple-8000-security.md). Hello regisztrációs kulcs kaphat elérése **kulcsok** a StorSimple Device Manager panelen.

Hajtsa végre a következő lépéseket tooget hello szolgáltatás regisztrációs kulcsának hello.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Hello szolgáltatás regisztrációs kulcsának tartsa biztonságos helyen. Szüksége lesz a kulcs, valamint hello szolgáltatásadat-titkosítási kulcs, tooregister további eszközöket a szolgáltatással. Miután beszerezte a hello Szolgáltatásregisztrációs kulcs, konfigurálnia kell az eszköz StorSimple illesztőfelület hello Windows PowerShell használatával.

További részletekért toouse a regisztrációs kulcs, lásd: [3. lépés: eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-hello-service-registration-key"></a>Hello Szolgáltatásregisztrációs kulcs újragenerálása
Ha szükséges tooperform kulcs elforgatási, vagy hogy módosult-e a szolgáltatás-rendszergazdák hello listája kell tooregenerate egy Szolgáltatásregisztrációs kulcs. Amikor újragenerálja hello kulcsot, hello új kulccsal csak a következő eszközök regisztrálása. már regisztrált hello eszközöket nem érinti ez a folyamat.

Hajtsa végre a következő lépéseket tooregenerate a szolgáltatás regisztrációs kulcsának hello.

### <a name="tooregenerate-hello-service-registration-key"></a>tooregenerate hello Szolgáltatásregisztrációs kulcs
1. A hello **StorSimple Device Manager** paneljén lépjen túl**felügyeleti &gt;**  **kulcsok**.
    
    ![Kulcsok panel](./media/storsimple-8000-manage-service/regenregkey2.png)

2. A hello **kulcsok** panelen kattintson a **újragenerálja**.

    ![Kattintson a újragenerálása](./media/storsimple-8000-manage-service/regenregkey3.png)
3. A hello **Szolgáltatásregisztrációs kulcs újragenerálása** panelt, tekintse át a hello művelet kötelező, ha a hello kulcsok újragenerálása vannak. A szolgáltatáshoz regisztrált összes hello további eszközök hello új regisztrációs kulcsot használja. Kattintson a **újragenerálja** tooconfirm. Értesítés jelenik meg hello újragenerálása befejeződése után.

    ![Regenerate megerősítése](./media/storsimple-8000-manage-service/regenregkey4.png)

4. Egy új Szolgáltatásregisztrációs kulcs jelenik meg.

5. Másolja ki ezt a kulcsot, és mentse az új eszköz regisztrálása ezt a szolgáltatást.



## <a name="change-hello-service-data-encryption-key"></a>Szolgáltatásadat-titkosítási kulcs hello módosítása
Szolgáltatás adattitkosítási kulcsokat használt tooencrypt bizalmas felhasználói adatok, például a tárfiók hitelesítő adatait, a StorSimple Manager szolgáltatás toohello StorSimple eszközről küldött. Szüksége lesz toochange ezeket a kulcsokat rendszeres időközönként, ha az IT szervezet rendelkezik egy kulcs Elforgatás házirend hello tárolóeszközökön. hello kulcsváltozás folyamat kis mértékben eltérhet attól függően, hogy van egy egyetlen vagy több eszközön hello StorSimple Manager szolgáltatás kezeli. További információ: túl[StorSimple biztonság és adatvédelem](storsimple-8000-security.md).

Változó hello szolgáltatásadat-titkosítási kulcs egy 3 lépésben:

1. Egy eszköz toochange hello szolgáltatásadat-titkosítási kulcs Windows PowerShell-parancsfájlok az Azure erőforrás-kezelő használatával engedélyezheti.
2. A Windows PowerShell használatával a StorSimple, indítsa el a hello szolgáltatás titkosítási kulcs változását.
3. Ha egynél több StorSimple eszközt, frissíteni hello szolgáltatásadat-titkosítási kulcs hello az egyéb eszközöket.

### <a name="step-1-use-windows-powershell-script-tooauthorize-a-device-toochange-hello-service-data-encryption-key"></a>1. lépés: A Windows PowerShell parancsfájl tooAuthorize egy eszköz toochange hello szolgáltatásadat-titkosítási kulcs
Általában hello eszköz-rendszergazdai kérni fogja, hogy hello szolgáltatás-rendszergazda engedélyezi egy eszköz toochange szolgáltatás titkosítási kulcsokat. hello szolgáltatás-rendszergazda majd engedélyeztetéséhez használandó hello toochange hello eszközkulcs.

Ez a lépés hello Azure Resource Manager-alapú parancsfájl használatával történik. hello szolgáltatás rendszergazda kiválaszthat egy eszköz, amely jogosult toobe engedélyezett. hello eszköz nem engedélyezett toostart hello szolgáltatásadat-titkosítási kulcs módosítása folyamat. 

Hello parancsfájl használatával kapcsolatos további információkért lépjen túl[engedélyezés-ServiceEncryptionRollover.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Authorize-ServiceEncryptionRollover.ps1)

#### <a name="which-devices-can-be-authorized-toochange-service-data-encryption-keys"></a>Mely eszközök engedélyezett toochange szolgáltatás titkosítási kulcsokat?
Egy eszköz hello ahhoz, azok engedélyezett tooinitiate szolgáltatás titkosítási kulcs adatváltozásokat a következő feltételeknek kell megfelelniük:

* hello eszköz jogosult szolgáltatás adatok titkosítás kulcsváltozás engedélyezése az online toobe kell lennie.
* Engedélyezheti a hello ugyanarra az eszközre újra 30 másodperc után ha hello kulcs módosítása nem lett inicializálva.
* Engedélyezheti egy másik eszközön, feltéve, hogy hello kulcsváltozás hello korábban eszköz nem lett inicializálva. Miután hello új eszköz engedélyezve van-e, hello régi eszköz hello módosítása nem kezdeményezhetik.
* Egy eszköz nem lehet engedélyezni, amíg folyamatban van hello hello szolgáltatás adatok adattitkosítási kulcshoz kapcsolódó kulcsváltást.
* Ha néhány hello szolgáltatásban regisztrált hello eszközök rendelkezik tanúsítványváltást hello titkosítási míg mások számára nem engedélyezhető a eszköz. 

### <a name="step-2-use-windows-powershell-for-storsimple-tooinitiate-hello-service-data-encryption-key-change"></a>2. lépés: A Windows Powershellt használja a StorSimple tooinitiate hello szolgáltatás titkosítási kulcs változását
Ebben a lépésben történik hello Windows PowerShell, a StorSimple illesztőfelülete hello a StorSimple eszköz engedélyezett.

> [!NOTE]
> Nincsenek műveletek is elvégezhetők hello Azure-portálon a StorSimple Manager szolgáltatás hello kulcshoz kapcsolódó kulcsváltás után befejezéséig.
> 
> 

Ha hello eszköz soros konzoljához tooconnect toohello Windows PowerShell-felületet használ, hajtsa végre a lépéseket követve hello.

#### <a name="tooinitiate-hello-service-data-encryption-key-change"></a>tooinitiate hello szolgáltatásadat-titkosítási kulcs módosítása
1. Válassza ki az 1. lehetőség toolog teljes hozzáféréssel.
2. Hello parancsot, írja be a parancssorba:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Miután hello parancsmag sikeresen befejeződött, kap egy új szolgáltatásadat-titkosítási kulcs. Másolja ki és mentse a kulcs használható a 3. lépésben a folyamat során. Ez a kulcs lesz tooupdate használt összes hello fennmaradó hello StorSimple Manager szolgáltatásban regisztrált eszközöket.
   
   > [!NOTE]
   > Ez a folyamat engedélyezése a StorSimple eszköz négy órán belül kell kezdeményezni.
   > 
   > 
   
   Ez az új kulcs küldi el a rendszer toohello szolgáltatás leküldött toobe tooall hello regisztrált eszközök hello szolgáltatásban. Riasztás megjelenik hello szolgáltatás irányítópultját. hello szolgáltatás le fogja tiltani az összes hello művelet hello regisztrált eszközökön, és eszköz-rendszergazdai hello majd kell tooupdate hello szolgáltatásadat-titkosítási kulcs a hello, más eszközök. Azonban hello i/o műveletek (gazdagép toohello felhő adatok küldése) fog nem működni.
   
   Ha még egyetlen eszközt regisztrált tooyour szolgáltatás, hello helyettesítő folyamat befejeződött, és hello a következő lépést kihagyhatja. Ha több eszközök regisztrált tooyour szolgáltatás, folytassa a toostep 3.

### <a name="step-3-update-hello-service-data-encryption-key-on-other-storsimple-devices"></a>3. lépés: Frissítés hello szolgáltatásadat-titkosítási kulcs a más StorSimple eszközökhöz
Hello Windows PowerShell felületet a StorSimple eszköz ezeket a lépéseket kell elvégezni, ha több eszközök regisztrált tooyour StorSimple Manager szolgáltatás. 2. lépésben beszerzett hello kulcs összes hello StorSimple eszköz regisztrálva a StorSimple Manager szolgáltatás hello fennmaradó használt tooupdate kell lennie.

Hajtsa végre a hello követő lépéseket tooupdate hello szolgáltatásadat-titkosítási az eszközön.

#### <a name="tooupdate-hello-service-data-encryption-key"></a>tooupdate hello szolgáltatásadat-titkosítási kulcs
1. StorSimple tooconnect toohello konzol a Windows Powershellt használja. Válassza ki az 1. lehetőség toolog teljes hozzáféréssel.
2. Hello parancsot, írja be a parancssorba:
   
    `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Adja meg a hello szolgáltatásadat-titkosítási kulcs beolvasott [2. lépés: a Windows Powershellt használja a StorSimple tooinitiate hello szolgáltatás titkosítási kulcs változását](#to-initiate-the-service-data-encryption-key-change).


## <a name="next-steps"></a>Következő lépések
* További tudnivalók hello [StorSimple telepítési folyamatának](storsimple-8000-deployment-walkthrough-u2.md).
* További információ [tárfiók felügyelete a StorSimple](storsimple-8000-manage-storage-accounts.md).
* További tudnivalók túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).
