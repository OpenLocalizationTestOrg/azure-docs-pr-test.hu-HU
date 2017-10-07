---
title: "StorSimple Device Manager szolgáltatás aaaDeploy |} Microsoft Docs"
description: "Azt ismerteti, hogyan toocreate és delete hello hello Azure-portálon a StorSimple eszköz kezelő szolgáltatás, és ismerteti, hogyan toomanage hello szolgáltatás regisztrációs kulcsának."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 28499494-8c4d-4a7f-9d44-94b2b8a93c93
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: alkohli
ms.openlocfilehash: 9064053addc7b3dfcce08b47e81b38c2e0e1b559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-device-manager-service-for-storsimple-virtual-array"></a>A StorSimple virtuális tömb hello StorSimple Device Manager szolgáltatás telepítése
## <a name="overview"></a>Áttekintés

hello StorSimple Device Manager szolgáltatás fut a Microsoft Azure-ban, és kapcsolódik toomultiple StorSimple eszközökhöz. Miután létrehozta a hello szolgáltatást, használhatja toomanage hello eszközök hello Microsoft Azure portál futtatása böngészőben. Ez lehetővé teszi egyetlen, központi helyről, ezáltal az adminisztratív terhek minimalizálása szolgáltatás összes hello eszköz csatlakoztatott toohello StorSimple Device Manager toomonitor.

hello gyakori feladatok kapcsolódó tooa StorSimple Device Manager szolgáltatás a következők:

* Szolgáltatás létrehozása
* A szolgáltatás törlése
* Hello Szolgáltatásregisztrációs kulcs lekérése
* Hello Szolgáltatásregisztrációs kulcs újragenerálása

Ez az oktatóanyag leírja, hogyan tooperform minden a hello előző feladatok. Ebben a cikkben szereplő hello információ alkalmazható csak tooStorSimple virtuális tömbök. További információ a StorSimple 8000 Series: túl[StorSimple Manager szolgáltatás telepítése](storsimple-manage-service.md).

## <a name="create-a-service"></a>Szolgáltatás létrehozása

egy szolgáltatás toocreate, toohave kell:

* Nagyvállalati szerződéssel rendelkező előfizetés
* Egy aktív Microsoft Azure storage-fiók
* hello kezelési használt számlázási adatokat

Másik lehetőségként toogenerate tárfiók hello szolgáltatás létrehozásakor.

Egyetlen szolgáltatás több eszközöket kezelheti. Azonban egy eszköz nem terjedhetnek ki több szolgáltatásra. A nagyvállalatok rendelkezhet több szolgáltatás példányok toowork különböző előfizetések, a szervezetek vagy még a központi telepítési helyét.

> [!NOTE]
> StorSimple Device Manager szolgáltatás toomanage StorSimple 8000 sorozat eszközeire és a StorSimple virtuális tömbök külön példányának van szüksége.


Hajtsa végre a következő lépéseket toocreate szolgáltatás hello.

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

## <a name="delete-a-service"></a>A szolgáltatás törlése

A szolgáltatás törlése előtt győződjön meg arról, hogy nem csatlakoztatott eszközök-k használják. Ha hello szolgáltatást használja, inaktiválása hello csatlakoztatott eszközöket. hello inaktiválása művelet Server hello kapcsolatot hello eszköz és a hello szolgáltatást, de hello felhőben hello eszköz adatok megőrzése érdekében.

> [!IMPORTANT]
> Szolgáltatás törlését követően hello művelet nem vonható vissza. Minden olyan eszköz, amely hello szolgáltatás használatával lett kell toobe gyári beállításokra történő visszaállításának ahhoz, hogy egy másik szolgáltatást kell használni. Ebben a forgatókönyvben hello hello eszköz, valamint hello konfigurációs helyi adatok elvesznek.
 

Hajtsa végre a következő lépéseket toodelete szolgáltatás hello.

#### <a name="toodelete-a-service"></a>a szolgáltatás toodelete

1. Nyissa meg túl**összes erőforrás**. Keresse meg a StorSimple eszköz Manager szolgáltatást. Válassza ki, hogy kívánja-e toodelete hello szolgáltatást.
   
    ![Válassza ki a szolgáltatás toodelete](./media/storsimple-virtual-array-manage-service/deleteservice2.png)
2. Nyissa meg tooyour szolgáltatás irányítópult tooensure nincsenek egyetlen eszköz sem kapcsolódó toohello szolgáltatás. Ha nincsenek a szolgáltatáshoz regisztrált eszközök, ezenkívül megjelenik egy szalagcím üzenet toohello hatást. Kattintson a **Törlés** gombra.
   
    ![Szolgáltatás törlése](./media/storsimple-virtual-array-manage-service/deleteservice3.png)

