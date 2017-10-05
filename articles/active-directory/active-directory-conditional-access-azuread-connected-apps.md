---
title: "Azure feltételes hozzáférés SaaS-alkalmazásokhoz |} Microsoft Docs"
description: "Feltételes hozzáférés az Azure AD lehetővé teszi, hogy alkalmazásonként többtényezős hitelesítést hozzáférési szabályok és a nem megbízható hálózaton felhasználók hozzáférésének blokkolása konfigurálhatja. "
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: efaa70467346e910a78a63d182041029bb34b1cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Azure Active Directory feltételes hozzáférés – első lépések
Az Azure Active Directory feltételes hozzáférés a [SaaS](https://azure.microsoft.com/overview/what-is-saas/) alkalmazásokat és az Azure AD csatlakoztatott csoport, a hely és az alkalmazás érzékenysége alapján feltételes hozzáférést konfigurál apps segítségével. 

Feltételes hozzáféréssel alkalmazás érzékenysége alapján többtényezős hitelesítés (MFA) hozzáférési szabályok alkalmazásonként állíthatja be. Alkalmazásonként többtényezős hitelesítés lehetővé teszi a felhasználók, akik nem megbízható hálózatokon hozzáférésének blokkolása. MFA szabályokat alkalmazhat az alkalmazáshoz, vagy csak a felhasználók számára megadott biztonsági csoportokban rendelt összes felhasználó.  Felhasználók kizárhatók a többtényezős hitelesítés követelménye, ha az alkalmazás érik el a belsejében a vállalati hálózaton található IP-címet.

Ezek a képességek, amelyek az Azure Active Directory Premium-licencet vásárolt használó ügyfelek számára elérhető lesz.

## <a name="scenario-prerequisites"></a>A forgatókönyv előfeltételei
* Az Azure Active Directory Premium licenc
* Összevont vagy kezelt Azure Active Directory-bérlő
* Összevont bérlők használatához, hogy a multi-factor authentication engedélyezni kell.

## <a name="configure-per-application-access-rules"></a>Alkalmazás hozzáférési szabályok konfigurálása
Ez a szakasz ismerteti az alkalmazás hozzáférési szabályok konfigurálása.

1. Jelentkezzen be a klasszikus Azure portálra egy olyan fiókkal, amely az Azure AD globális rendszergazda.
2. A bal oldali panelen válassza az **Active Directory** elemet.
3. A könyvtár lapon válassza ki a címtárát.
4. Válassza ki a **alkalmazások** fülre.
5. Válassza ki az alkalmazást, amely a szabályhoz lesznek beállítva.
6. Válassza a **Konfigurálás** lapot.
7. Görgessen le a hozzáférési szabályok szakaszban. Jelölje ki a kívánt hozzáférési szabályt.
8. Adja meg a felhasználókat, a szabály vonatkozik.
9. A házirend engedélyezéséhez jelölje ki **engedélyezve kell lennie a**.

## <a name="understanding-access-rules"></a>Hozzáférési szabályok ismertetése
Ez a szakasz részletes leírást ad a hozzáférési szabályok támogatja az Azure feltételes alkalmazás-hozzáférés.

### <a name="specifying-the-users-the-access-rules-apply-to"></a>A felhasználók, a hozzáférési szabályok alkalmazása
Alapértelmezés szerint a házirend az alkalmazáshoz hozzáféréssel rendelkező összes felhasználóra vonatkozni fognak. Azonban korlátozhatja is a szabályzatot a felhasználók számára, hogy a megadott biztonsági csoportok tagjai. A **csoport hozzáadása** gomb használatával válassza ki egy vagy több csoportok a csoport kiválasztása párbeszédpanel, amely a hozzáférési szabály vonatkozik. Ezen a párbeszédpanelen kijelölt csoportok törlése is használható. Ha a szabályok csoportjaira alkalmazhatók van kijelölve, a hozzáférési szabályok csak kényszeríti ki a felhasználók számára, amely a megadott biztonsági csoportok egyikéhez tartozik.

Biztonsági csoportok is explicit módon kizárhatók a házirend kiválasztásával a **kivéve** lehetőséget, és egy vagy több csoport megadása. Felhasználók, akik a csoport tagja a **kivéve** lista nem vesznek részt a multi-factor authentication követelményeinek, akkor is, ha tagja egy csoportnak, amelyre a hozzáférési szabály vonatkozik.
Az alább látható hozzáférési szabály szükséges többtényezős hitelesítés használatára, az alkalmazáshoz való hozzáféréskor a vezetők csoport minden tagjára.

![Feltételes hozzáférési szabályok a multi-factor Authentication szolgáltatás beállítása](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>Feltételes hozzáférési szabályai többtényezős hitelesítéssel
Ha a felhasználó a felhasználói a multi-factor authentication szolgáltatás használatával lett konfigurálva, ezt a beállítást, a felhasználó a multi-factor Authentication hitelesítés szabályoknak az alkalmazás fogja össze. Ez azt jelenti, hogy a felhasználó a multi-factor authentication felhasználói konfigurált lesz szükség ahhoz, hogy többtényezős hitelesítést végezni, akkor is, ha az alkalmazáshoz a multi-factor authentication szabályok alól. További tudnivalók a többtényezős hitelesítés és az egyes felhasználók beállításait.

### <a name="access-rule-options"></a>Hozzáférési szabály beállítása
A következő beállításokat támogatja:

* **Többtényezős hitelesítés megkövetelése**: A felhasználók, akiknek a hozzáférési szabályok vonatkoznak, az alkalmazást, amely a házirend érvényes elérése előtt teljes többtényezős hitelesítést lesz.
* **Ha nem munkahelyi többtényezős hitelesítést**: egy felhasználó adatforrásból származó megbízható IP-címnek nem kell-e többtényezős hitelesítés. A többtényezős hitelesítési beállítások lapon konfigurálhatja a megbízható IP-címtartományok.
* **Letiltja a hozzáférést, ha nem munkahelyi**: nem származik megbízható IP-címnek felhasználó le lesz tiltva. A többtényezős hitelesítési beállítások lapon konfigurálhatja a megbízható IP-címtartományok.

### <a name="setting-rule-status"></a>A szabály állapotának beállítása
Hozzáférési szabály állapota lehetővé teszi, hogy a szabályok be- és kikapcsolhatja. Ha a hozzáférési szabályok vannak kapcsolva, a multi-factor authentication követelményeinek nem lép életbe.

### <a name="access-rule-evaluation"></a>Hozzáférési szabály értékelése
Hozzáférési szabályok van kiértékelve, amikor egy felhasználó egy összevont alkalmazás által használt OAuth 2.0-s, OpenID Connect, SAML és WS-Federation fér hozzá. Ezenkívül hozzáférési szabályok értékelésének az OAuth 2.0 és az OpenID Connect használja a frissítési jogkivonat olyan hozzáférési jogkivonatot. Házirend kiértékelése sikertelen lesz, amikor a frissítési token történik, ha a hiba **invalid_grant** adja vissza, ez azt jelzi, hogy a felhasználónak újra hitelesíteni az ügyfélnek van szüksége.

### <a name="configure-federation-services-to-provide-multi-factor-authentication"></a>A többtényezős hitelesítés összevonási szolgáltatások konfigurálása
Összevont bérlők MFA hajthatja végre az Azure Active Directory vagy a helyszíni AD FS-kiszolgálón.

Alapértelmezés szerint az MFA egy Azure Active Directory által üzemeltetett oldalon történik. Konfigurálhatja az MFA a helyszínen, a **– SupportsMFA** tulajdonság értékre kell állítani **igaz** az Azure Active Directoryban, az Azure AD-modullal a Windows PowerShell használatával.

A következő példa bemutatja, hogyan engedélyezése a helyi multi-factor Authentication használatával a [Set-MsolDomainFederationSettings parancsmag](https://msdn.microsoft.com/library/azure/dn194088.aspx) a contoso.com tenant:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Ez a jelző mellett az összevont bérlői AD FS-példányt kell állítani a többtényezős hitelesítést végezni. Kövesse az utasításokat [üzembe helyezése az Azure multi-factor Authentication helyszíni](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directoryhoz csatlakoztatott Office 365 és az egyéb alkalmazások hozzáférésének biztonságossá tétele](active-directory-conditional-access.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)

