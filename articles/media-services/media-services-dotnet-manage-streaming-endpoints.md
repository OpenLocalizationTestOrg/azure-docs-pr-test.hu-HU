---
title: "Adatfolyam-végpontokat kezelheti a .NET SDK-val. | Microsoft Docs"
description: "Ez a témakör azt ismerteti, hogyan adatfolyam-végpontokat kezelheti a az Azure-portálon."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: 0da34a97-f36c-48d0-8ea2-ec12584a2215
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 2f4f464f8604b6f453d6b50b736c6a3a889a3408
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-streaming-endpoints-with-net-sdk"></a><span data-ttu-id="06ba0-104">.NET SDK-val adatfolyam-továbbítási végpontok kezelése</span><span class="sxs-lookup"><span data-stu-id="06ba0-104">Manage streaming endpoints with .NET SDK</span></span>

>[!NOTE]
><span data-ttu-id="06ba0-105">Mindenképpen tekintse át a [áttekintése](media-services-streaming-endpoints-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="06ba0-105">Make sure to review the [overview](media-services-streaming-endpoints-overview.md) topic.</span></span> <span data-ttu-id="06ba0-106">Ezenkívül tekintse át a [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span><span class="sxs-lookup"><span data-stu-id="06ba0-106">Also, review [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span></span>

<span data-ttu-id="06ba0-107">Ebben a témakörben a kód bemutatja, hogyan hajtsa végre a következő feladatokat az Azure Media Services .NET SDK használatával:</span><span class="sxs-lookup"><span data-stu-id="06ba0-107">The code in this topic shows how to do the following tasks using the Azure Media Services .NET SDK:</span></span>

- <span data-ttu-id="06ba0-108">Vizsgálja meg az alapértelmezett streamvégpontból.</span><span class="sxs-lookup"><span data-stu-id="06ba0-108">Examine the default streaming endpoint.</span></span>
- <span data-ttu-id="06ba0-109">Új streamvégpont létrehozása/felvétele.</span><span class="sxs-lookup"><span data-stu-id="06ba0-109">Create/add new streaming endpoint.</span></span>

    <span data-ttu-id="06ba0-110">Érdemes több adatfolyam-továbbítási végpontok is, ha azt tervezi, hogy a különböző tartalomtovábbító vagy a CDN és a közvetlen hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="06ba0-110">You might want to have multiple streaming endpoints if you plan to have different CDNs or a CDN and direct access.</span></span>

    > [!NOTE]
    > <span data-ttu-id="06ba0-111">Csak számlázása, amikor a Streamvégpontot futó állapotban van.</span><span class="sxs-lookup"><span data-stu-id="06ba0-111">You are only billed when your Streaming Endpoint is in running state.</span></span>
    
- <span data-ttu-id="06ba0-112">Az adatfolyam-továbbítási végpontjának frissítése.</span><span class="sxs-lookup"><span data-stu-id="06ba0-112">Update the streaming endpoint.</span></span>
    
    <span data-ttu-id="06ba0-113">Ellenőrizze, hogy Update() függvény.</span><span class="sxs-lookup"><span data-stu-id="06ba0-113">Make sure to call the Update() function.</span></span>

- <span data-ttu-id="06ba0-114">Az adatfolyam-továbbítási végpontjának törlése.</span><span class="sxs-lookup"><span data-stu-id="06ba0-114">Delete the streaming endpoint.</span></span>

    >[!NOTE]
    ><span data-ttu-id="06ba0-115">Az alapértelmezett streamvégpontból nem lehet törölni.</span><span class="sxs-lookup"><span data-stu-id="06ba0-115">The default streaming endpoint cannot be deleted.</span></span>

<span data-ttu-id="06ba0-116">A streamvégpont méretezése kapcsolatos információkért lásd: [ez](media-services-portal-scale-streaming-endpoints.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="06ba0-116">For information about how to scale the streaming endpoint, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="06ba0-117">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="06ba0-117">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="06ba0-118">Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="06ba0-118">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="add-code-that-manages-streaming-endpoints"></a><span data-ttu-id="06ba0-119">Adja hozzá a kódot, amely adatfolyam-továbbítási végpontok kezelése</span><span class="sxs-lookup"><span data-stu-id="06ba0-119">Add code that manages streaming endpoints</span></span>
    
<span data-ttu-id="06ba0-120">Cserélje le a Program.cs lévő kódot az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="06ba0-120">Replace the code in the Program.cs with the following code:</span></span>

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.Live;

    namespace AMSStreamingEndpoint
    {
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
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            var defaultStreamingEndpoint = _context.StreamingEndpoints.Where(s => s.Name.Contains("default")).FirstOrDefault();
            ExamineStreamingEndpoint(defaultStreamingEndpoint);

            IStreamingEndpoint newStreamingEndpoint = AddStreamingEndpoint();
            UpdateStreamingEndpoint(newStreamingEndpoint);
            DeleteStreamingEndpoint(newStreamingEndpoint);
        }

        static public void ExamineStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            Console.WriteLine(streamingEndpoint.Name);
            Console.WriteLine(streamingEndpoint.StreamingEndpointVersion);
            Console.WriteLine(streamingEndpoint.FreeTrialEndTime);
            Console.WriteLine(streamingEndpoint.ScaleUnits);
            Console.WriteLine(streamingEndpoint.CdnProvider);
            Console.WriteLine(streamingEndpoint.CdnProfile);
            Console.WriteLine(streamingEndpoint.CdnEnabled);
        }

        static public IStreamingEndpoint AddStreamingEndpoint()
        {
            var name = "StreamingEndpoint" + DateTime.UtcNow.ToString("hhmmss");
            var option = new StreamingEndpointCreationOptions(name, 1)
            {
            StreamingEndpointVersion = new Version("2.0"),
            CdnEnabled = true,
            CdnProfile = "CdnProfile",
            CdnProvider = CdnProviderType.PremiumVerizon
            };

            var streamingEndpoint = _context.StreamingEndpoints.Create(option);

            return streamingEndpoint;
        }

        static public void UpdateStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            if (streamingEndpoint.StreamingEndpointVersion == "1.0")
            streamingEndpoint.StreamingEndpointVersion = "2.0";

            streamingEndpoint.CdnEnabled = false;
            streamingEndpoint.Update();
        }

        static public void DeleteStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            streamingEndpoint.Delete();
        }
        }
    }


## <a name="next-steps"></a><span data-ttu-id="06ba0-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06ba0-121">Next steps</span></span>
<span data-ttu-id="06ba0-122">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="06ba0-122">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="06ba0-123">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="06ba0-123">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

