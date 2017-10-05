---
title: "Az Azure App Service API Apps szolgáltatás egyszerű hitelesítéssel |} Microsoft Docs"
description: "Ismerje meg, hogyan védi meg a szolgáltatások közötti forgatókönyvek az Azure App Service API-alkalmazás."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 7ca0bab2-1d29-4d51-b779-dce0edd34f8b
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: alkarche
ms.openlocfilehash: 95653287546bbe358111ed16af0c30a53caff2b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Az Azure App Service API Apps szolgáltatás egyszerű hitelesítés
## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan App Service hitelesítés *belső* API-alkalmazások elérésére. Egy belső forgatókönyv, csak a saját alkalmazáskód felhasználhatóvá kell használni kívánt API-alkalmazások esetében. Az ajánlott módszer az App Service-ben Ez a forgatókönyv végrehajtásához az Azure AD használatára a hívott API-alkalmazás védelme érdekében. Meghívja az alkalmazásidentitás (egyszerű szolgáltatásnév) hitelesítő adatok megadásával az Azure AD-ből kapott tulajdonosi jogkivonatok védett API-alkalmazásba. Más Azure AD, tekintse meg a **szolgáltatások közötti hitelesítési** szakasza a [Azure App Service hitelesítés áttekintése](../app-service/app-service-authentication-overview.md#service-to-service-authentication).

Ebből a cikkből megtudhatja:

* Hogyan illetéktelen hozzáférés elleni védelmének API-alkalmazás az Azure Active Directory (Azure AD) segítségével.
* Hogyan védett API-alkalmazás egy API-alkalmazást, a webes alkalmazás vagy a mobilalkalmazás használják az Azure AD szolgáltatás rendszerbiztonsági tag (app-identitás) hitelesítő adatait. A logikai alkalmazás felhasználása kapcsolatos információkért lásd: [az App Service a Logic apps üzemeltetett egyéni API használata](../logic-apps/logic-apps-custom-hosted-api.md).
* Győződjön meg arról, hogy a védett API-alkalmazás nem lehet meghívni egy böngészőből bejelentkezett felhasználók módjáról.
* Győződjön meg arról, hogy a védett API-alkalmazás csak akkor hívható meg az adott Azure AD szolgáltatás egyszerű módja.

A cikk két részből áll:

