---
title: "az Azure App Service API Apps hitelesítése aaaUser |} Microsoft Docs"
description: "Ismerje meg, hogyan férhetnek hozzá a tooprotect engedélyezésével az Azure App Service API-alkalmazás csak tooauthenticated felhasználók."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 3896760d-46ff-4b67-b98a-edd233f24758
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: alkarche
ms.openlocfilehash: 4702fc77fcfe736405e22b033c35d22ae3c5a300
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Az Azure App Service API-alkalmazások felhasználói hitelesítésének
## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja, hogyan tooprotect Azure API-alkalmazást úgy, hogy csak hitelesített felhasználók tudják hívni. hello cikk feltételezi, hogy rendelkezik olvasási hello [Azure App Service hitelesítés áttekintése](../app-service/app-service-authentication-overview.md).

Az oktatóanyagból a következőket sajátíthatja el:

* Hogyan tooconfigure egy hitelesítésszolgáltatót, az Azure Active Directory (Azure AD) adatokkal.
* Hogyan tooconsume védett API-alkalmazás használatával hello [Active Directory Authentication Library (ADAL) a JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).

hello cikk két részből áll:

* Hello [hogyan tooconfigure felhasználói hitelesítés az Azure App Service](#authconfig) a szakasz ismerteti a általános hogyan tooconfigure bármely API-alkalmazások felhasználói hitelesítésének és tooall keretrendszerek App Service, beleértve a .NET, által támogatott keretrendszerre alkalmazható NODE.js és Java.
* Hello kezdve [hello .NET API-alkalmazások oktatóanyagok folytatása](#tutorialstart) területen hello segítségével konfigurálja a .NET mintaalkalmazás biztonsági másolatot az AngularJS első előtérrel és, hogy az Azure Active Directory felhasználó cikk útmutatók hitelesítés. 

## <a id="authconfig"></a>Hogyan tooconfigure felhasználói hitelesítés az Azure App Service-ben
Ez a témakör általános útmutatást, amelyek érvényesek a tooany API-alkalmazás. A lépések adott toohello tooDo lista .NET mintaalkalmazás, go túl[hello .NET-bevezető oktatóanyagok folytatása](#tutorialstart).

1. A hello [Azure-portálon](https://portal.azure.com/), keresse meg a toohello **beállítások** hello API-alkalmazás, amelyet az tooprotect, keresés hello panel **szolgáltatások** szakaszt, és kattintson a  **Hitelesítési / engedélyezési**.
   
    ![Azure-portálon hitelesítési/engedélyezési](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. A hello **hitelesítési / engedélyezési** panelen kattintson a **a**.
3. Hello lehetőségek közül választhatnak hello **művelet tootake hitelesítetlen kérés esetén** legördülő listából.
   
   * Ha azt szeretné, csak a hitelesített hívásokon tooreach az API-alkalmazás, válasszon egyet az hello **jelentkezzen be az...**  beállítások. Ezzel a beállítással azt tooprotect hello API-alkalmazás az azt futtató programozás nélkül.
   * Ha azt szeretné, hogy az összes meghívja tooreach az API-alkalmazás, **kérés engedélyezése (intézkedés)**. A beállítás nem hitelesített toodirect hívóknak tooa választott hitelesítésszolgáltatókat is használhatja. Ezt a beállítást az informatikai részleg toowrite kód toohandle engedélyezési.
     
     További információ: [Az API Apps hitelesítése és engedélyezése az Azure App Service-ben](app-service-api-authentication.md#multiple-protection-options).
4. Jelöljön ki legalább egy hello **hitelesítésszolgáltatókat**.
   
    hello kép bemutatja az Azure AD által hitelesített összes hívóknak toobe igénylő lehetőségeket.
   
    ![Az Azure portál hitelesítési/engedélyezési panel](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
   
    Ha úgy dönt, hogy egy hitelesítésszolgáltató, hello portál egy konfigurációs panel jeleníti meg ezt a szolgáltatót. 
   
    Ismerteti, hogyan tooconfigure hello hitelesítési szolgáltató konfigurációs paneleken részletes útmutatás: [hogyan tooconfigure az App Service alkalmazás toouse Azure Active Directory bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (hello hivatkozás kerül tooan cikk arról, hogy az Azure AD meg, de hello cikkbe, amely hozzárendeli a hello tooarticles más hitelesítésszolgáltatókat lapot tartalmaz,.)
5. Amikor elkészült, a hello hitelesítési szolgáltató konfigurációs panelen, kattintson a **OK**.
6. A hello **hitelesítési / engedélyezési** panelen kattintson a **mentése**.

Ebben az esetben, ha az App Service API-hívások hitelesíti a hello API-alkalmazás elérése előtti. hello hitelesítési szolgáltatások munkahelyi hello ugyanaz az App service támogat, beleértve a .NET, Node.js és Java minden nyelvhez. 

toomake hitelesített API-hívások, hello hívó tartalmaz hello hitelesítési szolgáltató OAuth 2.0 tulajdonosi jogkivonattal a HTTP-kérések hello Authorization fejlécet. hello token szerezhető hello hitelesítési szolgáltató SDK használatával.

## <a id="tutorialstart"></a>Hello .NET API-alkalmazások oktatóanyagok folytatása
Ha a következő hello Node.js vagy Java oktatóanyagok API-alkalmazások, hagyja ki toohello a következő cikkben [szolgáltatás egyszerű hitelesítési API-alkalmazások](app-service-api-dotnet-service-principal-auth.md). 

Ha a következő hello .NET oktatóanyag adatsorozat API-alkalmazások és a már telepített hello mintaalkalmazás hello megfelelően [első](app-service-api-dotnet-get-started.md) és [második](app-service-api-cors-consume-javascript.md) oktatóanyagok kihagyása toohello [beállítása az App Service és az Azure AD hitelesítési](#azureauth) szakasz.

Ha azt szeretné toofollow ebben az oktatóanyagban hello első és második oktatóanyagok áthaladás nélkül, hello a következő lépések azt ismertetik, hogyan tooget egy automatikus folyamat toodeploy hello mintaalkalmazás használatával indította.

> [!NOTE]
> hello lépések első azonos kiindulási pontja, mintha az első két oktatóanyagokat, egy kivétellel volt hello toohello: a Visual Studio már nem tudja, melyik webalkalmazás vagy API-alkalmazás, amelynek minden projekt telepítése lekérdezi. Ez azt jelenti, hogy emiatt nem tud pontos útmutatásokat ebben az oktatóanyagban hogyan toodeploy toohello megfelelő célokat. Ha nem beválik tudja, hogyan toodo hello telepítési lépéseit a saját, a rendszer jobb toofollow hello oktatóanyag adatsorozat hello a [első oktatóanyaga, amely](app-service-api-dotnet-get-started.md) mint toostart ennek automatikus központi telepítési folyamat.
> 
> 

1. Győződjön meg arról, hogy rendelkezik az összes felsorolt hello hello Előfeltételek [első oktatóanyaga, amely](app-service-api-dotnet-get-started.md). Továbbá toohello előfeltételeket a hitelesítési oktatóanyagok azt feltételezik, az App Service web apps és az API apps a Visual Studio dolgozott, és hello Azure-portálon.
2. Kattintson a hello **tooAzure telepítése** hello gombjára [tooDo lista minta tárház információs fájl](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) toodeploy hello API apps és hello webes alkalmazás. Jegyezze fel a hello Azure erőforráscsoport jön létre, hasonlóan a későbbi toolook web app és az API-alkalmazás nevét.
3. Letöltés vagy a Klónozás hello [tooDo lista minta tárház](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) tooget hello kódját fogja használata helyileg a Visual Studióban.

## <a id="azureauth"></a>Az App Service és az Azure AD hitelesítési beállítása
Most már rendelkezik anélkül, hogy a felhasználók hitelesítése az Azure App Service-ben futó hello alkalmazás. Ebben a szakaszban hitelesítés hello a következő feladatok végrehajtásával hozzáadása:

* Konfigurálja az App Service toorequire Azure Active Directory (Azure AD-) hitelesítést a hello középső rétegbeli API-alkalmazást hívja.
* Hozzon létre egy Azure AD-alkalmazást.
* Hello Azure AD alkalmazás toosend hello tulajdonosi jogkivonattal konfigurálása után bejelentkezési toohello AngularJS előtér. 

Ha a következő hello oktatóanyag utasításait során problémákat tapasztal, tekintse meg a hello [hibaelhárítás](#troubleshooting) szakasz hello oktatóanyag hello végén. 

### <a name="configure-authentication-for-hello-middle-tier-api-app"></a>Hitelesítés beállítása a hello középső rétegbeli API-alkalmazás
1. A hello [Azure-portálon](https://portal.azure.com/), keresse meg a toohello **beállítások** hello ToDoListAPI projektben létrehozott hello API-alkalmazás paneljén található hello **szolgáltatások** szakaszban, majd Kattintson a **hitelesítési / engedélyezési**.
   
    ![Azure-portálon hitelesítési/engedélyezési](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. A hello **hitelesítési / engedélyezési** panelen kattintson a **a**.
3. A hello **hitelesítetlen kérés esetén a művelet tootake** legördülő listában válassza **bejelentkezés Azure Active Directoryval**.
   
    Ez a beállítás biztosítja, hogy unauathenticated kérelmek nem elérje hello API-alkalmazás. 
4. A **hitelesítésszolgáltatókat**, kattintson a **Azure Active Directory**.
   
    ![Az Azure portál hitelesítési/engedélyezési panel](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. A hello **Azure Active Directory beállításai** paneljén kattintson **Express**
   
    ![Az Azure portál hitelesítési/engedélyezési Express beállítás](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)
   
    A hello **Express** beállítás, az App Service automatikusan létrehozhat egy Azure AD-alkalmazást az Azure ad [bérlői](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 
   
    Nincs toocreate a bérlőt, mert minden Azure-fiók automatikusan van egy.
6. A **üzemmód**, kattintson a **új AD-alkalmazás létrehozása** Ha nincs kiválasztva, és figyelje meg, amely hello hello érték **alkalmazás létrehozása** szövegmező; lesz meg az aad-ben a klasszikus Azure portálon később hello alkalmazás.
   
    ![Azure-portálon az Azure AD-beállítások](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)
   
    Azure automatikusan hoz létre egy Azure AD-alkalmazást, hello kijelzett nevet adja meg az Azure AD-bérlő. Alapértelmezés szerint az Azure AD-alkalmazást hello nevű hello ugyanaz, mint hello API alkalmazás. Tetszés szerint megadhat egy másik nevet.
7. Kattintson az **OK** gombra.
8. A hello **hitelesítési / engedélyezési** panelen kattintson a **mentése**.
   
    ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Most már csak az Azure AD-bérlő felhasználók meghívhatja hello API-alkalmazásba.

### <a name="optional-test-hello-api-app"></a>Választható lehetőség: Hello API-alkalmazás tesztelése
1. Egy böngészőben nyissa meg hello API-alkalmazás URL-CÍMÉT toohello: a hello **API-alkalmazás** panel az Azure-portálon hello alatt hello hivatkozásra **URL-cím**.  
   
    Átirányított tooa bejelentkezési képernyő áll, mert nem hitelesített kérelmek nem engedélyezettek tooreach hello API-alkalmazás.
   
    Ha a böngésző "sikeresen létrejött" toohello lapra kerül, hello böngésző már kerülhettek naplózásra – ebben az esetben, nyisson meg egy InPrivate vagy Incognito ablakot és nyissa meg toohello API-alkalmazás URL-CÍMÉT.
2. Jelentkezzen be az Azure AD-bérlő a felhasználó hitelesítő adataival.
   
    Ha van bejelentkezve, hello "sikeresen létrejött" lap hello böngészőben.
3. Bezárás hello böngésző.

### <a name="configure-hello-azure-ad-application"></a>Hello Azure AD alkalmazás konfigurálása
Ha konfigurálta az Azure AD-alapú hitelesítés, az App Service létre egy Azure AD-alkalmazást. Alapértelmezés szerint a hello új Azure AD-alkalmazás konfigurált tooprovide hello tulajdonosi jogkivonat toohello API-alkalmazás URL-cím volt. Ez a szakasz konfigurálja hello Azure AD alkalmazás tooprovide hello tulajdonosi jogkivonat toohello AngularJS előterének helyett közvetlenül toohello középső rétegbeli API-alkalmazást. hello AngularJS előtér által küldött hello token toohello API app meghívja hello API-alkalmazásba.

> [!NOTE]
> tookeep hello folyamat lehető legegyszerűbb, ez az oktatóanyag használja egy egyetlen Azure AD alkalmazás hello előtér és hello középső rétegbeli API-alkalmazást. Másik lehetőség is toouse két Azure AD-alkalmazások. Ebben az esetben az Azure AD-alkalmazást toogive hello előtér meg az Azure AD alkalmazás engedély toocall hello középső réteg kellene lennie. Ezt a módszert használja a több alkalmazást ad meg részletesebben vezérelhető, engedélyek tooeach réteg keresztül.
> 
> 

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), nyissa meg túl**Azure Active Directory**.
   
   Ezért rendelkezik toouse hello klasszikus portál bizonyos Azure Active Directory szükséges beállítások eléréséhez tooare még nem érhető el a hello Azure-portálon.
2. A hello **Directory** lapra, majd az AAD-bérlőt.
   
   ![A klasszikus portálon az Azure AD](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)
3. Kattintson a **alkalmazások > a vállalat tulajdonában lévő alkalmazások**, majd kattintson a pipa hello.
   
   Toorefresh hello lap toosee hello új alkalmazás is szükség lehet.
4. Az alkalmazások hello listában kattintson egy Azure hozott létre az Ön számára az API-alkalmazás hitelesítési engedélyezésekor hello hello nevét.
   
   ![Az Azure AD-alkalmazások lap](./media/app-service-api-dotnet-user-principal-auth/appstab.png)
5. Kattintson a **Configure** (Konfigurálás) elemre.
   
   ![Az Azure AD konfigurálása lap](./media/app-service-api-dotnet-user-principal-auth/configure.png)
6. Állítsa be **bejelentkezési URL-cím** toohello URL-cím, az AngularJS webalkalmazás, nem végződhet perjellel.
   
   Például: https://todolistangular.azurewebsites.net
   
   ![Bejelentkezési URL-cím](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)
7. Állítsa be **válasz URL-CÍMEN** toohello URL-címet a webalkalmazás, nem végződhet perjellel.
   
   Például: https://todolistsangular.azurewebsites.net
8. Kattintson a **Save** (Mentés) gombra.
   
   ![Válasz URL-címe](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)
9. Hello a hello lap alján, kattintson **kezelése jegyzékfájl > Letöltés jegyzékfájl**.
   
   ![Töltse le a jegyzékben](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)
10. Töltse le a hello tooa fájlhelyre ahol módosíthatja azt.
11. A letöltött hello jegyzékfájl, keresse meg a hello `oauth2AllowImplicitFlow` tulajdonság. Ezt a tulajdonságot hello értékének módosítása `false` túl`true`, majd mentse hello fájlt.
    
    Ez a beállítás a hozzáférést a a JavaScript egyoldalas alkalmazások szükség. Ez lehetővé teszi a hello Oauth 2.0 tulajdonosi jogkivonat toobe a hello URL-cím töredéket adott vissza.
12. Kattintson a **kezelése jegyzékfájl > feltöltés jegyzékfájl**, és hello fájlfeltöltés, amelyeket azért frissített, az előző lépésben hello.
    
    ![Töltse fel a jegyzékben](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)
13. Másolás hello **ügyfél-azonosító** értékét, és mentse valahol beszerezheti a később.

## <a name="configure-hello-todolistangular-project-toouse-authentication"></a>Hello ToDoListAngular projekt toouse hitelesítés konfigurálása
Ebben a szakaszban módosíthatja hello AngularJS előtér, hogy az Active Directory Authentication Library (ADAL) a JS tooacquire hello bejelentkezett felhasználó az Azure AD egy tulajdonosi jogkivonatot használ. hello kódot tartalmazza hello jogkivonat a középső réteg toohello, küldött HTTP-kérelmek hello a következő ábrán látható módon. 

![Felhasználói hitelesítés diagramja](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

Ellenőrizze a következő módosításokat toofiles hello ToDoListAngular projekt hello.

1. Nyissa meg hello *index.cshtml* fájlt.
2. Állítsa vissza az Active Directory Authentication Library (ADAL) hello JS parancsfájlok hivatkozó hello sorokat.
   
        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>
3. Nyissa meg hello *app/scripts/app.js* fájlt.
4. Hello kódblokkot megjegyzésbe "nélkül hitelesítés" megjelölve, és törölje a "-hitelesítés a" hello kódblokkot megjelölve.
   
    Ez a változás hello ADAL JS hitelesítésszolgáltatót és konfigurációs értékeket tooit. Az alábbi lépésekkel hello beállítása hello konfigurációs értékeket az API-alkalmazás és az Azure AD-alkalmazást.
5. Beállítja a hello hello kódban `endpoints` változó, beállíthatja hello API URL-címe toohello URL-címe hello API-alkalmazást, amely hello ToDoListAPI projekt készült, és állítsa be az Azure AD alkalmazás azonosítója toohello ügyfél-azonosító, melyet a klasszikus Azure portálon hello kimásolt hello.
   
    hello kód mostantól a következő példa hasonló toohello.
   
        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };
6. A hello hívása túl`adalProvider.init`, beállíthatja `tenant` tooyour bérlő neve és `clientId` hello előző lépésben használt toosame érték.
   
    hello kód mostantól a következő példa hasonló toohello.
   
        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );
   
    A módosítások túl`app.js` adja meg, hogy hello hívó kód és API hívása hello hello ugyanazt az Azure AD alkalmazás legyenek-e.
7. Nyissa meg hello *app/scripts/homeCtrl.js* fájlt.
8. Hello kódblokkot megjegyzésbe "nélkül hitelesítés" megjelölve, és törölje a "-hitelesítés a" hello kódblokkot megjelölve.
9. Nyissa meg hello *app/scripts/indexCtrl.js* fájlt.
10. Hello kódblokkot megjegyzésbe "nélkül hitelesítés" megjelölve, és törölje a "-hitelesítés a" hello kódblokkot megjelölve.

### <a name="deploy-hello-todolistangular-project-tooazure"></a>Hello ToDoListAngular projekt tooAzure telepítése
1. A **Megoldáskezelőben**, kattintson a jobb gombbal a ToDoListAngular projekt hello, és kattintson a **közzététel**.
2. Kattintson a **Publish** (Közzététel) gombra.
   
    Visual Studio telepíti hello projektet, és megnyitja a böngésző toohello webes alkalmazás alap URL-címet. Ez azt mutatja majd 403-as hiba lap, egy kísérlet toogo tooa Web API alap URL-CÍMÉT a böngésző normális.
   
    Továbbra is meg kell toomake egy módosítás toohello középső rétegbeli API-alkalmazás előtt hello alkalmazást tesztelheti.
3. Bezárás hello böngésző.

## <a name="configure-hello-todolistapi-project-toouse-authentication"></a>Hello ToDoListAPI projekt toouse hitelesítés konfigurálása
Jelenleg a ToDoListAPI projekt küld hello "*" hello, `owner` érték tooToDoListDataAPI. Ebben a szakaszban módosítsa hello kód toosend hello Azonosítót hello bejelentkezett felhasználó.

Ellenőrizze a következő módosításokat hello ToDoListAPI projektben hello.

1. Nyissa meg hello *Controllers/ToDoListController.cs* fájlt, és állítsa vissza az egyes tartozó műveleti módszer, amely beállítja a hello sor `owner` toohello az Azure AD `NameIdentifier` jogcím értéke. Példa:
   
        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;
   
    **Fontos**: ne állítsa vissza a hello kód `ToDoListDataAPI` metódus; el kell végeznie, amelyek később hello szolgáltatás egyszerű hitelesítés az oktatóanyaghoz.

### <a name="deploy-hello-todolistapi-project-tooazure"></a>Hello ToDoListAPI projekt tooAzure telepítése
1. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello ToDoListAPI projektet, és kattintson **közzététel**.
2. Kattintson a **Publish** (Közzététel) gombra.
   
    Visual Studio telepíti hello projektet, és megnyitja a böngésző toohello API-alkalmazás alap URL-címet.
3. Bezárás hello böngésző.

### <a name="test-hello-application"></a>Hello alkalmazás tesztelése
1. Nyissa meg hello webalkalmazás, toohello URL-címe **HTTPS-kapcsolaton keresztül, nem HTTP**.
2. Kattintson a hello **tooDo lista** fülre.
   
    A kért toolog áll.
3. Jelentkezzen be az AAD-bérlőben felhasználó hello hitelesítő adatait.
4. Hello **tooDo lista** lap jelenik meg.
   
   ![tooDo lista lap](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)
   
   Nincs tennivaló jelennek meg, mert eddig összes voltak tulajdonosa "*". Most hello középső réteg által kért elemek hello bejelentkezett felhasználó számára, és nincs még létrejött.
5. Vegyen fel új hello alkalmazás működik-e tennivaló elemek tooverify.
6. Egy másik böngészőablakban, lépjen a Swagger felhasználói felület URL-cím toohello hello ToDoListDataAPI API-alkalmazás, és kattintson **ToDoList > Get**. Adjon meg csillagot hello `owner` paraméter, és kattintson **próbálja ki**.
   
   hello válasz azt mutatja, hogy új teendőelemet hello hello tényleges az Azure AD felhasználói azonosító hello tulajdonos tulajdonságában.
   
   ![JSON-NÁ válaszul tulajdonos azonosítója](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)

## <a name="building-hello-projects-from-scratch"></a>Teljesen új hello projektek felépítése
hello két Web API-projektet hello használatával létrehozott **Azure API App** projektek sablon és a ToDoList vezérlő tagjára hello alapértelmezett értékek vezérlő. 

Információ a túl hozzon létre egy AngularJS egyoldalas alkalmazást a Web API 2 háttérből, lásd: [beavatkozás nélküli a labor: egy egyetlen oldal alkalmazás (SPA) az ASP.NET Web API és Angular.js Build](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). További információ tooadd az Azure AD hitelesítési kódot, tekintse meg a következő erőforrások hello:

* [Az Azure AD-alkalmazások biztonságossá tétele az AngularJS egyetlen lapon](../active-directory/active-directory-devquickstarts-angular.md).
* [A V1-es ADAL JS bemutatása](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Hibaelhárítás
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Győződjön meg arról, hogy azt ne tévessze össze ToDoListAPI (középső réteg) és a ToDoListDataAPI (adatrétegbeli). Például győződjön meg arról, hogy hozzáadta a hitelesítési toohello középső rétegbeli API-alkalmazást, nem hello adatrétegbeli. 
* Győződjön meg arról, hogy a hello AngularJS forrás kód hivatkozások hello középső rétegbeli API alkalmazás URL-CÍMÉT (ToDoListAPI, ToDoListDataAPI nem) és hello megfelelő ügyfél-azonosítót az Azure AD. 

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban megtanulta, hogyan toouse API-alkalmazás az App Service hitelesítés, és hogyan toocall hello segítségével API-alkalmazás hello ADAL JS kódtárat. Az oktatóanyag következő hello megtudhatja, hogyan túl[biztonságos hozzáférést tooyour API app service-to-service forgatókönyvek](app-service-api-dotnet-service-principal-auth.md).

