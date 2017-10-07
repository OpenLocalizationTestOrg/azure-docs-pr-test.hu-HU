---
title: "Azure AD Connect szinkronizálása: ütemező |} Microsoft Docs"
description: "Ez a témakör ismerteti az Azure AD Connect szinkronizálási szolgáltatás hello beépített ütemezési szolgáltatása."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b1a598f-89c0-4244-9b20-f4aaad5233cf
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: c587039cc68d305862a07beff364894b6f74cd2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Connect szinkronizálása: a Feladatütemező
Ez a témakör ismerteti (más néven hello beépített ütemezési az Azure AD Connect szinkronizálási szolgáltatás a szinkronizálási motor).

Ez a funkció a build 1.1.105.0 (dátuma: 2016. februári) ban jelent meg.

## <a name="overview"></a>Áttekintés
Azure AD Connect szinkronizálása a változásokat a Feladatütemező használatával a helyszíni címtárral szinkronizálja. Nincsenek két Feladatütemező folyamatok, jelszó-szinkronizálás egyet, majd egy másikat a szinkronizálás és a karbantartási feladatok eszközobjektum-attribútum. Ez a témakör hello ez utóbbi tartalmazza.

A korábbi kiadásokban az objektumok és attribútumok hello Feladatütemező külső toohello szinkronizálási motor volt. A Windows Feladatütemező vagy egy különálló Windows szolgáltatás tootrigger hello szinkronizálási folyamat használja azt. hello Feladatütemező a szinkronizálási motor 1.1 kiadásokban beépített toohello hello és néhány testreszabásának engedélyezése. hello új alapértelmezett szinkronizálási gyakoriság értéke 30 perc.

hello Feladatütemező felelős két feladatot:

* **Szinkronizálási ciklust**. hello folyamat tooimport, szinkronizálja, és exportálja a módosításokat.
* **Karbantartási feladatok**. Újítsa meg a kulcsoknak és tanúsítványoknak a jelszó alaphelyzetbe állítása és az Eszközregisztrációs szolgáltatás (DRS). Régi bejegyzések hello műveleti napló kiürítése.

hello maga mindig fut, de konfigurált tooonly futtatnia kell egy, vagy nincs ezeket a feladatokat. Például ha toohave kell a saját szinkronizálási ciklust folyamat, letilthatja ezt a feladatot az hello Feladatütemező, de továbbra is futtassa hello karbantartási feladatot.

## <a name="scheduler-configuration"></a>Ütemezési konfiguráció
toosee a jelenlegi konfigurációs beállításokat, nyissa meg tooPowerShell, és futtassa `Get-ADSyncScheduler`. Jelzi, hogy a kép hasonlót:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings2016.png)

Ha látja **hello sync parancsot vagy a parancsmag nem áll rendelkezésre** Ez a parancsmag futtatásakor majd hello PowerShell-modulja nincs betöltve. Ez a probléma akkor fordulhat elő, ha az Azure AD Connect futtatása egy tartományvezérlőn vagy magasabb PowerShell korlátozási szintű mint hello alapértelmezett beállítások a kiszolgálón. Ha ezt a hibaüzenetet látja, majd futtassa `Import-Module ADSync` toomake hello parancsmag érhető el.

