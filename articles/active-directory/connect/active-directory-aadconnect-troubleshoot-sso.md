---
title: "Az Azure AD Connect: Zökkenőmentes egyszeri bejelentkezés hibaelhárítása |} Microsoft Docs"
description: "Ez a témakör ismerteti az Azure Active Directory zökkenőmentes egyszeri bejelentkezést (az Azure AD zökkenőmentes SSO) hibaelhárítása."
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
ms.openlocfilehash: bc4ff9125553c8918df3a1f84041560a5b7d4cd8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Hibaelhárítás az Azure Active Directory zökkenőmentes egyszeri bejelentkezést.

Ez a cikk segít hibaelhárítási kapcsolatban az Azure AD zökkenőmentes egyszeri bejelentkezést közös hibákkal kapcsolatos információkat.

## <a name="known-issues"></a>Ismert problémák

- Ha szinkronizál a 30 vagy több AD-erdőkkel, zökkenőmentes SSO-t az Azure AD Connect nem engedélyezhető. Megoldás is [manuális módszerrel engedélyezze az](#manual-reset-of-azure-ad-seamless-sso) a bérlő a funkciót.
- Az Azure AD szolgáltatás URL-címek (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) hozzáadása a "Megbízható helyek" zónához helyett a "Helyi intranet" zóna **megakadályozza a felhasználókat ebben**.
- Zökkenőmentes SSO Firefox és a peremhálózati titkos tallózási módban nem működik. És is be az Internet Explorer fokozott védelem módja bekapcsolása.

>[!IMPORTANT]
>A Microsoft nemrég visszaállítása vizsgálja meg az ügyfél által jelentett problémák peremhálózati támogatása.

## <a name="check-status-of-the-feature"></a>A szolgáltatás állapotának ellenőrzése

Győződjön meg arról, hogy a zökkenőmentes SSO-funkció még **engedélyezve** a tenant. Állapot megnyitásával ellenőrizheti a **az Azure AD Connect** paneljét a [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com/).

![Az Azure Active Directory felügyeleti központ bemutatása – az Azure AD Connect panel](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center"></a>Bejelentkezési hiba okai a Azure Active Directory felügyeleti központ

Egy remek kezdőpont a felhasználói bejelentkezési kapcsolatos hibák elhárításának zökkenőmentes SSO-hoz tekintse meg a [bejelentkezési tevékenységei jelentése](../active-directory-reporting-activity-sign-ins.md) a a [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com/).

![Az Azure Active Directory felügyeleti központ – bejelentkezések jelentés](./media/active-directory-aadconnect-sso/sso9.png)

Navigáljon a **Azure Active Directory** -> **bejelentkezések** a a [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com/) kattintson egy adott felhasználó bejelentkezési tevékenységet. Keresse meg a **SIGN-IN hibakód** mező. A mező értékének leképezése egy hiba okát is megadva és az alábbi táblázat alapján:

|Bejelentkezési hiba kódja|Bejelentkezési hiba okát is megadva|Megoldás:
| --- | --- | ---
| 81001 | A felhasználó Kerberos-jegye túl nagy. | Csökkentse a felhasználói csoport tagságát, és próbálkozzon újra.
| 81002 | Nem érvényesíthető a felhasználó Kerberos-jegye. | Lásd: [ellenőrzőlista hibaelhárítási](#troubleshooting-checklist).
| 81003 | Nem érvényesíthető a felhasználó Kerberos-jegye. | Lásd: [ellenőrzőlista hibaelhárítási](#troubleshooting-checklist).
| 81004 | A Kerberos-hitelesítési kísérlet meghiúsult. | Lásd: [ellenőrzőlista hibaelhárítási](#troubleshooting-checklist).
| 81008 | Nem érvényesíthető a felhasználó Kerberos-jegye. | Lásd: [ellenőrzőlista hibaelhárítási](#troubleshooting-checklist).
| 81009 | "Nem érvényesíthető a felhasználó Kerberos-jegy. | Lásd: [ellenőrzőlista hibaelhárítási](#troubleshooting-checklist).
| 81010 | A zavartalan egyszeri bejelentkezés meghiúsult, mert a felhasználó Kerberos-jegye lejárt vagy érvénytelen. | Felhasználó be kell jelentkeznie a vállalati hálózaton belüli tartományhoz csatlakoztatott eszközről.
| 81011 | Nem található a felhasználói objektum a felhasználó Kerberos-jegyének információi alapján. | Az Azure AD Connect használatával az Azure AD-be a felhasználói adatok szinkronizálása.
| 81012 | Az Azure AD-ba bejelentkezni próbáló felhasználó különbözik az eszközbe jelentkezett felhasználótól. | Jelentkezzen be egy másik eszközről.
| 81013 | Nem található a felhasználói objektum a felhasználó Kerberos-jegyének információi alapján. |Az Azure AD Connect használatával az Azure AD-be a felhasználói adatok szinkronizálása. 

## <a name="troubleshooting-checklist"></a>Hibaelhárítási ellenőrzőlista

A következő ellenőrzőlista használható zökkenőmentes SSO-problémák hibaelhárításához:

- Ellenőrizze, hogy a zökkenőmentes SSO-szolgáltatás engedélyezve van-e az Azure AD Connect. Ha nem engedélyezi a szolgáltatást (például mert a blokkolt port), győződjön meg arról, hogy minden a [szükséges előfeltételek](active-directory-aadconnect-sso-quick-start.md#step-1-check-prerequisites) helyen.
- Ellenőrizze, hogy mind az Azure AD URL-címek (https://autologon.microsoftazuread-sso.com és https://aadg.windows.net.nsatc.net) állapotúak-e a felhasználó Intranet zóna beállítások részeként.
- Győződjön meg arról, a vállalati eszközök az AD-tartományhoz csatlakozik.
- Győződjön meg arról, a felhasználó az eszközre, az AD tartományi fiókkal jelentkezik be.
- Győződjön meg arról, hogy a felhasználói fiók egy AD-erdőhöz, ahol zökkenőmentes SSO megtörtént a be van állítva.
- Gondoskodjon arról, hogy az eszköz csatlakozik a vállalati hálózaton.
- Győződjön meg arról, hogy az eszköz az Active Directory és a tartományvezérlők ideje szinkronizálva van-e, és öt percen belül van.
- Meglévő Kerberos-jegyet az eszköz használatával listáról a **klist** parancsot a parancssorból. Ellenőrizze, hogy ha a jegyektől ki a `AZUREADSSOACCT` számítógépfiók találhatók. A felhasználói Kerberos-jegyek érvényesek általában 12 óra. Előfordulhat, hogy különböző beállítások az Active Directoryban.
- A eszközön a meglévő Kerberos-jegyek kiüríteni a **klist kiürítése** parancsot, majd próbálkozzon újra.
- Annak meghatározásához, hogy a JavaScript-kapcsolatos problémák vannak, tekintse át a konzol naplói (a "fejlesztői eszközök") a böngésző.
- Tekintse át a [tartományvezérlő naplózza](#domain-controller-logs) is.

### <a name="domain-controller-logs"></a>Tartományvezérlő naplói

Ha sikeres naplózás engedélyezve van a tartományvezérlőn, majd minden alkalommal, amikor egy felhasználó zökkenőmentes SSO használatával jelentkezik be egy biztonsági bejegyzést az Eseménynaplóban van rögzítve. A biztonsági események, a következő lekérdezéssel található (eseményt **4769** számítógép fiókhoz tartozó **AzureADSSOAcc$**):

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>A szolgáltatás kézi alaphelyzetbe állítás

Hibaelhárítás nem oldották problémát, a bérlő a szolgáltatás kézi alaphelyzetbe. Kövesse az alábbi lépéseket a helyszíni kiszolgálón az Azure AD Connect futtató:

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>1. lépés: A zökkenőmentes SSO PowerShell modul importálása

1. Első lépésként töltse le és telepítse a [Microsoft Online Services bejelentkezési segéd](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Ezután töltse le és telepítse a [64 bites Azure Active Directory-modul Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Keresse meg a `%programfiles%\Microsoft Azure Active Directory Connect` mappát.
4. Importálja a zökkenőmentes SSO PowerShell modult használja a következő parancsot: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-ad-forests-on-which-seamless-sso-has-been-enabled"></a>2. lépés: Az AD-erdőkkel, amelyen a zökkenőmentes SSO engedélyezve van a listájának lekérdezése

1. PowerShell futtatása rendszergazdaként. A PowerShellben hívás `New-AzureADSSOAuthenticationContext`. Amikor a rendszer kéri, adja meg a bérlő globális rendszergazda hitelesítő adatait.
2. Hívás `Get-AzureADSSOStatus`. Ez a parancs listáját jeleníti meg, akkor az AD-erdőkkel (nézze meg a "Tartományok" lista) a, amelyhez ez a funkció engedélyezve van.

### <a name="step-3-disable-seamless-sso-for-each-ad-forest-that-it-was-set-it-up-on"></a>3. lépés: Letiltja a zökkenőmentes SSO minden úgy lett beállítva, az AD-erdőben

1. Hívás `$creds = Get-Credential`. Amikor a rendszer kéri, adja meg a tartományi rendszergazda hitelesítő adatait a tervezett AD-erdőben.
2. Hívás `Disable-AzureADSSOForest -OnPremCredentials $creds`. Ez a parancs eltávolítja a `AZUREADSSOACCT` az adott AD-erdő a helyi tartományvezérlő számítógépfiókját.
3. Ismételje meg a fenti lépéseket minden beállította a szolgáltatást az AD-erdőben.

### <a name="step-4-enable-seamless-sso-for-each-ad-forest"></a>4. lépés: Zökkenőmentes SSO engedélyezése minden Active Directory-erdőben

1. Hívás `Enable-AzureADSSOForest`. Amikor a rendszer kéri, adja meg a tartományi rendszergazda hitelesítő adatait a tervezett AD-erdőben.
2. Ismételje meg az előző lépést minden AD-erdőben, a szolgáltatás beállítása a kívánt.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>5. lépés A tenant funkció engedélyezése

Hívás `Enable-AzureADSSO` és írja be a "true", a `Enable: ` Rákérdezés bekapcsolja a funkciót, az Ön bérelt szolgáltatásának.
