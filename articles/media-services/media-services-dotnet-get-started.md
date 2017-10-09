---
title: "aaaGet .NET segítségével igény szerinti tartalomtovábbítás a használatába |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello végrehajtási egy on igény szerinti tartalomtovábbító alkalmazást az Azure Media Services .NET használatával."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 388b8928-9aa9-46b1-b60a-a918da75bd7b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 4ca9394bd581e1d9062e5a008a410b2c058e017e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Tartalmak továbbítása igény szerint a .NET SDK használatával
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Ez az oktatóanyag végigvezeti hello egy alapszintű Video-on-Demand (VoD) tartalomtovábbító service végrehajtási hello Azure Media Services .NET SDK-t használó Azure Media Services (AMS) alkalmazással.

## <a name="prerequisites"></a>Előfeltételek

Az alábbiakban hello szükséges toocomplete hello oktatóanyag:

* Egy Azure-fiók. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* Egy Media Services-fiók. egy Media Services-fiók toocreate lásd [hogyan tooCreate Media Services-fiók](media-services-portal-create-account.md).
* A .NET-keretrendszer 4.0-s vagy újabb verziója.
* Visual Studio.

Ez az oktatóanyag hello a következő feladatokat tartalmazza:

1. Indítsa el az adatfolyam-továbbítási végpontra (hello Azure-portál használatával).
2. Egy Visual Studio-projekt létrehozása és konfigurálása.
3. Csatlakozás a Media Services-fiók toohello.
2. Videofájl feltöltése
3. Hello forrásfájl kódolása adaptív sávszélességű MP4-fájlokat alakítja.
4. Hello objektum közzététele, és a streamelési és a progresszív letöltési URL-cím.  
5. Tartalom lejátszása

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag végigvezeti hello végrehajtási egy Video-on-Demand (VoD) tartalomtovábbító alkalmazást .NET-keretrendszerhez készült Azure Media Services (AMS) SDK használatával.

hello útmutató bemutatja a Media Services alapvető munkafolyamatait hello és hello leggyakoribb programozási objektumokat és a Media Services-fejlesztés szükséges feladatok. A hello hello az oktatóanyag befejezése után képes toostream kell vagy fokozatosan letölteni egy feltöltött, kódolt és letöltött példa médiafájlt lesz.

### <a name="ams-model"></a>AMS-modell

hello következő kép bemutatja a leggyakrabban használt hello objektumok hello Media Services OData modellre VoD-alkalmazások fejlesztése során.

Kattintson a hello kép tooview, teljes méret.  

<a href="./media/media-services-dotnet-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-dotnet-get-started/media-services-overview-object-model-small.png"></a> 

Megtekintheti a teljes minta hello [Itt](https://media.windows.net/API/$metadata?api-version=2.15).  

## <a name="start-streaming-endpoints-using-hello-azure-portal"></a>Indítsa el az adatfolyam-végpontok hello Azure-portál használatával

Ha az Azure Media Services egyik leggyakoribb forgatókönyve hello elkötelezett használatával adatfolyam adaptív sávszélességű streamelést működik. A Media Services dinamikus becsomagolást biztosít, amely lehetővé teszi toodeliver az adaptív sávszélességű MP4-kódolású tartalmak anélkül, hogy előre csomagolt toostore (MPEG DASH, HLS, Smooth Streaming), a Media Services just-in-time, által támogatott streamformátumok Ezekbe a formátumokba egyes verzióit.

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.

toostart hello streamvégpontra, a következő hello:

1. Jelentkezzen be hello [Azure-portálon](https://portal.azure.com/).
2. Hello-beállítások ablakában kattintson a Streaming végpontok.
3. Kattintson a hello alapértelmezett streamvégpontra.

    hello alapértelmezett STREAMING ENDPOINT részletek ablak.

4. Kattintson a hello Start ikonra.
5. Kattintson a hello Mentés gombra toosave a módosításokat.

## <a name="create-and-configure-a-visual-studio-project"></a>Egy Visual Studio-projekt létrehozása és konfigurálása

1. A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md). 
2. Hozzon létre egy új mappát (mappa bárhol lehet a helyi meghajtóról), és szeretné, hogy tooencode és adatfolyam, vagy fokozatosan letölteni egy .mp4 fájlt másolja. Ebben a példában hello "C:\VideoFiles" elérési utat használja.

## <a name="connect-toohello-media-services-account"></a>Csatlakozás a Media Services-fiók toohello

A Media Services használata a .NET, használnia kell a hello **CloudMediaContext** osztály a legtöbb Media Services-programozási feladathoz: tooMedia Services-fiók összekötő; létrehozása, frissítése, elérése és hello következő törlése objektumok: eszközök, eszköz-fájlok, feladatok, hozzáférési házirendek, keresők, stb.

Hello alapértelmezett Program osztályt felülírása a kódját a következő hello. hello kód bemutatja, hogyan tooread hello csatlakozási érték hello App.config fájlból, és hogyan toocreate hello **CloudMediaContext** rendelés tooconnect tooMedia objektum szolgáltatások. További információkért lásd: [csatlakozás a Media Services API toohello](media-services-use-aad-auth-to-access-ams-api.md).

Győződjön meg arról, hogy tooupdate hello fájl neve és elérési toowhere rendelkezik a médiafájl.

Hello **fő** függvény olyan módszereket, amelyek később lesznek meghatározva hív további ebben a szakaszban.

> [!NOTE]
> Akkor lesz kell első fordítási hibák összes hello funkciók definícióit hozzáadásáig.

    class Program
    {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
        try
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Add calls toomethods defined in this section.
            // Make sure tooupdate hello file name and path toowhere you have your media file.
            IAsset inputAsset =
            UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

            IAsset encodedAsset =
            EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

            PublishAssetGetURLs(encodedAsset);
        }
        catch (Exception exception)
        {
            // Parse hello XML error message in hello Media Services response and create a new
            // exception with its content.
            exception = MediaServicesExceptionParser.Parse(exception);

            Console.Error.WriteLine(exception.Message);
        }
        finally
        {
            Console.ReadLine();
        }
        }
    }

