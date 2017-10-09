---
title: "az alkalmazások Azure AD SSO aaaConfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan kapcsolódnak az tooself-szolgáltatás a alkalmazások tooAzure Active Directory SAML és a jelszó-alapú egyszeri bejelentkezés használatával"
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: 0d42eb0c-6d3f-4557-9030-e88e86709a19
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.reviewer: luleon
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 002a19a6c7ad25ea2f3b9c6a7c7874ed2be23cce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-single-sign-on-tooapplications-that-are-not-in-hello-azure-active-directory-application-gallery"></a>Egyszeri bejelentkezés tooapplications, amelyek nincsenek hello Azure Active Directory alkalmazáskatalógusában konfigurálása
Ez a cikk egy szolgáltatás, amely lehetővé teszi a rendszergazdák tooconfigure egyszeri bejelentkezés tooapplications nem található meg a hello Azure Active Directory-alkalmazásgyűjtemény tárgya *kód írása nélkül*. Ez a funkció a 2015. November 18. a technical Preview-ban jelent meg, és szerepel a [Azure Active Directory Premium](active-directory-editions.md). Ha ehelyett keresett fejlesztői útmutatás toointegrate egyéni alkalmazások az Azure AD kód, lásd: [hitelesítési forgatókönyvek az Azure AD](active-directory-authentication-scenarios.md).

hello Azure Active Directory alkalmazáskatalógusában felsorolja az alkalmazások, amelyről ismert, toosupport egy formája, amelyet az egyszeri bejelentkezés az Azure Active Directoryval, a [Ez a cikk](active-directory-appssoaccess-whatis.md). (A egy informatikai szakember vagy a rendszer integráló a szervezet) megkeresése hello alkalmazás meg tooconnect, elkezdheti kövesse hello részletes utasításokkal hello Azure felügyeleti portál tooenable egyszeri bejelentkezés állnak rendelkezésre.

Az ügyfelek [Azure Active Directory Premium](active-directory-editions.md) licencek is ezekhez a kiegészítő lehetőségekhez beolvasása:

