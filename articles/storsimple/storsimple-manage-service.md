---
title: "a StorSimple Manager szolgáltatás aaaDeploy hello |} Microsoft Docs"
description: "Azt ismerteti, hogyan toocreate és delete hello StorSimple Manager szolgáltatás a hello a klasszikus Azure portálon, és ismerteti, hogyan toomanage hello szolgáltatás regisztrációs kulcsának."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: bc1d5650-275c-42ed-bc77-cdb596f85943
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/14/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f49b647d91b03bb89ebd0e5cce196e50e3c00296
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-manager-service-in-hello-azure-classic-portal"></a>A klasszikus Azure portálon hello hello StorSimple Manager szolgáltatás telepítése

## <a name="overview"></a>Áttekintés
hello StorSimple Manager szolgáltatás fut a Microsoft Azure-ban, és kapcsolódik toomultiple StorSimple eszközökhöz. Miután létrehozta a hello szolgáltatást, használhatja toomanage hello eszközök hello Microsoft Azure klasszikus portál futtatása böngészőben. Ez lehetővé teszi toomonitor összes hello eszköz csatlakoztatott toohello StorSimple Manager szolgáltatás egységes, központi helyről, ami minimalizálja a felügyeleti terheket.

hello StorSimple Manager kezdőlapja minden hello StorSimple Manager szolgáltatás használható toomanage a StorSimple tárolóeszközök sorolja fel. Minden egyes StorSimple Manager szolgáltatás a következő információk hello hello StorSimple Manager lapon megjelenő van:

* **Név** – hello neve rendelt tooyour StorSimple Manager szolgáltatás létrehozásakor. **hello szolgáltatás neve nem lehet módosítani, hello szolgáltatás létrehozása után. Ez akkor is igaz, egyéb entitások, például az eszközök, a kötetek, a kötettárolók és a biztonsági mentési házirendek, amelyek nem lehet átnevezni a hello a klasszikus Azure portálon.**
* **Állapot** – hello szolgáltatás, amely lehet állapotának hello **aktív**, **létrehozása**, vagy **Online**.
* **Hely** – hello földrajzi hely, mely hello StorSimple eszköz telepítve lesz.
* **Előfizetés** – hello a szolgáltatáshoz társított előfizetés számlázási.

hello kapcsolatos általános feladatok hello StorSimple Manager lap segítségével végezheti el a következők:

* Szolgáltatás létrehozása
* A szolgáltatás törlése
* Hello Szolgáltatásregisztrációs kulcs lekérése
* Hello Szolgáltatásregisztrációs kulcs újragenerálása

Ez az oktatóanyag leírja, hogyan tooperform minden ezeket a feladatokat.

## <a name="create-a-service"></a>Szolgáltatás létrehozása
Használjon hello **Gyorslétrehozás** toocreate a StorSimple Manager szolgáltatás lehetőséget, ha azt szeretné, toodeploy a StorSimple eszköz. egy szolgáltatás toocreate, toohave kell:

* Nagyvállalati szerződéssel rendelkező előfizetés
* Egy aktív Microsoft Azure storage-fiók
* hello kezelési használt számlázási adatokat

Másik lehetőségként toogenerate egy alapértelmezett tárfiókot hello szolgáltatás létrehozásakor.

Egyetlen szolgáltatás több eszközöket kezelheti. Azonban egy eszköz nem terjedhetnek ki több szolgáltatásra. A nagyvállalatok rendelkezhet több szolgáltatás példányok toowork különböző előfizetések, a szervezetek vagy még a központi telepítési helyét. Vegye figyelembe a StorSimple Manager szolgáltatás toomanage StorSimple 8000 sorozat eszközeire és a StorSimple virtuális tömbök külön kell.

> [!IMPORTANT] 
> Ha egy nem használt szolgáltatás létrehozása (nincs ehhez az erőforráshoz a végrehajtott műveletek eszköz) előzetes tooAugust 2016, az Azure portál vagy a klasszikus Azure portálon keresztül nem kezelhető. Azt javasoljuk, hogy hozzon létre egy új szolgáltatás a hello Azure-portálon.

