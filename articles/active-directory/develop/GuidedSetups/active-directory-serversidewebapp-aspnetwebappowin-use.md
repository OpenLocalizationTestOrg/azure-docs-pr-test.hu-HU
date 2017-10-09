---
title: "aaaAzure AD v2 ASP.NET Web Server első lépések – használata |} Microsoft Docs"
description: "A Microsoft bejelentkezés végrehajtási egy ASP.NET-megoldás a hagyományos böngészőalapú webalkalmazás a szabványos OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 03afce6fa6598215e8c4af841c00762c143a0cd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="add-a-controller-toohandle-sign-in-and-sign-out-requests"></a>A vezérlő toohandle be- és kijelentkezési kérések

A lépés bemutatja, hogyan toocreate egy új tartományvezérlő tooexpose bejelentkezés és kijelentkezési metódusokat.

1.  Kattintson jobb gombbal a hello `Controllers` mappára, és válassza`Add` > `Controller`
2.  Válassza a(z) `MVC (.NET version) Controller – Empty` lehetőséget.
3.  Kattintson a *hozzáadása*
4.  Nevezze el `HomeController` kattintson *hozzáadása*
5.  Adja hozzá *OWIN* toohello osztály hivatkozik:

```csharp
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
```
<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Vegye fel a hello két módszer toohandle bejelentkezési és kijelentkezési tooyour vezérlő alább egy hitelesítési kérdés kód kezdeményezése szerint:
</li>
</ol>

```csharp
/// <summary>
/// Send an OpenID Connect sign-in request.
/// Alternatively, you can just decorate hello SignIn method with hello [Authorize] attribute
/// </summary>
public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties{ RedirectUri = "/" },
            OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
    HttpContext.GetOwinContext().Authentication.SignOut(
            OpenIdConnectAuthenticationDefaults.AuthenticationType,
            CookieAuthenticationDefaults.AuthenticationType);
}
```

## <a name="create-hello-apps-home-page-toosign-in-users-via-a-sign-in-button"></a>A bejelentkezési gomb felhasználókat hello alkalmazás kezdőlapját toosign létrehozása

A Visual Studio hozzon létre egy új nézet tooadd hello bejelentkezés gombra, és hitelesítést követően a felhasználói adatok megjelenítése:

1.  Kattintson jobb gombbal a hello `Views\Home` mappára, és válassza`Add View`
2.  Nevezze el a következőképpen: `Index`.
3.  Adja hozzá a hello HTML, amely hello bejelentkezési gomb, toohello fájl tartalmazza a következő:

```html
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Sign-In with Microsoft Guide</title>
</head>
<body>
@if (!Request.IsAuthenticated)
{
    <!-- If hello user is not authenticated, display hello sign-in button -->
    <a href="@Url.Action("SignIn", "Home")" style="text-decoration: none;">
        <svg xmlns="http://www.w3.org/2000/svg" xml:space="preserve" width="300px" height="50px" viewBox="0 0 3278 522" class="SignInButton">
        <style type="text/css">.fil0:hover {fill: #4B4B4B;} .fnt0 {font-size: 260px;font-family: 'Segoe UI Semibold', 'Segoe UI'; text-decoration: none;}</style>
        <rect class="fil0" x="2" y="2" width="3174" height="517" fill="black" />
        <rect x="150" y="129" width="122" height="122" fill="#F35325" />
        <rect x="284" y="129" width="122" height="122" fill="#81BC06" />
        <rect x="150" y="263" width="122" height="122" fill="#05A6F0" />
        <rect x="284" y="263" width="122" height="122" fill="#FFBA08" />
        <text x="470" y="357" fill="white" class="fnt0">Sign in with Microsoft</text>
        </svg>
    </a>
}
else
{
    <span><br/>Hello @System.Security.Claims.ClaimsPrincipal.Current.FindFirst("name").Value;</span>
    <br /><br />
    @Html.ActionLink("See Your Claims", "Index", "Claims")
    <br /><br />
    @Html.ActionLink("Sign out", "SignOut", "Home")
}
@if (!string.IsNullOrWhiteSpace(Request.QueryString["errormessage"]))
{
    <div style="background-color:red;color:white;font-weight: bold;">Error: @Request.QueryString["errormessage"]</div>
}
</body>
</html>
```
<!--start-collapse-->
### <a name="more-information"></a>További információ
> Ezen a lapon egy fekete háttérrel SVG-formátumban ad hozzá a Bejelentkezés gombra:<br/>![Jelentkezzen be Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-use/aspnetsigninbuttonsample.png)<br/> A több bejelentkezési gomb, lépjen a toohello [ezen a lapon](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "irányelvek Branding").
<!--end-collapse-->

