---
title: "a művelet és a szolgáltatás-naplók segítségével a Stream Analytics aaaDebug |} Microsoft Docs"
description: "Hogyan-toouse Stream Analytics műveleti naplói"
keywords: "Service naplóit"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: a2ed9676-f0bd-4398-87c8-a592779ac728
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: d3dd27706ccc879a724e1894b33d47021d972f31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Hibakeresés és üzemeltetése naplók segítségével Stream Analytics-feladatok
Az összes Azure-szolgáltatások ellátási működési naplózási üzenetek toousers toorecord részletek kapcsolatos toomanagement műveleteket. A Azure Stream Analytics ezt az információt hibakeresési célra például feladat állapotát, a feladatok előrehaladásának és a hiba üzenetek tootrack hello előrehaladását a feladatok megtekintése idővel start tooprocessing toooutput is használható.

## <a name="find-operation-logs-in-hello-azure-management-portal"></a>Műveletnaplókat hello Azure felügyeleti portálon található
A műveletnaplók kétféleképpen érhető el:  

* A Stream Analytics-feladat hello irányítópult  
* Felügyeleti szolgáltatások hello a klasszikus Azure portálon  

## <a name="dashboard-of-hello-stream-analytics-job"></a>A Stream Analytics-feladat hello irányítópult
A naplók a Stream Analytics-feladatok megfelelő hivatkozásra toohello hello feladat irányítópult lapon jelenik meg. Ha a hivatkozásra kattint, az hello szűrők állítja be oly módon, hogy azt mutatja, hogy adott feladat legfrissebb naplókat.

  ![Válassza ki a felügyeleti szolgáltatások naplók](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Felügyeleti szolgáltatások
toomanually toohello műveletnaplók lépjen a Stream Analytics-és egyéb szolgáltatások hello a klasszikus Azure portálon:

1. Kattintson a **szolgáltatások** a hello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Válassza ki **Stream Analytics** a **típus** és hello feladat neve hello **szolgáltatásnév**.  
   
   ![Válassza ki a Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-hello-azure-portal"></a>Naplók található hello Azure-portálon
a Stream Analytics-feladat hello Azure-portálon a műveleti naplókat toofind kattintson **Tallózás** , és válassza **naplók**.

  ![Válassza ki a Stream Analytics Azure-portálon](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Ekkor megnyílik egy panel megjelenítő hello származó események legutóbbi 7 nap minden erőforrás az előfizetéshez.  Adjon meg típusú vagy időkereten belül toosee események hello kattintva szűrheti **szűrő** parancsot.

  ![Válassza ki a Stream Analytics Azure-portálon](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Részletek a naplóban
Szűrhet időtartomány és állapot tooview hello naplók a feladathoz.

Hello Azure felügyeleti portálon, kattintson a hello **részletek** hello ablak tooview hello alján gombot a további információkat a kijelölt eseményre. 

  ![Válassza ki a részletei](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Hello az Azure portálon kattintson a napló bejegyzés toosee a hello részletes esemény azt.

  ![Azure-portálon válassza részletei](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Ott, nyissa meg hello **részletes** panelre. Ehhez kattintson a hello esemény.

  ![Azure-portálon válassza részletei](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Sikertelen feladat hibakeresése
Hello Azure felügyeleti portálon kattintson a hello keresés ikonra, és írja be a "sikertelen". Ez az összes naplók hibákkal eredményt ad. 

  ![Sikertelen feladat-hibakeresés](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

A hello Azure-portálon, az üzenet tooview szintje szerint szűrheti **kritikus** események.

  ![Az Azure portál hibakeresési](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Válassza ki a hello hibák, és kattintson a hello **részletek** hello a hibáról további információ.  Bizonyos hibaüzenetek is ismertetik, hogyan toomitigate hello probléma. 

  ![Művelet részletei](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

Abban az esetben szüksége toocontact [támogatási](https://azure.microsoft.com/support/options/) , vagy adjon meg információt toohello team hello keresztül [MSDN fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), vegye figyelembe hello működési részleteket, kifejezetten hello **.Korrelációazonosító:**. 

## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

