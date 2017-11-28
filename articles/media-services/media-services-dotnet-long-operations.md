---
title: "a hosszú ideig futó műveletek aaaPolling |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toopoll hosszú futású műveleteket."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 9a68c4b1-6159-42fe-9439-a3661a90ae03
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: f8315a5ddbe484d794c3e2164e47dd9e70521671
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="delivering-live-streaming-with-azure-media-services"></a><span data-ttu-id="77396-103">Az Azure Media Services élő adatfolyam továbbítása</span><span class="sxs-lookup"><span data-stu-id="77396-103">Delivering Live Streaming with Azure Media Services</span></span>

## <a name="overview"></a><span data-ttu-id="77396-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="77396-104">Overview</span></span>

<span data-ttu-id="77396-105">A Microsoft Azure Media Services kínál API-k által küldött kérelmek tooMedia szolgáltatások toostart műveletek (például: hozzon létre, elindítása, leállítása vagy csatorna törlése).</span><span class="sxs-lookup"><span data-stu-id="77396-105">Microsoft Azure Media Services offers APIs that send requests tooMedia Services toostart operations (for example: create, start, stop, or delete a channel).</span></span> <span data-ttu-id="77396-106">Ezek a műveletek olyan hosszan futó.</span><span class="sxs-lookup"><span data-stu-id="77396-106">These operations are long-running.</span></span>

<span data-ttu-id="77396-107">hello Media Services .NET SDK API-t hello kérést küldeni, majd várja meg a hello művelet toocomplete (belső, API-k vannak lekérdezési művelet folyamatban van a esetében bizonyos időközönként hello) biztosít.</span><span class="sxs-lookup"><span data-stu-id="77396-107">hello Media Services .NET SDK provides APIs that send hello request and wait for hello operation toocomplete (internally, hello APIs are polling for operation progress at some intervals).</span></span> <span data-ttu-id="77396-108">Például, ha meghívja a csatorna. Start(), hello metódus hello csatorna végrehajtásának megkezdése után adja vissza.</span><span class="sxs-lookup"><span data-stu-id="77396-108">For example, when you call channel.Start(), hello method returns after hello channel is started.</span></span> <span data-ttu-id="77396-109">Hello aszinkron verzióját is használhatja: await csatorna. StartAsync() (feladatalapú aszinkron mintát kapcsolatos információkért lásd: [KOPPINTSON](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)).</span><span class="sxs-lookup"><span data-stu-id="77396-109">You can also use hello asynchronous version: await channel.StartAsync() (for information about Task-based Asynchronous Pattern, see [TAP](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)).</span></span> <span data-ttu-id="77396-110">API-k művelet kérelmet küldött, és majd kérdezze le az hello állapot hello művelet befejezéséig "lekérdezési módszerek" nevezzük.</span><span class="sxs-lookup"><span data-stu-id="77396-110">APIs that send an operation request and then poll for hello status until hello operation is complete are called “polling methods”.</span></span> <span data-ttu-id="77396-111">Ezek a módszerek (különösen a hello aszinkron verzió) gazdagügyfél-alkalmazásokkal és/vagy állapotalapú szolgáltatások használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="77396-111">These methods (especially hello Async version) are recommended for rich client applications and/or stateful services.</span></span>

<span data-ttu-id="77396-112">Vannak esetek, amikor egy alkalmazás egy hosszú ideig futó http-kérelem nem várja meg, így manuálisan szeretne toopoll hello művelet folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="77396-112">There are scenarios where an application cannot wait for a long running http request and wants toopoll for hello operation progress manually.</span></span> <span data-ttu-id="77396-113">Egy jellemző példa erre egy böngészőt, egy állapot nélküli webszolgáltatás való interakció: hello böngésző kér toocreate egy csatornát, hello webszolgáltatás indít el egy hosszú ideig futó művelet, és vissza hello művelet azonosítója toohello böngésző.</span><span class="sxs-lookup"><span data-stu-id="77396-113">A typical example would be a browser interacting with a stateless web service: when hello browser requests toocreate a channel, hello web service initiates a long running operation and returns hello operation ID toohello browser.</span></span> <span data-ttu-id="77396-114">hello böngésző sikerült majd kérje meg a hello web service tooget hello műveleti állapotának az hello azonosítója alapján.</span><span class="sxs-lookup"><span data-stu-id="77396-114">hello browser could then ask hello web service tooget hello operation status based on hello ID.</span></span> <span data-ttu-id="77396-115">hello Media Services .NET SDK API-k, melyek hasznosak lehetnek a forgatókönyv biztosítja.</span><span class="sxs-lookup"><span data-stu-id="77396-115">hello Media Services .NET SDK provides APIs that are useful for this scenario.</span></span> <span data-ttu-id="77396-116">Ezen API-k "nem lekérdezési módszerek" nevezzük.</span><span class="sxs-lookup"><span data-stu-id="77396-116">These APIs are called “non-polling methods”.</span></span>
<span data-ttu-id="77396-117">hello "nem lekérdezési módszerek" rendelkezik elnevezési minta a következő hello: küldése*OperationName*műveletet (például SendCreateOperation).</span><span class="sxs-lookup"><span data-stu-id="77396-117">hello “non-polling methods” have hello following naming pattern: Send*OperationName*Operation (for example, SendCreateOperation).</span></span> <span data-ttu-id="77396-118">Küldési*OperationName*művelet módszerek vissza hello **IOperation** hello visszaadott objektum információkat tartalmaz, amelyek lehetnek használt tootrack hello művelet; objektum.</span><span class="sxs-lookup"><span data-stu-id="77396-118">Send*OperationName*Operation methods return hello **IOperation** object; hello returned object contains information that can be used tootrack hello operation.</span></span> <span data-ttu-id="77396-119">hello küldési*OperationName*OperationAsync módszerek **feladat<IOperation>**.</span><span class="sxs-lookup"><span data-stu-id="77396-119">hello Send*OperationName*OperationAsync methods return **Task<IOperation>**.</span></span>

