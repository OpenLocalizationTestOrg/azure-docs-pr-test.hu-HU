---
title: "-Microsoft fenyegetések modellezése eszköz - adatok Azure aaaSensitive |} Microsoft Docs"
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
ms.openlocfilehash: 8a18f43af439241ba193ccf668971ddc4655355f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-sensitive-data--mitigations"></a>Biztonsági keret: Bizalmas adatok |} Megoldást 
| A termék vagy szolgáltatás | Cikk |
| --------------- | ------- |
| **Gép megbízhatósági kapcsolat határán** | <ul><li>[Győződjön meg arról, hogy bináris formában vannak, ha bizalmas információkat tartalmaznak](#binaries-info)</li><li>[Fontolja meg, a titkosított fájlrendszer (EFS) használt tooprotect bizalmas felhasználói adatok](#efs-user)</li><li>[Győződjön meg arról, hogy hello alkalmazás hello fájlrendszeren tárolt bizalmas adatok titkosítása](#filesystem)</li></ul> | 
| **Webalkalmazás** | <ul><li>[Győződjön meg arról, hogy bizalmas tartalom gyorsítótárazva van, nem hello böngésző](#cache-browser)</li><li>[Webes alkalmazás konfigurációs fájlja bizalmas adatokat tartalmazó szakasz titkosítása](#encrypt-data)</li><li>[Explicit módon letiltja az hello autocomplete HTML-attribútum bizalmas űrlapokban és a bemeneti adatok](#autocomplete-input)</li><li>[Győződjön meg arról, hogy van-e maszkolva bizalmas adatokat a hello felhasználói képernyőn jelenik meg](#data-mask)</li></ul> | 
| **Adatbázis** | <ul><li>[Dinamikus adatok maszkolása toolimit bizalmas adatok veszélyeztetettségének nem kiemelt felhasználók megvalósítása](#dynamic-users)</li><li>[Győződjön meg arról, hogy a jelszavak sózott kivonatoló formátumban kell tárolni](#salted-hash)</li><li>[Győződjön meg arról, hogy adatbázismezőknek bizalmas adatok titkosítva van](#db-encrypted)</li><li>[Győződjön meg arról, hogy adatbázis szintű titkosítást (TDE) engedélyezve van](#tde-enabled)</li><li>[Győződjön meg arról, hogy titkosítva legyenek-e az adatbázis biztonsági mentése](#backup)</li></ul> | 
| **Webes API** | <ul><li>[Győződjön meg arról, hogy a böngésző tárolási nem tárolja a API bizalmas adatok vonatkozó tooWeb](#api-browser)</li></ul> | 
| Az Azure Document DB rendszerbe | <ul><li>[A documentdb-ben tárolt bizalmas adatok titkosítása](#encrypt-docdb)</li></ul> | 
| **Azure IaaS virtuális gép megbízhatósági kapcsolat határán** | <ul><li>[Virtuális gépek által használt használata Azure Disk Encryption tooencrypt lemezek](#disk-vm)</li></ul> | 
| **Service Fabric megbízhatósági kapcsolat határán** | <ul><li>[Service Fabric-alkalmazások titkos kulcsainak titkosítása](#fabric-apps)</li></ul> | 
| **Dynamics CRM** | <ul><li>[Biztonsági modellezési üzleti egység/csoportok használható, és ha szükséges](#modeling-teams)</li><li>[Hozzáférés tooshare funkciót kritikus entitások minimalizálása érdekében](#entities)</li><li>[A Dynamics CRM-megosztás funkció hello és jó biztonsági gyakorlat hello kockázatok vonat felhasználók](#good-practices)</li><li>[Közé tartozik a megjelenítő konfigurációs részletek a kivételek kezelése proscribing fejlesztési szabványok szabály](#exception-mgmt)</li></ul> | 
| **Azure Storage** | <ul><li>[Használja az Azure Storage Service Encryption (SSE) for Data at Rest (Preview)](#sse-preview)</li><li>[Az Azure Storage ügyféloldali titkosítás toostore bizalmas adatok használata](#client-storage)</li></ul> | 
| **Mobileszköz ügyfél** | <ul><li>[Érzékeny vagy a helyi tároló toophones írt személyazonosításra alkalmas adatok titkosítása](#pii-phones)</li><li>[Generált bináris takarják tooend felhasználók kiosztása előtt](#binaries-end)</li></ul> | 
| **WCF** | <ul><li>[Set clientCredentialType tooCertificate vagy a Windows](#cert)</li><li>[WCF-biztonsági mód nem engedélyezett.](#security)</li></ul> | 

## <a id="binaries-info"></a>Győződjön meg arról, hogy bináris formában vannak, ha bizalmas információkat tartalmaznak

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Gép megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Győződjön meg arról, hogy bináris rejtjelezett vannak, ha azok tartalmaznak olyan bizalmas adatokat, például kereskedelmi titkok, bizalmas üzleti logika, amely kell nem fordított irányú. Ez a toostop visszafejtése szerelvények. Eszközök, például `CryptoObfuscator` erre a célra használható. |

## <a id="efs-user"></a>Fontolja meg, a titkosított fájlrendszer (EFS) használt tooprotect bizalmas felhasználói adatok

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Gép megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Érdemes lehet a titkosított fájlrendszer (EFS) használt tooprotect bizalmas felhasználói adatok való fizikai hozzáférés toohello számítógéppel szemléltetik. |

## <a id="filesystem"></a>Győződjön meg arról, hogy hello alkalmazás hello fájlrendszeren tárolt bizalmas adatok titkosítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Gép megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Győződjön meg arról, hogy hello alkalmazás hello fájlrendszeren tárolt bizalmas adatok titkosítva van (például a DPAPI-t használ), ha az EFS nem kényszeríthető. |

## <a id="cache-browser"></a>Győződjön meg arról, hogy bizalmas tartalom gyorsítótárazva van, nem hello böngésző

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, Web Forms, MVC5, MVC6 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Böngészők célokra és adatok gyorsítótárazása előzmények tárolhatja. A gyorsítótárazott fájlok, például az Internet Explorer hello esetben hello ideiglenes internetfájlok mappa egy mappában tárolják. Ha az adott lapok újra hivatkozunk, hello böngésző megjeleníti azokat a gyorsítótárból. Ha bizalmas információk (például a címet, a hitelkártya adatait, az társadalombiztosítási szám vagy a felhasználónév) megjelenített toohello felhasználó, akkor ezeket az információkat lehet a böngésző gyorsítótárában tárolt, és ezért lekérhető keresztül hello a gyorsítótár vizsgálata vagy lenyomásával egyszerűen hello böngésző "Újra" gombra. Állítsa be a cache-control válasz állomásfejléc-érték túl "no-store" minden lap. |

### <a name="example"></a>Példa
```XML
<configuration>
  <system.webServer>
   <httpProtocol>
    <customHeaders>
        <add name="Cache-Control" value="no-cache" />
        <add name="Pragma" value="no-cache" />
        <add name="Expires" value="-1" />
    </customHeaders>
  </httpProtocol>
 </system.webServer>
</configuration>
```

### <a name="example"></a>Példa
Ez a szűrő keresztül hajtható végre. Használható a következő példa: 
```C#
public override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            if (filterContext == null || (filterContext.HttpContext != null && filterContext.HttpContext.Response != null && filterContext.HttpContext.Response.IsRequestBeingRedirected))
            {
                //// Since this is MVC pipeline, this should never be null.
                return;
            }

            var attributes = filterContext.ActionDescriptor.GetCustomAttributes(typeof(System.Web.Mvc.OutputCacheAttribute), false);
            if (attributes == null || **Attributes**.Count() == 0)
            {
                filterContext.HttpContext.Response.Cache.SetNoStore();
                filterContext.HttpContext.Response.Cache.SetCacheability(HttpCacheability.NoCache);
                filterContext.HttpContext.Response.Cache.SetExpires(DateTime.UtcNow.AddHours(-1));
                if (!filterContext.IsChildAction)
                {
                    filterContext.HttpContext.Response.AppendHeader("Pragma", "no-cache");
                }
            }

            base.OnActionExecuting(filterContext);
        }
``` 

## <a id="encrypt-data"></a>Webes alkalmazás konfigurációs fájlja bizalmas adatokat tartalmazó szakasz titkosítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Útmutató: Az ASP.NET 2.0 használatával DPAPI konfigurációs szakasz titkosítása](https://msdn.microsoft.com/library/ff647398.aspx), [adja meg egy védett Konfigurációszolgáltatót](https://msdn.microsoft.com/library/68ze1hb2.aspx), [Azure Key Vault használatával tooprotect alkalmazás titkos kulcsok](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Lépések** | Például hello Web.config konfigurációs fájl, appsettings.json van gyakran használt toohold bizalmas adatokat, beleértve a felhasználónevek, jelszavak, adatbázis-kapcsolati karakterláncok és titkosítási kulcsokat. Ez az információ nem védi, az alkalmazás akkor sebezhető tooattackers vagy rosszindulatú felhasználók megszerezni a bizalmas adatokat, például a fiók felhasználói nevét és a jelszavak, a adatbázis nevét és a kiszolgálók nevei. Hello központi telepítési típus (azure vagy a helyszínen) alapuló, titkosítása hello bizalmas DPAPI-t vagy szolgáltatásokat, mint az Azure Key Vault segítségével konfigurációs fájljainak szakaszait. |

## <a id="autocomplete-input"></a>Explicit módon letiltja az hello autocomplete HTML-attribútum bizalmas űrlapokban és a bemeneti adatok

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN: automatikus kiegészítés attribútum](http://msdn.microsoft.com/library/ms533486(VS.85).aspx), [automatikus kiegészítési funkciójának használatával HTML](http://msdn.microsoft.com/library/ms533032.aspx), [HTML-tisztítási biztonsági](http://technet.microsoft.com/security/bulletin/MS10-071), [Autocomplete., újra?](http://blog.mindedsecurity.com/2011/10/autocompleteagain.html) |
| **Lépések** | hello autocomplete attribútum Megadja, hogy kell-e az űrlap a be- és kikapcsolása autocomplete kapcsolatos rendelkeznek. Ha az automatikus kiegészítés, hello böngésző automatikusan értékek értékek alapján, hogy hello felhasználó megadott előtt. Például amikor egy új nevet és jelszót is meg kell adni egy űrlapon, és hello űrlap elküldése, hello böngésző megkérdezi, hogy ha hello jelszó mentésére. Ezt követően hello képernyő jelenik meg, amikor hello nevét és jelszavát automatikusan kitölti, vagy megadta, hello nevét is meg kell adni. Helyi hozzáféréssel rendelkező támadó hello tiszta szöveges jelszavak hello böngésző gyorsítótárából szerezze be. Alapértelmezés szerint engedélyezve van az automatikus kiegészítés, és explicit módon le kell tiltani. |

### <a name="example"></a>Példa
```C#
<form action="Login.aspx" method="post " autocomplete="off" >
      Social Security Number: <input type="text" name="ssn" />
      <input type="submit" value="Submit" />    
</form>
```

## <a id="data-mask"></a>Győződjön meg arról, hogy van-e maszkolva bizalmas adatokat a hello felhasználói képernyőn jelenik meg

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Amikor megjelenik az üdvözlő képernyőt kell legyen maszkolva a bizalmas adatokat, például jelszavakat, hitelkártyaszámokat tartalmazó SSN stb. Ez a nem engedélyezett tooprevent személyzet hello adatokat (például a képernyőre pillant-surfing jelszavak, SSN felhasználóval megtekintése a technikai támogatási csoporthoz) hozzáférését. Győződjön meg arról, hogy ezek az adatok elemek nem láthatók el a formázatlan szöveges megfelelően maszkolva. Ez azokat (pl. bemenetként elfogadása közben végrehajtott fontos toobe rendelkezik. Adjon meg típust = a "password") valamint vissza a program üdvözlő képernyőt (pl. megjelenítési csak hello hello hitelkártyaszám utolsó 4 számjegyével). |

## <a id="dynamic-users"></a>Dinamikus adatok maszkolása toolimit bizalmas adatok veszélyeztetettségének nem kiemelt felhasználók megvalósítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Az SQL Azure, a helyi üzemeltetésű |
| **Attribútumok**              | SQL - 12-es verzió, az SQL-verzió - verzió MsSQL2016 |
| **Hivatkozások**              | [Dinamikus Adatmaszkolási](https://msdn.microsoft.com/library/mt130841) |
| **Lépések** | hello dinamikus adatmaszkolási célja toolimit a bizalmas adatok felfedésnek, felhasználók, akik nem rendelkezhet hozzáférés toohello adatokat megtekintés megakadályozza. Dinamikus adatok maszkolása nem célja tooprevent adatbázis felhasználók toohello adatbázis közvetlen csatlakozás és a futó teljes körű lekérdezések hello bizalmas adatok visszaállítását. Dinamikus adatmaszkolási kiegészítő tooother SQL Server biztonsági funkciói (naplózás, a titkosítás és sorszintű biztonság...), és lehetőleg toouse azokat együtt ez a szolgáltatás továbbá rendelés toobetter a hello bizalmas adatok védelmének hello az adatbázis. Vegye figyelembe, hogy ez a funkció csak az SQL Server 2016 kezdve és az Azure SQL Database által támogatott. |

## <a id="salted-hash"></a>Győződjön meg arról, hogy a jelszavak sózott kivonatoló formátumban kell tárolni

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Jelszó Hashing .NET titkosítási API-k használatával](http://docs.asp.net/en/latest/security/data-protection/consumer-apis/password-hashing.html) |
| **Lépések** | Jelszavak egyéni felhasználói tároló adatbázisok nem kell tárolni. Jelszó-kivonatok helyette védőérték értékekkel kell tárolni. Ellenőrizze, hogy hello védőérték hello felhasználó mindig egyedi és alkalmazása b-crypt program segítségével, a titkosítási-s vagy PBKDF2 hello jelszó 150 000 minimális munkahelyi tényező iterációs számaival együtt tárolja a hurokban, tooeliminate hello lehetőségét, amely találgatásos újraindítás előtt.| 

## <a id="db-encrypted"></a>Győződjön meg arról, hogy adatbázismezőknek bizalmas adatok titkosítva van

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | SQL-verzió - minden |
| **Hivatkozások**              | [Az SQL server bizalmas adatok titkosítása](https://technet.microsoft.com/library/ff848751(v=sql.105).aspx), [hogyan: egy SQL Server adatok oszlop titkosítása](https://msdn.microsoft.com/library/ms179331), [tanúsítvány titkosítása](https://msdn.microsoft.com/library/ms188061) |
| **Lépések** | Érzékeny adatok, például a hitelkártyaszámukat hello adatbázisban titkosított toobe rendelkezik. Adatok titkosíthatók oszlopszintű titkosítással vagy egy alkalmazás függvény hello titkosítási funkciókat használja. |

## <a id="tde-enabled"></a>Győződjön meg arról, hogy adatbázis szintű titkosítást (TDE) engedélyezve van

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [SQL Server átlátható adattitkosítás (TDE) ismertetése](https://technet.microsoft.com/library/bb934049(v=sql.105).aspx) |
| **Lépések** | Transzparens Data Encryption (TDE) beállítást, az SQL server segítségével titkosított egy adatbázisban lévő bizalmas adatokat, és hello kulcsokat is használt tooencrypt hello adatok tanúsítvánnyal védelmét. Ez megakadályozza, hogy bárki hello kulcsok nélkül hello adatokkal. TDE adatok védelmét "Inaktív", azaz hello adatainak és naplókönyvtárainak fájlokat. Hello képességét toocomply nyújtja a sok törvényi, a szabályozókat és a különböző iparágak létrehozott iránymutatásokat. |

## <a id="backup"></a>Győződjön meg arról, hogy titkosítva legyenek-e az adatbázis biztonsági mentése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Az SQL Azure, a helyi üzemeltetésű |
| **Attribútumok**              | SQL - 12-es verzió, az SQL-verzió - verzió MsSQL2014 |
| **Hivatkozások**              | [SQL-adatbázis a biztonsági mentés titkosítása](https://msdn.microsoft.com/library/dn449489) |
| **Lépések** | SQL Server biztonsági másolat létrehozásakor hello képességét tooencrypt hello adatokat tartalmaz. Hello titkosítási algoritmus és hello titkosító (tanúsítvánnyal vagy aszimmetrikus kulcs) megadásával a biztonsági másolat létrehozásakor egy hozhat létre egy titkosított biztonságimásolat-fájl. |

## <a id="api-browser"></a>Győződjön meg arról, hogy a böngésző tárolási nem tárolja a API bizalmas adatok vonatkozó tooWeb

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | MVC 5, 6 MVC |
| **Attribútumok**              | Identity Provider - ADFS, identitásszolgáltató - az Azure AD |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Bizonyos implementációkban érzékeny összetevőihez vonatkozó tooWeb API hitelesítési böngésző helyi tároló vannak tárolva. Például az Azure AD hitelesítési összetevők például a adal.idtoken, adal.nonce.idtoken, adal.access.token.key, adal.token.keys, adal.state.login, adal.session.state, adal.expiration.key stb.</p><p>Ezen összetevők után érhetők el az összes jelentkezzen ki, vagy a böngésző le van zárva. Ha egy ellenfél toothese összetevők lekérdezi az access, többé felhasználhatja őket tooaccess hello védett erőforrások (API). Győződjön meg arról, hogy az összes érzékeny összetevőihez kapcsolódó tooWeb API nem tárolódik a böngésző tároló. Azokban az esetekben, ahol az ügyféloldali tárolási elkerülhetetlen (egy lap alkalmazások (SPA) használó Implicit OpenIdConnect/OAuth adatfolyamok kell például a toostore hozzáférési jogkivonatok helyileg is), használja tárolási lehetőségek a nincs megőrzését. például inkább SessionStorage tooLocalStorage.</p>| 

### <a name="example"></a>Példa
alább JavaScript részlet hello egy egyéni hitelesítési tár hitelesítési összetevők tárol a helyi tároló származik. Ilyen megvalósítások el kell kerülni. 
```javascript
ns.AuthHelper.Authenticate = function () {
window.config = {
instance: 'https://login.microsoftonline.com/',
tenant: ns.Configurations.Tenant,
clientId: ns.Configurations.AADApplicationClientID,
postLogoutRedirectUri: window.location.origin,
cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
};
```

## <a id="encrypt-docdb"></a>A Cosmos DB tárolt bizalmas adatok titkosítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az Azure Document DB rendszerbe | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Alkalmazás szinten bizalmas adatok titkosítására dokumentum DB tárolása előtt, vagy a bizalmas adatok tárolása egyéb tárolási megoldások, például az Azure Storage vagy az Azure SQL| 

## <a id="disk-vm"></a>Virtuális gépek által használt használata Azure Disk Encryption tooencrypt lemezek

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure IaaS virtuális gép megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Használja az Azure Disk Encryption tooencrypt lemezt a virtuális gépek által használt](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) |
| **Lépések** | <p>Az Azure Disk Encryption az új szolgáltatása, amely jelenleg előzetes verzióban érhetők. Ez a funkció lehetővé teszi tooencrypt hello az operációs rendszer és az infrastruktúra-szolgáltatási virtuális gép által használt adatok lemezek. A Windows hello meghajtók titkosítása szabványos BitLocker titkosítás technológia használatával. A Linux hello lemezek titkosítása hello DM-Crypt technológia használatával. Ez az integrálva van az Azure Key Vault tooallow meg toocontrol és hello lemez titkosítási kulcsok kezeléséhez. hello Azure Disk Encryption megoldás támogatja a következő három titkosítási forgatókönyvet hello:</p><ul><li>Engedélyezze a titkosítást a felhasználói által titkosított fájlok és az ügyfél által megadott titkosítási kulcs esetében, amelyek tárolódnak az Azure Key Vault létrehozott új IaaS virtuális gépeken.</li><li>Engedélyezheti a titkosítást hello Azure piactér alapján létrehozott új IaaS virtuális gépeket.</li><li>Engedélyezze a titkosítást a meglévő infrastruktúra-szolgáltatási virtuális gépeken már fut az Azure-ban.</li></ul>| 

## <a id="fabric-apps"></a>Service Fabric-alkalmazások titkos kulcsainak titkosítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Service Fabric megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Környezet – Azure |
| **Hivatkozások**              | [A Service Fabric-alkalmazások titkos kulcsok kezelése](https://azure.microsoft.com/documentation/articles/service-fabric-application-secret-management/) |
| **Lépések** | Titkos kulcsok lehet bármely bizalmas adatokat, például a tárolási kapcsolati karakterláncok, jelszavak és egyéb értékek, amelyek nem egyszerű szöveges kezelje. Service fabric-alkalmazások az Azure Key Vault toomanage kulcsok és titkos használja. |

## <a id="modeling-teams"></a>Biztonsági modellezési üzleti egység/csoportok használható, és ha szükséges

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Dynamics CRM | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Biztonsági modellezési üzleti egység/csoportok használható, és ha szükséges |

## <a id="entities"></a>Hozzáférés tooshare funkciót kritikus entitások minimalizálása érdekében

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Dynamics CRM | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Hozzáférés tooshare funkciót kritikus entitások minimalizálása érdekében |

## <a id="good-practices"></a>A Dynamics CRM-megosztás funkció hello és jó biztonsági gyakorlat hello kockázatok vonat felhasználók

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Dynamics CRM | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | A Dynamics CRM-megosztás funkció hello és jó biztonsági gyakorlat hello kockázatok vonat felhasználók |

## <a id="exception-mgmt"></a>Közé tartozik a megjelenítő konfigurációs részletek a kivételek kezelése proscribing fejlesztési szabványok szabály

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Dynamics CRM | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Közé tartozik a megjelenítő konfigurációs részletek a kivételek kezelése fejlesztési kívül proscribing fejlesztési szabványok szabály. Ellenőrizze, hogy ez a kód értékelést vagy a rendszeres ellenőrzés részeként.|

## <a id="sse-preview"></a>Használja az Azure Storage Service Encryption (SSE) for Data at Rest (Preview)

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Storage | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | StorageType - Blob |
| **Hivatkozások**              | [Az Azure Storage Service Encryption for Data at Rest (Preview)](https://azure.microsoft.com/documentation/articles/storage-service-encryption/) |
| **Lépések** | <p>Az Azure Storage szolgáltatás titkosítási (SSE) inaktív adatok segítségével és az adatok toomeet megvédeni a szervezeti biztonsági és megfelelőségi jár kötelezettségekkel. Ez a szolgáltatás Azure Storage automatikusan titkosítja az adatokat a korábbi toopersisting toostorage és előzetes tooretrieval visszafejti. hello titkosítási, visszafejtési és kulcs kezelése teljes mértékben transzparens toousers. SSE csak tooblock blobokat, lapblobokat, vonatkozik, és a hozzáfűző blobokhoz. hello más típusú adatok, beleértve a táblák, üzenetsorok és fájlok, nem lesznek titkosítva.</p><p>Titkosítás és visszafejtés munkafolyamat:</p><ul><li>hello ügyfél lehetővé teszi, hogy a titkosítás hello storage-fiók</li><li>Ha hello ügyfél írja az új (PUT Blob, PUT blokk, PUT lap, stb.) tooBlob adattárolás; minden egyes van titkosítva, 256 bites AES titkosítással, hello legerősebb blokk Rejtjelek elérhető egyik</li><li>Ha hello ügyféligények tooaccess adatokat (a Blob LEKÉRÉSE, stb.), adatai automatikusan visszafejtett toohello felhasználói visszaküldés előtt.</li><li>Védelem le van tiltva, ha új írási műveletek többé nem lesznek titkosítva, és a meglévő titkosított adatok titkosítva maradnak, amíg hello felhasználó írni. Titkosítási be van kapcsolva, írások tooBlob tárolási lesz titkosítva. az adatok hello állapotát nem változik hello felhasználó váltása hello tárfiók titkosítás engedélyezése vagy tiltása</li><li>Minden titkosítási kulcs tárolása, titkosított és a Microsoft által felügyelt</li></ul><p>Ne feledje, hogy jelenleg, Microsoft által felügyelt hello titkosításhoz használt hello kulcsok. Microsoft hello kulcsok eredetileg állít elő, és a belső Microsoft házirend által meghatározott hello biztonságos tárolására hello kulcsok, valamint a hello rendszeres elforgatási felügyelete. A jövőbeli hello, az ügyfelek megkapják hello képességét toomanage saját > titkosítási kulcsokat, és adja meg a Microsoft által felügyelt kulcsok áttelepítési elérési kulcsok toocustomer által felügyelt.</p>| 

## <a id="client-storage"></a>Az Azure Storage ügyféloldali titkosítás toostore bizalmas adatok használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Storage | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Ügyféloldali titkosítás és a Microsoft Azure tárolás az Azure Key Vault](https://azure.microsoft.com/documentation/articles/storage-client-side-encryption/), [oktatóanyag: titkosításához és visszafejtéséhez az Azure Key Vault használatával a Microsoft Azure Storage blobs](https://azure.microsoft.com/documentation/articles/storage-encrypt-decrypt-blobs-key-vault/), [adattárolás az Azure-Blobba Tárolás az Azure titkosítási kiterjesztésű](https://blogs.msdn.microsoft.com/partnercatalystteam/2015/06/17/storing-data-securely-in-azure-blob-storage-with-azure-encryption-extensions/) |
| **Lépések** | <p>hello Azure Storage ügyféloldali kódtára a .NET Nuget-csomag támogatja a titkosított adatok ügyfélalkalmazásokon belüli előtt tooAzure tárolási feltöltését, és az adatok visszafejtése toohello ügyfél letöltése során. hello kódtár emellett támogatja az Azure Key Vault integration tárfiókkulcs-kezelés számára. Ügyféloldali titkosítása működése rövid leírása itt található:</p><ul><li>hello Azure Storage ügyfél SDK állít elő, a tartalom titkosítási kulcs (CEK), amely a szimmetrikus kulcs egy egyszeri használata</li><li>Felhasználói adatok titkosítása a CEK</li><li>hello CEK ezt követően (titkosított) hello kulcs titkosítási kulcs (KEK) használatával. hello KEK kulcsazonosítójával azonosíthatók és aszimmetrikus kulcspár vagy egy szimmetrikus kulcsot kell és is kezelhetők helyileg vagy az Azure Key Vault tárolja. hello tárolási ügyfélen soha nem hozzáférés toohello KEK. Egyszerűen meghívja a Key Vault által biztosított hello kulcs alkalmazásburkoló algoritmus. Az ügyfelek kiválaszthatja egyéni szolgáltatók toouse alkalmazásburkoló/kicsomagolásával Ha szeretné, hogy azok kulcs</li><li>hello titkosított adatok van majd toohello Azure Storage szolgáltatás feltöltve. Ellenőrizze a hello hivatkozások hello references szakaszában, az alacsony szintű megvalósítás részletei.</li></ul>|

## <a id="pii-phones"></a>Érzékeny vagy a helyi tároló toophones írt személyazonosításra alkalmas adatok titkosítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Mobileszköz ügyfél | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, Xamarin  |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Beállításainak és funkcióinak az eszközök a Microsoft Intune-házirendek kezelése](https://docs.microsoft.com/intune/deploy-use/manage-settings-and-features-on-your-devices-with-microsoft-intune-policies#create-a-configuration-policy), [kulcslánc Valet](https://components.xamarin.com/view/square.valet) |
| **Lépések** | <p>Ha hello alkalmazás írja az érzékeny adatok, például a felhasználó személyhez köthető adatokat (e-mail, telefonszám, utónevét, vezetéknevét, beállítások stb.) -mobileszköz a fájlrendszerben, majd azt titkosítani kell toohello helyi fájlrendszer írása előtt. Ha már egy vállalati alkalmazás hello alkalmazás, így megismerkedhet hello lehetőségét, hogy a Windows Intune-nal közzétételi alkalmazás.</p>|

### <a name="example"></a>Példa
Intune a következő biztonsági házirendek toosafeguard bizalmas adatok konfigurálható: 
```C#
Require encryption on mobile device    
Require encryption on storage cards
Allow screen capture
```

### <a name="example"></a>Példa
Ha hello alkalmazás nem egy vállalati alkalmazás, majd a megadott keystore használata platform, hello fájlrendszerben keychains toostore titkosítási kulcsokat, amely kriptográfiai művelet használatával hajthatja végre. Kód következő kódrészletben láthatja, hogyan tooaccess xamarin használata kulcslánc kulcsot: 
```C#
        protected static string EncryptionKey
        {
            get
            {
                if (String.IsNullOrEmpty(_Key))
                {
                    var query = new SecRecord(SecKind.GenericPassword);
                    query.Service = NSBundle.MainBundle.BundleIdentifier;
                    query.Account = "UniqueID";

                    NSData uniqueId = SecKeyChain.QueryAsData(query);
                    if (uniqueId == null)
                    {
                        query.ValueData = NSData.FromString(System.Guid.NewGuid().ToString());
                        var err = SecKeyChain.Add(query);
                        _Key = query.ValueData.ToString();
                    }
                    else
                    {
                        _Key = uniqueId.ToString();
                    }
                }

                return _Key;
            }
        }
```

## <a id="binaries-end"></a>Generált bináris takarják tooend felhasználók kiosztása előtt

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Mobileszköz ügyfél | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [A .NET-hez titkosítási címmódosítás](http://www.ssware.com/cryptoobfuscator/obfuscator-net.htm) |
| **Lépések** | Generált bináris fájlokat (szerelvények apk belül) visszafejtése szerelvények rejtjelezett toostop kell lennie. Eszközök, például `CryptoObfuscator` erre a célra használható. |

## <a id="cert"></a>Set clientCredentialType tooCertificate vagy a Windows

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | .NET-keretrendszer 3 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Erősítse meg](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Lépések** | Hello jelszó tooattackers, akik hello SOAP-üzenetekkel is figyelheti a UsernameToken használata a titkosítatlan szöveges jelszót titkosítatlan csatornán keresztül elérhetővé teszi. Hello UsernameToken használó szolgáltatók előfordulhat, hogy fogadja el a jelszavak nem titkosított szövegként küldi. Egyszerű szöveges jelszavak küldése titkosítatlan csatornán keresztül is elérhetővé teheti hello credential tooattackers, akik hello SOAP-üzenetet is figyelheti. | 

### <a name="example"></a>Példa
hello következő WCF szolgáltató konfigurációja használja hello UsernameToken: 
```
<security mode="Message"> 
<message clientCredentialType="UserName" />
``` 
ClientCredentialType tooCertificate vagy a Windows beállítása. 

## <a id="security"></a>WCF-biztonsági mód nem engedélyezett.

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, .NET-keretrendszer 3 |
| **Attribútumok**              | Biztonsági mód - átvitel, biztonsági üzemmód - üzenet |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Királyság erősítse](https://vulncat.fortify.com/en/vulncat/index.html), [WCF biztonsági alapok CoDe magazin](http://www.codemag.com/article/0611051) |
| **Lépések** | Nincs transport vagy üzenet biztonság van definiálva. Alkalmazások, amelyek üzenetek nélkül transport vagy biztonsági nem garantálható, hogy hello épsége vagy bizalmassága hello üzenetek üzenetet továbbítani. Ha egy WCF biztonsági kötések tooNone, átviteli és üzenet biztonsági le vannak tiltva. |

### <a name="example"></a>Példa
a következő konfigurációs készletek hello biztonsági mód tooNone hello. 
```
<system.serviceModel> 
  <bindings> 
    <wsHttpBinding> 
      <binding name=""MyBinding""> 
        <security mode=""None""/> 
      </binding> 
  </bindings> 
</system.serviceModel> 
```

### <a name="example"></a>Példa
Biztonsági mód között az összes szolgáltatási kötések nem öt lehetséges biztonsági mód: 
* nincs. Biztonsági kikapcsolása. 
* Átviteli. Használja a kölcsönös hitelesítés és az üzenet védelmi biztonsági átviteli. 
* Üzenet. Kölcsönös hitelesítés és az üzenet védelmi üzenetbiztonság használ. 
* Mindkettő. Lehetővé teszi toosupply beállítások átviteli és az üzenet-biztonság (csak MSMQ támogatja ezt). 
* TransportWithMessageCredential. Üdvözlőüzenetére és az üzenet védelmi átadott hitelesítő adatokat, és kiszolgálóhitelesítés hello szállítási réteg által biztosított. 
* TransportCredentialOnly. Hello átviteli réteg átadott ügyfél hitelesítő adatait, és nem üzenet védelem akkor lép életbe. Üzenet és a transport biztonsági tooprotect hello sértetlenségét és bizalmasságát az üzenetek használata. az alábbi hello konfigurációs közli hello szolgáltatás toouse a transport security üzenet hitelesítő adatokkal.
```
<system.serviceModel>
  <bindings>
    <wsHttpBinding>
    <binding name=""MyBinding""> 
    <security mode=""TransportWithMessageCredential""/> 
    <message clientCredentialType=""Windows""/> 
    </binding> 
  </bindings> 
</system.serviceModel> 
```
