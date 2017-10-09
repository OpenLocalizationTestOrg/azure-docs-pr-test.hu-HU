---
title: "AES-128 aaaUsing dinamikus titkosítás és a kulcs kézbesítési szolgáltatás |} Microsoft Docs"
description: "A Microsoft Azure Media Services lehetővé teszi, hogy Ön toodeliver a 128 bites AES titkosítási kulccsal titkosított tartalom. A Media Services hello kulcs kézbesítési szolgáltatás letölti a titkosítási kulcsok tooauthorized felhasználók is biztosít. Ez a témakör bemutatja, hogyan toodynamically titkosítani az AES-128 és hello kulcs kézbesítési szolgáltatás használata."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: cb1b413ec2ba79f7437464099cf72236ab93f312
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>AES-128 dinamikus titkosítás és a kulcs kézbesítési szolgáltatás használatával
> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-aes128.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Áttekintés
> [!NOTE]
> Lásd: [ez](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) videó megtudhatja, hogyan tooprotect médiatartalmak tartalom AES titkosítással.
> 
> 

A Microsoft Azure Media Services lehetővé teszi toodeliver Http-Live-Streaming (HLS), és zökkenőmentes adatfolyamok titkosítva az Advanced Encryption Standard (AES) (a 128 bites titkosítási kulcsok használatával). A Media Services hello kulcs kézbesítési szolgáltatás letölti a titkosítási kulcsok tooauthorized felhasználók is biztosít. Ha azt szeretné, a Media Services tooencrypt egy eszköz tooassociate hello eszközt titkosítási kulcsot kell, és engedélyezési házirendeket hello kulcs is konfigurálhatja. Ha olyan adatfolyamot kell megadni a Windows Media Player van szükség, Media Services használ-e a megadott hello kulcs toodynamically titkosítani az AES titkosítással történik. toodecrypt hello stream, hello player fog igényelni hello kulcs hello kulcs kézbesítési szolgáltatásból. hello felhasználónak van-e toodecide engedélyezett tooget hello kulcs, hello szolgáltatás értékeli az hello kulcshoz megadott hello engedélyezési házirendeket.

