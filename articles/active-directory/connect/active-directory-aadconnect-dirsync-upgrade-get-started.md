---
title: "Azure AD Connect: Frissítés a DirSyncről | Microsoft Docs"
description: "Megtudhatja, hogyan tooupgrade a DirSync tooAzure AD Connect. Ez a cikk a frissítés a DirSync tooAzure AD Connect hello lépéseit írja le."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: baf52da7-76a8-44c9-8e72-33245790001c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 05572af410698deaa1392c8837bfcb749efc69e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-upgrade-from-dirsync"></a>Azure AD Connect: Frissítés a DirSync szolgáltatásról
Az Azure AD Connect hello követő tooDirSync. Ez a témakör a frissítés a Dirsync szolgáltatásról hello lehetőségeit találja. Ezek lépések nem használhatóak az Azure AD Connect másik kiadásáról vagy az Azure AD Syncről való frissítéshez.

Az Azure AD Connect telepítése előtt győződjön meg arról, hogy túl[töltse le az Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) és teljes hello előzetesen szükséges lépések [az Azure AD Connect: hardver és előfeltételek](active-directory-aadconnect-prerequisites.md). Különösen kívánt tooread kapcsolatos hello következő, mivel ezek a területek nem ugyanaz a Dirsync szolgáltatásról:

* hello .net és a PowerShell verziója szükséges. Újabb verziókra szükséges toobe hello kiszolgálóra DirSync szükség.
* hello proxykiszolgáló-konfigurációt. Ha a proxy server tooreach hello internet, ezt a beállítást kell megadni, mielőtt frissít. A DirSync mindig használja azt hello felhasználóhoz beállított hello proxykiszolgáló, de az Azure AD Connect által használt számítógép-beállítások helyett.
* Nyissa meg a hello URL-címek szükséges toobe hello proxykiszolgálót. Alapszintű forgatókönyvek esetén is támogatja a DirSync azokra hello követelményei, hello azonos. Ha azt szeretné, toouse hello az Azure AD Connect található új szolgáltatások egyikét sem, néhány új URL-címeket kell megnyitni.

> [!NOTE]
> Miután engedélyezte az új Azure AD Connect kiszolgáló toostart szinkronizálási módosítások tooAzure AD, meg kell állítja vissza a DirSync vagy az Azure AD Sync toousing. Többek között a DirSync és Azure AD Sync az Azure AD Connect toolegacy ügyfelek alacsonyabb verziójúra változtatása nem támogatott, és az Azure ad-ben például adatvesztés tooissues vezethet.

