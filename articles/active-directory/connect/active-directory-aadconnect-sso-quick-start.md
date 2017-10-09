---
title: "Az Azure AD Connect: Zökkenőmentes egyszeri bejelentkezés – első lépések |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooget el az Azure Active Directory zökkenőmentes egyszeri bejelentkezést."
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
ms.openlocfilehash: 97d40ed41b3bfad9ff7e11ddbdb8de594ee85de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Az Azure Active Directory zökkenőmentes egyszeri bejelentkezés: első lépések

## <a name="how-toodeploy-seamless-sso"></a>Hogyan toodeploy zökkenőmentes SSO

Azure Active Directory zökkenőmentes egyszeri bejelentkezést (az Azure AD zökkenőmentes SSO) automatikusan amikor azok a vállalati asztali számítógép csatlakoztatott tooyour vállalati hálózaton lévő felhasználók jelentkezik. Hozzáférést biztosít a a felhasználók könnyen tooyour felhőalapú alkalmazások anélkül, hogy semmilyen további helyszíni összetevőt.

>[!IMPORTANT]
>hello zökkenőmentes SSO funkció jelenleg előzetes verzió.

toofollow szüksége toodeploy zökkenőmentes SSO, ezeket a lépéseket:

## <a name="step-1-check-prerequisites"></a>1. lépés: Követelmények ellenőrzésének megismétlése

Győződjön meg arról, hogy hello előfeltételeiről a következő helyen:

1. Az Azure AD Connect-kiszolgáló beállítása: Ha [áteresztő hitelesítés](active-directory-aadconnect-pass-through-authentication.md) legyen a bejelentkezési módszer semmilyen további műveletet nem szükséges. Ha [Jelszókivonat-szinkronizálást](active-directory-aadconnectsync-implement-password-synchronization.md) legyen a bejelentkezési módszer, és ha az Azure AD Connect és az Azure AD között tűzfal, ellenőrizze, hogy:
- Verziók 1.1.484.0 használ vagy újabb, az Azure AD Connect.
- Az Azure AD Connect kommunikálhatnak `*.msappproxy.net` URL-címek és a 443-as porton alatt. Ezt az előfeltételt csak engedélyezi hello szolgáltatást, nem a tényleges felhasználói bejelentkezések esetén alkalmazható.
- Az Azure AD Connect tehet közvetlen IP-kapcsolatok toohello [Azure adatközpont IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653). Ebben az esetben ezt az előfeltételt alkalmazható csak akkor engedélyezi hello szolgáltatást.
2. Tartományi rendszergazdai hitelesítő adatokra van szüksége minden tooAzure AD szinkronizálja Active Directory-erdőben (az Azure AD Connect használatával) és amelynek felhasználók tooenable zökkenőmentes SSO keresi.

## <a name="step-2-enable-hello-feature"></a>2. lépés: Hello szolgáltatás engedélyezése

Zökkenőmentes SSO használatával engedélyezhető [az Azure AD Connect](active-directory-aadconnect.md).

Ha az Azure AD Connect új példánya, válassza ki a hello [egyéni telepítési útvonal](active-directory-aadconnect-get-started-custom.md). Hello "Felhasználói bejelentkezés" lapon jelölje be hello "Engedélyezése egyszeri bejelentkezéshez" beállítást.

![Az Azure AD Connect - felhasználói bejelentkezés](./media/active-directory-aadconnect-sso/sso8.png)

Ha már rendelkezik egy Azure AD Connect telepítése, válassza a "Módosítási felhasználói bejelentkezési oldalon" az Azure AD Connect, és kattintson a "Tovább gombra". Ezután jelölje be hello "Engedélyezése egyszeri bejelentkezéshez" beállítást.

