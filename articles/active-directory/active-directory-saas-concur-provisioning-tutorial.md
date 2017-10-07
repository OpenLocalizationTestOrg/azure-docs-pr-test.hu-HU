---
title: "Oktatóanyag: Azure Active Directoryval integrált Concur |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Concur között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: df47f55f-a894-4e01-a82e-0dbf55fc8af1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 13ba364af26a5ce0f1d2b51aaa0f84a4c353b107
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-concur-for-user-provisioning"></a>Oktatóanyag: A felhasználók átadása konfigurálása egyetért

hello Ez az oktatóanyag célja tooshow meg hello tooperform Concur és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooConcur a szükséges lépéseket.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő.
*   Egy Concur egyszeri bejelentkezés engedélyezve van az előfizetés.
*   Egy felhasználói fiókot az Concur Team rendszergazdai engedélyekkel.

## <a name="assigning-users-tooconcur"></a>Felhasználók tooConcur hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour Concur alkalmazást kell használni az Azure AD jelentik hello felhasználók. Ha úgy döntött, hozzárendelheti a felhasználók tooyour Concur app itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-tooconcur"></a>Fontos tippek a felhasználók tooConcur hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooConcur tootest hello konfigurálása kiosztás rendelni. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooConcur rendel, ki kell választania egy érvényes felhasználói szerepkörnek. hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.

## <a name="enable-user-provisioning"></a>Felhasználó-kiépítés engedélyezése

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooConcur felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Concur alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.

> [!Tip] 
> Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Concur hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.

### <a name="tooconfigure-user-account-provisioning"></a>tooconfigure felhasználói fiók kiépítése:

hello ebben a szakaszban célja toooutline hogyan tooConcur tooenable kiépítése Active Directory felhasználói fiókok.

tooenable alkalmazások hello költség szolgáltatás, nem rendelkezik a toobe megfelelő telepítő és a webes szolgáltatás-rendszergazdák profil használatát. Ne adjon hello WS rendszergazdai szerepkör tooyour meglévő rendszergazda profillal, amelyekkel a T & E felügyeleti funkcióihoz.

Tanácsadók egyetért vagy hello ügyfél rendszergazdának létre kell hoznia egy különálló webes szolgáltatás-rendszergazda profil és hello ügyfél rendszergazda ezt a profilt kell használnia hello Web Services rendszergazdai feladatokat (például, amely lehetővé teszi alkalmazások). Ezeket a profilokat el kell különíteni a hello ügyfél rendszergazda napi T & E felügyeleti profil (hello T & E felügyeleti profil nem lehetnek hello WSAdmin szerepkörrel).

Hello-profil toobe hello alkalmazás engedélyezésének használt létrehozásakor adja meg a hello ügyfél rendszergazda nevét hello felhasználói profil mezőkbe. Ez a tulajdonosi toohello profil rendeli hozzá. Egy vagy több profilok létrehozása után hello ügyfél jelentkezzen be a profil tooclick hello "*engedélyezése*" gombra egy Partner alkalmazás hello webszolgáltatások menü belül.

A következő okok miatt hello Ez a művelet nem használ a normál T & E felügyeleti hello-profillal kell végrehajtani.

* hello ügyfél rendelkezik egy, a kattintásokról hello toobe "*Igen*" hello párbeszéd ablak akkor jelenik meg, egy alkalmazás engedélyezése után. Kattintson a hello ügyfél van hajlandó hello Partner alkalmazás tooaccess az adataikat, akkor vagy hello Partner nem kattintson, amely az Igen gombra, elfogadja.

* Ha egy ügyfél hello T & E felügyeleti profilt használó alkalmazások engedélyező kilép hello vállalati (ami alatt inaktivált hello-profil), a profilt használó engedélyezett alkalmazások nem működik hello alkalmazás egy másik aktív WS Admin engedélyezéséig profil. Ezért toocreate különböző WS-felügyeleti profilok kellett volna lennie.

* Ha egy rendszergazda hello vállalati kilép, hello neve társított WS-felügyeleti profil lehet módosított toohello helyettesítő rendszergazda, ha engedélyezett hello befolyásolása nélkül kívánt alkalmazást, mert a profilt nem kell inaktivált toohello.

**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**

1. Jelentkezzen be tooyour **Concur** bérlő.

2. A hello **felügyeleti** menü **webszolgáltatások**.
   
    ![Concur bérlői](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur bérlői")

3. A bal oldalon, a hello hello **webszolgáltatások** ablaktáblán válassza előbb **Partner alkalmazás engedélyezése**.
   
    ![Partner alkalmazás engedélyezése](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "Partner alkalmazás engedélyezése")

4. A hello **alkalmazás engedélyezése** listáról válassza ki **Azure Active Directory**, és kattintson a **engedélyezése**.
   
    ![A Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directoryban")

5. Kattintson a **Igen** tooclose hello **a művelet megerősítéséhez** párbeszédpanel.
   
    ![Erősítse meg a műveletet](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "erősítse meg a műveletet")

6. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

7. Ha már konfigurált Concur egyszeri bejelentkezést, keresse meg a hello keresési mező Concur példányát. Máskülönben válassza **Hozzáadás** keresse meg a **Concur** hello alkalmazás gyűjteményben. Válassza ki a Concur hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.

8. Jelölje ki a Concur példányát, majd jelölje ki a hello **kiépítési** fülre.

9. Set hello **kiépítési üzemmódban** túl**automatikus**. 
 
    ![Kiépítés](./media/active-directory-saas-concur-provisioning-tutorial/provisioning.png)

10. A hello **rendszergazdai hitelesítő adataival** területen adja meg a hello **felhasználónév** és hello **jelszó** Concur rendszergazdától.

11. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour Concur alkalmazás képes kapcsolódni. Ha hello létesített kapcsolat megszakad, győződjön meg arról, Concur fiókja Team rendszergazdai jogosultságokkal rendelkezik.

12. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be hello jelölőnégyzetet.

13. Kattintson a **mentéséhez.**

14. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooConcur.**

15. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooConcur szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok Concur a frissítési műveletekben. Válassza ki a hello Mentés gombra toocommit módosításokat.

16. tooenable hello Azure AD Concur, módosítás hello a létesítési szolgáltatás **kiépítési állapot** túl**a** a hello **beállítások** szakasz

17. Kattintson a **mentéséhez.**

Mostantól létrehozhat egy olyan fiókot. Várjon, amíg fel hello fiókot töltött tooverify szinkronizált tooConcur too20 perc.

## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [Egyszeri bejelentkezés konfigurálása](active-directory-saas-concur-tutorial.md)

