---
title: "aaaConfigure SaaS-alkalmazásokhoz az Azure Active Directory B2B együttműködés |} Microsoft Docs"
description: "Azure Active Directory B2B együttműködés kód és a PowerShell-példák"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: c3f22f81567c04ac23ef2316c09de718ecb15d26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a>B2B együttműködés SaaS-alkalmazások konfigurálása

Az Azure Active Directory (Azure AD) B2B együttműködés működik együtt a legtöbb alkalmazást, amelyekbe beépül az Azure AD. Ez a szakasz azt végezze el az Azure AD B2B néhány népszerű SaaS-alkalmazások a konfigurálására vonatkozó útmutatásokat.

Mielőtt az alkalmazás-specifikus utasításokkal tekinti meg, az alábbiakban néhány szabályok megoldás:

* A legtöbb hello alkalmazások, a felhasználó a telepítőnek toohappen manuálisan. Ez azt jelenti, hogy felhasználók manuálisan kell létrehozni, valamint hello alkalmazásban.

* Automatikus telepítés, például a Dropbox, támogató alkalmazások esetében külön meghívókat hello alkalmazásokból jönnek létre. Felhasználók kell lennie, hogy tooaccept minden meghívó.

* Hello felhasználói attribútumok toomitigate esetleges problémáinak összekeveredett felhasználói profil lemezre (UPD) a vendégfelhasználók, mindig beállította **felhasználói azonosító** túl**user.mail**.


## <a name="dropbox-business"></a>Dropbox üzleti

tooenable felhasználók toosign a szervezeti fiókjával be, manuálisan kell konfigurálni az Azure AD Dropbox üzleti toouse Security Assertion Markup Language (SAML) identitás-szolgáltatóként. Ha Dropbox üzleti nem konfigurált toodo tehát nem kérni és egyébként teszik lehetővé a felhasználók toosign az Azure AD használatával.

1. tooadd hello Dropbox üzleti alkalmazást az Azure AD, válassza ki **vállalati alkalmazások** hello bal oldali ablaktáblán, és kattintson a **Hozzáadás**.

  ![hello vállalati alkalmazások lapon hello "Hozzáadás" gombra](media/active-directory-b2b-configure-saas-apps/add-dropbox.png)

2. A hello **alkalmazás hozzáadása** ablak, írja be **dropbox** hello a keresési mezőbe, és válassza ki **Dropbox vállalati** hello eredménylistában.

  ![Keressen a "dropbox" hello egy alkalmazás-weblap hozzáadása](media/active-directory-b2b-configure-saas-apps/add-app-dialog.png)

3. A hello **egyszeri bejelentkezés** lapon jelölje be **egyszeri bejelentkezés** a hello bal oldali ablaktáblán, és írja be **user.mail** a hello **felhasználói azonosító** mezőbe. (Érték szerint UPN alapértelmezés szerint.)

  ![Egyszeri bejelentkezés hello alkalmazás konfigurálása](media/active-directory-b2b-configure-saas-apps/configure-app-sso.png)

4. toodownload hello tanúsítvány toouse Dropbox-konfigurációhoz, válassza ki **konfigurálása DropBox**, majd válassza ki **SAML-alapú egyszeri bejelentkezési szolgáltatás URL-cím** hello listában.

  ![Dropbox konfigurációs hello tanúsítvány letöltése](media/active-directory-b2b-configure-saas-apps/download-certificate.png)

5. Jelentkezzen be a hello tooDropbox bejelentkezés URL-címet a hello **egyszeri bejelentkezés** lap.

  ![hello Dropbox-bejelentkezés lap](media/active-directory-b2b-configure-saas-apps/sign-in-to-dropbox.png)

6. A hello menüben válassza **felügyeleti konzol**.

  ![hello "Felügyeleti konzol" hivatkozásra kattintva hello Dropbox menü](media/active-directory-b2b-configure-saas-apps/dropbox-menu.png)

