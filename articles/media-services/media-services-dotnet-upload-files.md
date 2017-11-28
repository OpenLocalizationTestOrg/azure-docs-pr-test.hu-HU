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
# <a name="upload-files-into-a-media-services-account-using-net"></a><span data-ttu-id="1022f-103">Fájlok feltöltése a Media Services-fiók .NET használatával</span><span class="sxs-lookup"><span data-stu-id="1022f-103">Upload files into a Media Services account using .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1022f-104">.NET</span><span class="sxs-lookup"><span data-stu-id="1022f-104">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="1022f-105">REST</span><span class="sxs-lookup"><span data-stu-id="1022f-105">REST</span></span>](media-services-rest-upload-files.md)
> * [<span data-ttu-id="1022f-106">Portál</span><span class="sxs-lookup"><span data-stu-id="1022f-106">Portal</span></span>](media-services-portal-upload-files.md)
> 
> 

<span data-ttu-id="1022f-107">A Media Services szolgáltatásban a digitális fájlok feltöltése vagy kimenete egy adategységbe történik.</span><span class="sxs-lookup"><span data-stu-id="1022f-107">In Media Services, you upload (or ingest) your digital files into an asset.</span></span> <span data-ttu-id="1022f-108">Hello **eszköz** entitás tartalmazhat videót, hang, képek, miniatűröket, szöveg nyomon követi és feliratfájlokat fájlokat (és mindezen fájlok metaadatait hello.)  Miután hello fájlok feltöltése után a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.</span><span class="sxs-lookup"><span data-stu-id="1022f-108">hello **Asset** entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.)  Once hello files are uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span>

<span data-ttu-id="1022f-109">hello eszköz hello fájlok nevezzük **adategység-fájloknak**.</span><span class="sxs-lookup"><span data-stu-id="1022f-109">hello files in hello asset are called **Asset Files**.</span></span> <span data-ttu-id="1022f-110">Hello **AssetFile** példány és a hello tényleges media fájl is két különböző objektum.</span><span class="sxs-lookup"><span data-stu-id="1022f-110">hello **AssetFile** instance and hello actual media file are two distinct objects.</span></span> <span data-ttu-id="1022f-111">hello AssetFile példány hello media fájlról metaadatot tartalmaz, amíg hello médiafájl tartalmaz hello tényleges médiatartalmakat.</span><span class="sxs-lookup"><span data-stu-id="1022f-111">hello AssetFile instance contains metadata about hello media file, while hello media file contains hello actual media content.</span></span>

