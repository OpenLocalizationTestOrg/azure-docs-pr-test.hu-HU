---
title: "Oktatóanyag: Azure Active Directoryval integrált Citrix GoToMeeting |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Citrix GoToMeeting között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 3c6eed5309dfa384c292b0cf63f8aa58988add81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a>Oktatóanyag: Citrix GoToMeeting konfigurálása az automatikus felhasználó létesítése

hello Ez az oktatóanyag célja tooshow meg hello a Citrix GoToMeeting és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooCitrix GoToMeeting tooperform szükséges lépéseket.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő.
*   A Citrix GoToMeeting egyszeri bejelentkezés engedélyezve van az előfizetésben.
*   Egy felhasználói fiókot a Citrix GoToMeeting Team rendszergazdai engedélyekkel.

## <a name="assigning-users-toocitrix-gotomeeting"></a>Felhasználók tooCitrix GoToMeeting hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooyour Citrix GoToMeeting app kell meghatároznia. Ha úgy döntött, hozzárendelheti a felhasználók tooyour Citrix GoToMeeting app itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toocitrix-gotomeeting"></a>Fontos tippek a felhasználók tooCitrix GoToMeeting hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooCitrix GoToMeeting tootest hello konfigurálása kiosztás van hozzárendelve. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooCitrix GoToMeeting rendel, ki kell választania egy érvényes felhasználói szerepkörnek. hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.

## <a name="enable-automated-user-provisioning"></a>Az automatikus felhasználó-kiépítés engedélyezése

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooCitrix GoToMeeting felhasználói fiók kiépítése API és a kiépítés szolgáltatás toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Citrix GoToMeeting alapján a felhasználók és csoportok az Azure AD-hozzárendelés.

> [!TIP]
> Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Citrix GoToMeeting hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatikus felhasználói fiók kiépítése:

1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2. Ha már konfigurált Citrix GoToMeeting egyszeri bejelentkezést, keresse meg a Citrix GoToMeeting hello keresési mező példányát. Máskülönben válassza **Hozzáadás** keresse meg a **Citrix GoToMeeting** hello alkalmazás gyűjteményben. Válassza ki a Citrix GoToMeeting hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.

3. Jelölje ki a Citrix GoToMeeting példányát, majd válassza ki a hello **kiépítési** fülre.

4. Set hello **kiépítési** mód túl**automatikus**. 

    ![Kiépítés](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. A rendszergazdai hitelesítő adataival szakasz hello hajtsa végre a lépéseket követve hello:
   
    a. A hello **Citrix GoToMeeting rendszergazda felhasználóneve** szövegmező, írja be a hello felhasználónevet egy rendszergazda.

    b. A hello **Citrix GoToMeeting rendszergazdai jelszó** szövegmezőhöz hello rendszergazdai jelszót.

6. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak tooyour Citrix GoToMeeting alkalmazást. Ha hello létesített kapcsolat megszakad, győződjön meg arról, a Citrix GoToMeeting fiók Team rendszergazdai jogosultságokkal rendelkezik, majd próbálja hello **"Rendszergazdai hitelesítő adatok"** léptessen ismét.

7. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be hello jelölőnégyzetet.

8. Kattintson a **mentéséhez.**

9. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooCitrix GoToMeeting.**

10. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooCitrix GoToMeeting szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok Citrix GoToMeeting a frissítési műveletekben. Válassza ki a hello Mentés gombra toocommit módosításokat.

11. tooenable hello Azure AD létesítési szolgáltatás a Citrix GoToMeeting, módosítás hello **kiépítési állapot** túl**a** hello beállítások szakaszában a

12. Kattintson a **mentéséhez.**

Hello azokat a felhasználókat a kezdeti szinkronizálását kezdődik, és/vagy csoportok hozzárendelve tooCitrix GoToMeeting hello felhasználók és csoportok szakaszban. hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a Citrix GoToMeeting app a kiépítés végrehajtott műveletekről, amelyeket követve.

## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [Egyszeri bejelentkezés konfigurálása](active-directory-saas-citrix-gotomeeting-tutorial.md)


