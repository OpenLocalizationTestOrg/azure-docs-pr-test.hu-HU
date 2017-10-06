---
title: "Oktatóanyag: Azure Active Directory Online szolgáltatással való integráció ArcGIS |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az Online ArcGIS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: f3dd55d798cf3256fb2758e011f33946baa405ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Oktatóanyag: Azure Active Directory Online szolgáltatással való integráció ArcGIS

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ArcGIS kapcsolódik az Azure Active Directory (Azure AD).

ArcGIS Online integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooArcGIS Online rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooArcGIS Online (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with ArcGIS Online, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in ArcGIS Online.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Előfeltételek

tooconfigure ArcGIS Online az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy ArcGIS Online egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből ArcGIS Online hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-arcgis-online-from-hello-gallery"></a>Hello gyűjteményből ArcGIS Online hozzáadása
tooconfigure hello integrációs ArcGIS online az Azure AD-be, meg kell tooadd ArcGIS Online hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**hello gyűjteményből Online ArcGIS tooadd hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **új alkalmazás** hello felül hello párbeszédpanel tooadd új alkalmazás gomb.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **ArcGIS Online**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_search.png)

5. A hello eredmények panelen válassza ki a **ArcGIS Online**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés ArcGIS Online "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói ArcGIS online tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználói ArcGIS online és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** ArcGIS Online-ban.

tooconfigure és a ArcGIS Online az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy ArcGIS Online tesztfelhasználó létrehozása](#creating-an-arcgis-online-test-user)**  -toohave egy megfelelője a Britta Simon ArcGIS Online felhasználói ábrázolása csatolt toohello az Azure AD-ban.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az ArcGIS Online alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a ArcGIS Online, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **ArcGIS Online** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. A hello **ArcGIS Online tartomány és az URL-címek** csoportjában hajtsa végre a következő lépés hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://<company>.maps.arcgis.com`

    > [!NOTE] 
    > Ez az érték nincs valós hello. Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT. Ügyfél [ArcGIS Online ügyfél-támogatási csoport](http://support.esri.com/) tooget ezt az értéket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. Egy másik webes böngészőablakban jelentkezzen be a ArcGIS vállalati webhely rendszergazdaként.

7. Kattintson a **beállítások szerkesztése**.

    ![Beállítások szerkesztése](./media/active-directory-saas-arcgis-tutorial/ic784742.png "beállításainak szerkesztése")

8. Kattintson a **biztonsági**.

    ![Biztonsági](./media/active-directory-saas-arcgis-tutorial/ic784743.png "biztonsági")

9. A **vállalati bejelentkezések**, kattintson a **IDENTITÁSSZOLGÁLTATÓ beállítása**.

    ![Vállalati bejelentkezések](./media/active-directory-saas-arcgis-tutorial/ic784744.png "vállalati bejelentkezések")

10. A hello **identitásszolgáltató beállítása** konfiguráció lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Identitásszolgáltató beállítása](./media/active-directory-saas-arcgis-tutorial/ic784745.png "identitásszolgáltató beállítása")
   
    a. A hello **neve** szövegmező, írja be a szervezet nevét.

    b. A **használatával hello vállalati identitásszolgáltató tartozó metaadatok lesznek megadott**, jelölje be **A fájl**.

    c. tooupload a letöltött metaadatfájl, kattintson a **fájl kiválasztása**.

    d. Kattintson a **SET IDENTITÁSSZOLGÁLTATÓ**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a Britta Simon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-arcgis-online-test-user"></a>Egy ArcGIS Online tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog történő ArcGIS Online azok kell üzembe ArcGIS Online be.  
Az ArcGIS Online hello esetben kézi tevékenység.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour **ArcGIS** bérlő.

2. Kattintson a **tagok MEGHÍVÁSA**.
   
    ![Tagok meghívása](./media/active-directory-saas-arcgis-tutorial/ic784747.png "tagok meghívása")

3. Válassza ki **tagok automatikus hozzáadása nélkül e-mail**, és kattintson a **következő**.
   
    ![Tagok hozzáadása automatikusan](./media/active-directory-saas-arcgis-tutorial/ic784748.png "tagok automatikus hozzáadása")

4. A hello **tagok** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
     ![Adja hozzá, és tekintse át](./media/active-directory-saas-arcgis-tutorial/ic784749.png "hozzáadása, és nézze meg")
    
     a. Adja meg a hello **E-mail**, **Utónév**, és **Vezetéknév** tooprovision kívánt fiók érvényes aad-ben.
  
     b. Kattintson a **hozzáadása, és NÉZZE meg**.
5. Tekintse át a hello adatokat adta-e, és kattintson **tagok hozzáadása**.
   
    ![Tag hozzáadása](./media/active-directory-saas-arcgis-tutorial/ic784750.png "tag hozzáadása")
        
    > [!NOTE]
    > hello Azure Active Directory fióktulajdonos fog egy e-maileket, és kövesse a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés Online hozzáférés tooArcGIS megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooArcGIS Online, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **ArcGIS Online**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello ArcGIS Online csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour ArcGIS Online alkalmazás kapja meg.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_203.png

