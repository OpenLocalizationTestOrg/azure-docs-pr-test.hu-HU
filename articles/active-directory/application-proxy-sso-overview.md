---
title: "egyszeri bejelentkezés az Azure AD alkalmazásproxy aaaManage |} Microsoft Docs"
description: "További tudnivalók az egyszeri bejelentkezés az alkalmazásproxy hello alapjai"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: a278751a5cb1bf98c970a4e5d2eb3edc3b784096
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a>Hogyan nyújt az Azure AD-alkalmazásproxy egyszeri bejelentkezéshez?

Egyszeri bejelentkezés az Azure AD alkalmazásproxy egyik fő eleme.  Mert a felhasználók csak toosign tooAzure Active Directory-hello felhő hello legjobb felhasználói élményt biztosít. Amennyiben az Active Directory tooAzure hitelesítenie, hello alkalmazásproxy-összekötő kezeli hello hitelesítési toohello a helyszíni alkalmazások. a háttéralkalmazás hello nem sikerült megállapítani, jelentkezzen be a Proxy és a normál használata egy tartományhoz csatlakoztatott eszközön keresztül egy távoli felhasználó hello különbsége. 

toouse Azure Active Directoryval az egyszeri bejelentkezés tooyour alkalmazások, kell tooselect **Azure Active Directory** hello előhitelesítési módszer. Ha **csatlakoztatott** ezt követően a felhasználók hitelesítéséhez tooAzure Active Directory egyáltalán nem, de irányított egyenes toohello alkalmazás. E beállítás konfigurálása először közzétenni egy alkalmazást, vagy keresse meg a tooyour hello Azure-portál alkalmazást és hello alkalmazásproxy beállításainak szerkesztése. 

toosee az egyszeri bejelentkezésre vonatkozó beállításokat, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Keresse meg a túl**Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás**.
3. Jelölje be hello alkalmazás beállításai közül, amelynek egyszeri bejelentkezés toomanage szeretné.
4. Válassza ki **egyszeri bejelentkezés**.

   ![Egyszeri bejelentkezés legördülő menü](./media/application-proxy-sso-overview/single-sign-on-mode.png)

hello legördülő menü egyszeri bejelentkezés tooyour alkalmazás öt lehetőségeket mutatja be:

* Az Azure AD az egyszeri bejelentkezés le van tiltva
* Jelszó alapú bejelentkezés
* Csatolt bejelentkezés
* Integrált Windows-hitelesítés
* Fejléc-alapú bejelentkezés

## <a name="azure-ad-single-sign-on-disabled"></a>Az Azure AD az egyszeri bejelentkezés le van tiltva

Ha nem szeretné, egyszeri bejelentkezés tooyour alkalmazás Azure Active Directory-integráció toouse, **az Azure AD az egyszeri bejelentkezés le van tiltva**. Ezt a lehetőséget választja a felhasználók kétszer hitelesíthetők. Először tooAzure Active Directory hitelesítéséhez, és jelentkezzen be magának toohello alkalmazásnak. 

Ez a beállítás akkor hasznos, ha a helyszíni alkalmazások felhasználók tooauthenticate nem igényel, de a tooadd Azure Active Directory biztonsági réteget, a távoli hozzáféréshez. 

## <a name="password-based-sign-on"></a>Jelszó alapú bejelentkezés

Ha a helyszíni alkalmazások szeretné toouse Azure Active Directory és a jelszó-tárolónak, **jelszóalapú bejelentkezés**. Ez a beállítás akkor hasznos, ha az alkalmazás végzi a hitelesítést egy felhasználónév/jelszó kombinált jogkivonatot, vagy a fejlécek helyett. A jelszó alapú bejelentkezés a a felhasználók első alkalommal kell a toohello alkalmazás hello toosign. Ezt követően Azure Active Directory megadja az hello felhasználónév és jelszó hello felhasználó nevében. 

Jelszó alapú bejelentkezés beállításával kapcsolatos információkért lásd: [az egyszeri bejelentkezés az alkalmazásproxy vaulting jelszó](application-proxy-sso-azure-portal.md).

## <a name="linked-sign-on"></a>Csatolt bejelentkezés

Ha már rendelkezik egyszeri bejelentkezéshez megoldás beállítása a helyszíni identitások, válassza a **bejelentkezés kapcsolódó**. Ez a beállítás lehetővé teszi az Azure Active Directory tooleverage meglévő SSO-megoldások, de továbbra is biztosít a felhasználók távelérési toohello alkalmazást. 

Csatolt bejelentkezés (hivatalosan néven meglévő egyszeri bejelentkezés) kapcsolatos információkért lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="integrated-windows-authentication"></a>Integrált Windows-hitelesítés

Ha a helyszíni alkalmazások integrált Windows-Authentication(IWA) használja, vagy ha az egyszeri bejelentkezés toouse Kerberos által korlátozott delegálás (KCD) szeretne, válassza ki a **integrált Windows-hitelesítés**. Ezzel a beállítással a felhasználók csak az Active Directory tooauthenticate tooAzure kell, és majd hello Application Proxy connector megszemélyesít hello felhasználói tooget egy Kerberos-jogkivonata, és jelentkezzen be toohello alkalmazás. 

Integrált Windows-hitelesítés beállításával kapcsolatos információkért lásd: [Kerberos által korlátozott delegálás az egyszeri bejelentkezés az alkalmazásproxy](active-directory-application-proxy-sso-using-kcd.md).

## <a name="header-based-sign-on"></a>Fejléc-alapú bejelentkezés 

Ha az alkalmazások fejlécek használnak a hitelesítéshez, válassza a **fejléc-alapú bejelentkezés**. Ezzel a beállítással a felhasználók csak kell tooauthentication hello Azure Active Directoryban. Microsoft-partnereknek, egy harmadik fél hitelesítési szolgáltatással PingAccess, amely hello Azure Active Directory hozzáférési jogkivonat lefordítani a fejléc formátuma hello alkalmazás neve. 

Fejléc-alapú hitelesítés beállításával kapcsolatos információkért lásd: [fejléc-alapú hitelesítés egyszeri bejelentkezéshez az alkalmazásproxy](application-proxy-ping-access.md).

## <a name="next-steps"></a>Következő lépések

- [Az egyszeri bejelentkezés az alkalmazásproxy vaulting jelszó](application-proxy-sso-azure-portal.md)
- [Kerberos által korlátozott delegálás az egyszeri bejelentkezés az alkalmazásproxy](active-directory-application-proxy-sso-using-kcd.md)
- [Az egyszeri bejelentkezés az alkalmazásproxy fejléc-alapú hitelesítés](application-proxy-ping-access.md) 
