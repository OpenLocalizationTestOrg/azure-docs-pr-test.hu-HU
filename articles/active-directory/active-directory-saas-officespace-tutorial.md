---
title: "Oktatóanyag: Azure Active Directory-integráció OfficeSpace szoftverrel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és OfficeSpace szoftverek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: b53afb648b8a6057c32c782d857e34c06e152c67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Oktatóanyag: Azure Active Directory-integráció OfficeSpace szoftverrel

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate OfficeSpace szoftver az Azure Active Directoryval (Azure AD).

OfficeSpace szoftver integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooOfficeSpace szoftver rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooOfficeSpace szoftver (egyszeri bejelentkezés) a saját Azure AD-fiókok.
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure OfficeSpace szoftver az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy OfficeSpace szoftver egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből OfficeSpace szoftver hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-officespace-software-from-hello-gallery"></a>Hello gyűjteményből OfficeSpace szoftver hozzáadása
tooconfigure hello integrációs OfficeSpace szoftver az Azure AD-be, meg kell tooadd OfficeSpace szoftver hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd OfficeSpace szoftver hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **OfficeSpace szoftver**, jelölje be **OfficeSpace szoftver** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Hello OfficeSpace szoftver eredmények listájában](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban konfigurálása és tesztelése az Azure AD az egyszeri bejelentkezés OfficeSpace szoftverrel "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói OfficeSpace szoftverfrissítési tooa felhasználói az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello OfficeSpace szoftver közötti kapcsolat kapcsolatot kell létrehozni toobe.

OfficeSpace szoftver, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése OfficeSpace szoftverrel, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[OfficeSpace szoftver tesztfelhasználó létrehozása](#create-a-officespace-software-test-user)**  -toohave egy megfelelője a Britta Simon OfficeSpace szoftver, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a OfficeSpace alkalmazás egyszeri bejelentkezés konfigurálása.

**az Azure AD tooconfigure egyszeri bejelentkezés OfficeSpace szoftvert, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **OfficeSpace szoftver** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_samlbase.png)

3. A hello **OfficeSpace szoftver tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információk OfficeSpace szoftver tartomány és az URL-címek](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.officespacesoftware.com/users/sign_in/saml`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`<company name>.officespacesoftware.com`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [OfficeSpace szoftver ügyfél-támogatási csoport](mailto:support@officespacesoftware.com) tooget ezeket az értékeket. 

4. OfficeSpace alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban. Állítsa be az alkalmazás jogcímek a következő hello. Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján. a következő képernyőkép hello ezen mutat egy példát.
    
    ![Attribútum konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_attribute.png)

5. A hello **felhasználói attribútumok** hello szakaszt **egyszeri bejelentkezés** párbeszédablakban válassza **user.mail** , **felhasználói azonosító** és minden egyes sorára látható hello az alábbi táblázatban szereplő hajtsa végre a lépéseket követve hello:
    
    | Attribútum neve | Attribútum-érték |
    | --- | --- |    
    | E-mailek | User.mail |
    | név | User.DisplayName |
    | Utónév | User.givenName |
    | Vezetéknév | User.surname |

    a. Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.

    ![Konfigurálása hozzáadása ](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_04.png)

    ![Attribútum konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_05.png)
    
    b. A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.
    
    c. A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.
    
    d. Kattintson a **Ok**
 
6. A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** hello tanúsítvány értékét.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_certificate.png) 

7. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_general_400.png)

8. A hello **OfficeSpace szoftverkonfigurációt** kattintson **OfficeSpace szoftver konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![OfficeSpace szoftverkonfigurációjának összeállítása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_configure.png) 

9. Egy másik webes böngészőablakban bejelentkezni a OfficeSpace szoftver Bérlői rendszergazda.

10. Nyissa meg túl**beállítások** kattintson **összekötők**.

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_002.png)

11. Kattintson a **SAML-alapú hitelesítés**.

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_003.png)

12. A hello **SAML-alapú hitelesítés** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_004.png)

    a. A hello **kijelentkezési szolgáltató URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.

    b. A hello **ügyfél idp célként megadott url** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.

    c. Beillesztés hello **ujjlenyomat** érték, amely az Azure portálról történő hello másolta **ügyfél IDP tanúsítvány-ujjlenyomat** szövegmező. 

    d. Kattintson a **beállítások mentése**.


> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-officespace-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-officespace-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-officespace-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-officespace-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-officespace-software-test-user"></a>OfficeSpace szoftver tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate OfficeSpace szoftver Britta Simon nevű felhasználó. OfficeSpace szoftver, amellyel just-in-time átadása, amely alapértelmezés szerint van engedélyezve.

Nincs ebben a szakaszban az Ön művelet elem. Új felhasználó létrejön egy kísérlet tooaccess OfficeSpace szoftver során, ha még nem létezik.

> [!NOTE]
> Ha egy felhasználó toocreate manuálisan kell, tooContact kell [OfficeSpace szoftver támogatási csoport](mailto:support@officespacesoftware.com).

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooOfficeSpace szoftver megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooOfficeSpace szoftver, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **OfficeSpace szoftver**.

    ![hello OfficeSpace szoftverhivatkozás hello alkalmazások listájában](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello OfficeSpace szoftver hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour OfficeSpace alkalmazás.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_203.png

