---
title: "Az Azure Stream Analytics használja az SQL Data Warehouse szolgáltatással |} Microsoft Docs"
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
ms.openlocfilehash: 14783f0464764a11d7f03a5db1c2d63728a4cb50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a><span data-ttu-id="fb5b1-103">Az Azure Stream Analytics használja az SQL Data Warehouse szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="fb5b1-103">Use Azure Stream Analytics with SQL Data Warehouse</span></span>
<span data-ttu-id="fb5b1-104">Az Azure Stream Analytics egy olyan teljes körűen felügyelt szolgáltatás biztosítása kis késleltetésű, magas rendelkezésre állású, méretezhető összetett eseményfeldolgozást keresztül a streamelési adatok a felhőben.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-104">Azure Stream Analytics is a fully managed service providing low-latency, highly available, scalable complex event processing over streaming data in the cloud.</span></span> <span data-ttu-id="fb5b1-105">Az alapok tanul olvasása [Azure Stream Analytics bemutatása][Introduction to Azure Stream Analytics].</span><span class="sxs-lookup"><span data-stu-id="fb5b1-105">You can learn the basics by reading [Introduction to Azure Stream Analytics][Introduction to Azure Stream Analytics].</span></span> <span data-ttu-id="fb5b1-106">Majd megismerheti egy végpont megoldás létrehozása a Stream Analytics a következő a [Azure Stream Analytics használatának első] [ Get started using Azure Stream Analytics] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-106">You can then learn how to create an end-to-end solution with Stream Analytics by following the [Get started using Azure Stream Analytics][Get started using Azure Stream Analytics] tutorial.</span></span>

<span data-ttu-id="fb5b1-107">Meg ebből a cikkből megtudhatja, hogyan használható az Azure SQL Data Warehouse-adatbázis szolgáltatást kimeneti fogadóként a gőz Analytics-feladatok.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-107">In this article, you will learn how to use your Azure SQL Data Warehouse database as an output sink for your Steam Analytics jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb5b1-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fb5b1-108">Prerequisites</span></span>
<span data-ttu-id="fb5b1-109">Először futtassa a következő lépések a [Azure Stream Analytics használatának első] [ Get started using Azure Stream Analytics] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-109">First, run through the following steps in the [Get started using Azure Stream Analytics][Get started using Azure Stream Analytics] tutorial.</span></span>  

1. <span data-ttu-id="fb5b1-110">Az Event Hubs bemeneti létrehozása</span><span class="sxs-lookup"><span data-stu-id="fb5b1-110">Create an Event Hub input</span></span>
2. <span data-ttu-id="fb5b1-111">Konfigurálni és elindítani az esemény-készítő alkalmazás</span><span class="sxs-lookup"><span data-stu-id="fb5b1-111">Configure and start event generator application</span></span>
3. <span data-ttu-id="fb5b1-112">A Stream Analytics-feladat kiépítése</span><span class="sxs-lookup"><span data-stu-id="fb5b1-112">Provision a Stream Analytics job</span></span>
4. <span data-ttu-id="fb5b1-113">Adja meg a feladat bemeneti és a lekérdezés</span><span class="sxs-lookup"><span data-stu-id="fb5b1-113">Specify job input and query</span></span>

<span data-ttu-id="fb5b1-114">Ezután hozzon létre egy Azure SQL Data Warehouse-adatbázis</span><span class="sxs-lookup"><span data-stu-id="fb5b1-114">Then, create an Azure SQL Data Warehouse database</span></span>

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a><span data-ttu-id="fb5b1-115">Adja meg a feladat kimenetére: Azure SQL Data Warehouse-adatbázis</span><span class="sxs-lookup"><span data-stu-id="fb5b1-115">Specify job output: Azure SQL Data Warehouse database</span></span>
### <a name="step-1"></a><span data-ttu-id="fb5b1-116">1. lépés</span><span class="sxs-lookup"><span data-stu-id="fb5b1-116">Step 1</span></span>
<span data-ttu-id="fb5b1-117">Kattintson a Stream Analytics-feladat **kimeneti** , és kattintson a lista elejéről **hozzáadása kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-117">In your Stream Analytics job click **OUTPUT** from the top of the page, and then click **ADD OUTPUT**.</span></span>

