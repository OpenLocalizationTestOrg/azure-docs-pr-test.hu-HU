---
title: "aaaHow toomanage service konfigurációk és a profilok |} Microsoft Docs"
description: "Megtudhatja, hogyan szolgáltatás-konfigurációk és profilok konfigurációs fájlokat toowork |} amely hello telepítési környezetekben a beállítások tárolásához, és a közzétételi beállítások felhőszolgáltatásai számára."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7da8c551-fb06-4057-b5c7-c77f4b39d803
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/11/2017
ms.author: kraigb
ms.openlocfilehash: 1dba9df2fa57fe94dacc90ae74b05ccdc28270c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-service-configurations-and-profiles"></a>Hogyan toomanage szolgáltatás konfigurációk és profilok
## <a name="overview"></a>Áttekintés
Amikor közzétesz egy felhőalapú szolgáltatás, a Visual Studio kétféle konfigurációs fájlok konfigurációs információkat tárolja: konfigurációk és profilok. A szolgáltatáskonfiguráció (.cscfg-fájlok) hello telepítési környezetekben az Azure-felhőszolgáltatás beállítások tárolásához. Azure használja ezeket a konfigurációs fájlokat, amikor a cloud services csomag kezeli. A hello ugyanakkor, profilok (.azurePubxml fájlok) áruházbeli közzétételi beállítások felhőszolgáltatásai számára. Ezek a beállítások beállításoktól hello használatakor közzététele varázsló, és a Visual Studio helyi használják. Ez a témakör azt ismerteti, hogyan toowork mindkét típusú konfigurációs fájlok.

## <a name="service-configurations"></a>Szolgáltatás-konfigurációk
Több szolgáltatás konfigurációk toouse hozhat létre az egyes a telepítési környezetekben. Például előfordulhat, hogy hozzon létre helyi környezetben hello toorun használni, és az Azure-alkalmazások és egy másik szolgáltatás konfigurációjának tesztelése az éles környezetben a szolgáltatás konfigurációját.

Hozzáadhat, törlése, nevezze át, és ezek a követelmények alapján szolgáltatáskonfiguráció módosítása. Kezelheti a szolgáltatáskonfiguráció a Visual Studio eszközből, ahogy az ábra a következő hello.

