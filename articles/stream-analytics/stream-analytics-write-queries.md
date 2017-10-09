---
title: "a Stream Analytics aaaHow toowrite lekérdezések |} Microsoft Docs"
description: "A Stream Analytics és adatait kérdezi le a lekérdezéseket írhat |} tanulási elérésiút-szegmens."
keywords: "Hogyan toowrite a lekérdezések adatait, a lekérdezés, lekérdezések írásáról írása"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0e9cdadd-0ee0-4bee-b65b-4a06fb863c95
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: b943c34f10afd2b21789afbd341c471a5f168729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toowrite-queries-in-stream-analytics"></a>Hogyan toowrite lekérdezi a Stream Analytics
A streamfeldolgozó logikákat, az Azure Stream Analytics lekérdezések írásáról valósul meg "állandó lekérdezés" előtt egy feladat elindul, és végre az adatok, mivel lehet spórolni a hello feladat van definiálva. hello van adatátalakítást, az SQL-szerű lekérdezésnyelvet, amely része a nagy mértékben T-SQL egy hozzáadott nyelvi fájlkiterjesztéseket – például [Ablakozó](https://msdn.microsoft.com/library/azure/dn835019.aspx) tooexpress historikus szemantikáját használja.

## <a name="writing-queries"></a>Lekérdezések írásáról:
1. A Stream Analytics-feladat hello Azure felügyeleti portálon, kattintson **lekérdezés**.
   
    ![Válassza ki a lekérdezés](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  
   
    Hello Azure portál, kattintson **lekérdezés**.
   
    ![Válassza ki a lekérdezés előnézeti](./media/stream-analytics-write-queries/query-preview-portal.png)  
2. Új feladatnak a kezdéshez lekérdezés sablon toohelp vannak. hello sablon hajtja végre a "csatlakoztatott" query adott projektek bemeneti események összes mezőjét hello kimeneti be.  
   
   * Ha legalább egy bemeneti és kimeneti meghatározta a feldolgozás, lecserélheti hello helyőrző "[YourOutputAlias]" és "[YourInputAlias]" mezőjének hello aliasok a hello bemeneti és kimeneti használata először kívánja. Ezenkívül is hozhatnak létre és a lekérdezés tesztelése a klasszikus Azure portál hello hello feladaton bemenetekhez és kimenetekhez definiálása nélkül.
   * Ha a tooperform mint egyszerű csatlakoztatott további feldolgozás, szerkesztheti hello lekérdezésdefiníciója. tooget használatába lekérdezés készítése, tekintse meg néhány gyakori lekérdezési minták a rendszer rögzíti [Itt](stream-analytics-stream-analytics-query-patterns.md).  
   
   ![Ablak adatait](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="toovalidate-query-data-is-working"></a>működik-e toovalidate adatait:
Tesztelheti, hogy a lekérdezés egy vagy több helyi JSON a fájlokat tartalmazó Tesztadatok hello böngészőben futó várt módon viselkedik-e. Ez nem fog elindulni hello feladat vagy számlázási hatással van.

> [!NOTE]
> Jelenleg a böngészőben a lekérdezéstesztelés nem támogatott hello Azure portálon.  
> 
> 

1. Győződjön meg arról, hogy nincsenek-e hibák hello lekérdezés (ellenkező esetben hello teszt gomb le lesz tiltva) majd hello tesztelése gombra.  
   
   ![Teszt adatait](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  
2. Az egyes hello lekérdezésben hivatkozott hello bemenetek felszólító toospecify fájlok lesznek. Ebben a példában hello sablon lekérdezés állapotban maradt-van, így a "yourinputalias" nevű bemeneti arra kéri a hello párbeszédpanel.  
   
   ![Adatok lekérdezés tesztelése](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  
3. Keresse meg a fájl tooa tesztelése. Több mintafájlt érhető el a [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) és a saját adatok adatfolyam bemenetei hello mintaadatok függvény hello bemenetek lapon keresztül is lekérhetik mintaadatok.  
   
   ![A bemeneti lekérdezés](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  
4. Hello párbeszédpanel bezárása, után keresztül hello Tesztadatok futtatni szeretné a lekérdezést, és látni fogja a hello eredmények alján hello hello lekérdezés.  
   
   ![Lekérdezés összegzése](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

