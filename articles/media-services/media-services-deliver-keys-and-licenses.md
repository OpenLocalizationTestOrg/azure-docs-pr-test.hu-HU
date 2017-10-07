---
title: Azure Media Services toodeliver DRM-licencek aaaUse vagy AES-kulcsok
description: "Ez a cikk ismerteti, hogyan használható az Azure Media Services (AMS) toodeliver PlayReady és/vagy Widevine-licencek és AES-kulcsok, de hello rest (kódolása, titkosítása, streaming) a helyszíni kiszolgálók használatával."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 8546c2c1-430b-4254-a88d-4436a83f9192
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: a81da2973c79e5182ae58aeca7a0f14f3fc7c9ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-media-services-toodeliver-drm-licenses-or-aes-keys"></a><span data-ttu-id="3310f-103">Azure Media Services toodeliver DRM-licencek vagy AES-kulcsok használata</span><span class="sxs-lookup"><span data-stu-id="3310f-103">Use Azure Media Services toodeliver DRM licenses or AES keys</span></span>
<span data-ttu-id="3310f-104">Az Azure Media Services (AMS) lehetővé teszi a tooingest, kódolására, adja hozzá a védett tartalom és a tartalmak (lásd: [ez](media-services-protect-with-drm.md) cikkben alább).</span><span class="sxs-lookup"><span data-stu-id="3310f-104">Azure Media Services (AMS) enables you tooingest, encode, add content protection, and stream your content (see [this](media-services-protect-with-drm.md) article for details).</span></span> <span data-ttu-id="3310f-105">Van azonban az ügyfelek, akik csak szeretné, hogy toouse AMS toodeliver licencek és/vagy kulcsok, és tegye kódolása, titkosítása és adatfolyam-azok helyszíni kiszolgálókkal.</span><span class="sxs-lookup"><span data-stu-id="3310f-105">However, there are customers who only want toouse AMS toodeliver licenses and/or keys and do encoding, encrypting and streaming using their on-premises servers.</span></span> <span data-ttu-id="3310f-106">Ez a cikk ismerteti, hogyan használhatja az AMS toodeliver PlayReady és/vagy Widevine-licencek, de rest hello a helyszíni kiszolgálókkal.</span><span class="sxs-lookup"><span data-stu-id="3310f-106">This article describes how you can use AMS toodeliver PlayReady and/or Widevine licenses but do hello rest with your on-premises servers.</span></span> 

## <a name="overview"></a><span data-ttu-id="3310f-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3310f-107">Overview</span></span>
<span data-ttu-id="3310f-108">A Media Services része egy szolgáltatás, licencek és AES-128 kulcsok továbbítása a PlayReady és Widevine DRM-Védelemmel.</span><span class="sxs-lookup"><span data-stu-id="3310f-108">Media Services provides a service for delivering PlayReady and Widevine DRM licenses and AES-128 keys.</span></span> <span data-ttu-id="3310f-109">A Media Services is biztosít, amelyek lehetővé teszik a hello jogok konfigurálása API-k és korlátozásokat, amelyeket használni szeretne hello DRM futásidejű tooenforce, amikor egy felhasználó lejátssza hello DRM védett tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="3310f-109">Media Services also provides APIs that let you configure hello rights and restrictions that you want for hello DRM runtime tooenforce when a user plays back hello DRM protected content.</span></span> <span data-ttu-id="3310f-110">Ha a felhasználói kérelmek hello védett tartalom, hello lejátszóalkalmazás fog licencet kér hello AMS-licencelési szolgáltatástól.</span><span class="sxs-lookup"><span data-stu-id="3310f-110">When a user requests hello protected content, hello player application will request a license from hello AMS license service.</span></span> <span data-ttu-id="3310f-111">hello AMS-licencelési szolgáltatás akkor adja hello licenc toohello player, (Ha a kérelmező).</span><span class="sxs-lookup"><span data-stu-id="3310f-111">hello AMS license service will issue hello license toohello player (if it is authorized).</span></span> <span data-ttu-id="3310f-112">hello PlayReady és Widevine-licencek tartalmaz, amelyek segítségével hello ügyfél player toodecrypt és adatfolyam hello tartalom hello visszafejtési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="3310f-112">hello PlayReady and Widevine licenses contain hello decryption key that can be used by hello client player toodecrypt and stream hello content.</span></span>

