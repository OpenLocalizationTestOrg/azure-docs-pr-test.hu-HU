---
title: aaaProtect HLS tartalmakat a Microsoft PlayReady vagy Apple FairPlay - Azure |} Microsoft Docs
description: "Ez a témakör áttekintést nyújt, és bemutatja, hogyan toouse Azure Media Services toodynamically titkosítani a HTTP-Live Streaming (HLS) az Apple FairPlay. Azt is bemutatja, hogyan toouse hello Media Services licenc kézbesítési szolgáltatás toodeliver FairPlay-licenc tooclients."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 91ca451e3e7bf0da1d74dac4c99180f08f39e4ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protect-your-hls-content-with-apple-fairplay-or-microsoft-playready"></a>A tartalom Apple FairPlay vagy a Microsoft PlayReady HLS védelme
Az Azure Media Services lehetővé teszi, hogy Ön toodynamically titkosítani a HTTP-Live Streaming (HLS) a következő formátumok hello segítségével:  

* **AES-128 boríték tiszta kulcsot**

    hello teljes adatrészlet használatával titkosítja hello **AES-128 CBC** mód. hello adatfolyam hello visszafejtése natív módon támogatott iOS és OS X-lejátszót. További információkért lásd: [AES-128 használata a dinamikus titkosítás és a kulcs kézbesítési szolgáltatás](media-services-protect-with-aes128.md).
* **Apple FairPlay**

    egyes videó hello és hang minták hello segítségével lettek titkosítva **AES-128 CBC** mód. **FairPlay Streaming** (FPS) integrálva van hello eszköz-operációsrendszerek, iOS és az Apple TV natív támogatását. OS x Safari FPS hello titkosított adathordozó-bővítmények (EME) felület támogatása használatával lehetővé teszi.
* **Microsoft PlayReady**

hello következő kép bemutatja hello **HLS + FairPlay vagy PlayReady a dinamikus titkosítás** munkafolyamat.

