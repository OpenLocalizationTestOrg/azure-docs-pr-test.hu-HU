---
title: "a Bejelentkezés lapon található hello Azure Active Directory aaaCustomize |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadd egy vállalati arculat Azure bejelentkezés toohello lap"
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeffgilb
custom: it-pro
ms.openlocfilehash: 7a7ccdeef0764f6cf9e9e224acd4350983031fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-add-company-branding-tooyour-sign-in-page-in-azure-ad"></a>Gyors üzembe helyezés: Adja hozzá a vállalati arculat megjelenítése a tooyour bejelentkezési oldal az Azure ad-ben
tooavoid zavart, számos vállalat szeretné, hogy egy egységes megjelenést és működést tooapply összes hello webhelyek és szolgáltatások kezelnek. Azure Active Directory (Azure AD) lehetővé teszik, hogy toocustomize hello megjelenésének hello bejelentkezési lapot a vállalat emblémájának elhelyezésével és egyéni színsémák biztosítja ezt a lehetőséget. hello bejelentkezési oldal hello oldal jelenik meg a bejelentkezéskor tooOffice 365 vagy más webes alkalmazásokat, az identitás-szolgáltatóként az Azure AD-t használó. Ön a szolgáltatóosztályokkal a lap tooenter a hitelesítő adatait.

