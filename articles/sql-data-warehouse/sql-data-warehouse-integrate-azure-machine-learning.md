---
title: "az SQL Data Warehouse szolgáltatással Azure Machine Learning aaaUse |} Microsoft Docs"
description: "Oktatóanyag az Azure Machine Learning Azure SQL Data Warehouse adattárházzal történő, megoldások fejlesztésére irányuló használatához."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: barbkess
editor: 
ms.assetid: ac6bc731-6add-47a9-b3fe-68996e656f4d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: fdfe8c936d2bb7a02163a0bbf6435e1ebd518d4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a><span data-ttu-id="d482c-103">Az Azure gépi tanulás használja az SQL Data Warehouse szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="d482c-103">Use Azure Machine Learning with SQL Data Warehouse</span></span>
<span data-ttu-id="d482c-104">Az Azure Machine Learning egy olyan teljes körűen felügyelt prediktív elemzési szolgáltatás is használja toocreate prediktív modelleket SQL-adatraktár adatai alapján, és tegye közzé felhasználásához felhasználásra kész webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="d482c-104">Azure Machine Learning is a fully managed predictive analytics service that you can use toocreate predictive models against your data in SQL Data Warehouse, and then publish as ready-to-consume web services.</span></span> <span data-ttu-id="d482c-105">Ismerje meg, a prediktív elemzés hello alapjait és a gépi tanulás olvasásával [bemutatása tooMachine Azure tanulási][Introduction tooMachine Learning on Azure].</span><span class="sxs-lookup"><span data-stu-id="d482c-105">You can learn hello basics of predictive analytics and machine learning by reading [Introduction tooMachine Learning on Azure][Introduction tooMachine Learning on Azure].</span></span>  <span data-ttu-id="d482c-106">Majd megtudhatja hogyan toocreate, betanítását, pontozása és tesztelése a gépi tanulási modell használatával hello [létrehozás kísérlet oktatóanyag][Create experiment tutorial].</span><span class="sxs-lookup"><span data-stu-id="d482c-106">You can then learn how toocreate, train, score and test a machine learning model using hello [Create experiment tutorial][Create experiment tutorial].</span></span>

<span data-ttu-id="d482c-107">Ebből a cikkből megtudhatja, hogyan hello segítségével a következő toodo hello [Azure Machine Learning Studio][Azure Machine Learning Studio]:</span><span class="sxs-lookup"><span data-stu-id="d482c-107">In this article, you will learn how toodo hello following using hello [Azure Machine Learning Studio][Azure Machine Learning Studio]:</span></span>

* <span data-ttu-id="d482c-108">Adatokat olvasni az adatbázis toocreate, betanítását és a prediktív modell pontozása</span><span class="sxs-lookup"><span data-stu-id="d482c-108">Read data from your database toocreate, train and score a predictive model</span></span>
* <span data-ttu-id="d482c-109">Adatok tooyour adatbázis írása</span><span class="sxs-lookup"><span data-stu-id="d482c-109">Write data tooyour database</span></span>

## <a name="read-data-from-sql-data-warehouse"></a><span data-ttu-id="d482c-110">Az SQL Data Warehouse-adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="d482c-110">Read data from SQL Data Warehouse</span></span>
<span data-ttu-id="d482c-111">A Microsoft adatokat olvasni termék hello AdventureWorksDW adatbázis táblájában.</span><span class="sxs-lookup"><span data-stu-id="d482c-111">We will read data from Product table in hello AdventureWorksDW database.</span></span>

### <a name="step-1"></a><span data-ttu-id="d482c-112">1. lépés</span><span class="sxs-lookup"><span data-stu-id="d482c-112">Step 1</span></span>
<span data-ttu-id="d482c-113">Új kísérlet indításához kattintson + új hello hello Machine Learning Studio ablakának alsó részén válassza ki a KÍSÉRLETET, és válassza az üres kísérlet.</span><span class="sxs-lookup"><span data-stu-id="d482c-113">Start a new experiment by clicking +NEW at hello bottom of hello Machine Learning Studio window, select EXPERIMENT, and then select Blank Experiment.</span></span> <span data-ttu-id="d482c-114">SELECT hello alapértelmezett neve hello vásznon a hello tetején kísérletezhet, és adjon neki toosomething értelmes, például a kerékpárgyártó árának előrejelzése.</span><span class="sxs-lookup"><span data-stu-id="d482c-114">Select hello default experiment name at hello top of hello canvas and rename it toosomething meaningful, for example, Bicycle price prediction.</span></span>

