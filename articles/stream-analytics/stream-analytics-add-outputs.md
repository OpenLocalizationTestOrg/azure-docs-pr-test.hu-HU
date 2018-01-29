---
title: "A Stream Analytics-feladatok kimenete adatok konfigurálása |} Microsoft Docs"
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
ms.openlocfilehash: 1ffa517469da1a8d79917b9747abc97ca3bef463
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>A Stream Analytics-feladatok kimenete adatok konfigurálása

Az Azure Stream Analytics-feladatok egy vagy több adatok kimenetek, amelyek határozza meg a kapcsolat egy meglévő adatokat a fogadó lehet csatlakoztatni. A Stream Analytics-feladat dolgozza fel, és átalakítja a bejövő adatok, az adatok a kimeneti eseményekben adatfolyam íródik a feladat kimenetére.

Stream Analytics-adatok kimenetek segítségével valós idejű irányítópultokat vagy riasztások, adatok mozgása munkafolyamatok indítására, vagy egyszerűen archiválja kötegelt feldolgozásra később. A Stream Analytics számos Azure-szolgáltatásokkal, itt részletesen ismertetett első osztályú integrációs rendelkezik.

A Stream Analytics-feladat kimenetnek hozzáadása:

1. Az a [Azure-portálon](https://portal.azure.com), nyissa meg a feladatot, és kattintson a **kimenetek** , majd **Hozzáadás** a kimenetek paneljén megjelenő.
   
    ![Kimenet hozzáadása](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
2. Adjon meg egy rövid nevet a kimeneti a **kimeneti Alias** mezőbe. Ez a név segítségével a feladat lekérdezésben később tekintse meg a kimeneti.  
   
    Töltse ki a többi csatlakozni a kimeneti kapcsolathoz szükséges tulajdonság.  Ezek a mezők kimeneti típus szerint eltérőek, és részletesen meghatározása.  
   
    ![A mozgás adattípus kiválasztása](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
3. Attól függően, hogy a kimeneti típus meg kell adnia az adatok szerializált vagy formázott módját. A megadott szerializálási beállításokat az egyes kimeneti Itt szerepelnek.
   
    Töltse ki a többi a szükséges csatlakozási tulajdonságokat az adatforráshoz való kapcsolódáshoz. Ezek a mezők bemeneti és a forrás típusa változhat, és részletesen meghatározása a [feladat létrehozása a cikk](stream-analytics-create-a-job.md).  

> [!Note]
>
> Bármely a feladatot, kimeneti eleme már léteznie kell a feladat elindult és események start továbbítására. Például ha egy kimeneti Blob-tároló használja, a feladat nem hoz létre egy tárfiókot automatikusan. A felhasználó által létrehozott, a ASA feladat végrehajtásának megkezdése előtt szükséges.
> 
 

## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [Az Azure Stream Analytics bemutatása](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

