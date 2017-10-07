---
title: "aaaHow tooscale Azure idő adatsorozat Insights környezetét |} Microsoft Docs"
description: "Ez az oktatóanyag ismerteti hogyan tooscale Azure idő adatsorozat Insights környezetében"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: 55eda388997589185bd34228762b95e182b228ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-your-time-series-insights-environment"></a><span data-ttu-id="f0017-103">Hogyan tooscale idő adatsorozat Insights környezetében</span><span class="sxs-lookup"><span data-stu-id="f0017-103">How tooscale your Time Series Insights environment</span></span>

<span data-ttu-id="f0017-104">Ez az oktatóanyag ismerteti hogyan tooscale idő adatsorozat Insights környezetét.</span><span class="sxs-lookup"><span data-stu-id="f0017-104">This tutorial covers how tooscale your Time Series Insights environment.</span></span>

> [!NOTE]
> <span data-ttu-id="f0017-105">Termékváltozat-típusok között felskálázott nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="f0017-105">Scale up across sku types is not allowed.</span></span> <span data-ttu-id="f0017-106">Egy S1 Sku tartalmazó környezet nem alakítható át olyan S2 környezetet.</span><span class="sxs-lookup"><span data-stu-id="f0017-106">An environment with a S1 Sku cannot be converted into an S2 environment.</span></span>

## <a name="s1-sku-ingress-rates-and-capacities"></a><span data-ttu-id="f0017-107">S1 SKU érkező sebességét és a kapacitások</span><span class="sxs-lookup"><span data-stu-id="f0017-107">S1 SKU ingress rates and capacities</span></span>

| <span data-ttu-id="f0017-108">S1 Termékváltozat-kapacitás</span><span class="sxs-lookup"><span data-stu-id="f0017-108">S1 SKU Capacity</span></span> | <span data-ttu-id="f0017-109">Érkező arány</span><span class="sxs-lookup"><span data-stu-id="f0017-109">Ingress Rate</span></span> | <span data-ttu-id="f0017-110">Tárolókészlet teljes tárhelykapacitását</span><span class="sxs-lookup"><span data-stu-id="f0017-110">Maximum Storage Capacity</span></span>
| --- | --- | --- |
| <span data-ttu-id="f0017-111">1</span><span class="sxs-lookup"><span data-stu-id="f0017-111">1</span></span> | <span data-ttu-id="f0017-112">1 GB (1 millió esemény)</span><span class="sxs-lookup"><span data-stu-id="f0017-112">1 GB (1 million events)</span></span> | <span data-ttu-id="f0017-113">Havonta 30 GB (30 millió esemény)</span><span class="sxs-lookup"><span data-stu-id="f0017-113">30 GB (30 million events) per month</span></span> |
| <span data-ttu-id="f0017-114">10</span><span class="sxs-lookup"><span data-stu-id="f0017-114">10</span></span> | <span data-ttu-id="f0017-115">10 GB-os (10 millió esemény)</span><span class="sxs-lookup"><span data-stu-id="f0017-115">10 GB (10 million events)</span></span> | <span data-ttu-id="f0017-116">Havonta 300 GB (300 millió esemény)</span><span class="sxs-lookup"><span data-stu-id="f0017-116">300 GB (300 million events) per month</span></span> |

## <a name="s2-sku-ingress-rates-and-capacities"></a><span data-ttu-id="f0017-117">S2 SKU érkező sebességét és a kapacitások</span><span class="sxs-lookup"><span data-stu-id="f0017-117">S2 SKU ingress rates and capacities</span></span>

| <span data-ttu-id="f0017-118">S2 Termékváltozat-kapacitás</span><span class="sxs-lookup"><span data-stu-id="f0017-118">S2 SKU Capacity</span></span> | <span data-ttu-id="f0017-119">Érkező arány</span><span class="sxs-lookup"><span data-stu-id="f0017-119">Ingress Rate</span></span> | <span data-ttu-id="f0017-120">Tárolókészlet teljes tárhelykapacitását</span><span class="sxs-lookup"><span data-stu-id="f0017-120">Maximum Storage Capacity</span></span>
| --- | --- | --- |
| <span data-ttu-id="f0017-121">1</span><span class="sxs-lookup"><span data-stu-id="f0017-121">1</span></span> | <span data-ttu-id="f0017-122">10 GB-os (10 millió esemény)</span><span class="sxs-lookup"><span data-stu-id="f0017-122">10 GB (10 million events)</span></span> | <span data-ttu-id="f0017-123">Havonta 300 GB (300 millió esemény)</span><span class="sxs-lookup"><span data-stu-id="f0017-123">300 GB (300 million events) per month</span></span> |
| <span data-ttu-id="f0017-124">10</span><span class="sxs-lookup"><span data-stu-id="f0017-124">10</span></span> | <span data-ttu-id="f0017-125">100 GB (100 millió esemény)</span><span class="sxs-lookup"><span data-stu-id="f0017-125">100 GB (100 million events)</span></span> | <span data-ttu-id="f0017-126">Havonta 3 TB (3 milliárd esemény)</span><span class="sxs-lookup"><span data-stu-id="f0017-126">3 TB (3 billion events) per month</span></span> |

<span data-ttu-id="f0017-127">Kapacitások lineárisan lehessen méretezni, így a S1 sku 2 kapacitással rendelkező támogatja a 2 GB (2 millió) esemény / nap érkező sebessége és 60 GB (60 millió esemény).</span><span class="sxs-lookup"><span data-stu-id="f0017-127">Capacities scale linearly, so a S1 sku with capacity 2 supports 2 GB (2 million) events per day ingress rate and 60 GB (60 million events) per month.</span></span>

## <a name="changing-hello-capacity-of-your-environment"></a><span data-ttu-id="f0017-128">A környezet hello képességét módosítása</span><span class="sxs-lookup"><span data-stu-id="f0017-128">Changing hello capacity of your environment</span></span>

1. <span data-ttu-id="f0017-129">A hello Azure-portálon, válassza ki hello környezet amelynek kapacitása toochange szeretné.</span><span class="sxs-lookup"><span data-stu-id="f0017-129">In hello Azure portal, select hello environment whose capacity you want toochange.</span></span>
1. <span data-ttu-id="f0017-130">A beállítások konfigurálásához kattintson.</span><span class="sxs-lookup"><span data-stu-id="f0017-130">Under Settings, click Configure.</span></span>
1. <span data-ttu-id="f0017-131">Hello kapacitás csúszkát tooselect hello, amely megfelel az érkező díjszabás hello követelményei és a tárolási kapacitás használja.</span><span class="sxs-lookup"><span data-stu-id="f0017-131">Use hello Capacity slider tooselect hello capacity that meets hello requirements for your ingress rates and storage capacity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0017-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0017-132">Next steps</span></span>

* <span data-ttu-id="f0017-133">Győződjön meg arról, hogy új kapacitás hello elegendő sávszélesség-szabályozás tooprevent.</span><span class="sxs-lookup"><span data-stu-id="f0017-133">Verify that hello new capacity is sufficient tooprevent throttling.</span></span> <span data-ttu-id="f0017-134">További részletekért lásd: hello *a környezetében előfordulhat, hogy lehet első halmozódni* szakasz [Itt](time-series-insights-diagnose-and-solve-problems.md).</span><span class="sxs-lookup"><span data-stu-id="f0017-134">For more details, see hello *Your environment might be getting throttled* section [here](time-series-insights-diagnose-and-solve-problems.md).</span></span>
