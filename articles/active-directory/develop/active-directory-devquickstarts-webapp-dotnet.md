---
title: "aaaAzure AD .NET web app bevezetés |} Microsoft Docs"
description: "A .NET MVC webes alkalmazás, amely az Azure AD-bejelentkezés létrehozása."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e15a41a4-dc5d-4c90-b3fe-5dc33b9a1e96
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 6d3098c9e3d7e1916ccb110c703f501ae52e788f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a>ASP.NET webalkalmazás be- és kijelentkezési az Azure ad szolgáltatással
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Be- és kijelentkezési egyetlen azáltal csak néhány sornyi kódot, Azure Active Directory (Azure AD) egyszerűen meg toooutsource webalkalmazás identitáskezeléshez. Regisztrálhat a felhasználók ASP.NET webes alkalmazások mindkét hello Microsoft általi implementációja Open Web Interface .NET (OWIN) köztes használatával. Közösségi szerkesztésű OWIN köztes a .NET-keretrendszer 4.5 része. Ez a cikk bemutatja, hogyan toouse OWIN számára:

* A felhasználók beléptetése tooweb alkalmazásokban az Azure AD hello identitás-szolgáltatóként.
* Bizonyos felhasználói információk jelennek meg.
* A felhasználók hello alkalmazások kívül.

## <a name="before-you-get-started"></a>A kezdés előtt
* Töltse le a hello [app vázat](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) vagy töltse le a hello [elkészült mintát](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).
* Meg kell az Azure AD-bérlő mely tooregister hello alkalmazásban is. Ha még nem telepítette az Azure AD-bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).

Ha készen áll, hello eljárásokat hajtsa végre a következő négy részből áll hello.

## <a name="step-1-register-hello-new-app-with-azure-ad"></a>1. lépés: Hello új alkalmazás regisztrálása az Azure ad szolgáltatással
tooset hello app tooauthenticate felhasználókat, először regisztrálja az Ön bérelt szolgáltatásának hello következő tevékenységek végrehajtásával:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello felső sávon kattintson a fiók nevére. A hello **Directory** listán, válassza hello tooregister hello alkalmazás kívánt Active Directory-bérlő.
3. Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.
4. Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.
5. Hajtsa végre a hello kérni fogja az új toocreate **webalkalmazás és/vagy WebAPI**.
  * **Név** hello app toousers ismerteti.
  * **Bejelentkezési URL-cím** hello alap URL-cím hello alkalmazás. hello vázat alapértelmezett URL-címe: https://localhost:44320 /.
6. Hello regisztráció befejezését követően az Azure AD hozzárendel hello app egy egyedi azonosítót. Másolja ki hello app lap toouse hello következő szakaszokban lévő hello érték.
7. A hello **beállítások** -> **tulajdonságok** az alkalmazás lapján hello App ID URI frissítése. Hello **App ID URI** hello alkalmazás egyedi azonosítója. hello elnevezési konvenció: `https://<tenant-domain>/<app-name>` (például `https://contoso.onmicrosoft.com/my-first-aad-app`).

## <a name="step-2-set-up-hello-app-toouse-hello-owin-authentication-pipeline"></a>2. lépés: Hello app toouse hello OWIN hitelesítési folyamatot beállítása
Ez a lépés konfigurál hello OWIN köztes toouse hello OpenID Connect hitelesítési protokoll. Használjon OWIN tooissue be- és kijelentkezési kérések, kezelni a felhasználói munkameneteket, felhasználói adatok beolvasása, és így tovább.

1. toobegin, hello OWIN köztes NuGet csomagjainak toohello projekt hozzáadása hello Csomagkezelő konzol használatával.

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. az OWIN indítási osztály toohello projektet nevezik tooadd `Startup.cs`, kattintson a jobb gombbal a projekt hello, válassza ki **Hozzáadás**, jelölje be **új elem**, majd keresse meg a **OWIN**. hello OWIN köztes hív meg hello **Configuration(...)**  metódus hello alkalmazás indításakor.
3. Hello osztálydeklaráció túl módosítása`public partial class Startup`. Azt korábban már megvalósított Ez az osztály tartozik, egy másik fájlban. A hello **Configuration(...)**  módszer, túl hívás**ConfgureAuth(...)**  tooset hello alkalmazás hitelesítést.  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. Nyissa meg a hello App_Start\Startup.Auth.cs fájlt, és megvalósításához hello **ConfigureAuth(...)**  metódust. Megadja a paraméterek hello *OpenIDConnectAuthenticationOptions* hello app toocommunicate az Azure AD-koordináták szolgálnak. Szükség tooset cookie-k hitelesítés, mert hello OpenID Connect köztes hello háttérben cookie-kat használ.

     ```C#
     public void ConfigureAuth(IAppBuilder app)
     {
         app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

         app.UseCookieAuthentication(new CookieAuthenticationOptions());

         app.UseOpenIdConnectAuthentication(
             new OpenIdConnectAuthenticationOptions
             {
                 ClientId = clientId,
                 Authority = authority,
                 PostLogoutRedirectUri = postLogoutRedirectUri,
                 Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = context =>
                        {
                            context.HandleResponse();
                            context.Response.Redirect("/Error?message=" + context.Exception.Message);
                            return Task.FromResult(0);
                        }
                    }
             });
     }
     ```

