---
title: "-Microsoft fenyegetések modellezése eszköz - kezelés Azure aaaConfiguration |} Microsoft Docs"
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
ms.openlocfilehash: 77aa4352fa61e928a1b7a4ff1d488a55d3d9b970
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-configuration-management--mitigations"></a>Biztonsági keret: Konfigurációkezelés |} Megoldást 
| A termék vagy szolgáltatás | Cikk |
| --------------- | ------- |
| **Webalkalmazás** | <ul><li>[Tartalom biztonsági házirend (CSP) megvalósítása, és tiltsa le a beágyazott javascript](#csp-js)</li><li>[Böngésző lehetővé szűrő engedélyezése](#xss-filter)</li><li>[ASP.NET-alkalmazások le kell tiltania előzetes toodeployment nyomkövetéséhez és](#trace-deploy)</li><li>[Hozzáférés külső JavaScript-kódok csak megbízható forrásból származó](#js-trusted)</li><li>[Győződjön meg arról, hogy a hitelesített ASP.NET-lapok tartalmaznia felhasználói felület Redressing vagy kattintással emelési védelmekkel](#ui-defenses)</li><li>[Győződjön meg arról, hogy csak megbízható források engedélyezve legyenek-e, ha az ASP.NET-webalkalmazások CORS engedélyezett](#cors-aspnet)</li><li>[ASP.NET-lapok ValidateRequest attribútum engedélyezése](#validate-aspnet)</li><li>[JavaScript-tárak legújabb verzióját helyileg üzemeltetett használata](#local-js)</li><li>[Tiltsa le az automatikus MIME-elemzése](#mime-sniff)</li><li>[Távolítsa el a Windows Azure-webhelyek tooavoid ujjlenyomat általános jogú kiszolgálói-fejlécei](#standard-finger)</li></ul> |
| **Adatbázis** | <ul><li>[A Windows tűzfal konfigurálása a hozzáféréshez](#firewall-db)</li></ul> |
| **Webes API** | <ul><li>[Győződjön meg arról, hogy csak megbízható források engedélyezve legyenek-e, ha az ASP.NET Web API CORS engedélyezett](#cors-api)</li><li>[Titkosítani bizalmas adatokat tartalmazó Web API konfigurációs fájljainak szakaszait](#config-sensitive)</li></ul> |
| **IoT-eszközök** | <ul><li>[Győződjön meg arról, hogy az összes felügyeleti felületeiről biztosított erős hitelesítő adatokkal](#admin-strong)</li><li>[Győződjön meg arról, hogy ismeretlen kód nem hajtható végre olyan eszközök](#unknown-exe)</li><li>[Az operációs rendszer és az AppLocker-bit az IoT-eszközök további partíciók titkosítása](#partition-iot)</li><li>[Győződjön meg arról, hogy csak hello minimális szolgáltatások vagy szolgáltatások engedélyezve vannak-e az eszközökön](#min-enable)</li></ul> |
| **Az IoT-mező átjáró** | <ul><li>[Az operációs rendszer és az AppLocker-bit IoT mező átjáró további partíciók titkosítása](#field-bit-locker)</li><li>[Győződjön meg arról, hogy hello mező átjáró hello alapértelmezett bejelentkezési hitelesítő adatok megváltoznak a telepítés során](#default-change)</li></ul> |
| **Az IoT átjáró** | <ul><li>[Győződjön meg arról, hogy hello Felhőátjáróhoz valósítja meg a folyamat tookeep hello csatlakoztatott eszközök belső vezérlőprogram toodate mentése](#cloud-firmware)</li></ul> |
| **Gép megbízhatósági kapcsolat határán** | <ul><li>[Győződjön meg arról, hogy eszközök szervezeti házirendek szerint beállított végpont biztonsági vezérlő](#controls-policies)</li></ul> |
| **Azure Storage** | <ul><li>[Az Azure storage elérési kulcsok biztonságos kezelése](#secure-keys)</li><li>[Győződjön meg arról, hogy csak megbízható források engedélyezve legyenek-e, ha a CORS engedélyezve van az Azure storage](#cors-storage)</li></ul> |
| **WCF** | <ul><li>[A WCF szolgáltatás szabályozásával engedélyezése](#throttling)</li><li>[WCF-adatokhoz való illetéktelen hozzáférés metaadatok keresztül](#info-metadata)</li></ul> | 

## <a id="csp-js"></a>Tartalom biztonsági házirend (CSP) megvalósítása, és tiltsa le a beágyazott javascript

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Egy bevezető tooContent biztonsági házirend](http://www.html5rocks.com/en/tutorials/security/content-security-policy/), [tartalom biztonsági szabályzatainak ismertetése](http://content-security-policy.com/), [biztonsági funkciók](https://developer.microsoft.com/microsoft-edge/platform/documentation/dev-guide/security/), [bemutatása toocontent biztonsági házirend](https://docs.webplatform.org/wiki/tutorials/content-security-policy), [Használható CSP?](http://caniuse.com/#feat=contentsecuritypolicy) |
| **Lépések** | <p>Tartalom biztonsági házirend (CSP) egy egy védelmi jellegű biztonsági mechanizmus, egy szabványos, W3C, amely lehetővé teszi, hogy a webes alkalmazás tulajdonosok toohave vezérlő a helyükön lévő beágyazott hello a tartalomhoz. Kriptográfiai Szolgáltató egy HTTP-válaszfejléc hello webkiszolgálón meg van adva, és kikényszeríti a hello ügyféloldali böngészőkben. Egy engedélyezési lista-alapú házirend - webhely deklarálhatnak mely aktív tartalom a megbízható tartományok készlete, például JavaScript tölthetők be.</p><p>Kriptográfiai szolgáltató a következő biztonsági szempontból előnyökkel járhat hello:</p><ul><li>**Védelmet biztosít a lehetővé:** Ha egy lap sebezhető tooXSS, egy támadó kihasználhatja azt 2 módon:<ul><li>Szúrjon `<script>malicious code</script>`. A biztonsági rés nem fog működni a korlátozás-1-miatt tooCSP tartozó alapja</li><li>Szúrjon `<script src=”http://attacker.com/maliciousCode.js”/>`. A biztonsági rés nem fog működni, mert hello általa tartomány nem lesz a felhőbeli Szolgáltató engedélyezett tartományok</li></ul></li><li>**Adatok exfiltration szabályozhatják:** bármilyen weblap rosszindulatú tartalmat próbál tooconnect tooan külső webhely és eltulajdonít adatokat, ha hello kapcsolat megszakította CSP-hez. Ennek az az oka hello céltartomány nem lesz a felhőbeli Szolgáltató engedélyezési lista</li><li>**Kattintson az-emelési elleni védelmet:** kattintson-emelési egy támadási módszer használatával, amely egy ellenfél is keret eredeti webhely és a felhasználók tooclick kényszerítése a felhasználói felületi elemei. Jelenleg kattintson-emelési elleni védelmet egy fejléc-X-keret-válaszbeállítások beállításával valósul meg. Nem minden böngésző, figyelembe vegyék ezt a fejlécet, és előre CSP fog a szokásos módon toodefend kattintson-emelési ellen</li><li>**Valós idejű támadás reporting:** injektálási támadást egy CSP-kompatibilis webhelyen van, ha böngészők automatikusan vált egy értesítési tooan végpont hello webkiszolgáló konfigurálva. Ezzel a módszerrel CSP valós idejű figyelmeztető rendszer funkcionál.</li></ul> |

### <a name="example"></a>Példa
Példa házirend: 
```C#
Content-Security-Policy: default-src 'self'; script-src 'self' www.google-analytics.com 
```
Ez a házirend lehetővé teszi, hogy a parancsfájlok tooload csak a hello webes alkalmazás kiszolgáló és a google analytics kiszolgálón. Bármely más helyről betöltött parancsprogramok a program elutasítja. Ha CSP engedélyezve van a webhelyen, hello következő funkciók válnak automatikusan letiltott toomitigate lehetővé támadásokat. 

### <a name="example"></a>Példa
Beágyazott parancsfájlokra nem fogja végrehajtani. Az alábbiakban példákat beágyazott parancsfájlokra 
```javascript
<script> some Javascript code </script>
Event handling attributes of HTML tags (e.g., <button onclick=”function(){}”>
javascript:alert(1);
```

### <a name="example"></a>Példa
Karakterláncok kódú nem lesz kiértékelve. 
```javascript
Example: var str="alert(1)"; eval(str);
```

## <a id="xss-filter"></a>Böngésző lehetővé szűrő engedélyezése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Lehetővé védelmi szűrő](https://www.owasp.org/index.php/List_of_useful_HTTP_headers#X-XSS-Protection) |
| **Lépések** | <p>X-lehetővé-védelmet válasz fejléce konfigurációs vezérlők hello böngésző helyközi parancsfájl szűrő. A válasz fejléce a következő értékeket veheti fel:</p><ul><li>`0:`Ezzel a lépéssel letiltja hello szűrő</li><li>`1: Filter enabled`Ha egy többhelyes parancsfájl-kezelési támogatás észlel, az order toostop hello támadások, hello böngésző fog sanitize hello lap</li><li>`1: mode=block : Filter enabled`. Helyett sanitize hello lapon, a lehetővé támadás észlelése esetén, hello böngésző megakadályozza, hogy megjelenítési hello lap</li><li>`1: report=http://[YOURDOMAIN]/your_report_URI : Filter enabled`. hello böngésző fog sanitize hello oldal és a jelentés hello megsértése.</li></ul><p>Ez az okhoz CSP megsértése jelentések toosend részletek tooa az Ön által választott URI króm függvényt. hello utolsó 2 beállítások minősülnek biztonságos értékeket.</p>|

## <a id="trace-deploy"></a>ASP.NET-alkalmazások le kell tiltania előzetes toodeployment nyomkövetéséhez és

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [ASP.NET Debugging Overview](http://msdn2.microsoft.com/library/ms227556.aspx), [ASP.NET követés áttekintése](http://msdn2.microsoft.com/library/bb386420.aspx), [How to: enable Tracing for ASP.NET-alkalmazás engedélyezése](http://msdn2.microsoft.com/library/0x5wc973.aspx), [hogyan: az ASP.NET-alkalmazások hibakeresés engedélyezése](http://msdn2.microsoft.com/library/e8z01xdh(VS.80).aspx) |
| **Lépések** | Hello lap nyomkövetése engedélyezve van, amikor is a kért minden böngésző jut hozzá a belső kiszolgáló állapota és a munkafolyamat adatait tartalmazó hello nyomkövetési adatokat. Ez az információ biztonsági bizalmas lehet. Hello lap engedélyezve van a hibakeresés során hiba történt az hello kiszolgáló akár egy teljes verem nyomkövetési adatok toohello böngésző jelenik meg. Adatok biztonsági szempontból kényes információt hello server munkafolyamat tehetik közzé. |

## <a id="js-trusted"></a>Hozzáférés külső JavaScript-kódok csak megbízható forrásból származó

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | külső JavaScript-kódok csak megbízható forrásból kell lehet hivatkozni. hello hivatkozás végpontok mindig SSL kell lennie. |

## <a id="ui-defenses"></a>Győződjön meg arról, hogy a hitelesített ASP.NET-lapok tartalmaznia felhasználói felület Redressing vagy kattintással emelési védelmekkel

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Védelmi Cheat lap kattintson-emelési OWASP](https://www.owasp.org/index.php/Clickjacking_Defense_Cheat_Sheet), [IE Internals - elleni kattintson-emelési az X-keret-beállítások](https://blogs.msdn.microsoft.com/ieinternals/2010/03/30/combating-click-jacking-with-x-frame-options/) |
| **Lépések** | <p>Kattintson-emelési, más néven a "felhasználói felület jogorvoslati támadás", akkor, ha egy támadó használ több átlátszó vagy nem átlátszó rétegek tootrick egy felhasználó át egy gombra való kattintást vagy hivatkozás az egy másik lapon, ha azok volt szándékos volt tooclick hello legfelső szintű lapon.</p><p>Ez a réteges létrehozásával iframe, amelyet az áldozat hello lapon be egy rosszindulatú lapon érhető el. Ebből kifolyólag hello támadó "térít" kattint azt jelentette, hogy a lap és útválasztási őket tooanother lap, nagy valószínűséggel tulajdonosa egy másik alkalmazás, tartomány, vagy mindkettőt. tooprevent kattintson-emelési támadások set hello megfelelő X-keret-beállítások HTTP-válaszfejlécek, melyek arra utasítják hello böngésző toonot engedélyezése keretezés más tartományokból</p>|

### <a name="example"></a>Példa
hello X-keret-beállítások fejlécet az IIS web.config keresztül állítható be. Web.config kódrészletet, amely soha nem keretezhető helyek: 
```C#
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="DENY"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

### <a name="example"></a>Példa
Lapok csak keretezhető által helyek Web.config kódját a hello azonos tartományban: 
```C#
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="SAMEORIGIN"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

## <a id="cors-aspnet"></a>Győződjön meg arról, hogy csak megbízható források engedélyezve legyenek-e, ha az ASP.NET-webalkalmazások CORS engedélyezett

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Web Forms keretrendszerre, MVC5 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Böngésző biztonsági megakadályozza, hogy egy weblap AJAX-kérelmek tooanother tartomány. Ez a korlátozás hello azonos eredetű házirend nevezik, és megakadályozza, hogy egy rosszindulatú hely bizalmas adatok olvasása a másik helyről. Azonban néha előfordulhat, hogy szükséges tooexpose API-k biztonságosan, amely más webhelyek is felhasználhatnak. Közötti Origin Resource Sharing (CORS) van olyan W3C szabvány, amely lehetővé teszi, hogy a kiszolgáló toorelax hello azonos eredetű házirend. CORS használatával egy kiszolgáló kifejezetten engedélyezhet bizonyos egyes eltérő eredetű kérések elutasítása mások közben.</p><p>A CORS biztonságosabb és rugalmasabb, mint például a JSONP korábbi technikák. A a fő CORS engedélyezése az eszköz tooadding néhány HTTP-válaszfejlécek (hozzáférés - vezérlési-*) toohello webes alkalmazás, és ez több módon is végrehajtható.</p>|

### <a name="example"></a>Példa
Ha hozzáférés tooWeb.config érhető el, majd CORS segítségével is hozzáadhat hello a következő kódot: 
```XML
<system.webServer>
    <httpProtocol>
      <customHeaders>
        <clear />
        <add name="Access-Control-Allow-Origin" value="http://example.com" />
      </customHeaders>
    </httpProtocol>
```

### <a name="example"></a>Példa
Ha access tooweb.config nem érhető el, majd CORS konfigurálható csharp nyelvű kód a következő hello hozzáadásával: 
```C#
HttpContext.Response.AppendHeader("Access-Control-Allow-Origin", "http://example.com")
```

Vegye figyelembe, hogy a rendszer, amely tartalmazza a "Hozzáférés-vezérlési-engedélyezése – Origin" attribútum lista hello kritikus tooensure állítsa tooa végesnek és megbízható készletét tartalmazza. Ezen helytelenül eljárva sikertelen tooconfigure (mint pl. hello érték "*") lehetővé teszi rosszindulatú webhelyeket tootrigger közötti eredetű kérések toohello webalkalmazás > nélkül korlátozások, ezáltal hello alkalmazás sebezhető tooCSRF támadások. 

## <a id="validate-aspnet"></a>ASP.NET-lapok ValidateRequest attribútum engedélyezése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Web Forms keretrendszerre, MVC5 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Kérelem érvényesítése - parancsfájl támadások megelőzése](http://www.asp.net/whitepapers/request-validation) |
| **Lépések** | <p>Kérelem érvényesítése, a szolgáltatás az ASP.NET 1.1-es verzió óta megakadályozza, hogy a hello kiszolgáló elfogadása tartalmazó nem kódolt HTML tartalom. Ez a szolgáltatás úgy van kialakítva, toohelp amellyel ügyfél parancsfájlkód vagy HTML lehet a tudtukon elküldött tooa kiszolgáló tárolja, és ezután tooother felhasználók parancsfájl-injektálás a támadások megelőzése. Továbbra is javasoljuk, hogy az összes bemeneti adatok érvényesítése és HTML kódolására, amikor szükséges.</p><p>Kérelem érvényesítése az összes bemeneti adatok tooa lista potenciálisan veszélyes értékek összehasonlításával történik. Egyezés akkor fordul elő, ha az ASP.NET kivált egy `HttpRequestValidationException`. Kérelem érvényesítése funkció alapértelmezés szerint engedélyezve van.</p>|

### <a name="example"></a>Példa
Azonban ez a szolgáltatás letiltható lap szinten: 
```XML
<%@ Page validateRequest="false" %> 
```
vagy alkalmazás szinten 
```XML
<configuration>
   <system.web>
      <pages validateRequest="false" />
   </system.web>
</configuration>
```
Vegye figyelembe, hogy érvényesítés kérése a szolgáltatás nem támogatott, és nem MVC6 folyamat részeként. 

## <a id="local-js"></a>JavaScript-tárak legújabb verzióját helyileg üzemeltetett használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Standard JavaScript-könyvtárak JQuery kell használnia, mint segítségével a fejlesztők jóváhagyott közös JavaScript-tárak, amelyek nem tartalmaznak ismert biztonsági hiányosságokat verzióját. Bevált gyakorlat toouse hello a legújabb verzióra hello szalagtárak azért, mert a biztonsági javításokat az ismert biztonsági rések a régebbi verzióját a tartalmaznak.</p><p>Hello legújabb kiadására toocompatibility okok miatt nem használható, ha hello minimális verziója alatt kell használni.</p><p>Elfogadható minimális verziója:</p><ul><li>**JQuery**<ul><li>1.7.1 JQuery</li><li>JQueryUI 1.10.0</li><li>JQuery 1.9 ellenőrzése</li><li>JQuery Mobile 1.0.1-es</li><li>JQuery ciklus 2.99</li><li>JQuery DataTables 1.9.0</li></ul></li><li>**AJAX-vezérlő eszközkészlet**<ul><li>AJAX-vezérlő eszközkészlet 40412</li></ul></li><li>**Az ASP.NET Web Forms keretrendszerre és Ajax**<ul><li>Az ASP.NET Web Forms keretrendszerre és Ajax 4</li><li>Az ASP.NET Ajax 3.5</li></ul></li><li>**ASP.NET MVC**<ul><li>ASP.NET MVC 3.0</li></ul></li></ul><p>Soha nem tölthető be egyetlen JavaScript kódtár külső helyekről, például a nyilvános tartalomtovábbító</p>|

## <a id="mime-sniff"></a>Tiltsa le az automatikus MIME-elemzése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [IE8 biztonsági rész V: átfogó védelmének](http://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx), [MIME-típus](http://en.wikipedia.org/wiki/Mime_type) |
| **Lépések** | X-tartalom-típust-beállítások hello fejléc egy HTTP-fejlécet, amely lehetővé teszi a fejlesztők számára, hogy a tartalmat nem kell MIME-felszippantásra toospecify. Ezt a fejlécet tervezett toomitigate MIME-elemzése támadások. Az egyes lapok tartalmazhatnak felhasználói ellenőrizhető tartalmat, használnia kell a HTTP-fejléc X hello-tartalom-típust-beállítások: nosniff. tooenable hello szükséges fejléc globálisan hello alkalmazás összes lapja esetében, akkor teheti hello következő|

### <a name="example"></a>Példa
Hello fejléc hozzáadása hello web.config fájlban, ha hello alkalmazás által az Internet Information Services (IIS) 7 és újabb verziók esetében. 
```XML
<system.webServer>
<httpProtocol>
<customHeaders>
<add name="X-Content-Type-Options" value="nosniff"/>
</customHeaders>
</httpProtocol>
</system.webServer>
```

### <a name="example"></a>Példa
Hello keresztül hello fejléc hozzáadása globális alkalmazás\_BeginRequest 
```C#
void Application_BeginRequest(object sender, EventArgs e)
{
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
}
```

### <a name="example"></a>Példa
Egyéni HTTP-modulja megvalósítása 
```C#
public class XContentTypeOptionsModule : IHttpModule
{
#region IHttpModule Members
public void Dispose()
{
}
public void Init(HttpApplication context)
{
context.PreSendRequestHeaders += newEventHandler(context_PreSendRequestHeaders);
}
#endregion
void context_PreSendRequestHeaders(object sender, EventArgs e)
{
HttpApplication application = sender as HttpApplication;
if (application == null)
  return;
if (application.Response.Headers["X-Content-Type-Options "] != null)
  return;
application.Response.Headers.Add("X-Content-Type-Options ", "nosniff");
}
}
```

### <a name="example"></a>Példa
Hello szükséges fejléc csak az adott lapok tooindividual válaszok hozzáadással engedélyezheti: 

```C#
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
```

## <a id="standard-finger"></a>Távolítsa el a Windows Azure-webhelyek tooavoid ujjlenyomat általános jogú kiszolgálói-fejlécei

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | EnvironmentType – Azure |
| **Hivatkozások**              | [A Windows Azure Web Sites általános jogú kiszolgálói fejlécek eltávolítása](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/) |
| **Lépések** | -Kiszolgáló, X-regisztráló-, például fejlécek X-AspNet-verzió hello kiszolgáló és az alapjául szolgáló technológiák hello információk felfedése. Ajánlott toosuppress ezek a fejlécek, ami megakadályozza az ujjlenyomat hello alkalmazás |

## <a id="firewall-db"></a>A Windows tűzfal konfigurálása a hozzáféréshez

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Az SQL Azure, a helyi üzemeltetésű |
| **Attribútumok**              | N/A, SQL verzió - 12-es verzió |
| **Hivatkozások**              | [Hogyan tooconfigure egy Azure SQL adatbázis-tűzfal](https://azure.microsoft.com/documentation/articles/sql-database-firewall-configure/), [beállítani a Windows tűzfalat a hozzáféréshez](https://msdn.microsoft.com/library/ms175043) |
| **Lépések** | Tűzfalak működésével toocomputer erőforrások jogosulatlan hozzáférés megelőzése érdekében. tooaccess egy példányát az SQL Server adatbázismotor hello tűzfalon keresztül, konfigurálnia kell hello tűzfal hello futtató SQL Server tooallow hozzáférés |

## <a id="cors-api"></a>Győződjön meg arról, hogy csak megbízható források engedélyezve legyenek-e, ha az ASP.NET Web API CORS engedélyezett

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | MVC 5 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Az ASP.NET Web API 2 eltérő eredetű kérések engedélyezése](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api), [ASP.NET Web API - ASP.NET Web API 2 a CORS-támogatás](https://msdn.microsoft.com/magazine/dn532203.aspx) |
| **Lépések** | <p>Böngésző biztonsági megakadályozza, hogy egy weblap AJAX-kérelmek tooanother tartomány. Ez a korlátozás hello azonos eredetű házirend nevezik, és megakadályozza, hogy egy rosszindulatú hely bizalmas adatok olvasása a másik helyről. Azonban néha előfordulhat, hogy szükséges tooexpose API-k biztonságosan, amely más webhelyek is felhasználhatnak. Közötti Origin Resource Sharing (CORS) van olyan W3C szabvány, amely lehetővé teszi, hogy a kiszolgáló toorelax hello azonos eredetű házirend.</p><p>CORS használatával egy kiszolgáló kifejezetten engedélyezhet bizonyos egyes eltérő eredetű kérések elutasítása mások közben. A CORS biztonságosabb és rugalmasabb, mint például a JSONP korábbi technikák.</p>|

### <a name="example"></a>Példa
Hello App_Start/WebApiConfig.cs adja hozzá a következő kód toohello WebApiConfig.Register metódus hello 
```C#
using System.Web.Http;
namespace WebService
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // New code
            config.EnableCors();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
        }
    }
}
```

### <a name="example"></a>Példa
EnableCors attribútum lehet egy tartományvezérlőre alkalmazott tooaction módszerek az alábbiak szerint: 

```C#
public class ResourcesController : ApiController
{
  [EnableCors("http://localhost:55912", // Origin
              null,                     // Request headers
              "GET",                    // HTTP methods
              "bar",                    // Response headers
              SupportsCredentials=true  // Allow credentials
  )]
  public HttpResponseMessage Get(int id)
  {
    var resp = Request.CreateResponse(HttpStatusCode.NoContent);
    resp.Headers.Add("bar", "a bar value");
    return resp;
  }
  [EnableCors("http://localhost:55912",       // Origin
              "Accept, Origin, Content-Type", // Request headers
              "PUT",                          // HTTP methods
              PreflightMaxAge=600             // Preflight cache duration
  )]
  public HttpResponseMessage Put(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  [EnableCors("http://localhost:55912",       // Origin
              "Accept, Origin, Content-Type", // Request headers
              "POST",                         // HTTP methods
              PreflightMaxAge=600             // Preflight cache duration
  )]
  public HttpResponseMessage Post(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
}
```

Vegye figyelembe, hogy a rendszer kritikus fontosságú, hogy a lista tartalmazza a EnableCors attribútum hello tooensure állítsa tooa végesnek és megbízható készletét tartalmazza. Ezen helytelenül eljárva sikertelen tooconfigure (mint pl. hello érték "*") lehetővé teszi a rosszindulatú webhelyeket tootrigger közötti eredetű kérések toohello API, korlátozások nélküli > ezáltal hello API sebezhető tooCSRF támadások. EnableCors vezérlő szintjén látható is el. 

### <a name="example"></a>Példa
egy adott osztály adott metódusra CORS toodisable hello DisableCors attribútum is használható a lent látható módon: 
```C#
[EnableCors("http://example.com", "Accept, Origin, Content-Type", "POST")]
public class ResourcesController : ApiController
{
  public HttpResponseMessage Put(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  public HttpResponseMessage Post(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  // CORS not allowed because of hello [DisableCors] attribute
  [DisableCors]
  public HttpResponseMessage Delete(int id)
  {
    return Request.CreateResponse(HttpStatusCode.NoContent);
  }
}
```

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | MVC 6 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Az ASP.NET Core 1.0 (CORS) eltérő eredetű kérések engedélyezése](https://docs.asp.net/en/latest/security/cors.html) |
| **Lépések** | <p>Az ASP.NET Core 1.0-s verziójában a CORS engedélyezhető köztes akár segítségével MVC. MVC tooenable CORS hello azonos CORS szolgáltatásokat használja, de a CORS köztes nincs hello használatakor.</p>|

**Megközelítés-1** CORS engedélyezése a köztes: hello teljes alkalmazás CORS tooenable hozzáadása hello CORS köztes toohello kérelemfeldolgozási sorban hello UseCors bővítmény metódussal. Eltérő eredetű házirend felvételekor hello CORS köztes hello CorsPolicyBuilder osztály használatával adható meg. Nincsenek két módon toodo ezt:

### <a name="example"></a>Példa
hello első az toocall UseCors rendelkező egy lambda kifejezésben. hello lambda egy CorsPolicyBuilder objektum hajtja végre: 
```C#
public void Configure(IApplicationBuilder app)
{
    app.UseCors(builder =>
        builder.WithOrigins("http://example.com")
        .WithMethods("GET", "POST", "HEAD")
        .WithHeaders("accept", "content-type", "origin", "x-custom-header"));
}
```

### <a name="example"></a>Példa
hello második toodefine egy vagy több nevű CORS házirendeket, és jelölje ki hello házirend nevű futási időben. 
```C#
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors(options =>
    {
        options.AddPolicy("AllowSpecificOrigin",
            builder => builder.WithOrigins("http://example.com"));
    });
}
public void Configure(IApplicationBuilder app)
{
    app.UseCors("AllowSpecificOrigin");
    app.Run(async (context) =>
    {
        await context.Response.WriteAsync("Hello World!");
    });
}
```

**Megközelítés-2** CORS engedélyezése az mvc-ben: a fejlesztők azt is megteheti a MVC tooapply műveletenként, vezérlőnként és globálisan az összes adott CORS.

### <a name="example"></a>Példa
Műveletenként: a CORS házirendet egy bizonyos művelet toospecify hello [EnableCors] attribútum toohello művelet hozzáadása. Adja meg a hello házirend nevét. 
```C#
public class HomeController : Controller
{
    [EnableCors("AllowSpecificOrigin")] 
    public IActionResult Index()
    {
        return View();
    }
```

### <a name="example"></a>Példa
Vezérlőnként: 
```C#
[EnableCors("AllowSpecificOrigin")]
public class HomeController : Controller
{
```

### <a name="example"></a>Példa
Globális: 
```C#
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.Configure<MvcOptions>(options =>
    {
        options.Filters.Add(new CorsAuthorizationFilterFactory("AllowSpecificOrigin"));
    });
}
```
Vegye figyelembe, hogy a rendszer kritikus fontosságú, hogy a lista tartalmazza a EnableCors attribútum hello tooensure állítsa tooa végesnek és megbízható készletét tartalmazza. Ezen helytelenül eljárva sikertelen tooconfigure (mint pl. hello érték "*") lehetővé teszi a rosszindulatú webhelyeket tootrigger közötti eredetű kérések toohello API, korlátozások nélküli > ezáltal hello API sebezhető tooCSRF támadások. 

### <a name="example"></a>Példa
a CORS toodisable egy tartományvezérlő vagy a művelet, hello [DisableCors] attribútum használata. 
```C#
[DisableCors]
    public IActionResult About()
    {
        return View();
    }
```

## <a id="config-sensitive"></a>Titkosítani bizalmas adatokat tartalmazó Web API konfigurációs fájljainak szakaszait

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Útmutató: Az ASP.NET 2.0 használatával DPAPI konfigurációs szakasz titkosítása](https://msdn.microsoft.com/library/ff647398.aspx), [adja meg egy védett Konfigurációszolgáltatót](https://msdn.microsoft.com/library/68ze1hb2.aspx), [Azure Key Vault használatával tooprotect alkalmazás titkos kulcsok](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Lépések** | Például hello Web.config konfigurációs fájl, appsettings.json van gyakran használt toohold bizalmas adatokat, beleértve a felhasználónevek, jelszavak, adatbázis-kapcsolati karakterláncok és titkosítási kulcsokat. Ez az információ nem védi, az alkalmazás akkor sebezhető tooattackers vagy rosszindulatú felhasználók megszerezni a bizalmas adatokat, például a fiók felhasználói nevét és a jelszavak, a adatbázis nevét és a kiszolgálók nevei. Hello központi telepítési típus (azure vagy a helyszínen) alapuló, titkosítása hello bizalmas DPAPI-t vagy szolgáltatásokat, mint az Azure Key Vault segítségével konfigurációs fájljainak szakaszait. |

## <a id="admin-strong"></a>Győződjön meg arról, hogy az összes felügyeleti felületeiről biztosított erős hitelesítő adatokkal

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | IoT-eszközök | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Minden felügyeleti felületek hello eszközökön, vagy mező átjáró tesz elérhetővé titkosítani kell erős hitelesítő adataival. Is bármely egyéb kitett felületek, például Wi-Fi, SSH, fájlmegosztások, FTP titkosítani kell erős hitelesítő adatokkal. Alapértelmezett érték a gyenge jelszavakat nem lehet használni. |

## <a id="unknown-exe"></a>Győződjön meg arról, hogy ismeretlen kód nem hajtható végre olyan eszközök

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | IoT-eszközök | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [A biztonságos rendszerindítás és az AppLocker-bit Eszköztitkosítás a Windows 10 IoT Core engedélyezése](https://developer.microsoft.com/windows/iot/win10/sb_bl) |
| **Lépések** | UEFI biztonságos rendszerindítás korlátozza hello rendszer tooonly a megadott hatóság aláírásával bináris fájl futtatását teszi lehetővé. Ez a szolgáltatás megakadályozó ismeretlen kód hello platformon végrehajtott és potenciálisan gyengítése hello biztonságot azt. Engedélyezze a UEFI biztonságos rendszerindítás, és korlátozhatja hello tartozó kód aláírása megbízható tanúsítványszolgáltatók listáját. Hello megbízható hitelesítésszolgáltatók egyikével hello eszközön telepített összes kód aláírása. |

## <a id="partition-iot"></a>Az operációs rendszer és az AppLocker-bit az IoT-eszközök további partíciók titkosítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | IoT-eszközök | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Windows 10 IoT Core bit-tároló Eszköztitkosítás, amelynek hello platformon, beleértve hello szükséges preOS protokoll, amely hello szükséges mérések végez UEFI TPM hello jelenléte erős függőség van egyszerűsített verziója valósítja meg. E preOS mérések győződjön meg arról, hogy az operációs rendszer újabb tartalmaz hogyan hello OS elindult végleges bejegyzést hello. Az operációs rendszer partíciók bit-tároló és a további partíciókat is használ, ha azokat a bizalmas adatokat tárolhatnak titkosításához. |

## <a id="min-enable"></a>Győződjön meg arról, hogy csak hello minimális szolgáltatások vagy szolgáltatások engedélyezve vannak-e az eszközökön

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | IoT-eszközök | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Ne engedélyezze és ne kapcsolja ki a bármely szolgáltatások vagy az operációs rendszer, amelyre nincs szükség hello megoldás hello működéséhez hello szolgáltatást. A pl. Ha hello eszköz nem követeli meg a felhasználói felület toobe telepítve, telepítse a Windows IoT távfelügyeleti módban. |

## <a id="field-bit-locker"></a>Az operációs rendszer és az AppLocker-bit IoT mező átjáró további partíciók titkosítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT-mező átjáró | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Windows 10 IoT Core bit-tároló Eszköztitkosítás, amelynek hello platformon, beleértve hello szükséges preOS protokoll, amely hello szükséges mérések végez UEFI TPM hello jelenléte erős függőség van egyszerűsített verziója valósítja meg. E preOS mérések győződjön meg arról, hogy az operációs rendszer újabb tartalmaz hogyan hello OS elindult végleges bejegyzést hello. Az operációs rendszer partíciók bit-tároló és a további partíciókat is használ, ha azokat a bizalmas adatokat tárolhatnak titkosításához. |

## <a id="default-change"></a>Győződjön meg arról, hogy hello mező átjáró hello alapértelmezett bejelentkezési hitelesítő adatok megváltoznak a telepítés során

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT-mező átjáró | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Győződjön meg arról, hogy hello mező átjáró hello alapértelmezett bejelentkezési hitelesítő adatok megváltoznak a telepítés során |

## <a id="cloud-firmware"></a>Győződjön meg arról, hogy hello Felhőátjáróhoz valósítja meg a folyamat tookeep hello csatlakoztatott eszközök belső vezérlőprogram toodate mentése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT átjáró | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Átjáró choice - Azure IoT Hub |
| **Hivatkozások**              | [Az IoT Hub eszköz kezelése-áttekintés](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-overview/), [hogyan tooupdate eszköz belső vezérlőprogram](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-device-jobs/) |
| **Lépések** | LWM2M egy olyan protokoll, az IoT-felügyeleti nyissa meg a Mobile Alliance hello. Az Azure IoT-eszközfelügyeleti lehetővé teszi, hogy toointeract a fizikai eszközök eszköz feladatok. Győződjön meg arról, hogy hello Felhőátjáróhoz valósítja meg a folyamat tooroutinely megtartása hello eszköz és egyéb konfigurációs adatok mentése toodate kezelés Azure IoT Hub használatával. |

## <a id="controls-policies"></a>Győződjön meg arról, hogy eszközök szervezeti házirendek szerint beállított végpont biztonsági vezérlő

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Gép megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Győződjön meg arról, hogy eszközök végponti biztonsági vezérlők, például a lemez szintű titkosítást, a víruskereső frissített aláírásokkal bit-tárolót, gazdagép alapú tűzfal, az operációs rendszer frissítései, a csoport házirendek stb szervezeti biztonsági házirendek megfelelően vannak konfigurálva. |

## <a id="secure-keys"></a>Az Azure storage elérési kulcsok biztonságos kezelése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Storage | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Az Azure Storage biztonsági útmutató - a Tárfiók kulcsait kezelése](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_managing-your-storage-account-keys) |
| **Lépések** | <p>Kulcstároló:, Akkor ajánlott toostore hello Azure Tárelérési kulcsot az Azure Key Vault egy titkos kulcsként, és hello kulcs lekérése kulcstároló hello alkalmazásokat. Az ajánlott miatt toohello következő okok miatt:</p><ul><li>hello alkalmazás soha nem lesz hello tárolási kulcs szoftveresen kötött a konfigurációs fájlban, amely eltávolítja az adott erőfeszítések valaki fog hozzáférni toohello kulcsok külön engedélye nélkül az</li><li>Toohello hívóbetűk Azure Active Directory használatával lehet irányítani. Ez azt jelenti, hogy egy fiók tulajdonosának biztosíthat hozzáférést toohello néhány olyan tooretrieve hello kulcsok Azure Key Vault származó alkalmazásokat. Más alkalmazások nem lesznek képesek tooaccess hello kulcsok anélkül, hogy azok kifejezetten engedéllyel kellene</li><li>Kulcs újragenerálása: Ajánlott toohave egy folyamatot az Azure storage access hely tooregenerate kulcsok biztonsági okokból. Miért és hogyan tooplan a kulcs újragenerálása ismertetett hello Azure tárolási biztonsági útmutatója részleteinek cikk hivatkozik</li></ul>|

## <a id="cors-storage"></a>Győződjön meg arról, hogy csak megbízható források engedélyezve legyenek-e, ha a CORS engedélyezve van az Azure storage

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Storage | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Hello Azure Storage szolgáltatásainak CORS-támogatás](https://msdn.microsoft.com/library/azure/dn535601.aspx) |
| **Lépések** | Az Azure Storage tooenable CORS – Cross eredetű erőforrások megosztása lehetővé teszi. Minden tárfiók hello erőforrásokhoz, hogy a tárfiók tartományokat is megadhat. A CORS alapértelmezés szerint le van tiltva az összes szolgáltatás. A CORS hello REST API vagy hello tárolási ügyfél könyvtár toocall hello módszerek tooset hello szolgáltatás házirendek egyikét használatával engedélyezheti. |

## <a id="throttling"></a>A WCF szolgáltatás szabályozásával engedélyezése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | .NET-keretrendszer 3 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg Királyság](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Lépések** | <p>Erőforrások erőforrás-fogyási, és végső soron szolgáltatásmegtagadást okozhat a rendszer nem helyezi maximális hello használja.</p><ul><li>**Magyarázat:** Windows Communication Foundation (WCF) hello képességét toothrottle szolgáltatáskérések kínál. Túl sok ügyfél kérésének engedélyezése a rendszer kéréssekkel, és az erőforrások lefoglalhat. A hello ugyanakkor, így csak kevés kérelem tooa szolgáltatás képes megakadályozni legitim felhasználók hello szolgáltatás használatával. Minden szolgáltatás konfigurált külön-külön bennünket tooand tooallow hello megfelelő mennyiségű erőforrást kell lennie.</li><li>**JAVASLATOK** engedélyezése WCF szabályozási szolgáltatás és a megadott korlát a megfelelő alkalmazás.</li></ul>|

### <a name="example"></a>Példa
hello következő egy példa konfiguráció sávszélesség-szabályozás engedélyezve:
```
<system.serviceModel> 
  <behaviors>
    <serviceBehaviors>
    <behavior name="Throttled">
    <serviceThrottling maxConcurrentCalls="[YOUR SERVICE VALUE]" maxConcurrentSessions="[YOUR SERVICE VALUE]" maxConcurrentInstances="[YOUR SERVICE VALUE]" /> 
  ...
</system.serviceModel> 
```

## <a id="info-metadata"></a>WCF-adatokhoz való illetéktelen hozzáférés metaadatok keresztül

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | .NET-keretrendszer 3 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg Királyság](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Lépések** | Metaadatok segítségével a támadók hello rendszer információ, és tervezze meg a támadás űrlap. WCF-szolgáltatások konfigurált tooexpose metaadatok lehet. Metaadatok szolgáltatás részletes leírását tartalmazza, és nem kell éles környezetben szórási. Hello `HttpGetEnabled`  /  `HttpsGetEnabled` hello ServiceMetaData osztály tulajdonságait határozza meg, hogy egy szolgáltatás hello metaadatok teszi közzé. | 

### <a name="example"></a>Példa
az alábbi kód hello utasítja WCF toobroadcast a szolgáltatás metaadatai
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
smb.HttpGetEnabled = true; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb); 
```
Éles környezetben a szolgáltatás metaadatai nem szórási. Állítsa be a hello HttpGetEnabled / hello ServiceMetaData HttpsGetEnabled tulajdonságainak toofalse osztály. 

### <a name="example"></a>Példa
az alábbi kód hello arra utasítja a WCF toonot szórási a szolgáltatás metaadatai. 
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior(); 
smb.HttpGetEnabled = false; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb);
```
