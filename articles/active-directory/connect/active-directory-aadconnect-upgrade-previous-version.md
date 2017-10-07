---
title: "Az Azure AD Connect: Frissítés egy korábbi verzióról |} Microsoft Docs"
description: "Hello különböző módszereket tooupgrade toohello legújabb kiadása Azure Active Directory Connect, beleértve a helyben frissítés és egy mozgó áttelepítési ismerteti."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 31f084d8-2b89-478c-9079-76cf92e6618f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 57bd5b094654e4983cafa303b6f3daecadafb01c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-upgrade-from-a-previous-version-toohello-latest"></a>Az Azure AD Connect: Frissítés a legújabb egy korábbi verziója toohello
Ez a témakör ismerteti a használható tooupgrade az Azure Active Directory (Azure AD) Connect telepítési toohello legújabb kiadásának hello különböző módszereket. Azt javasoljuk, hogy őrizze meg az Azure AD Connect hello kiadásainak aktuális. Hello hello lépéseket is használhat [áttelepítési éppen](#swing-migration) szakaszában, amikor olyan jelentős konfigurációs módosítást hajt végre.

Ha azt szeretné, hogy a Dirsync szolgáltatásról tooupgrade, [Azure AD Szinkronizáló eszközéről (DirSync) verzióról](active-directory-aadconnect-dirsync-upgrade-get-started.md) helyette.

Nincsenek használható az Azure AD Connect tooupgrade néhány különböző stratégiákat.

| Módszer | Leírás |
| --- | --- |
| [Automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md) |Ez a hello legegyszerűbb módszer az expressz telepítési rendelkező ügyfelek esetén. |
| [Frissítés helyben](#in-place-upgrade) |Ha egy önálló kiszolgáló, frissítheti a hello telepítési helyben hello ugyanarra a kiszolgálóra. |
| [Áttelepítési éppen](#swing-migration) |Két kiszolgálóval készítse elő hello hello új kiadás vagy konfigurációs kiszolgálók, és módosítsa a hello aktív kiszolgáló, amikor készen áll. |

Engedélyek információkért lásd: hello [a frissítéshez szükséges engedélyek](active-directory-aadconnect-accounts-permissions.md#upgrade).

> [!NOTE]
> Az új Azure AD Connect kiszolgáló toostart szinkronizálási módosítások tooAzure AD engedélyezése után meg kell állítja vissza a DirSync vagy az Azure AD Sync toousing. Alacsonyabb verziójúra változtatása az Azure AD Connect toolegacy ügyfelekről, beleértve a DirSync és Azure AD Sync programot, nem támogatott, és az Azure ad-ben például adatvesztés tooissues vezethet.

## <a name="in-place-upgrade"></a>Frissítés helyben
Egy frissítés tér át Azure AD Sync vagy az Azure AD Connect működik. A Dirsync szolgáltatásról áthelyezésére vagy megoldás a Forefront Identity Manager (FIM) + Azure AD-összekötő nem működik.

Ez a módszer használata ajánlott, ha egy önálló kiszolgáló és kisebb, mint körülbelül 100 000 objektumok. A módosításokat toohello out-of-box szinkronizálási szabályok vonatkoznak, ha egy teljes importálást és teljes szinkronizálást fordulhat elő, hello frissítés után. Ez a módszer biztosítja, hogy hello új konfigurációt alkalmazott tooall meglévő objektumok hello rendszerben. Ehhez a futtató órára is szükség lehet néhány, attól függően, hogy hello hello szinkronizálási motor hatókörében lévő objektumok száma. hello normál különbözeti szinkronizálás Feladatütemező (amely alapértelmezés szerint 30 percenként szinkronizál) fel van függesztve, de a jelszó-szinkronizálás továbbra is. Érdemes valamilyen módon hello helybeni frissítést a hétvégén. Ha hello új Azure AD Connect kiadással módosítások toohello out-of-box konfigurálásra, majd normál különbözeti importálás/szinkronizálási indítása helyette.  
![Frissítés helyben](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Ha végrehajtott módosítások toohello out-of-box szinkronizálási szabályokat, majd ezek a szabályok állítja vissza toohello alapértelmezett konfigurációja a frissítés. arról, hogy a konfigurációs tárolódik a frissítések közötti toomake győződjön meg arról, hogy módosítania folyamatban ismertetett módon [ajánlott eljárások a hello alapértelmezett konfiguráció módosítása](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

Helybeni verziófrissítés során előfordulhat módosítások bevezetett meghatározott szinkronizálási tevékenységeket (beleértve a teljes importálás és a teljes szinkronizálás lépést) toobe végrehajtása a frissítés befejezése után. toodefer ilyen tevékenységek, tekintse meg a toosection [hogyan toodefer teljes szinkronizálás a frissítés után](#how-to-defer-full-synchronization-after-upgrade).

## <a name="swing-migration"></a>Párhuzamos migrálás
Ha egy összetett üzembe helyezése vagy több objektumot, nem célszerű toodo hello élő rendszer helyben frissítés lehet. Egyes ügyfelek esetén ez a folyamat eltarthat több nap –, és ebben az időszakban nem változáskülönbözeteit feldolgozása. Is használhatja ezt a módszert a toomake jelentős módosításokat tooyour konfigurációjának megtervezése és tootry ahhoz, azok közben leküldött toohello felhő ki őket.

módszer forgatókönyvek esetén ajánlott hello toouse mozgó áttelepítés. (Legalább) két kiszolgáló – egy aktív kiszolgáló és egy átmeneti kiszolgálón van szüksége. (lásd az alábbi képen hello teli kék vonallal) hello aktív kiszolgáló felelős hello aktív üzemi terhelés. hello új kiadás vagy konfigurációs kiszolgálóhoz (lila szaggatott vonallal) átmeneti hello kész. Amikor teljesen készen áll, a kiszolgáló akkor aktív. hello előző aktív kiszolgáló, amely most hello régi verziója vagy a konfigurációs telepítve van, a kiszolgáló átmeneti hello történik, és frissítése.

két kiszolgáló hello különböző verzióit is használhatják. Például hello aktív kiszolgáló toodecommission megtervezni használhatja az Azure AD Sync, és új átmeneti kiszolgálón hello használhatja az Azure AD Connect. Ha mozgó áttelepítési toodevelop egy új konfigurációt, az a jó ötlet toohave hello ugyanezen verziókban a hello két kiszolgálóval.  
![Átmeneti kiszolgáló](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

> [!NOTE]
> Egyes ügyfelek előnyben részesítik a három vagy négy toohave kiszolgálók ehhez a forgatókönyvhöz. Kiszolgáló átmeneti hello frissítésekor a biztonsági kiszolgáló nem rendelkezik [vész-helyreállítási](active-directory-aadconnectsync-operations.md#disaster-recovery). Három vagy négy kiszolgálókkal előkészítheti a hello új verziójára, amely biztosítja, hogy nincs-e mindig egy átmeneti kiszolgálón, amely készen áll a tootake keresztül elsődleges vagy készenléti állapotban lévő kiszolgálók egy csoportja.

Ezeket a lépéseket az Azure AD Sync vagy az Azure AD-összekötő FIM kiegészítve megoldás toomove is együttműködik. Ezeket a lépéseket a Dirsync nem működnek, de ugyanazon mozgó áttelepítési metódust használ (más néven párhuzamos üzembe helyezés) lépések DirSync hello [frissítés Azure Active Directory-szinkronizálás (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="use-a-swing-migration-tooupgrade"></a>Egy mozgó áttelepítési tooupgrade használata
1. Ha a kiszolgálók és a terv tooonly ellenőrizze a konfiguráció módosítása az Azure AD Connect használ, ellenőrizze, hogy az aktív kiszolgáló és az átmeneti kiszolgálón egyaránt használatával hello ugyanazt a verziót. Amelyek révén könnyebben toocompare különbségek később. Ha az Azure AD-Szinkronizálóról frissít, ezeket a kiszolgálókat különböző verziói működnek. Ha frissít, az Azure AD Connect egy korábbi verziójából származó, a rendszer egy jó ötlet toostart hello két kiszolgálókkal, amelyek segítségével hello ugyanazt a verziót, de ez nem szükséges.
2. Ha egy egyéni konfigurációt végrehajtott, és az átmeneti kiszolgálón nincs, lépésekkel hello alatt [egyéni konfiguráció áthelyezése hello aktív kiszolgáló toohello kiszolgáló átmeneti](#move-custom-configuration-from-active-to-staging-server).
3. Ha frissít, az Azure AD Connect egy korábbi kiadásáról, frissítse az hello átmeneti server toohello legújabb verzióra. Ha az Azure AD-Szinkronizálóról telepít át, majd az Azure AD Connect telepítése az átmeneti kiszolgálón.
4. Lehetővé teszik a hello szinkronizálási motor futtatása teljes importálást és teljes szinkronizálást az átmeneti kiszolgálón.
5. Győződjön meg az adott hello új konfiguráció nem következtében a nem várt módosítások a "Hitelesítés" hello lépések segítségével [ellenőrizze hello konfigurációs kiszolgáló](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server). Ha valami nem a várt, helyes-e, futtassa a hello importálás és szinkronizálás, és ellenőrizze a hello adatokat, amíg azt megfelelőnek tűnik, hello lépéseket követve.
6. Kiszolgáló toobe hello aktív kiszolgáló átmeneti hello váltani. Ez az utolsó lépés hello "Kapcsoló aktív kiszolgáló" a [ellenőrizze hello konfigurációs kiszolgáló](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Ha a frissítés során az Azure AD Connect frissítése van a most átmeneti mód toohello legújabb kiadásának hello kiszolgáló. Hajtsa végre az azonos előtt tooget hello adatok és a frissített konfigurációs lépések hello. Ha frissített az Azure AD-Szinkronizálóról, ezután kapcsolja ki a és a régi kiszolgáló leszerelése.

### <a name="move-a-custom-configuration-from-hello-active-server-toohello-staging-server"></a>Egyéni konfiguráció áthelyezése hello aktív toohello átmeneti kiszolgálót.
Ha végrehajtott konfigurációs módosításokat toohello aktív kiszolgáló, szükség toomake meg arról, hogy azonos hello változtatások kiszolgáló átmeneti alkalmazott toohello. Ennek toohelp helyezi át, használhatja a hello [az Azure AD Connect konfigurációs dokumentáló](https://github.com/Microsoft/AADConnectConfigDocumenter).

Áthelyezheti hello egyéni szinkronizálási szabályokat, hogy a PowerShell használatával létrehozott. Telepítenie kell az egyéb módosítások hello ugyanúgy egyaránt rendszerek, és nem telepíthető át hello módosításokat. Hello [konfigurációs dokumentáló](https://github.com/Microsoft/AADConnectConfigDocumenter) hello két rendszerek toomake meg arról, hogy azok egyezését összehasonlításával nyújt segítséget. Ebben a szakaszban található hello lépéseket automatizálását is segíthetnek hello eszköz.

Tooconfigure hello következőkre lesz szüksége dolgot hello azonos módon mindkét kiszolgálón:

* Kapcsolat toohello azonos erdőben
* Bármely tartomány és szervezeti egységek szűrése
* hello azonos választható funkciók, például a jelszó-szinkronizálás és jelszóvisszaíró

**Helyezze át az egyéni szinkronizálási szabályok**  
toomove egyéni szinkronizálási szabályait, hello a következő:

1. Nyissa meg **szinkronizálási szabályok szerkesztő** az aktív kiszolgálón.
2. Jelöljön ki egy egyéni szabályt. Kattintson a **exportálása**. Ekkor megjelenik a Jegyzettömb ablak. Hello ideiglenes fájl mentése PS1 kiterjesztéssel. Így egy PowerShell-parancsfájlt. Hello PS1 fájl toohello kiszolgáló átmeneti másolja.  
   ![Szinkronizálási szabály exportálása](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. hello összekötő GUID kiszolgáló átmeneti hello eltérő, és akkor kell megváltoztatnia. tooget hello GUID, indítsa el **szinkronizálási szabályok szerkesztő**, válasszon egyet a hello out-of-box szabályok azonos rendszer csatlakozott, majd kattintson az adott jelentik hello **exportálása**. Cserélje le a kiszolgáló átmeneti hello GUID hello hello GUID az a PS1 fájlnevet.
4. A PowerShell-parancssorból futtassa a hello PS1 fájlt. Ez server átmeneti hello hello egyéni szinkronizálási szabályt hoz létre.
5. Ismételje meg ezt az egyéni szabályok.

## <a name="how-toodefer-full-synchronization-after-upgrade"></a>Hogyan toodefer teljes szinkronizálás a frissítés után
Helybeni verziófrissítés során előfordulhat bevezetett változások meghatározott szinkronizálási tevékenységeket (beleértve a teljes importálás és a teljes szinkronizálás lépést) toobe végre igénylő. Például összekötő séma módosításához **teljes importálás** lépés és out-of-box szinkronizálási szabály módosításához **teljes szinkronizálás** lépést végre a fertőzött összekötők toobe. A frissítés során az Azure AD Connect meghatározza, hogy milyen szinkronizálási tevékenységek szükségesek, és rögzíti őket *felülbírálások*. A következő szinkronizálási ciklusban hello hello szinkronizálási Feladatütemező szerzi be ezeket a felülbírálásokat, és végrehajtja azokat. Felülbírálás sikeresen végre, ha eltávolítják azt.

Előfordulhat, hogy esetekben nem célszerű a felülbírálások tootake hely közvetlenül a frissítés után. Például számos szinkronizált objektummal rendelkezik, és azt szeretné, a szinkronizálás lépéseket toooccur munkaidő után. Ezek a felülbírálások tooremove:

1. A frissítés során **, törölje a jelet** beállítás hello **hello szinkronizálási folyamat indítása, ha a konfiguráció befejezése**. Ezzel letiltja a hello szinkronizálási Feladatütemező, és megakadályozza, hogy a szinkronizálási ciklust megakadályozhat automatikusan hello felülbírálások eltávolítása előtt.

   ![DisableFullSyncAfterUpgrade](./media/active-directory-aadconnect-upgrade-previous-version/disablefullsync01.png)

2. Frissítés befejezése után futtassa a következő parancsmag toofind, mely a felülbírálások hozzá vannak adva hello:`Get-ADSyncSchedulerConnectorOverride | fl`

   >[!NOTE]
   > hello felülbírálások connector-specifikus. Hello a következő példa, a teljes importálás és a teljes szinkronizálás lépést hozzáadott tooboth hello a helyszíni AD-összekötő és az Azure AD-összekötőt.

   ![DisableFullSyncAfterUpgrade](./media/active-directory-aadconnect-upgrade-previous-version/disablefullsync02.png)

3. Jegyezze fel, amelyek hozzá vannak adva hello meglévő felülbírálások.
   
4. tooremove hello felülbírálja a teljes importálás és a teljes szinkronizálás egy tetszőleges összekötőn, futtassa a következő parancsmag hello:`Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid-of-ConnectorIdentifier> -FullImportRequired $false -FullSyncRequired $false`

   tooremove hello felülbírálások összekötők, a következő PowerShell-parancsfájl hello hajtható végre:

   ```
   foreach ($connectorOverride in Get-ADSyncSchedulerConnectorOverride)
   {
       Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier $connectorOverride.ConnectorIdentifier.Guid -FullSyncRequired $false -FullImportRequired $false
   }
   ```

5. tooresume hello Feladatütemező, futtassa a következő parancsmag hello:`Set-ADSyncScheduler -SyncCycleEnabled $true`

   >[!IMPORTANT]
   > Ne felejtse el tooexecute szükséges hello szinkronizálásának lépései legkorábbi tetszés. Manuálisan hajtható végre ezeket a lépéseket hello Synchronization Service Manager használatával, vagy hello felülbírálások vissza a hello Set-ADSyncSchedulerConnectorOverride parancsmag használatával adja hozzá.

tooadd hello felülbírálja a teljes importálás és a teljes szinkronizálás egy tetszőleges összekötőn, futtassa a következő parancsmag hello:`Set-ADSyncSchedulerConnectorOverride -ConnectorIdentifier <Guid> -FullImportRequired $true -FullSyncRequired $true`

## <a name="next-steps"></a>Következő lépések
További információ [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
