---
title: "aaaHow tooperform élő Stream továbbítása helyszíni kódolókkal .NET használatával |} Microsoft Docs"
description: "Ez a témakör bemutatja hogyan toouse .NET tooperform élő helyszíni kódolókkal kódolási."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 15908152-d23c-4d55-906a-3bfd74927db5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 07/18/2017
ms.author: cenkdin;juliako
ms.openlocfilehash: 332582c9f925f8b9270929b3fa8140fce010bbf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-live-streaming-with-on-premises-encoders-using-net"></a><span data-ttu-id="57f7d-103">Hogyan tooperform élő adatfolyam helyszíni kódolókkal .NET használatával</span><span class="sxs-lookup"><span data-stu-id="57f7d-103">How tooperform live streaming with on-premises encoders using .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="57f7d-104">Portal</span><span class="sxs-lookup"><span data-stu-id="57f7d-104">Portal</span></span>](media-services-portal-live-passthrough-get-started.md)
> * [<span data-ttu-id="57f7d-105">.NET</span><span class="sxs-lookup"><span data-stu-id="57f7d-105">.NET</span></span>](media-services-dotnet-live-encode-with-onpremises-encoders.md)
> * [<span data-ttu-id="57f7d-106">REST</span><span class="sxs-lookup"><span data-stu-id="57f7d-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

<span data-ttu-id="57f7d-107">Ez az oktatóanyag végigvezeti hello hello Azure Media Services .NET SDK toocreate használatával egy **csatorna** , amely átmenő közvetítésre van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="57f7d-107">This tutorial walks you through hello steps of using hello Azure Media Services .NET SDK toocreate a **Channel** that is configured for a pass-through delivery.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="57f7d-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="57f7d-108">Prerequisites</span></span>
<span data-ttu-id="57f7d-109">Az alábbiakban hello szükséges toocomplete hello oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="57f7d-109">hello following are required toocomplete hello tutorial:</span></span>

* <span data-ttu-id="57f7d-110">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="57f7d-110">An Azure account.</span></span>
* <span data-ttu-id="57f7d-111">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="57f7d-111">A Media Services account.</span></span>    <span data-ttu-id="57f7d-112">egy Media Services-fiók toocreate lásd [hogyan tooCreate Media Services-fiók](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="57f7d-112">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="57f7d-113">A fejlesztői környezet beállítása.</span><span class="sxs-lookup"><span data-stu-id="57f7d-113">Set up your dev environment.</span></span> <span data-ttu-id="57f7d-114">További információkért lásd: [állítsa be a környezetet](media-services-set-up-computer.md).</span><span class="sxs-lookup"><span data-stu-id="57f7d-114">For more information, see [Set up your environment](media-services-set-up-computer.md).</span></span>
* <span data-ttu-id="57f7d-115">Egy webkamera.</span><span class="sxs-lookup"><span data-stu-id="57f7d-115">A webcam.</span></span> <span data-ttu-id="57f7d-116">Például a [Telestream Wirecast kódoló](http://www.telestream.net/wirecast/overview.htm).</span><span class="sxs-lookup"><span data-stu-id="57f7d-116">For example, [Telestream Wirecast encoder](http://www.telestream.net/wirecast/overview.htm).</span></span>

<span data-ttu-id="57f7d-117">Ajánlott tooreview hello a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="57f7d-117">Recommended tooreview hello following articles:</span></span>

* <span data-ttu-id="57f7d-118">[Azure Media Services RTMP Support and Live Encoders](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/) (Az Azure Media Services RTMP-támogatása és az élő kódolók)</span><span class="sxs-lookup"><span data-stu-id="57f7d-118">[Azure Media Services RTMP Support and Live Encoders](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)</span></span>
* <span data-ttu-id="57f7d-119">[Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md) (Élő stream továbbítása többszörös átviteli sebességű streamet létrehozó helyszíni kódolókkal)</span><span class="sxs-lookup"><span data-stu-id="57f7d-119">[Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md)</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="57f7d-120">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="57f7d-120">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="57f7d-121">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="57f7d-121">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="example"></a><span data-ttu-id="57f7d-122">Példa</span><span class="sxs-lookup"><span data-stu-id="57f7d-122">Example</span></span>
<span data-ttu-id="57f7d-123">a következő példakód hello bemutatja, hogyan tooachieve hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="57f7d-123">hello following code example demonstrates how tooachieve hello following tasks:</span></span>

* <span data-ttu-id="57f7d-124">Connect tooMedia szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="57f7d-124">Connect tooMedia Services</span></span>
* <span data-ttu-id="57f7d-125">Csatorna létrehozása</span><span class="sxs-lookup"><span data-stu-id="57f7d-125">Create a channel</span></span>
* <span data-ttu-id="57f7d-126">Frissítés hello csatorna</span><span class="sxs-lookup"><span data-stu-id="57f7d-126">Update hello channel</span></span>
* <span data-ttu-id="57f7d-127">Hello csatorna bemeneti végpont beolvasása.</span><span class="sxs-lookup"><span data-stu-id="57f7d-127">Retrieve hello channel’s input endpoint.</span></span> <span data-ttu-id="57f7d-128">hello bemeneti végpontot kell megadni a toohello a helyszíni élő kódoló.</span><span class="sxs-lookup"><span data-stu-id="57f7d-128">hello input endpoint should be provided toohello on-premises live encoder.</span></span> <span data-ttu-id="57f7d-129">hello élő kódoló konvertálja jeleinek hello kamera toostreams toohello csatorna bemeneti küldött (betöltési) végpont.</span><span class="sxs-lookup"><span data-stu-id="57f7d-129">hello live encoder converts signals from hello camera toostreams that are sent toohello channel’s input (ingest) endpoint.</span></span>
* <span data-ttu-id="57f7d-130">Hello csatorna előnézeti végpont lekérése</span><span class="sxs-lookup"><span data-stu-id="57f7d-130">Retrieve hello channel’s preview endpoint</span></span>
* <span data-ttu-id="57f7d-131">Hozzon létre, és egy programot elindítani</span><span class="sxs-lookup"><span data-stu-id="57f7d-131">Create and start a program</span></span>
* <span data-ttu-id="57f7d-132">A szükséges tooaccess hello program egy kereső létrehozása</span><span class="sxs-lookup"><span data-stu-id="57f7d-132">Create a locator needed tooaccess hello program</span></span>
* <span data-ttu-id="57f7d-133">Hozzon létre, és indítsa el a StreamingEndpoint</span><span class="sxs-lookup"><span data-stu-id="57f7d-133">Create and start a StreamingEndpoint</span></span>
* <span data-ttu-id="57f7d-134">Adatfolyam-továbbítási végpontra hello frissítése</span><span class="sxs-lookup"><span data-stu-id="57f7d-134">Update hello streaming endpoint</span></span>
* <span data-ttu-id="57f7d-135">Erőforrások leállítása</span><span class="sxs-lookup"><span data-stu-id="57f7d-135">Shut down resources</span></span>

>[!IMPORTANT]
><span data-ttu-id="57f7d-136">Győződjön meg arról, amelyből el kívánja toostream tartalom streamvégpontra hello hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="57f7d-136">Make sure hello streaming endpoint from which you want toostream content is in hello **Running** state.</span></span> 
    
>[!NOTE]
><span data-ttu-id="57f7d-137">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="57f7d-137">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="57f7d-138">Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek.</span><span class="sxs-lookup"><span data-stu-id="57f7d-138">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="57f7d-139">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="57f7d-139">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="57f7d-140">Információ az élő kódolók tooconfigure lásd: [Azure Media Services RMTP-támogatása és valós idejű kódolók](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/).</span><span class="sxs-lookup"><span data-stu-id="57f7d-140">For information on how tooconfigure a live encoder, see [Azure Media Services RTMP Support and Live Encoders](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/).</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Linq;
    using System.Net;
    using System.Security.Cryptography;
    using Microsoft.WindowsAzure.MediaServices.Client;

    namespace AMSLiveTest
    {
        class Program
        {
        private const string StreamingEndpointName = "streamingendpoint001";
        private const string ChannelName = "channel001";
        private const string AssetlName = "asset001";
        private const string ProgramlName = "program001";

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

            IChannel channel = CreateAndStartChannel();

            // Set hello Live Encoder toopoint toohello channel's input endpoint:
            string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();

            // Use hello previewEndpoint toopreview and verify
            // that hello input from hello encoder is actually reaching hello Channel.
            string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();

            IProgram program = CreateAndStartProgram(channel);
            ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);
            IStreamingEndpoint streamingEndpoint = CreateAndStartStreamingEndpoint(false);

            // Once you are done streaming, clean up your resources.
            Cleanup(streamingEndpoint, channel);
        }

        public static IChannel CreateAndStartChannel()
        {
            //If you want toochange hello Smooth fragments tooHLS segment ratio, you would set hello ChannelCreationOptions’s Output property.

            IChannel channel = _context.Channels.Create(
            new ChannelCreationOptions
            {
            Name = ChannelName,
            Input = CreateChannelInput(),
            Preview = CreateChannelPreview()
            });

            //Starting and stopping Channels can take some time tooexecute. toodetermine hello state of operations after calling Start or Stop, query hello IChannel.State .

            channel.Start();

            return channel;
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
                    // Setting 0.0.0.0 for Address and 0 for SubnetPrefixLength
                    // will allow access tooIP addresses.
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
                    // Setting 0.0.0.0 for Address and 0 for SubnetPrefixLength
                    // will allow access tooIP addresses.
                    Address = IPAddress.Parse("0.0.0.0"),
                    SubnetPrefixLength = 0
                    }
                }
            }
            };
        }

        public static void UpdateCrossSiteAccessPoliciesForChannel(IChannel channel)
        {
            var clientPolicy =
            @"<?xml version=""1.0"" encoding=""utf-8""?>
            <access-policy>
                <cross-domain-access>
                <policy>
                    <allow-from http-request-headers=""*"" http-methods=""*"">
                    <domain uri=""*""/>
                    </allow-from>
                    <grant-to>
                       <resource path=""/"" include-subpaths=""true""/>
                    </grant-to>
                </policy>
                </cross-domain-access>
            </access-policy>";

            var xdomainPolicy =
            @"<?xml version=""1.0"" ?>
            <cross-domain-policy>
                <allow-access-from domain=""*"" />
            </cross-domain-policy>";

            channel.CrossSiteAccessPolicies.ClientAccessPolicy = clientPolicy;
            channel.CrossSiteAccessPolicies.CrossDomainPolicy = xdomainPolicy;

            channel.Update();
        }

        public static IProgram CreateAndStartProgram(IChannel channel)
        {
            IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);

            // Create a Program on hello Channel. You can have multiple Programs that overlap or are sequential;
            // however each Program must have a unique name within your Media Services account.
            IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
            program.Start();

            return program;
        }

        public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
        {
            // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            

            var locator = _context.Locators.CreateLocator
            (
                LocatorType.OnDemandOrigin,
                asset,
                _context.AccessPolicies.Create
                (
                "Live Stream Policy",
                ArchiveWindowLength,
                AccessPermissions.Read
                )
            );

            return locator;
        }

        public static IStreamingEndpoint CreateAndStartStreamingEndpoint(bool createNew)
        {
            IStreamingEndpoint streamingEndpoint = null;
            if (createNew)
            {
            var options = new StreamingEndpointCreationOptions
            {
                Name = StreamingEndpointName,
                ScaleUnits = 1,
                AccessControl = GetAccessControl(),
                CacheControl = GetCacheControl()
            };

            streamingEndpoint = _context.StreamingEndpoints.Create(options);
            }
            else
            {
            streamingEndpoint = _context.StreamingEndpoints.FirstOrDefault();
            }


            if (streamingEndpoint.State == StreamingEndpointState.Stopped)
            streamingEndpoint.Start();

            return streamingEndpoint;
        }

        private static StreamingEndpointAccessControl GetAccessControl()
        {
            return new StreamingEndpointAccessControl
            {
            IPAllowList = new List<IPRange>
                {
                new IPRange
                {
                    Name = "Allow all",
                    Address = IPAddress.Parse("0.0.0.0"),
                    SubnetPrefixLength = 0
                }
                },

            AkamaiSignatureHeaderAuthenticationKeyList = new List<AkamaiSignatureHeaderAuthenticationKey>
                {
                new AkamaiSignatureHeaderAuthenticationKey
                {
                    Identifier = "My key",
                    Expiration = DateTime.UtcNow + TimeSpan.FromDays(365),
                    Base64Key = Convert.ToBase64String(GenerateRandomBytes(16))
                }
                }
            };
        }

        private static byte[] GenerateRandomBytes(int length)
        {
            var bytes = new byte[length];
            using (var rng = new RNGCryptoServiceProvider())
            {
            rng.GetBytes(bytes);
            }

            return bytes;
        }

        private static StreamingEndpointCacheControl GetCacheControl()
        {
            return new StreamingEndpointCacheControl
            {
            MaxAge = TimeSpan.FromSeconds(1000)
            };
        }

        public static void UpdateCrossSiteAccessPoliciesForStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            var clientPolicy =
            @"<?xml version=""1.0"" encoding=""utf-8""?>
            <access-policy>
                <cross-domain-access>
                <policy>
                    <allow-from http-request-headers=""*"" http-methods=""*"">
                    <domain uri=""*""/>
                    </allow-from>
                    <grant-to>
                       <resource path=""/"" include-subpaths=""true""/>
                    </grant-to>
                </policy>
                </cross-domain-access>
            </access-policy>";

            var xdomainPolicy =
            @"<?xml version=""1.0"" ?>
            <cross-domain-policy>
                <allow-access-from domain=""*"" />
            </cross-domain-policy>";

            streamingEndpoint.CrossSiteAccessPolicies.ClientAccessPolicy = clientPolicy;
            streamingEndpoint.CrossSiteAccessPolicies.CrossDomainPolicy = xdomainPolicy;

            streamingEndpoint.Update();
        }

        public static void Cleanup(IStreamingEndpoint streamingEndpoint,
                        IChannel channel)
        {
            if (streamingEndpoint != null)
            {
            streamingEndpoint.Stop();
            if(streamingEndpoint.Name != "default")
                streamingEndpoint.Delete();
            }

            IAsset asset;
            if (channel != null)
            {

            foreach (var program in channel.Programs)
            {
                asset = _context.Assets.Where(se => se.Id == program.AssetId)
                            .FirstOrDefault();

                program.Stop();
                program.Delete();

                if (asset != null)
                {
                foreach (var l in asset.Locators)
                    l.Delete();

                asset.Delete();
                }
            }

            channel.Stop();
            channel.Delete();
            }
        }
        }
    }

## <a name="next-step"></a><span data-ttu-id="57f7d-141">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="57f7d-141">Next Step</span></span>
<span data-ttu-id="57f7d-142">Tekintse át a Media Services tanulási útvonalai</span><span class="sxs-lookup"><span data-stu-id="57f7d-142">Review Media Services learning paths</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="57f7d-143">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="57f7d-143">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

