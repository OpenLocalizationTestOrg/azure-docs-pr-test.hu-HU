---
title: "Töltse le a számítógép - Azure Media Services eszközök |} Microsoft Docs"
description: "Ismerje meg a kívánt eszközök letöltése a számítógépre. Kódminták C# nyelven íródtak, és a Media Services SDK használata a .NET-hez."
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
ms.openlocfilehash: d8e740e969f68c85842f42c109328423da1b4414
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-deliver-an-asset-by-download"></a><span data-ttu-id="0ab40-104">Útmutató: egy eszköz letöltés biztosításához</span><span class="sxs-lookup"><span data-stu-id="0ab40-104">How to: Deliver an Asset by Download</span></span>
<span data-ttu-id="0ab40-105">Ebben a témakörben ismertetett beállítások tölteni a Media Services adathordozó eszközök kézbesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0ab40-105">This topic discusses options for delivering media assets uploaded to Media Services.</span></span> <span data-ttu-id="0ab40-106">Számos alkalmazás-forgatókönyveket a Media Services tartalom biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="0ab40-106">You can deliver Media Services content in numerous application scenarios.</span></span> <span data-ttu-id="0ab40-107">Töltse le az adathordozó eszközök, vagy egy lokátor segítségével elérhet.</span><span class="sxs-lookup"><span data-stu-id="0ab40-107">You can download media assets, or access them by using a locator.</span></span> <span data-ttu-id="0ab40-108">A médiatartalom küldhet egy másik alkalmazás vagy egy másik szolgáltatóhoz.</span><span class="sxs-lookup"><span data-stu-id="0ab40-108">You can send media content to another application or to another content provider.</span></span> <span data-ttu-id="0ab40-109">Jobb teljesítmény és méretezhetőség is biztosíthat tartalmat egy Content Delivery Network (CDN) használatával.</span><span class="sxs-lookup"><span data-stu-id="0ab40-109">For improved performance and scalability, you can also deliver content by using a Content Delivery Network (CDN).</span></span>

<span data-ttu-id="0ab40-110">Ez a példa bemutatja, hogyan adathordozó eszközök letölteni a Media Services a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0ab40-110">This example shows how to download media assets from Media Services to your local computer.</span></span> <span data-ttu-id="0ab40-111">A kód lekéri a Feladatazonosító és fér hozzá a Media Services-fiókjához társított feladatok a **OutputMediaAssets** gyűjtemény (amely a készlet egy vagy több kimeneti adathordozó eszközök fut egy feladat annak az eredménye).</span><span class="sxs-lookup"><span data-stu-id="0ab40-111">The code queries the jobs associated with the Media Services account by job ID and accesses its **OutputMediaAssets** collection (which is the set of one or more output media assets that results from running a job).</span></span> <span data-ttu-id="0ab40-112">A példa bemutatja, hogyan töltheti le kimeneti adathordozó eszközök egy feladatot, de ugyanezt a megközelítést, más eszközök letöltésére is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="0ab40-112">This  example shows how to download output media assets from a job, but you can apply the same approach to download other assets.</span></span>

>[!NOTE]
><span data-ttu-id="0ab40-113">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="0ab40-113">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="0ab40-114">Ha mindig ugyanazokat a napokat/hozzáférési engedélyeket használja (például olyan keresők szabályzatait, amelyek hosszú ideig érvényben maradnak, vagyis nem feltöltött szabályzatokat), a szabályzatazonosítónak is ugyanannak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0ab40-114">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="0ab40-115">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="0ab40-115">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 

        // Get a reference to the job. 
        IJob job = GetJob(jobId);

        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];

        // Create a SAS locator to download the asset
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
            // Use the following event handler to check download progress.
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



## <a name="media-services-learning-paths"></a><span data-ttu-id="0ab40-116">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="0ab40-116">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0ab40-117">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="0ab40-117">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="0ab40-118">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0ab40-118">See Also</span></span>
[<span data-ttu-id="0ab40-119">Adatfolyam-továbbítási tartalom</span><span class="sxs-lookup"><span data-stu-id="0ab40-119">Deliver streaming content</span></span>](media-services-deliver-streaming-content.md)

