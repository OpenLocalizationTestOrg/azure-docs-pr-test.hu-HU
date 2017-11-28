---
title: "A táblázatos modellek létrehozása az Azure Analysis Services Web-tervezővel |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre egy Azure Analysis Services táblázatos modell a webalkalmazás-tervező használata Azure-portálon."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: bd58f1845dabf6afb47ce27236d14479677a8808
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-model-in-azure-portal"></a><span data-ttu-id="1d99e-103">A modell létrehozása Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1d99e-103">Create a model in Azure portal</span></span>

<span data-ttu-id="1d99e-104">Az Azure Analysis Services-webalkalmazás designer (előzetes verzió) Azure-portálon szolgáltatás biztosítja az gyorsan és egyszerűen és szerkesztése a táblázatos modellek és lekérdezi a modell adatok jobb a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="1d99e-104">The Azure Analysis Services web designer (preview) feature in Azure portal provides a quick and easy way to create and edit tabular models and query model data right in your browser.</span></span> 

<span data-ttu-id="1d99e-105">Ne feledje, a webes Designer **előzetes**.</span><span class="sxs-lookup"><span data-stu-id="1d99e-105">Keep in mind, the web designer is **preview**.</span></span> <span data-ttu-id="1d99e-106">Új funkciók hozzáadott folyamatosan, kép, amíg a funkciók korlátozva.</span><span class="sxs-lookup"><span data-stu-id="1d99e-106">While new functionality is being added all the time, in preview, functionality is limited.</span></span> <span data-ttu-id="1d99e-107">A speciális modell fejlesztéshez és teszteléshez érdemes használni a Visual Studio (SSDT) és az SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="1d99e-107">For more advanced model development and testing, it's best to use Visual Studio (SSDT) and SQL Server Management Studio (SSMS).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d99e-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1d99e-108">Prerequisites</span></span>

- <span data-ttu-id="1d99e-109">Egy, a Standard vagy fejlesztői réteg Azure Analysis Services-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="1d99e-109">An Azure Analysis Services server at the Standard or Developer tier.</span></span> <span data-ttu-id="1d99e-110">A webalkalmazás-tervezővel létrehozott új modellek DirectQuery, csak ezek a rétegek támogatja.</span><span class="sxs-lookup"><span data-stu-id="1d99e-110">New models created by using the Web designer are DirectQuery, supported only by these tiers.</span></span>
- <span data-ttu-id="1d99e-111">Egy Azure SQL Database, Azure SQL Data warehouse-bA vagy egy adatforrást a Power BI Desktop (.pbix) fájlt.</span><span class="sxs-lookup"><span data-stu-id="1d99e-111">An Azure SQL Database, Azure SQL Data Warehouse, or Power BI Desktop (.pbix) file as a datasource.</span></span> <span data-ttu-id="1d99e-112">A Power BI Desktop fájlok támogatása az Azure SQL Database, Azure SQL Data Warehouse, Oracle és Teradata adatforrások létrehozott új modellek.</span><span class="sxs-lookup"><span data-stu-id="1d99e-112">New models created from Power BI Desktop files support Azure SQL Database, Azure SQL Data Warehouse, Oracle, and Teradata data sources.</span></span>
- <span data-ttu-id="1d99e-113">Egy SQL Server-fiókkal és jelszóval csatlakozás az Azure SQL Database vagy az Azure SQL Data Warehouse-adatforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="1d99e-113">A SQL Server account and password for connecting to Azure SQL Database or Azure SQL Data Warehouse data sources.</span></span>

## <a name="to-create-a-new-tabular-model"></a><span data-ttu-id="1d99e-114">Egy új táblázatos modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d99e-114">To create a new tabular model</span></span>