> [!NOTE]
> * Vállalati arculat megjelenítése csak akkor, ha frissített toohello prémium vagy alapszintű kiadására az Azure AD, vagy az Office 365-licencre van. Ha egy támogatja a licenctípus ellenőrizze hello toolearn [Azure Active Directory árképzési adatai lap](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * Az Azure Active Directory prémium és alapszintű kiadásai érhetők el a kínai ügyfelek számára az Azure Active Directory hello világszerte példányát használja. Az Azure Active Directory prémium és alapszintű kiadásai jelenleg nem támogatottak Kínában a 21Vianet által működtetett hello Microsoft Azure szolgáltatásban. További információkért lépjen velünk kapcsolatba: hello [Azure Active Directory fórumán](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="customizing-hello-sign-in-page"></a>Hello bejelentkezési oldal testreszabása

<!--You can customize hello following elements on hello sign-in page: <attach image>-->

Márkajelzési testreszabások hello Azure AD bejelentkezési oldal jelenik meg, amikor a felhasználó hozzáfér egy bérlőspecifikus URL-CÍMMEL, mint vállalati [ *https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

Amikor a felhasználók által felkeresett www.office.com például általános URL-címet, hello bejelentkezési oldal vállalati arculat megjelenítése a testreszabások, mert hello rendszer nem ismeri hello felhasználó, aki nem tartalmaz. Vállalati arculat megjelenítése felhasználók adja meg a felhasználói Azonosítóját, vagy a felhasználói csempe kiválasztása után jelennek meg.

> [!NOTE]
> * A tartománynév szerepelnie kell "Aktív" hello **tartományok** része hello Azure portál, amely a vállalati arculatot konfigurálta. További információkért lásd: [egy egyéni tartománynév hozzáadása](add-custom-domain.md).
> * Bejelentkezési oldal vállalati arculata nem jelenik meg toohello bejelentkezési oldal személyes Microsoft-fiók. Ha az alkalmazottak vagy az üzleti vendégek jelentkezzen be személyes Microsoft-fiókkal, a bejelentkezési oldal nem tükrözi a szervezet branding hello.


### <a name="banner-logo"></a>Szalagcímembléma 

Leírás | Korlátozások | Javaslatok
------- | ------- | ----------
hello szalagcím emblémájának hello bejelentkezés és hello hozzáférési panel oldalakon jelenik meg.<br>Ez a hello bejelentkezési oldal, a után hello felhasználó szervezete állapota határozza meg, általában hello felhasználónév megadása után jeleníti meg.  | Transzparens JPG vagy PNG<br>Maximális magasságát: 36 képpont<br>Maximális szélessége: 245 képpont | Itt adhat meg a vállalati embléma.<br>Transzparens lemezképet használ. Nem érdemes feltételezni, hogy hello háttér fehér lesz.<br>Ne adja hozzá az embléma körüli szövegtávolság hello kép, vagy az embléma aránytalanul kis fog kinézni.

### <a name="username-hint"></a>Felhasználónév-emlékeztető   
Leírás | Korlátozások | Javaslatok
------- | ------- | ----------
Ez testreszabja hello emlékeztető szöveg hello felhasználónév mezőben. | Unicode szöveg too64 karakterek mentése<br>Csak egyszerű szöveg | Azt javasoljuk, hogy nincs megadva ez Ha vendégfelhasználók kívül a szervezet toosign tooyour alkalmazásban.
            
### <a name="sign-in-page-text"></a>A bejelentkezési oldal szövege   
Leírás | Korlátozások | Javaslatok
------- | ------- | ----------
Hello aljához hello bejelentkezési képernyő jelenik meg, és lehet használt toocommunicate további információt, például a hello telefon száma tooyour támogatási szolgálat, vagy egy jogi nyilatkozatot. | Unicode szöveg too256 karakterek mentése<br>Csak egyszerű szöveg (hivatkozások és HTML-címkék nélkül) 

### <a name="sign-in-page-image"></a>Bejelentkezési lap képe  
Leírás | Korlátozások | Javaslatok
------- | ------- | ----------
Ez hello háttérben hello bejelentkezési oldal jelenik meg az rögzített toohello center hello megtekinthető terület, méretezhető és toofill körülvágása hello böngészőablakot.  <br>Például a mobiltelefonok keskeny képernyő a lemezkép nem jelenik meg.<br>Egy fekete maszkra 0.55 átlátszatlanság fogja őket alkalmazni, a kép kód hello oldal betöltésekor. | JPG vagy PNG<br>Kép méretei: 1920 x 1080 képpont<br>A fájl mérete: &gt; 300 KB | <br>A lemezképeket amennyiben nem áll rendelkezésre egy erős tulajdonos fókuszt. hello nem átlátszó bejelentkezési képernyő jelenik meg a lemezkép hello közepére keresztül, és terjedhetnek ki valamely része szerint hello kép hello böngészőablak hello méretétől függően.<br>Hello fájlméret mérete legyen a lehetséges tooensure gyors betöltési idők érdekében. 

### <a name="background-color"></a>Háttérszín
Leírás | Korlátozások | Javaslatok
------- | ------- | ----------
Ezt a színt használja a háttérkép hello alacsony sávszélességű kapcsolatok helyett. |   Hexadecimális RGB szín (például: #FFFFFF | Javasoljuk, hogy a hello szalagcím emblémájának elsődleges színét hello vagy a szervezet megfelelő.

### <a name="show-option-tooremain-signed-in"></a>Bejelentkezett beállítás tooremain megjelenítése
Leírás | Korlátozások | Javaslatok
------- | ------- | ----------
Az Azure AD bejelentkezés által biztosított hello felhasználói hello beállítás tooremain bejelentkezett zárja be, és nyissa meg újra a böngészőt. A toohide ezt a beállítást használja.<br>Állítsa ezt túl "nem" toohide a felhasználók ezt a lehetőséget. | &nbsp; | Ez nem befolyásolja a munkamenetek élettartamát.<br>Office 2010 és SharePoint Online néhány szolgáltatása képes toochoose tooremain bejelentkezett felhasználók függ. Ha a rejtett toobe, a felhasználók további és váratlan kér toosign a jelenhet meg.

> [!NOTE]
> Ezen elemek egyike sem kötelező. Például ha nincs háttérkép szalagcímemblémát adja meg, a hello bejelentkezési oldal megjelenik az embléma és hello háttérképének hello célhelyre (például Office 365).

## <a name="add-company-branding-tooyour-directory"></a>Vállalati arculat megjelenítése a tooyour könyvtár

1. Jelentkezzen be a túl[hello Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.

   ![Nyitó felhasználók kezelése](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. A hello **felhasználók és csoportok** panelen válassza **vállalati arculat**.
4. A hello **felhasználók és csoportok – a vállalati arculat** panelen, jelölje be hello **szerkesztése** parancsot.

    ![Egyéni branding szerkesztése](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. Módosítsa a kívánt toocustomize hello elemek. Ezen elemek egyike sem kötelező.
6. Kattintson a **Save** (Mentés) gombra.

A módosítások toohello bejelentkezési lapon márkajelzési tooappear is eltarthat tooan óra.

## <a name="add-language-specific-company-branding-tooyour-directory"></a>Nyelvspecifikus vállalati arculat megjelenítése a tooyour könyvtár

1. Jelentkezzen be toohello [az Azure AD felügyeleti központban](https://aad.portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.

   ![Nyitó felhasználók kezelése](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. A hello **felhasználók és csoportok** panelen válassza **vállalati arculat**.
4. A hello **felhasználók és csoportok – a vállalati arculat** panelen, jelölje be hello **nyelv hozzáadása** parancsot.

    ![Nyelvspecifikus márkajelzési elemek hozzáadása](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. Módosítsa a kívánt toocustomize hello elemek. Ezen elemek egyike sem kötelező.
6. Kattintson a **Save** (Mentés) gombra.

A módosítások toohello bejelentkezési lapon márkajelzési tooappear is eltarthat tooan óra.

## <a name="next-steps"></a>Következő lépések
A gyors üzembe helyezés is megtanulta, hogyan tooadd vállalati arculat tooyour az Azure Active directory. 

A következő hivatkozás tooconfigure a vállalati védjegyadatoknak a hello Azure AD-ban Azure-portálon hello is használhatja.

> [!div class="nextstepaction"]
> [Vállalati arculat konfigurálása](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 