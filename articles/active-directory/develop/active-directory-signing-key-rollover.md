---
title: "az Azure AD-kulcs átfordulási aaaSigning |} Microsoft Docs"
description: "Ez a cikk ismerteti, amelyek hello aláírási kulcs átfordulási gyakorlati tanácsok az Azure Active Directory"
services: active-directory
documentationcenter: .net
author: dstrockis
manager: krassk
editor: 
ms.assetid: ed964056-0723-42fe-bb69-e57323b9407f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ac6ade7f3ba2fbd22ea6d447aa5d07a2d6bdd451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="signing-key-rollover-in-azure-active-directory"></a>Az Azure Active Directoryban kulcsváltás aláírása
Ez a témakör ismerteti, mire van szüksége az Azure Active Directory (Azure AD) toosign biztonsági jogkivonatokat használt nyilvános kulcsok hello tooknow. Fontos, hogy rendszeres időközönként, és vészhelyzet esetén a kulcsok helyettesítő volt állítva keresztül azonnal toonote. Minden alkalmazás, amely használhatja az Azure Active Directory legyen képes tooprogrammatically leíró hello kulcsváltás folyamat vagy rendszeres manuális helyettesítő-folyamatot. Továbbra is toounderstand olvasása, hogyan hello kulcsok működik, hogyan tooassess hello hello helyettesítő tooyour alkalmazás hatását, és hogyan tooupdate az alkalmazás vagy egy rendszeres manuális váltása folyamat toohandle kulcsváltás létesíteni, ha szükséges.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Az Azure AD-aláírókulcsok áttekintése
Az Azure AD ipari szabványok tooestablish megbízhatósági és hello között épülő nyilvános kulcsú titkosítást használ, az azt használó alkalmazások. Gyakorlatilag, ez hello a következő módon működik: az Azure AD nyilvános és titkos kulcsból álló kulcspárt tartalmazó aláíró kulcsot használ. Tooan alkalmazás az Azure AD használ a hitelesítéshez a felhasználó bejelentkezik, az Azure AD létrehoz egy biztonsági jogkivonatot, amely hello felhasználói adatait tartalmazza. Ez a token használatával annak titkos kulcsát, hátsó toohello alkalmazás elküldés előtt az Azure AD által aláírt. amely a hello token tooverify érvényes és az Azure AD ténylegesen kezdeményezésű, hello alkalmazásnak ellenőriznie kell a nyilvános kulcsával. hello hello bérlői lévő az Azure AD által elérhetővé tett hello jogkivonat aláírása [OpenID Connect felderítési dokumentum](http://openid.net/specs/openid-connect-discovery-1_0.html) vagy SAML/WS-Fed [összevonási metaadat-dokumentum](active-directory-federation-metadata.md).

Biztonsági okokból az Azure AD aláírási kulcs összesíti rendszeres időközönként és hello esetben egy ideiglenes volt állítva azonnal. Bármely alkalmazás, amely az Azure AD egy kulcsváltás esemény nem számít, hogy milyen gyakran előfordulhat toohandle kell készíteni. Ha nem, és a kísérletet tesz egy lejárt kulcs tooverify hello aláírás a jogkivonat toouse, hello bejelentkezési kérelme sikertelen lesz.

Nincs mindig egynél több érvényes kulcs hello OpenID Connect felderítési dokumentum és hello összevonási metaadat-dokumentum. Az alkalmazás kell készíteni a toouse hello kulcsok megadott hello dokumentum, mivel előfordulhat, hogy később is visszalátogatnia, állítva egy kulcs egy másik előfordulhat, hogy azok helyettesítéseivel, és így tovább.

## <a name="how-tooassess-if-your-application-will-be-affected-and-what-toodo-about-it"></a>Hogyan tooassess, ha az alkalmazás érinti, és milyen toodo információk
Például az alkalmazás vagy milyen identitás protokoll és a könyvtár használt hello típusú változók függ, hogy miként kezeli az alkalmazás a kulcsváltás. az alábbi hello szakaszok ellenőrzéséhez, hogy hello leggyakoribb alkalmazások hello kulcsváltás által érintett, és hogyan tooupdate alkalmazás toosupport automatikus helyettesítő hello, vagy manuálisan frissítse a hello kulcs útmutatást nyújtanak.

* [Natív ügyfélalkalmazások erőforrások elérése](#nativeclient)
* [Webalkalmazások / API-k erőforrások elérése](#webclient)
* [Webalkalmazások / API-k erőforrások védelméről, és segítségével az Azure App Service szolgáltatások](#appservices)
* [Webalkalmazások / API-k használata a .NET OWIN OpenID Connect, WS-Fed vagy WindowsAzureActiveDirectoryBearerAuthentication köztes erőforrások védelme](#owin)
* [Webalkalmazások / API-k használata a .NET Core OpenID Connect vagy JwtBearerAuthentication köztes erőforrások védelme](#owincore)
* [Webalkalmazások / API-k Node.js passport-azure-ad-modullal erőforrások védelme](#passport)
* [Webalkalmazások / API-k erőforrások védelméről, és a Visual Studio 2015-öt vagy a Visual Studio 2017](#vs2015)
* [Webalkalmazások erőforrások védelméről, és a Visual Studio 2013 létre](#vs2013)
* [Webes API-k erőforrások védelméről, és a Visual Studio 2013 létre](#vs2013_webapi)
* [Erőforrások védelme és a Visual Studio 2012 létrehozott webes alkalmazások](#vs2012)
* [Webalkalmazások erőforrások védelméről, és a Visual Studio 2010, a Windows Identity Foundation használatával 2008 o létre](#vs2010)
* [Webalkalmazások / API-k erőforrások védelme bármely más szalagtárak, vagy manuálisan végrehajtási bármelyik hello támogatott protokollok](#other)

Ez az útmutató **nem** alkalmazható:

* Az Azure AD Application Gallery (beleértve az egyéni) hozzáadott alkalmazások rendelkeznek tanúsítványinformációit toosigning kulcsok külön segítséget. [További információt.](../active-directory-sso-certs.md)
* A helyszíni alkalmazásproxy keresztül közzétett alkalmazás nem rendelkezik tooworry információ az aláíró kulcsok.

### <a name="nativeclient"></a>Natív ügyfélalkalmazások erőforrások elérése
Erőforrások (azaz csak elérő alkalmazások A Microsoft Graph, KeyVault, Outlook API és más Microsoft APIs) általában csak jogkivonat beszerzése, és adja át mentén toohello erőforrás tulajdonosa. Figyelembe véve, hogy azok nem védett erőforrások, nem vizsgálja meg a hello jogkivonatot, és emiatt nem kell a megfelelő aláírás tooensure.

Natív ügyfélalkalmazások, asztali vagy hordozható, ebbe a kategóriába tartozik, és így nem érinti hello kulcsváltás.

### <a name="webclient"></a>Webalkalmazások / API-k erőforrások elérése
Erőforrások (azaz csak elérő alkalmazások A Microsoft Graph, KeyVault, Outlook API és más Microsoft APIs) általában csak jogkivonat beszerzése, és adja át mentén toohello erőforrás tulajdonosa. Figyelembe véve, hogy azok nem védett erőforrások, nem vizsgálja meg a hello jogkivonatot, és emiatt nem kell a megfelelő aláírás tooensure.

Webalkalmazások és webes API-khoz hello csak alkalmazás folyamat által használt (ügyfél-hitelesítő adatok / ügyféltanúsítvány), ebbe a kategóriába tartozik, és így nem érinti hello helyettesítő.

### <a name="appservices"></a>Webalkalmazások / API-k erőforrások védelméről, és segítségével az Azure App Service szolgáltatások
Az Azure App Services hitelesítési / engedélyezési (EasyAuth) funkció már hello szükséges logika toohandle kulcsváltás automatikusan.

### <a name="owin"></a>Webalkalmazások / API-k használata a .NET OWIN OpenID Connect, WS-Fed vagy WindowsAzureActiveDirectoryBearerAuthentication köztes erőforrások védelme
Ha az alkalmazás által használt .NET OWIN OpenID Connect, WS-Fed vagy WindowsAzureActiveDirectoryBearerAuthentication köztes hello már rendelkezik hello szükséges logika toohandle kulcsváltás automatikusan.

Úgy ellenőrizheti, hogy az alkalmazás ezek az alábbi kódtöredékek az alkalmazás Startup.cs vagy Startup.Auth.cs hello bármelyike használ

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
     });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Webalkalmazások / API-k használata a .NET Core OpenID Connect vagy JwtBearerAuthentication köztes erőforrások védelme
Ha az alkalmazás nem használja a .NET Core OWIN OpenID Connect hello vagy JwtBearerAuthentication köztes, már rendelkezik hello szükséges logika toohandle kulcsváltás automatikusan.

Úgy ellenőrizheti, hogy az alkalmazás ezek az alábbi kódtöredékek az alkalmazás Startup.cs vagy Startup.Auth.cs hello bármelyike használ

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
     });
```

### <a name="passport"></a>Webalkalmazások / API-k Node.js passport-azure-ad-modullal erőforrások védelme
Az alkalmazás hello Node.js passport-ad-modullal használ, ha már rendelkezik hello szükséges logika toohandle kulcsváltás automatikusan.

Úgy ellenőrizheti, hogy az alkalmazás passport-ad a következő kódrészletet a az alkalmazás app.js hello keresése

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Webalkalmazások / API-k erőforrások védelméről, és a Visual Studio 2015-öt vagy a Visual Studio 2017
Ha az alkalmazást a Visual Studio 2015-öt vagy a Visual Studio 2017 webes alkalmazás sablon használatával lett létrehozva, és a kiválasztott **munkahelyi és iskolai fiókok** a hello **hitelesítés módosítása** menü azt már hello szükséges logika toohandle kulcsváltás automatikusan rendelkezik. A programot, hello OWIN OpenID Connect köztes ágyazott kéri le, és gyorsítótárba helyezi azt a hello OpenID Connect felderítési dokumentum hello kulcsokat, és rendszeresen frissülnek.

Ha manuálisan adta hozzá a hitelesítési tooyour megoldás, az alkalmazás esetleg nincs hello szükséges kulcsváltás logikát. Szüksége lesz a toowrite azt saját magára vagy kövesse hello szükséges lépések [webalkalmazások / API-k bármely más könyvtárak segítségével, vagy manuálisan végrehajtási bármelyik hello támogatott protokollok.](#other).

### <a name="vs2013"></a>Webalkalmazások erőforrások védelméről, és a Visual Studio 2013 létre
Ha az alkalmazás a Visual Studio 2013 webes alkalmazás sablon használatával lett létrehozva, és a kiválasztott **munkahelyi és iskolai fiókok** a hello **hitelesítés módosítása** menü már hello szükséges logikai toohandle automatikusan kulcs átfordulási. Ezt a szervezet egyedi azonosítóját és a két adatbázis tábla hello projekttel kapcsolatos legfontosabb tudnivalókat aláíró hello tárolja. Hello kapcsolati karakterlánc hello adatbázis hello projekt Web.config fájlban található.

Ha manuálisan adta hozzá a hitelesítési tooyour megoldás, az alkalmazás esetleg nincs hello szükséges kulcsváltás logikát. Szüksége lesz a toowrite azt saját magára vagy kövesse hello szükséges lépések [webalkalmazások / API-k bármely más könyvtárak segítségével, vagy manuálisan végrehajtási bármelyik hello támogatott protokollok.](#other).

a lépéseket követve hello segítségével ellenőrizze, hogy hello programot az alkalmazás megfelelően működik.

1. A Visual Studio 2013, nyissa meg hello megoldást, és kattintson a hello **Server Explorer** hello jobb oldali lapján.
2. Bontsa ki a **adatkapcsolatok**, **DefaultConnection**, majd **táblák**. Keresse meg a hello **IssuingAuthorityKeys** tábla, kattintson a jobb gombbal, és kattintson a **tábla adatok megjelenítése**.
3. A hello **IssuingAuthorityKeys** table, legalább egy sort, hello kulcs toohello ujjlenyomat értékét megfelelő lesz. Hello tábla sorokat törölni.
4. Kattintson a jobb gombbal hello **bérlők** tábla, és kattintson a **tábla adatok megjelenítése**.
5. A hello **bérlők** table, legalább egy sort, amely megfelel a tooa egyedi directory bérlő azonosítója lesz. Hello tábla sorokat törölni. Ha mindkét hello hello sorok nem töröljük **bérlők** tábla és **IssuingAuthorityKeys** tábla, akkor hibaüzenet fog futásidőben.
6. Hozza létre és hello alkalmazás futtatásához. Miután tooyour fiók már bejelentkezett, le is hello alkalmazás.
7. Térjen vissza a toohello **Server Explorer** , és tekintse meg a hello hello értékek **IssuingAuthorityKeys** és **bérlők** tábla. Láthatja, hogy azok rendelkeznek lett automatikusan tölteni hello összevonási metaadatok dokumentumból hello megfelelő információkkal.

### <a name="vs2013"></a>Webes API-k erőforrások védelméről, és a Visual Studio 2013 létre
Ha a webes API-alkalmazás létrehozása a Visual Studio 2013 hello webes API-sablonnal, és akkor kiválasztva **munkahelyi és iskolai fiókok** a hello **hitelesítés módosítása** menü, akkor már rendelkezik hello az alkalmazás lévő szükséges logika.

Ha manuálisan konfigurált hitelesítési, lépésekkel hello alatt toolearn hogyan tooconfigure a Web API tooautomatically frissíteni az kulcs adatait.

hello következő kódrészletet bemutatja, hogyan tooget hello hello összevonási metaadat-dokumentum legújabb kulcsokat, és a hello [JWT jogkivonat-kezelő](https://msdn.microsoft.com/library/dn205065.aspx) toovalidate hello jogkivonat. hello kódrészletet feltételezi, hogy az Azure AD-ről saját tárolásakor hello kulcs toovalidate jövőbeli mechanizmus gyorsítótárazás jogkivonatok használt legyenek az adatbázis, a konfigurációs fájlban vagy máshol.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates hello JWT Token that's part of hello Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in hello Azure Classic Portal]",
                ValidIssuer = "[hello issuer for hello token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache hello signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from hello specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in hello metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Erőforrások védelme és a Visual Studio 2012 létrehozott webes alkalmazások
Ha az alkalmazást a Visual Studio 2012 lett létrehozva, akkor valószínűleg használt hello identitás és hozzáférés eszköz tooconfigure az alkalmazás. Is valószínű, hogy azok be hello [ellenőrzése kibocsátó neve beállításjegyzék (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). hello VINR karbantartásáért felelős megbízható identitás-szolgáltatóktól (az Azure AD) kapcsolatos információkat, és hello kulcsait használja az általuk kiállított toovalidate jogkivonatokat. hello VINR is teszi, hogy könnyen tooautomatically frissítés hello kulcsadatokat hello legújabb összevonási metaadatok társított dokumentum a címtárban, ha hello konfiguráció legújabb elavult hello ellenőrzése a letöltés a Web.config fájlban tárolt a dokumentum, és frissítési hello alkalmazás toouse hello új kulcs szükséges.

Ha bármely hello mintakódok vagy a Microsoft által biztosított forgatókönyv dokumentáció segítségével az alkalmazás hozott létre, hello kulcsváltás logika már szerepel a projektben. Megfigyelheti, hogy hello kódot a projekt már létezik. Ha az alkalmazás már nincs a logikai, lépésekkel hello tooadd alatt, és megfelelően működik tooverify.

1. A **Megoldáskezelőben**, adja hozzá egy hivatkozást toohello **System.IdentityModel** hello megfelelő projekt szerelvény.
2. Nyissa meg hello **Global.asax.cs** fájlt, és adja hozzá a következő hello irányelvek segítségével:
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. Adja hozzá a következő metódus toohello hello **Global.asax.cs** fájlt:
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. Hello meghívása **RefreshValidationSettings()** metódus a hello **Application_Start()** metódus a **Global.asax.cs** látható módon:
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

Ha követte ezeket a lépéseket, az alkalmazás Web.config dokumentumból hello összevonási metaadatok, többek között a legújabb kulcsok hello hello legújabb adatokkal frissül. A frissítés történik, minden alkalommal, amikor az alkalmazáskészlet újraindul, az IIS-ben; az IIS alapértelmezés szerint értéke toorecycle alkalmazások 29 óránként.

Kövesse az alábbi tooverify, hogy működik-e hello kulcsváltás logika hello lépéseket.

1. Miután ellenőrizte, hogy az alkalmazás hello nyissa meg a fenti hello olyan helykódot alkalmaz **Web.config** fájlt, és keresse meg a toohello  **<issuerNameRegistry>**  blokk, néhány sor a következő hello kifejezetten keresése:
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. A hello  **<add thumbprint=””>**  bármely karakter helyett egy másikat a beállításban módosítsa az hello ujjlenyomat értékét. Mentse a hello **Web.config** fájlt.
3. Hello alkalmazás létrehozása, és futtassa azt. Ha hello bejelentkezési folyamat elvégzése az alkalmazás sikeresen frissíti a hello kulcs által hello szükséges adatok letöltése a címtár összevonási metaadatok dokumentumból. Ha a probléma jelentkezik be, győződjön meg arról, hello módosítások az alkalmazás helyesek hello olvasásával [hozzáadása bejelentkezés tooYour webes alkalmazás használatával Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) a témakörben vagy letöltése, és tanulmányozza a következő példakód hello: [ Az Azure Active Directory több-Bérlős felhőalapú alkalmazásnál](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).

### <a name="vs2010"></a>Erőforrások védelme és a Visual Studio 2008 vagy 2010 webes alkalmazások és a Windows Identity Foundation (WIF) 1.0-s a .NET 3.5
Ha egy alkalmazás WIF 1.0-s verziója található, nincs nincs megadott mechanizmus tooautomatically frissítése az alkalmazás konfigurációs toouse egy új kulcsot.

* *Legegyszerűbben* hello FedUtil tooling szereplő hello WIF SDK-t, így hello legújabb metaadat-dokumentum beolvasása és a konfiguráció frissítését használja.
* Frissítse az alkalmazás too.NET 4.5, amely hello WIF hello rendszer névtérben található legújabb verzióját is tartalmazza. Hello segítségével [ellenőrzése kibocsátó neve beállításjegyzék (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) tooperform a hello alkalmazást automatikus frissítéseit.
* Hajtsa végre a manuális váltása hello utasításokat az útmutató végén hello szerint.

Toouse hello FedUtil tooupdate a konfigurációs utasításokat:

1. Győződjön meg arról, hogy rendelkezik-e hello WIF 1.0-s verziója a fejlesztői gépen telepítve van a Visual Studio 2008 vagy 2010 SDK. Is [töltse le innen](https://www.microsoft.com/en-us/download/details.aspx?id=4451) Ha nem telepítette még azt.
2. A Visual Studióban nyissa meg a hello megoldást, kattintson a jobb gombbal a megfelelő projekt hello és válassza a **összevonási metaadatok frissítése**. Ez a beállítás nem érhető el, ha FedUtil és/vagy hello WIF 1.0-s verziójú SDK nem lett telepítve.
3. Hello parancssorból kiválasztása **frissítés** toobegin az összevonási metaadatok frissítése. Ha hozzáférés-toohello server dolgozik, ahol hello alkalmazás tárolása, is használhat FedUtil tartozó [automatikus metaadat-frissítés Feladatütemező](https://msdn.microsoft.com/library/ee517272.aspx).
4. Kattintson a **Befejezés** toocomplete hello frissítési folyamatot.

### <a name="other"></a>Webalkalmazások / API-k erőforrások védelme bármely más szalagtárak, vagy manuálisan végrehajtási bármelyik hello támogatott protokollok
Ha néhány más szalagtár használata, vagy manuálisan megvalósított hello támogatott protokollok, tooreview hello könyvtárban lesz szüksége, vagy a megvalósítás tooensure, amely hello kulcs lekérése folyamatban van a hello OpenID Connect felderítési dokumentum vagy hello összevonási metaadatokat tartalmazó fájl. Ez a módosítás nem vonható toocheck toodo egy keresésre a vagy hello könyvtár kódban bármely hívások tooeither hello OpenID felderítési dokumentum vagy hello összevonási metaadat-dokumentum.

Ha ezek a kulcsok tárolása valahol vagy szoftveresen kötött az alkalmazásban, manuálisan beolvasása hello kulcs és a frissítés, ennek megfelelően szerint hajtsa végre egy manuális váltása hello utasításokat az útmutató végén hello szerint. **Erősen ajánlott, hogy javítható-e az alkalmazás toosupport automatikus helyettesítő** e hello megközelíti vázlat ezen cikk tooavoid jövőbeli megszakításait és a terhelést, hogy az Azure AD növeli az átfordulási ütemben történik, illetve hogy az vészhelyzeti sávon kívüli kulcsváltás.

## <a name="how-tootest-your-application-toodetermine-if-it-will-be-affected"></a>Hogyan tootest az alkalmazás toodetermine, amennyiben ez érinti
Ellenőrizheti, hogy az alkalmazás támogatja az automatikus kulcsváltást hello parancsfájlok letöltése és hello utasításait követve [a GitHub-tárházban.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-tooperform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Hogyan tooperform Ha alkalmazás nem támogatja az automatikus helyettesítő egy manuális váltása
Ha az alkalmazás **nem** támogatja az automatikus helyettesítő, szüksége lesz egy folyamat, amely rendszeresen figyeli az Azure AD által aláíró kulcsok és hajt végre egy manuális váltása tooestablish ennek megfelelően. [A GitHub-tárházban](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) parancsfájlok és útmutatást tartalmaz toodo ez.

