---
title: "A Microsoft Azure StorSimple adatokat kezelő felhasználói felületén |} Microsoft Docs"
description: "StorSimple adatok Manager szolgáltatással (magán előnézetben) felhasználói felület használata"
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
ms.openlocfilehash: 53a8599df2c647613122cd791b680e2e658586b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-using-the-storsimple-data-manager-service-ui-private-preview"></a><span data-ttu-id="b0ae3-103">A StorSimple adatokat kezelő szolgáltatással felhasználói felület (magán előnézetben) kezelése</span><span class="sxs-lookup"><span data-stu-id="b0ae3-103">Manage using the StorSimple Data Manager service UI (Private Preview)</span></span>

<span data-ttu-id="b0ae3-104">Ez a cikk ismerteti, hogyan használhatja a StorSimple adatokat kezelő felhasználói felületén a StorSimple 8000 sorozat eszközeire levő adatok a data transformation végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-104">This article explains how you can use the StorSimple Data Manager UI to perform data transformation on data residing on the StorSimple 8000 series devices.</span></span> <span data-ttu-id="b0ae3-105">Az átalakított adatok majd más Azure-szolgáltatások például az Azure Media Services, az Azure HDInsight, az Azure Machine Learning és az Azure Search által igénybe vehető.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-105">The transformed data can then be consumed by other Azure services such as Azure Media Services, Azure HDInsight, Azure Machine Learning, and Azure Search.</span></span> 


## <a name="use-storsimple-data-transformation"></a><span data-ttu-id="b0ae3-106">StorSimple adatok átalakítással</span><span class="sxs-lookup"><span data-stu-id="b0ae3-106">Use StorSimple Data Transformation</span></span>

<span data-ttu-id="b0ae3-107">A StorSimple-kezelő a Data Transformation példányosítható erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-107">The StorSimple Data Manager is the resource within which Data Transformation can be instantiated.</span></span> <span data-ttu-id="b0ae3-108">A Data Transformation szolgáltatás lehetővé teszi az eszközét a helyszíni adatok áthelyezése az Azure-tárfiókba blobok.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-108">The Data Transformation service lets you move data from your StorSimple on-premises device to blobs in Azure storage.</span></span> <span data-ttu-id="b0ae3-109">Emiatt a munkafolyamat meg kell adnia a StorSimple eszköz és a storage-fiók áthelyezése kívánt érdeklő adatok részleteit.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-109">Hence, in workflow you need to specify the details about your StorSimple device and the data of interest that you want to move to the storage account.</span></span>

### <a name="create-a-storsimple-data-manager-service"></a><span data-ttu-id="b0ae3-110">StorSimple adatkezelő szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0ae3-110">Create a StorSimple Data Manager service</span></span>

<span data-ttu-id="b0ae3-111">A következő lépésekkel StorSimple adatkezelő szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-111">Perform the following steps to create a StorSimple Data Manager service.</span></span>

1. <span data-ttu-id="b0ae3-112">Hozhatja létre a StorSimple adatok Manager szolgáltatást, [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)</span><span class="sxs-lookup"><span data-stu-id="b0ae3-112">To create a StorSimple Data Manager service, go to [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)</span></span>

2. <span data-ttu-id="b0ae3-113">Kattintson a  **+**  ikonra, és keresse a StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-113">Click the **+** icon and search for StorSimple Data Manager.</span></span> <span data-ttu-id="b0ae3-114">Kattintson a StorSimple adatok Manager szolgáltatást, majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-114">Click your StorSimple Data Manager service and then click **Create**.</span></span>