1. <span data-ttu-id="1d99e-115">A Server **áttekintése** panel > **webes Tervező**, kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="1d99e-115">In your server's **Overview** blade > **Web designer**, click **Open**.</span></span>

    ![A modell létrehozása Azure-portálon](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. <span data-ttu-id="1d99e-117">A **webes Tervező** > **modellek**, kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="1d99e-117">In **Web designer** > **Models**, click **+ Add**.</span></span>

    ![A modell létrehozása Azure-portálon](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. <span data-ttu-id="1d99e-119">A **új modell**, írja be a modell neve, majd válassza ki egy adatforrást.</span><span class="sxs-lookup"><span data-stu-id="1d99e-119">In **New model**, type a model name, and then select a data source.</span></span>

    ![Azure-portálon az új modell párbeszédpanel](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. <span data-ttu-id="1d99e-121">A **Connect**, adja meg a kapcsolat tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="1d99e-121">In **Connect**, enter the connection properties.</span></span> <span data-ttu-id="1d99e-122">Felhasználónév és jelszó SQL Server fióknak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1d99e-122">Username and password must be a SQL Server account.</span></span>

     ![Csatlakozás Azure-portálon párbeszédpanel](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. <span data-ttu-id="1d99e-124">A **táblák és nézetek**, válassza ki a modell tartalmazza, és kattintson a táblákat **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="1d99e-124">In **Tables and views**, select the tables to include in your model, and then click **Create**.</span></span> <span data-ttu-id="1d99e-125">Egy kulcspár rendelkező táblák közötti kapcsolatok jönnek létre automatikusan.</span><span class="sxs-lookup"><span data-stu-id="1d99e-125">Relationships are created automatically between tables with a key pair.</span></span>

     ![Válassza ki a táblák és nézetek](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

<span data-ttu-id="1d99e-127">Az új modell jelenik meg a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="1d99e-127">Your new model appears in your browser.</span></span> <span data-ttu-id="1d99e-128">Itt a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="1d99e-128">From here, you can:</span></span>   

- <span data-ttu-id="1d99e-129">Query modelladatok húzzon mezőket a Lekérdezéstervezőben, és vegye fel a szűrőket.</span><span class="sxs-lookup"><span data-stu-id="1d99e-129">Query model data by dragging fields to the query designer and adding filters.</span></span>
- <span data-ttu-id="1d99e-130">Hozzon létre új mértékek táblában.</span><span class="sxs-lookup"><span data-stu-id="1d99e-130">Create new measures in tables.</span></span>
- <span data-ttu-id="1d99e-131">Módosítsa a modell metaadatait a json-szerkesztő segítségével.</span><span class="sxs-lookup"><span data-stu-id="1d99e-131">Edit model metadata by using the json editor.</span></span>
- <span data-ttu-id="1d99e-132">Nyissa meg a modellt a Visual Studio (SSDT), a Power BI Desktop vagy az Excel.</span><span class="sxs-lookup"><span data-stu-id="1d99e-132">Open the model in Visual Studio (SSDT), Power BI Desktop, or Excel.</span></span>

![Válassza ki a táblák és nézetek](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> <span data-ttu-id="1d99e-134">Modell metaadatainak szerkesztése, illetve a böngészőben új mértékek létrehozása menti ezeket a módosításokat az Azure-ban a modellhez.</span><span class="sxs-lookup"><span data-stu-id="1d99e-134">When you edit model metadata or create new measures in your browser, you're saving those changes to your model in Azure.</span></span> <span data-ttu-id="1d99e-135">Ha még dolgozik a modell SSDT, a Power BI Desktop vagy az Excel, a modell szinkronban kérheti le.</span><span class="sxs-lookup"><span data-stu-id="1d99e-135">If you're also working on your model in SSDT, Power BI Desktop, or Excel, your model can get out of sync.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1d99e-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d99e-136">Next steps</span></span> 
[<span data-ttu-id="1d99e-137">Adatbázis-szerepkörök és a felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="1d99e-137">Manage database roles and users</span></span>](analysis-services-database-users.md)  
[<span data-ttu-id="1d99e-138">Kapcsolódás Excellel</span><span class="sxs-lookup"><span data-stu-id="1d99e-138">Connect with Excel</span></span>](analysis-services-connect-excel.md)  


