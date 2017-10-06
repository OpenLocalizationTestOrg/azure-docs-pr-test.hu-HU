---
title: "Oktatóanyag: Azure Active Directory-integráció Amazon Web Services (AWS) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az Amazon Web Services (AWS) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 1b79572ace63f6174ce4fa014c49bf44bd728228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Oktatóanyag: Azure Active Directory-integráció Amazon Web Services (AWS)

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Amazon Web Services (AWS) az Azure Active Directoryval (Azure AD).

Amazon Web Services (AWS) integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooAmazon Web Services (AWS) rendelkező Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAmazon Web Services (AWS) (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with Amazon Web Services (AWS), it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD-integrációs Amazon Web Services (AWS), a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Amazon Web Services (AWS) egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Amazon Web Services (AWS) hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-amazon-web-services-aws-from-hello-gallery"></a>Amazon Web Services (AWS) hozzáadása hello gyűjteményből
tooconfigure hello integrációs Amazon Web Services (AWS) az Azure AD-be, meg kell tooadd Amazon Web Services (AWS) hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Amazon Web Services (AWS) hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure Portal](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Amazon Web Services (AWS)**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. A hello eredmények panelen válassza ki a **Amazon Web Services (AWS)**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az Amazon Web Services (AWS) "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Amazon Web Services (AWS) tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználói Amazon Web Services (AWS) és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Amazon Web Services (AWS).

tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése az Amazon Web Services (AWS), a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Az Amazon Web Services tesztfelhasználó létrehozása](#creating-an-amazon-web-services-test-user)**  -toohave Britta Simon az Amazon Web Services (AWS), amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Amazon Web Services (AWS) alkalmazásban.

**az Amazon Web Services (AWS), az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**

1. Az Azure-portálon hello a hello **Amazon Web Services (AWS)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanelen, **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. A hello **Amazon Web Services (AWS) tartományhoz és URL-címek** területen hello felhasználó nem rendelkezik tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. hello Amazon Web Services (AWS) alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban. Állítsa be az alkalmazás jogcímek a következő hello. Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján. a következő képernyőkép hello ezen mutat egy példát.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. A hello **felhasználói attribútumok** hello című szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum fenti hello ábrán látható módon, és hajtsa végre az alábbi lépésekkel hello:
    
    | Attribútum neve  | Attribútum-érték | Namespace |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://AWS.amazon.com/SAML/Attributes |
    | Szerepkör            | User.assignedroles |  https://AWS.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Az AWS konzol minden hello szerepkörök az Azure AD toofetch kiépítési tooconfigure hello felhasználó van szüksége. Tekintse meg az alábbi lépéseket kiépítés hello.

    a. Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    b. A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    c. A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett. Hello Namespace érték hozzáadásával a fent megadott.
    
    d. Kattintson az **OK** gombra.

7. Kattintson a **mentése** Azure toosave hello beállítások gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. Egy másik böngészőablakban, bejelentkezés tooyour Amazon Web Services (AWS) vállalati hely rendszergazdaként.

9. Kattintson a **konzol otthoni**.
   
    ![Egyszeri bejelentkezés konfigurálása][11]

10. Kattintson a **IAM** a **biztonsági, identitás- & megfelelőségi** szolgáltatás.
   
    ![Egyszeri bejelentkezés konfigurálása][12]

11. Kattintson a **identitás-szolgáltatóktól**, és kattintson a **létrehozása szolgáltató**.
   
    ![Egyszeri bejelentkezés konfigurálása][13]

12. A hello **szolgáltató konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés konfigurálása][14]
 
    a. Mint **szolgáltatótípust**, jelölje be **SAML**.

    b. A hello **szolgáltatónevet** szövegmező, írja be a szolgáltató nevét (pl.: *fahulladékok*).

    c. tooupload a letöltött metaadatfájl, kattintson a **Choose File**.

    d. Kattintson a **következő lépés**.

13. A hello **ellenőrizze a szolgáltató adatait** párbeszédpanel lap, kattintson a **létrehozása**. 
    
    ![Egyszeri bejelentkezés konfigurálása][15]

14. Kattintson a **szerepkörök**, és kattintson a **hozzon létre új szerepkör**. 
    
    ![Egyszeri bejelentkezés konfigurálása][16]

15. A hello **szerepkörnév beállítása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello: 
    
    ![Egyszeri bejelentkezés konfigurálása][17] 

    a. A hello **szerepkörnév** szövegmező, írja be a szerepkör nevét (pl.: *tesztfelhasználó néven*). 

    b. Kattintson a **következő lépés**.

16. A hello **szerepkör típusának kiválasztása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello: 
    
    ![Egyszeri bejelentkezés konfigurálása][18] 

    a. Válassza ki **Identity Provider hozzáférési szerepkör**. 

    b. A hello **Grant webes egyszeri bejelentkezés (WebSSO) hozzáférési tooSAML szolgáltatók** területén kattintson **válasszon**.

17. A hello **létre megbízható** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:  
    
    ![Egyszeri bejelentkezés konfigurálása][19] 

    a. SAML-szolgáltatóként, válassza ki a korábban létrehozott hello SAML-szolgáltatót (pl.: *fahulladékok*)
  
    b. Kattintson a **következő lépés**.

