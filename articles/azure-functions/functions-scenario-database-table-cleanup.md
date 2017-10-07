---
title: "aaaUse Azure Functions tooperform egy adatbázis karbantartási feladat |} Microsoft Docs"
description: "Használja az Azure Functions tooschedule egy feladatot, amely a tooAzure SQL-adatbázis tooperiodically sorait tisztítja meg."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 076f5f95-f8d2-42c7-b7fd-6798856ba0bb
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/22/2017
ms.author: glenga
ms.openlocfilehash: 063a25fe8d14a75d54e9b72cec9fc1e25fa3ff44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-functions-tooconnect-tooan-azure-sql-database"></a>Az Azure Functions tooconnect tooan Azure SQL Database használata
Ez a témakör bemutatja, hogyan toouse Azure Functions toocreate egy ütemezett feladat, amely egy Azure SQL-adatbázis egy tábla sorainak a szükségtelenné vált. hello új C# függvény hello Azure-portálon előre definiált időzítő eseményindító sablont alapján jön létre. toosupport ebben az esetben meg kell adnia egy adatbázis-kapcsolati karakterlánc hello függvény alkalmazásban beállításként. Ez a forgatókönyv hello adatbázison csoportos művelet használ. toohave helyette használja a függvény folyamat egyes CRUD műveletek az egy Mobile Apps-táblázatot, [Mobile Apps kötések](functions-bindings-mobile-apps.md).

## <a name="prerequisites"></a>Előfeltételek

+ Ez a témakör egy időzítő indított függvényt használja. Teljes hello hello témakör lépéseit [függvény létrehozása az Azure-időzítő által elindított](functions-create-scheduled-function.md) toocreate egy C# verziója ezt a funkciót.   

+ Ebben a témakörben bemutatjuk a Transact-SQL-parancs, amely végrehajtja a tömeges törlésének művelete a hello **SalesOrderHeader** hello AdventureWorksLT minta adatbázis táblájában. toocreate hello AdventureWorksLT mintaadatbázis hello témakör lépéseit teljes hello [hello Azure-portálon az Azure SQL-adatbázis létrehozása](../sql-database/sql-database-get-started-portal.md). 

## <a name="get-connection-information"></a>Kapcsolatadatok lekérése

Tooget hello kapcsolati karakterlánc szükséges befejezése létrehozott hello adatbázis [hello Azure-portálon az Azure SQL-adatbázis létrehozása](../sql-database/sql-database-get-started-portal.md).

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
 
3. Válassza ki **SQL-adatbázisok** hello bal oldali menüben válassza ki az adatbázist a hello **SQL-adatbázisok** lap.

4. Válassza ki **adatbázis-kapcsolati karakterláncok megjelenítése** és másolási hello teljes **ADO.NET** kapcsolati karakterláncot.

    ![Másolja a hello ADO.NET kapcsolati karakterláncot.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-hello-connection-string"></a>Hello kapcsolati karakterlánc beállítása 

Egy függvényalkalmazás hello a függvények végrehajtásához szükséges az Azure-ban. A bevált gyakorlat toostore kapcsolati karakterláncok és más titkos adatokat az függvény alkalmazás beállításaiban. Alkalmazásbeállítások használatával elkerülheti a véletlen közzétételének hello kapcsolati karakterláncot a kóddal. 

1. Keresse meg a létrehozott tooyour függvény app [függvény létrehozása az Azure-időzítő által elindított](functions-create-scheduled-function.md).

2. Válassza ki **Platform funkciói** > **Alkalmazásbeállítások**.
   
    ![Alkalmazásbeállítások hello függvény alkalmazás.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. Görgessen lefelé, túl**kapcsolati karakterláncok** , és adja hozzá a hello beállításokkal hello táblázatban megadott kapcsolati karakterlánc.
   
    ![Adja hozzá a kapcsolati karakterlánc toohello függvény Alkalmazásbeállítások.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | Beállítás       | Ajánlott érték | Leírás             | 
    | ------------ | ------------------ | --------------------- | 
    | **Name (Név)**  |  sqldb_connection  | Használt tooaccess hello tárolt kapcsolati karakterlánc a funkciókódot.    |
    | **Érték** | Másolt karakterlánc  | Elmúlt hello kapcsolati karakterlánc másolt hello előző szakaszban. |
    | **Típus** | SQL Database | Hello alapértelmezett SQL-adatbázis-kapcsolat használata. |   

3. Kattintson a **Save** (Mentés) gombra.

Most hello C# függvény kódot, amely a tooyour SQL-adatbázis is hozzáadhat.

## <a name="update-your-function-code"></a>Frissítse a függvény kódot

1. A függvény alkalmazásban hello időzítő-eseményindítóval aktivált függvény kiválasztása
 
3. Adja hozzá a következő szerelvények hivatkozásai hello meglévő funkciókódot hello tetején hello:

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```

3. Adja hozzá a következő hello `using` utasítások toohello függvény:
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. Lecseréli a meglévő hello **futtatása** függvényt a következő kód hello:
    ```cs
    public static async Task Run(TimerInfo myTimer, TraceWriter log)
    {
        var str = ConfigurationManager.ConnectionStrings["sqldb_connection"].ConnectionString;
        using (SqlConnection conn = new SqlConnection(str))
        {
            conn.Open();
            var text = "UPDATE SalesLT.SalesOrderHeader " + 
                    "SET [Status] = 5  WHERE ShipDate < GetDate();";

            using (SqlCommand cmd = new SqlCommand(text, conn))
            {
                // Execute hello command and log hello # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.Info($"{rows} rows were updated");
            }
        }
    }
    ```

    Ez a minta parancs frissíti hello **állapot** oszlop hello szállítási dátum alapján. Azt frissítenie kell a 32 sornyi adatot.

5. Kattintson a **mentése**, figyelési hello **naplók** windows hello a következő függvény végrehajtása, majd jegyezze fel a hello frissített sorok száma hello **SalesOrderHeader** tábla.

    ![Hello megjelenítheti.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a>Következő lépések

A következő megtudhatja, hogyan toouse Logic Apps toointegrate más szolgáltatásokkal együtt működik.

> [!div class="nextstepaction"] 
> [Hozzon létre egy függvényt, amely integrálható a Logic Apps](functions-twitter-email.md)

Funkciók kapcsolatos további információkért tekintse meg a következő témakörök hello:

* [Az Azure Functions fejlesztői segédanyagai](functions-reference.md)  
  Programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.
* [Az Azure Functions tesztelése](functions-test-a-function.md)  
  Különböző függvénytesztelési eszközöket és technikákat ír le.  
