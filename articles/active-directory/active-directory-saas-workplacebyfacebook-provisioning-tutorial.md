---
title: "Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a munkahely által Facebook között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 551ec353a5ec1da936373587688c299a6f4acca7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-workplace-by-facebook-for-user-provisioning"></a>Oktatóanyag: A felhasználók átadása által Facebook munkahelyi konfigurálása

hello Ez az oktatóanyag célja tooshow meg hello által Facebook és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooWorkplace által Facebook munkahelyi tooperform lépéseit.

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD-integráció a munkahely által Facebook, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A munkahelyi által Facebook egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assigning-users-tooworkplace-by-facebook"></a>Felhasználók tooWorkplace által Facebook hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooyour munkahelyi Facebook-alkalmazást úgy kell meghatároznia. Ha úgy döntött, hozzárendelheti a felhasználók tooyour munkahelyi Facebook-alkalmazás által itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-tooworkplace-by-facebook"></a>Fontos tippek a felhasználók tooWorkplace által Facebook hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó Facebook tootest hello konfigurálása kiosztás által hozzárendelt tooWorkplace. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooWorkplace által Facebook rendel, ki kell választania egy érvényes felhasználói szerepkörnek. hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.

## <a name="enable-user-provisioning"></a>Felhasználó-kiépítés engedélyezése

Ez a szakasz végigvezeti az Azure AD tooWorkplace csatlakozó Facebook a felhasználói fiók kiépítése API és a kiépítés szolgáltatás toocreate hello konfigurálása, frissítése, és tiltsa le a munkahelyi Facebook alapján a felhasználók és csoportok által hozzárendelt felhasználói fiókok az Azure AD-hozzárendelés.

>[!Tip]
>Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Facebook, a következő utasításokat hello által munkahelyi [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.

### <a name="tooconfigure-user-account-provisioning-tooworkplace-by-facebook-in-azure-ad"></a>tooconfigure felhasználói fiók által Facebook tooWorkplace kiépítése az Azure ad-ben:

hello ebben a szakaszban célja toooutline hogyan tooenable kiépítése Active Directory felhasználói fiókok által Facebook tooWorkplace.

Az Azure AD által támogatott hello képességét tooautomatically szinkronizálása hello fiók részleteinek felhasználók tooWorkplace Facebook által hozzárendelt. Az automatikus szinkronizálás lehetővé teszi, hogy a munkahelyi Facebook tooget hello adatok tooauthorize felhasználók a hozzáférés érdekében előtt kell őket toosign a hello az első alkalommal próbált. Azt is deszerializálni látja el felhasználók a munkahely által Facebook amikor hozzáférés az Azure AD vissza lett vonva.

1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás** szakasz.

2. Ha már beállította az egyszeri bejelentkezés munkahelyi által Facebook, keresse meg a munkahely által hello keresési mező Facebook példányát. Máskülönben válassza **Hozzáadás** keresse meg a **által Facebook munkahelyi** hello alkalmazás gyűjteményben. Válassza ki a munkahely által Facebook hello a keresési eredmények, és vegye fel tooyour alkalmazások listáját.

3. Válassza ki a munkahely által Facebook példányt, majd jelölje ki a hello **kiépítési** fülre.

4. Set hello **kiépítési üzemmódban** túl**automatikus**. 

    ![Kiépítés](./media/active-directory-saas-workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. A hello **rendszergazdai hitelesítő adataival** szakaszt, adja meg a titkos kulcs Token hello és bérlői URL-cím, a munkahelyének Facebook rendszergazda hello.

6. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak munkahelyi tooyour által Facebook-alkalmazást. Ha hello létesített kapcsolat megszakad, győződjön meg arról, a Facebook-fiókkal munkahelyi Team rendszergazdai jogosultságokkal rendelkezik.

7. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be hello jelölőnégyzetet.

8. Kattintson a **mentéséhez.**

9. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooWorkplace által Facebook-on.**

10. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooWorkplace Facebook által szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok: használt toomatch hello tartozó felhasználói fiókok által Facebook munkahelyi frissítési műveleteket. Válassza ki a hello Mentés gombra toocommit módosításokat.

11. tooenable hello Azure AD létesítési szolgáltatás által a Facebook, a módosítás hello munkahelyi fiókhoz **kiépítési állapot** túl**a** a hello **beállítások** szakasz

12. Kattintson a **mentéséhez.**

További információt a tooconfigure automatikus kiépítés, lásd: [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)

Mostantól létrehozhat egy olyan fiókot. Várjon, amíg fel hello fiókot töltött tooverify tooWorkplace Facebook által szinkronizált too20 perc.

## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [Egyszeri bejelentkezés konfigurálása](active-directory-saas-workplacebyfacebook-tutorial.md)

