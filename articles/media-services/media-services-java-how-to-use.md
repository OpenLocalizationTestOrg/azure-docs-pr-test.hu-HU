---
title: "Igény szerinti tartalomtovábbítás Java használatával | Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti a lépéseken, amelyek segítségével alapszintű igény szerinti videotartalom-továbbítási szolgáltatást hozhat létre a Java segítségével, az Azure Media Services (AMS) alkalmazással."
services: media-services
documentationcenter: java
author: juliako
manager: cfowler
editor: 
ms.assetid: b884bd61-dbdb-42ea-b170-8fb02e7fded7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: get-started-article
ms.date: 01/10/2017
ms.author: juliako
ms.openlocfilehash: 2294f3de094389f8aa500c75472e753339b18358
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-java"></a><span data-ttu-id="7af52-103">Igény szerinti tartalomtovábbítás Java használatával</span><span class="sxs-lookup"><span data-stu-id="7af52-103">Get started with delivering content on demand using Java</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="7af52-104">Ez az oktatóanyag végigvezeti a lépéseken, amelyek segítségével alapszintű igény szerinti videotartalom-továbbítási szolgáltatást hozhat létre a Java segítségével, az Azure Media Services (AMS) alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="7af52-104">This tutorial walks you through the steps of implementing a basic Video-on-Demand (VoD) content delivery service with Azure Media Services (AMS) application using Java.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7af52-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7af52-105">Prerequisites</span></span>

<span data-ttu-id="7af52-106">Az ismertetett eljárás végrehajtásához a következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="7af52-106">The following are required to complete the tutorial:</span></span>

* <span data-ttu-id="7af52-107">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="7af52-107">An Azure account.</span></span> <span data-ttu-id="7af52-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7af52-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="7af52-109">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="7af52-109">A Media Services account.</span></span> <span data-ttu-id="7af52-110">A Media Services-fiók létrehozásáról a [Media Services-fiók létrehozása](media-services-portal-create-account.md) című cikk nyújt tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="7af52-110">To create a Media Services account, see [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="7af52-111">A Javához készült Azure-könyvtárak, amelyeket az [Azure Java fejlesztői központból][Azure Java Developer Center] lehet telepíteni.</span><span class="sxs-lookup"><span data-stu-id="7af52-111">The Azure Libraries for Java, which you can install from the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

## <a name="how-to-use-media-services-with-java"></a><span data-ttu-id="7af52-112">Útmutató: A Media Services használata Javával</span><span class="sxs-lookup"><span data-stu-id="7af52-112">How to: Use Media Services with Java</span></span>

>[!NOTE]
><span data-ttu-id="7af52-113">Az AMS-fiók létrehozásakor a rendszer hozzáad egy **alapértelmezett** streamvégpontot a fiókhoz **Leállítva** állapotban.</span><span class="sxs-lookup"><span data-stu-id="7af52-113">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="7af52-114">A tartalom streamelésének megkezdéséhez, valamint a dinamikus csomagolás és a dinamikus titkosítás kihasználásához a tartalomstreameléshez használt streamvégpontnak **Fut** állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7af52-114">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 

>[!NOTE]
><span data-ttu-id="7af52-115">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="7af52-115">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="7af52-116">Ha mindig ugyanazokat a napokat/hozzáférési engedélyeket használja (például olyan keresők szabályzatait, amelyek hosszú ideig érvényben maradnak, vagyis nem feltöltött szabályzatokat), a szabályzatazonosítónak is ugyanannak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7af52-116">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="7af52-117">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="7af52-117">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="7af52-118">A következő kód bemutatja, hogyan hozhat létre egy adategységet, tölthet fel az adategységbe egy médiafájlt, futtathat le egy feladatot az adategység átalakításához, és hozhat létre egy keresőt a videó továbbításához.</span><span class="sxs-lookup"><span data-stu-id="7af52-118">The following code shows how to create an asset, upload a media file to the asset, run a job with a task to transform the asset, and create a locator to stream your video.</span></span>