Ha nem a DirSync szolgáltatásról frissít, tekintse meg az egyéb forgatókönyvek [vonatkozó dokumentációját](#related-documentation).

## <a name="upgrade-from-dirsync"></a>Frissítés a DirSync szolgáltatásról
Attól függően, hogy a DirSync jelenleg aktív üzemelő hello frissítés különböző lehetőségek közül. Ha hello várt frissítési idő kevesebb mint három órán keresztül, hello ajánljuk, helybeni frissítés toodo. Ha hello várt frissítési idő több mint három órán, hello ajánljuk, toodo párhuzamos üzembe helyezést egy másik kiszolgálón. A becslések, hogy ha több mint 50 000 objektummal rendelkezik tart a több mint három órán keresztül toodo hello frissítését.

| Forgatókönyv |
| --- | --- |
| [Frissítés helyben](#in-place-upgrade) |
| [Párhuzamos üzembe helyezés](#parallel-deployment) |

> [!NOTE]
> Ha azt tervezi, hogy a DirSync tooAzure AD tooupgrade csatlakozás, a dirsyncet hello frissítés előtt. Az Azure AD Connect beolvassa és hello konfiguráció áttelepítése a Dirsync szolgáltatásról és hello kiszolgáló ellenőrzése után távolítsa el.

**Frissítés helyben**  
hello várható ideje toocomplete hello frissítés hello varázsló jelenik meg. Ez a becslés azon három óra toocomplete 50 000 objektumot (felhasználók, partnerek és csoportok) adatbázis frissítés azt veszi hello feltevésen alapulnak. Ha hello az adatbázis-objektumok száma kisebb, mint 50 000, majd az Azure AD Connect azt javasolja, hogy egy frissítés. Ha úgy dönt, hogy a toocontinue, az aktuális beállítások automatikusan alkalmazza a frissítés során, és a kiszolgáló automatikusan folytatja az aktív szinkronizálást.

Ha szeretné, hogy toodo konfiguráció-áttelepítést és párhuzamos üzembe helyezést, majd felülbírálhatja hello helyben történő frissítésre tett javaslatot. Például eltarthat hello lehetőség toorefresh hello hardverre és operációs rendszerre. További információkért lásd: hello [párhuzamos üzembe helyezés](#parallel-deployment) szakasz.

**Párhuzamos üzembe helyezés**  
Ha 50 000-nél több objektummal rendelkezik, párhuzamos üzembe helyezés ajánlott. Ezt az üzembehelyezési módszert választva a felhasználók nem tapasztalnak semmilyen késedelmet a rendszer működésében. hello Azure AD Connect telepítés megkísérli tooestimate hello állásidő hello frissítésre, de ha a korábbi hello végzett korábban is DirSync, saját tapasztalatai alapján valószínűleg toobe hello ajánlott útmutatója.

### <a name="supported-dirsync-configurations-toobe-upgraded"></a>A DirSync-konfiguráció támogatott toobe frissítése
a következő konfigurációs módosításainak hello a frissített DirSync támogatottak:

* Tartományok és szervezeti egységek szűrése
* Másik azonosító (UPN)
* Jelszó-szinkronizálás és Exchange hibrid beállítások
* Erdők/tartományok és az Azure AD beállítási
* Szűrés felhasználói attribútumok alapján

Módosítsa a következő hello nem frissíthető. Ha ezt a konfigurációt, hello frissítés blokkolva van:

* Nem támogatott DirSync-módosítások, például eltávolított attribútumok, vagy egyedi bővítmény-DLL használata

![Frissítés blokkolva](./media/active-directory-aadconnect-dirsync-upgrade-get-started/analysisblocked.png)

Ezekben az esetekben hello ajánljuk, tooinstall egy új Azure AD Connect kiszolgáló [átmeneti módban](active-directory-aadconnectsync-operations.md#staging-mode) , és ellenőrizze hello régi DirSync és az új Azure AD Connect konfigurálása. Alkalmazza újra az összes módosítást egyéni konfiguráció használatával az [Azure AD Connect Sync custom configuration](active-directory-aadconnectsync-whatis.md) (Azure AD Connect szinkronizálása egyéni konfigurációval) szakaszban foglaltak szerint.

a hello szolgáltatás fiókjainak a DirSync által használt hello jelszavakat nem lehet beolvasni, és nem települnek át. Ezek a jelszavak alaphelyzetbe állnak az hello frissítés során.

### <a name="high-level-steps-for-upgrading-from-dirsync-tooazure-ad-connect"></a>AD Connect a DirSync tooAzure frissítés magas szintű lépései
1. Üdvözli a tooAzure AD Connect
2. Az aktuális DirSync-konfiguráció elemzése
3. Az Azure AD globális rendszergazdai jelszavának begyűjtése
4. Egy vállalati rendszergazdai fiók hitelesítő adatainak begyűjtése (csak az Azure AD Connect hello telepítésekor használt)
5. Az Azure AD Connect telepítése
   * A DirSync eltávolítása (vagy ideiglenes letiltása)
   * Az Azure AD Connect telepítése
   * Választhatóan a szinkronizálás indítása

További lépések végrehajtása szükséges, ha:

* Jelenleg teljes SQL Server rendszert használ – akár helyit, akár távolit
* 50 000-nél több objektummal rendelkezik a szinkronizálás hatókörében

## <a name="in-place-upgrade"></a>Frissítés helyben
1. Indítsa el a hello Azure AD Connect telepítőt (MSI).
2. Tekintse át, és fogadja el a toolicense feltételeit és az adatvédelmi nyilatkozatot.  
   ![Üdvözli a tooAzure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Welcome.png)
3. Kattintson a meglévő DirSync telepítés elemzésének következő toobegin.  
   ![A meglévő Directory Sync telepítés elemzése](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Analyze.png)
4. Hello elemzés befejezése után hogyan hello ajánlott megtekintéséhez tooproceed.  
   * Ha SQL Server Expresst használ, és kisebb, mint 50 000 objektummal rendelkezik, a következő képernyő hello jelenik meg:  
     ![Elemzés befejezve, készen áll a tooupgrade a Dirsync szolgáltatásról](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReady.png)
   * Ha teljes SQL Servert használ a DirSynchez, ehelyett a következő képernyő jelenik meg:  
     ![Elemzés befejezve, készen áll a tooupgrade a Dirsync szolgáltatásról](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReadyFullSQL.png)  
     hello hello meglévő SQL Server adatbázis-kiszolgáló DirSync által használt kapcsolatos információk. Amennyiben szükséges, végezze el a megfelelő módosításokat. Kattintson a **következő** toocontinue hello telepítését.
   * Ha 50 000-nél több objektummal rendelkezik, ehelyett a következő képernyő jelenik meg:  
     ![Elemzés befejezve, készen áll a tooupgrade a Dirsync szolgáltatásról](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)  
     tooproceed egy helybeni frissítést, kattintson a jelölőnégyzet következő toothis üdvözlőüzenetére: **folytassa a DirSync frissítését ezen a számítógépen.**
     toodo egy [párhuzamos üzembe helyezés](#parallel-deployment) ehelyett hello DirSync konfigurációs beállítások exportálása, és helyezze át hello konfigurációs toohello új kiszolgálót.
5. Jelenleg használt tooconnect tooAzure AD hello fióknak hello jelszót adjon meg. Ez a DirSync által jelenleg használt hello-fióknak kell lennie.  
   ![Adja meg Azure AD hitelesítő adatait](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToAzureAD.png)  
   Ha hibaüzenetet kap, és problémák adódnak a kapcsolódással, tekintse meg a [Troubleshoot connectivity problems](active-directory-aadconnect-troubleshoot-connectivity.md) (Kapcsolati problémák elhárítása) szakaszt.
6. Adjon meg egy vállalati rendszergazdai fiókot az Active Directory szolgáltatáshoz.  
   ![Adja meg ADDS hitelesítő adatait](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToADDS.png)
7. Most már készen áll a tooconfigure. Amikor az **Upgrade** (Frissítés) gombra kattint, megtörténik a DirSync eltávolítása, az Azure AD Connect konfigurálása és a szinkronizálás indítása.  
   ![Készen áll a tooconfigure](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ReadyToConfigure.png)
8. Miután hello telepítés befejeződött, jelentkezzen ki, és jelentkezzen be újra tooWindows, mielőtt a Synchronization Service Managert, szinkronizálási szabály szerkesztő, vagy próbálja toomake konfigurációs módosításokat.

## <a name="parallel-deployment"></a>Párhuzamos üzembe helyezés
### <a name="export-hello-dirsync-configuration"></a>Hello DirSync-konfiguráció exportálása
**Párhuzamos üzembe helyezés 50 000-nél több objektum esetén**

Ha 50 000-nél több objektummal rendelkezik, majd hello Azure AD Connect telepítés azt javasolja, hogy a párhuzamos üzembe helyezést.

Egy hasonló toohello alábbi képernyő jelenik meg:  
![Elemzés befejeződött](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)

Ha azt szeretné, hogy a párhuzamos üzembe helyezést tooproceed, kell tooperform hello a következő lépéseket:

* Kattintson a hello **beállítások exportálása** gombra. Ha az Azure AD Connect különálló kiszolgálóra telepíti, ezeket a beállításokat az aktuális DirSync tooyour új Azure AD Connect telepítés települnek át.

Ha a beállítások exportálása sikeresen megtörtént, kiléphet hello Azure AD Connect varázsló hello DirSync kiszolgálón. Folytassa a hello következő túl[Azure AD Connect telepítése különálló kiszolgálóra](#installation-of-azure-ad-connect-on-separate-server)

**Párhuzamos üzembe helyezés 50 000-nél kevesebb objektummal**

Ha 50 000-nél kevesebb objektummal rendelkezik, de továbbra is toodo párhuzamos üzembe helyezést, majd hello a következő:

1. Futtassa a hello Azure AD Connect telepítőt (MSI).
2. Amikor megjelenik a hello **üdvözlő tooAzure AD Connect** képernyőn, kilépési hello telepítővarázsló hello jobb felső sarkában hello ablak hello "X" elemre kattintva.
3. Nyisson meg egy parancssort.
4. A hello telepítési a helyet az Azure AD Connect (alapértelmezett: C:\Program Files\Microsoft Azure Active Directory Connect) hajtsa végre a következő parancs hello: `AzureADConnect.exe /ForceExport`.
5. Kattintson a hello **beállítások exportálása** gombra. Ha az Azure AD Connect különálló kiszolgálóra telepíti, ezeket a beállításokat az aktuális DirSync tooyour új Azure AD Connect telepítés települnek át.

![Elemzés befejeződött](./media/active-directory-aadconnect-dirsync-upgrade-get-started/forceexport.png)

Ha a beállítások exportálása sikeresen megtörtént, kiléphet hello Azure AD Connect varázsló hello DirSync kiszolgálón. Folytassa a hello következő túl[Azure AD Connect telepítése különálló kiszolgálóra](#installation-of-azure-ad-connect-on-separate-server).

### <a name="install-azure-ad-connect-on-separate-server"></a>Az Azure AD Connect telepítése különálló kiszolgálóra
Az Azure AD Connect egy új kiszolgálóra történő telepítésekor hello feltételezése, amelyet az tooperform az Azure AD Connect tiszta telepítését. Hiszen toouse hello DirSync-konfiguráció, van néhány további lépést tootake:

1. Futtassa a hello Azure AD Connect telepítőt (MSI).
2. Amikor megjelenik a hello **üdvözlő tooAzure AD Connect** képernyőn, kilépési hello telepítővarázsló hello jobb felső sarkában hello ablak hello "X" elemre kattintva.
3. Nyisson meg egy parancssort.
4. A hello telepítési a helyet az Azure AD Connect (alapértelmezett: C:\Program Files\Microsoft Azure Active Directory Connect) hajtsa végre a következő parancs hello: `AzureADConnect.exe /migrate`.
   hello Azure AD Connect telepítővarázsló elindul, és megjeleníti a következő képernyő hello:  
   ![Adja meg Azure AD hitelesítő adatait](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ImportSettings.png)
5. Válassza ki a DirSync-telepítésből exportált beállításfájl hello.
6. Konfigurálja az esetleges speciális beállításokat, beleértve a következőket:
   * Egyedi telepítési helyet adhat meg az Azure AD Connect számára.
   * Megadhatja az SQL Server egy meglévő példányát (alapértelmezett beállítás: az Azure AD Connect telepíti az SQL Server 2012 Expresst). Ne használjon hello ugyanazt az adatbázispéldányt a DirSync-kiszolgálóval.
   * Szolgáltatásfiók használja a tooconnect tooSQL Server (ha az SQL Server adatbázis távoli akkor ennek a fióknak tartományi szolgáltatásfióknak kell lennie).
     Ezek a beállítások láthatók ezen a képernyőn:  
     ![Adja meg Azure AD hitelesítő adatait](./media/active-directory-aadconnect-dirsync-upgrade-get-started/advancedsettings.png)
7. Kattintson a **Tovább** gombra.
8. A hello **készen tooconfigure** lapján hello hagyja **hello szinkronizálási folyamat indítása, amint hello konfigurálás befejeződik** be van jelölve. hello kiszolgáló jelenleg a [átmeneti módban](active-directory-aadconnectsync-operations.md#staging-mode) így a módosításokat nem exportált tooAzure AD.
9. Kattintson az **Install** (Telepítés) gombra.
10. Miután hello telepítés befejeződött, jelentkezzen ki, és jelentkezzen be újra tooWindows, mielőtt a Synchronization Service Managert, szinkronizálási szabály szerkesztő, vagy próbálja toomake konfigurációs módosításokat.

> [!NOTE]
> Windows Server Active Directory és az Azure Active Directory közötti szinkronizálás kezdődik, de nem végez módosítást exportált tooAzure AD. Egy időben csupán egyetlen szinkronizálási eszköz exportálhat aktívan módosításokat. Ezt az állapotot nevezzük [átmeneti módnak](active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-that-azure-ad-connect-is-ready-toobegin-synchronization"></a>Győződjön meg arról, hogy az Azure AD Connect készen toobegin szinkronizálás
tooopen szüksége, amely az Azure AD Connect a Dirsync szolgáltatásról keresztül készen tootake tooverify, **Synchronization Service Managert** hello csoport **az Azure AD Connect** hello start menüből.

Hello alkalmazásban nyissa meg toohello **műveletek** fülre. Ezen a lapon erősítse meg, hogy a következő műveletek hello befejeződött:

* Importálhatja a hello AD Connectoron
* Importálhatja a hello Azure AD Connectoron
* Teljes szinkronizálás az hello AD Connectoron
* Teljes szinkronizálás az Azure AD-összekötő hello

![Importálás és szinkronizálás befejezve](./media/active-directory-aadconnect-dirsync-upgrade-get-started/importsynccompleted.png)

Tekintse át ezeket a műveleteket hello eredményt, és győződjön meg arról, nincsenek hibák.

Ha szeretné, hogy toosee, és vizsgálja meg az exportált toobe tooAzure AD kapcsolatos, hello módosításokat, majd beolvassa hogyan tooverify hello konfigurációja [átmeneti módban](active-directory-aadconnectsync-operations.md#staging-mode). Hajtsa végre a szükséges módosításokat, amíg semmi váratlant nem lát már.

A DirSync tooAzure AD, ha elvégezte ezeket a lépéseket, és nem gond hello eredménnyel készen tooswitch áll.

### <a name="uninstall-dirsync-old-server"></a>A DirSync eltávolítása (régi kiszolgáló)
* A **Programok és szolgáltatások** felületen keresse meg a **Windows Azure Active Directory sync tool** eszközt
* Távolítsa el a **Windows Azure Active Directory sync tool** eszközt
* hello eltávolítása is igénybe vehet fel too15 perc toocomplete.

DirSync toouninstall később tetszés szerint átmenetileg hello kiszolgáló leállítása vagy letiltása hello szolgáltatást. Ha valamilyen hiba, ez a módszer lehetővé teszi toore engedélyezése azt. Azonban nem valószínű, hogy hello tovább lépés sikertelen lesz, ezért ennek kell nem lehet szükség.

A DirSync eltávolítása, vagy le van tiltva nincs aktív kiszolgáló tooAzure AD exportálása. hello tovább lépés tooenable az Azure AD Connect előbb kell befejeződnie a helyszíni Active Directoryban módosításokat továbbra is szinkronizálja toobe tooAzure AD.

### <a name="enable-azure-ad-connect-new-server"></a>Az Azure AD Connect engedélyezése (új kiszolgáló)
A telepítés után újból megnyitni az Azure AD connect lesz toomake további konfigurációmódosításokat lehetővé teszik. Start **az Azure AD Connect** hello start menüből vagy a hello hello asztali parancsikonjára. Ellenőrizze, hogy nem próbálja meg toorun hello telepítő MSI-t újra.

Hello következő kell megjelennie:  
![További feladatok](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AdditionalTasks.png)

* Válassza a **Configure staging mode** (Átmeneti mód konfigurálása) lehetőséget.
* Kapcsolja ki az átmeneti eszközhitelesítést hello által **az átmeneti környezetű üzemmód engedélyezve** jelölőnégyzetet.

![Adja meg Azure AD hitelesítő adatait](./media/active-directory-aadconnect-dirsync-upgrade-get-started/configurestaging.png)

* Kattintson a hello **következő** gomb
* A hello megerősítési oldalán kattintson a hello **telepítése** gombra.

Az Azure AD Connect jelenleg az aktív kiszolgáló, és meg nem vált vissza toousing a meglévő DirSync kiszolgáló.

## <a name="next-steps"></a>Következő lépések
Most, hogy az Azure AD Connect telepítése is [hello telepítésének ellenőrzése és licencek hozzárendelése](active-directory-aadconnect-whats-next.md).

További információk, amelyek hello telepítéssel engedélyezett új szolgáltatásokkal: [automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md), [véletlen törlések megakadályozása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md), és [az Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Tudjon meg többet a következő általános témaköröket: [Feladatütemező, és hogyan tootrigger szinkronizálása](active-directory-aadconnectsync-feature-scheduler.md).

További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
