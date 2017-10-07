---
title: "Event Hubs rögzítése aaaAzure portálon keresztül engedélyezése |} Microsoft Docs"
description: "Hello Azure-portál használatával hello Event Hubs rögzítése funkció engedélyezéséhez."
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
ms.openlocfilehash: 27c7528552c497a4d98873a22bd56a991c66247c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-event-hubs-capture-using-hello-azure-portal"></a><span data-ttu-id="8fd13-103">Engedélyezze az Event Hubs rögzítése hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="8fd13-103">Enable Event Hubs Capture using hello Azure portal</span></span>

<span data-ttu-id="8fd13-104">Létrehozáskor hello event hub hello segítségével konfigurálhatja a rögzítési [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8fd13-104">You can configure Capture at hello event hub creation time using hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8fd13-105">Akkor is, vagy rögzítési hello adatok tooan Azure [Blob-tároló](https://azure.microsoft.com/services/storage/blobs/) tároló vagy tooan [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) fiók.</span><span class="sxs-lookup"><span data-stu-id="8fd13-105">You can either capture hello data tooan Azure [Blob storage](https://azure.microsoft.com/services/storage/blobs/) container, or tooan [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) account.</span></span>

## <a name="capture-data-tooan-azure-storage-account"></a><span data-ttu-id="8fd13-106">Adatok tooan Azure Storage-fiókjának rögzítése</span><span class="sxs-lookup"><span data-stu-id="8fd13-106">Capture data tooan Azure Storage account</span></span>  

<span data-ttu-id="8fd13-107">Amikor létrehoz egy eseményközpontot, rögzítési hello kattintva engedélyezheti **a** hello gombjára **Eseményközpont létrehozása** portál panel.</span><span class="sxs-lookup"><span data-stu-id="8fd13-107">When you create an event hub, you can enable Capture by clicking hello **On** button in hello **Create Event Hub** portal blade.</span></span> <span data-ttu-id="8fd13-108">Majd adja meg a Tárfiók és tároló kattintva **Azure Storage** a hello **rögzítése szolgáltató** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="8fd13-108">You then specify a Storage Account and container by clicking **Azure Storage** in hello **Capture Provider** box.</span></span> <span data-ttu-id="8fd13-109">Mivel az Event Hubs rögzítése használja a szolgáltatások közötti hitelesítés tárolóval, nem kell toospecify egy tárolási kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8fd13-109">Because Event Hubs Capture uses service-to-service authentication with storage, you do not need toospecify a storage connection string.</span></span> <span data-ttu-id="8fd13-110">hello erőforrás objektumválasztó hello erőforrás URI azonosítója a tárfiók automatikusan kijelöli.</span><span class="sxs-lookup"><span data-stu-id="8fd13-110">hello resource picker selects hello resource URI for your storage account automatically.</span></span> <span data-ttu-id="8fd13-111">Az Azure Resource Manager használatakor explicit módon meg kell adnia ezt az URI-t karakterláncként.</span><span class="sxs-lookup"><span data-stu-id="8fd13-111">If you use Azure Resource Manager, you must supply this URI explicitly as a string.</span></span>

<span data-ttu-id="8fd13-112">időkerete hello alapértelmezett érték 5 perc.</span><span class="sxs-lookup"><span data-stu-id="8fd13-112">hello default time window is 5 minutes.</span></span> <span data-ttu-id="8fd13-113">minimális hello értéke-1, maximális 15 hello.</span><span class="sxs-lookup"><span data-stu-id="8fd13-113">hello minimum value is 1, hello maximum 15.</span></span> <span data-ttu-id="8fd13-114">Hello **mérete** ablak tartománnyal rendelkező 10 – 500 MB.</span><span class="sxs-lookup"><span data-stu-id="8fd13-114">hello **Size** window has a range of 10-500 MB.</span></span>

![][1]

## <a name="capture-data-tooan-azure-data-lake-store-account"></a><span data-ttu-id="8fd13-115">Rögzítése adatok tooan Azure Data Lake Store-fiók</span><span class="sxs-lookup"><span data-stu-id="8fd13-115">Capture data tooan Azure Data Lake Store account</span></span>

<span data-ttu-id="8fd13-116">toocapture adatok tooAzure Data Lake Store, létrehozhat egy Data Lake Store-fiókot, és az eseményközpontok:</span><span class="sxs-lookup"><span data-stu-id="8fd13-116">toocapture data tooAzure Data Lake Store, you create a Data Lake Store account, and an event hub:</span></span>

### <a name="create-an-azure-data-lake-store-account-and-folders"></a><span data-ttu-id="8fd13-117">Azure Data Lake Store-fiók és -mappák létrehozása</span><span class="sxs-lookup"><span data-stu-id="8fd13-117">Create an Azure Data Lake Store account and folders</span></span>

1. <span data-ttu-id="8fd13-118">Hozzon létre egy Data Lake Store-fiókot, hello utasításait követve [Ismerkedés az Azure Data Lake Store használatának hello Azure-portálon](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8fd13-118">Create a Data Lake Store account, following hello instructions in [Get started with Azure Data Lake Store using hello Azure portal](../data-lake-store/data-lake-store-get-started-portal.md).</span></span> 
2. <span data-ttu-id="8fd13-119">Hozzon létre egy mappát, ezzel a fiókkal, hello hello utasításait követve [mappák létrehozása az Azure Data Lake Store-fiók](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) szakasz.</span><span class="sxs-lookup"><span data-stu-id="8fd13-119">Create a folder under this account, following hello instructions in hello [Create folders in Azure Data Lake Store account](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) section.</span></span>
3. <span data-ttu-id="8fd13-120">Hello Data Lake Store-fiók panelen kattintson a **adatkezelő**.</span><span class="sxs-lookup"><span data-stu-id="8fd13-120">In hello Data Lake Store account blade, click **Data Explorer**.</span></span>
4. <span data-ttu-id="8fd13-121">Kattintson a **Hozzáférés** elemre.</span><span class="sxs-lookup"><span data-stu-id="8fd13-121">Click **Access**.</span></span>
5. <span data-ttu-id="8fd13-122">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="8fd13-122">Click **Add**.</span></span>
6. <span data-ttu-id="8fd13-123">A hello **Keresés név vagy e-mail** mezőbe írja be **Microsoft.EventHubs** , és válassza ki ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="8fd13-123">In hello **Search by name or email** box type **Microsoft.EventHubs** and then select this option.</span></span> 
7. <span data-ttu-id="8fd13-124">Hello **engedélyek** lap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8fd13-124">hello **Permissions** tab appears.</span></span> <span data-ttu-id="8fd13-125">Hello engedélyek beállítása a hello a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="8fd13-125">Set hello permissions as shown in hello following figure:</span></span>

    ![][6]

8. <span data-ttu-id="8fd13-126">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8fd13-126">Click **OK**.</span></span>
9. <span data-ttu-id="8fd13-127">Most hozzon létre egy mappát hello gyökérmappában toohello Célmappa tallózása, majd kattintson a hello mappa neve.</span><span class="sxs-lookup"><span data-stu-id="8fd13-127">Now, create a folder in hello root folder by browsing toohello target folder and clicking on hello folder name.</span></span>
10. <span data-ttu-id="8fd13-128">Kattintson a **Hozzáférés** elemre.</span><span class="sxs-lookup"><span data-stu-id="8fd13-128">Click **Access**.</span></span>
11. <span data-ttu-id="8fd13-129">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="8fd13-129">Click **Add**.</span></span>
12. <span data-ttu-id="8fd13-130">A hello **Keresés név vagy e-mail** mezőbe írja be **Microsoft.EventHubs** , és válassza ki ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="8fd13-130">In hello **Search by name or email** box type **Microsoft.EventHubs** and then select this option.</span></span>
13. <span data-ttu-id="8fd13-131">Hello **engedélyek** lap jelenik meg újra.</span><span class="sxs-lookup"><span data-stu-id="8fd13-131">hello **Permissions** tab appears again.</span></span> <span data-ttu-id="8fd13-132">Hello engedélyek beállítása a hello a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="8fd13-132">Set hello permissions as shown in hello following figure:</span></span>

    ![][5]

### <a name="create-an-event-hub"></a><span data-ttu-id="8fd13-133">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="8fd13-133">Create an event hub</span></span>

1. <span data-ttu-id="8fd13-134">Vegye figyelembe, hogy hello eseményközpont kell lennie a hello hello Azure Data Lake Store imént létrehozott, azonos Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8fd13-134">Note that hello event hub must be in hello same Azure subscription as hello Azure Data Lake Store you just created.</span></span> <span data-ttu-id="8fd13-135">Hozzon létre hello eseményközpont, hello kattintva **a** gombra kattint, a **rögzítése** a hello **Eseményközpont létrehozása** portál panel.</span><span class="sxs-lookup"><span data-stu-id="8fd13-135">Create hello event hub, clicking hello **On** button under **Capture** in hello **Create Event Hub** portal blade.</span></span> 
2. <span data-ttu-id="8fd13-136">A hello **Eseményközpont létrehozása** portál panel, jelölje be **Azure Data Lake Store** a hello **rögzítése szolgáltató** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="8fd13-136">In hello **Create Event Hub** portal blade, select **Azure Data Lake Store** from hello **Capture Provider** box.</span></span>
3. <span data-ttu-id="8fd13-137">A **válasszon Data Lake Store**, adja meg a korábban, és hello létrehozott Data Lake Store-fiókot hello **elérési utat a Data Lake** mezőbe írja be a hello elérési toohello adatok létrehozott mappába.</span><span class="sxs-lookup"><span data-stu-id="8fd13-137">In **Select Data Lake Store**, specify hello Data Lake Store account you created previously, and in hello **Data Lake Path** field, enter hello path toohello data folder you created.</span></span>

    ![][3]

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a><span data-ttu-id="8fd13-138">A Capture hozzáadása vagy konfigurálása egy meglévő eseményközponton</span><span class="sxs-lookup"><span data-stu-id="8fd13-138">Add or configure Capture on an existing event hub</span></span>

<span data-ttu-id="8fd13-139">A Capture-t olyan meglévő eseményközpontokon konfigurálhatja, amelyek az Event Hubs-névterekben találhatók.</span><span class="sxs-lookup"><span data-stu-id="8fd13-139">You can configure Capture on existing event hubs that are in Event Hubs namespaces.</span></span> <span data-ttu-id="8fd13-140">tooenable egy meglévő eseményközpont, vagy toochange rögzíti a rögzítési beállításokat, kattintson a hello névtér tooload hello **Essentials** panelen, majd kattintson a hello eseményközpont, amelynek szeretné, hogy tooenable vagy hello rögzítési beállítás módosításával.</span><span class="sxs-lookup"><span data-stu-id="8fd13-140">tooenable Capture on an existing event hub, or toochange your Capture settings, click hello namespace tooload hello **Essentials** blade, then click hello event hub for which you want tooenable or change hello Capture setting.</span></span> <span data-ttu-id="8fd13-141">Végül kattintson a hello **tulajdonságok** hello szakasza panel megnyitásához, és hello rögzítési beállításait, majd szerkessze a hello a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="8fd13-141">Finally, click hello **Properties** section of hello open blade and then edit hello Capture settings, as shown in hello following figures:</span></span>

### <a name="azure-blob-storage"></a><span data-ttu-id="8fd13-142">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="8fd13-142">Azure Blob Storage</span></span>

![][2]

### <a name="azure-data-lake-store"></a><span data-ttu-id="8fd13-143">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="8fd13-143">Azure Data Lake Store</span></span>

![][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png
[5]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture5.png
[6]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture6.png

## <a name="next-steps"></a><span data-ttu-id="8fd13-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8fd13-144">Next steps</span></span>

<span data-ttu-id="8fd13-145">Az Azure Resource Manager-sablonok használatával is konfigurálhatja az Event Hubs Capture-t.</span><span class="sxs-lookup"><span data-stu-id="8fd13-145">You can also configure Event Hubs Capture using Azure Resource Manager templates.</span></span> <span data-ttu-id="8fd13-146">További információkért lásd: [Rögzítés funkció engedélyezése az Azure Resource Manager-sablonjának használatával](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).</span><span class="sxs-lookup"><span data-stu-id="8fd13-146">For more information, see [Enable Capture using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).</span></span>