5. Nyissa meg hello web.config fájl hello hello projekt gyökérkönyvtárában, és adja meg az hello konfigurációs értékeket hello `<appSettings>` szakasz.
  * `ida:ClientId`: hello GUID, átmásolva hello Azure portálra az "1. lépés: Register hello új alkalmazást az Azure ad szolgáltatással."
  * `ida:Tenant`: az Azure AD-bérlő (például contoso.onmicrosoft.com) hello nevét.
  * `ida:PostLogoutRedirectUri`: hello kijelző, amely közli az Azure AD, ha a rendszer átirányítja az a felhasználó a kijelentkezési kérés sikeres végrehajtása után.

## <a name="step-3-use-owin-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a>: 3. lépés OWIN tooissue be- és kijelentkezési kérések tooAzure AD
hello alkalmazás mostantól az Azure ad-val megfelelően konfigurált toocommunicate hello OpenID Connect hitelesítési protokoll használatával. OWIN kezelt összes hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartásáról a felhasználói munkamenetek hello részleteit. Hogy továbbra is van toogive a felhasználók egy a módon toosign és kijelentkezett.

1. Használhatja a tartományvezérlők toorequire felhasználók toosign a címkék engedélyezi bizonyos lapok való hozzáférés előtt. toodo Igen, nyissa meg a Controllers\HomeController.cs, és adja hozzá a hello `[Authorize]` toohello vezérlő vonatkozó címke.

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. OWIN toodirectly probléma a érkező hitelesítési kéréseket a kód is használható. toodo nyisson meg Controllers\AccountController.cs. Ezt követően a hello SignIn() és SignOut() műveletek adja ki az OpenID Connect kérdés és kijelentkezési kérések.

     ```C#
     public void SignIn()
     {
         // Send an OpenID Connect sign-in request.
         if (!Request.IsAuthenticated)
         {
             HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
         }
     }
     public void SignOut()
     {
         // Send an OpenID Connect sign-out request.
         HttpContext.GetOwinContext().Authentication.SignOut(
              OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
     }
     ```

3. Nyissa meg a Views\Shared\_LoginPartial.cshtml tooshow hello felhasználói hello app be- és kijelentkezési hivatkozásokat, és tooprint kimenő hello felhasználónév nézetben.

    ```HTML
    @if (Request.IsAuthenticated)
    {
     <text>
         <ul class="nav navbar-nav navbar-right">
             <li class="navbar-text">
                 Hello, @User.Identity.Name!
             </li>
             <li>
                 @Html.ActionLink("Sign out", "SignOut", "Account")
             </li>
         </ul>
     </text>
    }
    else
    {
     <ul class="nav navbar-nav navbar-right">
         <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
     </ul>
    }
    ```

## <a name="step-4-display-user-information"></a>4. lépés: Felhasználói információk megjelenítése
Amikor a felhasználók az OpenID Connect, az Azure AD, hogy a "jogcímek" vagy a helyességi feltételek hello felhasználóról tartalmaz id_token toohello alkalmazások adja vissza. Használhatja a jogcímalapú toopersonalize hello alkalmazás hello következő tevékenységek végrehajtásával:

1. Nyissa meg a hello Controllers\HomeController.cs fájlt. A tartományvezérlők keresztül hello keresztül elérhető hello felhasználói jogcímek `ClaimsPrincipal.Current` rendszerbiztonsági objektumot.

 ```C#
 public ActionResult About()
 {
     ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
     ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
     ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
     ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
     ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

     return View();
 }
 ```

2. Hozza létre, és futtassa hello alkalmazást. Ha egy új felhasználó még nem hozott létre, hogy az Ön bérlőjében egy onmicrosoft.com tartománnyal, most már áll hello idő toodo stb. Ezt a következőképpen teheti meg:

  a. Jelentkezzen be, hogy a felhasználó, és vegye figyelembe, hogyan jelennek meg hello felső sávon hello felhasználói azonosító.

  b. Jelentkezzen ki, majd jelentkezzen be egy másik felhasználó az Ön bérelt szolgáltatásának.

  c. Különösen gondolkodhat már, ha regisztrálja, és futtassa az alkalmazást (a saját clientId) egy másik példánya, és egyszeri bejelentkezés működés közben bemutató.

## <a name="next-steps"></a>Következő lépések
Referenciáért lásd: [befejeződött hello minta](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (a konfigurációs értékek nélkül).

Most már továbbléphet a témakörök speciális toomore. Például [egy webes API-t az Azure ad-vel biztonságos](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
