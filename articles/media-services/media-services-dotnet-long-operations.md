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
# <a name="delivering-live-streaming-with-azure-media-services"></a>Az Azure Media Services élő adatfolyam továbbítása

## <a name="overview"></a>Áttekintés

A Microsoft Azure Media Services kínál API-k által küldött kérelmek tooMedia szolgáltatások toostart műveletek (például: hozzon létre, elindítása, leállítása vagy csatorna törlése). Ezek a műveletek olyan hosszan futó.

hello Media Services .NET SDK API-t hello kérést küldeni, majd várja meg a hello művelet toocomplete (belső, API-k vannak lekérdezési művelet folyamatban van a esetében bizonyos időközönként hello) biztosít. Például, ha meghívja a csatorna. Start(), hello metódus hello csatorna végrehajtásának megkezdése után adja vissza. Hello aszinkron verzióját is használhatja: await csatorna. StartAsync() (feladatalapú aszinkron mintát kapcsolatos információkért lásd: [KOPPINTSON](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)). API-k művelet kérelmet küldött, és majd kérdezze le az hello állapot hello művelet befejezéséig "lekérdezési módszerek" nevezzük. Ezek a módszerek (különösen a hello aszinkron verzió) gazdagügyfél-alkalmazásokkal és/vagy állapotalapú szolgáltatások használata ajánlott.

Vannak esetek, amikor egy alkalmazás egy hosszú ideig futó http-kérelem nem várja meg, így manuálisan szeretne toopoll hello művelet folyamatban van. Egy jellemző példa erre egy böngészőt, egy állapot nélküli webszolgáltatás való interakció: hello böngésző kér toocreate egy csatornát, hello webszolgáltatás indít el egy hosszú ideig futó művelet, és vissza hello művelet azonosítója toohello böngésző. hello böngésző sikerült majd kérje meg a hello web service tooget hello műveleti állapotának az hello azonosítója alapján. hello Media Services .NET SDK API-k, melyek hasznosak lehetnek a forgatókönyv biztosítja. Ezen API-k "nem lekérdezési módszerek" nevezzük.
hello "nem lekérdezési módszerek" rendelkezik elnevezési minta a következő hello: küldése*OperationName*műveletet (például SendCreateOperation). Küldési*OperationName*művelet módszerek vissza hello **IOperation** hello visszaadott objektum információkat tartalmaz, amelyek lehetnek használt tootrack hello művelet; objektum. hello küldési*OperationName*OperationAsync módszerek **feladat<IOperation>**.

Jelenleg a következő osztályok támogatás nem lekérdezési módszerek hello: **csatorna**, **StreamingEndpoint**, és **Program**.

a hello művelet állapotot használata hello toopoll **GetOperation** hello metódusa **OperationBaseCollection** osztály. A következő időközönként toocheck hello műveleti állapotának hello használata: a **csatorna** és **StreamingEndpoint** műveletei, pedig 30 másodperc; **Program** műveletei, pedig 10 másodperc.

## <a name="create-and-configure-a-visual-studio-project"></a>Egy Visual Studio-projekt létrehozása és konfigurálása

A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).

## <a name="example"></a>Példa

hello alábbi példa meghatározza nevű osztály **ChannelOperations**. Ez az osztály definícióját a webszolgáltatás-osztály definíciójának kiindulási pontot lehet. Az egyszerűség kedvéért hello alábbi példák használata hello nem aszinkron metódusok.

hello példa azt is bemutatja, hogyan hello ügyfél használhatja-e ez az osztály.

### <a name="channeloperations-class-definition"></a>ChannelOperations osztálydefiníció

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

### <a name="hello-client-code"></a>hello Ügyfélkód
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



## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