### <a name="step-2"></a><span data-ttu-id="fb5b1-118">2. lépés</span><span class="sxs-lookup"><span data-stu-id="fb5b1-118">Step 2</span></span>
<span data-ttu-id="fb5b1-119">Válassza ki az SQL-adatbázist, és kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-119">Select SQL Database and click next.</span></span>

![][add-output]

### <a name="step-3"></a><span data-ttu-id="fb5b1-120">3. lépés</span><span class="sxs-lookup"><span data-stu-id="fb5b1-120">Step 3</span></span>
<span data-ttu-id="fb5b1-121">Adja meg a következő értékeket a következő lapon:</span><span class="sxs-lookup"><span data-stu-id="fb5b1-121">Enter the following values on the next page:</span></span>

* <span data-ttu-id="fb5b1-122">*A kimeneti Alias*: Adjon meg egy rövid nevet a feladat kimenete.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-122">*Output Alias*: Enter a friendly name for this job output.</span></span>
* <span data-ttu-id="fb5b1-123">*Előfizetés*:</span><span class="sxs-lookup"><span data-stu-id="fb5b1-123">*Subscription*:</span></span>
  * <span data-ttu-id="fb5b1-124">Ha az SQL Data Warehouse-adatbázis a Stream Analytics-feladat tárolóként ugyanazt az előfizetést, válassza ki az SQL Database a jelenlegi előfizetés.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-124">If your SQL Data Warehouse database is in the same subscription as the Stream Analytics job, select Use SQL Database from Current Subscription.</span></span>
  * <span data-ttu-id="fb5b1-125">Ha az adatbázis egy másik előfizetésben, válassza ki az SQL Database egy másik előfizetésből.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-125">If your database is in a different subscription, select Use SQL Database from Another Subscription.</span></span>
* <span data-ttu-id="fb5b1-126">*Adatbázis*: Adja meg a célként megadott adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-126">*Database*: Specify the name of a destination database.</span></span>
* <span data-ttu-id="fb5b1-127">*Kiszolgálónév*: Adja meg az adatbázis csak a megadott kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-127">*Server Name*: Specify the server name for the database you just specified.</span></span> <span data-ttu-id="fb5b1-128">A klasszikus Azure portál segítségével ez.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-128">You can use the Azure Classic Portal to find this.</span></span>

![][server-name]

* <span data-ttu-id="fb5b1-129">*Felhasználónév*: Adja meg az adatbázis írási engedéllyel rendelkező fiók felhasználónevét.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-129">*User Name*: Specify the user name of an account that has write permissions for the database.</span></span>
* <span data-ttu-id="fb5b1-130">*Jelszó*: írja be a megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-130">*Password*: Provide the password for the specified user account.</span></span>
* <span data-ttu-id="fb5b1-131">*Tábla*: Adja meg a nevét a céloldali tábla az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-131">*Table*: Specify the name of the target table in the database.</span></span>

![][add-database]

### <a name="step-4"></a><span data-ttu-id="fb5b1-132">4. lépés</span><span class="sxs-lookup"><span data-stu-id="fb5b1-132">Step 4</span></span>
<span data-ttu-id="fb5b1-133">Kattintson a pipa gombra a feladat kimenetére hozzáadása, és győződjön meg arról, hogy a Stream Analytics képes csatlakozni az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-133">Click the check button to add this job output and to verify that Stream Analytics can successfully connect to the database.</span></span>

![][test-connection]

<span data-ttu-id="fb5b1-134">Ha a kapcsolat az adatbázissal sikeres, akkor megjelenik egy értesítés a portál alján.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-134">When the connection to the database succeeds, you will see a notification at the bottom of the portal.</span></span> <span data-ttu-id="fb5b1-135">Kapcsolat tesztelése gombra az alsó tesztelje a kapcsolatot az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="fb5b1-135">You can click Test Connection at the bottom to test the connection to the database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb5b1-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fb5b1-136">Next steps</span></span>
<span data-ttu-id="fb5b1-137">Az integráció áttekintéséért lásd: [SQL Data Warehouse integrációjának áttekintése][SQL Data Warehouse integration overview].</span><span class="sxs-lookup"><span data-stu-id="fb5b1-137">For an overview of integration, see [SQL Data Warehouse integration overview][SQL Data Warehouse integration overview].</span></span>

<span data-ttu-id="fb5b1-138">További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="fb5b1-138">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction to Azure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
