---
title: "Az Azure AD Connect: Automatikus frissítés |} Microsoft Docs"
description: "Ez a témakör ismerteti a hello beépített automatikus frissítési szolgáltatás az Azure AD Connect szinkronizálási szolgáltatás."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b395e8f-fa3c-4e55-be54-392dd303c472
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 70d15eb3adf7758d8a43d278157daa504e059a98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: automatikus frissítés
Ez a funkció a build 1.1.105.0 (dátuma: 2016. februári) ban jelent meg.

## <a name="overview"></a>Áttekintés
Annak biztosítása, az Azure AD Connect telepítése mindig működik toodate soha nem volt megkönnyíti a hello **automatikus frissítés** szolgáltatás. Ezt a szolgáltatást alapértelmezés szerint az Expressz telepítés és a DirSync frissítés. Amikor megjelenik egy új verziója, a rendszer automatikusan frissíti a telepítést.

Automatikus frissítés a következő hello alapértelmezés szerint engedélyezve van:

* A telepítést és a DirSync frissítéseinek Express.
* SQL Express LocalDB használata, amely a mi expressz beállításokat mindig használjon. DirSync és az SQL Express LocalDB is használhatja.
* hello AD-fiókot az hello alapértelmezett MSOL_ fiók expressz beállításokat és a DirSync által létrehozott.
* A hello metaverse kisebb, mint 100 000 objektummal rendelkezik.

automatikus frissítés aktuális állapotának hello hello PowerShell-parancsmaggal lehet megtekinteni `Get-ADSyncAutoUpgrade`. A következő állapotok hello rendelkezik:

| Állapot | Megjegyzés |
| --- | --- |
| Engedélyezve |Automatikus frissítés engedélyezve van. |
| Felfüggesztve |Csak a rendszer hello állítja be. hello rendszer már nem jogosult tooreceive automatikus frissítéseket. |
| Letiltva |Automatikus frissítés le van tiltva. |

Között módosíthatja **engedélyezve** és **letiltott** rendelkező `Set-ADSyncAutoUpgrade`. Csak hello rendszer-et kell beállítania hello állapot **felfüggesztett**.

