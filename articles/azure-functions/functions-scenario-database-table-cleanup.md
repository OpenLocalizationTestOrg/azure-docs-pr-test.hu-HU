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
# <a name="use-azure-functions-tooconnect-tooan-azure-sql-database"></a><span data-ttu-id="c53ca-103">Az Azure Functions tooconnect tooan Azure SQL Database használata</span><span class="sxs-lookup"><span data-stu-id="c53ca-103">Use Azure Functions tooconnect tooan Azure SQL Database</span></span>
<span data-ttu-id="c53ca-104">Ez a témakör bemutatja, hogyan toouse Azure Functions toocreate egy ütemezett feladat, amely egy Azure SQL-adatbázis egy tábla sorainak a szükségtelenné vált.</span><span class="sxs-lookup"><span data-stu-id="c53ca-104">This topic shows you how toouse Azure Functions toocreate a scheduled job that cleans up rows in a table in an Azure SQL Database.</span></span> <span data-ttu-id="c53ca-105">hello új C# függvény hello Azure-portálon előre definiált időzítő eseményindító sablont alapján jön létre.</span><span class="sxs-lookup"><span data-stu-id="c53ca-105">hello new C# function is created based on a pre-defined timer trigger template in hello Azure portal.</span></span> <span data-ttu-id="c53ca-106">toosupport ebben az esetben meg kell adnia egy adatbázis-kapcsolati karakterlánc hello függvény alkalmazásban beállításként.</span><span class="sxs-lookup"><span data-stu-id="c53ca-106">toosupport this scenario, you must also set a database connection string as a setting in hello function app.</span></span> <span data-ttu-id="c53ca-107">Ez a forgatókönyv hello adatbázison csoportos művelet használ.</span><span class="sxs-lookup"><span data-stu-id="c53ca-107">This scenario uses a bulk operation against hello database.</span></span> <span data-ttu-id="c53ca-108">toohave helyette használja a függvény folyamat egyes CRUD műveletek az egy Mobile Apps-táblázatot, [Mobile Apps kötések](functions-bindings-mobile-apps.md).</span><span class="sxs-lookup"><span data-stu-id="c53ca-108">toohave your function process individual CRUD operations in a Mobile Apps table, you should instead use [Mobile Apps bindings](functions-bindings-mobile-apps.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c53ca-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c53ca-109">Prerequisites</span></span>

+ <span data-ttu-id="c53ca-110">Ez a témakör egy időzítő indított függvényt használja.</span><span class="sxs-lookup"><span data-stu-id="c53ca-110">This topic uses a timer triggered function.</span></span> <span data-ttu-id="c53ca-111">Teljes hello hello témakör lépéseit [függvény létrehozása az Azure-időzítő által elindított](functions-create-scheduled-function.md) toocreate egy C# verziója ezt a funkciót.</span><span class="sxs-lookup"><span data-stu-id="c53ca-111">Complete hello steps in hello topic [Create a function in Azure that is triggered by a timer](functions-create-scheduled-function.md) toocreate a C# version of this function.</span></span>   

