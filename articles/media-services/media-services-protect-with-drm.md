---
title: "aaaUsing PlayReady és/vagy Widevine a dynamic common encryption |} Microsoft Docs"
description: "A Microsoft Azure Media Services lehetővé teszi, hogy Ön toodeliver MPEG-DASH, Smooth Streaming vagy Http-Live-Streaming (HLS) streamjeit Microsoft PlayReady DRM. Emellett lehetővé teszi a Widevine DRM-Védelemmel ellátott DASH toodelivery. Ez a témakör bemutatja, hogyan toodynamically titkosítása a PlayReady vagy Widevine DRM-Védelemmel."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 548d1a12-e2cb-45fe-9307-4ec0320567a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 0475e6ec80dcf39eb4e5c4ad4d17f821502951bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-playready-andor-widevine-dynamic-common-encryption"></a>A PlayReady és/vagy Widevine Dynamic Common Encryption titkosítás használata

> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-drm.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
>
>

A Microsoft Azure Media Services lehetővé teszi a toodeliver MPEG-DASH, Smooth Streaming vagy HTTP-Live-Streaming (HLS) streamjeit [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). Emellett lehetővé teszi a Widevine DRM-licencek toodeliver titkosított DASH-streameket. Mind a PlayReady, mind a Widevine titkosítása hello Common Encryption (ISO/IEC 23001-7 CENC) megadását. Használhat [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (3.5.1 hello verziójával kezdődően) vagy a REST API tooconfigure az AssetDeliveryConfiguration toouse Widevine.

A Media Services része egy szolgáltatás, amelynek segítségével PlayReady vagy Widevine DRM-licenceket továbbíthat. A Media Services is biztosít, amelyek lehetővé teszik a hello jogok konfigurálása API-k és korlátozásokat, amelyeket használni szeretne hello PlayReady vagy Widevine DRM futásidejű tooenforce felhasználó lejátssza védett tartalmakat. Ha egy felhasználó egy DRM védett tartalmat igényel, hello lejátszóalkalmazás fog licencet kér hello AMS-licencelési szolgáltatástól. hello AMS-licencelési szolgáltatástól egy licenc toohello player állít ki, ha engedélyezett. A PlayReady vagy Widevine-licencek tartalmazza, amelyek segítségével hello ügyfél player toodecrypt és adatfolyam hello tartalom hello visszafejtési kulcsot.

Használhatja a következő AMS-partnereket toohelp Widevine-licencek átadná hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). További információk: integráció az [Axinom](media-services-axinom-integration.md) és a [castLabs](media-services-castlabs-integration.md) rendszerekkel.

A Media Services szolgáltatásban több különböző módot is beállíthat, amelyek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat. hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: Nyissa meg, vagy a token korlátozás. hello token korlátozott házirend által a Secure Token Service (STS) kiadott tokennek kell csatolni. A Media Services hello támogatja a jogkivonatokat [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) formátumú és [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) formátumú. További információkért lásd: Configure hello tartalomkulcs engedélyezési házirendjét.

tootake előnye a dinamikus titkosítás toohave többszörös sávszélességű MP4-fájlok vagy többféle sávszélességű Smooth Streaming-forrásfájlokat tartalmazó objektumot kell. Szükség tooconfigure hello továbbítási házirendjeit hello eszköz (lásd a témakör későbbi részében). Majd a streamelési URL-cím hello hello formátummegadás alapján, hello Igényalapú Streamelési kiszolgáló biztosítja, hogy hello adatfolyam a rendszer a kiválasztott hello protokollal. Ennek eredményeképpen csak akkor kell toostore és fizetési hello fájlokat egy egyetlen tárolási formátumban és a Media Services elkészíti és kiszolgálja hello ügyféltől érkező kéréseknek megfelelő HTTP-válasz.