Automatikus frissítés az Azure AD Connect Health hello frissítési infrastruktúra használja. Az automatikus frissítési toowork, ellenőrizze, hogy a proxykiszolgáló hello URL-címek indította el **az Azure AD Connect Health** dokumentált módon [Office 365 URL-címei és IP-címtartományok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Ha hello **Synchronization Service Managert** felhasználói felület hello kiszolgálón fut, majd hello frissítés erre az időre szünetel hello felhasználói felület le van zárva.

## <a name="troubleshooting"></a>Hibaelhárítás
A Connect telepítés nem frissülnek magát az elvárásoknak megfelelően, kövesse a lépéseket toofind ki, hogy mi lehet a hiba.

Először nem fognak hello próbált automatikus frissítés toobe hello első napja megjelent egy új verziója. Nincs olyan szándékos annak véletlenszerű előtt frissítés kísérlet történik, így nem kell alarmed, ha a telepítés azonnal nincs frissítve.

Ha úgy gondolja, hogy valami nem megfelelő, először futtassa `Get-ADSyncAutoUpgrade` tooensure automatikus frissítés engedélyezve van.

Ezt követően győződjön meg arról, hello szükséges URL-címek nyitotta meg a proxy vagy tűzfal. Automatikus frissítés használ az Azure AD Connect Health hello leírtak [áttekintése](#overview). Ha proxyt használ, győződjön meg arról, egészségügyi konfigurált toouse lett egy [proxykiszolgáló](../connect-health/active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Hello is vizsgálati [állapotfigyelő kapcsolat](../connect-health/active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) tooAzure AD.

Hello kapcsolat tooAzure AD ellenőrizte idő toolook hello Eventlog be. Hello az Eseménynapló elindítása és a hely hello **alkalmazás** az eseménynaplóban talál. Az eseménynaplóban szűrőket hello forrás hozzá **az Azure AD Connect frissítése** és hello esemény Adatazonosító-tartomány **300-399**.  
![Automatikus frissítés szűrőt az eseménynaplóban talál](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Automatikus frissítés állapota hello társított hello Eventlog most már megtekintheti.  
![Automatikus frissítés szűrőt az eseménynaplóban talál](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

hello eredménykódja hello állapot áttekintést előtag van.

| Eredmény kódja előtagja | Leírás |
| --- | --- |
| Sikeres |hello telepítési sikeresen frissítve. |
| UpgradeAborted |Egy ideiglenes állapot hello frissítés leállt. Újból megpróbálkozik és hello általános gyakorlat, hogy később sikeres-e. |
| UpgradeNotSupported |hello a rendszer olyan konfigurációt, amely blokkolja az automatikusan frissítendő hello rendszer van. Rendszer toosee akkor használható, ha hello állapotba vált, de hello általános gyakorlat, hogy a rendszer hello manuálisan kell frissíteni. |

Ez egy lista hello leggyakoribb üzenetek találja. Nem található az összes, de az eredmény üdvözlőüzenetére törölje a következő milyen hello probléma van.

| Eredmény üzenet | Leírás |
| --- | --- |
| **UpgradeAborted** | |
| UpgradeAbortedCouldNotSetUpgradeMarker |Toohello beállításkulcs írása sikertelen volt. |
| UpgradeAbortedInsufficientDatabasePermissions |hello beépített Rendszergazdák csoport nem rendelkezik engedélyekkel toohello adatbázis. Frissítsen kézzel az Azure AD Connect tooaddress toohello legújabb verzióját a probléma. |
| UpgradeAbortedInsufficientDiskSpace |Nincs elegendő lemez terület toosupport frissítés. |
| UpgradeAbortedSecurityGroupsNotPresent |Nem található és nem oldja meg a szinkronizálási motor hello által használt összes biztonsági csoport. |
| UpgradeAbortedServiceCanNotBeStarted |NT-szolgáltatás hello **Microsoft Azure AD Sync** toostart sikertelen. |
| UpgradeAbortedServiceCanNotBeStopped |NT-szolgáltatás hello **Microsoft Azure AD Sync** toostop sikertelen. |
| UpgradeAbortedServiceIsNotRunning |NT-szolgáltatás hello **Microsoft Azure AD Sync** nem működik. |
| UpgradeAbortedSyncCycleDisabled |hello SyncCycle beállítás hello [Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md) le van tiltva. |
| UpgradeAbortedSyncExeInUse |Hello [synchronization service Managert felhasználói felület](active-directory-aadconnectsync-service-manager-ui.md) hello nyitva. |
| UpgradeAbortedSyncOrConfigurationInProgress |hello telepítővarázsló fut, vagy szinkronizálási hello Feladatütemező kívül lett ütemezve. |
| **UpgradeNotSupported** | |
| UpgradeNotSupportedCustomizedSyncRules |A saját egyéni szabályok toohello konfigurációs hozzáadását. |
| UpgradeNotSupportedDeviceWritebackEnabled |Engedélyezte a hello [eszközvisszaíró](active-directory-aadconnect-feature-device-writeback.md) szolgáltatás. |
| UpgradeNotSupportedGroupWritebackEnabled |Engedélyezte a hello [csoportvisszaírásról](active-directory-aadconnect-feature-preview.md#group-writeback) szolgáltatás. |
| UpgradeNotSupportedInvalidPersistedState |hello telepítési nincs egy expressz beállításokat, vagy a DirSync frissítését. |
| UpgradeNotSupportedMetaverseSizeExceeeded |A hello metaverse több mint 100 000 objektummal rendelkezik. |
| UpgradeNotSupportedMultiForestSetup |Több erdő mint toomore csatlakozik. A gyorstelepítés csak tooone erdő csatlakozik. |
| UpgradeNotSupportedNonLocalDbInstall |Nem használ egy SQL Server Express LocalDB adatbázisban. |
| UpgradeNotSupportedNonMsolAccount |Hello [AD-összekötő fiókhoz](active-directory-aadconnect-accounts-permissions.md#active-directory-account) már nem hello alapértelmezett MSOL_ fiók. |
| UpgradeNotSupportedStagingModeEnabled |hello kiszolgáló toobe beállítása [átmeneti módban](active-directory-aadconnectsync-operations.md#staging-mode). |
| UpgradeNotSupportedUserWritebackEnabled |Engedélyezte a hello [felhasználó-visszaírás](active-directory-aadconnect-feature-preview.md#user-writeback) szolgáltatás. |

## <a name="next-steps"></a>Következő lépések
További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
