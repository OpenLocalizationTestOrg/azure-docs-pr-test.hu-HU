---
title: "egy Media Services-fiók használata a .NET aaaUpload fájlok |} Microsoft Docs"
description: "Ismerje meg, hogyan tooget multimédiás tartalom a Media Services létrehozásával és feltöltésével eszközök."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c9c86380-9395-4db8-acea-507c52066f73
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2017
ms.author: juliako
ms.openlocfilehash: 11c8a359b09efe04b54490fd48ac0cd7c366f8b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-net"></a>Fájlok feltöltése a Media Services-fiók .NET használatával
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Portál](media-services-portal-upload-files.md)
> 
> 

A Media Services szolgáltatásban a digitális fájlok feltöltése vagy kimenete egy adategységbe történik. Hello **eszköz** entitás tartalmazhat videót, hang, képek, miniatűröket, szöveg nyomon követi és feliratfájlokat fájlokat (és mindezen fájlok metaadatait hello.)  Miután hello fájlok feltöltése után a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.

hello eszköz hello fájlok nevezzük **adategység-fájloknak**. Hello **AssetFile** példány és a hello tényleges media fájl is két különböző objektum. hello AssetFile példány hello media fájlról metaadatot tartalmaz, amíg hello médiafájl tartalmaz hello tényleges médiatartalmakat.

> [!NOTE]
> a következő szempontok hello vonatkoznak:
> 
> * Media Services hello hello IAssetFile.Name tulajdonság értékét használja, amikor a hello adatfolyam-tartalmat (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) URL-címek kiépítéséhez Emiatt százalék-kódolás nem engedélyezett. hello értékének hello **neve** tulajdonság nem lehet hello következő [százalék kódolás-fenntartott karakterek](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Emellett csak lehet egy "." hello fájlnévkiterjesztés.
> * hello hello nevének hossza nem lehet hosszabb 260 karakternél.
> * Nincs a Media Services feldolgozás támogatott korlátot toohello fájl maximális méretét. Ellenőrizze a [ez](media-services-quotas-and-limitations.md) témakör hello méretű fájlt választhat vonatkozó további információért.
> * A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat. Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek. További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.
> 

Eszközök létrehozásakor az alábbi titkosítási beállítások hello is megadhat. 

* **Nincs** – Nincs titkosítás. Ez az alapértelmezett érték hello. Vegye figyelembe, hogy ez a beállítás használatakor a tartalom nem védett átvitel, sem tárolás közben.
  Ha egy MP4 toodeliver fájlt progresszív letöltés útján tervez, használja ezt a beállítást. 
* **CommonEncryption** -használja ezt a beállítást, ha már titkosítva és általános titkosítás vagy a PlayReady DRM által (például védett Smooth Streaming egy PlayReady DRM) védett tartalmat.
* **EnvelopeEncrypted** – használja ezt a beállítást, ha AES által titkosított HLS. Vegye figyelembe, hogy hello fájlokat kell kódolni és titkosítani Transform Manager használatával.
* **StorageEncrypted** - titkosítja a tiszta tartalom helyileg az AES-256 bites titkosítás használata és tooAzure helyén tárolás titkosítása feltöltését. Storage-titkosítással védett adategységek automatikusan titkosítás és egy titkosított fájl rendszer előzetes tooencoding, és ha szükséges újra titkosítani előzetes toouploading egy új kimeneti eszközként helyezve. hello elsődleges használati eset a tárolás titkosítása akkor, ha azt szeretné, hogy a jó minőségű bemeneti médiafájljait erős titkosítással, rest-lemezen toosecure.
  
    A Media Services biztosít az eszközök, nem over tömörített digitális Manager (DRM) például a lemezen tárolás titkosítása.
  
    Ha az adategységen tárolótitkosítást alkalmaz, konfigurálnia kell az adategység továbbítási házirendjét. További információ: [objektumtovábbítási szabályzat konfigurálása](media-services-dotnet-configure-asset-delivery-policy.md).

Ha hogy adja meg az eszköz toobe titkosítva egy **CommonEncrypted** lehetőséget, vagy egy **EnvelopeEncypted** lehetőséget, akkor tooassociate az objektum egy **ContentKey**. További információkért lásd: [hogyan toocreate egy ContentKey](media-services-dotnet-create-contentkey.md). 

Ha hogy adja meg az eszköz toobe titkosítva egy **StorageEncrypted** lehetőségre, a Media Services SDK hello a .NET hoz létre egy **StorateEncrypted** **ContentKey** a a eszköz.

Ez a témakör bemutatja, hogyan toouse Media Services .NET SDK, valamint a Media Services .NET SDK bővítmények tooupload fájlok egy Media Services-objektumba.

## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Media Services .NET SDK-val egy fájl feltöltése
az alábbi hello mintakód .NET SDK tooupload akár egyetlen fájlt használja. hello AccessPolicy és lokátor létrehozása és hello feltöltés függvény megsemmisül. 


        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }


## <a name="upload-multiple-files-with-media-services-net-sdk"></a>Media Services .NET SDK-val több fájl feltöltése
a következő kód bemutatja hogyan hello toocreate egy eszköz és a több fájl feltöltése.

hello kód hello a következő:

* Létrehoz egy üres eszköz metódussal hello CreateEmptyAsset hello előző lépésben meghatározott.
* Létrehoz egy **AccessPolicy** -példányt, hello engedélyekkel és hozzáférési toohello eszköz időtartamát határozza meg.
* Létrehoz egy **lokátor** toohello eszköz hozzáférést biztosító példány.
* Létrehoz egy **BlobTransferClient** példány. Ez a típus jelképezi ügyfél, amely az Azure-blobok hello működik. Ebben a példában használjuk hello ügyfél toomonitor hello feltöltési folyamatáról. 
* Hello megadott könyvtárban található fájlok keresztül enumerálása, és létrehoz egy **AssetFile** -példány minden fájlt.
* Feltöltések hello fájlok a Media Services segítségével hello **UploadAsync** metódust. 

