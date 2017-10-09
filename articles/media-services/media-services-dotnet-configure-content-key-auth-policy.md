---
title: "Media Services .NET SDK használatával aaaConfigure tartalomkulcs-hitelesítési házirend |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconfigure egy Media Services .NET SDK használatával tartalomkulcs engedélyezési házirend."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 1a0aedda-5b87-4436-8193-09fc2f14310c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;mingfeiy
ms.openlocfilehash: cfcbc5da9819bcec8b163fef183988a8beff9ed2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>A dinamikus titkosítás: konfigurálja a tartalomkulcs-hitelesítési házirend
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Áttekintés
A Microsoft Azure Media Services lehetővé teszi a toodeliver MPEG-DASH, Smooth Streaming vagy HTTP-Live-Streaming (HLS) streamjeit az Advanced Encryption Standard (AES) (a 128 bites titkosítási kulcsok használatával) vagy [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS azt is lehetővé teszi, hogy a akkor toodeliver DASH-streameket Widevine DRM titkosítva. Mind a PlayReady, mind a Widevine titkosítása hello Common Encryption (ISO/IEC 23001-7 CENC) megadását.

A Media Services is biztosít a **kulcs/Licenctovábbítási szolgáltatása** , amelyekről az ügyfelek AES-kulcsok úgy szerezheti be, vagy a PlayReady vagy Widevine-licencek tooplay hello titkosított tartalmat.

Ha azt szeretné, a Media Services tooencrypt egy eszköz tooassociate titkosítási kulcsot kell (**CommonEncryption** vagy **EnvelopeEncryption**) hello eszközt (leírtak [Itt](media-services-dotnet-create-contentkey.md)) és engedélyezési házirendeket hello kulcs (ebben a cikkben leírtak) is konfigurálhatja.

Ha olyan adatfolyamot kell megadni a Windows Media Player van szükség, Media Services használ-e a megadott hello kulcs toodynamically titkosítása a tartalmat, AES vagy DRM titkosítással történik. toodecrypt hello stream, hello player fog igényelni hello kulcs hello kulcs kézbesítési szolgáltatásból. hello felhasználónak van-e toodecide engedélyezett tooget hello kulcs, hello szolgáltatás értékeli az hello kulcshoz megadott hello engedélyezési házirendeket.

A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat. hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: **nyissa meg a** vagy **token** korlátozás. hello token korlátozott házirend által a Secure Token Service (STS) kiadott tokennek kell csatolni. A Media Services hello támogatja a jogkivonatokat **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) formátumú és **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) formátumban.

A Media Services nem nyújt Secure Token szolgáltatásokat. Hozzon létre egy egyéni STS, vagy használja a Microsoft Azure ACS tooissue jogkivonatokat. hello STS konfigurált toocreate hello megadott kulccsal aláírva jogkivonatot kell lennie, és probléma jogcímek megadott hello token korlátozás konfigurációs (ebben a cikkben leírtak). hello Media Services kulcs kézbesítési szolgáltatás visszaadható hello titkosítási kulcs toohello ügyfél Ha hello jogkivonat érvényes, és hello hello jogkivonatában lévő jogcímeket egyeznek hello tartalomkulcsot konfigurálva.

További információkért lásd:

