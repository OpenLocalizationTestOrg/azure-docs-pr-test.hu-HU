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
# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a><span data-ttu-id="984ed-103">A dinamikus titkosítás: konfigurálja a tartalomkulcs-hitelesítési házirend</span><span class="sxs-lookup"><span data-stu-id="984ed-103">Dynamic encryption: configure content key authorization policy</span></span>
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a><span data-ttu-id="984ed-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="984ed-104">Overview</span></span>
<span data-ttu-id="984ed-105">A Microsoft Azure Media Services lehetővé teszi a toodeliver MPEG-DASH, Smooth Streaming vagy HTTP-Live-Streaming (HLS) streamjeit az Advanced Encryption Standard (AES) (a 128 bites titkosítási kulcsok használatával) vagy [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span><span class="sxs-lookup"><span data-stu-id="984ed-105">Microsoft Azure Media Services enables you toodeliver MPEG-DASH, Smooth Streaming, and HTTP-Live-Streaming (HLS) streams protected with Advanced Encryption Standard (AES) (using 128-bit encryption keys) or [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span></span> <span data-ttu-id="984ed-106">AMS azt is lehetővé teszi, hogy a akkor toodeliver DASH-streameket Widevine DRM titkosítva.</span><span class="sxs-lookup"><span data-stu-id="984ed-106">AMS also enables you toodeliver DASH streams encrypted with Widevine DRM.</span></span> <span data-ttu-id="984ed-107">Mind a PlayReady, mind a Widevine titkosítása hello Common Encryption (ISO/IEC 23001-7 CENC) megadását.</span><span class="sxs-lookup"><span data-stu-id="984ed-107">Both PlayReady and Widevine are encrypted per hello Common Encryption (ISO/IEC 23001-7 CENC) specification.</span></span>

<span data-ttu-id="984ed-108">A Media Services is biztosít a **kulcs/Licenctovábbítási szolgáltatása** , amelyekről az ügyfelek AES-kulcsok úgy szerezheti be, vagy a PlayReady vagy Widevine-licencek tooplay hello titkosított tartalmat.</span><span class="sxs-lookup"><span data-stu-id="984ed-108">Media Services also provides a **Key/License Delivery Service** from which clients can obtain AES keys or PlayReady/Widevine licenses tooplay hello encrypted content.</span></span>

<span data-ttu-id="984ed-109">Ha azt szeretné, a Media Services tooencrypt egy eszköz tooassociate titkosítási kulcsot kell (**CommonEncryption** vagy **EnvelopeEncryption**) hello eszközt (leírtak [Itt](media-services-dotnet-create-contentkey.md)) és engedélyezési házirendeket hello kulcs (ebben a cikkben leírtak) is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="984ed-109">If you want for Media Services tooencrypt an asset, you need tooassociate an encryption key (**CommonEncryption** or **EnvelopeEncryption**) with hello asset (as described [here](media-services-dotnet-create-contentkey.md)) and also configure authorization policies for hello key (as described in this article).</span></span>

<span data-ttu-id="984ed-110">Ha olyan adatfolyamot kell megadni a Windows Media Player van szükség, Media Services használ-e a megadott hello kulcs toodynamically titkosítása a tartalmat, AES vagy DRM titkosítással történik.</span><span class="sxs-lookup"><span data-stu-id="984ed-110">When a stream is requested by a player, Media Services uses hello specified key toodynamically encrypt your content using AES or DRM encryption.</span></span> <span data-ttu-id="984ed-111">toodecrypt hello stream, hello player fog igényelni hello kulcs hello kulcs kézbesítési szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="984ed-111">toodecrypt hello stream, hello player will request hello key from hello key delivery service.</span></span> <span data-ttu-id="984ed-112">hello felhasználónak van-e toodecide engedélyezett tooget hello kulcs, hello szolgáltatás értékeli az hello kulcshoz megadott hello engedélyezési házirendeket.</span><span class="sxs-lookup"><span data-stu-id="984ed-112">toodecide whether or not hello user is authorized tooget hello key, hello service evaluates hello authorization policies that you specified for hello key.</span></span>

<span data-ttu-id="984ed-113">A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="984ed-113">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="984ed-114">hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: **nyissa meg a** vagy **token** korlátozás.</span><span class="sxs-lookup"><span data-stu-id="984ed-114">hello content key authorization policy could have one or more authorization restrictions: **open** or **token** restriction.</span></span> <span data-ttu-id="984ed-115">hello token korlátozott házirend által a Secure Token Service (STS) kiadott tokennek kell csatolni.</span><span class="sxs-lookup"><span data-stu-id="984ed-115">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="984ed-116">A Media Services hello támogatja a jogkivonatokat **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) formátumú és **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) formátumban.</span><span class="sxs-lookup"><span data-stu-id="984ed-116">Media Services supports tokens in hello **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) format and **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) format.</span></span>

