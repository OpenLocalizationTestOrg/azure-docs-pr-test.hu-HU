---
title: "a Machine Learning webszolgáltatásba az Excelből aaaConsume |} Microsoft Docs"
description: "Az Azure Machine Learning webszolgáltatásba az Excelből felhasználása"
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
ms.assetid: 3f3cdd2f-1816-487e-ab78-530e01e9788f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/13/2017
ms.author: tedway
ms.openlocfilehash: e2e8bbf7ba75b6618a0285539555ce175ec03c1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="consuming-an-azure-machine-learning-web-service-from-excel"></a><span data-ttu-id="92c6f-103">Azure Machine Learning webszolgáltatások használata az Excel programból</span><span class="sxs-lookup"><span data-stu-id="92c6f-103">Consuming an Azure Machine Learning Web Service from Excel</span></span>
 <span data-ttu-id="92c6f-104">Az Azure Machine Learning Studio segítségével könnyen toocall webszolgáltatások közvetlenül az Excelből hello kell toowrite kódok nélkül.</span><span class="sxs-lookup"><span data-stu-id="92c6f-104">Azure Machine Learning Studio makes it easy toocall web services directly from Excel without hello need toowrite any code.</span></span>

<span data-ttu-id="92c6f-105">Ha az Excel 2013 (vagy újabb verzió) vagy az Excel Online, akkor azt javasoljuk, hogy használja-e hello Excel [Excel-bővítmény](machine-learning-excel-add-in-for-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="92c6f-105">If you are using Excel 2013 (or later) or Excel Online, then we recommend that you use hello Excel [Excel add-in](machine-learning-excel-add-in-for-web-services.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="steps"></a><span data-ttu-id="92c6f-106">Lépések</span><span class="sxs-lookup"><span data-stu-id="92c6f-106">Steps</span></span>
<span data-ttu-id="92c6f-107">A webszolgáltatás közzététele.</span><span class="sxs-lookup"><span data-stu-id="92c6f-107">Publish a web service.</span></span> <span data-ttu-id="92c6f-108">[Ezen a lapon](machine-learning-walkthrough-5-publish-web-service.md) ismerteti, hogyan toodo azt.</span><span class="sxs-lookup"><span data-stu-id="92c6f-108">[This page](machine-learning-walkthrough-5-publish-web-service.md) explains how toodo it.</span></span> <span data-ttu-id="92c6f-109">Hello Excel-munkafüzet funkció jelenleg csak egy egyetlen kimeneti (Ez azt jelenti, hogy egy pontozási egycímkés) rendelkező kérelem/válasz szolgáltatásokat támogatott.</span><span class="sxs-lookup"><span data-stu-id="92c6f-109">Currently hello Excel workbook feature is only supported for Request/Response services that have a single output (that is, a single scoring label).</span></span> 

<span data-ttu-id="92c6f-110">Miután egy webszolgáltatás, kattintson a hello **WEBSZOLGÁLTATÁSOK** hello bal oldali hello Studio szakaszt, és válassza ki az Excelből hello web service tooconsume.</span><span class="sxs-lookup"><span data-stu-id="92c6f-110">Once you have a web service, click on hello **WEB SERVICES** section on hello left of hello studio, and then select hello web service tooconsume from Excel.</span></span>

<span data-ttu-id="92c6f-111">**Klasszikus webszolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="92c6f-111">**Classic Web Service**</span></span>

1. <span data-ttu-id="92c6f-112">A hello **IRÁNYÍTÓPULT** hello webszolgáltatás egy sort hello lapján **kérelem/válasz** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="92c6f-112">On hello **DASHBOARD** tab for hello web service is a row for hello **REQUEST/RESPONSE** service.</span></span> <span data-ttu-id="92c6f-113">Ha ez a szolgáltatás egyetlen kimeneti rendelkezett, megjelennie hello **töltse le az Excel-munkafüzet** sorhoz hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="92c6f-113">If this service had a single output, you should see hello **Download Excel Workbook** link in that row.</span></span>
   
    ![][1]
2. <span data-ttu-id="92c6f-114">Kattintson a **töltse le az Excel-munkafüzet**.</span><span class="sxs-lookup"><span data-stu-id="92c6f-114">Click on **Download Excel Workbook**.</span></span>

<span data-ttu-id="92c6f-115">**Új webszolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="92c6f-115">**New Web Service**</span></span>

1. <span data-ttu-id="92c6f-116">Hello Azure Machine Learning webszolgáltatás portálon, válassza ki a **felhasználás**.</span><span class="sxs-lookup"><span data-stu-id="92c6f-116">In hello Azure Machine Learning Web Service portal, select **Consume**.</span></span>
2. <span data-ttu-id="92c6f-117">A hello felhasználás lap hello **webes szolgáltatás felhasználásához beállításai** területen kattintson a hello Excel ikonra.</span><span class="sxs-lookup"><span data-stu-id="92c6f-117">On hello Consume page, in hello **Web service consumption options** section, click hello Excel icon.</span></span>

<span data-ttu-id="92c6f-118">**Hello munkafüzet használatát**</span><span class="sxs-lookup"><span data-stu-id="92c6f-118">**Using hello workbook**</span></span>

1. <span data-ttu-id="92c6f-119">Nyissa meg hello munkafüzet.</span><span class="sxs-lookup"><span data-stu-id="92c6f-119">Open hello workbook.</span></span>
2. <span data-ttu-id="92c6f-120">Biztonsági figyelmeztetés jelenik meg; Kattintson a hello **szerkesztésének engedélyezése** gombra.</span><span class="sxs-lookup"><span data-stu-id="92c6f-120">A Security Warning appears; click on hello **Enable Editing** button.</span></span>
   
    ![][2]
3. <span data-ttu-id="92c6f-121">Biztonsági figyelmeztetés jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="92c6f-121">A Security Warning appears.</span></span> <span data-ttu-id="92c6f-122">Kattintson a hello **tartalom engedélyezése** toorun makrók a munkalapon gombra.</span><span class="sxs-lookup"><span data-stu-id="92c6f-122">Click on hello **Enable Content** button toorun macros on your spreadsheet.</span></span>
   
    ![][3]
4. <span data-ttu-id="92c6f-123">Ha makrók engedélyezve vannak, egy tábla jön létre.</span><span class="sxs-lookup"><span data-stu-id="92c6f-123">Once macros are enabled, a table is generated.</span></span> <span data-ttu-id="92c6f-124">Oszlopok kék is szükséges hello bevitt RR-EKET webes szolgáltatás, vagy **paraméterek**.</span><span class="sxs-lookup"><span data-stu-id="92c6f-124">Columns in blue are required as input into hello RRS web service, or **PARAMETERS**.</span></span> <span data-ttu-id="92c6f-125">Vegye figyelembe a hello RR-EKET szolgáltatás hello kimenete **ELŐREJELZETT ÉRTÉKEKET** zöld.</span><span class="sxs-lookup"><span data-stu-id="92c6f-125">Note hello output of hello RRS service, **PREDICTED VALUES** in green.</span></span> <span data-ttu-id="92c6f-126">Minden oszlop egy adott sor kitöltődnek, hello munkafüzet automatikusan meghívja a pontozási API hello és hello eredmények program pontozza a mennyiségeket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="92c6f-126">When all columns for a given row are filled, hello workbook automatically calls hello scoring API, and displays hello scored results.</span></span>
   
    ![][4]
5. <span data-ttu-id="92c6f-127">tooscore több mint egy olyan sor, kitöltés hello második sor adatok és hello előre meghatározott értékek előállítása.</span><span class="sxs-lookup"><span data-stu-id="92c6f-127">tooscore more than one row, fill hello second row with data and hello predicted values are produced.</span></span> <span data-ttu-id="92c6f-128">Több sort is beillesztheti egyszerre.</span><span class="sxs-lookup"><span data-stu-id="92c6f-128">You can even paste several rows at once.</span></span>

<span data-ttu-id="92c6f-129">(Diagramokat, energiagazdálkodási térkép, feltételes formázási stb) hello Excel szolgáltatások bármelyikét használhatja hello előre meghatározott értékek toohelp hello adatainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="92c6f-129">You can use any of hello Excel features (graphs, power map, conditional formatting, etc.) with hello predicted values toohelp visualize hello data.</span></span>    

## <a name="sharing-your-workbook"></a><span data-ttu-id="92c6f-130">A munkafüzet megosztása</span><span class="sxs-lookup"><span data-stu-id="92c6f-130">Sharing your workbook</span></span>
<span data-ttu-id="92c6f-131">A hello makrók toowork az API-kulcs hello számolótábla részének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="92c6f-131">For hello macros toowork, your API Key must be part of hello spreadsheet.</span></span> <span data-ttu-id="92c6f-132">Ez azt jelenti, hogy ossza meg hello munkafüzetbe csak a megbízható entitások/személyeknek.</span><span class="sxs-lookup"><span data-stu-id="92c6f-132">That means that you should share hello workbook only with entities/individuals you trust.</span></span>

## <a name="automatic-updates"></a><span data-ttu-id="92c6f-133">Automatikus frissítések</span><span class="sxs-lookup"><span data-stu-id="92c6f-133">Automatic updates</span></span>
<span data-ttu-id="92c6f-134">Az RRS kezdeményezték a két ezekben a helyzetekben:</span><span class="sxs-lookup"><span data-stu-id="92c6f-134">An RRS call is made in these two situations:</span></span>

1. <span data-ttu-id="92c6f-135">először egy sor tartalma az összes hello a **paraméterek**</span><span class="sxs-lookup"><span data-stu-id="92c6f-135">hello first time a row has content in all of its **PARAMETERS**</span></span>
2. <span data-ttu-id="92c6f-136">Amikor bármelyik hello **paraméterek** kellett az összes sor változásairól a **paraméterek** megadott.</span><span class="sxs-lookup"><span data-stu-id="92c6f-136">Any time any of hello **PARAMETERS** changes in a row that had all of its **PARAMETERS** entered.</span></span>

[1]: ./media/machine-learning-consuming-from-excel/excellink.png
[2]: ./media/machine-learning-consuming-from-excel/enableeditting.png
[3]: ./media/machine-learning-consuming-from-excel/enablecontent.png
[4]: ./media/machine-learning-consuming-from-excel/sampletable.png