![Az Azure AD Connect - módosítás felhasználói bejelentkezés](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Továbbra is hello varázsló lépéseinek, amíg elér toohello "Engedélyezése egyszeri bejelentkezéshez" lapon. Tartományi rendszergazdai hitelesítő adatok megadása minden Active Directory-erdőben tooAzure AD szinkronizálja (Azure AD Connect használatával) és amelynek felhasználók tooenable zökkenőmentes SSO keresi. 

Zökkenőmentes SSO hello varázsló befejezése után a bérlői engedélyezett.

>[!NOTE]
> hello tartományi rendszergazda hitelesítő adatai nem tárolja az Azure AD Connectben vagy az Azure ad-ben, de csak a felhasznált tooenable hello szolgáltatást.

Hajtsa végre az ezen utasításokat tooverify engedélyezte zökkenőmentes SSO megfelelően:

1. Jelentkezzen be toohello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) hello globális rendszergazdai hitelesítő adataival, a bérlő számára.
2. Válassza ki **Azure Active Directory** hello bal oldali navigációs.
3. Válassza ki **az Azure AD Connect**.
4. Győződjön meg arról, hogy hello **zökkenőmentes egyszeri bejelentkezést** funkció jelenít meg, mint a **engedélyezve**.

![Azure portál – az Azure AD Connect panel](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-hello-feature"></a>3. lépés: Megkezdik hello szolgáltatás

hello szolgáltatás tooyour felhasználók tooroll, meg kell tooadd két Azure AD URL-címek (https://autologon.microsoftazuread-sso.com és https://aadg.windows.net.nsatc.net) toousers' Intranet zóna beállításait az Active Directory csoportházirend segítségével.

>[!NOTE]
> hello a következő utasítások csak olyan feladaton végezhető az Internet Explorer és a Google Chrome Windows (ha az Internet Explorer Megbízható helyek URL-osztanak azt). Olvassa el a következő szakaszban hello utasításokat tooset Mozilla Firefox és a Mac Google Chrome

### <a name="why-do-you-need-toomodify-users-intranet-zone-settings"></a>Miért kell toomodify felhasználók Intranet zóna beállítását?

Alapértelmezés szerint hello böngésző automatikusan kiszámítja hello jobb zóna (internetes vagy intranetes) URL-címről. Például http://contoso/ csatlakoztatott toohello intranetzónához, mivel http://intranet.contoso.com/ csatlakoztatott toohello Internet zóna (mert hello URL-cím pontot tartalmaz). Böngészők ne küldjön a Kerberos jegyek tooa felhővégpontnak - hello két Azure AD URL-címekhez hasonlít -, kivéve, ha az URL-cím van kifejezetten toohello böngésző Intranet zónához.

### <a name="detailed-steps"></a>Részletes lépések

1. Hello Csoportházirend kezelése eszköz megnyitásához.
2. Hello csoportházirend által alkalmazott toosome vagy a felhasználók szerkesztése. A jelen példában használjuk hello **alapértelmezett tartományházirend**.
3. Keresse meg a túl**Felhasználó konfigurációja\Felügyeleti sablonok\Windows összetevők\Internet Explorer\Internet vezérlő Panel\Security lap** válassza **tooZone társításának listája hely**.
![Egyszeri bejelentkezés](./media/active-directory-aadconnect-sso/sso6.png)  
4. Hello házirend engedélyezése, és írja be a következő értékeket (az Azure AD URL-címek amelyben továbbított Kerberos-jegyek) hello és adatok (*1* Intranet zóna jelzi) hello párbeszédpanelen.

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> Toodisallow egyes felhasználók a zökkenőmentes SSO - példányhoz, ha ezek a felhasználók a megosztott számítógépeken - bejelentkezés állítsa az értékek túl megelőző hello*4*. Ez a művelet hello Azure AD URL-címek toohello tiltott helyek zóna hozzáadása, és zökkenőmentes SSO mindig hello sikertelen.

5. Kattintson a **OK** és **OK** újra.

![Egyszeri bejelentkezés](./media/active-directory-aadconnect-sso/sso7.png)

### <a name="browser-considerations"></a>Böngésző kapcsolatos szempontok

#### <a name="mozilla-firefox"></a>Mozilla Firefox

Mozilla Firefox automatikusan nem használ Kerberos-hitelesítést. Minden felhasználó rendelkezik-e toomanually hello Azure AD URL-címek tootheir Firefox a beállításokat a következő lépéseket hello hozzáadása:
1. Futtassa a Firefox, és adja meg `about:config` hello címsorába. Hagyja figyelmen kívül belőle értesítéseket, amelyek akkor jelennek meg.
2. Keresse meg a hello **network.negotiate-auth.trusted-URI-azonosítók** beállítás. Ez a beállítás a Firefox a megbízható helyek a Kerberos-hitelesítést sorolja fel.
3. Kattintson a jobb gombbal, és válassza a "Módosítás".
4. Adja meg "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" hello mezőben.
5. Az "OK" gombra, majd nyissa meg a hello böngésző.

#### <a name="safari-on-mac-os"></a>Mac OS Safari

Győződjön meg arról, hogy fut a Mac OS hello gép illesztett tooAD. Útmutatás meg, hogyan toodo, amely [Itt](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).

#### <a name="google-chrome-on-mac-os"></a>Google Chrome Mac OS

Google Chrome a Mac OS és más nem Windows platformokon, tekintse meg túl[Ez a cikk](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) hogyan toowhitelist hello integrált hitelesítés URL-címeit az Azure AD tájékoztatást.

Külső Active Directory csoportházirend segítségével bővítmények tooroll hello Azure AD URL-címek tooFirefox és Google Chrome Mac-felhasználók nem érhető el ez a cikk.

#### <a name="known-limitations"></a>Ismert korlátozásai

Privát böngészés módban a Firefox és Edge böngésző zökkenőmentes SSO nem működik. Még nem működik az Internet Explorer hello böngésző fokozott védelmet módban fut. Ha.

>[!IMPORTANT]
>A Microsoft nemrég visszaállítása peremhálózati tooinvestigate támogatása ügyfél jelentett problémák.

## <a name="step-4-test-hello-feature"></a>4. lépés: Hello szolgáltatás tesztelése

tootest hello szolgáltatás adott felhasználó, ügyeljen arra, hogy _összes_ hello a következő feltételek érvényesek:
  - Vállalati eszköz hello felhasználó jelentkezik be.
  - hello eszköz lett-e korábban csatlakoztatott tooyour Active Directory (AD) tartományhoz.
  - hello eszközhöz tartozik egy közvetlen kapcsolat tooyour tartományvezérlőn (DC), vagy a hello vállalati vezetékes vagy vezeték nélküli hálózathoz, vagy távelérési kapcsolatot, például a VPN-kapcsolaton keresztül.
  - Rendelkezik [hello szolgáltatás megkezdődött](##step-3-roll-out-the-feature) toothis felhasználói csoportházirend használatával.

tootest hello forgatókönyv ahol hello felhasználói belép csak hello felhasználónév, de nem hello jelszó:
- Jelentkezzen be a *https://myapps.microsoft.com/* egy új titkos böngésző-munkamenetben.

tootest hello forgatókönyv ahol hello a felhasználó nem rendelkezik a tooenter hello felhasználónév vagy hello jelszó: 
- Jelentkezzen be a *https://myapps.microsoft.com/contoso.onmicrosoft.com* egy új titkos böngésző-munkamenetben. Cserélje le "*contoso*" a bérlő névvel.
- Jelentkezzen be vagy *https://myapps.microsoft.com/contoso.com* egy új titkos böngésző-munkamenetben. Cserélje le "*contoso.com*" a bérlő ellenőrzött tartomány (nem összevont tartományhoz).

## <a name="step-5-roll-over-keys"></a>5. lépés: A kulcs váltása

2. lépésben az Azure AD Connect számítógépfiókot hoz létre (az Azure AD közti) az összes hello AD-erdőben, amelyiken engedélyezte a zökkenőmentes SSO. Ismerje meg, a részletesebb [Itt](active-directory-aadconnect-sso-how-it-works.md). A nagyobb biztonság érdekében javasoljuk, hogy Ön gyakran váltása hello Kerberos visszafejtési kulcs esetében a számítógépfiókok.

>[!IMPORTANT]
>Nem kell toodo ebben a lépésben _azonnal_ hello szolgáltatás engedélyezése után. Váltása hello Kerberos visszafejtési kulcs esetében legalább 30 nap.

## <a name="next-steps"></a>Következő lépések

- [**Műszaki mélyreható** ](active-directory-aadconnect-sso-how-it-works.md) – Ez a funkció működésének megismerése.
- [**Gyakran ismételt kérdések** ](active-directory-aadconnect-sso-faq.md) -kérdések toofrequently válaszol.
- [**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-sso.md) -megtudhatja, hogyan tooresolve közös hello szolgáltatást érintő problémákat.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.
