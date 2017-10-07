---
title: "Az Azure AD Connect: Zökkenőmentes egyszeri bejelentkezés – gyakori kérdések |} Microsoft Docs"
description: "Válaszok toofrequently kérdések kapcsolatos Azure Active Directory zökkenőmentes egyszeri bejelentkezést."
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
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: e91e7950670633c08c180ece873f7451fa19eef8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Az Azure Active Directory zökkenőmentes egyszeri bejelentkezés: gyakran ismételt kérdések

Ebben a cikkben oldjuk gyakori kérdésekkel kapcsolatos Azure Active Directory zökkenőmentes egyszeri bejelentkezést (zökkenőmentes SSO). Vissza az új tartalom ellenőrizni.

>[!IMPORTANT]
>hello zökkenőmentes SSO funkció jelenleg előzetes verzió.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>Milyen bejelentkezési módszerek zökkenőmentes SSO működnek?

Zökkenőmentes SSO kombinálva vagy hello [Jelszókivonat-szinkronizálást](active-directory-aadconnectsync-implement-password-synchronization.md) vagy [áteresztő hitelesítés](active-directory-aadconnect-pass-through-authentication.md) bejelentkezési módszerek. Azonban ez a szolgáltatás az Active Directory összevonási szolgáltatások (ADFS) nem használható.

## <a name="is-seamless-sso-a-free-feature"></a>Lehetővé teszi az zökkenőmentes SSO szabad?

Zökkenőmentes SSO szabad funkció, minden Azure AD toouse fizetős verziója nem kell azt. A leválasztást szabad hello szolgáltatás általános elérhetőségével elérésekor.

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>Milyen alkalmazások előnyeit `domain_hint` vagy `login_hint` zökkenőmentes SSO-paraméter képességét?

Folyamatban van, hello folyamatának fordítása hello küldeni ezeket a paramétereket és hello azokon, nem alkalmazások listáját. Ha olyan alkalmazásokkal rendelkezik, érdekelt, ossza meg velünk hello Megjegyzések szakaszban.

## <a name="does-seamless-sso-support-alternate-id-as-hello-username-instead-of-userprincipalname"></a>Zökkenőmentes egyszeri Bejelentkezést támogatja `Alternate ID` , a felhasználónév, ahelyett, hogy hello `userPrincipalName`?

Igen. Zökkenőmentes egyszeri Bejelentkezést támogatja `Alternate ID` , amikor konfigurálása az Azure AD Connectben, ahogy felhasználónév hello [Itt](active-directory-aadconnect-get-started-custom.md). Nem minden Office 365-alkalmazások támogatják a `Alternate ID`. Tekintse meg a toohello adott alkalmazás által dokumentációját hello támogatási utasítás.

## <a name="i-want-tooregister-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>Szeretném tooregister nem Windows 10-eszközökre az Azure ad-vel, az AD FS használata nélkül. Használható is zökkenőmentes SSO helyett?

