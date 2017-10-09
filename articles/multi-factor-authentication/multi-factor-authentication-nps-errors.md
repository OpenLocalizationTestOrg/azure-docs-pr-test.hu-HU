---
title: "hello Azure MFA NPS bővítmény aaaTroubleshoot hibakódok |} Microsoft Docs"
description: "A hálózati házirend-kiszolgáló bővítmény hello problémák elhárításáról Azure multi-factor Authentication leggyakoribb hibaüzenetek adott feloldását kapcsolatos súgó elérése"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 8b602954364c6e9f801d86edca6432bd8446c58b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-error-messages-from-hello-nps-extension-for-azure-multi-factor-authentication"></a>Hárítsa el a hálózati házirend-kiszolgáló bővítmény hello hibaüzeneteket az Azure multi-factor Authentication

Ha hibákba ütközik az NPS-bővítmény hello Azure multi-factor Authentication, használjon gyorsabban Ez a cikk tooreach felbontást eredményez. 

## <a name="troubleshooting-steps-for-common-errors"></a>Gyakori hibák hibaelhárítási lépéseket

| Hibakód: | Hibaelhárítási lépések |
| ---------- | --------------------- |
| **CONTACT_SUPPORT** | [Forduljon a támogatási szolgálathoz](#contact-microsoft-support), és említse meg a naplók gyűjtésére szolgáló lépéseket hello listája. Minél több információt arról, mi történt előtt hello hiba, beleértve a bérlő azonosítója, és egyszerű felhasználónév (UPN) is biztosít. |
| **CLIENT_CERT_INSTALL_ERROR** | Előfordulhat, hogy hogyan hello ügyféltanúsítvány lett telepítve, vagy a tenanthoz társított kapcsolatos problémát. Hello utasításait követve [hibaelhárítás hello MFA NPS bővítmény](multi-factor-authentication-nps-extension.md#troubleshooting) tooinvestigate ügyfél cert problémák. |
| **ESTS_TOKEN_ERROR** | Hello utasításait követve [hibaelhárítás hello MFA NPS bővítmény](multi-factor-authentication-nps-extension.md#troubleshooting) tooinvestigate ügyféltanúsítványt és ADAL token problémákat. |
| **HTTPS_COMMUNICATION_ERROR** | hello hálózati házirend-kiszolgáló az Azure MFA nem tooreceive válaszát. Ellenőrizze, hogy a tűzfalon a forgalom tooand https://adnotifications.windowsazure.com a nyitott kétirányúan |
| **HTTP_CONNECT_ERROR** | Hello hálózati házirend-kiszolgáló bővítmény futó hello kiszolgálón győződjön meg arról, hogy https://adnotifications.windowsazure.com és https://login.microsoftonline.com/ elérhessék. Ha ezeket a helyeket nem tölthető be, hárítsa el a kapcsolatot az adott kiszolgálón. |
| **REGISTRY_CONFIG_ERROR** | Hello beállításjegyzék hello alkalmazás, amely lehet, hogy hiányzik egy kulcs hello [PowerShell-parancsfájl](multi-factor-authentication-nps-extension.md#install-the-nps-extension) nem futtathatja a telepítés után. hello hibaüzenet hello hiányzó kulcsot kell tartalmaznia. Ellenőrizze, hogy a HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa hello kulcsot. |
| **REQUEST_FORMAT_ERROR** <br> RADIUS-kérelmet RADIUS-userName\Identifier kötelező attribútum hiányzik. Győződjön meg arról, hogy a hálózati házirend-kiszolgáló részesül-e a RADIUS-kérelmek | Ez a hiba általában a telepítési hibát tükrözi. hello hálózati házirend-kiszolgáló bővítmény telepítenie kell a hálózati házirend-kiszolgálókat, amelyek a RADIUS-kérelmek fogadására. NPS-kiszolgálókon telepített szolgáltatásokat, mint a távoli asztali Átjárókiszolgáló és a hozzá tartozó függőségek nem fogadjon RADIUS-kérelmeket. Hálózati házirend-kiszolgáló kiterjesztés csak akkor működik, amikor hello részletek hello hitelesítési kérelem nem olvasása óta. ilyen telepítések és a kimenő hibák telepíti. |
| **REQUEST_MISSING_CODE** | Ellenőrizze, hogy, hogy hello jelszó titkosítási protokollt hello hálózati házirend-kiszolgáló és NAS-kiszolgálók közötti támogatja-e a hello másodlagos hitelesítési módszer használata esetén. **PAP** hello felhőben hello hitelesítési módszerek mindegyikéhez Azure MFA támogatja: telefonhívás, a egyirányú SMS-üzenet, a mobilalkalmazáson keresztüli értesítések és a mobilalkalmazás ellenőrzőkódjának. **CHAPv2** és **EAP** támogatja a telefonhívás, és értesítést a mobilalkalmazásban. |
| **USERNAME_CANONICALIZATION_ERROR** | Győződjön meg arról, hogy hello felhasználói meglétét a helyszíni Active Directory-példányban, és a hálózati házirend-kiszolgáló szolgáltatás hello engedélyek tooaccess hello könyvtár. Erdők közötti bizalmi kapcsolatok, használatakor [forduljon a támogatási szolgálathoz](#contact-microsoft-support) további segítségért. |


   

### <a name="alternate-login-id-errors"></a>Másodlagos bejelentkezési azonosító hibák

| Hibakód: | Hibaüzenet | Hibaelhárítási lépések |
| ---------- | ------------- | --------------------- |
| **ALTERNATE_LOGIN_ID_ERROR** | Hiba: userObjectSid keresés sikertelen volt | Győződjön meg arról, hogy hello a felhasználó létezik a helyszíni Active Directory-példányban. Erdők közötti bizalmi kapcsolatok, használatakor [forduljon a támogatási szolgálathoz](#contact-microsoft-support) további segítségért. |
| **ALTERNATE_LOGIN_ID_ERROR** | Hiba: Nem sikerült másik LoginId keresési | Ellenőrizze, hogy LDAP_ALTERNATE_LOGINID_ATTRIBUTE tooa [érvényes active directory-attribútumot](https://msdn.microsoft.com/library/ms675090(v=vs.85).aspx). <br><br> Ha LDAP_FORCE_GLOBAL_CATALOG tooTrue van beállítva, vagy LDAP_LOOKUP_FORESTS egy nem üres érték van beállítva, ellenőrizze, hogy konfigurálta a globális katalógus attribútum, amely hello AlternateLoginId tooit kerül. <br><br> LDAP_LOOKUP_FORESTS egy nem üres érték van beállítva, győződjön meg arról, hogy helyesek-e a hello érték. Ha egynél több erdő neve, hello neveket pontosvesszővel kell elválasztani, szóközt nem kell elválasztva. <br><br> Ha ezeket a lépéseket nem oldják hello problémát, [forduljon a támogatási szolgálathoz](#contact-microsoft-support) további segítséget itt találhat. |
| **ALTERNATE_LOGIN_ID_ERROR** | Hiba: Alternatív LoginId értéke üres | Ellenőrizze, hogy hello AlternateLoginId attribútum hello felhasználó van beállítva. |


## <a name="errors-your-users-may-encounter"></a>A felhasználók szembesülhetnek hibák

| Hibakód: | Hibaüzenet | Hibaelhárítási lépések |
| ---------- | ------------- | --------------------- |
| **Hozzáférés megtagadva** | Hívó bérlő nem rendelkezik hozzáférési engedélyekkel toodo hitelesítési hello felhasználó | Ellenőrizze, hogy hello bérlő és a hello egyszerű felhasználónév (UPN) hello tartományban vannak hello azonos. Például győződjön meg arról, hogy user@contoso.com tooauthenticate toohello Contoso bérlő próbál. hello UPN jelöli egy érvényes felhasználói hello bérlő az Azure-ban. |
| **AuthenticationMethodNotConfigured** | hello megadott hitelesítési módszert hello felhasználó esetén nem volt beállítva | Hello felhasználó lehet hozzáadni vagy ellenőrizni az ellenőrzési módszereket toohello utasításait szerint [kezelheti a kétlépéses ellenőrzés beállításait](./end-user/multi-factor-authentication-end-user-manage-settings.md). |
| **AuthenticationMethodNotSupported** | Nem támogatott a megadott hitelesítési módszert. | Gyűjteni a naplókat, amely tartalmazza a hiba, és [forduljon a támogatási szolgálathoz](#contact-microsoft-support). Ha támogatási szolgálatához fordul, adja meg a hello felhasználónevét és hello hiba kiváltó hello másodlagos hitelesítési módszert. |
| **BecAccessDenied** | MSODS Bec hívása adott vissza a hozzáférés megtagadva, valószínűleg hello nincs megadva hello-bérlőben | hello felhasználó szerepel a helyszíni Active Directory, de nem van-e szinkronizálva az Azure AD által AD Connect. Vagy hello felhasználói hello bérlő számára. Hello felhasználói tooAzure AD hozzá, azok hozzáadása a hitelesítési módszerek toohello utasításait szerint [kezelheti a kétlépéses ellenőrzés beállításait](./end-user/multi-factor-authentication-end-user-manage-settings.md). |
| **InvalidFormat** vagy **StrongAuthenticationServiceInvalidParameter** | hello telefonszám formátuma ismeretlen van | Javítsa ki az ellenőrzési telefonszámok hello felhasználó rendelkezik. |
| **InvalidSession** | hello megadott munkamenet érvénytelen, vagy esetleg elévült | hello munkamenet több mint három perc toocomplete hajtott végre. Győződjön meg arról, hogy a felhasználó hello hello ellenőrző kód megadása, vagy toohello alkalmazásban megjelenő értesítésre hello hitelesítési kérelem kezdeményezése három percen belül válaszol. Ha ez nem segít hello problémán, ellenőrizze, hogy vannak-e ügyfél, a NAS kiszolgáló, a hálózati házirend-kiszolgáló és a hello Azure MFA végpont között nincs hálózati késések fordulnak elő.  |
| **NoDefaultAuthenticationMethodIsConfigured** | Hello felhasználói a nem alapértelmezett hitelesítési módszer konfigurálása | Hello felhasználó lehet hozzáadni vagy ellenőrizni az ellenőrzési módszereket toohello utasításait szerint [kezelheti a kétlépéses ellenőrzés beállításait](./end-user/multi-factor-authentication-end-user-manage-settings.md). Győződjön meg arról, hogy hello a felhasználó egy alapértelmezett hitelesítési módszer választása, és ez a módszer a fiókjuk konfigurálva. |
| **OathCodePinIncorrect** | Helytelen kódot és a megadott PIN-kód. | Ez a hiba nem várható a hálózati házirend-kiszolgáló bővítmény hello. Ha a felhasználó tapasztal, [forduljon a támogatási szolgálathoz](#contact-microsoft-support) hibaelhárítási segítséget. |
| **ProofDataNotFound** | Ellenőrző adatok nem konfigurálta hello megadott hitelesítési módszert. | Próbálja meg egy másik ellenőrzési módszerrel, vagy adjon hozzá egy új ellenőrzési módszert toohello utasításait szerint hello felhasználó [kezelheti a kétlépéses ellenőrzés beállításait](./end-user/multi-factor-authentication-end-user-manage-settings.md). Ha hello felhasználó továbbra is fennáll, ez a hiba, miután megerősítette, hogy az ellenőrzési módszert megfelelően van-e beállítva toosee [forduljon a támogatási szolgálathoz](#contact-microsoft-support). |
| **SMSAuthFailedWrongCodePinEntered** | Helytelen kódot és a megadott PIN-kód. (OneWaySMS) | Ez a hiba nem várható a hálózati házirend-kiszolgáló bővítmény hello. Ha a felhasználó tapasztal, [forduljon a támogatási szolgálathoz](#contact-microsoft-support) hibaelhárítási segítséget. |
| **TenantIsBlocked** | Bérlői le van tiltva. | [Forduljon a támogatási szolgálathoz](#contact-microsoft-support) Directory azonosítójú hello Azure AD tulajdonságai lapon a hello Azure-portálon. |
| **UserNotFound** | hello megadott felhasználó nem található. | hello bérlő most már az Azure AD aktívnak látható. Ellenőrizze, hogy az előfizetés aktív, hogy hello első féltől származó alkalmazások szükséges. Győződjön meg arról is hello bérlői hello tanúsítvány tulajdonosának a várt módon, és hello cert továbbra is érvényesek és regisztrált egyszerű hello szolgáltatást a. |

## <a name="messages-your-users-may-encounter-that-arent-errors"></a>A felhasználók szembesülhetnek, amely üzenetek nem hibák

Egyes esetekben a felhasználók is jelenik meg a multi-factor Authentication a hitelesítési kérés meghiúsult, mert. Ezek nem hello termék konfigurációjának hibáit, de a rendszer szándékos figyelmeztetések foglalja össze, miért lett megtagadva a hitelesítési kérelmet.

| Hibakód: | Hibaüzenet | Javasolt lépések | 
| ---------- | ------------- | ----------------- |
| **OathCodeIncorrect** | Helytelen kódot entered\OATH kód helytelen | Nem hiba, a felhasználó sikeres megadott kód helytelen. | hello felhasználói megadott hello helytelen kódot. Adjon meg egy új kódot kér, vagy jelentkezzen be újra rendelkezik. | 
| **SMSAuthFailedMaxAllowedCodeRetryReached** | Elérte a maximális megengedett kód újrapróbálkozási | hello felhasználói hello ellenőrző kérdésre túl sokszor sikertelen volt. Most egy rendszergazda által feloldva toobe szükségük a beállításoktól függ.  |
| **SMSAuthFailedWrongCodeEntered** | Hibás a megadott vagy szöveges üzenet OTP helytelen kód | hello felhasználói megadott hello helytelen kódot. Adjon meg egy új kódot kér, vagy jelentkezzen be újra rendelkezik. |

## <a name="errors-that-require-support"></a>A hibákat, támogatásra van szüksége

Ha egy ezeket a hibákat észlel, azt javasoljuk, hogy Ön [forduljon a támogatási szolgálathoz](#contact-microsoft-support) diagnosztikai segítségét. Nincs szabványos lépésekre lehet oldani ezeket a hibákat. Támogatási szolgálatához fordul, lehet, hogy tooinclude mennyi információ lehető tooan hiba vezető hello lépéseket, és a bérlői kapcsolatos információkat.

| Hibakód: | Hibaüzenet |
| ---------- | ------------- |
| **InvalidParameter** | Kérelem nem lehet null. |
| **InvalidParameter** | ObjectId nem lehet null vagy üres a ReplicationScope: {0} |
| **InvalidParameter** | Cégnév hosszát hello \{0} \ hosszabb, mint hello maximális engedélyezett hosszt {1} |
| **InvalidParameter** | UserPrincipalName nem lehet null vagy üres |
| **InvalidParameter** | a megadott hello a TenantId nem a megfelelő formátumban van. |
| **InvalidParameter** | Munkamenet-azonosító nem lehet null vagy üres |
| **InvalidParameter** | Nem sikerült feloldani a ProofData kérelemből vagy az Msods. hello ProofData érvénytelen. |
| **InternalError** |  |
| **OathCodePinIncorrect** |  |
| **VersionNotSupported** |  |
| **MFAPinNotSetup** |  |

## <a name="next-steps"></a>Következő lépések

### <a name="troubleshoot-user-accounts"></a>Felhasználói fiókok hibáinak elhárítása

Ha a felhasználók is [problémák adódtak a kétlépéses ellenőrzéshez használttal](./end-user/multi-factor-authentication-end-user-troubleshoot.md), őket: problémák önálló diagnosztizálása érdekében. 

### <a name="contact-microsoft-support"></a>Forduljon a Microsoft támogatási szolgálatához.

Ha további segítségre van szüksége, forduljon a támogatási szakember keresztül [Azure multi-factor Authentication kiszolgáló támogatási](https://support.microsoft.com/oas/default.aspx?prid=14947). Lépjen kapcsolatba velünk, ha esetén lehet hasznos információt tartalmazhatnak a lehető problémával kapcsolatos. Megadhat olyan adatok tartalmazzák a hello lap, ahol hello hiba, a hiba kódja hello adott munkamenet-azonosító, hello azonosítója hello felhasználó hello látta látott hello hiba, és hibakeresési naplókat.

támogatási diagnosztikai toocollect hibakeresési naplók lépések hello használata: 

1. Nyisson meg egy rendszergazdai parancssort, és futtassa az alábbi parancsokat:

   ```
   Mkdir c:\NPS
   Cd NPS
   netsh trace start Scenario=NetConnection capture=yes tracefile=c:\NPS\nettrace.etl
   logman create trace "NPSExtension" -ow -o c:\NPS\NPSExtension.etl -p {7237ED00-E119-430B-AB0F-C63360C8EE81} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode Circular -f bincirc -max 4096 -ets
   logman update trace "NPSExtension" -p {EC2E6D3A-C958-4C76-8EA4-0262520886FF} 0xffffffffffffffff 0xff -ets
   ```

2. Hello probléma reprodukálásához szükséges

3. Állítsa le a következő parancsokkal hello nyomkövetés:

   ```
   logman stop "NPSExtension" -ets
   netsh trace stop
   wevtutil epl AuthNOptCh C:\NPS\%computername%_AuthNOptCh.evtx
   wevtutil epl AuthZOptCh C:\NPS\%computername%_AuthZOptCh.evtx
   wevtutil epl AuthZAdminCh C:\NPS\%computername%_AuthZAdminCh.evtx
   Start .
   ```

4. A ZIP-hello hello C:\NPS mappa tartalmát, és csatolja hello zip fájlt toohello támogatási esetet.