## <a name="add-a-controller-toodisplay-users-claims"></a>A vezérlő toodisplay felhasználói jogcímeket adhatnak hozzá
Ez a vezérlő bemutatja hello használatára hello `[Authorize]` tooprotect vezérlő attribútumát. Ez az attribútum csak a hitelesített felhasználók lehetővé toohello vezérlő korlátozza. az alábbi kód hello hello toodisplay felhasználói jogcímeket, amely részeként hello bejelentkezés lekérése használ.

1.  Kattintson jobb gombbal a hello `Controllers` mappába:`Add` > `Controller`
2.  Válassza a(z) `MVC {version} Controller – Empty` lehetőséget.
3.  Kattintson a *hozzáadása*
4.  Nevezze el`ClaimsController`
5.  Hello kódot a vezérlő osztályhoz az alábbi - hello kóddal, ez a gyorsjavítás hello `[Authorize]` toohello attribútumosztály:

```csharp
[Authorize]
public class ClaimsController : Controller
{
    /// <summary>
    /// Add user's claims tooviewbag
    /// </summary>
    /// <returns></returns>
    public ActionResult Index()
    {
        var claimsPrincipalCurrent = System.Security.Claims.ClaimsPrincipal.Current;
        //You get hello user’s first and last name below:
        ViewBag.Name = claimsPrincipalCurrent.FindFirst("name").Value;

        // hello 'preferred_username' claim can be used for showing hello username
        ViewBag.Username = claimsPrincipalCurrent.FindFirst("preferred_username").Value;

        // hello subject claim can be used toouniquely identify hello user across hello web
        ViewBag.Subject = claimsPrincipalCurrent.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier).Value;

        // TenantId is hello unique Tenant Id - which represents an organization in Azure AD
        ViewBag.TenantId = claimsPrincipalCurrent.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;

        return View();
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>További információ
> Hello hello használata miatt `[Authorize]` csak akkor hajtható végre attribútum, a tartományvezérlő minden módszert, ha hello felhasználó hitelesítése. Ha hello felhasználó nincs hitelesítve, és megpróbál tooaccess hello vezérlő, OWIN egy hitelesítési kérdés kezdeményezése, és hello felhasználói tooauthenticate kényszerítése. megállapítja, hogy hello fent hello kód jogcímek hello gyűjteménye `ClaimsPrincipal.Current` adott felhasználói attribútumok a tokenben hello felhasználói példányt. Ezek az attribútumok hello felhasználó teljes nevét és a felhasználónév, valamint hello globális felhasználói azonosító tulajdonos tartalmazza. Hello is tartalmaz *Bérlőazonosító*, amely hello felhasználó szervezete hello Azonosítóját jelöli. 
<!--end-collapse-->

## <a name="create-a-view-toodisplay-hello-users-claims"></a>Nézet létrehozása toodisplay hello felhasználói jogcímek

Új nézet létrehozása a Visual Studio, a toodisplay hello felhasználói jogcímeket egy weblapon:

1.  Kattintson jobb gombbal a hello `Views\Claims` mappa és:`Add View`
2.  Nevezze el a következőképpen: `Index`.
3.  Adja hozzá a következő HTML toohello fájl hello:

```html
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Sign-In with Microsoft Sample</title>
    <link href="@Url.Content("~/Content/bootstrap.min.css")" rel="stylesheet" type="text/css" />
</head>
<body style="padding:50px">
    <h3>Main Claims:</h3>
    <table class="table table-striped table-bordered table-hover">
        <tr><td>Name</td><td>@ViewBag.Name</td></tr>
        <tr><td>Username</td><td>@ViewBag.Username</td></tr>
        <tr><td>Subject</td><td>@ViewBag.Subject</td></tr>
        <tr><td>TenantId</td><td>@ViewBag.TenantId</td></tr>
    </table>
    <br />
    <h3>All Claims:</h3>
    <table class="table table-striped table-bordered table-hover table-condensed">
    @foreach (var claim in System.Security.Claims.ClaimsPrincipal.Current.Claims)
    {
        <tr><td>@claim.Type</td><td>@claim.Value</td></tr>
    }
</table>
    <br />
    <br />
    @Html.ActionLink("Sign out", "SignOut", "Home", null, new { @class = "btn btn-primary" })
</body>
</html>
```