> [!NOTE]
> Hello UploadAsync metódus tooensure hello hívások nem korlátozzák, és hello fájlok feltöltése párhuzamosan használható.
> 
> 

        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended toovalidate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading hello files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }

    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as hello upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Eszközök nagy számú feltöltését, vegye figyelembe hello következő.

* Hozzon létre egy új **CloudMediaContext** szálankénti objektum. Hello **CloudMediaContext** osztály nincs többszálú futtatásra.
* Növelje a NumberOfConcurrentTransfers hello alapértelmezett érték 2 tooa magasabb érték 5 hasonlóan. A tulajdonság beállítása hatással van az összes példányát **CloudMediaContext**. 
* Tartsa ParallelTransferThreadCount 10 hello alapértelmezett értéket.

## <a id="ingest_in_bulk"></a>A Media Services .NET SDK használatával tömeges választásával dolgozhat fel eszközök
Nagy eszköz fájlok feltöltése lehet a szűk keresztmetszetek eszköz létrehozása során. Választásával dolgozhat fel eszközök tömeges vagy a "tömeges választásával dolgozhat" fel, magában foglalja a leválasztásával eszköz létrehozása hello feltöltési folyamat során. toouse egy tömeges ingesting módszert használja, hozzon létre a jegyzékfájl (IngestManifest), amely hello eszköz és az ahhoz tartozó fájlokat ismerteti. Hello fájlfeltöltési módszer a választott tooupload hello társított fájlokat toohello jegyzék blob tároló használni. A Microsoft Azure Media Services hello blob tároló hello jegyzékfájl társított figyeli. Amikor egy fájl feltöltött toohello blob tároló, a Microsoft Azure Media Services befejezése hello eszköz létrehozása hello konfiguráció hello eszköz hello jegyzékfájlban (IngestManifestAsset) alapján.

egy új IngestManifest toocreate metódushívás hello létrehozása hello hello CloudMediaContext IngestManifests gyűjtemény által elérhetővé tett tárolókra. Ezzel a módszerrel hoz létre egy új IngestManifest hello manifest-nevet.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Hozzon létre hello eszközök, amelyek az hello tömeges IngestManifest lesz társítva. Konfigurálja a szükséges hello titkosítási beállításokat hello eszköz tömeges választásával dolgozhat fel.

    // Create hello assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

Egy IngestManifestAsset egy eszköz társít egy tömeges IngestManifest tömeges választásával dolgozhat fel. Azt is, amelyek minden eszköz AssetFiles hello társítja. egy IngestManifestAsset toocreate módszert hello létrehozás hello kiszolgáló környezetben.

hello következő példa bemutatja, hozzáadását két új IngestManifestAssets társító hello két eszközök korábban létrehozott toohello tömeges betöltési jegyzékben. Minden IngestManifestAsset is hozzárendeli a minden eszköz lesz feltöltve fájlokat alatt tömeges választásával dolgozhat fel.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });

Használhatja a nagy sebességű ügyfélalkalmazás képes hello eszköz fájlok toohello blob storage tárolót hello által biztosított URI feltöltése **IIngestManifest.BlobStorageUriForUpload** hello IngestManifest tulajdonsága. Egy figyelmet a jelentősebb nagy sebességű feltöltési szolgáltatás [Aspera igény szerinti Azure alkalmazáshoz](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Is írhat kódot tooupload hello eszközök fájlok ahogy az alábbi kódpéldát hello.

    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);

            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);

            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);

            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });

        copytask.Start();
    }

hello hello adategység-fájloknak a jelen témakörben használt hello minta feltöltése kód az alábbi kódpéldát hello.

    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);


Azt is meghatározhatja, hello tömeges választásával dolgozhat fel társított összes eszköz hello előrehaladását egy **IngestManifest** hello hello statisztika tulajdonságának lekérdezésével **IngestManifest**. A sorrend tooupdate végrehajtási adatok, kell használnia egy új **CloudMediaContext** minden alkalommal, amikor lekérdezi hello statisztika tulajdonság.

hello következő példa bemutatja egy IngestManifest által lekérdezési a **azonosító**.

    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();

          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);

                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }

             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }



## <a name="upload-files-using-net-sdk-extensions"></a>Töltse fel a fájlokat a .NET SDK-bővítmények
hello az alábbi példa bemutatja, hogyan tooupload egy egyetlen fájl .NET SDK-bővítmények használatával. Ebben az esetben hello **CreateFromFile** módszert használ, de hello aszinkron verzióját is rendelkezésre áll (**CreateFromFileAsync**). Hello **CreateFromFile** módszer lehetővé teszi a adja meg a hello fájlnév, a titkosítási beállítás és a sorrend tooreport hello visszahívás feltöltése hello fájl előrehaladását.

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

hello alábbi példa UploadFile függvényt és a tárolás titkosítása hello eszköz létrehozására beállítást adja meg.  

    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);

## <a name="next-steps"></a>Következő lépések

Most már kódolhatja a feltöltött adategységeket. További információ: [Encode Assets](media-services-portal-encode.md) (Adategységek kódolása).

Használhatja az Azure Functions tootrigger egy kódolási feladat, a konfigurált hello tároló érkező fájl alapján. További információkért tekintse meg [ezt a mintát](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Következő lépés
Most, hogy egy eszköz már feltöltött tooMedia szolgáltatások, go toohello [hogyan tooGet Media processzort] [ How tooGet a Media Processor] témakör.

[How tooGet a Media Processor]: media-services-get-media-processor.md