![A dinamikus titkosítás munkafolyamat diagramja](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

Ez a témakör bemutatja, hogyan toouse Media Services toodynamically titkosítani az Apple FairPlay HLS tartalmaihoz. Azt is bemutatja, hogyan toouse hello Media Services licenc kézbesítési szolgáltatás toodeliver FairPlay-licenc tooclients.

> [!NOTE]
> Ha is szeretné tooencrypt a HLS tartalom a PlayReady, toocreate közös tartalomkulcs kell, és rendelje hozzá azt az objektumot. A is kell tooconfigure hello tartalomkulcs hitelesítési szabályzatának, [segítségével PlayReady a dynamic common encryption](media-services-protect-with-drm.md).
>
>

## <a name="requirements-and-considerations"></a>Követelmények és szempontok

hello következő szükség, a Media Services toodeliver titkosított HLS integrált FairPlay és toodeliver FairPlay-licenc használata esetén:

  * Egy Azure-fiók. További részletek: [Ingyenes Azure-próbafiók](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
  * Egy Media Services-fiók. toocreate, lásd: [hello Azure-portál használatával Azure Media Services-fiók létrehozása](media-services-portal-create-account.md).
  * A regisztráció [Apple fejlesztési Program](https://developer.apple.com/).
  * Apple szükséges hello tartalom tulajdonosa tooobtain hello [központi telepítési csomag](https://developer.apple.com/contact/fps/). Adja meg, hogy már megvalósította a kulcs biztonsági modul (KSM) a Media Services, és, hogy a kért hello végső FPS csomag. Nincsenek a végső FPS toogenerate hitelesítő csomagot, majd az beszerzése hello utasításait hello alkalmazás titkos kulcs (KÉRJEN). Kérje meg tooconfigure FairPlay használja.
  * Az Azure Media Services .NET SDK-verzió **3.6.0** vagy újabb.

a Media Services kulcs kézbesítési oldalon dolgok következő hello kell beállítani:

  * **Alkalmazás Cert (AC)**: Ez az hello titkos kulcsot tartalmazó .pfx fájlt. A fájl létrehozásához, és a titkosítás jelszóval.

       A kulcs továbbítási szabályzatban konfigurálásakor meg kell adnia a jelszót és hello .pfx fájlt Base64 formátumban.

      hello lépések azt mutatják be, hogyan toogenerate .pfx tanúsítvány FairPlay fájlt:

    1. A https://slproweb.com/products/Win32OpenSSL.html OpenSSL telepíteni.

        Nyissa meg a toohello mappára, ahol hello FairPlay tanúsítvány és egyéb fájlokat, az Apple által szállított.
    2. Futtassa a parancsot követő hello parancssorból hello. Ez a hello .cer fájl tooa .pem-fájlokat alakítja át.

        "C:\OpenSSL-Win32\bin\openssl.exe" x509-der tájékoztatja-a fairplay.cer-fairplay-out.pem kimenő
    3. Futtassa a parancsot követő hello parancssorból hello. Hello .pem fájl tooa .pfx fájl alakítja hello titkos kulccsal. hello jelszót hello .pfx fájl majd OpenSSL: kérte.

        "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-exportálása - kimenő fairplay-out.pfx-inkey privatekey.pem-fairplay-out.pem - passin file:privatekey-pem-pass.txt a
  * **Alkalmazás Cert jelszó**: hello jelszót a .pfx fájl hello létrehozásához.
  * **Alkalmazás Cert jelszó azonosítója**: hello jelszó, hasonló toohow azokat más Media Services kulcsok töltse fel kell tölteni. Használjon hello **ContentKeyType.FairPlayPfxPassword** felsorolási érték tooget hello Media Services-azonosító. Ez a szükséges toouse belül hello kulcs kézbesítési házirend-beállításként.
  * **IV**: egy véletlenszerű érték 16 bájt. Meg kell egyeznie a hello objektumtovábbítási szabályzat iv hello. Létrehozhat hello iv, és mindkét helyen elhelyezi: hello adategység továbbítási házirendjét és hello kulcs kézbesítési házirend-beállításként.
  * **Kérje meg**: Ez a kulcs érkezik hello hitelesítő hello Apple Developer portálon keresztül generálásakor. Minden egyes fejlesztői csapat egyedi KÉRJEN fog kapni. Hello KÉRJEN másolatának mentése, és tárolja biztonságos helyen. Később szüksége lesz tooconfigure KÉRJEN FairPlayAsk tooMedia szolgáltatásként.
  * **Kérje meg azonosító**: Ez az azonosító kapjuk meg, kérje meg a Media Services feltöltésekor. Kérje meg a hello kell feltöltenie **ContentKeyType.FairPlayAsk** számbavételi érték. Hello, így hello Media Services Azonosítót ad vissza, és azt, hogy mit kell használni, amikor hello kulcs kézbesítési házirend beállítást.

hello következőket kell beállítania a hello FPS ügyféloldali:

  * **Alkalmazás Cert (AC)**: egy.cer/.der fájl, amely hello nyilvános kulcsot, mely hello operációs rendszere tooencrypt néhány hasznos tartalmaz. A Media Services tooknow információt kell, mert a hello player miatt szükséges. hello kulcs kézbesítési szolgáltatás visszafejti hello megfelelő titkos kulccsal.

tooplay biztonsági FairPlay titkosított adatfolyam, első lekérni egy valódi kérje meg, és majd a valódi tanúsítvány jön létre. A folyamat minden három részből hoz létre:

  * .der fájl
  * .pfx-fájlt
  * hello .pfx jelszavát

hello következő ügyfelek támogatják a HLS **AES-128 CBC** titkosítási: OS X, az Apple TV IOS Safari böngésző.

## <a name="configure-fairplay-dynamic-encryption-and-license-delivery-services"></a>FairPlay dinamikus titkosítás és licenc licenctovábbítási szolgáltatások konfigurálása
Az alábbiakban hello általános lépéseket a FairPlay eszközök védelmének hello Media Services licenctovábbítási szolgáltatása segítségével, valamint a dinamikus titkosítás használatával.

1. Hozzon létre egy eszközt, és a fájlok feltöltése hello objektumba.
2. Hello eszköz, amely tartalmazza a hello fájl toohello adaptív sávszélességű MP4 típusú beállításkészlettel kódolása.
3. Hozzon létre egy tartalomkulcsot, és társítsa a kódolt hello eszköz.  
4. Hello tartalomkulcs hitelesítési szabályzatának konfigurálása. Adja meg a hello következő beállításokat:

   * hello kézbesítési módszert (ebben az esetben az FairPlay).
   * FairPlay házirend-beállítások konfigurálása. További részletekért tooconfigure FairPlay, lásd: hello **ConfigureFairPlayPolicyOptions()** hello minta az alábbi metódust.

     > [!NOTE]
     > Általában érdemes választani tooconfigure FairPlay házirend beállításai csak egyszer, mert csak egy hitelesítő és egy KÉRJEN egy készletét van.
     >
     >
   * Korlátozások (nyitott vagy token).
   * Információt adott toohello kulcs kézbesítési típust, amely meghatározza, hogyan hello kulcs toohello ügyfél kerül-e.
5. Hello objektumtovábbítási szabályzat konfigurálása. hello továbbítási szabályzat konfigurációjához tartalmazza:

   * hello objektumtovábbítási protokoll (HLS).
   * a dinamikus titkosítás (közös CBC titkosítás) hello típusa.
   * hello licenc licenckérési URL-cím.

     > [!NOTE]
     > Ha azt szeretné, hogy toodeliver FairPlay és egy másik digitális tartalomvédelmi (DRM) rendszeren titkosított adatfolyam, tooconfigure külön továbbítási házirendjeit van:
     >
     > * Egy IAssetDeliveryPolicy tooconfigure dinamikus adaptív Streameléshez HTTP (DASH) a Common Encryption (CENC) (PlayReady + Widevine), és zökkenőmentes a Playreadyvel
     > * Egy másik IAssetDeliveryPolicy tooconfigure a HLS FairPlay
     >
     >
6. Hozzon létre egy OnDemand-lokátor tooget egy adatfolyam-továbbítási URL-címet.

## <a name="use-fairplay-key-delivery-by-player-apps"></a>FairPlay kulcs kézbesítési player alkalmazások használatát
Player alkalmazásokat fejleszthet hello iOS SDK használatával. toobe képes tooplay FairPlay tartalmat, hogy tooimplement hello licenc exchange protokoll. Ez a protokoll Apple nincs megadva. Már be tooeach alkalmazást hogyan toosend kulcs kézbesítési kéri. Media Services FairPlay kulcs kézbesítési szolgáltatás hello hello SPC toocome www-form-url kódolt feladás egy vagy több üzenetet, a következő képernyő hello várja:

    spc=<Base64 encoded SPC>

> [!NOTE]
> Az Azure Media Player nem támogatja a FairPlay lejátszás hello kezdő verzióról. MAC OS x, tooget FairPlay lejátszás hello minta player szerzi be hello Apple developer-fiók.
>
>

## <a name="streaming-urls"></a>Adatfolyam-továbbítási URL-címek
Ha az objektum egynél több DRM lett titkosítva, a streamelési URL-cím hello egy titkosítási címke használja: (formátum = "m3u8-aapl" titkosítási = "xxx").

a következő szempontok hello vonatkoznak:

* Csak nulla vagy egy titkosítási típus adható meg.
* hello titkosítási típus toobe hello URL-címben megadott, ha csak egy titkosítási lett alkalmazott toohello eszköz nem rendelkezik.
* hello titkosítási típus-és nagybetűket.
* a következő típusú titkosítás hello adható meg:  
  * **cenc**: Common encryption (PlayReady vagy Widevine)
  * **cbcs-aapl**: FairPlay
  * **CBC**: AES envelope titkosítás

## <a name="create-and-configure-a-visual-studio-project"></a>Egy Visual Studio-projekt létrehozása és konfigurálása

1. A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md). 
2. Adja hozzá a következő elemek túl hello**appSettings** az app.config fájlban meghatározott:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a>Példa

a következő minta hello hello képességét toouse Media Services toodeliver FairPlay titkosított tartalom mutatja be. Ez a funkció a .NET-hez verzió 3.6.0 bevezetett hello Azure Media Services SDK-t. 

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
    using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
    using Newtonsoft.Json;
    using System.Security.Cryptography.X509Certificates;

    namespace DynamicEncryptionWithFairPlay
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

            IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);

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

            IJob job = _context.Jobs.Create(String.Format("Encoding {0}", inputAsset.Name));

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

        static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
        {
            // Create HLS SAMPLE AES encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryptionCbcs);

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


            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();

            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
            ContentKeyDeliveryType.FairPlay,
            restrictions,
            FairPlayConfiguration);


            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

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

            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();


            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                   ContentKeyDeliveryType.FairPlay,
                   restrictions,
                   FairPlayConfiguration);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate hello content key authorization policy with hello content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        private static string ConfigureFairPlayPolicyOptions()
        {
            // For testing you can provide all zeroes for ASK bytes together with hello cert from Apple FPS SDK.
            // However, for production you must use a real ASK from Apple bound tooa real prod certificate.
            byte[] askBytes = Guid.NewGuid().ToByteArray();
            var askId = Guid.NewGuid();
            // Key delivery retrieves askKey by askId and uses this key toogenerate hello response.
            IContentKey askKey = _context.ContentKeys.Create(
                        askId,
                        askBytes,
                        "askKey",
                        ContentKeyType.FairPlayASk);

            //Customer password for creating hello .pfx file.
            string pfxPassword = "<customer password for creating hello .pfx file>";
            // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key toogenerate hello response.
            var pfxPasswordId = Guid.NewGuid();
            byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
            IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                        pfxPasswordId,
                        pfxPasswordBytes,
                        "pfxPasswordKey",
                        ContentKeyType.FairPlayPfxPassword);

            // iv - 16 bytes random value, must match hello iv in hello asset delivery policy.
            byte[] iv = Guid.NewGuid().ToByteArray();

            //Specify hello .pfx file created by hello customer.
            var appCert = new X509Certificate2("path toohello .pfx file created by hello customer", pfxPassword, X509KeyStorageFlags.Exportable);

            string FairPlayConfiguration =
            Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                appCert,
                pfxPassword,
                pfxPasswordId,
                askId,
                iv);

            return FairPlayConfiguration;
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

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();

            var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);

            FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);

            // Get hello FairPlay license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);

            // hello reason hello below code replaces "https://" with "skd://" is because
            // in hello IOS player sample code which you obtained in Apple developer account,
            // hello player only recognizes a Key URL that starts with skd://.
            // However, if you are using a customized player,
            // you can choose whatever protocol you want.
            // For example, "https".

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                    {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
            };

            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
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


## <a name="next-steps-media-services-learning-paths"></a>Következő lépések: Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