3. Amikor felszólítja a megerősítésre, kattintson **Igen** hello megerősítési értesítés. 
   
    ![Szolgáltatás törlésének megerősítése](./media/storsimple-virtual-array-manage-service/deleteservice4.png)
4. Hello szolgáltatás toobe törölt néhány percig is eltarthat. Után hello szolgáltatás sikeresen törlődik, értesítést fog kapni.
   
    ![Sikeres szolgáltatás törlése](./media/storsimple-virtual-array-manage-service/deleteservice6.png)

szolgáltatások hello listája frissítve lesz.

 ![Szolgáltatás frissített listája](./media/storsimple-virtual-array-manage-service/deleteservice7.png)

## <a name="get-hello-service-registration-key"></a>Hello Szolgáltatásregisztrációs kulcs lekérése
Sikeresen létrehozott egy szolgáltatást, akkor a StorSimple eszköz hello szolgáltatásban tooregister. tooregister az első StorSimple eszköz meg fogja kell hello szolgáltatás regisztrációs kulcsának. egy meglévő StorSimple szolgáltatás további eszközök tooregister, szüksége lesz hello regisztrációs kulcs és a hello szolgáltatásadat-titkosítási kulcs (amely a regisztráció során van hello első eszközön létre). További információ a szolgáltatásadat-titkosítási kulcs hello: [StorSimple biztonsági](storsimple-security.md). Hello regisztrációs kulcs kaphat hello elérésével **kulcsok** panel a szolgáltatás.

Hajtsa végre a következő lépéseket tooget hello szolgáltatás regisztrációs kulcsának hello.

#### <a name="tooget-hello-service-registration-key"></a>tooget hello Szolgáltatásregisztrációs kulcs
1. A hello **StorSimple Device Manager** paneljén lépjen túl**felügyeleti &gt;**  **kulcsok**.
   
   ![Kulcsok panel](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. A hello **kulcsok** panelen, a szolgáltatás regisztrációs kulcsának jelenik meg. Hello másolás ikon használata hello regisztrációs kulcs másolása. 

Hello szolgáltatás regisztrációs kulcsának tartsa biztonságos helyen. Szüksége lesz a kulcs, valamint hello szolgáltatásadat-titkosítási kulcs, tooregister további eszközöket a szolgáltatással. Miután beszerezte a hello szolgáltatás regisztrációs kulcsának, szüksége lesz tooconfigure hello Windows PowerShell az eszköz StorSimple felületet.

## <a name="regenerate-hello-service-registration-key"></a>Hello Szolgáltatásregisztrációs kulcs újragenerálása
Szüksége lesz a szolgáltatás regisztrációs kulcsának tooregenerate szükséges tooperform kulcs elforgatás vagy ha a szolgáltatás-rendszergazdák hello listája megváltozott-e. Amikor újragenerálja hello kulcsot, hello új kulccsal csak a következő eszközök regisztrálása. már regisztrált hello eszközöket nem érinti ez a folyamat.

Hajtsa végre a következő lépéseket tooregenerate a szolgáltatás regisztrációs kulcsának hello.

#### <a name="tooregenerate-hello-service-registration-key"></a>tooregenerate hello Szolgáltatásregisztrációs kulcs
1. A hello **StorSimple Device Manager** paneljén lépjen túl**felügyeleti &gt;**  **kulcsok**.
   
   ![Kulcsok panel](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. A hello **kulcsok** panelen kattintson a **újragenerálja**.
   
   ![Kattintson a újragenerálása](./media/storsimple-virtual-array-manage-service/getregkey5.png)
3. A hello **Szolgáltatásregisztrációs kulcs újragenerálása** panelt, tekintse át a hello művelet kötelező, ha a hello kulcsok újragenerálása vannak. A szolgáltatáshoz regisztrált összes hello további eszközök hello új regisztrációs kulcsot fogja használni. Kattintson a **újragenerálja** tooconfirm. Értesítést fog kapni hello regisztráció befejezése után.
   
   ![Ellenőrizze a kulcs újragenerálása](./media/storsimple-virtual-array-manage-service/getregkey3.png)
4. Egy új Szolgáltatásregisztrációs kulcs jelenik meg.
   
    ![Ellenőrizze a kulcs újragenerálása](./media/storsimple-virtual-array-manage-service/getregkey4.png)
   
   Másolja ki ezt a kulcsot, és mentse az új eszköz regisztrálása ezt a szolgáltatást.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[Ismerkedés](storsimple-virtual-array-deploy1-portal-prep.md) egy StorSimple virtuális tömbbel rendelkező.
* Ismerje meg, hogyan túl[felügyelete a StorSimple eszköz](storsimple-ova-web-ui-admin.md).

