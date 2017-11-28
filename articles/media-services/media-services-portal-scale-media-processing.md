---
title: "aaaScale adathordozó használatával feldolgozása hello Azure portálon |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello méretezési adathordozó feldolgozása hello Azure-portál használatával."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: 89240c6f7579b8795e7b47f2b1c398b1d5477e20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-reserved-unit-type"></a><span data-ttu-id="b177d-103">Hello szolgáltatás számára fenntartott egység típusának módosítása</span><span class="sxs-lookup"><span data-stu-id="b177d-103">Change hello reserved unit type</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b177d-104">.NET</span><span class="sxs-lookup"><span data-stu-id="b177d-104">.NET</span></span>](media-services-dotnet-encoding-units.md)
> * [<span data-ttu-id="b177d-105">Portál</span><span class="sxs-lookup"><span data-stu-id="b177d-105">Portal</span></span>](media-services-portal-scale-media-processing.md)
> * [<span data-ttu-id="b177d-106">REST</span><span class="sxs-lookup"><span data-stu-id="b177d-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [<span data-ttu-id="b177d-107">Java</span><span class="sxs-lookup"><span data-stu-id="b177d-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="b177d-108">PHP</span><span class="sxs-lookup"><span data-stu-id="b177d-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="b177d-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b177d-109">Overview</span></span>

<span data-ttu-id="b177d-110">Egy Media Services-fiók fenntartott egység típusú, amely megadja, hogy hello sebesség, amellyel a feladatok feldolgozása media feldolgozása hozzá rendelve.</span><span class="sxs-lookup"><span data-stu-id="b177d-110">A Media Services account is associated with a Reserved Unit Type, which determines hello speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="b177d-111">Hello következő közötti választhatja ki a szolgáltatás számára fenntartott egység: **S1**, **S2**, vagy **S3**.</span><span class="sxs-lookup"><span data-stu-id="b177d-111">You can pick between hello following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="b177d-112">Például ugyanazon kódolási feladat fut gyorsabban hello használatakor hello **S2** fenntartott egységnek típus összehasonlítása toohello **S1** típusa.</span><span class="sxs-lookup"><span data-stu-id="b177d-112">For example, hello same encoding job runs faster when you use hello **S2** reserved unit type compare toohello **S1** type.</span></span>

<span data-ttu-id="b177d-113">Ezenkívül toospecifying hello fenntartott egység típusát, akkor megadhatja tooprovision fiókját **fenntartott egységek** (RUs).</span><span class="sxs-lookup"><span data-stu-id="b177d-113">In addition toospecifying hello reserved unit type, you can specify tooprovision your account with **Reserved Units** (RUs).</span></span> <span data-ttu-id="b177d-114">kiépített RUs hello száma határozza meg, egyidejűleg dolgozhatók fel egy adott fiókhoz media feladatok hello száma.</span><span class="sxs-lookup"><span data-stu-id="b177d-114">hello number of provisioned RUs determines hello number of media tasks that can be processed concurrently in a given account.</span></span>

>[!NOTE]
><span data-ttu-id="b177d-115">A Fenntartott egységek az összes médiafeldolgozás párhuzamossá tételéért felelősek, beleértve az Azure Media Indexerrel végzett indexelési feladatokat is.</span><span class="sxs-lookup"><span data-stu-id="b177d-115">RUs work for parallelizing all media processing, including indexing jobs using Azure Media Indexer.</span></span> <span data-ttu-id="b177d-116">De a kódolással ellentétben az indexelési feladatok feldolgozása nem lesz gyorsabb a gyorsabb Fenntartott egységekkel.</span><span class="sxs-lookup"><span data-stu-id="b177d-116">However, unlike encoding, indexing jobs do not get processed faster with faster reserved units.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b177d-117">Győződjön meg arról, hogy tooreview hello [áttekintése](media-services-scale-media-processing-overview.md) témakör tooget témakör feldolgozása media méretezésével kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="b177d-117">Make sure tooreview hello [overview](media-services-scale-media-processing-overview.md) topic tooget more information about scaling media processing topic.</span></span>
> 
> 

## <a name="scale-media-processing"></a><span data-ttu-id="b177d-118">Méretezhető médiafeldolgozás</span><span class="sxs-lookup"><span data-stu-id="b177d-118">Scale media processing</span></span>
<span data-ttu-id="b177d-119">toochange hello szolgáltatás számára fenntartott egység típusa és hello fenntartott egységek számát, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b177d-119">toochange hello reserved unit type and hello number of reserved units, do hello following:</span></span>

1. <span data-ttu-id="b177d-120">A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="b177d-120">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="b177d-121">A hello **beállítások** ablakban válassza ki **Media szolgáltatás számára fenntartott egység**.</span><span class="sxs-lookup"><span data-stu-id="b177d-121">In hello **Settings** window, select **Media reserved units**.</span></span>
   
    <span data-ttu-id="b177d-122">hello fenntartott egységek száma toochange hello fenntartott egységnek típust választotta, használja a hello **Media kiszolgált egységek** csúszkát.</span><span class="sxs-lookup"><span data-stu-id="b177d-122">toochange hello number of reserved units for hello selected reserved unit type, use hello **Media Served Units** slider.</span></span>
   
    <span data-ttu-id="b177d-123">toochange hello **FENNTARTOTT EGYSÉGTÍPUS**, nyomja le az S1, S2 vagy S3.</span><span class="sxs-lookup"><span data-stu-id="b177d-123">toochange hello **RESERVED UNIT TYPE**, press S1, S2, or S3.</span></span>
   
    ![Feldolgozók lap](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. <span data-ttu-id="b177d-125">Nyomja le az hello gomb toosave menti a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b177d-125">Press hello SAVE button toosave your changes.</span></span>
   
    <span data-ttu-id="b177d-126">új fenntartott egységek hello MENTÉS megnyomásakor foglal le.</span><span class="sxs-lookup"><span data-stu-id="b177d-126">hello new reserved units are allocated when you press SAVE.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b177d-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b177d-127">Next steps</span></span>
<span data-ttu-id="b177d-128">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="b177d-128">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b177d-129">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="b177d-129">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

