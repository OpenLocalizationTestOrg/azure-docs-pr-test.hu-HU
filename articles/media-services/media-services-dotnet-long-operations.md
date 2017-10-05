---
title: "A hosszú ideig futó műveletek lekérdezési |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan, és kérdezze le a hosszú futású műveleteket."
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
ms.openlocfilehash: 7123a2d44d3b7c332afe30fb0fcea88ca29e313a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="delivering-live-streaming-with-azure-media-services"></a><span data-ttu-id="43b73-103">Az Azure Media Services élő adatfolyam továbbítása</span><span class="sxs-lookup"><span data-stu-id="43b73-103">Delivering Live Streaming with Azure Media Services</span></span>

## <a name="overview"></a><span data-ttu-id="43b73-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="43b73-104">Overview</span></span>

<span data-ttu-id="43b73-105">A Microsoft Azure Media Services kínál API-kérelmeket küldjön a Media Services operations indításához (például: hozzon létre, elindítása, leállítása vagy csatorna törlése).</span><span class="sxs-lookup"><span data-stu-id="43b73-105">Microsoft Azure Media Services offers APIs that send requests to Media Services to start operations (for example: create, start, stop, or delete a channel).</span></span> <span data-ttu-id="43b73-106">Ezek a műveletek olyan hosszan futó.</span><span class="sxs-lookup"><span data-stu-id="43b73-106">These operations are long-running.</span></span>

<span data-ttu-id="43b73-107">A Media Services .NET SDK API-t elküldeni a kérelmet, és várjon, amíg a művelet elvégzéséhez biztosít (belsőleg, az API-k vannak ciklikus lekérdezés a művelet folyamatban van bizonyos időközönként).</span><span class="sxs-lookup"><span data-stu-id="43b73-107">The Media Services .NET SDK provides APIs that send the request and wait for the operation to complete (internally, the APIs are polling for operation progress at some intervals).</span></span> <span data-ttu-id="43b73-108">Például, ha meghívja a csatorna. Start(), a metódus visszaadja a csatorna indítása után.</span><span class="sxs-lookup"><span data-stu-id="43b73-108">For example, when you call channel.Start(), the method returns after the channel is started.</span></span> <span data-ttu-id="43b73-109">Is használhatja az aszinkron: await csatorna. StartAsync() (feladatalapú aszinkron mintát kapcsolatos információkért lásd: [KOPPINTSON](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)).</span><span class="sxs-lookup"><span data-stu-id="43b73-109">You can also use the asynchronous version: await channel.StartAsync() (for information about Task-based Asynchronous Pattern, see [TAP](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)).</span></span> <span data-ttu-id="43b73-110">API-k művelet kérelmet küldött, és majd kérdezze le az állapot addig, amíg a művelet be nem fejeződött "lekérdezési módszerek" nevezzük.</span><span class="sxs-lookup"><span data-stu-id="43b73-110">APIs that send an operation request and then poll for the status until the operation is complete are called “polling methods”.</span></span> <span data-ttu-id="43b73-111">Ezek a módszerek (különösen a aszinkron verzió) gazdagügyfél-alkalmazásokkal és/vagy állapotalapú szolgáltatások használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="43b73-111">These methods (especially the Async version) are recommended for rich client applications and/or stateful services.</span></span>

