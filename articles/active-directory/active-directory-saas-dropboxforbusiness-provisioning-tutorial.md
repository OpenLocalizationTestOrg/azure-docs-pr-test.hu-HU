---
title: "Oktatóanyag: Azure Active Directoryval integrált vállalati Dropbox |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a vállalati Dropbox között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 0fb01eab4f7c6c4516eac64a4343e46ea221f98d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a>Oktatóanyag: Üzleti Dropbox konfigurálása automatikus felhasználói történő üzembe helyezéséhez

hello Ez az oktatóanyag célja meg van szüksége a Dropbox tooperform üzleti és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooDropbox vállalati lépéseket hello tooshow.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő.
*   A Dropbox az üzleti egyszeri bejelentkezés engedélyezve van az előfizetésben.
*   Egy felhasználói fiókot az üzleti Dropbox Team rendszergazdai engedélyekkel.

## <a name="assigning-users-toodropbox-for-business"></a>A vállalati felhasználók tooDropbox hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooyour Dropbox alkalmazás kell meghatároznia. Ha úgy döntött, itt hello utasításokat követve rendelhet a felhasználók tooyour Dropbox alkalmazás:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toodropbox-for-business"></a>Fontos tippek az üzleti felhasználók tooDropbox hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó hozzá van rendelve az üzleti tootest hello konfigurálása kiosztás tooDropbox. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooDropbox rendel a vállalati, ki kell választania egy érvényes felhasználói szerepkörnek. hello "alapértelmezett" szerepkör nem működik a kiépítés...

## <a name="enable-automated-user-provisioning"></a>Az automatikus felhasználó-kiépítés engedélyezése

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooDropbox vállalat felhasználói fiók kiépítése API és a kiépítés szolgáltatás toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Dropbox vállalati felhasználók és csoportok alapján az Azure AD-hozzárendelés.

>[!Tip]
>Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a vállalati hello megjelenő utasításokat követve Dropbox [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatikus felhasználói fiók kiépítése:

1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2. Ha már konfigurálta a vállalati Dropbox az egyszeri bejelentkezést, keresse meg az üzleti hello keresési mező Dropbox-példány. Máskülönben válassza **Hozzáadás** keresse meg a **Dropbox vállalati** hello alkalmazás gyűjteményben. Válassza ki a vállalati Dropbox hello a keresési eredmények, és vegye fel tooyour alkalmazások listáját.

3. Jelölje ki a Dropbox vállalati példányát, majd jelölje ki a hello **kiépítési** fülre.

4. Set hello **kiépítési üzemmódban** túl**automatikus**. 

    ![Kiépítés](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. A hello **rendszergazdai hitelesítő adataival** kattintson **engedélyezés**. A Dropbox bejelentkezési párbeszédpanel üzleti egy új böngészőablakban nyílik meg.

6. A hello **bejelentkezési tooDropbox toolink az Azure ad-val** párbeszédpanelen tooyour Dropbox üzleti bérlő bejelentkezés.

     ![A felhasználók átadása](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "a felhasználók átadása")

7. Győződjön meg arról, hogy szeretné-e toogive Azure Active Directory engedély toomake módosítások tooyour Dropbox üzleti bérlő számára. Kattintson a **engedélyezése**.
    
      ![A felhasználók átadása](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "a felhasználók átadása")

8. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD alkalmazás Dropbox tooyour kapcsolódhatnak. Hello létesített kapcsolat megszakad, ha a vállalati fiók Team rendszergazdai jogosultságokkal rendelkezik, győződjön meg arról, hogy a dropbox-ba, és próbálkozzon hello **"Engedélyezés"** léptessen ismét.

9. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be hello jelölőnégyzetet.

10. Kattintson a **mentéséhez.**

11. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooDropbox vállalati.**

12. A hello **attribútum-leképezésekhez** szakaszban, tekintse át a vállalati szinkronizált az Azure AD tooDropbox hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságainak használt toomatch hello felhasználói fiókok a Dropbox vállalati a frissítési műveletek. Válassza ki a hello Mentés gombra toocommit módosításokat.

13. tooenable hello Azure AD létesítési szolgáltatás a Dropbox vállalati, a módosítás hello **kiépítési állapot** túl**a** hello beállítások szakaszában a

14. Kattintson a **mentéséhez.**

Hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok hello felhasználók a vállalati tooDropbox és a csoportok szakasz kezdődik. hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hajtsa végre a hivatkozások tooprovisioning Tevékenységjelentések, amely alkalmazás a Dropbox a szolgáltatás kiépítését hello által végzett összes műveletet írják le.

Mostantól létrehozhat egy olyan fiókot. Várjon, amíg fel hello fiókot töltött tooverify tooDropbox szinkronizálása a vállalati too20 perc.

A sikeresen befejezett felhasználók átadásához ciklus kapcsolódó állapotát jelzi.

![Felhasználók hozzárendelése](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "felhasználók hozzárendelése")


## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [Egyszeri bejelentkezés konfigurálása](active-directory-saas-dropboxforbusiness-tutorial.md)