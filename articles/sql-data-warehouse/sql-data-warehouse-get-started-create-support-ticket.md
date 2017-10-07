---
title: "az SQL Data Warehouse egy támogatási jegy aaaHow toocreate |} Microsoft Docs"
description: "Hogyan toocreate támogatási jegyet az Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: a91d1f53-03cb-464b-9d5b-4a9c1a194ed3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 72f7eac82112fb7f1bfb05abca4ce40aeb3c828c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-support-ticket-for-sql-data-warehouse"></a>Hogyan toocreate támogatási jegyet az SQL Data Warehouse
Ha bármilyen probléma merül fel az SQL Data Warehouses-szal kapcsolatban, hozzon létre egy támogatási jegyet, hogy mérnöki csapatunk a segítségére lehessen.

> [!NOTE] 
> Azure-portálon hello hello erőforrás állapot-ellenőrzéssel nincs pontos 12/20/2016. Folyamatosan dolgozunk a toofix probléma. 


## <a name="create-a-support-ticket"></a>Támogatási jegy létrehozása
1. Nyissa meg hello [Azure-portálon][Azure portal].
2. Hello kezdőképernyőn kattintson hello **súgó + támogatás** csempére.
   
    ![Súgó és támogatás](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. A hello Súgó és támogatás panelen, kattintson **támogatási kérelem létrehozása**.
   
    ![Új támogatási kérelem](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. Jelölje be hello **kérelem típus**.
   
    ![Kérelemtípus](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > Alapértelmezés szerint minden SQL-kiszolgáló (például a myserver.database.windows.net) rendelkezik egy 45 000 egységnyi **DTU-kvótával**. Ez a kvóta egyszerűen egy biztonsági korlát. A támogatási jegy létrehozása, majd válassza a kvóta növelhető *kvóta* hello kérelem típusa. a DTU, meg kell szorozni toocalculate 7.5 hello teljes hello által [DWU] [ DWU] szükséges. Például azt szeretné, a két toohost egyetlen SQL Servert, majd DW6000s igényeljen egy 90,000 a DTU-kvótát.  Az aktuális DTU-használat hello SQL server panelen hello portálon tekintheti meg. Felfüggesztett, mind a nem felfüggesztett adatbázisok száma felé hello DTU-kvótát. 
   > 
   > 
5. Jelölje be hello **előfizetés** , hogy a gazdagépek hello hello hiba az adatbázisban.
   
    ![Előfizetés](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. Válassza ki **SQL Data Warehouse** , hello erőforrás.
   
    ![Erőforrás](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. Jelölje ki az [Azure támogatási csomagot][Azure support plan].
   
   * A **számlázással, kvótával és az előfizetés kezelésével** kapcsolatos támogatás minden támogatási szinten elérhető.
   * A **javítás/csere** támogatás a [Fejlesztői][Developer], a [Standard][Standard], a [Közvetlen professzionális támogatás][Professional Direct] vagy a [Premier szintű támogatás][Premier] esetén érhető el. Javítás / Csere problémák az Azure használata során az ügyfelek által tapasztalt problémák esetén egy ésszerű elvárásokat adott okozott Microsoft hello problémát.
   * **Fejlesztői mentorálás** és **tanácsadási szolgáltatás** érhetők el hello [közvetlen professzionális] [ Professional Direct] és [Premier] [ Premier] szintek. 
     
     Ha a Premier szintű támogatás megléte, az SQL Data Warehouse is jelentheti kapcsolatos hiba lépett fel a hello [Microsoft Premier online portálon][Microsoft Premier online portal].  Lásd: [Azure-támogatás ügyfeleknek] [ Azure support plan] további információk a különböző támogatja a csomagokat, beleértve azok hatókörét, a válaszidőt, díjszabást hello toolearn stb.  Az Azure-támogatással kapcsolatos gyakori kérdéseket az [Azure-támogatás – gyakori kérdések][Azure support FAQs] című témakör tekinti át.  
     
     ![Támogatási csomag](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. Jelölje be hello **problématípust** és **kategória**. Ebben a példában azt választotta, "Eszközök" hello probléma típusa és "Client tools" hello kategória szerint. 
   
    ![Probléma típusa és kategóriája](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. Hello probléma leíró, és válassza ki azt az üzletmenetre gyakorolt hatás hello szintet.
   
    ![A probléma leírása](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. A támogatási jegyen előre ki lesznek töltve a **kapcsolattartási adatok**. Ha szükséges, pontosítsa azokat.
    
    ![Kapcsolattartási adatok](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. Kattintson a **létrehozása** toosubmit hello támogatási kérelmet.

## <a name="monitor-a-support-ticket"></a>Támogatási jegy nyomon követése
Miután hello támogatási kérelmet elküldte, hello Azure támogatási csapata kapcsolatba lép Önnel. toocheck a kérelem állapotának és részleteinek, kattintson a **támogatási kérelmek kezelése** hello irányítópulton.

![Állapot ellenőrzése](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Egyéb források
Ezenkívül kapcsolatba léphet az SQL Data Warehouse-Közösséggel hello a [Stack Overflow] [ Stack Overflow] vagy hello [Azure SQL Data Warehouse MSDN fórumon] [ Azure SQL Data Warehouse MSDN forum].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Azure support plan]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure support FAQs]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online portal]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

