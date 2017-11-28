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
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a><span data-ttu-id="3f76a-103">Az Azure Stream Analytics használja az SQL Data Warehouse szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="3f76a-103">Use Azure Stream Analytics with SQL Data Warehouse</span></span>
<span data-ttu-id="3f76a-104">Az Azure Stream Analytics egy teljes körűen felügyelt szolgáltatás biztosítása kis késleltetésű, magas rendelkezésre állású, méretezhető összetett eseményfeldolgozást keresztül adatfolyam hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="3f76a-104">Azure Stream Analytics is a fully managed service providing low-latency, highly available, scalable complex event processing over streaming data in hello cloud.</span></span> <span data-ttu-id="3f76a-105">Elolvasásával megismerheti hello alapjai [Stream Analytics bemutatása tooAzure][Introduction tooAzure Stream Analytics].</span><span class="sxs-lookup"><span data-stu-id="3f76a-105">You can learn hello basics by reading [Introduction tooAzure Stream Analytics][Introduction tooAzure Stream Analytics].</span></span> <span data-ttu-id="3f76a-106">Majd megtudhatja, hogyan toocreate egy végpont megoldás a Stream Analytics következő hello [Azure Stream Analytics használatának első] [ Get started using Azure Stream Analytics] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="3f76a-106">You can then learn how toocreate an end-to-end solution with Stream Analytics by following hello [Get started using Azure Stream Analytics][Get started using Azure Stream Analytics] tutorial.</span></span>

<span data-ttu-id="3f76a-107">Ebből a cikkből megtudhatja, hogyan toouse az Azure SQL Data Warehouse-adatbázis szolgáltatást kimeneti fogadóként a gőz Analytics-feladatok.</span><span class="sxs-lookup"><span data-stu-id="3f76a-107">In this article, you will learn how toouse your Azure SQL Data Warehouse database as an output sink for your Steam Analytics jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f76a-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3f76a-108">Prerequisites</span></span>
<span data-ttu-id="3f76a-109">Először futtassa a következő lépéseket a hello hello keresztül [Azure Stream Analytics használatának első] [ Get started using Azure Stream Analytics] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="3f76a-109">First, run through hello following steps in hello [Get started using Azure Stream Analytics][Get started using Azure Stream Analytics] tutorial.</span></span>  

1. <span data-ttu-id="3f76a-110">Az Event Hubs bemeneti létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f76a-110">Create an Event Hub input</span></span>
2. <span data-ttu-id="3f76a-111">Konfigurálni és elindítani az esemény-készítő alkalmazás</span><span class="sxs-lookup"><span data-stu-id="3f76a-111">Configure and start event generator application</span></span>
3. <span data-ttu-id="3f76a-112">A Stream Analytics-feladat kiépítése</span><span class="sxs-lookup"><span data-stu-id="3f76a-112">Provision a Stream Analytics job</span></span>
4. <span data-ttu-id="3f76a-113">Adja meg a feladat bemeneti és a lekérdezés</span><span class="sxs-lookup"><span data-stu-id="3f76a-113">Specify job input and query</span></span>

<span data-ttu-id="3f76a-114">Ezután hozzon létre egy Azure SQL Data Warehouse-adatbázis</span><span class="sxs-lookup"><span data-stu-id="3f76a-114">Then, create an Azure SQL Data Warehouse database</span></span>

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a><span data-ttu-id="3f76a-115">Adja meg a feladat kimenetére: Azure SQL Data Warehouse-adatbázis</span><span class="sxs-lookup"><span data-stu-id="3f76a-115">Specify job output: Azure SQL Data Warehouse database</span></span>
### <a name="step-1"></a><span data-ttu-id="3f76a-116">1. lépés</span><span class="sxs-lookup"><span data-stu-id="3f76a-116">Step 1</span></span>
<span data-ttu-id="3f76a-117">Kattintson a Stream Analytics-feladat **kimeneti** hello felső részén hello lapot, és kattintson a **hozzáadása kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="3f76a-117">In your Stream Analytics job click **OUTPUT** from hello top of hello page, and then click **ADD OUTPUT**.</span></span>

### <a name="step-2"></a><span data-ttu-id="3f76a-118">2. lépés</span><span class="sxs-lookup"><span data-stu-id="3f76a-118">Step 2</span></span>
<span data-ttu-id="3f76a-119">Válassza ki az SQL-adatbázist, és kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="3f76a-119">Select SQL Database and click next.</span></span>

![][add-output]

### <a name="step-3"></a><span data-ttu-id="3f76a-120">3. lépés</span><span class="sxs-lookup"><span data-stu-id="3f76a-120">Step 3</span></span>
<span data-ttu-id="3f76a-121">Adja meg a következő értékeket a következő oldalon hello hello:</span><span class="sxs-lookup"><span data-stu-id="3f76a-121">Enter hello following values on hello next page:</span></span>

