---
title: "Azure StorSimple adatokat kezelő felhasználói felületén aaaMicrosoft |} Microsoft Docs"
description: "Ismerteti, hogyan toouse StorSimple adatok Manager szolgáltatással felhasználói felület (magán előnézetben)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: b0ee12b3e495400b54e48eb1a98c68b1af2e5f7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-using-hello-storsimple-data-manager-service-ui-private-preview"></a><span data-ttu-id="22750-103">Kezelheti a hello StorSimple adatkezelő szolgáltatás felhasználói felületi (magán előnézetben)</span><span class="sxs-lookup"><span data-stu-id="22750-103">Manage using hello StorSimple Data Manager service UI (Private Preview)</span></span>

<span data-ttu-id="22750-104">Ez a cikk ismerteti, hogyan használhatja hello StorSimple adatokat kezelő felhasználói felületén tooperform adatok átalakítása hello StorSimple 8000 sorozat eszközeire levő adatok.</span><span class="sxs-lookup"><span data-stu-id="22750-104">This article explains how you can use hello StorSimple Data Manager UI tooperform data transformation on data residing on hello StorSimple 8000 series devices.</span></span> <span data-ttu-id="22750-105">hello átalakított adatok lehet majd használni például az Azure Media Services, az Azure HDInsight, az Azure Machine Learning és az Azure Search más Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="22750-105">hello transformed data can then be consumed by other Azure services such as Azure Media Services, Azure HDInsight, Azure Machine Learning, and Azure Search.</span></span> 


## <a name="use-storsimple-data-transformation"></a><span data-ttu-id="22750-106">StorSimple adatok átalakítással</span><span class="sxs-lookup"><span data-stu-id="22750-106">Use StorSimple Data Transformation</span></span>

<span data-ttu-id="22750-107">StorSimple adatkezelő hello Data Transformation példányosítható hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="22750-107">hello StorSimple Data Manager is hello resource within which Data Transformation can be instantiated.</span></span> <span data-ttu-id="22750-108">hello Data Transformation szolgáltatás lehetővé teszi a tárolt adatok mozgatása az Azure storage a StorSimple a helyszíni eszközök tooblobs.</span><span class="sxs-lookup"><span data-stu-id="22750-108">hello Data Transformation service lets you move data from your StorSimple on-premises device tooblobs in Azure storage.</span></span> <span data-ttu-id="22750-109">Emiatt munkafolyamat kell toospecify hello részletek a StorSimple eszköz és hello adatok részleteit, amelyet az toomove toohello tárfiók iránt.</span><span class="sxs-lookup"><span data-stu-id="22750-109">Hence, in workflow you need toospecify hello details about your StorSimple device and hello data of interest that you want toomove toohello storage account.</span></span>

### <a name="create-a-storsimple-data-manager-service"></a><span data-ttu-id="22750-110">StorSimple adatkezelő szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="22750-110">Create a StorSimple Data Manager service</span></span>

<span data-ttu-id="22750-111">Hajtsa végre a következő lépéseket toocreate StorSimple adatkezelő szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="22750-111">Perform hello following steps toocreate a StorSimple Data Manager service.</span></span>

1. <span data-ttu-id="22750-112">túl nyissa meg a StorSimple adatkezelő szolgáltatás toocreate[https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)</span><span class="sxs-lookup"><span data-stu-id="22750-112">toocreate a StorSimple Data Manager service, go too[https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)</span></span>

2. <span data-ttu-id="22750-113">Kattintson a hello  **+**  ikonra, és keresse a StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="22750-113">Click hello **+** icon and search for StorSimple Data Manager.</span></span> <span data-ttu-id="22750-114">Kattintson a StorSimple adatok Manager szolgáltatást, majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="22750-114">Click your StorSimple Data Manager service and then click **Create**.</span></span>

