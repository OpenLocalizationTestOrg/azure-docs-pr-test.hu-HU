---
title: az adatok aaaAdd bemeneti tooyour Stream Analytics-feladatok |} Microsoft Docs
description: "Ismerje meg, hogyan mentése a data source tooyour Stream Analytics toohook feladat Blog tárolási adatokat az Event Hubs vagy hivatkozás a streamelési adatok bemenetének."
keywords: adatfolyam-adatok, bemeneti adatokat
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: 
ms.assetid: 9e59bd24-2a80-4ecb-b6b2-309a07c70bcd
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 674000268fcdf9bc000af3e2f166cb66f1366922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-streaming-data-input-or-reference-data-tooa-stream-analytics-job"></a>A streamelési adatok beviteli vagy hivatkozás adatok tooa Stream Analytics-feladat hozzáadása
Ismerje meg, hogyan mentése a data source tooyour Stream Analytics toohook feladat Event Hubs vagy hivatkozás adatokat a Blob storage a streamelési adatok bemenetének.

Az Azure Stream Analytics-feladatok csatlakoztatott tooone adatbevitel vagy több, amelyek mindegyike definiáljon kapcsolati tooan meglévő lehet. Toothat adatforrás adatokat küldi el, mert hello Stream Analytics-feladat által felhasznált és valós időben adatfolyam feldolgozása. A Stream Analytics első osztályú integrálva van [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) és [Azure Blob Storage tárolóban](../storage/blobs/storage-dotnet-how-to-use-blobs.md) belül és kívül hello feladat előfizetés.

Ez a cikk Ez hello lépés [Stream Analytics képzési terv](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Bemenetek: adatok és referenciaadatok
Két különböző típusú bemenetében benne a Stream Analytics: adatfolyamokat és a referenciaadatoknál.

* **Az adatfolyamok**: Stream Analytics-feladatok tartalmaznia kell legalább egy adatok adatfolyam bemeneti toobe fel, és át legyenek-e hello feladat által. Az Azure Blob storage és az Azure Event Hubs adatforrások adatfolyam bemeneti is támogatottak. Az Azure Event Hubs használt toocollect eseményfolyamokat csatlakoztatott eszközök, szolgáltatások és alkalmazások. Az Azure Blob storage eseményközpontokból tömeges adatfolyamként egy bemeneti forrásaként használható.  
* **Referenciaadatok**: a Stream Analytics támogatja a második típusú bemeneti hívott hivatkozást kiegészítő adatok.  Ezek az adatok mozgó megakadályozását toodata lesz statikus vagy lelassulnak, ami módosítása.  Általában használatos look-ups és adatok adatfolyamok toocreate adatok csoportadatokra korrelációt végrehajtásához.  Az Azure Blob storage jelenleg csak a támogatott hello bemeneti forrás a referenciaadatoknál.  

egy bemeneti tooyour Stream Analytics-feladat tooadd:

1. Hello Azure-portálon kattintson **bemenetek** majd **vegye fel a bemeneti** a Stream Analytics-feladat.
   
    ![Klasszikus Azure portál – bemeneti hozzáadása.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  
   
    Hello Azure-portálon kattintson hello **bemenetek** csempére a Stream Analytics-feladat.  
   
    ![Azure portál – vegye fel a bemeneti adatok.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  
2. Adja meg a hello bemenet hello típusát: vagy **adatfolyam** vagy **referenciaadatok**.
   
    ![Hello helyes adatokat adjon meg, folyamatos átviteli vagy hivatkozás hozzáadása](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  
   
    ![Hello helyes adatokat adjon meg, folyamatos átviteli vagy hivatkozás hozzáadása](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  
3. Egy adatfolyam-bemenet létrehozásakor, adja meg a hello bemeneti hello forrástípusát.  Ez a lépés is kimarad, csak a Blob storage jelenleg támogatott referenciaadatok létrehozása során.
   
    ![Adatfolyam-bemenetre adatok hozzáadása](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  
   
    ![Adatfolyam-bemenetre adatok hozzáadása](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  
4. Adja meg a bemeneti Alias mezőbe a bemeneti hello rövid nevét.  Ez a név fog települni a feladat lekérdezése később toorefer toohello bemeneti.
   
    Töltse ki hello többi hello szükséges kapcsolati tulajdonságok tooconnect tooyour adatforrás. Ezek a mezők bemeneti és a forrás típusa változhat, és részletesen meghatározása [Itt](stream-analytics-create-a-job.md).  
   
    ![Event hub bemeneti hozzáadása](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  
5. Adja meg a bemeneti adatok hello hello szerializálási beállításait:
   
   * toomake meg arról, hogy a lekérdezések használhatók hello megfelelően, adja meg a hello **esemény szerializálási formátum** a bejövő adatok.  Támogatott szerializálási formátumok a következők: JSON, CSV és az Avro.
   * Ellenőrizze a hello **kódolás** hello adatok.  Az UTF-8 hello csak akkor támogatja a kódolási formátum jelenleg.
     
     ![Hello bemeneti adatok szerializálási beállításai](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  
     
     ![Hello bemeneti adatok szerializálási beállításai](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  
6. Hello bemeneti létrehozásának befejezése után a Stream Analytics ellenőrzi, hogy csatlakozni toohello bemeneti forrás.  Megtekintheti a kapcsolat tesztelése művelet hello hello állapotának hello értesítési központban.
   
    ![A bemeneti adatfolyam hello kapcsolat tesztelése](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  
   
    ![A bemeneti adatfolyam hello kapcsolat tesztelése](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Segítség az adatok bemeneti adatfolyam
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

