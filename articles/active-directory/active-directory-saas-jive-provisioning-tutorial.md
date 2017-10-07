---
title: "Oktatóanyag: Azure Active Directoryval integrált Jive |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Jive között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: b1c0d0bc2d79427c055f577fe5f9d30d10f1bbdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-jive-for-user-provisioning"></a>Oktatóanyag: A felhasználók átadása Jive konfigurálása

hello Ez az oktatóanyag célja tooshow meg hello tooperform Jive és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooJive a szükséges lépéseket.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő.
*   Egy Jive egyszeri bejelentkezés engedélyezve van az előfizetésben.
*   Egy felhasználói fiókot az Jive Team rendszergazdai engedélyekkel.

## <a name="assigning-users-toojive"></a>Felhasználók tooJive hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooyour Jive app kell meghatároznia. Ha úgy döntött, hozzárendelheti a felhasználók tooyour Jive app itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toojive"></a>Fontos tippek a felhasználók tooJive hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooJive tootest hello konfigurálása kiosztás rendelni. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooJive rendel, ki kell választania egy érvényes felhasználói szerepkörnek. hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.

## <a name="enable-user-provisioning"></a>Felhasználó-kiépítés engedélyezése

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooJive felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Jive alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.

> [!TIP]
> Dönthet úgy is SAML-alapú egyszeri bejelentkezés az Jive hello megjelenő utasításokat követve tooenabled [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.

### <a name="tooconfigure-user-account-provisioning"></a>tooconfigure felhasználói fiók kiépítése:

hello ebben a szakaszban célja toooutline hogyan tooJive tooenable a felhasználók átadása az Active Directory felhasználói fiókok.
Ez az eljárás részeként szükség tooprovide szüksége toorequest Jive.com a felhasználó biztonsági jogkivonatot.

1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2. Ha már konfigurált Jive egyszeri bejelentkezést, keresse meg a hello keresési mező Jive példányát. Máskülönben válassza **Hozzáadás** keresse meg a **Jive** hello alkalmazás gyűjteményben. Válassza ki a Jive hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.

3. Jelölje ki a Jive példányát, majd jelölje ki a hello **kiépítési** fülre.

4. Set hello **kiépítési üzemmódban** túl**automatikus**. 

    ![Kiépítés](./media/active-directory-saas-jive-provisioning-tutorial/provisioning.png)

5. A hello **rendszergazdai hitelesítő adataival** területen adja meg a következő konfigurációs beállítások hello:
   
    a. A hello **Jive rendszergazda felhasználóneve** szövegmezőhöz típus egy Jive fióknév rendelkezik hello **rendszergazda** Jive.com rendelt profillal.
   
    b. A hello **Jive rendszergazdai jelszó** szövegmezőhöz típus hello fiókhoz tartozó jelszót.
   
    c. A hello **Jive bérlői URL-cím** szövegmezőhöz típus hello Jive bérlői URL-cím.
      
      > [!NOTE]
      > hello Jive bérlői URL-cím a szervezet toolog tooJive a által használt URL-cím.  
      > Általában a hello URL-címnek a hello a következő formátumban: **www.\< szervezet\>. jive.com**.          

6. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour Jive alkalmazás képes kapcsolódni.

7. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be az alábbi hello jelölőnégyzetet.

8. Kattintson a **mentéséhez.**

9. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooJive.**

10. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooJive szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok Jive a frissítési műveletekben. Válassza ki a hello Mentés gombra toocommit módosításokat.

11. tooenable hello Azure AD Jive, módosítás hello a létesítési szolgáltatás **kiépítési állapot** túl**a** hello beállítások szakaszában a

12. Kattintson a **mentéséhez.**

Hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooJive a hello felhasználók és csoportok szakasz kezdődik. hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és kövesse az Jive alkalmazásnak szolgáltatás kiépítését hello által végzett összes műveletet írják le hivatkozások tooprovisioning tevékenység jelentéseket.

Mostantól létrehozhat egy olyan fiókot. Várjon, amíg fel hello fiókot töltött tooverify szinkronizált tooJive too20 perc.

## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [Egyszeri bejelentkezés konfigurálása](active-directory-saas-jive-tutorial.md)