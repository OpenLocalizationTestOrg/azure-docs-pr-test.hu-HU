---
title: "aaaAdd hivatkozás adatkészlet tooyour Azure idő adatsorozat Insights környezetben |} Microsoft Docs"
description: "Ebben az oktatóanyagban hivatkozás adatkészlet tooyour idő adatsorozat Insights környezet hozzáadása"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: venkatja
ms.openlocfilehash: 05e626ed81a22f2a8710b23a931ccd17c0f38ca5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-hello-ibiza-portal"></a>Hello Ibiza portálon idő adatsorozat Insights környezetnek hivatkozás adatok készlet létrehozása

A referencia-adatkészlet hello események az esemény forrását a rendszer kiegészítve elemek gyűjteménye. A Time Series Insights bejövő forgalmat kezelő motorja a referencia-adatkészlet egy elemét csatolja egy eseményforrásbeli eseményhez. Ez a kibővített esemény ezután lekérdezhető. Ezt az összekapcsolást hello kulcsok vannak meghatározva a a referencia-adatkészlet alapján történik.

## <a name="steps-tooadd-a-reference-data-set-tooyour-environment"></a>Lépéseket tooadd egy referencia-adatkészlet tooyour környezet

1. Jelentkezzen be toohello [Ibiza portálon](https://portal.azure.com).
2. Kattintson az "Összes erőforrás" hello menü hello hello Ibiza portálon a bal oldalán.
3. Válassza ki az Azure Time Series Insights-környezetet.

    ![Hozzon létre hello idő adatsorozat Insights referencia-adatkészlet](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. Válassza a „Referencia-adatkészletek” lehetőséget, majd kattintson a „+ Hozzáadás” gombra.

    ![Hozzon létre hello idő adatsorozat Insights referencia-adatkészlet - részletek](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. Adja meg a referencia-adatkészlet hello hello nevét.
6. Adja meg a hello kulcsnév és annak típusára. Ilyen nevű és típusú elem használt toopick hello megfelelő tulajdonság az eseményforrás hello eseménytől. Például ha ad meg, mint a "DeviceId" kulcs nevét és a "String" típusú, majd hello idő adatsorozat Insights érkező motor megkeresi a "DeviceId" típus "Karakterlánc" hello bejövő eseményben hello nevű tulajdonságot. Egynél több kulcsfontosságú toojoin hello esemény biztosíthat. hello tulajdonság nevének egyeznie kell az kis-és nagybetűket.

     ![Hozzon létre hello idő adatsorozat Insights referencia-adatkészlet - részletek](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. Kattintson a „Létrehozás” elemre.

## <a name="next-steps"></a>Következő lépések

* [Referencia-adatok kezelése](time-series-insights-manage-reference-data-csharp.md) programozott módon.
* Hello teljes API referenciáért lásd: [referencia az API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) dokumentum.