A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat. hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: Nyissa meg, vagy a token korlátozás. hello token korlátozott házirend által a Secure Token Service (STS) kiadott tokennek kell csatolni. A Media Services hello támogatja a jogkivonatokat [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) formátumú és [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) formátumú. További információkért lásd: [hello tartalomkulcs hitelesítési szabályzatának konfigurálása](media-services-protect-with-aes128.md#configure_key_auth_policy).

tootake előnye a dinamikus titkosítás toohave többszörös sávszélességű MP4-fájlok vagy többféle sávszélességű Smooth Streaming-forrásfájlokat tartalmazó objektumot kell. Szükség tooconfigure hello objektumtovábbítási szabályzat hello eszköz (lásd a témakör későbbi részében). Majd a streamelési URL-cím hello hello formátummegadás alapján, hello Igényalapú Streamelési kiszolgáló biztosítja, hogy hello adatfolyam a rendszer a kiválasztott hello protokollal. Ennek eredményeképpen csak akkor kell toostore és fizetési hello fájlokat egyetlen tárolási formátumban és a Media Services szolgáltatás elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ hello.

Ez a témakör, amely védett médiafájlok továbbításával foglalkoznak hasznos toodevelopers lenne. hello a témakör bemutatja, hogyan tooconfigure hello-e kulcs kézbesítési szolgáltatás vonatkozó szabályzatokat úgy, hogy csak az arra jogosult ügyfelek kaphassák meg hello titkosítási kulcsokat. Azt is bemutatja, hogyan toouse a dinamikus titkosítás.


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>AES-128, a dinamikus titkosítás és a kulcs kézbesítési szolgáltatás munkafolyamat

Az alábbiakban hello általános lépéseket, hogy kell tooperform AES, hello Media Services kulcs szállítási szolgáltatás használatával, és a dinamikus titkosítás használata az eszközök titkosításához.

1. [Hozzon létre egy eszközt, majd fájlok feltöltése hello objektumba](media-services-protect-with-aes128.md#create_asset).
2. [Hello fájl toohello adaptív sávszélességű MP4-készletet tartalmazó hello objektum kódolása](media-services-protect-with-aes128.md#encode_asset).
3. [Hozzon létre egy tartalomkulcsot, és azt társítsa a kódolt hello eszköz](media-services-protect-with-aes128.md#create_contentkey). A Media Services szolgáltatásban a hello tartalomkulcs tartalmazza a hello objektum titkosítási kulcsát.
4. [Hello tartalomkulcs hitelesítési szabályzatának konfigurálása](media-services-protect-with-aes128.md#configure_key_auth_policy). hello tartalomkulcs-hitelesítési szabályzatot kell állította be, és ahhoz, hogy hello tartalom kulcs toobe kézbesített toohello ügyfél hello ügyfél teljesíti.
5. [Az objektum továbbítási szabályzatát hello konfigurálása](media-services-protect-with-aes128.md#configure_asset_delivery_policy). hello továbbítási szabályzat konfigurációjához tartalmazza: licenckérési URL-cím és-inicializálási vektor (IV) (az AES-128 szükséges hello azonos IV toobe megadott titkosításához és visszafejtéséhez), objektumtovábbítási protokoll (például MPEG DASH, HLS, Smooth Streaming vagy az összes), hello típusa a dinamikus titkosítás (például a boríték vagy a dinamikus titkosítás nélkül).

    Tudta alkalmazni a különböző házirend tooeach protokollt a következő hello azonos eszköz. Beállíthat például PlayReady titkosítási tooSmooth/DASH és AES Envelope tooHLS. Nincsenek megadva a továbbítási szabályzatban protokollok (például hozzáadhat egy szabályzatban, amely csak HLS hello protokoll) le lesz tiltva streaming. hello kivétel toothis jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot egyáltalán. Ezt követően hello törölje a jelet minden protokoll engedélyezett.

6. [Hozzon létre egy OnDemand-kereső](media-services-protect-with-aes128.md#create_locator) a rendezés tooget egy adatfolyam-továbbítási URL-címet.

hello témakör is látható [hogyan ügyfélalkalmazás is kérhet egy kulcs hello kulcs kézbesítési szolgáltatás](media-services-protect-with-aes128.md#client_request).

A teljes .NET található [példa](media-services-protect-with-aes128.md#example) hello hello témakör végén.

a következő kép hello hello fentiekben leírt munkafolyamatot mutatja be. Itt hello jogkivonat-hitelesítéshez használt.

![Védelem 128 bites AES-titkosítással](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Ez a témakör többi hello részletes magyarázatokat, kódmintákat és olyan hivatkozásokat tootopics, amelyek bemutatják, hogyan tooachieve hello a fent leírt biztosít.

## <a name="current-limitations"></a>Aktuális korlátozások
Objektumtovábbítási szabályzat hozzáadásakor vagy módosításakor törölnie kell az ahhoz tartozó meglévő lokátort (ha van), majd létre kell hoznia egy új lokátort.

## <a id="create_asset"></a>Hozzon létre egy eszközt, majd fájlok feltöltése hello objektumba
A sorrend toomanage, kódolásához és streameléséhez a videók, akkor először fel kell töltenie a tartalom a Microsoft Azure Media Services. A feltöltést követően a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő. 

További információk: [Upload Files into a Media Services account](media-services-dotnet-upload-files.md) (Fájlok feltöltése a Media Services-fiókba).

## <a id="encode_asset"></a>Hello eszköz tartalmazó hello fájl toohello adaptív sávszélességű MP4 típusú beállításkészlettel kódolása
A dinamikus titkosítás szüksége toocreate többszörös sávszélességű MP4-fájlok vagy többféle sávszélességű Smooth Streaming-forrásfájlokat tartalmazó objektumot. Majd hello hello jegyzékben megadott formátumnak alapján kérelem darabolható, vagy igény szerinti adatfolyam-kiszolgáló biztosítja a megjelenő hello adatfolyam a kiválasztott protokollal hello hello. Ennek eredményeképpen csak akkor kell toostore és fizetési hello fájlokat egyetlen tárolási formátumban és a Media Services szolgáltatás elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ hello. További információkért lásd: hello [dinamikus becsomagolás áttekintése](media-services-dynamic-packaging-overview.md) témakör.

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. 
>
>Emellett toobe képes toouse dinamikus csomagolás és a dinamikus titkosítás az eszköz tartalmaznia kell egy adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá készletét.

Útmutatást tooencode, lásd: [hogyan tooencode keresztül Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a id="create_contentkey"></a>Hozzon létre egy tartalomkulcsot, és azt társítsa a kódolt hello eszköz
A Media Services szolgáltatásban hello tartalomkulcs tartalmazza, amelyet az eszköz tooencrypt hello kulcs együtt.

További információk: [Create content key](media-services-dotnet-create-contentkey.md) (Tartalomkulcs létrehozása).

## <a id="configure_key_auth_policy"></a>Hello tartalomkulcs hitelesítési szabályzatának konfigurálása
A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat. hello tartalomkulcs-hitelesítési szabályzatot kell Ön által konfigurált és érheti el, ha hello ügyfél (a lejátszó) ahhoz, hogy hello kulcs toobe toohello ügyfél kézbesíteni. hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: Nyissa meg a, lexikális elem: korlátozás vagy IP-korlátozás.

További információk: [A tartalomkulcs hitelesítési szabályzatának létrehozása](media-services-dotnet-configure-content-key-auth-policy.md).

## <a id="configure_asset_delivery_policy"></a>Objektumtovábbítási szabályzat konfigurálása
Az objektum továbbítási szabályzatát hello konfigurálása. Néhány dolog, amely az eszköz továbbítási szabályzat konfigurációjához hello tartalmazza:

* hello kulcs licenckérési URL-cím. 
* hello inicializálási vektor (IV) toouse hello boríték titkosításhoz. AES-128 hello azonos IV toobe megadni, ha az titkosításához és visszafejtéséhez szükséges. 
* hello objektumtovábbítási protokoll (például MPEG DASH, HLS, Smooth Streaming vagy all).
* a dinamikus titkosítás (például AES envelope) hello típus vagy a dinamikus titkosítás nélkül. 

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

## <a id="client_request"></a>Hogyan is az ügyfél kérhet egy kulcs hello kulcs kézbesítési szolgáltatás?
Hello a korábbi lépésben hello URL-CÍMMEL tooa jegyzékfájl kialakítani. Az ügyfélnek kell tooextract hello szükséges információkat hello streaming rendelés toomake egy kérelem toohello kulcs kézbesítési szolgáltatás a jegyzékfájlt.

### <a name="manifest-files"></a>Fájlok
hello ügyfélnek kell tooextract hello URL-címe (tartalmazó is tartalomkulcsot azonosítója (kid)) értéket hello jegyzékfájlt. hello ügyfél fogják megpróbálni tooget hello titkosítási kulcs hello kulcs kézbesítési szolgáltatásból. hello ügyfél is tooextract hello IV értéket kell, és használja azt a következő kódrészletet hello stream.hello visszafejtéséhez látható hello <Protection> hello Smooth Streaming jegyzékfájl elemet.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

HLS hello esetben hello legfelső szintű jegyzékfájl lebontva szegmens fájlokat. 

Például hello legfelső szintű jegyzékfájl van: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl), és a szegmens fájlnevek listáját tartalmazza.

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Ha hello szegmens fájlok megnyitása szövegszerkesztőben (például http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should tartalmaz #EXT-X-kulcsot, amely azt jelzi, hogy hello fájl titkosítva van.

    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST

>[!NOTE] 
>Ha azt tervezi, tooplay az AES titkosított HLS a Safari című [ebben a blogban](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

### <a name="request-hello-key-from-hello-key-delivery-service"></a>Hello kulcs hello kulcs kézbesítési szolgáltatás kérése

hello következő kód bemutatja, hogyan toosend egy kérelem toohello Media Services kulcs kézbesítési szolgáltatás egy kulcs kézbesítés URI-azonosítóhoz (hello jegyzékfájl kinyert) használata, valamint a jogkivonatot (a jelen témakör nem szolgáltatással kapcsolatban, hogyan tooget egyszerű webes jogkivonatokat kérhetnek a Secure Token Service).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);

        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;

        var response = request.GetResponse();

        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }

        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();

        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }

## <a name="protect-your-content-with-aes-128-using-net"></a>Az AES-128 tartalomvédelemre .NET használatával

### <a name="create-and-configure-a-visual-studio-project"></a>Egy Visual Studio-projekt létrehozása és konfigurálása

1. A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md). 
2. Adja hozzá a következő elemek túl hello**appSettings** az app.config fájlban meghatározott:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <a id="example"></a>Példa

Írja felül a Program.cs fájlban hello kód hello kód itt látható.
 
>[!NOTE]
>A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat. Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek. További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.

Győződjön meg arról, hogy tooupdate változók toopoint toofolders ahol a bemeneti fájlok találhatók.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Security.Cryptography;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace AESDynamicEncryptionAndKeyDeliverySvc
    {
        class Program
        {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // A Uri describing hello issuer of hello token.  
        // Must match hello value in hello token for hello token toobe considered valid.
        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        // hello Audience or Scope of hello token.  
        // Must match hello value in hello token for hello token toobe considered valid.
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

            IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
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

            // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
            // Note, you need toopass hello key id Guid because we specified 
            // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

            //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
            //so you have tooadd it in front of hello token string. 
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
            Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use hello bit.ly/aesplayer Flash player tootest hello URL 
            // (with open authorization policy). 
            // Paste hello URL and click hello Update button tooplay hello video. 
            //
            string URL = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
            Console.WriteLine();
            Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
            Console.WriteLine();

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
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);


            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass tooit hello name of hello 
            // processor toouse for hello specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with hello encoding details, using a string preset.
            // In this case "Adaptive Streaming" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify hello input asset toobe encoded.
            task.InputAssets.Add(asset);
            // Add an output asset toocontain hello results of hello job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means hello output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }

        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }

        static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
        {
            // Create envelope encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                keyId,
                contentKey,
                "ContentKey",
                ContentKeyType.EnvelopeEncryption);

            // Associate hello key with hello asset.
            asset.ContentKeys.Add(key);

            return key;
        }

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

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);

            string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));

            // When configuring delivery policy, you can choose tooassociate it
            // with a key acquisition URL that has a KID appended or
            // or a key acquisition URL that does not have a KID appended  
            // in which case a content key can be reused. 

            // EnvelopeKeyAcquisitionUrl:  contains a key ID in hello key URL.
            // EnvelopeBaseKeyAcquisitionUrl:  hello URL does not contains a key ID

            // hello following policy configuration specifies: 
            // key url that will have KID=<Guid> appended toohello envelope and
            // hello Initialization Vector (IV) toouse for hello envelope encryption.

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
            };

            IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
                AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                assetDeliveryPolicyConfiguration);

            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
        }

        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference toohello streaming manifest file from hello  
            // collection of files in hello asset. 

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                EndsWith(".ism")).
                FirstOrDefault();

            // Create a 30-day readonly access policy. 
            // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
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

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int size)
        {
            byte[] randomBytes = new byte[size];
            using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
            {
            rng.GetBytes(randomBytes);
            }

            return randomBytes;
        }
        }
    }


## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

