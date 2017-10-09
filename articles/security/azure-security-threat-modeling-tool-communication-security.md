---
title: "-Microsoft fenyegetések modellezése eszköz - biztonsági Azure aaaCommunication |} Microsoft Docs"
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
ms.openlocfilehash: 667829c75123f4dbe0b383fdaf8cd899802f9b16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-communication-security--mitigations"></a>Biztonsági keret: Kommunikációs biztonsági |} Megoldást 
| A termék vagy szolgáltatás | Cikk |
| --------------- | ------- |
| **Az Azure Event Hubs** | <ul><li>[Központi SSL/TLS használatával biztonságos kommunikáció érdekében tooEvent](#comm-ssltls)</li></ul> |
| **Dynamics CRM** | <ul><li>[Ellenőrizze a jogosultságokat, és ellenőrizze, hogy egyéni szolgáltatások vagy az ASP.NET-lapok hello tiszteletben tartják CRM biztonsági szolgáltatásfiók](#priv-aspnet)</li></ul> |
| **Azure Data Factory** | <ul><li>[Csatlakozás a helyi SQL Server tooAzure adat-előállító közben az adatkezelési átjáró használata](#sqlserver-factory)</li></ul> |
| **Identity Serverben** | <ul><li>[Győződjön meg arról, hogy az összes forgalom tooIdentity kiszolgáló HTTPS-kapcsolaton keresztül](#identity-https)</li></ul> |
| **Webalkalmazás** | <ul><li>[X.509-tanúsítványokat használ, tooauthenticate SSL, a TLS és DTLS kapcsolatok ellenőrzése](#x509-ssltls)</li><li>[Az egyéni tartomány SSL tanúsítvány konfigurálása az Azure App Service-ben](#ssl-appservice)</li><li>[Az összes forgalom tooAzure App Service HTTPS-kapcsolaton keresztül kényszerítése](#appservice-https)</li><li>[HTTP szigorú a Transport Security (HSTS) engedélyezése](#http-hsts)</li></ul> |
| **Adatbázis** | <ul><li>[Ellenőrizze az SQL server-kapcsolat titkosítási és a tanúsítvány érvényesítése](#sqlserver-validation)</li><li>[Titkosított kommunikáció tooSQL kiszolgáló kényszerítése](#encrypted-sqlserver)</li></ul> |
| **Azure Storage** | <ul><li>[Győződjön meg arról, hogy kommunikációs tooAzure tároló van HTTPS-KAPCSOLATON keresztül](#comm-storage)</li><li>[MD5 kivonatoló érvényesítése után blob letöltése, ha HTTPS nem engedélyezhető.](#md5-https)</li><li>[Használja az SMB 3.0 kompatibilis ügyfél tooensure az átvitel közbeni adatok titkosítás tooAzure fájlmegosztások](#smb-shares)</li></ul> |
| **Mobileszköz ügyfél** | <ul><li>[Tanúsítvány rögzítését megvalósítása](#cert-pinning)</li></ul> |
| **WCF** | <ul><li>[Engedélyezze a HTTPS - az átviteli csatornát](#https-transport)</li><li>[WCF: Set Message biztonsági védelmi szint tooEncryptAndSign](#message-protection)</li><li>[WCF: Használja a legkevésbé jogosultsági szintű fiók toorun a WCF-szolgáltatás](#least-account-wcf)</li></ul> |
| **Webes API** | <ul><li>[Az összes forgalom tooWeb API-k HTTPS-kapcsolaton keresztül kényszerítése](#webapi-https)</li></ul> |
| **Azure Redis Cache** | <ul><li>[Győződjön meg arról, hogy kommunikációs tooAzure Redis Cache van SSL-en keresztül](#redis-ssl)</li></ul> |
| **Az IoT-mező átjáró** | <ul><li>[Eszköz tooField átjáró kommunikáció biztosításához](#device-field)</li></ul> |
| **Az IoT átjáró** | <ul><li>[Eszköz tooCloud átjáró kommunikációt SSL/TLS biztonságos](#device-cloud)</li></ul> |

## <a id="comm-ssltls"></a>Központi SSL/TLS használatával biztonságos kommunikáció érdekében tooEvent

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az Azure Event Hubs | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Event Hubs hitelesítés és a biztonsági modell – áttekintés](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Lépések** | Biztonságos AMQP vagy HTTP-kapcsolatok tooEvent központi SSL/TLS használatával |

## <a id="priv-aspnet"></a>Ellenőrizze a jogosultságokat, és ellenőrizze, hogy egyéni szolgáltatások vagy az ASP.NET-lapok hello tiszteletben tartják CRM biztonsági szolgáltatásfiók

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Dynamics CRM | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Ellenőrizze a jogosultságokat, és ellenőrizze, hogy egyéni szolgáltatások vagy az ASP.NET-lapok hello tiszteletben tartják CRM biztonsági szolgáltatásfiók |

## <a id="sqlserver-factory"></a>Csatlakozás a helyi SQL Server tooAzure adat-előállító közben az adatkezelési átjáró használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Data Factory | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Kapcsolódószolgáltatás-típusok - Azure és a helyi |
| **Hivatkozások**              |[Adatok áthelyezése másik helyszíni és az Azure Data Factory](https://azure.microsoft.com/documentation/articles/data-factory-move-data-between-onprem-and-cloud/#create-gateway), [az adatkezelési átjáró](https://azure.microsoft.com/documentation/articles/data-factory-data-management-gateway/) |
| **Lépések** | <p>hello adatok felügyeleti átjáró (DMG) eszköze szükséges tooconnect toodata források corpnet vagy egy tűzfal mögött található védett.</p><ol><li>Hello gép zárolása elkülöníti hello DMG eszköz, és megakadályozza, hogy hibás programok káros vagy megfigyelő hello adatok forrásgépen. (Pl. telepíteni kell a legújabb frissítéseket, minimális engedélyezése szükséges portok ellenőrzött fiókok átadásához, a naplózás engedélyezve van, lemez engedélyezhető a titkosítás stb.)</li><li>Adatok átjárókulcs kell forgatni rendszeres időközönként, vagy amikor megújítja hello DMG szolgáltatásfiók jelszavát</li><li>Adatok továbbítására hivatkozás szolgáltatáson keresztül titkosítva kell lennie</li></ol> |

## <a id="identity-https"></a>Győződjön meg arról, hogy az összes forgalom tooIdentity kiszolgáló HTTPS-kapcsolaton keresztül

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Identity Serverben | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [IdentityServer3 - kulcsokat, aláírás és titkosítás](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html), [IdentityServer3 - telepítés](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Lépések** | Alapértelmezés szerint a IdentityServer minden bejövő kapcsolatok toocome igényel a HTTPS-KAPCSOLATON keresztül. Feltétlenül kötelező, hogy IdentityServer kommunikáció csak biztonságos átvitelnél keresztül hajtja végre. Nincsenek bizonyos telepítési helyzetekben, például az SSL-feladatkiszervezést, ahol ezt a követelményt mérsékelhető. Hello Identity Serverben az hello hivatkozások további információt a központi telepítési oldal jelenik meg. |

## <a id="x509-ssltls"></a>X.509-tanúsítványokat használ, tooauthenticate SSL, a TLS és DTLS kapcsolatok ellenőrzése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>SSL, a TLS és DTLS használó alkalmazások teljesen ellenőrizni kell a hello X.509-tanúsítványokat hello entitások való csatlakozáshoz. Ez magában foglalja az hello tanúsítványok ellenőrzése:</p><ul><li>Tartománynév</li><li>Érvényességi dátumok (kezdő és a lejárati dátum)</li><li>Visszavonási állapotát</li><li>Használat (például kiszolgálóhitelesítéshez kiszolgálók, ügyfelek ügyfél-hitelesítés)</li><li>Megbízhatósági láncában. Tanúsítványok tooa legfelső szintű hitelesítésszolgáltató (CA) megbízhatónak hello platform vagy explicit módon hello rendszergazda által beállított programhoz</li><li>A tanúsítvány nyilvános kulcsa kulcshossza > 2048 bit</li><li>Kivonatoló algoritmus kell lennie az SHA256 vagy újabb verzió |

## <a id="ssl-appservice"></a>Az egyéni tartomány SSL tanúsítvány konfigurálása az Azure App Service-ben

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | EnvironmentType – Azure |
| **Hivatkozások**              | [HTTPS engedélyezése az alkalmazásoknak az Azure App Service-ben](https://azure.microsoft.com/documentation/articles/web-sites-configure-ssl-certificate/) |
| **Lépések** | Alapértelmezés szerint Azure már lehetővé teszi, hogy HTTPS hello helyettesítő tanúsítványt minden alkalmazást a *. azurewebsites.net tartományban. Azonban minden helyettesítő karakteres tartományok, például nincs olyan biztonságos, mint az egyéni tartománynév használatával saját tanúsítvánnyal [olvassa el a](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates/). Ajánlott tooenable SSL hello egyéni tartomány mely hello telepített alkalmazás keresztül kell elérnie|

## <a id="appservice-https"></a>Az összes forgalom tooAzure App Service HTTPS-kapcsolaton keresztül kényszerítése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | EnvironmentType – Azure |
| **Hivatkozások**              | [Az Azure App Service HTTPS kényszerítése] https://azure.microsoft.com/documentation/articles/web-sites-configure-ssl-certificate/#4-enforce-https-on-your-app) |
| **Lépések** | <p>Bár az Azure már lehetővé teszi, hogy HTTPS az Azure app Service szolgáltatások helyettesítő tanúsítványt hello tartomány *. azurewebsites.net, akkor kényszeríti ki a HTTPS. Látogatók hello alkalmazás HTTP-n, veszélyeztethetik a hello alkalmazás biztonsági keresztül is hozzáférhetnek, és ezért HTTPS van-e kényszerítve explicit módon toobe. ASP.NET MVC alkalmazások használhatják hello [RequireHttps szűrő](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) , amely előírja, hogy egy nem biztonságos HTTP kérelem toobe újra küldött HTTPS-KAPCSOLATON keresztül.</p><p>Másik lehetőségként a hello URL-újraíró modult, amely tartalmazza az Azure App Service használt tooenforce HTTPS lehet. URL-újraíró modult lehetővé teszi, hogy a fejlesztők toodefine szabályokat alkalmazott tooincoming kérelmek előtt hello kérelmek tooyour alkalmazás kell adni. URL-újraíró szabályok vannak meghatározva, a web.config fájlban tárolt hello hello alkalmazás gyökérkönyvtárában</p>|

### <a name="example"></a>Példa
hello alábbi példa alap URL-újraíró szabályt tartalmaz, amely minden bejövő forgalom toouse HTTPS kényszeríti
```XML
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```
Ez a szabály működik vissza egy HTTP-állapotkód 301 (Állandó átirányítás), ha hello felhasználói kísérel meg HTTP lapot. hello 301 átirányítások hello kérelem toohello hello látogató URL-CÍMÉRE kért, de cserél hello HTTP hello kérelmet HTTPS része. Például a HTTP://contoso.com átirányított tooHTTPS://contoso.com lenne. 

## <a id="http-hsts"></a>HTTP szigorú a Transport Security (HSTS) engedélyezése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [OWASP HTTP szigorú a Transport Security Cheat lap](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) |
| **Lépések** | <p>HTTP szigorú átviteli biztonsági (HSTS) egy különös válaszfejléc hello használata egy webalkalmazás által megadott opt-in biztonsági továbbfejlesztése. Ha egy támogatott böngésző megkapja ezt a fejlécet, hogy a böngésző megakadályozza, hogy minden kommunikáció HTTP toohello megadott tartomány keresztül küldött, és helyette küldi a kommunikáció minden esetben HTTPS PROTOKOLLON keresztül. Megakadályozza a HTTPS kattintson Rákérdezés a böngészők keresztül is.</p><p>tooimplement HSTS, a következő válaszfejléc hello konfigurálni egy webhely globálisan, vagy a kód vagy a konfigurációs toobe rendelkezik. Strict-átviteli-biztonsági: maximális-életkora = 300; includeSubDomains HSTS hello fenyegetések a következő címet:</p><ul><li>Felhasználó könyvjelzők vagy manuálisan http://example.com meg kell adnia, és tulajdonos tooa-átjárójának támadó: HSTS automatikusan átirányítja a HTTP-kérelmek tooHTTPS hello cél tartomány</li><li>Webes alkalmazás, amely kizárólag HTTPS véletlenül HTTP hivatkozásokat tartalmaz, vagy HTTP-kapcsolaton keresztül tartalmat szolgál tervezett toobe: HSTS automatikusan átirányítja a HTTP-kérelmek tooHTTPS hello cél tartomány</li><li>Egy-átjárójának támadó megpróbál toointercept forgalom áldozata a felhasználó érvénytelen tanúsítványt használ, és reméli hello felhasználói elfogadja hello hibás tanúsítvány: HSTS nem engedélyezi a felhasználó toooverride érvénytelen tanúsítvány köszönőüzenetei</li></ul>|

## <a id="sqlserver-validation"></a>Ellenőrizze az SQL server-kapcsolat titkosítási és a tanúsítvány érvényesítése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | SQL Azure  |
| **Attribútumok**              | SQL-verzió - 12-es verzió |
| **Hivatkozások**              | [Gyakorlati tanácsok írása a biztonságos kapcsolati karakterláncok SQL-adatbázis](http://social.technet.microsoft.com/wiki/contents/articles/2951.windows-azure-sql-database-connection-security.aspx#best) |
| **Lépések** | <p>SQL-adatbázis és az ügyfél alkalmazás közötti minden kommunikáció titkosítása mindig Secure Sockets Layer (SSL) használatával. SQL-adatbázis nem támogatja a titkosított kapcsolatokat. toovalidate tanúsítványait, az alkalmazás kódja vagy eszközök, explicit módon titkosított kapcsolat kérése és hello kiszolgálói tanúsítványok nem megbízható. Az alkalmazás kódja vagy az eszközök nem kérő titkosított kapcsolatot, ha azok még megérkeznek a titkosított kapcsolatokat</p><p>Azonban ezek nem lehet érvényesíteni a hello kiszolgálói tanúsítványok, és így lesz téve túl "hello középső man" támadások. toovalidate tanúsítványait az ADO.NET alkalmazáskód, állítsa be `Encrypt=True` és `TrustServerCertificate=False` hello adatbázis kapcsolati karakterláncban. SQL Server Management Studio segítségével toovalidate tanúsítványok hello Connect tooServer párbeszédpanel megnyitásához. Kattintson a kapcsolat titkosítása hello kapcsolat tulajdonságai lap</p>|

## <a id="encrypted-sqlserver"></a>Titkosított kommunikáció tooSQL kiszolgáló kényszerítése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | A helyi üzemeltetésű |
| **Attribútumok**              | SQL verzió - MsSQL2016, SQL - MsSQL2012, SQL verzió - verzió MsSQL2014 |
| **Hivatkozások**              | [Titkosított kapcsolatok toohello adatbázismotor engedélyezése](https://msdn.microsoft.com/library/ms191192)  |
| **Lépések** | SSL engedélyezése titkosítás növeli hello biztonsági hálózatokon, SQL Server-példány és az alkalmazások között továbbított adatok. |

## <a id="comm-storage"></a>Győződjön meg arról, hogy kommunikációs tooAzure tároló van HTTPS-KAPCSOLATON keresztül

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Storage | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Az Azure Storage átviteli szintű titkosítást – HTTPS-kapcsolaton keresztül](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_encryption-in-transit) |
| **Lépések** | az Azure Storage adatok az átvitel, tooensure hello biztonsági mindig hello HTTPS protokollt használják, amikor tárolási objektumokat hello REST API-k hívása vagy eléréséhez. Is a megosztott hozzáférési aláírásokkal, amely lehet használt toodelegate tooAzure tárolási objektum eléréséhez, közé tartozik egy beállítás toospecify, hogy csak HTTPS protokoll használható a megosztott hozzáférési aláírásokkal, győződjön meg arról, hogy birtokában bárki küldi ki az SAS-tokenje hivatkozások fog használatakor hello hello megfelelő protokollt használja.|

## <a id="md5-https"></a>MD5 kivonatoló érvényesítése után blob letöltése, ha HTTPS nem engedélyezhető.

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Storage | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | StorageType - Blob |
| **Hivatkozások**              | [A Windows Azure-blobot MD5 áttekintése](https://blogs.msdn.microsoft.com/windowsazurestorage/2011/02/17/windows-azure-blob-md5-overview/) |
| **Lépések** | <p>Windows Azure Blob szolgáltatás mechanizmusok tooensure adatintegritást egyaránt hello alkalmazás és a szállítási réteg biztosít. Ha bármilyen okból HTTP helyett HTTPS, és dolgozunk a blokkblobokhoz toouse van szüksége, használhatja az MD5 ellenőrzése toohelp ellenőrizni hello hello blobok átvitele</p><p>Ez segít hálózati transport layer hibák elleni védelem, de nem feltétlenül közvetítő támadások. Ha a HTTPS-t, amely átvitelszintű biztonság, akkor használja, MD5 ellenőrzése redundáns és szükségtelen is használhatja.</p>|

## <a id="smb-shares"></a>Használja az SMB 3.0 kompatibilis ügyfél tooensure adatok az átvitel közbeni titkosítás tooAzure fájlmegosztások

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Mobileszköz ügyfél | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | StorageType - fájl |
| **Hivatkozások**              | [Az Azure File Storage](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/#comment-2529238931), [az Azure File Storage SMB támogatása a Windows-ügyfelek](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files/#_mount-the-file-share) |
| **Lépések** | Az Azure File Storage REST API hello használata esetén támogatja a HTTPS PROTOKOLLT, de gyakrabban használt SMB-fájlmegosztás tooa VM hozzá van kapcsolva. SMB 2.1 nem támogatja a titkosítást, tehát kapcsolatok csak engedélyezett hello belül azonos Azure-régiót. Azonban az SMB 3.0 támogatja a titkosítást, és használható a Windows Server 2012 R2, Windows 8, Windows 8.1 és Windows 10, lehetővé téve a kereszt-régió hozzáférni, sőt akár hello asztalon hozzáférés. |

## <a id="cert-pinning"></a>Tanúsítvány rögzítését megvalósítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Storage | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, Windows Phone |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [A tanúsítvány és nyilvános kulcs rögzítése](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning#.Net) |
| **Lépések** | <p>Tanúsítvány rögzítését defends-átjárójának (MITM) támadások ellen. Rögzítési az állomás társít a várt X509 hello folyamat tanúsítvány vagy nyilvános kulcs. Miután a tanúsítvány nyilvános kulcs ismert vagy állomás látható, hello tanúsítvány vagy nyilvános kulcs nem társított vagy "rögzített" toohello állomás. </p><p>Így amikor egy ellenfél próbál SSL MITM támadás során SSL kézfogási hello kulcs támadó kiszolgálótól eltérő lesz toodo hello rögzítve tanúsítvány kulcsa, és hello kérelmet a rendszer elveti, megelőzve MITM tanúsítvány rögzítését érhető el a ServicePointManager a végrehajtási `ServerCertificateValidationCallback` delegálni.</p>|

### <a name="example"></a>Példa
```C#
using System;
using System.Net;
using System.Net.Security;
using System.Security.Cryptography;

namespace CertificatePinningExample
{
    class CertificatePinningExample
    {
        /* Note: In this example, we're hardcoding a hello certificate's public key and algorithm for 
           demonstration purposes. In a real-world application, this should be stored in a secure
           configuration area that can be updated as needed. */

        private static readonly string PINNED_ALGORITHM = "RSA";

        private static readonly string PINNED_PUBLIC_KEY = "3082010A0282010100B0E75B7CBE56D31658EF79B3A1" +
            "294D506A88DFCDD603F6EF15E7F5BCBDF32291EC50B2B82BA158E905FE6A83EE044A48258B07FAC3D6356AF09B2" +
            "3EDAB15D00507B70DB08DB9A20C7D1201417B3071A346D663A241061C151B6EC5B5B4ECCCDCDBEA24F051962809" +
            "FEC499BF2D093C06E3BDA7D0BB83CDC1C2C6660B8ECB2EA30A685ADE2DC83C88314010FFC7F4F0F895EDDBE5C02" +
            "ABF78E50B708E0A0EB984A9AA536BCE61A0C31DB95425C6FEE5A564B158EE7C4F0693C439AE010EF83CA8155750" +
            "09B17537C29F86071E5DD8CA50EBD8A409494F479B07574D83EDCE6F68A8F7D40447471D05BC3F5EAD7862FA748" +
            "EA3C92A60A128344B1CEF7A0B0D94E50203010001";


        public static void Main(string[] args)
        {
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("https://azure.microsoft.com");
            request.ServerCertificateValidationCallback = (sender, certificate, chain, sslPolicyErrors) =>
            {
                if (certificate == null || sslPolicyErrors != SslPolicyErrors.None)
                {
                    // Error getting certificate or hello certificate failed basic validation
                    return false;
                }

                var targetKeyAlgorithm = new Oid(certificate.GetKeyAlgorithm()).FriendlyName;
                var targetPublicKey = certificate.GetPublicKeyString();
                
                if (targetKeyAlgorithm == PINNED_ALGORITHM &&
                    targetPublicKey == PINNED_PUBLIC_KEY)
                {
                    // Success, hello certificate matches hello pinned value.
                    return true;
                }
                // Reject, either hello key or hello algorithm does not match hello expected value.
                return false;
            };

            try
            {
                var response = (HttpWebResponse)request.GetResponse();
                Console.WriteLine($"Success, HTTP status code: {response.StatusCode}");
            }
            catch(Exception ex)
            {
                Console.WriteLine($"Failure, {ex.Message}");
            }
            Console.WriteLine("Press any key tooend.");
            Console.ReadKey();
        }
    }
}
```

## <a id="https-transport"></a>Engedélyezze a HTTPS - az átviteli csatornát

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | NET-keretrendszer 3 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg Királyság](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Lépések** | hello Alkalmazáskonfiguráció győződjön meg arról, hogy a HTTPS PROTOKOLLT használja minden toosensitive adatok eléréséhez.<ul><li>**Magyarázat:** Ha egy alkalmazás kezeli a bizalmas adatokat, és üzenet szintű titkosítást használ, akkor azt csak akkor engedélyezhető toocommunicate egy átviteli titkosított csatornán keresztül.</li><li>**JAVASLATOK:** győződjön meg arról, hogy a HTTP-átvitel le van tiltva, és engedélyezze a HTTPS átviteli protokoll helyett. Helyettesítse be például hello `<httpTransport/>` rendelkező `<httpsTransport/>` címke. Támaszkodjon a hálózati konfiguráció (tűzfal) tooguarantee hello alkalmazás csak elérése egy biztonságos csatornán keresztül. Egy világnézeti szempontjából a biztonság hello hálózati hello alkalmazás nem függ.</li></ul><p>Gyakorlati szempontból hello felelős hello hálózat védelme mindig nyomkövetés tiltása hello alkalmazás hello biztonsági követelményeinek, azok fejleszteni.</p>|

## <a id="message-protection"></a>WCF: Set Message biztonsági védelmi szint tooEncryptAndSign

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | .NET-keretrendszer 3 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff650862.aspx) |
| **Lépések** | <ul><li>**Magyarázat:** Ha védelem szintje túl "none" le fogja tiltani üzenet-védelem. Titkosítás és integritásának megfelelő szintű beállítás érhető el.</li><li>**JAVASLATOK:**<ul><li>Ha `Mode=None` -üzenet védelem letiltása</li><li>Ha `Mode=Sign` -jeleket azonban nem titkosítja a üdvözlőüzenetére; kell használni, amikor az adatok integritásának fontos</li><li>Ha `Mode=EncryptAndSign` -jelentkezik, és titkosítja a köszönőüzenetei</li></ul></li></ul><p>Fontolja meg a titkosítási kikapcsolása, és csak aláíráshoz az üzenetet, ha egyszerűen toovalidate hello integritását bizalmas semmiképp hello információkat. Ez akkor lehet hasznos, a műveletek vagy szolgáltatási szerződések, amelyben toovalidate hello eredeti küldő van szüksége, de nem érzékeny adatátvitel. Hello védelmi szint csökkentését, amikor ügyeljen arra, hogy üdvözlőüzenetére nem tartalmaz személyazonosításra alkalmas adatok (PII).</p>|

### <a name="example"></a>Példa
Hello szolgáltatást és hello művelet tooonly bejelentkezési üdvözlőüzenetére konfigurálása a következő példák hello jelenik meg. Szolgáltatási szerződés példa `ProtectionLevel.Sign`: hello az alábbiakban látható hello szolgáltatási szerződés szinten ProtectionLevel.Sign használatának példája: 
```
[ServiceContract(Protection Level=ProtectionLevel.Sign] 
public interface IService 
  { 
  string GetData(int value); 
  } 
```

### <a name="example"></a>Példa
Művelet szerződés példa `ProtectionLevel.Sign` (a tanúsítványhasználat pontos szabályzása): hello az alábbiakban látható egy példa segítségével `ProtectionLevel.Sign` : hello OperationContract szintje:

```
[OperationContract(ProtectionLevel=ProtectionLevel.Sign] 
string GetData(int value);
``` 

## <a id="least-account-wcf"></a>WCF: Használja a legkevésbé jogosultsági szintű fiók toorun a WCF-szolgáltatás

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | .NET-keretrendszer 3 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648826.aspx ) |
| **Lépések** | <ul><li>**Magyarázat:** admin vagy magas jogosultsági fiókkal WCF-szolgáltatások nem futnak. szolgáltatások biztonsági sérülés esetén nagy jelentőségű eredményez.</li><li>**JAVASLATOK:** egy a WCF szolgáltatást, mert az alkalmazás támadási felület csökkentése, és csökkentheti a hello fellépő potenciális károknak, ha vannak megtámadott legkevésbé jogosultsági szintű fiók toohost használja. Ha hello szolgáltatásfiók infrastruktúra erőforrások, például MSMQ további hozzáférési jogosultságra van szüksége, hello Eseménynapló, teljesítményszámlálók és hello fájlrendszer, megfelelő engedélyeket kell adni toothese erőforrások, hogy futtathatók hello WCF-szolgáltatások sikeresen megtörtént.</li></ul><p>Ha a szolgáltatás tooaccess egy adott erőforráshoz hello eredeti hívó nevében, egy alsóbb rétegbeli jogosultsági ellenőrzés használni megszemélyesítés és a delegálás tooflow hello hívó identitását. A fejlesztési forgatókönyvek hello helyi hálózati szolgáltatásfiók használata, amelyek különleges beépített fiók, amely korlátozott jogosultsággal rendelkezik. Egy éles telepítési forgatókönyvhöz hozzon létre egy egyéni legkevésbé jogosultsági szintű tartományi szolgáltatásfiókot.</p>|

## <a id="webapi-https"></a>Az összes forgalom tooWeb API-k HTTPS-kapcsolaton keresztül kényszerítése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | MVC5, MVC6 |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [A webes API-vezérlőben SSL kényszerítése](http://www.asp.net/web-api/overview/security/working-with-ssl-in-web-api) |
| **Lépések** | Ha egy alkalmazás egy HTTPS- és a HTTP-kötést, az ügyfelek továbbra is használhatja HTTP tooaccess hello helyen. tooprevent a, egy művelet szűrő tooensure tooprotected API-k kérő mindig HTTPS-KAPCSOLATON keresztül is használja.|

### <a name="example"></a>Példa 
hello következő kód bemutatja egy webes API hitelesítési szűrő, amely ellenőrzi az SSL-hez: 
```C#
public class RequireHttpsAttribute : AuthorizationFilterAttribute
{
    public override void OnAuthorization(HttpActionContext actionContext)
    {
        if (actionContext.Request.RequestUri.Scheme != Uri.UriSchemeHttps)
        {
            actionContext.Response = new HttpResponseMessage(System.Net.HttpStatusCode.Forbidden)
            {
                ReasonPhrase = "HTTPS Required"
            };
        }
        else
        {
            base.OnAuthorization(actionContext);
        }
    }
}
```
Adja hozzá a tooany szűrő-Web API SSL igénylő műveleteket: 
```C#
public class ValuesController : ApiController
{
    [RequireHttps]
    public HttpResponseMessage Get() { ... }
}
```
 
## <a id="redis-ssl"></a>Győződjön meg arról, hogy kommunikációs tooAzure Redis Cache van SSL-en keresztül

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Redis Cache | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Azure Redis SSL-támogatás](https://azure.microsoft.com/documentation/articles/cache-faq/#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis) |
| **Lépések** | Redis kiszolgáló nem támogatja az SSL hello kezdő verzióról, azonban az Azure Redis Cache nem. Ha tooAzure Redis Cache csatlakozik, és az ügyfél támogatja az SSL, például a StackExchange.Redis, akkor SSL kell használnia. Nem SSL port az új Azure Redis Cache példány alapértelmezés szerint le van tiltva. Győződjön meg arról, hogy hello biztonságos alapértelmezett értéke, nem módosulnak kivéve, ha egy függőséget az SSL redis-ügyfelek támogatása. |

Ne feledje, hogy a Redis a tervezett toobe megbízható ügyfelek megbízható környezetek belül érhető el. Ez azt jelenti, hogy általában nem jó ötlet tooexpose hello Redis példány közvetlenül toohello internet vagy általában tooan környezetben, ahol az ügyfelek nem megbízható közvetlenül hozzáférhet hello Redis TCP-port vagy a UNIX szoftvercsatorna. 

## <a id="device-field"></a>Eszköz tooField átjáró kommunikáció biztosításához

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT-mező átjáró | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | IP-alapú eszközök esetén hello kommunikációs protokollt általában sikerült kell beágyazva egy SSL/TLS-csatorna tooprotect adatokat átvitel közben. Egyéb protokollok, amelyek nem támogatják az SSL/TLS vizsgálja meg, hogy hello protokoll, amely biztonsági transport vagy üzenet rétegben biztonságos verziója van. |

## <a id="device-cloud"></a>Eszköz tooCloud átjáró kommunikációt SSL/TLS biztonságos

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT átjáró | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Válassza ki a kommunikációs protokollja](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#messaging) |
| **Lépések** | Használja az SSL/TLS biztonságos HTTP/AMQP vagy MQTT protokollokat. |
