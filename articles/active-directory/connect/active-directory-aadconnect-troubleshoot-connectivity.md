---
title: "Az Azure AD Connect: Csatlakozási problémák elhárítása |} Microsoft Docs"
description: "Ismerteti, hogyan tootroubleshoot kapcsolat az Azure AD Connect érintő problémákat."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 3aa41bb5-6fcb-49da-9747-e7a3bd780e64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 60d6b7c4ad8a3ab907c20e598ec9443f115df287
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Az Azure AD Connect csatlakozási problémák
Ez a cikk azt ismerteti, hogyan működik, az Azure AD Connect és az Azure AD közötti kapcsolatot, és hogyan tootroubleshoot kapcsolódási problémák. Ezek a problémák valószínűleg toobe proxykiszolgálóval környezetben látható.

## <a name="troubleshoot-connectivity-issues-in-hello-installation-wizard"></a>Hello telepítővarázslóban csatlakozási problémák
Az Azure AD Connect (hello ADAL-könyvtár használatával) a Modern hitelesítést használ a hitelesítéshez. hello telepítővarázsló és hello szinkronizálási motor megfelelő kell a machine.config toobe konfigurációja, mivel ez a két .NET-alkalmazások.

Ebben a cikkben megmutatjuk, hogyan Fabrikam csatlakozzon a proxyn keresztül történő tooAzure AD. hello proxykiszolgáló neve fabrikamproxy, és a 8080-as portot használja.