* <span data-ttu-id="3f76a-122">*A kimeneti Alias*: Adjon meg egy rövid nevet a feladat kimenete.</span><span class="sxs-lookup"><span data-stu-id="3f76a-122">*Output Alias*: Enter a friendly name for this job output.</span></span>
* <span data-ttu-id="3f76a-123">*Előfizetés*:</span><span class="sxs-lookup"><span data-stu-id="3f76a-123">*Subscription*:</span></span>
  * <span data-ttu-id="3f76a-124">Ha az SQL Data Warehouse-adatbázis hello ugyanahhoz az előfizetéshez hello Stream Analytics-feladat, mint a jelenlegi előfizetés válassza ki az SQL Database.</span><span class="sxs-lookup"><span data-stu-id="3f76a-124">If your SQL Data Warehouse database is in hello same subscription as hello Stream Analytics job, select Use SQL Database from Current Subscription.</span></span>
  * <span data-ttu-id="3f76a-125">Ha az adatbázis egy másik előfizetésben, válassza ki az SQL Database egy másik előfizetésből.</span><span class="sxs-lookup"><span data-stu-id="3f76a-125">If your database is in a different subscription, select Use SQL Database from Another Subscription.</span></span>
* <span data-ttu-id="3f76a-126">*Adatbázis*: Adja meg a célként megadott adatbázis hello nevét.</span><span class="sxs-lookup"><span data-stu-id="3f76a-126">*Database*: Specify hello name of a destination database.</span></span>
* <span data-ttu-id="3f76a-127">*Kiszolgálónév*: Adja meg az imént megadott hello adatbázis hello kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="3f76a-127">*Server Name*: Specify hello server name for hello database you just specified.</span></span> <span data-ttu-id="3f76a-128">Ezzel hello klasszikus Azure portál toofind.</span><span class="sxs-lookup"><span data-stu-id="3f76a-128">You can use hello Azure Classic Portal toofind this.</span></span>

![][server-name]

* <span data-ttu-id="3f76a-129">*Felhasználónév*: hello hello adatbázis írási engedéllyel rendelkező fiók felhasználónevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="3f76a-129">*User Name*: Specify hello user name of an account that has write permissions for hello database.</span></span>
* <span data-ttu-id="3f76a-130">*Jelszó*: Adja meg a hello hello jelszó megadott felhasználói fiók.</span><span class="sxs-lookup"><span data-stu-id="3f76a-130">*Password*: Provide hello password for hello specified user account.</span></span>
* <span data-ttu-id="3f76a-131">*Tábla*: meg kell adnia hello adatbázis hello céltábla hello nevet.</span><span class="sxs-lookup"><span data-stu-id="3f76a-131">*Table*: Specify hello name of hello target table in hello database.</span></span>

![][add-database]

### <a name="step-4"></a><span data-ttu-id="3f76a-132">4. lépés</span><span class="sxs-lookup"><span data-stu-id="3f76a-132">Step 4</span></span>
<span data-ttu-id="3f76a-133">Ezt a feladatot, kimeneti és, hogy a Stream Analytics képes csatlakozni toohello adatbázis tooverify, kattintson a hello ellenőrzése gombra tooadd.</span><span class="sxs-lookup"><span data-stu-id="3f76a-133">Click hello check button tooadd this job output and tooverify that Stream Analytics can successfully connect toohello database.</span></span>

![][test-connection]

<span data-ttu-id="3f76a-134">Ha hello kapcsolat toohello adatbázis sikeres, látni fogja a hello portal hello alján értesítést.</span><span class="sxs-lookup"><span data-stu-id="3f76a-134">When hello connection toohello database succeeds, you will see a notification at hello bottom of hello portal.</span></span> <span data-ttu-id="3f76a-135">Kapcsolat tesztelése hello alsó tootest hello kapcsolat toohello adatbázis elemre.</span><span class="sxs-lookup"><span data-stu-id="3f76a-135">You can click Test Connection at hello bottom tootest hello connection toohello database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f76a-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3f76a-136">Next steps</span></span>
<span data-ttu-id="3f76a-137">Az integráció áttekintéséért lásd: [SQL Data Warehouse integrációjának áttekintése][SQL Data Warehouse integration overview].</span><span class="sxs-lookup"><span data-stu-id="3f76a-137">For an overview of integration, see [SQL Data Warehouse integration overview][SQL Data Warehouse integration overview].</span></span>

<span data-ttu-id="3f76a-138">További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="3f76a-138">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

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
