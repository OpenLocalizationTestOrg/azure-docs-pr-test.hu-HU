---
title: "a táblázatos modellek hello Azure Analysis Services Web-tervezővel aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate az Azure Analysis Services táblázatos modell használatával hello webes Tervező Azure-portálon."
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
ms.openlocfilehash: a37b326b76c84fc3a4300827bc1c8706b0584701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-model-in-azure-portal"></a><span data-ttu-id="cde3c-103">A modell létrehozása Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="cde3c-103">Create a model in Azure portal</span></span>

<span data-ttu-id="cde3c-104">hello Azure Analysis Services web designer (előzetes verzió) szolgáltatás Azure-portálon egy gyorsan és egyszerűen elvégezhető toocreate biztosít, és szerkesztheti a táblázatos modellek és lekérdezés modell adatok jobb a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="cde3c-104">hello Azure Analysis Services web designer (preview) feature in Azure portal provides a quick and easy way toocreate and edit tabular models and query model data right in your browser.</span></span> 

<span data-ttu-id="cde3c-105">Ne feledje, a hello webes Tervező **előzetes**.</span><span class="sxs-lookup"><span data-stu-id="cde3c-105">Keep in mind, hello web designer is **preview**.</span></span> <span data-ttu-id="cde3c-106">Új funkciók hozzáadott összes hello idő, kép, amíg a funkciók korlátozva.</span><span class="sxs-lookup"><span data-stu-id="cde3c-106">While new functionality is being added all hello time, in preview, functionality is limited.</span></span> <span data-ttu-id="cde3c-107">A speciális modell fejlesztéshez és teszteléshez akkor a legjobb toouse Visual Studio (SSDT) és az SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="cde3c-107">For more advanced model development and testing, it's best toouse Visual Studio (SSDT) and SQL Server Management Studio (SSMS).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cde3c-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cde3c-108">Prerequisites</span></span>

- <span data-ttu-id="cde3c-109">Egy hello Standard vagy fejlesztői réteg: Azure Analysis Services-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="cde3c-109">An Azure Analysis Services server at hello Standard or Developer tier.</span></span> <span data-ttu-id="cde3c-110">Hello Web-tervezővel létrehozott új modellek DirectQuery, csak ezek a rétegek támogatja.</span><span class="sxs-lookup"><span data-stu-id="cde3c-110">New models created by using hello Web designer are DirectQuery, supported only by these tiers.</span></span>
- <span data-ttu-id="cde3c-111">Egy Azure SQL Database, Azure SQL Data warehouse-bA vagy egy adatforrást a Power BI Desktop (.pbix) fájlt.</span><span class="sxs-lookup"><span data-stu-id="cde3c-111">An Azure SQL Database, Azure SQL Data Warehouse, or Power BI Desktop (.pbix) file as a datasource.</span></span> <span data-ttu-id="cde3c-112">A Power BI Desktop fájlok támogatása az Azure SQL Database, Azure SQL Data Warehouse, Oracle és Teradata adatforrások létrehozott új modellek.</span><span class="sxs-lookup"><span data-stu-id="cde3c-112">New models created from Power BI Desktop files support Azure SQL Database, Azure SQL Data Warehouse, Oracle, and Teradata data sources.</span></span>
- <span data-ttu-id="cde3c-113">Egy SQL Server fiókot és jelszót tooAzure SQL Database vagy az Azure SQL Data Warehouse adatforrások csatlakoztatásához.</span><span class="sxs-lookup"><span data-stu-id="cde3c-113">A SQL Server account and password for connecting tooAzure SQL Database or Azure SQL Data Warehouse data sources.</span></span>

## <a name="toocreate-a-new-tabular-model"></a><span data-ttu-id="cde3c-114">egy új táblázatos modell toocreate</span><span class="sxs-lookup"><span data-stu-id="cde3c-114">toocreate a new tabular model</span></span>

