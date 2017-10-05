---
title: "Az Azure App Service API-alkalmazások felhasználói hitelesítésének |} Microsoft Docs"
description: "Ismerje meg, hogyan védi az Azure App Service API-alkalmazás hozzáférés csak a hitelesített felhasználókhoz."
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
ms.openlocfilehash: a5b713f3a60b3886d813438496d704f464d914e6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Az Azure App Service API-alkalmazások felhasználói hitelesítésének
## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja, hogyan védi az Azure API-alkalmazások, hogy csak a hitelesített felhasználók is hívják. A cikk feltételezi, hogy elolvasta a [Azure App Service hitelesítés áttekintése](../app-service/app-service-authentication-overview.md).

Az oktatóanyagból a következőket sajátíthatja el:

* Hogyan konfigurálható egy hitelesítésszolgáltatót, az Azure Active Directory (Azure AD) adatokkal.
* Hogyan használják a védett API-alkalmazások a [Active Directory Authentication Library (ADAL) a JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).

A cikk két részből áll:

* A [felhasználói hitelesítés beállítása az Azure App Service](#authconfig) szakasz általánosságban ismerteti bármely API-alkalmazások felhasználói hitelesítésének konfigurálása, és minden keretrendszerek App Service által támogatott keretrendszerre alkalmazható többek között a .NET, Node.js, és Java.
* Kezdve a [a .NET API-alkalmazások oktatóanyagok folytatása](#tutorialstart) szakaszban, a cikk végigvezeti a felhasználót, hogy az Azure Active Directory felhasználó számára a .NET háttérből és egy AngularJS előtér mintaalkalmazás konfigurálása hitelesítés. 

## <a id="authconfig"></a>Felhasználói hitelesítés beállítása az Azure App Service-ben
Ez a témakör általános API-alkalmazásra vonatkozó utasításokat. A lépések az adott való tegye lista .NET mintaalkalmazás, lásd [a .NET-bevezető oktatóanyagok folytatása](#tutorialstart).

1. Az a [Azure-portálon](https://portal.azure.com/), keresse meg a **beállítások** panelen az API-alkalmazás, amelyet szeretne védeni, keresse meg a **szolgáltatások** szakaszt, és kattintson a  **Hitelesítési / engedélyezési**.
   
    ![Azure-portálon hitelesítési/engedélyezési](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. Az a **hitelesítési / engedélyezési** panelen kattintson a **a**.
3. A beállítások valamelyikét kell választani a **hitelesítetlen kérés esetén elvégzendő művelet** legördülő listából.
   
   * Ha azt szeretné, hogy csak hitelesített hívásokat az API-alkalmazás eléréséhez, válasszon egyet az a **jelentkezzen be az...**  beállítások. Ez a beállítás lehetővé teszi az API-alkalmazások védelmét az azt futtató programozás nélkül.
   * Ha azt szeretné elérni az API-alkalmazás összes hívások, **kérés engedélyezése (intézkedés)**. Ez a beállítás segítségével egy kiválasztott hitelesítésszolgáltatók nem hitelesített hívóknak közvetlen. Ezzel a beállítással rendelkezik írhat kódot az engedélyezés kezeléséhez.
     
     További információ: [Az API Apps hitelesítése és engedélyezése az Azure App Service-ben](app-service-api-authentication.md#multiple-protection-options).
4. Válasszon ki egy vagy több a **hitelesítésszolgáltatókat**.
   
    A képen látható összes hívó számára az Azure AD hitelesíti igénylő lehetőségek.
   
    ![Az Azure portál hitelesítési/engedélyezési panel](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
   
    Ha úgy dönt, hogy egy hitelesítésszolgáltató, a portál megjeleníti a konfigurációs panel az adott szolgáltató. 
   
    Azt ismertetik, hogyan konfigurálhatja a hitelesítési szolgáltató konfigurációs paneleken részletes útmutatás: [konfigurálása az App Service alkalmazás használhatja az Azure Active Directory bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (A hivatkozás az Azure AD ismertető cikkben ugrik, de magát a cikket, amelyek cikkekhez tartozó más hitelesítésszolgáltatók lapot tartalmaz,.)
5. Ha a hitelesítési szolgáltató konfigurációs panelen végzett, kattintson **OK**.
6. Az a **hitelesítési / engedélyezési** panelen kattintson a **mentése**.

Ebben az esetben, ha az App Service API-hívások hitelesíti, az API-alkalmazás elérése előtti. A hitelesítési szolgáltatások azonos az összes, az App service támogat, beleértve a .NET, Node.js és Java nyelven működik. 

Ahhoz, hogy a hitelesített API-hívások, a hívó tartalmazza a hitelesítésszolgáltató OAuth 2.0 tulajdonosi jogkivonattal a HTTP-kérések hitelesítési fejlécéhez. A token szerezhető: a hitelesítésszolgáltató SDK használatával.

## <a id="tutorialstart"></a>A .NET API-alkalmazások oktatóanyagok folytatása
Ha a Node.js vagy Java API-alkalmazások oktatóanyag, ugorjon a következő cikk [szolgáltatás egyszerű hitelesítési API-alkalmazások](app-service-api-dotnet-service-principal-auth.md). 

Ha a .NET API-alkalmazások oktatóanyag adatsorozat követi, és már telepítették a mintaalkalmazás megfelelően a [első](app-service-api-dotnet-get-started.md) és [második](app-service-api-cors-consume-javascript.md) oktatóanyagokat, ugorjon a [beállítása az App Service és az Azure AD hitelesítési](#azureauth) szakasz.

Ha azt szeretné, hogy ez az oktatóanyag az első és második oktatóanyagok áthaladás nélkül, hajtsa végre az alábbi lépéseket, melyek azt ismertetik, hogyan lásson a mintaalkalmazás telepítése egy automatikus folyamat segítségével.

> [!NOTE]
> Az alábbi lépéseket megkeresheti az azonos kiindulási pont, mintha csak az első két oktatóanyagok, egy kivétellel tette: Visual Studio már nem tudja, melyik webalkalmazás vagy API-alkalmazás, amelynek minden projekt telepítése lekérdezi. Ez azt jelenti, hogy emiatt nem tud pontos útmutatásokat ebben az oktatóanyagban a megfelelő tárolók üzembe helyezése. Ha nem tudja az üzembe helyezés lépései alapján saját módjáról, a Feladatkezelő, érdemes az oktatóanyag adatsorozat kövesse a [első oktatóanyaga, amely](app-service-api-dotnet-get-started.md) mint kezdődnie Ez automatikus központi telepítési folyamat.
> 
> 

1. Győződjön meg arról, hogy rendelkezik az összes felsorolt Előfeltételek a [első oktatóanyaga, amely](app-service-api-dotnet-get-started.md). Mellett az előfeltételeket a hitelesítési oktatóanyagok azt feltételezik, hogy dolgozott App Service web apps és az API apps a Visual Studio és az Azure-portálon.
2. Kattintson a **az Azure telepítéséhez** gombra a [To Do List minta tárház információs fájl](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) az API apps és a webes alkalmazás központi telepítését. Jegyezze fel az Azure erőforráscsoport, amely lekérdezi létrehozni, mert ezzel újabb ellenőrizzék a web app és az API-alkalmazás nevét.
3. Töltse le, vagy klónozza a [To Do List minta tárház](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) a kódot, amely meg fog dolgozni helyileg a Visual Studio segítségével.

## <a id="azureauth"></a>Az App Service és az Azure AD hitelesítési beállítása
Most már rendelkezik anélkül, hogy a felhasználók hitelesítése az Azure App Service-ben futó alkalmazáshoz. Ez a szakasz a hitelesítés a következő feladatok végrehajtásával hozzáadása:

* Azure Active Directory (Azure AD-) hitelesítés megkövetelése hívásakor a középső rétegbeli API-alkalmazást az App Service konfigurálása.
* Hozzon létre egy Azure AD-alkalmazást.
* Az Azure AD alkalmazás bejelentkezést követően a tulajdonosi jogkivonattal küldeni az AngularJS előtér beállítása. 

Ha problémát tapasztal az oktatóanyag utasításait követve közben, olvassa el a [hibaelhárítás](#troubleshooting) szakasz az oktatóanyag végén. 

### <a name="configure-authentication-for-the-middle-tier-api-app"></a>A középső réteg API-alkalmazás-hitelesítés konfigurálása
1. Az a [Azure-portálon](https://portal.azure.com/), keresse meg a **beállítások** a ToDoListAPI projekt létrehozott API-alkalmazás paneljén található a **szolgáltatások** szakaszt, és kattintson a  **Hitelesítési / engedélyezési**.
   
    ![Azure-portálon hitelesítési/engedélyezési](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. Az a **hitelesítési / engedélyezési** panelen kattintson a **a**.
3. Az a **hitelesítetlen kérés esetén elvégzendő művelet** legördülő listában válassza **bejelentkezés Azure Active Directoryval**.
   
    Ez a beállítás biztosítja, hogy unauathenticated kérelmek nem elérje az API-alkalmazásba. 
4. A **hitelesítésszolgáltatókat**, kattintson a **Azure Active Directory**.
   
    ![Az Azure portál hitelesítési/engedélyezési panel](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. Az a **Azure Active Directory beállításai** paneljén kattintson **Express**
   
    ![Az Azure portál hitelesítési/engedélyezési Express beállítás](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)
   
    Az a **Express** beállítás, az App Service automatikusan létrehozhat egy Azure AD-alkalmazást az Azure ad [bérlői](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 
   
    Nem rendelkezik a bérlő létrehozására, mert minden Azure-fiók automatikusan van egy.
6. A **üzemmód**, kattintson a **új AD-alkalmazás létrehozása** Ha nincs kiválasztva, és figyelje meg az értéket, amely a a **alkalmazás létrehozása** szövegmező; lesz meg az aad-ben alkalmazás a klasszikus Azure portálon később.
   
    ![Azure-portálon az Azure AD-beállítások](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)
   
    Azure automatikusan létrehoz egy Azure AD-alkalmazást az Azure AD-bérlő jelzett nevű. Alapértelmezés szerint az Azure AD-alkalmazás neve megegyezik az API-alkalmazásba. Tetszés szerint megadhat egy másik nevet.
7. Kattintson az **OK** gombra.
8. Az a **hitelesítési / engedélyezési** panelen kattintson a **mentése**.
   
    ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Most már csak az Azure AD-bérlő felhasználók meghívhatja az API-alkalmazásba.

### <a name="optional-test-the-api-app"></a>Választható lehetőség: Az API-alkalmazás tesztelése
1. Egy böngészőben nyissa meg az URL-címet a API-alkalmazás: az a **API-alkalmazás** panel az Azure-portálon kattintson a hivatkozásra kattintva **URL-cím**.  
   
    Ekkor megnyílik egy bejelentkezési képernyő, mert nem hitelesített kérelmek nem engedélyezettek az API-alkalmazás eléréséhez.
   
    Ha a böngésző a "sikeres létrehozásról" lapra ugrik, a böngésző már kerülhettek naplózásra a – ebben az esetben, nyisson meg egy InPrivate vagy Incognito ablakot, és navigáljon az API-alkalmazás URL-CÍMÉT.
2. Jelentkezzen be az Azure AD-bérlő a felhasználó hitelesítő adataival.
   
    Ha van bejelentkezve, a "sikeres létrehozásról" tájékoztató oldal jelenik meg a böngészőben.
3. Zárja be a böngészőt.

### <a name="configure-the-azure-ad-application"></a>Az Azure AD-alkalmazás konfigurálása
Ha konfigurálta az Azure AD-alapú hitelesítés, az App Service létre egy Azure AD-alkalmazást. Az új Azure AD alapértelmezés szerint az alkalmazás a tulajdonosi jogkivonat biztosításához az API-alkalmazás URL-cím lett beállítva. Ebben a szakaszban konfigurálása arra, hogy a tulajdonosi jogkivonatot az AngularJS előtér ahelyett, hogy közvetlenül a középső rétegbeli API-alkalmazást az Azure AD-alkalmazást. Az AngularJS előtér visszaküldi a jogkivonatot az API-alkalmazásba Ha meghívja az API-alkalmazásba.

> [!NOTE]
> A folyamat tartsa a lehető egyszerű, a oktatóanyag használ egy egyetlen Azure AD alkalmazás az előtér- és a középső réteg API-alkalmazásba. Egy másik lehetőség, hogy két Azure AD-alkalmazások használja. Ebben az esetben kellene ahhoz, hogy megkapja az előtér az Azure AD alkalmazás számára a középső réteg az Azure AD-alkalmazást hívja. Ezt a módszert használja a több alkalmazást ad meg az egyes rétegek engedélyek több részletes szabályozását.
> 
> 

1. Az a [a klasszikus Azure portálon](https://manage.windowsazure.com/), és **Azure Active Directory**.
   
   Akkor használja a klasszikus portálra, mert bizonyos Azure Active Directory-beállítások, amelyek hozzá kell férnie még nem állnak rendelkezésre az Azure-portálon.
2. Az a **Directory** lapra, majd az AAD-bérlőt.
   
   ![A klasszikus portálon az Azure AD](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)
3. Kattintson a **alkalmazások > a vállalat tulajdonában lévő alkalmazások**, majd kattintson a pipa jelre.
   
   Is előfordulhat, hogy frissítse a lapot, melyen megtekintheti az új alkalmazás.
4. Az alkalmazások listájának megtekintéséhez kattintson az Azure létrehozó meg, ha engedélyezte a hitelesítés az API-alkalmazás neve.
   
   ![Az Azure AD-alkalmazások lap](./media/app-service-api-dotnet-user-principal-auth/appstab.png)
5. Kattintson a **Configure** (Konfigurálás) elemre.
   
   ![Az Azure AD konfigurálása lap](./media/app-service-api-dotnet-user-principal-auth/configure.png)
6. Állítsa be **bejelentkezési URL-cím** URL-címet a AngularJS webalkalmazás, nem végződhet perjellel.
   
   Például: https://todolistangular.azurewebsites.net
   
   ![Bejelentkezési URL-cím](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)
7. Állítsa be **válasz URL-CÍMEN** URL-címet a webalkalmazás, nem végződhet perjellel.
   
   Például: https://todolistsangular.azurewebsites.net
8. Kattintson a **Save** (Mentés) gombra.
   
   ![Válasz URL-címe](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)
9. Kattintson a lap alján **kezelése jegyzékfájl > Letöltés jegyzékfájl**.
   
   ![Töltse le a jegyzékben](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)
10. Töltse le a fájlt olyan helyre, ahol módosíthatja azt.
11. A letöltött jegyzékfájl, keresse meg a `oauth2AllowImplicitFlow` tulajdonság. Ez a tulajdonság értékének módosítása `false` való `true`, majd mentse a fájlt.
    
    Ez a beállítás a hozzáférést a a JavaScript egyoldalas alkalmazások szükség. Lehetővé teszi a Oauth 2.0 tulajdonosi jogkivonatot az URL-töredéket adott vissza.
12. Kattintson a **kezelése jegyzékfájl > feltöltés jegyzékfájl**, és feltölteni a fájlt, hogy az előző lépésben.
    
    ![Töltse fel a jegyzékben](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)
13. Másolás a **ügyfél-azonosító** értékét, és mentse valahol beszerezheti a később.

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>Hitelesítés használatára a ToDoListAngular projekt konfigurálása
Ebben a szakaszban módosíthatja az AngularJS előtér, hogy a bejelentkezett felhasználó az Azure AD egy tulajdonosi jogkivonatot szerezni az Active Directory Authentication Library (ADAL) a JS használja. A kódot tartalmazza a jogkivonatot a HTTP-kérések a középső réteg küldött az alábbi ábrán látható módon. 

![Felhasználói hitelesítés diagramja](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

A következő módosításokat a ToDoListAngular projekthez a fájlokat.

1. Nyissa meg a *index.cshtml* fájlt.
2. Állítsa vissza az Active Directory Authentication Library (ADAL) JS parancsfájlok hivatkozó sorokat.
   
        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>
3. Nyissa meg a *app/scripts/app.js* fájlt.
4. A "-hitelesítés nélkül" megjelölve kódblokkot megjegyzéssé, és állítsa vissza a kódblokkot az "a hitelesítés" jelölésű.
   
    Ez a változás az ADAL JS hitelesítésszolgáltatót és konfigurációs értékeket. A következő lépésekben állítsa be a konfigurációs értékeket az API-alkalmazás és az Azure AD-alkalmazást.
5. A kódban, amely beállítja a `endpoints` változót, az API-alkalmazás URL-címet a API URL-Címének beállítása a létre a ToDoListAPI projektet, és az ügyfél-azonosító, melyet a klasszikus Azure portálon kimásolt beállítása az Azure AD alkalmazás-azonosítója.
   
    A kód mostantól az alábbi példához hasonló.
   
        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };
6. A hívásban `adalProvider.init`, beállíthatja `tenant` a bérlő neve és `clientId` ugyanarra az előző lépésben használt értékre.
   
    A kód mostantól az alábbi példához hasonló.
   
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
   
    A módosítások a `app.js` adja meg, hogy a hívó kód és a hívott API legyenek-e az azonos Azure AD-alkalmazás.
7. Nyissa meg a *app/scripts/homeCtrl.js* fájlt.
8. A "-hitelesítés nélkül" megjelölve kódblokkot megjegyzéssé, és állítsa vissza a kódblokkot az "a hitelesítés" jelölésű.
9. Nyissa meg a *app/scripts/indexCtrl.js* fájlt.
10. A "-hitelesítés nélkül" megjelölve kódblokkot megjegyzéssé, és állítsa vissza a kódblokkot az "a hitelesítés" jelölésű.

### <a name="deploy-the-todolistangular-project-to-azure"></a>A ToDoListAngular projekt telepítése az Azure-bA
1. A **Solution Explorer** (Megoldáskezelő) területén kattintson a jobb gombbal a ToDoListAngular projektre, majd kattintson a **Publish** (Közzététel) elemre.
2. Kattintson a **Publish** (Közzététel) gombra.
   
    A Visual Studio telepíti a projektet, és a webes alkalmazás alap URL-cím egy böngészőablakban megnyitja. Ez azt mutatja majd 403-as hibalap, ez nem jelent problémát, a Web API alap URL-címet egy böngészőben nyissa tett kísérlet.
   
    Továbbra is meg kell az alkalmazást tesztelheti előtt lehet módosítani a középső réteg API-alkalmazás.
3. Zárja be a böngészőt.

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>Hitelesítés használatára a ToDoListAPI projekt konfigurálása
Jelenleg a ToDoListAPI projekt küld "*", a `owner` ToDoListDataAPI értéket. Ebben a szakaszban módosítsa a kód küldése a bejelentkezett felhasználó Azonosítóját.

A következő módosításokat a ToDoListAPI projektben.

1. Nyissa meg a *Controllers/ToDoListController.cs* fájlt, és állítsa vissza a sor minden tartozó műveleti módszer, amely beállítja a `owner` az Azure ad `NameIdentifier` jogcím értéke. Példa:
   
        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;
   
    **Fontos**: ne állítsa vissza a kód a `ToDoListDataAPI` metódus; el kell végeznie, amely később a szolgáltatás egyszerű hitelesítés az oktatóanyaghoz.

### <a name="deploy-the-todolistapi-project-to-azure"></a>A ToDoListAPI projekt telepítése az Azure-bA
1. A **Megoldáskezelőben**, kattintson a jobb gombbal a ToDoListAPI projektet, és kattintson a **közzététel**.
2. Kattintson a **Publish** (Közzététel) gombra.
   
    A Visual Studio telepíti a projektet, és az API-alkalmazás alap URL-cím egy böngészőablakban megnyitja.
3. Zárja be a böngészőt.

### <a name="test-the-application"></a>Az alkalmazás tesztelése
1. Nyissa meg a webes alkalmazás URL-címét **HTTPS-kapcsolaton keresztül, nem HTTP**.
2. Kattintson a **To Do List** fülre.
   
    Jelentkezzen be kéri.
3. Jelentkezzen be az AAD-bérlőben egy olyan felhasználó hitelesítő adataival.
4. A **To Do List** lap jelenik meg.
   
   ![Tennivalók listája lap](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)
   
   Nincs tennivaló jelennek meg, mert eddig összes voltak tulajdonosa "*". A középső réteg most kér a bejelentkezett felhasználó elemeket, és nincs még létrejött.
5. Vegyen fel új tennivaló, hogy az alkalmazás működésének ellenőrzéséhez.
6. Egy másik böngészőablakban, nyissa meg a Swagger felhasználói felület URL-címe a ToDoListDataAPI API-alkalmazást, és kattintson a **ToDoList > Get**. Írjon be csillag karaktert a `owner` paraméter, és kattintson **próbálja ki**.
   
   A válasz azt mutatja, hogy új teendőelemet a tényleges az Owner tulajdonság az Azure AD felhasználói Azonosítóval.
   
   ![JSON-NÁ válaszul tulajdonos azonosítója](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)

## <a name="building-the-projects-from-scratch"></a>Teljesen új projektek felépítése
A két Web API-projektek használatával létrehozott a **Azure API App** projektek sablon és az alapértelmezett értékek vezérlő lecserélését egy ToDoList tartományvezérlőre. 

Hozzon létre egy AngularJS egyoldalas alkalmazást a Web API 2 háttérből kapcsolatos információkért lásd: [beavatkozás nélküli a labor: egy egyetlen oldal alkalmazás (SPA) az ASP.NET Web API és Angular.js Build](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Az Azure AD hitelesítési kód adásával kapcsolatos információkért lásd a következőket:

* [Az Azure AD-alkalmazások biztonságossá tétele az AngularJS egyetlen lapon](../active-directory/active-directory-devquickstarts-angular.md).
* [A V1-es ADAL JS bemutatása](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Hibaelhárítás
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Győződjön meg arról, hogy azt ne tévessze össze ToDoListAPI (középső réteg) és a ToDoListDataAPI (adatrétegbeli). Például győződjön meg arról, hogy a középső rétegbeli API-alkalmazást, nem az adatréteghez tartozó hitelesítési felvette. 
* Győződjön meg arról, hogy az AngularJS forráskód hivatkozik-e a középső réteg API alkalmazás URL-CÍMÉT (ToDoListAPI, ToDoListDataAPI nem) és a megfelelő Azure AD ügyfél-azonosítót. 

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban megismerte az API-alkalmazás az App Service authentication használatával, és hogyan hívhatja meg az API-alkalmazás az ADAL-JS kódtárat használatával. A következő oktatóanyag megtudhatja, hogyan [biztonságos hozzáférés az API-alkalmazás a szolgáltatások közötti forgatókönyvek](app-service-api-dotnet-service-principal-auth.md).