> [!NOTE]
> <span data-ttu-id="1022f-112">a következő szempontok hello vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="1022f-112">hello following considerations apply:</span></span>
> 
> * <span data-ttu-id="1022f-113">Media Services hello hello IAssetFile.Name tulajdonság értékét használja, amikor a hello adatfolyam-tartalmat (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) URL-címek kiépítéséhez Emiatt százalék-kódolás nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="1022f-113">Media Services uses hello value of hello IAssetFile.Name property when building URLs for hello streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="1022f-114">hello értékének hello **neve** tulajdonság nem lehet hello következő [százalék kódolás-fenntartott karakterek](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ".</span><span class="sxs-lookup"><span data-stu-id="1022f-114">hello value of hello **Name** property cannot have any of hello following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="1022f-115">Emellett csak lehet egy "." hello fájlnévkiterjesztés.</span><span class="sxs-lookup"><span data-stu-id="1022f-115">Also, there can only be one '.' for hello file name extension.</span></span>
> * <span data-ttu-id="1022f-116">hello hello nevének hossza nem lehet hosszabb 260 karakternél.</span><span class="sxs-lookup"><span data-stu-id="1022f-116">hello length of hello name should not be greater than 260 characters.</span></span>
> * <span data-ttu-id="1022f-117">Nincs a Media Services feldolgozás támogatott korlátot toohello fájl maximális méretét.</span><span class="sxs-lookup"><span data-stu-id="1022f-117">There is a limit toohello maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="1022f-118">Ellenőrizze a [ez](media-services-quotas-and-limitations.md) témakör hello méretű fájlt választhat vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="1022f-118">Please see [this](media-services-quotas-and-limitations.md) topic for details about hello file size limitation.</span></span>
> * <span data-ttu-id="1022f-119">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="1022f-119">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="1022f-120">Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek.</span><span class="sxs-lookup"><span data-stu-id="1022f-120">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="1022f-121">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="1022f-121">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>
> 

<span data-ttu-id="1022f-122">Eszközök létrehozásakor az alábbi titkosítási beállítások hello is megadhat.</span><span class="sxs-lookup"><span data-stu-id="1022f-122">When you create assets, you can specify hello following encryption options.</span></span> 

* <span data-ttu-id="1022f-123">**Nincs** – Nincs titkosítás.</span><span class="sxs-lookup"><span data-stu-id="1022f-123">**None** - No encryption is used.</span></span> <span data-ttu-id="1022f-124">Ez az alapértelmezett érték hello.</span><span class="sxs-lookup"><span data-stu-id="1022f-124">This is hello default value.</span></span> <span data-ttu-id="1022f-125">Vegye figyelembe, hogy ez a beállítás használatakor a tartalom nem védett átvitel, sem tárolás közben.</span><span class="sxs-lookup"><span data-stu-id="1022f-125">Note that when using this option your content is not protected in transit or at rest in storage.</span></span>
  <span data-ttu-id="1022f-126">Ha egy MP4 toodeliver fájlt progresszív letöltés útján tervez, használja ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="1022f-126">If you plan toodeliver an MP4 using progressive download, use this option.</span></span> 
* <span data-ttu-id="1022f-127">**CommonEncryption** -használja ezt a beállítást, ha már titkosítva és általános titkosítás vagy a PlayReady DRM által (például védett Smooth Streaming egy PlayReady DRM) védett tartalmat.</span><span class="sxs-lookup"><span data-stu-id="1022f-127">**CommonEncryption** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="1022f-128">**EnvelopeEncrypted** – használja ezt a beállítást, ha AES által titkosított HLS.</span><span class="sxs-lookup"><span data-stu-id="1022f-128">**EnvelopeEncrypted** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="1022f-129">Vegye figyelembe, hogy hello fájlokat kell kódolni és titkosítani Transform Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="1022f-129">Note that hello files must have been encoded and encrypted by Transform Manager.</span></span>
* <span data-ttu-id="1022f-130">**StorageEncrypted** - titkosítja a tiszta tartalom helyileg az AES-256 bites titkosítás használata és tooAzure helyén tárolás titkosítása feltöltését.</span><span class="sxs-lookup"><span data-stu-id="1022f-130">**StorageEncrypted** - Encrypts your clear content locally using AES-256 bit encryption and then uploads it tooAzure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="1022f-131">Storage-titkosítással védett adategységek automatikusan titkosítás és egy titkosított fájl rendszer előzetes tooencoding, és ha szükséges újra titkosítani előzetes toouploading egy új kimeneti eszközként helyezve.</span><span class="sxs-lookup"><span data-stu-id="1022f-131">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior tooencoding, and optionally re-encrypted prior toouploading back as a new output asset.</span></span> <span data-ttu-id="1022f-132">hello elsődleges használati eset a tárolás titkosítása akkor, ha azt szeretné, hogy a jó minőségű bemeneti médiafájljait erős titkosítással, rest-lemezen toosecure.</span><span class="sxs-lookup"><span data-stu-id="1022f-132">hello primary use case for Storage Encryption is when you want toosecure your high quality input media files with strong encryption at rest on disk.</span></span>
  
    <span data-ttu-id="1022f-133">A Media Services biztosít az eszközök, nem over tömörített digitális Manager (DRM) például a lemezen tárolás titkosítása.</span><span class="sxs-lookup"><span data-stu-id="1022f-133">Media Services provides on-disk storage encryption for your assets, not over-the-wire like Digital Rights Manager (DRM).</span></span>
  
    <span data-ttu-id="1022f-134">Ha az adategységen tárolótitkosítást alkalmaz, konfigurálnia kell az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="1022f-134">If your asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="1022f-135">További információ: [objektumtovábbítási szabályzat konfigurálása](media-services-dotnet-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="1022f-135">For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).</span></span>

<span data-ttu-id="1022f-136">Ha hogy adja meg az eszköz toobe titkosítva egy **CommonEncrypted** lehetőséget, vagy egy **EnvelopeEncypted** lehetőséget, akkor tooassociate az objektum egy **ContentKey**.</span><span class="sxs-lookup"><span data-stu-id="1022f-136">If you specify for your asset toobe encrypted with a **CommonEncrypted** option, or an **EnvelopeEncypted** option, you will need tooassociate your asset with a **ContentKey**.</span></span> <span data-ttu-id="1022f-137">További információkért lásd: [hogyan toocreate egy ContentKey](media-services-dotnet-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="1022f-137">For more information, see [How toocreate a ContentKey](media-services-dotnet-create-contentkey.md).</span></span> 

<span data-ttu-id="1022f-138">Ha hogy adja meg az eszköz toobe titkosítva egy **StorageEncrypted** lehetőségre, a Media Services SDK hello a .NET hoz létre egy **StorateEncrypted** **ContentKey** a a eszköz.</span><span class="sxs-lookup"><span data-stu-id="1022f-138">If you specify for your asset toobe encrypted with a **StorageEncrypted** option, hello Media Services SDK for .NET will create a **StorateEncrypted** **ContentKey** for your asset.</span></span>

<span data-ttu-id="1022f-139">Ez a témakör bemutatja, hogyan toouse Media Services .NET SDK, valamint a Media Services .NET SDK bővítmények tooupload fájlok egy Media Services-objektumba.</span><span class="sxs-lookup"><span data-stu-id="1022f-139">This topic shows how toouse Media Services .NET SDK as well as Media Services .NET SDK extensions tooupload files into a Media Services asset.</span></span>

## <a name="upload-a-single-file-with-media-services-net-sdk"></a><span data-ttu-id="1022f-140">Media Services .NET SDK-val egy fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="1022f-140">Upload a single file with Media Services .NET SDK</span></span>
<span data-ttu-id="1022f-141">az alábbi hello mintakód .NET SDK tooupload akár egyetlen fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="1022f-141">hello sample code below uses .NET SDK tooupload a single file.</span></span> <span data-ttu-id="1022f-142">hello AccessPolicy és lokátor létrehozása és hello feltöltés függvény megsemmisül.</span><span class="sxs-lookup"><span data-stu-id="1022f-142">hello AccessPolicy and Locator are created and destroyed by hello Upload function.</span></span> 


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


## <a name="upload-multiple-files-with-media-services-net-sdk"></a><span data-ttu-id="1022f-143">Media Services .NET SDK-val több fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="1022f-143">Upload multiple files with Media Services .NET SDK</span></span>
<span data-ttu-id="1022f-144">a következő kód bemutatja hogyan hello toocreate egy eszköz és a több fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="1022f-144">hello following code shows how toocreate an asset and upload multiple files.</span></span>

<span data-ttu-id="1022f-145">hello kód hello a következő:</span><span class="sxs-lookup"><span data-stu-id="1022f-145">hello code does hello following:</span></span>

* <span data-ttu-id="1022f-146">Létrehoz egy üres eszköz metódussal hello CreateEmptyAsset hello előző lépésben meghatározott.</span><span class="sxs-lookup"><span data-stu-id="1022f-146">Creates an empty asset using hello CreateEmptyAsset method defined in hello previous step.</span></span>
* <span data-ttu-id="1022f-147">Létrehoz egy **AccessPolicy** -példányt, hello engedélyekkel és hozzáférési toohello eszköz időtartamát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1022f-147">Creates an **AccessPolicy** instance that defines hello permissions and duration of access toohello asset.</span></span>
* <span data-ttu-id="1022f-148">Létrehoz egy **lokátor** toohello eszköz hozzáférést biztosító példány.</span><span class="sxs-lookup"><span data-stu-id="1022f-148">Creates a **Locator** instance that provides access toohello asset.</span></span>
* <span data-ttu-id="1022f-149">Létrehoz egy **BlobTransferClient** példány.</span><span class="sxs-lookup"><span data-stu-id="1022f-149">Creates a **BlobTransferClient** instance.</span></span> <span data-ttu-id="1022f-150">Ez a típus jelképezi ügyfél, amely az Azure-blobok hello működik.</span><span class="sxs-lookup"><span data-stu-id="1022f-150">This type represents a client that operates on hello Azure blobs.</span></span> <span data-ttu-id="1022f-151">Ebben a példában használjuk hello ügyfél toomonitor hello feltöltési folyamatáról.</span><span class="sxs-lookup"><span data-stu-id="1022f-151">In this example we use hello client toomonitor hello upload progress.</span></span> 
* <span data-ttu-id="1022f-152">Hello megadott könyvtárban található fájlok keresztül enumerálása, és létrehoz egy **AssetFile** -példány minden fájlt.</span><span class="sxs-lookup"><span data-stu-id="1022f-152">Enumerates through files in hello specified directory and creates an **AssetFile** instance for each file.</span></span>
* <span data-ttu-id="1022f-153">Feltöltések hello fájlok a Media Services segítségével hello **UploadAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="1022f-153">Uploads hello files into Media Services using hello **UploadAsync** method.</span></span> 

> [!NOTE]
> <span data-ttu-id="1022f-154">Hello UploadAsync metódus tooensure hello hívások nem korlátozzák, és hello fájlok feltöltése párhuzamosan használható.</span><span class="sxs-lookup"><span data-stu-id="1022f-154">Use hello UploadAsync method tooensure that hello calls are not blocking and hello files are uploaded in parallel.</span></span>
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



<span data-ttu-id="1022f-155">Eszközök nagy számú feltöltését, vegye figyelembe hello következő.</span><span class="sxs-lookup"><span data-stu-id="1022f-155">When uploading a large number of assets, consider hello following.</span></span>

* <span data-ttu-id="1022f-156">Hozzon létre egy új **CloudMediaContext** szálankénti objektum.</span><span class="sxs-lookup"><span data-stu-id="1022f-156">Create a new **CloudMediaContext** object per thread.</span></span> <span data-ttu-id="1022f-157">Hello **CloudMediaContext** osztály nincs többszálú futtatásra.</span><span class="sxs-lookup"><span data-stu-id="1022f-157">hello **CloudMediaContext** class is not thread safe.</span></span>
* <span data-ttu-id="1022f-158">Növelje a NumberOfConcurrentTransfers hello alapértelmezett érték 2 tooa magasabb érték 5 hasonlóan.</span><span class="sxs-lookup"><span data-stu-id="1022f-158">Increase NumberOfConcurrentTransfers from hello default value of 2 tooa higher value like 5.</span></span> <span data-ttu-id="1022f-159">A tulajdonság beállítása hatással van az összes példányát **CloudMediaContext**.</span><span class="sxs-lookup"><span data-stu-id="1022f-159">Setting this property affects all instances of **CloudMediaContext**.</span></span> 
* <span data-ttu-id="1022f-160">Tartsa ParallelTransferThreadCount 10 hello alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="1022f-160">Keep ParallelTransferThreadCount at hello default value of 10.</span></span>

## <span data-ttu-id="1022f-161"><a id="ingest_in_bulk"></a>A Media Services .NET SDK használatával tömeges választásával dolgozhat fel eszközök</span><span class="sxs-lookup"><span data-stu-id="1022f-161"><a id="ingest_in_bulk"></a>Ingesting Assets in Bulk using Media Services .NET SDK</span></span>
<span data-ttu-id="1022f-162">Nagy eszköz fájlok feltöltése lehet a szűk keresztmetszetek eszköz létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="1022f-162">Uploading large asset files can be a bottleneck during asset creation.</span></span> <span data-ttu-id="1022f-163">Választásával dolgozhat fel eszközök tömeges vagy a "tömeges választásával dolgozhat" fel, magában foglalja a leválasztásával eszköz létrehozása hello feltöltési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="1022f-163">Ingesting Assets in Bulk or “Bulk Ingesting”, involves decoupling asset creation from hello upload process.</span></span> <span data-ttu-id="1022f-164">toouse egy tömeges ingesting módszert használja, hozzon létre a jegyzékfájl (IngestManifest), amely hello eszköz és az ahhoz tartozó fájlokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="1022f-164">toouse a bulk ingesting approach, create a manifest (IngestManifest) that describes hello asset and its associated files.</span></span> <span data-ttu-id="1022f-165">Hello fájlfeltöltési módszer a választott tooupload hello társított fájlokat toohello jegyzék blob tároló használni.</span><span class="sxs-lookup"><span data-stu-id="1022f-165">Then use hello upload method of your choice tooupload hello associated files toohello manifest’s blob container.</span></span> <span data-ttu-id="1022f-166">A Microsoft Azure Media Services hello blob tároló hello jegyzékfájl társított figyeli.</span><span class="sxs-lookup"><span data-stu-id="1022f-166">Microsoft Azure Media Services watches hello blob container associated with hello manifest.</span></span> <span data-ttu-id="1022f-167">Amikor egy fájl feltöltött toohello blob tároló, a Microsoft Azure Media Services befejezése hello eszköz létrehozása hello konfiguráció hello eszköz hello jegyzékfájlban (IngestManifestAsset) alapján.</span><span class="sxs-lookup"><span data-stu-id="1022f-167">Once a file is uploaded toohello blob container, Microsoft Azure Media Services completes hello asset creation based on hello configuration of hello asset in hello manifest (IngestManifestAsset).</span></span>

<span data-ttu-id="1022f-168">egy új IngestManifest toocreate metódushívás hello létrehozása hello hello CloudMediaContext IngestManifests gyűjtemény által elérhetővé tett tárolókra.</span><span class="sxs-lookup"><span data-stu-id="1022f-168">toocreate a new IngestManifest call hello Create method exposed by hello IngestManifests collection on hello CloudMediaContext.</span></span> <span data-ttu-id="1022f-169">Ezzel a módszerrel hoz létre egy új IngestManifest hello manifest-nevet.</span><span class="sxs-lookup"><span data-stu-id="1022f-169">This method will create a new IngestManifest with hello manifest name you provide.</span></span>

    IIngestManifest manifest = context.IngestManifests.Create(name);

<span data-ttu-id="1022f-170">Hozzon létre hello eszközök, amelyek az hello tömeges IngestManifest lesz társítva.</span><span class="sxs-lookup"><span data-stu-id="1022f-170">Create hello assets that will be associated with hello bulk IngestManifest.</span></span> <span data-ttu-id="1022f-171">Konfigurálja a szükséges hello titkosítási beállításokat hello eszköz tömeges választásával dolgozhat fel.</span><span class="sxs-lookup"><span data-stu-id="1022f-171">Configure hello desired encryption options on hello asset for bulk ingesting.</span></span>

    // Create hello assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

<span data-ttu-id="1022f-172">Egy IngestManifestAsset egy eszköz társít egy tömeges IngestManifest tömeges választásával dolgozhat fel.</span><span class="sxs-lookup"><span data-stu-id="1022f-172">An IngestManifestAsset associates an Asset with a bulk IngestManifest for bulk ingesting.</span></span> <span data-ttu-id="1022f-173">Azt is, amelyek minden eszköz AssetFiles hello társítja.</span><span class="sxs-lookup"><span data-stu-id="1022f-173">It also associates hello AssetFiles that will make up each Asset.</span></span> <span data-ttu-id="1022f-174">egy IngestManifestAsset toocreate módszert hello létrehozás hello kiszolgáló környezetben.</span><span class="sxs-lookup"><span data-stu-id="1022f-174">toocreate an IngestManifestAsset, use hello Create method on hello server context.</span></span>

<span data-ttu-id="1022f-175">hello következő példa bemutatja, hozzáadását két új IngestManifestAssets társító hello két eszközök korábban létrehozott toohello tömeges betöltési jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="1022f-175">hello following example demonstrates adding two new IngestManifestAssets that associate hello two assets previously created toohello bulk ingest manifest.</span></span> <span data-ttu-id="1022f-176">Minden IngestManifestAsset is hozzárendeli a minden eszköz lesz feltöltve fájlokat alatt tömeges választásával dolgozhat fel.</span><span class="sxs-lookup"><span data-stu-id="1022f-176">Each IngestManifestAsset also associates a set of files that will be uploaded for each asset during bulk ingesting.</span></span>  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });

<span data-ttu-id="1022f-177">Használhatja a nagy sebességű ügyfélalkalmazás képes hello eszköz fájlok toohello blob storage tárolót hello által biztosított URI feltöltése **IIngestManifest.BlobStorageUriForUpload** hello IngestManifest tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="1022f-177">You can use any high speed client application capable of uploading hello asset files toohello blob storage container URI provided by hello **IIngestManifest.BlobStorageUriForUpload** property of hello IngestManifest.</span></span> <span data-ttu-id="1022f-178">Egy figyelmet a jelentősebb nagy sebességű feltöltési szolgáltatás [Aspera igény szerinti Azure alkalmazáshoz](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6).</span><span class="sxs-lookup"><span data-stu-id="1022f-178">One notable high speed upload service is [Aspera On Demand for Azure Application](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6).</span></span> <span data-ttu-id="1022f-179">Is írhat kódot tooupload hello eszközök fájlok ahogy az alábbi kódpéldát hello.</span><span class="sxs-lookup"><span data-stu-id="1022f-179">You can also write code tooupload hello assets files as shown in hello following code example.</span></span>

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

<span data-ttu-id="1022f-180">hello hello adategység-fájloknak a jelen témakörben használt hello minta feltöltése kód az alábbi kódpéldát hello.</span><span class="sxs-lookup"><span data-stu-id="1022f-180">hello code for uploading hello asset files for hello sample used in this topic is shown in hello following code example.</span></span>

    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);


<span data-ttu-id="1022f-181">Azt is meghatározhatja, hello tömeges választásával dolgozhat fel társított összes eszköz hello előrehaladását egy **IngestManifest** hello hello statisztika tulajdonságának lekérdezésével **IngestManifest**.</span><span class="sxs-lookup"><span data-stu-id="1022f-181">You can determine hello progress of hello bulk ingesting for all assets associated with an **IngestManifest** by polling hello Statistics property of hello **IngestManifest**.</span></span> <span data-ttu-id="1022f-182">A sorrend tooupdate végrehajtási adatok, kell használnia egy új **CloudMediaContext** minden alkalommal, amikor lekérdezi hello statisztika tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="1022f-182">In order tooupdate progress information, you must use a new **CloudMediaContext** each time you poll hello Statistics property.</span></span>

<span data-ttu-id="1022f-183">hello következő példa bemutatja egy IngestManifest által lekérdezési a **azonosító**.</span><span class="sxs-lookup"><span data-stu-id="1022f-183">hello following example demonstrates polling an IngestManifest by its **Id**.</span></span>

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



## <a name="upload-files-using-net-sdk-extensions"></a><span data-ttu-id="1022f-184">Töltse fel a fájlokat a .NET SDK-bővítmények</span><span class="sxs-lookup"><span data-stu-id="1022f-184">Upload files using .NET SDK Extensions</span></span>
<span data-ttu-id="1022f-185">hello az alábbi példa bemutatja, hogyan tooupload egy egyetlen fájl .NET SDK-bővítmények használatával.</span><span class="sxs-lookup"><span data-stu-id="1022f-185">hello example below shows how tooupload a single file using .NET SDK Extensions.</span></span> <span data-ttu-id="1022f-186">Ebben az esetben hello **CreateFromFile** módszert használ, de hello aszinkron verzióját is rendelkezésre áll (**CreateFromFileAsync**).</span><span class="sxs-lookup"><span data-stu-id="1022f-186">In this case hello **CreateFromFile** method is used, but hello asynchronous version is also available (**CreateFromFileAsync**).</span></span> <span data-ttu-id="1022f-187">Hello **CreateFromFile** módszer lehetővé teszi a adja meg a hello fájlnév, a titkosítási beállítás és a sorrend tooreport hello visszahívás feltöltése hello fájl előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="1022f-187">hello **CreateFromFile** method lets you specify hello file name, encryption option, and a callback in order tooreport hello upload progress of hello file.</span></span>

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

