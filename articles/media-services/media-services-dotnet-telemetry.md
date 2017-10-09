---
title: Azure Media Services .NET telemetriai adatok aaaConfiguring |} Microsoft Docs
description: "Ez a cikk bemutatja, hogyan toouse hello Azure Media Services telemetriai .NET SDK használatával."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f8f55e37-0714-49ea-bf4a-e6c1319bec44
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 4019fa7d080ca3f8a8709bd1e666f7062b883954
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-azure-media-services-telemetry-with-net"></a><span data-ttu-id="2bf74-103">A .NET Azure Media Services telemetriai konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2bf74-103">Configuring Azure Media Services telemetry with .NET</span></span>

<span data-ttu-id="2bf74-104">Ez a témakör általános lépéseket, amelyek esetleg hello Azure Media Services (AMS) telemetriai .NET SDK használatával konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="2bf74-104">This topic describes general steps that you might take when configuring hello Azure Media Services (AMS) telemetry using .NET SDK.</span></span> 

>[!NOTE]
><span data-ttu-id="2bf74-105">A hello részletesen ismerteti, mit AMS telemetriai adatokat, és hogyan tooconsume, lásd: hello [áttekintése](media-services-telemetry-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="2bf74-105">For hello detailed explanation of what is AMS telemetry and how tooconsume it, see hello [overview](media-services-telemetry-overview.md) topic.</span></span>

<span data-ttu-id="2bf74-106">Használatba vehetné a telemetriai adatokat a következő módokon hello egyikében:</span><span class="sxs-lookup"><span data-stu-id="2bf74-106">You can consume telemetry data in one of hello following ways:</span></span>

- <span data-ttu-id="2bf74-107">Olvassa el az adatokat közvetlenül az Azure Table Storage (pl. a hello Storage szolgáltatás SDK használatával).</span><span class="sxs-lookup"><span data-stu-id="2bf74-107">Read data directly from Azure Table Storage (e.g. using hello Storage SDK).</span></span> <span data-ttu-id="2bf74-108">Telemetriai adatok tárolási táblák hello ismertetését lásd: hello **telemetriai adatokat fel** a [ez](https://msdn.microsoft.com/library/mt742089.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="2bf74-108">For hello description of telemetry storage tables, see hello **Consuming telemetry information** in [this](https://msdn.microsoft.com/library/mt742089.aspx) topic.</span></span>

<span data-ttu-id="2bf74-109">Vagy</span><span class="sxs-lookup"><span data-stu-id="2bf74-109">Or</span></span>

- <span data-ttu-id="2bf74-110">Hello-támogatás a Media Services .NET SDK hello tárolási adatok olvasásához.</span><span class="sxs-lookup"><span data-stu-id="2bf74-110">Use hello support in hello Media Services .NET SDK for reading storage data.</span></span> <span data-ttu-id="2bf74-111">Ez a témakör bemutatja, hogyan tooenable telemetriai adat hello megadott AMS-fiók, és hogyan tooquery hello metrikák használatával hello Azure Media Services .NET SDK-t.</span><span class="sxs-lookup"><span data-stu-id="2bf74-111">This topic shows how tooenable telemetry for hello specified AMS account and how tooquery hello metrics using hello Azure Media Services .NET SDK.</span></span>  

## <a name="configuring-telemetry-for-a-media-services-account"></a><span data-ttu-id="2bf74-112">Telemetria a Media Services-fiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2bf74-112">Configuring telemetry for a Media Services account</span></span>

<span data-ttu-id="2bf74-113">hello következő lépésekre szükséges tooenable telemetriai:</span><span class="sxs-lookup"><span data-stu-id="2bf74-113">hello following steps are needed tooenable telemetry:</span></span>

- <span data-ttu-id="2bf74-114">Hello hello tárolási kapcsolódó számla toohello Media Services-fiók hitelesítő adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2bf74-114">Get hello credentials of hello storage account attached toohello Media Services account.</span></span> 
- <span data-ttu-id="2bf74-115">Az értesítési végpont létrehozásához **EndPointType** túl beállítása**AzureTable** és endPointAddress toohello tárolási tábla mutat.</span><span class="sxs-lookup"><span data-stu-id="2bf74-115">Create a Notification Endpoint with **EndPointType** set too**AzureTable** and endPointAddress pointing toohello storage table.</span></span>

        INotificationEndPoint notificationEndPoint = 
                      _context.NotificationEndPoints.Create("monitoring", 
                      NotificationEndPointType.AzureTable,
                      "https://" + _mediaServicesStorageAccountName + ".table.core.windows.net/");

- <span data-ttu-id="2bf74-116">Figyelési konfiguráció létrehozása beállítások hello szolgáltatási azt szeretné, hogy toomonitor.</span><span class="sxs-lookup"><span data-stu-id="2bf74-116">Create a monitoring configuration settings for hello services you want toomonitor.</span></span> <span data-ttu-id="2bf74-117">Legfeljebb egy engedélyezett figyelési konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="2bf74-117">No more than one monitoring configuration settings is allowed.</span></span> 
  
        IMonitoringConfiguration monitoringConfiguration = _context.MonitoringConfigurations.Create(notificationEndPoint.Id,
            new List<ComponentMonitoringSetting>()
            {
                new ComponentMonitoringSetting(MonitoringComponent.Channel, MonitoringLevel.Normal),
                new ComponentMonitoringSetting(MonitoringComponent.StreamingEndpoint, MonitoringLevel.Normal)
            });

## <a name="consuming-telemetry-information"></a><span data-ttu-id="2bf74-118">Telemetria információk felhasználása</span><span class="sxs-lookup"><span data-stu-id="2bf74-118">Consuming telemetry information</span></span>

<span data-ttu-id="2bf74-119">További információ a fogyasztó telemetriai adatokat: [ez](media-services-telemetry-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="2bf74-119">For information about consuming telemetry information, see [this](media-services-telemetry-overview.md) topic.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="2bf74-120">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2bf74-120">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="2bf74-121">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="2bf74-121">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

2. <span data-ttu-id="2bf74-122">Adja hozzá a következő elem túl hello**appSettings** az app.config fájlban meghatározott:</span><span class="sxs-lookup"><span data-stu-id="2bf74-122">Add hello following element too**appSettings** defined in your app.config file:</span></span>

    <add key="StorageAccountName" value="storage_name" />
 
## <a name="example"></a><span data-ttu-id="2bf74-123">Példa</span><span class="sxs-lookup"><span data-stu-id="2bf74-123">Example</span></span>  
    
<span data-ttu-id="2bf74-124">hello a következő példa bemutatja, hogyan tooenable telemetriai adat hello megadott AMS-fiók, és hogyan tooquery hello metrikák használatával hello Azure Media Services .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="2bf74-124">hello following example shows how tooenable telemetry for hello specified AMS account and how tooquery hello metrics using hello Azure Media Services .NET SDK.</span></span>  

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;

    namespace AMSMetrics
    {
        class Program
        {
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static readonly string _mediaServicesStorageAccountName =
            ConfigurationManager.AppSettings["StorageAccountName"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static IStreamingEndpoint _streamingEndpoint = null;
        private static IChannel _channel = null;

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            _streamingEndpoint = _context.StreamingEndpoints.FirstOrDefault();
            _channel = _context.Channels.FirstOrDefault();

            var monitoringConfigurations = _context.MonitoringConfigurations;
            IMonitoringConfiguration monitoringConfiguration = null;

            // No more than one monitoring configuration settings is allowed.
            if (monitoringConfigurations.ToArray().Length != 0)
            {
            monitoringConfiguration = _context.MonitoringConfigurations.FirstOrDefault();
            }
            else
            {
            INotificationEndPoint notificationEndPoint =
                      _context.NotificationEndPoints.Create("monitoring",
                      NotificationEndPointType.AzureTable, GetTableEndPoint());

            monitoringConfiguration = _context.MonitoringConfigurations.Create(notificationEndPoint.Id,
                new List<ComponentMonitoringSetting>()
                {
                    new ComponentMonitoringSetting(MonitoringComponent.Channel, MonitoringLevel.Normal),
                    new ComponentMonitoringSetting(MonitoringComponent.StreamingEndpoint, MonitoringLevel.Normal)

                });
            }

            //Print metrics for a Streaming Endpoint.
            PrintStreamingEndpointMetrics();

            Console.ReadLine();
        }

        private static string GetTableEndPoint()
        {
            return "https://" + _mediaServicesStorageAccountName + ".table.core.windows.net/";
        }

        private static void PrintStreamingEndpointMetrics()
        {
            Console.WriteLine(string.Format("Telemetry for streaming endpoint '{0}'", _streamingEndpoint.Name));

            DateTime timerangeEnd = DateTime.UtcNow;
            DateTime timerangeStart = DateTime.UtcNow.AddHours(-5);

            // Get some streaming endpoint metrics.
            var telemetry = _streamingEndpoint.GetTelemetry();

            var res = telemetry.GetStreamingEndpointRequestLogs(timerangeStart, timerangeEnd);

            Console.Title = "Streaming endpoint metrics:";

            foreach (var log in res)
            {
            Console.WriteLine("AccountId: {0}", log.AccountId);
            Console.WriteLine("BytesSent: {0}", log.BytesSent);
            Console.WriteLine("EndToEndLatency: {0}", log.EndToEndLatency);
            Console.WriteLine("HostName: {0}", log.HostName);
            Console.WriteLine("ObservedTime: {0}", log.ObservedTime);
            Console.WriteLine("PartitionKey: {0}", log.PartitionKey);
            Console.WriteLine("RequestCount: {0}", log.RequestCount);
            Console.WriteLine("ResultCode: {0}", log.ResultCode);
            Console.WriteLine("RowKey: {0}", log.RowKey);
            Console.WriteLine("ServerLatency: {0}", log.ServerLatency);
            Console.WriteLine("StatusCode: {0}", log.StatusCode);
            Console.WriteLine("StreamingEndpointId: {0}", log.StreamingEndpointId);
            Console.WriteLine();
            }

            Console.WriteLine();
        }

        private static void PrintChannelMetrics()
        {
            if (_channel == null)
            {
            Console.WriteLine("There are no channels in this AMS account");
            return;
            }

            Console.WriteLine(string.Format("Telemetry for channel '{0}'", _channel.Name));

            DateTime timerangeEnd = DateTime.UtcNow; 
            DateTime timerangeStart = DateTime.UtcNow.AddHours(-5);

            // Get some channel metrics.
            var telemetry = _channel.GetTelemetry();

            var channelMetrics = telemetry.GetChannelHeartbeats(timerangeStart, timerangeEnd);

            // Print hello channel metrics.
            Console.WriteLine("Channel metrics:");

            foreach (var channelHeartbeat in channelMetrics.OrderBy(x => x.ObservedTime))
            {
            Console.WriteLine(
                "    Observed time: {0}, Last timestamp: {1}, Incoming bitrate: {2}",
                channelHeartbeat.ObservedTime,
                channelHeartbeat.LastTimestamp,
                channelHeartbeat.IncomingBitrate);
            }

            Console.WriteLine();
        }
        }
    }


## <a name="next-steps"></a><span data-ttu-id="2bf74-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2bf74-125">Next steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="2bf74-126">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="2bf74-126">Provide feedback</span></span>

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