[JWT jogkivonat hitelesítési](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Azure Media Services OWIN MVC-alapú alkalmazások integrálása az Azure Active Directoryban, és korlátozhatja a JWT jogcímei alapján kulcs tartalomkézbesítési](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Használjon Azure ACS tooissue tokeneket](http://mingfeiy.com/acs-with-key-services).

### <a name="some-considerations-apply"></a>Vegye figyelembe a következőket:
* Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás, az adatfolyam-továbbítási végpontjának tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart toobe rendelkezik hello **futtató** állapotát. 
* Az eszköz tartalmaznia kell egy adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá készletét. További információkért lásd: [kódolása egy eszköz](media-services-encode-asset.md).
* Töltse fel, és használja a kódolásához **AssetCreationOptions.StorageEncrypted** lehetőséget.
* Ha azt tervezi, hogy toohave több tartalomkulcs igénylő hello azonos házirend-konfigurációt is erősen ajánlott toocreate egyetlen engedélyezési házirend, és újra felhasználhatja több tartalomkulcs.
* hello kulcs kézbesítési szolgáltatás 15 percig gyorsítótárazza a ContentKeyAuthorizationPolicy és a kapcsolódó objektumok (házirend-beállítások és korlátozásai).  Ha hozzon létre egy ContentKeyAuthorizationPolicy és toouse "Token" korlátozás, adja meg, majd tesztelje, és frissítse a hello házirend túl "nyissa meg" korlátozás, mielőtt hello kapcsolók toohello "Megnyitás" házirendverzió hello házirend nagyjából 15 percig tart.
* Objektumtovábbítási szabályzat hozzáadásakor vagy módosításakor törölnie kell az ahhoz tartozó meglévő lokátort (ha van), majd létre kell hoznia egy új lokátort.
* Jelenleg nem titkosítható progresszív letöltés.

## <a name="aes-128-dynamic-encryption"></a>AES-128, a dinamikus titkosítás
### <a name="open-restriction"></a>Nyissa meg a szoftverkorlátozó
Nyissa meg a szoftverkorlátozó azt jelenti, hogy a rendszer hello hello kulcs tooanyone kulcs kérést fog továbbítani. Ez a korlátozás tesztelési célokra hasznos lehet.

a következő példa hello létrehoz egy megnyitott engedélyezési szabályzatot, és hozzáadja azt toohello tartalomkulcsot.

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
        // Create ContentKeyAuthorizationPolicy with Open restrictions
        // and create authorization policy
        IContentKeyAuthorizationPolicy policy = _context.
        ContentKeyAuthorizationPolicies.
        CreateAsync("Open Authorization Policy").Result;
        
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };

        restrictions.Add(restriction);

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy tooContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);
    }


### <a name="token-restriction"></a>Token korlátozása
Ez a szakasz ismerteti, hogyan toocreate tartalomra kulcs engedélyezési házirendjét, és rendelje hozzá azt hello tartalomkulcsot. hello engedélyezési házirend írja le, milyen hitelesítési követelmények fennállnak toodetermine kell lennie, ha hello felhasználó jogosult tooreceive hello kulcs (például hello "ellenőrzési kulcsot" lista tartalmaz hello kulcs lett aláírva, adott hello token).

tooconfigure hello token korlátozás beállítást kell toouse XML toodescribe hello token engedélyezési követelményeihez. hello token korlátozás konfigurációs XML-t meg kell felelnie a következő XML-séma toohello.

#### <a id="schema"></a>Token korlátozás séma
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Hello konfigurálásakor **token** korlátozott házirend, meg kell adnia hello elsődleges ** ellenőrző kulcs **, **kibocsátó** és **célközönség** paraméterek. hello ** elsődleges hitelesítési kulcs ** tartalmaz, amely a hello token hello kulcs aláírt, **kibocsátó** hello biztonságos biztonságijogkivonat-szolgáltatás adott problémák hello token van. Hello **célközönség** (más néven **hatókör**) hello leképezés ismerteti hello token vagy hello erőforrás hello token engedélyezi a hozzáférést. hello Media Services kulcs kézbesítési szolgáltatás ellenőrzi, hogy ezek az értékek hello token értékekre, hello hello sablont. 

Használata esetén **Media Services SDK for .NET**, használhatja a hello **TokenRestrictionTemplate** osztály toogenerate hello korlátozás jogkivonat.
a következő példa hello házirendet hoz létre engedélyezési jogkivonat korlátozás. Ebben a példában hello ügyfélnek ehhez már toopresent tartalmazó jogkivonatok: aláírási kulcs (VerificationKey), a jogkivonat-kibocsátó és a szükséges jogcímeket.

    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();

        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;

        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();

        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };

        restrictions.Add(restriction);

        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy tooContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);

        return tokenTemplateString;
    }

    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();

        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

        return TokenRestrictionTemplateSerializer.Serialize(template);
    }

#### <a id="test"></a>Teszt jogkivonat
tooget a teszt jogkivonat hello hello kulcshitelesítési szabályzatban használt jogkivonat korlátozás alapján, a következő hello.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on hello hello data in hello given TokenRestrictionTemplate.
    // Note, you need toopass hello key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

    //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
    //so you have tooadd it in front of hello token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


