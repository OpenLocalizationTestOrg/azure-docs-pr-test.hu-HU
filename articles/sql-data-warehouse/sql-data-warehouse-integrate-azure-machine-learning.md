---
title: "Az Azure gépi tanulás használja az SQL Data Warehouse szolgáltatással |} Microsoft Docs"
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
ms.openlocfilehash: c19860c6b5b1c15d1e29ddc67f9cf9ad4618725b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a><span data-ttu-id="5bb5a-103">Az Azure gépi tanulás használja az SQL Data Warehouse szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="5bb5a-103">Use Azure Machine Learning with SQL Data Warehouse</span></span>
<span data-ttu-id="5bb5a-104">Az Azure Machine Learning egy teljes körűen felügyelt prediktív elemzési szolgáltatás, amely segítségével az adatok alapján prediktív modellek létrehozása az SQL Data Warehouse, és tegye közzé felhasználásához felhasználásra kész webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-104">Azure Machine Learning is a fully managed predictive analytics service that you can use to create predictive models against your data in SQL Data Warehouse, and then publish as ready-to-consume web services.</span></span> <span data-ttu-id="5bb5a-105">A prediktív elemzés alapvető és a gépi tanulás olvasásával [Bevezetés az Machine Learning Azure][Introduction to Machine Learning on Azure].</span><span class="sxs-lookup"><span data-stu-id="5bb5a-105">You can learn the basics of predictive analytics and machine learning by reading [Introduction to Machine Learning on Azure][Introduction to Machine Learning on Azure].</span></span>  <span data-ttu-id="5bb5a-106">Tudja majd megismerheti, hogyan hozhat létre, betanítását, pontozása és tesztelése a gépi tanulási modell használatával a [létrehozás kísérlet oktatóanyag][Create experiment tutorial].</span><span class="sxs-lookup"><span data-stu-id="5bb5a-106">You can then learn how to create, train, score and test a machine learning model using the [Create experiment tutorial][Create experiment tutorial].</span></span>

<span data-ttu-id="5bb5a-107">Ebből a cikkből megtudhatja, hogyan ehhez a következő használatával a [Azure Machine Learning Studio][Azure Machine Learning Studio]:</span><span class="sxs-lookup"><span data-stu-id="5bb5a-107">In this article, you will learn how to do the following using the [Azure Machine Learning Studio][Azure Machine Learning Studio]:</span></span>

* <span data-ttu-id="5bb5a-108">Adatokat olvasni az adatbázis létrehozását, betanítását és a prediktív modell pontozása</span><span class="sxs-lookup"><span data-stu-id="5bb5a-108">Read data from your database to create, train and score a predictive model</span></span>
* <span data-ttu-id="5bb5a-109">Az adatbázis adatok írása</span><span class="sxs-lookup"><span data-stu-id="5bb5a-109">Write data to your database</span></span>

## <a name="read-data-from-sql-data-warehouse"></a><span data-ttu-id="5bb5a-110">Az SQL Data Warehouse-adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="5bb5a-110">Read data from SQL Data Warehouse</span></span>
<span data-ttu-id="5bb5a-111">A Microsoft adatokat olvasni az AdventureWorksDW adatbázis termék tábla.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-111">We will read data from Product table in the AdventureWorksDW database.</span></span>

### <a name="step-1"></a><span data-ttu-id="5bb5a-112">1. lépés</span><span class="sxs-lookup"><span data-stu-id="5bb5a-112">Step 1</span></span>
<span data-ttu-id="5bb5a-113">Új kísérlet indításához kattintson + a Machine Learning Studio ablakának alján új kísérlet, majd válassza ki és üres kipróbálásához.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-113">Start a new experiment by clicking +NEW at the bottom of the Machine Learning Studio window, select EXPERIMENT, and then select Blank Experiment.</span></span> <span data-ttu-id="5bb5a-114">Válassza ki a kísérlet alapértelmezett nevét a vászon tetején, és módosítsa valami értelmesebbre, például a kerékpárgyártó árának előrejelzése.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-114">Select the default experiment name at the top of the canvas and rename it to something meaningful, for example, Bicycle price prediction.</span></span>

### <a name="step-2"></a><span data-ttu-id="5bb5a-115">2. lépés</span><span class="sxs-lookup"><span data-stu-id="5bb5a-115">Step 2</span></span>
<span data-ttu-id="5bb5a-116">Keresse meg az olvasó modul az adathalmaz a paletta és a modulok bal oldalán a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-116">Look for the Reader module in the palette of datasets and modules on the left of the experiment canvas.</span></span> <span data-ttu-id="5bb5a-117">A modul húzza a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-117">Drag the module to the experiment canvas.</span></span>
![][drag_reader]

### <a name="step-3"></a><span data-ttu-id="5bb5a-118">3. lépés</span><span class="sxs-lookup"><span data-stu-id="5bb5a-118">Step 3</span></span>
<span data-ttu-id="5bb5a-119">Válassza ki az olvasó modul, és töltse ki a Tulajdonságok panelen.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-119">Select the Reader module and fill out the properties pane.</span></span>

