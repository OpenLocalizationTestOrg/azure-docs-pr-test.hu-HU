---
title: "Az Azure AD Connect: Zökkenőmentes egyszeri bejelentkezés hibaelhárítása |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan tootroubleshoot Azure Active Directory zökkenőmentes egyszeri bejelentkezést (az Azure AD zökkenőmentes SSO)."
services: active-directory
keywords: "Mi az Azure AD Connect telepítés Active Directory szükséges összetevőket az Azure AD, SSO, egyszeri bejelentkezést."
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
ms.openlocfilehash: f1c1c11522f22d5bc742c126fff483c5b06e1f06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Hibaelhárítás az Azure Active Directory zökkenőmentes egyszeri bejelentkezést.

Ez a cikk segít hibaelhárítási kapcsolatban az Azure AD zökkenőmentes egyszeri bejelentkezést közös hibákkal kapcsolatos információkat.

## <a name="known-issues"></a>Ismert problémák

- Ha szinkronizál a 30 vagy több AD-erdőkkel, zökkenőmentes SSO-t az Azure AD Connect nem engedélyezhető. Megoldás is [manuális módszerrel engedélyezze az](#manual-reset-of-azure-ad-seamless-sso) a bérlő hello szolgáltatást.
- Hozzáadása az Azure AD szolgáltatás URL-címek (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) toohello "Megbízható helyek" zóna helyett hello "Helyi intranet" zóna **megakadályozza a felhasználókat ebben**.
- Zökkenőmentes SSO Firefox és a peremhálózati titkos tallózási módban nem működik. És is be az Internet Explorer fokozott védelem módja bekapcsolása.

>[!IMPORTANT]
>A Microsoft nemrég visszaállítása peremhálózati tooinvestigate támogatása ügyfél jelentett problémák.

## <a name="check-status-of-hello-feature"></a>Hello szolgáltatás állapotának ellenőrzése

Győződjön meg arról, zökkenőmentes SSO hello szolgáltatás még mindig **engedélyezve** a tenant. Állapot toohello címen ellenőrizheti **az Azure AD Connect** hello paneljét [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com/).

![Az Azure Active Directory felügyeleti központ bemutatása – az Azure AD Connect panel](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="sign-in-failure-reasons-on-hello-azure-active-directory-admin-center"></a>Bejelentkezési hiba okai a hello Azure Active Directory felügyeleti központ

A felhasználói bejelentkezési kapcsolatos hibák elhárításának zökkenőmentes SSO remek toostart: hello toolook [bejelentkezési tevékenységei jelentése](../active-directory-reporting-activity-sign-ins.md) a hello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com/).

![Az Azure Active Directory felügyeleti központ – bejelentkezések jelentés](./media/active-directory-aadconnect-sso/sso9.png)

Keresse meg a túl**Azure Active Directory** -> **bejelentkezések** a hello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com/) kattintson egy adott felhasználó bejelentkezési tevékenységet. Keresse meg hello **SIGN-IN hibakód** mező. Térkép hello adott mező tooa a hiba oka és értéke alapján a következő táblázat hello:

