---
title: "Fájlok feltöltése a Media Services-fiók használata a .NET |} Microsoft Docs"
description: "Útmutató a médiatartalom feltölti a Media Services létrehozásával és feltöltésével eszközök."
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
ms.openlocfilehash: ec8c1da633374ba684f6a0a895c542ee76ef73b8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-into-a-media-services-account-using-net"></a><span data-ttu-id="76c79-103">Fájlok feltöltése a Media Services-fiók .NET használatával</span><span class="sxs-lookup"><span data-stu-id="76c79-103">Upload files into a Media Services account using .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="76c79-104">.NET</span><span class="sxs-lookup"><span data-stu-id="76c79-104">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="76c79-105">REST</span><span class="sxs-lookup"><span data-stu-id="76c79-105">REST</span></span>](media-services-rest-upload-files.md)
> * [<span data-ttu-id="76c79-106">Portal</span><span class="sxs-lookup"><span data-stu-id="76c79-106">Portal</span></span>](media-services-portal-upload-files.md)
> 
> 

<span data-ttu-id="76c79-107">A Media Services szolgáltatásban a digitális fájlok feltöltése vagy kimenete egy adategységbe történik.</span><span class="sxs-lookup"><span data-stu-id="76c79-107">In Media Services, you upload (or ingest) your digital files into an asset.</span></span> <span data-ttu-id="76c79-108">A **eszköz** entitás tartalmazhat videót, hang, képek, miniatűröket, szöveg nyomon követi és feliratfájlokat fájlokat (és a mindezen fájlok metaadatait.)  A fájlok feltöltése után a tartalom a felhőben lesz biztonságosan tárolva további feldolgozás és adatfolyam-továbbítás céljából.</span><span class="sxs-lookup"><span data-stu-id="76c79-108">The **Asset** entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and the metadata about these files.)  Once the files are uploaded, your content is stored securely in the cloud for further processing and streaming.</span></span>

<span data-ttu-id="76c79-109">Az adategységben található fájlokat **adategység-fájloknak** nevezzük.</span><span class="sxs-lookup"><span data-stu-id="76c79-109">The files in the asset are called **Asset Files**.</span></span> <span data-ttu-id="76c79-110">A **AssetFile** példány és a tényleges médiafájl két különböző objektum.</span><span class="sxs-lookup"><span data-stu-id="76c79-110">The **AssetFile** instance and the actual media file are two distinct objects.</span></span> <span data-ttu-id="76c79-111">A AssetFile példány media fájl metaadatainak tartalmaz, míg a médiafájl tartalmazza a tényleges médiatartalmakat.</span><span class="sxs-lookup"><span data-stu-id="76c79-111">The AssetFile instance contains metadata about the media file, while the media file contains the actual media content.</span></span>

