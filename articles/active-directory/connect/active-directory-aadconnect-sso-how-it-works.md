---
title: "Az Azure AD Connect: Zökkenőmentes egyszeri bejelentkezés – hogyan működik |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan hello Azure Active Directory zökkenőmentes egyszeri bejelentkezést szolgáltatás működik."
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
ms.openlocfilehash: 17ce35b32832d241068ab878cf7aac42deab74ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Az Azure Active Directory zökkenőmentes egyszeri bejelentkezés: technikai részletes bemutatója

Ez a cikk lehetővé teszi az technikai részleteket a hello Azure Active Directory zökkenőmentes egyszeri bejelentkezést (zökkenőmentes SSO) funkció működése.

>[!IMPORTANT]
>hello zökkenőmentes SSO funkció jelenleg előzetes verzió.

## <a name="how-does-seamless-sso-work"></a>Hogyan működik a zökkenőmentes SSO?

Ez a szakasz két részből tooit rendelkezik:
1. hello zökkenőmentes SSO szolgáltatás hello beállítása.
2. Egyetlen felhasználó bejelentkezési tranzakcióban működése a zökkenőmentes egyszeri Bejelentkezést.

### <a name="how-does-set-up-work"></a>Hogyan állítsa be a munkahelyi?

Zökkenőmentes SSO engedélyezve van az Azure AD Connect használatával, ahogy [Itt](active-directory-aadconnect-sso-quick-start.md). Hello funkció engedélyezése, miközben a lépéseket követve hello fordulhat elő:
- Nevű számítógépfiók `AZUREADSSOACCT` (amely jelöli az Azure AD) jön létre a helyszíni Active Directory (AD).
- hello számítógépfiók Kerberos visszafejtési kulcs biztonságosan megosztott Azure AD-val.
- Továbbá a két Kerberos egyszerű szolgáltatásnév (SPN) toorepresent két URL-címek az Azure AD-bejelentkezés során használt jönnek létre.

>[!NOTE]
> minden Active Directory-erdőben hello számítógép fiókjának és hello Kerberos egyszerű szolgáltatásnevek jönnek létre tooAzure AD szinkronizálása (Azure AD Connect használatával) és zökkenőmentes SSO szeretne, amelynek felhasználói számára. Helyezze át a hello `AZUREADSSOACCT` számítógép fiók tooan szervezeti egységet (OU), amely a felügyelt tárolt tooensure más számítógépes fiókok esetén hello azonos módon, és nem törlődik.

>[!IMPORTANT]
>Erősen ajánlott, hogy Ön [hello Kerberos visszafejtési kulcs váltása](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) a hello `AZUREADSSOACCT` számítógépfiók legalább 30 nap.

### <a name="how-does-sign-in-with-seamless-sso-work"></a>Hogyan jelentkezzen be munkahelyi zökkenőmentes SSO?

Hello telepítés végrehajtása után a zökkenőmentes SSO hello ugyanaz, mint bármilyen más módon bejelentkezés integrált Windows-hitelesítéssel (IWA) használó működik. hello folyamat a következőképpen történik:

1. hello felhasználó megpróbál tooaccess egy alkalmazás (például az Outlook Web App - hello https://outlook.office365.com/owa/) a tartományhoz csatlakoztatott vállalati eszköz a vállalati hálózaton belül.
2. Hello felhasználó már nem jelentkezett be, ha a hello felhasználó átirányított toohello az Azure AD bejelentkezési oldalára.

  >[!NOTE]
  >Ha hello Azure AD-bejelentkezés kérelemben egy `domain_hint` (azonosító a bérlő – például, contoso.onmicrosoft.com) vagy `login_hint` (hello felhasználó – például azonosító user@contoso.onmicrosoft.com vagy user@contoso.com) paramétert, majd a 2. lépés kimarad.

3. hello felhasználótípusokat hello Azure AD bejelentkezési oldal a felhasználó nevét.
4. A JavaScript használatával hello háttérben, az Azure AD felkéri hello böngésző 401-es jogosulatlan választ, tooprovide keresztül a Kerberos jegyet.
5. hello böngésző, viszont a jegyet az Active Directoryból vonatkozó kérések hello `AZUREADSSOACCT` számítógépfiók (amely az Azure AD).
6. Az Active Directory hello számítógépfiók megkeresi, és adja vissza egy Kerberos jegy toohello böngésző hello számítógépfiók titkos kulcs titkosított.
7. hello böngésző továbbítja azért szerzett, az Active Directory tooAzure AD hello Kerberos-jegy (hello egyik [az Azure AD URL-címek korábban hozzáadott toohello böngésző Intranet zóna beállításai](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).
8. Az Azure AD visszafejtése hello Kerberos-jegyet szereznek, magában foglalja a hello felhasználó identitásával az hello hello vállalati eszköz be van jelentkezve, hello használatával korábban megosztott kulcs.
9. Kiértékelésével, olyan után az Azure AD adja vissza egy token hátsó toohello alkalmazást vagy hello felhasználói tooperform további igazolást, például a multi-factor Authentication kéri.
10. Hello felhasználói bejelentkezés sikeres, hello felhasználói akkor tudja tooaccess hello alkalmazás.

hello alábbi ábrán látható összes hello összetevők és hello lépéseit.

![Zökkenőmentes egyszeri bejelentkezés](./media/active-directory-aadconnect-sso/sso2.png)

Zökkenőmentes SSO alkalmi, ami azt jelenti, ha a hiba, hello bejelentkezés során tapasztal élmény visszavált tooits rendszeres viselkedés - Egytényezős, hello felhasználó számára szükséges tooenter a jelszó toosign a.

## <a name="next-steps"></a>Következő lépések

- [**Gyors üzembe helyezési** ](active-directory-aadconnect-sso-quick-start.md) - létrehozásához, és az Azure AD zökkenőmentes SSO futtatása.
- [**Gyakran ismételt kérdések** ](active-directory-aadconnect-sso-faq.md) -kérdések toofrequently válaszol.
- [**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-sso.md) -megtudhatja, hogyan tooresolve közös hello szolgáltatást érintő problémákat.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.