+ <span data-ttu-id="c53ca-112">Ebben a témakörben bemutatjuk a Transact-SQL-parancs, amely végrehajtja a tömeges törlésének művelete a hello **SalesOrderHeader** hello AdventureWorksLT minta adatbázis táblájában.</span><span class="sxs-lookup"><span data-stu-id="c53ca-112">This topic demonstrates a Transact-SQL command that executes a bulk cleanup operation in hello **SalesOrderHeader** table in hello AdventureWorksLT sample database.</span></span> <span data-ttu-id="c53ca-113">toocreate hello AdventureWorksLT mintaadatbázis hello témakör lépéseit teljes hello [hello Azure-portálon az Azure SQL-adatbázis létrehozása](../sql-database/sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c53ca-113">toocreate hello AdventureWorksLT sample database, complete hello steps in hello topic [Create an Azure SQL database in hello Azure portal](../sql-database/sql-database-get-started-portal.md).</span></span> 

## <a name="get-connection-information"></a><span data-ttu-id="c53ca-114">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="c53ca-114">Get connection information</span></span>

<span data-ttu-id="c53ca-115">Tooget hello kapcsolati karakterlánc szükséges befejezése létrehozott hello adatbázis [hello Azure-portálon az Azure SQL-adatbázis létrehozása](../sql-database/sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c53ca-115">You need tooget hello connection string for hello database you created when you completed [Create an Azure SQL database in hello Azure portal](../sql-database/sql-database-get-started-portal.md).</span></span>

1. <span data-ttu-id="c53ca-116">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c53ca-116">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
 
3. <span data-ttu-id="c53ca-117">Válassza ki **SQL-adatbázisok** hello bal oldali menüben válassza ki az adatbázist a hello **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="c53ca-117">Select **SQL Databases** from hello left-hand menu, and select your database on hello **SQL databases** page.</span></span>

4. <span data-ttu-id="c53ca-118">Válassza ki **adatbázis-kapcsolati karakterláncok megjelenítése** és másolási hello teljes **ADO.NET** kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="c53ca-118">Select **Show database connection strings** and copy hello complete **ADO.NET** connection string.</span></span>

    ![Másolja a hello ADO.NET kapcsolati karakterláncot.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-hello-connection-string"></a><span data-ttu-id="c53ca-120">Hello kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="c53ca-120">Set hello connection string</span></span> 

<span data-ttu-id="c53ca-121">Egy függvényalkalmazás hello a függvények végrehajtásához szükséges az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="c53ca-121">A function app hosts hello execution of your functions in Azure.</span></span> <span data-ttu-id="c53ca-122">A bevált gyakorlat toostore kapcsolati karakterláncok és más titkos adatokat az függvény alkalmazás beállításaiban.</span><span class="sxs-lookup"><span data-stu-id="c53ca-122">It is a best practice toostore connection strings and other secrets in your function app settings.</span></span> <span data-ttu-id="c53ca-123">Alkalmazásbeállítások használatával elkerülheti a véletlen közzétételének hello kapcsolati karakterláncot a kóddal.</span><span class="sxs-lookup"><span data-stu-id="c53ca-123">Using application settings prevents accidental disclosure of hello connection string with your code.</span></span> 

1. <span data-ttu-id="c53ca-124">Keresse meg a létrehozott tooyour függvény app [függvény létrehozása az Azure-időzítő által elindított](functions-create-scheduled-function.md).</span><span class="sxs-lookup"><span data-stu-id="c53ca-124">Navigate tooyour function app you created [Create a function in Azure that is triggered by a timer](functions-create-scheduled-function.md).</span></span>

2. <span data-ttu-id="c53ca-125">Válassza ki **Platform funkciói** > **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="c53ca-125">Select **Platform features** > **Application settings**.</span></span>
   
    ![Alkalmazásbeállítások hello függvény alkalmazás.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. <span data-ttu-id="c53ca-127">Görgessen lefelé, túl**kapcsolati karakterláncok** , és adja hozzá a hello beállításokkal hello táblázatban megadott kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="c53ca-127">Scroll down too**Connection strings** and add a connection string using hello settings as specified in hello table.</span></span>
   
    ![Adja hozzá a kapcsolati karakterlánc toohello függvény Alkalmazásbeállítások.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | <span data-ttu-id="c53ca-129">Beállítás</span><span class="sxs-lookup"><span data-stu-id="c53ca-129">Setting</span></span>       | <span data-ttu-id="c53ca-130">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="c53ca-130">Suggested value</span></span> | <span data-ttu-id="c53ca-131">Leírás</span><span class="sxs-lookup"><span data-stu-id="c53ca-131">Description</span></span>             | 
    | ------------ | ------------------ | --------------------- | 
    | <span data-ttu-id="c53ca-132">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="c53ca-132">**Name**</span></span>  |  <span data-ttu-id="c53ca-133">sqldb_connection</span><span class="sxs-lookup"><span data-stu-id="c53ca-133">sqldb_connection</span></span>  | <span data-ttu-id="c53ca-134">Használt tooaccess hello tárolt kapcsolati karakterlánc a funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="c53ca-134">Used tooaccess hello stored connection string in your function code.</span></span>    |
    | <span data-ttu-id="c53ca-135">**Érték**</span><span class="sxs-lookup"><span data-stu-id="c53ca-135">**Value**</span></span> | <span data-ttu-id="c53ca-136">Másolt karakterlánc</span><span class="sxs-lookup"><span data-stu-id="c53ca-136">Copied string</span></span>  | <span data-ttu-id="c53ca-137">Elmúlt hello kapcsolati karakterlánc másolt hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="c53ca-137">Past hello connection string you copied in hello previous section.</span></span> |
    | <span data-ttu-id="c53ca-138">**Típus**</span><span class="sxs-lookup"><span data-stu-id="c53ca-138">**Type**</span></span> | <span data-ttu-id="c53ca-139">SQL Database</span><span class="sxs-lookup"><span data-stu-id="c53ca-139">SQL Database</span></span> | <span data-ttu-id="c53ca-140">Hello alapértelmezett SQL-adatbázis-kapcsolat használata.</span><span class="sxs-lookup"><span data-stu-id="c53ca-140">Use hello default SQL Database connection.</span></span> |   

3. <span data-ttu-id="c53ca-141">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="c53ca-141">Click **Save**.</span></span>

<span data-ttu-id="c53ca-142">Most hello C# függvény kódot, amely a tooyour SQL-adatbázis is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="c53ca-142">Now, you can add hello C# function code that connects tooyour SQL Database.</span></span>

## <a name="update-your-function-code"></a><span data-ttu-id="c53ca-143">Frissítse a függvény kódot</span><span class="sxs-lookup"><span data-stu-id="c53ca-143">Update your function code</span></span>

1. <span data-ttu-id="c53ca-144">A függvény alkalmazásban hello időzítő-eseményindítóval aktivált függvény kiválasztása</span><span class="sxs-lookup"><span data-stu-id="c53ca-144">In your function app, select hello timer-triggered function.</span></span>
 
3. <span data-ttu-id="c53ca-145">Adja hozzá a következő szerelvények hivatkozásai hello meglévő funkciókódot hello tetején hello:</span><span class="sxs-lookup"><span data-stu-id="c53ca-145">Add hello following assembly references at hello top of hello existing function code:</span></span>

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```

3. <span data-ttu-id="c53ca-146">Adja hozzá a következő hello `using` utasítások toohello függvény:</span><span class="sxs-lookup"><span data-stu-id="c53ca-146">Add hello following `using` statements toohello function:</span></span>
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. <span data-ttu-id="c53ca-147">Lecseréli a meglévő hello **futtatása** függvényt a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="c53ca-147">Replace hello existing **Run** function with hello following code:</span></span>
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

    <span data-ttu-id="c53ca-148">Ez a minta parancs frissíti hello **állapot** oszlop hello szállítási dátum alapján.</span><span class="sxs-lookup"><span data-stu-id="c53ca-148">This sample command updates hello **Status** column based on hello ship date.</span></span> <span data-ttu-id="c53ca-149">Azt frissítenie kell a 32 sornyi adatot.</span><span class="sxs-lookup"><span data-stu-id="c53ca-149">It should update 32 rows of data.</span></span>

5. <span data-ttu-id="c53ca-150">Kattintson a **mentése**, figyelési hello **naplók** windows hello a következő függvény végrehajtása, majd jegyezze fel a hello frissített sorok száma hello **SalesOrderHeader** tábla.</span><span class="sxs-lookup"><span data-stu-id="c53ca-150">Click **Save**, watch hello **Logs** windows for hello next function execution, then note hello number of rows updated in hello **SalesOrderHeader** table.</span></span>

    ![Hello megjelenítheti.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a><span data-ttu-id="c53ca-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c53ca-152">Next steps</span></span>

<span data-ttu-id="c53ca-153">A következő megtudhatja, hogyan toouse Logic Apps toointegrate más szolgáltatásokkal együtt működik.</span><span class="sxs-lookup"><span data-stu-id="c53ca-153">Next, learn how toouse Functions with Logic Apps toointegrate with other services.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="c53ca-154">Hozzon létre egy függvényt, amely integrálható a Logic Apps</span><span class="sxs-lookup"><span data-stu-id="c53ca-154">Create a function that integrates with Logic Apps</span></span>](functions-twitter-email.md)

<span data-ttu-id="c53ca-155">Funkciók kapcsolatos további információkért tekintse meg a következő témakörök hello:</span><span class="sxs-lookup"><span data-stu-id="c53ca-155">For more information about Functions, see hello following topics:</span></span>

* [<span data-ttu-id="c53ca-156">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="c53ca-156">Azure Functions developer reference</span></span>](functions-reference.md)  
  <span data-ttu-id="c53ca-157">Programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="c53ca-157">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="c53ca-158">Az Azure Functions tesztelése</span><span class="sxs-lookup"><span data-stu-id="c53ca-158">Testing Azure Functions</span></span>](functions-test-a-function.md)  
  <span data-ttu-id="c53ca-159">Különböző függvénytesztelési eszközöket és technikákat ír le.</span><span class="sxs-lookup"><span data-stu-id="c53ca-159">Describes various tools and techniques for testing your functions.</span></span>  
