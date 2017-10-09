---
title: "kódolási egységeket - Azure általi feldolgozás aaaScale media |}  Microsoft Docs"
description: "Megtudhatja, hogyan toohow tooadd kódolási egységek .NET"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;milangada;
ms.openlocfilehash: b9f71a6487c5d136319a38a1598d60edfaa81b9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-encoding-with-net-sdk"></a><span data-ttu-id="0bfd9-103">Hogyan tooscale kódolás .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="0bfd9-103">How tooscale encoding with .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0bfd9-104">Portál</span><span class="sxs-lookup"><span data-stu-id="0bfd9-104">Portal</span></span>](media-services-portal-scale-media-processing.md)
> * [<span data-ttu-id="0bfd9-105">.NET</span><span class="sxs-lookup"><span data-stu-id="0bfd9-105">.NET</span></span>](media-services-dotnet-encoding-units.md)
> * [<span data-ttu-id="0bfd9-106">REST</span><span class="sxs-lookup"><span data-stu-id="0bfd9-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [<span data-ttu-id="0bfd9-107">Java</span><span class="sxs-lookup"><span data-stu-id="0bfd9-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="0bfd9-108">PHP</span><span class="sxs-lookup"><span data-stu-id="0bfd9-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="0bfd9-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0bfd9-109">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0bfd9-110">Győződjön meg arról, hogy tooreview hello [áttekintése](media-services-scale-media-processing-overview.md) témakör tooget témakör feldolgozása media méretezésével kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="0bfd9-110">Make sure tooreview hello [overview](media-services-scale-media-processing-overview.md) topic tooget more information about scaling media processing topic.</span></span>
> 
> 

<span data-ttu-id="0bfd9-111">toochange hello szolgáltatás számára fenntartott egység típusa és hello száma kódoláshoz fenntartott egység .NET SDK használatával hello a következő:</span><span class="sxs-lookup"><span data-stu-id="0bfd9-111">toochange hello reserved unit type and hello number of encoding reserved units using .NET SDK, do hello following:</span></span>

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds tooS1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a><span data-ttu-id="0bfd9-112">Támogatási jegy megnyitása</span><span class="sxs-lookup"><span data-stu-id="0bfd9-112">Opening a Support Ticket</span></span>
<span data-ttu-id="0bfd9-113">Alapértelmezés szerint minden Media Services-fiók tooup too25 méretezhető kódolási és 5 igény szerinti folyamatos átvitelhez fenntartott egységek.</span><span class="sxs-lookup"><span data-stu-id="0bfd9-113">By default every Media Services account can scale tooup too25 Encoding and 5 On-Demand Streaming Reserved Units.</span></span> <span data-ttu-id="0bfd9-114">Magasabb határérték kérhet egy támogatási jegy megnyitásával.</span><span class="sxs-lookup"><span data-stu-id="0bfd9-114">You can request a higher limit by opening a support ticket.</span></span>

### <a name="open-a-support-ticket"></a><span data-ttu-id="0bfd9-115">Támogatási jegy megnyitása</span><span class="sxs-lookup"><span data-stu-id="0bfd9-115">Open a support ticket</span></span>
<span data-ttu-id="0bfd9-116">támogatási jegy tooopen hello a következő:</span><span class="sxs-lookup"><span data-stu-id="0bfd9-116">tooopen a support ticket do hello following:</span></span>

1. <span data-ttu-id="0bfd9-117">Kattintson a [segítségre van szüksége](https://manage.windowsazure.com/?getsupport=true).</span><span class="sxs-lookup"><span data-stu-id="0bfd9-117">Click [Get Support](https://manage.windowsazure.com/?getsupport=true).</span></span> <span data-ttu-id="0bfd9-118">Ha nem jelentkezett be, meg fog felszólító tooenter kell a hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="0bfd9-118">If you are not logged in, you will be prompted tooenter your credentials.</span></span>
2. <span data-ttu-id="0bfd9-119">Válassza ki előfizetését.</span><span class="sxs-lookup"><span data-stu-id="0bfd9-119">Select your subscription.</span></span>
3. <span data-ttu-id="0bfd9-120">A támogatási típusa válassza a "Műszaki".</span><span class="sxs-lookup"><span data-stu-id="0bfd9-120">Under support type, select "Technical".</span></span>
4. <span data-ttu-id="0bfd9-121">Kattintson az "A jegy létrehozása".</span><span class="sxs-lookup"><span data-stu-id="0bfd9-121">Click on "Create Ticket".</span></span>
5. <span data-ttu-id="0bfd9-122">Válassza ki az "Azure Media Services" hello termékek listáját a következő hello lapon megjelenő.</span><span class="sxs-lookup"><span data-stu-id="0bfd9-122">Select "Azure Media Services" in hello product list presented on hello next page.</span></span>
6. <span data-ttu-id="0bfd9-123">Válassza ki a "probléma típusa" megfelelő a problémát.</span><span class="sxs-lookup"><span data-stu-id="0bfd9-123">Select a "Problem type" that is appropriate for your issue.</span></span>
7. <span data-ttu-id="0bfd9-124">Kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="0bfd9-124">Click Continue.</span></span>
8. <span data-ttu-id="0bfd9-125">Kövesse az utasításokat a következő oldalon, és adja meg a probléma részleteit.</span><span class="sxs-lookup"><span data-stu-id="0bfd9-125">Follow instructions on next page and then enter details about your issue.</span></span>
9. <span data-ttu-id="0bfd9-126">Kattintson a tooopen hello jegy nyújt.</span><span class="sxs-lookup"><span data-stu-id="0bfd9-126">Click submit tooopen hello ticket.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="0bfd9-127">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="0bfd9-127">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0bfd9-128">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="0bfd9-128">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

