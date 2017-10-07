---
title: "aaaAzure Active Directory v2.0 Android-alkalmazás |} Microsoft Docs"
description: "Android-alkalmazás, mely aláírja a felhasználók személyes Microsoft-fiók és a munkahelyi vagy iskolai fiókok és hívások toobuild hogyan Graph API hello harmadik féltől származó könyvtárak használatával."
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: 16294c07-f27d-45c9-833f-7dbb12083794
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 1dd40bd3bcea28c629abce09abaed66b38774162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-android-app-using-a-third-party-library-with-graph-api-using-hello-v20-endpoint"></a>Egy külső könyvtár használata a Graph API segítségével hello v2.0-végponttól bejelentkezési tooan Android alkalmazás hozzáadása
hello Microsoft identitásplatformmal például OAuth2 és az OpenID Connect nyitott szabványok használja. A fejlesztők a kívánják a szolgáltatások toointegrate függvénytárat. toohelp fejlesztők más könyvtárakkal a platformot használ, azt korábban írt néhány forgatókönyvek például a egy toodemonstrate hogyan tooconfigure külső szalagtárak tooconnect toohello Microsoft identitásplatformmal. A legtöbb tárak, amelyek megvalósítják az [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) toohello Microsoft identitásplatformmal kapcsolódhatnak.

Ez a forgatókönyv létrehozó hello alkalmazást, a felhasználók tootheir szervezet bejelentkezhet és majd keresse meg a magukat a szervezetek hello Graph API segítségével.

Ha új tooOAuth2 vagy az OpenID Connect, ez a minta konfigurálási nem tehetik logika tooyou. Azt javasoljuk, hogy olvassa el [2.0 protokoll - OAuth 2.0 hitelesítési kód Flow](active-directory-v2-protocols-oauth-code.md) a háttérben.

> [!NOTE]
> Egyes szolgáltatások, amelyek rendelkeznek egy kifejezés hello OAuth2 vagy OpenID Connect szabványok, például a feltételes hozzáférés és a Csoportházirend kezelése Intune-ban, a platform szükség akkor toouse a Microsoft Azure identitáskezelési szalagtárak nyílt forráskódú.
> 
> 

hello v2.0-végpontra nem támogatja az összes Azure Active Directory forgatókönyvek és funkciók.

> [!NOTE]
> toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
> 
> 