|Bejelentkezési hiba kódja|Bejelentkezési hiba okát is megadva|Megoldás:
| --- | --- | ---
| 81001 | A felhasználó Kerberos-jegye túl nagy. | Csökkentse a felhasználói csoport tagságát, és próbálkozzon újra.
| 81002 | Nem lehet toovalidate felhasználói Kerberos jegy. | Lásd: [ellenőrzőlista hibaelhárítási](#troubleshooting-checklist).
| 81003 | Nem lehet toovalidate felhasználói Kerberos jegy. | Lásd: [ellenőrzőlista hibaelhárítási](#troubleshooting-checklist).
| 81004 | A Kerberos-hitelesítési kísérlet meghiúsult. | Lásd: [ellenőrzőlista hibaelhárítási](#troubleshooting-checklist).
| 81008 | Nem lehet toovalidate felhasználói Kerberos jegy. | Lásd: [ellenőrzőlista hibaelhárítási](#troubleshooting-checklist).
| 81009 | "Nem toovalidate felhasználói Kerberos jegyet. | Lásd: [ellenőrzőlista hibaelhárítási](#troubleshooting-checklist).
| 81010 | Zökkenőmentes egyszeri bejelentkezés sikertelen volt, mert hello felhasználói Kerberos jegy lejárt vagy érvénytelen. | Felhasználói a vállalati hálózaton belüli tartományhoz csatlakoztatott eszközről a toosign kell.
| 81011 | Nem lehet toofind felhasználói objektum hello felhasználói Kerberos jegy szereplő információ alapján. | Használja az Azure AD Connect toosynchronize felhasználói adatokat az Azure AD-be.
| 81012 | a tooAzure AD toosign próbált hello felhasználói eltér hello felhasználói hello eszközre. | Jelentkezzen be egy másik eszközről.
| 81013 | Nem lehet toofind felhasználói objektum hello felhasználói Kerberos jegy szereplő információ alapján. |Használja az Azure AD Connect toosynchronize felhasználói adatokat az Azure AD-be. 

## <a name="troubleshooting-checklist"></a>Hibaelhárítási ellenőrzőlista

A következő ellenőrzőlista tootroubleshoot zökkenőmentes SSO problémák hello használata:

- Ellenőrizze, hogy hello zökkenőmentes SSO szolgáltatás engedélyezve van-e az Azure AD Connect. Ha nem engedélyezi a hello szolgáltatást (például miatt tooa blokkolt port), gondoskodjon arról, hogy minden hello [szükséges előfeltételek](active-directory-aadconnect-sso-quick-start.md#step-1-check-prerequisites) helyen.
- Ellenőrizze, hogy e mindkét az Azure AD URL-címek (https://autologon.microsoftazuread-sso.com és https://aadg.windows.net.nsatc.net) hello felhasználói Intranet zóna beállítások részeként.
- Győződjön meg arról hello vállalati eszköz illesztett toohello AD-tartományhoz.
- Győződjön meg arról, hello van bejelentkezett felhasználó toohello eszköz egy AD-tartományi fiókkal.
- Győződjön meg arról, hogy hello felhasználói fiók egy AD-erdőhöz, ahol zökkenőmentes SSO megtörtént a be van állítva.
- Győződjön meg arról, hello eszköz csatlakozik-e hello a vállalati hálózaton.
- Győződjön meg arról, hogy hello eszköz hello Active Directory és a hello rendszerű tartományvezérlők idő szinkronizálva van-e, és öt percen belül van.
- Meglévő Kerberos-jegyek hello hello-eszközt a listából **klist** parancsot a parancssorból. Ellenőrizze, hogy ha jegyek kiállított hello `AZUREADSSOACCT` számítógépfiók találhatók. A felhasználói Kerberos-jegyek érvényesek általában 12 óra. Előfordulhat, hogy különböző beállítások az Active Directoryban.
- Meglévő Kerberos-jegyek kiürítése hello segítségével hello eszközről **klist kiürítése** parancsot, majd próbálkozzon újra.
- toodetermine JavaScript-kapcsolatos problémák esetén tekintse át a hello konzolnaplófájlokban hello böngésző (a "fejlesztői eszközök").
- Felülvizsgálati hello [tartományvezérlő naplózza](#domain-controller-logs) is.

### <a name="domain-controller-logs"></a>Tartományvezérlő naplói

Ha sikeres naplózás engedélyezve van a tartományvezérlőn, majd minden alkalommal, amikor egy felhasználó zökkenőmentes SSO használatával jelentkezik be egy biztonsági bejegyzés hello esemény naplóban rögzített. A biztonsági események a következő lekérdezés hello segítségével megtalálhatja (eseményt **4769** hello számítógépfiók társított **AzureADSSOAcc$**):

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-hello-feature"></a>A következő hello szolgáltatás kézi alaphelyzetbe állítás

Hibaelhárítás súgója nem, a bérlő hello szolgáltatás manuálisan alaphelyzetbe. Kövesse az alábbi lépéseket a hello helyszíni kiszolgálón az Azure AD Connect futtató:

### <a name="step-1-import-hello-seamless-sso-powershell-module"></a>1. lépés: Hello zökkenőmentes SSO PowerShell modul importálása

1. Első lépésként töltse le, és telepítse a hello [Microsoft Online Services bejelentkezési segéd](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Majd letöltéséhez és telepítéséhez hello [64 bites Azure Active Directory-modul Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Keresse meg a toohello `%programfiles%\Microsoft Azure Active Directory Connect` mappa.
4. Importálás hello zökkenőmentes SSO PowerShell modult használja a következő parancsot: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-hello-list-of-ad-forests-on-which-seamless-sso-has-been-enabled"></a>2. lépés: AD-erdőkkel, amelyen engedélyezve van az zökkenőmentes SSO hello listájának beolvasása

1. PowerShell futtatása rendszergazdaként. A PowerShellben hívás `New-AzureADSSOAuthenticationContext`. Amikor a rendszer kéri, adja meg a bérlő globális rendszergazda hitelesítő adatait.
2. Hívás `Get-AzureADSSOStatus`. Ez a parancs biztosítja, hogy AD-erdőkkel (hello "tartományok" lista pillantást) listája, hello meg, amelyhez ez a funkció engedélyezve van.

### <a name="step-3-disable-seamless-sso-for-each-ad-forest-that-it-was-set-it-up-on"></a>3. lépés: Letiltja a zökkenőmentes SSO minden úgy lett beállítva, az AD-erdőben

1. Hívás `$creds = Get-Credential`. Amikor a rendszer kéri, adjon meg hello tartományi rendszergazdai hitelesítő adatokra hello készült Active Directory-erdőben.
2. Hívás `Disable-AzureADSSOForest -OnPremCredentials $creds`. Ez a parancs eltávolítja hello `AZUREADSSOACCT` hello számítógépfiókkal a helyi tartományvezérlőt az adott AD-erdő.
3. Ismételje meg minden beállítása hello szolgáltatást az AD-erdőben lépései megelőző hello.

### <a name="step-4-enable-seamless-sso-for-each-ad-forest"></a>4. lépés: Zökkenőmentes SSO engedélyezése minden Active Directory-erdőben

1. Hívás `Enable-AzureADSSOForest`. Amikor a rendszer kéri, adjon meg hello tartományi rendszergazdai hitelesítő adatokra hello készült Active Directory-erdőben.
2. Ismételje meg minden AD-erdőben, amelyet fel hello szolgáltatás tooset a lépései megelőző hello.

### <a name="step-5-enable-hello-feature-on-your-tenant"></a>5. lépés A tenant hello szolgáltatás engedélyezése

Hívás `Enable-AzureADSSO` és írja be a "true" hello, `Enable: ` Rákérdezés tooturn hello szolgáltatást az Ön bérelt szolgáltatásának.
