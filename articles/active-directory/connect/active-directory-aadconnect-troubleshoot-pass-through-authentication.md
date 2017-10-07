---
title: "Az Azure AD Connect: Áteresztő hitelesítés hibaelhárítása |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tootroubleshoot Azure Active Directory (Azure AD) áteresztő hitelesítés."
services: active-directory
keywords: "Az Azure AD Connect áteresztő hitelesítés elhárításával kapcsolatos tudnivalókat Active Directory, az Azure AD, SSO, szükséges összetevők telepítése egyszeri bejelentkezést."
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 87130952f660762f91b0a34b05287603b565639f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-active-directory-pass-through-authentication"></a>Az Azure Active Directory áteresztő hitelesítés hibáinak elhárítása

Ez a cikk segít hibaelhárítási Azure AD áteresztő hitelesítés kapcsolatos gyakori problémákat kapcsolatos információkat.

>[!IMPORTANT]
>Felhasználói bejelentkezési problémák átmenő hitelesítéssel küzdenek, ha az nem hello szolgáltatás letiltása, vagy távolítsa el az áteresztő hitelesítés ügynökök anélkül, hogy egy kizárólag felhőalapú globális rendszergazdai fiók toofall újra be. További tudnivalók [csak felhőalapú globális rendszergazdai fiókot hozzá](../active-directory-users-create-azure-portal.md). Ez a lépés elvégzése alapvető fontosságú, és biztosítja, hogy Ön nem ártatlan tévedéssel a bérlő.

## <a name="general-issues"></a>Általános problémák

### <a name="check-status-of-hello-feature-and-authentication-agents"></a>Hello szolgáltatás és hitelesítési ügynökök állapotának ellenőrzése

Győződjön meg arról, adott hello áteresztő hitelesítés szolgáltatás még mindig **engedélyezve** a bérlői és hello hitelesítési ügynökök állapotát jeleníti meg **aktív**, és nem **inaktív**. Állapot toohello címen ellenőrizheti **az Azure AD Connect** hello paneljét [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com/).

![Az Azure Active Directory felügyeleti központ bemutatása – az Azure AD Connect panel](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Az Azure Active Directory felügyeleti központ – áteresztő hitelesítés panel](./media/active-directory-aadconnect-pass-through-authentication/pta11.png)

### <a name="user-facing-sign-in-error-messages"></a>Felhasználók számára is elérhető bejelentkezési hibaüzenetek

Ha hello felhasználó nem toosign az átmenő hitelesítést használ, a következő felhasználók számára is elérhető hibák hello Azure AD bejelentkezési képernyőn hello egyik jelenhetnek meg: 

|Hiba|Leírás|Megoldás:
| --- | --- | ---
|AADSTS80001|Nem lehet tooconnect tooActive könyvtár|Győződjön meg arról, hogy ügynök kiszolgálók hello ugyanazt az AD-erdő jelszóval kell érvényesíteni toobe hello felhasználóként és képes tooconnect tooActive könyvtárban.  
|AADSTS8002|Időtúllépés történt a kapcsolódó tooActive könyvtár|Ellenőrizze, hogy az Active Directory érhető el, és az ügynökök hello toorequests válaszol tooensure.
|AADSTS80004|az átadott toohello ügynök hello felhasználónév érvénytelen.|Győződjön meg arról, hello felhasználó megpróbál toosign hello jobb oldali felhasználónév be.
|AADSTS80005|Érvényesítési előre nem látható WebException észlelt|Egy átmeneti hiba. Ismételje meg a hello kérelmet. Ha toofail folytatja, a Microsoft támogatási szolgálatához.
|AADSTS80007|Hiba történt az Active Directory kommunikál|Hello ügynök naplókban találhatók további információk, és győződjön meg arról, hogy a várt módon működik-e az Active Directory.

### <a name="sign-in-failure-reasons-on-hello-azure-active-directory-admin-center"></a>Bejelentkezési hiba okai a hello Azure Active Directory felügyeleti központ

Indítsa el a felhasználói bejelentkezési problémák elhárítása hello megtekintésével [bejelentkezési tevékenységei jelentése](../active-directory-reporting-activity-sign-ins.md) a hello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com/).

![Az Azure Active Directory felügyeleti központ – bejelentkezések jelentés](./media/active-directory-aadconnect-pass-through-authentication/pta4.png)

Keresse meg a túl**Azure Active Directory** -> **bejelentkezések** a hello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com/) kattintson egy adott felhasználó bejelentkezési tevékenységet. Keresse meg hello **SIGN-IN hibakód** mező. Térkép hello adott mező tooa a hiba oka és értéke alapján a következő táblázat hello:

