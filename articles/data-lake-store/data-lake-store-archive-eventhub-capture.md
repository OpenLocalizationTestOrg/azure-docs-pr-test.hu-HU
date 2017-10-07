---
title: az Event Hubs az Azure Data Lake Store aaaCapture adatok |} Microsoft Docs
description: "Használja az Azure Data Lake Store toocapture Eseményközpontokból származó adatokat"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 09b17bd0b47043bd2c83dba72c01a8064f206a0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-lake-store-toocapture-data-from-event-hubs"></a><span data-ttu-id="4d5be-103">Használja az Azure Data Lake Store toocapture Eseményközpontokból származó adatokat</span><span class="sxs-lookup"><span data-stu-id="4d5be-103">Use Azure Data Lake Store toocapture data from Event Hubs</span></span>

<span data-ttu-id="4d5be-104">Ismerje meg, hogyan toouse Azure Data Lake Store toocapture adatokat az Azure Event Hubs fogadja.</span><span class="sxs-lookup"><span data-stu-id="4d5be-104">Learn how toouse Azure Data Lake Store toocapture data received by Azure Event Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4d5be-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4d5be-105">Prerequisites</span></span>

* <span data-ttu-id="4d5be-106">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-106">**An Azure subscription**.</span></span> <span data-ttu-id="4d5be-107">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4d5be-107">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="4d5be-108">**Egy Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-108">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="4d5be-109">Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4d5be-109">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md).</span></span>

*  <span data-ttu-id="4d5be-110">**Az Event Hubs névtér**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-110">**An Event Hubs namespace**.</span></span> <span data-ttu-id="4d5be-111">Útmutatásért lásd: [az Event Hubs-névtér létrehozása](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace).</span><span class="sxs-lookup"><span data-stu-id="4d5be-111">For instructions, see [Create an Event Hubs namespace](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace).</span></span> <span data-ttu-id="4d5be-112">Gondoskodjon arról, hogy hello Data Lake Store-fiók és hello Event Hubs névtér hello azonos Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="4d5be-112">Make sure hello Data Lake Store account and hello Event Hubs namespace are in hello same Azure subscription.</span></span>


## <a name="assign-permissions-tooevent-hubs"></a><span data-ttu-id="4d5be-113">Engedélyek hozzárendelése tooEvent hubok</span><span class="sxs-lookup"><span data-stu-id="4d5be-113">Assign permissions tooEvent Hubs</span></span>

<span data-ttu-id="4d5be-114">Ebben a szakaszban hozzon létre egy mappát hello fiókon belül, ahol azt szeretné, hogy toocapture hello Eseményközpontokból származó adatokat.</span><span class="sxs-lookup"><span data-stu-id="4d5be-114">In this section, you create a folder within hello account where you want toocapture hello data from Event Hubs.</span></span> <span data-ttu-id="4d5be-115">Akkor is engedélyeket tooEvent hubok, hogy adatokat írhat be egy Data Lake Store-fiókba.</span><span class="sxs-lookup"><span data-stu-id="4d5be-115">You also assign permissions tooEvent Hubs so that it can write data into a Data Lake Store account.</span></span> 

1. <span data-ttu-id="4d5be-116">Ha szeretné, hogy toocapture Eseményközpontokból származó adatokat, és kattintson a hello Data Lake Store-fiók megnyitása **adatkezelő**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-116">Open hello Data Lake Store account where you want toocapture data from Event Hubs and then click on **Data Explorer**.</span></span>

    <span data-ttu-id="4d5be-117">![Data Lake Store adatkezelő](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Data Lake Store adatkezelő")</span><span class="sxs-lookup"><span data-stu-id="4d5be-117">![Data Lake Store data explorer](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Data Lake Store data explorer")</span></span>

2.  <span data-ttu-id="4d5be-118">Kattintson a **új mappa** és írjon be egy nevet a mappára, ahol toocapture hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="4d5be-118">Click **New Folder** and then enter a name for folder where you want toocapture hello data.</span></span>

    <span data-ttu-id="4d5be-119">![Hozzon létre egy új mappát a Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "hozzon létre egy új mappát a Data Lake Store-ban")</span><span class="sxs-lookup"><span data-stu-id="4d5be-119">![Create a new folder in Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "Create a new folder in Data Lake Store")</span></span>

