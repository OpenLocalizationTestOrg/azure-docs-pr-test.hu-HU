---
title: "Az Azure Event Hubs Capture engedélyezése a portálon keresztül | Microsoft Docs"
description: "Engedélyezze az Event Hubs Capture funkciót az Azure Portal használatával."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: 0cbf45dce922647f2996f929d2c7cf885a2f4db4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="enable-event-hubs-capture-using-the-azure-portal"></a><span data-ttu-id="8c45f-103">Az Event Hubs Capture engedélyezése az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="8c45f-103">Enable Event Hubs Capture using the Azure portal</span></span>

<span data-ttu-id="8c45f-104">A Capture-t konfigurálhatja az eseményközpont létrehozásakor az [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8c45f-104">You can configure Capture at the event hub creation time using the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8c45f-105">Az adatokat Azure [Blob Storage](https://azure.microsoft.com/services/storage/blobs/)-tárolóban vagy [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/)-fiókban is rögzítheti.</span><span class="sxs-lookup"><span data-stu-id="8c45f-105">You can either capture the data to an Azure [Blob storage](https://azure.microsoft.com/services/storage/blobs/) container, or to an [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) account.</span></span>

## <a name="capture-data-to-an-azure-storage-account"></a><span data-ttu-id="8c45f-106">Adatok rögzítése Azure Storage-fiókba</span><span class="sxs-lookup"><span data-stu-id="8c45f-106">Capture data to an Azure Storage account</span></span>  

<span data-ttu-id="8c45f-107">Amikor létrehoz egy eseményközpontot, rögzítési kattintva engedélyezheti a **a** gombra a **Eseményközpont létrehozása** portál panel.</span><span class="sxs-lookup"><span data-stu-id="8c45f-107">When you create an event hub, you can enable Capture by clicking the **On** button in the **Create Event Hub** portal blade.</span></span> <span data-ttu-id="8c45f-108">Ezután a **Capture-szolgáltató** mező **Azure Storage** gombjára kattintva adhatja meg a Storage-fiókot és -tárolót.</span><span class="sxs-lookup"><span data-stu-id="8c45f-108">You then specify a Storage Account and container by clicking **Azure Storage** in the **Capture Provider** box.</span></span> <span data-ttu-id="8c45f-109">Mivel az Event Hubs Capture szolgáltatások közötti hitelesítést használ a tárolóval, nem kell megadnia egy tárfiók kapcsolati sztringjét.</span><span class="sxs-lookup"><span data-stu-id="8c45f-109">Because Event Hubs Capture uses service-to-service authentication with storage, you do not need to specify a storage connection string.</span></span> <span data-ttu-id="8c45f-110">Az erőforrás-választó automatikusan kiválasztja az erőforrás URI-azonosítóját a tárfiókhoz.</span><span class="sxs-lookup"><span data-stu-id="8c45f-110">The resource picker selects the resource URI for your storage account automatically.</span></span> <span data-ttu-id="8c45f-111">Az Azure Resource Manager használatakor explicit módon meg kell adnia ezt az URI-t karakterláncként.</span><span class="sxs-lookup"><span data-stu-id="8c45f-111">If you use Azure Resource Manager, you must supply this URI explicitly as a string.</span></span>

<span data-ttu-id="8c45f-112">Az időkeret alapértelmezett értéke 5 perc.</span><span class="sxs-lookup"><span data-stu-id="8c45f-112">The default time window is 5 minutes.</span></span> <span data-ttu-id="8c45f-113">A minimális értéke 1, a maximális 15.</span><span class="sxs-lookup"><span data-stu-id="8c45f-113">The minimum value is 1, the maximum 15.</span></span> <span data-ttu-id="8c45f-114">A **Méret** ablak 10–500 MB tartománnyal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8c45f-114">The **Size** window has a range of 10-500 MB.</span></span>

![][1]

## <a name="capture-data-to-an-azure-data-lake-store-account"></a><span data-ttu-id="8c45f-115">Adatok rögzítése egy Azure Data Lake Store-fiókba</span><span class="sxs-lookup"><span data-stu-id="8c45f-115">Capture data to an Azure Data Lake Store account</span></span>

<span data-ttu-id="8c45f-116">Az adatok Azure Data Lake Store-ban történő rögzítéséhez létre kell hoznia egy Data Lake Store-fiókot és egy eseményközpontot:</span><span class="sxs-lookup"><span data-stu-id="8c45f-116">To capture data to Azure Data Lake Store, you create a Data Lake Store account, and an event hub:</span></span>

### <a name="create-an-azure-data-lake-store-account-and-folders"></a><span data-ttu-id="8c45f-117">Azure Data Lake Store-fiók és -mappák létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c45f-117">Create an Azure Data Lake Store account and folders</span></span>

1. <span data-ttu-id="8c45f-118">A Data Lake Store-fiók létrehozásához kövesse [Az Azure Data Lake Store használatának első lépései az Azure Portal használatával](../data-lake-store/data-lake-store-get-started-portal.md) című témakör utasításait.</span><span class="sxs-lookup"><span data-stu-id="8c45f-118">Create a Data Lake Store account, following the instructions in [Get started with Azure Data Lake Store using the Azure portal](../data-lake-store/data-lake-store-get-started-portal.md).</span></span> 
2. <span data-ttu-id="8c45f-119">Hozzon létre egy mappát ebben a fiókban a [Mappák létrehozása az Azure Data Lake Store-fiókban](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) szakasz útmutatásai szerint.</span><span class="sxs-lookup"><span data-stu-id="8c45f-119">Create a folder under this account, following the instructions in the [Create folders in Azure Data Lake Store account](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) section.</span></span>
3. <span data-ttu-id="8c45f-120">A Data Lake Store-fiók panelen kattintson a **adatkezelő**.</span><span class="sxs-lookup"><span data-stu-id="8c45f-120">In the Data Lake Store account blade, click **Data Explorer**.</span></span>
4. <span data-ttu-id="8c45f-121">Kattintson a **Hozzáférés** elemre.</span><span class="sxs-lookup"><span data-stu-id="8c45f-121">Click **Access**.</span></span>
5. <span data-ttu-id="8c45f-122">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="8c45f-122">Click **Add**.</span></span>
6. <span data-ttu-id="8c45f-123">A **Keresés név vagy e-mail-cím alapján** mezőbe írja be a **Microsoft.EventHubs** kifejezést, majd válassza ki ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="8c45f-123">In the **Search by name or email** box type **Microsoft.EventHubs** and then select this option.</span></span> 
7. <span data-ttu-id="8c45f-124">Megjelenik az **Engedélyek** lap.</span><span class="sxs-lookup"><span data-stu-id="8c45f-124">The **Permissions** tab appears.</span></span> <span data-ttu-id="8c45f-125">Állítsa be az engedélyeket az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="8c45f-125">Set the permissions as shown in the following figure:</span></span>

    ![][6]

8. <span data-ttu-id="8c45f-126">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8c45f-126">Click **OK**.</span></span>
9. <span data-ttu-id="8c45f-127">Most hozzon létre egy mappát a gyökérmappában. Ehhez navigáljon a célmappához, és kattintson a mappa nevére.</span><span class="sxs-lookup"><span data-stu-id="8c45f-127">Now, create a folder in the root folder by browsing to the target folder and clicking on the folder name.</span></span>
10. <span data-ttu-id="8c45f-128">Kattintson a **Hozzáférés** elemre.</span><span class="sxs-lookup"><span data-stu-id="8c45f-128">Click **Access**.</span></span>
11. <span data-ttu-id="8c45f-129">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="8c45f-129">Click **Add**.</span></span>
12. <span data-ttu-id="8c45f-130">A **Keresés név vagy e-mail-cím alapján** mezőbe írja be a **Microsoft.EventHubs** kifejezést, majd válassza ki ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="8c45f-130">In the **Search by name or email** box type **Microsoft.EventHubs** and then select this option.</span></span>
13. <span data-ttu-id="8c45f-131">Ekkor újra megjelenik az **Engedélyek** lap.</span><span class="sxs-lookup"><span data-stu-id="8c45f-131">The **Permissions** tab appears again.</span></span> <span data-ttu-id="8c45f-132">Állítsa be az engedélyeket az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="8c45f-132">Set the permissions as shown in the following figure:</span></span>

    ![][5]

### <a name="create-an-event-hub"></a><span data-ttu-id="8c45f-133">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c45f-133">Create an event hub</span></span>

1. <span data-ttu-id="8c45f-134">Vegye figyelembe, hogy az eseményközpontnak ugyanabban az Azure-előfizetésben kell lennie, mint amelyben az imént létrehozott Azure Data Lake Store van.</span><span class="sxs-lookup"><span data-stu-id="8c45f-134">Note that the event hub must be in the same Azure subscription as the Azure Data Lake Store you just created.</span></span> <span data-ttu-id="8c45f-135">Az event hubs gombra kattintva hozzon létre a **a** gombra kattint, a **rögzítése** a a **Eseményközpont létrehozása** portál panel.</span><span class="sxs-lookup"><span data-stu-id="8c45f-135">Create the event hub, clicking the **On** button under **Capture** in the **Create Event Hub** portal blade.</span></span> 
2. <span data-ttu-id="8c45f-136">Az a **Eseményközpont létrehozása** portál panel, jelölje be **Azure Data Lake Store** a a **rögzítése szolgáltató** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="8c45f-136">In the **Create Event Hub** portal blade, select **Azure Data Lake Store** from the **Capture Provider** box.</span></span>
3. <span data-ttu-id="8c45f-137">A **Data Lake Store kiválasztása** mezőben adja meg a korábban létrehozott Data Lake Store-fiókot, a **Data Lake elérési útja** mezőben pedig a létrehozott adatmappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="8c45f-137">In **Select Data Lake Store**, specify the Data Lake Store account you created previously, and in the **Data Lake Path** field, enter the path to the data folder you created.</span></span>

    ![][3]

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a><span data-ttu-id="8c45f-138">A Capture hozzáadása vagy konfigurálása egy meglévő eseményközponton</span><span class="sxs-lookup"><span data-stu-id="8c45f-138">Add or configure Capture on an existing event hub</span></span>

<span data-ttu-id="8c45f-139">A Capture-t olyan meglévő eseményközpontokon konfigurálhatja, amelyek az Event Hubs-névterekben találhatók.</span><span class="sxs-lookup"><span data-stu-id="8c45f-139">You can configure Capture on existing event hubs that are in Event Hubs namespaces.</span></span> <span data-ttu-id="8c45f-140">A Capture meglévő eseményközpontban történő engedélyezéséhez, vagy a Capture beállításainak módosításához kattintson a névtérre az **Alapvető szolgáltatások** panel betöltéséhez, majd kattintson arra az eseményközpontra, amelyet engedélyezni kíván, vagy amelyhez módosítani szeretné a Capture-beállítást.</span><span class="sxs-lookup"><span data-stu-id="8c45f-140">To enable Capture on an existing event hub, or to change your Capture settings, click the namespace to load the **Essentials** blade, then click the event hub for which you want to enable or change the Capture setting.</span></span> <span data-ttu-id="8c45f-141">Végül kattintson a **tulajdonságok** szakasz nyissa meg a penge és szerkessze a rögzítési beállítások, a következő ábrákon látható módon:</span><span class="sxs-lookup"><span data-stu-id="8c45f-141">Finally, click the **Properties** section of the open blade and then edit the Capture settings, as shown in the following figures:</span></span>

### <a name="azure-blob-storage"></a><span data-ttu-id="8c45f-142">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="8c45f-142">Azure Blob Storage</span></span>

![][2]

### <a name="azure-data-lake-store"></a><span data-ttu-id="8c45f-143">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="8c45f-143">Azure Data Lake Store</span></span>

![][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png
[5]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture5.png
[6]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture6.png

## <a name="next-steps"></a><span data-ttu-id="8c45f-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8c45f-144">Next steps</span></span>

<span data-ttu-id="8c45f-145">Az Azure Resource Manager-sablonok használatával is konfigurálhatja az Event Hubs Capture-t.</span><span class="sxs-lookup"><span data-stu-id="8c45f-145">You can also configure Event Hubs Capture using Azure Resource Manager templates.</span></span> <span data-ttu-id="8c45f-146">További információkért lásd: [Rögzítés funkció engedélyezése az Azure Resource Manager-sablonjának használatával](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).</span><span class="sxs-lookup"><span data-stu-id="8c45f-146">For more information, see [Enable Capture using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).</span></span>