Először igazolnia kell toomake meg arról, hogy [ **machine.config** ](active-directory-aadconnect-prerequisites.md#connectivity) megfelelően van beállítva.  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

> [!NOTE]
> Az egyes nem a Microsofttól blogok ismerteti, hogy módosítani toomiiserver.exe.config helyette. Azonban ez a fájl nem minden frissítés, még akkor is működik, kezdeti telepítése alatt, ha nem működik hello rendszer első frissítés felülírja. Éppen ezért hello ajánljuk, tooupdate machine.config helyette.
>
>

hello proxykiszolgáló szükséges hello megnyitott URL-címeket is rendelkeznie kell. hello hivatalos lista részletes ismertetését lásd: [Office 365 URL-címei és IP-címtartományok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

URL-címek a következő táblázat hello egyáltalán hello abszolút operációs rendszer minimális toobe képes tooconnect tooAzure AD. Ez a lista nem tartalmaz semmilyen választható funkciók, például a jelszóvisszaírás, vagy az Azure AD Connect Health. Már dokumentált ide toohelp hello kezdeti konfiguráció során.

| URL-CÍME | Port | Leírás |
| --- | --- | --- |
| mscrl.microsoft.com |A HTTP/80 |Tanúsítvány-visszavonási listát használt toodownload sorolja fel. |
| \*. verisign.com |A HTTP/80 |Tanúsítvány-visszavonási listát használt toodownload sorolja fel. |
| \*. entrust.com |A HTTP/80 |Használt toodownload tanúsítvány-visszavonási listát a multi-factor Authentication sorolja fel. |
| \*.windows.net |HTTPS/443-AS |Az tooAzure AD használt toosign. |
| Secure.aadcdn.microsoftonline-p.com |HTTPS/443-AS |Használja az MFA szolgáltatásra. |
| \*.microsoftonline.com |HTTPS/443-AS |Tooconfigure használt Azure Active directory és az importálási/exportálási adatait. |

## <a name="errors-in-hello-wizard"></a>Hibák a hello varázsló
hello telepítővarázsló két eltérő biztonsági környezetben használ. Hello oldalon **tooAzure AD Connect**, az aktuálisan bejelentkezett felhasználó hello használ. Hello oldalon **konfigurálása**, toohello jal [hello szinkronizálási motor hello szolgáltatást futtató fiók](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account). Ha probléma van, úgy tűnik valószínűleg már: hello **tooAzure AD Connect** , mivel a hello proxykonfigurációt globális hello varázsló lapján.

a következő problémák hello hello hello telepítővarázslóban előforduló leggyakoribb hibák.

### <a name="hello-installation-wizard-has-not-been-correctly-configured"></a>hello telepítővarázsló nincs megfelelően konfigurálva
Ez a hiba akkor jelenik meg, amikor maga hello varázsló hello proxy nem tudja elérni.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

* Ha ezt a hibaüzenetet látja, ellenőrizze a hello [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) megfelelően van konfigurálva.
* Ha meg, amely megfelelő, kövesse hello [ellenőrizze a proxy kapcsolatát](#verify-proxy-connectivity) toosee Ha hello problémát, valamint a hello varázsló kívül található.

### <a name="a-microsoft-account-is-used"></a>A Microsoft-fiók használata
Ha egy **Microsoft-fiók** helyett egy **iskolai vagy a szervezet** fiók, megjelenik egy általános hiba.  
![A Microsoft Account történik](./media/active-directory-aadconnect-troubleshoot-connectivity/unknownerror.png)

### <a name="hello-mfa-endpoint-cannot-be-reached"></a>hello MFA végpont nem érhető el
Ez a hiba akkor jelenik meg, ha hello végpont **https://secure.aadcdn.microsoftonline-p.com** nem érhető el, és a globális rendszergazda engedélyezve van az MFA.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

* Ha ezt a hibaüzenetet látja, győződjön meg arról, hogy hello végpont **secure.aadcdn.microsoftonline-p.com** toohello proxy bővült.

### <a name="hello-password-cannot-be-verified"></a>hello jelszó nem lehet ellenőrizni.
Ha hello telepítővarázsló tooAzure AD csatlakozás sikeres, de hello jelszó maga nem ellenőrizhető, hogy ezt a hibaüzenetet látja:  
![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

* Hello jelszó ideiglenes jelszót és meg kell változtatni? Az ténylegesen hello a helyes jelszót? Próbálja meg toosign a toohttps://login.microsoftonline.com (azon a számítógépen egy másik hello Azure AD Connect-kiszolgáló), és ellenőrizze használható hello fiók.

### <a name="verify-proxy-connectivity"></a>Proxy-kapcsolat ellenőrzése
tooverify hello Azure AD Connect-kiszolgáló tud-e tényleges hello Proxy és az interneten keresztül, ha használja, néhány PowerShell toosee Ha hello proxy engedélyezi, hogy a webes kérések vagy sem. A PowerShell-parancssorból, futtassa az `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Technikailag hello első tekintendő, amely toohttps://login.microsoftonline.com és az URI-t is működik, de hello más URI gyorsabb toorespond.)

PowerShell a machine.config toocontact hello proxy hello konfigurációt használja. a winhttp/netsh hello-beállítások kell befolyásolja a parancsmagok használatával.

Hello proxy megfelelően van beállítva, akkor kapja meg az egy sikeres állapotnak: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Ha **nem tooconnect toohello távoli kiszolgáló**, majd PowerShell toomake közvetlen hívásakor próbál hello proxy használata nélkül, vagy DNS nincs megfelelően konfigurálva. Győződjön meg arról, hogy hello **machine.config** fájl megfelelően van beállítva.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Hello proxy nem megfelelően van beállítva, ha hibaüzenetet kap: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

| Hiba | Hiba szövege | Megjegyzés |
| --- | --- | --- |
| 403 |Tiltott |hello proxy a hello kért URL-címe nem lett megnyitva. Hello proxykonfigurációt le újra, és győződjön meg arról, hogy hello [URL-címek](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) lett megnyitva. |
| 407 |Proxyhitelesítés szükséges |hello proxykiszolgáló szükséges, a bejelentkezés, és nincs megadva. Ha a proxykiszolgálóhoz hitelesítés szükséges, győződjön meg arról, hogy toohave ezzel a beállítással konfigurált hello Machine.config fájlban. Győződjön meg arról is hello felhasználó hello varázsló futtatását, és hello szolgáltatásfiók tartományi fiókokat használ. |

## <a name="hello-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>hello kommunikációs minta az Azure AD Connect és az Azure AD között
Ha követte a fenti lépéseket, és továbbra sem sikerül kapcsolódni, akkor előfordulhat, hogy ezen a ponton start hálózati naplók megnézi. Ez a szakasz a normál és a sikeres kapcsolat minta van dokumentálása. Azt a közös piros hering figyelmen kívül hagyható hello hálózati naplók olvasásakor listázása is van.

* Nincsenek hívások toohttps://dc.services.visualstudio.com. Már nem szükséges toohave hello proxy hello telepítési toosucceed és hívásokat a megnyitott URL-címet figyelmen kívül hagyható.
* Láthatja, hogy a DNS-feloldás hello tényleges állomások toobe hello DNS nevét terület nsatc.net és egyéb névterek nem microsoftonline.com alapján sorolja fel. Azonban nincsenek a webszolgáltatási kérelmeket hello kiszolgáló tényleges nevét, és nem rendelkezik tooadd ezen URL-címek toohello proxy.
* hello végpontok adminwebservice provisioningapi felderítési végpontok és toofind hello tényleges végpontja toouse használt. Ezeket a végpontokat a régiójától függően eltérőek.

### <a name="reference-proxy-logs"></a>Hivatkozás proxy naplók
Ez egy biztonsági másolat a tényleges proxy napló és a hello telepítési varázsló oldal honnan készült (a duplikált bejegyzéseket toohello azonos végpont el lettek távolítva). Ez a szakasz a saját proxy- és hálózati naplók referenciaként használható. hello tényleges végpontok eltérhet a környezetben (különösen az URL-címeket a *dőlt*).

**TooAzure AD Connect**

| Time | URL-CÍME |
| --- | --- |
| 1/11/2016 8:31 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:31 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:32 |Csatlakozás: / /*bba800-rögzítési*. microsoftonline.com:443 |
| 1/11/2016 8:32 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:33 |Connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:33 |Csatlakozás: / /*bwsc02-továbbítási*. microsoftonline.com:443 |

**Konfigurálás**

| Time | URL-CÍME |
| --- | --- |
| 1/11/2016 8:43 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:43 |Csatlakozás: / /*bba800-rögzítési*. microsoftonline.com:443 |
| 1/11/2016 8:43 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |Csatlakozás: / /*bba900-rögzítési*. microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |Csatlakozás: / /*bba800-rögzítési*. microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://Login.microsoftonline.com:443 |
| 1/11/2016 8:46 |Connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:46 |Csatlakozás: / /*bwsc02-továbbítási*. microsoftonline.com:443 |

**Kezdeti szinkronizálás**

| Time | URL-CÍME |
| --- | --- |
| 1/11/2016 8:48 |Connect://Login.Windows.NET:443 |
| 1/11/2016 8:49 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:49 |Csatlakozás: / /*bba900-rögzítési*. microsoftonline.com:443 |
| 1/11/2016 8:49 |Csatlakozás: / /*bba800-rögzítési*. microsoftonline.com:443 |

## <a name="authentication-errors"></a>Hitelesítési hibák
Ez a fejezet az adal-t (hello hitelesítési tár használják az Azure AD Connect) és PowerShell visszaadható hibák. hello hiba alapján kell megismerheti a a következő lépéseket.

### <a name="invalid-grant"></a>Érvénytelen támogatás
Érvénytelen felhasználónév vagy jelszó. További információkért lásd: [hello jelszó nem lehet ellenőrizni](#the-password-cannot-be-verified).

### <a name="unknown-user-type"></a>Ismeretlen felhasználó típusa
Az Azure AD-címtár nem található vagy nem megoldott. Lehet, hogy próbálja meg a felhasználónevet használva a nem ellenőrzött tartományt toologin?

### <a name="user-realm-discovery-failed"></a>Nem sikerült felhasználói feltárásának
Hálózati vagy a proxy konfigurációs problémák. hello hálózat nem érhető el. Lásd: [csatlakozási problémák hello telepítővarázslóban](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>A felhasználói jelszó lejárt
A hitelesítő adatok lejártak. Módosítsa a jelszavát.

### <a name="authorizationfailure"></a>AuthorizationFailure
Ismeretlen hiba.

### <a name="authentication-cancelled"></a>Hitelesítési megszakítva
hello többtényezős hitelesítés (MFA) kérdésre meg lett szakítva.

### <a name="connecttomsonline"></a>ConnectToMSOnline
A hitelesítés sikerült, de az Azure AD PowerShell rendelkezik egy hitelesítési probléma.

### <a name="azurerolemissing"></a>AzureRoleMissing
Hitelesítés sikeres volt. A globális rendszergazda tartoznak.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Hitelesítés sikeres volt. Privileged identity management engedélyezve van, és jelenleg nincs a globális rendszergazdája. További információkért lásd: [Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md).

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Hitelesítés sikeres volt. Nem tudta beolvasni a vállalat adatait az Azure AD.

### <a name="retrievedomains"></a>RetrieveDomains
Hitelesítés sikeres volt. Nem sikerült lekérdezni az adatokat az Azure AD.

### <a name="unexpected-exception"></a>Váratlan kivétel
Váratlan hiba történt a telepítési varázsló hello formájában jelenik meg. Akkor fordulhat elő, ha toouse egy **Microsoft Account** helyett egy **iskolai vagy a szervezeti fiók**.

## <a name="troubleshooting-steps-for-previous-releases"></a>Hibaelhárítási lépéseket a korábbi kiadásokban.
Buildszám (dátuma: 2016. februári) 1.1.105.0 kezdve kiadásainak hello bejelentkezési segéd lett visszavonva. Ez a szakasz és hello konfiguráció már nem szükséges, de hivatkozásként maradnak.

Hello single-bejelentkezési segéd toowork a winhttp kell konfigurálni. Ez a konfiguráció nem végezhető [ **netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![Netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="hello-sign-in-assistant-has-not-been-correctly-configured"></a>hello bejelentkezési segéd nincs megfelelően konfigurálva
Ez a hiba akkor jelenik meg, amikor hello bejelentkezési segéd nem érhető el a hello proxy vagy hello proxykiszolgáló nem engedélyezi, hogy hello kérelem.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

* Ha ezt a hibaüzenetet látja, nézze meg a hello proxykonfigurációt [netsh](active-directory-aadconnect-prerequisites.md#connectivity) , és ellenőrizze, helyes.
  ![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
* Ha meg, amely megfelelő, kövesse hello [ellenőrizze a proxy kapcsolatát](#verify-proxy-connectivity) toosee Ha hello problémát, valamint a hello varázsló kívül található.

## <a name="next-steps"></a>Következő lépések
További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
