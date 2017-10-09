---
title: "egy Azure idő adatsorozat Insights környezet aaaCreate |} Microsoft Docs"
description: "Ebből az oktatóanyagból megtudhatja, hogyan toocreate adatsorozat környezet, csatlakoztassa tooan eseményforrás, és a eseményadatok tooanalyze kész percben."
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.openlocfilehash: 7120fc9a6e4d4a4972f8cb37e4d9945cfb746fd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-new-time-series-insights-environment-in-hello-azure-portal"></a>Hozzon létre egy új idő adatsorozat Insights környezetben hello Azure-portálon

A Time Series Insights-környezet egy Azure-erőforrás bejövő adatforgalmi és tárolási kapacitással. Az ügyfelek hello szükséges kapacitással rendelkező környezetek hello Azure-portálon keresztül kiépítéséhez.

## <a name="steps-toocreate-hello-environment"></a>Lépéseket toocreate hello környezet

Kövesse ezeket a lépéseket toocreate a környezetben:

1.  Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2.  Kattintson a bal sarok hello felső hello plusz jel ("+").
3.  Keresse meg a "Idő adatsorozat Insights" hello Keresés mezőbe.

  ![Hello idő adatsorozat Insights környezet létrehozása](media/get-started/getstarted-create-environment1.png)

4.  Válassza a „Time Series Insights” elemet, és kattintson a „Létrehozás” lehetőségre.

  ![Hello idő adatsorozat Insights erőforráscsoport létrehozása](media/get-started/getstarted-create-environment2.png)

5.  Adja meg a környezet nevét. Ez a név jelöli hello környezet [idő adatsorozat explorer](https://insights.timeseries.azure.com).
6.  Válasszon egy előfizetést. Azt az előfizetést válassza, amely tartalmazza az eseményforrást. Idő adatsorozat Insights is automatikus észlelésű Azure IoT-központot, és a meglévő Eseményközpont erőforrások hello ugyanahhoz az előfizetéshez.
7.  Válasszon ki vagy hozzon létre egy erőforráscsoportot. Az erőforráscsoport olyan Azure-erőforrások gyűjteménye, amelyeket együtt használnak.
8.  Válasszon egy üzemeltetési helyet. tooavoid áthelyezése az adatok között adatközpontok, válassza ki az esemény forrását tartalmazó helyet.
9.  Válasszon tarifacsomagot.
10. Válassza ki a kapacitást. A környezet kapacitása a létrehozást követően módosítható.
11. Hozza létre a környezetet. Egyszerűen hozzáférhetnek a környezeti toohello irányítópult is rögzítheti, amikor bejelentkezik.

  ![Hello idő adatsorozat Insights PIN-kód toodashboard létrehozása](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a>Következő lépések

* [Adja meg az adat-hozzáférési házirendjeit](time-series-insights-data-access.md) tooaccess a környezet [idő adatsorozat Insights portálon találja meg](https://insights.timeseries.azure.com)
* [Eseményforrás létrehozása](time-series-insights-add-event-source.md)
* [Események küldése](time-series-insights-send-events.md) toohello eseményforrás
