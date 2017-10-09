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
# <a name="how-to-deliver-an-asset-by-download"></a><span data-ttu-id="b6959-104">Útmutató: egy eszköz letöltés biztosításához</span><span class="sxs-lookup"><span data-stu-id="b6959-104">How to: Deliver an Asset by Download</span></span>
<span data-ttu-id="b6959-105">Ebben a témakörben ismertetett beállítások, az adathordozó eszközök kézbesítéséhez feltöltött tooMedia szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b6959-105">This topic discusses options for delivering media assets uploaded tooMedia Services.</span></span> <span data-ttu-id="b6959-106">Számos alkalmazás-forgatókönyveket a Media Services tartalom biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="b6959-106">You can deliver Media Services content in numerous application scenarios.</span></span> <span data-ttu-id="b6959-107">Töltse le az adathordozó eszközök, vagy egy lokátor segítségével elérhet.</span><span class="sxs-lookup"><span data-stu-id="b6959-107">You can download media assets, or access them by using a locator.</span></span> <span data-ttu-id="b6959-108">Tartalom tooanother médiaalkalmazás vagy tooanother tartalomszolgáltató küldhet.</span><span class="sxs-lookup"><span data-stu-id="b6959-108">You can send media content tooanother application or tooanother content provider.</span></span> <span data-ttu-id="b6959-109">Jobb teljesítmény és méretezhetőség is biztosíthat tartalmat egy Content Delivery Network (CDN) használatával.</span><span class="sxs-lookup"><span data-stu-id="b6959-109">For improved performance and scalability, you can also deliver content by using a Content Delivery Network (CDN).</span></span>

<span data-ttu-id="b6959-110">A példa bemutatja, hogyan toodownload adathordozó eszközök Media Services tooyour helyi számítógépről.</span><span class="sxs-lookup"><span data-stu-id="b6959-110">This example shows how toodownload media assets from Media Services tooyour local computer.</span></span> <span data-ttu-id="b6959-111">hello kód lekérdezések hello Feladatazonosító és hozzáférések hello Media Services-fiókjához társított feladatok a **OutputMediaAssets** gyűjtemény (amely hello készlete egy vagy több kimeneti adathordozó eszközök által a feladat futtatásával).</span><span class="sxs-lookup"><span data-stu-id="b6959-111">hello code queries hello jobs associated with hello Media Services account by job ID and accesses its **OutputMediaAssets** collection (which is hello set of one or more output media assets that results from running a job).</span></span> <span data-ttu-id="b6959-112">Ez a példa bemutatja, hogyan toodownload kimeneti adathordozó eszközök egy feladatot, de alkalmazhat hello azonos megközelítés toodownload más eszközök.</span><span class="sxs-lookup"><span data-stu-id="b6959-112">This  example shows how toodownload output media assets from a job, but you can apply hello same approach toodownload other assets.</span></span>

>[!NOTE]
><span data-ttu-id="b6959-113">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="b6959-113">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="b6959-114">Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek.</span><span class="sxs-lookup"><span data-stu-id="b6959-114">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="b6959-115">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="b6959-115">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

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



## <a name="media-services-learning-paths"></a><span data-ttu-id="b6959-116">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="b6959-116">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b6959-117">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="b6959-117">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="b6959-118">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b6959-118">See Also</span></span>
[<span data-ttu-id="b6959-119">Adatfolyam-továbbítási tartalom</span><span class="sxs-lookup"><span data-stu-id="b6959-119">Deliver streaming content</span></span>](media-services-deliver-streaming-content.md)

