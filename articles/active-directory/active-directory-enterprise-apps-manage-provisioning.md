---
title: "vállalati alkalmazások hello Azure Active Directory-kezelés kialakítási aaaUser |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage felhasználóifiók-létesítési hello Azure Active Directory használó vállalati alkalmazásokat"
services: active-directory
documentationcenter: 
author: asmalser
manager: femila
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: a613f844c8f51e04b92e62b488313a78ab85f7ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-hello-azure-portal"></a>Felhasználói fiók kiépítése hello Azure-portálon a vállalati alkalmazások kezelése
Ez a cikk ismerteti, hogyan toouse hello [Azure-portálon](https://portal.azure.com) toomanage automatikus felhasználói fiók üzembe helyezést és megszüntetést az alkalmazások, amelyek támogatják ezt, különösen azokat, a hello hozzáadott "kiemelt" kategóriája Hello [Azure Active Directory alkalmazáskatalógusában](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). További információ az automatikus felhasználói fiók kiépítése és annak működéséről, toolearn lásd [Felhasználókiépítés és -megszüntetés tooSaaS alkalmazásokat az Azure Active Directoryval](active-directory-saas-app-provisioning.md).

## <a name="finding-your-apps-in-hello-portal"></a>Az alkalmazások keresése hello portálon
Az egyszeri bejelentkezést a könyvtárban található a hello segítségével directory rendszergazda által beállított összes alkalmazás [Azure Active Directory alkalmazáskatalógusában](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), tekinthetők meg és felügyelete az hello [Azure-portálon](https://portal.azure.com). hello alkalmazások hello található **több szolgáltatások** &gt; **vállalati alkalmazások** hello portál szakaszban. Vállalati alkalmazások olyan alkalmazások, telepített és a szervezetén belül.

![Vállalati alkalmazások panel][0]

Kiválasztásával hello **összes alkalmazás** hello bal oldali hivatkozást vannak konfigurálva, beleértve az alkalmazások hello gyűjteményből hozzáadott összes alkalmazások listáját tartalmazza. Hello erőforráspanelen az alkalmazáshoz, ahol jelentések megtekinthetők az alkalmazáshoz, és különböző beállítások kezelheti egy alkalmazás kiválasztása tölti be.

Felhasználói fiók kialakítási beállításai kezelhető kiválasztásával **kiépítési** hello bal oldalon.

![Alkalmazás erőforráspanelen][1]

## <a name="provisioning-modes"></a>Üzembe helyezési mód
Hello **kiépítési** panel kezdődik egy **mód** menü, amely jeleníti meg, milyen üzembe helyezési mód támogatja egy vállalati alkalmazás, és lehetővé teszi, hogy azok toobe konfigurálva. hello elérhető lehetőségek a következők:

* **Automatikus** – Ez a beállítás akkor jelenik meg, ha az Azure AD automatikus API-alapú üzembe helyezés és/vagy felhasználói fiókok toothis alkalmazás megszüntetést támogatja. Válassza ezt a módot jeleníti meg, amely végigvezeti a rendszergazdák a konfigurálása az Azure AD tooconnect toohello alkalmazás felhasználói felügyeleti API, fiók-hozzárendelések és a munkafolyamatok, amelyek meghatározzák, hogyan felhasználói fiókhoz kell adatfolyam az Azure AD közötti létrehozásának illesztőfelület és hello alkalmazás és létesítési szolgáltatás kezelését hello Azure AD.
* **Manuális** -e beállítás jelenik meg, ha az Azure AD nem támogatja a felhasználói fiókok toothis alkalmazás automatikus kiépítés. Ez a beállítás azt jelenti, hogy felhasználói fiókot rögzíti hello alkalmazásban tárolt felügyelni a külső folyamat, hello felhasználói felügyeleti és üzembe helyezési által biztosított képességek adott alkalmazás (például SAML fordítója kiépítése) alapján.

## <a name="configuring-automatic-user-account-provisioning"></a>Automatikus felhasználói fiók kiépítése konfigurálása
Kiválasztásával hello **automatikus** beállítás, amely négy részből oszlik képernyő jelenik meg:

### <a name="admin-credentials"></a>Rendszergazdai hitelesítő adatokat
Ez azért, ahol meg van-e megadva hello hitelesítő adatokat az Azure AD tooconnect toohello alkalmazás felhasználói felügyeleti API szükséges. hello beavatkozás szükséges hello alkalmazástól függően változik. toolearn hello hitelesítőadat-típusok és adott alkalmazásokra vonatkozó követelményeiről lásd: hello [konfigurációs oktatóanyag az adott alkalmazás](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

Kiválasztásával hello **kapcsolat tesztelése** gomb lehetővé teszi, hogy tootest hello hitelesítő adatokat, hogy tooconnect toohello app kísérlet az Azure AD meg a kiépítés app hello megadott hitelesítő adatok használatával.

### <a name="mappings"></a>Hozzárendelések
Ez az adott rendszergazdák megtekintéséhez és szerkesztéséhez, milyen felhasználói attribútumok folyamat az Azure AD között és hello célalkalmazás, ha a felhasználói fiókok vannak kiépítésekor vagy frissítésekor.

Nincs olyan előre konfigurált megfeleltetéseket az Azure AD felhasználói és minden SaaS-alkalmazás felhasználói objektumok között. Egyes alkalmazások más típusú objektumok, például a csoportok vagy névjegyek kezelése. Hello hozzárendelési szerkesztőt a leképezések egyikének kiválasztásával a hello táblázatban látható toohello jobb, ha azok tekinthetők meg és testre szabott.

![Alkalmazás erőforráspanelen][2]

Támogatott testreszabások a következők:

* Engedélyezése és letiltása adott objektumok, például hello Azure AD felhasználói objektum toohello SaaS-alkalmazáshoz tartozó felhasználói objektum leképezéseit.
* Szerkesztése, mely attribútumok haladjanak hello Azure AD felhasználói objektum toohello alkalmazás felhasználói objektum. További információ a címtárattribútum-leképezésben: [attribútum hozzárendelési típusokhoz ismertetése](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).
* Az Azure AD hello végez műveleteket kiépítés szűrő hello célzott alkalmazás. Ahelyett, hogy az Azure AD teljesen-objektumokat szinkronizálni, korlátozhatja a hello végrehajtott műveleteket. Például kiválasztásával csak **frissítés**, az Azure AD csak frissítések meglévő felhasználói fiókok az alkalmazásban, és nem hoz létre újakat. Csak kiválasztásával **létrehozása**, Azure csak új felhasználói fiókokat hoz létre, de nem frissíti a már meglévőket. Ez a funkció lehetővé teszi, hogy rendszergazdák toocreate különböző leképezéseihez fiók munkafolyamatok létrehozása és frissítése.

### <a name="settings"></a>Beállítások
Ez a szakasz lehetővé teszi, hogy a rendszergazdák toostart és stop hello Azure AD létesítési szolgáltatás a kiválasztott hello alkalmazás, valamint opcionálisan egyértelmű hello kiépítés gyorsítótárba, és indítsa újra hello szolgáltatást.

Ha kiépítés folyamatban engedélyezett hello először az alkalmazás, kapcsolja be a hello szolgáltatás hello módosításával **kiépítési állapot** túl**a**. Ennek hatására az Azure AD hello létesítési szolgáltatás tooperform egy kezdeti szinkronizálást, ahol olvassa be a hello hello felhasználóira **felhasználók és csoportok** szakaszban lekérdezi a célalkalmazás hello számukra, és végrehajtja a hello kiépítése az Azure AD hello definiált műveletek **hozzárendelések** szakasz. A folyamat során hello szolgáltatás kiépítését milyen felhasználói fiókok kezel, a gyorsítótárazott adatokat tárolja, hogy nem kezelt fiókok belül hello célalkalmazások soha nem található a hozzárendelési hatókör megszüntetést műveletek nem érinti. Hello a kezdeti szinkronizálás hello szolgáltatás kiépítését automatikusan szinkronizálást felhasználók és csoportok objektumok 10 perces időközönként.

Változó hello **kiépítési állapot** túl**ki** egyszerűen szünet hello létesítési szolgáltatás. Ebben az állapotban lévő Azure nem létrehozása, frissítése, vagy távolítsa el a felhasználói és csoportobjektumok hello alkalmazásban. Hello állapot hátsó tooon módosítása hatására hello szolgáltatás toopick fel, ahol abbahagyta.

Kiválasztásával hello **törölje az aktuális állapotát, és indítsa újra a szinkronizálási** jelölőnégyzetet, és leállítja mentése hello létesítési szolgáltatás, milyen fiókok Azure AD memóriaképek hello gyorsítótárazott adatokat kezel, hello szolgáltatás újraindul, és hello hajt végre újra a kezdeti szinkronizálást. Ez a beállítás lehetővé teszi, hogy a rendszergazdák toostart hello kiépítési folyamat újra keresztül.

### <a name="synchronization-details"></a>Szinkronizálás részletei
Ez a szakasz hello szolgáltatás kiépítését hello működésének hozzáadását ismerteti, beleértve a hello első és utolsó időpontját hello létesítési szolgáltatás lefutott hello alkalmazást, és hány felhasználó- és objektumok felügyelete alatt áll.

Hivatkozásokkal toohello **tevékenységgel kapcsolatos jelentés kiépítés**, amely biztosítja, hogy a napló összes felhasználók és csoportok létrehozása, frissítése és eltávolított között az Azure AD és a célalkalmazásban is hello és toohello **létesítési hiba a jelentés** biztosító részletes hibaüzeneteket, felhasználó és csoport objektumokat, hogy hibás toobe olvasni, létrehozott, frissíteni vagy eltávolítani. 

##<a name="feedback"></a>Visszajelzés

Reméljük, például a Azure AD felhasználói élményt. Ne hamarosan hello visszajelzését! Visszajelzését és ötleteket a fejlesztéséhez fel hello **felügyeleti portál** szakasza a [visszajelzési fórumon](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Ritkán használt adatok új dolgai minden nap kiépítésével foglalkozó még érdeklődőbbek és felhasználása a útmutatást tooshape és határozza meg, mi készíteni.


[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
