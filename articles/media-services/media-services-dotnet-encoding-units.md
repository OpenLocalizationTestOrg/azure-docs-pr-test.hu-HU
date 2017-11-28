---
title: "Kódolási egységeket - Azure általi feldolgozás media méretezése |}  Microsoft Docs"
description: "Megtudhatja, hogyan kell a .NET kódolási egység hozzáadása"
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
ms.openlocfilehash: 72a8729d22a9e76c8076d7a3347619a2163e4f09
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-scale-encoding-with-net-sdk"></a><span data-ttu-id="6a271-103">A kódolás méretezése a .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="6a271-103">How to scale encoding with .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6a271-104">Portal</span><span class="sxs-lookup"><span data-stu-id="6a271-104">Portal</span></span>](media-services-portal-scale-media-processing.md)
> * [<span data-ttu-id="6a271-105">.NET</span><span class="sxs-lookup"><span data-stu-id="6a271-105">.NET</span></span>](media-services-dotnet-encoding-units.md)
> * [<span data-ttu-id="6a271-106">REST</span><span class="sxs-lookup"><span data-stu-id="6a271-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [<span data-ttu-id="6a271-107">Java</span><span class="sxs-lookup"><span data-stu-id="6a271-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="6a271-108">PHP</span><span class="sxs-lookup"><span data-stu-id="6a271-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="6a271-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6a271-109">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="6a271-110">Mindenképpen tekintse át a [áttekintése](media-services-scale-media-processing-overview.md) témakör feldolgozása media méretezésével kapcsolatos további információkat a témakör.</span><span class="sxs-lookup"><span data-stu-id="6a271-110">Make sure to review the [overview](media-services-scale-media-processing-overview.md) topic to get more information about scaling media processing topic.</span></span>
> 
> 

<span data-ttu-id="6a271-111">A fenntartott egységnek típusának és a kódoláshoz fenntartott egység .NET SDK használatával számának módosításához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6a271-111">To change the reserved unit type and the number of encoding reserved units using .NET SDK, do the following:</span></span>

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a><span data-ttu-id="6a271-112">Támogatási jegy megnyitása</span><span class="sxs-lookup"><span data-stu-id="6a271-112">Opening a Support Ticket</span></span>
<span data-ttu-id="6a271-113">Alapértelmezés szerint minden Media Services-fiók méretezhető legfeljebb 25 kódolás és 5 igény, folyamatos átvitelhez fenntartott egységek.</span><span class="sxs-lookup"><span data-stu-id="6a271-113">By default every Media Services account can scale to up to 25 Encoding and 5 On-Demand Streaming Reserved Units.</span></span> <span data-ttu-id="6a271-114">Magasabb határérték kérhet egy támogatási jegy megnyitásával.</span><span class="sxs-lookup"><span data-stu-id="6a271-114">You can request a higher limit by opening a support ticket.</span></span>

### <a name="open-a-support-ticket"></a><span data-ttu-id="6a271-115">Támogatási jegy megnyitása</span><span class="sxs-lookup"><span data-stu-id="6a271-115">Open a support ticket</span></span>
<span data-ttu-id="6a271-116">Nyissa meg a támogatási jegy tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6a271-116">To open a support ticket do the following:</span></span>

1. <span data-ttu-id="6a271-117">Kattintson a [segítségre van szüksége](https://manage.windowsazure.com/?getsupport=true).</span><span class="sxs-lookup"><span data-stu-id="6a271-117">Click [Get Support](https://manage.windowsazure.com/?getsupport=true).</span></span> <span data-ttu-id="6a271-118">Ha nem jelentkezett be, kérni fogja a hitelesítő adatok megadását.</span><span class="sxs-lookup"><span data-stu-id="6a271-118">If you are not logged in, you will be prompted to enter your credentials.</span></span>
2. <span data-ttu-id="6a271-119">Válassza ki előfizetését.</span><span class="sxs-lookup"><span data-stu-id="6a271-119">Select your subscription.</span></span>
3. <span data-ttu-id="6a271-120">A támogatási típusa válassza a "Műszaki".</span><span class="sxs-lookup"><span data-stu-id="6a271-120">Under support type, select "Technical".</span></span>
4. <span data-ttu-id="6a271-121">Kattintson az "A jegy létrehozása".</span><span class="sxs-lookup"><span data-stu-id="6a271-121">Click on "Create Ticket".</span></span>
5. <span data-ttu-id="6a271-122">Válassza ki az "Azure Media Services" a termék listájában jelenik meg a következő oldalon.</span><span class="sxs-lookup"><span data-stu-id="6a271-122">Select "Azure Media Services" in the product list presented on the next page.</span></span>
6. <span data-ttu-id="6a271-123">Válassza ki a "probléma típusa" megfelelő a problémát.</span><span class="sxs-lookup"><span data-stu-id="6a271-123">Select a "Problem type" that is appropriate for your issue.</span></span>
7. <span data-ttu-id="6a271-124">Kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="6a271-124">Click Continue.</span></span>
8. <span data-ttu-id="6a271-125">Kövesse az utasításokat a következő oldalon, és adja meg a probléma részleteit.</span><span class="sxs-lookup"><span data-stu-id="6a271-125">Follow instructions on next page and then enter details about your issue.</span></span>
9. <span data-ttu-id="6a271-126">Kattintson a hibajegyet nyújt.</span><span class="sxs-lookup"><span data-stu-id="6a271-126">Click submit to open the ticket.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="6a271-127">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="6a271-127">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6a271-128">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="6a271-128">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