> [!NOTE]
> <span data-ttu-id="76c79-112">A következők érvényesek:</span><span class="sxs-lookup"><span data-stu-id="76c79-112">The following considerations apply:</span></span>
> 
> * <span data-ttu-id="76c79-113">A Media Services a IAssetFile.Name tulajdonság értékét használja, amikor az adatfolyam-tartalmak (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) a URL-címek kiépítéséhez Emiatt százalék-kódolás nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="76c79-113">Media Services uses the value of the IAssetFile.Name property when building URLs for the streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="76c79-114">Értékét a **neve** tulajdonság nem lehet a következő [százalék kódolás-fenntartott karakterek](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ".</span><span class="sxs-lookup"><span data-stu-id="76c79-114">The value of the **Name** property cannot have any of the following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="76c79-115">Emellett csak lehet egy "." a fájlnévkiterjesztés.</span><span class="sxs-lookup"><span data-stu-id="76c79-115">Also, there can only be one '.' for the file name extension.</span></span>
> * <span data-ttu-id="76c79-116">A név hossza nem lehet hosszabb 260 karakternél.</span><span class="sxs-lookup"><span data-stu-id="76c79-116">The length of the name should not be greater than 260 characters.</span></span>
> * <span data-ttu-id="76c79-117">A Media Services által feldolgozható maximális támogatott fájlméret korlátozott.</span><span class="sxs-lookup"><span data-stu-id="76c79-117">There is a limit to the maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="76c79-118">A fájlméretre vonatkozó korlátozással kapcsolatban további információt [ebben](media-services-quotas-and-limitations.md) a témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="76c79-118">Please see [this](media-services-quotas-and-limitations.md) topic for details about the file size limitation.</span></span>
> * <span data-ttu-id="76c79-119">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="76c79-119">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="76c79-120">Ha mindig ugyanazokat a napokat/hozzáférési engedélyeket használja (például olyan keresők szabályzatait, amelyek hosszú ideig érvényben maradnak, vagyis nem feltöltött szabályzatokat), a szabályzatazonosítónak is ugyanannak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="76c79-120">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="76c79-121">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="76c79-121">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>
> 

<span data-ttu-id="76c79-122">Eszközök létrehozásakor a következő titkosítási beállításokat is megadhat.</span><span class="sxs-lookup"><span data-stu-id="76c79-122">When you create assets, you can specify the following encryption options.</span></span> 

* <span data-ttu-id="76c79-123">**Nincs** – Nincs titkosítás.</span><span class="sxs-lookup"><span data-stu-id="76c79-123">**None** - No encryption is used.</span></span> <span data-ttu-id="76c79-124">Ez az alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="76c79-124">This is the default value.</span></span> <span data-ttu-id="76c79-125">Vegye figyelembe, hogy ez a beállítás használatakor a tartalom nem védett átvitel, sem tárolás közben.</span><span class="sxs-lookup"><span data-stu-id="76c79-125">Note that when using this option your content is not protected in transit or at rest in storage.</span></span>
  <span data-ttu-id="76c79-126">Ha egy MP4-fájlt progresszív letöltés útján tervez továbbítani, használja ezt a lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="76c79-126">If you plan to deliver an MP4 using progressive download, use this option.</span></span> 
* <span data-ttu-id="76c79-127">**CommonEncryption** -használja ezt a beállítást, ha már titkosítva és általános titkosítás vagy a PlayReady DRM által (például védett Smooth Streaming egy PlayReady DRM) védett tartalmat.</span><span class="sxs-lookup"><span data-stu-id="76c79-127">**CommonEncryption** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="76c79-128">**EnvelopeEncrypted** – használja ezt a beállítást, ha AES által titkosított HLS.</span><span class="sxs-lookup"><span data-stu-id="76c79-128">**EnvelopeEncrypted** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="76c79-129">Megjegyzés: ehhez a fájlokat a Transform Manager használatával kell kódolni és titkosítani.</span><span class="sxs-lookup"><span data-stu-id="76c79-129">Note that the files must have been encoded and encrypted by Transform Manager.</span></span>
* <span data-ttu-id="76c79-130">**StorageEncrypted** - titkosítja a tiszta tartalom helyileg az AES-256 bites titkosítás használata, és feltölti azt Azure Storage helyén titkosítása.</span><span class="sxs-lookup"><span data-stu-id="76c79-130">**StorageEncrypted** - Encrypts your clear content locally using AES-256 bit encryption and then uploads it to Azure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="76c79-131">A Storage-titkosítással védett adategységek titkosítása a kódolás előtt automatikusan fel lesz oldva, és egy titkosított fájlrendszerbe kerülnek; az új kimeneti adategységként való újbóli feltöltés előtt pedig lehetőség van az újbóli titkosításukra.</span><span class="sxs-lookup"><span data-stu-id="76c79-131">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior to encoding, and optionally re-encrypted prior to uploading back as a new output asset.</span></span> <span data-ttu-id="76c79-132">A tárolás titkosítása elsődleges használati eset az, amikor biztonságossá tételéhez a kiváló minőségű bemeneti médiafájljait erős titkosítással aktívan a lemezen.</span><span class="sxs-lookup"><span data-stu-id="76c79-132">The primary use case for Storage Encryption is when you want to secure your high quality input media files with strong encryption at rest on disk.</span></span>
  
    <span data-ttu-id="76c79-133">A Media Services biztosít az eszközök, nem over tömörített digitális Manager (DRM) például a lemezen tárolás titkosítása.</span><span class="sxs-lookup"><span data-stu-id="76c79-133">Media Services provides on-disk storage encryption for your assets, not over-the-wire like Digital Rights Manager (DRM).</span></span>
  
    <span data-ttu-id="76c79-134">Ha az adategységen tárolótitkosítást alkalmaz, konfigurálnia kell az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="76c79-134">If your asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="76c79-135">További információ: [objektumtovábbítási szabályzat konfigurálása](media-services-dotnet-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="76c79-135">For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).</span></span>

<span data-ttu-id="76c79-136">Ha az eszköz titkosítva, amelyet megad egy **CommonEncrypted** lehetőséget, vagy egy **EnvelopeEncypted** beállítást, szüksége lesz az objektum hozzárendelése egy **ContentKey**.</span><span class="sxs-lookup"><span data-stu-id="76c79-136">If you specify for your asset to be encrypted with a **CommonEncrypted** option, or an **EnvelopeEncypted** option, you will need to associate your asset with a **ContentKey**.</span></span> <span data-ttu-id="76c79-137">További információkért lásd: [létrehozása egy ContentKey](media-services-dotnet-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="76c79-137">For more information, see [How to create a ContentKey](media-services-dotnet-create-contentkey.md).</span></span> 

<span data-ttu-id="76c79-138">Ha az eszköz titkosítva, amelyet megad egy **StorageEncrypted** beállítás, a Media Services SDK, a .NET hoz létre egy **StorateEncrypted** **ContentKey** a a eszköz.</span><span class="sxs-lookup"><span data-stu-id="76c79-138">If you specify for your asset to be encrypted with a **StorageEncrypted** option, the Media Services SDK for .NET will create a **StorateEncrypted** **ContentKey** for your asset.</span></span>

<span data-ttu-id="76c79-139">Ez a témakör bemutatja a Media Services .NET SDK-t, valamint a Media Services .NET SDK-bővítmények használata fájlok feltöltése a Media Services-objektumba.</span><span class="sxs-lookup"><span data-stu-id="76c79-139">This topic shows how to use Media Services .NET SDK as well as Media Services .NET SDK extensions to upload files into a Media Services asset.</span></span>

## <a name="upload-a-single-file-with-media-services-net-sdk"></a><span data-ttu-id="76c79-140">Media Services .NET SDK-val egy fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="76c79-140">Upload a single file with Media Services .NET SDK</span></span>
<span data-ttu-id="76c79-141">Az alábbi példakód .NET SDK használatával egyetlen fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="76c79-141">The sample code below uses .NET SDK to upload a single file.</span></span> <span data-ttu-id="76c79-142">A AccessPolicy és lokátor létrehozása és a feltöltés függvény megsemmisül.</span><span class="sxs-lookup"><span data-stu-id="76c79-142">The AccessPolicy and Locator are created and destroyed by the Upload function.</span></span> 


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


## <a name="upload-multiple-files-with-media-services-net-sdk"></a><span data-ttu-id="76c79-143">Media Services .NET SDK-val több fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="76c79-143">Upload multiple files with Media Services .NET SDK</span></span>
<span data-ttu-id="76c79-144">A következő kód bemutatja, hogyan hozzon létre egy eszközt, és több fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="76c79-144">The following code shows how to create an asset and upload multiple files.</span></span>

<span data-ttu-id="76c79-145">A kód a következőket teszi:</span><span class="sxs-lookup"><span data-stu-id="76c79-145">The code does the following:</span></span>

* <span data-ttu-id="76c79-146">Létrehoz egy üres eszköz az előző lépésben meghatározott CreateEmptyAsset metódussal.</span><span class="sxs-lookup"><span data-stu-id="76c79-146">Creates an empty asset using the CreateEmptyAsset method defined in the previous step.</span></span>
* <span data-ttu-id="76c79-147">Létrehoz egy **AccessPolicy** -példányt, engedélyekkel és hozzáférési időtartama határozza meg az eszközre.</span><span class="sxs-lookup"><span data-stu-id="76c79-147">Creates an **AccessPolicy** instance that defines the permissions and duration of access to the asset.</span></span>
* <span data-ttu-id="76c79-148">Létrehoz egy **lokátor** példánya, amely hozzáférést biztosít az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="76c79-148">Creates a **Locator** instance that provides access to the asset.</span></span>
* <span data-ttu-id="76c79-149">Létrehoz egy **BlobTransferClient** példány.</span><span class="sxs-lookup"><span data-stu-id="76c79-149">Creates a **BlobTransferClient** instance.</span></span> <span data-ttu-id="76c79-150">Ez a típus jelképezi ügyfél, amely az Azure-blobokat működik.</span><span class="sxs-lookup"><span data-stu-id="76c79-150">This type represents a client that operates on the Azure blobs.</span></span> <span data-ttu-id="76c79-151">Ebben a példában használjuk az ügyfél feltöltési előrehaladásának figyeléséhez.</span><span class="sxs-lookup"><span data-stu-id="76c79-151">In this example we use the client to monitor the upload progress.</span></span> 
* <span data-ttu-id="76c79-152">A megadott könyvtárban található fájlok keresztül enumerálása, és létrehoz egy **AssetFile** -példány minden fájlt.</span><span class="sxs-lookup"><span data-stu-id="76c79-152">Enumerates through files in the specified directory and creates an **AssetFile** instance for each file.</span></span>
* <span data-ttu-id="76c79-153">A Media Services segítségével feltölti a fájlokat a **UploadAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="76c79-153">Uploads the files into Media Services using the **UploadAsync** method.</span></span> 

> [!NOTE]
> <span data-ttu-id="76c79-154">A UploadAsync módszer segítségével győződjön meg arról, hogy a hívások nem korlátozzák, és a fájlok feltöltése párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="76c79-154">Use the UploadAsync method to ensure that the calls are not blocking and the files are uploaded in parallel.</span></span>
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

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }

    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



<span data-ttu-id="76c79-155">Eszközök nagy számú feltöltését, vegye figyelembe a következőket.</span><span class="sxs-lookup"><span data-stu-id="76c79-155">When uploading a large number of assets, consider the following.</span></span>

* <span data-ttu-id="76c79-156">Hozzon létre egy új **CloudMediaContext** szálankénti objektum.</span><span class="sxs-lookup"><span data-stu-id="76c79-156">Create a new **CloudMediaContext** object per thread.</span></span> <span data-ttu-id="76c79-157">A **CloudMediaContext** osztály nincs többszálú futtatásra.</span><span class="sxs-lookup"><span data-stu-id="76c79-157">The **CloudMediaContext** class is not thread safe.</span></span>
* <span data-ttu-id="76c79-158">Az alapértelmezett érték 2 nagyobb értékre mint 5 NumberOfConcurrentTransfers növelése</span><span class="sxs-lookup"><span data-stu-id="76c79-158">Increase NumberOfConcurrentTransfers from the default value of 2 to a higher value like 5.</span></span> <span data-ttu-id="76c79-159">A tulajdonság beállítása hatással van az összes példányát **CloudMediaContext**.</span><span class="sxs-lookup"><span data-stu-id="76c79-159">Setting this property affects all instances of **CloudMediaContext**.</span></span> 
* <span data-ttu-id="76c79-160">Az alapértelmezett érték 10 ParallelTransferThreadCount megőrizni.</span><span class="sxs-lookup"><span data-stu-id="76c79-160">Keep ParallelTransferThreadCount at the default value of 10.</span></span>

## <span data-ttu-id="76c79-161"><a id="ingest_in_bulk"></a>A Media Services .NET SDK használatával tömeges választásával dolgozhat fel eszközök</span><span class="sxs-lookup"><span data-stu-id="76c79-161"><a id="ingest_in_bulk"></a>Ingesting Assets in Bulk using Media Services .NET SDK</span></span>
<span data-ttu-id="76c79-162">Nagy eszköz fájlok feltöltése lehet a szűk keresztmetszetek eszköz létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="76c79-162">Uploading large asset files can be a bottleneck during asset creation.</span></span> <span data-ttu-id="76c79-163">Választásával dolgozhat fel eszközök tömeges vagy a "tömeges választásával dolgozhat" fel, magában foglalja a leválasztásával eszköz létrehozását a feltöltési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="76c79-163">Ingesting Assets in Bulk or “Bulk Ingesting”, involves decoupling asset creation from the upload process.</span></span> <span data-ttu-id="76c79-164">A tömeges választásával dolgozhat fel módszer használatához hozzon létre a jegyzékfájl (IngestManifest), amely az eszköz és az ahhoz tartozó fájlokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="76c79-164">To use a bulk ingesting approach, create a manifest (IngestManifest) that describes the asset and its associated files.</span></span> <span data-ttu-id="76c79-165">Az Ön által választott a fájlfeltöltési módszer segítségével a társított fájlok feltöltése a jegyzékfájl blob tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="76c79-165">Then use the upload method of your choice to upload the associated files to the manifest’s blob container.</span></span> <span data-ttu-id="76c79-166">A Microsoft Azure Media Services a blob tároló a jegyzékfájl társított figyeli.</span><span class="sxs-lookup"><span data-stu-id="76c79-166">Microsoft Azure Media Services watches the blob container associated with the manifest.</span></span> <span data-ttu-id="76c79-167">Miután egy fájlt tölt fel az a blob-tároló, a Microsoft Azure Media Services befejezte a jegyzékfájlban (IngestManifestAsset) az eszköz konfigurációján alapul eszköz létrehozását.</span><span class="sxs-lookup"><span data-stu-id="76c79-167">Once a file is uploaded to the blob container, Microsoft Azure Media Services completes the asset creation based on the configuration of the asset in the manifest (IngestManifestAsset).</span></span>

<span data-ttu-id="76c79-168">Hozzon létre egy új IngestManifest hívja meg a Create metódus által a CloudMediaContext az IngestManifests gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="76c79-168">To create a new IngestManifest call the Create method exposed by the IngestManifests collection on the CloudMediaContext.</span></span> <span data-ttu-id="76c79-169">Ezzel a módszerrel hoz létre egy új IngestManifest a jegyzékfájl-nevet.</span><span class="sxs-lookup"><span data-stu-id="76c79-169">This method will create a new IngestManifest with the manifest name you provide.</span></span>

    IIngestManifest manifest = context.IngestManifests.Create(name);

<span data-ttu-id="76c79-170">Az eszközöket, amely a tömeges IngestManifest társítva lesz létrehozni.</span><span class="sxs-lookup"><span data-stu-id="76c79-170">Create the assets that will be associated with the bulk IngestManifest.</span></span> <span data-ttu-id="76c79-171">Az eszköz tömeges választásával dolgozhat fel a kívánt titkosítási beállításainak megadása</span><span class="sxs-lookup"><span data-stu-id="76c79-171">Configure the desired encryption options on the asset for bulk ingesting.</span></span>

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

<span data-ttu-id="76c79-172">Egy IngestManifestAsset egy eszköz társít egy tömeges IngestManifest tömeges választásával dolgozhat fel.</span><span class="sxs-lookup"><span data-stu-id="76c79-172">An IngestManifestAsset associates an Asset with a bulk IngestManifest for bulk ingesting.</span></span> <span data-ttu-id="76c79-173">Azt is, amelyek minden eszköz a AssetFiles társítja.</span><span class="sxs-lookup"><span data-stu-id="76c79-173">It also associates the AssetFiles that will make up each Asset.</span></span> <span data-ttu-id="76c79-174">Hozzon létre egy IngestManifestAsset, használja a Create metódussal a kiszolgáló a környezetben.</span><span class="sxs-lookup"><span data-stu-id="76c79-174">To create an IngestManifestAsset, use the Create method on the server context.</span></span>

<span data-ttu-id="76c79-175">A következő példa bemutatja, hozzáadását két új IngestManifestAssets, amelyek a két eszközök tömeges korábban létrehozott társítják betöltési jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="76c79-175">The following example demonstrates adding two new IngestManifestAssets that associate the two assets previously created to the bulk ingest manifest.</span></span> <span data-ttu-id="76c79-176">Minden IngestManifestAsset is hozzárendeli a minden eszköz lesz feltöltve fájlokat alatt tömeges választásával dolgozhat fel.</span><span class="sxs-lookup"><span data-stu-id="76c79-176">Each IngestManifestAsset also associates a set of files that will be uploaded for each asset during bulk ingesting.</span></span>  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });

<span data-ttu-id="76c79-177">A nagy sebességű ügyfélalkalmazás képes az eszköz fájlok feltöltése a blob storage tárolók által megadott URI-Azonosítót is használhatja a **IIngestManifest.BlobStorageUriForUpload** a IngestManifest tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="76c79-177">You can use any high speed client application capable of uploading the asset files to the blob storage container URI provided by the **IIngestManifest.BlobStorageUriForUpload** property of the IngestManifest.</span></span> <span data-ttu-id="76c79-178">Egy figyelmet a jelentősebb nagy sebességű feltöltési szolgáltatás [Aspera igény szerinti Azure alkalmazáshoz](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6).</span><span class="sxs-lookup"><span data-stu-id="76c79-178">One notable high speed upload service is [Aspera On Demand for Azure Application](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6).</span></span> <span data-ttu-id="76c79-179">Ahogy az az alábbi Kódpélda az eszközök fájlok feltöltéséhez kódot is írhat.</span><span class="sxs-lookup"><span data-stu-id="76c79-179">You can also write code to upload the assets files as shown in the following code example.</span></span>

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

<span data-ttu-id="76c79-180">A minta a jelen témakörben használt eszköz fájlok feltöltése a kódját a következő kódrészlet példa látható.</span><span class="sxs-lookup"><span data-stu-id="76c79-180">The code for uploading the asset files for the sample used in this topic is shown in the following code example.</span></span>

    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);


