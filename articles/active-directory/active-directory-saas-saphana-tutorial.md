---
title: "Oktatóanyag: Azure Active Directoryval integrált SAP HANA |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az SAP HANA között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 5ff6bfde0b1d7ab14025a4bc45199f9f826affd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Oktatóanyag: Azure Active Directoryval integrált SAP HANA

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SAP HANA az Azure Active Directoryval (Azure AD).

SAP HANA integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooSAP HANA rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSAP HANA (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása SAP HANA tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy SAP HANA egyszeri bejelentkezés engedélyezve van az előfizetés
- Egy futó HANA példány bármely nyilvános IaaS, a helyszíni vagy Azure virtuális gépeken vagy SAP nagy példányok az Azure-ban
- hello XSA felügyeleti webes felülete, valamint a HANA Studio telepítve hello HANA példány.

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, nem javasoljuk a SAP HANA éles környezetben. Vizsgálat hello integrációs először fejlesztési vagy átmeneti környezet hello alkalmazás, majd használja hello éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. SAP HANA hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-sap-hana-from-hello-gallery"></a>SAP HANA hozzáadása hello gyűjteményből
tooconfigure hello integrációja SAP HANA az Azure AD-be, meg kell tooadd SAP HANA hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd SAP HANA hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **SAP HANA**, jelölje be **SAP HANA** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra. 

    ![Új alkalmazás hello](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az SAP HANA "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SAP HANA tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello SAP HANA közötti kapcsolat kapcsolatot kell létrehozni toobe.

SAP HANA, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés SAP HANA-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy SAP HANA tesztfelhasználó létrehozása](#creating-a-sap-hana-test-user)**  -toohave egy megfelelője a Britta Simon az SAP HANA, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez az SAP HANA-alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést az SAP HANA, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **SAP HANA** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. A hello **SAP HANA-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Tartomány- és URL-címeket az egyszeri bejelentkezés információk](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    a. A hello **azonosító** szövegmezőhöz típusú, mint:`HA100` 

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel. Ügyfél [SAP HANA-ügyfél-támogatási csoport](https://cloudplatform.sap.com/contact.html) tooget ezeket az értékeket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Ha a tanúsítvány nincs aktív majd aktiválja az hello kattintva "Ellenőrizze új tanúsítvány aktív" jelölőnégyzetet a hello Azure AD. 

5. SAP HANA-alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban. a következő képernyőkép hello ezen mutat egy példát. Itt azt leképezett hello **felhasználói azonosító** rendelkező **ExtractMailPrefix()** funkciója **user.mail**. Így az hello előtag értéke az e-mailek hello felhasználó, amelynek van hello egyedi felhasználói azonosító. Minden sikeres választ küldött toohello SAP HANA-alkalmazás.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanel:

    a. A hello **felhasználói azonosító** legördülő listában válassza ki **ExtractMailPrefix**.
    
    b. A hello **Mail** legördülő listában válassza ki **user.mail**.

7. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. tooconfigure egyszeri bejelentkezést a **SAP HANA** oldalon, a bejelentkezési tooyour **HANA XSA Webkonzol** toohello tallózással megfelelő HTTPS-végpont.

    > [!Note]
    > Hello alapértelmezés szerint a hello URL-címet átirányítja a felhasználókat hello kérelem tooa bejelentkezési képernyő, amely egy hitelesített SAP HANA adatbázis felhasználói toocomplete hello bejelentkezési folyamat hello hitelesítő adatok szükségesek. hello bejelentkező felhasználó hello jogosultságokkal szükséges tooperform SAML-alapú felügyeleti feladatok kell rendelkeznie.

9. Hello XSA webes felülete, lépjen a túl**SAML-Identitásszolgáltatóként** ott, kattintson a hello **"+"** -hello alján, hello képernyő toodisplay hello identitás szolgáltató adatai hozzáadása panel gombra, majd hajtsa végre hello a következő lépéseket:

    ![Identitás-szolgáltató felvétele](./media/active-directory-saas-saphana-tutorial/sap1.png)

    a. A hello **identitás szolgáltató adatai hozzáadása** ablaktáblán, Beillesztés hello tartalmát hello metaadat-XML fájlok, amelyek már letöltötte az Azure portálról történő hello **metaadatok** szövegmező.

    ![Identitásszolgáltató beállítások hozzáadása](./media/active-directory-saas-saphana-tutorial/sap2.png)

    b. Hello XML-dokumentum tartalmát hello érvényesek, ha folyamat elemzése hello hello szükséges adatokat tooinsert kibontja a hello **tulajdonosa Entitásazonosító és kibocsátó** hello általános adatok mezőinek képernyőn területen, és a hello hello mező URL-címe Képernyő célterületre, például  **SingleSignOn és alap URL-címe (*)**.

    ![Identitásszolgáltató beállítások hozzáadása](./media/active-directory-saas-saphana-tutorial/sap3.png)

    c. Az általános adatok hello hello név mezőben területen kattintson a jobb adjon meg egy nevet az új SAML SSO identitásszolgáltató hello.

    > [!Note]
    > hello SAML IDP hello nevét kötelező megadni, és egyedi; kell lennie Ha kiválasztható SAML hello hitelesítési módszer az SAP HANA XS alkalmazások toouse, például hello hitelesítési képernyő területén hello XS összetevő-felügyeleti eszköz szerepel érhető el, akkor jelenik meg, SAML IDPs hello listája.

10. Mentse a hello új SAML-Identitásszolgáltatóként hello részleteit. Válasszon **mentése** toosave hello hello SAML-Identitásszolgáltatóként részleteit, és adja hozzá a hello új SAML IDP toohello listája ismert SAML IDPs.

    ![Mentés gombja](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. Hello rendszer tulajdonságainak hello belül HANA Studio **konfigurációs** lapján csak a beállítások alapján szűrése **saml** , és módosítsa a hello **assertion_timeout** a**10 másodpercnél** túl**120 másodperc**.

    ![assertion_timeout beállítás](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-sap-hana-test-user"></a>Egy SAP HANA tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog a tooSAP HANA, akkor ki kell építenie az SAP HANA.
SAP HANA támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.

Ha egy felhasználó toocreate manuálisan kell, hajtsa végre a lépéseket követve hello:

>[!Note]
>Hello hello felhasználó által használt külső hitelesítés módosíthatja.
A külső rendszerek, például egy Kerberos rendszer a külső felhasználók hitelesítése. Külső identitások kapcsolatos részletes információkért forduljon a [tartományi rendszergazda](https://cloudplatform.sap.com/contact.html).

1. Nyissa meg hello [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) , rendszergazda, és engedélyezze a SAML SSO hello adatbázis-felhasználó.

    ![Felhasználó létrehozása](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. Osztásjelek hello láthatatlan jelölőnégyzet toohello a bal oldali **SAML** és hello beállítása hivatkozásra.

3. Kattintson a **Hozzáadás** tooadd hello SAML IDP, és kattintson a **OK** kiválasztásával hello megfelelő SAML IDP.

4. Adja hozzá a hello **külső identitások** (például) Itt BrittaSimon), vagy válasszon **"Bármely"** kattintson **OK**.

    >[!Note]
    >Ha nincs bejelölve a "Bármely" jelölőnégyzetet, majd HANA hello a felhasználónév kell tooexactly egyezés hello hello UPN hello tartományi utótag elé hello felhasználó nevét (pl. BrittaSimon@contoso.com válna a HANA BrittaSimon).

5. Tesztelési célokra, rendelje hozzá az összes **"XS"** toohello felhasználói szerepköröket.

    ![szerepkörök hozzárendelése](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > A használati esetek, csak a megfelelő engedélyeket kell adnia.

6. Hello felhasználói mentéséhez.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSAP HANA megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooSAP HANA, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **SAP HANA**.

    ![Felhasználó hozzárendelése](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello SAP HANA csempe gombra kattint, SAP HANA-alkalmazás automatikusan bejelentkezett tooyour szerezheti be.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_203.png

