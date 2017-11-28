---
title: "adatfolyam-végpontok az Azure-portálon hello aaaScale |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello a méretezés streamvégpontok a hello Azure-portálon."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: e466edf9232558b9e270f54ee2849cd9b22ad121
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-streaming-endpoints-with-hello-azure-portal"></a><span data-ttu-id="c74a1-103">Hello Azure-portálon az adatfolyam-továbbítási végpontok méretezése</span><span class="sxs-lookup"><span data-stu-id="c74a1-103">Scale streaming endpoints with hello Azure portal</span></span>
## <a name="overview"></a><span data-ttu-id="c74a1-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c74a1-104">Overview</span></span>

> [!NOTE]
> <span data-ttu-id="c74a1-105">toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c74a1-105">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="c74a1-106">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c74a1-106">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="c74a1-107">A **prémium** szintű streamvégpontok a speciális feladatokhoz ideálisak, mert dedikált és méretezhető sávszélesség-kapacitást nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="c74a1-107">**Premium** streaming endpoints are suitable for advanced workloads, providing dedicated and scalable bandwidth capacity.</span></span> <span data-ttu-id="c74a1-108">A **prémium** streamvégponttal rendelkező ügyfelek alapértelmezés szerint kapnak egy adategységet (SU-t).</span><span class="sxs-lookup"><span data-stu-id="c74a1-108">Customers that have a **Premium** streaming endpoint, by default get one streaming unit (SU).</span></span> <span data-ttu-id="c74a1-109">adatfolyam-továbbítási végpontra hello SUs hozzáadásával is méretezhető.</span><span class="sxs-lookup"><span data-stu-id="c74a1-109">hello streaming endpoint can be scaled by adding SUs.</span></span> <span data-ttu-id="c74a1-110">Minden egyes SU további sávszélesség kapacitás toohello alkalmazásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c74a1-110">Each SU provides additional bandwidth capacity toohello application.</span></span> <span data-ttu-id="c74a1-111">További információ a streaming endpoint típusok és a CDN-konfiguráció: hello [Streaming Endpoint áttekintése](media-services-portal-manage-streaming-endpoints.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="c74a1-111">For more information about streaming endpoint types and CDN configuration, see hello [Streaming Endpoint overview](media-services-portal-manage-streaming-endpoints.md) topic.</span></span>
 
<span data-ttu-id="c74a1-112">Ez a témakör bemutatja, hogyan tooscale egy streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="c74a1-112">This topic shows how tooscale a streaming endpoint.</span></span>

<span data-ttu-id="c74a1-113">Az árképzésre vonatkozó további információkért lásd: [Media Services Pricing Details](http://go.microsoft.com/fwlink/?LinkId=275107) (A Media Services árképzésére vonatkozó információk).</span><span class="sxs-lookup"><span data-stu-id="c74a1-113">For information about pricing details, see [Media Services Pricing Details](http://go.microsoft.com/fwlink/?LinkId=275107).</span></span>

## <a name="scale-streaming-endpoints"></a><span data-ttu-id="c74a1-114">Az adatfolyam végpontjainak méretezése</span><span class="sxs-lookup"><span data-stu-id="c74a1-114">Scale streaming endpoints</span></span>

<span data-ttu-id="c74a1-115">streamelési egységek számát toochange hello hello a következő:</span><span class="sxs-lookup"><span data-stu-id="c74a1-115">toochange hello number of streaming units, do hello following:</span></span>

1. <span data-ttu-id="c74a1-116">A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="c74a1-116">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="c74a1-117">A hello **beállítások** ablakban válassza ki **adatfolyam-végpontok**.</span><span class="sxs-lookup"><span data-stu-id="c74a1-117">In hello **Settings** window, select **Streaming endpoints**.</span></span>
3. <span data-ttu-id="c74a1-118">Kattintson a hello streamvégpontra, amelyet az tooscale.</span><span class="sxs-lookup"><span data-stu-id="c74a1-118">Click on hello streaming endpoint that you want tooscale.</span></span> 

    [!NOTE] <span data-ttu-id="c74a1-119">Csak méretezheti **prémium** adatfolyam-végpontok.</span><span class="sxs-lookup"><span data-stu-id="c74a1-119">You can only scale **Premium** streaming endpoints.</span></span>

4. <span data-ttu-id="c74a1-120">Helyezze át a hello csúszkát toospecify hello streamelési egységek számát.</span><span class="sxs-lookup"><span data-stu-id="c74a1-120">Move hello slider toospecify hello number of streaming units.</span></span>

    ![Adatfolyam-továbbítási végpontra](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a><span data-ttu-id="c74a1-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c74a1-122">Next steps</span></span>
<span data-ttu-id="c74a1-123">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="c74a1-123">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c74a1-124">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="c74a1-124">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