![Szolgáltatás-konfigurációk kezelése](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

Hello is megnyithatja **konfigurációk kezelése** hello szerepkör tulajdonságlapjain párbeszédpanelt. egy szerepkör esetén az Azure-projekt tooopen hello tulajdonságok adott szerepkörhöz tartozó hello helyi menü megnyitásához, és válassza **tulajdonságok**. A hello **beállítások** lapján bontsa ki a hello **szolgáltatáskonfiguráció** listában, és válassza ki **kezelése** tooopen hello **kezelése konfigurációk**párbeszédpanel megnyitásához.

### <a name="tooadd-a-service-configuration"></a>a szolgáltatás konfigurációs tooadd
1. A Solution Explorerben nyissa meg a hello Azure-projekt helyi menüjének hello, és válassza ki **konfigurációk kezelése**.
   
    Hello **szolgáltatáskonfiguráció kezelése** párbeszédpanel jelenik meg.
2. tooadd szolgáltatáskonfiguráció, létre kell hoznia egy meglévő konfigurációnak egy példányát. toodo, szeretné, hogy toocopy hello név listából, és válassza ki a hello konfiguráció **készítsen másolatot**.
3. (Választható) toogive hello szolgáltatás konfigurációját egy másik nevet, választhat hello új szolgáltatáskonfiguráció hello nevét, és válassza ki **átnevezése**. A hello **neve** hello típusnév toouse szeretné, hogy a szolgáltatás-konfigurációhoz, és válassza ki, a **OK**.
   
    Egy új szolgáltatáskonfigurációs fájlt nevű ServiceConfiguration. [Új neve] .cscfg toohello Azure-projekt kerül a Megoldáskezelőben.

### <a name="toodelete-a-service-configuration"></a>a szolgáltatás konfigurációs toodelete
1. A Solution Explorerben nyissa meg a hello Azure-projekt helyi menüjének hello, és válassza ki **konfigurációk kezelése**.
   
    Hello **szolgáltatáskonfiguráció kezelése** párbeszédpanel jelenik meg.
2. toodelete szolgáltatáskonfiguráció, válasszon hello konfigurációt, hogy szeretné-e a hello toodelete **neve** listában, és válassza ki **eltávolítása**. Megjelenik egy párbeszédpanel tooverify megjeleníteni kívánt toodelete ezt a konfigurációt.
3. Válassza a **Törlés** elemet.
   
     hello szolgáltatás konfigurációs fájlja hello Azure projektben a Megoldáskezelőre törlődik.

### <a name="toorename-a-service-configuration"></a>a szolgáltatás konfigurációs toorename
1. A Megoldáskezelőben hello hello Azure-projekt helyi menüjének megnyitásához, és válassza **konfigurációk kezelése**.
   
    Hello **szolgáltatáskonfiguráció kezelése** párbeszédpanel jelenik meg.
2. toorename a szolgáltatás konfigurációs lehetőségek közül választhat hello új szolgáltatáskonfiguráció hello **neve** listában, és válassza ki **átnevezése**. A hello **neve** szövegmezőben, hello típusnév toouse szeretné, hogy a szolgáltatás-konfigurációhoz, és válassza **OK**.
   
    hello hello szolgáltatás konfigurációs fájlja módosul neve hello Azure projektben a Megoldáskezelőre.

### <a name="toochange-a-service-configuration"></a>a szolgáltatás konfigurációs toochange
* Ha azt szeretné, hogy a szolgáltatáskonfiguráció toochange, nyissa meg a helyi menü hello hello adott szerepkörhöz szeretné, hogy az Azure-projekt hello toochange, és válassza **tulajdonságok**. Lásd: [hogyan: hello szerepkörök konfigurálása a Visual Studio Azure Cloud Service](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) további információt.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Ellenőrizze a különböző beállítás kombinációk profilok használatával
A profil segítségével automatikusan kitöltése hello **közzététele varázsló** eltérő kombinációja a különböző célokra beállításait. Például rendelkezhet egy profil hibakeresési, és összeállít egy másik, a kiadáshoz. Ebben az esetben a **Debug** profil kellene **IntelliTrace** engedélyezve van, és hello **Debug** konfigurációs kiválasztva, és a **kiadás** profil kellene **IntelliTrace** le van tiltva és hello **kiadás** kiválasztott konfigurációs. Különböző profilok toodeploy során eltérő tárfiók szolgáltatás is használhatja.

Hello hello varázsló futtatásakor első alkalommal létrejön egy alapértelmezett profilt. A Visual Studio hello-profil tárol az Azure-projekt alatt hello tooyour felvett .azurePubXml kiterjesztéssel rendelkező fájlt **profilok** mappát. Ha manuálisan ad meg más lehetőségek hello varázsló későbbi futtatásakor, hello fájl automatikusan frissíti. A következő eljárás hello futtatása előtt kell már közzétett a felhőalapú szolgáltatáshoz legalább egyszer.

### <a name="tooadd-a-profile"></a>a profil tooadd
1. Nyissa meg az Azure-projekt helyi menüjének hello, és válassza **közzététel**.
2. Következő toohello **céloz profil** listában, jelölje be hello **profil mentése** gomb hello a következő ábra azt mutatja. Ez létrehoz egy profilt.
   
    ![Új profil létrehozása](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)
3. Hello-profil létrehozása után válassza ki a **< kezelés... >** a hello **céloz profil** lista.
   
    Hello **profilok kezelése** párbeszédpanel jelenik meg, a következő ábra azt mutatja be hello.
   
    ![Profilok párbeszédpanel kezelése](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)
4. A hello **neve** listában válassza ki a profilt, és válassza ki **másolat létrehozása**.
5. Válassza ki a hello **Bezárás** gombra.
   
    új hello-profil hello cél profil listájában jelenik meg.
6. A hello **céloz profil** listájában, újonnan létrehozott válassza hello-profil. hello közzététele varázsló beállításainak a program kitölti hello lehetőségek a kiválasztott hello-profil.
7. Jelölje be hello **előző** és **következő** gombok toodisplay hello közzététele varázsló minden lapján, és azután testre szabhatják hello-beállítások megadása ehhez a profilhoz. Lásd: [Azure alkalmazás közzététele varázsló](http://go.microsoft.com/fwlink/p/?LinkID=623085) információt.
8. Hello-beállítások testreszabása befejezése után válassza ki a **következő** toogo hátsó toohello beállítások lapon. hello-profil hello szolgáltatást ezek a beállítások segítségével történő közzétételekor, vagy ha mentett **mentése** profilok következő toohello listája.

### <a name="toorename-or-delete-a-profile"></a>toorename vagy egy profil törlése
1. Nyissa meg az Azure-projekt helyi menüjének hello, és válassza **közzététel**.
2. A hello **céloz profil** listáról válassza ki **kezelése**.
3. A hello **profilok kezelése** párbeszédpanel megnyitásához, jelölje be hello profil toodelete szeretne, és válassza **eltávolítása**.
4. Hello művelet megerősítését kérő párbeszédpanelen megjelenő, válassza ki a **OK**.
5. Válassza ki **Bezárás**.

### <a name="toochange-a-profile"></a>a profil toochange
1. Nyissa meg az Azure-projekt helyi menüjének hello, és válassza **közzététel**.
2. A hello **céloz profil** listájában, a megjeleníteni kívánt toochange válassza hello-profil.
3. Jelölje be hello **előző** és **következő** toodisplay minden egyes oldalának hello közzététele varázsló, majd a kívánt hello beállításainak módosítása gomb. Lásd: [Azure alkalmazás közzététele varázsló](http://go.microsoft.com/fwlink/p/?LinkID=623085) információt.
4. Hello szolgáltatás beállításainak módosítása után válassza ki a **következő** toogo hátsó toohello **beállítások** lap.
5. (Nem kötelező) jelölje ki **közzététel** toopublish hello felhőalapú szolgáltatás hello új beállításokkal. Ha nem szeretné, hogy a felhőszolgáltatás ezúttal, és zárja be toopublish hello közzététele varázsló, a Visual Studio rákérdez, ha azt szeretné, toosave hello módosítások toohello profil.

## <a name="next-steps"></a>Következő lépések
toolearn az Azure-projekt Visual Studio, a többi részével konfigurálásáról lásd: [egy Azure-projekt konfigurálása](http://go.microsoft.com/fwlink/p/?LinkID=623075)

