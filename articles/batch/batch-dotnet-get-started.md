---
title: "aaaTutorial - használata hello Azure Batch ügyféloldali kódtára a .NET-hez |} Microsoft Docs"
description: "További tudnivalók az Azure Batch hello alapvető fogalmait, és egyszerű megoldás létrehozása a .NET használatával."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 76cb9807-cbc1-405a-8136-d1e53e66e82b
ms.service: batch
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06062b3886a8081bd9a831824a981503ef55f9b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-building-solutions-with-hello-batch-client-library-for-net"></a><span data-ttu-id="03bae-103">Ismerkedés a .NET-alapú megoldásaikat hello kötegelt ügyféloldali kódtár</span><span class="sxs-lookup"><span data-stu-id="03bae-103">Get started building solutions with hello Batch client library for .NET</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="03bae-104">.NET</span><span class="sxs-lookup"><span data-stu-id="03bae-104">.NET</span></span>](batch-dotnet-get-started.md)
> * [<span data-ttu-id="03bae-105">Python</span><span class="sxs-lookup"><span data-stu-id="03bae-105">Python</span></span>](batch-python-tutorial.md)
> * [<span data-ttu-id="03bae-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="03bae-106">Node.js</span></span>](batch-nodejs-get-started.md)
>
>

<span data-ttu-id="03bae-107">A hello alapvető [Azure Batch] [ azure_batch] és hello [Batch .NET] [ net_api] könyvtár ebben a cikkben egy C# minta alkalmazás lépésről arról lesz szó, a lépést.</span><span class="sxs-lookup"><span data-stu-id="03bae-107">Learn hello basics of [Azure Batch][azure_batch] and hello [Batch .NET][net_api] library in this article as we discuss a C# sample application step by step.</span></span> <span data-ttu-id="03bae-108">Úgy tekintünk, hogyan hello mintaalkalmazás hello Batch szolgáltatás tooprocess használja. Ez a párhuzamos munkaterhelés hello felhőben, és milyen hatással van az [Azure Storage](../storage/common/storage-introduction.md) fájl átmeneti és lekérése.</span><span class="sxs-lookup"><span data-stu-id="03bae-108">We look at how hello sample application leverages hello Batch service tooprocess a parallel workload in hello cloud, and how it interacts with [Azure Storage](../storage/common/storage-introduction.md) for file staging and retrieval.</span></span> <span data-ttu-id="03bae-109">Kell egy közös kötegelt alkalmazás munkafolyamata bemutatása és szerezzen egy alapszintű hello fő összetevőit kötegelt feladatok, feladatok, készletek megértése és számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="03bae-109">You'll learn a common Batch application workflow and gain a base understanding of hello major components of Batch such as jobs, tasks, pools, and compute nodes.</span></span>

<span data-ttu-id="03bae-110">![Batch-megoldás munkafolyamata (alapszintű)][11]</span><span class="sxs-lookup"><span data-stu-id="03bae-110">![Batch solution workflow (basic)][11]</span></span><br/>

## <a name="prerequisites"></a><span data-ttu-id="03bae-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="03bae-111">Prerequisites</span></span>
<span data-ttu-id="03bae-112">Ez a cikk a C# és a Visual Studio gyakorlati ismeretét feltételezi.</span><span class="sxs-lookup"><span data-stu-id="03bae-112">This article assumes that you have a working knowledge of C# and Visual Studio.</span></span> <span data-ttu-id="03bae-113">Azt is feltételezi, hogy Ön képes toosatisfy hello létrehozása követelmények az alább megadott Azure- és hello Batch-és tárolási szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="03bae-113">It also assumes that you're able toosatisfy hello account creation requirements that are specified below for Azure and hello Batch and Storage services.</span></span>

### <a name="accounts"></a><span data-ttu-id="03bae-114">Fiókok</span><span class="sxs-lookup"><span data-stu-id="03bae-114">Accounts</span></span>
* <span data-ttu-id="03bae-115">**Azure-fiók**: Ha még nincs Azure-előfizetése, [hozzon létre egy ingyenes Azure-fiókot][azure_free_account].</span><span class="sxs-lookup"><span data-stu-id="03bae-115">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account][azure_free_account].</span></span>
* <span data-ttu-id="03bae-116">**Batch-fiók**: Ha már rendelkezik Azure-előfizetéssel, [hozzon létre egy Azure Batch-fiókot](batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="03bae-116">**Batch account**: Once you have an Azure subscription, [create an Azure Batch account](batch-account-create-portal.md).</span></span>
* <span data-ttu-id="03bae-117">**Tárfiók**: Lásd a [Tudnivalók az Azure Storage-fiókokról](../storage/common/storage-create-storage-account.md) cikk [Tárfiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account) szakaszát.</span><span class="sxs-lookup"><span data-stu-id="03bae-117">**Storage account**: See [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="03bae-118">Jelenleg a Batch-támogatja *csak* hello **általános célú** tárfióktípus, #5. lépésben leírtak [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) a [kapcsolatos Azure Storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="03bae-118">Batch currently supports *only* hello **general-purpose** storage account type, as described in step #5 [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
>
>

### <a name="visual-studio"></a><span data-ttu-id="03bae-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03bae-119">Visual Studio</span></span>
<span data-ttu-id="03bae-120">Rendelkeznie kell **Visual Studio 2015-ös vagy újabb** toobuild hello mintaprojektet.</span><span class="sxs-lookup"><span data-stu-id="03bae-120">You must have **Visual Studio 2015 or newer** toobuild hello sample project.</span></span> <span data-ttu-id="03bae-121">Visual Studio ingyenes, mind a próbaverziós verziójának hello található [Visual Studio termékek áttekintése][visual_studio].</span><span class="sxs-lookup"><span data-stu-id="03bae-121">You can find free and trial versions of Visual Studio in hello [overview of Visual Studio products][visual_studio].</span></span>

### <a name="dotnettutorial-code-sample"></a><span data-ttu-id="03bae-122">*DotNetTutorial* kódminta</span><span class="sxs-lookup"><span data-stu-id="03bae-122">*DotNetTutorial* code sample</span></span>
<span data-ttu-id="03bae-123">Hello [DotNetTutorial] [ github_dotnettutorial] minta egyike sok kötegelt mintakódok található hello hello [azure-köteg-minták] [ github_samples] tárházából GitHub.</span><span class="sxs-lookup"><span data-stu-id="03bae-123">hello [DotNetTutorial][github_dotnettutorial] sample is one of hello many Batch code samples found in hello [azure-batch-samples][github_samples] repository on GitHub.</span></span> <span data-ttu-id="03bae-124">Összes hello minta kattintva letöltheti **Klónozás vagy letöltési > töltse le a ZIP-** hello tárház kezdőlapján, vagy kattintson a hello [azure-köteg-minták-master.zip] [ github_samples_zip]közvetlen letöltési hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="03bae-124">You can download all hello samples by clicking  **Clone or download > Download ZIP** on hello repository home page, or by clicking hello [azure-batch-samples-master.zip][github_samples_zip] direct download link.</span></span> <span data-ttu-id="03bae-125">Miután kibontotta már hello hello ZIP-fájl tartalmát, található hello megoldás hello a következő mappát:</span><span class="sxs-lookup"><span data-stu-id="03bae-125">Once you've extracted hello contents of hello ZIP file, you can find hello solution in hello following folder:</span></span>

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a><span data-ttu-id="03bae-126">Azure Batch Explorer (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="03bae-126">Azure Batch Explorer (optional)</span></span>
<span data-ttu-id="03bae-127">Hello [Azure Batch Explorer] [ github_batchexplorer] szabad segédprogram, amely része a hello [azure-köteg-minták] [ github_samples] GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="03bae-127">hello [Azure Batch Explorer][github_batchexplorer] is a free utility that is included in hello [azure-batch-samples][github_samples] repository on GitHub.</span></span> <span data-ttu-id="03bae-128">Nem szükséges toocomplete közben ebben az oktatóanyagban ez hasznos lehet a fejlesztési és a kötegelt megoldások hibakeresési közben.</span><span class="sxs-lookup"><span data-stu-id="03bae-128">While not required toocomplete this tutorial, it can be useful while developing and debugging your Batch solutions.</span></span>

## <a name="dotnettutorial-sample-project-overview"></a><span data-ttu-id="03bae-129">DotNetTutorial mintaprojekt áttekintése</span><span class="sxs-lookup"><span data-stu-id="03bae-129">DotNetTutorial sample project overview</span></span>
<span data-ttu-id="03bae-130">Hello *DotNetTutorial* kódminta egy Visual Studio megoldás, amely két projektet tartalmaz: **DotNetTutorial** és **TaskApplication**.</span><span class="sxs-lookup"><span data-stu-id="03bae-130">hello *DotNetTutorial* code sample is a Visual Studio solution that consists of two projects: **DotNetTutorial** and **TaskApplication**.</span></span>

* <span data-ttu-id="03bae-131">**DotNetTutorial** hello ügyfél alkalmazás, amely a számítási csomópontok (virtuális gépek) párhuzamos terhelése hello kötegelt és tárolási szolgáltatások tooexecute kommunikál.</span><span class="sxs-lookup"><span data-stu-id="03bae-131">**DotNetTutorial** is hello client application that interacts with hello Batch and Storage services tooexecute a parallel workload on compute nodes (virtual machines).</span></span> <span data-ttu-id="03bae-132">A DotNetTutorial a helyi munkaállomáson fut.</span><span class="sxs-lookup"><span data-stu-id="03bae-132">DotNetTutorial runs on your local workstation.</span></span>
* <span data-ttu-id="03bae-133">**TaskApplication** Azure tooperform hello tényleges munka számítási csomópontjain futó hello program.</span><span class="sxs-lookup"><span data-stu-id="03bae-133">**TaskApplication** is hello program that runs on compute nodes in Azure tooperform hello actual work.</span></span> <span data-ttu-id="03bae-134">Hello mintában `TaskApplication.exe` elemez hello (hello bemeneti fájl) Azure Storage-ból letöltött fájlok szöveget.</span><span class="sxs-lookup"><span data-stu-id="03bae-134">In hello sample, `TaskApplication.exe` parses hello text in a file downloaded from Azure Storage (hello input file).</span></span> <span data-ttu-id="03bae-135">Ezután azt szöveges fájlt hoz létre (hello kimeneti fájl), amely hello a három legfontosabb szereplő szavakkal hello bemeneti fájl listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="03bae-135">Then it produces a text file (hello output file) that contains a list of hello top three words that appear in hello input file.</span></span> <span data-ttu-id="03bae-136">Miután létrehoz hello kimeneti fájlt, TaskApplication fájlfeltöltések hello fájl tooAzure tárolási.</span><span class="sxs-lookup"><span data-stu-id="03bae-136">After it creates hello output file, TaskApplication uploads hello file tooAzure Storage.</span></span> <span data-ttu-id="03bae-137">Így elérhető toohello ügyfélalkalmazás letölthető.</span><span class="sxs-lookup"><span data-stu-id="03bae-137">This makes it available toohello client application for download.</span></span> <span data-ttu-id="03bae-138">TaskApplication hello Batch szolgáltatás több számítási csomóponton párhuzamosan fut.</span><span class="sxs-lookup"><span data-stu-id="03bae-138">TaskApplication runs in parallel on multiple compute nodes in hello Batch service.</span></span>

<span data-ttu-id="03bae-139">hello következő diagram azt ábrázolja, hello hello ügyfélalkalmazás, által elvégzett elsődleges műveletek *DotNetTutorial*, és hello alkalmazás hello feladatok által végrehajtott *TaskApplication*.</span><span class="sxs-lookup"><span data-stu-id="03bae-139">hello following diagram illustrates hello primary operations that are performed by hello client application, *DotNetTutorial*, and hello application that is executed by hello tasks, *TaskApplication*.</span></span> <span data-ttu-id="03bae-140">Ez az alapvető munkafolyamat számos, a Batch használatával létrehozott számítási megoldásra jellemző.</span><span class="sxs-lookup"><span data-stu-id="03bae-140">This basic workflow is typical of many compute solutions that are created with Batch.</span></span> <span data-ttu-id="03bae-141">Minden elérhető, a Batch szolgáltatás hello szolgáltatást nem azt mutatják, amíg a szinte minden kötegelt az eset tartalmazza a munkafolyamat részeit.</span><span class="sxs-lookup"><span data-stu-id="03bae-141">While it does not demonstrate every feature available in hello Batch service, nearly every Batch scenario includes portions of this workflow.</span></span>

<span data-ttu-id="03bae-142">![Példa Batch-munkafolyamat][8]</span><span class="sxs-lookup"><span data-stu-id="03bae-142">![Batch example workflow][8]</span></span><br/>

[<span data-ttu-id="03bae-143">**1. lépés**</span><span class="sxs-lookup"><span data-stu-id="03bae-143">**Step 1.**</span></span>](#step-1-create-storage-containers) <span data-ttu-id="03bae-144">**Tárolók** létrehozása az Azure Blob Storage-ban.</span><span class="sxs-lookup"><span data-stu-id="03bae-144">Create **containers** in Azure Blob Storage.</span></span><br/><span data-ttu-id="03bae-145">
[**2. lépés**](#step-2-upload-task-application-and-data-files)</span><span class="sxs-lookup"><span data-stu-id="03bae-145">
[**Step 2.**](#step-2-upload-task-application-and-data-files)</span></span> <span data-ttu-id="03bae-146">Feltöltési feladat fájljainak és a bemeneti fájlok toocontainers.</span><span class="sxs-lookup"><span data-stu-id="03bae-146">Upload task application files and input files toocontainers.</span></span><br/><span data-ttu-id="03bae-147">
[**3. lépés**](#step-3-create-batch-pool)</span><span class="sxs-lookup"><span data-stu-id="03bae-147">
[**Step 3.**](#step-3-create-batch-pool)</span></span> <span data-ttu-id="03bae-148">Batch-**készlet** létrehozása.</span><span class="sxs-lookup"><span data-stu-id="03bae-148">Create a Batch **pool**.</span></span><br/>
  <span data-ttu-id="03bae-149">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span><span class="sxs-lookup"><span data-stu-id="03bae-149">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span></span> <span data-ttu-id="03bae-150">készlet hello **StartTask** letöltések hello feladat bináris fájlokat (TaskApplication) toonodes, mivel azok csatlakoztatását hello készlet.</span><span class="sxs-lookup"><span data-stu-id="03bae-150">hello pool **StartTask** downloads hello task binary files (TaskApplication) toonodes as they join hello pool.</span></span><br/><span data-ttu-id="03bae-151">
[**4. lépés**](#step-4-create-batch-job)</span><span class="sxs-lookup"><span data-stu-id="03bae-151">
[**Step 4.**](#step-4-create-batch-job)</span></span> <span data-ttu-id="03bae-152">Batch-**feladat** létrehozása.</span><span class="sxs-lookup"><span data-stu-id="03bae-152">Create a Batch **job**.</span></span><br/><span data-ttu-id="03bae-153">
[**5. lépés**](#step-5-add-tasks-to-job)</span><span class="sxs-lookup"><span data-stu-id="03bae-153">
[**Step 5.**](#step-5-add-tasks-to-job)</span></span> <span data-ttu-id="03bae-154">Adja hozzá **feladatok** toohello feladat.</span><span class="sxs-lookup"><span data-stu-id="03bae-154">Add **tasks** toohello job.</span></span><br/>
  <span data-ttu-id="03bae-155">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span><span class="sxs-lookup"><span data-stu-id="03bae-155">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span></span> <span data-ttu-id="03bae-156">hello feladatok ütemezett tooexecute csomópontján.</span><span class="sxs-lookup"><span data-stu-id="03bae-156">hello tasks are scheduled tooexecute on nodes.</span></span><br/>
    <span data-ttu-id="03bae-157">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span><span class="sxs-lookup"><span data-stu-id="03bae-157">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span></span> <span data-ttu-id="03bae-158">Mindegyik tevékenység letölti a bemeneti adatait az Azure Storage-ból, majd elkezdi a végrehajtást.</span><span class="sxs-lookup"><span data-stu-id="03bae-158">Each task downloads its input data from Azure Storage, then begins execution.</span></span><br/><span data-ttu-id="03bae-159">
[**6. lépés**](#step-6-monitor-tasks)</span><span class="sxs-lookup"><span data-stu-id="03bae-159">
[**Step 6.**](#step-6-monitor-tasks)</span></span> <span data-ttu-id="03bae-160">Tevékenységek figyelése.</span><span class="sxs-lookup"><span data-stu-id="03bae-160">Monitor tasks.</span></span><br/>
  <span data-ttu-id="03bae-161">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span><span class="sxs-lookup"><span data-stu-id="03bae-161">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span></span> <span data-ttu-id="03bae-162">Feladatok befejezésekor, ezek a kimeneti adatok tooAzure tárolási feltöltése.</span><span class="sxs-lookup"><span data-stu-id="03bae-162">As tasks are completed, they upload their output data tooAzure Storage.</span></span><br/><span data-ttu-id="03bae-163">
[**7. lépés**](#step-7-download-task-output)</span><span class="sxs-lookup"><span data-stu-id="03bae-163">
[**Step 7.**](#step-7-download-task-output)</span></span> <span data-ttu-id="03bae-164">Tevékenység kimenetének letöltése a Storage-ból.</span><span class="sxs-lookup"><span data-stu-id="03bae-164">Download task output from Storage.</span></span>

<span data-ttu-id="03bae-165">Ahogy azt korábban említettük, nem minden kötegelt megoldásnak a pontos lépéseket végzi el, és előfordulhat, hogy sok más többek között hello *DotNetTutorial* mintaalkalmazás található kötegelt megoldás az általános folyamatoktól mutatja be.</span><span class="sxs-lookup"><span data-stu-id="03bae-165">As mentioned, not every Batch solution performs these exact steps, and may include many more, but hello *DotNetTutorial* sample application demonstrates common processes found in a Batch solution.</span></span>

## <a name="build-hello-dotnettutorial-sample-project"></a><span data-ttu-id="03bae-166">Build hello *DotNetTutorial* mintaprojektet</span><span class="sxs-lookup"><span data-stu-id="03bae-166">Build hello *DotNetTutorial* sample project</span></span>
<span data-ttu-id="03bae-167">Hello minta sikeres futtatásához, meg kell adnia kötegelt és a tárolási fiók hitelesítő adatait hello *DotNetTutorial* projekt `Program.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="03bae-167">Before you can successfully run hello sample, you must specify both Batch and Storage account credentials in hello *DotNetTutorial* project's `Program.cs` file.</span></span> <span data-ttu-id="03bae-168">Ha még nem tette meg, nyissa meg hello megoldást a Visual Studio hello duplán kattintva `DotNetTutorial.sln` megoldásfájlt.</span><span class="sxs-lookup"><span data-stu-id="03bae-168">If you have not done so already, open hello solution in Visual Studio by double-clicking hello `DotNetTutorial.sln` solution file.</span></span> <span data-ttu-id="03bae-169">Nyissa meg a Visual Studio használatával hello vagy **fájl > Nyissa meg a > Projekt/megoldás** menü.</span><span class="sxs-lookup"><span data-stu-id="03bae-169">Or open it from within Visual Studio by using hello **File > Open > Project/Solution** menu.</span></span>

<span data-ttu-id="03bae-170">Nyissa meg `Program.cs` belül hello *DotNetTutorial* projekt.</span><span class="sxs-lookup"><span data-stu-id="03bae-170">Open `Program.cs` within hello *DotNetTutorial* project.</span></span> <span data-ttu-id="03bae-171">Majd adja hozzá a hitelesítő adatok megadott hello tetején hello fájlt:</span><span class="sxs-lookup"><span data-stu-id="03bae-171">Then add your credentials as specified near hello top of hello file:</span></span>

```csharp
// Update hello Batch and Storage account credential strings below with hello values
// unique tooyour accounts. These are used when constructing connection strings
// for hello Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [!IMPORTANT]
> <span data-ttu-id="03bae-172">Fent említett jelenleg adjon meg hitelesítő adatokat hello egy **általános célú** storage Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="03bae-172">As mentioned above, you must currently specify hello credentials for a **general-purpose** storage account in Azure Storage.</span></span> <span data-ttu-id="03bae-173">A Batch-alkalmazások használata a blob storage belül hello **általános célú** storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="03bae-173">Your Batch applications use blob storage within hello **general-purpose** storage account.</span></span> <span data-ttu-id="03bae-174">Ne adjon meg hello hello kiválasztásával létrehozott tárfiók hitelesítő adatainak *Blob-tároló* fiók típusa.</span><span class="sxs-lookup"><span data-stu-id="03bae-174">Do not specify hello credentials for a Storage account that was created by selecting hello *Blob storage* account type.</span></span>
>
>

<span data-ttu-id="03bae-175">A kötegelt és a tárolási fiók hitelesítő adataival belül minden szolgáltatás hello fiók panelen található hello [Azure-portálon][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="03bae-175">You can find your Batch and Storage account credentials within hello account blade of each service in hello [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="03bae-176">![A Batch-hitelesítő adatok hello portálon][9]
![hello portal tároló hitelesítő adatait][10]</span><span class="sxs-lookup"><span data-stu-id="03bae-176">![Batch credentials in hello portal][9]
![Storage credentials in hello portal][10]</span></span><br/>

<span data-ttu-id="03bae-177">Most, hogy a hitelesítő adataival hello projekt frissítette, kattintson a jobb gombbal a hello megoldás a Megoldáskezelőben, majd kattintson **megoldás fordítása**.</span><span class="sxs-lookup"><span data-stu-id="03bae-177">Now that you've updated hello project with your credentials, right-click hello solution in Solution Explorer and click **Build Solution**.</span></span> <span data-ttu-id="03bae-178">Hello helyreállítása a minden NuGet-csomagok, győződjön meg arról, ha a számítógép.</span><span class="sxs-lookup"><span data-stu-id="03bae-178">Confirm hello restoration of any NuGet packages, if you're prompted.</span></span>

> [!TIP]
> <span data-ttu-id="03bae-179">Ha hello NuGet-csomagok nem automatikusan vissza, vagy egy hiba toorestore hello csomagok hibába ütközik, ellenőrizze, hogy rendelkezik-e hello [NuGet-Csomagkezelő] [ nuget_packagemgr] telepítve.</span><span class="sxs-lookup"><span data-stu-id="03bae-179">If hello NuGet packages are not automatically restored, or if you see errors about a failure toorestore hello packages, ensure that you have hello [NuGet Package Manager][nuget_packagemgr] installed.</span></span> <span data-ttu-id="03bae-180">Engedélyezze a hello letöltése a csomagok hiányzik.</span><span class="sxs-lookup"><span data-stu-id="03bae-180">Then enable hello download of missing packages.</span></span> <span data-ttu-id="03bae-181">Lásd: [engedélyezése csomag visszaállítása során Build] [ nuget_restore] tooenable csomag letöltése.</span><span class="sxs-lookup"><span data-stu-id="03bae-181">See [Enabling Package Restore During Build][nuget_restore] tooenable package download.</span></span>
>
>

<span data-ttu-id="03bae-182">A következő részekben hello azt felosztása le hello mintaalkalmazás hello lépéseket, hogy tooprocess segítségével végzi a Batch szolgáltatás hello munkaterhelés, és ezeket a lépéseket részletesen ismertetik.</span><span class="sxs-lookup"><span data-stu-id="03bae-182">In hello following sections, we break down hello sample application into hello steps that it performs tooprocess a workload in hello Batch service, and discuss those steps in detail.</span></span> <span data-ttu-id="03bae-183">Javasoljuk, toorefer toohello megoldás nyissa meg a Visual Studióban, mivel nem minden kódsort hello minta tárgyalt a módja, ez a cikk többi hello útján munka közben.</span><span class="sxs-lookup"><span data-stu-id="03bae-183">We encourage you toorefer toohello open solution in Visual Studio while you work your way through hello rest of this article, since not every line of code in hello sample is discussed.</span></span>

<span data-ttu-id="03bae-184">Keresse meg a toohello felső részén hello `MainAsync` metódus a hello *DotNetTutorial* projekt `Program.cs` fájl toostart az 1. lépés.</span><span class="sxs-lookup"><span data-stu-id="03bae-184">Navigate toohello top of hello `MainAsync` method in hello *DotNetTutorial* project's `Program.cs` file toostart with Step 1.</span></span> <span data-ttu-id="03bae-185">Minden egyes lépést alatt, majd nagyjából követi hello előmenetel metódus meghívja `MainAsync`.</span><span class="sxs-lookup"><span data-stu-id="03bae-185">Each step below then roughly follows hello progression of method calls in `MainAsync`.</span></span>

## <a name="step-1-create-storage-containers"></a><span data-ttu-id="03bae-186">1. lépés: Storage-tárolók létrehozása</span><span class="sxs-lookup"><span data-stu-id="03bae-186">Step 1: Create Storage containers</span></span>
<span data-ttu-id="03bae-187">![Tárolók létrehozása az Azure Storage-ban][1]
</span><span class="sxs-lookup"><span data-stu-id="03bae-187">![Create containers in Azure Storage][1]
</span></span><br/>

<span data-ttu-id="03bae-188">A Batch beépített támogatást tartalmaz az Azure Storage használatához.</span><span class="sxs-lookup"><span data-stu-id="03bae-188">Batch includes built-in support for interacting with Azure Storage.</span></span> <span data-ttu-id="03bae-189">A tárfiók a tárolók hello feladatok futtatása a Batch-fiók szükséges hello fájlokat ad meg.</span><span class="sxs-lookup"><span data-stu-id="03bae-189">Containers in your Storage account will provide hello files needed by hello tasks that run in your Batch account.</span></span> <span data-ttu-id="03bae-190">hello tárolók nagy előnye, egy hely toostore hello kimeneti adatok, amelyek hello feladatok egymással.</span><span class="sxs-lookup"><span data-stu-id="03bae-190">hello containers also provide a place toostore hello output data that hello tasks produce.</span></span> <span data-ttu-id="03bae-191">először thing hello hello *DotNetTutorial* ügyfél alkalmazás hozható létre a három tároló [Azure Blob Storage](../storage/common/storage-introduction.md):</span><span class="sxs-lookup"><span data-stu-id="03bae-191">hello first thing hello *DotNetTutorial* client application does is create three containers in [Azure Blob Storage](../storage/common/storage-introduction.md):</span></span>

* <span data-ttu-id="03bae-192">**alkalmazás**: Ez a tároló hello feladatok, valamint minden függőségét, például DLL-ek által futtatott hello alkalmazás fogja tárolni.</span><span class="sxs-lookup"><span data-stu-id="03bae-192">**application**: This container will store hello application run by hello tasks, as well as any of its dependencies, such as DLLs.</span></span>
* <span data-ttu-id="03bae-193">**bemeneti**: feladatok töltik le hello adatok fájlok tooprocess hello *bemeneti* tároló.</span><span class="sxs-lookup"><span data-stu-id="03bae-193">**input**: Tasks will download hello data files tooprocess from hello *input* container.</span></span>
* <span data-ttu-id="03bae-194">**kimeneti**: a bemeneti fájl feldolgozása végeztével a feladatok hello eredmények toohello feltölti *kimeneti* tároló.</span><span class="sxs-lookup"><span data-stu-id="03bae-194">**output**: When tasks complete input file processing, they will upload hello results toohello *output* container.</span></span>

<span data-ttu-id="03bae-195">Egy tároló rendelés toointeract a fiókot, és tárolók, hozzon létre hello használjuk [Azure Storage ügyféloldali kódtára a .NET][net_api_storage].</span><span class="sxs-lookup"><span data-stu-id="03bae-195">In order toointeract with a Storage account and create containers, we use hello [Azure Storage Client Library for .NET][net_api_storage].</span></span> <span data-ttu-id="03bae-196">Egy hivatkozási toohello fiókhoz létrehozhatunk [CloudStorageAccount][net_cloudstorageaccount], és, hogy hozzon létre egy [CloudBlobClient][net_cloudblobclient]:</span><span class="sxs-lookup"><span data-stu-id="03bae-196">We create a reference toohello account with [CloudStorageAccount][net_cloudstorageaccount], and from that create a [CloudBlobClient][net_cloudblobclient]:</span></span>

```csharp
// Construct hello Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve hello storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create hello blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

<span data-ttu-id="03bae-197">Hello használjuk `blobClient` hello alkalmazás teljes hivatkoznak, és adja át paraméterként tooseveral módszerek.</span><span class="sxs-lookup"><span data-stu-id="03bae-197">We use hello `blobClient` reference throughout hello application and pass it as a parameter tooseveral methods.</span></span> <span data-ttu-id="03bae-198">Például az utána következő hello fenti, ahol nevezzük hello kódblokk van `CreateContainerIfNotExistAsync` tooactually hello tárolók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="03bae-198">An example of this is in hello code block that immediately follows hello above, where we call `CreateContainerIfNotExistAsync` tooactually create hello containers.</span></span>

```csharp
// Use hello blob client toocreate hello containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

<span data-ttu-id="03bae-199">Hello tárolók létrehozása után, hello alkalmazás most már feltöltheti hello feladatok által használt hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="03bae-199">Once hello containers have been created, hello application can now upload hello files that will be used by hello tasks.</span></span>

> [!TIP]
> <span data-ttu-id="03bae-200">[Hogyan toouse Blob Storage a .NET-](../storage/blobs/storage-dotnet-how-to-use-blobs.md) jó áttekintést nyújt az Azure Storage tárolók és blobok használata.</span><span class="sxs-lookup"><span data-stu-id="03bae-200">[How toouse Blob Storage from .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) provides a good overview of working with Azure Storage containers and blobs.</span></span> <span data-ttu-id="03bae-201">Meg kell az olvasási lista hello tetején, az Batch használatának megkezdése.</span><span class="sxs-lookup"><span data-stu-id="03bae-201">It should be near hello top of your reading list as you start working with Batch.</span></span>
>
>

## <a name="step-2-upload-task-application-and-data-files"></a><span data-ttu-id="03bae-202">2. lépés: Tevékenységalkalmazás- és adatfájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="03bae-202">Step 2: Upload task application and data files</span></span>
<span data-ttu-id="03bae-203">![Feltöltési feladat alkalmazás- és bemeneti (adatok) fájlok toocontainers][2]
</span><span class="sxs-lookup"><span data-stu-id="03bae-203">![Upload task application and input (data) files toocontainers][2]
</span></span><br/>

<span data-ttu-id="03bae-204">Hello fájl feltöltése a művelet, *DotNetTutorial* először határozza meg a gyűjteményei **alkalmazás** és **bemeneti** elérési utak fájlt a helyi számítógépen hello léteznek.</span><span class="sxs-lookup"><span data-stu-id="03bae-204">In hello file upload operation, *DotNetTutorial* first defines collections of **application** and **input** file paths as they exist on hello local machine.</span></span> <span data-ttu-id="03bae-205">Majd ezen fájlok toohello tárolók hello előző lépésben létrehozott feltölti azt.</span><span class="sxs-lookup"><span data-stu-id="03bae-205">Then it uploads these files toohello containers that you created in hello previous step.</span></span>

```csharp
// Paths toohello executable and its dependencies that will be executed by hello tasks
List<string> applicationFilePaths = new List<string>
{
    // hello DotNetTutorial project includes a project reference tooTaskApplication,
    // allowing us toodetermine hello path of hello task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// hello collection of data files that are toobe processed by hello tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload hello application and its dependencies tooAzure Storage. This is the
// application that will process hello data files, and will be executed by each
// of hello tasks on hello compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload hello data files. This is hello data that will be processed by each of
// hello tasks that are executed on hello compute nodes within hello pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

<span data-ttu-id="03bae-206">A két módszer `Program.cs` hello feltöltési folyamat részt:</span><span class="sxs-lookup"><span data-stu-id="03bae-206">There are two methods in `Program.cs` that are involved in hello upload process:</span></span>

* <span data-ttu-id="03bae-207">`UploadFilesToContainerAsync`: Ez a módszer gyűjteményének beolvasása az [ResourceFile] [ net_resourcefile] objektumok (alább), és belső hívások `UploadFileToContainerAsync` minden fájlt tooupload átadott hello *filePaths* paraméter.</span><span class="sxs-lookup"><span data-stu-id="03bae-207">`UploadFilesToContainerAsync`: This method returns a collection of [ResourceFile][net_resourcefile] objects (discussed below) and internally calls `UploadFileToContainerAsync` tooupload each file that is passed in hello *filePaths* parameter.</span></span>
* <span data-ttu-id="03bae-208">`UploadFileToContainerAsync`: Ez a ténylegesen hello fájlfeltöltés hajt végre, és létrehozza a hello hello módszer [ResourceFile] [ net_resourcefile] objektumok.</span><span class="sxs-lookup"><span data-stu-id="03bae-208">`UploadFileToContainerAsync`: This is hello method that actually performs hello file upload and creates hello [ResourceFile][net_resourcefile] objects.</span></span> <span data-ttu-id="03bae-209">Hello fájl feltöltése után azt jut hozzá a közös hozzáférésű jogosultságkód (SAS) hello fájl és egy azt képviselő ResourceFile objektum beállítása/beolvasása.</span><span class="sxs-lookup"><span data-stu-id="03bae-209">After uploading hello file, it obtains a shared access signature (SAS) for hello file and returns a ResourceFile object that represents it.</span></span> <span data-ttu-id="03bae-210">A közös hozzáférésű jogosultságkódok ismertetése is az alábbiakban olvasható.</span><span class="sxs-lookup"><span data-stu-id="03bae-210">Shared access signatures are also discussed below.</span></span>

```csharp
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} toocontainer [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath);

        // Set hello expiry time and permissions for hello blob shared access signature.
        // In this case, no start time is specified, so hello shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct hello SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a><span data-ttu-id="03bae-211">ResourceFiles</span><span class="sxs-lookup"><span data-stu-id="03bae-211">ResourceFiles</span></span>
<span data-ttu-id="03bae-212">A [ResourceFile] [ net_resourcefile] biztosít hello URL-cím tooa fájl, amely letöltött tooa Azure Storage a kötegelt feladatok számítási csomópont e feladat futása előtt.</span><span class="sxs-lookup"><span data-stu-id="03bae-212">A [ResourceFile][net_resourcefile] provides tasks in Batch with hello URL tooa file in Azure Storage that is downloaded tooa compute node before that task is run.</span></span> <span data-ttu-id="03bae-213">Hello [ResourceFile.BlobSource] [ net_resourcefile_blobsource] tulajdonság hello teljes URL-címet hello fájl határozza meg, az Azure Storage található.</span><span class="sxs-lookup"><span data-stu-id="03bae-213">hello [ResourceFile.BlobSource][net_resourcefile_blobsource] property specifies hello full URL of hello file as it exists in Azure Storage.</span></span> <span data-ttu-id="03bae-214">hello URL-címet is a közös hozzáférésű jogosultságkód (SAS), amely toohello fájlt biztonságos hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="03bae-214">hello URL may also include a shared access signature (SAS) that provides secure access toohello file.</span></span> <span data-ttu-id="03bae-215">A Batch .NET-en belüli legtöbb tevékenység tartalmaz egy *ResourceFiles* tulajdonságot, beleértve a következőket:</span><span class="sxs-lookup"><span data-stu-id="03bae-215">Most tasks types within Batch .NET include a *ResourceFiles* property, including:</span></span>

* <span data-ttu-id="03bae-216">[CloudTask][net_task]</span><span class="sxs-lookup"><span data-stu-id="03bae-216">[CloudTask][net_task]</span></span>
* <span data-ttu-id="03bae-217">[StartTask][net_pool_starttask]</span><span class="sxs-lookup"><span data-stu-id="03bae-217">[StartTask][net_pool_starttask]</span></span>
* <span data-ttu-id="03bae-218">[JobPreparationTask][net_jobpreptask]</span><span class="sxs-lookup"><span data-stu-id="03bae-218">[JobPreparationTask][net_jobpreptask]</span></span>
* <span data-ttu-id="03bae-219">[JobReleaseTask][net_jobreltask]</span><span class="sxs-lookup"><span data-stu-id="03bae-219">[JobReleaseTask][net_jobreltask]</span></span>

<span data-ttu-id="03bae-220">hello DotNetTutorial mintaalkalmazás hello JobPreparationTask vagy JobReleaseTask tevékenységtípusok nem használható, de további a rájuk vonatkozó [Futtatás feladat előkészítése és a befejezési feladatok Azure Batch számítási csomópontjain](batch-job-prep-release.md).</span><span class="sxs-lookup"><span data-stu-id="03bae-220">hello DotNetTutorial sample application does not use hello JobPreparationTask or JobReleaseTask task types, but you can read more about them in [Run job preparation and completion tasks on Azure Batch compute nodes](batch-job-prep-release.md).</span></span>

### <a name="shared-access-signature-sas"></a><span data-ttu-id="03bae-221">Közös hozzáférésű jogosultságkód (SAS)</span><span class="sxs-lookup"><span data-stu-id="03bae-221">Shared access signature (SAS)</span></span>
<span data-ttu-id="03bae-222">Közös hozzáférésű aláírások karakterláncok, amelyek – amikor egy URL-cím részét képező – adja meg a biztonságos hozzáférést toocontainers és az Azure Storage blobs.</span><span class="sxs-lookup"><span data-stu-id="03bae-222">Shared access signatures are strings which—when included as part of a URL—provide secure access toocontainers and blobs in Azure Storage.</span></span> <span data-ttu-id="03bae-223">hello DotNetTutorial alkalmazás mindkét blob és tároló megosztott hozzáférési aláírást URL-címeket, és bemutatja, hogyan ezek megosztott tooobtain elérje aláírás karakterláncok hello tároló szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="03bae-223">hello DotNetTutorial application uses both blob and container shared access signature URLs, and demonstrates how tooobtain these shared access signature strings from hello Storage service.</span></span>

* <span data-ttu-id="03bae-224">**A BLOB megosztott hozzáférési aláírásokkal**: hello készlet StartTask DotNetTutorial a blob megosztott hozzáférési aláírásokkal használ, letöltésekor hello alkalmazás bináris fájljait és a bemeneti adatok fájlok a tároló (lásd az alábbi #3. lépés).</span><span class="sxs-lookup"><span data-stu-id="03bae-224">**Blob shared access signatures**: hello pool's StartTask in DotNetTutorial uses blob shared access signatures when it downloads hello application binaries and input data files from Storage (see Step #3 below).</span></span> <span data-ttu-id="03bae-225">Hello `UploadFileToContainerAsync` DotNetTutorial tartozó metódus `Program.cs` hello kódot tartalmaz, amely minden egyes blob megosztott hozzáférési aláírást beolvassa.</span><span class="sxs-lookup"><span data-stu-id="03bae-225">hello `UploadFileToContainerAsync` method in DotNetTutorial's `Program.cs` contains hello code that obtains each blob's shared access signature.</span></span> <span data-ttu-id="03bae-226">Ezt a [CloudBlob.GetSharedAccessSignature][net_sas_blob] meghívásával végzi el.</span><span class="sxs-lookup"><span data-stu-id="03bae-226">It does so by calling [CloudBlob.GetSharedAccessSignature][net_sas_blob].</span></span>
* <span data-ttu-id="03bae-227">**Tároló közös hozzáférésű jogosultságkód**: minden tevékenység befejezése hello számítási csomóponton teendőit, mivel azt feltölti a kimeneti fájl toohello *kimeneti* az Azure Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="03bae-227">**Container shared access signatures**: As each task finishes its work on hello compute node, it uploads its output file toohello *output* container in Azure Storage.</span></span> <span data-ttu-id="03bae-228">toodo tehát TaskApplication használ egy tároló közös hozzáférésű jogosultságkódot, amely írási hozzáférés toohello tárolót biztosít hello elérési út részeként, ha azt feltölt hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="03bae-228">toodo so, TaskApplication uses a container shared access signature that provides write access toohello container as part of hello path when it uploads hello file.</span></span> <span data-ttu-id="03bae-229">Nem sikerült beolvasni hello tároló közös hozzáférésű jogosultságkódot, ha nem sikerült beolvasni hello blob megosztott hozzáférési aláírást hasonló módon végezhető el.</span><span class="sxs-lookup"><span data-stu-id="03bae-229">Obtaining hello container shared access signature is done in a similar fashion as when obtaining hello blob shared access signature.</span></span> <span data-ttu-id="03bae-230">DotNetTutorial, adott hello található `GetContainerSasUrl` segítő metódushívások [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] toodo így.</span><span class="sxs-lookup"><span data-stu-id="03bae-230">In DotNetTutorial, you will find that hello `GetContainerSasUrl` helper method calls [CloudBlobContainer.GetSharedAccessSignature][net_sas_container] toodo so.</span></span> <span data-ttu-id="03bae-231">Lesz olvasható további információ arról, hogyan TaskApplication használja hello tároló megosztott hozzáférési aláírást a "6. lépés: figyelési feladatok."</span><span class="sxs-lookup"><span data-stu-id="03bae-231">You'll read more about how TaskApplication uses hello container shared access signature in "Step 6: Monitor Tasks."</span></span>

> [!TIP]
> <span data-ttu-id="03bae-232">A közös hozzáférésű jogosultságkódokról hello kétrészes adatsorozat kivételének [1. rész: Understanding hello megosztott hozzáférési jogosultságkód (SAS) alapuló modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md) és [2. rész: létrehozása és a közös hozzáférésű jogosultságkód (SAS) használata a Blob storage](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), további biztosítanak, és a tárfiókban lévő biztonságos hozzáférést toodata toolearn.</span><span class="sxs-lookup"><span data-stu-id="03bae-232">Check out hello two-part series on shared access signatures, [Part 1: Understanding hello shared access signature (SAS) model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) and [Part 2: Create and use a shared access signature (SAS) with Blob storage](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn more about providing secure access toodata in your Storage account.</span></span>
>
>

## <a name="step-3-create-batch-pool"></a><span data-ttu-id="03bae-233">3. lépés: Batch-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="03bae-233">Step 3: Create Batch pool</span></span>
<span data-ttu-id="03bae-234">![Batch-készlet létrehozása][3]
</span><span class="sxs-lookup"><span data-stu-id="03bae-234">![Create a Batch pool][3]
</span></span><br/>

<span data-ttu-id="03bae-235">A Batch-**készletek** olyan számítási csomópontok (virtuális gépek) gyűjteményei, amelyeken a Batch a feladatok tevékenységeit végzi el.</span><span class="sxs-lookup"><span data-stu-id="03bae-235">A Batch **pool** is a collection of compute nodes (virtual machines) on which Batch executes a job's tasks.</span></span>

<span data-ttu-id="03bae-236">Hello alkalmazás és adatok fájlok toohello Azure Storage API-khoz, tárfiók feltöltése után *DotNetTutorial* hívások toohello Batch szolgáltatás végez hello Batch .NET kódtár által nyújtott API-khoz.</span><span class="sxs-lookup"><span data-stu-id="03bae-236">After uploading hello application and data files toohello Storage account with Azure Storage APIs, *DotNetTutorial* begins making calls toohello Batch service with APIs provided by hello Batch .NET library.</span></span> <span data-ttu-id="03bae-237">hello kód először létrehoz egy [BatchClient][net_batchclient]:</span><span class="sxs-lookup"><span data-stu-id="03bae-237">hello code first creates a [BatchClient][net_batchclient]:</span></span>

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

<span data-ttu-id="03bae-238">A következő hello minta létrehoz számítási csomópontok készletét a Batch-fiók hello hívással túl`CreatePoolIfNotExistsAsync`.</span><span class="sxs-lookup"><span data-stu-id="03bae-238">Next, hello sample creates a pool of compute nodes in hello Batch account with a call too`CreatePoolIfNotExistsAsync`.</span></span> <span data-ttu-id="03bae-239">`CreatePoolIfNotExistsAsync`felhasználási hello [BatchClient.PoolOperations.CreatePool] [ net_pool_create] metódus toocreate egy új készletet a Batch szolgáltatás hello:</span><span class="sxs-lookup"><span data-stu-id="03bae-239">`CreatePoolIfNotExistsAsync` uses hello [BatchClient.PoolOperations.CreatePool][net_pool_create] method toocreate a new pool in hello Batch service:</span></span>

```csharp
private static async Task CreatePoolIfNotExistAsync(BatchClient batchClient, string poolId, IList<ResourceFile> resourceFiles)
{
    CloudPool pool = null;
    try
    {
        Console.WriteLine("Creating pool [{0}]...", poolId);

        // Create hello unbound pool. Until we call CloudPool.Commit() or CommitAsync(), no pool is actually created in the
        // Batch service. This CloudPool instance is therefore considered "unbound," and we can modify its properties.
        pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicatedComputeNodes: 3,                                             // 3 compute nodes
            virtualMachineSize: "small",                                                // single-core, 1.75 GB memory, 225 GB disk
            cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));   // Windows Server 2012 R2

        // Create and assign hello StartTask that will be executed when compute nodes join hello pool.
        // In this case, we copy hello StartTask's resource files (that will be automatically downloaded
        // toohello node by hello StartTask) into hello shared directory that all tasks will have access to.
        pool.StartTask = new StartTask
        {
            // Specify a command line for hello StartTask that copies hello task application files toothe
            // node's shared directory. Every compute node in a Batch pool is configured with a number
            // of pre-defined environment variables that can be referenced by commands or applications
            // run by tasks.

            // Since a successful execution of robocopy can return a non-zero exit code (e.g. 1 when one or
            // more files were successfully copied) we need toomanually exit with a 0 for Batch toorecognize
            // StartTask execution success.
            CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
            ResourceFiles = resourceFiles,
            WaitForSuccess = true
        };

        await pool.CommitAsync();
    }
    catch (BatchException be)
    {
        // Swallow hello specific error code PoolExists since that is expected if hello pool already exists
        if (be.RequestInformation?.BatchError != null && be.RequestInformation.BatchError.Code == BatchErrorCodeStrings.PoolExists)
        {
            Console.WriteLine("hello pool {0} already existed when we tried toocreate it", poolId);
        }
        else
        {
            throw; // Any other exception is unexpected
        }
    }
}
```

<span data-ttu-id="03bae-240">A készlet létrehozásakor [CreatePool][net_pool_create], akkor adja meg például a számítási csomópontok hello számos paraméter hello [hello csomópontok méret](../cloud-services/cloud-services-sizes-specs.md), és működési csomópontok hello a rendszer.</span><span class="sxs-lookup"><span data-stu-id="03bae-240">When you create a pool with [CreatePool][net_pool_create], you specify several parameters such as hello number of compute nodes, hello [size of hello nodes](../cloud-services/cloud-services-sizes-specs.md), and hello nodes' operating system.</span></span> <span data-ttu-id="03bae-241">A *DotNetTutorial*, használjuk [CloudServiceConfiguration] [ net_cloudserviceconfiguration] a Windows Server 2012 R2 toospecify [Felhőszolgáltatások](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="03bae-241">In *DotNetTutorial*, we use [CloudServiceConfiguration][net_cloudserviceconfiguration] toospecify Windows Server 2012 R2 from [Cloud Services](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span> 

<span data-ttu-id="03bae-242">Hello megadásával is létrehozhat, amelyek az Azure virtuális gépek (VM) számítási csomópontok készleteinek [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] a készlethez.</span><span class="sxs-lookup"><span data-stu-id="03bae-242">You can also create pools of compute nodes that are Azure Virtual Machines (VMs) by specifying hello [VirtualMachineConfiguration][net_virtualmachineconfiguration] for your pool.</span></span> <span data-ttu-id="03bae-243">A virtuális gépek számításicsomópont-készletei Windows- vagy [Linux-rendszerképből](batch-linux-nodes.md) hozhatóak létre.</span><span class="sxs-lookup"><span data-stu-id="03bae-243">You can create a pool of VM compute nodes from either Windows or [Linux images](batch-linux-nodes.md).</span></span> <span data-ttu-id="03bae-244">a Virtuálisgép-lemezképek hello forrása is lehet:</span><span class="sxs-lookup"><span data-stu-id="03bae-244">hello source for your VM images can be either:</span></span>

- <span data-ttu-id="03bae-245">Hello [Azure virtuális gépek piactér][vm_marketplace], amely biztosítja, hogy Windows és Linux lemezképeket, amelyek használatra kész.</span><span class="sxs-lookup"><span data-stu-id="03bae-245">hello [Azure Virtual Machines Marketplace][vm_marketplace], which provides both Windows and Linux images that are ready-to-use.</span></span> 
- <span data-ttu-id="03bae-246">Egy Ön által előkészített és biztosított, egyéni rendszerkép.</span><span class="sxs-lookup"><span data-stu-id="03bae-246">A custom image that you prepare and provide.</span></span> <span data-ttu-id="03bae-247">További részletek az egyéni rendszerképekről: [Nagy léptékű párhuzamos számítási megoldások fejlesztése a Batch segítségével](batch-api-basics.md#pool).</span><span class="sxs-lookup"><span data-stu-id="03bae-247">For more details about custom images, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#pool).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="03bae-248">A Batchben lévő számítási erőforrások díjkötelesek.</span><span class="sxs-lookup"><span data-stu-id="03bae-248">You are charged for compute resources in Batch.</span></span> <span data-ttu-id="03bae-249">csökkentheti toominimize költségek `targetDedicatedComputeNodes` too1 hello minta futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="03bae-249">toominimize costs, you can lower `targetDedicatedComputeNodes` too1 before you run hello sample.</span></span>
>
>

<span data-ttu-id="03bae-250">A fizikai csomópont tulajdonságok együtt is megadhat egy [StartTask] [ net_pool_starttask] hello készlet.</span><span class="sxs-lookup"><span data-stu-id="03bae-250">Along with these physical node properties, you may also specify a [StartTask][net_pool_starttask] for hello pool.</span></span> <span data-ttu-id="03bae-251">hello StartTask végrehajtása minden egyes csomóponton, hogy a csomópont csatlakozik hello készletet, és minden alkalommal, amikor újraindítja a csomópontot.</span><span class="sxs-lookup"><span data-stu-id="03bae-251">hello StartTask executes on each node as that node joins hello pool, and each time a node is restarted.</span></span> <span data-ttu-id="03bae-252">hello StartTask esetén különösen hasznos alkalmazásokat telepít a számítási csomópontok előzetes toohello feladatok végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="03bae-252">hello StartTask is especially useful for installing applications on compute nodes prior toohello execution of tasks.</span></span> <span data-ttu-id="03bae-253">Például a feladatok adatok feldolgozása a Python-parancsfájlok használatával, használhatja a StartTask tooinstall Python hello számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="03bae-253">For example, if your tasks process data by using Python scripts, you could use a StartTask tooinstall Python on hello compute nodes.</span></span>

<span data-ttu-id="03bae-254">A mintaalkalmazás hello StartTask hello tárolásból tölt le fájlokat másolja át. (amely hello használatával megadott [StartTask][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] tulajdonság) hello StartTask working directory toohello megosztott könyvtárból, amely *összes* hello csomóponton futó feladatok férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="03bae-254">In this sample application, hello StartTask copies hello files that it downloads from Storage (which are specified by using hello [StartTask][net_starttask].[ResourceFiles][net_starttask_resourcefiles] property) from hello StartTask working directory toohello shared directory that *all* tasks running on hello node can access.</span></span> <span data-ttu-id="03bae-255">Alapvetően, ennek `TaskApplication.exe` és a függőségek toohello, megosztott directory minden egyes csomóponton hello csomópont csatlakozik hello készlet, mivel így hello csomóponton futó minden feladatot-e hozzáférési engedélye.</span><span class="sxs-lookup"><span data-stu-id="03bae-255">Essentially, this copies `TaskApplication.exe` and its dependencies toohello shared directory on each node as hello node joins hello pool, so that any tasks that run on hello node can access it.</span></span>

> [!TIP]
> <span data-ttu-id="03bae-256">Hello **alkalmazáscsomagok** az Azure Batch szolgáltatás más módon tooget biztosít az alkalmazás telepítését a készlet számítási csomópontjai hello.</span><span class="sxs-lookup"><span data-stu-id="03bae-256">hello **application packages** feature of Azure Batch provides another way tooget your application onto hello compute nodes in a pool.</span></span> <span data-ttu-id="03bae-257">Lásd: [központi telepítése az alkalmazások toocompute csomópontokat a kötegelt alkalmazáscsomagok](batch-application-packages.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="03bae-257">See [Deploy applications toocompute nodes with Batch application packages](batch-application-packages.md) for details.</span></span>
>
>

<span data-ttu-id="03bae-258">A fenti hello kódrészletet lényeges hello hello két környezeti változók használatát a rendszer *CommandLine* hello StartTask tulajdonsága: `%AZ_BATCH_TASK_WORKING_DIR%` és `%AZ_BATCH_NODE_SHARED_DIR%`.</span><span class="sxs-lookup"><span data-stu-id="03bae-258">Also notable in hello code snippet above is hello use of two environment variables in hello *CommandLine* property of hello StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` and `%AZ_BATCH_NODE_SHARED_DIR%`.</span></span> <span data-ttu-id="03bae-259">A Batch-készlet belül minden számítási csomópont automatikusan a több környezeti változókat, amelyek adott tooBatch van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="03bae-259">Each compute node within a Batch pool is automatically configured with several environment variables that are specific tooBatch.</span></span> <span data-ttu-id="03bae-260">E feladat által végrehajtott folyamat rendelkezik hozzáférési toothese környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="03bae-260">Any process that is executed by a task has access toothese environment variables.</span></span>

> [!TIP]
> <span data-ttu-id="03bae-261">További információk a számítási csomópontok a Batch-készlet és a feladat működő könyvtárak, információkat elérhető hello környezeti változók toofind lásd: hello [környezeti beállítások feladatok](batch-api-basics.md#environment-settings-for-tasks) és [fájlok és könyvtárak ](batch-api-basics.md#files-and-directories) hello részeiben [Batch funkcióinak áttekintése a fejlesztők](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="03bae-261">toofind out more about hello environment variables that are available on compute nodes in a Batch pool, and information on task working directories, see hello [Environment settings for tasks](batch-api-basics.md#environment-settings-for-tasks) and [Files and directories](batch-api-basics.md#files-and-directories) sections in hello [Batch feature overview for developers](batch-api-basics.md).</span></span>
>
>

## <a name="step-4-create-batch-job"></a><span data-ttu-id="03bae-262">4. lépés: Batch-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="03bae-262">Step 4: Create Batch job</span></span>
<span data-ttu-id="03bae-263">![Batch-feladat létrehozása][4]</span><span class="sxs-lookup"><span data-stu-id="03bae-263">![Create Batch job][4]</span></span><br/>

<span data-ttu-id="03bae-264">A Batch-**feladatok** számítási csomópontok készletéhez társított tevékenységek gyűjteményei.</span><span class="sxs-lookup"><span data-stu-id="03bae-264">A Batch **job** is a collection of tasks, and is associated with a pool of compute nodes.</span></span> <span data-ttu-id="03bae-265">hello feladatot a feladatok végrehajtása tartozó hello készlet számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="03bae-265">hello tasks in a job execute on hello associated pool's compute nodes.</span></span>

<span data-ttu-id="03bae-266">Használhatja a feladat nem csak rendszerezése és nyomon követni a feladatokat a kapcsolódó munkaterheléseknél, hanem is előíró bizonyos megkötéseknek – például a maximális futási hello hello feladat (és -kiterjesztés, a feladatok által) valamint a kapcsolat tooother feladatok hello kötegelt feladat prioritása fiók.</span><span class="sxs-lookup"><span data-stu-id="03bae-266">You can use a job not only for organizing and tracking tasks in related workloads, but also for imposing certain constraints--such as hello maximum runtime for hello job (and by extension, its tasks) as well as job priority in relation tooother jobs in hello Batch account.</span></span> <span data-ttu-id="03bae-267">Ebben a példában azonban hello feladat tartozik csak a #3. lépésben létrehozott alkalmazáskészlet hello.</span><span class="sxs-lookup"><span data-stu-id="03bae-267">In this example, however, hello job is associated only with hello pool that was created in step #3.</span></span> <span data-ttu-id="03bae-268">Nincsenek konfigurálva további tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="03bae-268">No additional properties are configured.</span></span>

<span data-ttu-id="03bae-269">Mindegyik Batch-feladat egy adott készlettel van társítva.</span><span class="sxs-lookup"><span data-stu-id="03bae-269">All Batch jobs are associated with a specific pool.</span></span> <span data-ttu-id="03bae-270">Ez a társítás hello feladat tevékenységeit fogja végrehajtani a csomópontok jelzi.</span><span class="sxs-lookup"><span data-stu-id="03bae-270">This association indicates which nodes hello job's tasks will execute on.</span></span> <span data-ttu-id="03bae-271">Ezek megadása hello segítségével [CloudJob.PoolInformation] [ net_job_poolinfo] tulajdonság, ahogy az alábbi hello kódrészlet.</span><span class="sxs-lookup"><span data-stu-id="03bae-271">You specify this by using hello [CloudJob.PoolInformation][net_job_poolinfo] property, as shown in hello code snippet below.</span></span>

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

<span data-ttu-id="03bae-272">Most, hogy a feladat létrehozása után feladatok tooperform hello munkahelyi lettek hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="03bae-272">Now that a job has been created, tasks are added tooperform hello work.</span></span>

## <a name="step-5-add-tasks-toojob"></a><span data-ttu-id="03bae-273">5. lépés: A feladatok toojob hozzáadása</span><span class="sxs-lookup"><span data-stu-id="03bae-273">Step 5: Add tasks toojob</span></span>
<span data-ttu-id="03bae-274">![Feladatok toojob hozzáadása][5]</span><span class="sxs-lookup"><span data-stu-id="03bae-274">![Add tasks toojob][5]</span></span><br/><span data-ttu-id="03bae-275">
*(1) feladatok toohello feladat kerülnek, (2) hello feladatok ütemezett toorun csomópontján, és (3) hello feladatok hello adatok fájlok tooprocess letöltése*</span><span class="sxs-lookup"><span data-stu-id="03bae-275">
*(1) Tasks are added toohello job, (2) hello tasks are scheduled toorun on nodes, and (3) hello tasks download hello data files tooprocess*</span></span>

<span data-ttu-id="03bae-276">Kötegelt **feladatok** hello egyes Munkaegységek hello hajt végre, a számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="03bae-276">Batch **tasks** are hello individual units of work that execute on hello compute nodes.</span></span> <span data-ttu-id="03bae-277">A feladatot a parancssor és hello parancsfájlok vagy végrehajtható fájlok, amely megadja, hogy a parancssor futtatja.</span><span class="sxs-lookup"><span data-stu-id="03bae-277">A task has a command line and runs hello scripts or executables that you specify in that command line.</span></span>

<span data-ttu-id="03bae-278">tooactually feladatok végrehajtására, a feladatok tooa feladat hozzá kell adni.</span><span class="sxs-lookup"><span data-stu-id="03bae-278">tooactually perform work, tasks must be added tooa job.</span></span> <span data-ttu-id="03bae-279">Minden egyes [CloudTask] [ net_task] parancssori tulajdonság használatával van konfigurálva, és [ResourceFiles] [ net_task_resourcefiles] (például hello készlet StartTask), hello feladat toohello csomópont tölti le, a parancssor automatikusan végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="03bae-279">Each [CloudTask][net_task] is configured by using a command-line property and [ResourceFiles][net_task_resourcefiles] (as with hello pool's StartTask) that hello task downloads toohello node before its command line is automatically executed.</span></span> <span data-ttu-id="03bae-280">A hello *DotNetTutorial* mintaprojektet, minden tevékenység csak egy fájl dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="03bae-280">In hello *DotNetTutorial* sample project, each task processes only one file.</span></span> <span data-ttu-id="03bae-281">A ResourceFiles gyűjtemény így egyetlen elemet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="03bae-281">Thus, its ResourceFiles collection contains a single element.</span></span>

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks toojob [{1}]...", inputFiles.Count, jobId);

    // Create a collection toohold hello tasks that we'll be adding toohello job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of hello tasks. Because we copied hello task application toothe
    // node's shared directory with hello pool's StartTask, we can access it via
    // hello shared directory on hello node that hello task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add hello tasks as a collection, as opposed tooissuing a separate AddTask call
    // for each. Bulk task submission helps tooensure efficient underlying API calls
    // toohello Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [!IMPORTANT]
> <span data-ttu-id="03bae-282">Ha környezeti változók hozzáférésük van például `%AZ_BATCH_NODE_SHARED_DIR%` vagy egy alkalmazás nem található a hello csomópont végrehajtása `PATH`, feladat parancssorokat kell előtagként `cmd /c`.</span><span class="sxs-lookup"><span data-stu-id="03bae-282">When they access environment variables such as `%AZ_BATCH_NODE_SHARED_DIR%` or execute an application not found in hello node's `PATH`, task command lines must be prefixed with `cmd /c`.</span></span> <span data-ttu-id="03bae-283">Ezzel explicit módon hello parancsértelmező hajtható végre, és kérje meg az tooterminate a parancs végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="03bae-283">This will explicitly execute hello command interpreter and instruct it tooterminate after carrying out your command.</span></span> <span data-ttu-id="03bae-284">Ez a követelmény nem szükséges, ha a feladatok végrehajtása egy alkalmazás hello csomópontban `PATH` (például *robocopy.exe* vagy *powershell.exe*), és nem környezeti változókat szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="03bae-284">This requirement is unnecessary if your tasks execute an application in hello node's `PATH` (such as *robocopy.exe* or *powershell.exe*) and no environment variables are used.</span></span>
>
>

<span data-ttu-id="03bae-285">Hello belül `foreach` hurok a fenti hello kódrészletet, láthatja, hogy úgy, hogy három parancssori argumentumok túl átadott összeállított hello feladat parancssorának hello*TaskApplication.exe*:</span><span class="sxs-lookup"><span data-stu-id="03bae-285">Within hello `foreach` loop in hello code snippet above, you can see that hello command line for hello task is constructed such that three command-line arguments are passed too*TaskApplication.exe*:</span></span>

1. <span data-ttu-id="03bae-286">Hello **első argumentum** hello fájl tooprocess hello elérési útja.</span><span class="sxs-lookup"><span data-stu-id="03bae-286">hello **first argument** is hello path of hello file tooprocess.</span></span> <span data-ttu-id="03bae-287">Ez az hello helyi elérési út toohello fájlt, mert létezik hello csomóponton.</span><span class="sxs-lookup"><span data-stu-id="03bae-287">This is hello local path toohello file as it exists on hello node.</span></span> <span data-ttu-id="03bae-288">Ha hello ResourceFile objektum `UploadFileToContainerAsync` létrehozásának újabb hello fájlnév volt megadva ehhez a tulajdonsághoz (paraméter toohello ResourceFile konstruktorban).</span><span class="sxs-lookup"><span data-stu-id="03bae-288">When hello ResourceFile object in `UploadFileToContainerAsync` was first created above, hello file name was used for this property (as a parameter toohello ResourceFile constructor).</span></span> <span data-ttu-id="03bae-289">Ez azt jelzi, hogy hello fájl található hello azonos könyvtárhoz, mint a *TaskApplication.exe*.</span><span class="sxs-lookup"><span data-stu-id="03bae-289">This indicates that hello file can be found in hello same directory as *TaskApplication.exe*.</span></span>
2. <span data-ttu-id="03bae-290">Hello **második argumentum** határozza meg, hogy hello felső *N* szavak toohello kimeneti fájl kell írni.</span><span class="sxs-lookup"><span data-stu-id="03bae-290">hello **second argument** specifies that hello top *N* words should be written toohello output file.</span></span> <span data-ttu-id="03bae-291">Hello mintában ez van kódolva, hogy hello felső három szavak írt toohello kimeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="03bae-291">In hello sample, this is hard-coded so that hello top three words are written toohello output file.</span></span>
3. <span data-ttu-id="03bae-292">Hello **harmadik argumentum** hello közös hozzáférésű jogosultságkód (SAS), amely írási toohello van **kimeneti** az Azure Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="03bae-292">hello **third argument** is hello shared access signature (SAS) that provides write access toohello **output** container in Azure Storage.</span></span> <span data-ttu-id="03bae-293">*TaskApplication.exe* használja ezt a megosztott aláírás URL-cím érhető el, amikor feltölti a hello kimeneti fájl tooAzure tároló.</span><span class="sxs-lookup"><span data-stu-id="03bae-293">*TaskApplication.exe* uses this shared access signature URL when it uploads hello output file tooAzure Storage.</span></span> <span data-ttu-id="03bae-294">Hello kód ehhez az található hello `UploadFileToContainer` hello TaskApplication projektben metódus `Program.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="03bae-294">You can find hello code for this in hello `UploadFileToContainer` method in hello TaskApplication project's `Program.cs` file:</span></span>

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference toohello container using hello SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload hello file (as a new blob) toohello container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when hello Batch service
                // sets hello CloudTask.ExecutionInformation.ExitCode for hello task that
                // executed this application, it properly indicates that there was a
                // problem with hello task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a><span data-ttu-id="03bae-295">6. lépés: Tevékenységek figyelése</span><span class="sxs-lookup"><span data-stu-id="03bae-295">Step 6: Monitor tasks</span></span>
<span data-ttu-id="03bae-296">![Tevékenységek figyelése][6]</span><span class="sxs-lookup"><span data-stu-id="03bae-296">![Monitor tasks][6]</span></span><br/><span data-ttu-id="03bae-297">
*hello ügyfél alkalmazás (1) figyelők hello-feladatok befejezési és sikeres állapotát, és (2) hello feladatok feltöltés eredmény adatok tooAzure tároló*</span><span class="sxs-lookup"><span data-stu-id="03bae-297">
*hello client application (1) monitors hello tasks for completion and success status, and (2) hello tasks upload result data tooAzure Storage*</span></span>

<span data-ttu-id="03bae-298">Feladatok tooa feladatot ad hozzá, amikor ezeket automatikusan aszinkron és számítási csomóponton belül hello hello feladattal társított végrehajtásra ütemezni.</span><span class="sxs-lookup"><span data-stu-id="03bae-298">When tasks are added tooa job, they are automatically queued and scheduled for execution on compute nodes within hello pool associated with hello job.</span></span> <span data-ttu-id="03bae-299">Hello megadott beállítások alapján, kötegelt kezeli az összes feladat queuing, ütemezés, újrapróbálkozás és más tevékenység felügyeleti feladatokat meg.</span><span class="sxs-lookup"><span data-stu-id="03bae-299">Based on hello settings you specify, Batch handles all task queuing, scheduling, retrying, and other task administration duties for you.</span></span>

<span data-ttu-id="03bae-300">Nincsenek számos különböző módszer toomonitoring feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="03bae-300">There are many approaches toomonitoring task execution.</span></span> <span data-ttu-id="03bae-301">A DotNetTutorial egy egyszerű példát mutat be, amely csak a tevékenységek befejezéséről, illetve a meghiúsult vagy a sikeres állapotról küld jelentést.</span><span class="sxs-lookup"><span data-stu-id="03bae-301">DotNetTutorial shows a simple example that reports only on completion and task failure or success states.</span></span> <span data-ttu-id="03bae-302">Hello belül `MonitorTasks` DotNetTutorial tartozó metódus `Program.cs`, vitafórum szavatolja három Batch .NET fogalmat van.</span><span class="sxs-lookup"><span data-stu-id="03bae-302">Within hello `MonitorTasks` method in DotNetTutorial's `Program.cs`, there are three Batch .NET concepts that warrant discussion.</span></span> <span data-ttu-id="03bae-303">Az alábbiakban láthatja ezeket a megjelenésük sorrendjében:</span><span class="sxs-lookup"><span data-stu-id="03bae-303">They are listed below in their order of appearance:</span></span>

1. <span data-ttu-id="03bae-304">**ODATADetailLevel**: Az [ODATADetailLevel][net_odatadetaillevel] meghatározása a listaműveletekben (amilyen például a feladatok tevékenységlistáinak beszerzése) elengedhetetlen a Batch-alkalmazások megfelelő teljesítményének biztosítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="03bae-304">**ODATADetailLevel**: Specifying [ODATADetailLevel][net_odatadetaillevel] in list operations (such as obtaining a list of a job's tasks) is essential in ensuring Batch application performance.</span></span> <span data-ttu-id="03bae-305">Adja hozzá [hello Azure Batch szolgáltatás hatékony lekérdezéséhez](batch-efficient-list-queries.md) tooyour olvasási lista, ha tervezi ennek során tetszőleges belül a Batch-alkalmazások figyelése.</span><span class="sxs-lookup"><span data-stu-id="03bae-305">Add [Query hello Azure Batch service efficiently](batch-efficient-list-queries.md) tooyour reading list if you plan on doing any sort of status monitoring within your Batch applications.</span></span>
2. <span data-ttu-id="03bae-306">**TaskStateMonitor**: A [TaskStateMonitor][net_taskstatemonitor] segédprogramokat biztosít a Batch .NET-alkalmazásoknak a feladatállapotok megfigyeléséhez.</span><span class="sxs-lookup"><span data-stu-id="03bae-306">**TaskStateMonitor**: [TaskStateMonitor][net_taskstatemonitor] provides Batch .NET applications with helper utilities for monitoring task states.</span></span> <span data-ttu-id="03bae-307">A `MonitorTasks`, *DotNetTutorial* megvárja-e a minden feladat tooreach [TaskState.Completed] [ net_taskstate] a határidőn belül.</span><span class="sxs-lookup"><span data-stu-id="03bae-307">In `MonitorTasks`, *DotNetTutorial* waits for all tasks tooreach [TaskState.Completed][net_taskstate] within a time limit.</span></span> <span data-ttu-id="03bae-308">Ezután a rendszer megszakítja hello feladat.</span><span class="sxs-lookup"><span data-stu-id="03bae-308">Then it terminates hello job.</span></span>
3. <span data-ttu-id="03bae-309">**TerminateJobAsync**: egy feladatot leállítja [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (vagy hello blokkolja JobOperations.TerminateJob) befejezettként jelöli meg a feladatot.</span><span class="sxs-lookup"><span data-stu-id="03bae-309">**TerminateJobAsync**: Terminating a job with [JobOperations.TerminateJobAsync][net_joboperations_terminatejob] (or hello blocking JobOperations.TerminateJob) marks that job as completed.</span></span> <span data-ttu-id="03bae-310">Alapvető toodo, ha a köteg megoldást használ egy [JobReleaseTask][net_jobreltask].</span><span class="sxs-lookup"><span data-stu-id="03bae-310">It is essential toodo so if your Batch solution uses a [JobReleaseTask][net_jobreltask].</span></span> <span data-ttu-id="03bae-311">Ez egy speciális típusú feladat, amelyről a [feladatok előkészítési és befejezési tevékenységeit](batch-job-prep-release.md) ismertető szakaszban olvashat.</span><span class="sxs-lookup"><span data-stu-id="03bae-311">This is a special type of task, which is described in [Job preparation and completion tasks](batch-job-prep-release.md).</span></span>

<span data-ttu-id="03bae-312">Hello `MonitorTasks` módszerrel *DotNetTutorial*tartozó `Program.cs` alatt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="03bae-312">hello `MonitorTasks` method from *DotNetTutorial*'s `Program.cs` appears below:</span></span>

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed tooreach hello Completed state within hello timeout period.";

    // Obtain hello collection of tasks currently managed by hello job. Note that we use
    // a detail level too specify that only hello "id" property of each task should be
    // populated. Using a detail level for all list operations helps toolower
    // response time from hello Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor toomonitor hello state of our tasks. In this case, we
    // will wait for all tasks tooreach hello Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached hello "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property tooensure that it did not encounter a failure
    // or return a non-zero exit code.

    // Update hello detail level toopopulate only hello task id and executionInfo
    // properties. We refresh hello tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate hello task's properties with hello latest info from hello Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.Result == TaskExecutionResult.Failure)
        {
            // A task with failure information set indicates there was a problem with hello task. It is important toonote that
            // hello task's state can be "Completed," yet still have encountered a failure.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a failure: {1}", task.Id, task.ExecutionInformation.FailureInformation.Message);
            if (task.ExecutionInformation.ExitCode != 0)
            {
                // A non-zero exit code may indicate that hello application executed by hello task encountered an error
                // during execution. As not every application returns non-zero on failure by default (e.g. robocopy),
                // your implementation of error checking may differ from this example.

                Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
            }
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within hello specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a><span data-ttu-id="03bae-313">7. lépés: Feladat kimenetének letöltése</span><span class="sxs-lookup"><span data-stu-id="03bae-313">Step 7: Download task output</span></span>
<span data-ttu-id="03bae-314">![Tevékenység kimenetének letöltése a Storage-ból][7]</span><span class="sxs-lookup"><span data-stu-id="03bae-314">![Download task output from Storage][7]</span></span><br/>

<span data-ttu-id="03bae-315">Most, hogy hello feladat befejezését követően hello feladatok kimenete hello Azure Storage-ból letölthető.</span><span class="sxs-lookup"><span data-stu-id="03bae-315">Now that hello job is completed, hello output from hello tasks can be downloaded from Azure Storage.</span></span> <span data-ttu-id="03bae-316">Ez a lépés hívása túl`DownloadBlobsFromContainerAsync` a *DotNetTutorial*tartozó `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="03bae-316">This is done with a call too`DownloadBlobsFromContainerAsync` in *DotNetTutorial*'s `Program.cs`:</span></span>

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference tooa previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all hello block blobs in hello specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference toohello current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents tooa file in hello specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded too{0}", directoryPath);
}
```

> [!NOTE]
> <span data-ttu-id="03bae-317">hívás túl hello`DownloadBlobsFromContainerAsync` a hello *DotNetTutorial* alkalmazás megadhatja, hogy hello fájlok letöltött tooyour `%TEMP%` mappa.</span><span class="sxs-lookup"><span data-stu-id="03bae-317">hello call too`DownloadBlobsFromContainerAsync` in hello *DotNetTutorial* application specifies that hello files should be downloaded tooyour `%TEMP%` folder.</span></span> <span data-ttu-id="03bae-318">Szabad toomodify érzi, hogy a kimeneti helyet.</span><span class="sxs-lookup"><span data-stu-id="03bae-318">Feel free toomodify this output location.</span></span>
>
>

## <a name="step-8-delete-containers"></a><span data-ttu-id="03bae-319">8. lépés: Tárolók törlése</span><span class="sxs-lookup"><span data-stu-id="03bae-319">Step 8: Delete containers</span></span>
<span data-ttu-id="03bae-320">Mivel van szó, az Azure-tárolóban lévő adatok, még mindig egy jó ötlet tooremove blobokat, már nem szükséges a kötegelt feladatok.</span><span class="sxs-lookup"><span data-stu-id="03bae-320">Because you are charged for data that resides in Azure Storage, it's always a good idea tooremove blobs that are no longer needed for your Batch jobs.</span></span> <span data-ttu-id="03bae-321">A DotNetTutorial tartozó `Program.cs`, ez a lépés három hívások toohello segédmetódus `DeleteContainerAsync`:</span><span class="sxs-lookup"><span data-stu-id="03bae-321">In DotNetTutorial's `Program.cs`, this is done with three calls toohello helper method `DeleteContainerAsync`:</span></span>

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

<span data-ttu-id="03bae-322">hello metódusból csupán jut hozzá a hivatkozás toohello tárolója, és ekkor meghívja a [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:</span><span class="sxs-lookup"><span data-stu-id="03bae-322">hello method itself merely obtains a reference toohello container, and then calls [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:</span></span>

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-hello-job-and-hello-pool"></a><span data-ttu-id="03bae-323">9. lépés: Hello feladat és hello készlet törlése</span><span class="sxs-lookup"><span data-stu-id="03bae-323">Step 9: Delete hello job and hello pool</span></span>
<span data-ttu-id="03bae-324">Hello utolsó lépést, az Ön felszólító toodelete hello feladat és hello készlet hello DotNetTutorial alkalmazás által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="03bae-324">In hello final step, you're prompted toodelete hello job and hello pool that were created by hello DotNetTutorial application.</span></span> <span data-ttu-id="03bae-325">Bár magukért a munkákért és feladatokért nem kell fizetnie, a számítási csomópontokért *igen*.</span><span class="sxs-lookup"><span data-stu-id="03bae-325">Although you're not charged for jobs and tasks themselves, you *are* charged for compute nodes.</span></span> <span data-ttu-id="03bae-326">Ezért ajánlott csak szükség szerint lefoglalni a csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="03bae-326">Thus, we recommend that you allocate nodes only as needed.</span></span> <span data-ttu-id="03bae-327">A nem használt készletek törlése a karbantartási folyamat része lehet.</span><span class="sxs-lookup"><span data-stu-id="03bae-327">Deleting unused pools can be part of your maintenance process.</span></span>

<span data-ttu-id="03bae-328">hello BatchClient [JobOperations] [ net_joboperations] és [PoolOperations] [ net_pooloperations] egyaránt rendelkezik a megfelelő törlési metódussal, ha néven hello felhasználó megerősíti, hogy törlése:</span><span class="sxs-lookup"><span data-stu-id="03bae-328">hello BatchClient's [JobOperations][net_joboperations] and [PoolOperations][net_pooloperations] both have corresponding deletion methods, which are called if hello user confirms deletion:</span></span>

```csharp
// Clean up hello resources we've created in hello Batch account if hello user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [!IMPORTANT]
> <span data-ttu-id="03bae-329">Ne feledje, hogy a számítási erőforrások díjkötelesek – a nem használt készletek törlése minimalizálja a költségeket.</span><span class="sxs-lookup"><span data-stu-id="03bae-329">Keep in mind that you are charged for compute resources—deleting unused pools will minimize cost.</span></span> <span data-ttu-id="03bae-330">Emellett vegye figyelembe, hogy törli a készlet törlése belül, hogy minden számítási csomópontokat, és, hogy bármely hello csomópontok adatainak lesz helyreállíthatatlan hello készlet törlése után.</span><span class="sxs-lookup"><span data-stu-id="03bae-330">Also, be aware that deleting a pool deletes all compute nodes within that pool, and that any data on hello nodes will be unrecoverable after hello pool is deleted.</span></span>
>
>

## <a name="run-hello-dotnettutorial-sample"></a><span data-ttu-id="03bae-331">Futtassa a hello *DotNetTutorial* minta</span><span class="sxs-lookup"><span data-stu-id="03bae-331">Run hello *DotNetTutorial* sample</span></span>
<span data-ttu-id="03bae-332">Hello mintaalkalmazás futtatásakor hello a konzol kimeneti lesz hasonló toohello következő.</span><span class="sxs-lookup"><span data-stu-id="03bae-332">When you run hello sample application, hello console output will be similar toohello following.</span></span> <span data-ttu-id="03bae-333">Végrehajtásakor, a szünetelést fog tapasztalni `Awaiting task completion, timeout in 00:30:00...` közben hello készlet számítási csomópontok indulnak el.</span><span class="sxs-lookup"><span data-stu-id="03bae-333">During execution, you will experience a pause at `Awaiting task completion, timeout in 00:30:00...` while hello pool's compute nodes are started.</span></span> <span data-ttu-id="03bae-334">Használjon hello [Azure-portálon] [ azure_portal] toomonitor a készlet, a számítási csomópontok, a feladat és a feladatok során, és végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="03bae-334">Use hello [Azure portal][azure_portal] toomonitor your pool, compute nodes, job, and tasks during and after execution.</span></span> <span data-ttu-id="03bae-335">Használjon hello [Azure-portálon] [ azure_portal] vagy hello [Azure Tártallózó] [ storage_explorers] tooview hello tárolási erőforrások (tárolók és blobok), amelyek hello alkalmazás által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="03bae-335">Use hello [Azure portal][azure_portal] or hello [Azure Storage Explorer][storage_explorers] tooview hello Storage resources (containers and blobs) that are created by hello application.</span></span>

<span data-ttu-id="03bae-336">Tipikus végrehajtási idő **körülbelül 5 percig** futtatásakor hello alkalmazás alapértelmezett konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="03bae-336">Typical execution time is **approximately 5 minutes** when you run hello application in its default configuration.</span></span>

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe toocontainer [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll toocontainer [application]...
Uploading file ..\..\taskdata1.txt toocontainer [input]...
Uploading file ..\..\taskdata2.txt toocontainer [input]...
Uploading file ..\..\taskdata3.txt toocontainer [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks toojob [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within hello specified timeout period.
Downloading all files from container [output]...
All files downloaded tooC:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER tooexit...
```

## <a name="next-steps"></a><span data-ttu-id="03bae-337">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="03bae-337">Next steps</span></span>
<span data-ttu-id="03bae-338">Szabad toomake módosítások érzi, hogy túl*DotNetTutorial* és *TaskApplication* tooexperiment másik számítási forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="03bae-338">Feel free toomake changes too*DotNetTutorial* and *TaskApplication* tooexperiment with different compute scenarios.</span></span> <span data-ttu-id="03bae-339">Például, próbálja meg a túl hozzáadni egy végrehajtási késedelem*TaskApplication*, például a [Thread.Sleep][net_thread_sleep], toosimulate hosszan futó feladatokat, és figyelheti azokat hello portálon.</span><span class="sxs-lookup"><span data-stu-id="03bae-339">For example, try adding an execution delay too*TaskApplication*, such as with [Thread.Sleep][net_thread_sleep], toosimulate long-running tasks and monitor them in hello portal.</span></span> <span data-ttu-id="03bae-340">Próbálja meg további feladatok hozzáadása vagy módosítása a számítási csomópontok száma hello.</span><span class="sxs-lookup"><span data-stu-id="03bae-340">Try adding more tasks or adjusting hello number of compute nodes.</span></span> <span data-ttu-id="03bae-341">Adja hozzá a logikai toocheck és egy meglévő készlet toospeed végrehajtási idő hello használatának engedélyezése (*mutató*: látogasson el `ArticleHelpers.cs` a hello [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] a projekt [azure-köteg-minták][github_samples]).</span><span class="sxs-lookup"><span data-stu-id="03bae-341">Add logic toocheck for and allow hello use of an existing pool toospeed execution time (*hint*: check out `ArticleHelpers.cs` in hello [Microsoft.Azure.Batch.Samples.Common][github_samples_common] project in [azure-batch-samples][github_samples]).</span></span>

<span data-ttu-id="03bae-342">Most, hogy ismeri a kötegelt megoldás hello alapvető munkafolyamattal, idő toodig a Batch szolgáltatás hello toohello további szolgáltatásait is.</span><span class="sxs-lookup"><span data-stu-id="03bae-342">Now that you're familiar with hello basic workflow of a Batch solution, it's time toodig in toohello additional features of hello Batch service.</span></span>

* <span data-ttu-id="03bae-343">Felülvizsgálati hello [áttekintés az Azure Batch funkcióinak](batch-api-basics.md) cikk, amely ajánlott, ha még új toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="03bae-343">Review hello [Overview of Azure Batch features](batch-api-basics.md) article, which we recommend if you're new toohello service.</span></span>
* <span data-ttu-id="03bae-344">A Start hello egyéb kötegelt fejlesztési cikkek alapján **részletes fejlesztési** a hello [kötegelt képzési terv][batch_learning_path].</span><span class="sxs-lookup"><span data-stu-id="03bae-344">Start on hello other Batch development articles under **Development in-depth** in hello [Batch learning path][batch_learning_path].</span></span>
* <span data-ttu-id="03bae-345">Tekintse meg a "legfontosabb N szavak" hello munkaterhelés feldolgozása kötegelt hello segítségével különböző végrehajtásának [TopNWords] [ github_topnwords] minta.</span><span class="sxs-lookup"><span data-stu-id="03bae-345">Check out a different implementation of processing hello "top N words" workload by using Batch in hello [TopNWords][github_topnwords] sample.</span></span>
* <span data-ttu-id="03bae-346">Felülvizsgálati hello Batch .NET [kibocsátási megjegyzéseket](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/Batch/DataPlane/changelog.md#azurebatch-release-notes) a legfrissebb változásokat hello hello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="03bae-346">Review hello Batch .NET [release notes](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/Batch/DataPlane/changelog.md#azurebatch-release-notes) for hello latest changes in hello library.</span></span>

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/vs/
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Tárolók létrehozása az Azure Storage-ban"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Feltöltési feladat alkalmazás- és bemeneti (adatok) fájlok toocontainers"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Batch-készlet létrehozása"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Batch-feladat létrehozása"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Feladatok toojob hozzáadása"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Tevékenységek figyelése"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Tevékenység kimenetének letöltése a Storage-ból"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Batch-megoldás munkafolyamata (teljes diagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "A Batch hitelesítő adatai a portálon"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "A Storage hitelesítő adatai a portálon"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Batch-megoldás munkafolyamata (minimális diagram)"