## <a name="create-a-new-asset-and-upload-a-video-file"></a>Új adategység létrehozása és videofájl feltöltése

A Media Services szolgáltatásban a digitális fájlok feltöltése vagy kimenete egy adategységbe történik. Hello **eszköz** entitás tartalmazhat videót, hang, képek, miniatűröket, szöveges nyomon követi, és feliratfájlokat fájlok (és mindezen fájlok metaadatait hello.)  Miután hello fájlok feltöltése után a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő. hello eszköz hello fájlok nevezzük **adategység-fájloknak**.

Hello **UploadFile** hívások az alábbiakban meghatározott metódus **CreateFromFile** (.NET SDK-bővítmények meghatározott). **CreateFromFile** létrehoz egy új adategységet, mely hello a megadott forrásfájl fel lesz töltve.

Hello **CreateFromFile** metódus **AssetCreationOptions** amellyel meg kell adni hello alábbi adategység-létrehozási lehetőségek egyikét:

* **Nincs** – Nincs titkosítás. Ez az alapértelmezett érték hello. Ügyeljen arra, hogy ezen lehetőség használatakor a tartalom sem átvitel, sem tárolás közben nincs védve.
  Ha egy MP4 toodeliver fájlt progresszív letöltés útján tervez, használja ezt a beállítást.
* **StorageEncrypted** -használja ezt a beállítást tooencrypt a tiszta tartalom helyileg titkosítással Advanced Encryption Standard (AES)-256 bit, amely tooAzure helyén tárolás titkosítása feltöltését. Storage-titkosítással védett adategységek automatikusan titkosítás és egy titkosított fájl rendszer előzetes tooencoding, és ha szükséges újra titkosítani előzetes toouploading egy új kimeneti eszközként helyezve. a tárolás titkosítása hello elsődleges használati eset az, amikor toosecure a kiváló minőségű bemeneti médiafájljait erős titkosítással aktívan a lemezen.
* **CommonEncryptionProtected** – Használja ezt a lehetőséget, ha olyan tartalmat tölt fel, amely már korábban titkosítva és védve lett általános titkosítás vagy a PlayReady DRM által (például egy PlayReady DRM titkosítással védett Smooth Streaming-fájlt).
* **EnvelopeEncryptionProtected** – Használja ezt a lehetőséget, ha AES által titkosított HLS tartalmakat tölt fel. Vegye figyelembe, hogy hello fájlokat kell kódolni és titkosítani Transform Manager használatával.

Hello **CreateFromFile** módszert is lehetővé válik annak a visszahívás rendelés tooreport hello feltöltési folyamatáról hello fájl.

A következő példa hello, azt adja meg a **nincs** hello eszköz lehetőségek.

Adja hozzá a következő metódus toohello Program osztály hello.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


## <a name="encode-hello-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Hello forrásfájl kódolása adaptív sávszélességű MP4-fájlsorozattá készletére
Miután az adategységek bevitele a Media Services, az adathordozó lehet kódolású transmuxed teljesítményjellemzőit, és így tovább, mielőtt továbbítva lennének tooclients. Ezek a tevékenységek ütemezett, és több háttér szerepkör példányok tooensure nagy teljesítményt és rendelkezésre állás futtatni. Ezeket a tevékenységeket feladatoknak nevezzük, és minden feladat áll hello hello objektumfájlt valódi munkát atomi feladatokhoz.

Mivel korábban már említettük, az Azure Media Services használatakor, egyik leggyakoribb forgatókönyve hello elkötelezett az adaptív sávszélességű streamelési tooyour ügyfelek. A Media Services tudja dinamikusan csomagolni adaptív sávszélességű MP4-fájlok készlete hello a következő formátumok egyikét: HTTP Live Streaming (HLS), Smooth Streaming vagy MPEG DASH.

tootake előny dinamikus becsomagolás tooencode vagy szükséges alakítható át a mezzanine (forrás) fájlt az adaptív sávszélességű MP4-vagy adaptív sávszélességű Smooth Streaming-fájlsorozattá.  