<span data-ttu-id="76c79-181">A tömeges választásával dolgozhat fel társított összes eszköz előrehaladását segítségével meghatározhatja egy **IngestManifest** statisztika tulajdonságának lekérdezésével a **IngestManifest**.</span><span class="sxs-lookup"><span data-stu-id="76c79-181">You can determine the progress of the bulk ingesting for all assets associated with an **IngestManifest** by polling the Statistics property of the **IngestManifest**.</span></span> <span data-ttu-id="76c79-182">Folyamatban lévő információk frissítéséhez kell használnia egy új **CloudMediaContext** minden alkalommal, amikor lekérdezi a statisztika tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="76c79-182">In order to update progress information, you must use a new **CloudMediaContext** each time you poll the Statistics property.</span></span>

<span data-ttu-id="76c79-183">A következő példa bemutatja, hogy egy IngestManifest által lekérdezési a **azonosító**.</span><span class="sxs-lookup"><span data-stu-id="76c79-183">The following example demonstrates polling an IngestManifest by its **Id**.</span></span>

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



## <a name="upload-files-using-net-sdk-extensions"></a><span data-ttu-id="76c79-184">Töltse fel a fájlokat a .NET SDK-bővítmények</span><span class="sxs-lookup"><span data-stu-id="76c79-184">Upload files using .NET SDK Extensions</span></span>
<span data-ttu-id="76c79-185">Az alábbi példa bemutatja, hogyan .NET SDK-bővítmények használatával egyetlen fájl feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="76c79-185">The example below shows how to upload a single file using .NET SDK Extensions.</span></span> <span data-ttu-id="76c79-186">Ebben az esetben a **CreateFromFile** módszert használ, de az aszinkron verzióját is rendelkezésre áll (**CreateFromFileAsync**).</span><span class="sxs-lookup"><span data-stu-id="76c79-186">In this case the **CreateFromFile** method is used, but the asynchronous version is also available (**CreateFromFileAsync**).</span></span> <span data-ttu-id="76c79-187">A **CreateFromFile** módszer lehetővé teszi a adja meg a fájl, a titkosítási beállítással, és egy visszahívási ahhoz, hogy a fájl feltöltési folyamatáról.</span><span class="sxs-lookup"><span data-stu-id="76c79-187">The **CreateFromFile** method lets you specify the file name, encryption option, and a callback in order to report the upload progress of the file.</span></span>

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