Hajtsa végre a következő lépéseket toocreate szolgáltatás hello.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## <a name="delete-a-service"></a>A szolgáltatás törlése
A szolgáltatás törlése előtt győződjön meg arról, hogy nem csatlakoztatott eszközök-k használják. Ha hello szolgáltatást használja, inaktiválása hello csatlakoztatott eszközöket. hello inaktiválása művelet Server hello kapcsolatot hello eszköz és a hello szolgáltatást, de hello felhőben hello eszköz adatok megőrzése érdekében.

> [!IMPORTANT] 
> Szolgáltatás törlését követően hello művelet nem vonható vissza. Minden olyan eszköz, amely hello szolgáltatás használatával lett kell toobe gyári beállításokra történő visszaállításának ahhoz, hogy egy másik szolgáltatást kell használni. Ebben a forgatókönyvben hello hello eszköz, valamint hello konfigurációs helyi adatok elvesznek.

Hajtsa végre a következő lépéseket toodelete szolgáltatás hello.

### <a name="toodelete-a-service"></a>a szolgáltatás toodelete
1. A hello **StorSimple Manager szolgáltatás** lapon, hogy kívánja-e toodelete válassza hello szolgáltatás.
2. Kattintson a **törlése** hello lap hello alján.
3. Kattintson a **Igen** hello megerősítési értesítés. Hello szolgáltatás toobe törölt néhány percig is eltarthat.

## <a name="get-hello-service-registration-key"></a>Hello Szolgáltatásregisztrációs kulcs lekérése
Sikeresen létrehozott egy szolgáltatást, akkor a StorSimple eszköz hello szolgáltatásban tooregister. tooregister az első StorSimple eszköz meg fogja kell hello szolgáltatás regisztrációs kulcsának. egy meglévő StorSimple szolgáltatás további eszközök tooregister, szüksége lesz hello regisztrációs kulcs és a hello szolgáltatásadat-titkosítási kulcs (amely a regisztráció során van hello első eszközön létre). További információ a szolgáltatásadat-titkosítási kulcs hello: [StorSimple biztonsági](storsimple-security.md). Hello regisztrációs kulcs kaphat elérése **regisztrációs kulcs** a hello **szolgáltatások** lap.

Hajtsa végre a következő lépéseket tooget hello szolgáltatás regisztrációs kulcsának hello.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

Hello szolgáltatás regisztrációs kulcsának tartsa biztonságos helyen. Szüksége lesz a kulcs, valamint hello szolgáltatásadat-titkosítási kulcs, tooregister további eszközöket a szolgáltatással. Miután beszerezte a hello szolgáltatás regisztrációs kulcsának, szüksége lesz tooconfigure hello Windows PowerShell az eszköz StorSimple felületet.

További részletekért toouse a regisztrációs kulcs, lásd: [3. lépés: eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-hello-service-registration-key"></a>Hello Szolgáltatásregisztrációs kulcs újragenerálása
Szüksége lesz a szolgáltatás regisztrációs kulcsának tooregenerate szükséges tooperform kulcs elforgatás vagy ha a szolgáltatás-rendszergazdák hello listája megváltozott-e. Amikor újragenerálja hello kulcsot, hello új kulccsal csak a következő eszközök regisztrálása. már regisztrált hello eszközöket nem érinti ez a folyamat.

Hajtsa végre a következő lépéseket tooregenerate a szolgáltatás regisztrációs kulcsának hello.

### <a name="tooregenerate-hello-service-registration-key"></a>tooregenerate hello Szolgáltatásregisztrációs kulcs
1. A hello **StorSimple Manager szolgáltatás** kattintson **regisztrációs kulcs**.
2. A hello **Szolgáltatásregisztrációs kulcs** párbeszédpanel, kattintson a **újragenerálja**.
3. Egy megerősítő üzenet jelenik meg. Kattintson a **OK** toocontinue a hello újragenerálása.
4. Egy új Szolgáltatásregisztrációs kulcs jelenik meg.
5. Másolja ki ezt a kulcsot, és mentse az új eszköz regisztrálása ezt a szolgáltatást.
6. Kattintson a pipa ikonra hello ![Pipa ikon](./media/storsimple-manage-service/HCS_CheckIcon.png) tooclose ezen a párbeszédpanelen.

## <a name="next-steps"></a>Következő lépések
* További tudnivalók hello [StorSimple telepítési folyamatának](storsimple-deployment-walkthrough-u2.md).
* További információ [tárfiók felügyelete a StorSimple](storsimple-manage-storage-accounts.md).
* További tudnivalók túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).