hello a következő kód bemutatja, hogyan kódolással toosubmit feladat. hello feladat egyetlen műveletet tartalmaz, amely meghatározza a tootranscode hello mezzazine-fájlt használatával adaptív sávszélességű MP4 készletére **Media Encoder Standard**. hello kód elküldi hello feladatot, és vár, amíg az befejeződik.

Ha hello feladat befejeződött, lenne képes toostream lehet az objektumot, vagy fokozatosan letölteni az átkódolás során létrejött MP4-fájlok.

Adja hozzá a következő metódus toohello Program osztály hello.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {

        // Prepare a job with a single task tootranscode hello specified asset
        // into a multi-bitrate asset.

        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "Adaptive Streaming",
            asset,
            "Adaptive Bitrate MP4",
            options);

        Console.WriteLine("Submitting transcoding job...");


        // Submit hello job and wait until it is completed.
        job.Submit();

        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;

        Console.WriteLine("Transcoding job finished.");

        IAsset outputAsset = job.OutputMediaAssets[0];

        return outputAsset;
    }

## <a name="publish-hello-asset-and-get-urls-for-streaming-and-progressive-download"></a>Hello objektum közzététele és az adatfolyam-továbbításhoz és progresszív letöltési URL-címek lekérése

toostream vagy egy eszköz letöltése, akkor először kell túl "közzététele" egy kereső létrehozásával. Lokátorok biztosítanak hozzáférést toofiles hello eszköz szerepel. Media Services két lokátortípust támogat: OnDemandOrigin keresők használt toostream media (például MPEG DASH, HLS vagy Smooth Streaming) és a hozzáférési aláírás (SAS) lokátortípus, használt toodownload médiafájlok (vonatkozó további információ a SAS-keresők lásd: [ez](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog).

### <a name="some-details-about-url-formats"></a>Néhány információ az URL-formátumokról

Hello keresők létrehozása után hozhat létre hello URL-címeket kell használt toostream vagy töltse le a fájlokat. Ebben az oktatóanyagban hello minta fog kimeneti illessze be a megfelelő böngészőkben URL-címeket. Ez a szakasz néhány rövid példán mutatja be a különféle formátumokat.

#### <a name="a-streaming-url-for-mpeg-dash-has-hello-following-format"></a>MPEG DASH-továbbítási egy URL-formátum a következő hello rendelkezik:

{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest**(format=mpd-time-csf)**

#### <a name="a-streaming-url-for-hls-has-hello-following-format"></a>Egy HLS streamelési URL-címet a következő formátumban hello rendelkezik:

{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest**(format=m3u8-aapl)**

#### <a name="a-streaming-url-for-smooth-streaming-has-hello-following-format"></a>Smooth Streaming egy adatfolyam-továbbítási URL-címnek a hello a következő formátumban:

{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest


#### <a name="a-sas-url-used-toodownload-files-has-hello-following-format"></a>A használt SAS URL-cím toodownload fájlok hello formátuma a következő esetében:

{blob-tároló neve}/{adategység neve}/{fájlnév}/{SAS-aláírás}

Media Services .NET SDK-bővítmények adja meg, hogy visszatérési URL-címek formázva hello kényelmes segédmódszereket közzétett objektumnak.

hello következő használ a .NET SDK-bővítmények toocreate keresők és tooget streaming és a progresszív letöltési URL-cím. hello kód azt is bemutatja, hogyan toodownload fájlok tooa helyi mappát.

Adja hozzá a következő metódus toohello Program osztály hello.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish hello output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get hello Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and hello Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get hello URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  hello streaming URLs.
        Console.WriteLine("Use hello following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display hello URLs for progressive download.
        Console.WriteLine("Use hello following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download hello output asset tooa local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files tooa local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

## <a name="test-by-playing-your-content"></a>A tartalom lejátszhatóságának tesztelése

Hello előző szakaszban meghatározott hello program futtatása után hello hasonló toohello következő jelenik meg a konzolablakban hello URL-címeket.

Adaptív adatfolyam-továbbítási URL-címek:

Smooth Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG DASH

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Fokozatos letöltési URL-címek (audió és videó).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


toostream a videót, illessze be az URL-cím hello URL-cím textbox hello [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

tootest progresszív letöltés, illessze be egy URL-címet a böngészőjébe (például az Internet Explorer, Chrome vagy Safari).

További információkért tekintse meg a következő témakörök hello:

- [A tartalom lejátszása meglévő lejátszókkal](media-services-playback-content-with-existing-players.md)
- [Videolejátszó alkalmazások fejlesztése](media-services-develop-video-players.md)
- [MPEG-DASH adaptív streamelt videók beágyazása DASH.js-sel rendelkező HTML5-alkalmazásba](media-services-embed-mpeg-dash-in-html5.md)

## <a name="download-sample"></a>Minta letöltése
hello alábbi kódminta hello kódot tartalmaz, amely ebben az oktatóanyagban létrehozott: [minta](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



<!-- Anchors. -->


<!-- URLs. -->
[Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
[Portal]: http://manage.windowsazure.com/