3. <span data-ttu-id="b0ae3-115">Ha az előfizetés engedélyezve van ez a szolgáltatás létrehozása, a következő panelen láthatja.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-115">If your subscription is enabled for creating this service, you see the following blade.</span></span>

    ![StorSimple adatok kezelők erőforrás létrehozása](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. <span data-ttu-id="b0ae3-117">Adja meg az adatokat, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-117">Enter the inputs and click **Create**.</span></span> <span data-ttu-id="b0ae3-118">A megadott helyen az legyen, amely a storage-fiókok és a StorSimple Manager szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-118">The specified location should be the one that houses your storage accounts and your StorSimple Manager service.</span></span> <span data-ttu-id="b0ae3-119">Jelenleg csak az USA nyugati régiója és Nyugat-Európában régiók támogatottak.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-119">Currently, only West US and West Europe regions are supported.</span></span> <span data-ttu-id="b0ae3-120">Emiatt a StorSimple Manager szolgáltatás, kezelő szolgáltatás és a kapcsolódó tárfiók kell lenniük az előző támogatott régióban.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-120">Hence, your StorSimple Manager service, Data Manager service, and the associated storage account should all be in the preceding supported regions.</span></span> <span data-ttu-id="b0ae3-121">A szolgáltatás létrehozásához körülbelül egy perce szükséges.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-121">It takes about a minute to create the service.</span></span>

### <a name="create-a-data-transformation-job-definition"></a><span data-ttu-id="b0ae3-122">A data transformation feladatdefiníció létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0ae3-122">Create a data transformation job definition</span></span>

<span data-ttu-id="b0ae3-123">A StorSimple adatkezelő szolgáltatásokon belüli létrehozásához szükséges egy adatok átalakítási feladat definíciójához.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-123">Within a StorSimple Data Manager service, you need to create a data transformation job definition.</span></span> <span data-ttu-id="b0ae3-124">Olyan feladatdefinícióban határozza meg, hogy érdekli helyezi át a natív formátumban a tárfiók adatok részleteit.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-124">A job definition specifies details of the data that you are interested in moving into a storage account in the native format.</span></span> 

<span data-ttu-id="b0ae3-125">A következő lépésekkel hozzon létre egy új adatok átalakítási feladat definíciójához.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-125">Perform the following steps to create a new data transformation job definition.</span></span>

1.  <span data-ttu-id="b0ae3-126">Nyissa meg a létrehozott szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-126">Navigate to the service that you created.</span></span> <span data-ttu-id="b0ae3-127">Kattintson a **+ Definition feladat**.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-127">Click **+ Job Definition**.</span></span>

    ![Kattintson a kívánt feladat definíciója](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. <span data-ttu-id="b0ae3-129">Az új feladat definition panel is megnyílik.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-129">The new job definition blade opens up.</span></span> <span data-ttu-id="b0ae3-130">Nevezze el a feladat definíciójához, és kattintson a **forrás**.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-130">Give your job definition a name and click **Source**.</span></span> <span data-ttu-id="b0ae3-131">Az a **konfigurálása adatforrás** panelen adja meg a StorSimple eszköz részleteit és érdeklő adatok.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-131">In the **Configure data source** blade, specify the details of your StorSimple device and the data of interest.</span></span>

    ![Feladat-definíció létrehozása](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. <span data-ttu-id="b0ae3-133">Mivel ez egy új adatokat kezelő szolgáltatás, nem adattárolók vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-133">Since this is a new Data Manager service, no data repositories are configured.</span></span> <span data-ttu-id="b0ae3-134">A StorSimple Manager hozzáadni a rendszert, kattintson a **új hozzáadása** az adatok tárház legördülő majd **adattárház hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-134">To add your StorSimple Manager as a data repository, click **Add new** in the data repository dropdown and then click **Add Data Repository**.</span></span>

4. <span data-ttu-id="b0ae3-135">Válasszon **StorSimple 8000 series Manager** tárházaként írja be, és adja meg a tulajdonságait a **StorSimple Manager**.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-135">Choose **StorSimple 8000 series Manager** as the repository type and enter the properties of your **StorSimple Manager**.</span></span> <span data-ttu-id="b0ae3-136">Az a **erőforrás-azonosító** mezőben meg kell adnia a száma, mielőtt a **:** a regisztrációs kulcs a StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-136">For the **Resource Id** field, you need to enter the number before the **:** in the registration key of your StorSimple manager.</span></span>

    ![Adatforrás létrehozása](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  <span data-ttu-id="b0ae3-138">Kattintson a **OK** végzett.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-138">Click **OK** when done.</span></span> <span data-ttu-id="b0ae3-139">Ez az adattárház menti, és a StorSimple Manager ezek a paraméterek ismételt beírása nélkül is újrahasznosíthatók lévő más definíciók.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-139">This saves your data repository and this StorSimple Manager can be reused in other job definitions without entering these parameters again.</span></span> <span data-ttu-id="b0ae3-140">Kattintás után néhány másodpercet vesz igénybe **OK** a StorSimple Manager megjeleníti őket a legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-140">It takes a few seconds after you click **OK** for the StorSimple Manager to show up in the dropdown.</span></span>

6.  <span data-ttu-id="b0ae3-141">Az a **konfigurálása adatforrás** panelen adja meg az eszköz nevét és a kötet neve, amely a fontos adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-141">In the **Configure data source** blade, enter the device name and the volume name that has your data of interest.</span></span>

7.  <span data-ttu-id="b0ae3-142">Az a **szűrő** alszakasz, adja meg a gyökérkönyvtár egyik fontos adatokat tartalmazó (ebben a mezőben kell kezdődnie, egy `\`).</span><span class="sxs-lookup"><span data-stu-id="b0ae3-142">In the **Filter** subsection, enter the root directory that contains your data of interest (this field should start with a `\`).</span></span> <span data-ttu-id="b0ae3-143">Bármely fájlszűrők is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-143">You can also add any file filters here.</span></span>

8.  <span data-ttu-id="b0ae3-144">A data transformation szolgáltatás működik-e az adatok az Azure-bA leküldött, a pillanatképek keresztül.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-144">The data transformation service works on the data that is pushed up to the Azure via snapshots.</span></span> <span data-ttu-id="b0ae3-145">Ez a feladat futtatásakor is választja, a biztonsági mentés minden futtatásakor ez a feladat (csak ezen legfrissebb adatokat), vagy ha a legutóbbi meglévő biztonsági mentés a felhőben (ha az egyes archivált adatokat).</span><span class="sxs-lookup"><span data-stu-id="b0ae3-145">When running this job, you can choose to take a backup every time this job is run (to work on latest data) or to use the last existing backup in the cloud (if you are working on some archived data).</span></span>

    ![Új adatforrás részletei](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. <span data-ttu-id="b0ae3-147">Ezután a tárolóbeállítások kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-147">Next, the Target settings need to be configured.</span></span> <span data-ttu-id="b0ae3-148">Támogatott célok – Azure Storage-fiókok és az Azure Media Services-fiókok 2 típusa van.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-148">There are 2 types of supported targets – Azure Storage accounts and Azure Media Services accounts.</span></span> <span data-ttu-id="b0ae3-149">Válassza ki a fájlok kerüljenek a fiókhoz tartozó BLOB storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-149">Choose storage accounts to put files into blobs in that account.</span></span> <span data-ttu-id="b0ae3-150">Válassza ki a media services-fiók fájlok kerüljenek a fiókhoz tartozó eszközök számára.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-150">Choose media services account to put files into assets in that account.</span></span> <span data-ttu-id="b0ae3-151">Ebben az esetben kell vegyen fel egy tárházat.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-151">Again, we need to add a repository.</span></span> <span data-ttu-id="b0ae3-152">A legördülő listában jelölje ki **új hozzáadása** , majd **beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-152">In the dropdown, select **Add new** and then **Configure settings**.</span></span>

    ![Adatokat fogadó létrehozása](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. <span data-ttu-id="b0ae3-154">Itt is válassza ki a hozzáadni kívánt tárház és a tárház társított a többi paraméter típusát.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-154">Here, you can select the type of repository you want to add and the other parameters associated with the repository.</span></span> <span data-ttu-id="b0ae3-155">Mindkét esetben a tároló várólista létrejön, a feladat futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-155">In both cases, a storage queue is created when the job runs.</span></span> <span data-ttu-id="b0ae3-156">A sorba az átalakított blobokkal kapcsolatos üzenetek kerülnek, amint a blobok elkészültek.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-156">This queue is populated with messages about transformed blobs as they are ready.</span></span> <span data-ttu-id="b0ae3-157">A sor neve megegyezik a feladatdefiníció nevével.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-157">The name of this queue is the same as the name of the job definition.</span></span> <span data-ttu-id="b0ae3-158">Ha **Media Services** tárház típusként, majd is megadhat tárfiók hitelesítő adatainak ahol a várólista létrejön.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-158">If you select **Media Services** as the repo type, then you can also enter storage account credentials where the queue is created.</span></span>

    ![Új adatokat fogadó részletei](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. <span data-ttu-id="b0ae3-160">Miután hozzáadta az adattárház (amely néhány másodpercet vesz igénybe), megtalálja a tárházban meg a legördülő listában a a **célfiók neve**.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-160">After adding the data repository (which takes a few seconds), you will find the repo in the dropdown in the **Target account name**.</span></span>  <span data-ttu-id="b0ae3-161">Válassza ki a cél, amelyekre szüksége van.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-161">Choose the target that you need.</span></span>

12. <span data-ttu-id="b0ae3-162">Kattintson a **OK** létrehozni a feladat definíciójához.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-162">Click **OK** to create the job definition.</span></span> <span data-ttu-id="b0ae3-163">A feladat definíciójához most már be van állítva.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-163">Your job definition is now set up.</span></span> <span data-ttu-id="b0ae3-164">A feladat definíciójához a felhasználói felületen keresztül többször is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-164">You can use this job definition multiple times via the UI.</span></span>

    ![Új projekt hozzáadása](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-the-job-definition"></a><span data-ttu-id="b0ae3-166">A feladat definíciójához futtatása</span><span class="sxs-lookup"><span data-stu-id="b0ae3-166">Run the job definition</span></span>

<span data-ttu-id="b0ae3-167">Adatok áthelyezése StorSimple feladatdefinícióban megadott tárfiók van szüksége, szüksége lesz indítsa el.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-167">Whenever you need to move data from StorSimple to the storage account that you have specified in the job definition, you will need to invoke it.</span></span> <span data-ttu-id="b0ae3-168">Rugalmas szerepel a paraméterek módosítása, minden alkalommal, amikor a feladat indításakor.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-168">There is some flexibility in changing the parameters every time you invoke the job.</span></span> <span data-ttu-id="b0ae3-169">A lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="b0ae3-169">The steps are as follows:</span></span>

1. <span data-ttu-id="b0ae3-170">Válassza ki a StorSimple adatkezelő szolgáltatást, és navigáljon **figyelés**.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-170">Select your StorSimple Data Manager service and go to **Monitoring**.</span></span> <span data-ttu-id="b0ae3-171">Kattintson a **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-171">Click **Run Now**.</span></span>

    ![Eseményindító feladatdefiníció](./media/storsimple-data-manager-ui/run-now.png)

2. <span data-ttu-id="b0ae3-173">Válassza ki a futtatni kívánt feladat definíciójához.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-173">Choose the job definition that you want to run.</span></span> <span data-ttu-id="b0ae3-174">Kattintson a **futtatási beállítások** módosítani a beállításait, előfordulhat, hogy módosítani szeretné a feladat futtatásához.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-174">Click **Run settings** to modify any settings that you might want to change for this job run.</span></span>

    ![Feladatbeállítások futtatása](./media/storsimple-data-manager-ui/run-settings.png)

3. <span data-ttu-id="b0ae3-176">Kattintson a **OK** majd **futtatása** elindítani a feladatot.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-176">Click **OK** and then click **Run** to launch your job.</span></span> <span data-ttu-id="b0ae3-177">Ez a feladat figyeléséhez, keresse fel a **feladatok** a StorSimple adatkezelő lap.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-177">To monitor this job, go to the **Jobs** page in your StorSimple Data Manager.</span></span>

    ![Feladatok listája és állapota](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. <span data-ttu-id="b0ae3-179">A figyelés mellett a **feladatok** panelen, akkor is figyelheti a a tároló várólista, ahol egy üzenet jelenik meg minden alkalommal, amikor egy fájl átkerül a StorSimple a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b0ae3-179">In addition to monitoring in the **Jobs** blade, you can also listen on the storage queue where a message is added every time a file is moved from StorSimple to the storage account.</span></span>


## <a name="next-steps"></a><span data-ttu-id="b0ae3-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b0ae3-180">Next steps</span></span>

<span data-ttu-id="b0ae3-181">[.NET SDK használatával indítsa el a StorSimple adatkezelő feladatok](storsimple-data-manager-dotnet-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="b0ae3-181">[Use .NET SDK to launch StorSimple Data Manager jobs](storsimple-data-manager-dotnet-jobs.md).</span></span>