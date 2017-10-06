---
title: "aaaAzure AD .NET webes API bevezetés |} Microsoft Docs"
description: "Hogyan toobuild egy .NET MVC webes API-t, amely integrálható az Azure AD a hitelesítéshez és engedélyezéshez."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 91c93e1fe18855f5648076e59e2ccf081eec34bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a>A webes API védelme az Azure AD tulajdonosi jogkivonatok segítségével
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Ha egy alkalmazás, amely hozzáférést biztosít a tooprotected erőforrások még felépítése, szükség van-e tooknow hogyan férhetnek hozzá a jogosulatlan tooprevent toothose erőforrásokat.
Azure Active Directory (Azure AD) egyszerűen és egyszerű toohelp webes API-k védelme csak néhány sornyi kód OAuth 2.0 tulajdonosi hozzáférési jogkivonatok használatával.

Az ASP.NET web apps Microsoft általi implementációja hello hello közösségi szerkesztésű OWIN köztes szerepel a .NET-keretrendszer 4.5 használatával végezheti el a védelmet. Itt a "lista tooDo" webes API OWIN toobuild fogjuk használni, amelyek:

* Jelöl ki, amelyek API-k védett.
* Ellenőrzi, hogy hello webes API-hívások tartalmaznak egy érvényes jogkivonat.

toobuild hello tooDo lista API, először kell:

1. Alkalmazás regisztrálása az Azure AD-ben.
2. Állítson be hello app toouse hello OWIN hitelesítési folyamatot.
3. Ügyfél alkalmazás toocall hello webes API-k konfigurálása.