* **AllowedSyncCycleInterval**. hello legrövidebb között eltelt idő szinkronizálási ciklust az Azure AD által engedélyezett. Nem lehet szinkronizálni a gyakrabban ezt a beállítást, és továbbra is támogatott.
* **CurrentlyEffectiveSyncCycleInterval**. hello ütemezés jelenleg érvényben. Ugyanaz, mint CustomizedSyncInterval érték hello rendelkezik (ha beállítása) Ha még nincs mint AllowedSyncInterval. Ha 1.1.281 előtt build használja, és CustomizedSyncCycleInterval módosítja, a módosítás után a következő szinkronizálási ciklusban lép életbe. A build 1.1.281 hello módosítása azonnal érvénybe lép.
* **CustomizedSyncCycleInterval**. Ha hello Feladatütemező toorun gyakorisággal bármely más hello alapértelmezett mint 30 percet, majd konfigurálja ezt a beállítást. A fenti hello kép hello Feladatütemező van beállítva toorun óránként helyette. Ha ez a beállítás tooa érték AllowedSyncInterval alacsonyabb, ez utóbbi hello szolgál.
* **NextSyncCyclePolicyType**. Különbözeti vagy kezdeti. Meghatározza, hogy a következő futtatás hello kell csak a folyamat változási különbözeteket, vagy ha hello a következő futtatáskor kell tennie egy teljes importálása és szinkronizálás. Ez utóbbi hello is újból feldolgozza, hogy minden új vagy módosított szabályok.
* **NextSyncCycleStartTimeInUTC**. Hello Feladatütemező következő indításakor hello a következő szinkronizálási ciklusban.
* **PurgeRunHistoryInterval**. hello time művelet naplók kell tartani. Ezek a naplók hello synchronization service Managert tekinthetők át. alapértelmezett hello 7 napig ezek a naplók tookeep van.
* **SyncCycleEnabled**. Azt jelzi, hogy hello Feladatütemező fut-e exportálási folyamatok, hello importálása és szinkronizálás a művelet részeként.
* **MaintenanceEnabled**. Azt jelzi, ha engedélyezve van-e a hello karbantartási folyamata. Hello tanúsítványok vagy kulcsok frissíti, és pon hello műveleti napló.
* **StagingModeEnabled**. Bemutatja, ha [átmeneti módban](active-directory-aadconnectsync-operations.md#staging-mode) engedélyezve van. Ha engedélyezi ezt a beállítást, majd nem jelenít meg hello kivitel futtatását, de továbbra is futnak, importálása és szinkronizálás.
* **SchedulerSuspended**. Connect során be egy frissítési tootemporarily blokk hello Feladatütemező futtatását.

Módosíthatja az egyes beállításokat a `Set-ADSyncScheduler`. a következő paraméterek hello módosíthatja:

* CustomizedSyncCycleInterval
* NextSyncCyclePolicyType
* PurgeRunHistoryInterval
* SyncCycleEnabled
* MaintenanceEnabled

Az Azure AD Connect korábbi buildekben **isStagingModeEnabled** a Set-ADSyncScheduler volt kitéve. Az **nem támogatott** tooset ezt a tulajdonságot. hello tulajdonság **SchedulerSuspended** Connect csak kell módosítani. Az **nem támogatott** tooset Ez közvetlenül a PowerShell használatával.

az Azure AD hello ütemezési konfiguráció tárolja. Ha egy átmeneti kiszolgálón, hello elsődleges kiszolgáló bármilyen változás is érinti hello átmeneti kiszolgálón (kivéve a IsStagingModeEnabled).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Szintaxis:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - nap, HH - órák, pp - perc, mm - másodperc

Példa:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Módosítások hello Feladatütemező toorun 3 óránként.

Példa:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Megváltoztatása hello Feladatütemező toorun naponta.

### <a name="disable-hello-scheduler"></a>Tiltsa le a hello Feladatütemező  
Ha toomake konfigurációs módosításokat, majd kívánt toodisable hello Feladatütemező. Például, amikor Ön [szűrésének beállítása](active-directory-aadconnectsync-configure-filtering.md) vagy [módosításokat toosynchronization szabályok](active-directory-aadconnectsync-change-the-configuration.md).

toodisable hello Feladatütemező futtatása `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![Tiltsa le a hello Feladatütemező](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)

Ha a módosításokat hajtott végre, ne feledje tooenable hello Feladatütemező újra `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="start-hello-scheduler"></a>Hello a Feladatütemező indítása
hello Feladatütemező 30 percenként futtatása alapértelmezés szerint ki van. Bizonyos esetekben érdemes lehet a szinkronizálási ciklust Between hello toorun ütemezett ciklusok vagy más típusú toorun van szüksége.

**A különbözeti szinkronizálási ciklusban**  
A különbözeti szinkronizálási ciklusban hello a következő lépéseket tartalmazza:

* Különbözeti importálás az összekötők
* Különbözeti szinkronizálás az összekötők
* Az összekötők exportálása

Annak oka az lehet, hogy rendelkezik olyan sürgős módosítást szinkronizálni kell azonnal, ezért szüksége toomanually egy ciklus futtatására. Ha toomanually kell ciklust, majd futtassa a PowerShell futási `Start-ADSyncSyncCycle -PolicyType Delta`.

**Teljes szinkronizálás ciklus**  
Ha hello a következő konfigurációs módosítások történtek, akkor toorun egy teljes szinkronizálási ciklust (más néven Kezdeti):

* Importált forráskönyvtárat további objektumokat vagy attribútumok toobe hozzáadva
* A végrehajtott módosítások toohello szinkronizálási szabályok
* Módosított [szűrés](active-directory-aadconnectsync-configure-filtering.md) , a különböző számú objektum tartalmaznia kell

Ha ezek a változások történtek, akkor szüksége egy teljes szinkronizálási ciklust így hello szinkronizálási motor hello lehetőség tooreconsolidate hello összekötőterek toorun. Egy teljes szinkronizálási ciklust hello a következő lépéseket tartalmazza:

* Teljes importálás az összekötők
* Teljes szinkronizálás az összekötők
* Az összekötők exportálása

egy teljes szinkronizálási ciklust tooinitiate futtatása `Start-ADSyncSyncCycle -PolicyType Initial` egy PowerShell-parancssorból. Ez a parancs elindít egy teljes szinkronizálási ciklust.

## <a name="stop-hello-scheduler"></a>Hello a Feladatütemező leállítása
Ha hello Feladatütemező jelenleg fut egy szinkronizálási ciklust, szükség lehet a toostop azt. Ha például hello telepítési varázsló elindításához, és ez a hibaüzenet:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

A szinkronizálási ciklusban futtatásakor konfigurációs módosítások nem hajtható végre. Sikerült megvárni, amíg hello Feladatütemező hello folyamat befejeződött, de is leállíthatja azt, hogy a módosítások azonnal. Hello leállítása aktuális ciklus nincs káros és függőben lévő módosítások dolgozza fel, ezután futtassa.

1. Kezdő történő hello Feladatütemező toostop jelenlegi hello PowerShell-parancsmaggal ciklus `Stop-ADSyncSyncCycle`.
2. Ha egy elkészítési 1.1.281 előtt, akkor hello a Feladatütemező leállítása nem állítja le hello aktuális összekötő az aktuális feladatot. tooforce hello összekötő toostop, a következő műveletek hello érvénybe: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
   * Start **szinkronizálási szolgáltatás** hello start menüből. Nyissa meg túl**összekötők**, hello összekötő hello állapotú jelöljön ki **futtató**, válassza ki **leállítása** hello műveletek a.

hello Feladatütemező még mindig aktív, és újra elindul a Tovább lehetőséget.

## <a name="custom-scheduler"></a>Egyéni Feladatütemező
hello ebben a szakaszban leírt parancsmagok érhetők el csak építés [1.1.130.0](active-directory-aadconnect-version-history.md#111300) és újabb verziók.

Ha hello beépített ütemezési nem felel meg a követelményeknek, majd ütemezheti hello összekötők PowerShell használatával.

### <a name="invoke-adsyncrunprofile"></a>Invoke-ADSyncRunProfile
A profil egy összekötő ily módon megkezdése:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

hello nevek toouse a [összekötő nevek](active-directory-aadconnectsync-service-manager-ui-connectors.md) és [futtassa a profilnév](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) hello található [Synchronization Service Manager felhasználói felületén](active-directory-aadconnectsync-service-manager-ui.md).

![Futtatási profil meghívása](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

Hello `Invoke-ADSyncRunProfile` szinkron parancsmag, ez azt jelenti, hogy azt nem ad vissza vezérlő csak hello összekötő befejezése után hello művelet sikeresen vagy hibával.

Az összekötők ütemezésekor hello ajánlás tooschedule-e a sorrend hello őket:

1. (Teljes vagy különbözeti) A helyszíni címtárakat, például az Active Directory importálása
2. (Teljes vagy különbözeti) Az Azure AD importálása
3. (Teljes vagy különbözeti) A helyszíni címtárakat, például az Active Directory-szinkronizálás
4. (Teljes vagy különbözeti) Az Azure AD szinkronizálási
5. Exportálás tooAzure AD
6. Tooon helyszíni címtárakat, például az Active Directory exportálása

Ez a sorrendje hello összekötők hello beépített ütemezési működésével.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
Hello szinkronizálási motor toosee foglalt vagy üresjárati esetén is figyelheti. Ez a parancsmag üres értéket ad vissza, ha hello szinkronizálási motor üresjáratban, és nem fut egy összekötőt. Ha egy összekötő fut, hello összekötő hello nevét adja vissza.

```
Get-ADSyncConnectorRunStatus
```

![Összekötő állapota futtatása](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
A fenti hello kép hello első sor olyan állapotban, amelyben hello szinkronizálási motor üresjárati származik. hello hello Azure AD-összekötő fut. Ha a második sorban.

## <a name="scheduler-and-installation-wizard"></a>Ütemező és a telepítési varázsló
Ha hello telepítővarázsló elindításához hello Feladatütemező ideiglenesen fel van függesztve. Ez a viselkedés oka, hogy a rendszer feltételezi, hogy a konfigurációs módosításokat, és ezeket a beállításokat nem lehet alkalmazni, ha a szinkronizálási motor hello aktívan fut. Ezért ne hagyja hello telepítővarázsló nyissa meg leállítja hello szinkronizálási motor az egyetlen szinkronizálási műveletek végrehajtása óta.

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.

További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