<span data-ttu-id="3310f-113">Media Services engedélyezése a felhasználók, akik licenc vagy a kulcs kérelmek több lehetőséget támogatja.</span><span class="sxs-lookup"><span data-stu-id="3310f-113">Media Services supports multiple ways of authorizing users who make license or key requests.</span></span> <span data-ttu-id="3310f-114">Hello tartalomkulcs hitelesítési szabályzatának konfigurálása és hello házirend rendelkezhet egy vagy több korlátozás: Nyissa meg, vagy a token korlátozás.</span><span class="sxs-lookup"><span data-stu-id="3310f-114">You configure hello content key's authorization policy and hello policy could have one or more restrictions: open or token restriction.</span></span> <span data-ttu-id="3310f-115">hello token korlátozott házirend által a Secure Token Service (STS) kiadott tokennek kell csatolni.</span><span class="sxs-lookup"><span data-stu-id="3310f-115">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="3310f-116">A Media Services hello Simple Web Tokens (SWT) és JSON webes jogkivonat (JWT) formátumú tokeneket támogatja.</span><span class="sxs-lookup"><span data-stu-id="3310f-116">Media Services supports tokens in hello Simple Web Tokens (SWT) format and JSON Web Token (JWT) format.</span></span>

<span data-ttu-id="3310f-117">hello alábbi ábrán látható hello fő lépést kell tootake toouse AMS toodeliver PlayReady és/vagy Widevine-licencek, de a többi hello a helyszíni kiszolgálókkal.</span><span class="sxs-lookup"><span data-stu-id="3310f-117">hello following diagram shows hello main steps you need tootake toouse AMS toodeliver PlayReady and/or Widevine licenses but do hello rest with your on-premises servers.</span></span>

![Védelem biztosítása a PlayReadyvel](./media/media-services-deliver-keys-and-licenses/media-services-diagram1.png)