3. <span data-ttu-id="22750-115">Az előfizetés engedélyezve van ez a szolgáltatás létrehozása, ha a következő panel hello láthatja.</span><span class="sxs-lookup"><span data-stu-id="22750-115">If your subscription is enabled for creating this service, you see hello following blade.</span></span>

    ![StorSimple adatok kezelők erőforrás létrehozása](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. <span data-ttu-id="22750-117">Megadja a hello bemeneti adatokat, majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="22750-117">Enter hello inputs and click **Create**.</span></span> <span data-ttu-id="22750-118">a megadott hello egyet, amely a storage-fiókok és a StorSimple Manager szolgáltatás hello kell lennie.</span><span class="sxs-lookup"><span data-stu-id="22750-118">hello specified location should be hello one that houses your storage accounts and your StorSimple Manager service.</span></span> <span data-ttu-id="22750-119">Jelenleg csak az USA nyugati régiója és Nyugat-Európában régiók támogatottak.</span><span class="sxs-lookup"><span data-stu-id="22750-119">Currently, only West US and West Europe regions are supported.</span></span> <span data-ttu-id="22750-120">Emiatt a StorSimple Manager szolgáltatás, kezelő szolgáltatás és hello kapcsolódó tárfiók kell lenniük a fenti hello támogatott régióban.</span><span class="sxs-lookup"><span data-stu-id="22750-120">Hence, your StorSimple Manager service, Data Manager service, and hello associated storage account should all be in hello preceding supported regions.</span></span> <span data-ttu-id="22750-121">A percben toocreate hello szolgáltatás vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="22750-121">It takes about a minute toocreate hello service.</span></span>

### <a name="create-a-data-transformation-job-definition"></a><span data-ttu-id="22750-122">A data transformation feladatdefiníció létrehozása</span><span class="sxs-lookup"><span data-stu-id="22750-122">Create a data transformation job definition</span></span>

<span data-ttu-id="22750-123">A StorSimple adatkezelő szolgáltatáson belül kell toocreate a data transformation feladat definíciójához.</span><span class="sxs-lookup"><span data-stu-id="22750-123">Within a StorSimple Data Manager service, you need toocreate a data transformation job definition.</span></span> <span data-ttu-id="22750-124">A feladatdefiníció érdekli helyezi át a tárfiók hello natív formátumban hello adatok részleteit.</span><span class="sxs-lookup"><span data-stu-id="22750-124">A job definition specifies details of hello data that you are interested in moving into a storage account in hello native format.</span></span> 

<span data-ttu-id="22750-125">Hajtsa végre a következő lépéseket toocreate egy új data transformation feladatdefiníció hello.</span><span class="sxs-lookup"><span data-stu-id="22750-125">Perform hello following steps toocreate a new data transformation job definition.</span></span>

1.  <span data-ttu-id="22750-126">Keresse meg a létrehozott toohello szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="22750-126">Navigate toohello service that you created.</span></span> <span data-ttu-id="22750-127">Kattintson a **+ Definition feladat**.</span><span class="sxs-lookup"><span data-stu-id="22750-127">Click **+ Job Definition**.</span></span>

    ![Kattintson a kívánt feladat definíciója](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. <span data-ttu-id="22750-129">hello új feladat definition panel is megnyílik.</span><span class="sxs-lookup"><span data-stu-id="22750-129">hello new job definition blade opens up.</span></span> <span data-ttu-id="22750-130">Nevezze el a feladat definíciójához, és kattintson a **forrás**.</span><span class="sxs-lookup"><span data-stu-id="22750-130">Give your job definition a name and click **Source**.</span></span> <span data-ttu-id="22750-131">A hello **konfigurálása adatforrás** panelen adja meg a StorSimple eszköz hello adatait, és hello érdeklő adatok.</span><span class="sxs-lookup"><span data-stu-id="22750-131">In hello **Configure data source** blade, specify hello details of your StorSimple device and hello data of interest.</span></span>

    ![Feladat-definíció létrehozása](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. <span data-ttu-id="22750-133">Mivel ez egy új adatokat kezelő szolgáltatás, nem adattárolók vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="22750-133">Since this is a new Data Manager service, no data repositories are configured.</span></span> <span data-ttu-id="22750-134">tooadd adatok tára, mint a StorSimple Manager kattintson **új hozzáadása** a hello adatok tárház legördülő menüből, majd **adattárház hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="22750-134">tooadd your StorSimple Manager as a data repository, click **Add new** in hello data repository dropdown and then click **Add Data Repository**.</span></span>

4. <span data-ttu-id="22750-135">Válasszon **StorSimple 8000 series Manager** hello tárház, írja be, és írja be a hello tulajdonságainak a **StorSimple Manager**.</span><span class="sxs-lookup"><span data-stu-id="22750-135">Choose **StorSimple 8000 series Manager** as hello repository type and enter hello properties of your **StorSimple Manager**.</span></span> <span data-ttu-id="22750-136">A hello **erőforrás-azonosító** mezőjét, előtt hello tooenter hello számot kell **:** hello regisztrációs kulcs a StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="22750-136">For hello **Resource Id** field, you need tooenter hello number before hello **:** in hello registration key of your StorSimple manager.</span></span>

    ![Adatforrás létrehozása](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  <span data-ttu-id="22750-138">Kattintson a **OK** végzett.</span><span class="sxs-lookup"><span data-stu-id="22750-138">Click **OK** when done.</span></span> <span data-ttu-id="22750-139">Ez az adattárház menti, és a StorSimple Manager ezek a paraméterek ismételt beírása nélkül is újrahasznosíthatók lévő más definíciók.</span><span class="sxs-lookup"><span data-stu-id="22750-139">This saves your data repository and this StorSimple Manager can be reused in other job definitions without entering these parameters again.</span></span> <span data-ttu-id="22750-140">Kattintás után néhány másodpercet vesz igénybe **OK** a StorSimple Manager tooshow mentése hello legördülő hello.</span><span class="sxs-lookup"><span data-stu-id="22750-140">It takes a few seconds after you click **OK** for hello StorSimple Manager tooshow up in hello dropdown.</span></span>

6.  <span data-ttu-id="22750-141">A hello **konfigurálása adatforrás** panelen hello eszköz nevét adja meg, és a fontos adatokat tartalmazó kötet neve hello.</span><span class="sxs-lookup"><span data-stu-id="22750-141">In hello **Configure data source** blade, enter hello device name and hello volume name that has your data of interest.</span></span>

7.  <span data-ttu-id="22750-142">A hello **szűrő** alszakasz, adja meg az egyik fontos adatokat tartalmazó hello gyökérkönyvtár (ebben a mezőben kell kezdődnie, egy `\`).</span><span class="sxs-lookup"><span data-stu-id="22750-142">In hello **Filter** subsection, enter hello root directory that contains your data of interest (this field should start with a `\`).</span></span> <span data-ttu-id="22750-143">Bármely fájlszűrők is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="22750-143">You can also add any file filters here.</span></span>

8.  <span data-ttu-id="22750-144">hello data transformation szolgáltatás hello adatokon keresztül pillanatképek be toohello Azure fejlesztőre működik.</span><span class="sxs-lookup"><span data-stu-id="22750-144">hello data transformation service works on hello data that is pushed up toohello Azure via snapshots.</span></span> <span data-ttu-id="22750-145">Ez a feladat futtatásakor választhat tootake biztonsági másolatot minden alkalommal, amikor a feladat fut (a legújabb adatokkal toowork) vagy toouse hello hello felhőben utolsó meglévő biztonsági mentés (ha az egyes archivált adatokat).</span><span class="sxs-lookup"><span data-stu-id="22750-145">When running this job, you can choose tootake a backup every time this job is run (toowork on latest data) or toouse hello last existing backup in hello cloud (if you are working on some archived data).</span></span>

    ![Új adatforrás részletei](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. <span data-ttu-id="22750-147">A következő hello tárolóbeállítások kell toobe konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="22750-147">Next, hello Target settings need toobe configured.</span></span> <span data-ttu-id="22750-148">Támogatott célok – Azure Storage-fiókok és az Azure Media Services-fiókok 2 típusa van.</span><span class="sxs-lookup"><span data-stu-id="22750-148">There are 2 types of supported targets – Azure Storage accounts and Azure Media Services accounts.</span></span> <span data-ttu-id="22750-149">Válassza ki tárfiókok tooput fájlok a fiókhoz tartozó blobokat.</span><span class="sxs-lookup"><span data-stu-id="22750-149">Choose storage accounts tooput files into blobs in that account.</span></span> <span data-ttu-id="22750-150">Tooput fájlok az media services-fiók kiválasztása a fiókhoz tartozó eszközök.</span><span class="sxs-lookup"><span data-stu-id="22750-150">Choose media services account tooput files into assets in that account.</span></span> <span data-ttu-id="22750-151">Ebben az esetben kell tooadd tára.</span><span class="sxs-lookup"><span data-stu-id="22750-151">Again, we need tooadd a repository.</span></span> <span data-ttu-id="22750-152">Hello legördülő menüben válassza ki **új hozzáadása** , majd **beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="22750-152">In hello dropdown, select **Add new** and then **Configure settings**.</span></span>

    ![Adatokat fogadó létrehozása](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. <span data-ttu-id="22750-154">Itt kiválaszthatja kívánt tooadd és hello más paramétereket hello tárház társított tárház hello típusa.</span><span class="sxs-lookup"><span data-stu-id="22750-154">Here, you can select hello type of repository you want tooadd and hello other parameters associated with hello repository.</span></span> <span data-ttu-id="22750-155">Mindkét esetben a tároló várólista létrejön hello feladat futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="22750-155">In both cases, a storage queue is created when hello job runs.</span></span> <span data-ttu-id="22750-156">A sorba az átalakított blobokkal kapcsolatos üzenetek kerülnek, amint a blobok elkészültek.</span><span class="sxs-lookup"><span data-stu-id="22750-156">This queue is populated with messages about transformed blobs as they are ready.</span></span> <span data-ttu-id="22750-157">a várólista nevét hello van hello ugyanaz, mint a hello feladatdefiníció hello nevét.</span><span class="sxs-lookup"><span data-stu-id="22750-157">hello name of this queue is hello same as hello name of hello job definition.</span></span> <span data-ttu-id="22750-158">Ha **Media Services** hello tárház típusa, majd is megadhat tárfiók hitelesítő adatainak ahol hello várólista létrejön.</span><span class="sxs-lookup"><span data-stu-id="22750-158">If you select **Media Services** as hello repo type, then you can also enter storage account credentials where hello queue is created.</span></span>

    ![Új adatokat fogadó részletei](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. <span data-ttu-id="22750-160">A felvett hello adattárház (amely néhány másodpercet vesz igénybe), található hello tárház hello hello legördülő **célfiók neve**.</span><span class="sxs-lookup"><span data-stu-id="22750-160">After adding hello data repository (which takes a few seconds), you will find hello repo in hello dropdown in hello **Target account name**.</span></span>  <span data-ttu-id="22750-161">Válassza ki a hello cél, amelyekre szüksége van.</span><span class="sxs-lookup"><span data-stu-id="22750-161">Choose hello target that you need.</span></span>

12. <span data-ttu-id="22750-162">Kattintson a **OK** toocreate hello feladat definíciójához.</span><span class="sxs-lookup"><span data-stu-id="22750-162">Click **OK** toocreate hello job definition.</span></span> <span data-ttu-id="22750-163">A feladat definíciójához most már be van állítva.</span><span class="sxs-lookup"><span data-stu-id="22750-163">Your job definition is now set up.</span></span> <span data-ttu-id="22750-164">A feladat definíciójához hello felhasználói felületén keresztül többször is használhatja.</span><span class="sxs-lookup"><span data-stu-id="22750-164">You can use this job definition multiple times via hello UI.</span></span>

    ![Új projekt hozzáadása](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-hello-job-definition"></a><span data-ttu-id="22750-166">Hello feladatdefiníció futtatása</span><span class="sxs-lookup"><span data-stu-id="22750-166">Run hello job definition</span></span>

<span data-ttu-id="22750-167">StorSimple toohello tárfiók a hello feladatdefinícióban megadott toomove adatait van szüksége, szüksége lesz a tooinvoke azt.</span><span class="sxs-lookup"><span data-stu-id="22750-167">Whenever you need toomove data from StorSimple toohello storage account that you have specified in hello job definition, you will need tooinvoke it.</span></span> <span data-ttu-id="22750-168">Nincs rugalmas hello paraméterek módosítása, minden alkalommal, amikor hello feladatot indít.</span><span class="sxs-lookup"><span data-stu-id="22750-168">There is some flexibility in changing hello parameters every time you invoke hello job.</span></span> <span data-ttu-id="22750-169">hello lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="22750-169">hello steps are as follows:</span></span>

1. <span data-ttu-id="22750-170">Válassza ki a StorSimple adatkezelő szolgáltatást, és nyissa meg túl**figyelés**.</span><span class="sxs-lookup"><span data-stu-id="22750-170">Select your StorSimple Data Manager service and go too**Monitoring**.</span></span> <span data-ttu-id="22750-171">Kattintson a **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="22750-171">Click **Run Now**.</span></span>

    ![Eseményindító feladatdefiníció](./media/storsimple-data-manager-ui/run-now.png)

2. <span data-ttu-id="22750-173">Válassza ki a hello feladatdefiníció, amelyet az toorun.</span><span class="sxs-lookup"><span data-stu-id="22750-173">Choose hello job definition that you want toorun.</span></span> <span data-ttu-id="22750-174">Kattintson a **futtatási beállítások** toomodify toochange érdemes futtatni a feladatot a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="22750-174">Click **Run settings** toomodify any settings that you might want toochange for this job run.</span></span>

    ![Feladatbeállítások futtatása](./media/storsimple-data-manager-ui/run-settings.png)

3. <span data-ttu-id="22750-176">Kattintson a **OK** majd **futtatása** toolaunch a feladatot.</span><span class="sxs-lookup"><span data-stu-id="22750-176">Click **OK** and then click **Run** toolaunch your job.</span></span> <span data-ttu-id="22750-177">toomonitor ezt a feladatot, nyissa meg toohello **feladatok** a StorSimple adatkezelő lap.</span><span class="sxs-lookup"><span data-stu-id="22750-177">toomonitor this job, go toohello **Jobs** page in your StorSimple Data Manager.</span></span>

    ![Feladatok listája és állapota](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. <span data-ttu-id="22750-179">A hozzáadása toomonitoring a hello **feladatok** panelen, akkor is figyelheti a hello storage üzenetsorába, ahol egy üzenet jelenik meg minden alkalommal, amikor egy fájl átkerül a StorSimple toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="22750-179">In addition toomonitoring in hello **Jobs** blade, you can also listen on hello storage queue where a message is added every time a file is moved from StorSimple toohello storage account.</span></span>


## <a name="next-steps"></a><span data-ttu-id="22750-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="22750-180">Next steps</span></span>

<span data-ttu-id="22750-181">[.NET SDK toolaunch StorSimple adatkezelő feladatok](storsimple-data-manager-dotnet-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="22750-181">[Use .NET SDK toolaunch StorSimple Data Manager jobs](storsimple-data-manager-dotnet-jobs.md).</span></span>
