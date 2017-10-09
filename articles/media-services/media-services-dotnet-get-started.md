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
# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a><span data-ttu-id="e4e23-103">Tartalmak továbbítása igény szerint a .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="e4e23-103">Get started with delivering content on demand using .NET SDK</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="e4e23-104">Ez az oktatóanyag végigvezeti hello egy alapszintű Video-on-Demand (VoD) tartalomtovábbító service végrehajtási hello Azure Media Services .NET SDK-t használó Azure Media Services (AMS) alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="e4e23-104">This tutorial walks you through hello steps of implementing a basic Video-on-Demand (VoD) content delivery service with Azure Media Services (AMS) application using hello Azure Media Services .NET SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4e23-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e4e23-105">Prerequisites</span></span>

<span data-ttu-id="e4e23-106">Az alábbiakban hello szükséges toocomplete hello oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="e4e23-106">hello following are required toocomplete hello tutorial:</span></span>

* <span data-ttu-id="e4e23-107">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="e4e23-107">An Azure account.</span></span> <span data-ttu-id="e4e23-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e4e23-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="e4e23-109">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="e4e23-109">A Media Services account.</span></span> <span data-ttu-id="e4e23-110">egy Media Services-fiók toocreate lásd [hogyan tooCreate Media Services-fiók](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="e4e23-110">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="e4e23-111">A .NET-keretrendszer 4.0-s vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="e4e23-111">.NET Framework 4.0 or later.</span></span>
* <span data-ttu-id="e4e23-112">Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e4e23-112">Visual Studio.</span></span>

<span data-ttu-id="e4e23-113">Ez az oktatóanyag hello a következő feladatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="e4e23-113">This tutorial includes hello following tasks:</span></span>

1. <span data-ttu-id="e4e23-114">Indítsa el az adatfolyam-továbbítási végpontra (hello Azure-portál használatával).</span><span class="sxs-lookup"><span data-stu-id="e4e23-114">Start streaming endpoint (using hello Azure portal).</span></span>
2. <span data-ttu-id="e4e23-115">Egy Visual Studio-projekt létrehozása és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e4e23-115">Create and configure a Visual Studio project.</span></span>
3. <span data-ttu-id="e4e23-116">Csatlakozás a Media Services-fiók toohello.</span><span class="sxs-lookup"><span data-stu-id="e4e23-116">Connect toohello Media Services account.</span></span>
2. <span data-ttu-id="e4e23-117">Videofájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="e4e23-117">Upload a video file.</span></span>
3. <span data-ttu-id="e4e23-118">Hello forrásfájl kódolása adaptív sávszélességű MP4-fájlokat alakítja.</span><span class="sxs-lookup"><span data-stu-id="e4e23-118">Encode hello source file into a set of adaptive bitrate MP4 files.</span></span>
4. <span data-ttu-id="e4e23-119">Hello objektum közzététele, és a streamelési és a progresszív letöltési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="e4e23-119">Publish hello asset and get streaming and progressive download URLs.</span></span>  
5. <span data-ttu-id="e4e23-120">Tartalom lejátszása</span><span class="sxs-lookup"><span data-stu-id="e4e23-120">Play your content.</span></span>

## <a name="overview"></a><span data-ttu-id="e4e23-121">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e4e23-121">Overview</span></span>
<span data-ttu-id="e4e23-122">Ez az oktatóanyag végigvezeti hello végrehajtási egy Video-on-Demand (VoD) tartalomtovábbító alkalmazást .NET-keretrendszerhez készült Azure Media Services (AMS) SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="e4e23-122">This tutorial walks you through hello steps of implementing a Video-on-Demand (VoD) content delivery application using Azure Media Services (AMS) SDK for .NET.</span></span>

<span data-ttu-id="e4e23-123">hello útmutató bemutatja a Media Services alapvető munkafolyamatait hello és hello leggyakoribb programozási objektumokat és a Media Services-fejlesztés szükséges feladatok.</span><span class="sxs-lookup"><span data-stu-id="e4e23-123">hello tutorial introduces hello basic Media Services workflow and hello most common programming objects and tasks required for Media Services development.</span></span> <span data-ttu-id="e4e23-124">A hello hello az oktatóanyag befejezése után képes toostream kell vagy fokozatosan letölteni egy feltöltött, kódolt és letöltött példa médiafájlt lesz.</span><span class="sxs-lookup"><span data-stu-id="e4e23-124">At hello completion of hello tutorial, you will be able toostream or progressively download a sample media file that you uploaded, encoded, and downloaded.</span></span>

### <a name="ams-model"></a><span data-ttu-id="e4e23-125">AMS-modell</span><span class="sxs-lookup"><span data-stu-id="e4e23-125">AMS model</span></span>

<span data-ttu-id="e4e23-126">hello következő kép bemutatja a leggyakrabban használt hello objektumok hello Media Services OData modellre VoD-alkalmazások fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="e4e23-126">hello following image shows some of hello most commonly used objects when developing VoD applications against hello Media Services OData model.</span></span>

<span data-ttu-id="e4e23-127">Kattintson a hello kép tooview, teljes méret.</span><span class="sxs-lookup"><span data-stu-id="e4e23-127">Click hello image tooview it full size.</span></span>  

<a href="./media/media-services-dotnet-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-dotnet-get-started/media-services-overview-object-model-small.png"></a> 

<span data-ttu-id="e4e23-128">Megtekintheti a teljes minta hello [Itt](https://media.windows.net/API/$metadata?api-version=2.15).</span><span class="sxs-lookup"><span data-stu-id="e4e23-128">You can view hello whole model [here](https://media.windows.net/API/$metadata?api-version=2.15).</span></span>  

## <a name="start-streaming-endpoints-using-hello-azure-portal"></a><span data-ttu-id="e4e23-129">Indítsa el az adatfolyam-végpontok hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="e4e23-129">Start streaming endpoints using hello Azure portal</span></span>

<span data-ttu-id="e4e23-130">Ha az Azure Media Services egyik leggyakoribb forgatókönyve hello elkötelezett használatával adatfolyam adaptív sávszélességű streamelést működik.</span><span class="sxs-lookup"><span data-stu-id="e4e23-130">When working with Azure Media Services one of hello most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="e4e23-131">A Media Services dinamikus becsomagolást biztosít, amely lehetővé teszi toodeliver az adaptív sávszélességű MP4-kódolású tartalmak anélkül, hogy előre csomagolt toostore (MPEG DASH, HLS, Smooth Streaming), a Media Services just-in-time, által támogatott streamformátumok Ezekbe a formátumokba egyes verzióit.</span><span class="sxs-lookup"><span data-stu-id="e4e23-131">Media Services provides dynamic packaging, which allows you toodeliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having toostore pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="e4e23-132">Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="e4e23-132">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="e4e23-133">a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="e4e23-133">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span>

<span data-ttu-id="e4e23-134">toostart hello streamvégpontra, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="e4e23-134">toostart hello streaming endpoint, do hello following:</span></span>

1. <span data-ttu-id="e4e23-135">Jelentkezzen be hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e4e23-135">Log in at hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e4e23-136">Hello-beállítások ablakában kattintson a Streaming végpontok.</span><span class="sxs-lookup"><span data-stu-id="e4e23-136">In hello Settings window, click Streaming endpoints.</span></span>
3. <span data-ttu-id="e4e23-137">Kattintson a hello alapértelmezett streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="e4e23-137">Click hello default streaming endpoint.</span></span>

    <span data-ttu-id="e4e23-138">hello alapértelmezett STREAMING ENDPOINT részletek ablak.</span><span class="sxs-lookup"><span data-stu-id="e4e23-138">hello DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="e4e23-139">Kattintson a hello Start ikonra.</span><span class="sxs-lookup"><span data-stu-id="e4e23-139">Click hello Start icon.</span></span>
5. <span data-ttu-id="e4e23-140">Kattintson a hello Mentés gombra toosave a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="e4e23-140">Click hello Save button toosave your changes.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="e4e23-141">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e4e23-141">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="e4e23-142">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="e4e23-142">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="e4e23-143">Hozzon létre egy új mappát (mappa bárhol lehet a helyi meghajtóról), és szeretné, hogy tooencode és adatfolyam, vagy fokozatosan letölteni egy .mp4 fájlt másolja.</span><span class="sxs-lookup"><span data-stu-id="e4e23-143">Create a new folder (folder can be anywhere on your local drive) and copy an .mp4 file that you want tooencode and stream or progressively download.</span></span> <span data-ttu-id="e4e23-144">Ebben a példában hello "C:\VideoFiles" elérési utat használja.</span><span class="sxs-lookup"><span data-stu-id="e4e23-144">In this example, hello "C:\VideoFiles" path is used.</span></span>

## <a name="connect-toohello-media-services-account"></a><span data-ttu-id="e4e23-145">Csatlakozás a Media Services-fiók toohello</span><span class="sxs-lookup"><span data-stu-id="e4e23-145">Connect toohello Media Services account</span></span>

<span data-ttu-id="e4e23-146">A Media Services használata a .NET, használnia kell a hello **CloudMediaContext** osztály a legtöbb Media Services-programozási feladathoz: tooMedia Services-fiók összekötő; létrehozása, frissítése, elérése és hello következő törlése objektumok: eszközök, eszköz-fájlok, feladatok, hozzáférési házirendek, keresők, stb.</span><span class="sxs-lookup"><span data-stu-id="e4e23-146">When using Media Services with .NET, you must use hello **CloudMediaContext** class for most Media Services programming tasks: connecting tooMedia Services account; creating, updating, accessing, and deleting hello following objects: assets, asset files, jobs, access policies, locators, etc.</span></span>

<span data-ttu-id="e4e23-147">Hello alapértelmezett Program osztályt felülírása a kódját a következő hello.</span><span class="sxs-lookup"><span data-stu-id="e4e23-147">Overwrite hello default Program class with hello following code.</span></span> <span data-ttu-id="e4e23-148">hello kód bemutatja, hogyan tooread hello csatlakozási érték hello App.config fájlból, és hogyan toocreate hello **CloudMediaContext** rendelés tooconnect tooMedia objektum szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="e4e23-148">hello code demonstrates how tooread hello connection values from hello App.config file and how toocreate hello **CloudMediaContext** object in order tooconnect tooMedia Services.</span></span> <span data-ttu-id="e4e23-149">További információkért lásd: [csatlakozás a Media Services API toohello](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="e4e23-149">For more information, see [connecting toohello Media Services API](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

<span data-ttu-id="e4e23-150">Győződjön meg arról, hogy tooupdate hello fájl neve és elérési toowhere rendelkezik a médiafájl.</span><span class="sxs-lookup"><span data-stu-id="e4e23-150">Make sure tooupdate hello file name and path toowhere you have your media file.</span></span>

<span data-ttu-id="e4e23-151">Hello **fő** függvény olyan módszereket, amelyek később lesznek meghatározva hív további ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="e4e23-151">hello **Main** function calls methods that will be defined further in this section.</span></span>

> [!NOTE]
> <span data-ttu-id="e4e23-152">Akkor lesz kell első fordítási hibák összes hello funkciók definícióit hozzáadásáig.</span><span class="sxs-lookup"><span data-stu-id="e4e23-152">You will be getting compilation errors until you add definitions for all hello functions.</span></span>

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

## <a name="create-a-new-asset-and-upload-a-video-file"></a><span data-ttu-id="e4e23-153">Új adategység létrehozása és videofájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="e4e23-153">Create a new asset and upload a video file</span></span>

<span data-ttu-id="e4e23-154">A Media Services szolgáltatásban a digitális fájlok feltöltése vagy kimenete egy adategységbe történik.</span><span class="sxs-lookup"><span data-stu-id="e4e23-154">In Media Services, you upload (or ingest) your digital files into an asset.</span></span> <span data-ttu-id="e4e23-155">Hello **eszköz** entitás tartalmazhat videót, hang, képek, miniatűröket, szöveges nyomon követi, és feliratfájlokat fájlok (és mindezen fájlok metaadatait hello.)  Miután hello fájlok feltöltése után a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.</span><span class="sxs-lookup"><span data-stu-id="e4e23-155">hello **Asset** entity can contain video, audio, images, thumbnail collections, text tracks, and closed caption files (and hello metadata about these files.)  Once hello files are uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span> <span data-ttu-id="e4e23-156">hello eszköz hello fájlok nevezzük **adategység-fájloknak**.</span><span class="sxs-lookup"><span data-stu-id="e4e23-156">hello files in hello asset are called **Asset Files**.</span></span>

<span data-ttu-id="e4e23-157">Hello **UploadFile** hívások az alábbiakban meghatározott metódus **CreateFromFile** (.NET SDK-bővítmények meghatározott).</span><span class="sxs-lookup"><span data-stu-id="e4e23-157">hello **UploadFile** method defined below calls **CreateFromFile** (defined in .NET SDK Extensions).</span></span> <span data-ttu-id="e4e23-158">**CreateFromFile** létrehoz egy új adategységet, mely hello a megadott forrásfájl fel lesz töltve.</span><span class="sxs-lookup"><span data-stu-id="e4e23-158">**CreateFromFile** creates a new asset into which hello specified source file is uploaded.</span></span>

<span data-ttu-id="e4e23-159">Hello **CreateFromFile** metódus **AssetCreationOptions** amellyel meg kell adni hello alábbi adategység-létrehozási lehetőségek egyikét:</span><span class="sxs-lookup"><span data-stu-id="e4e23-159">hello **CreateFromFile** method takes **AssetCreationOptions** which lets you specify one of hello following asset creation options:</span></span>

* <span data-ttu-id="e4e23-160">**Nincs** – Nincs titkosítás.</span><span class="sxs-lookup"><span data-stu-id="e4e23-160">**None** - No encryption is used.</span></span> <span data-ttu-id="e4e23-161">Ez az alapértelmezett érték hello.</span><span class="sxs-lookup"><span data-stu-id="e4e23-161">This is hello default value.</span></span> <span data-ttu-id="e4e23-162">Ügyeljen arra, hogy ezen lehetőség használatakor a tartalom sem átvitel, sem tárolás közben nincs védve.</span><span class="sxs-lookup"><span data-stu-id="e4e23-162">Note that when using this option, your content is not protected in transit or at rest in storage.</span></span>
  <span data-ttu-id="e4e23-163">Ha egy MP4 toodeliver fájlt progresszív letöltés útján tervez, használja ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="e4e23-163">If you plan toodeliver an MP4 using progressive download, use this option.</span></span>
* <span data-ttu-id="e4e23-164">**StorageEncrypted** -használja ezt a beállítást tooencrypt a tiszta tartalom helyileg titkosítással Advanced Encryption Standard (AES)-256 bit, amely tooAzure helyén tárolás titkosítása feltöltését.</span><span class="sxs-lookup"><span data-stu-id="e4e23-164">**StorageEncrypted** - Use this option tooencrypt your clear content locally using Advanced Encryption Standard (AES)-256 bit encryption, which then uploads it tooAzure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="e4e23-165">Storage-titkosítással védett adategységek automatikusan titkosítás és egy titkosított fájl rendszer előzetes tooencoding, és ha szükséges újra titkosítani előzetes toouploading egy új kimeneti eszközként helyezve.</span><span class="sxs-lookup"><span data-stu-id="e4e23-165">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior tooencoding, and optionally re-encrypted prior toouploading back as a new output asset.</span></span> <span data-ttu-id="e4e23-166">a tárolás titkosítása hello elsődleges használati eset az, amikor toosecure a kiváló minőségű bemeneti médiafájljait erős titkosítással aktívan a lemezen.</span><span class="sxs-lookup"><span data-stu-id="e4e23-166">hello primary use case for Storage Encryption is when you want toosecure your high-quality input media files with strong encryption at rest on disk.</span></span>
* <span data-ttu-id="e4e23-167">**CommonEncryptionProtected** – Használja ezt a lehetőséget, ha olyan tartalmat tölt fel, amely már korábban titkosítva és védve lett általános titkosítás vagy a PlayReady DRM által (például egy PlayReady DRM titkosítással védett Smooth Streaming-fájlt).</span><span class="sxs-lookup"><span data-stu-id="e4e23-167">**CommonEncryptionProtected** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="e4e23-168">**EnvelopeEncryptionProtected** – Használja ezt a lehetőséget, ha AES által titkosított HLS tartalmakat tölt fel.</span><span class="sxs-lookup"><span data-stu-id="e4e23-168">**EnvelopeEncryptionProtected** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="e4e23-169">Vegye figyelembe, hogy hello fájlokat kell kódolni és titkosítani Transform Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="e4e23-169">Note that hello files must have been encoded and encrypted by Transform Manager.</span></span>

<span data-ttu-id="e4e23-170">Hello **CreateFromFile** módszert is lehetővé válik annak a visszahívás rendelés tooreport hello feltöltési folyamatáról hello fájl.</span><span class="sxs-lookup"><span data-stu-id="e4e23-170">hello **CreateFromFile** method also lets you specify a callback in order tooreport hello upload progress of hello file.</span></span>

<span data-ttu-id="e4e23-171">A következő példa hello, azt adja meg a **nincs** hello eszköz lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="e4e23-171">In hello following example, we specify **None** for hello asset options.</span></span>

<span data-ttu-id="e4e23-172">Adja hozzá a következő metódus toohello Program osztály hello.</span><span class="sxs-lookup"><span data-stu-id="e4e23-172">Add hello following method toohello Program class.</span></span>

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


## <a name="encode-hello-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a><span data-ttu-id="e4e23-173">Hello forrásfájl kódolása adaptív sávszélességű MP4-fájlsorozattá készletére</span><span class="sxs-lookup"><span data-stu-id="e4e23-173">Encode hello source file into a set of adaptive bitrate MP4 files</span></span>
<span data-ttu-id="e4e23-174">Miután az adategységek bevitele a Media Services, az adathordozó lehet kódolású transmuxed teljesítményjellemzőit, és így tovább, mielőtt továbbítva lennének tooclients.</span><span class="sxs-lookup"><span data-stu-id="e4e23-174">After ingesting assets into Media Services, media can be encoded, transmuxed, watermarked, and so on, before it is delivered tooclients.</span></span> <span data-ttu-id="e4e23-175">Ezek a tevékenységek ütemezett, és több háttér szerepkör példányok tooensure nagy teljesítményt és rendelkezésre állás futtatni.</span><span class="sxs-lookup"><span data-stu-id="e4e23-175">These activities are scheduled and run against multiple background role instances tooensure high performance and availability.</span></span> <span data-ttu-id="e4e23-176">Ezeket a tevékenységeket feladatoknak nevezzük, és minden feladat áll hello hello objektumfájlt valódi munkát atomi feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="e4e23-176">These activities are called Jobs, and each Job is composed of atomic Tasks that do hello actual work on hello Asset file.</span></span>

<span data-ttu-id="e4e23-177">Mivel korábban már említettük, az Azure Media Services használatakor, egyik leggyakoribb forgatókönyve hello elkötelezett az adaptív sávszélességű streamelési tooyour ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="e4e23-177">As was mentioned earlier, when working with Azure Media Services, one of hello most common scenarios is delivering adaptive bitrate streaming tooyour clients.</span></span> <span data-ttu-id="e4e23-178">A Media Services tudja dinamikusan csomagolni adaptív sávszélességű MP4-fájlok készlete hello a következő formátumok egyikét: HTTP Live Streaming (HLS), Smooth Streaming vagy MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="e4e23-178">Media Services can dynamically package a set of adaptive bitrate MP4 files into one of hello following formats: HTTP Live Streaming (HLS), Smooth Streaming, and MPEG DASH.</span></span>

<span data-ttu-id="e4e23-179">tootake előny dinamikus becsomagolás tooencode vagy szükséges alakítható át a mezzanine (forrás) fájlt az adaptív sávszélességű MP4-vagy adaptív sávszélességű Smooth Streaming-fájlsorozattá.</span><span class="sxs-lookup"><span data-stu-id="e4e23-179">tootake advantage of dynamic packaging, you need tooencode or transcode your mezzanine (source) file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files.</span></span>  

<span data-ttu-id="e4e23-180">hello a következő kód bemutatja, hogyan kódolással toosubmit feladat.</span><span class="sxs-lookup"><span data-stu-id="e4e23-180">hello following code shows how toosubmit an encoding job.</span></span> <span data-ttu-id="e4e23-181">hello feladat egyetlen műveletet tartalmaz, amely meghatározza a tootranscode hello mezzazine-fájlt használatával adaptív sávszélességű MP4 készletére **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="e4e23-181">hello job contains one task that specifies tootranscode hello mezzanine file into a set of adaptive bitrate MP4s using **Media Encoder Standard**.</span></span> <span data-ttu-id="e4e23-182">hello kód elküldi hello feladatot, és vár, amíg az befejeződik.</span><span class="sxs-lookup"><span data-stu-id="e4e23-182">hello code submits hello job and waits until it is completed.</span></span>

<span data-ttu-id="e4e23-183">Ha hello feladat befejeződött, lenne képes toostream lehet az objektumot, vagy fokozatosan letölteni az átkódolás során létrejött MP4-fájlok.</span><span class="sxs-lookup"><span data-stu-id="e4e23-183">Once hello job is completed, you would be able toostream your asset or progressively download MP4 files that were created as a result of transcoding.</span></span>

<span data-ttu-id="e4e23-184">Adja hozzá a következő metódus toohello Program osztály hello.</span><span class="sxs-lookup"><span data-stu-id="e4e23-184">Add hello following method toohello Program class.</span></span>

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

## <a name="publish-hello-asset-and-get-urls-for-streaming-and-progressive-download"></a><span data-ttu-id="e4e23-185">Hello objektum közzététele és az adatfolyam-továbbításhoz és progresszív letöltési URL-címek lekérése</span><span class="sxs-lookup"><span data-stu-id="e4e23-185">Publish hello asset and get URLs for streaming and progressive download</span></span>

<span data-ttu-id="e4e23-186">toostream vagy egy eszköz letöltése, akkor először kell túl "közzététele" egy kereső létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="e4e23-186">toostream or download an asset, you first need too"publish" it by creating a locator.</span></span> <span data-ttu-id="e4e23-187">Lokátorok biztosítanak hozzáférést toofiles hello eszköz szerepel.</span><span class="sxs-lookup"><span data-stu-id="e4e23-187">Locators provide access toofiles contained in hello asset.</span></span> <span data-ttu-id="e4e23-188">Media Services két lokátortípust támogat: OnDemandOrigin keresők használt toostream media (például MPEG DASH, HLS vagy Smooth Streaming) és a hozzáférési aláírás (SAS) lokátortípus, használt toodownload médiafájlok (vonatkozó további információ a SAS-keresők lásd: [ez](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog).</span><span class="sxs-lookup"><span data-stu-id="e4e23-188">Media Services supports two types of locators: OnDemandOrigin locators, used toostream media (for example, MPEG DASH, HLS, or Smooth Streaming) and Access Signature (SAS) locators, used toodownload media files (for more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog).</span></span>

### <a name="some-details-about-url-formats"></a><span data-ttu-id="e4e23-189">Néhány információ az URL-formátumokról</span><span class="sxs-lookup"><span data-stu-id="e4e23-189">Some details about URL formats</span></span>

<span data-ttu-id="e4e23-190">Hello keresők létrehozása után hozhat létre hello URL-címeket kell használt toostream vagy töltse le a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="e4e23-190">After you create hello locators, you can build hello URLs that would be used toostream or download your files.</span></span> <span data-ttu-id="e4e23-191">Ebben az oktatóanyagban hello minta fog kimeneti illessze be a megfelelő böngészőkben URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="e4e23-191">hello sample in this tutorial will output URLs that you can paste in appropriate browsers.</span></span> <span data-ttu-id="e4e23-192">Ez a szakasz néhány rövid példán mutatja be a különféle formátumokat.</span><span class="sxs-lookup"><span data-stu-id="e4e23-192">This section just gives short examples of what different formats look like.</span></span>

#### <a name="a-streaming-url-for-mpeg-dash-has-hello-following-format"></a><span data-ttu-id="e4e23-193">MPEG DASH-továbbítási egy URL-formátum a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e4e23-193">A streaming URL for MPEG DASH has hello following format:</span></span>

<span data-ttu-id="e4e23-194">{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest**(format=mpd-time-csf)**</span><span class="sxs-lookup"><span data-stu-id="e4e23-194">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=mpd-time-csf)**</span></span>

#### <a name="a-streaming-url-for-hls-has-hello-following-format"></a><span data-ttu-id="e4e23-195">Egy HLS streamelési URL-címet a következő formátumban hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e4e23-195">A streaming URL for HLS has hello following format:</span></span>

<span data-ttu-id="e4e23-196">{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest**(format=m3u8-aapl)**</span><span class="sxs-lookup"><span data-stu-id="e4e23-196">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=m3u8-aapl)**</span></span>

#### <a name="a-streaming-url-for-smooth-streaming-has-hello-following-format"></a><span data-ttu-id="e4e23-197">Smooth Streaming egy adatfolyam-továbbítási URL-címnek a hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="e4e23-197">A streaming URL for Smooth Streaming has hello following format:</span></span>

<span data-ttu-id="e4e23-198">{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="e4e23-198">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>


#### <a name="a-sas-url-used-toodownload-files-has-hello-following-format"></a><span data-ttu-id="e4e23-199">A használt SAS URL-cím toodownload fájlok hello formátuma a következő esetében:</span><span class="sxs-lookup"><span data-stu-id="e4e23-199">A SAS URL used toodownload files has hello following format:</span></span>

<span data-ttu-id="e4e23-200">{blob-tároló neve}/{adategység neve}/{fájlnév}/{SAS-aláírás}</span><span class="sxs-lookup"><span data-stu-id="e4e23-200">{blob container name}/{asset name}/{file name}/{SAS signature}</span></span>

<span data-ttu-id="e4e23-201">Media Services .NET SDK-bővítmények adja meg, hogy visszatérési URL-címek formázva hello kényelmes segédmódszereket közzétett objektumnak.</span><span class="sxs-lookup"><span data-stu-id="e4e23-201">Media Services .NET SDK extensions provide convenient helper methods that return formatted URLs for hello published asset.</span></span>

<span data-ttu-id="e4e23-202">hello következő használ a .NET SDK-bővítmények toocreate keresők és tooget streaming és a progresszív letöltési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="e4e23-202">hello following code uses .NET SDK Extensions toocreate locators and tooget streaming and progressive download URLs.</span></span> <span data-ttu-id="e4e23-203">hello kód azt is bemutatja, hogyan toodownload fájlok tooa helyi mappát.</span><span class="sxs-lookup"><span data-stu-id="e4e23-203">hello code also shows how toodownload files tooa local folder.</span></span>

<span data-ttu-id="e4e23-204">Adja hozzá a következő metódus toohello Program osztály hello.</span><span class="sxs-lookup"><span data-stu-id="e4e23-204">Add hello following method toohello Program class.</span></span>

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

## <a name="test-by-playing-your-content"></a><span data-ttu-id="e4e23-205">A tartalom lejátszhatóságának tesztelése</span><span class="sxs-lookup"><span data-stu-id="e4e23-205">Test by playing your content</span></span>

<span data-ttu-id="e4e23-206">Hello előző szakaszban meghatározott hello program futtatása után hello hasonló toohello következő jelenik meg a konzolablakban hello URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="e4e23-206">Once you run hello program defined in hello previous section, hello URLs similar toohello following will be displayed in hello console window.</span></span>

<span data-ttu-id="e4e23-207">Adaptív adatfolyam-továbbítási URL-címek:</span><span class="sxs-lookup"><span data-stu-id="e4e23-207">Adaptive streaming URLs:</span></span>

<span data-ttu-id="e4e23-208">Smooth Streaming</span><span class="sxs-lookup"><span data-stu-id="e4e23-208">Smooth Streaming</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

<span data-ttu-id="e4e23-209">HLS</span><span class="sxs-lookup"><span data-stu-id="e4e23-209">HLS</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

<span data-ttu-id="e4e23-210">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="e4e23-210">MPEG DASH</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

<span data-ttu-id="e4e23-211">Fokozatos letöltési URL-címek (audió és videó).</span><span class="sxs-lookup"><span data-stu-id="e4e23-211">Progressive download URLs (audio and video).</span></span>

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


<span data-ttu-id="e4e23-212">toostream a videót, illessze be az URL-cím hello URL-cím textbox hello [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="e4e23-212">toostream your video, paste your URL in hello URL textbox in hello [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

<span data-ttu-id="e4e23-213">tootest progresszív letöltés, illessze be egy URL-címet a böngészőjébe (például az Internet Explorer, Chrome vagy Safari).</span><span class="sxs-lookup"><span data-stu-id="e4e23-213">tootest progressive download, paste a URL into a browser (for example, Internet Explorer, Chrome, or Safari).</span></span>

<span data-ttu-id="e4e23-214">További információkért tekintse meg a következő témakörök hello:</span><span class="sxs-lookup"><span data-stu-id="e4e23-214">For more information, see hello following topics:</span></span>

- [<span data-ttu-id="e4e23-215">A tartalom lejátszása meglévő lejátszókkal</span><span class="sxs-lookup"><span data-stu-id="e4e23-215">Playing your content with existing players</span></span>](media-services-playback-content-with-existing-players.md)
- [<span data-ttu-id="e4e23-216">Videolejátszó alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="e4e23-216">Develop video player applications</span></span>](media-services-develop-video-players.md)
- [<span data-ttu-id="e4e23-217">MPEG-DASH adaptív streamelt videók beágyazása DASH.js-sel rendelkező HTML5-alkalmazásba</span><span class="sxs-lookup"><span data-stu-id="e4e23-217">Embedding a MPEG-DASH Adaptive Streaming Video in an HTML5 Application with DASH.js</span></span>](media-services-embed-mpeg-dash-in-html5.md)

## <a name="download-sample"></a><span data-ttu-id="e4e23-218">Minta letöltése</span><span class="sxs-lookup"><span data-stu-id="e4e23-218">Download sample</span></span>
<span data-ttu-id="e4e23-219">hello alábbi kódminta hello kódot tartalmaz, amely ebben az oktatóanyagban létrehozott: [minta](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="e4e23-219">hello following code sample contains hello code that you created in this tutorial: [sample](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4e23-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4e23-220">Next Steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e4e23-221">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="e4e23-221">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



<!-- Anchors. -->


<!-- URLs. -->
[Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
[Portal]: http://manage.windowsazure.com/
