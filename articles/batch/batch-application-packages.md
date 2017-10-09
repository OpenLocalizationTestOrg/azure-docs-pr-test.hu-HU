---
title: "a számítási csomópontok - Azure Batch aaaInstall alkalmazáscsomagok |} Microsoft Docs"
description: "Az Azure Batch tooeasily használata hello alkalmazás csomagok szolgáltatása több alkalmazás kezelése és a kötegelt telepítés verziók számítási csomópontjain."
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
ms.openlocfilehash: 683be7b7f1bd5db7835332016f6dccb72f45c3b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-applications-toocompute-nodes-with-batch-application-packages"></a><span data-ttu-id="10a51-103">Alkalmazások toocompute csomópontokat a kötegelt kérelem csomagok központi telepítése</span><span class="sxs-lookup"><span data-stu-id="10a51-103">Deploy applications toocompute nodes with Batch application packages</span></span>

<span data-ttu-id="10a51-104">hello alkalmazás csomagok szolgáltatás Azure-köteg feladat alkalmazások könnyen kezelhető, és a központi telepítés toohello számítási csomópontok a készlet.</span><span class="sxs-lookup"><span data-stu-id="10a51-104">hello application packages feature of Azure Batch provides easy management of task applications and their deployment toohello compute nodes in your pool.</span></span> <span data-ttu-id="10a51-105">Az alkalmazáscsomagok töltse fel, és a feladatok futnak, beleértve a segédfájlok hello alkalmazások több verziójának kezelése.</span><span class="sxs-lookup"><span data-stu-id="10a51-105">With application packages, you can upload and manage multiple versions of hello applications your tasks run, including their supporting files.</span></span> <span data-ttu-id="10a51-106">Ezután automatikusan telepítheti egy vagy több ezen alkalmazások toohello számítási csomópontok a készlethez.</span><span class="sxs-lookup"><span data-stu-id="10a51-106">You can then automatically deploy one or more of these applications toohello compute nodes in your pool.</span></span>

<span data-ttu-id="10a51-107">Ebből a cikkből megtudhatja, hogyan tooupload és hello Azure-portálon az alkalmazáscsomagok kezelése.</span><span class="sxs-lookup"><span data-stu-id="10a51-107">In this article, you will learn how tooupload and manage application packages in hello Azure portal.</span></span> <span data-ttu-id="10a51-108">Ezután megismerkedhet a hogyan tooinstall őket a készlet számítási csomópontokat a hello [Batch .NET] [ api_net] könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="10a51-108">You will then learn how tooinstall them on a pool's compute nodes with hello [Batch .NET][api_net] library.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="10a51-109">Az alkalmazáscsomagok az összes 2017. július 5. után létrehozott Batch-készleten támogatottak.</span><span class="sxs-lookup"><span data-stu-id="10a51-109">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="10a51-110">2016. március 10. és 5. július 2017 között létrehozott, csak akkor, ha hello készlet lett létrehozva egy felhőalapú szolgáltatás konfigurációja kötegelt készletek azok támogatottak.</span><span class="sxs-lookup"><span data-stu-id="10a51-110">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if hello pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="10a51-111">Kötegelt készletek létrehozott előzetes too10 2016. március nem támogatják az alkalmazáscsomagok.</span><span class="sxs-lookup"><span data-stu-id="10a51-111">Batch pools created prior too10 March 2016 do not support application packages.</span></span>
>
> <span data-ttu-id="10a51-112">hello API-k létrehozására és kezelésére alkalmazáscsomagok részét képező hello [Batch Management .NET kódtárral] [[api_net_mgmt]] könyvtár.</span><span class="sxs-lookup"><span data-stu-id="10a51-112">hello APIs for creating and managing application packages are part of hello [Batch Management .NET][[api_net_mgmt]] library.</span></span> <span data-ttu-id="10a51-113">hello API-k adott számítási csomóponton alkalmazáscsomagok telepítésének részét képezik hello [Batch .NET] [ api_net] könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="10a51-113">hello APIs for installing application packages on a compute node are part of hello [Batch .NET][api_net] library.</span></span>  
>
> <span data-ttu-id="10a51-114">itt leírt hello alkalmazás csomagok szolgáltatás felülír hello Batch-alkalmazások szolgáltatás hello szolgáltatás korábbi verzióiban érhető el.</span><span class="sxs-lookup"><span data-stu-id="10a51-114">hello application packages feature described here supersedes hello Batch Apps feature available in previous versions of hello service.</span></span>
> 
> 

