---
title: "az Azure App Service API Apps aaaService egyszerű hitelesítéssel |} Microsoft Docs"
description: "Ismerje meg, hogy az API-k tooprotect alkalmazást az Azure App Service szolgáltatások forgatókönyvek esetén."
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
ms.openlocfilehash: 94d9ee11f38293df4a2fd815ef02c59cc6defed4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Az Azure App Service API Apps szolgáltatás egyszerű hitelesítés
## <a name="overview"></a>Áttekintés
Ez a cikk azt ismerteti, hogyan toouse App Service hitelesítés *belső* tooAPI alkalmazások eléréséhez. Egy belső forgatókönyv, melyekben egy API-alkalmazást, amelyet toobe fogyasztható csak a saját alkalmazás kóddal. hello ajánlott módja tooimplement ebben a forgatókönyvben az App Service-ben az Azure AD toouse tooprotect hello nevezik API-alkalmazásba. Védett hello API-alkalmazás azáltal, hogy az alkalmazásidentitás (egyszerű szolgáltatásnév) hitelesítő adatait az Azure AD-ből kapott tulajdonosi jogkivonatok hívható meg. Alternatívák toousing az Azure AD, lásd: hello **szolgáltatások közötti hitelesítési** hello szakasza [Azure App Service hitelesítés áttekintése](../app-service/app-service-authentication-overview.md#service-to-service-authentication).

Ebből a cikkből megtudhatja:

* Hogyan toouse Azure Active Directory (Azure AD) tooprotect egy API-alkalmazást a hitelesítés nélküli hozzáférést.
* Hogyan tooconsume védett API app egy API-alkalmazás, a webes alkalmazás vagy a mobilalkalmazás az Azure AD szolgáltatás rendszerbiztonsági tag (app-identitás) hitelesítő adatok használatával. További információ a logic App tooconsume lásd [az App Service a Logic apps üzemeltetett egyéni API használata](../logic-apps/logic-apps-custom-hosted-api.md).
* Hogyan toomake meg arról, hogy hello védett API-alkalmazás által a böngészőből nem hívható bejelentkezett felhasználók számára.
* Hogyan toomake meg arról, hogy hello védett API-alkalmazás csak meghívható egy adott Azure AD szolgáltatás egyszerű.

hello cikk két részből áll:

* Hello [hogyan tooconfigure szolgáltatás egyszerű hitelesítés az Azure App Service](#authconfig) a szakasz ismerteti a általános hogyan tooconfigure hitelesítési API-alkalmazást, és hogyan tooconsume hello védett API-alkalmazás. Ez a szakasz egyformán tooall keretrendszerek App Service, beleértve a .NET, Node.js és Java által támogatott.
* Hello kezdve [hello .NET-bevezető oktatóanyagok folytatása](#tutorialstart) szakaszban hello az oktatóanyag végigvezeti Önt egy "belső hozzáférés" forgatókönyvet, az App Service-ben futó .NET mintaalkalmazás konfigurálása. 

## <a id="authconfig"></a>Hogyan tooconfigure szolgáltatás egyszerű hitelesítés az Azure App Service-ben
Ez a témakör általános útmutatást, amelyek érvényesek a tooany API-alkalmazás. A lépések adott toohello tooDo lista .NET mintaalkalmazás, go túl[hello .NET API-alkalmazások útmutató-sorozat folytatása](#tutorialstart).

1. A hello [Azure-portálon](https://portal.azure.com/), keresse meg a toohello **beállítások** hello API-alkalmazás, hogy szeretné, hogy tooprotect, és ezután megkeresi a hello panel **szolgáltatások** szakaszt, és kattintson a **Hitelesítési / engedélyezési**.
   
    ![Hitelesítési/engedélyezési Azure-portálon](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. A hello **hitelesítési / engedélyezési** panelen kattintson a **a**.
3. A hello **hitelesítetlen kérés esetén a művelet tootake** legördülő listában válassza **bejelentkezés Azure Active Directoryval** .
4. A **hitelesítésszolgáltatókat**, jelölje be **Azure Active Directory**.
   
    ![Hitelesítési/engedélyezési panel az Azure-portálon](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. Hello konfigurálása **Azure Active Directory beállításai** panel toocreate egy új Azure AD-alkalmazást, vagy ha már rendelkezik egy meglévő Azure AD alkalmazás használja, amelyet az toouse.
   
    Belső forgatókönyvek általában tartalmaz, amely az API-alkalmazást hívja az API-alkalmazások. Használhatja a külön Azure AD-alkalmazásokat minden API-alkalmazás vagy egy Azure AD-alkalmazást.
   
    A panel részletes utasításokért lásd: [hogyan tooconfigure az App Service alkalmazás toouse Azure Active Directory bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).
6. Amikor elkészült, a hello hitelesítési szolgáltató konfigurációs panelen, kattintson a **OK**.
7. A hello **hitelesítési / engedélyezési** panelen kattintson a **mentése**.
   
    ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Ebben az esetben az App Service csak engedélyez kéréseket a konfigurált hello Azure AD-bérlő hívókat. Nincs hitelesítési vagy engedélyezési kód védett hello API-alkalmazás szükséges. hello tulajdonosi jogkivonattal átadott toohello API app együtt gyakran használt jogcímek HTTP-fejlécek, és ezt az információt, amelyek egy adott hívó, például az egyszerű szolgáltatás kérelmek kód toovalidate érheti el.

Ez a hitelesítési funkció hello működik ugyanúgy minden nyelvhez, hogy az App service támogatja, beleértve a .NET, Node.js és Java. 

#### <a name="how-tooconsume-hello-protected-api-app"></a>Hogyan tooconsume hello védett API-alkalmazás
hello hívónak biztosítania kell egy Azure AD-tulajdonosi jogkivonattal API-hívások. tooget szolgáltatás egyszerű hitelesítő adataival tulajdonosi jogkivonattal, hello hívó használja az Active Directory Authentication Library (adal-t a [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs), vagy [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). tooget jogkivonatot, ADAL behívó kód hello tooADAL hello a következő információkat biztosítja:

* az Azure AD-bérlő hello neve.
* hello azonosító és az ügyfél ügyfélkulcs (Alkalmazáskulcs) hello hívó társított hello Azure AD-alkalmazás.
* hello hello társított Azure AD-alkalmazás Ügyfélazonosítója hello védett API-alkalmazásba. (Ha csak egy Azure AD-alkalmazást használ, ez van hello ugyanazon ügyfél-Azonosítót, egy a hello hívó hello.)

Ezek az értékek érhetők el az Azure AD hello lapjain hello [a klasszikus Azure portálon](https://manage.windowsazure.com/).

Hello token beszerzett, miután hello hívó tartalmazza a HTTP-kérelmek hello engedélyezési fejlécben.  App Service érvényesíti hello jogkivonatot, és lehetővé teszi, hogy hello kérelmek tooreach hello védett API-alkalmazás.

#### <a name="how-tooprotect-hello-api-app-from-access-by-users-in-hello-same-tenant"></a>Hogyan tooprotect hello API-alkalmazást a hello a felhasználók ugyanazt a bérlői
Tulajdonosi jogkivonatok ugyanannak a bérlőnek érvényesek a hello hello lévő felhasználók számára védett API-alkalmazásba.  Ha azt szeretné, hogy csak egy egyszerű szolgáltatásnév meghívhatja tooensure hello védett API-alkalmazást, adja hozzá a kódot a hello védett API app toovalidate hello hello jogkivonatból jogcímek a következő:

* `appid`hello hívó társított hello Azure AD-alkalmazás ügyfél-azonosító hello kell lennie. 
* `oid`(`objectidentifier`) hello szolgáltatás résztvevő-azonosító hello hívó kell lennie. 

App Service emellett hello `objectidentifier` jogcím hello X-MS-CLIENT-tag-ID-fejlécben.

### <a name="how-tooprotect-hello-api-app-from-browser-access"></a>Hogyan tooprotect hello böngészőalapú hozzáférés API-alkalmazások
Ha Ön nem ellenőrizhesse a kódban védett hello API-alkalmazás, és külön használja az Azure AD-alkalmazást hello védett API-alkalmazás, győződjön meg arról, hogy az Azure AD alkalmazás-válasz URL-cím hello nem hello megegyeznek a hello API-alkalmazás alap URL-címet. Ha hello válasz URL-címet közvetlenül védett toohello API-alkalmazás, egy felhasználó hello ugyanazt az Azure AD-bérlőben sikerült toohello API-alkalmazás keresse meg, jelentkezzen be, és sikeresen meg tudja hívni hello API.

## <a id="tutorialstart"></a>Hello .NET API-alkalmazások útmutató-sorozat folytatása
Ha a következő hello Node.js vagy Java oktatóanyag adatsorozat API-alkalmazások, hagyja ki toohello [további lépések](#next-steps) szakasz. 

hello a cikk hátralévő része továbbra is hello .NET API-alkalmazások útmutató-sorozat, és feltételezi, hogy végrehajtotta-e az hello [felhasználói hitelesítési oktatóanyag](app-service-api-dotnet-user-principal-auth.md) és hello minta alkalmazást az Azure-ban és a felhasználói hitelesítés engedélyezve van.

## <a name="set-up-authentication-in-azure"></a>Az Azure-ban hitelesítés beállítása
Ebben a szakaszban App szolgáltatást konfigurálja úgy, hogy csak a HTTP-kérelmek tooreach hello API-alkalmazás adatrétegét lehetővé teszi, hogy hello hello érvényes Azure melyeket AD tulajdonosi jogkivonatoknak nevezzük. 

A következő szakasz hello hello középső réteg API app toosend alkalmazás hitelesítő adatok tooAzure AD konfigurálhatja, vissza egy tulajdonosi jogkivonatot, és küldjön hello tulajdonosi jogkivonat toohello adatrétegbeli API-alkalmazást. Ez a folyamat hello ábrán látható.

![Szolgáltatás-hitelesítés diagramja](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Ha a következő hello oktatóanyag utasításait során problémákat tapasztal, tekintse meg a hello [hibaelhárítás](#troubleshooting) szakasz hello oktatóanyag hello végén. 

1. A hello [Azure-portálon](https://portal.azure.com/), keresse meg a toohello **beállítások** hello API-alkalmazást, amely hello ToDoListDataAPI (adatok réteg) API-alkalmazást létrehozni, és kattintson a panel **beállítások**.
2. A hello **beállítások** panelen, a keresés hello **szolgáltatások** szakaszt, és kattintson a **hitelesítési / engedélyezési**.
   
    ![Hitelesítési/engedélyezési Azure-portálon](./media/app-service-api-dotnet-user-principal-auth/features.png)
3. A hello **hitelesítési / engedélyezési** panelen kattintson a **a**.
4. A hello **hitelesítetlen kérés esetén a művelet tootake** legördülő listában válassza **bejelentkezés Azure Active Directoryval**.
   
    Ez az, hogy csak hitelesített kérelmek reach hello API-alkalmazás az App Service tooensure okozó hello beállítása. Érvényes tulajdonosi jogkivonatok rendelkező kérelmek, az App Service hello jogkivonatok mentén toohello API app továbbítja, és tölti fel a HTTP-fejlécek a gyakran használt jogcímek toomake ezt az információt könnyebben elérhető tooyour kódot.
5. A **hitelesítésszolgáltatókat**, kattintson a **Azure Active Directory**.
   
    ![Hitelesítési/engedélyezési panel az Azure-portálon](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
6. A hello **Azure Active Directory beállításai** panelen kattintson a **Express**.
   
    A hello **Express** beállítás Azure automatikusan létrehozhat egy AAD-alkalmazást az Azure ad [bérlői](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 
   
    Nincs toocreate a bérlőt, mert minden Azure-fiók automatikusan van egy.
7. A **üzemmód**, kattintson a **új AD-alkalmazás létrehozása** Ha még nincs kiválasztva.
   
    hello portál csatlakozik hello **létrehozása App** beviteli mezőt, alapértelmezett értékkel. Alapértelmezés szerint az Azure AD-alkalmazást hello nevű hello ugyanaz, mint hello API alkalmazás. Tetszés szerint megadhat egy másik nevet.
   
    ![Az Azure AD-beállítások](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)
   
    **Megjegyzés:**: alternatív megoldásként használhat egy egyetlen Azure AD alkalmazás mindkét hello hívó API-alkalmazást, és hello védett API-alkalmazásba. Ha úgy dönt, hogy az alternatív, nem kell hello **új AD-alkalmazás létrehozása** itt beállítást, mert a korábbi részében hello felhasználói hitelesítési oktatóanyag már létrehozott egy Azure AD-alkalmazást. Ebben az oktatóanyagban használt Azure AD-alkalmazások külön hívja az API-alkalmazás és hello hello védett API-alkalmazásba.
8. Jegyezze fel a hello értéket, amely a hello **létrehozása App** beviteli mezőt; meg fogja keresni az AAD-alkalmazást hello a klasszikus Azure portálon később.
9. Kattintson az **OK** gombra.
10. A hello **hitelesítési / engedélyezési** panelen kattintson a **mentése**.
    
    ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)
    
    App Service hoz létre egy Azure Active Directory-alkalmazás az **bejelentkezési URL-cím** és **válasz URL-CÍMEN** toohello URL-címet a API-alkalmazás automatikusan beállítani. hello ez utóbbi érték az AAD-bérlő toolog a felhasználók és a hozzáférés hello API-alkalmazás lehetővé teszi.

### <a name="verify-that-hello-api-app-is-protected"></a>Győződjön meg arról, hogy hello API app védett
1. Egy böngészőben nyissa meg hello API-alkalmazás URL-CÍMÉT toohello: a hello **API-alkalmazás** panel az Azure-portálon hello alatt hello hivatkozásra **URL-cím**. 
   
    Átirányított tooa bejelentkezési képernyő áll, mert nem hitelesített kérelmek nem engedélyezettek tooreach hello API-alkalmazás. 
   
    Ha a böngészőben nyissa meg toohello Swagger felhasználói Felületet, előfordulhat, hogy a böngésző már bejelentkezett a – ebben az esetben, nyisson meg egy InPrivate vagy Incognito ablakot, és folytassa toohello Swagger felhasználói felület URL-CÍMÉT.
2. Jelentkezzen be az AAD-bérlőben egy olyan felhasználó hitelesítő adataival.
   
   Ha van bejelentkezve, hello "sikeresen létrejött" lap hello böngészőben.

## <a name="configure-hello-todolistapi-project-tooacquire-and-send-hello-azure-ad-token"></a>Hello ToDoListAPI projekt tooacquire konfigurálása és hello Azure AD-jogkivonat küldése
Ez a szakasz a következő feladatok hello:

* Adja hozzá a kódot hello középső rétegbeli API-alkalmazás, amely az Azure AD alkalmazás hitelesítő adatok tooacquire jogkivonatot használja, és a HTTP-kérelmek toohello API-alkalmazás adatrétegét küldése.
* Hello hitelesítő adatokat kell beolvasni az Azure AD.
* Adja meg a hello hitelesítő adatait az Azure App Service futásidejű környezeti beállításokban hello középső rétegbeli API-alkalmazás. 

### <a name="configure-hello-todolistapi-project-tooacquire-and-send-hello-azure-ad-token"></a>Hello ToDoListAPI projekt tooacquire konfigurálása és hello Azure AD-jogkivonat küldése
Ellenőrizze a következő hello ToDoListAPI projektre a Visual Studio változásai hello.

1. Állítsa vissza az összes hello hello kód *ServicePrincipal.cs* fájlt.
   
    Ez a .NET tooacquire hello Azure AD tulajdonosi jogkivonatot az adal-t használó hello kódot.  Több konfigurációs értékeket fogja beállított hello Azure futtatókörnyezetben később használ. Az alábbiakban hello kódot: 
   
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
   
    **Megjegyzés:** ezt a kódot hello adal-t kér .NET NuGet-csomagot (Microsoft.IdentityModel.Clients.ActiveDirectory), amely hello projekt már telepítve van. Ez a projekt teljesen új hozott létre, ha a csomag tooinstall kellene lennie. Ez a csomag nem automatikusan hello API app – új projekt sablon telepíti.
2. A *tartományvezérlők/ToDoListController*, állítsa vissza a hello hello kód `NewDataAPIClient` hello authorization fejlécet hello token tooHTTP kérelmek hozzáadó metódust.
   
        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);
3. Hello ToDoListAPI projekt telepítése. (Kattintson a jobb gombbal a hello projektet, majd kattintson az **közzététel > Közzététel**.)
   
    Visual Studio telepíti hello projektet, és megnyitja a böngésző toohello webes alkalmazás alap URL-címet. Ez azt mutatja majd 403-as hiba lap, egy kísérlet toogo tooa Web API alap URL-CÍMÉT a böngésző normális.
4. Bezárás hello böngésző.

### <a name="get-azure-ad-configuration-values"></a>Az Azure AD konfigurációs értékek beolvasása
1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), nyissa meg túl**Azure Active Directory**.
2. A hello **Directory** lapra, majd az AAD-bérlőt.
3. Kattintson a **alkalmazások > a vállalat tulajdonában lévő alkalmazások**, majd kattintson a pipa hello.
4. Az alkalmazások hello listában kattintson egy Azure hozott létre az Ön hitelesítési hello ToDoListDataAPI (adatok réteg) API-alkalmazás engedélyezésekor hello hello nevét.
5. Kattintson a hello **konfigurálása** fülre.
6. Másolás hello **ügyfél-azonosító** értékét, és mentse valahol beszerezheti a később. 
7. Hello a klasszikus Azure portálon lépjen vissza toohello listája **a vállalat tulajdonában lévő alkalmazások**, és kattintson a hello AAD-alkalmazást, amelyet létrehozott hello középső rétegbeli ToDoListAPI API-alkalmazás (hello nem létrehozott hello előző oktatóanyagban hello egyetlen, ebben az oktatóanyagban létre).
8. Kattintson a hello **konfigurálása** fülre.
9. Másolás hello **ügyfél-azonosító** értékét, és mentse valahol beszerezheti a később.
10. A **kulcsok**, jelölje be **1 év** hello a **időtartam kiválasztása** legördülő listából választhatja ki.
11. Kattintson a **Save** (Mentés) gombra.
    
     ![Alkalmazás-kulcs létrehozása](./media/app-service-api-dotnet-service-principal-auth/genkey.png)
12. Másolja ki hello kulcs értékét, és mentse valahol beszerezheti a később.
    
     ![Új alkalmazás kulcs másolása](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-hello-middle-tier-api-apps-runtime-environment"></a>Hello középső réteg API app futtatókörnyezetben az Azure AD-beállítások konfigurálása
1. Nyissa meg toohello [Azure-portálon](https://portal.azure.com/), és navigáljon a toohello **API-alkalmazás** panel, amelyen hello TodoListAPI (középső réteg) projekt hello API-alkalmazás.
2. Kattintson a **Settings > Application Settings** (Beállítások > Alkalmazásbeállítások) lehetőségre.
3. A hello **Alkalmazásbeállítások** területen írja be a következő hello kulcsok és értékek:
   
   | **Kulcs** | IDA: hatóság |
   | --- | --- |
   | **Érték** |{az Azure AD bérlő neve} https://Login.microsoftonline.com/ |
   | **Példa** |https://Login.microsoftonline.com/contoso.onmicrosoft.com |
   
   | **Kulcs** | IDA: ClientId |
   | --- | --- |
   | **Érték** |Ügyfél-azonosítója (középső réteg - ToDoListAPI) alkalmazás hívása hello |
   | **Példa** |960adec2-b74a-484A-960adec2-b74a-484A |
   
   | **Kulcs** | IDA: ClientSecret |
   | --- | --- |
   | **Érték** |Az alkalmazás (középső réteg - ToDoListAPI) hívása hello Alkalmazáskulcs |
   | **Példa** |e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |
   
   | **Kulcs** | IDA: erőforrás |
   | --- | --- |
   | **Érték** |Hello néven alkalmazás ügyfél-azonosítója (adatrétegbeli - ToDoListDataAPI) |
   | **Példa** |e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |
   
    **Megjegyzés:**: A `ida:Resource`, ellenőrizze, hogy az alkalmazás neve hello használata **ügyfél-azonosító** és nem a **App ID URI**.
   
    `ida:ClientId`és `ida:Resource` különböző értékeket ehhez az oktatóanyaghoz mert használata külön az Azure AD applicaations hello középső réteg és adatréteg. Ha egyetlen használta az Azure AD alkalmazás hívja az API-alkalmazás és hello hello védett API-alkalmazást, használja ugyanezt az értéket a mindkét hello `ida:ClientId` és `ida:Resource`.
   
    hello kód használ ConfigurationManager tooget ezeket az értékeket, így azok a hello projekt Web.config fájlban, vagy hello Azure futtatókörnyezetben tárolható. Az ASP.NET-alkalmazások futása közben az Azure App Service-ben, a környezet beállítások automatikusan felülbírálhatják a beállításokat a Web.config fájlból. A tesztkörnyezet beállításainak vannak általában egy [bizalmas adatok biztonságosabb módon toostore képest tooa Web.config fájl](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).
4. Kattintson a **Save** (Mentés) gombra.
   
    ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-hello-application"></a>Hello alkalmazás tesztelése
1. Nyissa meg böngészőben toohello hello AngularJS előtér-webalkalmazás HTTPS URL-CÍMÉT.
2. Kattintson a hello **tooDo lista** lapra, és jelentkezzen be az Azure AD-bérlő a felhasználó hitelesítő adataival. 
3. Vegye fel a tennivalók elemek tooverify hello alkalmazás működik.
   
    ![tooDo lista lap](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)
   
    Ha hello alkalmazás a várt módon nem működik, gondosan ellenőrizze a megadott Azure-portálon hello hello-beállítások. Ha hello beállítások jelennek meg a megfelelő toobe, lásd: hello [hibaelhárítás](#troubleshooting) szakasz az oktatóanyag későbbi részében.

## <a name="protect-hello-api-app-from-browser-access"></a>Böngésző-hozzáférés elleni védelmének hello API-alkalmazás
A jelen oktatóanyag egy külön létrehozott hello ToDoListDataAPI (adatok réteg) API-alkalmazás az Azure AD-alkalmazás. Most láthatta, amikor az App Service hoz létre egy AAD-alkalmazást, mert olyan módon, amely lehetővé teszi, hogy egy felhasználó toogo toohello API-alkalmazás URL-cím, egy böngésző és a napló a konfigurálja hello AAD-alkalmazást. Ez azt jelenti, lehetséges, hogy a felhasználó az Azure AD-bérlőn, nem csak egy egyszerű szolgáltatásnév, tooaccess hello API. 

Ha azt szeretné tooprevent böngésző-hozzáférés nélküli programozás hello a védett API-alkalmazás, módosíthatja a hello **válasz URL-CÍMEN** hello AAD-alkalmazást úgy, hogy az eltérő hello API-alkalmazás alap URL. 

### <a name="disable-browser-access"></a>Böngésző-hozzáférés letiltása
1. Hello klasszikus portál **konfigurálása** hello AAD-alkalmazást, amely az hello TodoListService készült lapján, módosítsa a hello hello értéket **válasz URL-CÍMEN** úgy, hogy egy érvényes URL-címet, de nem hello API alkalmazás mezőben URL-CÍME.
2. Kattintson a **Save** (Mentés) gombra.

### <a name="verify-browser-access-no-longer-works"></a>Ellenőrizze a böngésző-hozzáférés nem működik.
Korábban ellenőrizte, hogy lépjen toohello API-alkalmazás URL-CÍMÉT a böngésző egyéni felhasználói hitelesítő adatokkal való bejelentkezés. Ebben a szakaszban, ellenőrizze, hogy ez nem lehetséges. 

1. Egy új böngészőablakban nyissa meg újra toohello hello API-alkalmazás URL-CÍMÉT.
2. Ha a napló toodo úgy kéri.
3. Bejelentkezés sikeresen befejeződik, de tooan hibalap vezet.
   
    Hello AAD-alkalmazást, hogy a felhasználók az AAD-bérlőt hello nem jelentkezzen be és érhetik el hello API egy beállított. API-alkalmazás hello szolgáltatás egyszerű jogkivonat használatával, amely toohello webes alkalmazás URL-CÍMÉT is, és vegye fel a további tennivaló ellenőrizheti továbbra is elérheti.

## <a name="restrict-access-tooa-particular-service-principal"></a>Hozzáférés tooa adott szolgáltatás egyszerű korlátozása
Jelenleg, bármely hívó, amely egy jogkivonatot kérhet egy felhasználó vagy az Azure AD-bérlőben egyszerű meghívhatja hello TodoListDataAPI (adatok réteg) API-alkalmazást. Érdemes lehet toomake meg arról, hogy hello API-alkalmazás adatrétegét csak hívások hello TodoListAPI (középső réteg) API-alkalmazást, és csak egy adott szolgáltatás egyszerű fogad el. 

Ezek a korlátozások is hozzáadhat kód toovalidate hello hozzáadásával `appid` és `objectidentifier` jogcímet a bejövő hívások.

Ebben az oktatóanyagban hello kódot, amely ellenőrzi az alkalmazás és szolgáltatás egyszerű azonosító közvetlenül a tartományvezérlő műveletek a put.  Alternatívák toouse egyéni `Authorize` attribútum vagy toodo az ellenőrzés az a indítási sorozatok (pl. OWIN köztes). Ez utóbbi hello példáért lásd: [a mintaalkalmazás](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

Ellenőrizze a következő módosításokat toohello TodoListDataAPI projektet hello.

1. Nyissa meg hello *Controllers/TodoListController.cs* fájlt.
2. Állítsa vissza a beállított hello sorok `trustedCallerClientId` és `trustedCallerServicePrincipalId`.
   
        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];
3. Állítsa vissza a hello kód hello CheckCallerId metódust. Ez a módszer minden műveletmetódus hello vezérlőben hello elején nevezik. 
   
        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "hello appID or service principal ID is not hello expected value." });
            }
        }
4. Telepítse újra a hello ToDoListDataAPI projektet tooAzure App Service.
5. A böngészőben nyissa meg toohello AngularJS előtér webalkalmazás HTTPS URL-címet, és a hello kezdőlapján kattintson a hello **tooDo lista** fülre.
   
    hello alkalmazás nem működik, mert toohello vissza befejezéséhez hívás sikertelen. Új kód hello tényleges appid és típusinformáció ellenőrzi, de még nem kapott hello helyes az értékük toocheck elleni őket. a jelentés hello böngésző fejlesztői eszközök konzol hello kiszolgálón HTTP 401-es hibát adott vissza.
   
    ![Hiba történt a fejlesztői eszközök konzol](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)
   
    Az alábbi lépésekkel hello konfigurálja hello várt értékeket.
6. Az Azure AD PowerShell, a beolvasása hello szolgáltatás hello értékét egyszerű hello hello TodoListWebApp projekthez létrehozott Azure AD-alkalmazást.
   
    a. Útmutatást tooinstall Azure PowerShell és a csatlakozás tooyour előfizetés, lásd: [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).
   
    b. szolgáltatás résztvevők listájának tooget hajtható végre hello `Login-AzureRmAccount` parancsot, és ezután hello `Get-AzureRmADServicePrincipal` parancsot.
   
    c. Hello objectid keresése hello szolgáltatás egyszerű hello TodoListAPI alkalmazás, és mentse egy helyre másolhatja később.
7. A hello Azure-portálon keresse meg az API-alkalmazás paneljének toohello a hello API-alkalmazás telepített hello ToDoListDataAPI projektet.
8. Kattintson a **beállítások > Alkalmazásbeállítások**.
9. A hello **Alkalmazásbeállítások** területen írja be a következő hello kulcsok és értékek:
   
   | **Kulcs** | ToDo:TrustedCallerServicePrincipalId |
   | --- | --- |
   | **Érték** |Szolgáltatás résztvevő-azonosító az alkalmazás hívása |
   | **Példa** |4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |
   
   | **Kulcs** | ToDo:TrustedCallerClientId |
   | --- | --- |
   | **Érték** |Alkalmazás - hello TodoListAPI az Azure AD alkalmazás átmásolva hívása ügyfél-azonosítója |
   | **Példa** |960adec2-b74a-484A-960adec2-b74a-484A |
10. Kattintson a **Save** (Mentés) gombra.
    
     ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)
11. A böngészőben, térjen vissza a toohello webes alkalmazás URL-CÍMÉT, és a hello kezdőlapján kattintson a hello **tooDo lista** fülre.
    
     Az idő hello alkalmazás megfelelően működik-e mert hello megbízható hívó alkalmazás és szolgáltatás egyszerű azonosító hello várt értékek.
    
     ![tooDo lista lap](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-hello-projects-from-scratch"></a>Teljesen új hello projektek felépítése
hello két Web API-projektet hello használatával létrehozott **Azure API App** projektek sablon és a ToDoList vezérlő tagjára hello alapértelmezett értékek vezérlő. Az beszerzése az Azure AD szolgáltatás egyszerű jogkivonatok hello ToDoListAPI projektben, hello [Active Directory Authentication Library (ADAL) .NET-keretrendszerhez készült](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) NuGet-csomag lett telepítve.

Információ a túl hozzon létre egy AngularJS egyoldalas alkalmazást a Web API háttérből például a ToDoListAngular, lásd: [beavatkozás nélküli a labor: egy egyetlen oldal alkalmazás (SPA) az ASP.NET Web API és Angular.js Build](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). További információ tooadd az Azure AD hitelesítési kódot, lásd: [biztonságossá tétele AngularJS egyetlen oldal alkalmazások az Azure ad-val](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Hibaelhárítás
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Győződjön meg arról, hogy azt ne tévessze össze ToDoListAPI (középső réteg) és a ToDoListDataAPI (adatrétegbeli). Például ebben az oktatóanyagban hozzáadhat hitelesítési toohello API-alkalmazás adatrétegét, **hello Alkalmazáskulcs hello Azure AD-alkalmazást, amelyet létrehozott hello középső rétegbeli API-alkalmazást kell származnia, de**.

## <a name="next-steps"></a>Következő lépések
Ez az API Apps adatsorozat hello hello utolsó oktatóanyaga. 

Azure Active Directoryval kapcsolatos további információkért tekintse meg a következő erőforrások hello.

* [Az Azure AD fejlesztői útmutatója](http://aka.ms/aaddev)
* [Az Azure AD-forgatókönyvek](http://aka.ms/aadscenarios)
* [Azure AD-minták](http://aka.ms/aadsamples)
  
    Hello [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) minta hasonlít toowhat jelenik meg ebben az oktatóanyagban, de az App Service hitelesítés nélkül.

Más módon információt a Visual Studio toodeploy tooAPI alkalmazások, projektek, vagy a Visual Studio használatával [telepítés automatizálásáról](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) a egy [Forráskezelő rendszerből](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), lásd: [hogyan toodeploy egy Azure App Service alkalmazás](../app-service-web/web-sites-deploy.md).