<span data-ttu-id="43b73-112">Vannak esetek, amikor egy alkalmazás nem várja meg a hosszú ideig futó HTTP-kérelmek, így szeretné manuálisan kérdezze le az a művelet folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="43b73-112">There are scenarios where an application cannot wait for a long running http request and wants to poll for the operation progress manually.</span></span> <span data-ttu-id="43b73-113">Egy jellemző példa erre egy böngészőt, egy állapot nélküli webszolgáltatás való interakció: amikor a böngésző csatornát létrehozni, a webszolgáltatás indít el egy hosszú ideig futó művelet, és a böngésző a művelet-Azonosítót adja vissza.</span><span class="sxs-lookup"><span data-stu-id="43b73-113">A typical example would be a browser interacting with a stateless web service: when the browser requests to create a channel, the web service initiates a long running operation and returns the operation ID to the browser.</span></span> <span data-ttu-id="43b73-114">A böngésző sikerült majd kérje meg a webszolgáltatás számára, hogy az az azonosítója alapján műveleti állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="43b73-114">The browser could then ask the web service to get the operation status based on the ID.</span></span> <span data-ttu-id="43b73-115">A Media Services .NET SDK API-k, melyek hasznosak lehetnek a forgatókönyv biztosítja.</span><span class="sxs-lookup"><span data-stu-id="43b73-115">The Media Services .NET SDK provides APIs that are useful for this scenario.</span></span> <span data-ttu-id="43b73-116">Ezen API-k "nem lekérdezési módszerek" nevezzük.</span><span class="sxs-lookup"><span data-stu-id="43b73-116">These APIs are called “non-polling methods”.</span></span>
<span data-ttu-id="43b73-117">A "nem lekérdezési módszerek" rendelkezik a következő elnevezési: küldése*OperationName*műveletet (például SendCreateOperation).</span><span class="sxs-lookup"><span data-stu-id="43b73-117">The “non-polling methods” have the following naming pattern: Send*OperationName*Operation (for example, SendCreateOperation).</span></span> <span data-ttu-id="43b73-118">Küldési*OperationName*művelet módszerek a **IOperation** objektum; a visszaadott objektumot információkat tartalmaznak, amelyek segítségével nyomon követheti a művelet.</span><span class="sxs-lookup"><span data-stu-id="43b73-118">Send*OperationName*Operation methods return the **IOperation** object; the returned object contains information that can be used to track the operation.</span></span> <span data-ttu-id="43b73-119">A Küldés*OperationName*OperationAsync módszerek **feladat<IOperation>**.</span><span class="sxs-lookup"><span data-stu-id="43b73-119">The Send*OperationName*OperationAsync methods return **Task<IOperation>**.</span></span>

<span data-ttu-id="43b73-120">Jelenleg a következő osztályok támogatja-e a nem lekérdezési módszerek: **csatorna**, **StreamingEndpoint**, és **Program**.</span><span class="sxs-lookup"><span data-stu-id="43b73-120">Currently, the following classes support non-polling methods:  **Channel**, **StreamingEndpoint**, and **Program**.</span></span>

<span data-ttu-id="43b73-121">Kérdezze le a műveletállapot, használja a **GetOperation** metódust a **OperationBaseCollection** osztály.</span><span class="sxs-lookup"><span data-stu-id="43b73-121">To poll for the operation status, use the **GetOperation** method on the **OperationBaseCollection** class.</span></span> <span data-ttu-id="43b73-122">A művelet állapotának ellenőrzéséhez a következő időszakok használja: a **csatorna** és **StreamingEndpoint** műveletei, pedig 30 másodperc; **Program** műveletei, pedig 10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="43b73-122">Use the following intervals to check the operation status: for **Channel** and **StreamingEndpoint** operations, use 30 seconds; for **Program** operations, use 10 seconds.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="43b73-123">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43b73-123">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="43b73-124">Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="43b73-124">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span>

## <a name="example"></a><span data-ttu-id="43b73-125">Példa</span><span class="sxs-lookup"><span data-stu-id="43b73-125">Example</span></span>

<span data-ttu-id="43b73-126">Az alábbi példa meghatározza nevű osztály **ChannelOperations**.</span><span class="sxs-lookup"><span data-stu-id="43b73-126">The following example defines a class called **ChannelOperations**.</span></span> <span data-ttu-id="43b73-127">Ez az osztály definícióját a webszolgáltatás-osztály definíciójának kiindulási pontot lehet.</span><span class="sxs-lookup"><span data-stu-id="43b73-127">This class definition could be a starting point for your web service class definition.</span></span> <span data-ttu-id="43b73-128">Az egyszerűség kedvéért az alábbi példák a módszerek nem aszinkron verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="43b73-128">For simplicity, the following examples use the non-async versions of methods.</span></span>

<span data-ttu-id="43b73-129">A példa azt is bemutatja, hogyan használhatja az ügyfél a Ez az osztály.</span><span class="sxs-lookup"><span data-stu-id="43b73-129">The example also shows how the client might use this class.</span></span>

### <a name="channeloperations-class-definition"></a><span data-ttu-id="43b73-130">ChannelOperations osztálydefiníció</span><span class="sxs-lookup"><span data-stu-id="43b73-130">ChannelOperations class definition</span></span>

    using Microsoft.WindowsAzure.MediaServices.Client;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Net;

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
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
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
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
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;

            channelId = null;

            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
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

### <a name="the-client-code"></a><span data-ttu-id="43b73-131">Az Ügyfélkód</span><span class="sxs-lookup"><span data-stu-id="43b73-131">The client code</span></span>
    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");

    string channelId = null;
    bool isCompleted = false;

    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }

    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);



## <a name="media-services-learning-paths"></a><span data-ttu-id="43b73-132">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="43b73-132">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="43b73-133">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="43b73-133">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

