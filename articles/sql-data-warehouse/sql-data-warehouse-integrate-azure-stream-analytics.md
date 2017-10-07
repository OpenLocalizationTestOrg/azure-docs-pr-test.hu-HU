---
title: "az SQL Data Warehouse szolgáltatással Azure Stream Analytics aaaUse |} Microsoft Docs"
description: "Tippek az Azure SQL Data Warehouse adattárházzal történő, megoldások Azure Stream Analytics használ."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 8aeb2247-20c5-4a29-b327-30a8ce09dfdc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1278197a6764864124fd92fc672de00b83ec343f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Az Azure Stream Analytics használja az SQL Data Warehouse szolgáltatással
Az Azure Stream Analytics egy teljes körűen felügyelt szolgáltatás biztosítása kis késleltetésű, magas rendelkezésre állású, méretezhető összetett eseményfeldolgozást keresztül adatfolyam hello felhőben. Elolvasásával megismerheti hello alapjai [Stream Analytics bemutatása tooAzure][Introduction tooAzure Stream Analytics]. Majd megtudhatja, hogyan toocreate egy végpont megoldás a Stream Analytics következő hello [Azure Stream Analytics használatának első] [ Get started using Azure Stream Analytics] oktatóanyag.

Ebből a cikkből megtudhatja, hogyan toouse az Azure SQL Data Warehouse-adatbázis szolgáltatást kimeneti fogadóként a gőz Analytics-feladatok.

## <a name="prerequisites"></a>Előfeltételek
Először futtassa a következő lépéseket a hello hello keresztül [Azure Stream Analytics használatának első] [ Get started using Azure Stream Analytics] oktatóanyag.  

1. Az Event Hubs bemeneti létrehozása
2. Konfigurálni és elindítani az esemény-készítő alkalmazás
3. A Stream Analytics-feladat kiépítése
4. Adja meg a feladat bemeneti és a lekérdezés

Ezután hozzon létre egy Azure SQL Data Warehouse-adatbázis

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Adja meg a feladat kimenetére: Azure SQL Data Warehouse-adatbázis
### <a name="step-1"></a>1. lépés
Kattintson a Stream Analytics-feladat **kimeneti** hello felső részén hello lapot, és kattintson a **hozzáadása kimeneti**.

### <a name="step-2"></a>2. lépés
Válassza ki az SQL-adatbázist, és kattintson a Tovább gombra.

![][add-output]

### <a name="step-3"></a>3. lépés
Adja meg a következő értékeket a következő oldalon hello hello:

* *A kimeneti Alias*: Adjon meg egy rövid nevet a feladat kimenete.
* *Előfizetés*:
  * Ha az SQL Data Warehouse-adatbázis hello ugyanahhoz az előfizetéshez hello Stream Analytics-feladat, mint a jelenlegi előfizetés válassza ki az SQL Database.
  * Ha az adatbázis egy másik előfizetésben, válassza ki az SQL Database egy másik előfizetésből.
* *Adatbázis*: Adja meg a célként megadott adatbázis hello nevét.
* *Kiszolgálónév*: Adja meg az imént megadott hello adatbázis hello kiszolgáló nevét. Ezzel hello klasszikus Azure portál toofind.

![][server-name]

* *Felhasználónév*: hello hello adatbázis írási engedéllyel rendelkező fiók felhasználónevét adja meg.
* *Jelszó*: Adja meg a hello hello jelszó megadott felhasználói fiók.
* *Tábla*: meg kell adnia hello adatbázis hello céltábla hello nevet.

![][add-database]

### <a name="step-4"></a>4. lépés
Ezt a feladatot, kimeneti és, hogy a Stream Analytics képes csatlakozni toohello adatbázis tooverify, kattintson a hello ellenőrzése gombra tooadd.

![][test-connection]

Ha hello kapcsolat toohello adatbázis sikeres, látni fogja a hello portal hello alján értesítést. Kapcsolat tesztelése hello alsó tootest hello kapcsolat toohello adatbázis elemre.

## <a name="next-steps"></a>Következő lépések
Az integráció áttekintéséért lásd: [SQL Data Warehouse integrációjának áttekintése][SQL Data Warehouse integration overview].

További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction tooAzure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