## <a name="download-hello-code-from-github"></a>A Githubból hello kód letöltése
az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).  toofollow mellett, akkor [töltse le a .zip hello alkalmazás vázát](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) vagy a Klónozás hello vázat:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Csak is letöltheti hello mintát, és rögtön használatba:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Alkalmazás regisztrálása
Hozzon létre egy új alkalmazást hello [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy hajtsa végre a részletes lépésekről hello [hogyan tooregister egy alkalmazást a v2.0-végponttól hello](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

* Másolás hello **alkalmazásazonosító** , amely hozzárendelt tooyour app, mert hamarosan kell.
* Adja hozzá a hello **Mobile** platform az alkalmazásra vonatkozóan.

> Megjegyzés: hello alkalmazásregisztrációs portálra biztosít egy **átirányítási URI-** érték. Azonban ez a példa kell használnia hello alapértelmezett értékének `https://login.microsoftonline.com/common/oauth2/nativeclient`.
> 
> 

## <a name="download-hello-nxoauth2-third-party-library-and-create-a-workspace"></a>Töltse le a hello NXOAuth2 külső könyvtárban, és egy munkaterület létrehozása
A forgatókönyv a Githubból, amely alapján hello Google kódját OpenID Connect OAuth2 könyvtár OIDCAndroidLib hello fogja használni. Hello natív alkalmazásprofil valósítja meg, és támogatja a hello engedélyezési végpont hello felhasználó. Az alábbiakban összes hello, konfigurálnia kell toointegrate hello Microsoft identitásplatformmal együttműködve.

Hello OIDCAndroidLib tárház tooyour számítógép klónozása.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](../media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Az Android Studio környezet beállítása
1. Hozzon létre egy új Android Studio-projektet, és fogadja el hello alapértelmezett hello varázslóban.
   
    ![Az Android Studio új projekt létrehozása](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)
   
    ![Cél Android-eszközök](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)
   
    ![Egy tevékenység toomobile hozzáadása](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)
2. tooset be a projekt modulok áthelyezése hello klónozott tárház toohello projekt helyére. Hello projekt is létrehozhat, és ezután klónozza közvetlenül toohello projekt helyére.
   
    ![Projekt modulok](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)
3. Nyissa meg a hello modulok Projektbeállítások hello helyi menü használatával vagy hello Ctrl + Alt + Maj + S parancsikon használatával.
   
    ![Modulok Projektbeállítások](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)
4. Eltávolít hello alapértelmezett app modult, mivel csak hello Projektbeállítások tároló.
   
    ![hello alapértelmezett app modul](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)
5. Modul importálása hello klónozott tárház toohello aktuális projektben.
   
    ![Projekt importálása a gradle-lel](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![új modul lap](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)
6. Ismételje meg ezeket a lépéseket hello `oidlib-sample` modul.
7. Hello oidclib függőség a hello ellenőrzése `oidlib-sample` modul.
   
    ![oidclib függőségek hello oidlib-mintavételi modul](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)
8. Kattintson a **OK** és várjon, amíg a gradle-szinkronizálás.
   
    A settings.gradle hasonlóan kell kinéznie:
   
    ![Képernyőkép a settings.gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)
9. Build hello sample app toomake meg arról, hogy megfelelően fut hello minta.
   
    Ön nem fogja tudni toouse Ez az Azure Active Directoryval még. Szükség lesz tooconfigure egyes végpontok először. Ez a tooensure nem rendelkezik az Android Studio problémák előtt először hello mintaalkalmazás testreszabása.
10. Létrehozása és futtatása `oidlib-sample` Android Studio hello célként.
    
    ![A minta oidlib build folyamatban](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)
11. Törölje a hello `app ` címtárhoz, amely hagyták, amikor hello modul hello projektből eltávolította, mert a biztonsági Android Studio nem törli.
    
    ![Hello alkalmazáskönyvtárban tartalmazó fájlstruktúra](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)
12. Nyissa meg hello **szerkesztése konfigurációk** menü tooremove futtatása hello konfigurációs hello modul hello projekt való eltávolításakor is balra.
    
    ![Konfigurációk menü szerkesztése](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![konfigurációs alkalmazás futtatása](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-hello-endpoints-of-hello-sample"></a>Hello minta hello végpontok konfigurálása
Most, hogy hello `oidlib-sample` fut-e., most szerkesztése egyes végpontok tooget a munkát az Azure Active Directoryban.

### <a name="configure-your-client-by-editing-hello-oidcclientconfxml-file"></a>Az ügyfél konfigurálása hello oidc_clientconf.xml fájl szerkesztésével
1. OAuth2 adatfolyamok csak tooget egy tokent használ, és hello Graph API hívása, mert hello ügyfél toodo OAuth2 csak beállítása Egy újabb példa OIDC származnak.
   
    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```
2. Az ügyfél-azonosító kapott hello regisztrációs portál konfigurálása
   
    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```
3. Konfigurálja az átirányítási URI-t egy hello alatt.
   
    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```
4. A hatókörök, hogy meg kell a rendelés tooaccess hello Graph API konfigurálása.
   
    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

Hello `User.Read` értéket `oidc_scopes` lehetővé teszi, hogy tooread hello alapvető profiladataihoz hello bejelentkezett felhasználó.
Minden hello elérhető hatókörök kapcsolatos részletesebb [Microsoft Graph-engedélyhatókörök](https://graph.microsoft.io/docs/authorization/permission_scopes).

Ha azt szeretné magyarázatokat kapcsolatos `openid` vagy `offline_access` , az OpenID Connect hatókörök, lásd: [2.0 protokoll - OAuth 2.0 hitelesítési kód Flow](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-hello-oidcendpointsxml-file"></a>Az ügyfél-végpontok hello oidc_endpoints.xml fájl szerkesztésével konfigurálása
* Nyissa meg hello `oidc_endpoints.xml` , és győződjön meg a következő módosításokat hello:
  
    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Ezeket a végpontokat kell soha ne módosuljanak OAuth2 protokoll használatakor.

> [!NOTE]
> a végpontok hello `userInfoEndpoint` és `revocationEndpoint` jelenleg nem támogatottak az Azure Active Directoryban. Ha nem adja meg ezen a hello alapértékként example.com, fog emlékezteti, hogy nincsenek elérhető hello mintában :-)
> 
> 

## <a name="configure-a-graph-api-call"></a>A Graph API-hívás konfigurálása
* Nyissa meg hello `HomeActivity.java` , és győződjön meg a következő módosításokat hello:
  
    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Egy egyszerű Graph API-hívás itt az adatait adja vissza.

Most már az összes hello módosítását, hogy kell-e toodo. Futtassa a hello `oidlib-sample` alkalmazáshoz, majd kattintson **bejelentkezés**.

Már sikeresen hitelesítése után válassza ki a hello **védett erőforrás kérelem** tootest gombra a hívás toohello Graph API-val.

## <a name="get-security-updates-for-our-product"></a>A termék biztonsági frissítések beszerzése
Javasoljuk, biztonsági incidensek tooget értesítések hello felkeresésével [biztonsági TechCenter](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.

