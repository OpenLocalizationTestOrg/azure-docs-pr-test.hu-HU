---
title: "Alkalmazáscsomagok telepítse a számítási csomópontok - Azure Batch |} Microsoft Docs"
description: "Használja az alkalmazás csomagok szolgáltatása könnyedén kezelheti a több alkalmazás és a kötegelt telepítés verziók Azure Batch számítási csomópontjain."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 3b6044b7-5f65-4a27-9d43-71e1863d16cf
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: afcc04c80ec15872a22de5d5969a7ef6a583562f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-applications-to-compute-nodes-with-batch-application-packages"></a><span data-ttu-id="59d0b-103">A számítási csomópontokat a kötegelt alkalmazáscsomagok alkalmazások központi telepítése</span><span class="sxs-lookup"><span data-stu-id="59d0b-103">Deploy applications to compute nodes with Batch application packages</span></span>

<span data-ttu-id="59d0b-104">Az alkalmazás csomagok Azure-köteg szolgáltatása feladat alkalmazások kezelésének és a telepítés, a készlet számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="59d0b-104">The application packages feature of Azure Batch provides easy management of task applications and their deployment to the compute nodes in your pool.</span></span> <span data-ttu-id="59d0b-105">Az alkalmazáscsomagok töltse fel, és kezelheti az alkalmazásokat, a feladatok futnak, beleértve a segédfájlok több verziója.</span><span class="sxs-lookup"><span data-stu-id="59d0b-105">With application packages, you can upload and manage multiple versions of the applications your tasks run, including their supporting files.</span></span> <span data-ttu-id="59d0b-106">Majd automatikusan telepítheti ezeket az alkalmazásokat a számítási csomópontok közül a készletben.</span><span class="sxs-lookup"><span data-stu-id="59d0b-106">You can then automatically deploy one or more of these applications to the compute nodes in your pool.</span></span>

<span data-ttu-id="59d0b-107">Meg ebből a cikkből megtudhatja, hogyan töltse fel és kezelheti az alkalmazáscsomagok az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="59d0b-107">In this article, you will learn how to upload and manage application packages in the Azure portal.</span></span> <span data-ttu-id="59d0b-108">Majd megtudhatja, hogy telepítse őket a készlet számítási csomópont hogyan a [Batch .NET] [ api_net] könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="59d0b-108">You will then learn how to install them on a pool's compute nodes with the [Batch .NET][api_net] library.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="59d0b-109">Az alkalmazáscsomagok az összes 2017. július 5. után létrehozott Batch-készleten támogatottak.</span><span class="sxs-lookup"><span data-stu-id="59d0b-109">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="59d0b-110">A 2016. március 10. és 2017. július 5. között létrehozott Batch-készletek esetében csak akkor támogatottak, ha a készlet felhőszolgáltatás-konfigurációval lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="59d0b-110">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if the pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="59d0b-111">A 2016. március 10. előtt létrehozott Batch-készletek nem támogatják az alkalmazáscsomagokat.</span><span class="sxs-lookup"><span data-stu-id="59d0b-111">Batch pools created prior to 10 March 2016 do not support application packages.</span></span>
>
> <span data-ttu-id="59d0b-112">Az API-k létrehozására és kezelésére alkalmazáscsomagok részei a [Batch Management .NET kódtárral] [[api_net_mgmt]] könyvtár.</span><span class="sxs-lookup"><span data-stu-id="59d0b-112">The APIs for creating and managing application packages are part of the [Batch Management .NET][[api_net_mgmt]] library.</span></span> <span data-ttu-id="59d0b-113">Az API-k, a számítási csomóponton alkalmazáscsomagok telepítésének részét képezik a [Batch .NET] [ api_net] könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="59d0b-113">The APIs for installing application packages on a compute node are part of the [Batch .NET][api_net] library.</span></span>  
>
> <span data-ttu-id="59d0b-114">Az itt leírt alkalmazás csomagok szolgáltatás írja felül a Batch-alkalmazások szolgáltatás a szolgáltatás előző verzióiban érhető el.</span><span class="sxs-lookup"><span data-stu-id="59d0b-114">The application packages feature described here supersedes the Batch Apps feature available in previous versions of the service.</span></span>
> 
> 