7. A hello **hitelesítési** párbeszédpanelen jelölje ki **további**, hello-tanúsítvány feltöltése, a hello **jelentkezzen be az URL-cím** SAML-alapú egyszeri bejelentkezést hello URL-címet adja meg.

  ![hello hello "Több" hivatkozásra összecsukott párbeszédpanel](media/active-directory-b2b-configure-saas-apps/dropbox-auth-01.png)

  ![a hello "Bejelentkezési URL-a" Hello kibontva párbeszédpanel](media/active-directory-b2b-configure-saas-apps/paste-single-sign-on-URL.png)

8. tooconfigure automatikus felhasználó beállítása a hello Azure-portálon válassza **kiépítési** hello bal oldali ablaktáblában jelöljön ki **automatikus** a hello **kiépítési üzemmódban** mezőbe, majd válassza ki **Engedélyezik**.

  ![Automatikus felhasználók átadására a hello Azure-portálon](media/active-directory-b2b-configure-saas-apps/set-up-automatic-provisioning.png)

Vendég vagy tag felhasználók hello Dropbox alkalmazásban van beállítva, akkor kapnak egy külön meghívó az dropbox-bA. toouse Dropbox egyszeri bejelentkezést, a meghívott személyeknek el kell fogadnia hello felkérést egy hivatkozásra kattintva.

## <a name="box"></a>Box
Összevonási alapuló hello SAML protokoll segítségével lehetővé teheti a felhasználók tooauthenticate mezőben vendég felhasználók az Azure AD-fiókot. Ezzel az eljárással metaadatok tooBox.com feltöltése.

1. Hello vállalati alkalmazások hello Box alkalmazásához adja hozzá.

2. Egyszeri bejelentkezés konfigurálása sorrend hello:

  ![Mezőbe az egyszeri bejelentkezés konfigurálása](media/active-directory-b2b-configure-saas-apps/configure-box-sso.png)

 a. A hello **bejelentkezési URL-cím** győződjön meg arról, hogy hello bejelentkezési URL-cím értékre van megfelelően a Box hello Azure-portálon. Az URL-cím a Box.com bérlő hello URL-CÍMÉT. Akkor érdemes követnie hello elnevezési *https://.box.com*.  
 Hello **azonosító** toothis nem felel meg az alkalmazás, de azt is megjelenik a kötelező mező.

 b. A hello **felhasználói azonosító** adja meg a **user.mail** (az egyszeri bejelentkezés a Vendég-fiókok).

 c. A **SAML-aláíró tanúsítványa**, kattintson a **hozzon létre új tanúsítvány**.

 d. a Box.com bérlői toouse az Azure AD konfigurálása identitás-szolgáltatóként, toobegin hello metaadatait tartalmazó fájl letöltése, és mentse azt tooyour helyi meghajtó.

 e. Továbbítsa hello metaadatok fájl toohello mezőben támogatási csoport, amely egyszeri bejelentkezést az Ön konfigurálja.

3. Az Azure AD automatikus felhasználóbeállítás hello bal oldali ablaktáblában válassza a **kiépítési**, majd válassza ki **engedélyezés**.

  ![Az Azure AD tooconnect tooBox engedélyezése](media/active-directory-b2b-configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

Például a Dropbox a meghívott személyeknek mezőben a meghívott személyeknek kell beváltani a meghívó hello mezőben alkalmazásból.

## <a name="next-steps"></a>Következő lépések

Tekintse meg a következő cikkek az Azure AD B2B együttműködés hello:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B együttműködés felhasználó tulajdonságai](active-directory-b2b-user-properties.md)
* [B2B együttműködés felhasználói tooa szerepkör hozzáadása](active-directory-b2b-add-guest-to-role.md)
* [B2B együttműködés meghívókat delegálása](active-directory-b2b-delegate-invitations.md)
* [Dinamikus csoportok és a B2B együttműködés](active-directory-b2b-dynamic-groups.md)
* [B2B együttműködés kód és a PowerShell-példák](active-directory-b2b-code-samples.md)
* [B2B együttműködés felhasználói jogkivonatokhoz](active-directory-b2b-user-token.md)
* [B2B együttműködés felhasználói jogcímek leképezése](active-directory-b2b-claims-mapping.md)
* [Külső Office 365-megosztás](active-directory-b2b-o365-external-user.md)
* [B2B együttműködés aktuális korlátozásai](active-directory-b2b-current-limitations.md)
