---
title: a Stream Analytics-feladatok kimenete aaaHow tooconfigure adatok |} Microsoft Docs
description: "A Stream Analytics-feladatok kimenetének konfigurálása |} tanulási elérésiút-szegmens."
keywords: "kimeneti, adatok adatátvitel"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 3bbea3da-bfce-4af1-a15e-d4b23874034f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/26/2017
ms.author: samacha
ms.openlocfilehash: c5d89e9e9f9211d3e778580c071dd53d56aed9fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-data-outputs-for-stream-analytics-jobs"></a>Hogyan tooconfigure data Stream Analytics-feladatok kimenete

Az Azure Stream Analytics-feladatok csatlakoztatott tooone vagy további adatok kimenetek, amelyek meghatározzák egy kapcsolat tooan meglévő adatokat fogadó lehet. A Stream Analytics-feladat dolgozza fel, és átalakítja a bejövő adatok, az adatok a kimeneti eseményekben adatfolyam tooyour feladat kimenetére írása.

Stream Analytics adatok kimenetek használt toosource valós idejű irányítópultokat vagy riasztások, eseményindító adatok mozgása munkafolyamatok vagy egyszerűen archív adatok kötegelt feldolgozásra későbbi lehet. A Stream Analytics számos Azure-szolgáltatásokkal, itt részletesen ismertetett első osztályú integrációs rendelkezik.

tooadd egy kimeneti tooyour Stream Analytics-feladatot:

1. A hello [Azure-portálon](https://portal.azure.com), nyissa meg a feladatot, és kattintson a **kimenetek** , majd **Hozzáadás** hello kimenetek panelen megjelenő.
   
    ![Kimenet hozzáadása](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
2. Adjon meg egy rövid nevet a kimeneti hello **kimeneti Alias** mezőbe. Ez a név később a toorefer toohello kimenetet a feldolgozás lekérdezés használható.  
   
    Töltse ki a szükséges hello kapcsolati tulajdonságok tooconnect tooyour kimenet hello többi.  Ezek a mezők kimeneti típus szerint eltérőek, és részletesen meghatározása.  
   
    ![A mozgás adattípus kiválasztása](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
3. Hello kimeneti típusától függően szükség lehet a toospecify hogyan hello adatok szerializált vagy formázva. hello megadott szerializálási egyes beállításainak kimeneti Itt szerepelnek.
   
    Töltse ki hello többi hello szükséges kapcsolati tulajdonságok tooconnect tooyour adatforrás. Ezek a mezők bemeneti és a forrás típusa változhat, és részletes hello meghatározása [feladat létrehozása a cikk](stream-analytics-create-a-job.md).  

> [!Note]
>
> Minden kimeneti elem hozzáadott toohello feladat már léteznie kell hello feladat elindult és események start továbbítására. Például ha egy kimeneti Blob-tároló használja, hello feladat nem hoz létre egy tárfiókot automatikusan. Toobe hello felhasználó által létrehozott hello ASA feladat végrehajtásának megkezdése előtt szükséges.
> 
 

## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

