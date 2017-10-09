---
title: "aaaDownload Media Services eszközök tooyour számítógép - Azure |} Microsoft Docs"
description: "További információk a toodownload eszközök tooyour számítógép. Kódminták C# nyelven íródtak, és a Media Services SDK hello használata a .NET-hez."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 8908a1dd-3ffb-4f18-955d-4c8e2d82fc5d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 6c6e764720caa59d8371178a2682700345f7bc57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-deliver-an-asset-by-download"></a>Útmutató: egy eszköz letöltés biztosításához
Ebben a témakörben ismertetett beállítások, az adathordozó eszközök kézbesítéséhez feltöltött tooMedia szolgáltatások. Számos alkalmazás-forgatókönyveket a Media Services tartalom biztosíthat. Töltse le az adathordozó eszközök, vagy egy lokátor segítségével elérhet. Tartalom tooanother médiaalkalmazás vagy tooanother tartalomszolgáltató küldhet. Jobb teljesítmény és méretezhetőség is biztosíthat tartalmat egy Content Delivery Network (CDN) használatával.

A példa bemutatja, hogyan toodownload adathordozó eszközök Media Services tooyour helyi számítógépről. hello kód lekérdezések hello Feladatazonosító és hozzáférések hello Media Services-fiókjához társított feladatok a **OutputMediaAssets** gyűjtemény (amely hello készlete egy vagy több kimeneti adathordozó eszközök által a feladat futtatásával). Ez a példa bemutatja, hogyan toodownload kimeneti adathordozó eszközök egy feladatot, de alkalmazhat hello azonos megközelítés toodownload más eszközök.

>[!NOTE]
>A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat. Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek. További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.

    // Download hello output asset of hello specified job tooa local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how toodownload a single asset. 
        // However, you can iterate through hello OutputAssets
        // collection, and download all assets if there are many. 

        // Get a reference toohello job. 
        IJob job = GetJob(jobId);

        // Get a reference toohello first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];

        // Create a SAS locator toodownload hello asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);

        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };

        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use hello following event handler toocheck download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;

            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);

            Console.WriteLine("File download path:  " + localDownloadPath);

            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));

            outputFile.DownloadProgressChanged -= DownloadProgress;
        }

        Task.WaitAll(downloadTasks.ToArray());

        return outputAsset;
    }

    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Lásd még:
[Adatfolyam-továbbítási tartalom](media-services-deliver-streaming-content.md)

