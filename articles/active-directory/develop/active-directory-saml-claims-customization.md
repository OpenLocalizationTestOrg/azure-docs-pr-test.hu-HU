---
title: "a SAML-jogkivonat hello előre integrált alkalmazások az Azure Active Directoryban kiadott aaaCustomizing jogcímeket |} Microsoft Docs"
description: "Megtudhatja, miként toocustomize hello adja ki a hello SAML-jogkivonat előre integrált alkalmazások az Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: a376318929472403e799f02fdd3fbddc91d0a70c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customizing-claims-issued-in-hello-saml-token-for-pre-integrated-apps-in-azure-active-directory"></a>A hello kiállított jogcímek testreszabása SAML-jogkivonat előre integrált alkalmazások az Azure Active Directoryban
Ma Azure Active Directory támogatja a több ezer előre integrált alkalmazások az Azure AD Application Gallery, beleértve a több mint, amely támogatja az egyszeri bejelentkezés 360 hello hello SAML 2.0 protokoll használatával. Amikor a felhasználók hitelesítése tooan alkalmazást, amely SAML NYELVET használ az Azure AD keresztül, az Azure AD küld egy token toohello alkalmazás (HTTP POST) keresztül. És ezt követően a hello alkalmazás ellenőrzi, és használja a hello token toolog hello felhasználó nem kér felhasználónevet és jelszót. Ezeket a SAML-jogkivonatokat "jogcímek" néven ismert hello felhasználóval kapcsolatos információt tartalmazza.