|Bejelentkezési hiba kódja|Bejelentkezési hiba okát is megadva|Megoldás:
| --- | --- | ---
| 50144 | A felhasználó Active Directory jelszava lejárt. | Hello felhasználó jelszavát a helyszíni Active Directoryban.
| 80001 | Nem érhető el hitelesítési ügynök. | Telepíti és regisztrálja egy hitelesítési ügynök.
| 80002 | A hitelesítési ügynök jelszó-érvényesítési kérése túllépte az időkorlátot. | Ellenőrizze, hogy az Active Directory hitelesítési ügynök hello elérhető-e.
| 80003 | A hitelesítési ügynök érvénytelen választ kapott. | Ha több felhasználó között hello probléma következetesen reprodukálható, ellenőrizze az Active Directory konfigurációját.
| 80004 | A bejelentkezési kérésben helytelen egyszerű felhasználónevet (UPN-t) használtak. | Kérje meg a hello felhasználói toosign hello helyes felhasználónév be.
| 80005 | Hitelesítési ügynök: hiba történt. | Átmeneti hiba. Próbálkozzon újra később.
| 80007 | Hitelesítési ügynök nem tooconnect tooActive könyvtár. | Ellenőrizze, hogy az Active Directory hitelesítési ügynök hello elérhető-e.
| 80010 | Hitelesítési ügynök nem toodecrypt jelszót. | Ha hello probléma következetesen reprodukálható, telepítése, és regisztrálni egy új hitelesítési ügynök. És az aktuális hello. 
| 80011 | Hitelesítési ügynök nem tooretrieve visszafejtési kulcsot. | Ha hello probléma következetesen reprodukálható, telepítése, és regisztrálni egy új hitelesítési ügynök. És az aktuális hello.

## <a name="authentication-agent-installation-issues"></a>Hitelesítési ügynök telepítésével kapcsolatos problémák

### <a name="an-unexpected-error-occurred"></a>Váratlan hiba történt