## <a name="application-package-requirements"></a><span data-ttu-id="59d0b-115">Csomag alkalmazáskövetelmények</span><span class="sxs-lookup"><span data-stu-id="59d0b-115">Application package requirements</span></span>
<span data-ttu-id="59d0b-116">Alkalmazáscsomagok használatához szüksége [egy Azure Storage-fiók csatolása](#link-a-storage-account) a Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="59d0b-116">To use application packages, you need to [link an Azure Storage account](#link-a-storage-account) to your Batch account.</span></span>

<span data-ttu-id="59d0b-117">Ez a szolgáltatás bemutatott [Batch REST API] [ api_rest] 2015-12-01.2.2 verziója és a megfelelő [Batch .NET] [ api_net] könyvtárverzió 3.1.0.</span><span class="sxs-lookup"><span data-stu-id="59d0b-117">This feature was introduced in [Batch REST API][api_rest] version 2015-12-01.2.2 and the corresponding [Batch .NET][api_net] library version 3.1.0.</span></span> <span data-ttu-id="59d0b-118">Azt javasoljuk, hogy mindig használja a legújabb API-verzió az kötegelt használatakor.</span><span class="sxs-lookup"><span data-stu-id="59d0b-118">We recommend that you always use the latest API version when working with Batch.</span></span>

> [!NOTE]
> <span data-ttu-id="59d0b-119">Az alkalmazáscsomagok az összes 2017. július 5. után létrehozott Batch-készleten támogatottak.</span><span class="sxs-lookup"><span data-stu-id="59d0b-119">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="59d0b-120">A 2016. március 10. és 2017. július 5. között létrehozott Batch-készletek esetében csak akkor támogatottak, ha a készlet felhőszolgáltatás-konfigurációval lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="59d0b-120">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if the pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="59d0b-121">A 2016. március 10. előtt létrehozott Batch-készletek nem támogatják az alkalmazáscsomagokat.</span><span class="sxs-lookup"><span data-stu-id="59d0b-121">Batch pools created prior to 10 March 2016 do not support application packages.</span></span>
>
>

## <a name="about-applications-and-application-packages"></a><span data-ttu-id="59d0b-122">Alkalmazások és csomagok</span><span class="sxs-lookup"><span data-stu-id="59d0b-122">About applications and application packages</span></span>
<span data-ttu-id="59d0b-123">Azure Batch belül egy *alkalmazás* rendszerverzióval ellátott bináris fájljai, a számítási csomópontok a készlet automatikusan letöltött készlete hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="59d0b-123">Within Azure Batch, an *application* refers to a set of versioned binaries that can be automatically downloaded to the compute nodes in your pool.</span></span> <span data-ttu-id="59d0b-124">Egy *alkalmazáscsomag* hivatkozik egy *meghatározott* e bináris fájljait és jelöli egy adott *verzió* az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="59d0b-124">An *application package* refers to a *specific set* of those binaries and represents a given *version* of the application.</span></span>

<span data-ttu-id="59d0b-125">![Magas szintű diagramját, alkalmazások és csomagok][1]</span><span class="sxs-lookup"><span data-stu-id="59d0b-125">![High-level diagram of applications and application packages][1]</span></span>

### <a name="applications"></a><span data-ttu-id="59d0b-126">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="59d0b-126">Applications</span></span>
<span data-ttu-id="59d0b-127">Egy alkalmazás kötegben tartalmaz egy vagy több alkalmazás a csomagok és az alkalmazás konfigurációs beállításait adja meg.</span><span class="sxs-lookup"><span data-stu-id="59d0b-127">An application in Batch contains one or more application packages and specifies configuration options for the application.</span></span> <span data-ttu-id="59d0b-128">Például egy alkalmazás telepítése számítási csomópontok, és hogy a csomagokat is frissíthető és nem törölhető az alapértelmezett alkalmazáscsomag verzióját adhat meg.</span><span class="sxs-lookup"><span data-stu-id="59d0b-128">For example, an application can specify the default application package version to install on compute nodes and whether its packages can be updated or deleted.</span></span>

### <a name="application-packages"></a><span data-ttu-id="59d0b-129">Alkalmazáscsomagok</span><span class="sxs-lookup"><span data-stu-id="59d0b-129">Application packages</span></span>
<span data-ttu-id="59d0b-130">Alkalmazáscsomag, a bináris alkalmazásfájlokat tartalmazó .zip fájl és a fájlokat, amelyek szükségesek az alkalmazás futtatásához a feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="59d0b-130">An application package is a .zip file that contains the application binaries and supporting files that are required for your tasks to run the application.</span></span> <span data-ttu-id="59d0b-131">Minden alkalmazáscsomag jelenti. az alkalmazás egy adott verziójához.</span><span class="sxs-lookup"><span data-stu-id="59d0b-131">Each application package represents a specific version of the application.</span></span>

<span data-ttu-id="59d0b-132">A készlet és a feladat szinten alkalmazáscsomagok adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="59d0b-132">You can specify application packages at the pool and task levels.</span></span> <span data-ttu-id="59d0b-133">Egy készletet vagy feladat létrehozásakor megadhatja egy vagy több ezeket a csomagokat, és (opcionálisan) verzióval.</span><span class="sxs-lookup"><span data-stu-id="59d0b-133">You can specify one or more of these packages and (optionally) a version when you create a pool or task.</span></span>

* <span data-ttu-id="59d0b-134">**Tárolókészlet alkalmazáscsomagok** telepített *minden* csomópont a készletben.</span><span class="sxs-lookup"><span data-stu-id="59d0b-134">**Pool application packages** are deployed to *every* node in the pool.</span></span> <span data-ttu-id="59d0b-135">Alkalmazások vannak telepítve, amikor a csomópont csatlakozik egy készletet, és amikor újraindították vagy a lemezképet.</span><span class="sxs-lookup"><span data-stu-id="59d0b-135">Applications are deployed when a node joins a pool, and when it is rebooted or reimaged.</span></span>
  
    <span data-ttu-id="59d0b-136">Készlet alkalmazáscsomagok megfelelőek, amikor egy alkalmazáskészlet összes csomópontjának hajtható végre egy feladat tevékenységeit.</span><span class="sxs-lookup"><span data-stu-id="59d0b-136">Pool application packages are appropriate when all nodes in a pool execute a job's tasks.</span></span> <span data-ttu-id="59d0b-137">Megadhat egy vagy több alkalmazáscsomagok készletet hoz létre, és adja hozzá, vagy egy meglévő készlet-csomagok frissítése.</span><span class="sxs-lookup"><span data-stu-id="59d0b-137">You can specify one or more application packages when you create a pool, and you can add or update an existing pool's packages.</span></span> <span data-ttu-id="59d0b-138">Ha egy meglévő készlet alkalmazáscsomagok frissíti, újra kell indítania a csomópontot az új csomag telepítése.</span><span class="sxs-lookup"><span data-stu-id="59d0b-138">If you update an existing pool's application packages, you must restart its nodes to install the new package.</span></span>
* <span data-ttu-id="59d0b-139">**Tevékenység-alkalmazáscsomagok** csak egy feladat, csak a parancssor futtatása a feladatütemezés futtatása előtt futtatandó számítási csomópont van telepítve.</span><span class="sxs-lookup"><span data-stu-id="59d0b-139">**Task application packages** are deployed only to a compute node scheduled to run a task, just before running the task's command line.</span></span> <span data-ttu-id="59d0b-140">Ha a megadott alkalmazáscsomag és verziója már a csomóponton, nem újra telepíteni, és a meglévő csomag használata.</span><span class="sxs-lookup"><span data-stu-id="59d0b-140">If the specified application package and version is already on the node, it is not redeployed and the existing package is used.</span></span>
  
    <span data-ttu-id="59d0b-141">A feladat alkalmazáscsomagok olyan hasznos megosztott készlet környezetekben, ahol különböző feladat futtatása egy készletet, és a készlet nem törlődik, amikor a feladat befejezését.</span><span class="sxs-lookup"><span data-stu-id="59d0b-141">Task application packages are useful in shared-pool environments, where different jobs are run on one pool, and the pool is not deleted when a job is completed.</span></span> <span data-ttu-id="59d0b-142">Ha a feladatnál a készletben kevesebb a tevékenység, mint a csomópont, az alkalmazáscsomagok használatával csökkentheti az adatátviteli igényt, mivel így a rendszer csak azokon a csomópontokon helyezi üzembe az alkalmazást, amelyek ténylegesen futtatják a tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="59d0b-142">If your job has fewer tasks than nodes in the pool, task application packages can minimize data transfer since your application is deployed only to the nodes that run tasks.</span></span>
  
    <span data-ttu-id="59d0b-143">Más tevékenység alkalmazáscsomagok is előnyeit kihasználó forgatókönyvek, amelyek a nagyméretű alkalmazások futnak feladatok, de csak néhány feladatot.</span><span class="sxs-lookup"><span data-stu-id="59d0b-143">Other scenarios that can benefit from task application packages are jobs that run a large application, but for only a few tasks.</span></span> <span data-ttu-id="59d0b-144">Például egy előre feldolgozásra szakaszban, vagy egy merge feladatra, ahol az előzetes feldolgozás vagy egyesítési alkalmazás nehéz, előfordulhat, hogy előnyt az alkalmazáscsomagok feladat.</span><span class="sxs-lookup"><span data-stu-id="59d0b-144">For example, a pre-processing stage or a merge task, where the pre-processing or merge application is heavyweight, may benefit from using task application packages.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="59d0b-145">Nincsenek korlátozások, alkalmazások és a Batch-fiók alkalmazás csomagok számát és a csomag maximális mérete.</span><span class="sxs-lookup"><span data-stu-id="59d0b-145">There are restrictions on the number of applications and application packages within a Batch account and on the maximum application package size.</span></span> <span data-ttu-id="59d0b-146">Lásd: [kvótái és korlátai az Azure Batch szolgáltatás](batch-quota-limit.md) kapcsolatos részletekért.</span><span class="sxs-lookup"><span data-stu-id="59d0b-146">See [Quotas and limits for the Azure Batch service](batch-quota-limit.md) for details about these limits.</span></span>
> 
> 

### <a name="benefits-of-application-packages"></a><span data-ttu-id="59d0b-147">Az alkalmazáscsomagok előnyei</span><span class="sxs-lookup"><span data-stu-id="59d0b-147">Benefits of application packages</span></span>
<span data-ttu-id="59d0b-148">Alkalmazáscsomagok egyszerűsítheti a kötegelt megoldásban a kódot, és csökkentheti a terhelés, a feladatok által futtatott alkalmazásokat kezeléséhez szükségesek.</span><span class="sxs-lookup"><span data-stu-id="59d0b-148">Application packages can simplify the code in your Batch solution and lower the overhead required to manage the applications that your tasks run.</span></span>

<span data-ttu-id="59d0b-149">Alkalmazáscsomagok esetén az alkalmazáskészlet indítása feladat nem kell egyes Erőforrásfájlok csomópontjain telepítendő hosszú listáját adja meg.</span><span class="sxs-lookup"><span data-stu-id="59d0b-149">With application packages, your pool's start task doesn't have to specify a long list of individual resource files to install on the nodes.</span></span> <span data-ttu-id="59d0b-150">Nincs több verzió az alkalmazásfájlokat az Azure Storage vagy a csomóponton kézi kezelését.</span><span class="sxs-lookup"><span data-stu-id="59d0b-150">You don't have to manually manage multiple versions of your application files in Azure Storage, or on your nodes.</span></span> <span data-ttu-id="59d0b-151">És nem kell aggódnia generálása [SAS URL-címek](../storage/common/storage-dotnet-shared-access-signature-part-1.md) a tárfiókban lévő fájlok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="59d0b-151">And, you don't need to worry about generating [SAS URLs](../storage/common/storage-dotnet-shared-access-signature-part-1.md) to provide access to the files in your Storage account.</span></span> <span data-ttu-id="59d0b-152">Kötegelt működik az Azure Storage alkalmazáscsomagok tárolására is, és telepítheti azokat a számítási csomópontok a háttérben.</span><span class="sxs-lookup"><span data-stu-id="59d0b-152">Batch works in the background with Azure Storage to store application packages and deploy them to compute nodes.</span></span>

> [!NOTE] 
> <span data-ttu-id="59d0b-153">Az indítási tevékenységek összesített mérete nem lehet nagyobb, mint 32768 karaktert, beleértve az erőforrásfájlokat és a környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="59d0b-153">The total size of a start task must be less than or equal to 32768 characters, including resource files and environment variables.</span></span> <span data-ttu-id="59d0b-154">A kezdő tevékenység meghaladja ezt a korlátot, majd az alkalmazás-csomagok használata esetén egy másik lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="59d0b-154">If your start task exceeds this limit, then using application packages is another option.</span></span> <span data-ttu-id="59d0b-155">Is létrehozhat az erőforrás-fájlokat tartalmazó tömörített archívum létrehozása, töltse fel az Azure Storage blob, és ezután bontsa ki a parancssorból a kezdő tevékenység.</span><span class="sxs-lookup"><span data-stu-id="59d0b-155">You can also create a zipped archive containing your resource files, upload it as a blob to Azure Storage, and then unzip it from the command line of your start task.</span></span> 
>
>

## <a name="upload-and-manage-applications"></a><span data-ttu-id="59d0b-156">Alkalmazások kezelését és feltöltését</span><span class="sxs-lookup"><span data-stu-id="59d0b-156">Upload and manage applications</span></span>
<span data-ttu-id="59d0b-157">Használhatja a [Azure-portálon] [ portal] vagy a [Batch Management .NET kódtárral](batch-management-dotnet.md) szalagtár kezelése a Batch-fiók alkalmazás csomagokat.</span><span class="sxs-lookup"><span data-stu-id="59d0b-157">You can use the [Azure portal][portal] or the [Batch Management .NET](batch-management-dotnet.md) library to manage the application packages in your Batch account.</span></span> <span data-ttu-id="59d0b-158">A következő néhány szakaszokban először megmutatjuk, hogyan tárfiók hivatkozásra, majd további alkalmazásokat és a csomagok és kezelnie azokat a portállal ismertetik.</span><span class="sxs-lookup"><span data-stu-id="59d0b-158">In the next few sections, we first show how to link a Storage account, then discuss adding applications and packages and managing them with the portal.</span></span>

### <a name="link-a-storage-account"></a><span data-ttu-id="59d0b-159">A tárfiók csatolása</span><span class="sxs-lookup"><span data-stu-id="59d0b-159">Link a Storage account</span></span>
<span data-ttu-id="59d0b-160">Alkalmazáscsomagok használatához először a Batch-fiókhoz kell kapcsolni egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="59d0b-160">To use application packages, you must first link an Azure Storage account to your Batch account.</span></span> <span data-ttu-id="59d0b-161">Ha még nincs konfigurálva a Storage-fiók, az Azure portálon kattintson az első alkalommal figyelmeztetést jelenít meg a **alkalmazások** csempére a **a Batch-fiók** panelen.</span><span class="sxs-lookup"><span data-stu-id="59d0b-161">If you have not yet configured a Storage account, the Azure portal displays a warning the first time you click the **Applications** tile in the **Batch account** blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="59d0b-162">Jelenleg a Batch-támogatja *csak* a **általános célú** tárfióktípus 5, lépésben leírt [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account), a [kapcsolatos Azure Storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="59d0b-162">Batch currently supports *only* the **General-purpose** storage account type as described in step 5, [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account), in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="59d0b-163">Egy Azure Storage-fiók összekötése a Batch-fiókhoz, kapcsolja *csak* egy **általános célú** storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="59d0b-163">When you link an Azure Storage account to your Batch account, link *only* a **General-purpose** storage account.</span></span>
> 
> 

<span data-ttu-id="59d0b-164">!["Nincs beállítva tárfiók" figyelmeztetés Azure-portálon][9]</span><span class="sxs-lookup"><span data-stu-id="59d0b-164">!['No storage account configured' warning in Azure portal][9]</span></span>

<span data-ttu-id="59d0b-165">A Batch szolgáltatás a kapcsolódó tárfiók a alkalmazáscsomagok tárolására használja.</span><span class="sxs-lookup"><span data-stu-id="59d0b-165">The Batch service uses the associated Storage account to store your application packages.</span></span> <span data-ttu-id="59d0b-166">A két fiók csatolta, miután kötegelt automatikusan telepítheti a csomagokat, a számítási csomópontok a csatolt tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="59d0b-166">After you've linked the two accounts, Batch can automatically deploy the packages stored in the linked Storage account to your compute nodes.</span></span> <span data-ttu-id="59d0b-167">A Batch-fiók a tárfiók csatolásához kattintson **tárolási Fiókbeállítások** a a **figyelmeztetés** panelt, és kattintson **Tárfiók** a a **Tárfiók** panelen.</span><span class="sxs-lookup"><span data-stu-id="59d0b-167">To link a Storage account to your Batch account, click **Storage account settings** on the **Warning** blade, and then click **Storage Account** on the **Storage Account** blade.</span></span>

<span data-ttu-id="59d0b-168">![Storage-fiók panelen válassza az Azure-portálon][10]</span><span class="sxs-lookup"><span data-stu-id="59d0b-168">![Choose storage account blade in Azure portal][10]</span></span>

<span data-ttu-id="59d0b-169">Azt javasoljuk, hogy hozzon létre egy tárfiókot *kifejezetten* a Batch-fiókhoz való használatra, és jelölje ki itt.</span><span class="sxs-lookup"><span data-stu-id="59d0b-169">We recommend that you create a Storage account *specifically* for use with your Batch account, and select it here.</span></span> <span data-ttu-id="59d0b-170">A storage-fiók létrehozásával kapcsolatos további információkért lásd: "Create a Storage-fiók" az [Azure Storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="59d0b-170">For details about how to create a storage account, see "Create a Storage account" in [About Azure Storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="59d0b-171">Miután létrehozott egy tárfiókot, majd társíthatja azt a Batch-fiók használatával a **Tárfiók** panelen.</span><span class="sxs-lookup"><span data-stu-id="59d0b-171">After you've created a Storage account, you can then link it to your Batch account by using the **Storage Account** blade.</span></span>

> [!WARNING]
> <span data-ttu-id="59d0b-172">A Batch szolgáltatás Azure tárhelyét használja az alkalmazáscsomagok blokkblobként tárolni.</span><span class="sxs-lookup"><span data-stu-id="59d0b-172">The Batch service uses Azure Storage to store your application packages as block blobs.</span></span> <span data-ttu-id="59d0b-173">Ön [szokásos módon felszámított] [ storage_pricing] a blokk blob adatok.</span><span class="sxs-lookup"><span data-stu-id="59d0b-173">You are [charged as normal][storage_pricing] for the block blob data.</span></span> <span data-ttu-id="59d0b-174">Győződjön meg arról, fontolja meg a méretét és az alkalmazáscsomagok számát, és bizonyos időközönként eltávolítja az elavult csomagok költségek minimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="59d0b-174">Be sure to consider the size and number of your application packages, and periodically remove deprecated packages to minimize costs.</span></span>
> 
> 

### <a name="view-current-applications"></a><span data-ttu-id="59d0b-175">Aktuális alkalmazások megtekintése</span><span class="sxs-lookup"><span data-stu-id="59d0b-175">View current applications</span></span>
<span data-ttu-id="59d0b-176">A Batch-fiókhoz az alkalmazások megtekintéséhez kattintson a **alkalmazások** menüpont megjelenítése közben a bal oldali menüben a **a Batch-fiók** panelen.</span><span class="sxs-lookup"><span data-stu-id="59d0b-176">To view the applications in your Batch account, click the **Applications** menu item in the left menu while viewing the **Batch account** blade.</span></span>

<span data-ttu-id="59d0b-177">![Alkalmazások csempe][2]</span><span class="sxs-lookup"><span data-stu-id="59d0b-177">![Applications tile][2]</span></span>

<span data-ttu-id="59d0b-178">Ezzel a beállítással menü megnyitása a **alkalmazások** panel:</span><span class="sxs-lookup"><span data-stu-id="59d0b-178">Selecting this menu option opens the **Applications** blade:</span></span>

<span data-ttu-id="59d0b-179">![Alkalmazások listája][3]</span><span class="sxs-lookup"><span data-stu-id="59d0b-179">![List applications][3]</span></span>

<span data-ttu-id="59d0b-180">A **alkalmazások** panel megjeleníti minden egyes alkalmazás Azonosítójának fiókját és a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="59d0b-180">The **Applications** blade displays the ID of each application in your account and the following properties:</span></span>

* <span data-ttu-id="59d0b-181">**Csomagok**: A jelen alkalmazáshoz rendelt verziók számát.</span><span class="sxs-lookup"><span data-stu-id="59d0b-181">**Packages**: The number of versions associated with this application.</span></span>
* <span data-ttu-id="59d0b-182">**Alapértelmezett verzió**: A telepített, ha nem jelzi azt egy verzió az alkalmazás egy készlet megadott verziója.</span><span class="sxs-lookup"><span data-stu-id="59d0b-182">**Default version**: The application version installed if you do not indicate a version when you specify the application for a pool.</span></span> <span data-ttu-id="59d0b-183">Ez a beállítás nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="59d0b-183">This setting is optional.</span></span>
* <span data-ttu-id="59d0b-184">**Frissítések engedélyezése**: az érték, amely meghatározza, hogy frissíti a csomagot, törléseket és kiegészítéseit engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="59d0b-184">**Allow updates**: The value that specifies whether package updates, deletions, and additions are allowed.</span></span> <span data-ttu-id="59d0b-185">Ha a beállított érték **nem**, csomag frissítések és törlések le van tiltva az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="59d0b-185">If this is set to **No**, package updates and deletions are disabled for the application.</span></span> <span data-ttu-id="59d0b-186">Csak új alkalmazáscsomag-verziók is hozzáadhatók.</span><span class="sxs-lookup"><span data-stu-id="59d0b-186">Only new application package versions can be added.</span></span> <span data-ttu-id="59d0b-187">Az alapértelmezett érték **Igen**.</span><span class="sxs-lookup"><span data-stu-id="59d0b-187">The default is **Yes**.</span></span>

### <a name="view-application-details"></a><span data-ttu-id="59d0b-188">Az alkalmazás részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="59d0b-188">View application details</span></span>
<span data-ttu-id="59d0b-189">Az alkalmazás részleteit tartalmazó panel megnyitásához, válassza ki az alkalmazást a **alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="59d0b-189">To open the blade that includes the details for an application, select the application in the **Applications** blade.</span></span>

<span data-ttu-id="59d0b-190">![Az alkalmazás részletei][4]</span><span class="sxs-lookup"><span data-stu-id="59d0b-190">![Application details][4]</span></span>

<span data-ttu-id="59d0b-191">Az alkalmazás részleteit megjelenítő panelen található az alkalmazás a következő beállításokat lehet megadni.</span><span class="sxs-lookup"><span data-stu-id="59d0b-191">In the application details blade, you can configure the following settings for your application.</span></span>

* <span data-ttu-id="59d0b-192">**Frissítések engedélyezése**: Adja meg, hogy az alkalmazáscsomagok is frissíthető és nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="59d0b-192">**Allow updates**: Specify whether its application packages can be updated or deleted.</span></span> <span data-ttu-id="59d0b-193">Tekintse meg a "Frissíteni vagy törölni egy alkalmazáscsomagot" a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="59d0b-193">See "Update or Delete an application package" later in this article.</span></span>
* <span data-ttu-id="59d0b-194">**Alapértelmezett verzió**: Adjon meg egy számítási csomópontjain telepítendő alapértelmezett alkalmazáscsomagot.</span><span class="sxs-lookup"><span data-stu-id="59d0b-194">**Default version**: Specify a default application package to deploy to compute nodes.</span></span>
* <span data-ttu-id="59d0b-195">**Megjelenített név**: Adjon meg egy rövid nevet a kötegelt megoldás lehet használni, amikor megjeleníti az alkalmazás adatait, például a felhasználói felületen, a szolgáltatás, amely a kötegelt keresztül az ügyfeleknek.</span><span class="sxs-lookup"><span data-stu-id="59d0b-195">**Display name**: Specify a friendly name that your Batch solution can use when it displays information about the application, for example, in the UI of a service that you provide to your customers through Batch.</span></span>

### <a name="add-a-new-application"></a><span data-ttu-id="59d0b-196">Új alkalmazás felvétele</span><span class="sxs-lookup"><span data-stu-id="59d0b-196">Add a new application</span></span>
<span data-ttu-id="59d0b-197">Hozzon létre egy új alkalmazást, vegye fel egy alkalmazáscsomagot, és adjon meg egy új, egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="59d0b-197">To create a new application, add an application package and specify a new, unique application ID.</span></span> <span data-ttu-id="59d0b-198">Az első alkalmazáscsomag az új alkalmazás azonosítójával hozzáadott is létrehoz az új alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="59d0b-198">The first application package that you add with the new application ID also creates the new application.</span></span>

<span data-ttu-id="59d0b-199">Kattintson a **Hozzáadás** a a **alkalmazások** panelt, és nyissa meg a **új alkalmazás** panelen.</span><span class="sxs-lookup"><span data-stu-id="59d0b-199">Click **Add** on the **Applications** blade to open the **New application** blade.</span></span>

<span data-ttu-id="59d0b-200">![Új alkalmazás panel az Azure-portálon][5]</span><span class="sxs-lookup"><span data-stu-id="59d0b-200">![New application blade in Azure portal][5]</span></span>

<span data-ttu-id="59d0b-201">A **új alkalmazás** panelen a következő mezőket az új alkalmazások és alkalmazáscsomag beállításainak megadásához nyújt.</span><span class="sxs-lookup"><span data-stu-id="59d0b-201">The **New application** blade provides the following fields to specify the settings of your new application and application package.</span></span>

<span data-ttu-id="59d0b-202">**Alkalmazásazonosító**</span><span class="sxs-lookup"><span data-stu-id="59d0b-202">**Application id**</span></span>

<span data-ttu-id="59d0b-203">Ez a mező az új alkalmazást, amely Azure Kötegazonosító szabványos érvényesítési szabályok Azonosítóját adja meg.</span><span class="sxs-lookup"><span data-stu-id="59d0b-203">This field specifies the ID of your new application, which is subject to the standard Azure Batch ID validation rules.</span></span> <span data-ttu-id="59d0b-204">Az Alkalmazásazonosító biztosítani a szabályok a következők:</span><span class="sxs-lookup"><span data-stu-id="59d0b-204">The rules for providing an application ID are as follows:</span></span>

* <span data-ttu-id="59d0b-205">Windows-csomópont a az azonosító az alfanumerikus karaktereket, kötőjeleket és aláhúzásjeleket tartalmazhat tetszőleges kombinációját tartalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="59d0b-205">On Windows nodes, the ID can contain any combination of alphanumeric characters, hyphens, and underscores.</span></span> <span data-ttu-id="59d0b-206">Linux-csomópont csak alfanumerikus karaktereket és aláhúzás karaktereket tartalmazhatnak engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="59d0b-206">On Linux nodes, only alphanumeric characters and underscores are permitted.</span></span>
* <span data-ttu-id="59d0b-207">Nem lehet hosszabb 64 karakternél.</span><span class="sxs-lookup"><span data-stu-id="59d0b-207">Cannot contain more than 64 characters.</span></span>
* <span data-ttu-id="59d0b-208">A Batch-fiók belül egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="59d0b-208">Must be unique within the Batch account.</span></span>
* <span data-ttu-id="59d0b-209">Egy nagybetűket és a nagybetűk között.</span><span class="sxs-lookup"><span data-stu-id="59d0b-209">Is case-preserving and case-insensitive.</span></span>

<span data-ttu-id="59d0b-210">**Verzió**</span><span class="sxs-lookup"><span data-stu-id="59d0b-210">**Version**</span></span>

<span data-ttu-id="59d0b-211">Ez a mező határozza meg a feltölteni kívánt alkalmazáscsomag verzióját.</span><span class="sxs-lookup"><span data-stu-id="59d0b-211">This field specifies the version of the application package you are uploading.</span></span> <span data-ttu-id="59d0b-212">Verzió-karakterlánc a következő ellenőrzési szabályok vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="59d0b-212">Version strings are subject to the following validation rules:</span></span>

* <span data-ttu-id="59d0b-213">Windows-csomópont a verzió-karakterlánca az alfanumerikus karaktereket, kötőjeleket, aláhúzásjeleket és időszakok tetszőleges kombinációját tartalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="59d0b-213">On Windows nodes, the version string can contain any combination of alphanumeric characters, hyphens, underscores, and periods.</span></span> <span data-ttu-id="59d0b-214">Linux csomópontján verzió-karakterlánca csak alfanumerikus karaktereket és aláhúzásjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="59d0b-214">On Linux nodes, the version string can contain only alphanumeric characters and underscores.</span></span>
* <span data-ttu-id="59d0b-215">Nem lehet hosszabb 64 karakternél.</span><span class="sxs-lookup"><span data-stu-id="59d0b-215">Cannot contain more than 64 characters.</span></span>
* <span data-ttu-id="59d0b-216">Az alkalmazáson belül egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="59d0b-216">Must be unique within the application.</span></span>
* <span data-ttu-id="59d0b-217">A rendszer megőrzi és a nagybetűk között.</span><span class="sxs-lookup"><span data-stu-id="59d0b-217">Are case-preserving and case-insensitive.</span></span>

<span data-ttu-id="59d0b-218">**Alkalmazáscsomag**</span><span class="sxs-lookup"><span data-stu-id="59d0b-218">**Application package**</span></span>

<span data-ttu-id="59d0b-219">A bináris alkalmazásfájlokat tartalmazó .zip fájl és a fájlokat, amelyek szükségesek az alkalmazás ebben a mezőben adja meg.</span><span class="sxs-lookup"><span data-stu-id="59d0b-219">This field specifies the .zip file that contains the application binaries and supporting files that are required to execute the application.</span></span> <span data-ttu-id="59d0b-220">Kattintson a **válasszon ki egy fájlt** mező vagy a mappa ikonra kattintva jelölje ki az alkalmazás fájlokat tartalmazó .zip-fájlt.</span><span class="sxs-lookup"><span data-stu-id="59d0b-220">Click the **Select a file** box or the folder icon to browse to and select a .zip file that contains your application's files.</span></span>

<span data-ttu-id="59d0b-221">Miután kijelölt egy fájlt, kattintson **OK** Azure Storage való feltöltés indításához.</span><span class="sxs-lookup"><span data-stu-id="59d0b-221">After you've selected a file, click **OK** to begin the upload to Azure Storage.</span></span> <span data-ttu-id="59d0b-222">A feltöltési művelet befejezésekor a portál egy értesítést jelenít meg, és a panel bezárása után.</span><span class="sxs-lookup"><span data-stu-id="59d0b-222">When the upload operation is complete, the portal displays a notification and closes the blade.</span></span> <span data-ttu-id="59d0b-223">Attól függően, hogy a feltölteni kívánt fájl méretétől és a hálózati kapcsolat sebességét a művelet eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="59d0b-223">Depending on the size of the file that you are uploading and the speed of your network connection, this operation may take some time.</span></span>

> [!WARNING]
> <span data-ttu-id="59d0b-224">Ne zárja be a **új alkalmazás** panel a feltöltési művelet befejezése előtt.</span><span class="sxs-lookup"><span data-stu-id="59d0b-224">Do not close the **New application** blade before the upload operation is complete.</span></span> <span data-ttu-id="59d0b-225">Ezzel megszűnik a feltöltési folyamat.</span><span class="sxs-lookup"><span data-stu-id="59d0b-225">Doing so will stop the upload process.</span></span>
> 
> 

### <a name="add-a-new-application-package"></a><span data-ttu-id="59d0b-226">Új alkalmazás csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="59d0b-226">Add a new application package</span></span>
<span data-ttu-id="59d0b-227">Alkalmazás új csomagverziójának egy meglévő alkalmazás hozzáadásához válassza ki az alkalmazás a **alkalmazások** panelen kattintson **csomagok**, kattintson a **Hozzáadás** megnyitásához a **Hozzáadás csomag** panelen.</span><span class="sxs-lookup"><span data-stu-id="59d0b-227">To add a new application package version for an existing application, select an application in the **Applications** blade, click **Packages**, then click **Add** to open the **Add package** blade.</span></span>

<span data-ttu-id="59d0b-228">![Adja hozzá az alkalmazás csomag panel Azure-portálon][8]</span><span class="sxs-lookup"><span data-stu-id="59d0b-228">![Add application package blade in Azure portal][8]</span></span>

<span data-ttu-id="59d0b-229">Ahogy látja, a mezők megegyeznek a **új alkalmazás** panelen, de a **alkalmazásazonosító** mezőben le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="59d0b-229">As you can see, the fields match those of the **New application** blade, but the **Application id** box is disabled.</span></span> <span data-ttu-id="59d0b-230">Úgy, ahogy az új alkalmazás, adja meg a **verzió** az új csomag esetében keresse meg a **alkalmazáscsomag** .zip fájlt, majd kattintson az **OK** a csomag feltöltése.</span><span class="sxs-lookup"><span data-stu-id="59d0b-230">As you did for the new application, specify the **Version** for your new package, browse to your **Application package** .zip file, then click **OK** to upload the package.</span></span>

### <a name="update-or-delete-an-application-package"></a><span data-ttu-id="59d0b-231">Frissítés vagy törlés alkalmazáscsomag</span><span class="sxs-lookup"><span data-stu-id="59d0b-231">Update or delete an application package</span></span>
<span data-ttu-id="59d0b-232">Frissítés, vagy törölje a meglévő alkalmazáscsomag, nyissa meg a az alkalmazás részleteit megjelenítő panelen, kattintson **csomagok** megnyitásához a **csomagok** panelen kattintson a **három pont** a a sor az alkalmazáscsomag, amelyet szeretne módosítani, és jelölje ki a végrehajtani kívánt műveletet.</span><span class="sxs-lookup"><span data-stu-id="59d0b-232">To update or delete an existing application package, open the details blade for the application, click **Packages** to open the **Packages** blade, click the **ellipsis** in the row of the application package that you want to modify, and select the action that you want to perform.</span></span>

<span data-ttu-id="59d0b-233">![Frissítés vagy törlés csomag Azure-portálon][7]</span><span class="sxs-lookup"><span data-stu-id="59d0b-233">![Update or delete package in Azure portal][7]</span></span>

<span data-ttu-id="59d0b-234">**Update**</span><span class="sxs-lookup"><span data-stu-id="59d0b-234">**Update**</span></span>

<span data-ttu-id="59d0b-235">Amikor rákattint **frissítés**, a *csomag* panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="59d0b-235">When you click **Update**, the *Update package* blade is displayed.</span></span> <span data-ttu-id="59d0b-236">Ezen a panelen hasonlít a *új alkalmazáscsomag* panelen, azonban csak a csomag kiválasztási mezőben engedélyezve van, lehetővé téve adja meg az új ZIP-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="59d0b-236">This blade is similar to the *New application package* blade, however only the package selection field is enabled, allowing you to specify a new ZIP file to upload.</span></span>

<span data-ttu-id="59d0b-237">![Frissítési csomag panel az Azure-portálon][11]</span><span class="sxs-lookup"><span data-stu-id="59d0b-237">![Update package blade in Azure portal][11]</span></span>

<span data-ttu-id="59d0b-238">**Törlés**</span><span class="sxs-lookup"><span data-stu-id="59d0b-238">**Delete**</span></span>

<span data-ttu-id="59d0b-239">Amikor rákattint **törlése**, a rendszer felkéri a csomag verziószáma törlésének megerősítése és kötegelt törli a csomag az Azure Storage-ból.</span><span class="sxs-lookup"><span data-stu-id="59d0b-239">When you click **Delete**, you are asked to confirm the deletion of the package version, and Batch deletes the package from Azure Storage.</span></span> <span data-ttu-id="59d0b-240">Ha törli egy alkalmazás alapértelmezett verziója a **alapértelmezett verzió** beállítást a rendszer eltávolítja az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="59d0b-240">If you delete the default version of an application, the **Default version** setting is removed for the application.</span></span>

<span data-ttu-id="59d0b-241">![Alkalmazás törlése][12]</span><span class="sxs-lookup"><span data-stu-id="59d0b-241">![Delete application ][12]</span></span>

## <a name="install-applications-on-compute-nodes"></a><span data-ttu-id="59d0b-242">Alkalmazások telepítése a számítási csomópontok</span><span class="sxs-lookup"><span data-stu-id="59d0b-242">Install applications on compute nodes</span></span>
<span data-ttu-id="59d0b-243">Most, hogy megismerte az Azure-portálon az alkalmazáscsomagok kezelése, hogyan telepheti őket a számítási csomópontok és kötegelt feladatok futtathatók is tárgyaljuk.</span><span class="sxs-lookup"><span data-stu-id="59d0b-243">Now that you've learned how to manage application packages with the Azure portal, we can discuss how to deploy them to compute nodes and run them with Batch tasks.</span></span>

### <a name="install-pool-application-packages"></a><span data-ttu-id="59d0b-244">Készlet alkalmazáscsomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="59d0b-244">Install pool application packages</span></span>
<span data-ttu-id="59d0b-245">Egy alkalmazás telepítéséhez minden számítási csomóponton a készletben, adjon meg egy vagy több alkalmazáscsomagot *hivatkozások* a készlet.</span><span class="sxs-lookup"><span data-stu-id="59d0b-245">To install an application package on all compute nodes in a pool, specify one or more application package *references* for the pool.</span></span> <span data-ttu-id="59d0b-246">Az alkalmazáscsomagok számára egy készlet megadott minden számítási csomóponton települnek, amikor a csomópont csatlakozik a készlethez, és amikor a csomópont újraindítása után, vagy lemezképet.</span><span class="sxs-lookup"><span data-stu-id="59d0b-246">The application packages that you specify for a pool are installed on each compute node when that node joins the pool, and when the node is rebooted or reimaged.</span></span>

<span data-ttu-id="59d0b-247">A Batch .NET, adjon meg egy vagy több [CloudPool][net_cloudpool].[ ApplicationPackageReferences] [ net_cloudpool_pkgref] amikor létrehoz egy új készletet, vagy egy meglévő készlet.</span><span class="sxs-lookup"><span data-stu-id="59d0b-247">In Batch .NET, specify one or more [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref] when you create a new pool, or for an existing pool.</span></span> <span data-ttu-id="59d0b-248">A [ApplicationPackageReference] [ net_pkgref] osztály megadja egy alkalmazás-Azonosítót és verziót telepíteni a készlet számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="59d0b-248">The [ApplicationPackageReference][net_pkgref] class specifies an application ID and version to install on a pool's compute nodes.</span></span>

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package is installed on each.
await myCloudPool.CommitAsync();
```

> [!IMPORTANT]
> <span data-ttu-id="59d0b-249">Ha egy alkalmazás csomag központi telepítését a bármilyen okból nem sikerül, a Batch szolgáltatás jelöli meg a csomópont [használhatatlanná][net_nodestate], és nincsenek feladatok vannak ütemezve ezen a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="59d0b-249">If an application package deployment fails for any reason, the Batch service marks the node [unusable][net_nodestate], and no tasks are scheduled for execution on that node.</span></span> <span data-ttu-id="59d0b-250">Ebben az esetben kell **indítsa újra a** a központi csomagtelepítés újraindítani a csomópontot.</span><span class="sxs-lookup"><span data-stu-id="59d0b-250">In this case, you should **restart** the node to reinitiate the package deployment.</span></span> <span data-ttu-id="59d0b-251">A csomópont újraindítása is lehetővé teszi, hogy a feladatütemezés ismét a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="59d0b-251">Restarting the node also enables task scheduling again on the node.</span></span>
> 
> 

### <a name="install-task-application-packages"></a><span data-ttu-id="59d0b-252">A feladat alkalmazáscsomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="59d0b-252">Install task application packages</span></span>
<span data-ttu-id="59d0b-253">Hasonló a készlethez, megadott alkalmazáscsomag *hivatkozások* feladathoz.</span><span class="sxs-lookup"><span data-stu-id="59d0b-253">Similar to a pool, you specify application package *references* for a task.</span></span> <span data-ttu-id="59d0b-254">Egy feladat ütemezése egy csomópontján futtatni, a csomag letöltése és kibontása csak a parancssor a feladat végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="59d0b-254">When a task is scheduled to run on a node, the package is downloaded and extracted just before the task's command line is executed.</span></span> <span data-ttu-id="59d0b-255">Ha a megadott csomag és verziója már telepítve van a csomópont, a csomag letöltése nem történik meg, és a meglévő csomag használata.</span><span class="sxs-lookup"><span data-stu-id="59d0b-255">If a specified package and version is already installed on the node, the package is not downloaded and the existing package is used.</span></span>

<span data-ttu-id="59d0b-256">A feladat alkalmazáscsomag telepítéséhez úgy állítsa be a feladatnak [CloudTask][net_cloudtask].[ ApplicationPackageReferences] [ net_cloudtask_pkgref] tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="59d0b-256">To install a task application package, configure the task's [CloudTask][net_cloudtask].[ApplicationPackageReferences][net_cloudtask_pkgref] property:</span></span>

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a><span data-ttu-id="59d0b-257">A telepített alkalmazások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="59d0b-257">Execute the installed applications</span></span>
<span data-ttu-id="59d0b-258">A csomagok egy készletet vagy feladathoz megadott letöltődnek és elnevezett címtárhoz az kibontotta a `AZ_BATCH_ROOT_DIR` a csomópont.</span><span class="sxs-lookup"><span data-stu-id="59d0b-258">The packages that you've specified for a pool or task are downloaded and extracted to a named directory within the `AZ_BATCH_ROOT_DIR` of the node.</span></span> <span data-ttu-id="59d0b-259">Kötegelt is létrehoz egy környezeti változó, amely tartalmazza az elnevezett könyvtár elérési útját.</span><span class="sxs-lookup"><span data-stu-id="59d0b-259">Batch also creates an environment variable that contains the path to the named directory.</span></span> <span data-ttu-id="59d0b-260">A feladat parancssorokat e környezeti változó használatával való hivatkozáskor az alkalmazás a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="59d0b-260">Your task command lines use this environment variable when referencing the application on the node.</span></span> 

<span data-ttu-id="59d0b-261">Windows-csomópont a változó a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="59d0b-261">On Windows nodes, the variable is in the following format:</span></span>

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

<span data-ttu-id="59d0b-262">Linux-csomópont formátuma némileg eltérő.</span><span class="sxs-lookup"><span data-stu-id="59d0b-262">On Linux nodes, the format is slightly different.</span></span> <span data-ttu-id="59d0b-263">A pontok (.), kötőjelet (-) és a kettős kereszttel (#) vannak egybesimított-e az aláhúzás karaktereket tartalmazhatnak a környezeti változóban.</span><span class="sxs-lookup"><span data-stu-id="59d0b-263">Periods (.), hypens (-) and number signs (#) are flattened to underscores in the environment variable.</span></span> <span data-ttu-id="59d0b-264">Példa:</span><span class="sxs-lookup"><span data-stu-id="59d0b-264">For example:</span></span>

```
Linux:
AZ_BATCH_APP_PACKAGE_APPLICATIONID_version
```

<span data-ttu-id="59d0b-265">`APPLICATIONID`és `version` olyan értékek, amelyek megfelelnek a telepítéshez megadott alkalmazás- és csomag verziója.</span><span class="sxs-lookup"><span data-stu-id="59d0b-265">`APPLICATIONID` and `version` are values that correspond to the application and package version you've specified for deployment.</span></span> <span data-ttu-id="59d0b-266">Például, ha a megadott alkalmazás 2.7-es verzió *keverőgép* telepítendő Windows csomópont, a feladat parancssorokat használja ehhez a környezeti változóhoz a fájlok eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="59d0b-266">For example, if you specified that version 2.7 of application *blender* should be installed on Windows nodes, your task command lines would use this environment variable to access its files:</span></span>

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

<span data-ttu-id="59d0b-267">Linux-csomópont adja meg a környezeti változó a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="59d0b-267">On Linux nodes, specify the environment variable in this format:</span></span>

```
Linux:
AZ_BATCH_APP_PACKAGE_BLENDER_2_7
``` 

<span data-ttu-id="59d0b-268">Amikor tölt fel egy alkalmazáscsomagot, megadhatja a központi telepítése a számítási csomópontok alapértelmezett változata.</span><span class="sxs-lookup"><span data-stu-id="59d0b-268">When you upload an application package, you can specify a default version to deploy to your compute nodes.</span></span> <span data-ttu-id="59d0b-269">Ha egy alkalmazás alapértelmezett verziót adott meg, a verzió utótag kihagyhatja, ha az alkalmazás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="59d0b-269">If you have specified a default version for an application, you can omit the version suffix when you reference the application.</span></span> <span data-ttu-id="59d0b-270">Az alapértelmezett Alkalmazásverzió adhat meg az Azure portálon, az alkalmazások panel, ahogy az [alkalmazások kezelését és feltöltését](#upload-and-manage-applications).</span><span class="sxs-lookup"><span data-stu-id="59d0b-270">You can specify the default application version in the Azure portal, on the Applications blade, as shown in [Upload and manage applications](#upload-and-manage-applications).</span></span>

<span data-ttu-id="59d0b-271">Például, ha az alkalmazás alapértelmezett verziójaként "2.7" beállíthatja *keverőgép*, és a feladatok hivatkozik a következő környezeti változót, majd a Windows-csomópontok 2.7-es verzió hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="59d0b-271">For example, if you set "2.7" as the default version for application *blender*, and your tasks reference the following environment variable, then your Windows nodes will execute version 2.7:</span></span>

`AZ_BATCH_APP_PACKAGE_BLENDER`

<span data-ttu-id="59d0b-272">A következő kódrészlet egy példa a feladat parancssori indító alapértelmezett verzióját jeleníti meg a *keverőgép* alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="59d0b-272">The following code snippet shows an example task command line that launches the default version of the *blender* application:</span></span>

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> <span data-ttu-id="59d0b-273">Lásd: [környezeti beállítások feladatok](batch-api-basics.md#environment-settings-for-tasks) a a [Batch funkcióinak áttekintése](batch-api-basics.md) számítási csomópont környezet beállításaival kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="59d0b-273">See [Environment settings for tasks](batch-api-basics.md#environment-settings-for-tasks) in the [Batch feature overview](batch-api-basics.md) for more information about compute node environment settings.</span></span>
> 
> 

## <a name="update-a-pools-application-packages"></a><span data-ttu-id="59d0b-274">Készlet alkalmazáscsomagjainak frissítése</span><span class="sxs-lookup"><span data-stu-id="59d0b-274">Update a pool's application packages</span></span>
<span data-ttu-id="59d0b-275">Ha egy meglévő készlet már be van állítva egy alkalmazási csomaggal rendelkező, a készlet új csomagot is megadhat.</span><span class="sxs-lookup"><span data-stu-id="59d0b-275">If an existing pool has already been configured with an application package, you can specify a new package for the pool.</span></span> <span data-ttu-id="59d0b-276">Ha megadja az új csomag leírását, a készletbe, a következő apply:</span><span class="sxs-lookup"><span data-stu-id="59d0b-276">If you specify a new package reference for a pool, the following apply:</span></span>

* <span data-ttu-id="59d0b-277">A Batch szolgáltatás telepítheti az újonnan meghatározott csomag, az összes olyan új csomópont, amelyhez csatlakozni a készlet, illetve bármely létező csomópontján újraindították vagy lemezképet.</span><span class="sxs-lookup"><span data-stu-id="59d0b-277">The Batch service installs the newly specified package on all new nodes that join the pool and on any existing node that is rebooted or reimaged.</span></span>
* <span data-ttu-id="59d0b-278">A számítási csomópontot, amely már a készletben található csomaghivatkozásokhoz frissítésekor nem telepíti automatikusan az új alkalmazáscsomagot.</span><span class="sxs-lookup"><span data-stu-id="59d0b-278">Compute nodes that are already in the pool when you update the package references do not automatically install the new application package.</span></span> <span data-ttu-id="59d0b-279">Ezek a számítási csomópont újraindítása után kell lenniük, vagy az új csomag fogadásához lemezképet.</span><span class="sxs-lookup"><span data-stu-id="59d0b-279">These compute nodes must be rebooted or reimaged to receive the new package.</span></span>
* <span data-ttu-id="59d0b-280">Amikor új csomagot telepít, a létrehozott környezeti változók tükrözze az új alkalmazás csomaghivatkozásokhoz.</span><span class="sxs-lookup"><span data-stu-id="59d0b-280">When a new package is deployed, the created environment variables reflect the new application package references.</span></span>

<span data-ttu-id="59d0b-281">Ebben a példában a meglévő készlet 2.7-es verziójával rendelkezik a *keverőgép* egyik konfigurált alkalmazás a [CloudPool][net_cloudpool].[ ApplicationPackageReferences][net_cloudpool_pkgref].</span><span class="sxs-lookup"><span data-stu-id="59d0b-281">In this example, the existing pool has version 2.7 of the *blender* application configured as one of its [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref].</span></span> <span data-ttu-id="59d0b-282">A készlet csomópontok frissítése 2.76b verziójával, adjon meg egy új [ApplicationPackageReference] [ net_pkgref] új verziója, és véglegesítse a módosítás.</span><span class="sxs-lookup"><span data-stu-id="59d0b-282">To update the pool's nodes with version 2.76b, specify a new [ApplicationPackageReference][net_pkgref] with the new version, and commit the change.</span></span>

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

<span data-ttu-id="59d0b-283">Most, hogy az új verzióra van beállítva, a Batch szolgáltatás 2.76b verziót telepíti, sem *új* csomópont, amelyhez csatlakozik a készlethez.</span><span class="sxs-lookup"><span data-stu-id="59d0b-283">Now that the new version has been configured, the Batch service installs version 2.76b to any *new* node that joins the pool.</span></span> <span data-ttu-id="59d0b-284">2.76b a csomópontokon telepítendő *már* a tárolókészletben, indítsa újra, vagy újból lemezképet létrehozni őket.</span><span class="sxs-lookup"><span data-stu-id="59d0b-284">To install 2.76b on the nodes that are *already* in the pool, reboot or reimage them.</span></span> <span data-ttu-id="59d0b-285">Vegye figyelembe, hogy újraindított csomópontok tartsa meg a fájlok korábbi csomag telepítések.</span><span class="sxs-lookup"><span data-stu-id="59d0b-285">Note that rebooted nodes retain the files from previous package deployments.</span></span>

## <a name="list-the-applications-in-a-batch-account"></a><span data-ttu-id="59d0b-286">A Batch-fiók alkalmazások felsorolása</span><span class="sxs-lookup"><span data-stu-id="59d0b-286">List the applications in a Batch account</span></span>
<span data-ttu-id="59d0b-287">Az alkalmazások és a csomagok Batch-fiók is listázhatja használatával a [ApplicationOperations][net_appops].[ ListApplicationSummaries] [ net_appops_listappsummaries] metódust.</span><span class="sxs-lookup"><span data-stu-id="59d0b-287">You can list the applications and their packages in a Batch account by using the [ApplicationOperations][net_appops].[ListApplicationSummaries][net_appops_listappsummaries] method.</span></span>

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a><span data-ttu-id="59d0b-288">Burkolja</span><span class="sxs-lookup"><span data-stu-id="59d0b-288">Wrap up</span></span>
<span data-ttu-id="59d0b-289">Az alkalmazáscsomagok válassza ki arra, hogy az alkalmazásokat, és adja meg a pontos verziót kell használni, ha engedélyezve van a szolgáltatással a feladatok feldolgozásában, az ügyfelek segítségével.</span><span class="sxs-lookup"><span data-stu-id="59d0b-289">With application packages, you can help your customers select the applications for their jobs and specify the exact version to use when processing jobs with your Batch-enabled service.</span></span> <span data-ttu-id="59d0b-290">Meg lehet adni az feltöltése és nyomon követheti a szolgáltatás fel saját alkalmazásaikba az ügyfelek képesek.</span><span class="sxs-lookup"><span data-stu-id="59d0b-290">You might also provide the ability for your customers to upload and track their own applications in your service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59d0b-291">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59d0b-291">Next steps</span></span>
* <span data-ttu-id="59d0b-292">A [Batch REST API] [ api_rest] is támogatja az alkalmazás csomagokkal végzett munkához.</span><span class="sxs-lookup"><span data-stu-id="59d0b-292">The [Batch REST API][api_rest] also provides support to work with application packages.</span></span> <span data-ttu-id="59d0b-293">Például tekintse meg a [applicationPackageReferences] [ rest_add_pool_with_packages] elemében [a készlet hozzáadása partner] [ rest_add_pool] további információkat tudhat meg csomagok telepítése a REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="59d0b-293">For example, see the [applicationPackageReferences][rest_add_pool_with_packages] element in [Add a pool to an account][rest_add_pool] for information about how to specify packages to install by using the REST API.</span></span> <span data-ttu-id="59d0b-294">Lásd: [alkalmazások] [ rest_applications] alkalmazással kapcsolatos adatok beszerzése a Batch REST API használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="59d0b-294">See [Applications][rest_applications] for details about how to obtain application information by using the Batch REST API.</span></span>
* <span data-ttu-id="59d0b-295">Megtudhatja, hogyan programozott módon [kezelése az Azure Batch fiókjainak és kvótáinak a Batch Management .NET kódtárral](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="59d0b-295">Learn how to programmatically [manage Azure Batch accounts and quotas with Batch Management .NET](batch-management-dotnet.md).</span></span> <span data-ttu-id="59d0b-296">A [Batch Management .NET kódtárral][api_net_mgmt] könyvtár engedélyezheti a fiók létrehozását és törlését funkciói a kötegelt alkalmazást vagy szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="59d0b-296">The [Batch Management .NET][api_net_mgmt] library can enable account creation and deletion features for your Batch application or service.</span></span>

[api_net]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/client?view=azure-dotnet
[api_net_mgmt]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet
[api_rest]: https://docs.microsoft.com/en-us/rest/api/batchservice/
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Csomagok magas szintű diagramja"
[2]: ./media/batch-application-packages/app_pkg_02.png "Alkalmazások csempe az Azure portálon"
[3]: ./media/batch-application-packages/app_pkg_03.png "Alkalmazások panel az Azure-portálon"
[4]: ./media/batch-application-packages/app_pkg_04.png "Alkalmazás részletei panel az Azure-portálon"
[5]: ./media/batch-application-packages/app_pkg_05.png "Új alkalmazás panel az Azure-portálon"
[7]: ./media/batch-application-packages/app_pkg_07.png "Frissítés vagy törlés csomagok legördülő Azure-portálon"
[8]: ./media/batch-application-packages/app_pkg_08.png "Új alkalmazás csomag panel az Azure-portálon"
[9]: ./media/batch-application-packages/app_pkg_09.png "Csatolt tárolási fiók riasztás"
[10]: ./media/batch-application-packages/app_pkg_10.png "Storage-fiók panelen válassza az Azure-portálon"
[11]: ./media/batch-application-packages/app_pkg_11.png "Frissítési csomag panel az Azure-portálon"
[12]: ./media/batch-application-packages/app_pkg_12.png "Törlési megerősítés csomag Azure-portálon"
