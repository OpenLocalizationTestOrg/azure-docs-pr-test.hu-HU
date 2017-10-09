---
title: "Oktatóanyag: Azure Active Directory-integráció a Qlik logika vállalati |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Qlik logika vállalati között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 153edb3f8cd41790d4e9a74f15cfb806e5e9e3b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Oktatóanyag: Azure Active Directory-integráció a Qlik logika vállalati

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Qlik logika vállalati az Azure Active Directoryval (Azure AD).

Qlik logika vállalati integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooQlik logika vállalati rendelkező szabályozhatja.
- Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooQlik logika vállalati (egyszeri bejelentkezés).
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása a Qlik logika vállalati tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Qlik logika vállalati egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Qlik logika vállalati hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-qlik-sense-enterprise-from-hello-gallery"></a>Hello gyűjteményből Qlik logika vállalati hozzáadása
tooconfigure hello integrációs Qlik logika vállalat az Azure AD-be, meg kell tooadd Qlik logika vállalati hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Qlik logika vállalati hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Qlik logika vállalati**, jelölje be **Qlik logika vállalati** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Qlik logika vállalati hello eredmények listájában](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Qlik logika vállalati "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Qlik logika vállalati tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Qlik logika vállalati közötti kapcsolat kapcsolatot kell létrehozni toobe.

Qlik logika vállalati, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a Qlik logika vállalati, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Qlik logika vállalati tesztfelhasználó létrehozása](#create-a-qlik-sense-enterprise-test-user)**  -toohave egy megfelelője a Britta Simon Qlik logika vállalat, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Qlik logika vállalati alkalmazás egyszeri bejelentkezés beállítása.

**tooconfigure az Azure AD egyszeri bejelentkezést a Qlik logika vállalati, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Qlik logika vállalati** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. A hello **Qlik logika vállalati tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információk Qlik logika vállalati tartomány és az URL-címek](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`
    
    > [!NOTE]
    > Vegye figyelembe, záró perjelet ezt az URI hello végén hello. Szükség.
    
    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosítója, amelyeket ez az oktatóanyag, vagy forduljon a frissítés [Qlik logika vállalati ügyfél-támogatási csoport](https://www.qlik.com/us/services/support) tooget ezeket az értékeket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_400.png)

6. Készítse elő a hello összevonási metaadatok XML-fájlt, hogy feltöltheti tooQlik logika kiszolgálóval.
   
    > [!NOTE]
    > Hello IdP metaadatok toohello Qlik logika server feltöltését, mielőtt hello fájlt kell szerkeszteni toobe tooremove információk tooensure megfelelő működéséhez az Azure AD között és Qlik logika kiszolgáló.
    
    ![QlikSense][qs24]
   
    a. Nyissa meg szövegszerkesztőben az Azure portálról letöltött hello FederationMetaData.xml fájlt.
   
    b. Keresse meg hello érték **RoleDescriptor**.  Négy bejegyzést (nyitó és záró elemcímkék két pár) is tartalmaz.
   
    c. Törölje hello RoleDescriptor címkéket és az összes adatot a kettő között hello fájlból.
   
    d. Mentse hello fájlt, és a dokumentum későbbi használatra közelben tartani.

7. Keresse meg a toohello Qlik logika Qlik felügyeleti konzol (QMC) hozhat létre virtuális proxybeállításokkal felhasználóként.

8. Hello QMC, kattintson a hello **virtuális proxyk** menüpont.
   
    ![QlikSense][qs6] 

9. Hello a hello képernyő aljára, kattintson a hello **hozzon létre új** gombra.
   
    ![QlikSense][qs7]

10. hello virtuális proxy Szerkesztés képernyő jelenik meg.  Hello hello képernyő jobb oldalán található a konfigurációs beállítások láthatóvá tétele menü.
   
    ![QlikSense][qs9]

11. Hello azonosító menü beállítás be van jelölve adja meg a hello Azure virtuális proxykonfigurációt hello azonosító információkat.
    
    ![QlikSense][qs8]  
    
    a. Hello **leírás** mezője hello virtuális proxykonfigurációt rövid nevét.  Adjon meg egy értéket leírását.
    
    b. Hello **előtag** mező azonosítja hello virtuális proxy végpont tooQlik logika csatlakozni az Azure AD egyszeri bejelentkezést.  Adja meg a virtuális proxy előtag egyedi nevét.
    
    c. **Munkamenet inaktivitás időkorlátja (perc)** az hello időkorlátot a virtuális proxyn keresztül történő kapcsolatokhoz.
    
    d. Hello **munkamenet cookie-k fejlécnév** hello Qlik logika munkamenet a felhasználó megkapja a sikeres hitelesítés után a munkamenet-azonosítót hello hello cookie nevét tárolja.  Ez a név nem egyedi.

12. Kattintson a hello hitelesítési menü beállítás toomake azt látható.  hello hitelesítési képernyő jelenik meg.
    
    ![QlikSense][qs10]
    
    a. Hello **névtelen hozzáférési mód** legördülő lista határozza meg, ha a névtelen felhasználók férhetnek hozzá Qlik logika hello virtuális proxyn keresztül.  hello alapértelmezett beállítás nem névtelen felhasználó.
    
    b. Hello **hitelesítési módszer** legördülő lista határozza meg, hogy hello hitelesítési séma hello virtuális proxy fog használni.  Válassza ki a SAML hello legördülő listából.  További beállítások következtében jelennek meg.
    
    c. A hello **SAML gazdagép URI mező**, bemeneti hello állomásnév felhasználók tooaccess Qlik logika a SAML-alapú virtuális proxyn keresztül történő adja meg.  hello az állomásnév megadása hello Qlik logika server hello URI-azonosítóját.
    
    d. A hello **SAML Entitásazonosító**, adja meg ugyanazt az értéket a megadott hello SAML állomás URI mező hello.
    
    e. Hello **SAML IdP metaadatok** hello a korábbi szerkeszteni hello fájl **összevonási metaadatok szerkesztése az Azure AD-konfigurációból** szakasz.  **Hello IdP metaadatok feltöltése előtt hello fájlt kell szerkeszteni toobe** tooremove információk tooensure megfelelő működéséhez az Azure AD között és Qlik logika kiszolgáló.  **Ha hello fájl még toobe szerkeszteni tekintse meg a fenti toohello utasítások.**  Ha hello fájl kattintson a hello Tallózás gombra, és válassza hello szerkesztett metaadatok fájl tooupload azt toohello virtuális proxybeállításait.
    
    f. Adja meg a hello nevét vagy séma attribútumhivatkozás hello SAML attribútum hello képviselő **UserID** az Azure AD toohello Qlik logika server küld.  Hello Azure alkalmazás képernyők feladás egy vagy több konfigurációs séma referencia jellegű információt érhető el.  toouse hello névattribútuma. Adjon meg `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    g. Hello meg hello **felhasználói directory** tooQlik logika kiszolgálóhoz az Azure AD-n keresztül hitelesítéshez csatolt toousers fogja tárolni.  Szoftveresen kötött értékeket kell lennie.%n **szögletes szögletes zárójelbe []**.  toouse attribútum küldi hello Azure AD SAML-előfeltétel, a mezőben adja meg hello attribútum neve hello **nélkül** szögletes zárójelek között.
    
    h. Hello **SAML aláírási algoritmus** beállítása hello szolgáltatás szolgáltató (a kis Qlik logika kiszolgálón) tanúsítvány-aláírási hello virtuális proxy konfigurációhoz.  Ha Qlik logika kiszolgáló Microsoft Enhanced RSA és az AES kriptográfiai szolgáltató használatával létrehozott megbízható tanúsítványt használ, módosítsa hello SAML aláírási algoritmus túl**SHA-256**.
    
    i. hello SAML attribútum leképezési szakasz lehetővé teszi a csoportok küldött toobe tooQlik logika biztonsági szabályok használható például további attribútumokat.

13. Kattintson a hello **TERHELÉSELOSZTÁS** menü beállítás toomake azt látható.  hello terheléselosztás képernyő jelenik meg.
    
    ![QlikSense][qs11]

14. Kattintson a hello **új csomópont hozzáadása a kiszolgáló** gombra, jelölje be a vezérlő csomópont, vagy csomópontok Qlik logika munkamenetek toofor terheléselosztás céljából küld, és kattintson hello **Hozzáadás** gombra.
    
    ![QlikSense][qs12]

15. Kattintson a hello Speciális menüben beállítás toomake azt látható. hello speciális képernyő jelenik meg.
    
    ![QlikSense][qs13]
    
    hello állomás fehér lista toohello Qlik logika kiszolgálóhoz kapcsolódáskor elfogadott állomásnevek azonosítja.  **Adjon meg felhasználókat kell megadnia a tooQlik logika kiszolgálóhoz kapcsolódáskor hello állomásnevet.** hello az állomásnév megadása hello azonos érték szerint hello SAML állomás uri hello https:// nélkül.

16. Kattintson a hello **alkalmaz** gombra.
    
    ![QlikSense][qs14]

17. Kattintson az OK tooaccept hello figyelmeztető üzenet arról, hogy proxyk csatolt toohello virtuális proxy újraindul.
    
    ![QlikSense][qs15]

18. Az üdvözlő képernyőt hello jobb oldalán hello társított elemek menü jelenik meg.  Kattintson a hello **proxyk** menüjét.
    
    ![QlikSense][qs16]

19. hello proxy képernyő jelenik meg.  Kattintson a hello **hivatkozás** gombra kell hello alsó toolink proxy toohello virtuális proxyt.
    
    ![QlikSense][qs17]

20. Jelölje be hello gyorsítótárproxy-csomópont, amely támogatja a virtuális proxy kapcsolat kattintson hello **hivatkozás** gombra.  Csatolása, után hello proxy a társított proxyk alatt jelennek meg.
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. Miután körülbelül öt tooten másodperc, a hello QMC frissítése message jelenik meg.  Kattintson a hello **frissítése QMC** gombra.
    
    ![QlikSense][qs20]

22. Amikor frissíti a hello QMC, kattintson a hello **virtuális proxyk** menüpont. hello új SAML-alapú virtuális proxy bejegyzés az üdvözlő képernyőt hello táblázatban felsorolt.  Hello virtuális proxy bejegyzésre egyetlen kattintással.
    
    ![QlikSense][qs51]

23. Hello a hello képernyő aljára hello letöltése SP metaadatok gomb aktiválódik.  Kattintson a hello **letöltése SP metaadatok** gomb tooa toosave hello metaadatfájl.
    
    ![QlikSense][qs52]

24. Nyissa meg hello sp metaadatait tartalmazó fájl.  Tekintse át az hello **entityid beállítást** bejegyzés és hello **AssertionConsumerService** bejegyzés.  Ezek az értékek egyenértékű toohello **azonosító** és hello **bejelentkezési URL-cím** hello Azure AD alkalmazás-konfigurációban. Illessze be ezeket az értékeket a hello **Qlik logika vállalati tartomány és az URL-címek** szakasz hello Azure AD alkalmazás konfigurációban, ha azok nem egyeznek, akkor le kell cserélni őket hello Azure AD alkalmazás-konfigurációs varázsló.
    
    ![QlikSense][qs53]

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

   ![hello Azure Active Directory gomb](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

   ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

   ![hello Hozzáadás gomb](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

   ![hello felhasználó párbeszédpanel](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)

   a. A hello **neve** mezőbe írja be **BrittaSimon**.

   b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

   c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

   d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a>Qlik logika vállalati tesztfelhasználó létrehozása

Ebben a szakaszban Britta Simon meghívta Qlik logika vállalati felhasználó létrehozása. Együttműködve [Qlik logika vállalati ügyfél-támogatási csoport](https://www.qlik.com/us/services/support) felhasználót is hozzáadhat hello hello Qlik logika vállalati platform. Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt. 

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooQlik logika vállalati megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooQlik logika vállalati, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Qlik logika vállalati**.

    ![hello Qlik logika vállalati hivatkozásra hello alkalmazások listája](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Qlik logika vállalati csempe gombra kattint, automatikusan bejelentkezett tooyour Qlik logika vállalati alkalmazás szerezheti be. 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

