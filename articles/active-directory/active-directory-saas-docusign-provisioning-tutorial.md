---
title: "Oktatóanyag: Azure Active Directoryval integrált DocuSign |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és DocuSign között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 294cd6b8-74d7-44bc-92bc-020ccd13ff12
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 8562a8f9e05fb72d3331507b7da5c6afee38f9b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-docusign-for-user-provisioning"></a>Oktatóanyag: A felhasználók átadása DocuSign konfigurálása

hello Ez az oktatóanyag célja tooshow meg hello tooperform DocuSign és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooDocuSign a szükséges lépéseket.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő.
*   Egy DocuSign egyszeri bejelentkezés engedélyezve van az előfizetés.
*   Egy felhasználói fiókot az DocuSign Team rendszergazdai engedélyekkel.

## <a name="assigning-users-toodocusign"></a>Felhasználók tooDocuSign hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában, csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás az Azure AD szinkronizálva van.

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour DocuSign alkalmazást kell használni az Azure AD jelentik hello felhasználók. Ha úgy döntött, hozzárendelheti a felhasználók tooyour DocuSign app itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toodocusign"></a>Fontos tippek a felhasználók tooDocuSign hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooDocuSign tootest hello konfigurálása kiosztás van hozzárendelve. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooDocuSign rendel, ki kell választania egy érvényes felhasználói szerepkörnek. hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.

## <a name="enable-user-provisioning"></a>Felhasználó-kiépítés engedélyezése

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooDocuSign felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a DocuSign alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.

> [!Tip]
> Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a DocuSign hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.

### <a name="tooconfigure-user-account-provisioning"></a>tooconfigure felhasználói fiók kiépítése:

hello ebben a szakaszban célja toooutline hogyan tooDocuSign tooenable a felhasználók átadása az Active Directory felhasználói fiókok.

1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2. Ha már konfigurált DocuSign egyszeri bejelentkezést, keresse meg a hello keresési mező DocuSign példányát. Máskülönben válassza **Hozzáadás** keresse meg a **DocuSign** hello alkalmazás gyűjteményben. Válassza ki a DocuSign hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.

3. Jelölje ki a DocuSign példányát, majd jelölje ki a hello **kiépítési** fülre.

4. Set hello **kiépítési üzemmódban** túl**automatikus**. 

    ![Kiépítés](./media/active-directory-saas-docusign-provisioning-tutorial/provisioning.png)

5. A hello **rendszergazdai hitelesítő adataival** területen adja meg a következő konfigurációs beállítások hello:
   
    a. A hello **rendszergazda felhasználóneve** szövegmezőhöz típus egy DocuSign fióknév rendelkezik hello **rendszergazda** DocuSign.com rendelt profillal.
   
    b. A hello **rendszergazdai jelszó** szövegmezőhöz típus hello fiókhoz tartozó jelszót.

6. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour DocuSign alkalmazás képes kapcsolódni.

7. A hello **értesítő e-mailt** mezőbe írja be a hello e-mail címet vagy egy csoportot ki kell létesítési hiba értesítéseket, és jelölje be hello jelölőnégyzetet.

8. Kattintson a **mentéséhez.**

9. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooDocuSign.**

10. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooDocuSign szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok DocuSign a frissítési műveletekben. Válassza ki a hello Mentés gombra toocommit módosításokat.

11. tooenable hello Azure AD DocuSign, módosítás hello a létesítési szolgáltatás **kiépítési állapot** túl**a** hello beállítások szakaszában a

12. Kattintson a **mentéséhez.**

Hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooDocuSign a hello felhasználók és csoportok szakasz kezdődik. hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és kövesse az DocuSign alkalmazásnak szolgáltatás kiépítését hello által végzett összes műveletet írják le hivatkozások tooprovisioning tevékenység jelentéseket.

Mostantól létrehozhat egy olyan fiókot. Várjon, amíg fel hello fiókot töltött tooverify szinkronizált tooDocuSign too20 perc.

## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [Egyszeri bejelentkezés konfigurálása](active-directory-saas-docusign-tutorial.md)