Az identitás-beszéd, a "jogcímek" információ arról, hogy az identitásszolgáltató belül azok számára, hogy a felhasználó kiadják hello jogkivonat a felhasználó. A [SAML-jogkivonat](http://en.wikipedia.org/wiki/SAML_2.0), az adatokat általában hello SAML Attribute utasítást tartalmaz. hello a felhasználó egyedi azonosítója általában a jelzi hello SAML tulajdonos más néven azonosítója.

Alapértelmezés szerint az Azure Active Directory NameIdentifier jogcímet, hello felhasználónév (AKA egyszerű felhasználónév) az Azure AD értéket tartalmazó SAML-jogkivonat tooyour alkalmazás ad ki. Ez az érték egyedi azonosítására alkalmas hello felhasználó. hello SAML-jogkivonat a hello a felhasználó e-mail címét, Utónév és Vezetéknév tartalmazó további eszközjogcímeket is tartalmazza.

tooview vagy Szerkesztés hello jogcímek által kiadott hello SAML token toohello alkalmazás, nyissa meg hello alkalmazás Azure-portálon. Válassza ki hello **nézet és egyéb felhasználói attribútumok szerkesztése** hello jelölőnégyzet **felhasználói attribútumok** hello alkalmazás szakasza.

![Felhasználói attribútumok szakasz][1]

Nincsenek miért szükség lehet a hello SAML-jogkivonat kiadott tooedit hello jogcímeket két lehetséges okok:
* hello alkalmazás egy másik jogcím URI-azonosítók beállítása vagy jogcímértékek toorequire van írva.
* hello alkalmazás telepítve van, hogy valami eltérő hello felhasználónév (AKA egyszerű felhasználónév) Azure Active Directoryban tárolt hello NameIdentifier jogcím toobe igényel.

Hello alapértelmezett jogcímértékek tudja szerkeszteni. Válassza ki a hello jogcím sor hello SAML-jogkivonat attribútumok táblában. Ekkor megnyílik a hello **attribútum szerkesztése** jogcím neve, és névtérrel hello jogcím társított szerkesztheti szakaszban, majd meg.

![Felhasználói attribútum szerkesztése][2]

Is eltávolíthatja a jogcímek (eltérő NameIdentifier) hello kattintva megnyílik hello helyi menü **...**  ikonra.  Azt is megteheti hello új jogcímek **Hozzáadás attribútum** gombra.

![Felhasználói attribútum szerkesztése][3]

## <a name="editing-hello-nameidentifier-claim"></a>Hello NameIdentifier jogcímszabályok szerkesztése
ahol hello alkalmazás telepítve van a toosolve hello probléma segítségével más felhasználónevet, kattintson a hello **felhasználói azonosító** hello legördülő **felhasználói attribútumok** szakasz. Ez a művelet több, különböző beállításokkal párbeszédpanel biztosítja:

![Felhasználói attribútum szerkesztése][4]

Hello legördülő listán, válassza ki **user.mail** tooset hello NameIdentifier jogcím hello könyvtárban toobe hello felhasználó e-mail címét. Vagy válassza ki **user.onpremisessamaccountname** tooset toohello felhasználói SAM-neve, amely a szinkronizálása megtörtént a helyszíni Azure AD.

Is használhatja a speciális hello **ExtractMailPrefix()** függvény tooremove hello tartományutótagot hello e-mail címet, a SAM-neve vagy a hello egyszerű felhasználónév. Ez csak az első része hello hello felhasználói név átadta-e keresztül bontja ki (például "joe_smith" helyett joe_smith@contoso.com).

![Felhasználói attribútum szerkesztése][5]

Most is jelentek meg hello **join()** függvény toojoin hello ellenőrzött tartományához hello felhasználói azonosító érték. Ha kiválaszt hello join() függvény hello **felhasználói azonosító** először jelölje ki e-mail cím vagy a felhasználó egyszerű neve hasonló felhasználói azonosító hello és hello második legördülő listán válassza az ellenőrzött tartomány. Ha hello címre küldi hello ellenőrzött tartomány választja, akkor az Azure AD hello felhasználónév kiolvassa a hello első érték joe_smith a joe_smith@contoso.com és a contoso.onmicrosoft.com fűzi azokat. Tekintse meg a következő példa hello:

![Felhasználói attribútum szerkesztése][6]

## <a name="adding-claims"></a>Jogcím hozzáadása
Ha egy jogcímet ad hozzá, hello attribútumnév (igénylő nem feltétlenül toofollow hello SAML-specifikáció szerint URI mintát) is megadhat. Állítsa be a hello érték tooany felhasználói attribútum, amely hello könyvtárban tárolja.

![Felhasználói attribútum hozzáadása][7]

Például, hogy kell toosend felhasználói hello hello részleg tartozik tooin (mint például a Sales) jogcímszabályként szervezetek. Adja meg a hello jogcím neve hello alkalmazás által várt, és válassza **felhasználó.részleg** hello értékként.

> [!NOTE]
> Ha a megadott felhasználó nincs a kiválasztott attribútumnak tárolt érték, majd az igény nem alatt kiadott hello jogkivonat.

> [!TIP]
> Hello **user.onpremisesecurityidentifier** és **user.onpremisesamaccountname** csak akkor támogatott, ha a felhasználói adatok szinkronizálása a helyszíni Active Directory címtárhoz hello [Azure AD Connect eszköz](../active-directory-aadconnect.md).

## <a name="restricted-claims"></a>Korlátozott jogcímek

SAML néhány korlátozott jogcímek szerepelnek. Ha ezeket a jogcímeket, majd az Azure AD nem küld ezeket a jogcímeket. Hello SAML-alapú korlátozott jogcímek készletébe a következők:

    | Jogcím típusa (URI) |
    | ------------------- |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/Expiration |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/Expired |
    | http://schemas.microsoft.com/Identity/Claims/accesstoken |
    | http://schemas.microsoft.com/Identity/Claims/openid2_id |
    | http://schemas.microsoft.com/Identity/Claims/identityprovider |
    | http://schemas.microsoft.com/Identity/Claims/objectidentifier |
    | http://schemas.microsoft.com/Identity/Claims/PUID |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/NameIdentifier [MR1] |
    | http://schemas.microsoft.com/Identity/Claims/tenantid |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/AuthenticationInstant |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/AuthenticationMethod |
    | http://schemas.microsoft.com/accesscontrolservice/2010/07/Claims/identityprovider |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/groups |
    | http://schemas.microsoft.com/Claims/groups.Link |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/Role |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/wids |
    | http://schemas.microsoft.com/2014/09/devicecontext/Claims/iscompliant |
    | http://schemas.microsoft.com/2014/02/devicecontext/Claims/isknown |
    | http://schemas.microsoft.com/2012/01/devicecontext/Claims/ismanaged |
    | http://schemas.microsoft.com/2014/03/psso |
    | http://schemas.microsoft.com/Claims/authnmethodsreferences |
    | http://schemas.xmlsoap.org/ws/2009/09/Identity/Claims/Actor |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/samlissuername |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/confirmationkey |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsaccountname |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/primarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/primarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/authorizationdecision |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/Authentication |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/SID |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/denyonlyprimarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/denyonlyprimarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/denyonlysid |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/denyonlywindowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsdeviceclaim |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsfqbnversion |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowssubauthority |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsuserclaim |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/x500distinguishedname |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/UPN |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/GroupSID |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/SPN |
    | http://schemas.microsoft.com/ws/2008/06/Identity/Claims/ispersistent |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/privatepersonalidentifier |
    | http://schemas.microsoft.com/Identity/Claims/scope |

## <a name="next-steps"></a>Következő lépések
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](../active-directory-apps-index.md)
* [Egyszeri bejelentkezés tooapplications, amelyek nincsenek hello Azure Active Directory alkalmazáskatalógusában konfigurálása](../active-directory-saas-custom-apps.md)
* [Hibaelhárítási SAML-alapú egyszeri bejelentkezést.](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png