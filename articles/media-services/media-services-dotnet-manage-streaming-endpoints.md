---
title: "adatfolyam-végpontok .NET SDK-val aaaManage. | Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toomanage streamvégpontok a hello Azure-portálon."
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
ms.openlocfilehash: 30c092a8ebf4e2b2902392f4cf98f46d812ccdbc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-streaming-endpoints-with-net-sdk"></a><span data-ttu-id="6190a-104">.NET SDK-val adatfolyam-továbbítási végpontok kezelése</span><span class="sxs-lookup"><span data-stu-id="6190a-104">Manage streaming endpoints with .NET SDK</span></span>

>[!NOTE]
><span data-ttu-id="6190a-105">Győződjön meg arról, hogy tooreview hello [áttekintése](media-services-streaming-endpoints-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="6190a-105">Make sure tooreview hello [overview](media-services-streaming-endpoints-overview.md) topic.</span></span> <span data-ttu-id="6190a-106">Ezenkívül tekintse át a [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span><span class="sxs-lookup"><span data-stu-id="6190a-106">Also, review [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span></span>

<span data-ttu-id="6190a-107">Ebben a témakörben hello kód bemutatja, hogyan toodo hello a következő feladatok segítségével hello Azure Media Services .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="6190a-107">hello code in this topic shows how toodo hello following tasks using hello Azure Media Services .NET SDK:</span></span>

- <span data-ttu-id="6190a-108">Vizsgálja meg a hello alapértelmezett streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="6190a-108">Examine hello default streaming endpoint.</span></span>
- <span data-ttu-id="6190a-109">Új streamvégpont létrehozása/felvétele.</span><span class="sxs-lookup"><span data-stu-id="6190a-109">Create/add new streaming endpoint.</span></span>

    <span data-ttu-id="6190a-110">Érdemes lehet toohave több adatfolyam-továbbítási végpontok Ha azt tervezi, hogy toohave különböző tartalomtovábbító vagy a CDN és a közvetlen hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="6190a-110">You might want toohave multiple streaming endpoints if you plan toohave different CDNs or a CDN and direct access.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6190a-111">Csak számlázása, amikor a Streamvégpontot futó állapotban van.</span><span class="sxs-lookup"><span data-stu-id="6190a-111">You are only billed when your Streaming Endpoint is in running state.</span></span>
    
- <span data-ttu-id="6190a-112">Adatfolyam-továbbítási végpontra hello frissítése.</span><span class="sxs-lookup"><span data-stu-id="6190a-112">Update hello streaming endpoint.</span></span>
    
    <span data-ttu-id="6190a-113">Győződjön meg arról, hogy toocall hello Update() függvény.</span><span class="sxs-lookup"><span data-stu-id="6190a-113">Make sure toocall hello Update() function.</span></span>

- <span data-ttu-id="6190a-114">Adatfolyam-továbbítási végpontra hello törlése.</span><span class="sxs-lookup"><span data-stu-id="6190a-114">Delete hello streaming endpoint.</span></span>

    >[!NOTE]
    ><span data-ttu-id="6190a-115">hello alapértelmezett streamvégpontból nem lehet törölni.</span><span class="sxs-lookup"><span data-stu-id="6190a-115">hello default streaming endpoint cannot be deleted.</span></span>

<span data-ttu-id="6190a-116">Hogyan tooscale hello streamvégpont kapcsolatos információkért lásd: [ez](media-services-portal-scale-streaming-endpoints.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="6190a-116">For information about how tooscale hello streaming endpoint, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="6190a-117">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6190a-117">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="6190a-118">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="6190a-118">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="add-code-that-manages-streaming-endpoints"></a><span data-ttu-id="6190a-119">Adja hozzá a kódot, amely adatfolyam-továbbítási végpontok kezelése</span><span class="sxs-lookup"><span data-stu-id="6190a-119">Add code that manages streaming endpoints</span></span>
    
<span data-ttu-id="6190a-120">Cserélje le a hello Program.cs hello kód hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="6190a-120">Replace hello code in hello Program.cs with hello following code:</span></span>

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.Live;

    namespace AMSStreamingEndpoint
    {
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


## <a name="next-steps"></a><span data-ttu-id="6190a-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6190a-121">Next steps</span></span>
<span data-ttu-id="6190a-122">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="6190a-122">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6190a-123">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="6190a-123">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

