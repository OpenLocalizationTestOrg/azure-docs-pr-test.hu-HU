---
title: "vállalati alkalmazások az Azure Active Directory hello aaaSingle bejelentkezés felügyeleti |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage az egyszeri bejelentkezés a vállalati alkalmazások hello Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: asmalser
manager: femila
editor: 
ms.assetid: bcc954d3-ddbe-4ec2-96cc-3df996cbc899
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: asmalser
ms.openlocfilehash: b0a8e622ab10517b7b69f786406b6e9b9f2e7eaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-single-sign-on-for-enterprise-apps"></a>Egyszeri bejelentkezés a vállalati alkalmazások kezelése
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-enterprise-apps-manage-sso.md)
> * [klasszikus Azure portál](active-directory-sso-integrate-saas-apps.md)
> 

Ez a cikk ismerteti, hogyan toouse hello [Azure-portálon](https://portal.azure.com) toomanage egyszeri bejelentkezés vállalati alkalmazások beállításait. Vállalati alkalmazások olyan alkalmazások, telepített és a szervezetén belül. Ez a cikk vonatkozik, különösen a hello hozzáadott tooapps [Azure Active Directory alkalmazáskatalógusában](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). 

## <a name="finding-your-apps-in-hello-portal"></a>Az alkalmazások keresése hello portálon
Minden vállalati alkalmazás, amely az egyszeri bejelentkezés beállítása tekinthetők meg és hello Azure-portálon kezelhetők. hello alkalmazások hello található **több szolgáltatások** &gt; **vállalati alkalmazások** hello portál szakaszban. 

![Vállalati alkalmazások panel][1]

Válassza ki **összes alkalmazás** tooview konfigurált összes alkalmazások listáját. Hello erőforráspanelen az alkalmazáshoz, ahol jelentések megtekinthetők az alkalmazáshoz, és különböző beállítások kezelheti egy alkalmazás kiválasztása tölti be.

toomanage egyszeri bejelentkezés beállítást, **egyszeri bejelentkezés**.

![Alkalmazás erőforráspanelen][2]

## <a name="single-sign-on-modes"></a>Egyszeri bejelentkezési módok
Hello **egyszeri bejelentkezés** panel kezdődik egy **mód** menü, amely lehetővé teszi a hello egyszeri bejelentkezés mód toobe konfigurálva. hello elérhető lehetőségek a következők:

* **SAML-alapú bejelentkezés** – Ez a beállítás akkor használható, ha hello alkalmazás teljes összevont egyszeri bejelentkezést támogatja az Azure Active Directoryval hello SAML 2.0 protokoll használatával.
* **Jelszó alapú bejelentkezés** – Ez a beállítás akkor használható, ha az Azure AD jelszó űrlap kitöltését az alkalmazás támogatja.
* **A bejelentkezési kapcsolódó** – korábbi nevén a "Meglévő egyszeri bejelentkezéshez", ez a beállítás lehetővé teszi a rendszergazdák tooplace egy hivatkozásra a felhasználó Azure AD hozzáférési Panel vagy az Office 365 alkalmazásindító toothis alkalmazás.

Ezek módokkal kapcsolatos további információkért lásd: [hogyan egyszeri bejelentkezés az Azure Active Directory munkahelyi](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="saml-based-sign-on"></a>SAML-alapú bejelentkezés
Hello **SAML-alapú bejelentkezés** lehetőséget választja, megnyílik egy panel, amelyen a forgatókönyv négy részből áll:

### <a name="domains-and-urls"></a>Tartományok és URL-címek
Ez azért, ahol minden részleteit hello alkalmazás tartományi és URL-címek tooyour Azure AD-címtár kerülnek-e. Összes bemenet szükséges az toomake egyszeri bejelentkezés munkahelyi alkalmazás megjelenő közvetlenül üdvözlő képernyőt, mivel az összes opcionális bemenet hello kiválasztásával tekinthető **megjelenítése speciális URL-beállításainak** jelölőnégyzetet. hello támogatott bemeneti adatok teljes listáját tartalmazza:

* **URL-cím bejelentkezési** – amennyiben hello felhasználói toosign a toothis alkalmazás kerül. Ha hello alkalmazás konfigurált tooperform szolgáltatás szolgáltató által kezdeményezett egyszeri bejelentkezés, majd amikor a felhasználók megnyitják toothis URL-címe, hello szolgáltató hello szükséges átirányítási tooAzure AD tooauthenticate és bejelentkezési hello felhasználó. Ha ez a mező nem üres, az Azure AD az URL-cím toolaunch hello alkalmazás az Office 365 és Azure AD hozzáférési Panel hello fogja használni. Ha kihagyja, akkor az Azure AD helyette végez identitásszolgáltató-amikor hello indításától Office 365, Azure AD hozzáférési Panel hello vagy az Azure AD hello egyetlen kezdeményezett bejelentkezés bejelentkezés URL-CÍMÉT.
* **Azonosító** -ez URI egyedileg kell azonosítania hello alkalmazás, a mely egyszeri bejelentkezés konfigurálása történik. Ez az, hogy a célközönség paraméter hello SAML-jogkivonat hello esetben az Azure AD vissza tooapplication, és hello alkalmazás várt toovalidate hello érték azt. Ez az érték is hello Entitásazonosító bármely hello alkalmazás által biztosított SAML-metaadatokban jelenik meg.
* **Válasz URL-cím** -hello válasz URL-címe, ahol hello alkalmazás vár tooreceive hello SAML-jogkivonat. Ez a hivatkozott tooas hello helyességi feltétel fogyasztói Service (ACS) URL-CÍMÉT is. Után ezek, kattintson a következő képernyőn következő tooproceed toohello. Ezen a képernyőn milyen igények toobe állítja be a hello alkalmazás ügyféloldali tooenable tooaccept egy SAML-jogkivonatot az Azure AD tájékoztatást.
* **Továbbítási állapotot** -hello továbbítási állapota egy nem kötelező paraméter, amely segíthet meg, hogy hello alkalmazás hol tooredirect hello-e felhasználói hitelesítés befejezése után. Általában hello értéke egy érvényes URL-cím: hello alkalmazás, azonban bizonyos alkalmazások használhatják ezt a mezőt eltérően (lásd a hello app egyszeri bejelentkezési dokumentációjában). hello képességét tooset hello továbbítási állapota új funkciója, amely egyedi toohello új Azure-portálon.

### <a name="user-attributes"></a>Felhasználói attribútumok
Ez az adott rendszergazdák megtekinthetik, és jelentkezzen be a szerkesztési hello attribútumok hello SAML-jogkivonat, hogy az Azure AD kibocsát toohello alkalmazás felhasználók.

csak a támogatott szerkeszthető attribútum hello hello **felhasználói azonosító** attribútum. hello Ez az attribútum értéke hello mező az Azure ad-ben, amely egyedileg azonosítja az egyes felhasználók hello alkalmazáson belül. Például ha hello alkalmazás telepítve lett, használja az "E-mail címet" hello hello felhasználónév és egyedi azonosítója, majd hello volna értéke toohello "user.mail" mező az Azure ad-ben.

### <a name="saml-signing-certificate"></a>SAML-aláíró tanúsítványa
Ez a szakasz bemutatja, hogy az Azure AD által használt toosign hello SAML kiadott jogkivonatokat, minden alkalommal hello felhasználó hitelesíti magát toohello alkalmazás hello tanúsítvány hello adatait. Lehetővé teszi a hello tulajdonságainak hello jelenlegi tanúsítvány toobe megvizsgálni, beleértve a hello lejárati dátuma.

### <a name="application-configuration"></a>Alkalmazáskonfiguráció
hello végső rész hello dokumentációját, illetve az vezérlők szükséges tooconfigure hello alkalmazás maga toouse Azure Active Directory identitás-szolgáltatóként.

Hello **alkalmazás konfigurálása** úszó menü hello-alkalmazások konfigurálása az új tömör, beágyazott utasításokat tartalmazza. Ez egy másik új szolgáltatás egyedi toohello új Azure-portálon.

> [!NOTE]
> Beágyazott dokumentációjának átfogó példát lásd: hello Salesforce.com alkalmazás. További alkalmazások dokumentációját folyamatosan bővülő.
> 
> 

![Beágyazott docs][3]

## <a name="password-based-sign-on"></a>Jelszó alapú bejelentkezés
Támogatott hello alkalmazáshoz, ha kijelöli hello jelszóalapú SSO üzemmódban, majd válassza **mentése** azonnal konfigurálja toodo jelszó-alapú egyszeri Bejelentkezést. Egyszeri jelszó alapú központi telepítésével kapcsolatos további információkért lásd: [hogyan egyszeri bejelentkezés az Azure Active Directory munkahelyi](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Jelszó alapú bejelentkezés][4]

## <a name="linked-sign-on"></a>A csatolt bejelentkezési
Ha támogatott hello alkalmazáshoz, kapcsolódó hello SSO mód kiválasztása lehetővé teszi tooenter hello URL-címet, amelyet a hello Azure AD hozzáférési Panel vagy az Office 365 tooredirect toowhen felhasználók kattintson az alkalmazás a. Csatolt SSO (korábbi nevén "meglévő SSO") kapcsolatos további információkért lásd: [hogyan egyszeri bejelentkezés az Azure Active Directory munkahelyi](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Csatolt bejelentkezés][5]

##<a name="feedback"></a>Visszajelzés

Reméljük, például a hello használata jobb, az Azure AD élmény. Ne hamarosan hello visszajelzését! Visszajelzését és ötleteket a fejlesztéséhez fel hello **felügyeleti portál** szakasza a [visszajelzési fórumon](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Ritkán használt adatok új dolgai minden nap kiépítésével foglalkozó még érdeklődőbbek és az útmutatás tooshape felhasználása és határozza meg, mi készíteni.

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG
