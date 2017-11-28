---
title: "Tartalmak továbbítása igény szerint a .NET használatával | Microsoft Docs"
description: "Ez az útmutató lépésről lépésre ismerteti, hogyan valósíthat meg egy igény szerinti tartalomtovábbító alkalmazást a .NET-keretrendszert használó Azure Media Services segítségével."
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
ms.openlocfilehash: f0be787ba1ccee067fb1d7e6a6554be32f886089
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a><span data-ttu-id="da4fb-103">Tartalmak továbbítása igény szerint a .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="da4fb-103">Get started with delivering content on demand using .NET SDK</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="da4fb-104">Ez az oktatóanyag végigvezeti a lépéseken, amelyek segítségével alapszintű igény szerinti videotartalom-továbbítási szolgáltatást hozhat létre az Azure Media Services .NET SDK segítségével, az Azure Media Services (AMS) alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="da4fb-104">This tutorial walks you through the steps of implementing a basic Video-on-Demand (VoD) content delivery service with Azure Media Services (AMS) application using the Azure Media Services .NET SDK.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="da4fb-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="da4fb-105">Prerequisites</span></span>

<span data-ttu-id="da4fb-106">Az ismertetett eljárás végrehajtásához a következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="da4fb-106">The following are required to complete the tutorial:</span></span>

* <span data-ttu-id="da4fb-107">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="da4fb-107">An Azure account.</span></span> <span data-ttu-id="da4fb-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="da4fb-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="da4fb-109">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="da4fb-109">A Media Services account.</span></span> <span data-ttu-id="da4fb-110">A Media Services-fiók létrehozásáról a [Media Services-fiók létrehozása](media-services-portal-create-account.md) című cikk nyújt tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="da4fb-110">To create a Media Services account, see [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="da4fb-111">A .NET-keretrendszer 4.0-s vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="da4fb-111">.NET Framework 4.0 or later.</span></span>
* <span data-ttu-id="da4fb-112">Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="da4fb-112">Visual Studio.</span></span>

<span data-ttu-id="da4fb-113">Az oktatóanyag a következő feladatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="da4fb-113">This tutorial includes the following tasks:</span></span>

1. <span data-ttu-id="da4fb-114">A streamvégpont elindítása (az Azure Portal használatával).</span><span class="sxs-lookup"><span data-stu-id="da4fb-114">Start streaming endpoint (using the Azure portal).</span></span>
2. <span data-ttu-id="da4fb-115">Egy Visual Studio-projekt létrehozása és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="da4fb-115">Create and configure a Visual Studio project.</span></span>
3. <span data-ttu-id="da4fb-116">A Media Services-fiókhoz való csatlakozás.</span><span class="sxs-lookup"><span data-stu-id="da4fb-116">Connect to the Media Services account.</span></span>
2. <span data-ttu-id="da4fb-117">Videofájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="da4fb-117">Upload a video file.</span></span>
3. <span data-ttu-id="da4fb-118">Forrásfájl kódolása adaptív sávszélességű MP4-fájlokká</span><span class="sxs-lookup"><span data-stu-id="da4fb-118">Encode the source file into a set of adaptive bitrate MP4 files.</span></span>
4. <span data-ttu-id="da4fb-119">Az objektum közzététele, majd a streamelési és a progresszív letöltési URL-cím lekérése</span><span class="sxs-lookup"><span data-stu-id="da4fb-119">Publish the asset and get streaming and progressive download URLs.</span></span>  
5. <span data-ttu-id="da4fb-120">Tartalom lejátszása</span><span class="sxs-lookup"><span data-stu-id="da4fb-120">Play your content.</span></span>

## <a name="overview"></a><span data-ttu-id="da4fb-121">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="da4fb-121">Overview</span></span>
<span data-ttu-id="da4fb-122">Ez az útmutató lépésről lépésre bemutatja, hogyan valósíthat meg egy Video-on-Demand (VoD) tartalomtovábbító alkalmazást a .NET-keretrendszerhez készült Azure Media Services (AMS) SDK segítségével.</span><span class="sxs-lookup"><span data-stu-id="da4fb-122">This tutorial walks you through the steps of implementing a Video-on-Demand (VoD) content delivery application using Azure Media Services (AMS) SDK for .NET.</span></span>

<span data-ttu-id="da4fb-123">Az útmutató bemutatja a Media Services alapvető munkafolyamatait és a Media Services-fejlesztéshez szükséges leggyakoribb programozási objektumokat és feladatokat.</span><span class="sxs-lookup"><span data-stu-id="da4fb-123">The tutorial introduces the basic Media Services workflow and the most common programming objects and tasks required for Media Services development.</span></span> <span data-ttu-id="da4fb-124">Az oktatóprogram elvégzése után képes lesz adatfolyamot továbbítani vagy fokozatosan letölteni egy saját maga által feltöltött, kódolt és letöltött példa médiafájlt.</span><span class="sxs-lookup"><span data-stu-id="da4fb-124">At the completion of the tutorial, you will be able to stream or progressively download a sample media file that you uploaded, encoded, and downloaded.</span></span>

### <a name="ams-model"></a><span data-ttu-id="da4fb-125">AMS-modell</span><span class="sxs-lookup"><span data-stu-id="da4fb-125">AMS model</span></span>

<span data-ttu-id="da4fb-126">A következő kép a Media Services OData-modellen alapuló VoD-alkalmazásfejlesztések során leggyakrabban használt objektumok közül mutat be néhányat.</span><span class="sxs-lookup"><span data-stu-id="da4fb-126">The following image shows some of the most commonly used objects when developing VoD applications against the Media Services OData model.</span></span>

<span data-ttu-id="da4fb-127">Kattintson a képre a teljes méretű megjelenítéshez.</span><span class="sxs-lookup"><span data-stu-id="da4fb-127">Click the image to view it full size.</span></span>  

<a href="./media/media-services-dotnet-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-dotnet-get-started/media-services-overview-object-model-small.png"></a> 

<span data-ttu-id="da4fb-128">A teljes modellt [itt](https://media.windows.net/API/$metadata?api-version=2.15) tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="da4fb-128">You can view the whole model [here](https://media.windows.net/API/$metadata?api-version=2.15).</span></span>  

## <a name="start-streaming-endpoints-using-the-azure-portal"></a><span data-ttu-id="da4fb-129">A streamvégpont elindítása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="da4fb-129">Start streaming endpoints using the Azure portal</span></span>

<span data-ttu-id="da4fb-130">Az Azure Media Services egyik leggyakrabban használt funkciója a videók továbbítása az adaptív sávszélességű streamelés használatával.</span><span class="sxs-lookup"><span data-stu-id="da4fb-130">When working with Azure Media Services one of the most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="da4fb-131">A Media Services dinamikus csomagolást biztosít, amelynek köszönhetően adaptív sávszélességű, MP4 formátumban kódolt tartalmait a Media Services által támogatott streamformátumok valamelyikében (MPEG DASH, HLS, Smooth Streaming) továbbíthatja igény szerint, mindezt anélkül, hogy az adott formátumban előcsomagolt verziót tárolna.</span><span class="sxs-lookup"><span data-stu-id="da4fb-131">Media Services provides dynamic packaging, which allows you to deliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having to store pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="da4fb-132">Az AMS-fiók létrehozásakor a rendszer hozzáad egy **alapértelmezett** streamvégpontot a fiókhoz **Leállítva** állapotban.</span><span class="sxs-lookup"><span data-stu-id="da4fb-132">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="da4fb-133">A tartalom streamelésének megkezdéséhez, valamint a dinamikus csomagolás és a dinamikus titkosítás kihasználásához a tartalomstreameléshez használt streamvégpontnak **Fut** állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="da4fb-133">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span>

<span data-ttu-id="da4fb-134">A streamvégpont elindításához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="da4fb-134">To start the streaming endpoint, do the following:</span></span>

1. <span data-ttu-id="da4fb-135">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="da4fb-135">Log in at the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="da4fb-136">Kattintson a Settings (Beállítások) ablak Streaming endpoints (Streamvégpontok) elemére.</span><span class="sxs-lookup"><span data-stu-id="da4fb-136">In the Settings window, click Streaming endpoints.</span></span>
3. <span data-ttu-id="da4fb-137">Kattintson az alapértelmezett streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="da4fb-137">Click the default streaming endpoint.</span></span>

    <span data-ttu-id="da4fb-138">Megjelenik a DEFAULT STREAMING ENDPOINT DETAILS (Alapértelmezett streamvégpont adatai) ablak.</span><span class="sxs-lookup"><span data-stu-id="da4fb-138">The DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="da4fb-139">Kattintson a Start ikonra.</span><span class="sxs-lookup"><span data-stu-id="da4fb-139">Click the Start icon.</span></span>
5. <span data-ttu-id="da4fb-140">Mentse a módosításokat a Save (Mentés) gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="da4fb-140">Click the Save button to save your changes.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="da4fb-141">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="da4fb-141">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="da4fb-142">Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="da4fb-142">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="da4fb-143">Hozzon létre egy új mappát (a mappa a helyi meghajtón bárhol lehet), és másoljon bele egy .mp4-fájlt, amelyet szeretne kódolni vagy fokozatosan letölteni.</span><span class="sxs-lookup"><span data-stu-id="da4fb-143">Create a new folder (folder can be anywhere on your local drive) and copy an .mp4 file that you want to encode and stream or progressively download.</span></span> <span data-ttu-id="da4fb-144">Ebben a példában a „C:\VideoFiles” elérési utat használjuk.</span><span class="sxs-lookup"><span data-stu-id="da4fb-144">In this example, the "C:\VideoFiles" path is used.</span></span>

## <a name="connect-to-the-media-services-account"></a><span data-ttu-id="da4fb-145">Csatlakozás a Media Services-fiókhoz</span><span class="sxs-lookup"><span data-stu-id="da4fb-145">Connect to the Media Services account</span></span>

<span data-ttu-id="da4fb-146">A .NET-keretrendszerű Media Services-szolgáltatások használatakor a **CloudMediaContext** osztályt kell használnia a legtöbb Media Services-programozási feladathoz – ilyenek például a Media Services-fiókhoz való csatlakozás, továbbá az adategységek, adategységfájlok, feladatok, hozzáférési házirendek, keresők és egyebek létrehozása, frissítése, elérése és törlése.</span><span class="sxs-lookup"><span data-stu-id="da4fb-146">When using Media Services with .NET, you must use the **CloudMediaContext** class for most Media Services programming tasks: connecting to Media Services account; creating, updating, accessing, and deleting the following objects: assets, asset files, jobs, access policies, locators, etc.</span></span>

<span data-ttu-id="da4fb-147">Írja felül az alapértelmezett Program osztályt a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="da4fb-147">Overwrite the default Program class with the following code.</span></span> <span data-ttu-id="da4fb-148">A kód bemutatja, hogyan olvashatja be a csatlakozási értékeket az App.config fájlból, és hogyan hozhatja létre a Media Services-csatlakozáshoz szükséges **CloudMediaContext** objektumot.</span><span class="sxs-lookup"><span data-stu-id="da4fb-148">The code demonstrates how to read the connection values from the App.config file and how to create the **CloudMediaContext** object in order to connect to Media Services.</span></span> <span data-ttu-id="da4fb-149">További információ: [Connecting to the Media Services API](media-services-use-aad-auth-to-access-ams-api.md) (Csatlakozás a Media Services API-hoz).</span><span class="sxs-lookup"><span data-stu-id="da4fb-149">For more information, see [connecting to the Media Services API](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

<span data-ttu-id="da4fb-150">Ne feledje frissíteni a fájl nevét és elérési útvonalát a médiafájl helyének megfelelően.</span><span class="sxs-lookup"><span data-stu-id="da4fb-150">Make sure to update the file name and path to where you have your media file.</span></span>

<span data-ttu-id="da4fb-151">A **Fő** függvény olyan módszereket hív meg, amelyek jelen szakasz során később lesznek meghatározva.</span><span class="sxs-lookup"><span data-stu-id="da4fb-151">The **Main** function calls methods that will be defined further in this section.</span></span>

> [!NOTE]
> <span data-ttu-id="da4fb-152">Amíg nem veszi fel az összes függvénydefinícióit, a rendszer fordítási hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="da4fb-152">You will be getting compilation errors until you add definitions for all the functions.</span></span>

    class Program
    {
        // Read values from the App.config file.
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

            // Add calls to methods defined in this section.
            // Make sure to update the file name and path to where you have your media file.
            IAsset inputAsset =
            UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

            IAsset encodedAsset =
            EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

            PublishAssetGetURLs(encodedAsset);
        }
        catch (Exception exception)
        {
            // Parse the XML error message in the Media Services response and create a new
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

## <a name="create-a-new-asset-and-upload-a-video-file"></a><span data-ttu-id="da4fb-153">Új adategység létrehozása és videofájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="da4fb-153">Create a new asset and upload a video file</span></span>

<span data-ttu-id="da4fb-154">A Media Services szolgáltatásban a digitális fájlok feltöltése vagy kimenete egy adategységbe történik.</span><span class="sxs-lookup"><span data-stu-id="da4fb-154">In Media Services, you upload (or ingest) your digital files into an asset.</span></span> <span data-ttu-id="da4fb-155">Az **Adategység** entitás tartalmazhat videót, hangot, képeket, miniatűröket, szövegsávokat és feliratfájlokat (valamint mindezen fájlok metaadatait).  A fájlok feltöltése után a tartalom a felhőben lesz biztonságosan tárolva további feldolgozás és adatfolyam-továbbítás céljából.</span><span class="sxs-lookup"><span data-stu-id="da4fb-155">The **Asset** entity can contain video, audio, images, thumbnail collections, text tracks, and closed caption files (and the metadata about these files.)  Once the files are uploaded, your content is stored securely in the cloud for further processing and streaming.</span></span> <span data-ttu-id="da4fb-156">Az adategységben található fájlokat **adategység-fájloknak** nevezzük.</span><span class="sxs-lookup"><span data-stu-id="da4fb-156">The files in the asset are called **Asset Files**.</span></span>

<span data-ttu-id="da4fb-157">Az alábbiakban meghatározott **UploadFile** módszer a **CreateFromFile** módszert hívja meg (amely a .NET SDK-bővítmények között van meghatározva).</span><span class="sxs-lookup"><span data-stu-id="da4fb-157">The **UploadFile** method defined below calls **CreateFromFile** (defined in .NET SDK Extensions).</span></span> <span data-ttu-id="da4fb-158">A **CreateFromFile** létrehoz egy új adategységet, amelybe a megadott forrásfájl fel lesz töltve.</span><span class="sxs-lookup"><span data-stu-id="da4fb-158">**CreateFromFile** creates a new asset into which the specified source file is uploaded.</span></span>

<span data-ttu-id="da4fb-159">A **CreateFromFile** módszer számára az **AssetCreationOptions** alapján határozhatja meg, hogy az alábbi adategység-létrehozási lehetőségek közül melyiket használja:</span><span class="sxs-lookup"><span data-stu-id="da4fb-159">The **CreateFromFile** method takes **AssetCreationOptions** which lets you specify one of the following asset creation options:</span></span>

* <span data-ttu-id="da4fb-160">**Nincs** – Nincs titkosítás.</span><span class="sxs-lookup"><span data-stu-id="da4fb-160">**None** - No encryption is used.</span></span> <span data-ttu-id="da4fb-161">Ez az alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="da4fb-161">This is the default value.</span></span> <span data-ttu-id="da4fb-162">Ügyeljen arra, hogy ezen lehetőség használatakor a tartalom sem átvitel, sem tárolás közben nincs védve.</span><span class="sxs-lookup"><span data-stu-id="da4fb-162">Note that when using this option, your content is not protected in transit or at rest in storage.</span></span>
  <span data-ttu-id="da4fb-163">Ha egy MP4-fájlt progresszív letöltés útján tervez továbbítani, használja ezt a lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="da4fb-163">If you plan to deliver an MP4 using progressive download, use this option.</span></span>
* <span data-ttu-id="da4fb-164">**StorageEncrypted** – Ezen lehetőség használatakor a tiszta tartalom helyileg, 256 bites Advanced Encryption Standard (AES) titkosítással lesz titkosítva, és így kerül feltöltésre az Azure Storage tárolóba, ahol titkosítva lesz tárolva.</span><span class="sxs-lookup"><span data-stu-id="da4fb-164">**StorageEncrypted** - Use this option to encrypt your clear content locally using Advanced Encryption Standard (AES)-256 bit encryption, which then uploads it to Azure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="da4fb-165">A Storage-titkosítással védett adategységek titkosítása a kódolás előtt automatikusan fel lesz oldva, és egy titkosított fájlrendszerbe kerülnek; az új kimeneti adategységként való újbóli feltöltés előtt pedig lehetőség van az újbóli titkosításukra.</span><span class="sxs-lookup"><span data-stu-id="da4fb-165">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior to encoding, and optionally re-encrypted prior to uploading back as a new output asset.</span></span> <span data-ttu-id="da4fb-166">A Storage-titkosítás elsősorban akkor hasznos, ha a kiváló minőségű bemeneti médiafájljait erős titkosítással szeretné védeni a lemezen való tároláskor.</span><span class="sxs-lookup"><span data-stu-id="da4fb-166">The primary use case for Storage Encryption is when you want to secure your high-quality input media files with strong encryption at rest on disk.</span></span>
* <span data-ttu-id="da4fb-167">**CommonEncryptionProtected** – Használja ezt a lehetőséget, ha olyan tartalmat tölt fel, amely már korábban titkosítva és védve lett általános titkosítás vagy a PlayReady DRM által (például egy PlayReady DRM titkosítással védett Smooth Streaming-fájlt).</span><span class="sxs-lookup"><span data-stu-id="da4fb-167">**CommonEncryptionProtected** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="da4fb-168">**EnvelopeEncryptionProtected** – Használja ezt a lehetőséget, ha AES által titkosított HLS tartalmakat tölt fel.</span><span class="sxs-lookup"><span data-stu-id="da4fb-168">**EnvelopeEncryptionProtected** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="da4fb-169">Megjegyzés: ehhez a fájlokat a Transform Manager használatával kell kódolni és titkosítani.</span><span class="sxs-lookup"><span data-stu-id="da4fb-169">Note that the files must have been encoded and encrypted by Transform Manager.</span></span>

<span data-ttu-id="da4fb-170">A **CreateFromFile** módszer használatával egy visszahívást is megadhat, amely visszajelzést ad a fájl feltöltési folyamatáról.</span><span class="sxs-lookup"><span data-stu-id="da4fb-170">The **CreateFromFile** method also lets you specify a callback in order to report the upload progress of the file.</span></span>

<span data-ttu-id="da4fb-171">A következő példában az adategység lehetőségei közt a **Nincs** választ adtuk meg.</span><span class="sxs-lookup"><span data-stu-id="da4fb-171">In the following example, we specify **None** for the asset options.</span></span>

<span data-ttu-id="da4fb-172">Adja hozzá a Program osztályhoz a következő módszert.</span><span class="sxs-lookup"><span data-stu-id="da4fb-172">Add the following method to the Program class.</span></span>

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


## <a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a><span data-ttu-id="da4fb-173">Forrásfájl kódolása adaptív sávszélességű MP4-fájlsorozattá</span><span class="sxs-lookup"><span data-stu-id="da4fb-173">Encode the source file into a set of adaptive bitrate MP4 files</span></span>
<span data-ttu-id="da4fb-174">Miután az adategységek bevitele a Media Services szolgáltatásba megtörtént, a médiatartalmak többek között kódolhatók, transzmultiplexálás végezhető rajtuk vagy vízjelezhetők, mielőtt továbbítva lennének az ügyfelek felé.</span><span class="sxs-lookup"><span data-stu-id="da4fb-174">After ingesting assets into Media Services, media can be encoded, transmuxed, watermarked, and so on, before it is delivered to clients.</span></span> <span data-ttu-id="da4fb-175">Ezen tevékenységek több háttérbeli szerepkörpéldányhoz képest vannak ütemezve és futtatva a magas teljesítmény és rendelkezésre állás biztosítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="da4fb-175">These activities are scheduled and run against multiple background role instances to ensure high performance and availability.</span></span> <span data-ttu-id="da4fb-176">Ezeket a tevékenységeket feladatoknak nevezzük. Minden egyes feladat több részműveletből áll, ezek végzik el a valódi munkát az adategységfájlon.</span><span class="sxs-lookup"><span data-stu-id="da4fb-176">These activities are called Jobs, and each Job is composed of atomic Tasks that do the actual work on the Asset file.</span></span>

<span data-ttu-id="da4fb-177">Mint azt korábban már említettük, az Azure Media Services használatának egyik leggyakoribb forgatókönyve az adaptív sávszélességű streamelés az ügyfelek felé.</span><span class="sxs-lookup"><span data-stu-id="da4fb-177">As was mentioned earlier, when working with Azure Media Services, one of the most common scenarios is delivering adaptive bitrate streaming to your clients.</span></span> <span data-ttu-id="da4fb-178">A Media Services az adaptív sávszélességű MP4-fájlokat a következő formátumokba tudja dinamikusan csomagolni: HTTP Live Streaming (HLS), Smooth Streaming és MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="da4fb-178">Media Services can dynamically package a set of adaptive bitrate MP4 files into one of the following formats: HTTP Live Streaming (HLS), Smooth Streaming, and MPEG DASH.</span></span>

<span data-ttu-id="da4fb-179">A dinamikus csomagolás kihasználásához kódolja a mezzanine forrásfájlt egy adaptív sávszélességű MP4- vagy Smooth Streaming-fájlsorozattá.</span><span class="sxs-lookup"><span data-stu-id="da4fb-179">To take advantage of dynamic packaging, you need to encode or transcode your mezzanine (source) file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files.</span></span>  

<span data-ttu-id="da4fb-180">A következő kód bemutatja, hogyan küldhet el egy kódolási feladatot.</span><span class="sxs-lookup"><span data-stu-id="da4fb-180">The following code shows how to submit an encoding job.</span></span> <span data-ttu-id="da4fb-181">A feladat egyetlen műveletet tartalmaz, amely azért felel, hogy a mezzazine-fájlt egy adaptív sávszélességű MP4-fájlsorozattá kódolódjon át a **Media Encoder Standard** használatával.</span><span class="sxs-lookup"><span data-stu-id="da4fb-181">The job contains one task that specifies to transcode the mezzanine file into a set of adaptive bitrate MP4s using **Media Encoder Standard**.</span></span> <span data-ttu-id="da4fb-182">A kód elküldi a feladatot, és vár, amíg az befejeződik.</span><span class="sxs-lookup"><span data-stu-id="da4fb-182">The code submits the job and waits until it is completed.</span></span>

<span data-ttu-id="da4fb-183">Ha a feladat befejeződött, lehetővé válik az adategység továbbítása vagy az átkódolás során létrejött MP4-fájlok progresszív letöltése.</span><span class="sxs-lookup"><span data-stu-id="da4fb-183">Once the job is completed, you would be able to stream your asset or progressively download MP4 files that were created as a result of transcoding.</span></span>

<span data-ttu-id="da4fb-184">Adja hozzá a Program osztályhoz a következő módszert.</span><span class="sxs-lookup"><span data-stu-id="da4fb-184">Add the following method to the Program class.</span></span>

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {

        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.

        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "Adaptive Streaming",
            asset,
            "Adaptive Bitrate MP4",
            options);

        Console.WriteLine("Submitting transcoding job...");


        // Submit the job and wait until it is completed.
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

## <a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a><span data-ttu-id="da4fb-185">Adatkészlet közzététele és az adatfolyam-továbbításhoz és progresszív letöltéshez szükséges URL-címek lekérése</span><span class="sxs-lookup"><span data-stu-id="da4fb-185">Publish the asset and get URLs for streaming and progressive download</span></span>

<span data-ttu-id="da4fb-186">Egy adategység továbbításához vagy letöltéséhez először a „közzététele” szükséges, egy kereső létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="da4fb-186">To stream or download an asset, you first need to "publish" it by creating a locator.</span></span> <span data-ttu-id="da4fb-187">Az objektumban található fájlokhoz a lokátorok biztosítanak hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="da4fb-187">Locators provide access to files contained in the asset.</span></span> <span data-ttu-id="da4fb-188">A Media Services kétféle keresőtípust támogat: az OnDemandOrigin keresők médiatartalmak továbbításához használatosak (például MPEG DASH, HLS vagy Smooth Streaming), a közös hozzáférésű jogosultságkódos (SAS) keresők pedig médiafájlok letöltéséhez (a SAS-keresőkkel kapcsolatos további információkért lásd [ezt](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) a blogot).</span><span class="sxs-lookup"><span data-stu-id="da4fb-188">Media Services supports two types of locators: OnDemandOrigin locators, used to stream media (for example, MPEG DASH, HLS, or Smooth Streaming) and Access Signature (SAS) locators, used to download media files (for more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog).</span></span>

### <a name="some-details-about-url-formats"></a><span data-ttu-id="da4fb-189">Néhány információ az URL-formátumokról</span><span class="sxs-lookup"><span data-stu-id="da4fb-189">Some details about URL formats</span></span>

<span data-ttu-id="da4fb-190">A keresők létrehozása után összeállíthatja a fájlok továbbításához vagy letöltéséhez használandó URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="da4fb-190">After you create the locators, you can build the URLs that would be used to stream or download your files.</span></span> <span data-ttu-id="da4fb-191">Az oktatóanyagban lévő minta kimenetei olyan URL-címek, amelyek a megfelelő böngészőkbe beilleszthetőek.</span><span class="sxs-lookup"><span data-stu-id="da4fb-191">The sample in this tutorial will output URLs that you can paste in appropriate browsers.</span></span> <span data-ttu-id="da4fb-192">Ez a szakasz néhány rövid példán mutatja be a különféle formátumokat.</span><span class="sxs-lookup"><span data-stu-id="da4fb-192">This section just gives short examples of what different formats look like.</span></span>

#### <a name="a-streaming-url-for-mpeg-dash-has-the-following-format"></a><span data-ttu-id="da4fb-193">Egy MPEG DASH-továbbítási URL-címnek a következő formátumban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="da4fb-193">A streaming URL for MPEG DASH has the following format:</span></span>

<span data-ttu-id="da4fb-194">{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest**(format=mpd-time-csf)**</span><span class="sxs-lookup"><span data-stu-id="da4fb-194">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=mpd-time-csf)**</span></span>

#### <a name="a-streaming-url-for-hls-has-the-following-format"></a><span data-ttu-id="da4fb-195">Egy HLS-továbbítási URL-címnek a következő formátumban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="da4fb-195">A streaming URL for HLS has the following format:</span></span>

<span data-ttu-id="da4fb-196">{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest**(format=m3u8-aapl)**</span><span class="sxs-lookup"><span data-stu-id="da4fb-196">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=m3u8-aapl)**</span></span>

#### <a name="a-streaming-url-for-smooth-streaming-has-the-following-format"></a><span data-ttu-id="da4fb-197">Egy Smooth Streaming URL-címnek a következő formátumban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="da4fb-197">A streaming URL for Smooth Streaming has the following format:</span></span>

<span data-ttu-id="da4fb-198">{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="da4fb-198">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>


#### <a name="a-sas-url-used-to-download-files-has-the-following-format"></a><span data-ttu-id="da4fb-199">Egy fájlok letöltéséhez használt SAS URL-címnek a következő formátumban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="da4fb-199">A SAS URL used to download files has the following format:</span></span>

<span data-ttu-id="da4fb-200">{blob-tároló neve}/{adategység neve}/{fájlnév}/{SAS-aláírás}</span><span class="sxs-lookup"><span data-stu-id="da4fb-200">{blob container name}/{asset name}/{file name}/{SAS signature}</span></span>

<span data-ttu-id="da4fb-201">A Media Services .NET SDK-bővítmények olyan kényelmes segédmódszereket biztosítanak, amelyek formázott URL-címeket adnak meg a közzétett adategységekhez.</span><span class="sxs-lookup"><span data-stu-id="da4fb-201">Media Services .NET SDK extensions provide convenient helper methods that return formatted URLs for the published asset.</span></span>

<span data-ttu-id="da4fb-202">A következő kód .NET SDK-bővítmények használatával hoz létre keresőket és kér le adatfolyam-továbbítási és progresszív letöltési URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="da4fb-202">The following code uses .NET SDK Extensions to create locators and to get streaming and progressive download URLs.</span></span> <span data-ttu-id="da4fb-203">A kód azt is bemutatja, hogyan tölthetők le fájlok egy helyi mappába.</span><span class="sxs-lookup"><span data-stu-id="da4fb-203">The code also shows how to download files to a local folder.</span></span>

<span data-ttu-id="da4fb-204">Adja hozzá a Program osztályhoz a következő módszert.</span><span class="sxs-lookup"><span data-stu-id="da4fb-204">Add the following method to the Program class.</span></span>

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
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

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

## <a name="test-by-playing-your-content"></a><span data-ttu-id="da4fb-205">A tartalom lejátszhatóságának tesztelése</span><span class="sxs-lookup"><span data-stu-id="da4fb-205">Test by playing your content</span></span>

<span data-ttu-id="da4fb-206">Az előző szakaszban meghatározott program futtatásakor a konzolablakban a következőkhöz hasonló URL-címek jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="da4fb-206">Once you run the program defined in the previous section, the URLs similar to the following will be displayed in the console window.</span></span>

<span data-ttu-id="da4fb-207">Adaptív adatfolyam-továbbítási URL-címek:</span><span class="sxs-lookup"><span data-stu-id="da4fb-207">Adaptive streaming URLs:</span></span>

<span data-ttu-id="da4fb-208">Smooth Streaming</span><span class="sxs-lookup"><span data-stu-id="da4fb-208">Smooth Streaming</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

<span data-ttu-id="da4fb-209">HLS</span><span class="sxs-lookup"><span data-stu-id="da4fb-209">HLS</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

<span data-ttu-id="da4fb-210">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="da4fb-210">MPEG DASH</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

<span data-ttu-id="da4fb-211">Fokozatos letöltési URL-címek (audió és videó).</span><span class="sxs-lookup"><span data-stu-id="da4fb-211">Progressive download URLs (audio and video).</span></span>

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


<span data-ttu-id="da4fb-212">A videótovábbításhoz illessze be az URL-címet az [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) lejátszó URL szövegmezőjébe.</span><span class="sxs-lookup"><span data-stu-id="da4fb-212">To stream your video, paste your URL in the URL textbox in the [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

<span data-ttu-id="da4fb-213">A progresszív letöltés teszteléséhez másoljon egy URL-címet a böngészőjébe (például az Internet Explorerbe, Chrome-ba vagy Safariba).</span><span class="sxs-lookup"><span data-stu-id="da4fb-213">To test progressive download, paste a URL into a browser (for example, Internet Explorer, Chrome, or Safari).</span></span>

<span data-ttu-id="da4fb-214">További információkért tekintse át a következők témaköröket:</span><span class="sxs-lookup"><span data-stu-id="da4fb-214">For more information, see the following topics:</span></span>

- [<span data-ttu-id="da4fb-215">A tartalom lejátszása meglévő lejátszókkal</span><span class="sxs-lookup"><span data-stu-id="da4fb-215">Playing your content with existing players</span></span>](media-services-playback-content-with-existing-players.md)
- [<span data-ttu-id="da4fb-216">Videolejátszó alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="da4fb-216">Develop video player applications</span></span>](media-services-develop-video-players.md)
- [<span data-ttu-id="da4fb-217">MPEG-DASH adaptív streamelt videók beágyazása DASH.js-sel rendelkező HTML5-alkalmazásba</span><span class="sxs-lookup"><span data-stu-id="da4fb-217">Embedding a MPEG-DASH Adaptive Streaming Video in an HTML5 Application with DASH.js</span></span>](media-services-embed-mpeg-dash-in-html5.md)

## <a name="download-sample"></a><span data-ttu-id="da4fb-218">Minta letöltése</span><span class="sxs-lookup"><span data-stu-id="da4fb-218">Download sample</span></span>
<span data-ttu-id="da4fb-219">Az oktatóanyagban létrehozott kódot a következő kód tartalmazza: [minta](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="da4fb-219">The following code sample contains the code that you created in this tutorial: [sample](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="da4fb-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="da4fb-220">Next Steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="da4fb-221">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="da4fb-221">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



<!-- Anchors. -->


<!-- URLs. -->
[Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
[Portal]: http://manage.windowsazure.com/
