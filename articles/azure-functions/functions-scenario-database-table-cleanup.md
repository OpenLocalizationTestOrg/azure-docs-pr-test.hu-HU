---
title: "Az Azure Functions segítségével hajtsa végre a tisztítási feladat adatbázis |} Microsoft Docs"
description: "Az Azure Functions használatával, amely kapcsolódik az Azure SQL-adatbázis rendszeres időközönként sorait tisztítja meg a feladat ütemezését."
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
ms.openlocfilehash: 6fd0e32374827b249f5aba1cbfc39117c88c6272
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a><span data-ttu-id="d1be4-103">Használja az Azure Functions egy Azure SQL adatbázishoz való kapcsolódáshoz</span><span class="sxs-lookup"><span data-stu-id="d1be4-103">Use Azure Functions to connect to an Azure SQL Database</span></span>
<span data-ttu-id="d1be4-104">Ez a témakör bemutatja, hogyan hozzon létre egy ütemezett feladatot, amely egy Azure SQL-adatbázis egy tábla sorainak a szükségtelenné vált az Azure Functions használatával.</span><span class="sxs-lookup"><span data-stu-id="d1be4-104">This topic shows you how to use Azure Functions to create a scheduled job that cleans up rows in a table in an Azure SQL Database.</span></span> <span data-ttu-id="d1be4-105">Az új C# függvény az Azure-portálon előre definiált időzítő eseményindító sablon alapján jön létre.</span><span class="sxs-lookup"><span data-stu-id="d1be4-105">The new C# function is created based on a pre-defined timer trigger template in the Azure portal.</span></span> <span data-ttu-id="d1be4-106">Ez a forgatókönyv támogatása érdekében is meg kell adni egy adatbázis-kapcsolati karakterláncot a függvény alkalmazás-beállításként.</span><span class="sxs-lookup"><span data-stu-id="d1be4-106">To support this scenario, you must also set a database connection string as a setting in the function app.</span></span> <span data-ttu-id="d1be4-107">Ez a forgatókönyv használ egy tömeges művelet az adatbázison.</span><span class="sxs-lookup"><span data-stu-id="d1be4-107">This scenario uses a bulk operation against the database.</span></span> <span data-ttu-id="d1be4-108">Ahhoz, hogy a Mobile Apps-tábla egyedi CRUD műveletek feldolgozásához funkció Ehelyett használjon [Mobile Apps kötések](functions-bindings-mobile-apps.md).</span><span class="sxs-lookup"><span data-stu-id="d1be4-108">To have your function process individual CRUD operations in a Mobile Apps table, you should instead use [Mobile Apps bindings](functions-bindings-mobile-apps.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1be4-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d1be4-109">Prerequisites</span></span>

+ <span data-ttu-id="d1be4-110">Ez a témakör egy időzítő indított függvényt használja.</span><span class="sxs-lookup"><span data-stu-id="d1be4-110">This topic uses a timer triggered function.</span></span> <span data-ttu-id="d1be4-111">Hajtsa végre a következő témakör lépéseit [függvény létrehozása az Azure-időzítő által elindított](functions-create-scheduled-function.md) egy C# verzió függvény létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d1be4-111">Complete the steps in the topic [Create a function in Azure that is triggered by a timer](functions-create-scheduled-function.md) to create a C# version of this function.</span></span>   