## <a name="download-sample"></a><span data-ttu-id="3310f-119">Minta letöltése</span><span class="sxs-lookup"><span data-stu-id="3310f-119">Download sample</span></span>
<span data-ttu-id="3310f-120">Letöltheti a cikkben leírt hello mintát [Itt](https://github.com/Azure/media-services-dotnet-deliver-drm-licenses).</span><span class="sxs-lookup"><span data-stu-id="3310f-120">You can download hello sample described in this article from [here](https://github.com/Azure/media-services-dotnet-deliver-drm-licenses).</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="3310f-121">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3310f-121">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="3310f-122">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="3310f-122">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="3310f-123">Adja hozzá a következő elemek túl hello**appSettings** az app.config fájlban meghatározott:</span><span class="sxs-lookup"><span data-stu-id="3310f-123">Add hello following elements too**appSettings** defined in your app.config file:</span></span>

    <span data-ttu-id="3310f-124"><add key="Issuer" value="http://testacs.com"/> <add key="Audience" value="urn:test"/></span><span class="sxs-lookup"><span data-stu-id="3310f-124"><add key="Issuer" value="http://testacs.com"/> <add key="Audience" value="urn:test"/></span></span>

## <a name="net-code-example"></a><span data-ttu-id="3310f-125">.NET-példakód</span><span class="sxs-lookup"><span data-stu-id="3310f-125">.NET code example</span></span>

<span data-ttu-id="3310f-126">a következő példakód hello bemutatja, hogyan toocreate egy közös tartalom kulcs-e, és a PlayReady vagy Widevine licenc licenckérési URL-címek lekérése.</span><span class="sxs-lookup"><span data-stu-id="3310f-126">hello following code example shows how toocreate a common content key and get PlayReady or Widevine license acquisition URLs.</span></span> <span data-ttu-id="3310f-127">Tooget kell követően az adatokat az AMS hello és a helyi kiszolgáló konfigurálása: **tartalomkulcs**, **kulcsazonosítója**, **licenckérési URL-cím licenc**.</span><span class="sxs-lookup"><span data-stu-id="3310f-127">You need tooget hello following pieces of information from AMS and configure your on-premises server: **content key**, **key id**, **license acquisition URL**.</span></span> <span data-ttu-id="3310f-128">Ha megfelelően konfigurált, a helyszíni kiszolgáló, a sikerült adatfolyam saját adatfolyam-kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="3310f-128">Once you configure your on-premises server, you could stream from your own streaming server.</span></span> <span data-ttu-id="3310f-129">Hello titkosított adatfolyam pontok tooAMS licenckiszolgáló, mert a player fog igényelni a licenc az AMS.</span><span class="sxs-lookup"><span data-stu-id="3310f-129">Since hello encrypted stream points tooAMS license server, your player will request a license from AMS.</span></span> <span data-ttu-id="3310f-130">Ha úgy dönt, hogy a jogkivonat hitelesítési, hello AMS licenckiszolgáló HTTPS keresztül küldött hello token ellenőrzi, és (ha van érvényes) hello licenc hátsó tooyour player fog továbbítani.</span><span class="sxs-lookup"><span data-stu-id="3310f-130">If you choose token authentication, hello AMS license server will validate hello token you sent through HTTPS and (if valid) will deliver hello license back tooyour player.</span></span> <span data-ttu-id="3310f-131">(hello Kódpélda csak bemutatja, hogyan toocreate egy közös kulcs tartalom és a PlayReady vagy Widevine licenc licenckérési URL-címek lekérése.</span><span class="sxs-lookup"><span data-stu-id="3310f-131">(hello code example only shows how toocreate a common content key and  get PlayReady or Widevine license acquisition URLs.</span></span> <span data-ttu-id="3310f-132">Ha azt szeretné, hogy a kulcsok toodelivery AES-128, a boríték tartalomkulcsot toocreate kell, és egy kulcs licenckérési URL-cím beszerzése és [ez](media-services-protect-with-aes128.md) hogyan cikkre mutat be toodo azt).</span><span class="sxs-lookup"><span data-stu-id="3310f-132">If you want toodelivery AES-128 keys, you need toocreate an envelope content key and get a key acquisition URL and [this](media-services-protect-with-aes128.md) article shows how toodo it).</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
    using Newtonsoft.Json;

    namespace DeliverDRMLicenses
    {
        class Program
        {
            // Read values from hello App.config file.
            private static readonly string _AADTenantDomain =
                ConfigurationManager.AppSettings["AADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
                ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

            private static readonly Uri _sampleIssuer =
                new Uri(ConfigurationManager.AppSettings["Issuer"]);
            private static readonly Uri _sampleAudience =
                new Uri(ConfigurationManager.AppSettings["Audience"]);

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                bool tokenRestriction = true;
                string tokenTemplateString = null;


                IContentKey key = CreateCommonTypeContentKey();

                // Print out hello key ID and Key in base64 string format
                Console.WriteLine("Created key {0} with key value {1} ",
                    key.Id, System.Convert.ToBase64String(key.GetClearKeyValue()));

                Console.WriteLine("PlayReady License Key delivery URL: {0}",
                    key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));

                Console.WriteLine("Widevine License Key delivery URL: {0}",
                    key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine));

                if (tokenRestriction)
                    tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                else
                    AddOpenAuthorizationPolicy(key);

                Console.WriteLine("Added authorization policy: {0}",
                    key.AuthorizationPolicyId);
                Console.WriteLine();
                Console.ReadLine();
            }

            static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
            {

                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          

                List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                    new List<ContentKeyAuthorizationPolicyRestriction>
                {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Open",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null
                        }
                };

                // Configure PlayReady and Widevine license templates.
                string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

                string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

                IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, PlayReadyLicenseTemplate);

                IContentKeyAuthorizationPolicyOption WidevinePolicy =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.Widevine,
                        restrictions, WidevineLicenseTemplate);

                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with no restrictions").
                            Result;


                contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
                // Associate hello content key authorization policy with hello content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }

            public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
            {
                string tokenTemplateString = GenerateTokenRequirements();

                List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                    new List<ContentKeyAuthorizationPolicyRestriction>
                {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Token Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                            Requirements = tokenTemplateString,
                        }
                };

                // Configure PlayReady and Widevine license templates.
                string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

                string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

                IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, PlayReadyLicenseTemplate);

                IContentKeyAuthorizationPolicyOption WidevinePolicy =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                        ContentKeyDeliveryType.Widevine,
                            restrictions, WidevineLicenseTemplate);

                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with token restrictions").
                            Result;

                contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);

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

                //hello PlayReadyLicenseResponseTemplate class represents hello template 
                //for hello response sent back toohello end user. 
                //It contains a field for a custom data string between hello license server 
                //and hello application (may be useful for custom app logic) 
                //as well as a list of one or more license templates.

                PlayReadyLicenseResponseTemplate responseTemplate =
                    new PlayReadyLicenseResponseTemplate();

                // hello PlayReadyLicenseTemplate class represents a license template 
                // for creating PlayReady licenses
                // toobe returned toohello end users. 
                // It contains hello data on hello content key in hello license 
                // and any rights or restrictions toobe 
                // enforced by hello PlayReady DRM runtime when using hello content key.
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();

                // Configure whether hello license is persistent 
                // (saved in persistent storage on hello client) 
                // or non-persistent (only held in memory while hello player is using hello license).  
                licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

                // AllowTestDevices controls whether test devices can use hello license or not.  
                // If true, hello MinimumSecurityLevel property of hello license
                // is set too150.  If false (hello default), 
                // hello MinimumSecurityLevel property of hello license is set too2000.
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

                // IMPORTANT: These types of restrictions can be very powerful 
                // but can also affect hello consumer experience. 
                // If hello output protections are configured too restrictive, 
                // hello content might be unplayable on some clients. 
                // For more information, see hello PlayReady Compliance Rules document.

                // For example:
                //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

                responseTemplate.LicenseTemplates.Add(licenseTemplate);

                return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
            }


            private static string ConfigureWidevineLicenseTemplate()
            {
                var template = new WidevineMessage
                {
                    allowed_track_types = AllowedTrackTypes.SD_HD,
                    content_key_specs = new[]
                    {
                            new ContentKeySpecs
                            {
                                required_output_protection =
                                    new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                                security_level = 1,
                                track_type = "SD"
                            }
                        },
                    policy_overrides = new
                    {
                        can_play = true,
                        can_persist = true,
                        can_renew = false
                    }
                };

                string configuration = JsonConvert.SerializeObject(template);
                return configuration;
            }


            static public IContentKey CreateCommonTypeContentKey()
            {
                // Create envelope encryption content key
                Guid keyId = Guid.NewGuid();
                byte[] contentKey = GetRandomBuffer(16);

                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);

                return key;
            }

            static private byte[] GetRandomBuffer(int length)
            {
                var returnValue = new byte[length];

                using (var rng =
                    new System.Security.Cryptography.RNGCryptoServiceProvider())
                {
                    rng.GetBytes(returnValue);
                }

                return returnValue;
            }
        }
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="3310f-133">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="3310f-133">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3310f-134">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="3310f-134">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="3310f-135">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="3310f-135">See also</span></span>
[<span data-ttu-id="3310f-136">PlayReady és/vagy Widevine Dynamic Common Encryption használatával</span><span class="sxs-lookup"><span data-stu-id="3310f-136">Using PlayReady and/or Widevine Dynamic Common Encryption</span></span>](media-services-protect-with-drm.md)

[<span data-ttu-id="3310f-137">AES-128 dinamikus titkosítás és a kulcs kézbesítési szolgáltatás használatával</span><span class="sxs-lookup"><span data-stu-id="3310f-137">Using AES-128 Dynamic Encryption and Key Delivery Service</span></span>](media-services-protect-with-aes128.md)

[<span data-ttu-id="3310f-138">Partnerek toodeliver Widevine-licencek tooAzure Media Services használatával</span><span class="sxs-lookup"><span data-stu-id="3310f-138">Using partners toodeliver Widevine licenses tooAzure Media Services</span></span>](media-services-licenses-partner-integration.md)