18. A hello **szerepkör megbízható ellenőrizze** párbeszédpanel, kattintson **tovább**.
    
    ![Egyszeri bejelentkezés konfigurálása][32]

19. A hello **csatolása házirend** párbeszédpanel, kattintson a **tovább**.
    
    ![Egyszeri bejelentkezés konfigurálása][33]

20. A hello **felülvizsgálati** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
    
    ![Egyszeri bejelentkezés konfigurálása][34]
 
    a. Kattintson a **szerepkör létrehozása**.

    b. Igény szerint annyi szerepköröket hozhat létre, és oldalváltozókhoz toohello identitásszolgáltató.

21. Mostantól beállíthatja hello felhasználók toofetch átadásához AWS összes hello szerepkörei

    a. A hello AWS konzol jelentkezzen be a rendszergazdafiók

    b. A hello jobb felső sarokban kattintson a nevére, és kattintson a hello **saját biztonsági hitelesítő adatok** lehetőséget. Figyelmeztető üzenet, ekkor megnyílik egy olyan képernyőt. Hello gombra **biztonsági hitelesítő adatok** gomb toopass hello képernyő.
        
       ![Egyszeri bejelentkezés konfigurálása][36]

       ![Egyszeri bejelentkezés konfigurálása][37]

    c. A Tárelérési kulcsok hello szakaszban kattintson hello **új kulcs létrehozása** gombra. Ezt követően hello hozzáférési kulcs Azonosítóját és a token értékét.
    
       ![Egyszeri bejelentkezés konfigurálása][38]

    d. Másolja a következő két értéket, és töltse le a is maga után, hogy ne vesszenek el.

    e. Az Azure-portálon hello hello Amazon Web Services (AWS) alkalmazás integráció lapján, kattintson a **kiépítési**.
        
       ![Egyszeri bejelentkezés konfigurálása][35]

    f. Hello Kiépítési mód beállítása túl**automatikus**
        
       ![Egyszeri bejelentkezés konfigurálása][39]

    g. Most már a hello **clientsecret** és **titkos Token** illessze be a hello tartozó értékek, amelyek az AWS konzol másolta.
    
    h. Kattinthat a hello **kapcsolat tesztelése** tootest hello kapcsolat gombra. Miután művelet sikeres majd elindíthatja összekötő kiépítés hello.
       
       ![Egyszeri bejelentkezés konfigurálása][40]

    i. Most már engedélyezheti a kiépítési állapot hello túl**a**. Hello szerepkörök beolvasása a hello alkalmazás elindul.

       ![Egyszeri bejelentkezés konfigurálása][41]

    > [!NOTE]
    > Az Azure AD-kiépítés szolgáltatásának futtatásakor minden egyes idő toosync hello szerepkörök az AWS után. Megtekintheti az összes hello identitásszolgáltató csatolt AWS szerepkörök az Azure AD-be és hello alkalmazás toousers vagy csoportok hozzárendelésekor is használhatja őket.

<!--### Next steps

tooensure users can sign-in tooAmazon Web Services (AWS) after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooAmazon Web Services (AWS) in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-amazon-web-services-test-user"></a>Az Amazon Web Services tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog a Web Services (AWS) tooAmazon azok ki kell építenie az Amazon Web Services (AWS). Hello esetben Amazon Web Services (AWS) kézi tevékenység.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour **Amazon Web Services (AWS)** vállalati hely rendszergazdaként.

2. Kattintson a hello **konzol otthoni** ikonra. 
   
    ![Egyszeri bejelentkezés konfigurálása][11]

3. Kattintson az identitás és hozzáférés-kezelést. 
   
    ![Egyszeri bejelentkezés konfigurálása][28]

4. Hello irányítópultot, kattintson **felhasználók**, és kattintson a **hozzon létre új felhasználók**. 
   
    ![Egyszeri bejelentkezés konfigurálása][29]

5. Hello felhasználó létrehozása párbeszédpanelen hajtsa végre a lépéseket követve hello: 
   
    ![Egyszeri bejelentkezés konfigurálása][30]   
    
    a. A hello **adja meg a felhasználói neveket** szövegmezőből, írja be a Brita Simon felhasználónevet (userprincipalname), az Azure ad-ben.

    b. Kattintson a **létrehozása.**
        
### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezése Britta Simon toouse Azure egyszeri bejelentkezéshez használt hozzáférés tooAmazon Web Services (AWS) megadása.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooAmazon Web Services (AWS), hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Amazon Web Services (AWS)**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. A **Szerepkörválasztás** lapon adja meg, jelölje be hello megfelelő hello felhasználói szerepkört. Ezek a szerepkörök jelennek meg hello szerepkör nevét és az identitás-szolgáltató neve. Így egyszerűbb azonosítani AWS hello szerepköröket.

8. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello hello hozzáférési Panel Amazon Web Services (AWS) csempére kattintva kapja meg automatikusan bejelentkezett tooyour Amazon Web Services (AWS) alkalmazást. 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