<span data-ttu-id="984ed-117">A Media Services nem nyújt Secure Token szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="984ed-117">Media Services does not provide Secure Token Services.</span></span> <span data-ttu-id="984ed-118">Hozzon létre egy egyéni STS, vagy használja a Microsoft Azure ACS tooissue jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="984ed-118">You can create a custom STS or leverage Microsoft Azure ACS tooissue tokens.</span></span> <span data-ttu-id="984ed-119">hello STS konfigurált toocreate hello megadott kulccsal aláírva jogkivonatot kell lennie, és probléma jogcímek megadott hello token korlátozás konfigurációs (ebben a cikkben leírtak).</span><span class="sxs-lookup"><span data-stu-id="984ed-119">hello STS must be configured toocreate a token signed with hello specified key and issue claims that you specified in hello token restriction configuration (as described in this article).</span></span> <span data-ttu-id="984ed-120">hello Media Services kulcs kézbesítési szolgáltatás visszaadható hello titkosítási kulcs toohello ügyfél Ha hello jogkivonat érvényes, és hello hello jogkivonatában lévő jogcímeket egyeznek hello tartalomkulcsot konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="984ed-120">hello Media Services key delivery service will return hello encryption key toohello client if hello token is valid and hello claims in hello token match those configured for hello content key.</span></span>

<span data-ttu-id="984ed-121">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="984ed-121">For more information, see</span></span>

