---
title: "Adatfolyam-végpontok és az Azure portál méretezési |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti a intenzív streamvégpontok az Azure portálon."
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
ms.openlocfilehash: 4bb891371e3fc802fa667688a88878db18e32422
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="scale-streaming-endpoints-with-the-azure-portal"></a><span data-ttu-id="4f081-103">A streamvégpont méretezése az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="4f081-103">Scale streaming endpoints with the Azure portal</span></span>
## <a name="overview"></a><span data-ttu-id="4f081-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4f081-104">Overview</span></span>

> [!NOTE]
> <span data-ttu-id="4f081-105">Az oktatóanyag elvégzéséhez egy Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="4f081-105">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="4f081-106">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4f081-106">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="4f081-107">A **prémium** szintű streamvégpontok a speciális feladatokhoz ideálisak, mert dedikált és méretezhető sávszélesség-kapacitást nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="4f081-107">**Premium** streaming endpoints are suitable for advanced workloads, providing dedicated and scalable bandwidth capacity.</span></span> <span data-ttu-id="4f081-108">A **prémium** streamvégponttal rendelkező ügyfelek alapértelmezés szerint kapnak egy adategységet (SU-t).</span><span class="sxs-lookup"><span data-stu-id="4f081-108">Customers that have a **Premium** streaming endpoint, by default get one streaming unit (SU).</span></span> <span data-ttu-id="4f081-109">A streamvégpont adategységek hozzáadásával méretezhető.</span><span class="sxs-lookup"><span data-stu-id="4f081-109">The streaming endpoint can be scaled by adding SUs.</span></span> <span data-ttu-id="4f081-110">Mindegyik adategység további sávszélesség-kapacitást nyújt az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="4f081-110">Each SU provides additional bandwidth capacity to the application.</span></span> <span data-ttu-id="4f081-111">Adatfolyam-típusú végpontok esetében és a CDN konfigurációs kapcsolatos további információkért tekintse meg a [Streaming Endpoint áttekintése](media-services-portal-manage-streaming-endpoints.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="4f081-111">For more information about streaming endpoint types and CDN configuration, see the [Streaming Endpoint overview](media-services-portal-manage-streaming-endpoints.md) topic.</span></span>
 
<span data-ttu-id="4f081-112">Ez a témakör bemutatja a streamvégpontján méretezése.</span><span class="sxs-lookup"><span data-stu-id="4f081-112">This topic shows how to scale a streaming endpoint.</span></span>

<span data-ttu-id="4f081-113">Az árképzésre vonatkozó további információkért lásd: [Media Services Pricing Details](http://go.microsoft.com/fwlink/?LinkId=275107) (A Media Services árképzésére vonatkozó információk).</span><span class="sxs-lookup"><span data-stu-id="4f081-113">For information about pricing details, see [Media Services Pricing Details](http://go.microsoft.com/fwlink/?LinkId=275107).</span></span>

## <a name="scale-streaming-endpoints"></a><span data-ttu-id="4f081-114">Az adatfolyam végpontjainak méretezése</span><span class="sxs-lookup"><span data-stu-id="4f081-114">Scale streaming endpoints</span></span>

<span data-ttu-id="4f081-115">A streamelési egységek számának megváltoztatásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4f081-115">To change the number of streaming units, do the following:</span></span>

1. <span data-ttu-id="4f081-116">Az [Azure-portálon](https://portal.azure.com/) válassza ki Azure Media Services-fiókját.</span><span class="sxs-lookup"><span data-stu-id="4f081-116">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="4f081-117">Az a **beállítások** ablakban válassza ki **adatfolyam-végpontok**.</span><span class="sxs-lookup"><span data-stu-id="4f081-117">In the **Settings** window, select **Streaming endpoints**.</span></span>
3. <span data-ttu-id="4f081-118">Kattintson a skálázni kívánt streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="4f081-118">Click on the streaming endpoint that you want to scale.</span></span> 

    [!NOTE] <span data-ttu-id="4f081-119">Csak méretezheti **prémium** adatfolyam-végpontok.</span><span class="sxs-lookup"><span data-stu-id="4f081-119">You can only scale **Premium** streaming endpoints.</span></span>

4. <span data-ttu-id="4f081-120">Mozgassa a csúszkát adja meg a streamelési egységek számát.</span><span class="sxs-lookup"><span data-stu-id="4f081-120">Move the slider to specify the number of streaming units.</span></span>

    ![Adatfolyam-továbbítási végpontra](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a><span data-ttu-id="4f081-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f081-122">Next steps</span></span>
<span data-ttu-id="4f081-123">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="4f081-123">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4f081-124">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="4f081-124">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

