---
title: "Skála media feldolgozása az Azure portál használatával |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti a méretezési adathordozó feldolgozása az Azure portál használatával."
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
ms.openlocfilehash: 46ca29d3e66701f2abcb185791089e94761984e8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="change-the-reserved-unit-type"></a><span data-ttu-id="4c18e-103">A fenntartott egység típusának módosítása</span><span class="sxs-lookup"><span data-stu-id="4c18e-103">Change the reserved unit type</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4c18e-104">.NET</span><span class="sxs-lookup"><span data-stu-id="4c18e-104">.NET</span></span>](media-services-dotnet-encoding-units.md)
> * [<span data-ttu-id="4c18e-105">Portal</span><span class="sxs-lookup"><span data-stu-id="4c18e-105">Portal</span></span>](media-services-portal-scale-media-processing.md)
> * [<span data-ttu-id="4c18e-106">REST</span><span class="sxs-lookup"><span data-stu-id="4c18e-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [<span data-ttu-id="4c18e-107">Java</span><span class="sxs-lookup"><span data-stu-id="4c18e-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="4c18e-108">PHP</span><span class="sxs-lookup"><span data-stu-id="4c18e-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="4c18e-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4c18e-109">Overview</span></span>

<span data-ttu-id="4c18e-110">A Media Services-fiókok Fenntartott egység típussal vannak társítva, amely meghatározza a médiafeldolgozási feladatok feldolgozásának sebességét.</span><span class="sxs-lookup"><span data-stu-id="4c18e-110">A Media Services account is associated with a Reserved Unit Type, which determines the speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="4c18e-111">A következő Fenntartott egység típusok közül választhat: **S1**, **S2** vagy **S3**.</span><span class="sxs-lookup"><span data-stu-id="4c18e-111">You can pick between the following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="4c18e-112">Ugyanaz a kódolási feladat például gyorsabban fut, amikor az **S2** Fenntartott egység típust használja az **S1** típus helyett.</span><span class="sxs-lookup"><span data-stu-id="4c18e-112">For example, the same encoding job runs faster when you use the **S2** reserved unit type compare to the **S1** type.</span></span>

<span data-ttu-id="4c18e-113">A Fenntartott egység típusának meghatározása mellett megadhatja, hogy ellátja-e a fiókot **Fenntartott egységekkel** (RU-kkal).</span><span class="sxs-lookup"><span data-stu-id="4c18e-113">In addition to specifying the reserved unit type, you can specify to provision your account with **Reserved Units** (RUs).</span></span> <span data-ttu-id="4c18e-114">A megadott Fenntartott egységek száma határozza meg az egy adott fiókon egy időben feldolgozható médiafeladatok számát.</span><span class="sxs-lookup"><span data-stu-id="4c18e-114">The number of provisioned RUs determines the number of media tasks that can be processed concurrently in a given account.</span></span>

>[!NOTE]
><span data-ttu-id="4c18e-115">A Fenntartott egységek az összes médiafeldolgozás párhuzamossá tételéért felelősek, beleértve az Azure Media Indexerrel végzett indexelési feladatokat is.</span><span class="sxs-lookup"><span data-stu-id="4c18e-115">RUs work for parallelizing all media processing, including indexing jobs using Azure Media Indexer.</span></span> <span data-ttu-id="4c18e-116">De a kódolással ellentétben az indexelési feladatok feldolgozása nem lesz gyorsabb a gyorsabb Fenntartott egységekkel.</span><span class="sxs-lookup"><span data-stu-id="4c18e-116">However, unlike encoding, indexing jobs do not get processed faster with faster reserved units.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4c18e-117">Mindenképpen tekintse át a [áttekintése](media-services-scale-media-processing-overview.md) témakör feldolgozása media méretezésével kapcsolatos további információkat a témakör.</span><span class="sxs-lookup"><span data-stu-id="4c18e-117">Make sure to review the [overview](media-services-scale-media-processing-overview.md) topic to get more information about scaling media processing topic.</span></span>
> 
> 

## <a name="scale-media-processing"></a><span data-ttu-id="4c18e-118">Méretezhető médiafeldolgozás</span><span class="sxs-lookup"><span data-stu-id="4c18e-118">Scale media processing</span></span>
<span data-ttu-id="4c18e-119">A fenntartott egységnek típusát és fenntartott egységek számának megváltoztatásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4c18e-119">To change the reserved unit type and the number of reserved units, do the following:</span></span>

1. <span data-ttu-id="4c18e-120">Az [Azure-portálon](https://portal.azure.com/) válassza ki Azure Media Services-fiókját.</span><span class="sxs-lookup"><span data-stu-id="4c18e-120">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="4c18e-121">Az a **beállítások** ablakban válassza ki **Media szolgáltatás számára fenntartott egység**.</span><span class="sxs-lookup"><span data-stu-id="4c18e-121">In the **Settings** window, select **Media reserved units**.</span></span>
   
    <span data-ttu-id="4c18e-122">A kijelölt fenntartott egységnek típus fenntartott egységek számának módosításához használja a **Media kiszolgált egységek** csúszkát.</span><span class="sxs-lookup"><span data-stu-id="4c18e-122">To change the number of reserved units for the selected reserved unit type, use the **Media Served Units** slider.</span></span>
   
    <span data-ttu-id="4c18e-123">Módosíthatja a **FENNTARTOTT EGYSÉGTÍPUS**, nyomja le az S1, S2 vagy S3.</span><span class="sxs-lookup"><span data-stu-id="4c18e-123">To change the **RESERVED UNIT TYPE**, press S1, S2, or S3.</span></span>
   
    ![Feldolgozók lap](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. <span data-ttu-id="4c18e-125">A módosítások mentéséhez kattintson a SAVE (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="4c18e-125">Press the SAVE button to save your changes.</span></span>
   
    <span data-ttu-id="4c18e-126">Az új fenntartott egységek MENTÉS megnyomásakor foglal le.</span><span class="sxs-lookup"><span data-stu-id="4c18e-126">The new reserved units are allocated when you press SAVE.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c18e-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4c18e-127">Next steps</span></span>
<span data-ttu-id="4c18e-128">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="4c18e-128">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4c18e-129">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="4c18e-129">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