<span data-ttu-id="77396-120">Jelenleg a következő osztályok támogatás nem lekérdezési módszerek hello: **csatorna**, **StreamingEndpoint**, és **Program**.</span><span class="sxs-lookup"><span data-stu-id="77396-120">Currently, hello following classes support non-polling methods:  **Channel**, **StreamingEndpoint**, and **Program**.</span></span>

<span data-ttu-id="77396-121">a hello művelet állapotot használata hello toopoll **GetOperation** hello metódusa **OperationBaseCollection** osztály.</span><span class="sxs-lookup"><span data-stu-id="77396-121">toopoll for hello operation status, use hello **GetOperation** method on hello **OperationBaseCollection** class.</span></span> <span data-ttu-id="77396-122">A következő időközönként toocheck hello műveleti állapotának hello használata: a **csatorna** és **StreamingEndpoint** műveletei, pedig 30 másodperc; **Program** műveletei, pedig 10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="77396-122">Use hello following intervals toocheck hello operation status: for **Channel** and **StreamingEndpoint** operations, use 30 seconds; for **Program** operations, use 10 seconds.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="77396-123">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="77396-123">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="77396-124">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="77396-124">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span>

## <a name="example"></a><span data-ttu-id="77396-125">Példa</span><span class="sxs-lookup"><span data-stu-id="77396-125">Example</span></span>

<span data-ttu-id="77396-126">hello alábbi példa meghatározza nevű osztály **ChannelOperations**.</span><span class="sxs-lookup"><span data-stu-id="77396-126">hello following example defines a class called **ChannelOperations**.</span></span> <span data-ttu-id="77396-127">Ez az osztály definícióját a webszolgáltatás-osztály definíciójának kiindulási pontot lehet.</span><span class="sxs-lookup"><span data-stu-id="77396-127">This class definition could be a starting point for your web service class definition.</span></span> <span data-ttu-id="77396-128">Az egyszerűség kedvéért hello alábbi példák használata hello nem aszinkron metódusok.</span><span class="sxs-lookup"><span data-stu-id="77396-128">For simplicity, hello following examples use hello non-async versions of methods.</span></span>

<span data-ttu-id="77396-129">hello példa azt is bemutatja, hogyan hello ügyfél használhatja-e ez az osztály.</span><span class="sxs-lookup"><span data-stu-id="77396-129">hello example also shows how hello client might use this class.</span></span>

### <a name="channeloperations-class-definition"></a><span data-ttu-id="77396-130">ChannelOperations osztálydefiníció</span><span class="sxs-lookup"><span data-stu-id="77396-130">ChannelOperations class definition</span></span>

    using Microsoft.WindowsAzure.MediaServices.Client;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Net;

    /// <summary> 
    /// hello ChannelOperations class only implements 
    /// hello Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        public ChannelOperations()
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
        }

        /// <summary>  
        /// Initiates hello creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name toobe given toohello new channel</param>  
        /// <returns>  
        /// Operation Id for hello long running operation being executed by Media Services. 
        /// Use this operation Id toopoll for hello channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });

            return operation.Id;
        }

        /// <summary> 
        /// Checks if hello operation has been completed. 
        /// If hello operation succeeded, hello created channel Id is returned in hello out parameter.
        /// </summary> 
        /// <param name="operationId">hello operation Id.</param> 
        /// <param name="channel">
        /// If hello operation succeeded, 
        /// hello created channel Id is returned in hello out parameter.</param>
        /// <returns>Returns false if hello operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;

            channelId = null;

            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle hello failure. 
                    // For example, throw an exception. 
                    // Use hello following information in hello exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }

        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                }
            };
        }

        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                }
            };
        }

        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

### <a name="hello-client-code"></a><span data-ttu-id="77396-131">hello Ügyfélkód</span><span class="sxs-lookup"><span data-stu-id="77396-131">hello client code</span></span>
    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");

    string channelId = null;
    bool isCompleted = false;

    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }

    // If we got here, we should have hello newly created channel id.
    Console.WriteLine(channelId);



## <a name="media-services-learning-paths"></a><span data-ttu-id="77396-132">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="77396-132">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="77396-133">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="77396-133">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