1. <span data-ttu-id="5bb5a-120">Válassza ki az Azure SQL-adatbázis az adatforrással.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-120">Select Azure SQL Database as the Data Source.</span></span>
2. <span data-ttu-id="5bb5a-121">Adatbázis-kiszolgáló nevét: írja be a kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-121">Database server name: Type the server name.</span></span> <span data-ttu-id="5bb5a-122">Használhatja a [Azure-portálon] [ Azure portal] ez kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-122">You can use the [Azure portal][Azure portal] to find this.</span></span>

![][server_name]

1. <span data-ttu-id="5bb5a-123">Adatbázis neve: írja be az adatbázis neve csak a megadott kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-123">Database name: Type the name of a database on the server you just specified.</span></span>
2. <span data-ttu-id="5bb5a-124">Felhasználói fiók kiszolgálónév: írja be a felhasználó egy olyan fiók, amely rendelkezik hozzáférési engedélyekkel az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-124">Server user account name:  Type the user name of an account that has access permissions for the database.</span></span>
3. <span data-ttu-id="5bb5a-125">Kiszolgáló felhasználói fiók jelszavát: írja be a megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-125">Server user account password: Provide the password for the specified user account.</span></span>
4. <span data-ttu-id="5bb5a-126">Fogadja el a kiszolgálói tanúsítvány: használja ezt a beállítást (kevésbé biztonságos), ha ki szeretné hagyni a helykiszolgáló tanúsítványának megtekintésével, az adatok olvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-126">Accept any server certificate: Use this option (less secure) if you want to skip reviewing the site certificate before you read your data.</span></span>
5. <span data-ttu-id="5bb5a-127">Adatbázis-lekérdezés: Adjon meg egy SQL-utasítást, amely leírja azokat az olvasni kívánt adatokat.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-127">Database query: Enter a SQL statement that describes the data you want to read.</span></span> <span data-ttu-id="5bb5a-128">Ebben az esetben azt az adatokat olvassa a következő lekérdezéssel termék táblából.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-128">In this case, we will read data from Product table using the following query.</span></span>