Ez a témakör, amely többféle DRM, például PlayReady és Widevine-mel védett médiafájlok továbbításával foglalkoznak hasznos toodevelopers lenne. hello a témakör bemutatja, hogyan tooconfigure hello-e PlayReady-licenctovábbítási szolgáltatásra vonatkozó szabályzatokat úgy, hogy csak arra jogosult ügyfelek kaphassák a PlayReady és Widevine-licenceket. Azt is bemutatja, hogyan toouse dinamikus titkosítás funkciót a PlayReady vagy Widevine DRM-Védelemmel keresztüli kötőjel.

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. 

## <a name="download-sample"></a>Minta letöltése
Letöltheti a cikkben leírt hello mintát [Itt](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).

## <a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>A Dynamic Common Encryption és DRM-licenctovábbítási szolgáltatások konfigurálása

Az alábbiakban hello általános lépéseket, hogy kell tooperform a PlayReady, hello Media Services licenctovábbítási szolgáltatása segítségével, és a dinamikus titkosítás használata eszközök védelme esetén.

1. Hozzon létre egy eszközt, majd fájlok feltöltése hello objektumba.
2. Hello eszköz tartalmazó hello fájl toohello adaptív sávszélességű MP4 típusú beállításkészlettel kódolása.
3. Hozzon létre egy tartalomkulcsot, majd társítsa a kódolt hello eszköz. A Media Services szolgáltatásban a hello tartalomkulcs tartalmazza a hello objektum titkosítási kulcsát.
4. Hello tartalomkulcs hitelesítési szabályzatának konfigurálása. hello tartalomkulcs-hitelesítési szabályzatot kell állította be, és ahhoz, hogy hello tartalom kulcs toobe kézbesített toohello ügyfél hello ügyfél teljesíti.

    Hello tartalomkulcs-hitelesítési házirend létrehozásakor toospecify hello következőkre lesz szüksége: kézbesítési módszer (PlayReady vagy Widevine), korlátozások (nyitott vagy token), és információt adott toohello kulcs kézbesítési típust, amely meghatározza, hogyan kerül hello kulcs toohello ügyfél ([PlayReady](media-services-playready-license-template-overview.md) vagy [Widevine](media-services-widevine-license-template-overview.md) licencsablon).

5. Az objektum továbbítási szabályzatát hello konfigurálása. hello továbbítási szabályzat konfigurációjához tartalmazza: továbbítási protokoll (például MPEG DASH, HLS, Smooth Streaming vagy az összes), a dinamikus titkosítás (például Common Encryption) és a PlayReady vagy Widevine-licenc licenckérési URL-cím típusú hello.

    Tudta alkalmazni a különböző házirend tooeach protokollt a következő hello azonos eszköz. Beállíthat például PlayReady titkosítási tooSmooth/DASH és AES Envelope tooHLS. Nincsenek megadva a továbbítási szabályzatban protokollok (például hozzáadhat egy szabályzatban, amely csak HLS hello protokoll) le lesz tiltva streaming. hello kivétel toothis jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot egyáltalán. Ezt követően hello törölje a jelet minden protokoll engedélyezett.

6. Hozzon létre egy OnDemand-kereső rendelés tooget egy adatfolyam-továbbítási URL-címet.

Hello hello témakör végén teljes .NET típusú példát talál.

a következő kép hello hello fentiekben leírt munkafolyamatot mutatja be. Itt hello jogkivonat-hitelesítéshez használt.

