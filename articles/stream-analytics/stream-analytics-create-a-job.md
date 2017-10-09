---
title: "egy analytics feldolgozási feladatot a Stream Analytics aaaHow toocreate |} Microsoft Docs"
description: "Hozzon létre egy analytics feldolgozási feladatot a Stream Analytics |} tanulási elérésiút-szegmens."
keywords: "az elemzés adatfeldolgozás"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: d4a3c89d8862d59688d06a1719b063efa2ab1c93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-data-analytics-processing-job-for-stream-analytics"></a>Hogyan toocreate analytics adatfeldolgozási feladat a Stream Analytics
legfelső szintű erőforrás hello Azure Stream Analytics egy Stream Analytics-feladat.  Olyan karakterekből áll, egy vagy több bemeneti adatforrások, egy lekérdezés kifejező hello adatok átalakítása és egy vagy több eredmények írt kimenetet célozza. Együtt hello felhasználói tooperform adatelemzés forgatókönyvek a streamelési adatok számára történő feldolgozásakor ezek engedélyezése.

használja a Stream Analytics toostart először hozzon létre egy új Stream Analytics-feladat.  Vegye figyelembe, hogy ez a művelet nem számlázási hatással van, amíg hello feladat elindult.

1. Jelentkezzen be a hello online [a klasszikus Azure portálon](http://manage.windowsazure.com) vagy hello [Azure-portálon](https://portal.azure.com/).
2. Hello portálon: **kattintson az új**, majd kattintson a **adatszolgáltatások** vagy **adatelemzés** attól függően, hogy a portálon, és kattintson **Azure Stream Analytics** vagy **Stream Analytics** , majd **Gyorslétrehozás**.
   
   ![Az elemzés adatfeldolgozás feladat varázsló](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![Hozzon létre a feldolgozás adatelemzés](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. Adja meg a Stream Analytics-feladat hello hello kívánt beállításait.
   
   * A hello **feladatnév** mezőbe írja be egy nevet tooidentify hello Stream Analytics-feladat. Ha hello **feladatnév** van hitelesítve, egy zöld pipa hello feladat neve mezőjében jelenik meg. Hello **feladatnév** neve csak alfanumerikus karaktereket és hello tartalmazhat "-" karakter, és 3 és 63 karakter közé kell esnie.
   * Használjon **régió** hello Azure-portálon a vagy **hely** a hello Azure portál toospecify hello földrajzi helyének toorun hello feladat.
   * Ha Azure-portál használatával hello, válasszon, vagy hozzon létre egy tárolási fiók toouse, hello **Monitoring Storage-fiók regionális**. Ezt a tárfiókot használt toostore figyelési adatok az összes Stream Analytics-feladatok futtatása az ebben a régióban.
   * Ha az Azure portál használatával hello, adjon meg egy új vagy meglévő **erőforráscsoport** toohold kapcsolódó erőforrások az alkalmazáshoz.
4. Hello új Stream Analytics-feladat beállítások konfigurálása után kattintson **Stream Analytics-feladat létrehozása**. Hello Stream Analytics-feladat toobe létrehozott néhány percet is igénybe vehet. toocheck hello állapotát, figyelheti a hello értesítésközpontról hello előrehaladását.
   
   ![Adatok analytics feldolgozási feladat értesítési központ](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Feldolgozása az Azure portál adatelemzés feladat-feladat létrehozása](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. hello új feladat állapota megjelenik **Created**. Figyelje meg, hogy hello **Start** gomb le van tiltva. Konfigurálja a hello feladat bemeneti, a lekérdezés és a kimeneti hello feladat indítása előtt.
   
   ![Az elemzés adatfeldolgozás feladat állapota](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Feladatállapot feldolgozása az Azure portál adatelemzés](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