3. <span data-ttu-id="4d5be-120">A Data Lake Store hello hello gyökerében engedélyeket rendelhet.</span><span class="sxs-lookup"><span data-stu-id="4d5be-120">Assign permissions at hello root of hello Data Lake Store.</span></span> 

    <span data-ttu-id="4d5be-121">a.</span><span class="sxs-lookup"><span data-stu-id="4d5be-121">a.</span></span> <span data-ttu-id="4d5be-122">Kattintson a **adatkezelő**, jelölje ki a Data Lake Store-fiók hello hello gyökérkönyvtárában, és kattintson **hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-122">Click **Data Explorer**, select hello root of hello Data Lake Store account, and then click **Access**.</span></span>

    <span data-ttu-id="4d5be-123">![Engedélyeket rendelhet a Data Lake Store legfelső szintű](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "engedélyeket rendelhet a Data Lake Store gyökér")</span><span class="sxs-lookup"><span data-stu-id="4d5be-123">![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "Assign permissions for Data Lake Store root")</span></span>

    <span data-ttu-id="4d5be-124">b.</span><span class="sxs-lookup"><span data-stu-id="4d5be-124">b.</span></span> <span data-ttu-id="4d5be-125">A **hozzáférés**, kattintson a **Hozzáadás**, kattintson a **felhasználó vagy csoport kiválasztása**, majd keresse meg a `Microsoft.EventHubs`.</span><span class="sxs-lookup"><span data-stu-id="4d5be-125">Under **Access**, click **Add**, click **Select User or Group**, and then search for `Microsoft.EventHubs`.</span></span> 

    <span data-ttu-id="4d5be-126">![Engedélyeket rendelhet a Data Lake Store legfelső szintű](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "engedélyeket rendelhet a Data Lake Store gyökér")</span><span class="sxs-lookup"><span data-stu-id="4d5be-126">![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Assign permissions for Data Lake Store root")</span></span>
    
    <span data-ttu-id="4d5be-127">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4d5be-127">Click **Select**.</span></span>

    <span data-ttu-id="4d5be-128">c.</span><span class="sxs-lookup"><span data-stu-id="4d5be-128">c.</span></span> <span data-ttu-id="4d5be-129">A **engedélyek hozzárendelése**, kattintson a **Select engedélyeket**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-129">Under **Assign Permissions**, click **Select Permissions**.</span></span> <span data-ttu-id="4d5be-130">Állítsa be **engedélyek** túl**Execute**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-130">Set **Permissions** too**Execute**.</span></span> <span data-ttu-id="4d5be-131">Állítsa be **hozzáadása** túl**ezt a mappát, és minden gyermeknek**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-131">Set **Add to** too**This folder and all children**.</span></span> <span data-ttu-id="4d5be-132">Állítsa be **hozzáadni** túl**egy hozzáférési engedélybejegyzés és egy alapértelmezett engedélybejegyzés**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-132">Set **Add as** too**An access permission entry and a default permission entry**.</span></span>

    <span data-ttu-id="4d5be-133">![Engedélyeket rendelhet a Data Lake Store legfelső szintű](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "engedélyeket rendelhet a Data Lake Store gyökér")</span><span class="sxs-lookup"><span data-stu-id="4d5be-133">![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "Assign permissions for Data Lake Store root")</span></span>

    <span data-ttu-id="4d5be-134">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="4d5be-134">Click **OK**.</span></span>

4. <span data-ttu-id="4d5be-135">Engedélyeket rendelhet a hello célmappáját Data Lake Store-fiókjában toocapture adatokat.</span><span class="sxs-lookup"><span data-stu-id="4d5be-135">Assign permissions for hello folder under Data Lake Store account where you want toocapture data.</span></span>

    <span data-ttu-id="4d5be-136">a.</span><span class="sxs-lookup"><span data-stu-id="4d5be-136">a.</span></span> <span data-ttu-id="4d5be-137">Kattintson a **adatkezelő**, jelölje ki hello mappa hello Data Lake Store-fiókba, és kattintson a **hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-137">Click **Data Explorer**, select hello folder in hello Data Lake Store account, and then click **Access**.</span></span>

    <span data-ttu-id="4d5be-138">![Engedélyeket rendelhet a Data Lake Store-mappa](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "engedélyeket rendelhet a Data Lake Store-mappa")</span><span class="sxs-lookup"><span data-stu-id="4d5be-138">![Assign permissions for Data Lake Store folder](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "Assign permissions for Data Lake Store folder")</span></span>

    <span data-ttu-id="4d5be-139">b.</span><span class="sxs-lookup"><span data-stu-id="4d5be-139">b.</span></span> <span data-ttu-id="4d5be-140">A **hozzáférés**, kattintson a **Hozzáadás**, kattintson a **felhasználó vagy csoport kiválasztása**, majd keresse meg a `Microsoft.EventHubs`.</span><span class="sxs-lookup"><span data-stu-id="4d5be-140">Under **Access**, click **Add**, click **Select User or Group**, and then search for `Microsoft.EventHubs`.</span></span> 

    <span data-ttu-id="4d5be-141">![Engedélyeket rendelhet a Data Lake Store-mappa](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "engedélyeket rendelhet a Data Lake Store-mappa")</span><span class="sxs-lookup"><span data-stu-id="4d5be-141">![Assign permissions for Data Lake Store folder](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Assign permissions for Data Lake Store folder")</span></span>
    
    <span data-ttu-id="4d5be-142">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4d5be-142">Click **Select**.</span></span>

    <span data-ttu-id="4d5be-143">c.</span><span class="sxs-lookup"><span data-stu-id="4d5be-143">c.</span></span> <span data-ttu-id="4d5be-144">A **engedélyek hozzárendelése**, kattintson a **Select engedélyeket**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-144">Under **Assign Permissions**, click **Select Permissions**.</span></span> <span data-ttu-id="4d5be-145">Állítsa be **engedélyek** túl**Olvasás, írás,** és **Execute**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-145">Set **Permissions** too**Read, Write,** and **Execute**.</span></span> <span data-ttu-id="4d5be-146">Állítsa be **hozzáadása** túl**ezt a mappát, és minden gyermeknek**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-146">Set **Add to** too**This folder and all children**.</span></span> <span data-ttu-id="4d5be-147">Végezetül be **hozzáadni** túl**egy hozzáférési engedélybejegyzés és egy alapértelmezett engedélybejegyzés**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-147">Finally, set **Add as** too**An access permission entry and a default permission entry**.</span></span>

    <span data-ttu-id="4d5be-148">![Engedélyeket rendelhet a Data Lake Store-mappa](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "engedélyeket rendelhet a Data Lake Store-mappa")</span><span class="sxs-lookup"><span data-stu-id="4d5be-148">![Assign permissions for Data Lake Store folder](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "Assign permissions for Data Lake Store folder")</span></span>
    
    <span data-ttu-id="4d5be-149">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="4d5be-149">Click **OK**.</span></span> 

## <a name="configure-event-hubs-toocapture-data-toodata-lake-store"></a><span data-ttu-id="4d5be-150">Az Event Hubs toocapture tooData Lake adattárban konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4d5be-150">Configure Event Hubs toocapture data tooData Lake Store</span></span>

<span data-ttu-id="4d5be-151">Ebben a szakaszban egy Eseményközpontot, az Event Hubs névtéren belül hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4d5be-151">In this section, you create an Event Hub within an Event Hubs namespace.</span></span> <span data-ttu-id="4d5be-152">Konfigurálnia hello Eseményközpont toocapture adatok tooan Azure Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="4d5be-152">You also configure hello Event Hub toocapture data tooan Azure Data Lake Store account.</span></span> <span data-ttu-id="4d5be-153">Ez a szakasz azt feltételezi, hogy már létrehozta az Event Hubs névtér.</span><span class="sxs-lookup"><span data-stu-id="4d5be-153">This section assumes that you have already created an Event Hubs namespace.</span></span>

2. <span data-ttu-id="4d5be-154">A hello **áttekintése** hello Event Hubs névtér, panelén kattintson **+ Eseményközpont**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-154">From hello **Overview** pane of hello Event Hubs namespace, click **+ Event Hub**.</span></span>

    <span data-ttu-id="4d5be-155">![Eseményközpont létrehozása](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "Eseményközpont létrehozása")</span><span class="sxs-lookup"><span data-stu-id="4d5be-155">![Create Event Hub](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "Create Event Hub")</span></span>

3. <span data-ttu-id="4d5be-156">Adja meg a következő hello értékek tooconfigure Event Hubs toocapture tooData Lake adattárban.</span><span class="sxs-lookup"><span data-stu-id="4d5be-156">Provide hello following values tooconfigure Event Hubs toocapture data tooData Lake Store.</span></span>

    <span data-ttu-id="4d5be-157">![Eseményközpont létrehozása](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "Eseményközpont létrehozása")</span><span class="sxs-lookup"><span data-stu-id="4d5be-157">![Create Event Hub](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "Create Event Hub")</span></span>

    <span data-ttu-id="4d5be-158">a.</span><span class="sxs-lookup"><span data-stu-id="4d5be-158">a.</span></span> <span data-ttu-id="4d5be-159">Adja meg a hello Eseményközpont nevét.</span><span class="sxs-lookup"><span data-stu-id="4d5be-159">Provide a name for hello Event Hub.</span></span>
    
    <span data-ttu-id="4d5be-160">b.</span><span class="sxs-lookup"><span data-stu-id="4d5be-160">b.</span></span> <span data-ttu-id="4d5be-161">Ebben az oktatóanyagban beállítása **Partíciószámnak** és **üzenet megőrzési** toohello alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="4d5be-161">For this tutorial, set **Partition Count** and **Message Retention** toohello default values.</span></span>
    
    <span data-ttu-id="4d5be-162">c.</span><span class="sxs-lookup"><span data-stu-id="4d5be-162">c.</span></span> <span data-ttu-id="4d5be-163">Állítsa be **rögzítése** túl**a**.</span><span class="sxs-lookup"><span data-stu-id="4d5be-163">Set **Capture** too**On**.</span></span> <span data-ttu-id="4d5be-164">Set hello **időkerete** (milyen gyakran toocapture) és **méret ablakban** (adatok mérete toocapture).</span><span class="sxs-lookup"><span data-stu-id="4d5be-164">Set hello **Time Window** (how frequently toocapture) and **Size Window** (data size toocapture).</span></span> 
    
    <span data-ttu-id="4d5be-165">d.</span><span class="sxs-lookup"><span data-stu-id="4d5be-165">d.</span></span> <span data-ttu-id="4d5be-166">A **rögzítése szolgáltató**, jelölje be **Azure Data Lake Store** és hello válasszon hello Data Lake Store a korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="4d5be-166">For **Capture Provider**, select **Azure Data Lake Store** and hello select hello Data Lake Store you created earlier.</span></span> <span data-ttu-id="4d5be-167">A **elérési utat a Data Lake**, adja meg a Data Lake Store-fiók hello létrehozott hello mappa hello neve.</span><span class="sxs-lookup"><span data-stu-id="4d5be-167">For **Data Lake Path**, enter hello name of hello folder you created in hello Data Lake Store account.</span></span> <span data-ttu-id="4d5be-168">Csak akkor kell tooprovide hello relatív elérési út toohello mappát.</span><span class="sxs-lookup"><span data-stu-id="4d5be-168">You only need tooprovide hello relative path toohello folder.</span></span>

    <span data-ttu-id="4d5be-169">e.</span><span class="sxs-lookup"><span data-stu-id="4d5be-169">e.</span></span> <span data-ttu-id="4d5be-170">Hagyja hello **minta rögzítési neve formátumok** toohello alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="4d5be-170">Leave hello **Sample capture file name formats** toohello default value.</span></span> <span data-ttu-id="4d5be-171">Ez a beállítás szabályozza hello mappaszerkezet, amely hello rögzítési mappában jön létre.</span><span class="sxs-lookup"><span data-stu-id="4d5be-171">This option governs hello folder structure that is created under hello capture folder.</span></span>

    <span data-ttu-id="4d5be-172">f.</span><span class="sxs-lookup"><span data-stu-id="4d5be-172">f.</span></span> <span data-ttu-id="4d5be-173">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4d5be-173">Click **Create**.</span></span>

## <a name="test-hello-setup"></a><span data-ttu-id="4d5be-174">Teszt hello beállítása</span><span class="sxs-lookup"><span data-stu-id="4d5be-174">Test hello setup</span></span>

<span data-ttu-id="4d5be-175">Hello megoldás küldött adatok toohello Azure Event Hubs most tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="4d5be-175">You can now test hello solution by sending data toohello Azure Event Hub.</span></span> <span data-ttu-id="4d5be-176">Hajtsa végre a hello található utasítások segítségével: [tooAzure Event Hubs üzenetküldési](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).</span><span class="sxs-lookup"><span data-stu-id="4d5be-176">Follow hello instructions at [Send events tooAzure Event Hubs](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).</span></span> <span data-ttu-id="4d5be-177">Hello adatküldés elindítását követően adataira hello megjelennek a Data Lake Store megadott hello mappaszerkezet használatával.</span><span class="sxs-lookup"><span data-stu-id="4d5be-177">Once you start sending hello data, you see hello data reflected in Data Lake Store using hello folder structure you specified.</span></span> <span data-ttu-id="4d5be-178">Például láthatja a mappastruktúrát, ahogy az a következő képernyőkép a Data Lake Store-ban hello.</span><span class="sxs-lookup"><span data-stu-id="4d5be-178">For example, you see a folder structure, as shown in hello following screenshot, in your Data Lake Store.</span></span>

<span data-ttu-id="4d5be-179">![Az EventHub-adatokat a Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "minta az EventHub-adatok Data Lake Store-ban")</span><span class="sxs-lookup"><span data-stu-id="4d5be-179">![Sample EventHub data in Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "Sample EventHub data in Data Lake Store")</span></span>

> [!NOTE]
> <span data-ttu-id="4d5be-180">Akkor is, ha még nem rendelkezik az Event Hubsban érkező üzenetek, az Event Hubs hello Data Lake Store-fiók adatbázisba írja az üres fájlok csak hello fejlécekkel.</span><span class="sxs-lookup"><span data-stu-id="4d5be-180">Even if you do not have messages coming into Event Hubs, Event Hubs writes empty files with just hello headers into hello Data Lake Store account.</span></span> <span data-ttu-id="4d5be-181">hello fájlok írt: hello azonos hello Event Hubs létrehozásakor megadott időintervallumban.</span><span class="sxs-lookup"><span data-stu-id="4d5be-181">hello files are written at hello same time interval that you provided while creating hello Event Hubs.</span></span>
> 
>

## <a name="analyze-data-in-data-lake-store"></a><span data-ttu-id="4d5be-182">A Data Lake Store adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="4d5be-182">Analyze data in Data Lake Store</span></span>

<span data-ttu-id="4d5be-183">Ha hello adatok a Data Lake Store, futtathatja analitikai feladatok tooprocess és crunch hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="4d5be-183">Once hello data is in Data Lake Store, you can run analytical jobs tooprocess and crunch hello data.</span></span> <span data-ttu-id="4d5be-184">Lásd: [USQL Avro példa](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) hogyan toodo az Azure Data Lake Analytics használatával.</span><span class="sxs-lookup"><span data-stu-id="4d5be-184">See [USQL Avro Example](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) on how toodo this using Azure Data Lake Analytics.</span></span>
  

## <a name="see-also"></a><span data-ttu-id="4d5be-185">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4d5be-185">See also</span></span>
* [<span data-ttu-id="4d5be-186">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="4d5be-186">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="4d5be-187">Adatok másolása az Azure Storage Blobs tooData Lake Store-ból</span><span class="sxs-lookup"><span data-stu-id="4d5be-187">Copy data from Azure Storage Blobs tooData Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