<span data-ttu-id="1022f-188">hello alábbi példa UploadFile függvényt és a tárolás titkosítása hello eszköz létrehozására beállítást adja meg.</span><span class="sxs-lookup"><span data-stu-id="1022f-188">hello following example calls UploadFile function and specifies storage encryption as hello asset creation option.</span></span>  

    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);

## <a name="next-steps"></a><span data-ttu-id="1022f-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1022f-189">Next steps</span></span>

<span data-ttu-id="1022f-190">Most már kódolhatja a feltöltött adategységeket.</span><span class="sxs-lookup"><span data-stu-id="1022f-190">You can now encode your uploaded assets.</span></span> <span data-ttu-id="1022f-191">További információ: [Encode Assets](media-services-portal-encode.md) (Adategységek kódolása).</span><span class="sxs-lookup"><span data-stu-id="1022f-191">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="1022f-192">Használhatja az Azure Functions tootrigger egy kódolási feladat, a konfigurált hello tároló érkező fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="1022f-192">You can also use Azure Functions tootrigger an encoding job based on a file arriving in hello configured container.</span></span> <span data-ttu-id="1022f-193">További információkért tekintse meg [ezt a mintát](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="1022f-193">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="1022f-194">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="1022f-194">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1022f-195">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="1022f-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="1022f-196">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="1022f-196">Next step</span></span>
<span data-ttu-id="1022f-197">Most, hogy egy eszköz már feltöltött tooMedia szolgáltatások, go toohello [hogyan tooGet Media processzort] [ How tooGet a Media Processor] témakör.</span><span class="sxs-lookup"><span data-stu-id="1022f-197">Now that you have uploaded an asset tooMedia Services, go toohello [How tooGet a Media Processor][How tooGet a Media Processor] topic.</span></span>

[How tooGet a Media Processor]: media-services-get-media-processor.md

