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
# <a name="load-data-into-storage-environments-for-analytics"></a>Adatok betöltése a tárolási környezetekbe elemzés céljából
hello csapat az tudományos folyamata szükséges adatok keresztül a szervezetbe vagy egy másik tárolóhelyre környezetek toobe dolgozni, vagy az egyes fázisokban hello folyamat hello leginkább megfelelő módon elemzett számos betölti. Azure Blob Storage, SQL Azure-adatbázisok, SQL Server Azure virtuális Gépen, a HDInsight (Hadoop) és az Azure Machine Learning feldolgozásához gyakran használt adatok a célok közé 

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Ez **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan ezek adatok tooingest céloz környezetekben, ahol hello adatok tárolása és feldolgozása tootopics.

Műszaki és üzleti igényei, valamint hello kiindulási helyét, formázása, és az adatok mérete határozza meg, hogy mely hello az adatoknak kell okozhatnak toobe tooachieve hello célok elemzéshez hello környezetekben. A rendszer nem ritka, hogy a forgatókönyv toorequire adatok toobe több környezetek tooachieve hello számos feladatok szükséges tooconstruct prediktív modellek közötti áthelyezése. Ez a tevékenységsorozat tartalmazhatnak, például az adatok feltárása, az előzetes feldolgozás, a tisztítás, a régebbi-mintavételi és a modell betanítási.