elindult, tooget [hello app vázat letöltése](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) vagy [letöltése befejeződött hello minta](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Egy Visual Studio 2013 megoldást. Szükség mely tooregister az Azure AD-bérlő az alkalmazást. Ha Ön nem rendelkezik ilyennel, [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>1. lépés: Egy alkalmazás regisztrálása az Azure ad szolgáltatással
toohelp az alkalmazás biztonságos, akkor először kell toocreate egy alkalmazás az Ön bérlőjében, és adjon meg néhány kulcsfontosságú adatokat az Azure AD.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

2. Hello felső sávon kattintson a fiókját. A hello **Directory** menüben válassza ki a kívánt tooregister hello Azure AD-bérlő az alkalmazást.

3. Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.

4. Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.

5. Hello utasításokat követve, és hozzon létre egy új **webalkalmazás és/vagy webes API**.
  * **Név** az alkalmazás toousers ismerteti. Adja meg **tooDo lista szolgáltatás**.
  * **Átirányítási Uri** által az Azure AD használt a jogkivonatokat, hogy az alkalmazásban kérte tooreturn protokollt és a karakterlánc kombinációját. Adja meg `https://localhost:44321/` ehhez az értékhez.

6. A hello **beállítások** -> **tulajdonságok** az alkalmazás lapján hello App ID URI frissítése. Adjon meg egy bérlő-specifikus azonosítója. Adja meg például a következőt: `https://contoso.onmicrosoft.com/TodoListService`.

7. Hello konfigurációjának mentéséhez. Hagyja nyitva, hello portal, mert azt is el kell tooregister az ügyfélalkalmazást hamarosan.

## <a name="step-2-set-up-hello-app-toouse-hello-owin-authentication-pipeline"></a>2. lépés: Hello app toouse hello OWIN hitelesítési folyamatot beállítása
toovalidate bejövő kérelmek és jogkivonatok, tooset kell az alkalmazás toocommunicate az Azure AD-be.

1. toobegin, nyissa meg hello megoldást, és adja hozzá a hello OWIN köztes NuGet csomagjainak toohello TodoListService projekt hello Csomagkezelő konzol használatával.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. Adja hozzá egy OWIN indítási osztály toohello TodoListService projekt nevű `Startup.cs`.  Kattintson a jobb gombbal hello projektre, válassza **Hozzáadás** > **új elem**, majd keresse meg a **OWIN**. hello OWIN köztes által aktivált hello `Configuration(…)` módszer az alkalmazás indításakor.

3. Hello osztálydeklaráció túl módosítása`public partial class Startup`. Azt korábban már megvalósított Ez az osztály tartozik, egy másik fájlban. A hello `Configuration(…)` módszer, túl hívás`ConfgureAuth(…)` tooset hitelesítés a webalkalmazás.

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. Nyissa meg hello fájl `App_Start\Startup.Auth.cs` és valósíthatnak meg hello `ConfigureAuth(…)` metódust. hello által a paraméterek `WindowsAzureActiveDirectoryBearerAuthenticationOptions` erre a célra az Azure ad alkalmazás toocommunicate koordinátáit.

    ```C#
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
            });
    }
    ```

5. Mostantól a `[Authorize]` attribútumok toohelp megvédje a tartományvezérlőket és a műveletek JSON webes jogkivonat (JWT) tulajdonosi hitelesítéssel. Adja a hello `Controllers\TodoListController.cs` osztály engedélyezés címke használatával. Ezzel kikényszeríti a hello felhasználói toosign a lap elérése előtt.

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Amikor egy jogosult hívó sikeresen hív meg egyik hello `TodoListController` API-k, hello művelet is kell hozzáférhetnek tooinformation hello hívó kapcsolatban. OWIN hozzáférés toohello jogcímeket belül hello tulajdonosi jogkivonattal hello keresztül biztosít `ClaimsPrincpal` objektum.  

6. Kötelező megadni a webes API-k toovalidate hello "hatókör" hello lexikális elem szerepel. Ez biztosítja, hogy hello a felhasználó hozzájárult toohello engedélyek szükséges tooaccess hello tooDo lista szolgáltatás.

    ```C#
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is hello default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "hello Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. Nyissa meg hello `web.config` hello TodoListService projekt gyökerében hello fájlt, és adja meg a konfigurációs értékek hello `<appSettings>` szakasz.
  * `ida:Tenant`az Azure AD-bérlőn – például a contoso.onmicrosoft.com hello név.
  * `ida:Audience`az App ID URI hello alkalmazás hello Azure-portálon megadott hello.

## <a name="step-3-configure-a-client-application-and-run-hello-service"></a>3. lépés: Ügyfélalkalmazás konfigurálása és hello szolgáltatás futtatásához
Ahhoz, hogy hello tooDo lista szolgáltatás működés közben, tooconfigure hello tooDo lista ügyfél szüksége, hogy azok jogkivonatokhoz az Azure AD, és ellenőrizze a hívások toohello szolgáltatás.

1. Lépjen vissza toohello [Azure-portálon](https://portal.azure.com).

2. Új alkalmazás létrehozása az Azure AD-bérlőn, és válassza ki **natív ügyfélalkalmazás** hello eredményül kapott kér.
  * **Név** az alkalmazás toousers ismerteti.
  * Adja meg `http://TodoListClient/` a hello **átirányítási Uri-** érték.

3. Regisztráció befejezése után az Azure AD egy egyedi alkalmazás azonosítója tooyour app rendeli hozzá. Ez az érték kell a következő lépésekkel hello, ezért másolja hello alkalmazás oldalról.

4. A hello **beállítások** lapon jelölje be **szükséges engedélyek**, majd válassza ki **Hozzáadás**. Keresse meg és válassza ki a lista szolgáltatás hello tooDo, adja hozzá a hello **hozzáférés TodoListService** engedélyt a **delegált engedélyek**, és kattintson a **végzett**.

5. A Visual Studióban nyissa meg a `App.config` hello TodoListClient projektet, és adja meg a konfigurációs értékek hello `<appSettings>` szakasz.

  * `ida:Tenant`az Azure AD-bérlőn – például a contoso.onmicrosoft.com hello név.
  * `ida:ClientId`a rendszer hello Azure-portálon fájlból másolt hello alkalmazás Azonosítóját.
  * `todo:TodoListResourceId`hello App ID URI-azonosítója hello tooDo hello Azure-portálon megadott lista szolgáltatásalkalmazás.

## <a name="next-steps"></a>Következő lépések
Végül törlése, elkészítéséhez és futtatása minden olyan projekthez. Ha még nem tette meg, most már áll hello idő toocreate az Ön bérlőjében rendelkező új felhasználót a *. onmicrosoft.com tartományt. Toohello tooDo lista ügyfél számára, hogy a felhasználó bejelentkezhet, és adja hozzá az egyes feladatok toohello felhasználók feladatlistáit.

Referenciaként befejeződött hello mintát (a konfigurációs értékek nélkül) érhető el a [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Most már továbbléphet toomore identitás forgatókönyvek.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