* Önkiszolgáló integrációs bármely alkalmazás, amely támogatja az SAML 2.0 identitás-szolgáltatóktól (a Szolgáltató által kezdeményezett vagy a kiállító terjesztési hely által kezdeményezett)
* A webes alkalmazás, amelynek használatával egy bejelentkezési lap HTML-alapú önkiszolgáló integrációs [jelszó-alapú egyszeri bejelentkezés](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Kapcsolat az önkiszolgáló felhasználó kialakítási hello SCIM protokollt használó alkalmazások ([itt leírt](active-directory-scim-provisioning.md))
* Képes tooadd hivatkozásait hello tooany alkalmazás [Office 365 alkalmazás indító](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) vagy hello [Azure AD hozzáférési panel](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)

Ez magában foglalhatja nem csak a SaaS-alkalmazások használják, de még nem volt az Azure AD application gallery toohello előre telepített, de a szervezet szabályozhatja, hello felhőalapú vagy helyszíni tooservers központilag telepített külső webes alkalmazásokhoz.

Ezek a képességek, más néven *app integrációs sablonok*, adja meg a szabvány alapú csatlakozási pontok SAML, SCIM vagy űrlapalapú hitelesítést támogató alkalmazások esetében, és rugalmas lehetőségeket és a beállításokat széles körű több alkalmazáshoz is kompatibilisek. 

## <a name="adding-an-unlisted-application"></a>Egy fel nem sorolt alkalmazás hozzáadása
egy alkalmazás integrációs sablonnal, egy alkalmazás tooconnect jelentkezzen be az Azure Active Directory rendszergazdai fiókkal hello Azure felügyeleti portálra, és keresse meg a toohello **Active Directory > [Directory] > alkalmazások**szakaszban jelölje be **Hozzáadás**, majd **alkalmazás hozzáadása hello gyűjteményből**. 

![][1]

Hello alkalmazásgyűjtemény, hozzáadhat egy fel nem sorolt alkalmazáshoz hello **egyéni** hello bal oldali vagy hello kiválasztásával kategóriába **fel nem sorolt alkalmazás hozzáadása** hivatkozás található hello keresési eredmények, ha a kívánt alkalmazást nem található. Az alkalmazás egy név beírása után hello egyszeri bejelentkezésre vonatkozó beállításokat és viselkedése is konfigurálhatja. 

**Egyszerű ötlet**: hello keresési funkció toocheck toosee ajánlott eljárásként használja, ha hello alkalmazáskatalógusában hello alkalmazás már létezik. Ha hello app található, és annak leírását említi "egyszeri bejelentkezés", majd hello alkalmazás már támogatja az összevont egyszeri bejelentkezést. 

![][2]

Egy alkalmazás hozzáadása így biztosít egy nagyon hasonló élményt toohello egy előre integrált alkalmazások érhető el. Jelölje be toostart **konfigurálása egyszeri bejelentkezéshez**. hello következő képernyőn hello konfigurálása egyszeri bejelentkezéshez, három lehetséges a következő hello a következő részekben ismertetett mutatja be.

![][3]

## <a name="azure-ad-single-sign-on"></a>Az Azure AD-egyszeri bejelentkezés
Ez a beállítás tooconfigure hello alkalmazáshoz a SAML-alapú hitelesítés kiválasztása Ehhez szükséges, amely hello alkalmazástámogatási SAML 2.0, és információkat gyűjtenek a hogyan toouse hello SAML képességek hello alkalmazás folytatása előtt. Kiválasztása után **következő**, rákérdezéses tooenter három különböző URL-címek toohello SAML végpontok hello alkalmazáshoz megfelelő lesz. 

![][4]

Ezek a következők:

* **Bejelentkezés az URL-címe (Szolgáltató által kezdeményezett csak)** – amennyiben hello felhasználói toosign a toothis alkalmazás kerül. Ha hello alkalmazás van konfigurálva tooperform szolgáltatás szolgáltató által kezdeményezett egyszeri bejelentkezés, majd amikor a felhasználók megnyitják toothis URL-címe, hello szolgáltató lesz hello szükséges átirányítást tooAzure AD tooauthenticate, és jelentkezzen be a hello felhasználó. Ha ez a mező nem üres, az Azure AD az URL-cím toolaunch hello alkalmazás az Office 365 és Azure AD hozzáférési Panel hello fogja használni. Ha ez a mező ommited, akkor az Azure AD ehelyett hajtsa végre az identitásszolgáltató-amikor hello indításától Office 365, Azure AD hozzáférési Panel hello vagy az Azure AD hello egyetlen kezdeményezett bejelentkezés bejelentkezés URL-címe (copiable hello irányítópult lapján).
* **Kiállítói URL-címe** -hello kibocsátó URL-cím egyedileg kell azonosítania hello alkalmazás, a mely egyszeri bejelentkezés konfigurálása történik. Ez az, hogy az Azure AD vissza tooapplication küldi hello, hello érték **célközönség** hello SAML-jogkivonat és hello alkalmazás paraméter várt toovalidate azt. Ez az érték jelenik meg is hello **Entitásazonosító** bármely hello alkalmazás által biztosított SAML-metaadatokban. Hello alkalmazás SAML dokumentációjában talál részletes információt mi Entitásazonosító, vagy a célközönség érték. Alább látható egy példa hogyan hello célközönség URL-cím hello SAML-jogkivonat visszaadott toohello alkalmazás jelenik meg:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Válasz URL-cím** -hello válasz URL-címe, ahol hello alkalmazás vár tooreceive hello SAML-jogkivonat. Ez egyben hivatkozott tooas hello **helyességi feltétel fogyasztói Service (ACS) URL-cím**. Ellenőrizze a hello alkalmazás SAML dokumentációjában talál részletes információt az SAML-jogkivonat válasz URL-CÍMEN és ACS URL-címe van.
  Miután ezek megadott, kattintson a **következő** tooproceed toohello következő képernyőn. Ezen a képernyőn milyen igények toobe állítja be a hello alkalmazás ügyféloldali tooenable tooaccept egy SAML-jogkivonatot az Azure AD tájékoztatást. 

![][5]

Mely értékei szükséges hello alkalmazástól függően változnak, ezért ellenőrizze az alkalmazás hello SAML dokumentációjában. Hello **bejelentkezés** és **kijelentkezési** szolgáltatás URL-címe mindkét megoldásához toohello azonos végpontot, amely hello SAML kéréskezeléshez végpont esetében az Azure AD példányával. hello kiállítójának URL-címe hello érték hello "Kiállító" hello SAML-jogkivonat kiállított toohello alkalmazás belül jelenik meg. 

Az alkalmazás konfigurálása után kattintson **következő** gombra, majd hello **Complete** tooclose hello párbeszédpanel. 

## <a name="assigning-users-and-groups-tooyour-saml-application"></a>Felhasználók és csoportok tooyour SAML alkalmazás hozzárendelése
Miután az alkalmazás már konfigurált toouse az Azure AD egy SAML-alapú identitás-szolgáltatóként, majd már majdnem kész tootest. Biztonsági ellenőrzés, mint az Azure AD engedélyezi azok toosign hello alkalmazásba kivéve kaptak az Azure AD hozzáférési jogkivonat nem állítanak ki. Felhasználók adható hozzáférés közvetlenül, vagy egy csoportot, amely tagja. 

egy felhasználó vagy csoport tooyour alkalmazás tooassign kattintson hello **felhasználók hozzárendelése** gombra. Válassza ki a hello felhasználó vagy csoport tooassign kívánja, és válassza a hello **hozzárendelése** gombra. 

![][6]

Egy felhasználói hozzárendelése lehetővé teszi az Azure AD tooissue hello felhasználó, valamint az alkalmazás tooappear mozaikokra hello felhasználói hozzáférési panel, amely egy token. Egy alkalmazás csempe is megjelenik a hello Office 365 alkalmazásindító, ha hello felhasználó által használt Office 365. 

Az alkalmazások hello hello csempe emblémát tölthet **embléma feltöltéséhez** hello gombjára **konfigurálása** hello alkalmazás lapon. 

### <a name="customizing-hello-claims-issued-in-hello-saml-token"></a>Hello SAML-jogkivonat kiállított hello jogcímek testreszabása
Amikor a felhasználók hitelesítése toohello alkalmazás, az Azure AD egy SAML token toohello alkalmazást, amely tartalmazza az adatokat (vagy jogcímeket) állít ki, amely egyedileg azonosítja hello felhasználóról. Alapértelmezés szerint ez magában foglalja a hello felhasználói felhasználónév, e-mail címét, Utónév és Vezetéknév. 

Megtekintheti vagy hello hello SAML-jogkivonat toohello alkalmazást küldi hello jogcímek szerkesztése **attribútumok** fülre. 

![][7]

Miért szükség lehet a hello SAML-jogkivonat kiadott tooedit hello jogcímeket két lehetséges oka lehet: •hello alkalmazás írása toorequire egy másik jogcím URI-azonosítók beállítása vagy a jogcím-értékek •Your alkalmazás telepítve van a hello igényel NameIdentifier jogcím toobe értéktől eltérő hello felhasználónév (AKA egyszerű felhasználónév) az Azure Active Directoryban tárolja. 

Hogyan tooadd és Szerkesztés jogcím-forgatókönyvek esetén információkért tekintse meg a [jogcímek testreszabása foglalkozó](active-directory-saml-claims-customization.md). 

### <a name="testing-hello-saml-application"></a>Hello SAML-alkalmazás tesztelése
Hello SAML-alapú URL-címek és a tanúsítvány konfigurálása az Azure AD- és hello alkalmazásban felhasználókhoz vagy csoportokhoz van rendelve toohello alkalmazás az Azure és hello jogcímek felülvizsgálata és szerkeszthető, ha szükséges, majd a hello felhasználó kész toosign hello be az alkalmazás. 

tootest, egyszerűen csak jelentkezzen be hello Azure AD hozzáférési panel a https://myapps.microsoft.com toohello alkalmazás hozzárendelt felhasználói fiókkal, és kattintson hello mozaikokon hello alkalmazás tookick hello bejelentkezési folyamat egyetlen ki. Alternatív megoldásként tallózással közvetlenül toohello bejelentkezési URL-cím hello alkalmazás és a bejelentkezés onnan. 

Hibakeresési tippeket, megjelenik ez [cikkünket toodebug SAML-alapú egyszeri bejelentkezés tooapplications](active-directory-saml-debugging.md) 

## <a name="password-single-sign-on"></a>Jelszó egyszeri bejelentkezést.
Válassza ezt a beállítást tooconfigure [jelszó-alapú egyszeri bejelentkezést](active-directory-appssoaccess-whatis.md) , amely rendelkezik egy bejelentkezési lap HTML webalkalmazás. Egyszeri jelszó alapú is vaulting, hivatkozott tooas jelszó toomanage felhasználói hozzáférés és a jelszavak tooweb alkalmazások, amelyek nem támogatják az identitás-összevonási lehetővé teszi. Akkor célszerű is helyzetekben, amikor több felhasználónak kell tooshare ugyanazt a fiókot, például a szervezete tooyour közösségi app fiókok. 

Kiválasztása után **következő**, hello alkalmazás webalapú bejelentkezési oldal felszólító tooenter hello URL-cím lesz. Vegye figyelembe, hogy hello felhasználónév és jelszó bemeneti mezőket tartalmazó hello oldalon kell lennie. Többször megadott, az Azure AD egy folyamat tooparse hello bejelentkezési oldal az a felhasználónév megadásához és a jelszavak beírására kezdődik. Ha hello folyamat sikertelen, majd azt végigvezeti Önt egy másik folyamat telepíthet egy bővítmény (igényli az Internet Explorer, a Chrome vagy a Firefox), amely lehetővé teszi a toomanually rögzítési hello mezőket.

Amennyiben hello bejelentkezési oldal rögzítése, felhasználók és csoportok rendelt és credential házirendeket állíthat be hasonlóan rendszeres [jelszó SSO alkalmazások](active-directory-appssoaccess-whatis.md).

Megjegyzés: Az alkalmazások hello hello csempe emblémát tölthet **embléma feltöltéséhez** hello gombjára **konfigurálása** hello alkalmazás lapon. 

## <a name="existing-single-sign-on"></a>Meglévő egyszeri bejelentkezést.
Válassza ezt a beállítást tooadd egy hivatkozás tooan alkalmazás tooyour szervezet Azure AD hozzáférési Panel vagy az Office 365 portálra. Azure AD hitelesítésében helyett a tooadd hivatkozások toocustom webes alkalmazásokat, amelyek jelenleg használják az Azure Active Directory összevonási szolgáltatások (vagy más összevonási szolgáltatásból) is használhatja. Vagy mélyhivatkozással toospecific SharePoint-lapokat vagy más, hogy csak szeretné tooappear a felhasználói hozzáférés panelek a weblapok is hozzáadhat. 

Kiválasztása után **következő**, rákérdezéses tooenter hello URL-CÍMÉT a hello alkalmazás toolink lesz. Ezt követően felhasználókhoz és csoportokhoz rendelt toohello alkalmazásról, így hello alkalmazás tooappear hatására a hello [Office 365 alkalmazás indító](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) vagy hello [Azure AD hozzáférési panel](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) azoknak a felhasználóknak.

Megjegyzés: Az alkalmazások hello hello csempe emblémát tölthet **embléma feltöltéséhez** hello gombjára **konfigurálása** hello alkalmazás lapon.

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Hogyan tooCustomize jogcím a kiállított hello SAML-jogkivonat Pre-Integrated alkalmazások](active-directory-saml-claims-customization.md)
* [Hibaelhárítási SAML-alapú egyszeri bejelentkezést.](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png
