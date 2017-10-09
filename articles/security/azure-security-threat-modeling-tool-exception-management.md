---
title: "-Microsoft fenyegetések modellezése eszköz - kezelés Azure aaaException |} Microsoft Docs"
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
ms.openlocfilehash: 247096c10deeca94ebb9b19df7ba60e442ca1e4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-exception-management--mitigations"></a>Biztonsági keret: Kivételek kezelése |} Megoldást 
| A termék vagy szolgáltatás | Cikk |
| --------------- | ------- |
| **WCF** | <ul><li>[WCF - ne szerepeljen az serviceDebug csomópont a konfigurációs fájl](#servicedebug)</li><li>[WCF - ne szerepeljen az serviceMetadata csomópont a konfigurációs fájl](#servicemetadata)</li></ul> |
| **Webes API** | <ul><li>[Győződjön meg arról, hogy a megfelelő kivételkezelést történik-e az ASP.NET Web API](#exception)</li></ul> |
| **Webalkalmazás** | <ul><li>[Nem teszi közzé a látható a hibaüzenetekben biztonsági részletei](#messages)</li><li>[Alapértelmezett hibakezelési lap végrehajtása](#default)</li><li>[Az üzembe helyezési módszer tooRetail beállítása az IIS-ben](#deployment)</li><li>[Kivételek biztonságosan kell-e sikertelen](#fail)</li></ul> |

## <a id="servicedebug"></a>WCF - ne szerepeljen az serviceDebug csomópont a konfigurációs fájl

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, NET-keretrendszer 3 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg Királyság](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Lépések** | A Windows kommunikációs keretrendszer (WCF) szolgáltatásait konfigurált tooexpose hibakeresési információ is lehet. Hibakeresési információ nem használható üzemi környezetben. Hello `<serviceDebug>` címke határozza meg, hogy hello hibakeresési információ szolgáltatás engedélyezve van-e a WCF-szolgáltatás számára. Ha hello attribútum includeExceptionDetailInFaults tootrue, kivételek adatai hello alkalmazásból visszaadott tooclients. A támadók kihasználhatják a hello további információt a kimeneti toomount támadások megcélzott hello keretrendszer, adatbázisok vagy más hello alkalmazás által használt erőforrások hibakeresés kapnak. |

### <a name="example"></a>Példa
hello következő konfigurációs fájl tartalmaz hello `<serviceDebug>` címke: 
```
<configuration> 
<system.serviceModel> 
<behaviors> 
<serviceBehaviors> 
<behavior name=""MyServiceBehavior""> 
<serviceDebug includeExceptionDetailInFaults=""True"" httpHelpPageEnabled=""True""/> 
... 
```
Tiltsa le a hibakeresési adatok hello szolgáltatásban. Hello eltávolításával ehhez `<serviceDebug>` címke az alkalmazás konfigurációs fájljából. 

## <a id="servicemetadata"></a>WCF - ne szerepeljen az serviceMetadata csomópont a konfigurációs fájl

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Általános, NET-keretrendszer 3 |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg Királyság](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Lépések** | Nyilvánosan kitettségének szolgáltatás információt biztosíthat támadók számára azok hogyan hello szolgáltatást is elegendő értékes betekintést. Hello `<serviceMetadata>` címke engedélyezi hello metaadatok közzétételi szolgáltatást. Szolgáltatás metaadatai tartalmazhatnak bizalmas adatokat, amelyeket nem kell nyilvánosan elérhető. Minimális csak a megbízható felhasználóknak engedélyezze tooaccess hello metaadatok, és győződjön meg arról, hogy a szükségtelen adatokat nem lesz közzétéve. Még jobb teljes mértékben letiltása hello képességét toopublish metaadatok. A biztonságos WCF-konfiguráció nem tartalmaz hello `<serviceMetadata>` címke. |

## <a id="exception"></a>Győződjön meg arról, hogy a megfelelő kivételkezelést történik-e az ASP.NET Web API

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | MVC 5, 6 MVC |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [ASP.NET webes API-t a kivételkezelő](http://www.asp.net/web-api/overview/error-handling/exception-handling), [-ellenőrzés az ASP.NET Web API minta](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Lépések** | Alapértelmezés szerint az ASP.NET Web API leginkább a nem kezelt kivételek lefordítását egy HTTP-válasz állapotkód:`500, Internal Server Error`|

### <a name="example"></a>Példa
toocontrol hello hello API-t által visszaadott állapotkód `HttpResponseException` alább látható módon használhatja: 
```C#
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        throw new HttpResponseException(HttpStatusCode.NotFound);
    }
    return item;
}
```

### <a name="example"></a>Példa
Hello kivétel válasz további vezérlő hello `HttpResponseMessage` osztály használható a lent látható módon: 
```C#
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        var resp = new HttpResponseMessage(HttpStatusCode.NotFound)
        {
            Content = new StringContent(string.Format("No product with ID = {0}", id)),
            ReasonPhrase = "Product ID Not Found"
        }
        throw new HttpResponseException(resp);
    }
    return item;
}
```
toocatch kezeletlen kivételek fellépése, amelyek nincsenek hello típusú `HttpResponseException`, kivétel szűrők segítségével. Kivétel szűrők megvalósítása hello `System.Web.Http.Filters.IExceptionFilter` felületet. hello legegyszerűbb módja toowrite egy kivételszűrőbe a hello tooderive `System.Web.Http.Filters.ExceptionFilterAttribute` osztályhoz, és írja felül hello OnException metódust. 

### <a name="example"></a>Példa
Íme egy szűrő, amely átalakítja `NotImplementedException` kivételek be HTTP-állapotkód `501, Not Implemented`: 
```C#
namespace ProductStore.Filters
{
    using System;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http.Filters;

    public class NotImplExceptionFilterAttribute : ExceptionFilterAttribute 
    {
        public override void OnException(HttpActionExecutedContext context)
        {
            if (context.Exception is NotImplementedException)
            {
                context.Response = new HttpResponseMessage(HttpStatusCode.NotImplemented);
            }
        }
    }
}
```

Számos módon tooregister egy webes API kivételszűrőbe van:
- Művelet
- Vezérlő
- Globálisan

### <a name="example"></a>Példa
tooapply hello tooa bizonyos művelet szűréséhez hello szűrő attribútum toohello művelet hozzáadása: 
```C#
public class ProductsController : ApiController
{
    [NotImplExceptionFilter]
    public Contact GetContact(int id)
    {
        throw new NotImplementedException("This method is not implemented");
    }
}
```
### <a name="example"></a>Példa
tooapply hello szűrő tooall hello műveletek egy `controller`, adja hozzá hello szűrő egy attribútum toohello `controller` osztály: 

```C#
[NotImplExceptionFilter]
public class ProductsController : ApiController
{
    // ...
}
```

### <a name="example"></a>Példa
tooapply hello szűrése globálisan tooall Web API tartományvezérlőket, adjon hozzá egy hello szűrő toohello `GlobalConfiguration.Configuration.Filters` gyűjtemény. A gyűjtemény kivételszűrők tooany Web API vezérlő műveletének alkalmazása. 
```C#
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a>Példa
A modell-ellenőrzéshez hello modell állapota átadhatók tooCreateErrorResponse metódus alább látható módon: 
```C#
public HttpResponseMessage PostProduct(Product item)
{
    if (!ModelState.IsValid)
    {
        return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
    }
    // Implementation not shown...
}
```

Hello references szakaszában kivételes kezelésére és a modell-ellenőrzés az ASP.Net Web API kapcsolatos további részletekért ellenőrizze hello hivatkozások 

## <a id="messages"></a>Nem teszi közzé a látható a hibaüzenetekben biztonsági részletei

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Általános hiba üzenetek biztosított közvetlenül toohello felhasználó nélkül az alkalmazás bizalmas adatokat is beleértve. Bizalmas adatok közé tartoznak például:</p><ul><li>Kiszolgáló neve</li><li>Kapcsolati karakterláncok</li><li>Felhasználónevek</li><li>Jelszavak</li><li>SQL-eljárások</li><li>A dinamikus SQL-hibák részletei</li><li>A veremkivonatot és sornyi kód</li><li>A memóriában tárolt változók</li><li>Meghajtó és mappa</li><li>Alkalmazás telepítése pontok</li><li>Gazdagép konfigurációs beállításai</li><li>Egyéb részleteket a belső alkalmazás</li></ul><p>Az alkalmazáson belül az összes hiba túltöltés és nyújtó általános hibaüzenetek, valamint IIS belül az egyéni hibák engedélyezése segítségével információ felfedésének megakadályozására. SQL Server-adatbázis és a .NET kivételkezelő, más hibakezelési architektúrák mellett között is a különösen részletes és rendkívül hasznos tooa rosszindulatú felhasználó adatainak összegyűjtése az alkalmazás. Akkor hajtsa végre, nem közvetlenül a megjelenítési hello osztály tartalmát hello .NET kivétel osztályból származó, és győződjön meg arról, hogy megfelelő kivételkezelést, hogy véletlenül nem váratlan kivétel kiváltása közvetlenül toohello felhasználói.</p><ul><li>Adja meg, általános hibaüzenet közvetlenül található közvetlenül a hello kivétel/hibaüzenet jelenik meg rögtön részletes absztrakt toohello felhasználó</li><li>Tegye nem megjelenítési hello tartalmát a .NET-kivételt osztályban közvetlenül toohello felhasználói</li><li>Feldolgozzák-e az összes hibaüzenet, és adott esetben tájékoztatják egy általános hiba üzenetet küldött toohello alkalmazás ügyfél hello felhasználó</li><li>Nem teszi közzé hello kivételosztály hello tartalmát közvetlenül toohello felhasználói, különösen akkor hello visszatérési érték a `.ToString()`, vagy hello üzenet vagy Veremkivonat tulajdonságok hello értékeit. Biztonságosan jelentkezzen ezt az információt, és a több ártalmatlan üzenet toohello felhasználó megjelenítése</li></ul>|

## <a id="default"></a>Alapértelmezett hibakezelési lap végrehajtása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Az ASP.NET lapok beállításainak szerkesztése párbeszédpanel](https://technet.microsoft.com/library/dd569096(WS.10).aspx) |
| **Lépések** | <p>Az ASP.NET-alkalmazások sikertelen lesz, és HTTP/1.x 500 belső kiszolgáló hibát okoz, vagy egy kapcsolószolgáltatás-konfigurációja (például kérelmek szűrése) esetén a lap nem jelenik meg, amikor egy hibaüzenetet fog készülni. Rendszergazdák kiválaszthatják, függetlenül attól, hello alkalmazás egy barátságos üzenet toohello ügyfél, a hibával kapcsolatos részletes üzenet toohello ügyfél vagy a csak a hibával kapcsolatos részletes üzenet toolocalhost megjelenjen-e. Hello <customErrors> hello web.config-címke szolgáltatásnak három módja van:</p><ul><li>**A:** határozza meg, hogy engedélyezve vannak-e az egyéni hibák. Ha nem defaultRedirect attribútum meg van adva, a felhasználók látni egy általános hiba. hello az egyéni hibák toohello távoli ügyfelek és a helyi állomás toohello láthatók.</li><li>**OFF:** határozza meg, hogy van-e tiltva az egyéni hibák. hello részletes ASP.NET hibák jelennek meg toohello távoli ügyfelek és a helyi állomás toohello</li><li>**RemoteOnly:** Megadja, hogy megjelennek-e az egyéni hibák csak toohello távoli ügyfelek és, hogy megjelennek-e az ASP.NET-hibákat toohello helyi állomás. Ez az alapértelmezett érték hello</li></ul><p>Nyissa meg hello `web.config` hello alkalmazás vagy hely fájlt, és ügyeljen arra, hogy hello címke vagy `<customErrors mode="RemoteOnly" />` vagy `<customErrors mode="On" />` definiálva.</p>|

## <a id="deployment"></a>Az üzembe helyezési módszer tooRetail beállítása az IIS-ben

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [központi telepítés elem (ASP.NET beállítási séma)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx) |
| **Lépések** | <p>Hello `<deployment retail>` kapcsoló éles IIS-kiszolgálókkal való használatra készült. A kapcsoló használt toohelp alkalmazások hello legjobb lehetséges teljesítménnyel futnak, és legalább lehetséges biztonsági információk letiltásával szivárgást hello alkalmazás képes toogenerate nyomkövetés tiltása hello képességét toodisplay lapon a hiba részletei üzenetek tooend felhasználók és letiltását hello hibakeresési kapcsoló.</p><p>Gyakran, a kapcsolók és a kapcsolókról developer-arra irányul, például a sikertelen kérelmek nyomkövetésére vonatkozó és a hibakeresést, aktív fejlesztése során. Javasoljuk, hogy bármely üzemi kiszolgálón az üzembe helyezési módszer hello tooretail állítható be. Nyissa meg a hello machine.config fájl, és győződjön meg arról, hogy `<deployment retail="true" />` tootrue értéke.</p>|

## <a id="fail"></a>Kivételek biztonságosan kell-e sikertelen

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Biztonságos helyen sikertelen](https://www.owasp.org/index.php/Fail_securely) |
| **Lépések** | Alkalmazás biztonságosan sikertelen lesz. Bármelyik módszert, amely visszaadja az egy logikai érték, mely bizonyos döntés, alapján gondosan létrehozott kivételblokk kell rendelkeznie. Nincsenek nagy mennyiségű logikai hibák miatt toowhich biztonsági problémákat kötés a, ha hello kivételblokk carelessly ír.|

### <a name="example"></a>Példa
```C#
        public static bool ValidateDomain(string pathToValidate, Uri currentUrl)
        {
            try
            {
                if (!string.IsNullOrWhiteSpace(pathToValidate))
                {
                    var domain = RetrieveDomain(currentUrl);
                    var replyPath = new Uri(pathToValidate);
                    var replyDomain = RetrieveDomain(replyPath);

                    if (string.Compare(domain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                    {
                        //// Adding additional check tooenable CMS urls if they are not hosted on same domain.
                        if (!string.IsNullOrWhiteSpace(Utilities.CmsBase))
                        {
                            var cmsDomain = RetrieveDomain(new Uri(Utilities.Base.Trim()));
                            if (string.Compare(cmDomain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                            {
                                return false;
                            }
                            else
                            {
                                return true;
                            }
                        }

                        return false;
                    }
                }

                return true;
            }
            catch (UriFormatException ex)
            {
                LogHelper.LogException("Utilities:ValidateDomain", ex);
                return true;
            }
        }
```
hello fent metódus mindig ad vissza IGAZ, ha kivétel történik. Ha hello végfelhasználói biztosít egy nem megfelelően formázott URL-címet, amely hello böngésző tekintetben, de hello `Uri()` konstruktor nem, a rendszer kivételt jelez, és hello áldozata toohello érvényes, de nem megfelelően formázott URL-címet kell tenni. 
