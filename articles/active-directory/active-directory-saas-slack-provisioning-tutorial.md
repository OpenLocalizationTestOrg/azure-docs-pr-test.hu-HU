---
title: "Oktatóanyag: Slackhez konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooSlack."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: asmalser-msft
ms.reviewer: asmalser
ms.openlocfilehash: d0a565bbe0bd7e229b9dd99b1ebbaf67d93a2206
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-slack-for-automatic-user-provisioning"></a>Oktatóanyag: Slackhez konfigurálása az automatikus felhasználó létesítése


hello Ez az oktatóanyag célja tooshow meg hello lépéseket kell tooperform a Slackhez és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooSlack. 

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active Active directory-bérlő
*   A Slackhez-bérlőben hello [plusz terv](https://aadsyncfabric.slack.com/pricing) vagy jobban engedélyezve 
*   A Slackhez Team rendszergazdai jogosultságokkal rendelkező felhasználói fiókot 

Megjegyzés: hello az Azure AD-integrációs kiépítés támaszkodik hello [Slackhez SCIM API](https://api.slack.com/scim) Ez az hello terv plusz vagy nagyobb a rendelkezésre álló tooSlack csoportok.

## <a name="assigning-users-tooslack"></a>Felhasználók tooSlack hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás az Azure AD-szinkronizálásra kerül. 

Hello szolgáltatás kiépítését engedélyezése és konfigurálása, mielőtt kell toodecide milyen felhasználói és/vagy csoportok tooyour közzététele a Slack-alkalmazást kell használni az Azure AD jelentik hello felhasználók. Ha úgy döntött, hozzárendelheti a felhasználók tooyour Slack app itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-tooslack"></a>Fontos tippek a felhasználók tooSlack hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooSlack tootest hello konfigurálása kiosztás rendelni. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooSlack rendel, ki kell választania hello **felhasználói** vagy a "Csoport" szerepkör hello hozzárendelés párbeszédpanelen. hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.


## <a name="configuring-user-provisioning-tooslack"></a>Felhasználók átadásához tooSlack konfigurálása 

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooSlack felhasználói fiók kiépítése API, és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése és tiltsa le a hozzárendelt felhasználói fiókok a Slackhez alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.

**Tipp:** is választhatja, hogy SAML-alapú egyszeri bejelentkezés a Slackhez, (az Azure portálon) hello megjelenő utasításokat követve tooenabled [https://portal.azure.com]. Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.


### <a name="tooconfigure-automatic-user-account-provisioning-tooslack-in-azure-ad"></a>tooconfigure automatikus felhasználói fiók tooSlack kiépítése az Azure ad-ben:


1)  A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2) Ha már konfigurált Slackhez egyszeri bejelentkezést, keresse meg a Slackhez hello keresési mező példányát. Máskülönben válassza **Hozzáadás** keresse meg a **Slackhez** hello alkalmazás gyűjteményben. Válassza ki a Slackhez hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.

3)  Jelölje ki a Slackhez példányát, majd válassza ki a hello **kiépítési** fülre.

4)  Set hello **kiépítési üzemmódban** túl**automatikus**.

![Slack-kiépítés](./media/active-directory-saas-slack-provisioning-tutorial/Slack1.PNG)

5)  A hello **rendszergazdai hitelesítő adataival** kattintson **engedélyezés**. A Slack engedélyezési párbeszédpanel megnyílik egy új böngészőablakban. 

6) Hello új ablakban jelentkezzen be arra a Slackhez a csapat rendszergazda fiók használatával. hello eredményül kapott engedélyezési párbeszédpanelen válassza ki a Slack csoport, amely szeretné, hogy tooenable kiépítést, és válassza hello **engedélyezés**. Egyszer befejezett, visszatérési toohello az Azure portál toocomplete hello konfigurálása kiosztás.

![Engedélyezési párbeszédpanel](./media/active-directory-saas-slack-provisioning-tutorial/Slack3.PNG)

7) Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak tooyour közzététele a Slack-alkalmazást. Ha hello létesített kapcsolat megszakad, győződjön meg arról, a Slack fiók Team rendszergazdai jogosultságokkal rendelkezik, és próbálja meg újból. "Engedélyezés" lépés hello.

8) Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be az alábbi hello jelölőnégyzetet.

9) Kattintson a **Save** (Mentés) gombra. 

10) A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooSlack**.

11) A hello **attribútum-leképezésekhez** szakaszban, tekintse át a hello felhasználói attribútumok szinkronizálva lesznek az Azure AD tooSlack. Vegye figyelembe, hogy a kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókok a Slackhez a frissítési műveletek lesz. Válassza ki a hello Mentés gombra toocommit módosításokat.

12) tooenable hello Azure AD létesítési szolgáltatás a Slackhez, módosítás hello **kiépítési állapot** túl**a** a hello **beállítások** szakasz

13) Kattintson a **Save** (Mentés) gombra. 

Felhasználók és/vagy csoportok tooSlack a hello felhasználók és csoportok szakasz hello kezdeti szinkronizálása elindítja. Megjegyzés: hello kezdeti szinkronizálás hosszabb, mint körülbelül 10 percenként bekövetkező mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform kezeléséért. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a Slack app a kiépítés végrehajtott műveletekről, amelyeket követve.

## <a name="optional-configuring-group-object-provisioning-tooslack"></a>[Választható] Kiépítés tooSlack csoportházirend-objektum konfigurálása 

Másik lehetőségként engedélyezheti a hello kiépítése az Azure AD tooSlack objektumok. Ez nem azonos a "felhasználói csoportok hozzárendelése", a hello tényleges csoportobjektumnak továbbá tooits tagok replikálásra kerülnek a Azure AD tooSlack. Például ha a "Saját csoport" nevű az Azure AD-csoport, egy "Saját csoport" nevű identitical csoportot az Slackhez belül létrejön.

### <a name="tooenable-provisioning-of-group-objects"></a>tooenable kiépítés objektumok:

1) A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-csoportok tooSlack**.

2) Állítson be engedélyezve tooYes hello attribútum leképezési panelen.

3) A hello **attribútum-leképezésekhez** szakaszban, tekintse át a hello attribútumokat, amelyek az Azure AD tooSlack szinkronizálódnak. Vegye figyelembe, hogy a kiválasztott attribútumok hello **egyező** tulajdonságok lesz a frissítési műveletek Slackhez használt toomatch hello csoportok. 

4) Kattintson a **Save** (Mentés) gombra.

Ez az eredmény bármely csoporthoz rendelt objektumok tooSlack hello a **felhasználók és csoportok** szakaszban az Azure AD tooSlack a teljes szinkronizálás folyik. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a Slack app a kiépítés végrehajtott műveletekről, amelyeket követve.


## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-enterprise-apps-manage-provisioning.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
