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
# <a name="how-tooscale-your-time-series-insights-environment"></a>Hogyan tooscale idő adatsorozat Insights környezetében

Ez az oktatóanyag ismerteti hogyan tooscale idő adatsorozat Insights környezetét.

> [!NOTE]
> Termékváltozat-típusok között felskálázott nem engedélyezett. Egy S1 Sku tartalmazó környezet nem alakítható át olyan S2 környezetet.

## <a name="s1-sku-ingress-rates-and-capacities"></a>S1 SKU érkező sebességét és a kapacitások

| S1 Termékváltozat-kapacitás | Érkező arány | Tárolókészlet teljes tárhelykapacitását
| --- | --- | --- |
| 1 | 1 GB (1 millió esemény) | Havonta 30 GB (30 millió esemény) |
| 10 | 10 GB-os (10 millió esemény) | Havonta 300 GB (300 millió esemény) |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU érkező sebességét és a kapacitások

| S2 Termékváltozat-kapacitás | Érkező arány | Tárolókészlet teljes tárhelykapacitását
| --- | --- | --- |
| 1 | 10 GB-os (10 millió esemény) | Havonta 300 GB (300 millió esemény) |
| 10 | 100 GB (100 millió esemény) | Havonta 3 TB (3 milliárd esemény) |

Kapacitások lineárisan lehessen méretezni, így a S1 sku 2 kapacitással rendelkező támogatja a 2 GB (2 millió) esemény / nap érkező sebessége és 60 GB (60 millió esemény).

## <a name="changing-hello-capacity-of-your-environment"></a>A környezet hello képességét módosítása

1. A hello Azure-portálon, válassza ki hello környezet amelynek kapacitása toochange szeretné.
1. A beállítások konfigurálásához kattintson.
1. Hello kapacitás csúszkát tooselect hello, amely megfelel az érkező díjszabás hello követelményei és a tárolási kapacitás használja.

## <a name="next-steps"></a>Következő lépések

* Győződjön meg arról, hogy új kapacitás hello elegendő sávszélesség-szabályozás tooprevent. További részletekért lásd: hello *a környezetében előfordulhat, hogy lehet első halmozódni* szakasz [Itt](time-series-insights-diagnose-and-solve-problems.md).
