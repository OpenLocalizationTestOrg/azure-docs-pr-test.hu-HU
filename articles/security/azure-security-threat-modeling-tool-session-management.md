---
title: "-Microsoft fenyegetések modellezése eszköz - kezelés Azure aaaSession |} Microsoft Docs"
description: "a fenyegetések modellezése eszköz hello felfedett fenyegetések megoldást"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 915ffae3f775ca6902fcfb93e7e1952ce85612f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-session-management--articles"></a>Biztonsági keret: Munkamenet-kezelés |} Cikkek 
| A termék vagy szolgáltatás | Cikk |
| --------------- | ------- |
| **Azure AD**    | <ul><li>[Alkalmazzon megfelelő kijelentkezési ADAL módszerek használatát, ha az Azure AD segítségével](#logout-adal)</li></ul> |
| IoT-eszközök | <ul><li>[Generált SaS-tokenje véges élettartamai használata](#finite-tokens)</li></ul> |
| **Az Azure Document DB rendszerbe** | <ul><li>[A létrehozott erőforrás-jogkivonatokat minimális jogkivonat élettartamát használata](#resource-tokens)</li></ul> |
| **ADFS** | <ul><li>[Alkalmazzon megfelelő kijelentkezési WsFederation módszerek használatát, ha az AD FS használatával](#wsfederation-logout)</li></ul> |
| **Identity Serverben** | <ul><li>[Alkalmazzon megfelelő kijelentkezési Identitáskiszolgálók használatakor](#proper-logout)</li></ul> |
| **Webalkalmazás** | <ul><li>[HTTPS-KAPCSOLATON keresztül elérhető alkalmazások biztonságos cookie-kat kell használni.](#https-secure-cookies)</li><li>[Az összes HTTP-alapú alkalmazások csak olyan cookie-k meghatározása http kell megadni](#cookie-definition)</li><li>[ASP.NET-weblapok többhelyes kérelem hamisítására (CSRF) támadások elleni](#csrf-asp)</li><li>[Állítsa be a munkamenet inaktivitás élettartama](#inactivity-lifetime)</li><li>[Alkalmazzon megfelelő kijelentkezési hello alkalmazásból](#proper-app-logout)</li></ul> |
| **Webes API** | <ul><li>[ASP.NET webes API-k többhelyes kérelem hamisítására (CSRF) támadások elleni](#csrf-api)</li></ul> |

## <a id="logout-adal"></a>Alkalmazzon megfelelő kijelentkezési ADAL módszerek használatát, ha az Azure AD segítségével

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure AD | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Ha hello alkalmazás az Azure AD által kiállított jogkivonat támaszkodik, meg kell hívnia az hello kijelentkezési eseménykezelő |

### <a name="example"></a>Példa
```C#
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a>Példa
Akkor kell felhasználói munkamenet is megsemmisítése Session.Abandon() metódus meghívásával. A következő metódus jeleníti meg a felhasználó kijelentkezik biztonságos végrehajtásának:
```C#
    [HttpPost]
        [ValidateAntiForgeryToken]
        public void LogOff()
        {
            string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            AuthenticationContext authContext = new AuthenticationContext(Authority + TenantId, new NaiveSessionCache(userObjectID));
            authContext.TokenCache.Clear();
            Session.Clear();
            Session.Abandon();
            Response.SetCookie(new HttpCookie("ASP.NET_SessionId", string.Empty));
            HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
        } 
```

## <a id="finite-tokens"></a>Generált SaS-tokenje véges élettartamai használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | IoT-eszközök | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Az IoT-központ tooAzure generált SaS-tokenje kell véges lejárati idővel rendelkeznek. Tartsa hello SaS tokent élettartama tooa minimális toolimit hello időn visszajátszani abban az esetben hello jogkivonatok kerülnek veszélybe.|

## <a id="resource-tokens"></a>A létrehozott erőforrás-jogkivonatokat minimális jogkivonat élettartamát használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az Azure Document DB rendszerbe | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Csökkentse a hello timespan erőforrás token tooa minimális érték szükséges. Erőforrás alapértelmezett 1 órás érvényes timespan lehet.|

## <a id="wsfederation-logout"></a>Alkalmazzon megfelelő kijelentkezési WsFederation módszerek használatát, ha az AD FS használatával

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | ADFS | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Hello alkalmazás STS-jogkivonatot AD FS által kibocsátott támaszkodik, hello kijelentkezési eseménykezelő WSFederationAuthenticationModule.FederatedSignOut() metódus toolog hello felhasználói ki kell hívjuk. Is hello aktuális munkamenet meg kell semmisíteni, és hello munkamenet token legyen alaphelyzetbe állítása és hatálytalanítja.|

### <a name="example"></a>Példa
```C#
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes hello user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at hello specified security token service (STS) by using hello WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of hello current session and raises hello appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at hello specified security token service (STS) by using hello WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <a id="proper-logout"></a>Alkalmazzon megfelelő kijelentkezési Identitáskiszolgálók használatakor

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Identity Serverben | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [IdentityServer3 összevont kijelentkezés](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| **Lépések** | IdentityServer hello képességét toofederate a külső Identitásszolgáltatók támogatja. Amikor egy felhasználó kijelentkezik a felsőbb rétegbeli identitásszolgáltató, attól függően, hello protokoll által használt, előfordulhat, lehetséges tooreceive értesítést amikor hello felhasználó kijelentkezik. Lehetővé teszi az ügyfelek így is bejelentkezhetnek hello felhasználó IdentityServer toonotify. Hello dokumentációjában hello references szakaszában a hello megvalósítás részletei.|

## <a id="https-secure-cookies"></a>HTTPS-KAPCSOLATON keresztül elérhető alkalmazások biztonságos cookie-kat kell használni.

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | EnvironmentType - a helyi üzemeltetésű |
| **Hivatkozások**              | [Elem (ASP.NET beállítási séma) httpCookies](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure tulajdonság](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx) |
| **Lépések** | A cookie-k általában csak elérhető toohello tartományt, amelynek hatóköre volt. Sajnos "tartományi" hello definíciója nem tartalmazza a hello protokoll úgy, hogy a cookie-k, amelyek létrejönnek a HTTPS-KAPCSOLATON keresztül elérhető HTTP Protokollon keresztül. hello "biztonságos" attribútum azt jelöli, toohello böngésző cookie-k hello csak elérhetővé kell tenni HTTPS-KAPCSOLATON keresztül. Győződjön meg arról, hogy az összes cookie-kat, állítsa be HTTPS-KAPCSOLATON keresztül hello **biztonságos** attribútum. hello követelmény hello requireSSL attribútum tootrue beállításával kényszerítheti hello web.config fájlban. Hello megközelítés előnyben részesített, mert azt kényszeríti ki a hello **biztonságos** jelenlegi és jövőbeli cookie-k nélkül hello kell toomake attribútum kód módosításokat.|

### <a name="example"></a>Példa
```C#
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
hello beállítás akkor is, ha HTTP használt tooaccess hello alkalmazás lép életbe. Ha a HTTP protokoll tooaccess hello alkalmazás, hello hello alkalmazás mivel hello cookie-k hello biztonságos attribútum és hello böngészővel nem küld őket beállítás oldaltörések biztonsági toohello alkalmazás.

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Web Forms keretrendszerre, MVC5 |
| **Attribútumok**              | EnvironmentType - a helyi üzemeltetésű |
| **Hivatkozások**              | N/A  |
| **Lépések** | Amikor hello webalkalmazás hello függő fél részére, és hello IdP ADFS-kiszolgáló, hello FedAuth jogkivonat biztonságos attribútum konfigurálható requireSSL tooTrue beállítása a `system.identityModel.services` szakasz a Web.config fájl:|

### <a name="example"></a>Példa
```C#
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <a id="cookie-definition"></a>Az összes HTTP-alapú alkalmazások csak olyan cookie-k meghatározása http kell megadni

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Biztonságos Cookie-attribútum](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| **Lépések** | toomitigate hello információfelfedés kockázatának csökkentéséhez a többhelyes scripting (lehetővé) támadás rendelkező, új attribútum - httpOnly - bevezetett toocookies volt, és az összes ismertebb böngésző támogatja. hello attribútum Megadja, hogy a cookie-k nem érhető el a parancsfájlon keresztül. HttpOnly cookie-k használatával egy webalkalmazás hello lehetőségét, hogy az hello cookie-ban tárolt bizalmas információ keresztül parancsfájl ellopják és tooan támadó webhely küldött csökkenti. |

### <a name="example"></a>Példa
Cookie-k használó összes HTTP-alapú alkalmazások kell HttpOnly hello cookie-definíció adható meg, alkalmazásával segítse a web.config konfigurációs követően:
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Web Forms keretrendszerre |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [FormsAuthentication.RequireSSL tulajdonság](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| **Lépések** | hello RequireSSL tulajdonság értéke attribútumával hello requireSSL hello konfigurációs elem értéke egy ASP.NET-alkalmazás hello konfigurációs fájlban. Megadhat hello Web.config fájl az ASP.NET alkalmazás hogy SSL (Secure Sockets Layer)-e a szükséges tooreturn hello űrlap-hitelesítési cookie-toohello kiszolgáló hello requireSSL attribútumának beállításakor.|

### <a name="example"></a>Példa 
hello alábbi példakód beállítja hello requireSSL attribútum hello Web.config fájlban.
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | MVC5 |
| **Attribútumok**              | EnvironmentType - a helyi üzemeltetésű |
| **Hivatkozások**              | [A Windows Identity Foundation (WIF) konfigurációja – II. rész](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| **Lépések** | tooset httpOnly attribútuma FedAuth cookie-kat, hideFromCsript attribútum értékét meg kell tooTrue. |

### <a name="example"></a>Példa
Következő konfigurációs hello helyes konfiguráció látható:
```XML
<federatedAuthentication>
<cookieHandler mode="Custom"
                       hideFromScript="true"
                       name="FedAuth"
                       path="/"
                       requireSsl="true"
                       persistentSessionLifetime="25">
</cookieHandler>
</federatedAuthentication>
```

## <a id="csrf-asp"></a>ASP.NET-weblapok többhelyes kérelem hamisítására (CSRF) támadások elleni

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Webhelyközi kérések hamisítására (CSRF vagy XSRF), amelyben a támadó műveleteket végezhet el hello biztonsági környezetében webhelyen egy másik felhasználói munkamenetet a támadás típusú. hello cél toomodify, vagy törölje a tartalmat, ha hello célzott webhely kizárólag támaszkodik munkamenet cookie-k tooauthenticate kérelem érkezett. A támadó a biztonsági rés beolvas egy másik felhasználó böngésző tooload egy URL-címet egy parancs egy sebezhető helyhez, amelyen hello felhasználó van bejelentkezve. Egy támadó toodo, többek között helyez el egy másik webhely, amely betölti erőforrás hello sebezhető kiszolgálóról, vagy beolvasásakor hello felhasználói tooclick egy hivatkozást a számos módja van. hello támadás megelőzhető, ha hello kiszolgáló egy további token toohello ügyfél küldi hello ügyfél tooinclude adott lexikális elem szerepel az összes későbbi kérelmek, és ellenőrzi, hogy minden későbbi kérelmek tartalmaz egy jogkivonatot, amely számítógépfiókokhoz toohello aktuális munkamenetről, például által igényel ASP.NET AntiForgeryToken hello vagy ViewState használatával. |

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | MVC5, MVC6 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [ASP.NET MVC és weblapok XSRF/CSRF megelőzése](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| **Lépések** | Kártevőirtó-CSRF és az ASP.NET MVC űrlapok - használata hello `AntiForgeryToken` segédmetódust a nézetek; put egy `Html.AntiForgeryToken()` hello űrlapra, például|

### <a name="example"></a>Példa
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a>Példa
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Példa
Hello: ugyanaz, Html.AntiForgeryToken() által biztosított hello látogató adott területre a cookie-k hívása __RequestVerificationToken, az azonos érték értékként hello véletlenszerű rejtett fent látható hello eltöltött idő Egy bejövő közzétett űrlapból toovalidate ezután hello [ValidateAntiForgeryToken] szűrő toohello cél műveletmetódus adja hozzá. Példa:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Ellenőrzi, hogy a szűrő engedélyezési:
* hello bejövő kérelem rendelkezik egy cookie-k hívása __RequestVerificationToken
* hello bejövő kérelem rendelkezik egy `Request.Form` __RequestVerificationToken nevezett bejegyzés
* A cookie-k és `Request.Form` jól értékek egyeznek, feltéve, hogy az összes, hello kérelem végighalad normál. De ha nem, majd üzenettel engedélyezési hiba "szükséges hamisításgátló jogkivonat nincs megadva vagy érvénytelen". 

### <a name="example"></a>Példa
Kártevőirtó-CSRF és AJAX: hello űrlap token az AJAX-kérelmek problémát okozhat, mert AJAX-kérelmet el tudja küldeni a JSON-adatokat, nem a HTML-űrlapot adatok. Egy megoldás, toosend hello jogkivonatok egyéni HTTP-fejlécben. hello alábbira Razor szintaxis toogenerate hello jogkivonatokat használ, és hozzáadja hello jogkivonatok tooan AJAX kérelem. 
```C#
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }

    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Példa
Hello kérelem dolgozza fel, amikor hello jogkivonatok kinyerése hello kérelemfejlécet. Majd metódushívás hello AntiForgery.Validate toovalidate hello jogkivonatokat. hello Validate metódusának hívása kivételt jelez, ha hello jogkivonatok nem érvényes.
```C#
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Web Forms keretrendszerre |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Hajtsa végre a megfelelő előny az ASP.NET beépített szolgáltatásai tooFend ki webes támadások](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| **Lépések** | A webes alapú alkalmazásokban CSRF támadások enyhíthetők ViewStateUserKey tooa véletlenszerű karakterlánc változó beállítása minden felhasználó – felhasználói Azonosítót, vagy jobban még, munkamenet-azonosító. Műszaki és közösségi okokból számos, a munkamenet-azonosító egy javulás méretezése azért, mert egy munkamenet-azonosító előre nem látható, túllépi az időkorlátot, és a felhasználónkénti alapon változik.|

### <a name="example"></a>Példa
Az összes weblapot toohave szükséges hello kód itt található:
```C#
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <a id="inactivity-lifetime"></a>Állítsa be a munkamenet inaktivitás élettartama

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [HttpSessionState.Timeout tulajdonság](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx) |
| **Lépések** | Munkamenet időkorlátja jelöli, amikor a felhasználó nem bármely művelet elvégzésére webhelyen (a webkiszolgáló által megadott) egy időszakban bekövetkező hello esemény. hello esemény, a kiszolgáló oldalán, hello felhasználói munkamenet too'invalid hello állapotának módosítása "(például" nem használható többé") és utasítsa hello web server toodestroy azt (bele tárolt összes adat törlése). hello alábbi példakód beállítja hello időtúllépés munkamenet attribútum too15 perc hello Web.config fájlban.|

### <a name="example"></a>Példa
A(z) "XML-kódot <configuration> < system.web > <sessionState mode="InProc" cookieless="true" timeout="15" /> < /system.web ></configuration>
```

## <a id="threat-detection"></a>Enable Threat detection on Azure SQL
```

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Web Forms keretrendszerre |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Elem űrlap-hitelesítés (ASP.NET beállítási séma)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx) |
| **Lépések** | Állítsa be az űrlapos hitelesítési jegyet cookie-k időtúllépési too15 hello perc|

### <a name="example"></a>Példa
A(z) "XML-kódot<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms>
```

| Title                   | Details      |
| ----------------------- | ------------ |
| **Component**               | Web Application | 
| **SDL Phase**               | Build |  
| **Applicable Technologies** | Web Forms, MVC5 |
| **Attributes**              | EnvironmentType - OnPrem |
| **References**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Steps** | When hello web application is Relying Party and ADFS is hello STS, hello lifetime of hello authentication cookies - FedAuth tokens - can be set by hello following configuration in web.config:|

### Example
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use hello code below tooenable encryption-decryption of claims received from ADFS. Thumbprint value varies based on hello certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a>Példa
Is SAML jogcímek jogkivonat élettartama kiadott AD FS beállításaként too15 hello perc, a következő powershell-parancs hello ADFS-kiszolgálón a következő hello futtatásával:
```C#
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <a id="proper-app-logout"></a>Alkalmazzon megfelelő kijelentkezési hello alkalmazásból

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Hajtsa végre megfelelő kijelentkezés hello alkalmazásból, amikor a felhasználó présgépet Kijelentkezés gombra. Követően jelentkezzen ki alkalmazás kell semmisítse meg a felhasználó munkamenetét, és is alaphelyzetbe állítása és érvényteleníti a munkamenet cookie-értéket alaphelyzetbe állítása és a hitelesítési cookie-értéket érvénytelenítését együtt. Is ha több munkamenetet a feltételekhez tooa egyetlen felhasználói identitást, azok kell együttesen lezárni hello kiszolgáló oldalán, időtúllépés vagy jelentkezzen ki. Végül győződjön meg arról, hogy kijelentkezési funkció minden oldalon érhető el. |

## <a id="csrf-api"></a>ASP.NET webes API-k többhelyes kérelem hamisítására (CSRF) támadások elleni

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Webhelyközi kérések hamisítására (CSRF vagy XSRF), amelyben a támadó műveleteket végezhet el hello biztonsági környezetében webhelyen egy másik felhasználói munkamenetet a támadás típusú. hello cél toomodify, vagy törölje a tartalmat, ha hello célzott webhely kizárólag támaszkodik munkamenet cookie-k tooauthenticate kérelem érkezett. A támadó a biztonsági rés beolvas egy másik felhasználó böngésző tooload egy URL-címet egy parancs egy sebezhető helyhez, amelyen hello felhasználó van bejelentkezve. Egy támadó toodo, többek között helyez el egy másik webhely, amely betölti erőforrás hello sebezhető kiszolgálóról, vagy beolvasásakor hello felhasználói tooclick egy hivatkozást a számos módja van. hello támadás megelőzhető, ha hello kiszolgáló egy további token toohello ügyfél küldi hello ügyfél tooinclude adott lexikális elem szerepel az összes későbbi kérelmek, és ellenőrzi, hogy minden későbbi kérelmek tartalmaz egy jogkivonatot, amely számítógépfiókokhoz toohello aktuális munkamenetről, például által igényel ASP.NET AntiForgeryToken hello vagy ViewState használatával. |

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | MVC5, MVC6 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [ASP.NET webes API-t a Webhelyközi kérések hamisítására (CSRF) támadások megelőzése](http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| **Lépések** | Kártevőirtó-CSRF és AJAX: hello űrlap token az AJAX-kérelmek problémát okozhat, mert AJAX-kérelmet el tudja küldeni a JSON-adatokat, nem a HTML-űrlapot adatok. Egy megoldás, toosend hello jogkivonatok egyéni HTTP-fejlécben. hello alábbira Razor szintaxis toogenerate hello jogkivonatokat használ, és hozzáadja hello jogkivonatok tooan AJAX kérelem. |

### <a name="example"></a>Példa
```Javascript
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }
    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Példa
Hello kérelem dolgozza fel, amikor hello jogkivonatok kinyerése hello kérelemfejlécet. Majd metódushívás hello AntiForgery.Validate toovalidate hello jogkivonatokat. hello Validate metódusának hívása kivételt jelez, ha hello jogkivonatok nem érvényes.
```C#
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

### <a name="example"></a>Példa
Kártevőirtó-CSRF és ASP.NET MVC űrlapok - használata hello AntiForgeryToken segédmetódust a nézetet. például egy Html.AntiForgeryToken() üzembe hello képernyő
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a>Példa
a fenti hello példa fog kimeneti valami hasonló hello:
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Példa
Hello: ugyanaz, Html.AntiForgeryToken() által biztosított hello látogató adott területre a cookie-k hívása __RequestVerificationToken, az azonos érték értékként hello véletlenszerű rejtett fent látható hello eltöltött idő Egy bejövő közzétett űrlapból toovalidate ezután hello [ValidateAntiForgeryToken] szűrő toohello cél műveletmetódus adja hozzá. Példa:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Ellenőrzi, hogy a szűrő engedélyezési:
* hello bejövő kérelem rendelkezik egy cookie-k hívása __RequestVerificationToken
* hello bejövő kérelem rendelkezik egy `Request.Form` __RequestVerificationToken nevezett bejegyzés
* A cookie-k és `Request.Form` jól értékek egyeznek, feltéve, hogy az összes, hello kérelem végighalad normál. De ha nem, majd üzenettel engedélyezési hiba "szükséges hamisításgátló jogkivonat nincs megadva vagy érvénytelen".

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | MVC5, MVC6 |
| **Attribútumok**              | Identity Provider - ADFS, identitásszolgáltató - az Azure AD |
| **Hivatkozások**              | [Egyes partnerek és az ASP.NET Web API 2.2 helyi bejelentkezési webes API biztonságossá tétele](http://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| **Lépések** | Ha a webes API használatával lett biztonságossá téve OAuth 2.0-s, akkor azt hello egy tulajdonosi jogkivonatot csak akkor, ha hello jogkivonat érvényes engedélyezési kérelem fejléc és biztosít toohello kérést vár. Cookie-alapú hitelesítés, ellentétben a böngészők nem csatolható hello tulajdonosi jogkivonatok toorequests. hello tulajdonosi jogkivonattal, a kérelem fejlécében hello hello kérő ügyfél tooexplicitly kell csatolni. Ezért az ASP.NET Web API-k OAuth 2.0 használatával védett, tulajdonosi jogkivonatok számít egy CSRF támadások elleni védelmet. Adjon ne feledje, hogy ha hello alkalmazás hello MVC része az űrlapos hitelesítés (azaz használ cookie-k) használ, hamisítás jogkivonatok toobe hello MVC webalkalmazás használják. |

### <a name="example"></a>Példa
Webes API hello rendelkezik toobe toorely küldjenek csak tulajdonosi jogkivonatokat és nem a cookie-kat. A konfiguráció a következő hello úgy teheti `WebApiConfig.Register` metódus: "" C-éles kód config. SuppressDefaultHostAuthentication(); Config. Filters.Add (új HostAuthenticationFilter(OAuthDefaults.AuthenticationType));
```
hello SuppressDefaultHostAuthentication method tells Web API tooignore any authentication that happens before hello request reaches hello Web API pipeline, either by IIS or by OWIN middleware. That way, we can restrict Web API tooauthenticate only using bearer tokens.