### <a name="step-2"></a><span data-ttu-id="d482c-115">2. lépés</span><span class="sxs-lookup"><span data-stu-id="d482c-115">Step 2</span></span>
<span data-ttu-id="d482c-116">Keresse meg hello olvasó modul hello paletta az adatkészletek és a modulok hello kísérletvászonra hello balra.</span><span class="sxs-lookup"><span data-stu-id="d482c-116">Look for hello Reader module in hello palette of datasets and modules on hello left of hello experiment canvas.</span></span> <span data-ttu-id="d482c-117">Húzza a hello modul toohello kísérlet vászonra.</span><span class="sxs-lookup"><span data-stu-id="d482c-117">Drag hello module toohello experiment canvas.</span></span>
![][drag_reader]

### <a name="step-3"></a><span data-ttu-id="d482c-118">3. lépés</span><span class="sxs-lookup"><span data-stu-id="d482c-118">Step 3</span></span>
<span data-ttu-id="d482c-119">Válassza ki a hello olvasó modul, és töltse ki a hello tulajdonságok panelen.</span><span class="sxs-lookup"><span data-stu-id="d482c-119">Select hello Reader module and fill out hello properties pane.</span></span>

1. <span data-ttu-id="d482c-120">Az Azure SQL Database jelöli ki hello adatforrás.</span><span class="sxs-lookup"><span data-stu-id="d482c-120">Select Azure SQL Database as hello Data Source.</span></span>
2. <span data-ttu-id="d482c-121">Adatbázis-kiszolgáló nevét: hello kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="d482c-121">Database server name: Type hello server name.</span></span> <span data-ttu-id="d482c-122">Használhatja a hello [Azure-portálon] [ Azure portal] toofind ez.</span><span class="sxs-lookup"><span data-stu-id="d482c-122">You can use hello [Azure portal][Azure portal] toofind this.</span></span>

![][server_name]

1. <span data-ttu-id="d482c-123">Adatbázis neve: hello kiszolgálón csak a megadott adatbázis hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d482c-123">Database name: Type hello name of a database on hello server you just specified.</span></span>
2. <span data-ttu-id="d482c-124">Felhasználói fiók kiszolgálónév: írja be a hello felhasználónevet egy olyan fiók, amely rendelkezik hozzáférési engedélyekkel hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="d482c-124">Server user account name:  Type hello user name of an account that has access permissions for hello database.</span></span>
3. <span data-ttu-id="d482c-125">Kiszolgáló felhasználói fiók jelszavát: Adjon meg hello hello jelszót adott felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="d482c-125">Server user account password: Provide hello password for hello specified user account.</span></span>
4. <span data-ttu-id="d482c-126">Fogadja el a kiszolgálói tanúsítvány: használja ezt a beállítást (kevésbé biztonságos), ha azt szeretné, hogy az adatok olvasása előtt tekintse át a hello webhelytanúsítvány tooskip.</span><span class="sxs-lookup"><span data-stu-id="d482c-126">Accept any server certificate: Use this option (less secure) if you want tooskip reviewing hello site certificate before you read your data.</span></span>
5. <span data-ttu-id="d482c-127">Adatbázis-lekérdezés: Adjon meg egy SQL-utasítást, amely leírja a tooread kívánt hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="d482c-127">Database query: Enter a SQL statement that describes hello data you want tooread.</span></span> <span data-ttu-id="d482c-128">Ebben az esetben azt az adatokat olvassa a következő lekérdezés hello segítségével termék táblából.</span><span class="sxs-lookup"><span data-stu-id="d482c-128">In this case, we will read data from Product table using hello following query.</span></span>