## <a name="application-package-requirements"></a><span data-ttu-id="10a51-115">Csomag alkalmazáskövetelmények</span><span class="sxs-lookup"><span data-stu-id="10a51-115">Application package requirements</span></span>
<span data-ttu-id="10a51-116">toouse, meg kell túl[egy Azure Storage-fiók csatolása](#link-a-storage-account) tooyour Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="10a51-116">toouse application packages, you need too[link an Azure Storage account](#link-a-storage-account) tooyour Batch account.</span></span>

<span data-ttu-id="10a51-117">Ez a szolgáltatás bemutatott [Batch REST API] [ api_rest] 2015-12-01.2.2 verziója, a megfelelő hello [Batch .NET] [ api_net] könyvtárverzió 3.1.0.</span><span class="sxs-lookup"><span data-stu-id="10a51-117">This feature was introduced in [Batch REST API][api_rest] version 2015-12-01.2.2 and hello corresponding [Batch .NET][api_net] library version 3.1.0.</span></span> <span data-ttu-id="10a51-118">Javasoljuk, hogy mindig használjon hello legújabb API-verzió az kötegelt használatakor.</span><span class="sxs-lookup"><span data-stu-id="10a51-118">We recommend that you always use hello latest API version when working with Batch.</span></span>

> [!NOTE]
> <span data-ttu-id="10a51-119">Az alkalmazáscsomagok az összes 2017. július 5. után létrehozott Batch-készleten támogatottak.</span><span class="sxs-lookup"><span data-stu-id="10a51-119">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="10a51-120">2016. március 10. és 5. július 2017 között létrehozott, csak akkor, ha hello készlet lett létrehozva egy felhőalapú szolgáltatás konfigurációja kötegelt készletek azok támogatottak.</span><span class="sxs-lookup"><span data-stu-id="10a51-120">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if hello pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="10a51-121">Kötegelt készletek létrehozott előzetes too10 2016. március nem támogatják az alkalmazáscsomagok.</span><span class="sxs-lookup"><span data-stu-id="10a51-121">Batch pools created prior too10 March 2016 do not support application packages.</span></span>
>
>

## <a name="about-applications-and-application-packages"></a><span data-ttu-id="10a51-122">Alkalmazások és csomagok</span><span class="sxs-lookup"><span data-stu-id="10a51-122">About applications and application packages</span></span>
<span data-ttu-id="10a51-123">Azure Batch belül egy *alkalmazás* tooa készlete, amelyek a készlet számítási csomópontjai automatikusan letöltött toohello lehetnek rendszerverzióval ellátott bináris hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="10a51-123">Within Azure Batch, an *application* refers tooa set of versioned binaries that can be automatically downloaded toohello compute nodes in your pool.</span></span> <span data-ttu-id="10a51-124">Egy *alkalmazáscsomag* tooa hivatkozik *meghatározott* e bináris fájljait és jelöli egy adott *verzió* hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="10a51-124">An *application package* refers tooa *specific set* of those binaries and represents a given *version* of hello application.</span></span>

<span data-ttu-id="10a51-125">![Magas szintű diagramját, alkalmazások és csomagok][1]</span><span class="sxs-lookup"><span data-stu-id="10a51-125">![High-level diagram of applications and application packages][1]</span></span>

### <a name="applications"></a><span data-ttu-id="10a51-126">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="10a51-126">Applications</span></span>
<span data-ttu-id="10a51-127">Egy alkalmazás kötegben tartalmaz egy vagy több alkalmazás csomagokat, és adja meg a konfigurációs beállítások hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="10a51-127">An application in Batch contains one or more application packages and specifies configuration options for hello application.</span></span> <span data-ttu-id="10a51-128">Például egy alkalmazást adhat meg hello alapértelmezett alkalmazás csomag verziója tooinstall számítási csomópontokat, és hogy a csomagokat is frissíthető és nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="10a51-128">For example, an application can specify hello default application package version tooinstall on compute nodes and whether its packages can be updated or deleted.</span></span>

### <a name="application-packages"></a><span data-ttu-id="10a51-129">Alkalmazáscsomagok</span><span class="sxs-lookup"><span data-stu-id="10a51-129">Application packages</span></span>
<span data-ttu-id="10a51-130">Alkalmazáscsomag hello bináris alkalmazásfájlokat tartalmazó .zip fájl és a fájlokat, amelyek szükségesek a feladatok toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="10a51-130">An application package is a .zip file that contains hello application binaries and supporting files that are required for your tasks toorun hello application.</span></span> <span data-ttu-id="10a51-131">Minden alkalmazáscsomag hello alkalmazás adott verzióját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="10a51-131">Each application package represents a specific version of hello application.</span></span>

<span data-ttu-id="10a51-132">Alkalmazáscsomagok hello készlet és a feladat szintjén is megadhat.</span><span class="sxs-lookup"><span data-stu-id="10a51-132">You can specify application packages at hello pool and task levels.</span></span> <span data-ttu-id="10a51-133">Egy készletet vagy feladat létrehozásakor megadhatja egy vagy több ezeket a csomagokat, és (opcionálisan) verzióval.</span><span class="sxs-lookup"><span data-stu-id="10a51-133">You can specify one or more of these packages and (optionally) a version when you create a pool or task.</span></span>

* <span data-ttu-id="10a51-134">**Tárolókészlet alkalmazáscsomagok** túl telepített*minden* csomópont hello készletben.</span><span class="sxs-lookup"><span data-stu-id="10a51-134">**Pool application packages** are deployed too*every* node in hello pool.</span></span> <span data-ttu-id="10a51-135">Alkalmazások vannak telepítve, amikor a csomópont csatlakozik egy készletet, és amikor újraindították vagy a lemezképet.</span><span class="sxs-lookup"><span data-stu-id="10a51-135">Applications are deployed when a node joins a pool, and when it is rebooted or reimaged.</span></span>
  
    <span data-ttu-id="10a51-136">Készlet alkalmazáscsomagok megfelelőek, amikor egy alkalmazáskészlet összes csomópontjának hajtható végre egy feladat tevékenységeit.</span><span class="sxs-lookup"><span data-stu-id="10a51-136">Pool application packages are appropriate when all nodes in a pool execute a job's tasks.</span></span> <span data-ttu-id="10a51-137">Megadhat egy vagy több alkalmazáscsomagok készletet hoz létre, és adja hozzá, vagy egy meglévő készlet-csomagok frissítése.</span><span class="sxs-lookup"><span data-stu-id="10a51-137">You can specify one or more application packages when you create a pool, and you can add or update an existing pool's packages.</span></span> <span data-ttu-id="10a51-138">Ha egy meglévő készlet alkalmazáscsomagok frissítéséhez újra kell indítani a csomópontok tooinstall hello új csomag.</span><span class="sxs-lookup"><span data-stu-id="10a51-138">If you update an existing pool's application packages, you must restart its nodes tooinstall hello new package.</span></span>
* <span data-ttu-id="10a51-139">**Tevékenység-alkalmazáscsomagok** telepített csak tooa számítási csomópont ütemezett toorun egy feladatot, csak a hello feladat parancssor futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="10a51-139">**Task application packages** are deployed only tooa compute node scheduled toorun a task, just before running hello task's command line.</span></span> <span data-ttu-id="10a51-140">Ha meg van adva hello alkalmazáscsomag és verzió már hello csomóponton, nem újra telepíteni és hello meglévő csomag használata.</span><span class="sxs-lookup"><span data-stu-id="10a51-140">If hello specified application package and version is already on hello node, it is not redeployed and hello existing package is used.</span></span>
  
    <span data-ttu-id="10a51-141">A feladat alkalmazáscsomagok olyan hasznos megosztott készlet környezetekben, ahol különböző feladat futtatása egy készletet, és hello készlet nem törlődik, amikor a feladat befejezését.</span><span class="sxs-lookup"><span data-stu-id="10a51-141">Task application packages are useful in shared-pool environments, where different jobs are run on one pool, and hello pool is not deleted when a job is completed.</span></span> <span data-ttu-id="10a51-142">Ha a feladat csomópontok eltérő kevesebb feladatok hello készletben, feladat alkalmazáscsomagok minimálisra adatátvitel mivel az alkalmazás telepített csak toohello a csomópontokra, amelyeket a feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="10a51-142">If your job has fewer tasks than nodes in hello pool, task application packages can minimize data transfer since your application is deployed only toohello nodes that run tasks.</span></span>
  
    <span data-ttu-id="10a51-143">Más tevékenység alkalmazáscsomagok is előnyeit kihasználó forgatókönyvek, amelyek a nagyméretű alkalmazások futnak feladatok, de csak néhány feladatot.</span><span class="sxs-lookup"><span data-stu-id="10a51-143">Other scenarios that can benefit from task application packages are jobs that run a large application, but for only a few tasks.</span></span> <span data-ttu-id="10a51-144">Például egy előre feldolgozásra szakaszban, vagy egy merge feladatra, ahol hello előzetes feldolgozás vagy egyesítési alkalmazás nehéz, előfordulhat, hogy előnyt az alkalmazáscsomagok feladat.</span><span class="sxs-lookup"><span data-stu-id="10a51-144">For example, a pre-processing stage or a merge task, where hello pre-processing or merge application is heavyweight, may benefit from using task application packages.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="10a51-145">Nincsenek korlátozások hello számára az alkalmazások és az alkalmazáscsomagok belül Batch-fiók és a hello maximális csomag mérete.</span><span class="sxs-lookup"><span data-stu-id="10a51-145">There are restrictions on hello number of applications and application packages within a Batch account and on hello maximum application package size.</span></span> <span data-ttu-id="10a51-146">Lásd: [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md) kapcsolatos részletekért.</span><span class="sxs-lookup"><span data-stu-id="10a51-146">See [Quotas and limits for hello Azure Batch service](batch-quota-limit.md) for details about these limits.</span></span>
> 
> 

### <a name="benefits-of-application-packages"></a><span data-ttu-id="10a51-147">Az alkalmazáscsomagok előnyei</span><span class="sxs-lookup"><span data-stu-id="10a51-147">Benefits of application packages</span></span>
<span data-ttu-id="10a51-148">Alkalmazáscsomagok egyszerűbbé teheti az hello kód a kötegelt megoldás és alacsonyabb hello általános szükséges toomanage hello alkalmazások, amelyek a feladatok futnak.</span><span class="sxs-lookup"><span data-stu-id="10a51-148">Application packages can simplify hello code in your Batch solution and lower hello overhead required toomanage hello applications that your tasks run.</span></span>

<span data-ttu-id="10a51-149">Az alkalmazáscsomagok a készlet kezdő tevékenység nem rendelkezik toospecify egyéni erőforrás fájlok tooinstall listája túl hosszú hello csomópontján.</span><span class="sxs-lookup"><span data-stu-id="10a51-149">With application packages, your pool's start task doesn't have toospecify a long list of individual resource files tooinstall on hello nodes.</span></span> <span data-ttu-id="10a51-150">Az alkalmazásfájlokat az Azure Storage vagy a csomóponton több verziójának kezelése toomanually nincs.</span><span class="sxs-lookup"><span data-stu-id="10a51-150">You don't have toomanually manage multiple versions of your application files in Azure Storage, or on your nodes.</span></span> <span data-ttu-id="10a51-151">És nem kell talál az tooworry [SAS URL-címek](../storage/common/storage-dotnet-shared-access-signature-part-1.md) tooprovide toohello található fájlokat a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="10a51-151">And, you don't need tooworry about generating [SAS URLs](../storage/common/storage-dotnet-shared-access-signature-part-1.md) tooprovide access toohello files in your Storage account.</span></span> <span data-ttu-id="10a51-152">A Batch-működik az Azure Storage toostore alkalmazáscsomagok hello háttérben, és azok toocompute csomópontok.</span><span class="sxs-lookup"><span data-stu-id="10a51-152">Batch works in hello background with Azure Storage toostore application packages and deploy them toocompute nodes.</span></span>

> [!NOTE] 
> <span data-ttu-id="10a51-153">hello teljes mérete a kezdő tevékenységre kell kisebb vagy egyenlő, mint too32768 karaktert, köztük Erőforrásfájlok és környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="10a51-153">hello total size of a start task must be less than or equal too32768 characters, including resource files and environment variables.</span></span> <span data-ttu-id="10a51-154">A kezdő tevékenység meghaladja ezt a korlátot, majd az alkalmazás-csomagok használata esetén egy másik lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="10a51-154">If your start task exceeds this limit, then using application packages is another option.</span></span> <span data-ttu-id="10a51-155">Is létrehozhat az erőforrás-fájlokat tartalmazó tömörített archívum létrehozása, töltse fel a blob tooAzure Storage, és ezután bontsa ki a parancssorból hello a kezdő tevékenység.</span><span class="sxs-lookup"><span data-stu-id="10a51-155">You can also create a zipped archive containing your resource files, upload it as a blob tooAzure Storage, and then unzip it from hello command line of your start task.</span></span> 
>
>

## <a name="upload-and-manage-applications"></a><span data-ttu-id="10a51-156">Alkalmazások kezelését és feltöltését</span><span class="sxs-lookup"><span data-stu-id="10a51-156">Upload and manage applications</span></span>
<span data-ttu-id="10a51-157">Használhatja a hello [Azure-portálon] [ portal] vagy hello [Batch Management .NET kódtárral](batch-management-dotnet.md) könyvtár toomanage hello alkalmazáscsomagok a Batch-fiók.</span><span class="sxs-lookup"><span data-stu-id="10a51-157">You can use hello [Azure portal][portal] or hello [Batch Management .NET](batch-management-dotnet.md) library toomanage hello application packages in your Batch account.</span></span> <span data-ttu-id="10a51-158">A hello mellett néhány szakaszok, először megmutatjuk, hogyan toolink egy tárfiókot, majd a további alkalmazásokat ismertetik, és a csomagok és kezelnie azokat a hello portálon.</span><span class="sxs-lookup"><span data-stu-id="10a51-158">In hello next few sections, we first show how toolink a Storage account, then discuss adding applications and packages and managing them with hello portal.</span></span>

### <a name="link-a-storage-account"></a><span data-ttu-id="10a51-159">A tárfiók csatolása</span><span class="sxs-lookup"><span data-stu-id="10a51-159">Link a Storage account</span></span>
<span data-ttu-id="10a51-160">toouse alkalmazáscsomagok, először rendelnie egy Azure Storage-fiók tooyour Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="10a51-160">toouse application packages, you must first link an Azure Storage account tooyour Batch account.</span></span> <span data-ttu-id="10a51-161">Ha még nincs konfigurálva egy tárfiókot, hello Azure-portálon jeleníti meg a figyelmeztető hello először hello kattint **alkalmazások** hello csempére **a Batch-fiók** panelen.</span><span class="sxs-lookup"><span data-stu-id="10a51-161">If you have not yet configured a Storage account, hello Azure portal displays a warning hello first time you click hello **Applications** tile in hello **Batch account** blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="10a51-162">Jelenleg a Batch-támogatja *csak* hello **általános célú** tárfióktípus 5, lépésben leírt [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account), a [kapcsolatos Azure Storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="10a51-162">Batch currently supports *only* hello **General-purpose** storage account type as described in step 5, [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account), in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="10a51-163">Egy Azure Storage-fiók tooyour Batch-fiók csatolásához kapcsolja *csak* egy **általános célú** storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="10a51-163">When you link an Azure Storage account tooyour Batch account, link *only* a **General-purpose** storage account.</span></span>
> 
> 

<span data-ttu-id="10a51-164">!["Nincs beállítva tárfiók" figyelmeztetés Azure-portálon][9]</span><span class="sxs-lookup"><span data-stu-id="10a51-164">!['No storage account configured' warning in Azure portal][9]</span></span>

<span data-ttu-id="10a51-165">hello Batch szolgáltatás által használt hello társított tárolási fiók toostore az alkalmazáscsomagok.</span><span class="sxs-lookup"><span data-stu-id="10a51-165">hello Batch service uses hello associated Storage account toostore your application packages.</span></span> <span data-ttu-id="10a51-166">Után a hozzá csatolt hello két fiókot, akkor kötegelt automatikusan kapcsolódó hello tárolási fiók tooyour számítási csomópontok tárolt hello csomagok központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="10a51-166">After you've linked hello two accounts, Batch can automatically deploy hello packages stored in hello linked Storage account tooyour compute nodes.</span></span> <span data-ttu-id="10a51-167">toolink a tárolási fiók tooyour Batch-fiókhoz, kattintson a **tárolási Fiókbeállítások** a hello **figyelmeztetés** panelt, és kattintson **Tárfiók** hello a**Tárfiók** panelen.</span><span class="sxs-lookup"><span data-stu-id="10a51-167">toolink a Storage account tooyour Batch account, click **Storage account settings** on hello **Warning** blade, and then click **Storage Account** on hello **Storage Account** blade.</span></span>

<span data-ttu-id="10a51-168">![Storage-fiók panelen válassza az Azure-portálon][10]</span><span class="sxs-lookup"><span data-stu-id="10a51-168">![Choose storage account blade in Azure portal][10]</span></span>

<span data-ttu-id="10a51-169">Azt javasoljuk, hogy hozzon létre egy tárfiókot *kifejezetten* a Batch-fiókhoz való használatra, és jelölje ki itt.</span><span class="sxs-lookup"><span data-stu-id="10a51-169">We recommend that you create a Storage account *specifically* for use with your Batch account, and select it here.</span></span> <span data-ttu-id="10a51-170">Részletes információt toocreate egy tárfiókot, lásd: "Create a Storage-fiók" az [Azure Storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="10a51-170">For details about how toocreate a storage account, see "Create a Storage account" in [About Azure Storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="10a51-171">Miután létrehozott egy tárfiókot, majd társíthatja azt tooyour Batch-fiók hello segítségével **Tárfiók** panelen.</span><span class="sxs-lookup"><span data-stu-id="10a51-171">After you've created a Storage account, you can then link it tooyour Batch account by using hello **Storage Account** blade.</span></span>

> [!WARNING]
> <span data-ttu-id="10a51-172">hello Batch szolgáltatás által használt Azure Storage toostore az alkalmazáscsomagok blokkblobként.</span><span class="sxs-lookup"><span data-stu-id="10a51-172">hello Batch service uses Azure Storage toostore your application packages as block blobs.</span></span> <span data-ttu-id="10a51-173">Ön [szokásos módon felszámított] [ storage_pricing] hello blokk blob adatok.</span><span class="sxs-lookup"><span data-stu-id="10a51-173">You are [charged as normal][storage_pricing] for hello block blob data.</span></span> <span data-ttu-id="10a51-174">Lehet, hogy tooconsider hello méretét és az alkalmazáscsomagok számát, és bizonyos időközönként eltávolítja az elavult csomagok toominimize költségeket.</span><span class="sxs-lookup"><span data-stu-id="10a51-174">Be sure tooconsider hello size and number of your application packages, and periodically remove deprecated packages toominimize costs.</span></span>
> 
> 

### <a name="view-current-applications"></a><span data-ttu-id="10a51-175">Aktuális alkalmazások megtekintése</span><span class="sxs-lookup"><span data-stu-id="10a51-175">View current applications</span></span>
<span data-ttu-id="10a51-176">a Batch-fiók tooview hello alkalmazások kattintson hello **alkalmazások** menüpont hello bal oldali menüben hello megtekintésekor **a Batch-fiók** panelen.</span><span class="sxs-lookup"><span data-stu-id="10a51-176">tooview hello applications in your Batch account, click hello **Applications** menu item in hello left menu while viewing hello **Batch account** blade.</span></span>

<span data-ttu-id="10a51-177">![Alkalmazások csempe][2]</span><span class="sxs-lookup"><span data-stu-id="10a51-177">![Applications tile][2]</span></span>

<span data-ttu-id="10a51-178">Ezzel a beállítással menü megnyitása hello **alkalmazások** panel:</span><span class="sxs-lookup"><span data-stu-id="10a51-178">Selecting this menu option opens hello **Applications** blade:</span></span>

<span data-ttu-id="10a51-179">![Alkalmazások listája][3]</span><span class="sxs-lookup"><span data-stu-id="10a51-179">![List applications][3]</span></span>

<span data-ttu-id="10a51-180">Hello **alkalmazások** panelt jeleníti meg a fiók minden alkalmazás hello és hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="10a51-180">hello **Applications** blade displays hello ID of each application in your account and hello following properties:</span></span>

* <span data-ttu-id="10a51-181">**Csomagok**: hello jelen alkalmazáshoz rendelt verzióinak száma.</span><span class="sxs-lookup"><span data-stu-id="10a51-181">**Packages**: hello number of versions associated with this application.</span></span>
* <span data-ttu-id="10a51-182">**Alapértelmezett verzió**: hello alkalmazás verziója a Ha hello alkalmazás készlet megadásakor nem jelzi azt egy verzió telepítve.</span><span class="sxs-lookup"><span data-stu-id="10a51-182">**Default version**: hello application version installed if you do not indicate a version when you specify hello application for a pool.</span></span> <span data-ttu-id="10a51-183">Ez a beállítás nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="10a51-183">This setting is optional.</span></span>
* <span data-ttu-id="10a51-184">**Frissítések engedélyezése**: hello érték, amely meghatározza, hogy frissíti a csomagot, törléseket és kiegészítéseit engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="10a51-184">**Allow updates**: hello value that specifies whether package updates, deletions, and additions are allowed.</span></span> <span data-ttu-id="10a51-185">Ha a beállított érték túl**nem**, csomag frissítések és törlések hello alkalmazás le vannak tiltva.</span><span class="sxs-lookup"><span data-stu-id="10a51-185">If this is set too**No**, package updates and deletions are disabled for hello application.</span></span> <span data-ttu-id="10a51-186">Csak új alkalmazáscsomag-verziók is hozzáadhatók.</span><span class="sxs-lookup"><span data-stu-id="10a51-186">Only new application package versions can be added.</span></span> <span data-ttu-id="10a51-187">hello alapértelmezett érték a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="10a51-187">hello default is **Yes**.</span></span>

### <a name="view-application-details"></a><span data-ttu-id="10a51-188">Az alkalmazás részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="10a51-188">View application details</span></span>
<span data-ttu-id="10a51-189">tooopen hello panel, amely tartalmazza az alkalmazás, jelölje be hello alkalmazás hello részletek hello **alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="10a51-189">tooopen hello blade that includes hello details for an application, select hello application in hello **Applications** blade.</span></span>

<span data-ttu-id="10a51-190">![Az alkalmazás részletei][4]</span><span class="sxs-lookup"><span data-stu-id="10a51-190">![Application details][4]</span></span>

<span data-ttu-id="10a51-191">Hello alkalmazás részleteit megjelenítő panelen konfigurálhatja az alkalmazás beállításainak a következő hello.</span><span class="sxs-lookup"><span data-stu-id="10a51-191">In hello application details blade, you can configure hello following settings for your application.</span></span>

* <span data-ttu-id="10a51-192">**Frissítések engedélyezése**: Adja meg, hogy az alkalmazáscsomagok is frissíthető és nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="10a51-192">**Allow updates**: Specify whether its application packages can be updated or deleted.</span></span> <span data-ttu-id="10a51-193">Tekintse meg a "Frissíteni vagy törölni egy alkalmazáscsomagot" a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="10a51-193">See "Update or Delete an application package" later in this article.</span></span>
* <span data-ttu-id="10a51-194">**Alapértelmezett verzió**: Adjon meg egy alapértelmezett alkalmazás csomag toodeploy toocompute csomópontok.</span><span class="sxs-lookup"><span data-stu-id="10a51-194">**Default version**: Specify a default application package toodeploy toocompute nodes.</span></span>
* <span data-ttu-id="10a51-195">**Megjelenített név**: Adjon meg egy rövid nevet, amely a kötegelt megoldás használható tartalmáról hello alkalmazás adatait, például hello felhasználói felületén keresztül kötegelt tooyour ügyfelek biztosító szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="10a51-195">**Display name**: Specify a friendly name that your Batch solution can use when it displays information about hello application, for example, in hello UI of a service that you provide tooyour customers through Batch.</span></span>

### <a name="add-a-new-application"></a><span data-ttu-id="10a51-196">Új alkalmazás felvétele</span><span class="sxs-lookup"><span data-stu-id="10a51-196">Add a new application</span></span>
<span data-ttu-id="10a51-197">toocreate egy új alkalmazást, vegye fel egy alkalmazáscsomagot, és adjon meg egy új, egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="10a51-197">toocreate a new application, add an application package and specify a new, unique application ID.</span></span> <span data-ttu-id="10a51-198">hello első alkalmazáscsomag felvételekor hello új application ID hello új alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="10a51-198">hello first application package that you add with hello new application ID also creates hello new application.</span></span>

<span data-ttu-id="10a51-199">Kattintson a **Hozzáadás** a hello **alkalmazások** panel tooopen hello **új alkalmazás** panelen.</span><span class="sxs-lookup"><span data-stu-id="10a51-199">Click **Add** on hello **Applications** blade tooopen hello **New application** blade.</span></span>

<span data-ttu-id="10a51-200">![Új alkalmazás panel az Azure-portálon][5]</span><span class="sxs-lookup"><span data-stu-id="10a51-200">![New application blade in Azure portal][5]</span></span>

<span data-ttu-id="10a51-201">Hello **új alkalmazás** panel biztosít hello következő mezői toospecify hello beállításait az új alkalmazás- és alkalmazáscsomagot.</span><span class="sxs-lookup"><span data-stu-id="10a51-201">hello **New application** blade provides hello following fields toospecify hello settings of your new application and application package.</span></span>

<span data-ttu-id="10a51-202">**Alkalmazásazonosító**</span><span class="sxs-lookup"><span data-stu-id="10a51-202">**Application id**</span></span>

<span data-ttu-id="10a51-203">Ez a mező az új alkalmazást, amely tulajdonos toohello szabványos Azure Kötegazonosító ellenőrzési szabályok hello Azonosítóját adja meg.</span><span class="sxs-lookup"><span data-stu-id="10a51-203">This field specifies hello ID of your new application, which is subject toohello standard Azure Batch ID validation rules.</span></span> <span data-ttu-id="10a51-204">biztosítani az Alkalmazásazonosító hello szabályok a következők:</span><span class="sxs-lookup"><span data-stu-id="10a51-204">hello rules for providing an application ID are as follows:</span></span>

* <span data-ttu-id="10a51-205">Windows-csomópont a hello azonosító az alfanumerikus karaktereket, kötőjeleket és aláhúzásjeleket tartalmazhat tetszőleges kombinációját tartalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="10a51-205">On Windows nodes, hello ID can contain any combination of alphanumeric characters, hyphens, and underscores.</span></span> <span data-ttu-id="10a51-206">Linux-csomópont csak alfanumerikus karaktereket és aláhúzás karaktereket tartalmazhatnak engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="10a51-206">On Linux nodes, only alphanumeric characters and underscores are permitted.</span></span>
* <span data-ttu-id="10a51-207">Nem lehet hosszabb 64 karakternél.</span><span class="sxs-lookup"><span data-stu-id="10a51-207">Cannot contain more than 64 characters.</span></span>
* <span data-ttu-id="10a51-208">A Batch-fiókhoz hello belül egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="10a51-208">Must be unique within hello Batch account.</span></span>
* <span data-ttu-id="10a51-209">Egy nagybetűket és a nagybetűk között.</span><span class="sxs-lookup"><span data-stu-id="10a51-209">Is case-preserving and case-insensitive.</span></span>

<span data-ttu-id="10a51-210">**Verzió**</span><span class="sxs-lookup"><span data-stu-id="10a51-210">**Version**</span></span>

<span data-ttu-id="10a51-211">Ez a mező határozza meg a feltölteni kívánt alkalmazáscsomag hello hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="10a51-211">This field specifies hello version of hello application package you are uploading.</span></span> <span data-ttu-id="10a51-212">Verzió értékek a következők: tulajdonos toohello ellenőrzési szabályok a következő:</span><span class="sxs-lookup"><span data-stu-id="10a51-212">Version strings are subject toohello following validation rules:</span></span>

* <span data-ttu-id="10a51-213">Windows csomópont a hello verzió-karakterláncnak a alfanumerikus karaktereket, kötőjeleket, aláhúzásjeleket és időszakok tetszőleges kombinációját tartalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="10a51-213">On Windows nodes, hello version string can contain any combination of alphanumeric characters, hyphens, underscores, and periods.</span></span> <span data-ttu-id="10a51-214">Linux csomópont hello verzió-karakterlánca csak alfanumerikus karaktereket és aláhúzásjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="10a51-214">On Linux nodes, hello version string can contain only alphanumeric characters and underscores.</span></span>
* <span data-ttu-id="10a51-215">Nem lehet hosszabb 64 karakternél.</span><span class="sxs-lookup"><span data-stu-id="10a51-215">Cannot contain more than 64 characters.</span></span>
* <span data-ttu-id="10a51-216">Hello alkalmazáson belül egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="10a51-216">Must be unique within hello application.</span></span>
* <span data-ttu-id="10a51-217">A rendszer megőrzi és a nagybetűk között.</span><span class="sxs-lookup"><span data-stu-id="10a51-217">Are case-preserving and case-insensitive.</span></span>

<span data-ttu-id="10a51-218">**Alkalmazáscsomag**</span><span class="sxs-lookup"><span data-stu-id="10a51-218">**Application package**</span></span>

<span data-ttu-id="10a51-219">Ez a mező hello bináris alkalmazásfájlokat tartalmazó hello .zip fájl és a fájlokat, amelyek a szükséges tooexecute hello alkalmazás adja meg.</span><span class="sxs-lookup"><span data-stu-id="10a51-219">This field specifies hello .zip file that contains hello application binaries and supporting files that are required tooexecute hello application.</span></span> <span data-ttu-id="10a51-220">Kattintson a hello **válasszon ki egy fájlt** mező vagy hello mappa ikon toobrowse tooand, válassza ki az alkalmazás fájlokat tartalmazó .zip-fájlt.</span><span class="sxs-lookup"><span data-stu-id="10a51-220">Click hello **Select a file** box or hello folder icon toobrowse tooand select a .zip file that contains your application's files.</span></span>

<span data-ttu-id="10a51-221">Miután kijelölt egy fájlt, kattintson **OK** toobegin hello feltöltés tooAzure tároló.</span><span class="sxs-lookup"><span data-stu-id="10a51-221">After you've selected a file, click **OK** toobegin hello upload tooAzure Storage.</span></span> <span data-ttu-id="10a51-222">Hello feltöltési művelet befejeződése után hello portál egy értesítést jelenít meg, és hello panel bezárása után.</span><span class="sxs-lookup"><span data-stu-id="10a51-222">When hello upload operation is complete, hello portal displays a notification and closes hello blade.</span></span> <span data-ttu-id="10a51-223">Attól függően, hogy-e a hálózati kapcsolat sebességétől feltöltését és hello hello fájl hello méretét Ez a művelet eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="10a51-223">Depending on hello size of hello file that you are uploading and hello speed of your network connection, this operation may take some time.</span></span>

> [!WARNING]
> <span data-ttu-id="10a51-224">Ne zárja be az hello **új alkalmazás** panel hello feltöltési művelet befejezése előtt.</span><span class="sxs-lookup"><span data-stu-id="10a51-224">Do not close hello **New application** blade before hello upload operation is complete.</span></span> <span data-ttu-id="10a51-225">Ezzel megszűnik a hello feltöltési folyamat.</span><span class="sxs-lookup"><span data-stu-id="10a51-225">Doing so will stop hello upload process.</span></span>
> 
> 

### <a name="add-a-new-application-package"></a><span data-ttu-id="10a51-226">Új alkalmazás csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="10a51-226">Add a new application package</span></span>
<span data-ttu-id="10a51-227">tooadd alkalmazás új csomagverziójának egy meglévő alkalmazáshoz, válasszon ki egy alkalmazást hello **alkalmazások** panelen kattintson **csomagok**, majd kattintson a **Hozzáadás** tooopen Hello **Hozzáadás csomag** panelen.</span><span class="sxs-lookup"><span data-stu-id="10a51-227">tooadd a new application package version for an existing application, select an application in hello **Applications** blade, click **Packages**, then click **Add** tooopen hello **Add package** blade.</span></span>

<span data-ttu-id="10a51-228">![Adja hozzá az alkalmazás csomag panel Azure-portálon][8]</span><span class="sxs-lookup"><span data-stu-id="10a51-228">![Add application package blade in Azure portal][8]</span></span>

<span data-ttu-id="10a51-229">Ahogy látja, hello mezők egyeznek hello **új alkalmazás** panelen, de hello **alkalmazásazonosító** mezőben le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="10a51-229">As you can see, hello fields match those of hello **New application** blade, but hello **Application id** box is disabled.</span></span> <span data-ttu-id="10a51-230">Új alkalmazás hello hasonló módon adja meg a hello **verzió** az új csomag tallózással tooyour **alkalmazáscsomag** .zip fájlt, majd kattintson az **OK** tooupload hello a csomag.</span><span class="sxs-lookup"><span data-stu-id="10a51-230">As you did for hello new application, specify hello **Version** for your new package, browse tooyour **Application package** .zip file, then click **OK** tooupload hello package.</span></span>

### <a name="update-or-delete-an-application-package"></a><span data-ttu-id="10a51-231">Frissítés vagy törlés alkalmazáscsomag</span><span class="sxs-lookup"><span data-stu-id="10a51-231">Update or delete an application package</span></span>
<span data-ttu-id="10a51-232">tooupdate vagy törölje a meglévő alkalmazáscsomag, nyissa meg hello részleteit megjelenítő panelen hello alkalmazáshoz, kattintson **csomagok** tooopen hello **csomagok** panelen kattintson a hello **három pont**hello alkalmazáscsomagot, hogy szeretné, hogy toomodify, és válassza ki a megjeleníteni kívánt tooperform hello művelet hello sorában.</span><span class="sxs-lookup"><span data-stu-id="10a51-232">tooupdate or delete an existing application package, open hello details blade for hello application, click **Packages** tooopen hello **Packages** blade, click hello **ellipsis** in hello row of hello application package that you want toomodify, and select hello action that you want tooperform.</span></span>

<span data-ttu-id="10a51-233">![Frissítés vagy törlés csomag Azure-portálon][7]</span><span class="sxs-lookup"><span data-stu-id="10a51-233">![Update or delete package in Azure portal][7]</span></span>

<span data-ttu-id="10a51-234">**Update**</span><span class="sxs-lookup"><span data-stu-id="10a51-234">**Update**</span></span>

<span data-ttu-id="10a51-235">Amikor rákattint **frissítés**, hello *csomag* panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="10a51-235">When you click **Update**, hello *Update package* blade is displayed.</span></span> <span data-ttu-id="10a51-236">Ezen a panelen hasonló toohello *új alkalmazáscsomag* panelen, azonban csak hello csomag kiválasztási mezőben engedélyezve van, lehetővé teszi egy új ZIP-fájl tooupload toospecify.</span><span class="sxs-lookup"><span data-stu-id="10a51-236">This blade is similar toohello *New application package* blade, however only hello package selection field is enabled, allowing you toospecify a new ZIP file tooupload.</span></span>

<span data-ttu-id="10a51-237">![Frissítési csomag panel az Azure-portálon][11]</span><span class="sxs-lookup"><span data-stu-id="10a51-237">![Update package blade in Azure portal][11]</span></span>

<span data-ttu-id="10a51-238">**Törlés**</span><span class="sxs-lookup"><span data-stu-id="10a51-238">**Delete**</span></span>

<span data-ttu-id="10a51-239">Amikor rákattint **törlése**, a rendszer felkéri tooconfirm hello törlésének hello Csomagverzió és kötegelt hello csomag törli az Azure Storage-ból.</span><span class="sxs-lookup"><span data-stu-id="10a51-239">When you click **Delete**, you are asked tooconfirm hello deletion of hello package version, and Batch deletes hello package from Azure Storage.</span></span> <span data-ttu-id="10a51-240">Ha törli egy alkalmazás alapértelmezett verzióját hello, hello **alapértelmezett verzió** beállítás hello alkalmazás eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="10a51-240">If you delete hello default version of an application, hello **Default version** setting is removed for hello application.</span></span>

<span data-ttu-id="10a51-241">![Alkalmazás törlése][12]</span><span class="sxs-lookup"><span data-stu-id="10a51-241">![Delete application ][12]</span></span>

## <a name="install-applications-on-compute-nodes"></a><span data-ttu-id="10a51-242">Alkalmazások telepítése a számítási csomópontok</span><span class="sxs-lookup"><span data-stu-id="10a51-242">Install applications on compute nodes</span></span>
<span data-ttu-id="10a51-243">Most, hogy megismerte a hogyan toomanage alkalmazás Azure-portálon hello csomagok, tárgyaljuk is hogyan toodeploy őket toocompute csomópontok és kötegelt feladatok futtathatók.</span><span class="sxs-lookup"><span data-stu-id="10a51-243">Now that you've learned how toomanage application packages with hello Azure portal, we can discuss how toodeploy them toocompute nodes and run them with Batch tasks.</span></span>

### <a name="install-pool-application-packages"></a><span data-ttu-id="10a51-244">Készlet alkalmazáscsomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="10a51-244">Install pool application packages</span></span>
<span data-ttu-id="10a51-245">egy alkalmazáscsomag összes tooinstall számítási csomópontjain a készletben, adjon meg egy vagy több alkalmazáscsomagot *hivatkozások* hello készlet.</span><span class="sxs-lookup"><span data-stu-id="10a51-245">tooinstall an application package on all compute nodes in a pool, specify one or more application package *references* for hello pool.</span></span> <span data-ttu-id="10a51-246">Ha a csomópont csatlakozik hello alkalmazáskészlet, illetve ha hello csomópont újraindítása után, vagy lemezképet hello csomagok számára egy készlet megadott minden számítási csomópont telepítése.</span><span class="sxs-lookup"><span data-stu-id="10a51-246">hello application packages that you specify for a pool are installed on each compute node when that node joins hello pool, and when hello node is rebooted or reimaged.</span></span>

<span data-ttu-id="10a51-247">A Batch .NET, adjon meg egy vagy több [CloudPool][net_cloudpool].[ ApplicationPackageReferences] [ net_cloudpool_pkgref] amikor létrehoz egy új készletet, vagy egy meglévő készlet.</span><span class="sxs-lookup"><span data-stu-id="10a51-247">In Batch .NET, specify one or more [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref] when you create a new pool, or for an existing pool.</span></span> <span data-ttu-id="10a51-248">Hello [ApplicationPackageReference] [ net_pkgref] osztály megadja egy alkalmazás-Azonosítót és tooinstall a készlet számítási csomópontjain verziót.</span><span class="sxs-lookup"><span data-stu-id="10a51-248">hello [ApplicationPackageReference][net_pkgref] class specifies an application ID and version tooinstall on a pool's compute nodes.</span></span>

```csharp
// Create hello unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify hello application and version tooinstall on hello compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit hello pool so that it's created in hello Batch service. As hello nodes join
// hello pool, hello specified application package is installed on each.
await myCloudPool.CommitAsync();
```

> [!IMPORTANT]
> <span data-ttu-id="10a51-249">Az alkalmazás csomag központi telepítése a bármilyen okból nem sikerül, ha hello Batch szolgáltatás jelek hello csomópont [használhatatlanná][net_nodestate], és nincsenek feladatok vannak ütemezve ezen a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="10a51-249">If an application package deployment fails for any reason, hello Batch service marks hello node [unusable][net_nodestate], and no tasks are scheduled for execution on that node.</span></span> <span data-ttu-id="10a51-250">Ebben az esetben kell **indítsa újra a** hello csomópont tooreinitiate hello package deployment mappában.</span><span class="sxs-lookup"><span data-stu-id="10a51-250">In this case, you should **restart** hello node tooreinitiate hello package deployment.</span></span> <span data-ttu-id="10a51-251">Újraindítás hello csomópont is lehetővé teszi, hogy a feladatütemezés ismét hello csomóponton.</span><span class="sxs-lookup"><span data-stu-id="10a51-251">Restarting hello node also enables task scheduling again on hello node.</span></span>
> 
> 

### <a name="install-task-application-packages"></a><span data-ttu-id="10a51-252">A feladat alkalmazáscsomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="10a51-252">Install task application packages</span></span>
<span data-ttu-id="10a51-253">Hasonló tooa készlet meg alkalmazáscsomagot *hivatkozások* feladathoz.</span><span class="sxs-lookup"><span data-stu-id="10a51-253">Similar tooa pool, you specify application package *references* for a task.</span></span> <span data-ttu-id="10a51-254">Ha a feladat ütemezett toorun egy csomóponton, hello csomag letöltése és kicsomagolása, csak a parancssor hello feladat végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="10a51-254">When a task is scheduled toorun on a node, hello package is downloaded and extracted just before hello task's command line is executed.</span></span> <span data-ttu-id="10a51-255">Ha a megadott csomag és verzió hello csomóponton már van telepítve, hello csomag letöltése nem történik meg, és hello meglévő csomag használata.</span><span class="sxs-lookup"><span data-stu-id="10a51-255">If a specified package and version is already installed on hello node, hello package is not downloaded and hello existing package is used.</span></span>

<span data-ttu-id="10a51-256">a feladat alkalmazáscsomagok tooinstall konfigurálása hello feladat [CloudTask][net_cloudtask].[ ApplicationPackageReferences] [ net_cloudtask_pkgref] tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="10a51-256">tooinstall a task application package, configure hello task's [CloudTask][net_cloudtask].[ApplicationPackageReferences][net_cloudtask_pkgref] property:</span></span>

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

## <a name="execute-hello-installed-applications"></a><span data-ttu-id="10a51-257">Hello telepített alkalmazások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="10a51-257">Execute hello installed applications</span></span>
<span data-ttu-id="10a51-258">hello csomagok egy készletet vagy feladathoz megadott vannak letöltött és kibontott nevű könyvtár belüli hello tooa `AZ_BATCH_ROOT_DIR` hello csomópont.</span><span class="sxs-lookup"><span data-stu-id="10a51-258">hello packages that you've specified for a pool or task are downloaded and extracted tooa named directory within hello `AZ_BATCH_ROOT_DIR` of hello node.</span></span> <span data-ttu-id="10a51-259">Kötegelt is létrehoz egy környezeti változó, amely tartalmazza a hello elérési toohello nevű könyvtár.</span><span class="sxs-lookup"><span data-stu-id="10a51-259">Batch also creates an environment variable that contains hello path toohello named directory.</span></span> <span data-ttu-id="10a51-260">A feladat parancssorokat e környezeti változó használatával való hivatkozáskor hello alkalmazás hello csomóponton.</span><span class="sxs-lookup"><span data-stu-id="10a51-260">Your task command lines use this environment variable when referencing hello application on hello node.</span></span> 

<span data-ttu-id="10a51-261">Windows csomópontján hello változó hello formátuma a következő szerepel:</span><span class="sxs-lookup"><span data-stu-id="10a51-261">On Windows nodes, hello variable is in hello following format:</span></span>

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

<span data-ttu-id="10a51-262">Linux-csomópont hello formátuma némileg eltérő.</span><span class="sxs-lookup"><span data-stu-id="10a51-262">On Linux nodes, hello format is slightly different.</span></span> <span data-ttu-id="10a51-263">A pontok (.), kötőjelet (-) és a kettős kereszttel (#) is egybesimított toounderscores hello környezeti változóban.</span><span class="sxs-lookup"><span data-stu-id="10a51-263">Periods (.), hypens (-) and number signs (#) are flattened toounderscores in hello environment variable.</span></span> <span data-ttu-id="10a51-264">Példa:</span><span class="sxs-lookup"><span data-stu-id="10a51-264">For example:</span></span>

```
Linux:
AZ_BATCH_APP_PACKAGE_APPLICATIONID_version
```

<span data-ttu-id="10a51-265">`APPLICATIONID`és `version` értékek, amelyek megfelelnek a megadott központi telepítés toohello alkalmazás és csomag verziója.</span><span class="sxs-lookup"><span data-stu-id="10a51-265">`APPLICATIONID` and `version` are values that correspond toohello application and package version you've specified for deployment.</span></span> <span data-ttu-id="10a51-266">Például, ha a megadott alkalmazás 2.7-es verzió *keverőgép* telepítendő Windows csomópont, a feladat parancssorokat szeretné használni a környezeti változó tooaccess fájlokat:</span><span class="sxs-lookup"><span data-stu-id="10a51-266">For example, if you specified that version 2.7 of application *blender* should be installed on Windows nodes, your task command lines would use this environment variable tooaccess its files:</span></span>

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

<span data-ttu-id="10a51-267">Linux-csomópont adja meg a hello környezeti változó a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="10a51-267">On Linux nodes, specify hello environment variable in this format:</span></span>

```
Linux:
AZ_BATCH_APP_PACKAGE_BLENDER_2_7
``` 

<span data-ttu-id="10a51-268">Ha feltölt egy alkalmazáscsomagot, megadhat egy alapértelmezett verzió toodeploy tooyour számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="10a51-268">When you upload an application package, you can specify a default version toodeploy tooyour compute nodes.</span></span> <span data-ttu-id="10a51-269">Ha egy alkalmazás alapértelmezett verziót adott meg, hello verzió utótag kihagyhatja, ha hello alkalmazás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="10a51-269">If you have specified a default version for an application, you can omit hello version suffix when you reference hello application.</span></span> <span data-ttu-id="10a51-270">Megadhat hello alapértelmezett Alkalmazásverzió hello Azure-portálon, a hello alkalmazások panelen látható módon [alkalmazások kezelését és feltöltését](#upload-and-manage-applications).</span><span class="sxs-lookup"><span data-stu-id="10a51-270">You can specify hello default application version in hello Azure portal, on hello Applications blade, as shown in [Upload and manage applications](#upload-and-manage-applications).</span></span>

<span data-ttu-id="10a51-271">Például, ha állítja be "2.7" hello alapértelmezett verzió az alkalmazáshoz *keverőgép*, és a feladatok a következő környezeti változó hello hivatkozzon, majd a Windows-csomópontok végrehajtja a 2.7-es verzió:</span><span class="sxs-lookup"><span data-stu-id="10a51-271">For example, if you set "2.7" as hello default version for application *blender*, and your tasks reference hello following environment variable, then your Windows nodes will execute version 2.7:</span></span>

`AZ_BATCH_APP_PACKAGE_BLENDER`

<span data-ttu-id="10a51-272">hello következő kódrészletet látható egy példa a feladat parancssori hello alapértelmezett verziója hello indító *keverőgép* alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="10a51-272">hello following code snippet shows an example task command line that launches hello default version of hello *blender* application:</span></span>

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> <span data-ttu-id="10a51-273">Lásd: [környezeti beállítások feladatok](batch-api-basics.md#environment-settings-for-tasks) a hello [Batch funkcióinak áttekintése](batch-api-basics.md) számítási csomópont környezet beállításaival kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="10a51-273">See [Environment settings for tasks](batch-api-basics.md#environment-settings-for-tasks) in hello [Batch feature overview](batch-api-basics.md) for more information about compute node environment settings.</span></span>
> 
> 

## <a name="update-a-pools-application-packages"></a><span data-ttu-id="10a51-274">Készlet alkalmazáscsomagjainak frissítése</span><span class="sxs-lookup"><span data-stu-id="10a51-274">Update a pool's application packages</span></span>
<span data-ttu-id="10a51-275">Ha egy meglévő készlet már be van állítva egy alkalmazási csomaggal rendelkező, hello készlet új csomagot is megadhat.</span><span class="sxs-lookup"><span data-stu-id="10a51-275">If an existing pool has already been configured with an application package, you can specify a new package for hello pool.</span></span> <span data-ttu-id="10a51-276">Ha megadja az új csomag leírását, a készletbe, a következő apply hello:</span><span class="sxs-lookup"><span data-stu-id="10a51-276">If you specify a new package reference for a pool, hello following apply:</span></span>

* <span data-ttu-id="10a51-277">hello Batch szolgáltatás telepítheti hello újonnan meghatározott csomag, az összes új csomópont, amelyhez csatlakozni hello alkalmazáskészlet, illetve bármely létező csomópontján újraindították vagy lemezképet.</span><span class="sxs-lookup"><span data-stu-id="10a51-277">hello Batch service installs hello newly specified package on all new nodes that join hello pool and on any existing node that is rebooted or reimaged.</span></span>
* <span data-ttu-id="10a51-278">A számítási csomópontokat, amelyek már hello készletben hello csomaghivatkozásokhoz frissítésekor nem telepíti automatikusan hello új alkalmazáscsomagot.</span><span class="sxs-lookup"><span data-stu-id="10a51-278">Compute nodes that are already in hello pool when you update hello package references do not automatically install hello new application package.</span></span> <span data-ttu-id="10a51-279">Ezek a számítási csomópontok újra kell indítani, vagy azon tooreceive hello új csomag.</span><span class="sxs-lookup"><span data-stu-id="10a51-279">These compute nodes must be rebooted or reimaged tooreceive hello new package.</span></span>
* <span data-ttu-id="10a51-280">Új csomag telepítésekor a környezeti változók létrehozott hello hello új alkalmazás csomaghivatkozásokhoz tükrözik.</span><span class="sxs-lookup"><span data-stu-id="10a51-280">When a new package is deployed, hello created environment variables reflect hello new application package references.</span></span>

<span data-ttu-id="10a51-281">Ebben a példában a meglévő készlet hello rendelkezik hello 2.7-es verziójának *keverőgép* egyik konfigurált alkalmazás a [CloudPool][net_cloudpool].[ ApplicationPackageReferences][net_cloudpool_pkgref].</span><span class="sxs-lookup"><span data-stu-id="10a51-281">In this example, hello existing pool has version 2.7 of hello *blender* application configured as one of its [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref].</span></span> <span data-ttu-id="10a51-282">tooupdate hello készlet csomópontok verziójával 2.76b, adjon meg egy új [ApplicationPackageReference] [ net_pkgref] hello új verziója, és a véglegesítési hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="10a51-282">tooupdate hello pool's nodes with version 2.76b, specify a new [ApplicationPackageReference][net_pkgref] with hello new version, and commit hello change.</span></span>

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

<span data-ttu-id="10a51-283">Most, hogy hello új verzióra van beállítva, hello Batch-szolgáltatás telepítése verzió 2.76b tooany *új* csomópont, amelyhez csatlakozik hello készlet.</span><span class="sxs-lookup"><span data-stu-id="10a51-283">Now that hello new version has been configured, hello Batch service installs version 2.76b tooany *new* node that joins hello pool.</span></span> <span data-ttu-id="10a51-284">tooinstall 2.76b csomópontokon hello *már* hello készletben, indítsa újra, vagy újból lemezképet létrehozni őket.</span><span class="sxs-lookup"><span data-stu-id="10a51-284">tooinstall 2.76b on hello nodes that are *already* in hello pool, reboot or reimage them.</span></span> <span data-ttu-id="10a51-285">Vegye figyelembe, hogy újraindított csomópontok megőrzi-e a csomag korábbi telepítések hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="10a51-285">Note that rebooted nodes retain hello files from previous package deployments.</span></span>

## <a name="list-hello-applications-in-a-batch-account"></a><span data-ttu-id="10a51-286">Hello alkalmazások listáján a Batch-fiók</span><span class="sxs-lookup"><span data-stu-id="10a51-286">List hello applications in a Batch account</span></span>
<span data-ttu-id="10a51-287">Hello segítségével is listázhatja hello alkalmazások és a Batch-fiók csomagok [ApplicationOperations][net_appops].[ ListApplicationSummaries] [ net_appops_listappsummaries] metódust.</span><span class="sxs-lookup"><span data-stu-id="10a51-287">You can list hello applications and their packages in a Batch account by using hello [ApplicationOperations][net_appops].[ListApplicationSummaries][net_appops_listappsummaries] method.</span></span>

```csharp
// List hello applications and their application packages in hello Batch account.
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

## <a name="wrap-up"></a><span data-ttu-id="10a51-288">Burkolja</span><span class="sxs-lookup"><span data-stu-id="10a51-288">Wrap up</span></span>
<span data-ttu-id="10a51-289">Az alkalmazáscsomagok esetén az ügyfelek arra, hogy hello alkalmazások kiválasztása, és adja meg a hello pontos verziót toouse, ha engedélyezve van a szolgáltatással a feladatok feldolgozásában, segítségével.</span><span class="sxs-lookup"><span data-stu-id="10a51-289">With application packages, you can help your customers select hello applications for their jobs and specify hello exact version toouse when processing jobs with your Batch-enabled service.</span></span> <span data-ttu-id="10a51-290">Előfordulhat, hogy hello lehetőséget biztosít az ügyfelek tooupload, és nyomon követheti a saját alkalmazásaikat a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="10a51-290">You might also provide hello ability for your customers tooupload and track their own applications in your service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="10a51-291">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10a51-291">Next steps</span></span>
* <span data-ttu-id="10a51-292">Hello [Batch REST API] [ api_rest] is biztosít támogatást toowork alkalmazáscsomagok.</span><span class="sxs-lookup"><span data-stu-id="10a51-292">hello [Batch REST API][api_rest] also provides support toowork with application packages.</span></span> <span data-ttu-id="10a51-293">Lásd például: hello [applicationPackageReferences] [ rest_add_pool_with_packages] elemében [tooan alkalmazáskészlet-fiók hozzáadása] [ rest_add_pool] további információ toospecify csomagok tooinstall hello REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="10a51-293">For example, see hello [applicationPackageReferences][rest_add_pool_with_packages] element in [Add a pool tooan account][rest_add_pool] for information about how toospecify packages tooinstall by using hello REST API.</span></span> <span data-ttu-id="10a51-294">Lásd: [alkalmazások] [ rest_applications] hogyan tooobtain alkalmazással kapcsolatos információk segítségével hello Batch REST API vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="10a51-294">See [Applications][rest_applications] for details about how tooobtain application information by using hello Batch REST API.</span></span>
* <span data-ttu-id="10a51-295">Megtudhatja, hogyan tooprogrammatically [kezelése az Azure Batch fiókjainak és kvótáinak a Batch Management .NET kódtárral](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="10a51-295">Learn how tooprogrammatically [manage Azure Batch accounts and quotas with Batch Management .NET](batch-management-dotnet.md).</span></span> <span data-ttu-id="10a51-296">Hello [Batch Management .NET kódtárral][api_net_mgmt] könyvtár engedélyezheti a fiók létrehozását és törlését funkciói a kötegelt alkalmazást vagy szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="10a51-296">hello [Batch Management .NET][api_net_mgmt] library can enable account creation and deletion features for your Batch application or service.</span></span>

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