* A [szolgáltatás egyszerű hitelesítés konfigurálása az Azure App Service](#authconfig) a szakasz a hitelesítés az API-alkalmazások konfigurálása, és a védett API-alkalmazások felhasználása általában ismerteti. Ez a szakasz App Service, beleértve a .NET, Node.js és Java által támogatott összes keretrendszerek egyaránt vonatkozik.
* Kezdve a [a .NET-bevezető oktatóanyagok folytatása](#tutorialstart) szakaszban az oktatóanyag végigvezeti Önt egy "belső hozzáférés" forgatókönyvet, az App Service-ben futó .NET mintaalkalmazás konfigurálása. 

## <a id="authconfig"></a>Az Azure App Service szolgáltatás egyszerű hitelesítés konfigurálása
Ez a témakör általános API-alkalmazásra vonatkozó utasításokat. A lépések az adott való tegye lista .NET mintaalkalmazás, lásd [a .NET API-alkalmazások útmutató-sorozat folytatása](#tutorialstart).

1. Az a [Azure-portálon](https://portal.azure.com/), keresse meg a **beállítások** panelen az API-alkalmazás, amelyet szeretne védeni, és keresse meg a **szolgáltatások** szakaszt, és kattintson **hitelesítési / engedélyezési**.
   
    ![Hitelesítési/engedélyezési Azure-portálon](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. Az a **hitelesítési / engedélyezési** panelen kattintson a **a**.
3. Az a **hitelesítetlen kérés esetén elvégzendő művelet** legördülő listában válassza **bejelentkezés Azure Active Directoryval** .
4. A **hitelesítésszolgáltatókat**, jelölje be **Azure Active Directory**.
   
    ![Hitelesítési/engedélyezési panel az Azure-portálon](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. Konfigurálja a **Azure Active Directory beállításai** hozzon létre egy új panel az Azure AD-alkalmazást, vagy ha már rendelkezik egy használni kívánt, meglévő Azure AD alkalmazás használja.
   
    Belső forgatókönyvek általában tartalmaz, amely az API-alkalmazást hívja az API-alkalmazások. Használhatja a külön Azure AD-alkalmazásokat minden API-alkalmazás vagy egy Azure AD-alkalmazást.
   
    A panel részletes utasításokért lásd: [konfigurálása az App Service alkalmazás használhatja az Azure Active Directory bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).
6. Ha a hitelesítési szolgáltató konfigurációs panelen végzett, kattintson **OK**.
7. Az a **hitelesítési / engedélyezési** panelen kattintson a **mentése**.
   
    ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Ebben az esetben, ha az App Service csak lehetővé teszi a kérelmek a beállított a hívók az Azure AD-bérlő. Nincs hitelesítési vagy engedélyezési kód szükséges a védett API-alkalmazás. Az API-alkalmazásba, általában együtt a tulajdonosi jogkivonattal a HTTP-fejlécekben használt jogcímek átadását és érheti el ezt az információt a kódot, hogy ellenőrizze, hogy vannak-e kérelmek egy adott hívó, például egy egyszerű szolgáltatást.

Ez a hitelesítési szolgáltatás működik, mint az összes, az App service támogat, beleértve a .NET, Node.js és Java nyelven. 

#### <a name="how-to-consume-the-protected-api-app"></a>Hogyan kell a védett API-alkalmazások felhasználása
A hívónak biztosítania kell egy Azure AD-tulajdonosi jogkivonattal API-hívások. Ahhoz, hogy a szolgáltatás egyszerű hitelesítő adataival tulajdonosi jogkivonattal, a hívó által használt Active Directory Authentication Library (adal-t a [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs), vagy [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). Egy token beszerzéséhez az adal-t meghívó kód az adal-ra a következő adatokat tartalmazza:

* Az Azure AD-bérlő neve.
* Az ügyfél-azonosító és a titkos ügyfélkódot (Alkalmazáskulcs) az Azure AD-alkalmazás a hívó társított.
* Az Azure AD-alkalmazás a védett API-alkalmazáshoz tartozó ügyfél-azonosító. (Ha csak egy Azure AD-alkalmazást használ, ez az ügyfél azonosítója megegyezik azzal a hívónak.)

Ezeket az értékeket az Azure AD lapján érhetők el a [a klasszikus Azure portálon](https://manage.windowsazure.com/).

A token beszerzett, ha a hívó tartalmazza a HTTP-kérések a hitelesítési fejléc.  App Service érvényesíti a jogkivonatot, és lehetővé teszi a kérelmek a védett API-alkalmazás eléréséhez.

#### <a name="how-to-protect-the-api-app-from-access-by-users-in-the-same-tenant"></a>Az API-alkalmazás a felhasználók ugyanazzal a bérlővel védelme
A felhasználók ugyanazzal a bérlővel tulajdonosi jogkivonatok érvényesek védett API-alkalmazás.  Ha azt szeretné, annak érdekében, hogy csak egy egyszerű meghívhatja a védett API-alkalmazás, adja hozzá a kódot a védett API-alkalmazás a következő jogcímeket a jogkivonatból ellenőrzése:

* `appid`az ügyfél-Azonosítóját az Azure AD-alkalmazás, amely a hívó társított kell lennie. 
* `oid`(`objectidentifier`) kell lennie a hívó szolgáltatás résztvevő-azonosító. 

App Service emellett a `objectidentifier` jogcím X-MS-CLIENT-tag-ID fejlécében.

### <a name="how-to-protect-the-api-app-from-browser-access"></a>Hogyan védi meg a böngésző-hozzáférés az API-alkalmazás
Ha Ön nem ellenőrizhesse a kódban a védett API-alkalmazás, és külön használhatja az Azure AD alkalmazás védett API-alkalmazás, ügyeljen arra, hogy az Azure AD-alkalmazás válasz URL-CÍMEN nem ugyanaz, mint az API-alkalmazás alap URL-címet. Ha a válasz URL-címet közvetlenül a védett API-alkalmazás mutat, az azonos Azure AD-bérlő a felhasználó képes keresse meg az API-alkalmazás, jelentkezzen be, és sikeresen hívja az API-t.

## <a id="tutorialstart"></a>A .NET API-alkalmazások útmutató-sorozat folytatása
Ha a Node.js vagy Java oktatóanyag sorozat API-alkalmazások, ugorjon a [további lépések](#next-steps) szakasz. 

Az a cikk hátralévő része a .NET API-alkalmazások útmutató-sorozat folytatja, és feltételezi, hogy végrehajtotta a [felhasználói hitelesítési oktatóanyag](app-service-api-dotnet-user-principal-auth.md) , és a felhasználói hitelesítés engedélyezve van az Azure-ban futó mintaalkalmazást.

## <a name="set-up-authentication-in-azure"></a>Az Azure-ban hitelesítés beállítása
Ebben a szakaszban megadhatja az App Service, hogy csak a HTTP-kérelmek az adatréteg API-alkalmazásának eléréséhez, akkor azok, amelyek érvényes Azure AD tulajdonosi jogkivonatoknak nevezzük. 

A következő szakaszban a középső réteg API-alkalmazás alkalmazás hitelesítő adatok küldése az Azure AD vissza egy tulajdonosi jogkivonatot és a tulajdonosi jogkivonattal küldeni az adatréteg API-alkalmazásának konfigurálásához. Ez a folyamat az az ábrán látható.

![Szolgáltatás-hitelesítés diagramja](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Ha problémát tapasztal az oktatóanyag utasításait követve közben, olvassa el a [hibaelhárítás](#troubleshooting) szakasz az oktatóanyag végén. 

1. A a [Azure-portálon](https://portal.azure.com/), keresse meg a **beállítások** ToDoListDataAPI (adatok réteg) API-alkalmazás létrehozása, és kattintson az API-alkalmazás paneljén **beállítások**.
2. Az a **beállítások** panelen található az **szolgáltatások** szakaszt, és kattintson a **hitelesítési / engedélyezési**.
   
    ![Hitelesítési/engedélyezési Azure-portálon](./media/app-service-api-dotnet-user-principal-auth/features.png)
3. Az a **hitelesítési / engedélyezési** panelen kattintson a **a**.
4. Az a **hitelesítetlen kérés esetén elvégzendő művelet** legördülő listában válassza **bejelentkezés Azure Active Directoryval**.
   
    Ez a beállítás hatására az App Service annak érdekében, hogy csak hitelesített kérelmek reach az API-alkalmazásba. Érvényes tulajdonosi jogkivonatok rendelkező kéréseknél az App Service mentén jogkivonatok átadása az API-alkalmazás tölti fel ezt az információt könnyebben számára elérhetővé tenni a kódot a gyakran használt JOGCÍMEKKEL rendelkező és HTTP-fejlécek.
5. A **hitelesítésszolgáltatókat**, kattintson a **Azure Active Directory**.
   
    ![Hitelesítési/engedélyezési panel az Azure-portálon](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
6. Az a **Azure Active Directory beállításai** panelen kattintson a **Express**.
   
    Az a **Express** beállítás Azure automatikusan létrehozhat egy AAD-alkalmazást az Azure ad [bérlői](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 
   
    Nem rendelkezik a bérlő létrehozására, mert minden Azure-fiók automatikusan van egy.
7. A **üzemmód**, kattintson a **új AD-alkalmazás létrehozása** Ha még nincs kiválasztva.
   
    A portál csatlakozik a **létrehozása App** beviteli mezőt, alapértelmezett értékkel. Alapértelmezés szerint az Azure AD-alkalmazás neve megegyezik az API-alkalmazásba. Tetszés szerint megadhat egy másik nevet.
   
    ![Az Azure AD-beállítások](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)
   
    **Megjegyzés:**: alternatív megoldásként használhat egy egyetlen Azure AD alkalmazás a hívó API-alkalmazás és a védett API-alkalmazásba. Ha úgy dönt, hogy az alternatív, nem kell a **új AD-alkalmazás létrehozása** itt beállítást, mert már létrehozott egy Azure AD-alkalmazást a felhasználó hitelesítési az oktatóanyag korábbi részében. Ebben az oktatóanyagban fogja használni a hívó API-alkalmazás és a védett API-alkalmazás az Azure AD-alkalmazások.
8. Jegyezze fel az értéket a **alkalmazás létrehozása** beviteli mezőt; az AAD-alkalmazást a klasszikus Azure portálon később lesz meg.
9. Kattintson az **OK** gombra.
10. Az a **hitelesítési / engedélyezési** panelen kattintson a **mentése**.
    
    ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)
    
    App Service hoz létre az Azure Active Directory alkalmazás **bejelentkezési URL-cím** és **válasz URL-CÍMEN** automatikusan beállítja az API-alkalmazás URL-címét. Az utóbbi érték lehetővé teszi a felhasználók az AAD-bérlőben jelentkezzen be, és az API-alkalmazás eléréséhez.

### <a name="verify-that-the-api-app-is-protected"></a>Győződjön meg arról, hogy védett-e az API-alkalmazás
1. Egy böngészőben nyissa meg az URL-címet a API-alkalmazás: az a **API-alkalmazás** panel az Azure-portálon kattintson a hivatkozásra kattintva **URL-cím**. 
   
    Ekkor megnyílik egy bejelentkezési képernyő, mert nem hitelesített kérelmek nem engedélyezettek az API-alkalmazás eléréséhez. 
   
    Ha a böngészőben navigáljon a Swagger felhasználói felület, a böngésző már kerülhettek naplózásra a – ebben az esetben, nyisson meg egy InPrivate vagy Incognito ablakot, és navigáljon a Swagger felhasználói felület URL-CÍMÉT.
2. Jelentkezzen be az AAD-bérlőben egy olyan felhasználó hitelesítő adataival.
   
   Ha van bejelentkezve, a "sikeres létrehozásról" tájékoztató oldal jelenik meg a böngészőben.

## <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>A beszerzést, mind az Azure AD-jogkivonat küldése a ToDoListAPI projekt konfigurálása
Ebben a szakaszban a következő feladatokat hajthatnak végre:

* Adja hozzá a kódot a középső réteg API-alkalmazás, amely az Azure AD alkalmazás hitelesítő adatainak használatával egy jogkivonat és a HTTP-kérések küldése az adatréteg API-alkalmazását.
* A hitelesítő adatokat kell beolvasni az Azure AD.
* Adja meg a hitelesítő adatokat az Azure App Service futásidejű környezeti beállításokban a középső rétegbeli API-alkalmazás. 

### <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>A beszerzést, mind az Azure AD-jogkivonat küldése a ToDoListAPI projekt konfigurálása
A következő módosításokat a ToDoListAPI projekt, a Visual Studio.

1. Állítsa vissza az összes lévő kódot az *ServicePrincipal.cs* fájlt.
   
    Ez egy, a kódot, amelyeket a .NET-hez készült adal-t használ az Azure AD tulajdonosi jogkivonat.  Több konfigurációs értékeket, amelyek akkor lesznek állítva az Azure futtatókörnyezetben később használ. A kód itt látható: 
   
        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
   
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
   
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }
   
    **Megjegyzés:** ezt a kódot az adal-t kér .NET NuGet-csomagot (Microsoft.IdentityModel.Clients.ActiveDirectory), amely a projekt már telepítve van. Ez a projekt teljesen új hozott létre, ha kellene telepíteni ezt a csomagot. Ez a csomag automatikusan nincs telepítve az API app – új projekt sablonban.
2. A *tartományvezérlők/ToDoListController*, állítsa vissza a kód a `NewDataAPIClient` módszer, amelynek kiegészíti a jogkivonattal a HTTP-kérelmek engedélyezési fejlécében.
   
        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);
3. Telepítse a ToDoListAPI projektet. (Kattintson a jobb gombbal a projektre, majd kattintson az **közzététel > Közzététel**.)
   
    A Visual Studio telepíti a projektet, és a webes alkalmazás alap URL-cím egy böngészőablakban megnyitja. Ez azt mutatja majd 403-as hibalap, ez nem jelent problémát, a Web API alap URL-címet egy böngészőben nyissa tett kísérlet.
4. Zárja be a böngészőt.

### <a name="get-azure-ad-configuration-values"></a>Az Azure AD konfigurációs értékek beolvasása
1. Az a [a klasszikus Azure portálon](https://manage.windowsazure.com/), és **Azure Active Directory**.
2. Az a **Directory** lapra, majd az AAD-bérlőt.
3. Kattintson a **alkalmazások > a vállalat tulajdonában lévő alkalmazások**, majd kattintson a pipa jelre.
4. Az alkalmazások listájának megtekintéséhez kattintson a Azure létrehozó meg, ha engedélyezte a hitelesítési ToDoListDataAPI (adatok réteg) API-alkalmazás nevét.
5. Kattintson a **Configure** (Konfigurálás) lapra.
6. Másolás a **ügyfél-azonosító** értékét, és mentse valahol beszerezheti a később. 
7. A Azure klasszikus portál lépjen vissza a listája **a vállalat tulajdonában lévő alkalmazások**, és kattintson az AAD-alkalmazást, középső rétegbeli ToDoListAPI API-alkalmazás (hozta létre az előző oktatóanyag, nem az ebben az oktatóanyagban létrehozott egy).
8. Kattintson a **Configure** (Konfigurálás) lapra.
9. Másolás a **ügyfél-azonosító** értékét, és mentse valahol beszerezheti a később.
10. A **kulcsok**, jelölje be **1 év** a a **időtartam kiválasztása** legördülő listából választhatja ki.
11. Kattintson a **Save** (Mentés) gombra.
    
     ![Alkalmazás-kulcs létrehozása](./media/app-service-api-dotnet-service-principal-auth/genkey.png)
12. Másolja ki a kulcs értékét, és mentse valahol beszerezheti a később.
    
     ![Új alkalmazás kulcs másolása](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-the-middle-tier-api-apps-runtime-environment"></a>A középső réteg API-alkalmazásának futtatókörnyezetben az Azure AD-beállítások konfigurálása
1. Lépjen a [Azure-portálon](https://portal.azure.com/), majd keresse meg a **API-alkalmazás** panel, amelyen a TodoListAPI (középső réteg) projekt API-alkalmazás.
2. Kattintson a **Settings > Application Settings** (Beállítások > Alkalmazásbeállítások) lehetőségre.
3. Az a **Alkalmazásbeállítások** területen írja be a következő kulcsok és értékek:
   
   | **Kulcs** | IDA: hatóság |
   | --- | --- |
   | **Érték** |{az Azure AD bérlő neve} https://Login.microsoftonline.com/ |
   | **Példa** |https://Login.microsoftonline.com/contoso.onmicrosoft.com |
   
   | **Kulcs** | IDA: ClientId |
   | --- | --- |
   | **Érték** |A hívó alkalmazás (középső réteg - ToDoListAPI) ügyfél-azonosítója |
   | **Példa** |960adec2-b74a-484A-960adec2-b74a-484A |
   
   | **Kulcs** | IDA: ClientSecret |
   | --- | --- |
   | **Érték** |A hívó alkalmazás (középső réteg - ToDoListAPI) Alkalmazáskulcs |
   | **Példa** |e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |
   
   | **Kulcs** | IDA: erőforrás |
   | --- | --- |
   | **Érték** |A hívott alkalmazás ügyfél-azonosító (adatrétegbeli - ToDoListDataAPI) |
   | **Példa** |e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |
   
    **Megjegyzés:**: A `ida:Resource`, a hívott alkalmazást kell használnia **ügyfél-azonosító** és nem a **App ID URI**.
   
    `ida:ClientId`és `ida:Resource` eltérő értékek ehhez az oktatóanyaghoz, mert használata külön az Azure AD applicaations a középső réteg és az adatszint. Ha egyetlen használta az Azure AD-alkalmazást a hívó API-alkalmazás és a védett API-alkalmazás, használhatja ugyanazt az értéket mindkét `ida:ClientId` és `ida:Resource`.
   
    A kód ConfigurationManager beolvasni ezeket az értékeket, így azok tárolható, a projekt Web.config fájlban, vagy az Azure futtatókörnyezetben használja. Az ASP.NET-alkalmazások futása közben az Azure App Service-ben, a környezet beállítások automatikusan felülbírálhatják a beállításokat a Web.config fájlból. A tesztkörnyezet beállításainak vannak általában egy [biztonságát módot a Web.config fájlban képest bizalmas adatokat tárol](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).
4. Kattintson a **Save** (Mentés) gombra.
   
    ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-the-application"></a>Az alkalmazás tesztelése
1. Nyissa meg böngészőben az AngularJS előtér-webalkalmazás HTTPS URL-címét.
2. Kattintson a **To Do List** lapra, és jelentkezzen be az Azure AD-bérlő a felhasználó hitelesítő adataival. 
3. Adja hozzá a Tennivalólista elemein, hogy az alkalmazás működésének ellenőrzéséhez.
   
    ![Tennivalók listája lap](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)
   
    Ha az alkalmazás a várt módon nem működik, gondosan ellenőrizze a megadott Azure-portálon beállítások. Ha a beállítások csak akkor lehet helyes jelenik meg, tekintse meg a [hibaelhárítás](#troubleshooting) szakasz az oktatóanyag későbbi részében.

## <a name="protect-the-api-app-from-browser-access"></a>Böngésző-hozzáférés elleni védelmének az API-alkalmazás
A jelen oktatóanyag egy külön létrehozott ToDoListDataAPI (adatok réteg) API-alkalmazás az Azure AD-alkalmazás. Most láthatta, ha az App Service egy AAD-alkalmazást hoz létre, mert oly módon, amely lehetővé teszi az API-alkalmazás URL-címet egy böngészőben nyissa, és jelentkezzen be egy felhasználó beállítja az AAD-alkalmazást. Ez azt jelenti, lehetséges, hogy a felhasználó az Azure AD-bérlőn, nem csak egy egyszerű szolgáltatásnév, az API eléréséhez. 

Ha meg szeretné akadályozni, hogy a böngésző-hozzáférés a védett API-alkalmazás programozás nélkül, módosíthatja a **válasz URL-CÍMEN** az AAD-alkalmazást, hogy eltér az API-alkalmazás alap URL. 

### <a name="disable-browser-access"></a>Böngésző-hozzáférés letiltása
1. A klasszikus portálon **konfigurálása** lapján az AAD-alkalmazást, amely a TodoListService lett létrehozva, módosítsa a **válasz URL-CÍMEN** úgy, hogy egy érvényes URL-címet, de nem az API-alkalmazás URL-cím mezőben.
2. Kattintson a **Save** (Mentés) gombra.

### <a name="verify-browser-access-no-longer-works"></a>Ellenőrizze a böngésző-hozzáférés nem működik.
Korábban ellenőrizte, hogy lépjen az API-alkalmazás URL-CÍMÉT a böngésző egyéni felhasználói hitelesítő adatokkal való bejelentkezés. Ebben a szakaszban, ellenőrizze, hogy ez nem lehetséges. 

1. Egy új böngészőablakban újra az URL-címet a API-alkalmazás.
2. Jelentkezzen be, amikor a rendszer erre kéri.
3. Bejelentkezés sikeresen befejeződik, de egy hibalap vezet.
   
    Az AAD-alkalmazást, hogy a felhasználók az AAD-bérlőben nem jelentkezzen be és érhetik el az API-t egy beállított. Az API-alkalmazás továbbra is elérheti, ha a szolgáltatás egyszerű token, ellenőrizheti a webalkalmazás URL-címen, és vegye fel a további Tennivalólista elemein.

## <a name="restrict-access-to-a-particular-service-principal"></a>Egy adott szolgáltatás egyszerű való hozzáférés korlátozása
Jelenleg, bármely hívó, amely egy jogkivonatot kérhet egy felhasználó vagy az Azure AD-bérlőben egyszerű meghívhatja a TodoListDataAPI (adatok réteg) API-alkalmazásba. Győződjön meg arról, hogy csak az adatréteg API-alkalmazásának elfogadja a hívásokat a TodoListAPI (középső réteg) API-alkalmazást, és csak egy adott szolgáltatás egyszerű kívánt. 

Ezek a korlátozások érvényesítése kódrészletet adhat hozzá a `appid` és `objectidentifier` jogcímet a bejövő hívások.

Ebben az oktatóanyagban a kódot, amely ellenőrzi az alkalmazás és szolgáltatás egyszerű azonosító közvetlenül a tartományvezérlő műveletek a put.  Alternatívák egy egyéni használandó `Authorize` attribútumot, vagy az ellenőrzés elvégzéséhez az indítási feladatütemezések (pl. OWIN köztes). Az utóbbi példáért lásd: [a mintaalkalmazás](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

Az alábbi módosításokat végezni, a TodoListDataAPI projektet.

1. Nyissa meg a *Controllers/TodoListController.cs* fájlt.
2. Állítsa vissza a sorok beállított `trustedCallerClientId` és `trustedCallerServicePrincipalId`.
   
        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];
3. Állítsa vissza a kódot a CheckCallerId metódusban. Ez a metódus neve elején. minden műveleti módszer a vezérlőben. 
   
        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The appID or service principal ID is not the expected value." });
            }
        }
4. Telepítse újra a ToDoListDataAPI projektet az Azure App Service.
5. A böngészőben nyissa meg az AngularJS előtérbeli webes alkalmazás HTTPS URL-CÍMÉT, és a kezdőlapján kattintson a **To Do List** fülre.
   
    Az alkalmazás nem működik, mert a háttérben hívások nem működnek. Az új kódot tényleges appid és típusinformáció ellenőrzi, de még nem rendelkezik a megfelelő értékek kereséséhez, ellen. A böngésző fejlesztői eszközök konzol jelenti, hogy a kiszolgáló HTTP 401-es hibát ad vissza.
   
    ![Hiba történt a fejlesztői eszközök konzol](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)
   
    A következő lépésekben konfigurál a várt értékeket.
6. Az Azure AD-alkalmazást, a TodoListWebApp projekthez az Azure AD PowerShell, a szolgáltatás egyszerű értékének beolvasása.
   
    a. Azure PowerShell telepítése és csatlakozás az előfizetéshez kapcsolatos útmutatásért lásd: [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).
   
    b. Szolgáltatás résztvevők listájának lekéréséhez hajtható végre a `Login-AzureRmAccount` parancsot, majd a `Get-AzureRmADServicePrincipal` parancsot.
   
    c. Az ObjectId azonosító található a szolgáltatás egyszerű a TodoListAPI alkalmazás, és mentse egy helyre másolhatja később.
7. Az Azure portálon keresse meg az API-t telepítette, a ToDoListDataAPI projektet az API-alkalmazás panelen.
8. Kattintson a **beállítások > Alkalmazásbeállítások**.
9. Az a **Alkalmazásbeállítások** területen írja be a következő kulcsok és értékek:
   
   | **Kulcs** | ToDo:TrustedCallerServicePrincipalId |
   | --- | --- |
   | **Érték** |Szolgáltatás résztvevő-azonosító az alkalmazás hívása |
   | **Példa** |4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |
   
   | **Kulcs** | ToDo:TrustedCallerClientId |
   | --- | --- |
   | **Érték** |Alkalmazás - átmásolva a TodoListAPI az Azure AD alkalmazás hívása ügyfél-azonosítója |
   | **Példa** |960adec2-b74a-484A-960adec2-b74a-484A |
10. Kattintson a **Save** (Mentés) gombra.
    
     ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)
11. A böngészőben lépjen vissza a webes alkalmazás URL-címet, és a kezdőlapján kattintson a **To Do List** fülre.
    
     Ezúttal az alkalmazás megfelelően működik-e mivel a megbízható hívó alkalmazás és szolgáltatás egyszerű azonosító a várt értékeket.
    
     ![Tennivalók listája lap](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-the-projects-from-scratch"></a>Teljesen új projektek felépítése
A két Web API-projektek használatával létrehozott a **Azure API App** projektek sablon és az alapértelmezett értékek vezérlő lecserélését egy ToDoList tartományvezérlőre. Az Azure AD szolgáltatás egyszerű jogkivonatokat a ToDoListAPI projektet az beszerzése a [Active Directory Authentication Library (ADAL) .NET-keretrendszerhez készült](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) NuGet-csomag lett telepítve.

Hozzon létre egy AngularJS egyoldalas alkalmazást a Web API háttérből például a ToDoListAngular kapcsolatos információkért lásd: [beavatkozás nélküli a labor: egy egyetlen oldal alkalmazás (SPA) az ASP.NET Web API és Angular.js Build](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Az Azure AD hitelesítési kód adásával kapcsolatos információkért lásd: [biztonságossá tétele AngularJS egyetlen oldal alkalmazások az Azure ad-val](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Hibaelhárítás
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Győződjön meg arról, hogy azt ne tévessze össze ToDoListAPI (középső réteg) és a ToDoListDataAPI (adatrétegbeli). Például ebben az oktatóanyagban, hitelesítés hozzáadása az adatréteg API-alkalmazását, **, de az alkalmazás kulcsot kell származnia ahhoz az Azure AD-alkalmazás, amelyet a középső réteg API-alkalmazásának létrehozott**.

## <a name="next-steps"></a>Következő lépések
Ez az utolsó oktatóanyaga, amely az API Apps adatsorozat. 

Azure Active Directoryval kapcsolatos további információkért lásd a következőket.

* [Az Azure AD fejlesztői útmutatója](http://aka.ms/aaddev)
* [Az Azure AD-forgatókönyvek](http://aka.ms/aadscenarios)
* [Azure AD-minták](http://aka.ms/aadsamples)
  
    A [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) minta hasonlít megjelenő elemek ebben az oktatóanyagban, de az App Service hitelesítés nélkül.

Más módokon is telepíthet a Visual Studio-projektek API-alkalmazások esetén, vagy a Visual Studio használatával kapcsolatos információk [telepítés automatizálásáról](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) a egy [Forráskezelő rendszerből](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), lásd: [központi telepítése egy Azure App Service-alkalmazást](../app-service-web/web-sites-deploy.md).