![Védelem biztosítása a PlayReadyvel](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

Ez a témakör többi hello részletes magyarázatokat, kódmintákat és olyan hivatkozásokat tootopics, amelyek bemutatják, hogyan tooachieve hello a fent leírt biztosít.

## <a name="current-limitations"></a>Aktuális korlátozások
Ha hozzáadásakor vagy módosításakor az adategység továbbítási házirendjét, törölnie kell hello tartozó lokátort (ha van ilyen), majd hozzon létre egy új lokátort.

Az Azure Media Services szolgáltatással végzett Widevine-titkosításra vonatkozó korlátozások: a funkció jelenleg nem támogatja több tartalomkulcs használatát.

## <a name="create-an-asset-and-upload-files-into-hello-asset"></a>Hozzon létre egy eszközt, majd fájlok feltöltése hello objektumba
A sorrend toomanage, kódolásához és streameléséhez a videók, akkor először fel kell töltenie a tartalom a Microsoft Azure Media Services. A feltöltést követően a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.

További információk: [Upload Files into a Media Services account](media-services-dotnet-upload-files.md) (Fájlok feltöltése a Media Services-fiókba).

## <a name="encode-hello-asset-containing-hello-file-toohello-adaptive-bitrate-mp4-set"></a>Hello eszköz tartalmazó hello fájl toohello adaptív sávszélességű MP4 típusú beállításkészlettel kódolása
A dinamikus titkosítás szüksége toocreate többszörös sávszélességű MP4-fájlok vagy többféle sávszélességű Smooth Streaming-forrásfájlokat tartalmazó objektumot. Ezt követően hello hello jegyzékben megadott formátumnak alapján és kérelem darabolható, igény szerinti adatfolyam-kiszolgáló biztosítja a megjelenő hello adatfolyam a kiválasztott protokollal hello hello. Ennek eredményeképpen csak akkor kell toostore és fizetési hello fájlokat egyetlen tárolási formátumban és a Media Services szolgáltatás elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ hello. További információkért lásd: hello [dinamikus becsomagolás áttekintése](media-services-dynamic-packaging-overview.md) témakör.

Útmutatást tooencode, lásd: [hogyan tooencode keresztül Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a id="create_contentkey"></a>Hozzon létre egy tartalomkulcsot, és azt társítsa a kódolt hello eszköz
A Media Services szolgáltatásban hello tartalomkulcs tartalmazza, amelyet az eszköz tooencrypt hello kulcs együtt.

További információk: [Create content key](media-services-dotnet-create-contentkey.md) (Tartalomkulcs létrehozása).

## <a id="configure_key_auth_policy"></a>Hello tartalomkulcs hitelesítési szabályzatának konfigurálása
A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat. hello tartalomkulcs-hitelesítési szabályzatot kell Ön által konfigurált és érheti el, ha hello ügyfél (a lejátszó) ahhoz, hogy hello kulcs toobe toohello ügyfél kézbesíteni. hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: Nyissa meg, vagy a token korlátozás.

További információk: [A tartalomkulcs hitelesítési szabályzatának létrehozása](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).

## <a id="configure_asset_delivery_policy"></a>Objektumtovábbítási szabályzat konfigurálása
Az objektum továbbítási szabályzatát hello konfigurálása. Néhány dolog, amely az eszköz továbbítási szabályzat konfigurációjához hello tartalmazza:

* hello DRM licenc licenckérési URL-cím.
* hello objektumtovábbítási protokoll (például MPEG DASH, HLS, Smooth Streaming vagy all).
* a dinamikus titkosítás (az ebben az esetben Common Encrpytion) hello típusa.

További információk: [Objektumtovábbítási szabályzat konfigurálása ](media-services-rest-configure-asset-delivery-policy.md).

## <a id="create_locator"></a>Hozzon létre egy OnDemand-lokátor a rendelés tooget streamelési URL-cím
Szüksége lesz tooprovide a felhasználó az URL-címe Smooth, DASH vagy HLS streamelési hello.

> [!NOTE]
> Objektumtovábbítási szabályzat hozzáadásakor vagy módosításakor törölnie kell az ahhoz tartozó meglévő lokátort (ha van), majd létre kell hoznia egy új lokátort.
>
>

Hogyan toopublish egy eszköz és -buildek a streamelési URL-cím: kapcsolatos utasításokat [streamelési URL-cím összeállítása](media-services-deliver-streaming-content.md).

## <a name="get-a-test-token"></a>Tesztjogkivonat lekérése
Tesztjogkivonat lekérése hello hello kulcshitelesítési szabályzatban használt jogkivonat korlátozás alapján.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
    //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
    //so you have tooadd it in front of hello token string.
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);


Használhatja a hello [AMS-lejátszóval](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest az adatfolyam.

## <a name="create-and-configure-a-visual-studio-project"></a>Egy Visual Studio-projekt létrehozása és konfigurálása

1. A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md). 
2. Adja hozzá a következő elemek túl hello**appSettings** az app.config fájlban meghatározott:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a>Példa

hello következő mutatja be .NET-keretrendszerhez készült Azure Media Services SDK-ban bevezetett funkcióinak-verzió 3.5.2 (pontosabban hello képességét toodefine egy Widevine sablon licenc és Widevine licencet kér az Azure Media Services).

Írja felül a Program.cs fájlban hello kód hello kód itt látható.

>[!NOTE]
>A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat. Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek. További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.

Győződjön meg arról, hogy tooupdate változók toopoint toofolders ahol a bemeneti fájlok találhatók.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
    using Newtonsoft.Json;

    namespace DynamicEncryptionWithDRM
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

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateCommonTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
            Console.WriteLine();

            if (tokenRestriction)
            tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
            else
            AddOpenAuthorizationPolicy(key);

            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
            Console.WriteLine();

            CreateAssetDeliveryPolicy(encodedAsset, key);
            Console.WriteLine("Created asset delivery policy. \n");
            Console.WriteLine();

            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
            {
            // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
            // back into a TokenRestrictionTemplate class instance.
            TokenRestrictionTemplate tokenTemplate =
                TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

            // Generate a test token based on hello hello data in hello given TokenRestrictionTemplate.
            // Note, you need toopass hello key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use hello http://amsplayer.azurewebsites.net/azuremediaplayer.html player tootest streams.
            // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);

            Console.ReadLine();
        }

        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
            Console.WriteLine("File does not exist.");
            return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
        {
            var encodingPreset = "Adaptive Streaming";

            IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} too{1}",
                        inputAsset.Name,
                        encodingPreset));

            var mediaProcessors =
            _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();

            var latestMediaProcessor =
            mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();

            ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
            encodeTask.InputAssets.Add(inputAsset);
            encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }


        static public IContentKey CreateCommonTypeContentKey(IAsset asset)
        {

            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryption);

            // Associate hello key with hello asset.
            asset.ContentKeys.Add(key);

            return key;
        }

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

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
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

        private static string ConfigureWidevineLicenseTemplate()
        {
            var template = new WidevineMessage
            {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                    new ContentKeySpecs
                    {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
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

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            // Get hello PlayReady license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

            // GetKeyDeliveryUrl for Widevine attaches hello KID toohello URL.
            // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
            // hello WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption
            // tooappend /? KID =< keyId > toohello end of hello url when creating hello manifest.
            // As a result Widevine license acquisition URL will have KID appended twice,
            // so we need tooremove hello KID that in hello URL when we call GetKeyDeliveryUrl.

            Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
            UriBuilder uriBuilder = new UriBuilder(widevineUrl);
            uriBuilder.Query = String.Empty;
            widevineUrl = uriBuilder.Uri;

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                    {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}

            };

            // In this case we only specify Dash streaming protocol in hello delivery policy,
            // All other protocols will be blocked from streaming.
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }

        /// <summary>
        /// Gets hello streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference toohello streaming manifest file from hello  
            // collection of files in hello asset.

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator toohello streaming content on an origin.
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL toohello manifest file.
            return originLocator.Path + assetFile.Name;
        }

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
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


## <a name="next-step"></a>Következő lépés
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Lásd még:
[CENC with Multi-DRM and Access Control](media-services-cenc-with-multidrm-access-control.md) (CENC többszörös DRM-mel és hozzáférés-vezérléssel)

[Configure Widevine packaging with AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services) (Widevine-csomagolás konfigurálása az AMS-szel)

[Announcing Google Widevine license delivery services in Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/) (A Google Widevine-licenctovábbítási szolgáltatás megjelenése az Azure Media Services-ben)