Ebben a forgatókönyvben Igen, van szüksége a verzió a 2.1-es vagy újabb hello [munkahelyhez ügyfél](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="how-can-i-roll-over-hello-kerberos-decryption-key-of-hello-azureadssoacct-computer-account"></a>Hogyan lehet I váltása hello Kerberos visszafejtési kulcsa hello `AZUREADSSOACCT` számítógépfiók?

Fontos toofrequently váltása hello Kerberos visszafejtési kulcsa hello `AZUREADSSOACCT` (amely az Azure AD) létrehozott számítógépfiók a helyszíni Active Directory-erdőben.

>[!IMPORTANT]
>Erősen ajánlott, hogy Ön váltása hello Kerberos visszafejtési kulcs legalább 30 nap.

Kövesse az alábbi lépéseket a hello helyszíni kiszolgálón az Azure AD Connect futtató:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>1. lépés Szerezze be az AD-erdőhöz, ahol engedélyezve van zökkenőmentes SSO listát

1. Első lépésként töltse le, és telepítse a hello [Microsoft Online Services bejelentkezési segéd](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Majd letöltéséhez és telepítéséhez hello [64 bites Azure Active Directory-modul Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Keresse meg a toohello `%programfiles%\Microsoft Azure Active Directory Connect` mappa.
4. Importálás hello zökkenőmentes SSO PowerShell modult használja a következő parancsot: `Import-Module .\AzureADSSO.psd1`.
5. PowerShell futtatása rendszergazdaként. A PowerShellben hívás `New-AzureADSSOAuthenticationContext`. Ez a parancs a helyi menü tooenter biztosítani fogja a bérlő globális rendszergazda hitelesítő adatait.
6. Hívás `Get-AzureADSSOStatus`. Ez a parancs biztosítja, hogy AD-erdőkkel (hello "tartományok" lista pillantást) listája, hello meg, amelyhez ez a funkció engedélyezve van.

### <a name="step-2-update-hello-kerberos-decryption-key-on-each-ad-forest-that-it-was-set-it-up-on"></a>2. lépés Minden AD-erdőben, amely úgy lett beállítva, a hello Kerberos visszafejtési kulcs frissítése

1. Hívás `$creds = Get-Credential`. Amikor a rendszer kéri, adjon meg hello tartományi rendszergazdai hitelesítő adatokra hello készült Active Directory-erdőben.
2. Hívás `Update-AzureADSSOForest -OnPremCredentials $creds`. Ez a parancs frissítések hello Kerberos visszafejtési kulcs hello `AZUREADSSOACCT` számítógép fiókját az adott Active Directory-erdőben, és frissíti az Azure AD-ben.
3. Ismételje meg minden beállítása hello szolgáltatást az AD-erdőben lépései megelőző hello.

>[!IMPORTANT]
>Győződjön meg arról, hogy _nem_ hello futtatása `Update-AzureADSSOForest` parancs egynél többször. Ellenkező esetben a hello szolgáltatás nem működik a felhasználók Kerberos-jegyek lejár, és a helyszíni Active Directory rendszer kiadását hello időpontig.

## <a name="how-can-i-disable-seamless-sso"></a>Hogyan letilthatja zökkenőmentes SSO?

Zökkenőmentes SSO Azure AD Connect használatával lehet letiltani.

Futtassa az Azure AD Connect, válassza a "Módosítás felhasználói bejelentkezési oldalának", és kattintson a "Tovább gombra". Ezután törölje a jelet hello "Engedélyezése egyszeri bejelentkezéshez" lehetőséget. Kövesse hello varázsló lépéseit. Hello varázsló befejezése után zökkenőmentes egyszeri bejelentkezés le van tiltva a bérlő.

Azonban egy üzenet jelenik meg, hogy a következő képernyőn:

"Egyszeri bejelentkezés nem engedélyezett, de nincsenek további manuális lépések tooperform rendelés toocomplete karbantartás. További információ"

toocomplete hello folyamat, kövesse az alábbi manuális lépéseket hello helyszíni kiszolgálón az Azure AD Connect futtató:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>1. lépés Szerezze be az AD-erdőhöz, ahol engedélyezve van zökkenőmentes SSO listát

1. Első lépésként töltse le, és telepítse a hello [Microsoft Online Services bejelentkezési segéd](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Majd letöltéséhez és telepítéséhez hello [64 bites Azure Active Directory-modul Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Keresse meg a toohello `%programfiles%\Microsoft Azure Active Directory Connect` mappa.
4. Importálás hello zökkenőmentes SSO PowerShell modult használja a következő parancsot: `Import-Module .\AzureADSSO.psd1`.
5. PowerShell futtatása rendszergazdaként. A PowerShellben hívás `New-AzureADSSOAuthenticationContext`. Ez a parancs a helyi menü tooenter biztosítani fogja a bérlő globális rendszergazda hitelesítő adatait.
6. Hívás `Get-AzureADSSOStatus`. Ez a parancs biztosítja, hogy AD-erdőkkel (hello "tartományok" lista pillantást) listája, hello meg, amelyhez ez a funkció engedélyezve van.

### <a name="step-2-manually-delete-hello-azureadssoacct-computer-account-from-each-ad-forest-that-you-see-listed"></a>2. lépés Törölje kézzel a hello `AZUREADSSOACCT` számítógépfiókkal minden AD-erdőben, melyek megjelennek a listában.

## <a name="next-steps"></a>Következő lépések

- [**Gyors üzembe helyezési** ](active-directory-aadconnect-sso-quick-start.md) - létrehozásához, és az Azure AD zökkenőmentes SSO futtatása.
- [**Műszaki mélyreható** ](active-directory-aadconnect-sso-how-it-works.md) – Ez a funkció működésének megismerése.
- [**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-sso.md) -megtudhatja, hogyan tooresolve közös hello szolgáltatást érintő problémákat.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.