+ <span data-ttu-id="d1be4-112">Ebben a témakörben bemutatjuk hajt végre egy tömeges törlésének művelete a Transact-SQL parancsot a **SalesOrderHeader** a AdventureWorksLT mintaadatbázis tábla.</span><span class="sxs-lookup"><span data-stu-id="d1be4-112">This topic demonstrates a Transact-SQL command that executes a bulk cleanup operation in the **SalesOrderHeader** table in the AdventureWorksLT sample database.</span></span> <span data-ttu-id="d1be4-113">A AdventureWorksLT mintaadatbázis létrehozásához hajtsa végre a témakör lépéseit [Azure SQL-adatbázis létrehozása az Azure portálon](../sql-database/sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d1be4-113">To create the AdventureWorksLT sample database, complete the steps in the topic [Create an Azure SQL database in the Azure portal](../sql-database/sql-database-get-started-portal.md).</span></span> 

## <a name="get-connection-information"></a><span data-ttu-id="d1be4-114">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="d1be4-114">Get connection information</span></span>

<span data-ttu-id="d1be4-115">Le kell töltenie a kapcsolati karakterláncot az adatbázishoz való befejezésekor létrehozott [Azure SQL-adatbázis létrehozása az Azure portálon](../sql-database/sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d1be4-115">You need to get the connection string for the database you created when you completed [Create an Azure SQL database in the Azure portal](../sql-database/sql-database-get-started-portal.md).</span></span>

1. <span data-ttu-id="d1be4-116">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d1be4-116">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
 
3. <span data-ttu-id="d1be4-117">Válassza ki **SQL-adatbázisok** a bal oldali menüben válassza ki az adatbázist a a **SQL-adatbázisok** lap.</span><span class="sxs-lookup"><span data-stu-id="d1be4-117">Select **SQL Databases** from the left-hand menu, and select your database on the **SQL databases** page.</span></span>

4. <span data-ttu-id="d1be4-118">Válassza ki **adatbázis-kapcsolati karakterláncok megjelenítése** , és másolja a teljes **ADO.NET** kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="d1be4-118">Select **Show database connection strings** and copy the complete **ADO.NET** connection string.</span></span>

    ![Másolja az ADO.NET kapcsolati karakterláncot.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a><span data-ttu-id="d1be4-120">A kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="d1be4-120">Set the connection string</span></span> 

<span data-ttu-id="d1be4-121">A függvények végrehajtásához szükséges gazdaszolgáltatást az Azure-ban egy függvényalkalmazás biztosítja.</span><span class="sxs-lookup"><span data-stu-id="d1be4-121">A function app hosts the execution of your functions in Azure.</span></span> <span data-ttu-id="d1be4-122">Ajánlott a kapcsolati karakterláncokat és más titkos adatokat tárolja a függvény Alkalmazásbeállítások.</span><span class="sxs-lookup"><span data-stu-id="d1be4-122">It is a best practice to store connection strings and other secrets in your function app settings.</span></span> <span data-ttu-id="d1be4-123">Alkalmazásbeállítások használatával elkerülheti a véletlen közzétételét a kód a kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="d1be4-123">Using application settings prevents accidental disclosure of the connection string with your code.</span></span> 

1. <span data-ttu-id="d1be4-124">Keresse meg a létrehozott függvény app [függvény létrehozása az Azure-időzítő által elindított](functions-create-scheduled-function.md).</span><span class="sxs-lookup"><span data-stu-id="d1be4-124">Navigate to your function app you created [Create a function in Azure that is triggered by a timer](functions-create-scheduled-function.md).</span></span>

2. <span data-ttu-id="d1be4-125">Válassza ki **Platform funkciói** > **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="d1be4-125">Select **Platform features** > **Application settings**.</span></span>
   
    ![A függvény alkalmazás Alkalmazásbeállítások.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. <span data-ttu-id="d1be4-127">Görgessen le a **kapcsolati karakterláncok** , és adja hozzá egy kapcsolati karakterláncot a táblázatban megadott beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="d1be4-127">Scroll down to **Connection strings** and add a connection string using the settings as specified in the table.</span></span>
   
    ![A kapcsolati karakterlánc hozzáadásának a függvény alkalmazás beállításai.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | <span data-ttu-id="d1be4-129">Beállítás</span><span class="sxs-lookup"><span data-stu-id="d1be4-129">Setting</span></span>       | <span data-ttu-id="d1be4-130">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="d1be4-130">Suggested value</span></span> | <span data-ttu-id="d1be4-131">Leírás</span><span class="sxs-lookup"><span data-stu-id="d1be4-131">Description</span></span>             | 
    | ------------ | ------------------ | --------------------- | 
    | <span data-ttu-id="d1be4-132">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="d1be4-132">**Name**</span></span>  |  <span data-ttu-id="d1be4-133">sqldb_connection</span><span class="sxs-lookup"><span data-stu-id="d1be4-133">sqldb_connection</span></span>  | <span data-ttu-id="d1be4-134">A tárolt kapcsolati karakterlánc, a függvény kódban elérésére használhatók.</span><span class="sxs-lookup"><span data-stu-id="d1be4-134">Used to access the stored connection string in your function code.</span></span>    |
    | <span data-ttu-id="d1be4-135">**Érték**</span><span class="sxs-lookup"><span data-stu-id="d1be4-135">**Value**</span></span> | <span data-ttu-id="d1be4-136">Másolt karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d1be4-136">Copied string</span></span>  | <span data-ttu-id="d1be4-137">A kapcsolati karakterlánc túli másolta az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="d1be4-137">Past the connection string you copied in the previous section.</span></span> |
    | <span data-ttu-id="d1be4-138">**Típus**</span><span class="sxs-lookup"><span data-stu-id="d1be4-138">**Type**</span></span> | <span data-ttu-id="d1be4-139">SQL Database</span><span class="sxs-lookup"><span data-stu-id="d1be4-139">SQL Database</span></span> | <span data-ttu-id="d1be4-140">Az alapértelmezett SQL-adatbázis-kapcsolat használata.</span><span class="sxs-lookup"><span data-stu-id="d1be4-140">Use the default SQL Database connection.</span></span> |   

3. <span data-ttu-id="d1be4-141">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d1be4-141">Click **Save**.</span></span>

<span data-ttu-id="d1be4-142">Most a C# függvény kódot, amely összeköti az SQL-adatbázis is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="d1be4-142">Now, you can add the C# function code that connects to your SQL Database.</span></span>

## <a name="update-your-function-code"></a><span data-ttu-id="d1be4-143">Frissítse a függvény kódot</span><span class="sxs-lookup"><span data-stu-id="d1be4-143">Update your function code</span></span>

1. <span data-ttu-id="d1be4-144">A függvény alkalmazást válassza ki a időzítő-eseményindítóval aktivált függvény.</span><span class="sxs-lookup"><span data-stu-id="d1be4-144">In your function app, select the timer-triggered function.</span></span>
 
3. <span data-ttu-id="d1be4-145">A meglévő funkciókódot tetején adja hozzá a következő szerelvény hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="d1be4-145">Add the following assembly references at the top of the existing function code:</span></span>

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```

3. <span data-ttu-id="d1be4-146">Adja hozzá a következő `using` utasítást, hogy a funkciót:</span><span class="sxs-lookup"><span data-stu-id="d1be4-146">Add the following `using` statements to the function:</span></span>
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. <span data-ttu-id="d1be4-147">Cserélje le a meglévő **futtatása** függvény a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="d1be4-147">Replace the existing **Run** function with the following code:</span></span>
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
                // Execute the command and log the # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.Info($"{rows} rows were updated");
            }
        }
    }
    ```

    <span data-ttu-id="d1be4-148">Ez a minta parancs frissíti a **állapot** oszlop a szállítási dátum alapján.</span><span class="sxs-lookup"><span data-stu-id="d1be4-148">This sample command updates the **Status** column based on the ship date.</span></span> <span data-ttu-id="d1be4-149">Azt frissítenie kell a 32 sornyi adatot.</span><span class="sxs-lookup"><span data-stu-id="d1be4-149">It should update 32 rows of data.</span></span>

5. <span data-ttu-id="d1be4-150">Kattintson a **mentése**, tekintse meg a **naplók** windows a következő függvény végrehajtása, majd jegyezze fel a frissített sorok száma a **SalesOrderHeader** tábla.</span><span class="sxs-lookup"><span data-stu-id="d1be4-150">Click **Save**, watch the **Logs** windows for the next function execution, then note the number of rows updated in the **SalesOrderHeader** table.</span></span>

    ![A függvény naplók megtekintéséhez.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a><span data-ttu-id="d1be4-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d1be4-152">Next steps</span></span>

<span data-ttu-id="d1be4-153">A következő további funkciók használata a Logic Apps a más szolgáltatásokkal való integrációjához.</span><span class="sxs-lookup"><span data-stu-id="d1be4-153">Next, learn how to use Functions with Logic Apps to integrate with other services.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="d1be4-154">Hozzon létre egy függvényt, amely integrálható a Logic Apps</span><span class="sxs-lookup"><span data-stu-id="d1be4-154">Create a function that integrates with Logic Apps</span></span>](functions-twitter-email.md)

<span data-ttu-id="d1be4-155">Funkciók kapcsolatos további információkért lásd a következő témaköröket:</span><span class="sxs-lookup"><span data-stu-id="d1be4-155">For more information about Functions, see the following topics:</span></span>

* [<span data-ttu-id="d1be4-156">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="d1be4-156">Azure Functions developer reference</span></span>](functions-reference.md)  
  <span data-ttu-id="d1be4-157">Programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="d1be4-157">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="d1be4-158">Az Azure Functions tesztelése</span><span class="sxs-lookup"><span data-stu-id="d1be4-158">Testing Azure Functions</span></span>](functions-test-a-function.md)  
  <span data-ttu-id="d1be4-159">Különböző függvénytesztelési eszközöket és technikákat ír le.</span><span class="sxs-lookup"><span data-stu-id="d1be4-159">Describes various tools and techniques for testing your functions.</span></span>  