<span data-ttu-id="76c79-188">Az alábbi példa UploadFile függvényt, és adja meg a tárolás titkosítása az eszköz létrehozása beállításként.</span><span class="sxs-lookup"><span data-stu-id="76c79-188">The following example calls UploadFile function and specifies storage encryption as the asset creation option.</span></span>  

    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);

## <a name="next-steps"></a><span data-ttu-id="76c79-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="76c79-189">Next steps</span></span>

<span data-ttu-id="76c79-190">Most már kódolhatja a feltöltött adategységeket.</span><span class="sxs-lookup"><span data-stu-id="76c79-190">You can now encode your uploaded assets.</span></span> <span data-ttu-id="76c79-191">További információ: [Encode Assets](media-services-portal-encode.md) (Adategységek kódolása).</span><span class="sxs-lookup"><span data-stu-id="76c79-191">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="76c79-192">Emellett az Azure Functions használatával is elindíthatja a kódolási feladatokat a konfigurált tárolóba érkező fájlok alapján.</span><span class="sxs-lookup"><span data-stu-id="76c79-192">You can also use Azure Functions to trigger an encoding job based on a file arriving in the configured container.</span></span> <span data-ttu-id="76c79-193">További információkért tekintse meg [ezt a mintát](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="76c79-193">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="76c79-194">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="76c79-194">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="76c79-195">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="76c79-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="76c79-196">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="76c79-196">Next step</span></span>
<span data-ttu-id="76c79-197">Most, hogy egy eszköz a Media Services feltöltött, navigáljon a [hogyan kérhet egy adathordozó processzor] [ How to Get a Media Processor] témakör.</span><span class="sxs-lookup"><span data-stu-id="76c79-197">Now that you have uploaded an asset to Media Services, go to the [How to Get a Media Processor][How to Get a Media Processor] topic.</span></span>

[How to Get a Media Processor]: media-services-get-media-processor.md