[Ügynök naplógyűjtéshez](#collecting-pass-through-authentication-agent-logs) hello kiszolgáló és a probléma megoldásához forduljon a Microsoft Support.

## <a name="authentication-agent-registration-issues"></a>Hitelesítési ügynök regisztrációs problémák

### <a name="registration-of-hello-authentication-agent-failed-due-tooblocked-ports"></a>Tooblocked portok miatt nem sikerült hello hitelesítési ügynök regisztrálása

Győződjön meg arról, hogy hello server mely hello hitelesítési ügynök telepítve van az URL-címek és portok felsorolt szolgáltatás is kommunikálni [Itt](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites).

### <a name="registration-of-hello-authentication-agent-failed-due-tootoken-or-account-authorization-errors"></a>Tootoken vagy fiók hitelesítési hiba miatt nem sikerült hello hitelesítési ügynök regisztrálása

Győződjön meg arról, hogy egy csak felhőalapú globális rendszergazdai fiókot használja-e az összes Azure AD Connect vagy önálló hitelesítési ügynök telepítése és regisztrálása az operations. Egy ismert probléma az MFA-kompatibilis globális rendszergazdai fiókok; Kapcsolja ki a multi-factor Authentication ideiglenesen (csak toocomplete hello műveletek) megoldás.

### <a name="an-unexpected-error-occurred"></a>Váratlan hiba történt

[Ügynök naplógyűjtéshez](#collecting-pass-through-authentication-agent-logs) hello kiszolgáló és a probléma megoldásához forduljon a Microsoft Support.

## <a name="authentication-agent-uninstallation-issues"></a>Hitelesítési ügynök eltávolítása problémák

### <a name="warning-message-when-uninstalling-azure-ad-connect"></a>Figyelmeztetés jelenik meg, amikor az Azure AD Connect eltávolítása

Ha engedélyezve van a bérlő az áteresztő hitelesítés, és az Azure AD Connect toouninstall, azt mutatja meg a következő figyelmeztető üzenet hello: "felhasználók nem fognak tudni toosign a tooAzure AD használni, ha nincs más áteresztő hitelesítés ügynökök más kiszolgálókra telepített."

Győződjön meg arról, hogy a telepítő [magas rendelkezésre álló](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) megtörje felhasználói bejelentkezés az Azure AD Connect tooavoid eltávolítása előtt.

## <a name="issues-with-enabling-hello-feature"></a>Hello funkció engedélyezése problémákat

### <a name="enabling-hello-feature-failed-because-there-were-no-authentication-agents-available"></a>Hello szolgáltatás engedélyezése nem sikerült, mert nem volt elérhető az egyetlen hitelesítési ügynökök

Toohave kell legalább egy aktív hitelesítési ügynök tooenable áteresztő hitelesítés a tenant. Egy hitelesítési ügynök telepítése az Azure AD Connect, vagy egy önálló hitelesítési ügynök telepítése.

### <a name="enabling-hello-feature-failed-due-tooblocked-ports"></a>Nem sikerült engedélyezni a hello szolgáltatás miatt tooblocked portok

Győződjön meg arról, hogy hello kiszolgáló, amelyen telepítve van az Azure AD Connect kommunikálhat a szolgáltatás URL-címek és portok felsorolt [Itt](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites).

### <a name="enabling-hello-feature-failed-due-tootoken-or-account-authorization-errors"></a>Engedélyezze a hello szolgáltatást tootoken vagy fiók hitelesítési hiba miatt sikertelen volt

Győződjön meg arról, hogy egy kizárólag felhőalapú globális rendszergazdai fiók hello szolgáltatás engedélyezésekor. Egy ismert probléma, és a többtényezős hitelesítés (MFA)-engedélyezve van a globális rendszergazdai fiókok; Kapcsolja ki a multi-factor Authentication ideiglenesen (csak a toocomplete hello művelet) megoldás.

## <a name="exchange-activesync-configuration-issues"></a>Exchange ActiveSync konfigurációs problémák

Ezek a hello gyakori problémákat, amikor konfigurálja az Exchange ActiveSync áteresztő hitelesítés támogatása.

### <a name="exchange-powershell-issue"></a>Exchange PowerShell-problémát

Ha megjelenik a hello "**paraméter nem található egyező"PerTenantSwitchToESTSEnabled"paraméternév\.**" Hiba hello futtatásakor `Set-OrganizationConfig` Exchange PowerShell paranccsal, forduljon a Microsoft Support.

### <a name="exchange-activesync-not-working"></a>Exchange ActiveSync nem működik

hello konfiguráció lép érvénybe bizonyos idő tootake – hello a időszakot adott környezettől függ. Ha hosszú ideig hello helyzet továbbra is fennáll, forduljon a Microsoft Support.

## <a name="collecting-pass-through-authentication-agent-logs"></a>Naplózza az áteresztő hitelesítés gyűjtését ügynök

Előfordulhat, hogy probléma hello típusától függően toolook különböző helyeken az áteresztő hitelesítés ügynök naplók kell.

### <a name="authentication-agent-event-logs"></a>Hitelesítési ügynök eseménynaplók

Hibák kapcsolódó toohello hitelesítési ügynök, nyissa meg a hello Eseménynapló alkalmazás hello kiszolgálón, és ellenőrizze a **alkalmazás és szolgáltatás Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**.

Részletes elemzéséhez hello "Munkamenet" napló engedélyezése. Ne futtassa a hello hitelesítési ügynök a napló engedélyezve; a normál műveletek során a csak a hibaelhárításhoz használja. hello napló tartalmát csak láthatók, miután hello napló újra le van tiltva.

### <a name="detailed-trace-logs"></a>Részletes nyomkövetési naplók

tootroubleshoot felhasználói sikertelen bejelentkezés, keresse meg a következő nyomkövetési naplók **%programdata%\Microsoft\Azure AD Connect hitelesítési Agent\Trace\\**. Ezek a naplók egy adott felhasználói bejelentkezés sikertelenségének hello áteresztő hitelesítés funkciójával okok közé tartoznak. Ezek a hibák oka is csatlakoztatott toohello bejelentkezési hiba látható a fenti hello [tábla](#sign-in-failure-reasons-on-the-Azure-portal). Az alábbiakban látható egy példa naplóbejegyzés:

```
    AzureADConnectAuthenticationAgentService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

Hello hiba (a fenti példa hello "1328" jelöli) leíró részleteinek kaphat hello parancssort, és a következő parancs futó hello megnyitása (Megjegyzés: "1328" cserélje le a naplók látható hello tényleges hiba száma):

`Net helpmsg 1328`

![Átmenő hitelesítés](./media/active-directory-aadconnect-pass-through-authentication/pta3.png)

### <a name="domain-controller-logs"></a>Tartományvezérlő naplói

Naplózás engedélyezve van, ha további információt a tartományvezérlők biztonsági rögzít hello található. Egy egyszerű módon tooquery bejelentkezési által küldött kérelmek áteresztő hitelesítés ügynökök a következőképpen történik:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft Azure AD Connect Authentication Agent\AzureADConnectAuthenticationAgentService.exe')]]</Select>
    </Query>
    </QueryList>
```

### <a name="performance-monitor-counters"></a>Teljesítményszámlálók

Egy másik módja toomonitor hitelesítési ügynökök tootrack specifikus teljesítményszámlálók minden kiszolgálón, amelyen telepítve van-e az hello hitelesítési ügynök. A következő globális számlálók használata hello (**# ESP hitelesítések**, **#PTA sikertelen hitelesítés** és **#PTA sikeres hitelesítések**) és hiba számlálók (**# ESP hitelesítési hibák**):

![Áteresztő hitelesítés Teljesítményfigyelő-számlálókból](./media/active-directory-aadconnect-pass-through-authentication/pta12.png)

>[!IMPORTANT]
>Áteresztő hitelesítés több hitelesítési ügynököt használ, magas rendelkezésre állást biztosít, és _nem_ terheléselosztás. A konfigurációtól függően _nem_ a hitelesítési ügynökök körülbelül kap _egyenlő_ kérelmek száma. Akkor lehet, hogy egy adott hitelesítési ügynök minden megkapja-e sincs forgalom.