<span data-ttu-id="7af52-119">A kód használatához először létre kell hoznia egy Media Services-fiókot.</span><span class="sxs-lookup"><span data-stu-id="7af52-119">You need to set up a Media Services account before using this code.</span></span> <span data-ttu-id="7af52-120">A fiók létrehozásával kapcsolatos információk: [Media Services-fiók létrehozása](media-services-portal-create-account.md)</span><span class="sxs-lookup"><span data-stu-id="7af52-120">For information about setting up an account, see [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>

<span data-ttu-id="7af52-121">A „clientId” és „clientSecret” változó helyére helyettesítse be a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="7af52-121">Substitute your values for the 'clientId' and 'clientSecret' variables.</span></span> <span data-ttu-id="7af52-122">A kód egy helyileg tárolt fájlt is használ.</span><span class="sxs-lookup"><span data-stu-id="7af52-122">The code also relies on a locally stored file.</span></span> <span data-ttu-id="7af52-123">Meg kell adnia a saját használandó fájlját.</span><span class="sxs-lookup"><span data-stu-id="7af52-123">You'll need to provide your own file to use.</span></span>

    import java.io.*;
    import java.security.NoSuchAlgorithmException;
    import java.util.EnumSet;

    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.exception.ServiceException;
    import com.microsoft.windowsazure.services.media.MediaConfiguration;
    import com.microsoft.windowsazure.services.media.MediaContract;
    import com.microsoft.windowsazure.services.media.MediaService;
    import com.microsoft.windowsazure.services.media.WritableBlobContainerContract;
    import com.microsoft.windowsazure.services.media.models.AccessPolicy;
    import com.microsoft.windowsazure.services.media.models.AccessPolicyInfo;
    import com.microsoft.windowsazure.services.media.models.AccessPolicyPermission;
    import com.microsoft.windowsazure.services.media.models.Asset;
    import com.microsoft.windowsazure.services.media.models.AssetFile;
    import com.microsoft.windowsazure.services.media.models.AssetFileInfo;
    import com.microsoft.windowsazure.services.media.models.AssetInfo;
    import com.microsoft.windowsazure.services.media.models.Job;
    import com.microsoft.windowsazure.services.media.models.JobInfo;
    import com.microsoft.windowsazure.services.media.models.JobState;
    import com.microsoft.windowsazure.services.media.models.ListResult;
    import com.microsoft.windowsazure.services.media.models.Locator;
    import com.microsoft.windowsazure.services.media.models.LocatorInfo;
    import com.microsoft.windowsazure.services.media.models.LocatorType;
    import com.microsoft.windowsazure.services.media.models.MediaProcessor;
    import com.microsoft.windowsazure.services.media.models.MediaProcessorInfo;
    import com.microsoft.windowsazure.services.media.models.Task;

    public class HelloMediaServices
    {
        // Media Services account credentials configuration
        private static String mediaServiceUri = "https://media.windows.net/API/";
        private static String oAuthUri = "https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13";
        private static String clientId = "account name";
        private static String clientSecret = "account key";
        private static String scope = "urn:WindowsAzureMediaServices";
        private static MediaContract mediaService;

        // Encoder configuration
        private static String preferedEncoder = "Media Encoder Standard";
        private static String encodingPreset = "Adaptive Streaming";

        public static void main(String[] args)
        {

            try {
                // Set up the MediaContract object to call into the Media Services account
                Configuration configuration = MediaConfiguration.configureWithOAuthAuthentication(
                mediaServiceUri, oAuthUri, clientId, clientSecret, scope);
                mediaService = MediaService.create(configuration);


                // Upload a local file to an Asset
                AssetInfo uploadAsset = uploadFileAndCreateAsset("BigBuckBunny.mp4");
                System.out.println("Uploaded Asset Id: " + uploadAsset.getId());


                // Transform the Asset
                AssetInfo encodedAsset = encode(uploadAsset);
                System.out.println("Encoded Asset Id: " + encodedAsset.getId());

                // Create the Streaming Origin Locator
                String url = getStreamingOriginLocator(encodedAsset);

                System.out.println("Origin Locator URL: " + url);
                System.out.println("Sample completed!");

            } catch (ServiceException se) {
                System.out.println("ServiceException encountered.");
                System.out.println(se.toString());
            } catch (Exception e) {
                System.out.println("Exception encountered.");
                System.out.println(e.toString());
            }

        }

        private static AssetInfo uploadFileAndCreateAsset(String fileName)
            throws ServiceException, FileNotFoundException, NoSuchAlgorithmException {

            WritableBlobContainerContract uploader;
            AssetInfo resultAsset;
            AccessPolicyInfo uploadAccessPolicy;
            LocatorInfo uploadLocator = null;

            // Create an Asset
            resultAsset = mediaService.create(Asset.create().setName(fileName).setAlternateId("altId"));
            System.out.println("Created Asset " + fileName);

            // Create an AccessPolicy that provides Write access for 15 minutes
            uploadAccessPolicy = mediaService
                .create(AccessPolicy.create("uploadAccessPolicy", 15.0, EnumSet.of(AccessPolicyPermission.WRITE)));

            // Create a Locator using the AccessPolicy and Asset
            uploadLocator = mediaService
                .create(Locator.create(uploadAccessPolicy.getId(), resultAsset.getId(), LocatorType.SAS));

            // Create the Blob Writer using the Locator
            uploader = mediaService.createBlobWriter(uploadLocator);

            File file = new File("BigBuckBunny.mp4"); 

            // The local file that will be uploaded to your Media Services account
            InputStream input = new FileInputStream(file);

            System.out.println("Uploading " + fileName);

            // Upload the local file to the asset
            uploader.createBlockBlob(fileName, input);

            // Inform Media Services about the uploaded files
            mediaService.action(AssetFile.createFileInfos(resultAsset.getId()));
            System.out.println("Uploaded Asset File " + fileName);

            mediaService.delete(Locator.delete(uploadLocator.getId()));
            mediaService.delete(AccessPolicy.delete(uploadAccessPolicy.getId()));

            return resultAsset;
        }

        // Create a Job that contains a Task to transform the Asset
        private static AssetInfo encode(AssetInfo assetToEncode)
            throws ServiceException, InterruptedException {

            // Retrieve the list of Media Processors that match the name
            ListResult<MediaProcessorInfo> mediaProcessors = mediaService
                            .list(MediaProcessor.list().set("$filter", String.format("Name eq '%s'", preferedEncoder)));

            // Use the latest version of the Media Processor
            MediaProcessorInfo mediaProcessor = null;
            for (MediaProcessorInfo info : mediaProcessors) {
                if (null == mediaProcessor || info.getVersion().compareTo(mediaProcessor.getVersion()) > 0) {
                    mediaProcessor = info;
                }
            }

            System.out.println("Using Media Processor: " + mediaProcessor.getName() + " " + mediaProcessor.getVersion());

            // Create a task with the specified Media Processor
            String outputAssetName = String.format("%s as %s", assetToEncode.getName(), encodingPreset);
            String taskXml = "<taskBody><inputAsset>JobInputAsset(0)</inputAsset>"
                    + "<outputAsset assetCreationOptions=\"0\"" // AssetCreationOptions.None
                    + " assetName=\"" + outputAssetName + "\">JobOutputAsset(0)</outputAsset></taskBody>";

            Task.CreateBatchOperation task = Task.create(mediaProcessor.getId(), taskXml)
                    .setConfiguration(encodingPreset).setName("Encoding");

            // Create the Job; this automatically schedules and runs it.
            Job.Creator jobCreator = Job.create()
                    .setName(String.format("Encoding %s to %s", assetToEncode.getName(), encodingPreset))
                    .addInputMediaAsset(assetToEncode.getId()).setPriority(2).addTaskCreator(task);
            JobInfo job = mediaService.create(jobCreator);

            String jobId = job.getId();
            System.out.println("Created Job with Id: " + jobId);

            // Check to see if the Job has completed
            checkJobStatus(jobId);
            // Done with the Job

            // Retrieve the output Asset
            ListResult<AssetInfo> outputAssets = mediaService.list(Asset.list(job.getOutputAssetsLink()));
            return outputAssets.get(0);
        }


        public static String getStreamingOriginLocator(AssetInfo asset) throws ServiceException {
            // Get the .ISM AssetFile
            ListResult<AssetFileInfo> assetFiles = mediaService.list(AssetFile.list(asset.getAssetFilesLink()));
            AssetFileInfo streamingAssetFile = null;
            for (AssetFileInfo file : assetFiles) {
                if (file.getName().toLowerCase().endsWith(".ism")) {
                    streamingAssetFile = file;
                    break;
                }
            }

            AccessPolicyInfo originAccessPolicy;
            LocatorInfo originLocator = null;

            // Create a 30-day readonly AccessPolicy
            double durationInMinutes = 60 * 24 * 30;
            originAccessPolicy = mediaService.create(
                    AccessPolicy.create("Streaming policy", durationInMinutes, EnumSet.of(AccessPolicyPermission.READ)));

            // Create a Locator using the AccessPolicy and Asset
            originLocator = mediaService
                    .create(Locator.create(originAccessPolicy.getId(), asset.getId(), LocatorType.OnDemandOrigin));

            // Create a Smooth Streaming base URL
            return originLocator.getPath() + streamingAssetFile.getName() + "/manifest";
        }

        private static void checkJobStatus(String jobId) throws InterruptedException, ServiceException {
            boolean done = false;
            JobState jobState = null;
            while (!done) {
                // Sleep for 5 seconds
                Thread.sleep(5000);

                // Query the updated Job state
                jobState = mediaService.get(Job.get(jobId)).getState();
                System.out.println("Job state: " + jobState);

                if (jobState == JobState.Finished || jobState == JobState.Canceled || jobState == JobState.Error) {
                    done = true;
                }
            }
        }

    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="7af52-124">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="7af52-124">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7af52-125">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="7af52-125">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="additional-resources"></a><span data-ttu-id="7af52-126">További források</span><span class="sxs-lookup"><span data-stu-id="7af52-126">Additional Resources</span></span>
<span data-ttu-id="7af52-127">A Media Services Javadoc-dokumentációja: [Java-dokumentáció az Azure-könyvtárakban][Azure Libraries for Java documentation].</span><span class="sxs-lookup"><span data-stu-id="7af52-127">For Media Services Javadoc documentation, see [Azure Libraries for Java documentation][Azure Libraries for Java documentation].</span></span>

<!-- URLs. -->

[Azure Java Developer Center]: http://azure.microsoft.com/develop/java/
[Azure Libraries for Java documentation]: http://dl.windowsazure.com/javadoc/
[Media Services Client Development]: http://msdn.microsoft.com/library/windowsazure/dn223283.aspx