```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a><span data-ttu-id="d482c-129">4. lépés</span><span class="sxs-lookup"><span data-stu-id="d482c-129">Step 4</span></span>
1. <span data-ttu-id="d482c-130">Futtassa a hello kísérlet hello kísérletvászon alatt a Futtatás gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="d482c-130">Run hello experiment by clicking Run under hello experiment canvas.</span></span>
2. <span data-ttu-id="d482c-131">Hello kísérlet befejezésekor hello olvasó modul lesz egy zöld pipa tooindicate sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="d482c-131">When hello experiment finishes, hello Reader module will have a green check mark tooindicate that it has completed successfully.</span></span> <span data-ttu-id="d482c-132">Figyelje meg is hello hello jobb felső sarkában található futási állapota kész.</span><span class="sxs-lookup"><span data-stu-id="d482c-132">Notice also hello Finished running status in hello upper-right corner.</span></span>

![][run]

1. <span data-ttu-id="d482c-133">toosee importált adatok hello hello kimeneti port hello hello autókat tartalmazó adathalmaz alsó részén kattintson, és válassza a képi megjelenítés opcióra.</span><span class="sxs-lookup"><span data-stu-id="d482c-133">toosee hello imported data, click hello output port at hello bottom of hello automobile dataset and select Visualize.</span></span>

## <a name="create-train-and-score-a-model"></a><span data-ttu-id="d482c-134">Létrehozását, betanítását és pontszám modell</span><span class="sxs-lookup"><span data-stu-id="d482c-134">Create, train and score a model</span></span>
<span data-ttu-id="d482c-135">Most már használhatja az adatkészlethez:</span><span class="sxs-lookup"><span data-stu-id="d482c-135">Now you can use this dataset to:</span></span>

* <span data-ttu-id="d482c-136">A modell létrehozása: feldolgozni az adatokat, és a jellemzők meghatározása</span><span class="sxs-lookup"><span data-stu-id="d482c-136">Create a Model: Process data and define features</span></span>
* <span data-ttu-id="d482c-137">Hello tanítási: kiválasztása és tanulási algoritmus alkalmazása</span><span class="sxs-lookup"><span data-stu-id="d482c-137">Train hello model: Choose and apply a learning algorithm</span></span>
* <span data-ttu-id="d482c-138">Pontszám és tesztelési hello modell: új kerékpárgyártó árának előrejelzése</span><span class="sxs-lookup"><span data-stu-id="d482c-138">Score and test hello model: Predict new bicycle price</span></span>

![][model]

<span data-ttu-id="d482c-139">arról, hogyan toocreate, betanítását, pontozása és tesztelése a gépi tanulási modell használata hello további toolearn [létrehozás kísérlet oktatóanyag][Create experiment tutorial].</span><span class="sxs-lookup"><span data-stu-id="d482c-139">toolearn more about how toocreate, train, score and test a machine learning model use hello [Create experiment tutorial][Create experiment tutorial].</span></span>

## <a name="write-data-tooazure-sql-data-warehouse"></a><span data-ttu-id="d482c-140">Az SQL Data Warehouse adatok tooAzure írása</span><span class="sxs-lookup"><span data-stu-id="d482c-140">Write data tooAzure SQL Data Warehouse</span></span>
<span data-ttu-id="d482c-141">Hello eredménytáblájában set tooProductPriceForecast hello AdventureWorksDW adatbázis írja azt.</span><span class="sxs-lookup"><span data-stu-id="d482c-141">We will write hello result set tooProductPriceForecast table in hello AdventureWorksDW database.</span></span>

### <a name="step-1"></a><span data-ttu-id="d482c-142">1. lépés</span><span class="sxs-lookup"><span data-stu-id="d482c-142">Step 1</span></span>
<span data-ttu-id="d482c-143">Keresse meg hello író modul hello paletta az adatkészletek és a modulok hello kísérletvászonra hello balra.</span><span class="sxs-lookup"><span data-stu-id="d482c-143">Look for hello Writer module in hello palette of datasets and modules on hello left of hello experiment canvas.</span></span> <span data-ttu-id="d482c-144">Húzza a hello modul toohello kísérlet vászonra.</span><span class="sxs-lookup"><span data-stu-id="d482c-144">Drag hello module toohello experiment canvas.</span></span>

![][drag_writer]

### <a name="step-2"></a><span data-ttu-id="d482c-145">2. lépés</span><span class="sxs-lookup"><span data-stu-id="d482c-145">Step 2</span></span>
<span data-ttu-id="d482c-146">Válassza ki a hello író modul, és töltse ki a hello tulajdonságok panelen.</span><span class="sxs-lookup"><span data-stu-id="d482c-146">Select hello Writer module and fill out hello properties pane.</span></span>

1. <span data-ttu-id="d482c-147">Az Azure SQL Database jelöli ki hello adatok cél.</span><span class="sxs-lookup"><span data-stu-id="d482c-147">Select Azure SQL Database as hello Data Destination.</span></span>
2. <span data-ttu-id="d482c-148">Adatbázis-kiszolgáló nevét: hello kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="d482c-148">Database server name: Type hello server name.</span></span> <span data-ttu-id="d482c-149">Használhatja a hello [Azure-portálon] [ Azure portal] toofind ez.</span><span class="sxs-lookup"><span data-stu-id="d482c-149">You can use hello [Azure portal][Azure portal] toofind this.</span></span>
3. <span data-ttu-id="d482c-150">Adatbázis neve: hello kiszolgálón csak a megadott adatbázis hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d482c-150">Database name: Type hello name of a database on hello server you just specified.</span></span>
4. <span data-ttu-id="d482c-151">Felhasználói fiók kiszolgálónév: hello adatbázis írási engedéllyel rendelkező fiók hello felhasználói nevét.</span><span class="sxs-lookup"><span data-stu-id="d482c-151">Server user account name:  Type hello user name of an account that has write permissions for hello database.</span></span>
5. <span data-ttu-id="d482c-152">Kiszolgáló felhasználói fiók jelszavát: Adjon meg hello hello jelszót adott felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="d482c-152">Server user account password: Provide hello password for hello specified user account.</span></span>
6. <span data-ttu-id="d482c-153">Fogadja el a kiszolgálói tanúsítvány (nem biztonságos): válassza ezt a lehetőséget, ha nem szeretné, hogy tooview hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="d482c-153">Accept any server certificate (insecure): Select this option if you don’t want tooview hello certificate.</span></span>
7. <span data-ttu-id="d482c-154">Mentett oszlopok toobe vesszővel elválasztott listája: hello dataset vagy eredmény oszlopok listáját tartalmazzák, amelyet az toooutput.</span><span class="sxs-lookup"><span data-stu-id="d482c-154">Comma-separated list of columns toobe saved: Provide a list of hello dataset or result columns that you want toooutput.</span></span>
8. <span data-ttu-id="d482c-155">Adatok táblanév: hello hello adattábla nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="d482c-155">Data table name: Specify hello name of hello data table.</span></span>
9. <span data-ttu-id="d482c-156">A datatable oszlopok vesszővel tagolt listája: hello oszlop nevek toouse meg hello új tábla.</span><span class="sxs-lookup"><span data-stu-id="d482c-156">Comma-separated list of datatable columns:  Specify hello column names toouse in hello new table.</span></span> <span data-ttu-id="d482c-157">hello oszlopnevek különbözhet hello néhányat a meglévők közül hello forrás adatkészletben, de kell listán azonos számú oszlopot itt a kimeneti táblához hello meghatározó hello.</span><span class="sxs-lookup"><span data-stu-id="d482c-157">hello column names can be different from hello ones in hello source dataset, but you must list hello same number of columns here that you define for hello output table.</span></span>
10. <span data-ttu-id="d482c-158">Az SQL Azure-művelet írt sorok száma: beállíthatja a hello tooa SQL-adatbázis egy művelet írt sorok száma.</span><span class="sxs-lookup"><span data-stu-id="d482c-158">Number of rows written per SQL Azure operation: You can configure hello number of rows that are written tooa SQL database in one operation.</span></span>

![][writer_properties]

### <a name="step-3"></a><span data-ttu-id="d482c-159">3. lépés</span><span class="sxs-lookup"><span data-stu-id="d482c-159">Step 3</span></span>
1. <span data-ttu-id="d482c-160">Futtassa a hello kísérlet hello kísérletvászon alatt a Futtatás gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="d482c-160">Run hello experiment by clicking Run under hello experiment canvas.</span></span>
2. <span data-ttu-id="d482c-161">Hello kísérlet befejezése után minden modul lesz egy zöld pipa tooindicate, hogy sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="d482c-161">When hello experiment finishes, all modules will have a green check mark tooindicate that they completed successfully.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d482c-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d482c-162">Next steps</span></span>
<span data-ttu-id="d482c-163">További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="d482c-163">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Create experiment tutorial]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Introduction toomachine learning on Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
