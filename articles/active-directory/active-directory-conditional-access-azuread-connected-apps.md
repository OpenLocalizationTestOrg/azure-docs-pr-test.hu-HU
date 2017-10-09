---
title: "Feltételes hozzáférés a Szolgáltatottszoftver-alkalmazásoknál aaaAzure |} Microsoft Docs"
description: "Feltételes hozzáférés az Azure AD lehetővé teszi, hogy akkor tooconfigure a multi-factor authentication alkalmazás hozzáférési szabályok és hello képességét tooblock hozzáférés a felhasználók számára nem megbízható hálózaton. "
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
ms.openlocfilehash: 69748014c0c8e266ba66562980c784aba4c68d80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Azure Active Directory feltételes hozzáférés – első lépések
Az Azure Active Directory feltételes hozzáférés a [SaaS](https://azure.microsoft.com/overview/what-is-saas/) alkalmazásokat és az Azure AD csatlakoztatott csoport, a hely és az alkalmazás érzékenysége alapján feltételes hozzáférést konfigurál apps segítségével. 

Feltételes hozzáféréssel alkalmazás érzékenysége alapján többtényezős hitelesítés (MFA) hozzáférési szabályok alkalmazásonként állíthatja be. Alkalmazásonként többtényezős Hitelesítést hello képességét tooblock hozzáférést biztosít a felhasználók számára, akik nem megbízható hálózatokon. Többtényezős hitelesítés szabályok tooall felhasználók vannak hozzárendelve toohello alkalmazás, vagy csak a felhasználók belül meghatározott biztonsági csoportok is alkalmazhatja.  Felhasználók kizárhatók hello többtényezős hitelesítés követelménye, ha hello alkalmazás hello vállalati hálózaton belül az IP-címről érik el.

Ezek a képességek lesz elérhető toocustomers, amely Azure Active Directory Premium-licencet vásárolt.

## <a name="scenario-prerequisites"></a>A forgatókönyv előfeltételei
* Az Azure Active Directory Premium licenc
* Összevont vagy kezelt Azure Active Directory-bérlő
* Összevont bérlők használatához, hogy a multi-factor authentication engedélyezni kell.

## <a name="configure-per-application-access-rules"></a>Alkalmazás hozzáférési szabályok konfigurálása
Ez a szakasz ismerteti, hogyan férnek hozzá az alkalmazásokhoz tooconfigure szabályok.

1. Jelentkezzen be toohello a klasszikus Azure portál, amely az Azure AD globális rendszergazda fiókkal.
2. Hello bal oldali ablaktáblában jelölje ki **Active Directory**.
3. Hello könyvtár lapján válassza ki a címtárát.
4. Jelölje be hello **alkalmazások** fülre.
5. A szabály hello válassza hello alkalmazás lesz beállítva.
6. Jelölje be hello **konfigurálása** fülre.
7. Görgessen lefelé toohello hozzáférési szabályok szakasz. Válassza ki a hello kívánt hozzáférési szabály.
8. Adja meg a hello felhasználók hello szabály vonatkozik.
9. Hello házirend engedélyezéséhez jelölje ki **toobe engedélyezve**.

## <a name="understanding-access-rules"></a>Hozzáférési szabályok ismertetése
Ez a szakasz részletes leírást ad hello hozzáférési szabályok Azure feltételes alkalmazás-hozzáférés hello támogatott.

### <a name="specifying-hello-users-hello-access-rules-apply-to"></a>Adja meg hello felhasználók hello hozzáférési szabályok alkalmazása
Alapértelmezés szerint a hello szabályzat hatálya alá tooall felhasználóknak, akik számára access toohello alkalmazást. Azonban korlátozhatja is, amelyek tagjai a megadott hello hello házirend toousers biztonsági csoportokat. Hello **csoport hozzáadása** gomb használt tooselect egy vagy több olyan csoportot hello csoport kiválasztása párbeszédpanel, amely hello hozzáférési szabály vonatkozik. Ezen a párbeszédpanelen kijelölt használt tooremove csoportok is lehet. Ha hello szabályok kijelölt tooapply tooGroups, hello hozzáférési szabályok csak kényszeríti ki a felhasználók számára a megadott hello tooone tartozó biztonsági csoportokat.

Biztonsági csoportok is explicit módon kizárhatók hello házirend hello kiválasztásával **kivéve** lehetőséget, és egy vagy több csoport megadása. Felhasználók, akik a hello egy csoport tagjai **kivéve** lista csak akkor tulajdonos toohello multi-factor authentication követelményeinek, akkor is, ha tagja egy csoportnak, hogy hello hozzáférési szabály vonatkozik.
hello hozzáférési szabály alább látható lesz, a felhasználóknak minden a hello vezetők csoport toouse multi-factor authentication hello alkalmazáshoz való hozzáféréskor.

![Feltételes hozzáférési szabályok a multi-factor Authentication szolgáltatás beállítása](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>Feltételes hozzáférési szabályai többtényezős hitelesítéssel
Ha egy felhasználó hello felhasználói a multi-factor authentication szolgáltatás használatával lett konfigurálva, ezt a beállítást az hello felhasználói hello a multi-factor authentication szabályokkal hello alkalmazás fogja össze. Ez azt jelenti, hogy a felhasználó a multi-factor authentication felhasználói konfigurált lesz szükséges tooperform többtényezős hitelesítést, akkor is, ha az hello alkalmazáshoz a multi-factor authentication szabályok alól. További tudnivalók a többtényezős hitelesítés és az egyes felhasználók beállításait.

### <a name="access-rule-options"></a>Hozzáférési szabály beállítása
a következő beállítások hello támogatottak:

* **Többtényezős hitelesítés megkövetelése**: toowhom hello hozzáférési szabályok vonatkoznak, hello felhasználók fognak szükséges toocomplete a multi-factor authentication ahhoz, hogy a házirend hello hello alkalmazás elérésének vonatkozik.
* **Ha nem munkahelyi többtényezős hitelesítést**: egy olyan felhasználó, a megbízható IP-cím érkezik csak akkor szükséges tooperform többtényezős hitelesítést. hello megbízható IP-címtartományok hello többtényezős hitelesítési beállítások lapon konfigurálhatja.
* **Letiltja a hozzáférést, ha nem munkahelyi**: nem származik megbízható IP-címnek felhasználó le lesz tiltva. hello megbízható IP-címtartományok hello többtényezős hitelesítési beállítások lapon konfigurálhatja.

### <a name="setting-rule-status"></a>A szabály állapotának beállítása
Hozzáférési szabály állapota lehetővé teszi, hogy hello szabályok be- és kikapcsolhatja. Amikor hello hozzáférési szabályok vannak kapcsolva, hello többtényezős hitelesítésre vonatkozó követelmény nincs alkalmazva.

### <a name="access-rule-evaluation"></a>Hozzáférési szabály értékelése
Hozzáférési szabályok van kiértékelve, amikor egy felhasználó egy összevont alkalmazás által használt OAuth 2.0-s, OpenID Connect, SAML és WS-Federation fér hozzá. Ezenkívül hozzáférési szabályok értékelésének hello OAuth 2.0 és az OpenID Connect használata a frissítési token tooacquire olyan hozzáférési jogkivonatot. Házirend kiértékelése sikertelen a frissítési jogkivonat használata esetén, ha hiba hello **invalid_grant** adja vissza, ez azt jelzi, hogy hello felhasználói kell toore-toohello ügyfél hitelesítésére.

### <a name="configure-federation-services-tooprovide-multi-factor-authentication"></a>Összevonási szolgáltatások tooprovide többtényezős hitelesítés beállítása
Összevont bérlők MFA hajthatja végre hello vagy Azure Active Directory által a helyszíni AD FS-kiszolgálón.

Alapértelmezés szerint az MFA egy Azure Active Directory által üzemeltetett oldalon történik. tooconfigure többtényezős hitelesítés a helyszíni hello **– SupportsMFA** tulajdonság túl be kell állítani**igaz** az Azure Active Directoryban, hello Azure AD-modul Windows PowerShell használatával.

hello következő példa bemutatja, hogyan tooenable helyszíni MFA hello segítségével [Set-MsolDomainFederationSettings parancsmag](https://msdn.microsoft.com/library/azure/dn194088.aspx) hello contoso.com tenant:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Továbbá toosetting ezt a jelzőt, hello összevont bérlői AD FS-példányt kell tooperform a multi-factor authentication konfigurálva. Kövesse az utasításokat hello [üzembe helyezése az Azure multi-factor Authentication helyszíni](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Active Directory tooAzure tooOffice 365 és az egyéb alkalmazások hozzáférésének biztonságossá tétele csatlakoztatva](active-directory-conditional-access.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)

