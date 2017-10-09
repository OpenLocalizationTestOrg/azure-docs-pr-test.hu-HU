---
title: "az Azure storage analytics környezeteket aaaLoad adatok |} Microsoft Docs"
description: "Helyezze át az adatokat tooand Azure Blob Storage-ból"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 0fea2290991f9fa63d9e46c3a657000e27d95289
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-storage-environments-for-analytics"></a><span data-ttu-id="7eac9-103">Adatok betöltése a tárolási környezetekbe elemzés céljából</span><span class="sxs-lookup"><span data-stu-id="7eac9-103">Load data into storage environments for analytics</span></span>
<span data-ttu-id="7eac9-104">hello csapat az tudományos folyamata szükséges adatok keresztül a szervezetbe vagy egy másik tárolóhelyre környezetek toobe dolgozni, vagy az egyes fázisokban hello folyamat hello leginkább megfelelő módon elemzett számos betölti.</span><span class="sxs-lookup"><span data-stu-id="7eac9-104">hello Team Data Science Process requires that data be ingested or loaded into a variety of different storage environments toobe processed or analyzed in hello most appropriate way in each stage of hello process.</span></span> <span data-ttu-id="7eac9-105">Azure Blob Storage, SQL Azure-adatbázisok, SQL Server Azure virtuális Gépen, a HDInsight (Hadoop) és az Azure Machine Learning feldolgozásához gyakran használt adatok a célok közé</span><span class="sxs-lookup"><span data-stu-id="7eac9-105">Data destinations commonly used for processing include Azure Blob Storage, SQL Azure databases, SQL Server on Azure VM, HDInsight (Hadoop), and Azure Machine Learning.</span></span> 

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<span data-ttu-id="7eac9-106">Ez **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan ezek adatok tooingest céloz környezetekben, ahol hello adatok tárolása és feldolgozása tootopics.</span><span class="sxs-lookup"><span data-stu-id="7eac9-106">This **menu** links tootopics that describe how tooingest data into these target environments where hello data is stored and processed.</span></span>

<span data-ttu-id="7eac9-107">Műszaki és üzleti igényei, valamint hello kiindulási helyét, formázása, és az adatok mérete határozza meg, hogy mely hello az adatoknak kell okozhatnak toobe tooachieve hello célok elemzéshez hello környezetekben.</span><span class="sxs-lookup"><span data-stu-id="7eac9-107">Technical and business needs, as well as hello initial location, format and size of your data will determine hello target environments into which hello data needs toobe ingested tooachieve hello goals of your analysis.</span></span> <span data-ttu-id="7eac9-108">A rendszer nem ritka, hogy a forgatókönyv toorequire adatok toobe több környezetek tooachieve hello számos feladatok szükséges tooconstruct prediktív modellek közötti áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="7eac9-108">It is not uncommon for a scenario toorequire data toobe moved between several environments tooachieve hello variety of tasks required tooconstruct a predictive model.</span></span> <span data-ttu-id="7eac9-109">Ez a tevékenységsorozat tartalmazhatnak, például az adatok feltárása, az előzetes feldolgozás, a tisztítás, a régebbi-mintavételi és a modell betanítási.</span><span class="sxs-lookup"><span data-stu-id="7eac9-109">This sequence of tasks can include, for example, data exploration, pre-processing, cleaning, down-sampling, and model training.</span></span>