```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a><span data-ttu-id="5bb5a-129">4. lépés</span><span class="sxs-lookup"><span data-stu-id="5bb5a-129">Step 4</span></span>
1. <span data-ttu-id="5bb5a-130">A kísérlet futtatásához kattintson a Futtatás a kísérletvászon alatt.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-130">Run the experiment by clicking Run under the experiment canvas.</span></span>
2. <span data-ttu-id="5bb5a-131">A kísérlet befejezése után az olvasó modul lesz egy zöld pipa jelzi, hogy sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-131">When the experiment finishes, the Reader module will have a green check mark to indicate that it has completed successfully.</span></span> <span data-ttu-id="5bb5a-132">Figyelje meg a jobb felső sarokban futási állapota kész.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-132">Notice also the Finished running status in the upper-right corner.</span></span>

![][run]

1. <span data-ttu-id="5bb5a-133">Az importált adatok megtekintéséhez kattintson az autókat tartalmazó adathalmaz alsó a kimeneti portra, és válassza ki a képi megjelenítés opcióra.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-133">To see the imported data, click the output port at the bottom of the automobile dataset and select Visualize.</span></span>

## <a name="create-train-and-score-a-model"></a><span data-ttu-id="5bb5a-134">Létrehozását, betanítását és pontszám modell</span><span class="sxs-lookup"><span data-stu-id="5bb5a-134">Create, train and score a model</span></span>
<span data-ttu-id="5bb5a-135">Most már használhatja az adatkészlethez:</span><span class="sxs-lookup"><span data-stu-id="5bb5a-135">Now you can use this dataset to:</span></span>

* <span data-ttu-id="5bb5a-136">A modell létrehozása: feldolgozni az adatokat, és a jellemzők meghatározása</span><span class="sxs-lookup"><span data-stu-id="5bb5a-136">Create a Model: Process data and define features</span></span>
* <span data-ttu-id="5bb5a-137">A modell betanításához: kiválasztása és tanulási algoritmus alkalmazása</span><span class="sxs-lookup"><span data-stu-id="5bb5a-137">Train the model: Choose and apply a learning algorithm</span></span>
* <span data-ttu-id="5bb5a-138">Pontozni és tesztelni a modellt: új kerékpárgyártó árának előrejelzése</span><span class="sxs-lookup"><span data-stu-id="5bb5a-138">Score and test the model: Predict new bicycle price</span></span>

![][model]

<span data-ttu-id="5bb5a-139">Létrehozásával kapcsolatos további tudnivalókért betanítása, pontozása és tesztelése a gépi tanulási modell használja a [létrehozás kísérlet oktatóanyag][Create experiment tutorial].</span><span class="sxs-lookup"><span data-stu-id="5bb5a-139">To learn more about how to create, train, score and test a machine learning model use the [Create experiment tutorial][Create experiment tutorial].</span></span>

## <a name="write-data-to-azure-sql-data-warehouse"></a><span data-ttu-id="5bb5a-140">Az Azure SQL Data Warehouse-adatok írása</span><span class="sxs-lookup"><span data-stu-id="5bb5a-140">Write data to Azure SQL Data Warehouse</span></span>
<span data-ttu-id="5bb5a-141">Az AdventureWorksDW adatbázis ProductPriceForecast táblájához eredménykészlet írja azt.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-141">We will write the result set to ProductPriceForecast table in the AdventureWorksDW database.</span></span>

### <a name="step-1"></a><span data-ttu-id="5bb5a-142">1. lépés</span><span class="sxs-lookup"><span data-stu-id="5bb5a-142">Step 1</span></span>
<span data-ttu-id="5bb5a-143">Keresse meg a író modul az adathalmaz a paletta és a modulok bal oldalán a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-143">Look for the Writer module in the palette of datasets and modules on the left of the experiment canvas.</span></span> <span data-ttu-id="5bb5a-144">A modul húzza a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-144">Drag the module to the experiment canvas.</span></span>

![][drag_writer]

### <a name="step-2"></a><span data-ttu-id="5bb5a-145">2. lépés</span><span class="sxs-lookup"><span data-stu-id="5bb5a-145">Step 2</span></span>
<span data-ttu-id="5bb5a-146">Válassza ki azt a író modult, és töltse ki a Tulajdonságok panelen.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-146">Select the Writer module and fill out the properties pane.</span></span>

1. <span data-ttu-id="5bb5a-147">Válassza ki az Azure SQL-adatbázis adatok céljaként.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-147">Select Azure SQL Database as the Data Destination.</span></span>
2. <span data-ttu-id="5bb5a-148">Adatbázis-kiszolgáló nevét: írja be a kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-148">Database server name: Type the server name.</span></span> <span data-ttu-id="5bb5a-149">Használhatja a [Azure-portálon] [ Azure portal] ez kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-149">You can use the [Azure portal][Azure portal] to find this.</span></span>
3. <span data-ttu-id="5bb5a-150">Adatbázis neve: írja be az adatbázis neve csak a megadott kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-150">Database name: Type the name of a database on the server you just specified.</span></span>
4. <span data-ttu-id="5bb5a-151">Felhasználói fiók kiszolgálónév: írja be a felhasználó egy olyan fiók, amely rendelkezik írási engedéllyel az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-151">Server user account name:  Type the user name of an account that has write permissions for the database.</span></span>
5. <span data-ttu-id="5bb5a-152">Kiszolgáló felhasználói fiók jelszavát: írja be a megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-152">Server user account password: Provide the password for the specified user account.</span></span>
6. <span data-ttu-id="5bb5a-153">Fogadja el a kiszolgálói tanúsítvány (nem biztonságos): válassza ezt a lehetőséget, ha nem szeretné a tanúsítvány megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-153">Accept any server certificate (insecure): Select this option if you don’t want to view the certificate.</span></span>
7. <span data-ttu-id="5bb5a-154">A mentendő oszlopok vesszővel tagolt listája: Adja meg, amelyet a kimeneti adatkészlet vagy eredmény oszlopok listája.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-154">Comma-separated list of columns to be saved: Provide a list of the dataset or result columns that you want to output.</span></span>
8. <span data-ttu-id="5bb5a-155">Adatok táblanév: Adja meg a tábla nevét.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-155">Data table name: Specify the name of the data table.</span></span>
9. <span data-ttu-id="5bb5a-156">A datatable oszlopok vesszővel tagolt listája: Adja meg az új táblázat oszlopneveit.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-156">Comma-separated list of datatable columns:  Specify the column names to use in the new table.</span></span> <span data-ttu-id="5bb5a-157">Lehet, hogy az oszlopok neveit eltérő a forrás adatkészletben, de a azonos számú oszlopot itt a kimeneti táblához meghatározó kell listában.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-157">The column names can be different from the ones in the source dataset, but you must list the same number of columns here that you define for the output table.</span></span>
10. <span data-ttu-id="5bb5a-158">Az SQL Azure-művelet írt sorok száma: konfigurálhatja egy műveletet az SQL-adatbázisba írt sorok száma.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-158">Number of rows written per SQL Azure operation: You can configure the number of rows that are written to a SQL database in one operation.</span></span>

![][writer_properties]

### <a name="step-3"></a><span data-ttu-id="5bb5a-159">3. lépés</span><span class="sxs-lookup"><span data-stu-id="5bb5a-159">Step 3</span></span>
1. <span data-ttu-id="5bb5a-160">A kísérlet futtatásához kattintson a Futtatás a kísérletvászon alatt.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-160">Run the experiment by clicking Run under the experiment canvas.</span></span>
2. <span data-ttu-id="5bb5a-161">A kísérlet befejezése után minden modul lesz egy zöld pipa jelzi, hogy sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="5bb5a-161">When the experiment finishes, all modules will have a green check mark to indicate that they completed successfully.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5bb5a-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5bb5a-162">Next steps</span></span>
<span data-ttu-id="5bb5a-163">További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="5bb5a-163">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

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
[Introduction to machine learning on Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