1. <span data-ttu-id="cde3c-115">A Server **áttekintése** panel > **webes Tervező**, kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="cde3c-115">In your server's **Overview** blade > **Web designer**, click **Open**.</span></span>

    ![A modell létrehozása Azure-portálon](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. <span data-ttu-id="cde3c-117">A **webes Tervező** > **modellek**, kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="cde3c-117">In **Web designer** > **Models**, click **+ Add**.</span></span>

    ![A modell létrehozása Azure-portálon](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. <span data-ttu-id="cde3c-119">A **új modell**, írja be a modell neve, majd válassza ki egy adatforrást.</span><span class="sxs-lookup"><span data-stu-id="cde3c-119">In **New model**, type a model name, and then select a data source.</span></span>

    ![Azure-portálon az új modell párbeszédpanel](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. <span data-ttu-id="cde3c-121">A **Connect**, adja meg a hello kapcsolat tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="cde3c-121">In **Connect**, enter hello connection properties.</span></span> <span data-ttu-id="cde3c-122">Felhasználónév és jelszó SQL Server fióknak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cde3c-122">Username and password must be a SQL Server account.</span></span>

     ![Csatlakozás Azure-portálon párbeszédpanel](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. <span data-ttu-id="cde3c-124">A **táblák és nézetek**, jelölje ki a hello táblák tooinclude a modell, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cde3c-124">In **Tables and views**, select hello tables tooinclude in your model, and then click **Create**.</span></span> <span data-ttu-id="cde3c-125">Egy kulcspár rendelkező táblák közötti kapcsolatok jönnek létre automatikusan.</span><span class="sxs-lookup"><span data-stu-id="cde3c-125">Relationships are created automatically between tables with a key pair.</span></span>

     ![Válassza ki a táblák és nézetek](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

<span data-ttu-id="cde3c-127">Az új modell jelenik meg a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="cde3c-127">Your new model appears in your browser.</span></span> <span data-ttu-id="cde3c-128">Itt a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="cde3c-128">From here, you can:</span></span>   

- <span data-ttu-id="cde3c-129">Query modelladatok húzzon mezőket toohello Lekérdezéstervező, és vegye fel a szűrők.</span><span class="sxs-lookup"><span data-stu-id="cde3c-129">Query model data by dragging fields toohello query designer and adding filters.</span></span>
- <span data-ttu-id="cde3c-130">Hozzon létre új mértékek táblában.</span><span class="sxs-lookup"><span data-stu-id="cde3c-130">Create new measures in tables.</span></span>
- <span data-ttu-id="cde3c-131">Szerkessze a modell metaadatainak hello json-szerkesztő használatával.</span><span class="sxs-lookup"><span data-stu-id="cde3c-131">Edit model metadata by using hello json editor.</span></span>
- <span data-ttu-id="cde3c-132">Nyissa meg a hello modellt a Visual Studio (SSDT), a Power BI Desktop vagy az Excel.</span><span class="sxs-lookup"><span data-stu-id="cde3c-132">Open hello model in Visual Studio (SSDT), Power BI Desktop, or Excel.</span></span>

![Válassza ki a táblák és nézetek](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> <span data-ttu-id="cde3c-134">Modell metaadatainak szerkesztése, illetve a böngészőben új mértékek létrehozása az Azure-ban e módosítások tooyour modell menti.</span><span class="sxs-lookup"><span data-stu-id="cde3c-134">When you edit model metadata or create new measures in your browser, you're saving those changes tooyour model in Azure.</span></span> <span data-ttu-id="cde3c-135">Ha még dolgozik a modell SSDT, a Power BI Desktop vagy az Excel, a modell szinkronban kérheti le.</span><span class="sxs-lookup"><span data-stu-id="cde3c-135">If you're also working on your model in SSDT, Power BI Desktop, or Excel, your model can get out of sync.</span></span>


## <a name="next-steps"></a><span data-ttu-id="cde3c-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cde3c-136">Next steps</span></span> 
[<span data-ttu-id="cde3c-137">Adatbázis-szerepkörök és a felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="cde3c-137">Manage database roles and users</span></span>](analysis-services-database-users.md)  
[<span data-ttu-id="cde3c-138">Kapcsolódás Excellel</span><span class="sxs-lookup"><span data-stu-id="cde3c-138">Connect with Excel</span></span>](analysis-services-connect-excel.md)  