[<span data-ttu-id="984ed-122">JWT jogkivonat hitelesítési</span><span class="sxs-lookup"><span data-stu-id="984ed-122">JWT token authentication</span></span>](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

<span data-ttu-id="984ed-123">[Azure Media Services OWIN MVC-alapú alkalmazások integrálása az Azure Active Directoryban, és korlátozhatja a JWT jogcímei alapján kulcs tartalomkézbesítési](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span><span class="sxs-lookup"><span data-stu-id="984ed-123">[Integrate Azure Media Services OWIN MVC based app with Azure Active Directory and restrict content key delivery based on JWT claims](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span></span>

<span data-ttu-id="984ed-124">[Használjon Azure ACS tooissue tokeneket](http://mingfeiy.com/acs-with-key-services).</span><span class="sxs-lookup"><span data-stu-id="984ed-124">[Use Azure ACS tooissue tokens](http://mingfeiy.com/acs-with-key-services).</span></span>

### <a name="some-considerations-apply"></a><span data-ttu-id="984ed-125">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="984ed-125">Some considerations apply:</span></span>
* <span data-ttu-id="984ed-126">Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="984ed-126">When your AMS account is created a **default** streaming endpoint is added  tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="984ed-127">a dinamikus csomagolás és a dinamikus titkosítás, az adatfolyam-továbbítási végpontjának tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart toobe rendelkezik hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="984ed-127">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, your streaming endpoint has toobe in hello **Running** state.</span></span> 
* <span data-ttu-id="984ed-128">Az eszköz tartalmaznia kell egy adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá készletét.</span><span class="sxs-lookup"><span data-stu-id="984ed-128">Your asset must contain a set of adaptive bitrate MP4s or  adaptive bitrate Smooth Streaming files.</span></span> <span data-ttu-id="984ed-129">További információkért lásd: [kódolása egy eszköz](media-services-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="984ed-129">For more information, see [Encode an asset](media-services-encode-asset.md).</span></span>
* <span data-ttu-id="984ed-130">Töltse fel, és használja a kódolásához **AssetCreationOptions.StorageEncrypted** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="984ed-130">Upload and encode your assets using **AssetCreationOptions.StorageEncrypted** option.</span></span>
* <span data-ttu-id="984ed-131">Ha azt tervezi, hogy toohave több tartalomkulcs igénylő hello azonos házirend-konfigurációt is erősen ajánlott toocreate egyetlen engedélyezési házirend, és újra felhasználhatja több tartalomkulcs.</span><span class="sxs-lookup"><span data-stu-id="984ed-131">If you plan toohave multiple content keys that require hello same policy configuration, it is strongly recommended toocreate a single authorization policy and reuse it with multiple content keys.</span></span>
* <span data-ttu-id="984ed-132">hello kulcs kézbesítési szolgáltatás 15 percig gyorsítótárazza a ContentKeyAuthorizationPolicy és a kapcsolódó objektumok (házirend-beállítások és korlátozásai).</span><span class="sxs-lookup"><span data-stu-id="984ed-132">hello Key Delivery service caches ContentKeyAuthorizationPolicy and its related objects (policy options and restrictions) for 15 minutes.</span></span>  <span data-ttu-id="984ed-133">Ha hozzon létre egy ContentKeyAuthorizationPolicy és toouse "Token" korlátozás, adja meg, majd tesztelje, és frissítse a hello házirend túl "nyissa meg" korlátozás, mielőtt hello kapcsolók toohello "Megnyitás" házirendverzió hello házirend nagyjából 15 percig tart.</span><span class="sxs-lookup"><span data-stu-id="984ed-133">If you create a ContentKeyAuthorizationPolicy and specify toouse a “Token” restriction, then test it, and then update hello policy too“Open” restriction, it will take roughly 15 minutes before hello policy switches toohello “Open” version of hello policy.</span></span>
* <span data-ttu-id="984ed-134">Objektumtovábbítási szabályzat hozzáadásakor vagy módosításakor törölnie kell az ahhoz tartozó meglévő lokátort (ha van), majd létre kell hoznia egy új lokátort.</span><span class="sxs-lookup"><span data-stu-id="984ed-134">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
* <span data-ttu-id="984ed-135">Jelenleg nem titkosítható progresszív letöltés.</span><span class="sxs-lookup"><span data-stu-id="984ed-135">Currently, you cannot encrypt progressive downloads.</span></span>

## <a name="aes-128-dynamic-encryption"></a><span data-ttu-id="984ed-136">AES-128, a dinamikus titkosítás</span><span class="sxs-lookup"><span data-stu-id="984ed-136">AES-128 Dynamic Encryption</span></span>
### <a name="open-restriction"></a><span data-ttu-id="984ed-137">Nyissa meg a szoftverkorlátozó</span><span class="sxs-lookup"><span data-stu-id="984ed-137">Open Restriction</span></span>
<span data-ttu-id="984ed-138">Nyissa meg a szoftverkorlátozó azt jelenti, hogy a rendszer hello hello kulcs tooanyone kulcs kérést fog továbbítani.</span><span class="sxs-lookup"><span data-stu-id="984ed-138">Open restriction means hello system will deliver hello key tooanyone who makes a key request.</span></span> <span data-ttu-id="984ed-139">Ez a korlátozás tesztelési célokra hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="984ed-139">This restriction might be useful for testing purposes.</span></span>

<span data-ttu-id="984ed-140">a következő példa hello létrehoz egy megnyitott engedélyezési szabályzatot, és hozzáadja azt toohello tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="984ed-140">hello following example creates an open authorization policy and adds it toohello content key.</span></span>

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


### <a name="token-restriction"></a><span data-ttu-id="984ed-141">Token korlátozása</span><span class="sxs-lookup"><span data-stu-id="984ed-141">Token Restriction</span></span>
<span data-ttu-id="984ed-142">Ez a szakasz ismerteti, hogyan toocreate tartalomra kulcs engedélyezési házirendjét, és rendelje hozzá azt hello tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="984ed-142">This section describes how toocreate a content key authorization policy and associate it with hello content key.</span></span> <span data-ttu-id="984ed-143">hello engedélyezési házirend írja le, milyen hitelesítési követelmények fennállnak toodetermine kell lennie, ha hello felhasználó jogosult tooreceive hello kulcs (például hello "ellenőrzési kulcsot" lista tartalmaz hello kulcs lett aláírva, adott hello token).</span><span class="sxs-lookup"><span data-stu-id="984ed-143">hello authorization policy describes what authorization requirements must be met toodetermine if hello user is authorized tooreceive hello key (for example, does hello “verification key” list contain hello key that hello token was signed with).</span></span>

<span data-ttu-id="984ed-144">tooconfigure hello token korlátozás beállítást kell toouse XML toodescribe hello token engedélyezési követelményeihez.</span><span class="sxs-lookup"><span data-stu-id="984ed-144">tooconfigure hello token restriction option, you need toouse an XML toodescribe hello token’s authorization requirements.</span></span> <span data-ttu-id="984ed-145">hello token korlátozás konfigurációs XML-t meg kell felelnie a következő XML-séma toohello.</span><span class="sxs-lookup"><span data-stu-id="984ed-145">hello token restriction configuration XML must conform toohello following XML schema.</span></span>

#### <span data-ttu-id="984ed-146"><a id="schema"></a>Token korlátozás séma</span><span class="sxs-lookup"><span data-stu-id="984ed-146"><a id="schema"></a>Token restriction schema</span></span>
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

<span data-ttu-id="984ed-147">Hello konfigurálásakor **token** korlátozott házirend, meg kell adnia hello elsődleges ** ellenőrző kulcs **, **kibocsátó** és **célközönség** paraméterek.</span><span class="sxs-lookup"><span data-stu-id="984ed-147">When configuring hello **token** restricted policy, you must specify hello primary** verification key**, **issuer** and **audience** parameters.</span></span> <span data-ttu-id="984ed-148">hello ** elsődleges hitelesítési kulcs ** tartalmaz, amely a hello token hello kulcs aláírt, **kibocsátó** hello biztonságos biztonságijogkivonat-szolgáltatás adott problémák hello token van.</span><span class="sxs-lookup"><span data-stu-id="984ed-148">hello **primary verification key **contains hello key that hello token was signed with, **issuer** is hello secure token service that issues hello token.</span></span> <span data-ttu-id="984ed-149">Hello **célközönség** (más néven **hatókör**) hello leképezés ismerteti hello token vagy hello erőforrás hello token engedélyezi a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="984ed-149">hello **audience** (sometimes called **scope**) describes hello intent of hello token or hello resource hello token authorizes access to.</span></span> <span data-ttu-id="984ed-150">hello Media Services kulcs kézbesítési szolgáltatás ellenőrzi, hogy ezek az értékek hello token értékekre, hello hello sablont.</span><span class="sxs-lookup"><span data-stu-id="984ed-150">hello Media Services key delivery service validates that these values in hello token match hello values in hello template.</span></span> 

<span data-ttu-id="984ed-151">Használata esetén **Media Services SDK for .NET**, használhatja a hello **TokenRestrictionTemplate** osztály toogenerate hello korlátozás jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="984ed-151">When using **Media Services SDK for .NET**, you can use hello **TokenRestrictionTemplate** class toogenerate hello restriction token.</span></span>
<span data-ttu-id="984ed-152">a következő példa hello házirendet hoz létre engedélyezési jogkivonat korlátozás.</span><span class="sxs-lookup"><span data-stu-id="984ed-152">hello following example creates an authorization policy with a token restriction.</span></span> <span data-ttu-id="984ed-153">Ebben a példában hello ügyfélnek ehhez már toopresent tartalmazó jogkivonatok: aláírási kulcs (VerificationKey), a jogkivonat-kibocsátó és a szükséges jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="984ed-153">In this example, hello client would have toopresent a token that contains: signing key (VerificationKey), a token issuer, and required claims.</span></span>

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

#### <span data-ttu-id="984ed-154"><a id="test"></a>Teszt jogkivonat</span><span class="sxs-lookup"><span data-stu-id="984ed-154"><a id="test"></a>Test token</span></span>
<span data-ttu-id="984ed-155">tooget a teszt jogkivonat hello hello kulcshitelesítési szabályzatban használt jogkivonat korlátozás alapján, a következő hello.</span><span class="sxs-lookup"><span data-stu-id="984ed-155">tooget a test token based on hello token restriction that was used for hello key authorization policy, do hello following.</span></span>

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


## <a name="playready-dynamic-encryption"></a><span data-ttu-id="984ed-156">PlayReady dinamikus titkosítás</span><span class="sxs-lookup"><span data-stu-id="984ed-156">PlayReady Dynamic Encryption</span></span>
<span data-ttu-id="984ed-157">A Media Services lehetővé teszi tooconfigure hello jogokat és korlátozásokat, amelyet a PlayReady DRM futásidejű tooenforce hello amikor egy felhasználó próbál tooplay vissza védett tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="984ed-157">Media Services enables you tooconfigure hello rights and restrictions that you want for hello PlayReady DRM runtime tooenforce when a user is trying tooplay back protected content.</span></span> 

<span data-ttu-id="984ed-158">Amikor a Playreadyvel a tartalmak védelmére, hello egyikét meg kell toospecify a hitelesítési házirend hello definiáló XML-karakterlánc [PlayReady licencsablon](media-services-playready-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="984ed-158">When protecting your content with PlayReady, one of hello things you need toospecify in your authorization policy is an XML string that defines hello [PlayReady license template](media-services-playready-license-template-overview.md).</span></span> <span data-ttu-id="984ed-159">A Media Services SDK for .NET, hello **PlayReadyLicenseResponseTemplate** és **PlayReadyLicenseTemplate** határozza meg a PlayReady-Licencsablon hello osztályok nyújtanak segítséget.</span><span class="sxs-lookup"><span data-stu-id="984ed-159">In Media Services SDK for .NET, hello **PlayReadyLicenseResponseTemplate** and **PlayReadyLicenseTemplate** classes will help you define hello PlayReady License Template.</span></span>

<span data-ttu-id="984ed-160">[Ez a témakör](media-services-protect-with-drm.md) bemutatja, hogyan tooencrypt a tartalmaknak a **PlayReady** és **Widevine**.</span><span class="sxs-lookup"><span data-stu-id="984ed-160">[This topic](media-services-protect-with-drm.md) shows how tooencrypt your content with **PlayReady** and **Widevine**.</span></span>

### <a name="open-restriction"></a><span data-ttu-id="984ed-161">Nyissa meg a szoftverkorlátozó</span><span class="sxs-lookup"><span data-stu-id="984ed-161">Open Restriction</span></span>
<span data-ttu-id="984ed-162">Nyissa meg a szoftverkorlátozó azt jelenti, hogy a rendszer hello hello kulcs tooanyone kulcs kérést fog továbbítani.</span><span class="sxs-lookup"><span data-stu-id="984ed-162">Open restriction means hello system will deliver hello key tooanyone who makes a key request.</span></span> <span data-ttu-id="984ed-163">Ez a korlátozás tesztelési célokra hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="984ed-163">This restriction might be useful for testing purposes.</span></span>

<span data-ttu-id="984ed-164">a következő példa hello létrehoz egy megnyitott engedélyezési szabályzatot, és hozzáadja azt toohello tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="984ed-164">hello following example creates an open authorization policy and adds it toohello content key.</span></span>

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

### <a name="token-restriction"></a><span data-ttu-id="984ed-165">Token korlátozása</span><span class="sxs-lookup"><span data-stu-id="984ed-165">Token Restriction</span></span>
<span data-ttu-id="984ed-166">tooconfigure hello token korlátozás beállítást kell toouse XML toodescribe hello token engedélyezési követelményeihez.</span><span class="sxs-lookup"><span data-stu-id="984ed-166">tooconfigure hello token restriction option, you need toouse an XML toodescribe hello token’s authorization requirements.</span></span> <span data-ttu-id="984ed-167">hello token korlátozás konfigurációs XML-t meg kell felelnie a toohello XML-séma látható [ez](#schema) szakasz.</span><span class="sxs-lookup"><span data-stu-id="984ed-167">hello token restriction configuration XML must conform toohello XML schema shown in [this](#schema) section.</span></span>

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


<span data-ttu-id="984ed-168">teszt jogkivonat alapján hello token korlátozás a hello hitelesítési házirend lásd használt tooget [ez](#test) szakasz.</span><span class="sxs-lookup"><span data-stu-id="984ed-168">tooget a test token based on hello token restriction that was used for hello key authorization policy see [this](#test) section.</span></span> 

## <span data-ttu-id="984ed-169"><a id="types"></a>Típusok ContentKeyAuthorizationPolicy definiálásakor használja</span><span class="sxs-lookup"><span data-stu-id="984ed-169"><a id="types"></a>Types used when defining ContentKeyAuthorizationPolicy</span></span>
### <span data-ttu-id="984ed-170"><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType</span><span class="sxs-lookup"><span data-stu-id="984ed-170"><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType</span></span>
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

### <span data-ttu-id="984ed-171"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="984ed-171"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span></span>
    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

### <span data-ttu-id="984ed-172"><a id="TokenType"></a>TokenType</span><span class="sxs-lookup"><span data-stu-id="984ed-172"><a id="TokenType"></a>TokenType</span></span>
    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="984ed-173">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="984ed-173">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="984ed-174">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="984ed-174">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="984ed-175">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="984ed-175">Next step</span></span>
<span data-ttu-id="984ed-176">Most, hogy konfigurálta a tartalomkulcs hitelesítési szabályzatának, nyissa meg toohello [hogyan tooconfigure objektumtovábbítási szabályzat](media-services-dotnet-configure-asset-delivery-policy.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="984ed-176">Now that you have configured content key's authorization policy, go toohello [How tooconfigure asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md) topic.</span></span>

