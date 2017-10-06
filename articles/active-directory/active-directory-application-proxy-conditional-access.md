---
title: "aaaConditional hozzáférés tooon helyszínen alkalmazások – az Azure AD |} Microsoft Docs"
description: "Ismerteti, hogyan tooset feltételes hozzáférés beállítása alkalmazások közzététele toobe elérhető távolról az Azure AD alkalmazásproxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2e97722b-eb4e-4078-b607-9fed210d8a0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 7bed25dd4ba17941e77d8c4b2b9ba4edcf0cf597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-conditional-access-in-azure-ad-application-proxy"></a>Feltételes hozzáférés az Azure AD alkalmazásproxy használata

>[!NOTE]
>Ez a cikk a klasszikus Azure portál, amely a használatból van toohello vonatkozik. Azt javasoljuk, hogy használja-e hello [Azure-portálon](https://portal.azure.com). Hello Azure-portálon, az alkalmazások is rendelkeznek alkalmazásproxy mint bármilyen más SaaS-alkalmazás hello azonos feltételes hozzáférési funkciókat. További információ a feltételes hozzáférés toolearn lásd [Ismerkedés a feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal-get-started.md).

Konfigurálhatja a hozzáférési szabályok toogrant feltételes hozzáférés tooapplications közzé a Proxy használatát. Ezzel a következőket teheti:

* Alkalmazásonként többtényezős hitelesítést
* Többtényezős hitelesítés szükséges, csak akkor, ha a felhasználók nem rendelkeznek a munkahelyi hálózatban
* Hello alkalmazás elérésének, amikor nincsenek munkahelyi felhasználók megakadályozása

Ezek a szabályok lehetnek alkalmazott tooall felhasználók és csoportok vagy csak toospecific felhasználók és csoportok. Alapértelmezés szerint a hello szabály tooall, akik rendelkeznek access toohello alkalmazást. Azonban hello szabály korlátozott toousers, amelyek adott biztonsági csoport tagjai is lehetnek.  

Hozzáférési szabályok van kiértékelve, amikor egy felhasználó egy összevont alkalmazás által használt OAuth 2.0, az OpenID Connect, a SAML-alapú vagy a WS-Federation fér hozzá. Ezenkívül hozzáférési szabályok értékelésének OAuth 2.0 és az OpenID Connect egy frissítési token használt tooacquire olyan hozzáférési jogkivonatot.

## <a name="conditional-access-prerequisites"></a>Feltételes hozzáférés Előfeltételek
* Active Directory Premium előfizetés tooAzure
* Egy összevont vagy kezelt Azure Active Directory-bérlő
* Összevont bérlők szükséges többtényezős hitelesítés (MFA)  
    ![Hozzáférési szabályainak beállítása – többtényezős hitelesítést](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Alkalmazásonként többtényezős hitelesítés beállítása
1. Jelentkezzen be rendszergazdaként hello a klasszikus Azure portálon.
2. Nyissa meg tooActive könyvtár, és válassza ki a kívánt tooenable proxyval hello könyvtár.
3. Kattintson a **alkalmazások** és toohello görgetve **hozzáférési szabályok** szakasz. hello hozzáférési szabályok szakaszban csak akkor jelenik meg, az alkalmazások közzététele az alkalmazásproxy használatával összevont hitelesítést használó.
4. Hello szabály engedélyezéséhez jelölje ki **engedélyezése hozzáférési szabályok** túl**a**.
5. Adja meg a hello felhasználók és csoportok toowhom hello szabályok vonatkoznak. Használjon hello **csoport hozzáadása** tooselect gombra egy vagy több csoportjára toowhich hello hozzáférési szabály vonatkozik. Ezen a párbeszédpanelen kijelölt használt tooremove csoportok is lehet.  Ha hello szabályok kijelölt tooapply toogroups, hello hozzáférési szabályok esetében kötelezően érvényben van csak a megadott hello tooone tartozó felhasználók biztonsági csoportokat.  

   * tooexplicitly kizárási biztonsági csoportok hello szabályból ellenőrizze **kivéve** , és adja meg egy vagy több csoportjára. Felhasználók, akik tagjai egy csoportot a listában kivéve hello nincsenek szükséges tooperform többtényezős hitelesítést.  
   * Ha egy felhasználó hello felhasználói a multi-factor authentication szolgáltatás használatával lett konfigurálva, ez a beállítás elsőbbséget élvez hello alkalmazáshoz a multi-factor authentication szabályok. A felhasználó, aki engedélyezve van a felhasználói a multi-factor authentication szükséges tooperform a multi-factor authentication akkor is, ha az alkalmazás hello többtényezős hitelesítési szabályok alól. További információ [többtényezős hitelesítés és a felhasználói beállítások](../multi-factor-authentication/multi-factor-authentication.md).
6. Válassza ki a kívánt tooset hello hozzáférési szabályt:

   * **Többtényezős hitelesítés megkövetelése**: felhasználók toowhom hozzáférési szabályokat alkalmazza a rendszer szükséges toocomplete többtényezős hitelesítést, mielőtt elérése során hello alkalmazás toowhich hello szabály vonatkozik.
   * **Ha nem munkahelyi többtényezős hitelesítést**: tooaccess hello alkalmazás egy megbízható IP-címről érkező felhasználók csak akkor szükséges tooperform többtényezős hitelesítést. hello megbízható IP-címtartományok hello többtényezős hitelesítési beállítások lapon konfigurálhatja.
   * **Letiltja a hozzáférést, ha nem munkahelyi**: tooaccess hello alkalmazást a vállalati hálózaton kívüli felhasználók nem lesz képes tooaccess hello alkalmazás.

## <a name="configuring-mfa-for-federation-services"></a>Többtényezős hitelesítés konfigurálása összevonási szolgáltatások
Összevont bérlők, többtényezős hitelesítés (MFA) hajthatja végre hello vagy Azure Active Directory által a helyszíni AD FS-kiszolgálón. Többtényezős hitelesítés alapértelmezés szerint bármely Azure Active Directory által üzemeltetett lapján történik. tooconfigure többtényezős hitelesítés a helyszíni, futtassa a Windows PowerShell és a használata hello – SupportsMFA tulajdonság tooset hello Azure AD-modullal.

hello következő példa bemutatja, hogyan tooenable helyszíni MFA hello segítségével [Set-MsolDomainFederationSettings parancsmag](https://msdn.microsoft.com/library/azure/dn194088.aspx) hello contoso.com tenant:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Továbbá toosetting ezt a jelzőt, hello összevont bérlői AD FS-példányt kell tooperform a multi-factor authentication konfigurálva. Kövesse az utasításokat hello [központi telepítése a Microsoft Azure multi-factor Authentication hitelesítés a helyszíni](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="see-also"></a>Lásd még:
* [Munkavégzés jogcímeket figyelembe vevő alkalmazásokkal](active-directory-application-proxy-claims-aware-apps.md)
* [Alkalmazások közzététele az alkalmazásproxy](active-directory-application-proxy-publish.md)
* [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
* [Alkalmazások közzététele saját tartománynév használatával](active-directory-application-proxy-custom-domains.md)

Hello legfrissebb híreket és frissítéseket, tekintse meg a hello [alkalmazásproxy blog](http://blogs.technet.com/b/applicationproxyblog/)
