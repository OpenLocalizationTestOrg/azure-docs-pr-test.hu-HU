---
title: "Oktatóanyag: A felhasználók átadása által Facebook munkahelyi konfigurálása |} Microsoft Docs"
description: "Ismerje meg, hogyan tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, a Facebook által az Azure AD tooWorkplace."
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
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 33d294dbc8f441b29138408b3c9ca41f2141f8af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configure-workplace-by-facebook-for-user-provisioning"></a>Oktatóanyag: A felhasználók átadása által Facebook munkahelyi konfigurálása

Az oktatóanyag azt mutatja be, a szükséges lépéseket tooautomatically hello kiépítése és leépíti a felhasználói fiókok Azure Active Directory (Azure AD) tooWorkplace által Facebook-on.

## <a name="prerequisites"></a>Előfeltételek

az Azure AD-integráció a munkahely által Facebook tooconfigure, következőkre lesz szüksége hello:

- Az Azure AD szolgáltatásra
- A munkahelyi Facebook egyszeri bejelentkezés (SSO) által engedélyezett előfizetés

Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assign-users-tooworkplace-by-facebook"></a>Rendelje hozzá a felhasználók tooWorkplace által Facebook-on

Az Azure AD egy fogalom, mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok tooan alkalmazás az Azure ad-ben rendelt szinkronizálva.

Hello szolgáltatás, kiépítés engedélyezése és konfigurálása eldönti mely felhasználók és csoportok az Azure AD képviselő hello felhasználók számára, akik kell tooyour munkahelyi által Facebook-alkalmazást. Facebook-alkalmazás által a felhasználók tooyour munkahelyi rendelheti a következő témakör utasításait: hello [hozzárendelése egy felhasználóhoz vagy csoporthoz tooan vállalati alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

>[!IMPORTANT]
>*   Teszt hello egyetlen hozzárendelésével konfigurációs kiépítése az Azure AD felhasználói tooWorkplace által Facebook-on. További felhasználók és csoportok hozzárendelése a később.
>*   Ha egy felhasználó tooWorkplace által Facebook, ki kell választania egy érvényes felhasználói szerepkörnek. hello alapértelmezett szerepkör nem működik történő üzembe helyezéséhez.

## <a name="enable-automated-user-provisioning"></a>Az automatikus felhasználó-kiépítés engedélyezése

Ez a szakasz végigvezeti az Azure AD toohello felhasználói fiókja munkahelyi API által a Facebook-kiépítés kapcsolódás. Azt is megtudhatja, hogyan tooconfigure hello létesítési szolgáltatás toocreate frissítése, és tiltsa le a munkahelyi Facebook által hozzárendelt felhasználói fiókok. Ez a felhasználó és az Azure AD-csoport-hozzárendelés alapul.

>[!Tip]
>Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezés által Facebook, a munkahelyi fiókhoz az hello hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés automatikus kiépítés, függetlenül konfigurálhatók, abban az esetben, ha ez a két funkció egészítik ki egymást.

### <a name="configure-user-account-provisioning-tooworkplace-by-facebook-in-azure-ad"></a>Az Azure AD által Facebook tooWorkplace kiépítés felhasználói fiók beállítása

Az Azure AD által támogatott hello képességét tooautomatically szinkronizálása hello fiók részleteinek felhasználók tooWorkplace Facebook által hozzárendelt. Az automatikus szinkronizálás lehetővé teszi, hogy a munkahelyi Facebook tooget hello adatok tooauthorize felhasználók a hozzáférés érdekében előtt kell őket toosign a hello az első alkalommal próbált. Azt is deszerializálni látja el felhasználók a munkahely által Facebook amikor hozzáférés az Azure AD vissza lett vonva.

1. A hello [Azure-portálon](https://portal.azure.com), jelölje be **Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás**.

2. Ha már beállította az egyszeri bejelentkezés munkahelyi által Facebook, keresse meg a munkahely által Facebook példányát hello keresési mező használatával. Máskülönben válassza **Hozzáadás** keresse meg a **által Facebook munkahelyi** hello alkalmazás gyűjteményben. Válassza ki **által Facebook munkahelyi** hello a keresési eredményekben, és adja hozzá tooyour alkalmazások listáját.

3. A munkahely által Facebook példányát, majd válassza ki és hello **kiépítési** fülre.

4. Állítsa be **kiépítési üzemmódban** túl**automatikus**. 

    ![Képernyőfelvétel a munkahelyi által Facebook üzembe helyezési lehetőségek](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. A hello **rendszergazdai hitelesítő adataival** területen adja meg a hello **jogkivonat titkos kulcs** és hello **bérlői URL-cím** a munkahelyének Facebook-rendszergazda.

6. Hello Azure-portálon, válassza ki **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak munkahelyi tooyour által Facebook-alkalmazást. Ha hello létesített kapcsolat megszakad, győződjön meg arról, hogy a munkahelyi Facebook-fiókkal Team rendszergazdai jogosultságokkal rendelkezik egy.

7. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be a hello jelölőnégyzetet.

8. Kattintson a **Mentés** gombra.

9. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooWorkplace által Facebook**.

10. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooWorkplace Facebook által szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok: használt toomatch hello tartozó felhasználói fiókok által Facebook munkahelyi frissítési műveleteket. toocommit a módosításokat, válassza ki **mentése**.

11. tooenable hello Azure AD létesítési szolgáltatás által a Facebook, a munkahelyi fiókhoz hello **beállítások** területen módosítsa a hello **kiépítési állapot** túl**a**.

12. Kattintson a **Mentés** gombra.

További információt a tooconfigure automatikus kiépítés, lásd: [Facebook-dokumentáció hello](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).

Mostantól létrehozhat egy olyan fiókot. Várjon, amíg fel hello fiókot töltött tooverify tooWorkplace Facebook által szinkronizált too20 perc.

## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [Egyszeri bejelentkezés konfigurálása](active-directory-saas-facebook-at-work-tutorial.md)