## <a name="playready-dynamic-encryption"></a>PlayReady dinamikus titkosítás
A Media Services lehetővé teszi tooconfigure hello jogokat és korlátozásokat, amelyet a PlayReady DRM futásidejű tooenforce hello amikor egy felhasználó próbál tooplay vissza védett tartalmakat. 

Amikor a Playreadyvel a tartalmak védelmére, hello egyikét meg kell toospecify a hitelesítési házirend hello definiáló XML-karakterlánc [PlayReady licencsablon](media-services-playready-license-template-overview.md). A Media Services SDK for .NET, hello **PlayReadyLicenseResponseTemplate** és **PlayReadyLicenseTemplate** határozza meg a PlayReady-Licencsablon hello osztályok nyújtanak segítséget.

[Ez a témakör](media-services-protect-with-drm.md) bemutatja, hogyan tooencrypt a tartalmaknak a **PlayReady** és **Widevine**.

### <a name="open-restriction"></a>Nyissa meg a szoftverkorlátozó
Nyissa meg a szoftverkorlátozó azt jelenti, hogy a rendszer hello hello kulcs tooanyone kulcs kérést fog továbbítani. Ez a korlátozás tesztelési célokra hasznos lehet.

a következő példa hello létrehoz egy megnyitott engedélyezési szabályzatot, és hozzáadja azt toohello tartalomkulcsot.

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {

        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          

        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };

        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);

        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;


        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate hello content key authorization policy with hello content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

### <a name="token-restriction"></a>Token korlátozása
tooconfigure hello token korlátozás beállítást kell toouse XML toodescribe hello token engedélyezési követelményeihez. hello token korlátozás konfigurációs XML-t meg kell felelnie a toohello XML-séma látható [ez](#schema) szakasz.

    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();

        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;

        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };

        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);

        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy tooContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate hello content key authorization policy with hello content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;

        return tokenTemplateString;
    }

    static private string GenerateTokenRequirements()
    {

        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();


        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 

    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // hello following code configures PlayReady License Template using .NET classes
        // and returns hello XML string.

        //hello PlayReadyLicenseResponseTemplate class represents hello template for hello response sent back toohello end user. 
        //It contains a field for a custom data string between hello license server and hello application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // hello PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // toobe returned toohello end users. 
        //It contains hello data on hello content key in hello license and any rights or restrictions toobe 
        //enforced by hello PlayReady DRM runtime when using hello content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether hello license is persistent (saved in persistent storage on hello client) 
        //or non-persistent (only held in memory while hello player is using hello license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

        // AllowTestDevices controls whether test devices can use hello license or not.  
        // If true, hello MinimumSecurityLevel property of hello license
        // is set too150.  If false (hello default), hello MinimumSecurityLevel property of hello license is set too2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure hello Play Right in hello PlayReady license by using hello PlayReadyPlayRight class. 
        // It grants hello user hello ability tooplayback hello content subject toohello zero or more restrictions 
        // configured in hello license and on hello PlayRight itself (for playback specific policy). 
        // Much of hello policy on hello PlayRight has toodo with output restrictions 
        // which control hello types of outputs that hello content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if hello DigitalVideoOnlyContentRestriction is enabled, 
        //then hello DRM runtime will only allow hello video toobe displayed over digital outputs 
        //(analog video outputs won’t be allowed toopass hello content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect hello consumer experience. 
        // If hello output protections are configured too restrictive, 
        // hello content might be unplayable on some clients. For more information, see hello PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


teszt jogkivonat alapján hello token korlátozás a hello hitelesítési házirend lásd használt tooget [ez](#test) szakasz. 

## <a id="types"></a>Típusok ContentKeyAuthorizationPolicy definiálásakor használja
### <a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

### <a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType
    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

### <a id="TokenType"></a>TokenType
    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Következő lépés
Most, hogy konfigurálta a tartalomkulcs hitelesítési szabályzatának, nyissa meg toohello [hogyan tooconfigure objektumtovábbítási szabályzat](media-services-dotnet-configure-asset-delivery-policy.md) témakör.

