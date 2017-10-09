---
title: "Oktatóanyag: Azure Active Directory-integráció a Microsoft Azure felhőalapú felügyeleti portállal |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a felhő felügyeleti portál a Microsoft Azure között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4ea9f47c-25ca-42b0-a878-9e7aa6f34973
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 9596826e3dc1289b95009bf01ec8b86f823ef345
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a>Oktatóanyag: Azure Active Directory-integráció a Microsoft Azure felhőalapú felügyeleti portállal

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate felhőalapú kezelési portál a Microsoft Azure az Azure Active Directoryval (Azure AD).

Microsoft Azure felhőalapú kezelési portál integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooCloud a Microsoft Azure Management Portal rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCloud (egyszeri bejelentkezés) a Microsoft Azure felügyeleti portálon a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD integrálása felhő felügyeleti portál a Microsoft Azure, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A Microsoft Azure egyszeri bejelentkezést engedélyezett előfizetés felhő felügyeleti portál

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Felhőalapú felügyeleti portál a Microsoft Azure hozzáadását hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-cloud-management-portal-for-microsoft-azure-from-hello-gallery"></a>Felhőalapú felügyeleti portál a Microsoft Azure hozzáadását hello gyűjteményből
tooconfigure hello integrációs felhő felügyeleti portál a Microsoft Azure, az Azure AD-be, szükség van tooadd felhőalapú kezelési portál a Microsoft Azure hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd felhőalapú kezelési portál a Microsoft Azure hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **felhő felügyeleti portál a Microsoft Azure**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_search.png)

5. A hello eredmények panelen válassza a **felhő felügyeleti portál a Microsoft Azure**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján a Microsoft Azure felhőalapú felügyeleti portállal.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói felhőalapú kezelési portál a Microsoft Azure tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználói felhőalapú kezelési portál a Microsoft Azure és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

A Microsoft Azure felhőbe a kezelési portálon, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a felhő felügyeleti portállal a Microsoft Azure, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[A felhő kezelési portál a Microsoft Azure tesztfelhasználó létrehozása](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)**  -toohave egy megfelelője a Britta Simon felhőalapú kezelési portál a Microsoft Azure-felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a felhő kezelési portál a Microsoft Azure-alkalmazás az egyszeri bejelentkezés beállítása.

**Felhő felügyeleti portál a Microsoft Azure-ban az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **felhő felügyeleti portál a Microsoft Azure** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_samlbase.png)

3. A hello **felhő felügyeleti portál a Microsoft Azure-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra: 
    
    | |
    |--|
    | `https://portal.newsignature.com/<instancename>` |   
    | `https://portal.igcm.com/<instancename>` |
    
    b. A hello **azonosító** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra: 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com` |
    | `https://<subdomain>.newsignature.com` |

    c. A hello **válasz URL-CÍMEN** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra: 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com/<instancename>` |
    | `https://<subdomain>.newsignature.com` |
    | `https://<subdomain>.newsignature.com/<instancename>` |

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítheti ezeket az értékeket hello tényleges bejelentkezési URL-cím, a azonosítója és a válasz URL-címmel. Ügyfél [felhő felügyeleti portál a Microsoft Azure ügyfél-támogatási csoport](mailto:jczernuszka@newsignature.com) tooget ezeket az értékeket. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-newsignature-tutorial/tutorial_general_400.png)

6. A hello **felhő felügyeleti portál a Microsoft Azure Configuration** kattintson **felhő portál konfigurálása a Microsoft Azure** tooopen **bejelentkezéskonfigurálása**ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_configure.png) 

7. tooconfigure egyszeri bejelentkezést a **felhő felügyeleti portál a Microsoft Azure** oldalon kell letöltött toosend hello **tanúsítvány**, **Sign-Out URL-cím**, **SAML-alapú egyszeri bejelentkezési URL-címe** és **SAML Entitásazonosító** túl[felhő felügyeleti portál a Microsoft Azure támogatási csoport](mailto:jczernuszka@newsignature.com). Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a>A felhő kezelési portál a Microsoft Azure tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate Britta Simon nevű felhő kezelési portál a Microsoft Azure felhasználó. Adjon együttműködve [felhő felügyeleti portál a Microsoft Azure támogatási csapatával](mailto:jczernuszka@newsignature.com) tooadd hello felhasználók hello felhőalapú kezelési portál a Microsoft Azure-fiók.


### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCloud a Microsoft Azure Management Portal megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooCloud a Microsoft Azure Management Portal hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **felhő felügyeleti portál a Microsoft Azure**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.
Hello felhőalapú kezelési portál a Microsoft Azure csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour felhőalapú kezelési portál szerezheti be a Microsoft Azure-alkalmazás.

További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png

